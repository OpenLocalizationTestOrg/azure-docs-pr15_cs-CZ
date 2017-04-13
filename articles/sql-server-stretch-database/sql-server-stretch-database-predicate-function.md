<properties
    pageTitle="Vyberte řádky migrace pomocí funkce filter (roztáhnout databáze) | Microsoft Azure"
    description="Zjistěte, jak chcete-li vybrat řádky, které chcete migrovat pomocí funkce filtru."
    services="sql-server-stretch-database"
    documentationCenter=""
    authors="douglaslMS"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-server-stretch-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="06/28/2016"
    ms.author="douglasl"/>

# <a name="select-rows-to-migrate-by-using-a-filter-function-stretch-database"></a>Vyberte řádky migrace pomocí funkce filter (roztáhnout databáze)

Pokud ukládáte studenou dat do tabulky, můžete nakonfigurovat roztáhnout databázi migrovat celou tabulku. Pokud tabulka obsahuje teplé a studené data, na druhé straně můžete zadat funkci Filtr vyberte řádky, které chcete migrovat. Predikát filtr je vložená tabulka\-vracejícími (funkce). Toto téma popisuje, jak psát vložená tabulka\-vracejícími funkce vyberte řádky, které chcete migrovat.

>   [AZURE.NOTE] Pokud zadaná filtr funkci, která provádí špatně provádí migraci dat také špatně. Roztažení databáze slouží k použití funkce filter do tabulky pomocí operátoru křížové použít.

Pokud nezadáte funkce filtru, je poštovním celou tabulku.

Při spuštění databáze povolit Průvodce roztáhnout, můžete migrovat celou tabulku nebo můžete zadat funkci jednoduchý filtr v průvodci. Pokud chcete použít jiný typ funkce Filtr vyberte řádky, které chcete migrovat, proveďte jeden z následujících možností:

-   Ukončete průvodce a spusťte příkaz ALTER TABLE povolení roztáhnout tabulky a zadat funkci filtr.

-   Spusťte příkaz ALTER TABLE můžete určit funkce filter po ukončení průvodce.

Syntaxe ALTER TABLE pro přidání funkce je popsané dál v tomto tématu.

## <a name="basic-requirements-for-the-filter-function"></a>Základní požadavky pro funkce filter
Vložená tabulka\-hodnotami funkce potřebných pro predikátem filtr roztáhnout databáze vypadá jako v následujícím příkladu.

```tsql
CREATE FUNCTION dbo.fn_stretchpredicate(@column1 datatype1, @column2 datatype2 [, ...n])
RETURNS TABLE
WITH SCHEMABINDING
AS
RETURN  SELECT 1 AS is_eligible
        WHERE <predicate>
```
Parametry funkce musí být identifikátory sloupců z tabulky.

Vazba schémat je potřeba k zabránit sloupce, které využívají funkce filter z textu nebo změnit.

### <a name="return-value"></a>Vrácená hodnota
Pokud funkce vrátí nenulovou\-prázdný výsledek, že se řádek oprávněni k poštovním. V opačném případě \- , pokud funkce nevrací výsledku \- řádek není oprávněni k poštovním.

### <a name="conditions"></a>Podmínky
&lt; *Predikát* &gt; mohou být tvořeny jedné podmínky, nebo více podmínek spojené s logickým operátorem AND.

```
<predicate> ::= <condition> [ AND <condition> ] [ ...n ]
```
Každé podmínce zase mohou být tvořeny jedné základní podmínky, nebo více podmínek základní spojené s logickým operátorem nebo.

```
<condition> ::= <primitive_condition> [ OR <primitive_condition> ] [ ...n ]
```

### <a name="primitive-conditions"></a>Základní podmínky
Základní podmínky můžete udělat jednu z následujících jejich porovnání.

```
<primitive_condition> ::=
{
<function_parameter> <comparison_operator> constant
| <function_parameter> { IS NULL | IS NOT NULL }
| <function_parameter> IN ( constant [ ,...n ] )
}
```

-   Porovnejte funkce parametr konstantní výraz. Například `@column1 < 1000`.

    Tady je příklad zkontroluje, jestli je hodnotu ze sloupce *date* &lt; 1/1/2016.

    ```tsql
    CREATE FUNCTION dbo.fn_stretchpredicate(@column1 datetime)
    RETURNS TABLE
    WITH SCHEMABINDING
    AS
    RETURN  SELECT 1 AS is_eligible
            WHERE @column1 < CONVERT(datetime, '1/1/2016', 101)
    GO

    ALTER TABLE stretch_table_name SET ( REMOTE_DATA_ARCHIVE = ON (
        FILTER_PREDICATE = dbo.fn_stretchpredicate(date),
        MIGRATION_STATE = OUTBOUND
    ) )
    ```

-   Použití operátoru IS NULL nebo není NULL parametrů funkce.

-   Použijte operátor a umožňuje porovnávat parametr funkce seznam konstanty.

    Tady je příklad zkontroluje, jestli hodnota *dodávky\_stav* sloupec je `IN (N'Completed', N'Returned', N'Cancelled')`.

    ```tsql
    CREATE FUNCTION dbo.fn_stretchpredicate(@column1 nvarchar(15))
    RETURNS TABLE
    WITH SCHEMABINDING
    AS
    RETURN  SELECT 1 AS is_eligible
            WHERE @column1 IN (N'Completed', N'Returned', N'Cancelled')
    GO

    ALTER TABLE table1 SET ( REMOTE_DATA_ARCHIVE = ON (
        FILTER_PREDICATE = dbo.fn_stretchpredicate(shipment_status),
        MIGRATION_STATE = OUTBOUND
    ) )
    ```

### <a name="comparison-operators"></a>Relační operátory
Jsou podporovány následující relační operátory.

`<, <=, >, >=, =, <>, !=, !<, !>`

```
<comparison_operator> ::= { < | <= | > | >= | = | <> | != | !< | !> }
```

### <a name="constant-expressions"></a>Konstantní výrazy
Konstanty, které používáte ve funkci Filtr může být deterministický výraz, který může být vyhodnocen, když definujete funkci. Konstantní výrazy mohou obsahovat následující věci.

-   Literály. Například `N’abc’, 123`.

-   Algebraický výrazů. Například `123 + 456`.

-   Deterministický funkce. Například `SQRT(900)`.

-   Deterministický převody, které používají CAST nebo převést. Například `CONVERT(datetime, '1/1/2016', 101)`.

### <a name="other-expressions"></a>Dalších výrazů
Pokud funkce výsledné odpovídá pravidel popsaných v tomto poli po nahradit BETWEEN a nikoli mezi operátory pomocí výrazů odpovídající a a nebo můžete použít BETWEEN a ne BETWEEN operátory.

Nelze použít poddotazy nebo jiných\-deterministický Funkce RAND() ATP GETDATE().

## <a name="add-a-filter-function-to-a-table"></a>Přidání funkce filter do tabulky
Přidání funkce filter do tabulky spuštěním příkaz **ALTER TABLE** a určení existující tabulky vložené\-vracejícími funkce jako hodnota **Filtr\_PREDIKÁTU** parametr. Příklad:

```tsql
ALTER TABLE stretch_table_name SET ( REMOTE_DATA_ARCHIVE = ON (
    FILTER_PREDICATE = dbo.fn_stretchpredicate(column1, column2),
    MIGRATION_STATE = <desired_migration_state>
) )
```
Po svážete funkci do tabulky jako predikátem, jsou pravdivé následující věci.

-   Další migraci dat čas výskytu jenom ty řádky, pro které funkce vrátí nenulovou\-se migrují prázdnou hodnotu.

-   Sloupce užívané u funkce jsou schématu vazby. Tyto sloupce nelze změnit také tabulky používá funkce jako jeho predikát filtr.

Nemůžete odstranit tabulku vložené\-vracejícími funkce jako tabulky používá funkce jako jeho predikát filtr.

>   [AZURE.NOTE] Aby se zvýšil výkon funkce filter, vytvořte index podle sloupce užívané u funkce.

### <a name="passing-column-names-to-the-filter-function"></a>Funkce filter předávání názvů sloupců
Pokud přiřadíte funkce filter do tabulky, zadejte názvy sloupců předán funkci filtru s názvem složené z jedné části. Pokud zadáte tří částí po úspěšném sloupci jména, následné dotazů roztáhnout\-povolené tabulce nezdaří.

Pokud zadáte název sloupce tří částí, jak je vidět v následujícím příkladu, příkaz spustí úspěšně ale dalších dotazů v tabulce nezdaří.

```tsql
ALTER TABLE SensorTelemetry
  SET ( REMOTE_DATA_ARCHIVE = ON (
    FILTER_PREDICATE=dbo.fn_stretchpredicate(dbo.SensorTelemetry.ScanDate),
    MIGRATION_STATE = OUTBOUND )
  )
```

Místo toho zadejte funkci filtru s názvem sloupce složené z jedné části, jak je vidět v následujícím příkladu.

```tsql
ALTER TABLE SensorTelemetry
  SET ( REMOTE_DATA_ARCHIVE = ON  (
    FILTER_PREDICATE=dbo.fn_stretchpredicate(ScanDate),
    MIGRATION_STATE = OUTBOUND )
  )
```

## <a name="addafterwiz"></a>Přidání funkce filter po spuštění Průvodce  

Pokud chcete použít funkci, nelze vytvořit v průvodci **Povolit databáze pro roztáhnout** , můžete použít příkaz ALTER TABLE chcete po ukončení průvodce zadat funkci. Abyste mohli použít funkci, však máte zastavit migraci dat, která už probíhá a pak znovu migrovaná data. (Další informace o tom, proč je nutné, najdete v článku [nahradit stávající funkce filtru](#replacePredicate).  

1. Otočení směru migrace a zobrazte zpět, které už poštovním data. Tuto operaci nelze zrušit po spuštění. Můžete taky vynakládá na Azure přenosů odchozí dat \(výstupní\). Další informace najdete v tématu [jak Azure ceny funguje](https://azure.microsoft.com/pricing/details/data-transfers/).  

    ```tsql  
    ALTER TABLE <table name>  
         SET ( REMOTE_DATA_ARCHIVE ( MIGRATION_STATE = INBOUND ) ) ;   
    ```  

2. Počkejte migrovat na Dokončit. Kontrola stavu v **Sledování databáze roztáhnout** od SQL Server Management Studio nebo zobrazení **sys.dm_db_rda_migration_status** můžete dotazu. Další informace najdete v tématu [sledování a odstraňování problémů s migrací dat](sql-server-stretch-database-monitor.md) nebo [sys.dm_db_rda_migration_status](https://msdn.microsoft.com/library/dn935017.aspx).  

3. Vytvoření funkci filtr, který chcete použít pro tabulku.  

4. Přidat funkci do tabulky a restartujte migrace dat z Azure.  

    ```tsql  
    ALTER TABLE <table name>  
        SET ( REMOTE_DATA_ARCHIVE  
            (           
                FILTER_PREDICATE = <predicate>,  
                MIGRATION_STATE = OUTBOUND  
            )  
        );   
    ```  

## <a name="filter-rows-by-date"></a>Filtrování řádků podle data
Následující příklad migruje řádky, kde sloupec **kalendářních dat** obsahuje hodnoty starší než 1. ledna 2016.

```tsql
-- Filter by date
--
CREATE FUNCTION dbo.fn_stretch_by_date(@date datetime2)
RETURNS TABLE
WITH SCHEMABINDING
AS
       RETURN SELECT 1 AS is_eligible WHERE @date < CONVERT(datetime2, '1/1/2016', 101)
GO
```

## <a name="filter-rows-by-the-value-in-a-status-column"></a>Filtrování řádků podle hodnoty ve sloupci Stav
Následující příklad migruje řádky, které ve sloupci **Stav** obsahuje jednu ze zadané hodnoty.

```tsql
-- Filter by status column
--
CREATE FUNCTION dbo.fn_stretch_by_status(@status nvarchar(128))
RETURNS TABLE
WITH SCHEMABINDING
AS
       RETURN SELECT 1 AS is_eligible WHERE @status IN (N'Completed', N'Returned', N'Cancelled')
GO
```

## <a name="filter-rows-by-using-a-sliding-window"></a>Filtrování řádků pomocí posuvná okna
Filtrování řádků pomocí posuvná okna, mějte na paměti následující požadavky pro funkci filtr.

-   Funkce musí být deterministický. Proto nelze vytvořit funkci, která automaticky přepočítá posuvná okna po uplynutí určitého času.

-   Použije funkce vazba schémat. Proto nelze aktualizovat jednoduše funkci "na místě" každý den tak, že zavoláte **Změnit funkce** přesunout posuvná okna.

Funkce filter jako v následujícím příkladu migruje řádků místo, kam se sloupci **systemEndTime** obsahuje hodnotu starší než 1. ledna 2016 začínat.

```tsql
CREATE FUNCTION dbo.fn_StretchBySystemEndTime20160101(@systemEndTime datetime2)
RETURNS TABLE
WITH SCHEMABINDING  
AS  
RETURN SELECT 1 AS is_eligible
  WHERE @systemEndTime < CONVERT(datetime2, '2016-01-01T00:00:00', 101) ;
```

Funkce filter platí pro tabulku.

```tsql
ALTER TABLE <table name>
SET (
        REMOTE_DATA_ARCHIVE = ON
                (
                        FILTER_PREDICATE = dbo.fn_StretchBySystemEndTime20160101 (SysEndTime)
                                , MIGRATION_STATE = OUTBOUND
                )
        )
;
```

Když chcete aktualizovat posuvná okna, proveďte následující akce.

1.  Vytvořte novou funkci, která určuje nového posuvná okna. Následující příklad slouží k výběru data starší než 2 leden 2016, místo 1. ledna 2016.

2.  Nahraďte předchozí funkce filter se nový obrázek, volající **ALTER TABLE**, jak je vidět v následujícím příkladu.

3. Volitelně můžete přetáhněte předchozí filtr funkci, kterou už používáte volání **Uvolnění funkce**. (Tento krok není uveden v příkladu).

```tsql
BEGIN TRAN
GO
        /*(1) Create new predicate function definition */
        CREATE FUNCTION dbo.fn_StretchBySystemEndTime20160102(@systemEndTime datetime2)
        RETURNS TABLE
        WITH SCHEMABINDING
        AS
        RETURN SELECT 1 AS is_eligible
               WHERE @systemEndTime < CONVERT(datetime2,'2016-01-02T00:00:00', 101)
        GO

        /*(2) Set the new function as the filter predicate */
        ALTER TABLE <table name>
        SET
        (
               REMOTE_DATA_ARCHIVE = ON
               (
                       FILTER_PREDICATE = dbo.fn_StretchBySystemEndTime20160102(SysEndTime),
                       MIGRATION_STATE = OUTBOUND
               )
        )
COMMIT ;
```

## <a name="more-examples-of-valid-filter-functions"></a>Další příklady platné filtrovací funkce

-   V následujícím příkladu spojuje dva základní podmínek pomocí logickým operátorem AND.

    ```tsql
    CREATE FUNCTION dbo.fn_stretchpredicate((@column1 datetime, @column2 nvarchar(15))
    RETURNS TABLE
    WITH SCHEMABINDING
    AS
    RETURN  SELECT 1 AS is_eligible
      WHERE @column1 < N'20150101' AND @column2 IN (N'Completed', N'Returned', N'Cancelled')
    GO

    ALTER TABLE table1 SET ( REMOTE_DATA_ARCHIVE = ON (
        FILTER_PREDICATE = dbo.fn_stretchpredicate(date, shipment_status),
        MIGRATION_STATE = OUTBOUND
    ) )
    ```

-   V následujícím příkladu několik podmínek a deterministický převodu s převést.

    ```tsql
    CREATE FUNCTION dbo.fn_stretchpredicate_example1(@column1 datetime, @column2 int, @column3 nvarchar)
    RETURNS TABLE
    WITH SCHEMABINDING
    AS
    RETURN  SELECT 1 AS is_eligible
        WHERE @column1 < CONVERT(datetime, '1/1/2015', 101)AND (@column2 < -100 OR @column2 > 100 OR @column2 IS NULL)AND @column3 IN (N'Completed', N'Returned', N'Cancelled')
    GO
    ```

-   V následujícím příkladu matematické operátory a funkcí.

    ```tsql
    CREATE FUNCTION dbo.fn_stretchpredicate_example2(@column1 float)
    RETURNS TABLE
    WITH SCHEMABINDING
    AS
    RETURN  SELECT 1 AS is_eligible
            WHERE @column1 < SQRT(400) + 10
    GO
    ```

-   V následujícím příkladu BETWEEN a ne BETWEEN operátory. Tento použití je platný, protože funkce výsledné odpovídá pravidel popsaných v tomto poli po nahradit BETWEEN a nikoli mezi operátory pomocí výrazů odpovídající a a nebo.

    ```tsql
    CREATE FUNCTION dbo.fn_stretchpredicate_example3(@column1 int, @column2 int)
    RETURNS TABLE
    WITH SCHEMABINDING
    AS
    RETURN  SELECT 1 AS is_eligible
            WHERE @column1 BETWEEN 0 AND 100
                AND (@column2 NOT BETWEEN 200 AND 300 OR @column1 = 50)
    GO
    ```
    Předchozí funkci odpovídá následující funkce po nahradíte BETWEEN a ne BETWEEN operátorů výrazů odpovídající a a nebo.

    ```tsql
    CREATE FUNCTION dbo.fn_stretchpredicate_example4(@column1 int, @column2 int)
    RETURNS TABLE
    WITH SCHEMABINDING
    AS
    RETURN  SELECT 1 AS is_eligible
            WHERE @column1 >= 0 AND @column1 <= 100AND (@column2 < 200 OR @column2 > 300 OR @column1 = 50)
    GO
    ```

## <a name="examples-of-filter-functions-that-arent-valid"></a>Příklady filtrovací funkce, které nejsou platné

-   Následující funkce není platný, protože obsahuje nenulovou\-deterministický převod.

    ```tsql
    CREATE FUNCTION dbo.fn_example5(@column1 datetime)
    RETURNS TABLE
    WITH SCHEMABINDING
    AS
    RETURN  SELECT 1 AS is_eligible
            WHERE @column1 < CONVERT(datetime, '1/1/2016')
    GO
    ```

-   Následující funkce není platný, protože obsahuje nenulovou\-volání deterministický funkce.

    ```tsql
    CREATE FUNCTION dbo.fn_example6(@column1 datetime)
    RETURNS TABLE
    WITH SCHEMABINDING
    AS
    RETURN  SELECT 1 AS is_eligible
            WHERE @column1 < DATEADD(day, -60, GETDATE())
    GO
    ```

-   Následující funkce nejsou platné, protože obsahuje poddotazu.

    ```tsql
    CREATE FUNCTION dbo.fn_example7(@column1 int)
    RETURNS TABLE
    WITH SCHEMABINDING
    AS
    RETURN  SELECT 1 AS is_eligible
            WHERE @column1 IN (SELECT SupplierID FROM Supplier WHERE Status = 'Defunct'))
    GO
    ```

-   Následující funkce nejsou platné protože výrazů, jejichž algebraický operátory nebo integrované\-ve funkcích musí být konstanta když definujete funkci. Nesmí obsahovat odkazy na sloupce v algebraické výrazy nebo volání funkce.

    ```tsql
    CREATE FUNCTION dbo.fn_example8(@column1 int)
    RETURNS TABLE
    WITH SCHEMABINDING
    AS
    RETURN  SELECT 1 AS is_eligible
            WHERE @column1 % 2 =  0
    GO

    CREATE FUNCTION dbo.fn_example9(@column1 int)
    RETURNS TABLE
    WITH SCHEMABINDING
    AS
    RETURN  SELECT 1 AS is_eligible
            WHERE SQRT(@column1) = 30
    GO
    ```

-   Následující funkce nejsou platné, protože porušuje pravidla zde popsané po nahradíte operátor BETWEEN ekvivalentním výrazem a.

    ```tsql
    CREATE FUNCTION dbo.fn_example10(@column1 int, @column2 int)
    RETURNS TABLE
    WITH SCHEMABINDING
    AS
    RETURN  SELECT 1 AS is_eligible
            WHERE (@column1 BETWEEN 1 AND 200 OR @column1 = 300) AND @column2 > 1000
    GO
    ```
    Předchozí funkci odpovídá následující funkce po nahradíte operátor BETWEEN ekvivalentním výrazem a. Tato funkce není platný, protože základní podmínky můžete použít jenom s logickým operátorem nebo.

    ```tsql
    CREATE FUNCTION dbo.fn_example11(@column1 int, @column2 int)
    RETURNS TABLE
    WITH SCHEMABINDING
    AS
    RETURN  SELECT 1 AS is_eligible
            WHERE (@column1 >= 1 AND @column1 <= 200 OR @column1 = 300) AND @column2 > 1000
    GO
    ```

## <a name="how-stretch-database-applies-the-filter-function"></a>Použití funkce filter roztáhnout databáze
Roztažení databáze použije funkce filter do tabulky a určuje měl řádků pomocí operátoru křížové použít. Příklad:

```tsql
SELECT * FROM stretch_table_name CROSS APPLY fn_stretchpredicate(column1, column2)
```
Pokud funkce vrátí nenulovou\-prázdný výsledek řady tento řádek představuje oprávněni k poštovním.

## <a name="replacePredicate"></a>Nahrazení stávajících funkce filter
Funkce filter dříve zadané můžete nahradit spuštěním příkaz **ALTER TABLE** znovu a určení nová hodnota pro **Filtr\_PREDIKÁTU** parametr. Příklad:

```tsql
ALTER TABLE stretch_table_name SET ( REMOTE_DATA_ARCHIVE = ON (
    FILTER_PREDICATE = dbo.fn_stretchpredicate2(column1, column2),
    MIGRATION_STATE = <desired_migration_state>
```
Nové tabulky vložené\-hodnotami má tyto požadavky.

-   Nová funkce musí být menší omezující než předchozí funkce.

-   Všechny operátory, které existují ve funkci staré musí existovat v této nové funkci.

-   Nová funkce nesmí obsahovat operátory, které nejsou ve funkci staré.

-   Pořadí operátorů argumenty nelze změnit.

-   Pouze konstanty, které jsou součástí `<, <=, >, >=` porovnání můžete změnit tak, aby funkce méně omezující.

### <a name="example-of-a-valid-replacement"></a>Příklad platné náhradní
Předpokládejme, že následující funkce predikát aktuální filtr.

```tsql
CREATE FUNCTION dbo.fn_stretchpredicate_old (@column1 datetime, @column2 int)
RETURNS TABLE
WITH SCHEMABINDING
AS
RETURN  SELECT 1 AS is_eligible
        WHERE @column1 < CONVERT(datetime, '1/1/2016', 101)
            AND (@column2 < -100 OR @column2 > 100)
GO
```
Následující funkce nahrazuje platné protože nové datum konstanta (určuje pozdější datum uzavření) funkci méně omezující.

```tsql
CREATE FUNCTION dbo.fn_stretchpredicate_new (@column1 datetime, @column2 int)
RETURNS TABLE
WITH SCHEMABINDING
AS
RETURN  SELECT 1 AS is_eligible
        WHERE @column1 < CONVERT(datetime, '2/1/2016', 101)
            AND (@column2 < -50 OR @column2 > 50)
GO
```

### <a name="examples-of-replacements-that-arent-valid"></a>Příklady, které nejsou platné nahrazení
Následující funkce není platný náhradní, protože nové datum konstanta (určuje dříve přerušení datem) není tomu, aby fungoval méně omezující.

```tsql
CREATE FUNCTION dbo.fn_notvalidreplacement_1 (@column1 datetime, @column2 int)
RETURNS TABLE
WITH SCHEMABINDING
AS
RETURN  SELECT 1 AS is_eligible
        WHERE @column1 < CONVERT(datetime, '1/1/2015', 101)
            AND (@column2 < -100 OR @column2 > 100)
GO
```
Následující funkce není platný náhradní, protože jeden z relační operátory odebrána.

```tsql
CREATE FUNCTION dbo.fn_notvalidreplacement_2 (@column1 datetime, @column2 int)
RETURNS TABLE
WITH SCHEMABINDING
AS
RETURN  SELECT 1 AS is_eligible
        WHERE @column1 < CONVERT(datetime, '1/1/2016', 101)
            AND (@column2 < -50)
GO
```
Následující funkce není platný náhradní, protože novou podmínku je přidán s logickým operátorem AND.

```tsql
CREATE FUNCTION dbo.fn_notvalidreplacement_3 (@column1 datetime, @column2 int)
RETURNS TABLE
WITH SCHEMABINDING
AS
RETURN  SELECT 1 AS is_eligible
        WHERE @column1 < CONVERT(datetime, '1/1/2016', 101)
            AND (@column2 < -100 OR @column2 > 100)
            AND (@column2 <> 0)
GO
```

## <a name="remove-a-filter-function-from-a-table"></a>Odebrání filtru funkce tabulky
Migrace celou tabulku místo vybraných řádků, odstraňte existující funkce nastavením **Filtr\_PREDIKÁTU** na hodnotu null. Příklad:

```tsql
ALTER TABLE stretch_table_name SET ( REMOTE_DATA_ARCHIVE = ON (
    FILTER_PREDICATE = NULL,
    MIGRATION_STATE = <desired_migration_state>
) )
```
Po odebrání funkce filter všechny řádky v tabulce se dá použít k migraci. Funkce filter pro stejné tabulky později nelze zadat jako výsledek, není-li se vrátit Vzdálená data pro tabulku z Azure nejdřív. Chcete-li předejít situaci, kde řádky, které se nedají migrace při poskytování nové funkce filter již byly přešli na Azure existuje toto omezení.

## <a name="check-the-filter-function-applied-to-a-table"></a>Kontrola funkci Filtr použitý na tabulku
Kontrola funkci Filtr použitý na tabulku, otevřete zobrazení katalogu **sys.remote\_dat\_archivu\_tabulek** a zkontrolujte hodnotu **Filtr\_predikát** sloupce. Pokud je argument hodnota null, je oprávněn pro archivaci celou tabulku. Další informace najdete v tématu [sys.remote_data_archive_tables jazyce Transact-SQL ()](https://msdn.microsoft.com/library/dn935003.aspx).

## <a name="security-notes-for-filter-functions"></a>Poznámky k zabezpečení pro filtrovací funkce  
Hostují účtu s oprávněními db_owner můžete provádět následující akce.  

-   Vytvoření a použití funkce vracející tabulku, která používá velké množství prostředků serveru nebo čeká delší dobu výsledkem odmítnutí služby.  

-   Vytvoření a použití funkce vracející tabulku, která vám umožní odvodit obsah tabulky, pro kterou uživatel byl explicitně odepřen přístup pro čtení.  

## <a name="see-also"></a>Viz taky

[ALTER TABLE (Transact-SQL)](https://msdn.microsoft.com/library/ms190273.aspx)
