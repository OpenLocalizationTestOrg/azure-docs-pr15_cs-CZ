<properties
    pageTitle="Vytváření a správa škálovanou se databáze SQL Azure pomocí pružná úlohy | Micosoft Azure"
    description="Projděte si postup vytvoření a správa úlohy pružná databáze."
    services="sql-database"
    documentationCenter=""
    manager="jhubbard"
    authors="ddove"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.workload="sql-database"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/27/2016"
    ms.author="ddove"/>

# <a name="create-and-manage-scaled-out-azure-sql-databases-using-elastic-jobs-preview"></a>Vytváření a správa škálovanou se databáze SQL Azure pomocí pružná úloh (verze preview)

> [AZURE.SELECTOR]
- [Azure portálu](sql-database-elastic-jobs-create-and-manage.md)
- [Prostředí PowerShell](sql-database-elastic-jobs-powershell.md)


**Pružná databáze úlohy** zjednodušení správy skupin databází spuštěním pro správu operace, například změny schématu, Správa přihlašovacích údajů, aktualizaci dat odkazu, shromažďování dat výkonu nebo kolekce telemetrie klienta (zákazník). Pružná úloh databází není momentálně dostupné prostřednictvím Azure portálem a rutinách Powershellu. Však Azure portálu ploch omezenou funkčnost omezený na spuštění přes všechny databáze ve [fondu pružná databáze (verze preview)](sql-database-elastic-pool.md). Přístup k další funkce a provádění skriptů přes celou skupinu databází včetně kolekce vlastní nebo shard sadu (vytvořené pomocí [pružná databáze klienta knihovny](sql-database-elastic-scale-introduction.md)), najdete v článku [vytváření a správa projektů pomocí Powershellu](sql-database-elastic-jobs-powershell.md). Další informace o projektu najdete v článku [Přehled úloh pružná databáze](sql-database-elastic-jobs-overview.md). 

## <a name="prerequisites"></a>Zjistit předpoklady pro

* Předplatné Azure. Bezplatnou zkušební verzi najdete v článku [bezplatnou zkušební verzi jeden měsíc](https://azure.microsoft.com/pricing/free-trial/).
* Fond pružná databáze. V tématu [o pružná fondů databáze](sql-database-elastic-pool.md)
* Instalace součásti služby úlohy pružná databáze. V tématu [Instalace služby práci pružná databáze](sql-database-elastic-jobs-service-installation.md).

## <a name="creating-jobs"></a>Vytváření úloh

1. Pomocí [Azure portál](https://portal.azure.com)z existujícího projektu fondu pružná databáze, klikněte na **Vytvořit úlohu**.
2. Zadejte uživatelské jméno a heslo správce databáze (vytvořená během instalace úloh) pro databázi řízení úloh (úložišti metadat pro úlohy).

    ![Název projektu, zadejte nebo vložte kód a klikněte na příkaz Spustit][1]
2. V zásuvné **Vytvoření projektu** zadejte název projektu.
3. Zadejte uživatelské jméno a heslo pro připojení k databázím cílové dostatečná oprávnění pro spuštění skriptu proběhla úspěšně.
4. Vložení nebo zadejte skript T-SQL.
5. Klikněte na tlačítko **Uložit** a potom klikněte na **Spustit**.

    ![Vytvoření úlohy a spuštění][5]

## <a name="run-idempotent-jobs"></a>Spuštění idempotent úloh

Když spustíte skript vůči sadě databází, musí být jistotu, že skript idempotent. To znamená skript musíte mít ke spuštění několikrát, i když se nezdařila před ve stavu neúplné. Třeba když skript nepovede, úlohy bude automaticky opakovat, dokud neproběhne úspěšně (v určených mezích, jako opakování použití logických operátorů postupně přestane opakování). Způsob, jak to udělat, je použít klauzuli "Pokud existuje" a odstranit všechny nalezené instanci před vytvořením nového objektu. Příklad je zobrazena zde:

    IF EXISTS (SELECT name FROM sys.indexes
            WHERE name = N'IX_ProductVendor_VendorID')
    DROP INDEX IX_ProductVendor_VendorID ON Purchasing.ProductVendor;
    GO
    CREATE INDEX IX_ProductVendor_VendorID
    ON Purchasing.ProductVendor (VendorID);

Můžete taky použijte klauzuli "IF NOT EXISTS" před vytvořením nové instance:

    IF NOT EXISTS (SELECT name FROM sys.tables WHERE name = 'TestTable')
    BEGIN
     CREATE TABLE TestTable(
      TestTableId INT PRIMARY KEY IDENTITY,
      InsertionTime DATETIME2
     );
    END
    GO

    INSERT INTO TestTable(InsertionTime) VALUES (sysutcdatetime());
    GO

Tento skript pak aktualizujete tabulku vytvořili v předchozích krocích.

    IF NOT EXISTS (SELECT columns.name FROM sys.columns INNER JOIN sys.tables on columns.object_id = tables.object_id WHERE tables.name = 'TestTable' AND columns.name = 'AdditionalInformation')
    BEGIN

    ALTER TABLE TestTable

    ADD AdditionalInformation NVARCHAR(400);
    END
    GO

    INSERT INTO TestTable(InsertionTime, AdditionalInformation) VALUES (sysutcdatetime(), 'test');
    GO


## <a name="checking-job-status"></a>Kontrola stavu úloh

Po zahájení projektu můžete zkontrolovat, že o průběhu.

1. Na stránce fondu pružná databázi klikněte na tlačítko **Spravovat projekty**.

    ![Klikněte na možnost "Spravovat projekty"][2]

2. Klikněte na název (a) projektu. **Stav** může být "Dokončeno" nebo "Se nezdařila." Podrobnosti projektu se zobrazí (b) s jejího data a času vytvoření a spuštění. V seznamu (c), který zobrazuje průběh skript vůči každou databázi do fondu, včetně jejího data a času podrobností.

    ![Kontrola dokončení projektu][3]


## <a name="checking-failed-jobs"></a>Kontrola úlohy se nezdařila.

Pokud se nezdaří, úlohy Protokol spuštění můžete najít. Klikněte na název neúspěšná úloha zobrazíte její podrobnosti.

![Zkontrolujte chyby úlohy][4]


[AZURE.INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

<!--Image references-->
[1]: ./media/sql-database-elastic-jobs-create-and-manage/screen-1.png
[2]: ./media/sql-database-elastic-jobs-create-and-manage/click-manage-jobs.png
[3]: ./media/sql-database-elastic-jobs-create-and-manage/running-jobs.png
[4]: ./media/sql-database-elastic-jobs-create-and-manage/failed.png
[5]: ./media/sql-database-elastic-jobs-create-and-manage/screen-2.png

 
