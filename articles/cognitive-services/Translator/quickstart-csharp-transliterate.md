---
title: 'Início Rápido: converter script de texto, C# – Tradução de Texto'
titleSuffix: Azure Cognitive Services
description: Neste início rápido, vai aprender a transcrição de texto (converter) a partir de um script para outro e usando a API de REST de texto do Translator e .NET Core. Neste exemplo, o japonês é transliterado para utilizar o alfabeto latino.
services: cognitive-services
author: erhopf
manager: cgronlun
ms.service: cognitive-services
ms.component: translator-text
ms.topic: quickstart
ms.date: 11/21/2018
ms.author: erhopf
ms.openlocfilehash: fcf913762eb883d299c93e4579c9c81b03739ddb
ms.sourcegitcommit: beb4fa5b36e1529408829603f3844e433bea46fe
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/22/2018
ms.locfileid: "52290880"
---
# <a name="quickstart-transliterate-text-with-the-translator-text-rest-api-c"></a>Início Rápido: transliterar texto com a API REST de Texto do Microsoft Translator (C#)

Neste início rápido, irá aprender a transcrição de texto (converter) a partir de um script para outro com o .NET Core (C#) e a API de REST de texto do Translator. No exemplo apresentado, o japonês é transliterado para utilizar o alfabeto latino.

Este início rápido requer uma [conta dos Serviços Cognitivos do Azure](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) com um recurso de Tradução de Texto. Se não tiver uma conta, pode utilizar a [avaliação gratuita](https://azure.microsoft.com/try/cognitive-services/) para obter uma chave de subscrição.

## <a name="prerequisites"></a>Pré-requisitos

* [SDK do .NET](https://www.microsoft.com/net/learn/dotnet/hello-world-tutorial)
* [Pacote de NuGet Json.NET](https://www.nuget.org/packages/Newtonsoft.Json/)
* [Visual Studio](https://visualstudio.microsoft.com/downloads/), [Visual Studio Code](https://code.visualstudio.com/download), ou no seu editor de texto favorito
* Uma chave de subscrição do Azure para o serviço de voz

## <a name="create-a-net-core-project"></a>Criar um projeto .NET Core

Abra uma nova linha de comandos (ou sessão de terminal) e execute estes comandos:

```console
dotnet new console -o transliterate-sample
cd transliterate-sample
```

O primeiro comando faz duas coisas. Cria uma nova aplicação de consola .NET e cria um diretório chamado `transliterate-sample`. O segundo comando altera-se para o diretório para o seu projeto.

## <a name="add-required-namespaces-to-your-project"></a>Adicionar espaços de nomes necessários ao seu projeto

O `dotnet new console` comando que executou anteriormente criado um projeto, incluindo `Program.cs`. Este ficheiro é onde vai pôr o código da aplicação. Abra `Program.cs`e substitua as instruções de utilização existentes. Essas instruções Certifique-se de que tem acesso a todos os tipos necessários para criar e executar a aplicação de exemplo.

```csharp
using System;
using System.Net.Http;
using System.Text;
using Newtonsoft.Json;
```

## <a name="create-a-function-to-translate-text"></a>Criar uma função para traduzir texto

Dentro de `Program` classe, crie uma função chamada `TransliterateText`. Essa classe encapsula o código usado para chamar o recurso de Translate e imprima o resultado à consola.

```csharp
static void TransliterateText()
{
  /*
   * The code for your call to the translation service will be added to this
   * function in the next few sections.
   */
}
```

## <a name="set-the-subscription-key-host-name-and-path"></a>Definir a chave de subscrição, nome de anfitrião e caminho

Adicionar estas linhas para o `TransliterateText` função. Perceberá que, juntamente com o `api-version`, foi anexou dois parâmetros adicionais para o `route`. Esses parâmetros são usados para definir as saídas de tradução. Neste exemplo, ele é definido como alemão (`de`) e italiano (`it`). Certifique-se de que atualizar o valor da chave de subscrição.

```csharp
string host = "https://api.cognitive.microsofttranslator.com";
string route = "/transliterate?api-version=3.0&language=ja&fromScript=jpan&toScript=latn";
string subscriptionKey = "YOUR_SUBSCRIPTION_KEY";
```

Em seguida, temos de criar e serializar o objeto JSON que inclua o texto que pretende converter. Tenha em mente, pode passar mais do que um objeto no `body` matriz.

```csharp
System.Object[] body = new System.Object[] { new { Text = @"こんにちは" } };
var requestBody = JsonConvert.SerializeObject(body);
```

## <a name="instantiate-the-client-and-make-a-request"></a>Criar uma instância do cliente e fazer um pedido

Estas linhas instanciar o `HttpClient` e o `HttpRequestMessage`:

```csharp
using (var client = new HttpClient())
using (var request = new HttpRequestMessage())
{
  // In the next few sections you'll add code to construct the request.
}
```

## <a name="construct-the-request-and-print-the-response"></a>Construa a solicitação e resposta de impressão

Dentro do `HttpRequestMessage` irá:

* Declarar o método HTTP
* Construir o URI do pedido
* Insira o corpo do pedido (objeto JSON serializado)
* Adicionar cabeçalhos necessários
* Fazer uma solicitação assíncrona
* Imprimir a resposta

Adicione este código para o `HttpRequestMessage`:

```csharp
// Set the method to POST
request.Method = HttpMethod.Post;

// Construct the full URI
request.RequestUri = new Uri(host + route);

// Add the serialized JSON object to your request
request.Content = new StringContent(requestBody, Encoding.UTF8, "application/json");

// Add the authorization header
request.Headers.Add("Ocp-Apim-Subscription-Key", subscriptionKey);

// Send request to Azure service, get response
var response = client.SendAsync(request).Result;
var jsonResponse = response.Content.ReadAsStringAsync().Result;

// Print the response
Console.WriteLine(jsonResponse);
Console.WriteLine("Press any key to continue.");
```

## <a name="put-it-all-together"></a>Juntar tudo

A última etapa é chamar `TransliterateText()` no `Main` função. Localize `static void Main(string[] args)` e adicionar estas linhas:

```csharp
TransliterateText();
Console.ReadLine();
```

## <a name="run-the-sample-app"></a>Execute a aplicação de exemplo

É isso, está pronto para executar a aplicação de exemplo de texto para voz. A partir da linha de comandos (ou sessão de terminal), navegue para o diretório de projeto e execute:

```console
dotnet run
```

## <a name="sample-response"></a>Resposta de amostra

```json
[
    {
        "script": "latn",
        "text": "konnnichiha"
    }
]
```

## <a name="clean-up-resources"></a>Limpar recursos

Não se esqueça de remover quaisquer informações confidenciais do código-fonte da sua aplicação de exemplo, como chaves de subscrição.

## <a name="next-steps"></a>Passos Seguintes

Explore o código de exemplo neste início rápido e outros, incluindo a transliteração e a identificação do idioma, assim como outros projetos de Tradução de Texto no GitHub.

> [!div class="nextstepaction"]
> [Explorar exemplos do C# no GitHub](https://aka.ms/TranslatorGitHub?type=&language=c%23)

## <a name="see-also"></a>Consulte também

* [Traduzir texto](quickstart-csharp-translate.md)
* [Identificar o idioma por entrada](quickstart-csharp-detect.md)
* [Obter traduções alternativas](quickstart-csharp-dictionary.md)
* [Obter uma lista de idiomas suportados](quickstart-csharp-languages.md)
* [Determinar o comprimento das frases a partir de uma entrada](quickstart-csharp-sentences.md)
