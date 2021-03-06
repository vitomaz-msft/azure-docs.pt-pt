---
title: Adicionar intenções em aplicativos de LUIS
titleSuffix: Azure Cognitive Services
description: Adicione intenções à sua aplicação LUIS para identificar os grupos de perguntas ou comandos que têm as mesmo intenções.
services: cognitive-services
author: diberry
manager: cgronlun
ms.component: language-understanding
ms.topic: article
ms.date: 10/24/2018
ms.author: diberry
ms.service: cognitive-services
ms.openlocfilehash: 495b7e99319126b3ee9e655b2d9aa4af940e1d56
ms.sourcegitcommit: 0f54b9dbcf82346417ad69cbef266bc7804a5f0e
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/26/2018
ms.locfileid: "50139926"
---
# <a name="add-intents"></a>Adicionar intenções 

Adicione [intenções](luis-concept-intent.md) à sua aplicação LUIS para identificar os grupos de perguntas ou comandos que têm a mesma intenção. 

Objetivos são geridos a partir da barra de navegação superior **crie** secção, em seguida, a partir do painel esquerdo **intenções**. 

## <a name="create-an-app"></a>Criar uma aplicação

1. Inicie sessão para o [LUIS](https://www.luis.ai) portal.

1. Selecione **Create new app** (Criar nova aplicação). 

1. Nomeie a nova aplicação `MyHumanResourcesApp`. Selecione o **inglês** cultura. A descrição é opcional. 

1. Selecione **Done** (Concluído). 

## <a name="add-intent"></a>Adicionar intenções

1. A aplicação abre-se para o **intenções** lista.

1. Na página **Intents** (Intenções), selecione **Create new intent** (Criar nova intenção).

1. Na **criar novo intenção** caixa de diálogo, introduza o nome da intenção, `GetEmployeeInformation`e clique em **feito**.

    ![Adicionar intenções](./media/luis-how-to-add-intents/Addintent-dialogbox.png)

## <a name="add-an-example-utterance"></a>Adicionar uma expressão de exemplo

Expressões com de exemplo são exemplos de texto de perguntas de utilizador ou de comandos. Ensinar a compreensão de idiomas (LUIS), terá de adicionar expressões de exemplo para um objetivo.

1. Sobre o **GetEmployeeInformation** detalhes da intenção página, introduza uma expressão relevante que se espera de seus usuários, como `Does John Smith work in Seattle?` na caixa de texto abaixo do nome intenção e, em seguida, prima Enter.
 
    ![Página de detalhes de captura de ecrã de objetivos, com a expressão realçado](./media/luis-how-to-add-intents/add-new-utterance-to-intent.png) 

    LUIS converte todas as expressões de com em minúsculas e adiciona os espaços em torno de tokens, como hífens.

## <a name="intent-prediction-discrepancy-errors"></a>Erros de discrepância de predição de intenção 

Uma expressão numa intenção, pode ter uma discrepância de predição de intenção entre a intenção de selecionado e a pontuação de predição. LUIS indica essa discrepância com uma caixa vermelha à volta da **rotulado intenção** na linha da expressão de exemplo. 

![Página de detalhes de captura de ecrã de objetivos, com a expressão realçado](./media/luis-how-to-add-intents/prediction-discrepancy-intent.png) 

No painel de navegação superior, selecione **Train**. A discrepância de predição é agora já era.

## <a name="add-a-custom-entity"></a>Adicionar uma entidade personalizada

Depois de uma expressão é adicionado a uma intenção, pode selecionar o texto da dentro da expressão para criar uma entidade personalizada. Uma entidade personalizada é uma forma de texto da etiqueta de extração, juntamente com a intenção correta. 

1. Selecione o word, `Seattle`, em que a expressão. Colchetes são desenhados em todo o texto e é apresentado um menu de lista pendente. 

    ![Página de detalhes de captura de ecrã de objetivos, a criar a entidade personalizada](./media/luis-how-to-add-intents/create-custom-entity.png) 

    Neste exemplo seleciona uma única palavra para marcar como uma entidade. Pode marcar funciona única e frases como entidades.

1. Superior-caixa de texto do menu, introduza `Location`, em seguida, selecione **criar nova entidade**. 

    ![Página de detalhes de captura de ecrã de objetivos, a criação de nome de entidade personalizada](./media/luis-how-to-add-intents/create-custom-entity-name.png) 

1. Na **o tipo de entidade que pretende criar?** janela pop-up para a criação de entidade, validar que o **nome da entidade** é _localização_e o **tipo de entidade**  é _simples_. Selecione **Done** (Concluído).

## <a name="entity-prediction-discrepancy-errors"></a>Erros de discrepância de predição de entidade 

A entidade está sublinhada em vermelho para indicar uma [discrepância de predição de entidade](luis-how-to-add-example-utterances.md#entity-status-predictions). Uma vez que esta é a primeira ocorrência de uma entidade, não há suficiente exemplos para LUIS ter uma confiança elevada que este texto é marcado com a entidade correta. Essa discrepância é removida quando a aplicação é preparada. 

![Página de detalhes de captura de ecrã de objetivos, nome de entidade personalizada realçado em azul](./media/luis-how-to-add-intents/create-custom-entity-name-blue-highlight.png) 

O texto é realçado em azul, indicando uma entidade.  

## <a name="add-a-prebuilt-entity"></a>Adicionar uma entidade pré-criados

Para obter informações, consulte [criados previamente entidade](luis-how-to-add-entities.md#add-prebuilt-entity).

## <a name="using-the-contextual-toolbar"></a>Usando a barra de ferramentas contextual

Quando um ou mais expressões de exemplo é selecionado na lista, marcando a caixa à esquerda de uma expressão, a barra de ferramentas acima da lista de expressão permite-lhe executar as seguintes ações:

* Reatribuir intenção: mover utterance(s) em intenção diferente
* Eliminar utterance(s)
* Filtros de entidade: Mostrar apenas as expressões que contém entidades filtradas
* Mostrar tudo / apenas erros: Mostrar expressões com erros de predição ou mostrar todas as expressões de com
* Vista de entidades/Tokens: Mostrar a exibição de entidades com nomes de entidade ou mostrar o texto não processado de expressão
* Lupa: procure expressões com contendo texto específico

## <a name="working-with-an-individual-utterance"></a>Trabalhar com uma expressão individual

As seguintes ações podem ser executadas numa expressão individual no menu de reticências à direita da expressão:

* Editar: alterar o texto da expressão
* Eliminação: remova a expressão a intenção. Se pretender continuar a expressão, um método melhor é para movê-lo para o **None** intenção. 
* Adicionar um padrão: um padrão permite-lhe tirar uma expressão comum e marcar texto substituível e texto ignorable, reduzindo assim a necessidade de expressões com mais na intenção. 

O **rotulado intenção** coluna permite-lhe alterar o objetivo da expressão.

## <a name="train-your-app-after-changing-model-with-intents"></a>Preparar a sua aplicação depois de alterar o modelo com objetivos

Depois de adicionar, editar ou remover intenções, [treinar](luis-how-to-train.md) e [publicar](luis-how-to-publish-app.md) seu aplicativo para que as suas alterações são aplicadas às consultas de ponto final. 

## <a name="next-steps"></a>Passos Seguintes

Saiba mais sobre como adicionar [expressões de exemplo](luis-how-to-add-example-utterances.md) com entidades. 
