---
title: Tutorial de limpeza de cluster autónomo do Service Fabric - Azure Service Fabric | Microsoft Docs
description: Neste tutorial saiba como limpar o seu cluster autónomo
services: service-fabric
documentationcenter: .net
author: david-stanford
manager: timlt
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: tutorial
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 05/11/2018
ms.author: dastanfo
ms.custom: mvc
ms.openlocfilehash: d92712200920f135e362e15e1d9536dc2342ebaa
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/16/2018
ms.locfileid: "34211094"
---
# <a name="tutorial-clean-up-your-standalone-cluster"></a>Tutorial: limpe o seu cluster autónomo

Os clusters autónomos do Service Fabric oferecem a opção de escolher o seu ambiente e criar um cluster como parte da abordagem "qualquer SO, qualquer cloud" que o Service Fabric está a realizar. Nesta série de tutoriais, vai criar um cluster autónomo alojado no AWS e instale uma aplicação no mesmo.

Este tutorial é a quarta parte de uma série. Esta parte do tutorial mostra como limpar os recursos do AWS que criou para alojar o seu cluster do Service Fabric.

Na quarta parte da série, ficará a saber como:

> [!div class="checklist"]
> * Apagar um cluster do Service Fabric
> * Apagar os seus recursos do AWS

## <a name="clean-up-service-fabric-cluster"></a>Apagar um cluster do Service Fabric

* RDP para a instância de EC2 que utilizou para instalar o Service Fabric
* Abrir o PowerShell
* Altere o diretório para a pasta extraída do segundo tutorial.
* Execute o seguinte comando para remover o cluster do Service Fabric:

```powershell
.\RemoveServiceFabricCluster.ps1 -ClusterConfigFilePath .\ClusterConfig.Unsecure.MultiMachine.json
```

* `Y` Quando lhe for pedido, se foi efetuada com êxito, a saída terá o seguinte aspeto, em que os endereços IP são substituídos:

```powershell
Best Practices Analyzer completed successfully.
Removing configuration from machine 172.31.21.141
Removing configuration from machine 172.31.27.1
Removing configuration from machine 172.31.20.163
Configuration removed from machine 172.31.21.141
Configuration removed from machine 172.31.27.1
Configuration removed from machine 172.31.20.163
The cluster is successfully removed.
```

## <a name="clean-up-aws-resources"></a>Apague os recursos do AWS

* Inicie sessão na sua conta AWS
* Vá para a consola EC2.
* Selecione os três nós que criou na primeira parte do tutorial.
* Clique em **Ações** > **Estado da Instância** > **Terminar**

## <a name="next-steps"></a>Passos seguintes

Na quarta parte da série, aprendeu a apagar os recursos criados nos passos anteriores.

> [!div class="checklist"]
> * Apague os seus recursos

> [!div class="nextstepaction"]
> [Voltar ao início](service-fabric-tutorial-standalone-create-infrastructure.md)