---
title: Registos de atividades do Azure Active Directory no Azure Monitor (pré-visualização) | Microsoft Docs
description: Introdução à atividade do Azure Active Directory que inicia sessão no Azure Monitor (pré-visualização)
services: active-directory
documentationcenter: ''
author: priyamohanram
manager: mtillman
editor: ''
ms.assetid: 4b18127b-d1d0-4bdc-8f9c-6a4c991c5f75
ms.service: active-directory
ms.devlang: na
ms.topic: concept
ms.tgt_pltfrm: na
ms.workload: identity
ms.component: report-monitor
ms.date: 11/13/2018
ms.author: priyamo
ms.reviewer: dhanyahk
ms.openlocfilehash: 760110d0ac359f6b7f135bf869e2520b8028ba6e
ms.sourcegitcommit: 1f9e1c563245f2a6dcc40ff398d20510dd88fd92
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/14/2018
ms.locfileid: "51625441"
---
# <a name="azure-ad-activity-logs-in-azure-monitor-preview"></a>Registos de atividades do Azure AD no Azure Monitor (pré-visualização)

Agora, pode encaminhar registos de atividades do Azure Active Directory (Azure AD) vários pontos finais de longo termo dados e retenção de informações. A pré-visualização pública dos registos do Azure AD no Azure Monitor permite-lhe:

* Registos de atividades de arquivo do Azure AD para uma conta de armazenamento do Azure, para manter os dados durante muito tempo.
* Registos de atividades de Stream do Azure AD para um hub de eventos do Azure para análise, com ferramentas populares de informações de segurança e gestão de eventos (SIEM), como Splunk e QRadar.
* Integrar o Azure AD registos de atividades com suas próprias soluções de registo personalizado por transmissão em fluxo para um hub de eventos.
* Registos de atividade de envio do Azure AD para o Log Analytics para ativar visualizações ricas, monitorização e alertas sobre os dados ligados.

> [!VIDEO https://www.youtube.com/embed/syT-9KNfug8]

## <a name="supported-reports"></a>Relatórios suportados

Pode encaminhar o Azure AD de auditoria registos e registos de início de sessão para a sua conta de armazenamento do Azure, hub de eventos, o Log Analytics ou solução personalizada ao utilizar esta funcionalidade. 

* **Registos de auditoria**: o [relatório de atividades de registos de auditoria](concept-audit-logs.md) dá-lhe acesso ao histórico de cada tarefa que é executada no seu inquilino.
* **Registos de inícios de sessão**: com o [relatório de atividades de inícios de sessão](concept-sign-ins.md), pode saber quem executou as tarefas reportadas no relatório de registos de auditoria.

> [!NOTE]
> A auditoria relacionada com B2C e os registos de atividades de inícios de sessão não são suportados atualmente.
>

## <a name="prerequisites"></a>Pré-requisitos

Para utilizar esta funcionalidade, precisa de:

* Uma subscrição do Azure. Se não tiver uma subscrição do Azure, pode [inscrever-se para obter uma avaliação gratuita](https://azure.microsoft.com/free/).
* [Licença](https://azure.microsoft.com/pricing/details/active-directory/) do Azure AD Gratuito, Básico, Premium 1 ou Premium 2 para aceder aos registos de auditoria do Azure AD no portal do Azure. 
* Um inquilino do Azure AD.
* Um utilizador que seja **administrador global** ou **administrador de segurança** do inquilino do Azure AD.
* [Licença](https://azure.microsoft.com/pricing/details/active-directory/) do Azure AD Premium 1 ou Premium 2 para aceder aos registos de início de sessão do Azure AD no portal do Azure. 

Dependendo do local para onde pretende encaminhar os dados de registo de auditoria, precisa de um dos seguintes:

* Uma conta de armazenamento do Azure, para a qual tenha permissões *ListKeys*. Recomendamos que utilize uma conta de armazenamento para fins gerais e não uma conta de armazenamento de Blobs. Para obter informações sobre os preços de armazenamento, veja a [Calculadora de preços do Armazenamento do Azure](https://azure.microsoft.com/pricing/calculator/?service=storage). 
* Um espaço de nomes dos Hubs de Eventos do Azure, para integrar com soluções de terceiros.
* Uma área de trabalho do Log Analytics do Azure para enviar registos para o Log Analytics.

## <a name="cost-considerations"></a>Considerações de custos

Se já tiver uma licença do Azure AD, precisa de uma subscrição do Azure para configurar a conta de armazenamento e o hub de eventos. A subscrição do Azure é fornecida sem custos, mas terá de pagar para utilizar os recursos do Azure, incluindo a conta de armazenamento que utilizar para arquivo e o hub de eventos que utilizar para transmissão em fluxo. A quantidade de dados e, por conseguinte, o custo incorrido, pode variar significativamente consoante o tamanho do inquilino. 

### <a name="storage-size-for-activity-logs"></a>Tamanho de armazenamento para registos de atividades

Cada evento de registo de auditoria consome cerca de 2 KB de armazenamento de dados. Para um inquilino com 100 000 utilizadores, o que implicaria cerca de 1,5 milhões de eventos por dia, precisaria de aproximadamente 3 GB de armazenamento de dados por dia. Uma vez que ocorrem escritas em lotes com a duração aproximada de cinco minutos, é possível prever aproximadamente 9000 operações de escrita por mês. 

A tabela seguinte contém uma estimativa do custo, dependendo do tamanho do inquilino, de uma conta de armazenamento para fins gerais v2 nos E.U.A. Oeste durante, pelo menos, um ano de retenção. Para criar uma estimativa mais exata do volume de dados que prevê para a sua aplicação, utilize a [calculadora de preços do armazenamento do Azure](https://azure.microsoft.com/pricing/details/storage/blobs/). 

| Categoria do registo | Número de utilizadores | Eventos por dia | Volume de dados por mês (est.) | Custo por mês (est.) | Custo por ano (est.) |
|--------------|-----------------|----------------------|--------------------------------------|----------------------------|---------------------------|
| Auditoria | 100 000 | 1,5&nbsp;milhões | 90 GB | $1,93 | $23,12 |
| Auditoria | 1,000 | 15 000 | 900 MB | $0,02 | $0,24 |
| Inícios de sessão | 1,000 | 34 800 | 4GB | $0,13 | $1,56 |
| Inícios de sessão | 100 000 | 15&nbsp;milhões | 1,7 TB | $35,41 | $424,92 | 


### <a name="event-hub-messages-for-activity-logs"></a>Mensagens do hub de eventos para os registos de atividades

Os eventos são colocados em lote com aproximadamente cinco minutos de intervalo e são enviados como uma única mensagem que contém todos os eventos durante esse período de tempo. Uma mensagem no hub de eventos tem um tamanho máximo de 256 KB e, se o tamanho total de todas as mensagens durante o período de tempo exceder esse volume, são enviadas várias mensagens. 

Por exemplo, para um inquilino grande com mais de 100 000 utilizadores, ocorrem normalmente cerca de 18 eventos por segundo, uma taxa que equivale a 5400 eventos de cinco em cinco minutos. Uma vez que os registos de auditoria têm perto de 2k por evento, equivale a 10,8 MB de dados. Consequentemente, são enviadas 43 mensagens para o hub de eventos nesse intervalo de cinco minutos. 

A tabela seguinte contém os custos estimados por mês para um hub de eventos básico nos E.U.A. Oeste, consoante o volume de dados de eventos. Para calcular uma estimativa exata do volume de dados que prevê para a sua aplicação, utilize a [calculadora de preços do Hub de Eventos](https://azure.microsoft.com/pricing/details/event-hubs/).

| Categoria do registo | Número de utilizadores | Eventos por segundo | Eventos por intervalo de cinco minutos | Volume por intervalo | Mensagens por intervalo | Mensagens por mês | Custo por mês (est.) |
|--------------|-----------------|-------------------------|----------------------------------------|---------------------|---------------------------------|------------------------------|----------------------------|
| Auditoria | 100 000 | 18 | 5400 | 10,8 MB | 43 | 371 520 | $10,83 |
| Auditoria | 1,000 | 0.1 | 52 | 104 KB | 1 | 8640 | 10,80 $ |
| Inícios de sessão | 1,000 | 178 | 53 400 | 106,8&nbsp;MB | 418 | 3.611.520 | $11,06 |  

### <a name="log-analytics-cost-considerations"></a>Considerações de custos do log Analytics

Para rever os custos relacionados ao gerenciamento de área de trabalho do Log Analytics, consulte [gerir os custos ao controlar o volume de dados e a retenção do Log Analytics](https://docs.microsoft.com/azure/log-analytics/log-analytics-manage-cost-storage).

## <a name="frequently-asked-questions"></a>Perguntas mais frequentes

Esta secção responde às perguntas mais frequentes e inclui discussões sobre problemas conhecidos dos registos do Azure AD no Azure Monitor.

**P: Que registos estão incluídos?**

**R:** Tanto os registos de inícios de sessão, como os registos de auditoria, estão disponíveis para encaminhamento através desta funcionalidade, embora os eventos de auditoria relacionados com B2C não estejam atualmente incluídos. Para conhecer os tipos de registos e que registos baseados na funcionalidade são atualmente suportados, veja [Audit log schema](reference-azure-monitor-audit-log-schema.md) (Esquema de registos de auditoria) e [Sign-in log schema](reference-azure-monitor-sign-ins-log-schema.md) (Esquema de registo de inícios de sessão). 

---

**P: como logo após uma ação os registos correspondentes aparecerá no meu hub de eventos?**

**R**: os registos devem aparecer no hub de eventos entre dois a cinco minutos após a ação ter sido realizada. Para obter mais informações sobre os Hubs de Eventos, veja [O que são os Hubs de Eventos do Azure?](../../event-hubs/event-hubs-about.md)

---

**P: como logo após uma ação os registos correspondentes aparecerá na minha conta de armazenamento?**

**R**: Relativamente às contas de armazenamento do Azure, a latência situa-se entre 5 e 15 minutos após a ação ter sido realizada.

---

**P: Qual será o custo de armazenar os meus dados?**

**R**: Os custos de armazenamento dependem do tamanho dos seus registos e do período de retenção que escolher. Para obter uma lista dos custos estimados para os inquilinos, que dependerão do volume de registos gerados, veja a secção [Tamanho do armazenamento para os registos de atividades](#storage-size-for-activity-logs).

---

**P: Qual será o custo de transmitir os meus dados para os hubs de eventos?**

**R**: Os custos da transmissão em fluxo dependem do número de mensagens que recebe por minuto. Este artigo mostra como é que os custos são calculados e apresenta uma lista das estimativas de custos, que têm por base o número de mensagens. 

---

**P: Como posso integrar registos de atividades do Azure AD no meu sistema SIEM?**

**R:** Pode fazê-lo de duas maneiras:

- Utilize o Azure Monitor com os Hubs de Eventos para transmitir os registos para o seu sistema SIEM. Primeiro, [transmita os registos para um hub de eventos](tutorial-azure-monitor-stream-logs-to-event-hub.md) e, em seguida [configure a ferramenta SIEM](tutorial-azure-monitor-stream-logs-to-event-hub.md#access-data-from-your-event-hub) com o hub de eventos configurado. 

- Utilize a [Graph API de Relatórios](concept-reporting-api.md) para aceder aos dados e, em seguida, envie-os para o sistema SIEM através dos seus próprios scripts.

---

**P: Que ferramentas SIEM são atualmente suportadas?** 

**R:** Atualmente, o Azure Monitor é suportado pelo [Splunk](tutorial-integrate-activity-logs-with-splunk.md), o QRadar e o [Sumo Logic](https://help.sumologic.com/Send-Data/Applications-and-Other-Data-Sources/Azure_Active_Directory). Para obter mais informações sobre como funcionam os conectores, veja [Stream Azure monitoring data to an event hub for consumption by an external tool](../../monitoring-and-diagnostics/monitor-stream-monitoring-data-event-hubs.md) (Transmitir em fluxo dados de monitorização do Azure para um hub de eventos, para consumo por uma ferramenta externa).

---

**P: Como posso integrar registos de atividades do Azure AD na minha instância do Splunk?**

**R**: Primeiro, [encaminhe os registos de atividade do Azure AD para um hub de eventos](quickstart-azure-monitor-stream-logs-to-event-hub.md) e, em seguida, siga os passos para [Integrar registos de atividade com o Splunk](tutorial-integrate-activity-logs-with-splunk.md).

---

**P: Como posso integrar registos de atividades do Azure AD com o Sumo Logic?** 

**R**: Primeiro, [encaminhe os registos de atividade do Azure AD para um hub de eventos](https://help.sumologic.com/Send-Data/Applications-and-Other-Data-Sources/Azure_Active_Directory/Collect_Logs_for_Azure_Active_Directory) e, em seguida, siga os passos para [Instalar a aplicação do Azure AD e ver os dashboards no SumoLogic](https://help.sumologic.com/Send-Data/Applications-and-Other-Data-Sources/Azure_Active_Directory/Install_the_Azure_Active_Directory_App_and_View_the_Dashboards).

---

**P: Posso aceder aos dados de um hub de eventos sem utilizar uma ferramenta SIEM externa?** 

**R**: Sim. Pode utilizar a [API dos Hub de Eventos](../../event-hubs/event-hubs-dotnet-standard-getstarted-receive-eph.md) para aceder aos registos da sua aplicação personalizada. 

---


## <a name="next-steps"></a>Passos Seguintes

* [Arquivar registos de atividades numa conta de armazenamento](quickstart-azure-monitor-route-logs-to-storage-account.md)
* [Encaminhar registos de atividades para um hub de eventos](quickstart-azure-monitor-stream-logs-to-event-hub.md)
* [Integrar registos de atividade com o Log Analytics](howto-integrate-activity-logs-with-log-analytics.md)
