<properties 
    pageTitle="Výukové doporučení rozhraní API si přečtěte následující dokumentaci pro počítač | Microsoft Azure" 
    description="Azure počítače výukové doporučení API si přečtěte následující dokumentaci pro doporučení modul dostupné na webu Microsoft Azure Marketplace." 
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
    ms.author="LuisCa"/>

#<a name="azure-machine-learning-recommendations-api-documentation"></a>Azure počítače výukové si přečtěte následující dokumentaci doporučení rozhraní API

>[AZURE.NOTE]Měli byste začít používat službu kognitivní rozhraní API doporučení místo tato verze. Kognitivní službu doporučení se nahradí tuto službu a s novými funkcemi bude vyvinuté tam. Má nové funkce jako dávky podpory lepší rozhraní API aplikace Explorer, opět dostanete čistší prostředí povrchový, jednotnější registrace/fakturace rozhraní API, atd.
> Další informace o [migraci do nové kognitivní služby](http://aka.ms/recomigrate)

Tento dokument znázorňuje Microsoft Azure počítače výukové doporučení API.


[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

##<a name="1-general-overview"></a>1 celkového přehledu
Tento dokument je odkaz na rozhraní API. By měly začít s ním "Azure počítače výukové doporučení – rychlý Start".

Rozhraní API Azure počítače výukové doporučení možné rozdělit následujícím logických skupinách:

- <ins>Omezení</ins> - rozhraní API doporučení omezení.
- <ins>Obecné informace</ins> – informace o ověřování a služby URI správy verzí.
- <ins>Model základní</ins> - rozhraní API, která umožňují základních operací na modelu (například vytvořit, aktualizovat a odstraňovat modelu).
- <ins>Model Upřesnit</ins> - rozhraní API, která umožňují získat upřesnění interpretaci dat v modelu.
- <ins>Model obchodní pravidla</ins> - rozhraní API, která vám umožní spravovat obchodní pravidla na stránce výsledků doporučení modelu.
- <ins>Katalog</ins> - rozhraní API, která vám umožňují provádět základní operace s katalogem modelu. Katalog obsahuje metadata informace o použití datové položky.
- <ins>Funkce</ins> - rozhraní API, která umožní získat další informace na položky do katalogu a jak se tyto informace použít k vytvoření lepšího doporučení.
- <ins>Použití zásad správy informací</ins> – rozhraní API, která vám umožňují provádět základní operace na použití datového modelu. Údaje o využití ve formuláři základní obsahuje řádky, které obsahují párů #60; ID uživatele & #62; & #60; itemId & #62;.
- <ins>Vytvoření</ins> - rozhraní API, která vám umožní aktivační událost modelu Tvůrce dotazů a proveďte základní operace, které se vztahují k této Tvůrce dotazů. Vytvoření modelu můžete aktivovat, až budete mít cenný použití zásad správy informací.
- <ins>Doporučení</ins> - rozhraní API, která umožňují využívat doporučení po skončení modelu sestavování.
- <ins>Uživatelská Data</ins> – rozhraní API, která umožňuje načítat informace na kartě data použití uživatele.
- <ins>Oznámení</ins> - rozhraní API, která vám umožní přijímat oznámení o problémech souvisejících s rozhraním API operace. (Například vykazujete použití zásad správy informací pomocí získávání dat a většinu událostí se nedaří zpracování. Oznámení o chybě se zvýší.)

##<a name="2-limitations"></a>2. omezení

- Model na jedno předplatné maximálně 10.
- Maximální počet buildy na model je 20.
- Maximální počet položek, které můžou obsahovat katalog je 100,000.
- Maximální počet bodů použití, které jsou k dispozici je 5 000 000 ~. Nejstarší se odstraní Pokud nové bude nahráli nebo vykázaného.
- Maximální velikosti doručovaných data, která mohou být odeslány do příspěvku (například import katalogu dat, import o využití) je 200MB.
- Maximální počet položek, které se můžou objevit výzva k získání doporučení po 150.

##<a name="3-apis---general-information"></a>3. rozhraní API – obecné informace

###<a name="31-authentication"></a>3.1. Ověřování
Postupujte podle webu Microsoft Azure Marketplace pokyny týkající se ověřování. Tržiště podporuje základní nebo OAuth metody ověřování.

###<a name="32-service-uri"></a>3,2. Služba URI
Odmocnina služby URI pro rozhraní API, doporučení výukové Azure počítače se [zde.](https://api.datamarket.azure.com/amla/recommendations/v3/)

Kompletní URI vyjádřena pomocí prvků specifikaci OData.  

###<a name="33-api-version"></a>3.3. Rozhraní API verze
Rozhraní API volání bude mít na konci, k dotazu parametr s názvem apiVersion nastavené 1.0.

###<a name="34-ids-are-case-sensitive"></a>3.4. ID jsou malá a velká písmena
ID vrácené jednotlivých rozhraní API se rozlišují velká a bude použito jako takové při předávání jako parametrů v další hovory rozhraní API. Například modelu ID a katalogu ID jsou malá a velká písmena.

##<a name="4-recommendations-quality-and-cold-items"></a>4. doporučení kvality a studenou položek

###<a name="41-recommendation-quality"></a>4.1. Zlepšení kvality doporučení

Vytvoření modelu doporučení bývá dost umožňuje systému poskytnout doporučení. Však doporučení kvality se liší v závislosti na použití zpracování a pokrytí katalogu. Třeba když máte před sebou hodně studenou položek (položky bez významné použití), systému bude mít potíže poskytující doporučení pro tyto položky, nebo používají tyto položky některou doporučené. Abyste mohli studenou položku problém vyřešit, systému umožňuje používat metadat položek k vylepšení doporučení. Tato metadata je uvedená jako funkce. Typické funkce jsou knihy Autor nebo filmového actor. Funkce jsou k dispozici prostřednictvím katalogu v podobě řetězců klíč/hodnota. Úplné formát souboru katalogu naleznete v [katalogu oddíl importu](#81-import-catalog-data). 

###<a name="42-rank-build"></a>4.2. RANK Tvůrce dotazů

Funkce můžete být díky dokonalejšímu modelu doporučení, ale k tomu nevyzve vyžaduje použití smysluplného funkcí. K tomuto účelu nové sestavení byla zavedená – rank Tvůrce dotazů. Toto sestavení bude pořadí použitelnost funkce. Význam funkce je funkci se skóre pořadí čísla 2 a nahoru.
Po principy, které funkce jsou smysluplného, aktivace doporučení sestavit seznam (nebo dílčí seznam) smysluplného funkce. Je možné použít tyto funkce pro zvýšení teplé položek a studenou položky. Mohli použít u teplé položek `UseFeatureInModel` by měl být vytvořit parametr nastaven. K použití funkce studenou položek `AllowColdItemPlacement` používat vytvořit parametr.
Poznámka: Není možné povolit `AllowColdItemPlacement` bez povolení `UseFeatureInModel`.

###<a name="43-recommendation-reasoning"></a>4.3. Důvody doporučení

Důvody doporučení je další aspekty aplikace použití funkce. Nakonec modul Azure počítače výukové doporučení pomocí funkcí (také zadat vysvětlení doporučení důvody), vést k další důvěru k položce doporučené od příjemce doporučení.
Chcete-li povolit důvody, `AllowFeatureCorrelation` a `ReasoningFeatureList` parametry by měl být nastavení před žádosti o doporučení Tvůrce dotazů.


##<a name="5-model-basic"></a>5. základní modelu

###<a name="51-create-model"></a>5.1. Vytvoření modelu
Vytvoří žádost o "Vytvoření modelu".

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

    <feed xmlns:base="https://api.datamarket.azure.com/amla/recommendations/v3/CreateModel" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
      <title type="text" />
      <subtitle type="text">Create A New Model</subtitle>
      <id>https://api.datamarket.azure.com/amla/recommendations/v3/CreateModel?modelName='MyFirstModel'&amp;apiVersion='1.0'</id>
      <rights type="text" />
      <updated>2014-10-05T06:35:21Z</updated>
     <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/CreateModel?modelName='MyFirstModel'&amp;apiVersion='1.0'" />
      <entry>
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/CreateModel?modelName='MyFirstModel'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
    <title type="text">CreateANewModelEntity2</title>
    <updated>2014-10-05T06:35:21Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/CreateModel?modelName='MyFirstModel'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
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

###<a name="52-get-model"></a>5,2. Získání modelu
Vytvoří žádost o "get modelu".

| Metoda HTTP | IDENTIFIKÁTOR URI |
|:--------|:--------|
|ZÍSKÁNÍ     |`<rootURI>/GetModel?id=%27<model_id>%27&apiVersion=%271.0%27`<br>Příklad:<br>`<rootURI>/GetModel?id=%271cac7b76-def4-41f1-bc81-29b806adb1de%27&apiVersion=%271.0%27`|

|   Název parametru  |   Platné hodnoty                        |
|:--------          |:--------                              |
|   ID  |   Jedinečný identifikátor modelu (malá a velká písmena) |
|   apiVersion      | 1.0 |
|||
| Žádost o textu | ŽÁDNÁ |

**Odpověď**:

Nastavit informace HTTP stavový kód: 200

Datového modelu najdete v části tyto prvky:

- `feed/entry/content/properties/Id`– Jedinečné ID modelu.
- `feed/entry/content/properties/Name`-Název modelu.
- `feed/entry/content/properties/Date`-Datum vytvoření modelu.
- `feed/entry/content/properties/Status`-Model stav. Jedna z následujících akcí:
    - Vytvořené - modelu je a neobsahuje katalogu a použití.
    - ReadyForBuild - modelu se vytvoří a obsahuje katalog a použití.
- `feed/entry/content/properties/HasActiveBuild`-Označuje, pokud bylo úspěšné vytvoření modelu.
- `feed/entry/content/properties/BuildId`– ID modelu aktivní Tvůrce dotazů.
- `feed/entry/content/properties/Mpr`-Model řazení střed_hodn percentil (MPR – Další informace najdete v tématu ModelInsight).
- `feed/entry/content/properties/UserName`-Modelu interní uživatelské jméno.

OData XML

    <feed xmlns:base="https://api.datamarket.azure.com/amla/recommendations/v3/GetAllModels" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
      <title type="text" />
      <subtitle type="text">Get A List Of All Models</subtitle>
      <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetAllModels?apiVersion='1.0'</id>
      <rights type="text" />
      <updated>2014-10-28T14:35:51Z</updated>
     <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetAllModels?apiVersion='1.0'" />
      <entry>
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetAllModels?apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
    <title type="text">GetAListOfAllModelsEntity</title>
    <updated>2014-10-28T14:35:51Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetAllModels?apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Id m:type="Edm.String">68b232f2-1037-45f7-8f49-af822695ee63</d:Id>
        <d:Name m:type="Edm.String">vah-11111</d:Name>
        <d:Date m:type="Edm.String">10/28/2014 2:16:07 PM</d:Date>
        <d:Status m:type="Edm.String">ReadyForBuild</d:Status>
        <d:HasActiveBuild m:type="Edm.String">false</d:HasActiveBuild>
        <d:BuildId m:type="Edm.String">-1</d:BuildId>
        <d:Mpr m:type="Edm.String">0</d:Mpr>
        <d:UsageFileNames m:type="Edm.String">ImplicitMatrix10_Guid_small.txt, ImplicitMatrix11_Guid_small.txt</d:UsageFileNames>
        <d:CatalogId m:type="Edm.String">626babdb-cab6-43a6-82ef-4fb86345700c</d:CatalogId>
        <d:UserName m:type="Edm.String">89dc4722-03ba-4f90-8821-b1db388408b5@dm.com</d:UserName>
        <d:Description m:type="Edm.String">short description</d:Description>
        <d:CatalogFileName m:type="Edm.String">catalog10_small.txt</d:CatalogFileName>
      </m:properties>
    </content>
      </entry>
    </feed>

###<a name="53-get-all-models"></a>5.3. Zobrazí všechny modely
Vyhledá všechny modely aktuálního uživatele.

| Metoda HTTP | IDENTIFIKÁTOR URI |
|:--------|:--------|
|ZÍSKÁNÍ     |`<rootURI>/GetAllModels?apiVersion=%271.0%27`<br>Příklad:<br>`<rootURI>/GetAllModels?apiVersion=%271.0%27`|

|   Název parametru  |   Platné hodnoty                        |
|:--------          |:--------                              |
|   apiVersion      | 1.0 |
|||
| Žádost o textu | ŽÁDNÁ |

**Odpověď**:

Nastavit informace HTTP stavový kód: 200

- `feed/entry/content/properties/Id`– Jedinečné ID modelu.
- `feed/entry/content/properties/Name`-Název modelu.
- `feed/entry/content/properties/Date`-Datum vytvoření modelu.
- `feed/entry/content/properties/Status`-Model stav. Jedna z následujících akcí:
  - Vytvořené - modelu je a neobsahuje katalogu a použití.
  - ReadyForBuild - modelu se vytvoří a obsahuje katalog a použití.
- `feed/entry/content/properties/HasActiveBuild`-Označuje, pokud bylo úspěšné vytvoření modelu.
- `feed/entry/content/properties/BuildId`– ID modelu aktivní Tvůrce dotazů.
- `feed/entry/content/properties/Mpr`-Model MPR (Další informace najdete v tématu ModelInsight).
- `feed/entry/content/properties/UserName`-Modelu interní uživatelské jméno.
- `feed/entry/content/properties/UsageFileNames`– Seznam souborů použití modelu oddělené čárkou.
- `feed/entry/content/properties/CatalogId`– ID modelu katalogu.
- `feed/entry/content/properties/Description`-Model popis.
- `feed/entry/content/properties/CatalogFileName`-Název souboru katalogu modelu.

OData XML


    <feed xmlns:base="https://api.datamarket.azure.com/amla/recommendations/v3/GetAllModels" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
        <title type="text" />
        <subtitle type="text">Get A List Of All Models</subtitle>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetAllModels?apiVersion='1.0'</id>
        <rights type="text" />
        <updated>2014-10-28T14:35:51Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetAllModels?apiVersion='1.0'" />
        <entry>
            <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetAllModels?apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
            <title type="text">GetAListOfAllModelsEntity</title>
    <updated>2014-10-28T14:35:51Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetAllModels?apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
            <content type="application/xml">
                <m:properties>
                    <d:Id m:type="Edm.String">68b232f2-1037-45f7-8f49-af822695ee63</d:Id>
                    <d:Name m:type="Edm.String">vah-11111</d:Name>
                    <d:Date m:type="Edm.String">10/28/2014 2:16:07 PM</d:Date>
                    <d:Status m:type="Edm.String">ReadyForBuild</d:Status>
                    <d:HasActiveBuild m:type="Edm.String">false</d:HasActiveBuild>
                    <d:BuildId m:type="Edm.String">-1</d:BuildId>
                    <d:Mpr m:type="Edm.String">0</d:Mpr>
                    <d:UsageFileNames m:type="Edm.String">ImplicitMatrix10_Guid_small.txt, ImplicitMatrix11_Guid_small.txt</d:UsageFileNames>
                    <d:CatalogId m:type="Edm.String">626babdb-cab6-43a6-82ef-4fb86345700c</d:CatalogId>
                    <d:UserName m:type="Edm.String">89dc4722-03ba-4f90-8821-b1db388408b5@dm.com</d:UserName>
                    <d:Description m:type="Edm.String">short description</d:Description>
                    <d:CatalogFileName m:type="Edm.String">catalog10_small.txt</d:CatalogFileName>
                </m:properties>
            </content>
        </entry>
    </feed>

###<a name="54-update-model"></a>5.4. Aktualizace modelu

Můžete aktualizovat popis modelu nebo ID aktivní Tvůrce dotazů.<br>
<ins>Aktivní sestavení ID</ins> – každý vytvořit pro každý model obsahuje ID tvůrce dotazů. ID aktivního sestavení je první úspěšném sestavení každý nový model. Jakmile máte ID aktivní Tvůrce dotazů a máte další vytvoří pro stejný model, budete muset výslovně nastavené jako výchozí sestavení ID Pokud chcete. Při používání doporučení, pokud nezadáte sestavení ID, který chcete použít, použijí se automaticky výchozí.<br>
Tento postup lze - až budete mít modelu doporučení na výroba - k vytvoření nové modely a otestujete před podporu výroby.


| Metoda HTTP | IDENTIFIKÁTOR URI |
|:--------|:--------|
|UMÍSTĚNÍ     |`<rootURI>/UpdateModel?id=%27<modelId>%27&apiVersion=%271.0%27`<br>Příklad:<br>`<rootURI>/UpdateModel?id=%279559872f-7a53-4076-a3c7-19d9385c1265%27&apiVersion=%271.0%27`|

|   Název parametru  |   Platné hodnoty                        |
|:--------          |:--------                              |
|   ID      | Jedinečný identifikátor modelu (malá a velká písmena)  |
|   apiVersion      | 1.0 |
|||
| Žádost o textu | `<ModelUpdateParams xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">`<br>`<Description>New Description</Description>`<br>`<ActiveBuildId>-1</ActiveBuildId>`<br>` </ModelUpdateParams>`<br><br>Všimněte si, že jsou značky XML popis a ActiveBuildId nepovinné. Pokud nechcete nastavit popis nebo ActiveBuildId, odeberte celou značku.|

**Odpověď**:

Nastavit informace HTTP stavový kód: 200

###<a name="55-delete-model"></a>5.5. Odstranění modelu
Odstraní existujícího modelu pomocí ID.

| Metoda HTTP | IDENTIFIKÁTOR URI |
|:--------|:--------|
|ODSTRANĚNÍ     |`<rootURI>/DeleteModel?id=%27<model_id>%27&apiVersion=%271.0%27`<br>Příklad:<br>`<rootURI>/DeleteModel?id=%271cac7b76-def4-41f1-bc81-29b806adb1de%27&apiVersion=%271.0%27`|

|   Název parametru  |   Platné hodnoty                        |
|:--------          |:--------                              |
|   ID  |   Jedinečný identifikátor modelu (malá a velká písmena) |
|   apiVersion      | 1.0 |
|||
| Žádost o textu | ŽÁDNÁ |

**Odpověď**:

Nastavit informace HTTP stavový kód: 200

OData XML

    <feed xmlns:base="https://api.datamarket.azure.com/amla/recommendations/v3/DeleteModel" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
      <title type="text" />
      <subtitle type="text">Delete Model by Id</subtitle>
      <id>https://api.datamarket.azure.com/amla/recommendations/v3/DeleteModel?id='1cac7b76-def4-41f1-bc81-29b806adb1de'&amp;apiVersion='1.0'</id>
      <rights type="text" />
      <updated>2014-10-28T10:39:33Z</updated>
     <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/DeleteModel?id='1cac7b76-def4-41f1-bc81-29b806adb1de'&amp;apiVersion='1.0'" />
      <entry>
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/DeleteModel?id='1cac7b76-def4-41f1-bc81-29b806adb1de'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
    <title type="text">DeleteModelByIdEntity</title>
    <updated>2014-10-28T10:39:33Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/DeleteModel?id='1cac7b76-def4-41f1-bc81-29b806adb1de'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:string m:type="Edm.String"></d:string>
      </m:properties>
    </content>
      </entry>
    </feed>

##<a name="6-model-advanced"></a>6. rozšířené možnosti modelu

###<a name="61-model-data-insight"></a>6.1. Přehled datového modelu
Vrátí statistických údajů na použití data, která byla vytvořena tento model.

K dispozici pouze pro sestavení doporučení.

| Metoda HTTP | IDENTIFIKÁTOR URI |
|:--------|:--------|
|ZÍSKÁNÍ     |`<rootURI>/GetDataInsight?modelId=%27<model_id>%27&apiVersion=%271.0%27`<br>Příklad:<br>`<rootURI>/GetDataInsight?modelId=%271cac7b76-def4-41f1-bc81-29b806adb1de%27&apiVersion=%271.0%27`|

|   Název parametru  |   Platné hodnoty                        |
|:--------          |:--------                              |
|   modelId |   Jedinečný identifikátor modelu |
|   apiVersion      | 1.0 |
|||
| Žádost o textu | ŽÁDNÁ |

**Odpověď**:

Nastavit informace HTTP stavový kód: 200

Data je vrácené jako sada vlastností.

- `feed/entry/id/content/properties/key`-Obsahuje název vlastnosti.
- `feed/entry/id/content/properties/value`-Obsahuje hodnotu.

Následující tabulka popisuje hodnotu, která představuje každý klíč.

|Klíč|Popis|
|:-----|:----|
| AvgItemLength | Průměrný počet různých uživatelů na položku. |
| AvgUserLength | Průměrný počet jedinečných položek jednotlivé uživatele. |
| DensificationNumberOfItems | Počet položek po vymazání položky, které nelze vycházet. |
| DensificationNumberOfUsers | Počet bodů použití po vymazání uživatelů a položky, které nelze vycházet. |
| DensificationNumberOfRecords | Počet bodů použití po vymazání uživatelů a položky, které nelze vycházet. |
| MaxItemLength | Počet jedinečných uživatelů Nejoblíbenější položky. |
| MaxUserLength | Maximální počet jedinečných položek pro uživatele. |
| MinItemLength | Maximální počet jedinečných uživatelů pro položku. |
| MinUserLength | Minimální počet jedinečných položek pro uživatele. |
| RawNumberOfItems | Počet položek v seznamu soubory použití. |
| RawNumberOfUsers | Počet bodů použití před všechny vyřazování. |
| RawNumberOfRecords | Počet bodů použití před všechny vyřazování. |
| SamplingNumberOfItems | NENÍ K DISPOZICI |
| SamplingNumberOfRecords | NENÍ K DISPOZICI |
| SamplingNumberOfUsers | NENÍ K DISPOZICI |

OData XML

    <feed xmlns:base="https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
    <title type="text" />
    <subtitle type="text">Get data insight statistics</subtitle>
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'</id>
    <rights type="text" />
    <updated>2014-10-27T14:21:21Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'" />
    <entry>
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
    <title type="text">GetDataInsightStatisticsEntity</title>
    <updated>2014-10-27T14:21:21Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Key m:type="Edm.String">AvgItemLength</d:Key>
        <d:Value m:type="Edm.String">18.936</d:Value>
      </m:properties>
    </content>
    </entry>
    <entry>
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=1&amp;$top=1</id>
    <title type="text">GetDataInsightStatisticsEntity</title>
    <updated>2014-10-27T14:21:21Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=1&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Key m:type="Edm.String">AvgUserLength</d:Key>
        <d:Value m:type="Edm.String">51.879</d:Value>
      </m:properties>
    </content>
    </entry>
    <entry>
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=2&amp;$top=1</id>
    <title type="text">GetDataInsightStatisticsEntity</title>
    <updated>2014-10-27T14:21:21Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=2&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Key m:type="Edm.String">DensificationNumberOfItems</d:Key>
        <d:Value m:type="Edm.String">2,000</d:Value>
      </m:properties>
    </content>
    </entry>
    <entry>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=3&amp;$top=1</id>
    <title type="text">GetDataInsightStatisticsEntity</title>
    <updated>2014-10-27T14:21:21Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=3&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Key m:type="Edm.String">DensificationNumberOfRecords</d:Key>
        <d:Value m:type="Edm.String">37,872</d:Value>
      </m:properties>
    </content>
    </entry>
    <entry>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=4&amp;$top=1</id>
    <title type="text">GetDataInsightStatisticsEntity</title>
    <updated>2014-10-27T14:21:21Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=4&amp;$top=1" />
    <content type="application/xml">
        <m:properties>
            <d:Key m:type="Edm.String">DensificationNumberOfUsers</d:Key>
            <d:Value m:type="Edm.String">730</d:Value>
      </m:properties>
    </content>
    </entry>
    <entry>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=5&amp;$top=1</id>
        <title type="text">GetDataInsightStatisticsEntity</title>
        <updated>2014-10-27T14:21:21Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=5&amp;$top=1" />
        <content type="application/xml">
            <m:properties>
                <d:Key m:type="Edm.String">MaxItemLength</d:Key>
                <d:Value m:type="Edm.String">4,704</d:Value>
            </m:properties>
        </content>
    </entry>
    <entry>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=6&amp;$top=1</id>
        <title type="text">GetDataInsightStatisticsEntity</title>
        <updated>2014-10-27T14:21:21Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=6&amp;$top=1" />
        <content type="application/xml">
            <m:properties>
                <d:Key m:type="Edm.String">MaxUserLength</d:Key>
                <d:Value m:type="Edm.String">190</d:Value>
            </m:properties>
        </content>
    </entry>
    <entry>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=7&amp;$top=1</id>
        <title type="text">GetDataInsightStatisticsEntity</title>
        <updated>2014-10-27T14:21:21Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=7&amp;$top=1" />
        <content type="application/xml">
            <m:properties>
                <d:Key m:type="Edm.String">MinItemLength</d:Key>
                <d:Value m:type="Edm.String">8</d:Value>
            </m:properties>
        </content>
    </entry>
    <entry>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=8&amp;$top=1</id>
        <title type="text">GetDataInsightStatisticsEntity</title>
        <updated>2014-10-27T14:21:21Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=8&amp;$top=1" />
        <content type="application/xml">
            <m:properties>
                <d:Key m:type="Edm.String">MinUserLength</d:Key>
                <d:Value m:type="Edm.String">20</d:Value>
            </m:properties>
        </content>
    </entry>
    <entry>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=9&amp;$top=1</id>
        <title type="text">GetDataInsightStatisticsEntity</title>
        <updated>2014-10-27T14:21:21Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=9&amp;$top=1" />
        <content type="application/xml">
            <m:properties>
                <d:Key m:type="Edm.String">RawNumberOfItems</d:Key>
                <d:Value m:type="Edm.String">2,000</d:Value>
            </m:properties>
        </content>
    </entry>
    <entry>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=10&amp;$top=1</id>
        <title type="text">GetDataInsightStatisticsEntity</title>
        <updated>2014-10-27T14:21:21Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=10&amp;$top=1" />
        <content type="application/xml">
            <m:properties>
                <d:Key m:type="Edm.String">RawNumberOfRecords</d:Key>
                <d:Value m:type="Edm.String">72,022</d:Value>
            </m:properties>
        </content>
    </entry>
    <entry>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=11&amp;$top=1</id>
        <title type="text">GetDataInsightStatisticsEntity</title>
        <updated>2014-10-27T14:21:21Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=11&amp;$top=1" />
        <content type="application/xml">
            <m:properties>
                <d:Key m:type="Edm.String">RawNumberOfUsers</d:Key>
                <d:Value m:type="Edm.String">9,044</d:Value>
            </m:properties>
        </content>
    </entry>
    <entry>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=12&amp;$top=1</id>
        <title type="text">GetDataInsightStatisticsEntity</title>
        <updated>2014-10-27T14:21:21Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=12&amp;$top=1" />
        <content type="application/xml">
            <m:properties>
                <d:Key m:type="Edm.String">SampelingNumberOfItems</d:Key>
                <d:Value m:type="Edm.String">2,000</d:Value>
            </m:properties>
        </content>
    </entry>
    <entry>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=13&amp;$top=1</id>
        <title type="text">GetDataInsightStatisticsEntity</title>
        <updated>2014-10-27T14:21:21Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=13&amp;$top=1" />
        <content type="application/xml">
            <m:properties>
                <d:Key m:type="Edm.String">SampelingNumberOfRecords</d:Key>
                <d:Value m:type="Edm.String">72,022</d:Value>
            </m:properties>
        </content>
    </entry>
    <entry>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=14&amp;$top=1</id>
        <title type="text">GetDataInsightStatisticsEntity</title>
        <updated>2014-10-27T14:21:21Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=14&amp;$top=1" />
        <content type="application/xml">
            <m:properties>
                <d:Key m:type="Edm.String">SampelingNumberOfUsers</d:Key>
                <d:Value m:type="Edm.String">9,044</d:Value>
            </m:properties>
        </content>
    </entry>
    </feed>

###<a name="62-model-insight"></a>6.2. Přehled modelu
Vrátí model přehled na aktivní sestavení nebo (Pokud možnost) na konkrétní Tvůrce dotazů.

K dispozici pouze pro sestavení doporučení.

| Metoda HTTP | IDENTIFIKÁTOR URI |
|:--------|:--------|
|ZÍSKÁNÍ     |S ID aktivní Tvůrce dotazů:<br>`<rootURI>/GetModelInsight?modelId=%27<model_id>%27&apiVersion=%271.0%27`<br><br>Příklad:<br>`<rootURI>/GetModelInsight?modelId=%271cac7b76-def4-41f1-bc81-29b806adb1de%27&apiVersion=%271.0%27`<br><br>S konkrétními vytvoření ID:<br>`<rootURI>/GetModelInsight?modelId=%27<model_id>%27&buildId=%27<build_id>%27&apiVersion=%271.0%27`|

|   Název parametru  |   Platné hodnoty                        |
|:--------          |:--------                              |
|   modelId |   Jedinečný identifikátor modelu |
|   buildId |   Volitelné - číslo označující úspěšné Tvůrce dotazů. |
|   apiVersion      | 1.0 |
|||
| Žádost o textu | ŽÁDNÁ |

**Odpověď**:

Nastavit informace HTTP stavový kód: 200

Data je vrácené jako sada vlastností.

- `feed/entry/id/content/properties/key`
- `feed/entry/id/content/properties/value`


Následující tabulka popisuje hodnotu, která představuje každý klíč.

| Klíč | Popis |
|:---- |:----|
| CatalogCoverage | Část katalogu můžete vycházet vzorky použití. Zbývající položky bude nutné funkce založené na obsah. |
| MPR | Střední hodnota percentilu hodnocení modelu. Funkce malá je lepší. |
| NumberOfDimensions | Počet rozměry používané algoritmu factorization matice. |


OData XML

    <feed xmlns:base="https://api.datamarket.azure.com/amla/recommendations/v3/GetModelInsight" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
    <title type="text" />
    <subtitle type="text">Get model insight statistics</subtitle>
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetModelInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'</id>
    <rights type="text" />
    <updated>2014-10-27T14:27:11Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetModelInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'" />
    <entry>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetModelInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
        <title type="text">GetModelInsightStatisticsEntity</title>
        <updated>2014-10-27T14:27:11Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetModelInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
        <content type="application/xml">
            <m:properties>
                <d:Key m:type="Edm.String">CatalogCoverage</d:Key>
                <d:Value m:type="Edm.String">1.000</d:Value>
            </m:properties>
        </content>
    </entry>
    <entry>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetModelInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=1&amp;$top=1</id>
        <title type="text">GetModelInsightStatisticsEntity</title>
        <updated>2014-10-27T14:27:11Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetModelInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=1&amp;$top=1" />
        <content type="application/xml">
            <m:properties>
                <d:Key m:type="Edm.String">Mpr</d:Key>
                <d:Value m:type="Edm.String">0.367</d:Value>
            </m:properties>
        </content>
    </entry>
    <entry>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetModelInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=2&amp;$top=1</id>
        <title type="text">GetModelInsightStatisticsEntity</title>
        <updated>2014-10-27T14:27:11Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetModelInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=2&amp;$top=1" />
        <content type="application/xml">
            <m:properties>
                <d:Key m:type="Edm.String">NumberOfDimensions</d:Key>
                <d:Value m:type="Edm.String">20</d:Value>
            </m:properties>
        </content>
    </entry>
    </feed>

###<a name="63-get-model-sample"></a>6.3. Získání ukázkový Model
Získá ukázkový model doporučení.

| Metoda HTTP | IDENTIFIKÁTOR URI |
|:--------|:--------|
|ZÍSKÁNÍ     |`<rootURI>/GetModelSample?modelId=%27<model_id>%27&apiVersion=%271.0%27`<br>Příklad:<br>`<rootURI>/GetModelSample?modelId=%271cac7b76-def4-41f1-bc81-29b806adb1de%27&apiVersion=%271.0%27`<br><br>S konkrétními vytvoření ID:<br>`<rootURI>/GetModelSample?modelId=%27<model_id>%27&buildId=%27<build_id>%27&apiVersion=%271.0%27`<br>Příklad:<br>`<rootURI>/GetModelSample?modelId=%271cac7b76-def4-41f1-bc81-29b806adb1de%27&buildId=%271500068%27&apiVersion=%271.0%27`|

|   Název parametru  |   Platné hodnoty                        |
|:--------          |:--------                              |
|   modelId |   Jedinečný identifikátor modelu |
|   apiVersion      | 1.0 |
|||
| Žádost o textu | ŽÁDNÁ |

**Odpověď**:

Nastavit informace HTTP stavový kód: 200

OData XML

Ve formátu nezpracovanými text je vrácena odpověď:

<pre>
Úrovně 1---655fc 955-a5a3-4a26-9723-3090859cb27b, kořistí: nové 655fc 955-a5a3-4a26-9723-3090859cb27b, kořistí: nové hodnocení: 8 0.5215 3f471802-f84f-44a0 - 99c-6d2e7418eec1, celý černý Hawk dolů: příběhu moderní War hodnocení: 0.5151 07b10e28-9e7c-4032-90b7-10acab7f2460, Cryptonomicon hodnocení: 0.5148 6afc18e4 8c2a - 43d 1-9021-57543d6b11d8 Imajica hodnocení: 0.5146 e4cc5e69-3567-43ab-b00f-f0d8d0506870, přístupů seznamu hodnocení: 0.514 56b61441-0eed-46cc-a8f6-112775b81892, životnost a úmrtí v Shanghai 56b61441-0eed-46cc-a8f6-112775b81892, životnost a úmrtí v Shanghai hodnocení: 0.5218 53156702-cc0c-443d-b718-6fb74b2491d3, Syn z \ hodnocení: 0.5212 fb8cf7a6 8719-46ee - 97d 4-92f931d77a3a, kouře a zrcadlí: krátké Fictions a Illusions hodnocení : 0.5188 8f5fe006-79e4-4679-816b-950989d1db4b, místem nikdy se stali hodnocení (moderní American Fiction): 0.5156 d8db4583-cc0f-49ce-bc95-b7fa3491623f, štěstí: nové hodnocení: 0.5156 50471eec 9aeb 4900 84 d 7 – 21567ab18546, pokud Buddha datovaná: příručku pro hledání láska na duchovní cestu cfe922a1-7ca0-4f8d-ad9d-b7cc87bfe0ef, Divine tajemství ja-ja Sisterhood: nové hodnocení: 0.5266 ff91a483-1ce5-4b37-a6fd-5ffcf21f8745, biblické Poisonwood: nové hodnocení: 28 0.5252 973f8cbd 0846-4f6b – 9d-4dd0d7dc3a19, prasat nebe hodnocení: 0.5244 e2cbf7ad-0636-4117-8b30-298da6df7077, zvířat sny hodnocení : 0.5227 6c818fd3-5a09-417d-9ab4-7ffe090f0fef, Confessions dobře Stepsister: nové hodnocení: 0.5222 5e97148f defb - 4 2D 74-af2d-80f4763bf531, hloubkové konec oceánu (Oprah v adresáři klubu) 5e97148f defb - 4 2D 74-af2d-80f4763bf531, hloubkové konec hodnocení oceánu (Oprah v adresáři klubu): 0.537 5dcbac37-2946-4f2a-a0b3-bbe710f9409a, nahoru ostrov: nové hodnocení: 0.5277 bc5b69db-733b-4346-adde-3927544258f7, centra města hodnocení: 0.5275 31fe5c63 3e5a - 48 2D 0-802b-d3b0f989a634 Dobrý den: kontrolních sledování krevního a Sweatsocks hodnocení: 0.5252 0adf981a b65b - 4c 11 – b36b-78aca2f948a2, správný bouře : PRAVDA dlouhé zprávy o muži proti hodnocení moři: 0.5238 68f97068-ae1a-4163-9e94-396b800b743d, Modoc: PRAVDA postupu největší Slon, která nikdy životnost 68f97068-ae1a-4163-9e94-396b800b743d, Modoc: True postupu největší Slon, která nikdy životnost hodnocení: 0.5379 6724862e-e4e7-4022-9614-1468d8b902ff, malé Chaloupku na hodnocení psounů: 4 0.5345 cdedb837-1620-496d - 94c-6ccfed888320, malé Chaloupku ve velké hodnocení jazyk: 0.5325 382164ba-406b-4187-b726-d7a54b9d790d, Tao of hodnocení Pooh: 0.5309 6a068d6a-bb74-4ba3-b3f2-a956c4f9d1b5 , Na bank Švestka říčku hodnocení: 0.5285 37ef8e74-e348-44e5-aabc-1d7f9efcb25b, jsou muži z Mars ženy jsou z Venus: praktické Průvodce pro vylepšení komunikace a zprovoznění co se má ve vaší relace 37ef8e74-e348-44e5-aabc-1d7f9efcb25b, muži jsou z Mars, ženy z Venus: praktické Průvodce pro vylepšení komunikace a zprovoznění co chcete vztahů hodnocení: 0.5397 f2be16d4 5faf - 4 2D 32-ab83-7ba74d29261e, Politically správné Bedtime články : Moderní kontrolky naše dobu a časy hodnocení: 0.5207 ef732c5c-334b-4d6b-ab82-7255eb7286d0, Oslava mezi zloději hodnocení: 0.5195 0b209b8c-7cdd-47fd-b940-05c7ff7c60fc, okno poskytuje strom hodnocení: 0.5194 883b360f-8b42-407f-b977-2f44ad840877, strach články zjistit v tmavé: odebrané hodnocení American folklórní (strach články): 0.5184 ff51b67e-fa8e-4c5e-8f4d-02a928de735d, muži v práci: řemesla Baseballovou d008dae9-c73a-40a1-9a9b-96d5cf546f36, Gulag souostroví 1918 1956: pokus v hodnocení literárních vyšetřování můžu II: 0.5416 ff51b67e-fa8e-4c5e-8f4d-02a928de735d, muži v práci : Řemesla Baseballovou hodnocení: 0.5403 49dec30e-0adb-411a-b186-48eaabf6f8bc, Fatherland hodnocení: 0.5394 cc7964fd-d30f-478e-a425-93ddbdf094ed, Magické shromažďování: zápasy objemové 1 hodnocení: 0.5379 8a1e9f36-97af-4614-bed9-24e3940a05f3, další Sniglets: libovolné slovo, které se nezobrazuje na slovník, ale měli hodnocení: 0.5377 12a6d988-be21-4a09-8143-9d5f4261ba16, sen Eagles 07b10e28-9e7c-4032-90b7-10acab7f2460, Cryptonomicon hodnocení: 0.5417 e4cc5e69-3567-43ab-b00f-f0d8d0506870, přístupů seznamu hodnocení: 0.5416 1f1a34c4-9781-49f5-a3cc-acec3ae3c71d, řady hodnocení: 0.5371 56daeffe 7d 48-43cd-8ef8-7dffd0c103d3, Kilo třídy hodnocení: 0.5366 b2fe511e-5cb9-4a56-b823-2801e63e6a96, právní nabídky hodnocení : 0.5366 df87525b-e435-4bd6-8701-4e60ad344e28, zjištění ryby 56d 33036-dfda-46b9-8e2a-76cb03921bb0, X soubory: základu nula hodnocení: 0.5417 0780cde8-6529-4e1d-b6c6-082c1b80e596, 12 červená Herrings hodnocení: 0.5416 df87525b-e435-4bd6-8701-4e60ad344e28, ryby hledání hodnocení: 0.5408 400fe331 - 2c 35-490c-adbc-b28b4b73d56c, se nám poznat, prezidenta? Hodnocení: 0.5383 f86ad7d0 - 5c 03-42b3-aebf-13d44aec8b30, odstíny poskytnutá hodnocení: 0.5358 de1f62a4 89e6 - 44 2D 2-aaee-992a4bf093f1, mapování, které změní na světě: William Novák a narození moderní geologie de1f62a4 89e6 - 44 2D 2-aaee-992a4bf093f1, mapování, které změní na světě: William Novák a narození moderní geologie hodnocení: 0.5422 b303538f-e2c6-4a2c-b425-8d21e684fc3e, Moje Uncle Oswald hodnocení: 4 0.5385 34b84627-48af-4a4c - 96c-b26fb3863f56, půlnoci v vzorníku zahradní dobré a Evil hodnocení: 0.5379 306cbaa7 b1a8 4142 9 d 55-e11b5018a7a8, ulice advokát hodnocení : 0.5376 e53b4baa - 8c 09 45c 4 95c 0-b6a26b98770b, paní Smillas úplně pro Snow hodnocení: 0.5367

<a name="level-2"></a>Úrovně 2
---------------
352aaea1-6b12-454d-a3d5-46379d9e4eb2, Sinister Prasátko (Hillerman Tony) 352aaea1-6b12-454d-a3d5-46379d9e4eb2, Sinister Prasátko (Hillerman Tony) hodnocení: 0.5425 74c 49398-bc10-4af5-a658-a996a1201254, děti bouře (Peters Markétě) hodnocení: 0.5387 9ba80080-196e-43fd-8025-391d963f77e7, plovoucí holčičky hodnocení: 0.5372 e68f81d5-7745-4cc7-b943-fedb8fcc2ced, používání hodnocení smajlíka (Scottoline Lisa): 0.5353 b2fe511e-5cb9-4a56-b823-2801e63e6a96, nabídka právní hodnocení: 0.5332 c65c3995-abf7-4c7b-bb3c-8eb5aa9be7a5, jezera Wobegon dnů 0adf981a b65b - 4c 11 – b36b-78aca2f948a2, správný bouře: A True textu z muži proti moři hodnocení: 0.5433 dnů jezera Wobegon c65c3995-abf7-4c7b-bb3c-8eb5aa9be7a5, hodnocení : 0.543 a00ae6ad-4a7f-4211-9836-75ce8834eb11, Sniglets (Snig'lit: kterékoli slovo, které nejsou zobrazeny v adresáři, ale měli) hodnocení: 0.5327 6f6e192e 0d 64-49ca-9b63-f09413ea1ee6, Politically správné svátků články: pro Enlightened vánoční ročními hodnocení: 0.5307 798051a8 147d - 4 2D 46-b0dc-e836325029e6, stáří INNOCENCE (FILM maloobchodě) hodnocení: 0.5301 73f3e25a-e996-4162-9ed8-ff3d34075650, známost O průkopníky! (Tučňáka vysokého dvacetinu Century klasické) cba8163f-6536-436b-8130-47b4a43c827f, důvěřovat hodnocení nikdo (oficiální pokyny pro X soubory objemové 2): 0.5434 5708e4cb 2492 49 c 0-94a8-cc413eec5d89, malé Gods (Discworld romány (Paperback)) hodnocení: 0.5406 73f3e25a-e996-4162-9ed8-ff3d34075650, známost O průkopníky! (Tučňáka vysokého dvacetinu Century klasické) Hodnocení: 0.5403 d885b0bd-ae4b-452d-bdf2-faa90197dbc9, barvy Magic hodnocení: 0.539 b133a9c4-4784-4db3-b100-d0d6dffb94d2, pravdy je hodnocení tam (oficiální průvodci objemové soubory X 1): 0.5367 271700a5-854a-4d5a-8409-6b57a5ee4de4, Fluke: nebo mám vědět, proč Winged velryba Sings 271700a5-854a-4d5a-8409-6b57a5ee4de4, Fluke: nebo mám vědět, proč Winged velryba Sings hodnocení: 0.5445 2de1c354-90ff - 47c 5-a0db-1bad7d88ef94 hodnocení Wife (děti násilí řad) Salaryman: 0.5329 d279416e - 19c 0-43f8-9ec9-a585947879ca, Zen poloze hodnocení : 0.5316 c8f854d7-3de3-4b23-8217-f4f851670fd4, odvety dívky Cootie: hodnocení Mystery (Petra Hudsonem Záhady historických (Paperback)) Petra Hudsonem: 0.5305 8ef4751c-7074-409e-a3ac-d49b222fc864, kde jsou hodnocení volně věci: 0.5289 9ad1b620-0a7b-4543-8673-66d4c3bcb2f1, jejich Eyes byly sledování i 9ad1b620-0a7b-4543-8673-66d4c3bcb2f1, jejich Eyes byly sledování i hodnocení: 0.5446 da45c4d5-aba1-413b-a9bd-50df98b1e1d2, šířka stromy hodnocení: 0.5389 65ecbdd1 131c - 40c 3-a3d6-d86ca281377a, i malé věci hodnocení: 0.5387 c78743bf-7947-4a0c-8db7-8a3bfe69ba70, minerální diáře hodnocení: 28 0.5355 973f8cbd 0846-4f6b – 9d-4dd0d7dc3a19, prasat nebe hodnocení : 0.5344 5f17d90a-. 2604-4fe8-8977-1a280b9098b1, pro peněžní (Stephanie Švestka romány (Paperback)) 5f17d90a-. 2604-4fe8-8977-1a280b9098b1, pro hodnocení peníze (Stephanie Švestka romány (Paperback)): 0.5446 57169b2b-9a8a-486b-9aac-1ed98ce57168 a konečném odvolání hodnocení: 0.5332 efcb1bc4-7278-4a8f-b491-befde02070d6, momentový pravdy hodnocení: 0.5329 1efa91a2 993b - 4c 43-9f5c-3454fc12612d vypálit faktor hodnocení: 0.5309 24c 59962-458a-4ec8-b95d-d694e861919c, doma v Mitford (Mitford roků) hodnocení: 0.5303 4fd48c46 1a20 - 4c 57-bc7f-a02ef123dc52 jako přírodní udělali mu: chlapečka, kdo aktivované jako 4fd48c46-1a20-4c57-bc7f-a02ef123dc52 holčičky , Podle povahy udělali jemu: chlapečka kdo byl mocninu jako holčičky hodnocení: 0.5449 cd5f2c03-20cb-43be-a1fb-3b4233e63222, prasat nebe hodnocení: 0.5329 19985fdb-d07a-4a25-ae4a-97b9cb61e5d1, láska v hodnocení čas cholery (tučňáka vysokého skvělých knih 20 Century): 0.5267 15689d 09 c711 4844 84 d 8 130a90237b26 Bel Canto hodnocení: 0.5245 ff91a483-1ce5-4b37-a6fd-5ffcf21f8745, biblické Poisonwood: A nové hodnocení: 0.5235 98df28ec-41e7-4fca-b77f-8b0d3109085d, hvězdy Trek záznamy f874b5a3 5d 40 – 4436-94ff-0fa1c090ddf5, ne taky zvyšuje (klasický A Scribner) hodnocení : 0.5451 98df28ec-41e7-4fca-b77f-8b0d3109085d, hvězdy Trek záznamy hodnocení: 0.5442 0ce0014a-9a48-4013-a08a-7f2c11877930, H.M.S. Neviditelný hodnocení: 0.5421 15316ca6-1e38-425f-893d-691944a47000, více strach články zjistit v tmavě hodnocení: 0.5409 329d 5682-3dc3-4206-8aa2-eef4b1032258, písmena hodnocení Zemitých: 49 0.54 5b9445d5 c072 - 419c - 8d-6f669bb1b0a9, dcera Fortune: nové (Oprah v adresáři klubu (Hardcover)) 5b9445d5 c072 - 419c - 8d 49-6f669bb1b0a9, dcera Fortune: hodnocení nové (Oprah v adresáři klubu (Hardcover)): 0.5462 ff91a483-1ce5-4b37-a6fd-5ffcf21f8745, biblické Poisonwood: nové hodnocení: 0.5372 604eb3bd-6026-4f51-bffd-9fb54f180400, řady obrázky: nové hodnocení: 0.5341 8d06d01d – 31cd-4678-b6b1-140a67987ce9, skladeb v hodnocení běžný čas (Oprah v adresáři klubu (Paperback)) : 0.5334 da45c4d5-aba1-413b-a9bd-50df98b1e1d2, šířka stromy hodnocení: 0.5319 d5358189-d70f-4e35-8add-34b83b4942b3, prasat nebe d5358189-d70f-4e35-8add-34b83b4942b3, prasat nebe hodnocení: 0.5491 ff91a483-1ce5-4b37-a6fd-5ffcf21f8745, biblické Poisonwood: nové hodnocení: 0.5401 c78743bf-7947-4a0c-8db7-8a3bfe69ba70, minerální diáře hodnocení: 0.5393 8d06d01d – 31cd-4678-b6b1-140a67987ce9, skladeb v běžných čas (Oprah v adresáři klubu (Paperback)) hodnocení: 28 0.5382 973f8cbd 0846-4f6b – 9d-4dd0d7dc3a19, prasat nebe hodnocení: 0.5367

</pre>


##<a name="7-model-business-rules"></a>7. pravidla firmy modelu

Toto jsou typy pravidel podporované:
- <strong>Seznamu blokovaných odesílatelů</strong> - seznamu blokovaných odesílatelů umožňuje poskytovat seznam položek, které nechcete, aby se vrátíte do výsledků doporučení. 

- <strong>FeatureBlockList</strong> – funkce seznamu blokovaných odesílatelů umožňuje blokovat položky na základě hodnot jeho funkcí.

*Neodesílat víc než 1 000 položek v pravidlu jednoho seznamu blokovaných odesílatelů nebo volání můžou časový limit. Pokud potřebujete blokovat víc než 1 000 položek, můžete volat několika seznamu blokovaných odesílatelů.*

- <strong>Upsale</strong> - Upsale umožňuje vynutit položky se vrátíte do výsledků doporučení.

- <strong>Povolených</strong> – bílá seznamu umožňuje maximálně doporučení ze seznamu položek.

- <strong>FeatureWhiteList</strong> – seznam bílé funkcí umožňuje pouze doporučujeme položky, které mají hodnot konkrétní funkcí.

- <strong>PerSeedBlockList</strong> - za počáteční hodnota rámeček – seznam blokování umožňuje na položku seznam položek, které nelze zobrazit ve výsledcích doporučení.




###<a name="71-get-model-rules"></a>7.1. Získání pravidel modelu

| Metoda HTTP | IDENTIFIKÁTOR URI |
|:--------|:--------|
|ZÍSKÁNÍ     |`<rootURI>/GetModelRules?modelId=%27<model_id>%27&apiVersion=%271.0%27`<br>Příklad:<br>`<rootURI>/GetModelRules?modelId=%271cac7b76-def4-41f1-bc81-29b806adb1de%27&apiVersion=%271.0%27`|

|   Název parametru  |   Platné hodnoty                        |
|:--------          |:--------                              |
|   modelId |   Jedinečný identifikátor modelu |
|   apiVersion      | 1.0 |
|||
| Žádost o textu | ŽÁDNÁ |

**Odpověď**:

Nastavit informace HTTP stavový kód: 200

- `feed/entry/content/properties/Id`-Jedinečný identifikátor toto pravidlo.
- `feed/entry/content/properties/Type`– Typ pravidla.
- `feed/entry/content/properties/Parameter`– Parametr pravidla.

OData XML

    <feed xmlns:base="https://api.datamarket.azure.com/amla/recommendations/v3/GetModelRules" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
    <title type="text" />
    <subtitle type="text">Get a list of rules for a model</subtitle>
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetModelRules?modelId='5e824626-50d3-469d-a824-564d38453103'&amp;apiVersion='1.0'</id>
    <rights type="text" />
    <updated>2014-11-05T12:58:57Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetModelRules?modelId='5e824626-50d3-469d-a824-564d38453103'&amp;apiVersion='1.0'" />
    <entry>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetModelRules?modelId='5e824626-50d3-469d-a824-564d38453103'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
        <title type="text">GetAListOfRulesForAModelEntity</title>
        <updated>2014-11-05T12:58:57Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetModelRules?modelId='5e824626-50d3-469d-a824-564d38453103'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
        <content type="application/xml">
            <m:properties>
                <d:Id m:type="Edm.String">1000043</d:Id>
                <d:Type m:type="Edm.String">BlockList</d:Type>
                <d:Parameters m:type="Edm.String">{"ItemsToExclude":["1000"]}</d:Parameters>
            </m:properties>
        </content>
    </entry>
    <entry>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetModelRules?modelId='5e824626-50d3-469d-a824-564d38453103'&amp;apiVersion='1.0'&amp;$skip=1&amp;$top=1</id>
        <title type="text">GetAListOfRulesForAModelEntity</title>
        <updated>2014-11-05T12:58:57Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetModelRules?modelId='5e824626-50d3-469d-a824-564d38453103'&amp;apiVersion='1.0'&amp;$skip=1&amp;$top=1" />
        <content type="application/xml">
            <m:properties>
                <d:Id m:type="Edm.String">1000044</d:Id>
                <d:Type m:type="Edm.String">BlockList</d:Type>
                <d:Parameters m:type="Edm.String">{"ItemsToExclude":["1001"]}</d:Parameters>
            </m:properties>
        </content>
    </entry>
    </feed>

###<a name="72-add-rule"></a>7.2. Přidání pravidla

| Metoda HTTP | IDENTIFIKÁTOR URI |
|:--------|:--------|
|PŘÍSPĚVEK     |`<rootURI>/AddRule?apiVersion=%271.0%27`|
|ZÁHLAVÍ   |`"Content-Type", "text/xml"`|

|   Název parametru  |   Platné hodnoty                        |
|:--------          |:--------                              |
|   apiVersion      | 1.0 |
|||
| Žádost o textu | 
<ins>Pokaždé, když poskytuje ID položky pro obchodní pravidla, zkontrolujte, že použijte externí Id položky (stejné Id, které jste použili v souboru katalogu)</ins><br>
<ins>Přidání pravidla seznamu blokovaných odesílatelů:</ins><br>`<ApiFilter xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"><ModelId>24024f7e-b45c-419e-bfa2-dfd947e0d253</ModelId><Type>BlockList</Type><Value>{"ItemsToExclude":["2406E770-769C-4189-89DE-1C9283F93A96","3906E110-769C-4189-89DE-1C9283F98888"]}</Value></ApiFilter>`<br><br><ins>
<ins>Přidání pravidla FeatureBlockList:</ins><br>
<br>
`<ApiFilter xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"><ModelId>24024f7e-b45c-419e-bfa2-dfd947e0d253</ModelId><Type>FeatureBlockList</Type><Value>{"Name":"Movie_category","Values":["Adult","Drama"]}</Value></ApiFilter>`<br><br><ins>Chcete-li přidat pravidlo Upsale:</ins><br>`<ApiFilter xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"><ModelId>24024f7e-b45c-419e-bfa2-dfd947e0d253</ModelId><Type>Upsale</Type><Value>{"ItemsToUpsale":["2406E770-769C-4189-89DE-1C9283F93A96"],"NumberOfItemsToUpsale":5}</Value></ApiFilter>`<br><br>
<ins>Přidání pravidla povolených:</ins><br>
`<ApiFilter xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"><ModelId>24024f7e-b45c-419e-bfa2-dfd947e0d253</ModelId><Type>WhiteList</Type><Value>{"ItemsToInclude":["2406E770-769C-4189-89DE-1C9283F93A96","1116E770-769C-4189-89DE-1C9283F88888"]}</Value></ApiFilter>`<br><br><ins>
<ins>Přidání pravidla FeatureWhiteList:</ins><br>
<br>
`<ApiFilter xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"><ModelId>24024f7e-b45c-419e-bfa2-dfd947e0d253</ModelId><Type>FeatureWhiteList</Type><Value>{"Name":"Movie_rating","Values":["PG13"]}</Value></ApiFilter>`<br><br><ins>Přidání pravidla PerSeedBlockList:</ins><br>`<ApiFilter xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"><ModelId>24024f7e-b45c-419e-bfa2-dfd947e0d253</ModelId><Type>PerSeedBlockList</Type><Value>{"SeedItems":["9949"],"ItemsToExclude":["9862","8158","8244"]}</Value></ApiFilter>`|


**Odpověď**:

Nastavit informace HTTP stavový kód: 200

Rozhraní API vrátí nově vytvořené pravidlo s její podrobnosti. Vlastnost pravidel můžete načtená z kontingenčního seznamu následující cesty:

- `feed/entry/content/properties/Id`-Jedinečný identifikátor toto pravidlo.
- `feed/entry/content/properties/Type`– Typ pravidla: seznamu blokovaných odesílatelů nebo Upsale.
- `feed/entry/content/properties/Parameter`– Parametr pravidla.

OData XML

    <feed xmlns:base="https://api.datamarket.azure.com/amla/recommendations/v3/AddRule" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
    <title type="text" />
    <subtitle type="text">Add A Rule</subtitle>
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/AddRule?apiVersion='1.0'</id>
    <rights type="text" />
    <updated>2014-11-05T11:13:28Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/AddRule?apiVersion='1.0'" />
    <entry>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/AddRule?apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
        <title type="text">AddARuleEntity</title>
        <updated>2014-11-05T11:13:28Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/AddRule?apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
        <content type="application/xml">
            <m:properties>
                <d:Id m:type="Edm.String">1000041</d:Id>
                <d:Type m:type="Edm.String">BlockList</d:Type>
                <d:Parameters m:type="Edm.String">{"ItemsToExclude":["1002"]}</d:Parameters>
            </m:properties>
        </content>
    </entry>
    </feed>

###<a name="73-delete-rule"></a>7.3. Odstranění pravidla

| Metoda HTTP | IDENTIFIKÁTOR URI |
|:--------|:--------|
|ODSTRANĚNÍ     |`<rootURI>/DeleteRule?modelId=%27<model_id>%27&filterId=%27<filter_Id>%27&apiVersion=%271.0%27`<br><br>Příklad:<br>`DeleteRule?modelId=%2724024f7e-b45c-419e-bfa2-dfd947e0d253%27&filterId=%271000011%27&apiVersion=%271.0%27`|

|   Název parametru  |   Platné hodnoty                        |
|:--------          |:--------                              |
|   modelId |   Jedinečný identifikátor modelu |
|   filterId    |   Jedinečný identifikátor filtru |
|   apiVersion      | 1.0 |
|||
| Žádost o textu | ŽÁDNÁ |

**Odpověď**:

Nastavit informace HTTP stavový kód: 200

###<a name="74-delete-all-rules"></a>7.4. Odstranění všech pravidel

| Metoda HTTP | IDENTIFIKÁTOR URI |
|:--------|:--------|
|ODSTRANĚNÍ     |`<rootURI>/DeleteAllRules?modelId=%27<model_id>%27&apiVersion=%271.0%27`<br><br>Příklad:<br>`DeleteAllRules?modelId=%2724024f7e-b45c-419e-bfa2-dfd947e0d253%27&apiVersion=%271.0%27`|

|   Název parametru  |   Platné hodnoty                        |
|:--------          |:--------                              |
|   modelId |   Jedinečný identifikátor modelu |
|   apiVersion      | 1.0 |
|||
| Žádost o textu | ŽÁDNÁ |

**Odpověď**:

Nastavit informace HTTP stavový kód: 200

##<a name="8-catalog"></a>8. katalog

###<a name="81-import-catalog-data"></a>8.1. Import katalogu dat

Pokud odešlete několik katalogu souborů do stejné modelu s několika voláním jsme vloží jenom nové položky katalogu. Existující položky zůstanou s původní hodnoty. Katalog dat nelze aktualizovat tímto způsobem.

Katalog dat měli podle pokynů v tomto formátu:

- Bez funkcí:`<Item Id>,<Item Name>,<Item Category>[,<Description>]`

- Pomocí funkcí:`<Item Id>,<Item Name>,<Item Category>,[<Description>],<Features list>`

Poznámka: Maximální velikost souboru je 200MB.

**Podrobnosti o formátu**

| Jméno | Povinné | Typ |  Popis |
|:---|:---|:---|:---|
| Id položky |Ano | [A-z], [a-z], [0 – 9], [_] & #40; Podtržení & #41; [-] & #40; pomlčku & #41;<br> Maximální délka: 50 | Jedinečný identifikátor položky. |
| Název položky | Ano | Alfanumerické znaky<br> Maximální počet znaků: 255 | Název položky. | 
| Kategorie položek | Ano | Alfanumerické znaky <br> Maximální počet znaků: 255 | Kategorie, ke kterému patří tuto položku (například vaření publikace obrázkům...); může být prázdné. |
| Popis | Ne, pokud jsou funkce prezentovat (, ale může být prázdný) | Alfanumerické znaky <br> Maximální délka: 4000 | Popis této položky. |
| Seznam funkcí | Ne | Alfanumerické znaky <br> Maximální délka: 4000; Maximální počet funkcí: 20 | Seznam oddělený čárkami názvu funkce = funkce hodnota, která slouží k rozšíření modelu doporučení; Přečtěte si téma oddílu [Pokročilé témata](#2-advanced-topics) . |


| Metoda HTTP | IDENTIFIKÁTOR URI |
|:--------|:--------|
|PŘÍSPĚVEK     |`<rootURI>/ImportCatalogFile?modelId=%27<modelId>%27&filename=%27<fileName>%27&apiVersion=%271.0%27`<br><br>Příklad:<br>`<rootURI>/ImportCatalogFile?modelId=%27a658c626-2baa-43a7-ac98-f6ee26120a12%27&filename=%27catalog10_small.txt%27&apiVersion=%271.0%27`|
|ZÁHLAVÍ   |`"Content-Type", "text/xml"`|

|   Název parametru  |   Platné hodnoty                        |
|:--------          |:--------                              |
|   modelId |   Jedinečný identifikátor modelu  |
| Název souboru | Textový identifikátor katalogu.<br>Jenom písmena (A-Z, z), číslice (0 – 9), spojovníky (--) a jsou povoleny podtržítka (_).<br>Maximální délka: 50 |
|   apiVersion      | 1.0 |
|||
| Žádost o textu | Příklad (s funkcemi):<br/>vytváření Clara Callan knihy, popis knihy 2406e770-769c-4189-89de-1c9283f93a96, = Richard Wright Publisheru = Harper Flamingo Kanada rok = 2001<br>21bf8088-b6c0-4509-870c-e1c7ac78304a, zapomenutí místnosti: vytváření A Fiction (Byzantium Book), sešit, = přezdívka Bantock Publisheru = Harpercollins, rok = 1997<br>3bb5cb44-d143-4bdd-a55c-443964bf4b23, Spadework, knihy, vytvářet = Alexander Findley Publisheru = HarperFlamingo Kanada rok = 2001<br>552a1940-21e4-4399-82bb-594b46d7ed54, omezení zvířata, knihy, popis knihy vytváření = Magnus lisoven Publisheru = arkádová publikování rok = 1998</pre> |


**Odpověď**:

Nastavit informace HTTP stavový kód: 200

Rozhraní API chybovou zprávu importu.
- `feed\entry\content\properties\LineCount`-Byla přijata počet řádků.
- `feed\entry\content\properties\ErrorCount`-Počet řádků, které nebyly vkládat kvůli chybě.

OData XML

    <feed xmlns:base="https://api.datamarket.azure.com/amla/recommendations/v3/ImportCatalogFile" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
    <title type="text" />
    <subtitle type="text">Import catalog file</subtitle>
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/ImportCatalogFile?modelId='a658c626-2baa-43a7-ac98-f6ee26120a12'&amp;filename='catalog10_small.txt'&amp;apiVersion='1.0'</id>
    <rights type="text" />
    <updated>2014-10-05T06:58:04Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/ImportCatalogFile?modelId='a658c626-2baa-43a7-ac98-f6ee26120a12'&amp;filename='catalog10_small.txt'&amp;apiVersion='1.0'" />
    <entry>
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/ImportCatalogFile?modelId='a658c626-2baa-43a7-ac98-f6ee26120a12'&amp;filename='catalog10_small.txt'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
        <title type="text">ImportCatalogFileEntity2</title>
        <updated>2014-10-05T06:58:04Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/ImportCatalogFile?modelId='a658c626-2baa-43a7-ac98-f6ee26120a12'&amp;filename='catalog10_small.txt'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
        <content type="application/xml">
            <m:properties>
                <d:LineCount m:type="Edm.String">4</d:LineCount>
                <d:ErrorCount m:type="Edm.String">0</d:ErrorCount>
            </m:properties>
        </content>
    </entry>
    </feed>

###<a name="82-get-catalog"></a>8.2. Získání katalogu
Vyhledá všechny položky katalogu.
Katalog bude načtena jednu stránku po druhém. Pokud chcete získat položky na konkrétní indexu, můžete použít parametr $skip odata. Například pokud chcete získat položky od pozice 100, přidejte parametr $skip = 100 k žádosti o.

| Metoda HTTP | IDENTIFIKÁTOR URI |
|:--------|:--------|
|ZÍSKÁNÍ     |`<rootURI>/GetCatalog?modelId=%27<model_id>%27&apiVersion=%271.0%27`<br><br>Příklad:<br>`GetCatalog?modelId=%2724024f7e-b45c-419e-bfa2-dfd947e0d253%27&apiVersion=%271.0%27`|

|   Název parametru  |   Platné hodnoty                        |
|:--------          |:--------                              |
|   modelId |   Jedinečný identifikátor modelu |
|   apiVersion      | 1.0 |
|||
| Žádost o textu | ŽÁDNÁ |

**Odpověď**:

Nastavit informace HTTP stavový kód: 200

Odpověď na obsahuje jednu položku za položky katalogu. Každá položka má následující údaje:

- `feed/entry/content/properties/ExternalId`– ID položky katalogu externí, jaké je uvedeno zákazníkem.
- `feed/entry/content/properties/InternalId`-Katalogu interní ID položky ten, který obsahuje generované Azure počítače výukové doporučení.
- `feed/entry/content/properties/Name`-Název položky katalogu.
- `feed/entry/content/properties/Category`-Kategorie položky katalogu.
- `feed/entry/content/properties/Description`-Popis položky katalogu.
- `feed/entry/content/properties/Metadata`-Katalogu metadata položky.


OData XML

    <feed xmlns:base="https://api.datamarket.azure.com/amla/recommendations/v3/GetCatalog" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
        <title type="text" />
        <subtitle type="text">Get All Catalog Items</subtitle>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetCatalog?modelId='0dbb55fa-7f11-418d-8537-8ff2d9d1d9c6'&amp;apiVersion='1.0'</id>
        <rights type="text" />
        <updated>2014-10-29T11:13:26Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetCatalog?modelId='0dbb55fa-7f11-418d-8537-8ff2d9d1d9c6'&amp;apiVersion='1.0'" />
        <entry>
            <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetCatalog?modelId='0dbb55fa-7f11-418d-8537-8ff2d9d1d9c6'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
        <title type="text">AllCatalogItemsEntity</title>
        <updated>2014-10-29T11:13:26Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetCatalog?modelId='0dbb55fa-7f11-418d-8537-8ff2d9d1d9c6'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
        <content type="application/xml">
            <m:properties>
                <d:ExternalId m:type="Edm.String">552A1940-21E4-4399-82BB-594B46D7ED54</d:ExternalId>
                <d:InternalId m:type="Edm.String">060db26a-e6a6-464e-bb52-436d2da82a50</d:InternalId>
                <d:Name m:type="Edm.String">Restraint of Beasts</d:Name>
                <d:Category m:type="Edm.String">Book</d:Category>
                <d:Description m:type="Edm.String"></d:Description>
                <d:Metadata m:type="Edm.String"></d:Metadata>
            </m:properties>
        </content>
    </entry>
    <entry>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetCatalog?modelId='0dbb55fa-7f11-418d-8537-8ff2d9d1d9c6'&amp;apiVersion='1.0'&amp;$skip=1&amp;$top=1</id>
        <title type="text">AllCatalogItemsEntity</title>
        <updated>2014-10-29T11:13:26Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetCatalog?modelId='0dbb55fa-7f11-418d-8537-8ff2d9d1d9c6'&amp;apiVersion='1.0'&amp;$skip=1&amp;$top=1" />
        <content type="application/xml">
            <m:properties>
                <d:ExternalId m:type="Edm.String">2406E770-769C-4189-89DE-1C9283F93A96</d:ExternalId>
                <d:InternalId m:type="Edm.String">209d7bfe-2eb9-4455-92a3-7c867a41a74a</d:InternalId>
                <d:Name m:type="Edm.String">Clara Callan</d:Name>
                <d:Category m:type="Edm.String">Book</d:Category>
                <d:Description m:type="Edm.String"></d:Description>
                <d:Metadata m:type="Edm.String"></d:Metadata>
            </m:properties>
        </content>
    </entry>
    <entry>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetCatalog?modelId='0dbb55fa-7f11-418d-8537-8ff2d9d1d9c6'&amp;apiVersion='1.0'&amp;$skip=2&amp;$top=1</id>
        <title type="text">AllCatalogItemsEntity</title>
        <updated>2014-10-29T11:13:26Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetCatalog?modelId='0dbb55fa-7f11-418d-8537-8ff2d9d1d9c6'&amp;apiVersion='1.0'&amp;$skip=2&amp;$top=1" />
        <content type="application/xml">
            <m:properties>
                <d:ExternalId m:type="Edm.String">3BB5CB44-D143-4BDD-A55C-443964BF4B23</d:ExternalId>
                <d:InternalId m:type="Edm.String">913ff79b-059b-4d4d-8042-6fa4cc1391dd</d:InternalId>
                <d:Name m:type="Edm.String">Spadework</d:Name>
                <d:Category m:type="Edm.String">Book</d:Category>
                <d:Description m:type="Edm.String"></d:Description>
                <d:Metadata m:type="Edm.String"></d:Metadata>
            </m:properties>
        </content>
    </entry>
    <entry>
            <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetCatalog?modelId='0dbb55fa-7f11-418d-8537-8ff2d9d1d9c6'&amp;apiVersion='1.0'&amp;$skip=3&amp;$top=1</id>
        <title type="text">AllCatalogItemsEntity</title>
        <updated>2014-10-29T11:13:26Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetCatalog?modelId='0dbb55fa-7f11-418d-8537-8ff2d9d1d9c6'&amp;apiVersion='1.0'&amp;$skip=3&amp;$top=1" />
        <content type="application/xml">
            <m:properties>
                <d:ExternalId m:type="Edm.String">21BF8088-B6C0-4509-870C-E1C7AC78304A</d:ExternalId>
                <d:InternalId m:type="Edm.String">ea65e4fa-768c-40b4-92c3-69d3e8178691</d:InternalId>
                <d:Name m:type="Edm.String">The Forgetting Room: A Fiction (Byzantium Book)</d:Name>
                <d:Category m:type="Edm.String">Book</d:Category>
                <d:Description m:type="Edm.String"></d:Description>
                <d:Metadata m:type="Edm.String"></d:Metadata>
            </m:properties>
        </content>
    </entry>
    </feed>

###<a name="83-get-catalog-items-by-token"></a>8.3. Získání položky katalogu tak, že tokenu

| Metoda HTTP | IDENTIFIKÁTOR URI |
|:--------|:--------|
|ZÍSKÁNÍ     |`<rootURI>/GetCatalogItemsByToken?modelId=%27<modelId>%27&token=%27<token>%27&apiVersion=%271.0%27`<br><br>Příklad:<br>`GetCatalogItemsByToken?modelId=%270dbb55fa-7f11-418d-8537-8ff2d9d1d9c6%27&token=%27Cla%27&apiVersion=%271.0%27`|

|   Název parametru  |   Platné hodnoty                        |
|:--------          |:--------                              |
|   modelId |   Jedinečný identifikátor modelu |
|   tokenu   |   Token název položky katalogu. Smí obsahovat alespoň na úrovni 3 znaky. |
|   apiVersion      | 1.0 |
|||
| Žádost o textu | ŽÁDNÁ |

**Odpověď**:

Nastavit informace HTTP stavový kód: 200

Odpověď na obsahuje jednu položku za položky katalogu. Každá položka má následující údaje:

- `feed/entry/content/properties/InternalId`-Katalogu interní ID položky ten, který obsahuje generované Azure počítače výukové doporučení.
- `feed/entry/content/properties/Name`-Název položky katalogu.
- `feed/entry/content/properties/Rating`-(pro budoucí použití)
- `feed/entry/content/properties/Reasoning`-(pro budoucí použití)
- `feed/entry/content/properties/Metadata`-(pro budoucí použití)
- `feed/entry/content/properties/FormattedRating`-(pro budoucí použití)

OData XML

    <feed xmlns:base="https://api.datamarket.azure.com/amla/recommendations/v3/GetCatalogItemsByToken" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
        <title type="text" />
        <subtitle type="text">Get Catalog items that contain a token</subtitle>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetCatalogItemsByToken?modelId='0dbb55fa-7f11-418d-8537-8ff2d9d1d9c6'&amp;token='Cla'&amp;apiVersion='1.0'</id>
        <rights type="text" />
        <updated>2014-10-29T11:48:19Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetCatalogItemsByToken?modelId='0dbb55fa-7f11-418d-8537-8ff2d9d1d9c6'&amp;token='Cla'&amp;apiVersion='1.0'" />
        <entry>
            <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetCatalogItemsByToken?modelId='0dbb55fa-7f11-418d-8537-8ff2d9d1d9c6'&amp;token='Cla'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
            <title type="text">CatalogItemsThatContainATokenEntity</title>
            <updated>2014-10-29T11:48:19Z</updated>
            <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetCatalogItemsByToken?modelId='0dbb55fa-7f11-418d-8537-8ff2d9d1d9c6'&amp;token='Cla'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
            <content type="application/xml">
                <m:properties>
                    <d:Id m:type="Edm.String">2406E770-769C-4189-89DE-1C9283F93A96</d:Id>
                    <d:Name m:type="Edm.String">Clara Callan</d:Name>
                    <d:Rating m:type="Edm.Double">0</d:Rating>
                    <d:Reasoning m:type="Edm.String"></d:Reasoning>
                    <d:Metadata m:type="Edm.String"></d:Metadata>
                    <d:FormattedRating m:type="Edm.Double" m:null="true"></d:FormattedRating>
                </m:properties>
            </content>
        </entry>
    </feed>

##<a name="9-usage-data"></a>9. údaje o využití
###<a name="91-import-usage-data"></a>9.1. Import použití zásad správy informací
####<a name="911-uploading-file"></a>9.1.1. Nahrávání souborů
Tato část popisuje nahrát použití zásad správy informací pomocí souboru. Můžete volat toto rozhraní API s použití zásad správy informací. Pro všechny hovory budou uloženy všechny použití zásad správy informací.

| Metoda HTTP | IDENTIFIKÁTOR URI |
|:--------|:--------|
|PŘÍSPĚVEK     |`<rootURI>/ImportUsageFile?modelId=%27<modelId>%27&filename=%27<fileName>%27&apiVersion=%271.0%27`<br><br>Příklad:<br>`<rootURI>/ImportUsageFile?modelId=%27a658c626-2baa-43a7-ac98-f6ee26120a12%27&filename=%27ImplicitMatrix10_Guid_small.txt%27&apiVersion=%271.0%27`|

|   Název parametru  |   Platné hodnoty                        |
|:--------          |:--------                              |
|   modelId |   Jedinečný identifikátor modelu  |
| Název souboru | Textový identifikátor katalogu.<br>Jenom písmena (A-Z, z), číslice (0 – 9), spojovníky (--) a jsou povoleny podtržítka (_).<br>Maximální délka: 50 |
|   apiVersion      | 1.0 |
|||
| Žádost o textu | Údaje o využití. Formát:<br>`<User Id>,<Item Id>[,<Time>,<Event>]`<br><br><table><tr><th>Jméno</th><th>Povinné</th><th>Typ</th><th>Popis</th></tr><tr><td>Id uživatele</td><td>Ano</td><td>[A-z], [a-z], [0 – 9], [_] & #40; Podtržení & #41; [-] & #40; pomlčku & #41;<br> Maximální počet znaků: 255 </td><td>Jedinečný identifikátor uživatele.</td></tr><tr><td>Id položky</td><td>Ano</td><td>[A-z], [a-z], [0 – 9], [& #95;] & #40; Podtržení & #41; [-] & #40; pomlčku & #41;<br> Maximální délka: 50</td><td>Jedinečný identifikátor položky.</td></tr><tr><td>Čas</td><td>Ne</td><td>Datum ve formátu: YYYY/MM/ddTHH (například 2013/06/20T10:00:00)</td><td>Čas data.</td></tr><tr><td>Události</td><td>No; Pokud předávaných musíte také vložit datum</td><td>Jedna z následujících akcí:<br>• Klepnutím na tlačítko<br>• RecommendationClick<br>• AddShopCart<br>• RemoveShopCart<br>• Nákupu</td><td></td></tr></table><br>Maximální velikost souboru: 200MB<br><br>Příklad:<br><pre>149452 1b3d95e2-84e4-414c-bb38-be9cf461c347<br>6360 1b3d95e2-84e4-414c-bb38-be9cf461c347<br>50321 1b3d95e2-84e4-414c-bb38-be9cf461c347<br>71285 1b3d95e2-84e4-414c-bb38-be9cf461c347<br>224450 1b3d95e2-84e4-414c-bb38-be9cf461c347<br>236645 1b3d95e2-84e4-414c-bb38-be9cf461c347<br>107951 1b3d95e2-84e4-414c-bb38-be9cf461c347</pre> |

**Odpověď**:

Nastavit informace HTTP stavový kód: 200

- `Feed\entry\content\properties\LineCount`-Byla přijata počet řádků.
- `Feed\entry\content\properties\ErrorCount`-Počet řádků, které nebyly vkládat kvůli chybě.
- `Feed\entry\content\properties\FileId`-Souborů identifikátor.

OData XML

    <feed xmlns:base="https://api.datamarket.azure.com/amla/recommendations/v3/ImportUsageFile" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
    <title type="text" />
    <subtitle type="text">Add bulk usage data (usage file)</subtitle>
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/ImportUsageFile?modelId='a658c626-2baa-43a7-ac98-f6ee26120a12'&amp;filename='ImplicitMatrix10_Guid_small.txt'&amp;apiVersion='1.0'</id>
    <rights type="text" />
    <updated>2014-10-05T07:21:44Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/ImportUsageFile?modelId='a658c626-2baa-43a7-ac98-f6ee26120a12'&amp;filename='ImplicitMatrix10_Guid_small.txt'&amp;apiVersion='1.0'" />
    <entry>
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/ImportUsageFile?modelId='a658c626-2baa-43a7-ac98-f6ee26120a12'&amp;filename='ImplicitMatrix10_Guid_small.txt'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
    <title type="text">AddBulkUsageDataUsageFileEntity2</title>
    <updated>2014-10-05T07:21:44Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/ImportUsageFile?modelId='a658c626-2baa-43a7-ac98-f6ee26120a12'&amp;filename='ImplicitMatrix10_Guid_small.txt'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:LineCount m:type="Edm.String">33</d:LineCount>
        <d:ErrorCount m:type="Edm.String">0</d:ErrorCount>
        <d:FileId m:type="Edm.String">fead7c1c-db01-46c0-872f-65bcda36025d</d:FileId>
      </m:properties>
    </content>
    </entry>
    </feed>


####<a name="912-using-data-acquisition"></a>9.1.2. Použití získávání dat
Tato část popisuje odešlete událostí v reálném čase Azure počítače výukové doporučení, obvykle z webu.

| Metoda HTTP | IDENTIFIKÁTOR URI |
|:--------|:--------|
|PŘÍSPĚVEK     |`<rootURI>/AddUsageEvent?apiVersion=%271.0%27`|
|ZÁHLAVÍ   |`"Content-Type", "text/xml"`|

|   Název parametru  |   Platné hodnoty                        |
|:--------          |:--------                              |
|   apiVersion      | 1.0 |
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
                    <PurchaseItem>
                        <ItemId>ABBF8081-C5C0-4F09-9701-E1C7AC78304A</ItemId>
                        <Count>1</Count>
                    </PurchaseItem>
                    <PurchaseItem>
                        <ItemId>21BF8088-B6C0-4509-870C-11C0AC7F304B</ItemId>
                        <Count>3</Count>
                    </PurchaseItem>
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

###<a name="92-list-model-usage-files"></a>9.2. Seznam modelu použití souborů
Načte metadat všechny soubory použití modelu.
Použití souborů budou načtená jednu stránku najednou. Jednotlivé položky obsahuje 100 stránky. Pokud chcete získat položky na konkrétní indexu, můžete použít parametr $skip odata. Například pokud chcete získat položky od pozice 100, přidejte parametr $skip = 100 k žádosti o.

| Metoda HTTP | IDENTIFIKÁTOR URI |
|:--------|:--------|
|ZÍSKÁNÍ     |`<rootURI>/ListModelUsageFiles?forModelId=%27<model_id>%27&apiVersion=%271.0%27`<br><br>Příklad:<br>`<rootURI>/ListModelUsageFiles?forModelId=%270dbb55fa-7f11-418d-8537-8ff2d9d1d9c6%27&apiVersion=%271.0%27`|

|   Název parametru  |   Platné hodnoty                        |
|:--------          |:--------                              |
|   forModelId  |   Jedinečný identifikátor modelu |
|   apiVersion      | 1.0 |
|||
| Žádost o textu | ŽÁDNÁ |

**Odpověď**:

Nastavit informace HTTP stavový kód: 200

Odpověď na obsahuje jednu položku za použití soubor. Každá položka má následující údaje:

- `feed\entry\content\properties\Id`– ID použití souboru.
- `feed\entry\content\properties\Length`– Délka soubor použití v MB.
- `feed\entry\content\properties\DateModified`-Datum vytvoření souboru použití.
- `feed\entry\content\properties\UseInModel`– Jestli použití soubor je používán v modelu.

OData XML

    <feed xmlns:base="https://api.datamarket.azure.com/amla/recommendations/v3/ListModelUsageFiles" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
        <title type="text" />
        <subtitle type="text">Get a list of model's usage files info</subtitle>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/ListModelUsageFiles?forModelId='1c1110f8-7d9f-4c64-a807-4c9c5329993a'&amp;apiVersion='1.0'</id>
        <rights type="text" />
        <updated>2014-10-30T09:40:25Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/ListModelUsageFiles?forModelId='1c1110f8-7d9f-4c64-a807-4c9c5329993a'&amp;apiVersion='1.0'" />
        <entry>
            <id>https://api.datamarket.azure.com/amla/recommendations/v3/ListModelUsageFiles?forModelId='1c1110f8-7d9f-4c64-a807-4c9c5329993a'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
            <title type="text">GetAListOfModelsUsageFilesInfoEntity</title>
            <updated>2014-10-30T09:40:25Z</updated>
            <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/ListModelUsageFiles?forModelId='1c1110f8-7d9f-4c64-a807-4c9c5329993a'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
            <content type="application/xml">
                <m:properties>
                    <d:Id m:type="Edm.String">4c067b42-e975-4cb2-8c98-a6ab80ed6d63</d:Id>
                    <d:Length m:type="Edm.Double">0</d:Length>
                    <d:DateModified m:type="Edm.String">10/30/2014 9:19:57 AM</d:DateModified>
                    <d:UseInModel m:type="Edm.String">true</d:UseInModel>
                </m:properties>
            </content>
        </entry>
    <entry>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/ListModelUsageFiles?forModelId='1c1110f8-7d9f-4c64-a807-4c9c5329993a'&amp;apiVersion='1.0'&amp;$skip=1&amp;$top=1</id>
        <title type="text">GetAListOfModelsUsageFilesInfoEntity</title>
        <updated>2014-10-30T09:40:25Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/ListModelUsageFiles?forModelId='1c1110f8-7d9f-4c64-a807-4c9c5329993a'&amp;apiVersion='1.0'&amp;$skip=1&amp;$top=1" />
        <content type="application/xml">
            <m:properties>
                <d:Id m:type="Edm.String">3126d816-4e80-4248-8339-1ebbdb9d544d</d:Id>
                <d:Length m:type="Edm.Double">0.001</d:Length>
                <d:DateModified m:type="Edm.String">10/30/2014 9:21:44 AM</d:DateModified>
                <d:UseInModel m:type="Edm.String">true</d:UseInModel>
            </m:properties>
        </content>
    </entry>
</feed>

###<a name="93-get-usage-statistics"></a>9.3. Získání statistiky využívání
Získá statistiky využívání.

| Metoda HTTP | IDENTIFIKÁTOR URI |
|:--------|:--------|
|ZÍSKÁNÍ     |`<rootURI>/GetUsageStatistics?modelId=%27<modelId>%27& startDate=%27<date>%27&endDate=%27<date>%27&eventTypes=%27<types>%27&apiVersion=%271.0%27`<br><br>Příklad:<br>`<rootURI>/GetUsageStatistics?modelId=%271d20c34f-dca1-4eac-8e5d-f299e4e4ad66%27&startDate=%272014%2F10%2F17T00%3A00%3A00%27&endDate=%272014%2F11%2F16T00%3A00%3A00%27&eventTypes=%271%2C2%2C3%2C4%2C5%27&apiVersion=%271.0%27`|

|   Název parametru  |   Platné hodnoty                        |
|:--------          |:--------                              |
| modelId | Jedinečný identifikátor modelu  |
| startDate |   Počáteční datum. Formát: yyyy/MM/ddTHH |
| Koncové datum | Koncové datum. Formát: yyyy/MM/ddTHH |
| eventTypes |  Hodnoty oddělené řetězec typy událostí nebo hodnota null přesuňte všechny události  |
| apiVersion | 1.0 |
|||
| Žádost o textu | ŽÁDNÁ |

**Odpověď**:

Nastavit informace HTTP stavový kód: 200

Kolekci prvků klíč/hodnota. Každý z nich obsahuje součet událostí pro určitý typ události seskupené podle hodinu.

- `feed\entry[i]\content\properties\Key`-Obsahuje čas (seskupené podle hodin) a typ události.
- `feed\entry[i]\content\properties\Value`-Počet celkové událostí.

OData XML

    <feed xmlns:base="https://api.datamarket.azure.com/amla/recommendations/v3/GetUsageStatistics" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
        <title type="text" />
        <subtitle type="text">Get usage statistics</subtitle>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetUsageStatistics?modelId='1d20c34f-dca1-4eac-8e5d-f299e4e4ad66'&amp;startDate='2014/10/12T00:00:00'&amp;endDate='2014/11/10T00:00:00'&amp;eventTypes=''&amp;apiVersion='1.0'</id>
    <rights type="text" />
    <updated>2014-11-18T11:39:16Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetUsageStatistics?modelId='1d20c34f-dca1-4eac-8e5d-f299e4e4ad66'&amp;startDate='2014/10/12T00:00:00'&amp;endDate='2014/11/10T00:00:00'&amp;eventTypes=''&amp;apiVersion='1.0'" />
    <entry>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetUsageStatistics?modelId='1d20c34f-dca1-4eac-8e5d-f299e4e4ad66'&amp;startDate='2014/10/12T00:00:00'&amp;endDate='2014/11/10T00:00:00'&amp;eventTypes=''&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
        <title type="text">GetUsageStatisticsEntity</title>
        <updated>2014-11-18T11:39:16Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetUsageStatistics?modelId='1d20c34f-dca1-4eac-8e5d-f299e4e4ad66'&amp;startDate='2014/10/12T00:00:00'&amp;endDate='2014/11/10T00:00:00'&amp;eventTypes=''&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
        <content type="application/xml">
            <m:properties>
                <d:Event m:type="Edm.String">11/9/2014 8:00:00 AM;Click</d:Event>
                <d:Value m:type="Edm.String">5</d:Value>
            </m:properties>
        </content>
    </entry>
    <entry>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetUsageStatistics?modelId='1d20c34f-dca1-4eac-8e5d-f299e4e4ad66'&amp;startDate='2014/10/12T00:00:00'&amp;endDate='2014/11/10T00:00:00'&amp;eventTypes=''&amp;apiVersion='1.0'&amp;$skip=1&amp;$top=1</id>
        <title type="text">GetUsageStatisticsEntity</title>
        <updated>2014-11-18T11:39:16Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetUsageStatistics?modelId='1d20c34f-dca1-4eac-8e5d-f299e4e4ad66'&amp;startDate='2014/10/12T00:00:00'&amp;endDate='2014/11/10T00:00:00'&amp;eventTypes=''&amp;apiVersion='1.0'&amp;$skip=1&amp;$top=1" />
        <content type="application/xml">
            <m:properties>
                <d:Event m:type="Edm.String">11/9/2014 8:00:00 AM;RecommendationClick</d:Event>
                <d:Value m:type="Edm.String">10</d:Value>
            </m:properties>
        </content>
    </entry>
    <entry>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetUsageStatistics?modelId='1d20c34f-dca1-4eac-8e5d-f299e4e4ad66'&amp;startDate='2014/10/12T00:00:00'&amp;endDate='2014/11/10T00:00:00'&amp;eventTypes=''&amp;apiVersion='1.0'&amp;$skip=2&amp;$top=1</id>
        <title type="text">GetUsageStatisticsEntity</title>
        <updated>2014-11-18T11:39:16Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetUsageStatistics?modelId='1d20c34f-dca1-4eac-8e5d-f299e4e4ad66'&amp;startDate='2014/10/12T00:00:00'&amp;endDate='2014/11/10T00:00:00'&amp;eventTypes=''&amp;apiVersion='1.0'&amp;$skip=2&amp;$top=1" />
        <content type="application/xml">
            <m:properties>
                <d:Event m:type="Edm.String">11/1/2014 8:00:00 AM;RemoveShopCart</d:Event>
                <d:Value m:type="Edm.String">10</d:Value>
            </m:properties>
        </content>
    </entry>
    <entry>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetUsageStatistics?modelId='1d20c34f-dca1-4eac-8e5d-f299e4e4ad66'&amp;startDate='2014/10/12T00:00:00'&amp;endDate='2014/11/10T00:00:00'&amp;eventTypes=''&amp;apiVersion='1.0'&amp;$skip=3&amp;$top=1</id>
        <title type="text">GetUsageStatisticsEntity</title>
        <updated>2014-11-18T11:39:16Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetUsageStatistics?modelId='1d20c34f-dca1-4eac-8e5d-f299e4e4ad66'&amp;startDate='2014/10/12T00:00:00'&amp;endDate='2014/11/10T00:00:00'&amp;eventTypes=''&amp;apiVersion='1.0'&amp;$skip=3&amp;$top=1" />
        <content type="application/xml">
            <m:properties>
                <d:Event m:type="Edm.String">11/5/2014 8:00:00 AM;RemoveShopCart</d:Event>
                <d:Value m:type="Edm.String">10</d:Value>
            </m:properties>
        </content>
    </entry>
    </feed>

###<a name="94-get-usage-file-sample"></a>9.4. Získání ukázkový soubor použití
Použije první 2KB obsah souboru použití.

| Metoda HTTP | IDENTIFIKÁTOR URI |
|:--------|:--------|
|ZÍSKÁNÍ     |`<rootURI>/GetUsageFileSample?modelId=%27<modelId>%27& fileId=%27<fileId>%27&apiVersion=%271.0%27`<br><br>Příklad:<br>`<rootURI>/GetUsageFileSample?modelId=%271c1110f8-7d9f-4c64-a807-4c9c5329993a%27&fileId=%274c067b42-e975-4cb2-8c98-a6ab80ed6d63%27&apiVersion=%271.0%27`|

|   Název parametru  |   Platné hodnoty                        |
|:--------          |:--------                              |
| modelId | Jedinečný identifikátor modelu  |
| fileId |  Jedinečný identifikátor použití soubor modelu  |
| apiVersion | 1.0 |
|||
| Žádost o textu | ŽÁDNÁ |

**Odpověď**:

Nastavit informace HTTP stavový kód: 200

Ve formátu nezpracovanými text je vrácena odpověď:
<pre>
85526,2406E770-769C-4189-89DE-1C9283F93A96,2014/11/02T13:40:15,True,1 210926,2406E770-769C-4189-89DE-1C9283F93A96,2014/11/02T13:40:15,True,1 116866,2406E770-769C-4189-89DE-1C9283F93A96,2014/11/02T13:40:15,True,1 177458,2406E770-769C-4189-89DE-1C9283F93A96,2014/11/02T13:40:15,True,1 274004,21BF8088-B6C0-4509-870C-E1C7AC78304A,2014/11/02T13:40:15,True,1 123883,21BF8088-B6C0-4509-870C-E1C7AC78304A,2014/11/02T13:40:15,True,1 37712,21BF8088-B6C0-4509-870C-E1C7AC78304A,2014/11/02T13:40:15,True,1 152249,21BF8088-B6C0-4509-870C-E1C7AC78304A,2014/11/02T13:40:15,True,1 250948,21BF8088-B6C0-4509-870C-E1C7AC78304A,2014/11/02T13:40:15 , PRAVDA, 1 235588, 21BF8088-B6C0-4509-870C-E1C7AC78304A, 2014/11/02T13:40:15, PRAVDA, 1 158254, 21BF8088-B6C0-4509-870 C-E1C7AC78304A, 2014/11/02T13:40:15, PRAVDA, 1 271195, 21BF8088-B6C0-4509-870 C-E1C7AC78304A, 2014/11/02T13:40:15, PRAVDA, 1 141157, 21BF8088-B6C0-4509-870 C-E1C7AC78304A, 2014/11/02T13:40:15, PRAVDA, 1 171118, 3BB5CB44-D143-4BDD-A55C-443964BF4B23, 2014/11/02T13:40:15, PRAVDA, 1 225087, 3BB5CB44-D143-4BDD-A55C-443964BF4B23, 2014/11/02T13:40:15, PRAVDA, 1
</pre>


###<a name="95-get-model-usage-file"></a>9.5. Získejte soubor použití modelu
Obnoví úplný obsah souboru použití.

| Metoda HTTP | IDENTIFIKÁTOR URI |
|:--------|:--------|
|ZÍSKÁNÍ     |`<rootURI>/GetModelUsageFile?mid=%27<modelId>%27& fid=%27<fileId>%27&download=%27<download_value>%27&apiVersion=%271.0%27`<br><br>Příklad:<br>`<rootURI>/GetModelUsageFile?mid=%271c1110f8-7d9f-4c64-a807-4c9c5329993a%27&fid=%273126d816-4e80-4248-8339-1ebbdb9d544d%27&download=%271%27&apiVersion=%271.0%27`|

|   Název parametru  |   Platné hodnoty                        |
|:--------          |:--------                              |
| Funkce MID | Jedinečný identifikátor modelu  |
| FID | Jedinečný identifikátor použití soubor modelu |
| ke stažení | 1 |
| apiVersion | 1.0 |
|||
| Žádost o textu | ŽÁDNÁ |

**Odpověď**:

Nastavit informace HTTP stavový kód: 200

Ve formátu nezpracovanými text je vrácena odpověď:
<pre>
85526,2406E770-769C-4189-89DE-1C9283F93A96,2014/11/02T13:40:15,True,1 210926,2406E770-769C-4189-89DE-1C9283F93A96,2014/11/02T13:40:15,True,1 116866,2406E770-769C-4189-89DE-1C9283F93A96,2014/11/02T13:40:15,True,1 177458,2406E770-769C-4189-89DE-1C9283F93A96,2014/11/02T13:40:15,True,1 274004,21BF8088-B6C0-4509-870C-E1C7AC78304A,2014/11/02T13:40:15,True,1 123883,21BF8088-B6C0-4509-870C-E1C7AC78304A,2014/11/02T13:40:15,True,1 37712,21BF8088-B6C0-4509-870C-E1C7AC78304A,2014/11/02T13:40:15,True,1 152249,21BF8088-B6C0-4509-870C-E1C7AC78304A,2014/11/02T13:40:15,True,1 250948,21BF8088-B6C0-4509-870C-E1C7AC78304A,2014/11/02T13:40:15 ,True,1 235588,21BF8088-B6C0-4509-870C-E1C7AC78304A,2014/11/02T13:40:15,True,1 158254,21BF8088-B6C0-4509-870C-E1C7AC78304A,2014/11/02T13:40:15,True,1 271195,21BF8088-B6C0-4509-870C-E1C7AC78304A,2014/11/02T13:40:15,True,1 141157,21BF8088-B6C0-4509-870C-E1C7AC78304A,2014/11/02T13:40:15,True,1 171118,3BB5CB44-D143-4BDD-A55C-443964BF4B23,2014/11/02T13:40:15,True,1 225087,3BB5CB44-D143-4BDD-A55C-443964BF4B23,2014/11/02T13:40:15,True,1 244881,3BB5CB44-D143-4BDD-A55C-443964BF4B23,2014/11/02T13:40:15,True,1 50547,3BB5CB44-D143-4BDD-A55C-443964BF4B23,2014/11/02T13:40:15,True,1 213090,3BB5CB44-D143-4BDD-A55C-443964BF4B23,2014/11/02T13:40:15 ,True,1 260655,3BB5CB44-D143-4BDD-A55C-443964BF4B23,2014/11/02T13:40:15,True,1 72214,3BB5CB44-D143-4BDD-A55C-443964BF4B23,2014/11/02T13:40:15,True,1 189334,3BB5CB44-D143-4BDD-A55C-443964BF4B23,2014/11/02T13:40:15,True,1 36326,3BB5CB44-D143-4BDD-A55C-443964BF4B23,2014/11/02T13:40:15,True,1 189336,3BB5CB44-D143-4BDD-A55C-443964BF4B23,2014/11/02T13:40:15,True,1 189334,552A1940-21E4-4399-82BB-594B46D7ED54,2014/11/02T13:40:15,True,1 260655,552A1940-21E4-4399-82BB-594B46D7ED54,2014/11/02T13:40:15,True,1 162100,552A1940-21E4-4399-82BB-594B46D7ED54,2014/11/02T13:40:15,True,1 54946,552A1940-21E4-4399-82BB-594B46D7ED54,2014/11/02T13:40:15 , PRAVDA, 1 260965, 552A1940-21E4-4399-82BB-594B46D7ED54, 2014/11/02T13:40:15, PRAVDA, 1 102758, 552A1940-21E4-4399-82BB-594B46D7ED54, 2014/11/02T13:40:15, PRAVDA, 1 112602, 552A1940-21E4-4399-82BB-594B46D7ED54, 2014/11/02T13:40:15, PRAVDA, 1 163925, 552A1940-21E4-4399-82BB-594B46D7ED54, 2014/11/02T13:40:15, PRAVDA, 1 262998, 552A1940-21E4-4399-82BB-594B46D7ED54, 2014/11/02T13:40:15, PRAVDA, 1 144717, 552A1940-21E4-4399-82BB-594B46D7ED54, 2014/11/02T13:40:15, PRAVDA, 1
</pre>

###<a name="96-delete-usage-file"></a>9.6. Odstraňte soubor použití
Odstraní zadaném použití souboru modelu.

| Metoda HTTP | IDENTIFIKÁTOR URI |
|:--------|:--------|
|ODSTRANĚNÍ     |`<rootURI>/DeleteUsageFile?modelId=%27<modelId>%27&fileId=%27<fileId>%27&apiVersion=%271.0%27`<br><br>Příklad:<br>`<rootURI>/DeleteUsageFile?modelId=%270f86d698-d0f4-4406-a684-d13d22c47a73%27&fileId=%27f2e0b09d-be5c-46b2-9ac2-c7f622e5e1a5%27&apiVersion=%271.0%27`|

| Název parametru    |   Platné hodnoty                        |
|:--------          |:--------                              |
| modelId | Jedinečný identifikátor modelu  |
| fileId | Jedinečný identifikátor souboru odstraněno. |
| apiVersion | 1.0 |
|||
| Žádost o textu | ŽÁDNÁ |

**Odpověď**:

Nastavit informace HTTP stavový kód: 200


###<a name="97-delete-all-usage-files"></a>9.7. Odstraňte všechny soubory použití
Odstraní všechny soubory použití modelu.

| Metoda HTTP | IDENTIFIKÁTOR URI |
|:--------|:--------|
|ODSTRANĚNÍ     |`<rootURI>/DeleteAllUsageFiles?modelId=%27<modelId>%27&apiVersion=%271.0%27`<br><br>Příklad:<br>`<rootURI>/DeleteAllUsageFiles?modelId=%271c1110f8-7d9f-4c64-a807-4c9c5329993a%27&apiVersion=%271.0%27`|

| Název parametru    |   Platné hodnoty                        |
|:--------          |:--------                              |
| modelId | Jedinečný identifikátor modelu  |
| apiVersion | 1.0 |
|||
| Žádost o textu | ŽÁDNÁ |

**Odpověď**:

Nastavit informace HTTP stavový kód: 200

##<a name="10-features"></a>10. funkce
Tato část popisuje získat informace o funkci, jako je třeba importovaných funkce a jejich hodnoty, jejich pořadí a po přidělení toto pořadí. Funkce naimportují jako součást katalog dat a potom souvisí jejich pořadí po dokončení rank Tvůrce dotazů.
Funkce rank můžete změnit podle vzorek použití zásad správy informací a typ položky. Ale konzistentní používání/položek, pořadí by měl pouze malé kolísání.
Pořadí funkcí je nezáporné číslo. Číslo 0 znamená, že tato funkce nebyla zařazených jako (se stane, když vyvolat toto rozhraní API před dokončením první rank vytvořit). Datum niž byl atributy pořadí se nazývá aktuálnost skóre.

###<a name="101-get-features-info-for-last-rank-build"></a>10.1. Získat informace o funkcích (poslední Rank sestavení)
Načte informace o funkci, včetně jejich pořadí pro poslední úspěšné rank Tvůrce dotazů.

| Metoda HTTP | IDENTIFIKÁTOR URI |
|:--------|:--------|
|ZÍSKÁNÍ      |`<rootURI>/GetModelFeatures?modelId=%27<modelId>%27&samplingSize=%27<samplingSize>%27&apiVersion=%271.0%27`<br><br>Příklad:<br>`<rootURI>/GetModelFeatures?modelId=%271c1110f8-7d9f-4c64-a807-4c9c5329993a%27&samplingSize=10%27&apiVersion=%271.0%27`

| Název parametru    |   Platné hodnoty                        |
|:--------          |:--------                              |
| modelId | Jedinečný identifikátor modelu  |
|samplingSize| Počet hodnot a ty pak zahrnout pro jednotlivé funkce podle data obsažená v katalogu. <br/>Možné hodnoty jsou:<br> -1 - všechny vzorky. <br>0 - žádný odběr. <br>N - návrat N vzorků pro každý název funkce.|
| apiVersion | 1.0 |
|||
| Žádost o textu | ŽÁDNÁ |


**Odpověď**:

Nastavit informace HTTP stavový kód: 200

Odpověď obsahuje seznam položek informací funkce. Každá položka obsahuje:

- `feed/entry/content/m:properties/d:Name`-Název funkce.
- `feed/entry/content/m:properties/d:RankUpdateDate`-Datum niž pořadí byla přiřazená tuto funkci také Funkce aktuálnost skóre. Uplynulé datum (0001-01-01T00:00:00 ") znamená, že bylo provedeno žádné rank Tvůrce dotazů.
- `feed/entry/content/m:properties/d:Rank`– Funkce rank (plovoucí). Pořadí 2.0 a až se považuje za vhodné funkci.
- `feed/entry/content/m:properties/d:SampleValues`-Čárkou oddělený seznam hodnot až velikost odběr požadované.

OData XML

    <feed xmlns:base="https://api.datamarket.azure.com/amla/recommendations/v3/GetModelFeatures" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
    <title type="text" />
    <subtitle type="text">Get the features of a model</subtitle>
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetModelFeatures?modelId='f13ab2e8-b530-4aa1-86f7-2f4a24714765'&amp;samplingSize='10'&amp;apiVersion='1.0'</id>
    <rights type="text" />
    <updated>2015-01-08T13:15:02Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetModelFeatures?modelId='f13ab2e8-b530-4aa1-86f7-2f4a24714765'&amp;samplingSize='10'&amp;apiVersion='1.0'" />
    <entry>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetModelFeatures?modelId='f13ab2e8-b530-4aa1-86f7-2f4a24714765'&amp;samplingSize='10'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
        <title type="text">ModelFeaturesEntity</title>
        <updated>2015-01-08T13:15:02Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetModelFeatures?modelId='f13ab2e8-b530-4aa1-86f7-2f4a24714765'&amp;samplingSize='10'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
        <content type="application/xml">
            <m:properties>
                <d:Name m:type="Edm.String">Author</d:Name>
                <d:RankUpdateDate m:type="Edm.String">0001-01-01T00:00:00</d:RankUpdateDate>
                <d:Rank m:type="Edm.String">0</d:Rank>
                <d:SampleValues m:type="Edm.String">A. A. Attanasio, A. A. Milne, A. Bates, A. C. Bhaktivedanta Swami Prabhupada et al., A. C. Crispin, A. C. Doyle, A. C. H. Smith, A. E. Parker, A. J. Holt, A. J. Matthews</d:SampleValues>
            </m:properties>
        </content>
    </entry>
    <entry>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetModelFeatures?modelId='f13ab2e8-b530-4aa1-86f7-2f4a24714765'&amp;samplingSize='10'&amp;apiVersion='1.0'&amp;$skip=1&amp;$top=1</id>
        <title type="text">ModelFeaturesEntity</title>
        <updated>2015-01-08T13:15:02Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetModelFeatures?modelId='f13ab2e8-b530-4aa1-86f7-2f4a24714765'&amp;samplingSize='10'&amp;apiVersion='1.0'&amp;$skip=1&amp;$top=1" />
        <content type="application/xml">
            <m:properties>
                <d:Name m:type="Edm.String">Publisher</d:Name>
                <d:RankUpdateDate m:type="Edm.String">0001-01-01T00:00:00</d:RankUpdateDate>
                <d:Rank m:type="Edm.String">0</d:Rank>
                <d:SampleValues m:type="Edm.String">A. Mondadori, Abacus, Abacus Press, Abacus Uk, Abstract Studio, Acacia Press, Academy Chicago Publishers, Ace Books, ACE Charter, Actar</d:SampleValues>
            </m:properties>
        </content>
    </entry>
    <entry>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetModelFeatures?modelId='f13ab2e8-b530-4aa1-86f7-2f4a24714765'&amp;samplingSize='10'&amp;apiVersion='1.0'&amp;$skip=2&amp;$top=1</id>
        <title type="text">ModelFeaturesEntity</title>
        <updated>2015-01-08T13:15:02Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetModelFeatures?modelId='f13ab2e8-b530-4aa1-86f7-2f4a24714765'&amp;samplingSize='10'&amp;apiVersion='1.0'&amp;$skip=2&amp;$top=1"/>
        <contenttype="application/xml">
        <m:properties>
            <d:Name m:type="Edm.String">Year</d:Name>
            <d:RankUpdateDate m:type="Edm.String">0001-01-01T00:00:00</d:RankUpdateDate>
            <d:Rank m:type="Edm.String">0</d:Rank>
            <d:SampleValues m:type="Edm.String">0, 1920, 1926, 1927, 1929, 1930, 1932, 1942, 1943, 1946</d:SampleValues>
        </m:properties>
        </content>
    </entry>
</feed>


###<a name="102-get-features-info-for-specific-rank-build"></a>10.2. Získat informace o funkcích (pro konkrétní Rank sestavení)

Načte informace o funkci, včetně jejich pořadí pro konkrétní rank Tvůrce dotazů.

| Metoda HTTP | IDENTIFIKÁTOR URI |
|:--------|:--------|
|ZÍSKÁNÍ      |`<rootURI>/GetModelFeatures?modelId=%27<modelId>%27&samplingSize=%27<samplingSize>%27&rankBuildId=<rankBuildId>&apiVersion=%271.0%27`<br><br>Příklad:<br>`<rootURI>/GetModelFeatures?modelId=%271c1110f8-7d9f-4c64-a807-4c9c5329993a%27&samplingSize=10%27&rankBuildId=1000551&apiVersion=%271.0%27`

| Název parametru    |   Platné hodnoty    |
|:--------          |:--------          |
| modelId | Jedinečný identifikátor modelu  |
|samplingSize| Počet hodnot a ty pak zahrnout pro jednotlivé funkce podle data obsažená v katalogu.<br/> Možné hodnoty jsou:<br> -1 - všechny vzorky. <br>0 - žádný odběr. <br>N - návrat N vzorků pro každý název funkce.|
|rankBuildId| Jedinečný identifikátor rank sestavení nebo -1 pro poslední rank Tvůrce dotazů|
| apiVersion | 1.0 |
|||
| Žádost o textu | ŽÁDNÁ |


**Odpověď**:

Nastavit informace HTTP stavový kód: 200

Odpověď obsahuje seznam položek informací funkce. Každá položka obsahuje:

- `feed/entry/content/m:properties/d:Name`-Název funkce.
- `feed/entry/content/m:properties/d:RankUpdateDate`-Datum niž pořadí byla přiřazená tuto funkci také Funkce aktuálnost skóre. Uplynulé datum (0001-01-01T00:00:00 ") znamená, že bylo provedeno žádné rank Tvůrce dotazů.
- `feed/entry/content/m:properties/d:Rank`– Funkce rank (plovoucí). Pořadí 2.0 a až se považuje za vhodné funkci.
- `feed/entry/content/m:properties/d:SampleValues`-Čárkou oddělený seznam hodnot až velikost odběr požadované.

OData

    <feed xmlns:base="https://api.datamarket.azure.com/amla/recommendations/v3/GetModelFeatures" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
        <title type="text" />
        <subtitle type="text">Get the features of a model</subtitle>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetModelFeatures?modelId='f13ab2e8-b530-4aa1-86f7-2f4a24714765'&amp;samplingSize='10'&amp;rankBuildId=1000653&amp;apiVersion='1.0'</id>
        <rights type="text" />
        <updated>2015-01-08T13:54:22Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetModelFeatures?modelId='f13ab2e8-b530-4aa1-86f7-2f4a24714765'&amp;samplingSize='10'&amp;rankBuildId=1000653&amp;apiVersion='1.0'" />
        <entry>
            <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetModelFeatures?modelId='f13ab2e8-b530-4aa1-86f7-2f4a24714765'&amp;samplingSize='10'&amp;rankBuildId=1000653&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
            <title type="text">ModelFeaturesEntity</title>
            <updated>2015-01-08T13:54:22Z</updated>
            <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetModelFeatures?modelId='f13ab2e8-b530-4aa1-86f7-2f4a24714765'&amp;samplingSize='10'&amp;rankBuildId=1000653&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
            <content type="application/xml">
                <m:properties>
                    <d:Name m:type="Edm.String">Author</d:Name>
                    <d:RankUpdateDate m:type="Edm.String">2015-01-08T13:52:14.673</d:RankUpdateDate>
                    <d:Rank m:type="Edm.String">3.38867426</d:Rank>
                    <d:SampleValues m:type="Edm.String">A. A. Attanasio, A. A. Milne, A. Bates, A. C. Bhaktivedanta Swami Prabhupada et al., A. C. Crispin, A. C. Doyle, A. C. H. Smith, A. E. Parker, A. J. Holt, A. J. Matthews</d:SampleValues>
                </m:properties>
            </content>
        </entry>
        <entry>
            <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetModelFeatures?modelId='f13ab2e8-b530-4aa1-86f7-2f4a24714765'&amp;samplingSize='10'&amp;rankBuildId=1000653&amp;apiVersion='1.0'&amp;$skip=1&amp;$top=1</id>
            <title type="text">ModelFeaturesEntity</title>
            <updated>2015-01-08T13:54:22Z</updated>
            <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetModelFeatures?modelId='f13ab2e8-b530-4aa1-86f7-2f4a24714765'&amp;samplingSize='10'&amp;rankBuildId=1000653&amp;apiVersion='1.0'&amp;$skip=1&amp;$top=1" />
            <content type="application/xml">
                <m:properties>
                    <d:Name m:type="Edm.String">Publisher</d:Name>
                    <d:RankUpdateDate m:type="Edm.String">2015-01-08T13:52:14.673</d:RankUpdateDate>
                    <d:Rank m:type="Edm.String">1.67839336</d:Rank>
                    <d:SampleValues m:type="Edm.String">A. Mondadori, Abacus, Abacus Press, Abacus Uk, Abstract Studio, Acacia Press, Academy Chicago Publishers, Ace Books, ACE Charter, Actar</d:SampleValues>
                </m:properties>
            </content>
        </entry>
        <entry>
            <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetModelFeatures?modelId='f13ab2e8-b530-4aa1-86f7-2f4a24714765'&amp;samplingSize='10'&amp;rankBuildId=1000653&amp;apiVersion='1.0'&amp;$skip=2&amp;$top=1</id>
            <title type="text">ModelFeaturesEntity</title>
            <updated>2015-01-08T13:54:22Z</updated>
            <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetModelFeatures?modelId='f13ab2e8-b530-4aa1-86f7-2f4a24714765'&amp;samplingSize='10'&amp;rankBuildId=1000653&amp;apiVersion='1.0'&amp;$skip=2&amp;$top=1" />
            <content type="application/xml">
                <m:properties>
                    <d:Name m:type="Edm.String">Year</d:Name>
                    <d:RankUpdateDate m:type="Edm.String">2015-01-08T13:52:14.673</d:RankUpdateDate>
                    <d:Rank m:type="Edm.String">1.12352145</d:Rank>
                    <d:SampleValues m:type="Edm.String">0, 1920, 1926, 1927, 1929, 1930, 1932, 1942, 1943, 1946</d:SampleValues>
                </m:properties>
            </content>
        </entry>
    </feed>


##<a name="11-build"></a>11. sestavení

  Tato část popisuje různé rozhraní API vztahující se k sestavení. Existují 3 typy buildy: sestavení doporučení, pořadí a FBT (často koupili společně) sestavení.

Sestavení doporučení slouží k vygenerování modelu doporučení pro předpovědí. Předpovědí (pro tento typ sestavení) pocházet ve dvou provedeních:
* I2I - také Doporučení položkami - příslušné položky nebo seznam položek tuto možnost, budou předpovídat seznam položek, které by mohly být vysoké potřebné.
* U2I - také Uživatel položku doporučení: vzhledem k tomu, id uživatele (a volitelně seznam položek) tuto možnost, bude předpovídání seznam položek, které by mohly být vysoké potřebné pro daného uživatele (a další volbu položky). Doporučení U2I vycházejí historie položky, které jsou potřebné pro uživatele až do doby, které tvořily původní modelu.

Rank Tvůrce dotazů je technické Tvůrce dotazů, můžete se naučit používat použitelnost funkce. Má obvykle, abyste mohli získávat nejlepších výsledků modelu doporučení týkající se funkcí, proveďte následující kroky:
- Aktivace rank sestavení (Pokud je výsledek funkce stabilní) a počkejte, dokud se zobrazí výsledek funkce.
- Získat pořadí funkce tak, že zavoláte [Získat informace o funkcích](#101-get-features-info-for-last-rank-build) rozhraní API.
- Pomocí následujících parametrů konfigurace sestavení doporučení:
    - `useFeatureInModel`-Nastavena na hodnotu True.
    - `ModelingFeatureList`– Nastavte na hodnoty oddělené seznam funkcí se skóre 2.0 nebo více (podle rozměrů obnovená v předchozím kroku).
    - `AllowColdItemPlacement`-Nastavena na hodnotu True.
    - Volitelně můžete nastavit `EnableFeatureCorrelation` true (pravda) a `ReasoningFeatureList` do seznamu funkcí, kterou chcete použít pro vysvětlení (obvykle stejného seznamu funkcí používaných v modelování nebo dílčí seznam).
- Aktivace sestavení doporučení s nakonfigurovanou parametry.

Poznámka: Pokud není konfigurace všechny parametry (například vyvolání Tvůrce dotazů doporučení bez parametrů) nebo výslovně nezakážete použití funkce (například `UseFeatureInModel` nastavena na hodnotu False), systém nastavit parametry souvisejících s funkcí nad vysvětleno hodnoty v případě, že existuje rank Tvůrce dotazů.

Neexistuje žádná omezení spouštění pořadí a doporučení sestavení souběžně pro stejný model. Však se nedá spustit na stejný model současně dvě buildy stejného typu.

Sestavení FBT (často koupili společně) ještě další doporučení algoritmus s názvem někdy "konzervativní" doporučení, což je vhodné pro katalogy, které nejsou povahy (homogenní: publikace, videa, některé jídla dotazování; nestejnorodého: počítač a zařízení, doménami, vysoce různorodého).

Poznámka: Pokud použití soubory, které jste nahráli obsahují volitelné pole "typ události" pak pro FBT modelování pouze "Nákup" události se použijí. Pokud žádný typ události je, že všechny události se považují nákup.


####<a name="111-build-parameters"></a>11.1 sestavit parametry

Každý typ Tvůrce dotazů je možné konfigurovat pomocí sadu parametrů (je vidět níže). Pokud nechcete nakonfigurovat parametry, systému automaticky atribut hodnoty parametrů podle informace v době aktivovat sestavení.

#####<a name="1111-usage-condenser"></a>11.1.1. Použití chladič
Uživatelé nebo položek s několik bodů použití může obsahovat více šum než informace. Jestli chcete systém pokusí se předpovídání minimální počet bodů použití za uživatele a položka se nemusí používat v modelu. Toto číslo bude nacházet v oblasti definované parametry ItemCutoffLowerBound a ItemCutoffUpperBound položek a oblasti definované parametry UserCutOffLowerBound a UserCutoffUpperBound pro uživatele. Chladič neovlivní položky nebo uživatelům můžete minimalizovat nastavením alespoň jedno z odpovídající hranice na nulu.

#####<a name="1112-rank-build-parameters"></a>11.1.2. Pořadí sestavit parametry

Následující tabulka popisuje sestavení parametry rank Tvůrce dotazů.

|Klíč|Popis|Typ|Platná hodnota|
|:-----|:----|:----|:---|
|NumberOfModelIterations | Celkový čas výpočetním a přesnost modelu se projeví počtu iterací, které provede modelu. Vyšší čísla, zlepšit přesnost se zobrazí, ale časového využití bude trvat déle.| Celé číslo | 10 – 50 |
| NumberOfModelDimensions | Počet rozměry se týká počet modelu pokusí se najít v rámci dat funkcí. Zvýšení počtu rozměry vám umožní lepší optimalizaci výsledků do menší clusterů. Však příliš mnoho rozměry zabrání modelu nalézt korelace mezi položkami. | Celé číslo | 10 – 40 |
|ItemCutOffLowerBound| Definuje dolní mez položky pro chladiče. V tématu Použití chladič výše. | Celé číslo | 2 nebo víc (0 zakázat chladič) |
|ItemCutOffUpperBound| Definuje horní mez položky pro chladiče. V tématu Použití chladič výše. | Celé číslo | 2 nebo víc (0 zákaz chladič) |
|UserCutOffLowerBound| Definuje dolní mez uživatele pro chladiče. V tématu Použití chladič výše. | Celé číslo | 2 nebo víc (0 zakázat chladič) |
|UserCutOffUpperBound| Definuje horní mez uživatele pro chladiče. V tématu Použití chladič výše. | Celé číslo | 2 nebo víc (0 zakázat chladič) |

#####<a name="1113-recommendation-build-parameters"></a>11.1.3. Doporučení sestavení parametry
Následující tabulka popisuje sestavení parametry sestavení doporučení.

|Klíč|Popis|Typ|Platná hodnota|
|:-----|:----|:----|:---|
|NumberOfModelIterations | Celkový čas výpočetním a přesnost modelu se projeví počtu iterací, které provede modelu. Vyšší čísla, zlepšit přesnost se zobrazí, ale časového využití bude trvat déle.| Celé číslo | 10 – 50 |
| NumberOfModelDimensions | Počet rozměry se týká počet modelu pokusí se najít v rámci dat funkcí. Zvýšení počtu rozměry vám umožní lepší optimalizaci výsledků do menší clusterů. Však příliš mnoho rozměry zabrání modelu nalézt korelace mezi položkami. | Celé číslo | 10 – 40 |
|ItemCutOffLowerBound| Definuje dolní mez položky pro chladiče. V tématu Použití chladič výše. | Celé číslo | 2 nebo víc (0 zákaz chladič) |
|ItemCutOffUpperBound| Definuje horní mez položky pro chladiče. V tématu Použití chladič výše. | Celé číslo | 2 nebo víc (0 zakázat chladič) |
|UserCutOffLowerBound| Definuje dolní mez uživatele pro chladiče. V tématu Použití chladič výše. | Celé číslo | 2 nebo víc (0 zakázat chladič) |
|UserCutOffUpperBound| Definuje horní mez uživatele pro chladiče. V tématu Použití chladič výše. | Celé číslo | 2 nebo víc (0 zakázat chladič) |
| Popis | Vytvořte popis. | Řetězec | Libovolný text, maximální 512 znaků |
| EnableModelingInsights | Umožňuje výpočet metriky na modelu doporučení. | Logická hodnota | True nebo False |
| UseFeaturesInModel | Označuje, pokud funkce se dají použít k rozšíření modelu doporučení. | Logická hodnota | True nebo False |
| ModelingFeatureList | Hodnoty oddělené seznam názvů funkcí pro použití v dialogu sestavení doporučení s cílem zdokonalit doporučení. | Řetězec | Funkce názvy, aby byl 512 znaků |
| AllowColdItemPlacement | Označuje, pokud doporučení by měl taky nabízená studenou položky pomocí funkce podobnosti. | Logická hodnota | True nebo False |
| EnableFeatureCorrelation | Označuje, pokud funkce se dají použít v důvody. | Logická hodnota | True nebo False |
| ReasoningFeatureList | Hodnoty oddělené seznam názvů funkcí pro důvody vět (například doporučení vysvětlení).  | Řetězec | Funkce názvy, aby byl 512 znaků |
| EnableU2I | Povolit přizpůsobených doporučení také U2I (uživateli doporučení položky). | Logická hodnota | True nebo False (výchozí PRAVDA) |

#####<a name="1114-fbt-build-parameters"></a>11.1.4. Parametry FBT Tvůrce dotazů
Následující tabulka popisuje sestavení parametry sestavení doporučení.

|Klíč|Popis|Typ|Platná hodnota (výchozí)|
|:-----|:----|:----|:---|
|FbtSupportThreshold | Jak konzervativní se model. Počet výskytů dalších položek pro modelování.| Celé číslo | 3 – 50 (6). |
|FbtMaxItemSetSize | Počet položek v sadě časté Bounds.| Celé číslo | 2-3 (2) |
|FbtMinimalScore | Minimální skóre, které často sady by měla být chcete být součástí vrácených výsledků. Vyšší lepší.| Dvojité | 0 a větší než (0) |
|FbtSimilarityFunction | Definuje funkce podobnosti pro použití v dialogu sestavení. Výtah upřednostňuje serendipity, spolu výskyt upřednostňuje předvídatelnost a Jaccard je hodní narušení zabezpečení mezi nimi. | Řetězec | cooccurrence, výtah, jaccard (zdviž) |


###<a name="112-trigger-a-recommendation-build"></a>11.2. Aktivace sestavení doporučení

  Ve výchozím nastavení spustí toto rozhraní API sestavení modelu doporučení. Aktivovat rank Tvůrce dotazů (abyste mohli skóre funkcí), bude použito varianty rozhraní API sestavení s parametrem typ Tvůrce dotazů.


| Metoda HTTP | IDENTIFIKÁTOR URI |
|:--------|:--------|
|PŘÍSPĚVEK     |`<rootURI>/BuildModel?modelId=%27<modelId>%27&userDescription=%27<description>%27&apiVersion=%271.0%27`<br><br>Příklad:<br>`<rootURI>/BuildModel?modelId=%27a658c626-2baa-43a7-ac98-f6ee26120a12%27&userDescription=%27First%20build%27&apiVersion=%271.0%27`|
|ZÁHLAVÍ   |`"Content-Type", "text/xml"`(Pokud odeslání žádosti o textu)|

|   Název parametru  |   Platné hodnoty                        |
|:--------          |:--------                              |
| modelId | Jedinečný identifikátor modelu  |
| userDescription | Textový identifikátor katalogu. Všimněte si, že používáte mezery byste musí kódovat s % 20 místo. Viz příklad nahoře.<br>Maximální délka: 50 |
| apiVersion | 1.0 |
|||
| Žádost o textu | Pokud prázdné bude sestavení spouštět s výchozími sestavení parametry.<br><br>Pokud chcete nastavit parametry Tvůrce dotazů, poslat parametrů ve formátu XML do textu jako v následujícím příkladu. (Viz oddíl "Sestavit parametry" Vysvětlení parametrů.)`<NumberOfModelIterations>40</NumberOfModelIterations><NumberOfModelDimensions>20</NumberOfModelDimensions><MinItemAppearance>5</MinItemAppearance><MinUserAppearance>5</MinUserAppearance><EnableModelingInsights>true</EnableModelingInsights><UseFeaturesInModel>false</UseFeaturesInModel><ModelingFeatureList>feature_name_1,feature_name_2,...</ModelingFeatureList><AllowColdItemPlacement>false</AllowColdItemPlacement><EnableFeatureCorrelation>false</EnableFeatureCorrelation><ReasoningFeatureList>feature_name_a,feature_name_b,...</ReasoningFeatureList></BuildParametersList>` |

**Odpověď**:

Nastavit informace HTTP stavový kód: 200

Toto je asynchronní rozhraní API. Zobrazí se Tvůrce dotazů ID jako odpověď. Se dozvíte po ukončení sestavení by měl volání rozhraní API "Získat vytvoří stav modelem" a najděte toto ID tvůrce dotazů v odpovědi. Všimněte si, že sestavení může trvat minut hodin v závislosti na velikosti data.

Nelze používat doporučení pokladny sestavení má na konci.

Stav platné sestavení:

- Vytvoření – vytvořit žádost o byl vytvořen.
- Ve frontě - sestavení žádost odešla a je ve frontě.
- Probíhá stavební - Tvůrce dotazů.
- Úspěch - buildu ukončena úspěšně.
- Chyba: sestavení ukončí s se nepovede.
- Zrušená - sestavení uživatel ho zrušil.
- Zrušení - zrušit žádost pro sestavení odešla.


Všimněte si, že ID tvůrce dotazů naleznete v následujícím umístění:`Feed\entry\content\properties\Id`

OData XML

    <feed xmlns:base="https://api.datamarket.azure.com/amla/recommendations/v3/BuildModel" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
    <title type="text" />
    <subtitle type="text">Build a Model with RequestBody</subtitle>
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/BuildModel?modelId='9559872f-7a53-4076-a3c7-19d9385c1265'&amp;userDescription='First build'&amp;apiVersion='1.0'</id>
    <rights type="text" />
    <updated>2014-10-05T08:56:34Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/BuildModel?modelId='9559872f-7a53-4076-a3c7-19d9385c1265'&amp;userDescription='First%20build'&amp;apiVersion='1.0'" />
    <entry>
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/BuildModel?modelId='9559872f-7a53-4076-a3c7-19d9385c1265'&amp;userDescription='First build'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
    <title type="text">BuildAModelEntity2</title>
    <updated>2014-10-05T08:56:34Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/BuildModel?modelId='9559872f-7a53-4076-a3c7-19d9385c1265'&amp;userDescription='First%20build'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
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

###<a name="113-trigger-build-recommendation-rank-or-fbt"></a>11.3. Sestavení aktivační události (doporučení, pořadí nebo FBT)

| Metoda HTTP | IDENTIFIKÁTOR URI |
|:--------|:--------|
|PŘÍSPĚVEK     |`<rootURI>/BuildModel?modelId=%27<modelId>%27&userDescription=%27<description>%27&buildType=%27<buildType>%27&apiVersion=%271.0%27`<br><br>Příklad:<br>`<rootURI>/BuildModel?modelId=%27a658c626-2baa-43a7-ac98-f6ee26120a12%27&userDescription=%27First%20build%27&buildType=%27Ranking%27&apiVersion=%271.0%27`|
|ZÁHLAVÍ   |`"Content-Type", "text/xml"`(Pokud odeslání žádosti o textu)|

|   Název parametru  |   Platné hodnoty                        |
|:--------          |:--------                              |
| modelId | Jedinečný identifikátor modelu  |
| userDescription | Textový identifikátor katalogu. Všimněte si, že používáte mezery byste musí kódovat s % 20 místo. Viz příklad nahoře.<br>Maximální délka: 50 |
| buildType | Typ sestavení vyvolat: <br/> -"Doporučení" pro sestavení doporučení <br> -Řazení pro rank Tvůrce dotazů <br/> -"Fbt" pro FBT sestavení
| apiVersion | 1.0 |
|||
| Žádost o textu | Pokud prázdné bude sestavení spouštět s výchozími sestavení parametry.<br><br>Pokud chcete nastavit sestavení parametry, pošlete jim ve formátu XML do textu jako v následujícím příkladu. (Viz oddíl "Sestavit parametry" vysvětlení a úplný seznam parametrů.)`<BuildParametersList><NumberOfModelIterations>40</NumberOfModelIterations><NumberOfModelDimensions>20</NumberOfModelDimensions><MinItemAppearance>5</MinItemAppearance><MinUserAppearance>5</MinUserAppearance></BuildParametersList>` |

**Odpověď**:

Nastavit informace HTTP stavový kód: 200

Toto je asynchronní rozhraní API. Zobrazí se Tvůrce dotazů ID jako odpověď. Se dozvíte po ukončení sestavení by měl volání rozhraní API "Získat vytvoří stav modelem" a najděte toto ID tvůrce dotazů v odpovědi. Všimněte si, že sestavení může trvat minut hodin v závislosti na velikosti data.

Nelze používat doporučení pokladny sestavení má na konci.

Stav platné sestavení:

- Vytvoření – Model byl vytvořen.
- Ve frontě - modelu sestavení spuštěná a je ve frontě.
- Vytváření - modelu je vytvářeného.
- Úspěch - buildu ukončena úspěšně.
- Chyba: sestavení ukončí s se nepovede.
- Zrušená - sestavení uživatel ho zrušil.
- Zrušení - zrušená Tvůrce dotazů.

Všimněte si, že ID tvůrce dotazů naleznete v následujícím umístění:`Feed\entry\content\properties\Id`

OData XML

    <feed xmlns:base="https://api.datamarket.azure.com/amla/recommendations/v3/BuildModel" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
    <title type="text" />
    <subtitle type="text">Build a Model with RequestBody</subtitle>
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/BuildModel?modelId='9559872f-7a53-4076-a3c7-19d9385c1265'&amp;userDescription='First build'&amp;apiVersion='1.0'</id>
    <rights type="text" />
    <updated>2014-10-05T08:56:34Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/BuildModel?modelId='9559872f-7a53-4076-a3c7-19d9385c1265'&amp;userDescription='First%20build'&amp;apiVersion='1.0'" />
    <entry>
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/BuildModel?modelId='9559872f-7a53-4076-a3c7-19d9385c1265'&amp;userDescription='First build'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
    <title type="text">BuildAModelEntity2</title>
    <updated>2014-10-05T08:56:34Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/BuildModel?modelId='9559872f-7a53-4076-a3c7-19d9385c1265'&amp;userDescription='First%20build'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
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




###<a name="114-get-builds-status-of-a-model"></a>11.4. Stav Buildy modelu
Načte buildy a jejich stav pro zadaný model.

| Metoda HTTP | IDENTIFIKÁTOR URI |
|:--------|:--------|
|ZÍSKÁNÍ     |`<rootURI>/GetModelBuildsStatus?modelId=%27<modelId>%27&onlyLastBuild=<bool>&apiVersion=%271.0%27`<br><br>Příklad:<br>`<rootURI>/GetModelBuildsStatus?modelId=%279559872f-7a53-4076-a3c7-19d9385c1265%27&onlyLastBuild=true&apiVersion=%271.0%27`|


|   Název parametru  |   Platné hodnoty                        |
|:--------          |:--------                              |
|   modelId         |   Jedinečný identifikátor modelu  |
|   onlyLastBuild   |   Označuje, zda chcete vrátit historii sestavení modelu nebo jenom stav na nejnovější verzi  |
|   apiVersion      |   1.0                                 |


**Odpověď**:

Nastavit informace HTTP stavový kód: 200

Odpověď na obsahuje jednu položku za Tvůrce dotazů. Každá položka má následující údaje:

- `feed/entry/content/properties/UserName`– Jméno uživatele.
- `feed/entry/content/properties/ModelName`– Název modelu.
- `feed/entry/content/properties/ModelId`-Modelu jedinečný identifikátor.
- `feed/entry/content/properties/IsDeployed`– Jestli (také nasazení sestavení aktivní sestavení).
- `feed/entry/content/properties/BuildId`-Vytvořte jedinečný identifikátor.
- `feed/entry/content/properties/BuildType`– Typ sestavení.
- `feed/entry/content/properties/Status`– Vytvoření stav. Může být jeden z těchto věcí: Chyba budovy, ve frontě, Cancelling, zrušeno, úspěch.
- `feed/entry/content/properties/StatusMessage`-Zpráva podrobný stav (platí jenom pro konkrétní států).
- `feed/entry/content/properties/Progress`– Vytvoření průběh (%).
- `feed/entry/content/properties/StartTime`-Čas zahájení Tvůrce dotazů.
- `feed/entry/content/properties/EndTime`– Vytvoření koncového času.
- `feed/entry/content/properties/ExecutionTime`– Vytvoření doby trvání.
- `feed/entry/content/properties/ProgressStep`– Podrobnosti o aktuálního stupeň sestavení probíhá.

Stav platné sestavení:
- Vytvořili – vytvořit žádost o byla vytvořena.
- Ve frontě - spouštěný žádost Tvůrce dotazů a je ve frontě.
- Probíhá stavební - Tvůrce dotazů.
- Úspěch - buildu ukončena úspěšně.
- Chyba: sestavení ukončí s se nepovede.
- Zrušená - sestavení uživatel ho zrušil.
- Zrušení - zrušená Tvůrce dotazů.

Platné hodnoty pro typ Tvůrce dotazů:
- Pořadí – vytvoření pořadí.
- Doporučení: sestavení doporučení.


OData XML

    <feed xmlns:base="https://api.datamarket.azure.com/amla/recommendations/v3/GetModelBuildsStatus" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
        <title type="text" />
        <subtitle type="text">Get builds status of a model</subtitle>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetModelBuildsStatus?modelId='1d20c34f-dca1-4eac-8e5d-f299e4e4ad66'&amp;onlyLastBuild=False&amp;apiVersion='1.0'</id>
        <rights type="text" />
        <updated>2014-11-05T17:51:10Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetModelBuildsStatus?modelId='1d20c34f-dca1-4eac-8e5d-f299e4e4ad66'&amp;onlyLastBuild=False&amp;apiVersion='1.0'" />
        <entry>
            <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetModelBuildsStatus?modelId='1d20c34f-dca1-4eac-8e5d-f299e4e4ad66'&amp;onlyLastBuild=False&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
            <title type="text">GetBuildsStatusEntity</title>
            <updated>2014-11-05T17:51:10Z</updated>
            <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetModelBuildsStatus?modelId='1d20c34f-dca1-4eac-8e5d-f299e4e4ad66'&amp;onlyLastBuild=False&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
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


###<a name="115-get-builds-status"></a>11.5. Sestavení stav
Načte vytvářet stavy všech modelů uživatele.

| Metoda HTTP | IDENTIFIKÁTOR URI |
|:--------|:--------|
|ZÍSKÁNÍ     |`<rootURI>/GetUserBuildsStatus?onlyLastBuilds=<bool>&apiVersion=%271.0%27`<br><br>Příklad:<br>`<rootURI>/GetUserBuildsStatus?onlyLastBuilds=true&apiVersion=%271.0%27`|


|   Název parametru  |   Platné hodnoty                        |
|:--------          |:--------                              |
|   onlyLastBuild   |   Označuje, zda chcete vrátit historii sestavení modelu nebo jenom stav na nejnovější verzi. |
|   apiVersion      |   1.0                                 |


**Odpověď**:

Nastavit informace HTTP stavový kód: 200

Odpověď na obsahuje jednu položku za Tvůrce dotazů. Každá položka má následující údaje:

- `feed/entry/content/properties/UserName`– Jméno uživatele.
- `feed/entry/content/properties/ModelName`– Název modelu.
- `feed/entry/content/properties/ModelId`-Modelu jedinečný identifikátor.
- `feed/entry/content/properties/IsDeployed`– Jestli sestavení nasazení.
- `feed/entry/content/properties/BuildId`-Vytvořte jedinečný identifikátor.
- `feed/entry/content/properties/BuildType`– Typ sestavení.
- `feed/entry/content/properties/Status`– Vytvoření stav. Může být jeden z těchto věcí: Chyba budovy, ve frontě, zrušeno, Cancelling, úspěch.
- `feed/entry/content/properties/StatusMessage`-Zpráva podrobný stav (platí jenom pro konkrétní států).
- `feed/entry/content/properties/Progress`– Vytvoření průběh (%).
- `feed/entry/content/properties/StartTime`-Čas zahájení Tvůrce dotazů.
- `feed/entry/content/properties/EndTime`– Vytvoření koncového času.
- `feed/entry/content/properties/ExecutionTime`– Vytvoření doby trvání.
- `feed/entry/content/properties/ProgressStep`– Podrobnosti o aktuálního stupeň sestavení probíhá.

Stav platné sestavení:
- Vytvořili – vytvořit žádost o byla vytvořena.
- Ve frontě - spouštěný žádost Tvůrce dotazů a je ve frontě.
- Probíhá stavební - Tvůrce dotazů.
- Úspěch - buildu ukončena úspěšně.
- Chyba: sestavení ukončí s se nepovede.
- Zrušená - sestavení uživatel ho zrušil.
- Zrušení - zrušená Tvůrce dotazů.


Platné hodnoty pro typ Tvůrce dotazů:
- Pořadí – vytvoření pořadí.
- Doporučení: sestavení doporučení.


OData XML

    <feed xmlns:base="https://api.datamarket.azure.com/amla/recommendations/v3/GetUserBuildsStatus" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
        <title type="text" />
        <subtitle type="text">Get builds status of a user</subtitle>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetUserBuildsStatus?onlyLastBuilds=False&amp;apiVersion='1.0'</id>
        <rights type="text" />
        <updated>2014-11-05T18:41:21Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetUserBuildsStatus?onlyLastBuilds=False&amp;apiVersion='1.0'" />
        <entry>
            <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetUserBuildsStatus?onlyLastBuilds=False&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
            <title type="text">GetBuildsStatusEntity</title>
            <updated>2014-11-05T18:41:21Z</updated>
            <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetUserBuildsStatus?onlyLastBuilds=False&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
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


###<a name="116-delete-build"></a>11.6. Odstranění Tvůrce dotazů
Odstraní sestavení.

POZNÁMKA: <br>Nelze odstranit aktivní Tvůrce dotazů. Model třeba aktualizovat na jiný aktivní vybudování před odstraněním.<br>V průběhu sestavení nelze odstranit. Sestavení by měl nejdřív zrušit tak, že zavoláte <strong>Zrušit vytvořit</strong>.

| Metoda HTTP | IDENTIFIKÁTOR URI |
|:--------|:--------|
|ODSTRANĚNÍ     |`<rootURI>/DeleteBuild?buildId=%27<buildId>%27&apiVersion=%271.0%27`<br><br>Příklad:<br>`<rootURI>/DeleteBuild?buildId=%271500068%27&apiVersion=%271.0%27`|

|   Název parametru  |   Platné hodnoty                        |
|:--------          |:--------                              |
| buildId | Jedinečný identifikátor sestavení. |
| apiVersion | 1.0 |

**Odpověď:**

Nastavit informace HTTP stavový kód: 200

###<a name="117-cancel-build"></a>11.7. Zrušení Tvůrce dotazů
Zruší Tvůrce dotazů, které je při vytváření stav.

| Metoda HTTP | IDENTIFIKÁTOR URI |
|:--------|:--------|
|UMÍSTĚNÍ     |`<rootURI>/CancelBuild?buildId=%27<buildId>%27&apiVersion=%271.0%27`<br><br>Příklad:<br>`<rootURI>/CancelBuild?buildId=%271500076%27&apiVersion=%271.0%27`|

|   Název parametru  |   Platné hodnoty                        |
|:--------          |:--------                              |
| buildId | Jedinečný identifikátor sestavení. |
| apiVersion | 1.0 |

**Odpověď:**

Nastavit informace HTTP stavový kód: 200

###<a name="118-get-build-parameters"></a>11.8. Získání parametry Tvůrce dotazů
Načte vytvářet parametry.

| Metoda HTTP | IDENTIFIKÁTOR URI |
|:--------|:--------|
|ZÍSKÁNÍ     |`<rootURI>/GetBuildParameters?buildId=%27<buildId>%27&apiVersion=%271.0%27`<br><br>Příklad:<br>`<rootURI>/GetBuildParameters?buildId=%271000653%27&apiVersion=%271.0%27`|

|   Název parametru  |   Platné hodnoty                        |
|:--------          |:--------                              |
| buildId | Jedinečný identifikátor sestavení. |
| apiVersion | 1.0 |

**Odpověď:**

Nastavit informace HTTP stavový kód: 200

Toto rozhraní API vrátí kolekci prvků klíč/hodnota. Každý prvek představuje parametru a jeho hodnotu:
- `feed/entry/content/properties/Key`– Vytvoření název parametru.
- `feed/entry/content/properties/Value`– Vytvoření hodnotu parametru.

Následující tabulka popisuje hodnotu, která představuje každý klíč.

|Klíč|Popis|Typ|Platná hodnota|
|:-----|:----|:----|:---|
|NumberOfModelIterations | Celkový čas výpočetním a přesnost modelu se projeví počtu iterací, které provede modelu. Vyšší čísla, zlepšit přesnost se zobrazí, ale časového využití bude trvat déle.| Celé číslo | 10 – 50 |
| NumberOfModelDimensions | Počet rozměry se týká počet modelu pokusí se najít v rámci dat funkcí. Zvýšení počtu rozměry vám umožní lepší optimalizaci výsledků do menší clusterů. Však příliš mnoho rozměry zabrání modelu nalézt korelace mezi položkami. | Celé číslo | 10 – 40 |
|ItemCutOffLowerBound| Definuje dolní mez položky pro chladiče. V tématu Použití chladič výše. | Celé číslo | 2 nebo víc (0 zakázat chladič) |
|ItemCutOffUpperBound| Definuje horní mez položky pro chladiče. V tématu Použití chladič výše. | Celé číslo | 2 nebo víc (0 zakázat chladič) |
|UserCutOffLowerBound| Definuje dolní mez uživatele pro chladiče. V tématu Použití chladič výše. | Celé číslo | 2 nebo víc (0 zakázat chladič) |
|UserCutOffUpperBound| Definuje horní mez uživatele pro chladiče. V tématu Použití chladič výše. | Celé číslo | 2 nebo víc (0 zakázat chladič) |
| Popis | Vytvořte popis. | Řetězec | Libovolný text, maximální 512 znaků |
| EnableModelingInsights | Umožňuje výpočet metriky na modelu doporučení. | Logická hodnota | True nebo False |
| UseFeaturesInModel | Označuje, pokud funkce se dají použít k rozšíření modelu doporučení. | Logická hodnota | True nebo False |
| ModelingFeatureList | Hodnoty oddělené seznam názvů funkcí pro použití v dialogu sestavení doporučení s cílem zdokonalit doporučení. | Řetězec | Funkce názvy, aby byl 512 znaků |
| AllowColdItemPlacement | Označuje, pokud doporučení by měl taky nabízená studenou položky pomocí funkce podobnosti. | Logická hodnota | True nebo False |
| EnableFeatureCorrelation | Označuje, pokud funkce se dají použít v důvody. | Logická hodnota | True nebo False |
| ReasoningFeatureList | Hodnoty oddělené seznam názvů funkcí pro důvody vět (například doporučení vysvětlení).  | Řetězec | Funkce názvy, aby byl 512 znaků |


OData XML

    <feed xmlns:base="https://api.datamarket.azure.com/amla/recommendations/v3/GetBuildParameters" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
        <title type="text" />
        <subtitle type="text">Get build parameters</subtitle>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetBuildParameters?buildId='1000653'&amp;apiVersion='1.0'</id>
        <rights type="text" />
        <updated>2015-01-08T13:50:57Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetBuildParameters?buildId='1000653'&amp;apiVersion='1.0'" />
        <entry>
            <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetBuildParameters?buildId='1000653'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
            <title type="text">GetBuildParametersEntity</title>
            <updated>2015-01-08T13:50:57Z</updated>
            <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetBuildParameters?buildId='1000653'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
            <content type="application/xml">
                <m:properties>
                    <d:Key m:type="Edm.String">UseFeaturesInModel</d:Key>
                    <d:Value m:type="Edm.String">False</d:Value>
                </m:properties>
            </content>
        </entry>
        <entry>
            <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetBuildParameters?buildId='1000653'&amp;apiVersion='1.0'&amp;$skip=1&amp;$top=1</id>
            <title type="text">GetBuildParametersEntity</title>
            <updated>2015-01-08T13:50:57Z</updated>
            <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetBuildParameters?buildId='1000653'&amp;apiVersion='1.0'&amp;$skip=1&amp;$top=1" />
            <content type="application/xml">
                <m:properties>
                    <d:Key m:type="Edm.String">AllowColdItemPlacement</d:Key>
                    <d:Value m:type="Edm.String">False</d:Value>
                </m:properties>
            </content>
        </entry>
        <entry>
            <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetBuildParameters?buildId='1000653'&amp;apiVersion='1.0'&amp;$skip=2&amp;$top=1</id>
            <title type="text">GetBuildParametersEntity</title>
            <updated>2015-01-08T13:50:57Z</updated>
            <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetBuildParameters?buildId='1000653'&amp;apiVersion='1.0'&amp;$skip=2&amp;$top=1" />
            <content type="application/xml">
                <m:properties>
                    <d:Key m:type="Edm.String">EnableFeatureCorrelation</d:Key>
                    <d:Value m:type="Edm.String">False</d:Value>
                </m:properties>
            </content>
        </entry>
        <entry>
            <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetBuildParameters?buildId='1000653'&amp;apiVersion='1.0'&amp;$skip=3&amp;$top=1</id>
            <title type="text">GetBuildParametersEntity</title>
            <updated>2015-01-08T13:50:57Z</updated>
            <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetBuildParameters?buildId='1000653'&amp;apiVersion='1.0'&amp;$skip=3&amp;$top=1" />
            <content type="application/xml">
                <m:properties>
                    <d:Key m:type="Edm.String">EnableModelingInsights</d:Key>
                    <d:Value m:type="Edm.String">False</d:Value>
                </m:properties>
            </content>
        </entry>
        <entry>
            <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetBuildParameters?buildId='1000653'&amp;apiVersion='1.0'&amp;$skip=4&amp;$top=1</id>
            <title type="text">GetBuildParametersEntity</title>
            <updated>2015-01-08T13:50:57Z</updated>
            <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetBuildParameters?buildId='1000653'&amp;apiVersion='1.0'&amp;$skip=4&amp;$top=1" />
            <content type="application/xml">
                <m:properties>
                    <d:Key m:type="Edm.String">NumberOfModelIterations</d:Key>
                    <d:Value m:type="Edm.String">10</d:Value>
                </m:properties>
            </content>
        </entry>
        <entry>
            <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetBuildParameters?buildId='1000653'&amp;apiVersion='1.0'&amp;$skip=5&amp;$top=1</id>
            <titletype="text">GetBuildParametersEntity</title>
            <updated>2015-01-08T13:50:57Z</updated>
            <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetBuildParameters?buildId='1000653'&amp;apiVersion='1.0'&amp;$skip=5&amp;$top=1" />
            <content type="application/xml">
                <m:properties>
                    <d:Key m:type="Edm.String">NumberOfModelDimensions</d:Key>
                    <d:Value m:type="Edm.String">10</d:Value>
                </m:properties>
            </content>
        </entry>
        <entry>
            <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetBuildParameters?buildId='1000653'&amp;apiVersion='1.0'&amp;$skip=6&amp;$top=1</id>
            <title type="text">GetBuildParametersEntity</title>
            <updated>2015-01-08T13:50:57Z</updated>
            <linkrel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetBuildParameters?buildId='1000653'&amp;apiVersion='1.0'&amp;$skip=6&amp;$top=1" />
            <content type="application/xml">
                <m:properties>
                    <d:Key m:type="Edm.String">ItemCutOffLowerBound</d:Key>
                    <d:Value m:type="Edm.String">2</d:Value>
                </m:properties>
            </content>
        </entry>
        <entry>
            <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetBuildParameters?buildId='1000653'&amp;apiVersion='1.0'&amp;$skip=7&amp;$top=1</id>
            <title type="text">GetBuildParametersEntity</title>
            <updated>2015-01-08T13:50:57Z</updated>
            <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetBuildParameters?buildId='1000653'&amp;apiVersion='1.0'&amp;$skip=7&amp;$top=1" />
            <content type="application/xml">
                <m:properties>
                    <d:Key m:type="Edm.String">ItemCutOffUpperBound</d:Key>
                    <d:Value m:type="Edm.String">2147483647</d:Value>
                </m:properties>
            </content>
        </entry>
        <entry>
            <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetBuildParameters?buildId='1000653'&amp;apiVersion='1.0'&amp;$skip=8&amp;$top=1</id>
            <title type="text">GetBuildParametersEntity</title>
            <updated>2015-01-08T13:50:57Z</updated>
            <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetBuildParameters?buildId='1000653'&amp;apiVersion='1.0'&amp;$skip=8&amp;$top=1" />
            <content type="application/xml">
                <m:properties>
                    <d:Key m:type="Edm.String">UserCutOffLowerBound</d:Key>
                    <d:Value m:type="Edm.String">2</d:Value>
                </m:properties>
            </content>
        </entry>
        <entry>
            <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetBuildParameters?buildId='1000653'&amp;apiVersion='1.0'&amp;$skip=9&amp;$top=1</id>
            <title type="text">GetBuildParametersEntity</title>
            <updated>2015-01-08T13:50:57Z</updated>
            <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetBuildParameters?buildId='1000653'&amp;apiVersion='1.0'&amp;$skip=9&amp;$top=1" />
            <content type="application/xml">
                <m:properties>
                    <d:Key m:type="Edm.String">UserCutOffUpperBound</d:Key>
                    <d:Value m:type="Edm.String">2147483647</d:Value>
                </m:properties>
            </content>
        </entry>
        <entry>
            <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetBuildParameters?buildId='1000653'&amp;apiVersion='1.0'&amp;$skip=10&amp;$top=1</id>
            <title type="text">GetBuildParametersEntity</title>
            <updated>2015-01-08T13:50:57Z</updated>
            <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetBuildParameters?buildId='1000653'&amp;apiVersion='1.0'&amp;$skip=10&amp;$top=1" />
            <content type="application/xml">
                <m:properties>
                    <d:Key m:type="Edm.String">ModelingFeatureList</d:Key>
                    <d:Value m:type="Edm.String"/>
                </m:properties>
            </content>
        </entry>
        <entry>
            <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetBuildParameters?buildId='1000653'&amp;apiVersion='1.0'&amp;$skip=11&amp;$top=1</id>
            <title type="text">GetBuildParametersEntity</title>
            <updated>2015-01-08T13:50:57Z</updated>
            <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetBuildParameters?buildId='1000653'&amp;apiVersion='1.0'&amp;$skip=11&amp;$top=1" />
            <content type="application/xml">
                <m:properties>
                    <d:Key m:type="Edm.String">ReasoningFeatureList</d:Key>
                    <d:Value m:type="Edm.String"/>
                </m:properties>
            </content>
        </entry>
        <entry>
            <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetBuildParameters?buildId='1000653'&amp;apiVersion='1.0'&amp;$skip=12&amp;$top=1</id>
            <title type="text">GetBuildParametersEntity</title>
            <updated>2015-01-08T13:50:57Z</updated>
            <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetBuildParameters?buildId='1000653'&amp;apiVersion='1.0'&amp;$skip=12&amp;$top=1" />
            <content type="application/xml">
                <m:properties>
                    <d:Key m:type="Edm.String">Description</d:Key>
                    <d:Value m:type="Edm.String">rankBuild</d:Value>
                </m:properties>
            </content>
        </entry>
    </feed>

##<a name="12-recommendation"></a>12. doporučení
###<a name="121-get-item-recommendations-for-active-build"></a>12.1. Získat doporučení položky (aktivní sestavení)

Získání doporučení aktivního sestavení typu "Doporučení" nebo "Fbt" založené na seznamu semen (vstupní) položek.

| Metoda HTTP | IDENTIFIKÁTOR URI |
|:--------|:--------|
|ZÍSKÁNÍ     |`<rootURI>/ItemRecommend?modelId=%27<modelId>%27&itemIds=%27<itemId>%27&numberOfResults=<int>&includeMetadata=<bool>&apiVersion=%271.0%27`<br><br>Příklad:<br>`<rootURI>/ItemRecommend?modelId=%272779c063-48fb-46c1-bae3-74acddc8c1d1%27&itemIds=%271003%27&numberOfResults=10&includeMetadata=false&apiVersion=%271.0%27`|

|   Název parametru  |   Platné hodnoty                        |
|:--------          |:--------                              |
| modelId | Jedinečný identifikátor modelu |
| položky ItemID | Hodnoty oddělené seznam položky, které chcete doporučit. <br>Pokud je aktivní sestavení typu FBT můžete poslat jenom o jednu úroveň. <br>Maximální délka: 1 024 |
| numberOfResults | Počet požadované výsledky <br> Maximální: 150 |
| includeMetatadata | Další použití, vždy false |
| apiVersion | 1.0 |

**Odpověď:**

Nastavit informace HTTP stavový kód: 200


Odpověď na obsahuje jednu položku za doporučené položky. Každá položka má následující údaje:
- `Feed\entry\content\properties\Id`– ID Doporučené položky.
- `Feed\entry\content\properties\Name`– Název položky.
- `Feed\entry\content\properties\Rating`– Hodnocení doporučení; vyšší čísla znamená vyšší spolehlivosti.
- `Feed\entry\content\properties\Reasoning`-Doporučení důvody (například doporučení vysvětlení).

Následující příklad odpověď obsahuje 10 Doporučené položky.

OData XML

    <feed xmlns:base="https://api.datamarket.azure.com/amla/recommendations/v3/ItemRecommend" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
    <title type="text" />
     <subtitle type="text">Get Recommendation</subtitle>
     <id>https://api.datamarket.azure.com/amla/recommendations/v3/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'</id>
    <rights type="text" />
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'" />
    <entry>
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
    <title type="text">GetRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
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
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=1&amp;$top=1</id>
    <title type="text">GetRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=1&amp;$top=1" />
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
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=2&amp;$top=1</id>
    <title type="text">GetRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=2&amp;$top=1" />
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
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=3&amp;$top=1</id>
    <title type="text">GetRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=3&amp;$top=1" />
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
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=4&amp;$top=1</id>
    <title type="text">GetRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=4&amp;$top=1" />
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
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=5&amp;$top=1</id>
    <title type="text">GetRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=5&amp;$top=1" />
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
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=6&amp;$top=1</id>
    <title type="text">GetRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=6&amp;$top=1" />
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
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=7&amp;$top=1</id>
    <title type="text">GetRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=7&amp;$top=1" />
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
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=8&amp;$top=1</id>
    <title type="text">GetRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=8&amp;$top=1" />
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
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=9&amp;$top=1</id>
    <title type="text">GetRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=9&amp;$top=1" />
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

###<a name="122-get-item-recommendations-of-a-specific-build"></a>12.2. Získání položku doporučení (konkrétní sestavení)

Pokud potřebujete doporučení konkrétní sestavení typu "Doporučení" nebo "Fbt".

| Metoda HTTP | IDENTIFIKÁTOR URI |
|:--------|:--------|
|ZÍSKÁNÍ     |`<rootURI>/ItemRecommend?modelId=%27<modelId>%27&itemIds=%27<itemId>%27&numberOfResults=<int>&includeMetadata=<bool>&buildId=<int>&apiVersion=%271.0%27`<br><br>Příklad:<br>`<rootURI>/ItemRecommend?modelId=%272779c063-48fb-46c1-bae3-74acddc8c1d1%27&itemIds=%271003%27&numberOfResults=10&includeMetadata=false&buildId=1234&apiVersion=%271.0%27`|

|   Název parametru  |   Platné hodnoty                        |
|:--------          |:--------                              |
| modelId | Jedinečný identifikátor modelu |
| položky ItemID | Hodnoty oddělené seznam položky, které chcete doporučit. <br>Pokud je aktivní sestavení typu FBT můžete poslat jenom o jednu úroveň. <br>Maximální délka: 1 024 |
| numberOfResults | Počet požadované výsledky <br> Maximální: 150  |
| includeMetatadata | Další použití, vždy false
| buildId | vytvoření id pro účely tohoto požadavku doporučení |
| apiVersion | 1.0 |

**Odpověď:**

Nastavit informace HTTP stavový kód: 200


Odpověď na obsahuje jednu položku za doporučené položky. Každá položka má následující údaje:
- `Feed\entry\content\properties\Id`– ID Doporučené položky.
- `Feed\entry\content\properties\Name`– Název položky.
- `Feed\entry\content\properties\Rating`– Hodnocení doporučení; vyšší čísla znamená vyšší spolehlivosti.
- `Feed\entry\content\properties\Reasoning`-Doporučení důvody (například doporučení vysvětlení).

Podívejte se na příklad odpovědi v 12.1

###<a name="123-get-fbt-recommendations-for-active-build"></a>12.3. Získat FBT doporučení (aktivní sestavení)

Pokud potřebujete doporučení aktivního sestavení typu "Fbt", na kterých založený na položku Počáteční hodnota (input).

| Metoda HTTP | IDENTIFIKÁTOR URI |
|:--------|:--------|
|ZÍSKÁNÍ     |`<rootURI>/ItemFbtRecommend?modelId=%27<modelId>%27&itemId=%27<itemId>%27&numberOfResults=<int>&minimalScore=<double>&includeMetadata=<bool>&apiVersion=%271.0%27`<br><br>Příklad:<br>`<rootURI>/ItemFbtRecommend?modelId=%272779c063-48fb-46c1-bae3-74acddc8c1d1%27&itemId=%271003%27&numberOfResults=10&minimalScore=<double>&includeMetadata=false&apiVersion=%271.0%27`|

|   Název parametru  |   Platné hodnoty                        |
|:--------          |:--------                              |
| modelId | Jedinečný identifikátor modelu |
| itemId | Položku, kterou chcete doporučit. <br>Maximální délka: 1 024 |
| numberOfResults | Počet požadované výsledky <br>Maximální: 150 |
| minimalScore | Minimální skóre, které často sady by měla být chcete být součástí vrácených výsledků |
| includeMetatadata | Další použití, vždy false |
| apiVersion | 1.0 |

**Odpověď:**

Nastavit informace HTTP stavový kód: 200


Odpověď na obsahuje jednu položku za doporučené položky sady (sady položek, které jsou obvykle koupili společně s počáteční hodnota/vstupní položky). Každá položka má následující údaje:
- `Feed\entry\content\properties\Id1`– ID Doporučené položky.
- `Feed\entry\content\properties\Name1`– Název položky.
- `Feed\entry\content\properties\Id2`ID – 2nd Doporučené položky (volitelné).
- `Feed\entry\content\properties\Name2`-Název 2 položky (volitelné).
- `Feed\entry\content\properties\Rating`– Hodnocení doporučení; vyšší čísla znamená vyšší spolehlivosti.
- `Feed\entry\content\properties\Reasoning`-Doporučení důvody (například doporučení vysvětlení).

Odpověď na příkladu obsahuje 3 sadu Doporučené položky.

OData XML

    <feed xmlns:base="https://api.datamarket.azure.com/amla/recommendations/v3/ItemFbtRecommend" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
    <title type="text" />
     <subtitle type="text">Get Recommendation</subtitle>
     <id>https://api.datamarket.azure.com/amla/recommendations/v3/ItemFbtRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemId='1003'&amp;numberOfResults=3&amp;minimalScore=0.1&amp;includeMetadata=false&amp;apiVersion='1.0'</id>
    <rights type="text" />
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/ItemFbtRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=3&amp;minimalScore=0.1&amp;includeMetadata=false&amp;apiVersion='1.0'" />
    <entry>
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/ItemFbtRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=3&amp;minimalScore=0.1&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
    <title type="text">GetFbtRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/ItemFbtRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=3&amp;minimalScore=0.1&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Id1 m:type="Edm.String">159</d:Id1>
        <d:Name1 m:type="Edm.String">159</d:Name1>
        <d:Id2 m:type="Edm.String"></d:Id2>
        <d:Name2 m:type="Edm.String"></d:Name2>
        <d:Rating m:type="Edm.Double">0.543343480387708</d:Rating>
        <d:Reasoning m:type="Edm.String">People who bought '1003' also bought '159'</d:Reasoning>
      </m:properties>
    </content>
     </entry>
     <entry>
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/ItemFbtRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=3&amp;minimalScore=0.1&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=1&amp;$top=1</id>
    <title type="text">GetFbtRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/ItemFbtRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=3&amp;minimalScore=0.1&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=1&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Id1 m:type="Edm.String">52</d:Id1>
        <d:Name1 m:type="Edm.String">52</d:Name1>
        <d:Id2 m:type="Edm.String"></d:Id2>
        <d:Name2 m:type="Edm.String"></d:Name2>
        <d:Rating m:type="Edm.Double">0.533343480387708</d:Rating>
        <d:Reasoning m:type="Edm.String">People who bought '1003' also bought '52'</d:Reasoning>
      </m:properties>
    </content>
     </entry>
     <entry>
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/ItemFbtRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=3&amp;minimalScore=0.1&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=2&amp;$top=1</id>
    <title type="text">GetFbtRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/ItemFbtRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=3&amp;minimalScore=0.1&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=2&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Id1 m:type="Edm.String">35</d:Id1>
        <d:Name1 m:type="Edm.String">35</d:Name1>
        <d:Id2 m:type="Edm.String">102</d:Id2>
        <d:Name2 m:type="Edm.String">102</d:Name2>
        <d:Rating m:type="Edm.Double">0.523343480387708</d:Rating>
        <d:Reasoning m:type="Edm.String">People who bought '1003' also bought '35' and '102'</d:Reasoning>
      </m:properties>
    </content>
     </entry>
    </feed>

###<a name="124-get-fbt-recommendations-of-a-specific-build"></a>12.4. Získání FBT doporučení (konkrétní sestavení)

Pokud potřebujete doporučení konkrétní sestavení typu "Fbt".

| Metoda HTTP | IDENTIFIKÁTOR URI |
|:--------|:--------|
|ZÍSKÁNÍ     |`<rootURI>/ItemFbtRecommend?modelId=%27<modelId>%27&itemId=%27<itemId>%27&numberOfResults=<int>&minimalScore=<double>&includeMetadata=<bool>&buildId=<int>&apiVersion=%271.0%27`<br><br>Příklad:<br>`<rootURI>/ItemFbtRecommend?modelId=%272779c063-48fb-46c1-bae3-74acddc8c1d1%27&itemId=%271003%27&numberOfResults=10&minimalScore=0.1&includeMetadata=false&buildId=1234&apiVersion=%271.0%27`|

|   Název parametru  |   Platné hodnoty                        |
|:--------          |:--------                              |
| modelId | Jedinečný identifikátor modelu |
| itemId | Položku, kterou chcete doporučit. <br>Maximální délka: 1 024 |
| numberOfResults | Počet požadované výsledky <br>Maximální: 150 |
| minimalScore | Minimální skóre, které často sady by měla být chcete být součástí vrácených výsledků |
| includeMetatadata | Další použití, vždy false |
| buildId | vytvoření id pro účely tohoto požadavku doporučení |
| apiVersion | 1.0 |

**Odpověď:**

Nastavit informace HTTP stavový kód: 200


Odpověď na obsahuje jednu položku za doporučené položky sady (sady položek, které jsou obvykle koupili společně s počáteční hodnota/vstupní položky). Každá položka má následující údaje:
- `Feed\entry\content\properties\Id1`– ID Doporučené položky.
- `Feed\entry\content\properties\Name1`– Název položky.
- `Feed\entry\content\properties\Id2`ID – 2nd Doporučené položky (volitelné).
- `Feed\entry\content\properties\Name2`-Název 2 položky (volitelné).
- `Feed\entry\content\properties\Rating`– Hodnocení doporučení; vyšší čísla znamená vyšší spolehlivosti.
- `Feed\entry\content\properties\Reasoning`-Doporučení důvody (například doporučení vysvětlení).

Podívejte se na příklad odpovědi v 12.3

###<a name="125-get-user-recommendations-for-active-build"></a>12,5. Získat doporučení uživatelů (pro aktivní sestavení)

Získáte uživatele doporučení sestavení typu "Doporučení" označeny jako aktivní Tvůrce dotazů.

Rozhraní API vrátí seznam předpovídané položky podle historie využití tohoto uživatele.

Poznámky: 
 1. Neexistuje žádná doporučení uživatele pro FBT sestavení.
 2. Pokud je aktivní sestavení FBT bude tato metoda vrátí chybu.

| Metoda HTTP | IDENTIFIKÁTOR URI |
|:--------|:--------|
|ZÍSKÁNÍ     |`<rootURI>/UserRecommend?modelId=%27<modelId>%27&userId=%27<userId>%27&numberOfResults=<int>&includeMetadata=<bool>&apiVersion=%271.0%27`<br><br>Příklad:<br>`<rootURI>/UserRecommend?modelId=%272779c063-48fb-46c1-bae3-74acddc8c1d1%27&userId=%27u1101%27&numberOfResults=10&includeMetadata=false&apiVersion=%271.0%27`|

|   Název parametru  |   Platné hodnoty                        |
|:--------          |:--------                              |
| modelId | Jedinečný identifikátor modelu |
| ID uživatele  | Jedinečný identifikátor uživatele |
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

Podívejte se na příklad odpovědi v 12.1

###<a name="126-get-user-recommendations-with-item-list-for-active-build"></a>12,6. Získat doporučení uživatele pomocí seznamu položek (aktivní sestavení)

Získat uživatele doporučení sestavení typu "Doporučení" označeny jako aktivní sestavení s další seznamu položek

Rozhraní API vrátí seznam předpovídané položky podle historie využití uživatele a další zadané položky.

Poznámky: 
 1. Neexistuje žádná doporučení uživatele pro FBT sestavení.
 2. Pokud je aktivní sestavení FBT bude tato metoda vrátí chybu.


| Metoda HTTP | IDENTIFIKÁTOR URI |
|:--------|:--------|
|ZÍSKÁNÍ     |`<rootURI>/UserRecommend?modelId=%27<modelId>%27&userId=%27<userId>&itemsIds=%27<itemsIds>%27&numberOfResults=<int>&includeMetadata=<bool>&apiVersion=%271.0%27`<br><br>Příklad:<br>`<rootURI>/UserRecommend?modelId=%272779c063-48fb-46c1-bae3-74acddc8c1d1%27&userId=%27u1101%27&itemsIds=%271003%2C1000%27&numberOfResults=10&includeMetadata=false&apiVersion=%271.0%27`|

|   Název parametru  |   Platné hodnoty                        |
|:--------          |:--------                              |
| modelId | Jedinečný identifikátor modelu |
| ID uživatele  | Jedinečný identifikátor uživatele |
| itemsIds | Hodnoty oddělené seznam položky, které chcete doporučit. Maximální délka: 1 024 |
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

Podívejte se na příklad odpovědi v 12.1

###<a name="127-get-user-recommendations--of-a-specific-build"></a>12.7. Získání uživatele doporučení (konkrétní sestavení)

Pokud potřebujete uživatele doporučení konkrétní sestavení typu "Doporučení".

Rozhraní API vrátí seznam předpovídané položky podle historie využití uživatele (použitý v konkrétní vytvořit).

Poznámka: Neexistuje žádné uživatele doporučení pro FBT sestavení.

| Metoda HTTP | IDENTIFIKÁTOR URI |
|:--------|:--------|
|ZÍSKÁNÍ     |`<rootURI>/UserRecommend?modelId=%27<modelId>%27&userId=%27<userId>%27&numberOfResults=<int>&includeMetadata=<bool>&buildId=<int>&apiVersion=%271.0%27`<br><br>Příklad:<br>`<rootURI>/UserRecommend?modelId=%272779c063-48fb-46c1-bae3-74acddc8c1d1%27&userId=%27u1101%27&numberOfResults=10&includeMetadata=false&buildId=50012&apiVersion=%271.0%27`|


|   Název parametru  |   Platné hodnoty                        |
|:--------          |:--------                              |
| modelId | Jedinečný identifikátor modelu |
| ID uživatele  | Jedinečný identifikátor uživatele |
| numberOfResults | Počet požadované výsledky |
| includeMetatadata | Další použití, vždy false |
| buildId | vytvoření id pro účely tohoto požadavku doporučení |
| apiVersion | 1.0 |

**Odpověď:**

Nastavit informace HTTP stavový kód: 200


Odpověď na obsahuje jednu položku za doporučené položky. Každá položka má následující údaje:
- `Feed\entry\content\properties\Id`– ID Doporučené položky.
- `Feed\entry\content\properties\Name`– Název položky.
- `Feed\entry\content\properties\Rating`– Hodnocení doporučení; vyšší čísla znamená vyšší spolehlivosti.
- `Feed\entry\content\properties\Reasoning`-Doporučení důvody (například doporučení vysvětlení).

Podívejte se na příklad odpovědi v 12.1


###<a name="128-get-user-recommendations-with-item-list-of-a-specific-build"></a>12.8. Získat doporučení uživatele s položky seznamu (konkrétní sestavení)

Pokud potřebujete uživatele doporučení konkrétní sestavení typu "Doporučení" a seznam dalších položek.

Rozhraní API vrátí seznam předpovídané položky podle historie využití uživatele a další seznam položek.

Poznámka: Tthere je žádné uživatele doporučení pro FBT sestavení.

| Metoda HTTP | IDENTIFIKÁTOR URI |
|:--------|:--------|
|ZÍSKÁNÍ     |`<rootURI>/UserRecommend?modelId=%27<modelId>%27&userId=%27<userId>%27&itemsIds=%27<itemsIds>%27&numberOfResults=<int>&includeMetadata=<bool>&buildId=<int>&apiVersion=%271.0%27`<br><br>Příklad:<br>`<rootURI>/UserRecommend?modelId=%272779c063-48fb-46c1-bae3-74acddc8c1d1%27&userId=%27u1101%27&itemsIds=%271003%27&numberOfResults=10&includeMetadata=false&buildId=50012&apiVersion=%271.0%27`|



|   Název parametru  |   Platné hodnoty                        |
|:--------          |:--------                              |
| modelId | Jedinečný identifikátor modelu |
| ID uživatele  | Jedinečný identifikátor uživatele |
| položky ItemID | Hodnoty oddělené seznam položky, které chcete doporučit. Maximální délka: 1 024 |
| numberOfResults | Počet požadované výsledky |
| includeMetatadata | Další použití, vždy false |
| buildId | vytvoření id pro účely tohoto požadavku doporučení |
| apiVersion | 1.0 |

**Odpověď:**

Nastavit informace HTTP stavový kód: 200


Odpověď na obsahuje jednu položku za doporučené položky. Každá položka má následující údaje:
- `Feed\entry\content\properties\Id`– ID Doporučené položky.
- `Feed\entry\content\properties\Name`– Název položky.
- `Feed\entry\content\properties\Rating`– Hodnocení doporučení; vyšší čísla znamená vyšší spolehlivosti.
- `Feed\entry\content\properties\Reasoning`-Doporučení důvody (například doporučení vysvětlení).

Podívejte se na příklad odpovědi v 12.1

##<a name="13-user-usage-history"></a>13. použití historie uživatele
Byla vytvořenou modelu doporučení k načtení historie uživatele (položky související s určitým uživatelem) umožní systém používaný pro sestavení.
Toto rozhraní API povolit k načtení historie uživatele

Poznámka: historie uživatele je momentálně dostupné jenom pro sestavení doporučení.

###<a name="131-retrieve-user-history"></a>13.1 načíst historie uživatele
Seznam položek použita aktivního sestavení nebo v zadaném sestavení pro dané uživatelské id načítejte.

| Metoda HTTP | IDENTIFIKÁTOR URI |
|:--------|:--------|
|ZÍSKÁNÍ     | Zobrazit historii uživatele pro aktivní Tvůrce dotazů.<br/>`<rootURI>/GetUserHistory?modelId=%27<model_id>%27&userId=%27<userId>%27&apiVersion=%271.0%27`<br/><br/>Zobrazit historii uživatele pro dané Tvůrce dotazů`<rootURI>/GetUserHistory?modelId=%27<model_id>%27&userId=%27<userId>%27&buildId=<int>&apiVersion=%271.0%27`<br/><br/>Příklad:`<rootURI>/GetUserHistory?modelId=%2727967136e8-f868-4258-9331-10d567f87fae%27&&userId=%27u_1013%27&apiVersion=%271.0%277`|


|   Název parametru  |   Platné hodnoty                        |
|:--------          |:--------                              |
| modelId | Jedinečný identifikátor modelu.|
| ID uživatele | Jedinečný identifikátor uživatele.
| buildId | Volitelný parametr, aby označují, ze které sestavení historie uživatele bude vzdálené použití
| apiVersion | 1.0 |


**Odpověď:**

Nastavit informace HTTP stavový kód: 200

Odpověď na obsahuje jednu položku za doporučené položky. Každá položka má následující údaje:
- `Feed\entry\content\properties\Id`– ID Doporučené položky.
- `Feed\entry\content\properties\Name`– Název položky.
- `Feed\entry\content\properties\Rating`-NENÍ K DISPOZICI.
- `Feed\entry\content\properties\Reasoning`-NENÍ K DISPOZICI.

OData XML

    <feed xmlns:base="https://api.datamarket.azure.com/amla/recommendations/v3/GetUserHistory" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
    <title type="text" />
    <subtitle type="text">Get User History</subtitle>
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetUserHistory?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;userId='u_1013'&amp;apiVersion='1.0'</id>
    <rights type="text" />
    <updated>2015-05-26T15:32:47Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetUserHistory?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;userId='u_1013'&amp;apiVersion='1.0'" />
    <entry>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetUserHistory?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;userId='u_1013'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
        <title type="text">CatalogItemsThatContainATokenEntity</title>
        <updated>2015-05-26T15:32:47Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetUserHistory?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;userId='u_1013'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
        <content type="application/xml">
            <m:properties>
                <d:Id m:type="Edm.String">2406E770-769C-4189-89DE-1C9283F93A96</d:Id>
                <d:Name m:type="Edm.String">Clara Callan</d:Name>
                <d:Rating m:type="Edm.Double">0</d:Rating>
                <d:Reasoning m:type="Edm.String"/>
                <d:Metadata m:type="Edm.String"/>
                <d:FormattedRating m:type="Edm.Double" m:null="true"/>
            </m:properties>
        </content>
    </entry>
</feed>

##<a name="14-notifications"></a>14. oznámení o
Azure doporučení výukové počítače vytvoří oznámení při trvalý chyb v systému. Existují 3 typy oznámení:
1.  Sestavení chyba – toto oznámení se spustí pro každý selhání Tvůrce dotazů.
2.  Získávání dat zpracování chyba – toto oznámení se aktivuje, když máme víc než 100 chyby za posledních pět minut při zpracování událostí použití na model.
3.  Doporučení spotřeba chyba – toto oznámení se aktivuje, když máme víc než 100 chyby za posledních pět minut při zpracování žádostí o doporučení na model.


###<a name="141-get-notifications"></a>14.1. Oznámení
Obnoví všechna oznámení pro všechny modely nebo jediný model.

| Metoda HTTP | IDENTIFIKÁTOR URI |
|:--------|:--------|
|ZÍSKÁNÍ     |`<rootURI>/GetNotifications?modelId=%27<model_id>%27&apiVersion=%271.0%27`<br><br>Zobrazuje všechny oznámení pro všechny modely:<br>`<rootURI>/GetNotifications?apiVersion=%271.0%27`<br><br>Příklad usnadňující oznámení pro konkrétní model:<br>`<rootURI>/GetNotifications?modelId=%27967136e8-f868-4258-9331-10d567f87fae%27&apiVersion=%271.0%277`|


|   Název parametru  |   Platné hodnoty                        |
|:--------          |:--------                              |
| modelId | Volitelný parametr. Při vynechání, zobrazí se všechny oznámení pro všechny modely. <br>Platná hodnota: Jedinečný identifikátor modelu.|
| apiVersion | 1.0 |
|||
| Žádost o textu | ŽÁDNÁ |

**Odpověď:**

Nastavit informace HTTP stavový kód: 200

OData XML

    The response includes one entry per notification. Each entry has the following data:
        * feed\entry\content\properties\UserName - Internal user name identification.
        * feed\entry\content\properties\ModelId - Model ID.
        * feed\entry\content\properties\Message - Notification message.
        * feed\entry\content\properties\DateCreated - Date that this notification was created in UTC format.
        * feed\entry\content\properties\NotificationType - Notification types. Values are BuildFailure, RecommendationFailure, and DataAquisitionFailure.

    <feed xmlns:base="https://api.datamarket.azure.com/amla/recommendations/v3/GetNotifications" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
        <title type="text" />
        <subtitle type="text">Get a list of Notifications for a user</subtitle>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetNotifications?modelId='967136e8-f868-4258-9331-10d567f87fae'&amp;apiVersion='1.0'</id>
        <rights type="text" />
        <updated>2014-11-04T13:24:23Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetNotifications?modelId='967136e8-f868-4258-9331-10d567f87fae'&amp;apiVersion='1.0'" />
        <entry>
                <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetNotifications?modelId='967136e8-f868-4258-9331-10d567f87fae'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
            <title type="text">GetAListOfNotificationsForAUserEntity</title>
            <updated>2014-11-04T13:24:23Z</updated>
            <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetNotifications?modelId='967136e8-f868-4258-9331-10d567f87fae'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
            <content type="application/xml">
                <m:properties>
                    <d:UserName m:type="Edm.String">515506bc-3693-4dce-a5e2-81cb3e8efb56@dm.com</d:UserName>
                    <d:ModelId m:type="Edm.String">967136e8-f868-4258-9331-10d567f87fae</d:ModelId>
                    <d:Message m:type="Edm.String">Build failed for user</d:Message>
                    <d:DateCreated m:type="Edm.String">2014-11-04T13:23:14.383</d:DateCreated>
                    <d:NotificationType m:type="Edm.String">BuildFailure</d:NotificationType>
                </m:properties>
            </content>
        </entry>
    </feed>

###<a name="142-delete-model-notifications"></a>14.2. Odstraňování oznámení modelu
Odstraní všechny další oznámení pro model.

| Metoda HTTP | IDENTIFIKÁTOR URI |
|:--------|:--------|
|ODSTRANĚNÍ     |`<rootURI>/DeleteModelNotifications?modelId=%<model_id>%27&apiVersion=%271.0%27`<br><br>Příklad:<br>`<rootURI>/DeleteModelNotifications?modelId=%27967136e8-f868-4258-9331-10d567f87fae%27&apiVersion=%271.0%27`|


|   Název parametru  |   Platné hodnoty                        |
|:--------          |:--------                              |
| modelId | Jedinečný identifikátor modelu |
| apiVersion | 1.0 |
|||
| Žádost o textu | ŽÁDNÁ |

**Odpověď**:

Nastavit informace HTTP stavový kód: 200

###<a name="143-delete-user-notifications"></a>14.3. Odstraňování oznámení uživatele
Odstraní všechna oznámení pro všechny modely.

| Metoda HTTP | IDENTIFIKÁTOR URI |
|:--------|:--------|
|ODSTRANĚNÍ     |`<rootURI>/DeleteUserNotifications?apiVersion=%271.0%27`|


|   Název parametru  |   Platné hodnoty                        |
|:--------          |:--------                              |
| apiVersion | 1.0 |
|||
| Žádost o textu | ŽÁDNÁ |

**Odpověď**:

Nastavit informace HTTP stavový kód: 200




##<a name="15-legal"></a>15. právní
Tento dokument je k dispozici "jako-je". Informace a názory uvedené v tomto dokumentu, včetně URL adresy a dalších odkazů na webu, mohou změnit bez předchozího upozornění.<br><br>
Některé zde uvedené příklady jsou k dispozici pouze ilustrace a jsou fiktivní. Žádné skutečné přidružení připojení je určená nebo událostmi.<br><br>
Tento dokument vám neposkytuje s všechny práva duševního vlastnictví jakéhokoli produktu společnosti Microsoft. Můžete zkopírovat a použijte tento dokument pro svou interní potřebu jako referenci.<br><br>
© 2015 Microsoft. Microsoft Corporation.
 
