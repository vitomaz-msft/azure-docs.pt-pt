---
title: Uma aplicação de Proxy da aplicação demora demasiado tempo a carregar | Documentos da Microsoft
description: Resolução de problemas de desempenho de carregamento de página com o Proxy de aplicações do Azure AD
services: active-directory
documentationcenter: ''
author: barbkess
manager: mtillman
ms.assetid: ''
ms.service: active-directory
ms.component: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 07/11/2017
ms.author: barbkess
ms.reviewer: asteen
ms.openlocfilehash: 0c18647fbfc5e328276eef248f4b6aa1dc7f48a2
ms.sourcegitcommit: af9cb4c4d9aaa1fbe4901af4fc3e49ef2c4e8d5e
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/11/2018
ms.locfileid: "44356981"
---
# <a name="an-application-proxy-application-takes-too-long-to-load"></a>Uma aplicação de Proxy da aplicação demora demasiado tempo a carregar

Este artigo ajuda-o a compreender por que um aplicativo de Proxy de aplicações do Azure AD pode demorar muito tempo a carregar. Também explica o que pode fazer para resolver este problema.

## <a name="overview"></a>Descrição geral
Embora as aplicações estão a funcionar, eles podem assistir a uma longa latência. Pode haver ajustes de topologia de rede que pode fazer para melhorar a velocidade. Para uma edição de avaliação das topologias diferentes, consulte a [documento de considerações de rede](application-proxy-network-topology.md).

Além da topologia de rede, atualmente não existem outras recomendações de otimização de desempenho. Como o Proxy de aplicações, serviço expande-lo pode ser para um centro de dados que está fisicamente mais próximo. A proximidade com mais detalhes pode ajudar a latência. Para obter uma lista dos centros de dados do Azure, consulte a [página de teste de latência](http://www.azurespeed.com/Azure/Latency). 

Os centros de dados com o serviço de Proxy de aplicações podem ser encontrados com o [ferramenta de teste de portas de conector](https://aadap-portcheck.connectorporttest.msappproxy.net/). 

## <a name="feedback-on-application-proxy-data-center-locations"></a>Comentários sobre localizações de Datacenter Proxy de aplicações 
Pode haver centros de dados do Azure que não, mas incluem o Proxy de aplicações, mas poderia levar a uma melhoria de latência excelentes para. Enviar o Datacenter é a seguinte localização para aadapfeedback@microsoft.com. A Microsoft utiliza os seus comentários para planos de expansão.

A Microsoft está trabalhando em capacidades adicionais para melhorar a latência. Assim que esses aprimoramentos estão disponíveis, a documentação será atualizada.

## <a name="next-steps"></a>Passos Seguintes
[Trabalhar com servidores de proxy no local existentes](application-proxy-configure-connectors-with-proxy-servers.md)
