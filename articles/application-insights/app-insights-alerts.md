---
title: Definir alertas no Application Insights do Azure | Documentos da Microsoft
description: Seja notificado sobre tempos de resposta lenta, exceções e outros desempenho ou alterações de utilização na sua aplicação web.
services: application-insights
documentationcenter: ''
author: mrbullwinkle
manager: carmonm
ms.reviewer: lagayhar
ms.assetid: f8ebde72-f819-4ba5-afa2-31dbd49509a5
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: conceptual
ms.date: 03/14/2017
ms.author: mbullwin
ms.openlocfilehash: 1061f356d75037bae440a5289413b2b5d17af1c9
ms.sourcegitcommit: 2b2129fa6413230cf35ac18ff386d40d1e8d0677
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/30/2018
ms.locfileid: "43246957"
---
# <a name="set-alerts-in-application-insights"></a>Definir alertas no Application Insights
[O Azure Application Insights] [ start] pode alertá-lo para as alterações nas métricas de desempenho ou a utilização na sua aplicação web. 

O Application Insights monitoriza a sua aplicação em direto numa [ampla variedade de plataformas] [ platforms] para o ajudar a diagnosticar problemas de desempenho e compreender os padrões de utilização.

Existem três tipos de alertas:

* **Alertas de métricas** informá-lo quando uma métrica ultrapassa um valor de limiar durante um determinado período - por exemplo, tempos de resposta, contagens de exceção, utilização da CPU ou vistas de página. 
* [**Testes Web** ] [ availability] informá-lo quando o site estiver disponível na internet ou está a responder lentamente. [Saiba mais][availability].
* [**Diagnósticos proativos** ](app-insights-proactive-diagnostics.md) são automaticamente configurados para notificá-lo sobre os padrões de desempenho invulgar.

Vamos nos concentrar nos alertas de métricas neste artigo.

## <a name="set-a-metric-alert"></a>Definir um alerta de métrica
Abra o painel de regras de alerta e, em seguida, utilize o botão Adicionar. 

![No painel de regras de alerta, escolha Adicionar alerta. Defina a aplicação que o recurso para medir, forneça um nome para o alerta e selecione uma métrica.](./media/app-insights-alerts/01-set-metric.png)

* Defina o recurso antes das outras propriedades. **Escolha o recurso de "(componentes)"** se pretender definir alertas em métricas de desempenho ou na utilização.
* O nome que irá fornecer ao alerta tem de ser exclusivo no grupo de recursos (não apenas a sua aplicação).
* Cuidado-se de que tenha em atenção as unidades em que lhe for pedido para introduzir o valor de limiar.
* Se selecionar a caixa "Proprietários de E-Mail...", os alertas são enviados por e-mail para todos os utilizadores quem tem acesso a este grupo de recursos. Para expandir este conjunto de pessoas, adicioná-los para o [grupo de recursos ou subscrição](app-insights-resources-roles-access-control.md) (não o recurso).
* Se especificar "E-mails adicionais", os alertas são enviados para esses indivíduos ou grupos (ou não tiver selecionado a caixa "... os proprietários de e-mail"). 
* Definir um [webhook endereço](../monitoring-and-diagnostics/insights-webhooks-alerts.md) se tiver configurado uma aplicação web que responde a alertas. Ele é chamado quando o alerta está ativado e quando este estiver resolvido. (Mas observe que, neste momento, parâmetros de consulta não são submetidos como propriedades de webhook.)
* Pode desativar ou ativar o alerta: consulte os botões na parte superior do painel.

*Não vejo no botão Adicionar alerta.* 

* Está a utilizar uma conta da organização? Pode definir alertas se tiver de proprietário ou Contribuidor aceder a este recurso de aplicação. Dar uma olhada no painel de controlo de acesso. [Saiba mais sobre o controlo de acesso][roles].

> [!NOTE]
> No painel de alertas, verá que já existe um conjunto de alerta cópia de segurança: [diagnósticos Proativos](app-insights-proactive-failure-diagnostics.md). O alerta automática monitoriza uma determinada métrica, pedido taxa de falhas. A menos que se optar por desativar o alerta proativo, não precisa definir sua própria alerta na taxa de falhas de pedido. 
> 
> 

## <a name="see-your-alerts"></a>Ver os seus alertas
Receber um e-mail quando um alerta muda de estado entre inativa e ativa. 

O estado atual de cada alerta é mostrado no painel de regras de alerta.

Há um resumo da atividade recente nos alertas pendentes:

![Alertas pendentes](./media/app-insights-alerts/010-alert-drop.png)

É o histórico de alterações de estado no registo de atividades:

![No painel de descrição geral, clique em configurações, registos de auditoria](./media/app-insights-alerts/09-alerts.png)

## <a name="how-alerts-work"></a>Como funcionam os alertas
* Um alerta tem três Estados: "Nunca ativado", "Ativado" e "Resolvido". Ativado significa que a condição especificada era true, quando foi avaliado pela última vez.
* Uma notificação é gerada quando um alerta muda de estado. (Se a condição de alerta já era verdadeira quando criou o alerta, poderá não receber uma notificação até que a condição for false.)
* Cada notificação gera uma mensagem de e-mail, se selecionado a caixa de mensagens de correio eletrónico ou fornecido endereços de e-mail. Também pode ver a notificações na lista pendente.
* Um alerta é avaliado sempre que uma métrica chega, mas, caso não o contrário.
* A avaliação agrega a métrica ao longo do período anterior e, em seguida, compara-o limiar para determinar o estado de novo.
* O período que escolher Especifica o intervalo durante o qual as métricas são agregadas. Não afeta a frequência de avaliação do alerta: que depende da frequência de chegada de métricas.
* Se não existem dados são recebidos para uma métrica em particular há algum tempo, a lacuna tem efeitos diferentes na avaliação de alerta e dos gráficos no Explorador de métrica. No Explorador de métrica, se nenhum dado é visto por mais tempo do que o intervalo de amostragem do gráfico, o gráfico mostra um valor de 0. Mas um alerta com base na mesma métrica não é reavaliada, e o estado do alerta permanece inalterado. 
  
    Quando dados eventualmente chegam, o gráfico salta para um valor diferente de zero. O alerta é avaliada com base nos dados disponíveis para o período especificado. Se o novo ponto de dados é o único disponível no período, a agregação baseia-se apenas no ponto de dados.
* Um alerta pode cintilação frequentemente entre os Estados de alerta e bom estado de funcionamento, mesmo que defina um longo período. Isto pode acontecer se o valor da métrica passa á volta o limiar. Não existe nenhum hysteresis no limiar: a transição para o alerta acontece com o mesmo valor que a transição para o bom estado de funcionamento.

## <a name="what-are-good-alerts-to-set"></a>O que são alertas de bom para definir?
Depende de seu aplicativo. Para começar, é melhor não definir métricas demasiadas. Passe algum tempo observando os gráficos de métricas, enquanto a aplicação está em execução, para ter uma noção de como ele se comporta normalmente. Essa prática ajuda-o a encontrar formas de melhorar seu desempenho. Em seguida, defina alertas para informá-lo quando as métricas enviados para fora da zona normal. 

Alertas populares incluem:

* [Métricas de browser][client], especialmente Browser **tempos de carregamento de página**, são ideais para aplicações web. Se sua página tiver muitos scripts, deve buscar **exceções do browser**. Para obter estas métricas e alertas, terá de configurar [monitorização de página web][client].
* **Tempo de resposta do servidor** no lado do servidor de aplicativos web. Bem como a configuração de alertas, manter a par esta métrica para ver se desproporcional varia com taxas de pedidos de alta: variação pode indicar que a aplicação está a ficar sem recursos. 
* **Exceções de servidor** - para vê-los, precisa fazer algumas [programa de configuração adicional](app-insights-asp-net-exceptions.md).

Não se esqueça de que [diagnóstico de taxa de falha proativos](app-insights-proactive-failure-diagnostics.md) monitorizar automaticamente a taxa a que a aplicação responde a solicitações com códigos de falha. 

## <a name="automation"></a>Automatização
* [Utilize o PowerShell para automatizar a configuração de alertas](app-insights-powershell-alerts.md)
* [Utilizar webhooks para automatizar a responder a alertas](../monitoring-and-diagnostics/insights-webhooks-alerts.md)

## <a name="video"></a>Vídeo

> [!VIDEO https://channel9.msdn.com/events/Connect/2016/112/player]

## <a name="see-also"></a>Consulte também
* [Testes web de disponibilidade](app-insights-monitor-web-app-availability.md)
* [Automatizar a configuração de alertas](app-insights-powershell-alerts.md)
* [Diagnósticos proativos](app-insights-proactive-diagnostics.md) 

<!--Link references-->

[availability]: app-insights-monitor-web-app-availability.md
[client]: app-insights-javascript.md
[platforms]: app-insights-platforms.md
[roles]: app-insights-resources-roles-access-control.md
[start]: app-insights-overview.md

