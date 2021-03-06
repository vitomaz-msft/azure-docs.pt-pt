---
title: Descrição geral das Funções do Azure | Microsoft Docs
description: Compreenda como utilizar as Funções do Azure para otimizar cargas de trabalho assíncronas em minutos.
services: functions
documentationcenter: na
author: mattchenderson
manager: jeconnoc
keywords: funções do azure, funções, processamento de eventos, webhooks, computação dinâmica, arquitetura sem servidor
ms.assetid: 01d6ca9f-ca3f-44fa-b0b9-7ffee115acd4
ms.service: azure-functions
ms.devlang: multiple
ms.topic: overview
ms.date: 10/03/2017
ms.author: glenga
ms.custom: H1Hack27Feb2017, mvc
ms.openlocfilehash: f2890fa4d8260485fa76478571aa66abe1efd35e
ms.sourcegitcommit: 5a1d601f01444be7d9f405df18c57be0316a1c79
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/10/2018
ms.locfileid: "51515408"
---
# <a name="an-introduction-to-azure-functions"></a>Uma introdução às Funções do Azure  
Funções do Azure é uma solução para uma fácil execução de pequenos blocos de código, ou "funções", na nuvem. É possível escrever apenas o código que necessita para o problema em questão, sem ter de se preocupar com toda a aplicação ou a infraestrutura para executá-la. As funções podem tornar o desenvolvimento ainda mais produtivo, e podem utilizar uma linguagem de desenvolvimento à sua escolha, tal como C#, F#, Node.js, Java ou PHP. Paga apenas para a hora que o seu código é executado e confie no Azure para dimensionar conforme necessário. As Funções do Azure permitem desenvolver aplicações [sem servidor](https://azure.microsoft.com/solutions/serverless/) no Microsoft Azure.

Este tópico fornece uma descrição geral de alto nível das Funções do Azure. Se quiser iniciar de imediato e utilizar as Funções, comece por [Criar a sua primeira Função do Azure](functions-create-first-azure-function.md). Se estiver à procura de mais informações técnicas acerca das Funções, consulte a [referência para programadores](functions-reference.md).

## <a name="features"></a>Funcionalidades
Seguem-se algumas funcionalidades-chave das Funções:

* **Escolha da linguagem** - Escreva funções com C#, F# ou Javascript, conforme a sua escolha. Veja [Idiomas suportados](supported-languages.md) para outras opções.
* **Modelo de preços de pagamento por utilização** - Pague apenas o tempo despendido a executar o seu código. Veja a opção do plano de alojamento de Consumo na [secção preços](#pricing).  
* **Traga as suas próprias dependências** - As funções são compatíveis com NuGet e NPM, pelo que pode utilizar as suas bibliotecas favoritas.  
* **Segurança integrada** - Proteja as funções acionadas por HTTP com fornecedores de OAuth, como o Azure Active Directory, Facebook, Google, Twitter e Conta Microsoft.  
* **Integração simplificada** - Tire facilmente partido dos serviços do Azure e ofertas de software como serviço (SaaS). Consulte a [secção Integrações](#integrations) para obter alguns exemplos.  
* **Desenvolvimento flexível** – codifique as suas funções diretamente no portal ou configure a integração contínua e implemente o seu código através do [GitHub](../app-service/scripts/app-service-cli-continuous-deployment-github.md), dos [Serviços de DevOps do Azure](../app-service/scripts/app-service-cli-continuous-deployment-vsts.md) e de outras [ferramentas de programação suportadas](../app-service/app-service-deploy-local-git.md).  
* **Código aberto** - O tempo de execução das Funções é de código aberto e [está disponível no GitHub](https://github.com/azure/azure-webjobs-sdk-script).  

## <a name="what-can-i-do-with-functions"></a>O que posso fazer com as Funções?
Funções é uma excelente solução para processar dados, integrar sistemas, trabalhar com a Internet das Coisas (IoT) e a criação de API simples e micro-serviços. Considere utilizar as Funções para tarefas como processamento de imagem ou encomendas, manutenção de ficheiros ou para quaisquer tarefas que pretende executar com base numa agenda. 

Funções fornece modelos para ajudar a começar com cenários-chave, que incluem:

* **HTTPTrigger** - Acionar a execução do seu código através da utilização de um pedido de HTTP. Por exemplo, veja [Create your first function (Criar a sua primeira função)](functions-create-first-azure-function.md).
* **TimerTrigger** - Executar tarefas de limpeza ou outras tarefas de lote numa agenda predefinida. Por exemplo, veja [Create a function triggered by a timer (Criar uma função acionada por um temporizador)](functions-create-scheduled-function.md).
* **CosmosDBTrigger** - Processar documentos do Azure Cosmos DB quando são adicionados ou atualizados em coleções numa base de dados NoSQL. Para obter mais informações, veja [Enlaces do Azure Cosmos DB](functions-bindings-cosmosdb-v2.md).
* **BlobTrigger** - Processa blobs de armazenamento do Azure quando são adicionados a contentores. Pode utilizar esta função para redimensionar a imagem. Para obter mais informações, veja [Blob storage bindings (Enlaces de armazenamento de blobs)](functions-bindings-storage-blob.md).
* **QueueTrigger** - Responder a mensagens à medida que chegam a uma fila de Armazenamento do Azure. Para obter mais informações, veja [Enlaces do Armazenamento de filas do Azure](functions-bindings-storage-queue.md).
* **EventGridTrigger** – Responder a eventos fornecidos a uma subscrição no Azure Event Grid. Suporta um modelo com base na subscrição para receber eventos, incluindo a filtragem. Uma boa solução para a criação de arquiteturas baseadas em eventos. Para obter um exemplo, veja [Automatizar o redimensionamento de imagens carregadas com o Event Grid](../event-grid/resize-images-on-storage-blob-upload-event.md).
* **EventHubTrigger** - Responder a eventos fornecidos a um Hub de Eventos do Azure. Particularmente útil em cenários de internet das coisas (IoT), de processamento de experiência ou fluxo de trabalho de utilizador e de instrumentação de aplicações. Para obter mais informações, veja [Event Hubs bindings (Enlaces de Hubs de Eventos)](functions-bindings-event-hubs.md).
* **ServiceBusQueueTrigger** - Ligar o seu código a outros serviços do Azure ou serviços no local através da escuta de filas de mensagens. Para obter mais informações, veja [Service Bus bindings (Enlaces do Service Bus)](functions-bindings-service-bus.md).
* **ServiceBusQueueTrigger** - Ligar o seu código a outros serviços do Azure ou serviços no local através da subscrição de tópicos. Para obter mais informações, veja [Service Bus bindings (Enlaces do Service Bus)](functions-bindings-service-bus.md).

Funções do Azure suporta *acionadores*, que são formas de iniciar a execução do seu código, e *enlaces*, que são formas para simplificar a codificação de dados de entrada e saída. Para obter uma descrição detalhada dos acionadores e enlaces fornecidos pelas Funções do Azure, consulte o artigo [Referência para programadores de acionadores e enlaces das Funções do Azure](functions-triggers-bindings.md).

## <a name="integrations"></a>ntegrações
Funções do Azure integra-se com vários serviços do Azure e de terceiros. Estes serviços podem acionar a sua função e iniciar a execução ou podem servir como entrada e saída para o seu código. As Funções do Azure são compatíveis com as seguintes integrações de serviço:

* Azure Cosmos DB
* Azure Event Hubs
* Azure Event Grid
* Azure Notification Hubs
* Azure Service Bus (filas e tópicos)
* Armazenamento do Azure (blob, filas e tabelas)
* No local (utilizando o Service Bus)
* Twilio (mensagens SMS)

## <a name="pricing"></a>Quanto custam as Funções?
As Funções do Azure têm dois tipos de planos de preços. Escolha o que melhor se adequa às suas necessidades: 

* **Plano de Consumo** - Quando a função é executada, o Azure fornece todos os recursos informáticos necessários. Não tem de se preocupar com a gestão de recursos, paga apenas o tempo de execução do seu código. 
* **Plano do Serviço de Aplicações** – Executar as funções como aplicações da Web. Se já utilizar o App Service para outras aplicações, pode executar as suas funções no mesmo plano sem custos adicionais. 

Para obter mais informações sobre os planos de alojamento, veja [Azure Functions hosting plan comparison (Comparação do plano de alojamento das Funções do Azure)](functions-scale.md). Os detalhes completos sobre os preços estão disponíveis na [Preços das Funções](https://azure.microsoft.com/pricing/details/functions/).

## <a name="next-steps"></a>Próximos Passos
* [Criar a sua primeira Função do Azure](functions-create-first-azure-function.md)  
  Comece de imediato e crie a sua primeira função com o guia de introdução das Funções do Azure. 
* [Referência para programadores das Funções do Azure](functions-reference.md)  
  Fornece mais informações técnicas sobre o tempo de execução das Funções do Azure e uma referência para as funções de codificação e definição de acionadores e enlaces.
* [Testar as Funções do Azure](functions-test-a-function.md)  
  Descreve várias ferramentas e técnicas para testar as suas funções.
* [Como dimensionar as Funções do Azure](functions-scale.md)  
  Aborda os planos de serviço disponíveis com as Funções do Azure, incluindo o plano de alojamento de Consumo e como escolher o plano adequado. 
* [Saiba mais sobre o Azure App Service](../app-service/app-service-web-overview.md)  
  Funções do Azure melhora o Serviço de Aplicações do Azure para funcionalidades essenciais como implementações, variáveis de ambiente e diagnósticos. 

