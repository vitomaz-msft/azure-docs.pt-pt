---
title: Dimensionamento do SAP HANA no Azure (instâncias grandes) | Documentos da Microsoft
description: Dimensionamento de SAP HANA no Azure (instâncias grandes).
services: virtual-machines-linux
documentationcenter: ''
author: RicksterCDN
manager: jeconnoc
editor: ''
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 09/04/2018
ms.author: saghorpa
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: d0d72a9ef592579b21d082111ab8ad00f8995de2
ms.sourcegitcommit: d211f1d24c669b459a3910761b5cacb4b4f46ac9
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/06/2018
ms.locfileid: "44028711"
---
# <a name="sizing"></a>Dimensionamento

Dimensionamento para a instância grande do HANA não é diferente de dimensionamento para o HANA em geral. Para existente e sistemas que pretende mover de outros RDBMS ao HANA, SAP fornece vários relatórios que são executadas em seus sistemas SAP existentes. Se a base de dados for movido para o HANA, estes relatórios Verifique os dados e calcular os requisitos de memória para a instância do HANA. Para obter mais informações sobre como executar esses relatórios e obtenha os mais recentes patches ou versões, leia as notas de SAP seguintes:

- [A nota SAP #1793345 - Dimensionar para SAP Suite em HANA](https://launchpad.support.sap.com/#/notes/1793345)
- [SAP Note #1872170 - Suite em HANA e S/4 HANA relatório de dimensionamento](https://launchpad.support.sap.com/#/notes/1872170)
- [A nota SAP #2121330 - FAQ: SAP BW em HANA relatório de dimensionamento](https://launchpad.support.sap.com/#/notes/2121330)
- [SAP Note #1736976 - relatório de dimensionamento para BW em HANA](https://launchpad.support.sap.com/#/notes/1736976)
- [SAP Note #2296290 - Dimensionar novo relatório para BW em HANA](https://launchpad.support.sap.com/#/notes/2296290)

Para implementações de campo verde, Sizer rápido do SAP está disponível para calcular os requisitos de memória da implementação de software SAP em cima do HANA.

Requisitos de memória para HANA aumentam à medida que aumenta o volume de dados. Tenha em atenção o consumo de memória atual para o ajudar a prever o que ele vai ser no futuro. Com base nos requisitos de memória, em seguida, pode mapear o seu pedido em um dos SKUs de instância grande do HANA.

**Passos seguintes?**
- Consulte [requisitos de integração](hana-onboarding-requirements.md)