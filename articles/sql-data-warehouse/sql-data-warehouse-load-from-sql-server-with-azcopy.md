<properties
   pageTitle="Načtení dat ze serveru SQL Server do Azure SQL datový sklad (PolyBase) | Microsoft Azure"
   description="Export dat z SQL serveru k plochému souborům, AZCopy k importu dat k úložišti objektů blob Azure a PolyBase jedí data do SQL Azure datový sklad používá bcp."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="ckarst"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="06/30/2016"
   ms.author="cakarst;barbkess;sonyama"/>


# <a name="load-data-from-sql-server-into-azure-sql-data-warehouse-azcopy"></a>Načtení dat ze serveru SQL Server do Azure SQL datový sklad (AZCopy)

Načtení dat z SQL serveru k úložišti objektů blob Azure pomocí bcp a nástroje AZCopy příkazového řádku. Pomocí PolyBase nebo Azure Data Factory data načítáte do datový sklad SQL Azure. 


## <a name="prerequisites"></a>Zjistit předpoklady pro

Můžete přecházet mezi tohoto kurzu, budete potřebovat:

- Databáze SQL datový sklad
- Nástroj příkazového řádku bcp nainstalovaný
- Nástroj SQLCMD příkazového řádku pro nainstalovaný

>[AZURE.NOTE] Nástroje bcp a sqlcmd si můžete stáhnout z [Webu služby Stažení softwaru][].

## <a name="import-data-into-sql-data-warehouse"></a>Import dat do SQL datový sklad

V tomto kurzu vytvoříte tabulku v Azure SQL datový sklad a import dat do tabulky.

### <a name="step-1-create-a-table-in-azure-sql-data-warehouse"></a>Krok 1: Vytvoření tabulky v Azure SQL datový sklad

Na příkazovém řádku umožňuje sqlcmd spuštěním následujícího dotazu vytvořte tabulku na instanci aplikace:

```sql
sqlcmd.exe -S <server name> -d <database name> -U <username> -P <password> -I -Q "
    CREATE TABLE DimDate2
    (
        DateId INT NOT NULL,
        CalendarQuarter TINYINT NOT NULL,
        FiscalQuarter TINYINT NOT NULL
    )
    WITH
    (
        CLUSTERED COLUMNSTORE INDEX,
        DISTRIBUTION = ROUND_ROBIN
    );
"
```

>[AZURE.NOTE] Další informace o vytvoření tabulky na SQL datový sklad a možnosti dostupné v klauzuli s najdete v článku [Přehled tabulky][] nebo [Syntaxe vytvořit tabulku][] .

### <a name="step-2-create-a-source-data-file"></a>Krok 2: Vytvoření souboru zdroje dat

Otevřete Poznámkový blok a zkopírujte následující řádky dat do nového textového souboru a pak tento soubor uložit do vaší místní adresář temp, C:\Temp\DimDate2.txt.

```
20150301,1,3
20150501,2,4
20151001,4,2
20150201,1,3
20151201,4,2
20150801,3,1
20150601,2,4
20151101,4,2
20150401,2,4
20150701,3,1
20150901,3,1
20150101,1,3
```

> [AZURE.NOTE] Je důležité si zapamatovat, že tento bcp.exe nepodporuje kódování UTF-8 soubor. Stiskněte klávesovou zkratku ASCII soubory nebo UTF-16 při použití bcp.exe.

### <a name="step-3-connect-and-import-the-data"></a>Krok 3: Připojení a import dat
Použití bcp, můžete připojení a import dat pomocí následujícího příkazu nahrazení hodnot podle potřeby:

```sql
bcp DimDate2 in C:\Temp\DimDate2.txt -S <Server Name> -d <Database Name> -U <Username> -P <password> -q -c -t  ','
```

Můžete ověřit, jestli že byla spuštěním následujícího dotazu pomocí sqlcmd načtení dat:

```sql
sqlcmd.exe -S <server name> -d <database name> -U <username> -P <password> -I -Q "SELECT * FROM DimDate2 ORDER BY 1;"
```

To, měly vrátit následující výsledky:

DateId |CalendarQuarter |FiscalQuarter
----------- |--------------- |-------------
20150101 |1 |3
20150201 |1 |3
20150301 |1 |3
20150401 |2 |4
20150501 |2 |4
20150601 |2 |4
20150701 |3 |1
20150801 |3 |1
20150801 |3 |1
20151001 |4 |2
20151101 |4 |2
20151201 |4 |2

### <a name="step-4-create-statistics-on-your-newly-loaded-data"></a>Krok 4: Vytvoření statistiky na nově načtení dat

Azure SQL datový sklad znamená dosud podpory automaticky vytvořit nebo automatické aktualizace statistiky. Abyste mohli získávat optimálního výkonu z dotazech, je důležité statistiky být vytvořené ve všech sloupcích všech tabulek po první načtení nebo nastanou jakékoliv podstatné změny data. Podrobné vysvětlení statistiky naleznete v tématu [statistiky][] ve skupině vývoje témat. Tady je rychlý příklad toho, jak vytvořit statistik tabulkovém načtené v tomto příkladu

Spusťte následující příkazy vytvořit statistiky sqlcmd řádku:

```sql
sqlcmd.exe -S <server name> -d <database name> -U <username> -P <password> -I -Q "
    create statistics [DateId] on [DimDate2] ([DateId]);
    create statistics [CalendarQuarter] on [DimDate2] ([CalendarQuarter]);
    create statistics [FiscalQuarter] on [DimDate2] ([FiscalQuarter]);
"
```

## <a name="export-data-from-sql-data-warehouse"></a>Export dat z SQL datový sklad
V tomto kurzu vytvoříte datového souboru z tabulky SQL datový sklad. Do nového datového souboru s názvem DimDate2_export.txt jsme bude export dat, kterou jsme vytvořili nad.

### <a name="step-1-export-the-data"></a>Krok 1: Export dat

Pomocí nástroje bcp, se můžete připojit a exportovat data pomocí následujícího příkazu nahrazení hodnot podle potřeby:

```sql
bcp DimDate2 out C:\Temp\DimDate2_export.txt -S <Server Name> -d <Database Name> -U <Username> -P <password> -q -c -t ','
```
Můžete ověřit, že data exportován správně otevřením nový soubor. Data v souboru by měly odpovídat následující text:

```
20150301,1,3
20150501,2,4
20151001,4,2
20150201,1,3
20151201,4,2
20150801,3,1
20150601,2,4
20151101,4,2
20150401,2,4
20150701,3,1
20150901,3,1
20150101,1,3
```

>[AZURE.NOTE] Z důvodu přírodní distribuované systémy nemusí být pořadí dat přes databáze SQL datový sklad stejné. Další možností je použít funkci **parametrem QUERYOUT** bcp psaní extrahovat dotazu, spíše než exportovat celou tabulku.

## <a name="next-steps"></a>Další kroky
Přehled načítání naleznete v tématu [načíst data do SQL datový sklad][].
Další tipy vývoj najdete v tématu [Přehled vývoje SQL datový sklad][].

<!--Image references-->

<!--Article references-->

[Načíst data do SQL datový sklad]: ./sql-data-warehouse-overview-load.md
[Přehled vývoje SQL datový sklad]: ./sql-data-warehouse-overview-develop.md
[Přehled tabulky]: ./sql-data-warehouse-tables-overview.md
[Statistiky]: ./sql-data-warehouse-tables-statistics.md

<!--MSDN references-->
[bcp]: https://msdn.microsoft.com/library/ms162802.aspx
[Syntaxe vytvořit tabulku]: https://msdn.microsoft.com/library/mt203953.aspx

<!--Other Web references-->
[Stažení softwaru společnosti Microsoft]: https://www.microsoft.com/download/details.aspx?id=36433
