---
title: 'Tutorial: Integração do Azure Active Directory com O.C. Tanner - AppreciateHub | Documentos da Microsoft'
description: Saiba como configurar o início de sessão único entre o Azure Active Directory e O.C. Tanner - AppreciateHub.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: dee8fbca-0b60-4a21-8917-1fb6919de5a0
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/09/2017
ms.author: jeedes
ms.openlocfilehash: 2a3c6641c3fd9402ede2176e3c5c3f3ec15ed9de
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/02/2018
ms.locfileid: "39438710"
---
# <a name="tutorial-azure-active-directory-integration-with-oc-tanner---appreciatehub"></a>Tutorial: Integração do Azure Active Directory com O.C. Tanner - AppreciateHub

Neste tutorial, saiba como integrar O.C. Tanner - AppreciateHub com o Azure Active Directory (Azure AD).

Integrar O.C. Tanner - AppreciateHub com o Azure AD fornece as seguintes vantagens:

- Pode controlar no Azure AD que tenha acesso ao O.C. Tanner - AppreciateHub
- Pode permitir que os utilizadores automaticamente obter com sessão iniciada para O.C. Tanner - AppreciateHub (Single Sign-On) com as suas contas do Azure AD
- Pode gerir as suas contas num local central – portal do Azure

Se quiser saber mais detalhes sobre a integração de aplicações SaaS com o Azure AD, veja [o que é o acesso a aplicações e início de sessão único com o Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com O.C. Tanner - AppreciateHub, precisa dos seguintes itens:

- Uma subscrição do Azure AD
- UM O.C. Tanner - AppreciateHub logon único habilitado subscrição

> [!NOTE]
> Para testar os passos neste tutorial, recomendamos que não utilize um ambiente de produção.

Para testar os passos neste tutorial, deve seguir estas recomendações:

- Não utilize o seu ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, pode obter uma versão de avaliação de um mês [aqui](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, vai testar do Azure AD início de sessão único num ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos de construção principais:

1. Adicionar O.C. Tanner - AppreciateHub a partir da Galeria
1. Configuração e teste do Azure AD início de sessão único

## <a name="adding-oc-tanner---appreciatehub-from-the-gallery"></a>Adicionar O.C. Tanner - AppreciateHub a partir da Galeria
Para configurar a integração do O.C. Tanner - AppreciateHub com o Azure AD, tem de adicionar O.C. Tanner - AppreciateHub partir da galeria, à sua lista de aplicações de SaaS geridas.

**Para adicionar O.C. Tanner - AppreciateHub a partir da galeria, execute os seguintes passos:**

1. Na  **[portal do Azure](https://portal.azure.com)**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone. 

    ![Active Directory][1]

1. Navegue para **aplicações empresariais**. Em seguida, aceda a **todos os aplicativos**.

    ![Aplicações][2]
    
1. Para adicionar nova aplicação, clique em **nova aplicação** botão na parte superior de caixa de diálogo.

    ![Aplicações][3]

1. Na caixa de pesquisa, escreva **O.C. Tanner - AppreciateHub**.

    ![Criar um utilizador de teste do Azure AD](./media/oc-tanner-tutorial/tutorial_octannerappreciatehub_search.png)

1. No painel de resultados, selecione **O.C. Tanner - AppreciateHub**e, em seguida, clique em **adicionar** botão para adicionar a aplicação.

    ![Criar um utilizador de teste do Azure AD](./media/oc-tanner-tutorial/tutorial_octannerappreciatehub_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configuração e teste do Azure AD início de sessão único
Nesta secção, configurar e testar o Azure AD início de sessão único com O.C. Tanner - AppreciateHub com base num utilizador de teste chamado "Eduarda Almeida".

Para o início de sessão único funcione, o Azure AD precisa de saber o que o utilizador de contraparte O.C. Tanner - AppreciateHub é um utilizador no Azure AD. Em outras palavras, uma relação de ligação entre um utilizador do Azure AD e o utilizador relacionado no O.C. Tanner - AppreciateHub deve ser estabelecido.

No O.C. Tanner - AppreciateHub, atribuir o valor do **nome de utilizador** no Azure AD como o valor da **Username** para estabelecer a relação de ligação.

Para configurar e testar o Azure AD início de sessão único com O.C. Tanner - AppreciateHub, tem de concluir os seguintes blocos de construção:

1. **[Configurar o Azure AD início de sessão único](#configuring-azure-ad-single-sign-on)**  - para permitir que os utilizadores utilizar esta funcionalidade.
1. **[Criar um utilizador de teste do Azure AD](#creating-an-azure-ad-test-user)**  - para testar o Azure AD início de sessão único com Eduarda Almeida.
1. **[Criar um O.C. Tanner - utilizador de teste AppreciateHub](#creating-a-oc-tanner---appreciatehub-test-user)**  - para ter um equivalente da Eduarda Almeida na O.C. Tanner - AppreciateHub que está ligado à representação de utilizador do Azure AD.
1. **[Atribuir o utilizador de teste do Azure AD](#assigning-the-azure-ad-test-user)**  - para ativar a Eduarda Almeida utilizar o Azure AD início de sessão único.
1. **[Teste de início de sessão único](#testing-single-sign-on)**  - para verificar se a configuração funciona.

### <a name="configuring-azure-ad-single-sign-on"></a>Configuração do Azure AD início de sessão único

Nesta secção, ativar o Azure AD início de sessão único no portal do Azure e configurar o início de sessão único no seu O.C. Tanner - AppreciateHub aplicação.

**Para configurar o Azure AD início de sessão único com O.C. Tanner - AppreciateHub, execute os seguintes passos:**

1. No portal do Azure, sobre o **O.C. Tanner - AppreciateHub** página de integração de aplicação, clique em **início de sessão único**.

    ![Configurar o início de sessão único][4]

1. Sobre o **início de sessão único** caixa de diálogo, selecione **modo** como **baseado em SAML logon** para ativar o início de sessão único.
 
    ![Configurar o início de sessão único](./media/oc-tanner-tutorial/tutorial_octannerappreciatehub_samlbase.png)

1. Sobre o **O.C. Tanner - AppreciateHub domínio e URLs** secção, execute os seguintes passos:

    ![Configurar o início de sessão único](./media/oc-tanner-tutorial/tutorial_octannerappreciatehub_url.png)

    a. Na **URL de resposta** caixa de texto, escreva um URL com o seguinte padrão: `https://<companyname>.octanner.net/sp/ACS.saml2`

    > [!NOTE] 
    > Este valor não é real. Atualize este valor com o URL de resposta real. Contacto [O.C. Tanner - equipa de suporte de AppreciateHub](mailto:sso@octanner.com) para obter este valor.

    b. Abra o ficheiro de metadados através da seguinte ligação: [ https://fed.appreciatehub.com/fed/sp/metadata ](https://fed.appreciatehub.com/fed/sp/metadata).
   
    c. Localize a **md:AssertionConsumerService** nó. 
   
    d. Copie o valor do **localização** atributo. 
   
    ![Configurar definições da aplicação][12]
   
    e. Na **URL de início de sessão** caixa de texto, após o valor de obteve no passo anterior.

1. Sobre o **certificado de assinatura SAML** secção, clique em **XML de metadados** e, em seguida, guarde o ficheiro de metadados no seu computador.

    ![Configurar o início de sessão único](./media/oc-tanner-tutorial/tutorial_octannerappreciatehub_certificate.png) 

1. Clique em **guardar** botão.

    ![Configurar o início de sessão único](./media/oc-tanner-tutorial/tutorial_general_400.png)

1. Para configurar o início de sessão único em **O.C. Tanner - AppreciateHub** lado, terá de enviar o transferido **XML de metadados** para [O.C. Tanner - equipa de suporte de AppreciateHub](mailto:sso@octanner.com).

> [!TIP]
> Agora pode ler uma versão concisa destas instruções dentro do [portal do Azure](https://portal.azure.com), enquanto estiver a configurar a aplicação!  Depois de adicionar esta aplicação a partir da **do Active Directory > aplicações empresariais** secção, basta clicar o **Single Sign-On** separador e a documentação do embedded através de acesso a  **Configuração** seção na parte inferior. Pode ler mais sobre a funcionalidade de documentação do embedded aqui: [documentação do embedded do Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD
O objetivo desta secção é criar um utilizador de teste no portal do Azure chamado Eduarda Almeida.

![Criar utilizador do Azure AD][100]

**Para criar um utilizador de teste no Azure AD, execute os seguintes passos:**

1. Na **portal do Azure**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone.

    ![Criar um utilizador de teste do Azure AD](./media/oc-tanner-tutorial/create_aaduser_01.png) 

1. Para apresentar a lista de utilizadores, aceda a **utilizadores e grupos** e clique em **todos os utilizadores**.
    
    ![Criar um utilizador de teste do Azure AD](./media/oc-tanner-tutorial/create_aaduser_02.png) 

1. Para abrir o **usuário** caixa de diálogo, clique em **Add** na parte superior da caixa de diálogo.
 
    ![Criar um utilizador de teste do Azure AD](./media/oc-tanner-tutorial/create_aaduser_03.png) 

1. Sobre o **utilizador** caixa de diálogo página, execute os seguintes passos:
 
    ![Criar um utilizador de teste do Azure AD](./media/oc-tanner-tutorial/create_aaduser_04.png) 

    a. Na **Name** caixa de texto, tipo **BrittaSimon**.

    b. Na **nome de utilizador** caixa de texto, tipo a **endereço de e-mail** de BrittaSimon.

    c. Selecione **mostrar palavra-passe** e indique o valor da **palavra-passe**.

    d. Clique em **Criar**.
 
### <a name="creating-a-oc-tanner---appreciatehub-test-user"></a>Criar um O.C. Tanner - AppreciateHub utilizador de teste

O objetivo desta secção consiste em criar um usuário chamado Eduarda Almeida no O.C. Tanner - AppreciateHub.

**Para criar um usuário chamado Eduarda Almeida no O.C. Tanner - AppreciateHub, execute os seguintes passos:**

Peça ao seu [O.C. Tanner - equipa de suporte de AppreciateHub](mailto:sso@octanner.com) para criar um utilizador que tenha como atributo nameID o mesmo valor que o nome de utilizador da Eduarda Almeida no Azure AD.

### <a name="assigning-the-azure-ad-test-user"></a>Atribuir o utilizador de teste do Azure AD

Nesta secção, vai ativar a Eduarda Almeida utilizar o Azure início de sessão único ao conceder acesso para O.C. Tanner - AppreciateHub.

![Atribuir utilizador][200] 

**Para atribuir a Eduarda Almeida a O.C. Tanner - AppreciateHub, execute os seguintes passos:**

1. No portal do Azure, abra a vista de aplicativos e, em seguida, navegue para a vista de diretório e aceda a **aplicações empresariais** , em seguida, clique em **todos os aplicativos**.

    ![Atribuir utilizador][201] 

1. Na lista de aplicações, selecione **O.C. Tanner - AppreciateHub**.

    ![Configurar o início de sessão único](./media/oc-tanner-tutorial/tutorial_octannerappreciatehub_app.png) 

1. No menu à esquerda, clique em **utilizadores e grupos**.

    ![Atribuir utilizador][202] 

1. Clique em **adicionar** botão. Em seguida, selecione **utilizadores e grupos** nos **adicionar atribuição** caixa de diálogo.

    ![Atribuir utilizador][203]

1. No **utilizadores e grupos** caixa de diálogo, selecione **Eduarda Almeida** na lista utilizadores.

1. Clique em **selecionar** botão **utilizadores e grupos** caixa de diálogo.

1. Clique em **atribua** botão **adicionar atribuição** caixa de diálogo.
    
### <a name="testing-single-sign-on"></a>Teste de início de sessão único

O objetivo desta secção é testar a configuração do Azure AD única início de sessão com o painel de acesso.  
Quando clica no O.C. Tanner - mosaico AppreciateHub no painel de acesso, deve obter automaticamente com sessão iniciada para sua O.C. Tanner - AppreciateHub aplicação.

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (O que é o acesso a aplicações e o início de sessão único com o Azure Active Directory?)

<!--Image references-->

[1]: ./media/oc-tanner-tutorial/tutorial_general_01.png
[2]: ./media/oc-tanner-tutorial/tutorial_general_02.png
[3]: ./media/oc-tanner-tutorial/tutorial_general_03.png
[4]: ./media/oc-tanner-tutorial/tutorial_general_04.png

[12]: ./media/oc-tanner-tutorial/tutorial_octanner_08.png

[100]: ./media/oc-tanner-tutorial/tutorial_general_100.png

[200]: ./media/oc-tanner-tutorial/tutorial_general_200.png
[201]: ./media/oc-tanner-tutorial/tutorial_general_201.png
[202]: ./media/oc-tanner-tutorial/tutorial_general_202.png
[203]: ./media/oc-tanner-tutorial/tutorial_general_203.png

