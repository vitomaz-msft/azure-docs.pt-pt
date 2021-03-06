---
title: Descrição Geral da Programação de Aplicações de Bases de Dados SQL | Microsoft Docs
description: Aprenda sobre as bibliotecas de conetividade disponíveis e as melhores práticas para as aplicações ligarem à Base de Dados SQL.
services: sql-database
ms.service: sql-database
ms.subservice: development
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: stevestein
ms.author: sstein
ms.reviewer: genemi
manager: craigg
ms.date: 06/20/2018
ms.openlocfilehash: 707e10f77bf00ed12f09a23e490105f52ceed4ab
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/07/2018
ms.locfileid: "51241604"
---
# <a name="sql-database-application-development-overview"></a>Descrição geral da programação de aplicativo da linha de base de dados SQL
Este artigo explica as considerações básicas que um programador deve conhecer ao escrever códigos para ligar à base de dados SQL do Azure.

> [!TIP]
> Para obter um tutorial que mostra como criar um servidor, criar uma firewall baseada no servidor, ver propriedades do servidor, ligar com o SQL Server Management Studio, consultar a base de dados mestra, criar uma base de dados de exemplo e uma base de dados em branco, consultar as propriedades da base de dados, ligar com o SQL Server Management Studio e consultar a base de dados de exemplo, veja [Tutorial de Introdução](sql-database-get-started-portal.md).
>

## <a name="language-and-platform"></a>Linguagem e plataforma
Estão disponíveis exemplos de código para várias linguagens de programação e plataformas. Pode encontrar ligações para exemplos de código em: 

* Obter mais informações: [bibliotecas de ligação para base de dados SQL e SQL Server](sql-database-libraries.md).

## <a name="tools"></a>Ferramentas 
Pode tirar partido de ferramentas de código-fonte aberto, como [cheetah](https://github.com/wunderlist/cheetah), [sql-cli](https://www.npmjs.com/package/sql-cli), [VS Code](https://code.visualstudio.com/). Além disso, a Base de Dados SQL do Azure funciona com ferramentas da Microsoft como o [Visual Studio](https://www.visualstudio.com/downloads/) e o [SQL Server Management Studio](https://msdn.microsoft.com/library/ms174173.aspx).  Também pode utilizar o Portal de Gestão do Azure, o PowerShell e as API REST que o ajudam a obter produtividade adicional.

## <a name="resource-limitations"></a>Limitações de recursos
Base de dados SQL do Azure gere os recursos disponíveis para uma base de dados usando dois mecanismos diferentes: governação de recursos e imposição dos limites. Para obter mais informações, consulte:

- [Limites de modelo de recursos baseados em DTU - base de dados](sql-database-dtu-resource-limits-single-databases.md)
- [Limites de modelo de recursos baseados em DTU - conjuntos elásticos](sql-database-dtu-resource-limits-elastic-pools.md)
- [limites de recursos baseados em vCore - bases de dados individuais](sql-database-vcore-resource-limits-single-databases.md)
- [limites de recursos baseados em vCore - conjuntos elásticos](sql-database-vcore-resource-limits-elastic-pools.md)

## <a name="security"></a>Segurança
A Base de Dados SQL do Azure fornece recursos para limitar o acesso, proteger os dados e monitorizar as atividades numa Base de Dados SQL.

* Obter mais informações: [proteger a sua base de dados do SQL](sql-database-security-overview.md).

## <a name="authentication"></a>Autenticação
* A Base de Dados SQL do Azure suporta utilizadores e inícios de sessão de autenticação do SQL Server, bem como utilizadores e inícios de sessão de [autenticação do Azure Active Directory](sql-database-aad-authentication.md).
* Tem de especificar uma base de dados específica, em vez de utilizar a predefinição para a base de dados *mestra*.
* Não pode utilizar o Transact-SQL **UTILIZAR myDatabaseName;** instrução na base de dados SQL para mudar para outra base de dados.
* Obter mais informações: [segurança de base de dados SQL: Gerir a segurança de acesso e o início de sessão da base de dados](sql-database-manage-logins.md).

## <a name="resiliency"></a>Resiliência
Quando ocorre um erro transitório ao ligar à Base de Dados SQL, o seu código deverá repetir a chamada.  Recomendamos que tente novamente a lógica de repetição utilize a lógica de backoff, para que não sobrecarregue a Base de Dados SQL com vários clientes em simultâneo a tentar novamente.

* Exemplos de código: para exemplos de código que ilustram a lógica de repetição, consulte exemplos para a linguagem da sua preferência em: [bibliotecas de ligação para base de dados SQL e SQL Server](sql-database-libraries.md).
* Obter mais informações: [mensagens de erro para os programas de cliente de base de dados SQL](sql-database-develop-error-messages.md).

## <a name="managing-connections"></a>Gerir ligações
* Na sua lógica de ligação de cliente, substitua o tempo limite predefinido para 30 segundos.  A predefinição de 15 segundos é demasiado curta para ligações que dependem da Internet.
* Se estiver a utilizar um [conjunto de ligações](https://msdn.microsoft.com/library/8xx3tyca.aspx), certifique-se de que fecha a ligação assim que o seu programa não estiver a utilizá-la ativamente e não estiver a preparar-se para reutilizá-la.

## <a name="network-considerations"></a>Considerações de rede
* No computador que aloja o seu programa cliente, certifique-se de que a firewall permite a comunicação TCP de saída na porta 1433.  Obter mais informações: [configurar uma firewall de base de dados do Azure SQL](sql-database-configure-firewall-settings.md).
* Se o seu programa cliente se liga à base de dados do SQL, enquanto o cliente executa numa máquina virtual do Azure (VM), tem de abrir determinados intervalos de portas na VM. Obter mais informações: [portas para além do 1433 para ADO.NET 4.5 e base de dados SQL](sql-database-develop-direct-route-ports-adonet-v12.md).
* Ligações de cliente para a base de dados do Azure SQL, às vezes, ignoram o proxy e interagem diretamente com a base de dados. As portas que não sejam 1433 tornam-se importantes. Para obter mais informações, [arquitetura de conectividade do Azure SQL Database](sql-database-connectivity-architecture.md) e [portas para além do 1433 para ADO.NET 4.5 e base de dados SQL](sql-database-develop-direct-route-ports-adonet-v12.md).

## <a name="data-sharding-with-elastic-scale"></a>Fragmentação de dados com escala elástica
Dimensionamento elástico simplifica o processo de aumentar horizontalmente (e). 

* [Padrões de conceção para aplicações de SaaS multi-inquilino com a base de dados SQL do Azure](sql-database-design-patterns-multi-tenancy-saas-applications.md).
* [Encaminhamento dependente de dados](sql-database-elastic-scale-data-dependent-routing.md).
* [Introdução à pré-visualização de escala elástica de base de dados SQL do Azure](sql-database-elastic-scale-get-started.md).

## <a name="next-steps"></a>Passos Seguintes
Explore todas as [capacidades da Base de Dados SQL](sql-database-technical-overview.md).
