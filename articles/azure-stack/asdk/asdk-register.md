---
title: Registe o ASDK com o Azure | Documentos da Microsoft
description: Descreve como registar o Azure Stack com o Azure para permitir relatórios de utilização e distribuição de mercado.
services: azure-stack
documentationcenter: ''
author: jeffgilb
manager: femila
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/11/2018
ms.author: jeffgilb
ms.reviewer: misainat
ms.openlocfilehash: 19206163a07964b564300bbed1fed45973c8fc74
ms.sourcegitcommit: 3a02e0e8759ab3835d7c58479a05d7907a719d9c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/13/2018
ms.locfileid: "49311070"
---
# <a name="azure-stack-registration"></a>Registo do Azure Stack
Pode registrar sua instalação do Azure Stack Development Kit (ASDK) com o Azure para transferir itens do marketplace do Azure e configurar relatórios de volta à Microsoft de dados de comércio. É necessário Registro para dar suporte a todas as funcionalidades do Azure Stack, incluindo a distribuição de mercado. Registo é recomendado porque permite-lhe testar a funcionalidade importante do Azure Stack, como relatórios de utilização e distribuição de mercado. Depois de registar o Azure Stack, a utilização é comunicada ao Azure commerce. Pode vê-lo sob a subscrição utilizada para o registo. No entanto, os utilizadores ASDK para não são cobrados qualquer utilização que reportam.

Se não registar a sua ASDK, poderá ver uma **ativação necessária** alerta de aviso que indica que se Registre seu Kit de desenvolvimento do Azure Stack. Este comportamento é esperado.

## <a name="prerequisites"></a>Pré-requisitos
Antes de utilizar estas instruções para registar o ASDK com o Azure, certifique-se de que tem instalado o Azure Stack do PowerShell e baixado as ferramentas do Azure Stack, conforme descrito no [configuração pós-implantação](asdk-post-deploy.md) artigo.

Além disso, o modo de idioma do PowerShell deve ser definido como **FullLanguageMode** no computador utilizado para registar o ASDK no Azure. Para verificar se o modo de idioma atual está definido como completa, abra uma janela elevada do PowerShell e execute os seguintes comandos do PowerShell:

```PowerShell  
$ExecutionContext.SessionState.LanguageMode
```

Certifique-se de que a saída devolve **FullLanguageMode**. Se qualquer outro modo de idioma é retornado, registo, terá de ser executado noutro computador ou o modo de idioma tem de ser definido como **FullLanguageMode** antes de continuar.

## <a name="register-azure-stack-with-azure"></a>Registar o Azure Stack com o Azure
Siga estes passos para registar o ASDK com o Azure.

> [!NOTE]
> Todos estes passos tem de ser executados a partir de um computador que tenha acesso ao ponto final com privilégios. Para o ASDK, que é o computador de anfitrião do kit de desenvolvimento.

1. Abra uma consola do PowerShell como administrador.  

2. Execute os seguintes comandos do PowerShell para registar a sua instalação ASDK com o Azure. Terá de iniciar sessão na sua subscrição do Azure e a instalação de ASDK local. Se não tiver uma subscrição do Azure, ainda, pode [criar uma conta gratuita do Azure aqui](https://azure.microsoft.com/free/?b=17.06). Registar o Azure Stack, incorre em sem custos na sua subscrição do Azure.<br><br>Se estiver a executar o script de Registro em mais de uma instância do Azure Stack com o mesmo ID de subscrição do Azure, defina um nome exclusivo para o registo ao executar o **Set-AzsRegistration** cmdlet. O **RegistrationName** parâmetro tem um valor padrão de **AzureStackRegistration**. No entanto, se utilizar o mesmo nome em mais de uma instância do Azure Stack, o script falhará.

    ```PowerShell  
    # Add the Azure cloud subscription environment name. 
    # Supported environment names are AzureCloud, AzureChinaCloud or AzureUSGovernment depending which Azure subscription you are using.
    Add-AzureRmAccount -EnvironmentName "AzureCloud"

    # Register the Azure Stack resource provider in your Azure subscription
    Register-AzureRmResourceProvider -ProviderNamespace Microsoft.AzureStack

    #Import the registration module that was downloaded with the GitHub tools
    Import-Module C:\AzureStack-Tools-master\Registration\RegisterWithAzure.psm1

    #Register Azure Stack
    $AzureContext = Get-AzureRmContext
    $CloudAdminCred = Get-Credential -UserName AZURESTACK\CloudAdmin -Message "Enter the credentials to access the privileged endpoint."
    $RegistrationName = "<unique-registration-name>"
    $UsageReporting = $true # Set to $false if using the Capacity Billing model
    Set-AzsRegistration `
    -PrivilegedEndpointCredential $CloudAdminCred `
    -PrivilegedEndpoint AzS-ERCS01 `
    -BillingModel Development `
    -RegistrationName $RegistrationName `
    -UsageReportingEnabled:$UsageReporting
    ```
3. Quando o script tiver concluído, deverá ver esta mensagem: **seu ambiente está agora registado e ativado usando os parâmetros fornecidos.**

    ![O ambiente está agora registado](media/asdk-register/1.PNG)

## <a name="verify-the-registration-was-successful"></a>Certifique-se de que o registo foi concluída com êxito
Siga estes passos para verificar se o registo ASDK com o Azure foi concluída com êxito.

1. Inicie sessão para o [portal de administração do Azure Stack](https://adminportal.local.azurestack.external).

2. Clique em **gestão de Marketplace** > **adicionar a partir do Azure**.

    ![](media/asdk-register/2.PNG)

3. Se vir uma lista de itens disponíveis do Azure, a ativação foi concluída com êxito.

    ![](media/asdk-register/3.PNG)

## <a name="move-a-registration-resource"></a>Mover um recurso de registo
Mover um recurso de registro entre grupos de recursos na mesma subscrição **é** suportado. Para obter mais informações sobre como mover recursos para um novo grupo de recursos, consulte [mover recursos para um novo grupo de recursos ou subscrição](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-move-resources).


## <a name="next-steps"></a>Passos Seguintes
[Adicionar um item do mercado do Azure Stack](.\.\azure-stack-marketplace.md)
