---
title: Diminuir o desempenho de aplicações web no serviço de aplicações | Documentos da Microsoft
description: Este artigo ajuda-o a resolver problemas de desempenho de aplicações de web lento no serviço de aplicações do Azure.
services: app-service\web
documentationcenter: ''
author: cephalin
manager: erikre
editor: ''
tags: top-support-issue
keywords: desempenho das aplicações Web, aplicações lenta, lento da aplicação
ms.assetid: b8783c10-3a4a-4dd6-af8c-856baafbdde5
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/03/2016
ms.author: cephalin
ms.openlocfilehash: b1ae61b5372fa1061448c02e7b5c589a167888c9
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/07/2018
ms.locfileid: "51237218"
---
# <a name="troubleshoot-slow-web-app-performance-issues-in-azure-app-service"></a>Resolver problemas de desempenho de aplicações de web lento no serviço de aplicações do Azure
Este artigo ajuda-o a resolver problemas de desempenho de aplicações de web lenta na [App Service do Azure](https://go.microsoft.com/fwlink/?LinkId=529714).

Se precisar de mais ajuda a qualquer momento neste artigo, pode contactar os especialistas do Azure no [do Azure do MSDN e os fóruns de Stack Overflow](https://azure.microsoft.com/support/forums/). Em alternativa, também pode enviar um incidente de suporte do Azure. Vá para o [site de suporte do Azure](https://azure.microsoft.com/support/options/) e clique em **obter suporte**.

## <a name="symptom"></a>Sintoma
Ao navegar devagar da aplicação web, a carga de páginas e, às vezes, tempo limite.

## <a name="cause"></a>Causa
Esse problema normalmente é causado por problemas de nível de aplicativos, tais como:

* pedidos de rede a demorar muito tempo
* consultas de base de dados ou de código do aplicativo a ser ineficiente
* aplicação com a memória/CPU elevada
* aplicativo falha devido a uma exceção

## <a name="troubleshooting-steps"></a>Passos de resolução de problemas
Resolução de problemas pode ser dividida em três tarefas distintas, em ordem sequencial:

1. [Observar e monitorar o comportamento do aplicativo](#observe)
2. [Recolher dados](#collect)
3. [Mitigar o problema](#mitigate)

[Web Apps do App Service](app-service-web-overview.md) dá-lhe várias opções em cada passo.

<a name="observe" />

### <a name="1-observe-and-monitor-application-behavior"></a>1. Observar e monitorar o comportamento do aplicativo
#### <a name="track-service-health"></a>Controlar o estado de funcionamento do serviço
Microsoft Azure publicizes sempre que há uma degradação de desempenho ou interrupções de serviço. Pode acompanhar o estado de funcionamento do serviço no [portal do Azure](https://portal.azure.com/). Para obter mais informações, consulte [controlar o estado de funcionamento do serviço](../monitoring-and-diagnostics/insights-service-health.md).

#### <a name="monitor-your-web-app"></a>Monitorizar a sua aplicação web
Esta opção permite-lhe saber se a aplicação está a ter problemas. No painel da sua aplicação web, clique a **pedidos e erros** mosaico. O **métrica** painel mostra-lhe todas as métricas, pode adicionar.

Algumas das métricas que possa querer monitorizar para a sua aplicação web são

* Média do conjunto de trabalho de memória
* Tempo médio de resposta
* Tempo de CPU
* Conjunto de trabalho de memória
* Pedidos

![monitorizar o desempenho de aplicações web](./media/app-service-web-troubleshoot-performance-degradation/1-monitor-metrics.png)

Para obter mais informações, consulte:

* [Monitorizar aplicações da Web no serviço de aplicações do Azure](web-sites-monitor.md)
* [Receber notificações de alerta](../monitoring-and-diagnostics/insights-receive-alert-notifications.md)

#### <a name="monitor-web-endpoint-status"></a>Monitorizar o estado do ponto final web
Se estiver a executar a aplicação web **padrão** escalão de preço, aplicações Web permite-lhe monitorizar pontos de duas extremidade de três localizações geográficas.

Monitorização de pontos finais configura testes web de localizações geograficamente distribuídos que testam o tempo de resposta e o tempo de atividade dos URLs da web. O teste executa uma operação HTTP GET no URL de web para determinar o tempo de resposta e o tempo de atividade de cada localização. Cada localização configurada é executado um teste de cada cinco minutos.

Tempo de atividade é monitorizado com códigos de resposta HTTP e o tempo de resposta é medido em milissegundos. Um teste de monitorização falhar se o código de resposta HTTP é maior que ou igual a 400 ou se a resposta demora mais de 30 segundos. Um ponto final é considerado disponível se tiver êxito seus testes de monitorização de todas as localizações especificadas.

Para configurar, consulte [monitorizar aplicações no serviço de aplicações do Azure](web-sites-monitor.md).

Além disso, veja [manter os Web Sites do Azure backup e o monitoramento de ponto final - com Stefan Schackow](https://channel9.msdn.com/Shows/Azure-Friday/Keeping-Azure-Web-Sites-up-plus-Endpoint-Monitoring-with-Stefan-Schackow) para um vídeo sobre como monitorizar o ponto final.

#### <a name="application-performance-monitoring-using-extensions"></a>Monitorização de desempenho de aplicações com extensões
Também pode monitorizar o desempenho da aplicação ao utilizar um *extensão de site*.

Cada aplicação web do serviço de aplicações fornece um ponto de final de gestão extensível que permite que use um conjunto poderoso de ferramentas implementadas como extensões de site. Extensões incluem: 

- Gostam de editores de código fonte [do Azure DevOps](https://www.visualstudio.com/products/what-is-visual-studio-online-vs.aspx). 
- Ferramentas de gestão para recursos ligados, tais como a base de dados MySQL ligada a uma aplicação web.

[O Azure Application Insights](https://azure.microsoft.com/services/application-insights/) é um extensão de site que também está disponível de monitorização do desempenho. Para utilizar o Application Insights, reconstrua o seu código com um SDK. Também pode instalar uma extensão que fornece acesso a dados adicionais. O SDK permite-lhe escrever código para monitorizar a utilização e o desempenho da sua aplicação em mais detalhes. Para obter mais informações, consulte [monitorizar o desempenho em aplicativos web](../application-insights/app-insights-web-monitor-performance.md).

<a name="collect" />

### <a name="2-collect-data"></a>2. Recolher dados
O ambiente de aplicações Web fornece funcionalidade de diagnóstico para informações de registo do servidor web e a aplicação web. As informações são separadas em diagnóstico do servidor web e ao application diagnostics.

#### <a name="enable-web-server-diagnostics"></a>Ativar o diagnóstico do servidor web
Pode ativar ou desativar os seguintes tipos de registos:

* **Detalhadas de registo de erro** -erro informações detalhadas de códigos de estado HTTP que indicam uma falha (código de estado 400 ou superior). Isto pode conter informações que podem ajudar a determinar o motivo pelo qual o servidor devolveu o código de erro.
* **Falha do rastreio de pedido** -informações detalhadas sobre pedidos com falhas, incluindo um rastreamento dos componentes do IIS, usado para processar o pedido e o tempo decorrido em cada componente. Isso pode ser útil se está a tentar melhorar o desempenho das aplicações web ou isolar o que está a causar um erro HTTP específico.
* **O registo do servidor da Web** -informações sobre transações de HTTP utilizando o formato de ficheiro de registo expandido de W3C. Isto é útil ao determinar as métricas de aplicação web geral, como o número de pedidos processados ou o número de pedidos é de um endereço IP específico.

#### <a name="enable-application-diagnostics"></a>Ativar diagnósticos de aplicação
Existem várias opções para recolher dados de desempenho de aplicações a partir de aplicações Web, perfil de seu aplicativo em direto a partir do Visual Studio ou modificar o código da aplicação para obter mais informações e rastreios de registo. Pode escolher as opções com base nas ferramentas de acesso que terá do aplicativo e o que observadas da monitorização.

##### <a name="use-application-insights-profiler"></a>Utilizar o Application Insights Profiler
Pode habilitar o do Application Insights Profiler começar a capturar rastreios de desempenho detalhadas. Pode acessar rastreios capturados até cinco dias atrás quando precisa para investigar problemas aconteceu no passado. Pode escolher esta opção, desde que terá acesso ao recurso do Application Insights da aplicação web no portal do Azure.

Application Insights Profiler fornece estatísticas sobre o tempo de resposta para cada chamada de web e rastreios, que indica que linha de código causada as repostas lentas. Às vezes, a aplicação de serviço de aplicações é lenta porque certos tipos de código não é escrito num alto desempenho forma. Os exemplos incluem código seqüencial que pode ser executado em contenções de bloqueio de base de dados paralelas e indesejados. Remover esses afunilamentos no código aumenta o desempenho da aplicação, mas eles são difíceis de detectar sem configurar rastreios elaborados e registos. Os rastreios recolhidos pelo Application Insights Profiler ajuda a identificar as linhas de código que o aplicativo mais lento e superar esse desafio para as aplicações de serviço de aplicações.

 Para obter mais informações, consulte [aplicações web do Azure em direto de criação de perfil com o Application Insights](../application-insights/app-insights-profiler.md).

##### <a name="use-remote-profiling"></a>Utilizar a análise para otimização remota
No serviço de aplicações do Azure, aplicações Web, aplicações API e WebJobs pode ser remotamente as quais criar perfis. Escolha esta opção se tiver acesso ao recurso de aplicação web e sabe como reproduzir o problema ou, se souber o intervalo de hora exata a que o problema de desempenho acontece.

Criação de perfil remota é útil se a utilização da CPU do processo é alta e seu processo está a ser executado mais lentamente do que o esperado, ou a latência dos pedidos HTTP são superior ao normal, pode remotamente seu processo de perfil e obter as pilhas de chamadas de amostragem de CPU para analisar o processo atividade e o código de caminhos de acesso frequente.

Para obter mais informações, consulte [suporte de análise para otimização remota no serviço de aplicações do Azure](https://azure.microsoft.com/blog/remote-profiling-support-in-azure-app-service).

##### <a name="set-up-diagnostic-traces-manually"></a>Configurar manualmente o rastreios de diagnóstico
Se tiver acesso ao código de origem do aplicativo web, o Application diagnostics permite-lhe capturar informações produzidas por um aplicativo web. Os aplicativos ASP.NET podem usar o `System.Diagnostics.Trace` classe para registar informações no registo de diagnóstico de aplicação. No entanto, terá de alterar o código e Reimplementar a sua aplicação. Este método é recomendado se a aplicação está em execução num ambiente de teste.

Para obter instruções detalhadas sobre como configurar a sua aplicação para o registo, consulte [ativar o registo de diagnóstico para aplicações web no App Service do Azure](web-sites-enable-diagnostic-log.md).

#### <a name="use-the-azure-app-service-support-portal"></a>Utilizar o portal de suporte do serviço de aplicações do Azure
As aplicações Web fornece a capacidade de resolver problemas relacionados com a sua aplicação web ao observar a HTTP registos, registos de eventos, despejos de processo e muito mais. Pode acessar todas essas informações com o nosso portal de suporte em **http://&lt;o nome da aplicação >.scm.azurewebsites.net/Support**

O portal de suporte do serviço de aplicações do Azure fornece três separadores separados para suportar os três passos de um cenário de resolução de problemas comuns:

1. Observar o comportamento atual
2. Analisar a recolher informações de diagnóstico e executando os analisadores internos
3. Mitigar

Se o problema está acontecendo neste momento, clique em **Analyze** > **diagnósticos** > **diagnosticar agora** para criar uma sessão de diagnóstico, que recolhe registos de HTTP, registos do Visualizador de eventos, despejos de memória, registos de erros PHP e o relatório de processos PHP.

Quando os dados são recolhidos, o portal de suporte é executada uma análise dos dados e fornece-lhe um relatório HTML.

No caso de pretender transferir os dados, por padrão, ele seria armazenado na pasta D:\home\data\DaaS.

Para obter mais informações sobre o portal de suporte do serviço de aplicações do Azure, consulte [novas atualizações a extensão de Site de suporte para Web sites Azure](https://azure.microsoft.com/blog/new-updates-to-support-site-extension-for-azure-websites).

#### <a name="use-the-kudu-debug-console"></a>Utilizar a consola de depuração do Kudu
As aplicações Web é fornecido com uma consola de depuração que pode usar para depuração, explorar, carregar ficheiros, bem como pontos finais de JSON para obter informações sobre o seu ambiente. Esta consola é chamada de *consola Kudu* ou o *SCM Dashboard* para a sua aplicação web.

Pode aceder a este dashboard ao aceder à ligação **https://&lt;o nome da aplicação >.scm.azurewebsites.net/**.

Algumas das coisas que fornece o Kudu são:

* definições de ambiente para a sua aplicação
* fluxo de registo
* informação de diagnóstico
* consola na qual pode executar cmdlets do Powershell e comandos básicos de depuração.

Outro recurso útil do Kudu é que, no caso da aplicação que está a gerar exceções de primeira chance, pode utilizar o Kudu e despeja a ferramenta SysInternals Procdump para criar a memória. Esses despejos de memória são instantâneos do processo e, muitas vezes, podem ajudar a resolver problemas mais complicados, com a sua aplicação web.

Para obter mais informações sobre os recursos disponíveis no Kudu, consulte [ferramentas de DevOps do Azure que precisa conhecer](https://azure.microsoft.com/blog/windows-azure-websites-online-tools-you-should-know-about/).

<a name="mitigate" />

### <a name="3-mitigate-the-issue"></a>3. Mitigar o problema
#### <a name="scale-the-web-app"></a>Dimensionar a aplicação web
No serviço de aplicações do Azure, para aumentar o desempenho e o débito, pode ajustar a escala a que está a executar a aplicação. Aumentar verticalmente uma aplicação web envolve duas ações relacionadas: alterar o seu plano do serviço de aplicações para um escalão de preço superior e a configuração de determinadas definições depois que tiver mudado para o escalão de preço superior.

Para obter mais informações sobre o dimensionamento, veja [dimensionar uma aplicação web no serviço de aplicações do Azure](web-sites-scale.md).

Além disso, pode optar por executar a sua aplicação em mais de uma instância. Aumento horizontal não apenas fornece a mais capacidade de processamento, mas também fornece algum período de tolerância a falhas. Se o processo de ficar inativo numa instância, as outras instâncias continuam para servir pedidos.

Pode definir o dimensionamento Manual ou automático.

#### <a name="use-autoheal"></a>Utilizam o AutoHeal
AutoHeal recicla o processo de trabalho para a sua aplicação com base nas definições que escolher (como alterações de configuração, pedidos, limites baseados em memória ou o tempo necessário para executar um pedido). Na maioria das vezes, a forma mais rápida para recuperar a partir de um problema é a Reciclagem do processo. Embora sempre pode reiniciar a aplicação web a partir diretamente no portal do Azure, AutoHeal faz isso automaticamente para. Tudo o que precisa fazer é adicionar alguns acionadores ao Web. config de raiz para a sua aplicação web. Estas definições funciona da mesma forma, mesmo que seu aplicativo não é uma aplicação .net.

Para obter mais informações, consulte [Autorrecuperação dos Web Sites Azure](https://azure.microsoft.com/blog/auto-healing-windows-azure-web-sites/).

#### <a name="restart-the-web-app"></a>Reinicie a aplicação web
A reiniciar, muitas vezes, é a forma mais simples para recuperar rapidamente de problemas de uso individual. Sobre o [portal do Azure](https://portal.azure.com/), no painel da sua aplicação web, tem as opções para parar ou reiniciar a aplicação.

 ![reiniciar a aplicação web para resolver problemas de desempenho](./media/app-service-web-troubleshoot-performance-degradation/2-restart.png)

Também pode gerir a sua aplicação web com o Azure Powershell. Para obter mais informações, veja [Using Azure PowerShell with Azure Resource Manager (Utilizar o Azure PowerShell com o Azure Resource Manager)](../powershell-azure-resource-manager.md).
