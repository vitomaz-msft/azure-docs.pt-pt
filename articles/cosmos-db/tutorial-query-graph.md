---
title: Como consultar dados de gráfico no Azure Cosmos DB? | Microsoft Docs
description: Saiba como consultar dados de gráfico no Azure Cosmos DB
services: cosmos-db
author: luisbosquez
manager: kfile
editor: ''
tags: ''
ms.service: cosmos-db
ms.component: cosmosdb-graph
ms.devlang: na
ms.topic: tutorial
ms.date: 01/02/2018
ms.author: lbosq
ms.custom: mvc
ms.openlocfilehash: e3ed90d0b706e742588a5a0966d9ac3bda44ecbd
ms.sourcegitcommit: cb61439cf0ae2a3f4b07a98da4df258bfb479845
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/05/2018
ms.locfileid: "43697447"
---
# <a name="tutorial-query-azure-cosmos-db-gremlin-api-by-using-gremlin"></a>Tutorial: Consultar a API do Gremlin do Azure Cosmos DB através do Gremlin

A [API do Gremlin](graph-introduction.md) do Azure Cosmos DB suporta consultas [Gremlin](https://github.com/tinkerpop/gremlin/wiki). Este artigo fornece documentos e consultas de exemplo para poder começar. É fornecida uma referência ao Gremlin detalhada no artigo [Suporte para Gremlin](gremlin-support.md).

Este artigo abrange as seguintes tarefas: 

> [!div class="checklist"]
> * Consultar dados com o Gremlin

## <a name="prerequisites"></a>Pré-requisitos

Para estas consultas funcionarem, tem de ter uma conta do Azure Cosmos DB e dados de gráfico no contentor. Não tem qualquer um destes? Conclua o [guia de introdução de 5 minutos](create-graph-dotnet.md) ou o [tutorial do programador](tutorial-query-graph.md) para criar uma conta e povoar a base de dados. Pode executar as seguintes consultas com a [consola do Gremlin](https://tinkerpop.apache.org/docs/current/reference/#gremlin-console) ou o seu controlador Gremlin favorito.

## <a name="count-vertices-in-the-graph"></a>Contar vértices no gráfico

O fragmento seguinte mostra como contabilizar o número de vértices no gráfico:

```
g.V().count()
```

## <a name="filters"></a>Filtros

Pode executar filtros através dos passos `has` e `hasLabel` do Gremlin e combiná-los com `and`, `or` e `not` para criar filtros mais complexos. O Azure Cosmos DB fornece uma indexação independente de esquema de todas as propriedades nos vértices e graus para consultas rápidas:

```
g.V().hasLabel('person').has('age', gt(40))
```

## <a name="projection"></a>Projeção

Pode projetar algumas propriedades nos resultados da consulta com o passo `values`:

```
g.V().hasLabel('person').values('firstName')
```

## <a name="find-related-edges-and-vertices"></a>Localizar arestas e vértices relacionados

Até ao momento, apenas vimos operadores de consulta que funcionam em qualquer base de dados. Os gráficos são rápidos e eficientes para operações transversais quando precisar de navegar para arestas e vértices relacionados. Vamos localizar todos os amigos do Tomás. Podemos fazê-lo através do passo `outE` do Gremlin para localizar todas as arestas exteriores do Tomás e atravessá-las nos vértices interiores dessas arestas através do passo `inV` do Gremlin:

```cs
g.V('thomas').outE('knows').inV().hasLabel('person')
```

A consulta seguinte executa dois saltos para localizar todos os "amigos de amigos" do Tomás ao chamar `outE` e `inV` duas vezes. 

```cs
g.V('thomas').outE('knows').inV().hasLabel('person').outE('knows').inV().hasLabel('person')
```

Pode criar consultas mais complexas e implementar uma lógica de gráfico transversal poderosa com o Gremlin, incluindo misturar expressões de filtro, efetuar o ciclo com o passo `loop` e implementar a navegação condicional com o passo `choose`. Saiba mais sobre o que pode fazer com o [Suporte para Gremlin](gremlin-support.md)!

## <a name="next-steps"></a>Passos seguintes

Neste tutorial, fez o seguinte:

> [!div class="checklist"]
> * Aprendeu a fazer consultas com o Graph 

Agora, pode avançar para a secção Conceitos para obter mais informações sobre o Cosmos DB.

> [!div class="nextstepaction"]
> [Distribuição global](distribute-data-globally.md) 

