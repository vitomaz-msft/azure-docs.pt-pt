---
title: Criar um público Balanceador de carga Standard com frontend de endereço IP público com redundância de zona com o PowerShell | Documentos da Microsoft
description: Saiba como criar Standard de Balanceador de carga público com um frontend de endereço IP público com redundância de zona com o PowerShell
services: load-balancer
documentationcenter: na
author: KumudD
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/23/2018
ms.author: kumud
ms.openlocfilehash: 9dfd477678148f3e4bb80bafe61e4867dab54c8e
ms.sourcegitcommit: ae45eacd213bc008e144b2df1b1d73b1acbbaa4c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/01/2018
ms.locfileid: "50739537"
---
#  <a name="create-a-public-load-balancer-standard-with-zone-redundant-public-ip-address-frontend-using-powershell"></a>Criar um público Balanceador de carga Standard com frontend de endereço IP público com redundância de zona com o PowerShell

Este artigo mostra-se através da criação de um público [Balanceador de carga Standard](https://aka.ms/azureloadbalancerstandard) com um front-end com redundância de zona utilizando um endereço IP público Standard.

Se não tiver uma subscrição do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de começar.

> [!NOTE]
 Suporte para zonas de disponibilidade está disponível para os recursos do Azure selecionados e regiões e famílias de tamanhos VM. Para obter mais informações sobre como começar a utilizar e quais recursos do Azure, regiões e famílias de tamanhos VM pode experimentar com as zonas de disponibilidade, consulte [descrição geral das zonas de disponibilidade](https://docs.microsoft.com/azure/availability-zones/az-overview). Para obter suporte, pode contactar-nos no [Stack Overflow](https://stackoverflow.com/questions/tagged/azure-availability-zones) ou [abrir um pedido de suporte do Azure](../azure-supportability/how-to-create-azure-support-request.md?toc=%2fazure%2fvirtual-network%2ftoc.json).

## <a name="log-in-to-azure"></a>Iniciar sessão no Azure

Inicie sessão na sua subscrição do Azure com o comando `Connect-AzureRmAccount` e siga as instruções no ecrã.

```powershell
Connect-AzureRmAccount
```

## <a name="create-resource-group"></a>Criar grupo de recursos

Crie um grupo de recursos com o seguinte comando:

```powershell
New-AzureRmResourceGroup -Name myResourceGroup -Location westeurope
```

## <a name="create-a-public-ip-standard"></a>Criar um padrão de IP público 
Crie um IP Standard público com o seguinte comando:

```powershell
$publicIp = New-AzureRmPublicIpAddress -ResourceGroupName myResourceGroup -Name 'myPublicIP' `
  -Location westeurope -AllocationMethod Static -Sku Standard 
```

## <a name="create-a-front-end-ip-configuration-for-the-website"></a>Criar uma configuração de IP Front-end para o Web site

Crie uma configuração de IP de front-end com o seguinte comando:

```powershell
$feip = New-AzureRmLoadBalancerFrontendIpConfig -Name 'myFrontEndPool' -PublicIpAddress $publicIp
```

## <a name="create-the-back-end-address-pool"></a>Criar o conjunto de endereços de back-end

Crie um conjunto de endereços de back-end com o seguinte comando:

```powershell
$bepool = New-AzureRmLoadBalancerBackendAddressPoolConfig -Name 'myBackEndPool'
```

## <a name="create-a-load-balancer-probe-on-port-80"></a>Criar uma sonda de Balanceador de carga na porta 80

Crie uma sonda de estado de funcionamento na porta 80 para o Balanceador de carga com o seguinte comando:

```powershell
$probe = New-AzureRmLoadBalancerProbeConfig -Name 'myHealthProbe' -Protocol Http -Port 80 `
  -RequestPath / -IntervalInSeconds 360 -ProbeCount 5
```

## <a name="create-a-load-balancer-rule"></a>Crie uma regra de balanceador de carga
 Crie uma regra de Balanceador de carga com o seguinte comando:

```powershell
   $rule = New-AzureRmLoadBalancerRuleConfig -Name HTTP -FrontendIpConfiguration $feip -BackendAddressPool  $bepool -Probe $probe -Protocol Tcp -FrontendPort 80 -BackendPort 80
```

## <a name="create-a-load-balancer"></a>Criar um balanceador de carga
Crie um balanceador de carga Standard com o seguinte comando:

```powershell
$lb = New-AzureRmLoadBalancer -ResourceGroupName myResourceGroup -Name 'MyLoadBalancer' -Location westeurope `
  -FrontendIpConfiguration $feip -BackendAddressPool $bepool `
  -Probe $probe -LoadBalancingRule $rule -Sku Standard
```

## <a name="next-steps"></a>Passos Seguintes
- Saiba mais sobre [zonas de disponibilidade e o Balanceador de carga Standard](load-balancer-standard-availability-zones.md).



