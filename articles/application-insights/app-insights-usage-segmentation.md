---
title: Análise de usuário, sessão e eventos no Application Insights do Azure | Documentos da Microsoft
description: Análise demográficas de utilizadores da sua aplicação web.
services: application-insights
documentationcenter: ''
author: NumberByColors
manager: carmonm
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: multiple
ms.topic: conceptual
ms.date: 01/24/2018
ms.reviewer: mbullwin
ms.pm_owner: daviste;NumberByColors
ms.author: daviste
ms.openlocfilehash: 6fd8adab93f5741afe6d3eab0c50ca50a327fbff
ms.sourcegitcommit: 0f54b9dbcf82346417ad69cbef266bc7804a5f0e
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/26/2018
ms.locfileid: "50140337"
---
# <a name="users-sessions-and-events-analysis-in-application-insights"></a>Análise de utilizadores, sessões e eventos no Application Insights

Descubra quando as pessoas utilizam a sua aplicação web, quais são as páginas que estiverem mais interessados, onde os utilizadores estão localizados e quais navegadores e sistemas operativos que utilizam. Analisar a telemetria de negócio e a utilização utilizando [do Azure Application Insights](app-insights-overview.md).

![Captura de ecrã de utilizadores do Application Insights](./media/app-insights-usage-segmentation/0001-users.png)

## <a name="get-started"></a>Introdução

Se ainda não vir dados nos utilizadores, sessões ou painéis de eventos no portal do Application Insights, [Saiba como começar com as ferramentas de utilização](app-insights-usage-overview.md).

## <a name="the-users-sessions-and-events-segmentation-tool"></a>A ferramenta de segmentação de utilizadores, sessões e eventos

Três dos painéis de utilização utilizam a mesma ferramenta para examinar telemetria da sua aplicação web de três perspectivas. Ao filtrar e dividir os dados, pode descobrir informações sobre a utilização relativa de diferentes páginas e funcionalidades.

* **Ferramenta utilizadores**: quantas pessoas utilizado a sua aplicação e as respetivas funcionalidades.  Os utilizadores são contabilizados ao utilizar IDs anônimos armazenados em cookies do browser. Uma única pessoa através de browsers diferentes ou em máquinas será contada como mais de um usuário.
* **Ferramenta de sessões**: O número de sessões de atividade do utilizador incluem determinadas páginas e funcionalidades da sua aplicação. Uma sessão é contabilizada após a meia hora de inatividade do usuário ou após 24 horas de utilização contínua.
* **Ferramenta de eventos**: com que frequência são utilizadas determinadas páginas e funcionalidades da sua aplicação. Uma vista de página é contabilizada quando um navegador carrega uma página da sua aplicação, desde que tenha [instrumentada-](app-insights-javascript.md). 

    Um evento personalizado representa uma ocorrência de algo acontecer na sua aplicação, muitas vezes, uma interação do usuário, como o clique de um botão ou a conclusão de alguma tarefa. Insira o código na sua aplicação para [gerar eventos personalizados](app-insights-api-custom-events-metrics.md#trackevent).

## <a name="querying-for-certain-users"></a>Para determinados consultando os utilizadores

Explore os diferentes grupos de utilizadores ao ajustar as opções de consulta na parte superior da ferramenta utilizadores:

* Show: Escolha uma coorte de utilizadores para analisar.
* Quem utilizou: Escolha os eventos personalizados e vistas de página.
* Durante a: Escolha um intervalo de tempo.
* Por: Escolha como registo de dados, por um período de tempo ou por outra propriedade, como o navegador ou cidade.
* Dividir por: Escolha uma propriedade pelo qual dividir ou segmento os dados. 
* Adicionar filtros: Limite a consulta para determinados utilizadores, sessões ou eventos com base em suas propriedades, como o navegador ou cidade. 
 
## <a name="saving-and-sharing-reports"></a>A guardar e partilhar relatórios 
Pode salvar relatórios de utilizadores, privados apenas para na secção os meus relatórios ou partilhados com pessoas com acesso a este recurso do Application Insights na secção relatórios partilhados.

Para partilhar uma ligação a um relatório de utilizadores, sessões ou eventos; Clique em **partilha** na barra de ferramentas, em seguida, copie a ligação.

Para partilhar uma cópia dos dados num relatório de utilizadores, sessões ou eventos; Clique em **partilha** na barra de ferramentas, clique nas **ícone do Word** para criar um documento do Word com os dados. Em alternativa, clique nas **ícone de Word** acima do gráfico principal.

## <a name="meet-your-users"></a>Conheça os seus utilizadores

O **satisfazer os seus utilizadores** seção mostra informações de cerca de cinco utilizadores de exemplo correspondidos por consulta atual. Considerando e explorar os comportamentos de indivíduos, além de agregados, podem fornecer informações sobre como as pessoas utilizam, na verdade, a sua aplicação.

## <a name="next-steps"></a>Passos Seguintes

- Para permitir experiências de utilização, começar a enviar [eventos personalizados](https://docs.microsoft.com/azure/application-insights/app-insights-api-custom-events-metrics#trackevent) ou [vistas de página](https://docs.microsoft.com/azure/application-insights/app-insights-api-custom-events-metrics#page-views).
- Se já tiver enviar eventos personalizados ou vistas de página, explore as ferramentas de utilização para saber como os utilizadores utilizam o seu serviço.
    - [Funis](usage-funnels.md)
    - [Retenção](app-insights-usage-retention.md)
    - [Fluxos do Utilizador](app-insights-usage-flows.md)
    - [Livros](app-insights-usage-workbooks.md)
    - [Adicionar contexto de utilizador](app-insights-usage-send-user-context.md)