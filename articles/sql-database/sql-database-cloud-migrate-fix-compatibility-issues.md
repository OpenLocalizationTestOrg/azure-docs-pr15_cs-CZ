<properties
   pageTitle="Řešení problémů s kompatibilitou databáze SQL serveru před migrací k SQL databázi | Microsoft Azure"
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

# <a name="use-sql-azure-migration-wizard-to-fix-sql-server-database-compatibility-issues-before-migration-to-azure-sql-database"></a>Pomocí Průvodce přenesením SQL Azure řešení Server SQL databáze problémy s kompatibilitou před migrací k databázi SQL Azure

> [AZURE.SELECTOR]
- Pomocí [Průvodce přenesením Azure SQL](sql-database-cloud-migrate-fix-compatibility-issues.md)
- Použití [SSDT](sql-database-cloud-migrate-fix-compatibility-issues-ssdt.md)
- Použití [SSMS](sql-database-cloud-migrate-fix-compatibility-issues-ssms.md)

V tomto článku se naučíte rozpoznávání a oprava problémů kompatibility databáze SQL serveru pomocí Průvodce přenesením SQL Azure před migrací k databázi SQL Azure.

## <a name="using-sql-azure-migration-wizard"></a>Pomocí Průvodce přenesením SQL Azure

Skript T-SQL negeneruje kompatibilní zdrojové databáze pomocí nástroje CodePlex [Průvodce přenesením SQL Azure](http://sqlazuremw.codeplex.com/) . Tento skript potom transformovat pomocí Průvodce pro zajištění kompatibility s databáze SQL. Pak připojíte k databázi SQL Azure spustit skript. Tento nástroj také analyzuje sledování soubory k určení problémy s kompatibilitou. Skript můžete vytvořit s pouze schéma nebo mohou obsahovat data ve formátu BCP. Další si přečtěte následující dokumentaci, včetně podrobné pokyny najdete na webu CodePlex na [Průvodce přenesením SQL Azure](http://sqlazuremw.codeplex.com/).  

 ![Diagram SAMW migrace](./media/sql-database-cloud-migrate/02SAMWDiagram.png)

  > [AZURE.NOTE] Ne všechny kompatibilní schéma, které jsou zjištěny průvodcem lze opravit tak, že předdefinované transformace. Nekompatibilní skript, který nelze adresovaná vznikly jako chybné, s komentáři vložené do generovaný skript. Zjištění mnoho chyb Visual Studio nebo SQL Server Management Studio projít a vyřešit podle všechny chyby, které nelze opravit pomocí Průvodce přenesením SQL serveru.

## <a name="next-steps"></a>Další kroky

- [Nejnovější verze SSDT](https://msdn.microsoft.com/library/mt204009.aspx)
- [Nejnovější verzi SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx)
- [Migrace kompatibilní databáze SQL serveru k SQL databázi](sql-database-cloud-migrate.md#migrate-a-compatible-sql-server-database-to-sql-database)

## <a name="additional-resources"></a>Další zdroje informací

- [V12 databáze SQL](sql-database-v12-whats-new.md)
- [Skalární funkce částečně nebo nepodporované funkce](sql-database-transact-sql-information.md)
- [Migrace databáze SQL serveru pomocníka SQL Server migrace](http://blogs.msdn.com/b/ssma/)
