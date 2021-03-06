---
title: Permitir que as aplicações obter segredos do Cofre de chaves do Azure Stack | Documentos da Microsoft
description: Utilizar uma aplicação de exemplo para trabalhar com o Cofre de chaves do Azure Stack
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.assetid: 3748b719-e269-4b48-8d7d-d75a84b0e1e5
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 08/15/2018
ms.author: sethm
ms.openlocfilehash: ed02174247de1a99f3d9a4880fd0afa60f867552
ms.sourcegitcommit: d2f2356d8fe7845860b6cf6b6545f2a5036a3dd6
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/16/2018
ms.locfileid: "42139343"
---
# <a name="a-sample-application-that-uses-keys-and-secrets-stored-in-a-key-vault"></a>Uma aplicação de exemplo que utiliza as chaves e segredos armazenados num cofre de chaves

*Aplica-se a: integrados do Azure Stack, sistemas e o Kit de desenvolvimento do Azure Stack*

Siga os passos neste artigo para executar um aplicativo de exemplo (HelloKeyVault) que obtém as chaves e segredos de um cofre de chaves no Azure Stack.

## <a name="prerequisites"></a>Pré-requisitos

Pode instalar os seguintes pré-requisitos a partir do Azure Stack [Development Kit](azure-stack-connect-azure-stack.md#connect-to-azure-stack-with-remote-desktop), ou a partir de um baseado em Windows cliente externo se estiver [ligados através de VPN](azure-stack-connect-azure-stack.md#connect-to-azure-stack-with-vpn):

* Instale [módulos do Azure Stack compatível com o Azure PowerShell](azure-stack-powershell-install.md).
* No desenvolvimento do [Azure Stack Kit, deve ser o blobEndpoint](azure-stack-powershell-download.md) .

## <a name="create-and-get-the-key-vault-and-application-settings"></a>Criar e obter o Cofre de chaves e as definições da aplicação

Para se preparar para a aplicação de exemplo:

* Crie um cofre de chaves no Azure Stack.
* Registe uma aplicação no Azure Active Directory (Azure AD).

Pode utilizar o portal do Azure ou o PowerShell para se preparar para a aplicação de exemplo. Este artigo mostra como criar um cofre de chaves e registar uma aplicação com o PowerShell.

>[!NOTE]
>Por predefinição, o script do PowerShell cria um novo aplicativo no Active Directory. No entanto, pode registrar um de seus aplicativos existentes.

 Antes de executar o script a seguir, certifique-se de fornecer valores para o `aadTenantName` e `applicationPassword` variáveis. Se não especificar um valor para `applicationPassword`, esse script gera uma palavra-passe aleatória.

```powershell
$vaultName           = 'myVault'
$resourceGroupName   = 'myResourceGroup'
$applicationName     = 'myApp'
$location            = 'local'

# Password for the application. If not specified, this script will generate a random password during app creation.
$applicationPassword = ''

# Function to generate a random password for the application.
Function GenerateSymmetricKey()
{
    $key = New-Object byte[](32)
    $rng = [System.Security.Cryptography.RNGCryptoServiceProvider]::Create()
    $rng.GetBytes($key)
    return [System.Convert]::ToBase64String($key)
}

Write-Host 'Please log into your Azure Stack user environment' -foregroundcolor Green

$tenantARM = "https://management.local.azurestack.external"
$aadTenantName = "PLEASE FILL THIS IN WITH YOUR AAD TENANT NAME. FOR EXAMPLE: myazurestack.onmicrosoft.com"

# Configure the Azure Stack operator’s PowerShell environment.
Add-AzureRMEnvironment `
  -Name "AzureStackUser" `
  -ArmEndpoint $tenantARM

Set-AzureRmEnvironment `
  -Name "AzureStackAdmin" `
  -GraphAudience "https://graph.windows.net/"

$TenantID = Get-AzsDirectoryTenantId `
  -AADTenantName $aadTenantName `
  -EnvironmentName AzureStackUser

# Sign in to the user portal.
Add-AzureRmAccount `
  -EnvironmentName "AzureStackUser" `
  -TenantId $TenantID `

$now = [System.DateTime]::Now
$oneYearFromNow = $now.AddYears(1)

$applicationPassword = GenerateSymmetricKey

# Create a new Azure AD application.
$identifierUri = [string]::Format("http://localhost:8080/{0}",[Guid]::NewGuid().ToString("N"))
$homePage = "http://contoso.com"

Write-Host "Creating a new AAD Application"
$ADApp = New-AzureRmADApplication `
  -DisplayName $applicationName `
  -HomePage $homePage `
  -IdentifierUris $identifierUri `
  -StartDate $now `
  -EndDate $oneYearFromNow `
  -Password $applicationPassword

Write-Host "Creating a new AAD service principal"
$servicePrincipal = New-AzureRmADServicePrincipal `
  -ApplicationId $ADApp.ApplicationId

# Create a new resource group and a key vault in that resource group.
New-AzureRmResourceGroup `
  -Name $resourceGroupName `
  -Location $location

Write-Host "Creating vault $vaultName"
$vault = New-AzureRmKeyVault -VaultName $vaultName `
  -ResourceGroupName $resourceGroupName `
  -Sku standard `
  -Location $location

# Specify full privileges to the vault for the application.
Write-Host "Setting access policy"
Set-AzureRmKeyVaultAccessPolicy -VaultName $vaultName `
  -ObjectId $servicePrincipal.Id `
  -PermissionsToKeys all `
  -PermissionsToSecrets all

Write-Host "Paste the following settings into the app.config file for the HelloKeyVault project:"
'<add key="VaultUrl" value="' + $vault.VaultUri + '"/>'
'<add key="AuthClientId" value="' + $servicePrincipal.ApplicationId + '"/>'
'<add key="AuthClientSecret" value="' + $applicationPassword + '"/>'
Write-Host

```

Captura de ecrã seguinte mostra a saída do script usado para criar o Cofre de chaves:

![Cofre de chaves com chaves de acesso](media/azure-stack-kv-sample-app/settingsoutput.png)

Anote o **VaultUrl**, **AuthClientId**, e **AuthClientSecret** valores devolvidos pelo script anterior. Utilize estes valores para executar a aplicação de HelloKeyVault.

## <a name="download-and-configure-the-sample-application"></a>Transferir e configurar o aplicativo de exemplo

Transferir o exemplo de Cofre de chaves do Azure [exemplos de cliente do Cofre de chaves](https://www.microsoft.com/en-us/download/details.aspx?id=45343) página. Extraia os conteúdos do ficheiro. zip em sua estação de trabalho de desenvolvimento. Existem dois aplicativos na pasta de exemplos, este artigo utiliza HelloKeyVault.

Para carregar o exemplo de HelloKeyVault:

* Navegue para o **Microsoft.Azure.KeyVault.Samples** > **exemplos** > **HelloKeyVault** pasta.
* Abra a aplicação de HelloKeyVault no Visual Studio.

### <a name="configure-the-sample-application"></a>Configurar a aplicação de exemplo

No Visual Studio:

* Abra o ficheiro de HelloKeyVault\App.config e encontrar, navegue para o &lt; **appSettings** &gt; elemento.
* Atualização do **VaultUrl**, **AuthClientId**, e **AuthClientSecret** chaves com os valores devolvidos por utilizado para criar o Cofre de chaves. (Por predefinição, o ficheiro App. config tem um marcador de posição *AuthCertThumbprint*. Substitua este marcador de posição pela *AuthClientSecret*.)

  ![Definições de aplicação](media/azure-stack-kv-sample-app/appconfig.png)

* Recompile a solução.

## <a name="run-the-application"></a>Executar a aplicação

Quando executa HelloKeyVault, o aplicativo inicia sessão com o Azure AD e, em seguida, utiliza o token de AuthClientSecret para autenticar para o Cofre de chaves no Azure Stack.

Pode utilizar o exemplo de HelloKeyVault para:

* Execute operações básicas, tais como criar, encriptarem, moldagem e eliminam nas chaves e segredos.
* Transmita os parâmetros, tal como *encriptar* e *desencriptar* para HelloKeyVault e aplique as alterações especificadas para um cofre de chaves.

## <a name="next-steps"></a>Passos Seguintes

[Implementar uma VM com uma palavra-passe do Cofre de Chaves](azure-stack-kv-deploy-vm-with-secret.md)

[Implementar uma VM com um certificado do Key Vault](azure-stack-kv-push-secret-into-vm.md)
