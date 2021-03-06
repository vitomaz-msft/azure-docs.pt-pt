---
title: Apresenta uma lista de frase para melhorar a deteção de entidade
titleSuffix: Azure Cognitive Services
description: Utilizar a compreensão de idiomas (LUIS) para adicionar funcionalidades de aplicação que podem melhorar a deteção ou a predição de objetivos e entidades que categorias e padrões
services: cognitive-services
author: diberry
manager: cgronlun
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 09/06/2018
ms.author: diberry
ms.openlocfilehash: 32ef8ba2f6416e1b59fc98595f1b204e94bd2ead
ms.sourcegitcommit: 26cc9a1feb03a00d92da6f022d34940192ef2c42
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/06/2018
ms.locfileid: "48830995"
---
# <a name="use-phrase-lists-to-boost-signal-of-word-list"></a>Apresenta uma lista de frase de utilização para o sinal de aumento de lista de palavras

Pode adicionar funcionalidades à sua aplicação LUIS para melhorar a precisão. Recursos ajudam o LUIS, fornecendo as sugestões que determinadas palavras e frases fazem parte de um vocabulário de domínio de aplicação. 

## <a name="add-phrase-list"></a>Adicionar a lista de frase

1. Abra a sua aplicação ao clicar em seu nome no **as minhas aplicações** página e, em seguida, clique em **crie**, em seguida, clique em **frase listas** no painel esquerdo da sua aplicação. 

2. Sobre o **frase listas** página, clique em **criar nova lista de frase**. 
 
3. Na **lista de frase adicionar** caixa de diálogo, escreva "Cidades" como o nome da lista frase. Na **valor** , escreva os valores da lista frase. Escreva um valor numa hora ou um conjunto de valores separados por vírgulas e, em seguida, prima **Enter**.

    ![Adicionar frase lista cidades](./media/luis-add-features/add-phrase-list-cities.png)

4. LUIS pode propor valores relacionados para adicionar à sua lista de frase. Clique em **Recomendamos** para obter um grupo de valores propostos semanticamente relacionados com o added value(s). Pode clicar em qualquer um dos valores propostos ou clique em **adicionar todos os** para adicioná-los todos.

    ![Lista de frase proposto valores](./media/luis-add-features/related-values.png)

5. Clique em **estes valores são intercambiáveis** se os valores de lista de frase adicionados são alternativas que podem ser utilizadas alternadamente.

    ![Lista de frase proposto valores](./media/luis-add-features/interchangeable.png)

6. Clique em **Guardar**. A lista de frase "Cidades" é adicionada para o **frase listas** página.

<a name="edit-phrase-list"></a>
<a name="delete-phrase-list"></a>
<a name="deactivate-phrase-list"></a>

> [!Note]
> Pode eliminar ou desativar uma lista de frase da barra de ferramentas contextual sobre o **frase listas** página.

## <a name="pattern-regular-expression-feature"></a>Funcionalidade do padrão (expressão regular) 
**Esta funcionalidade foi preterida**. Não não possível adicionar novos recursos de padrão LUIS. Todos os recursos padrão existentes são suportados até Maio de 2018. Contribuir para a expressão regular padrão do LUIS, correspondência com um pedido Pull para o [repositório do Github de texto reconhecedores](https://github.com/Microsoft/Recognizers-Text). 

## <a name="next-steps"></a>Passos Seguintes

Após adicionar, editar, eliminar ou desativar uma lista de frase [treinar e testar a aplicação](luis-interactive-test.md) novamente para ver se melhora o desempenho.
