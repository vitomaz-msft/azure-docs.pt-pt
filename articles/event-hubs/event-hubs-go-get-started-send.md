---
title: Enviar eventos para Hubs de eventos do Azure com Go | Documentos da Microsoft
description: Introdução ao envio de eventos para Hubs de eventos com Go
services: event-hubs
author: ShubhaVijayasarathy
manager: kamalb
ms.service: event-hubs
ms.workload: core
ms.topic: article
ms.date: 10/18/2018
ms.author: shvija
ms.openlocfilehash: f5e30a103b09613caee8e9912a89a5bc2d390f65
ms.sourcegitcommit: 668b486f3d07562b614de91451e50296be3c2e1f
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/19/2018
ms.locfileid: "49458093"
---
# <a name="send-events-to-event-hubs-using-go"></a>Enviar eventos para Hubs de eventos com Go

Os Hubs de Eventos do Azure são uma plataforma de fluxo de Macrodados e um serviço de ingestão de eventos capaz de receber e processar milhões de eventos por segundo. Os Hubs de Eventos podem processar e armazenar eventos, dados ou telemetria produzidos por dispositivos e software distribuído. Os dados enviados para um hub de eventos podem ser transformados e armazenados em qualquer fornecedor de análise em tempo real ou adaptadores de armazenamento/criação de batches. Para uma visão geral detalhada dos Hubs de eventos, consulte [descrição geral dos Hubs de eventos](event-hubs-about.md) e [funcionalidades dos Hubs de eventos](event-hubs-features.md).

Este tutorial descreve como enviar eventos para um hub de eventos de um aplicativo escrito em Go. 

> [!NOTE]
> Pode transferir este guia de introdução como uma amostra a partir do [GitHub](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/eventhubs), substitua `EventHubConnectionString` e `EventHubName` cadeias de caracteres com os seus valores de hub de eventos, e executá-lo. Em alternativa, pode seguir os passos neste tutorial para criar o seu próprio.

## <a name="prerequisites"></a>Pré-requisitos

Para concluir este tutorial, precisa dos seguintes pré-requisitos:

* Vá instalado localmente. Siga [estas instruções](https://golang.org/doc/install) se necessário.
* Os Hubs de eventos espaço de nomes e o event hub existente. Pode criar estas entidades, seguindo as instruções em [este artigo](event-hubs-create.md).

## <a name="create-an-event-hubs-namespace-and-an-event-hub"></a>Criar um espaço de nomes de Hubs de Eventos e um hub de eventos
O primeiro passo consiste em utilizar o [portal do Azure](https://portal.azure.com) para criar um espaço de nomes do tipo Hubs de Eventos e obter as credenciais de gestão de que a sua aplicação precisa para comunicar com o hub de eventos. Para criar um espaço de nomes e um hub de eventos, siga o procedimento [este artigo](event-hubs-create.md), em seguida, continuar com os seguintes passos neste tutorial.

## <a name="install-go-package"></a>Instalar o pacote do Go

Obter o pacote do Go para os Hubs de eventos com `go get` ou `dep`. Por exemplo:

```bash
go get -u github.com/Azure/azure-event-hubs-go
go get -u github.com/Azure/azure-amqp-common-go/...

# or

dep ensure -add github.com/Azure/azure-event-hubs-go
dep ensure -add github.com/Azure/azure-amqp-common-go
```

## <a name="import-packages-in-your-code-file"></a>Importar pacotes em seu arquivo de código

Para importar os pacotes de Go, utilize o seguinte exemplo de código:

```go
import (
    aad "github.com/Azure/azure-amqp-common-go/aad"
    eventhubs "github.com/Azure/azure-event-hubs-go"
)
```

## <a name="create-service-principal"></a>Criar um principal de serviço

Crie um novo principal de serviço ao seguir as instruções em [criar um Azure principal de serviço com a CLI 2.0 do Azure](/cli/azure/create-an-azure-service-principal-azure-cli). Guarde as credenciais fornecidas no seu ambiente com os seguintes nomes. O Azure SDK para Go e os pacotes de Hubs de eventos estão pré-configuradas para procurar por estes nomes de variáveis:

```bash
export AZURE_CLIENT_ID=
export AZURE_CLIENT_SECRET=
export AZURE_TENANT_ID=
export AZURE_SUBSCRIPTION_ID= 
```

Agora, crie um fornecedor de autorização para o seu cliente de Hubs de eventos que utiliza estas credenciais:

```go
tokenProvider, err := aad.NewJWTProvider(aad.JWTProviderWithEnvironmentVars())
if err != nil {
    log.Fatalf("failed to configure AAD JWT provider: %s\n", err)
}
```

## <a name="create-event-hubs-client"></a>Crie os Hubs de eventos do cliente

O código a seguir cria um cliente dos Hubs de eventos:

```go
hub, err := eventhubs.NewHub("namespaceName", "hubName", tokenProvider)
ctx := context.WithTimeout(context.Background(), 10 * time.Second)
defer hub.Close(ctx)
if err != nil {
    log.Fatalf("failed to get hub %s\n", err)
}
```

## <a name="send-messages"></a>Enviar mensagens

No seguinte fragmento, utilize (1) para enviar mensagens interativamente a partir de um terminal ou (2) para enviar mensagens dentro de seu programa:

```go
// 1. send messages at the terminal
ctx = context.Background()
reader := bufio.NewReader(os.Stdin)
for {
    fmt.Printf("Input a message to send: ")
    text, _ := reader.ReadString('\n')
    hub.Send(ctx, eventhubs.NewEventFromString(text))
}

// 2. send messages within program
ctx = context.Background()
hub.Send(ctx, eventhubs.NewEventFromString("hello Azure!")
```

## <a name="extras"></a>Extras

Obter os IDs das partições no seu hub de eventos:

```go
info, err := hub.GetRuntimeInformation(ctx)
if err != nil {
    log.Fatalf("failed to get runtime info: %s\n", err)
}
log.Printf("got partition IDs: %s\n, info.PartitionIDs)
```

Execute a aplicação para enviar eventos para o hub de eventos. 

Parabéns! Enviou agora mensagens para um hub de eventos.

## <a name="next-steps"></a>Passos Seguintes
Neste início rápido, enviou mensagens para um hub de eventos através de Go. Para saber como receber eventos de um hub de eventos através de Go, veja [receber eventos do hub de eventos - Go](event-hubs-go-get-started-receive-eph.md).

<!-- Links -->
[Event Hubs overview]: event-hubs-about.md
[free account]: https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio
