---
title: Opções de ajuda e suporte para programadores de identidade do Azure | Documentos da Microsoft
description: Saber como obter ajuda e suporte para perguntas relacionadas com desenvolvimento e problemas durante a criação de aplicativos que se integram com identidades do Microsoft Azure (Azure Active Directory e MSA)
services: active-directory
documentationcenter: dev-center-name
author: CelesteDG
manager: mtillman
editor: ''
ms.assetid: 820acdb7-d316-4c3b-8de9-79df48ba3b06
ms.service: active-directory
ms.component: develop
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 10/27/2017
ms.author: celested
ms.reviewer: andret
ms.custom: aaddev
ms.openlocfilehash: 9b2ccce4ef6fad4f7710ba2092d67ac5f4e73b71
ms.sourcegitcommit: 1f0587f29dc1e5aef1502f4f15d5a2079d7683e9
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/07/2018
ms.locfileid: "39602012"
---
# <a name="support-and-help-options-for-developers"></a>Opções de ajuda e suporte para programadores 

Independentemente se estiver apenas a começar a integrar com o Azure Active Directory, as identidades da Microsoft ou o Microsoft Graph API, ou quando estiver a implementar um novo recurso ao seu aplicativo, há ocasiões em que precisa de obter ajuda da Comunidade ou compreender o Opções de suporte que já têm como um desenvolvedor. Este artigo ajuda-o a compreender estas opções, abaixo um resumo:

> [!div class="checklist"]
> * Pesquisa para verificar se a sua pergunta problema não foi respondida pela Comunidade, ou se uma documentação existente para a funcionalidade que está a tentar implementar já existe
> * Em alguns casos, apenas desejar usar nossas ferramentas de suporte para o ajudar a depurar uma específicas do seu problema
> * Se não conseguir encontrar a resposta do que precisa, pode querer fazer uma pergunta sobre *Stack Overflow*
> * Se encontrar um problema com uma das nossas bibliotecas de autenticação, emitir uma *GitHub* problema
> * Por fim, se precisar de falar com alguém, poderá abrir um pedido de suporte


## <a name="search"></a>Pesquisa

Se tiver dúvidas relacionadas com o desenvolvimento, poderá encontrar a resposta que precisa na nossa documentação, nosso [exemplos do github](https://github.com/azure-samples), ou respostas para [Stack Overflow](https://www.stackoverflow.com) perguntas.

### <a name="scoped-search"></a>Procura de âmbito
Para obter resultados mais rápidos, o âmbito da pesquisa para o Stack Overflow, nossa documentação e nossos exemplos de código através do seguinte no seu [mecanismo de pesquisa favorito](https://bing.com):
```
{Your Search Terms} (site:stackoverflow.com OR site:docs.microsoft.com OR site:github.com/azure-samples OR site:cloudidentity.com OR site:developer.microsoft.com/en-us/graph)
```
Em que *{Your termos de pesquisa}* é suas palavras-chave de pesquisa.
<br/>

## <a name="use-our-development-support-tools"></a>Utilize as nossas ferramentas de suporte de desenvolvimento

|Ferramenta  |Descrição  |
|---------|---------|
|[jwt.MS](https://jwt.ms)| Cole um tokens de acesso ou ID para decodificar os valores e nomes de afirmações |
|[Analisador de código de erro](https://apps.dev.microsoft.com/portal/tools/errors)| Cole o código de erro recebido durante o início de sessão ou páginas para ver as causas possíveis e remediações de consentimento |
|[Do Microsoft Graph](https://developer.microsoft.com/graph/graph-explorer)| Ferramenta que permite-lhe fazer pedidos e ver respostas com o Microsoft Graph API|

<br/>

[![O Stack Overflow](./media/developer-support-help-options/stackoverflow-logo.png)](https://www.stackoverflow.com)
## <a name="post-a-question-to-stack-overflow"></a>Publique uma pergunta do Stack Overflow

Estouro de pilha é o canal preferido para questões relacionadas com desenvolvimento - em que membros da Equipe ambos os membros da Comunidade como a Microsoft está diretamente envolvido em ajudá-lo para resolver seu problema.

Se não conseguir encontrar uma resposta para o seu problema através da pesquisa, enviar uma nova pergunta para Stack Overflow: Utilize um dos seguintes etiquetas ao fazer perguntas para ajudar a Comunidade a identificar, em seguida, responda à sua pergunta em tempo útil:

|Componente/área  |Etiquetas  |
|---------|---------|
|Biblioteca ADAL |[[adal]](http://stackoverflow.com/questions/tagged/adal)|
|Biblioteca MSAL     |[[msal]](http://stackoverflow.com/questions/tagged/msal)|
|Middleware da OWIN  |[[azure-active-directory]](http://stackoverflow.com/questions/tagged/azure-active-directory)|
|[B2B do Azure](https://docs.microsoft.com/azure/active-directory/active-directory-b2b-what-is-azure-ad-b2b)  |[[azure-ad-b2b]](http://stackoverflow.com/questions/tagged/azure-ad-b2b)|
|[B2C do Azure](https://azure.microsoft.com/services/active-directory-b2c/)  |[[azure-ad-b2c]](http://stackoverflow.com/questions/tagged/azure-ad-b2b)|
|[Microsoft Graph API](https://developer.microsoft.com/graph/) |[[microsoft-graph]](http://stackoverflow.com/questions/tagged/microsoft-graph)
|Qualquer outra área relacionadas com a autenticação ou autorização tópicos |[[azure-active-directory]](http://stackoverflow.com/questions/tagged/azure-active-directory)
<br/>
> [!TIP]
> As seguintes mensagens de Stack Overflow contêm dicas sobre como fazer perguntas e sugestões sobre como adicionar o código-fonte - seguir essas diretrizes podem ajudar a aumentar as hipóteses de membros da Comunidade avaliar e responder rapidamente a sua pergunta:  
> - [Como posso fazer uma boa pergunta](https://stackoverflow.com/help/how-to-ask)
> - [Como criar um mínimo, conclua e exemplo verificável](https://stackoverflow.com/help/mcve)

<br/>


[![O Stack Overflow](./media/developer-support-help-options/github-logo.png)](https://www.github.com)
## <a name="create-a-github-issue"></a>Criar um problema do GitHub

 Se encontrar um erro ou problema relacionado com a nossas bibliotecas, gerar um problema no nosso repositório do GitHub. Como nossas bibliotecas de código-fonte aberto, também tem liberdade para submeter um pedido pull também. O seguinte artigo contém uma lista de bibliotecas e seus repositórios do GitHub:

- [ADAL e Owin MSAL middleware](active-directory-authentication-libraries.md) bibliotecas e os repositórios do GitHub

<br/>

## <a name="open-a-support-request"></a>Abra um pedido de suporte

Se precisar de falar com alguém, pode abrir um pedido de suporte. Se for um cliente do Azure, existem várias opções de suporte disponíveis. Comparar planos, consulte [esta página](https://azure.microsoft.com/support/plans/). Suporte para programadores também está disponível para clientes do Azure. Para obter informações sobre como comprar planos de suporte programador, consulte [esta página](https://azure.microsoft.com/support/plans/developer/).

- Se já tiver um plano do Azure suporta, [abrir um pedido de suporte aqui](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest)

- Se não for um cliente do Azure, também pode abrir um pedido de suporte com a Microsoft via [nosso suporte comercial](https://support.microsoft.com/en-us/gp/contactus81?Audience=Commercial).

Também pode tentar [nosso agente virtual](https://support.microsoft.com/contactus/?ws=support) para obter suporte ou colocar questões.

### <a name="free-chat-support-for-a-limited-time"></a>Suporte de bate-papo gratuito por um período limitado

Também pode utilizar o nosso suporte por chat - que é gratuita para Microsoft Partners por um período limitado. Se a sua empresa não é um Microsoft Partner, pode utilizar para inscrevê-lo gratuitamente e obter outros benefícios indo [aqui](https://partners.microsoft.com/PartnerProgram/simplifiedenrollment.aspx).

Depois de inscrever a sua empresa, pode iniciar o pedido de bate-papo [aqui](https://aka.ms/devchat).