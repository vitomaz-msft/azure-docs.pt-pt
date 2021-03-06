---
title: Atualização do antivírus do Windows Defender no Azure Stack
description: Obter detalhes sobre como antivírus é mantido atualizado no Azure Stack
services: azure-stack
author: PatAltimore
manager: femila
ms.service: azure-stack
ms.topic: article
ms.date: 09/26/2018
ms.author: patricka
ms.reviewer: fiseraci
ms.openlocfilehash: 59c59705e840d3cdd0d3c5310d84d711cd00c96a
ms.sourcegitcommit: d1aef670b97061507dc1343450211a2042b01641
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/27/2018
ms.locfileid: "47394017"
---
# <a name="update-windows-defender-antivirus-on-azure-stack"></a>Atualização do antivírus do Windows Defender no Azure Stack

[Antivírus do Windows Defender](https://docs.microsoft.com/windows/security/threat-protection/windows-defender-antivirus/windows-defender-antivirus-in-windows-10) é uma solução de antimalware que fornece segurança e proteção contra vírus. Todos os componentes de infraestrutura do Azure Stack (anfitriões Hyper-V e máquinas virtuais) é protegido com o antivírus do Windows Defender. Para proteção atualizada, definições do antivírus do Windows Defender, mecanismo e plataforma requerem atualizações periódicas. Como as atualizações são aplicadas depende da sua configuração.

## <a name="connected-scenario"></a>Cenário ligado

Para antimalware mecanismo de atualizações de definições e, o Azure Stack [fornecedor de recursos de atualização](azure-stack-updates.md#the-update-resource-provider) transfere as definições de antimalware e atualizações de mecanismos várias vezes por dia. Cada componente de infraestrutura do Azure Stack obtém a atualização do fornecedor de recursos de atualização e aplica automaticamente a atualização.

Para as atualizações de plataforma antimalware, aplicar a [atualização mensal do Azure Stack](azure-stack-apply-updates.md). A atualização mensal do Azure Stack inclui atualizações de plataforma do antivírus do Windows Defender para o mês.

## <a name="disconnected-scenario"></a>Cenário de desligado

 Aplicar a [atualização mensal do Azure Stack](azure-stack-apply-updates.md). A atualização mensal do Azure Stack inclui definições de antivírus do Windows Defender, mecanismo e as atualizações de plataforma para o mês.

## <a name="next-steps"></a>Passos Seguintes

[Saiba mais sobre a segurança do Azure Stack](azure-stack-security-foundations.md)
