---
title: 'Tutorial: Integração do Azure Active Directory com as vídeo 23 | Microsoft Docs'
description: Saiba como configurar o início de sessão entre o Azure Active Directory e as vídeo 23.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: 5e73dd1d-3995-4a73-b9cf-1b2318d49cb3
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/26/2017
ms.author: jeedes
ms.openlocfilehash: 8b4b41551a1679948518846a63eee87bbd1bbfd9
ms.sourcegitcommit: 16ddc345abd6e10a7a3714f12780958f60d339b6
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/19/2018
ms.locfileid: "36222670"
---
# <a name="tutorial-azure-active-directory-integration-with-23-video"></a>Tutorial: Integração do Azure Active Directory com as vídeo 23

Neste tutorial, irá aprender a integrar as vídeo 23 com o Azure Active Directory (Azure AD).

Integrar 23 vídeo com o Azure AD fornece as seguintes vantagens:

- Pode controlar no Azure AD que tenha acesso ao vídeo 23
- Pode permitir que os utilizadores automaticamente obter com sessão iniciada para as vídeo 23 (Single Sign-On) com as respetivas contas do Azure AD
- Pode gerir as contas numa localização central - portal do Azure

Se pretender saber mais detalhes sobre a integração de aplicações SaaS com o Azure AD, consulte o artigo [que é o acesso a aplicações e início de sessão no Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com as vídeo 23, terá dos seguintes itens:

- Uma subscrição do Azure AD
- Um 23 vídeo início de sessão único subscrição ativado

> [!NOTE]
> Para testar os passos neste tutorial, não recomendamos a utilização num ambiente de produção.

Para testar os passos neste tutorial, deve seguir estas recomendações:

- Não utilize o seu ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, pode obter uma avaliação de um mês [aqui](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, teste do Azure AD-início de sessão único num ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos modulares principais:

1. Adicionar as vídeo 23 na galeria do
2. Configurar e testar o Azure AD de sessão único-

## <a name="adding-23-video-from-the-gallery"></a>Adicionar as vídeo 23 na galeria do
Para configurar a integração de vídeo 23 com o Azure AD, tem de adicionar as vídeo 23 na Galeria à sua lista de aplicações SaaS geridas.

**Para adicionar as vídeo 23 a partir da galeria, execute os seguintes passos:**

1. No  **[portal do Azure](https://portal.azure.com)**, no painel de navegação esquerdo, clique em **do Azure Active Directory** ícone. 

    ![Active Directory][1]

2. Navegue para **aplicações empresariais**. Em seguida, aceda a **todas as aplicações**.

    ![Aplicações][2]
    
3. Para adicionar a nova aplicação, clique em **nova aplicação** botão no topo da caixa de diálogo.

    ![Aplicações][3]

4. Na caixa de pesquisa, escreva **as vídeo 23**.

    ![Criar um utilizador de teste do Azure AD](./media/23video-tutorial/tutorial_23video_search.png)

5. No painel de resultados, selecione **as vídeo 23**e, em seguida, clique em **adicionar** botão para adicionar a aplicação.

    ![Criar um utilizador de teste do Azure AD](./media/23video-tutorial/tutorial_23video_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configurar e testar o Azure AD de sessão único-
Nesta secção, configure e teste do Azure AD-início de sessão único com as vídeo 23 com base num utilizador de teste chamado "Britta Simon."

Para início de sessão trabalhar, do Azure AD tem de saber o que o utilizador homólogo as vídeo 23 é um utilizador no Azure AD. Por outras palavras, uma relação de ligação entre um utilizador do Azure AD e o utilizador relacionado no 23 vídeo tem de ser estabelecida.

As vídeo 23, atribua o valor do **nome de utilizador** no Azure AD como o valor a **Username** para estabelecer a relação de ligação.

Para configurar e testar o Azure AD-início de sessão único com as vídeo 23, tem de concluir os blocos modulares seguintes:

1. **[Configurar o Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)**  - para permitir aos utilizadores utilizar esta funcionalidade.
2. **[Criar um utilizador de teste do Azure AD](#creating-an-azure-ad-test-user)**  - para testar o Azure AD-início de sessão único com Britta Simon.
3. **[Criar um utilizador de teste de vídeo 23](#creating-a-23-video-test-user)**  - para ter um homólogo de Britta Simon as vídeo 23 que está ligada a representação do Azure AD do utilizador.
4. **[Atribuir o utilizador de teste do Azure AD](#assigning-the-azure-ad-test-user)**  - para ativar Britta Simon utilizar o Azure AD-início de sessão único.
5. **[Teste o início de sessão único](#testing-single-sign-on)**  - para verificar se a configuração funciona.

### <a name="configuring-azure-ad-single-sign-on"></a>Configurar o Azure AD-início de sessão único

Nesta secção, pode ativar do Azure AD início de sessão no portal do Azure e configurar o início de sessão único na sua aplicação de vídeo 23.

**Para configurar o Azure AD-início de sessão único com as vídeo 23, execute os seguintes passos:**

1. No portal do Azure, no **as vídeo 23** página de integração de aplicações, clique em **de sessão único-**.

    ![Configurar o início de sessão único][4]

2. No **de sessão único-** caixa de diálogo, selecione **modo** como **baseados em SAML início de sessão** para ativar o início de sessão único.
 
    ![Configurar o início de sessão único](./media/23video-tutorial/tutorial_23video_samlbase.png)

3. No **23 URLs e de domínio de vídeo** secção, execute os seguintes passos:

    ![Configurar o início de sessão único](./media/23video-tutorial/tutorial_23video_url.png)

    a. No **URL de início de sessão** caixa de texto, escreva um URL a utilizar o padrão do seguinte: `https://<subdomain>.23video.com`

    b. No **identificador** caixa de texto, escreva um URL a utilizar o padrão do seguinte: `https://www.23video.com/saml/trust/<uniqueid>`

    > [!NOTE] 
    > Estes valores não estiverem reais. Atualize estes valores com o URL de início de sessão e o identificador real. Contacte [23 equipa de suporte de cliente de vídeo](mailto:support@23company.com) para obter estes valores. 
 
4. No **certificado de assinatura de SAML** secção, clique em **certificado (Base64)** e, em seguida, guarde o ficheiro de certificado no seu computador.

    ![Configurar o início de sessão único](./media/23video-tutorial/tutorial_23video_certificate.png) 

5. Clique em **guardar** botão.

    ![Configurar o início de sessão único](./media/23video-tutorial/tutorial_general_400.png)

6. No **23 vídeo configuração** secção, clique em **configurar vídeo de 23** para abrir **configurar início de sessão** janela. Copiar o **Sign-Out URL, o ID de entidade de SAML e o único início de sessão no URL do serviço SAML** do **secção de referência rápida.**

    ![Configurar o início de sessão único](./media/23video-tutorial/tutorial_23video_configure.png) 

7. Para configurar o início de sessão único em **as vídeo 23** lado, terá de enviar o transferido **certificado (Base64)**, **Sign-Out URL, o ID de entidade de SAML e o único início de sessão no URL do serviço SAML** para [23 equipa de suporte de vídeo](mailto:support@23company.com). 


> [!TIP]
> Pode agora ler estas instruções dentro de uma versão concisa o [portal do Azure](https://portal.azure.com), enquanto estiver a configurar a aplicação!  Depois de adicionar esta aplicação a partir do **do Active Directory > aplicações da empresa** secção, basta clicar no **Single Sign-On** separador e aceder à documentação do embedded através de **configuração** secção na parte inferior. Pode ler mais sobre a funcionalidade de documentação incorporados aqui: [do Azure AD incorporado documentação]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="creating-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD
O objetivo desta secção consiste em criar um utilizador de teste no portal do Azure chamado Britta Simon.

![Criar utilizador do Azure AD][100]

**Para criar um utilizador de teste no Azure AD, execute os seguintes passos:**

1. No **portal do Azure**, no painel de navegação esquerdo, clique em **do Azure Active Directory** ícone.

    ![Criar um utilizador de teste do Azure AD](./media/23video-tutorial/create_aaduser_01.png) 

2. Para apresentar a lista de utilizadores, aceda a **utilizadores e grupos** e clique em **todos os utilizadores**.
    
    ![Criar um utilizador de teste do Azure AD](./media/23video-tutorial/create_aaduser_02.png) 

3. Para abrir o **utilizador** caixa de diálogo, clique em **adicionar** na parte superior da caixa de diálogo.
 
    ![Criar um utilizador de teste do Azure AD](./media/23video-tutorial/create_aaduser_03.png) 

4. No **utilizador** diálogo página, execute os seguintes passos:
 
    ![Criar um utilizador de teste do Azure AD](./media/23video-tutorial/create_aaduser_04.png) 

    a. No **nome** caixa de texto, tipo **BrittaSimon**.

    b. No **nome de utilizador** caixa de texto, tipo de **endereço de correio eletrónico** de BrittaSimon.

    c. Selecione **mostrar palavra-passe** e anote o valor da **palavra-passe**.

    d. Clique em **Criar**.
 
### <a name="creating-a-23-video-test-user"></a>Criar um utilizador de teste de vídeo 23

O objetivo desta secção consiste em criar um utilizador chamado Britta Simon as vídeo 23.

**Para criar um utilizador chamado Britta Simon as vídeo 23, execute os seguintes passos:**

1. Inicie sessão site da sua empresa vídeo 23 como administrador.

2. Aceda a **definições**.
 
3. No **utilizadores** secção, clique em **configurar**.
   
    ![Atribua o utilizador][400]

4. Clique em **adicionar um novo utilizador**. 
   
    ![Atribua o utilizador][401]

5. No **convidar alguém aderir a este site** secção, execute os seguintes passos:
   
    ![Atribua o utilizador][402]

    a. No **endereços de correio electrónico** caixa de texto, escreva o endereço de correio eletrónico de Britta Simon no Azure AD.  
 
    b. Clique em **adicionar o utilizador**.   

### <a name="assigning-the-azure-ad-test-user"></a>Atribuir o utilizador de teste do Azure AD

Nesta secção, vai ativar Britta Simon utilizar o Azure-início de sessão único, concedendo acesso para as vídeo 23.

![Atribua o utilizador][200] 

**Para atribuir Britta Simon para as vídeo 23, execute os seguintes passos:**

1. No portal do Azure, abra a vista de aplicações e, em seguida, navegue para a vista de diretório e aceda a **aplicações empresariais** , em seguida, clique em **todas as aplicações**.

    ![Atribua o utilizador][201] 

2. Na lista de aplicações, selecione **as vídeo 23**.

    ![Configurar o início de sessão único](./media/23video-tutorial/tutorial_23video_app.png) 

3. No menu à esquerda, clique em **utilizadores e grupos**.

    ![Atribua o utilizador][202] 

4. Clique em **adicionar** botão. Em seguida, selecione **utilizadores e grupos** no **adicionar atribuição** caixa de diálogo.

    ![Atribua o utilizador][203]

5. No **utilizadores e grupos** caixa de diálogo, selecione **Britta Simon** na lista utilizadores.

6. Clique em **selecione** botão no **utilizadores e grupos** caixa de diálogo.

7. Clique em **atribuir** botão no **adicionar atribuição** caixa de diálogo.
    
### <a name="testing-single-sign-on"></a>Teste o início de sessão único

O objetivo desta secção consiste em testar a configuração de SSO do Azure AD através do painel de acesso.

Quando clica no mosaico de vídeo 23 no painel de acesso, deve obter automaticamente com sessão iniciada para a aplicação de vídeo 23. 

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory](tutorial-list.md)
* [O que é o acesso a aplicações e início de sessão no Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/23video-tutorial/tutorial_general_01.png
[2]: ./media/23video-tutorial/tutorial_general_02.png
[3]: ./media/23video-tutorial/tutorial_general_03.png
[4]: ./media/23video-tutorial/tutorial_general_04.png

[100]: ./media/23video-tutorial/tutorial_general_100.png

[200]: ./media/23video-tutorial/tutorial_general_200.png
[201]: ./media/23video-tutorial/tutorial_general_201.png
[202]: ./media/23video-tutorial/tutorial_general_202.png
[203]: ./media/23video-tutorial/tutorial_general_203.png

[400]: ./media/23video-tutorial/tutorial_23video_10.png
[401]: ./media/23video-tutorial/tutorial_23video_11.png
[402]: ./media/23video-tutorial/tutorial_23video_12.png
