---
title: Secção de parâmetro de modelo do Azure Resource Manager | Documentos da Microsoft
description: Descreve a secção de parâmetros de modelos Azure Resource Manager usando sintaxe declarativa do JSON.
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/30/2018
ms.author: tomfitz
ms.openlocfilehash: 83ba1b94413990c0eb8dff42c49d46456a658d5a
ms.sourcegitcommit: 6135cd9a0dae9755c5ec33b8201ba3e0d5f7b5a1
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/31/2018
ms.locfileid: "50417774"
---
# <a name="parameters-section-of-azure-resource-manager-templates"></a>Secção de parâmetros de modelos Azure Resource Manager
Na secção de parâmetros do modelo, especifique os valores que pode inserir ao implementar os recursos. Estes valores de parâmetros permitem-lhe personalizar a implementação, fornecendo valores que são adaptadas para um ambiente específico (por exemplo, desenvolvimento, teste e produção). Não é necessário fornecer os parâmetros no seu modelo, mas sem parâmetros seu modelo implementaria sempre os mesmos recursos com os mesmos nomes, locais e propriedades.

Está limitado a 256 parâmetros num modelo. Pode reduzir o número de parâmetros com objetos que contêm várias propriedades, conforme mostrado neste artigo.

## <a name="define-and-use-a-parameter"></a>Definir e utilizar um parâmetro

O exemplo seguinte mostra uma definição de parâmetro simples. Ele define o nome do parâmetro e especifica que demora um valor de cadeia de caracteres. O parâmetro só aceita valores que façam sentido para seu uso pretendido. Especifica um valor predefinido quando é fornecido nenhum valor durante a implementação. Por fim, o parâmetro inclui uma descrição de seu uso. 

```json
"parameters": {
  "storageSKU": {
    "type": "string",
    "allowedValues": [
      "Standard_LRS",
      "Standard_ZRS",
      "Standard_GRS",
      "Standard_RAGRS",
      "Premium_LRS"
    ],
    "defaultValue": "Standard_LRS",
    "metadata": {
      "description": "The type of replication to use for the storage account."
    }
  }   
}
```

No modelo, referenciar o valor para o parâmetro com a seguinte sintaxe:

```json
"resources": [
  {
    "type": "Microsoft.Storage/storageAccounts",
    "sku": {
      "name": "[parameters('storageSKU')]"
    },
    ...
  }
]
```

## <a name="available-properties"></a>Propriedades disponíveis

O exemplo anterior mostrou apenas algumas das propriedades que pode utilizar a secção de parâmetro. As propriedades disponíveis são:

```json
"parameters": {
    "<parameter-name>" : {
        "type" : "<type-of-parameter-value>",
        "defaultValue": "<default-value-of-parameter>",
        "allowedValues": [ "<array-of-allowed-values>" ],
        "minValue": <minimum-value-for-int>,
        "maxValue": <maximum-value-for-int>,
        "minLength": <minimum-length-for-string-or-array>,
        "maxLength": <maximum-length-for-string-or-array-parameters>,
        "metadata": {
            "description": "<description-of-the parameter>" 
        }
    }
}
```

| Nome do elemento | Necessário | Descrição |
|:--- |:--- |:--- |
| parameterName |Sim |Nome do parâmetro. Tem de ser um identificador de JavaScript válido. |
| tipo |Sim |Tipo do valor de parâmetro. Os tipos permitidos e os valores são **cadeia de caracteres**, **securestring**, **int**, **bool**, **objeto**, **secureObject**, e **matriz**. |
| defaultValue |Não |Valor predefinido para o parâmetro, se não for fornecido nenhum valor para o parâmetro. |
| allowedValues |Não |Matriz de valores permitidos para o parâmetro para se certificar de que o valor correto é fornecido. |
| minValue |Não |O valor mínimo para os parâmetros de tipo int, este valor é inclusivo. |
| maxValue |Não |O valor máximo para os parâmetros de tipo int, este valor é inclusivo. |
| minLength |Não |O comprimento mínimo para a cadeia de caracteres, securestring e parâmetros de tipo de matriz, este valor é inclusivo. |
| maxLength |Não |O comprimento máximo para a cadeia de caracteres, securestring e parâmetros de tipo de matriz, este valor é inclusivo. |
| descrição |Não |Descrição do parâmetro que é apresentado aos utilizadores através do portal. |

## <a name="template-functions-with-parameters"></a>Funções de modelo com parâmetros

Ao especificar o valor predefinido para um parâmetro, pode utilizar a maioria das funções de modelo. Pode utilizar outro valor de parâmetro para criar um valor predefinido. O modelo seguinte demonstra o uso de funções no valor predefinido:

```json
"parameters": {
  "siteName": {
    "type": "string",
    "defaultValue": "[concat('site', uniqueString(resourceGroup().id))]",
    "metadata": {
      "description": "The site name. To use the default value, do not specify a new value."
    }
  },
  "hostingPlanName": {
    "type": "string",
    "defaultValue": "[concat(parameters('siteName'),'-plan')]",
    "metadata": {
      "description": "The host name. To use the default value, do not specify a new value."
    }
  }
}
```

Não é possível utilizar o `reference` função na secção de parâmetros. Parâmetros são avaliados antes da implantação até a `reference` função não é possível obter o estado de tempo de execução de um recurso. 

## <a name="objects-as-parameters"></a>Objetos como parâmetros

Pode ser mais fácil organizar os valores relacionados, passando-os na como um objeto. Essa abordagem também reduz o número de parâmetros no modelo.

Defina o parâmetro no seu modelo e especifique um objeto JSON em vez de um único valor durante a implementação. 

```json
"parameters": {
  "VNetSettings": {
    "type": "object",
    "defaultValue": {
      "name": "VNet1",
      "location": "eastus",
      "addressPrefixes": [
        {
          "name": "firstPrefix",
          "addressPrefix": "10.0.0.0/22"
        }
      ],
      "subnets": [
        {
          "name": "firstSubnet",
          "addressPrefix": "10.0.0.0/24"
        },
        {
          "name": "secondSubnet",
          "addressPrefix": "10.0.1.0/24"
        }
      ]
    }
  }
},
```

Em seguida, referenciar subproperties do parâmetro ao utilizar o operador de ponto.

```json
"resources": [
  {
    "apiVersion": "2015-06-15",
    "type": "Microsoft.Network/virtualNetworks",
    "name": "[parameters('VNetSettings').name]",
    "location": "[parameters('VNetSettings').location]",
    "properties": {
      "addressSpace":{
        "addressPrefixes": [
          "[parameters('VNetSettings').addressPrefixes[0].addressPrefix]"
        ]
      },
      "subnets":[
        {
          "name":"[parameters('VNetSettings').subnets[0].name]",
          "properties": {
            "addressPrefix": "[parameters('VNetSettings').subnets[0].addressPrefix]"
          }
        },
        {
          "name":"[parameters('VNetSettings').subnets[1].name]",
          "properties": {
            "addressPrefix": "[parameters('VNetSettings').subnets[1].addressPrefix]"
          }
        }
      ]
    }
  }
]
```

## <a name="recommendations"></a>Recomendações
As seguintes informações podem ser úteis quando trabalha com parâmetros:

* Minimize a utilização de parâmetros. Sempre que possível, utilize uma variável ou um valor literal. Utilize parâmetros apenas para estes cenários:
   
   * Definições que pretende utilizar variações de acordo com o ambiente (SKU, tamanho, capacidade).
   * Nomes de recursos que pretende especificar para fácil identificação.
   * Valores que utiliza com frequência para realizar outras tarefas (por exemplo, um nome de utilizador de administrador).
   * Segredos (por exemplo, palavras-passe).
   * O número ou a matriz de valores a utilizar quando criar mais do que uma instância de um tipo de recurso.
* Utilize maiúscula para nomes de parâmetro.
* Forneça uma descrição de cada parâmetro nos metadados:

   ```json
   "parameters": {
       "storageAccountType": {
           "type": "string",
           "metadata": {
               "description": "The type of the new storage account created to store the VM disks."
           }
       }
   }
   ```

* Defina valores predefinidos para os parâmetros (exceto para senhas e chaves SSH). Ao especificar um valor predefinido, o parâmetro torna-se opcional durante a implementação. O valor predefinido pode ser uma cadeia vazia. 
   
   ```json
   "parameters": {
        "storageAccountType": {
            "type": "string",
            "defaultValue": "Standard_GRS",
            "metadata": {
                "description": "The type of the new storage account created to store the VM disks."
            }
        }
   }
   ```

* Uso **securestring** para todas as palavras-passe e segredos. Se passar dados confidenciais num objeto JSON, utilize o **secureObject** tipo. Não não possível ler os parâmetros de modelo com tipos de securestring ou secureObject após a implementação de recursos. 
   
   ```json
   "parameters": {
       "secretValue": {
           "type": "securestring",
           "metadata": {
               "description": "The value of the secret to store in the vault."
           }
       }
   }
   ```

* Utilizar um parâmetro para especificar a localização e partilhar esse valor de parâmetro tanto quanto possível com os recursos que é provável que estejam na mesma localização. Essa abordagem minimiza o número de vezes que os utilizadores são solicitados a fornecer informações de localização. Se um tipo de recurso é suportado em apenas um número limitado de localizações, pode querer especificar uma localização válida de diretamente no modelo ou adicione outro parâmetro de localização. Quando uma organização limita as regiões permitidas para seus usuários, o **resourceGroup (). location** expressão pode impedir um utilizador de implementar o modelo. Por exemplo, um utilizador cria um grupo de recursos numa região. Um segundo utilizador tem de implementar no grupo de recursos, mas não tem acesso para a região. 
   
   ```json
   "resources": [
     {
         "name": "[variables('storageAccountName')]",
         "type": "Microsoft.Storage/storageAccounts",
         "apiVersion": "2016-01-01",
         "location": "[parameters('location')]",
         ...
     }
   ]
   ```
    
* Evite utilizar um parâmetro ou variável para a versão de API para um tipo de recurso. Propriedades de recursos e os valores podem variar consoante o número de versão. IntelliSense num editor de código não é possível determinar o esquema correto quando a versão de API está definida como um parâmetro ou variável. Em vez disso, versão de codificar a API no modelo.
* Evite especificar um nome de parâmetro no seu modelo que corresponda a um parâmetro no comando de implementação. Gestor de recursos resolve este conflito de nomes, adicionando o sufixo **FromTemplate** para o parâmetro de modelo. Por exemplo, se incluir um parâmetro denominado **ResourceGroupName** no seu modelo, está em conflito com o **ResourceGroupName** parâmetro no [New-AzureRmResourceGroupDeployment ](/powershell/module/azurerm.resources/new-azurermresourcegroupdeployment) cmdlet. Durante a implementação, lhe for pedido para fornecer um valor para **ResourceGroupNameFromTemplate**.

## <a name="example-templates"></a>Modelos de exemplo

Estes modelos de exemplo demonstram alguns cenários de utilização de parâmetros. Implementá-las para testar como os parâmetros são tratados em cenários diferentes.

|Modelo  |Descrição  |
|---------|---------|
|[parâmetros com funções para os valores predefinidos](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/parameterswithfunctions.json) | Demonstra como utilizar funções de modelo ao definir os valores predefinidos de parâmetros. O modelo implementa todos os recursos. Ele constrói os valores de parâmetros e retorna esses valores. |
|[Objeto de parâmetro](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/parameterobject.json) | Demonstra como utilizar um objeto para um parâmetro. O modelo implementa todos os recursos. Ele constrói os valores de parâmetros e retorna esses valores. |

## <a name="next-steps"></a>Passos Seguintes

* Para ver modelos completos para vários tipos de soluções, veja os [Modelos de Início Rápido do Azure](https://azure.microsoft.com/documentation/templates/).
* Para saber como os valores de parâmetro de entrada durante a implementação, consulte [implementar uma aplicação com o modelo Azure Resource Manager](resource-group-template-deploy.md). 
* Para obter detalhes sobre as funções que pode utilizar a partir de dentro de um modelo, consulte [funções de modelo do Azure Resource Manager](resource-group-template-functions.md).
* Para obter informações sobre a utilização de um objeto de parâmetro, consulte [utilize um objeto como um parâmetro num modelo Azure Resource Manager](/azure/architecture/building-blocks/extending-templates/objects-as-parameters).
