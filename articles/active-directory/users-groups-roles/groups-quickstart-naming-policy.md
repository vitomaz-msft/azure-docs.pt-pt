---
title: Adicionar uma política de nomenclatura para grupos do Office 365 no Azure Active Directory | Microsoft Docs
description: Explica como adicionar novos utilizadores ou eliminar os utilizadores existentes no Azure Active Directory
services: active-directory
documentationcenter: ''
author: curtand
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: quickstart
ms.date: 08/08/2018
ms.author: curtand
ms.reviewer: kairaz.contractor
ms.custom: it-pro
ms.openlocfilehash: 8ebdb22ba5ca04a5c811b3b368055f5f4371c75f
ms.sourcegitcommit: 30c7f9994cf6fcdfb580616ea8d6d251364c0cd1
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/18/2018
ms.locfileid: "40208941"
---
# <a name="quickstart-naming-policy-for-groups-in-azure-active-directory"></a>Início rápido: Política de nomenclatura para grupos no Azure Active Directory

Neste início rápido, irá configurar a política de nomenclatura no inquilino do Azure Active Directory (Azure AD) para grupos do Office 365 criados pelo utilizador, para o ajudar a ordenar e procurar os grupos do seu inquilino. Por exemplo, pode utilizar a política de nomenclatura para:

* Comunicar a função de um grupo, associação, região geográfica ou quem criou o grupo.
* Ajudar a categorizar grupos no livro de endereços.
* Bloquear a utilização de palavras específicas em nomes de grupos e aliases.

Se não tiver uma subscrição do Azure, [crie uma conta gratuita](https://azure.microsoft.com/free/) antes de começar.

## <a name="install-powershell-cmdlets"></a>Instalar cmdlets do PowerShell

Certifique-se de que desinstala qualquer versão anterior do módulo Azure Active Directory PowerShell para Graph para o Windows PowerShell e instala o [Azure Active Directory PowerShell para Graph - Versão de Pré-visualização Pública 2.0.0.137](https://www.powershellgallery.com/packages/AzureADPreview/2.0.0.137) antes de executar os comandos do PowerShell. 

1. Abra a aplicação Windows PowerShell como administrador.
2. Desinstale qualquer versão anterior do AzureADPreview.
  
  ````
  Uninstall-Module AzureADPreview
  ````
3. Instale a versão mais recente do AzureADPreview.
  
  ````
  Install-Module AzureADPreview
  ````
Se lhe for pedido para aceder a um repositório não fidedigno, escreva **Y**. Pode demorar alguns minutos a instalar o novo módulo.

## <a name="set-up-naming-policy"></a>Configurar a política de nomenclatura

### <a name="step-1-sign-in-using-powershell-cmdlets"></a>Passo 1: iniciar sessão com cmdlets do PowerShell

1. Abra a aplicação Windows PowerShell. Não precisa de privilégios elevados.

2. Execute os seguintes comandos para preparar a execução dos cmdlets.
  
  ````
  Import-Module AzureADPreview
  Connect-AzureAD
  ````
  No ecrã **Iniciar sessão na sua conta** apresentado, introduza a conta de administrador e a palavra-passe para ligar ao serviço e selecione **Iniciar sessão**.

3. Siga os passos em [Cmdlets do Azure Active Directory para configurar definições de grupo](groups-settings-cmdlets.md) para criar definições de grupo para este inquilino.

### <a name="step-2-view-the-current-settings"></a>Passo 2: ver as definições atuais

1. Veja as definições atuais da política de nomenclatura.
  
  ````
  $Setting = Get-AzureADDirectorySetting -Id (Get-AzureADDirectorySetting | where -Property DisplayName -Value "Group.Unified" -EQ).id
  ````
  
2. Apresente as definições atuais do grupo.
  
  ````
  $Setting.Values
  ````
  
### <a name="step-3-set-the-naming-policy-and-any-custom-blocked-words"></a>Passo 3: configurar a política de nomenclatura e quaisquer palavras bloqueadas personalizadas

1. Defina os prefixos e sufixos de nomes de grupo no Azure AD PowerShell.
  
  ````
  $Setting["PrefixSuffixNamingRequirement"] =“GRP_[GroupName]_[Department]"
  ````
  
2. Defina as palavras bloqueadas personalizadas que quer restringir. O exemplo seguinte ilustra como pode adicionar as suas próprias palavras personalizadas.
  
  ````
  $Setting["CustomBlockedWordsList"]=“Payroll,CEO,HR"
  ````
  
3. Guarde as definições da nova política para serem aplicadas, como no exemplo a seguir.
  
  ````
  Set-AzureADDirectorySetting -Id (Get-AzureADDirectorySetting | where -Property DisplayName -Value "Group.Unified" -EQ).id -DirectorySetting $Setting
  ````
  
Já está. Definiu a política de nomenclatura e adicionou palavras bloqueadas personalizadas.

## <a name="clean-up-resources"></a>Limpar recursos

1. Defina os prefixos e sufixos de nomes de grupo no Azure AD PowerShell.
  
  ````
  $Setting["PrefixSuffixNamingRequirement"] =""
  ````
  
2. Defina as palavras bloqueadas personalizadas que quer restringir. O exemplo seguinte ilustra como pode adicionar as suas próprias palavras personalizadas.
  
  ````
  $Setting["CustomBlockedWordsList"]=""
  ````
  
3. Guarde as definições da nova política para serem aplicadas, como no exemplo a seguir.
  
  ````
  Set-AzureADDirectorySetting -Id (Get-AzureADDirectorySetting | where -Property DisplayName -Value "Group.Unified" -EQ).id -DirectorySetting $Setting
  ````

## <a name="next-steps"></a>Passos seguintes

Neste início rápido, aprendeu a utilizar cmdlets do PowerShell para definir a política de nomenclatura para o inquilino do Azure AD.

Para obter mais informações sobre restrições técnicas, adicionar uma lista de palavras bloqueadas personalizadas ou as experiências de utilizador final nas aplicações do Office 365, avance para o artigo seguinte para obter detalhes sobre a política de nomenclatura.
> [!div class="nextstepaction"]
> [Todos os detalhes da política de nomenclatura](groups-naming-policy.md)