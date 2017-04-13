<properties
   pageTitle="Přiřazení proměnných v SQL datový sklad | Microsoft Azure"
   description="Tipy pro přiřazení proměnné jazyce Transact-SQL Azure SQL datový sklad k vývoji řešení."
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

# <a name="assign-variables-in-sql-data-warehouse"></a>Přiřazení proměnných v SQL datový sklad
Proměnné v SQL datový sklad se nastavují pomocí `DECLARE` údajů nebo `SET` údajů.

Všechny následující způsoby dokonale platnou hodnotu proměnné:

## <a name="setting-variables-with-declare"></a>Nastavení proměnné DECLARE

Inicializace proměnných s DECLARE je taková flexibilní způsoby, jak nastavit hodnotu proměnné v SQL datový sklad.

```sql
DECLARE @v  int = 0
;
```

DECLARE můžete taky nastavit proměnnou více než jeden po druhém. Nelze použít `SELECT` nebo `UPDATE` můžete to udělat:

```sql
DECLARE @v  INT = (SELECT TOP 1 c_customer_sk FROM Customer where c_last_name = 'Smith')
,       @v1 INT = (SELECT TOP 1 c_customer_sk FROM Customer where c_last_name = 'Jones')
;
```

Nelze inicializaci a v stejný příkaz DECLARE použít proměnnou. **Ke znázornění čárky následujícím příkladu je Nepovoleno jako** @p1 inicializován i používán stejný příkaz DECLARE. Bude výsledkem chyba.

```sql
DECLARE @p1 int = 0
,       @p2 int = (SELECT COUNT (*) FROM sys.types where is_user_defined = @p1 )
;
```

## <a name="setting-values-with-set"></a>Nastavení hodnot pomocí sady
Nastavení je velmi metodu pro nastavení jedné proměnné.

Všechny následující příklady způsoby platné nastavení proměnnou se SADOU:

```sql
SET     @v = (Select max(database_id) from sys.databases);
SET     @v = 1;
SET     @v = @v+1;
SET     @v +=1;
```

Jednou proměnnou lze nastavit se SADOU pouze po druhém. Jak je vidět nad jsou složeného operátory povolená.

## <a name="limitations"></a>Omezení
Výběr nebo aktualizace nelze použít pro proměnné přiřazení.


## <a name="next-steps"></a>Další kroky
Další tipy vývoj najdete v tématu [Přehled vývoje][].

<!--Image references-->

<!--Article references-->
[Přehled vývoje]: sql-data-warehouse-overview-develop.md

<!--MSDN references-->

<!--Other Web references-->
