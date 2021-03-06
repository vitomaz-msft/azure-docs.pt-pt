---
title: Confiança do conteúdo no Azure Container Registry
description: Saiba como ativar a confiança do conteúdo para o seu registo de contentor do Azure e como enviar e extrair imagens assinadas.
services: container-registry
author: dlepow
ms.service: container-registry
ms.topic: quickstart
ms.date: 08/20/2018
ms.author: danlep
ms.openlocfilehash: 1f1e7ce41c00078c0181fc5f32c43b7e5885eef8
ms.sourcegitcommit: 67abaa44871ab98770b22b29d899ff2f396bdae3
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/08/2018
ms.locfileid: "48857674"
---
# <a name="content-trust-in-azure-container-registry"></a>Confiança do conteúdo no Azure Container Registry

Verificar a *origem* e a *integridade* dos dados que entram no sistema é importante para qualquer sistema distribuído desenhado tendo em conta a segurança. Os consumidores dos dados têm de poder verificar o publicador (origem) dos dados e garantir que os mesmos não foram modificados depois de terem sido publicados (integridade). O Azure Container Registry suporta ambas as capacidades mediante a implementação do modelo de [confiança do conteúdo][docker-content-trust] do Docker. Este artigo ajuda-o a começar.

> [!IMPORTANT]
> Esta funcionalidade encontra-se em pré-visualização. As pré-visualizações ser-lhe-ão disponibilizadas na condição de concordar com os [termos suplementares de utilização][terms-of-use]. Alguns aspetos desta funcionalidade podem alterar-se após a disponibilidade geral (GA).

## <a name="how-content-trust-works"></a>Como funciona a confiança do conteúdo

Enquanto publicador de imagens, a confiança do conteúdo permite-lhe **assinar** as imagens que envia para o seu registo. Os consumidores dessas imagens (pessoas ou sistemas que extraem as imagens do seu registo) podem configurar os clientes deles para extrair *apenas* as imagens assinadas. Quando um consumidor de imagens extrai uma imagem assinada, o cliente do Docker dele verifica a integridade da mesma. Neste modelo, os consumidores ficam com a garantia de que as imagens assinadas no seu registo foram realmente publicadas por si e que não foram modificadas desde a publicação.

### <a name="trusted-images"></a>Imagens fiáveis

A confiança do conteúdo funciona com as **etiquetas** num repositório. Os repositórios de imagens podem conter imagens com etiquetas assinadas e não assinadas. Por exemplo, pode assinar apenas as imagens `myimage:stable` e `myimage:latest`, mas não `myimage:dev`.

### <a name="signing-keys"></a>Chaves de assinatura

A confiança do conteúdo é gerida com um conjunto de chaves de assinatura criptográficas. Essas chaves são associadas a um repositório específico num registo. Os clientes do Docker e o seu registo utilizam vários tipos de chaves de assinatura para a gestão da confiança das etiquetas num repositório. Quando ativa a confiança do conteúdo e a integra no pipeline de publicação e consumo do seu contentor, tem de gerir as chaves com cuidado. Para obter mais informações, veja [Gestão de chaves](#key-management), mais à frente no artigo, e [Manage keys for content trust][docker-manage-keys] (Gerir chaves para a confiança do conteúdo), na documentação do Docker.

> [!TIP]
> Esta é uma descrição geral bastante genérica do modelo de confiança do conteúdo do Docker. Para uma discussão mais aprofundada da confiança do conteúdo, veja [Content trust in Docker][docker-content-trust] (Confiança do conteúdo no Docker).

## <a name="enable-registry-content-trust"></a>Ativar a confiança do conteúdo do registo

O primeiro passo é ativar a confiança do conteúdo ao nível do registo. Depois de ativar a confiança do conteúdo, os clientes (utilizadores ou serviços) podem enviar imagens assinadas para o seu registo. A ativação da confiança do conteúdo no seu registo não limita a utilização do mesmo apenas aos consumidores que tenham a confiança ativada. Os consumidores que não a tenham ativada continuam a poder utilizar o seu registo como normalmente. Contudo, os consumidores que tenham ativado a confiança do conteúdo nos clientes deles conseguirão ver *apenas* as imagens assinadas no seu registo.

Para ativar a confiança do conteúdo no seu registo, navegue primeiro para o mesmo no portal do Azure. Em**POLÍTICAS**, selecione **Confiança do Conteúdo (Pré-visualização)** > **Ativada** > **Guardar**.

![Ativar a confiança do conteúdo num registo no portal do Azure][content-trust-01-portal]

## <a name="enable-client-content-trust"></a>Ativar a confiança do conteúdo no cliente

Para trabalhar com as imagens fiáveis, tanto os publicadores de imagens, como os consumidores, têm de ativar a confiança do conteúdo nos respetivos clientes do Docker. Enquanto publicador, pode assinar as imagens que envia para um registo com a confiança do conteúdo ativada. Enquanto consumidor, a ativação da confiança limita a visualização dos registos apenas às imagens assinadas. A confiança do conteúdo está desativada por predefinição nos clientes do Docker, mas pode ativá-la por sessão da shell ou por comando.

Para ativar a confiança do conteúdo para uma sessão da shell, defina a variável de ambiente `DOCKER_CONTENT_TRUST` como **1**. Por exemplo, na shell do Bash:

```bash
# Enable content trust for shell session
export DOCKER_CONTENT_TRUST=1
```

Se preferir ativar ou desativar a confiança do conteúdo para um comando individual, vários comandos do Docker suportam o argumento `--disable-content-trust`. Para ativar a confiança do conteúdo para um comando individual:

```bash
# Enable content trust for single command
docker build --disable-content-trust=false -t myacr.azurecr.io/myimage:v1 .
```

Se tiver ativado a confiança do conteúdo para a sessão da shell e quiser desativá-la para um comando individual:

```bash
# Disable content trust for single command
docker build --disable-content-trust -t myacr.azurecr.io/myimage:v1 .
```

## <a name="grant-image-signing-permissions"></a>Conceder permissões de assinatura de imagens

Só os utilizadores ou sistemas aos quais tenha concedido permissão podem enviar imagens fiáveis para o seu registo. Para conceder permissão de envio de imagens fiáveis a um utilizador (ou a um sistema com um principal de serviço), dê às respetivas identidades do Azure Active Directory a função `AcrImageSigner`. Esta função é concedida como adição à função `Contributor` (ou `Owner`) necessária para enviar imagens para o registo.

Pode ver abaixo os detalhes para conceder a função `AcrImageSigner` no portal do Azure e na CLI do Azure.

### <a name="azure-portal"></a>Portal do Azure

Navegue para o registo no portal do Azure e selecione **Controlo de Acesso (IAM)** > **Adicionar**. Em **Adicionar permissões**, selecione `AcrImageSigner`, em **Função**, escolha **Selecionar** um ou mais utilizadores ou principais de serviço e selecione **Guardar**.

Neste exemplo, foi atribuída a duas entidades a função `AcrImageSigner`: um principal de serviço denominado “service-principal” e um utilizador com o nome “Azure user”.

![Ativar a confiança do conteúdo num registo no portal do Azure][content-trust-02-portal]

### <a name="azure-cli"></a>CLI do Azure

Para conceder permissões de assinatura a um utilizador com a CLI do Azure, atribua-lhe a função `AcrImageSigner`, com âmbito para o seu registo. O formato do comando é:

```azurecli
az role assignment create --scope <registry ID> --role AcrImageSigner --assignee <user name>
```

Por exemplo, para se conceder a si próprio a função, pode executar os comandos seguintes numa sessão autenticada da CLI do Azure. Modifique o valor `REGISTRY`, de modo a refletir o nome do seu registo de contentor do Azure.

```bash
# Grant signing permissions to authenticated Azure CLI user
REGISTRY=myregistry
USER=$(az account show --query user.name --output tsv)
REGISTRY_ID=$(az acr show --name $REGISTRY --query id --output tsv)

az role assignment create --scope $REGISTRY_ID --role AcrImageSigner --assignee $USER
```

Também pode conceder a um [principal de serviço](container-registry-auth-service-principal.md) os direitos para enviar imagens fiáveis para o registo. A utilização de um principal de serviço é útil para sistemas de compilação e para outros sistemas autónomos que têm de enviar imagens fiáveis para o seu registo. O formato é semelhante a conceder permissão a um utilizador, mas é especificado um ID de principal de serviço no valor `--assignee`.

```azurecli
az role assignment create --scope $REGISTRY_ID --role AcrImageSigner --assignee <service principal ID>
```

`<service principal ID>` pode ser **appId** ou **objectId** do principal de serviço ou um dos respetivos **servicePrincipalNames**. Para obter mais informações sobre como trabalhar com os principais de serviço e o Azure Container Registry, veja [Azure Container Registry authentication with service principals](container-registry-auth-service-principal.md) (Autenticação do Azure Container Registry com principais de serviço).

## <a name="push-a-trusted-image"></a>Enviar uma imagem fiável

Para enviar uma etiqueta de imagem fiável para o registo de contentor, ative a confiança do conteúdo e envie a imagem com `docker push`. Na primeira vez que enviar uma etiqueta assinada, é-lhe pedido que crie uma frase de acesso para uma chave de assinatura raiz e uma chave de assinatura de repositório. Ambas as chaves são geradas e armazenadas localmente no seu computador.

```console
$ docker push myregistry.azurecr.io/myimage:v1
The push refers to repository [myregistry.azurecr.io/myimage]
ee83fc5847cb: Pushed
v1: digest: sha256:aca41a608e5eb015f1ec6755f490f3be26b48010b178e78c00eac21ffbe246f1 size: 524
Signing and pushing trust metadata
You are about to create a new root signing key passphrase. This passphrase
will be used to protect the most sensitive key in your signing system. Please
choose a long, complex passphrase and be careful to keep the password and the
key file itself secure and backed up. It is highly recommended that you use a
password manager to generate the passphrase and keep it safe. There will be no
way to recover this key. You can find the key in your config directory.
Enter passphrase for new root key with ID 4c6c56a:
Repeat passphrase for new root key with ID 4c6c56a:
Enter passphrase for new repository key with ID bcd6d98:
Repeat passphrase for new repository key with ID bcd6d98:
Finished initializing "myregistry.azurecr.io/myimage"
Successfully signed myregistry.azurecr.io/myimage:v1
```

Após o seu primeiro `docker push` com a confiança do conteúdo ativada, o cliente do Docker utiliza a mesma chave raiz para os envios subsequentes. Em cada envio subsequente para o mesmo repositório, só lhe é pedida a chave do repositório. Sempre que enviar uma imagem fiável para um repositório novo, é-lhe pedido que indique uma frase de acesso para uma chave de repositório nova.

## <a name="pull-a-trusted-image"></a>Extrair uma imagem fiável

Para extrair uma imagem fiável, ative a confiança do conteúdo e execute o comando `docker pull` normalmente. Os consumidores que tenham a confiança do conteúdo ativada só podem extrair imagens com etiquetas assinadas. Eis um exemplo de extração de uma etiqueta assinada:

```console
$ docker pull myregistry.azurecr.io/myimage:signed
Pull (1 of 1): myregistry.azurecr.io/myimage:signed@sha256:0800d17e37fb4f8194495b1a188f121e5b54efb52b5d93dc9e0ed97fce49564b
sha256:0800d17e37fb4f8194495b1a188f121e5b54efb52b5d93dc9e0ed97fce49564b: Pulling from myimage
8e3ba11ec2a2: Pull complete
Digest: sha256:0800d17e37fb4f8194495b1a188f121e5b54efb52b5d93dc9e0ed97fce49564b
Status: Downloaded newer image for myregistry.azurecr.io/myimage@sha256:0800d17e37fb4f8194495b1a188f121e5b54efb52b5d93dc9e0ed97fce49564b
Tagging myregistry.azurecr.io/myimage@sha256:0800d17e37fb4f8194495b1a188f121e5b54efb52b5d93dc9e0ed97fce49564b as myregistry.azurecr.io/myimage:signed
```

Se um cliente que tenha a confiança do conteúdo ativada tentar extrair uma etiqueta não assinada, a operação falha:

```console
$ docker pull myregistry.azurecr.io/myimage:unsigned
No valid trust data for unsigned
```

### <a name="behind-the-scenes"></a>Nos bastidores

Quando executa `docker pull`, o cliente do Docker utiliza a mesma biblioteca do [Notary CLI][docker-notary-cli] para pedir o mapeamento de resumo da etiqueta para SHA-256 relativo à etiqueta que está a extrair. Depois de validar as assinaturas nos dados fiáveis, o cliente diz ao Docker Engine para fazer uma “extração por resumo”. Durante a extração, o Engine utiliza a soma de verificação SHA-256 como endereço do conteúdo para pedir e validar o manifesto da imagem do registo de contentor do Azure.

## <a name="key-management"></a>Gestão de chaves

Conforme mencionado na saída `docker push` quando envia a primeira imagem fiável, a chave raiz é a mais sensível. Certifique-se de que cria uma cópia de segurança da mesma e que a armazena numa localização segura. Por predefinição, o cliente do Docker armazena as chaves de assinatura no seguinte diretório:

```sh
~/.docker/trust/private
```

Para criar uma cópia de segurança das chaves raiz e do repositório, comprima-as num arquivo e armazene-o em segurança offline (como num dispositivo de armazenamento USB). Por exemplo, no Bash:

```bash
umask 077; tar -zcvf docker_private_keys_backup.tar.gz ~/.docker/trust/private; umask 022
```

Juntamente com as chaves raiz e do repositório geradas localmente, o Azure Container Registry gera e armazena muitas outras quando envia uma imagem fiável. Para uma discussão detalhada das várias chaves na implementação da confiança do conteúdo do Docker, incluindo orientações de gestão adicionais, veja [Manage keys for content trust][docker-manage-keys] (Gerir chaves para a confiança do conteúdo), na documentação do Docker.

### <a name="lost-root-key"></a>Chave raiz perdida

Se perder o acesso à chave raiz, perde acesso às etiquetas assinadas em todos os repositórios cujas etiquetas tenham sido assinadas com essa chave. O Azure Container Registry não pode restaurar o acesso às etiquetas de imagens assinadas com chaves raiz que se tenham perdido. Para remover todos os dados fiáveis (assinaturas) do seu registo, desative e, depois, reative a confiança do conteúdo para o registo.

> [!WARNING]
> A desativação e reativação da confiança do conteúdo no registo **elimina todos os dados fiáveis de todas as etiquetas assinadas em todos os repositórios no seu registo**. Esta ação é irreversível. O Azure Container Registry não consegue recuperar os dados fiáveis eliminados. A desativação da confiança do conteúdo não elimina as imagens.

Para desativar a confiança do conteúdo no seu registo, navegue para o mesmo no portal do Azure. Em **POLÍTICAS**, selecione **Confiança do Conteúdo (Pré-visualização)** > **Desativada** > **Guardar**. Recebe um aviso de que todas as assinaturas no registo se vão perder. Selecione **OK** para eliminar permanentemente todas as assinaturas no seu registo.

![Desativar a confiança do conteúdo num registo no portal do Azure][content-trust-03-portal]

## <a name="next-steps"></a>Passos seguintes

Veja a documentação do Docker para obter mais informações sobre a confiança do conteúdo. Embora este artigo tenha abordado vários pontos importantes, a confiança do conteúdo é um tópico extenso e é descrito mais pormenorizadamente na documentação do Docker.

[Content trust in Docker][docker-content-trust] (Confiança do conteúdo no Docker)

<!-- IMAGES> -->
[content-trust-01-portal]: ./media/container-registry-content-trust/content-trust-01-portal.png
[content-trust-02-portal]: ./media/container-registry-content-trust/content-trust-02-portal.png
[content-trust-03-portal]: ./media/container-registry-content-trust/content-trust-03-portal.png

<!-- LINKS - external -->
[docker-content-trust]: https://docs.docker.com/engine/security/trust/content_trust
[docker-manage-keys]: https://docs.docker.com/engine/security/trust/trust_key_mng/
[docker-notary-cli]: https://docs.docker.com/notary/getting_started/
[docker-push]: https://docs.docker.com/engine/reference/commandline/push/
[docker-tag]: https://docs.docker.com/engine/reference/commandline/tag/
[terms-of-use]: https://azure.microsoft.com/support/legal/preview-supplemental-terms/

<!-- LINKS - internal -->
[azure-cli]: /cli/azure/install-azure-cli
