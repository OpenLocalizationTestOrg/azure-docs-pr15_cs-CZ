<properties
   pageTitle="Návod, jak vytvořit datové služby pro Marketplace | Microsoft Azure"
   description="Podrobné pokyny, jak vytvářet, certifikace a nasazení datové služby pro nákup na Azure Marketplace."
   services="marketplace-publishing"
   documentationCenter=""
   authors="HannibalSII"
   manager="hascipio"
   editor=""/>

   <tags
      ms.service="marketplace"
      ms.devlang="na"
      ms.topic="article"
      ms.tgt_pltfrm="na"
      ms.workload="na"
      ms.date="08/26/2016"
      ms.author="hascipio; avikova" />

# <a name="understanding-the-nodes-schema-for-mapping-an-existing-web-service-to-odata-through-csdl"></a>Principy schématu uzly mapování stávající webové služby na OData pomocí CSDL

>[AZURE.IMPORTANT] **V současné době jsme už rychlého připojení všechny nové vydavatelé datové služby. Nové dataservices nebude získat schváleno seznam.** Pokud máte s aplikací business SaaS chcete publikovat na AppSource můžete najít další informace [v tomto poli](https://appsource.microsoft.com/partners). Pokud máte IaaS aplikace nebo vývojář služby rádi byste publikovat na webu Azure Marketplace můžete najít další informace [v tomto poli](https://azure.microsoft.com/marketplace/programs/certified/).

Tohoto dokumentu bude pomáhají objasnit struktuře uzel pro mapování protokolu OData CSDL. Je důležité mít na paměti, že je struktuře uzel dobře vytvořený kód XML. Takže kořenové, nadřazené a podřízené schématu platí při návrhu OData mapování.

## <a name="ignored-elements"></a>Ignorované prvky
Následují nejvyšší úrovně CSDL prvkům (uzlů XML) neprojdou pro použití v Azure Marketplace back-end při importu metadat webové služby. Lze prezentovat, ale budou ignorovat.

| Element | Rozsah |
|----|----|
| Použití elementu | Uzel, podřízené uzly a všech atributů |
| Element si přečtěte následující dokumentaci | Uzel, podřízené uzly a všech atributů |
| ComplexType | Uzel, podřízené uzly a všech atributů |
| Element přidružení | Uzel, podřízené uzly a všech atributů |
| Rozšířené vlastnosti | Uzel, podřízené uzly a všech atributů |
| EntityContainer | Jsou ignorovány pouze následujícími atributy: *slouží k rozšíření* a *AssociationSet* |
| Schéma | Jsou ignorovány pouze následujícími atributy: *Namespace* |
| FunctionImport | Jsou ignorovány pouze následujícími atributy: *režim* (výchozí hodnota ln pokládá) |
| Typ entity | Pouze následující podřízené uzly ignorovány: *PropertyRef* a *klíče* |

V následujícím textu najdete změny (přidali a ignorovat prvky) do různých uzlů CSDL XML podrobně.

## <a name="functionimport-node"></a>FunctionImport uzel
FunctionImport uzel představuje jeden adresu URL (vstupní bod), která poskytuje služby koncový uživatel. Uzel umožňuje popisující, jak je adresovaný adresu URL, které parametry jsou k dispozici koncový uživatel a jak se tyto parametrů jsou k dispozici.

Podrobnosti o tomto uzlu se nacházejí v [tady] [MSDNFunctionImportLink]

[MSDNFunctionImportLink]: (https://msdn.microsoft.com/library/cc716710 (v=vs.100).aspx)

Jsou následující další atributy (nebo doplňky ke atributů), které jsou zveřejněné příslušným uzel FunctionImport:

**d:BaseUri** -
URI šablony pro ZBÝVAJÍCÍ prostředek, který se zobrazí na Marketplace. Tržiště používá šablonu k vytvoření dotazů webová služba REST. Šablona URI obsahuje zástupné symboly parametrů ve formě {název parametru}, kde název parametru je název parametru. Například apiVersion = {apiVersion}.
Parametry jsou povoleny vytisknout jako identifikátor URI parametry nebo jako součást cestu URI. V případě vzhled na cestě jsou vždy povinné (nemůžete označit jako s možnou hodnotou Null). *Příklad:*`d:BaseUri="http://api.MyWeb.com/Site/{url}/v1/visits?start={start}&amp;end={end}&amp;ApiKey=3fadcaa&amp;Format=XML"`

**Název** - název importovaného funkce.  Nemůžou být stejné jako ostatní definované názvy CSDL.  Například Název = "GetModelUsageFile"

**Entit** *(volitelné)* – Pokud kolekce typy entit, vrátí funkce přínosu **entit** musí být sada, ke které patří kolekci entit. V opačném **entit** atribut nesmí použije. *Příklad:*`EntitySet="GetUsageStatisticsEntitySet"`

**Typ_vrácených_informací** *(Volitelné)* - Určuje typ prvků vrácených identifikátor URI.  Nepoužívejte Tenhle atribut, pokud funkce nevrací hodnotu. Podporované typy jsou následující:

 - **Kolekce (<Entity type name>)**: Určuje kolekci typů definovaný entity. Název se nachází v atributu názvu uzel typ entity. Příklad je kolekce (WXC. HourlyResult).
 - **Suroviny (<mime type>)**: Určuje nezpracovanými dokumentu/objektů blob vrácená uživateli. Příklad je Raw(image/jpeg) Další příklady:

  - ReturnType="Raw(text/plain)"
  - Typ_vrácených_informací = "kolekce (sage. DeleteAllUsageFilesEntity) "*

**d:Paging** - Určuje, jak stránkování uskutečněných jednotlivými ZBÝVAJÍCÍ zdroje. Hodnoty parametrů se používají v rámci složených odvětví, například stránky = {$page} & vlastnost itemsperpage = {$size} máte několik možností:

- **Žádný:** neexistuje žádná stránkování
- **Přeskočit:** stránkování vyjadřuje prostřednictvím logické "Přeskočit" a "vzít" (nahoře). Přeskočit kůň úpěl ďábelské prvky M a opět převzít vrátí další prvky N. Hodnota parametru: $skip
- **Trvat:** Převzetí vrátí další prvky N. Hodnota parametru: $take
- **PageSize:** stránkování vyjadřuje prostřednictvím logické stránky a velikost (počet položek na stránce). Stránka představuje aktuální stránky, která je vrácena. Hodnota parametru: $page
- **Velikost:** velikost představuje počet vrácených pro každou stránku položek. Hodnota parametru: $size

**d:AllowedHttpMethods** *(Volitelné)* - Určuje, které slovesné uskutečněných jednotlivými ZBÝVAJÍCÍ zdroje. Navíc omezuje přípustném slovesné zadanou hodnotu.  Výchozí = příspěvek.  *Příklad:* `d:AllowedHttpMethods="GET"` Máte několik možností:

- **Získat:** obvykle používá k vrácení dat
- **Příspěvku:** obvykle slouží k vložení nového listu
- **Umístění:** obvykle používá k aktualizaci dat
- **Odstranit:** slouží k odstranění dat

Další podřízené uzly (nevztahuje dokumentace CSDL) v rámci uzel FunctionImport jsou:

**d:RequestBody** *(Volitelné)* - požadavku slouží k označení, že žádosti očekává textu k odeslání. Parametry může být zadán v žádosti o textu. Jsou vyjádřeny uvnitř složených závorek, například {název parametru}. Tyto parametry jsou namapované na vstup do textu, která jsou přenášena obsahu poskytovatele služby. RequestBody element obsahuje atribut s vlastnost název httpMethod. Atribut umožňuje dvě hodnoty:

- **Příspěvku:** Použít, pokud je žádost HTTP příspěvku
- **Získat:** Použít, pokud je žádost HTTP GET

    Příklad:

        `<d:RequestBody d:httpMethod="POST">
        <![CDATA[
        <req1:Request xmlns:r1="http://schemas.mysite.com//generic/requests/1" Version="1.0">
        <req1:Query>{Query}</req1:Query><req1:AppId>D453474</req1:AppId>
        <req:DestinationSchemas><req1:Schema>Generic.RequestResponse[1.0]</req1:Schema>
        </req1:DestinationSchemas>
        </req1: Request>
        ]]>
        </d:RequestBody>`

**d:namespaces** a **d:Namespace** - popisuje tento uzel obory názvů jsou definované v souboru XML, který vám vrátil funkce import (Uniform koncový bod). XML, který vám vrátil službu back-end může obsahovat libovolný počet obory názvů k odlišení obsah, který bude vrácena. **Všechny tyto obory názvů, pokud v d:Map nebo d:Match dotazů XPath použili muset být uvedené.** Uzel d:Namespaces obsahuje sadu/seznam d:Namespace uzlů. Každý z nich jsou uvedeny jeden názvů používaný v odpovědi back-end služby. Následující seznam uvádí atribut uzel d:Namespace:

-   **d:Prefix:** Tuto předponu pro názvů, jak je vidět XML výsledky službou, například f:FirstName, f:LastName, kde f je předponu.
- **d:Uri:** Úplné URI oboru použitých v dokumentu výsledek. Představuje hodnotu, kterou předponu vyřeší za běhu.

**d:ErrorHandling** *(Volitelné)* – tento uzel obsahuje podmínky pro zpracování chyb. Každou z podmínek je ověřena výsledek, který vám vrátil obsahu poskytovatele služeb. Pokud podmínky odpovídá navrhované kód chyby koncový uživatel je vrácena chybová zpráva.

**d:ErrorHandling** *(Nepovinné)* a **d:Condition** *(volitelné)* - uzel podmínka obsahuje jeden podmínek, které se změnami výsledek vrácené obsahu poskytovatele služeb. Následují má **požadované** atributy:

- **d:Match:** Výraz XPath ověřuje, zda je daný uzel/hodnota účastní poskytovatel obsahu výstup XML. Výraz XPath je spuštěn výstup a měly vrátit hodnotu PRAVDA, pokud je podmínka POZVYHLEDAT nebo NEPRAVDA jinak.
- **d:HttpStatusCode:** Odpovídá HTTP stavový kód, který by měl být vrácen Marketplace v případě podmínka. Tržiště signalizes chyby uživateli prostřednictvím protokolu HTTP stavů. Seznam stavů HTTP jsou k dispozici na http://en.wikipedia.org/wiki/HTTP_status_code
- **d:ErrorMessage:** Chybová zpráva, která je vrácena – s kódem stavu HTTP – koncový uživatel. To by měl být popisný chybovou zprávu, která neobsahuje žádné tajemství.

**d:Title** *(Volitelné)* – umožňuje popisující název funkce. Pochází hodnota pro název

- Volitelné mapy atribut (xpath) určující, kde najdete název v odpovědi vrácených žádosti o službu.
- – Nebo – název zadaný jako hodnota uzlu.

**d:Rights** *(Volitelné)* - (například copyright) práva přidružený k této funkci. Hodnota pro práva pochází:

- Volitelné mapy atribut (xpath) určující, kde najdete práva v odpovědi vrácených žádosti o službu.
-   – Nebo – práva zadaný jako hodnota uzlu.

**d:Description** *(Volitelné)* - krátký popis funkce. Pochází hodnota pro popis

- Volitelné mapy atribut (xpath) určující, kde najdete popis v odpovědi vrácených žádosti o službu.
- – Nebo – popis zadaný jako hodnota uzlu.

**d:EmitSelfLink** - *najdete v článku nad příklad "FunctionImport"Procházení"vrátil data"*

**d:EncodeParameterValue** - volitelná rozšíření na OData

**d:QueryResourceCost** - volitelná rozšíření na OData

**d:Map** - volitelná rozšíření na OData

**d:Headers** - volitelná rozšíření na OData

**d:Headers** - volitelná rozšíření na OData

**d:Value** - volitelná rozšíření na OData

**d:HttpStatusCode** - volitelná rozšíření na OData

**d:ErrorMessage** - volitelná rozšíření na OData

## <a name="parameter-node"></a>Parametr uzel

Tento uzel představuje jeden parametr, který se zobrazí jako součást šabloně URI / požadavku uvedenou v uzel FunctionImport.

Stránky dokumentu velmi užitečné informace o uzel "Element parametru" nachází se na [zde](http://msdn.microsoft.com/library/ee473431.aspx) (pomocí rozevíracího seznamu **Jiné verze** vyberte jinou verzi v případě potřeby přečtěte následující dokumentaci). *Příklad:*`<Parameter Name="Query" Nullable="false" Mode="In" Type="String" d:Description="Query" d:SampleValues="Rudy Duck" d:EncodeParameterValue="true" MaxLength="255" FixedLength="false" Unicode="false" annotation:StoreGeneratedPattern="Identity"/>`

| Atribut parametru | Požaduje | Hodnota |
|----|----|----|
| Jméno | Ano | Název parametru. Malá a velká písmena.  Malá a velká písmena BaseUri. **Příklad:**`<Property Name="IsDormant" Type="Byte" />` |
| Typ | Ano | Typ parametrů. Hodnota musí být **EDMSimpleType** nebo komplexní typ, který je v rozsahu modelu. Další informace najdete v tématu "6 podporované parametr/vlastnost typů".  (Malá a velká písmena. První znak je velké písmeno, zbývající malými písmeny.)  Viz také, [koncepční modelu typy (CSDL)] [MSDNParameterLink]. **Příklad:**`<Property Name="LimitedPartnershipID " Type="Int32" />` |
| Režim | Ne | **V**, nebo vstup podle toho, zda je parametrem vstup, výstup nebo vstupní a výstupní parametr. ("Na" je k dispozici jen v Azure Marketplace.) **Příklad:**`<Parameter Name="StudentID" Mode="In" Type="Int32" />` |
| MaxLength | Ne | Neomezovat Délka parametru. **Příklad:**`<Property Name="URI" Type="String" MaxLength="100" FixedLength="false" Unicode="false" />` |
| Přesnost | Ne | Přesnost parametr. **Příklad:**`<Property Name="PreviousDate" Type="DateTime" Precision="0" />` |
| Stupnice | Ne | Měřítka parametr. **Příklad:**`<Property Name="SICCode" Type="Decimal" Precision="10" Scale="0" />` |

[MSDNParameterLink]: (http://msdn.microsoft.com/library/bb399548 (v=VS.100).aspx)

Atributy, které byly přidány specifikaci CSDL jsou následující:

| Atribut parametru | Popis |
|----|----|
| **d:Regex** *(Volitelné)* | Výkaz regex používaný k ověření vstupní hodnotu parametru. Pokud vstupní hodnoty neodpovídají příkazu hodnotu odmítnuto. To umožňuje zadat také možných hodnot, například ^ [0 – 9] +? $ povolit pouze čísla. **Příklad:** "< název parametru ="název"režimu ="V"typ ="Řetězec"d: s možnou hodnotou Null ="false"d:Regex =" ^ [a-zA-Z] * $"d:Description ="D název nesmí obsahovat mezery nebo jiné než anglické znaky-alfa"d:SampleValues ="Jirka|Petr|Kutěj|James "/ >" |
| **d:enum** *(Volitelné)* | Kanálu oddělený seznam hodnot platná pro parametr. Typ hodnoty musí odpovídat definovaný typu parametru. Příklad: "Angličtina|metriky|jako nezpracovaná`. Enum will display as a selectable dropdown list of parameters in the UI (service explorer). **Example:** `< název parametru = "Doba trvání" typ = "Řetězec" režim = "V" s možnou hodnotou Null = "true" d:Enum = "jeden rok|5years|10years "/ >" |
| **d: s možnou hodnotou Null** *(Volitelné)* | Umožňuje definovat, zda může být parametr null. Výchozí hodnota je: PRAVDA. Parametry, které jsou k dispozici jako část cesty v šabloně URI však nesmí být null. Pokud atribut nastavena na hodnotu false těchto parametrů – vstup uživatele přepsat. **Příklad:**`<Parameter Name="BikeType" Type="String" Mode="In" Nullable="false"/>` |
| **d:SampleValue** *(Volitelné)* | Ukázka hodnota zobrazí jako poznámka k desktopovému klientovi v uživatelském rozhraní.  Je možné přidat různé hodnoty pomocí seznamu kanálu oddělené, tedy "|b|c` **Example:** `< název parametru = "BikeOwner" typ = "Řetězec" režim = "V" d:SampleValues = "Jirka|Petr|Kutěj|James "/ >" |

## <a name="entitytype-node"></a>Typ entity uzel

Tento uzel představuje jeden z typů, které se vracejí z Marketplace koncovému uživateli. Obsahuje také mapování z výstupu, který vám vrátil obsahu poskytovatele služeb pro hodnoty, které vracejí do koncový uživatel.

Podrobnosti o tomto uzlu se nacházejí v [tady](http://msdn.microsoft.com/library/bb399206.aspx) (pomocí rozevíracího seznamu **Jiné verze** vyberte jinou verzi v případě potřeby přečtěte následující dokumentaci.)

| Název atributu | Požaduje | Hodnota |
|----|----|----|
| Jméno | Ano | Název typu entity. **Příklad:**`<EntityType Name="ListOfAllEntities" d:Map="//EntityModel">` |
| BaseType | Ne | Název jiného typu entitu, který je základní typ typu definovaného. **Příklad:**`<EntityType Name="PhoneRecord" BaseType="dqs:RequestRecord">` |

Atributy, které byly přidány specifikaci CSDL jsou následující:

**d:Map** - výraz XPath technologie spouštět oproti výstupu služby. Za předpokladu je, že výstupu služby obsahuje sadu prvky, které se opakují, jako například ATOM kanálů kde je sada uzlů položky, které se opakují. Každá z těchto opakující se uzly obsahuje jeden záznam. Výraz XPath je pak určené tak, aby ukazovaly na uzel jednotlivé opakující se ve výsledku služby poskytovatele obsahu, který obsahuje hodnoty u jednotlivých záznamů. Příklad výstup ze služby:

        `<foo>
          <bar> … content … </bar>
          <bar> … content … </bar>
          <bar> … content … </bar>
        </foo>`

Výraz XPath by být /foo/pruhu protože každý panelu uzel je opakující se uzel do výstupu a obsahuje aktuální obsah, který je vrácena koncový uživatel.

**Klíč** - Tenhle atribut je ignorován v Marketplace. ZBÝVAJÍCÍ podle webové služby, není obecně zpřístupnit primárního klíče.


## <a name="property-node"></a>Vlastnost uzel

Tento uzel obsahuje jednu vlastnost záznamu.

Podrobnosti o tomto uzlu se nacházejí v [http://msdn.microsoft.com/library/bb399546.aspx](http://msdn.microsoft.com/library/bb399546.aspx) (pomocí rozevíracího seznamu **Jiné verze** vyberte jinou verzi v případě potřeby přečtěte následující dokumentaci.) *Příklad:*
        `<EntityType Name="MetaDataEntityType" d:Map="/MyXMLPath">
        <Property Name="Name"   Type="String" Nullable="true" d:Map="./Service/Name" d:IsPrimaryKey="true" DefaultValue=”Joe Doh” MaxLength="25" FixedLength="true" />
        ...
        </EntityType>`

| Název_atributu | Povinné | Hodnota |
|----|----|----|
| Jméno | Ano | Název vlastnosti. |
| Typ | Ano | Zadejte hodnotu. Typ hodnoty vlastnosti musí být **EDMSimpleType** nebo složité typ (označený plně kvalifikovaný název), který je v rozsahu modelu. Další informace najdete v tématu typy koncepční modelu (CSDL). |
| S možnou hodnotou Null | Ne | **PRAVDA** (výchozí hodnota) nebo **False** v závislosti na tom, zda vlastnost může obsahovat hodnotu null. Poznámka: Ve verzi CSDL označen oboru [http://schemas.microsoft.com/ado/2006/04/edm](http://schemas.microsoft.com/ado/2006/04/edm) vlastnost komplexní typ musí mít s možnou hodnotou Null = "False". |
| Výchozí hodnota | Ne | Výchozí hodnota vlastnost. |
|MaxLength | Ne | Maximální délka hodnotu. |
| FixedLength | Ne | **True** nebo **False** v závislosti na tom, zda budou uloženy hodnotu jako řetězec fiexed délky. |
| Přesnost | Ne | Odkazuje na maximální počet číslic, které chcete uchovávat v číselnou hodnotu. |
| Stupnice | Ne | Maximální počet desetinných míst uchovávání číselnou hodnotu. |
| Unicode | Ne | **True** nebo **False** v závislosti na tom, zda hodnotu být uloženy jako řetězec znaků Unicode. |
| Řazení | Ne | Řetězec, který určuje pořadí řazení se nemusí používat ve zdroji dat. |
| Režim ConcurrencyMode | Ne | **Žádná** (výchozí hodnota) nebo **Pevná**. Pokud je hodnota nastavena na hodnotu **pevný**, hodnotu se použije v optimistické souběžné kontroly. |

Následuje další atributy, které byly přidány specifikaci CSDL:

**d:Map** - výraz XPath spouštět oproti službu výstup a extrahuje jednu vlastnost výstupu. Výraz XPath zadané je relativní uzel s opakováním vybraný uzel entity typu XPath. Také je možné určit absolutní XPath umožňuje včetně statické zdroje v žádném z výstupu uzly, jako je třeba autorských práv příkazu SELECT, který je dostupné jen jednou službu původní výstup ale by měly tvořit v jednotlivých řádcích do výstupu OData. Příklad ze služby:

        `<foo>
          <bar>
           <baz0>… value …</baz0>
           <baz1>… value …</baz1>
           <baz2>… value …</baz2>
          </bar>
        </foo>`

Výraz XPath tady bude ./bar/baz0 přejít na uzel baz0 z obsahu poskytovatele služeb.

**d:CharMaxLength** - typu řetězec, můžete určit maximální délka. Podívejte se na příklad DataService CSDL

**d:IsPrimaryKey** - označuje, pokud je sloupec primární klíč v tabulce nebo zobrazit. Viz příklad DataService CSDL.

Určuje **d:isExposed** – Pokud se zobrazí schématu tabulky (obvykle PRAVDA). Podívejte se na příklad DataService CSDL

**d:IsView** *(Volitelné)* - PRAVDA, pokud je to je založená na zobrazení, spíše než tabulky.  Podívejte se na příklad DataService CSDL

**d:Tableschema** - viz DataService CSDL příklad

**d:ColumnName** – je název sloupce v tabulce nebo zobrazit.  Podívejte se na příklad DataService CSDL

**d:IsReturned** – je logická hodnota, která určuje, pokud službu zpřístupní tuto hodnotu k desktopovému klientovi.  Podívejte se na příklad DataService CSDL

**d:IsQueryable** – je logická hodnota, která určuje sloupce lze nastavit v databázových dotazů.   Podívejte se na příklad DataService CSDL

**d:OrdinalPosition** – je sloupce číselnou pozici vzhledu, x, v tabulce nebo v zobrazení, kde x je od 1 do požadovaný počet sloupců v tabulce.  Podívejte se na příklad DataService CSDL

**d:DatabaseDataType** – je datového typu sloupce v databázi, tedy SQL datového typu. Podívejte se na příklad DataService CSDL

## <a name="supported-parametersproperty-types"></a>Typy podporovaného parametry a vlastností
Dále jsou podporované typy pro parametry a vlastnosti. (Malá a velká písmena)

| Základní typy | Popis |
|----|----|
| Hodnota Null | Představuje chybějící hodnoty. |
| Logická hodnota | Představuje matematické koncepci použití logických operátorů vracející na binární.|
| Bajt | 8bitovou celočíselnou hodnotu bez znaménka|
|Data a času| Představuje datum a čas, s hodnotami od půlnoci 12:00:00, 1, 1753. ledna až 11:59:59 P.M, prosinec 9999 N.L.|
|Decimal | Představuje číselné hodnoty s pevnou přesností a měřítko. Tento typ lze popsat číselné hodnoty od záporné 10 ^ 255 + 1 až kladné 10 ^ 255 -1|
| Dvojité | Představuje plovoucí desetinnou číslo s přesností na 15 platných číslic představující hodnot pomocí přibližné oblast odchylkou 2, 23E-308 odchylkou až 1, 79E +308. **Použití desetinných kvůli Exel export problém**|
| Jeden | Představuje plovoucí desetinnou číslo s přesností 7 číslicemi představující hodnot pomocí přibližné oblast přesností 1, 18E-38 odchylkou až 3, 40E +38|
|Identifikátor GUID |Představuje hodnotu jedinečný identifikátor 16 bajtů (128bitové) |
|Int16|Představuje podepsané 16bitovou celočíselnou hodnotu |
|Int32|Představuje hodnotu se znaménkem 32bitová verze |
|Int64|Představuje hodnotu se znaménkem 64bitová verze |
|Řetězec | Představuje pevnou - nebo proměnné znak délky |


## <a name="see-also"></a>Viz taky
- Pokud vás zajímají Principy celkové proces mapování OData a účel, v tomto článku [Data Service OData mapování](marketplace-publishing-data-service-creation-odata-mapping.md) kontroloval definice, struktury a pokyny.
- Pokud vás zajímají revizí příkladů naleznete v tomto článku jsou [Data Service OData mapování příklady](marketplace-publishing-data-service-creation-odata-mapping-examples.md) najdete v článku ukázkový kód a interpretaci syntaxi kódu a kontext.
- Pokud chcete vrátit do předepsaném cesta publikování datové služby Azure Marketplace, v tomto článku [Data služby publikování Guide](marketplace-publishing-data-service-creation.md).
