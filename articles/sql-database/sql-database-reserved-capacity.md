---
title: Efetue o pré-pagamento do vCores de SQL Database do Azure poupar dinheiro | Documentos da Microsoft
description: Saiba como comprar capacidade de base de dados do SQL Azure reservadas para poupar nos custos de computação.
services: sql-database
ms.service: sql-database
ms.subservice: ''
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: anosov1960
ms.author: sashan
ms.reviewer: carlrab
manager: craigg
ms.date: 09/20/2018
ms.openlocfilehash: e9edbc2a3bbc878f7a3cf99a3ca6efb4e69797ad
ms.sourcegitcommit: d1aef670b97061507dc1343450211a2042b01641
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/27/2018
ms.locfileid: "47394720"
---
# <a name="prepay-for-sql-database-compute-resources-with-azure-sql-database-reserved-capacity"></a>Efetue o pré-pagamento do recursos de computação de base de dados SQL com capacidade de base de dados do SQL Azure reservados

Poupe dinheiro com a base de dados do Azure SQL ao prepaying para recursos de computação de base de dados do Azure SQL em comparação comparados os preços pay as you go. Com capacidade de base de dados do SQL Azure reservado, efetuarem uma alocação adiantada na base de dados SQL durante um período de um ou três anos para obter um desconto significativo sobre os custos de computação. Para comprar capacidade de base de dados de SQL reservadas, terá de especificar a região do Azure, o tipo de implementação, o escalão de desempenho e o termo. 

Não é necessário atribuir a reserva a instâncias de base de dados SQL. Correspondência de instâncias de base de dados SQL, que já estão em execução ou aqueles que são implementados recentemente, obtém automaticamente o benefício. Ao comprar uma reserva, está a pagar previamente os custos de computação para as instâncias de base de dados SQL durante um período de um ou três anos. Assim que adquirir uma reserva, a base de dados de SQL, os custos de computação que correspondem os atributos de reserva já não são cobrados a pay as you go taxas. Uma reserva não abrange os custos de software, armazenamento ou nas redes associados à instância de base de dados SQL. No final do período de reserva, expira o benefício de faturação e as bases de dados do SQL são cobrados ao pay as you acedo preço. As reservas não de renovação automática. Para obter informações sobre preços, consulte a [base de dados SQL reservadas oferta de capacidade](https://azure.microsoft.com/pricing/details/sql-database/managed/).

Pode adquirir capacidade de base de dados do SQL Azure reservado [portal do Azure](https://portal.azure.com). Base de dados SQL de comprar capacidade de reserva:
- Tem de estar na função de proprietário para, pelo menos, uma empresa ou uma subscrição pay as you go.
- Para subscrições Enterprise, compras de reserva do Azure tem de estar ativadas no [portal EA](https://ea.azure.com).
-  Para o programa de fornecedor de soluções Cloud (CSP), apenas o admin de agentes ou agentes de vendas podem comprar capacidade de base de dados de SQL reservadas.

Os detalhes sobre como são cobrados os clientes empresariais e os clientes de pay as you go para compras de reserva, consulte [utilização de reserva de compreender o Azure da sua inscrição de Enterprise](../billing/billing-understand-reserved-instance-usage-ea.md) e [reserva de compreender o Azure utilização para a sua subscrição pay as you go](../billing/billing-understand-reserved-instance-usage.md).

## <a name="determine-the-right-sql-size-before-purchase"></a>Determinar a dimensão certa de SQL antes da compra

O tamanho da reserva deve ser com base na quantidade total de computação utilizada pelo SQL existente ou em breve-em--implementado único bases de dados e/ou conjuntos elásticos dentro de uma região específica e utilizar a mesma geração de hardware e de camada de desempenho. 

Por exemplo, vamos supor que está a executar uma finalidade geral, Gen5 – 16 vCore de conjunto elástico e dois crítico para a empresa, Gen5 – 4 vCore único bases de dados. Além disso, vamos suposto que planeja implantar no próximo mês, um para fins gerais adicionais, Gen5 – 16 vCore de conjunto elástico e um crítico para a empresa, Gen5 – conjunto elástico de vCore 32. Além disso, vamos supor que saiba que precisará estes recursos para, pelo menos, 1 ano. Neste caso, deve comprar um 32 vCores (2 x 16), a reserva de 1 ano de SQL da base de dados única/Elastic Pool de fins gerais da-computação Gen5 e um 40 (2 x 4 + 32) reserva de 1 ano de vCore para SQL da base de dados única/Elastic Pool críticas para a empresa - computação Gen5.

## <a name="buy-sql-database-reserved-capacity"></a>Comprar capacidade de base de dados de SQL reservadas

1. Inicie sessão no [portal do Azure](https://portal.azure.com).
2. Selecione **todos os serviços** > **reservas**.
3. Selecione **Add** e, em seguida, no painel de selecionar o tipo de produto, selecione **base de dados SQL** para comprar uma reserva nova para a base de dados SQL.
4. Preencha os campos obrigatórios. Existentes ou novas bases de dados únicas ou elástica agrupamentos que correspondam aos atributos que selecionar se qualificar para obter o desconto de capacidade de reserva. O número real de suas instâncias de base de dados SQL que obter o desconto depende do escopo e quantidade selecionado.

   ![Captura de ecrã antes de submeter a base de dados de SQL reservadas compra de capacidade](./media/sql-database-reserved-vcores/sql-reserved-vcores-purchase.png)

    | Campo      | Descrição|
    |:------------|:--------------|
    |Nome        |O nome desta reserva.| 
    |Subscrição|A subscrição utilizada para pagar a reserva de capacidade de base de dados de SQL reservadas. O método de pagamento da subscrição é cobrado os custos iniciais para a reserva de capacidade de base de dados de SQL reservadas. O tipo de subscrição tem de ser um Contrato Enterprise (número da oferta: MS-AZR-0017P) ou o modelo Pay As You Go (número da oferta: MS-AZR-0003P). Para uma subscrição Enterprise, os custos são deduzidos do saldo de fidelização monetária da inscrição ou cobrados como utilização excedida. Para a subscrição Pay As You Go, os custos são debitados no cartão de crédito ou cobrados de acordo com o método de pagamento indicado na subscrição.|    
    |Âmbito       |Âmbito da reserva de vCore pode abranger uma subscrição ou várias subscrições (âmbito partilhado). Se selecionar: <ul><li>Subscrição individual - o desconto de reserva de vCore é aplicada às instâncias de base de dados SQL nesta subscrição. </li><li>O desconto de reserva de vCore partilhado - é aplicado às instâncias de base de dados SQL em execução no caso de subscrições no seu contexto de faturação. Para os clientes empresariais, o escopo compartilhado é a inscrição e inclui todas as subscrições (exceto as subscrições de desenvolvimento/teste) na inscrição. Para clientes pay as you go, o âmbito partilhado é todas as subscrições pay as you go a criada pelo administrador de conta.</li></ul>|
    |Região      |A região do Azure que é abrangida por base de dados SQL reservado reserva de capacidade.|    
    |Tipo de Implementação|O tipo de recurso SQL que deseja comprar a reserva para.|
    |Escalão de Desempenho|A camada de serviços para as instâncias de base de dados SQL.
    |Termo        |Um ano ou três anos.|
    |Quantidade    |O número de instâncias que está a ser comprado na base de dados SQL reservado reserva de capacidade. A quantidade é o número de instâncias de base de dados SQL que podem obter o desconto de faturação em execução. Por exemplo, se estiver a executar 10 instâncias de base de dados SQL nos EUA leste, em seguida, tem de especificar quantidade como 10 para maximizar o benefício de todas as máquinas em execução. |

5. Reveja o custo da base de dados SQL reservadas reserva de capacidade no **custos** secção.
6. Selecione **Comprar**.
7. Selecione **ver esta reserva** para ver o estado da sua compra.

## <a name="cancellations-and-exchanges"></a>Cancelamentos e trocas

Se precisar de cancelar a sua base de dados de SQL reservadas de reserva de capacidade, pode haver uma taxa de rescisão antecipada de 12%. Reembolsos baseiam-se sobre o preço mais baixo do seu preço de compra ou o preço atual da reserva. Reembolsos estão limitados a 50 000 por ano. O reembolso que receber é o restante saldo pro-rata subtraindo a taxa de rescisão antecipada de 12%. Para pedir um cancelamento, vá para a reserva no portal do Azure e selecione **reembolsar** para criar um pedido de suporte.

Se precisar de alterar a sua reserva de capacidade de base de dados de SQL reservadas para outra região, tipo de implementação, o escalão de desempenho ou termo, podem trocá-lo para outro reserva do valor igual ou superior. A data de início do prazo para a nova reserva não passa da reserva trocada. Começa a partir ao criar a nova reserva de 1 ou o termo de 3 anos. Para pedir uma troca, vá para a reserva no portal do Azure e selecione **Exchange** para criar um pedido de suporte.

## <a name="vcore-size-flexibility"></a>flexibilidade de tamanho de vCore

flexibilidade de tamanho de vCore ajuda a aumentar ou reduzir verticalmente dentro de um escalão de desempenho e a região, sem perder os benefícios de capacidade de reserva. Capacidade de base de dados de SQL reservadas também fornece-lhe a flexibilidade temporariamente mover seus bancos de dados de acesso frequente entre conjuntos e bases de dados individuais como parte das suas operações normais (dentro da mesma camada de região e o desempenho) sem perder a capacidade de reserva benefício. Ao manter uma memória intermédia não aplicada na sua reserva, pode gerenciar com eficiência os picos de desempenho sem exceder seu orçamento.

## <a name="next-steps"></a>Passos Seguintes

O desconto de reserva de vCore é aplicado automaticamente para o número de instâncias de base de dados SQL que correspondem à base de dados de SQL reservadas âmbito da reserva de capacidade e atributos. Pode atualizar o âmbito da reserva de capacidade de base de dados de SQL reservadas através de [portal do Azure](https://portal.azure.com), PowerShell, CLI ou através da API. 

Para saber como gerir a base de dados de SQL reservadas de reserva de capacidade, consulte [capacidade de reserva de gerir a base de dados do SQL](../billing/billing-manage-reserved-vm-instance.md).

Para saber mais sobre as reservas do Azure, veja os artigos seguintes:

- [Quais são as reservas do Azure?](../billing/billing-save-compute-costs-reservations.md)
- [Gerir o Azure Reservations](../billing/billing-manage-reserved-vm-instance.md)
- [Compreender o que Azure reservas de desconto](../billing/billing-understand-reservation-charges.md)
- [Compreender a utilização de reserva para a sua subscrição pay as you go](../billing/billing-understand-reserved-instance-usage.md)
- [Compreender a utilização de reserva para inscrição da sua empresa](../billing/billing-understand-reserved-instance-usage-ea.md)
- [Reservas do Azure no programa de fornecedor de soluções (CSP) do parceiro Center na nuvem](https://docs.microsoft.com/partner-center/azure-reservations)

## <a name="need-help-contact-support"></a>Precisa de ajuda? Contactar o suporte

Se ainda tiver mais perguntas, [contacte o suporte](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) para a sua questão resolvidos rapidamente.

