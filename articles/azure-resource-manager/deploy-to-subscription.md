---
title: Implementar recursos na subscrição do Azure | Documentos da Microsoft
description: Descreve como criar um modelo do Azure Resource Manager que implementa os recursos no âmbito da subscrição.
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/10/2018
ms.author: tomfitz
ms.openlocfilehash: 1d281ebe80c6089c559cfaa77f4875a856566092
ms.sourcegitcommit: 4b1083fa9c78cd03633f11abb7a69fdbc740afd1
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/10/2018
ms.locfileid: "49079383"
---
# <a name="deploy-resources-to-an-azure-subscription"></a>Implementar recursos para uma subscrição do Azure

Normalmente, implementa recursos para um grupo de recursos na sua subscrição do Azure. No entanto, alguns recursos podem ser implementados no nível da sua subscrição do Azure. Estes recursos aplicam-se na sua subscrição. [As políticas](../azure-policy/azure-policy-introduction.md), [controlo de acesso baseado em funções](../role-based-access-control/overview.md), e [Centro de segurança do Azure](../security-center/security-center-intro.md) são os serviços que talvez deseje aplicam-se ao nível da subscrição, em vez de nível do grupo de recursos.

Este artigo utiliza a CLI do Azure e o PowerShell para implementar os modelos.

## <a name="name-and-location-for-deployment"></a>Nome e a localização para a implementação

Ao implementar a sua subscrição, tem de fornecer uma localização para a implementação. Também pode fornecer um nome para a implementação. Se não especificar um nome para a implementação, o nome do modelo é utilizado como o nome da implementação. Por exemplo, implementar um modelo com o nome **azuredeploy. JSON** cria um nome de implementação do padrão de **azuredeploy**.

A localização das implementações de nível de subscrição é imutável. Não é possível criar uma implementação num único local quando existe uma implementação existente com o mesmo nome mas localização diferente. Se receber o código de erro `InvalidDeploymentLocation`, utilize um nome diferente ou a mesma localização como a implementação anterior para esse nome.

## <a name="using-template-functions"></a>Com as funções de modelo

Para implementações de nível de subscrição, existem algumas considerações importantes ao utilizar as funções de modelo:

* O [resourceGroup()](resource-group-template-functions-resource.md#resourcegroup) função é **não** suportado.
* O [resourceId()](resource-group-template-functions-resource.md#resourceid) função é suportada. Utilize-o para obter o ID de recurso para recursos que são utilizados em implementações de nível de subscrição. Por exemplo, obter o ID de recurso para uma definição de política com `resourceId('Microsoft.Authorization/roleDefinitions/', parameters('roleDefinition'))`
* O [reference()](resource-group-template-functions-resource.md#reference) e [List ()](resource-group-template-functions-resource.md#list) as funções são suportadas.

## <a name="assign-policy"></a>Atribuir política

O exemplo seguinte atribui uma definição de política existente para a subscrição. Se a política tem parâmetros, forneça-los como um objeto. Se a política não aceitam parâmetros, utilize o objeto vazio padrão.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2018-05-01/subscriptionDeploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "policyDefinitionID": {
            "type": "string"
        },
        "policyName": {
            "type": "string"
        },
        "policyParameters": {
            "type": "object",
            "defaultValue": {}
        }
    },
    "variables": {},
    "resources": [
        {
            "type": "Microsoft.Authorization/policyAssignments",
            "name": "[parameters('policyName')]",
            "apiVersion": "2018-03-01",
            "properties": {
                "scope": "[subscription().id]",
                "policyDefinitionId": "[parameters('policyDefinitionID')]",
                "parameters": "[parameters('policyParameters')]"
            }
        }
    ]
}
```

Para aplicar uma política incorporada a sua subscrição do Azure, utilize os seguintes comandos do CLI do Azure. Neste exemplo, a política não tem parâmetros

```azurecli-interactive
# Built-in policy that does not accept parameters
definition=$(az policy definition list --query "[?displayName=='Audit resource location matches resource group location'].id" --output tsv)

az deployment create \
  -n policyassign \
  -l southcentralus \
  --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/policyassign.json \
  --parameters policyDefinitionID=$definition policyName=auditRGLocation
```

Para implementar este modelo com o PowerShell, utilize:

```azurepowershell-interactive
$definition = Get-AzureRmPolicyDefinition | Where-Object { $_.Properties.DisplayName -eq 'Audit resource location matches resource group location' }

New-AzureRmDeployment `
  -Name policyassign `
  -Location southcentralus `
  -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/policyassign.json `
  -policyDefinitionID $definition.PolicyDefinitionId `
  -policyName auditRGLocation
```

Para aplicar uma política incorporada a sua subscrição do Azure, utilize os seguintes comandos do CLI do Azure. Neste exemplo, a política tem parâmetros.

```azurecli-interactive
# Built-in policy that accepts parameters
definition=$(az policy definition list --query "[?displayName=='Allowed locations'].id" --output tsv)

az deployment create \
  -n policyassign \
  -l southcentralus \
  --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/policyassign.json \
  --parameters policyDefinitionID=$definition policyName=setLocation policyParameters="{'listOfAllowedLocations': {'value': ['westus']} }"
```

Para implementar este modelo com o PowerShell, utilize:

```azurepowershell-interactive
$definition = Get-AzureRmPolicyDefinition | Where-Object { $_.Properties.DisplayName -eq 'Allowed locations' }

$locations = @("westus", "westus2")
$policyParams =@{listOfAllowedLocations = @{ value = $locations}}

New-AzureRmDeployment `
  -Name policyassign `
  -Location southcentralus `
  -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/policyassign.json `
  -policyDefinitionID $definition.PolicyDefinitionId `
  -policyName setLocation `
  -policyParameters $policyParams
```

## <a name="define-and-assign-policy"></a>Definir e atribuir política

Pode [definir](../azure-policy/policy-definition.md) e atribuir uma política no modelo do mesmo.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2018-05-01/subscriptionDeploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {},
    "variables": {},
    "resources": [
        {
            "type": "Microsoft.Authorization/policyDefinitions",
            "name": "locationpolicy",
            "apiVersion": "2018-05-01",
            "properties": {
                "policyType": "Custom",
                "parameters": {},
                "policyRule": {
                    "if": {
                        "field": "location",
                        "equals": "northeurope"
                    },
                    "then": {
                        "effect": "deny"
                    }
                }
            }
        },
        {
            "type": "Microsoft.Authorization/policyAssignments",
            "name": "location-lock",
            "apiVersion": "2018-05-01",
            "dependsOn": [
                "locationpolicy"
            ],
            "properties": {
                "scope": "[subscription().id]",
                "policyDefinitionId": "[resourceId('Microsoft.Authorization/policyDefinitions', 'locationpolicy')]"
            }
        }
    ]
}
```

Para criar a definição de política na sua subscrição e aplicá-la para a subscrição, utilize o seguinte comando da CLI.

```azurecli-interactive
az deployment create \
  -n definePolicy \
  -l southcentralus \
  --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/policydefineandassign.json
```

Para implementar este modelo com o PowerShell, utilize:

```azurepowershell-interactive
New-AzureRmDeployment `
  -Name definePolicy `
  -Location southcentralus `
  -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/policydefineandassign.json
```

## <a name="assign-role"></a>Atribuir função

O exemplo seguinte atribui uma função a um utilizador ou grupo.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2018-05-01/subscriptionDeploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "principalId": {
            "type": "string"
        },
        "roleDefinitionId": {
            "type": "string"
        }
    },
    "variables": {},
    "resources": [
        {
            "type": "Microsoft.Authorization/roleAssignments",
            "name": "[guid(parameters('principalId'), deployment().name)]",
            "apiVersion": "2017-09-01",
            "properties": {
                "roleDefinitionId": "[resourceId('Microsoft.Authorization/roleDefinitions', parameters('roleDefinitionId'))]",
                "principalId": "[parameters('principalId')]"
            }
        }
    ]
}
```

Para atribuir um grupo do Active Directory a uma função para a sua subscrição, utilize os seguintes comandos do CLI do Azure.

```azurecli-interactive
# Get ID of the role you want to assign
role=$(az role definition list --name Contributor --query [].name --output tsv)

# Get ID of the AD group to assign the role to
principalid=$(az ad group show --group demogroup --query objectId --output tsv)

az deployment create \
  -n demoRole \
  -l southcentralus \
  --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/roleassign.json \
  --parameters principalId=$principalid roleDefinitionId=$role
```

Para implementar este modelo com o PowerShell, utilize:

```azurepowershell-interactive
$role = Get-AzureRmRoleDefinition -Name Contributor

$adgroup = Get-AzureRmADGroup -DisplayName demogroup

New-AzureRmDeployment `
  -Name demoRole `
  -Location southcentralus `
  -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/roleassign.json `
  -roleDefinitionId $role.Id `
  -principalId $adgroup.Id
```

## <a name="next-steps"></a>Passos Seguintes
* Para obter um exemplo de implementação de definições de área de trabalho para o Centro de segurança do Azure, veja [deployASCwithWorkspaceSettings.json](https://github.com/krnese/AzureDeploy/blob/master/ARM/deployments/deployASCwithWorkspaceSettings.json).
* Para criar um grupo de recursos, veja [criar grupos de recursos nos modelos do Azure Resource Manager](create-resource-group-in-template.md).
* Para saber mais sobre a criação de modelos Azure Resource Manager, veja [criação de modelos](resource-group-authoring-templates.md). 
* Para obter uma lista das funções disponíveis num modelo, consulte [funções de modelo](resource-group-template-functions.md).

