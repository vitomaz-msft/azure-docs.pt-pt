---
title: O Azure Site Recovery de resolução de problemas para erros e problemas de replicação do Azure para o Azure | Documentos da Microsoft
description: Erros e problemas de resolução de problemas ao replicar máquinas virtuais do Azure para recuperação após desastre
services: site-recovery
author: sujayt
manager: rochakm
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.date: 08/09/2018
ms.author: sujayt
ms.openlocfilehash: 040ace1eab4062c011ed82a59e7f5bfb789c256b
ms.sourcegitcommit: 9e179a577533ab3b2c0c7a4899ae13a7a0d5252b
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/23/2018
ms.locfileid: "49945744"
---
# <a name="troubleshoot-azure-to-azure-vm-replication-issues"></a>Resolver problemas de replicação de VMS do Azure para o Azure

Este artigo descreve os problemas comuns no Azure Site Recovery quando replicar e recuperar máquinas virtuais do Azure a partir de uma região para outra região e explica como solucionar esses problemas. Para obter mais informações sobre configurações suportadas, consulte a [matriz de suporte para replicar VMs do Azure](site-recovery-support-matrix-azure-to-azure.md).

## <a name="azure-resource-quota-issues-error-code-150097"></a>Problemas de quota de recursos do Azure (código de erro 150097)
Sua assinatura deve estar ativada para criar VMs do Azure na região de destino que pretende utilizar como a sua região de recuperação após desastre. Além disso, a sua subscrição deve tem quota suficiente ativada para criar VMs de tamanho específico. Por predefinição, o Site Recovery escolhe o mesmo tamanho da VM de destino da VM de origem. Se o tamanho correspondente não estiver disponível, o tamanho mais próximo possível será escolhido automaticamente. Se não houver nenhum tamanho correspondente que suporte a configuração de VM de origem, é apresentada esta mensagem de erro:

**Código de erro** | **Causas possíveis** | **Recomendação**
--- | --- | ---
150097<br></br>**Mensagem**: não foi possível ativar a replicação para a máquina virtual VmName. | -O seu ID de subscrição poderá não estar ativada para criar todas as VMs na localização de região de destino.</br></br>-O seu ID de subscrição não pode ser ativada ou não tem quota suficiente para criar os tamanhos VM específicos na localização de região de destino.</br></br>-Um tamanho VM que corresponde à origem de contagem de NIC de VM (2) de destino adequado não for encontrado para o ID de subscrição na localização de região de destino.| Contacte [suporte de faturação do Azure](https://docs.microsoft.com/azure/azure-supportability/resource-manager-core-quotas-request) para permitir a criação de VM para os tamanhos VM necessários na localização de destino para a sua subscrição. Depois de ser ativada, repita a operação falhada.

### <a name="fix-the-problem"></a>Corrigir o problema
Pode contactar [suporte de faturação do Azure](https://docs.microsoft.com/azure/azure-supportability/resource-manager-core-quotas-request) para ativar a sua subscrição criar VMs de tamanhos necessários na localização de destino.

Se a localização de destino tem uma restrição de capacidade, desative a replicação e ative-o para uma localização diferente em que a sua subscrição tem quota suficiente para criar VMs dos tamanhos necessários.

## <a name="trusted-root-certificates-error-code-151066"></a>Certificados de raiz fidedigna (código de erro 151066)

Se todos os certificados de raiz fidedigna mais recentes não estão presentes na VM, poderá falhar a tarefa "ativar replicação". Sem os certificados, a autenticação e autorização de chamadas de serviço de recuperação de sites da VM falharem. É apresentada a mensagem de erro para a tarefa de recuperação de sites "ativar replicação" falhada:

**Código de erro** | **Causa possível** | **Recommendations (Recomendações)**
--- | --- | ---
151066<br></br>**Mensagem**: Falha na configuração do Site Recovery. | Necessários fidedignos utilizados certificados de raiz para autorização e autenticação não estão presentes na máquina. | -Para uma VM com o sistema operativo do Windows, certifique-se de que os certificados de raiz fidedigna estão presentes na máquina. Para obter informações, consulte [configurar raízes confiáveis e não são permitidas certificados](https://technet.microsoft.com/library/dn265983.aspx).<br></br>-Para uma VM a executar o sistema operativo Linux, siga as orientações para certificados de raiz fidedigna publicada pelo distribuidor de versão do sistema operativo Linux.

### <a name="fix-the-problem"></a>Corrigir o problema
**Windows**

Instale todas as atualizações mais recentes do Windows na VM para que todos os certificados de raiz fidedigna estão presentes na máquina. Se fizer parte de um ambiente desligado, siga o processo de atualização padrão do Windows na sua organização para obter os certificados. Se os certificados necessários não estão presentes na VM, as chamadas para o serviço de recuperação de Site falharem por motivos de segurança.

Siga o processo normal do Windows update gestão ou o certificado atualização management na sua organização para obter todos os certificados de raiz mais recentes e a lista de revogação de certificado atualizado nas VMs.

Para verificar se o problema está resolvido, vá para login.microsoftonline.com partir de um browser na VM.

**Linux**

Siga as orientações fornecidas pelo distribuidor do Linux para obter os certificados de raiz fidedigna mais recentes e a mais recente lista de revogação de certificado na VM.

Como o SuSE Linux usa links simbólicos, para manter uma lista de certificados, siga estes passos:

1.  Inicie sessão como um utilizador de raiz.

2.  Execute este comando para alterar o diretório.

      ``# cd /etc/ssl/certs``

3. Verifique se o certificado de AC de raiz da Symantec está presente.

      ``# ls VeriSign_Class_3_Public_Primary_Certification_Authority_G5.pem``

4. Se não for encontrado o certificado de AC de raiz da Symantec, execute o seguinte comando para transferir o ficheiro. Verifique se existem quaisquer erros e siga a ação recomendada para falhas de rede.

      ``# wget https://www.symantec.com/content/dam/symantec/docs/other-resources/verisign-class-3-public-primary-certification-authority-g5-en.pem -O VeriSign_Class_3_Public_Primary_Certification_Authority_G5.pem``

5. Verifique se o certificado de AC de raiz Baltimore está presente.

      ``# ls Baltimore_CyberTrust_Root.pem``

6. Se não for encontrado o certificado de AC de raiz Baltimore, transfira o certificado.  

    ``# wget http://www.digicert.com/CACerts/BaltimoreCyberTrustRoot.crt.pem -O Baltimore_CyberTrust_Root.pem``

7. Verifique se o certificado de DigiCert_Global_Root_CA está presente.

    ``# ls DigiCert_Global_Root_CA.pem``

8. Se não for encontrado o DigiCert_Global_Root_CA, execute os seguintes comandos para transferir o certificado.

    ``# wget http://www.digicert.com/CACerts/DigiCertGlobalRootCA.crt``

    ``# openssl x509 -in DigiCertGlobalRootCA.crt -inform der -outform pem -out DigiCert_Global_Root_CA.pem``

9. Execute o script de rehash para atualizar o certificado de hashes de assunto para os certificados acabado de transferir.

    ``# c_rehash``

10. Verifique se o assunto codifica como links simbólicos são criados para os certificados.

    - Comando

      ``# ls -l | grep Baltimore``

    - Saída

      ``lrwxrwxrwx 1 root root   29 Jan  8 09:48 3ad48a91.0 -> Baltimore_CyberTrust_Root.pem
      -rw-r--r-- 1 root root 1303 Jun  5  2014 Baltimore_CyberTrust_Root.pem``

    - Comando

      ``# ls -l | grep VeriSign_Class_3_Public_Primary_Certification_Authority_G5``

    - Saída

      ``-rw-r--r-- 1 root root 1774 Jun  5  2014 VeriSign_Class_3_Public_Primary_Certification_Authority_G5.pem
      lrwxrwxrwx 1 root root   62 Jan  8 09:48 facacbc6.0 -> VeriSign_Class_3_Public_Primary_Certification_Authority_G5.pem``

    - Comando

      ``# ls -l | grep DigiCert_Global_Root``

    - Saída

      ``lrwxrwxrwx 1 root root   27 Jan  8 09:48 399e7759.0 -> DigiCert_Global_Root_CA.pem
      -rw-r--r-- 1 root root 1380 Jun  5  2014 DigiCert_Global_Root_CA.pem``

11. Criar uma cópia do ficheiro VeriSign_Class_3_Public_Primary_Certification_Authority_G5.pem com b204d74a.0 de nome de ficheiro

    ``# cp VeriSign_Class_3_Public_Primary_Certification_Authority_G5.pem b204d74a.0``

12. Criar uma cópia do ficheiro Baltimore_CyberTrust_Root.pem com 653b494a.0 de nome de ficheiro

    ``# cp Baltimore_CyberTrust_Root.pem 653b494a.0``

13. Criar uma cópia do ficheiro DigiCert_Global_Root_CA.pem com 3513523f.0 de nome de ficheiro

    ``# cp DigiCert_Global_Root_CA.pem 3513523f.0``  


14. Verifique se os ficheiros estão presentes.  

    - Comando

      ``# ls -l 653b494a.0 b204d74a.0 3513523f.0``

    - Saída

      ``-rw-r--r-- 1 root root 1774 Jan  8 09:52 3513523f.0
      -rw-r--r-- 1 root root 1303 Jan  8 09:52 653b494a.0
      -rw-r--r-- 1 root root 1774 Jan  8 09:52 b204d74a.0``


## <a name="outbound-connectivity-for-site-recovery-urls-or-ip-ranges-error-code-151037-or-151072"></a>Conectividade de saída para intervalos de IP ou URLs do Site Recovery (código de erro 151037 ou 151072)

Para replicação do Site Recovery para o trabalho, a conectividade de saída para URLs ou IP específicos a intervalos é necessária da VM. Se a VM estiver protegido por uma firewall ou utiliza regras de grupo (NSG) de segurança de rede para controlar a conectividade de saída, poderá deparar-se com um desses problemas.

### <a name="issue-1-failed-to-register-azure-virtual-machine-with-site-recovery-151037-br"></a>Problema 1: Falha ao registar a máquina virtual do Azure com o Site Recovery (151037) </br>
- **Causa possível** </br>
  - Está a utilizar o NSG para controlar o acesso de saída na VM e o IP necessário intervalos não estão na lista de permissões para acesso de saída.
  - Estiver a utilizar ferramentas de firewall de terceiros e os necessários intervalos IP/URLs não estão na lista de permissões.


- **Resolução**
   - Se estiver a utilizar o proxy de firewall para controlar a conectividade de rede de saída na VM, certifique-se de que os URLs de pré-requisito ou intervalos IP do datacenter estão na lista de permissões. Para obter informações, consulte [documentação de orientação do proxy de firewall](https://aka.ms/a2a-firewall-proxy-guidance).
   - Se estiver a utilizar as regras do NSG para controlar a conectividade de rede de saída na VM, certifique-se de que os intervalos IP do datacenter pré-requisitos estão na lista de permissões. Para obter informações, consulte [orientação do grupo de segurança de rede](azure-to-azure-about-networking.md).
   - A lista branca [os URLs necessários](azure-to-azure-about-networking.md#outbound-connectivity-for-urls) ou o [necessário intervalos de IP](azure-to-azure-about-networking.md#outbound-connectivity-for-ip-address-ranges), siga os passos no [documento de orientação para funcionamento em rede](azure-to-azure-about-networking.md).

### <a name="issue-2-site-recovery-configuration-failed-151072"></a>Problema 2: Configuração do Site Recovery falhou (151072)
- **Causa possível** </br>
  - Não é possível estabelecer ligação a pontos finais de serviço de recuperação de sites


- **Resolução**
   - Se estiver a utilizar o proxy de firewall para controlar a conectividade de rede de saída na VM, certifique-se de que os URLs de pré-requisito ou intervalos IP do datacenter estão na lista de permissões. Para obter informações, consulte [documentação de orientação do proxy de firewall](https://aka.ms/a2a-firewall-proxy-guidance).
   - Se estiver a utilizar as regras do NSG para controlar a conectividade de rede de saída na VM, certifique-se de que os intervalos IP do datacenter pré-requisitos estão na lista de permissões. Para obter informações, consulte [orientação do grupo de segurança de rede](https://aka.ms/a2a-nsg-guidance).
   - A lista branca [os URLs necessários](azure-to-azure-about-networking.md#outbound-connectivity-for-urls) ou o [necessário intervalos de IP](azure-to-azure-about-networking.md#outbound-connectivity-for-ip-address-ranges), siga os passos no [documento de orientação para funcionamento em rede](site-recovery-azure-to-azure-networking-guidance.md).

### <a name="issue-3-a2a-replication-failed-when-the-network-traffic-goes-through-on-premise-proxy-server-151072"></a>Problema 3: A replicação A2A falha quando o tráfego de rede atravessa o servidor de proxy no local (151072)
 - **Causa possível** </br>
   - As definições de proxy personalizado são inválidas e agente do serviço de mobilidade de ASR detetar automaticamente as definições de proxy do IE


 - **Resolução**
  1.    Agente do serviço de mobilidade Deteta as definições de proxy do IE no Windows e /etc/environment no Linux.
  2.  Se preferir definir proxy apenas para o serviço de mobilidade de ASR, em seguida, pode fornecer os detalhes do proxy no ProxyInfo.conf localizado em:</br>
      - ``/usr/local/InMage/config/`` no ***Linux***
      - ``C:\ProgramData\Microsoft Azure Site Recovery\Config`` no ***Windows***
  3.    O ProxyInfo.conf deve ter as definições de proxy no seguinte formato INI. </br>
                   *[proxy]*</br>
                   *Endereço =http://1.2.3.4*</br>
                   *Porta = 567*</br>
  4. Agente do serviço de mobilidade de ASR só suporta ***proxies utenticados***.

### <a name="fix-the-problem"></a>Corrigir o problema
A lista branca [os URLs necessários](azure-to-azure-about-networking.md#outbound-connectivity-for-urls) ou o [necessário intervalos de IP](azure-to-azure-about-networking.md#outbound-connectivity-for-ip-address-ranges), siga os passos no [documento de orientação para funcionamento em rede](site-recovery-azure-to-azure-networking-guidance.md).

## <a name="disk-not-found-in-the-machine-error-code-150039"></a>Não foi encontrado na máquina (código de erro 150039) de disco

Um disco novo anexado à VM tem de ser inicializado.

**Código de erro** | **Causas possíveis** | **Recommendations (Recomendações)**
--- | --- | ---
150039<br></br>**Mensagem**: o disco de dados do Azure (DiskName) (DiskURI) com o número de unidade lógica (LUN) (LUNValue) não foi mapeado para um disco correspondente a ser comunicado pela dentro da VM que tenha o mesmo valor LUN. | -Um novo disco de dados foi anexado à VM, mas ele não foi inicializado.</br></br>-O disco de dados dentro da VM não está corretamente a comunicar o valor LUN em que o disco foi anexado à VM.| Certifique-se de que os discos de dados são inicializados e, em seguida, repita a operação.</br></br>Para Windows: [Attach e inicializar um novo disco](https://docs.microsoft.com/azure/virtual-machines/windows/attach-managed-disk-portal).</br></br>Para Linux: [inicializar um novo disco de dados no Linux](https://docs.microsoft.com/azure/virtual-machines/linux/add-disk).

### <a name="fix-the-problem"></a>Corrigir o problema
Certifique-se de que os discos de dados foram inicializados e, em seguida, repita a operação:

- Para Windows: [Attach e inicializar um novo disco](https://docs.microsoft.com/azure/virtual-machines/windows/attach-managed-disk-portal).
- Para Linux: [adicione um novo disco de dados no Linux](https://docs.microsoft.com/azure/virtual-machines/linux/add-disk).

Se o problema persistir, contacte o suporte.


## <a name="unable-to-see-the-azure-vm-for-selection-in-enable-replication"></a>Não é possível ver a VM do Azure para seleção em "ativar replicação"

 **Causa 1: Grupo de recursos e a Máquina Virtual de origem estão numa localização diferente** <br>
O Azure Site Recovery atualmente impostas pela que o grupo de recursos de região de origem e de máquinas virtuais devem estar na mesma localização. Se não for esse o caso, em seguida, não seria capaz de encontrar a máquina virtual durante o tempo de proteção.

**Causa 2: O grupo de recursos não é parte da subscrição selecionada** <br>
Poderá não conseguir encontrar o grupo de recursos no momento da proteção, se não fizer parte de uma determinada subscrição. Certifique-se de que o grupo de recursos pertence à subscrição que está a ser utilizada.

 **Causa 3: Configuração de obsoleta** <br>
Se não vir a VM que pretende ativar para a replicação, pode ser devido a uma configuração de recuperação de Site obsoleta deixado na VM do Azure. A configuração obsoleta poderá apresentar numa VM do Azure nos seguintes casos:

- Ativar a replicação para a VM do Azure utilizando a recuperação de Site e, em seguida, eliminar o Cofre de recuperação de sites sem explicitamente a desativar a replicação na VM.
- Ativar a replicação para a VM do Azure utilizando a recuperação de Site e, em seguida, eliminar o grupo de recursos que contém o Cofre de recuperação de sites sem explicitamente a desativar a replicação na VM.

### <a name="fix-the-problem"></a>Corrigir o problema

Pode usar [remover o script de configuração de ASR obsoleto](https://gallery.technet.microsoft.com/Azure-Recovery-ASR-script-3a93f412) e remover a configuração da recuperação de Site obsoleta na VM do Azure. Deve ser capaz de ver a VM depois de remover a configuração obsoleta.

## <a name="unable-to-select-virtual-machine-for-protection"></a>Não é possível selecionar a Máquina Virtual para proteção 
 **Causa 1: a Máquina Virtual tem alguns extensão instalado num Estado com falhas ou não responde** <br>
 Vá para as máquinas virtuais > configuração > extensões e verifique se existem quaisquer extensões em estado de falha. Desinstale a extensão com falha e repita a proteger a máquina virtual.<br>
 **Causa 2: [o estado de aprovisionamento da VM não é válido](#vms-provisioning-state-is-not-valid-error-code-150019)**

## <a name="vms-provisioning-state-is-not-valid-error-code-150019"></a>Estado de aprovisionamento da VM não é válido (código de erro 150019)

Para ativar a replicação da VM, deve ser o estado de aprovisionamento **bem-sucedido**. Pode verificar o estado VM ao seguir os passos abaixo.

1.  Selecione o **Explorador de recursos** partir **todos os serviços** no portal do Azure.
2.  Expanda a **subscrições** lista e selecione a sua subscrição.
3.  Expanda a **ResourceGroups** lista e selecione o grupo de recursos da VM.
4.  Expanda a **recursos** lista e selecione a sua máquina virtual
5.  Verifique os **provisioningState** campo na vista de instância no lado direito.

### <a name="fix-the-problem"></a>Corrigir o problema

- Se **provisioningState** é **falha**, contacte o suporte com detalhes para resolver problemas.
- Se **provisioningState** é **atualização**, outra extensão foi possível obter implementar. Verifique se existem quaisquer operações em curso na VM, aguarde que estas seja concluída e tente novamente a recuperação de Site com falha **ativar a replicação** tarefa.

## <a name="unable-to-select-target-virtual-network---network-selection-tab-is-grayed-out"></a>Não é possível selecionar o destino de rede virtual - separador de seleção de rede está a cinzento.

**Causa 1: Se a sua VM está ligada a uma rede que já está mapeada para uma "rede de destino".**
- Se a VM de origem faz parte de uma rede virtual e a outra VM na mesma rede virtual já está mapeada com uma rede no grupo de recursos de destino, em seguida, pela seleção de rede predefinida pendente será desativada.

![Network_Selection_greyed_out](./media/site-recovery-azure-to-azure-troubleshoot/unabletoselectnw.png)

**Causa 2: Se tiver protegido a VM com o Azure Site Recovery e desativado a replicação.**
 - A desativar a replicação de uma VM não elimina o mapeamento da rede. Tem de ser eliminadas do cofre dos serviços de recuperação onde a VM foi protegida. </br>
 Navegue para cofre dos serviços de recuperação > infraestrutura do Site Recovery > mapeamento de rede. </br>
 ![Delete_NW_Mapping](./media/site-recovery-azure-to-azure-troubleshoot/delete_nw_mapping.png)
 - Rede de destino configurado durante a configuração de recuperação após desastre pode ser alterado após a configuração, depois da VM está protegida inicial. </br>
 ![Modify_NW_mapping](./media/site-recovery-azure-to-azure-troubleshoot/modify_nw_mapping.png)
 - Tenha em atenção que a alteração de mapeamento de rede afeta todas protegidos VMs que utilizam esse mapeamento de rede específicas.


## <a name="comvolume-shadow-copy-service-error-error-code-151025"></a>COM c++ /CLI erro de serviço de cópia sombra de volumes (código de erro 151025)
**Código de erro** | **Causas possíveis** | **Recommendations (Recomendações)**
--- | --- | ---
151025<br></br>**Mensagem**: extensão de recuperação de Site Falha ao instalar | -Serviço de "sistema aplicação COM+" desativado.</br></br>-O serviço de "Cópia de sombra de volume" está desativado.| Definir serviços "sistema aplicação COM+" e "Cópia de sombra de volumes" para o modo de arranque automático ou manual.

### <a name="fix-the-problem"></a>Corrigir o problema

Pode abrir a consola de "Serviços" e certifique-se o "sistema aplicação COM+" e "Cópia de sombra de Volume" não estão definida como "Disabled" de tipo de arranque.
  ![Erro de com](./media/azure-to-azure-troubleshoot-errors/com-error.png)

## <a name="next-steps"></a>Passos Seguintes
[Replicar máquinas virtuais do Azure](site-recovery-replicate-azure-to-azure.md)
