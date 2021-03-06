---
title: 'Início Rápido: SDK de Pesquisa de Vídeos do Bing, Node'
titleSuffix: Azure Cognitive Services
description: Configure a aplicação de consola do SDK de Pesquisa de Vídeos do Bing.
services: cognitive-services
author: mikedodaro
manager: cgronlun
ms.service: cognitive-services
ms.component: bing-video-search
ms.topic: quickstart
ms.date: 02/12/2018
ms.author: rosh
ms.openlocfilehash: 9a2544fe5a3c0bac763b9aee79c36893ec56f351
ms.sourcegitcommit: db2cb1c4add355074c384f403c8d9fcd03d12b0c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/15/2018
ms.locfileid: "51686692"
---
# <a name="quickstart-bing-video-search-sdk-with-node"></a>Início Rápido: SDK de Pesquisa de Vídeos do Bing com Node

O SDK de Pesquisa de Vídeos do Bing contém a funcionalidade da API REST para consultas de vídeo e resultados de análise. 

O [código-fonte para os exemplos de SDK Node da Pesquisa de Vídeos do Bing](https://github.com/Azure-Samples/cognitive-services-node-sdk-samples/blob/master/Samples/videoSearch.js) está disponível no Git Hub.

## <a name="application-dependencies"></a>Dependências da aplicação

Para configurar uma aplicação de consola com o SDK de pesquisa de vídeos do Bing:
* Executar `npm install ms-rest-azure` no seu ambiente de desenvolvimento.
* Executar `npm install azure-cognitiveservices-videosearch` no seu ambiente de desenvolvimento.

## <a name="video-search-client"></a>Cliente de Pesquisa de Vídeos
Obtenha uma [chave de acesso aos Serviços Cognitivos](https://azure.microsoft.com/try/cognitive-services/) em *Pesquisa*. Crie uma instância do `CognitiveServicesCredentials`:
```
const CognitiveServicesCredentials = require('ms-rest-azure').CognitiveServicesCredentials;
let credentials = new CognitiveServicesCredentials('YOUR-ACCESS-KEY');
```
Em seguida, inicie o cliente:
```
const VideoSearchAPIClient = require('azure-cognitiveservices-videosearch');
let client = new VideoSearchAPIClient(credentials);
```
Pesquise os resultados.
```
client.videosOperations.search('Interstellar Trailer').then((result) => {
    console.log(result.value);
}).catch((err) => {
    throw err;
});

```

<!-- Remove until the response can be replace with a sanitized version.
The code prints `result.value` items to the console without parsing any text. The results will be:
- _type: 'VideoObjectElementType'

![Video results](media/video-search-sdk-node-results.png)
-->

## <a name="next-steps"></a>Passos Seguintes

[Exemplos de SDK Node.js dos Serviços Cognitivos](https://github.com/Azure-Samples/cognitive-services-node-sdk-samples)
