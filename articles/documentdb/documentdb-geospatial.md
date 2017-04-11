<properties 
    pageTitle="Práce s geografická data v Azure DocumentDB | Microsoft Azure" 
    description="Pochopte, jak můžete vytvářet, index a dotaz Prostorové objekty se Azure DocumentDB." 
    services="documentdb" 
    documentationCenter="" 
    authors="arramac" 
    manager="jhubbard" 
    editor="monicar"/>

<tags 
    ms.service="documentdb" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.workload="data-services" 
    ms.date="10/25/2016" 
    ms.author="arramac"/>
    
# <a name="working-with-geospatial-data-in-azure-documentdb"></a>Práce s geografická data v Azure DocumentDB

Tento článek je Úvod k funkcím geografická v [Azure DocumentDB](https://azure.microsoft.com/services/documentdb/). Po přečtení to, budou moct odpovězte na následující otázky:

- Jak uložit prostorových dat v Azure DocumentDB
- Jak lze dotazu geografická data v DocumentDB Azure SQL a LINQ?
- Jak povolit nebo zakázat prostorové indexování v DocumentDB?

Přečtěte si téma tento [Github projektu](https://github.com/Azure/azure-documentdb-dotnet/blob/master/samples/code-samples/Geospatial/Program.cs) pro ukázek kódu.

## <a name="introduction-to-spatial-data"></a>Základní informace o prostorových dat

Prostorové data popisují umístění a obrazce objektů na místo. Ve většině aplikací tyto odpovídají objekty na světě, tedy geografická data. Prostorové dat lze znázornit umístění člověka, místo potřebné nebo hranici města nebo jezera. Běžné případy použití často zahrnují blízkost dotazů, například "najít všechny kavárně poblíž aktuální umístění". 

### <a name="geojson"></a>GeoJSON
DocumentDB podporuje indexování a dotazování geografická bodů dat, která představuje pomocí [GeoJSON specifikace](http://geojson.org/geojson-spec.html). GeoJSON jsou datové struktury vždy platné JSON objekty, mohou být uloženy a dotazovaném pomocí DocumentDB bez specializované nástroje knihovny. SDK DocumentDB poskytují třídy podpory a metody, které usnadňují práci s daty prostorové. 

### <a name="points-linestrings-and-polygons"></a>Body, linestrings a mnohoúhelník
**Bod** označuje jednoho umístění v prostoru. V geografická data představuje bod přesné místo, které by mohly být ulice potravin úložiště, prohlížení, Auto nebo město.  V GeoJSON (a DocumentDB) pomocí jeho souřadnice dvojice nebo šířky a šířky představuje bod. Tady je příklad JSON bod.

**Bodů DocumentDB**

    {
       "type":"Point",
       "coordinates":[ 31.9, -4.8 ]
    }

>[AZURE.NOTE] Specifikace GeoJSON Určuje délky první a šířky druhé. Stejně jako v jiných aplikacích mapování šířky a délky jsou úhlu a jsou vyjádřeny z hlediska stupňů. Hodnoty délky měřeno od poledníkem objektového a dosahuje jejich stáří – 180 a 180.0 stupňů a šířky hodnoty jsou měřeno od rovníku a dosahuje jejich stáří-90.0 a 90.0 stupňů. 
>
> DocumentDB interpretovat souřadnice uvedených na odkaz systém WGS 84. Níže naleznete další informace o systémech souřadnicového odkaz.

To může být vožený v dokumentu DocumentDB uvedeno v tomto příkladu profilu uživatele, který obsahuje data umístění:

**Pomocí umístění uložených v DocumentDB profilu**

    {
       "id":"documentdb-profile",
       "screen_name":"@DocumentDB",
       "city":"Redmond",
       "topics":[ "NoSQL", "Javascript" ],
       "location":{
          "type":"Point",
          "coordinates":[ 31.9, -4.8 ]
       }
    }

Kromě body podporuje GeoJSON také LineStrings a mnohoúhelník. **LineStrings** představuje posloupnost dva nebo více bodů v prostoru a segmentů čar, která se připojují je. V geografická data jsou linestrings běžně používaných představující silnice nebo řeky. **Mnohoúhelník** je okraj propojené body, které tvoří skrytých LineString. Mnohoúhelníku se běžně používají představující natural sestavy jako jezera nebo politických příslušnosti jako měst a států. Tady je příklad mnohoúhelníku v DocumentDB. 

**Mnohoúhelníku v DocumentDB**

    {
       "type":"Polygon",
       "coordinates":[
           [ 31.8, -5 ],
           [ 31.8, -4.7 ],
           [ 32, -4.7 ],
           [ 32, -5 ],
           [ 31.8, -5 ]
       ]
    }

>[AZURE.NOTE] Specifikace GeoJSON vyžaduje, aby pro platná mnohoúhelníku poslední souřadnic dvojici podle by měl být stejný jako první, vytvoření uzavřený obrazec.
>
>Musí být zadány body v rámci mnohoúhelníku v pořadí proti směru hodinových ručiček. Mnohoúhelníku uvedené v pořadí, ve směru hodinových ručiček představuje inverzní oblasti v něm obsažené.

Kromě bodu, LineString a mnohoúhelník GeoJSON také určuje znázornění způsob seskupení několika geografická místech, a taky jak libovolného vlastnosti přidružit zeměpisná poloha jako **funkce**. Protože tyto objekty jsou platné JSON, mohou všechny být uloženy a zpracovávají v DocumentDB. Ale DocumentDB podporuje pouze automatické indexování bodů.

### <a name="coordinate-reference-systems"></a>Koordinaci odkaz systémy

Protože obrazec země je nepravidelný, souřadnice geografická data představuje systému mnoho souřadnic referenční rezervační (systém), oba objekty mají vlastní referenční snímky a měrné jednotky. Například "vnitrostátní mřížky z Británie" je odkaz systém velmi přesné pro Spojené království, ale ne mimo něj. 

Nejoblíbenější používá dnes se geodetické systém světě [WGS 84](http://earth-info.nga.mil/GandG/wgs84/). GPS zařízení a mnoho mapování služby včetně mapy Google a rozhraní API mapy Bing pomocí WGS 84. DocumentDB podporuje indexování a dotazování geografická data pomocí rezervační WGS 84 systém pouze. 

## <a name="creating-documents-with-spatial-data"></a>Vytváření dokumentů s prostorových dat
Při vytváření dokumentů, které obsahují hodnoty GeoJSON budou automaticky indexovaných prostorové indexu podle indexování zásad kolekci. Pokud pracujete s DocumentDB SDK v jazyce dynamicky zadaný jako Python nebo Node.js, musíte vytvořit platné GeoJSON.

**Vytvoření dokumentu s geografická data v Node.js**

    var userProfileDocument = {
       "name":"documentdb",
       "location":{
          "type":"Point",
          "coordinates":[ -122.12, 47.66 ]
       }
    };

    client.createDocument(`dbs/${databaseName}/colls/${collectionName}`, userProfileDocument, (err, created) => {
        // additional code within the callback
    });

Pokud pracujete s .NET (nebo Java) SDK, můžete použít nové čárky a mnohoúhelník třídy v rámci oboru Microsoft.Azure.Documents.Spatial k vložení údajů z umístění v rámci aplikačních objektů. Tyto třídy zjednodušit serializace a rekonstrukce prostorových dat do GeoJSON.

**Vytvoření dokumentu s geografická data v .NET**

    using Microsoft.Azure.Documents.Spatial;
    
    public class UserProfile
    {
        [JsonProperty("name")]
        public string Name { get; set; }

        [JsonProperty("location")]
        public Point Location { get; set; }
        
        // More properties
    }
    
    await client.CreateDocumentAsync(
        UriFactory.CreateDocumentCollectionUri("db", "profiles"), 
        new UserProfile 
        { 
            Name = "documentdb", 
            Location = new Point (-122.12, 47.66) 
        });

Pokud nemáte informace šířky a délky, ale máte fyzické adresy nebo název umístění, například město nebo země, můžete vyhledat skutečné souřadnice pomocí provedení geografického kódování služby, jako je služby ZBÝVAJÍCÍ mapy Bing. Další informace o provedení geografického kódování mapy Bing [v tomto poli](https://msdn.microsoft.com/library/ff701713.aspx).

## <a name="querying-spatial-types"></a>Dotazování prostorové typy

Teď můžeme jste dostali pohled na tom, jak vložit geografická data, Podívejme se na to, jak k vytvoření dotazu tato data pomocí DocumentDB pomocí SQL a LINQ.

### <a name="spatial-sql-built-in-functions"></a>Prostorové předdefinované funkce SQL
DocumentDB geografická dotazování podporuje následující předdefinované funkce Otevřít geografická Consortium (OGC). Podrobné informace o úplná sada předdefinovaných funkcí v jazyce SQL získáte [DocumentDB dotazu](documentdb-sql-query.md).

<table>
<tr>
  <td><strong>Použití</strong></td>
  <td><strong>Popis</strong></td>
</tr>
<tr>
  <td>ST_DISTANCE (point_expr, point_expr)</td>
  <td>Vrátí vzdálenost mezi dva výrazy GeoJSON bodu.</td>
</tr>
<tr>
  <td>ST_WITHIN (point_expr, polygon_expr)</td>
  <td>Vrátí logický výraz určující, zda je v rámci mnohoúhelník GeoJSON ve druhém argumentu GeoJSON čárky zadaný v prvním argumentu.</td>
</tr>
<tr>
  <td>ST_ISVALID</td>
  <td>Vrátí hodnotu typu Boolean označující, zda je zadaný výraz čárky nebo mnohoúhelník GeoJSON platná.</td>
</tr>
<tr>
  <td>ST_ISVALIDDETAILED</td>
  <td>Vrátí hodnotu JSON obsahující logickou hodnotu hodnotu, pokud platí zadaný výraz čárky nebo mnohoúhelník GeoJSON a neplatný, kromě důvod, proč jako hodnotu řetězce.</td>
</tr>
</table>

Prostorové funkcí lze provádět blízkost dotazů prostorových dat. Tady je příklad dotazu, která vrací všechny rodinný dokumenty, které spadají do 30 km zadaného umístění pomocí předdefinované funkce ST_DISTANCE. 

**Dotaz**

    SELECT f.id 
    FROM Families f 
    WHERE ST_DISTANCE(f.location, {'type': 'Point', 'coordinates':[31.9, -4.8]}) < 30000

**Výsledky**

    [{
      "id": "WakefieldFamily"
    }]

Jestliže zahrnete prostorové indexování indexování zásady, potom "vzdálenost dotazů" bude zasílat efektivní prostřednictvím index. Podrobné informace o prostorových indexování najdete v části. Pokud nemáte prostorové indexu pro zadané cesty, můžete pořád dělat prostorové dotazů zadáním `x-ms-documentdb-query-enable-scan` hlavičky se sadou hodnotu "true". V .NET stačí předáním volitelný argument **FeedOptions** dotazů se sadou [EnableScanInQuery](https://msdn.microsoft.com/library/microsoft.azure.documents.client.feedoptions.enablescaninquery.aspx#P:Microsoft.Azure.Documents.Client.FeedOptions.EnableScanInQuery) true (pravda). 

ST_WITHIN lze použít ke kontrole, pokud bod ležel mnohoúhelník. Běžně mnohoúhelníku slouží k představují hranice jako poštovní směrovací čísla, stát hranice nebo přírodní sestavy. Znovu Jestliže zahrnete prostorové indexování indexování zásady, potom "v" dotazů bude zasílat efektivní prostřednictvím index. 

Mnohoúhelník argumentů ST_WITHIN může obsahovat pouze jeden zvonění, tedy mnohoúhelníku nesmí obsahovat mezery v nich. 

**Dotaz**

    SELECT * 
    FROM Families f 
    WHERE ST_WITHIN(f.location, {
        'type':'Polygon', 
        'coordinates': [[[31.8, -5], [32, -5], [32, -4.7], [31.8, -4.7], [31.8, -5]]]
    })

**Výsledky**

    [{
      "id": "WakefieldFamily",
    }]
    
>[AZURE.NOTE] Podobně jako jak Neshoda typů funguje v DocumentDB dotazu, pokud hodnotu v poli umístění podle buď argument je poškozený nebo neplatné a potom ji vyhodnotí a **Nedefinováno** vyhodnocené dokumentu přeskočit ve výsledcích dotazu. Pokud dotaz vrátí žádné výsledky, spusťte ST_ISVALIDDETAILED k ladění proč typ spatail není platný.     

DocumentDB taky podporuje provádění inverzní dotazy, tedy můžete indexovat mnohoúhelníku nebo řádky v DocumentDB a potom dotazu oblastech, které obsahují určité bod. Tento způsob je běžně používaných v doprava k identifikaci například když zboží zadá nebo ponechá určené oblasti. 

**Dotaz**

    SELECT * 
    FROM Areas a 
    WHERE ST_WITHIN({'type': 'Point', 'coordinates':[31.9, -4.8]}, a.location)


**Výsledky**

    [{
      "id": "MyDesignatedLocation",
      "location": {
        "type":"Polygon", 
        "coordinates": [[[31.8, -5], [32, -5], [32, -4.7], [31.8, -4.7], [31.8, -5]]]
      }
    }]

ST_ISVALID a ST_ISVALIDDETAILED lze zkontrolovat, zda je platný prostorové objektu. Například následující dotaz kontroluje platnost bodu s nepřítomnosti hodnotu šířky oblasti (-132.8). ST_ISVALID vrátí jenom logickou hodnotu a vrátí ST_ISVALIDDETAILED logické hodnoty a řetězec obsahující důvod, proč nebude považována za neplatné.

**Dotaz**

    SELECT ST_ISVALID({ "type": "Point", "coordinates": [31.9, -132.8] })

**Výsledky**

    [{
      "$1": false
    }]

Tyto funkce lze také ověřit mnohoúhelník. Třeba tady používáme ST_ISVALIDDETAILED ověřuje mnohoúhelník, které není uzavřeno. 

**Dotaz**

    SELECT ST_ISVALIDDETAILED({ "type": "Polygon", "coordinates": [[ 
        [ 31.8, -5 ], [ 31.8, -4.7 ], [ 32, -4.7 ], [ 32, -5 ] 
        ]]})

**Výsledky**

    [{
       "$1": { 
          "valid": false, 
          "reason": "The Polygon input is not valid because the start and end points of the ring number 1 are not the same. Each ring of a polygon must have the same start and end points." 
        }
    }]
    
### <a name="linq-querying-in-the-net-sdk"></a>LINQ dotazování v .NET SDK

DocumentDB .NET SDK taky poskytovatelů stub metody `Distance()` a `Within()` pro použití v rámci LINQ výrazy. Zprostředkovatel DocumentDB LINQ překládá tato metoda volání odpovídající hovory předdefinované funkce SQL (ST_DISTANCE a ST_WITHIN v tomto pořadí). 

Tady je příklad LINQ dotazu, který najde všechny dokumenty v kolekci DocumentDB jehož hodnotu "umístění" je v rámci poloměrem 30km zadaného přejděte pomocí LINQ.

**LINQ dotaz pro distance (vzdálenost)**

    foreach (UserProfile user in client.CreateDocumentQuery<UserProfile>(UriFactory.CreateDocumentCollectionUri("db", "profiles"))
        .Where(u => u.ProfileType == "Public" && a.Location.Distance(new Point(32.33, -4.66)) < 30000))
    {
        Console.WriteLine("\t" + user);
    }

Tady je podobně dotazu pro vyhledání všechny dokumenty, jehož "umístění" je součástí zadané pole/mnohoúhelník. 

**LINQ dotazu v rámci**

    Polygon rectangularArea = new Polygon(
        new[]
        {
            new LinearRing(new [] {
                new Position(31.8, -5),
                new Position(32, -5),
                new Position(32, -4.7),
                new Position(31.8, -4.7),
                new Position(31.8, -5)
            })
        });

    foreach (UserProfile user in client.CreateDocumentQuery<UserProfile>(UriFactory.CreateDocumentCollectionUri("db", "profiles"))
        .Where(a => a.Location.Within(rectangularArea)))
    {
        Console.WriteLine("\t" + user);
    }


Teď můžeme jste dostali si téma Jak hledat dokumenty pomocí LINQ a SQL znalostní bázi, Podívejme se na postup při konfiguraci DocumentDB prostorové indexování.

## <a name="indexing"></a>Indexování

Podle jsme [Schématu bez ohledu na indexování s Azure DocumentDB](http://www.vldb.org/pvldb/vol8/p1668-shukla.pdf) papíru, jsme navržený DocumentDB na databázový stroj je opravdu schématu tím a poskytuje prvotřídní podporu pro JSON. Databázový stroj zápisu optimalizovaných z DocumentDB nativně rozumí prostorové (body, mnohoúhelníku a řádky) prezentovaná data ve standardu GeoJSON.

Ve zkratce geometrii je plánované z geodetické souřadnice na 2D plochu a pak postupně rozdělit buňky pomocí **quadtree**. Tyto buňky jsou namapované na 1D na základě polohy buňku v rámci **Hilbertův místo vyplňování křivky**, který zachovává místo bodů. Kromě indexovat data umístění prochází proces jmenoval **teselace**, tedy všechny buňky, které protínají umístění jsou označené a uložených jako zkratky na rejstřík DocumentDB. V době dotazu argumenty jako bodů a mnohoúhelníku jsou také teselace sestavy extrahovat ID oblasti příslušnou buňku a potom použitý k načtení dat z indexu.

Pokud zadáte zásadu indexování, která obsahuje prostorové index / * (všechny cesty), pak všechny body nalézt v rámci kolekce indexovaných efektivně prostorové dotazů (ST_WITHIN a ST_DISTANCE). Prostorové indexy nemají hodnotu přesnost a vždy použít výchozí hodnoty s přesností.

>[AZURE.NOTE] DocumentDB podporuje automatické indexování body, mnohoúhelníku a LineStrings

Následující úryvek JSON zobrazující indexování zásady s prostorové indexování povoleno, tedy indexovat kamkoli GeoJSON nacházejí ve dokumentů pro prostorové dotazu. Chcete-li změnit indexování zásad pomocí portálu Azure, můžete použít následující JSON indexování zásady Povolit prostorové indexování kolekci.

**Kolekce indexování zásad JSON s Spatial povolena u bodů a mnohoúhelník**

    {
       "automatic":true,
       "indexingMode":"Consistent",
       "includedPaths":[
          {
             "path":"/*",
             "indexes":[
                {
                   "kind":"Range",
                   "dataType":"String",
                   "precision":-1
                },
                {
                   "kind":"Range",
                   "dataType":"Number",
                   "precision":-1
                },
                {
                   "kind":"Spatial",
                   "dataType":"Point"
                },
                {
                   "kind":"Spatial",
                   "dataType":"Polygon"
                }                
             ]
          }
       ],
       "excludedPaths":[
       ]
    }

Tady je fragment kódu v .NET, který ukazuje, jak vytvořit kolekci s prostorové indexování zapnuté pro všechny cesty obsahující body. 

**Vytvoření kolekce s prostorové indexování**

    DocumentCollection spatialData = new DocumentCollection()
    spatialData.IndexingPolicy = new IndexingPolicy(new SpatialIndex(DataType.Point)); //override to turn spatial on by default
    collection = await client.CreateDocumentCollectionAsync(UriFactory.CreateDatabaseUri("db"), spatialData);

A ukážeme si, jak můžete upravit existující kolekce umožní využít výhod prostorové indexování přes žádné body, které jsou uložené v dokumentech.

**Upravit existující kolekce s prostorové indexování**

    Console.WriteLine("Updating collection with spatial indexing enabled in indexing policy...");
    collection.IndexingPolicy = new IndexingPolicy(new SpatialIndex(DataType.Point));
    await client.ReplaceDocumentCollectionAsync(collection);

    Console.WriteLine("Waiting for indexing to complete...");
    long indexTransformationProgress = 0;
    while (indexTransformationProgress < 100)
    {
        ResourceResponse<DocumentCollection> response = await client.ReadDocumentCollectionAsync(UriFactory.CreateDocumentCollectionUri("db", "coll"));
        indexTransformationProgress = response.IndexTransformationProgress;

        await Task.Delay(TimeSpan.FromSeconds(1));
    }

> [AZURE.NOTE] Pokud umístění GeoJSON hodnotu v dokumentu je poškozený nebo neplatné, pak ji nebude indexování pro prostorové dotazu. Můžete ověřit umístění hodnoty pomocí ST_ISVALID a ST_ISVALIDDETAILED.
>
> Pokud vaše kolekce definice klíč oddílu, indexování transformace průběh není uvedena. 

## <a name="next-steps"></a>Další kroky
Teď máte zkušenosti o tom, jak začít s podporou geografická v DocumentDB, máte tyto možnosti:

- Zahájení kódování [geografická .NET ukázky na Github](https://github.com/Azure/azure-documentdb-dotnet/blob/fcf23d134fc5019397dcf7ab97d8d6456cd94820/samples/code-samples/Geospatial/Program.cs)
- Získat rukou na s geografická dotazování na [Hřišť DocumentDB dotazu](http://www.documentdb.com/sql/demo#geospatial)
- Další informace o [DocumentDB dotazu](documentdb-sql-query.md)
- Další informace o [DocumentDB indexování zásady](documentdb-indexing-policies.md)
