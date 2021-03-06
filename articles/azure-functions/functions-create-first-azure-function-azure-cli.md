---
title: Criar a sua primeira função com a CLI do Azure
description: Saiba como criar a sua primeira Função do Azure para execução sem servidor através da CLI do Azure e das Ferramentas Base das Funções do Azure.
services: functions
keywords: ''
author: ggailey777
ms.author: glenga
ms.assetid: 674a01a7-fd34-4775-8b69-893182742ae0
ms.date: 11/13/2018
ms.topic: quickstart
ms.service: azure-functions
ms.custom: mvc
ms.devlang: azure-cli
manager: jeconnoc
ms.openlocfilehash: 05b35ac182d70d6d7a7630a14c8a8aa3b7a6a9fd
ms.sourcegitcommit: 0b7fc82f23f0aa105afb1c5fadb74aecf9a7015b
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/14/2018
ms.locfileid: "51634316"
---
# <a name="create-your-first-function-from-the-command-line"></a>Criar a primeira função a partir da linha de comandos

Este tópico do guia de introdução mostra-lhe como criar a sua primeira função a partir da linha de comandos ou do terminal. Vai utilizar a CLI do Azure para criar uma aplicação de funções, que é a infraestrutura [sem servidor](https://azure.microsoft.com/solutions/serverless/) que aloja a sua função. O projeto de código de função é gerado a partir de um modelo com as [Ferramentas Base das Funções do Azure](functions-run-local.md), que também é utilizado para implementar o projeto de aplicação de funções no Azure.

Pode seguir os passos abaixo num computador Mac, Windows ou Linux.

## <a name="prerequisites"></a>Pré-requisitos

Antes de executar este exemplo, tem de ter o seguinte:

+ Instale a [versão 2.x das Ferramentas Base do Azure](functions-run-local.md#v2).

+ Instale a [CLI do Azure]( /cli/azure/install-azure-cli). Este artigo requer a versão 2.0 ou posterior da CLI do Azure. Execute `az --version` para localizar a versão atual. Pode também utilizar o [Azure Cloud Shell](https://shell.azure.com/bash).

+ Uma subscrição ativa do Azure.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="create-the-local-function-app-project"></a>Criar o projeto de aplicação de funções local

Execute o seguinte comando na linha de comandos para criar um projeto de aplicação de funções na pasta `MyFunctionProj` do diretório local atual. Também é criado um repositório do GitHub em `MyFunctionProj`.

```bash
func init MyFunctionProj
```

Quando lhe for pedido, selecione um runtime de trabalho a partir das seguintes opções de linguagem:

+ `dotnet`: cria um projeto de biblioteca de classes .NET (.csproj).
+ `node`: cria um projeto do JavaScript.

Quando o comando for executado, verá algo parecido com o seguinte resultado:

```output
Writing .gitignore
Writing host.json
Writing local.settings.json
Initialized empty Git repository in C:/functions/MyFunctionProj/.git/
```

Utilize o comando seguinte para navegar para a nova pasta do projeto `MyFunctionProj`.

```bash
cd MyFunctionProj
```

[!INCLUDE [functions-create-function-core-tools](../../includes/functions-create-function-core-tools.md)]

[!INCLUDE [functions-update-function-code](../../includes/functions-update-function-code.md)]

[!INCLUDE [functions-run-function-test-local](../../includes/functions-run-function-test-local.md)]

[!INCLUDE [functions-create-resource-group](../../includes/functions-create-resource-group.md)]

[!INCLUDE [functions-create-storage-account](../../includes/functions-create-storage-account.md)]

## <a name="create-a-function-app"></a>Criar uma aplicação de função

Precisa de uma aplicação Function App para alojar a execução das suas funções. A aplicação Function App proporciona um ambiente para a execução sem servidor do código da sua função. Permite-lhe agrupar funções como unidades lógicas para uma gestão, implementação e partilha de recursos mais fácil. Utilize o comando [az functionapp create](/cli/azure/functionapp#az-functionapp-create) para criar uma aplicação Function App. 

No comando seguinte, substitua o nome da sua aplicação de funções exclusivo onde vir o marcador de posição `<app_name>` e o nome da conta de armazenamento para `<storage_name>`. O `<app_name>` vai ser utilizado como o domínio DNS predefinido para a aplicação Function App, daí que o nome tenha de ser exclusivo em todas as aplicações no Azure. O parâmetro _deployment-source-url_ é um repositório de exemplo no GitHub que contém uma função acionada por HTTP "Hello World".

```azurecli-interactive
az functionapp create --resource-group myResourceGroup --consumption-plan-location westeurope \
--name <app_name> --storage-account  <storage_name>  
```

Definir o parâmetro _consumption-plan-location_ significa que a aplicação de funções é alojada num plano de alojamento de Consumo. Neste plano sem servidor, os recursos são adicionados dinamicamente conforme exigido pelas suas funções e paga apenas quando as funções estão em execução. Para obter mais informações, veja [Choose the correct hosting plan](functions-scale.md) (Escolher o plano de alojamento certo).

Depois de a aplicação Function App ter sido criada, a CLI do Azure mostra informações semelhantes às do exemplo seguinte:

```json
{
  "availabilityState": "Normal",
  "clientAffinityEnabled": true,
  "clientCertEnabled": false,
  "containerSize": 1536,
  "dailyMemoryTimeQuota": 0,
  "defaultHostName": "quickstart.azurewebsites.net",
  "enabled": true,
  "enabledHostNames": [
    "quickstart.azurewebsites.net",
    "quickstart.scm.azurewebsites.net"
  ],
   ....
    // Remaining output has been truncated for readability.
}
```

### <a name="configure-the-function-app-nodejs"></a>Configurar a aplicação de funções (Node.js)

Quando cria uma aplicação de funções do JavaScript, é importante direcionar a versão correta do Node.js. A versão 2.x do runtime de Funções exige a versão 8.x do Node.js. A definição da aplicação `WEBSITE_NODE_DEFAULT_VERSION` controla a versão do Node.js que é utilizada pela aplicação de funções no Azure. Utilize o comando [az functionapp config appsettings set](https://docs.microsoft.com/cli/azure/functionapp/config/appsettings#set) para definir a versão do Node.js para `8.11.1`.

No seguinte comando da CLI do Azure, <app_name> é o nome da sua aplicação de funções.

```azurecli-interactive
az functionapp config appsettings set --resource-group myResourceGroup \
 --name <app_name> --settings WEBSITE_NODE_DEFAULT_VERSION=8.11.1
```

Verifique a nova definição na saída.

[!INCLUDE [functions-publish-project](../../includes/functions-publish-project.md)]

[!INCLUDE [functions-test-function-code](../../includes/functions-test-function-code.md)]

[!INCLUDE [functions-cleanup-resources](../../includes/functions-cleanup-resources.md)]

[!INCLUDE [functions-quickstart-next-steps-cli](../../includes/functions-quickstart-next-steps-cli.md)]

