<properties
   pageTitle="Import k SQL databázi ze souboru BACPAC pomocí SqlPackage"
   description="Import databáze databázi Microsoft Azure SQL, migrace databáze, importovat BACPAC, sqlpackage"
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

# <a name="import-to-sql-database-from-a-bacpac-file-using-sqlpackage"></a>Import k SQL databázi ze souboru BACPAC pomocí SqlPackage

> [AZURE.SELECTOR]
- [SSMS](sql-database-cloud-migrate-compatible-import-bacpac-ssms.md)
- [SqlPackage](sql-database-cloud-migrate-compatible-import-bacpac-sqlpackage.md)
- [Azure portálu](sql-database-import.md)
- [Prostředí PowerShell](sql-database-import-powershell.md)

Tento článek ukazuje, jak importovat k SQL databázi ze souboru [BACPAC](https://msdn.microsoft.com/library/ee210546.aspx#Anchor_4) pomocí nástroje [SqlPackage](https://msdn.microsoft.com/library/hh550080.aspx) příkazového řádku. Tento nástroj se dodává s nejnovější verzí systému [SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx) a [SQL Server Data Tools for Visual Studio](https://msdn.microsoft.com/library/mt204009.aspx)nebo stáhnout nejnovější verzi [SqlPackage](https://www.microsoft.com/en-us/download/details.aspx?id=53876) přímo ze služby Stažení softwaru.


> [AZURE.NOTE] Následující postup předpokládá, máte už zřízení databáze SQL serveru, připravte informace o připojení a ověřili, že je kompatibilní zdrojové databáze.

## <a name="import-from-a-bacpac-file-into-azure-sql-database-using-sqlpackage"></a>Import ze souboru BACPAC do databáze SQL Azure pomocí SqlPackage

Pomocí následujících kroků nástroj příkazového řádku [SqlPackage.exe](https://msdn.microsoft.com/library/hh550080.aspx) importovat kompatibilní databáze SQL serveru (nebo databáze Azure SQL) ze souboru BACPAC.

> [AZURE.NOTE] Následující postup předpokládá, máte už zřízení server databáze SQL Azure a mít informace o připojení k dispozici.

1. Otevřete příkazový řádek a změňte adresář obsahující nástroj příkazového řádku sqlpackage.exe: Tento nástroj součástí Visual Studia a SQL Server.
2. Spusťte následující příkaz sqlpackage.exe s těmito argumenty ve vašem prostředí:

    `sqlpackage.exe /Action:Import /tsn:< server_name > /tdn:< database_name > /tu:< user_name > /tp:< password > /sf:< source_file >`

  	| Argument  | Popis  |
  	|---|---|
  	| < název_serveru >  | název serveru cíl  |
  	| < Název_databáze >  | Název cílové databáze  |
  	| < uživatelské_jméno >  | uživatelské jméno v cílovém serveru |
  	| < heslo >  | heslo uživatele  |
  	| < source_file >  | Název souboru a umístění importovaného souboru BACPAC  |

    ![Export dat osy aplikace z nabídky úkoly](./media/sql-database-cloud-migrate/TestForCompatibilityUsingSQLPackage01c.png)

## <a name="next-steps"></a>Další kroky

- [Nejnovější verze SSDT](https://msdn.microsoft.com/library/mt204009.aspx)
- [Nejnovější verzi SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx)

## <a name="additional-resources"></a>Další zdroje informací

- [V12 databáze SQL](sql-database-v12-whats-new.md)
- [Skalární funkce částečně nebo nepodporované funkce](sql-database-transact-sql-information.md)
- [Migrace databáze SQL serveru pomocníka SQL Server migrace](http://blogs.msdn.com/b/ssma/)
