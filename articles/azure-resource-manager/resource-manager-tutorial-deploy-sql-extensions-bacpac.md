---
title: Importar ficheiros SQL BACPAC com modelos do Azure Resource Manager | Microsoft Docs
description: Saiba como utilizar a extensão de Base de Dados SQL para importar ficheiros SQL BACPAC com modelos do Azure Resource Manager
services: azure-resource-manager
documentationcenter: ''
author: mumian
manager: dougeby
editor: ''
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.date: 11/13/2018
ms.topic: tutorial
ms.author: jgao
ms.openlocfilehash: 6349f3e6a30ce1b7a3162f54056901313327a101
ms.sourcegitcommit: b62f138cc477d2bd7e658488aff8e9a5dd24d577
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/13/2018
ms.locfileid: "51612542"
---
# <a name="tutorial-import-sql-bacpac-files-with-azure-resource-manager-templates"></a>Tutorial: Importar ficheiros SQL BACPAC com modelos do Azure Resource Manager

Saiba como utilizar as extensões de Base de Dados SQL do Azure para importar um ficheiro BACPAC. Neste tutorial, vai criar um modelo para implementar um Servidor SQL do Azure, uma Base de Dados SQL e um ficheiro BACPAC. Para obter informações sobre a implementação de extensões de máquina virtual do Azure com modelos do Azure Resource Manager, veja [Tutorial: Implementar extensões de máquina virtual com modelos do Azure Resource Manager](./resource-manager-tutorial-deploy-vm-extensions.md).

Este tutorial abrange as seguintes tarefas:

> [!div class="checklist"]
> * Preparar um ficheiro BACPAC
> * Abrir um modelo de Início Rápido
> * Editar o modelo
> * Implementar o modelo
> * Verificar a implementação

Se não tiver uma subscrição do Azure, [crie uma conta gratuita](https://azure.microsoft.com/free/) antes de começar.

## <a name="prerequisites"></a>Pré-requisitos

Para concluir este artigo, precisa de:

* [Visual Studio Code](https://code.visualstudio.com/) com a extensão Ferramentas do Resource Manager. Veja [Instalar a extensão](./resource-manager-quickstart-create-templates-use-visual-studio-code.md#prerequisites).
* Para aumentar a segurança, utilize uma palavra-passe gerada para a conta de administrador do SQL Server. Eis um exemplo para gerar uma palavra-passe:

    ```azurecli-interactive
    openssl rand -base64 32
    ```
    O Azure Key Vault foi criado para salvaguardar chaves criptográficos e outros segredos. Para obter mais informações, veja [Tutorial: Integrar o Azure Key Vault na implementação de modelos do Resource Manager](./resource-manager-tutorial-use-key-vault.md). Também recomendamos que atualize a palavra-passe a cada três meses.

## <a name="prepare-a-bacpac-file"></a>Preparar um ficheiro BACPAC

Um ficheiro BACPAC é partilhado numa [conta de Armazenamento do Azure com acesso público](https://armtutorials.blob.core.windows.net/sqlextensionbacpac/SQLDatabaseExtension.bacpac). Para criar o seu próprio, veja [Exportar uma base de dados SQL do Azure para um ficheiro BACPAC](../sql-database/sql-database-export.md). Se optar por publicar o ficheiro na sua própria localização, tem de atualizar o modelo mais tarde no tutorial.

## <a name="open-a-quickstart-template"></a>Abrir um modelo de Início Rápido

Os Modelos de Início Rápido do Azure são um repositório de modelos do Resource Manager. Em vez de criar um modelo do zero, pode encontrar um modelo de exemplo e personalizá-lo. O modelo utilizado neste tutorial é chamado [Implementar um Servidor SQL do Azure com Deteção de Ameaças](https://azure.microsoft.com/resources/templates/201-sql-threat-detection-server-policy-optional-db/).

1. No Visual Studio Code, selecione **Ficheiro**>**Abrir Ficheiro**.
2. em **Nome de ficheiro**, cole o seguinte URL:

    ```url
    https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-sql-threat-detection-server-policy-optional-db/azuredeploy.json
    ```
3. Selecione **Abrir** para abrir o ficheiro.

    Existem três recursos definidos no modelo:

    * `Microsoft.Sql/servers`. Veja a [referência do modelo](https://docs.microsoft.com/azure/templates/microsoft.sql/servers).
    * `Microsoft.SQL/servers/securityAlertPolicies`. Veja a [referência do modelo](https://docs.microsoft.com/azure/templates/microsoft.sql/servers/securityalertpolicies).
    * `Microsoft.SQL.servers/databases`.  Veja a [referência do modelo](https://docs.microsoft.com/azure/templates/microsoft.sql/servers/databases).
    É útil ter alguma compreensão básica do modelo antes de personalizá-lo.
4. Selecione **Ficheiro**>**Guardar Como** para guardar uma cópia do ficheiro no computador local, com o nome **azuredeploy.json**.

## <a name="edit-the-template"></a>Editar o modelo

Tem de adicionar dois recursos adicionais ao modelo.

* Para permitir que a extensão de base de dados SQL importe ficheiros BACPAC, tem de permitir o acesso aos serviços do Azure. Adicione o JSON seguinte à definição do servidor SQL:

    ```json
    {
        "type": "firewallrules",
        "name": "AllowAllAzureIps",
        "location": "[parameters('location')]",
        "apiVersion": "2014-04-01",
        "dependsOn": [
            "[variables('databaseServerName')]"
        ],
        "properties": {
            "startIpAddress": "0.0.0.0",
            "endIpAddress": "0.0.0.0"
        }
    }
    ```

    O modelo deve ter o seguinte aspeto:

    ![Azure Resource Manager deploy sql extensions BACPAC](./media/resource-manager-tutorial-deploy-sql-extensions-bacpac/resource-manager-tutorial-deploy-sql-extensions-bacpac-firewall.png)

* Adicione um recurso de extensão de Base de Dados SQL à definição de base de dados com o seguinte JSON:

    ```json
    "resources": [
        {
            "name": "Import",
            "type": "extensions",
            "apiVersion": "2014-04-01",
            "dependsOn": [
                "[resourceId('Microsoft.Sql/servers/databases', variables('databaseServerName'), variables('databaseName'))]"
            ],
            "properties": {
                "storageKeyType": "SharedAccessKey",
                "storageKey": "?",
                "storageUri": "https://armtutorials.blob.core.windows.net/sqlextensionbacpac/SQLDatabaseExtension.bacpac",
                "administratorLogin": "[variables('databaseServerAdminLogin')]",
                "administratorLoginPassword": "[variables('databaseServerAdminLoginPassword')]",
                "operationMode": "Import",
            }
        }
    ]
    ```

    O modelo deve ter o seguinte aspeto:

    ![Azure Resource Manager deploy sql extensions BACPAC](./media/resource-manager-tutorial-deploy-sql-extensions-bacpac/resource-manager-tutorial-deploy-sql-extensions-bacpac.png)

    Para compreender a definição do recurso, veja a [referência de extensão de Base de Dados SQL](https://docs.microsoft.com/azure/templates/microsoft.sql/servers/databases/extensions). Seguem alguns elementos importantes:

    * **dependsOn**: O recurso de extensão tem de ser criado após a criação da base de dados SQL.
    * **storageKeyType**: O tipo da chave de armazenamento a utilizar. O valor pode ser `StorageAccessKey` ou `SharedAccessKey`. Uma vez que o ficheiro BACPAC fornecido é partilhado numa conta de Armazenamento do Azure com acesso público, é utilizado "SharedAccessKey".
    * **storageKey**: A chave de armazenamento a utilizar. Se o tipo de chave de armazenamento for SharedAccessKey, tem de ser precedido por "?".
    * **storageUri**: o URI de armazenamento a utilizar. Se optar por não utilizar o ficheiro BACPAC fornecido, tem de atualizar os valores.
    * **administratorLoginPassword**: A palavra-passe do administrador do SQL. É recomendado utilizar uma palavra-passe gerada. Veja [Pré-requisitos](#prerequisites).

## <a name="deploy-the-template"></a>Implementar o modelo

Veja a secção [Implementar o modelo](./resource-manager-tutorial-create-multiple-instances.md#deploy-the-template) para obter o procedimento de implementação. Em alternativa, utilize o seguinte script de implementação do PowerShell:

```azurepowershell
$deploymentName = Read-Host -Prompt "Enter the name for this deployment"
$resourceGroupName = Read-Host -Prompt "Enter the Resource Group name"
$location = Read-Host -Prompt "Enter the location (i.e. centralus)"
$adminUsername = Read-Host -Prompt "Enter the virtual machine admin username"
$adminPassword = Read-Host -Prompt "Enter the admin password" -AsSecureString

New-AzureRmResourceGroup -Name $resourceGroupName -Location $location
New-AzureRmResourceGroupDeployment -Name $deploymentName `
    -ResourceGroupName $resourceGroupName `
    -adminUser $adminUsername `
    -adminPassword $adminPassword `
    -TemplateFile azuredeploy.json
```

É recomendado utilizar uma palavra-passe gerada. Veja [Pré-requisitos](#prerequisites).

## <a name="verify-the-deployment"></a>Verificar a implementação

No portal, selecione a base de dados SQL a partir do grupo de recursos recentemente implementado. Selecione **Editor de consultas (pré-visualização)** e, em seguida, introduza as credenciais de administrador. Deverá ver duas tabelas importadas para a base de dados:

![Azure Resource Manager deploy sql extensions BACPAC](./media/resource-manager-tutorial-deploy-sql-extensions-bacpac/resource-manager-tutorial-deploy-sql-extensions-bacpac-query-editor.png)

## <a name="clean-up-resources"></a>Limpar recursos

Quando os recursos do Azure já não forem necessários, limpe os recursos implementados ao eliminar o grupo de recursos.

1. No portal do Azure, selecione **Grupo de recursos** no menu à esquerda.
2. Introduza o nome do grupo de recursos no campo **Filtrar por nome**.
3. Selecione o nome do grupo de recursos.  Verá um total de seis recursos no grupo de recursos.
4. Selecione **Eliminar grupo de recursos** no menu superior.

## <a name="next-steps"></a>Passos Seguintes

Neste tutorial, implementou um Servidor SQL, uma Base de Dados SQL e importou um ficheiro BACPAC. Para saber como implementar recursos do Azure em várias regiões e como utilizar práticas de implementação seguras, veja

> [!div class="nextstepaction"]
> [Utilizar o Azure Deployment Manager](./resource-manager-tutorial-deploy-vm-extensions.md)
