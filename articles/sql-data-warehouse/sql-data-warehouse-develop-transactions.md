<properties
   pageTitle="Transakce v SQL datový sklad | Microsoft Azure"
   description="Tipy pro implementaci transakce v Azure SQL datový sklad k vývoji řešení."
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
   ms.date="07/31/2016"
   ms.author="jrj;barbkess;sonyama"/>

# <a name="transactions-in-sql-data-warehouse"></a>Transakce v SQL datový sklad

Jak byste čekali, SQL datový sklad podporuje transakce jako součást pracovní zátěž datový sklad. Zajistit, aby že výkon SQL datový sklad spravován ve velkém měřítku některé funkce jsou ale omezený ve srovnání s SQL serveru. Tento článek zvýrazňuje rozdíly a seznamy na ostatní. 

## <a name="transaction-isolation-levels"></a>Úrovně izolace transakce
SQL datový sklad používá kyseliny transakce. Izolace transakční podpory je ale omezený na `READ UNCOMMITTED` a nejde je změnit. Můžete provést řadu kódování způsoby zabránění hrubé čtení dat, pokud je to problém za vás. Nejoblíbenější metody využít CTAS a tabulky oddíl přechodu na jiný (často označované jako klouzavé vzorek okna) zabránit uživatelům v dotazy na data, která je stále připravena. Zobrazení, která předem filtrování dat je také oblíbených přístup.  

## <a name="transaction-size"></a>Velikost transakce
Změna transakce jednu datovou má omezenou velikost. Limit dnes je použita "za rozdělení". Proto můžete vypočítat celkové přidělení vynásobením limit počtu rozdělení. Chcete přibližnou maximální počet řádků v transakce vydělte zakončení rozdělení celkovou velikost každý řádek. Proměnná délka sloupce je třeba zvážit přijímat délky průměr sloupce spíše než maximální velikost.

V následující tabulce následující předpoklady bylo provedeno:

* Došlo k rovnoměrné rozdělení dat 
* Délka průměr řádek je 250 bajtů

| [DWU][]    | Caps za rozdělení (OS) | Počet rozdělení | MAXIMÁLNÍ velikost transakce (OS) | # Řádky za rozdělení. | Maximální počet řádků na transakce |
| ------ | -------------------------- | ----------------------- | -------------------------- | ----------------------- | ------------------------ |
| DW100  |  1                         | 60                      |   60                       |   4,000,000             |    240,000,000           |
| DW200  |  1.5                       | 60                      |   90                       |   6,000,000             |    360,000,000           |
| DW300  |  2.25                      | 60                      |  135                       |   9,000,000             |    540,000,000           |
| DW400  |  3                         | 60                      |  180                       |  12,000,000             |    720,000,000           |
| DW500  |  3,75                      | 60                      |  225                       |  15,000,000             |    900,000,000           |
| DW600  |  4.5                       | 60                      |  270 stupňů                       |  18,000,000             |  1,080,000,000           |
| DW1000 |  7.5                       | 60                      |  450                       |  30,000,000             |  1,800,000,000           |
| DW1200 |  9                         | 60                      |  540                       |  36,000,000             |  2,160,000,000           |
| DW1500 | 11,25                      | 60                      |  675                       |  45,000,000             |  2,700,000,000           |
| DW2000 | 15                         | 60                      |  900                       |  60 000 000             |  3,600,000,000           |
| DW3000 | 22.5                       | 60                      |  1,350                     |  90,000,000             |  5,400,000,000           |
| DW6000 | 45                         | 60                      |  2,700                     | 180,000,000             | 10,800,000,000           |

Omezení velikosti transakce se použije na transakce nebo operace. Se nepoužil přes všechny souběžné transakce. Proto každou transakci povoleno zapisovat tohoto množství dat do protokolu. 

Optimalizace a minimalizace množství dat do protokolu naleznete v článku [transakce doporučené postupy][] .

> [AZURE.WARNING] Velikost maximální transakce lze dosáhnout pouze pro HASH nebo dokonce ROUND_ROBIN distributed tabulky se šíření data. Pokud transakce zapisuje dat asymetrické způsobem rozdělení je limit mohou získat přístup na stránku před transakce maximální velikost.
<!--REPLICATED_TABLE-->

## <a name="transaction-state"></a>Stav transakce
SQL datový sklad používá funkci XACT_STATE() vykazování neúspěšné transakce použít hodnotu -2. To znamená, že transakce selže a je označený pro vrácení zpět pouze

> [AZURE.NOTE] Použití čísla -2 funkcí XACT_STATE k označení neúspěšné transakce představuje různé chování k serveru SQL Server. SQL Server použije hodnotu -1 představující zrušením committable transakce. SQL Server nevadí vám chybami uvnitř transakce bez nutnosti označit jako zrušením committable. Příklad `SELECT 1/0` byste měli mít za následek chybu, ale bez vynucení transakce do zrušením committable stavu. SQL Server také umožňuje čtení v zrušením committable transakce. Však SQL datový sklad neumožňuje udělejte toto. Pokud dojde k chybě uvnitř transakce SQL datový sklad zadá automaticky stav-2 a nebudete moct provést další vyberte příkazy, dokud příkazu má vrátit zpět. Proto je důležité zkontrolovat, že kód aplikace zobrazíte při XACT_STATE() používala provádět úpravy kódu.

V SQL serveru se může zobrazit transakce, která vypadá takto:

```sql
SET NOCOUNT ON;
DECLARE @xact_state smallint = 0;

BEGIN TRAN
    BEGIN TRY
        DECLARE @i INT;
        SET     @i = CONVERT(INT,'ABC');
    END TRY
    BEGIN CATCH
        SET @xact_state = XACT_STATE();

        SELECT  ERROR_NUMBER()    AS ErrNumber
        ,       ERROR_SEVERITY()  AS ErrSeverity
        ,       ERROR_STATE()     AS ErrState
        ,       ERROR_PROCEDURE() AS ErrProcedure
        ,       ERROR_MESSAGE()   AS ErrMessage
        ;
        
        IF @@TRANCOUNT > 0
        BEGIN
            PRINT 'ROLLBACK';
            ROLLBACK TRAN;
        END

    END CATCH;

IF @@TRANCOUNT >0
BEGIN
    PRINT 'COMMIT';
    COMMIT TRAN;
END

SELECT @xact_state AS TransactionState;
```

Pokud jste ponechejte uvedený váš kód nad obdržíte následující chybovou zprávu:

Zpráva 111233, 16 úroveň stát 1, řádek 1 111233; Aktuální transakce přerušena a změny čekající na vyřízení mají vrátit zpět. Příčina: Transakce ve stavu pouze vrácení nebyl vrátit explicitně před DDL, DML nebo příkazu SELECT.

Také nedostanete výstup funkce ERROR_ *.

V SQL datový sklad kód je třeba mírně změnit:

```sql
SET NOCOUNT ON;
DECLARE @xact_state smallint = 0;

BEGIN TRAN
    BEGIN TRY
        DECLARE @i INT;
        SET     @i = CONVERT(INT,'ABC');
    END TRY
    BEGIN CATCH
        SET @xact_state = XACT_STATE();
        
        IF @@TRANCOUNT > 0
        BEGIN
            PRINT 'ROLLBACK';
            ROLLBACK TRAN;
        END

        SELECT  ERROR_NUMBER()    AS ErrNumber
        ,       ERROR_SEVERITY()  AS ErrSeverity
        ,       ERROR_STATE()     AS ErrState
        ,       ERROR_PROCEDURE() AS ErrProcedure
        ,       ERROR_MESSAGE()   AS ErrMessage
        ;
    END CATCH;

IF @@TRANCOUNT >0
BEGIN
    PRINT 'COMMIT';
    COMMIT TRAN;
END

SELECT @xact_state AS TransactionState;
```

Očekávaná schůzku dodržení. Chyba v transakce spravují a funkce ERROR_ * zadejte hodnoty podle očekávání.

Vše, co se změnilo, která je `ROLLBACK` transakce musel dojít před číst informace o chybě v `CATCH` blok.

## <a name="errorline-function"></a>Error_Line() (funkce)
Také je vhodné poznamenat, že SQL datový sklad implementace nebo podporovat funkce ERROR_LINE(). Pokud máte nainstalovanou v kódu budete muset odebrat, aby byl kompatibilní s SQL datový sklad. Použijte dotazu popisky v kódu implementovat odpovídající funkce. Získáte v článku [Popisek][] podrobné informace o této funkci.

## <a name="using-throw-and-raiserror"></a>Použití hodit a RAISERROR
ZAHODIT je Modernější implementace pro zvýšení výjimky v SQL datový sklad ale podporuje se taky RAISERROR. Existuje několik rozdílů, které jsou jmění ale platíte pozornost.

- Chybovým zprávám, které nesmí být čísla v oblasti 150 100 000 000 pro hodit definované uživatelem
- Chybové zprávy RAISERROR řeší na 50 000
- Použití sys.messages nepodporuje

## <a name="limitiations"></a>Limitiations
SQL datový sklad mít několik další omezení, které se vztahují k transakce.

Jsou následujícím způsobem:

- Žádné distribuované transakce
- Žádné vnořené transakce povolené.
- Bez uložení body povolené
- Žádné pojmenované transakce
- Žádné označené transakce
- Podpora DDL jako `CREATE TABLE` uvnitř uživatelem definované transakce

## <a name="next-steps"></a>Další kroky
Další informace o optimalizaci transakce, najdete v článku [transakce doporučené postupy][].  Další informace o dalších SQL datový sklad doporučených postupech najdete v tématu [Doporučené postupy SQL datový sklad][].

<!--Image references-->

<!--Article references-->
[DWU]: ./sql-data-warehouse-overview-what-is.md#data-warehouse-units
[development overview]: ./sql-data-warehouse-overview-develop.md
[Doporučené postupy transakce]: ./sql-data-warehouse-develop-best-practices-transactions.md
[Doporučené postupy SQL datový sklad]: ./sql-data-warehouse-best-practices.md
[POPISEK]: ./sql-data-warehouse-develop-label.md

<!--MSDN references-->

<!--Other Web references-->
