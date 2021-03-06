---
title: Aguarde que a atividade no Azure Data Factory | Microsoft Docs
description: A atividade de espera interrompe a execução de pipeline para o período de tempo especificado.
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
ms.date: 01/10/2018
ms.author: shlo
ms.openlocfilehash: 74a5d687535915fab7d518faaf916b98ab262c4b
ms.sourcegitcommit: 0c490934b5596204d175be89af6b45aafc7ff730
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/27/2018
ms.locfileid: "37053903"
---
# <a name="wait-activity-in-azure-data-factory"></a>Aguarde que a atividade no Azure Data Factory
Quando utiliza uma atividade Aguardar num pipeline, este aguarda o período de tempo especificado antes de continuar a execução das atividades subsequentes. 

## <a name="syntax"></a>Sintaxe

```json
{
    "name": "MyWaitActivity",
    "type": "Wait",
    "typeProperties": {
        "waitTimeInSeconds": 1
    }
}

```

## <a name="type-properties"></a>Propriedades do tipo

Propriedade | Descrição | Valores permitidos | Necessário
-------- | ----------- | -------------- | --------
nome | Nome do `Wait` atividade. | Cadeia | Sim
tipo | Tem de ser definido como **aguarde**. | Cadeia | Sim
waitTimeInSeconds | O número de segundos que o pipeline aguarda antes de continuar com o processamento. | Número inteiro | Sim

## <a name="example"></a>Exemplo

> [!NOTE]
> Esta secção fornece definições de JSON e comandos do PowerShell de exemplo para executar o pipeline. Para obter instruções com instruções passo a passo para criar um pipeline de fábrica de dados utilizando as definições do Azure PowerShell e JSON, consulte [tutorial: criar uma fábrica de dados utilizando o Azure PowerShell](quickstart-create-data-factory-powershell.md).

### <a name="pipeline-with-wait-activity"></a>Pipeline com atividade de espera
Neste exemplo, o pipeline com duas atividades: **até** e **aguarde**. A atividade de espera está configurada para aguardar por um segundo. O pipeline é executado a atividade de Web num ciclo com um segundo, à espera que o tempo entre cada execução. 

```json
{
    "name": "DoUntilPipeline",
    "properties": {
        "activities": [
            {
                "type": "Until",
                "typeProperties": {
                    "expression": {
                        "value": "@equals('Failed', coalesce(body('MyUnauthenticatedActivity')?.status, actions('MyUnauthenticatedActivity')?.status, 'null'))",
                        "type": "Expression"
                    },
                    "timeout": "00:00:01",
                    "activities": [
                        {
                            "name": "MyUnauthenticatedActivity",
                            "type": "WebActivity",
                            "typeProperties": {
                                "method": "get",
                                "url": "https://www.fake.com/",
                                "headers": {
                                    "Content-Type": "application/json"
                                }
                            },
                            "dependsOn": [
                                {
                                    "activity": "MyWaitActivity",
                                    "dependencyConditions": [ "Succeeded" ]
                                }
                            ]
                        },
                        {
                            "type": "Wait",
                            "typeProperties": {
                                "waitTimeInSeconds": 1
                            },
                            "name": "MyWaitActivity"
                        }
                    ]
                },
                "name": "MyUntilActivity"
            }
        ]
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
