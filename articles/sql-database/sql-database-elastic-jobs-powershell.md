<properties 
    pageTitle="Vytváření a správa pružná databáze úlohy pomocí prostředí PowerShell" 
    description="Prostředí PowerShell sloužící ke správě skupin databáze SQL Azure" 
    services="sql-database" documentationCenter=""  
    manager="jhubbard" 
    authors="ddove"/>

<tags 
    ms.service="sql-database" 
    ms.workload="sql-database" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="05/27/2016" 
    ms.author="ddove" />

# <a name="create-and-manage-a-sql-database-elastic-database-jobs-using-powershell-preview"></a>Vytváření a správa projektů pružná databáze SQL databáze pomocí prostředí PowerShell (verze preview)

> [AZURE.SELECTOR]
- [Azure portálu](sql-database-elastic-jobs-create-and-manage.md)
- [Prostředí PowerShell](sql-database-elastic-jobs-powershell.md)



Rozhraní API Powershellu pro **úlohy pružná databáze** (v náhledu), umožňují definovat skupiny databází, podle kterých se spustí skriptů. Tento článek ukazuje, jak vytvářet a spravovat **pružná databáze úlohy** pomocí rutin prostředí PowerShell. V tématu [Přehled pružná úlohy](sql-database-elastic-jobs-overview.md). 

## <a name="prerequisites"></a>Zjistit předpoklady pro
* Předplatné Azure. Bezplatnou zkušební verzi najdete v článku [bezplatnou zkušební verzi jeden měsíc](https://azure.microsoft.com/pricing/free-trial/).
* Sada databáze vytvořené pomocí pružná databázové nástroje. V tématu [Začínáme s pružná databázové nástroje](sql-database-elastic-scale-get-started.md).
* Azure Powershellu. Podrobné informace najdete v tématu [instalace a konfigurace prostředí PowerShell Azure](../powershell-install-configure.md).
* **Úlohy pružné databáze** Balíček Powershellu: viz [instalace pružná databáze úlohy](sql-database-elastic-jobs-service-installation.md)

### <a name="select-your-azure-subscription"></a>Vyberte předplatné Azure

Chcete-li vybrat název předplatné potřebujete předplatného Id (**-SubscriptionId**) nebo předplatného (**-SubscriptionName**). Pokud máte víc předplatných spusťte rutinu **Get-AzureRmSubscription** a zkopírujte informace o předplatném požadované ze sady výsledků. Až budete mít informace o předplatném, spusťte následující PowerShell Chcete-li nastavit jako výchozí, hlavně cílovém pro vytváření a Správa úloh toto předplatné:

    Select-AzureRmSubscription -SubscriptionId {SubscriptionID}

[Prostředí PowerShell ISE](https://technet.microsoft.com/library/dd315244.aspx) je vhodné pro použití k vytvoření a spuštění skriptů Powershellu úloh pružná databází.

## <a name="elastic-database-jobs-objects"></a>Pružná databázových úloh objektů

V následující tabulce jsou uvedeny se všechny typy objektů **Pružná databáze** úloh spolu s jeho popis a rozhraní API relevantní Powershellu.

<table style="width:100%">
  <tr>
    <th>Typ objektu</th>
    <th>Popis</th>
    <th>Související API prostředí PowerShell</th>
  </tr>
  <tr>
    <td>Přihlašovací údaje</td>
    <td>Uživatelské jméno a heslo pro použití při připojení k databázím pro provádění skriptů nebo aplikaci DACPACs. <p>Heslo musí být zašifrovaný před odesláním a ukládání do databáze pružná úloh databází.  Heslo dešifrovat službou pružná databáze úlohy pomocí přihlašovacích údajů vytvořené a nahrané z skript instalace.</td>
    <td><p>Get-AzureSqlJobCredential</p>
    <p>Nové AzureSqlJobCredential</p><p>Nastavení AzureSqlJobCredential</p></td></td>
  </tr>

  <tr>
    <td>Skript</td>
    <td>Transact-SQL skript, který se dá použít pro provádění různých databází.  Skript by byl vytvořen jako idempotent od služby zopakuje spuštění skriptu při k chybám.
    </td>
    <td>
    <p>Get-AzureSqlJobContent</p>
    <p>Get-AzureSqlJobContentDefinition</p>
    <p>Nové AzureSqlJobContent</p>
    <p>Nastavení AzureSqlJobContentDefinition</p>
    </td>
  </tr>

  <tr>
    <td>DACPAC</td>
    <td><a href="https://msdn.microsoft.com/library/ee210546.aspx">Aplikace datové vrstvy </a> balíček, který chcete použít přes databází.

    </td>
    <td>
    <p>Get-AzureSqlJobContent</p>
    <p>Nové AzureSqlJobContent</p>
    <p>Nastavení AzureSqlJobContentDefinition</p>
    </td>
  </tr>
  <tr>
    <td>Cílové databáze</td>
    <td>Databáze a název serveru směřující k databázi SQL Azure.

    </td>
    <td>
    <p>Get-AzureSqlJobTarget</p>
    <p>Nové AzureSqlJobTarget</p>
    </td>
  </tr>
  <tr>
    <td>Cíl shard mapy</td>
    <td>Kombinace cílové databáze a přihlašovací údaje k požívaný k určení informací uložených v mapování shard pružná databáze.
    </td>
    <td>
    <p>Get-AzureSqlJobTarget</p>
    <p>Nové AzureSqlJobTarget</p>
    <p>Nastavení AzureSqlJobTarget</p>
    </td>
  </tr>
<tr>
    <td>Vlastní kolekce cíl</td>
    <td>Skupina definované databází souhrnně použít pro spuštění.</td>
    <td>
    <p>Get-AzureSqlJobTarget</p>
    <p>Nové AzureSqlJobTarget</p>
    </td>
  </tr>
<tr>
    <td>Vlastní kolekce podřízené cíl</td>
    <td>Cílové databáze, na které se odkazuje z kolekce vlastní.</td>
    <td>
    <p>Přidání AzureSqlJobChildTarget</p>
    <p>Odebrat AzureSqlJobChildTarget</p>
    </td>
  </tr>

<tr>
    <td>Úlohy</td>
    <td>
    <p>Definice parametrů, které lze použít k aktivaci spuštění nebo ke splnění plánu projektu.</p>
    </td>
    <td>
    <p>Get-AzureSqlJob</p>
    <p>Nové AzureSqlJob</p>
    <p>Nastavení AzureSqlJob</p>
    </td>
  </tr>

<tr>
    <td>Spuštění úlohy</td>
    <td>
    <p>Kontejner úkolů potřebnou ke splnění spuštění skript nebo použili jste DACPAC cíl pomocí přihlašovací údaje pro připojení databáze s chybami zpracování podle zásad spuštění.</p>
    </td>
    <td>
    <p>Get-AzureSqlJobExecution</p>
    <p>Zahájení AzureSqlJobExecution</p>
    <p>Zastavit AzureSqlJobExecution</p>
    <p>Počkat AzureSqlJobExecution</p>
  </tr>

<tr>
    <td>Provádění úkolů projektu</td>
    <td>
    <p>Jednu jednotku práce ke splnění projektu.</p>
    <p>Pokud úlohu nedokáže úspěšně spustit výsledné zpráva o výjimce se Zaprotokolují a novou úlohu odpovídající vytvořili a budou provedeny podle zásad zadaný spouštění.</p></p>
    </td>
    <td>
    <p>Get-AzureSqlJobExecution</p>
    <p>Zahájení AzureSqlJobExecution</p>
    <p>Zastavit AzureSqlJobExecution</p>
    <p>Počkat AzureSqlJobExecution</p>
  </tr>

<tr>
    <td>Zásady spuštění úlohy</td>
    <td>
    <p>Ovládací prvky úlohy časové limity spuštění, omezení opakování a intervalů mezi opakování.</p>
    <p>Pružná úlohy databáze obsahuje výchozí zásady spuštění úlohy, které způsobují v podstatě nekonečné opakování selhání úkolů projektu s exponenciální zdvojnásobení intervalů mezi jednotlivými opakováními.</p>
    </td>
    <td>
    <p>Get-AzureSqlJobExecutionPolicy</p>
    <p>Nové AzureSqlJobExecutionPolicy</p>
    <p>Nastavení AzureSqlJobExecutionPolicy</p>
    </td>
  </tr>

<tr>
    <td>Plán</td>
    <td>
    <p>Čas na základě specifikace pro provádění opakovaném intervalu nebo najednou.</p>
    </td>
    <td>
    <p>Get-AzureSqlJobSchedule</p>
    <p>Nové AzureSqlJobSchedule</p>
    <p>Nastavení AzureSqlJobSchedule</p>
    </td>
  </tr>

<tr>
    <td>Aktivace projektu</td>
    <td>
    <p>Mapování mezi úlohy a rozpis spuštění úlohy aktivační událost podle plánu.</p>
    </td>
    <td>
    <p>Nové AzureSqlJobTrigger</p>
    <p>Odebrat AzureSqlJobTrigger</p>
    </td>
  </tr>
</table>

## <a name="supported-elastic-database-jobs-group-types"></a>Podporované úloh pružná databází seskupení typy
Úlohy provede skriptů Transact-SQL (T-SQL) nebo aplikace DACPACs přes celou skupinu databází. Po odeslání úlohy má provádět přes celou skupinu databází úlohy "rozbalí" na podřízené úkoly, kde každý provede požadované spuštění jednu databázi ve skupině. 
 
Existují dva typy skupin, které můžete vytvořit: 

* Skupina [Shard mapy](sql-database-elastic-scale-shard-map-management.md) : po odeslání úlohy jej směrovat mapě shard úlohy dotazy shard mapování a určení jeho aktuální sadu shards a vytvoří podřízené úlohy pro každou shard v mapě shard.
* Vlastní skupiny kolekce: vlastní definice sady databází. Když úlohy zaměřuje vlastní kolekci, vytvoří podřízené úlohy na každou databázi aktuálně v kolekci vlastní.

## <a name="to-set-the-elastic-database-jobs-connection"></a>Chcete-li nastavit pružná databáze úlohy připojení

Připojení, musí být nastavena na úlohy *databázi řízení* před použitím úloh rozhraní API. Tato rutina spuštěná spustí za tímto oknem přihlašovacích údajů se objevovat žádosti o uživatelské jméno a heslo vytvořené při instalaci úloh pružná databází. Všechny příklady uvedené v tomto tématu se předpokládá, že již byly provedeny tohoto prvního kroku.

Otevřete připojení k projektům pružná databáze:

    Use-AzureSqlJobConnection -CurrentAzureSubscription 

## <a name="encrypted-credentials-within-the-elastic-database-jobs"></a>Šifrované pověření v rámci úloh pružná databází

Přihlašovací údaje pro databázi lze vložit do úloh *ovládací prvek databáze* s jeho heslo zašifrované. Je nezbytné k ukládání přihlašovacích údajů k povolení úlohy provedeny na pozdější čas (pomocí plány projektu).
 
Šifrování funguje prostřednictvím certifikát vytvořený jako součást skript instalace. Skript instalace vytvoří a odešle certifikátu do cloudové služby Azure pro dešifrování uložené šifrovaná hesla. Cloudové služby Azure později ukládá veřejný klíč v rámci úlohy *ovládací prvek databáze* , která umožňuje rozhraní API prostředí PowerShell nebo Azure klasické portálu šifrování zadané heslo bez nutnosti certifikát, který má být místně nainstalovaný.
 
Hesla přihlašovací údaje jsou zašifrované a zabezpečené z uživatelé s přístupem jen pro čtení pružná databázových úloh objektů. Pořád zlými úmysly s přístup pro čtení i zápis k objektům pružné úloh databází extrahovat hesla. Přihlašovací údaje jsou určeny pro použití různých spuštění úlohy. Přihlašovací údaje předávají na cílové databáze se po vytvoření připojení. Momentálně neexistují žádná omezení na cílové databáze použité pro každou přihlašovací údaje, škodlivý uživatel může přidat cílové databáze pro danou databázi pod kontrolou uživateli se zlými úmysly. Uživatel může následně spuštění úlohy zacílení tuto databázi získat heslo přihlašovací údaje.

Zabezpečení doporučené postupy pro úlohy pružná databáze patří:

* Omezení použití rozhraní API důvěryhodných jednotlivcům.
* Přihlašovací údaje by měl mít aspoň oprávnění nutné provést úlohu.  Další informace najdete v tomto článku SQL Server MSDN [autorizace a oprávnění](https://msdn.microsoft.com/library/bb669084.aspx) .

### <a name="to-create-an-encrypted-credential-for-job-execution-across-databases"></a>Jak vytvořit šifrované přihlašovací údaje pro spuštění úloh přes databází

Pokud chcete vytvořit nové šifrované přihlašovacích údajů, [**rutiny Get-pověření**](https://technet.microsoft.com/library/hh849815.aspx) zobrazí výzvu k zadání uživatelské jméno a heslo, které lze předat [**rutinu New-AzureSqlJobCredential**](https://msdn.microsoft.com/library/mt346063.aspx).

    $credentialName = "{Credential Name}"
    $databaseCredential = Get-Credential
    $credential = New-AzureSqlJobCredential -Credential $databaseCredential -CredentialName $credentialName
    Write-Output $credential

### <a name="to-update-credentials"></a>Chcete-li aktualizovat přihlašovací údaje

Pokud dojde ke změně hesla, použijte [**rutinu Set-AzureSqlJobCredential**](https://msdn.microsoft.com/library/mt346062.aspx) a nastavit parametr **CredentialName** .

    $credentialName = "{Credential Name}"
    Set-AzureSqlJobCredential -CredentialName $credentialName -Credential $credential 

## <a name="to-define-an-elastic-database-shard-map-target"></a>Definování cíl pružná databáze shard mapy

Spuštění úlohy všechny databází v sadě shard (vytvořené pomocí [pružná databáze klienta knihovny](sql-database-elastic-database-client-library.md)), použijte mapě shard jako cílovou databázi. Tento příklad vyžaduje sharded aplikaci vytvořili pomocí klienta knihovny pružná databáze. Najdete v článku [Začínáme s ukázkovými nástroje pružná databáze](sql-database-elastic-scale-get-started.md).

Databáze shard mapy správce musí být nastavený jako cílovou databázi a potom mapy konkrétní shard musí být zadán jako cíle.

    $shardMapCredentialName = "{Credential Name}"
    $shardMapDatabaseName = "{ShardMapDatabaseName}" #example: ElasticScaleStarterKit_ShardMapManagerDb
    $shardMapDatabaseServerName = "{ShardMapServerName}"
    $shardMapName = "{MyShardMap}" #example: CustomerIDShardMap
    $shardMapDatabaseTarget = New-AzureSqlJobTarget -DatabaseName $shardMapDatabaseName -ServerName $shardMapDatabaseServerName
    $shardMapTarget = New-AzureSqlJobTarget -ShardMapManagerCredentialName $shardMapCredentialName -ShardMapManagerDatabaseName $shardMapDatabaseName -ShardMapManagerServerName $shardMapDatabaseServerName -ShardMapName $shardMapName
    Write-Output $shardMapTarget

## <a name="create-a-t-sql-script-for-execution-across-databases"></a>Vytvořit skript T-SQL pro provádění různých databází

Při vytváření T-SQL skripty pro spuštění, je velmi doporučené vytvářet, aby byly [idempotent](https://en.wikipedia.org/wiki/Idempotence) a pružné proti chybám. Pružná úloh databází zopakuje provádění skriptu při každém spuštění dojde k chybě, bez ohledu na klasifikace selhání.

Použijte [**rutinu New-AzureSqlJobContent**](https://msdn.microsoft.com/library/mt346085.aspx) můžete vytvořit a uložit skript pro spuštění a **- ContentName** a **- CommandText** parametry.

    $scriptName = "Create a TestTable"

    $scriptCommandText = "
    IF NOT EXISTS (SELECT name FROM sys.tables WHERE name = 'TestTable')
    BEGIN
        CREATE TABLE TestTable(
            TestTableId INT PRIMARY KEY IDENTITY,
            InsertionTime DATETIME2
        );
    END
    GO
    INSERT INTO TestTable(InsertionTime) VALUES (sysutcdatetime());
    GO"

    $script = New-AzureSqlJobContent -ContentName $scriptName -CommandText $scriptCommandText
    Write-Output $script

### <a name="create-a-new-script-from-a-file"></a>Vytvořit nový skript ze souboru
Pokud v souboru je definován skript T-SQL, slouží k importu skript:

    $scriptName = "My Script Imported from a File"
    $scriptPath = "{Path to SQL File}"
    $scriptCommandText = Get-Content -Path $scriptPath
    $script = New-AzureSqlJobContent -ContentName $scriptName -CommandText $scriptCommandText
    Write-Output $script

### <a name="to-update-a-t-sql-script-for-execution-across-databases"></a>Aktualizovat T-SQL skriptu pro spuštění přes databází  

Tento skript Powershellu aktualizuje příkazu text T-SQL pro existující skript.

Nastavení následujících proměnných tak, aby odrážely definici požadované skript nastavení:

    $scriptName = "Create a TestTable"
    $scriptUpdateComment = "Adding AdditionalInformation column to TestTable"
    $scriptCommandText = "
    IF NOT EXISTS (SELECT name FROM sys.tables WHERE name = 'TestTable')
    BEGIN
    CREATE TABLE TestTable(
        TestTableId INT PRIMARY KEY IDENTITY,
        InsertionTime DATETIME2
    );
    END
    GO

    IF NOT EXISTS (SELECT columns.name FROM sys.columns INNER JOIN sys.tables on columns.object_id = tables.object_id WHERE tables.name = 'TestTable' AND columns.name = 'AdditionalInformation')
    BEGIN
    ALTER TABLE TestTable
    ADD AdditionalInformation NVARCHAR(400);
    END
    GO

    INSERT INTO TestTable(InsertionTime, AdditionalInformation) VALUES (sysutcdatetime(), 'test');
    GO"

### <a name="to-update-the-definition-to-an-existing-script"></a>Aktualizujte definici existující skript

    Set-AzureSqlJobContentDefinition -ContentName $scriptName -CommandText $scriptCommandText -Comment $scriptUpdateComment 

## <a name="to-create-a-job-to-execute-a-script-across-a-shard-map"></a>K vytvoření úlohy provést skript přes shard mapy

Tento skript Powershellu spustí úlohu pro provádění skriptu přes každý shard mapování shard pružná měřítko.

Nastavení následujících proměnných podle potřeby skriptu a cíl:

    $jobName = "{Job Name}"
    $scriptName = "{Script Name}"
    $shardMapServerName = "{Shard Map Server Name}"
    $shardMapDatabaseName = "{Shard Map Database Name}"
    $shardMapName = "{Shard Map Name}"
    $credentialName = "{Credential Name}"
    $shardMapTarget = Get-AzureSqlJobTarget -ShardMapManagerDatabaseName $shardMapDatabaseName -ShardMapManagerServerName $shardMapServerName -ShardMapName $shardMapName 
    $job = New-AzureSqlJob -ContentName $scriptName -CredentialName $credentialName -JobName $jobName -TargetId $shardMapTarget.TargetId
    Write-Output $job

## <a name="to-execute-a-job"></a>Spuštění úlohy 

Provede tento skript Powershellu existujícího projektu:

Aktualizace následující proměnnou tak, aby odrážely název požadovaného úlohy, který jste spouštět:

    $jobName = "{Job Name}"
    $jobExecution = Start-AzureSqlJobExecution -JobName $jobName 
    Write-Output $jobExecution
 
## <a name="to-retrieve-the-state-of-a-single-job-execution"></a>K načtení stav provádění jednoho projektu

Získáte pomocí [**rutiny Get-AzureSqlJobExecution**](https://msdn.microsoft.com/library/mt346058.aspx) a nastavit parametr **JobExecutionId** stavu úlohy spuštění zobrazení.

    $jobExecutionId = "{Job Execution Id}"
    $jobExecution = Get-AzureSqlJobExecution -JobExecutionId $jobExecutionId
    Write-Output $jobExecution

Použijte stejné rutiny **Get-AzureSqlJobExecution** s parametrem **IncludeChildren** stavu podřízené spuštění úlohy, hlavně specifické stát pro každé spuštění úloh každou databázi cílené úloha zobrazení.

    $jobExecutionId = "{Job Execution Id}"
    $jobExecutions = Get-AzureSqlJobExecution -JobExecutionId $jobExecutionId -IncludeChildren
    Write-Output $jobExecutions 

## <a name="to-view-the-state-across-multiple-job-executions"></a>Chcete-li zobrazit stav napříč několika spuštění úlohy

[**Rutina Get-AzureSqlJobExecution**](https://msdn.microsoft.com/library/mt346058.aspx) obsahuje více volitelné parametrů, které lze zobrazit více spuštění úlohy filtrované prostřednictvím uvedených parametrů. Následující znázorňuje některé z možných způsobů použití Get-AzureSqlJobExecution:

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

Tento skript Powershellu mohou sloužit k zobrazení podrobností o provádění úkolů úlohy, což je užitečné při ladění chyb spuštění.

    $jobTaskExecutionId = "{Job Task Execution Id}"
    $jobTaskExecution = Get-AzureSqlJobTaskExecution -JobTaskExecutionId $jobTaskExecutionId
    Write-Output $jobTaskExecution

## <a name="to-retrieve-failures-within-job-task-executions"></a>K načtení k chybám v rámci spuštění úkolů projektu

**Objekt JobTaskExecution** zahrnuje vlastnost životním cyklu úkolu společně s vlastnost zprávy. Pokud provádění úkolů projektu selhalo vlastnost životního cyklu nastaví se *nezdařilo* a vlastností zprávy nastaví výsledné zpráva o výjimce a jeho zásobníku. Pokud úlohy se nezdařila, je důležité podrobnosti úlohy, které se nezdařila pro danou úlohu.

    $jobExecutionId = "{Job Execution Id}"
    $jobTaskExecutions = Get-AzureSqlJobTaskExecution -JobExecutionId $jobExecutionId
    Foreach($jobTaskExecution in $jobTaskExecutions) 
        {
        if($jobTaskExecution.Lifecycle -ne 'Succeeded')
            {
            Write-Output $jobTaskExecution
            }
        }

## <a name="to-wait-for-a-job-execution-to-complete"></a>Až provádění práce k dokončení

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
    $executionPolicy = New-AzureSqlJobExecutionPolicy -ExecutionPolicyName $executionPolicyName -InitialRetryInterval $initialRetryInterval -JobTimeout $jobTimeout -MaximumAttempts $maximumAttempts -MaximumRetryInterval $maximumRetryInterval 
    -RetryIntervalBackoffCoefficient $retryIntervalBackoffCoefficient
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

Pružná úloh databází podporuje žádostí o zrušení úloh.  Pokud pružná úloh databází zjistí žádost o zrušení projektu probíhajících, pokusí se ukončit úlohu.

Že pružná úloh databází může provádět zrušení dvěma různými způsoby:

1. Zrušit aktuálně provádění úkolů: Pokud úkol je spuštěná aktuálně ke zjištění zrušení, zrušení pokusí v rámci aktuálně spouštěné poměr úkolu.  Příklad: Pokud dlouhodobé dotazu aktuálně provedených při pokusu o zrušení se budou pokus o zruší dotaz.
2. Zrušení opakování úkolu: Pokud zrušení je zjištěno podprocesem ovládací prvek před úkolu je spuštěn pro spuštění, vlákna ovládací prvek zabránit spuštění úkol a deklarovat žádost jako zrušená.

Pokud zrušení úlohy žádá nadřazené projektu, bude žádost o zrušení dodržet nadřazené úlohy a všechny její podřízené úlohy.
 
Odeslání žádosti o zrušení, získáte pomocí [**rutiny zastavit AzureSqlJobExecution**](https://msdn.microsoft.com/library/mt346053.aspx) a nastavit parametr **JobExecutionId** .

    $jobExecutionId = "{Job Execution Id}"
    Stop-AzureSqlJobExecution -JobExecutionId $jobExecutionId

## <a name="to-delete-a-job-and-job-history-asynchronously"></a>Odstranění úlohy a historie úlohy asynchronní

Pružná úloh databází podporuje asynchronní odstranění úlohy. Úlohy můžete označit pro odstranění a systém odstraní úlohy a všechny její historie úlohy po spuštění všechny úlohy dokončili pro daný úkol. Systému nebude zrušit automatické spuštění aktivní úlohy.  

Vyvolání [**Zastavit AzureSqlJobExecution**](https://msdn.microsoft.com/library/mt346053.aspx) zrušit aktivní úlohy spuštění.

Aktivace odstranění úlohy, získáte pomocí [**rutiny odebrat AzureSqlJob**](https://msdn.microsoft.com/library/mt346083.aspx) a nastavit parametr **název_úlohy** .

    $jobName = "{Job Name}"
    Remove-AzureSqlJob -JobName $jobName
 
## <a name="to-create-a-custom-database-target"></a>Vytvoření vlastní databáze cíl

Můžete definovat cílů vlastní databáze pro přímého spuštění nebo zahrnutí ve skupině vlastní databázi. Například protože **fondů pružné databáze** dosud nepodporuje doplňky přímo pomocí prostředí PowerShell API, můžete vytvořit cílovou vlastní databáze a cílové kolekce vlastní databáze, která zahrnuje všechny databáze z fondu.

Nastavení následujících proměnných tak, aby odrážely informace požadované databáze:

    $databaseName = "{Database Name}"
    $databaseServerName = "{Server Name}"
    New-AzureSqlJobDatabaseTarget -DatabaseName $databaseName -ServerName $databaseServerName 

## <a name="to-create-a-custom-database-collection-target"></a>Vytvoření cílové kolekce vlastní databáze

Použijte rutinu [**New-AzureSqlJobTarget**](https://msdn.microsoft.com/library/mt346077.aspx) k definování cílové kolekce vlastní databáze povolíte spuštění napříč několika cílů definovaný databáze. Po vytvoření skupiny databáze, může být přidružené k vlastní kolekci cílové databáze.

Nastavení následujících proměnných tak, aby odrážely konfiguraci cílové požadované vlastní kolekce:

    $customCollectionName = "{Custom Database Collection Name}"
    New-AzureSqlJobTarget -CustomCollectionName $customCollectionName 

### <a name="to-add-databases-to-a-custom-database-collection-target"></a>Přidání databáze do cílové kolekce vlastní databáze

Chcete-li přidat databázi na určitou kolekci vlastní rutina [**AzureSqlJobChildTarget přidat**](https://msdn.microsoft.comlibrary/mt346064.aspx) .

    $databaseServerName = "{Database Server Name}"
    $databaseName = "{Database Name}"
    $customCollectionName = "{Custom Database Collection Name}"
    Add-AzureSqlJobChildTarget -CustomCollectionName $customCollectionName -DatabaseName $databaseName -ServerName $databaseServerName 

#### <a name="review-the-databases-within-a-custom-database-collection-target"></a>Kontrola databází v rámci kolekce cíle vlastní databáze

Použijte rutinu [**Get-AzureSqlJobTarget**](https://msdn.microsoft.com/library/mt346077.aspx) k načtení podřízené databází v rámci kolekce cíle vlastní databázi. 
 
    $customCollectionName = "{Custom Database Collection Name}"
    $target = Get-AzureSqlJobTarget -CustomCollectionName $customCollectionName
    $childTargets = Get-AzureSqlJobTarget -ParentTargetId $target.TargetId
    Write-Output $childTargets

### <a name="create-a-job-to-execute-a-script-across-a-custom-database-collection-target"></a>Vytvoření projektu provést skript přes cílové kolekce vlastní databáze

Použijte rutinu [**New-AzureSqlJob**](https://msdn.microsoft.com/library/mt346078.aspx) k vytvoření úlohy proti skupiny databází definované cílové kolekce vlastní databázi. Pružná databáze projektů rozbalte projektu do více úloh podřízené odpovídající jednotlivým databáze přidružený k cílové kolekce vlastní databáze a zrušte zaškrtnutí spuštění skriptu každou databázi. Znovu je důležité, že skripty jsou idempotent pružné opakování.

    $jobName = "{Job Name}"
    $scriptName = "{Script Name}"
    $customCollectionName = "{Custom Collection Name}"
    $credentialName = "{Credential Name}"
    $target = Get-AzureSqlJobTarget -CustomCollectionName $customCollectionName
    $job = New-AzureSqlJob -JobName $jobName -CredentialName $credentialName -ContentName $scriptName -TargetId $target.TargetId
    Write-Output $job

## <a name="data-collection-across-databases"></a>Shromažďování dat přes databází

Úlohy slouží k provedení dotazu přes celou skupinu databází a odešlete tak výsledky konkrétní tabulky. Tabulku můžete dotazovaném ve skutečnosti a zobrazte výsledky dotazu na každou databázi. Tato funkce poskytuje asynchronní způsob spuštění dotazu na přes mnoho databází. Neúspěšné pokusy jsou automaticky zpracovány prostřednictvím opakování.

Zadaný cílové tabulky se automaticky vytvoří Pokud ještě neexistuje. Novou tabulku odpovídá schématu sadu vrácených výsledků. Pokud skript vrátí více sad výsledků dotazu, odeslat úloh pružná databází pouze první do cílové tabulky.

Tento skript Powershellu provádí skript a shromažďuje jeho výsledky do zadané tabulky. Tento skript předpokládá, že skript T-SQL byl vytvořen které výstupy sady výsledkem je jedna hodnota a vytvořené cílové kolekce vlastní databázi.

Tento skript používá rutinu [**Get-AzureSqlJobTarget**](https://msdn.microsoft.com/library/mt346077.aspx) . Nastavení parametrů skript, přihlašovací údaje a cílové spuštění:

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

### <a name="to-create-and-start-a-job-for-data-collection-scenarios"></a>Vytvoření a spuštění úlohy scénáře shromažďování dat

Tento skript používá rutinu [**Start AzureSqlJobExecution**](https://msdn.microsoft.com/library/mt346055.aspx) .
 
    $job = New-AzureSqlJob -JobName $jobName 
    -CredentialName $executionCredentialName 
    -ContentName $scriptName 
    -ResultSetDestinationServerName $destinationServerName 
    -ResultSetDestinationDatabaseName $destinationDatabaseName 
    -ResultSetDestinationSchemaName $destinationSchemaName 
    -ResultSetDestinationTableName $destinationTableName 
    -ResultSetDestinationCredentialName $destinationCredentialName 
    -TargetId $target.TargetId
    Write-Output $job
    $jobExecution = Start-AzureSqlJobExecution -JobName $jobName
    Write-Output $jobExecution

## <a name="to-schedule-a-job-execution-trigger"></a>Naplánování aktivační spuštění úlohy

Tento skript Powershellu mohou sloužit k vytvoření opakované plánu. Tento skript používá minute interval, ale [**Nový AzureSqlJobSchedule**](https://msdn.microsoft.com/library/mt346068.aspx) taky podporuje - DayInterval - HourInterval, - MonthInterval a - WeekInterval parametry. Plány, které spouštět jenom jednou se dají vytvářet podle předávání - jednorázově.

Vytvoření nového plánu:

    $scheduleName = "Every one minute"
    $minuteInterval = 1
    $startTime = (Get-Date).ToUniversalTime()
    $schedule = New-AzureSqlJobSchedule 
    -MinuteInterval $minuteInterval 
    -ScheduleName $scheduleName 
    -StartTime $startTime 
    Write-Output $schedule

### <a name="to-trigger-a-job-executed-on-a-time-schedule"></a>Ke spuštění úlohy na časový plán

Aktivační události projektu je to možné definovat mít spouštět podle časového plánu projektu. Tento skript Powershellu můžete použít k vytvoření aktivační události projektu.

Použít [Nový AzureSqlJobTrigger](https://msdn.microsoft.com/library/mt346069.aspx) a nastavte následující proměnné tak, aby odpovídaly požadovaný projekt a plán:

    $jobName = "{Job Name}"
    $scheduleName = "{Schedule Name}"
    $jobTrigger = New-AzureSqlJobTrigger
    -ScheduleName $scheduleName
    –JobName $jobName
    Write-Output $jobTrigger

### <a name="to-remove-a-scheduled-association-to-stop-job-from-executing-on-schedule"></a>Odebrání naplánovaných přidružení ukončit úlohu spuštění plánu

Pokud chcete přestat opakovaném spuštění úlohy pomocí aktivační události projektu, je možné odebrat aktivační události projektu. Odebrání aktivační úlohu ukončit úlohu z prováděný podle plánu pomocí [**rutiny AzureSqlJobTrigger odebrat**](https://msdn.microsoft.com/library/mt346070.aspx).

    $jobName = "{Job Name}"
    $scheduleName = "{Schedule Name}"
    Remove-AzureSqlJobTrigger 
    -ScheduleName $scheduleName 
    -JobName $jobName

### <a name="retrieve-job-triggers-bound-to-a-time-schedule"></a>Načtení aktivačních událostí projektu vázaný na časový plán

Tento skript Powershellu lze získat a zobrazit úlohy aktivace registrované pro konkrétní časový plán.

    $scheduleName = "{Schedule Name}"
    $jobTriggers = Get-AzureSqlJobTrigger -ScheduleName $scheduleName
    Write-Output $jobTriggers

### <a name="to-retrieve-job-triggers-bound-to-a-job"></a>K načtení aktivačních událostí projektu vázaný na projektu 

[Get-AzureSqlJobTrigger](https://msdn.microsoft.com/library/mt346067.aspx) umožňuje získat a zobrazit plány obsahující registrovaných projektu.

    $jobName = "{Job Name}"
    $jobTriggers = Get-AzureSqlJobTrigger -JobName $jobName
    Write-Output $jobTriggers

## <a name="to-create-a-data-tier-application-dacpac-for-execution-across-databases"></a>Vytvoření aplikace osy dat (DACPAC) pro provádění různých databází

Vytvoření DACPAC najdete v tématu [datové vrstvy aplikací](https://msdn.microsoft.com/library/ee210546.aspx). Abyste mohli nasadit DACPAC, použijte [rutinu New-AzureSqlJobContent](https://msdn.microsoft.com/library/mt346085.aspx). DACPAC musí být přístupné pro službu. Je vhodné ukládat vytvořené DACPAC k základnímu úložišti Azure a vytvářet [Sdílené podpis přístup](../storage/storage-dotnet-shared-access-signature-part-1.md) pro DACPAC.

    $dacpacUri = "{Uri}"
    $dacpacName = "{Dacpac Name}"
    $dacpac = New-AzureSqlJobContent -DacpacUri $dacpacUri -ContentName $dacpacName 
    Write-Output $dacpac

### <a name="to-update-a-data-tier-application-dacpac-for-execution-across-databases"></a>Aktualizovat datové vrstvy aplikace (DACPAC) pro spuštění přes databází

Existující DACPACs registrována uvnitř pružná úloh databází lze aktualizovat tak, aby ukazovaly na nové URI. Použijte [**rutinu Set-AzureSqlJobContentDefinition**](https://msdn.microsoft.com/library/mt346074.aspx) k aktualizaci URI DACPAC na existující registrovaných DACPAC:

    $dacpacName = "{Dacpac Name}"
    $newDacpacUri = "{Uri}"
    $updatedDacpac = Set-AzureSqlJobDacpacDefinition -ContentName $dacpacName -DacpacUri $newDacpacUri
    Write-Output $updatedDacpac

## <a name="to-create-a-job-to-apply-a-data-tier-application-dacpac-across-databases"></a>K vytvoření úlohy použít aplikace osy dat (DACPAC) napříč databázemi

Po vytvoření DACPAC v rámci pružná úlohy databáze můžete použít DACPAC přes celou skupinu databází vytvořen projektu. Tento skript Powershellu lze použít k vytvoření úlohy DACPAC k dokumentu mezi vlastní skupinu databází:

    $jobName = "{Job Name}"
    $dacpacName = "{Dacpac Name}"
    $customCollectionName = "{Custom Collection Name}"
    $credentialName = "{Credential Name}"
    $target = Get-AzureSqlJobTarget 
    -CustomCollectionName $customCollectionName
    $job = New-AzureSqlJob 
    -JobName $jobName 
    -CredentialName $credentialName 
    -ContentName $dacpacName -TargetId $target.TargetId
    Write-Output $job 

[AZURE.INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

<!--Image references-->
[1]: ./media/sql-database-elastic-jobs-powershell/cmd-prompt.png
[2]: ./media/sql-database-elastic-jobs-powershell/portal.png
<!--anchors-->
