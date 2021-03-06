---
title: Exportar as suas informações de nível superior de subscrição do Azure | Documentos da Microsoft
description: Descreve como pode ver todos os IDs associados à conta de subscrição do Azure.
keywords: ''
services: billing
documentationcenter: ''
author: adpick
manager: adpick
editor: ''
tags: billing
ms.service: billing
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 06/15/2018
ms.author: cwatson
ms.openlocfilehash: 5c32b90c8a291ff744b4894af12f8d623cb95137
ms.sourcegitcommit: d1aef670b97061507dc1343450211a2042b01641
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/27/2018
ms.locfileid: "47391409"
---
# <a name="export-and-view-your-top-level-subscription-information"></a>Exportar e veja as suas informações de subscrição de nível superior
Se precisar de ver o conjunto de IDs associados com as suas credenciais de utilizador, de subscrição [transferir um ficheiro. JSON com informações da sua subscrição a partir do Centro de contas do Azure](http://account.azure.com/subscriptions/download).

[!INCLUDE [gdpr-dsr-and-stp-note](../../includes/gdpr-dsr-and-stp-note.md)]

O ficheiro. JSON transferido fornece as seguintes informações:
- E-mail: O endereço de e-mail associado à sua conta.
- PUID: O identificador exclusivo associado à sua conta de cobrança.
- SubscriptionIds: Uma lista de subscrições que pertencem à sua conta, enumerada pelo ID da subscrição.

### <a name="subscriptionsjson-sample"></a>exemplo de subscriptions.JSON
~~~~
{
  "Email":"admin@contoso.com",
  "Puid":"00052xxxxxxxxxxx",
  "SubscriptionIds":[
    "38124d4d-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
    "7c8308f1-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
    "39a25f2b-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
    "52ec2489-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
    "e42384b2-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
    "90757cdc-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
  ]
}
~~~~
