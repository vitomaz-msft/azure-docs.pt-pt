---
title: incluir ficheiro
description: incluir ficheiro
services: backup
author: markgalioto
ms.service: backup
ms.topic: include
ms.date: 9/10/2018
ms.author: trinadhk;sogup
ms.custom: include file
ms.openlocfilehash: 64101ea5a3bbaac4a6b2e349a04d06ea84a87081
ms.sourcegitcommit: 5a9be113868c29ec9e81fd3549c54a71db3cec31
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/11/2018
ms.locfileid: "44381105"
---
Os limites seguintes aplicam-se a cópia de segurança do Azure.

| Identificador de limite | Limite Predefinido |
| --- | --- |
| Número de servidores/máquinas que podem ser registados por cada Cofre |50 para o Windows Server SCDPM/cliente <br/> 1000 para IaaS VMs |
| Tamanho da origem de dados para dados armazenados no armazenamento do cofre do Azure |Máx. de 54400 GB<sup>1</sup> |
| Número de cofres de cópia de segurança que podem ser criadas em cada subscrição do Azure |500 cofres de serviços de recuperação por região |
| Número de vezes que pode ser agendada a cópia de segurança por dia |3 por dia para o Windows Server/cliente <br/> 2 por dia para SCDPM <br/> Uma vez por dia para IaaS VMs |
| Discos de dados ligados a uma máquina virtual do Azure para cópia de segurança |16 |
| Tamanho do disco de dados individuais anexado a uma máquina virtual do Azure para cópia de segurança| 4095 GB|

* <sup>1</sup>o limite de 54400 GB não é aplicável a cópia de segurança de VM de IaaS.
 

