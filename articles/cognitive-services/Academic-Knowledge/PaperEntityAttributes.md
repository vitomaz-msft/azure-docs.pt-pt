---
title: Atributos de entidade de papel - API de conhecimento académico
titlesuffix: Azure Cognitive Services
description: Saiba os atributos que pode utilizar com a entidade de papel na API de conhecimento académico.
services: cognitive-services
author: alch-msft
manager: cgronlun
ms.service: cognitive-services
ms.component: academic-knowledge
ms.topic: conceptual
ms.date: 03/31/2017
ms.author: alch
ms.openlocfilehash: ae238a3c3af091b764147fcde8aa2a98986da92e
ms.sourcegitcommit: 48592dd2827c6f6f05455c56e8f600882adb80dc
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/26/2018
ms.locfileid: "50158895"
---
# <a name="paper-entity"></a>Entidade de documento

<sub> * Abaixo atributos são específicas para a entidade de documento. (Ty = '0') </sub>


Nome    |Descrição                                        |Tipo       | Operações
------- | ------------------------------------------------- | --------- | ----------------------------
Id      |ID de entidade                                          |Int64      |Igual a
Te      |Título do documento                                        |Cadeia     |É igual a,<br/>StartsWith
L       |Código de idioma do documento separados por "\@@ @"            |Cadeia     |Igual a
S       |Ano de documento                                         |Int32      |É igual a,<br/>IsBetween
D       |Data do documento                                         |Date       |É igual a,<br/>IsBetween
Cc      |Contagem de citação                                     |Int32      |nenhum  
ECC     |Contagem de citação estimada                           |Int32      |nenhum
AA. AuN  |Nome do autor                                        |Cadeia     |É igual a,<br/>StartsWith
AA. AuId |ID do autor                                          |Int64      |Igual a
AA. AfN  |Nome de afiliação do autor                            |Cadeia     |É igual a,<br/>StartsWith
AA. AfId |ID de afiliação de autor                              |Int64      |Igual a
AA. S    |Ordem de autor para o documento                         |Int32      |Igual a
F.FN    |Campo de nome de estudo                                |Cadeia     |É igual a,<br/>StartsWith
F.FId   |Campo de ID de estudo                                  |Int64      |Igual a
J.JN    |Nome do diário                                       |Cadeia     |É igual a,<br/>StartsWith
J.JId   |ID do diário                                         |Int64      |Igual a
C.CN    |Nome da série de conferências                             |Cadeia     |É igual a,<br/>StartsWith
C.CId   |ID de série de conferências                               |Int64      |Igual a
De rId     |ID de referenciado papers                              |Int64 []    |Igual a
W       |Palavras do título de documento e o resumo                |String]   |Igual a
E       |Expandido metadados (consulte a tabela abaixo)                |Cadeia     |nenhum  
        


## <a name="extended-metadata-attributes"></a>Atributos de metadados expandidos ##

Nome    | Descrição               
--------|---------------------------    
DN      | Nome a apresentar do documento 
S       | Origens - lista de origens de web do documento, classificados por rank estático
S.Ty    | Tipo de origem (1:HTML, 2:Text, 3:PDF, 4:DOC, 5:PPT, 6:XLS, 7:PS)
S.U     | URL de Origem
VFN     | Nome completo do local - nome completo do diário ou conferência
VSN     | Nome abreviado do local - nome abreviado do diário ou conferência
V       | Volume - volume diário
BV      | Nome do diário
BT      | 
PB      | Abreviations de diário
I       | Problema – edição de diário
FP      | FirstPage - primeira página do documento
LP      | LastPage - última página do documento
DOI     | Identificador de objeto digital
Cc      | Contextos de citação – lista de papel referenciado IDs e o contexto correspondente no documento (por exemplo, [{123: ["foxes castanhas são conhecidos para saltar conforme referenciado no documento 123", "os cães lentas são um nome inapropriado histórico conforme mostrado no artigo 123"]})
IA      | Invertida Abstrato
IA. IndexLength| Número de itens no índice (contagem de palavras do abstrato)
IA. InvertedIndex| Lista de palavras abstratas e a respetiva posição correspondente na teoria original (por exemplo, [{"a": [0, 15, 30]}, {"castanha": [1]}, {"fox":[2]}])
