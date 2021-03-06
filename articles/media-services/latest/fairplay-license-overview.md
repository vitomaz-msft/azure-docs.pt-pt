---
title: Suporte de licença do Apple FairPlay e dos serviços de multimédia do Azure | Documentos da Microsoft
description: Este tópico fornece uma descrição geral de uma licença do Apple FairPlay requisitos e a configuração.
author: juliako
manager: femila
editor: ''
services: media-services
documentationcenter: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/15/2018
ms.author: juliako
ms.openlocfilehash: 19f382de3ffe11253005f5fa2874ee817abaeed3
ms.sourcegitcommit: 3a7c1688d1f64ff7f1e68ec4bb799ba8a29a04a8
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/17/2018
ms.locfileid: "49376759"
---
# <a name="apple-fairplay-license-requirements-and-configuration"></a>Configuração e requisitos de licença do Apple FairPlay 

Serviços de multimédia do Azure permite-lhe encriptar o seu conteúdo HLS com **Apple FairPlay** (AES-128 CBC). Serviços de multimédia também fornecem um serviço para entregar licenças do FairPlay. Quando um jogador tenta reproduzir o conteúdo protegido por FairPlay, é enviado um pedido para o serviço de entrega de licença para obter uma licença. Se o serviço de licença aprovar o pedido, que emite a licença que é enviada ao cliente e é utilizada para desencriptar e reproduzir o conteúdo especificado.

Serviços de multimédia também fornecem APIs que pode utilizar para configurar as suas licenças do FairPlay. Este tópico aborda os requisitos de licença do FairPlay e demonstra como pode configurar uma **FairPlay** com APIs de Sercies de mídia de licença. 

## <a name="requirements"></a>Requisitos

São necessários os seguintes quando utilizar os serviços de multimédia para encriptar o seu conteúdo HLS com **Apple FairPlay** e utilizar os serviços de multimédia para entregar licenças do FairPlay:

* Inscreva-se com [programa de desenvolvimento da Apple](https://developer.apple.com/).
* Apple exige o proprietário do conteúdo obter o [pacote de implementação](https://developer.apple.com/contact/fps/). Estado que já implementada módulo de segurança da chave (KSM) com os serviços de multimédia, e que está a solicitar o pacote FPS final. Existem instruções no pacote final FPS para gerar a certificação e obter a chave de segredo de aplicação (ASK). Utilize ASK para configurar o FairPlay.
* Os seguintes procedimentos tem de ser definidos no lado de entrega de chaves/licenças de serviços de multimédia:

    * **Certificado da aplicação (CA)**: Este é um ficheiro. pfx que contém a chave privada. Pode criar este ficheiro e criptografá-la com uma palavra-passe. Shoul de ficheiro. pfx de estar no formato Base64.

        Os passos seguintes descrevem como gerar um ficheiro de certificado. pfx para o FairPlay:

        1. Instalar o OpenSSL de https://slproweb.com/products/Win32OpenSSL.html.

            Aceda à pasta onde se o certificado de FairPlay e outros ficheiros fornecidos pela Apple.
        2. Execute o comando seguinte a partir da linha de comandos. Desta forma, o ficheiro. cer para um ficheiro. pem.

            "C:\OpenSSL-Win32\bin\openssl.exe" x509-informar der-no FairPlay.cer-out FairPlay out.pem
        3. Execute o comando seguinte a partir da linha de comandos. Isto converte o ficheiro. pem para um ficheiro. pfx com a chave privada. A palavra-passe para o ficheiro. pfx, em seguida, é pedida ao OpenSSL.

            "C:\OpenSSL-Win32\bin\openssl.exe" pkcs12 -export -out FairPlay-out.pfx -inkey privatekey.pem -in FairPlay-out.pem -passin file:privatekey-pem-pass.txt
            
    * **Palavra-passe do certificado de aplicação**: A palavra-passe para criar o ficheiro. pfx.
    * **PEDIR**: esta chave é recebida quando gera a certificação com o portal de programador da Apple. Cada equipe de desenvolvimento recebe um ASK exclusivo. Guardar uma cópia do ASK e armazene-o num local seguro. Tem de configurar ASK como FairPlayAsk com os Media Services.
    
* Os seguintes procedimentos tem de ser definidos pelo lado do cliente FPS:

  * **Certificado da aplicação (CA)**: Este é um ficheiro de.cer/.der que contém a chave pública, o que o sistema operativo utiliza para criptografar alguns payload. Serviços de multimédia precisa saber sobre isso, porque é necessário pelo jogador. O serviço de entrega de chave descriptografa-lo com a chave privada correspondente.

* Para reproduzir um fluxo encriptado de FairPlay, obtenha respostas fazem real primeiro e, em seguida, gerar um certificado real. Esse processo cria todas as três partes:

  * ficheiro. der
  * ficheiro. pfx
  * palavra-passe para o. pfx

## <a name="fairplay-and-player-apps"></a>FairPlay e leitor de aplicações

Quando o seu conteúdo é encriptado com **Apple FairPlay**, as amostras de áudio e vídeos individuais são encriptadas utilizando o **AES-128 CBC** modo. **FairPlay Streaming** (FPS) está integrado nos sistemas operativos dos dispositivos, com suporte nativo em dispositivos iOS e Apple TV. Safari nos X permite FPS com o suporte de interface Encrypted Media Extensions (EME).

O leitor de multimédia do Azure também oferece suporte a reprodução de FairPlay. Para obter mais informações, consulte [documentação de leitor de multimédia do Azure](https://amp.azure.net/libs/amp/latest/docs/index.html).

Pode desenvolver as suas aplicações de leitor, utilizando o SDK do iOS. Para ser capaz de reproduzir conteúdo de FairPlay, deve implementar o protocolo de troca de licença. Este protocolo não é especificado pela Apple. Cabe a cada aplicação como enviar pedidos de entrega de chave. O serviço de entrega de chave do FairPlay de serviços de multimédia espera o SPC por vir como uma mensagem de mensagem codificada www-form-url, o seguinte formato:

```
spc=<Base64 encoded SPC>
```

## <a name="fairplay-configuration-net-example"></a>Exemplo de .NET de configuração do FairPlay

Pode utilizar a API de serviços de multimédia para configurar as licenças do FairPlay. Quando o jogador tenta reproduzir o conteúdo protegido por FairPlay, é enviado um pedido para o serviço de entrega de licença para obter a licença. Se o serviço de licença aprovar o pedido, o serviço emite a licença. Ele é enviado para o cliente e é utilizado para desencriptar e reproduzir o conteúdo especificado.

> [!NOTE]
> Normalmente, iria querer configurar opções de política do FairPlay apenas uma vez, uma vez que apenas terá um conjunto de uma certificação e um ASK.

O exemplo seguinte utiliza [SDK .NET dos Media Services](https://docs.microsoft.com/dotnet/api/microsoft.azure.management.media.models?view=azure-dotnet) para configurar a licença.

```csharp
private static ContentKeyPolicyFairPlayConfiguration ConfigureFairPlayPolicyOptions()
{

    string askHex = "";
    string FairPlayPfxPassword = "";

    var appCert = new X509Certificate2("FairPlayPfxPath", FairPlayPfxPassword, X509KeyStorageFlags.Exportable);

    byte[] askBytes = Enumerable
        .Range(0, askHex.Length)
        .Where(x => x % 2 == 0)
        .Select(x => Convert.ToByte(askHex.Substring(x, 2), 16))
        .ToArray();

    ContentKeyPolicyFairPlayConfiguration fairPlayConfiguration =
    new ContentKeyPolicyFairPlayConfiguration
    {
        Ask = askBytes,
        FairPlayPfx =
                Convert.ToBase64String(appCert.Export(X509ContentType.Pfx, FairPlayPfxPassword)),
        FairPlayPfxPassword = FairPlayPfxPassword,
        RentalAndLeaseKeyType =
                ContentKeyPolicyFairPlayRentalAndLeaseKeyType
                .PersistentUnlimited,
        RentalDuration = 2249
    };

    return fairPlayConfiguration;
}
```

## <a name="next-steps"></a>Passos Seguintes

Verificar como [proteger com o DRM](protect-with-drm.md)