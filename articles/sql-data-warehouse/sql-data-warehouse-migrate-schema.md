<properties
   pageTitle="Migrace schématu do SQL datový sklad | Microsoft Azure"
   description="Tipy pro migraci schématu Azure SQL datový sklad k vývoji řešení."
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
   ms.date="08/25/2016"
   ms.author="jrj;barbkess;sonyama"/>

# <a name="migrate-your-schema-to-sql-data-warehouse"></a>Migrace schématu do SQL datový sklad#

Následující souhrny pomohou porozumět rozdíly mezi SQL serveru a SQL datový sklad můžete migrovat databázi.

## <a name="table-migration"></a>Migrace tabulky

Při migraci tabulkách, je vhodné se seznamte s funkce tabulky SQL datový sklad tabulek.  [Přehled tabulky][] je skvělé místo, kde začít.  Tento článek vás seznámí s nejdůležitější co byste měli zvážit při vytváření tabulky například statistiky tabulek, rozdělení, oddílů a indexování.  Také popisuje některé [nepodporované funkce tabulky][] a jejich řešení.

SQL datový sklad podporuje datové typy běžné obchodní.  V tématu [datové typy][] seznam podporované a [nepodporovaných datových typů][].  [Datové typy][] článek obsahuje taky dotazu identifikujícího [nepodporovaných datových typů][].  Při převodu datových typů, ujistěte se, můžete visiové [datový typ doporučené postupy][].

## <a name="next-steps"></a>Další kroky
Po přesunutí schématu databáze úspěšně k SQL datový sklad, přejděte na jednu z těchto článků:

- [Přenést data][]
- [Migrace kódu][]

Další informace o doporučených postupech SQL datový sklad najdete v článku [Doporučené postupy][] .

<!--Image references-->

<!--Article references-->
[Migrace kódu]: ./sql-data-warehouse-migrate-code.md
[Přenést data]: ./sql-data-warehouse-migrate-data.md
[Doporučené postupy]: ./sql-data-warehouse-best-practices.md
[Přehled tabulky]: ./sql-data-warehouse-tables-overview.md
[nepodporované funkce tabulky]: ./sql-data-warehouse-tables-overview.md#unsupported-table-features
[datové typy]: ./sql-data-warehouse-tables-data-types.md
[nepodporované datové typy]: ./sql-data-warehouse-tables-data-types.md#unsupported-data-types
[datový typ doporučené postupy]: ./sql-data-warehouse-tables-data-types.md#data-type-best-practices

<!--MSDN references-->


<!--Other Web references-->
