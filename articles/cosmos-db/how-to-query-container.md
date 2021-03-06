---
title: Consultar contentores no Azure Cosmos DB
description: Como consultar contentores no Azure Cosmos DB
services: cosmos-db
author: markjbrown
ms.service: cosmos-db
ms.topic: sample
ms.date: 11/06/2018
ms.author: mjbrown
ms.openlocfilehash: 5d64aa8b50cdde23d1bb8980510cfac202204f9a
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/07/2018
ms.locfileid: "51262459"
---
# <a name="query-containers-in-azure-cosmos-db"></a>Consultar contentores no Azure Cosmos DB

Este artigo explica como consultar um contentor (coleção, gráfico, tabela) no Azure Cosmos DB.

## <a name="in-partition-query"></a>Consulta na partição

Quando consulta dados a partir de contentores, o Azure Cosmos DB encaminha automaticamente a consulta para as partições que correspondem aos valores das chaves de partições especificados no filtro (se existirem). Por exemplo, esta consulta é encaminhada para a partição que contém a chave de partição "XMS-0001".

```csharp
// Query using partition key into a class called, DeviceReading
IQueryable<DeviceReading> query = client.CreateDocumentQuery<DeviceReading>(
    UriFactory.CreateDocumentCollectionUri("myDatabaseName", "myCollectionName"))
    .Where(m => m.MetricType == "Temperature" && m.DeviceId == "XMS-0001");
```

## <a name="cross-partition-query"></a>Consulta entre partições

A seguinte consulta não tem um filtro para a chave de partição (DeviceId) e é distribuída para todas as partições onde é executada no índice da partição. Para executar uma consulta entre partições, defina **EnableCrossPartitionQuery** como verdadeira (ou x-ms-documentdb-query-enablecrosspartition na API REST).

```csharp
// Query across partition keys into a class called, DeviceReading
IQueryable<DeviceReading> crossPartitionQuery = client.CreateDocumentQuery<DeviceReading>(
    UriFactory.CreateDocumentCollectionUri("myDatabaseName", "myCollectionName"),
    new FeedOptions { EnableCrossPartitionQuery = true })
    .Where(m => m.MetricType == "Temperature" && m.MetricValue > 100);
```

O cosmos DB suporta as funções de agregação COUNT (contagem), MIN (mínimo), MAX (máximo) e AVG (média) através dos contentores com o SQL. As funções de agregação através de contentores a partir do SDK versão 1.12.0 e superior. As consultas têm de incluir um operador de agregação único e têm de incluir um valor único na projeção.

## <a name="parallel-cross-partition-query"></a>Consulta entre partições paralelas

Os SDKs do Cosmos DB 1.9.0 e superior suportam opções de execução de consulta paralela.  As consultas entre partições paralelas permitem-lhe realizar consultas entre partições de baixa latência. Por exemplo, a seguinte consulta está configurada para ser executada em paralelo em várias partições.

```csharp
// Cross-partition Order By Query with parallel execution
IQueryable<DeviceReading> crossPartitionQuery = client.CreateDocumentQuery<DeviceReading>(
    UriFactory.CreateDocumentCollectionUri("myDatabaseName", "myCollectionName"),  
    new FeedOptions { EnableCrossPartitionQuery = true, MaxDegreeOfParallelism = 10, MaxBufferedItemCount = 100})
    .Where(m => m.MetricType == "Temperature" && m.MetricValue > 100)
    .OrderBy(m => m.MetricValue);
```

Pode gerir a execução paralela da consulta ao otimizar os parâmetros abaixo:

- **MaxDegreeOfParallelism**: define o número máximo de ligações de rede simultâneas às partições do contentor. Se definir esta propriedade como -1, o nível de paralelismo é gerido pelo SDK. Se MaxDegreeOfParallelism não for especificado ou for definido como 0, que é o valor predefinido, existirá uma única ligação de rede às partições do contentor.

- **MaxBufferedItemCount**: negoceia a latência da consulta em relação à utilização da memória do lado do cliente. Se esta opção for omitida ou definida como -1, o número de itens colocados em memória intermédia durante a execução paralela da consulta é gerido pelo SDK.

Com o mesmo estado da coleção, uma consulta paralela devolverá resultados pela mesma ordem que uma execução em série. Quando realizar uma consulta em várias partições que inclua operadores de ordenação (ORDER BY e/ou TOP), o SDK do Azure Cosmos DB emite a consulta em paralelo para as várias partições e combina os resultados ordenados parcialmente no lado do cliente, de modo a produzir resultados ordenados globalmente.

## <a name="next-steps"></a>Passos seguintes

Veja os seguintes artigos para saber mais sobre a criação de partições no Cosmos DB:

- [Criação de partições no Azure Cosmos DB](partitioning-overview.md)
- [Chaves de partição sintéticas no Azure Cosmos DB](synthetic-partition-keys.md)
