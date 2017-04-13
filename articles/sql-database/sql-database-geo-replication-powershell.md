<properties 
    pageTitle="Konfigurace aktivní Geo replikace databáze SQL Azure pomocí prostředí PowerShell | Microsoft Azure" 
    description="Konfigurace aktivní Geo replikace databáze SQL Azure pomocí prostředí PowerShell" 
    services="sql-database" 
    documentationCenter="" 
    authors="stevestein" 
    manager="jhubbard" 
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.topic="article"
    ms.tgt_pltfrm="powershell"
   ms.workload="NA"
    ms.date="07/14/2016"
    ms.author="sstein"/>

# <a name="configure-geo-replication-for-azure-sql-database-with-powershell"></a>Konfigurace Geo replikace databáze Azure SQL pomocí prostředí PowerShell

> [AZURE.SELECTOR]
- [Základní informace](sql-database-geo-replication-overview.md)
- [Azure portálu](sql-database-geo-replication-portal.md)
- [Prostředí PowerShell](sql-database-geo-replication-powershell.md)
- [T-SQL](sql-database-geo-replication-transact-sql.md)

Tento článek popisuje, jak nakonfigurovat aktivní Geo replikace databáze SQL pomocí prostředí PowerShell.

Zahajte převzetí pomocí Powershellu najdete v článku [Zahájit plánované nebo neplánované přepojení pro databáze SQL Azure pomocí prostředí PowerShell](sql-database-geo-replication-failover-powershell.md).

>[AZURE.NOTE] Aktivní Geo replikace (čitelné druhotné) je teď k dispozici pro všechny databáze ve všech vrstvách služby. V dubnovou 2017-čitelné sekundární typ ukončení a existující-čitelné databáze upgradují automaticky k čitelné druhotné.



Konfigurace aktivní Geo replikace pomocí Powershellu, budete potřebovat následující:

- Předplatné Azure. 
- Azure SQL databázi - primární databáze, kterou chcete replikovat.
- Azure PowerShell 1.0 nebo novější. Můžete stáhnout a nainstalovat moduly Azure PowerShell následující [Postup instalace a konfigurace prostředí PowerShell Azure](../powershell-install-configure.md).


## <a name="configure-your-credentials-and-select-your-subscription"></a>Konfigurace přihlašovacích údajů a vyberte předplatného

Nejprve je třeba vytvořit přístup ke svému účtu Azure tak spuštění prostředí PowerShell a znovu spusťte následující rutinu. Na přihlašovací obrazovce zadejte stejné e-mail a heslo, které používáte pro přihlášení k portálu Azure.


    Login-AzureRmAccount

Po úspěšném přihlášení uvidíte některé informace na obrazovce, který obsahuje Id přihlášení se a Azure předplatné, ke kterým máte přístup k.


### <a name="select-your-azure-subscription"></a>Vyberte předplatné Azure

Vyberte předplatné potřebujete předplatné Id. Id předplatného můžete zkopírovat z informace zobrazené v předchozím kroku nebo pokud máte víc předplatných a potřebujete další informace můžete spusťte rutinu **Get-AzureRmSubscription** a zkopírujte informace o předplatném požadované z této sadě. Následující rutinu nastaví aktuálního předplatného pomocí Id předplatného:

    Select-AzureRmSubscription -SubscriptionId 4cac86b0-1e56-bbbb-aaaa-000000000000

Po úspěšném dokončení **Vyberte AzureRmSubscription** vrátíte se prostředí PowerShell příkazový řádek.


## <a name="add-secondary-database"></a>Přidání sekundární databáze


Podle těchto kroků vytvořit novou databázi sekundární Geo replikace partnerství.  
  
Chcete-li povolit sekundární musí být vlastník předplatného nebo spoluvlastníka. 

Můžete použít rutinu **New-AzureRmSqlDatabaseSecondary** přidat sekundární databáze na serveru partner místní databáze na serveru, ke kterému jsou připojené (primární databáze). 

Tato rutina nahradí **Start AzureSqlDatabaseCopy** s parametrem **– IsContinuous** .  Bude výstupní **AzureRmSqlDatabaseSecondary** objekt, který lze tak, že ostatní rutiny jasně vystihují konkrétní replikace odkaz. Tato rutina vrátí při sekundárním databáze vytvořené a plně nasadí. V závislosti na velikosti databáze může trvat od minut hodin.

Replikovanou databázi na vedlejší serveru bude mít stejný název jako databáze na primárním serveru a ve výchozím nastavení mají stejné úrovni služby. Sekundární databáze může být číst nebo čitelné a může být jednu databázi nebo pružná databáze. Další informace najdete v tématu [Nový AzureRMSqlDatabaseSecondary](https://msdn.microsoft.com/library/mt603689.aspx) a [Úrovní služby](sql-database-service-tiers.md).
Po vytvoření a nasadí sekundární začne se data replikace z primární databáze do nové sekundární databáze. Následující kroky popisují, jak k provedení této úlohy pomocí prostředí PowerShell vytvořit-čitelnější a čitelný druhotné, ať už jednu databázi nebo databázi pružná.

Příkaz se nezdaří, pokud již existuje databáze partnera (například - důsledku ukončení předchozí relace Geo replikace).



### <a name="add-a-non-readable-secondary-single-database"></a>Přidání-čitelné sekundární (jedné databáze)

Následující příkaz vytvoří-čitelné sekundární databáze "mydb" server "srv2" v zdrojů skupina "rg2":

    $database = Get-AzureRmSqlDatabase –DatabaseName "mydb" -ResourceGroupName "rg1" -ServerName "srv1"
    $secondaryLink = $database | New-AzureRmSqlDatabaseSecondary –PartnerResourceGroupName "rg2" –PartnerServerName "srv2" -AllowConnections "No"



### <a name="add-readable-secondary-single-database"></a>Přidání čitelné sekundární (jedné databáze)

Následující příkaz vytvoří čitelné sekundární databáze "mydb" server "srv2" v zdrojů skupina "rg2":

    $database = Get-AzureRmSqlDatabase –DatabaseName "mydb" -ResourceGroupName "rg1" -ServerName "srv1"
    $secondaryLink = $database | New-AzureRmSqlDatabaseSecondary –PartnerResourceGroupName "rg2" –PartnerServerName "srv2" -AllowConnections "All"




### <a name="add-a-non-readable-secondary-elastic-database"></a>Přidání-čitelné sekundární (pružná databáze)

Následující příkaz vytvoří-čitelné sekundární databáze "mydb" ve fondu pružná databáze s názvem "ElasticPool1" server "srv2" v zdrojů skupina "rg2":

    $database = Get-AzureRmSqlDatabase –DatabaseName "mydb" -ResourceGroupName "rg1" -ServerName "srv1"
    $secondaryLink = $database | New-AzureRmSqlDatabaseSecondary –PartnerResourceGroupName "rg2" –PartnerServerName "srv2" –SecondaryElasticPoolName "ElasticPool1" -AllowConnections "No"


### <a name="add-a-readable-secondary-elastic-database"></a>Přidání čitelné sekundární (pružná databáze)

Následující příkaz vytvoří čitelné sekundární databáze "mydb" ve fondu pružná databáze s názvem "ElasticPool1" server "srv2" v zdrojů skupina "rg2":

    $database = Get-AzureRmSqlDatabase –DatabaseName "mydb" -ResourceGroupName "rg1" -ServerName "srv1"
    $secondaryLink = $database | New-AzureRmSqlDatabaseSecondary –PartnerResourceGroupName "rg2" –PartnerServerName "srv2" –SecondaryElasticPoolName "ElasticPool1" -AllowConnections "All"





## <a name="remove-secondary-database"></a>Odebrat sekundární databázi

Použijte rutinu **AzureRmSqlDatabaseSecondary odebrat** trvale ukončit replikace partnerství jeho primární a sekundární databáze. Po ukončení relace sekundární databáze přestane databázi pro čtení i zápis. Při přerušení propojení sekundární databáze příkazu povede, ale sekundární stane po obnovení připojení pro čtení i zápis. Další informace najdete v tématu [Odebrat AzureRmSqlDatabaseSecondary](https://msdn.microsoft.com/library/mt603457.aspx) a [Úrovní služby](sql-database-service-tiers.md).

Tato rutina slouží k nahrazení zastavit AzureSqlDatabaseCopy replikace. 

Odebrání odpovídá vynucený ukončení, která odebere propojení replikace a ponechá bývalého sekundární jako samostatný databázi, která není plně replikovat před ukončení. Pokud nebo pokud jsou k dispozici, budou všechny propojit data vyčistit na bývalého primární a sekundární bývalá. Tato rutina vrátí při odebrání replikace odkaz. 


Pokud chcete odebrat sekundární, měli uživatelé zápisu do primárních a sekundárních databáze podle RBAC. V tématu řízení přístupu na základě rolí podrobnosti.

Následující odebere propojení replikace databáze s názvem "mydb" na server "srv2" ze skupiny zdrojů "rg2". 

    $database = Get-AzureRmSqlDatabase –DatabaseName "mydb" -ResourceGroupName "rg1" -ServerName "srv1"
    $secondaryLink = $database | Get-AzureRmSqlDatabaseReplicationLink –SecondaryResourceGroup "rg2" –PartnerServerName "srv2"
    $secondaryLink | Remove-AzureRmSqlDatabaseSecondary 


## <a name="monitor-geo-replication-configuration-and-health"></a>Sledování stavu a konfigurace geo replikace

Sledování úkolů zahrnují sledování konfiguraci geo replikace a sledování stavu replikace data.  

[Get-AzureRmSqlDatabaseReplicationLink](https://msdn.microsoft.com/library/mt619330.aspx) lze získat informace o přesměrování replikační odkazy zobrazené sys.geo_replication_links katalogu.

Následující příkaz obnoví stav replikace propojení mezi databází "mydb" na primární a sekundární na server "srv2" ze skupiny zdrojů "rg2".

    $database = Get-AzureRmSqlDatabase –DatabaseName "mydb" -ResourceGroupName "rg1" -ServerName "srv1"
    $secondaryLink = $database | Get-AzureRmSqlDatabaseReplicationLink –PartnerResourceGroup "rg2” –PartnerServerName "srv2”


## <a name="next-steps"></a>Další kroky

- Další informace o aktivní Geo replikace najdete v tématu - [Aktivní Geo replikace](sql-database-geo-replication-overview.md)
- Přehled kontinuitu business a scénáře najdete v tématu [Přehled kontinuitu Business](sql-database-business-continuity.md)

