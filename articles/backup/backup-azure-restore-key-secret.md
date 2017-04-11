<properties
    pageTitle="Obnovení klíč trezoru spolu s tajná pro šifrované VMs zálohování Azure | Microsoft Azure"
    description="Zjistěte, jak obnovit klíč trezoru spolu s tajná v Azure zálohování pomocí prostředí PowerShell"
    services="backup"
    documentationCenter=""
    authors="JPallavi"
    manager="vijayts"
    editor=""/>

<tags
    ms.service="backup"
    ms.workload="storage-backup-recovery"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="JPallavi" />

# <a name="restore-key-vault-key-and-secret-for-encrypted-vms-using-azure-backup"></a>Obnovení klíč trezoru spolu s tajná pro šifrované VMs zálohování Azure
Tento článek pojednává o zálohování Azure OM obnovení šifrované VMs Azure, pokud vaše spolu s tajná není k dispozici v klíčové trezoru. Tento postup lze také pokud si chcete spravovat vlastní kopii key (klíč šifrovacího klíče) a tajná (šifrovací klíč BitLocker) pro obnovená OM.

## <a name="pre-requisites"></a>Předpoklady

1. **Zálohování zašifrovaných VMs** - zašifrovaných VMs Azure byla vytvořená záloha zálohování Azure. Podrobné informace o tom, jak zálohovat šifrované VMs Azure naleznete v článku [Správa zálohování a obnovení VMs Azure pomocí Powershellu](backup-azure-vms-automation.md) .

2. **Konfigurace trezoru Azure klíč** – zkontrolujte, zda základní trezoru, ke kterému klíče a tajemství muset obnovit již existuje. Podrobnosti o správy klíčů trezoru naleznete v článku [Začínáme s Azure klíč trezoru](../key-vault/key-vault-get-started.md) .

## <a name="setup-recovery-services-vault"></a>Nastavení služby trezoru obnovení 
Pomocí následujících kroků můžete Přihlaste se k prostředí PowerShell a nastavit obnovení služby trezoru kontextu

### <a name="log-in-to-azure-powershell"></a>Přihlaste se k prostředí PowerShell Azure 

Přihlaste se k účet Azure pomocí následující rutiny

```
PS C:\> Login-AzureRmAccount
```

### <a name="set-recovery-services-vault-context"></a>Nastavení obnovení služby trezoru kontextu

Po přihlášení pomocí následující rutiny získat seznam dostupných předplatných

```
PS C:\> Get-AzureRmSubscription
```

Vyberte předplatné, ve kterém jsou k dispozici zdroje

```
PS C:\> Set-AzureRmContext -SubscriptionId "<subscription-id>"
```

Nastavení kontextu trezoru pomocí služby Recovery trezoru níž byla zálohování povoleno šifrované VMs

```
PS C:\> Get-AzureRmRecoveryServicesVault -ResourceGroupName "<rg-name>" -Name "<rs-vault-name>" | Set-AzureRmRecoveryServicesVaultContext
```

### <a name="get-recovery-point"></a>Získání bod obnovení 

Vyberte kontejner v trezoru představující šifrované Azure virtuálního počítače

```
PS C:\> $namedContainer = Get-AzureRmRecoveryServicesBackupContainer -ContainerType "AzureVM" -Status "Registered" -Name "<vm-name>"
```

Pomocí tohoto kontejneru, vrátit se položky odpovídající virtuálního počítače

```
PS C:\> $backupitem = Get-AzureRmRecoveryServicesBackupItem -Container $namedContainer -WorkloadType "AzureVM"
```

Získat maticových bodů obnovení záložní vybrané položky v proměnné rp

```
PS C:\> $startDate = (Get-Date).AddDays(-7)
PS C:\> $endDate = Get-Date
PS C:\> $rp = Get-AzureRmRecoveryServicesBackupRecoveryPoint -Item $backupitem -StartDate $startdate.ToUniversalTime() -EndDate $enddate.ToUniversalTime()
```

## <a name="restore-encrypted-virtual-machine"></a>Obnovení šifrované virtuálního počítače
Obnovení šifrované OM jeho spolu s tajná pomocí následujících kroků.

### <a name="restore-key"></a>Obnovení klíče

Pole $rp výše uvedená je seřazené v obráceném pořadí času s nejnovější obnovení bodě indexu 0. Příklad: $rp [0] Vybere poslední bod obnovení.

```
PS C:\> $rp1 = Get-AzureRmRecoveryServicesBackupRecoveryPoint -RecoveryPointId $rp[0].RecoveryPointId -Item $backupItem -KeyFileDownloadLocation "C:\Users\downloads"
```

> [AZURE.NOTE]
Po úspěšném získáte tuto rutinu získá do zadané složky v počítači, na kterém je spuštěn vygeneruje objektů blob soubor. Tento soubor objektů blob představuje klíč zašifrovaných klíč v šifrované podobě.

Obnovte klíčové zadní klíčové trezoru pomocí následující rutiny. 

```
PS C:\> Restore-AzureKeyVaultKey -VaultName "contosokeyvault" -InputFile "C:\Users\downloads\key.blob"
```

### <a name="restore-secret"></a>Obnovení tajná

Obnovit skryté data z bod obnovení získané nad

```
PS C:\> $rp1.KeyAndSecretDetails.SecretUrl

https://contosokeyvault.vault.azure.net/secrets/B3284AAA-DAAA-4AAA-B393-60CAA848AAAA/20aaae9eaa99996d89d99a29990d999a
```

> [AZURE.NOTE]
Text před vault.azure.net představuje původní název klíčové trezoru. Text za tajemství / představuje tajné název. 

Pokud potřebujete tajné název-hodnota z výstupu rutinu spustit výše v případě, že chcete používat stejný název skrytou. V ostatních případech je třeba aktualizovat $secretname dole používat skrytou nový název. 

```
PS C:\> $secretname = "B3284AAA-DAAA-4AAA-B393-60CAA848AAAA"
PS C:\> $secretdata = $rp1.KeyAndSecretDetails.SecretData
PS C:\> $Secret = ConvertTo-SecureString -String $secretdata -AsPlainText -Force
```

Nastavte značky u tajné, v případě, že OM je potřeba obnovit taky. Značky DiskEncryptionKeyFileName smí obsahovat hodnoty název tajná, který budete chtít použít. 

```
PS C:\> $Tags = @{'DiskEncryptionKeyEncryptionAlgorithm' = 'RSA-OAEP';'DiskEncryptionKeyFileName' = 'B3284AAA-DAAA-4AAA-B393-60CAA848AAAA.BEK';'DiskEncryptionKeyEncryptionKeyURL' = 'https://contosokeyvault.vault.azure.net:443/keys/KeyName/84daaac999949999030bf99aaa5a9f9';'MachineName' = 'vm-name'}
```

> [AZURE.NOTE]
Hodnota pro DiskEncryptionKeyFileName je stejný příkaz jako tajné názvu, který získá nad. Hodnota parametru DiskEncryptionKeyEncryptionKeyURL získat z trezoru klíčové po obnovení klávesy zpět a pomocí rutiny [Get-AzureKeyVaultKey](https://msdn.microsoft.com/library/dn868053.aspx)   

Nastavení tajné zpět do klíčové trezoru

```
PS C:\> Set-AzureKeyVaultSecret -VaultName "contosokeyvault" -Name $secretname -SecretValue $secret -Tags $Tags -SecretValue $Secret -ContentType  "Wrapped BEK"
```

### <a name="restore-virtual-machine"></a>Obnovení virtuálního počítače
Výše uvedené rutin prostředí PowerShell vám spolu s tajné zpět do klíčové trezoru v případě obnovit zálohujete zašifrované OM zálohování OM Azure. Po jejich obnovování najdete v článku [Správa zálohování a obnovení VMs Azure pomocí prostředí PowerShell](backup-azure-vms-automation.md) obnovit zašifrované VMs.
