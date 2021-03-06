---
title: Implementar uma aplicação do Service Fabric num cluster no Azure | Microsoft Docs
description: Saiba como implementar uma aplicação num cluster a partir do Visual Studio.
services: service-fabric
documentationcenter: .net
author: rwike77
manager: msfussell
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: tutorial
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 07/12/2018
ms.author: ryanwi,mikhegn
ms.custom: mvc
ms.openlocfilehash: fe6df20d294a3b1802d396085c36a6587dc45730
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/07/2018
ms.locfileid: "51249086"
---
# <a name="tutorial-deploy-a-service-fabric-application-to-a-cluster-in-azure"></a>Tutorial: Implementar uma aplicação do Service Fabric num cluster no Azure

Este tutorial é a segunda parte de uma série. Mostra-lhe como implementar uma aplicação do Azure Service Fabric num novo cluster no Azure.

Neste tutorial, ficará a saber como:
> [!div class="checklist"]
> * Criar um party cluster.
> * Implementar uma aplicação num cluster remoto com o Visual Studio.

Nesta série de tutoriais, ficará a saber como:
> [!div class="checklist"]
> * [Criar uma aplicação .NET do Service Fabric](service-fabric-tutorial-create-dotnet-app.md).
> * Implementar a aplicação num cluster remoto.
> * [Adicionar um ponto final HTTPS a um serviço de front-end ASP.NET Core](service-fabric-tutorial-dotnet-app-enable-https-endpoint.md).
> * [Configurar CI/CD com o Azure Pipelines](service-fabric-tutorial-deploy-app-with-cicd-vsts.md).
> * [Configurar a monitorização e os diagnósticos da aplicação](service-fabric-tutorial-monitoring-aspnet.md).

## <a name="prerequisites"></a>Pré-requisitos

Antes de começar este tutorial:

* Se não tiver uma subscrição do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
* [Instale o Visual Studio 2017](https://www.visualstudio.com/) e as cargas de trabalho de **desenvolvimento no Azure** e **desenvolvimento na Web e em ASP.NET**.
* [Instale o SDK do Service Fabric](service-fabric-get-started.md).

## <a name="download-the-voting-sample-application"></a>Transferir o exemplo de aplicação de votação

Se não conseguiu criar a aplicação de votação de exemplo na [primeira parte desta série de tutoriais](service-fabric-tutorial-create-dotnet-app.md), pode transferi-la. Numa janela de comando, execute o seguinte comando para clonar o repositório da aplicação de exemplo para o seu computador local.

```git
git clone https://github.com/Azure-Samples/service-fabric-dotnet-quickstart 
```

## <a name="publish-to-a-service-fabric-cluster"></a>Publicar num cluster do Service Fabric

Agora que a aplicação está pronta, pode implementá-la num cluster diretamente a partir do Visual Studio. Um [cluster do Service Fabric](https://docs.microsoft.com/azure/service-fabric/service-fabric-deploy-anywhere) é um conjunto ligado à rede de máquinas virtuais ou físicas, no qual os microsserviços são implementados e geridos.

Para este tutorial, tem duas opções de implementação da aplicação de votação num cluster do Service Fabric com o Visual Studio:

* Publicar num cluster de avaliação (party). 
* Publique num cluster existente na sua subscrição. Pode criar clusters do Service Fabric através do [portal do Azure](https://portal.azure.com) com os scripts do [PowerShell](./scripts/service-fabric-powershell-create-secure-cluster-cert.md) ou da [CLI do Azure](./scripts/cli-create-cluster.md) ou a partir de um [modelo do Azure Resource Manager](service-fabric-tutorial-create-vnet-and-windows-cluster.md).

> [!NOTE]
> Muitos serviços utilizam proxy inverso para comunicar entre si. Os clusters criados a partir do Visual Studio e os clusters de terceiros têm proxy inverso ativado por predefinição. Se estiver a utilizar um cluster existente, tem de [ativar o proxy inverso no cluster](service-fabric-reverseproxy-setup.md).


### <a name="find-the-voting-web-service-endpoint-for-your-azure-subscription"></a>Encontrar o ponto final de serviço Web de votação para a sua subscrição do Azure

Para publicar a aplicação de votação na sua própria subscrição do Azure, encontre o ponto final do serviço Web de front-end. Se utilizar um party cluster, ligue à porta 8080 com o exemplo de votação aberto automaticamente. Não precisa de configurá-lo no balanceador de carga do party cluster.

O serviço Web de front-end está à escuta numa porta específica. Quando implementa a aplicação num cluster do Azure, tanto o cluster como a aplicação são executados atrás de um balanceador de carga do Azure. A porta da aplicação tem de ser aberta com uma regra no balanceador de carga do Azure do cluster. A porta aberta envia o tráfego de entrada para o serviço Web. A porta encontra-se no ficheiro **VotingWeb/PackageRoot/ServiceManifest.xml** no elemento **Endpoint**. Um exemplo é a porta 8080.

```xml
<Endpoint Protocol="http" Name="ServiceEndpoint" Type="Input" Port="8080" />
```

Para a sua subscrição do Azure, abra esta porta através de uma regra de balanceamento de carga no Azure com um [script do PowerShell](./scripts/service-fabric-powershell-open-port-in-load-balancer.md) ou através do balanceador de carga para este cluster no [portal do Azure](https://portal.azure.com).

### <a name="join-a-party-cluster"></a>Aderir a um party cluster

> [!NOTE]
>  Se quiser publicar a aplicação no seu próprio cluster numa subscrição do Azure, avance para a secção [Publicar a aplicação com o Visual Studio](#publish-the-application-by-using-visual-studio). 

Os clusters comemorativos são clusters do Service Fabric gratuitos e de tempo limitado, alojados no Azure e executados pela equipa do Service Fabric. Qualquer pessoa pode implementar aplicações e obter informações sobre a plataforma. O cluster utiliza um único certificado autoassinado para a segurança de nó para nó e cliente para nó.

Inicie sessão e [adira a um cluster do Windows](https://aka.ms/tryservicefabric). Para transferir o certificado PFX para o seu computador, selecione a ligação **PFX**. Selecione a ligação **Como ligar a um Party cluster seguro?** e copie a palavra-passe do certificado. O certificado, a palavra-passe do certificado e o valor do **Ponto final da ligação** são utilizados nos passos seguintes.

![PFX e ponto final de ligação](./media/service-fabric-quickstart-dotnet/party-cluster-cert.png)

> [!Note]
> Existe um número limitado de party clusters por hora. Se obtiver um erro ao tentar inscrever-se num party cluster, aguarde e tente novamente. Ou siga estes passos no tutorial [Implementar uma aplicação .NET](https://docs.microsoft.com/azure/service-fabric/service-fabric-tutorial-deploy-app-to-party-cluster#deploy-the-sample-application) para criar um cluster do Service Fabric na sua subscrição do Azure e implementar a aplicação no mesmo. Se não tiver uma subscrição do Azure, pode [criar uma conta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
>

No seu computador Windows, instale o PFX no arquivo de certificados **CurrentUser\My**.

```powershell
PS C:\mycertificates> Import-PfxCertificate -FilePath .\party-cluster-873689604-client-cert.pfx -CertStoreLocation Cert:\CurrentUser\My -Password (ConvertTo-SecureString 873689604 -AsPlainText -Force)


   PSParentPath: Microsoft.PowerShell.Security\Certificate::CurrentUser\My

Thumbprint                                Subject
----------                                -------
3B138D84C077C292579BA35E4410634E164075CD  CN=zwin7fh14scd.westus.cloudapp.azure.com
```

Não se esqueça do thumbprint no passo seguinte.

> [!Note]
> Por predefinição, o serviço de front-end da Web está configurado para escutar tráfego de entrada na porta 8080. A porta 8080 está aberta no party cluster. Se tiver de alterar a porta da aplicação, altere-a para uma das que estão abertas no party cluster.
>

### <a name="publish-the-application-by-using-visual-studio"></a>Publicar a aplicação com o Visual Studio

Agora que a aplicação está pronta, pode implementá-la num cluster diretamente a partir do Visual Studio.

1. Clique com o botão direito do rato em **Votação** no Explorador de Soluções. Escolha **Publicar**. É apresentada a caixa de diálogo **Publicar**.

2. Copie o **Ponto Final da Ligação** a partir da página do party cluster ou da subscrição do Azure para o campo **Ponto Final da Ligação**. Um exemplo é `zwin7fh14scd.westus.cloudapp.azure.com:19000`. Selecione **Parâmetros de Ligação Avançados**.  Certifique-se de que os valores **FindValue** e **ServerCertThumbprint** correspondem ao thumbprint do certificado instalado num passo anterior para um party cluster ou ao certificado correspondente à sua subscrição do Azure.

    ![Publicar uma aplicação do Service Fabric](./media/service-fabric-quickstart-dotnet/publish-app.png)

    Cada aplicação no cluster tem de ter um nome exclusivo. Os party clusters são ambientes públicos e partilhados, pelo que poderá haver um conflito com aplicações já existentes. Se houver um conflito de nomes, mude o nome do projeto do Visual Studio e reimplemente-o.

3. Selecione **Publicar**.

4. Para aceder à aplicação de votação no cluster, abra um browser e introduza o endereço do cluster seguido de **:8080**. Ou introduza outra porta, se estiver configurada uma. Um exemplo é `http://zwin7fh14scd.westus.cloudapp.azure.com:8080`. Deverá ver a aplicação em execução no cluster no Azure. Na página Web de votação, experimente adicionar e eliminar as opções de votação e votar numa ou em várias destas opções.

    ![Exemplo de votação do Service Fabric](./media/service-fabric-quickstart-dotnet/application-screenshot-new-azure.png)


## <a name="next-steps"></a>Passos seguintes

Avance para o tutorial seguinte:
> [!div class="nextstepaction"]
> [Ativar HTTPS](service-fabric-tutorial-dotnet-app-enable-https-endpoint.md)
