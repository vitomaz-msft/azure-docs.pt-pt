---
title: Explorador de métricas do Azure Monitor
description: Saiba mais sobre os novos recursos do Explorador de métricas do Azure Monitor
author: vgorbenko
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 09/17/2017
ms.author: vitaly.gorbenko
ms.component: metrics
ms.openlocfilehash: f82b4dff20e2b26e62889c41b3ff3c27bc86066a
ms.sourcegitcommit: 7824e973908fa2edd37d666026dd7c03dc0bafd0
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/10/2018
ms.locfileid: "48901418"
---
# <a name="azure-monitor-metrics-explorer"></a>Explorador de métricas de Monitor do Azure

Explorador de métricas do Azure Monitor é um componente do portal do Microsoft Azure que lhe permite desenhar gráficos, visualmente correlacionar as tendências e investigar picos e quedas nos valores das métricas. Explorador de métricas é um ponto de partida essencial para vários problemas de disponibilidade com as suas aplicações e infraestrutura alojados no Azure ou monitorizado pelos serviços do Azure Monitor de desempenho e a investigar. 

## <a name="what-are-metrics-in-azure"></a>Quais são as métricas no Azure?

As métricas no Microsoft Azure estão a série de valores de medida e contagens de que são recolhidas e armazenadas ao longo do tempo. Existem métricas standard (ou "plataforma") e métricas personalizadas. As métricas standard são fornecidas pela própria plataforma do Azure. Métricas padrão refletem as estatísticas de estado de funcionamento e a utilização de recursos do Azure. Ao passo que as métricas personalizadas são enviadas para o Azure pelas suas aplicações com o [API do Application Insights para eventos personalizados](https://docs.microsoft.com/azure/application-insights/app-insights-api-custom-events-metrics). Métricas personalizadas são armazenadas nos recursos do Application Insights, juntamente com outras métricas específicas do aplicativo.

## <a name="how-do-i-create-a-new-chart"></a>Como posso criar um novo gráfico?

1. Abra o portal do Azure
2. Navegue para a nova **Monitor** separador e, em seguida, selecione **métricas**.

   ![Imagem de métricas](./media/monitoring-metric-charts/0001.png)

3. O **Seletor de métrica** será automaticamente aberta para. Escolha um recurso a partir da lista para ver o respetiva das métricas associadas. Apenas os recursos com as métricas são apresentados na lista.

   ![Imagem de métricas](./media/monitoring-metric-charts/0002.png)

   > [!NOTE]
   >Se tiver mais do que uma subscrição do Azure, o Explorador de métricas obtém os recursos em todas as subscrições selecionadas nas definições do Portal -> filtro por lista de subscrições. Para alterá-lo, clique no ícone de engrenagem de definições de Portal na parte superior do ecrã e selecione as subscrições que pretende utilizar.

4. Para alguns tipos de recursos (contas de armazenamento e máquinas virtuais), antes de selecionar uma métrica tem de escolher uma **espaço de nomes**. Cada espaço de nomes carrega seu próprio conjunto de métricas que são relevantes para este espaço de nomes apenas e não para outros namespaces.

   Por exemplo, cada armazenamento do Azure tem as métricas para subservices "Blobs", "Ficheiros", "Filas" e "Tabelas", que são todas as partes da conta de armazenamento. No entanto, a métrica "contagem de mensagens da fila" é naturalmente aplicável para o subservice "Fila" e não para quaisquer outros subservices de conta de armazenamento.

   ![Imagem de métricas](./media/monitoring-metric-charts/0003.png)

5. Selecione uma métrica da lista. Se souber o nome parcial da métrica que pretende, pode começar escrevendo-o para ver uma lista filtrada de métricas disponíveis:

   ![Imagem de métricas](./media/monitoring-metric-charts/0004.png)

6. Depois de selecionar uma métrica, o gráfico serão compostos com a agregação predefinida para a métrica selecionada. Neste momento só precisa clicar em distância, a partir da **Seletor de métricas** para fechá-lo. Opcionalmente, também pode mudar o gráfico para uma agregação diferente. Para algumas métricas, alternância de agregação permite-lhe escolher qual o valor que pretende ver no gráfico. Por exemplo, pode alternar entre os valores de média, mínimos e máximo. 

7. Ao clicar no ícone de adicionar métrica ![ícone de métrica](./media/monitoring-metric-charts/icon001.png) e repetir os passos 3 a 6 pode adicionar mais métricas no mesmo gráfico.

   > [!NOTE]
   > Geralmente não querem ter métricas com diferentes unidades de medida (ou seja, "milissegundos" e "quilobytes") ou com dimensionamento significativamente diferente num gráfico. Em vez disso, considere a utilização de vários gráficos. Clique no botão Adicionar gráfico para criar vários gráficos no Explorador de métricas.

## <a name="how-do-i-apply-filters-to-the-charts"></a>Como posso aplicar filtros para os gráficos?

Pode aplicar filtros para os gráficos que mostram as métricas com dimensões. Por exemplo, se a métrica "Contagem de transações" tem uma dimensão, "Tipo de resposta", que indica se a resposta das transações foi concluída com êxito ou falha, em seguida, filtrar esta dimensão seria desenhar uma linha do gráfico para apenas com êxito (ou apenas com falhas) transações. 

### <a name="to-add-a-filter"></a>Para adicionar um filtro:

1. Clique no ícone de adicionar filtro ![ícone de filtro](./media/monitoring-metric-charts/icon002.png) acima do gráfico

2. Selecione a dimensão (propriedade) que pretende filtrar

   ![imagem de métrica](./media/monitoring-metric-charts/0006.png)

3. Selecione os valores de dimensão que pretende incluir ao desenhar o gráfico (Este exemplo mostra a filtragem as transações de armazenamento com êxito):

   ![imagem de métrica](./media/monitoring-metric-charts/0007.png)

4. Depois de selecionar os valores de filtro, clique na direção oposta o Seletor de filtro para fechá-lo. Agora o gráfico mostra o número de transações de armazenamento tem falhado:

   ![imagem de métrica](./media/monitoring-metric-charts/0008.png)

5. Pode repetir os passos 1 a 4 para aplicar vários filtros para os mesmo gráficos.

## <a name="how-do-i-segment-a-chart"></a>Como posso segmentar um gráfico?

Pode dividir uma métrica por dimensão para visualizar como diferentes segmentos da comparação de métrica em relação a si e identificar os segmentos afastados de uma dimensão. 

### <a name="to-segment-a-chart"></a>Para segmentar um gráfico:

1. Clique no ícone de adicionar agrupamento  ![imagem de métrica](./media/monitoring-metric-charts/icon003.png) acima do gráfico.
 
   > [!NOTE]
   > Pode ter vários filtros, mas apenas um agrupamento em qualquer gráfico único.

2. Escolha uma dimensão no qual pretende segmentar o gráfico: 

   ![imagem de métrica](./media/monitoring-metric-charts/0010.png)

   Agora o gráfico mostra agora várias linhas, uma para cada segmento da dimensão:

   ![imagem de métrica](./media/monitoring-metric-charts/0012.png)

3. Clique de distância, a partir da **Seletor de agrupamento** para fechá-lo.

   > [!NOTE]
   > Utilize a filtragem e agrupamento na mesma dimensão para ocultar os segmentos que são irrelevantes para o seu cenário e facilitam a leitura de gráficos.

## <a name="how-do-i-pin-charts-to-dashboards"></a>Como posso afixar gráficos nos dashboards?

Depois de configurar os gráficos, pode querer adicioná-lo aos dashboards, para que possa vê-la novamente, possivelmente no contexto de outro telemetria de monitorização, ou partilhar com a sua equipa. 

Para afixar um gráfico configurado a um dashboard:

Depois de configurar o seu gráfico, clique nas **ações do gráfico** menu no lado direito principais do gráfico e clique em **afixar ao dashboard**.

   ![imagem de métrica](./media/monitoring-metric-charts/0013.png)

## <a name="next-steps"></a>Passos Seguintes

  Leia [criar dashboards de KPI personalizados](https://docs.microsoft.com/azure/application-insights/app-insights-tutorial-dashboards) para saber mais sobre as melhores práticas para a criação de dashboards passíveis de ação com a métrica.