---
title: Os custos de software de reservas Windows do Azure | Documentos da Microsoft
description: Saiba quais medidores de software do Windows não estão incluídos nos custos de instância de VM reservada do Azure.
services: billing
documentationcenter: ''
author: manish-shukla01
manager: manshuk
editor: ''
tags: billing
ms.service: billing
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/09/2018
ms.author: cwatson
ms.openlocfilehash: 6ec1d0e1b8a768dd61a42c8e9284a93aee3c9337
ms.sourcegitcommit: d1aef670b97061507dc1343450211a2042b01641
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/27/2018
ms.locfileid: "47392539"
---
# <a name="windows-software-costs-not-included-with-azure-reserved-vm-instances"></a>Custos de software do Windows não são incluídos com instâncias de VM reservadas do Azure

Se não tiver um benefício de utilização híbrida do Azure nas suas instâncias de máquina virtual reservada, em seguida, é-lhe cobrada os medidores de software do Windows listados na secção seguinte.

## <a name="windows-software-meters-not-included-in-reservation-cost"></a>Medidores de software do Windows não incluídos no custo de reserva

| Id do Medidor | MeterName no ficheiro de utilização | Utilizado por VM |
| ------- | ------------------------| --- |
| e7e152ac-f29c-4cce-ad6e-026192c01ef2 | Períodos de rajada de instâncias reservadas do Windows de reserva Server (1 núcleo) | Série B |
| cac255a2-9f0f-4c62-8bd6-f0fa449c5f76 | Períodos de rajada de instâncias reservadas do Windows de reserva Server (2 núcleos) | Série B |
| 09756b58-3fb5-4390-976d-9ddd14f9ed18 | Períodos de rajada de instâncias reservadas do Windows de reserva Server (4 núcleos) | Série B |
| e828cb37-5920-4dc7-b30f-664e4dbcb6c7 | Períodos de rajada de instâncias reservadas do Windows de reserva Server (8 núcleos) | Série B |
| f65a06cf-c9c3-47a2-8104-f17a8542215a | Windows de instâncias reservadas do Windows Server (1 núcleo) | Tudo, exceto a série B |
| b99d40ae-41fe-4d1d-842b-56d72f3d15ee | Windows de instâncias reservadas do Windows Server (2 núcleos) | Tudo, exceto a série B |
| 1cb88381-0905-4843-9ba2-7914066aabe5 | Windows de instâncias reservadas do Windows Server (4 núcleos) | Tudo, exceto a série B |
| 07d9e10d-3e3e-4672-ac30-87f58ec4b00a | Windows de instâncias reservadas do Windows Server (6 núcleos) | Tudo, exceto a série B |
| 603f58d1-1e96-460b-a933-ce3775ac7e2e | Windows de instâncias reservadas do Windows Server (8 núcleos) | Tudo, exceto a série B |
| 36aaadda-da86-484a-b465-c8b5ab292d71 | Windows de instâncias reservadas do Windows Server (12 núcleos) | Tudo, exceto a série B |
| 02968a6b-1654-4495-ada6-13f378ba7172 | Windows de instâncias reservadas do Windows Server (16 núcleos) | Tudo, exceto a série B |
| 175434d8-75f9-474b-9906-5d151b6bed84 | Windows de instâncias reservadas do Windows Server (20 núcleos) | Tudo, exceto a série B |
| 77eb6dd0-88f5-4a16-ab39-05d1742efb25 | Windows de instâncias reservadas do Windows Server (24 núcleos) | Tudo, exceto a série B |
| 0d5bdf46-b719-4b1f-a780-b9bdfffd0591 | Windows de instâncias reservadas do Windows Server (32 núcleos) | Tudo, exceto a série B |
| f1214b5c-cc16-445f-be6c-a3bb75f8395a | Windows de instâncias reservadas do Windows Server (40 núcleos) | Tudo, exceto a série B |
| 637b7c77-65ad-4486-9cc7-dc7b3e9a8731 | Windows de instâncias reservadas do Windows Server (64 núcleos) | Tudo, exceto a série B |
| da612742-e7cc-4ca3-9334-0fb7234059cd | Windows de instâncias reservadas do Windows Server (72 núcleos) | Tudo, exceto a série B |
| a485cb8c-069b-4cf3-9a8e-ddd84b323da2 | Windows de instâncias reservadas do Windows Server (128 núcleos) | Tudo, exceto a série B |
| 904c5c71-1eb7-43a6-961c-d305a9681624 | Windows de instâncias reservadas do Windows Server (256 núcleos) | Tudo, exceto a série B |
| 6fdab81b-4284-4df9-8939-c237cc7462fe | Windows de instâncias reservadas do Windows Server (96 núcleos) | Tudo, exceto a série B |

Pode obter o custo de cada um destes medidores pela API de RateCard do Azure. Para obter informações sobre como obter as taxas de para um medidor do azure, consulte [Obtenha informações sobre o preço e os metadados de recursos utilizados numa subscrição do Azure](https://msdn.microsoft.com/library/azure/mt219004).

## <a name="next-steps"></a>Passos Seguintes
Para saber mais sobre as reservas do Azure, veja os artigos seguintes:

- [Quais são as reservas do Azure?](billing-save-compute-costs-reservations.md)
- [Efetue o pré-pagamento de máquinas virtuais com instâncias de VM reservadas do Azure](../virtual-machines/windows/prepay-reserved-vm-instances.md)
- [Gerir o Azure Reservations](billing-manage-reserved-vm-instance.md)
- [Compreender como o desconto de reserva é aplicado](billing-understand-vm-reservation-charges.md)
- [Compreender a utilização de reserva para a sua subscrição pay as you go](billing-understand-reserved-instance-usage.md)
- [Compreender a utilização de reserva para inscrição da sua empresa](billing-understand-reserved-instance-usage-ea.md)

## <a name="need-help-contact-support"></a>Precisa de ajuda? Contactar o suporte

Se ainda tiver mais perguntas, [contacte o suporte](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) para a sua questão resolvidos rapidamente.



