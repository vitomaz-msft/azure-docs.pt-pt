---
title: Teste a sua aplicação LUIS
titleSuffix: Azure Cognitive Services
description: O teste é o processo de fornecimento de expressões de exemplo para LUIS e obter uma resposta de LUIS reconhecidas intenções e entidades. Pode testar interativamente, LUIS uma expressão de cada vez, ou fornecer um lote de expressões. Com os testes, compare o modelo de Active Directory atual para o modelo publicado.
services: cognitive-services
author: diberry
manager: cgronlun
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: conceptual
ms.date: 09/10/2018
ms.author: diberry
ms.openlocfilehash: 58bcf45d1a45d448f2f5845ffe43ad07f886a6fc
ms.sourcegitcommit: 17633e545a3d03018d3a218ae6a3e4338a92450d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/22/2018
ms.locfileid: "49637016"
---
# <a name="testing-example-utterances-in-luis"></a>Teste de expressões de exemplo no LUIS

O teste é o processo de fornecimento de expressões de exemplo para LUIS e obter uma resposta de LUIS reconhecidas intenções e entidades. 

Pode [testar](luis-interactive-test.md) LUIS interativamente, uma expressão de cada vez, ou fornecer um [batch](luis-concept-batch-test.md) de expressões. Com os testes, comparar o atual [Active Directory](luis-concept-version.md#active-version) modelos para o modelo publicado. 

<a name="A-test-score"></a>
<a name="Score-all-intents"></a>
<a name="E-(exponent)-notation"></a>

## <a name="what-is-a-score-in-testing"></a>O que é uma pontuação no teste?
Ver [pontuação de predição](luis-concept-prediction-score.md) conceitos para saber mais sobre as pontuações de predição.

## <a name="interactive-testing"></a>Testes interativos
Teste interativo é feita a partir da **teste** painel do Web site. Pode inserir uma expressão para ver como intenções e entidades são identificadas e classificadas. Se o LUIS não é prever as intenções e entidades conforme o esperado numa expressão no painel de teste, copie-o para o **intenção** página como uma expressão de novo. Em seguida, as partes dessa expressão de etiqueta e treinar o LUIS. 

## <a name="batch-testing"></a>Testes em lote
Ver [testes de batch](luis-concept-batch-test.md) se estiver a testar mais do que uma expressão de cada vez.

## <a name="endpoint-testing"></a>Teste de ponto final
Pode testar através da [ponto final](luis-glossary.md#endpoint) com um máximo de duas versões da sua aplicação. Com a sua versão principal ou em direto da sua aplicação definida como o **produção** ponto final, adicionar uma segunda versão para o **teste** ponto final. Esta abordagem dá-lhe três versões de uma expressão: o modelo atual no painel de teste do [LUIS](luis-reference-regions.md) Web site e as versões de dois em dois pontos de extremidade diferentes. 

Todos os ponto final teste contagens para sua quota de utilização. 

## <a name="do-not-log-tests"></a>Não sessão testes
Se o teste com um ponto final e não pretender que a expressão com sessão iniciada, não se esqueça de utilizar o `logging=false` configuração de cadeia de caracteres de consulta.

## <a name="where-to-find-utterances"></a>Onde encontrar expressões com
LUIS armazena expressões com tudo com sessão iniciadas no log de consulta, disponível para download no [LUIS](luis-reference-regions.md) site **Apps** página de lista, bem como o LUIS [APIs de criação](https://aka.ms/luis-authoring-apis). 

Qualquer expressões com os LUIS não tem certeza de que estão listados na **[rever expressões de ponto final](luis-how-to-review-endoint-utt.md)** página do [LUIS](luis-reference-regions.md) Web site. 

![Rever pronunciações de ponto final](./media/luis-concept-test/review-endpoint-utterances.png)
 
## <a name="remember-to-train"></a>Não se esqueça de treinar
Lembre-se [treinar](luis-how-to-train.md) LUIS depois de efetuar alterações ao modelo. Alterações à aplicação LUIS não são vistas nos testes até que a aplicação é preparada. 

## <a name="best-practices"></a>Melhores práticas
Saiba mais [melhores práticas](luis-concept-best-practices.md).

## <a name="next-steps"></a>Passos Seguintes

* Saiba mais sobre [teste](luis-interactive-test.md) seus discursos.