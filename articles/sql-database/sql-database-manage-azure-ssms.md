<properties 
    pageTitle="Správa databáze SQL pomocí SSMS | Microsoft Azure" 
    description="Naučte se používat SQL Server Management Studio ke správě databáze SQL servery a databází." 
    services="sql-database" 
    documentationCenter=".net" 
    authors="stevestein" 
    manager="jhubbard" 
    editor="tysonn"/>

<tags 
    ms.service="sql-database" 
    ms.workload="data-management" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/29/2016" 
    ms.author="sstein"/>


# <a name="managing-azure-sql-database-using-sql-server-management-studio"></a>Správa databáze SQL Azure pomocí SQL Server Management Studio 


> [AZURE.SELECTOR]
- [Azure portálu](sql-database-manage-portal.md)
- [SSMS](sql-database-manage-azure-ssms.md)
- [Prostředí PowerShell](sql-database-manage-powershell.md)

SQL Server Management Studio (SSMS) slouží ke správě databáze SQL Azure servery a databází. Toto téma vás provede běžné úkoly s SSMS. Má už server a databázi vytvořenou v databázi SQL Azure, než začnete. Další informace naleznete v tématu [vytvoření první databáze SQL Azure](sql-database-get-started.md) a [připojit a dotaz pomocí SSMS](sql-database-connect-query-ssms.md) .

Doporučujeme používat nejnovější verzi aplikace SSMS při práci s databáze SQL Azure. 

> [AZURE.IMPORTANT] Vždy použijte nejnovější verzi SSMS, protože neustále lepší pro práci s nejnovější aktualizace a databáze SQL Azure. Získání nejnovější verzi, naleznete v článku [Stažení SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx).



## <a name="create-and-manage-azure-sql-databases"></a>Vytváření a Správa databáze Azure SQL

Při připojení k databázi **předlohy** , můžete vytvořit databáze na serveru a změnit nebo přetáhnout existující databáze. Následující kroky popisují, jak provádět několik běžné úlohy správy databáze, až Management Studio. Abyste mohli provést tyto úlohy, zkontrolujte, že jste připojení k databázi **předlohy** s přihlášením hlavní úrovni serveru, který jste vytvořili při nastavení serveru.

Otevřete okno s dotazem v Management Studio otevřete složku, do databáze, rozbalte složku **Systémové databáze** , klikněte pravým tlačítkem myši na **hlavní**a potom klikněte na **Nový dotaz**.

-   Vytvoření databáze pomocí příkazu **Vytvořit databázi** . Další informace najdete v tématu [Vytvoření databáze (databáze SQL)](https://msdn.microsoft.com/library/dn268335.aspx). Následující výraz vytvoří databázi s názvem **myTestDB** a určuje, že je to standardní vydání S0 databáze pomocí výchozí maximální velikosti doručovaných 250 GB.

        CREATE DATABASE myTestDB
        (EDITION='Standard',
         SERVICE_OBJECTIVE='S0');

Klikněte na tlačítko **Spustit** spusťte dotaz.

-   Pomocí příkazu **Vlastnosti databáze** upravte existující databázi, například pokud chcete změnit název a edition databáze. Další informace najdete v tématu [Vlastnosti databáze (databáze SQL)](https://msdn.microsoft.com/library/ms174269.aspx). Následující příkaz upravuje databázi jste vytvořili v předchozím kroku změnit edition na standardní S1.

        ALTER DATABASE myTestDB
        MODIFY
        (SERVICE_OBJECTIVE='S1');

-   Umožňuje odstranit existující databázi **ODPOJIT databázi** údajů. Další informace najdete v tématu [DROP DATABASE (databáze SQL)](https://msdn.microsoft.com/library/ms178613.aspx). Následující příkaz odstraní **myTestDB** databázi, ale nechcete pusťte teď protože vám ji použijete k vytváření přihlášení v dalším kroku.

        DROP DATABASE myTestBase;

-   Hlavní databáze obsahuje **sys.databases** zobrazení, které můžete zobrazit údaje o všechny databáze. Pokud chcete zobrazit všechny existující databáze, spusťte následující příkaz:

        SELECT * FROM sys.databases;

-   V SQL databázi **pomocí** příkazu není podporována pro přepínání mezi databázemi Místo toho budete muset vytvořit připojení přímo do cílové databáze.

>[AZURE.NOTE] Spoustu jazyce Transact-SQL příkazy, které vytvoření nebo změně databáze spuštěním ve vlastní list a zformátuje se další příkazy jazyce Transact-SQL. Další informace najdete v tématu informace o jednotlivých údajů.

## <a name="create-and-manage-logins"></a>Vytváření a správa přihlášení

**Hlavní** databáze obsahuje přihlášení a které přihlášení přiřazena oprávnění k vytvoření databáze nebo jiných přihlášení. Přihlášení můžete spravovat po připojení k databázi **předlohy** s přihlášením hlavní úrovni serveru, který jste vytvořili při nastavení serveru. Příkazy **Vytvořit přihlášení**, **Změnit PŘIHLAŠOVACÍ**nebo **Přímé přihlášení** umožňuje spouštění dotazů předlohy databázi, která spravuje přihlášení napříč celou serveru. Další informace najdete v tématu [Správa databáze a přihlášení do SQL databáze](http://msdn.microsoft.com/library/azure/ee336235.aspx). 


-   Umožňuje vytvořit přihlášení úrovni serveru **Vytvořit PŘIHLAŠOVACÍCH** údajů. Další informace najdete v tématu [Vytvoření LOGIN (databáze SQL)](https://msdn.microsoft.com/library/ms189751.aspx). Následující výraz vytvoří přihlášení s názvem **login1**. Nahraďte **hesla1** heslem podle svého výběru.

        CREATE LOGIN login1 WITH password='password1';

-   Použijte příkaz **CREATE USER** udělit oprávnění na úrovni databáze. Všechny přihlášení je vytvořit v **hlavní** databáze. Pro přihlášení se připojit k jiné databázi je třeba udělit ho oprávnění na úrovni databáze pomocí příkazu **Vytvořit uživatele** na tuto databázi. Další informace najdete v tématu [Vytvoření uživatele (databáze SQL)](https://msdn.microsoft.com/library/ms173463.aspx). 

-   Pokud chcete udělit oprávnění login1 k databázi s názvem **myTestDB**, proveďte následující postup:

 1.  Abyste mohli aktualizovat Průzkumník objektů zobrazíte **myTestDB** databáze, kterou jste vytvořili, klikněte pravým tlačítkem myši na název serveru v prohlížeči objektů a klikněte na **Aktualizovat**.  

     Pokud jste zavřeli připojení, se můžete připojit tak, že vyberete **Připojení Průzkumník objektů** v nabídce Soubor.

 2. Klikněte pravým tlačítkem myši **myTestDB** databáze a vyberte **Nový dotaz**.

    3.  Spusťte následující příkaz databázi myTestDB k vytvoření databáze uživatele s názvem **login1User** , který odpovídá úrovni serveru přihlášení **login1**.

            CREATE USER login1User FROM LOGIN login1;

-   Použití **sp\_addrolemember** uložené procedury a udělte uživatelský účet na přijatelnou úroveň oprávnění k databázi. Další informace najdete v tématu [sp_addrolemember jazyce Transact-SQL ()](http://msdn.microsoft.com/library/ms187750.aspx). Následující příkaz poskytuje **login1User** jen pro čtení oprávnění k databázi přidáním **login1User** k **db\_datareader** role.

        exec sp_addrolemember 'db_datareader', 'login1User';    

-   Chcete-li upravit existující přihlášení, například pokud chcete změnit heslo pro přihlášení použijte příkaz **Změnit PŘIHLAŠOVACÍ** . Další informace najdete v tématu [Změnit PŘIHLAŠOVACÍ (databáze SQL)](https://msdn.microsoft.com/library/ms189828.aspx). Příkaz **ALTER přihlášení** by měla běžet s **hlavní** databází. Přepněte se zpátky do okna dotazu, který je připojen k této databázi. Následující příkaz změní **login1** přihlášení k resetování hesla. Nahraďte **nové_heslo** heslo volba a **Původní_heslo** s aktuální heslo pro přihlášení.

        ALTER LOGIN login1
        WITH PASSWORD = 'newPassword'
        OLD_PASSWORD = 'oldPassword';

-   Odstranění existující přihlášení pomocí **PŘETÁHNOUT PŘIHLAŠOVACÍCH** údajů. Odstranění přihlášení na úrovni serveru odstraněny také všechny přidružené databáze uživatelské účty. Další informace najdete v tématu [DROP DATABASE (databáze SQL)](https://msdn.microsoft.com/library/ms178613.aspx). **Uvolnění PŘIHLAŠOVACÍCH** údajů by měla běžet s **hlavní** databází. Příkaz odstraní **login1** přihlášení.

        DROP LOGIN login1;

-   Hlavní databáze obsahuje **sys.sql\_přihlášení** zobrazení, můžete použít k zobrazení přihlášení. Pokud chcete zobrazit všechny existující přihlášení, spusťte následující příkaz:

        SELECT * FROM sys.sql_logins;

## <a name="monitor-sql-database-using-dynamic-management-views"></a>Sledovat pomocí dynamického zobrazení pro správu databáze SQL

Databáze SQL podporuje několik Správa dynamických zobrazení, které slouží ke sledování jednotlivé databáze. Několik příkladů typu dat monitoru, kterou můžete získat prostřednictvím těchto zobrazení sledují. Úplné podrobnosti a další příklady použití najdete v tématu [Sledování databáze SQL pomocí dynamická správa zobrazení](https://msdn.microsoft.com/library/azure/ff394114.aspx).

-   Dotazování Správa dynamických zobrazení vyžaduje oprávnění **Stav databáze zobrazení** . Pokud chcete udělit oprávnění **Stav databáze zobrazit** konkrétní databázi uživateli, připojení k databázi a spusťte následující příkaz v databázi:

        GRANT VIEW DATABASE STATE TO login1User;

-   Výpočet pomocí velikost databáze **sys.dm\_db\_oddíl\_stat** zobrazení. **Sys.dm\_db\_oddíl\_stat** zobrazení vrátí informace o každého oddílu stránky a počet řádků v databázi, která slouží k výpočtu velikost databáze. Následující dotaz vrátí velikost databáze v megabajtech:

        SELECT SUM(reserved_page_count)*8.0/1024
        FROM sys.dm_db_partition_stats;   

-   Použití **sys.dm\_spouštění\_připojení** a **sys.dm\_spouštění\_relací** zobrazení získat informace o aktuální připojení uživatelů a interní úkoly spojené s databází. Následující dotaz vrátí informace o aktuální připojení:

        SELECT
            e.connection_id,
            s.session_id,
            s.login_name,
            s.last_request_end_time,
            s.cpu_time
        FROM
            sys.dm_exec_sessions s
            INNER JOIN sys.dm_exec_connections e
              ON s.session_id = e.session_id;

-   Použití **sys.dm\_spouštění\_dotazu\_stat** zobrazení k načtení statistiky agregační výkonu cached plány dotazů. Následující dotaz vrátí informace o nejčastější dotazy pět jde řadit podle Průměrná doba využití procesoru.

        SELECT TOP 5 query_stats.query_hash AS "Query Hash",
            SUM(query_stats.total_worker_time), SUM(query_stats.execution_count) AS "Avg CPU Time",
            MIN(query_stats.statement_text) AS "Statement Text"
        FROM
            (SELECT QS.*,
            SUBSTRING(ST.text, (QS.statement_start_offset/2) + 1,
            ((CASE statement_end_offset
                WHEN -1 THEN DATALENGTH(ST.text)
                ELSE QS.statement_end_offset END
                    - QS.statement_start_offset)/2) + 1) AS statement_text
             FROM sys.dm_exec_query_stats AS QS
             CROSS APPLY sys.dm_exec_sql_text(QS.sql_handle) as ST) as query_stats
        GROUP BY query_stats.query_hash
        ORDER BY 2 DESC;
 
 
