---
title: Desenvolvimento em equipa com o Azure Dev Spaces com .NET Core e VS Code | Microsoft Docs
titleSuffix: Azure Dev Spaces
services: azure-dev-spaces
ms.service: azure-dev-spaces
ms.component: azds-kubernetes
author: zr-msft
ms.author: zarhoads
ms.date: 07/09/2018
ms.topic: tutorial
description: Desenvolvimento rápido da Kubernetes com contentores e microsserviços no Azure
keywords: Docker, Kubernetes, Azure, AKS, Azure Kubernetes Service, contentores
ms.openlocfilehash: 4357c3a2e13e0eda2eb1d8c0071a21ed21aa36ef
ms.sourcegitcommit: 275eb46107b16bfb9cf34c36cd1cfb000331fbff
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/15/2018
ms.locfileid: "51705740"
---
# <a name="team-development-with-azure-dev-spaces"></a>Desenvolvimento em Equipa com o Azure Dev Spaces

Neste tutorial, irá aprender a utilizar vários espaços de desenvolvimento para trabalhar simultaneamente em ambientes de desenvolvimento diferentes, mantendo o trabalho separado em espaços de desenvolvimento separados no mesmo cluster.

## <a name="call-a-service-running-in-a-separate-container"></a>Chamar um serviço em execução num contentor separado

Nesta secção, vai criar um segundo serviço, `mywebapi` a ser chamado por `webfrontend`. Cada serviço vai ser executado em contentores separados. Em seguida, vai realizar a depuração em ambos os contentores.

![Vários contentores](media/common/multi-container.png)

### <a name="download-sample-code-for-mywebapi"></a>Transfira o código de exemplo para *mywebapi*
Para ser mais rápido, vamos transferir código de exemplo de um repositório do GitHub. Aceda a https://github.com/Azure/dev-spaces e selecione **Clone or Download** (Clonar ou Transferir) para transferir o repositório do GitHub. O código para esta secção está em `samples/dotnetcore/getting-started/mywebapi`.

### <a name="run-mywebapi"></a>Execute *mywebapi*
1. Abra a pasta `mywebapi` numa *janela separada do VS Code*.
1. Abra a **Paleta de Comandos** (através do menu **Ver | Paleta de Comandos**), e utilize o preenchimento automático para escrever e selecione este comando: `Azure Dev Spaces: Prepare configuration files for Azure Dev Spaces`. Este comando não deve ser confundido com o comando `azds prep`, que configura o projeto para implementação.
1. Prima F5 e aguarde que o serviço seja criado e implementado. Saberá que está pronto quando for apresentada a barra de depuração do VS Code.
1. O URL de ponto final será algo parecido com http://localhost:\<portnumber\>. **Sugestão: a barra de estado do VS Code apresentará um URL clicável.** Poderá parecer que o contentor está a ser executado localmente. Contudo, na verdade, está a ser executado no nosso espaço de programador no Azure. O motivo para o endereço localhost é porque `mywebapi` não definiu quaisquer pontos finais públicos e apenas pode ser acedido a partir da instância do Kubernetes. Para sua comodidade e para facilitar a interação com o serviço privado da sua máquina local, o Azure Dev Spaces cria um túnel SSH temporário para o contentor em execução no Azure.
1. Quando `mywebapi` estiver pronto, abra o browser no endereço localhost. Anexe `/api/values` ao URL para invocar a API GET predefinida para `ValuesController`. 
1. Se todos os passos forem concluídos com êxito, deve conseguir ver uma resposta a partir do serviço `mywebapi`.

### <a name="make-a-request-from-webfrontend-to-mywebapi"></a>Efetue um pedido de *webfrontend* para *mywebapi*
Vamos agora escrever código em `webfrontend` que efetua um pedido a `mywebapi`.
1. Mude para a janela do VS Code para `webfrontend`.
1. *Substitua* pelo código para o método About:

    ```csharp
    public async Task<IActionResult> About()
    {
        ViewData["Message"] = "Hello from webfrontend";
        
        using (var client = new System.Net.Http.HttpClient())
            {
                // Call *mywebapi*, and display its response in the page
                var request = new System.Net.Http.HttpRequestMessage();
                request.RequestUri = new Uri("http://mywebapi/api/values/1");
                if (this.Request.Headers.ContainsKey("azds-route-as"))
                {
                    // Propagate the dev space routing header
                    request.Headers.Add("azds-route-as", this.Request.Headers["azds-route-as"] as IEnumerable<string>);
                }
                var response = await client.SendAsync(request);
                ViewData["Message"] += " and " + await response.Content.ReadAsStringAsync();
            }

        return View();
    }
    ```

O exemplo de código anterior reencaminha o cabeçalho `azds-route-as` do pedido a receber para o pedido a enviar. Verá posteriormente como isto ajuda as equipas de desenvolvimento de colaboração.

### <a name="debug-across-multiple-services"></a>Depurar em vários serviços
1. Neste momento, `mywebapi` ainda deve estar em execução com o depurador anexado. Se não for esse o caso, prima F5 no projeto `mywebapi`.
1. Defina um ponto de interrupção no método `Get(int id)` que processa pedidos GET `api/values/{id}`.
1. No projeto `webfrontend`, defina um ponto de interrupção antes de ser enviado um pedido GET para `mywebapi/api/values`.
1. Prima F5 no projeto `webfrontend`.
1. Invoque a aplicação Web e siga o código em ambos os serviços.
1. Na aplicação Web, a página About (Sobre) apresentará uma mensagem concatenada pelos dois serviços: "Hello from webfrontend and Hello from mywebapi".


Já está! Tem agora uma aplicação com vários contentores, na qual cada contentor pode ser desenvolvido e implementado separadamente.

## <a name="learn-about-team-development"></a>Saiba mais sobre desenvolvimento em equipa

[!INCLUDE [](../../includes/team-development-1.md)]

Vamos vê-lo em ação. Vá para a janela do VS Code para `mywebapi` e faça uma edição de código no método `string Get(int id)`, por exemplo:

```csharp
[HttpGet("{id}")]
public string Get(int id)
{
    return "mywebapi now says something new";
}
```


[!INCLUDE [](../../includes/team-development-2.md)]

### <a name="well-done"></a>Já está!
Concluiu o guia de introdução! Aprendeu a:

> [!div class="checklist"]
> * Configurar os Espaços de Programador do Azure com um cluster Kubernetes gerido no Azure.
> * Desenvolver iterativamente código em contentores.
> * Desenvolver de modo independente dois serviços separados e utilizar a deteção do serviço DNS de Kubernetes para fazer uma chamada para outro serviço.
> * Desenvolver e testar de forma produtiva o seu código num ambiente de equipa.

Agora que explorou os Espaços de Programação do Azure, [partilhe o seu espaço de programação com um membro da equipa](how-to/share-dev-spaces.md) e ajude-o a ver como é fácil colaborar em conjunto.

## <a name="clean-up"></a>Limpeza
Para eliminar completamente uma instância dos Espaços de Programador do Azure num cluster, incluindo todos os espaços de programador e serviços em execução, utilize o comando `az aks remove-dev-spaces`. Não se esqueça que esta ação é irreversível. Pode adicionar suporte aos Espaços de Programador do Azure novamente no cluster, mas será como se estivesse novamente a começar. Os seus serviços e espaços antigos não serão restaurados.

O exemplo a seguir lista os controladores dos Espaços de Programador do Azure na sua subscrição ativa e, em seguida, elimina o controlador do mesmo que está associado ao cluster do AKS "myaks" do AKS no grupo de recursos "myaks-rg".

```cmd
    azds controller list
    az aks remove-dev-spaces --name myaks --resource-group myaks-rg
```
