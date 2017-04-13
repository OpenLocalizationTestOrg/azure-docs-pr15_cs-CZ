<properties 
    pageTitle="Zkopírujte databázi Azure SQL pomocí jazyce Transact-SQL | Microsoft Azure" 
    description="Vytvoření kopie databáze Azure SQL pomocí jazyce Transact-SQL" 
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


# <a name="copy-an-azure-sql-database-using-transact-sql"></a>Zkopírujte databázi Azure SQL pomocí jazyce Transact-SQL


> [AZURE.SELECTOR]
- [Základní informace](sql-database-copy.md)
- [Azure portálu](sql-database-copy-portal.md)
- [Prostředí PowerShell](sql-database-copy-powershell.md)
- [T-SQL](sql-database-copy-transact-sql.md)


Následující kroky ukazují, jak zkopírovat databáze SQL pomocí jazyce Transact-SQL na stejný server nebo jiný server. Kopírování databáze pomocí příkazu [Vytvořit databázi](https://msdn.microsoft.com/library/ms176061.aspx) .

Kroky v tomto článku je potřeba k provedení těchto věcí:

- Předplatné Azure. Pokud potřebujete předplatné Azure jednoduše klikněte na **Bezplatnou zkušební verzi** v horní části této stránky a pak se vrátit k dokončení tohoto článku.
- Databáze Azure SQL. Pokud nemáte SQL databáze, vytvořte jednu kroků v tomto článku: [vytvoření první databáze SQL Azure](sql-database-get-started.md).
- [SQL Server Management Studio (SSMS)](https://msdn.microsoft.com/library/ms174173.aspx). Pokud nemáte SSMS nebo pokud funkcí popsaných v tomto článku jsou k dispozici, [Stáhněte si nejnovější verzi](https://msdn.microsoft.com/library/mt238290.aspx).


## <a name="copy-your-sql-database"></a>Kopírování databáze SQL

Přihlaste se k databázi předlohy pomocí přihlášení hlavní úrovni serveru nebo přihlášení, vytvořenou databázi, kterou chcete kopírovat. Přihlášení, které nejsou hlavní úrovni serveru musí být členem dbmanager role kopírovat databází. Další informace o přihlášení a připojení k serveru najdete v článku [Správa přihlášení](sql-database-manage-logins.md).

Spusťte kopírování zdrojové databáze příkazem [Vytvořit databázi](https://msdn.microsoft.com/library/ms176061.aspx) . Provádění tohoto příkazu zahájí databázi kopírování obrázku. Vzhledem k tomu kopírování databáze asynchronní proces, příkaz Vytvořit databázi vrátí před dokončením databázi kopírování.


### <a name="copy-a-sql-database-to-the-same-server"></a>Zkopírujte databázi SQL na stejný server

Přihlaste se k databázi předlohy pomocí přihlášení hlavní úrovni serveru nebo přihlášení, vytvořenou databázi, kterou chcete kopírovat. Přihlášení, které nejsou hlavní úrovni serveru musí být členem dbmanager role kopírovat databází.

Tento příkaz kopií Database1 k novou databázi s názvem Database2 na stejný server. V závislosti na velikosti databáze zkopírování může chvíli trvat dokončete.

    -- Execute on the master database.
    -- Start copying.
    CREATE DATABASE Database1_copy AS COPY OF Database1;

### <a name="copy-a-sql-database-to-a-different-server"></a>Zkopírujte databázi SQL na jiný server

Přihlaste se k databázi předlohy cílového serveru, databázi SQL Azure serveru, kde má vytvořit novou databázi. Použití přihlášení, které má stejné jméno a heslo jsou vlastníka databáze (DBO) zdrojové databáze na serveru databáze SQL Azure zdroje. Přihlášení na cílovém serveru musí taky být členem dbmanager role nebo přihlášení hlavní úrovni serveru.

Tento příkaz kopií Database1 server1 – novou databázi s názvem Database2 na server2. V závislosti na velikosti databáze zkopírování může chvíli trvat dokončete.


    -- Execute on the master database of the target server (server2)
    -- Start copying from Server1 to Server2
    CREATE DATABASE Database1_copy AS COPY OF server1.Database1;
    

## <a name="monitor-the-progress-of-the-copy-operation"></a>Sledování průběhu operace kopírování

Proces kopírování sledujte pomocí dotazu sys.databases a sys.dm_database_copies zobrazení. Probíhá kopírování, kopírování nastavenou sloupci state_desc sys.databases zobrazení pro novou databázi.


- Pokud kopírování selže, sloupci state_desc sys.databases zobrazení nové databáze je nastavena na PODEZŘELÉ. V tomto případě provedení příkazu PŘETAŽENÍ na nové databáze a zkuste to později.
- Pokud kopírování úspěšné, sloupci state_desc sys.databases zobrazení nové databáze je nastavený na ONLINE. V tomto případě kopírování dokončení a nové databáze je standardní databázi nelze změnit nezávisle na zdrojové databáze.

> [AZURE.NOTE] – Pokud se rozhodnete zrušit kopírování probíhá ho, spusťte příkaz [ODPOJIT databázi](https://msdn.microsoft.com/library/ms178613.aspx) na novou databázi. Můžete taky spouštěním uvolnění databáze na zdrojové databáze zruší také proces kopírování.


## <a name="resolve-logins-after-the-copy-operation-completes"></a>Po dokončení operace kopírování vyřešit přihlášení

Po online na serveru cílové novou databázi použijte příkaz [ALTER USER](https://msdn.microsoft.com/library/ms176060.aspx) přemapovat uživatelům přihlášení na cílovém serveru z novou databázi. Řešení osamocené uživatele, přečtěte si článek [Poradce při potížích s osamocené uživatelů](https://msdn.microsoft.com/library/ms175475.aspx). Další informace najdete v článku [Správa zabezpečení databáze Azure SQL po obnovení havárie](sql-database-geo-replication-security-config.md).

Všichni uživatelé v nové databázi spravovat oprávnění, která jako ve zdrojové databáze. Kopie databáze, které iniciuje uživatel může být vlastník databáze nové databáze a přiřazen nového zabezpečení ID. Po úspěšném kopírování a před ostatními uživateli se znovu namapovat pouze přihlašovací jméno, které iniciuje kopírování, vlastníka databáze (DBO), přihlášení do nové databáze.


## <a name="next-steps"></a>Další kroky

- Základní informace o kopírování databáze SQL Azure v tématu [kopírování databáze Azure SQL](sql-database-copy.md) .
- V tématu [kopírování databáze Azure SQL pomocí portálu Azure](sql-database-copy-portal.md) zkopírujte databáze pomocí portálu Azure.
- V tématu kopírování databáze pomocí prostředí PowerShell [kopii databáze Azure SQL pomocí příkazu Powershellu](sql-database-copy-powershell.md) .
- Zjistěte, [jak spravovat zabezpečení databáze Azure SQL po obnovení havárie](sql-database-geo-replication-security-config.md) Další informace o správě uživatelů a přihlášení při kopírování databáze na jiný logické server.



## <a name="additional-resources"></a>Další zdroje informací

- [Správa přihlášení](sql-database-manage-logins.md)
- [Připojení k databázi SQL s SQL Server Management Studio a provádění ukázkový T-SQL dotaz](sql-database-connect-query-ssms.md)
- [Export databáze BACPAC](sql-database-export.md)
- [Obchodní kontinuitu přehled](sql-database-business-continuity.md)
- [Dokumentace databáze SQL](https://azure.microsoft.com/documentation/services/sql-database/)


