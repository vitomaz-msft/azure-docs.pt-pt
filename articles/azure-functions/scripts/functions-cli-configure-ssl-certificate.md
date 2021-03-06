---
title: Exemplo do Script da CLI do Azure – Vincular um certificado SSL personalizado a uma aplicação de funções | Microsoft Docs
description: Exemplo do Script da CLI do Azure – Vincular um certificado SSL personalizado a uma aplicação de funções no Azure
services: functions
documentationcenter: ''
author: ggailey777
manager: jeconnoc
ms.assetid: eb95d350-81ea-4145-a1e2-6eea3b7469b2
ms.service: azure-functions
ms.devlang: azurecli
ms.topic: sample
ms.date: 07/03/2013
ms.author: glenga
ms.custom: mvc
ms.openlocfilehash: 689764543f5d927273f92deecbfd43e282fc028c
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/24/2018
ms.locfileid: "46961370"
---
# <a name="bind-a-custom-ssl-certificate-to-a-function-app"></a>Vincular um certificado SSL personalizado a uma aplicação de funções

Este script de exemplo cria uma aplicação de funções no Serviço de Aplicações com os respetivos recursos relacionados e, em seguida, vincula o certificado SSL de um nome de domínio personalizado ao mesmo. Neste exemplo, precisa de:

* Acesso à página de configuração de DNS da sua entidade de registo de domínios.
* Um ficheiro .PFX válido e a respetiva palavra-passe do certificado SSL que pretende carregar e vincular.
* Ter configurado um registo A no seu domínio personalizado que aponta para o nome de domínio predefinido da sua aplicação Web. Para obter mais informações, consulte as [Instruções do mapa do domínio personalizado para o Serviço de Aplicações do Azure](https://aka.ms/appservicecustomdns).

Para vincular um certificado SSL, a aplicação de funções tem de ser criada num plano do Serviço de Aplicações e não num plano de consumo.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Se optar por instalar e utilizar a CLI localmente, tem de utilizar a versão 2.0 ou posterior da CLI do Azure. Executar `az --version` para localizar a versão. Se precisar de instalar ou atualizar, veja [Instalar a CLI do Azure]( /cli/azure/install-azure-cli). 

## <a name="sample-script"></a>Script de exemplo

[!code-azurecli-interactive[main](../../../cli_scripts/azure-functions/configure-ssl-certificate/configure-ssl-certificate.sh?highlight=3-5 "Bind a custom SSL certificate to a web app")]

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a>Explicação do script

Este script utiliza os seguintes comandos. Cada comando na tabela liga à documentação específica do comando.

| Comando | Notas |
|---|---|
| [az group create](https://docs.microsoft.com/cli/azure/group#az-group-create) | Cria um grupo de recursos no qual todos os recursos são armazenados. |
| [az storage account create](https://docs.microsoft.com/cli/azure/storage/account#az-storage-account-create) | Cria uma conta de armazenamento necessária para a aplicação de funções. |
| [az appservice plan create](https://docs.microsoft.com/cli/azure/appservice/plan#az-appservice-plan-create) | Cria um plano do Serviço de Aplicações necessário para vincular certificados SSL. |
| [az functionapp create](https://docs.microsoft.com/cli/azure/functionapp#az-functionapp-create) | Cria uma aplicação de funções no plano do Serviço de Aplicações. |
| [az functionapp config hostname add](https://docs.microsoft.com/cli/azure/functionapp/config/hostname#az-functionapp-config-hostname-add) | Mapeia um domínio personalizado para uma aplicação de funções. |
| [az functionapp config ssl upload](https://docs.microsoft.com/cli/azure/functionapp/config/ssl#az-functionapp-config-ssl-upload) | Carrega um certificado SSL para uma aplicação de funções. |
| [az functionapp config ssl bind](https://docs.microsoft.com/cli/azure/functionapp/config/ssl#az-functionapp-config-ssl-bind) | Vincula um certificado SSL carregado a uma aplicação de funções. |

## <a name="next-steps"></a>Passos seguintes

Para obter mais informações sobre a CLI do Azure, veja [Documentação da CLI do Azure](https://docs.microsoft.com/cli/azure).

Pode ver exemplos do script da CLI do Serviço de Aplicações adicionais na [documentação do Serviço de Aplicações do Azure](../functions-cli-samples.md).
