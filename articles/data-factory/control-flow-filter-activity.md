---
title: Filtrar a atividade no Azure Data Factory | Microsoft Docs
description: A atividade de filtro filtra as entradas.
services: data-factory
documentationcenter: ''
author: sharonlo101
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 05/04/2018
ms.author: shlo
ms.openlocfilehash: b3b26869a84b8519ced19a4c93a6d39d6ed20f9b
ms.sourcegitcommit: 0c490934b5596204d175be89af6b45aafc7ff730
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/27/2018
ms.locfileid: "37050357"
---
# <a name="filter-activity-in-azure-data-factory"></a>Filtro de atividade no Azure Data Factory
Pode utilizar uma atividade de filtro num pipeline para aplicar uma expressão de filtro a uma matriz de entrada. 

## <a name="syntax"></a>Sintaxe

```json
{
    "name": "MyFilterActivity",
    "type": "filter",
    "typeProperties": {
        "condition": "<condition>",
        "items": "<input array>"
    }
}
```

## <a name="type-properties"></a>Propriedades do tipo

Propriedade | Descrição | Valores permitidos | Necessário
-------- | ----------- | -------------- | --------
nome | Nome do `Filter` atividade. | Cadeia | Sim
tipo | Tem de ser definido como **filtro**. | Cadeia | Sim
condição | Condição a utilizar para a entrada de filtragem. | Expressão | Sim
itens | Matriz de entrada em que o filtro deve ser aplicado. | Expressão | Sim

## <a name="example"></a>Exemplo

Neste exemplo, o pipeline com duas atividades: **filtro** e **ForEach**. A atividade do filtro está configurada para filtrar a matriz de entrada para itens com um valor superior a 3. Em seguida, a atividade ForEach itera repetidamente os valores filtrados e aguarda o número de segundos especificado pelo valor atual.

```json
{
    "name": "PipelineName",
    "properties": {
        "activities": [{
                "name": "MyFilterActivity",
                "type": "filter",
                "typeProperties": {
                    "condition": "@greater(item(),3)",
                    "items": "@pipeline().parameters.inputs"
                }
            },
            {
                "name": "MyForEach",
                "type": "ForEach",
                "typeProperties": {
                    "isSequential": "false",
                    "batchCount": 1,
                    "items": "@activity('MyFilterActivity').output.value",
                    "activities": [{
                        "type": "Wait",
                        "typeProperties": {
                            "waitTimeInSeconds": "@item()"
                        },
                        "name": "MyWaitActivity"
                    }]
                },
                "dependsOn": [{
                    "activity": "MyFilterActivity",
                    "dependencyConditions": ["Succeeded"]
                }]
            }
        ],
        "parameters": {
            "inputs": {
                "type": "Array",
                "defaultValue": [1, 2, 3, 4, 5, 6]
            }
        }
    }
}
```

## <a name="next-steps"></a>Passos Seguintes
Outras atividades de fluxo de controlo suportadas pela fábrica de dados, consulte: 

- [Atividade Se Condição](control-flow-if-condition-activity.md)
- [Atividade Executar Pipeline](control-flow-execute-pipeline-activity.md)
- [Para cada atividade](control-flow-for-each-activity.md)
- [Atividade Obter Metadados](control-flow-get-metadata-activity.md)
- [Atividade de Pesquisa](control-flow-lookup-activity.md)
- [Atividade de Web](control-flow-web-activity.md)
- [Atividade Until](control-flow-until-activity.md)
