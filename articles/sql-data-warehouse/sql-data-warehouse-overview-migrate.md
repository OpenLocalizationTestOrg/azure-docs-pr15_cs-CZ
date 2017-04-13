<properties
   pageTitle="Migrace řešení pro systém SQL Data Warehouse | Microsoft Azure"
   description="Migrace pokyny pro dosažení řešení Azure SQL datový sklad platformu."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="barbkess"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="08/30/2016"
   ms.author="barbkess;jrj;sonyama"/>

# <a name="migrate-your-solution-to-sql-data-warehouse"></a>Migrace řešení pro systém SQL Data Warehouse

SQL datový sklad je distribuované databáze systému, který elastically upraví podle svých potřeb. Abyste zachovali výkon a měřítko, jsou do SQL datový sklad implementovaná ne všechny funkce v serveru SQL Server. V následujících tématech migrace klepněte na některé klíčové faktory pro migraci řešení pro systém SQL Data Warehouse. Navrhování sklady dat pro měřítko uvádí různé návrhu: vzorky a tak tradiční přístupů nejsou vždy maximum. Proto můžete narazit, přizpůsobení existujícího řešení zajišťuje, že můžete využít úplný platformu distribuované poskytovanou SQL datový sklad.

Také je důležité si zapamatovat, je platformy založené na Microsoft Azure SQL datový sklad. Proto část migraci mohou také obsahovat přenos dat do cloudu. Přenos dat je společnému předmětu vlastní vpravo a pečlivě považovat za; zejména jako svazky zvýšení. Přenos dat a načítání dat jsou samostatné témata.

## <a name="migration-guidance"></a>Pokyny pro migraci

Zkontrolujte, jestli že máte číst pomocí těchto článků, abyste zajistili některé rozdíly produktu a základy srozumitelný před věnovat migraci.

- [Migrace schématu][]
- [Přenést data][]
- [Migrace kódu][]

## <a name="next-steps"></a>Další kroky

KOČKA (zákazník poradní týmu) má i některé skvělé SQL datový sklad pokyny, které publikují prostřednictvím blogy.  Podívejte se na jejich článek [migraci dat Azure SQL datový sklad v praxi][] Další informace o migraci.

<!--Image references-->

<!--Article references-->
[Migrace schématu]: sql-data-warehouse-migrate-schema.md
[Přenést data]: sql-data-warehouse-migrate-data.md
[Migrace kódu]: sql-data-warehouse-migrate-code.md


<!--MSDN references-->


<!--Other Web references-->
[Migraci dat Azure SQL datový sklad v praxi]: https://blogs.msdn.microsoft.com/sqlcat/2016/08/18/migrating-data-to-azure-sql-data-warehouse-in-practice/
