---
title: 'Tutorial: Integração do Azure Active Directory com PurelyHR | Documentos da Microsoft'
description: Saiba como configurar o início de sessão único entre o Azure Active Directory e PurelyHR.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: 86a9c454-596d-4902-829a-fe126708f739
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/20/2017
ms.author: jeedes
ms.openlocfilehash: c02dc274c5d22c16b2bda6d7896ee64c41d6e2ab
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/02/2018
ms.locfileid: "39438999"
---
# <a name="tutorial-azure-active-directory-integration-with-purelyhr"></a>Tutorial: Integração do Azure Active Directory com PurelyHR

Neste tutorial, saiba como integrar PurelyHR com o Azure Active Directory (Azure AD).

Integrar PurelyHR no Azure AD fornece as seguintes vantagens:

- Pode controlar no Azure AD que tenha acesso ao PurelyHR
- Pode permitir que os utilizadores automaticamente obter com sessão iniciada para PurelyHR (Single Sign-On) com as suas contas do Azure AD
- Pode gerir as suas contas num local central – portal do Azure

Se quiser saber mais detalhes sobre a integração de aplicações SaaS com o Azure AD, veja [o que é o acesso a aplicações e início de sessão único com o Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com PurelyHR, terá dos seguintes itens:

- Uma subscrição do Azure AD
- Um PurelyHR início de sessão único na subscrição ativado

> [!NOTE]
> Para testar os passos neste tutorial, recomendamos que não utilize um ambiente de produção.

Para testar os passos neste tutorial, deve seguir estas recomendações:

- Não utilize o seu ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, pode obter uma versão de avaliação de um mês [aqui](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, vai testar do Azure AD início de sessão único num ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos de construção principais:

1. Adicionando PurelyHR da Galeria
1. Configuração e teste do Azure AD início de sessão único

## <a name="adding-purelyhr-from-the-gallery"></a>Adicionando PurelyHR da Galeria
Para configurar a integração do PurelyHR com o Azure AD, terá de adicionar PurelyHR a partir da Galeria à sua lista de aplicações de SaaS geridas.

**Para adicionar PurelyHR a partir da galeria, execute os seguintes passos:**

1. Na  **[portal do Azure](https://portal.azure.com)**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone. 

    ![Active Directory][1]

1. Navegue para **aplicações empresariais**. Em seguida, aceda a **todos os aplicativos**.

    ![Aplicações][2]
    
1. Para adicionar nova aplicação, clique em **nova aplicação** botão na parte superior de caixa de diálogo.

    ![Aplicações][3]

1. Na caixa de pesquisa, escreva **PurelyHR**.

    ![Criar um utilizador de teste do Azure AD](./media/purelyhr-tutorial/tutorial_purelyhr_search.png)

1. No painel de resultados, selecione **PurelyHR**e, em seguida, clique em **Add** botão para adicionar a aplicação.

    ![Criar um utilizador de teste do Azure AD](./media/purelyhr-tutorial/tutorial_purelyhr_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configuração e teste do Azure AD início de sessão único
Nesta secção, configure e teste do Azure AD início de sessão único com PurelyHR com base num utilizador de teste chamado "Eduarda Almeida."

Para o início de sessão único funcione, o Azure AD precisa saber qual é o utilizador de contraparte no PurelyHR a um utilizador no Azure AD. Em outras palavras, uma relação de ligação entre um utilizador do Azure AD e o utilizador relacionado no PurelyHR deve ser estabelecido.

PurelyHR, atribua o valor do **nome de utilizador** no Azure AD como o valor do **Username** para estabelecer a relação de ligação.

Para configurar e testar o Azure AD início de sessão único com PurelyHR, tem de concluir os seguintes blocos de construção:

1. **[Configurar o Azure AD início de sessão único](#configuring-azure-ad-single-sign-on)**  - para permitir que os utilizadores utilizar esta funcionalidade.
1. **[Criar um utilizador de teste do Azure AD](#creating-an-azure-ad-test-user)**  - para testar o Azure AD início de sessão único com Eduarda Almeida.
1. **[Criar um utilizador de teste PurelyHR](#creating-a-purelyhr-test-user)**  - para ter um equivalente da Eduarda Almeida na PurelyHR que está ligado à representação de utilizador do Azure AD.
1. **[Atribuir o utilizador de teste do Azure AD](#assigning-the-azure-ad-test-user)**  - para ativar a Eduarda Almeida utilizar o Azure AD início de sessão único.
1. **[Teste de início de sessão único](#testing-single-sign-on)**  - para verificar se a configuração funciona.

### <a name="configuring-azure-ad-single-sign-on"></a>Configuração do Azure AD início de sessão único

Nesta secção, pode ativar o Azure AD início de sessão único no portal do Azure e configurar início de sessão único em seu aplicativo PurelyHR.

**Para configurar o Azure AD início de sessão único com PurelyHR, execute os seguintes passos:**

1. No portal do Azure, sobre o **PurelyHR** página de integração de aplicação, clique em **início de sessão único**.

    ![Configurar o início de sessão único][4]

1. Sobre o **início de sessão único** caixa de diálogo, selecione **modo** como **baseado em SAML logon** para ativar o início de sessão único.
 
    ![Configurar o início de sessão único](./media/purelyhr-tutorial/tutorial_purelyhr_samlbase.png)

1. Sobre o **PurelyHR domínio e URLs** secção, execute os seguintes passos, se desejar configurar a aplicação no **IDP** iniciada pelo modo:

    ![Configurar o início de sessão único](./media/purelyhr-tutorial/tutorial_purelyhr_url.png)
   
    Na **URL de resposta** caixa de texto, escreva um URL com o seguinte padrão: `https://<companyID>.purelyhr.com/sso-consume`

1. Verifique **Mostrar definições de URL avançadas**, se desejar configurar a aplicação na **SP** iniciada pelo modo:

    ![Configurar o início de sessão único](./media/purelyhr-tutorial/tutorial_purelyhr_url1.png)
    
    Na **URL de início de sessão** caixa de texto, digite o valor usando o seguinte padrão: `https://<companyID>.purelyhr.com/sso-initiate`
     
    > [!NOTE]
    > Estes valores não são o real. Atualize estes valores com o URL de resposta e o URL de início de sessão real. Contacte [equipa de suporte de cliente PurelyHR](http://support.purelyhr.com/) obter esses valores. 

1. Sobre o **certificado de assinatura SAML** secção, clique em **certificado (Base64)** e, em seguida, guarde o ficheiro de certificado no seu computador.

    ![Configurar o início de sessão único](./media/purelyhr-tutorial/tutorial_purelyhr_certificate.png) 

1. Clique em **guardar** botão.

    ![Configurar o início de sessão único](./media/purelyhr-tutorial/tutorial_general_400.png)
    
1. Sobre o **PurelyHR configuração** secção, clique em **configurar PurelyHR** para abrir **configurar início de sessão** janela. Cópia a **ID de entidade de SAML e SAML único início de sessão no URL do serviço** partir o **secção de referência rápida.**

    ![Configurar o início de sessão único](./media/purelyhr-tutorial/tutorial_purelyhr_configure.png) 

1. Para configurar o início de sessão único num **PurelyHR** lado, o início de sessão no seu site como um administrador.

1. Abra o **Dashboard** entre as opções na barra de ferramentas e clique em **as definições de SSO**.

1. Cole os valores nas caixas, conforme descrito abaixo-

    ![Configurar o início de sessão único](./media/purelyhr-tutorial/purelyhr-dashboard-sso-settings.png)  

    a. Abra o **Certificate(Bas64)** transferido a partir do portal do Azure no bloco de notas e copie o valor do certificado. Cole o valor copiado para o **certificado X.509** caixa.

    b. Na **URL de emissor de Idp** caixa, cole a **ID de entidade de SAML** copiados a partir do portal do Azure.

    c. Na **URL de ponto final de Idp** caixa, cole a **SAML único início de sessão no URL do serviço** copiados a partir do portal do Azure. 

    d. Verifique os **Auto-criar utilizadores** caixa de verificação para ativar o aprovisionamento automático de utilizadores no PurelyHR.

    e. Clique em **guardar alterações** para guardar as definições.

> [!TIP]
> Agora pode ler uma versão concisa destas instruções dentro do [portal do Azure](https://portal.azure.com), enquanto estiver a configurar a aplicação!  Depois de adicionar esta aplicação a partir da **do Active Directory > aplicações empresariais** secção, basta clicar o **Single Sign-On** separador e a documentação do embedded através de acesso a  **Configuração** seção na parte inferior. Pode ler mais sobre a funcionalidade de documentação do embedded aqui: [documentação do embedded do Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD
O objetivo desta secção é criar um utilizador de teste no portal do Azure chamado Eduarda Almeida.

![Criar utilizador do Azure AD][100]

**Para criar um utilizador de teste no Azure AD, execute os seguintes passos:**

1. Na **portal do Azure**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone.

    ![Criar um utilizador de teste do Azure AD](./media/purelyhr-tutorial/create_aaduser_01.png) 

1. Para apresentar a lista de utilizadores, aceda a **utilizadores e grupos** e clique em **todos os utilizadores**.
    
    ![Criar um utilizador de teste do Azure AD](./media/purelyhr-tutorial/create_aaduser_02.png) 

1. Para abrir o **usuário** caixa de diálogo, clique em **Add** na parte superior da caixa de diálogo.
 
    ![Criar um utilizador de teste do Azure AD](./media/purelyhr-tutorial/create_aaduser_03.png) 

1. Sobre o **utilizador** caixa de diálogo página, execute os seguintes passos:
 
    ![Criar um utilizador de teste do Azure AD](./media/purelyhr-tutorial/create_aaduser_04.png) 

    a. Na **Name** caixa de texto, tipo **BrittaSimon**.

    b. Na **nome de utilizador** caixa de texto, tipo a **endereço de e-mail** de BrittaSimon.

    c. Selecione **mostrar palavra-passe** e indique o valor da **palavra-passe**.

    d. Clique em **Criar**.
 
### <a name="creating-a-purelyhr-test-user"></a>Criar um utilizador de teste PurelyHR

Para ativar a utilizadores do Azure AD iniciar sessão no PurelyHR, tem de ser aprovisionados em PurelyHR. PurelyHR, aprovisionamento é uma tarefa automática e não existem passos manuais são necessários quando o aprovisionamento automático de utilizadores está ativado.

### <a name="assigning-the-azure-ad-test-user"></a>Atribuir o utilizador de teste do Azure AD

Nesta secção, vai ativar Eduarda Almeida utilizar o Azure início de sessão único ao conceder acesso para PurelyHR.

![Atribuir utilizador][200] 

**Para atribuir a Eduarda Almeida a PurelyHR, execute os seguintes passos:**

1. No portal do Azure, abra a vista de aplicativos e, em seguida, navegue para a vista de diretório e aceda a **aplicações empresariais** , em seguida, clique em **todos os aplicativos**.

    ![Atribuir utilizador][201] 

1. Na lista de aplicações, selecione **PurelyHR**.

    ![Configurar o início de sessão único](./media/purelyhr-tutorial/tutorial_purelyhr_app.png) 

1. No menu à esquerda, clique em **utilizadores e grupos**.

    ![Atribuir utilizador][202] 

1. Clique em **adicionar** botão. Em seguida, selecione **utilizadores e grupos** nos **adicionar atribuição** caixa de diálogo.

    ![Atribuir utilizador][203]

1. No **utilizadores e grupos** caixa de diálogo, selecione **Eduarda Almeida** na lista utilizadores.

1. Clique em **selecionar** botão **utilizadores e grupos** caixa de diálogo.

1. Clique em **atribua** botão **adicionar atribuição** caixa de diálogo.
    
### <a name="testing-single-sign-on"></a>Teste de início de sessão único

Nesta secção, vai testar a configuração do Azure AD única início de sessão com o painel de acesso.

Clique no mosaico de LMS absorver no painel de acesso, obtém automaticamente com sessão iniciada para a sua aplicação de absorver LMS.

Para obter mais informações sobre o painel de acesso, consulte. [Introdução ao painel de acesso](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (O que é o acesso a aplicações e o início de sessão único com o Azure Active Directory?)



<!--Image references-->

[1]: ./media/purelyhr-tutorial/tutorial_general_01.png
[2]: ./media/purelyhr-tutorial/tutorial_general_02.png
[3]: ./media/purelyhr-tutorial/tutorial_general_03.png
[4]: ./media/purelyhr-tutorial/tutorial_general_04.png

[100]: ./media/purelyhr-tutorial/tutorial_general_100.png

[200]: ./media/purelyhr-tutorial/tutorial_general_200.png
[201]: ./media/purelyhr-tutorial/tutorial_general_201.png
[202]: ./media/purelyhr-tutorial/tutorial_general_202.png
[203]: ./media/purelyhr-tutorial/tutorial_general_203.png

