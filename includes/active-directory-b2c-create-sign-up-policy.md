---
author: PatAltimore
ms.service: active-directory-b2c
ms.topic: include
ms.date: 11/03/2016
ms.author: patricka
ms.openlocfilehash: 511b05e6cae769a5b39ae81a3e67efd05d374511
ms.sourcegitcommit: 9d7391e11d69af521a112ca886488caff5808ad6
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/25/2018
ms.locfileid: "50133294"
---
Se quiser permitir que apenas Inscreva-se na sua aplicação, utilize um **Inscreva-se** política. Esta política descreve as experiências que os clientes passam por durante a inscrição e os conteúdos de tokens que a aplicação recebe nas inscrições com êxito.

[!INCLUDE [active-directory-b2c-portal-navigate-b2c-service](active-directory-b2c-portal-navigate-b2c-service.md)]

Clique em **Políticas de inscrição**.

Clique em **+ Adicionar** na parte superior do painel.

O **Nome** determina o nome da política de inscrição utilizado pela sua aplicação. Por exemplo, introduza **SiUp**.

Clique em **Fornecedores de identidade** e selecione **Inscrição de e-mail**. Opcionalmente, também pode selecionar fornecedores de identidade social, se já estiverem configurados. Clique em **OK**.

Clique em **Atributos de inscrição**. Aqui pode escolher os atributos que quer recolher do consumidor durante a inscrição. Por exemplo, selecione **País/Região**, **Nome a Apresentar**, e **Código Postal**. Clique em **OK**.

Clique em **Afirmações de aplicação**. Aqui, pode escolher as afirmações que quer que sejam devolvidas nos tokens enviados para a sua aplicação depois de uma experiência de inscrição com êxito. Por exemplo, selecione **Nome a Apresentar**, **Fornecedor de Identidade**, **Código Postal**, **O utilizador é novo** e **ID de Objeto do Utilizador**.

Clique em **Criar**. A política criada é apresentada como **B2C_1_SiUp** (o fragmento **B2C\_1\_**  é adicionado automaticamente) no painel **Políticas de inscrição**.

Abra a política ao clicar em **B2C_1_SiUp**.

Selecione **Aplicação Contoso B2C** no menu pendente **Aplicações** e `https://localhost:44321/` no menu pendente **URL de Resposta/URI de Redirecionamento**.

Clique em **Executar agora**. É aberto um novo separador do browser e pode percorrer a experiência de consumidor de inscrição na sua aplicação.

> [!NOTE]
> A criação da política e as atualizações demoram até um minuto a serem aplicadas.
>