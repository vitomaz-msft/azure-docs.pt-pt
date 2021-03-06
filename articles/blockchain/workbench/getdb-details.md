---
title: Obter detalhes da base de dados do Azure Blockchain Workbench
description: Saiba como obter informações do servidor e da base de dados do Azure Blockchain Workbench.
services: azure-blockchain
keywords: ''
author: PatAltimore
ms.author: patricka
ms.date: 10/1/2018
ms.topic: article
ms.service: azure-blockchain
ms.reviewer: mmercuri
manager: femila
ms.openlocfilehash: 4020500e20973e8818320424833815c1b5224f6c
ms.sourcegitcommit: 1981c65544e642958917a5ffa2b09d6b7345475d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/03/2018
ms.locfileid: "48243104"
---
# <a name="get-information-about-your-azure-blockchain-workbench-database"></a>Obter informações sobre a base de dados do Azure Blockchain Workbench

Este artigo mostra como obter informações detalhadas sobre a base de dados do Azure Blockchain Workbench.

## <a name="overview"></a>Descrição geral

São fornecidas informações sobre as aplicações, os fluxos de trabalho e a execução de contrato inteligente através das vistas de base de dados na BD SQL do Blockchain Workbench. Os programadores podem utilizar estas informações quando utilizarem ferramentas como o Microsoft Excel, PowerBI, Visual Studio e SQL Server Management Studio.

Antes de um programador conseguir ligar à base de dados, precisa de:

* Acesso de cliente externo permitido na firewall da base de dados. Este artigo explica como configurar uma firewall de base de dados e permitir o acesso.
* O nome do servidor da base de dados e o nome da base de dados.

## <a name="connect-to-the-blockchain-workbench-database"></a>Ligar à base de dados do Blockchain Workbench

Para ligar à base de dados:

1. Inicie sessão no Portal do Azure com uma conta que tenha permissões de **Proprietário** para os recursos do Azure Blockchain Workbench.
2. Na navegação à esquerda, selecione **Grupos de recursos**.
3. Escolha o nome do grupo de recursos para a sua implementação do Blockchain Workbench.
4. Selecione **Tipo** para ordenar a lista de recursos e, em seguida, selecione **SQL Server**. A lista ordenada na captura de ecrã seguinte mostra duas bases de dados SQL, a "mestra" e a que utiliza "lhgn" como **Prefixo de recurso**.

   ![Lista de recursos ordenada do Blockchain Workbench](./media/getdb-details/sorted-workbench-resource-list.png)

5. Para ver informações detalhadas sobre a base de dados do Blockchain Workbench, selecione a ligação para a base de dados com o **Prefixo de recurso** fornecido para a implementação do Blockchain Workbench.

   ![Detalhes da base de dados](./media/getdb-details/workbench-db-details.png)

O nome do servidor da base de dados e o nome da base de dados permitem-lhe ligar à base de dados do Blockchain Workbench com a sua ferramenta de desenvolvimento ou de relatórios.

## <a name="next-steps"></a>Passos Seguintes

> [!div class="nextstepaction"]
> [Vistas de base de dados no Azure Blockchain Workbench](database-views.md)