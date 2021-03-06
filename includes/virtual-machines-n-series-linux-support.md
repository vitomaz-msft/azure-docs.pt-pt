---
title: incluir ficheiro
description: incluir ficheiro
services: virtual-machines-linux
author: cynthn
ms.service: virtual-machines-linux
ms.topic: include
ms.date: 11/08/2018
ms.author: cynthn
ms.custom: include file
ms.openlocfilehash: eef61421de2a87750caac228d12888421f7442a8
ms.sourcegitcommit: 275eb46107b16bfb9cf34c36cd1cfb000331fbff
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/15/2018
ms.locfileid: "51716249"
---
## <a name="supported-distributions-and-drivers"></a>Distribuições e controladores suportados

### <a name="nvidia-cuda-drivers"></a>Controladores de NVIDIA CUDA

Controladores de NVIDIA CUDA para NC, NCv2, NCv3, ND e as VMs da série NDv2 (opcionais para a série NV) são suportados apenas nas distribuições de Linux listadas na tabela seguinte. Informações de controlador CUDA são atuais no momento da publicação. Para os mais recentes drivers executam CUDA, visite o [NVIDIA](https://developer.nvidia.com/cuda-zone) Web site. Certifique-se de que instala ou atualizar para os mais recentes drivers CUDA para a sua distribuição. 

> [!TIP]
> Como alternativa à instalação de controlador CUDA manual numa VM do Linux, pode implementar do Azure [máquina de Virtual de ciência de dados](../articles/machine-learning/data-science-virtual-machine/overview.md) imagem. As edições DSVM para Ubuntu 16.04 LTS ou CentOS 7.4 previamente instalar controladores de NVIDIA CUDA, CUDA Deep Neural biblioteca de rede e outras ferramentas.

| Distribuição | Controlador |
| --- | -- | 
| Ubuntu 16.04 LTS<br/><br/> Red Hat Enterprise Linux 7.3 ou 7.4<br/><br/> Baseada em centOS 7.3 ou 7.4, baseada em CentOS 7.4 HPC | NVIDIA CUDA 10.0, ramo de controlador R410 |

### <a name="nvidia-grid-drivers"></a>Controladores de GRID da NVIDIA

Microsoft redistribui programas de instalação de controladores de NVIDIA GRID NV e as VMs da série NVv2 utilizadas como estações de trabalho virtuais ou para aplicações virtuais. Instale apenas estes controladores GRID em VMs do Azure NV, apenas em sistemas operativos listados na tabela seguinte. Estes controladores incluem licenças de Software GPU Virtual GRADE no Azure. Não é necessário configurar um servidor de licenças de software de vGPU NVIDIA.

| Distribuição | Controlador |
| --- | -- |
| Ubuntu 16.04 LTS<br/><br/>Red Hat Enterprise Linux 7.3 ou 7.4<br/><br/>Baseada em centOS 7.3 ou 7.4 | NVIDIA GRID 6.2, ramo de controlador R390|

> [!WARNING] 
> A instalação de software de terceiros em produtos Red Hat pode afetar os termos de suporte do Red Hat. Veja o [Red Hat Knowledgebase](https://access.redhat.com/articles/1067) (Base de Dados de Conhecimento do Red Hat).
>
