<properties
    pageTitle="Obnovení databáze Azure SQL z automatické zálohy (Azure portál) | Microsoft Azure"
    description="Obnovení databáze Azure SQL z automatické zálohy (Azure portál)."
    services="sql-database"
    documentationCenter=""
    authors="stevestein"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.date="10/18/2016"
    ms.author="sstein"
    ms.workload="NA"
    ms.topic="article"
    ms.tgt_pltfrm="NA"/>


# <a name="restore-an-azure-sql-database-from-an-automatic-backup-using-the-azure-portal"></a>Obnovení databáze Azure SQL z automatické zálohy pomocí portálu Azure


> [AZURE.SELECTOR]
- [Základní informace](sql-database-recovery-using-backups.md#geo-restore)
- [Obnovení GEO: prostředí PowerShell](sql-database-geo-restore-powershell.md)

Tento článek ukazuje, jak obnovit z [Automatické zálohování](sql-database-automated-backups.md) do nového serveru s [Geo obnovit](sql-database-recovery-using-backups/.md#geo-restore) pomocí portálu Azure.

## <a name="select-a-database-to-restore"></a>Vyberte databázi, kterou chcete obnovit

Obnovení databáze na portálu Azure, proveďte následující kroky:

1.  Přejděte na [portál Azure](https://portal.azure.com).
2.  Na levé straně obrazovky vyberte **+ Nový** > **databází** > **Databáze SQL**:

    ![Obnovení databáze Azure SQL](./media/sql-database-geo-restore-portal/new-sql-database.png)

3.  Vyberte **záložní** jako zdroj a pak vyberte zálohy, kterou chcete obnovit. Zadejte název databáze, serveru, kterou chcete obnovit databázi, a potom klikněte na **vytvořit**:
  
    ![Obnovení databáze Azure SQL](./media/sql-database-geo-restore-portal/geo-restore.png)

Sledování stavu operace obnovení kliknutím na ikonu upozornění v pravém horním rohu stránky. 


## <a name="next-steps"></a>Další kroky

- Přehled kontinuitu business a scénáře najdete v tématu [Přehled kontinuitu Business](sql-database-business-continuity.md)
- Další informace o databáze SQL Azure automatické zálohy, najdete v článku [automatické zálohy databáze SQL](sql-database-automated-backups.md)
- Další informace o použití zálohy pro automatické obnovení najdete v tématu [obnovení databáze ze zálohy spuštěná služba](sql-database-recovery-using-backups.md)
- Další informace o rychlejší možnosti obnovení najdete v tématu [Aktivní Geo replikace](sql-database-geo-replication-overview.md)  
- Další informace o použití zálohy pro automatické archivace najdete v tématu [kopie databáze](sql-database-copy.md)
