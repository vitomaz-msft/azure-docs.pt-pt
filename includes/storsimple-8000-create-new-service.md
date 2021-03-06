---
author: alkohli
ms.service: storsimple
ms.topic: include
ms.date: 10/26/2018
ms.author: alkohli
ms.openlocfilehash: 7139b8ef2bfc9262611e51ce620da6a4769494b4
ms.sourcegitcommit: 48592dd2827c6f6f05455c56e8f600882adb80dc
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/26/2018
ms.locfileid: "50165207"
---
<!--author=alkohli last changed:02/10/2017-->

---
title: incluir ficheiro
description: incluir ficheiro
services: storage
author: alkohli
ms.service: storage
ms.topic: include
ms.date: 08/20/2018
ms.author: alkohli
ms.custom: include file
---

#### <a name="to-create-a-new-service"></a>Para criar um novo serviço

1. Utilize as credenciais da sua conta Microsoft para iniciar sessão no [portal do Azure](https://portal.azure.com/).

2. No portal do Azure, clique em **Criar um recurso** e, em seguida, no mercado, clique em **Ver todos**.

    ![Crie o Gestor de Dispositivos do StorSimple](./media/storsimple-8000-create-new-service/createssdevman1.png)

    Procure o _StorSimple Físico_. Selecione e clique em **Série do Dispositivo Físico do StorSimple** e, em seguida, clique em **Criar**. Em alternativa, no portal do Azure clique em **+** e, em seguida, em **Armazenamento**, clique em **Série do Dispositivo Físico do StorSimple**.

    ![Crie o Gestor de Dispositivos do StorSimple](./media/storsimple-8000-create-new-service/createssdevman11.png)

3. No painel **Gestor de Dispositivos do StorSimple**, siga os seguintes passos:
   
   1. Forneça um **Nome do recurso** exclusivo para o serviço. Este é um nome amigável que pode ser utilizado para identificar o serviço. O nome pode ter entre 2 e 50 carateres que podem ser letras, números e hífenes. O nome tem de começar e terminar com uma letra ou um número.

   2. Escolha uma **Subscrição** na lista pendente. A subscrição está ligada à sua conta de faturação. Este campo não estará presente se tiver apenas uma subscrição.

   3. Para **Grupo de recursos**, **Utilizar existente** ou **Criar novo**. Para obter mais informações, veja [Azure resource groups](https://azure.microsoft.com/documentation/articles/virtual-machines-windows-infrastructure-resource-groups-guidelines/) (Grupos de recursos do Azure).
   
   4. Forneça uma **Localização** para o serviço. Em geral, escolha uma localização mais próxima da região geográfica onde pretende implementar o dispositivo. Pode também querer ter em conta as seguintes considerações: 
      
      * Se tiver cargas de trabalho existentes no Azure que também tenciona implementar com o dispositivo StorSimple, deve utilizar esse datacenter.
      * O serviço Gestor de Dispositivos do StorSimple e o armazenamento do Azure podem estar em duas localizações diferentes. Se for esse o caso, terá de criar separadamente a conta do Gestor de Dispositivos do StorSimple e a conta de armazenamento do Azure. Para criar uma conta de armazenamento do Azure, vá para o serviço de Armazenamento do Azure no portal do Azure e siga os passos em [Create an Azure Storage account](../articles/storage/common/storage-quickstart-create-account.md) (Criar uma Conta de armazenamento do Azure). Uma vez criada esta conta, adicione-a ao Gestor de Dispositivos do StorSimple seguindo os passos em [Configure a new storage account for the service](../articles/storsimple/storsimple-8000-deployment-walkthrough-u2.md#configure-a-new-storage-account-for-the-service) (Configurar uma nova conta de armazenamento para o serviço).

   5. Selecione **Criar uma nova conta de armazenamento** para criar automaticamente uma conta de armazenamento com o serviço. Especifique um nome para esta conta de armazenamento. Se precisar de ter os seus dados numa localização diferente, desmarque esta caixa.

   6. Assinale **Afixar ao dashboard** se pretender uma ligação rápida para este serviço no seu dashboard.
      
   7. Clique em **Criar** para criar o Gestor de Dispositivos do StorSimple.

       ![Crie o Gestor de Dispositivos do StorSimple](./media/storsimple-8000-create-new-service/createssdevman2.png)
   
A criação do serviço demora alguns minutos. Depois de o serviço ser criado com êxito, verá uma notificação e o novo painel de serviço é aberto.
   
![Crie o Gestor de Dispositivos do StorSimple](./media/storsimple-8000-create-new-service/createssdevman5.png)


