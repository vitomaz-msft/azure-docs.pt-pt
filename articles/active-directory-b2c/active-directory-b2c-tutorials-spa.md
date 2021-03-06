---
title: Tutorial - Ativar a autenticação de aplicações de página única com contas através do Azure Active Directory B2C | Microsoft Docs
description: Tutorial sobre como utilizar o Azure Active Directory B2C para fornecer início de sessão do utilizador para uma aplicação de página única (JavaScript).
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.author: davidmu
ms.date: 3/02/2018
ms.custom: mvc
ms.topic: tutorial
ms.service: active-directory
ms.component: B2C
ms.openlocfilehash: 0f2fa2bb8e20ce4cc187fe6f061d2d8c251c4673
ms.sourcegitcommit: 9e179a577533ab3b2c0c7a4899ae13a7a0d5252b
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/23/2018
ms.locfileid: "49945217"
---
# <a name="tutorial-enable-single-page-app-authentication-with-accounts-using-azure-active-directory-b2c"></a>Tutorial: Ativar a autenticação de aplicações de página única com contas através do Azure Active Directory B2C

Este tutorial mostra como utilizar o Azure Active Directory (Azure AD) B2C para início de sessão e inscrição de utilizadores numa aplicação de página única (SPA). O Azure AD B2C permite às aplicações efetuar a autenticação em contas de redes sociais, contas empresariais e contas do Azure Active Directory através de protocolos padrão abertos.

Neste tutorial, ficará a saber como:

> [!div class="checklist"]
> * Registe uma aplicação de página única no seu diretório do Azure AD B2C.
> * Criar políticas para inscrição e início de sessão de utilizadores, editar um perfil e repor a palavra-passe.
> * Configurar o exemplo de aplicação para utilizar o seu diretório do Azure AD B2C.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Pré-requisitos

* Criar o seu próprio [Diretório do Azure AD B2C](active-directory-b2c-get-started.md)
* Instale o [Visual Studio 2017](https://www.visualstudio.com/downloads/) com a carga de trabalho de **desenvolvimento na Web e em ASP.NET**.
* [SDK .NET Core 2.0.0](https://www.microsoft.com/net/core) ou posterior
* Instalar o [Node. js](https://nodejs.org/en/download/)

## <a name="register-single-page-app"></a>Registar a aplicação de página única

As aplicações têm de ser [registadas](../active-directory/develop/developer-glossary.md#application-registration) no diretório antes de poderem receber [tokens de acesso](../active-directory/develop/developer-glossary.md#access-token) do Azure Active Directory. O registo de aplicações cria um [ID de aplicação](../active-directory/develop/developer-glossary.md#application-id-client-id) para a aplicação no diretório. 

Inicie sessão no [portal do Azure](https://portal.azure.com/) como administrador global do diretório do Azure AD B2C.

[!INCLUDE [active-directory-b2c-switch-b2c-tenant](../../includes/active-directory-b2c-switch-b2c-tenant.md)]

1. Selecione **Azure AD B2C** na lista de serviços no portal do Azure. 

2. Nas definições do B2C, clique em **Aplicações** e, em seguida, clique em **Adicionar**. 

    Para registar a aplicação Web de exemplo no diretório, utilize as seguintes definições:
    
    ![Adicionar uma nova aplicação](media/active-directory-b2c-tutorials-spa/spa-registration.png)
    
    | Definição      | Valor sugerido  | Descrição                                        |
    | ------------ | ------- | -------------------------------------------------- |
    | **Nome** | A minha aplicação de página única de exemplo | Introduza um **Nome** que descreva a aplicação aos consumidores. | 
    | **Incluir aplicação/API Web** | Sim | Selecione **Sim** para uma aplicação de página única. |
    | **Permitir fluxo implícito** | Sim | Selecione **Sim** se a aplicação utilizar o [Início de sessão OpenID Connect](active-directory-b2c-reference-oidc.md). |
    | **URL de resposta** | `http://localhost:6420` | Os URLs de resposta são pontos finais para onde o Azure AD B2C devolve quaisquer tokens que a aplicação solicite. Neste tutorial, o exemplo é executado localmente (localhost) e escuta na porta 6420. |
    | **Incluir cliente nativo** | Não | Uma vez que se trata de uma aplicação de página única e não um cliente nativo, selecione Não. |
    
3. Clique em **Criar** para registar a aplicação.

As aplicações registadas são apresentadas na lista de aplicações para o diretório do Azure AD B2C. Selecione a aplicação de página única na lista. É apresentado o painel de propriedades da aplicação de página única registada.

![Propriedades de aplicações de página única](./media/active-directory-b2c-tutorials-spa/b2c-spa-properties.png)

Anote o **ID de Cliente da Aplicação**. O ID identifica exclusivamente a aplicação e é necessário quando configurar a aplicação mais tarde no tutorial.

## <a name="create-policies"></a>Criar políticas

Uma política do Azure AD B2C define os fluxos de trabalho do utilizador. Por exemplo, iniciar sessão, inscrever, alterar palavras-passe e editar perfis são fluxos de trabalho comuns.

### <a name="create-a-sign-up-or-sign-in-policy"></a>Criar uma política de inscrição ou início de sessão

Para inscrever utilizadores para acederem e iniciarem sessão na aplicação Web, crie uma **política de inscrição ou início de sessão**.

1. Na página do portal do Azure AD B2C, selecione **Políticas de inscrição ou início de sessão** e clique em **Adicionar**.

    Para configurar a política, utilize as seguintes definições:

    ![Adicionar uma política de inscrição ou início de sessão](media/active-directory-b2c-tutorials-web-app/add-susi-policy.png)

    | Definição      | Valor sugerido  | Descrição                                        |
    | ------------ | ------- | -------------------------------------------------- |
    | **Nome** | SiUpIn | Introduza um **Nome** para a política. O nome da política tem o prefixo **B2C_1_**. Utilize o nome completo da política **B2C_1_SiUpIn** no código de exemplo. | 
    | **Fornecedor de identidade** | Inscrever-se no e-mail | O fornecedor de identidade utilizado para identificar exclusivamente o utilizador. |
    | **Atributos de inscrição** | Nome a Apresentar e Código Postal | Selecione os atributos a recolher do utilizador durante a inscrição. |
    | **Afirmações da aplicação** | Nome a Apresentar, Código Postal, O utilizador é novo, ID de Objeto do Utilizador | Selecione as [afirmações](../active-directory/develop/developer-glossary.md#claim) que quer incluir no [token de acesso](../active-directory/develop/developer-glossary.md#access-token). |

2. Clique em **Criar** para criar a política. 

### <a name="create-a-profile-editing-policy"></a>Criar uma política de edição de perfil

Para permitir aos utilizadores repor as respetivas informações de perfil por si próprios, crie uma **política de edição de perfil**.

1. Na página do portal do Azure AD B2C, selecione **Políticas de edição de perfil** e clique em **Adicionar**.

    Para configurar a política, utilize as seguintes definições:

    | Definição      | Valor sugerido  | Descrição                                        |
    | ------------ | ------- | -------------------------------------------------- |
    | **Nome** | SiPe | Introduza um **Nome** para a política. O nome da política tem o prefixo **B2C_1_**. Utilize o nome completo da política **B2C_1_SiPe** no código de exemplo. | 
    | **Fornecedor de identidade** | Inscrição da conta local | O fornecedor de identidade utilizado para identificar exclusivamente o utilizador. |
    | **Atributos do perfil** | Nome a Apresentar e Código Postal | Selecione os atributos que os utilizadores podem modificar durante a edição de perfil. |
    | **Afirmações da aplicação** | Nome a Apresentar, Código Postal, ID de Objeto do Utilizador | Selecione as [afirmações](../active-directory/develop/developer-glossary.md#claim) que quer incluir no [token de acesso](../active-directory/develop/developer-glossary.md#access-token) após uma edição de perfil com êxito. |

2. Clique em **Criar** para criar a política. 

### <a name="create-a-password-reset-policy"></a>Criar uma política de reposição de palavra-passe

Para ativar a reposição de palavras-passe na aplicação, tem de criar uma **política de reposição de palavra-passe**. Esta política descreve as experiências que os consumidores têm durante a reposição de palavras-passe e os conteúdos de tokens que a aplicação recebe após a conclusão com êxito.

1. Na página do portal do Azure AD B2C, selecione **Políticas de reposição de palavra-passe** e clique em **Adicionar**.

    Para configurar a política, utilize as seguintes definições.

    | Definição      | Valor sugerido  | Descrição                                        |
    | ------------ | ------- | -------------------------------------------------- |
    | **Nome** | SSPR | Introduza um **Nome** para a política. O nome da política tem o prefixo **B2C_1_**. Utilize o nome completo da política **B2C_1_SSPR** no código de exemplo. | 
    | **Fornecedor de identidade** | Repor a palavra-passe com o endereço de e-mail | Trata-se do fornecedor de identidade utilizado para identificar exclusivamente o utilizador. |
    | **Afirmações da aplicação** | ID de Objeto do Utilizador | Selecione as [afirmações](../active-directory/develop/developer-glossary.md#claim) que quer incluir no [token de acesso](../active-directory/develop/developer-glossary.md#access-token) após uma reposição de palavra-passe com êxito. |

2. Clique em **Criar** para criar a política. 

## <a name="update-single-page-app-code"></a>Atualizar o código de aplicação de página única

Agora que tem uma aplicação registada e as políticas criadas, tem de configurar a aplicação para utilizar o diretório do Azure AD B2C. Neste tutorial, irá configurar uma aplicação SPA JavaScript de exemplo que pode transferir a partir do GitHub. 

[Transfira um ficheiro zip](https://github.com/Azure-Samples/active-directory-b2c-javascript-msal-singlepageapp/archive/master.zip) ou clone a aplicação Web de exemplo a partir do GitHub.

```
git clone https://github.com/Azure-Samples/active-directory-b2c-javascript-msal-singlepageapp.git
```
O exemplo de aplicação demonstra como uma aplicação de página única pode utilizar o Azure AD B2C para inscrever e iniciar a sessão do utilizador e chamar uma API Web protegida. Tem de alterar a aplicação para utilizar o registo de aplicações no seu diretório e configurar as políticas criadas por si. 

Para alterar as definições da aplicação:

1. Abra o ficheiro `index.html` no exemplo de aplicação de página única Node.js.
2. Configure o exemplo com as informações de registo de diretório do Azure AD B2C. Altere as seguintes linhas de código (certifique-se de que substitui os valores pelos nomes do seu diretório e APIs):

    ```javascript
    // The current application coordinates were pre-registered in a B2C directory.
    var applicationConfig = {
        clientID: '<Application ID for your SPA obtained from portal app registration>',
        authority: "https://fabrikamb2c.b2clogin.com/tfp/fabrikamb2c.onmicrosoft.com/B2C_1_<Sign-up or sign-in policy name>",
        b2cScopes: ["https://fabrikamb2c.onmicrosoft.com/demoapi/demo.read"],
        webApi: 'https://fabrikamb2chello.azurewebsites.net/hello',
    };
    ```

    O nome de política utilizado neste tutorial é **B2C_1_SiUpIn**. Se estiver a utilizar um nome de política diferente, utilize o seu nome de política no valor `authority`.

## <a name="run-the-sample"></a>Executar o exemplo

1. Inicie uma linha de comandos Node.js.
2. Mude para o diretório que contém o exemplo de Node.js. Por exemplo, `cd c:\active-directory-b2c-javascript-msal-singlepageapp`
3. Execute os seguintes comandos:
    ```
    npm install && npm update
    node server.js
    ```

    A janela da consola mostra o número de porta onde está alojada a aplicação.
    
    ```
    Listening on port 6420...
    ```

4. Utilize um browser para navegar para o endereço `http://localhost:6420` para ver a aplicação.

A aplicação de exemplo suporta inscrição, início de sessão, edição de perfil e reposição de palavra-passe. Este tutorial destaca como um utilizador se inscreve para utilizar a aplicação com um endereço de e-mail. Pode explorar outros cenários.

### <a name="sign-up-using-an-email-address"></a>Inscrever-se com um endereço de e-mail

1. Clique em **Iniciar sessão** para se inscrever como um utilizador da aplicação SPA. Esta opção utiliza a política **B2C_1_SiUpIn** definida num passo anterior.

2. O Azure AD B2C apresenta uma página de início de sessão com uma ligação de inscrição. Uma vez que ainda não tem uma conta, clique na ligação **Inscrever-se agora**. 

3. O fluxo de trabalho de inscrição apresenta uma página para recolher e verificar a identidade do utilizador através de um endereço de e-mail. O fluxo de trabalho de inscrição também recolhe a palavra-passe do utilizador e os atributos solicitados definidos na política.

    Utilize um endereço de e-mail válido e valide com o código de verificação. Defina uma palavra-passe. Introduza os valores para os atributos solicitados. 

    ![Fluxo de trabalho de inscrição](media/active-directory-b2c-tutorials-desktop-app/sign-up-workflow.png)

4. Clique em **Criar** para criar uma conta local no diretório do Azure AD B2C.

Agora, o utilizador pode utilizar o respetivo endereço de e-mail para iniciar sessão e utilizar a aplicação SPA.

> [!NOTE]
> Após iniciar sessão, a aplicação mostra um erro de "permissões insuficientes". Recebe este erro porque está a tentar aceder a um recurso a partir do diretório de demonstração. Como o seu token de acesso só é válido para o seu diretório do Azure AD, a chamada à API não é autorizada. Avance para o próximo tutorial para criar uma API Web protegida para o seu diretório. 

## <a name="clean-up-resources"></a>Limpar recursos

Pode utilizar o diretório do Azure AD B2C se planeia experimentar outros tutoriais do Azure AD B2C. Quando já não for preciso, pode [eliminar o diretório do Azure AD B2C](active-directory-b2c-faqs.md#how-do-i-delete-my-azure-ad-b2c-tenant).

## <a name="next-steps"></a>Passos seguintes

Neste tutorial, aprendeu a criar um diretório do Azure AD B2C, criar políticas e atualizar a aplicação de página única de exemplo para utilizar o diretório do Azure AD B2C. Avance para o próximo tutorial para aprender a registar, configurar e chamar uma API Web protegida a partir de uma aplicação de ambiente de trabalho.

> [!div class="nextstepaction"]
> [Exemplos de código do Azure AD B2C](https://azure.microsoft.com/resources/samples/?service=active-directory-b2c&sort=0)
