<properties
   pageTitle="Export databáze SQL serveru do souboru BACPAC pomocí SqlPackage | Microsoft Azure"
   description="Databázi Microsoft Azure SQL, migrace databáze export databáze, export do souboru BACPAC, sqlpackage"
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

# <a name="export-a-sql-server-database-to-a-bacpac-file-using-sqlpackage"></a>Export do BACPAC soubor pomocí SqlPackage databáze SQL serveru

> [AZURE.SELECTOR]
- [SSMS](sql-database-cloud-migrate-compatible-export-bacpac-ssms.md)
- [SqlPackage](sql-database-cloud-migrate-compatible-export-bacpac-sqlpackage.md)

Tento článek ukazuje, jak exportovat do souboru [BACPAC](https://msdn.microsoft.com/library/ee210546.aspx#Anchor_4) pomocí nástroje příkazového řádku [SqlPackage](https://msdn.microsoft.com/library/hh550080.aspx) databáze SQL serveru. Tento nástroj se dodává s nejnovější verzí systému [SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx) a [SQL Server Data Tools for Visual Studio](https://msdn.microsoft.com/library/mt204009.aspx)nebo stáhnout nejnovější verzi [SqlPackage](https://www.microsoft.com/en-us/download/details.aspx?id=53876) přímo ze služby Stažení softwaru.

1. Otevřete příkazový řádek a změňte adresář obsahující nástroj příkazového řádku sqlpackage.exe: Tento nástroj součástí Visual Studia a SQL Server. Vyhledejte cestu ve vašem prostředí pomocí funkce ve vašem počítači.
2. Spusťte následující příkaz sqlpackage.exe s těmito argumenty ve vašem prostředí:

    "sqlpackage.exe /Action:Export /ssn: < název_serveru > /sdn: < Název_databáze > /tf: < target_file >

  	| Argument  | Popis  |
  	|---|---|
  	| < název_serveru >  | název serveru zdroje  |
  	| < Název_databáze >  | Název zdrojové databáze  |
  	| < target_file >  | název a umístění souboru BACPAC souborů  |

    ![Export dat osy aplikace z nabídky úkoly](./media/sql-database-cloud-migrate/TestForCompatibilityUsingSQLPackage01b.png)

## <a name="next-steps"></a>Další kroky

- [Nejnovější verze SSDT](https://msdn.microsoft.com/library/mt204009.aspx)
- [Nejnovější verzi SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx)
- [Import BACPAC k databázi SQL Azure pomocí SSMS](sql-database-cloud-migrate-compatible-import-bacpac-ssms.md)
- [Import BACPAC SqlPackage databáze Azure SQL](sql-database-cloud-migrate-compatible-import-bacpac-sqlpackage.md)
- [Import BACPAC do portálu Azure databáze SQL Azure](sql-database-import.md)
- [Import BACPAC prostředí PowerShell databáze Azure SQL](sql-database-import-powershell.md)

## <a name="additional-resources"></a>Další zdroje informací

- [V12 databáze SQL](sql-database-v12-whats-new.md)
- [Skalární funkce částečně nebo nepodporované funkce](sql-database-transact-sql-information.md)
- [Migrace databáze SQL serveru pomocníka SQL Server migrace](http://blogs.msdn.com/b/ssma/)
