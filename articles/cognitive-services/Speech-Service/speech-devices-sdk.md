---
title: Sobre os dispositivos de voz SDK
titleSuffix: Azure Cognitive Services
description: Obtenha uma introdução ao SDK de dispositivos de voz.
services: cognitive-services
author: erhopf
manager: cgronlun
ms.service: cognitive-services
ms.component: speech-service
ms.topic: conceptual
ms.date: 05/07/2018
ms.author: erhopf
ms.openlocfilehash: eac3542059f1bc5d32a91ef871e5185fad1d2798
ms.sourcegitcommit: 62759a225d8fe1872b60ab0441d1c7ac809f9102
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/19/2018
ms.locfileid: "49464102"
---
# <a name="about-the-speech-devices-sdk-preview"></a>Sobre os dispositivos de voz SDK (pré-visualização)

O [serviço de voz](overview.md) funciona com uma ampla variedade de dispositivos e fontes de áudio. Agora, pode tirar seus aplicativos de fala para o próximo nível com correspondentes de hardware e software. O SDK de dispositivos de voz é uma biblioteca pretuned que se encontra emparelhada com finalidade específica, kits de desenvolvimento de matriz de microfone. 

O SDK de dispositivos de voz pode ajudá-lo a:
* Teste rapidamente a novos cenários de voz.
* Integre mais facilmente o serviço de voz com base na cloud no seu dispositivo.
* Crie uma experiência de usuário excecional para os seus clientes. 

O SDK de dispositivos de voz consome os [SDK de voz](speech-sdk.md). Ele usa o SDK de voz para enviar o áudio que é processado pelo nosso algoritmo de processamento de áudio avançado da matriz de microfone do dispositivo para o [serviço de voz](overview.md). Ele usa o áudio multicanal para oferecer extremidade campo mais preciso [reconhecimento de fala](speech-to-text.md) através de supressão de ruído, cancelamento de eco, beamforming e dereverberation.

Também pode utilizar o SDK de dispositivos de voz para criar dispositivos de ambiente que têm seu próprio [personalizado do word de reativação](speech-devices-sdk-create-kws.md)— para que a indicação de que inicia uma interação do utilizador é exclusiva para a sua marca. 

O SDK de dispositivos de voz facilita a uma variedade de cenários habilitados para voz, como a unidade através de sistemas de ordenação, assistentes na loja ou in-home e alto-falantes inteligentes. Pode responder a utilizadores com texto, falar para si num padrão ou [voz personalizada](how-to-customize-voice-font.md), forneça os resultados da pesquisa, [traduzir](speech-translation.md) para outros idiomas e muito mais. Estamos ansiosos por ver o que cria!

## <a name="development-kit-providers"></a>Fornecedores do kit de desenvolvimento

Atualmente, esses designs de referência completa, ponto-a-ponto sistema estão disponíveis: 

|||
|-|-|
|[![Logótipo ROOBO](media/speech-devices-sdk/roobo-logo.png)](http://ddk.roobo.com/)|ROOBO fornece completa artificial intelligence soluções de sistema (IA) para aparelhos electric domésticos, automóveis, robôs, toys e outros setores. Estruturas de referência do ROOBO reduzem significativamente o desenvolvimento time-to-market por meio da integração com o serviço Microsoft Speech. [Visite ROOBO](http://ddk.roobo.com/).|

## <a name="next-steps"></a>Passos Seguintes

Para começar, obtenha uma [conta gratuita do Azure](https://azure.microsoft.com/free/ai/) e inscreva-se para o SDK de dispositivos de voz.

> [!div class="nextstepaction"]
> [Inscreva-se o SDK de dispositivos de voz](get-speech-devices-sdk.md)

