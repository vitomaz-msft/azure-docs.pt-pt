---
title: Sobre a conversão de voz em texto
titleSuffix: Azure Cognitive Services
description: Uma descrição geral das funcionalidades de voz para a API de texto.
services: cognitive-services
author: erhopf
manager: cgronlun
ms.service: cognitive-services
ms.component: speech-service
ms.topic: conceptual
ms.date: 05/07/2018
ms.author: erhopf
ms.openlocfilehash: 041ec1c095ec604fed7906368ff266c1586df570
ms.sourcegitcommit: 62759a225d8fe1872b60ab0441d1c7ac809f9102
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/19/2018
ms.locfileid: "49471344"
---
# <a name="about-the-speech-to-text-api"></a>Sobre a conversão de voz em texto API

O **conversão de voz em texto** API *converte* fluxos de áudio em texto que seu aplicativo pode apresentar ao utilizador ou agir como comando de entrada. As APIs podem ser utilizadas com uma biblioteca de cliente do SDK (para as plataformas suportadas e idiomas) ou uma API REST.

O **conversão de voz em texto** API oferece as seguintes funcionalidades:

- Advanced tecnologia de reconhecimento de voz da Microsoft — o mesmo usado pelo Cortana, Office e outros produtos da Microsoft.

- Reconhecimento contínuo em tempo real. **Conversão de voz em texto** permite que os usuários a transcrição de áudio em texto em tempo real. Também suporta a receber os resultados intermediários das palavras que tem sido reconhecidos até agora. O serviço reconhece automaticamente o final de voz. Os utilizadores também podem escolher opções de formatação adicionais, incluindo capitalização e pontuação, linguagem inapropriada máscara e normalização do texto inversa.

- Os resultados devolvidos em formulários de apresentação e Lexical (para obter resultados Lexical, consulte DetailedSpeechRecognitionResult a exemplos ou a API).

- Suporte para vários idiomas e dialetos. Para obter a lista completa das linguagens suportadas em cada modo de reconhecimento, consulte [idiomas suportados](language-support.md#speech-to-text).

- Personalizado modelos acústicos e de linguagem, pelo que pode personalizar a aplicação do vocabulário de domínio especializadas dos seus utilizadores, falando em ambiente e a forma de falar.

- Compreensão de linguagem natural. Através da integração com [compreensão de idiomas](https://docs.microsoft.com/azure/cognitive-services/luis/) (LUIS), pode derivar intenções e entidades de voz. Os utilizadores não precisam conhecer o vocabulário da sua aplicação, mas podem descrever o que eles querem em suas próprias palavras.

- Pontuação de confiança é devolvida a resposta do serviço se especificar uma saída detalhada no objeto de configuração de voz (SpeechConfig.OutputFormat propriedade). Em seguida, pode utilizar qualquer um dos métodos de Best() no resultado ou obtê-lo diretamente a pontuação de JSON devolvido do serviço (algo como resultado. Properties.GetProperty(PropertyId.SpeechServiceResponse_JsonResult)).

## <a name="api-capabilities"></a>Capacidades de API

Algumas das capacidades do **conversão de voz em texto** API, sobretudo em torno da personalização, estão disponíveis através de REST. A tabela seguinte resume as capacidades de cada método de acessar a API. Para obter uma lista completa de capacidades e detalhes de API, consulte [Swagger referência](https://westus.cris.ai/swagger/ui/index).

| Caso de utilização | REST | SDKs |
|-----|-----|-----|----|
| Transcrição de uma expressão de curto, tal como um comando (comprimento < 15 s); Não existem resultados intermediárias | Sim | Sim |
| Transcrição de uma expressão maior (> 15 s) | Não | Sim |
| Transcrição de áudio de transmissão em fluxo com resultados provisórias opcionais | Não | Sim |
| Compreender os objetivos de orador por meio do LUIS | Não\* | Sim |
| Criar testes de precisão | Sim | Não |
| Carregar conjuntos de dados para adaptação de modelos | Sim | Não |
| Criar e gerir modelos de voz | Sim | Não |
| Criar e gerir implementações de modelos | Sim | Não |
| Gerir Subscrições | Sim | Não |
| Criar e gerir implementações de modelos | Sim | Não |
| Criar e gerir implementações de modelos | Sim | Não |

> [!NOTE]
> A API REST implementa limitação que limita os pedidos de API para 25 por 5 segundos. Cabeçalhos de mensagens informará os limites

\* *LUIS intenções e entidades podem ser obtidas através de uma subscrição separada do LUIS. Com esta subscrição, o SDK pode chamar o LUIS para e fornecer resultados de entidade e propósito, bem como as transcrições de voz. Com a API REST, pode chamar o LUIS para derivar intenções e entidades com a sua subscrição do LUIS.*

## <a name="next-steps"></a>Passos Seguintes

* [Obter a subscrição de avaliação de Voz](https://azure.microsoft.com/try/cognitive-services/)
* [Início rápido: reconhecer voz em c#](quickstart-csharp-dotnet-windows.md)
* [Veja como a reconhecer intenções de voz em c#](how-to-recognize-intents-from-speech-csharp.md)
