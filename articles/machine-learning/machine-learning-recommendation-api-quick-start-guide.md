<properties 
    pageTitle="Úvodní příručka: počítače výukové doporučení API | Microsoft Azure" 
    description="Azure počítače výukové doporučení – úvodní příručka" 
    services="machine-learning" 
    documentationCenter="" 
    authors="LuisCabrer" 
    manager="jhubbard" 
    editor="cgronlun"/>

<tags 
    ms.service="machine-learning" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/08/2016" 
    ms.author="luisca"/>

# <a name="quick-start-guide-for-the-machine-learning-recommendations-api"></a>Úvodní příručka pro rozhraní API doporučení výukové počítače

>[AZURE.NOTE]Měli byste začít používat službu kognitivní rozhraní API doporučení místo tato verze. Kognitivní službu doporučení se nahradí tuto službu a s novými funkcemi bude vyvinuté tam. Má nové funkce jako dávky podpory lepší rozhraní API aplikace Explorer, opět dostanete čistší prostředí povrchový, jednotnější registrace/fakturace rozhraní API, atd.
> Další informace o [migraci do nové kognitivní služby](http://aka.ms/recomigrate)


Tento dokument popisuje, jak na integrovaný služby nebo aplikace pro použití Microsoft Azure počítače výukové doporučení. Další informace na rozhraní API doporučení můžete najít v [Galerii](http://gallery.cortanaanalytics.com/MachineLearningAPI/Recommendations-2).

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

##<a name="general-overview"></a>Obecné základní informace

Použít Azure počítače výukové doporučení, je potřeba provést následující kroky:

* Vytvoření modelu – modelu je kontejner použití dat, katalogu dat a doporučení modelu.
* Import katalogu dat – katalog informacemi metadat na položky. 
* Údaje o využití import - použití zásad správy informací můžete odeslat v jedním ze způsobů 2 (nebo obojí):
    * Uložením souboru, který obsahuje data o využití.
    * Tak, že data pořízení události.
    Obvykle nahrát soubor použití za účelem vytvoření datového modelu pro počáteční doporučení (zavádění) a použijte ji dokud systému shromažďuje dost dat pomocí formátu pořízení data.
* Vytvoření modelu doporučení: to je asynchronní operace, ve kterém systému doporučení zabírá všechna data použití a vytvoří model doporučení. Tato operace může trvat několik minut nebo i několik hodin v závislosti na velikosti data a konfigurace parametrů Tvůrce dotazů. Při aktivaci sestavení, dostanete ID tvůrce dotazů. Ho použijte k prohlédnutí po ukončení procesu sestavení předtím, než začnete používat doporučení.
* Používání doporučení - Get doporučení pro určité položky nebo seznam položek.

Všechny výše uvedené kroky se dá prostřednictvím rozhraní API Azure počítače výukové doporučení.  Můžete si stáhnout ukázku aplikace implementující každý z těchto kroků z [Galerie i.](http://1drv.ms/1xeO2F3)

##<a name="limitations"></a>Omezení

* Model na jedno předplatné maximálně 10.
* Maximální počet položek, které můžou obsahovat katalog je 100,000.
* Maximální počet bodů použití, které jsou k dispozici je 5 000 000 ~. Nejstarší se odstraní Pokud nové bude nahráli nebo vykázaného.
* Maximální velikosti doručovaných data, která mohou být odeslány do příspěvku (například import katalogu dat, import o využití) je 200MB.
* Počet transakce za sekundu sestavení modelu doporučení, který není aktivní je ~ 2TPS. Sestavení modelu doporučení, která je aktivní můžou obsahovat 20TPS.

##<a name="integration"></a>Integrace

###<a name="authentication"></a>Ověřování
Micosoft Azure Marketplace podporuje základní nebo OAuth metody ověřování. Můžete snadno najít klíče účtu tak, že přejdete na klíče marketplace v části [Nastavení účtu](https://datamarket.azure.com/account/keys). 
####<a name="basic-authentication"></a>Základní ověřování
Přidání záhlaví se tak mohli ověřovat:

    Authorization: Basic <creds>
               
    Where <creds> = ConvertToBase64("AccountKey:" + yourAccountKey);  
    
Převod na ve formátu Base 64 (C#)

    var bytes = Encoding.UTF8.GetBytes("AccountKey:" + yourAccountKey);
    var creds = Convert.ToBase64String(bytes);
    
Převod na ve formátu Base 64 (JavaScript)

    var creds = window.btoa("AccountKey" + ":" + yourAccountKey);
    



###<a name="service-uri"></a>Služba URI
Odmocnina služby URI pro rozhraní API, doporučení výukové Azure počítače se [zde.](https://api.datamarket.azure.com/amla/recommendations/v2/)

Kompletní URI vyjádřena pomocí prvků specifikaci OData.

###<a name="api-version"></a>Rozhraní API verze
Rozhraní API volání bude mít na konci, k dotazu parametr s názvem apiVersion, nastavené na "1.0".

###<a name="ids-are-case-sensitive"></a>ID jsou malá a velká písmena
ID vrácené jednotlivých rozhraní API se rozlišují velká a bude použito jako takové při předávání jako parametrů v další hovory rozhraní API. Například modelu ID a katalogu ID jsou malá a velká písmena.

###<a name="create-a-model"></a>Vytvoření modelu
Vytvoření žádost o "Vytvoření modelu":

| Metoda HTTP | IDENTIFIKÁTOR URI |
|:--------|:--------|
|PŘÍSPĚVEK     |`<rootURI>/CreateModel?modelName=%27<model_name>%27&apiVersion=%271.0%27`<br><br>Příklad:<br>`<rootURI>/CreateModel?modelName=%27MyFirstModel%27&apiVersion=%271.0%27`|

|   Název parametru  |   Platné hodnoty                        |
|:--------          |:--------                              |
|   modelName   |   Jenom písmena (A-Z, z), číslice (0 – 9), spojovníky (--) a jsou povoleny podtržítka (_).<br>Maximální počet znaků: 20 |
|   apiVersion      | 1.0 |
|||
| Žádost o textu | ŽÁDNÁ |


**Odpověď**:

Nastavit informace HTTP stavový kód: 200

- `feed/entry/content/properties/id`-Obsahuje ID modelu.
**Poznámka**: velká a malá písmena je ID modelu.

OData XML

    <feed xmlns:base="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/CreateModel" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
      <title type="text" />
      <subtitle type="text">Create A New Model</subtitle>
      <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/CreateModel?modelName='MyFirstModel'&amp;apiVersion='1.0'</id>
      <rights type="text" />
      <updated>2014-10-05T06:35:21Z</updated>
     <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/CreateModel?modelName='MyFirstModel'&amp;apiVersion='1.0'" />
      <entry>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/CreateModel?modelName='MyFirstModel'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
    <title type="text">CreateANewModelEntity2</title>
    <updated>2014-10-05T06:35:21Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/CreateModel?modelName='MyFirstModel'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Id m:type="Edm.String">a658c626-2baa-43a7-ac98-f6ee26120a12</d:Id>
        <d:Name m:type="Edm.String">MyFirstModel</d:Name>
        <d:Date m:type="Edm.String">10/5/2014 6:35:19 AM</d:Date>
        <d:Status m:type="Edm.String">Created</d:Status>
        <d:HasActiveBuild m:type="Edm.String">false</d:HasActiveBuild>
        <d:BuildId m:type="Edm.String">-1</d:BuildId>
        <d:Mpr m:type="Edm.String">0</d:Mpr>
        <d:UserName m:type="Edm.String">5-4058-ab36-1fe254f05102@dm.com</d:UserName>
        <d:Description m:type="Edm.String"></d:Description>
      </m:properties>
    </content>
      </entry>
    </feed>


###<a name="import-catalog-data"></a>Import katalogu dat

Pokud odešlete několik katalogu souborů do stejné modelu s několika voláním jsme vloží jenom nové položky katalogu. Existující položky zůstanou s původní hodnoty.

| Metoda HTTP | IDENTIFIKÁTOR URI |
|:--------|:--------|
|PŘÍSPĚVEK     |`<rootURI>/ImportCatalogFile?modelId=%27<modelId>%27&filename=%27<fileName>%27&apiVersion=%271.0%27`<br><br>Příklad:<br>`<rootURI>/ImportCatalogFile?modelId=%27a658c626-2baa-43a7-ac98-f6ee26120a12%27&filename=%27catalog10_small.txt%27&apiVersion=%271.0%27`|

|   Název parametru  |   Platné hodnoty                        |
|:--------          |:--------                              |
|   modelId |   Jedinečný identifikátor modelu (malá a velká písmena)  |
| Název souboru | Textový identifikátor katalogu.<br>Jenom písmena (A-Z, z), číslice (0 – 9), spojovníky (--) a jsou povoleny podtržítka (_).<br>Maximální délka: 50 |
|   apiVersion      | 1.0 |
|||
| Žádost o textu | Katalog dat. Formát:<br>`<Item Id>,<Item Name>,<Item Category>[,<description>]`<br><br><table><tr><th>Jméno</th><th>Povinné</th><th>Typ</th><th>Popis</th></tr><tr><td>Id položky</td><td>Ano</td><td>Alfanumerický, maximální délka 50</td><td>Jedinečný identifikátor položky</td></tr><tr><td>Název položky</td><td>Ano</td><td>Alfanumerický, maximální délka 255</td><td>Název položky</td></tr><tr><td>Kategorie položek</td><td>Ano</td><td>Alfanumerický, maximální délka 255</td><td>Kategorie, ke kterému patří tuto položku (například vaření publikace obrázkům...)</td></tr><tr><td>Popis</td><td>Ne</td><td>Alfanumerický, maximální délka 4000</td><td>Popis této položky</td></tr></table><br>Maximální velikost souboru je 200MB.<br><br>Příklad:<br><pre>2406e770-769c-4189-89de-1c9283f93a96, Clara Callan knihy<br>21bf8088-b6c0-4509-870c-e1c7ac78304a, místnosti zapomenutí: zarezervovat Fiction (Byzantium Book)<br>3bb5cb44-d143-4bdd-a55c-443964bf4b23, Spadework, knihy<br>552a1940-21e4-4399-82bb-594b46d7ed54, omezení zvířata, knihy</pre> |


**Odpověď**:

Nastavit informace HTTP stavový kód: 200

- `Feed\entry\content\properties\LineCount`-Byla přijata počet řádků.
- `Feed\entry\content\properties\ErrorCount`-Počet řádků, které nebyly vkládat kvůli chybě.

OData XML

    <feed xmlns:base="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ImportCatalogFile" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
      <title type="text" />
        <subtitle type="text">Import catalog file</subtitle>
        <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ImportCatalogFile?modelId='a658c626-2baa-43a7-ac98-f6ee26120a12'&amp;filename='catalog10_small.txt'&amp;apiVersion='1.0'</id>
    <rights type="text" />
    <updated>2014-10-05T06:58:04Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ImportCatalogFile?modelId='a658c626-2baa-43a7-ac98-f6ee26120a12'&amp;filename='catalog10_small.txt'&amp;apiVersion='1.0'" />
    <entry>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ImportCatalogFile?modelId='a658c626-2baa-43a7-ac98-f6ee26120a12'&amp;filename='catalog10_small.txt'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
    <title type="text">ImportCatalogFileEntity2</title>
    <updated>2014-10-05T06:58:04Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ImportCatalogFile?modelId='a658c626-2baa-43a7-ac98-f6ee26120a12'&amp;filename='catalog10_small.txt'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:LineCount m:type="Edm.String">4</d:LineCount>
        <d:ErrorCount m:type="Edm.String">0</d:ErrorCount>
      </m:properties>
    </content>
     </entry>
    </feed>


###<a name="import-usage-data"></a>Import použití zásad správy informací

####<a name="uploading-a-file"></a>Nahrání souboru
Tato část popisuje nahrát použití zásad správy informací pomocí souboru. Můžete volat toto rozhraní API s použití zásad správy informací. Pro všechny hovory budou uloženy všechny použití zásad správy informací.

| Metoda HTTP | IDENTIFIKÁTOR URI |
|:--------|:--------|
|PŘÍSPĚVEK     |`<rootURI>/ImportUsageFile?modelId=%27<modelId>%27&filename=%27<fileName>%27&apiVersion=%271.0%27`<br><br>Příklad:<br>`<rootURI>/ImportUsageFile?modelId=%27a658c626-2baa-43a7-ac98-f6ee26120a12%27&filename=%27ImplicitMatrix10_Guid_small.txt%27&apiVersion=%271.0%27`|

|   Název parametru  |   Platné hodnoty                        |
|:--------          |:--------                              |
|   modelId |   Jedinečný identifikátor modelu (malá a velká písmena) |
| Název souboru | Textový identifikátor katalogu.<br>Jenom písmena (A-Z, z), číslice (0 – 9), spojovníky (--) a jsou povoleny podtržítka (_).<br>Maximální délka: 50 |
|   apiVersion      | 1.0 |
|||
| Žádost o textu | Údaje o využití. Formát:<br>`<User Id>,<Item Id>[,<Time>,<Event>]`<br><br><table><tr><th>Jméno</th><th>Povinné</th><th>Typ</th><th>Popis</th></tr><tr><td>Id uživatele</td><td>Ano</td><td>Alfanumerický</td><td>Jedinečný identifikátor uživatele</td></tr><tr><td>Id položky</td><td>Ano</td><td>Alfanumerický, maximální délka 50</td><td>Jedinečný identifikátor položky</td></tr><tr><td>Čas</td><td>Ne</td><td>Datum ve formátu: YYYY/MM/ddTHH (například 2013/06/20T10:00:00)</td><td>Čas dat</td></tr><tr><td>Události</td><td>Ne, předávaných nakonec musíte také vložit datum</td><td>Jedna z následujících akcí:<br>• Klepnutím na tlačítko<br>• RecommendationClick<br>• AddShopCart<br>• RemoveShopCart<br>• Nákupu</td><td></td></tr></table><br>Maximální velikost souboru je 200MB.<br><br>Příklad:<br><pre>149452 1b3d95e2-84e4-414c-bb38-be9cf461c347<br>6360 1b3d95e2-84e4-414c-bb38-be9cf461c347<br>50321 1b3d95e2-84e4-414c-bb38-be9cf461c347<br>71285 1b3d95e2-84e4-414c-bb38-be9cf461c347<br>224450 1b3d95e2-84e4-414c-bb38-be9cf461c347<br>236645 1b3d95e2-84e4-414c-bb38-be9cf461c347<br>107951 1b3d95e2-84e4-414c-bb38-be9cf461c347</pre> |

**Odpověď**:

Nastavit informace HTTP stavový kód: 200

- `Feed\entry\content\properties\LineCount`-Byla přijata počet řádků.
- `Feed\entry\content\properties\ErrorCount`-Počet řádků, které nebyly vkládat kvůli chybě.
- `Feed\entry\content\properties\FileId`-Souborů identifikátor.


OData XML

    <feed xmlns:base="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ImportUsageFile" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
    <title type="text" />
    <subtitle type="text">Add bulk usage data (usage file)</subtitle>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ImportUsageFile?modelId='a658c626-2baa-43a7-ac98-f6ee26120a12'&amp;filename='ImplicitMatrix10_Guid_small.txt'&amp;apiVersion='1.0'</id>
    <rights type="text" />
    <updated>2014-10-05T07:21:44Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ImportUsageFile?modelId='a658c626-2baa-43a7-ac98-f6ee26120a12'&amp;filename='ImplicitMatrix10_Guid_small.txt'&amp;apiVersion='1.0'" />
    <entry>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ImportUsageFile?modelId='a658c626-2baa-43a7-ac98-f6ee26120a12'&amp;filename='ImplicitMatrix10_Guid_small.txt'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
    <title type="text">AddBulkUsageDataUsageFileEntity2</title>
    <updated>2014-10-05T07:21:44Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ImportUsageFile?modelId='a658c626-2baa-43a7-ac98-f6ee26120a12'&amp;filename='ImplicitMatrix10_Guid_small.txt'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:LineCount m:type="Edm.String">33</d:LineCount>
        <d:ErrorCount m:type="Edm.String">0</d:ErrorCount>
        <d:FileId m:type="Edm.String">fead7c1c-db01-46c0-872f-65bcda36025d</d:FileId>
      </m:properties>
    </content>
    </entry>
    </feed>


####<a name="using-data-acquisition"></a>Použití získávání dat
Tato část popisuje odešlete událostí v reálném čase Azure počítače výukové doporučení, obvykle z webu.

| Metoda HTTP | IDENTIFIKÁTOR URI |
|:--------|:--------|
|PŘÍSPĚVEK     |`<rootURI>/AddUsageEvent?apiVersion=%271.0%27-f6ee26120a12%27&filename=%27catalog10_small.txt%27&apiVersion=%271.0%27`|

|   Název parametru  |   Platné hodnoty                        |
|:--------          |:--------                              |
|   apiVersion      | 1.0 |
|||
|Žádost o textu| Zadávání dat událostí pro každou událost, kterým chcete poslat. Byste měli odeslat stejnou relaci uživatele nebo prohlížeče stejné ID v poli ID relace. (Viz ukázka textu události dole.)|


- Příklad pro událost klikněte "na":

        <Event xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
        <ModelId>2779c063-48fb-46c1-bae3-74acddc8c1d1</ModelId>
        <SessionId>11112222</SessionId>
        <EventData>
        <EventData>
        <Name>Click</Name>
        <ItemId>21BF8088-B6C0-4509-870C-E1C7AC78304A</ItemId>
        </EventData>
        </EventData>
        </Event>

- Příklad pro událost "RecommendationClick":

        <Event xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
        <ModelId>2779c063-48fb-46c1-bae3-74acddc8c1d1</ModelId>
        <SessionId>11112222</SessionId>
        <EventData>
        <EventData>
        <Name>RecommendationClick</Name>
        <ItemId>21BF8088-B6C0-4509-870C-E1C7AC78304A</ItemId>
        </EventData>
        </EventData>
        </Event>

- Příklad pro událost "AddShopCart":

        <Event xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
        <ModelId>2779c063-48fb-46c1-bae3-74acddc8c1d1</ModelId>
        <SessionId>11112222</SessionId>
        <EventData>
        <EventData>
        <Name>AddShopCart</Name>
        <ItemId>21BF8088-B6C0-4509-870C-E1C7AC78304A</ItemId>
        </EventData>
        </EventData>
        </Event>

- Příklad pro událost "RemoveShopCart":

        <Event xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
        <ModelId>2779c063-48fb-46c1-bae3-74acddc8c1d1</ModelId>
        <SessionId>11112222</SessionId>
        <EventData>
        <EventData>
        <Name>RemoveShopCart</Name>
        <ItemId>21BF8088-B6C0-4509-870C-E1C7AC78304A</ItemId>
        </EventData>
        </EventData>
        </Event>

- Příklad pro událost "Nákup":

        <Event xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
        <ModelId>2779c063-48fb-46c1-bae3-74acddc8c1d1</ModelId>
        <SessionId>11112222</SessionId>
        <EventData>
        <EventData>
            <Name>Purchase</Name> 
            <PurchaseItems>
            <PurchaseItems>
                <ItemId>21BF8088-B6C0-4509-870C-E1C7AC78304A</ItemId>
                <Count>3</Count>
            </PurchaseItems>
        </PurchaseItems>
        </EventData>
        </EventData>
        </Event>

- Příklad odesílání 2 události, "Klikněte na" a "AddShopCart":

        <Event xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
        <ModelId>2779c063-48fb-46c1-bae3-74acddc8c1d1</ModelId>
        <SessionId>11112222</SessionId>
        <EventData>
        <EventData>
        <Name>Click</Name>
        <ItemId>21BF8088-B6C0-4509-870C-E1C7AC78304A</ItemId>
        <ItemName>itemName</ItemName>
        <ItemDescription>item description</ItemDescription>
        <ItemCategory>category</ItemCategory>
        </EventData>
        <EventData>
        <Name>AddShopCart</Name>
        <ItemId>552A1940-21E4-4399-82BB-594B46D7ED54</ItemId>
        </EventData>
        </EventData>
        </Event>

**Odpověď**: HTTP stavový kód: 200

###<a name="build-a-recommendation-model"></a>Vytvoření modelu doporučení

| Metoda HTTP | IDENTIFIKÁTOR URI |
|:--------|:--------|
|PŘÍSPĚVEK     |`<rootURI>/BuildModel?modelId=%27<modelId>%27&userDescription=%27<description>%27&apiVersion=%271.0%27`<br><br>Příklad:<br>`<rootURI>/BuildModel?modelId=%27a658c626-2baa-43a7-ac98-f6ee26120a12%27&userDescription=%27First%20build%27&apiVersion=%271.0%27`|

|   Název parametru  |   Platné hodnoty                        |
|:--------          |:--------                              |
| modelId | Jedinečný identifikátor modelu (malá a velká písmena)  |
| userDescription | Textový identifikátor katalogu. Všimněte si, že používáte mezery byste musí kódovat s % 20 místo. Viz příklad nahoře.<br>Maximální délka: 50 |
| apiVersion | 1.0 |
|||
| Žádost o textu | ŽÁDNÁ |

**Odpověď**:

Nastavit informace HTTP stavový kód: 200

Toto je asynchronní rozhraní API. Zobrazí se Tvůrce dotazů ID jako odpověď. Se dozvíte po ukončení sestavení by měl volání rozhraní API "Získat vytvoří stav modelem" a najděte toto ID tvůrce dotazů v odpovědi. Všimněte si, že sestavení může trvat minut hodin v závislosti na velikosti data.

Nelze používat doporučení pokladny sestavení má na konci.

Stav platné sestavení:

- Vytvoření – Model byl vytvořen.
- Ve frontě – modelu sestavení spuštěná a je ve frontě.
- Vytváření – modelu je vytvářeného.
- Úspěch – buildu ukončena úspěšně.
- Chyba – sestavení ukončí s se nepovede.
- Zrušená – bylo zrušeno Tvůrce dotazů.
- Zrušení – rušení Tvůrce dotazů.


Všimněte si, že ID tvůrce dotazů naleznete v následujícím umístění:`Feed\entry\content\properties\Id`

OData XML

    <feed xmlns:base="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/BuildModel" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
    <title type="text" />
    <subtitle type="text">Build a Model with RequestBody</subtitle>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/BuildModel?modelId='9559872f-7a53-4076-a3c7-19d9385c1265'&amp;userDescription='First build'&amp;apiVersion='1.0'</id>
    <rights type="text" />
    <updated>2014-10-05T08:56:34Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/BuildModel?modelId='9559872f-7a53-4076-a3c7-19d9385c1265'&amp;userDescription='First%20build'&amp;apiVersion='1.0'" />
    <entry>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/BuildModel?modelId='9559872f-7a53-4076-a3c7-19d9385c1265'&amp;userDescription='First build'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
    <title type="text">BuildAModelEntity2</title>
    <updated>2014-10-05T08:56:34Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/BuildModel?modelId='9559872f-7a53-4076-a3c7-19d9385c1265'&amp;userDescription='First%20build'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Id m:type="Edm.String">1000272</d:Id>
        <d:UserName m:type="Edm.String"></d:UserName>
        <d:ModelId m:type="Edm.String">9559872f-7a53-4076-a3c7-19d9385c1265</d:ModelId>
        <d:ModelName m:type="Edm.String">docTest</d:ModelName>
        <d:Type m:type="Edm.String">Recommendation</d:Type>
        <d:CreationTime m:type="Edm.String">2014-10-05T08:56:31.893</d:CreationTime>
        <d:Progress_BuildId m:type="Edm.String">1000272</d:Progress_BuildId>
        <d:Progress_ModelId m:type="Edm.String">9559872f-7a53-4076-a3c7-19d9385c1265</d:Progress_ModelId>
        <d:Progress_UserName m:type="Edm.String">5-4058-ab36-1fe254f05102@dm.com</d:Progress_UserName>
        <d:Progress_IsExecutionStarted m:type="Edm.String">false</d:Progress_IsExecutionStarted>
        <d:Progress_IsExecutionEnded m:type="Edm.String">false</d:Progress_IsExecutionEnded>
        <d:Progress_Percent m:type="Edm.String">0</d:Progress_Percent>
        <d:Progress_StartTime m:type="Edm.String">0001-01-01T00:00:00</d:Progress_StartTime>
        <d:Progress_EndTime m:type="Edm.String">0001-01-01T00:00:00</d:Progress_EndTime>
        <d:Progress_UpdateDateUTC m:type="Edm.String"></d:Progress_UpdateDateUTC>
        <d:Status m:type="Edm.String">Queued</d:Status>
        <d:Key1 m:type="Edm.String">UseFeaturesInModel</d:Key1>
        <d:Value1 m:type="Edm.String">False</d:Value1>
      </m:properties>
    </content>
    </entry>
    </feed>

###<a name="get-build-status-of-a-model"></a>Stav sestavení modelu

| Metoda HTTP | IDENTIFIKÁTOR URI |
|:--------|:--------|
|ZÍSKÁNÍ     |`<rootURI>/GetModelBuildsStatus?modelId=%27<modelId>%27&onlyLastBuild=<bool>&apiVersion=%271.0%27`<br><br>Příklad:<br>`<rootURI>/GetModelBuildsStatus?modelId=%279559872f-7a53-4076-a3c7-19d9385c1265%27&onlyLastBuild=true&apiVersion=%271.0%27`|



|   Název parametru  |   Platné hodnoty                        |
|:--------          |:--------                              |
|   modelId         |   Jedinečný identifikátor modelu (malá a velká písmena)    |
|   onlyLastBuild   |   Označuje, zda chcete vrátit historii sestavení modelu nebo jenom stav na nejnovější verzi. |
|   apiVersion      |   1.0                                 |


**Odpověď**:

Nastavit informace HTTP stavový kód: 200

Odpověď na obsahuje jednu položku za Tvůrce dotazů. Každá položka má následující údaje:

- `feed/entry/content/properties/UserName`– Jméno uživatele.
- `feed/entry/content/properties/ModelName`– Název modelu.
- `feed/entry/content/properties/ModelId`– Modelu jedinečný identifikátor.
- `feed/entry/content/properties/IsDeployed`– Jestli (také nasazení sestavení aktivní sestavení).
- `feed/entry/content/properties/BuildId`– Vytvořte jedinečný identifikátor.
- `feed/entry/content/properties/BuildType`– Typ sestavení.
- `feed/entry/content/properties/Status`– Vytvoření stav. Může být jeden z těchto věcí: Chyba budovy, ve frontě, Cancelling, zrušeno, úspěch
- `feed/entry/content/properties/StatusMessage`– Zpráva podrobný stav (platí jenom pro konkrétní států).
- `feed/entry/content/properties/Progress`– Vytvoření průběh (%).
- `feed/entry/content/properties/StartTime`– Vytvoření počáteční čas.
- `feed/entry/content/properties/EndTime`– Vytvoření koncového času.
- `feed/entry/content/properties/ExecutionTime`– Vytvoření doby trvání.
- `feed/entry/content/properties/ProgressStep`– Podrobnosti o aktuální dílčí fáze, která je vytvořit v průběhu v.

Stav platné sestavení:
- Vytvořili – vytvořit žádost o byla vytvořena.
- Ve frontě – spouštěný žádost Tvůrce dotazů a je ve frontě.
- Probíhá stavební – Tvůrce dotazů.
- Úspěch – buildu ukončena úspěšně.
- Chyba – sestavení ukončí s se nepovede.
- Zrušená – bylo zrušeno Tvůrce dotazů.
- Zrušení – rušení Tvůrce dotazů.

Platné hodnoty pro typ Tvůrce dotazů:
- Pořadí – vytvoření pořadí. (Pořadí vytvořit podrobnosti, získáte "Počítače výukové doporučení rozhraní API přečtěte následující dokumentaci pro" dokumentu.)
- Doporučení: sestavení doporučení.
- Fbt – často si koupili společně Tvůrce dotazů.

OData XML

    <feed xmlns:base="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/GetModelBuildsStatus" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
    <title type="text" />
    <subtitle type="text">Get builds status of a model</subtitle>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/GetModelBuildsStatus?modelId='1d20c34f-dca1-4eac-8e5d-f299e4e4ad66'&amp;onlyLastBuild=False&amp;apiVersion='1.0'</id>
    <rights type="text" />
    <updated>2014-11-05T17:51:10Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/GetModelBuildsStatus?modelId='1d20c34f-dca1-4eac-8e5d-f299e4e4ad66'&amp;onlyLastBuild=False&amp;apiVersion='1.0'" />
    <entry>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/GetModelBuildsStatus?modelId='1d20c34f-dca1-4eac-8e5d-f299e4e4ad66'&amp;onlyLastBuild=False&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
    <title type="text">GetBuildsStatusEntity</title>
    <updated>2014-11-05T17:51:10Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/GetModelBuildsStatus?modelId='1d20c34f-dca1-4eac-8e5d-f299e4e4ad66'&amp;onlyLastBuild=False&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:UserName m:type="Edm.String">b-434e-b2c9-84935664ff20@dm.com</d:UserName>
        <d:ModelName m:type="Edm.String">ModelName</d:ModelName>
        <d:ModelId m:type="Edm.String">1d20c34f-dca1-4eac-8e5d-f299e4e4ad66</d:ModelId>
        <d:IsDeployed m:type="Edm.String">true</d:IsDeployed>
        <d:BuildId m:type="Edm.String">1000272</d:BuildId>
        <d:BuildType m:type="Edm.String">Recommendation</d:BuildType>
        <d:Status m:type="Edm.String">Success</d:Status>
        <d:StatusMessage m:type="Edm.String"></d:StatusMessage>
        <d:Progress m:type="Edm.String">0</d:Progress>
        <d:StartTime m:type="Edm.String">2014-11-02T13:43:51</d:StartTime>
        <d:EndTime m:type="Edm.String">2014-11-02T13:45:10</d:EndTime>
        <d:ExecutionTime m:type="Edm.String">00:01:19</d:ExecutionTime>
        <d:IsExecutionStarted m:type="Edm.String">false</d:IsExecutionStarted>
        <d:ProgressStep m:type="Edm.String"></d:ProgressStep>
      </m:properties>
     </content>
    </entry>
    </feed>


###<a name="get-recommendations"></a>Získat doporučení

| Metoda HTTP | IDENTIFIKÁTOR URI |
|:--------|:--------|
|ZÍSKÁNÍ     |`<rootURI>/ItemRecommend?modelId=%27<modelId>%27&itemIds=%27<itemId>%27&numberOfResults=<int>&includeMetadata=<bool>&apiVersion=%271.0%27`<br><br>Příklad:<br>`<rootURI>/ItemRecommend?modelId=%272779c063-48fb-46c1-bae3-74acddc8c1d1%27&itemIds=%271003%27&numberOfResults=10&includeMetadata=false&apiVersion=%271.0%27`|



|   Název parametru  |   Platné hodnoty                        |
|:--------          |:--------                              |
| modelId | Jedinečný identifikátor modelu (malá a velká písmena) |
| položky ItemID | Hodnoty oddělené seznam položky, které chcete doporučit.<br>Maximální délka: 1 024 |
| numberOfResults | Počet požadované výsledky |
| includeMetatadata | Další použití, vždy false |
| apiVersion | 1.0 |

**Odpověď:**

Nastavit informace HTTP stavový kód: 200

Odpověď na obsahuje jednu položku za doporučené položky. Každá položka má následující údaje:

- `Feed\entry\content\properties\Id`– ID Doporučené položky.
- `Feed\entry\content\properties\Name`– Název položky.
- `Feed\entry\content\properties\Rating`– Hodnocení doporučení; vyšší čísla znamená vyšší spolehlivosti.
- `Feed\entry\content\properties\Reasoning`-Doporučení důvody (například doporučení vysvětlení).

OData XML

Následující příklad odpověď obsahuje 10 Doporučené položky:

    <feed xmlns:base="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
    <title type="text" />
     <subtitle type="text">Get Recommendation</subtitle>
     <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'</id>
    <rights type="text" />
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'" />
    <entry>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
    <title type="text">GetRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Id m:type="Edm.String">159</d:Id>
        <d:Name m:type="Edm.String">159</d:Name>
        <d:Rating m:type="Edm.Double">0.543343480387708</d:Rating>
        <d:Reasoning m:type="Edm.String">People who like '1003' also like '159'</d:Reasoning>
      </m:properties>
    </content>
     </entry>
     <entry>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=1&amp;$top=1</id>
    <title type="text">GetRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=1&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Id m:type="Edm.String">52</d:Id>
        <d:Name m:type="Edm.String">52</d:Name>
        <d:Rating m:type="Edm.Double">0.539588900748721</d:Rating>
        <d:Reasoning m:type="Edm.String">People who like '1003' also like '52'</d:Reasoning>
      </m:properties>
    </content>
     </entry>
     <entry>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=2&amp;$top=1</id>
    <title type="text">GetRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=2&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Id m:type="Edm.String">35</d:Id>
        <d:Name m:type="Edm.String">35</d:Name>
        <d:Rating m:type="Edm.Double">0.53842946443853</d:Rating>
        <d:Reasoning m:type="Edm.String">People who like '1003' also like '35'</d:Reasoning>
      </m:properties>
    </content>
     </entry>
     <entry>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=3&amp;$top=1</id>
    <title type="text">GetRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=3&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Id m:type="Edm.String">124</d:Id>
        <d:Name m:type="Edm.String">124</d:Name>
        <d:Rating m:type="Edm.Double">0.536712832792886</d:Rating>
        <d:Reasoning m:type="Edm.String">People who like '1003' also like '124'</d:Reasoning>
      </m:properties>
    </content>
    </entry>
     <entry>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=4&amp;$top=1</id>
    <title type="text">GetRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=4&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Id m:type="Edm.String">120</d:Id>
        <d:Name m:type="Edm.String">120</d:Name>
        <d:Rating m:type="Edm.Double">0.533673023762878</d:Rating>
        <d:Reasoning m:type="Edm.String">People who like '1003' also like '120'</d:Reasoning>
      </m:properties>
    </content>
     </entry>
     <entry>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=5&amp;$top=1</id>
    <title type="text">GetRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=5&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Id m:type="Edm.String">96</d:Id>
        <d:Name m:type="Edm.String">96</d:Name>
        <d:Rating m:type="Edm.Double">0.532544826370521</d:Rating>
        <d:Reasoning m:type="Edm.String">People who like '1003' also like '96'</d:Reasoning>
      </m:properties>
    </content>
    </entry>
     <entry>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=6&amp;$top=1</id>
    <title type="text">GetRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=6&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Id m:type="Edm.String">69</d:Id>
        <d:Name m:type="Edm.String">69</d:Name>
        <d:Rating m:type="Edm.Double">0.531678607847759</d:Rating>
        <d:Reasoning m:type="Edm.String">People who like '1003' also like '69'</d:Reasoning>
      </m:properties>
    </content>
     </entry>
     <entry>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=7&amp;$top=1</id>
    <title type="text">GetRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=7&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Id m:type="Edm.String">172</d:Id>
        <d:Name m:type="Edm.String">172</d:Name>
        <d:Rating m:type="Edm.Double">0.530957821375951</d:Rating>
        <d:Reasoning m:type="Edm.String">People who like '1003' also like '172'</d:Reasoning>
      </m:properties>
    </content>
     </entry>
     <entry>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=8&amp;$top=1</id>
    <title type="text">GetRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=8&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Id m:type="Edm.String">155</d:Id>
        <d:Name m:type="Edm.String">155</d:Name>
        <d:Rating m:type="Edm.Double">0.529093541481333</d:Rating>
        <d:Reasoning m:type="Edm.String">People who like '1003' also like '155'</d:Reasoning>
      </m:properties>
    </content>
     </entry>
     <entry>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=9&amp;$top=1</id>
    <title type="text">GetRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=9&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Id m:type="Edm.String">32</d:Id>
        <d:Name m:type="Edm.String">32</d:Name>
        <d:Rating m:type="Edm.Double">0.528917978168322</d:Rating>
        <d:Reasoning m:type="Edm.String">People who like '1003' also like '32'</d:Reasoning>
      </m:properties>
    </content>
     </entry>
    </feed>

###<a name="update-model"></a>Aktualizace modelu
Můžete aktualizovat popis modelu nebo ID aktivní Tvůrce dotazů.
*Aktivní sestavení ID* – každý vytvořit pro každý model obsahuje ID tvůrce dotazů. ID aktivního sestavení je první úspěšném sestavení každý nový model. Jakmile máte ID aktivní Tvůrce dotazů a máte další vytvoří pro stejný model, budete muset výslovně nastavené jako výchozí sestavení ID Pokud chcete. Při používání doporučení, pokud nezadáte sestavení ID, který chcete použít, použijí se automaticky výchozí.

Tento postup lze - až budete mít modelu doporučení na výroba - k vytvoření nové modely a otestujete před podporu výroby.

| Metoda HTTP | IDENTIFIKÁTOR URI |
|:--------|:--------|
|UMÍSTĚNÍ     |`<rootURI>/UpdateModel?id=%27<modelId>%27&apiVersion=%271.0%27`<br><br>Příklad:<br>`<rootURI>/UpdateModel?id=%279559872f-7a53-4076-a3c7-19d9385c1265%27&apiVersion=%271.0%27`|


|   Název parametru  |   Platné hodnoty                        |
|:--------          |:--------                              |
| ID | Jedinečný identifikátor modelu (malá a velká písmena) |
| apiVersion | 1.0 |
|||
| Žádost o textu | `<ModelUpdateParams xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">`<br>`   <Description>New Description</Description>`<br>`          <ActiveBuildId>-1</ActiveBuildId>`<br>`</ModelUpdateParams>`<br><br>Všimněte si, že jsou značky XML popis a ActiveBuildId nepovinné. Pokud nechcete nastavit popis nebo ActiveBuildId, odeberte celou značku. |

**Odpověď**:

Nastavit informace HTTP stavový kód: 200

OData XML

    <feed xmlns:base="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/UpdateModel" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
    <title type="text" />
    <subtitle type="text">Update an Existing Model</subtitle>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/UpdateModel?id='9559872f-7a53-4076-a3c7-19d9385c1265'&amp;apiVersion='1.0'</id>
    <rights type="text" />
     <updated>2014-10-05T10:27:17Z</updated>
     <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/UpdateModel?id='9559872f-7a53-4076-a3c7-19d9385c1265'&amp;apiVersion='1.0'" />
    </feed>

##<a name="legal"></a>Právní
Tento dokument je k dispozici "jako-je". Informace a názory uvedené v tomto dokumentu, včetně URL adresy a dalších odkazů na webu Internet, mohou změnit bez předchozího upozornění. Některé zde uvedené příklady jsou k dispozici pouze ilustrace a jsou fiktivní. Žádné skutečné přidružení připojení je určená nebo událostmi. Tento dokument vám neposkytuje s všechny práva duševního vlastnictví jakéhokoli produktu společnosti Microsoft. Můžete zkopírovat a použijte tento dokument pro svou interní potřebu jako referenci. © 2014 společnosti Microsoft. Microsoft Corporation. 
 
