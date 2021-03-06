---
title: Como atribuir funções de diretório para os utilizadores com o Azure Active Directory | Documentos da Microsoft
description: Saiba como atribuir funções de diretório para os utilizadores com o Azure Active Directory.
services: active-directory
author: eross-msft
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.component: fundamentals
ms.topic: conceptual
ms.date: 09/06/2018
ms.author: lizross
ms.reviewer: jeffsta
ms.openlocfilehash: b73df5ec0381e83c54c8cd9f8c0335448def0c6d
ms.sourcegitcommit: 1b561b77aa080416b094b6f41fce5b6a4721e7d5
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/17/2018
ms.locfileid: "45733047"
---
# <a name="how-to-assign-roles-and-administrators-to-users-with-azure-active-directory"></a>Como: atribuir funções e os administradores a utilizadores com o Azure Active Directory
Se um utilizador na sua organização necessita da permissão para gerir os recursos do Azure Active Directory (Azure AD), tem de atribuir ao utilizador uma função adequada no Azure AD, com base nas ações que o utilizador tem permissão para executar.

Para obter mais informações sobre as funções disponíveis, consulte [atribuir funções de administrador no Azure Active Directory](../users-groups-roles/directory-assign-admin-roles.md). Para obter mais informações sobre como adicionar utilizadores, consulte [adicionar novos utilizadores ao Azure Active Directory](add-users-azure-active-directory.md).

## <a name="assign-roles"></a>Atribuir funções
Uma maneira comum de atribuir funções do Azure AD a um utilizador é sobre o **função de diretório** para um utilizador.

Também pode atribuir funções usando o Privileged Identity Management (PIM). Para obter mais informações sobre como utilizar o PIM, consulte [Privileged Identity Management](https://docs.microsoft.com/azure/active-directory/privileged-identity-management).

### <a name="to-assign-a-role-to-a-user"></a>Para atribuir uma função a um utilizador
1. Inicie sessão para o [portal do Azure](https://portal.azure.com/) com uma conta de Administrador Global do diretório.

2. Selecione **do Azure Active Directory**, selecione **utilizadores**e, em seguida, procure e selecione o utilizador a obter a atribuição de função. Por exemplo, _Alain Charon_.

3. Sobre o **Alain Charon - perfil** página, selecione **função de diretório**.

    O **Alain Charon - função de diretório** é apresentada a página.

4. Selecione **Adicionar função**, selecione a função para atribuir a Alain (por exemplo, _administrador da aplicação_) e, em seguida, escolha **selecionar**.

    ![Página de funções de diretório, que mostra a função selecionada](media/active-directory-users-assign-role-azure-portal/directory-role-select-role.png)

    A função de administrador da aplicação é atribuída a Alain Charon e é apresentado no **Alain Charon - função de diretório** página.

## <a name="remove-a-role-assignment"></a>Remover uma atribuição de função
Se precisar de remover a atribuição de função de um utilizador, também pode fazer a partir da **Alain Charon - função de diretório** página.

### <a name="to-remove-a-role-assignment-from-a-user"></a>Para remover uma atribuição de função de um utilizador

1. Selecione **do Azure Active Directory**, selecione **utilizadores**e, em seguida, procure e selecione o utilizador a obter a atribuição de função removida. Por exemplo, _Alain Charon_.

2. Selecione **função de diretório**, selecione **administrador da aplicação**e, em seguida, selecione **remover função**.

    ![Página de funções de diretório, que mostra a função selecionada e a opção Remover](media/active-directory-users-assign-role-azure-portal/directory-role-remove-role.png)

    A função de administrador do aplicativo é removida da Alain Charon e deixa de aparecer no **Alain Charon - função de diretório** página.

## <a name="next-steps"></a>Passos Seguintes
- [Adicionar ou eliminar utilizadores](add-users-azure-active-directory.md)

- [Adicionar ou alterar as informações de perfil](active-directory-users-profile-azure-portal.md)

- [Adicionar utilizadores convidados a partir de outro diretório](../b2b/what-is-b2b.md)

Ou pode realizar outras tarefas de gestão de utilizador, como a atribuição de delegados, através de políticas e partilhar contas de utilizador. Para obter mais informações sobre outras ações disponíveis, consulte [documentação de gestão de utilizador do Azure Active Directory](../users-groups-roles/index.yml).


