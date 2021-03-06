---
title: Sobre o texto em fala - serviço de voz
titleSuffix: Azure Cognitive Services
description: A API de voz oferece mais de 75 vozes em mais de 45 idiomas e localidades. Para utilizar tipos de voz padrão, só tem de especificar o nome de voz com alguns outros parâmetros ao chamar o serviço de voz.
services: cognitive-services
author: erhopf
manager: cgronlun
ms.service: cognitive-services
ms.component: speech-service
ms.topic: conceptual
ms.date: 10/16/2018
ms.author: erhopf
ms.openlocfilehash: 7f01fe5c71cdd6f4c70527fcf2553374aae9a5d8
ms.sourcegitcommit: 62759a225d8fe1872b60ab0441d1c7ac809f9102
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/19/2018
ms.locfileid: "49469933"
---
# <a name="about-the-text-to-speech-api"></a>Sobre o texto em voz, API

O **texto em voz** API (TTS) do serviço de voz converte a entrada de texto em voz com som natural (também denominado *síntese de fala*).

Para gerar voz, a sua aplicação envia pedidos de HTTP POST para o serviço de voz. Lá, o texto é sintetizado em voz com som humanos e retornado como um arquivo de áudio. Uma variedade de vozes e idiomas são suportados.

Os cenários em que fala síntese está a ser adotada incluem:

* *Melhorar a acessibilidade:* **texto em voz** tecnologia permite que os proprietários de conteúdo e os editores a responder para as pessoas de diferentes formas interagirem com os seus conteúdos. As pessoas com dificuldades de leitura ou o visual impairment apreciam a capacidade de consumir conteúdos aurally. Voz de saída também torna mais fácil para as pessoas usufruir conteúdo textual, como jornais ou posta em blog, em dispositivos móveis ao exercitar transporte ou viagens.

* *A responder em cenários de multitarefa:* **texto em voz** permite que as pessoas que visam absorver informações importantes rapidamente e confortavelmente ao dirigir ou caso contrário, um conveniente fora ler ambiente. A navegação é um aplicativo comum nessa área.

* *Aprimoramento de aprendizagem com vários modos:* pessoas diferentes saiba a melhor de formas diferentes. Especialistas de aprendizagem online têm mostrado que fornecer o texto e voz em conjunto pode ajudar a facilitar as informações saber mais e manter.

* *Fornecimento de bots intuitivo ou assistentes:* a capacidade de comunicar com pode ser uma parte integral de um chatbot inteligente ou um assistente virtual. Cada vez mais empresas estão desenvolvendo bots de bate-papo para fornecer experiências de serviço de cliente atraente para seus clientes. Voz adiciona outra dimensão, permitindo que as respostas do bot para que sejam recebidas aurally (por exemplo, por telefone).

## <a name="voice-support"></a>Suporte de voz

A Microsoft **voz** serviço oferece mais de 75 vozes em mais de 45 idiomas e localidades. Para utilizar estes padrão "tipos de voz", só precisa de especificar o nome de voz com alguns outros parâmetros quando chama a API de REST do serviço. Para obter os detalhes das vozes suportados, consulte [idiomas suportados](language-support.md#text-to-speech).

Se desejar uma voz única para a sua aplicação, pode criar [tipos de voz personalizada](how-to-customize-voice-font.md) de seus próprios exemplos de voz.

## <a name="api-capabilities"></a>Capacidades de API

Muitas das funcionalidades dos **texto em voz** API, sobretudo em torno da personalização, estão disponíveis através de REST. A tabela seguinte resume as capacidades de cada método de acessar a API. Para obter uma lista completa de capacidades e detalhes de API, consulte [Swagger referência](https://westus.cris.ai/swagger/ui/index).

| Caso de utilização | REST | SDKs |
|-----|-----|-----|----|
| Carregar conjuntos de dados para adaptação de voz | Sim | Não |
| Criar e gerir modelos de tipo de letra de voz | Sim | Não |
| Criar e gerir implementações de tipo de letra de voz | Sim | Não |
| Criar e gerir os testes de tipo de letra de voz| Sim | Não |
| Gerir Subscrições | Sim | Não |

> [!NOTE]
> A API implementa limitação que limita os pedidos de API para 25 por 5 segundos. Cabeçalhos de mensagens informará os limites.

## <a name="next-steps"></a>Passos Seguintes

* [Obter a subscrição de avaliação de Voz](https://azure.microsoft.com/try/cognitive-services/)
* [Veja como sintetizar voz através da API REST](how-to-text-to-speech.md)
