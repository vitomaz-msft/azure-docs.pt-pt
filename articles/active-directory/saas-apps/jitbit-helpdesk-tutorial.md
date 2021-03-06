---
title: 'Tutorial: Integração do Azure Active Directory com o suporte técnico de Jitbit | Documentos da Microsoft'
description: Saiba como configurar o início de sessão único entre o Azure Active Directory e o suporte técnico de Jitbit.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: 15ce27d4-0621-4103-8a34-e72c98d72ec3
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/28/2017
ms.author: jeedes
ms.openlocfilehash: 94ded0ef1bf77de20973a87a1ca2d6d1dd3fdf3f
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/02/2018
ms.locfileid: "39426657"
---
# <a name="tutorial-azure-active-directory-integration-with-jitbit-helpdesk"></a>Tutorial: Integração do Azure Active Directory com o suporte técnico de Jitbit

Neste tutorial, saiba como integrar o suporte técnico de Jitbit com o Azure Active Directory (Azure AD).

Integrar o suporte técnico de Jitbit com o Azure AD fornece as seguintes vantagens:

- Pode controlar no Azure AD que tenha acesso ao suporte técnico de Jitbit
- Pode permitir que os utilizadores automaticamente obter com sessão iniciada para o suporte técnico de Jitbit (Single Sign-On) com as suas contas do Azure AD
- Pode gerir as suas contas num local central – portal do Azure

Se quiser saber mais detalhes sobre a integração de aplicações SaaS com o Azure AD, veja [o que é o acesso a aplicações e início de sessão único com o Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com o suporte técnico de Jitbit, terá dos seguintes itens:

- Uma subscrição do Azure AD
- Um suporte técnico de Jitbit logon único habilitado subscrição

> [!NOTE]
> Para testar os passos neste tutorial, recomendamos que não utilize um ambiente de produção.

Para testar os passos neste tutorial, deve seguir estas recomendações:

- Não utilize o seu ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, pode obter uma versão de avaliação de um mês [aqui](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, vai testar do Azure AD início de sessão único num ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos de construção principais:

1. Adicionando suporte técnico de Jitbit da Galeria
1. Configuração e teste do Azure AD início de sessão único

## <a name="adding-jitbit-helpdesk-from-the-gallery"></a>Adicionando suporte técnico de Jitbit da Galeria
Para configurar a integração do suporte técnico de Jitbit com o Azure AD, terá de adicionar suporte técnico de Jitbit a partir da Galeria à sua lista de aplicações de SaaS geridas.

**Para adicionar suporte técnico de Jitbit a partir da galeria, execute os seguintes passos:**

1. Na  **[portal do Azure](https://portal.azure.com)**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone. 

    ![Active Directory][1]

1. Navegue para **aplicações empresariais**. Em seguida, aceda a **todos os aplicativos**.

    ![Aplicações][2]
    
1. Para adicionar nova aplicação, clique em **nova aplicação** botão na parte superior de caixa de diálogo.

    ![Aplicações][3]

1. Na caixa de pesquisa, escreva **suporte técnico de Jitbit**.

    ![Criar um utilizador de teste do Azure AD](./media/jitbit-helpdesk-tutorial/tutorial_jitbit-helpdesk_search.png)

1. No painel de resultados, selecione **suporte técnico de Jitbit**e, em seguida, clique em **Add** botão para adicionar a aplicação.

    ![Criar um utilizador de teste do Azure AD](./media/jitbit-helpdesk-tutorial/tutorial_jitbit-helpdesk_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configuração e teste do Azure AD início de sessão único
Nesta secção, configure e teste do Azure AD início de sessão único com o suporte técnico de Jitbit com base num utilizador de teste chamado "Eduarda Almeida".

Para o início de sessão único funcione, o Azure AD precisa saber qual é o utilizador de contraparte no suporte técnico de Jitbit a um utilizador no Azure AD. Em outras palavras, uma relação de ligação entre um utilizador do Azure AD e o utilizador relacionado no suporte técnico de Jitbit deve ser estabelecido.

No suporte técnico de Jitbit, atribuir o valor do **nome de utilizador** no Azure AD como o valor da **nome de utilizador** para estabelecer a relação de ligação.

Para configurar e testar o Azure AD início de sessão único com o suporte técnico de Jitbit, tem de concluir os seguintes blocos de construção:

1. **[Configurar o Azure AD início de sessão único](#configuring-azure-ad-single-sign-on)**  - para permitir que os utilizadores utilizar esta funcionalidade.
1. **[Criar um utilizador de teste do Azure AD](#creating-an-azure-ad-test-user)**  - para testar o Azure AD início de sessão único com Eduarda Almeida.
1. **[Criar um utilizador de teste de suporte técnico de Jitbit](#creating-a-jitbit-helpdesk-test-user)**  - para ter um equivalente da Eduarda Almeida no suporte técnico Jitbit que está ligado à representação de utilizador do Azure AD.
1. **[Atribuir o utilizador de teste do Azure AD](#assigning-the-azure-ad-test-user)**  - para ativar a Eduarda Almeida utilizar o Azure AD início de sessão único.
1. **[Teste de início de sessão único](#testing-single-sign-on)**  - para verificar se a configuração funciona.

### <a name="configuring-azure-ad-single-sign-on"></a>Configuração do Azure AD início de sessão único

Nesta secção, pode ativar o Azure AD início de sessão único no portal do Azure e configurar início de sessão único em seu aplicativo de suporte técnico de Jitbit.

**Para configurar o Azure AD início de sessão único com o suporte técnico de Jitbit, execute os seguintes passos:**

1. No portal do Azure, sobre o **Jitbit Helpdesk** página de integração de aplicativo, clique em **início de sessão único**.

    ![Configurar o início de sessão único][4]

1. Sobre o **início de sessão único** caixa de diálogo, selecione **modo** como **baseado em SAML logon** para ativar o início de sessão único.
 
    ![Configurar o início de sessão único](./media/jitbit-helpdesk-tutorial/tutorial_jitbit-helpdesk_samlbase.png)

1. Sobre o **Jitbit suporte técnico domínio e URLs** secção, execute os seguintes passos:

    ![Configurar o início de sessão único](./media/jitbit-helpdesk-tutorial/tutorial_jitbit-helpdesk_url.png)

    a. Na **URL de início de sessão** caixa de texto, escreva um URL com o seguinte padrão: 
    | |     
    | ----------------------------------------|
    | `https://<hostname>/helpdesk/User/Login`|
    | `https://<tenant-name>.Jitbit.com`|
    | |
    
    > [!NOTE] 
    > Este valor não é real. Atualize este valor com o URL de início de sessão real. Contacte [equipa de suporte de cliente de suporte técnico Jitbit](https://www.jitbit.com/support/) para obter este valor. 
    
    b.  Na **identificador** caixa de texto, escreva um URL seguinte: `https://www.jitbit.com/web-helpdesk/`

    
 


1. Sobre o **certificado de assinatura SAML** secção, clique em **Certificate(Base64)** e, em seguida, guarde o ficheiro de certificado no seu computador.

    ![Configurar o início de sessão único](./media/jitbit-helpdesk-tutorial/tutorial_jitbit-helpdesk_certificate.png) 

1. Clique em **guardar** botão.

    ![Configurar o início de sessão único](./media/jitbit-helpdesk-tutorial/tutorial_general_400.png)

1. Sobre o **Jitbit configuração de suporte técnico** secção, clique em **configurar a assistência técnica de Jitbit** para abrir **configurar início de sessão** janela. Cópia a **SAML único início de sessão no URL do serviço** partir o **secção de referência rápida.**

    ![Configurar o início de sessão único](./media/jitbit-helpdesk-tutorial/tutorial_jitbit-helpdesk_configure.png) 

1. Numa janela do browser web diferente, inicie sessão no site da sua empresa Jitbit Helpdesk como um administrador.

1. Na barra de ferramentas na parte superior, clique em **administração**.
   
    ![Administração](./media/jitbit-helpdesk-tutorial/ic777681.png "administração")

1. Clique em **definições gerais**.
   
    ![Os utilizadores, empresas e permissões](./media/jitbit-helpdesk-tutorial/ic777680.png "utilizadores, as empresas e permissões")

1. Na **definições de autenticação** configuração secção, execute os seguintes passos:
   
    ![Definições de autenticação](./media/jitbit-helpdesk-tutorial/ic777683.png "definições de autenticação")
    
    a. Selecione **ativar SAML 2.0 único início de sessão**, para iniciar sessão com o único início de sessão (SSO), com **OneLogin**.

    b. Na **URL de ponto final** caixa de texto, cole o valor de **SAML único início de sessão no URL do serviço** que copiou do portal do Azure.

    c. Abra sua **base 64** codificado de certificado no bloco de notas, copie o conteúdo do mesmo para a área de transferência e, em seguida, cole-os para o **certificado X.509** caixa de texto

    d. Clique em **guardar alterações**.

> [!TIP]
> Agora pode ler uma versão concisa destas instruções dentro do [portal do Azure](https://portal.azure.com), enquanto estiver a configurar a aplicação!  Depois de adicionar esta aplicação a partir da **do Active Directory > aplicações empresariais** secção, basta clicar o **Single Sign-On** separador e a documentação do embedded através de acesso a  **Configuração** seção na parte inferior. Pode ler mais sobre a funcionalidade de documentação do embedded aqui: [documentação do embedded do Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD
O objetivo desta secção é criar um utilizador de teste no portal do Azure chamado Eduarda Almeida.

![Criar utilizador do Azure AD][100]

**Para criar um utilizador de teste no Azure AD, execute os seguintes passos:**

1. Na **portal do Azure**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone.

    ![Criar um utilizador de teste do Azure AD](./media/jitbit-helpdesk-tutorial/create_aaduser_01.png) 

1. Para apresentar a lista de utilizadores, aceda a **utilizadores e grupos** e clique em **todos os utilizadores**.
    
    ![Criar um utilizador de teste do Azure AD](./media/jitbit-helpdesk-tutorial/create_aaduser_02.png) 

1. Para abrir o **usuário** caixa de diálogo, clique em **Add** na parte superior da caixa de diálogo.
 
    ![Criar um utilizador de teste do Azure AD](./media/jitbit-helpdesk-tutorial/create_aaduser_03.png) 

1. Sobre o **utilizador** caixa de diálogo página, execute os seguintes passos:
 
    ![Criar um utilizador de teste do Azure AD](./media/jitbit-helpdesk-tutorial/create_aaduser_04.png) 

    a. Na **Name** caixa de texto, nome do tipo **BrittaSimon**.

    b. Na **nome de utilizador** caixa de texto, tipo a **endereço de e-mail** de BrittaSimon.

    c. Selecione **mostrar palavra-passe** e indique o valor da **palavra-passe**.

    d. Clique em **Criar**.
 
### <a name="creating-a-jitbit-helpdesk-test-user"></a>Criar um utilizador de teste de suporte técnico de Jitbit

Para habilitar os utilizadores do Azure AD iniciar sessão no suporte técnico de Jitbit, tem de ser aprovisionados no suporte técnico de Jitbit.  No caso de suporte técnico de Jitbit, o aprovisionamento é uma tarefa manual.

**Para Aprovisionar uma conta de utilizador, execute os seguintes passos:**

1. Inicie sessão no seu **suporte técnico de Jitbit** inquilino.

1. No menu na parte superior, clique em **administração**.
   
    ![Administração](./media/jitbit-helpdesk-tutorial/ic777681.png "administração")

1. Clique em **utilizadores, as empresas e permissões**.
   
    ![Os utilizadores, empresas e permissões](./media/jitbit-helpdesk-tutorial/ic777682.png "utilizadores, as empresas e permissões")

1. Clique em **adicionar utilizador**.
   
    ![Adicionar utilizador](./media/jitbit-helpdesk-tutorial/ic777685.png "adicionar utilizador")
   
1. Na secção de criar, escreva os dados da conta do Azure AD que pretende aprovisionar da seguinte forma:

    ![Crie](./media/jitbit-helpdesk-tutorial/ic777686.png "criar")
   
   a. Na **nome de utilizador** caixa de texto, tipo **BrittaSimon**, o nome de utilizador como no portal do Azure.

   b. Na **E-Mail** caixa de texto, como o tipo de e-mail do utilizador **BrittaSimon@contoso.com**.

   c. Na **nome próprio** caixa de texto, nome do utilizador, como o tipo **Eduarda**.

   d. Na **sobrenome** caixa de texto, último nome de tipo do utilizador, como **Simon**.
   
   e. Clique em **Criar**.

>[!NOTE]
>Pode utilizar quaisquer outras ferramentas de criação da conta de utilizador de suporte técnico de Jitbit ou APIs fornecidas pelo suporte técnico de Jitbit para aprovisionar contas de utilizador do Azure AD.
> 
        

### <a name="assigning-the-azure-ad-test-user"></a>Atribuir o utilizador de teste do Azure AD

Nesta secção, vai ativar Eduarda Almeida utilizar o Azure início de sessão único ao conceder acesso ao suporte técnico de Jitbit.

![Atribuir utilizador][200] 

**Para atribuir a Eduarda Almeida a assistência técnica de Jitbit, execute os seguintes passos:**

1. No portal do Azure, abra a vista de aplicativos e, em seguida, navegue para a vista de diretório e aceda a **aplicações empresariais** , em seguida, clique em **todos os aplicativos**.

    ![Atribuir utilizador][201] 

1. Na lista de aplicações, selecione **suporte técnico de Jitbit**.

    ![Configurar o início de sessão único](./media/jitbit-helpdesk-tutorial/tutorial_jitbit-helpdesk_app.png) 

1. No menu à esquerda, clique em **utilizadores e grupos**.

    ![Atribuir utilizador][202] 

1. Clique em **adicionar** botão. Em seguida, selecione **utilizadores e grupos** nos **adicionar atribuição** caixa de diálogo.

    ![Atribuir utilizador][203]

1. No **utilizadores e grupos** caixa de diálogo, selecione **Eduarda Almeida** na lista utilizadores.

1. Clique em **selecionar** botão **utilizadores e grupos** caixa de diálogo.

1. Clique em **atribua** botão **adicionar atribuição** caixa de diálogo.
    
### <a name="testing-single-sign-on"></a>Teste de início de sessão único

Nesta secção, vai testar a configuração do Azure AD única início de sessão com o painel de acesso.

Quando clica no mosaico de suporte técnico de Jitbit no painel de acesso, deve obter a página de início de sessão de assistência técnica de Jitbit aplicativo.
Para obter mais informações sobre o painel de acesso, consulte [introdução ao painel de acesso](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (O que é o acesso a aplicações e o início de sessão único com o Azure Active Directory?)



<!--Image references-->

[1]: ./media/jitbit-helpdesk-tutorial/tutorial_general_01.png
[2]: ./media/jitbit-helpdesk-tutorial/tutorial_general_02.png
[3]: ./media/jitbit-helpdesk-tutorial/tutorial_general_03.png
[4]: ./media/jitbit-helpdesk-tutorial/tutorial_general_04.png

[100]: ./media/jitbit-helpdesk-tutorial/tutorial_general_100.png

[200]: ./media/jitbit-helpdesk-tutorial/tutorial_general_200.png
[201]: ./media/jitbit-helpdesk-tutorial/tutorial_general_201.png
[202]: ./media/jitbit-helpdesk-tutorial/tutorial_general_202.png
[203]: ./media/jitbit-helpdesk-tutorial/tutorial_general_203.png

