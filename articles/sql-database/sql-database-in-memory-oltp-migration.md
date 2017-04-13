<properties
    pageTitle="V paměti OLTP zlepšuje výkon txn SQL | Microsoft Azure"
    description="Přijmout v paměti OLTP transakční zlepšili v existující databázi SQL."
    services="sql-database"
    documentationCenter=""
    authors="jodebrui"
    manager="jhubbard"
    editor="MightyPen"/>


<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/03/2016"
    ms.author="jodebrui"/>


# <a name="use-in-memory-oltp-preview-to-improve-your-application-performance-in-sql-database"></a>Použití v paměti OLTP (verze preview) pro zvýšení výkonu aplikace v databázi SQL

[V paměti OLTP](sql-database-in-memory.md) mohou sloužit k vylepšení výkonu pracovního vytížení OLTP v databázích SQL Azure [Premium](sql-database-service-tiers.md) bez zvýšení výkonu úroveň.

Tímto postupem přijmout OLTP v paměti v existující databázi.

## <a name="step-1-ensure-your-premium-database-supports-in-memory-oltp"></a>Krok 1: Zajistěte, aby že databázi Premium podporuje OLTP v paměti

Premium databáze vytvořené v listopadu 2015 nebo novější podporuje funkci v paměti. Můžete zjistit, zda databáze Premium podporuje funkci v paměti spuštěním následující příkaz Transact-SQL. V paměti je podporovaná, pokud vrácený výsledek je 1 (ne 0):

```
SELECT DatabasePropertyEx(Db_Name(), 'IsXTPSupported');
```

*XTP* zastupuje *Krajní zpracování transakce*

Stávající databázi musí přesunete do nové databáze V12 Premium, můžete použít následující postupy exportovat a importovat data.

#### <a name="export-steps"></a>Kroky exportu

Exportujte provozní databáze do bacpac pomocí:

- [Export](sql-database-export.md) funkci na [portálu](https://portal.azure.com/).

- Funkce **aplikace datové vrstvy Export** v [aktuální SSMS.exe](http://msdn.microsoft.com/library/mt238290.aspx) (SQL Server Management Studio).
 1. V okně **Průzkumník objektů**rozbalte uzel **databáze** .
 2. Klikněte pravým tlačítkem na uzel databáze.
 3. Klikněte na **úkoly** > **Exportovat Data osy aplikaci**.
 4. Ovládání, která se zobrazí okno průvodce.


#### <a name="import-steps"></a>Kroky importu

Importujte bacpac do nové databáze Premium.

1. Azure [portálu](https://portal.azure.com/)
 - Přejděte na server.
 - Vyberte možnost [Importovat databáze](sql-database-import.md) .
 - Vyberte Premium ceny osy.

2. Umožňuje importovat bacpac SSMS:
 - V **Prohlížeči objektů**klikněte pravým tlačítkem myši na uzel **databází** .
 - Klikněte na **Import osy dat aplikace**.
 - Ovládání, která se zobrazí okno průvodce.


## <a name="step-2-identify-objects-to-migrate-to-in-memory-oltp"></a>Krok 2: Určení objekty migrovat do OLTP v paměti

SSMS obsahuje **Přehled Analýza výkonu transakce** sestavy, jehož spuštěním se aktivní pracovní zátěž v databázi. Sestava identifikuje tabulek a uložené procedury, které jsou kandidáty pro migraci do OLTP v paměti.

V SSMS, abyste vygenerovali sestavu:
- V **Prohlížeči objektů**klikněte pravým tlačítkem myši uzel databáze.
- Klikněte na **sestavy** > **Standardní sestavy** > **Přehled Analýza výkonu transakce**.

Další informace najdete v tématu [určení u tabulek a uložené procedury by měl přenést do OLTP v paměti](http://msdn.microsoft.com/library/dn205133.aspx).


## <a name="step-3-create-a-comparable-test-database"></a>Krok 3: Vytvoření srovnatelná test databáze

Předpokládejme, že sestavu označuje, že databáze obsahuje tabulku, která byste měli využívat převedena na tabulku optimalizované paměti. Doporučujeme nejdřív otestovat potvrďte označení testováním.

Potřebujete zkušební kopii databáze výroby. Test databáze musí být na stejné úrovni služby osy jako provozní databáze.

Usnadnit testování doladit test databáze následujícím způsobem:

1. Připojení k databázi test pomocí SSMS.

2. Chcete-li předejít nutností možnost s (SNÍMEK) v dotazech, nastavte možnost databáze, jak ukazuje následující příkaz T SQL:
```
ALTER DATABASE CURRENT
    SET
        MEMORY_OPTIMIZED_ELEVATE_TO_SNAPSHOT = ON;
```


## <a name="step-4-migrate-tables"></a>Krok 4: Migrace tabulek

Vytvořte a naplnění paměť optimalizována kopii tabulku, kterou chcete testovat. Můžete ji vytvořit pomocí:

- Po ruce Průvodce optimalizace paměti v SSMS.
- Ruční T-SQL.


#### <a name="memory-optimization-wizard-in-ssms"></a>Průvodce optimalizace paměti v SSMS

Tuto možnost migrace použít:

1. Připojení k databázi test s SSMS.

2. V **Prohlížeči objektů**klikněte pravým tlačítkem myši na tabulku a klikněte **Advisor optimalizace paměti**.
 - Zobrazí se průvodce **Advisor optimalizace paměti tabulky** .

3. V průvodci klikněte **migrace ověření** (nebo na tlačítko **Další** ) Pokud chcete zobrazit, pokud tabulka obsahuje nepodporované funkce, které nejsou podporovány v paměti optimalizované tabulek. Další informace najdete v tématu:
 - *Kontrolní seznam optimalizace paměti* v [Advisor optimalizace paměti](http://msdn.microsoft.com/library/dn284308.aspx).
 - [Konstrukce transact-SQL nejsou podporovány v paměti OLTP](http://msdn.microsoft.com/library/dn246937.aspx).
 - [Migrace na OLTP v paměti](http://msdn.microsoft.com/library/dn247639.aspx).

4. Pokud tabulka obsahuje nepodporované funkce, poradce můžete provést skutečné schématu a migraci dat.


#### <a name="manual-t-sql"></a>Ruční T-SQL

Tuto možnost migrace použít:

1. Připojení k databázi test pomocí SSMS (nebo podobné nástroj).

2. Získáte celý skript T-SQL pro tabulky a jeho indexy.
 - V SSMS klikněte pravým tlačítkem uzel tabulky.
 - Klikněte na **tabulku skript jako** > **vytvořit** > **nové okno dotazu**.

3. V okně skript přidat WITH (MEMORY_OPTIMIZED = zapnuto) na příkaz CREATE TABLE.

4. Pokud není SESKUPENÝ index, můžete jej změňte na NONCLUSTERED.

5. Přejmenování existující tabulky pomocí SP_RENAME.

6. Vytvořte novou paměť optimalizována kopii tabulky spuštěním upravený skript vytvořit tabulku.

7. Zkopírujte data do tabulky paměť optimalizována pomocí příkazu Vložit... VYBERTE * DO:
    
```
INSERT INTO <new_memory_optimized_table>
        SELECT * FROM <old_disk_based_table>;
```


## <a name="step-5-optional-migrate-stored-procedures"></a>Krok 5 (nepovinné): migrace uložené procedury

Funkce v paměti můžete také změnit uložené procedury pro zvýšení výkonu.


### <a name="considerations-with-natively-compiled-stored-procedures"></a>Důležité údaje související s nativně zkompilované uložené procedury

Nativně zkompilované uložená procedura musí mít následující možnosti na jeho T-SQL s klauzule:

- NATIVE_COMPILATION

- SCHEMABINDING: znamená tabulek, které uložená procedura nesmí obsahovat jejich sloupec definice změnit žádným způsobem, který by mohly ovlivnit uložená procedura, pokud přetáhnout uložená procedura.


Nativní modulu musíte použít jeden velký [ATOMOVÁ bloky](http://msdn.microsoft.com/library/dn452281.aspx) správy transakce. Je žádné role explicitní začít transakce nebo ODVOLAT TRANSAKCI. Pokud váš kód zjistí porušení obchodního pravidla, jej ukončit atomová blok s příkazem [VYVOLAT](http://msdn.microsoft.com/library/ee677615.aspx) .


### <a name="typical-create-procedure-for-natively-compiled"></a>Typický postup vytvoření pro nativně kompilovaný

Obvykle T-SQL vytvořit nativně zkompilované uložená procedura podobá následující šablonu:

```
CREATE PROCEDURE schemaname.procedurename
    @param1 type1, …
    WITH NATIVE_COMPILATION, SCHEMABINDING
    AS
        BEGIN ATOMIC WITH
            (TRANSACTION ISOLATION LEVEL = SNAPSHOT,
            LANGUAGE = N'your_language__see_sys.languages'
            )
        …
        END;
```

- Pro TRANSACTION_ISOLATION_LEVEL SNÍMEK hodnotu nejběžnější nativně zkompilované uložená procedura. Však podmnožinu jiné hodnoty jsou taky podporované:
 - OPAKUJÍCÍ PRO ČTENÍ
 - SERIALIZOVATELNÝ


- V zobrazení sys.languages musí být hodnota jazyk.


### <a name="how-to-migrate-a-stored-procedure"></a>Jak migrovat uložená procedura

Migrace se:


1. Získáte skript CREATE PROCEDURE běžná interpretovaný uložené procedury.

2. Revize jeho záhlaví podle předchozího šablony.

3. Zjistit, zda uložená procedura kód T SQL používá funkce, které nejsou podporovány nativně zkompilované uložené procedury. Implementace řešení v případě potřeby.
 - Podrobnosti najdete v článku [Migrace problémů nativně kompilovaný uložené procedury](http://msdn.microsoft.com/library/dn296678.aspx).

4. Přejmenování staré uložená procedura pomocí SP_RENAME. Nebo jednoduše PŘETÁHNOUT.

5. Spusťte upravený skript vytvořit postup T-SQL.


## <a name="step-6-run-your-workload-in-test"></a>Krok 6: Spuštění svého pracovního vytížení při zkoušce

Spuštění pracovního vytížení v testu databázi, která se podobá pracovní zátěž, který běží výrobní databáze. To by měly odhalit použitím funkce v paměti pro tabulky a uložené procedury dosáhnout zvýšení výkonu.

Hlavní atributy pracovní zátěž jsou:

- Počet souběžné připojení.

- Poměr pro čtení i zápis.


Přizpůsobení a spustit pracovní zátěž test, zvažte použití nástroje po ruce ostress.exe znázorněno [zde](sql-database-in-memory.md).


Minimalizovat latence sítě, spusťte test ve stejné Azure zeměpisná oblast místo, kam databáze existuje.


## <a name="step-7-post-implementation-monitoring"></a>Krok 7: Po provedení sledování

Vezměte v úvahu sledování výkonu projevů implementaci v paměti v výrobní:

- [Sledování v paměti úložiště](sql-database-in-memory-oltp-monitoring.md).

- [Sledování databáze SQL Azure pomocí Správa dynamických zobrazení](sql-database-monitoring-with-dmvs.md)


## <a name="related-links"></a>Související odkazy

- [Paměť OLTP (Optimalizace v paměti)](http://msdn.microsoft.com/library/dn133186.aspx)

- [Úvod k nativně zkompilované uložené procedury](http://msdn.microsoft.com/library/dn133184.aspx)

- [Konference Advisor optimalizace paměti](http://msdn.microsoft.com/library/dn284308.aspx)

