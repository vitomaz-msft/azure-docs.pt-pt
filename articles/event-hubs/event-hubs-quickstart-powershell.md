---
title: Início Rápido do Azure - Criar um hub de eventos com o PowerShell | Microsoft Docs
description: Este início rápido descreve como criar um hub de eventos com o Azure PowerShell e, em seguida, enviar e receber eventos com o SDK .NET Standard.
services: event-hubs
author: ShubhaVijayasarathy
manager: timlt
editor: ''
ms.service: event-hubs
ms.devlang: na
ms.topic: quickstart
ms.custom: mvc
ms.date: 08/16/2018
ms.author: shvija
ms.openlocfilehash: 9f0a6a910ab9b9589e09b6c8e1e7b16f7e63c5c8
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/07/2018
ms.locfileid: "51258974"
---
# <a name="quickstart-create-an-event-hub-using-azure-powershell"></a>Início Rápido: Criar um hub de eventos com o Azure PowerShell

Os Hubs de Eventos do Azure são uma plataforma de fluxo de Macrodados e um serviço de ingestão de eventos capaz de receber e processar milhões de eventos por segundo. Os Hubs de Eventos podem processar e armazenar eventos, dados ou telemetria produzidos por dispositivos e software distribuído. Os dados enviados para um hub de eventos podem ser transformados e armazenados em qualquer fornecedor de análise em tempo real ou adaptadores de armazenamento/criação de batches. Para uma descrição geral detalhada dos Hubs de Eventos, veja [Descrição geral dos Hubs de Eventos](event-hubs-about.md) e [Funcionalidades dos Hubs de Eventos](event-hubs-features.md).

Neste início rápido, vai criar um hub de eventos com o Azure PowerShell.

## <a name="prerequisites"></a>Pré-requisitos

Para concluir este tutorial, confirme que tem:

- Subscrição do Azure. Se não tiver uma, [crie uma conta gratuita][] antes de começar.
- [Visual Studio 2017 Atualização 3 (versão 15.3, 26730.01)](https://www.visualstudio.com/vs) ou posterior.
- [SDK .NET Standard](https://www.microsoft.com/net/download/windows), versão 2.0 ou posterior.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Se estiver a utilizar o PowerShell localmente, tem de executar a versão mais recente do PowerShell para concluir este início rápido. Se precisar de instalar ou atualizar, veja [Instalar e Configurar o Azure PowerShell](https://docs.microsoft.com/powershell/azure/install-azurerm-ps?view=azurermps-5.7.0).

## <a name="create-a-resource-group"></a>Criar um grupo de recursos

Um grupo de recursos é uma coleção lógica dos recursos do Azure e precisa de um grupo de recursos para criar um hub de eventos. 

O exemplo seguinte cria um grupo de recursos na região E.U.A Leste. Substitua `myResourceGroup` pelo nome do grupo de recursos que pretende utilizar:

```azurepowershell-interactive
New-AzureRmResourceGroup –Name myResourceGroup –Location eastus
```

## <a name="create-an-event-hubs-namespace"></a>Criar um espaço de nomes dos Hubs de Eventos

Assim que o seu grupo de recursos é feito, crie um espaço de nomes dos Hubs de Eventos dentro desse grupo de recursos. Um espaço de nomes dos Hubs de Eventos oferece um nome de domínio completamente qualificado e exclusivo, no qual pode criar o seu hub de eventos. Substitua `namespace_name` por um nome exclusivo para o seu espaço de nomes:

```azurepowershell-interactive
New-AzureRmEventHubNamespace -ResourceGroupName myResourceGroup -NamespaceName namespace_name -Location eastus
```

## <a name="create-an-event-hub"></a>Criar um hub de eventos

Agora que tem um espaço de nomes dos Hubs de Eventos, crie um hub de eventos nesse espaço de nomes:

```azurepowershell-interactive
New-AzureRmEventHub -ResourceGroupName myResourceGroup -NamespaceName namespace_name -EventHubName eventhub_name
```

Parabéns! Utilizou o Azure PowerShell para criar um espaço de nomes dos Hubs de Eventos e um hub de eventos nesse espaço de nomes. 

## <a name="next-steps"></a>Passos seguintes

Neste artigo, criou o espaço de nomes dos Hubs de Eventos e utilizou exemplos de aplicações para enviar e receber eventos do hub de eventos. Para obter instruções passo a passo para enviar eventos para (ou) receber eventos de um hub de eventos, veja os seguintes tutoriais: 

- **Enviar eventos para um hub de eventos**: [.NET Standard](event-hubs-dotnet-standard-getstarted-send.md), [.NET Framework](event-hubs-dotnet-framework-getstarted-send.md), [Java](event-hubs-java-get-started-send.md), [Python](event-hubs-python-get-started-send.md), [Node.js](event-hubs-node-get-started-send.md), [Go](event-hubs-go-get-started-send.md), [C](event-hubs-c-getstarted-send.md)
- **Receber eventos de um hub de eventos**: [.NET Standard](event-hubs-dotnet-standard-getstarted-receive-eph.md), [.NET Framework](event-hubs-dotnet-framework-getstarted-receive-eph.md), [Java](event-hubs-java-get-started-receive-eph.md), [Python](event-hubs-python-get-started-receive.md), [Node.js](event-hubs-node-get-started-receive.md), [Go](event-hubs-go-get-started-receive-eph.md), [Apache Storm](event-hubs-storm-getstarted-receive.md)

[crie uma conta gratuita]: https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio
[Install and Configure Azure PowerShell]: https://docs.microsoft.com/powershell/azure/install-azurerm-ps
[New-AzureRmResourceGroup]: https://docs.microsoft.com/powershell/module/azurerm.resources/new-azurermresourcegroup
[fully qualified domain name]: https://wikipedia.org/wiki/Fully_qualified_domain_name
[3]: ./media/event-hubs-quickstart-powershell/sender1.png
[4]: ./media/event-hubs-quickstart-powershell/receiver1.png
[5]: ./media/event-hubs-quickstart-powershell/metrics.png
