---
title: Restaurar a chave do Key Vault e o segredo para VMs encriptadas utilizando o Azure Backup
description: Saiba como restaurar a chave do Key Vault e o segredo na cópia de segurança do Azure com o PowerShell
services: backup
author: sogup
manager: vijayts
ms.service: backup
ms.topic: conceptual
ms.date: 08/28/2017
ms.author: sogup
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 6ac3c3d8f2a5ae37f1d32f9781f0cdbec0b293e8
ms.sourcegitcommit: 17fe5fe119bdd82e011f8235283e599931fa671a
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/11/2018
ms.locfileid: "42057304"
---
# <a name="restore-key-vault-key-and-secret-for-encrypted-vms-using-azure-backup"></a>Restaurar a chave do Key Vault e o segredo para VMs encriptadas utilizando o Azure Backup
Este artigo fala sobre como utilizar o Azure VM Backup para efetuar o restauro de VMs encriptadas do Azure, se a sua chave e segredo não existem no Cofre de chaves. Estes passos também podem ser usados se quiser manter uma cópia separada de chave (chave de encriptação de chave) e o segredo (chave de encriptação do BitLocker) para a VM restaurada.

## <a name="prerequisites"></a>Pré-requisitos
* **Cópia de segurança de VMs encriptadas** - encriptados VMs do Azure com backup feitas utilizando o Azure Backup. Consulte o artigo [gerir cópia de segurança e restauro de VMs do Azure com o PowerShell](backup-azure-vms-automation.md) para obter detalhes sobre como a cópia de segurança de VMs encriptadas do Azure.
* **Configurar o Azure Key Vault** – Certifique-se de que esse cofre de chaves para que as chaves e segredos tem de ser restaurada já se encontra presente. Consulte o artigo [introdução ao Azure Key Vault](../key-vault/key-vault-get-started.md) para obter detalhes sobre a gestão do Cofre de chaves.
* **Restaurar disco** -Certifique-se de que acionou a tarefa de restauro para restaurar discos para a utilização de VM encriptada [passos de PowerShell](backup-azure-vms-automation.md#restore-an-azure-vm). Isso é porque esta tarefa gera um ficheiro JSON na sua conta de armazenamento que contém as chaves e segredos para a VM encriptada para serem restaurados.

## <a name="get-key-and-secret-from-azure-backup"></a>Obter a chave e o segredo de cópia de segurança do Azure

> [!NOTE]
> Assim que o disco foi restaurado para a VM encriptada, certifique-se de que:
> 1. $details é preenchido com os detalhes de tarefa de discos de restauro, tal como mencionado em [PowerShell etapas na restauração da secção de discos](backup-azure-vms-automation.md#restore-an-azure-vm)
> 2. VM deve ser criada a partir de discos restaurados apenas **após o restauro chave e o segredo ao Cofre de chaves**.
>
>

Consulte as propriedades de disco restaurado para os detalhes da tarefa.

```
PS C:\> $properties = $details.properties
PS C:\> $storageAccountName = $properties["Target Storage Account Name"]
PS C:\> $containerName = $properties["Config Blob Container Name"]
PS C:\> $encryptedBlobName = $properties["Encryption Info Blob Name"]
```

Definir o contexto de armazenamento do Azure e restaurar o ficheiro de configuração JSON que contém a chave e detalhes secretas para a VM encriptada.

```
PS C:\> Set-AzureRmCurrentStorageAccount -Name $storageaccountname -ResourceGroupName '<rg-name>'
PS C:\> $destination_path = 'C:\vmencryption_config.json'
PS C:\> Get-AzureStorageBlobContent -Blob $encryptedBlobName -Container $containerName -Destination $destination_path
PS C:\> $encryptionObject = Get-Content -Path $destination_path  | ConvertFrom-Json
```

## <a name="restore-key"></a>Restaurar chave
Depois do ficheiro JSON é gerado no caminho de destino mencionado acima, gerar o arquivo de blob de chave a partir do JSON e inseri-la para restaurar a chave cmdlet para colocar a chave (de chaves KEK) novamente no Cofre de chaves.

```
PS C:\> $keyDestination = 'C:\keyDetails.blob'
PS C:\> [io.file]::WriteAllBytes($keyDestination, [System.Convert]::FromBase64String($encryptionObject.OsDiskKeyAndSecretDetails.KeyBackupData))
PS C:\> Restore-AzureKeyVaultKey -VaultName '<target_key_vault_name>' -InputFile $keyDestination
```

## <a name="restore-secret"></a>Restaurar o segredo
Utilize o ficheiro JSON gerado acima para obter o nome do segredo e o valor e inseri-la para definir o cmdlet secreta colocar novamente o segredo (BEK) no Cofre de chaves. **Utilize estes cmdlets, se a sua VM for encriptada com BEK e KEK.**

```
PS C:\> $secretdata = $encryptionObject.OsDiskKeyAndSecretDetails.SecretData
PS C:\> $Secret = ConvertTo-SecureString -String $secretdata -AsPlainText -Force
PS C:\> $secretname = 'B3284AAA-DAAA-4AAA-B393-60CAA848AAAA'
PS C:\> $Tags = @{'DiskEncryptionKeyEncryptionAlgorithm' = 'RSA-OAEP';'DiskEncryptionKeyFileName' = 'B3284AAA-DAAA-4AAA-B393-60CAA848AAAA.BEK';'DiskEncryptionKeyEncryptionKeyURL' = $encryptionObject.OsDiskKeyAndSecretDetails.KeyUrl;'MachineName' = 'vm-name'}
PS C:\> Set-AzureKeyVaultSecret -VaultName '<target_key_vault_name>' -Name $secretname -SecretValue $Secret -ContentType  'Wrapped BEK' -Tags $Tags
```

Se a sua VM está **encriptados com a BEK apenas**e gerar o ficheiro blob secreta partir do JSON e inseri-la para restaurar o cmdlet secreta para colocar o segredo (BEK) novamente no Cofre de chaves.

```
PS C:\> $secretDestination = 'C:\secret.blob'
PS C:\> [io.file]::WriteAllBytes($secretDestination, [System.Convert]::FromBase64String($encryptionObject.OsDiskKeyAndSecretDetails.KeyVaultSecretBackupData))
PS C:\> Restore-AzureKeyVaultSecret -VaultName '<target_key_vault_name>' -InputFile $secretDestination -Verbose
```

> [!NOTE]
> 1. Valor $secretname pode ser obtido ao referir-se à saída do $encryptionObject.OsDiskKeyAndSecretDetails.SecretUrl e utilizar o texto depois de segredos / por exemplo, o URL de segredo de saída é https://keyvaultname.vault.azure.net/secrets/B3284AAA-DAAA-4AAA-B393-60CAA848AAAA/xx000000xx0849999f3xx30000003163 e nome do segredo é B3284AAA-DAAA-4AAA-B393-60CAA848AAAA
> 2. Valor da etiqueta DiskEncryptionKeyFileName é igual ao nome do segredo.
>
>

## <a name="create-virtual-machine-from-restored-disk"></a>Criar máquina virtual a partir de disco restaurado
Se tiver uma cópia de segurança VM encriptada utilizando o Azure VM Backup, os cmdlets do PowerShell mencionado acima ajudam a que restaurar a chave e segredo back para o Cofre de chaves. Depois de restaurá-las, consulte o artigo [gerir cópia de segurança e restauro de VMs do Azure com o PowerShell](backup-azure-vms-automation.md#create-a-vm-from-restored-disks) para criar VMs encriptadas a partir de disco restaurado, a chave e segredo.

## <a name="legacy-approach"></a>Abordagem de legado
A abordagem mencionada acima funciona para todos os pontos de recuperação. No entanto, a abordagem mais antiga de obter a chave e informações secretas do ponto de recuperação, seria válido para pontos de recuperação com mais de 11 de Julho de 2017 para VMs encriptadas com BEK e KEK. Depois de concluída para a VM encriptada com a tarefa de restauro do disco [PowerShell passos](backup-azure-vms-automation.md#restore-an-azure-vm), certifique-se de que $rp é preenchido com um valor válido.

### <a name="restore-key"></a>Restaurar chave
Utilize os seguintes cmdlets para obter informações de chaves (KEK) do ponto de recuperação e inseri-la para restaurar a chave cmdlet colocá-lo novamente no Cofre de chaves.

```
PS C:\> $rp1 = Get-AzureRmRecoveryServicesBackupRecoveryPoint -RecoveryPointId $rp[0].RecoveryPointId -Item $backupItem -KeyFileDownloadLocation 'C:\Users\downloads'
PS C:\> Restore-AzureKeyVaultKey -VaultName '<target_key_vault_name>' -InputFile 'C:\Users\downloads'
```

### <a name="restore-secret"></a>Restaurar o segredo
Utilize os seguintes cmdlets para obter informações secretas de (BEK) do ponto de recuperação e inseri-la para definir o cmdlet secreta colocá-lo novamente no Cofre de chaves.

```
PS C:\> $secretname = 'B3284AAA-DAAA-4AAA-B393-60CAA848AAAA'
PS C:\> $secretdata = $rp1.KeyAndSecretDetails.SecretData
PS C:\> $Secret = ConvertTo-SecureString -String $secretdata -AsPlainText -Force
PS C:\> $Tags = @{'DiskEncryptionKeyEncryptionAlgorithm' = 'RSA-OAEP';'DiskEncryptionKeyFileName' = 'B3284AAA-DAAA-4AAA-B393-60CAA848AAAA.BEK';'DiskEncryptionKeyEncryptionKeyURL' = 'https://mykeyvault.vault.azure.net:443/keys/KeyName/84daaac999949999030bf99aaa5a9f9';'MachineName' = 'vm-name'}
PS C:\> Set-AzureKeyVaultSecret -VaultName '<target_key_vault_name>' -Name $secretname -SecretValue $secret -Tags $Tags -SecretValue $Secret -ContentType  'Wrapped BEK'
```

> [!NOTE]
> 1. Valor para $secretname pode ser obtido ao referir-se à saída do US $rp1. KeyAndSecretDetails.SecretUrl e com texto após segredos / por exemplo, o segredo de saída é URL https://keyvaultname.vault.azure.net/secrets/B3284AAA-DAAA-4AAA-B393-60CAA848AAAA/xx000000xx0849999f3xx30000003163 e nome do segredo é B3284AAA-DAAA-4AAA-B393-60CAA848AAAA
> 2. Valor da etiqueta DiskEncryptionKeyFileName é igual ao nome do segredo.
> 3. Valor para DiskEncryptionKeyEncryptionKeyURL pode ser obtido a partir do Cofre de chaves depois de restaurar as chaves de volta e usando [Get-AzureKeyVaultKey](https://docs.microsoft.com/powershell/module/azurerm.keyvault/get-azurekeyvaultkey) cmdlet
>
>

## <a name="next-steps"></a>Passos Seguintes
Depois de restaurar a chave e segredo back para o Cofre de chaves, consulte o artigo [gerir cópia de segurança e restauro de VMs do Azure com o PowerShell](backup-azure-vms-automation.md#create-a-vm-from-restored-disks) para criar VMs encriptadas a partir de disco restaurado, a chave e segredo.
