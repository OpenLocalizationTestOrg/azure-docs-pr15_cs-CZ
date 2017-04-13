<properties
    pageTitle="Obnovení databáze Azure SQL předchozí čárky (Azure portál) | Microsoft Azure"
    description="Obnovení databáze Azure SQL předchozí bodu."
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


# <a name="restore-an-azure-sql-database-to-a-previous-point-in-time-with-the-azure-portal"></a>Obnovení databáze Azure SQL předchozí bod pomocí portálu Azure


> [AZURE.SELECTOR]
- [Základní informace](sql-database-recovery-using-backups.md)
- [Obnovení v okamžiku: prostředí PowerShell](sql-database-point-in-time-restore-powershell.md)

Tento článek ukazuje, jak obnovit předchozí v čase [že automatické zálohy databáze SQL](sql-database-automated-backups.md) Azure portálu.

## <a name="restore-a-sql-database-to-a-previous-point-in-time"></a>Obnovení databáze SQL předchozí bodu

Vyberte databázi, kterou chcete obnovit Azure portálu:

1.  Otevřete [portál Azure](https://portal.azure.com).
2.  Na levé straně obrazovky vyberte **Další služby** > **SQL databáze**.
3.  Klikněte na databázi, kterou chcete obnovit.
4.  V horní části stránky vaší databáze vyberte **Obnovit**:

    ![Obnovení databáze Azure SQL](./media/sql-database-point-in-time-restore-portal/restore.png)

5.  Na stránce **Obnovit** vyberte datum a čas (UTC včas) obnovit databázi a klikněte na **OK**:

    ![Obnovení databáze Azure SQL](./media/sql-database-point-in-time-restore-portal/restore-details.png)

## <a name="monitor-the-restore-operation"></a>Sledování obnovení

1. Po kliknutí na tlačítko **OK** v předchozím kroku, klikněte na ikonu upozornění v pravém horním rohu stránky a klikněte na upozornění **obnovení SQL databáze** podrobnosti.

    ![Obnovení databáze Azure SQL](./media/sql-database-point-in-time-restore-portal/notification-icon.png)

2. Otevře se stránka obnovení SQL databáze s informacemi o stavu na obnovit. Klepněte na položku řádek další podrobnosti:

    ![Obnovení databáze Azure SQL](./media/sql-database-point-in-time-restore-portal/inprogress.png)

 

## <a name="next-steps"></a>Další kroky

- Přehled kontinuitu business a scénáře najdete v tématu [Přehled kontinuitu Business](sql-database-business-continuity.md)
- Další informace o databáze SQL Azure automatické zálohy, najdete v článku [automatické zálohy databáze SQL](sql-database-automated-backups.md)
- Další informace o použití zálohy pro automatické obnovení najdete v tématu [obnovení databáze ze zálohy spuštěná služba](sql-database-recovery-using-backups.md)
- Další informace o rychlejší možnosti obnovení najdete v tématu [Aktivní Geo replikace](sql-database-geo-replication-overview.md)  
- Další informace o použití zálohy pro automatické archivace najdete v tématu [kopie databáze](sql-database-copy.md)
