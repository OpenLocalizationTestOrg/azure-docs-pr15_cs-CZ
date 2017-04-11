<properties
   pageTitle="Použití výkonnosti v Azure diagnostiky | Microsoft Azure"
   description="Použití výkonnosti služby Azure cloud services nebo virtuálního počítače k vyhledání problémů a ladění výkonu."
   services="cloud-services"
   documentationCenter=".net"
   authors="rboucher"
   manager="jwhit"
   editor="tysonn" />
<tags
   ms.service="cloud-services"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="02/29/2016"
   ms.author="robb" />

# <a name="create-and-use-performance-counters-in-an-azure-application"></a>Kam zmizely výkonnosti v aplikaci Azure

Tento článek popisuje výhody a jak výkonnosti přepněte do aplikace Azure. Můžete využít k shromažďovat data o najít kritické body a ladění výkonu systému a aplikací.

K dispozici pro Windows Server, IIS a ASP.NET výkonnosti můžete taky shromažďují a požívaný k určení stavu Azure web role, role pracovní a virtuálních počítačích. Můžete taky vytvořit a používat vlastní výkonnosti.  

Můžete zkoumat data čítačů
1. Přímo na hostiteli aplikace pomocí nástroje pro sledování výkonu otevřít prostřednictvím Vzdálená plocha
2. S System Center Operations Manager pomocí Azure Management Pack
3. Pomocí dalších pro přístup k datům diagnostické nástroje pro sledování převedena Azure úložiště. Další informace naleznete v tématu [ukládání a zobrazení diagnostiky dat v úložišti Azure](https://msdn.microsoft.com/library/azure/hh411534.aspx) .  

Další informace o sledování výkonu aplikace [Azure klasické portál](http://manage.azure.com/)najdete v článku [jak ke Cloudovým službám Monitor](https://www.azure.com/manage/services/cloud-services/how-to-monitor-a-cloud-service/).

Další podrobné pokyny týkající se vytváření protokolování a sledování strategie a pomocí nástroje pro diagnostiku a jinými postupy k řešení potíží a optimalizace Azure aplikací najdete v článku [Poradce při potížích s doporučené postupy pro vývoj aplikací Azure](https://msdn.microsoft.com/library/azure/hh771389.aspx).


## <a name="enable-performance-counter-monitoring"></a>Povolit sledování čítač výkonu

Ve výchozím nastavení nejsou povoleny výkonnosti. Aplikace nebo spuštění úkolu třeba změnit výchozí konfigurace agent diagnostice zahrnout konkrétní výkonnosti, které chcete sledovat pro každou roli instanci.

### <a name="performance-counters-available-for-microsoft-azure"></a>Výkonnosti umožňující Microsoft Azure

Azure poskytuje podmnožinu výkonnosti k dispozici pro Windows Server, IIS a zásobníku ASP.NET. V následující tabulce jsou uvedeny některé výkonnosti konkrétní potřebné pro Azure aplikace.

|Kategorie čítače: Objektu (Instance)|Název čítače      |Odkaz|
|---|---|---|
|Výjimky CLR .NET (_globální_)|# Vyvolané vyvolané / s   |Výjimky výkonnosti|
|Paměť .NET CLR (_globální_)    |% Času v ° c            |Výkonnosti paměti|
|TECHNOLOGIE ASP.NET                      |Restartování aplikace    |Výkonnosti technologie ASP.NET|
|TECHNOLOGIE ASP.NET                      |Žádost o času spuštění  |Výkonnosti technologie ASP.NET|
|TECHNOLOGIE ASP.NET                      |Požadavky na odpojeno   |Výkonnosti technologie ASP.NET|
|TECHNOLOGIE ASP.NET                      |Restartování pracovního procesu |Výkonnosti technologie ASP.NET|
|Aplikace ASP.NET (__Celkem__)|Požadavky (celkem)        |Výkonnosti technologie ASP.NET|
|Aplikace ASP.NET (__Celkem__)|Žádosti o/Sec          |Výkonnosti technologie ASP.NET|
|ASP.NET v4.0.30319           |Žádost o času spuštění  |Výkonnosti technologie ASP.NET|
|ASP.NET v4.0.30319           |Žádost o prodleva       |Výkonnosti technologie ASP.NET|
|ASP.NET v4.0.30319           |Aktuální požadavky        |Výkonnosti technologie ASP.NET|
|ASP.NET v4.0.30319           |Požadavky ve frontě         |Výkonnosti technologie ASP.NET|
|ASP.NET v4.0.30319           |Zamítnuté požadavky       |Výkonnosti technologie ASP.NET|
|Paměti                       |K dispozici MB        |Výkonnosti paměti|
|Paměti                       |Potvrzené bajtů         |Výkonnosti paměti|
|Procesor(_Total)            |Čas procesoru %        |Výkonnosti technologie ASP.NET|
|TCPv4                        |Připojení k chybám     |TCP objektu|
|TCPv4                        |Připojení |TCP objektu|
|TCPv4                        |Obnovení připojení       |TCP objektu|
|TCPv4                        |Odeslané segmenty/s       |TCP objektu|
|Interface(*) síť         |Přijaté bajty/s      |Objekt rozhraní sítě|
|Interface(*) sítě         |Odeslané bajty/s          |Objekt rozhraní sítě|
|Sítě rozhraní (Microsoft Virtual Machine Bus síťový adaptér _2)|Přijaté bajty/s|Objekt rozhraní sítě|
|Sítě rozhraní (Microsoft Virtual Machine Bus síťový adaptér _2)|Odeslané bajty/s|Objekt rozhraní sítě|
|Sítě rozhraní (Microsoft Virtual Machine Bus síťový adaptér _2)|Bajty celkem/s|Objekt rozhraní sítě|

## <a name="create-and-add-custom-performance-counters-to-your-application"></a>Vytvoření a přidání vlastních výkonnosti do aplikace

Azure podporuje vytváření a změny vlastních výkonnosti web rolí a kolegy. Čítače lze sledovat a monitorovat chování specifické pro aplikaci. Můžete vytvořit a kategorií čítačů vlastní výkonu a specifikátory Zabránění spuštění úkolu, web role nebo pracovní role se zvýšenými oprávněními.

>[AZURE.NOTE] Kód, který změní vlastní výkonnosti musí zvýšenými oprávněními spustit. Pokud je tento kód v webu nebo pracovního role, role musí obsahovat značku <Runtime executionContext="elevated" /> v souboru ServiceDefinition.csdef rolí inicializace.

Vlastní data čítačů můžete posílat Azure úložiště použití agenta diagnostických nástrojů.

Standardní data čítačů je generováno aplikací Azure procesů. Vlastní data čítačů musí vytvořit webovou aplikaci role role nebo kolegy. Informace o typech data, která mohou být uloženy vlastní výkonnosti naleznete v tématu [Typy čítačů výkonu](https://msdn.microsoft.com/library/z573042h.aspx) . Příklad, který vytvoří a nastaví vlastní data čítačů v roli webu najdete v článku [Jako vzorku](http://code.msdn.microsoft.com/azure/) .

## <a name="store-and-view-performance-counter-data"></a>Ukládání a zobrazení data čítačů

Azure ukládá data čítačů s další diagnostické informace. Tato data jsou k dispozici pro vzdálené sledování instanci rolí je spuštěná pomocí přístup ke vzdálené ploše zobrazíte nástroje, například sledování výkonu. Uchovávat data mimo instanci rolí, musíte agenta diagnostiky přenášet data Azure úložiště. Omezení velikosti jeho výkonu uložených v mezipaměti údaje možné konfigurovat agenta diagnostiky nebo ho může nakonfigurovat tak, aby byl součástí sdílené limit pro diagnostiku data. Další informace o nastavení vyrovnávací paměť najdete v tématu [OverallQuotaInMB](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.diagnostics.diagnosticmonitorconfiguration.overallquotainmb.aspx) a [DirectoriesBufferConfiguration](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.diagnostics.directoriesbufferconfiguration.aspx). Základní informace o nastavení diagnostických nástrojů agent pro přenos dat do účtu úložiště naleznete v tématu [ukládání a zobrazení diagnostiky dat v úložišti Azure](https://msdn.microsoft.com/library/azure/hh411534.aspx) .

Jednotlivé instance čítač nakonfigurované výkonu nastavené při zadaném vzorkování a vybraných dat je převedena na úložiště účet žádost o plánované přenos nebo žádost o konverzaci na vyžádání přenos. Automatické předávání může být naplánován tak často, jak jednou za minutu. Data čítačů převedeny agentem diagnostických nástrojů se uloží do tabulky WADPerformanceCountersTable, v okně účet úložiště. V této tabulce mohou přistupovat a dotazovaném s standardní Azure úložiště rozhraní API metody. V tématu [Microsoft Azure jako ukázka](http://code.msdn.microsoft.com/Windows-Azure-PerformanceCo-7d80ebf9) příklad dotazů a zobrazení výkonu data z tabulky WADPerformanceCountersTable.

>[AZURE.NOTE] V závislosti na Diagnostika agent přenos počet_plateb a latence fronty poslední údaje výkonu v účtu úložiště může být zastaralý několik minut.

## <a name="enable-performance-counters-using-diagnostics-configuration-file"></a>Povolení výkonnosti pomocí diagnostických nástrojů konfiguračního souboru

Chcete-li povolit výkonnosti v aplikaci Azure pomocí následujícího postupu.

## <a name="prerequisites"></a>Zjistit předpoklady pro

V této části předpokládá, že jste importovali monitor diagnostiky do aplikace a přidali konfiguračního souboru diagnostiky do vašeho řešení Visual Studio (diagnostics.wadcfg SDK 2.4 a níže nebo diagnostics.wadcfgx SDK 2,5 a výše). Viz kroky 1 a 2 v [Povolení Diagnostika v Azure cloudovými službami a virtuálních počítačích](./cloud-services-dotnet-diagnostics.md)) Další informace.

## <a name="step-1-collect-and-store-data-from-performance-counters"></a>Krok 1: Shromažďovat a ukládat data z výkonnosti

Až přidáte soubor diagnostiky do vašeho řešení Visual Studio můžete nakonfigurovat shromažďování a úložiště jeho výkon údaje v Azure aplikace. Důvodem je přidáním výkonnosti diagnostických nástrojů soubor. Diagnostika, včetně výkonnosti, je nejdřív údaje instance. Data pak uložena v tabulce WADPerformanceCountersTable ve službě tabulky Azure, budete taky muset zadat účet úložiště v aplikaci. Pokud jste testování aplikace místně v emulátoru výpočet, můžete také uložit diagnostiky data místně v emulátoru úložiště. Před ukládání diagnostických nástrojů dat musíte nejdřív přejděte na [portál Azure klasické](http://manage.windowsazure.com/) a vytvořit účet úložiště. Doporučený postup je vyhledejte účtu úložiště ve stejném umístění geo jako Azure aplikace a zabránit tak platební náklady na externí šířku pásma a latence snížit.

### <a name="add-performance-counters-to-the-diagnostics-file"></a>Přidání výkonnosti k souboru diagnostických nástrojů

Existuje mnoho čítačů, které můžete použít. Následující příklad ukazuje několik výkonnosti, které jsou vhodné pro web a sledování role pracovních.

Otevřete soubor diagnostiky (diagnostics.wadcfg SDK 2.4 a níže nebo diagnostics.wadcfgx v SDK 2,5 a výše) a na požadovaný prvek DiagnosticMonitorConfiguration přidejte následující text:

```
    <PerformanceCounters bufferQuotaInMB="0" scheduledTransferPeriod="PT30M">
       <PerformanceCounterConfiguration counterSpecifier="\Memory\Available Bytes" sampleRate="PT30S" />
       <PerformanceCounterConfiguration counterSpecifier="\Processor(_Total)\% Processor Time" sampleRate="PT30S" />

    <!-- Use the Process(w3wp) category counters in a web role -->

       <PerformanceCounterConfiguration counterSpecifier="\Process(w3wp)\% Processor Time" sampleRate="PT30S" />
       <PerformanceCounterConfiguration counterSpecifier="\Process(w3wp)\Private Bytes" sampleRate="PT30S" />
       <PerformanceCounterConfiguration counterSpecifier="\Process(w3wp)\Thread Count" sampleRate="PT30S" />

    <!-- Use the Process(WaWorkerHost) category counters in a worker role.
       <PerformanceCounterConfiguration counterSpecifier="\Process(WaWorkerHost)\% Processor Time" sampleRate="PT30S" />
       <PerformanceCounterConfiguration counterSpecifier="\Process(WaWorkerHost)\Private Bytes" sampleRate="PT30S" />
       <PerformanceCounterConfiguration counterSpecifier="\Process(WaWorkerHost)\Thread Count" sampleRate="PT30S" />
    -->

       <PerformanceCounterConfiguration counterSpecifier="\.NET CLR Interop(_Global_)\# of marshalling" sampleRate="PT30S" />
       <PerformanceCounterConfiguration counterSpecifier="\.NET CLR Loading(_Global_)\% Time Loading" sampleRate="PT30S" />
       <PerformanceCounterConfiguration counterSpecifier="\.NET CLR LocksAndThreads(_Global_)\Contention Rate / sec" sampleRate="PT30S" />
       <PerformanceCounterConfiguration counterSpecifier="\.NET CLR Memory(_Global_)\# Bytes in all Heaps" sampleRate="PT30S" />
       <PerformanceCounterConfiguration counterSpecifier="\.NET CLR Networking(_Global_)\Connections Established" sampleRate="PT30S" />
       <PerformanceCounterConfiguration counterSpecifier="\.NET CLR Remoting(_Global_)\Remote Calls/sec" sampleRate="PT30S" />
       <PerformanceCounterConfiguration counterSpecifier="\.NET CLR Jit(_Global_)\% Time in Jit" sampleRate="PT30S" />
    </PerformanceCounters>
```

Atribut bufferQuotaInMB, který určuje maximální velikost úložiště souborů systému, který je k dispozici pro shromažďování dat (Azure protokoly, protokoly služby IIS, atd.). Výchozí hodnota je 0. Při dosažení kvóty od nejstaršího data odstraněna jako se přidá nová data. Součet všech vlastností bufferQuotaInMB musí být větší než hodnota atributu OverallQuotaInMB. Podrobnější informace o zjištění, jakou velikost úložiště bude nutné pro shromažďování informací diagnostiky naleznete v části Nastavení WAD [Poradce při potížích s doporučené postupy pro vývoj aplikací Azure](https://msdn.microsoft.com/library/windowsazure/hh771389.aspx).

Atribut scheduledTransferPeriod, který určuje interval mezi plánovaná přenosy dat, zaokrouhleno nahoru na nejbližší minutu. V následujícím příkladu je nastavena na PT30M (30 minut). Nastavení doby převodu na malé hodnotě, například 1 minuta nepříznivě ovlivnit výkon aplikace ve výrobním ale může být užitečné při shlédnutí diagnostiky rychle pracovat při testování. Plánované přenos období by měly být dostatečně stručné zajistit, že se dat diagnostiky nepřepíše instance, ale dostatečně velký k tomu, že ho nebude mít vliv na výkon aplikace.

Atribut counterSpecifier určuje výkonu proti shromáždit. Atribut sampleRate určuje rychlost niž čítač výkonu se odeberou, v tomto případě 30 sekund.

Po přidání výkonnosti, která chcete shromažďovat, uložte provedené změny diagnostických nástrojů soubor. Pak budete muset zadat úložiště účet, který diagnostiky data budou uložena do.

### <a name="specify-the-storage-account"></a>Zadejte účet úložiště

Zachování diagnostické informace ke svému účtu Azure úložiště, je nutné zadat připojovací řetězec v souboru konfigurace (ServiceConfiguration.cscfg) služby.

Pro Azure SDK 2,5 účet úložiště můžete být zadán v souboru diagnostics.wadcfgx.

>[AZURE.NOTE] Tyto pokyny se týkají pouze Azure SDK 2.4 a pod. Pro Azure SDK 2,5 účet úložiště můžete být zadán v souboru diagnostics.wadcfgx.

Nastavení připojení řetězce:

1. Otevřete soubor ServiceConfiguration.Cloud.cscfg Oblíbené textovém editoru a nastavte připojovací řetězec pro úložišti. Hodnoty *název účtu* a *AccountKey* se nacházejí v portálu Azure klasické na řídicím panelu úložiště účtu, klikněte v části Správa klíčů.

    ```
    <ConfigurationSettings>
       <Setting name="Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString" value="DefaultEndpointsProtocol=https;AccountName=<name>;AccountKey=<key>"/>
    </ConfigurationSettings>
    ```
2. Uložte soubor ServiceConfiguration.Cloud.cscfg.

3. Otevřete soubor ServiceConfiguration.Local.cscfg a ověřte, že UseDevelopmentStorage je nastavený na hodnotu true.

    ```
    <ConfigurationSettings>
      <Settingname="Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString" value="UseDevelopmentStorage=true"/>
    </ConfigurationSettings>
    ```
Teď jsou nastavené připojovací řetězce, bude aplikace uchovávat data diagnostice ke svému účtu úložiště při nasazení aplikace.
4. Uložení a vytvoření projektu a potom nasazení aplikace.

## <a name="step-2-optional-create-custom-performance-counters"></a>Krok 2: (Volitelné) vytvořit vlastní výkonnosti

Kromě předdefinovaných výkonnosti můžete přidat vlastní vlastní výkonnosti ke sledování webu nebo pracovního role. Vlastní výkonnosti lze sledovat a sledovat chování specifické pro aplikaci a můžou být vytvořené nebo smazali spuštění úkolu, web role nebo pracovní role se zvýšenými oprávněními.

Agent Azure diagnostiky aktualizuje výkonu čítač konfigurace ze souboru .wadcfg jednu minutu po spuštění.  Pokud vytvoříte vlastní výkonnosti v metodu při spuštění a spuštěním úkolů trvat déle než jednu minutu provést, vlastní výkonnosti nebude byly vytvořeny agenta Azure diagnostiky pokusí načíst je.  V tomto scénáři uvidí, že Azure diagnostiky správně zaznamenává všechna data diagnostiky kromě vlastní výkonnosti.  Tento problém vyřešíte vytvořit výkonnosti spuštění úkolu nebo přesunout některé z úkolu spuštění fungovat metody při spuštění po vytvoření výkonnosti.

Proveďte následující postup vytvoření jednoduchého vlastní výkonu proti s názvem "\MyCustomCounterCategory\MyButton1Counter":

1. Otevřete soubor definice služby (CSDEF) aplikace.
2. Přidání Runtime element WebRole nebo WorkerRole prvek ke spuštění se zvýšenými oprávněními:

    ```
    <runtime executioncontext="elevated"/>
    ```
3. Uložte soubor.
4. Otevřete soubor diagnostice (diagnostics.wadcfg SDK 2.4 a pod nebo diagnostics.wadcfgx v SDK 2,5 a výše) a přidejte do následující DiagnosticMonitorConfiguration 

    ```
    <PerformanceCounters bufferQuotaInMB="0" scheduledTransferPeriod="PT30M">
     <PerformanceCounterConfiguration counterSpecifier="\MyCustomCounterCategory\MyButton1Counter" sampleRate="PT30S"/>
    </PerformanceCounters>
    ```
5. Uložte soubor.
6. Vytvoření kategorie čítače vlastní výkonu v metodu při spuštění vaše role před vyvoláním základu. Při spuštění. Následující příklad C# vytvoří vlastní kategorie, pokud už neexistuje:

    ```
    public override bool OnStart()
    {
    if (!PerformanceCounterCategory.Exists("MyCustomCounterCategory"))
    {
       CounterCreationDataCollection counterCollection = new CounterCreationDataCollection();

       // add a counter tracking user button1 clicks
       CounterCreationData operationTotal1 = new CounterCreationData();
       operationTotal1.CounterName = "MyButton1Counter";
       operationTotal1.CounterHelp = "My Custom Counter for Button1";
       operationTotal1.CounterType = PerformanceCounterType.NumberOfItems32;
       counterCollection.Add(operationTotal1);

       PerformanceCounterCategory.Create(
         "MyCustomCounterCategory",
         "My Custom Counter Category",
         PerformanceCounterCategoryType.SingleInstance, counterCollection);

       Trace.WriteLine("Custom counter category created.");
    }
    else{
       Trace.WriteLine("Custom counter category already exists.");
    }

    return base.OnStart();
    }
    ```
7. Aktualizujte čítače v rámci aplikace. Následující příklad aktualizuje vlastní čítače o událostech Button1_Click:

    ```
    protected void Button1_Click(object sender, EventArgs e)
        {
         PerformanceCounter button1Counter = new PerformanceCounter(
           "MyCustomCounterCategory",
           "MyButton1Counter",
           string.Empty,
           false);
         button1Counter.Increment();
        this.Button1.Text = "Button 1 count: " +
           button1Counter.RawValue.ToString();
        }
    ```
8. Uložte soubor.  

Vlastní data čítačů budou shromážděny teď Azure diagnostiky monitor.

## <a name="step-3-query-performance-counter-data"></a>Krok 3: Data čítačů dotazu

Po nasazení aplikace a spuštění sledování diagnostiky začne se shromažďováním výkonnosti a uchování dat, které budou Azure úložiště. Pomocí nástrojů, jako jsou Průzkumníkovi serveru ve Visual Studiu, [Průzkumníka úložišť Azure](http://azurestorageexplorer.codeplex.com/)nebo [Správce diagnostiky Azure](http://www.cerebrata.com/Products/AzureDiagnosticsManager/Default.aspx) tak, že Cerebrata zobrazit údaje o výkonu čítače v tabulce WADPerformanceCountersTable. Můžete také programově dotaz tabulku služba pomocí [C#](../storage/storage-dotnet-how-to-use-tables.d), [Java](../storage/storage-java-how-to-use-table-storage.md), [Node.js](../storage/storage-nodejs-how-to-use-table-storage.md), [Python](../storage/storage-python-how-to-use-table-storage.md), [skutečné](../storage/storage-ruby-how-to-use-table-storage.md)nebo [PHP](../storage/storage-php-how-to-use-table-storage.md).

Následující příklad C# ukazuje jednoduchý dotaz na tabulce WADPerformanceCountersTable a Diagnostika data se uloží do souboru CSV. Jakmile výkonnosti se uloží do souboru CSV můžete grafovým funkce v aplikaci Microsoft Excel nebo jiný nástroj vizualizovat data. Ujistěte se, pokud chcete přidat odkaz na Microsoft.WindowsAzure.Storage.dll, která je součástí v Azure SDK .NET říjen 2012 a novějších. Sestavení se nainstaluje do adresáře aplikace Files%\Microsoft SDKs\Microsoft Azure.NET SDK\version-num\ref\ %.

```
    using Microsoft.WindowsAzure.Storage;
    using Microsoft.WindowsAzure.Storage.Auth;
    using Microsoft.WindowsAzure.Storage.Table;
    ...

    // Get the connection string. When using Microsoft Azure Cloud Services, it is recommended
    // you store your connection string using the Microsoft Azure service configuration
    // system (*.csdef and *.cscfg files). You can you use the CloudConfigurationManager type
    // to retrieve your storage connection string.  If you're not using Cloud Services, it's
    // recommended that you store the connection string in your web.config or app.config file.
    // Use the ConfigurationManager type to retrieve your storage connection string.

    string connectionString = Microsoft.WindowsAzure.CloudConfigurationManager.GetSetting("StorageConnectionString");
    //string connectionString = System.Configuration.ConfigurationManager.ConnectionStrings["StorageConnectionString"].ConnectionString;

    // Get a reference to the storage account using the connection string.  You can also use the development
    // storage account (Storage Emulator) for local debugging.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(connectionString);
    //CloudStorageAccount storageAccount = CloudStorageAccount.DevelopmentStorageAccount;

    // Create the table client.
    CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

    // Create the CloudTable object that represents the "WADPerformanceCountersTable" table.
    CloudTable table = tableClient.GetTableReference("WADPerformanceCountersTable");

    // Create the table query, filter on a specific CounterName, DeploymentId and RoleInstance.
    TableQuery<PerformanceCountersEntity> query = new TableQuery<PerformanceCountersEntity>()
       .Where(
          TableQuery.CombineFilters(
          TableQuery.GenerateFilterCondition("CounterName", QueryComparisons.Equal, @"\Processor(_Total)\% Processor Time"),
          TableOperators.And,
          TableQuery.CombineFilters(
          TableQuery.GenerateFilterCondition("DeploymentId", QueryComparisons.Equal, "ec26b7a1720447e1bcdeefc41c4892a3"),
          TableOperators.And,
          TableQuery.GenerateFilterCondition("RoleInstance", QueryComparisons.Equal, "WebRole1_IN_0")
          )
       )
    );

    // Execute the table query.
    IEnumerable<PerformanceCountersEntity> result = table.ExecuteQuery(query);

    // Process the query results and build a CSV file.
    StringBuilder sb = new StringBuilder("TimeStamp,EventTickCount,DeploymentId,Role,RoleInstance,CounterName,CounterValue\n");

    foreach (PerformanceCountersEntity entity in result)
    {
       sb.Append(entity.Timestamp + "," + entity.EventTickCount + "," + entity.DeploymentId + ","
          + entity.Role + "," + entity.RoleInstance + "," + entity.CounterName + "," + entity.CounterValue+"\n");
    }

    StreamWriter sw = File.CreateText(@"C:\temp\PerfCounters.csv");
    sw.Write(sb.ToString());
    sw.Close();
```

Entity mapování a C# objektů pomocí vlastní třídy odvozeno z **TableEntity**. Následující kód definuje entity třídy, která představuje čítače v tabulce **WADPerformanceCountersTable** .


    public class PerformanceCountersEntity : TableEntity
    {
       public long EventTickCount { get; set; }
       public string DeploymentId { get; set; }
       public string Role { get; set; }
       public string RoleInstance { get; set; }
       public string CounterName { get; set; }
       public double CounterValue { get; set; }
    }


## <a name="next-steps"></a>Další kroky
[Zobrazit další články o Azure diagnostice] (.. / azure diagnostics.md)
