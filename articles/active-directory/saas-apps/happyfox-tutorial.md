---
title: 'Tutorial: Integração do Azure Active Directory com HappyFox | Documentos da Microsoft'
description: Saiba como configurar o início de sessão único entre o Azure Active Directory e HappyFox.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: 8204ee77-f64b-4fac-b64a-25ea534feac0
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/16/2017
ms.author: jeedes
ms.openlocfilehash: 95def1ce278b0a816f19e3cd4e1b47bd3f68f1a9
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/02/2018
ms.locfileid: "39440294"
---
# <a name="tutorial-azure-active-directory-integration-with-happyfox"></a>Tutorial: Integração do Azure Active Directory com HappyFox

Neste tutorial, saiba como integrar HappyFox com o Azure Active Directory (Azure AD).

Integrar HappyFox no Azure AD fornece as seguintes vantagens:

- Pode controlar no Azure AD que tenha acesso ao HappyFox
- Pode permitir que os utilizadores automaticamente obter com sessão iniciada para HappyFox (Single Sign-On) com as suas contas do Azure AD
- Pode gerir as suas contas num local central – portal do Azure

Se quiser saber mais detalhes sobre a integração de aplicações SaaS com o Azure AD, veja [o que é o acesso a aplicações e início de sessão único com o Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com HappyFox, terá dos seguintes itens:

- Uma subscrição do Azure AD
- Um HappyFox início de sessão único na subscrição ativado

> [!NOTE]
> Para testar os passos neste tutorial, recomendamos que não utilize um ambiente de produção.

Para testar os passos neste tutorial, deve seguir estas recomendações:

- Não utilize o seu ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, pode obter uma versão de avaliação de um mês [aqui](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, vai testar do Azure AD início de sessão único num ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos de construção principais:

1. Adicionando HappyFox da Galeria
1. Configuração e teste do Azure AD início de sessão único

## <a name="adding-happyfox-from-the-gallery"></a>Adicionando HappyFox da Galeria
Para configurar a integração do HappyFox com o Azure AD, terá de adicionar HappyFox a partir da Galeria à sua lista de aplicações de SaaS geridas.

**Para adicionar HappyFox a partir da galeria, execute os seguintes passos:**

1. Na  **[portal do Azure](https://portal.azure.com)**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone. 

    ![Active Directory][1]

1. Navegue para **aplicações empresariais**. Em seguida, aceda a **todos os aplicativos**.

    ![Aplicações][2]
    
1. Para adicionar nova aplicação, clique em **nova aplicação** botão na parte superior de caixa de diálogo.

    ![Aplicações][3]

1. Na caixa de pesquisa, escreva **HappyFox**.

    ![Criar um utilizador de teste do Azure AD](./media/happyfox-tutorial/tutorial_happyfox_search.png)

1. No painel de resultados, selecione **HappyFox**e, em seguida, clique em **Add** botão para adicionar a aplicação.

    ![Criar um utilizador de teste do Azure AD](./media/happyfox-tutorial/tutorial_happyfox_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configuração e teste do Azure AD início de sessão único
Nesta secção, configure e teste do Azure AD início de sessão único com HappyFox com base num utilizador de teste chamado "Eduarda Almeida."

Para o início de sessão único funcione, o Azure AD precisa saber qual é o utilizador de contraparte no HappyFox a um utilizador no Azure AD. Em outras palavras, uma relação de ligação entre um utilizador do Azure AD e o utilizador relacionado no HappyFox deve ser estabelecido.

HappyFox, atribua o valor do **nome de utilizador** no Azure AD como o valor do **Username** para estabelecer a relação de ligação.

Para configurar e testar o Azure AD início de sessão único com HappyFox, tem de concluir os seguintes blocos de construção:

1. **[Configurar o Azure AD início de sessão único](#configuring-azure-ad-single-sign-on)**  - para permitir que os utilizadores utilizar esta funcionalidade.
1. **[Criar um utilizador de teste do Azure AD](#creating-an-azure-ad-test-user)**  - para testar o Azure AD início de sessão único com Eduarda Almeida.
1. **[Criar um utilizador de teste HappyFox](#creating-a-happyfox-test-user)**  - para ter um equivalente da Eduarda Almeida na HappyFox que está ligado à representação de utilizador do Azure AD.
1. **[Atribuir o utilizador de teste do Azure AD](#assigning-the-azure-ad-test-user)**  - para ativar a Eduarda Almeida utilizar o Azure AD início de sessão único.
1. **[Teste de início de sessão único](#testing-single-sign-on)**  - para verificar se a configuração funciona.

### <a name="configuring-azure-ad-single-sign-on"></a>Configuração do Azure AD início de sessão único

Nesta secção, pode ativar o Azure AD início de sessão único no portal do Azure e configurar início de sessão único em seu aplicativo HappyFox.

**Para configurar o Azure AD início de sessão único com HappyFox, execute os seguintes passos:**

1. No portal do Azure, sobre o **HappyFox** página de integração de aplicação, clique em **início de sessão único**.

    ![Configurar o início de sessão único][4]

1. Sobre o **início de sessão único** caixa de diálogo, selecione **modo** como **baseado em SAML logon** para ativar o início de sessão único.
 
    ![Configurar o início de sessão único](./media/happyfox-tutorial/tutorial_happyfox_samlbase.png)

1. Sobre o **HappyFox domínio e URLs** secção, execute os seguintes passos:

    ![Configurar o início de sessão único](./media/happyfox-tutorial/tutorial_happyfox_url.png)

    a. Na **URL de início de sessão** caixa de texto, escreva um URL com o seguinte padrão: `https://<subdomain>.happyfox.com/`

    b. Na **identificador** caixa de texto, escreva um URL com o seguinte padrão: `https://<subdomain>.happyfox.com/saml/metadata/`

    > [!NOTE] 
    > Estes valores não são reais. Atualize estes valores com o URL de início de sessão e o identificador real. Contacte [equipa de suporte de cliente HappyFox](https://support.happyfox.com/home) obter esses valores. 
 
1. Sobre o **certificado de assinatura SAML** secção, clique em **Certificate(Base64)** e, em seguida, guarde o ficheiro de certificado no seu computador.

    ![Configurar o início de sessão único](./media/happyfox-tutorial/tutorial_happyfox_certificate.png) 

1. Clique em **guardar** botão.

    ![Configurar o início de sessão único](./media/happyfox-tutorial/tutorial_general_400.png)

1. Sobre o **HappyFox configuração** secção, clique em **configurar HappyFox** para abrir **configurar início de sessão** janela. Cópia a **SAML único início de sessão no URL do serviço** partir a **seção de referência rápida**.

    ![Configurar o início de sessão único](./media/happyfox-tutorial/tutorial_happyfox_configure.png) 

1. Iniciar sessão portal de equipe do HappyFox e navegue para **Manage**, clique em **integrações** separador.

    ![Configurar o início de sessão único](./media/happyfox-tutorial/header.png) 

1. No separador de integrações, clique em **configurar** sob **SAML integração** para abrir o início de sessão único em definições.

    ![Configurar o início de sessão único](./media/happyfox-tutorial/configure.png) 

1. Dentro da seção de configuração de SAML, cole a **SAML único início de sessão no URL do serviço** que copiou do portal do Azure em **URL de destino de SSO** caixa de texto.

    ![Configurar o início de sessão único](./media/happyfox-tutorial/targeturl.png)

1. Abra o certificado transferido a partir do portal do Azure no bloco de notas e cole-no **IdP assinatura** secção.
 
    ![Configurar o início de sessão único](./media/happyfox-tutorial/cert.png)

1. Clique em **guardar definições** botão.

    ![Configurar o início de sessão único](./media/happyfox-tutorial/savesettings.png)

> [!TIP]
> Agora pode ler uma versão concisa destas instruções dentro do [portal do Azure](https://portal.azure.com), enquanto estiver a configurar a aplicação!  Depois de adicionar esta aplicação a partir da **do Active Directory > aplicações empresariais** secção, basta clicar o **Single Sign-On** separador e a documentação do embedded através de acesso a  **Configuração** seção na parte inferior. Pode ler mais sobre a funcionalidade de documentação do embedded aqui: [documentação do embedded do Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD
O objetivo desta secção é criar um utilizador de teste no portal do Azure chamado Eduarda Almeida.

![Criar utilizador do Azure AD][100]

**Para criar um utilizador de teste no Azure AD, execute os seguintes passos:**

1. Na **portal do Azure**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone.

    ![Criar um utilizador de teste do Azure AD](./media/happyfox-tutorial/create_aaduser_01.png) 

1. Para apresentar a lista de utilizadores, aceda a **utilizadores e grupos** e clique em **todos os utilizadores**.
    
    ![Criar um utilizador de teste do Azure AD](./media/happyfox-tutorial/create_aaduser_02.png) 

1. Para abrir o **usuário** caixa de diálogo, clique em **Add** na parte superior da caixa de diálogo.
 
    ![Criar um utilizador de teste do Azure AD](./media/happyfox-tutorial/create_aaduser_03.png) 

1. Sobre o **utilizador** caixa de diálogo página, execute os seguintes passos:
 
    ![Criar um utilizador de teste do Azure AD](./media/happyfox-tutorial/create_aaduser_04.png) 

    a. Na **Name** caixa de texto, tipo **BrittaSimon**.

    b. Na **nome de utilizador** caixa de texto, tipo a **endereço de e-mail** de BrittaSimon.

    c. Selecione **mostrar palavra-passe** e indique o valor da **palavra-passe**.

    d. Clique em **Criar**.
 
### <a name="creating-a-happyfox-test-user"></a>Criar um utilizador de teste HappyFox

Aplicativo oferece suporte apenas no tempo de aprovisionamento de utilizador e de utilizadores de autenticação são criados automaticamente no aplicativo.
        
### <a name="assigning-the-azure-ad-test-user"></a>Atribuir o utilizador de teste do Azure AD

Nesta secção, vai ativar Eduarda Almeida utilizar o Azure início de sessão único ao conceder acesso para HappyFox.

![Atribuir utilizador][200] 

**Para atribuir a Eduarda Almeida a HappyFox, execute os seguintes passos:**

1. No portal do Azure, abra a vista de aplicativos e, em seguida, navegue para a vista de diretório e aceda a **aplicações empresariais** , em seguida, clique em **todos os aplicativos**.

    ![Atribuir utilizador][201] 

1. Na lista de aplicações, selecione **HappyFox**.

    ![Configurar o início de sessão único](./media/happyfox-tutorial/tutorial_happyfox_app.png) 

1. No menu à esquerda, clique em **utilizadores e grupos**.

    ![Atribuir utilizador][202] 

1. Clique em **adicionar** botão. Em seguida, selecione **utilizadores e grupos** nos **adicionar atribuição** caixa de diálogo.

    ![Atribuir utilizador][203]

1. No **utilizadores e grupos** caixa de diálogo, selecione **Eduarda Almeida** na lista utilizadores.

1. Clique em **selecionar** botão **utilizadores e grupos** caixa de diálogo.

1. Clique em **atribua** botão **adicionar atribuição** caixa de diálogo.
    
### <a name="testing-single-sign-on"></a>Teste de início de sessão único

Nesta secção, vai testar a configuração do Azure AD única início de sessão com o painel de acesso.

1. Quando clica no mosaico HappyFox no painel de acesso, deve obter a página de início de sessão da aplicação de HappyFox. Deverá ver o **'SAML'** botão na página de início de sessão.

    ![Plug-in](./media/happyfox-tutorial/saml.png) 

1. Clique nas **'SAML'** botão para iniciar sessão no HappyFox com a sua conta do Azure AD.

Para obter mais informações sobre o painel de acesso, consulte [introdução ao painel de acesso](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (O que é o acesso a aplicações e o início de sessão único com o Azure Active Directory?)



<!--Image references-->

[1]: ./media/happyfox-tutorial/tutorial_general_01.png
[2]: ./media/happyfox-tutorial/tutorial_general_02.png
[3]: ./media/happyfox-tutorial/tutorial_general_03.png
[4]: ./media/happyfox-tutorial/tutorial_general_04.png

[100]: ./media/happyfox-tutorial/tutorial_general_100.png

[200]: ./media/happyfox-tutorial/tutorial_general_200.png
[201]: ./media/happyfox-tutorial/tutorial_general_201.png
[202]: ./media/happyfox-tutorial/tutorial_general_202.png
[203]: ./media/happyfox-tutorial/tutorial_general_203.png

