---
title: Resolver problemas de ligação de SSH para uma VM do Azure | Documentos da Microsoft
description: Como resolver problemas, como 'Falha na ligação de SSH' ou "Recusada a ligação de SSH" para uma VM do Azure em execução no Linux.
keywords: SSH ligação recusada, ssh erro, azure ssh, ligação de SSH falhou
services: virtual-machines-linux
documentationcenter: ''
author: genlin
manager: jeconnoc
editor: ''
tags: top-support-issue,azure-service-management,azure-resource-manager
ms.assetid: dcb82e19-29b2-47bb-99f2-900d4cfb5bbb
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.topic: troubleshooting
ms.date: 05/30/2017
ms.author: genli
ms.openlocfilehash: 45ddb31a20b830de29cf77231bb3acd6f8917601
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/07/2018
ms.locfileid: "51230908"
---
# <a name="troubleshoot-ssh-connections-to-an-azure-linux-vm-that-fails-errors-out-or-is-refused"></a>Resolver problemas de ligações SSH para uma VM do Linux do Azure que falhar, erros de saída, ou for recusada
Existem vários motivos que encontrar erros de Secure Shell (SSH), falhas de ligação de SSH, ou o SSH é recusado ao tentar ligar a uma máquina virtual (VM) do Linux. Este artigo ajuda-o a encontrar e corrigir os problemas. Pode utilizar o portal do Azure, CLI do Azure, ou a extensão de acesso de VM para Linux para resolver problemas de ligação.

[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-both-include.md)]

Se precisar de mais ajuda a qualquer momento neste artigo, pode contactar os especialistas do Azure no [os fóruns do Azure do MSDN e Stack Overflow](https://azure.microsoft.com/support/forums/). Em alternativa, pode enviar um incidente de suporte do Azure. Vá para o [site de suporte do Azure](https://azure.microsoft.com/support/options/) e selecione **obter suporte**. Para informações sobre como utilizar o suporte do Azure, leia os [FAQ do suporte Microsoft Azure](https://azure.microsoft.com/support/faq/).

## <a name="quick-troubleshooting-steps"></a>Passos de resolução de problemas rápidos
Após cada passo de resolução de problemas, tente voltar a ligar à VM.

1. Repor a configuração de SSH.
2. Repor as credenciais do utilizador.
3. Verifique se o [grupo de segurança de rede](../../virtual-network/security-overview.md) regras permitem tráfego SSH.
   * Certifique-se de que existe uma regra de grupo de segurança de rede para permitir tráfego SSH (por predefinição, a porta TCP 22).
   * Não é possível utilizar o redirecionamento de porta / mapeamento sem utilizar um balanceador de carga do Azure.
4. Verifique os [estado de funcionamento de recursos VM](../../resource-health/resource-health-overview.md). 
   * Certifique-se de que a VM reporta como estando em bom estado.
   * Se tiver ativado o diagnóstico de arranque, certifique-se de que a VM não está a comunicar erros nos registos de arranque.
5. Reinicie a VM.
6. Reimplemente a VM.

Continue a ler para obter passos de resolução de problemas mais detalhados e explicações.

## <a name="available-methods-to-troubleshoot-ssh-connection-issues"></a>Métodos disponíveis para resolver problemas de ligação de SSH
Pode repor as credenciais ou de configuração de SSH utilizando um dos seguintes métodos:

* [Portal do Azure](#use-the-azure-portal) - excelente se precisar de repor rapidamente a configuração de SSH ou a chave SSH e não tem as ferramentas do Azure instaladas.
* [CLI do Azure](#use-the-azure-cli) – se já estiver na linha de comando, rapidamente repor a configuração de SSH ou as credenciais. Também pode utilizar o [da CLI do Azure](#use-the-azure-classic-cli)
* [Extensão de VMAccessForLinux Azure](#use-the-vmaccess-extension) - criar e reutilizar os arquivos de definição de json para repor as credenciais de utilizador ou de configuração de SSH.

Após cada passo de resolução de problemas, tente ligar-se novamente à sua VM. Se ainda não conseguir ligar, experimente a próxima etapa.

## <a name="use-the-azure-portal"></a>Utilizar o portal do Azure
O portal do Azure fornece uma forma rápida de repor as credenciais de utilizador ou de configuração de SSH sem instalar quaisquer ferramentas no seu computador local.

Selecione a sua VM no portal do Azure. Desloque para baixo para o **suporte + resolução de problemas** secção e selecione **Repor palavra-passe** como no exemplo seguinte:

![Repor configuração SSH ou as credenciais no portal do Azure](./media/troubleshoot-ssh-connection/reset-credentials-using-portal.png)

### <a name="reset-the-ssh-configuration"></a>Reposição da configuração SSH
Como primeiro passo, selecione `Reset configuration only` partir a **modo** menu pendente como na captura de ecrã anterior, em seguida, clique no **repor** botão. Quando esta ação tiver sido concluída, tente aceder novamente à sua VM.

### <a name="reset-ssh-credentials-for-a-user"></a>Repor as credenciais SSH para um utilizador
Para repor as credenciais de um utilizador existente, selecione `Reset SSH public key` ou `Reset password` partir a **modo** menu pendente, como na captura de ecrã anterior. Especifique o nome de utilizador e uma chave SSH ou a nova palavra-passe, em seguida, clique nas **repor** botão.

Também pode criar um utilizador com privilégios sudo na VM neste menu. Introduza um novo nome de utilizador e palavra-passe associada ou uma chave SSH e, em seguida, clique nas **repor** botão.

### <a name="check-security-rules"></a>Regras de segurança de verificação

Uso [fluxo de IP](../../network-watcher/network-watcher-check-ip-flow-verify-portal.md) para confirmar se uma regra num grupo de segurança de rede está a bloquear o tráfego de ou para uma máquina virtual. Também pode consultar as regras do grupo de segurança efetivas para se certificar de NSG "Permitir entrada" regra existe e está priorizada para a porta RDP (predefinição 22). Para obter mais informações, consulte [usando regras de segurança efetivas para resolver problemas da VM de fluxo de tráfego](../../virtual-network/diagnose-network-traffic-filter-problem.md).

### <a name="check-routing"></a>Verifique o encaminhamento

Utilizar o observador de rede [do próximo salto](../../network-watcher/network-watcher-check-next-hop-portal.md) capacidade para confirmar que uma rota não está a impedir o tráfego, de que está a ser encaminhados para ou de uma máquina virtual. Também pode rever as rotas efetivas para ver todas as rotas efetivas para uma interface de rede. Para obter mais informações, consulte [fluxo de tráfego de utilizar rotas efetivas para resolver problemas da VM](../../virtual-network/diagnose-network-routing-problem.md).

## <a name="use-the-azure-cli"></a>Utilizar a CLI do Azure
Se ainda não o fez, instale a versão mais recente [CLI do Azure](/cli/azure/install-az-cli2) e para iniciar sessão no Azure através da conta [início de sessão az](/cli/azure/reference-index#az_login).

Se criou e carregou uma imagem de disco personalizada do Linux, certifique-se de que o [o agente Linux do Microsoft Azure](../extensions/agent-windows.md) versão 2.0.5 ou posterior está instalado. Para as VMs criadas com imagens da galeria, esta extensão de acesso já está instalado e configurado para si.

### <a name="reset-ssh-configuration"></a>Repor configuração SSH
Pode inicialmente tentar repor a configuração de SSH para os valores predefinidos e reiniciar o servidor SSH na VM. Tenha em atenção que isto não altera o nome de conta de utilizador, palavra-passe ou chaves SSH.
O exemplo seguinte utiliza [pt vm utilizador reposição-ssh](/cli/azure/vm/user#az_vm_user_reset_ssh) reposição da configuração de SSH na VM com o nome `myVM` no `myResourceGroup`. Utilize os seus próprios valores da seguinte forma:

```azurecli
az vm user reset-ssh --resource-group myResourceGroup --name myVM
```

### <a name="reset-ssh-credentials-for-a-user"></a>Repor as credenciais SSH para um utilizador
O exemplo seguinte utiliza [atualização do utilizador az vm](/cli/azure/vm/user#az_vm_user_update) para repor as credenciais dos `myUsername` para o valor especificado na `myPassword`, na VM com o nome `myVM` no `myResourceGroup`. Utilize os seus próprios valores da seguinte forma:

```azurecli
az vm user update --resource-group myResourceGroup --name myVM \
     --username myUsername --password myPassword
```

Se utilizar a autenticação de chave SSH, pode repor a chave SSH para um determinado usuário. O exemplo seguinte utiliza **az vm aceder o utilizador do linux do conjunto** para atualizar a chave SSH armazenada na `~/.ssh/id_rsa.pub` para o utilizador com o nome `myUsername`, na VM com o nome `myVM` no `myResourceGroup`. Utilize os seus próprios valores da seguinte forma:

```azurecli
az vm user update --resource-group myResourceGroup --name myVM \
    --username myUsername --ssh-key-value ~/.ssh/id_rsa.pub
```

## <a name="use-the-vmaccess-extension"></a>Utilizar a extensão VMAccess
Lê a extensão de acesso de VM para Linux num ficheiro json que define as ações a realizar. Essas ações incluem repor SSHD, reposição de uma chave SSH ou adicionar um utilizador. Continuar a utilizar a CLI do Azure para chamar a extensão VMAccess, mas é possível reutilizar os ficheiros json em várias VMs se assim o desejar. Esta abordagem permite-lhe criar um repositório de ficheiros json que pode ser chamado para fornecido cenários.

### <a name="reset-sshd"></a>Repor SSHD
Crie um ficheiro denominado `settings.json` com o seguinte conteúdo:

```json
{  
    "reset_ssh":"True"
}
```

Utilizar a CLI do Azure, em seguida, chame o `VMAccessForLinux` extensão para repor a sua ligação de SSHD ao especificar o ficheiro json. O exemplo seguinte utiliza [conjunto de extensão az vm](/cli/azure/vm/extension#az_vm_extension_set) SSHD na VM com o nome de repor `myVM` no `myResourceGroup`. Utilize os seus próprios valores da seguinte forma:

```azurecli
az vm extension set --resource-group philmea --vm-name Ubuntu \
    --name VMAccessForLinux --publisher Microsoft.OSTCExtensions --version 1.2 --settings settings.json
```

### <a name="reset-ssh-credentials-for-a-user"></a>Repor as credenciais SSH para um utilizador
Se for apresentada SSHD funcionar corretamente, pode repor as credenciais para um utilizador giver. Para repor a palavra-passe para um utilizador, crie um ficheiro denominado `settings.json`. O exemplo seguinte repõe as credenciais para `myUsername` para o valor especificado no `myPassword`. Introduza as seguintes linhas no seu `settings.json` de ficheiros, com seus próprios valores:

```json
{
    "username":"myUsername", "password":"myPassword"
}
```

Ou para repor a chave SSH para um utilizador, primeiro crie um ficheiro denominado `settings.json`. O exemplo seguinte repõe as credenciais para `myUsername` para o valor especificado na `myPassword`, na VM com o nome `myVM` no `myResourceGroup`. Introduza as seguintes linhas no seu `settings.json` de ficheiros, com seus próprios valores:

```json
{
    "username":"myUsername", "ssh_key":"mySSHKey"
}
```

Depois de criar o ficheiro json, utilizar a CLI do Azure para chamar o `VMAccessForLinux` extensão para repor as credenciais de utilizador SSH ao especificar o ficheiro json. O exemplo seguinte repõe as credenciais na VM com o nome `myVM` em `myResourceGroup`. Utilize os seus próprios valores da seguinte forma:

```azurecli
az vm extension set --resource-group philmea --vm-name Ubuntu \
    --name VMAccessForLinux --publisher Microsoft.OSTCExtensions --version 1.2 --settings settings.json
```

## <a name="use-the-azure-classic-cli"></a>Utilizar a CLI clássica do Azure
Se ainda não o fez, [instalar a CLI clássica do Azure e ligar à sua subscrição do Azure](../../cli-install-nodejs.md). Certifique-se de que está a utilizar modo Resource Manager da seguinte forma:

```azurecli
azure config mode arm
```

Se criou e carregou uma imagem de disco personalizada do Linux, certifique-se de que o [o agente Linux do Microsoft Azure](../extensions/agent-windows.md) versão 2.0.5 ou posterior está instalado. Para as VMs criadas com imagens da galeria, esta extensão de acesso já está instalado e configurado para si.

### <a name="reset-ssh-configuration"></a>Repor configuração SSH
A configuração de SSHD em si pode estar configurado incorretamente ou o serviço encontrou um erro. Pode redefinir SSHD para se certificar de que a configuração de SSH em si é válida. A repor SSHD deve ser a primeira etapa de resolução de problemas que tomar.

O exemplo seguinte repõe SSHD numa VM com o nome `myVM` no grupo de recursos com o nome `myResourceGroup`. Utilize os seus próprios nomes de grupos de recursos e de VM da seguinte forma:

```azurecli
azure vm reset-access --resource-group myResourceGroup --name myVM \
    --reset-ssh
```

### <a name="reset-ssh-credentials-for-a-user"></a>Repor as credenciais SSH para um utilizador
Se for apresentada SSHD funcionar corretamente, pode repor a palavra-passe para um utilizador giver. O exemplo seguinte repõe as credenciais para `myUsername` para o valor especificado na `myPassword`, na VM com o nome `myVM` no `myResourceGroup`. Utilize os seus próprios valores da seguinte forma:

```azurecli
azure vm reset-access --resource-group myResourceGroup --name myVM \
     --user-name myUsername --password myPassword
```

Se utilizar a autenticação de chave SSH, pode repor a chave SSH para um determinado usuário. O exemplo seguinte atualiza a chave SSH armazenada em `~/.ssh/id_rsa.pub` para o utilizador com o nome `myUsername`, na VM com o nome `myVM` no `myResourceGroup`. Utilize os seus próprios valores da seguinte forma:

```azurecli
azure vm reset-access --resource-group myResourceGroup --name myVM \
    --user-name myUsername --ssh-key-file ~/.ssh/id_rsa.pub
```


## <a name="restart-a-vm"></a>Reiniciar uma VM
Se tiver repor as credenciais de utilizador e a configuração de SSH, ou se encontrou um erro ao fazer isso, pode tentar reiniciar a VM para endereço subjacente a problemas de computação.

### <a name="azure-portal"></a>Portal do Azure
Para reiniciar uma VM com o portal do Azure, selecione a sua VM e clique nas **reiniciar** botão como no exemplo seguinte:

![Reiniciar uma VM no portal do Azure](./media/troubleshoot-ssh-connection/restart-vm-using-portal.png)

### <a name="azure-classic-cli"></a>CLI clássica do Azure
O seguinte exemplo reinicia a VM com o nome `myVM` no grupo de recursos com o nome `myResourceGroup`. Utilize os seus próprios valores da seguinte forma:

```azurecli
azure vm restart --resource-group myResourceGroup --name myVM
```

### <a name="azure-cli"></a>CLI do Azure
O exemplo seguinte utiliza [reinício da vm az](/cli/azure/vm#az_vm_restart) para reiniciar a VM com o nome `myVM` no grupo de recursos com o nome `myResourceGroup`. Utilize os seus próprios valores da seguinte forma:

```azurecli
az vm restart --resource-group myResourceGroup --name myVM
```


## <a name="redeploy-a-vm"></a>Reimplementar uma VM
Pode Reimplementar uma VM para outro nó no Azure, que pode corrigir problemas de rede subjacentes. Para obter informações sobre a Reimplementar uma VM, consulte [Reimplementar a máquina virtual para o novo nó do Azure](../windows/redeploy-to-new-node.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

> [!NOTE]
> Depois de concluída esta operação, disco efémero dados serão perdidos e endereços IP dinâmicos que estão associados a máquina virtual serão atualizados.
> 
> 

### <a name="azure-portal"></a>Portal do Azure
Para Reimplementar uma VM com o portal do Azure, selecione a sua VM e desloque para baixo para o **suporte + resolução de problemas** secção. Clique nas **Reimplementar** botão como no exemplo seguinte:

![Reimplementar uma VM no portal do Azure](./media/troubleshoot-ssh-connection/redeploy-vm-using-portal.png)

### <a name="azure-classic-cli"></a>CLI clássica do Azure
O exemplo seguinte reimplementa a VM com o nome `myVM` no grupo de recursos com o nome `myResourceGroup`. Utilize os seus próprios valores da seguinte forma:

```azurecli
azure vm redeploy --resource-group myResourceGroup --name myVM
```

### <a name="azure-cli"></a>CLI do Azure
A utilização de exemplo seguintes [reimplementação da vm de az](/cli/azure/vm#az_vm_redeploy) para voltar a implementar a VM com o nome `myVM` no grupo de recursos com o nome `myResourceGroup`. Utilize os seus próprios valores da seguinte forma:

```azurecli
az vm redeploy --resource-group myResourceGroup --name myVM
```

## <a name="vms-created-by-using-the-classic-deployment-model"></a>VMs criadas utilizando o modelo de implementação clássica
Experimente estes passos para resolver falhas mais comuns para a ligação de SSH para VMs que foram criadas utilizando o modelo de implementação clássica. Após cada passo, tente voltar a ligar à VM.

* Repor acesso remoto a partir da [portal do Azure](https://portal.azure.com). No portal do Azure, selecione a sua VM e clique no **reposição remota...**  botão.
* Reinicie a VM. Sobre o [portal do Azure](https://portal.azure.com), selecione a sua VM e clique nas **reiniciar** botão.
    
* Reimplemente a VM para um novo nó do Azure. Para obter informações sobre como Reimplementar uma VM, consulte [Reimplementar a máquina virtual para o novo nó do Azure](../windows/redeploy-to-new-node.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
  
    Depois de concluída esta operação, disco efémero dados serão perdidos e endereços IP dinâmicos que estão associados a máquina virtual serão atualizados.
* Siga as instruções em [como repor uma palavra-passe ou SSH para máquinas virtuais baseadas em Linux](../linux/classic/reset-access-classic.md?) para:
  
  * Repor a palavra-passe ou chave SSH.
  * Criar uma *sudo* conta de utilizador.
  * Repor a configuração de SSH.
* Verifique o estado de funcionamento do recurso da VM para quaisquer problemas de plataforma.<br>
     Selecione a sua VM e desloque para baixo **configurações** > **verificar o estado de funcionamento**.

## <a name="additional-resources"></a>Recursos adicionais
* Se ainda não é possível para encaminhar o SSH à VM depois de seguir os passos após, consulte [mais passos de resolução de problemas](detailed-troubleshoot-ssh-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) para rever os passos adicionais para resolver seu problema.
* Para obter mais informações sobre resolução de problemas de acesso da aplicação, consulte [resolver problemas de acesso para uma aplicação em execução numa máquina virtual do Azure](../windows/troubleshoot-app-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* Para obter mais informações sobre resolução de problemas de máquinas virtuais que foram criadas utilizando o modelo de implementação clássica, consulte [como repor uma palavra-passe ou SSH para máquinas virtuais baseadas em Linux](../linux/classic/reset-access-classic.md).


