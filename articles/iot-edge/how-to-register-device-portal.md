---
title: Registar um novo dispositivo Azure IoT Edge (portal) | Documentos da Microsoft
description: Utilize o portal do Azure para registar um novo dispositivo IoT Edge
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 06/05/2018
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: 6657203c76bc03a262fbcbd30b5bf74b5be140eb
ms.sourcegitcommit: 0fc99ab4fbc6922064fc27d64161be6072896b21
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/13/2018
ms.locfileid: "51577503"
---
# <a name="register-a-new-azure-iot-edge-device-from-the-azure-portal"></a>Registar um novo dispositivo Azure IoT Edge do portal do Azure

Antes de poder utilizar os seus dispositivos IoT com o Azure IoT Edge, tem de registá-los com o seu hub IoT. Depois de se registar um dispositivo, receberá uma cadeia de ligação que pode ser utilizada para configurar o dispositivo para cargas de trabalho do Edge. 

Este artigo mostra como registar um novo dispositivo IoT Edge com o portal do Azure.

## <a name="prerequisites"></a>Pré-requisitos

* Uma [IoT hub](../iot-hub/iot-hub-create-through-portal.md) na sua subscrição do Azure. 

## <a name="create-a-device"></a>Criar um dispositivo

No portal do Azure, os dispositivos do IoT Edge são criados e gerenciados separadamente a partir de dispositivos que ligar ao seu hub IoT, mas não são habilitados no edge. 

1. Inicie sessão para o [portal do Azure](https://portal.azure.com) e navegue até ao seu hub IoT. 
2. Selecione **IoT Edge** no menu.
3. Selecione **adicionar dispositivo IoT Edge**. 
4. Forneça um ID de dispositivo descritivo. 
5. Selecione **Guardar**. 

## <a name="view-all-devices"></a>Ver todos os dispositivos

São listados todos os dispositivos habilitados para edge que se ligam ao seu hub IoT a **IoT Edge** página. 

## <a name="retrieve-the-connection-string"></a>Obter a cadeia de ligação

Quando estiver pronto para configurar o dispositivo, terá da cadeia de ligação que liga o dispositivo físico com a respetiva identidade no IoT hub.

1. Partir do **IoT Edge** página no portal, clique no ID de dispositivo da lista de dispositivos periféricos. 
2. Copie o valor de qualquer um **cadeia de ligação (chave primária)** ou **cadeia de ligação (chave secundária)**. 

## <a name="next-steps"></a>Passos Seguintes

Saiba como [implementar módulos num dispositivo com o portal do Azure](how-to-deploy-modules-portal.md)
