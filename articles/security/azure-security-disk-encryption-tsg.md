---
title: Resolução de problemas de encriptação de disco do Azure | Documentos da Microsoft
description: Este artigo fornece sugestões de resolução de problemas para o Microsoft Azure disco encriptação para Windows e VMs de IaaS Linux.
author: mestew
ms.service: security
ms.subservice: Azure Disk Encryption
ms.topic: article
ms.author: mstewart
ms.date: 09/10/2018
ms.openlocfilehash: 3d52e031d6c3266ba9d15a2283adcdbce7a6b929
ms.sourcegitcommit: af9cb4c4d9aaa1fbe4901af4fc3e49ef2c4e8d5e
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/11/2018
ms.locfileid: "44347686"
---
# <a name="azure-disk-encryption-troubleshooting-guide"></a>Guia de resolução de problemas de encriptação de disco do Azure

Este guia destina-se a profissionais de TI, analistas de segurança de informações e os administradores de nuvem cujas organizações utilizam o Azure Disk Encryption. Este artigo é ajudar na resolução de problemas relacionados com encriptação de disco.

## <a name="troubleshooting-linux-os-disk-encryption"></a>Resolução de problemas de encriptação de disco do SO Linux

Encriptação de disco de sistema operativo (SO) do Linux tem desmontar a unidade de sistema operacional antes de executá-lo no processo de encriptação de disco completa. Se ele não é possível desmontar a unidade, uma mensagem de erro de "Falha ao desmontar após..." é provável que ocorram.

Este erro pode ocorrer quando a encriptação de disco do SO é tentasse num ambiente de VM de destino que foi alterado da imagem da Galeria das ações suportadas. Desvios em relação a imagem suportados podem interferir com a capacidade da extensão de desmontar a unidade do SO. Exemplos de desvios podem incluir os seguintes itens:
- Imagens personalizadas já não correspondem um sistema de ficheiros suportado ou o esquema de particionamento.
- Aplicativos grandes, como SAP, MongoDB, Apache Cassandra e a Docker não são suportados quando eles são instalados e em execução no sistema operacional antes de encriptação. O Azure Disk Encryption não consegue encerrar esses processos com segurança conforme necessário durante a preparação da unidade do SO para a encriptação de disco. Se existirem processos ainda ativos, que contém identificadores de abrir o ficheiro para a unidade de sistema operacional, a unidade do SO não pode estar desmontada, resultando numa falha ao encriptar a unidade do sistema operacional. 
- Custom scripts que são executados em proximidade fechar tempo para a encriptação ativada ou se existirem outras alterações estão a ser efetuadas na VM durante o processo de criptografia. Esse conflito pode acontecer quando um modelo Azure Resource Manager define várias extensões para executar em simultâneo, ou quando uma extensão de script personalizado ou outra ação é executada em simultâneo para encriptação de disco. Serializar e isolar esses passos poderão resolver o problema.
- Segurança avançada Linux (SELinux) não tiver sido desabilitada antes de ativar a encriptação, para que o passo de desmontar falhar. Pode ser reenabled SELinux após a conclusão da criptografia.
- O disco do SO utiliza um esquema de Gestor de volumes lógicos (LVM). Embora o suporte de disco de dados LVM limitado estiver disponível, não é um disco de SO de LVM.
- Requisitos mínimos de memória não forem cumpridos (7 GB é sugerida para a encriptação de disco do SO).
- Unidades de dados são recursivamente montada no diretório /mnt/ ou de outro (por exemplo, /mnt/data1, /mnt/data2, /data3 + /data3/data4).
- Outros Azure Disk Encryption [pré-requisitos](azure-security-disk-encryption-prerequisites.md) para Linux não forem cumpridas.

## <a name="unable-to-encrypt"></a>Não é possível encriptar

Em alguns casos, a encriptação de disco parece estar bloqueada em "Iniciada de encriptação de disco de SO" de Linux e SSH está desativada. O processo de encriptação pode demorar entre horas de 3-16 a terminar numa imagem da Galeria das ações. Se forem adicionados discos de dados de tamanho de terabytes de transmissões, o processo pode demorar dias.

A sequência de encriptação de disco do SO Linux desmonta temporariamente a unidade do SO. Em seguida, executa encriptação de bloco a bloco do disco do SO inteiro, antes de ele remounts-lo no seu estado encriptado. Ao contrário do Azure Disk Encryption no Windows, a encriptação de disco do Linux não permite para utilização em simultâneo da VM enquanto a encriptação está em curso. As características de desempenho da VM podem fazer uma diferença significativa no tempo necessário para a encriptação completa. Essas características incluem o tamanho do disco e se a conta de armazenamento é standard ou o armazenamento premium (SSD).

Para verificar o estado de encriptação, consultar o **ProgressMessage** campo devolvido do [Get-AzureRmVmDiskEncryptionStatus](/powershell/module/azurerm.compute/get-azurermvmdiskencryptionstatus) comando. Enquanto a unidade do SO está a ser encriptada, a VM entra num Estado de manutenção e desativa o SSH para evitar qualquer interrupção para o processo contínuo. O **EncryptionInProgress** relatórios para a maioria das vezes de mensagens, enquanto a encriptação está em curso. Várias horas depois, um **VMRestartPending** mensagem solicita que reinicie a VM. Por exemplo:


```
PS > Get-AzureRmVMDiskEncryptionStatus -ResourceGroupName $resourceGroupName -VMName $vmName
OsVolumeEncrypted          : EncryptionInProgress
DataVolumesEncrypted       : EncryptionInProgress
OsVolumeEncryptionSettings : Microsoft.Azure.Management.Compute.Models.DiskEncryptionSettings
ProgressMessage            : OS disk encryption started

PS > Get-AzureRmVMDiskEncryptionStatus -ResourceGroupName $resourceGroupName -VMName $vmName
OsVolumeEncrypted          : VMRestartPending
DataVolumesEncrypted       : Encrypted
OsVolumeEncryptionSettings : Microsoft.Azure.Management.Compute.Models.DiskEncryptionSettings
ProgressMessage            : OS disk successfully encrypted, please reboot the VM
```

Depois de lhe for pedido para reiniciar a VM e, depois de reinicia a VM, tem de aguardar 2 a 3 minutos para o reinício e para os últimos passos a serem executadas no destino. As alterações de mensagem de estado quando a encriptação é finalmente concluída. Depois desta mensagem estiver disponível, a unidade do SO encriptada deve estar pronta a utilizar e a VM está pronta para ser utilizada novamente.

Nos seguintes casos, recomendamos que restaure a VM para o instantâneo ou a cópia de segurança criada imediatamente antes de encriptação:
   - Se a sequência de reinicialização, descrita anteriormente, não acontece.
   - Se as informações de arranque, mensagem de progresso ou outros indicadores de erro se a encriptação de SO de relatório não no meio esse processo. Um exemplo de uma mensagem é o erro "Falha ao desmontar" descrita neste guia.

Antes da próxima tentativa, reavaliar as características da VM e certifique-se de que todos os pré-requisitos são cumpridos.

## <a name="troubleshooting-azure-disk-encryption-behind-a-firewall"></a>Resolução de problemas do Azure Disk Encryption atrás de uma firewall
Quando a conectividade é restringida por uma firewall, o requisito de proxy ou definições de grupo (NSG) de segurança de rede, a capacidade da extensão para efetuar tarefas necessárias pode ser interrompida. Este interrupção pode resultar em mensagens de estado, tais como o "Estado da extensão não está disponível na VM." Em cenários esperados, a encriptação não for concluída. As secções que se seguem têm alguns problemas comuns de firewall que pode investigar.

### <a name="network-security-groups"></a>Grupos de segurança de rede
Quaisquer definições de grupo de segurança de rede que são aplicadas ainda permite que o ponto final de acordo com a configuração de rede documentado [pré-requisitos](azure-security-disk-encryption-prerequisites.md#bkmk_GPO) para a encriptação de disco.

### <a name="azure-key-vault-behind-a-firewall"></a>O Azure Key Vault protegido por uma firewall
A VM tem de ser capaz de aceder um cofre de chaves. Consulte a documentação de orientação sobre o acesso ao Cofre de chaves através de uma firewall que o [do Azure Key Vault](../key-vault/key-vault-access-behind-firewall.md) equipe mantém. 

### <a name="linux-package-management-behind-a-firewall"></a>Gestão de pacotes do Linux protegida por uma firewall

No tempo de execução, o Azure Disk Encryption para Linux baseia-se no sistema de gestão do pacote de distribuição de destino para instalar os componentes de pré-requisitos necessários antes de ativar a encriptação. Se as definições da firewall impedem que a VM ser capaz de transferir e instalar estes componentes, em seguida, falhas subsequentes esperadas. Os passos para configurar este sistema de gestão de pacotes podem variar de distribuição. Em Red Hat, quando um proxy for necessário, deve certificar-se de que o Gestor de subscrições e yum estão configuradas corretamente. Para obter mais informações, consulte [como solucionar problemas do Gestor de subscrições e yum](https://access.redhat.com/solutions/189533).  


## <a name="troubleshooting-windows-server-2016-server-core"></a>Resolução de problemas do Server Core do Windows Server 2016

No Windows Server 2016 Server Core, o componente de bdehdcfg não está disponível por predefinição. Este componente é exigido pelo Azure Disk Encryption. É utilizado para dividir o volume do sistema de volume do sistema operacional, que é feito apenas uma vez para o tempo de vida da VM. Esses binários não são necessários durante as operações de encriptação posteriores.

Para contornar este problema, copie os seguintes ficheiros de quatro a partir de uma VM de centro de dados do Windows Server 2016 para o mesmo local no Server Core:

   ```
   \windows\system32\bdehdcfg.exe
   \windows\system32\bdehdcfglib.dll
   \windows\system32\en-US\bdehdcfglib.dll.mui
   \windows\system32\en-US\bdehdcfg.exe.mui
   ```

   2. Introduza o seguinte comando:

   ```
   bdehdcfg.exe -target default
   ```

   3. Este comando cria uma partição do sistema de 550 MB. Reinicialize o sistema.

   4. Use o DiskPart para verificar os volumes e, em seguida, avançar.  

Por exemplo:

```
DISKPART> list vol

  Volume ###  Ltr  Label        Fs     Type        Size     Status     Info
  ----------  ---  -----------  -----  ----------  -------  ---------  --------
  Volume 0     C                NTFS   Partition    126 GB  Healthy    Boot
  Volume 1                      NTFS   Partition    550 MB  Healthy    System
  Volume 2     D   Temporary S  NTFS   Partition     13 GB  Healthy    Pagefile
```
<!-- ## Troubleshooting encryption status

If the expected encryption state does not match what is being reported in the portal, see the following support article:
[Encryption status is displayed incorrectly on the Azure Management Portal](https://support.microsoft.com/en-us/help/4058377/encryption-status-is-displayed-incorrectly-on-the-azure-management-por) --> 

## <a name="next-steps"></a>Passos Seguintes

Neste documento, ficou a saber mais sobre alguns problemas comuns no Azure Disk Encryption e como solucionar esses problemas. Para obter mais informações sobre este serviço e as respetivas capacidades, consulte os artigos seguintes:

- [Aplicar encriptação de disco no Centro de segurança do Azure](../security-center/security-center-apply-disk-encryption.md)
- [Encriptação de dados do Azure em repouso](azure-security-encryption-atrest.md)
