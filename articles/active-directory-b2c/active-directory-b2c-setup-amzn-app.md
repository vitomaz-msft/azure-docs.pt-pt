---
title: Configurar a inscrição e início de sessão com uma conta de Amazon com o Azure Active Directory B2C | Documentos da Microsoft
description: Forneça a inscrição e início de sessão para clientes com contas do Amazon nas suas aplicações com o Azure Active Directory B2C.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 09/21/2018
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: 5bc1e0faf01125f498eb339bba841b96f6de9d82
ms.sourcegitcommit: 5b8d9dc7c50a26d8f085a10c7281683ea2da9c10
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/26/2018
ms.locfileid: "47181809"
---
# <a name="set-up-sign-up-and-sign-in-with-an-amazon-account-using-azure-active-directory-b2c"></a>Configurar a inscrição e início de sessão com uma conta de Amazon com o Azure Active Directory B2C

## <a name="create-an-amazon-application"></a>Criar uma aplicação da Amazon

Para utilizar uma conta do Amazon como um fornecedor de identidade no Azure Active Directory (Azure AD) B2C, terá de criar uma aplicação no seu inquilino que a representa. Se ainda não tiver uma conta do Amazon pode obtê-la em [ http://www.amazon.com/ ](http://www.amazon.com/).

1. Entrar para o [Centro de programadores do Amazon](https://login.amazon.com/) com as suas credenciais de conta da Amazon.
2. Se ainda não o fez, clique em **Inscreva-se**, siga os passos de registo do desenvolvedor e aceite a política.
3. Selecione **registar o novo aplicativo**.
4. Introduza um **Name**, **Descrição**, e **URL de aviso de privacidade**e, em seguida, clique em **guardar**. O aviso de privacidade é uma página que gere que fornece informações de privacidade aos utilizadores.
5. Na **as definições da Web** secção, copie os valores de **ID de cliente**. Selecione **Mostrar segredo** para obter o segredo do cliente e, em seguida, copiá-lo. Terá de ambos para configurar uma conta do Amazon como um fornecedor de identidade no seu inquilino. **Segredo do cliente** é uma credencial de segurança importantes.
6. No **as definições da Web** secção, selecione **editar**e, em seguida, introduza `https://your-tenant-name.b2clogin.com` no **permitido origens do JavaScript** e `https://your-tenant-name.b2clogin.com/your-tenant-name.onmicrosoft.com/oauth2/authresp` na **permitidos Devolver a URLs**. Substitua `your-tenant-name` com o nome do seu inquilino. Tem de utilizar todas as letras minúsculas, ao introduzir o nome do seu inquilino, mesmo que o inquilino está definido com letras maiúsculas no Azure AD B2C.
7. Clique em **Guardar**.

## <a name="configure-an-amazon-account-as-an-identity-provider"></a>Configurar uma conta do Amazon como fornecedor de identidade

1. Inicie sessão para o [portal do Azure](https://portal.azure.com/) como administrador global do inquilino do Azure AD B2C.
2. Certifique-se de que está a utilizar o diretório que contém o seu inquilino do Azure AD B2C, clicando no **filtro de diretório e subscrição** no menu superior e escolher o diretório que contém o seu inquilino.
3. Escolha **Todos os serviços** no canto superior esquerdo do portal do Azure, procure e selecione **Azure AD B2C**.
4. Selecione **fornecedores de identidade**e, em seguida, selecione **Add**.
5. Introduza um **nome**. Por exemplo, introduza *Amazon*.
6. Selecione **tipo de fornecedor de identidade**, selecione **Amazon**e clique em **OK**.
7. Selecione **configurar este fornecedor de identidade** e introduza o ID de cliente que registou anteriormente como a **ID de cliente** e introduza o segredo do cliente que registou como o **segredo do cliente**da aplicação Amazon que criou anteriormente.
8. Clique em **OK** e, em seguida, clique em **criar** para guardar a configuração da Amazon.

