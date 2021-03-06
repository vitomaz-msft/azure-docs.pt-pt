---
title: Configurar alertas de estado de funcionamento do serviço do Azure com OpsGenie | Documentos da Microsoft
description: Obtenha notificações personalizadas sobre eventos de estado de funcionamento de serviço à sua instância de OpsGenie.
author: shawntabrizi
manager: scotthit
editor: ''
services: service-health
documentationcenter: service-health
ms.assetid: ''
ms.service: service-health
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/14/2017
ms.author: shtabriz
ms.openlocfilehash: 6e9d44c4b64e41b04c40d0c511a638e539752da4
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/02/2018
ms.locfileid: "39435031"
---
# <a name="configure-service-health-alerts-with-opsgenie"></a>Configurar alertas de estado de funcionamento do serviço com OpsGenie

Este artigo mostra-lhe como configurar alertas de estado de funcionamento do serviço do Azure com OpsGenie através de um webhook. Usando [OpsGenie](https://www.opsgenie.com/)da integração de estado de funcionamento de serviço do Azure, pode reencaminhar os alertas do Azure Service Health para OpsGenie. OpsGenie pode determinar as pessoas certas para notificar com base em agendas na chamada, usando o email, mensagens de texto (SMS), chamadas telefónicas, iOS e notificações Android push e escalonar alertas até que o alerta seja confirmado ou fechado.

## <a name="creating-a-service-health-integration-url-in-opsgenie"></a>Criar um serviço de integração do Estado de funcionamento URL no OpsGenie
1.  Certifique-se de que se inscreveram cópia de segurança e iniciado sessão na sua [OpsGenie](https://www.opsgenie.com/) conta.

1.  Navegue para o **integrações** secção OpsGenie.

    ![A secção "Integrações" OpsGenie](./media/webhook-alerts/opsgenie-integrations-section.png)

1.  Selecione o **do Azure Service Health** botão de integração.

    !["O Azure Service Health botão" no OpsGenie](./media/webhook-alerts/opsgenie-azureservicehealth-button.png)

1.  **Nome** seu alerta e especifique o **atribuídos à equipe** campo.

1.  Preencher os outros campos, como **destinatários**, **ativado**, e **suprimir notificações**.

1.  Copie e guarde o **URL de integração**, que já deve conter o `apiKey` acrescentado ao fim.

    ![O "URL de integração" na OpsGenie](./media/webhook-alerts/opsgenie-integration-url.png)

1.  Selecione **guardar integração**

## <a name="create-an-alert-using-opsgenie-in-the-azure-portal"></a>Criar um alerta com OpsGenie no portal do Azure
### <a name="for-a-new-action-group"></a>Para um novo grupo de ação:
1. Siga os passos 1 a 8 na [criar um alerta numa notificação de estado de funcionamento do serviço para um novo grupo de ação com o portal do Azure](../monitoring-and-diagnostics/monitoring-activity-log-alerts-on-service-notifications.md).

1. Definir na lista de **ações**:

    a. **Tipo de ação:** *Webhook*

    b. **Detalhes:** o OpsGenie **URL de integração** tinha guardado.

    c. **Nome:** do Webhook nome, o alias ou o identificador.

1. Selecione **guardar** quando terminar de criar o alerta.

### <a name="for-an-existing-action-group"></a>Para um grupo de ação existente:
1. Na [portal do Azure](https://portal.azure.com/), selecione **Monitor**.

1. Na **configurações** secção, selecione **grupos de ação**.

1. Localize e selecione o grupo de ação que pretende editar.

1. Adicionar à lista de **ações**:

    a. **Tipo de ação:** *Webhook*

    b. **Detalhes:** o OpsGenie **URL de integração** tinha guardado.

    c. **Nome:** do Webhook nome, o alias ou o identificador.

1. Selecione **guardar** quando terminar de atualizar o grupo de ação.

## <a name="testing-your-webhook-integration-via-an-http-post-request"></a>Teste a sua integração de webhook através de um pedido POST de HTTP
1. Crie o payload de estado de funcionamento do serviço que pretende enviar. Pode encontrar um payload de webhook do Estado de funcionamento do serviço do exemplo na [alertas de registo de Webhooks para atividades do Azure](../monitoring-and-diagnostics/monitoring-activity-log-alerts-webhook.md).

1. Crie um pedido POST de HTTP da seguinte forma:

    ```
    POST        https://api.opsgenie.com/v1/json/azureservicehealth?apiKey=<APIKEY>

    HEADERS     Content-Type: application/json

    BODY        <service health payload>
    ```
1. Deverá receber um `200 OK` resposta com a mensagem de estado "com êxito".

1. Aceda a [OpsGenie](https://www.opsgenie.com/) para confirmar a sua integração foi configurada com êxito.

## <a name="next-steps"></a>Passos Seguintes
- Saiba como [configurar notificações de webhook para sistemas de gestão existentes do problema](service-health-alert-webhook-guide.md).
- Reveja os [esquema de webhook de alerta de registo de atividades](../monitoring-and-diagnostics/monitoring-activity-log-alerts-webhook.md). 
- Saiba mais sobre [notificações de estado de funcionamento de serviço](../monitoring-and-diagnostics/monitoring-service-notifications.md).
- Saiba mais sobre [grupos de ação](../monitoring-and-diagnostics/monitoring-action-groups.md).