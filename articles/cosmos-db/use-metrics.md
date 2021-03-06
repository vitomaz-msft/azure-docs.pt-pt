---
title: Monitor e de depuração com métricas na Azure Cosmos DB | Documentos da Microsoft
description: Utilize métricas no Azure Cosmos DB para depurar problemas comuns e monitorizar a base de dados.
keywords: métricas
services: cosmos-db
author: kanshiG
manager: kfile
editor: ''
ms.service: cosmos-db
ms.devlang: na
ms.topic: conceptual
ms.date: 11/15/2018
ms.author: govindk
ms.openlocfilehash: 5e2ff89c45797e8453a3a3e4da4ac7cbe1ee6f16
ms.sourcegitcommit: 7804131dbe9599f7f7afa59cacc2babd19e1e4b9
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/17/2018
ms.locfileid: "51852882"
---
# <a name="monitor-and-debug-with-metrics-in-azure-cosmos-db"></a>Monitor e de depuração com métricas na Azure Cosmos DB

O Azure Cosmos DB fornece as métricas de débito, armazenamento, consistência, disponibilidade e latência. O [portal do Azure](https://portal.azure.com) fornece uma exibição agregada destas métricas. Para métricas mais granulares, o SDK de cliente e o [registos de diagnóstico](./logging.md) estão disponíveis.

Este artigo explica-casos de utilização comuns e como as métricas do Azure Cosmos DB podem ser usadas para analisar e depurar esses problemas. As métricas são recolhidas a cada cinco minutos e são mantidas durante sete dias.

## <a name="understand-how-many-requests-are-succeeding-or-causing-errors"></a>Compreender o número de pedidos está a ter êxito ou a que está causando erros

Para começar, vá para o [portal do Azure](https://portal.azure.com) e navegue para o **métricas** painel. No painel, localize a **número de pedidos excedeu a capacidade por 1 minuto** gráfico. Este gráfico mostra um minuto a minuto total de pedidos segmentados pelo código de estado. Para obter mais informações sobre códigos de estado HTTP, consulte [códigos de estado HTTP para o Azure Cosmos DB](https://docs.microsoft.com/rest/api/cosmos-db/http-status-codes-for-cosmosdb).

O código de estado de erro mais comuns é 429 (limitação/limitação da taxa de). Este erro significa que os pedidos ao Azure Cosmos DB são mais do que o débito aprovisionado. A solução mais comuns para esse problema é [aumentar verticalmente o RUs](./set-throughput.md) para determinada coleção.

![Número de pedidos por minuto](media/use-metrics/metrics-12.png)

## <a name="determine-the-throughput-distribution-across-partitions"></a>Determinar a distribuição de débito entre partições

Ter uma boa cardinalidade das suas chaves de partição é essencial para qualquer aplicação dimensionável. Para determinar a distribuição de taxa de transferência de qualquer contentor particionado dividida por partições, navegue para o **painel de métricas** no [portal do Azure](https://portal.azure.com). Na **débito** separador, a divisão de armazenamento é mostrada na **RU/segundo máximos consumidos por cada partição física** gráfico. O gráfico seguinte ilustra um exemplo de uma distribuição fraco de dados, conforme exibido a partição distorcida mais à esquerda.

![Ver o uso intenso às 15:00: 05 de partição única](media/use-metrics/metrics-17.png)

Pode fazer com que uma distribuição desigual de débito *frequente* partições, que podem resultar em pedidos limitados e podem exigir a repartição. Para obter mais informações sobre a criação de partições no Azure Cosmos DB, consulte [particionar e dimensionar no Azure Cosmos DB](./partition-data.md).

## <a name="determine-the-storage-distribution-across-partitions"></a>Determinar a distribuição de armazenamento em várias partições

Ter uma boa cardinalidade da sua partição é essencial para qualquer aplicação dimensionável. Para determinar a distribuição de taxa de transferência de qualquer contentor particionado dividida por partições, vá para o painel de métricas no [portal do Azure](https://portal.azure.com). No separador de débito, a divisão de armazenamento é mostrada no RU/segundo máximos consumidos por cada gráfico de partição física. O gráfico seguinte ilustra uma distribuição fraco de dados, conforme exibido a partição distorcida mais à esquerda.

![Exemplo de distribuição de dados fraca](media/use-metrics/metrics-07.png)

Pode causa ao qual chave de partição é a inclinação a distribuição clicando numa partição no gráfico.

![Chave de partição é a inclinação de distribuição](media/use-metrics/metrics-05.png)

Depois de identificar qual chave de partição está a causar a distorção na distribuição, poderá ter de criar novas partições seu contentor com uma chave de partição mais distribuída. Para obter mais informações sobre a criação de partições no Azure Cosmos DB, consulte [particionar e dimensionar no Azure Cosmos DB](./partition-data.md).

## <a name="compare-data-size-against-index-size"></a>Comparar o tamanho dos dados contra o tamanho do índice

No Azure Cosmos DB, o armazenamento total consumido é a combinação do tamanho dos dados e o tamanho do índice. Normalmente, o tamanho do índice é uma fração do tamanho dos dados. No painel de métricas a [portal do Azure](https://portal.azure.com), separador armazenamento demonstra a divisão do consumo de armazenamento com base nos dados e índice.

```csharp
// Measure the document size usage (which includes the index size)  
ResourceResponse<DocumentCollection> collectionInfo = await client.ReadDocumentCollectionAsync(UriFactory.CreateDocumentCollectionUri("db", "coll"));
 Console.WriteLine("Document size quota: {0}, usage: {1}", collectionInfo.DocumentQuota, collectionInfo.DocumentUsage);
```

Se desejar conservar espaço de índice, pode ajustar os [política de indexação](index-policy.md).

## <a name="debug-why-queries-are-running-slow"></a>Depuração, por que consultas estão lentas

Os SDKs de API de SQL do Azure Cosmos DB fornece estatísticas de execução de consulta.

```csharp
IDocumentQuery<dynamic> query = client.CreateDocumentQuery(
 UriFactory.CreateDocumentCollectionUri(DatabaseName, CollectionName),
 "SELECT * FROM c WHERE c.city = 'Seattle'",
 new FeedOptions
 {
 PopulateQueryMetrics = true,
 MaxItemCount = -1,
 MaxDegreeOfParallelism = -1,
 EnableCrossPartitionQuery = true
 }).AsDocumentQuery();
FeedResponse<dynamic> result = await query.ExecuteNextAsync();

// Returns metrics by partition key range Id
IReadOnlyDictionary<string, QueryMetrics> metrics = result.QueryMetrics;
```

*QueryMetrics* fornece detalhes sobre cada componente da consulta de quanto tempo demorou a execução. A causa mais comum para consultas de execução longa é análises, que significa que a consulta não foi possível ao tirar partido de índices. Esse problema pode ser resolvido com uma condição de filtro melhor.

## <a name="next-steps"></a>Passos Seguintes

Agora já aprendeu como monitorizar e problemas de depuração com as métricas fornecidas no portal do Azure. Pode querer saber mais sobre como melhorar o desempenho da base de dados ao ler os artigos seguintes:

* [Desempenho e dimensionamento testes com o Azure Cosmos DB](performance-testing.md)
* [Sugestões de desempenho para o Azure Cosmos DB](performance-tips.md)
