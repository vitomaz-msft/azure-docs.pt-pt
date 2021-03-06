---
title: 'Tutorial: Integração do Azure Active Directory com Envi MMIS | Documentos da Microsoft'
description: Saiba como configurar o início de sessão único entre o Azure Active Directory e Envi MMIS.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: ab89f8ee-2507-4625-94bc-b24ef3d5e006
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/03/2018
ms.author: jeedes
ms.openlocfilehash: 96168dcb8400d2580d0b64257ceb861c1da3ff65
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/02/2018
ms.locfileid: "39447290"
---
# <a name="tutorial-azure-active-directory-integration-with-envi-mmis"></a>Tutorial: Integração do Azure Active Directory com Envi MMIS

Neste tutorial, saiba como integrar Envi MMIS com o Azure Active Directory (Azure AD).

Integrar Envi MMIS no Azure AD fornece as seguintes vantagens:

- Pode controlar no Azure AD que tenha acesso ao Envi MMIS.
- Pode permitir que os utilizadores automaticamente obter com sessão iniciada para Envi MMIS (Single Sign-On) com as suas contas do Azure AD.
- Pode gerir as suas contas num local central – portal do Azure.

Se quiser saber mais detalhes sobre a integração de aplicações SaaS com o Azure AD, veja [o que é o acesso a aplicações e início de sessão único com o Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com Envi MMIS, terá dos seguintes itens:

- Uma subscrição do Azure AD
- Um Envi MMIS logon único habilitado subscrição

> [!NOTE]
> Para testar os passos neste tutorial, recomendamos que não utilize um ambiente de produção.

Para testar os passos neste tutorial, deve seguir estas recomendações:

- Não utilize o seu ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, pode [obtenha uma avaliação de um mês](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, vai testar do Azure AD início de sessão único num ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos de construção principais:

1. Adicionando Envi MMIS da Galeria
1. Configuração e teste do Azure AD início de sessão único

## <a name="adding-envi-mmis-from-the-gallery"></a>Adicionando Envi MMIS da Galeria
Para configurar a integração do Envi MMIS com o Azure AD, terá de adicionar Envi MMIS a partir da Galeria à sua lista de aplicações de SaaS geridas.

**Para adicionar Envi MMIS a partir da galeria, execute os seguintes passos:**

1. Na  **[portal do Azure](https://portal.azure.com)**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone. 

    ![O botão do Azure Active Directory][1]

1. Navegue para **aplicações empresariais**. Em seguida, aceda a **todos os aplicativos**.

    ![O painel de aplicações empresariais][2]
    
1. Para adicionar nova aplicação, clique em **nova aplicação** botão na parte superior de caixa de diálogo.

    ![O novo botão de aplicativo][3]

1. Na caixa de pesquisa, escreva **Envi MMIS**, selecione **Envi MMIS** no painel de resultados, em seguida, clique em **Add** botão para adicionar a aplicação.

    ![Envi MMIS na lista de resultados](./media/envimmis-tutorial/tutorial_envimmis_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configurar e testar o Azure AD início de sessão único

Nesta secção, configure e teste do Azure AD início de sessão único com Envi MMIS com base num utilizador de teste chamado "Eduarda Almeida".

Para o início de sessão único funcione, o Azure AD precisa saber qual é o utilizador de contraparte no Envi MMIS a um utilizador no Azure AD. Em outras palavras, uma relação de ligação entre um utilizador do Azure AD e o utilizador relacionado no Envi MMIS deve ser estabelecido.

Para configurar e testar o Azure AD início de sessão único com Envi MMIS, tem de concluir os seguintes blocos de construção:

1. **[Configurar o Azure AD início de sessão único](#configure-azure-ad-single-sign-on)**  - para permitir que os utilizadores utilizar esta funcionalidade.
1. **[Criar um utilizador de teste do Azure AD](#create-an-azure-ad-test-user)**  - para testar o Azure AD início de sessão único com Eduarda Almeida.
1. **[Criar um utilizador de teste Envi MMIS](#create-an-envi-mmis-test-user)**  - para ter um equivalente da Eduarda Almeida na MMIS Envi que está ligado à representação de utilizador do Azure AD.
1. **[Atribua o utilizador de teste do Azure AD](#assign-the-azure-ad-test-user)**  - para ativar a Eduarda Almeida utilizar o Azure AD início de sessão único.
1. **[Testar início de sessão único](#test-single-sign-on)**  - para verificar se a configuração funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configurar o Azure AD início de sessão único

Nesta secção, pode ativar o Azure AD início de sessão único no portal do Azure e configurar início de sessão único em seu aplicativo Envi MMIS.

**Para configurar o Azure AD início de sessão único com Envi MMIS, execute os seguintes passos:**

1. No portal do Azure, sobre o **Envi MMIS** página de integração de aplicação, clique em **início de sessão único**.

    ![Configurar a ligação de início de sessão única][4]

1. Sobre o **início de sessão único** caixa de diálogo, selecione **modo** como **baseado em SAML logon** para ativar o início de sessão único.
 
    ![Caixa de diálogo de início de sessão único](./media/envimmis-tutorial/tutorial_envimmis_samlbase.png)

1. Sobre o **Envi MMIS domínio e URLs** secção, execute os seguintes passos, se desejar configurar a aplicação no **IDP** iniciada pelo modo:

    ![Envi MMIS domínio e URLs únicas início de sessão em informações](./media/envimmis-tutorial/tutorial_envimmis_url.png)

    a. Na **identificador** caixa de texto, escreva um URL com o seguinte padrão: `https://www.<CUSTOMER DOMAIN>.com/Account`

    b. Na **URL de resposta** caixa de texto, escreva um URL com o seguinte padrão: `https://www.<CUSTOMER DOMAIN>.com/Account/Acs`

1. Verifique **Mostrar definições de URL avançadas** e executar o passo seguinte, se desejar configurar a aplicação na **SP** iniciada pelo modo:

    ![Envi MMIS domínio e URLs únicas início de sessão em informações](./media/envimmis-tutorial/tutorial_envimmis_url1.png)

    Na **URL de início de sessão** caixa de texto, escreva um URL com o seguinte padrão: `https://www.<CUSTOMER DOMAIN>.com/Account`
     
    > [!NOTE]
    > Estes valores não são reais. Atualize estes valores com o identificador de real, a URL de resposta e o URL de início de sessão. Contacte [equipa de suporte de cliente de MMIS Envi](mailto:support@ioscorp.com) obter esses valores.

1. Sobre o **certificado de assinatura SAML** secção, clique em **XML de metadados** e, em seguida, guarde o ficheiro de metadados no seu computador.

    ![O link de download de certificado](./media/envimmis-tutorial/tutorial_envimmis_certificate.png) 

1. Clique em **guardar** botão.

    ![Configurar o botão único início de sessão em Guardar](./media/envimmis-tutorial/tutorial_general_400.png)

1. Numa janela do browser web diferente, inicie sessão no seu site Envi MMIS como um administrador.

1. Clique em **meu domínio** separador.

    ![Configurar o botão único início de sessão em Guardar](./media/envimmis-tutorial/configure1.png)

1. Clique em **Editar**.

    ![Configurar o botão único início de sessão em Guardar](./media/envimmis-tutorial/configure2.png)

1. Selecione **utilizar a autenticação remota** caixa de verificação e, em seguida, selecione **redirecionamento de HTTP** partir o **tipo de autenticação** lista pendente.

    ![Configurar o botão único início de sessão em Guardar](./media/envimmis-tutorial/configure3.png)

1. Selecione **recursos** separador e, em seguida, clique em **carregar metadados**.

    ![Configurar o botão único início de sessão em Guardar](./media/envimmis-tutorial/configure4.png)

1. Na **carregar metadados** pop-up, execute os seguintes passos:

    ![Configurar o botão único início de sessão em Guardar](./media/envimmis-tutorial/configure5.png)

    a. Selecione **arquivo** opção da **carregar de** lista pendente.

    b. Carregar o ficheiro de metadados baixado a partir do portal do Azure ao selecionar o **escolha o ícone de arquivo**.

    c. Clique em **OK**.

1. Depois de carregar o ficheiro de metadados baixado os campos irão obter preenchidos automaticamente. Clique em **Update**

    ![Configurar o botão único início de sessão em Guardar](./media/envimmis-tutorial/configure6.png)

### <a name="create-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD

O objetivo desta secção é criar um utilizador de teste no portal do Azure chamado Eduarda Almeida.

   ![Criar um utilizador de teste do Azure AD][100]

**Para criar um utilizador de teste no Azure AD, execute os seguintes passos:**

1. No portal do Azure, no painel esquerdo, clique nas **do Azure Active Directory** botão.

    ![O botão do Azure Active Directory](./media/envimmis-tutorial/create_aaduser_01.png)

1. Para apresentar a lista de utilizadores, aceda a **utilizadores e grupos**e, em seguida, clique em **todos os utilizadores**.

    !["Os utilizadores e grupos" e os links de "Todos os utilizadores"](./media/envimmis-tutorial/create_aaduser_02.png)

1. Para abrir o **usuário** caixa de diálogo, clique em **Add** na parte superior a **todos os utilizadores** caixa de diálogo.

    ![Botão Adicionar](./media/envimmis-tutorial/create_aaduser_03.png)

1. Na **utilizador** diálogo caixa, execute os seguintes passos:

    ![A caixa de diálogo de utilizador](./media/envimmis-tutorial/create_aaduser_04.png)

    a. Na **Name** , escreva **BrittaSimon**.

    b. Na **nome de utilizador** , escreva o endereço de e-mail do utilizador Eduarda Almeida.

    c. Selecione o **mostrar palavra-passe** caixa de verificação e, em seguida, anote o valor que é apresentado na **palavra-passe** caixa.

    d. Clique em **Criar**.
 
### <a name="create-an-envi-mmis-test-user"></a>Criar um utilizador de teste Envi MMIS

Para ativar a utilizadores do Azure AD iniciar sessão no Envi MMIS, tem de ser aprovisionados em Envi MMIS.  
No caso de Envi MMIS, o aprovisionamento é uma tarefa manual.

**Para Aprovisionar uma conta de utilizador, execute os seguintes passos:**

1. Inicie sessão no site da sua empresa Envi MMIS como administrador.

1. Clique em **lista de utilizadores** separador.

    ![Adicionar o funcionário](./media/envimmis-tutorial/user1.png)

1. Clique em **adicionar utilizador** botão.

    ![Adicionar o funcionário](./media/envimmis-tutorial/user2.png)

1. Na **adicionar utilizador** secção, execute os seguintes passos:

    ![Adicionar o funcionário](./media/envimmis-tutorial/user3.png)

    a. Na **nome de utilizador** caixa de texto, como o tipo o nome de utilizador da conta da Eduarda Almeida **brittasimon@contoso.com**.
    
    b. Na **nome próprio** como a caixa de texto, o primeiro nome typu BrittaSimon **Eduarda**.

    c. Na **sobrenome** caixa de texto, como o tipo o sobrenome de BrittaSimon **Simon**.

    d. Introduza o título do utilizador no **Title** da caixa de texto.
    
    e. Na **endereço de E-Mail** caixa de texto, como o tipo de endereço de e-mail da conta da Eduarda Almeida **brittasimon@contoso.com**.

    f. Na **nome de utilizador de SSO** caixa de texto, como o tipo o nome de utilizador da conta da Eduarda Almeida **brittasimon@contoso.com**.

    g. Clique em **Guardar**.

### <a name="assign-the-azure-ad-test-user"></a>Atribua o utilizador de teste do Azure AD

Nesta secção, vai ativar Eduarda Almeida utilizar o Azure início de sessão único ao conceder acesso para Envi MMIS.

![Atribuir a função de utilizador][200] 

**Para atribuir a Eduarda Almeida a Envi MMIS, execute os seguintes passos:**

1. No portal do Azure, abra a vista de aplicativos e, em seguida, navegue para a vista de diretório e aceda a **aplicações empresariais** , em seguida, clique em **todos os aplicativos**.

    ![Atribuir utilizador][201] 

1. Na lista de aplicações, selecione **Envi MMIS**.

    ![A ligação de Envi MMIS na lista de aplicações](./media/envimmis-tutorial/tutorial_envimmis_app.png)  

1. No menu à esquerda, clique em **utilizadores e grupos**.

    ![A ligação "Utilizadores e grupos"][202]

1. Clique em **adicionar** botão. Em seguida, selecione **utilizadores e grupos** nos **adicionar atribuição** caixa de diálogo.

    ![O painel Adicionar atribuição][203]

1. No **utilizadores e grupos** caixa de diálogo, selecione **Eduarda Almeida** na lista utilizadores.

1. Clique em **selecionar** botão **utilizadores e grupos** caixa de diálogo.

1. Clique em **atribua** botão **adicionar atribuição** caixa de diálogo.
    
### <a name="test-single-sign-on"></a>Testar início de sessão único

Nesta secção, vai testar a configuração do Azure AD única início de sessão com o painel de acesso.

Ao clicar no mosaico Envi MMIS no painel de acesso, deve obter automaticamente sessão iniciada em seu aplicativo Envi MMIS.
Para obter mais informações sobre o painel de acesso, consulte [introdução ao painel de acesso](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (O que é o acesso a aplicações e o início de sessão único com o Azure Active Directory?)



<!--Image references-->

[1]: ./media/envimmis-tutorial/tutorial_general_01.png
[2]: ./media/envimmis-tutorial/tutorial_general_02.png
[3]: ./media/envimmis-tutorial/tutorial_general_03.png
[4]: ./media/envimmis-tutorial/tutorial_general_04.png

[100]: ./media/envimmis-tutorial/tutorial_general_100.png

[200]: ./media/envimmis-tutorial/tutorial_general_200.png
[201]: ./media/envimmis-tutorial/tutorial_general_201.png
[202]: ./media/envimmis-tutorial/tutorial_general_202.png
[203]: ./media/envimmis-tutorial/tutorial_general_203.png

