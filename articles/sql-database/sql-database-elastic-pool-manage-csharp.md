<properties
    pageTitle="Sledování a správa fondu pružná databáze s C# | Microsoft Azure"
    description="Správa fondu pružná databáze databáze SQL Azure pomocí C# databáze vývojářské postupy."
    services="sql-database"
    documentationCenter=""
    authors="stevestein"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.topic="article"
    ms.tgt_pltfrm="csharp"
    ms.workload="data-management"
    ms.date="10/04/2016"
    ms.author="sstein"/>

# <a name="monitor-and-manage-an-elastic-database-pool-with-cx23"></a>Sledování a správa fondu pružná databáze s C a #x 23. 

> [AZURE.SELECTOR]
- [Azure portálu](sql-database-elastic-pool-manage-portal.md)
- [Prostředí PowerShell](sql-database-elastic-pool-manage-powershell.md)
- [C#](sql-database-elastic-pool-manage-csharp.md)
- [T-SQL](sql-database-elastic-pool-manage-tsql.md)


Naučte se spravovat [fondu pružná databáze](sql-database-elastic-pool.md) pomocí C a #x 23. 

>[AZURE.NOTE] Mnoho nových funkcí databáze SQL, jsou podporované jenom při použití [Správce prostředků Azure nasazení modelu](../azure-resource-manager/resource-group-overview.md), takže byste měli vždy používat nejnovější **knihovnou správy databáze SQL Azure pro .NET ([dokumenty](https://msdn.microsoft.com/library/azure/mt349017.aspx) | [NuGet balíčku](https://www.nuget.org/packages/Microsoft.Azure.Management.Sql))**. Starší [knihoven založené na klasické nasazení](https://www.nuget.org/packages/Microsoft.WindowsAzure.Management.Sql) podporuje pouze z důvodu zpětné kompatibility, proto doporučujeme používat novější knihovny na základě správce prostředků.

Abyste mohli postupujte podle pokynů v tomto článku, musíte následující položky:

- Pružná fondu (fondu, který chcete spravovat). Vytvoření fondu najdete v tématu [Vytvoření fondu pružná databáze s C#](sql-database-elastic-pool-create-csharp.md).
- Visual Studia. Bezplatné kopii Visual Studiu najdete na stránce [Visual Studio stáhne](https://www.visualstudio.com/downloads/download-visual-studio-vs) .


## <a name="move-a-database-into-an-elastic-pool"></a>Přesunutí databáze do fondu pružná

Samostatné databáze můžete přesouvat nebo oddálit fond.  

    // Retrieve current database properties.

    currentDatabase = sqlClient.Databases.Get("resourcegroup-name", "server-name", "Database1").Database;

    // Configure create or update parameters with existing property values, override those to be changed.
    DatabaseCreateOrUpdateParameters updatePooledDbParameters = new DatabaseCreateOrUpdateParameters()
    {
        Location = currentDatabase.Location,
        Properties = new DatabaseCreateOrUpdateProperties()
        {
            Edition = "Standard",
            RequestedServiceObjectiveName = "ElasticPool",
            ElasticPoolName = "ElasticPool1",
            MaxSizeBytes = currentDatabase.Properties.MaxSizeBytes,
            Collation = currentDatabase.Properties.Collation,
        }
    };

    // Update the database.
    var dbUpdateResponse = sqlClient.Databases.CreateOrUpdate("resourcegroup-name", "server-name", "Database1", updatePooledDbParameters);

## <a name="list-databases-in-an-elastic-pool"></a>Seznam databází ve fondu pružná

Vyhledat všechny databáze do fondu volejte metodu [ListDatabases](https://msdn.microsoft.com/library/microsoft.azure.management.sql.elasticpooloperationsextensions.listdatabases) .

    //List databases in the elastic pool
    DatabaseListResponse dbListInPool = sqlClient.ElasticPools.ListDatabases("resourcegroup-name", "server-name", "ElasticPool1");
    Console.WriteLine("Databases in Elastic Pool {0}", "server-name.ElasticPool1");
    foreach (Database db in dbListInPool)
    {
        Console.WriteLine("  Database {0}", db.Name);
    }

## <a name="change-performance-settings-of-a-pool"></a>Změna nastavení výkonu fondu

Načítejte existující fondu vlastnosti. Upravte hodnoty a spustit metodu CreateOrUpdate.

    var currentPool = sqlClient.ElasticPools.Get("resourcegroup-name", "server-name", "ElasticPool1").ElasticPool;

    // Configure create or update parameters with existing property values, override those to be changed.
    ElasticPoolCreateOrUpdateParameters updatePoolParameters = new ElasticPoolCreateOrUpdateParameters()
    {
        Location = currentPool.Location,
        Properties = new ElasticPoolCreateOrUpdateProperties()
        {
            Edition = currentPool.Properties.Edition,
            DatabaseDtuMax = 50, /* Setting DatabaseDtuMax to 50 limits the eDTUs that any one database can consume */
            DatabaseDtuMin = 10, /* Setting DatabaseDtuMin above 0 limits the number of databases that can be stored in the pool */
            Dtu = (int)currentPool.Properties.Dtu,
            StorageMB = currentPool.Properties.StorageMB,  /* For a Standard pool there is 1 GB of storage per eDTU. */
        }
    };

    newPoolResponse = sqlClient.ElasticPools.CreateOrUpdate("resourcegroup-name", "server-name", "ElasticPool1", newPoolParameters);


## <a name="latency-of-elastic-pool-operations"></a>Latence operací pružná fondu

- Změna eDTUs min za databáze nebo max eDTUs podle databáze obvykle provede v pět minut a méně.
- Chcete-li změnit velikost fondu (eDTUs) doba závisí na souhrnnou velikost všechny databáze ve fondu. Změny průměrná 90 minut nebo méně na 100 GB. Pokud například celkové místo všechny databáze ve fondu je 200 GB, klepněte očekávané latence pro změnu eDTU fondu za fondu je 3 hodiny nebo nižší.




## <a name="additional-resources"></a>Další zdroje informací

- [Kódy chyb SQL pro databáze SQL klientských aplikacích: databáze Chyba připojení i jiných problémů](sql-database-develop-error-messages.md).
- [Databáze SQL](https://azure.microsoft.com/documentation/services/sql-database/)
- [Rozhraní API správy Azure zdroje](https://msdn.microsoft.com/library/azure/dn948464.aspx)
- [Vytvoření nového fondu pružná databáze s C#](sql-database-elastic-pool-create-csharp.md)
- [Pokud má být použita fondu pružná databáze?](sql-database-elastic-pool-guidance.md)
- V tématu [měřítko, s databáze SQL Azure](sql-database-elastic-scale-introduction.md): umožňuje škálování, přesuňte data, pružná databázové nástroje dotazu nebo vytvořit transakce.

