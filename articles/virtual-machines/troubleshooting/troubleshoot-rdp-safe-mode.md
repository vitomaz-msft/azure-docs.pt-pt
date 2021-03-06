---
title: Não é possível ligar às máquinas de virtuais do Azure remotamente uma vez que a VM arranca no modo de segurança | Documentos da Microsoft
description: Saiba como resolver um problema em que não é possível RDP para uma VM porque a VM arranca no modo seguro. | Documentos da Microsoft
services: virtual-machines-windows
documentationCenter: ''
author: genlin
manager: cshepard
editor: ''
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: troubleshooting
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 11/13/2018
ms.author: genli
ms.openlocfilehash: 8dfe61430423298eea81510d3e92d49066217a05
ms.sourcegitcommit: 275eb46107b16bfb9cf34c36cd1cfb000331fbff
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/15/2018
ms.locfileid: "51708805"
---
#  <a name="cannot-rdp-to-a-vm-because-the-vm-boots-into-safe-mode"></a>Não é possível RDP para uma VM porque a VM arranca no modo de segurança

Este artigo mostra como resolver um problema em que não é possível o ambiente de trabalho remoto para máquinas de virtuais de Windows do Azure (VMs) porque a VM está configurada para efetuar o arranque no modo seguro.

> [!NOTE] 
> O Azure tem dois modelos de implementação para criar e trabalhar com recursos: [Resource Manager e Clássico](../../azure-resource-manager/resource-manager-deployment-model.md). Este artigo explica como utilizar o modelo de implementação do Resource Manager, que recomendamos que utilize para novas implementações em vez do modelo de implementação clássica. 

## <a name="symptoms"></a>Sintomas 

Não é possível efetuar uma ligação RDP e outras ligações (por exemplo, HTTP) para uma VM no Azure porque a VM está configurada para efetuar o arranque no modo seguro. Quando dá entrada a captura de ecrã do [diagnósticos de arranque](../troubleshooting/boot-diagnostics.md) no portal do Azure, poderá ver a VM arranca normalmente, mas a interface de rede não está disponível:

![Imagem sobre inferce de rede em modo de segurança](./media/troubleshoot-rdp-safe-mode/network-safe-mode.png)

## <a name="cause"></a>Causa

O serviço RDP não está disponível no modo de segurança. Apenas serviços e programas essenciais do sistema são carregados quando a VM arranca no modo de segurança. Isto aplica-se para as duas versões diferentes do modo de segurança que são "Mínimo de arranque seguro" e "Arranque seguro com a conectividade".


## <a name="solution"></a>Solução 

Antes de seguir estes passos, tire um instantâneo do disco do SO da VM afetado como uma cópia de segurança. Para obter mais informações, consulte [instantâneo de um disco](../windows/snapshot-copy-managed-disk.md).

Para resolver este problema, utilize o controle Serial para configurar a VM para efetuar o arranque no modo normal ou [Repare a VM offline](#repair-the-vm-offline) através de uma VM de recuperação.

### <a name="use-serial-control"></a>Utilizar o controlo de série

1. Ligar à [consola de série e Abrir instância CMD](./serial-console-windows.md#open-cmd-or-powershell-in-serial-console
). Se a consola de série não estiver ativada na sua VM, consulte [Repare a VM offline](#repair-the-vm-offline).
2. Verifique os dados de configuração de arranque: 

        bcdedit /enum

    Se a VM está configurada para inicializar em modo de segurança, verá um sinalizador extra sob os **carregador de inicialização do Windows** secção denominada **inicialização segura**. Se não vir a **inicialização segura** sinalizador, a VM não está no modo de segurança. Este artigo não é aplicável ao seu cenário.

    O sinalizador de inicialização segura pode aparecer com os seguintes valores:
    - Mínimo
    - Rede

    Em qualquer um desses dois modos, RDP será não iniciado. Portanto, a correção permanece igual.

    ![Imagem sobre o sinalizador do modo de segurança](./media/troubleshoot-rdp-safe-mode/safe-mode-tag.png)

3. Eliminar a **safemoade** sinalizar, para que a VM será arrancada no modo normal:

        bcdedit /deletevalue {current} safeboot
        
4. Verifique os dados de configuração de arranque para se certificar de que o sinalizador de inicialização segura é removido:

        bcdedit /enum

5. Reinicie a VM e, em seguida, verifique se o problema está resolvido.

### <a name="repair-the-vm-offline"></a>Repare a VM offline

#### <a name="attach-the-os-disk-to-a-recovery-vm"></a>Anexar o disco do SO a uma VM de recuperação

1. [Anexar o disco do SO a uma VM de recuperação](../windows/troubleshoot-recovery-disks-portal.md).
2. Inicie uma ligação de ambiente de trabalho remoto para a VM de recuperação. 
3. Certifique-se de que o disco é sinalizado de forma **Online** no console de gerenciamento de disco. Tenha em atenção a letra de unidade que está atribuída ao disco do SO anexado.

#### <a name="enable-dump-log-and-serial-console-optional"></a>Ativar o registo de informação e a consola de série (opcional)

O registo de informação e a consola de série irão ajudar-nos fazer ainda mais de resolução de problemas se não conseguir resolver o problema a solução neste artigo.

Para ativar o registo de despejo e consola de série, execute o seguinte script.

1. Abra uma sessão de linha de comandos elevada (**executar como administrador**).
2. Execute o seguintes script:

    Nesse script, partimos do princípio de que a letra de unidade que está atribuída ao disco do SO anexado é F. Substitua esta letra de unidade com o valor apropriado para a sua VM.

    ```powershell
    reg load HKLM\BROKENSYSTEM F:\windows\system32\config\SYSTEM

    REM Enable Serial Console
    bcdedit /store F:\boot\bcd /set {bootmgr} displaybootmenu yes
    bcdedit /store F:\boot\bcd /set {bootmgr} timeout 5
    bcdedit /store F:\boot\bcd /set {bootmgr} bootems yes
    bcdedit /store F:\boot\bcd /ems {<BOOT LOADER IDENTIFIER>} ON
    bcdedit /store F:\boot\bcd /emssettings EMSPORT:1 EMSBAUDRATE:115200

    REM Suggested configuration to enable OS Dump
    REG ADD "HKLM\BROKENSYSTEM\ControlSet001\Control\CrashControl" /v CrashDumpEnabled /t REG_DWORD /d 1 /f
    REG ADD "HKLM\BROKENSYSTEM\ControlSet001\Control\CrashControl" /v DumpFile /t REG_EXPAND_SZ /d "%SystemRoot%\MEMORY.DMP" /f
    REG ADD "HKLM\BROKENSYSTEM\ControlSet001\Control\CrashControl" /v NMICrashDump /t REG_DWORD /d 1 /f

    REG ADD "HKLM\BROKENSYSTEM\ControlSet002\Control\CrashControl" /v CrashDumpEnabled /t REG_DWORD /d 1 /f
    REG ADD "HKLM\BROKENSYSTEM\ControlSet002\Control\CrashControl" /v DumpFile /t REG_EXPAND_SZ /d "%SystemRoot%\MEMORY.DMP" /f
    REG ADD "HKLM\BROKENSYSTEM\ControlSet002\Control\CrashControl" /v NMICrashDump /t REG_DWORD /d 1 /f

    reg unload HKLM\BROKENSYSTEM

#### Configure the Windows to boot into normal mode

1. Open an elevated command prompt session (**Run as administrator**).
2. Check the boot configuration data. In the following commands, we assume that the drive letter that is assigned to the attached OS disk is F. Replace this drive letter with the appropriate value for your VM. 

        bcdedit /store F:\boot\bcd /enum
    Take note of the Identifier name of the partition that has the **\windows** folder. By default, the  Identifier name is "Default".  

    If the VM is configured to boot into Safe Mode, you will see an extra flag under the **Windows Boot Loader** section called **safeboot**. If you do not see the “safeboot” flag, this article does not apply to your scenario.

    ![The image about boot Identifier](./media/troubleshoot-rdp-safe-mode/boot-id.png)

3. Remove the **safeboot** flag, so the VM will boot into normal mode:

        bcdedit /store F:\boot\bcd /deletevalue {Default} safeboot
4. Check the boot configuration data to make sure that the safeboot flag is removed:

        bcdedit /store F:\boot\bcd /enum
5. [Detach the OS disk and recreate the VM](../windows/troubleshoot-recovery-disks-portal.md). Then check whether the issue is resolved.
