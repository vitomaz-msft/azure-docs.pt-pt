---
title: Ponto final de pré-visualização do URL do projeto
titlesuffix: Azure Cognitive Services
description: Resumo do ponto de extremidade de pré-visualização do URL.
services: cognitive-services
author: mikedodaro
manager: cgronlun
ms.service: cognitive-services
ms.component: url-preview
ms.topic: reference
ms.date: 03/29/2018
ms.author: rosh, v-gedod
ms.openlocfilehash: f75fc73bc1268db7b6f9f8a1f4fd602ee57281e8
ms.sourcegitcommit: 62759a225d8fe1872b60ab0441d1c7ac809f9102
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/19/2018
ms.locfileid: "49464953"
---
# <a name="project-url-preview-endpoint"></a>Ponto final de pré-visualização do URL do projeto

A API de pré-visualização do URL contém um ponto final.

## <a name="endpoint"></a>Ponto Final
Para obter uma pré-visualização de URL, envie um pedido para o seguinte ponto de extremidade. Utilize os cabeçalhos e os parâmetros de URL para outras especificações.

GET:
````
https://api.labs.cognitive.microsoft.com/urlpreview/v7.0/search?q=https://swiftkey.com

````

### <a name="query-parameters"></a>Parâmetros de consulta
|Nome|Valor|Tipo|Necessário|  
|----------|-----------|----------|--------------|  
|p|URL para a pré-visualização|Cadeia |Sim|
|pesquisa segura|Conteúdo para adultos ilegal ou pirateado conteúdo, é bloqueado com o código de erro 400 e o *isFamilyFriendly* sinalizador não for devolvido. <p>Para o conteúdo para adultos legal, segue-se o comportamento. Código de estado devolve 200 e o *isFamilyFriendly* sinalizador estiver definido como false.<ul><li>pesquisa segura = strict: título, descrição, URL e imagem não vão ser devolvidos.</li><li>pesquisa segura = moderado; Obtenha title, URL e descrição mas não a imagem descritiva.</li><li>pesquisa segura = desativar; Obter todos os resposta objetos/elementos – title, URL, descrição e imagem.</li></ul> |Cadeia|Não é necessário. </br> O padrão é safeSearch = rigorosa.| 

## <a name="response-object"></a>Objeto de resposta

A resposta inclui cabeçalhos HTTP e o objeto de página Web com atributos, conforme mostrado no exemplo seguinte: `name`, `url`, `description`, `isFamilyFriendly`, e `primaryImageOfPage`.

````
BingAPIs-TraceId: 15AFE52A97AA422F960433A94803F6CE
BingAPIs-SessionId: 40587764F42142D3A8BA99F66B2B3BB6
X-MSEdge-ClientID: 0389E3EDED106B5E1424E82FEC436A56
BingAPIs-Market: en-US
X-MSEdge-Ref: Ref A: 15AFE52A97AA422F960433A94803F6CE Ref B: PAOEDGE0418 Ref C: 2018-03-30T16:36:27Z
{
  "_type": "WebPage",
  "name": "SwiftKey - Smart prediction technology for easier mobile ...",
  "url": "https://swiftkey.com/",
   "description": "Discover the best Android and iPhone and iPad apps for faster, easier typing with emoji, colorful themes and more - download SwiftKey Keyboard free today.",
  "isFamilyFriendly": true,
  "primaryImageOfPage": {
    "contentUrl": "https://swiftkey.com/images/og/default.jpg"
  }
}

````

## <a name="next-steps"></a>Passos Seguintes
- [Início Rápido do C#](csharp.md)
- [Início rápido do Java](java-quickstart.md)
- [Início Rápido do JavaScript](javascript.md)
- [Início Rápido do Node](node-quickstart.md)
- [Início Rápido do Python](python-quickstart.md)
