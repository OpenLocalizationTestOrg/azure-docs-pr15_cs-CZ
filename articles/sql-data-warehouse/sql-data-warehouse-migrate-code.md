<properties
   pageTitle="Migrace kód SQL do SQL datový sklad | Microsoft Azure"
   description="Tipy pro migraci kód SQL Azure SQL datový sklad k vývoji řešení."
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
   ms.date="08/02/2016"
   ms.author="lodipalm;barbkess;sonyama;jrj"/>

# <a name="migrate-your-sql-code-to-sql-data-warehouse"></a>Migrace kód SQL do SQL datový sklad

Při migraci kódu z jiné databáze SQL datový sklad, bude nejspíš potřebujete změnit základ kódu. Některé funkce SQL datový sklad můžete výrazně zvýšit výkon, jako jsou navržený pro práci distribuované způsobem. Abyste zachovali výkon a měřítko, některé funkce jsou však také není k dispozici.

## <a name="common-t-sql-limitations"></a>Společná omezení T-SQL

Následující seznam shrnuje nejčastěji používané funkce, která nejsou podporované v Azure SQL datový sklad. Odkazy vás zavedou ke zástupná nepodporovanou funkcí:

- [Spojení na aktualizace][]
- [Spojení na odstraní][]
- [příkaz Sloučit][]
- spojení mezi databáze
- [kurzory][]
- [VYBERTE. DO][]
- [VLOŽENÍ. SPOUŠTĚNÍ][]
- klauzule výstup
- vložené uživatelem definovaných funkcí
- funkce více údajů
- [Tabulka výrazy](#Common-table-expressions)
- [rekurzivní tabulky výrazy (CTE)] (#Recursive-common-table-expressions-(CTE)
- Funkce CLR a postupy
- $partition (funkce)
- proměnné tabulky
- Tabulka hodnoty parametrů
- distribuované transakce
- potvrzení / pracovní vrácení zpět
- uložení transakce
- spuštění kontexty (spouštět AS)
- [klauzule s funkcí rollup Group by / datové krychle / sady možnosti seskupení][]
- [počet úrovní vnoření za 8][]
- [aktualizace prostřednictvím zobrazení][]
- [použití vyberte pro proměnné přiřazení][]
- [žádné MAX datový typ pro dynamické řetězců SQL][]

Většina tato omezení můžete naštěstí odpracovaných kolem. Vysvětlení jsou k dispozici v článcích relevantní vývoj výše uvedené.

## <a name="supported-cte-features"></a>Podporované funkce CTE

Tabulka výrazy (CTEs) částečně podporuje SQL datový sklad.  Nyní jsou podporované tyto funkce CTE:

- CTE může být zadán v příkazu SELECT.
- CTE může být zadán v podobě příkazu vytvořit zobrazení.
- CTE může být zadán v podobě příkazu vytvořit tabulky jako vyberte (CTAS).
- CTE může být zadán v podobě příkazu vytvořit vzdálené tabulky jako vyberte (CRTAS).
- CTE může být zadán v podobě příkazu vytvořit externí tabulky jako vyberte (CETAS).
- Vzdálené tabulky můžete odkázat CTE.
- Externí tabulku můžete odkázat CTE.
- Více definice dotazu CTE možné definovat ve CTE.

## <a name="cte-limitations"></a>Omezení CTE

Běžné výrazy tabulky mají určitá omezení v SQL datový sklad včetně:

- CTE následovat jediný příkaz SELECT. VLOŽENÍ, aktualizace, odstranění a SLOUČENÍ příkazy nejsou podporovány.
- Běžné výraz tabulky, která obsahuje odkazy na samotné (rekurzivní běžné výraz tabulky) není podporované (viz níže oddíl).
- Zadání více než jedna z nich klauzule CTE není povolená. Například obsahuje-li CTE_query_definition poddotazu, že poddotazu nesmí obsahovat vnořený s klauzulí, který definuje jiné CTE.
- Klauzule ORDER nelze použít v CTE_query_definition, s výjimkou, pokud není zadán klauzuli TOP.
- Když CTE bude použita ve výrazu, který je součástí listu, příkazu před následovat středníkem.
- Při použití v příkazech připravené sp_prepare CTEs chovat stejně jako další příkazy SELECT v PDW. Však použijete-li CTEs jako součást CETAS připravené sp_prepare, chování můžete odložit z SQL serveru a dalších údajů PDW vazby implementovaná pro sp_prepare odlišným způsobem. Pokud vyberte, že odkazy CTE používá nesprávném sloupci, který neexistuje v CTE sp_prepare předá bez zjištění chyby, ale chyba se místo toho během sp_execute vyvolání.

## <a name="recursive-ctes"></a>Rekurzivní CTEs

Rekurzivní CTEs nejsou podporované v SQL datový sklad.  Migraion rekurzivní CTE může být trochu a proces nejlepší je rozdělte do několika kroků. Obvykle můžete použít smyčky a vyplnění dočasné tabulky jako iteraci rekurzivní dočasné dotazy. Jakmile vyplněné dočasné tabulky můžete vrátí data jako sady výsledkem je jedna hodnota. Podobný přístup byla použita k řešení `GROUP BY WITH CUBE` v článku [s funkcí rollup klauzule group by / datové krychle / sady možnosti seskupení][] .

## <a name="unsupported-system-functions"></a>Funkce nepodporované systému

Existují také některé funkce systému, které nejsou podporované. Hlavní ty, které obvykle je možné použít v datech skladování jsou:

- NEWSEQUENTIALID()
- @@NESTLEVEL()
- @@IDENTITY()
- @@ROWCOUNT()
- ROWCOUNT_BIG
- ERROR_LINE()

Některé z těchto problémů můžete odpracovaných kolem.

## <a name="rowcount-workaround"></a>@@ROWCOUNTřešení:

Informace k alternativním řešením chybějící podpory pro @@ROWCOUNT, vytvořit uložené procedury, která bude načítat poslední počet řádků z sys.dm_pdw_request_steps a potom spustit `EXEC LastRowCount` po DML údajů.

```sql
CREATE PROCEDURE LastRowCount AS
WITH LastRequest as 
(   SELECT TOP 1    request_id
    FROM            sys.dm_pdw_exec_requests
    WHERE           session_id = SESSION_ID()
    AND             resource_class IS NOT NULL
    ORDER BY end_time DESC
),
LastRequestRowCounts as
(
    SELECT  step_index, row_count
    FROM    sys.dm_pdw_request_steps
    WHERE   row_count >= 0
    AND     request_id IN (SELECT request_id from LastRequest)
)
SELECT TOP 1 row_count FROM LastRequestRowCounts ORDER BY step_index DESC
;
```

## <a name="next-steps"></a>Další kroky
Úplný seznam všech podporovaných příkazů T SQL naleznete v tématech [Jazyce Transact-SQL][].

<!--Image references-->

<!--Article references-->
[Spojení na aktualizace]: ./sql-data-warehouse-develop-ctas.md#ansi-join-replacement-for-update-statements
[Spojení na odstraní]: ./sql-data-warehouse-develop-ctas.md#ansi-join-replacement-for-delete-statements
[příkaz Sloučit]: ./sql-data-warehouse-develop-ctas.md#replace-merge-statements
[VLOŽENÍ. SPOUŠTĚNÍ]: ./sql-data-warehouse-tables-temporary.md#modularizing-code
[Témata Transact-SQL]: ./sql-data-warehouse-reference-tsql-statements.md

[kurzory]: ./sql-data-warehouse-develop-loops.md
[VYBERTE. DO]: ./sql-data-warehouse-develop-ctas.md#selectinto
[klauzule s funkcí rollup Group by / datové krychle / sady možnosti seskupení]: ./sql-data-warehouse-develop-group-by-options.md
[počet úrovní vnoření za 8]: ./sql-data-warehouse-develop-transactions.md
[aktualizace prostřednictvím zobrazení]: ./sql-data-warehouse-develop-views.md
[použití vyberte pro proměnné přiřazení]: ./sql-data-warehouse-develop-variable-assignment.md
[žádné MAX datový typ pro dynamické řetězců SQL]: ./sql-data-warehouse-develop-dynamic-sql.md

<!--MSDN references-->

<!--Other Web references-->
