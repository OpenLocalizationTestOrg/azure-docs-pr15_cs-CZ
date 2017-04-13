<properties
    pageTitle="Začínáme s úloh pružná databází"
    description="použití úlohy pružná databáze"
    services="sql-database"
    documentationCenter=""  
    manager="jhubbard"
    authors="ddove"/>

<tags
    ms.service="sql-database"
    ms.workload="sql-database"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/06/2016"
    ms.author="ddove" />

# <a name="getting-started-with-elastic-database-jobs"></a>Začínáme s úloh pružná databází

Pružná úloh databází (verze preview) pro databázi SQL Azure umožňuje vám spolehlivost spustit skripty T-SQL, které zahrnují více databázím při automatické opakování a poskytuje záruky případné dokončení. Další informace o funkci úlohy pružná databáze najdete na [stránce Přehled funkcí](sql-database-elastic-jobs-overview.md).

Toto téma slouží k rozšíření výběru součástí [Začínáme s pružná databázové nástroje](sql-database-elastic-scale-get-started.md). Po dokončení, udělejte toto: Naučte se vytvářet a spravovat úloh, které Správa skupiny související databází. Není potřeba při použití nástroje pro pružná měřítko abyste mohli využívat všech výhod pružná úlohy.

## <a name="prerequisites"></a>Zjistit předpoklady pro

Stáhnout a spustit [Začínáme s ukázkovými nástroje pružná databáze](sql-database-elastic-scale-get-started.md).

## <a name="create-a-shard-map-manager-using-the-sample-app"></a>Vytvoření shard správce mapy v aplikaci ukázka

Tady vytvoříte mapě shard správce spolu s několika shards, následovaný vložení dat do shards. Pokud už máte shards nastavení sharded daty v nich můžete přeskočit následující kroky a přesunutí do další části.

1. Vytvoření a spuštění aplikace **Začínáme s pružná databázové nástroje** vzorku. Postupujte podle pokynů až do kroku 7 v části [Stáhnout a spustit aplikaci vzorku](sql-database-elastic-scale-get-started.md#Getting-started-with-elastic-database-tools). Na konci kroku 7 zobrazí se následující příkazového řádku:

    ![příkazový řádek][1]

2.  V okně příkazového napište "1" a stiskněte klávesu **Enter**. Vytvoří shard správce mapy a přidá dva shards k serveru. Potom zadejte hodnotu "3" a stiskněte klávesu **Enter**. Opakujte tuto akci čtyřikrát. To ve vaší shards vloží ukázkové datové řádky.

3.  [Portál Azure](https://portal.azure.com) by měl zobrazit tři nové databáze na serveru v12:

    ![Potvrzení Visual Studio][2]

    V tomto okamžiku vytvoříme kolekce vlastní databázi, která odpovídá všechny databáze na mapě shard. To vám umožní nám k vytváření a spouštění úlohy, přidat novou tabulku přes shards.

Tady by obvykle vytvoříme cíl mapy shard, pomocí rutinu **New-AzureSqlJobTarget** . Databáze shard mapy správce musí být nastavený jako cílovou databázi a pak určené konkrétní shard mapu jako cíle. Místo toho budeme umožňuje zobrazit výčet všechny databáze na serveru a přidejte databází nové vlastní kolekce s výjimkou hlavní databáze.

##<a name="creates-a-custom-collection-and-adds-all-databases-in-the-server-to-the-custom-collection-target-with-the-exception-of-master"></a>Vytvoří vlastní kolekci a přidá všechny databáze na serveru k vlastní kolekci cíli s výjimkou předlohy.


    $customCollectionName = "dbs_in_server"
    New-AzureSqlJobTarget -CustomCollectionName $customCollectionName 
    $ResourceGroupName = "ddove_samples"
    $ServerName = "samples"
    $dbsinserver = Get-AzureRMSqlDatabase -ResourceGroupName $ResourceGroupName -ServerName $ServerName 
    $dbsinserver | %{
    $currentdb = $_.DatabaseName 
    $ErrorActionPreference = "Stop"
    Write-Output ""

    Try
    {
       New-AzureSqlJobTarget -ServerName $ServerName -DatabaseName $currentdb | Write-Output
    }
    Catch 
    {
        $ErrorMessage = $_.Exception.Message
        $ErrorCategory = $_.CategoryInfo.Reason
                
        if ($ErrorCategory -eq 'UniqueConstraintViolatedException')
        {
             Write-Host $currentdb "is already a database target." 
        }

        else
        {
            throw $_
        }
    
    }

    Try
    {
        if ($currentdb -eq "master")
        {
            Write-Host $currentdb "will not be added custom collection target" $CustomCollectionName "."
        }

        else
        {
            Add-AzureSqlJobChildTarget -CustomCollectionName $CustomCollectionName -ServerName $ServerName -DatabaseName $currentdb
            Write-Host $currentdb "was added to" $CustomCollectionName "."
        }

    }
    Catch 
    {
        $ErrorMessage = $_.Exception.Message
        $ErrorCategory = $_.CategoryInfo.Reason

        if ($ErrorCategory -eq 'UniqueConstraintViolatedException')
        {
             Write-Host $currentdb "is already in the custom collection target" $CustomCollectionName"."
        }

        else
        {
            throw $_
        }
    }
    $ErrorActionPreference = "Continue"
}
    
## <a name="create-a-t-sql-script-for-execution-across-databases"></a>Vytvořit skript T-SQL pro provádění různých databází

    $scriptName = "NewTable"
    $scriptCommandText = "
    IF NOT EXISTS (SELECT name FROM sys.tables WHERE name = 'Test')
    BEGIN
        CREATE TABLE Test(
            TestId INT PRIMARY KEY IDENTITY,
            InsertionTime DATETIME2
        );
    END
    GO
    INSERT INTO Test(InsertionTime) VALUES (sysutcdatetime());
    GO"

    $script = New-AzureSqlJobContent -ContentName $scriptName -CommandText $scriptCommandText
    Write-Output $script

##<a name="create-the-job-to-execute-a-script-across-the-custom-group-of-databases"></a>Vytvoření projektu provést skript přes vlastní skupinu databází

    $jobName = "create on server dbs"
    $scriptName = "NewTable"
    $customCollectionName = "dbs_in_server"
    $credentialName = "ddove66"
    $target = Get-AzureSqlJobTarget -CustomCollectionName $customCollectionName
    $job = New-AzureSqlJob -JobName $jobName -CredentialName $credentialName -ContentName $scriptName -TargetId $target.TargetId
    Write-Output $job


##<a name="execute-the-job"></a>Provedení úlohy 

Tento skript Powershellu lze použít k provedení existujícího projektu:

Aktualizace následující proměnnou tak, aby odrážely na název požadovaného projektu provedli průzkum:

    $jobName = "create on server dbs"
    $jobExecution = Start-AzureSqlJobExecution -JobName $jobName 
    Write-Output $jobExecution
 
## <a name="retrieve-the-state-of-a-single-job-execution"></a>Načtení stav provádění jednoho projektu

Použijte stejné rutiny **Get-AzureSqlJobExecution** s parametrem **IncludeChildren** zobrazíte stavu podřízené spuštění úlohy, a to konkrétní stavu pro každé spuštění úloh každou databázi cílový projekt.

    $jobExecutionId = "{Job Execution Id}"
    $jobExecutions = Get-AzureSqlJobExecution -JobExecutionId $jobExecutionId -IncludeChildren
    Write-Output $jobExecutions 

## <a name="view-the-state-across-multiple-job-executions"></a>Zobrazit stav napříč několika spuštění úlohy

Rutina **Get-AzureSqlJobExecution** obsahuje více volitelné parametrů, které lze zobrazit více spuštění úlohy filtrované prostřednictvím uvedených parametrů. Následující znázorňuje některé z možných způsobů použití Get-AzureSqlJobExecution:

Načtení všechny spuštění aktivní nejvyšší úrovně úlohy:

    Get-AzureSqlJobExecution

Načtení všechny spuštění nejvyšší úrovně projektu, včetně spuštění neaktivní úlohy:

    Get-AzureSqlJobExecution -IncludeInactive

Načtení všechny spuštění úlohy podřízené spuštění ID zadané práce, včetně spuštění neaktivní úlohy:

    $parentJobExecutionId = "{Job Execution Id}"
    Get-AzureSqlJobExecution -AzureSqlJobExecution -JobExecutionId $parentJobExecutionId –IncludeInactive -IncludeChildren

Načtení všechny spuštění úlohy vytvořené pomocí plánu / funkce kombinace, včetně neaktivní úlohy:

    $jobName = "{Job Name}"
    $scheduleName = "{Schedule Name}"
    Get-AzureSqlJobExecution -JobName $jobName -ScheduleName $scheduleName -IncludeInactive

Načtení všechny úlohy zacílení zadaný shard mapu, včetně neaktivní úlohy:

    $shardMapServerName = "{Shard Map Server Name}"
    $shardMapDatabaseName = "{Shard Map Database Name}"
    $shardMapName = "{Shard Map Name}"
    $target = Get-AzureSqlJobTarget -ShardMapManagerDatabaseName $shardMapDatabaseName -ShardMapManagerServerName $shardMapServerName -ShardMapName $shardMapName
    Get-AzureSqlJobExecution -TargetId $target.TargetId –IncludeInactive

Načtení všechny úlohy zacílení vlastní kolekce, včetně neaktivní úlohy:

    $customCollectionName = "{Custom Collection Name}"
    $target = Get-AzureSqlJobTarget -CustomCollectionName $customCollectionName
    Get-AzureSqlJobExecution -TargetId $target.TargetId –IncludeInactive
 
Načtení seznamu spuštění úkolů projektu v rámci konkrétní úlohu spuštění:

    $jobExecutionId = "{Job Execution Id}"
    $jobTaskExecutions = Get-AzureSqlJobTaskExecution -JobExecutionId $jobExecutionId
    Write-Output $jobTaskExecutions 

Načtení podrobnosti úkolu spuštění úlohy:

Tento skript Powershellu mohou sloužit k zobrazení podrobností o provádění úkolů projektu, což je užitečné při ladění chyb spuštění.

    $jobTaskExecutionId = "{Job Task Execution Id}"
    $jobTaskExecution = Get-AzureSqlJobTaskExecution -JobTaskExecutionId $jobTaskExecutionId
    Write-Output $jobTaskExecution

## <a name="retrieve-failures-within-job-task-executions"></a>Načtení k chybám v rámci spuštění úkolů projektu

Objekt JobTaskExecution zahrnuje vlastnost pro životní cyklus úkolu společně s vlastnost zprávy. Pokud provádění úkolů projektu selhalo vlastnost životního cyklu nastaví se *nezdařilo* a vlastností zprávy nastaví výsledné zpráva o výjimce a jeho zásobníku. Pokud úlohy se nezdařila, je důležité podrobnosti úlohy, které se nezdařila pro danou úlohu.

    $jobExecutionId = "{Job Execution Id}"
    $jobTaskExecutions = Get-AzureSqlJobTaskExecution -JobExecutionId $jobExecutionId
    Foreach($jobTaskExecution in $jobTaskExecutions) 
        {
        if($jobTaskExecution.Lifecycle -ne 'Succeeded')
            {
            Write-Output $jobTaskExecution
            }
        }

## <a name="waiting-for-a-job-execution-to-complete"></a>Čekání na spuštění práce k dokončení

Až úlohu dokončete lze tento skript Powershellu:

    $jobExecutionId = "{Job Execution Id}"
    Wait-AzureSqlJobExecution -JobExecutionId $jobExecutionId 

## <a name="create-a-custom-execution-policy"></a>Vytvoření vlastní spuštění zásad

Pružná úloh databází podporuje vytváření vlastní spuštění zásad, které se dají použít při spuštění úlohy.
  
Pro definování aktuálně dovolit zásady spuštění:

* Název: Identifikátor zásad spouštění.
* Časový limit úlohy: Celkovou dobu, po jejímž úlohy zruší úlohami pružná databáze.
* Počáteční Interval opakování: Interval čekat, než první opakování.
* Maximální Interval opakování: Zakončení opakovat intervalů používat.
* Opakování koeficient zdvojnásobení intervalu: Koeficient použitých při výpočtu další interval mezi nimi.  Používá následující vzorec: (výchozí interval mezi opakovat) * Math.pow (Interval zdvojnásobení koeficient (), (počet pokusů o) - 2). 
* Maximální počet pokusů: Maximální počet opakování pokusí provádět v rámci projektu.

Výchozí zásady spuštění používá následující hodnoty:

* Název: Výchozí spuštění zásady
* Časový limit úlohy: minulého týdne
* Počáteční Interval opakování: 100 milisekund
* Maximální Interval opakování: 30 minut
* Opakovat koeficient intervalu: 2
* Maximální počet pokusů: 2 147 483 647

Vytvoření zásad požadované spuštění:

    $executionPolicyName = "{Execution Policy Name}"
    $initialRetryInterval = New-TimeSpan -Seconds 10
    $jobTimeout = New-TimeSpan -Minutes 30
    $maximumAttempts = 999999
    $maximumRetryInterval = New-TimeSpan -Minutes 1
    $retryIntervalBackoffCoefficient = 1.5
    $executionPolicy = New-AzureSqlJobExecutionPolicy -ExecutionPolicyName $executionPolicyName -InitialRetryInterval $initialRetryInterval -JobTimeout $jobTimeout -MaximumAttempts $maximumAttempts -MaximumRetryInterval $maximumRetryInterval -RetryIntervalBackoffCoefficient $retryIntervalBackoffCoefficient
    Write-Output $executionPolicy

### <a name="update-a-custom-execution-policy"></a>Aktualizovat zásady vlastní spuštění

Aktualizace aktualizace zásad požadované spuštění:

    $executionPolicyName = "{Execution Policy Name}"
    $initialRetryInterval = New-TimeSpan -Seconds 15
    $jobTimeout = New-TimeSpan -Minutes 30
    $maximumAttempts = 999999
    $maximumRetryInterval = New-TimeSpan -Minutes 1
    $retryIntervalBackoffCoefficient = 1.5
    $updatedExecutionPolicy = Set-AzureSqlJobExecutionPolicy -ExecutionPolicyName $executionPolicyName -InitialRetryInterval $initialRetryInterval -JobTimeout $jobTimeout -MaximumAttempts $maximumAttempts -MaximumRetryInterval $maximumRetryInterval -RetryIntervalBackoffCoefficient $retryIntervalBackoffCoefficient
    Write-Output $updatedExecutionPolicy
 
## <a name="cancel-a-job"></a>Zrušení úlohy

Pružná úloh databází podporuje žádostí o zrušení úlohy.  Pokud pružná úloh databází zjistí žádost o zrušení projektu probíhajících, pokusí se ukončit úlohu.

Že pružná úloh databází může provádět zrušení dvěma různými způsoby:

1. Zrušení aktuálně provádění úkolů: Pokud úkol je spuštěná aktuálně ke zjištění zrušení, zrušení pokusí v rámci aktuálně spouštěné poměr úkolu.  Příklad: Pokud dlouhodobé dotazu aktuálně provedených při pokusu o zrušení se budou pokus o zruší dotaz.
2. Zrušení opakování úkolu: V případě zrušení zjištění podprocesem ovládací prvek před úkolu je spuštěn pro spuštění, vlákna ovládací prvek bude zabránit spuštění úkol a deklarovat žádost jako zrušená.

Pokud zrušení úlohy žádá nadřazené projektu, bude žádost o zrušení dodržet nadřazené úlohy a všechny její podřízené úlohy.
 
Odeslání žádosti o zrušení, získáte pomocí rutiny **Zastavit AzureSqlJobExecution** a nastavit parametr **JobExecutionId** .

    $jobExecutionId = "{Job Execution Id}"
    Stop-AzureSqlJobExecution -JobExecutionId $jobExecutionId

## <a name="delete-a-job-by-name-and-the-jobs-history"></a>Odstranění úlohy podle názvu a historie projektu

Pružná úloh databází podporuje asynchronní odstranění úlohy. Úlohy můžete označit pro odstranění a systém odstraní úlohy a všechny její historie úlohy po spuštění všechny úlohy dokončili pro daný úkol. Systému nebude zrušit automatické spuštění aktivní úlohy.  

Místo toho zastavit AzureSqlJobExecution musí zavolat zrušit aktivní úlohy spuštění.

Aktivace odstranění úlohy, získáte pomocí rutiny **Odebrat AzureSqlJob** a nastavit parametr **název_úlohy** .

    $jobName = "{Job Name}"
    Remove-AzureSqlJob -JobName $jobName
 
## <a name="create-a-custom-database-target"></a>Vytvoření vlastní databáze cílového
Vlastní databáze cílů možné definovat ve úloh pružná databází, které lze použít pro provádění přímo nebo zahrnutí ve skupině vlastní databázi. Protože **fondů pružná databáze** dosud nepodporuje doplňky přímo prostřednictvím rozhraní API Powershellu, vytvoříte jednoduše cílové vlastní databáze a cílové kolekce vlastní databáze, která zahrnuje všechny databáze ve fondu.

Nastavení následujících proměnných tak, aby odrážely informace požadované databáze:

    $databaseName = "{Database Name}"
    $databaseServerName = "{Server Name}"
    New-AzureSqlJobDatabaseTarget -DatabaseName $databaseName -ServerName $databaseServerName 

## <a name="create-a-custom-database-collection-target"></a>Vytvoření vlastní databáze kolekce cílového
Cílové kolekce vlastní databáze je to možné definovat povolíte spuštění napříč několika cílů definovaný databáze. Po vytvoření skupiny databáze může být přidružené k vlastní kolekci cílové databáze.

Nastavení následujících proměnných tak, aby odrážely konfiguraci cílové požadované vlastní kolekce:

    $customCollectionName = "{Custom Database Collection Name}"
    New-AzureSqlJobTarget -CustomCollectionName $customCollectionName 

### <a name="add-databases-to-a-custom-database-collection-target"></a>Přidávání databází cílové kolekce vlastní databáze

Cíle databáze může být přidružené k cílů kolekce vlastní databáze pro vytvoření skupiny databází. Pokaždé, když je vytvořen projektu, který se zaměřuje cílové kolekce vlastní databáze, jeho rozbalená jej směrovat přidružené ke skupině v době spuštění databáze.

Přidejte požadované databáze na určitou kolekci vlastní:

    $serverName = "{Database Server Name}"
    $databaseName = "{Database Name}"
    $customCollectionName = "{Custom Database Collection Name}"
    Add-AzureSqlJobChildTarget -CustomCollectionName $customCollectionName -DatabaseName $databaseName -ServerName $databaseServerName 

#### <a name="review-the-databases-within-a-custom-database-collection-target"></a>Kontrola databází v rámci kolekce cíle vlastní databáze

Použijte rutinu **Get-AzureSqlJobTarget** k načtení podřízené databází v rámci kolekce cíle vlastní databázi. 
 
    $customCollectionName = "{Custom Database Collection Name}"
    $target = Get-AzureSqlJobTarget -CustomCollectionName $customCollectionName
    $childTargets = Get-AzureSqlJobTarget -ParentTargetId $target.TargetId
    Write-Output $childTargets

### <a name="create-a-job-to-execute-a-script-across-a-custom-database-collection-target"></a>Vytvoření projektu provést skript přes cílové kolekce vlastní databáze

Použijte rutinu **New-AzureSqlJob** k vytvoření úlohy proti skupiny databází definované cílové kolekce vlastní databázi. Pružná databáze projektů rozbalte projektu do více úloh podřízené odpovídající jednotlivým databáze přidružený k cílové kolekce vlastní databáze a zrušte zaškrtnutí spuštění skriptu každou databázi. Znovu je důležité, že skripty jsou idempotent pružné opakování.

    $jobName = "{Job Name}"
    $scriptName = "{Script Name}"
    $customCollectionName = "{Custom Collection Name}"
    $credentialName = "{Credential Name}"
    $target = Get-AzureSqlJobTarget -CustomCollectionName $customCollectionName
    $job = New-AzureSqlJob -JobName $jobName -CredentialName $credentialName -ContentName $scriptName -TargetId $target.TargetId
    Write-Output $job

## <a name="data-collection-across-databases"></a>Shromažďování dat přes databází

**Pružná databáze úlohy** podporuje spuštění dotazu na přes celou skupinu databází a odešle výsledky do tabulky zadané databázi. Tabulku můžete dotazovaném ve skutečnosti a zobrazte výsledky dotazu na každou databázi. To poskytuje mechanismus asynchronní spuštění dotazu na přes mnoho databází. Chyba při případech například jednu z databáze je dočasně nedostupný jsou zpracovány automaticky prostřednictvím opakování.

Zadaný cílové tabulky se automaticky vytvoří Pokud dosud neexistuje, odpovídající schématu sady vrácených výsledků. Pokud spuštění skriptu vrátí více sad výsledků dotazu, odeslat úloh pružná databází pouze první z nich do zadané cílové tabulky.

Tento skript Powershellu lze použít k provedení skript shromažďování jeho výsledky do zadané tabulky. Tento skript předpokládá, že byl vytvořen skript T-SQL, které výstupy sady výsledkem je jedna hodnota a cílové kolekce vlastní databázi vytvořila.

Nastavení podle následujících pokynů obsahovat požadované skript, přihlašovací údaje a cílové spuštění:

    $jobName = "{Job Name}"
    $scriptName = "{Script Name}"
    $executionCredentialName = "{Execution Credential Name}"
    $customCollectionName = "{Custom Collection Name}"
    $destinationCredentialName = "{Destination Credential Name}"
    $destinationServerName = "{Destination Server Name}"
    $destinationDatabaseName = "{Destination Database Name}"
    $destinationSchemaName = "{Destination Schema Name}"
    $destinationTableName = "{Destination Table Name}"
    $target = Get-AzureSqlJobTarget -CustomCollectionName $customCollectionName

### <a name="create-and-start-a-job-for-data-collection-scenarios"></a>Vytvoření a spuštění úlohy scénáře shromažďování dat
    $job = New-AzureSqlJob -JobName $jobName -CredentialName $executionCredentialName -ContentName $scriptName -ResultSetDestinationServerName $destinationServerName -ResultSetDestinationDatabaseName $destinationDatabaseName -ResultSetDestinationSchemaName $destinationSchemaName -ResultSetDestinationTableName $destinationTableName -ResultSetDestinationCredentialName $destinationCredentialName -TargetId $target.TargetId
    Write-Output $job
    $jobExecution = Start-AzureSqlJobExecution -JobName $jobName
    Write-Output $jobExecution

## <a name="create-a-schedule-for-job-execution-using-a-job-trigger"></a>Vytvoření plánu pro spuštění úlohy pomocí aktivační události projektu

Tento skript Powershellu mohou sloužit k vytvoření opakovaném plánu. Tento skript používá jeden minut, ale nový AzureSqlJobSchedule taky podporuje - DayInterval - HourInterval, - MonthInterval a - WeekInterval parametry. Plány, které spouštět jenom jednou se dají vytvářet podle předávání - jednorázově.

Vytvoření nového plánu:

    $scheduleName = "Every one minute"
    $minuteInterval = 1
    $startTime = (Get-Date).ToUniversalTime()
    $schedule = New-AzureSqlJobSchedule -MinuteInterval $minuteInterval -ScheduleName $scheduleName -StartTime $startTime 
    Write-Output $schedule

### <a name="create-a-job-trigger-to-have-a-job-executed-on-a-time-schedule"></a>Vytvoření aktivační úlohy mít úlohu spouštět na časový plán

Aktivační události projektu je to možné definovat mít spouštět podle časového plánu projektu. Tento skript Powershellu můžete použít k vytvoření aktivační události projektu.

Nastavení následujících proměnných tak, aby odpovídaly požadovaný projekt a plán:

    $jobName = "{Job Name}"
    $scheduleName = "{Schedule Name}"
    $jobTrigger = New-AzureSqlJobTrigger -ScheduleName $scheduleName –JobName $jobName
    Write-Output $jobTrigger

### <a name="remove-a-scheduled-association-to-stop-job-from-executing-on-schedule"></a>Odebrání naplánovaných přidružení ukončit úlohu spuštění plánu

Pokud chcete přestat opakovaném spuštění úlohy pomocí aktivační události projektu, je možné odebrat aktivační události projektu. Odebrání aktivační úlohu ukončit úlohu z prováděný podle plánu pomocí rutiny **AzureSqlJobTrigger odebrat** .

    $jobName = "{Job Name}"
    $scheduleName = "{Schedule Name}"
    Remove-AzureSqlJobTrigger -ScheduleName $scheduleName -JobName $jobName





## <a name="import-elastic-database-query-results-to-excel"></a>Import výsledků dotazu pružná databáze do aplikace Excel

 Výsledky dotazu do souboru aplikace Excel můžete importovat.

1. Spusťte aplikaci Excel 2013.
2.  Přejděte na pásu karet **Data** .
3.  Klikněte na **Z jiných zdrojů** a klikněte na **Ze serveru SQL Server**.

    ![Import z aplikace Excel z jiných zdrojů][5]
4.  Podle pokynů **Průvodce datovým připojením** zadejte název serveru a přihlašovací údaje. Klepněte na tlačítko **Další**.
5.  V dialogovém okně **Vyberte databázi obsahující data, která se má**vyberte databázi **ElasticDBQuery** .
6.  Vyberte tabulku **Zákazníci** v zobrazení seznamu a klikněte na tlačítko **Další**. Klepněte na tlačítko **Dokončit**.
7.  Ve formuláři **Importovat Data** ve skupinovém rámečku **Vyberte způsob zobrazení dat v sešitu**vyberte **tabulku** a klikněte na **OK**.

Všechny řádky v tabulce **Zákazníci** uložené v různých shards naplnění listu aplikace Excel.

## <a name="next-steps"></a>Další kroky
Teď můžete použít funkce dat aplikace Excel. Umožňuje připojovací řetězec s název serveru, název databáze a přihlašovací údaje vaší nástroje pro integraci BI a datové připojení k databázi pružná dotazu. Ujistěte se, že SQL Server je podporované jako zdroje dat pro nástroj. V nápovědě k databázi pružná dotazů a externí tabulky stejně jako jakékoli jiné databáze SQL serveru a tabulky SQL serveru, ke kterým by připojit pomocí svého nástroje.

### <a name="cost"></a>Pole náklady
Existuje bez dalších poplatků pro používání funkce dotazu pružná databáze. V současné době tato funkce je dostupná jenom u databází premium jako koncový bod však shards může mít libovolnou vrstvy služeb.

Ceny informace v tématu [SQL databáze ceny podrobnosti](https://azure.microsoft.com/pricing/details/sql-database/).


[AZURE.INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

<!--Image references-->
[1]: ./media/sql-database-elastic-query-getting-started/cmd-prompt.png
[2]: ./media/sql-database-elastic-query-getting-started/portal.png
[3]: ./media/sql-database-elastic-query-getting-started/tiers.png
[4]: ./media/sql-database-elastic-query-getting-started/details.png
[5]: ./media/sql-database-elastic-query-getting-started/exel-sources.png
<!--anchors-->
