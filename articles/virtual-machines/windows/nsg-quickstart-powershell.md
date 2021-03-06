---
title: Abrir portas para uma VM com o Azure PowerShell | Documentos da Microsoft
description: Saiba como abrir uma porta / criar um ponto final para a sua VM do Windows com o modo de implementação do Azure resource manager e o Azure PowerShell
services: virtual-machines-windows
documentationcenter: ''
author: cynthn
manager: jeconnoc
editor: ''
ms.assetid: cf45f7d8-451a-48ab-8419-730366d54f1e
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 12/13/2017
ms.author: cynthn
ms.openlocfilehash: f79db8cdec0aa48ae300aff4c58072fb6afdc932
ms.sourcegitcommit: aa988666476c05787afc84db94cfa50bc6852520
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/10/2018
ms.locfileid: "37932787"
---
# <a name="how-to-open-ports-and-endpoints-to-a-vm-in-azure-using-powershell"></a>Como abrir portas e pontos finais para uma VM no Azure com o PowerShell
[!INCLUDE [virtual-machines-common-nsg-quickstart](../../../includes/virtual-machines-common-nsg-quickstart.md)]

## <a name="quick-commands"></a>Comandos rápidos
Para criar um grupo de segurança de rede e a ACL de regras de que precisa [a versão mais recente do Azure PowerShell instalada](/powershell/azureps-cmdlets-docs). Também pode [executar estes passos com o portal do Azure](nsg-quickstart-portal.md).

Inicie sessão sua conta do Azure:

```powershell
Connect-AzureRmAccount
```

Nos exemplos a seguir, substitua os nomes de parâmetros com seus próprios valores. Os nomes de parâmetros de exemplo incluídos *myResourceGroup*, *myNetworkSecurityGroup*, e *myVnet*.

Crie uma regra com [New-AzureRmNetworkSecurityRuleConfig](/powershell/module/azurerm.network/new-azurermnetworksecurityruleconfig). O exemplo seguinte cria uma regra com o nome *myNetworkSecurityGroupRule* para permitir *tcp* tráfego na porta *80*:

```powershell
$httprule = New-AzureRmNetworkSecurityRuleConfig `
    -Name "myNetworkSecurityGroupRule" `
    -Description "Allow HTTP" `
    -Access "Allow" `
    -Protocol "Tcp" `
    -Direction "Inbound" `
    -Priority "100" `
    -SourceAddressPrefix "Internet" `
    -SourcePortRange * `
    -DestinationAddressPrefix * `
    -DestinationPortRange 80
```

Em seguida, crie o seu grupo de segurança de rede [New-AzureRmNetworkSecurityGroup](/powershell/module/azurerm.network/new-azurermnetworksecuritygroup) e atribuir a regra HTTP que acabou de criar da seguinte forma. O exemplo seguinte cria um grupo de segurança de rede com o nome *myNetworkSecurityGroup*:

```powershell
$nsg = New-AzureRmNetworkSecurityGroup `
    -ResourceGroupName "myResourceGroup" `
    -Location "EastUS" `
    -Name "myNetworkSecurityGroup" `
    -SecurityRules $httprule
```

Agora vamos atribuir o seu grupo de segurança de rede a uma sub-rede. O exemplo seguinte atribui uma rede virtual existente com o nome *myVnet* à variável *$vnet* com [Get-AzureRmVirtualNetwork](/powershell/module/azurerm.network/get-azurermvirtualnetwork):

```powershell
$vnet = Get-AzureRmVirtualNetwork `
    -ResourceGroupName "myResourceGroup" `
    -Name "myVnet"
```

Associar o seu grupo de segurança de rede com a sub-rede com [Set-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/set-azurermvirtualnetworksubnetconfig). O exemplo seguinte associa a sub-rede denominada *mySubnet* com o seu grupo de segurança de rede:

```powershell
$subnetPrefix = $vnet.Subnets|?{$_.Name -eq 'mySubnet'}

Set-AzureRmVirtualNetworkSubnetConfig `
    -VirtualNetwork $vnet `
    -Name "mySubnet" `
    -AddressPrefix $subnetPrefix.AddressPrefix `
    -NetworkSecurityGroup $nsg
```

Por fim, atualize a sua rede virtual com [Set-AzureRmVirtualNetwork](/powershell/module/azurerm.network/set-azurermvirtualnetwork) para que as alterações entrem em vigor:

```powershell
Set-AzureRmVirtualNetwork -VirtualNetwork $vnet
```


## <a name="more-information-on-network-security-groups"></a>Obter mais informações sobre grupos de segurança de rede
Os comandos rápidos aqui permitem-lhe começar a trabalhar com o tráfego que flui à sua VM. Grupos de segurança de rede fornecem muitos excelentes recursos e granularidade para controlar o acesso aos seus recursos. Pode ler mais sobre [criar um grupo de segurança de rede e a ACL de regras aqui](tutorial-virtual-network.md#secure-network-traffic).

Para aplicações web de elevada disponibilidade, deve colocar as suas VMs por trás de um balanceador de carga do Azure. O Balanceador de carga distribui o tráfego para VMs, com um grupo de segurança de rede que fornece a filtragem de tráfego. Para obter mais informações, consulte [como a carga de balanceamento de máquinas virtuais do Linux no Azure para criar uma aplicação de elevada disponibilidade](tutorial-load-balancer.md).

## <a name="next-steps"></a>Passos Seguintes
Neste exemplo, criou uma regra simple para permitir tráfego HTTP. Pode encontrar informações sobre como criar ambientes mais detalhadas nos seguintes artigos:

* [Descrição geral do Azure Resource Manager](../../azure-resource-manager/resource-group-overview.md)
* [O que é um grupo de segurança de rede?](../../virtual-network/security-overview.md)
* [Descrição geral do Gestor de recursos do Azure para balanceadores de carga](../../load-balancer/load-balancer-arm.md)

