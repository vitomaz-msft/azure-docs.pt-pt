---
title: O Azure Cosmos DB como um arquivo de chave-valor – descrição geral de custos | Documentos da Microsoft
description: Saiba mais sobre o baixo custo de utilização do Azure Cosmos DB como um arquivo de chave-valor.
keywords: arquivo de chave-valor
services: cosmos-db
author: SnehaGunda
manager: kfile
tags: ''
ms.service: cosmos-db
ms.devlang: na
ms.topic: conceptual
ms.date: 11/15/2017
ms.author: sngun
ms.openlocfilehash: 09abc89c8e4cc8b0ee79200739722fc62d1ece4b
ms.sourcegitcommit: ebf2f2fab4441c3065559201faf8b0a81d575743
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/20/2018
ms.locfileid: "52163205"
---
# <a name="azure-cosmos-db-as-a-key-value-store--cost-overview"></a>O Azure Cosmos DB como um arquivo de chave-valor – descrição geral de custo

O Azure Cosmos DB é um serviço de base de dados com múltiplos modelos distribuída globalmente, para criar aplicativos altamente disponíveis, em grande escala facilmente. Por predefinição, o Azure Cosmos DB indexa automaticamente todos os dados que ingere, com eficiência. Isto permite uma rápida e consistente [SQL](how-to-sql-query.md) (e [JavaScript](programming.md)) consultas em qualquer tipo de dados. 

Este artigo descreve o custo do Azure Cosmos DB para escrita simple e operações de leitura quando é utilizado como um arquivo de chave/valor. Escreva operações incluem inserções, substitui, eliminações e upserts de documentos. Além de garantir uma 99,99% SLA de disponibilidade para todas as contas de região única e todas as contas de várias regiões com consistência flexível e 99,999% de disponibilidade de leitura em todas as contas de base de dados de várias regiões, a garantia de ofertas do Azure Cosmos DB < 10 ms de latência para lê e < 15 ms de latência para (indexadas), respectivamente, escreve no percentil 99. 

## <a name="why-we-use-request-units-rus"></a>Por que podemos usar unidades de pedido (RUs)

Desempenho do Cosmos DB do Azure baseia-se na quantidade de aprovisionado [unidades de pedido](request-units.md) (RU) para a partição. O aprovisionamento é com uma granularidade segundo e é comprado em RUs/seg ([não deve ser confundido com faturação à hora](https://azure.microsoft.com/pricing/details/cosmos-db/)). RUs devem ser consideradas como uma moeda que simplifica o aprovisionamento do débito necessário para a aplicação. Os nossos clientes não é necessário pensar diferenciar entre a leitura e escrita de unidades de capacidade. O modelo de moeda único de RUs cria eficiências para partilhar a capacidade aprovisionada entre leituras e gravações. Esse modelo de capacidade aprovisionada permite que o serviço proporcionar um débito previsível e consistente, a garantia de baixa latência e elevada disponibilidade. Por fim, podemos usar RU para débito de modelo, mas cada RU aprovisionado também tem uma quantidade definida de recursos (memória, Core). RU/seg não é apenas de IOPS.

Como um sistema de base de dados globalmente distribuída, o Cosmos DB é o serviço apenas do Azure que fornece um SLA na latência, débito e consistência, além de elevada disponibilidade. O débito que Aprovisiona é aplicado a cada uma das regiões à sua conta de base de dados do Cosmos DB. Para leituras, o Cosmos DB oferece várias bem definidas [níveis de consistência](consistency-levels.md) para sua escolha. 

A tabela seguinte mostra que o número de RUs necessários para efetuar a leitura e com base no tamanho do documento de 1 KB e 100 KBs de transações de escrita.

|Tamanho do item|1 Read|1 write|
|-------------|------|-------|
|1 KB|1 RU|5 RUs|
|100 KB|10 RUs|50 RUs|

## <a name="cost-of-reads-and-writes"></a>Custo de leituras e gravações

Se aprovisionar 1.000 RU/seg, este quantidades m 3.6 RU/hora e irá custar US $0,08 durante a hora (nos EUA e Europa). Para um documento de 1 KB de tamanho, isso significa que pode consumir 3.6-m leituras ou escritas 0,72-m (3.6 m RU / 5) usando o débito aprovisionado. Normalizados para milhões de leituras e gravações, o custo seria US $0.022 /m leituras (US $0,08 / 3.6) e escritas de US $0.111/ m (US $0,08 / 0,72). O custo por milhões se torna um mínimo de conforme mostrado na tabela abaixo.

|Tamanho do item|Leitura de 1-m|Escrita de 1-m|
|-------------|-------|--------|
|1 KB|$0.022|$0.111|
|100 KB|$0.222|$1.111|


Maioria dos BLOBs básico ou objeto arquivos de serviços encargos 0,40 us $ por milhão de transações de leitura e US $5 por transações de escrita de milhões. Se utilizou de forma ideal, o Cosmos DB pode ser até 98% mais barato do que essas outras soluções (para transações de 1 KB).

## <a name="next-steps"></a>Passos Seguintes

Fique atento para novos artigos sobre a otimização do aprovisionamento de recursos do Azure Cosmos DB. Enquanto isso, fique à vontade utilizar o nosso [Calculadora de RU](https://www.documentdb.com/capacityplanner).

