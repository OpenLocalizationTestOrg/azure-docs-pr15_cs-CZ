<properties
    pageTitle="Obnovení odstraněné databáze Azure SQL (Azure portál) | Microsoft Azure"
    description="Obnovení odstraněné databáze Azure SQL (Azure portál)."
    services="sql-database"
    documentationCenter=""
    authors="stevestein"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.date="10/12/2016"
    ms.author="sstein"
    ms.workload="NA"
    ms.topic="article"
    ms.tgt_pltfrm="NA"/>


# <a name="restore-a-deleted-azure-sql-database-using-the-azure-portal"></a>Obnovení odstraněné databáze Azure SQL pomocí portálu Azure

> [AZURE.SELECTOR]
- [Základní informace](sql-database-recovery-using-backups.md)
- [**Obnovení odstraněných DB: portál**](sql-database-restore-deleted-database-portal.md)
- [Obnovení odstraněných DB: prostředí PowerShell](sql-database-restore-deleted-database-powershell.md)

## <a name="select-the-database-to-restore"></a>Vyberte databázi, kterou chcete obnovit 

Obnovení odstraněné databáze Azure portálu:

1.  V [Azure portál](https://portal.azure.com), klikněte na **Další služby** > **serverů SQL**.
3.  Vyberte server, který obsahoval databázi, kterou chcete obnovit.
4.  Přejděte dolů do části **operace** zásuvné serveru a vyberte **Odstraněná databáze**: ![obnovení databáze Azure SQL](./media/sql-database-restore-deleted-database-portal/restore-deleted-trashbin.png)
5.  Vyberte databázi, kterou chcete obnovit.
6.  Zadejte název databáze a klikněte na **OK**:

    ![Obnovení databáze Azure SQL](./media/sql-database-restore-deleted-database-portal/restore-deleted.png)


## <a name="next-steps"></a>Další kroky

- Přehled kontinuitu business a scénáře najdete v tématu [Přehled kontinuitu Business](sql-database-business-continuity.md)
- Další informace o databáze SQL Azure automatické zálohy, najdete v článku [automatické zálohy databáze SQL](sql-database-automated-backups.md)
- Další informace o použití zálohy pro automatické obnovení najdete v tématu [obnovení databáze ze zálohy spuštěná služba](sql-database-recovery-using-backups.md)
- Další informace o rychlejší možnosti obnovení najdete v tématu [Aktivní Geo replikace](sql-database-geo-replication-overview.md)  
- Další informace o použití zálohy pro automatické archivace najdete v tématu [kopie databáze](sql-database-copy.md)
