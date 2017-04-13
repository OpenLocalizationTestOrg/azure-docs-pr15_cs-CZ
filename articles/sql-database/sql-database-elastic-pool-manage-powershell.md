<properties 
    pageTitle="Správa fondu pružná databáze (Powershellu) | Microsoft Azure" 
    description="Informace o použití Powershellu ke správě fondu pružná databáze."  
    services="sql-database" 
    documentationCenter="" 
    authors="srinia" 
    manager="jhubbard" 
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.topic="article"
    ms.tgt_pltfrm="powershell"
    ms.workload="data-management" 
    ms.date="06/22/2016"
    ms.author="srinia"/>

# <a name="monitor-and-manage-an-elastic-database-pool-with-powershell"></a>Sledování a správa fondu pružná databáze pomocí prostředí PowerShell 

> [AZURE.SELECTOR]
- [Azure portálu](sql-database-elastic-pool-manage-portal.md)
- [Prostředí PowerShell](sql-database-elastic-pool-manage-powershell.md)
- [C#](sql-database-elastic-pool-manage-csharp.md)
- [T-SQL](sql-database-elastic-pool-manage-tsql.md)

Správa [fondu pružná databáze](sql-database-elastic-pool.md) pomocí rutin prostředí PowerShell. 

Běžné kódy chyb najdete v článku [kódy chyb SQL pro databáze SQL klientských aplikacích: databáze Chyba připojení i jiných problémů](sql-database-develop-error-messages.md).

Hodnoty pro fondy najdete v [eDTU a úložiště omezení](sql-database-elastic-pool.md#eDTU-and-storage-limits-for-elastic-pools-and-elastic-databases). 

## <a name="prerequisites"></a>Zjistit předpoklady pro

* Azure PowerShell 1.0 nebo vyšší. Podrobné informace najdete v tématu [instalace a konfigurace prostředí PowerShell Azure](../powershell-install-configure.md).
* Pružná databáze fondů jsou dostupné, jenom se servery V12 databáze SQL. Pokud máte V11 databáze SQL serveru [pomocí prostředí PowerShell upgradovat na V12 a vytvoření fondu](sql-database-upgrade-server-portal.md) v jednom kroku.


## <a name="move-a-database-into-an-elastic-pool"></a>Přesunutí databáze do fondu pružná

Databázi můžete přesouvat nebo stmívání fond s [AzureRmSqlDatabase sadu](https://msdn.microsoft.com/library/azure/mt619433.aspx). 

    Set-AzureRmSqlDatabase -ResourceGroupName "resourcegroup1" -ServerName "server1" -DatabaseName "database1" -ElasticPoolName "elasticpool1"

## <a name="change-performance-settings-of-a-pool"></a>Změna nastavení výkonu fondu

Když výkonu utrpí, můžete změnit nastavení fondu tak, aby zahrnoval LOGLINTREND. Použijte rutinu [Set-AzureRmSqlElasticPool](https://msdn.microsoft.com/library/azure/mt603511.aspx) . Nastavte parametr - Dtu eDTUs za fondu. V tématu [limity úložiště a eDTU](sql-database-elastic-pool.md#eDTU-and-storage-limits-for-elastic-pools-and-elastic-databases) možných hodnot.  

    Set-AzureRmSqlElasticPool –ResourceGroupName “resourcegroup1” –ServerName “server1” –ElasticPoolName “elasticpool1” –Dtu 1200 –DatabaseDtuMax 100 –DatabaseDtuMin 50 


## <a name="get-the-status-of-pool-operations"></a>Stav operace fondu

Vytvoření fondu může dobu trvat. Sledovat stav fondu operace včetně vytvoření a aktualizace, získáte pomocí rutiny [Get-AzureRmSqlElasticPoolActivity](https://msdn.microsoft.com/library/azure/mt603812.aspx) .

    Get-AzureRmSqlElasticPoolActivity –ResourceGroupName “resourcegroup1” –ServerName “server1” –ElasticPoolName “elasticpool1” 


## <a name="get-the-status-of-moving-an-elastic-database-into-and-out-of-a-pool"></a>Stav přesouvat pružná databáze do a z fondu

Přesunutí databáze může dobu trvat. Sledování stavu přesunout pomocí rutiny [Get-AzureRmSqlDatabaseActivity](https://msdn.microsoft.com/library/azure/mt603687.aspx) .

    Get-AzureRmSqlDatabaseActivity -ResourceGroupName "resourcegroup1" -ServerName "server1" -DatabaseName "database1" -ElasticPoolName "elasticpool1"

## <a name="get-resource-usage-data-for-a-pool"></a>Načtení dat využití prostředků pro fond

Metriky, které můžete získat v procentech limit fondu zdrojů:   


| Metrické název | Popis |
| :-- | :-- |
| Procesor\_procent | Průměr výpočet využití procento limit fondu. |
| pole fyzicky\_dat\_číst\_procent | Průměrná využití vstupu a výstupu v procentech na základě limitu fondu. |
| protokol\_psaní\_procent | Průměr napište využití prostředků v procentech limit fondu. | 
| DTU\_spotřebu\_procent | Využití průměr eDTU v procentech eDTU limit fondu | 
| úložiště\_procent | Průměrné využití úložiště v procentech limit úložiště ve fondu. |  
| pracovníci\_procent | Maximální souběžné pracující s informacemi (požadavky) v procentech na základě limitu fondu. |  
| relace\_procent | Maximální počet souběžné relací v procentech na základě limitu fondu. | 
| eDTU_limit | Aktuální maximální pružná fondu DTU nastavení pro tento pružná fond během tohoto intervalu. |
| úložiště\_limit | Aktuální maximální pružná fondu úložiště omezení nastavení pro tento pružná fond v megabajtech během tohoto intervalu. |
| eDTU\_používá | Průměrná eDTUs používaný fondu v tomto intervalu. |
| úložiště\_používá | Průměrná úložiště ve fondu v tomto intervalu v bajtech |

**Metriky granularity/uchovávání období:**

* Data budou vráceny na granularity pět minut.  
* Uchovávání informací je 35 dní.  

Tato rutina a rozhraní API omezuje počet řádků, které můžete získat v jednom volání do 1 000 řádků (asi 3 dnů datům granularity pět minut). Ale tohoto příkazu lze volat tisknutím s jinou začátek/konec časových intervalech k načítání dalších dat 

K načtení metriky:

    $metrics = (Get-AzureRmMetric -ResourceId /subscriptions/<subscriptionId>/resourceGroups/FabrikamData01/providers/Microsoft.Sql/servers/fabrikamsqldb02/elasticPools/franchisepool -TimeGrain ([TimeSpan]::FromMinutes(5)) -StartTime "4/18/2015" -EndTime "4/21/2015")  


## <a name="get-resource-usage-data-for-an-elastic-database"></a>Získat data využití prostředků pro pružná databáze

Tato rozhraní API jsou stejné jako aktuální rozhraní API (V12) slouží ke kontrole využití prostředků samostatnou databázi, s výjimkou následující sémantického rozdíl. 

Pro toto rozhraní API metriky načíst vyjádřený procentuální hodnotu za maximální eDTUs (nebo rovnocenný zakončení podkladového metriky jako procesoru, vstupu a výstupu atd) nastavení pro tuto skupinu. Například 50 % využívání některou z těchto metriky označuje, že spotřeba určitého zdroje na 50 % za zakončení limitu pro daný zdroj z fondu nadřazené. 

K načtení metriky:

    $metrics = (Get-AzureRmMetric -ResourceId /subscriptions/<subscriptionId>/resourceGroups/FabrikamData01/providers/Microsoft.Sql/servers/fabrikamsqldb02/databases/myDB -TimeGrain ([TimeSpan]::FromMinutes(5)) -StartTime "4/18/2015" -EndTime "4/21/2015") 

## <a name="add-an-alert-to-a-pool-resource"></a>Přidání upozornění do fondu zdrojů

Podívejte se na materiály pro odeslání e-mailová oznámení a výstrahy řetězce [koncové body URL](https://msdn.microsoft.com/library/mt718036.aspx) při zdroje narazí využití prahovou hodnotu, která nastavíte můžete přidat upozornění pravidla. Získáte pomocí rutiny AzureRmMetricAlertRule přidat. 

> [AZURE.IMPORTANT]Využití prostředků pro pružná fondů sledování má prodlevy nejméně 20 minut. Nastavení upozornění menší než 30 minut pro pružná fondů není aktuálně podporován. Upozornění na žádné nastavení pro pružná fondů tečkou (parametr s názvem "-velikost_okna" v prostředí PowerShell API) nemusí být menší než 30 minut spouštěný. Ujistěte se, že všechna oznámení, které definujete pro pružná fondy používat uplynutí (velikost_okna) zabere nejméně 30 minut.

V tomto příkladu přidá upozornění pro získání oznámit, přejde do fondu eDTU spotřebu nad určitou prahovou hodnotu.

    # Set up your resource ID configurations
    $subscriptionId = '<Azure subscription id>'      # Azure subscription ID
    $location =  '<location'                         # Azure region
    $resourceGroupName = '<resource group name>'     # Resource Group
    $serverName = '<server name>'                    # server name
    $poolName = '<elastic pool name>'                # pool name 

    #$Target Resource ID
    $ResourceID = '/subscriptions/' + $subscriptionId + '/resourceGroups/' +$resourceGroupName + '/providers/Microsoft.Sql/servers/' + $serverName + '/elasticpools/' + $poolName

    # Create an email action
    $actionEmail = New-AzureRmAlertRuleEmail -SendToServiceOwners -CustomEmail JohnDoe@contoso.com

    # create a unique rule name
    $alertName = $poolName + "- DTU consumption rule"

    # Create an alert rule for DTU_consumption_percent
    Add-AzureRMMetricAlertRule -Name $alertName -Location $location -ResourceGroup $resourceGroupName -TargetResourceId $ResourceID -MetricName "DTU_consumption_percent"  -Operator GreaterThan -Threshold 80 -TimeAggregationOperator Average -WindowSize 00:60:00 -Actions $actionEmail 

## <a name="add-alerts-to-all-databases-in-a-pool"></a>Přidejte oznámení pro všechny databáze do fondu

Můžete přidat pravidla výstrah všechny databáze ve fondu pružná odeslat e-mailová oznámení nebo upozornění řetězců pro [koncové body URL](https://msdn.microsoft.com/library/mt718036.aspx) při zdroj narazí využití mezní hodnoty nastavit tak, že na upozornění. 

> [AZURE.IMPORTANT] Využití prostředků pro pružná fondů sledování má prodlevy nejméně 20 minut. Nastavení upozornění menší než 30 minut pro pružná fondů není aktuálně podporován. Upozornění na žádné nastavení pro pružná fondů tečkou (parametr s názvem "-velikost_okna" v prostředí PowerShell API) nemusí být menší než 30 minut spouštěný. Ujistěte se, že všechna oznámení, které definujete pro pružná fondy používat uplynutí (velikost_okna) zabere nejméně 30 minut.

V tomto příkladu přidá upozornění pro každý databází ve fondu pro získání oznámit, půjde tuto databázi DTU spotřebu nad určitou prahovou hodnotu.

    # Set up your resource ID configurations
    $subscriptionId = '<Azure subscription id>'      # Azure subscription ID
    $location = '<location'                          # Azure region
    $resourceGroupName = '<resource group name>'     # Resource Group
    $serverName = '<server name>'                    # server name
    $poolName = '<elastic pool name>'                # pool name 

    # Get the list of databases in this pool.
    $dbList = Get-AzureRmSqlElasticPoolDatabase -ResourceGroupName $resourceGroupName -ServerName $serverName -ElasticPoolName $poolName

    # Create an email action
    $actionEmail = New-AzureRmAlertRuleEmail -SendToServiceOwners -CustomEmail JohnDoe@contoso.com

    # Get resource usage metrics for a database in an elastic database for the specified time interval.
    foreach ($db in $dbList)
    {
    $dbResourceId = '/subscriptions/' + $subscriptionId + '/resourceGroups/' + $resourceGroupName + '/providers/Microsoft.Sql/servers/' + $serverName + '/databases/' + $db.DatabaseName

    # create a unique rule name
    $alertName = $db.DatabaseName + "- DTU consumption rule"

    # Create an alert rule for DTU_consumption_percent
    Add-AzureRMMetricAlertRule -Name $alertName  -Location $location -ResourceGroup $resourceGroupName -TargetResourceId $dbResourceId -MetricName "dtu_consumption_percent"  -Operator GreaterThan -Threshold 80 -TimeAggregationOperator Average -WindowSize 00:60:00 -Actions $actionEmail

    # drop the alert rule
    #Remove-AzureRmAlertRule -ResourceGroup $resourceGroupName -Name $alertName
    } 



## <a name="collect-and-monitor-resource-usage-data-across-multiple-pools-in-a-subscription"></a>Shromažďovat a sledovat data využití prostředků napříč několika fondů v předplatné

Pokud máte velký počet databází v předplatné, je náročný sledování každý pružná fond samostatně. Místo toho rutiny prostředí PowerShell databáze SQL a dotazy T SQL kombinací získat informace o použití zdroje dat z více fondů a jejich databáze pro monitorování a analýzy využití prostředků. [Ukázka implementaci](https://github.com/Microsoft/sql-server-samples/tree/master/samples/manage/azure-sql-db-elastic-pools) množiny skriptů powershellu najdete v úložišti GitHub SQL Server ukázky spolu s si přečtěte následující dokumentaci pro akce a jak se používá.

Použít tato ukázková implementace postup vypsané dole.


1. Stáhněte si [skripty a si přečtěte následující dokumentaci](https://github.com/Microsoft/sql-server-samples/tree/master/samples/manage/azure-sql-db-elastic-pools):
2. Úprava skripty pro prostředí. Zadejte jedno nebo více servery, ve kterých jsou hostovány pružná fondů.
3. Určete databázi telemetrie místo, kam mají být uloženy shromážděných metriky. 
4. Vlastní skript, který chcete určit dobu trvání tyto skripty spuštění.

Tyto skripty na vysoké úrovni, postupujte takto:

*   Zobrazí všechny servery v dané Azure předplatné (nebo zadaný seznam servery).
*   Spustí úlohu pozadí pro každý server. Úlohy běží ve smyčce v pravidelných intervalech a shromažďuje telemetrickými daty pro všechny skupiny na serveru. Pak načte shromážděných dat do databáze zadaný telemetrie.
*   Zobrazí seznam databází v jednotlivých fondu shromažďovat data využití prostředků databáze. Potom načte shromážděná data do databázi telemetrie.

Sledování stavu pružná fondů a databází v něm lze analyzovat shromážděných metriky v databázi telemetrie. Skript nainstaluje taky předdefinované funkce hodnota tabulky (TVF) v databázi telemetrie pomůže aggregate metriky pro určeného časového intervalu. Příklad výsledků TVF mohou sloužit k zobrazení "horních N pružná fondů s využitím maximální eDTU v zadaného časového intervalu." Můžete také použijte k vytváření dotazů a analýze dat shromážděných analytické nástroje jako Excelu nebo Power BI.

## <a name="example-retrieve-resource-consumption-metrics-for-a-pool-and-its-databases"></a>Příklad: získání metriky spotřeba zdroje do fondu a jeho databáze

V tomto příkladu načte metriky spotřebu pro dané pružná fondu a všechny databáze. Údaje je formátovaný a napsali do formátu souboru .csv. Soubor můžete procházet s Excelem.

    $subscriptionId = '<Azure subscription id>'       # Azure subscription ID
    $resourceGroupName = '<resource group name>'             # Resource Group
    $serverName = <server name>                              # server name
    $poolName = <elastic pool name>                          # pool name
        
    # Login to Azure account and select the subscription.
    Login-AzureRmAccount
    Set-AzureRmContext -SubscriptionId $subscriptionId
    
    # Get resource usage metrics for an elastic pool for the specified time interval.
    $startTime = '4/27/2016 00:00:00'  # start time in UTC
    $endTime = '4/27/2016 01:00:00'    # end time in UTC
    
    # Construct the pool resource ID and retrive pool metrics at 5 minute granularity.
    $poolResourceId = '/subscriptions/' + $subscriptionId + '/resourceGroups/' + $resourceGroupName + '/providers/Microsoft.Sql/servers/' + $serverName + '/elasticPools/' + $poolName
    $poolMetrics = (Get-AzureRmMetric -ResourceId $poolResourceId -TimeGrain ([TimeSpan]::FromMinutes(5)) -StartTime $startTime -EndTime $endTime) 
    
    # Get the list of databases in this pool.
    $dbList = Get-AzureRmSqlElasticPoolDatabase -ResourceGroupName $resourceGroupName -ServerName $serverName -ElasticPoolName $poolName
    
    # Get resource usage metrics for a database in an elastic database for the specified time interval.
    $dbMetrics = @()
    foreach ($db in $dbList)
    {
        $dbResourceId = '/subscriptions/' + $subscriptionId + '/resourceGroups/' + $resourceGroupName + '/providers/Microsoft.Sql/servers/' + $serverName + '/databases/' + $db.DatabaseName
        $dbMetrics = $dbMetrics + (Get-AzureRmMetric -ResourceId $dbResourceId -TimeGrain ([TimeSpan]::FromMinutes(5)) -StartTime $startTime -EndTime $endTime)
    }
    
    #Optionally you can format the metrics and output as .csv file using the following script block.
    $command = {
    param($metricList, $outputFile)

    # Format metrics into a table.
    $table = @()
    foreach($metric in $metricList) { 
      foreach($metricValue in $metric.MetricValues) {
        $sx = New-Object PSObject -Property @{
            Timestamp = $metricValue.Timestamp.ToString()
            MetricName = $metric.Name; 
            Average = $metricValue.Average;
            ResourceID = $metric.ResourceId 
          }
          $table = $table += $sx
      }
    }
    
    # Output the metrics into a .csv file.
    write-output $table | Export-csv -Path $outputFile -Append -NoTypeInformation
    }
    
    # Format and output pool metrics
    Invoke-Command -ScriptBlock $command -ArgumentList $poolMetrics,c:\temp\poolmetrics.csv
    
    # Format and output database metrics
    Invoke-Command -ScriptBlock $command -ArgumentList $dbMetrics,c:\temp\dbmetrics.csv



## <a name="latency-of-elastic-pool-operations"></a>Latence operací pružná fondu

- Změna eDTUs min za databáze nebo max eDTUs podle databáze obvykle provede v 5 minut a méně.
- Změna eDTUs za fondu závisí na celkové využitého místa tak, že všechny databáze ve fondu. Změny průměrná 90 minut nebo méně na 100 GB. Pokud celkové místo používat všechny databáze ve fondu je například 200 GB očekávané latence pro změnu eDTU fondu za fondu následovně 3 hodiny nebo nižší.

## <a name="migrate-from-v11-to-v12-servers"></a>Migrace z V11 serverům V12

Rutiny prostředí PowerShell jsou k dispozici spustit, zastavit nebo sledovat upgrade V12 databáze SQL Azure, z V11 nebo jinou verzi pre V12.

- [Upgrade na V12 databáze SQL pomocí prostředí PowerShell](sql-database-upgrade-server-powershell.md)

Si přečtěte následující dokumentaci referenční informace o těchto rutin prostředí najdete v článku:


- [Get-AzureRMSqlServerUpgrade](https://msdn.microsoft.com/library/azure/mt603582.aspx)
- [Zahájení AzureRMSqlServerUpgrade](https://msdn.microsoft.com/library/azure/mt619403.aspx)
- [Zastavit AzureRMSqlServerUpgrade](https://msdn.microsoft.com/library/azure/mt603589.aspx)


Rutinu zastavit znamená zrušit, není pozastavit. Nejde žádným způsobem pokračujte v upgradu, než počínaje znovu. Rutinu zastavit vyčistí a aktualizací všechny příslušné zdroje.

## <a name="next-steps"></a>Další kroky

- [Vytvoření pružná úlohy](sql-database-elastic-jobs-overview.md) Pružná úlohy umožňují kontrolovat libovolný počet databází ve fondu skripty T-SQL.
- V tématu [měřítko, s databáze SQL Azure](sql-database-elastic-scale-introduction.md): umožňuje škálování, přesuňte data, pružná databázové nástroje dotazu nebo vytvořit transakce.
