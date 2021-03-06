---
title: Resolver problemas de consultas do Azure Stream Analytics
description: Este artigo descreve técnicas para resolver problemas de suas consultas em tarefas do Azure Stream Analytics.
services: stream-analytics
author: sidram
ms.author: sidram
ms.reviewer: mamccrea
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 10/11/2018
ms.openlocfilehash: c437f350e394dc8c264903508a2a5a66fa8225a7
ms.sourcegitcommit: 1aacea6bf8e31128c6d489fa6e614856cf89af19
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/16/2018
ms.locfileid: "49346862"
---
# <a name="troubleshoot-azure-stream-analytics-queries"></a>Resolver problemas de consultas do Azure Stream Analytics

Este artigo descreve problemas comuns com o desenvolvimento de consultas do Stream Analytics e como resolvê-los.

## <a name="query-is-not-producing-expected-output"></a>Consulta não está a produzir saída esperada 
1.  Examine os erros ao testar localmente:
    - Sobre o **consulta** separador, selecione **teste**. Utilizar dados de exemplo transferidos para [testar a consulta](stream-analytics-test-query.md). Examine os erros e tentar corrigi-las.   
    - Também pode [testar a consulta diretamente na entrada em direto](stream-analytics-live-data-local-testing.md) com as ferramentas do Stream Analytics para Visual Studio.

2.  Se usar [ **Timestamp By**](https://msdn.microsoft.com/library/azure/mt573293.aspx), verifique se os eventos têm carimbos maior do que o [hora de início da tarefa](stream-analytics-out-of-order-and-late-events.md).

3.  Elimine armadilhas comuns, tais como:
    - R [ **onde** ](https://msdn.microsoft.com/library/azure/dn835048.aspx) cláusula na consulta filtrou todos os eventos, impedindo que qualquer saída a ser gerado.
    - R [ **CAST** ](https://msdn.microsoft.com/azure/stream-analytics/reference/cast-azure-stream-analytics) falhar, fazendo com que a tarefa efetuar a ativação de função. Para evitar falhas de conversão de tipo, utilize [ **TRY_CAST** ](https://msdn.microsoft.com/azure/stream-analytics/reference/try-cast-azure-stream-analytics) em vez disso.
    - Quando utiliza as funções de janela, aguarde que a duração de janela inteira ver o resultado da consulta.
    - O carimbo de hora para eventos precede a hora de início de tarefa e, portanto, os eventos estão a ser ignorados.

4.  Certifique-se de que as políticas de ordenação de eventos são configuradas conforme esperado. Vá para o **configurações** e selecione [ **ordenação de eventos**](stream-analytics-out-of-order-and-late-events.md). A política é *não* aplicada ao utilizar o **testar** botão para testar a consulta. O resultado é uma diferença entre testar no browser em oposição a executar a tarefa em produção. 

5. Depure com auditoria e registos de diagnóstico:
    - Uso [registos de auditoria](../azure-resource-manager/resource-group-audit.md)e filtre para identificar e depurar erros.
    - Uso [registos de diagnóstico da tarefa](stream-analytics-job-diagnostic-logs.md) para identificar e depurar erros.

## <a name="job-is-consuming-too-many-streaming-units"></a>Tarefa consome demasiadas unidades de transmissão em fluxo
Certifique-se de que pode tirar partido da paralelização no Azure Stream Analytics. Para aprender [dimensionar com da paralelização de consultas](stream-analytics-parallelization.md) de tarefas do Stream Analytics ao configurar partições de entrada e de otimização de definição da consulta do analytics.

## <a name="debug-queries-progressively"></a>Depurar consultas progressivamente

No processamento de dados em tempo real, a saber o que aspeto dos dados no meio de consulta pode ser útil. Como entradas ou passos de uma tarefa do Azure Stream Analytics podem ser lido várias vezes, pode escrever instruções SELECT INTO extras. Se o fizer, produz dados intermediários para o armazenamento e permite-lhe inspecionar a correção dos dados, assim como acontece *assista variáveis* quando depura um programa.

A seguinte consulta de exemplo num trabalho do Azure Stream Analytics tem um fluxo de entrada, duas entradas de dados de referência e uma saída para o armazenamento de tabelas do Azure. A consulta associa os dados do hub de eventos e blobs de referência de dois para obter as informações de nome e categoria:

![Exemplo de consulta SELECT INTO](./media/stream-analytics-select-into/stream-analytics-select-into-query1.png)

Tenha em atenção que a tarefa está em execução, mas não existem eventos estão a ser produzido na saída. Na **monitorização** mosaico, mostrado aqui, pode ver que a entrada produz dados, mas não sabe qual etapa da **associar** causado todos os eventos a ser removido.

![O mosaico de monitorização](./media/stream-analytics-select-into/stream-analytics-select-into-monitor.png)
 
Nesta situação, pode adicionar alguns Extras em instruções SELECT INTO "iniciar sessão" os resultados intermediários de associação e os dados que são lidos a partir de entrada.

Neste exemplo, adicionámos duas novas "temporárias saídas." Eles podem ser qualquer coletor que desejar. Aqui usamos armazenamento do Azure como um exemplo:

![Adicionar Extras em instruções SELECT INTO](./media/stream-analytics-select-into/stream-analytics-select-into-outputs.png)

Em seguida, pode reescrever a consulta como esta:

![Reescrito consulta SELECT INTO](./media/stream-analytics-select-into/stream-analytics-select-into-query2.png)

Agora, iniciar a tarefa novamente e deixar executar durante alguns minutos. Em seguida, consultar temp1 e temp2 com o Visual Studio Cloud Explorer para produzir as tabelas seguintes:

**tabela de temp1**
![SELECT INTO temp1 tabela](./media/stream-analytics-select-into/stream-analytics-select-into-temp-table-1.png)

**tabela de temp2**
![SELECT INTO temp2 tabela](./media/stream-analytics-select-into/stream-analytics-select-into-temp-table-2.png)

Como pode ver, temp1 e temp2 têm dados e a coluna de nome é preenchida corretamente no temp2. No entanto, uma vez que ainda não houver dados na saída, algo está errado:

![SELECT INTO output1 tabela sem dados](./media/stream-analytics-select-into/stream-analytics-select-into-out-table-1.png)

Pelos dados de amostragem, pode ser quase certo que o problema é com a associação de segundo. Pode transferir os dados de referência de blob e dar uma olhada:

![Tabela de ref SELECT INTO](./media/stream-analytics-select-into/stream-analytics-select-into-ref-table-1.png)

Como pode ver, o formato de GUID nestes dados de referência é diferente do formato da [coluna na temp2 from]. É por isso que os dados não chegam output1 conforme esperado.

Pode corrigir o formato de dados, carregue-o para o blob de referência e tente novamente:

![SELECT INTO tabela temporária](./media/stream-analytics-select-into/stream-analytics-select-into-ref-table-2.png)

Desta vez, os dados no resultado são formatados e preenchidos conforme esperado.

![SELECT INTO tabela final](./media/stream-analytics-select-into/stream-analytics-select-into-final-table.png)

## <a name="get-help"></a>Obter ajuda

Para obter assistência, tente nosso [fórum do Azure Stream Analytics](https://social.msdn.microsoft.com/Forums/azure/home?forum=AzureStreamAnalytics).

## <a name="next-steps"></a>Passos Seguintes

* [Introdução ao Azure Stream Analytics](stream-analytics-introduction.md)
* [Começar a utilizar o Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md)
* [Tarefas de escala do Azure Stream Analytics](stream-analytics-scale-jobs.md)
* [Referência do idioma de consulta do Azure Stream Analytics](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [Referência de API do REST de gestão do Azure Stream Analytics](https://msdn.microsoft.com/library/azure/dn835031.aspx)