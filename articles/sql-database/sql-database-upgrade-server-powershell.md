<properties
    pageTitle="Upgrade na V12 databáze SQL Azure pomocí prostředí PowerShell | Microsoft Azure"
    description="Vysvětluje, jak upgradovat na V12 databáze SQL Azure včetně upgrade webu a Business databází a upgrade serveru V11 migrace své databáze přímo do fondu pružná databáze pomocí Powershellu."
    services="sql-database"
    documentationCenter=""
    authors="stevestein"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/19/2016"
    ms.author="sstein"/>

# <a name="upgrade-to-azure-sql-database-v12-using-powershell"></a>Upgrade na V12 databáze SQL Azure pomocí prostředí PowerShell


> [AZURE.SELECTOR]
- [Azure portálu](sql-database-upgrade-server-portal.md)
- [Prostředí PowerShell](sql-database-upgrade-server-powershell.md)


V12 SQL databáze je nejnovější verze, takže upgradu na SQL databáze V12 se doporučuje.
V12 SQL databáze obsahuje mnoho včetně [výhody předchozí verze](sql-database-v12-whats-new.md) :

- Lepší kompatibilitu se serverem SQL Server.
- Vylepšené premium výkon a nové úrovně výkonu.
- [Pružná databáze fondů](sql-database-elastic-pool.md).

Tento článek obsahuje pokyny pro upgrade stávající servery V11 databáze SQL a databází V12 databáze SQL.

Během upgradu a V12 upgradujete Web a Business databází nové vrstvy služeb, pokyny pro upgrade webu a Business databáze jsou však započítávány.

Kromě toho migraci do [fondu pružná databázi](sql-database-elastic-pool.md) lze nákladů efektivnější než upgradu na jednotlivé výkonu úrovně (cena za úrovní) pro jednoho databáze. Fondů taky zjednodušení správy databáze vzhledem k tomu potřebujete spravovat nastavení výkonu pro fondu ne odděleně Správa úrovní výkonu jednotlivé databází. Pokud máte databází na více serverech, zvažte možnost jejich přesunutí do stejné server a využívat výhod uvedení do fondu.

Postupujte podle pokynů v tomto článku snadno migrovat databáze z V11 servery přímo do skupin pružná databáze.

Všimněte si, že databáze zůstat online a pokračovat v práci během operace upgradu. V době skutečné přechodu na nový výkonu úroveň dočasné přetažením připojení k databázi může dojít po příliš malý dobu, obvykle se asi 90 sekund ale může mít až 5 minut. Pokud má vaše aplikace [přechodná poruch zpracování pro ukončení připojení](sql-database-connectivity-issues.md) je dostatečně vysoká pro ochranu proti zamítnuté připojení na konci upgradu.

Upgrade na SQL databáze V12 nelze je vrátit zpět. Po upgradu nelze serveru vrátit do V11.

Po upgradu na V12, [doporučení osy služby](sql-database-service-tier-advisor.md) a [doporučení pružná fondu](sql-database-elastic-pool-create-portal.md) není možné okamžitě dokud službu nějaký čas na vyhodnocení úloh na novém serveru. V11 serveru doporučení historie, nebudou mít vliv na V12 servery tak, aby se zachovají.  

## <a name="prepare-to-upgrade"></a>Příprava na upgrade

- **Upgrade všechny databáze pro Web a firmy**: použití portálu nebo pomocí [prostředí PowerShell upgradovat databáze a server](sql-database-upgrade-server-powershell.md).
- **Revize a pozastavení Geo replikace**: Pokud databázi Azure SQL nakonfigurovaný pro Geo replikace by měl dokumentu jeho aktuální konfigurace a [Zastavit Geo replikace](sql-database-geo-replication-portal.md#remove-secondary-database). Po dokončení upgradu překonfigurujte Geo replikace databáze.
- **Otevřete tyto porty, pokud máte klienty v Azure OM**: Pokud klientským programem připojuje k SQL databázi V12 v průběhu svému klientovi na Azure virtuálního počítače (OM), musíte otevřít rozsahy portů 11000 11999 a 14000 14999 na OM. Podrobnosti najdete v tématu [porty V12 databáze SQL](sql-database-develop-direct-route-ports-adonet-v12.md).


## <a name="prerequisites"></a>Zjistit předpoklady pro

V12 pomocí prostředí PowerShell upgradovat serveru, musíte mít nejnovější Azure PowerShell nainstalovanou a spuštěnou. Podrobné informace najdete v tématu [instalace a konfigurace prostředí PowerShell Azure](../powershell-install-configure.md).


## <a name="configure-your-credentials-and-select-your-subscription"></a>Konfigurace přihlašovacích údajů a vyberte předplatného

Abyste mohli spouštět rutiny prostředí PowerShell proti předplatného Azure, musíte nejdřív zřídit přístup ke svému účtu Azure. Spusťte následující a zobrazí se přihlašovací obrazovce zadejte své přihlašovací údaje. Použijte stejný e-mail a heslo, které používáte pro přihlášení k portálu Azure.

    Add-AzureRmAccount

Po úspěšném přihlášení byste měli vidět některé informace na obrazovce, který obsahuje Id přihlášení se a Azure předplatné, ke kterým máte přístup k.

Vyberte předplatné, které chcete pracovat s vámi potřebovat předplatného Id (**-SubscriptionId**) nebo předplatného zadejte název (**-SubscriptionName**). Můžete ho zkopírovat z předchozího kroku nebo pokud máte víc předplatných spuštěním rutiny **Get-AzureRmSubscription** a zkopírujte informace o předplatném požadované z této sadě.

Spusťte následující rutinu s informací o vašem předplatném nastavovaná aktuálního předplatného:

    Set-AzureRmContext -SubscriptionId 4cac86b0-1e56-bbbb-aaaa-000000000000

Následující příkazy spustí proti předplatné jste právě vybrali nad.

## <a name="get-recommendations"></a>Získat doporučení

Chcete-li získat doporučení pro upgrade na serveru, spusťte následující rutinu:

    $hint = Get-AzureRmSqlServerUpgradeHint -ResourceGroupName “resourcegroup1” -ServerName “server1”

Další informace najdete v tématu [Vytvoření fondu pružná databází](sql-database-elastic-pool-create-portal.md) a [databáze SQL Azure ceny doporučení osy](sql-database-service-tier-advisor.md).



## <a name="start-the-upgrade"></a>Spuštění upgradu

Spuštění upgradu serveru, spusťte následující rutinu:

    Start-AzureRmSqlServerUpgrade -ResourceGroupName “resourcegroup1” -ServerName “server1” -ServerVersion 12.0 -DatabaseCollection $hint.Databases -ElasticPoolCollection $hint.ElasticPools  


Když spustíte tento příkaz proces upgradu se spustí. Můžete přizpůsobit výstup doporučení a poskytnout upravený doporučení pro tuto rutinu.


## <a name="upgrade-a-server"></a>Upgrade na server


    # Adding the account
    #
    Add-AzureRmAccount

    # Setting the variables
    #
    $SubscriptionName = 'YOUR_SUBSCRIPTION'
    $ResourceGroupName = 'YOUR_RESOURCE_GROUP'
    $ServerName = 'YOUR_SERVER'

    # Selecting the right subscription
    #
    Set-AzureRmContext -SubscriptionName $SubscriptionName

    # Getting the upgrade recommendations
    #
    $hint = Get-AzureRmSqlServerUpgradeHint -ResourceGroupName $ResourceGroupName -ServerName $ServerName

    # Starting the upgrade process
    #
    Start-AzureRmSqlServerUpgrade -ResourceGroupName $ResourceGroupName -ServerName $ServerName -ServerVersion 12.0 -DatabaseCollection $hint.Databases -ElasticPoolCollection $hint.ElasticPools  


## <a name="custom-upgrade-mapping"></a>Vlastní upgrade mapování

Pokud doporučení není vhodné pro server a obchodní případ, pak můžete jak databáze upgradu a můžete namapovat na jeden nebo pružná databází.

Parametry ElasticPoolCollection a DatabaseCollection jsou volitelné:

    # Creating elastic pool mapping
    #
    $elasticPool = New-Object -TypeName Microsoft.Azure.Management.Sql.Models.UpgradeRecommendedElasticPoolProperties
    $elasticPool.DatabaseDtuMax = 100
    $elasticPool.DatabaseDtuMin = 0
    $elasticPool.Dtu = 800
    $elasticPool.Edition = "Standard"
    $elasticPool.DatabaseCollection = ("DB1", “DB2”, “DB3”, “DB4”)
    $elasticPool.Name = "elasticpool_1"
    $elasticPool.StorageMb = 800

    # Creating single database mapping for 2 databases. DBMain1 mapped to S0 and DBMain2 mapped to S2
    #
    $databaseMap1 = New-Object -TypeName Microsoft.Azure.Management.Sql.Models.UpgradeDatabaseProperties
    $databaseMap1.Name = "DBMain1"
    $databaseMap1.TargetEdition = "Standard"
    $databaseMap1.TargetServiceLevelObjective = "S0"

    $databaseMap2 = New-Object -TypeName Microsoft.Azure.Management.Sql.Models.UpgradeDatabaseProperties
    $databaseMap2.Name = "DBMain2"
    $databaseMap2.TargetEdition = "Standard"
    $databaseMap2.TargetServiceLevelObjective = "S2"

    # Starting the upgrade
    #
    Start-AzureRmSqlServerUpgrade –ResourceGroupName resourcegroup1 –ServerName server1 -ServerVersion 12.0 -DatabaseCollection @($databaseMap1, $databaseMap2) -ElasticPoolCollection @($elasticPool)



## <a name="monitor-databases-after-upgrading-to-sql-database-v12"></a>Sledování databází po upgradu na SQL databáze V12


Po upgradu, doporučujeme sledovat databázi aktivně zajistit aplikací pracují požadovaný výkon a optimálně podle potřeby.

Kromě toho ke sledování jednotlivé databáze můžete sledovat pružná databázi fondů [na portálu](sql-database-elastic-pool-manage-portal.md) nebo pomocí [prostředí PowerShell](sql-database-elastic-pool-manage-powershell.md)


**Zdroje dat spotřebu:** Pro Basic, Standard a Premium databáze je k dispozici až [sys.dm_ db_ resource_stats](http://msdn.microsoft.com/library/azure/dn800981.aspx) DMV v databázi uživatelů daty spotřebu zdrojů. Tento DMV poskytuje poblíž v reálném čase spotřebu informace o zdroji na 15 druhý granularity pro předchozí hodiny operace. Spotřebu DTU procentuální hodnotu pro interval počítá jako maximální procento využití procesoru, vstupu a výstupu a protokolu rozměry. Tady je dotazu pro výpočet průměru spotřebu procento DTU přes poslední hodiny:

    SELECT end_time
         , (SELECT Max(v)
             FROM (VALUES (avg_cpu_percent)
                         , (avg_data_io_percent)
                         , (avg_log_write_percent)
           ) AS value(v)) AS [avg_DTU_percent]
    FROM sys.dm_db_resource_stats
    ORDER BY end_time DESC;

Další informace o sledování:

- [Databáze SQL azure výkonu pokyny pro jeden databáze](http://msdn.microsoft.com/library/azure/dn369873.aspx).
- [Ceny a výkonu aspektech pro fondu pružná databáze](sql-database-elastic-pool-guidance.md).
- [Sledování databáze SQL Azure pomocí Správa dynamických zobrazení](sql-database-monitoring-with-dmvs.md)



**Upozornění:** Nastavte "upozornění na portálu Azure oznámí, že spotřeba DTU pro databázi upgradovaném blíží určitých vysokou úroveň. Databáze upozornění můžete nastavit v portálu Azure pro různé měřítka jako DTU, využití procesoru, vstupu a výstupu a protokolu. Přejděte do vaší databáze a vyberte **upozornění pravidla** v zásuvné **Nastavení** .

Například můžete nastavit e-mailovém upozornění na "DTU procentuální hodnotu" Pokud průměrná hodnota procenta DTU přesahuje 75 % přes poslední 5 minut. Další informace o tom, jak konfigurovat oznámení v nápovědě k [dostávat oznámení](../monitoring-and-diagnostics/insights-receive-alert-notifications.md) .



## <a name="next-steps"></a>Další kroky

- [Vytvoření fondu pružná databáze](sql-database-elastic-pool-create-portal.md) a přidejte některé nebo všechny databáze do fondu.
- [Změna služby osy a výkonu úrovně databáze](sql-database-scale-up.md).



## <a name="related-information"></a>Související informace

- [Get-AzureRmSqlServerUpgrade](https://msdn.microsoft.com/library/azure/mt603582.aspx)
- [Zahájení AzureRmSqlServerUpgrade](https://msdn.microsoft.com/library/azure/mt619403.aspx)
- [Zastavit AzureRmSqlServerUpgrade](https://msdn.microsoft.com/library/azure/mt603589.aspx)
