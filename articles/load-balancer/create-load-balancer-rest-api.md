---
title: Criar um balanceador de carga do Azure com a REST API | Documentos da Microsoft
description: Saiba como criar um balanceador de carga do Azure com a REST API.
services: load-balancer
documentationcenter: na
author: KumudD
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: load-balancer
ms.date: 06/06/2018
ms.author: kumud
ms.openlocfilehash: 2f5d3584a429ce35e6905b14bb412c3782fb0185
ms.sourcegitcommit: 6135cd9a0dae9755c5ec33b8201ba3e0d5f7b5a1
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/31/2018
ms.locfileid: "50416924"
---
# <a name="create-an-azure-basic-load-balancer-using-rest-api"></a>Criar um balanceador de carga básico do Azure com a REST API

Um balanceador de carga do Azure distribui novos fluxos de entrada que chegam no front-end de Balanceador de carga para as instâncias de conjunto de back-end, de acordo com as regras e sondas de estado de funcionamento. O Balanceador de carga está disponível em dois SKUs: básico e Standard. Compreender a diferença entre as duas versões SKU, [comparações de SKU do Balanceador de carga](load-balancer-overview.md#skus).
 
Nesta explicação de procedimento mostra como criar um balanceador de carga básico do Azure, utilizando [API REST do Azure](/rest/api/azure/) para ajudar a solicitação de entrada do balanceamento de carga em várias VMs dentro de uma rede virtual do Azure. Estão disponíveis na documentação de referência completa e exemplos adicionais do [referência de REST de Balanceador de carga do Azure](/rest/api/load-balancer/).
 
## <a name="build-the-request"></a>Criar o pedido
Utilize o seguinte pedido HTTP PUT para criar um novo Azure Balanceador de carga básico.
 ```HTTP
  PUT https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Network/loadBalancers/{loadBalancerName}?api-version=2018-02-01
  ```
### <a name="uri-parameters"></a>Parâmetros do URI

|Nome  |Em  |Necessário |Tipo |Descrição |
|---------|---------|---------|---------|--------|
|subscriptionId   |  caminho       |  Verdadeiro       |   cadeia      |  As credenciais de subscrição que identificam de forma exclusiva a subscrição do Microsoft Azure. O ID de subscrição faz parte do URI a cada chamada de serviço.      |
|resourceGroupName     |     caminho    | Verdadeiro        |  cadeia       |   O nome do grupo de recursos.     |
|loadBalancerName     |  caminho       |      Verdadeiro   |    cadeia     |    O nome do Balanceador de carga.    |
|versão de API    |   consulta     |  Verdadeiro       |     cadeia    |  Versão de API do cliente.      |



### <a name="request-body"></a>Corpo do pedido

O único parâmetro exigido é `location`. Se não definir o *SKU* versão, um balanceador de carga básico é criado por predefinição.  Uso [parâmetros opcionais](https://docs.microsoft.com/rest/api/load-balancer/loadbalancers/createorupdate#request-body) para personalizar o Balanceador de carga.

| Nome | Tipo | Descrição |
| :--- | :--- | :---------- |
| localização | cadeia | Localização do recurso. Obter uma lista atual de localizações com o [lista de localizações](https://docs.microsoft.com/rest/api/resources/subscriptions/listlocations) operação. |


## <a name="example-create-and-update-a-basic-load-balancer"></a>Exemplo: Criar e atualizar um balanceador de carga básico

Neste exemplo, primeiro crie um balanceador de carga básico, juntamente com os respetivos recursos. Em seguida, configure os recursos do Balanceador de carga que incluem uma configuração de IP de front-end, um conjunto de endereços de back-end, uma regra, uma sonda de estado de funcionamento e regras NAT de entrada de balanceamento de carga.

Antes de criar um balanceador de carga com o exemplo abaixo, criar uma rede virtual denominada *vnetlb* com uma sub-rede denominada *subnetlb* num grupo de recursos com o nome *rg1* no **E.U.A. Leste** localização.

### <a name="step-1-create-a-basic-load-balancer"></a>PASSO 1. Criar um Balanceador de Carga Básico
Neste passo, vai criar um balanceador de carga básico, com o nome *lb* no **E.U.A. Leste** local dentro da *rg1* grupo de recursos.
#### <a name="sample-request"></a>Pedido de exemplo

  ```HTTP    
  PUT https://management.azure.com/subscriptions/subid/resourceGroups/rg1/providers/Microsoft.Network/loadBalancers/lb?api-version=2018-02-01
  ```
#### <a name="request-body"></a>Corpo do pedido

  ```JSON
   {
    "location": "eastus",
   }
  ```
### <a name="step-2-configure-load-balancer-resources"></a>PASSO 2. Configurar recursos do Balanceador de carga
Neste passo, vai configurar o Balanceador de carga *lb* recursos que incluem uma configuração de IP de front-end (*fe-lb*), um conjunto de endereços de back-end (*lb ser*), um (regra de balanceamento de carga *rulelb*), uma sonda de estado de funcionamento (*sonda-lb*) e uma regra NAT de entrada (*no-nat-rule*).
#### <a name="sample-request"></a>Pedido de exemplo

  ```HTTP    
  PUT https://management.azure.com/subscriptions/subid/resourceGroups/rg1/providers/Microsoft.Network/loadBalancers/lb?api-version=2018-02-01
  ```
#### <a name="request-body"></a>Corpo do pedido

  ```JSON
{
  "properties": {
    "frontendIPConfigurations": [
      {
        "name": "fe-lb",
        "properties": {
          "subnet": {
            "id": "/subscriptions/subid/resourceGroups/rg1/providers/Microsoft.Network/virtualNetworks/vnetlb/subnets/subnetlb"
          },
          "loadBalancingRules": [
            {
              "id": "/subscriptions/subid/resourceGroups/rg1/providers/Microsoft.Network/loadBalancers/lb/loadBalancingRules/rulelb"
            }  ],
          "inboundNatRules": [
            {
              "id": "/subscriptions/subid/resourceGroups/rg1/providers/Microsoft.Network/loadBalancers/lb/inboundNatRules/in-nat-rule"
            }  ]  }  }  ],
    "backendAddressPools": [
      {
        "name": "be-lb",
        "properties": {
          "loadBalancingRules": [
            {
              "id": "/subscriptions/subid/resourceGroups/rg1/providers/Microsoft.Network/loadBalancers/lb/loadBalancingRules/rulelb"
            }  ]   }   }   ],
    "loadBalancingRules": [
      {
        "name": "rulelb",
        "properties": {
          "frontendIPConfiguration": {
            "id": "/subscriptions/subid/resourceGroups/rg1/providers/Microsoft.Network/loadBalancers/lb/frontendIPConfigurations/fe-lb"
          },
          "frontendPort": 80,
          "backendPort": 80,
          "enableFloatingIP": true,
          "idleTimeoutInMinutes": 15,
          "protocol": "Tcp",
          "loadDistribution": "Default",
          "backendAddressPool": {
            "id": "/subscriptions/subid/resourceGroups/rg1/providers/Microsoft.Network/loadBalancers/lb/backendAddressPools/be-lb"
          },
          "probe": {
            "id": "/subscriptions/subid/resourceGroups/rg1/providers/Microsoft.Network/loadBalancers/lb/probes/probe-lb"
          }  }  }  ],
    "probes": [
      {
        "name": "probe-lb",
        "properties": {
          "protocol": "Http",
          "port": 80,
          "requestPath": "healthcheck.aspx",
          "intervalInSeconds": 15,
          "numberOfProbes": 2,
          "loadBalancingRules": [
            {
              "id": "/subscriptions/subid/resourceGroups/rg1/providers/Microsoft.Network/loadBalancers/lb/loadBalancingRules/rulelb"
            }  ]  }  } ],
    "inboundNatRules": [
      {
        "name": "in-nat-rule",
        "properties": {
          "frontendIPConfiguration": {
            "id": "/subscriptions/subid/resourceGroups/rg1/providers/Microsoft.Network/loadBalancers/lb/frontendIPConfigurations/fe-lb"
          },
          "frontendPort": 3389,
          "backendPort": 3389,
          "enableFloatingIP": true,
          "idleTimeoutInMinutes": 15,
          "protocol": "Tcp"
        } } ],
    "inboundNatPools": [],
    "outboundNatRules": []
  }  }
```
