---
title: Utilizar mongoimport e mongorestore com a API do Azure Cosmos DB para MongoDB | Microsoft Docs
description: Saiba como utilizar mongoimport e mongorestore para importar dados para uma API para a conta do MongoDB
keywords: mongoimport, mongorestore
services: cosmos-db
author: SnehaGunda
ms.service: cosmos-db
ms.component: cosmosdb-mongo
ms.devlang: na
ms.topic: tutorial
ms.date: 05/07/2018
ms.author: sngun
ms.custom: mvc
ms.openlocfilehash: 13422434e6392ec7681ec4478533c45a84f40c9a
ms.sourcegitcommit: 275eb46107b16bfb9cf34c36cd1cfb000331fbff
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/15/2018
ms.locfileid: "51706981"
---
# <a name="tutorial-migrate-your-data-to-azure-cosmos-db-mongodb-api-account"></a>Tutorial: migrar os dados para a conta da API do MongoDB do Azure Cosmos DB

Este tutorial fornece instruções sobre como migrar os dados armazenados no MongoDB para a conta da API do MongoDB do Azure Cosmos DB. Se estiver a importar dados do MongoDB e pretender utilizá-los com a API do Azure Cosmos DB, deve utilizar a [ferramenta Migração de Dados](import-data.md) para a importação.

Este tutorial abrange as seguintes tarefas:

> [!div class="checklist"]
> * Planear a migração
> * Pré-requisitos de migração
> * Migrar dados com o mongoimport
> * Migrar dados com o mongorestore

Antes de migrar dados para a conta da API do MongoDB, certifique-se de que tem alguns dados do MongoDB de exemplo. Se não tiver uma base de dados do MongoDB de exemplo, pode transferir e instalar o [servidor de Comunidade do MongoDB](https://www.mongodb.com/download-center), criar uma base de dados de exemplo e utilizar o mongoimport.exe ou mongorestore.exe para carregar dados de exemplo. 

## <a name="plan-for-migration"></a>Planear a migração

1. Crie previamente e dimensione as suas coleções:
        
   * Por predefinição, o Azure Cosmos DB aprovisiona uma coleção do MongoDB nova com mil unidades de pedido por segundo (RU/seg.). Antes de começar a migração com mongoimport, mongorestore, crie previamente todas as coleções a partir do [portal do Azure](https://portal.azure.com) ou dos controladores e ferramentas do MongoDB. Se o tamanho dos dados for superior a 10 GB, certifique-se de que cria uma [coleção particionada](partition-data.md) com uma chave de fragmentação adequada. Recomenda o MongoDB para armazenar dados de entidade em coleções. Pode colocalizar entites de débito de tamanho e aprovisionar comparável ao nível da base de dados do Cosmos do Azure.

   * Partir do [portal do Azure](https://portal.azure.com), aumentar o débito de coleções do 1000 RUs/segundo, para uma coleção de partição única e 2.500 RUs/seg para uma coleção em partição horizontal apenas durante o período de migração. Com o débito mais elevado, pode evitar limitações de velocidade e realizar a migração em menos tempo. Pode reduzir o débito imediatamente após a migração, para reduzir os custos.

   * Para além de aprovisionar RUs/seg. ao nível da coleção, também pode aprovisionar RU/seg. para um conjunto de coleções ao nível da base de dados principal. Para tal, é necessário criar previamente a base de dados e as coleções, bem como definir uma chave fragmentada para cada coleção.

   * Pode criar coleções fragmentadas através do seu controlador, da sua ferramenta ou do seu SDK preferido. Neste exemplo, utilizamos a shell do Mongo para criar uma coleção fragmentada:

        ```bash
        db.runCommand( { shardCollection: "admin.people", key: { region: "hashed" } } )
        ```
    
        Resultados:

        ```JSON
        {
            "_t" : "ShardCollectionResponse",
            "ok" : 1,
            "collectionsharded" : "admin.people"
        }
        ```

1. Calcule os custos de RU aproximados para uma escrita em documento única:

   a. Ligue à conta API de MongoDB do Azure Cosmos DB a partir da shell do MongoDB. Pode encontrar instruções em [Connect a MongoDB application to Azure Cosmos DB](connect-mongodb-account.md) (Ligar uma aplicação MongoDB ao Azure Cosmos DB).
    
   b. Utilize um dos seus documentos de exemplo para executar um comando de inserção de exemplo a partir da shell do MongoDB:
   
      ```bash
      db.coll.insert({ "playerId": "a067ff", "hashedid": "bb0091", "countryCode": "hk" })
      ```
        
   c. Execute ```db.runCommand({getLastRequestStatistics: 1})``` e receberá uma resposta semelhante à seguinte:
     
      ```bash
        globaldb:PRIMARY> db.runCommand({getLastRequestStatistics: 1})
        {
            "_t": "GetRequestStatisticsResponse",
            "ok": 1,
            "CommandName": "insert",
            "RequestCharge": 10,
            "RequestDurationInMilliSeconds": NumberLong(50)
        }
      ```
        
    d. Tome nota do custo do pedido.
    
1. Determine a latência do seu computador para o serviço cloud do Azure Cosmos DB:
    
    a. Utilize o comando ```setVerboseShell(true)``` para ativar o registo verboso a partir da shell do MongoDB.
    
    b. Execute uma consulta simples na base de dados ```db.coll.find().limit(1)```. Receberá uma resposta semelhante à seguinte:

       ```bash
       Fetched 1 record(s) in 100(ms)
       ```
        
1. Remova o documento inserido antes da migração para garantir que não existem documentos duplicados. Pode remover documentos com o comando: ```db.coll.remove({})```

1. Calcular os valores de *batchSize* e *numInsertionWorkers* aproximados:

    * Para *batchSize*, divida as RUs totais aprovisionadas pelas RUs que a escrita única no documento consumiu, no passo 3.
    
    * Se o *batchSize* calculado for <= 24, utilize esse número como o valor do *batchSize*.
    
    * Se o *batchSize* calculado for > 24, defina o valor do *batchSize* como 24.
    
    * Para *numInsertionWorkers*, utilize esta equação:   *numInsertionWorkers = (débito aprovisionado * latência em segundos) / (tamanho do lote * RUs consumidas para uma única escrita)*.
        
    |Propriedade|Valor|
    |--------|-----|
    |batchSize| 24 |
    |RUs aprovisionadas | 10000 |
    |Latência | 0,100 s |
    |RU cobrada para 1 escrita no documento | 10 RUs |
    |numInsertionWorkers | ? |
    
    *numInsertionWorkers = (10 000 RUs x 0,1 s) / (24 x 10 RUs) = 4,1666*

1. Execute o comando de migração. As opções para migrar os dados são descritas nas secções seguintes.

   ```bash
   mongoimport.exe --host cosmosdb-mongodb-account.documents.azure.com:10255 -u cosmosdb-mongodb-account -p <Your_MongoDB_password> --ssl --sslAllowInvalidCertificates --jsonArray --db dabasename --collection collectionName --file "C:\sample.json" --numInsertionWorkers 4 --batchSize 24
   ```
   Ou com mongorestore (confirme que o débito de todas as coleções está definido para um número igual ou superior à das RUs utilizadas em cálculos anteriores):
   
   ```bash
   mongorestore.exe --host cosmosdb-mongodb-account.documents.azure.com:10255 -u cosmosdb-mongodb-account -p <Your_MongoDB_password> --ssl --sslAllowInvalidCertificates ./dumps/dump-2016-12-07 --numInsertionWorkersPerCollection 4 --batchSize 24
   ```

## <a name="prerequisites-for-migration"></a>Pré-requisitos de migração

* **Aumentar débito:** a duração da migração de dados depende da quantidade de débito que configurou para uma coleção individual ou um conjunto de coleções. Aumente o débito para migrações de dados maiores. Após concluir a migração, reduza o débito para reduzir os custos. Para obter mais informações sobre como aumentar o débito no [portal do Azure](https://portal.azure.com), veja [Performance levels and pricing tiers in Azure Cosmos DB](performance-levels.md) (Níveis de desempenho e escalões de preço do Azure Cosmos DB).

* **Ativar o SSL:** o Azure Cosmos DB tem requisitos e normas de segurança estritos. Lembre-se de ativar o SSL quando interagir com a sua conta. Os procedimentos no resto do artigo incluem como ativar o SSL para mongoimport e mongorestore.

* **Criar recursos do Azure Cosmos DB:** antes de começar a migração de dados, crie previamente todas as suas coleções no portal do Azure. Se estiver a migrar para uma conta do Azure Cosmos DB com débito ao nível da base de dados, confirme que proporciona uma chave de partição quando criar as coleções do Azure Cosmos DB.

## <a name="get-your-connection-string"></a>Obtenha a cadeia de ligação 

1. No [portal do Azure](https://portal.azure.com), no painel do lado esquerdo, clique na entrada **Azure Cosmos DB**.
1. No painel **Subscrições**, selecione o nome da sua conta.
1. No painel **Cadeia de Ligação**, clique em **Cadeia de Ligação**.

   O painel do lado direito contém todas as informações de que precisa para se ligar com êxito à sua conta.

   ![Painel Cadeia de Ligação](./media/mongodb-migrate/ConnectionStringBlade.png)

## <a name="migrate-data-by-using-mongoimport"></a>Migrar dados utilizando o mongoimport

Para importar dados para a sua conta do Azure Cosmos DB, utilize o modelo seguinte. Preencha *host* (anfitrião), *username* (nome de utilizador) e *password* (palavra-passe) com os valores específicos da sua conta.  

Modelo:

```bash
mongoimport.exe --host <your_hostname>:10255 -u <your_username> -p <your_password> --db <your_database> --collection <your_collection> --ssl --sslAllowInvalidCertificates --type json --file "C:\sample.json"
```

Exemplo:  

```bash
mongoimport.exe --host cosmosdb-mongodb-account.documents.azure.com:10255 -u cosmosdb-mongodb-account -p <Your_MongoDB_password> --ssl --sslAllowInvalidCertificates --db sampleDB --collection sampleColl --type json --file "C:\Users\admin\Desktop\*.json"
```

## <a name="migrate-data-by-using-mongorestore"></a>Migrar dados utilizando o mongorestore

Para restaurar dados para a sua API para a conta do MongoDB, utilize o modelo seguinte para executar a importação. Preencha *host* (anfitrião), *username* (nome de utilizador) e *password* (palavra-passe) com os valores específicos da sua conta.

Modelo:

```bash
mongorestore.exe --host <your_hostname>:10255 -u <your_username> -p <your_password> --db <your_database> --collection <your_collection> --ssl --sslAllowInvalidCertificates <path_to_backup>
```

Exemplo:

```bash
mongorestore.exe --host cosmosdb-mongodb-account.documents.azure.com:10255 -u cosmosdb-mongodb-account -p <Your_MongoDB_password> --ssl --sslAllowInvalidCertificates ./dumps/dump-2016-12-07
```

## <a name="next-steps"></a>Passos Seguintes

Pode avançar para o próximo tutorial para ficar a saber como utilizar o Azure Cosmos DB para consultar dados do MongoDB. 

> [!div class="nextstepaction"]
>[Como consultar dados do MongoDB?](../cosmos-db/tutorial-query-mongodb.md)
