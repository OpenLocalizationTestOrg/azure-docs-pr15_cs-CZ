<properties
    pageTitle="Použití Powershellu ke zálohování a obnovení aplikace služeb aplikací"
    description="Zjistěte, jak pomocí Powershellu zálohování a obnovení aplikace v aplikaci služby Azure"
    services="app-service"
    documentationCenter=""
    authors="NKing92"
    manager="wpickett"
    editor="" />

<tags
    ms.service="app-service"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/10/2016"
    ms.author="nicking"/>
# <a name="use-powershell-to-back-up-and-restore-app-service-apps"></a>Použití Powershellu ke zálohování a obnovení aplikace služeb aplikací

> [AZURE.SELECTOR]
- [Prostředí PowerShell](app-service-powershell-backup.md)
- [ROZHRANÍ REST API](../app-service-web/websites-csm-backup.md)

Zjistěte, jak pomocí Powershellu Azure zálohování a obnovení [aplikace aplikaci služby](https://azure.microsoft.com/services/app-service/web/). Další informace o webové aplikace zálohy, včetně požadavků a omezení najdete v článku [obecnějším údajům web app v aplikaci služby Azure](../app-service-web/web-sites-backup.md).

## <a name="prerequisites"></a>Zjistit předpoklady pro
Použití Powershellu ke správě zálohování aplikaci, musíte:

- **Přidružení zabezpečení adresy URL** , která umožňuje pro čtení a zápis kontejneru Azure úložiště. Vysvětlení přidružení zabezpečení adres URL najdete v článku [Principy modelu přidružení zabezpečení](../storage/storage-dotnet-shared-access-signature-part-1.md) . V tématu [přes Azure s úložištěm Azure](../storage/storage-powershell-guide-full.md) příklady Správa úložiště Azure pomocí Powershellu.
- **Připojovací řetězec databáze A** Pokud chcete zálohovat databázi spolu s webovou aplikaci.

### <a name="how-to-generate-a-sas-url-to-use-with-the-web-app-backup-cmdlets"></a>Generování adresy URL přidružení zabezpečení pro práci s záložní rutiny web app
Adresa URL přidružení zabezpečení můžete vytvořený pomocí prostředí PowerShell. Tady je příklad toho, jak se vygenerovat něhož pomocí rutin popisované v tomto článku.

        $storageAccountName = "<your storage account's name>"
        $storageAccountRg = "<your storage account's resource group>"

        # This returns an array of keys for your storage account. Be sure to select the appropriate key. Here we select the first key as a default.
        $storageAccountKey = Get-AzureRmStorageAccountKey -ResourceGroupName $storageAccountRg -Name $storageAccountName
        $context = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageAccountKey[0].Value

        $blobContainerName = "<name of blob container for app backups>"
        $sasUrl = New-AzureStorageContainerSASToken -Name $blobContainerName -Permission rwdl -Context $context -ExpiryTime (Get-Date).AddMonths(1) -FullUri

## <a name="install-azure-powershell-132-or-greater"></a>Instalace modulu Azure PowerShell 1.3.2 nebo vyšší

V tématu [přes Azure pomocí Správce prostředků Azure](../powershell-install-configure.md) pokyny k instalaci a pomocí prostředí PowerShell Azure.

## <a name="create-a-backup"></a>Vytvoření zálohy

Použijte rutinu New-AzureRmWebAppBackup vytvořit záložní kopii do webových aplikací.

        $sasUrl = "<your SAS URL>"
        $resourceGroupName = "Default-Web-WestUS"
        $appName = "ContosoApp"

        $backup = New-AzureRmWebAppBackup -ResourceGroupName $resourceGroupName -Name $appName -StorageAccountUrl $sasUrl

Tím vytvoříte zálohy se automaticky generované názvem. Pokud chcete zadat název zálohy, parametr název_zálohy nepovinné.

        $backup = New-AzureRmWebAppBackup -ResourceGroupName $resourceGroupName -Name $appName -StorageAccountUrl $sasUrl -BackupName MyBackup

Zahrnout zálohování databáze, nejdřív vytvořte záložní nastavení databáze pomocí rutinu New-AzureRmWebAppDatabaseBackupSetting, a pak zadejte toto nastavení v parametru databází rutinu New-AzureRmWebAppBackup. Parametr databází přijímá maticových nastavení databáze, umožňuje obecnějším údajům víc než jednu databázi.

        $dbSetting1 = New-AzureRmWebAppDatabaseBackupSetting -Name DB1 -DatabaseType SqlAzure -ConnectionString "<connection_string>"
        $dbSetting2 = New-AzureRmWebAppDatabaseBackupSetting -Name DB2 -DatabaseType SqlAzure -ConnectionString "<connection_string>"
        $dbBackup = New-AzureRmWebAppBackup -ResourceGroupName $resourceGroupName -Name $appName -BackupName MyBackup -StorageAccountUrl $sasUrl -Databases $dbSetting1,$dbSetting2

## <a name="get-backups"></a>Získání zálohy

Rutina Get-AzureRmWebAppBackupList vrátí matici všechny zálohy pro web app. Je nutné zadat název web appu a jeho pole Skupina zdroje.

        $resourceGroupName = "Default-Web-WestUS"
        $appName = "ContosoApp"
        $backups = Get-AzureRmWebAppBackupList -Name $appName -ResourceGroupName $resourceGroupName

Konkrétní zálohu získáte pomocí rutiny Get-AzureRmWebAppBackup.

        $backup = Get-AzureRmWebAppBackup -Name $appName -ResourceGroupName $resourceGroupName -BackupId 10102

Můžete taky kanálu objekt webové aplikace do žádného z rutiny pro správu zálohování pro usnadnění.

        $app = Get-AzureRmWebApp -Name ContosoApp -ResourceGroupName Default-Web-WestUS
        $backupList = $app | Get-AzureRmWebAppBackupList
        $backup = $app | Get-AzureRmWebAppBackup -BackupId 10102

## <a name="schedule-automatic-backups"></a>Naplánování automatické zálohování

Plánujete zálohování stát automaticky v intervalu. Abyste mohli nakonfigurovat plánu zálohování, získáte pomocí rutiny AzureRmWebAppBackupConfiguration upravit. Tato rutina jen několik parametrů:

- **Název** - název web appu.
- **ResourceGroupName** – název skupiny zdroje obsahující web appu.
- **Úsek** – volitelné. Název úsek web app.
- **StorageAccountUrl** - přidružení zabezpečení adresa URL kontejneru úložišti Azure využít k ukládání zálohy.
- **FrequencyInterval** - číselnou hodnotu pro intervalu je třeba zálohy. Musí být kladné celé číslo.
- **FrequencyUnit** - výběru časového intervalu zálohy třeba úseku. Možnosti se hodinu dne.
- **RetentionPeriodInDays** - kolik dnů automatické zálohování by měly být uloženy před automaticky odstraní.
- **Čas spuštění** – volitelné. Čas, kdy má začít automatického zálohování. Zálohování začít okamžitě, pokud se jedná null. Musí být DateTime.
- **Databáze** – volitelné. Pole DatabaseBackupSettings pro databáze pro zálohování.
- **KeepAtLeastOneBackup** – volitelné přepínat parametr. Zadat Pokud jedna záloha by měl být stále v klientovi úložiště, bez ohledu na stáří je.

Je příklad toho, jak se tahle rutina slouží.

        $resourceGroupName = "Default-Web-WestUS"
        $appName = "ContosoApp"
        $slotName = "StagingSlot"
        $dbSetting1 = New-AzureRmWebAppDatabaseBackupSetting -Name DB1 -DatabaseType SqlAzure -ConnectionString "<connection_string>"
        $dbSetting2 = New-AzureRmWebAppDatabaseBackupSetting -Name DB2 -DatabaseType SqlAzure -ConnectionString "<connection_string>"
        Edit-AzureRmWebAppBackupConfiguration -Name $appName -ResourceGroupName $resourceGroupName -Slot $slotName `
          -StorageAccountUrl "<your SAS URL>" -FrequencyInterval 6 -FrequencyUnit Hour -Databases $dbSetting1,$dbSetting2 `
          -KeepAtLeastOneBackup -StartTime (Get-Date).AddHours(1)

Aktuální plán zálohování získáte pomocí rutiny Get-AzureRmWebAppBackupConfiguration. To může být užitečné při úpravách plánu, které už nakonfigurovali.

        $configuration = Get-AzureRmWebAppBackupConfiguration -Name $appName -ResourceGroupName $resourceGroupName

        # Modify the configuration slightly
        $configuration.FrequencyInterval = 2
        $configuration.FrequencyUnit = "Day"

        # Apply the new configuration by piping it into the Edit-AzureRmWebAppBackupConfiguration cmdlet
        $configuration | Edit-AzureRmWebAppBackupConfiguration

## <a name="restore-a-web-app-from-a-backup"></a>Obnovení ze zálohy do webových aplikací

Pokud chcete obnovit do webových aplikací ze zálohy, získáte pomocí rutiny obnovení AzureRmWebAppBackup. Nejjednodušší způsob, jak používat tuto rutinu má kanálu v objektu záložní načtená z kontingenčního seznamu rutiny Get-AzureRmWebAppBackup nebo rutiny Get-AzureRmWebAppBackupList.

Až budete mít záložní objektu, můžete ho do rutinu obnovení AzureRmWebAppBackup kanálu. Zadejte parametr přepsat přepnout označíte, že chcete, aby byla přepis obsahu z web appu s obsahem zálohování. Obsahuje-li zálohování databáze, jsou i obnovit tyto databáze.

        $backup | Restore-AzureRmWebAppBackup -Overwrite

Je příklad toho, jak můžete obnovit AzureRmWebAppBackup tím, že zadáte všechny parametry.

        $resourceGroupName = "Default-Web-WestUS"
        $appName = "ContosoApp"
        $slotName = "StagingSlot"
        $blobName = "ContosoBackup.zip"
        $dbSetting1 = New-AzureRmWebAppDatabaseBackupSetting -Name DB1 -DatabaseType SqlAzure -ConnectionString "<connection_string>"
        $dbSetting2 = New-AzureRmWebAppDatabaseBackupSetting -Name DB2 -DatabaseType SqlAzure -ConnectionString "<connection_string>"
        Restore-AzureRmWebAppBackup -ResourceGroupName $resourceGroupName -Name $appName -Slot $slotName -StorageAccountUrl "<your SAS URL>" -BlobName $blobName -Databases $dbSetting1,$dbSetting2 -Overwrite

## <a name="delete-a-backup"></a>Odstranění zálohy

Pokud chcete odstranit zálohy, získáte pomocí rutiny AzureRmWebAppBackup odebrat. Zálohování to odebere z vašeho účtu úložiště. Zadejte název aplikace, jeho pole Skupina zdroje a ID zálohy, kterou chcete odstranit.

        $resourceGroupName = "Default-Web-WestUS"
        $appName = "ContosoApp"
        Remove-AzureRmWebAppBackup -ResourceGroupName $resourceGroupName -Name $appName -BackupId 10102

Můžete taky kanálu záložní objektu do rutinu AzureRmWebAppBackup odebrat odstraňte ji.

        $backup = Get-AzureRmWebAppBackup -Name $appName -ResourceGroupName $resourceGroupName -BackupId 10102
        $backup | Remove-AzureRmWebAppBackup -Overwrite
