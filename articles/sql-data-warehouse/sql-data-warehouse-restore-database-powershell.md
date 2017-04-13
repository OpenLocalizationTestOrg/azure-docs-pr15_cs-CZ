<properties
   pageTitle="Obnovení Azure SQL datový sklad (Powershellu) | Microsoft Azure"
   description="Prostředí PowerShell úkoly při obnovení datový sklad SQL Azure."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="Lakshmi1812"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="09/21/2016"
   ms.author="lakshmir;barbkess;sonyama"/>

# <a name="restore-an-azure-sql-data-warehouse-powershell"></a>Obnovení Azure SQL datový sklad (Powershellu)

> [AZURE.SELECTOR]
- [Základní informace][]
- [Portál][]
- [Prostředí PowerShell][]
- [ZBÝVAJÍCÍ][]

V tomto článku se dozvíte, jak obnovit sklad dat SQL Azure pomocí Powershellu.

## <a name="before-you-begin"></a>Než začnete

**Ověřte DTU kapacity.** Každý SQL datový sklad je hostovaný aplikací SQL server (například myserver.database.windows.net), který má výchozí kvóta DTU.  Před obnovením datový sklad SQL, ověřte, že serveru SQL server má dost zbývající DTU kvóty pro databáze obnovena. Zjistěte, jak vypočítat DTU potřeby nebo požádejte o další DTU, najdete v článku [požadavek na změnu DTU kvóty][].

### <a name="install-powershell"></a>Instalace prostředí PowerShell

Abyste mohli pomocí prostředí PowerShell Azure SQL datový sklad, budou muset nainstalovat Azure PowerShell verze 1.0 nebo vyšší.  Zkontrolujte verzi spuštěním **modul Get - ListAvailable – název AzureRM**.  Nejnovější verze lze nainstalovat z [Webové platformy Microsoft][].  Další informace o instalaci nejnovější verze najdete v článku [Jak nainstalovat a nakonfigurovat Azure Powershellu][].

## <a name="restore-an-active-or-paused-database"></a>Obnovení databáze aktivní nebo pozastaveného

Obnovení databáze ze snímku pomocí rutiny prostředí PowerShell [AzureRmSqlDatabase obnovit][] .

1. Otevřete Windows PowerShell.
2. Připojení k účtu Azure a výpis všech předplatných přidruženého k vašemu účtu.
3. Vyberte předplatné, které obsahuje databáze, kterou chcete obnovit.
4. Seznam body obnovení databáze.
5. Vyberte požadovanou obnovení bod pomocí RestorePointCreationDate.
6. Obnovení databáze bodu požadované obnovení.
7. Ověřte, zda je obnovený databáze online.

```Powershell

$SubscriptionName="<YourSubscriptionName>"
$ResourceGroupName="<YourResourceGroupName>"
$ServerName="<YourServerNameWithoutURLSuffixSeeNote>"  # Without database.windows.net
$DatabaseName="<YourDatabaseName>"
$NewDatabaseName="<YourDatabaseName>"

Login-AzureRmAccount
Get-AzureRmSubscription
Select-AzureRmSubscription -SubscriptionName $SubscriptionName

# List the last 10 database restore points
((Get-AzureRMSqlDatabaseRestorePoints -ResourceGroupName $ResourceGroupName -ServerName $ServerName -DatabaseName ($DatabaseName).RestorePointCreationDate)[-10 .. -1]

# Or list all restore points
Get-AzureRmSqlDatabaseRestorePoints -ResourceGroupName $ResourceGroupName -ServerName $ServerName -DatabaseName $DatabaseName

# Get the specific database to restore
$Database = Get-AzureRmSqlDatabase -ResourceGroupName $ResourceGroupName -ServerName $ServerName -DatabaseName $DatabaseName

# Pick desired restore point using RestorePointCreationDate
$PointInTime="<RestorePointCreationDate>"  

# Restore database from a restore point
$RestoredDatabase = Restore-AzureRmSqlDatabase –FromPointInTimeBackup –PointInTime $PointInTime -ResourceGroupName $Database.ResourceGroupName -ServerName $Database.$ServerName -TargetDatabaseName $NewDatabaseName –ResourceId $Database.ResourceID

# Verify the status of restored database
$RestoredDatabase.status

```

>[AZURE.NOTE] Po dokončení obnovení můžete nakonfigurovat obnoveného databázi tak, že následující [Konfigurace databáze po obnovení][].


## <a name="restore-a-deleted-database"></a>Obnovení odstraněné databáze

Pokud chcete obnovit odstraněnou databázi, získáte pomocí rutiny [AzureRmSqlDatabase obnovit][] .

1. Otevřete Windows PowerShell.
2. Připojení k účtu Azure a výpis všech předplatných přidruženého k vašemu účtu.
3. Vyberte předplatné, které obsahuje odstraněné databáze, kterou chcete obnovit.
4. Pokud potřebujete konkrétní odstraněné databáze.
5. Obnovení odstraněné databáze.
6. Ověřte, zda je online obnovenou databázi.

```Powershell
$SubscriptionName="<YourSubscriptionName>"
$ResourceGroupName="<YourResourceGroupName>"
$ServerName="<YourServerNameWithoutURLSuffixSeeNote>"  # Without database.windows.net
$DatabaseName="<YourDatabaseName>"
$NewDatabaseName="<YourDatabaseName>"

Login-AzureRmAccount
Get-AzureRmSubscription
Select-AzureRmSubscription -SubscriptionName $SubscriptionName

# Get the deleted database to restore
$DeletedDatabase = Get-AzureRmSqlDeletedDatabaseBackup -ResourceGroupName $ResourceGroupNam -ServerName $ServerName -DatabaseName $DatabaseName

# Restore deleted database
$RestoredDatabase = Restore-AzureRmSqlDatabase –FromDeletedDatabaseBackup –DeletionDate $DeletedDatabase.DeletionDate -ResourceGroupName $DeletedDatabase.ResourceGroupName -ServerName $DeletedDatabase.ServerName -TargetDatabaseName $NewDatabaseName –ResourceId $DeletedDatabase.ResourceID

# Verify the status of restored database
$RestoredDatabase.status
```

>[AZURE.NOTE] Po dokončení obnovení můžete nakonfigurovat obnoveného databázi tak, že následující [Konfigurace databáze po obnovení][].


## <a name="restore-from-an-azure-geographical-region"></a>Obnovení z Azure zeměpisnou oblast

Když Pokud chcete obnovit databázi, získáte pomocí rutiny [AzureRmSqlDatabase obnovit][] .

1. Otevřete Windows PowerShell.
2. Připojení k účtu Azure a výpis všech předplatných přidruženého k vašemu účtu.
3. Vyberte předplatné, které obsahuje databáze, kterou chcete obnovit.
4. Pokud potřebujete databázi, kterou chcete obnovit.
5. Vytvoření žádosti o obnovení databáze.
6. Zkontrolujte stav databáze geo obnovit.

```Powershell
Login-AzureRmAccount
Get-AzureRmSubscription
Select-AzureRmSubscription -SubscriptionName "<Subscription_name>"

# Get the database you want to recover
$GeoBackup = Get-AzureRmSqlDatabaseGeoBackup -ResourceGroupName "<YourResourceGroupName>" -ServerName "<YourServerName>" -DatabaseName "<YourDatabaseName>"

# Recover database
$GeoRestoredDatabase = Restore-AzureRmSqlDatabase –FromGeoBackup -ResourceGroupName "<YourResourceGroupName>" -ServerName "<YourTargetServer>" -TargetDatabaseName "<NewDatabaseName>" –ResourceId $GeoBackup.ResourceID

# Verify that the geo-restored database is online
$GeoRestoredDatabase.status
```

>[AZURE.NOTE] Po dokončení obnovení nastavení databáze naleznete v tématu [Konfigurace databáze po obnovení][]. 


Pokud zdrojové databáze zapnuté TDE, budou obnoveného databáze TDE s podporou.


## <a name="next-steps"></a>Další kroky
Další informace o funkcích business kontinuitu edicí databáze SQL Azure, přečtěte si [databáze SQL Azure obchodní kontinuitu přehled][].

<!--Image references-->

<!--Article references-->
[Základní informace kontinuitu firmy databáze SQL Azure]: sql-database-business-continuity.md
[Požadavek na změnu kvóty DTU]: ./sql-data-warehouse-get-started-create-support-ticket.md#request-quota-change
[Konfigurace databáze po obnovení]: ./sql-database-disaster-recovery.md#configure-your-database-after-recovery
[Instalace a konfigurace prostředí PowerShell Azure]: powershell-install-configure.md
[Základní informace]: ./sql-data-warehouse-restore-database-overview.md
[Portál]: ./sql-data-warehouse-restore-database-portal.md
[Prostředí PowerShell]: ./sql-data-warehouse-restore-database-powershell.md
[ZBÝVAJÍCÍ]: ./sql-data-warehouse-restore-database-rest-api.md
[Konfigurace databáze po obnovení]: ./sql-database-disaster-recovery.md#configure-your-database-after-recovery

<!--MSDN references-->
[Obnovení AzureRmSqlDatabase]: https://msdn.microsoft.com/library/mt693390.aspx

<!--Other Web references-->
[Azure Portal]: https://portal.azure.com/
[Microsoft Web platformy]: https://aka.ms/webpi-azps
