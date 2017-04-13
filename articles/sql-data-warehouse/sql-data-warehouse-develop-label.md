<properties
   pageTitle="Použití štítky, aby nástroje dotazy v SQL datový sklad | Microsoft Azure"
   description="Tipy pro použití štítky, aby nástroje dotazy v Azure SQL datový sklad k vývoji řešení."
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
   ms.date="06/14/2016"
   ms.author="jrj;barbkess;sonyama"/>

# <a name="use-labels-to-instrument-queries-in-sql-data-warehouse"></a>Použít popisky na nástroj dotazy v SQL datový sklad
SQL datový sklad podporuje koncept s názvem dotaz popisky. Před přechodem do libovolné název hloubkové Dejme prohlédněte příklad jedné:

```sql
SELECT *
FROM sys.tables
OPTION (LABEL = 'My Query Label')
;
```

Tento poslední řádek značky řetězec popisek Moje dotazu pro dotaz. To je vhodné zejména jako popisek je dotaz mít až DMVs. To nabízí nám mechanismus vysledovat problém dotazů a taky k identifikaci průběh prostřednictvím ETL spustit.

Dobrý pojmenování pomáhá skutečně tady. Třeba něco jako "projekt: postup: údajů: komentář" by pomoci identifikují dotazu v mezi všechny kód ovládacího prvku zdroje.

Vyhledávání podle štítku můžete použít následující dotaz, který používá Správa dynamických zobrazení:

```sql
SELECT  *
FROM    sys.dm_pdw_exec_requests r
WHERE   r.[label] = 'My Query Label'
;
```

> [AZURE.NOTE] Je důležité při dotazování zalomit hranatých závorek a dvojité uvozovky kolem nápisem Wordu. Popisek je rezervovaná slova a bude způsobila chybu, pokud nebyl středníkem.


## <a name="next-steps"></a>Další kroky
Další tipy vývoj najdete v tématu [Přehled vývoje][].

<!--Image references-->

<!--Article references-->
[Přehled vývoje]: sql-data-warehouse-overview-develop.md

<!--MSDN references-->

<!--Other Web references-->
