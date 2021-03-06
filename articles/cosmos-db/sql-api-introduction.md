---
title: 'Azure Cosmos DB: Introdução à API de SQL | Microsoft Docs'
description: Saiba como pode utilizar o Azure Cosmos DB para armazenar e consultar grandes volumes de dados de documentos JSON com baixa latência através da utilização do SQL e do JavaScript.
keywords: json database, document database
services: cosmos-db
author: rafats
manager: kfile
ms.service: cosmos-db
ms.component: cosmosdb-sql
ms.devlang: na
ms.topic: overview
ms.date: 05/22/2017
ms.author: rafats
ms.openlocfilehash: 5d1e86630ff9143a75e5b0502a64c7661cc2822c
ms.sourcegitcommit: ebf2f2fab4441c3065559201faf8b0a81d575743
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/20/2018
ms.locfileid: "52161089"
---
# <a name="introduction-to-azure-cosmos-db-sql-api"></a>Introdução à API do Azure Cosmos DB: API de SQL

O [Azure Cosmos DB](introduction.md) é um serviço de bases de dados com vários modelos e distribuído globalmente da Microsoft para aplicações críticas para atividades. O Azure Cosmos DB proporciona [distribuição global chave na mão](distribute-data-globally.md), [dimensionamento elástico de débito e armazenamento](partition-data.md) em todo o mundo, latências de milissegundos de um só dígito no percentil 99, [cinco níveis de consistência bem definidos](consistency-levels.md) e elevada disponibilidade garantida, tudo com o suporte de [SLAs líderes da indústria](https://azure.microsoft.com/support/legal/sla/cosmos-db/). O Azure Cosmos DB [indexa automaticamente os dados](http://www.vldb.org/pvldb/vol8/p1668-shukla.pdf) sem que tenha de lidar com a gestão de esquemas e índices. É multimodal e suporte modelos de dados em documentos, chaves-valores, gráficos e em colunas.

![API do Azure SQL](./media/sql-api-introduction/cosmosdb-sql-api.png) 

Com a API do SQL, o Azure Cosmos DB oferece [capacidades de consulta SQL](how-to-sql-query.md) avançadas e familiares com baixas latências consistentes em dados JSON sem esquemas. Neste artigo, apresentamos uma descrição geral da API do SQL do Azure Cosmos DB e informações sobre como pode utilizá-la para armazenar grandes volumes de dados JSON, consultá-los dentro da ordem de latência de milissegundos e evoluir o esquema facilmente. 

## <a name="what-capabilities-and-key-features-does-azure-cosmos-db-offer"></a>Que capacidades e funcionalidades chave oferece o Azure Cosmos DB?
O Azure Cosmos DB, através da API do SQL, oferece as principais capacidades e vantagens seguintes:

* **Débito e armazenamento dimensionável:** Aumente ou reduza verticalmente de forma fácil a sua base de dados JSON para satisfazer as necessidades da sua aplicação. Os seus dados são armazenados em unidades de estado sólido (SSD) para baixas latências previsíveis. O Azure Cosmos DB suporta contentores para armazenar dados JSON designados coleções que são dimensionáveis para praticamente qualquer tamanho de armazenamento e débito. Pode dimensionar de forma totalmente integrada o Azure Cosmos DB, com um desempenho previsível, à medida que a sua aplicação aumenta. 


* **Replicação de multirregião:** o Azure Cosmos DB replica de forma transparente os dados para todas as regiões que associou à conta do Azure Cosmos DB, permitindo-lhe desenvolver aplicações que necessitam de acesso global aos dados ao apresentar as responsabilidades entre consistência, disponibilidade e desempenho, tudo com garantias correspondentes. O Azure Cosmos DB oferece ativação pós-falha regional transparente com APIs multi-homing e a capacidade e dimensionar de forma elástica o débito e o armazenamento a nível global. Saiba mais em [Distribute data globally with Azure Cosmos DB](distribute-data-globally.md) (Distribuir dados globalmente com o Azure Cosmos DB).

* **Consultas ad hoc com a sintaxe familiar do SQL:** Armazenar documentos JSON heterogéneos e consultar estes documentos através de uma sintaxe familiar de SQL. O Azure Cosmos DB utiliza uma tecnologia de indexação estruturada em registos, sem bloqueio e de elevada simultaneidade, para indexar automaticamente todos os conteúdos do documento. Isto permite consultas em tempo real sem ter de especificar sugestões de esquema, índices secundários ou vistas. Saiba mais em [Query Azure Cosmos DB](how-to-sql-query.md) (Consultar o Azure Cosmos DB). 
* **Execução de JavaScript na base de dados:** Expressar a lógica de aplicação como procedimentos armazenados, acionadores e funções definidas pelo utilizador (UDFs) com o JavaScript padrão. Isto permite que a lógica da sua aplicação funcione através de dados sem ter de se preocupar sobre o erro de correspondência entre a aplicação e o esquema da base de dados. A API do SQL oferece a execução transacional completa da lógica de aplicação do JavaScript diretamente dentro do motor da base de dados. A integração profunda do JavaScript permite a execução das operações INSERIR, SUBSTITUIR, ELIMINAR e SELECIONAR de um programa de JavaScript como uma transação isolada. Saiba mais em [SQL server-side programming (Programação do lado do servidor do SQL)](programming.md).

* **Níveis de consistência sincronizáveis:** Selecione um dos cinco níveis de consistência bem definidos para alcançar compromissos ótimos entre a consistência e o desempenho. Para consultas e operações de leitura, o Azure Cosmos DB oferece cinco níveis de consistência distintos: forte, consistência vinculada, sessão, prefixo de consistência e eventual. Estes níveis de consistência granular e bem definidos permitem-lhe efetuar compromissos sonoros entre a consistência, a disponibilidade e a latência. Saiba mais em [Using consistency levels to maximize availability and performance](consistency-levels.md) (Utilizar níveis de consistência para maximizar a disponibilidade e desempenho).

* **Completamente geridos:** Eliminar a necessidade de gerir bases de dados e recursos de máquinas. O serviço Microsoft Azure sendo completamente gerido, não precisa de gerir máquinas virtuais, implementar e configurar o software, gerir o dimensionamento ou lidar com atualizações de camada de dados complexas. É feita uma cópia de segurança automática de cada base de dados, sendo protegida contra falhas regionais. Pode facilmente adicionar uma conta do Azure Cosmos DB e aprovisionar a capacidade conforme necessário, permitindo-lhe focar-se na sua aplicação em vez de explorar e gerir a sua base de dados. 

* **Abrir por conceção:** Comece a trabalhar rapidamente utilizando as competências e ferramentas existentes. A programação da API do SQL é simples, acessível e não requer a utilização de novas ferramentas ou a adesão a extensões personalizadas para o JSON ou o JavaScript. Pode aceder a todas as funcionalidades da base de dados, incluindo CRUD, consulta e processamento de JavaScript através de uma interface RESTful HTTP simples. A API do SQL engloba formatos, idiomas e normas existentes, oferecendo capacidades de base de dados de elevado valor.

* **Indexação automática:** por predefinição, o Azure Cosmos DB indexa automaticamente todos os documentos na base de dados e não espera nem exige nenhum esquema ou criação de índices secundários. Não pretende indexar tudo? Não se preocupe, também pode [optar por excluir caminhos nos seus ficheiros JSON](indexing-policies.md).

* **Suporte de feed de alterações:** O feed de alterações fornece uma lista ordenada de documentos dentro de uma coleção do Azure Cosmos DB, pela ordem em que foram modificados. Este feed serve para escutar alterações de dados para replicar dados, acionar chamadas de API ou realizar o processamento de fluxo em atualizações. O feed de alterações é automaticamente ativado e é fácil de utilizar: [saiba mais sobre o feed de alterações](https://docs.microsoft.com/azure/cosmos-db/change-feed). 

## <a name="data-management"></a>Como gerir dados com a API do SQL?
A API do SQL ajuda a gerir dados JSON através de recursos de bases de dados bem definidos. Estes recursos são replicados para elevada disponibilidade, sendo exclusivamente endereçáveis pelo respetivo URI lógico. A API do SQL oferece um modelo de programação HTTP simples com base na RESTful para todos os recursos. 


A conta da base de dados do Azure Cosmos DB é um espaço de nomes único que lhe dá acesso ao Azure Cosmos DB. Antes de poder criar uma conta de base de dados, terá de ter uma subscrição do Azure, que lhe permitirá aceder a vários serviços do Azure. 

Todos os recursos do Azure Cosmos DB são modelados e armazenados como documentos JSON. Os recursos são geridos como itens, ou seja documentos JSON que contêm metadados e como feeds que são coleções de itens. Os conjuntos de itens estão contidos nos respetivos feeds.

A imagem abaixo mostra as relações entre os recursos do Azure Cosmos DB:

![A relação hierárquica entre os recursos no Azure Cosmos DB][1] 

Uma conta de base de dados consiste num conjunto de bases de dados, em que cada uma contém várias coleções que, por sua vez, podem conter procedimentos armazenados, acionadores, UDFs, documentos e anexos relacionados. Uma base de dados também tem utilizadores associados, cada um com um conjunto de permissões para aceder a várias outras coleções, procedimentos armazenados, acionadores, UDFs, documentos ou anexos. Enquanto as bases de dados, os utilizadores, as permissões e as coleções são recursos definidos pelo sistema com esquemas bem conhecidos - os documentos, procedimentos armazenados, acionadores, UDFs e anexos contêm conteúdos JSON arbitrários definidos pelo utilizador.  

## <a name="develop"></a>Como posso programar aplicações com a API do SQL?

O Azure Cosmos DB expõe recursos através de APIs REST que podem ser chamadas por qualquer linguagem com capacidade de realizar pedidos HTTP/HTTPS. Além disso, oferecemos bibliotecas de programação para várias linguagens populares para a API do SQL. Estas bibliotecas de cliente simplificam muitos aspetos do trabalho com a API ao processar detalhes, como a colocação em cache do endereço, a gestão de exceções, tentativas automáticas e assim sucessivamente. Atualmente, as bibliotecas estão disponíveis para as seguintes linguagens e plataformas:  

| Transferência | Documentação |
| --- | --- |
| [SDK do .NET](https://go.microsoft.com/fwlink/?LinkID=402989) |[Documentos de referência .NET](/dotnet/api/overview/azure/cosmosdb?view=azure-dotnet) |
| [SDK Java](https://go.microsoft.com/fwlink/?LinkID=402380) |[Documentos de referência Java](/java/api/com.microsoft.azure.documentdb) |
| [SDK JavaScript](https://www.npmjs.com/package/@azure/cosmos) |[Documentos de referência JavaScript](https://docs.microsoft.com/javascript/api/@azure/cosmos/?view=azure-node-latest) |
| [Python SDK](https://pypi.python.org/pypi/pydocumentdb) |[Documentos de referência de Python](https://github.com/Azure/azure-cosmos-python) |

Ao utilizar o [Emulador do Azure Cosmos DB](local-emulator.md), pode programar e testar a sua aplicação localmente com a API do SQL, sem criar uma subscrição do Azure ou incorrer em custos. Quando estiver satisfeito com o funcionamento da sua aplicação no emulador, pode mudar e começar a utilizar uma conta do Azure Cosmos DB na cloud.

Para além das operações básicas de criação, leitura, atualização e eliminação, a API do SQL oferece uma interface de consulta SQL avançada para obter documentos JSON e suporte do lado do servidor para a execução transacional da lógica de aplicação do JavaScript. As interfaces de execução de consulta e script estão disponíveis através de todas as bibliotecas de plataforma, bem como as APIs REST. 

### <a name="sql-query"></a>Consulta SQL
O Azure Cosmos DB suporta a consulta de documentos através de uma linguagem SQL, enraizada no sistema de tipo JavaScript e expressões com suporte para consultas relacionais, hierárquicas e espaciais. A linguagem de consulta do Azure Cosmos DB é uma interface simples, mas poderosa, para consultar documentos JSON. A linguagem suporta um subconjunto de gramática ANSI SQL e adiciona uma integração profunda do objeto JavaScript, matrizes, construção de objeto e invocação de função. A API do SQL oferece o modelo de consulta sem qualquer esquema explícito ou indexação de sugestões do programador.

As Funções Definidas pelo Utilizador (UDFs) podem ser registadas com a API do SQL e referenciadas como parte de uma consulta SQL, expandindo deste modo a gramática para suportar a lógica de aplicação personalizada. Estas UDFs são escritas como programas JavaScript e executadas na base de dados. 

Para programadores de .NET, o [SDK do .NET](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.linq.aspx) da API do SQL também oferece um fornecedor de consultas LINQ. 

### <a name="transactions-and-javascript-execution"></a>Transações e execução de JavaScript
A API do SQL permite-lhe escrever a lógica da aplicação como programas com nome escritos inteiramente no JavaScript. Estes programas estão registados para uma coleção e podem emitir operações de base de dados em documentos de uma dada coleção. O JavaScript pode ser registado para ser executado como um acionador, procedimento armazenado ou função definida pelo utilizador. Os acionadores e procedimentos armazenados podem criar, ler, atualizar e eliminar documentos enquanto as funções definidas pelo utilizador executam a lógica de execução da consulta sem acesso à escrita na coleção.

A execução do JavaScript na CosmosDB é modelada após os conceitos suportados pelos sistemas de base de dados relacional, tendo o JavaScript como um substituto moderno para Transact-SQL. Toda a lógica do JavaScript é executada numa transação do ambiente ACID, com o isolamento do instantâneo. Durante a sua execução, se o JavaScript emitir uma exceção, toda a transação será abortada.

## <a name="next-steps"></a>Passos Seguintes
Já tem uma conta do Azure? Em seguida pode começar a utilizar o Azure Cosmos DB ao seguir o nosso [início rápido](../cosmos-db/create-sql-api-dotnet.md), que o irá guiar para criar uma conta e começar a trabalhar com o Cosmos DB.

[1]: ./media/sql-api-introduction/json-database-resources1.png

