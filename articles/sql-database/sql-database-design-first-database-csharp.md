---
title: Criar a sua primeira base de dados SQL do Azure - C# | Microsoft Docs
description: Saiba como criar a sua primeira base de dados SQL do Azure e ligar a esta com um programa C# a utilizar ADO.NET.
services: sql-database
ms.service: sql-database
ms.subservice: development
ms.custom: ''
ms.devlang: ''
ms.topic: tutorial
author: MightyPen
ms.author: genemi
ms.reviewer: carlrab
manager: craigg-msft
ms.date: 11/01/2018
ms.openlocfilehash: 82cf0303019d2cbb620c442fd6f750f733930f84
ms.sourcegitcommit: 799a4da85cf0fec54403688e88a934e6ad149001
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/02/2018
ms.locfileid: "50912344"
---
# <a name="tutorial-design-an-azure-sql-database-and-connect-with-cx23-and-adonet"></a>Tutorial: Criar uma base de dados SQL do Azure e ligar com C&#x23; e ADO.NET

A Base de Dados SQL do Azure é uma base de dados relacional como serviço (DBaaS) no Microsoft Cloud (Azure). Neste tutorial, vai aprender a utilizar o portal do Azure e o ADO.NET com o Visual Studio para:

> [!div class="checklist"]
> * Criar uma base de dados no portal do Azure
> * Configurar uma regra de firewall ao nível do servidor no portal do Azure
> * Ligar à base de dados com o ADO.NET e o Visual Studio
> * Criar tabelas com o ADO.NET
> * Inserir, atualizar e eliminar os dados com o ADO.NET
> * Consultar dados com o ADO.NET

Se não tiver uma subscrição do Azure, [crie uma conta gratuita](https://azure.microsoft.com/free/) antes de começar.

## <a name="prerequisites"></a>Pré-requisitos

Uma instalação do [Visual Studio Community 2017, Visual Studio Professional 2017 ou Visual Studio Enterprise 2017](https://www.visualstudio.com/downloads/).

<!-- The following included .md, sql-database-tutorial-portal-create-firewall-connection-1.md, is long.
And it starts with a ## H2.
-->

[!INCLUDE [sql-database-tutorial-portal-create-firewall-connection-1](../../includes/sql-database-tutorial-portal-create-firewall-connection-1.md)]

<!-- The following included .md, sql-database-csharp-adonet-create-query-2.md, is long.
And it starts with a ## H2.
-->

[!INCLUDE [sql-database-csharp-adonet-create-query-2](../../includes/sql-database-csharp-adonet-create-query-2.md)]

## <a name="next-steps"></a>Passos seguintes

Neste tutorial, aprendeu tarefas de base de dados básicas, como criar uma base de dados e tabelas, carregar e consultar dados e restaurar a base de dados para um ponto anterior no tempo. Aprendeu a:
> [!div class="checklist"]
> * Criar uma base de dados
> * Configurar uma regra de firewall
> * Ligar à base de dados com o [Visual Studio e o C#](sql-database-connect-query-dotnet-visual-studio.md)
> * Criar tabelas
> * Inserir, atualizar e eliminar dados
> * Consultar dados

Avance para o próximo tutorial para saber como migrar os seus dados.

> [!div class="nextstepaction"]
> [Migre a sua base de dados do SQL Server para a Base de Dados SQL do Azure](sql-database-migrate-your-sql-server-database.md)
