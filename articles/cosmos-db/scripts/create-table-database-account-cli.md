---
title: Script da CLI do Azure - Criar uma conta, base de dados e tabela da API de Tabela do Azure Cosmos DB | Microsoft Docs
description: Exemplo do Script da CLI do Azure - Criar uma conta, base de dados e tabela da API de Tabela do Azure Cosmos DB
author: markjbrown
ms.service: cosmos-db
ms.component: cosmosdb-table
ms.topic: sample
ms.date: 10/26/2018
ms.author: mjbrown
ms.openlocfilehash: 178ecfe6971a47009781d8866986d936f8e3a5d2
ms.sourcegitcommit: 00dd50f9528ff6a049a3c5f4abb2f691bf0b355a
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/05/2018
ms.locfileid: "51019587"
---
# <a name="azure-cosmos-db-create-a-table-api-account-using-azure-cli"></a>Azure Cosmos DB: Criar uma conta da API de Tabela com a CLI do Azure

Este script da CLI de exemplo cria uma conta, base de dados e tabela da API de Tabela do Azure Cosmos DB.

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Se optar por instalar e usar a CLI localmente, este tópico requer a execução da versão 2.0 ou posterior da CLI do Azure. Executar `az --version` para localizar a versão. Se precisar de instalar ou atualizar, veja [Instalar a CLI do Azure](/cli/azure/install-azure-cli).

## <a name="sample-script"></a>Script de exemplo

[!code-azurecli-interactive[main](../../../cli_scripts/cosmosdb/create-cosmosdb-table-account/create-cosmosdb-table-account.sh "Create an Azure Cosmos DB Table API account, database, and table.")]

## <a name="clean-up-deployment"></a>Limpar a implementação

Depois de executar o script de exemplo, pode ser utilizado o seguinte comando para remover o grupo de recursos e todos os recursos associados ao mesmo.

```azurecli-interactive
az group delete --name $resourceGroupName
```

## <a name="script-explanation"></a>Explicação do script

Este script utiliza os seguintes comandos. Cada comando na tabela liga à documentação específica do comando.

| Comando | Notas |
|---|---|
| [az group create](/cli/azure/group#az-group-create) | Cria um grupo de recursos no qual todos os recursos são armazenados. |
| [az cosmosdb create](/cli/azure/cosmosdb#az-cosmosdb-create) | Cria uma conta do Azure Cosmos DB. |
| [az cosmosdb database create](/cli/azure/cosmosdb/database#az-cosmosdb-database-create) | Cria uma base de dados do Azure Cosmos DB. |
| [az cosmosdb collection create](/cli/azure/cosmosdb/collection#az-cosmosdb-collection-create) | Cria uma tabela do Azure Cosmos DB. |
| [az group delete](/cli/azure/resource#az-resource-delete) | Elimina um grupo de recursos, incluindo todos os recursos aninhados. |

## <a name="next-steps"></a>Passos seguintes

Para obter mais informações sobre a CLI do Azure, veja [Documentação da CLI do Azure](/cli/azure).

Pode ver exemplos do script da CLI do Azure Cosmos DB adicionais na [documentação da CLI do Azure Cosmos DB](../cli-samples.md).
