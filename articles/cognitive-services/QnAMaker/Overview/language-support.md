---
title: Suporte de idiomas - QnA Maker
titleSuffix: Azure Cognitive Services
description: Uma lista de idiomas suportados pelo QnA Maker.
services: cognitive-services
author: tulasim88
manager: cgronlun
ms.service: cognitive-services
ms.component: qna-maker
ms.topic: article
ms.date: 11/09/2018
ms.author: tulasim
ms.openlocfilehash: 8c47c4a59f03328b1dc8d3df7771bac81864bb34
ms.sourcegitcommit: 6b7c8b44361e87d18dba8af2da306666c41b9396
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/12/2018
ms.locfileid: "51566640"
---
# <a name="language-and-region-support-for-qna-maker"></a>Suporte de idioma e região para a ferramenta QnA Maker

O idioma de uma base de dados de conhecimento afeta a capacidade de QnA Maker de extração automática perguntas e respostas partir [origens](../Concepts/data-sources-supported.md), bem como a relevância dos resultados do QnA Maker oferece em resposta a consultas do utilizador.

## <a name="auto-extraction"></a>Extração automática
A ferramenta QnA Maker suporta a extração de pergunta/resposta em qualquer página de idioma, mas a eficácia da extração é muito superior para os seguintes idiomas, como o QnA Maker utiliza as palavras-chave para identificar a perguntas.

|Idiomas suportados| Região|
|-----|----|
|Português|EN-*|
|Francês|FR-*|
|Italiano|isso-*|
|Alemão|Alemanha-*|
|Espanhol|es-*|

## <a name="query-matching-and-relevance"></a>Correspondência de consulta e relevância
Depende do QnA Maker [analisadores de idiomas](https://docs.microsoft.com/rest/api/searchservice/language-support) na pesquisa do Azure para fornecer os resultados. A classificação de recursos especiais estão disponíveis para En-* idiomas que permitem a relevância melhor.

Enquanto os recursos de pesquisa do Azure estão no mesmo nível para idiomas com suporte, a ferramenta QnA Maker tem um adicional classificador que fica acima os resultados da pesquisa do Azure. Nesse modelo classificador, usamos algum especial semântica e palavra recursos com base em en-*, que ainda não estão disponíveis para outros idiomas. Podemos não disponibilizar estas, como fazem parte do funcionamento interno do classificador. 

A ferramenta QnA Maker Deteta automaticamente de idioma da base de dados de conhecimento durante a criação e define o analisador em conformidade. Pode criar bases de dados de conhecimento nos seguintes idiomas. Leia [isso](../How-To/language-knowledge-base.md) para obter mais detalhes sobre como o QnA Maker processa os idiomas.


> [!Tip]
> Analisadores de idiomas, uma vez definido, não pode ser alterado. Além disso, o analisador de idioma aplica-se a todas as bases de dados de conhecimento num [serviço QnA Maker](../How-To/set-up-qnamaker-service-azure.md). Se planear ter bases de dados de conhecimento em idiomas diferentes, deve criá-los em serviços separados do QnA Maker.

|Idiomas suportados|
|-----|
|Árabe|
|Arménio|, 
Bengali|
|Basco (Basco)|
|Búlgaro|
|Catalão|
|Chinese_Simplified|
|Chinese_Traditional|
|Croata|
|Checo|
|Dinamarquês|
|Neerlandês|
|Português|
|Estónio|
|Finlandês|
|Francês|
|Galego|
|Alemão|
|Grego|
|Guzarate|
|Hebraico|
|Hindi|
|Húngaro|
|Islandês|
|Indonésio|
|Irlandês|
|Italiano|
|Japonês|
|Canarim|
|Coreano|
|Letão|
|Lituano|
|Malayalam|
|Malaio|
|Norueguês|
|Polaco|
|Português|
|Punjabi|
|Romeno|
|Russo|
|Serbian_Cyrillic|
|Serbian_Latin|
|Eslovaco|
|Esloveno|
|Espanhol|
|Sueco|
|Tamil|
|Télego|
|Tailandês|
|Turco|
|Ucraniano|
|Urdu|
|Vietnamita|
