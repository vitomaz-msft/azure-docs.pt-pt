---
title: Como utilizar a cli de redis com a Cache de Redis do Azure | Documentos da Microsoft
description: Saiba como utilizar a cli de redis com a Cache de Redis do Azure.
services: redis-cache
documentationcenter: ''
author: wesmc7777
manager: cfowler
editor: ''
ms.service: cache
ms.workload: tbd
ms.tgt_pltfrm: cache-redis
ms.devlang: na
ms.topic: article
ms.date: 03/22/2018
ms.author: wesmc
ms.openlocfilehash: facc3e2079c8f5e19266f5379762d71754a6ed84
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/11/2018
ms.locfileid: "38299096"
---
# <a name="how-to-use-the-redis-command-line-tool-with-azure-redis-cache"></a>Como utilizar a ferramenta de linha de comandos da Redis com a Cache de Redis do Azure

*redis cli.exe* é uma ferramenta de linha de comandos popular para interagir com uma Cache de Redis como um cliente. Essa ferramenta também está disponível para utilização com a Cache de Redis do Azure.

A ferramenta está disponível para plataformas Windows baixando o [Redis ferramentas da linha de comandos para Windows](https://github.com/MSOpenTech/redis/releases/). 

Se quiser executar a ferramenta da linha de comandos em outra plataforma, transfira a Cache de Redis do [ http://redis.io/download ](https://redis.io/download).

## <a name="gather-cache-access-information"></a>Recolher informações de acesso da cache

Pode recolher as informações necessárias para aceder à cache usando três métodos:

1. CLI do Azure com [az redis lista as chaves](https://docs.microsoft.com/cli/azure/redis?view=azure-cli-latest#az-redis-list-keys)
2. Azure PowerShell com [Get AzureRmRedisCacheKey](https://docs.microsoft.com/powershell/module/azurerm.rediscache/Get-AzureRmRedisCacheKey?view=azurermps-4.4.1)
3. No portal do Azure.

Nesta secção, irá obter as chaves do portal do Azure.

[!INCLUDE [redis-cache-create](../../includes/redis-cache-access-keys.md)]


## <a name="enable-access-for-redis-cliexe"></a>Ativar o acesso redis-cli.exe

Cache de Redis do Azure, apenas o SSL da porta (6380) está ativada por predefinição. O `redis-cli.exe` ferramenta da linha de comandos não suporta SSL. Há duas opções de configuração para utilizá-lo:

1. [Ativar a porta não SSL (6379)](cache-configure.md#access-ports) - **esta configuração não é recomendada** porque esta configuração, as chaves de acesso são enviadas através de TCP em texto não criptografado. Esta alteração pode comprometer o acesso à sua cache. O único cenário onde pode considerar a esta configuração é quando apenas estão a aceder uma cache de teste.

2. Transfira e instale [túnel](https://www.stunnel.org/downloads.html).

    Execute **túnel GUI início** para iniciar o servidor.

    Faça duplo clique no ícone da barra de tarefas para o servidor do túnel e clique em **Mostrar a janela de registo**.

    No menu da janela de registo de túnel, clique em **Configuration** > **Editar configuração** para abrir o ficheiro de configuração atual.

    Adicione a seguinte entrada para *redis cli.exe* sob a **definições do serviço** secção. Inserir o nome da sua cache real em vez de `yourcachename`. 

    ```
    [redis-cli]
    client = yes
    accept = 127.0.0.1:6380
    connect = yourcachename.redis.cache.windows.net:6380
    ```

    Guarde e feche o ficheiro de configuração. 
  
    No menu da janela de registo de túnel, clique em **Configuration** > **recarregar configuração**.


## <a name="connect-using-the-redis-command-line-tool"></a>Ligue-se com a ferramenta de linha de comandos da Redis.

Ao utilizar o túnel, execute *redis cli.exe*e apenas passar seu *porta*, e *chave de acesso* (primário ou secundário) para ligar à cache.

```
redis-cli.exe -p 6380 -a YourAccessKey
```

![túnel com o redis-cli](media/cache-how-to-redis-cli-tool/cache-redis-cli-stunnel.png)

Se estiver a utilizar uma cache de teste com o **inseguras** porta não SSL, execute `redis-cli.exe` e passar seu *nome de anfitrião*, *porta*, e *chave de acesso*(primário ou secundário) para ligar à cache de teste.

```
redis-cli.exe -h yourcachename.redis.cache.windows.net -p 6379 -a YourAccessKey
```

![túnel com o redis-cli](media/cache-how-to-redis-cli-tool/cache-redis-cli-non-ssl.png)




## <a name="next-steps"></a>Passos Seguintes

Saiba mais sobre como utilizar o [consola de Redis](cache-configure.md#redis-console) aos comandos do problema.

