---
title: Testes de recuperação de desastres de base de dados SQL | Documentos da Microsoft
description: Aprenda a orientações e melhores práticas para utilizar a SQL Database do Azure para realizar testes de recuperação após desastre.
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
ms.date: 04/01/2018
ms.openlocfilehash: c861163670b05b01c9c6d64b81f6e83c979a2af8
ms.sourcegitcommit: 51a1476c85ca518a6d8b4cc35aed7a76b33e130f
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/25/2018
ms.locfileid: "47163040"
---
# <a name="performing-disaster-recovery-drill"></a>Efetuar teste de recuperação após desastre
Recomenda-se que é executada periodicamente uma validação de preparação de aplicativos para o fluxo de trabalho de recuperação. A verificar o comportamento do aplicativo e as implicações de perda de dados e/ou a interrupção envolve a que a ativação pós-falha é uma boa prática de engenharia. Também é um requisito pela maioria dos padrões da indústria como parte da certificação de continuidade de negócio.

Executar um teste de recuperação após desastre é composta por:

* Simulação de falha de camada de dados
* A recuperar
* Validar a recuperação de postagem de integridade do aplicativo

Dependendo de como [projetado seu aplicativo para continuidade do negócio](sql-database-business-continuity.md), o fluxo de trabalho para executar o teste pode variar. Este artigo descreve as melhores práticas para realizar um teste de recuperação após desastre no contexto da base de dados do Azure SQL.

## <a name="geo-restore"></a>Georrestauro
Para evitar a potencial perda de dados ao conduzir um teste de recuperação após desastre, efetue o teste usando um ambiente de teste ao criar uma cópia do ambiente de produção e usá-lo para verificar o fluxo de trabalho de ativação pós-falha do aplicativo.

#### <a name="outage-simulation"></a>Simulação de falha
Para simular a falha, pode mudar o nome da base de dados de origem. Isso faz com que as falhas de conectividade de aplicativo.

#### <a name="recovery"></a>Recuperação
* Efetuar o georrestauro da base de dados num servidor diferente, conforme descrito [aqui](sql-database-disaster-recovery.md).
* Alterar a configuração de aplicação para ligar à base de dados recuperada e siga os [configurar uma base de dados após a recuperação](sql-database-disaster-recovery.md) guia para concluir a recuperação.

#### <a name="validation"></a>Validação
* Conclua a desagregação, verificando a recuperação de postagem de integridade do aplicativo (incluindo as cadeias de ligação, inícios de sessão, o teste de funcionalidade básica ou outra parte de validações dos procedimentos de signoffs de aplicativo padrão).

## <a name="failover-groups"></a>Grupos de ativação pós-falha
Para uma base de dados protegida através de grupos de ativação pós-falha, o exercício de teste envolve a ativação pós-falha planeada para o servidor secundário. A ativação pós-falha planeada garante que o principal e as bases de dados secundárias no grupo de ativação pós-falha permanecem sincronizados quando as funções estão no modo. Ao contrário da ativação pós-falha não planeada, esta operação resulta numa perda de dados, para que o de exploração pode ser executado no ambiente de produção.

#### <a name="outage-simulation"></a>Simulação de falha
Para simular a falha, pode desativar a aplicação web ou máquina virtual ligada à base de dados. Isso resulta em falhas de conectividade para os clientes web.

#### <a name="recovery"></a>Recuperação
* Certifique-se a configuração da aplicação em pontos de região de DR para o anterior secundário, que se torna a nova principal totalmente acessível.
* Iniciar [a ativação pós-falha planeada](scripts/sql-database-setup-geodr-and-failover-database-powershell.md) do grupo de ativação pós-falha a partir do servidor secundário.
* Siga os [configurar uma base de dados após a recuperação](sql-database-disaster-recovery.md) guia para concluir a recuperação.

#### <a name="validation"></a>Validação
Conclua a desagregação, verificando a recuperação de postagem de integridade do aplicativo (incluindo a conectividade, testar a funcionalidade básica ou outras validações necessárias para a exploração signoffs).

## <a name="next-steps"></a>Passos Seguintes
* Para saber mais sobre os cenários de continuidade de negócio, veja [cenários de continuidade](sql-database-business-continuity.md).
* Para saber mais sobre SQL do Azure, base de dados automatizada de cópias de segurança, consulte [cópias de segurança automatizadas de base de dados SQL](sql-database-automated-backups.md)
* Para saber mais sobre a utilização de cópias de segurança automatizadas para recuperação, veja [restaurar uma base de dados a partir de cópias de segurança iniciadas pelo serviço](sql-database-recovery-using-backups.md).
* Para saber mais sobre as opções de recuperação mais rápidas, veja [grupos de georreplicação e ativação pós-falha Active Directory](sql-database-geo-replication-overview.md).  
