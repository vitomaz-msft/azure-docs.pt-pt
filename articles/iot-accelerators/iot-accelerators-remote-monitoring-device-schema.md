---
title: Esquema de dispositivo na solução de monitorização remota - Azure | Documentos da Microsoft
description: Este artigo descreve o esquema JSON que define um dispositivo simulado na solução de monitorização remota.
author: dominicbetts
manager: timlt
ms.author: dobett
ms.service: iot-accelerators
services: iot-accelerators
ms.date: 01/29/2018
ms.topic: conceptual
ms.openlocfilehash: f312f29e14c371e7b500f3eee6471151e3544513
ms.sourcegitcommit: 0c64460a345c89a6b579b1d7e273435a5ab4157a
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/31/2018
ms.locfileid: "43338860"
---
# <a name="understand-the-device-model-schema"></a>Compreender o esquema do modelo de dispositivo

Pode utilizar dispositivos simulados na solução de monitorização remota para testar o seu comportamento. Ao implementar a solução de monitorização remota, uma coleção de dispositivos simulados é aprovisionada automaticamente. Pode personalizar os dispositivos simulados existentes ou criar os seus próprios.

Este artigo descreve o esquema do modelo de dispositivo Especifica as capacidades e comportamento de um dispositivo simulado. O modelo do dispositivo é armazenado num ficheiro JSON.

Os artigos seguintes estão relacionados ao artigo atual:

* [Implementar o comportamento de modelo do dispositivo](iot-accelerators-remote-monitoring-device-behavior.md) descreve os ficheiros de JavaScript que utilizar para implementar o comportamento de um dispositivo simulado.
* [Criar um novo dispositivo simulado](iot-accelerators-remote-monitoring-create-simulated-device.md) coloca tudo isso e mostra-lhe como implementar um novo tipo de dispositivo simulado à sua solução.

Neste artigo, vai aprender a:

>[!div class="checklist"]
> * Utilizar um ficheiro JSON para definir um modelo de dispositivo simulado
> * Especifique as propriedades do dispositivo simulado
> * Especifique a telemetria que do dispositivo simulado envia
> * Especifique os métodos de cloud para o dispositivo, que o dispositivo responde a

[!INCLUDE [iot-accelerators-device-schema](../../includes/iot-accelerators-device-schema.md)]

## <a name="next-steps"></a>Passos Seguintes

Este artigo descreveu como criar seu próprio modelo personalizado de dispositivo simulado. Este artigo mostrou como para:

<!-- Repeat task list from intro -->
>[!div class="checklist"]
> * Utilizar um ficheiro JSON para definir um modelo de dispositivo simulado
> * Especifique as propriedades do dispositivo simulado
> * Especifique a telemetria que do dispositivo simulado envia
> * Especifique os métodos de cloud para o dispositivo, que o dispositivo responde a

Agora que aprendeu sobre o esquema JSON, o passo seguinte sugerido é saber como [implementar o comportamento do seu dispositivo simulado](iot-accelerators-remote-monitoring-device-behavior.md).

Para obter mais informações para desenvolvedores sobre a solução de monitorização remota, consulte:

* [Guia de Referência para Programadores](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet/wiki/Developer-Reference-Guide)
* [Guia de Resolução de Problemas de Programadores](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet/wiki/Developer-Troubleshooting-Guide)
