---
title: Como configurar a monitorização no duplos Digital do Azure | Documentos da Microsoft
description: Como configurar a monitorização no duplos Digital do Azure
author: kingdomofends
manager: alinast
ms.service: digital-twins
services: digital-twins
ms.topic: conceptual
ms.date: 10/22/2018
ms.author: adgera
ms.openlocfilehash: 1c8f1931a29ae9769f7d8ad57a184e3240105a1a
ms.sourcegitcommit: 9e179a577533ab3b2c0c7a4899ae13a7a0d5252b
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/23/2018
ms.locfileid: "49945826"
---
# <a name="how-to-configure-monitoring-in-azure-digital-twins"></a>Como configurar a monitorização no duplos Digital do Azure

Duplos Digital do Azure suporta registo robusto, monitorização e análise. Os desenvolvedores de soluções podem utilizar o Azure Log Analytics, os registos de diagnóstico, registo de atividade e outros serviços para suportar as necessidades complexas de monitorização de uma aplicação IoT. Opções de registo podem ser combinadas para consultar ou apresentar registos em vários serviços e fornecer cobertura de Registro em log granular para muitos serviços.

Este artigo resume o registo e monitorização de opções e como combiná-los de formas específicas para Twins Digital do Azure.

## <a name="review-activity-logs"></a>Registos de atividades de revisão

Azure [registos de atividades](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs) fornecem informações rápidas sobre históricos de evento e a operação de nível de assinatura para cada instância de serviço do Azure.

Eventos de nível de assinatura incluem:

* Criação de recursos
* Remoção de recursos
* Criação de segredos de aplicação
* Integrar com outros serviços

Registo de atividade duplos Digital do Azure está ativado por predefinição e pode ser encontrado através do portal do Azure ao:

1. Selecionar a instância de duplos Digital do Azure.
1. Escolher **registo de atividades** para abrir o painel de exibição:

    ![Registo de atividades][1]

Para o registo de atividade avançadas:

1. Selecione o **Logs** opção para apresentar o **descrição geral da análise de registo de atividade**:

    ![Seleção][2]

1. O **descrição geral da análise de registo de atividade** resume os dados de registo de atividade essencial:

    ![Descrição geral da análise de registo de atividade][3]

>[!TIP]
>Uso **registos de atividades** para as informações rápidas sobre os eventos de nível de assinatura.

## <a name="enable-customer-diagnostic-logs"></a>Ativar registos de diagnóstico do cliente

Azure [das definições de diagnóstico](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs) podem ser definidos para cada instância do Azure complementar o registo de atividade. Enquanto os registos de atividades que dizem respeito a eventos de nível de assinatura, o registo de diagnósticos fornece informações sobre o histórico de operacional dos próprios recursos.

Exemplos de registo de diagnósticos:

* O tempo de execução para uma função definida pelo utilizador
* O código de estado de resposta de um pedido de API com êxito
* Obter uma chave de aplicação a partir de um cofre

Para ativar os registos de diagnóstico para uma instância:

1. Abra o recurso no portal do Azure.
1. Clique em **das definições de diagnóstico**:

    ![Definições de diagnóstico um][4]

1. Clique em **ativar os diagnósticos** para recolher dados (se ainda não ativada).
1. Preencha os campos de pedido e selecione como e onde os dados serão guardados:

    ![Duas definições de diagnóstico][5]

    Os registos de diagnóstico são, muitas vezes, salvo usando [File Storage do Azure](https://docs.microsoft.com/azure/storage/files/storage-files-deployment-guide) e partilhado com [Azure Log Analytics](https://docs.microsoft.com/azure/log-analytics/query-language/get-started-analytics-portal). Ambas as opções podem ser selecionadas.

>[!TIP]
>Uso **registos de diagnóstico** para informações sobre operações de recursos.

## <a name="azure-monitor-and-log-analytics"></a>Análise de registo e monitor do Azure

Aplicações de IoT unam-se diferentes recursos, dispositivos, locais e dados em um. Registro em log detalhado fornece informações detalhadas sobre cada parte específica, o serviço ou o componente da arquitetura geral do aplicativo, mas uma visão unificada, muitas vezes, é necessária para manutenção e a depuração.

Monitor do Azure inclui o serviço de Log Analytics poderoso, que permite que o registo origens sejam visualizados e analisados num único local. O Azure Monitor, por conseguinte, é extremamente útil para analisar os registos dentro sofisticados aplicações de IoT.

Exemplos de utilização incluem:

* Consultar vários históricos de registo de diagnóstico
* Ver registos para várias funções definidas pelo utilizador
* Exibindo logs para dois ou mais serviços dentro de um período de tempo específico

Consultar o registo completo é fornecido através de [do Azure Log Analytics](https://docs.microsoft.com/azure/log-analytics/log-analytics-queries). Para configurar estas funcionalidades poderosas:

1. Procure **do Log Analytics** no portal do Azure.
1. Verá sua disponíveis **do Log Analytics** instâncias. Escolha uma e selecione **registos** a consulta:

    ![Log analytics][6]

1. Se ainda não tiver uma **do Log Analytics** instância, pode criar uma área de trabalho ao clicar o **Add** botão:

    ![Criar o OMS][7]

Uma vez sua **do Log Analytics** instância é aprovisionada, pode utilizar consultas eficientes para encontrar entradas nos registos de múltiplos ou a procura utilizando critérios específicos usando **gestão de registos**:

   ![Gestão de registos][8]

Para obter mais informações sobre as operações de consulta poderosa, consulte [introdução às consultas](https://docs.microsoft.com/azure/log-analytics/query-language/get-started-queries).

> [!NOTE]
> Poderá haver um atraso de 5 minutos, ao enviar eventos para **do Log Analytics** pela primeira vez.

O Azure Log Analytics também fornece o erro poderosas e serviços de notificação de alerta, que podem ser visualizados clicando **diagnosticar e resolver problemas**:

   ![Notificações de alerta e de erro][9]

>[!TIP]
>Uso **do Log Analytics** para históricos de log de consulta para as várias funcionalidades de aplicação, subscrições ou serviços.

## <a name="other-options"></a>Outras opções

Duplos Digital do Azure também suporta o registo específico de aplicação e auditoria de segurança. Para obter uma descrição geral completa de todas as opções de registo do Azure disponíveis para a sua instância de duplos Digital do Azure, consulte a [auditoria de registos do Azure](https://docs.microsoft.com/azure/security/azure-log-audit) artigo.

## <a name="next-steps"></a>Passos Seguintes

Saiba mais sobre o Azure [registos de atividades](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs).

Aprofunde-se em definições de diagnóstico do Azure, lendo um [descrição geral dos registos de diagnóstico](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs).

Leia mais sobre [do Azure Log Analytics](https://docs.microsoft.com/azure/log-analytics/query-language/get-started-analytics-portal).

<!-- Images -->
[1]: media/how-to-configure-monitoring/activity-log.png
[2]: media/how-to-configure-monitoring/activity-log-select.png
[3]: media/how-to-configure-monitoring/log-analytics-overview.png
[4]: media/how-to-configure-monitoring/diagnostic-settings-one.png
[5]: media/how-to-configure-monitoring/diagnostic-settings-two.png
[6]: media/how-to-configure-monitoring/log-analytics.png
[7]: media/how-to-configure-monitoring/log-analytics-oms.png
[8]: media/how-to-configure-monitoring/log-analytics-management.png
[9]: media/how-to-configure-monitoring/log-analytics-notifications.png