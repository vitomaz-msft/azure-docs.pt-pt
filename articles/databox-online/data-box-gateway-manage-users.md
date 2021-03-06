---
title: Gerir utilizadores do Azure Data Box Gateway | Microsoft Docs
description: Descreve como utilizar o portal do Azure para gerir os utilizadores do Azure Data Box Gateway.
services: databox
author: alkohli
ms.service: databox
ms.subservice: gateway
ms.topic: overview
ms.date: 10/09/2018
ms.author: alkohli
ms.openlocfilehash: 8adec2986a286e55ee71f0fddca695d1800b5317
ms.sourcegitcommit: c282021dbc3815aac9f46b6b89c7131659461e49
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/12/2018
ms.locfileid: "49167193"
---
# <a name="use-the-azure-portal-to-manage-users-on-your-azure-data-box-gateway"></a>Utilizar o portal do Azure para gerir os utilizadores no Azure Data Box Gateway 

Este artigo descreve como utilizar o portal do Azure para gerir os utilizadores no Azure Data Box Gateway. Pode gerir o Azure Data Box Gateway através do portal do Azure ou da IU da Web local. Utilize o portal do Azure para adicionar, modificar ou eliminar utilizadores.

> [!IMPORTANT]
> - O Data Box Gateway está em pré-visualização. Reveja os [Termos de serviço do Azure para pré-visualização](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) antes de encomendar e implementar esta solução.

Neste artigo, vai aprender a:

> [!div class="checklist"]
> * Adicionar um utilizador
> * Modificar utilizador
> * Eliminar um utilizador 

## <a name="about-users"></a>Sobre os utilizadores

Os utilizadores podem ser só de leitura ou ter privilégios máximos. Conforme o nome indica, os utilizadores só de leitura só podem ver os dados de partilha. Os utilizadores com privilégios máximos podem ler dados de partilha, escrever nessas partilhas e modificar ou eliminar os dados de partilha. 

 - **Utilizador com privilégios máximos** - um utilizador local com acesso total. 
 - **Utilizador só de leitura** - um utilizador local com acesso só de leitura. Estes utilizadores estão associados a partilhas que permitem operações só de leitura.

As permissões de utilizador são definidas pela primeira vez quando o utilizador é criado durante a criação da partilha. Depois de as permissões associadas a um utilizador serem criadas, podem ser modificadas com o Explorador de Ficheiros. 


## <a name="add-a-user"></a>Adicionar um utilizador

Efetue os seguintes passos no portal do Azure para adicionar um utilizador.

1. No portal do Azure, aceda ao recurso Data Box Gateway e, em seguida, navegue para **Descrição Geral**. Clique em **+ Adicionar utilizador** na barra de comandos.

    ![Clique em adicionar utilizador](media/data-box-gateway-manage-users/add-user-1.png)

2. Especifique o nome de utilizador e a palavra-passe do utilizador que pretende adicionar. Confirme a palavra-passe e clique em **Adicionar**.

    ![Clique em adicionar utilizador](media/data-box-gateway-manage-users/add-user-2.png)

    > [!IMPORTANT] 
    > Estes utilizadores estão reservados pelo sistema e não devem ser utilizados: Administrador, EdgeUser, EdgeSupport, HcsSetupUser, WDAGUtilityAccount, CLIUSR, DefaultAccount, Convidado.  

3. Receberá uma notificação quando a criação do utilizador for iniciada e concluída. Depois de o utilizador ser criado, na barra de comandos, clique em **Atualizar** para ver a lista atualizada de utilizadores.


## <a name="modify-user"></a>Modificar utilizador

Pode alterar a palavra-passe associada a um utilizador depois de o utilizador ser criado. Selecione e clique na lista de utilizadores. Indique e confirme a nova palavra-passe. Guarde as alterações.
 
![Modificar utilizador](media/data-box-gateway-manage-users/modify-user-1.png)


## <a name="delete-a-user"></a>Eliminar um utilizador

Efetue os seguintes passos no portal do Azure para eliminar um utilizador.

1. Selecione e clique num utilizador na lista de utilizadores e, em seguida, clique em **Eliminar**.  

   ![Eliminar um utilizador](media/data-box-gateway-manage-users/delete-user-1.png)

2. Quando lhe for perguntado, confirme a eliminação. 

   ![Eliminar um utilizador](media/data-box-gateway-manage-users/delete-user-2.png)

A lista de utilizadores é atualizada para refletir o utilizador eliminado.

![Eliminar um utilizador](media/data-box-gateway-manage-users/delete-user-3.png)


## <a name="next-steps"></a>Passos seguintes

- Saiba como [Gerir a largura de banda](data-box-gateway-manage-bandwidth-schedules.md).
