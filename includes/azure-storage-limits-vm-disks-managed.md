---
title: incluir ficheiro
description: incluir ficheiro
services: virtual-machines
author: roygara
ms.service: virtual-machines
ms.topic: include
ms.date: 09/24/2018
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: 4441ad1e2940892c1627cbc2d4ee0186e4cfda17
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/07/2018
ms.locfileid: "51264298"
---
**HDDs de máquina virtual gerida Standard**

| Tipo de Disco Standard  | S4               | S6               | S10             | S15 | S20              | S30              | S40              | S50              | S60 *             | S70 *             | S80 *             |
|---------------------|---------------------|---------------------|------------------|------------------|------------------|------------------|------------------|------------------|------------------|------------------|------------------|
| Tamanho do disco em GiB          | 32             | 64             | 128            | 256  | 512            | 1,024    | 2.048     | 4095    | 8,192     | 16,384     | 32.767     |
| IOPs por disco       | Até 500              | Até 500              | Até 500              | Até 500 | Até 500              | Até 500              | Até 500             | Até 500              | Até 1.300              | Até 2000              | Até 2000              |
| Débito por disco | Até 60 MiB/seg | Até 60 MiB/seg | Até 60 MiB/seg | Até 60 MiB/seg | Até 60 MiB/seg | Até 60 MiB/seg | Até 60 MiB/seg | Até 60 MiB/seg| Até 300 MiB/seg | Até 500 MiB/seg | Até 500 MiB/seg |

**SSDs de máquina virtual gerida Standard**

| Tipo de disco Standard SSD  | E10               | E15               | E20             | E30 | E40              | E50              | E60 *             | E70 *             | E80 *             |
|---------------------|---------------------|---------------------|------------------|------------------|------------------|------------------|------------------|------------------|------------------|
| Tamanho do disco em GiB           | 128             | 256             | 512            | 1,024  | 2.048            | 4095     | 8,192     | 16,384     | 32.767    |
| IOPs por disco       | Até 500              | Até 500              | Até 500              | Até 500 | Até 500              | Até 500              | Até 500             | Até 500              | Até 1.300              | Até 2000              | Até 2000              |
| Débito por disco | Até 60 MB/seg | Até 60 MB/seg | Até 60 MB/seg | Até 60 MB/seg | Até 60 MB/seg | Até 60 MB/seg | Até 60 MB/seg | Até 60 MB/seg| Até 300 MiB/seg |  Até 500 MiB/seg | Até 500 MiB/seg |

**Discos da máquina virtual geridos premium: por limites de disco**

| Tipo de disco Premium  | P4               | P6               | P10             | P15 | P20              | S30              | P40              | P50              | P60 *             | P70 *             | P80 *             |
|---------------------|---------------------|---------------------|------------------|------------------|------------------|------------------|------------------|------------------|------------------|------------------|------------------|
| Tamanho do disco em GiB           | 32             | 64             | 128            | 256  | 512            | 1,024    | 2.048     | 4095    | 8,192     | 16,384     | 32.767     |
| IOPs por disco       | Até 120 | Até 240              | Até 500              | Até 1,100 | Até 2,300              | Até 5000              | Até 7.500             | Até 7.500              | Até 12.500              | Máximo de 15 000              | Até 20 000              |
| Débito por disco | Até 25 MiB/seg | Máximo de 50 MiB/seg | Até 100 MiB/seg | Até 125 MiB/seg | Até 150 MiB/seg | Até 200 MiB/seg | Até 250 MiB/seg | Até 250 MiB/seg| Até 480 MiB/seg | Até 750 MiB/seg | Até 750 MiB/seg |

**Discos da máquina virtual geridos premium: por limites de VM**

| Recurso | Limite Predefinido |
| --- | --- |
| IOPs Máx Por VM |80.000 IOPS com GS5 VM |
| Débito máx por VM |2.000 MB/s com GS5 VM |
