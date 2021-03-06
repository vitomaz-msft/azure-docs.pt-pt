---
title: Conjuntos de agentes do DC/OS para o Azure Container Service
description: Como os conjuntos de agentes públicos e privados funcionam com um cluster DC/OS do Azure Container Service
services: container-service
author: iainfoulds
manager: jeconnoc
ms.service: container-service
ms.topic: article
ms.date: 01/04/2017
ms.author: iainfou
ms.custom: mvc
ms.openlocfilehash: 17029f51be9fed8fc36c5f919ece84acbf0461d9
ms.sourcegitcommit: a2ae233e20e670e2f9e6b75e83253bd301f5067c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/13/2018
ms.locfileid: "42060887"
---
# <a name="dcos-agent-pools-for-azure-container-service"></a>Conjuntos de agentes do DC/OS para o Azure Container Service
Clusters de DC/OS no Azure Container Service contém nós de agente em dois conjuntos, um conjunto público e um pool privado. Um aplicativo pode ser implementado para o conjunto, que afetam a acessibilidade entre as máquinas no seu serviço de contentor. As máquinas podem ser expostas à internet (público) ou mantidas interno (privado). Este artigo fornece uma breve descrição geral de por que há agrupamentos públicos e privados.


* **Agentes privados**: os nós de agente privado executados através de uma rede não encaminháveis internos. Esta rede só é acessível da zona de administrador ou através de router de limite de zona pública. Por predefinição, o DC/OS inicia aplicações em nós de agente privado. 

* **Agentes públicos**: nós de agente público executar aplicações de DC/OS e serviços através de uma rede acessível ao público. 

Para obter mais informações sobre a segurança de rede do DC/OS, consulte a [documentação de DC/OS](https://dcos.io/docs/1.8/administration/securing-your-cluster/).

## <a name="deploy-agent-pools"></a>Implementar conjuntos de agentes

Os conjuntos de agentes do DC/OS no serviço de contentor do Azure são criados da seguinte forma:

* O **pool privado** contém o número de nós de agente que especifica quando [implementar o cluster DC/OS](container-service-deployment.md). 

* O **conjunto público** inicialmente contém um número pré-determinado de nós de agente. Este agrupamento é adicionado automaticamente quando o cluster DC/OS é aprovisionado.

O pool privado e o conjunto público são conjuntos de dimensionamento de máquina virtual do Azure. Pode redimensionar esses agrupamentos após a implementação.

## <a name="use-agent-pools"></a>Utilizar conjuntos de agentes
Por predefinição, **Marathon** implementa qualquer novo aplicativo para o *privada* nós de agente. Tem de implementar explicitamente o aplicativo para o *público* nós durante a criação do aplicativo. Selecione o **opcional** separador e introduza **slave_public** para o **funções dos recursos aceites** valor. Esse processo está documentado [aqui](container-service-mesos-marathon-ui.md#deploy-a-docker-formatted-container) e, no [DC/OS](https://dcos.io/docs/1.7/administration/installing/custom/create-public-agent/) documentação.

## <a name="next-steps"></a>Passos Seguintes
* Leia mais sobre [gerir os seus contentores do DC/OS](container-service-mesos-marathon-ui.md).

* Saiba como [abrir a firewall](container-service-enable-public-access.md) fornecido pelo Azure para permitir o acesso público para os contentores do DC/OS.

