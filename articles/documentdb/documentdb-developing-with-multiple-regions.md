<properties
   pageTitle="Vývoj s více oblastí v DocumentDB | Microsoft Azure"
   description="Zjistěte, jak přistupovat k datům ve více oblastech z Azure DocumentDB plně spravované služby NoSQL databáze."
   services="documentdb"
   documentationCenter=""
   authors="kiratp"
   manager="jhubbard"
   editor=""/>

<tags
   ms.service="documentdb"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/25/2016"
   ms.author="kipandya"/>
   
# <a name="developing-with-multi-region-documentdb-accounts"></a>Vývoj s více oblastí DocumentDB účty

> [AZURE.NOTE] Globální distribuční DocumentDB databází je všeobecně dostupná a automaticky povoleno pro všechny nově vytvořený DocumentDB účty. Pracujeme na povolení globálního distribuce pro všechny existující účty, ale mezitím můžete podle potřeby globální distribuční zapnuta účtu, [Kontaktujte podporu](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) a jsme budete povolte ho za vás teď.

Aby šlo využít [globální distribuční](documentdb-distribute-data-globally.md), můžete určit klientské aplikace uspořádaných předvoleb seznam oblastí sloužit k provádění operací dokumentu. Lze provést pomocí nastavení zásad připojení. Na základě konfigurace účtu Azure DocumentDB, aktuální místní dostupnost a seznam předvoleb zadali, zvolí nejčastěji optimální koncový bod SDK k provádění zapsat a další operace. 

Tento seznam předvoleb není zadán spuštění připojení s protokolem klientovi DocumentDB SDK. SDK přijmout volitelný parametr "PreferredLocations" to znamená uspořádaných seznamů Azure oblastí.

V SDK vám automaticky pošle že všechny zápisy aktuální psaní oblast. 

Všechny operace čtení odešle první oblasti k dispozici v seznamu PreferredLocations. Pokud žádost nezdaří, klient se nezdařilo v seznamu dolů k oblasti Další atd. 

Klient, kterého SDK pouze pokusí přečíst z oblasti v souladu s PreferredLocations. Ano například pokud účet databáze je k dispozici ve třech oblastech, ale jenom klientem dvou oblastí a zápis pro PreferredLocations, pak žádné čtení se být podávané množství pryč z oblasti zápisu i v případě převzetí.

Aplikace můžete ověřit aktuální koncový bod zapsat a číst koncový bod zvolí SDK tak, že zaškrtnete dvě vlastnosti WriteEndpoint a ReadEndpoint, k dispozici ve verzi SDK 1.8 a nad. 

Pokud PreferredLocations vlastnost není nastavená, všechny žádosti o zobrazovaná aktuální oblasti zápisu. 


## <a name="net-sdk"></a>.NET SDK
V SDK lze použít beze změn kód. V tomto případě SDK automaticky přesměruje obou operace čtení a zapíše do aktuální oblasti zápisu. 

Ve verzi 1,8 a novější .NET SDK má parametr ConnectionPolicy konstruktoru DocumentClient vlastnost s názvem Microsoft.Azure.Documents.ConnectionPolicy.PreferredLocations. Tato vlastnost je typu kolekce `<string>` a by měl obsahovat seznam jmen oblast. Řetězcové hodnoty jsou formátovány ve sloupci Název oblasti [Azure oblastí]  [ regions] stránku bez mezer před nebo za první a poslední znak v tomto pořadí.

Aktuální zapsat a čtení koncové body jsou k dispozici v DocumentClient.WriteEndpoint a DocumentClient.ReadEndpoint.

> [AZURE.NOTE] Adresy URL pro koncové body neměli považovány za dlouhodobé konstanty. Služba tyto kdykoli aktualizovat. V SDK zpracovává této změny automaticky.

    // Getting endpoints from application settings or other configuration location
    Uri accountEndPoint = new Uri(Properties.Settings.Default.GlobalDatabaseUri);
    string accountKey = Properties.Settings.Default.GlobalDatabaseKey;

    //Setting read region selection preference 
    connectionPolicy.PreferredLocations.Add(LocationNames.WestUS); // first preference
    connectionPolicy.PreferredLocations.Add(LocationNames.EastUS); // second preference
    connectionPolicy.PreferredLocations.Add(LocationNames.NorthEurope); // third preference

    // initialize connection
    DocumentClient docClient = new DocumentClient(
        accountEndPoint,
        accountKey,
        connectionPolicy);

    // connect to DocDB 
    await docClient.OpenAsync().ConfigureAwait(false);


## <a name="nodejs-javascript-and-python-sdks"></a>NodeJS, JavaScript a Python SDK
V SDK lze použít beze změn kód. V tomto případě SDK bude automaticky směrovat čtení a zápis na aktuální psaní oblast. 

Ve verzi 1,8 a novější každý SDK ConnectionPolicy parametr konstruktoru DocumentClient nové vlastnosti s názvem DocumentClient.ConnectionPolicy.PreferredLocations. To je parametr pole řetězců, kterým seznam jmen oblast. Názvy jsou formátovány ve sloupci Název oblasti v [Oblasti Azure]  [ regions] stránky. Taky můžete použít předdefinované konstanty ve objekt pohodlí AzureDocuments.Regions

Aktuální zapsat a čtení koncové body jsou k dispozici v DocumentClient.getWriteEndpoint a DocumentClient.getReadEndpoint.

> [AZURE.NOTE] Adresy URL pro koncové body neměli považovány za dlouhodobé konstanty. Služba tyto kdykoli aktualizovat. V SDK bude automaticky zpracovávat tuto změnu.

Tady je příklad NodeJS/JavaScript. Python a Java následovat stejné vzorku.

    // Creating a ConnectionPolicy object
    var connectionPolicy = new DocumentBase.ConnectionPolicy();
    
    // Setting read region selection preference, in the following order -
    // 1 - West US
    // 2 - East US
    // 3 - North Europe
    connectionPolicy.PreferredLocations = ['West US', 'East US', 'North Europe'];
    
    // initialize the connection
    var client = new DocumentDBClient(host, { masterKey: masterKey }, connectionPolicy);


## <a name="rest"></a>ZBÝVAJÍCÍ 
Jakmile účet databáze dojde k dispozici ve více oblastech, klienty dotaz jeho dostupnost provedením požadavek GET na následující URI.

    https://{databaseaccount}.documents.azure.com/

Služba vrátí seznam oblastí a jejich odpovídající DocumentDB koncový bod URI pro replik. Aktuální oblast zápisu bude uveden v odpovědi. Klient vyberte odpovídající koncový bod pro všechny další žádosti o rozhraní REST API následujícím způsobem.

Příklad odpověď

    {
        "_dbs": "//dbs/",
        "media": "//media/",
        "writableLocations": [
            {
                "Name": "West US",
                "DatabaseAccountEndpoint": "https://globaldbexample-westus.documents.azure.com:443/"
            }
        ],
        "readableLocations": [
            {
                "Name": "East US",
                "DatabaseAccountEndpoint": "https://globaldbexample-eastus.documents.azure.com:443/"
            }
        ],
        "MaxMediaStorageUsageInMB": 2048,
        "MediaStorageUsageInMB": 0,
        "ConsistencyPolicy": {
            "defaultConsistencyLevel": "Session",
            "maxStalenessPrefix": 100,
            "maxIntervalInSeconds": 5
        },
        "addresses": "//addresses/",
        "id": "globaldbexample",
        "_rid": "globaldbexample.documents.azure.com",
        "_self": "",
        "_ts": 0,
        "_etag": null
    }


-   Požadavky na umístění, publikovat a odstranit musíte přejít na určený zápisu URI
-   Všechny získá a jiných jen pro čtení žádosti o (například dotazy) může přejděte na libovolnou koncový bod služby volba klienta

Psaní, požadavky jen pro čtení oblasti nezdaří s kódem chyby HTTP 403 ("zakázáno").

Pokud oblast zápisu změní po klienta zjišťování počáteční fáze, následné zápisy na předchozí oblast zápisu se nepovede s kódem chyby HTTP 403 ("zakázáno"). Klient by měla získat potom seznam oblastí znovu zobrazíte oblasti aktualizované zápisu.

## <a name="next-steps"></a>Další kroky

Další informace o distribuce data globálně s DocumentDB v těchto článcích:

- [Distribuovat data globálně s DocumentDB](documentdb-distribute-data-globally.md)
- [Soulad úrovně](documentdb-consistency-levels.md)
- [Jak výkon spolupracuje více oblastí](documentdb-manage.md#how-throughput-works-with-multiple-regions)
- [Přidání oblastí pomocí portálu Azure](documentdb-portal-global-replication.md)

[regions]: https://azure.microsoft.com/regions/ 
