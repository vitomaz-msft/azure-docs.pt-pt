---
title: O que é o Azure IoT Edge | Microsoft Docs
description: Descrição geral do serviço Azure IoT Edge
author: kgremban
manager: philmea
ms.reviewer: chipalost
ms.service: iot-edge
services: iot-edge
ms.topic: overview
ms.date: 06/12/2018
ms.author: kgremban
ms.custom: mvc
ms.openlocfilehash: 1a1281be1c1b58b21406dad5826e240ccac6c898
ms.sourcegitcommit: 6b7c8b44361e87d18dba8af2da306666c41b9396
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/12/2018
ms.locfileid: "51567769"
---
# <a name="what-is-azure-iot-edge"></a>O que é o Hub IoT Edge

O Azure IoT Edge move as análises e a lógica empresarial personalizada da cloud para os dispositivos, para que a sua organização se possa dedicar às informações relevantes e não à gestão de dados. Permita que a sua solução se dimensione verdadeiramente ao configurar o seu software IoT, implementá-lo nos dispositivos através de contentores padrão e monitorizá-lo a partir da cloud.

>[!NOTE]
>O Azure IoT Edge está disponível no escalão gratuito e standard do Hub IoT. O escalão gratuito destina-se apenas a testes e avaliação. Para obter mais informações sobre os escalões básico e standard, veja [How to choose the right IoT Hub tier](../iot-hub/iot-hub-scaling.md) (Como escolher o escalão do Hub IoT certo).

Nas soluções IoT, o valor empresarial é impulsionado pelas análises, mas nem todas as análises têm de estar na cloud. Se quiser que um dispositivo responda a emergências o mais depressa possível, pode realizar uma deteção de anomalias no próprio dispositivo. Da mesma forma, se pretender reduzir os custos de largura de banda e evitar terabytes de transferências de dados não processados, pode fazer a limpeza e a agregação de dados localmente. Depois, envie as informações para a cloud. 

O Azure IoT Edge é composto por três componentes:
* Os módulos do IoT Edge são contentores que executam serviços do Azure, serviços de terceiros ou o seu próprio código. São implementados nos dispositivos IoT Edge e executados localmente nos mesmos. 
* O runtime do IoT Edge é executado em cada dispositivo IoT Edge e gere os módulos implementados em cada um deles. 
* Uma interface baseada na cloud permite-lhe monitorizar e gerir dispositivos IoT Edge.

## <a name="iot-edge-modules"></a>Módulos do IoT Edge

Os módulos do IoT Edge são unidades de execução, atualmente implementadas como contentores compatíveis do Docker, e que executam a sua lógica de negócio na periferia. Podem ser configurados vários módulos para comunicarem entre si, criando um pipeline para processamento de dados. Pode desenvolver módulos personalizados ou empacotar determinados serviços do Azure em módulos que disponibilizam informações offline e na periferia. 

### <a name="artificial-intelligence-on-the-edge"></a>Inteligência Artificial na periferia

O Azure IoT Edge permite-lhe implementar processamento de eventos complexo, machine learning, reconhecimento de imagens e outras IAs de elevado valor sem ter de escrevê-los internamente. Alguns serviços do Azure, como as Funções do Azure, o Azure Stream Analytics e o Azure Machine Learning, podem ser executados no local através do Azure IoT Edge; contudo, não está limitado aos serviços do Azure. Qualquer pessoa pode criar módulos de IA e disponibilizá-los para utilização por parte da comunidade. 

### <a name="bring-your-own-code"></a>Traga o seu próprio código

E se quiser implementar o seu próprio código nos seus dispositivos, o Azure IoT Edge também o suporta. O Azure IoT Edge tem o mesmo modelo de programação dos serviços do Azure IoT. O mesmo código pode ser executado num dispositivo ou na cloud. O Azure IoT Edge suporta o Linux e o Windows, para que possa programar para uma plataforma à sua escolha. Suporta Java, .NET Core 2.0, Node.js, C e Python, de modo que os seus programadores possam programar numa linguagem que já conheçam e utilizar lógica empresarial já existente sem terem de escrevê-la do zero.

## <a name="iot-edge-runtime"></a>Runtime do IoT Edge

O runtime do Azure IoT Edge permite lógica personalizada e da cloud nos dispositivos IoT Edge. Encontra-se no dispositivo IoT Edge e faz operações de gestão e comunicação. O runtime realiza várias funções:

* Instala e atualiza as cargas de trabalho no dispositivo.
* Mantém as normas de segurança do Azure IoT Edge no dispositivo.
* Garante que os módulos do IoT Edge estão sempre em execução.
* Reporta o estado de funcionamento dos módulos à cloud, para monitorização remota.
* Facilita a comunicação entre dispositivos de folha a jusante e o dispositivo IoT Edge.
* Facilita a comunicação entre os módulos no dispositivo IoT Edge.
* Facilita a comunicação entre o dispositivo do Azure IoT e a cloud.

![O runtime do IoT Edge envia informações e relatórios para o Hub IoT.](./media/about-iot-edge/runtime.png)

Cabe-lhe apenas a si decidir de que forma utiliza os dispositivos Azure IoT Edge. Muitas vezes, o runtime é utilizado para implementar a IA nos gateways que agregam e processam dados de vários outros dispositivos no local. Não obstante, esta é apenas uma opção. Os dispositivos de folha também podem ser dispositivos Azure IoT Edge, independentemente de estarem ligados a um gateway ou diretamente à cloud.

O runtime do Azure IoT Edge é executado num vasto conjunto de dispositivos IoT, para que possa ser utilizado de muitas e diversas formas. Suporta os sistemas operativos Linux e Windows e abstrai detalhes do hardware. Utilize um dispositivo mais pequeno do que o Raspberry Pi 3, se não estiver a processar muitos dados, ou aumente verticalmente para um servidor industrializado, para executar cargas de trabalho intensivas em termos de recursos.

## <a name="iot-edge-cloud-interface"></a>Interface na cloud do IoT Edge

Gerir o ciclo de vida do software dos dispositivos da empresa é complicado. E gerir o ciclo de vida de milhões de dispositivos IoT heterogéneos é ainda mais difícil. As cargas de trabalho têm de ser criadas e configuradas para um determinado tipo de dispositivos, implementadas em escala em milhões de dispositivos na sua solução e monitorizadas para detetar dispositivos que possam estar a funcionar mal. Estas atividades não podem ser feitas individualmente por dispositivo; têm de o ser em escala.

O Azure IoT Edge integra-se facilmente com os aceleradores de soluções do Azure IoT para proporcionar um plano de controlo para as necessidades da sua solução. Os serviços cloud permitem que os utilizadores:

* Criem e configurem cargas de trabalho para serem executadas num tipo de dispositivo específico.
* Enviem cargas de trabalho para um conjunto de dispositivos.
* Monitorizem as cargas de trabalho em execução em dispositivos no terreno.

![A telemetria, as informações e as ações dos dispositivos são coordenadas com a cloud](./media/about-iot-edge/cloud-interface.png)

## <a name="next-steps"></a>Passos Seguintes

Experimente estes conceitos ao [implementar o IoT Edge num dispositivo simulado](quickstart.md).

 
