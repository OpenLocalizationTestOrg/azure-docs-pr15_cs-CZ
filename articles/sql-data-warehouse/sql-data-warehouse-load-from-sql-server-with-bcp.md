<properties
   pageTitle="Načtení dat ze serveru SQL Server do Azure SQL datový sklad (bcp) | Microsoft Azure"
   description="Velikost malé dat používá bcp exportovat data ze serveru SQL Server textových souborů a data importovat přímo do datový sklad SQL Azure."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="lodipalm"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="06/30/2016"
   ms.author="lodipalm;barbkess;sonyama"/>


# <a name="load-data-from-sql-server-into-azure-sql-data-warehouse-flat-files"></a>Načtení dat ze serveru SQL Server do Azure SQL datový sklad (textových souborů)

> [AZURE.SELECTOR]
- [SSIS](sql-data-warehouse-load-from-sql-server-with-integration-services.md)
- [PolyBase](sql-data-warehouse-load-from-sql-server-with-polybase.md)
- [BCP](sql-data-warehouse-load-from-sql-server-with-bcp.md)

Pro malé uvedené množiny dat můžete nástroj bcp příkazového řádku pro export dat z SQL serveru a načíst jej přímo do Azure SQL datový sklad.

V tomto kurzu použijete bcp na:

- Pomocí bcp příkazu Exportovat tabulku z ze serveru SQL Server (nebo vytvořit jednoduchý ukázkový soubor)
- Importujte tabulky z plochému souboru SQL datový sklad.
- Načtení dat vytvořte statistiky.

>[AZURE.VIDEO loading-data-into-azure-sql-data-warehouse-with-bcp]

## <a name="before-you-begin"></a>Než začnete

### <a name="prerequisites"></a>Zjistit předpoklady pro

Můžete přecházet mezi tohoto kurzu, budete potřebovat:

- Databáze SQL datový sklad
- Nástroj příkazového řádku bcp nainstalovaný
- Nástroj příkazového řádku sqlcmd nainstalovaný

Nástroje bcp a sqlcmd si můžete stáhnout z [Webu služby Stažení softwaru][].

### <a name="data-in-ascii-or-utf-16-format"></a>Data ve formátu ASCII nebo UTF-16

Pokud se pokoušíte tento kurz na vlastních datech, datům musí používat ASCII nebo UTF-16 kódování od bcp nepodporuje UTF-8. 

PolyBase podporuje UTF-8, ale zatím nepodporuje UTF-16. Poznámka: Pokud chcete zahrnout do sloučeného bcp s PolyBase bude muset transformace dat na UTF-8 po exportu ze serveru SQL Server. 


## <a name="1-create-a-destination-table"></a>1. cílovou tabulku vytvořit

Definování tabulky v SQL datový sklad, který bude obsahovat cílové tabulce načíst. Sloupce v tabulce musí odpovídat data v každém řádku datového souboru aplikace.

Pokud chcete vytvořit tabulku, otevřete příkazový řádek a pomocí sqlcmd.exe spusťte tento příkaz:


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


## <a name="2-create-a-source-data-file"></a>2. vytvoření souboru zdroje dat

Otevřete Poznámkový blok a zkopírujte následující řádky dat do nového textového souboru a pak tento soubor uložit do vaší místní adresář temp, C:\Temp\DimDate2.txt. Tato data jsou ve formátu ASCII.

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

(Volitelné) Export vlastních dat z databáze SQL serveru, otevřete příkazový řádek a spusťte tento příkaz. Nahraďte název tabulky webu název serveru, název databáze, uživatelské jméno a heslo vlastními informacemi.

```sql
bcp <TableName> out C:\Temp\DimDate2_export.txt -S <ServerName> -d <DatabaseName> -U <Username> -P <Password> -q -c -t ','
```



## <a name="3-load-the-data"></a>3. data načítáte
Načtení dat, otevřete příkazový řádek a spusťte tento příkaz nahradit hodnoty název serveru, název databáze, uživatelské jméno a heslo vlastními informacemi.

```sql
bcp DimDate2 in C:\Temp\DimDate2.txt -S <ServerName> -d <DatabaseName> -U <Username> -P <password> -q -c -t  ','
```

Pomocí tohoto příkazu můžete ověřit, jestli že je správně načtení dat

```sql
sqlcmd.exe -S <server name> -d <database name> -U <username> -P <password> -I -Q "SELECT * FROM DimDate2 ORDER BY 1;"
```

Výsledky by měl vypadat takto:

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

## <a name="4-create-statistics"></a>4. vytvořit statistiky

SQL datový sklad znamená dosud podporu automatické vytváření nebo automaticky aktualizován statistiky. Získat optimálního výkonu dotazu, je důležité vytvořit statistiky ve všech sloupcích všech tabulek po první načtení nebo když nastanou jakékoliv podstatné změny data. Podrobné vysvětlení statistických údajů najdete v článku [statistiky][]. 

Spusťte tento příkaz vytvořit statistiky na nově načtení tabulky.

```sql
sqlcmd.exe -S <server name> -d <database name> -U <username> -P <password> -I -Q "
    create statistics [DateId] on [DimDate2] ([DateId]);
    create statistics [CalendarQuarter] on [DimDate2] ([CalendarQuarter]);
    create statistics [FiscalQuarter] on [DimDate2] ([FiscalQuarter]);
"
```

## <a name="5-export-data-from-sql-data-warehouse"></a>5. exportovat data z SQL datový sklad
Pro zábavy můžete exportovat data, který jste právě zavedli zpět mimo SQL datový sklad.  Příkaz Exportovat je přesně totéž jako export ze serveru SQL Server.

Je ale rozdíl ve výsledcích. Protože jsou data uložena distribuované místa v SQL datový sklad při exportu dat jednotlivých uzlech výpočetním zapíše ji dat ve výstupním souboru. Pořadí dat ve výstupním souboru je pravděpodobně se liší od pořadí dat ve vstupním souboru.

### <a name="export-a-table-and-compare-exported-results"></a>Export tabulky a porovnání exportovaného výsledků

Exportovaná data zobrazíte otevřete příkazový řádek a zadejte tento příkaz pomocí vlastní parametrů. Název serveru SQL Azure logické je webu název serveru.

```sql
bcp DimDate2 out C:\Temp\DimDate2_export.txt -S <Server Name> -d <Database Name> -U <Username> -P <password> -q -c -t ','
```
Můžete ověřit, že data exportován správně otevřením nový soubor. Data v souboru by měly odpovídat pod text, ale bude pravděpodobně seřazené v jiném pořadí:

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

### <a name="export-the-results-of-a-query"></a>Příkaz Exportovat výsledky dotazu

Funkce **parametrem QUERYOUT** bcp slouží k exportu výsledků dotazu místo exportu celou tabulku. 

## <a name="next-steps"></a>Další kroky
Přehled načítání naleznete v tématu [načíst data do SQL datový sklad][].
Další tipy vývoj najdete v tématu [Přehled vývoje SQL datový sklad][].
Další informace o vytvoření tabulky na SQL datový sklad najdete v článku [Přehled tabulky][] nebo [Syntaxe vytvořit tabulku][] .

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
