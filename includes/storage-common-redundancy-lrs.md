---
title: incluir ficheiro
description: incluir ficheiro
services: storage
author: tamram
ms.service: storage
ms.topic: include
ms.date: 10/24/2018
ms.author: tamram
ms.custom: include file
ms.openlocfilehash: a948a40e638b5f6e042c62ab58c2b7b65a49cd4e
ms.sourcegitcommit: ae45eacd213bc008e144b2df1b1d73b1acbbaa4c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/01/2018
ms.locfileid: "50741985"
---
Armazenamento localmente redundante (LRS) fornece, pelo menos, 99,999999999% (11 nines) durabilidade dos objetos ao longo de um ano. LRS fornece este durabilidade de objetos ao replicar os seus dados para uma unidade de escala de armazenamento. Centro de dados, localizado na região em que criou sua conta de armazenamento aloja a unidade de escala de armazenamento. Um pedido de escrita para uma conta de armazenamento LRS com êxito devolve apenas depois dos dados são gravados para todas as réplicas. Cada réplica reside em domínios de falha e domínios de atualização dentro de uma unidade de escala de armazenamento.

Uma unidade de escala de armazenamento é uma coleção de racks de nós de armazenamento. Um domínio de falha (FD) é um grupo de nós que representam uma unidade física de falha. Considere um domínio de falha como nós que pertencem ao mesmo rack físico. Um domínio de atualização (UD) é um grupo de nós que são atualizados em conjunto durante o processo de uma atualização do serviço (implementação). As réplicas são distribuídas em UDs e FDs dentro de uma unidade de escala de armazenamento. Esta arquitetura assegura que seus dados estão disponíveis se uma falha de hardware afetar um bastidor único ou quando nós sejam atualizados durante uma atualização do serviço.

LRS é a opção de replicação de custo mais baixo e oferece a durabilidade, pelo menos, em comparação comparada outras opções. Se ocorrer um desastre ao nível do Centro de dados (por exemplo, acionar ou sobrecarregar), todas as réplicas podem ser perdidas ou irrecuperáveis. Para mitigar este risco, a Microsoft recomenda a utilização de armazenamento com redundância de zona (ZRS) ou armazenamento georredundante (GRS).

* Se a aplicação armazena os dados que podem facilmente ser reconstruídos o caso de perda de dados, pode optar por LRS.
* Alguns aplicativos são restritos para replicar dados apenas dentro de um país devido a requisitos de governação de dados. Em alguns casos, as regiões emparelhadas em que os dados são replicados para contas GRS poderão estar em outro país. Para obter mais informações sobre as regiões emparelhadas, consulte [regiões do Azure](https://azure.microsoft.com/regions/).
