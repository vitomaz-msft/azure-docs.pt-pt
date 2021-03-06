---
title: Gerir o Cofre de chaves no Azure Stack através do portal | Documentos da Microsoft
description: Saiba como gerir o Cofre de chaves no Azure Stack com o portal
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.assetid: D4300668-461F-45F6-BF3B-33B502C39D17
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/12/2018
ms.author: sethm
ms.openlocfilehash: 51c04a567ff953c4e84930e3feae448f78627683
ms.sourcegitcommit: c29d7ef9065f960c3079660b139dd6a8348576ce
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/12/2018
ms.locfileid: "44713940"
---
# <a name="manage-key-vault-in-azure-stack-by-using-the-portal"></a>Gerir o Cofre de chaves no Azure Stack com o portal

Pode gerir o Cofre de chaves no Azure Stack com o portal do Azure Stack. Este artigo ajuda-o a começar a utilizar para criar e gerir um cofre de chaves no Azure Stack.

## <a name="prerequisites"></a>Pré-requisitos

Tem de subscrever uma oferta que inclui o serviço Azure Key Vault.

## <a name="create-a-key-vault"></a>Criar um cofre de chaves

1. Inicie sessão para o [portal de utilizador](https://portal.local.azurestack.external).

2. A partir do dashboard, selecione **+ criar um recurso** > **segurança + identidade** > **Key Vault**.

    ![Ecrã do Key Vault](media/azure-stack-kv-manage-portal/image1.png)

3. Na **criar Key Vault** painel, atribuir um **nome** para o cofre. Nomes de cofre podem conter apenas carateres alfanuméricos e o caráter especial de hífen (-). Eles não devem começar com um número.

4. Escolher uma **subscrição** na lista de subscrições disponíveis. Todas as subscrições que oferecem o serviço de Key Vault são apresentadas na lista pendente.

5. Selecione um **Grupo de Recursos** existente ou crie um novo.

6. Selecione o **escalão de preço**.
    >[!NOTE]
    > Chave cofres no suporte do Azure Stack Development Kit **padrão** apenas SKUs.

7. Escolha uma das existentes **políticas de acesso** ou criar um novo. Uma política de acesso permite-lhe conceder permissões de um utilizador, aplicação ou um grupo de segurança executar operações com este cofre.

8. Opcionalmente, escolha uma **política de acesso avançadas** para ativar o acesso aos recursos. Por exemplo: máquinas virtuais (VMs) para a implementação, Gestor de recursos para a implementação do modelo e o acesso ao Azure Disk Encryption para encriptação de volume.

9. Depois de configurar as definições, selecione **OK**e, em seguida, selecione **criar**. Esta ação inicia a implementação de Cofre de chaves.

## <a name="manage-keys-and-secrets"></a>Gerir chaves e segredos

Depois de criar um cofre, utilize os seguintes passos para criar e gerir chaves e segredos no cofre.

### <a name="create-a-key"></a>Criar uma chave

1. Inicie sessão para o [portal de utilizador](https://portal.local.azurestack.external).

2. A partir do dashboard, selecione **todos os recursos**, selecione o Cofre de chaves que criou anteriormente e, em seguida, selecione a **chaves** mosaico.

3. Na **chaves** painel, selecione **Add**.

4. Na **crie uma chave** painel, na lista de **opções**, escolha o método que pretende utilizar para criar uma chave. Pode **gerar** uma nova chave **carregar** existente de chaves ou utilizar **restaurar cópia de segurança** para selecionar uma cópia de segurança de uma chave.

5. Introduza um **nome** para a sua chave. O nome da chave pode conter apenas carateres alfanuméricos e o hífen (-) de caráter especial.

6. Opcionalmente, configure as **definir data de ativação** e **definir data de expiração** valores para a sua chave.

7. Selecione **criar** para iniciar a implementação.

Depois da chave for criada com êxito, pode selecionar-o para baixo **chaves** e ver ou modificar as respetivas propriedades. A secção de propriedades contém os **identificador de chave**, que é um identificador URI (Uniform Resource) que aplicativos externos utilizam para aceder esta chave. Para limitar as operações nesta chave, configure as definições sob **permitido operações**.

![Chave URI](media/azure-stack-kv-manage-portal/image4.png)

### <a name="create-a-secret"></a>Criar um segredo

1. Inicie sessão para o [portal de utilizador](https://portal.local.azurestack.external).
2. A partir do dashboard, selecione **todos os recursos**, selecione o Cofre de chaves que criou anteriormente e, em seguida, selecione a **segredos** mosaico.

3. Sob **segredos**, selecione **Add**.

4. Sob **criar um segredo**, na lista de **opções de carregamento**, escolha uma opção através do qual pretende criar um segredo. Pode criar um segredo **manualmente** se introduzir um valor para o segredo ou o carregamento de um **certificado** do seu computador local.

5. Introduza um **nome** para o segredo. O nome secreto pode conter apenas carateres alfanuméricos e o hífen (-) de caráter especial.

6. Opcionalmente, especifique a **tipo de conteúdo**e configurar os valores para **definir data de ativação** e **definir data de expiração** para o segredo.

7. Selecione **criar** para iniciar a implementação.

Após o segredo é criado com êxito, pode selecionar-o para baixo **segredos** e ver ou modificar as respetivas propriedades. O **identificador de segredo** é um URI que aplicativos externos podem utilizar para aceder Este segredo.

![Segredo do URI](media/azure-stack-kv-manage-portal/image5.png)

## <a name="next-steps"></a>Passos Seguintes

* [Implementar uma VM ao obter a palavra-passe armazenada no Cofre de chaves](azure-stack-kv-deploy-vm-with-secret.md)
* [Implementar uma VM com certificado armazenado no Cofre de chaves](azure-stack-kv-push-secret-into-vm.md)
