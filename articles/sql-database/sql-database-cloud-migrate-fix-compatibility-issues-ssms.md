<properties
   pageTitle="Řešení problémů kompatibility databáze SQL serveru pomocí SQL Server Managment Studio před migrací k SQL databázi | Microsoft Azure"
   description="Databázi Microsoft Azure SQL, migrace databáze, kompatibilita, Průvodce přenesením SQL Azure"
   services="sql-database"
   documentationCenter=""
   authors="CarlRabeler"
   manager="jhubbard"
   editor=""/>

<tags
   ms.service="sql-database"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="sqldb-migrate"
   ms.date="08/24/2016"
   ms.author="carlrab"/>

# <a name="fix-sql-server-database-compatibility-issues-using-sql-server-management-studio-before-migration-to-sql-database"></a>Řešení problémů kompatibility databáze SQL serveru pomocí SQL Server Management Studio před migrací k SQL databázi

> [AZURE.SELECTOR]
- Pomocí [Průvodce přenesením Azure SQL](sql-database-cloud-migrate-fix-compatibility-issues.md)
- Použití [SSDT](sql-database-cloud-migrate-fix-compatibility-issues-ssdt.md)
- Použití [SSMS](sql-database-cloud-migrate-fix-compatibility-issues-ssms.md)

Zkušení uživatelé můžete opravit problémy kompatibility databáze SQL serveru pomocí SQL Server Management Studio před migrací k databázi SQL Azure.


> [AZURE.IMPORTANT] Doporučujeme vždy používat nejnovější verzi Management Studio zůstat synchronizované s aktualizacemi a Microsoft Azure SQL databáze. [Aktualizace SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx).


## <a name="using-sql-server-management-studio"></a>Použití SQL Server Management Studio

Pomocí SQL Server Management Studio opravit problémy s kompatibilitou příkazy různých jazyce Transact-SQL, například **Vlastnosti databáze**. Tento způsob především pro pokročilé uživatele, které jsou vám funguje jazyce Transact-SQL na databázi live. Jinak doporučujeme použít SSDT. 



## <a name="next-steps"></a>Další kroky

- [Nejnovější verze SSDT](https://msdn.microsoft.com/library/mt204009.aspx)
- [Nejnovější verzi SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx)
- [Migrace kompatibilní databáze SQL serveru k SQL databázi](sql-database-cloud-migrate.md#migrate-a-compatible-sql-server-database-to-sql-database)

## <a name="additional-resources"></a>Další zdroje informací

- [V12 databáze SQL](sql-database-v12-whats-new.md)
- [Skalární funkce částečně nebo nepodporované funkce](sql-database-transact-sql-information.md)
- [Migrace databáze SQL serveru pomocníka SQL Server migrace](http://blogs.msdn.com/b/ssma/)
