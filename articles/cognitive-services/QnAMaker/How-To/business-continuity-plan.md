---
title: Plano de continuidade do negócio - QnA Maker
titleSuffix: Azure Cognitive Services
description: O objetivo principal do plano de continuidade de negócio é criar um ponto de final de base de dados de conhecimento resiliente, qual seria a garantir que nenhum período de indisponibilidade para o Bot ou a aplicação esteja a consumir.
services: cognitive-services
author: tulasim88
manager: cgronlun
ms.service: cognitive-services
ms.component: qna-maker
ms.topic: article
ms.date: 09/12/2018
ms.author: tulasim
ms.openlocfilehash: 41e7425a2e2e6dd8dc8416538cf77e5b8f273284
ms.sourcegitcommit: 4ecc62198f299fc215c49e38bca81f7eb62cdef3
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/24/2018
ms.locfileid: "47041943"
---
# <a name="create-a-business-continuity-plan-for-your-qna-maker-service"></a>Criar um plano de continuidade de negócio para o serviço QnA Maker

O objetivo principal do plano de continuidade de negócio é criar um ponto de final de base de dados de conhecimento resiliente, qual seria a garantir que nenhum período de indisponibilidade para o Bot ou a aplicação esteja a consumir.

![Plano de bcp do QnA Maker](../media/qnamaker-how-to-bcp-plan/qnamaker-bcp-plan.png)

A idéia de alto nível como ilustrado acima é o seguinte:

1. Configurar dois paralelo [serviços do QnA Maker](../How-To/set-up-qnamaker-service-azure.md) na [regiões emparelhadas do Azure](https://docs.microsoft.com/azure/best-practices-availability-paired-regions).

2. Manter os índices primários e secundários do Azure search em sincronia. Utilize o exemplo do github [aqui](https://github.com/pchoudhari/QnAMakerBackupRestore) para ver como cópia de segurança-restauro índices do Azure.

3. Cópia de segurança com o Application Insights [exportação contínua](https://docs.microsoft.com/azure/application-insights/app-insights-export-telemetry).

4. Assim que tem configuradas as pilhas de primárias e secundárias, utilize [Gestor de tráfego](https://docs.microsoft.com/azure/traffic-manager/) para configurar dois pontos de extremidade e configurar um método de encaminhamento.

5. Terá de criar um certificado SSL para o ponto de final do Gestor de tráfego. [Vincular o certificado SSL](https://docs.microsoft.com/azure/app-service/app-service-web-tutorial-custom-ssl) nos serviços de aplicação.

6. Por último, utilize o ponto de final do Gestor de tráfego na sua aplicação ou Bot.

## <a name="next-steps"></a>Passos Seguintes

> [!div class="nextstepaction"]
> [Escolha a capacidade para a sua implementação do QnA Maker](../Tutorials/choosing-capacity-qnamaker-deployment.md)
