<properties 
    pageTitle="Vytvoření nebo přesunutí databáze Azure SQL do fondu pružná pomocí T-SQL | Microsoft Azure" 
    description="Vytvoření databáze Azure SQL ve fondu pružná pomocí T-SQL. Nebo přejdete datbase a odhlášení z fondů pomocí T-SQL." 
    services="sql-database" 
    documentationCenter="" 
    authors="srinia" 
    manager="jhubbard" 
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.topic="article"
    ms.tgt_pltfrm="NA"
    ms.workload="data-management" 
    ms.date="05/27/2016"
    ms.author="srinia"/>

# <a name="monitor-and-manage-an-elastic-database-pool-with-transact-sql"></a>Sledování a správa fondu pružná databáze se jazyce Transact-SQL  

> [AZURE.SELECTOR]
- [Azure portálu](sql-database-elastic-pool-manage-portal.md)
- [Prostředí PowerShell](sql-database-elastic-pool-manage-powershell.md)
- [C#](sql-database-elastic-pool-manage-csharp.md)
- [T-SQL](sql-database-elastic-pool-manage-tsql.md)

Pomocí příkazů [Create Database (databáze SQL Azure)](https://msdn.microsoft.com/library/dn268335.aspx) a [Změnit Database(Azure SQL Database)](https://msdn.microsoft.com/library/mt574871.aspx) vytvořte a přesuňte databází do a od pružná fondů. Pružná fondu musí existovat před použitím tyto příkazy. Tyto příkazy vliv pouze databáze. Vytvoření nové skupiny a nastavení vlastnosti fondu (například min a max eDTUs) nelze změnit s příkazy T-SQL.

## <a name="create-a-new-database-in-an-elastic-pool"></a>Vytvoření nové databáze ve fondu pružná
Pomocí příkazu vytvořit databázi s parametrem SERVICE_OBJECTIVE.   

    CREATE DATABASE db1 ( SERVICE_OBJECTIVE = ELASTIC_POOL (name = [S3M100] ));
    -- Create a database named db1 in a pool named S3M100.

Všechny databáze ve fondu pružná dědí vlastnosti vrstvy služeb pružná fondu (základní, standardní, Premium). 


## <a name="move-a-database-between-elastic-pools"></a>Přesunutí databáze mezi pružná fondy
Příkaz Vlastnosti databáze s změnit a nastavení služby\_cíle možnost jako OHEBNÉ\_fondu; Zadejte název název cílové skupiny.

    ALTER DATABASE db1 MODIFY ( SERVICE_OBJECTIVE = ELASTIC_POOL (name = [PM125] ));
    -- Move the database named db1 to a pool named P1M125  

## <a name="move-a-database-into-an-elastic-pool"></a>Přesunutí databáze do fondu pružná 
Příkaz Vlastnosti databáze s změnit a nastavení služby\_cíle možnost jako ELASTIC_POOL; Zadejte název název cílové skupiny.

    ALTER DATABASE db1 MODIFY ( SERVICE_OBJECTIVE = ELASTIC_POOL (name = [S3100] ));
    -- Move the database named db1 to a pool named S3100.

## <a name="move-a-database-out-of-an-elastic-pool"></a>Přesunutí databáze z fondu pružná
Použití příkazu Vlastnosti databáze a nastavte SERVICE_OBJECTIVE na jednu úroveň výkonnosti (S0, S1 atd.).

    ALTER DATABASE db1 MODIFY ( SERVICE_OBJECTIVE = 'S1');
    -- Changes the database into a stand-alone database with the service objective S1.

## <a name="list-databases-in-an-elastic-pool"></a>Seznam databází ve fondu pružná
Použití [sys.database\_služby \_cíle zobrazení](https://msdn.microsoft.com/library/mt712619) zobrazíte všechny databáze ve fondu pružná. Přihlaste se na hlavní databáze k vytvoření dotazu v zobrazení.

    SELECT d.name, slo.*  
    FROM sys.databases d 
    JOIN sys.database_service_objectives slo  
    ON d.database_id = slo.database_id
    WHERE elastic_pool_name = 'MyElasticPool'; 

## <a name="get-resource-usage-data-for-a-pool"></a>Načtení dat využití prostředků pro fond

Použití [sys.elastic\_fondu \_zdroje \_stat zobrazení](https://msdn.microsoft.com/library/mt280062.aspx) zkoumat statistiky využívání zdroje pružná fondu na serveru logické. Přihlaste se na hlavní databáze k vytvoření dotazu v zobrazení.

    SELECT * FROM sys.elastic_pool_resource_stats 
    WHERE elastic_pool_name = 'MyElasticPool'
    ORDER BY end_time DESC;

## <a name="get-resource-usage-for-an-elastic-database"></a>Získání využití prostředků pro databázi pružná

Použití [sys.dm\_ db\_ zdroje\_stat zobrazení](https://msdn.microsoft.com/library/dn800981.aspx) nebo [sys.resource \_stat zobrazení](https://msdn.microsoft.com/library/dn269979.aspx) zkoumat statistiky využívání zdroje databáze ve fondu pružná. Tento postup je podobný na dotazy týkající se používání zdrojů pro jednu databázi.

## <a name="next-steps"></a>Další kroky

Po vytvoření fondu pružná databáze, můžete spravovat pružná databází ve fondu vytvořením pružná úlohy. Pružná úlohy usnadnění pracovního skriptů libovolný počet databází ve fondu T-SQL. Další informace najdete v tématu [Přehled úloh pružná databáze](sql-database-elastic-jobs-overview.md). 

V tématu [měřítko, s databáze SQL Azure](sql-database-elastic-scale-introduction.md): umožňuje škálování, přesuňte data, pružná databázové nástroje dotazu nebo vytvořit transakce.
