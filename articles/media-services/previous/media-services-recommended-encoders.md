---
title: Saiba mais sobre codificadores recomendados pelos serviços de multimédia do Azure | Documentos da Microsoft
description: Saiba mais sobre codificadores recomendados pelos serviços de multimédia
services: media-services
keywords: codificação; codificadores; suporte de dados
author: dbgeorge
manager: johndeu
ms.author: johndeu
ms.date: 09/13/2018
ms.topic: article
ms.service: media-services
ms.openlocfilehash: c90d6a5784fe9d80df4fab304b6122d3fa24d0b5
ms.sourcegitcommit: 616e63d6258f036a2863acd96b73770e35ff54f8
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/14/2018
ms.locfileid: "45605169"
---
# <a name="recommended-on-premises-encoders"></a>Codificadores locais recomendados
Quando em direto de transmissão em fluxo com os serviços de multimédia do Azure, pode especificar como pretende que o canal para receber o fluxo de entrada. Se optar por utilizar um codificador no local com um canal de codificação em direto, o seu codificador deve enviar um fluxo de velocidade única de alta qualidade como saída. Se optar por utilizar um codificador no local com um pass-through canal, seu codificador deve enviar um fluxo de velocidade de transmissão como saída com todas as qualidades de saída desejada. Para obter mais informações, consulte [transmissão em fluxo com codificadores no local do Live](media-services-live-streaming-with-onprem-encoders.md).

Serviços de multimédia do Azure recomenda a utilização de um dos seguintes codificadores em direto que tenham RTMP como saída:
- Suporte de dados de Adobe Flash Live Encoder 3.2
- Haivision Makito X HEVC
- Haivision KB
- Telestream Wirecast 8.1 +
- Telestream Wirecast S
- Setor de Teradek 756
- TriCaster 8000
- Tricaster Mini HD-4
- OBS Studio
- VMIX
- xStream
- Comutador Studio (iOS)

Serviços de multimédia do Azure recomenda a utilização de um dos seguintes codificadores em direto com velocidade de transmissão-MP4 fragmentado (Smooth Streaming) como saída:
- Suporte de dados do Excel Hero em direto e Hero 4K (UHD/HEVC)
- Ateme TITAN em direto
- Codificador de multimédia Digital de Cisco 2200
- Elemental em direto
- Envivio 4Caster C4 fins III
- Imagine Communications Selenio MCP3

> [!NOTE]
> Um codificador em direto pode enviar um fluxo de velocidade de transmissão única para um pass-through canal, mas esta configuração não é recomendada porque ele não permite que a velocidade de transmissão adaptável de transmissão em fluxo para o cliente.

## <a name="how-to-become-an-on-prem-encoder-partner"></a>Como se tornar um parceiro de codificador no local
Como parceiro de codificador de serviços de multimédia do Azure no local, serviços de multimédia promove seu produto ao recomendar seu codificador para os clientes empresariais. Para se tornar um parceiro de codificador no local, tem de verificar a compatibilidade de seu codificador no local com os Media Services. Para fazê-lo, conclua as seguintes verificações:

Passar a verificação de canal
1. Criar ou visite a sua conta de Media Services do Azure
2. Crie e inicie um **pass-through** canal
3. Configure seu codificador para enviar por push um velocidade de transmissão em fluxo em direto.
4. Criar um evento em direto publicado
5. Execute seu codificador em direto para cerca de 10 minutos
6. Pare o evento em direto
7. Criar, iniciar um ponto de final de transmissão em fluxo, utilize um leitor, tal como [leitor de multimédia do Azure](https://ampdemo.azureedge.net/azuremediaplayer.html) assista o elemento arquivado para garantir que essa reprodução tem sem falhas visível para todos os níveis de qualidade (ou em alternativa ver e validar via a URL de pré-visualização durante a sessão em direto antes do passo 6)
8. Registe o ID de recurso publicado o URL de transmissão em fluxo para este arquivo live e as definições e a versão utilizada a partir do seu codificador em direto
9. Repor o estado de canal depois de criar cada exemplo
10. Repita os passos 3 a 9 para todas as configurações suportados por seu codificador (com e sem ad sinalização/legendas/diferentes velocidades de codificação)

Verificação de canal de codificação em direto
1. Criar ou visite a sua conta de Media Services do Azure
2. Crie e inicie um **codificação em direto** canal
3. Configure seu codificador para enviar um fluxo em direto com velocidade de transmissão única.
4. Criar um evento em direto publicado
5. Execute seu codificador em direto para cerca de 10 minutos
6. Pare o evento em direto
7. Criar, iniciar um ponto de final de transmissão em fluxo, utilize um leitor, tal como [leitor de multimédia do Azure](https://ampdemo.azureedge.net/azuremediaplayer.html) assista o elemento arquivado para garantir que essa reprodução tem sem falhas visível para todos os níveis de qualidade (ou em alternativa ver e validar via a URL de pré-visualização durante a sessão em direto antes do passo 6)
8. Registe o ID de recurso publicado o URL de transmissão em fluxo para este arquivo live e as definições e a versão utilizada a partir do seu codificador em direto
9. Repor o estado de canal depois de criar cada exemplo
10. Repita os passos 3 a 9 para todas as configurações suportados por seu codificador (com e sem ad sinalização/legendas/várias velocidades de codificação)

Verificação de longevidade
1. Criar ou visite a sua conta de Media Services do Azure
2. Crie e inicie um **pass-through** canal
3. Configure seu codificador para enviar por push um velocidade de transmissão em fluxo em direto.
4. Criar um evento em direto publicado
5. Execute seu codificador em direto para uma semana ou mais
6. Utilizar como um jogador [leitor de multimédia do Azure](https://ampdemo.azureedge.net/azuremediaplayer.html) para ver a transmissão em direto da hora a hora (ou elemento arquivado) para garantir que essa reprodução tem sem falhas visível
7. Pare o evento em direto
8. Registe o ID de recurso publicado o URL de transmissão em fluxo para este arquivo live e as definições e a versão utilizada a partir do seu codificador em direto

Por último, envie as suas definições gravadas e live parâmetros de arquivo para os serviços de multimédia através do envio de amsstreaming@microsoft.com. Após a receção, serviços de multimédia executa testes de verificação em amostras de seu codificador em direto. Pode contactar os serviços de multimédia com dúvidas sobre este processo.