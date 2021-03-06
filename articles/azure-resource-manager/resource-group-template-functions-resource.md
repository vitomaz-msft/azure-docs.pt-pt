---
title: Funções de modelo do Azure Resource Manager - recursos | Documentos da Microsoft
description: Descreve as funções para utilizar num modelo do Azure Resource Manager para recuperar valores sobre os recursos.
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: ''
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: reference
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/06/2018
ms.author: tomfitz
ms.openlocfilehash: 6da2f7792df564ea3a41df37ab9b00574a205e5b
ms.sourcegitcommit: 1b186301dacfe6ad4aa028cfcd2975f35566d756
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/06/2018
ms.locfileid: "51219550"
---
# <a name="resource-functions-for-azure-resource-manager-templates"></a>Funções de recursos para modelos Azure Resource Manager

O Resource Manager proporciona as seguintes funções para obter valores do recurso:

* [listAccountSas](#list)
* [listKeys](#listkeys)
* [listSecrets](#list)
* [lista *](#list)
* [Fornecedores](#providers)
* [reference](#reference)
* [resourceGroup](#resourcegroup)
* [resourceId](#resourceid)
* [subscrição](#subscription)

Para obter valores de parâmetros, variáveis ou a implementação atual, veja [das funções de valor de implementação](resource-group-template-functions-deployment.md).

<a id="listkeys" />
<a id="list" />

## <a name="listaccountsas-listkeys-listsecrets-and-list"></a>listAccountSas, listKeys, listSecrets e lista *
`listAccountSas(resourceName or resourceIdentifier, apiVersion, functionValues)`

`listKeys(resourceName or resourceIdentifier, apiVersion)`

`listSecrets(resourceName or resourceIdentifier, apiVersion)`

`list{Value}(resourceName or resourceIdentifier, apiVersion)`

Devolve os valores para qualquer tipo de recurso que suporta a operação de lista. As utilizações mais comuns são `listKeys` e `listSecrets`. 

### <a name="parameters"></a>Parâmetros

| Parâmetro | Necessário | Tipo | Descrição |
|:--- |:--- |:--- |:--- |
| resourceName ou resourceIdentifier |Sim |cadeia |Identificador exclusivo para o recurso. |
| apiVersion |Sim |cadeia |Versão de API do Estado de runtime do recurso. Normalmente, no formato, **dd-mm-aaaa**. |
| functionValues |Não |objeto | Um objeto que tem valores para a função. Fornecer apenas este objeto para as funções que suportam a receção de um objeto com valores de parâmetros, tal como **listAccountSas** numa conta de armazenamento. | 

### <a name="return-value"></a>Valor de retorno

O objeto devolvido do listKeys tem o seguinte formato:

```json
{
  "keys": [
    {
      "keyName": "key1",
      "permissions": "Full",
      "value": "{value}"
    },
    {
      "keyName": "key2",
      "permissions": "Full",
      "value": "{value}"
    }
  ]
}
```

Outras funções de lista têm formatos de retornados diferentes. Para ver o formato de uma função, incluí-lo na secção de saídas como mostra o modelo de exemplo. 

### <a name="remarks"></a>Observações

Qualquer operação que começa com **lista** pode ser utilizado como uma função no seu modelo. As operações disponíveis incluem não só listKeys, mas também como o operations `list`, `listAdminKeys`, e `listStatus`. O [SAS de conta da lista](/rest/api/storagerp/storageaccounts#StorageAccounts_ListAccountSAS) operação requer que os parâmetros de corpo de pedido como *signedExpiry*. Para utilizar esta função num modelo, fornece um objeto com o corpo valores de parâmetros.

Para determinar que tipos de recursos tem uma operação de lista, tem as seguintes opções:

* Ver os [operações de REST API](/rest/api/) para um fornecedor de recursos e procure para operações de lista. Por exemplo, contas de armazenamento têm o [listKeys operação](/rest/api/storagerp/storageaccounts#StorageAccounts_ListKeys).
* Utilize o [Get-AzureRmProviderOperation](/powershell/module/azurerm.resources/get-azurermprovideroperation) cmdlet do PowerShell. O exemplo seguinte obtém todas as operações de lista para contas de armazenamento:

  ```powershell
  Get-AzureRmProviderOperation -OperationSearchString "Microsoft.Storage/*" | where {$_.Operation -like "*list*"} | FT Operation
  ```
* Utilize o seguinte comando da CLI do Azure para filtrar apenas as operações de lista:

  ```azurecli
  az provider operation show --namespace Microsoft.Storage --query "resourceTypes[?name=='storageAccounts'].operations[].name | [?contains(@, 'list')]"
  ```

Especifique o recurso com o nome de recurso ou o [resourceId função](#resourceid). Quando utilizar esta função no mesmo modelo que implementa o recurso referenciado, utilize o nome do recurso.

### <a name="example"></a>Exemplo

O seguinte procedimento [modelo de exemplo](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/listkeys.json) mostra como devolver as chaves primária e secundárias da conta de armazenamento na secção de saídas. Ele também retorna um token SAS para a conta de armazenamento. Para obter esse token, ele passa um objeto para a função de listAccountSas. Este exemplo destina-se para mostrar como utilizar as funções de lista. Normalmente, poderia usar o token SAS num valor de recursos em vez de retorná-lo como um valor de saída. Valores de saída são armazenados no histórico de implementação e não são seguras. Tem de especificar uma hora de expiração no futuro para a implementação com êxito.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "storagename": {
            "type": "string"
        },
        "location": {
            "type": "string",
            "defaultValue": "southcentralus"
        },
        "accountSasProperties": {
            "type": "object",
            "defaultValue": {
                "signedServices": "b",
                "signedPermission": "r",
                "signedExpiry": "2018-08-20T11:00:00Z",
                "signedResourceTypes": "s"
            }
        }
    },
    "resources": [
        {
            "apiVersion": "2018-02-01",
            "name": "[parameters('storagename')]",
            "location": "[parameters('location')]",
            "type": "Microsoft.Storage/storageAccounts",
            "sku": {
                "name": "Standard_LRS"
            },
            "kind": "StorageV2",
            "properties": {
                "supportsHttpsTrafficOnly": false,
                "accessTier": "Hot",
                "encryption": {
                    "services": {
                        "blob": {
                            "enabled": true
                        },
                        "file": {
                            "enabled": true
                        }
                    },
                    "keySource": "Microsoft.Storage"
                }
            },
            "dependsOn": []
        }
    ],
    "outputs": {
        "keys": {
            "type": "object",
            "value": "[listKeys(parameters('storagename'), '2018-02-01')]"
        },
        "accountSAS": {
            "type": "object",
            "value": "[listAccountSas(parameters('storagename'), '2018-02-01', parameters('accountSasProperties'))]"
        }
    }
}
``` 

Para implementar este modelo de exemplo com a CLI do Azure, utilize:

```azurecli-interactive
az group deployment create -g functionexamplegroup --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/listkeys.json --parameters storagename=<your-storage-account>
```

Para implementar este modelo de exemplo com o PowerShell, utilize:

```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName functionexamplegroup -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/listkeys.json -storagename <your-storage-account>
```

<a id="providers" />

## <a name="providers"></a>Fornecedores
`providers(providerNamespace, [resourceType])`

Devolve informações sobre um fornecedor de recursos e os tipos de recursos suportados. Se não fornecer um tipo de recurso, a função devolve todos os tipos suportados para o fornecedor de recursos.

### <a name="parameters"></a>Parâmetros

| Parâmetro | Necessário | Tipo | Descrição |
|:--- |:--- |:--- |:--- |
| providerNamespace |Sim |cadeia |Espaço de nomes do fornecedor |
| resourceType |Não |cadeia |O tipo de recurso dentro do espaço de nomes especificado. |

### <a name="return-value"></a>Valor de retorno

Cada tipo suportado é devolvido no seguinte formato: 

```json
{
    "resourceType": "{name of resource type}",
    "locations": [ all supported locations ],
    "apiVersions": [ all supported API versions ]
}
```

Ordenação de matriz de valores devolvidos não é garantido.

### <a name="example"></a>Exemplo

O seguinte procedimento [modelo de exemplo](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/providers.json) mostra como usar a função de fornecedor:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "providerNamespace": {
            "type": "string"
        },
        "resourceType": {
            "type": "string"
        }
    },
    "resources": [],
    "outputs": {
        "providerOutput": {
            "value": "[providers(parameters('providerNamespace'), parameters('resourceType'))]",
            "type" : "object"
        }
    }
}
```

Para o **Microsoft. Web** fornecedor de recursos e **sites** tipo de recurso, o exemplo anterior retorna um objeto no seguinte formato:

```json
{
  "resourceType": "sites",
  "locations": [
    "South Central US",
    "North Europe",
    "West Europe",
    "Southeast Asia",
    ...
  ],
  "apiVersions": [
    "2016-08-01",
    "2016-03-01",
    "2015-08-01-preview",
    "2015-08-01",
    ...
  ]
}
```

Para implementar este modelo de exemplo com a CLI do Azure, utilize:

```azurecli-interactive
az group deployment create -g functionexamplegroup --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/providers.json --parameters providerNamespace=Microsoft.Web resourceType=sites
```

Para implementar este modelo de exemplo com o PowerShell, utilize:

```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName functionexamplegroup -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/providers.json -providerNamespace Microsoft.Web -resourceType sites
```

<a id="reference" />

## <a name="reference"></a>Referência
`reference(resourceName or resourceIdentifier, [apiVersion], ['Full'])`

Devolve um objeto que representa o estado de runtime de um recurso.

### <a name="parameters"></a>Parâmetros

| Parâmetro | Necessário | Tipo | Descrição |
|:--- |:--- |:--- |:--- |
| resourceName ou resourceIdentifier |Sim |cadeia |Nome ou o identificador exclusivo de um recurso. |
| apiVersion |Não |cadeia |Versão de API do recurso especificado. Inclua este parâmetro quando o recurso não está aprovisionado no mesmo modelo. Normalmente, no formato, **dd-mm-aaaa**. |
| "Completo" |Não |cadeia |Valor que especifica se pretende devolver o objeto de recurso completo. Se não especificar `'Full'`, apenas o objeto de propriedades do recurso é retornado. O objeto completo inclui valores, como o ID de recurso e o local. |

### <a name="return-value"></a>Valor de retorno

Cada tipo de recurso devolve as propriedades diferentes para a função de referência. A função não retornará um único formato predefinido. Além disso, o valor retornado é diferente com base no se especificou o objeto completo. Para ver as propriedades de um tipo de recurso, retornar o objeto na secção de saídas, conforme mostrado no exemplo.

### <a name="remarks"></a>Observações

A função de referência é derivada de seu valor de um Estado de tempo de execução e, portanto, não pode ser utilizada na secção de variáveis. Pode ser utilizado na secção de saídas de um modelo ou [modelo ligado](resource-group-linked-templates.md#link-or-nest-a-template). Não é possível utilizar a secção de saídas de um [modelo aninhado](resource-group-linked-templates.md#link-or-nest-a-template). Para devolver os valores para um recurso implementado num modelo aninhado, converta seu modelo aninhado para um modelo ligado. 

Ao utilizar a função de referência, é implicitamente declarar que um recurso depende outro recurso, se o recurso referenciado está aprovisionado no mesmo modelo e consultar o recurso pelo respetivo nome (ID de recurso não). Não precisa de utilizar também a propriedade dependsOn. A função não é avaliada até que o recurso referenciado seja concluída a implementação.

Para ver os nomes de propriedade e os valores para um tipo de recurso, crie um modelo que retorna o objeto na secção de saídas. Se tiver um recurso existente desse tipo, o seu modelo retorna o objeto sem ter de implementar todos os novos recursos. 

Normalmente, utiliza a **referência** função para retornar um valor específico de um objeto, como o ponto final do blob URI ou o nome de domínio completamente qualificado.

```json
"outputs": {
    "BlobUri": {
        "value": "[reference(concat('Microsoft.Storage/storageAccounts/', parameters('storageAccountName')), '2016-01-01').primaryEndpoints.blob]",
        "type" : "string"
    },
    "FQDN": {
        "value": "[reference(concat('Microsoft.Network/publicIPAddresses/', parameters('ipAddressName')), '2016-03-30').dnsSettings.fqdn]",
        "type" : "string"
    }
}
```

Utilize `'Full'` quando necessitar de valores do recurso que não fazem parte do esquema de propriedades. Por exemplo, para definir políticas de acesso do Cofre de chaves, obter as propriedades de identidade para uma máquina virtual.

```json
{
  "type": "Microsoft.KeyVault/vaults",
  "properties": {
    "tenantId": "[reference(concat('Microsoft.Compute/virtualMachines/', variables('vmName')), '2017-03-30', 'Full').identity.tenantId]",
    "accessPolicies": [
      {
        "tenantId": "[reference(concat('Microsoft.Compute/virtualMachines/', variables('vmName')), '2017-03-30', 'Full').identity.tenantId]",
        "objectId": "[reference(concat('Microsoft.Compute/virtualMachines/', variables('vmName')), '2017-03-30', 'Full').identity.principalId]",
        "permissions": {
          "keys": [
            "all"
          ],
          "secrets": [
            "all"
          ]
        }
      }
    ],
    ...
```

Para o exemplo completo do modelo anterior, consulte [Windows para o Key Vault](https://github.com/rjmax/AzureSaturday/blob/master/Demo02.ManagedServiceIdentity/demo08.msiWindowsToKeyvault.json). Um exemplo semelhante está disponível para [Linux](https://github.com/rjmax/AzureSaturday/blob/master/Demo02.ManagedServiceIdentity/demo07.msiLinuxToArm.json).

### <a name="example"></a>Exemplo

O seguinte procedimento [modelo de exemplo](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/referencewithstorage.json) implementa um recurso e faz referência a esse recurso.

```json
{
  "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
      "storageAccountName": { 
          "type": "string"
      }
  },
  "resources": [
    {
      "name": "[parameters('storageAccountName')]",
      "type": "Microsoft.Storage/storageAccounts",
      "apiVersion": "2016-12-01",
      "sku": {
        "name": "Standard_LRS"
      },
      "kind": "Storage",
      "location": "[resourceGroup().location]",
      "tags": {},
      "properties": {
      }
    }
  ],
  "outputs": {
      "referenceOutput": {
          "type": "object",
          "value": "[reference(parameters('storageAccountName'))]"
      },
      "fullReferenceOutput": {
        "type": "object",
        "value": "[reference(parameters('storageAccountName'), '2016-12-01', 'Full')]"
      }
    }
}
``` 

O exemplo anterior devolve os dois objetos. O objeto de propriedades é no seguinte formato:

```json
{
   "creationTime": "2017-10-09T18:55:40.5863736Z",
   "primaryEndpoints": {
     "blob": "https://examplestorage.blob.core.windows.net/",
     "file": "https://examplestorage.file.core.windows.net/",
     "queue": "https://examplestorage.queue.core.windows.net/",
     "table": "https://examplestorage.table.core.windows.net/"
   },
   "primaryLocation": "southcentralus",
   "provisioningState": "Succeeded",
   "statusOfPrimary": "available",
   "supportsHttpsTrafficOnly": false
}
```

O objeto completo é no seguinte formato:

```json
{
  "apiVersion":"2016-12-01",
  "location":"southcentralus",
  "sku": {
    "name":"Standard_LRS",
    "tier":"Standard"
  },
  "tags":{},
  "kind":"Storage",
  "properties": {
    "creationTime":"2017-10-09T18:55:40.5863736Z",
    "primaryEndpoints": {
      "blob":"https://examplestorage.blob.core.windows.net/",
      "file":"https://examplestorage.file.core.windows.net/",
      "queue":"https://examplestorage.queue.core.windows.net/",
      "table":"https://examplestorage.table.core.windows.net/"
    },
    "primaryLocation":"southcentralus",
    "provisioningState":"Succeeded",
    "statusOfPrimary":"available",
    "supportsHttpsTrafficOnly":false
  },
  "subscriptionId":"<subscription-id>",
  "resourceGroupName":"functionexamplegroup",
  "resourceId":"Microsoft.Storage/storageAccounts/examplestorage",
  "referenceApiVersion":"2016-12-01",
  "condition":true,
  "isConditionTrue":true,
  "isTemplateResource":false,
  "isAction":false,
  "provisioningOperation":"Read"
}
```

Para implementar este modelo de exemplo com a CLI do Azure, utilize:

```azurecli-interactive
az group deployment create -g functionexamplegroup --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/referencewithstorage.json --parameters storageAccountName=<your-storage-account>
```

Para implementar este modelo de exemplo com o PowerShell, utilize:

```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName functionexamplegroup -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/referencewithstorage.json -storageAccountName <your-storage-account>
```

O seguinte procedimento [modelo de exemplo](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/reference.json) faz referência a uma conta de armazenamento que não está implementada neste modelo. A conta de armazenamento já existe no mesmo grupo de recursos.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "storageAccountName": {
            "type": "string"
        }
    },
    "resources": [],
    "outputs": {
        "ExistingStorage": {
            "value": "[reference(concat('Microsoft.Storage/storageAccounts/', parameters('storageAccountName')), '2016-01-01')]",
            "type" : "object"
        }
    }
}
```

Para implementar este modelo de exemplo com a CLI do Azure, utilize:

```azurecli-interactive
az group deployment create -g functionexamplegroup --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/reference.json --parameters storageAccountName=<your-storage-account>
```

Para implementar este modelo de exemplo com o PowerShell, utilize:

```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName functionexamplegroup -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/reference.json -storageAccountName <your-storage-account>
```

<a id="resourcegroup" />

## <a name="resourcegroup"></a>resourceGroup
`resourceGroup()`

Devolve um objeto que representa o grupo de recursos atual. 

### <a name="return-value"></a>Valor de retorno

O objeto devolvido é no seguinte formato:

```json
{
  "id": "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}",
  "name": "{resourceGroupName}",
  "location": "{resourceGroupLocation}",
  "tags": {
  },
  "properties": {
    "provisioningState": "{status}"
  }
}
```

### <a name="remarks"></a>Observações

Uma utilização comum da função resourceGroup é criar recursos na mesma localização que o grupo de recursos. O exemplo seguinte utiliza a localização do grupo de recursos para atribuir a localização para um web site.

```json
"resources": [
   {
      "apiVersion": "2016-08-01",
      "type": "Microsoft.Web/sites",
      "name": "[parameters('siteName')]",
      "location": "[resourceGroup().location]",
      ...
   }
]
```

### <a name="example"></a>Exemplo

O seguinte procedimento [modelo de exemplo](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/resourcegroup.json) devolve as propriedades do grupo de recursos.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [],
    "outputs": {
        "resourceGroupOutput": {
            "value": "[resourceGroup()]",
            "type" : "object"
        }
    }
}
```

O exemplo anterior retorna um objeto no seguinte formato:

```json
{
  "id": "/subscriptions/{subscription-id}/resourceGroups/examplegroup",
  "name": "examplegroup",
  "location": "southcentralus",
  "properties": {
    "provisioningState": "Succeeded"
  }
}
```

Para implementar este modelo de exemplo com a CLI do Azure, utilize:

```azurecli-interactive
az group deployment create -g functionexamplegroup --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/resourcegroup.json
```

Para implementar este modelo de exemplo com o PowerShell, utilize:

```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName functionexamplegroup -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/resourcegroup.json 
```

<a id="resourceid" />

## <a name="resourceid"></a>resourceId
`resourceId([subscriptionId], [resourceGroupName], resourceType, resourceName1, [resourceName2]...)`

Devolve o identificador exclusivo de um recurso. Utilize esta função quando o nome do recurso é ambíguo ou não aprovisionada dentro do mesmo modelo. 

### <a name="parameters"></a>Parâmetros

| Parâmetro | Necessário | Tipo | Descrição |
|:--- |:--- |:--- |:--- |
| subscriptionId |Não |a cadeia de caracteres (formato de GUID) |Valor predefinido é a subscrição atual. Especifica este valor quando precisar de recuperar um recurso noutra subscrição. |
| resourceGroupName |Não |cadeia |Valor predefinido é o grupo de recursos atual. Especifica este valor quando precisar de recuperar um recurso noutro grupo de recursos. |
| resourceType |Sim |cadeia |Tipo de recurso, incluindo o espaço de nomes de fornecedor de recursos. |
| resourceName1 |Sim |cadeia |Nome do recurso. |
| resourceName2 |Não |cadeia |Recurso nome segmento seguinte se o recurso está aninhado. |

### <a name="return-value"></a>Valor de retorno

O identificador é devolvido no seguinte formato:

```json
/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/{resourceProviderNamespace}/{resourceType}/{resourceName}
```

### <a name="remarks"></a>Observações

Os valores de parâmetro que especifica dependem se o recurso é o mesmo grupo de recursos e subscrição como a implementação atual.

Para obter o ID de recurso para uma conta de armazenamento na mesma subscrição e grupo de recursos, utilize:

```json
"[resourceId('Microsoft.Storage/storageAccounts','examplestorage')]"
```

Para obter o ID de recurso para uma conta de armazenamento na mesma subscrição, mas um grupo de recursos diferente, utilize:

```json
"[resourceId('otherResourceGroup', 'Microsoft.Storage/storageAccounts','examplestorage')]"
```

Para obter o ID de recurso para uma conta de armazenamento numa subscrição diferente e grupo de recursos, utilize:

```json
"[resourceId('xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx', 'otherResourceGroup', 'Microsoft.Storage/storageAccounts','examplestorage')]"
```

Para obter o ID de recurso para uma base de dados no grupo de recursos diferente, utilize:

```json
"[resourceId('otherResourceGroup', 'Microsoft.SQL/servers/databases', parameters('serverName'), parameters('databaseName'))]"
```

Muitas vezes, terá de utilizar esta função quando utilizar uma conta de armazenamento ou a rede virtual num grupo de recursos alternativos. O exemplo seguinte mostra como um recurso de um grupo de recurso externo pode ser facilmente utilizado:

```json
{
  "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
      "virtualNetworkName": {
          "type": "string"
      },
      "virtualNetworkResourceGroup": {
          "type": "string"
      },
      "subnet1Name": {
          "type": "string"
      },
      "nicName": {
          "type": "string"
      }
  },
  "variables": {
      "vnetID": "[resourceId(parameters('virtualNetworkResourceGroup'), 'Microsoft.Network/virtualNetworks', parameters('virtualNetworkName'))]",
      "subnet1Ref": "[concat(variables('vnetID'),'/subnets/', parameters('subnet1Name'))]"
  },
  "resources": [
  {
      "apiVersion": "2015-05-01-preview",
      "type": "Microsoft.Network/networkInterfaces",
      "name": "[parameters('nicName')]",
      "location": "[parameters('location')]",
      "properties": {
          "ipConfigurations": [{
              "name": "ipconfig1",
              "properties": {
                  "privateIPAllocationMethod": "Dynamic",
                  "subnet": {
                      "id": "[variables('subnet1Ref')]"
                  }
              }
          }]
       }
  }]
}
```

### <a name="example"></a>Exemplo

O seguinte procedimento [modelo de exemplo](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/resourceid.json) devolve o ID de recurso para uma conta de armazenamento no grupo de recursos:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [],
    "outputs": {
        "sameRGOutput": {
            "value": "[resourceId('Microsoft.Storage/storageAccounts','examplestorage')]",
            "type" : "string"
        },
        "differentRGOutput": {
            "value": "[resourceId('otherResourceGroup', 'Microsoft.Storage/storageAccounts','examplestorage')]",
            "type" : "string"
        },
        "differentSubOutput": {
            "value": "[resourceId('11111111-1111-1111-1111-111111111111', 'otherResourceGroup', 'Microsoft.Storage/storageAccounts','examplestorage')]",
            "type" : "string"
        },
        "nestedResourceOutput": {
            "value": "[resourceId('Microsoft.SQL/servers/databases', 'serverName', 'databaseName')]",
            "type" : "string"
        }
    }
}
```

O resultado do exemplo anterior com os valores predefinidos é:

| Nome | Tipo | Valor |
| ---- | ---- | ----- |
| sameRGOutput | Cadeia | /subscriptions/{current-sub-id}/resourceGroups/examplegroup/providers/Microsoft.Storage/storageAccounts/examplestorage |
| differentRGOutput | Cadeia | /subscriptions/{current-sub-id}/resourceGroups/otherResourceGroup/providers/Microsoft.Storage/storageAccounts/examplestorage |
| differentSubOutput | Cadeia | /subscriptions/11111111-1111-1111-1111-111111111111/resourceGroups/otherResourceGroup/providers/Microsoft.Storage/storageAccounts/examplestorage |
| nestedResourceOutput | Cadeia | /subscriptions/{current-sub-id}/resourceGroups/examplegroup/providers/Microsoft.SQL/servers/serverName/databases/databaseName |

Para implementar este modelo de exemplo com a CLI do Azure, utilize:

```azurecli-interactive
az group deployment create -g functionexamplegroup --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/resourceid.json
```

Para implementar este modelo de exemplo com o PowerShell, utilize:

```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName functionexamplegroup -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/resourceid.json 
```

<a id="subscription" />

## <a name="subscription"></a>subscrição
`subscription()`

Devolve detalhes sobre a subscrição para a implementação atual. 

### <a name="return-value"></a>Valor de retorno

A função devolve o seguinte formato:

```json
{
    "id": "/subscriptions/{subscription-id}",
    "subscriptionId": "{subscription-id}",
    "tenantId": "{tenant-id}",
    "displayName": "{name-of-subscription}"
}
```

### <a name="example"></a>Exemplo

O seguinte procedimento [modelo de exemplo](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/subscription.json) mostra a função de subscrição chamada na secção de saídas. 

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [],
    "outputs": {
        "subscriptionOutput": {
            "value": "[subscription()]",
            "type" : "object"
        }
    }
}
```

Para implementar este modelo de exemplo com a CLI do Azure, utilize:

```azurecli-interactive
az group deployment create -g functionexamplegroup --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/subscription.json
```

Para implementar este modelo de exemplo com o PowerShell, utilize:

```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName functionexamplegroup -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/subscription.json 
```

## <a name="next-steps"></a>Passos Seguintes
* Para obter uma descrição das secções num modelo Azure Resource Manager, consulte [modelos Authoring Azure Resource Manager](resource-group-authoring-templates.md).
* Para intercalar vários modelos, veja [utilizar modelos ligados com o Azure Resource Manager](resource-group-linked-templates.md).
* Para fazer a iteração de um número especificado de vezes ao criar um tipo de recurso, consulte [criar várias instâncias de recursos no Azure Resource Manager](resource-group-create-multiple.md).
* Para ver como implementar o modelo que criou, veja [implementar uma aplicação com o modelo Azure Resource Manager](resource-group-template-deploy.md).

