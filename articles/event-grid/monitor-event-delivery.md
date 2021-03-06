---
title: Monitorizar a entrega de mensagens de grelha de eventos do Azure
description: Descreve como monitorizar a entrega de mensagens de grelha de eventos do Azure.
services: event-grid
author: tfitzmac
manager: timlt
ms.service: event-grid
ms.topic: conceptual
ms.date: 05/24/2018
ms.author: tomfitz
ms.openlocfilehash: 625f3e228bb28c85e68fb592914fb2191baf3e4e
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/01/2018
ms.locfileid: "34626994"
---
# <a name="monitor-event-grid-message-delivery"></a>Monitorizar a entrega de mensagens de evento grelha 

Este artigo descreve como utilizar o portal para ver o estado de entregas de eventos.

Grelha de evento fornece entrega durável. Fornece cada mensagem, pelo menos, uma vez para cada subscrição. Os eventos são enviados para o webhook registado de cada subscrição imediatamente. Se um webhook não reconhecer a receção de um evento dentro de 60 segundos, da primeira tentativa de entrega, eventos grelha tentativas de entrega do evento.

Para obter informações sobre a entrega de eventos e de tentativas, [entrega de mensagens de evento grelha e volte a tentar](delivery-and-retry.md).

## <a name="delivery-metrics"></a>Métricas de entrega

O portal apresenta métricas para o estado de entrega de mensagens de evento.

Tópicos, as métricas são:

* **Publicar com êxito**: evento com êxito enviadas para o tópico e processada com uma resposta de 2xx.
* **Publicar falha**: evento enviadas para o tópico, mas rejeitou com um código de erro.
* **Sem correspondência**: evento publicada com êxito para o tópico, mas não corresponde a uma subscrição de evento. O evento foi ignorado.

Para as subscrições, as métricas são:

* **Com êxito de entrega**: evento com êxito entregue ao ponto final da subscrição e recebeu uma resposta de 2xx.
* **Falha de entrega**: eventos enviados para o ponto final da subscrição, mas recebeu uma resposta 4xx ou 5xx.
* **Expirado eventos**: não foi entregue evento e foram enviadas todas as tentativas de repetição. O evento foi ignorado.
* **Correspondência eventos**: evento no tópico foi correspondido pela subscrição de evento.

## <a name="event-subscription-status"></a>Estado de subscrição de evento

Para ver as métricas para uma subscrição de evento, pode optar por procurar por tipo de subscrição ou subscrições para um recurso específico.

Para procurar por tipo de subscrição de evento, selecione **todos os serviços**.

![Selecione todos os serviços](./media/monitor-event-delivery/all-services.png)

Procurar **grelha de evento** e selecione **eventos grelha subscrições** entre as opções disponíveis.

![Pesquisa para subscrições de eventos](./media/monitor-event-delivery/search-and-select.png)

Filtrar por tipo de evento, a subscrição e localização. Selecione **métricas** para a subscrição ver.

![Subscrições de eventos de filtro](./media/monitor-event-delivery/filter-events.png)

Ver as métricas para o tópico de eventos e uma subscrição.

![Veja as métricas de eventos](./media/monitor-event-delivery/subscription-metrics.png)

Para localizar as métricas de um recurso específico, selecione esse recurso. Em seguida, selecione **eventos**.

![Selecione os eventos para um recurso](./media/monitor-event-delivery/select-events.png)

Pode ver as métricas para subscrições para esse recurso.

## <a name="custom-event-status"></a>Estado do evento personalizado

Se tiver publicado um tópico personalizado, pode ver as métricas para o mesmo. Selecione o grupo de recursos para o tópico e selecione o tópico.

![Selecione o tópico personalizado](./media/monitor-event-delivery/select-custom-topic.png)

Ver as métricas para o tópico de evento personalizado.

![Veja as métricas de eventos](./media/monitor-event-delivery/custom-topic-metrics.png)

## <a name="next-steps"></a>Passos Seguintes

* Para obter informações sobre a entrega de eventos e de tentativas, [entrega de mensagens de evento grelha e volte a tentar](delivery-and-retry.md).
* Para obter uma introdução ao Event Grid, veja [Sobre o Azure Event Grid](overview.md).
* Para rapidamente começar a utilizar a grelha de eventos, consulte o artigo [criar e rota eventos personalizados com o Azure eventos grelha](custom-event-quickstart.md).
