<properties
    pageTitle="Povolení metriky úložišť na portálu Azure | Microsoft Azure"
    description="Jak povolit metriky úložišť služby objektů Blob, fronty, tabulky a souborů"
    services="storage"
    documentationCenter=""
    authors="robinsh"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="08/03/2016"
    ms.author="robinsh"/>

# <a name="enabling-azure-storage-metrics-and-viewing-metrics-data"></a>Povolení metriky úložišť Azure a zobrazení dat metriky

[AZURE.INCLUDE [storage-selector-portal-enable-and-view-metrics](../../includes/storage-selector-portal-enable-and-view-metrics.md)]

## <a name="overview"></a>Základní informace

Ve výchozím nastavení není povolený metriky úložišť úložiště služby. Můžete povolit sledování prostřednictvím [Portálu Azure](https://portal.azure.com) nebo prostředí Windows PowerShell nebo programově prostřednictvím knihovně úložiště klienta.

Když povolíte metriky úložišť, musíte vybrat období uchování dat: Toto období určuje, jak dlouho úložištěm služba udržuje metriky a poplatky místa nemusíte ukládány. Obvykle byste měli použít kratší období uchování pro minute metriky než hodinové metriky z důvodu významné mezery potřebných pro minute metriky. Měli byste období uchování tak, že máte dostatek času pro analýzu dat a stažení metriky, které chcete zachovat pro vytváření sestav účely offline analýzy nebo. Myslete na to, že se bude taky vám nebudou účtovat poplatky pro stažení metriky dat ze svého účtu úložiště.

## <a name="how-to-enable-metrics-using-the-azure-portal"></a>Jak povolit metriky pomocí portálu Azure

Tímto postupem povolíte metriky [Azure portálu](https://portal.azure.com):

1. Přejděte ke svému účtu úložiště.
1. Otevřete zásuvné **Nastavení** a vyberte **diagnostických nástrojů**.
1. Ujistěte se, že je nastaven **Stav** **na**.
1. Vyberte metriky pro služby, které chcete sledovat.
2. Zadejte zásady uchovávání informací označíte, jak dlouho Pokud chcete zachovat metriky a protokolování údajů.

Všimněte si, že na [Portálu Azure](https://portal.azure.com) neumožňuje aktuálně konfigurace minute metriky ve vašem účtu úložiště; je třeba povolit minute metriky pomocí prostředí PowerShell nebo programově.

## <a name="how-to-enable-metrics-using-powershell"></a>Jak povolit metriky pomocí prostředí PowerShell

Pomocí prostředí PowerShell na místním počítači můžete konfigurovat metriky úložišť ve vašem účtu úložiště pomocí prostředí PowerShell Azure rutiny Get-AzureStorageServiceMetricsProperty k načtení aktuální nastavení a rutinu Set-AzureStorageServiceMetricsProperty měnit aktuální nastavení.

Rutiny pro, kterými se řídí metriky úložišť pomocí následujících parametrů:

- MetricsType: možné hodnoty jsou hodiny a minuty.

- Typ_služby: možné hodnoty jsou objektů Blob fronty a tabulky.

- MetricsLevel: možné hodnoty jsou žádná přihlašovacích údajů a ServiceAndApi.

Například následující příkaz kombinace kláves vymění minute metrice pro službu objektů Blob ve vašem účtu úložiště výchozí s období nastavit až pět dnů uchovávání informací:

`Set-AzureStorageServiceMetricsProperty -MetricsType Minute -ServiceType Blob -MetricsLevel ServiceAndApi  -RetentionDays 5`

Následující příkaz načte aktuální hodinové metriky úroveň a uchovávání dnů pro službu objektů blob ve vašem účtu úložiště výchozí:

`Get-AzureStorageServiceMetricsProperty -MetricsType Hour -ServiceType Blob`

Informace o tom, jak konfigurovat rutiny prostředí PowerShell Azure pro práci s Azure předplatné a postup při výběru účet výchozí úložiště, který chcete použít, najdete v tématu: [Postup instalace a konfigurace prostředí PowerShell Azure](../powershell-install-configure.md).

## <a name="how-to-enable-storage-metrics-programmatically"></a>Jak povolit metriky úložišť programově

Následující úryvek C# ukazuje, jak povolit protokolování pro službu objektů Blob pomocí knihovně úložiště klienta pro .NET a metriky:

    //Parse the connection string for the storage account.
    const string ConnectionString = "DefaultEndpointsProtocol=https;AccountName=account-name;AccountKey=account-key";
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(ConnectionString);

    // Create service client for credentialed access to the Blob service.
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

    // Enable Storage Analytics logging and set retention policy to 10 days.
    ServiceProperties properties = new ServiceProperties();
    properties.Logging.LoggingOperations = LoggingOperations.All;
    properties.Logging.RetentionDays = 10;
    properties.Logging.Version = "1.0";

    // Configure service properties for metrics. Both metrics and logging must be set at the same time.
    properties.HourMetrics.MetricsLevel = MetricsLevel.ServiceAndApi;
    properties.HourMetrics.RetentionDays = 10;
    properties.HourMetrics.Version = "1.0";

    properties.MinuteMetrics.MetricsLevel = MetricsLevel.ServiceAndApi;
    properties.MinuteMetrics.RetentionDays = 10;
    properties.MinuteMetrics.Version = "1.0";

    // Set the default service version to be used for anonymous requests.
    properties.DefaultServiceVersion = "2015-04-05";

    // Set the service properties.
    blobClient.SetServiceProperties(properties);


## <a name="viewing-storage-metrics"></a>Zobrazení metriky úložišť

Po konfiguraci metriky úložišť analýzy sledování účtu úložiště úložiště analýzy záznamů metriky v set známý tabulek ve vašem účtu úložiště. Můžete nakonfigurovat grafy zobrazíte hodinové metriky [Azure portálu](https://portal.azure.com):

1. Přejděte ke svému účtu úložiště na [Portál Azure](https://portal.azure.com).
2. V části **Sledování** klikněte na **Přidat dlaždice** přidat nový graf. V **Galerii dlaždic**vyberte míru, které si přejete zobrazit a přetáhněte ho do části **Sledování** .
3. Pokud chcete upravit, které metriky zobrazených v grafu, klikněte na odkaz pro **Úpravy** . Můžete přidat nebo odebrat jednotlivé metriky zaškrtnutím nebo zrušením jejich výběru.
4. Po dokončení úprav metriky, klikněte na **Uložit** .

Pokud chcete ke stažení metriky služby dlouhodobě úložiště nebo k analýze místně, musíte:

- Použijte nástroj, který známa v této tabulce a bude možné prohlížet a stáhněte si je.
- Napište vlastní aplikaci nebo skriptu pro čtení a uložení tabulky.

Mnoho třetích stran procházení úložiště nástrojů se v této tabulce a umožňuje vám tyto informace zobrazit přímo.
Seznam dostupných nástrojů najdete v článku [Průzkumník úložiště Azure](storage-explorers.md) .

> [AZURE.NOTE] Od verze 0.8.0 [Microsoft Azure úložiště Průzkumníka] (http://storageexplorer.com/), můžete teď budou moct prohlížet ani stáhnout tabulek metriky analýzy.

Při přístupu k analýzy tabulky programově, Všimněte si, že tabulky analýzy nezobrazí seznam všech tabulek ve vašem účtu úložiště. Můžete se k nim přímo podle názvu nebo pomocí rozhraní [CloudAnalyticsClient API](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.analytics.cloudanalyticsclient.aspx) v knihovně klienta .NET k vytvoření dotazu názvy tabulek.

### <a name="hourly-metrics"></a>Každou hodinu metriky
- $MetricsHourPrimaryTransactionsBlob
- $MetricsHourPrimaryTransactionsTable
- $MetricsHourPrimaryTransactionsQueue

### <a name="minute-metrics"></a>Minuta metriky
- $MetricsMinutePrimaryTransactionsBlob
- $MetricsMinutePrimaryTransactionsTable
- $MetricsMinutePrimaryTransactionsQueue

### <a name="capacity"></a>Kapacity
- $MetricsCapacityBlob

O v této tabulce na [Úložiště analýzy metriky tabulky schématu](https://msdn.microsoft.com/library/azure/hh343264.aspx)najdete úplné podrobnosti schémat. Ukázka řádky pod zobrazit jen podmnožinu sloupce dostupné, ale ilustrují některé důležité funkce způsob, jakým metriky úložišť uloží tyto metriky:

| PartitionKey  |       RowKey       |                    Časové razítko | TotalRequests | TotalBillableRequests | TotalIngress | TotalEgress | Dostupnost | AverageE2ELatency | AverageServerLatency | PercentSuccess |
|---------------|:------------------:|-----------------------------:|---------------|-----------------------|--------------|-------------|--------------|-------------------|----------------------|----------------|
| 20140522T1100 |      uživatele. Všechny      | 2014-05-22T11:01:16.7650250Z | 7             | 7                     | 4003         | 46801       | 100          | 104.4286          | 6.857143             | 100            |
| 20140522T1100 | uživatele. QueryEntities | 2014-05-22T11:01:16.7640250Z | 5             | 5                     | 2694         | 45951       | 100          | 143.8             | 7,8                  | 100            |
| 20140522T1100 |  uživatele. QueryEntity  | 2014-05-22T11:01:16.7650250Z | 1             | 1                     | 538          | 633         | 100          | 3                 | 3                    | 100            |
| 20140522T1100 | uživatele. UpdateEntity  | 2014-05-22T11:01:16.7650250Z | 1             | 1                     | 771          | 217         | 100          | 9                 | 6                    | 100               |

V těchto datech minute metriky příklad klíč oddílu používá čas minute rozlišením. Klíč řádku označuje typ informací uložených v řádku, a to se skládá ze dvou částí informací, typ přístupu a typ požadavku:

- Typ přístupu způsobem řídit uživatel ani systému, kde uživatele odkazují na všechny uživatele žádosti o službu úložiště, a systému do žádosti úložiště analýzy.

- Typ požadavku je ve kterém případě se je souhrnný řádek nebo identifikuje konkrétní rozhraní API QueryEntity ATP UpdateEntity.


Ukázková data nad zobrazí všechny záznamy pro jednu minutu (počínaje 11:00 dop.), tak počet QueryEntities požadavků plus počet QueryEntity žádosti plus počet UpdateEntity požadavky přidat až sedm, které představuje součet zobrazené na řádku uživatele: všechny. Podobně můžete výpočtem odvodit průměrnou latenci začátku do konce 104.4286 na řádku uživatele: všechny ((143.8 * 5) + 3 + 9) / 7.

Nastavení upozornění v [Portálu Azure](https://portal.azure.com) na stránce Sledování byste měli zvážit tak, aby metriky úložišť můžete automaticky vás upozorní na všechny důležité změny v chování úložiště služby. Pokud používáte nástroj explorer úložiště ke stažení této metriky dat ve formátu s oddělovači, můžete k analýze dat aplikace Microsoft Excel. Najdete v blogovém příspěvku [Microsoft Azure úložiště Průzkumník](http://blogs.msdn.com/b/windowsazurestorage/archive/2014/03/11/windows-azure-storage-explorers-2014.aspx) seznam volného explorer nástroje.



## <a name="accessing-metrics-data-programmatically"></a>Přístup k datům metriky programově

Následující seznam zobrazuje ukázka C# kód, který přistupuje minute metriky pro oblast minut a zobrazí výsledky v konzole okna. Použije knihovně úložiště Azure verzi 4, která obsahuje CloudAnalyticsClient předmětu, který usnadňuje přístup k metriky tabulek v úložišti.

    private static void PrintMinuteMetrics(CloudAnalyticsClient analyticsClient, DateTimeOffset startDateTime, DateTimeOffset endDateTime)
    {
    // Convert the dates to the format used in the PartitionKey
    var start = startDateTime.ToUniversalTime().ToString("yyyyMMdd'T'HHmm");
    var end = endDateTime.ToUniversalTime().ToString("yyyyMMdd'T'HHmm");

    var services = Enum.GetValues(typeof(StorageService));
    foreach (StorageService service in services)
    {
    Console.WriteLine("Minute Metrics for Service {0} from {1} to {2} UTC", service, start, end);
    var metricsQuery = analyticsClient.CreateMinuteMetricsQuery(service, StorageLocation.Primary);
    var t = analyticsClient.GetMinuteMetricsTable(service);
    var opContext = new OperationContext();
    var query =
    from entity in metricsQuery
    // Note, you can't filter using the entity properties Time, AccessType, or TransactionType
    // because they are calculated fields in the MetricsEntity class.
    // The PartitionKey identifies the DataTime of the metrics.
    where entity.PartitionKey.CompareTo(start) >= 0 && entity.PartitionKey.CompareTo(end) <= 0
    select entity;

    // Filter on "user" transactions after fetching the metrics from Table Storage.
    // (StartsWith is not supported using LINQ with Azure table storage)
    var results = query.ToList().Where(m => m.RowKey.StartsWith("user"));
    var resultString = results.Aggregate(new StringBuilder(), (builder, metrics) => builder.AppendLine(MetricsString(metrics, opContext))).ToString();
    Console.WriteLine(resultString);
    }
    }

    private static string MetricsString(MetricsEntity entity, OperationContext opContext)
    {
    var entityProperties = entity.WriteEntity(opContext);
    var entityString =
    string.Format("Time: {0}, ", entity.Time) +
    string.Format("AccessType: {0}, ", entity.AccessType) +
    string.Format("TransactionType: {0}, ", entity.TransactionType) +
    string.Join(",", entityProperties.Select(e => new KeyValuePair<string, string>(e.Key.ToString(), e.Value.PropertyAsObject.ToString())));
    return entityString;

    }




## <a name="what-charges-do-you-incur-when-you-enable-storage-metrics"></a>K čemu poplatky nimž dochází při povolíte metriky úložišť?

Zápis požadavky na vytvoření tabulky entity metriky bude účtovaná za standardní sazby pro všechny operace Azure úložiště.

Požadavky pro čtení a odstranit klientem metriky dat jsou také fakturaci standardní sazbou. Pokud jste nakonfigurovali zásady uchovávání informací, nejsou Účtovaná při úložišti Azure odstraní původní data metriky. Pokud byste odstranili technologie pro analýzu dat, váš účet platit pro operace odstranění.

Kapacita používaný metriky tabulky je také fakturaci: následující můžete použít k odhadu kapacitu slouží k uložení metriky dat:

- Pokud každou hodinu služba využívá každé rozhraní API v každé služby, pak přibližně 148 znalostní bázi Knowledge Base dat uložen každou hodinu v tabulkách transakce metriky Pokud zapnete službu a rozhraní API úroveň souhrnné.

- Pokud každou hodinu služba využívá každé rozhraní API v každé služby, pak přibližně 12 znalostní bázi Knowledge Base dat uložen každou hodinu v tabulkách transakce metriky Pokud jste zapnuli automatický přístup jenom úroveň služeb souhrnné.

- Tabulka kapacita pro objekty BLOB obsahuje dva řádky přidané každý den (za předpokladu, že uživatel zvolil v protokoly): to znamená, že každý den velikost v této tabulce zvyšuje přibližně 300 bajtů.

## <a name="next-steps"></a>Další kroky:
[Povolení úložiště protokolování a přístup k datům protokolu](https://msdn.microsoft.com/library/dn782840.aspx)
