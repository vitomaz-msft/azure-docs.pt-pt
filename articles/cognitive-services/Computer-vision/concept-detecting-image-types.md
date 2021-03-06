---
title: Detetar tipos de imagem - o de imagem digitalizada
titleSuffix: Azure Cognitive Services
description: Conceitos relacionados a detetar tipos de imagem usando a API de imagem digitalizada.
services: cognitive-services
author: PatrickFarley
manager: cgronlun
ms.service: cognitive-services
ms.component: computer-vision
ms.topic: conceptual
ms.date: 08/29/2018
ms.author: pafarley
ms.openlocfilehash: ecdbdd60af41ee14070f2f45dfe50875e38ac3b3
ms.sourcegitcommit: 1aacea6bf8e31128c6d489fa6e614856cf89af19
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/16/2018
ms.locfileid: "49342012"
---
# <a name="detecting-image-types"></a>Detetar tipos de imagem

Imagem digitalizada pode analisar o tipo de conteúdo de imagens ao indicar se uma imagem é clip-art, a probabilidade numa escala ou um desenho de linha de classificação.

## <a name="detecting-clip-art"></a>Clip-art de detecção

Imagem digitalizada analisa uma imagem e classifica a probabilidade da imagem a ser clip-art numa escala de 0 a 3, conforme descrito na tabela seguinte.

| Valor | Significado |
|-------|---------|
| 0 | Não-clip-arts |
| 1 | Ambígua |
| 2 | Normal – clip-arts |
| 3 | Bom-clip-arts |

### <a name="clip-art-detection-examples"></a>Exemplos de deteção de arte do Clip

As respostas JSON seguintes ilustra o que o de imagem digitalizada devolve quando a probabilidade das imagens de exemplo que está a ser clip-art de classificação.

![Visão analisar queijo Clip-Art](./Images/cheese_clipart.png)

```json
{
    "imageType": {
        "clipArtType": 3,
        "lineDrawingType": 0
    },
    "requestId": "88c48d8c-80f3-449f-878f-6947f3b35a27",
    "metadata": {
        "height": 225,
        "width": 300,
        "format": "Jpeg"
    }
}
```

![Visão analisar Yard de casa](./Images/house_yard.png)

```json
{
    "imageType": {
        "clipArtType": 0,
        "lineDrawingType": 0
    },
    "requestId": "a9c8490a-2740-4e04-923b-e8f4830d0e47",
    "metadata": {
        "height": 200,
        "width": 300,
        "format": "Jpeg"
    }
}
```

## <a name="detecting-line-drawings"></a>Detetar desenhos de linha

Imagem digitalizada analisa uma imagem e retorna um valor booleano que indica se a imagem é um desenho de linha.

### <a name="line-drawing-detection-examples"></a>Exemplos de deteção de desenho de linha

As respostas JSON seguintes ilustra o que o de imagem digitalizada devolve quando que indica se as imagens de exemplo são desenhos de linha.

![Visão analisar Lion desenho](./Images/lion_drawing.png)

```json
{
    "imageType": {
        "clipArtType": 2,
        "lineDrawingType": 1
    },
    "requestId": "6442dc22-476a-41c4-aa3d-9ceb15172f01",
    "metadata": {
        "height": 268,
        "width": 300,
        "format": "Jpeg"
    }
}
```

![Visão analisar flor](./Images/flower.png)

```json
{
    "imageType": {
        "clipArtType": 0,
        "lineDrawingType": 0
    },
    "requestId": "98437d65-1b05-4ab7-b439-7098b5dfdcbf",
    "metadata": {
        "height": 200,
        "width": 300,
        "format": "Jpeg"
    }
}
```

## <a name="next-steps"></a>Passos Seguintes

Conheça os conceitos [marcação de imagens](concept-tagging-images.md) e [categorizar imagens](concept-categorizing-images.md).