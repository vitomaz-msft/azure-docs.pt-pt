---
title: Resolução de falhas de extensão de VM do Windows | Microsoft Docs
description: Saiba mais sobre a resolução de falhas de extensão de VM do Windows Azure
services: virtual-machines-windows
documentationcenter: ''
author: kundanap
manager: jeconnoc
editor: ''
tags: top-support-issue,azure-resource-manager
ms.assetid: 878ab9b6-c3e6-40be-82d4-d77fecd5030f
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 03/29/2016
ms.author: kundanap
ms.openlocfilehash: 9973eaa7e930d38e78289219e726b5934d82ee86
ms.sourcegitcommit: d98d99567d0383bb8d7cbe2d767ec15ebf2daeb2
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/10/2018
ms.locfileid: "33945422"
---
# <a name="troubleshooting-azure-windows-vm-extension-failures"></a>Resolução de falhas de extensão de VM do Windows Azure
[!INCLUDE [virtual-machines-common-extensions-troubleshoot](../../../includes/virtual-machines-common-extensions-troubleshoot.md)]

## <a name="viewing-extension-status"></a>Visualizar o estado de extensão
Modelos Azure Resource Manager podem ser executados a partir do Azure PowerShell. Assim que o modelo for executado, o estado da extensão pode ser visualizado Explorador de recursos do Azure ou as ferramentas de linha de comandos.

Segue-se um exemplo:

Azure PowerShell:

      Get-AzureRmVM -ResourceGroupName $RGName -Name $vmName -Status

Segue-se o resultado do exemplo:

      Extensions:  {
      "ExtensionType": "Microsoft.Compute.CustomScriptExtension",
      "Name": "myCustomScriptExtension",
      "SubStatuses": [
        {
          "Code": "ComponentStatus/StdOut/succeeded",
          "DisplayStatus": "Provisioning succeeded",
          "Level": "Info",
          "Message": "    Directory: C:\\temp\\n\\n\\nMode                LastWriteTime     Length Name
              \\n----                -------------     ------ ----                              \\n-a---          9/1/2015   2:03 AM         11
              test.txt                          \\n\\n",
                      "Time": null
          },
        {
          "Code": "ComponentStatus/StdErr/succeeded",
          "DisplayStatus": "Provisioning succeeded",
          "Level": "Info",
          "Message": "",
          "Time": null
        }
    }
  ]

## <a name="troubleshooting-extension-failures"></a>Resolução de problemas de falhas de extensão
### <a name="rerun-the-extension-on-the-vm"></a>Executar novamente a extensão da VM
Se estiver a executar scripts na VM com a extensão de Script personalizado, por vezes, é possível executar um erro em que a VM foi criada com êxito mas o script falhou. Nas seguintes condições, o modo recomendado para recuperar deste erro é remover a extensão e execute novamente o modelo novamente.
Nota: no futuro, esta opção seria possível melhorada para remover a necessidade de desinstalar a extensão.

#### <a name="remove-the-extension-from-azure-powershell"></a>Remova a extensão do Azure PowerShell
    Remove-AzureRmVMExtension -ResourceGroupName $RGName -VMName $vmName -Name "myCustomScriptExtension"

Assim que a extensão foi removida, o modelo pode ser novamente executado para executar os scripts na VM.

