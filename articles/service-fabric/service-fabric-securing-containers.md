---
title: Importar certificados para um contentor em execução no Azure Service Fabric | Documentos da Microsoft
description: Saiba como importar ficheiros de certificado para um serviço de contentor do Service Fabric.
services: service-fabric
documentationcenter: .net
author: TylerMSFT
manager: timlt
editor: ''
ms.assetid: ab49c4b9-74a8-4907-b75b-8d2ee84c6d90
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 2/23/2018
ms.author: twhitney, subramar
ms.openlocfilehash: d49c16741f581b2ad09dc173e8380fdf77391dbe
ms.sourcegitcommit: d372d75558fc7be78b1a4b42b4245f40f213018c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/09/2018
ms.locfileid: "51299066"
---
# <a name="import-a-certificate-file-into-a-container-running-on-service-fabric"></a>Importar um ficheiro de certificado para um contentor em execução no Service Fabric

Pode proteger os seus serviços de contentor ao especificar um certificado. O Service Fabric fornece um mecanismo para serviços dentro de um contentor para aceder a um certificado que está instalado em nós num cluster do Windows ou Linux (versão 5.7 ou superior). O certificado tem de estar instalado no LocalMachine em todos os nós do cluster. As informações do certificado são fornecidas no manifesto do aplicativo sob o `ContainerHostPolicies` marca como o fragmento seguinte mostra:

```xml
  <ContainerHostPolicies CodePackageRef="NodeContainerService.Code">
    <CertificateRef Name="MyCert1" X509StoreName="My" X509FindValue="[Thumbprint1]"/>
    <CertificateRef Name="MyCert2" X509FindValue="[Thumbprint2]"/>
 ```

Para clusters do Windows, ao iniciar a aplicação, o tempo de execução lê os certificados e gera um ficheiro PFX e a palavra-passe para cada certificado. Este ficheiro PFX e a palavra-passe estão acessíveis no interior do contentor com as seguintes variáveis de ambiente: 

* Certificates_ServicePackageName_CodePackageName_CertName_PFX
* Certificates_ServicePackageName_CodePackageName_CertName_Password

Para clusters do Linux, os certificados (. PEM) são copiados através da loja especificada pelo X509StoreName para o contentor. As variáveis de ambiente correspondente no Linux são:

* Certificates_ServicePackageName_CodePackageName_CertName_PEM
* Certificates_ServicePackageName_CodePackageName_CertName_PrivateKey

Em alternativa, se já tiver os certificados no formulário necessário e pretende aceder ao mesmo dentro do contentor, pode criar um pacote de dados dentro de seu pacote de aplicação e especifique o seguinte no interior o manifesto da aplicação:

```xml
<ContainerHostPolicies CodePackageRef="NodeContainerService.Code">
  <CertificateRef Name="MyCert1" DataPackageRef="[DataPackageName]" DataPackageVersion="[Version]" RelativePath="[Relative Path to certificate inside DataPackage]" Password="[password]" IsPasswordEncrypted="[true/false]"/>
 ```

O serviço de contentor ou o processo é responsável por importar os ficheiros de certificado para o contentor. Para importar o certificado, pode utilizar `setupentrypoint.sh` scripts ou executar código personalizado dentro do processo de contentor. Eis o código de exemplo em C# para importar o ficheiro PFX:

```csharp
string certificateFilePath = Environment.GetEnvironmentVariable("Certificates_MyServicePackage_NodeContainerService.Code_MyCert1_PFX");
string passwordFilePath = Environment.GetEnvironmentVariable("Certificates_MyServicePackage_NodeContainerService.Code_MyCert1_Password");
X509Store store = new X509Store(StoreName.My, StoreLocation.CurrentUser);
string password = File.ReadAllLines(passwordFilePath, Encoding.Default)[0];
password = password.Replace("\0", string.Empty);
X509Certificate2 cert = new X509Certificate2(certificateFilePath, password, X509KeyStorageFlags.MachineKeySet | X509KeyStorageFlags.PersistKeySet);
store.Open(OpenFlags.ReadWrite);
store.Add(cert);
store.Close();
```
Este certificado PFX pode ser utilizado para autenticar a aplicação ou serviço ou uma comunicação segura com outros serviços. Por predefinição, os ficheiros estão ACLed apenas ao sistema. Pode ACL-lo para outras contas conforme exigido pelo serviço.

Como passo seguinte, leia os artigos seguintes:

* [Implementar um contentor do Windows no Service Fabric no Windows Server 2016](service-fabric-get-started-containers.md)
* [Implementar um contentor de Docker para o Service Fabric no Linux](service-fabric-get-started-containers-linux.md)
