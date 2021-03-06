---
title: Criar a experimentação do Azure Machine Learning com um modelo Azure Resource Manager | Documentos da Microsoft
description: Este artigo fornece um exemplo para criar uma conta de experimentação do Azure Machine Learning com um modelo Azure Resource Manager.
services: machine-learning
author: hning86
ms.author: haining
ms.service: machine-learning
ms.component: core
ms.workload: data-services
ms.topic: article
ms.date: 11/14/2017
ROBOTS: NOINDEX
ms.openlocfilehash: 24ed164f4a1dfdb9a3913efa78fe58fab2b53696
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/24/2018
ms.locfileid: "46991968"
---
# <a name="configure-the-azure-machine-learning-experimentation-service"></a>Configurar o serviço de experimentação do Azure Machine Learning

[!INCLUDE [workbench-deprecated](../../../includes/aml-deprecating-preview-2017.md)]

## <a name="overview"></a>Descrição geral
Conta de serviço de experimentação do Machine Learning do Azure, a área de trabalho e projeto são recursos do Azure. Como tal, eles podem ser implementados com modelos do Gestor de recursos. Os modelos do Resource Manager são ficheiros JSON que definem os recursos que precisa de implementar para a sua solução. Para compreender os conceitos associados à implementação e gestão das suas soluções do Azure, veja [Descrição geral do Azure Resource Manager](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview).

## <a name="deploy-a-template"></a>Implementar um modelo
Implementar um modelo requer apenas algumas etapas na Interface de linha de comandos do Azure ou no portal do Azure.

### <a name="deploy-a-template-from-the-command-line"></a>Implementar um modelo a partir da linha de comandos
Usando a interface de linha de comandos, um único comando pode implementar um modelo para um grupo de recursos existente.
Veja o seguinte para obter informações sobre como criar um modelo.

```sh
# Login and validate your are in the right subscription context
az login

# Create a new resource group (you can use an existing one)
az group create --name <resource group name> --location <supported Azure region>
az group deployment create -n testdeploy -g <resource group name> --template-file <template-file.json> --parameters <parameters.json>
```

### <a name="deploy-a-template-from-the-azure-portal"></a>Implementar um modelo a partir do portal do Azure
Se preferir, também pode utilizar o portal do Azure para criar e implementar um modelo. Proceda do seguinte modo:

1. Navegue para o [portal do Azure](https://portal.azure.com).
2. Selecione **todos os serviços** e procure "templates".
3. Selecione **modelos**.
4. Clique em **+ adicionar** e copie as informações de modelo. 
5. Selecione o modelo que criou no passo #4 e clique em **Deploy**.


## <a name="create-a-template-from-an-existing-azure-resource-in-the-azure-portal"></a>Criar um modelo a partir de um recurso do Azure existente no portal do Azure
Se já tiver uma experimentação de máquina do Azure conta disponíveis, em [portal do Azure](https://portal.azure.com), pode gerar um modelo a partir desse recurso. 

1. Navegue para uma conta de experimentação do Azure no [portal do Azure](https://portal.azure.com).
2. Sob **configurações**, clique em **script de automação**.
3. Transfira o modelo. 

Em alternativa, pode editar manualmente os arquivos de modelo. Consulte o seguinte para obter um exemplo de um modelo e parâmetros de ficheiros. 

### <a name="template-file-example"></a>Exemplo de ficheiro de modelo
Crie um arquivo chamado "file.json de modelo" com abaixo conteúdo. 

```json
{
    "contentVersion": "1.0.0.0",
    "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "parameters": {
        "accountName": {
            "type": "string",
            "metadata": {
                "description": "Name of the machine learning experimentation team account"
            }
        },
        "location": {
            "type": "string",
            "metadata": {
                "description": "Location of the machine learning experimentation account and other dependent resources."
            }
        },
        "storageAccountSku": {
            "type": "string",
            "defaultValue": "Standard_LRS",
            "metadata": {
                "description": "Sku of the storage account"
            }
        }
    },
    "variables": {
        "mlexpVersion": "2017-05-01-preview",
        "stgResourceId": "[resourceId('Microsoft.Storage/storageAccounts', parameters('accountName'))]"
    },
    "resources": [
        {
            "name": "[parameters('accountName')]",
            "type": "Microsoft.Storage/storageAccounts",
            "location": "[parameters('location')]",
            "apiVersion": "2016-12-01",
            "tags": {
                "mlteamAccount": "[parameters('accountName')]"
            },
            "sku": {
                "name": "[parameters('storageAccountSku')]"
            },
            "kind": "Storage",
            "properties": {
                "encryption": {
                    "services": {
                        "blob": {
                            "enabled": true
                        }
                    },
                    "keySource": "Microsoft.Storage"
                }
            }
        },
        {
            "apiVersion": "[variables('mlexpVersion')]",
            "type": "Microsoft.MachineLearningExperimentation/accounts",
            "name": "[parameters('accountName')]",
            "location": "[parameters('location')]",
            "dependsOn": [
                "[resourceId('Microsoft.Storage/storageAccounts', parameters('accountName'))]"
            ],
            "properties": {
                "storageAccount": {
                    "storageAccountId": "[variables('stgResourceId')]",
                    "accessKey": "[listKeys(resourceId('Microsoft.Storage/storageAccounts', parameters('accountName')), '2016-12-01').keys[0].value]"
                }
            }
        }
    ]
}
```

### <a name="parameters"></a>Parâmetros 
Crie um ficheiro com abaixo conteúdo e guarde-o como < Parameters. JSON >. 

Há três valores que podem ser alteradas. 
* AccountName: O nome da conta de experimentação.
* Localização: Uma das regiões do Azure suportados.
* SKU da conta de armazenamento: O Azure ML só suporta armazenamento standard, premium não. Para obter mais informações sobre o armazenamento, consulte [introdução de armazenamento](https://docs.microsoft.com/azure/storage/common/storage-introduction). 

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
     "accountName": {
         "value": "MyExperimentationAccount"
     },
     "location": {
         "value": "eastus2"
     },
     "storageAccountSku": {
         "value": "Standard_LRS"
     }
  }
}
```

## <a name="next-steps"></a>Passos Seguintes
* [Criar e instalar o Azure Machine Learning](quickstart-installation.md)
