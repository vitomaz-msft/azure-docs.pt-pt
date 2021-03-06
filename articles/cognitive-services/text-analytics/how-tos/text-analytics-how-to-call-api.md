---
title: Chamar a API de Análise de Texto
titlesuffix: Azure Cognitive Services
description: Saiba como chamar a API de REST de análise de texto.
services: cognitive-services
author: ashmaka
manager: cgronlun
ms.service: cognitive-services
ms.component: text-analytics
ms.topic: conceptual
ms.date: 10/01/2018
ms.author: ashmaka
ms.openlocfilehash: a70ef893019264ffc0eb3cb2982b05b15ebd0acf
ms.sourcegitcommit: 55952b90dc3935a8ea8baeaae9692dbb9bedb47f
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/09/2018
ms.locfileid: "48884373"
---
# <a name="how-to-call-the-text-analytics-rest-api"></a>Como chamar a API de REST de análise de texto

Chamadas para o **API de análise de texto** são chamadas de HTTP POST/GET, que pode ser formular em qualquer idioma. Neste artigo, vamos utilizar o REST e [Postman](https://chrome.google.com/webstore/detail/postman/fhbjgbiflinjbdggehcddcbncdddomop) para demonstrar conceitos chave.

Cada pedido tem de incluir a chave de acesso e um ponto final HTTP. O ponto de extremidade Especifica a região que escolheu durante a cópia de segurança, o URL do serviço e um recurso utilizado no pedido: `sentiment`, `keyphrases`, `languages`, e `entities`. 

Lembre-se de que a análise de texto é sem monitoração de estado, portanto, existem não existem recursos de dados para gerir. O texto é carregado, analisados após a receção, e os resultados são retornados imediatamente ao aplicativo de chamada.

> [!Tip]
> Para obter chamadas pontuais ver como funciona a API, pode enviar pedidos POST de incorporada **consola de teste de API**, disponível em qualquer [página de documentação de API](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics.V2.0/operations/56f30ceeeda5650db055a3c6). Há uma configuração e os únicos requisitos são colar uma chave de acesso e os documentos JSON no pedido. 

## <a name="prerequisites"></a>Pré-requisitos

Tem de ter uma [conta de API dos Serviços Cognitivos](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) com a **API de Análise de Texto**. 

Tem de ter o [chave de acesso e de ponto final](text-analytics-how-to-access-key.md) que é gerado para quando se inscreve para os serviços cognitivos. 

<a name="json-schema"></a>

## <a name="json-schema-definition"></a>Definição de esquema JSON

Entrada tem de ser JSON em texto não estruturado bruto. XML não é suportado. O esquema é simples, que consiste dos elementos descritos na lista seguinte. 

Atualmente pode submeter os documentos mesmo para todas as operações de análise de texto: sentimentos, expressões-chave, deteção de idioma e identificação de entidade. (O esquema é provavelmente irá variar dependendo de cada análise no futuro.)

| Elemento | Valores válidos | Necessário? | Utilização |
|---------|--------------|-----------|-------|
|`id` |O tipo de dados é a cadeia de caracteres, mas na prática, IDs de documento tendem a ser números inteiros. | Necessário | O sistema utiliza os IDs de fornecer ao estruturar a saída. As classificações de sentimentos, expressões-chave e códigos de idioma são geradas para cada ID no pedido.|
|`text` | Texto não estruturado bruto, até 5000 carateres. | Necessário | Para deteção de idioma, o texto pode ser expresso em qualquer idioma. Para a análise de sentimentos, extração de expressões-chave e identificação de entidade, o texto deve estar numa [idioma suportado](../text-analytics-supported-languages.md). |
|`language` | 2 carateres [ISO 639-1](https://en.wikipedia.org/wiki/List_of_ISO_639-1_codes) de código para um [idioma suportado](../text-analytics-supported-languages.md) | Varia | Necessário para a análise de sentimentos, extração de expressões-chave e ligação de entidades; opcional para a deteção de idioma. Não há nenhum erro se exclui-lo, mas a análise é enfraquecida sem ele. O código de idioma deve corresponder do `text` que fornecer. |

Para obter mais informações sobre os limites, consulte [descrição geral da análise de texto > limites de dados](../overview.md#data-limits). 

## <a name="set-up-a-request-in-postman"></a>Configurar um pedido no Postman

O serviço aceita até 1 MB de tamanho do pedido. Se estiver a utilizar o Postman (ou outra ferramenta de teste de Web API), configurar o ponto final para incluir o recurso que pretende utilizar e fornece a chave de acesso num cabeçalho de pedido. Cada operação requer que acrescentar o recurso apropriado para o ponto final. 

1. No Postman:

   + Escolher **Post** como o tipo de pedido.
   + Cole o ponto final que copiou a partir da página de portal.
   + Acrescente um recurso.

  Pontos finais do recurso são da seguinte forma (a sua região pode variar):

   + `https://westus.api.cognitive.microsoft.com/text/analytics/v2.0/sentiment`
   + `https://westus.api.cognitive.microsoft.com/text/analytics/v2.0/keyPhrases`
   + `https://westus.api.cognitive.microsoft.com/text/analytics/v2.0/languages`
   + `https://westus.api.cognitive.microsoft.com/text/analytics/v2.1-preview/entities`

2. Defina os cabeçalhos de pedido de três:

   + `Ocp-Apim-Subscription-Key`: a chave de acesso, obtido a partir do portal do Azure.
   + `Content-Type`: aplicação/json.
   + `Accept`: aplicação/json.

  O pedido deve ser semelhante à seguinte captura de ecrã, partindo do princípio de um **/keyPhrases** recursos.

   ![Captura de ecrã com ponto final e cabeçalhos de pedido](../media/postman-request-keyphrase-1.png)

4. Clique em **corpo** e escolha **brutos** para o formato.

   ![Captura de ecrã com as definições do corpo do pedido](../media/postman-request-body-raw.png)

5. Cole alguns documentos JSON num formato válido para a análise pretendida. Para obter mais informações sobre uma análise específica, consulte os tópicos abaixo:

  + [Deteção de idioma](text-analytics-how-to-language-detection.md)  
  + [Extração de expressões-chave](text-analytics-how-to-keyword-extraction.md)  
  + [Análise de sentimentos](text-analytics-how-to-sentiment-analysis.md)  
  + [Reconhecimento de entidades (pré-visualização)](text-analytics-how-to-entity-linking.md)  


6. Clique em **enviar** para submeter o pedido. Pode enviar até 100 pedidos por minuto. 

  No Postman, a resposta é apresentada na janela seguinte, como um único documento JSON, com um item para cada ID de documento fornecido no pedido.

## <a name="see-also"></a>Consulte também 

 [Text Analytics Overview](../overview.md) (Descrição Geral da Análise de Texto)  
 [Perguntas Mais Frequentes (FAQ)](../text-analytics-resource-faq.md)

## <a name="next-steps"></a>Passos Seguintes

> [!div class="nextstepaction"]
> [Detetar idioma](text-analytics-how-to-language-detection.md)
