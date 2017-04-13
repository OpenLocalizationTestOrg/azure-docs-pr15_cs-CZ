<properties
   pageTitle="Načtení dat ze souboru CSV do Databaase SQL Azure (bcp) | Microsoft Azure"
   description="Velikost malé dat používá bcp k importu dat do databáze SQL Azure."
   services="sql-database"
   documentationCenter="NA"
   authors="CarlRabeler"
   manager="jhubbard"
   editor=""/>

<tags
   ms.service="sql-database"
   ms.devlang="NA"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="09/13/2016"
   ms.author="carlrab"/>


# <a name="load-data-from-csv-into-azure-sql-data-warehouse-flat-files"></a>Načtení dat ze souboru CSV do Azure SQL datový sklad (textových souborů)

Nástroj příkazového řádku bcp umožňuje import dat ze souboru CSV do databáze SQL Azure.

## <a name="before-you-begin"></a>Než začnete

### <a name="prerequisites"></a>Zjistit předpoklady pro

Můžete přecházet mezi tohoto kurzu, budete potřebovat:

- Databázový server Azure SQL logické a databáze
- Nástroj příkazového řádku bcp nainstalovaný
- Nástroj příkazového řádku sqlcmd nainstalovaný

Nástroje bcp a sqlcmd si můžete stáhnout z [Webu služby Stažení softwaru][].

### <a name="data-in-ascii-or-utf-16-format"></a>Data ve formátu ASCII nebo UTF-16

Pokud se pokoušíte tento kurz na vlastních datech, datům musí používat ASCII nebo UTF-16 kódování od bcp nepodporuje UTF-8. 

## <a name="1-create-a-destination-table"></a>1. cílovou tabulku vytvořit

Definice tabulky databáze SQL jako cílovou tabulku. Sloupce v tabulce musí odpovídat data v každém řádku datového souboru aplikace.

Pokud chcete vytvořit tabulku, otevřete příkazový řádek a pomocí sqlcmd.exe spusťte tento příkaz:


```sql
sqlcmd.exe -S <server name> -d <database name> -U <username> -P <password> -I -Q "
    CREATE TABLE DimDate2
    (
        DateId INT NOT NULL,
        CalendarQuarter TINYINT NOT NULL,
        FiscalQuarter TINYINT NOT NULL
    )
    ;
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


## <a name="next-steps"></a>Další kroky

Migrace databáze SQL serveru, najdete v článku [Migrace databáze SQL serveru](sql-database-cloud-migrate.md).

<!--MSDN references-->
[bcp]: https://msdn.microsoft.com/library/ms162802.aspx
[CREATE TABLE syntax]: https://msdn.microsoft.com/library/mt203953.aspx

<!--Other Web references-->
[Stažení softwaru společnosti Microsoft]: https://www.microsoft.com/download/details.aspx?id=36433
