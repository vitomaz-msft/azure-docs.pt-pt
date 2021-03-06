---
title: incluir ficheiro
description: incluir ficheiro
services: active-directory-b2c
author: davidmu1
ms.service: active-directory-b2c
ms.topic: include
ms.date: 04/09/2018
ms.author: davidmu
ms.custom: include file
ms.openlocfilehash: 8363d023e89c77aabc0d123f19264c9a0758a656
ms.sourcegitcommit: f0c2758fb8ccfaba76ce0b17833ca019a8a09d46
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/06/2018
ms.locfileid: "51208245"
---
[!INCLUDE [active-directory-b2c-portal-add-application](active-directory-b2c-portal-add-application.md)]

Para registar a sua aplicação móvel ou nativa, utilize as definições especificadas na tabela.

![Definições de registo de exemplo para a nova aplicação móvel ou nativa](./media/active-directory-b2c-register-mobile-native-app/b2c-new-mobile-native-app-settings.png)

| Definição      | Valor da amostra  | Descrição                                        |
| ------------ | ------- | -------------------------------------------------- |
| **Nome** | Aplicação Contoso B2C | Introduza um **Nome** para a aplicação que descreva a aplicação aos consumidores. |
| **Cliente nativo** | Sim | Selecione **Sim** para uma aplicação móvel ou nativa. |
| **URI de Redirecionamento Personalizado** | `com.onmicrosoft.contoso.appname://redirect/path` | Introduza um URI de redirecionamento com um esquema personalizado. Certifique-se de que escolhe um [bom URI de redirecionamento](../articles/active-directory-b2c/active-directory-b2c-app-registration.md) e não inclui caracteres especiais, como carateres de sublinhado. |

Clique em **Criar** para registar a aplicação.

A aplicação recentemente registada é apresentada na lista de aplicações para o inquilino do B2C. Selecione a sua aplicação móvel ou nativa na lista. É apresentado o painel de propriedades da aplicação.

![Propriedades da aplicação](./media/active-directory-b2c-register-mobile-native-app/b2c-mobile-native-app-properties.png)

Anote o **ID de Cliente da Aplicação** globalmente exclusivo. Utilize o ID no código da aplicação.

Se a sua aplicação nativa chamar uma API Web protegida pelo Azure AD B2C, efetue estes passos:
   1. Crie um segredo de aplicação ao aceder ao painel **Chaves** e ao clicar no botão **Gerar Chave**. Anote o valor da **Chave da aplicação**. Utilize o valor como o segredo de aplicação no código da aplicação.
   2. Clique em **Acesso à API**, clique em **Adicionar** e selecione a API Web e os âmbitos (permissões).

> [!NOTE]
> Um **Segredo de Aplicação** é uma credencial de segurança importante e deve ser protegida devidamente.
> 
