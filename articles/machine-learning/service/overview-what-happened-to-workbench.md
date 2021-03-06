---
title: O que está a acontecer ao Azure Machine Learning Workbench? | Microsoft Docs
description: Saiba mais sobre o que está a acontecer à aplicação Workbench, o que mudou no Azure Machine Learning e qual é a linha cronológica de suporte.
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: overview
ms.reviewer: jmartens
author: j-martens
ms.author: jmartens
ms.date: 09/24/2018
ms.openlocfilehash: b8263c399f287be79590860cce7036207ef2e3f7
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/07/2018
ms.locfileid: "51243748"
---
# <a name="what-is-happening-to-workbench-in-azure-machine-learning-preview"></a>O que está a acontecer ao Workbench no Azure Machine Learning (pré-visualização)?

A aplicação Workbench e algumas outras funcionalidades anteriores foram substituídas na versão de setembro de 2018, para dar lugar a uma [arquitetura](concept-azure-machine-learning-architecture.md) melhorada. A versão contém várias atualizações importantes baseadas no feedback dos clientes para melhorar a sua experiência. A funcionalidade principal das execuções experimentais para a implementação de modelos não foi alterada, mas agora pode utilizar um <a href="https://aka.ms/aml-sdk" target="_blank">SDK</a> e uma [CLI](reference-azure-machine-learning-cli.md) robustos para concluir tarefas e pipelines de machine learning.  

Neste artigo, ficará a saber o que mudou e como isso afeta o seu trabalho preexistente com o serviço Azure Machine Learning.

## <a name="what-changed"></a>O que mudou?

A versão mais recente do serviço do Azure Machine Learning inclui o seguinte:
+ Um [modelo de recursos do Azure simplificado](concept-azure-machine-learning-architecture.md)
+ [Nova IU do portal](how-to-track-experiments.md) para gerir as suas experimentações e destinos de computação
+ Um novo <a href="https://aka.ms/aml-sdk" target="_blank">SDK</a> de Python mais abrangente
+ Uma nova [extensão da CLI do Azure](reference-azure-machine-learning-cli.md) expandida para machine learning

A [arquitetura](concept-azure-machine-learning-architecture.md) foi redefinida tendo em conta a facilidade de utilização. Em vez de vários recursos e contas do Azure, precisa apenas de uma [Área de Trabalho do serviço do Azure Machine Learning](concept-azure-machine-learning-architecture.md#workspace).  Pode criar áreas de trabalho rapidamente no [portal do Azure](quickstart-get-started.md).  Uma área de trabalho pode ser utilizada por vários utilizadores para armazenar destinos de computação de preparação e implementação, experimentações de modelos, imagens do Docker, modelos implementados, entre outros.

Embora existam novos clientes de CLI e SDK melhorados na versão atual, a própria aplicação Workbench de ambiente de trabalho foi descontinuada. Agora, pode monitorizar as suas experimentações no [dashboard de área de trabalho do portal Web do Azure](how-to-track-experiments.md#view-the-experiment-in-the-azure-portal). Utilize o dashboard para obter o histórico de experimentações, gerir os destinos de computação associados à sua área de trabalho, gerir os seus modelos e imagens do Docker e até implementar serviços Web.

## <a name="how-do-i-migrate"></a>Como posso migrar?

A maioria dos artefactos criados na versão anterior do serviço Azure Machine Learning é armazenada no seu próprio local ou no armazenamento da cloud. Estes artefactos nunca irão desaparecer. Para migrar, terá de registar novamente os artefactos com o serviço Azure Machine Learning atualizado. Saiba o que pode migrar e como o pode fazer neste [artigo sobre a migração](how-to-migrate.md).

<a name="timeline"></a>

## <a name="support-timeline"></a>Linha cronológica de suporte

Pode continuar a utilizar as suas contas de experimentação e gestão de modelos, bem como a aplicação Workbench, durante mais algum tempo depois de setembro de 2018. O suporte para os seguintes recursos será removido progressivamente nos 3 a 4 meses que se seguem após esse lançamento. Pode ainda encontrar a documentação relativa às funcionalidades antigas na [secção Recursos](../desktop-workbench/tutorial-classifying-iris-part-1.md), na parte inferior do índice.

|Fase|Detalhes de suporte para funcionalidades anteriores|
|:---:|----------------|
|1|A capacidade de criar a _conta de Experimentação do Azure Machine Learning_ e a _conta de Gestão de Modelos_ no portal do Azure e a partir da CLI termina. A capacidade de criar Ambientes de Computação do ML a partir da CLI também terminará. Se tiver uma conta existente, a CLI e o Workbench no ambiente de trabalho continuam a funcionar nesta fase.|
|2|O suporte para tudo o resto, incluindo as APIs restantes e o Workbench no ambiente de trabalho, termina nesta fase.|

[Comece a migrar](how-to-migrate.md) hoje mesmo. Todas as mais recentes capacidades estão disponíveis com os novos <a href="https://aka.ms/aml-sdk" target="_blank">SDK</a>, [CLI](reference-azure-machine-learning-cli.md) e [portal](quickstart-get-started.md).

## <a name="what-about-run-histories"></a>E em relação aos históricos de execução?

Os históricos de execução permanecerão acessíveis durante algum tempo. Quando estiver pronto para mudar para a versão atualizada do serviço Azure Machine Learning, poderá exportar estes históricos de execução, caso pretenda guardar uma cópia.

Os históricos de execução são agora denominados _experimentações_ na versão atual. Pode recolher as experimentações do seu modelo e explorá-las com o SDK, a CLI ou o portal Web.

O dashboard de área de trabalho do portal é suportado apenas nos browsers Edge, Chrome e Firefox.

[ ![Portal online](./media/overview-what-happened-to-workbench/image001.png) ] (./media/overview-what-happened-to-workbench/image001.png#lightbox)


## <a name="can-i-still-prep-data"></a>Ainda posso preparar os dados?

Os seus ficheiros de preparação de dados já existentes não são portáteis para a versão mais recente, uma vez que já não disponibilizamos o Workbench. No entanto, pode continuar a preparar os seus dados de modelação.  

Com conjuntos de dados mais pequenos, pode utilizar o <a href="https://aka.ms/aml-sdk" target="_blank">SDK de Preparação de Dados do Azure Machine Learning</a> para preparar rapidamente os seus dados antes da modelação. 

Pode utilizar este mesmo <a href="https://aka.ms/aml-sdk" target="_blank">SDK</a> para conjuntos de dados maiores ou o Azure Databricks para preparar grandes conjuntos de dados. 

## <a name="will-projects-persist"></a>Os projetos serão mantidos?

Não irá perder qualquer código ou trabalho. Na versão mais antiga, os projetos são entidades na cloud com um diretório local. Na versão mais recente, pode associar diretórios locais à Área de Trabalho do serviço do Azure Machine Learning através de um ficheiro de configuração local. [Veja um diagrama da arquitetura mais recente](concept-azure-machine-learning-architecture.md).

Uma vez que grande parte do conteúdo do projeto já se encontrava no seu computador local, basta criar um ficheiro de configuração nesse diretório e referi-lo no código para ligar à sua área de trabalho. [Saiba como migrar os seus projetos existentes.](how-to-migrate.md#projects)

Saiba como começar a utilizar o [Python com o SDK principal](quickstart-get-started.md).

## <a name="what-about-my-registers-models-and-images"></a>E em relação aos meus modelos e imagens de registos?
 
Os modelos que registou no seu registo de modelos antigo têm de ser migrados para a sua nova área de trabalho se pretender continuar a utilizá-los. Pode fazê-lo ao [transferir os modelos e ao registá-los novamente](how-to-migrate.md) na sua nova área de trabalho. 

As imagens que criou no seu registo de imagens antigo têm de ser recriadas na nova área de trabalho para continuar a utilizá-las. Pode fazê-lo ao seguir os passos na secção de [criação da imagem do Docker](how-to-deploy-to-aci.md#configure-an-image). 

## <a name="what-about-deployed-web-services"></a>E em relação aos serviços Web implementados?

Os modelos que implementou como serviços Web com a sua conta de Gestão de Modelos irão continuar a funcionar, desde que o Azure Container Service (ACS) seja suportado. Esses serviços Web irão funcionar inclusivamente após o fim do suporte para contas de Gestão de Modelos. No entanto, quando o suporte para a antiga CLI terminar, terminará também a capacidade para gerir esses serviços Web.

Na versão mais recente, os modelos são implementados como serviços Web nos clusters do [Azure Container Instances](how-to-deploy-to-aci.md) (ACI) ou do [Azure Kubernetes Service](how-to-deploy-to-aks.md) (AKS). Também pode [implementar em FPGAs e no IoT Edge](how-to-deploy-and-where.md). Pode implementar novamente os modelos com o SDK ou a CLI novos, sem ter de alterar qualquer um dos seus ficheiros, dependências e esquemas de classificação. 

## <a name="what-about-the-old-sdk--cli"></a>E em relação ao SDK e à CLI antigos?

Sim, continuarão a funcionar durante algum tempo (veja a [linha cronológica](#timeline) acima). Recomendamos que comece a criar as suas experimentações e modelos novos com o SDK e/ou a CLI mais recentes.

Na versão mais recente, o novo SDK de Python permite-lhe interagir com o serviço Azure Machine Learning em qualquer ambiente de Python. Saiba como instalar a versão o <a href="https://aka.ms/aml-sdk" target="_blank">SDK</a> mais recente.  Também pode utilizar a [extensão de machine learning da CLI do Azure atualizada](reference-azure-machine-learning-cli.md) com o conjunto avançado de `az ml` comandos para interagir com o serviço em qualquer ambiente de linha de comandos, incluindo o Cloud Shell do portal do Azure.

## <a name="what-about-vs-code-tools-for-ai"></a>E em relação ao VS Code Tools for AI?

Com esta versão mais recente, a extensão Visual Studio (VS) Code Tools for AI foi expandida e melhorada para funcionar com os novos recursos acima.

[ ![Visual Studio Code Tools for AI](./media/overview-what-happened-to-workbench/vscode.png) ] (./media/overview-what-happened-to-workbench/vscode-big.png#lightbox)

## <a name="what-about-domain-packages"></a>E em relação aos pacotes de domínios?

Os pacotes de domínios para [Imagem Digitalizada, Análise de Texto e Previsão](../desktop-workbench/reference-python-package-overview.md) não podem ser utilizados com a versão mais recente do Azure Machine Learning. No entanto, pode continuar a criar e preparar os modelos de imagem digitalizada, texto e previsão com o <a href="https://aka.ms/aml-sdk" target="_blank">SDK</a> de Python do Azure Machine Learning mais recente. Para saber como migrar os modelos já existentes criados com os pacotes de Imagem Digitalizada, Análise de Texto e Previsão, contacte-nos através do endereço de e-mail [AML-Packages@microsoft.com](mailto:AML-Packages@microsoft.com).

## <a name="next-steps"></a>Passos seguintes

Saiba mais sobre [a arquitetura mais recente do serviço Azure Machine Learning](concept-azure-machine-learning-architecture.md) e experimente um dos guias de início rápido ou tutoriais:

* [O que é o serviço Azure Machine Learning?](overview-what-is-azure-ml.md)
* [Guia de Início Rápido: criar uma área de trabalho com Python](quickstart-get-started.md)
* [Tutorial: preparar um modelo](tutorial-train-models-with-aml.md)
