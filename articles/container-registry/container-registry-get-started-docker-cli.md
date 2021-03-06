---
title: Enviar imagem do Docker para registo privado do Azure
description: Enviar e extrair imagens do Docker para um registo privado de contentor do Azure com a CLI do Docker
services: container-registry
author: stevelas
manager: jeconnoc
ms.service: container-registry
ms.topic: article
ms.date: 11/29/2017
ms.author: stevelas
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 24cccd4745d611196046168f0125e7ef2a184e15
ms.sourcegitcommit: 615403e8c5045ff6629c0433ef19e8e127fe58ac
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/06/2018
ms.locfileid: "39576496"
---
# <a name="push-your-first-image-to-a-private-docker-container-registry-using-the-docker-cli"></a>Enviar a sua primeira imagem para um registo privado de contentor Docker com a CLI do Docker

Os registos de contentores do Azure armazenam e gerem imagens de contentores do [Docker](http://hub.docker.com) privadas, de forma semelhante a como o [Docker Hub](https://hub.docker.com/) armazena imagens do Docker públicas. Pode utilizar o [interface de linha de comandos do Docker](https://docs.docker.com/engine/reference/commandline/cli/) (Docker CLI) para [início de sessão](https://docs.docker.com/engine/reference/commandline/login/), [push](https://docs.docker.com/engine/reference/commandline/push/), [pull](https://docs.docker.com/engine/reference/commandline/pull/)e outras operações no seu contentor registo.

Nas etapas a seguir, baixar um oficial [imagem da Nginx](https://store.docker.com/images/nginx) partir do registo do Docker Hub público, identifique-a para o seu registo de contentores do Azure privado, enviá-la para o seu registo e, em seguida, tirá-lo a partir do registo.

## <a name="prerequisites"></a>Pré-requisitos

* **Registo de contentores do Azure** - crie um registos de contentores na sua subscrição do Azure. Por exemplo, utilizar o [portal do Azure](container-registry-get-started-portal.md) ou o [CLI do Azure](container-registry-get-started-azure-cli.md).
* **CLI do docker** - para configurar o seu computador local como anfitrião Docker e acessar os comandos da CLI do Docker, instale [Docker](https://docs.docker.com/engine/installation/).

## <a name="log-in-to-a-registry"></a>Iniciar sessão num registo

Existem [várias formas de autenticar](container-registry-authentication.md) para o seu registo de contentor privado. O método recomendado ao trabalhar numa linha de comandos é com o comando da CLI do Azure [início de sessão az acr](/cli/azure/acr?view=azure-cli-latest#az-acr-login). Por exemplo iniciar sessão para um registo denominado *myregistry*:

```azurecli
az acr login --name myregistry
```

Também pode iniciar sessão com [início de sessão do docker](https://docs.docker.com/engine/reference/commandline/login/). O exemplo seguinte transmite o ID e a palavra-passe de um [principal de serviço](../active-directory/develop/app-objects-and-service-principals.md) do Azure Active Directory. Por exemplo, poderá ter [atribuído um principal de serviço](container-registry-authentication.md#service-principal) para o seu registo para um cenário de automatização.

```Bash
docker login myregistry.azurecr.io -u xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx -p myPassword
```

Os dois comandos devolvem `Login Succeeded` depois de concluir. Se usar `docker login`, também poderá ver um aviso de segurança recomenda a utilização do `--password-stdin` parâmetro. Enquanto a sua utilização está fora do âmbito deste artigo, recomendamos que siga esta melhor prática. Para obter mais informações, consulte a [início de sessão do docker](https://docs.docker.com/engine/reference/commandline/login/) referência do comando.

> [!TIP]
> Sempre especificar o nome do registo completamente qualificado (em minúsculas) quando utiliza `docker login` e quando marca imagens para enviar por push para o seu registo. Nos exemplos neste artigo, é o nome completamente qualificado *myregistry.azurecr.io*.

## <a name="pull-the-official-nginx-image"></a>Extraia a imagem oficial do Nginx

Em primeiro lugar, extrair a imagem pública da Nginx para o computador local.

```Bash
docker pull nginx
```

## <a name="run-the-container-locally"></a>Executar o contentor localmente

Executar o seguinte [docker run](https://docs.docker.com/engine/reference/run/) comando para iniciar uma instância local do contentor de Nginx interativamente (`-it`) na porta 8080. O `--rm` argumento especifica que o contêiner deve ser removido quando, impedi-lo.

```Bash
docker run -it --rm -p 8080:80 nginx
```

Navegue até [ http://localhost:8080 ](http://localhost:8080) para ver a página da web padrão servidos pelo Nginx no contentor em execução. Deverá ver uma página semelhante ao seguinte:

![Nginx no computador local](./media/container-registry-get-started-docker-cli/nginx.png)

Uma vez que iniciou o contentor de forma interativa com `-it`, pode ver o servidor Nginx o resultado na linha de comando depois de navegar para o mesmo no seu browser.

Para parar e remover o contentor, prima `Control` + `C`.

## <a name="create-an-alias-of-the-image"></a>Criar um alias da imagem

Uso [etiqueta do docker](https://docs.docker.com/engine/reference/commandline/tag/) para criar um alias da imagem com o caminho totalmente qualificado para o seu registo. Este exemplo especifica o espaço de nomes `samples` para evitar sobrepovoar a raiz do registo.

```Bash
docker tag nginx myregistry.azurecr.io/samples/nginx
```

Para obter mais informações sobre a marcação com espaços de nomes, consulte a [espaços de nomes do repositório](container-registry-best-practices.md#repository-namespaces) secção [melhores práticas do Azure Container Registry](container-registry-best-practices.md).

## <a name="push-the-image-to-your-registry"></a>Enviar a imagem para o seu registo

Agora que já marcou a imagem com o caminho totalmente qualificado para o seu registo privado, pode enviá-la para o registo com [push do docker](https://docs.docker.com/engine/reference/commandline/push/):

```Bash
docker push myregistry.azurecr.io/samples/nginx
```

## <a name="pull-the-image-from-your-registry"></a>Extrair a imagem do seu registo

Utilize o [solicitação docker](https://docs.docker.com/engine/reference/commandline/pull/) comando para extrair a imagem a partir do registo:

```Bash
docker pull myregistry.azurecr.io/samples/nginx
```

## <a name="start-the-nginx-container"></a>Iniciar o contentor de Nginx

Utilize o [docker run](https://docs.docker.com/engine/reference/run/) comando para executar a imagem tiver obtido a partir do registo:

```Bash
docker run -it --rm -p 8080:80 myregistry.azurecr.io/samples/nginx
```

Navegue até [ http://localhost:8080 ](http://localhost:8080) para ver o contentor em execução.

Para parar e remover o contentor, prima `Control` + `C`.

## <a name="remove-the-image-optional"></a>Remova a imagem (opcional)

Se já não precisar da imagem da Nginx, pode eliminá-lo localmente com o [docker rmi](https://docs.docker.com/engine/reference/commandline/rmi/) comando.

```Bash
docker rmi myregistry.azurecr.io/samples/nginx
```

Para remover as imagens do seu registo de contentor do Azure, pode utilizar o comando da CLI do Azure [eliminação de repositório az acr](/cli/azure/acr/repository#az-acr-repository-delete). Por exemplo, o comando seguinte elimina o manifesto referenciado por uma marca, quaisquer dados de camada associado e todas as outras etiquetas que referencia o manifesto.

```azurecli
az acr repository delete --name myregistry --repository samples/nginx --tag latest --manifest
```

## <a name="next-steps"></a>Passos Seguintes

Agora que sabe as noções básicas, está pronto para começar a utilizar o seu registo! Implemente imagens de contentor a partir do registo para:

* [Serviço Kubernetes do Azure (AKS)](../aks/tutorial-kubernetes-prepare-app.md)
* [Instâncias de contentor do Azure](../container-instances/container-instances-tutorial-prepare-app.md)
* [Service Fabric](../service-fabric/service-fabric-tutorial-create-container-images.md)
