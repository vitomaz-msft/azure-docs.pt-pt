---
title: Descrição geral do Projeto de DevOps do Azure| Microsoft Docs
description: Compreender o valor para o Projeto de DevOps do Azure
services: devops-project
documentationcenter: ''
author: mlearned
manager: douge
editor: mlearned
ms.assetid: ''
ms.service: devops-project
ms.devlang: na
ms.topic: overview
ms.tgt_pltfrm: na
ms.workload: ''
ms.date: 05/03/2018
ms.author: mlearned
ms.openlocfilehash: 39dffad597b8382dea4df6fa1b0726d9582d67d1
ms.sourcegitcommit: f3bd5c17a3a189f144008faf1acb9fabc5bc9ab7
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/10/2018
ms.locfileid: "44293631"
---
# <a name="overview-of-azure-devops-project"></a>Descrição geral do Projeto de DevOps do Azure

O Projeto de DevOps do Azure permite começar a utilizar o Azure mais facilmente. O recurso do DevOps Project ajuda a iniciar o seu tipo de aplicação favorito no serviço do Azure à sua escolha em apenas alguns passos rápidos a partir do portal do Azure. O Projeto DevOps configura tudo o que precisa para desenvolver, implementar e monitorizar a sua aplicação.
O dashboard do Projeto de DevOps permite-lhe monitorizar consolidações, compilações e implementações de código a partir de uma única vista no portal do Azure.

## <a name="why-should-i-use-the-azure-devops-project"></a>Por que motivo devo utilizar o Projeto de DevOps do Azure?

O Projeto de DevOps do Azure automatiza a configuração de um pipeline completo de Integração Contínua (CI) e Entrega Contínua (CD) para o Azure.  Pode começar com o código existente ou utilizar um dos exemplos de aplicação fornecidos e, em seguida, implementar rapidamente essa aplicação nos vários serviços do Azure, tais como Máquinas Virtuais, Serviço de Aplicações, Azure Container Service, Base de Dados SQL do Azure e Azure Service Fabric.  

O Azure DevOps Project realiza todo o trabalho para a configuração inicial de um pipeline de DevOps, incluindo a configuração do repositório de Git inicial, a configuração do pipeline de CI/CD, a criação de um novo recurso do Application Insights para monitorização e o fornecimento de uma vista única da solução completa com a criação de um dashboard do Azure DevOps Project no portal do Azure.

Pode utilizar o Projeto de DevOps do Azure para:

* Implementar rapidamente a sua aplicação no Azure
* Automatizar a configuração de um pipeline CI/CD do Azure
* Utilizar o DevOps Project como modelo para ver e compreender como configurar corretamente a CI/CD para o Azure com o Azure DevOps
* Iniciar o pipeline CI/CD para o Azure e personalizar ainda mais o pipeline da versão com base nos seus cenários específicos

## <a name="how-do-i-use-the-azure-devops-project"></a>Como posso utilizar o Projeto de DevOps do Azure?

O Projeto de DevOps do Azure está disponível no portal do Azure.  Aqui, pode criar um recurso do Azure DevOps Project da mesma forma que qualquer outro recurso do Azure.  A experiência passo a passo do DevOps Project é semelhante à de um assistente para as várias opções de configuração.  

Pode escolher várias opções de configuração como parte da configuração inicial.  As opções incluem:

* Utilizar o exemplo de aplicação fornecido ou utilizar o seu próprio código
* Selecionar um idioma da aplicação
* Escolher uma estrutura de aplicação baseada no idioma
* Selecionar um serviço do Azure (destino da implementação)
* Organização do Azure DevOps (nova ou existente)
* Escolher a sua subscrição do Azure
* Escolher a localização de serviços do Azure
* Escolher entre vários escalões de preço para os serviços do Azure

Depois de utilizar o Projeto de DevOps do Azure, pode também eliminar todos os recursos de um único local a partir do dashboard do Projeto de DevOps do Azure no portal do Azure.

## <a name="azure-devops-project-and-azure-devops-integration"></a>Azure DevOps Project e integração do Azure DevOps

Os Projetos de DevOps têm tecnologia Azure DevOps.  O DevOps Project automatiza todo o trabalho necessário no Azure DevOps para configurar a CI/CD para o Azure.  É criado um repositório de Git numa organização do Azure DevOps nova ou existente.  O DevOps Project consolida um exemplo de aplicação ou o seu código existente para um novo repositório de Git.  A automatização estabelece também um acionador de CI para a compilação, de forma a que cada consolidação de código novo inicie uma compilação.  O DevOps Project cria também um acionador de CD e implementará todas as compilações novas com êxito no serviço do Azure à sua escolha.  Os pipelines de compilação e versão podem ser personalizados para mais cenários.  Também pode clonar os pipelines de compilação e versão para os utilizar noutros projetos.

Depois de criar o seu Projeto de DevOps, pode:

* Personalizar a sua compilação e pipeline da versão
* Utilizar os pedidos Pull para gerir o seu fluxo de códigos e manter uma qualidade elevada
* Testar e compilar cada consolidação antes de unir o seu código para aumentar a qualidade
* Controlar o registo de tarefas pendentes e problemas do seu Projeto com a sua aplicação

## <a name="how-do-i-start-using-the-azure-devops-project"></a>Como posso começar a utilizar o Projeto de DevOps do Azure?

* [Introdução ao Projeto de DevOps do Azure](https://docs.microsoft.com/azure/devops-project/azure-devops-project-github)

## <a name="azure-devops-project-videos"></a>Vídeos do Projeto de DevOps do Azure

* [Criar CI/CD com o Projeto de DevOps do Azure](https://channel9.msdn.com/Events/Connect/2017/T174/player/)
