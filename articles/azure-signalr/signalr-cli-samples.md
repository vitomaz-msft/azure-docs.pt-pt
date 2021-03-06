---
title: Exemplos da CLI do Azure – Serviço Azure SignalR | Microsoft Docs
description: Exemplos da CLI do Azure – Serviço Azure SignalR
services: signalr
documentationcenter: signalr
author: sffamily
manager: cfowler
editor: ''
tags: azure-service-management
ms.service: signalr
ms.devlang: na
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: signalr
ms.date: 04/20/2018
ms.author: zhshang
ms.custom: mvc
ms.openlocfilehash: a1ca02a5e058816c133accf3027f0a951db42cf2
ms.sourcegitcommit: 31241b7ef35c37749b4261644adf1f5a029b2b8e
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/04/2018
ms.locfileid: "43664008"
---
# <a name="azure-cli-samples"></a>Exemplos da CLI do Azure

A tabela seguinte inclui ligações para scripts bash do Serviço Azure SignalR criados com a CLI do Azure.

| | |
|-|-|
|**Criar**||
| [Criar um novo Serviço SignalR e grupo de recursos](scripts/signalr-cli-create-service.md) | Cria um novo recurso do Serviço Azure SignalR num novo grupo de recursos com um nome aleatório.  |
|**Integrar**||
| [Criar um novo Serviço SignalR e Aplicação Web configurada para utilizar o SignalR](scripts/signalr-cli-create-with-app-service.md) | Cria um novo recurso do Serviço Azure SignalR num novo grupo de recursos com um nome aleatório. Também adiciona um novo plano do Serviço de Aplicações e Aplicação Web para alojar uma Aplicação Web ASP.NET Core que utilize o Serviço SignalR. A aplicação Web está configurada com uma Definição de Aplicação para ligar ao novo recurso do serviço SignalR. |
| [Criar um novo Serviço SignalR e Aplicação Web configurada para utilizar o SignalR e o GitHub OAuth](scripts/signalr-cli-create-with-app-service-github-oauth.md) | Cria um novo recurso do Serviço Azure SignalR num novo grupo de recursos com um nome aleatório. Também adiciona uma nova Aplicação Web do Azure e plano de alojamento para alojar uma Aplicação Web ASP.NET Core que utilize o Serviço SignalR. A aplicação Web está configurada com definições de aplicação para a cadeia de ligação para o novo recurso do serviço SignalR e segredos do cliente para suportar a [autenticação GitHub](https://developer.github.com/v3/guides/basics-of-authentication/), conforme demonstrado no [tutorial de autenticação](signalr-authenticate-oauth.md). A aplicação Web também está configurada para utilizar uma origem de implementação do repositório Git local. |
| | |
