<properties
   pageTitle="Načtení ukázkových dat do SQL datový sklad | Microsoft Azure"
   description="Načtení ukázkových dat do SQL datový sklad"
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
   ms.date="08/16/2016"
   ms.author="lodipalm;barbkess;sonyama"/>

#<a name="load-sample-data-into-sql-data-warehouse"></a>Načtení ukázkových dat do SQL datový sklad

Tyto kroky jednoduché zavádění a dotaz Adventure Works ukázkovou databázi. Tyto skripty nejdřív pomocí sqlcmd spustit SQL, který vytvoří tabulek a zobrazení. Po vytvoření tabulky skriptů pomocí bcp načíst data.  Pokud ještě nemáte sqlcmd a bcp nainstalovaný, na těchto odkazech nainstalovat [bcp][] a nainstalovat [sqlcmd][].

##<a name="load-sample-data"></a>Načtení ukázkových dat

1. [Ukázky skriptů Adventure Works pro SQL datový sklad][] zip budou moct soubor stáhněte.

2. Extrahujte soubory z stažený zip do adresáře na místním počítači.

3. Upravte aw_create.bat extrahované souboru a nastavte následující proměnné obsažené v horní části souboru.  Ujistěte se, a zavřete okno bez mezery mezi "=" a parametr.  Tady jsou příklady jak může vypadat úpravami.

    ```
    server=mylogicalserver.database.windows.net
    user=mydwuser
    password=Mydwpassw0rd
    database=mydwdatabase
    ```

4. Z příkazového řádku Windows spusťte upravený aw_create.bat.  Ujistěte se, že se nacházíte v adresáři, které jste uložili upravenou verzi aw_create.bat.
Tento skript bude...
    * Uvolnění Adventure Works tabulky nebo zobrazení, které už v databázi
    * Vytvoření Adventure Works tabulek a zobrazení
    * Načtení každou tabulku Adventure Works pomocí bcp
    * Ověření počty řádků pro každou tabulku Adventure Works
    * Shromažďování statistik na každý sloupec pro každou tabulku Adventure Works


##<a name="query-sample-data"></a>Dotaz ukázková data

Až do SQL Data Warehouse jste načte ukázkovými daty, můžete rychle spustit několik dotazů.  Ke spuštění dotazu, připojte k databázi Adventure Works nově vytvořený v DW SQL Azure pomocí aplikace Visual Studio a SSDT, jak je uvedeno v dokumentu [dotazu s Visual Studio][] .

Příklad jednoduchého příkazu select získat informace o zaměstnance:

```sql
SELECT * FROM DimEmployee;
```

Příklad složitějšího dotazu pomocí konstrukcí, například GROUP BY podívat se na celkovou kapacitu pro všechny prodeje pro každý den:

```sql
SELECT OrderDateKey, SUM(SalesAmount) AS TotalSales
FROM FactInternetSales
GROUP BY OrderDateKey
ORDER BY OrderDateKey;
```

Příklad výběru se klauzuli WHERE k filtrování objednávky z před určitým datem:

```
SELECT OrderDateKey, SUM(SalesAmount) AS TotalSales
FROM FactInternetSales
WHERE OrderDateKey > '20020801'
GROUP BY OrderDateKey
ORDER BY OrderDateKey;
```

SQL datový sklad podporuje skoro všechny konstrukce T-SQL, které podporuje SQL serveru.  Rozdíly jsou popsány v naší [Přenést kód][] si přečtěte následující dokumentaci.

## <a name="next-steps"></a>Další kroky
Teď jste měli je moct vyzkoušet některé dotazy s ukázkovými daty, najdete v článku Jak [vývoj][], [načtení][]nebo [migraci][] do SQL datový sklad.

<!--Image references-->

<!--Article references-->
[migrace]: sql-data-warehouse-overview-migrate.md
[Můžete vyvíjet]: sql-data-warehouse-overview-develop.md
[načtení]: sql-data-warehouse-overview-load.md
[dotaz se Visual Studio]: sql-data-warehouse-query-visual-studio.md
[migrace kód]: sql-data-warehouse-migrate-code.md
[instalace bcp]: sql-data-warehouse-load-with-bcp.md
[instalace sqlcmd]: sql-data-warehouse-get-started-connect-sqlcmd.md

<!--Other Web references-->
[Adventure Works pro SQL datový sklad ukázky skriptů]: https://migrhoststorage.blob.core.windows.net/sqldwsample/AdventureWorksSQLDW2012.zip
