---
title: Diagnosticar e resolver problemas se desliga hub IoT do Azure
description: Saiba como diagnosticar e resolver erros comuns com a conectividade do dispositivo para o IoT Hub do Azure
author: jlian
manager: briz
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 07/19/2018
ms.author: jlian
ms.openlocfilehash: 5bd66e3cb3902665aab9245a524a2bec6f57dc8c
ms.sourcegitcommit: 4ea0cea46d8b607acd7d128e1fd4a23454aa43ee
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/15/2018
ms.locfileid: "42056956"
---
# <a name="detect-and-troubleshoot-disconnects-with-azure-iot-hub"></a>Detetar e resolver problemas se desliga hub IoT do Azure

Problemas de conectividade para dispositivos de IoT podem ser difíceis de resolver porque existem muitas possíveis pontos de falha. Lógica de aplicação do lado do dispositivo, redes físicas, protocolos, hardware e IoT Hub do Azure todos podem causar problemas. Este documento fornece recomendações sobre como detetar e resolver problemas de conectividade do dispositivo do lado da nuvem (ao contrário do lado do dispositivo).

## <a name="get-alerts-and-error-logs"></a>Obter alertas e registos de erros

Utilize o Azure Monitor para receber alertas e registos de escrita quando a lista de ligações de dispositivos.

### <a name="turn-on-diagnostic-logs"></a>Ativar registos de diagnóstico 

Para iniciar sessão erros e eventos de ligação de dispositivos, ative os diagnósticos para o IoT Hub. 

1. Inicie sessão no [portal do Azure](https://portal.azure.com).
1. Navegue até ao seu hub IoT.
1. Selecione **as definições de diagnóstico**.
1. Em seguida, selecione **ativar os diagnósticos**.
1. Certifique-se de que ative **ligações** registos serão recolhidos. 
1. Para facilitar a análise, deve ativar **enviar para o Log Analytics** ([Ver preços](https://azure.microsoft.com/pricing/details/log-analytics/)). Um exemplo posteriormente neste artigo usa o Log Analytics.

   ![Definições recomendadas][2]

Para obter mais informações, consulte [monitorizar o estado de funcionamento do IoT Hub do Azure e diagnosticar problemas rapidamente](iot-hub-monitor-resource-health.md).

### <a name="set-up-alerts-for-the-connected-devices-count-metric"></a>Configurar alertas para a métrica de contagem de dispositivos ligados

Para obter alertas quando desligar de dispositivos, configurar alertas na *dispositivos ligados* métrica. 

1. Inicie sessão no [portal do Azure](https://portal.azure.com).
1. Navegue até ao seu Hub IoT.
1. Selecione **alertas (clássico)**.
1. Clique em **Adicionar alerta de métrica (clássico)**.
1. Preencha o formulário e selecione **OK**. 

   ![Alerta de métrica recomendada][3]

Para obter mais informações, consulte [o que são alertas Clássicos no Microsoft Azure?](../monitoring-and-diagnostics/monitoring-overview-alerts.md).

## <a name="resolve-connectivity-errors"></a>Resolver erros de conectividade

Quando os registos de diagnóstico e alertas para dispositivos conectados estão ativadas, obtenha alertas quando algo corre mal. Esta secção descreve como resolver problemas comuns quando receber um alerta. Os passos abaixo partem do princípio de que configurou o Log Analytics para os seus registos de diagnóstico. 

1. Procurar a sua área de trabalho **do Log Analytics** no portal do Azure.
1. Clique em **pesquisa de registos**.
1. Para isolar os registos de erros de conectividade para o IoT Hub, introduza esta consulta na caixa, em seguida, prima **executar**.

    ```
    search *
    | where ( Type == "AzureDiagnostics" and ResourceType == "IOTHUBS")
    | where ( Category == "Connections" and Level == "Error")
    ```

1. Se existirem resultados, procure o `OperationName`, `ResultType` (código de erro), e `ResultDescription` (mensagem de erro) para obter mais detalhes sobre o erro.

   ![Exemplo de registo de erros][4]

1. Utilize esta tabela para compreender e resolver erros comuns.

    | Erro | Causa raiz | Resolução |
    |---------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
    | 404104 DeviceConnectionClosedRemotely | A ligação foi fechada pelo dispositivo, mas o IoT Hub não sabe por quê. Causas comuns incluem MQTT/AMQP perda de conectividade de tempo limite e internet. | Certifique-se de que o dispositivo consegue ligar ao IoT Hub por [a testar a ligação](tutorial-connectivity.md). Se a ligação está tudo bem, mas o dispositivo se desliga intermitentemente, certifique-se implementar a lógica de dispositivo ativo keep adequada da sua preferência de protocolo (MQTT/AMPQ). |
    | 401003 IoTHubUnauthorized | IoT Hub não foi possível autenticar a ligação. | Certifique-se de que a SAS ou outro token de segurança que utiliza não está expirado. [Os SDKs IoT do Azure](iot-hub-devguide-sdks.md) automaticamente gerar tokens sem a necessidade de configuração especial. |
    | 409002 LinkCreationConflict | Há mais do que um ligações para o mesmo dispositivo. Quando chega um novo pedido de ligação para um dispositivo, o IoT Hub fecha aquele anterior com este erro. | No caso mais comum, um dispositivo Deteta um desentendimento e tenta restabelecer a ligação, mas o IoT Hub não tenha considerado que ainda desligado, para que ele fecha a ligação anterior e registos de erro. Portanto, este erro, normalmente, é apresentado como um efeito colateral de um problema transitório diferente, procure outros erros nos registos para resolver problemas ainda mais. Caso contrário, certifique-se emitir um novo pedido de ligação apenas se a conexão caiu. |
    | 500001 ServerError | IoT Hub Ocorreu um problema do lado do servidor. Provavelmente, o problema é transitório. Embora o IoT Hub equipe trabalha difícil de manter [o SLA](https://azure.microsoft.com/support/legal/sla/iot-hub/), pequenos subconjuntos de nós do IoT Hub, ocasionalmente, poderão experienciar falhas transitórias. Quando o dispositivo tenta ligar a um nó que está a ter problemas, recebe este erro. | Para atenuar as falhas transitórias, emita uma repetição do dispositivo. Para [automaticamente gerir várias repetições](iot-hub-reliability-features-in-sdks.md#connection-and-retry), certifique-se de que utiliza a versão mais recente da [Azure IoT SDKs](iot-hub-devguide-sdks.md).<br><br>Para melhor prática no processamento de falhas transitórias e repetições, consulte [processamento de falhas transitórias](/azure/architecture/best-practices/transient-faults).  <br><br>Se o problema persistir após as repetições, verifique [Resource Health](iot-hub-monitor-resource-health.md#use-azure-resource-health) e [estado do Azure](https://azure.microsoft.com/status/history/) para ver se o IoT Hub tem um problema conhecido. Se não há nenhum problema conhecido e o problema persistir, [contacte o suporte](https://azure.microsoft.com/support/options/) para uma investigação mais aprofundada. |
    | 500008 GenericTimeout | IoT Hub não foi possível concluir o pedido de ligação que o tempo limite. Como 500001 ServerError, este erro é transitório provável. | Siga os passos de resolução de problemas para ServerError 500001 a causa raiz e resolver este erro.|

## <a name="other-steps-to-try"></a>Outros passos para experimentar

Se os passos acima não o ajudaram, aqui estão mais algumas coisas a experimentar:

* Se tiver acesso aos dispositivos problemáticos, física ou remotamente, (como SSH), siga os [guia de resolução de problemas do lado do dispositivo](https://github.com/Azure/azure-iot-sdk-node/wiki/Troubleshooting-Guide-Devices) para continuar a resolução de problemas.
* Certifique-se de que os seus dispositivos estão **ativado** no portal do Azure > seu IoT Hub > dispositivos IoT.
* Obtenha a ajuda dos [fórum do IoT Hub do Azure](https://social.msdn.microsoft.com/Forums/azure/home?forum=azureiothub), [Stack Overflow](https://stackoverflow.com/questions/tagged/azure-iot-hub), ou [suporte do Azure](https://azure.microsoft.com/support/options/).

Para ajudar a melhorar a documentação para todos os utilizadores, deixe um comentário abaixo se este guia não o ajudaram.

## <a name="next-steps"></a>Passos Seguintes

* Para saber mais sobre a resolução de problemas transitórios, veja [processamento de falhas transitórias](/azure/architecture/best-practices/transient-faults).
* Para saber mais sobre o SDK do Azure IoT e gerir várias repetições, consulte [como gerir a conectividade e o sistema de mensagens confiável através de SDKs de dispositivo do IoT Hub do Azure](iot-hub-reliability-features-in-sdks.md#connection-and-retry).

<!-- Images -->
[1]: ../../includes/media/iot-hub-diagnostics-settings/turnondiagnostics.png
[2]: ./media/iot-hub-troubleshoot-connectivity/diagnostic-settings-recommendation.png
[3]: ./media/iot-hub-troubleshoot-connectivity/metric-alert.png
[4]: ./media/iot-hub-troubleshoot-connectivity/diag-logs.png
