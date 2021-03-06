---
title: 'Portal do Azure: georreplicação de base de dados SQL | Documentos da Microsoft'
description: Configurar a georreplicação para base de dados SQL do Azure no portal do Azure e iniciar ativação pós-falha
services: sql-database
ms.service: sql-database
ms.subservice: operations
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: anosov1960
ms.author: sashan
ms.reviewer: carlrab
manager: craigg
ms.date: 09/14/2018
ms.openlocfilehash: 592e4c2dc375da34b3a6039bef7ea4da0fa3315b
ms.sourcegitcommit: 51a1476c85ca518a6d8b4cc35aed7a76b33e130f
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/25/2018
ms.locfileid: "47163975"
---
# <a name="configure-active-geo-replication-for-azure-sql-database-in-the-azure-portal-and-initiate-failover"></a>Configurar georreplicação ativa para o Azure SQL Database no portal do Azure e iniciar ativação pós-falha

Este artigo mostra-lhe como configurar a georreplicação ativa para base de dados SQL no [portal do Azure](http://portal.azure.com) e iniciar a ativação pós-falha.

Para iniciar a ativação pós-falha com o portal do Azure, consulte [iniciar uma ativação pós-falha planeada ou não planeada para a base de dados do Azure SQL com o portal do Azure](sql-database-geo-replication-portal.md).

Para configurar a georreplicação ativa com o portal do Azure, terá o seguinte recurso:

* Uma base de dados SQL do Azure: A base de dados principal que pretende replicar para uma região geográfica diferente.

> [!Note]
Georreplicação ativa tem de ser entre bases de dados na mesma subscrição.

## <a name="add-a-secondary-database"></a>Adicionar uma base de dados secundária
Os passos seguintes criam uma nova base de dados secundário numa parceria de replicação geográfica.  

Para adicionar uma base de dados secundária, tem de ser o proprietário da subscrição ou coproprietário.

A base de dados secundária tem o mesmo nome que a base de dados primário e tem, por predefinição, o mesmo serviço de camada e tamanho de computação. A base de dados secundária pode ser uma base de dados ou uma base de dados num conjunto elástico. Para obter mais informações, consulte [modelo de compra baseado em DTU](sql-database-service-tiers-dtu.md) e [modelo de compra baseado em vCore](sql-database-service-tiers-vcore.md).
Depois do secundário é criado e implantado, dados começa a replicar a partir da base de dados primária para a nova base de dados secundário.

> [!NOTE]
> Se já existir na base de dados do parceiro (por exemplo, como resultado da terminação uma relação de replicação geográfica anterior) o comando falha.
> 

1. Na [portal do Azure](http://portal.azure.com), navegue para a base de dados que pretende configurar a georreplicação.
2. Na página de banco de dados SQL, selecione **georreplicação**e, em seguida, selecione a região para criar a base de dados secundário. Pode selecionar qualquer região que a região que aloja a base de dados primária, mas recomendamos que o [região emparelhada](../best-practices-availability-paired-regions.md).
   
    ![Configurar georreplicação](./media/sql-database-geo-replication-portal/configure-geo-replication.png)
3. Selecione ou configurar o servidor e o escalão de preço para a base de dados secundário.
   
    ![Configurar secundário](./media/sql-database-geo-replication-portal/create-secondary.png)
4. Opcionalmente, pode adicionar uma base de dados secundária a um conjunto elástico. Para criar a base de dados secundário num conjunto, clique em **conjunto elástico** e selecione um agrupamento no servidor de destino. Um conjunto tem de existir no servidor de destino. Este fluxo de trabalho não cria um conjunto.
5. Clique em **criar** adicionar secundário.
6. A base de dados secundária é criada e iniciar o processo de propagação.
   
    ![Configurar secundário](./media/sql-database-geo-replication-portal/seeding0.png)
7. Quando o processo de propagação estiver concluído, a base de dados secundária apresenta o estado dele.
   
    ![Seeding concluída](./media/sql-database-geo-replication-portal/seeding-complete.png)

## <a name="initiate-a-failover"></a>Inicie uma ativação pós-falha

A base de dados secundária pode ser mudado para primária.  

1. Na [portal do Azure](http://portal.azure.com), navegue para a base de dados primária na parceria de replicação geográfica.
2. No painel da base de dados SQL, selecione **todas as definições** > **georreplicação**.
3. Na **bases de dados secundárias** , selecione a base de dados para a primária e clique em **ativação pós-falha**.
   
    ![Ativação pós-falha](./media/sql-database-geo-replication-failover-portal/secondaries.png)
4. Clique em **Sim** para iniciar a ativação pós-falha.

O comando muda imediatamente a base de dados secundária para a função primária. 

Existe um curto período de tempo durante o qual as bases de dados estão disponíveis (na ordem de 0 a 25 segundos), enquanto as funções são mudadas. Se a base de dados principal tiver várias bases de dados secundários, o comando reconfigura automaticamente as outras bases de dados secundárias para ligar para a nova principal. Toda a operação deve demorar menos de um minuto para concluir em circunstâncias normais. 

> [!NOTE]
> Este comando destina-se a recuperação rápida da base de dados em caso de indisponibilidade. Aciona ativação pós-falha sem sincronização de dados (ativação pós-falha forçada).  Se o principal está online e ao consolidar transações quando o comando é emitido alguma perda de dados pode ocorrer. 
> 
> 

## <a name="remove-secondary-database"></a>Remover a base de dados secundária
Esta operação permanentemente termina a replicação para a base de dados secundária e altera a função da secundária para uma base de dados de leitura / escrita regular. Se a conectividade à base de dados secundário for interrompida, o comando for concluída com êxito, mas o faz secundário não tornam-se leitura / escrita até depois a conectividade é restaurada.  

1. Na [portal do Azure](http://portal.azure.com), navegue para a base de dados primária na parceria de replicação geográfica.
2. Na página de banco de dados SQL, selecione **georreplicação**.
3. Na **bases de dados secundárias** , selecione a base de dados que pretende remover a parceria de replicação geográfica.
4. Clique em **parar a replicação**.
   
    ![Remover secundário](./media/sql-database-geo-replication-portal/remove-secondary.png)
5. É aberta uma janela de confirmação. Clique em **Sim** para remover a base de dados a parceria de replicação geográfica. (Defina-o como uma base de dados de leitura / escrita não faz parte de qualquer replicação.)

## <a name="next-steps"></a>Passos Seguintes
* Para saber mais sobre a georreplicação ativa, veja [georreplicação ativa](sql-database-geo-replication-overview.md).
* Para uma visão geral de continuidade de negócio e cenários, consulte [descrição geral da continuidade de negócio](sql-database-business-continuity.md).

