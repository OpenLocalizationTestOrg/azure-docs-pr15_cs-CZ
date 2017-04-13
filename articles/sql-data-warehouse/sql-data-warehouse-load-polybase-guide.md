<properties
   pageTitle="Příručka pro použití PolyBase v SQL datový sklad | Microsoft Azure"
   description="Pokyny a doporučení pro použití PolyBase v SQL datový sklad scénáře."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="ckarst"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="06/30/2016"
   ms.author="cakarst;barbkess;sonyama"/>


# <a name="guide-for-using-polybase-in-sql-data-warehouse"></a>Příručka pro použití PolyBase v SQL datový sklad

Tato příručka nabízí praktické informace o používání PolyBase v SQL datový sklad.

Začít pracovat, najdete v tématu kurz [načtení dat s PolyBase][] .


## <a name="rotating-storage-keys"></a>Otočení úložiště klíče

Občas můžete chtít změnit přístupové klávesy k úložišti objektů blob z bezpečnostních důvodů.

Většina elegantní způsobem k provedení tohoto úkolu je projděte si proces jmenoval "otočení klávesy". Jste si všimli, že máte dva klíče úložiště účtu úložiště objektů blob. Toto je tak, aby mohli přechodu

Otáčení kódu Key účet Azure úložiště je jednoduchý třech krocích

1. Vytvoření druhého databáze omezené pověření založené na sekundárním úložiště přístupová klávesa
2. Vytvoření druhého externímu zdroji dat na základě vypnout toto nové přihlašovací údaje
3. Uvolnění a vytvořte externí tabulky přejdete novému externímu zdroji dat

Když jste přešli v externí tabulky na nový zdroj externích dat a pak můžete provést vyčištění úkoly:

1. Uvolnění prvního externího zdroje dat
2. První databáze v rozevírací rozsah podle přístupová klávesa primární úložiště přihlašovacích údajů
3. Přihlaste se k Azure a obnovit primární přístupová klávesa jste připraveni na příště

## <a name="query-azure-blob-storage-data"></a>Úložiště objektů blob Azure dotaz na data
Dotazy týkající se externí tabulky jednoduše použít název tabulky, jako by bylo relační tabulky.

```sql
-- Query Azure storage resident data via external table.
SELECT * FROM [ext].[CarSensor_Data]
;
```

> [AZURE.NOTE] Zobrazí se chyba *"Při čtení z externího zdroje dotazu přerušené – maximální odmítnout mezní dosáhl"*může selhat dotazu v externí tabulce. Tento údaj označuje, že externích dat obsahuje *dirty* záznamy. Datový záznam je považován za "dirty", pokud skutečnými daty typy/počet sloupců neodpovídají definice sloupců externích tabulky nebo data nesplňuje zadaná externí formátu. Pokud to pokud chcete opravit, zkontrolujte správnost externí tabulka a externí soubor formátu definice a externích dat odpovídá tyto definice. V případě podmnožinu záznamů externích dat jsou chybná, můžete odmítnutí těchto záznamů pro dotazy pomocí možností na odmítnout v vytvořit externí DDL tabulky.


## <a name="load-data-from-azure-blob-storage"></a>Načtení dat z úložiště objektů blob Azure
V tomto příkladu načte data v úložišti objektů blob Azure SQL datový sklad databáze.

Ukládání dat přímo odebere dobu převodu dat pro dotazy. Ukládání dat s indexem columnstore zlepšuje výkon dotazu pro analýzy dotazy podle až 10 x.

Tento příklad používá k načtení dat příkaz Vytvořit SELECT jako tabulku. Novou tabulku dědí sloupce s názvem v dotazu. Datové typy tyto sloupce dědí od definice externí tabulka.

VYTVOŘIT tabulku jako vyberte je vysoce performant jazyka Transact-SQL, která načítá data paralelně do všech výpočetním uzlů datový sklad SQL.  Původně vyvinutý pro modul datových souběžné zpracování (MPP) v systému platformu technologie pro analýzu a je teď v SQL datový sklad.

```sql
-- Load data from Azure blob storage to SQL Data Warehouse

CREATE TABLE [dbo].[Customer_Speed]
WITH
(   
    CLUSTERED COLUMNSTORE INDEX
,   DISTRIBUTION = HASH([CarSensor_Data].[CustomerKey])
)
AS
SELECT *
FROM   [ext].[CarSensor_Data]
;
```

V tématu [Vytvoření tabulky jako vyberte (Transact-SQL)][].

## <a name="create-statistics-on-newly-loaded-data"></a>Vytvoření statistiky nově načtení dat

Azure SQL datový sklad znamená dosud podpory automaticky vytvořit nebo automatické aktualizace statistiky.  Abyste mohli získávat optimálního výkonu z dotazech, je důležité statistiky být vytvořené ve všech sloupcích všech tabulek po první načtení nebo nastanou jakékoliv podstatné změny data.  Podrobné vysvětlení statistiky naleznete v tématu [statistiky][] ve skupině vývoje témat.  Tady je rychlý příklad toho, jak vytvořit statistik tabulkovém načtené v tomto příkladu.

```sql
create statistics [SensorKey] on [Customer_Speed] ([SensorKey]);
create statistics [CustomerKey] on [Customer_Speed] ([CustomerKey]);
create statistics [GeographyKey] on [Customer_Speed] ([GeographyKey]);
create statistics [Speed] on [Customer_Speed] ([Speed]);
create statistics [YearMeasured] on [Customer_Speed] ([YearMeasured]);
```

## <a name="export-data-to-azure-blob-storage"></a>Export dat do úložišti objektů blob Azure
Tato část popisuje exportovat data z SQL datový sklad k úložišti objektů blob Azure. V tomto příkladu vytvořit externí tabulky jako vybrat, ke kterým je důrazně performant jazyka Transact-SQL export požadovaných dat paralelně ze všech výpočetním uzlů.

Následující příklad vytvoří externí tabulku Weblogs2014 pomocí definice sloupce a data z dbo. Tabulka komentář. Definice externí tabulky je uložen v SQL datový sklad a k adresáři "/ archivace/log2014 /" v části kontejneru objektů blob nastavil zdroje dat jsou exportovány výsledky příkazu SELECT. Data exportu ve formátu zadaný text.

```sql
CREATE EXTERNAL TABLE Weblogs2014 WITH
(
    LOCATION='/archive/log2014/',
    DATA_SOURCE=azure_storage,
    FILE_FORMAT=text_file_format
)
AS
SELECT
    Uri,
    DateRequested
FROM
    dbo.Weblogs
WHERE
    1=1
    AND DateRequested > '12/31/2013'
    AND DateRequested < '01/01/2015';
```


## <a name="working-around-the-polybase-utf-8-requirement"></a>Alternativní řešení požadovaného PolyBase UTF-8
Při prezentování PolyBase podporuje načítání datové soubory, které byly kódování UTF-8. UTF-8 používá stejnou kódování znaků jako ASCII PolyBase bude taky podpora načítání dat, která je ASCII kódování. Však PolyBase nepodporuje jiných kódování znaků, například UTF-16 / Unicode nebo prodloužit znaků ASCII. Rozšířená ASCII obsahuje znaky s akcenty například společné německé přehlásky.

Informace k alternativním řešením tohoto požadavku na nejlepší odpověď je znovu zapisovat do kódování UTF-8.

Existuje několik způsobů, jak to udělat. Pod dvěma způsoby pomocí Powershellu:

### <a name="simple-example-for-small-files"></a>Jednoduchý příklad pro malé soubory

Níže je jednoduchý jeden řádek skript Powershellu, který vytvoří soubor.

```PowerShell
Get-Content <input_file_name> -Encoding Unicode | Set-Content <output_file_name> -Encoding utf8
```

Když to je jednoduchý způsob, jak znovu kódovat data je však podle ne prostředky co nejefektivněji. Vstupu a výstupu streamování příklad je velmi, rychleji a dosahuje stejný výsledek.

### <a name="io-streaming-example-for-larger-files"></a>Příklad datových proudů/v pro větší soubory

Následující kód ukázce složitější ale během vysílání řádků dat ze zdroje nastavte cílovou mnohem efektivnější. Tento postup použijte pro větší soubory.

```PowerShell
#Static variables
$ascii = [System.Text.Encoding]::ASCII
$utf16le = [System.Text.Encoding]::Unicode
$utf8 = [System.Text.Encoding]::UTF8
$ansi = [System.Text.Encoding]::Default
$append = $False

#Set source file path and file name
$src = [System.IO.Path]::Combine("C:\input_file_path\","input_file_name.txt")

#Set source file encoding (using list above)
$src_enc = $ansi

#Set target file path and file name
$tgt = [System.IO.Path]::Combine("C:\output_file_path\","output_file_name.txt")

#Set target file encoding (using list above)
$tgt_enc = $utf8

$read = New-Object System.IO.StreamReader($src,$src_enc)
$write = New-Object System.IO.StreamWriter($tgt,$append,$tgt_enc)

while ($read.Peek() -ne -1)
{
    $line = $read.ReadLine();
    $write.WriteLine($line);
}
$read.Close()
$read.Dispose()
$write.Close()
$write.Dispose()
```

## <a name="next-steps"></a>Další kroky
Další informace o přesunutí dat do SQL datový sklad najdete v tématu [Přehled migrace dat][].

<!--Image references-->

<!--Article references-->
[Load data with bcp]: ./sql-data-warehouse-load-with-bcp.md
[Načtení dat s PolyBase]: ./sql-data-warehouse-get-started-load-with-polybase.md
[Statistiky]: ./sql-data-warehouse-tables-statistics.md
[Přehled migrace dat]: ./sql-data-warehouse-overview-migrate.md

<!--MSDN references-->
[supported source/sink]: https://msdn.microsoft.com/library/dn894007.aspx
[copy activity]: https://msdn.microsoft.com/library/dn835035.aspx
[SQL Server destination adapter]: https://msdn.microsoft.com/library/ms141095.aspx
[SSIS]: https://msdn.microsoft.com/library/ms141026.aspx

[CREATE EXTERNAL DATA SOURCE (Transact-SQL)]: https://msdn.microsoft.com/library/dn935022.aspx
[Vytvořit externí formátu (Transact-SQL)]: https://msdn.microsoft.com/library/dn935026) aspx [vytvořit externí tabulku (Transact-SQL)]: https://msdn.microsoft.com/library/dn935021.aspxx

[DROP EXTERNAL DATA SOURCE (Transact-SQL)]: https://msdn.microsoft.com/library/mt146367.aspx
[DROP EXTERNAL FILE FORMAT (Transact-SQL)]: https://msdn.microsoft.com/library/mt146379.aspx
[DROP EXTERNAL TABLE (Transact-SQL)]: https://msdn.microsoft.com/library/mt130698.aspx

[Vytvoření tabulky jako vyberte (Transact-SQL)]: https://msdn.microsoft.com/library/mt204041.aspx
[INSERT...SELECT (Transact-SQL)]: https://msdn.microsoft.com/library/ms174335.aspx
[CREATE MASTER KEY (Transact-SQL)]: https://msdn.microsoft.com/library/ms174382.aspx
[CREATE CREDENTIAL (Transact-SQL)]: https://msdn.microsoft.com/library/ms189522.aspx
[CREATE DATABASE SCOPED CREDENTIAL (Transact-SQL)]: https://msdn.microsoft.com/library/mt270260.aspx
[DROP CREDENTIAL (Transact-SQL)]: https://msdn.microsoft.com/library/ms189450.aspx

<!-- External Links -->
