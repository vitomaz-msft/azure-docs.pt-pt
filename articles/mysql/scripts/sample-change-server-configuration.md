---
title: Script da CLI do Azure - Alterar configurações do servidor
description: Este script de exemplo da CLI lista todas as configurações de servidor disponíveis e atualiza o valor de innodb_lock_wait_timeout.
services: mysql
author: ajlam
ms.author: andrela
manager: kfile
editor: jasonwhowell
ms.service: mysql
ms.devlang: azure-cli
ms.topic: sample
ms.custom: mvc
ms.date: 02/28/2018
ms.openlocfilehash: 011178f539fcb826e0e6fb60925d740945461ea9
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/24/2018
ms.locfileid: "46952896"
---
# <a name="list-and-update-configurations-of-an-azure-database-for-mysql-server-using-azure-cli"></a>Listar e atualizar configurações do servidor da Base de Dados do Azure para MySQL com a CLI do Azure
Este script de exemplo da CLI lista todos os parâmetros de configuração disponíveis, bem como os respetivos valores permitidos para o servidor da Base de Dados do Azure para MySQL e define *innodb_lock_wait_timeout* para um valor diferente do que está predefinido.

[!INCLUDE [cloud-shell-try-it](../../../includes/cloud-shell-try-it.md)]

Se optar por executar a CLI localmente, este artigo requer a execução da versão 2.0 ou posterior da CLI do Azure. Verifique a versão ao executar `az --version`. Veja [Instalar a CLI do Azure]( /cli/azure/install-azure-cli) para instalar ou atualizar a sua versão da CLI do Azure. 

## <a name="sample-script"></a>Script de exemplo
Neste script de exemplo, edite as linhas realçadas para atualizar o nome de utilizador administrador e a palavra-passe com os seus.
[!code-azurecli-interactive[main](../../../cli_scripts/mysql/change-server-configurations/change-server-configurations.sh?highlight=18-19 "List and update configurations of Azure Database for MySQL.")]

## <a name="clean-up-deployment"></a>Limpar a implementação
Utilize o comando seguinte para remover o grupo de recursos e todos os recursos associados ao mesmo, depois de executar o script. 
[!code-azurecli-interactive[main](../../../cli_scripts/mysql/change-server-configurations/delete-mysql.sh  "Delete the resource group.")]

## <a name="script-explanation"></a>Explicação do script
Este script utiliza os comandos descritos na tabela seguinte:

| **Comando** | **Notas** |
|---|---|
| [az group create](/cli/azure/group#az-group-create) | Cria um grupo de recursos no qual todos os recursos são armazenados. |
| [az mysql server create](/cli/azure/mysql/server#az-msql-server-create) | Cria um servidor MySQL que aloja as bases de dados. |
| [az mysql server configuration list](/cli/azure/mysql/server/configuration#az-msql-server-configuration-list) | Lista as configurações do servidor da Base de Dados do Azure para MySQL. |
| [az mysql server configuration set](/cli/azure/mysql/server/configuration#az-msql-server-configuration-set) | Atualiza a configuração do servidor da Base de Dados do Azure para MySQL. |
| [az mysql server configuration show](/cli/azure/mysql/server/configuration#az-msql-server-configuration-show) | Mostra a configuração do servidor da Base de Dados do Azure para MySQL. |
| [az group delete](/cli/azure/group#az-group-delete) | Elimina um grupo de recursos, incluindo todos os recursos aninhados. |

## <a name="next-steps"></a>Passos seguintes
- Leia mais informações sobre a CLI do Azure: [Documentação da CLI do Azure](/cli/azure).
- Experimente scripts adicionais: [Exemplos da CLI do Azure para a Base de Dados do Azure para MySQL](../sample-scripts-azure-cli.md)
- Para obter mais informações sobre os parâmetros de servidor, veja [Como Configurar Parâmetros de Servidor na Base de Dados do Azure para MySQL](../howto-server-parameters.md).
