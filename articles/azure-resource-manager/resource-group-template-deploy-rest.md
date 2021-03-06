---
title: Implementar recursos com a REST API e o modelo | Documentos da Microsoft
description: Utilize o Azure Resource Manager e API de REST do Resource Manager para implementar um recursos no Azure. Os recursos são definidos num modelo do Resource Manager.
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
ms.assetid: 1d8fbd4c-78b0-425b-ba76-f2b7fd260b45
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/26/2018
ms.author: tomfitz
ms.openlocfilehash: 058d6d398f6bb54e8569e727f118a325c338049d
ms.sourcegitcommit: 48592dd2827c6f6f05455c56e8f600882adb80dc
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/26/2018
ms.locfileid: "50154747"
---
# <a name="deploy-resources-with-resource-manager-templates-and-resource-manager-rest-api"></a>Implementar recursos com modelos do Resource Manager e API REST do Resource Manager

Este artigo explica como utilizar a API de REST do Resource Manager com modelos do Resource Manager para implementar os seus recursos no Azure.  

> [!TIP]
> Para obter ajuda com a depuração de um erro durante a implementação, consulte:
> 
> * [Ver as operações de implementação](resource-manager-deployment-operations.md) para saber mais sobre a obtenção de informações de que o ajuda a resolver problemas relacionados com o erro
> * [Resolver erros comuns ao implementar recursos no Azure com o Azure Resource Manager](resource-manager-common-deployment-errors.md) para saber como resolver erros de implementação comuns
> 
> 

Pode optar por incluir o modelo no corpo do pedido ou ligação para um ficheiro. Quando utilizar um ficheiro, pode ser um ficheiro local ou um arquivo externo que está disponível através de um URI. Quando o modelo estiver numa conta de armazenamento, pode restringir o acesso ao modelo e fornecer um token de assinatura (SAS) de acesso partilhado durante a implementação.

## <a name="deploy-with-the-rest-api"></a>Implementar com a API REST
1. Definir [parâmetros comuns e os cabeçalhos](/rest/api/azure/), incluindo os tokens de autenticação.

1. Se não tiver um grupo de recursos existente, crie um grupo de recursos. Forneça o ID da subscrição, o nome do novo grupo de recursos e localização que precisa para a sua solução. Para obter mais informações, consulte [criar um grupo de recursos](/rest/api/resources/resourcegroups/createorupdate).

  ```HTTP
  PUT https://management.azure.com/subscriptions/<YourSubscriptionId>/resourcegroups/<YourResourceGroupName>?api-version=2018-05-01
  ```

  Com um corpo de pedido, como:
  ```json
  {
    "location": "West US",
    "tags": {
      "tagname1": "tagvalue1"
    }
  }
  ```

1. Validar implementação antes de executá-lo ao executar o [validar uma implementação de modelo](/rest/api/resources/deployments/validate) operação. Ao testar a implementação, fornece os parâmetros exatamente como faria ao executar a implementação (mostrada na próxima etapa).

1. Crie uma implementação. Forneça o seu ID de subscrição, o nome do grupo de recursos, o nome da implantação e uma ligação ao seu modelo. Para obter informações sobre o ficheiro de modelo, consulte [ficheiro de parâmetros](#parameter-file). Para obter mais informações sobre a API de REST para criar um grupo de recursos, consulte [criar uma implementação de modelo](/rest/api/resources/deployments/createorupdate). Observe que o **modo** está definida como **Incremental**. Para executar uma implantação completa, definida **modo** ao **concluída**. Tenha cuidado ao usar o modo de conclusão, como inadvertidamente pode eliminar recursos que não estão no seu modelo.

  ```HTTP
  PUT https://management.azure.com/subscriptions/<YourSubscriptionId>/resourcegroups/<YourResourceGroupName>/providers/Microsoft.Resources/deployments/<YourDeploymentName>?api-version=2018-05-01
  ```

  Com um corpo de pedido, como:

   ```json
  {
    "properties": {
      "templateLink": {
        "uri": "http://mystorageaccount.blob.core.windows.net/templates/template.json",
        "contentVersion": "1.0.0.0"
      },
      "mode": "Incremental",
      "parametersLink": {
        "uri": "http://mystorageaccount.blob.core.windows.net/templates/parameters.json",
        "contentVersion": "1.0.0.0"
      }
    }
  }
  ```

    Se quiser iniciar conteúdo de resposta, o conteúdo do pedido ou ambos, inclua **debugSetting** no pedido.

  ```json
  "debugSetting": {
    "detailLevel": "requestContent, responseContent"
  }
  ```

    Pode configurar a sua conta de armazenamento para utilizar um token de assinatura (SAS) de acesso partilhado. Para obter mais informações, consulte [delegar acesso com uma assinatura de acesso partilhado](https://docs.microsoft.com/rest/api/storageservices/delegating-access-with-a-shared-access-signature).

1. Em vez de uma ligação para ficheiros de modelo e parâmetros, pode incluí-los no corpo do pedido.

  ```json
  {
      "properties": {
      "mode": "Incremental",
      "template": {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
          "storageAccountType": {
            "type": "string",
            "defaultValue": "Standard_LRS",
            "allowedValues": [
              "Standard_LRS",
              "Standard_GRS",
              "Standard_ZRS",
              "Premium_LRS"
            ],
            "metadata": {
              "description": "Storage Account type"
            }
          },
          "location": {
            "type": "string",
            "defaultValue": "[resourceGroup().location]",
            "metadata": {
              "description": "Location for all resources."
            }
          }
        },
        "variables": {
          "storageAccountName": "[concat(uniquestring(resourceGroup().id), 'standardsa')]"
        },
        "resources": [
          {
            "type": "Microsoft.Storage/storageAccounts",
            "name": "[variables('storageAccountName')]",
            "apiVersion": "2018-02-01",
            "location": "[parameters('location')]",
            "sku": {
              "name": "[parameters('storageAccountType')]"
            },
            "kind": "StorageV2",
            "properties": {}
          }
        ],
        "outputs": {
          "storageAccountName": {
            "type": "string",
            "value": "[variables('storageAccountName')]"
          }
        }
      },
      "parameters": {
        "location": {
          "value": "eastus2"
        }
      }
    }
  }
  ```

5. Obtenha o estado da implementação do modelo. Para obter mais informações, consulte [obter informações sobre uma implementação de modelo](/rest/api/resources/deployments/get).

  ```HTTP
  GET https://management.azure.com/subscriptions/<YourSubscriptionId>/resourcegroups/<YourResourceGroupName>/providers/Microsoft.Resources/deployments/<YourDeploymentName>?api-version=2018-05-01
  ```

## <a name="redeploy-when-deployment-fails"></a>Implementar novamente quando ocorre uma falha de implementação

Para implementações que falharam, pode especificar que uma implementação anterior do seu histórico de implementação é automaticamente implementada novamente. Para utilizar esta opção, as suas implementações tem de ter nomes exclusivos para que eles podem ser identificados na história. Se não tem nomes exclusivos, a falha na implementação atual poderá substituir a implementação bem-sucedida anterior na história. Só pode utilizar esta opção com implementações de nível de raiz. Implementações a partir de um modelo aninhado não estão disponíveis para reimplantação.

Para Reimplementar a última implementação efetuada com êxito se falhar a implementação atual, utilize:

```json
"onErrorDeployment": {
  "type": "LastSuccessful",
},
```

Para Reimplementar uma implementação específica, se falhar a implementação atual, utilize:

```json
"onErrorDeployment": {
  "type": "SpecificDeployment",
  "deploymentName": "<deploymentname>"
}
```

A implementação especificada tem de ter foi concluída com êxito.

## <a name="parameter-file"></a>Ficheiro de parâmetros

Se utilizar um ficheiro de parâmetros para transmitir valores de parâmetro durante a implementação, terá de criar um ficheiro JSON com um formato semelhante ao seguinte exemplo:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "webSiteName": {
            "value": "ExampleSite"
        },
        "webSiteHostingPlanName": {
            "value": "DefaultPlan"
        },
        "webSiteLocation": {
            "value": "West US"
        },
        "adminPassword": {
            "reference": {
               "keyVault": {
                  "id": "/subscriptions/{guid}/resourceGroups/{group-name}/providers/Microsoft.KeyVault/vaults/{vault-name}"
               }, 
               "secretName": "sqlAdminPassword" 
            }   
        }
   }
}
```

O tamanho do ficheiro de parâmetro não pode ser mais do que 64 KB.

Se precisar de fornecer um valor sensível para um parâmetro (por exemplo, uma palavra-passe), adicione esse valor para um cofre de chaves. Obter o Cofre de chaves durante a implementação, conforme mostrado no exemplo anterior. Para obter mais informações, consulte [transmitir valores seguros durante a implementação](resource-manager-keyvault-parameter.md). 

## <a name="next-steps"></a>Passos Seguintes
* Para especificar como lidar com recursos de que existem no grupo de recursos, mas não estão definidos no modelo, consulte [modos de implementação Azure Resource Manager](deployment-modes.md).
* Para saber mais sobre como lidar com operações assíncronas de REST, veja [monitorizar operações assíncronas de Azure](resource-manager-async-operations.md).
* Para obter um exemplo de implementação de recursos por meio da biblioteca de cliente .NET, consulte [implementar recursos com bibliotecas .NET e um modelo](../virtual-machines/windows/csharp-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
* Para definir parâmetros no modelo, veja [criação de modelos](resource-group-authoring-templates.md#parameters).
* Para obter documentação de orientação sobre como as empresas podem utilizar o Resource Manager para gerir subscrições de forma eficaz, consulte [Azure enterprise scaffold - prescriptive subscription governance (Andaime empresarial do Azure - governação de subscrições prescritivas)](/azure/architecture/cloud-adoption-guide/subscription-governance).

