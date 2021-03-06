---
title: Anexar um disco de dados a uma VM do Windows no Azure com o PowerShell | Documentos da Microsoft
description: Como anexar um disco de dados nova ou existente a uma VM do Windows com o PowerShell com o modelo de implementação do Resource Manager.
services: virtual-machines-windows
documentationcenter: ''
author: cynthn
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 10/16/2018
ms.author: cynthn
ms.openlocfilehash: cd11bb8ae8f22705feb7eebeafde385fcf11fdcd
ms.sourcegitcommit: 17633e545a3d03018d3a218ae6a3e4338a92450d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/22/2018
ms.locfileid: "49637090"
---
# <a name="attach-a-data-disk-to-a-windows-vm-with-powershell"></a>Anexar um disco de dados a uma VM do Windows com o PowerShell

Este artigo mostra-lhe como anexar discos de novos e existentes para uma máquina virtual do Windows com o PowerShell. 

Em primeiro lugar, reveja estas dicas:
* O tamanho da máquina virtual controla quantos discos de dados, pode anexar. Para obter mais informações, consulte [tamanhos de máquinas virtuais](sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
* Para utilizar o armazenamento Premium, terá de um tipo VM com capacidade de armazenamento Premium como a série DS ou a máquina virtual de série GS. Para obter mais informações, consulte [o armazenamento Premium: armazenamento de elevado desempenho para cargas de trabalho de Máquina Virtual de Azure](premium-storage.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

[!INCLUDE [cloud-shell-powershell.md](../../../includes/cloud-shell-powershell.md)]

Para instalar e utilizar o PowerShell localmente, este tutorial requer o Azure PowerShell versão do módulo 6.0.0 ou posterior. Executar ` Get-Module -ListAvailable AzureRM` para localizar a versão. Se precisar de atualizar, veja [Install Azure PowerShell module (Instalar o módulo do Azure PowerShell)](/powershell/azure/install-azurerm-ps). Se estiver executando o PowerShell localmente, terá também de executar `Connect-AzureRmAccount` para criar uma ligação com o Azure.


## <a name="add-an-empty-data-disk-to-a-virtual-machine"></a>Adicionar um disco de dados vazia para uma máquina virtual

Este exemplo mostra como adicionar um disco de dados vazia para uma máquina virtual existente.

### <a name="using-managed-disks"></a>Utilizar discos geridos

```azurepowershell-interactive
$rgName = 'myResourceGroup'
$vmName = 'myVM'
$location = 'East US' 
$storageType = 'Premium_LRS'
$dataDiskName = $vmName + '_datadisk1'

$diskConfig = New-AzureRmDiskConfig -SkuName $storageType -Location $location -CreateOption Empty -DiskSizeGB 128
$dataDisk1 = New-AzureRmDisk -DiskName $dataDiskName -Disk $diskConfig -ResourceGroupName $rgName

$vm = Get-AzureRmVM -Name $vmName -ResourceGroupName $rgName 
$vm = Add-AzureRmVMDataDisk -VM $vm -Name $dataDiskName -CreateOption Attach -ManagedDiskId $dataDisk1.Id -Lun 1

Update-AzureRmVM -VM $vm -ResourceGroupName $rgName
```

### <a name="using-managed-disks-in-an-availability-zone"></a>Utilizar discos geridos numa zona de disponibilidade
Para criar um disco numa zona de disponibilidade, utilize [New-AzureRmDiskConfig](/powershell/module/azurerm.compute/new-azurermdiskconfig) com o `-Zone` parâmetro. O exemplo seguinte cria um disco na zona *1*.


```powershell
$rgName = 'myResourceGroup'
$vmName = 'myVM'
$location = 'East US 2' 
$storageType = 'Premium_LRS'
$dataDiskName = $vmName + '_datadisk1'

$diskConfig = New-AzureRmDiskConfig -SkuName $storageType -Location $location -CreateOption Empty -DiskSizeGB 128 -Zone 1
$dataDisk1 = New-AzureRmDisk -DiskName $dataDiskName -Disk $diskConfig -ResourceGroupName $rgName

$vm = Get-AzureRmVM -Name $vmName -ResourceGroupName $rgName 
$vm = Add-AzureRmVMDataDisk -VM $vm -Name $dataDiskName -CreateOption Attach -ManagedDiskId $dataDisk1.Id -Lun 1

Update-AzureRmVM -VM $vm -ResourceGroupName $rgName
```


### <a name="initialize-the-disk"></a>Inicialize o disco

Depois de adicionar um disco vazio, precisará de inicializá-la. Para inicializar o disco, pode iniciar sessão a uma VM e utilizar a gestão de discos. Se ativou [WinRM](https://docs.microsoft.com/windows/desktop/WinRM/portal) e um certificado na VM quando o criou, pode utilizar o PowerShell remoto para inicializar o disco. Também pode utilizar uma extensão de script personalizado: 

```azurepowershell-interactive
    $location = "location-name"
    $scriptName = "script-name"
    $fileName = "script-file-name"
    Set-AzureRmVMCustomScriptExtension -ResourceGroupName $rgName -Location $locName -VMName $vmName -Name $scriptName -TypeHandlerVersion "1.4" -StorageAccountName "mystore1" -StorageAccountKey "primary-key" -FileName $fileName -ContainerName "scripts"
```
        
O ficheiro de script pode conter código para inicializar os discos, por exemplo:

```azurepowershell-interactive
    $disks = Get-Disk | Where partitionstyle -eq 'raw' | sort number

    $letters = 70..89 | ForEach-Object { [char]$_ }
    $count = 0
    $labels = "data1","data2"

    foreach ($disk in $disks) {
        $driveLetter = $letters[$count].ToString()
        $disk | 
        Initialize-Disk -PartitionStyle MBR -PassThru |
        New-Partition -UseMaximumSize -DriveLetter $driveLetter |
        Format-Volume -FileSystem NTFS -NewFileSystemLabel $labels[$count] -Confirm:$false -Force
    $count++
    }
```


## <a name="attach-an-existing-data-disk-to-a-vm"></a>Anexar um disco de dados existente a uma VM

Pode anexar um disco gerido existente a uma VM como um disco de dados. 

```azurepowershell-interactive
$rgName = "myResourceGroup"
$vmName = "myVM"
$location = "East US" 
$dataDiskName = "myDisk"
$disk = Get-AzureRmDisk -ResourceGroupName $rgName -DiskName $dataDiskName 

$vm = Get-AzureRmVM -Name $vmName -ResourceGroupName $rgName 

$vm = Add-AzureRmVMDataDisk -CreateOption Attach -Lun 0 -VM $vm -ManagedDiskId $disk.Id

Update-AzureRmVM -VM $vm -ResourceGroupName $rgName
```

## <a name="next-steps"></a>Passos Seguintes

Criar uma [instantâneo](snapshot-copy-managed-disk.md).
