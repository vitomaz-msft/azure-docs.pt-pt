---
title: Exemplos do Azure PowerShell - serviço de aplicações | Documentos da Microsoft
description: Exemplos do Azure PowerShell - serviço de aplicações
services: app-service
documentationcenter: app-service
author: syntaxc4
manager: erikre
editor: ggailey777
tags: azure-service-management
ms.assetid: b48d1137-8c04-46e0-b430-101e07d7e470
ms.service: app-service
ms.devlang: na
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: app-service
ms.date: 03/08/2017
ms.author: cfowler
ms.custom: mvc
ms.openlocfilehash: 211284f4eff112b9aebec7fa1a031f292a4a92f4
ms.sourcegitcommit: beb4fa5b36e1529408829603f3844e433bea46fe
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/22/2018
ms.locfileid: "52290744"
---
# <a name="azure-powershell-samples"></a>Exemplos do Azure PowerShell

A tabela seguinte inclui ligações para scripts do PowerShell criadas com o Azure PowerShell.

| | |
|-|-|
|**Criar aplicação**||
| [Criar uma aplicação com a implementação do GitHub](./scripts/app-service-powershell-deploy-github.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Cria uma aplicação web do Azure que solicita código a partir do GitHub. |
| [Criar uma aplicação com implementação contínua a partir do GitHub](./scripts/app-service-powershell-continuous-deployment-github.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Cria uma aplicação web do Azure que implementa continuamente o código a partir do GitHub. |
| [Criar uma aplicação e implementar código com FTP](./scripts/app-service-powershell-deploy-ftp.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Cria um web do Azure ficheiros de aplicação e o carregamento de um diretório local com o FTP. |
| [Criar uma aplicação e implementar código a partir de um repositório de Git local](./scripts/app-service-powershell-deploy-local-git.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Cria uma aplicação web do Azure e configura o push de código de um repositório de Git local. |
| [Criar uma aplicação e implementar código num ambiente de teste](./scripts/app-service-powershell-deploy-staging-environment.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Cria uma aplicação web do Azure com um bloco de implementação para alterações de código de teste. |
|**Configurar aplicação**||
| [Mapear um domínio personalizado para uma aplicação](./scripts/app-service-powershell-configure-custom-domain.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Cria uma aplicação web do Azure e mapeia um nome de domínio personalizado ao mesmo. |
| [Vincular um certificado SSL personalizado a uma aplicação](./scripts/app-service-powershell-configure-ssl-certificate.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Cria uma aplicação web do Azure e vincula o certificado SSL de um nome de domínio personalizado ao mesmo. |
|**Aplicação de dimensionamento**||
| [Dimensionar manualmente uma aplicação](./scripts/app-service-powershell-scale-manual.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Cria uma aplicação web do Azure e dimensiona-a entre 2 instâncias. |
| [Dimensionar uma aplicação todo o mundo com uma arquitetura de elevada disponibilidade](./scripts/app-service-powershell-scale-high-availability.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Cria duas aplicações web do Azure em duas regiões geográficas diferentes e disponibiliza-os através de um único ponto final de utilização do Gestor de tráfego do Azure. |
|**Ligar a aplicação aos recursos**||
| [Ligar uma aplicação para uma base de dados SQL](./scripts/app-service-powershell-connect-to-sql.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Cria uma aplicação web do Azure e uma base de dados SQL, em seguida, adiciona a cadeia de ligação de base de dados para as definições da aplicação. |
| [Ligar uma aplicação a uma conta de armazenamento](./scripts/app-service-powershell-connect-to-storage.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Cria uma aplicação web do Azure e uma conta de armazenamento, em seguida, adiciona a cadeia de ligação de armazenamento para as definições da aplicação. |
|**Criar cópias de segurança e restaurar a aplicação**||
| [Fazer cópias de segurança de uma aplicação](./scripts/app-service-powershell-backup-onetime.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Cria uma aplicação web do Azure e cria uma cópia de segurança única para o mesmo. |
| [Criar uma cópia de segurança agendada para uma aplicação](./scripts/app-service-powershell-backup-scheduled.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Cria uma aplicação web do Azure e cria uma cópia de segurança agendada para o mesmo. |
| [Eliminar uma cópia de segurança para uma aplicação](./scripts/app-service-powershell-backup-delete.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Elimina uma cópia de segurança existente para uma aplicação. |
| [Restaurar uma aplicação a partir de cópia de segurança](./scripts/app-service-powershell-backup-restore.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Restaura uma aplicação a partir de uma cópia de segurança anteriormente concluída. |
| [Restaurar uma cópia de segurança entre subscrições](./scripts/app-service-powershell-backup-restore-diff-sub.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Restaura uma aplicação web a partir de uma cópia de segurança noutra subscrição. |
|**Monitorizar aplicação**||
| [Monitorizar uma aplicação com os registos de servidor web](./scripts/app-service-powershell-monitor.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Cria uma aplicação web do Azure, ativa o registo para ele e transfere os registos para o computador local. |
| | |
