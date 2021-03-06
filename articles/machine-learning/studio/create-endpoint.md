---
title: Criação de pontos finais de serviço da Web no Machine Learning | Documentos da Microsoft
description: Criação de pontos finais de serviço da Web no Azure Machine Learning
services: machine-learning
documentationcenter: ''
author: ericlicoding
ms.custom: (previous ms.author=yahajiza, author=YasinMSFT)
ms.author: amlstudiodocs
manager: hjerez
editor: cgronlun
ms.assetid: 4657fc1b-5228-4950-a29e-bc709259f728
ms.service: machine-learning
ms.component: studio
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: tbd
ms.date: 10/04/2016
ms.openlocfilehash: f046db11bf3c04c9ea15e759b4e0080cab4f71d5
ms.sourcegitcommit: fa758779501c8a11d98f8cacb15a3cc76e9d38ae
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/20/2018
ms.locfileid: "52265071"
---
# <a name="creating-endpoints"></a>Criação de pontos finais
> [!NOTE]
>  Este tópico descreve técnicas aplicáveis a uma **clássico** serviço Web do Machine Learning.
> 
> 

Ao criar serviços Web que venda para a frente aos seus clientes, terá de fornecer modelos qualificados a cada cliente que ainda estejam ligadas à experimentação partir do qual o serviço Web foi criado. Além disso, todas as atualizações para a experimentação devem ser aplicadas seletivamente a um ponto final sem substituir as personalizações.

Para tal, o Azure Machine Learning permite-lhe criar vários pontos de extremidade para um serviço Web implementado. Cada ponto de extremidade no serviço Web independente é resolvido, limitado e gerido. Cada ponto de extremidade é um URL exclusivo e uma chave de autorização que pode distribuir aos seus clientes.

[!INCLUDE [machine-learning-free-trial](../../../includes/machine-learning-free-trial.md)]

## <a name="adding-endpoints-to-a-web-service"></a>Adicionar pontos finais para um serviço Web
Existem duas formas de adicionar um ponto final a um serviço Web.

* Através de programação
* Através do portal do Azure Machine Learning Web Services

Assim que o ponto final for criado, pode consumi-lo através de APIs síncronas, APIs, do batch e folhas de cálculo do excel. Além de adicionar pontos finais através essa interface do Usuário, também pode utilizar as APIs de gestão do ponto final para adicionar programaticamente os pontos finais.

> [!NOTE]
> Se tiver adicionado pontos finais adicionais para o serviço Web, não é possível eliminar o ponto final predefinido.
> 
> 

## <a name="adding-an-endpoint-programmatically"></a>Adicionar um ponto de extremidade por meio de programação
Pode adicionar um ponto de extremidade ao seu serviço de Web por meio de programação com o [AddEndpoint](https://github.com/raymondlaghaeian/AML_EndpointMgmt/blob/master/Program.cs) código de exemplo.

## <a name="adding-an-endpoint-using-the-azure-machine-learning-web-services-portal"></a>Adicionar um ponto de extremidade através do portal do Azure Machine Learning Web Services
1. No Machine Learning Studio, na coluna de navegação esquerda, clique em serviços da Web.
2. Na parte inferior do dashboard do serviço Web, clique em **gerir pontos finais**. O portal de serviços da Web do Azure Machine Learning abre-se para a página de pontos de extremidade para o serviço Web.
3. Clique em **Novo**.
4. Escreva um nome e descrição para o novo ponto final. Nomes de ponto final tem de ser 24 carateres ou menos de comprimento e devem ser constituídos por letras do alfabeto em minúsculas ou números. Selecione o nível de registo e se os dados de exemplo estão ativados. Para obter mais informações sobre o registo, consulte [ativar o registo para os serviços Web do Machine Learning](web-services-logging.md).

## <a name="next-steps"></a>Passos Seguintes
[Como consumir um serviço Web do Azure Machine Learning](consume-web-services.md).

