---
title: Utilizar dados do Azure Blockchain Workbench no Microsoft Power BI
description: Saiba como carregar e ver os dados da BD SQL do Azure Blockchain Workbench no Microsoft Power BI.
services: azure-blockchain
keywords: ''
author: PatAltimore
ms.author: patricka
ms.date: 10/1/2018
ms.topic: article
ms.service: azure-blockchain
ms.reviewer: mmercuri
manager: femila
ms.openlocfilehash: b1020389ef28c18c03536d686cd47ef0c65b9204
ms.sourcegitcommit: 1981c65544e642958917a5ffa2b09d6b7345475d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/03/2018
ms.locfileid: "48242808"
---
# <a name="using-azure-blockchain-workbench-data-with-microsoft-power-bi"></a>Utilizar dados do Azure Blockchain Workbench com o Microsoft Power BI

O Microsoft Power BI permite gerir facilmente relatórios poderosos a partir de bases de dados de base SQL com o Power BI Desktop e, em seguida, publicá-los em [https://www.powerbi.com](http://www.powerbi.com).

Este artigo contém instruções passo a passo sobre como ligar à Base de Dados SQL do Azure Blockchain Workbench a partir do Power BI Desktop, criar um relatório e implementá-lo no powerbi.com.

## <a name="prerequisites"></a>Pré-requisitos

* Transfira o [Power BI Desktop](https://aka.ms/pbidesktopstore).

## <a name="connecting-powerbi-to-data-in-azure-blockchain-workbench"></a>Ligar o PowerBI aos dados no Azure Blockchain Workbench

1.  Abra o Power BI Desktop.
2.  Selecione **Obter Dados**.

    ![Obter dados](./media/data-powerbi/get-data.png)
3.  Selecione **SQL Server** na lista de tipos de origens de dados.

4.  Introduza o nome do servidor e da base de dados na caixa de diálogo. Especifique se pretende importar os dados ou realizar uma **DirectQuery**. Selecione **OK**.

    ![Selecionar o SQL Server](./media/data-powerbi/select-sql.png)

5.  Introduza as credenciais da base de dados para aceder ao Azure Blockchain Workbench. Selecione **Base de Dados** e introduza as suas credenciais.

    Se estiver a utilizar as credenciais criadas pelo processo de implementação do Azure Blockchain Workbench, o nome de utilizador é **dbadmin** e a palavra-passe é a mesma que forneceu durante a implementação.

    ![Definições da BD SQL](./media/data-powerbi/db-settings.png)

6.  Uma vez ligado à base de dados, a caixa de diálogo **Navegador** apresenta as tabelas e as vistas disponíveis na base de dados. As vistas foram concebidas para a elaboração de relatórios e têm todas o prefixo **vw**.

    ![Navegador](./media/data-powerbi/navigator.png)

7.  Selecione as vistas que pretende incluir. Para fins de demonstração, incluímos **vwContractAction**, que fornece detalhes sobre todas as ações ocorridas num contrato.

    ![Selecionar vistas](./media/data-powerbi/select-views.png)

Pode agora criar e publicar relatórios tal como faria normalmente com o Power BI.

## <a name="next-steps"></a>Passos Seguintes

> [!div class="nextstepaction"]
> [Vistas de base de dados no Azure Blockchain Workbench](database-views.md)