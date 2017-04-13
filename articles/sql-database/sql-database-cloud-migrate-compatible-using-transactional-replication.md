<properties
   pageTitle="Migrace k SQL databázi pomocí transakční replikace | Microsoft Azure"
   description="Import databáze, databázi Microsoft Azure SQL, migrace databáze transakční replikace"
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
   ms.date="08/23/2016"
   ms.author="carlrab"/>

# <a name="migrate-sql-server-database-to-azure-sql-database-using-transactional-replication"></a>Migrace databáze SQL serveru k databázi SQL Azure pomocí transakční replikace

> [AZURE.SELECTOR]
- [Průvodce migrací SSMS](sql-database-cloud-migrate-compatible-using-ssms-migration-wizard.md)
- [Export do souboru BACPAC](sql-database-cloud-migrate-compatible-export-bacpac-ssms.md)
- [Importovat ze souboru BACPAC](sql-database-cloud-migrate-compatible-import-bacpac-ssms.md)
- [Transakční replikace](sql-database-cloud-migrate-compatible-using-transactional-replication.md)

V tomto článku se dozvíte migrace kompatibilní databáze SQL serveru k databázi SQL Azure s minimálními prostoj při použití transakční replikace SQL serveru.

## <a name="understanding-the-transactional-replication-architecture"></a>Principy transakční replikace architektura

Pokud si nemůžete dovolit databázi SQL serveru odebrání výrobní během migrace se vyskytuje, můžete jako migrace řešení transakční replikace SQL serveru. Použít toto řešení, nakonfigurujete jako odběratele instanci systému SQL Server místní, která chcete migrovat databázi SQL Azure. Místním transakční replikace distributor synchronizuje data z místní databázi k synchronizaci (vydavatel) během nové transakce pokračovat dojít. 

Můžete taky transakční replikace migrace podmnožinu místní databázi. Publikace, které jste k databázi SQL Azure může být omezená na podmnožinu tabulek replikace databáze. Pro každou tabulku replikace můžete omezit data, která chcete podmnožinu řádky a/nebo podmnožinu sloupce.

S transakční replikace všechny změny dat nebo schématu objeví v databázi SQL Azure. Po dokončení synchronizace a budete připravení k migraci, změňte připojovací řetězec z aplikací tak, aby ukazovaly na databázi SQL Azure. Jakmile transakční replikace vyprázdní změny doleva na místní databázi a všechny po něm aplikací přejděte na Azure DB, pak odinstalujete transakční replikace. Databáze SQL Azure je teď systému výroby.

 ![SeedCloudTR diagram](./media/sql-database-cloud-migrate/SeedCloudTR.png)

## <a name="transactional-replication-requirements"></a>Požadavky na transakční replikace

Transakční replikace je technologie předdefinované a integrované se serverem SQL Server od SQL Server 6.5. Je zdokonaleným a osvědčené technologie, že většina DBAs vědět, se kterým mají prostředí. S [SQL serveru 2016](https://www.microsoft.com/en-us/cloud-platform/sql-server)je možné ji nakonfigurovat jako [transakční replikace odběratele](https://msdn.microsoft.com/library/mt589530.aspx) do publikace místní databázi SQL Azure. Zkušenosti, můžete získat nastavení z Management Studio se stejně jako při nastavení transakční replikace odběratele na místního serveru. Podpora pro tento scénář je podporované, když vydavatele a distributor jsou alespoň jedno z následujících verzí systému SQL Server:

 - SQL Server 2016 a vyšší 
 - SQL Server 2014 SP1 CU3 a vyšší
 - SQL Server 2014 RTM CU10 a vyšší
 - SQL Server 2012 SP2 CU8 a vyšší
 - SQL Server 2012 SP3 a vyšší


> [AZURE.IMPORTANT] Použijte nejnovější verzi SQL Server Management Studio zůstat synchronizované s aktualizacemi a Microsoft Azure SQL databáze. Starší verze aplikace SQL Server Management Studio nemůže nastavit SQL databáze jako účastníka. [Aktualizace SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx).


## <a name="next-steps"></a>Další kroky

- [Nejnovější verzi SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx)
- [Nejnovější verze SSDT](https://msdn.microsoft.com/library/mt204009.aspx)
- [SQL Server 2016](https://www.microsoft.com/en-us/cloud-platform/sql-server)

## <a name="additional-resources"></a>Další zdroje informací

- [Transakční replikace](https://msdn.microsoft.com/library/mt589530.aspx)
- [V12 databáze SQL](sql-database-v12-whats-new.md)
- [Skalární funkce částečně nebo nepodporované funkce](sql-database-transact-sql-information.md)
- [Migrace databáze SQL serveru pomocníka SQL Server migrace](http://blogs.msdn.com/b/ssma/)
