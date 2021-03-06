---
title: Utilizar o controlo de acesso baseado em funções para StorSimple | Microsoft Docs
description: Descreve como utilizar o controlo de acesso baseado em funções do Azure (RBAC) no contexto do StorSimple.
services: storsimple
documentationcenter: ''
author: alkohli
manager: jconnoc
editor: ''
ms.assetid: ''
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/11/2017
ms.author: alkohli
ms.openlocfilehash: c500725508d2bf9f09279e665871ab286d9e495a
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/01/2018
ms.locfileid: "34652074"
---
# <a name="role-based-access-control-for-storsimple"></a>Controlo de acesso baseado em funções para StorSimple

Este artigo fornece uma breve descrição de como o controlo de acesso em funções do Azure (RBAC) podem ser utilizadas para o dispositivo StorSimple. RBAC oferece gestão de acesso detalhada para o Azure. Utilize o RBAC para conceder apenas a quantidade certa de acesso aos utilizadores efetuar as tarefas em vez de fornecer todas as pessoas StorSimple acesso sem restrições. Para obter mais informações sobre as noções básicas de gestão de acesso no Azure, consulte [introdução ao controlo de acesso baseado em funções no portal do Azure](../role-based-access-control/overview.md).

Este artigo aplica-se a dispositivos de série 8000 do StorSimple com o Update 3.0 ou posterior no portal do Azure.

## <a name="rbac-roles-for-storsimple"></a>Funções do RBAC para StorSimple

RBAC pode ser atribuído com base nas funções de. As funções Certifique-se determinados níveis de permissão com base nos recursos disponíveis no ambiente. Existem dois tipos de funções de que os utilizadores do StorSimple, podem escolher de entre: incorporado ou personalizado.

* **Funções incorporadas** -as funções incorporadas podem ser o proprietário, Contribuidor, leitor ou o administrador de acesso de utilizador. Para obter mais informações, consulte [funções incorporadas para controlo de acesso baseado em funções do Azure](../role-based-access-control/built-in-roles.md).

* **Funções personalizadas** -se as funções incorporadas não conforme as suas necessidades, pode criar funções RBAC personalizadas para StorSimple. Para criar uma função personalizada do RBAC, começar com uma função incorporada, editá-lo e, em seguida, importá-lo novamente no ambiente. A transferência e o carregamento da função são geridos através do Azure PowerShell ou a CLI do Azure. Para obter mais informações, consulte [criar funções personalizadas para controlo de acesso baseado em funções](../role-based-access-control/custom-roles.md).

Para ver as diferentes funções disponíveis para um utilizador do dispositivo StorSimple no portal do Azure, aceda ao seu serviço do Gestor de dispositivos do StorSimple e, em seguida, aceda a **(IAM) do controlo de acesso > funções**.


## <a name="create-a-custom-role-for-storsimple-infrastructure-administrator"></a>Criar uma função personalizada para o administrador de infraestrutura do StorSimple

No exemplo seguinte, podemos começar a utilizar a função incorporada **leitor** que permite aos utilizadores para ver todos os âmbitos de recursos, mas não editá-los ou criar novos. Iremos, em seguida, expandir esta função para criar uma nova função personalizada administrador de infraestrutura do StorSimple. Esta função é atribuída aos utilizadores que podem gerir a infraestrutura para os dispositivos StorSimple.

1. Execute o Windows PowerShell como administrador.

2. Iniciar sessão no Azure.

    `Connect-AzureRmAccount`

3. Exporte a função de leitor como um modelo JSON no seu computador.

    ```
    Get-AzureRMRoleDefinition -Name "Reader"

    Get-AzureRMRoleDefinition -Name "Reader" | ConvertTo-Json | Out-File C:\ssrbaccustom.json
    ```

4. Abra o ficheiro JSON no Visual Studio. Verá que uma função RBAC típica consiste em três secções principais, **ações**, **NotActions**, e **AssignableScopes**.

    No **ação** secção, todas as operações permitidas para esta função estão listadas. Cada ação é atribuída a partir de um fornecedor de recursos. Para um administrador de infraestrutura do StorSimple, utilize o `Microsoft.StorSimple` fornecedor de recursos.

    Utilize o PowerShell para ver todos os recursos fornecedores disponíveis e registados na sua subscrição.

    `Get-AzureRMResourceProvider`

    Também pode procurar os cmdlets do PowerShell disponíveis gerir os fornecedores de recursos.

    No **NotActions** secções, todas as ações restritas para uma função específica do RBAC estão listadas. Neste exemplo, não são ações restritas.
    
    Sob o **AssignableScopes**, são apresentados os IDs de subscrição. Certifique-se de que a função RBAC contém o ID de subscrição explícitos em que é utilizado. Se não for especificado o ID de subscrição correta, não são permitidas para importar a função na sua subscrição.

    Edite o ficheiro mantendo em mente as considerações de anteriores.

    ```
    {
        "Name":  "StorSimple Infrastructure Admin",
        "Id":  "<guid>",
        "IsCustom":  true,
        "Description":  "Lets you view everything, but not make any changes except for Clear alerts, Clear settings, install, download etc.",
        "Actions":  [
                        "Microsoft.StorSimple/managers/alerts/read",
                        "Microsoft.StorSimple/managers/devices/volumeContainers/read",
                        "Microsoft.StorSimple/managers/devices/jobs/read",
                        "Microsoft.StorSimple/managers/devices/alertSettings/read",
                        "Microsoft.StorSimple/managers/devices/alertSettings/write",
                        "Microsoft.StorSimple/managers/clearAlerts/action",
                        "Microsoft.StorSimple/managers/devices/networkSettings/read",
                        "Microsoft.StorSimple/managers/devices/publishSupportPackage/action",
                        "Microsoft.StorSimple/managers/devices/scanForUpdates/action",
                        "Microsoft.StorSimple/managers/devices/metrics/read"

                    ],
        "NotActions":  [

                    ],
        "AssignableScopes":  [
                                "/subscriptions/<subscription_ID>/"
                            ]
    }
    ```

6. Importe a função RBAC personalizada novamente para o ambiente.

    `New-AzureRMRoleDefinition -InputFile "C:\ssrbaccustom.json"`


Esta função deverá agora aparecer na lista de funções no **controlo de acesso** painel.

![Funções do RBAC de vista](./media/storsimple-8000-role-based-access-control/rbac-role-types.png)

Para obter mais informações, aceda a [funções personalizadas](../role-based-access-control/custom-roles.md).

### <a name="sample-output-for-custom-role-creation-via-the-powershell"></a>Saída de exemplo para a criação de função personalizada através do PowerShell

```
PS C:\WINDOWS\system32> Connect-AzureRmAccount

Environment           : AzureCloud
Account               : john.doe@contoso.com
TenantId              : <tenant_ID>
SubscriptionId        : <subscription_ID>
SubscriptionName      : Internal Consumption
CurrentStorageAccount :

PS C:\WINDOWS\system32> Get-AzureRMRoleDefinition -Name "Reader"

Name             : Reader
Id               : <guid>
IsCustom         : False
Description      : Lets you view everything, but not make any changes.
Actions          : {*/read}
NotActions       : {}
AssignableScopes : {/}

PS C:\WINDOWS\system32> Get-AzureRMRoleDefinition -Name "Reader" | ConvertTo-Json | Out-File C:\ssrbaccustom.json

PS C:\WINDOWS\system32> New-AzureRMRoleDefinition -InputFile "C:\ssrbaccustom.json"

Name             : StorSimple Infrastructure Admin
Id               : <tenant_ID>
IsCustom         : True
Description      : Lets you view everything, but not make any changes except for Clear alerts, Clear settings, install,
                   download etc.
Actions          : {Microsoft.StorSimple/managers/alerts/read,
                   Microsoft.StorSimple/managers/devices/volumeContainers/read,
                   Microsoft.StorSimple/managers/devices/jobs/read,
                   Microsoft.StorSimple/managers/devices/alertSettings/read...}
NotActions       : {}
AssignableScopes : {/subscriptions/<subscription_ID>/}

PS C:\WINDOWS\system32>
```

## <a name="add-users-to-the-custom-role"></a>Adicionar utilizadores para a função personalizada

Pode conceder acesso de dentro do recurso, grupo de recursos ou subscrição, que é o âmbito da atribuição de função. Ao fornecer o acesso, tenha em consideração que o acesso concedido o nó principal é herdado pelos subordinadas. Para obter mais informações, aceda a [controlo de acesso baseado em funções](../role-based-access-control/overview.md).

1. Aceda a **(IAM) do controlo de acesso**. Clique em **+ adicionar** no painel de controlo de acesso.

    ![Adicione o acesso a função RBAC](./media/storsimple-8000-role-based-access-control/rbac-add-role.png)

2. Selecione a função que pretende atribuir, neste caso, é o **administrador de infraestrutura do StorSimple**.

3. Selecione o utilizador, grupo ou aplicação no diretório para o qual pretende dar acesso. Pode procurar o diretório com os nomes a apresentar, endereços de correio eletrónico e identificadores de objetos.

4. Selecione **guardar** para criar a atribuição.

    ![Adicionar permissões a função RBAC](./media/storsimple-8000-role-based-access-control/rbac-create-role-infra-admin.png)

Um **adicionar utilizador** notificação controla o progresso. Depois do utilizador é adicionado com êxito, a lista de utilizadores no controlo de acesso é atualizada.

## <a name="view-permissions-for-the-custom-role"></a>Ver permissões para a função personalizada

Uma vez criada esta função, pode ver as permissões associadas esta função no portal do Azure.

1. Para ver as permissões associadas esta função, vá para **(IAM) do controlo de acesso > funções > administrador de infraestrutura do StorSimple**. É apresentada a lista de utilizadores nesta função.

2. Selecione um utilizador de administrador de infraestrutura do StorSimple e clique em **permissões**.

    ![Ver permissões para a função de administrador infra-estrutura do StorSimple](./media/storsimple-8000-role-based-access-control/rbac-roles-view-permissions.png)

3. São apresentadas as permissões associadas esta função.

    ![Ver utilizadores da função de administrador infra-estrutura do StorSimple](./media/storsimple-8000-role-based-access-control/rbac-infra-admin-permissions1.png)


## <a name="next-steps"></a>Passos Seguintes

Saiba como [atribuir funções personalizadas para os utilizadores internos e externos](../role-based-access-control/role-assignments-external-users.md).

