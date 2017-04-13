<properties
    pageTitle="Obnovení databáze SQL Azure ze zálohy geo nadbytečné (Powershellu) | Microsoft Azure"
    description="Obnovení databáze SQL Azure do nového serveru ze geo nadbytečné zálohy"
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
    ms.workload="NA"
    ms.date="07/17/2016"
    ms.author="sstein"/>

# <a name="restore-an-azure-sql-database-from-a-geo-redundant-backup-by-using-powershell"></a>Obnovení databáze SQL Azure ze zálohy geo nadbytečné pomocí prostředí PowerShell


> [AZURE.SELECTOR]
- [Základní informace](sql-database-recovery-using-backups.md)
- [GEO obnovení: Portál Azure](sql-database-geo-restore-portal.md)

Tento článek popisuje způsob obnovení databáze do nového serveru pomocí geo obnovit. Lze to prostřednictvím Powershellu.

[AZURE.INCLUDE [Start your PowerShell session](../../includes/sql-database-powershell.md)]

## <a name="geo-restore-your-database-into-a-standalone-database"></a>GEO obnovení databáze do samostatného databáze

1. Získání geo nadbytečné zálohování databáze, kterou chcete obnovit pomocí [Get-AzureRmSqlDatabaseGeoBackup] (https://msdn.microsoft.com/library/azure/mt693388(v=azure.300\).aspx) rutina.

        $GeoBackup = Get-AzureRmSqlDatabaseGeoBackup -ResourceGroupName "resourcegroup01" -ServerName "server01" -DatabaseName "database01"

2. Zahájit obnovení ze zálohy geo nadbytečné pomocí [obnovení-AzureRmSqlDatabase] (https://msdn.microsoft.com/library/azure/mt693390(v=azure.300\).aspx) rutina.

        Restore-AzureRmSqlDatabase –FromGeoBackup -ResourceGroupName "TargetResourceGroup" -ServerName "TargetServer" -TargetDatabaseName "RestoredDatabase" –ResourceId $GeoBackup.ResourceID -Edition "Standard" -RequestedServiceObjectiveName "S2"


## <a name="geo-restore-your-database-into-an-elastic-database-pool"></a>GEO obnovení databáze do fondu pružná databáze

1. Získání geo nadbytečné zálohování databáze, kterou chcete obnovit pomocí [Get-AzureRmSqlDatabaseGeoBackup] (https://msdn.microsoft.com/library/azure/mt693388(v=azure.300\).aspx) rutina.

        $GeoBackup = Get-AzureRmSqlDatabaseGeoBackup -ResourceGroupName "resourcegroup01" -ServerName "server01" -DatabaseName "database01"

2. Zahájit obnovení ze zálohy geo nadbytečné pomocí [obnovení-AzureRmSqlDatabase] (https://msdn.microsoft.com/library/azure/mt693390(v=azure.300\).aspx) rutina. Zadejte název fondu chcete obnovit databázi do.

        Restore-AzureRmSqlDatabase –FromGeoBackup -ResourceGroupName "TargetResourceGroup" -ServerName "TargetServer" -TargetDatabaseName "RestoredDatabase" –ResourceId $GeoBackup.ResourceID –ElasticPoolName "elasticpool01"  


## <a name="next-steps"></a>Další kroky

- Přehled kontinuitu business a scénáře najdete v tématu [Přehled kontinuitu Business](sql-database-business-continuity.md).
- Další informace o databáze SQL Azure automatické zálohy, najdete v článku [automatické zálohy databáze SQL](sql-database-automated-backups.md).
- Další informace o použití zálohy pro automatické obnovení najdete v tématu [obnovení databáze ze zálohy spuštěná služba](sql-database-recovery-using-backups.md).
- Další informace o rychlejší možnosti obnovení najdete v tématu [Aktivní Geo replikace](sql-database-geo-replication-overview.md).  
- Další informace o použití zálohy pro automatické archivace najdete v tématu [kopie databáze](sql-database-copy.md).
