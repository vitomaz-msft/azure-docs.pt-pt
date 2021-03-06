---
title: Migrar de voz do Bing para o serviço de voz
titleSuffix: Azure Cognitive Services
description: Aprenda as diferenças entre a voz do Bing e o serviço de voz de um ponto de vista do desenvolvedor e migrar a sua aplicação para utilizar o serviço de voz.
services: cognitive-services
author: wsturman
manager: cgronlun
ms.service: cognitive-services
ms.component: speech-service
ms.topic: conceptual
ms.date: 10/01/2018
ms.author: gracez
ms.openlocfilehash: fdd22e14e0b7636dbc337a20dd69bf93696bb924
ms.sourcegitcommit: 6135cd9a0dae9755c5ec33b8201ba3e0d5f7b5a1
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/31/2018
ms.locfileid: "50416285"
---
# <a name="migrate-from-bing-speech-to-the-speech-service"></a>Migrar de voz do Bing para o serviço de voz

Utilize este artigo para migrar as suas aplicações da API de voz do Bing para o serviço de voz.

Este artigo descreve as diferenças entre as APIs de voz do Bing e o serviço de voz e sugere estratégias para migrar seus aplicativos. A chave de subscrição de API de voz do Bing não aceite pelo serviço de voz; precisará de uma nova subscrição do serviço de voz.

Uma única chave de subscrição do Serviço de Voz concede acesso às seguintes funcionalidades. Cada uma é medida em separado, pelo que apenas lhe são cobradas as funcionalidades que utilizar.

* [Conversão de voz em texto](speech-to-text.md)
* [Conversão de voz em texto personalizada](https://cris.ai)
* [Conversão de texto em voz](text-to-speech.md)
* [Vozes personalizadas para conversão de texto em voz](how-to-customize-voice-font.md)
* [Tradução de Voz](speech-translation.md) (não inclui [Tradução de texto](../translator/translator-info-overview.md))

O [SDK de voz](speech-sdk.md) é uma substituição funcional para as bibliotecas de cliente de voz do Bing, mas utiliza uma API diferente.

## <a name="comparison-of-features"></a>Comparação de funcionalidades

O serviço de voz é muito semelhante de voz do Bing, com as seguintes diferenças.

Funcionalidade | Voz do Bing | Serviço de Voz | Detalhes
-|-|-|-
C++ SDK | : heavy_minus_sign: | :heavy_check_mark: | Serviço de voz suporta Windows e Linux.
SDK Java | :heavy_check_mark: | :heavy_check_mark: | Serviço de voz suporta Android e dispositivos de voz.
SDK C# | :heavy_check_mark: | :heavy_check_mark: | Serviço de voz oferece suporte a Windows 10, a plataforma Universal do Windows (UWP) e o .NET Standard 2.0.
Reconhecimento de fala contínua | 10 minutos | Ilimitado (com o SDK) | Voz do Bing e protocolos de WebSockets do serviço de voz suportam até 10 minutos por chamada. No entanto, o SDK de voz automaticamente volta no tempo limite de ligar ou desligar.
Resultados parciais ou intermediárias | :heavy_check_mark: | :heavy_check_mark: | Com o protocolo de WebSockets ou o SDK.
Modelos de voz personalizada | :heavy_check_mark: | :heavy_check_mark: | Voz do Bing requer uma subscrição separada de voz personalizada.
Tipos de voz personalizada | :heavy_check_mark: | :heavy_check_mark: | Voz do Bing requer uma subscrição separada de voz personalizada.
Vozes 24 kHz | : heavy_minus_sign: | :heavy_check_mark: 
Reconhecimento da intenção do discurso | Requer separada chamada à API de LUIS | Integrada (com o SDK) |  Pode utilizar uma chave de LUIS com o serviço de voz.
Reconhecimento da intenção Simple | : heavy_minus_sign: | :heavy_check_mark: 
Transcrição de batch de arquivos de áudio há muito tempo | : heavy_minus_sign: | :heavy_check_mark:
Modo de reconhecimento | Manual através do URI do ponto final | Automático | Modo de reconhecimento não está disponível no serviço de voz.
Localidade do ponto final | Global | Regional | Pontos finais regionais melhoram a latência.
APIs REST | :heavy_check_mark: | :heavy_check_mark: | API de REST do serviço de voz é compatível com a voz do Bing (ponto de extremidade diferente). REST APIs suportam a funcionalidade de voz em texto de texto para voz e limitada.
Protocolos de WebSockets | :heavy_check_mark: | :heavy_check_mark: | API de WebSockets de serviço de voz é compatível com a voz do Bing (ponto de extremidade diferente). Migre para o SDK de voz se possível, para simplificar o seu código.
Chamadas à API de serviços | :heavy_check_mark: | : heavy_minus_sign: | Fornecido em voz do Bing através da biblioteca de serviço c#. 
SDK de código aberto | :heavy_check_mark: | : heavy_minus_sign: |

O serviço de voz utiliza um modelo de preços baseados no tempo (em vez de um modelo baseado em transações). Veja os [Preços do Serviço de Voz](https://azure.microsoft.com/pricing/details/cognitive-services/speech-services/) para obter detalhes.

## <a name="migration-strategies"></a>Estratégias de migração

Se ou sua organização tiver aplicativos no desenvolvimento ou de produção que utilizam uma API de voz do Bing, atualize-os para utilizar o serviço de voz logo que possível. Consulte a [documentação de serviço de voz](index.yml) para disponíveis SDKs, exemplos de códigos e tutoriais.

O serviço de voz [REST APIs](rest-apis.md) são compatíveis com as APIs de voz do Bing. Se estiver a utilizar atualmente as APIs de REST de voz do Bing, precisará apenas alterar o ponto final REST e mudar para uma chave de subscrição do serviço de voz.

Os protocolos de WebSockets do serviço de voz também são compatíveis com os usados por voz do Bing. Recomendamos que, para desenvolvimento de novas, utilize o SDK do serviço de voz em vez de WebSockets. É uma boa idéia para migrar o código existente para o SDK também. No entanto, como com as APIs REST, código existente que utilize a voz do Bing através de WebSockets requer apenas uma alteração no ponto de extremidade e uma chave atualizada.

Se estiver a utilizar uma biblioteca de cliente de voz do Bing para uma linguagem de programação específica, migrar para o [SDK de voz](speech-sdk.md) requer alterações ao seu aplicativo, porque a API é diferente. O SDK de voz pode tornar o código mais simples, ao mesmo tempo dando acesso aos novos recursos.

Atualmente, o SDK de voz suporta c# (Windows 10, UWP, .NET Standard), o Java (dispositivos Android e personalizados), Objective C (iOS), C++ (Windows e Linux) e JavaScript. APIs em todas as plataformas são semelhantes, facilitando o desenvolvimento de Multiplataforma.

O serviço de voz atualmente não oferece um ponto final global. Determine se a aplicação funciona com eficiência quando utiliza um único ponto de final de regional para todo o tráfego. Caso contrário, utilize a localização geográfica para determinar o ponto de extremidade mais eficiente. Tem uma subscrição separada do serviço de voz em cada região a que utilizar.

Se a sua aplicação utiliza ligações longa duração e não é possível utilizar um SDK disponível, pode utilizar uma ligação de WebSockets. Gerir o limite de tempo limite de 10 minutos a restabelecer ligação nos momentos apropriados.

Para começar a utilizar com o SDK de voz:

1. Transfira o [SDK de voz](speech-sdk.md).
1. Trabalho através do serviço de voz [guias de início rápido](quickstart-csharp-dotnet-windows.md) e [tutoriais](how-to-recognize-intents-from-speech-csharp.md). Observe também o [exemplos de código](samples.md) obter experiência com as novas APIs.
1. Atualize a sua aplicação para utilizar o serviço de voz e APIs.

## <a name="support"></a>Suporte

Os clientes de voz do Bing devem contactar o suporte ao cliente ao abrir um [pedido de suporte](https://ms.portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest). Também pode contactar-nos se precisar de sua necessidade de suporte um [plano de suporte técnico](https://azure.microsoft.com/support/plans/).

Para obter suporte de API, SDK e serviço de voz, visite o serviço de voz [página de suporte](support.md).

## <a name="next-steps"></a>Passos Seguintes

* [Experimente gratuitamente o serviço de voz](get-started.md)
* [Início rápido: Reconhecer voz numa aplicação UWP utilizando o SDK de voz](quickstart-csharp-uwp.md)

## <a name="see-also"></a>Consulte também
* [Notas de versão do serviço de voz](releasenotes.md)
* [O que é o serviço de voz](overview.md)
* [Documentação do serviço de voz e o SDK](speech-sdk.md#get-the-sdk)
