---
title: Proteger serviços de aplicações no Centro de segurança do Azure | Documentos da Microsoft
description: Este artigo ajuda-o a começar a proteger os seus serviços de aplicação no Centro de segurança do Azure.
services: security-center
documentationcenter: na
author: rkarlin
manager: mbaldwin
editor: ''
ms.assetid: e8518710-fcf9-44a8-ae4b-8200dfcded1a
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/28/2018
ms.author: rkarlin
ms.openlocfilehash: 062d3ce75372cd09e617fb984208542a31cb8e4a
ms.sourcegitcommit: 00dd50f9528ff6a049a3c5f4abb2f691bf0b355a
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/05/2018
ms.locfileid: "51019250"
---
# <a name="protect-app-service-with-azure-security-center"></a>Proteger o serviço de aplicações com o Centro de segurança do Azure
Este artigo ajuda-o a utilizar o Centro de segurança do Azure para monitorizar e proteger as suas aplicações em execução com base no serviço de aplicações.

Serviço de aplicações permite-lhe criar e alojar aplicações web na linguagem de programação da sua preferência sem ter de gerir infraestrutura. Serviço de aplicações oferece dimensionamento automático e elevada disponibilidade, suporta Windows e Linux, os e implementações automáticas a partir do GitHub, Visual Studio Team Services ou qualquer repositório de Git. 

Vulnerabilidades em aplicativos web com freqüência são exploradas pelos atacantes, porque eles têm uma interface comum e dinâmica para quase todas as organizações na Internet. Pedidos de aplicações em execução com base no serviço de aplicações vai através de vários gateways implementados nos datacenters do Azure em todo o mundo, responsável pelo encaminhamento de cada pedido para seu aplicativo correspondente. 

Centro de segurança do Azure pode executar avaliações e as recomendações nas suas aplicações em execução no serviço de aplicações em áreas de segurança na sua VM ou instâncias a pedido. Ao tirar partido da visibilidade que o Azure tem como fornecedor de cloud, o Centro de segurança analisa os registos de internos do serviço de aplicações para monitorizar ataques de aplicação web comuns, que, muitas vezes, ser executados em vários destinos.

Centro de segurança tira partido da escala da cloud para identificar ataques nas suas aplicações de serviço de aplicações e concentre-se sobre ataques emergentes, enquanto os atacantes estão em fase de reconhecimento, análise para identificar vulnerabilidades em vários sites, alojados no Azure. Centro de segurança utiliza análises e modelos de machine learning para cobrir todas as interfaces que permite aos clientes interagirem com seus aplicativos, se por HTTP ou por meio de métodos de gestão. Além disso, como um serviço original no Azure, o Centro de segurança é também numa posição única para oferecer a análise de segurança baseada em anfitrião que abrange os nós de computação subjacente para esta PaaS, permitir que o Centro de segurança detetar ataques contra aplicações web que foram já exploradas.

## <a name="prerequisites"></a>Pré-requisitos

Para monitorizar e proteger o seu serviço de aplicações, tem de ter um plano de serviço de aplicações que estão associado com máquinas dedicadas. Estes planos são: Basic, Standard, Premium, Isolated ou Linux. Centro de segurança do Azure não suporta os planos gratuito, partilhado ou consumo. Para obter mais informações, consulte [planos do serviço de aplicações](https://azure.microsoft.com/pricing/details/app-service/plans/).

## <a name="security-center-protection"></a>Proteção de centro de segurança

Centro de segurança do Azure protege a instância VM em que o serviço de aplicações está em execução e a interface de gestão. Ele também monitora os pedidos e respostas enviadas de e para as suas aplicações em execução no serviço de aplicações.

Centro de segurança nativamente está integrado com o serviço de aplicações, eliminando a necessidade de implementação e integração - a integração é completamente transparente.



## <a name="enabling-monitoring-and-protection-of-app-service"></a>Ativar a monitorização e a proteção do serviço de aplicações

1. No Azure, escolha o Centro de segurança.
2. Aceda a **política de segurança** e escolha uma subscrição.
3. No final da linha da subscrição, clique em **editar as definições de**.
4. Sob **escalão de preço**, na **serviço de aplicações** da linha, alternar o seu plano para **ativado**.

![Ativar/desativar de serviço de aplicações](./media/security-center-app-services/app-services-toggle.png)

>[!NOTE]
> O número de instâncias listado para a quantidade de recurso representa o número de instâncias relevantes do Active Directory no momento quando aberto o painel de escalão de preços do serviço de aplicações. Uma vez que este número pode mudar com base nas opções de dimensionamento que selecionou, o número de instâncias, que é-lhe cobrada em conformidade será modificado.

Para desativar a monitorização e recomendações para o serviço de aplicações, repita este processo e um botão de alternar sua **serviço de aplicações** planeja **desativado**.



## <a name="see-also"></a>Consulte também
Neste artigo, aprendeu a utilizar as capacidades de monitorização no Centro de Segurança do Azure. Para saber mais acerca do Centro de Segurança do Azure, consulte o seguinte:

* [Definir políticas de segurança no Centro de Segurança do Azure](security-center-policies.md): saiba como configurar definições de segurança no Centro de Segurança do Azure.
* [Gerir e responder a alertas de segurança no Centro de Segurança do Azure](security-center-managing-and-responding-alerts.md): saiba como gerir e responder a alertas de segurança.
* [Monitorizar soluções de parceiros com o Centro de Segurança do Azure](security-center-partner-solutions.md): saiba como monitorizar o estado de funcionamento das suas soluções de parceiros.
* [Azure Security Center FAQ (FAQ do Centro de Segurança do Azure)](security-center-faq.md): encontre as perguntas mais frequentes acerca de como utilizar o serviço.
* [Blogue de Segurança do Azure](http://blogs.msdn.com/b/azuresecurity/): encontre publicações no blogue acerca da segurança e conformidade do Azure.
