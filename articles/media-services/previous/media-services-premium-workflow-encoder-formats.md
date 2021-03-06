---
title: Media Encoder Premium Workflow formatos e codecs | Documentos da Microsoft
description: Este tópico fornece uma visão geral dos formatos de fluxo de trabalho de Premium de codificador de multimédia formatos e codecs do
services: media-services
documentationcenter: ''
author: juliako
manager: femila
editor: ''
ms.assetid: b197fce8-3b9b-4189-8d08-486810c0426f
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/30/2018
ms.author: juliako;anilmur
ms.openlocfilehash: 337ee0edc3d6e644415b2b3f7524d829d0e3c692
ms.sourcegitcommit: 1d3353b95e0de04d4aec2d0d6f84ec45deaaf6ae
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/30/2018
ms.locfileid: "50246471"
---
# <a name="media-encoder-premium-workflow-formats-and-codecs"></a>Media Encoder Premium Workflow formatos e codecs do
> [!NOTE]
> Para perguntas de codificador premium, envie um e-mail mepd@microsoft.com.
> 
> Processador de multimédia do Media Encoder Premium Workflow debatido neste tópico não está disponível na China. 
> 
> 

Este documento contém uma lista de formatos de ficheiro de entrada e saída e codecs que são suportados pela versão de pré-visualização pública dos **Media Encoder Premium Workflow** codificador.

[E Codecs de formatos de entrada de Worflow de Premium de codificador de multimédia](#input_formats)

[E Codecs de formatos de saída Worflow de Premium de codificador de multimédia](#output_formats)

**Media Encoder Premium Workflow** suporta legendagem de áudio descritas [isso](#closed_captioning) secção. 

## <a id="input_formats"></a>Formatos e Codecs de entrada de fluxo de trabalho de Premium de codificador de multimédia
A seção a seguir lista os codecs e formatos de arquivo que este processador de multimédia suporta como entrada.

### <a name="input-containerfile-formats"></a>Formatos de arquivo/contentor de entrada
* Adobe® Flash® F4V
* FICHEIROS DO MXF/SMPTE 377M
* GXF
* Fluxos de transporte de MPEG-2
* Fluxos do programa de MPEG-2
* MPEG-4/MP4
* Windows Media/ASF
* AVI (descomprimido 8 bits/10 bits)

### <a name="input-video-codecs"></a>Codecs de vídeo de entrada
* AVC 8 bits/10-bits, até 4:2:2, incluindo AVCIntra
* Avid DNxHD (no MXF)
* DVCPro/DVCProHD (no MXF)
* HEVC/H.265, o principal e o perfil de Main 10
* JPEG2000
* MPEG-2 (até perfil 422 e alto nível; incluindo variantes como XDCAM, XDCAM HD, XDCAM IMX, CableLabs® e D10)
* MPEG-1
* Windows Media vídeo/VC-1

### <a name="input-audio-codecs"></a>Codecs de áudio de entrada
* AES (SMPTE 331m e 302 M, AES3 2003)
* Dolby® E
* Dolby® Digital (AC3)
* AAC (AAC-LC, AAC-HE e AAC-HEv2; até 5.1)
* MPEG camada 2
* MP3 (MPEG-1 camada de áudio 3)
* Áudio de suporte de dados do Windows
* WAV/PCM

## <a id="output_format"></a>E Codecs do codificador de multimédia Premium formatos de saída de fluxo de trabalho
A seção a seguir lista os codecs e formatos de arquivo que são suportados como saída deste processador de multimédia.

### <a name="output-containerfile-formats"></a>Formatos de arquivo/contêiner de saída
* Adobe® Flash® F4V
* Ficheiros do MXF (OP1a, XDCAM e AS02)
* O protocolo DPP (incluindo AS11)
* GXF
* MPEG-4/MP4
* Windows Media/ASF
* AVI (descomprimido 8 bits/10 bits)
* Formato de ficheiro transmissão em fluxo uniforme (PIFF 1.3)
* MPEG-TS 

### <a name="output-video-codecs"></a>Codecs de vídeo de saída
* AVC (H.264; de 8 bits; até o perfil de alto nível 5.2; 4 K Ultra em alta definição; Dentro de AVC)
* Avid DNxHD (no MXF)
* DVCPro/DVCProHD (no MXF)
* MPEG-2 (até perfil 422 e alto nível; incluindo variantes como XDCAM, XDCAM HD, XDCAM IMX, CableLabs® e D10)
* MPEG-1
* Windows Media vídeo/VC-1
* Criação de miniaturas de JPEG

### <a name="output-audio-codecs"></a>Codecs de áudio de saída
* AES (SMPTE 331m e 302 M, AES3 2003)
* Dolby® Digital (AC3)
* Dolby® Digital Plus (E-AC3) até 7.1
* AAC (AAC-LC, AAC-HE e AAC-HEv2; até 5.1)
* MPEG camada 2
* MP3 (MPEG-1 camada de áudio 3)
* Áudio de suporte de dados do Windows

>[!NOTE]
>Se a codificar em Dolby® Digital (AC3), a saída só pode ser escrita em ficheiros MP4 de ISO.

## <a id="closed_captioning"></a>Suporte para legendagem de áudio
Diante de ingestão **Media Encoder Premium Workflow** suporta:

1. Ficheiros de SCC
2. Ficheiros de SMPTE-TT
3. Legenda oculta CEA-608/legenda oculta CEA-708 – executado como dados de utilizador (SEI o mensagens dos fluxos elementares H.264, ATSC/53, SCTE20) ou executado como dados auxiliares nos ficheiros do MXF/GXF
4. Arquivos de legenda STL

Na saída, as seguintes opções estão disponíveis:

1. Legenda oculta CEA-608 para tradução de legenda oculta CEA-708
2. Legenda oculta CEA-608/legenda oculta CEA-708 pass-through (incorporado nas mensagens SEI de fluxos de elementares H.264 ou executadas como auxiliares dados nos ficheiros do MXF)
3. SCC
4. SMPTE excedeu o tempo limite de texto (de origem legenda oculta CEA-608 por SMPTE RP2052, incluindo a criação de ficheiro DFXP)
5. Ficheiro de legenda de SRT
6. Fluxos do DVB subtítulo

Nota: todos os formatos de saída acima não são suportados para a entrega por meio de transmissão em fluxo nos serviços de multimédia do Azure.

## <a name="known-issues"></a>Problemas conhecidos
Se o seu vídeo de entrada não contém legendagem de áudio, a saída Asset ainda conterá um ficheiro vazio do TTML. 

## <a name="media-services-learning-paths"></a>Percursos de aprendizagem dos Media Services
[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Enviar comentários
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

