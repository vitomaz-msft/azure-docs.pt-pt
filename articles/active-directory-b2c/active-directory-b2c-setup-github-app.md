---
title: Configurar a inscrição e início de sessão com uma conta do GitHub através do Azure Active Directory B2C | Documentos da Microsoft
description: Forneça a inscrição e início de sessão para os clientes com contas do GitHub nas suas aplicações com o Azure Active Directory B2C.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 09/11/2018
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: 7f8b2c6dc570f7a610c0d661da0c6df7491647bd
ms.sourcegitcommit: 5b8d9dc7c50a26d8f085a10c7281683ea2da9c10
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/26/2018
ms.locfileid: "47182183"
---
# <a name="set-up-sign-up-and-sign-in-with-a-github-account-using-azure-active-directory-b2c"></a>Configurar a inscrição e início de sessão com uma conta do GitHub através do Azure Active Directory B2C

> [!NOTE]
> Esta funcionalidade está em pré-visualização.
> 

Para utilizar uma conta do Github como um fornecedor de identidade no Azure Active Directory (Azure AD) B2C, terá de criar uma aplicação no seu inquilino que a representa. Se ainda não tiver uma conta do Github, pode obtê-la em [ https://www.github.com/ ](https://www.github.com/).

## <a name="create-a-github-oauth-application"></a>Criar uma aplicação de OAuth do GitHub

1. Entrar para o [GitHub desenvolvedor](https://github.com/settings/developers) Web site com as suas credenciais do GitHub.
2. Selecione **aplicações de OAuth** e, em seguida, selecione **nova aplicação de OAuth**.
3. Introduza um **nome da aplicação** e a sua **URL da home page**.
4. Introduza `https://your-tenant-name.b2clogin.com/your-tenant-name.onmicrosoft.com/oauth2/authresp` no **URL de chamada de retorno de autorização**. Substitua `your-tenant-name` com o nome do seu inquilino do Azure AD B2C.
5. Clique em **registar a aplicação**.
6. Copie os valores da **ID de cliente** e **segredo do cliente**. Precisa para adicionar o fornecedor de identidade ao seu inquilino.

## <a name="configure-a-github-account-as-an-identity-provider"></a>Configurar uma conta do GitHub como um fornecedor de identidade

1. Inicie sessão para o [portal do Azure](https://portal.azure.com/) como administrador global do inquilino do Azure AD B2C.
2. Certifique-se de que está a utilizar o diretório que contém o seu inquilino do Azure AD B2C, clicando no **filtro de diretório e subscrição** no menu superior e escolher o diretório que contém o seu inquilino.
3. Escolha **Todos os serviços** no canto superior esquerdo do portal do Azure, procure e selecione **Azure AD B2C**.
4. Selecione **fornecedores de identidade**e, em seguida, selecione **Add**.
5. Fornecer um **nome**. Por exemplo, introduza *Github*.
6. Selecione **tipo de fornecedor de identidade**, selecione **Github (pré-visualização)** e clique em **OK**.
7. Selecione **configurar este fornecedor de identidade** e introduza o Id de cliente que registou anteriormente como a **ID de cliente** e introduza o segredo do cliente que registou como o **segredo do cliente**da aplicação conta Github que criou anteriormente.
8. Clique em **OK** e, em seguida, clique em **criar** para guardar a configuração de conta do Github.