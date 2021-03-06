---
title: Através de AppAuth num aplicativo de iOS no Azure Active Directory B2C | Documentos da Microsoft
description: Este artigo mostra-lhe como criar uma aplicação iOS que utiliza AppAuth com o Azure Active Directory B2C para gerir identidades de utilizador e autenticar utilizadores.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 03/07/2017
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: 5f95b71497b59eafff09d4add2b4bb1c20656592
ms.sourcegitcommit: 0c64460a345c89a6b579b1d7e273435a5ab4157a
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/31/2018
ms.locfileid: "43339363"
---
# <a name="azure-ad-b2c-sign-in-using-an-ios-application"></a>O Azure AD B2C: Início de sessão com uma aplicação iOS

A plataforma de identidade da Microsoft utiliza as normas de abertura, como o OAuth2 e o OpenID Connect. Com um protocolo de padrão aberto oferece mais opções de programador ao selecionar uma biblioteca para integrar com os nossos serviços. Fornecemos este passo a passo e outros, como ele para auxiliar os desenvolvedores a escrita de aplicativos que se conectam a plataforma do Microsoft Identity. A maioria das bibliotecas que implementam [especificação do especificação RFC6749 OAuth2](https://tools.ietf.org/html/rfc6749) consegue ligar à plataforma do Microsoft Identity.

> [!WARNING]
> A Microsoft não fornece correções para as bibliotecas de terceiros e não fez uma revisão dessas bibliotecas. Este exemplo está a utilizar uma biblioteca de terceiros chamada AppAuth que foi testado quanto à compatibilidade em cenários básicos com o Azure AD B2C. Problemas e pedidos de funcionalidades devem ser direcionados para o projeto de código aberto da biblioteca. Para obter mais informações, consulte [este artigo](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-libraries).
>
>

Se estiver familiarizado com o OAuth2 ou o OpenID Connect, grande parte este exemplo de configuração pode não fazer muito sentido para. Recomendamos que leia a breve [descrição geral do protocolo aqui documentado](active-directory-b2c-reference-protocols.md).

## <a name="get-an-azure-ad-b2c-directory"></a>Obter um diretório do Azure AD B2C
Para poder utilizar o Azure AD B2C, tem de criar um diretório ou inquilino. Um diretório é um contentor para todos os seus utilizadores, aplicações, grupos e muito mais. Se ainda não tiver um, [crie um diretório do B2C](active-directory-b2c-get-started.md) antes de continuar.

## <a name="create-an-application"></a>Criar uma aplicação
Em seguida, precisa de criar uma aplicação no diretório do B2C. O registo de aplicações dá-informações do Azure AD que necessita para comunicar de forma segura com a sua aplicação. Para criar uma aplicação móvel, siga [estas instruções](active-directory-b2c-app-registration.md). É necessário:

* Incluir uma **Native client** no aplicativo.
* Copiar a **ID da Aplicação** atribuída à aplicação. Este GUID são necessários mais tarde.
* Configurar uma **URI de redirecionamento** com um esquema personalizado (por exemplo, com.onmicrosoft.fabrikamb2c.exampleapp://oauth/redirect). São necessários mais tarde este URI.

## <a name="create-your-policies"></a>Criar as políticas
No Azure AD B2C, cada experiência de utilizador é definida por uma [política](active-directory-b2c-reference-policies.md). Esta aplicação contém uma experiência de identidade: um combinado início de sessão e inscrição. Criar esta política, conforme descrito no [artigo de referência de política](active-directory-b2c-reference-policies.md#create-a-sign-up-policy). Quando cria a política, certifique-se de que:

* Sob **atributos de inscrição**, selecione o atributo **nome a apresentar**.  Pode selecionar também outros atributos.
* Sob **afirmações de aplicação**, selecione as declarações **nome a apresentar** e **ID de objeto do utilizador**. Pode selecionar outras afirmações.
* Copiar o **Nome** de cada política depois de a criar. Nome da sua política tem o prefixo `b2c_1_` quando guardar a política.  É necessário o nome de política mais tarde.

[!INCLUDE [active-directory-b2c-devquickstarts-policy](../../includes/active-directory-b2c-devquickstarts-policy.md)]

Depois de criar as políticas, está pronto para criar a aplicação.

## <a name="download-the-sample-code"></a>Baixe o código de exemplo
Fornecemos um exemplo funcional que utiliza AppAuth com o Azure AD B2C [no GitHub](https://github.com/Azure-Samples/active-directory-ios-native-appauth-b2c). Pode baixar o código e executá-lo. Para utilizar o seu inquilino do Azure AD B2C, siga as instruções no [README.md](https://github.com/Azure-Samples/active-directory-ios-native-appauth-b2c/blob/master/README.md).

Este exemplo foi criado ao seguir as instruções de Leia-me pela [no GitHub do projeto iOS AppAuth](https://github.com/openid/AppAuth-iOS). Para obter mais detalhes sobre como funcionam o exemplo e a biblioteca, referenciar o ficheiro README de AppAuth no GitHub.

## <a name="modifying-your-app-to-use-azure-ad-b2c-with-appauth"></a>Modificar a sua aplicação para utilizar o Azure AD B2C com AppAuth

> [!NOTE]
> AppAuth suporta o iOS 7 e acima.  No entanto, para suportar os inícios de sessão sociais no Google, SFSafariViewController é necessário que requer o iOS 9 ou superior.
>

### <a name="configuration"></a>Configuração

Pode configurar a comunicação com o Azure AD B2C, especificando o ponto final de autorização e o ponto final do token URIs.  Para gerar estes URIs, terá das seguintes informações:
* ID do inquilino (por exemplo, contoso.onmicrosoft.com)
* Nome da política (por exemplo, B2C\_1\_SignUpIn)

O ponto final do token URI que pode ser gerado, substituindo o inquilino\_ID e a política de\_nome no URL seguinte:

```objc
static NSString *const tokenEndpoint = @"https://<Tenant_name>.b2clogin.com/te/<Tenant_ID>/<Policy_Name>/oauth2/v2.0/token";
```

O ponto final de autorização URI que pode ser gerado, substituindo o inquilino\_ID e a política de\_nome no URL seguinte:

```objc
static NSString *const authorizationEndpoint = @"https://<Tenant_name>.b2clogin.com/te/<Tenant_ID>/<Policy_Name>/oauth2/v2.0/authorize";
```

Execute o seguinte código para criar o seu objeto AuthorizationServiceConfiguration:

```objc
OIDServiceConfiguration *configuration = 
    [[OIDServiceConfiguration alloc] initWithAuthorizationEndpoint:authorizationEndpoint tokenEndpoint:tokenEndpoint];
// now we are ready to perform the auth request...
```

### <a name="authorizing"></a>A Autorizar

Depois de configurar ou obter uma configuração de serviço de autorização, pode ser construído um pedido de autorização. Para criar o pedido, terá das seguintes informações:  
* ID de cliente (por exemplo, 00000000-0000-0000-0000-000000000000)
* URI de redirecionamento com um esquema personalizado (por exemplo, com.onmicrosoft.fabrikamb2c.exampleapp://oauth/redirect)

Ambos os itens devem ter sido guardados quando era [registar a sua aplicação](#create-an-application).

```objc
OIDAuthorizationRequest *request = 
    [[OIDAuthorizationRequest alloc] initWithConfiguration:configuration
                                                  clientId:kClientId
                                                    scopes:@[OIDScopeOpenID, OIDScopeProfile]
                                               redirectURL:[NSURL URLWithString:kRedirectUri]
                                              responseType:OIDResponseTypeCode
                                      additionalParameters:nil];

AppDelegate *appDelegate = (AppDelegate *)[UIApplication sharedApplication].delegate;
appDelegate.currentAuthorizationFlow = 
    [OIDAuthState authStateByPresentingAuthorizationRequest:request
                                   presentingViewController:self
                                                   callback:^(OIDAuthState *_Nullable authState, NSError *_Nullable error) {
        if (authState) {
            NSLog(@"Got authorization tokens. Access token: %@", authState.lastTokenResponse.accessToken);
            [self setAuthState:authState];
        } else {
            NSLog(@"Authorization error: %@", [error localizedDescription]);
            [self setAuthState:nil];
        }
    }];
```

Para configurar a sua aplicação para processar o redirecionamento para o URI com o esquema personalizado, terá de atualizar a lista de "Esquemas de URL" no seu ficheiro info. plist:
* Abra o ficheiro info. plist.
* Coloque o cursor sobre uma linha, como o "Código de tipo de SO de pacote" e clique no \+ símbolo.
* Renomeie a nova linha "Tipos de URL".
* Clique na seta à esquerda de "Tipos de URL" para abrir a árvore.
* Clique na seta à esquerda de "Item 0" para abrir a árvore.
* Mudar o nome do primeiro item sob o Item 0 para esquemas de URL.
* Clique na seta à esquerda de "Esquemas de URL" para abrir a árvore.
* Na coluna "Value", é um campo em branco à esquerda de "O Item 0" por baixo de esquemas de URL.  Defina o valor para o esquema de exclusivo da sua aplicação.  O valor tem de corresponder ao esquema utilizado no redirectURL ao criar o objeto de OIDAuthorizationRequest.  No exemplo, é utilizado o esquema 'com.onmicrosoft.fabrikamb2c.exampleapp'.

Consulte a [guia de AppAuth](https://openid.github.io/AppAuth-iOS/) sobre como concluir o resto do processo. Se precisar de iniciar rapidamente a uma aplicação de trabalho, veja [o exemplo](https://github.com/Azure-Samples/active-directory-ios-native-appauth-b2c). Siga os passos a [README.md](https://github.com/Azure-Samples/active-directory-ios-native-appauth-b2c/blob/master/README.md) para introduzir a sua própria configuração do Azure AD B2C.

Estamos sempre abertas para comentários e sugestões! Se tiver quaisquer problemas com este artigo, ou tiver recomendações para melhorar este conteúdo, Agradecemos os seus comentários na parte inferior da página. Para pedidos de funcionalidades, adicioná-los ao [UserVoice](https://feedback.azure.com/forums/169401-azure-active-directory/category/160596-b2c).
