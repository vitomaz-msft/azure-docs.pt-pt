---
title: Proteger a pontuação no Centro de segurança do Azure | Documentos da Microsoft
description: " Priorize suas recomendações de segurança com a classificação de segurança no Centro de segurança do Azure. "
services: security-center
documentationcenter: na
author: rkarlin
manager: MBaldwin
editor: ''
ms.assetid: c42d02e4-201d-4a95-8527-253af903a5c6
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/5/2018
ms.author: rkarlin
ms.openlocfilehash: 3a377441758fcd7dd91deefb5cae91579e881498
ms.sourcegitcommit: 00dd50f9528ff6a049a3c5f4abb2f691bf0b355a
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/05/2018
ms.locfileid: "51007062"
---
# <a name="improve-your-secure-score-in-azure-security-center"></a>Melhorar a sua pontuação segura no Centro de segurança do Azure


Com tantos serviços oferecem benefícios de segurança, geralmente é difícil saber que passos a efetuar primeiro para proteger e proteger a sua carga de trabalho. A classificação de segurança do Azure analisa as suas recomendações de segurança e estabelece uma prioridade para si, para saber quais as recomendações para executar primeiro. Isto ajuda a localizar as vulnerabilidades de segurança mais sérias, portanto, pode priorizar a investigação. Pontuação segura é uma ferramenta que ajuda a avaliar a sua postura de segurança da carga de trabalho.

![Proteger o dashboard de pontuação](./media/security-center-secure-score/secure-score-dashboard.png)

## <a name="secure-score-calculation"></a>Proteger o cálculo de pontuação

Centro de segurança imita o trabalho de um analista de segurança, revisão de suas recomendações de segurança e a aplicar algoritmos avançados para determinar como crucial é de cada recomendação.
Centro de segurança do Azure constantemente revisões de recomendações ativas e calcula a sua pontuação segura com base nos mesmos, a pontuação de uma recomendação é derivada de sua gravidade e melhores práticas de segurança que irão afetar a segurança de sua carga de trabalho mais.

Centro de segurança também lhe fornecem uma **secure geral pontuação**. 

**Proteger a pontuação geral** é uma acumulação de todas as suas pontuações de recomendação. Pode visualizar sua pontuação geral segura nas suas subscrições ou grupos de gestão, consoante o que selecione. A pontuação varia com base na subscrição selecionada e as Active Directory recomendações sobre estas subscrições.

 
Para verificar quais recomendações afetam sua pontuação segura mais, pode ver as recomendações de maior impacto de três principais no dashboard do Centro de segurança ou pode classificar as recomendações no recomendações lista painel com a **pontuação segura impacto** coluna.


Para ver a sua pontuação geral segura:

1. No dashboard do Azure, clique em **Centro de segurança** e, em seguida, clique em **recomendações**.
2. Na parte superior, pode ver a classificação segura, que representa a classificação de acordo com as políticas, por subscrição selecionada. 
2. Na tabela abaixo que lista as recomendações, pode ver que cada recomendação existe uma coluna que representa a **proteger o impacto de pontuação**. Este número representa irá melhorar sua pontuação geral segura quanto se seguir as recomendações. Por exemplo, no ecrã abaixo, se **remediar vulnerabilidades em configurações de segurança do contentor**, sua pontuação segura sofrerão um aumento até 35 pontos.

   ![pontuação segura](./media/security-center-secure-score/security-center-secure-score1.png)

## <a name="individual-secure-score"></a>Pontuação de segura individuais

Além disso, para ver as pontuações de seguras individuais, pode encontrá-los dentro do painel de recomendação individuais.  

O **recomendação secure pontuação** é um cálculo com base no rácio entre os recursos de bom estado de funcionamento e o total de recursos. Se o número de recursos de bom estado de funcionamento é igual ao número total de recursos, receberá a pontuação máxima segura da recomendação de 50 caracteres. Para tentar obter a sua pontuação segura mais próximo para a classificação máxima, corrigi os recursos de mau estado de funcionamento ao seguir as recomendações.

O **impacto de recomendação** permite-lhe determinar quanto melhora a sua pontuação segura se aplicar os passos de recomendação. Por exemplo, se sua pontuação segura é 42 e a **impacto de recomendação** é +3, efetuando os passos descritos na recomendação melhorar sua pontuação para se tornar 45.

A recomendação mostra quais ameaças sua carga de trabalho é exposta a se os passos de remediação não são executados.

![pontuação de seguro de recomendação individuais](./media/security-center-secure-score/indiv-recommendation-secure-score.png)

## <a name="next-steps"></a>Passos Seguintes
Este artigo mostrou como melhorar a sua postura de segurança utilizando **pontuação segura** no Centro de segurança do Azure. Para saber mais sobre o Centro de segurança, veja:

* [FAQ do Centro de Segurança do Azure](security-center-faq.md) – Encontre as perguntas mais frequentes acerca de como utilizar o serviço.
* [Monitorização do estado de funcionamento de segurança no Centro de Segurança do Azure](security-center-monitoring.md) – Saiba como monitorizar o estado de funcionamento dos seus recursos do Azure.


