<properties 
    pageTitle="Zkopírujte databázi Azure SQL pomocí prostředí PowerShell | Microsoft Azure" 
    description="Vytvoření kopie databáze Azure SQL pomocí prostředí PowerShell" 
    services="sql-database"
    documentationCenter=""
    authors="stevestein"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.date="09/08/2016"
    ms.author="sstein"
    ms.workload="data-management"
    ms.topic="article"
    ms.tgt_pltfrm="NA"/>


# <a name="copy-an-azure-sql-database-using-powershell"></a>Zkopírujte databázi Azure SQL pomocí prostředí PowerShell


> [AZURE.SELECTOR]
- [Základní informace](sql-database-copy.md)
- [Azure portálu](sql-database-copy-portal.md)
- [Prostředí PowerShell](sql-database-copy-powershell.md)
- [T-SQL](sql-database-copy-transact-sql.md)

Tento článek ukazuje, jak můžete kopírovat databáze SQL pomocí prostředí PowerShell na stejný server, na jiný server nebo zkopírujte databáze do [fondu pružná databáze](sql-database-elastic-pool.md). Kopírování databázi používá rutinu [New-AzureRmSqlDatabaseCopy](https://msdn.microsoft.com/library/mt603644.aspx) . 


Tento článek, je potřeba k provedení těchto věcí:

- Databáze Azure SQL (databáze zkopírovat). Pokud nemáte SQL databáze, vytvořte jednu kroků v tomto článku: [vytvoření první databáze SQL Azure](sql-database-get-started.md).
- Nejnovější verzi Azure Powershellu. Podrobné informace najdete v tématu [instalace a konfigurace prostředí PowerShell Azure](../powershell-install-configure.md).


Mnoho nových funkcí databáze SQL jsou podporované jenom při použití [Správce prostředků Azure nasazení modelu](../azure-resource-manager/resource-group-overview.md), takže příklady pomocí [rutin prostředí PowerShell databáze SQL Azure](https://msdn.microsoft.com/library/azure/mt574084.aspx) pro správce prostředků. Z důvodu zpětné kompatibility jsou podporované existujícího klasické nasazení modelu [(klasické) rutiny pro správu databáze SQL Azure](https://msdn.microsoft.com/library/azure/dn546723.aspx) , ale doporučujeme že pomocí rutin Správce prostředků.


>[AZURE.NOTE] V závislosti na velikosti databáze zkopírování může chvíli trvat dokončete.


## <a name="copy-a-sql-database-to-the-same-server"></a>Zkopírujte databázi SQL na stejný server

Pokud chcete vytvořit kopii na stejný server, vynechat `-CopyServerName` parametr (nebo nastavit na stejný server).

    New-AzureRmSqlDatabaseCopy -ResourceGroupName "resourcegroup1" -ServerName "server1" -DatabaseName "database1" -CopyDatabaseName "database1_copy"

## <a name="copy-a-sql-database-to-a-different-server"></a>Zkopírujte databázi SQL na jiný server

Pokud chcete vytvořit kopii na jiném serveru, zahrnout `-CopyServerName` parametr a nastavte ji na jiném serveru. *Kopírovat* serveru musí existovat. Pokud je ve skupině jiné zdroje a pak musí obsahovat také `-CopyResourceGroupName` parametr.

    New-AzureRmSqlDatabaseCopy -ResourceGroupName "resourcegroup1" -ServerName "server1" -DatabaseName "database1" -CopyServerName "server2" -CopyDatabaseName "database1_copy"


## <a name="copy-a-sql-database-into-an-elastic-database-pool"></a>Zkopírujte SQL databáze do fondu pružná databáze

Pokud chcete vytvořit kopii databáze SQL do fondu, nastavte `-ElasticPoolName` parametr existující fond.

    New-AzureRmSqlDatabaseCopy -ResourceGroupName "resourcegoup1" -ServerName "server1" -DatabaseName "database1" -CopyResourceGroupName "poolResourceGroup" -CopyServerName "poolServer1" -CopyDatabaseName "database1_copy" -ElasticPoolName "poolName"


## <a name="resolve-logins"></a>Řešení přihlášení

Po dokončení operace kopírování se dá přihlášení, najdete v článku [řešení přihlášení](sql-database-copy-transact-sql.md#resolve-logins-after-the-copy-operation-completes)


## <a name="example-powershell-script"></a>Příklad skript Powershellu

Tento skript předpokládá všechny skupiny prostředků, servery, a fondu existovat (nahraďte hodnotách proměnných existujících zdrojů). Všechno, co musí existovat, s výjimkou kopii databáze.

    # Sign in to Azure and set the subscription to work with
    # ------------------------------------------------------
    $SubscriptionId = "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
    Add-AzureRmAccount
    Set-AzureRmContext -SubscriptionId $SubscriptionId
    
    
    # SQL database source (the existing database to copy)
    # ---------------------------------------------------
    $sourceDbName = "db1"
    $sourceDbServerName = "server1"
    $sourceDbResourceGroupName = "rg1"
    
    # SQL database copy (the new db to be created)
    # --------------------------------------------
    $copyDbName = "db1_copy"
    $copyDbServerName = "server2"
    $copyDbResourceGroupName = "rg2"
    
    # Copy a database to the same server
    # ----------------------------------
    New-AzureRmSqlDatabaseCopy -ResourceGroupName $sourceDbResourceGroupName -ServerName $sourceDbServerName -DatabaseName $sourceDbName -CopyDatabaseName $copyDbName
    
    # Copy a database to a different server
    # -------------------------------------
    New-AzureRmSqlDatabaseCopy -ResourceGroupName $sourceDbResourceGroupName -ServerName $sourceDbServerName -DatabaseName $sourceDbName -CopyResourceGroupName $copyDbResourceGroupName -CopyServerName $copyDbServerName -CopyDatabaseName $copyDbName
    
    # Copy a database into an elastic database pool
    # ---------------------------------------------
    $poolName = "pool1"
    
    New-AzureRmSqlDatabaseCopy -ResourceGroupName $sourceDbResourceGroupName -ServerName $sourceDbServerName -DatabaseName $sourceDbName -CopyResourceGroupName $copyDbResourceGroupName -CopyServerName $copyDbServerName -ElasticPoolName $poolName -CopyDatabaseName $copyDbName



    

## <a name="next-steps"></a>Další kroky

- Základní informace o kopírování databáze SQL Azure v tématu [kopírování databáze Azure SQL](sql-database-copy.md) .
- V tématu [kopírování databáze Azure SQL pomocí portálu Azure](sql-database-copy-portal.md) zkopírujte databáze pomocí portálu Azure.
- V tématu kopírování databáze pomocí jazyce Transact-SQL [kopii databáze Azure SQL pomocí příkazu T-SQL](sql-database-copy-transact-sql.md) .
- Zjistěte, [jak spravovat zabezpečení databáze Azure SQL po obnovení havárie](sql-database-geo-replication-security-config.md) Další informace o správě uživatelů a přihlášení při kopírování databáze na jiný logické server.


## <a name="additional-resources"></a>Další zdroje informací

- [Nové AzureRmSqlDatabase](https://msdn.microsoft.com/library/mt603644.aspx)
- [Get-AzureRmSqlDatabaseActivity](https://msdn.microsoft.com/library/mt603687.aspx)
- [Správa přihlášení](sql-database-manage-logins.md)
- [Připojení k databázi SQL s SQL Server Management Studio a provádění ukázkový T-SQL dotaz](sql-database-connect-query-ssms.md)
- [Export databáze BACPAC](sql-database-export.md)
- [Obchodní kontinuitu přehled](sql-database-business-continuity.md)
- [Dokumentace databáze SQL](https://azure.microsoft.com/documentation/services/sql-database/)
- [Reference pro rutiny prostředí PowerShell databáze Azure SQL](https://msdn.microsoft.com/library/mt574084.aspx)
