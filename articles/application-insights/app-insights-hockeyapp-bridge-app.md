---
title: Explorar os dados do HockeyApp no Azure Application Insights | Documentos da Microsoft
description: Analise a utilização e desempenho da sua aplicação do Azure com o Application Insights.
services: application-insights
documentationcenter: windows
author: mrbullwinkle
manager: carmonm
ms.assetid: 97783cc6-67d6-465f-9926-cb9821f4176e
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: conceptual
ms.date: 03/30/2017
ms.author: mbullwin
ms.openlocfilehash: 1951f001de1b9d0667e5d4430454b32b5a3c890e
ms.sourcegitcommit: ada7419db9d03de550fbadf2f2bb2670c95cdb21
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/02/2018
ms.locfileid: "50957493"
---
# <a name="exploring-hockeyapp-data-in-application-insights"></a>Explorar os dados do HockeyApp no Application Insights

> [!NOTE]
> HockeyApp já não está disponível para novos aplicativos. HockeyApp as implementações existentes continuarão a funcionar. Visual Studio App Center agora é um serviço recomendado da Microsoft para a monitorização de novos aplicativos móveis. [Saiba como configurar as suas aplicações com o App Center e o Application Insights](app-insights-mobile-center-quickstart.md).

[HockeyApp](https://azure.microsoft.com/services/hockeyapp/) é um serviço para monitorização de aplicações em direto de dispositivos móveis e computadores. Do HockeyApp, pode enviar personalizado e rastrear a telemetria para monitorizar a utilização e ajudar no diagnóstico (além de obter dados de falhas). Este fluxo de telemetria pode ser consultado com a poderosa [Analytics](app-insights-analytics.md) recurso do [Azure Application Insights](app-insights-overview.md). Além disso, pode [exportar o personalizado e telemetria de rastreio](app-insights-export-telemetry.md). Para ativar estas funcionalidades, defina uma ponte que retransmite HockeyApp dados personalizados para o Application Insights.

## <a name="the-hockeyapp-bridge-app"></a>A aplicação de HockeyApp Bridge
A aplicação HockeyApp Bridge é o recurso de núcleos que permite-lhe aceder a HockeyApp personalizado e a telemetria de rastreio no Application Insights através das funcionalidades de análise e a exportação contínua. Eventos personalizados e de rastreio recolhidos pelo HockeyApp após a criação da aplicação HockeyApp Bridge serão acessível a partir desses recursos. Vamos ver como configurar uma destas aplicações de ponte.

No HockeyApp, abra as definições de conta [Tokens de API](https://rink.hockeyapp.net/manage/auth_tokens). Criar um novo token ou reutilizar um já existente. Os direitos mínimos necessários são "só de leitura". Fazer uma cópia da API do token.

![Obtenha o token de uma API de HockeyApp](./media/app-insights-hockeyapp-bridge-app/01.png)

Abra o portal do Microsoft Azure e [criar um recurso do Application Insights](app-insights-create-new-resource.md). Definir o tipo de aplicação como "Aplicação de HockeyApp bridge":

![Novo recurso do Application Insights](./media/app-insights-hockeyapp-bridge-app/02.png)

Não é necessário definir um nome - isto estará automaticamente definido do nome do HockeyApp.

Os campos de ponte de HockeyApp aparecem. 

![Introduza os campos de ponte](./media/app-insights-hockeyapp-bridge-app/03.png)

Introduza o token de HockeyApp que anotou anteriormente. Esta ação preenche o menu de lista pendente de "Aplicação de HockeyApp" com todas as suas aplicações do HockeyApp. Selecione a que pretende utilizar e conclua o restante dos campos. 

Abra o novo recurso. 

Tenha em atenção que os dados leva algum tempo para começarem a fluir.

![Recurso do Application Insights esperando pelos dados](./media/app-insights-hockeyapp-bridge-app/04.png)

Já está! Dados personalizados e de rastreio recolhidos na sua aplicação de HockeyApp instrumentado a partir deste ponto também estão agora disponíveis nos recursos de análises e exportação contínua do Application Insights.

Vamos revisar rapidamente cada uma destas funcionalidades agora disponíveis para si.

## <a name="analytics"></a>Análise
A análise é uma poderosa ferramenta de consulta ad hoc dos seus dados, permitindo-lhe diagnosticar e analisar a sua telemetria e descobrir rapidamente as causas raiz e padrões.

![Análise](./media/app-insights-hockeyapp-bridge-app/05.png)

* [Saiba mais sobre a análise](../log-analytics/query-language/get-started-analytics-portal.md)

## <a name="continuous-export"></a>Exportação contínua
A exportação contínua permite-lhe exportar os dados para um contentor de armazenamento de Blobs do Azure. Isso é muito útil se precisar de manter os seus dados por mais tempo do que o período de retenção atualmente oferecido pelo Application Insights. Pode manter os dados no armazenamento de BLOBs, processá-lo num banco de dados SQL ou a sua solução de armazenamento de dados preferenciais.

[Saiba mais sobre a exportação contínua](app-insights-export-telemetry.md)

## <a name="next-steps"></a>Passos Seguintes
* [Aplicar análises aos seus dados](../log-analytics/query-language/get-started-analytics-portal.md)

