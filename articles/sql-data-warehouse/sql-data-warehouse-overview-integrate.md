---
title: Crie soluções integradas com o SQL Data Warehouse | Documentos da Microsoft
description: 'Ferramentas e parceiros com soluções que se integram com o SQL Data Warehouse. '
services: sql-data-warehouse
author: kavithaj
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.component: consume
ms.date: 04/17/2018
ms.author: kavithaj
ms.reviewer: igorstan
ms.openlocfilehash: 5b302be582bd22a7b38601c90f5fe475062afb26
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/07/2018
ms.locfileid: "51239676"
---
# <a name="integrate-other-services-with-sql-data-warehouse"></a>Integrar outros serviços com o SQL Data Warehouse
Para além da funcionalidade central, o SQL Data Warehouse permite aos utilizadores integrar com muitos dos outros serviços no Azure. Alguns destes serviços incluem:

* Power BI
* Azure Data Factory
* Azure Machine Learning
* Azure Stream Analytics

O SQL Data Warehouse continua a integrar com mais serviços no Azure e muito mais [parceiros de integração](sql-data-warehouse-partner-data-integration.md).

## <a name="power-bi"></a>Power BI
Integração do Power BI permite-lhe combinar o poder de computação do SQL Data Warehouse com a geração de relatórios dinâmicos e visualização do Power BI. Integração do Power BI atualmente inclui:

* **Direct Connect**: um mais avançadas de ligação com a propagação de lógica no armazém de dados SQL. Propagação fornece uma análise mais rápido numa escala maior.
* **Abrir no Power BI**: O botão "Abrir no Power BI" passa a informação de instância para o Power BI de uma forma de simplifed para se ligar.

Para obter mais informações, consulte [integrar com o Power BI](sql-data-warehouse-get-started-visualize-with-power-bi.md), ou o [documentação do Power BI](https://blogs.msdn.com/b/powerbi/archive/2015/06/24/exploring-azure-sql-data-warehouse-with-power-bi.aspx).

## <a name="azure-data-factory"></a>Azure Data Factory
O Azure Data Factory proporciona aos usuários uma plataforma gerida para extrair complexa de criar e carregar pipelines. Inclui a integração do SQL Data Warehouse com o Azure Data Factory:

* **Procedimentos armazenados**: orquestrar a execução de procedimentos armazenados no SQL Data Warehouse.
* **Cópia**: Utilize o ADF para mover dados para o SQL Data Warehouse. Esta operação pode usar o mecanismo de movimento de dados padrão do ADF ou PolyBase nos bastidores. 

Para obter mais informações, consulte [integrar com o Azure Data Factory](sql-data-warehouse-get-started-visualize-with-power-bi.md).

## <a name="azure-machine-learning"></a>Azure Machine Learning
O Azure Machine Learning é um serviço de análise totalmente gerido, que permite-lhe criar modelos de intrincada através de um grande conjunto de ferramentas de previsão. O SQL Data Warehouse é suportado como uma origem e de destino para esses modelos com a seguinte funcionalidade:

* **Dados de leitura:** unidade modelos à escala com o T-SQL no armazém de dados SQL.
* **Dados de escrita:** consolidação é alterado de qualquer modelo de volta para o SQL Data Warehouse.

Para obter mais informações, consulte [integrar com o Azure Machine Learning](sql-data-warehouse-get-started-analyze-with-azure-machine-learning.md).

## <a name="azure-stream-analytics"></a>Azure Stream Analytics
O Azure Stream Analytics é uma infraestrutura complexa, totalmente gerida para processamento e consumir dados de eventos gerados a partir do Hub de eventos do Azure.  Integração com o SQL Data Warehouse permite a transmissão em fluxo de dados para processar e armazenar juntamente com dados relacionais, permitindo a análise mais aprofundada, mais avançado com eficiência.  

* **Resultado da tarefa:** enviar a saída de tarefas do Stream Analytics diretamente ao SQL Data Warehouse.

Para obter mais informações, consulte [integrar com o Azure Stream Analytics](sql-data-warehouse-integrate-azure-stream-analytics.md).

## <a name="next-steps"></a>Passos Seguintes
Para integrar com a base de dados do Azure SQL, consulte [consulta elástica de configurar a base de dados do SQL](tutorial-elastic-query-with-sql-datababase-and-sql-data-warehouse.md)

