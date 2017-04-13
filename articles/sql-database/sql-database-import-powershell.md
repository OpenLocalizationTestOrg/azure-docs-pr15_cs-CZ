<properties
    pageTitle="Import souboru BACPAC vytvoření databáze Azure SQL pomocí prostředí PowerShell | Microsoft Azure"
    description="Import souboru BACPAC vytvoření databáze Azure SQL pomocí prostředí PowerShell"
    services="sql-database"
    documentationCenter=""
    authors="stevestein"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.topic="article"
    ms.tgt_pltfrm="powershell"
    ms.workload="data-management"
    ms.date="08/31/2016"
    ms.author="sstein"/>

# <a name="import-a-bacpac-file-to-create-an-azure-sql-database-by-using-powershell"></a>Import souboru BACPAC vytvoření databáze Azure SQL pomocí prostředí PowerShell

**Jednu databázi**

> [AZURE.SELECTOR]
- [Azure portálu](sql-database-import.md)
- [Prostředí PowerShell](sql-database-import-powershell.md)
- [SSMS](sql-database-cloud-migrate-compatible-import-bacpac-ssms.md)
- [SqlPackage](sql-database-cloud-migrate-compatible-import-bacpac-sqlpackage.md)

Tento článek obsahuje pokyny pro vytvoření databáze Azure SQL pomocí importu souboru [BACPAC](https://msdn.microsoft.com/library/ee210546.aspx#Anchor_4) pomocí prostředí PowerShell.

Databáze se vytvoří ze souboru BACPAC (.bacpac) importovaný z kontejneru objektů blob Azure úložiště. Pokud nemáte BACPAC souboru v úložišti Azure, přečtěte si článek [archivu databáze Azure SQL do souboru BACPAC pomocí prostředí PowerShell](sql-database-export-powershell.md). Pokud už máte BACPAC soubor, který není uveden v úložišti Azure, [použití AzCopy snadno nahrajte ke svému účtu Azure úložiště](../storage/storage-use-azcopy.md#blob-upload).

> [AZURE.NOTE] Databáze SQL Azure automaticky vytvoří a udržuje zálohy pro každý uživatel databáze, která lze obnovit. Podrobnosti najdete v tématu [automatické zálohy databáze SQL](sql-database-automated-backups.md).


Import databáze SQL, musíte:

- Předplatné Azure. Pokud potřebujete předplatné Azure jednoduše klikněte na **Bezplatnou zkušební verzi** v horní části této stránky a pak se vrátit k dokončení tohoto článku.
- BACPAC soubor databáze, který chcete importovat. BACPAC musí být v kontejneru [účet Azure úložiště](../storage/storage-create-storage-account.md) objektů blob.



[AZURE.INCLUDE [Start your PowerShell session](../../includes/sql-database-powershell.md)]



## <a name="set-up-the-variables-for-your-environment"></a>Nastavení proměnné ve vašem prostředí

Existuje několik proměnné potřebujete-li nahradit hodnoty příklad určitých hodnot pro databázi a vaším účtem úložiště.

Název serveru by měl být serveru, který se právě nachází v předplatného vybrané v předchozím kroku. Je třeba serveru chcete vytvořit v databázi. Import databáze přímo do fondu pružná není podporován. Ale nejdřív importovat do jedné databáze a pak posuňte databáze do fondu.

Název databáze je název, který chcete použít pro novou databázi.

    $ResourceGroupName = "resource group name"
    $ServerName = "server name"
    $DatabaseName = "database name"


Následující proměnné jsou z účtu úložiště, kde je uložena vaše BACPAC. V [Azure portál](https://portal.azure.com)přejděte ke svému účtu úložiště zobrazíte tyto hodnoty. Po kliknutí na **všechna nastavení** a potom **kláves** z účtu úložiště zásuvné můžete najít primární přístupové klávesy.

Název objektů blob je název existujícího BACPAC souboru, který chcete vytvořit tuto databázi. Potřebujete zahrnout koncovku .bacpac.

    $StorageName = "storageaccountname"
    $StorageKeyType = "StorageAccessKey"
    $StorageUri = "http://$StorageName.blob.core.windows.net/containerName/filename.bacpac"
    $StorageKey = "primaryaccesskey"


Spuštění [Get-pověření] (https://msdn.microsoft.com/library/azure/hh849815(v=azure.300\).aspx) rutina otevře okno s žádostí o své uživatelské jméno a heslo. Zadejte správce přihlašovací jméno a heslo databáze SQL server ($ServerName od výše uvedeného) a ne uživatelské jméno a heslo pro účet služby Azure.

    $credential = Get-Credential


## <a name="import-the-database"></a>Import databáze

Tento příkaz odešle žádost import databáze služby. V závislosti na velikosti databáze operace importu může chvíli trvat dokončete.

    $importRequest = New-AzureRmSqlDatabaseImport –ResourceGroupName $ResourceGroupName –ServerName $ServerName –DatabaseName $DatabaseName –StorageKeytype $StorageKeyType –StorageKey $StorageKey -StorageUri $StorageUri –AdministratorLogin $credential.UserName –AdministratorLoginPassword $credential.Password –Edition Standard –ServiceObjectiveName S0 -DatabaseMaxSizeBytes 50000


## <a name="monitor-the-progress-of-the-operation"></a>Sledování průběhu operace

Po spuštění [nový AzureRmSqlDatabaseImport] (https://msdn.microsoft.com/library/azure/mt707793(v=azure.300\).aspx), můžete zkontrolovat stav žádosti o spuštěním [Get-AzureRmSqlDatabaseImportExportStatus] (https://msdn.microsoft.com/library/azure/mt707794(v=azure.300\).aspx).

    Get-AzureRmSqlDatabaseImportExportStatus -OperationStatusLink $importRequest.OperationStatusLink



## <a name="sql-database-powershell-import-script"></a>Importovat skript Powershellu databáze SQL


    $ResourceGroupName = "resourceGroupName"
    $ServerName = "servername"
    $DatabaseName = "databasename"

    $StorageName = "storageaccountname"
    $StorageKeyType = "StorageAccessKey"
    $StorageUri = "http://$StorageName.blob.core.windows.net/containerName/filename.bacpac"
    $StorageKey = "primaryaccesskey"

    $credential = Get-Credential

    $importRequest = New-AzureRmSqlDatabaseImport –ResourceGroupName $ResourceGroupName –ServerName $ServerName –DatabaseName $DatabaseName –StorageKeytype $StorageKeyType –StorageKey $StorageKey -StorageUri $StorageUri –AdministratorLogin $credential.UserName –AdministratorLoginPassword $credential.Password –Edition Standard –ServiceObjectiveName S0 -DatabaseMaxSizeBytes 50000

    Get-AzureRmSqlDatabaseImportExportStatus -OperationStatusLink $importRequest.OperationStatusLink



## <a name="next-steps"></a>Další kroky

- Zjistěte, jak připojit a dotaz importovaných databáze SQL, najdete v článku [připojení k databázi SQL s SQL Server Management Studio a dělat ukázkový T-SQL dotaz](sql-database-connect-query-ssms.md)
