---
title: Ativar as minhas funções de diretório do Azure AD no PIM | Documentos da Microsoft
description: Aprenda a ativar as funções de diretório do Azure AD no Azure AD Privileged Identity Management (PIM).
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: ''
ms.service: active-directory
ms.topic: conceptual
ms.workload: identity
ms.component: pim
ms.date: 08/27/2018
ms.author: rolyon
ms.custom: pim
ms.openlocfilehash: 59dab4651366c3ad6579e0da660baee0c653d1a3
ms.sourcegitcommit: 31241b7ef35c37749b4261644adf1f5a029b2b8e
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/04/2018
ms.locfileid: "43666007"
---
# <a name="activate-my-azure-ad-directory-roles-in-pim"></a>Ativar as minhas funções de diretório do Azure AD no PIM

O Azure Active Directory (Azure AD) Privileged Identity Management (PIM) simplifica como as empresas a gerir o acesso privilegiado a recursos no Azure AD e outros serviços online da Microsoft, como o Office 365 ou o Microsoft Intune.  

Se tiver sido efetuado elegível para uma função administrativa, o que significa que pode ativar essa função, quando precisa efetuar ações privilegiadas. Por exemplo, se ocasionalmente gerir recursos do Office 365, os administradores de função com privilégios da sua organização podem não fazer é um Administrador Global permanente, uma vez que essa função tem impacto sobre outros serviços, também. Em vez disso, eles fazem-no elegível para funções do Azure AD, como o administrador do Exchange Online. Pode pedir para ativar essa função, quando precisar de seus privilégios e, em seguida, terá controlo de administrador para um período de tempo predeterminado.

Este artigo destina-se a administradores que precisam de ativar a respetiva função de diretório do Azure AD no PIM.

## <a name="activate-a-role"></a>Ativar uma função

Quando tiver de assumir uma função de diretório do Azure AD, pode pedir a ativação ao utilizar o **minhas funções** opção de navegação no PIM.

1. Inicie sessão no [portal do Azure](https://portal.azure.com/).

1. Open **do Azure AD Privileged Identity Management**. Para obter informações sobre como adicionar o mosaico do PIM ao dashboard, consulte [começar a utilizar o PIM](pim-getting-started.md).

1. Clique em **funções de diretório do Azure AD**.

1. Clique em **minhas funções** para ver uma lista de sua elegíveis funções de diretório do Azure AD.

    ![Funções de diretório do Azure AD - as minhas funções](./media/pim-how-to-activate-role/directory-roles-my-roles.png)

1. Localize uma função que pretende ativar.

    ![Funções de diretório do Azure AD - minha lista de funções](./media/pim-how-to-activate-role/directory-roles-my-roles-activate.png)

1. Clique em **Activate** para abrir o painel de detalhes de ativação de função.

1. Se a sua função requer autenticação multifator (MFA), clique em **verificar a sua identidade antes de continuar**. Só precisa que autenticar uma vez por sessão.

    ![Certifique-se com a MFA antes da ativação de função](./media/pim-how-to-activate-role/directory-roles-my-roles-mfa.png)

1. Clique em **verificar a minha identidade** e siga as instruções para fornecer verificação adicional de segurança.

    ![Verificação adicional de segurança](./media/pim-how-to-activate-role/additional-security-verification.png)

1. Clique em **Activate** para abrir o painel de ativação.

    ![Painel de ativação](./media/pim-how-to-activate-role/directory-roles-activate.png)

1. Se necessário, especifique uma hora de início de ativação personalizado.

1. Especifique a duração de ativação.

1. Na **razão da ativação** , digite a razão para o pedido de ativação. Algumas funções exigem que forneça um número de pedido de suporte de problemas.

    ![Painel de ativação concluída](./media/pim-how-to-activate-role/directory-roles-activation-pane.png)

1. Clique em **ativar**.

    Se a função não necessita de aprovação, está agora ativado e a função é apresentada na lista de funções do Active Directory. Se o [a função requer aprovação](./azure-ad-pim-approval-workflow.md) para ativar, uma notificação será exibida no canto superior direito do seu navegador informando o pedido está com aprovação pendente.

    ![Pedido pendente de notificação](./media/pim-how-to-activate-role/directory-roles-activate-notification.png)

## <a name="view-the-status-of-your-requests"></a>Ver o estado dos pedidos

Pode ver o estado dos seus pedidos pendentes para ativar.

1. Abra o Azure AD Privileged Identity Management.

1. Clique em **funções de diretório do Azure AD**.

1. Clique em **os meus pedidos** para ver uma lista dos pedidos.

    ![Funções de diretório do Azure AD - os meus pedidos](./media/pim-how-to-activate-role/directory-roles-my-requests.png)

## <a name="use-a-role-immediately-after-activation"></a>Utilize uma função imediatamente após a ativação

Devido à colocação em cache, ativações não ocorrer imediatamente no portal do Azure sem uma atualização. Se precisar de reduzir a possibilidade de atrasos após a ativação de uma função, pode utilizar o **acesso a aplicações** página no portal. Aplicativos acessados a partir desta página verificar a existência de novas atribuições de função imediatamente.

1. Abra o Azure AD Privileged Identity Management.

1. Clique nas **acesso a aplicações** página.

    ![Acesso de aplicativo PIM](./media/pim-how-to-activate-role/pim-application-access.png)

1. Clique em **do Azure Active Directory** para reabrir o portal sobre o **todos os utilizadores** página.

    Quando clicar nesse link, forçar uma atualização, e há uma verificação para o novo Azure atribuições de funções do AD.

## <a name="deactivate-a-role"></a>Desativar uma função

Depois de uma função, automaticamente desativa quando for atingido o limite de tempo (duração elegível).

Se concluir suas tarefas de administrador no início, que também pode desativar uma função manualmente no Azure AD Privileged Identity Management.

1. Abra o Azure AD Privileged Identity Management.

1. Clique em **funções de diretório do Azure AD**.

1. Clique em **minhas funções**.

1. Clique em **funções do Active Directory** para ver uma lista de funções ativas.

1. Localizar a função já está a utilizar e, em seguida, clique em **desativar**.

## <a name="cancel-a-pending-request"></a>Cancelar um pedido pendente

Se não necessitar de ativação de uma função que requer aprovação, pode cancelar um pedido pendente em qualquer altura.

1. Abra o Azure AD Privileged Identity Management.

1. Clique em **funções de diretório do Azure AD**.

1. Clique em **os meus pedidos**.

1. Para a função que pretende cancelar, clique nas **Cancelar** botão.

    Quando clicar em Cancelar, a solicitação será cancelada. Para ativar a função mais uma vez, terá que submeter um novo pedido de ativação.

   ![Cancelar pedido pendente](./media/pim-how-to-activate-role/directory-role-cancel.png)

## <a name="next-steps"></a>Passos Seguintes

- [Ativar as minhas funções de recursos do Azure no PIM](pim-resource-roles-activate-your-roles.md)
