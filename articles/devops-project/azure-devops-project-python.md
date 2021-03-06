---
title: Criar um pipeline de CI/CD para a linguagem Python com o Projeto de DevOps do Azure | Início Rápido
description: O Projeto de DevOps permite começar a utilizar o Azure facilmente. Ajuda-o a utilizar o seu próprio código e o repositório GitHub para lançar uma aplicação num serviço do Azure à sua escolha com alguns passos rápidos.
ms.prod: devops
ms.technology: devops-cicd
services: vsts
documentationcenter: vs-devops-build
author: mlearned
manager: douge
editor: ''
ms.assetid: ''
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: quickstart
ms.date: 07/09/2018
ms.author: mlearned
ms.custom: mvc
monikerRange: vsts
ms.openlocfilehash: 46f65772ed4cb70b80674ae39629f52694be49e1
ms.sourcegitcommit: b7e5bbbabc21df9fe93b4c18cc825920a0ab6fab
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/27/2018
ms.locfileid: "47405654"
---
# <a name="create-a-cicd-pipeline-for-python-with-the-azure-devops-project"></a>Criar um pipeline de CI/CD para a linguagem Python com o Projeto de DevOps do Azure

O Projeto de DevOps do Azure apresenta uma experiência simplificada que cria os recursos do Azure e configura um pipeline de integração contínua (CI) e de entrega contínua (CD) para a sua aplicação Python nos Serviços DevOps do Azure.  

Se não tiver uma subscrição do Azure, pode obter uma subscrição gratuita através do [Visual Studio Dev Essentials](https://visualstudio.microsoft.com/dev-essentials/).

## <a name="sign-in-to-the-azure-portal"></a>Iniciar sessão no portal do Azure

O Projeto de DevOps do Azure cria um pipeline de CI/CD no Azure.  Pode criar uma organização **nova dos Serviços DevOps do Azure** gratuita ou utilizar uma **organização existente**.  O projeto de DevOps também cria **recursos do Azure** na **subscrição do Azure** que escolher.

1. Inicie sessão no [portal do Microsoft Azure](https://portal.azure.com).

1. Escolha o ícone **Criar um recurso** na barra de navegação esquerda e, em seguida, procure **Projeto de DevOps**.  Escolha **Criar**.

    ![Iniciar a configuração da Entrega Contínua](_img/azure-devops-project-python/fullbrowser.png)

## <a name="select-a-sample-application-and-azure-service"></a>Selecione um exemplo de aplicação e serviço do Azure

1. Selecione o exemplo de aplicação **Python**.  Os exemplos Python incluem várias opções de arquiteturas de aplicações.

1. A arquitetura de exemplo predefinida é **Django**. Deixe a predefinição e escolha **Seguinte**.  

1. **A Aplicação Web Para Contentores** é o destino de implementação predefinido.  A arquitetura de aplicações que escolheu nos passos anteriores dita o tipo de destino da implementação do serviço do Azure disponível aqui.  Deixe o serviço predefinido e, em seguida, escolha **Seguinte**.
 
## <a name="configure-azure-devops-services-and-an-azure-subscription"></a>Configurar os Serviços DevOps do Azure e uma subscrição do Azure 

1. Crie uma organização **nova** dos Serviços DevOps do Azure ou utilize uma organização **existente**.  Escolha um **nome** para o seu projeto de DevOps do Azure.  Selecione a sua **subscrição do Azure**, a **localização** e escolha um **nome** para a sua aplicação.  Quando tiver terminado, escolha **Concluído**.

1. Dentro de alguns minutos, o **dashboard do projeto** é carregado no portal do Azure.  Um exemplo de aplicação é configurado num repositório da sua organização de Serviços DevOps, uma compilação é executada e a sua aplicação é implementada no Azure.  Este dashboard oferece visibilidade para o **repositório de código**, o **pipeline CI/CD do Azure** e a sua **aplicação em execução no Azure**.  No lado direito do dashboard, selecione **Procurar** para ver a sua aplicação em execução.

    ![Vista do Dashboard](_img/azure-devops-project-python/dashboardnopreview.png) 
    
O Projeto de DevOps do Azure configura automaticamente um acionador de lançamento e de compilação de CI.  Agora, está pronto para colaborar com uma equipa na sua aplicação Python, com um processo de CI/CD que implementa automaticamente o seu trabalho mais recente no seu site.

## <a name="commit-code-changes-and-execute-cicd"></a>Consolidar as alterações de código e executar o CI/CD

O projeto de DevOps do Azure criou um repositório Git na sua organização de Serviços DevOps do Azure ou conta GitHub.  Siga os passos abaixo para ver o repositório e fazer alterações de código na aplicação.

1. No lado esquerdo do dashboard do Projeto de DevOps, selecione a ligação para o seu ramo **principal**.  Esta ligação abre uma vista para o repositório Git recentemente criado.

1. Para ver o URL de clone do repositório, selecione **Clone** na parte superior direita do browser. Pode clonar o repositório Git no seu IDE preferido.  Nos próximos passos, pode utilizar o browser para fazer e consolidar alterações de código diretamente no ramo principal.

1. No lado esquerdo do browser, navegue para o ficheiro **app/templates/app/index.html**.

1. Selecione **Editar** e faça uma alteração a algumas partes do texto.  Por exemplo, altere algum texto para uma das etiquetas div.

1. Escolha **Consolidar** e guarde as alterações.

1. No browser, navegue para o **dashboard do Projeto de DevOps do Azure**.  Deverá agora ver que está em curso uma compilação.  As alterações que acabou de fazer são automaticamente criadas e implementadas através de um pipeline de CI/CD do Azure.

## <a name="examine-the-azure-cicd-pipeline"></a>Examinar o pipeline de CI/CD do Azure

O Projeto de DevOps do Azure configurou automaticamente um pipeline de CI/CD completo do Azure na sua organização dos Serviços DevOps do Azure.  Explore e personalize o pipeline, conforme necessário.  Siga os passos abaixo para se familiarizar com os pipelines de compilação e de lançamento dos Serviços DevOps do Azure.

1. Selecione **Pipelines de Compilação** na **parte superior** do dashboard do Projeto de DevOps do Azure.  Esta ligação abre um separador do browser e abre o pipeline de compilação dos Serviços DevOps do Azure para o novo projeto.

1. Mova o cursor do rato para o lado direito do pipeline de compilação junto ao campo **Estado**. Selecione as **reticências** que são apresentadas.  Esta ação abre um menu onde pode iniciar várias atividades, como colocar uma nova compilação em fila, colocar uma compilação em pausa e editar o pipeline de compilação.

1. Selecione **Editar**.

1. Nesta vista, **examine as várias tarefas** do pipeline de compilação.  A compilação executa várias tarefas, como obter as origens do repositório Git do VSTS, restaurar dependências e publicar saídas utilizadas para implementações.

1. Na parte superior do pipeline de compilação, selecione o **nome do pipeline de compilação**.

1. Altere o **nome** do pipeline de compilação para algo mais descritivo.  Selecione **Guardar e colocar em fila** e, em seguida, selecione **Guardar**.

1. No nome do pipeline de compilação, selecione **Histórico**.  Verá um registo de auditoria das alterações recentes da compilação.  Os Serviços DevOps do Azure mantêm um registo de todas as alterações efetuadas no pipeline de compilação e permitem-lhe comparar versões.

1. Selecione **Acionadores**.  O Projeto de DevOps do Azure criou automaticamente um acionador de CI e cada consolidação no repositório inicia uma nova compilação.  Opcionalmente, pode optar por incluir ou excluir os ramos do processo de CI.

1. Selecione **Retenção**.  Com base no seu cenário, pode especificar políticas para manter ou remover um determinado número de compilações.

1. Selecione **Compilação e Versão** e, em seguida, escolha **Versões**.  O Projeto de DevOps do Azure criou um pipeline de lançamento dos Serviços DevOps do Azure para gerir as implementações no Azure.

1. No lado esquerdo do browser, selecione as **reticências** junto ao pipeline de lançamento e, em seguida, escolha **Editar**.

1. O pipeline de lançamento contém um **pipeline**, que define o processo de lançamento.  Em **Artefactos**, selecione **Remover**.  O pipeline de compilação que examinou nos passos anteriores produz a saída utilizada para o artefacto. 

1. No lado direito do ícone **Remover**, selecione o **Acionador de implementação contínua**.  Este pipeline de lançamento tem um acionador de CD ativado, que executa uma implementação sempre que estiver disponível um novo artefacto de compilação.  Opcionalmente, pode desativar o acionador, para que as suas implementações requeiram execução manual. 

1. No lado esquerdo do browser, selecione **Tarefas**.  As tarefas são as atividades que o processo de implementação executa.  Neste exemplo, foi criada uma tarefa para implementar no **Serviço de Aplicações do Azure**.

1. No lado direito do browser, selecione **Ver versões**.  Esta vista mostra um histórico das versões.

1. Selecione as **reticências** junto a uma das versões e escolha **Abrir**.  Estão disponíveis vários menus para explorar nesta vista, como um resumo de versões, itens de trabalho associados e testes.

1. Selecione **Consolidações**.  Esta vista mostra as consolidações de código associadas à implementação específica. 

1. Selecionar **Registos**.  Os registos contêm informações úteis sobre o processo de implementação.  Podem ser vistos durante e após as implementações.

## <a name="clean-up-resources"></a>Limpar recursos

Quando já não for necessário, pode eliminar o serviço de Aplicações do Azure e os recursos relacionados que criou neste início rápido, utilizando a funcionalidade **Eliminar** do dashboard do projeto de DevOps do Azure.

## <a name="next-steps"></a>Passos seguintes

Quando configurou o processo de CI/CD neste início rápido, um pipeline de compilação e um pipeline de lançamento foram automaticamente criados no Projeto de DevOps do Azure. Pode modificar estes pipelines de compilação e de lançamento para satisfazer as necessidades da sua equipa. Para saber mais, veja este tutorial:

> [!div class="nextstepaction"]
> [Personalizar o processo de CD](https://docs.microsoft.com/azure/devops/pipelines/release/define-multistage-release-process?view=vsts)
