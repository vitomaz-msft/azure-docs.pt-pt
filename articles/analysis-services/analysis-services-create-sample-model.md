---
title: Tutorial - Adicionar um modelo de exemplo básico ao seu servidor do Azure Analysis Services com o portal | Microsoft Docs
description: Nesta aula de tutorial, aprenda a adicionar um modelo de exemplo no Azure Analysis Services.
author: minewiskan
manager: kfile
ms.service: azure-analysis-services
ms.topic: tutorial
ms.date: 10/18/2018
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: def7ee54390a716027fcacb433b99826d3650f6f
ms.sourcegitcommit: 707bb4016e365723bc4ce59f32f3713edd387b39
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/19/2018
ms.locfileid: "49428697"
---
# <a name="tutorial-add-a-sample-model-from-the-portal"></a>Tutorial: Adicionar um modelo de exemplo a partir do portal

Neste tutorial, irá adicionar uma base de dados de modelo tabular do Adventure Works de exemplo ao seu servidor. O modelo de exemplo é uma versão completa do modelo de dados de exemplo de Vendas do Adventure Works Internet (1200). Um modelo de exemplo é útil para testar a gestão de modelos, ao ligar a ferramentas e aplicações de cliente e consultar dados do modelo. Este tutorial utiliza o [portal do Azure](https://portal.azure.com) e o [SQL Server Management Studio](/sql/ssms/download-sql-server-management-studio-ssms) (SSMS) para: 

> [!div class="checklist"]
> * Adicionar um modelo de dados tabular de exemplo concluído para um servidor 
> * Ligar ao modelo com o SSMS

Se não tiver uma subscrição do Azure, [crie uma conta gratuita](https://azure.microsoft.com/free/) antes de começar.

## <a name="before-you-begin"></a>Antes de começar

Para concluir este tutorial, precisa de:

- Um servidor do Azure Analysis Services. Para saber mais, veja [Criar um servidor - portal](analysis-services-create-server.md).
- Permissões de administrador de servidor
- [SQL Server Management Studio](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms)


## <a name="sign-in-to-the-azure-portal"></a>Iniciar sessão no portal do Azure

Inicie sessão no [portal](https://portal.azure.com/).

## <a name="add-a-sample-model"></a>Adicionar um modelo de exemplo

1. No servidor **Descrição geral**, clique em **Novo modelo**.

    ![Criar um modelo de exemplo](./media/analysis-services-create-sample-model/aas-create-sample-new-model.png)

2. Em **Novo modelo** > **Escolha uma origem de dados**, certifique-se de que os **Dados de exemplo** estão selecionados e, em seguida, clique em **Adicionar**.

    ![Selecionar dados de exemplo](./media/analysis-services-create-sample-model/aas-create-sample-data.png)

3. Na **Descrição geral**, certifique-se de que o modelo de exemplo `adventureworks` é adicionado.

    ![Selecionar dados de exemplo](./media/analysis-services-create-sample-model/aas-create-sample-verify.png)


## <a name="clean-up-resources"></a>Limpar recursos

O modelo de exemplo está a utilizar recursos de memória de cache. Se não estiver a utilizar o seu modelo de exemplo para fins de teste, deve removê-lo do seu servidor.

Estes passos descrevem como eliminar um modelo de um servidor com o SSMS. Também pode eliminar um modelo com a funcionalidade Estruturador Web de pré-visualização.

1. No SSMS > **Object Explorer**, clique em **Ligar** > **Analysis Services**.

2. Em **Ligar ao Servidor**, cole o nome do servidor e, em seguida, na **Autenticação**, escolha **Active Directory - Universal com suporte MFA**, introduza o seu nome de utilizador e clique em **Ligar**.

    ![Iniciar sessão](./media/analysis-services-create-sample-model/aas-create-sample-cleanup-signin.png)

3. No **Object Explorer**, clique com o botão direito do rato na base de dados de amostra `adventureworks` e, em seguida, clique em **Eliminar**.

    ![Eliminar base de dados de amostra](./media/analysis-services-create-sample-model/aas-create-sample-cleanup-delete.png)

## <a name="next-steps"></a>Passos seguintes 

Neste tutorial, aprendeu a adicionar um modelo de exemplo básico ao seu servidor. Agora que tem uma base de dados modelo, pode ligar-se à mesma a partir do SQL Server Management Studio e adicionar funções de utilizador. Para obter mais informações, avance para o próximo tutorial.

> [!div class="nextstepaction"]
> [Tutorial: Configurar funções de utilizador e de administrador de servidor](analysis-services-database-users.md)


