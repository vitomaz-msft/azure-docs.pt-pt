---
title: Como criar um modelo de aprendiz de conversação com node. js - serviços cognitivos da Microsoft | Documentos da Microsoft
titleSuffix: Azure
description: Saiba como criar um modelo de aprendiz de conversação com node. js.
services: cognitive-services
author: v-jaswel
manager: nolachar
ms.service: cognitive-services
ms.component: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: v-jaswel
ms.openlocfilehash: bc0cf0900ec9f87c75091b3bf219d92e0859aa1f
ms.sourcegitcommit: 068fc623c1bb7fb767919c4882280cad8bc33e3a
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/27/2018
ms.locfileid: "39282144"
---
# <a name="create-a-conversation-learner-model-using-nodejs"></a>Criar um modelo de aprendiz de conversação com node. js

Aprendiz de conversação reduz a complexidade da criação de bots. Permite que um híbrido desenvolvimento fluxo de trabalho que permite que o código escrito à mão e aprendizagem automática reduzir a quantidade de código necessário para escrever bots. Determinadas partes fixas do seu modelo, como a verificar se o utilizador inicia sessão ou a efetuar um pedido de API para verificar o inventário da loja, ainda podem ser codificadas. No entanto, outras alterações no Estado e a ação de seleção podem ser adquiridas a partir de caixas de diálogo de exemplo fornecidas pelo especialista de domínio ou o desenvolvedor.

## <a name="invitation-required"></a>Convite necessário

*Um convite é necessário para aceder ao aprendiz de conversação do projeto.*

Aprendiz de conversação do projeto é constituído por um SDK que adiciona ao seu bot e um serviço cloud que o SDK acessa para machine learning.  No momento, o acesso ao serviço de nuvem do projeto conversação Leaner requer um convite.  Se ainda não foi convidado já, [pedir um convite](https://aka.ms/conversation-learner-request-invite).  Se não tenha recebido um convite, não será possível acessar a API da cloud.

## <a name="prerequisites"></a>Pré-requisitos

- Nó 8.5.0 ou superior e o npm 5.3.0 ou superior. Instalar a partir [ https://nodejs.org ](https://nodejs.org).
  
- Chave de criação de LUIS:

  1. Inicie sessão no [ http://www.luis.ai ](http://www.luis.ai).

  2. Clique em seu nome no canto superior direito, em seguida, em "definições"

  3. Chave de criação é mostrada na página resultante

  (A sua chave de criação de LUIS serve 2 funções.  Em primeiro lugar, ele irá servir como a chave de criação de aprendiz de conversação.  Segundo, o aprendiz de conversação usa o LUIS para extração de entidades; o LUIS criação de chave é utilizado para criar modelos de LUIS em seu nome)

- Browser Google Chrome. Instalar a partir [ https://www.google.com/chrome/index.html ](https://www.google.com/chrome/index.html).

- Git. Instalar a partir [ https://git-scm.com/downloads ](https://git-scm.com/downloads).

- VSCode. Instalar a partir [ https://code.visualstudio.com/ ](https://code.visualstudio.com/). Tenha em atenção de que isso é recomendado, não é necessário.

## <a name="quick-start"></a>Início rápido 

1. Instalar e criar:

    ```bash    
    git clone https://github.com/Microsoft/ConversationLearner-Samples cl-bot-01
    cd cl-bot-01
    npm install
    npm run build
    ```

    > [!NOTE]
    > Durante a `npm install`, pode ignorar este erro se ocorrer: `gyp ERR! stack Error: Can't find Python executable`

2. Configure:

   Crie um ficheiro chamado `.env` no diretório `cl-bot-01`.  O conteúdo do ficheiro deve ser:

   ```
   NODE_ENV=development
   LUIS_AUTHORING_KEY=<your LUIS authoring key>
   ```

3. Bot de início:

    ```
    npm start
    ```

    Isso é executado o bot vazio genérico em `cl-bot-01/src/app.ts`.

3. Execute o aprendiz de conversação da interface do Usuário:

    ```bash
    [open second command prompt window]
    cd cl-bot-01
    npm run ui
    ```

4. Browser aberto para http://localhost:5050 

Agora está a utilizar o aprendiz de conversação e pode criar e ensinar um modelo de aprendiz de conversação.  

> [!NOTE]
> No início, o aprendiz de conversação de projeto está disponível por convite.  Se http://localhost:5050 mostra um HTTP `403` erro, isso significa que sua conta não foi convidada.  Tente [pedir um convite](https://aka.ms/conversation-learner-request-invite).

## <a name="tutorials-demos-and-switching-between-bots"></a>Tutoriais, demonstrações e alternar entre bots

As instruções acima a utilizar o bot vazio genérico.  Para executar um tutorial ou em vez disso, o bot de demonstração:

1. Se tiver web Aprendiz de conversação, abra a interface do Usuário, regressar à lista de modelos em http://localhost:5050/home.
    
2. Se estiver a executar outra bot (como `npm start` ou `npm run demo-pizza`), impedi-lo.  Não é necessário parar o processo de interface do Usuário ou fechar o navegador da web.

3. Execute um bot de demonstração da linha de comando (etapa 2 acima).  Demonstrações incluem:

  ```bash
  npm run tutorial-general
  npm run tutorial-entity-detection
  npm run tutorial-session-callbacks
  npm run tutorial-api-calls
  npm run tutorial-hybrid
  npm run demo-password
  npm run demo-pizza
  npm run demo-storage
  npm run demo-vrapp
  ```

4. Se não estiver já, mude para a web de aprendiz de conversação da interface do Usuário no Chrome carregando http://localhost:5050/home. 

5. Clique em "Tutoriais de importação" (apenas precisa ser feito uma vez).  Isto irá demorar cerca de um minuto e copiará os modelos de aprendiz de conversação para todos os tutoriais na sua conta de aprendiz de conversação.

6. Clique no modelo de demonstração na IU de aprendiz de conversação que corresponde à demonstração iniciou.

Ficheiros de origem para as demonstrações estão em `cl-bot-01/src/demos`

## <a name="create-a-bot-which-includes-back-end-code"></a>Criar um bot, que inclui o código de back-end

1. Se tiver web Aprendiz de conversação, abra a interface do Usuário, regressar à lista de modelos em http://localhost:5050/home.
    
2. Se estiver a executar um bot (como `npm run demo-pizza`), impedi-lo.  Não é necessário parar o processo de interface do Usuário ou fechar o navegador da web.

3. Se assim o desejar, editar o código na `cl-bot-01/src/app.ts`.

4. Reconstruir e voltar a iniciar bot:

    ```bash    
    npm run build
    npm start
    ```

5. Se não estiver já, mude para a web de aprendiz de conversação da interface do Usuário no Chrome carregando http://localhost:5050/home. 

6. Criar um novo modelo de aprendiz de conversação na interface de Usuário e iniciar o ensino.

7. Para fazer alterações de código no `cl-bot-01/src/app.ts`, repita os passos acima, a partir do passo 2.

## <a name="vscode"></a>VSCode

No VSCode, lá são executadas configurações para cada demonstração e para o bot"vazio" no `cl-bot-01/src/app.ts`.  Abra o `cl-bot-01` pasta no VSCode.

## <a name="advanced-configuration"></a>Configuração avançada

Há um modelo `.env.example` ficheiro mostra qual o ambiente pode ser definido para configurar os exemplos de variáveis.

Pode ajustar estas portas para evitar conflitos entre outros serviços em execução no seu computador, adicionando um `.env` ficheiro para a raiz do projeto:

```bash
cp .env.example .env
```

Esta opção utiliza a configuração padrão, o que permite-lhe executar localmente o seu bot e começar a utilizar o aprendiz de conversação.  (Mais tarde no, para implementar o seu bot ao Bot Framework, algumas edições para este ficheiro serão necessária.)

## <a name="support"></a>Suporte

- Marque as perguntas [Stack Overflow](https://stackoverflow.com) com "cognitivos da microsoft"
- Pedir uma funcionalidade no nosso [página de voz do utilizador](https://aka.ms/conversation-learner-uservoice)
- Abra um problema no nosso [repositório do github](https://github.com/Microsoft/ConversationLearner-Samples)

## <a name="contributing"></a>Contribuir

Este projeto adotou o [Microsoft Open Source Code of Conduct (Código de Conduta do Microsoft Open Source)](https://opensource.microsoft.com/codeofconduct/). Para obter mais informações, consulte a [código de conduta FAQ](https://opensource.microsoft.com/codeofconduct/faq/) ou contacte [ opencode@microsoft.com ](mailto:opencode@microsoft.com) com quaisquer perguntas ou comentários adicionais.

## <a name="source-repositories"></a>Repositórios de origem

- [Exemplos de conversationlearner](https://github.com/Microsoft/ConversationLearner-Samples)
- [conversationlearner-sdk](https://github.com/Microsoft/ConversationLearner-SDK)
- [modelos de conversationlearner](https://github.com/Microsoft/ConversationLearner-Models)
- [interface do usuário conversationlearner](https://github.com/Microsoft/ConversationLearner-UI)
- [conversationlearner Chat](https://github.com/Microsoft/ConversationLearner-WebChat)

## <a name="next-steps"></a>Passos Seguintes

> [!div class="nextstepaction"]
> [Olá, mundo](./tutorials/1-hello-world.md)
