<properties
    pageTitle="Vytvoření nového fondu pružná databáze pomocí prostředí PowerShell | Microsoft Azure"
    description="Zjistěte, jak pomocí Powershellu škálování databáze SQL Azure zdroje vytvořením fond scalable pružná databáze pro správu více databází."
    services="sql-database"
    documentationCenter=""
    authors="srinia"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="powershell"
    ms.workload="data-management"
    ms.date="05/27/2016"
    ms.author="srinia"/>

# <a name="create-a-new-elastic-database-pool-with-powershell"></a>Vytvoření nového fondu pružná databáze pomocí prostředí PowerShell

> [AZURE.SELECTOR]
- [Azure portálu](sql-database-elastic-pool-create-portal.md)
- [Prostředí PowerShell](sql-database-elastic-pool-create-powershell.md)
- [C#](sql-database-elastic-pool-create-csharp.md)


Naučte se vytvářet [fondu pružná databáze](sql-database-elastic-pool.md) pomocí rutin prostředí PowerShell. 

Běžné kódy chyb najdete v článku [kódy chyb SQL pro databáze SQL klientských aplikacích: databáze Chyba připojení i jiných problémů](sql-database-develop-error-messages.md).

> [AZURE.NOTE] Pružná fondů jsou přístupné (GA) ve všech oblastech Azure kromě severní centrální US a Západní Indie, kde je právě ve verzi preview.  GA pružná fondů v těchto oblastech se dozvíte při co nejdříve. Navíc pružná fondů aktuálně nepodporují databází používat [v paměti OLTP nebo analýzy v paměti](sql-database-in-memory.md).


Budete muset používat Azure PowerShell 1.0 nebo vyšší. Podrobné informace najdete v tématu [instalace a konfigurace prostředí PowerShell Azure](../powershell-install-configure.md).

## <a name="create-a-new-pool"></a>Vytvoření nového fondu

Rutinu [New-AzureRmSqlElasticPool](https://msdn.microsoft.com/library/azure/mt619378.aspx) vytvoří novou skupinu. Hodnoty pro eDTU za fondu, min a max Dtus omezené podle hodnot osy služby (základní, standardní nebo premium). V tématu [eDTU a úložiště omezení pro pružná fondů a pružná databází](sql-database-elastic-pool.md#eDTU-and-storage-limits-for-elastic-pools-and-elastic-databases).

    New-AzureRmSqlElasticPool -ResourceGroupName "resourcegroup1" -ServerName "server1" -ElasticPoolName "elasticpool1" -Edition "Standard" -Dtu 400 -DatabaseDtuMin 10 -DatabaseDtuMax 100


## <a name="create-a-new-elastic-database-in-a-pool"></a>Vytvořit novou databázi pružná do fondu

Použít rutinu [New-AzureRmSqlDatabase](https://msdn.microsoft.com/library/azure/mt619339.aspx) a nastavit parametr **ElasticPoolName** do fondu cílové. Přesunutí existující databáze do fondu, najdete v článku [Přesunutí databáze do fondu pružná](sql-database-elastic-pool-manage-powershell.md#Move-a-database-into-an-elastic-pool).

    New-AzureRmSqlDatabase -ResourceGroupName "resourcegroup1" -ServerName "server1" -DatabaseName "database1" -ElasticPoolName "elasticpool1"

## <a name="create-a-pool-and-populate-it-with-multiple-new-databases"></a>Vytvoření fondu a doplníte více nové databáze 

Vytvoření velkého počtu databáze do fondu může trvat čas, kdy provést pomocí portálu nebo rutiny prostředí PowerShell, který současně vytvořit jenom jednu databázi. Pokud chcete automatizovat vytváření do nového fondu, najdete v článku [CreateOrUpdateElasticPoolAndPopulate ](https://gist.github.com/billgib/d80c7687b17355d3c2ec8042323819ae).   

## <a name="example-create-a-pool-using-powershell"></a>Příklad: vytvoření fondu pomocí prostředí PowerShell 

Tento skript vytvoří novou skupinu Azure prostředků a nový server. Po zobrazení výzvy zadejte jména a hesla správce pro nový server (ne Azure přihlašovacích údajů).

    $subscriptionId = '<your Azure subscription id>'
    $resourceGroupName = '<resource group name>'
    $location = '<datacenter location>'
    $serverName = '<server name>'
    $poolName = '<pool name>'
    $databaseName = '<database name>'

    Login-AzureRmAccount
    Set-AzureRmContext -SubscriptionId $subscriptionId

    New-AzureRmResourceGroup -Name $resourceGroupName -Location $location
    New-AzureRmSqlServer -ResourceGroupName $resourceGroupName -ServerName $serverName -Location $location -ServerVersion "12.0"
    New-AzureRmSqlServerFirewallRule -ResourceGroupName $resourceGroupName -ServerName $serverName -FirewallRuleName "rule1" -StartIpAddress "192.168.0.198" -EndIpAddress "192.168.0.199"

    New-AzureRmSqlElasticPool -ResourceGroupName $resourceGroupName -ServerName $serverName -ElasticPoolName $poolName -Edition "Standard" -Dtu 400 -DatabaseDtuMin 10 -DatabaseDtuMax 100

    New-AzureRmSqlDatabase -ResourceGroupName $resourceGroupName -ServerName $serverName -DatabaseName $databaseName -ElasticPoolName $poolName -MaxSizeBytes 10GB



## <a name="next-steps"></a>Další kroky

- [Správa vašeho fondu](sql-database-elastic-pool-manage-powershell.md)
- [Vytvoření pružná úlohy](sql-database-elastic-jobs-overview.md) Pružná úlohy umožňují kontrolovat libovolný počet databází ve fondu skripty T-SQL.
- [Měřítko, s databáze SQL Azure](sql-database-elastic-scale-introduction.md): použití pružná databázové nástroje do škálování.

