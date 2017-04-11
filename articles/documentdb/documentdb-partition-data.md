<properties 
    pageTitle="Rozdělení a stejné měřítko v Azure DocumentDB | Microsoft Azure"      
    description="Zjistěte, jak rozdělení funguje v Azure DocumentDB, jak nakonfigurovat rozdělení a rozdělit klíče a jak vybrat klávesu Pravý oddíl aplikace."         
    services="documentdb" 
    authors="arramac" 
    manager="jhubbard" 
    editor="monicar" 
    documentationCenter=""/> 

<tags 
    ms.service="documentdb" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/20/2016" 
    ms.author="arramac"/> 

# <a name="partitioning-and-scaling-in-azure-documentdb"></a>Rozdělení a stejné měřítko v Azure DocumentDB
[Microsoft Azure DocumentDB](https://azure.microsoft.com/services/documentdb/) usnadňující dosáhnout rychlé a předvídatelná výkonu a měřítko Bezproblémová spolu s aplikace narůstající velikostí ho. Tento článek obsahuje přehled způsob rozdělení funguje v DocumentDB a popisuje, jak můžete nakonfigurovat DocumentDB kolekce účinně zobrazit aplikace.

Po přečtení v tomto článku, budou moct odpovězte na následující otázky:   

- Jak funguje rozdělení práce v Azure DocumentDB?
- Jak nakonfigurovat, oddílů v DocumentDB
- Co jsou klíči oddílů a jak vyberte klávesu Pravý oddíl pro aplikaci?

Začít s kódem, stáhněte si prosím projektu z [DocumentDB výkonu testování ovladač vzorku](https://github.com/Azure/azure-documentdb-dotnet/tree/a2d61ddb53f8ab2a23d3ce323c77afcf5a608f52/samples/documentdb-benchmark). 

## <a name="partitioning-in-documentdb"></a>Rozdělení v DocumentDB

V DocumentDB můžete uložit a dotaz schématu bez JSON dokumenty s doby odezvy pořadí milisekund ve všech velkém měřítku. DocumentDB obsahuje kontejnery ukládání dat s názvem **kolekcí**. Kolekce je logické zdrojů a může zahrnovat fyzické oddíly nebo servery. Počet oddílů, je určený podle DocumentDB na základě velikosti úložiště a zřizování výkon kolekce. Každý oddíl DocumentDB má pevný počet záložní SSD úložiště přidružená a replikovat vysoké dostupnosti. Oddíl Správa plně spravuje Azure DocumentDB a nemáte složité kódu nebo spravovat oddílů. Kolekce DocumentDB jsou **prakticky neomezený** z hlediska úložiště a výkon. 

Rozdělení je úplně průhledný aplikaci. DocumentDB podporuje rychlé čtení a zápisy, dotazy SQL a LINQ, transakční logiky JavaScriptu na základě, konzistence úrovně a řízení přístupu jemně odstupňovaná prostřednictvím rozhraní REST API hovory jednu kolekci zdroji. Služba zpracovává distribuce dat mezi oddíly a směrování dotazu požadavků na pravý oddíl. 

Jak to funguje? Když vytvoříte kolekci v DocumentDB, zjistíte, že je hodnota konfigurace **oddíl klíčové vlastnost** , která můžete zadat. Toto je vlastnost JSON (nebo cesty) do dokumentů, které můžete použít tak, že DocumentDB k distribuci dat mezi více servery nebo oddíly. DocumentDB bude hash hodnoty klíče oddíl a hash výsledků určete oddílu, ve kterém bude JSON dokument uložen. Všechny dokumenty se stejným klíčem oddíl uloží do stejného oddílu. 

Zvažte například aplikaci, která obsahuje data o zaměstnancích a jejich oddělení v DocumentDB. Pojďme zvolte `"department"` jako oddíl vlastnost klíče, abyste mohli rozšiřování dat podle oddělení. Každý dokument v DocumentDB musí obsahovat povinné `"id"` vlastnost, která musí být jedinečné pro každý dokument se stejnou hodnotou oddíl klíče, například `"Marketing`". Každý dokument uložený v kolekci musí být jedinečný kombinací oddíl spolu s id, například `{ "Department": "Marketing", "id": "0001" }`, `{ "Department": "Marketing", "id": "0002" }`, a `{ "Department": "Sales", "id": "0001" }`. Jinými slovy, složeného vlastnost (klíč, id oddílu) je primární klíč pro vaši kolekci.

### <a name="partition-keys"></a>Oddíl klíče
Volba klíč oddílu je důležité rozhodnutí, který budete muset vytvořit v době návrhu. Je třeba vybrat název vlastnosti JSON, která má širokou škálu hodnot a bude pravděpodobně rovnoměrně vzorků přístup. Klíč oddílu je zadaný jako JSON cesty, například `/department` představuje oddělení vlastnost. 

Následující tabulka zobrazuje příklady oddíl klíčové definice a odpovídající každé hodnoty JSON.

<table border="0" cellspacing="0" cellpadding="0">
    <tbody>
        <tr>
            <td valign="top"><p><strong>Cesta ke klíči oddíl</strong></p></td>
            <td valign="top"><p><strong>Popis</strong></p></td>
        </tr>
        <tr>
            <td valign="top"><p>/Department</p></td>
            <td valign="top"><p>Odpovídá JSON přínosu doc.department kde dokumentu je váš dokument.</p></td>
        </tr>
        <tr>
            <td valign="top"><p>/ vlastnosti nebo název</p></td>
            <td valign="top"><p>Odpovídá JSON přínosu doc.properties.name kde dokumentu je váš dokument (Vnořená vlastnost).</p></td>
        </tr>
        <tr>
            <td valign="top"><p>/ID</p></td>
            <td valign="top"><p>Odpovídá JSON přínosu doc.id (id a oddílu klíč jsou stejná vlastnost).</p></td>
        </tr>
        <tr>
            <td valign="top"><p>/ "název oddělení"</p></td>
            <td valign="top"><p>Odpovídá JSON přínosu dokumentu ["název oddělení"], kde je dokument v dokumentu.</p></td>
        </tr>
    </tbody>
</table>

> [AZURE.NOTE] Syntaxe cesta ke klíči oddíl je podobný specifikace cesty pro indexování zásad cesty klíčové rozdíl cestu odpovídající vlastnost místo hodnotu, tedy je žádné zástupné konci. Třeba byste měli zadat/oddělení /? indexování hodnoty v části oddělení, ale /department zadejte jako definici klíče oddíl. Cesta ke klíči oddíl je implicitně indexovaných a nelze vyloučit z indexování pomocí indexování zásad přepsání.

Podívejme se na jak volba klíč oddílu ovlivňuje výkon aplikace.

### <a name="partitioning-and-provisioned-throughput"></a>Rozdělení a zřizování výkon
DocumentDB je určený pro předvídatelná výkonu. Při vytváření kolekce rezervovat výkon z hlediska ** [žádosti o jednotky](documentdb-request-units.md) (RU) na druhé**. Každý požadavek se přiřadí nákladů žádost o jednotku, která je poměrné počtu systémové prostředky jako procesoru a vstupu a výstupu využívané operaci. Čtení 1 kB dokumentu s konci relace konzistence spotřebovává 1 žádost o jednotku. Čtení, která jsou 1 RU bez ohledu na počet položek nebo počet souběžně prováděné žádosti o spuštění současně. Větší dokumenty vyžadovat vyšší jednotky žádost v závislosti na velikosti. Pokud znáte velikost entity a počet čtení, které potřebujete pro podporu pro aplikaci, zřizujete přesný počet výkon požadovaných pro aplikace společnosti číst potřeb. 

Když DocumentDB ukládá dokumenty, rozmístěné je rovnoměrně mezi oddíly podle hodnoty klíče oddíl. Výkon je také distribuované rovnoměrně mezi dostupných oddíly, tedy výkon na oddíl = (celkový výkon kolekce) / (počet oddílů). 

>[AZURE.NOTE] Dosáhnout tak celou výkon kolekce, musíte zvolit klíč oddílu, který umožňuje rozmístěte požadavky mezi počet různých oddíl klíčových hodnot.

## <a name="single-partition-and-partitioned-collections"></a>Jeden oddíl a oddíly kolekcí
DocumentDB podporuje vytváření kolekce jedním oddílem a oddíly. 

- **Partitioned kolekce** může zahrnovat více oddílů a podpora velmi velké množství úložiště a výkon. Je nutné zadat klíč oddílu kolekce.
- **Kolekci jedním oddílem** může být nižší možnosti cena a možnost dotazů a zpracování transakce přes všechna data kolekce. Mají limity úložiště a škálovatelnost: jeden oddíl. Není nutné zadávat oddíl klíč pro tyto kolekce. 

![Rozdělený kolekcí v DocumentDB][2] 

Scénáře, které není nutné velkých objemů úložiště nebo výkon kolekce jeden oddíl se přesně hodí. Všimněte si, že jedním oddílem kolekci může být škálovatelnost a úložného prostoru jeden oddíl, tedy až 10 GB úložiště a až 10 000 jednotek požadavek na druhé. 

Rozdělený kolekce podporují velmi velké množství úložiště a výkon. Výchozí nabídky však není nakonfigurován ukládat až 250 GB úložiště a změna velikosti až 250 000 jednotek požadavek na druhé. V případě potřeby vyšší úložiště nebo výkon kolekce kontaktujte prosím [Podporu Azure](documentdb-increase-limits.md) mít tyto lepší pro váš účet.

V následující tabulce jsou uvedeny rozdíly v práci s jedním oddílem a oddíly kolekcí:

<table border="0" cellspacing="0" cellpadding="0">
    <tbody>
        <tr>
            <td valign="top"><p></p></td>
            <td valign="top"><p><strong>Kolekce jeden oddíl</strong></p></td>
            <td valign="top"><p><strong>Rozdělený kolekce</strong></p></td>
        </tr>
        <tr>
            <td valign="top"><p>Klíč oddílu</p></td>
            <td valign="top"><p>Žádná</p></td>
            <td valign="top"><p>Povinné</p></td>
        </tr>
        <tr>
            <td valign="top"><p>Primární klíč pro dokument</p></td>
            <td valign="top"><p>"identifikátor"</p></td>
            <td valign="top"><p>Složené klíče &lt;klíč oddílu&gt; a "identifikátor"</p></td>
        </tr>
        <tr>
            <td valign="top"><p>Minimální úložiště</p></td>
            <td valign="top"><p>0 GB</p></td>
            <td valign="top"><p>0 GB</p></td>
        </tr>
        <tr>
            <td valign="top"><p>Maximální velikosti úložiště</p></td>
            <td valign="top"><p>10 GB</p></td>
            <td valign="top"><p>Neomezeno (250 GB ve výchozím nastavení)</p></td>
        </tr>
        <tr>
            <td valign="top"><p>Minimální výkon</p></td>
            <td valign="top"><p>400 jednotky žádost za sekundu</p></td>
            <td valign="top"><p>10 000 jednotek žádost za sekundu</p></td>
        </tr>
        <tr>
            <td valign="top"><p>Maximální výkon</p></td>
            <td valign="top"><p>10 000 jednotek žádosti o za sekundu</p></td>
            <td valign="top"><p>Neomezeno (250 000 žádost jednotky sekundu ve výchozím nastavení)</p></td>
        </tr>
        <tr>
            <td valign="top"><p>Rozhraní API verze</p></td>
            <td valign="top"><p>Všechny</p></td>
            <td valign="top"><p>Rozhraní API 2015-12-16 a novější</p></td>
        </tr>
    </tbody>
</table>

## <a name="working-with-the-sdks"></a>Práce s SDK

Azure DocumentDB přidána podpora pro automatické dělení pomocí [Rozhraní REST API verze 2015-12-16](https://msdn.microsoft.com/library/azure/dn781481.aspx). K vytvoření oddílů kolekce, je třeba stáhnout SDK verze 1.6.0 nebo novější v jednom z podporované platformy SDK (.NET Node.js, Java, Python). 

### <a name="creating-partitioned-collections"></a>Vytvoření oddílů kolekce

V následujícím příkladu je ukázka .NET můžete vytvořit kolekci pro ukládání zařízení telemetrickými daty 20 000 jednotek žádost o sekundu výkon. V SDK nastaví hodnotu OfferThroughput (který zároveň nastaví `x-ms-offer-throughput` žádosti o záhlaví v rozhraní REST API). Tady nastavení `/deviceId` jako klíč oddílu. Volba klíč oddílu se uloží spolu s zbytek kolekce metadat jako název a indexování zásad.

Tento příklad jsme odebrána `deviceId` od nám vědět, že a od jsou velkého počtu zařízení, zápisy může být rovnoměrně mezi oddíly a abychom mohli změnit velikost databáze, kterou chcete jedí rozsáhlé objemy dat a (b) spoustu žádosti o jako načítání nejnovější čtení pro zařízení jsou obor vymezený na jednu deviceId můžete načtená z kontingenčního seznamu jeden oddíl.

    DocumentClient client = new DocumentClient(new Uri(endpoint), authKey);
    await client.CreateDatabaseAsync(new Database { Id = "db" });

    // Collection for device telemetry. Here the JSON property deviceId will be used as the partition key to 
    // spread across partitions. Configured for 10K RU/s throughput and an indexing policy that supports 
    // sorting against any number or string property.
    DocumentCollection myCollection = new DocumentCollection();
    myCollection.Id = "coll";
    myCollection.PartitionKey.Paths.Add("/deviceId");

    await client.CreateDocumentCollectionAsync(
        UriFactory.CreateDatabaseUri("db"),
        myCollection,
        new RequestOptions { OfferThroughput = 20000 });
        

> [AZURE.NOTE] K vytvoření oddílů kolekcí, musíte zadat hodnotu výkon > 10 000 jednotek žádosti o sekundu. Protože výkon je v násobcích 100, je třeba 10,100 nebo vyšší.

Tato metoda umožňuje rozhraní REST API volat DocumentDB a službě bude zřízení počet oddílů podle požadované výkon. Výkon kolekce můžete měnit podle výkon potřebuje vyvíjet. Další informace najdete v článku [Výkon úrovně](documentdb-performance-levels.md) .

### <a name="reading-and-writing-documents"></a>Čtení a zápis dokumentů

Teď Pojďme vložte data do DocumentDB. Tady je ukázka tříd obsahující zařízení pro čtení a volání CreateDocumentAsync k vložení nového zařízení čtení do kolekce.

    public class DeviceReading
    {
        [JsonProperty("id")]
        public string Id;

        [JsonProperty("deviceId")]
        public string DeviceId;

        [JsonConverter(typeof(IsoDateTimeConverter))]
        [JsonProperty("readingTime")]
        public DateTime ReadingTime;

        [JsonProperty("metricType")]
        public string MetricType;

        [JsonProperty("unit")]
        public string Unit;

        [JsonProperty("metricValue")]
        public double MetricValue;
      }

    // Create a document. Here the partition key is extracted as "XMS-0001" based on the collection definition
    await client.CreateDocumentAsync(
        UriFactory.CreateDocumentCollectionUri("db", "coll"),
        new DeviceReading
        {
            Id = "XMS-001-FE24C",
            DeviceId = "XMS-0001",
            MetricType = "Temperature",
            MetricValue = 105.00,
            Unit = "Fahrenheit",
            ReadingTime = DateTime.UtcNow
        });


Pojďme čtení dokumentu oddíl klíče a id, aktualizovat a jako poslední krok, odstraňte ji tak, že klíč oddílu a id. Poznámka: načte obsahovat hodnotu PartitionKey (odpovídající `x-ms-documentdb-partitionkey` žádost o záhlaví v rozhraní REST API).

    // Read document. Needs the partition key and the ID to be specified
    Document result = await client.ReadDocumentAsync(
      UriFactory.CreateDocumentUri("db", "coll", "XMS-001-FE24C"), 
      new RequestOptions { PartitionKey = new PartitionKey("XMS-0001") });

    DeviceReading reading = (DeviceReading)(dynamic)result;

    // Update the document. Partition key is not required, again extracted from the document
    reading.MetricValue = 104;
    reading.ReadingTime = DateTime.UtcNow;

    await client.ReplaceDocumentAsync(
      UriFactory.CreateDocumentUri("db", "coll", "XMS-001-FE24C"), 
      reading);

    // Delete document. Needs partition key
    await client.DeleteDocumentAsync(
      UriFactory.CreateDocumentUri("db", "coll", "XMS-001-FE24C"), 
      new RequestOptions { PartitionKey = new PartitionKey("XMS-0001") });



### <a name="querying-partitioned-collections"></a>Rozdělený kolekcích dotazování

Při zjišťování dat v rámci rozdělený kolekcí DocumentDB automaticky přesměrovává dotaz na oddíly odpovídající hodnoty klíče oddílu zadané ve filtru (pokud jsou k dispozici). Tento dotaz je třeba nasměrovaná jenom oddíl obsahující klávesu oddíl "XMS 0001".

    // Query using partition key
    IQueryable<DeviceReading> query = client.CreateDocumentQuery<DeviceReading>(
        UriFactory.CreateDocumentCollectionUri("db", "coll"))
        .Where(m => m.MetricType == "Temperature" && m.DeviceId == "XMS-0001");

Následující dotaz, které neobsahují filtr na klávesu oddílu (DeviceId) a je fanned pro všechny oddíly, kde je spouštět oproti na oddíl indexu. Poznámku, že budete muset zadat EnableCrossPartitionQuery (`x-ms-documentdb-query-enablecrosspartition` v rozhraní REST API) mít SDK spuštění dotazu na mezi oddíly.

    // Query across partition keys
    IQueryable<DeviceReading> crossPartitionQuery = client.CreateDocumentQuery<DeviceReading>(
        UriFactory.CreateDocumentCollectionUri("db", "coll"), 
        new FeedOptions { EnableCrossPartitionQuery = true })
        .Where(m => m.MetricType == "Temperature" && m.MetricValue > 100);

### <a name="parallel-query-execution"></a>Spuštění paralelní dotazu

SDK DocumentDB 1.9.0 hodnoty i všech podpory paralelní spuštění dotazu, které slouží k provádění zhoršeným latence dotazů rozdělený kolekce, i když to budou potřebovat dotyková velký počet oddílů. Například následující dotaz je nakonfigurovaný na spuštění paralelně mezi oddíly.

    // Cross-partition Order By Queries
    IQueryable<DeviceReading> crossPartitionQuery = client.CreateDocumentQuery<DeviceReading>(
        UriFactory.CreateDocumentCollectionUri("db", "coll"), 
        new FeedOptions { EnableCrossPartitionQuery = true, MaxDegreeOfParallelism = 10, MaxBufferedItemCount = 100})
        .Where(m => m.MetricType == "Temperature" && m.MetricValue > 100)
        .OrderBy(m => m.MetricValue);

Spuštění paralelní dotazu můžete ovládat optimalizace následujících parametrů:

- Nastavením `MaxDegreeOfParallelism`, můžete určit míru paralelismus tedy maximální počet souběžné síťové připojení k oddíly v kolekci. Pokud toto nastavíte na hodnotu -1, stupeň paralelismu spravuje SDK. Pokud `MaxDegreeOfParallelism` zadaný nebo nastavený na hodnotu 0, což je výchozí hodnota, bude v jediném připojení k oddílům v kolekci.
- Nastavením `MaxBufferedItemCount`, obchodujete vypnout využití paměti straně dotazu pro latence a klienta. Pokud vynecháte tento parametr nebo nastavit na hodnotu -1, kolik položek paměti při spuštění dotazu paralelní spravuje SDK.

Možnost stavu stejné kolekce, paralelní dotaz vrátí výsledky ve stejném pořadí jako sériové spouštění. Při provádění dotazu více oddílů, který zahrnuje řazení (Order a/nebo NAHOŘE), DocumentDB SDK problémy dotazu paralelně mezi oddíly a sloučí částečně seřazené výsledky na straně klienta globálně uspořádaných výsledky.

### <a name="executing-stored-procedures"></a>Spuštění uložené procedury

Můžete taky spustit jednotlivé transakce proti dokumentů se stejným ID zařízení, například pokud jste zachování agregační funkce a poslední stav zařízení v jednom dokumentu. 

    await client.ExecuteStoredProcedureAsync<DeviceReading>(
        UriFactory.CreateStoredProcedureUri("db", "coll", "SetLatestStateAcrossReadings"),
        new RequestOptions { PartitionKey = new PartitionKey("XMS-001") }, 
        "XMS-001-FE24C");

V části Další podíváme na tom, jak můžete přesunout do oddílů kolekce z jedním oddílem kolekcí.

<a name="migrating-from-single-partition"></a>
### <a name="migrating-from-single-partition-to-partitioned-collections"></a>Migrace z jedním oddílem rozdělený kolekcích
Pokud aplikace přes kolekce jedním oddílem potřebuje vyšší výkon (> 10 000 RU/s) nebo větší úložný prostor (> 10GB), [Nástroje pro migraci dat DocumentDB](http://www.microsoft.com/downloads/details.aspx?FamilyID=cda7703a-2774-4c07-adcc-ad02ddc1a44d) slouží k migraci dat z kolekci jedním oddílem ke kolekci oddíly. 

Migrace z kolekce jedním oddílem do kolekce oddíly

1. Exportujte dat z kolekce jedním oddílem JSON. Další informace najdete v článku [Export do souboru JSON](documentdb-import-data.md#export-to-json-file) .
2. Importujte data do oddílů kolekce vytvořená pomocí klíčových definice oddílu a víc než 10 000 jednotek žádost na druhý výkon, jak je vidět v následujícím příkladu. Další informace najdete v článku [Import DocumentDB](documentdb-import-data.md#DocumentDBSeqTarget) .

![Migraci dat do kolekce Partitioned v DocumentDB][3]  

>[AZURE.TIP] Pro rychlejší importu zvažte prodloužení číslo z paralelní požadavků 100 nebo vyšší využít k dispozici pro oddíly kolekce vyšší výkon. 

Teď můžeme jste dokončili základy, Podívejme se na několik důležitých navrhování při práci s obrázky v DocumentDB klávesová oddíl.

## <a name="designing-for-partitioning"></a>Navrhování pro rozdělení
Volba klíč oddílu je důležité rozhodnutí, který budete muset vytvořit v době návrhu. Tato část popisuje některé nevýhody zahrnuté do výběru oddíl klíč pro vaši kolekci.

### <a name="partition-key-as-the-transaction-boundary"></a>Klíč oddílu jako hranici transakce
U možnosti oddíl klíč by měl zůstatek potřeba povolit používání transakce proti požadovaného k distribuci entit napříč několika kláves oddíl zajistit scalable řešení. Na jeden krajní můžete nastavit stejné klíč oddílu pro všechny dokumenty, ale tím může omezit škálovatelnost řešení. V ostatních krajní můžete přiřadit jedinečné oddíl klíč pro každý dokument, který bude vysoce scalable, ale chcete zabránit použití křížového dokumentu transakce prostřednictvím uložené procedury a aktivace. Klíč ideální oddílu je taková, která vám umožní použít efektivně dotazů, který má dostatečné mohutnosti a zkontrolujte, že vaše řešení scalable. 

### <a name="avoiding-storage-and-performance-bottlenecks"></a>Ukládání a výkonu vyloučit kritická místa 
Je také třeba vyberte vlastnosti, které umožňuje zápisy rozvržena počet různých hodnot. Požadavky na stejné klíč oddílu nesmí překročit výkon jeden oddíl a bude omezena. Takže je důležité vyberte klíč oddílu, který nemá za následek **"aktivní body"** v rámci aplikace. Velikost celkové úložiště u dokumentů se stejným klíčem oddíl nelze překročit 10 GB úložiště. 

### <a name="examples-of-good-partition-keys"></a>Příklady správného klíče
Jak vybrat klíč oddílu aplikace několik příkladů:

* Pokud jste implementace back-end profilu uživatele, ID uživatele je dobrá volba, pokud klíč oddílu.
* Pokud máte uložené IoT data například stav zařízení, ID zařízení je dobrá volba, pokud klíč oddílu.
* Pokud používáte DocumentDB pro protokolování časových řad dat, ID název hostitele nebo obrázku je dobrá volba, pokud klíč oddílu.
* Pokud máte víc klienta architektura ID klienta je dobrá volba, pokud klíč oddílu.

Nezapomeňte, že v některých případech použít (například IoT a uživatelských profilech jsme je popsali výše) klíč oddílu může být stejné jako id (klíč dokumentu). V jiných jako dat časové řady bude pravděpodobně klíč oddílu, který se liší od id.

### <a name="partitioning-and-loggingtime-series-data"></a>Rozdělení a protokolování/časových řad dat
Jednou z nejběžnější případy použití DocumentDB je pro protokolování a telemetrie. Je důležité si vybrat klíč správného vzhledem k tomu může být nutné pro čtení i zápis velké objemy dat. Volba, závisí na vaší pro čtení a zápis sazby a typy dotazů, které budete chtít spustit. Tady je několik tipů na výběr správného klíče.

- Pokud váš případ použití zahrnuje malé sazbu přepíší acculumating dlouhé časové období a potřeba dotazu rozsahy časová razítka další filtry, pak pomocí kumulativní časové razítko například kalendářních dat a klíč oddílu je dobrým přístupem. To umožňuje dotazu přes všechna data pro datum jeden oddíl. 
- Pokud váš pracovní zátěž je těžké zapsat, což je obecně nejčastěji používaných, byste měli použít klíč oddílu, který není založený na časové razítko tak, aby DocumentDB můžete široké zápisy přes počet oddílů. Název hostitele, ID procesu, ID aktivity nebo jiné vlastnosti s vysokou mohutnosti následuje Dobrá volba, pokud. 
- Třetí přístup je hybridní jednu místo, kam jste ještě víc kolekcí, jeden za každý den/měsíc a klíč oddílu je podrobného vlastnost jako název hostitele. Výhodou, který lze nastavit jiné výkonu úrovně podle časového intervalu, například kolekce v aktuálním měsíci máte k dispozici s vyšší výkon vzhledem k tomu slouží čtení a zápis, že předchozí měsíců s nižší výkon od budou sloužit pouze čtení.

### <a name="partitioning-and-multi-tenancy"></a>Rozdělení a víceklientská
Pokud jsou implementace více klienta aplikace pomocí DocumentDB, existují dva hlavní vzorků pro provádění nájmu s DocumentDB – jeden oddíl jednoho klienta spolu s jednu kolekci jednoho klienta. Tady jsou k nim klady a zápory pro jednotlivá pole:

* Vždycky jen jednu klávesu oddíl jednoho klienta: V tomto modelu klienti jsou použít pro společné umísťování v rámci jedné kolekce. Ale dotazů a vloží dokumentů, které tvoří jednoho tenanta, kterého lze provést proti jeden oddíl. Můžete implementovat transakční použití logických operátorů ve všech dokumentech ve klienta. Protože víc klientů sdílet kolekce, můžete ušetřit náklady úložiště a výkon fond zdrojů pro klienty v rámci jedné kolekce spíše než zřizování navíc rezervou pro každého klienta. Nevýhodou je, že nemusíte výkonu izolace jednoho klienta. Zvýšení výkonu/výkon platí pro celou kolekci a cílových zvýšení pro klienty.
* Jedna kolekce jednoho klienta: každého klienta obsahuje vlastní kolekce. V tomto modelu můžete rezervovat výkonu jednoho klienta. S jeho DocumentDB nový spotřebu na základě ceny model je tento model efektivnější pro více klienta aplikace s malým počtem poštovních klientů.

Můžete taky použít kombinaci/režimy collocates malé klientů a migruje větší tenantů k vlastní kolekci.

## <a name="next-steps"></a>Další kroky
V tomto článku Popsali jsme způsob rozdělení funguje v Azure DocumentDB, jak můžete vytvořit rozdělený kolekcí a jak si můžete vybrat správného klíč pro aplikaci. 

-   Umožňuje proveďte měřítko a výkonu testováním pomocí DocumentDB. Ukázka naleznete v tématu [Výkon a měřítko testováním pomocí Azure DocumentDB](documentdb-performance-testing.md) .
-   Začínáme s [SDK](documentdb-sdk-dotnet.md) nebo [Rozhraní REST API](https://msdn.microsoft.com/library/azure/dn781481.aspx) kódování
-   Další informace o [zřizování výkon DocumentDB](documentdb-performance-levels.md)
-   Pokud chcete přizpůsobit, jak aplikace provede oddílů, můžete připojit k provedení dělení vlastní klienta. V tématu [klientských rozdělení podpory](documentdb-sharding.md).

[1]: ./media/documentdb-partition-data/partitioning.png
[2]: ./media/documentdb-partition-data/single-and-partitioned.png
[3]: ./media/documentdb-partition-data/documentdb-migration-partitioned-collection.png  

 
