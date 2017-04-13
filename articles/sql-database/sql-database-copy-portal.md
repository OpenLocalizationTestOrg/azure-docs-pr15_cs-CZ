<properties
    pageTitle="Zkopírujte databázi Azure SQL pomocí portálu Azure | Microsoft Azure"
    description="Vytvořit kopii databáze Azure SQL"
    services="sql-database"
    documentationCenter=""
    authors="stevestein"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.date="09/19/2016"
    ms.author="sstein"
    ms.workload="data-management"
    ms.topic="article"
    ms.tgt_pltfrm="NA"/>



# <a name="copy-an-azure-sql-database-using-the-azure-portal"></a>Zkopírujte databázi SQL Azure pomocí portálu Azure

> [AZURE.SELECTOR]
- [Základní informace](sql-database-copy.md)
- [Azure portálu](sql-database-copy-portal.md)
- [Prostředí PowerShell](sql-database-copy-powershell.md)
- [T-SQL](sql-database-copy-transact-sql.md)

Podle těchto kroků ukazují, jak zkopírovat databáze SQL pomocí [portálu Azure](https://portal.azure.com) na stejný server nebo jiný server.

Zkopírovat databázi SQL, musíte následující položky:

- Předplatné Azure. Pokud potřebujete předplatné Azure jednoduše klikněte na **Bezplatnou zkušební verzi** v horní části této stránky a pak se vrátit k dokončení tohoto článku.
- Databáze SQL zkopírovat. Pokud nemáte SQL databáze, vytvořte jednu kroků v tomto článku: [vytvoření první databáze SQL Azure](sql-database-get-started.md).


## <a name="copy-your-sql-database"></a>Kopírování databáze SQL

Otevřete stránku databáze SQL databáze, kterou chcete zkopírovat:

1.  Přejděte na [portál Azure](https://portal.azure.com).
2.  Klikněte na **Další služby** > **SQL databáze**a potom klikněte na požadovanou databázi.
3.  Na stránce SQL databáze klikněte na **Kopírovat**:

    ![Databáze SQL](./media/sql-database-copy-portal/sql-database-copy.png)

1.  Na stránce **zkopírovat** je k dispozici výchozí název databáze. Pokud chcete, zadejte jiný název (všechny databáze na serveru musí mít jedinečné názvy).
2.  Vyberte **cílový server**. Cílový server je, kde je vytvořena kopie databáze. Zkopírujte databázi na stejný server nebo jiný server. Můžete vytvořit server nebo vyberte stávající server ze seznamu. 
3.  Po výběru požadovaných možností **cílový server**, **fondu pružná databáze**a **ceny úroveň** bude povoleno. Pokud server má fond, můžete zkopírovat databáze do ní.
3.  Kliknutím na **OK** spusťte proces kopírování.

    ![Databáze SQL](./media/sql-database-copy-portal/copy-page.png)


## <a name="monitor-the-progress-of-the-copy-operation"></a>Sledování průběhu operace kopírování

- Po spuštění výtisku, klikněte na portálu upozornění podrobnosti.

    ![oznámení][3]
 
    ![sledování][4]


## <a name="verify-the-database-is-live-on-the-server"></a>Ověřte, zda že je databáze aktivní na serveru

- Klikněte na **Další služby** > **SQL databáze** a ověřte nové databáze je **Online**.


## <a name="resolve-logins"></a>Řešení přihlášení

Po dokončení operace kopírování se dá přihlášení, najdete v článku [řešení přihlášení](sql-database-copy-transact-sql.md#resolve-logins-after-the-copy-operation-completes)


## <a name="next-steps"></a>Další kroky

- Základní informace o kopírování databáze SQL Azure v tématu [kopírování databáze Azure SQL](sql-database-copy.md) .
- V tématu kopírování databáze pomocí prostředí PowerShell [kopii databáze Azure SQL pomocí příkazu Powershellu](sql-database-copy-powershell.md) .
- V tématu kopírování databáze pomocí jazyce Transact-SQL [kopii databáze Azure SQL pomocí příkazu T-SQL](sql-database-copy-transact-sql.md) .
- Zjistěte, [jak spravovat zabezpečení databáze Azure SQL po obnovení havárie](sql-database-geo-replication-security-config.md) Další informace o správě uživatelů a přihlášení při kopírování databáze na jiný logické server.



## <a name="additional-resources"></a>Další zdroje informací

- [Správa přihlášení](sql-database-manage-logins.md)
- [Připojení k databázi SQL s SQL Server Management Studio a provádění ukázkový T-SQL dotaz](sql-database-connect-query-ssms.md)
- [Export databáze BACPAC](sql-database-export.md)
- [Obchodní kontinuitu přehled](sql-database-business-continuity.md)
- [Dokumentace databáze SQL](https://azure.microsoft.com/documentation/services/sql-database/)




<!--Image references-->
[1]: ./media/sql-database-copy-portal/copy.png
[2]: ./media/sql-database-copy-portal/copy-ok.png
[3]: ./media/sql-database-copy-portal/copy-notification.png
[4]: ./media/sql-database-copy-portal/monitor-copy.png

