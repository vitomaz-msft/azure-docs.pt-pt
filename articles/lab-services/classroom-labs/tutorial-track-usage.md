---
title: Acompanhar a utilização de um laboratório no Azure Lab Services | Microsoft Docs
description: Neste tutorial, enquanto criador/proprietário do laboratório, vai acompanhar a utilização do mesmo.
services: lab-services
documentationcenter: na
author: spelluru
manager: ''
editor: ''
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.custom: mvc
ms.date: 11/14/2018
ms.author: spelluru
ms.openlocfilehash: 49d5761e3e37e1265938d1f1b27324de667a13ca
ms.sourcegitcommit: 275eb46107b16bfb9cf34c36cd1cfb000331fbff
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/15/2018
ms.locfileid: "51707100"
---
# <a name="tutorial-track-usage-of-a-lab-in-azure-lab-service"></a>Tutorial: Acompanhar a utilização de um laboratório no Azure Lab Services
Este tutorial mostra-lhe como é que os criadores/proprietários de laboratórios podem acompanhar a utilização dos laboratórios.

Neste tutorial, irá realizar as seguintes ações:

> [!div class="checklist"]
> * Ver utilizadores registados no laboratório
> * Ver a utilização das VMs no laboratório
> * Gerir VMs de alunos 


## <a name="view-users-registered-with-the-lab"></a>Ver utilizadores registados no laboratório

1. Navegue para o [site do Azure Lab Services](https://labs.azure.com). 
2. Selecione **Iniciar sessão** e introduza as suas credenciais. O Azure Lab Services suporta contas organizacionais e contas Microsoft.
3. Na página **Os meus laboratórios**, selecione o laboratório cuja utilização quer acompanhar. 
4. Selecione o separador **Users** (Utilizadores). Verá os alunos que se registaram no laboratório. Selecione **Registration link** (Ligação de registo), copie a ligação e envie-a para alunos novos que ainda não se tenham registado no mesmo. 

    ![Utilizadores registados](../media/tutorial-track-usage/registered-users.png)

## <a name="view-the-usage-of-vms-in-the-lab"></a>Ver a utilização das VMs no laboratório 

1. Selecione **Máquinas virtuais**, no menu do lado esquerdo. 
2. Confirme que vê o estado das VMs e o número de horas que estiveram em execução. O tempo gasto num estudante que VM não contabiliza em relação ao tempo de utilização, mostrado na última coluna. 

    ![Utilização de VM](../media/tutorial-track-usage/vm-usage.png)

## <a name="manage-student-vms"></a>Gerir VMs de alunos 
Conforme passa o mouse sobre uma linha na lista de máquina virtual, verá controles para realizar as seguintes tarefas (como mostrado na imagem na secção anterior): 

- Ligar a uma VM
- Iniciar uma VM
- Parar uma VM
- Eliminar uma VM



## <a name="next-steps"></a>Passos Seguintes
Para saber mais sobre laboratórios de sala de aula, veja artigos sob [guias de procedimentos](how-to-manage-lab-accounts.md).
