---
title: Descubra os recursos do Azure para gerir no PIM | Documentos da Microsoft
description: Saiba como detetar recursos do Azure para gerir no Azure AD Privileged Identity Management (PIM).
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
ms.service: active-directory
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.component: pim
ms.date: 08/30/2018
ms.author: rolyon
ms.openlocfilehash: d9a6ab49d619e487eee6fb13abe128cfc167b560
ms.sourcegitcommit: 1fb353cfca800e741678b200f23af6f31bd03e87
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/30/2018
ms.locfileid: "43306694"
---
# <a name="discover-azure-resources-to-manage-in-pim"></a>Descubra os recursos do Azure para gerir no PIM

Com o Azure AD Privileged Identity Management (PIM), pode melhorar a proteção dos seus recursos do Azure. Isso é útil para as organizações que já utilizam o PIM para funções de diretório do Azure AD de proteger e a proprietários da subscrição e grupo de gestão que estiver à procura para proteger os recursos de produção.

Quando configura primeiro a PIM para recursos do Azure, tem de detetar e selecione os recursos para proteger com o PIM. Não existe nenhum limite para o número de recursos que podem ser geridos com o PIM. No entanto, recomendamos começar com os seus recursos (produção) mais críticos.

## <a name="discover-resources"></a>Detetar recursos

1. Inicie sessão no [portal do Azure](https://portal.azure.com/).

1. Open **do Azure AD Privileged Identity Management**.

1. Clique em **recursos do Azure**.

    Se esta for a primeira vez que utilizar o PIM para recursos do Azure, verá um painel de recursos de descoberta.

    ![Detetar recursos - pela primeira vez](./media/pim-resource-roles-discover-resources/discover-resources-first-run.png)

    Se outro administrador de recursos ou o diretório da sua organização já está a gerir os recursos do Azure no PIM, verá uma lista dos recursos que estão atualmente a ser geridos.

    ![Detetar o painel de recursos](./media/pim-resource-roles-discover-resources/discover-resources.png)

1. Clique em **detetar recursos** para iniciar a experiência de deteção.

    ![Painel de deteção](./media/pim-resource-roles-discover-resources/discovery-pane.png)

1. No painel de deteção, utilize **filtro de estado de recurso** e **selecione o tipo de recurso** para filtrar a gestão de grupos ou subscrições que tem permissão para escrever. Ela provavelmente é mais fácil para começar **todos os** inicialmente.

    Só é possível procurar e selecione recursos de subscrição ou grupo de gestão para gerir com o PIM. Quando gerir um grupo de gestão ou de uma subscrição no PIM, também pode gerir os recursos subordinados.

1. Adicione uma marca de verificação junto a quaisquer recursos não gerenciados que pretende gerir.

    > [!NOTE]
    > Assim que uma subscrição ou grupo de gestão é definida gerida, não pode ser gerido. Isto impede o outro administrador de recursos e remover as definições do PIM.

    ![Deteção - gerir recursos](./media/pim-resource-roles-discover-resources/discovery-manage-resource.png)

1. Clique em **gerir recurso** para começar a gerir os recursos selecionados.

## <a name="next-steps"></a>Passos Seguintes

- [Configurar as definições de função de recursos do Azure no PIM](pim-resource-roles-configure-role-settings.md)
- [Atribuir funções de recursos do Azure no PIM](pim-resource-roles-assign-roles.md)
