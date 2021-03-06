---
title: 'Lição 8 do tutorial do Azure Analysis Services: Criar perspetivas | Microsoft Docs'
description: Descreve como criar perspetivas no projeto de tutorial do Azure Analysis Services.
author: minewiskan
manager: kfile
ms.service: azure-analysis-services
ms.topic: conceptual
ms.date: 10/18/2018
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: e92e86f59058f1f226c67f873530cd9c06b5d5fc
ms.sourcegitcommit: 707bb4016e365723bc4ce59f32f3713edd387b39
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/19/2018
ms.locfileid: "49428339"
---
# <a name="create-perspectives"></a>Criar perspetivas

Nesta lição, irá criar uma perspetiva de vendas na Internet. Uma perspetiva define um subconjunto exibível de um modelo que fornece pontos de vista concentrados, específicos da empresa ou específicos de aplicações. Quando um utilizador se liga a um modelo através de uma perspetiva, vê apenas os objetos de modelo (tabelas, colunas, medidas, hierarquias e KPI) como campos definidos nessa perspetiva. Para obter mais informações, consulte [Perspetivas](https://docs.microsoft.com/sql/analysis-services/tabular-models/perspectives-ssas-tabular).
  
A perspetiva de vendas na Internet que criou nesta lição exclui o objeto de tabela DimCustomer. Quando cria uma perspetiva que exclui determinados objetos de exibição, esse objeto ainda existe no modelo. No entanto, não é visível na lista de campos de relatórios do cliente. Colunas calculadas e medidas incluídas numa perspetiva ou não ainda podem ser calculadas a partir de dados de objeto que foram excluídos.  
  
A finalidade desta lição é descrever como criar perspetivas e se familiarizar-se com as ferramentas de criação de modelos tabulares. Se expandir este modelo para incluir posteriormente tabelas adicionais, poderá criar perspetivas adicionais para definir diferentes pontos de vista do modelo, por exemplo, inventário e vendas.  
  
Tempo estimado para concluir esta lição: **5 minutos**  
  
## <a name="prerequisites"></a>Pré-requisitos  
Este tópico faz parte de um tutorial de modelação em tabela que deve ser concluído por ordem. Antes de realizar as tarefas desta lição, deve ter concluído a lição anterior: [Lição 7: Criar os principais indicadores-chave](../tutorials/aas-lesson-7-create-key-performance-indicators.md).  
  
## <a name="create-perspectives"></a>Criar perspetivas  
  
#### <a name="to-create-an-internet-sales-perspective"></a>Para criar uma perspetiva de vendas na Internet  
  
1.  Clique no menu **Modelo** > **Perspetivas** > **Criar e gerir**.  
  
2.  Na caixa de diálogo **Perspetivas** , clique em **Nova perspetiva**.  
  
3.  Faça duplo clique no cabeçalho da coluna **Nova perspetiva** e mude o nome para **Vendas na Internet**.  
  
4.  Selecione todas as tabelas, *exceto* **DimCustomer**.  
  
    ![aas-lesson8-perspectives](../tutorials/media/aas-lesson8-perspectives.png)
  
    Numa lição posterior, irá utilizar a funcionalidade Analyze em Excel para testar esta perspetiva. A lista de campos de tabela dinâmica do Excel inclui cada tabela, exceto a tabela DimCustomer.  

## <a name="whats-next"></a>Passos seguintes?
[Lição 9: Criar hierarquias](../tutorials/aas-lesson-9-create-hierarchies.md).
  
  
  
  
