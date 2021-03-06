---
title: Texto dividir a habilidade de pesquisa cognitiva (Azure Search) | Documentos da Microsoft
description: Divida texto em segmentos ou páginas de texto com base no comprimento num pipeline de enriquecimento de Azure Search.
services: search
manager: pablocas
author: luiscabrer
ms.service: search
ms.devlang: NA
ms.workload: search
ms.topic: conceptual
ms.date: 05/01/2018
ms.author: luisca
ms.openlocfilehash: 583d2ac5a8ac4c236612cdfe78595da1812c56fa
ms.sourcegitcommit: 1b561b77aa080416b094b6f41fce5b6a4721e7d5
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/17/2018
ms.locfileid: "45730771"
---
#   <a name="text-split-cognitive-skill"></a>Texto a dividir competências cognitivas

O **divisão de texto** habilidade quebra o texto em segmentos de texto. Pode especificar se pretende dividem o texto em frases ou em páginas de um comprimento específico. Essa habilidade é especialmente útil se existirem texto máximo requisitos de comprimento de outras habilidades downstream. 

> [!NOTE]
> A Pesquisa Cognitiva está em pré-visualização pública. Conjunto de capacidades execução e a extração de imagem e a normalização atualmente são oferecidos gratuitamente. Posteriormente, os preços para estas capacidades serão anunciado. 

## <a name="odatatype"></a>@odata.type  
Microsoft.Skills.Text.SplitSkill 

## <a name="skill-parameters"></a>Parâmetros de habilidades

Parâmetros diferenciam maiúsculas de minúsculas.

| Nome do parâmetro     | Descrição |
|--------------------|-------------|
| textSplitMode      | "Páginas" ou "frases" | 
| maximumPageLength | Se textSplitMode estiver definido como "páginas", isso se refere ao comprimento máximo de página, medido pela `String.Length`. O valor mínimo é 100. | 
| defaultLanguageCode   | (opcional) Um dos seguintes códigos de idioma: `da, de, en, es, fi, fr, it, ko, pt`. A predefinição é o inglês (en). Alguns aspetos a considerar:<ul><li>Se passar um formato de languagecode-indicativo do país, é utilizada apenas a parte de languagecode no formato.</li><li>Se o idioma não estiver na lista anterior, a habilidade de divisão quebra o texto em limites dos caracteres.</li><li>Fornecer um código de idioma é útil para evitar cortando uma palavra em metade para não espaço idiomas como chinês, japonês e coreano.</li></ul>  |


## <a name="skill-inputs"></a>Entradas de habilidades

| Nome do parâmetro       | Descrição      |
|----------------------|------------------|
| texto  | O texto a dividir em subcadeia. |
| languageCode  | (Opcional) Código de idioma para o documento.  |

## <a name="skill-outputs"></a>Saídas de habilidades 

| Nome do parâmetro     | Descrição |
|--------------------|-------------|
| textItems | Uma matriz de subcadeias de carateres que foram extraídos. |


##  <a name="sample-definition"></a>Definição de exemplo

```json
{
    "@odata.type": "#Microsoft.Skills.Text.SplitSkill",
    "textSplitMode" : "pages", 
    "maximumPageLength": 1000,
    "defaultLanguageCode": "en",
    "inputs": [
        {
            "name": "text",
            "source": "/document/content"
        },
        {
            "name": "languageCode",
            "source": "/document/language"
        }
    ],
    "outputs": [
        {
            "name": "textItems",
            "targetName": "mypages"
        }
    ]
}
```

##  <a name="sample-input"></a>Entrada de exemplo

```json
{
    "values": [
        {
            "recordId": "1",
            "data": {
                "text": "This is a the loan application for Joe Romero, he is a Microsoft employee who was born in Chile and then moved to Australia…",
                "languageCode": "en"
            }
        },
        {
            "recordId": "2",
            "data": {
                "text": "This is the second document, which will be broken into several pages...",
                "languageCode": "en"
            }
        }
    ]
}
```

##  <a name="sample-output"></a>Saída de exemplo

```json
{
    "values": [
        {
            "recordId": "1",
            "data": {
                "textItems": [
                    "This is the loan…",
                    "On the second page we…"
                ]
            }
        },
        {
            "recordId": "2",
            "data": {
                "textItems": [
                    "This is the second document...",
                    "On the second page of the second doc…"
                ]
            }
        }
    ]
}
```

## <a name="error-cases"></a>Casos de erro
Se um idioma não é suportado, é gerado um aviso e o texto for dividido em limites dos caracteres.

## <a name="see-also"></a>Consulte também

+ [Competências predefinidas](cognitive-search-predefined-skills.md)
+ [Como definir um conjunto de capacidades](cognitive-search-defining-skillset.md)
