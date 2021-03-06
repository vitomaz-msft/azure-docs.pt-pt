---
title: Exemplos de CLI do Azure para o Azure Cosmos DB | Microsoft Docs
description: Exemplos de CLI do Azure – Criar e gerir contas, bases de dados, contentores, regiões e firewalls do Azure Cosmos DB.
author: markjbrown
ms.service: cosmos-db
ms.topic: sample
ms.date: 10/26/2018
ms.author: mjbrown
ms.openlocfilehash: 461207d0c9d27ed645dcac98e6256431bb23f8ad
ms.sourcegitcommit: 00dd50f9528ff6a049a3c5f4abb2f691bf0b355a
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/05/2018
ms.locfileid: "51005993"
---
# <a name="azure-cli-samples-for-azure-cosmos-db"></a>Exemplos da CLI do Azure para o Azure Cosmos DB

A tabela seguinte inclui ligações para scripts da CLI do Azure de exemplo para o Azure Cosmos DB. As páginas de referência para todos os comandos da CLI do Azure Cosmos DB encontram-se disponíveis em [Referência da CLI do Azure](/cli/azure/cosmosdb).

| |  |
|---|---|
|**Criar a conta, a base de dados e os contentores do Azure Cosmos DB**||
| [Criar uma conta da API de SQL](scripts/create-database-account-collections-cli.md?toc=%2fcli%2fazure%2ftoc.json)| Cria a conta, a base de dados e o contentor de uma API de SQL do Azure Cosmos DB única. |
| [Criar uma conta da API de MongoDB](scripts/create-mongodb-database-account-cli.md?toc=%2fcli%2fazure%2ftoc.json) | Cria a conta, a base de dados e a coleção de uma API de MongoDB do Azure Cosmos DB única. |
| [Criar uma conta da API Gremlin](scripts/create-gremlin-database-account-cli.md?toc=%2fcli%2fazure%2ftoc.json) | Cria a conta, a base de dados e o grafo de uma API Gremlin do Azure Cosmos DB única. |
| [Criar uma conta da API para Cassandra](scripts/create-cassandra-database-account-cli.md?toc=%2fcli%2fazure%2ftoc.json) | Cria uma única conta e base de dados da API para Cassandra do Azure Cosmos DB. |
| [Criar uma conta da API de Tabela](scripts/create-table-database-account-cli.md?toc=%2fcli%2fazure%2ftoc.json) | Cria uma conta, base de dados e tabela de API de Tabela do Azure Cosmos DB única. |
|**Dimensionar o Azure Cosmos DB**||
| [Dimensionar o débito do contentor](scripts/scale-collection-throughput-cli.md?toc=%2fcli%2fazure%2ftoc.json) | Altera o débito aprovisionado num contentor.|
| [Replicar a conta de base de dados do Azure Cosmos DB em várias regiões e configurar prioridades de ativação pós-falha](scripts/scale-multiregion-cli.md?toc=%2fcli%2fazure%2ftoc.json)|Replica globalmente os dados da conta em várias regiões com uma prioridade de ativação pós-falha especificada.|
|**Proteger o Azure Cosmos DB**||
| [Obter chaves de contas](scripts/secure-get-account-key-cli.md?toc=%2fcli%2fazure%2ftoc.json) | Obtém as chaves de escrita mestra primária e secundária, e as chaves só de leitura primária e secundária da conta.|
| [Obter a cadeia de ligação do MongoDB](scripts/secure-mongo-connection-string-cli.md?toc=%2fcli%2fazure%2ftoc.json) | Obtém a cadeia de ligação para ligar a aplicação MongoDB à sua conta do Azure Cosmos DB.|
| [Voltar a gerar chaves da conta](scripts/secure-regenerate-key-cli.md?toc=%2fcli%2fazure%2ftoc.json)|Regenere as chaves da conta.|
| [Criar uma firewall](scripts/create-firewall-cli.md?toc=%2fcli%2fazure%2ftoc.json)| Crie uma política de controlo de acesso IP de entrada para limitar o acesso à conta a partir de um conjunto aprovado de computadores e/ou serviços cloud.|
|**Elevada disponibilidade, recuperação após desastre, cópia de segurança e restauro**||
| [Configurar política de ativação pós-falha](scripts/ha-failover-policy-cli.md?toc=%2fcli%2fazure%2ftoc.json)|Define a prioridade de ativação pós-falha de cada região em que a conta é replicada.|
|**Ligar o Azure Cosmos DB a recursos**||
| [Ligar uma aplicação Web ao Azure Cosmos DB](../app-service/scripts/app-service-cli-app-service-documentdb.md?toc=%2fcli%2fazure%2ftoc.json)|Crie e ligue uma base de dados do Azure Cosmos DB e uma aplicação Web do Azure.|
|||
