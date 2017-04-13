<properties
   pageTitle="Určení kompatibility databáze SQL pomocí SqlPackage.exe | Microsoft Azure"
   description="Databázi Microsoft Azure SQL, migrace databáze, databáze SQL kompatibilita, SqlPackage"
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

# <a name="determine-sql-database-compatibility-using-sqlpackageexe"></a>Určení kompatibility databáze SQL pomocí SqlPackage.exe

> [AZURE.SELECTOR]
- [SSDT](sql-database-cloud-migrate-fix-compatibility-issues-ssdt.md)
- [SqlPackage](sql-database-cloud-migrate-determine-compatibility-sqlpackage.md)
- [SSMS](sql-database-cloud-migrate-determine-compatibility-ssms.md)
- [Poradce při upgradu](http://www.microsoft.com/download/details.aspx?id=48119)
- [SAMW](sql-database-cloud-migrate-fix-compatibility-issues.md)

V tomto článku se dozvíte zjistěte, jestli databázi SQL serveru není kompatibilní migrace k SQL databázi pomocí nástroje [SqlPackage](https://msdn.microsoft.com/library/hh550080.aspx) příkazový řádek.

## <a name="using-sqlpackageexe"></a>Použití SqlPackage.exe

1. Otevřete příkazový řádek a změňte adresář, který obsahuje nejnovější verzi sqlpackage.exe. Tento nástroj se dodává s nejnovější verzí systému [SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx) a [SQL Server Data Tools for Visual Studio](https://msdn.microsoft.com/library/mt204009.aspx)nebo stáhnout nejnovější verzi [SqlPackage](https://www.microsoft.com/en-us/download/details.aspx?id=53876) přímo ze služby Stažení softwaru.
2. Spusťte následující příkaz SqlPackage s těmito argumenty ve vašem prostředí:

    "sqlpackage.exe /Action:Export /ssn: < název_serveru > /sdn: < Název_databáze > /tf: /p:TableData < target_file > = < schema_name.table_name >>< Výstupní_soubor > 2 > & 1

  	| Argument  | Popis  |
  	|---|---|
  	| < název_serveru >  | název serveru zdroje  |
  	| < Název_databáze >  | Název zdrojové databáze  |
  	| < target_file >  | název a umístění souboru BACPAC souborů  |
  	| < schema_name.table_name >  | tabulek, pro které dat výstupu do cílového souboru  |
  	| < Výstupní_soubor >  | Název souboru a umístění ve výstupním souboru s chybami, pokud existuje  |

    Důvod /p:TableName argument je, že chceme jenom testování pro kompatibilitu databáze pro export V12 databáze SQL Azure, spíše než exportovat data ze všech tabulek. Bohužel export argument sqlpackage.exe podporuje extrahování nula tabulek. Budete muset zadat alespoň jednu tabulku, například malé jedné tabulky. < Výstupní_soubor > obsahuje zprávu o chybách. "> 2 > & 1" řetězec potrubí standardní výstup a výpočet standardní chyby vrácené spuštění příkazu zadaný výstupní soubor.

    ![Export dat osy aplikace z nabídky úkoly](./media/sql-database-cloud-migrate/TestForCompatibilityUsingSQLPackage01.png)

3. Otevřete ve výstupním souboru a kontrola kompatibility chyby, případně. 

    ![Export dat osy aplikace z nabídky úkoly](./media/sql-database-cloud-migrate/TestForCompatibilityUsingSQLPackage02.png)

## <a name="next-steps"></a>Další kroky

- [Nejnovější verze SSDT](https://msdn.microsoft.com/library/mt204009.aspx)
[nejnovější verzi SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx)
- [Řešení problémů s kompatibilitou migrace databáze](sql-database-cloud-migrate.md#fix-database-migration-compatibility-issues)
- [Migrace kompatibilní databáze SQL serveru k SQL databázi](sql-database-cloud-migrate.md#migrate-a-compatible-sql-server-database-to-sql-database)

## <a name="additional-resources"></a>Další zdroje informací

- [V12 databáze SQL](sql-database-v12-whats-new.md)
- [Skalární funkce částečně nebo nepodporované funkce](sql-database-transact-sql-information.md)
- [Migrace databáze SQL serveru pomocníka SQL Server migrace](http://blogs.msdn.com/b/ssma/)
