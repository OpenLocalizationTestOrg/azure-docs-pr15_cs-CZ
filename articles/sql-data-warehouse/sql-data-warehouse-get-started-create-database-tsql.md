<properties
   pageTitle="Vytvořit datový sklad SQL s TSQL | Microsoft Azure"
   description="Naučte se vytvářet datový sklad SQL Azure pomocí TSQL"
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="lodipalm"
   manager="barbkess"
   editor=""
   tags="azure-sql-data-warehouse"/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="08/24/2016"
   ms.author="lodipalm;barbkess;sonyama"/>

# <a name="create-a-sql-data-warehouse-database-by-using-transact-sql-tsql"></a>Vytvoření databáze SQL datový sklad pomocí jazyce Transact-SQL TSQL)

> [AZURE.SELECTOR]
- [Azure portálu](sql-data-warehouse-get-started-provision.md)
- [TSQL](sql-data-warehouse-get-started-create-database-tsql.md)
- [Prostředí PowerShell](sql-data-warehouse-get-started-provision-powershell.md)

V tomto článku se dozvíte, jak vytvořit datový sklad SQL pomocí T-SQL.

## <a name="prerequisites"></a>Zjistit předpoklady pro

Začněte tím, budete potřebovat: 

- **Účet Azure**: navštivte web [Bezplatnou zkušební verzi Azure][] nebo [MSDN Azure přeplatky][] vytvořte účet.
- **Server Azure SQL**: Podrobnosti najdete v části [vytvořit logické server databáze SQL Azure pomocí portálu Azure][] nebo [vytvořit logické server databáze SQL Azure pomocí prostředí PowerShell][] Další.
- **Pole Skupina zdroje**: používají stejné skupiny prostředků serveru Azure SQL nebo najdete v článku [jak vytvářet skupiny zdrojů][].
- **Prostředí provést T-SQL**: můžete použít [Visual Studio][Installing Visual Studio and SSDT], [sqlcmd][]nebo [SSMS][] provést T-SQL.

> [AZURE.NOTE] Vytváření datový sklad SQL může mít za následek novou fakturaci službu.  Další informace o cenách najdete v článku [SQL datový sklad ceny][] .

## <a name="create-a-database-with-visual-studio"></a>Vytvoření databáze pomocí aplikace Visual Studio

Pokud začínáte Visual Studiu, přečtěte si článek [Dotazu Azure SQL datový sklad (Visual Studio)][].  Začít, spusťte Průzkumníka objektu SQL serveru ve Visual Studiu a připojení k serveru, který bude hostitelem vaší databáze SQL datový sklad.  Po připojení, můžete vytvořit datový sklad SQL spuštěním následujícího příkazu SQL s **hlavní** databází.  Tento příkaz vytvoří databázi MySqlDwDb s cíle DW400 služby a povolit databáze, kterou chcete zvětšit maximální velikosti doručovaných 10 TB.

```sql
CREATE DATABASE MySqlDwDb COLLATE SQL_Latin1_General_CP1_CI_AS (EDITION='datawarehouse', SERVICE_OBJECTIVE = 'DW400', MAXSIZE= 10240 GB);
```

## <a name="create-a-database-with-sqlcmd"></a>Vytvoření databáze pomocí sqlcmd

Můžete taky můžete spuštěním stejný příkaz s sqlcmd následující příkazového řádku.

```sql
sqlcmd -S <Server Name>.database.windows.net -I -U <User> -P <Password> -Q "CREATE DATABASE MySqlDwDb COLLATE SQL_Latin1_General_CP1_CI_AS (EDITION='datawarehouse', SERVICE_OBJECTIVE = 'DW400', MAXSIZE= 10240 GB)"
```

Výchozí řazení, pokud není zadán je ZKOMPLETOVAT SQL_Latin1_General_CP1_CI_AS.  `MAXSIZE` Může být až 250 GB 240 TB.  `SERVICE_OBJECTIVE` Může být mezi DW100 a DW2000 [DWU][].  Seznam všech platných hodnot najdete v dokumentaci MSDN pro [Vytvoření databáze][].  MAXSIZE a SERVICE_OBJECTIVE bude možné měnit pomocí příkazu T SQL [Vlastnosti databáze][] .  Po vytvoření nelze změnit řazení databáze.   Opatrně při změně SERVICE_OBJECTIVE jako měněné DWU způsobí restart služby, které zruší všechny dotazy v letech.  Změna MAXSIZE nerestartuje služby jako je právě operace jednoduché metadat.

## <a name="next-steps"></a>Další kroky

Po dokončení SQL datový sklad zřizování se můžete [načtení ukázkových dat][] nebo najdete v článku Jak [vývoj][], [načtení][]nebo [migrace][].

<!--Article references-->
[DWU]: ./sql-data-warehouse-overview-what-is.md#data-warehouse-units
[how to create a SQL Data Warehouse from the Azure portal]: sql-data-warehouse-get-started-provision.md
[Dotaz Azure SQL datový sklad (Visual Studio)]: sql-data-warehouse-query-visual-studio.md
[migrace]: sql-data-warehouse-overview-migrate.md
[Můžete vyvíjet]: sql-data-warehouse-overview-develop.md
[načtení]: sql-data-warehouse-overview-load.md
[načtení ukázkových dat]: sql-data-warehouse-load-sample-databases.md
[Vytvoření logické serveru databáze SQL Azure pomocí portálu Azure]: ../sql-database/sql-database-get-started.md#create-an-azure-sql-database-logical-server
[Vytvoření logické serveru databáze SQL Azure pomocí prostředí PowerShell]: ../sql-database/sql-database-get-started-powershell.md#database-setup-create-a-resource-group-server-and-firewall-rule
[jak vytvořit skupinu zdrojů]: ../resource-group-template-deploy-portal.md#create-resource-group
[Installing Visual Studio and SSDT]: sql-data-warehouse-install-visual-studio.md
[SqlCmd]: sql-data-warehouse-get-started-connect-sqlcmd.md

<!--MSDN references--> 
[VYTVOŘENÍ DATABÁZE]: https://msdn.microsoft.com/library/mt204021.aspx
[PŘÍKAZ ALTER DATABÁZE]: https://msdn.microsoft.com/library/mt204042.aspx
[SSMS]: https://msdn.microsoft.com/library/mt238290.aspx

<!--Other Web references-->
[SQL datový sklad ceny]: https://azure.microsoft.com/pricing/details/sql-data-warehouse/
[Bezplatnou zkušební verzi Azure]: https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F
[MSDN Azure kreditů]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F
