---
title: Mapeamento no Azure Active Directory de afirmações de utilizador de colaboração do B2B | Documentos da Microsoft
description: Personalize as afirmações de utilizador que são emitidas no token SAML para utilizadores B2B do Azure Active Directory (Azure AD).
services: active-directory
ms.service: active-directory
ms.component: B2B
ms.topic: conceptual
ms.date: 04/06/2018
ms.author: mimart
author: msmimart
manager: mtillman
ms.reviewer: sasubram
ms.openlocfilehash: fd18af3cca200813ba447e642ab678c3c1a8900a
ms.sourcegitcommit: 776b450b73db66469cb63130c6cf9696f9152b6a
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/18/2018
ms.locfileid: "45985686"
---
# <a name="b2b-collaboration-user-claims-mapping-in-azure-active-directory"></a>Mapeamento no Azure Active Directory de afirmações de utilizador de colaboração B2B

Personalizando as afirmações que são emitidas no token SAML para utilizadores de colaboração B2B do Azure suporta o Active Directory (Azure AD). Quando um utilizador efetua a autenticação à aplicação, o Azure AD emite um token SAML para a aplicação que contém informações (ou afirmações) sobre o utilizador que identifica exclusivamente. Por predefinição, isto inclui o nome de utilizador, endereço de e-mail, nome próprio e apelido do utilizador.

Na [portal do Azure](https://portal.azure.com), pode ver ou editar as afirmações que são enviadas no token SAML para o aplicativo. Para aceder as definições, selecione **do Azure Active Directory** > **aplicações empresariais** > a aplicação que está configurada para início de sessão único > **início de sessão único** . Ver as definições de token SAML no **atributos de utilizador** secção.

![Mostra os atributos de token SAML na interface do Usuário](media/claims-mapping/view-claims-in-saml-token.png)

Existem duas razões possíveis porque poderá ter de editar as afirmações que são emitidas no SAML token:

1. A aplicação requer um conjunto diferente de URIs de afirmação ou afirmação valores.

2. A aplicação requer a afirmação NameIdentifier ser algo que não seja o nome de principal de utilizador (UPN) armazenado no Azure AD.

Para obter informações sobre como adicionar e editar afirmações, consulte [personalizar afirmações emitidas no token SAML para aplicações empresariais no Azure Active Directory](../develop/active-directory-saml-claims-customization.md).

Colaboração B2B utilizadores, mapeamento NameID e UPN entre inquilinos impedidos por motivos de segurança.

## <a name="next-steps"></a>Passos Seguintes

- Para obter informações sobre as propriedades de utilizador de colaboração do B2B, consulte [propriedades de um utilizador de colaboração do Azure Active Directory B2B](user-properties.md).
- Para obter informações sobre os tokens de utilizador para os utilizadores de colaboração do B2B, consulte [compreender tokens de utilizador em colaboração B2B do Azure AD](user-token.md).

