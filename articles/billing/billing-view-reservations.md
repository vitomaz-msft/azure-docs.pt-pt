---
title: Ver as reservas do Azure | Documentos da Microsoft
description: Saiba como ver as reservas do Azure no portal do Azure.
services: billing
documentationcenter: ''
author: yashesvi
manager: yashar
editor: ''
ms.service: billing
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/03/2018
ms.author: cwatson
ms.openlocfilehash: 2ae30ca55f3ca03a64438025960ddd807e288216
ms.sourcegitcommit: f58fc4748053a50c34a56314cf99ec56f33fd616
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/04/2018
ms.locfileid: "48272651"
---
# <a name="view-azure-reservations-in-the-azure-portal"></a>Ver as reservas de Azure no portal do Azure

Dependendo do tipo de subscrição e permissões, existem duas formas para ver as reservas do Azure.

## <a name="view-reservations-as-owner-or-reader"></a>Reservas de exibição como proprietário ou leitor

Por predefinição, quando adquire uma reserva, e o administrador de conta podem ver a reserva. O administrador de conta e obtém automaticamente a função de proprietário a reserva. Para permitir que outras pessoas ver a reserva, tem de adicioná-los como um **proprietário** ou **leitor** a reserva. Para obter mais informações, consulte [adicionar ou alterar os utilizadores que podem gerir uma reserva](billing-manage-reserved-vm-instance.md#add-or-change-users-who-can-manage-a-reservation).
 
Para ver uma reserva como proprietário ou leitor,

1. Inicie sessão no [portal do Azure]( http://portal.azure.com).
1. Pesquisar nos **reservas**.

    ![Captura de ecrã que mostra a pesquisa de portal do Azure](./media/billing-view-reservation/portal-reservation-search.png)

1. Deverá ver uma lista das reservas onde tem a função de proprietário ou leitor.

Se precisar de alterar o âmbito de uma reserva, dividir uma reserva ou alteração que pode gerir uma reserva, consulte [Gerir reservas de Azure](billing-manage-reserved-vm-instance.md).

## <a name="view-reservation-transactions-for-enterprise-enrollments"></a>Ver transações de reserva para inscrições de empresas

 Se tiver uma inscrição de Enterprise parceiro por instrutor, ver as reservas ao aceder **relatórios** no portal do EA. Para outras inscrições de empresas, pode ver as reservas no portal do EA e no portal do Azure. Tem de ser um administrador EA para exibir transações de reserva.

Para exibir transações de reserva no portal do Azure,

1. Inicie sessão no [portal do Azure]( http://portal.azure.com).
1. Pesquisar nos **custo Management + faturação**.

    ![Captura de ecrã que mostra a pesquisa de portal do Azure](./media/billing-view-reservation/portal-cm-billing-search.png)

1. Selecione **transações de reserva**.
1. Para filtrar os resultados, selecione **Timespan**, **tipo**, ou **Descrição**.
1. Selecione **Aplicar**.

    ![Captura de ecrã que mostra os resultados de transações de reserva](./media/billing-view-reservation/portal-billing-reservation-transaction-results.png)

Para obter os dados através de uma API, consulte [tarifas de transação de obter a instância reservada para os clientes empresariais](/rest/api/billing/enterprise/billing-enterprise-api-reserved-instance-charges).

## <a name="next-steps"></a>Passos Seguintes

Para saber mais sobre as reservas do Azure, veja os artigos seguintes:

- [Quais são as reservas do Azure?](billing-save-compute-costs-reservations.md)
- [Efetue o pré-pagamento do Cosmos DB reservado capacidade](../cosmos-db/cosmos-db-reserved-capacity.md)
- [Efetuar o pré-pagamento de recursos de computação da Base de Dados SQL com a capacidade reservada da Base de Dados SQL do Azure](../sql-database/sql-database-reserved-capacity.md)
- [Efetuar o pré-pagamento de Máquinas Virtuais com o Azure Reserved VM Instances](../virtual-machines/windows/prepay-reserved-vm-instances.md)
- [Gerir o Azure Reservations](billing-manage-reserved-vm-instance.md)
- [Compreender a utilização de reserva para a sua subscrição pay as you go](billing-understand-reserved-instance-usage.md)
- [Compreender a utilização de reserva para inscrição da sua empresa](billing-understand-reserved-instance-usage-ea.md)
- [Compreender a utilização de reserva para subscrições de CSP](https://docs.microsoft.com/partner-center/azure-reservations)

## <a name="need-help-contact-support"></a>Precisa de ajuda? Contactar o suporte

Se ainda tiver mais perguntas, [contacte o suporte](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) para a sua questão resolvidos rapidamente.
