---
title: Enviar eventos personalizados do Azure Event Grid para a ligação híbrida | Microsoft Docs
description: Utilize o Azure Event Grid e a CLI do Azure para publicar um tópico e subscrever esse evento. Uma ligação híbrida serve para o ponto final.
services: event-grid
keywords: ''
author: tfitzmac
ms.author: tomfitz
ms.date: 10/30/2018
ms.topic: tutorial
ms.service: event-grid
ms.openlocfilehash: 488f4e09e329ee41fb307dc3579e76b5378d3a9f
ms.sourcegitcommit: 6678e16c4b273acd3eaf45af310de77090137fa1
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/01/2018
ms.locfileid: "50747784"
---
# <a name="route-custom-events-to-azure-relay-hybrid-connections-with-azure-cli-and-event-grid"></a>Encaminhar eventos personalizados para as Ligações Híbridas do Azure Relay com a CLI do Azure e o Event Grid

O Azure Event Grid é um serviço de eventos para a cloud. As Ligações Híbridas do Azure Relay são um dos processadores de eventos suportados. Utiliza ligações híbridas como o processador de eventos quando necessita de processar eventos de aplicações que não tenham um ponto final público. Estas aplicações podem estar dentro da sua rede empresarial. Neste artigo, a CLI do Azure serve para criar um tópico personalizado, subscrever o tópico personalizado e acionar o evento para ver o resultado. Os eventos são enviados para a ligação híbrida.

## <a name="prerequisites"></a>Pré-requisitos

Este artigo pressupõe que já tem uma ligação híbrida e uma aplicação de serviço de escuta. Para começar a trabalhar com ligações híbridas, veja [Introdução às Ligações Híbridas de Reencaminhamento - .NET](../service-bus-relay/relay-hybrid-connections-dotnet-get-started.md) ou [Introdução às Ligações Híbridas de Reencaminhamento - Nó](../service-bus-relay/relay-hybrid-connections-node-get-started.md).

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

[!INCLUDE [event-grid-preview-feature-note.md](../../includes/event-grid-preview-feature-note.md)]

## <a name="create-a-resource-group"></a>Criar um grupo de recursos

Os tópicos do Event Grid são recursos do Azure e têm de ser colocados num grupo de recursos do Azure. Um grupo de recursos é uma coleção lógica na qual os recursos do Azure são implementados e geridos.

Crie um grupo de recursos com o comando [az group create](/cli/azure/group#az-group-create). 

O exemplo seguinte cria um grupo de recursos com o nome *gridResourceGroup* na localização *westus2*.

```azurecli-interactive
az group create --name gridResourceGroup --location westus2
```

## <a name="create-a-custom-topic"></a>Criar um tópico personalizado

Um tópico do Event Grid fornece um ponto final definido pelo utilizador no qual publica os eventos. O exemplo seguinte cria o tópico personalizado no seu grupo de recursos. Substitua `<topic_name>` por um nome exclusivo para o seu tópico personalizado. O nome do tópico do Event Grid deve ser exclusivo, porque é representado por uma entrada DNS.

```azurecli-interactive
# if you have not already installed the extension, do it now.
# This extension is required for preview features.
az extension add --name eventgrid

az eventgrid topic create --name <topic_name> -l westus2 -g gridResourceGroup
```

## <a name="subscribe-to-a-custom-topic"></a>Subscrever um tópico personalizado

Subscreva um tópico do Event Grid para comunicar ao Event Grid os eventos que quer controlar. O exemplo seguinte subscreve o tópico personalizado que criou e transmite o ID do recurso da ligação híbrida ao ponto final. O ID da ligação híbrida está no formato:

`/subscriptions/<subscription-id>/resourceGroups/<resource-group-name>/providers/Microsoft.Relay/namespaces/<relay-namespace>/hybridConnections/<hybrid-connection-name>`

O script seguinte obtém o ID do recurso do espaço de nomes de reencaminhamento. Constrói o ID para a ligação híbrida e subscreve um tópico do Event Grid. O script define o tipo de ponto final `hybridconnection` e utiliza o ID da ligação híbrida para o ponto final.

```azurecli-interactive
relayname=<namespace-name>
relayrg=<resource-group-for-relay>
hybridname=<hybrid-name>

relayid=$(az resource show --name $relayname --resource-group $relayrg --resource-type Microsoft.Relay/namespaces --query id --output tsv)
hybridid="$relayid/hybridConnections/$hybridname"
topicid=$(az eventgrid topic show --name <topic_name> -g gridResourceGroup --query id --output tsv)

az eventgrid event-subscription create \
  --source-resource-id $topicid \
  --name <event_subscription_name> \
  --endpoint-type hybridconnection \
  --endpoint $hybridid \
  --expiration-date "<yyyy-mm-dd>"
```

Observe que está definida uma [data de expiração](concepts.md#event-subscription-expiration) para a subscrição.

## <a name="create-application-to-process-events"></a>Criar aplicação para processar eventos

Precisa de uma aplicação que possa recuperar eventos a partir da ligação híbrida. O [exemplo do Consumidor de Ligação Híbrida do Microsoft Azure Event Grid para C#](https://github.com/Azure-Samples/event-grid-dotnet-hybridconnection-destination) realiza essa operação. Já terminou os passos dos pré-requisitos.

1. Certifique-se de que tem a versão 15.5 ou posterior do Visual Studio 2017.

1. Clone o repositório para o seu computador local.

1. Carregue o projeto HybridConnectionConsumer no Visual Studio.

1. Em Program.cs, substitua `<relayConnectionString>` e `<hybridConnectionName>` pela cadeia de ligação de reencaminhamento e pelo nome da ligação híbrida que criou.

1. Compilar e executar a aplicação a partir do Visual Studio.

## <a name="send-an-event-to-your-topic"></a>Enviar um evento para o seu tópico

Vamos acionar um evento para ver como o Event Grid distribui a mensagem para o ponto final. Este artigo mostra como utilizar a CLI do Azure para acionar o evento. Em alternativa, pode utilizar a [aplicação do publicador do Event Grid](https://github.com/Azure-Samples/event-grid-dotnet-publish-consume-events/tree/master/EventGridPublisher).

Em primeiro lugar, vamos obter o URL e a chave do tópico personalizado. Novamente, utilize o nome do tópico personalizado de `<topic_name>`.

```azurecli-interactive
endpoint=$(az eventgrid topic show --name <topic_name> -g gridResourceGroup --query "endpoint" --output tsv)
key=$(az eventgrid topic key list --name <topic_name> -g gridResourceGroup --query "key1" --output tsv)
```

Para simplificar este artigo, vai utilizar dados do evento de exemplo para enviar para o tópico personalizado. Normalmente, uma aplicação ou serviço do Azure enviaria os dados do evento. CURL é um utilitário que envia os pedidos HTTP. Neste artigo, utilize o CURL para enviar o evento para o tópico personalizado.

```azurecli-interactive
event='[ {"id": "'"$RANDOM"'", "eventType": "recordInserted", "subject": "myapp/vehicles/motorcycles", "eventTime": "'`date +%Y-%m-%dT%H:%M:%S%z`'", "data":{ "make": "Ducati", "model": "Monster"},"dataVersion": "1.0"} ]'
curl -X POST -H "aeg-sas-key: $key" -d "$event" $endpoint
```

A aplicação de serviço de escuta deverá receber a mensagem de evento.

## <a name="clean-up-resources"></a>Limpar recursos
Se quiser continuar a trabalhar com este evento, não limpe os recursos criados neste artigo. Caso contrário, utilize o comando seguinte para eliminar os recursos que criou neste artigo.

```azurecli-interactive
az group delete --name gridResourceGroup
```

## <a name="next-steps"></a>Passos seguintes

Agora que sabe como criar tópicos e subscrições de eventos, saiba mais sobre o que o Event Grid pode ajudá-lo a fazer:

- [Sobre o Event Grid](overview.md)
- [Encaminhar eventos de armazenamento de Blobs para um ponto final Web personalizado](../storage/blobs/storage-blob-event-quickstart.md?toc=%2fazure%2fevent-grid%2ftoc.json)
- [Monitorizar alterações de máquina virtual com o Azure Event Grid e Logic Apps](monitor-virtual-machine-changes-event-grid-logic-app.md)
- [Transmitir macrodados em fluxo para um armazém de dados](event-grid-event-hubs-integration.md)
