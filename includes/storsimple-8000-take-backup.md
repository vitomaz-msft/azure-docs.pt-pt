---
author: alkohli
ms.service: storsimple
ms.topic: include
ms.date: 10/26/2018
ms.author: alkohli
ms.openlocfilehash: 5ec3bff505d2c887269b20c5fa4d0363a3040c1a
ms.sourcegitcommit: 48592dd2827c6f6f05455c56e8f600882adb80dc
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/26/2018
ms.locfileid: "50166364"
---
<!--author=alkohli last changed: 01/12/17-->

### <a name="to-take-a-backup"></a>Para efetuar uma cópia de segurança

1. Aceda ao seu serviço Gestor de Dispositivos do StorSimple. A partir da lista tabular de dispositivos, selecione e clique no seu dispositivo e, em seguida, clique em **Todas as definições**. No painel **Definições**, aceda a **Definições > Gerir > Política de cópias de segurança**.

    ![Adicionar-política-cópia-de-segurança](./media/storsimple-8000-take-backup/step8takebu1.png)

2. No painel **Política de cópias de segurança**, clique em **+ Adicionar política**.

    ![Adicionar-política-cópia-de-segurança](./media/storsimple-8000-take-backup/step8takebu2.png)

3. No painel **Criar política de cópias de segurança**, escreva um nome que contenha entre 3 e 150 carateres para a sua política de cópia de segurança.

4. Selecione os volumes cuja cópia de segurança efetuar. Se selecionar mais do que um volume, esses volumes são agrupados para criar uma cópia de segurança consistente de falhas.

    ![Adicionar-política-cópia-de-segurança](./media/storsimple-8000-take-backup/step8takebu4.png)

5. No painel **Adicionar primeira agenda**:

    1. Selecione o tipo de cópia de segurança. Para restauros mais rápidos, selecione instantâneo **Local**. Para resiliência de dados, selecione instantâneo da **Cloud**.
    2. Especifique a frequência da cópia de segurança em minutos, horas, dias ou semanas.
    3. Selecione um tempo de retenção. As opções de período de retenção dependem da frequência da cópia de segurança. Por exemplo, para uma política diária, o período de retenção pode ser especificado em semanas, ao passo que o período de retenção para uma política mensal é em meses.
    4. Selecione a hora e data de início para a política de cópia de segurança.
    5. Clique em **OK** para criar a política de cópias de segurança.

        ![Adicionar-política-cópia-de-segurança](./media/storsimple-8000-take-backup/step8takebu5.png) 

6. Clique em **Criar** para iniciar a criação da política de cópias de segurança. Será notificado quando a política de cópias de segurança for criada com êxito. A lista de políticas de cópias de segurança também é atualizada.
      
      ![Adicionar-política-cópia-de-segurança](./media/storsimple-8000-take-backup/step8takebu9.png)
      
      Tem agora uma política de cópias de segurança que cria as cópias de segurança agendadas dos dados do seu volume.




