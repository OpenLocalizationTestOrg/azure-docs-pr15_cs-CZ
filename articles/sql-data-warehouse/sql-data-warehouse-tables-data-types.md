<properties
   pageTitle="Datové typy pro tabulky v SQL datový sklad | Microsoft Azure"
   description="Video: Začínáme s datovými typy tabulek Azure SQL datový sklad."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="jrowlandjones"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="06/29/2016"
   ms.author="jrj;barbkess;sonyama"/>

# <a name="data-types-for-tables-in-sql-data-warehouse"></a>Datové typy pro tabulky v SQL datový sklad

> [AZURE.SELECTOR]
- [Základní informace][]
- [Datové typy][]
- [Distribuce][]
- [Index][]
- [Oddíl][]
- [Statistiky][]
- [Dočasné][]

Podporuje SQL datový sklad nejčastěji používají datové typy.  Tady je seznam datových typů podporovaných SQL datový sklad.  Další informace o datový typ podpory najdete v tématu [Vytvoření tabulky][].

|**Podporované datové typy**|||
|---|---|---|
[bigint][]|[Decimal][]|[smallint][]|
[binární.][]|[uvolnit][]|[Smallmoney][]|
[Bit][]|[Funkce INT][]|[sysname][]|
[znak][]|[peníze][]|[čas][]|
[Datum][]|[nchar][]|[tinyint][]|
[data a času][]|[nvarchar][]|[UniqueIdentifier][]|
[datetime2][]|[skutečné][]|[Seskupit][]|
[DateTimeOffset][]|[smalldatetime][]|[Datová][]|


## <a name="data-type-best-practices"></a>Datový typ doporučené postupy

 Při definování typy sloupců, pomocí nejmenší datový typ, který bude podporovat dat bude výkon dotazu. To je zvlášť důležité znak a datová sloupců. Pokud je argument nejdelší hodnota ve sloupci 25 znaků, definujte sloupce jako VARCHAR(25). Vyhněte se definování všechny sloupce znak velké výchozí délku. Kromě toho sloupce, definujte jako datová, je-li vše, co je potřeba místo [NVARCHAR][].  Použijte NVARCHAR(4000) VARCHAR(8000), pokud je to možné místo NVARCHAR(MAX) nebo VARCHAR(MAX).

## <a name="polybase-limitation"></a>Omezení Polybase

Pokud používáte Polybase načtení tabulky, definujte tabulkách, aby maximální možné velikost řádku, včetně celou délku proměnné délky sloupce nepřekročí 32 767 bajtů.  Když definujete řádku daty proměnné délky, která můžete překročit tato šířka a načtení řádků s BCP, nebudete moct používat Polybase načíst tato data.  Podpora Polybase široké řádky se přidá brzy bude k dispozici.

## <a name="unsupported-data-types"></a>Nepodporované datové typy

Pokud ze kterého migrujete databázi jiné platformy SQL, jako jsou databáze SQL Azure, jak můžete migrovat, můžete narazit některé datové typy, které nejsou podporovány v SQL datový sklad.  Tady jsou nepodporovaných datových typů a také pár alternativ, které lze použít místo nepodporovaných datových typů.

|Datový typ|Řešení:|
|---|---|
|[geometrie][]|[Seskupit][]|
|[zeměpisná oblast][]|[Seskupit][]|
|[ID hierarchie][]|[nvarchar][] (4000)|
|[Obrázek][ntext,text,image]|[Seskupit][]|
|[text][ntext,text,image]|[Datová][]|
|[ntext][ntext,text,image]|[nvarchar][]|
|[SQL_VARIANT][]|Rozdělit sloupec do několika silného sloupců.|
|[tabulky][]|Převeďte dočasné tabulky.|
|[časové razítko][]|Přepracovat kód použít [datetime2][] a `CURRENT_TIMESTAMP` (funkce).  Pouze konstanty jsou podporované jako výchozí, tedy current_timestamp nemůžou být definované jako výchozí omezení. Pokud potřebujete přejít řádku verze hodnoty ze sloupce zadaný časové razítko použijte [binární][](8) nebo [Seskupit][binární](8) pro není NULL nebo hodnota NULL řádku hodnoty verze.|
|[XML][]|[Datová][]|
|[typy definované uživatelem][]|Pokud je to možné převést zpět na své nativní typy|
|výchozí hodnoty|výchozí hodnoty podporují literály a pouze konstanty.  Nedeterministické výrazy nebo funkcí, jako například `GETDATE()` nebo `CURRENT_TIMESTAMP`, nejsou podporované.|

Pod SQL možné spouštět na aktuální databáze SQL k identifikaci sloupce, které se nesmí být nepodporuje Azure SQL datový sklad:

```sql
SELECT  t.[name], c.[name], c.[system_type_id], c.[user_type_id], y.[is_user_defined], y.[name]
FROM sys.tables  t
JOIN sys.columns c on t.[object_id]    = c.[object_id]
JOIN sys.types   y on c.[user_type_id] = y.[user_type_id]
WHERE y.[name] IN ('geography','geometry','hierarchyid','image','text','ntext','sql_variant','timestamp','xml')
 AND  y.[is_user_defined] = 1;
```

## <a name="next-steps"></a>Další kroky

Další informace naleznete v článcích na [Tabulky přehled][Přehled], [Distribuce tabulky][distribuovat], [indexování tabulky][Index], [rozdělení tabulky][oddílu], [Zachování statistiky tabulek][Statistika] a [Dočasné tabulky][dočasné].  Další informace o doporučených postupech najdete v tématu [SQL Data Warehouse doporučené postupy][].

<!--Image references-->

<!--Article references-->
[Základní informace]: ./sql-data-warehouse-tables-overview.md
[Datové typy]: ./sql-data-warehouse-tables-data-types.md
[Distribuce]: ./sql-data-warehouse-tables-distribute.md
[Index]: ./sql-data-warehouse-tables-index.md
[Oddíl]: ./sql-data-warehouse-tables-partition.md
[Statistiky]: ./sql-data-warehouse-tables-statistics.md
[Dočasné]: ./sql-data-warehouse-tables-temporary.md
[SQL datový sklad doporučené postupy]: ./sql-data-warehouse-best-practices.md

<!--MSDN references-->

<!--Other Web references-->
[Vytvoření tabulky]: https://msdn.microsoft.com/library/mt203953.aspx
[bigint]: https://msdn.microsoft.com/library/ms187745.aspx
[binární.]: https://msdn.microsoft.com/library/ms188362.aspx
[Bit]: https://msdn.microsoft.com/library/ms177603.aspx
[znak]: https://msdn.microsoft.com/library/ms176089.aspx
[Datum]: https://msdn.microsoft.com/library/bb630352.aspx
[data a času]: https://msdn.microsoft.com/library/ms187819.aspx
[datetime2]: https://msdn.microsoft.com/library/bb677335.aspx
[DateTimeOffset]: https://msdn.microsoft.com/library/bb630289.aspx
[Decimal]: https://msdn.microsoft.com/library/ms187746.aspx
[uvolnit]: https://msdn.microsoft.com/library/ms173773.aspx
[geometrie]: https://msdn.microsoft.com/library/cc280487.aspx
[zeměpisná oblast]: https://msdn.microsoft.com/library/cc280766.aspx
[ID hierarchie]: https://msdn.microsoft.com/library/bb677290.aspx
[Funkce INT]: https://msdn.microsoft.com/library/ms187745.aspx
[peníze]: https://msdn.microsoft.com/library/ms179882.aspx
[nchar]: https://msdn.microsoft.com/library/ms186939.aspx
[nvarchar]: https://msdn.microsoft.com/library/ms186939.aspx
[ntext,text,image]: https://msdn.microsoft.com/library/ms187993.aspx
[skutečné]: https://msdn.microsoft.com/library/ms173773.aspx
[smalldatetime]: https://msdn.microsoft.com/library/ms182418.aspx
[smallint]: https://msdn.microsoft.com/library/ms187745.aspx
[Smallmoney]: https://msdn.microsoft.com/library/ms179882.aspx
[SQL_VARIANT]: https://msdn.microsoft.com/library/ms173829.aspx
[sysname]: https://msdn.microsoft.com/library/ms186939.aspx
[tabulky]: https://msdn.microsoft.com/library/ms175010.aspx
[čas]: https://msdn.microsoft.com/library/bb677243.aspx
[časové razítko]: https://msdn.microsoft.com/library/ms182776.aspx
[tinyint]: https://msdn.microsoft.com/library/ms187745.aspx
[UniqueIdentifier]: https://msdn.microsoft.com/library/ms187942.aspx
[Seskupit]: https://msdn.microsoft.com/library/ms188362.aspx
[Datová]: https://msdn.microsoft.com/library/ms186939.aspx
[XML]: https://msdn.microsoft.com/library/ms187339.aspx
[typy definované uživatelem]: https://msdn.microsoft.com/library/ms131694.aspx
