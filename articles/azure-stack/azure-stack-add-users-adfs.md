---
title: Adicionar utilizadores ao ADFS do Azure Stack | Documentos da Microsoft
description: Saiba como adicionar utilizadores para as implementações ADFS do Azure Stack
services: azure-stack
documentationcenter: ''
author: patricka
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/15/2018
ms.author: patricka
ms.reviewer: unknown
ms.openlocfilehash: f8abacbcb05d1346931b5c2e1097660cfbd8e594
ms.sourcegitcommit: 1aacea6bf8e31128c6d489fa6e614856cf89af19
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/16/2018
ms.locfileid: "49344171"
---
# <a name="add-azure-stack-users-in-ad-fs"></a>Adicionar utilizadores do Azure Stack no AD FS
Pode utilizar o **Active Directory Users and Computers** snap-in para adicionar utilizadores adicionais a um ambiente do Azure Stack tirar partido do AD FS como respetivo fornecedor de identidade.

## <a name="add-windows-server-active-directory-users"></a>Adicionar utilizadores do Active Directory do Windows Server
> [!TIP]
> Este exemplo utiliza o padrão azurestack ASDK active directory. 

1.  Inicie sessão no computador com uma conta que fornecer acesso a ferramentas administrativas do Windows e abra uma consola de gestão do novo Microsoft (MMC).
2.  Clique em **ficheiro > Adicionar ou remover snap-in**.
3.  Selecione **utilizadores e computadores do Active Directory** > **Azurestack** > **utilizadores**.
4.  Clique em **ação** > **nova** > **utilizador**.
5.  No novo objeto – janela de utilizador, forneça e confirme uma palavra-passe
6.  Clique em **seguinte** para finalizar os valores e clique em Concluir para criar o utilizador.


## <a name="next-steps"></a>Passos Seguintes
[Criar principais de serviço](azure-stack-create-service-principals.md)