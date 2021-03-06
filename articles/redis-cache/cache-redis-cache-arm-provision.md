---
title: Aprovisionar uma Cache de Redis com o Azure Resource Manager | Documentos da Microsoft
description: Utilize o modelo Azure Resource Manager para implementar uma Cache de Redis do Azure.
services: app-service
documentationcenter: ''
author: wesmc7777
manager: cfowler
editor: ''
ms.assetid: ce6f5372-7038-4655-b1c5-108f7c148282
ms.service: cache
ms.workload: web
ms.tgt_pltfrm: cache-redis
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: wesmc
ms.openlocfilehash: c4c95218ca88ce1fe832fda4fabaf957fdd69dc1
ms.sourcegitcommit: 0f54b9dbcf82346417ad69cbef266bc7804a5f0e
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/26/2018
ms.locfileid: "50140371"
---
# <a name="create-a-redis-cache-using-a-template"></a>Criar uma Cache de Redis com um modelo
Neste tópico, saiba como criar um modelo do Azure Resource Manager que implementa uma Cache de Redis do Azure. O cache pode ser utilizado com uma conta de armazenamento existente para manter os dados de diagnóstico. Também aprenderá como definir quais recursos são implementados e como definir os parâmetros que são especificados quando a implementação é executada. Pode utilizar este modelo para as suas próprias implementações ou personalizá-lo para satisfazer as suas necessidades.

Atualmente, as definições de diagnóstico são partilhadas para todos os caches na mesma região para uma subscrição. A atualizar uma cache na região afeta todos os outros caches na região.

Para obter mais informações sobre a criação de modelos, consulte [criação de modelos do Azure Resource Manager](../azure-resource-manager/resource-group-authoring-templates.md).

Para o modelo completo, consulte [modelo de Cache de Redis](https://github.com/Azure/azure-quickstart-templates/blob/master/101-redis-cache/azuredeploy.json).

> [!NOTE]
> Modelos do Resource Manager para o novo [escalão Premium](cache-premium-tier-intro.md) estão disponíveis. 
> 
> * [Criar uma Cache de Redis Premium com clustering](https://azure.microsoft.com/documentation/templates/201-redis-premium-cluster-diagnostics/)
> * [Criar a Cache de Redis Premium com persistência de dados](https://azure.microsoft.com/documentation/templates/201-redis-premium-persistence/)
> * [Criar a Cache de Redis Premium com VNet e o clustering opcional](https://azure.microsoft.com/documentation/templates/201-redis-premium-vnet-cluster-diagnostics/)
> 
> Para verificar os modelos mais recentes, consulte [modelos de início rápido do Azure](https://azure.microsoft.com/documentation/templates/) e procure `Redis Cache`.
> 
> 

## <a name="what-you-will-deploy"></a>O que irá implementar
Neste modelo, irá implementar uma Cache de Redis do Azure que utiliza uma conta de armazenamento existente para dados de diagnóstico.

Para executar automaticamente a implementação, clique no seguinte botão:

[![Implementar no Azure](./media/cache-redis-cache-arm-provision/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-redis-cache%2Fazuredeploy.json)

## <a name="parameters"></a>Parâmetros
Com o Azure Resource Manager, define parâmetros para os valores que pretende especificar quando o modelo é implementado. O modelo inclui uma seção chamada parâmetros, que contém todos os valores de parâmetro.
Deverá definir um parâmetro para esses valores que variam com base no projeto ou no ambiente que está a implementar. Não defina parâmetros para valores que permanecem sempre iguais. Cada valor de parâmetro é utilizado no modelo para definir os recursos que são implementados. 

[!INCLUDE [app-service-web-deploy-redis-parameters](../../includes/cache-deploy-parameters.md)]

### <a name="rediscachelocation"></a>redisCacheLocation
A localização da Cache de Redis. Para um melhor desempenho, utilize a mesma localização que a aplicação a ser utilizado com a cache.

    "redisCacheLocation": {
      "type": "string"
    }

### <a name="existingdiagnosticsstorageaccountname"></a>existingDiagnosticsStorageAccountName
O nome da conta de armazenamento existente para utilizar para obter um diagnóstico. 

    "existingDiagnosticsStorageAccountName": {
      "type": "string"
    }

### <a name="enablenonsslport"></a>enableNonSslPort
Um valor booleano que indica se deve permitir o acesso através de portas não SSL.

    "enableNonSslPort": {
      "type": "bool"
    }

### <a name="diagnosticsstatus"></a>diagnosticsStatus
Um valor que indica se o diagnóstico está ativado. Utilize ou ativar ou DESATIVAR.

    "diagnosticsStatus": {
      "type": "string",
      "defaultValue": "ON",
      "allowedValues": [
            "ON",
            "OFF"
        ]
    }

## <a name="resources-to-deploy"></a>Recursos a implementar
### <a name="redis-cache"></a>Cache de Redis
Cria a Cache de Redis do Azure.

    {
      "apiVersion": "2015-08-01",
      "name": "[parameters('redisCacheName')]",
      "type": "Microsoft.Cache/Redis",
      "location": "[parameters('redisCacheLocation')]",
      "properties": {
        "enableNonSslPort": "[parameters('enableNonSslPort')]",
        "sku": {
          "capacity": "[parameters('redisCacheCapacity')]",
          "family": "[parameters('redisCacheFamily')]",
          "name": "[parameters('redisCacheSKU')]"
        }
      },
      "resources": [
        {
          "apiVersion": "2017-05-01-preview",
          "type": "Microsoft.Cache/redis/providers/diagnosticsettings",
          "name": "[concat(parameters('redisCacheName'), '/Microsoft.Insights/service')]",
          "location": "[parameters('redisCacheLocation')]",
          "dependsOn": [
            "[concat('Microsoft.Cache/Redis/', parameters('redisCacheName'))]"
          ],
          "properties": {
            "status": "[parameters('diagnosticsStatus')]",
            "storageAccountName": "[parameters('existingDiagnosticsStorageAccountName')]"
          }
        }
      ]
    }



## <a name="commands-to-run-deployment"></a>Comandos para executar a implementação
[!INCLUDE [app-service-deploy-commands](../../includes/app-service-deploy-commands.md)]

### <a name="powershell"></a>PowerShell
    New-AzureRmResourceGroupDeployment -TemplateUri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-redis-cache/azuredeploy.json -ResourceGroupName ExampleDeployGroup -redisCacheName ExampleCache

### <a name="azure-cli"></a>CLI do Azure
    azure group deployment create --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-redis-cache/azuredeploy.json -g ExampleDeployGroup


