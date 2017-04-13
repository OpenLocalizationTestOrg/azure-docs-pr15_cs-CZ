<properties
    pageTitle="Zkopírujte databázi Azure SQL | Microsoft Azure"
    description="Vytvořit kopii databáze Azure SQL"
    services="sql-database"
    documentationCenter=""
    authors="anosov1960"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.date="10/24/2016"
    ms.author="sstein; sashan"
    ms.workload="data-management"
    ms.topic="article"
    ms.tgt_pltfrm="NA"/>



# <a name="copy-an-azure-sql-database"></a>Zkopírujte databázi Azure SQL

> [AZURE.SELECTOR]
- [Základní informace](sql-database-copy.md)
- [Azure portálu](sql-database-copy-portal.md)
- [Prostředí PowerShell](sql-database-copy-powershell.md)
- [T-SQL](sql-database-copy-transact-sql.md)

Azure [SQL databáze automatické zálohy](sql-database-automated-backups.md) můžete použít k vytvoření kopie databáze SQL. Kopie databáze používá stejnou technologii jako funkce geo replikace. Ale na rozdíl od geo replikace ukončí odkaz replikace jako po dokončení osazení fáze. Kopie databáze tedy snímek zdrojové databáze od doby požadavku na kopírování.  
Vytvoření kopie databáze na stejný server nebo jiný server. Služba osy a výkonu úrovně (ceny vrstvy) kopie databáze jsou stejné jako zdrojové databáze ve výchozím nastavení. Při použití rozhraní API, můžete vybrat jiné výkonu úroveň v rámci stejné vrstvy služeb (edition). Po dokončení výtisku výtisku bude plně funkční nezávisle na databázi. V tomto okamžiku můžete upgradovat nebo přejít na libovolnou verzí starší. Přihlášení, uživatelé a oprávnění dá se ovládat nezávisle na sobě.  

Při kopírování databáze na stejný server logické stejné přihlášení lze použít v obou databází. Hlavní že slouží ke zkopírování databázi zabezpečení bude vlastníka databáze (DBO) na novou databázi. Všichni uživatelé databázi, svá oprávnění a jejich zabezpečení identifikátory zkopírují kopii databáze.  

Když zkopírujete databáze na jiný logické server, bude zabezpečení na novém serveru vlastník databáze na novou databázi. Pokud používáte [obsažené uživatelů databáze](sql-database-manage-logins.md) zajišťuje přístup k datům primárních a sekundárních databází vždy mít stejný přihlašovací údaje uživatele, aby po dokončení kopie budete mít okamžitě přístup s stejné přihlašovací údaje. Pokud používáte [Azure Active Directory](../active-directory/active-directory-whatis.md), můžete úplně odstranit potřebné pro správu přihlašovací údaje do pole kopírovat. Ale když zkopírujete databáze do nového serveru, přístup pro přihlášení na základě se obecně nehodí, protože neexistují přihlášení na novém serveru. Zjistěte, [jak spravovat zabezpečení databáze Azure SQL po obnovení havárie](sql-database-geo-replication-security-config.md) Další informace o správě přihlášení při kopírování databáze na jiný logické server. 

Zkopírovat databázi SQL, musíte:

- Předplatné Azure. Pokud potřebujete předplatné Azure jednoduše klikněte na **Bezplatnou zkušební verzi** v horní části této stránky a pak se vrátit k dokončení tohoto článku.
- Databáze SQL zkopírovat. Pokud nemáte SQL databáze, vytvořte jednu kroků v tomto článku: [vytvoření první databáze SQL Azure](sql-database-get-started.md).

## <a name="next-steps"></a>Další kroky

- V tématu [kopírování databáze Azure SQL pomocí portálu Azure](sql-database-copy-portal.md) zkopírujte databáze pomocí portálu Azure.
- V tématu kopírování databáze pomocí prostředí PowerShell [kopii databáze Azure SQL pomocí příkazu Powershellu](sql-database-copy-powershell.md) .
- V tématu kopírování databáze pomocí jazyce Transact-SQL [kopii databáze Azure SQL pomocí příkazu T-SQL](sql-database-copy-transact-sql.md) .
- Zjistěte, [jak spravovat zabezpečení databáze Azure SQL po obnovení havárie](sql-database-geo-replication-security-config.md) Další informace o správě uživatelů a přihlášení při kopírování databáze na jiný logické server.



## <a name="additional-resources"></a>Další zdroje informací

- [Správa přihlášení](sql-database-manage-logins.md)
- [Připojení k databázi SQL s SQL Server Management Studio a provádění ukázkový T-SQL dotaz](sql-database-connect-query-ssms.md)
- [Export databáze BACPAC](sql-database-export.md)
- [Obchodní kontinuitu přehled](sql-database-business-continuity.md)
- [Dokumentace databáze SQL](https://azure.microsoft.com/documentation/services/sql-database/)
