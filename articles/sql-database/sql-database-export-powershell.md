<properties
    pageTitle="Archivace databáze Azure SQL do souboru BACPAC pomocí prostředí PowerShell"
    description="Archivace databáze Azure SQL do souboru BACPAC pomocí prostředí PowerShell"
    services="sql-database"
    documentationCenter=""
    authors="stevestein"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.date="08/15/2016"
    ms.author="sstein"
    ms.workload="data-management"
    ms.topic="article"
    ms.tgt_pltfrm="NA"/>


# <a name="archive-an-azure-sql-database-to-a-bacpac-file-by-using-powershell"></a>Archivace databáze Azure SQL do souboru BACPAC pomocí prostředí PowerShell

> [AZURE.SELECTOR]
- [Azure portálu](sql-database-export.md)
- [Prostředí PowerShell](sql-database-export-powershell.md)


Tento článek obsahuje pokyny pro archivaci databáze Azure SQL do souboru [BACPAC](https://msdn.microsoft.com/library/ee210546.aspx#Anchor_4) (uložené v úložišti objektů Blob Azure) pomocí Powershellu.

Když je potřeba vytvořit archivu databáze Azure SQL, můžete exportovat do souboru BACPAC schéma databáze a data. Soubor BACPAC je jednoduše ZIP soubor s příponou .bacpac. Soubor BACPAC lze uložit později v úložišti objektů Blob Azure nebo místní úložiště na místního pracoviště. Ji můžete taky importovat zpět do databáze SQL Azure nebo do instalace SQL serveru místní.

**Co byste měli zvážit**

- Archivace využití transakce konzistentní, musíte zajistit, aby bez zápisu aktivity dochází při exportu nebo který exportujete ze [využití transakce konzistentní kopii](sql-database-copy.md) databáze Azure SQL.
- Maximální velikost souboru BACPAC archivace k úložišti objektů Blob Azure je 200 GB. Archivace větší soubor BACPAC k místním úložišti, nástroj [SqlPackage](https://msdn.microsoft.com/library/hh550080.aspx) příkazový řádek. Tento nástroj se dodává s Visual Studia a SQL Server. Také je možné [Stáhnout](https://msdn.microsoft.com/library/mt204009.aspx) nejnovější verzi SQL Server Data Tools získat tento nástroj.
- Archivace k základnímu úložišti Azure premium pomocí souboru BACPAC není podporovaná.
- Pokud operace exportu překročí 20 hodin, může ji zrušit. Pokud chcete zvýšit výkon při exportu, máte tyto možnosti:
 - Dočasně zvýšit úroveň služby.
 - Ukončení všechny čtení a zápis aktivity při exportu.
 - Použití [seskupený index](https://msdn.microsoft.com/library/ms190457.aspx) s jinou hodnotu než null na všechny rozsáhlých tabulek. Bez skupinový indexy export nemusí podařit, pokud trvá déle než 6-12 hodin. Je to proto službu exportovat je potřeba dokončit prohledávání tabulky při exportu celou tabulku. Spolehlivých způsobů, jak zjistit, pokud jsou tabulkách optimalizované pro export je spustit **DBCC SHOW_STATISTICS** a ověřte, že *RANGE_HI_KEY* není null jeho hodnotu má dobré rozdělení. Podrobnosti najdete v tématu [DBCC SHOW_STATISTICS](https://msdn.microsoft.com/library/ms174384.aspx).

> [AZURE.NOTE] BACPACs nejsou určeny se dá použít pro zálohování a obnovení. Databáze SQL Azure automaticky vytvoří zálohy pro každý uživatel databáze. Podrobnosti najdete v tématu [automatické zálohy databáze SQL](sql-database-automated-backups.md).

Tento článek, je potřeba k provedení těchto věcí:

- Předplatné Azure.
- Databáze Azure SQL.
- [Účet Azure standardní úložiště](../storage/storage-create-storage-account.md), s kontejnerem objektů blob ukládat BACPAC v standardní úložiště.


[AZURE.INCLUDE [Start your PowerShell session](../../includes/sql-database-powershell.md)]




## <a name="export-your-database"></a>Export databáze

[New-AzureRmSqlDatabaseExport] (https://msdn.microsoft.com/library/azure/mt707796(v=azure.300\).aspx) rutina odešle žádost export databáze služby. V závislosti na velikosti databáze operace exportu může chvíli trvat dokončete.

> [AZURE.IMPORTANT] Chcete-li zajistit využití transakce konzistentní BACPAC soubor, by měl první [vytvořit kopii databáze](sql-database-copy-powershell.md)a potom soubor vyexportujte kopii databáze.


     $exportRequest = New-AzureRmSqlDatabaseExport –ResourceGroupName $ResourceGroupName –ServerName $ServerName `
       –DatabaseName $DatabaseName –StorageKeytype $StorageKeytype –StorageKey $StorageKey -StorageUri $BacpacUri `
       –AdministratorLogin $creds.UserName –AdministratorLoginPassword $creds.Password


## <a name="monitor-the-progress-of-the-export-operation"></a>Sledování průběhu operace exportu

Po spuštění [nový AzureRmSqlDatabaseExport] (https://msdn.microsoft.com/library/azure/mt603644(v=azure.300\).aspx), můžete zkontrolovat stav žádosti o spuštěním [Get-AzureRmSqlDatabaseImportExportStatus] (https://msdn.microsoft.com/library/azure/mt707794(v=azure.300\).aspx). Spuštěna ihned po zadání obvykle vrátí **Stav: probíhající**. Až uvidíte **Stav: úspěšně** dokončení exportu.


    Get-AzureRmSqlDatabaseImportExportStatus -OperationStatusLink $exportRequest.OperationStatusLink



## <a name="export-sql-database-example"></a>Export Příklad databáze SQL

Následující příklad exportuje existující databáze SQL BACPAC a potom ukazuje, jak chcete zkontrolovat stav operace exportu.

V příkladu spustíte existuje několik proměnných, které chcete nahradit určitých hodnot pro váš účet databáze a úložiště. V [Azure portál](https://portal.azure.com)přejděte ke svému účtu úložiště získat název účtu úložiště objektů blob jméno container a hodnoty klíče. Najděte klávesu kliknutím **přístupových kláves z verze** na váš účet zásuvné úložiště.

Nahrazení následující `VARIABLE-VALUES` s hodnotami pro konkrétní Azure zdroje. Název databáze je existující databázi, kterou chcete exportovat.



    $subscriptionId = "YOUR AZURE SUBSCRIPTION ID"

    Login-AzureRmAccount
    Set-AzureRmContext -SubscriptionId $subscriptionId

    # Database to export
    $DatabaseName = "DATABASE-NAME"
    $ResourceGroupName = "RESOURCE-GROUP-NAME"
    $ServerName = "SERVER-NAME"
    $serverAdmin = "ADMIN-NAME"
    $serverPassword = "ADMIN-PASSWORD" 
    $securePassword = ConvertTo-SecureString –String $serverPassword –AsPlainText -Force
    $creds = New-Object –TypeName System.Management.Automation.PSCredential –ArgumentList $serverAdmin, $securePassword

    # Generate a unique filename for the BACPAC
    $bacpacFilename = $DatabaseName + (Get-Date).ToString("yyyyMMddHHmm") + ".bacpac"

    # Storage account info for the BACPAC
    $BaseStorageUri = "https://STORAGE-NAME.blob.core.windows.net/BLOB-CONTAINER-NAME/"
    $BacpacUri = $BaseStorageUri + $bacpacFilename
    $StorageKeytype = "StorageAccessKey"
    $StorageKey = "YOUR STORAGE KEY"

    $exportRequest = New-AzureRmSqlDatabaseExport –ResourceGroupName $ResourceGroupName –ServerName $ServerName `
       –DatabaseName $DatabaseName –StorageKeytype $StorageKeytype –StorageKey $StorageKey -StorageUri $BacpacUri `
       –AdministratorLogin $creds.UserName –AdministratorLoginPassword $creds.Password
    $exportRequest

    # Check status of the export
    Get-AzureRmSqlDatabaseImportExportStatus -OperationStatusLink $exportRequest.OperationStatusLink



## <a name="next-steps"></a>Další kroky

- Informace o importu databáze Azure SQL pomocí prostředí Powershell najdete v tématu [Import BACPAC pomocí Powershellu](sql-database-import-powershell.md).


## <a name="additional-resources"></a>Další zdroje informací

- [Nové AzureRmSqlDatabaseExport] (https://msdn.microsoft.com/library/azure/mt707796(v=azure.300\).aspx)
- [Get-AzureRmSqlDatabaseImportExportStatus] (https://msdn.microsoft.com/library/azure/mt707794(v=azure.300\).aspx)