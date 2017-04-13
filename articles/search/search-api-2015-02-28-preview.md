<properties
   pageTitle="Azure služby REST API verze 2015-02-28náhled hledání | Microsoft Azure | Náhled Azure hledání rozhraní API"
   description="Azure služba REST verze rozhraní API 2015-02-28náhled hledání obsahuje pokusné funkce, jako jsou přirozeného jazyka analyzátory a moreLikeThis hledání."
   services="search"
   documentationCenter="na"
   authors="brjohnstmsft"
   manager="pablocas"
   editor=""/>

<tags
   ms.service="search"
   ms.devlang="rest-api"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="search"
   ms.date="09/07/2016"
   ms.author="brjohnst"/>

# <a name="azure-search-service-rest-api-version-2015-02-28-preview"></a>Azure hledání služba REST API: Verze 2015-02-28 – verze Preview

Tento článek je určený referenční dokumentaci pro `api-version=2015-02-28-Preview`. V tomto náhledu rozšiřuje aktuální obecně dostupných verzí [verze rozhraní api = 2015 02 28](https://msdn.microsoft.com/library/dn798935.aspx), zadáním následujících pokusné funkcí:

- `moreLikeThis`dotaz parametrů v [Hledat dokumenty](#SearchDocs) rozhraní API. Najde jiné dokumenty, které jsou důležité pro jiné konkrétního dokumentu.

Několik dalších díly `2015-02-28-Preview` rozhraní REST API jsou popsány samostatně. Jedná se o:

- [Hodnocení profilů](search-api-scoring-profiles-2015-02-28-preview.md)
- [Indexování](search-api-indexers-2015-02-28-preview.md)

Azure vyhledávací služba je k dispozici ve více verzích. Podrobnosti získáte [Vyhledávací služby správy verzí](http://msdn.microsoft.com/library/azure/dn864560.aspx) .

## <a name="apis-in-this-document"></a>Rozhraní API v tomto dokumentu

Rozhraní API pro službu Azure hledání podporuje dva syntaxe adresy URL pro rozhraní API operace: jednoduché a OData (podrobnosti najdete v části [Podpora OData (hledání Azure rozhraní API)](http://msdn.microsoft.com/library/azure/dn798932.aspx) ). V následujícím seznamu vidíte jednoduché syntaxe.

[Vytvoření indexu](#CreateIndex)

    POST /indexes?api-version=2015-02-28-Preview

[Aktualizovat rejstřík](#UpdateIndex)

    PUT /indexes/[index name]?api-version=2015-02-28-Preview

[Získání indexu](#GetIndex)

    GET /indexes/[index name]?api-version=2015-02-28-Preview

[Seznam indexy](#ListIndexes)

    GET /indexes?api-version=2015-02-28-Preview

[Získání statistických údajů indexu](#GetIndexStats)

    GET /indexes/[index name]/stats?api-version=2015-02-28-Preview

[Analýza test](#TestAnalyzer)

    POST /indexes/[index name]/analyze?api-version=2015-02-28-Preview

[Odstranění indexu](#DeleteIndex)

    DELETE /indexes/[index name]?api-version=2015-02-28-Preview

[Přidat, odstranit a aktualizovat Data v rámci rejstříku](#AddOrUpdateDocuments)

    POST /indexes/[index name]/docs/index?api-version=2015-02-28-Preview

[Hledání dokumentů](#SearchDocs)

    GET /indexes/[index name]/docs?[query parameters]
    POST /indexes/[index name]/docs/search?api-version=2015-02-28-Preview

[Vyhledávání dokumentu](#LookupAPI)

     GET /indexes/[index name]/docs/[key]?[query parameters]

[Počet dokumentů](#CountDocs)

    GET /indexes/[index name]/docs/$count?api-version=2015-02-28-Preview

[Návrhy](#Suggestions)

    GET /indexes/[index name]/docs/suggest?[query parameters]
    POST /indexes/[index name]/docs/suggest?api-version=2015-02-28-Preview

________________________________________
<a name="IndexOps"></a>
## <a name="index-operations"></a>Operace indexování

Můžete vytvořit a spravovat indexy v Azure vyhledávací služby serveru prostřednictvím jednoduchých HTTP požadavků (příspěvek, GET, umístění, odstranit) proti dané index zdroje. Při vytváření rejstříku nejdřív publikovat JSON dokument, který popisuje schématu index. Schéma definuje pole index, jejich datových typů a jak můžete použít (například v hledání, filtry, řazení nebo faceting). Definuje také bodování profily suggesters a další atributy pro nastavení chování funkce index.

Následující příklad uvádí obrázek schématu pro vyhledávání v hotelu informace s poli Popis definované ve dvou jazycích. Všimněte si, jak atributy určit, jak se používá pole. Například `hotelId` slouží jako klíč dokumentu (`"key": true`) a má z hledání vyloučit (`"searchable": false`).

    {
    "name": "hotels",  
    "fields": [
      {"name": "hotelId", "type": "Edm.String", "key": true, "searchable": false},
      {"name": "baseRate", "type": "Edm.Double"},
      {"name": "description", "type": "Edm.String", "filterable": false, "sortable": false, "facetable": false},
      {"name": "description_fr", "type": "Edm.String", "filterable": false, "sortable": false, "facetable": false, "analyzer": "fr.lucene"},
      {"name": "hotelName", "type": "Edm.String"},
      {"name": "category", "type": "Edm.String"},
      {"name": "tags", "type": "Collection(Edm.String)"},
      {"name": "parkingIncluded", "type": "Edm.Boolean"},
      {"name": "smokingAllowed", "type": "Edm.Boolean"},
      {"name": "lastRenovationDate", "type": "Edm.DateTimeOffset"},
      {"name": "rating", "type": "Edm.Int32"},
      {"name": "location", "type": "Edm.GeographyPoint"}
     ],
     "suggesters": [
      {
       "name": "sg",
       "searchMode": "analyzingInfixMatching",
       "sourceFields": ["hotelName"]
      }
     ]
    }

Po vytvoření indexu budete odesílat dokumenty, které naplnění index. Dalším krokem najdete v článku [Přidání nebo aktualizace dokumentů](#AddOrUpdateDocuments) .

Úvodní video k indexování hledání Azure najdete v článku [kanálu 9 cloudu průvodní dílu na Azure vyhledávání](http://go.microsoft.com/fwlink/p/?LinkId=511509).


<a name="CreateIndex"></a>
## <a name="create-index"></a>Vytvoření indexu

Index je primární prostředky k uspořádání a hledání dokumentů v Azure vyhledávání, podobně jako jak tabulky jsou uspořádány záznamů v databázi. Každý index má sbírky sledovaných dokumentů, že všechny odpovídají schématu index (názvy polí, datovým typům a vlastnostem), ale indexy také zadat další konstrukce (suggesters bodování profily a možnosti CORS), které definují jiných chování při hledání.

Můžete vytvořit nový index v rámci služby Azure vyhledávání pomocí žádost HTTP příspěvku nebo umístění. Textu žádosti o je JSON schéma, které určuje index a informace o konfiguraci.

    POST https://[service name].search.windows.net/indexes?api-version=[api-version]
    Content-Type: application/json
    api-key: [admin key]

Můžete taky můžete použít umístění a zadejte název index na URI. Pokud index neexistuje, bude vytvořena.

    PUT https://[search service url]/indexes/[index name]?api-version=[api-version]

Vytvoření rejstříku určuje strukturu dokumentů uložených v a používat vyhledávání operací. Vyplnění index je samostatná operace. V tomto kroku můžete [Přidat, aktualizovat nebo odstranění dokumenty](https://msdn.microsoft.com/library/azure/dn798930.aspx) operace nebo [indexování](https://msdn.microsoft.com/library/azure/mt183328.aspx) (umožňující podporovaných zdrojů dat). Odeslání dokumentů, vygeneruje se Invertovaný index.

**Poznámka**: maximální počet indexy povolené se liší podle ceny osy. Bezplatné služby umožňuje až 3 indexy. Standardní služba umožňuje 50 indexy za vyhledávací služby serveru. Podrobnosti najdete v článku [limity a omezení](http://msdn.microsoft.com/library/azure/dn798934.aspx) .

**Žádost o**

Protokol HTTPS je nutný pro všechny žádosti o služby. Žádost o **Create Index** lze vytvořit metodou příspěvku nebo umístění. Pokud chcete použít příspěvek, je nutné zadat název indexu v hlavním textu žádosti o spolu s definice schématu indexu. S umístění je název indexu část adresy URL. Pokud index neexistuje, bude vytvořen. Pokud již existuje, se aktualizuje na novou definici.

Název indexu musí být malá písmena, začínají na písmeno nebo číslo, bez lomítka nebo tečky a být menší než 128 znaků. Po název indexu začínající na písmeno nebo číslo, můžete zahrnout zbytek název písmeno, číslo a typ čáry, dokud pomlčky nejsou po sobě jdoucí.

`api-version` Požaduje. Seznam dostupných verzí naleznete v tématu [Vyhledávací služby správy verzí](http://msdn.microsoft.com/library/azure/dn864560.aspx) .

**Žádost o záhlaví**

Následující seznam popisuje povinných a nepovinných žádost o záhlaví.

- `Content-Type`: Povinné. Nastavte`application/json`
- `api-key`: Povinné. `api-key` Se provede
- ověření žádost vyhledávací služby. Je hodnotu řetězce jedinečné pro vaši službu. Žádost o **Vytvoření indexu** musí obsahovat `api-key` záhlaví nastavit klíč správce (na rozdíl od dotazu klíče).

Budete taky potřebovat název služby vytvářet požadavku na adresu URL. Získejte název služby a `api-key` z řídicího panelu služby na portálu Azure. V tématu [Vytvoření Azure vyhledávací služby v portálu](search-create-service-portal.md) pro navigaci na stránce Nápověda.

<a name="RequestData"></a>
**Žádost o textu syntaxi**

Textu žádosti o obsahuje definice schématu, který obsahuje seznam datových polí do dokumentů, které budou uloženy v tento index, datové typy, atributy i volitelný seznam bodování profily, které slouží ke skóre odpovídající dokumenty v době dotazu.

Všimněte si, že k příspěvku požádat o, je nutné zadat název indexu v hlavním textu žádosti o.

V indexu může být jenom jedno pole s klíčem. Musí být textové pole. Toto pole představuje jedinečný identifikátor pro každý dokument uložena v indexu.

Hlavní části rejstříku patřit následující úkoly:

- `name`
- `fields`která budou uloženy v indexu, včetně názvu, datový typ a vlastnosti, které definují povolenou akce na tomto poli.
- `suggesters`používá se pro automatického dokončování nebo automatickým dokončováním dotazy.
- `scoringProfiles`použít pro vlastní vyhledávání skóre řazení. Další informace najdete v článku [hodnocení profilů přidat](https://msdn.microsoft.com/library/azure/dn798928.aspx) .
- `analyzers`, `charFilters`, `tokenizers`, `tokenFilters` umožňuje určit, jak jsou vaše dokumenty/dotazy rozdělené indexovanou/s možností vyhledávání tokeny. Další informace najdete v článku [Analýza v Azure vyhledávání](https://aka.ms//azsanalysis) .
- `defaultScoringProfile`slouží k přepsat výchozí bodování chování.
- `corsOptions`Pokud chcete povolit více origin dotazů index.

Syntaxe strukturování datové žádost je následujícím způsobem. Žádost o vzorku je k dispozici další na v tomto tématu.

    {
      "name": (optional on PUT; required on POST) "name_of_index",
      "fields": [
        {
          "name": "name_of_field",
          "type": "Edm.String | Collection(Edm.String) | Edm.Int32 | Edm.Int64 | Edm.Double | Edm.Boolean | Edm.DateTimeOffset | Edm.GeographyPoint",
          "searchable": true (default where applicable) | false (only Edm.String and Collection(Edm.String) fields can be searchable),
          "filterable": true (default) | false,
          "sortable": true (default where applicable) | false (Collection(Edm.String) fields cannot be sortable),
          "facetable": true (default where applicable) | false (Edm.GeographyPoint fields cannot be facetable),
          "key": true | false (default, only Edm.String fields can be keys),
          "retrievable": true (default) | false,              
          "analyzer": "name of the analyzer used for search and indexing", (only if 'searchAnalyzer' and 'indexAnalyzer' are not set)
          "searchAnalyzer": "name of the search analyzer", (only if 'indexAnalyzer' is set and 'analyzer' is not set)
          "indexAnalyzer": "name of the indexing analyzer" (only if 'searchAnalyzer' is set and 'analyzer' is not set)
        }
      ],
      "suggesters": [
        {
          "name": "name of suggester",
          "searchMode": "analyzingInfixMatching" (other modes may be added in the future),
          "sourceFields": ["field1", "field2", ...]
        }
      ],
      "scoringProfiles": [
        {
          "name": "name of scoring profile",
          "text": (optional, only applies to searchable fields) {
            "weights": {
              "searchable_field_name": relative_weight_value (positive numbers),
              ...
            }
          },
          "functions": (optional) [
            {
              "type": "magnitude | freshness | distance | tag",
              "boost": # (positive number used as multiplier for raw score != 1),
              "fieldName": "...",
              "interpolation": "constant | linear (default) | quadratic | logarithmic",
              "magnitude": {
                "boostingRangeStart": #,
                "boostingRangeEnd": #,
                "constantBoostBeyondRange": true | false (default)
              },
              "freshness": {
                "boostingDuration": "..." (value representing timespan leading to now over which boosting occurs)
              },
              "distance": {
                "referencePointParameter": "...", (parameter to be passed in queries to use as reference location, see "scoringParameter" for syntax details)
                "boostingDistance": # (the distance in kilometers from the reference location where the boosting range ends)
              },
              "tag": {
                "tagsParameter": "..." (parameter to be passed in queries to specify list of tags to compare against target field, see "scoringParameter" for syntax details)
              }
            }
          ],
          "functionAggregation": (optional, applies only when functions are specified)
            "sum (default) | average | minimum | maximum | firstMatching"
        }
      ],
      "analyzers":(optional)[ ... ],
      "charFilters":(optional)[ ... ],
      "tokenizers":(optional)[ ... ],
      "tokenFilters":(optional)[ ... ],
      "defaultScoringProfile": (optional) "...",
      "corsOptions": (optional) {
        "allowedOrigins": ["*"] | ["origin_1", "origin_2", ...],
        "maxAgeInSeconds": (optional) max_age_in_seconds (non-negative integer)
      }
    }

**Atributy indexu**

Následující atributy můžete nastavit při vytváření rejstříku. Podrobnosti o bodování a bodování profilech najdete v tématu [Přidání hodnocení profily](https://msdn.microsoft.com/library/azure/dn798928.aspx).

`name`-Nastaví název pole.

`type`-Nastaví datový typ pole. Seznam podporovaných typů naleznete v tématu [Podporované datové typy](#DataTypes) .

`searchable`-Označí pole jako fulltextové možnost vyhledávání. To znamená, že bude projít analýzy například dělení slov při indexování. Pokud nastavíte `searchable` pole hodnotu jako "slunečno den" interně ho bude jde rozdělit na jednotlivé tokeny "slunečno" a "den". Díky fulltextové vyhledá těchto podmínek. Pole typu `Edm.String` nebo `Collection(Edm.String)` jsou `searchable` ve výchozím nastavení. Pole z jiných typů nemohou být `searchable`.

  - **Poznámka**: `searchable` pole používat mezery v rejstříku od Azure hledání se uloží další tokenizovaný verzi hodnotu pole pro hledání. Pokud chcete ušetřit místo ve odkazu a nemusíte pole, které chcete zahrnout do vyhledávání, nastavte `searchable` k `false`.

`filterable`– Umožňuje pole odkazuje na `$filter` dotazů. `filterable`se liší od `searchable` ve zpracování řetězce. Pole typu `Edm.String` nebo `Collection(Edm.String)` , které jsou `filterable` neprojdou dělení slov, tak, aby byly porovnání pro přesně odpovídá. Například, pokud nastavíte toto pole `f` "slunečno den" `$filter=f eq 'sunny'` bude hledat žádné shody, ale `$filter=f eq 'sunny day'` bude. Jsou všechna pole `filterable` ve výchozím nastavení.

`sortable`-Ve výchozím nastavení systému Seřadí výsledky podle skóre, ale v mnoha prostředí budou uživatelé chcete řadit podle polí v dokumentech. Pole typu `Collection(Edm.String)` nesmí být `sortable`. Všechny ostatní pole jsou `sortable` ve výchozím nastavení.

`facetable`-Používána v prezentaci výsledků hledání obsahující počet přístupů podle kategorie (například vyhledávání digitální fotoaparáty a přístupů Zobrazit značku, megapixely, cena, atd.). Tuto možnost nelze použít s poli typu `Edm.GeographyPoint`. Všechny ostatní pole jsou `facetable` ve výchozím nastavení.

  - **Poznámky**: pole typu `Edm.String` , které jsou `filterable`, `sortable`, nebo `facetable` může být nejvýše 32 KB délce. Toto je, protože taková pole je považováno za jeden hledaný termín a maximální délka období v Azure hledání je 32KB. Pokud potřebujete uložit více textu než v poli jednoho řetězce, musíte se výslovně nastavené `filterable`, `sortable`, a `facetable` k `false` v definice indexu.

  - **Poznámka**: Pokud pole obsahuje žádná z výše uvedených atributů nastavit `true` (`searchable`, `filterable`, `sortable`, nebo`facetable`) pole efektivně, nezahrne se Invertovaný index. Tato možnost je vhodná pro pole, která nejsou použity v dotazech, ale jsou potřebné ve výsledcích hledání. Kromě těchto polí z indexu zlepšuje výkon.

`key`-Označí pole jako obsahující jedinečné identifikátory dokumentů, které tvoří index. Přesně jedno pole musí být vybrána jako `key` pole a musí být typu `Edm.String`. Klíče slouží k vyhledání dokumentů přímo prostřednictvím rozhraní [API vyhledávání](#LookupAPI).

`retrievable`-Nastaví, zda pole může být vrácen ve výsledcích hledání.  To je užitečné, když chcete použít pole (například okraje) jako filtr, řazení nebo bodování mechanismus, ale nechcete, aby pole být viditelné pro koncového uživatele. Tento atribut musí být `true` pro `key` pole.

`analyzer`-Nastaví název analyzer použití pole na hledání a indexování čas. Povolené sadu hodnot najdete v článku [analyzátory](https://msdn.microsoft.com/library/mt605304.aspx). Tuto možnost, můžete použít jenom se `searchable` polí a nelze použít společně s buď `searchAnalyzer` nebo `indexAnalyzer`.  Jakmile Průvodce analýzou je vybrán, nemůžete už změnit pole.

`searchAnalyzer`-Nastaví název analyzer používány okamžiku vyhledávací pole. Povolené sadu hodnot najdete v článku [analyzátory](https://msdn.microsoft.com/library/mt605304.aspx). Tuto možnost, můžete použít jenom se `searchable` pole. Musí být nastavena spolu s `indexAnalyzer` a nelze ji nastavit spolu s `analyzer` možnost. Tento Průvodce analýzou lze aktualizovat na existující pole.

`indexAnalyzer`-Nastaví název analyzer používají indexovací současně pole. Povolené sadu hodnot najdete v článku [analyzátory](https://msdn.microsoft.com/library/mt605304.aspx). Tuto možnost, můžete použít jenom se `searchable` pole. Musí být nastavena spolu s `searchAnalyzer` a nelze ji nastavit spolu s `analyzer` možnost. Jakmile Průvodce analýzou je vybrán, nemůžete už změnit pole.

`suggesters`-Nastaví režim vyhledávání a polí, která jsou zdroje obsahu pro návrhy. Další informace najdete v článku [Suggesters](#Suggesters) .

`scoringProfiles`-Definuje vlastní bodování chování, které umožňují ovlivnit položek, které se zobrazí vyšší ve výsledcích hledání. Hodnocení profily vytvořené pole gramáží a funkcí. Další informace o atributy používané v bodování profilu najdete v článku [Přidání hodnocení profily](https://msdn.microsoft.com/library/azure/dn798928.aspx) .

<!-- This is a standalone topic in MSDN -->
<a name="LanguageSupport"></a>
**Podpora jazyků**

Vyhledávání pole projít analýza, které většina často zahrnuje dělení slov textu normalizace a termínů vyfiltrování. Ve výchozím nastavení jsou vyhledávání polí v Azure hledání analyzovat pomocí [Apache Lucene standardní analýza](http://lucene.apache.org/core/4_9_0/analyzers-common/index.html) , které rozdělí text na prvky, které následují pravidla["segmentace Unicode Text"](http://unicode.org/reports/tr29/) . Standardní analyzer navíc převede všechny znaky na samá velká písmena formuláře. Indexované dokumenty a hledané termíny absolvovat analýzu během indexování a zpracování dotazů.

Azure hledání podporuje různé jazyky. Každý jazyk vyžaduje analyzer nestandardní text, který představuje vlastnosti daný jazyk. Azure hledání vám přináší dva typy analyzátory:

- 35 analyzátory podporovaným Lucene.
- 50 analyzátory podporovaným speciální přirozeného jazyka Microsoft zpracování technologie používané v Office a Bing.

Některé vývojáři chtít intuitivnější jednoduché, otevřít zdroj řešení Lucene. Lucene analyzátory jsou rychleji, ale analyzátory Microsoft mít pokročilé funkce, například Lematizace, word decompounding (v jazycích třeba němčina, dánština, holandština, švédština, norština, estonština, dokončit, maďarština, slovenština) a entity rozpoznávání (adresy URL, e-mailů, kalendářní data, čísla). Pokud je to možné by měla běžet porovnání společnosti Microsoft a Lucene analyzátory rozhodnout, který z nich je vešel.

***Jaký je rozdíl mezi veřejným***

Průvodce analýzou Lucene pro angličtinu rozšiřuje standardní analýza. Odebere Přivlastňovací pád (koncové společnosti) ze slov, platí vyplývající podle [silné vyplývající algoritmus](http://tartarus.org/~martin/PorterStemmer/)a odstraní angličtiny [Zastavit slova](http://en.wikipedia.org/wiki/Stop_words).

Ve srovnání s Microsoft analyzer provede Lematizace místo vyplývající. Znamená to, že zvládne tvary tvary a nepravidelné slova mnohem lepší jaké výsledky ve víc relevantních výsledků hledání (podívejte se na video modul 7 [Azure hledání MVA](http://www.microsoftvirtualacademy.com/training-courses/adding-microsoft-azure-search-to-your-websites-and-apps) prezentace další podrobnosti).

Indexování s Microsoft analyzátory je průměrně dvakrát až třikrát nižší než ekvivalenty Lucene v závislosti na požadovaný jazyk. Výkon vyhledávání neměli to mít vliv na výrazně Průměrná velikost dotazů.

***Konfigurace***

Pro každé pole v definici indexu můžete nastavit `analyzer` vlastnost, která určuje, který jazyk a dodavatele názvu analyzer. Funkce stejné analýza se použije při indexování a vyhledávání pro toto pole.
Máte samostatných polích pro angličtinu, francouzštinu a španělštinu popisy hotelu, které jsou vedle sebe ve stejném indexu. [Parametr dotazu "searchFields"](#SearchQueryParameters) použijte k určení pole specifické pro určitý jazyk, které chcete hledat proti v dotazech. Můžete zkontrolovat příklady dotazu, které obsahují `analyzer` vlastnost v [Hledat dokumenty](#SearchDocs). 

***Analýza seznamu***

Tady je seznam jazyků podporovaných společně s názvy analyzer Lucene a Microsoft.

<table style="font-size:12">
    <tr>
        <th>Jazyk</th>
        <th>Název analýza společnosti Microsoft</th>
        <th>Název analyzer Lucene</th>
    </tr>
    <tr>
        <td>Arabština</td>
        <td>ar.microsoft</td>
        <td>ar.lucene</td>      
    </tr>
    <tr>
        <td>Arménština</td>
        <td></td>
        <td>hy.lucene</td>
    </tr>
    <tr>
        <td>Bengálština</td>
        <td>Bn.microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>Baskičtina</td>
        <td></td>
        <td>EU.lucene</td>
    </tr>
    <tr>
        <td>Bulharština</td>
        <td>BG.microsoft</td>
        <td>BG.lucene</td>
    </tr>
    <tr>
        <td>Katalánština</td>
        <td>CA.microsoft</td>
        <td>CA.lucene</td>          
    </tr>
    <tr>
        <td>Zjednodušená čínština</td>
        <td>zh Hans.microsoft</td>
        <td>zh Hans.lucene</td>     
    </tr>
    <tr>
        <td>Tradiční čínština</td>
        <td>zh Hant.microsoft</td>
        <td>zh Hant.lucene</td>     
    <tr>
    <tr>
        <td>Chorvatština</td>
        <td>hr.microsoft</td>
        <td/></td>
    </tr>
    <tr>
        <td>Čeština</td>
        <td>cs.microsoft</td>
        <td>cs.lucene</td>      
    </tr>    
    <tr>
        <td>Dánština</td>
        <td>da.microsoft</td>
        <td>da.lucene</td>      
    </tr>    
    <tr>
        <td>Holandština</td>
        <td>NL.microsoft</td>
        <td>NL.lucene</td>  
    </tr>    
    <tr>
        <td>Angličtina</td>        
        <td>en.microsoft</td>
        <td>en.lucene</td>      
    </tr>
    <tr>
        <td>Estonština</td>
        <td>et.microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>Finština</td>
        <td>Fi.microsoft</td>
        <td>Fi.lucene</td>      
    </tr>    
    <tr>
        <td>Francouzština</td>
        <td>FR.microsoft</td>
        <td>FR.lucene</td>      
    </tr>
    <tr>
        <td>Galicijština</td>
        <td></td>
        <td>GL.lucene</td>      
    </tr>
    <tr>
        <td>Němčina</td>
        <td>de.microsoft</td>
        <td>de.lucene</td>      
    </tr>
    <tr>
        <td>Řečtina</td>
        <td>El.microsoft</td>
        <td>El.lucene</td>      
    </tr>
    <tr>
        <td>Gudžarátština</td>
        <td>gu.microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>Hebrejština</td>
        <td>he.microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>Hindština</td>
        <td>Hi.microsoft</td>
        <td>Hi.lucene</td>      
    </tr>
    <tr>
        <td>Maďarština</td>      
        <td>HU.microsoft</td>
        <td>HU.lucene</td>
    </tr>
    <tr>
        <td>Islandština</td>
        <td>is.microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>Indonéština (Bahasa)</td>
        <td>ID.microsoft</td>
        <td>ID.lucene</td>      
    </tr>
    <tr>
        <td>Irština</td>
        <td></td>
        <td>GA.lucene</td>
    </tr>
    <tr>
        <td>Italština</td>
        <td>IT.microsoft</td>
        <td>IT.lucene</td>      
    </tr>
    <tr>
        <td>Japonština</td>
        <td>ja.microsoft</td>
        <td>ja.lucene</td>
        
    </tr>
    <tr>
        <td>Kannadština</td>
        <td>Ka.microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>Korejština</td>
        <td>Ko.microsoft</td>
        <td>Ko.lucene</td>
    </tr>
    <tr>
        <td>Lotyština</td>        
        <td>LV.microsoft</td>
        <td>LV.lucene</td>  
    </tr>
    <tr>
        <td>Litevština</td>
        <td>lt.microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>Malajalámština</td>
        <td>ml.microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>Malajština (latinka)</td>
        <td>MS.microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>Maráthština</td>
        <td>MR.microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>Norština</td>
        <td>NB.microsoft</td>
        <td>No.lucene</td>      
    </tr>
    <tr>
        <td>Perština</td>
        <td></td>
        <td>fa.lucene</td>      
    </tr>
    <tr>
        <td>Polština</td>
        <td>PL.microsoft</td>
        <td>PL.lucene</td>      
    </tr>
    <tr>
        <td>Portugalština (Brazílie)</td>
        <td>PT Br.microsoft</td>
        <td>PT Br.lucene</td>       
    </tr>
    <tr>
        <td>Portugalština (Portugalsko)</td>
        <td>PT Pt.microsoft</td>        
        <td>PT Pt.lucene</td>
    </tr>
    <tr>
        <td>Paňdžábština</td>
        <td>Pa.microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>Rumunština</td>
        <td>ro.microsoft</td>
        <td>ro.lucene</td>
    </tr>
    <tr>
        <td>Ruština</td>
        <td>RU.microsoft</td>
        <td>RU.lucene</td>  
    </tr>
    <tr>
        <td>Srbština (cyrilice)</td>
        <td>SR cyrillic.microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>Srbština (latinka)</td>
        <td>SR latin.microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>Slovenština</td>
        <td>Sk.microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>Slovinština</td>
        <td>SL.microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>Španělština</td>
        <td>ES.microsoft</td>
        <td>ES.lucene</td>
    </tr>
    <tr>
        <td>Švédština</td>
        <td>Sv.microsoft</td>
        <td>Sv.lucene</td>
    </tr>

    <tr>
        <td>Tamilština</td>
        <td>ta.microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>Telugština</td>
        <td>te.microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>Thajština</td>
        <td>TH.microsoft</td>
        <td>TH.lucene</td>
    </tr>
    <tr>
        <td>Turečtina</td>
        <td>TR.microsoft</td>
        <td>TR.lucene</td>      
    </tr>
    <tr>
        <td>Ukrajinština</td>
        <td>UK.microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>Urdština</td>
        <td>Your.microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>Vietnamština</td>
        <td>VI.microsoft</td>
        <td></td>
    </tr>
    <td colspan="3">Kromě Azure můžete vyhledávat konfigurace analyzer bez ohledu na jazyk</td>
    <tr>
        <td>Přeložení standardní ASCII</td>
        <td>standardasciifolding.lucene</td>
        <td>
        <ul>
            <li>Unicode text segmentace (standardní tokenizátoru)</li>
            <li>ASCII sklopným filtru – převede znaků kódu Unicode, které nechcete patří do skupiny nejdřív 127 znaků ASCII do ekvivalenty ASCII. To je užitečné pro odebrání diakritická znaménka.</li>
        </ul>
        </td>
    </tr>
</table>

Všechny analyzátory s názvy označena <i>lucene</i> jsou technologii [analyzátory Apache Lucene jazyk](http://lucene.apache.org/core/4_9_0/analyzers-common/overview-summary.html). Najdete další informace o ASCII přeložení filtr [tady](http://lucene.apache.org/core/4_9_0/analyzers-common/org/apache/lucene/analysis/miscellaneous/ASCIIFoldingFilter.html).

**Suggesters**

A `suggester` definuje polí, která v indexu slouží k podpoře automatického dokončování v hledání. Obvykle částečné vyhledávání řetězce jsou odeslány rozhraní [API návrhy](#Suggestions) během při zadávání vyhledávacího dotazu a rozhraní API vrátí sadu navrhované fráze. Suggester, který určíte v indexu Určuje, pole, která se používají k vytvoření automatickým dokončováním hledaných slov. Podrobné informace o konfiguraci najdete v článku [Suggesters](#Suggesters) .

**Hodnocení profilů**

A `scoringProfile` definuje vlastní bodování chování, které umožňují ovlivnit položek, které se zobrazí vyšší ve výsledcích hledání. Hodnocení profily vytvořené pole gramáží a funkcí. Je používat, zadejte profilu v řetězci dotazu podle názvu.

Výchozí bodování profilu pracuje na pozadí pro výpočet hledání skóre pro jednotlivé položky v sadě výsledků. Můžete vnitřní, nepojmenované bodování profilu. Můžete taky nastavit `defaultScoringProfile` používat vlastní profil jako výchozí, vyvolat kdykoli vlastní profil není zadané v řetězci dotazu.

Podrobnosti najdete v článku [hodnocení profilů přidat do indexu vyhledávání (Azure služba REST rozhraní API pro hledání)](search-api-scoring-profiles-2015-02-28-preview.md) .

**Možnosti CORS**

Javascript na straně klienta nelze všechny rozhraní API volat ve výchozím nastavení od prohlížeči zabrání všechny žádosti o křížově origin. Povolit CORS (sdílení zdrojů mezi Origin) nastavením `corsOptions` atribut umožňuje origin křížové dotazy pro index. Poznámka: Tento pouze dotaz rozhraní API podpory CORS z bezpečnostních důvodů. Pro CORS můžete nastavit následující možnosti:

- `allowedOrigins`(povinné): Toto je seznam typů, které budou mít udělený přístup pro index. To znamená, že všechny kód v JavaScriptu podávané množství z těchto typů bude moct pořizovat dotazu index (za předpokladu, že poskytuje správný klíč rozhraní API). Každý origin je obvykle formuláře `protocol://fully-qualified-domain-name:port` sice port často vynecháte argument. Podívejte se [v tomto článku](http://go.microsoft.com/fwlink/?LinkId=330822) další podrobnosti.
 - Pokud chcete povolit přístup ke všem typů, vkládat `*` jako jedna položka v `allowedOrigins` pole. Všimněte si, že **nedoporučuje praktické cvičení vyhledávací služby výroby.** Však může být užitečné pro vývoj nebo účely ladění.
- `maxAgeInSeconds`(volitelné): prohlížeče používat tuto hodnotu k určení doby trvání (v sekundách) do mezipaměti CORS výstupem odpovědi na ně. Musí být kladné celé číslo. Větší je tato hodnota, bude mít lepší výkon, ale tím déle, bude trvat CORS zásad změny se projeví. Pokud není nastavená, použije se výchozí doby trvání 5 minut.

<a name="CreateUpdateIndexExample"></a>
**Příklad textu žádosti o**

    {
      "name": "hotels",  
      "fields": [
        {"name": "hotelId", "type": "Edm.String", "key": true, "searchable": false},
        {"name": "baseRate", "type": "Edm.Double"},
        {"name": "description", "type": "Edm.String", "filterable": false, "sortable": false, "facetable": false},
        {"name": "description_fr", "type": "Edm.String", "filterable": false, "sortable": false, "facetable": false, "analyzer": "fr.lucene"},
        {"name": "hotelName", "type": "Edm.String"},
        {"name": "category", "type": "Edm.String"},
        {"name": "tags", "type": "Collection(Edm.String)"},
        {"name": "parkingIncluded", "type": "Edm.Boolean"},
        {"name": "smokingAllowed", "type": "Edm.Boolean"},
        {"name": "lastRenovationDate", "type": "Edm.DateTimeOffset"},
        {"name": "rating", "type": "Edm.Int32"},
        {"name": "location", "type": "Edm.GeographyPoint"}
      ],
      "suggesters": [
        {
          "name": "sg",
          "searchMode": "analyzingInfixMatching",
          "sourceFields": ["hotelName"]
        }
      ]
    }

**Odpověď**

Pro úspěšné žádost: "201 vytvořeno".

Ve výchozím nastavení bude obsahovat obsah odpovědí JSON definující index, který byl vytvořený. Pokud `Prefer` žádost o záhlaví je nastavený na `return=minimal`těle odpovědi se vyprázdní a stavový kód úspěchu bude "204 žádný obsah" místo "201 vytvořeno". Je to bez ohledu na to, zda umístění nebo příspěvku byla použita k vytvoření indexu.

**Poznámky:**

V současné době není omezenou podporu pro aktualizace schématu index. Všechny aktualizace schéma, které vyžadují přeindexování například změnit typy polí nepodporují aktuálně. Přestože existující pole nelze změnili nebo odstranili, nových polí můžete přidat existující index kdykoli. Po přidání nového pole se všechny existující dokumenty v indexu automaticky obsahovat hodnotu null tohoto pole. Žádná další úložný prostor bude potřeba, dokud nové dokumenty přidávají do indexu.

<a name="Suggesters"></a>
## <a name="suggesters"></a>Suggesters

Funkce návrhy hledání Azure je automatickým dokončováním nebo automatického dokončování dotazu funkce obsahující seznam možných hledaných slov v odpovědi vstupů částečné řetězec zadané do vyhledávacího pole. Určitě jste si všimli návrhy dotazů při použití komerční webu vyhledávače: psaní ".NET" v Bingu vytvoří seznam podmínek pro ".NET 4,5", ".NET Framework 3,5", a tak dále. Pokud chcete použít vyhledávací služba REST API, implementace návrhy ve vlastní aplikace Vyhledávací Azure vyžaduje:

- Povolte návrhy přidáním **suggester** konstrukce v rejstříku, pojmenování název, režim vyhledávání a seznam polí, u kterých automatickým dokončováním se spouští. Například pokud zadáte "mesto" zdrojové pole psát částečné vyhledávání řetězec "Moři" povede "hodnotu Dobříš", "Pobřežního" a "Seatac" (všechny tři jsou uvedeny názvy skutečné Město) jako návrhy dotazů nabízených uživatele.

- Návrhy vyvolejte tak, že zavoláte rozhraní [API návrhy](#Suggestions) v kódu aplikace. Obvykle částečné vyhledávání řetězce jsou odeslány službu a při zadávání vyhledávacího dotazu toto rozhraní API vrátí sadu navrhované fráze.

Tento článek vysvětluje, jak nakonfigurovat **suggester**. Přečtěte si seznam také rozhraní [API návrhy](#Suggestions) podrobné informace o tom, jak se používá suggester.

**Použití**

`Suggesters`vytvořené v indexu a práce nejlépe, když se použije pro návrh dokumenty spíše než volné termíny nebo fráze. Pole nejlepší candidate jsou názvy, jména a jiné poměrně krátkém věty, které můžete určit položky. Méně efektivní jsou opakované pole, například kategorie a značky, nebo velmi dlouhý polí, například polí popisy a komentáře.

Jako součást definici indexu, můžete přidat jeden suggester k `suggesters` kolekce. Vlastnosti, které definují suggester patřit následující úkoly:

- `name`: Název suggester. Použijte název suggester při volání `suggest` rozhraní API.
- `searchMode`: Strategie používaný k vyhledání candidate fráze. Je režim jen v současnosti podporované `analyzingInfixMatching`, která provede flexibilní shodu mezi větami na začátku nebo uprostřed věty.
- `sourceFields`: Seznam jedné nebo více polí, která jsou zdroje obsahu pro návrhy. Pouze pole typu `Edm.String` a `Collection(Edm.String)` může být zdroje pro návrhy. Lze pouze pole, která nemají analyzer vlastní jazykové nastavení.

**Příklad suggester**

Suggester je součástí indexu. Jedinou suggester může existovat v `suggesters` kolekce v aktuální verzi vedle kolekci polí a `scoringProfiles`.

        {
          "name": "hotels",
          "fields": [
             . . .
           ],
          "suggesters": [
            {
            "name": "sg",
            "searchMode": "analyzingInfixMatching",
            "sourceFields: ["hotelName", "category"]
            }
          ],
          "scoringProfiles": [
             . . .
          ]
        }

> [AZURE.NOTE]  Pokud jste použili veřejné náhled verzi Azure hledání `suggesters` nahradí starší vlastnost typu boolean (`"suggestions": false`), který podporuje jen předponu návrhy krátké řetězce (3-25 znaků). Nahrazení, `suggesters`, podporuje infix shody, které najde odpovídající podmínky na začátku nebo uprostřed obsah pole s lepší odchylkou chyb v řetězci pro vyhledávání. Obecně dostupných počínaje to je teď jenom provádění návrhy rozhraní API. Starší `suggestions` vlastnost, která byla zavedená v `api-version=2014-07-31-Preview` bude dál fungovat i v této verzi, ale není funkční v `2015-02-28` nebo novější verze Azure vyhledávání.

<a name="UpdateIndex"></a>
## <a name="update-index"></a>Aktualizovat rejstřík

Je možné aktualizovat existující index v Azure vyhledávání pomocí žádost HTTP umístění. Aktualizace může obsahovat přidávání nových polí do existujícího schématu, změna CORS možnosti a úprava bodování profilů. Další informace najdete v tématu [Přidání hodnocení profily](https://msdn.microsoft.com/library/azure/dn798928.aspx) . Zadejte název indexu aktualizovat na vyžádání URI:

    PUT https://[search service url]/indexes/[index name]?api-version=[api-version]
    Content-Type: application/json
    api-key: [admin key]

**Důležité:** Podpora pro aktualizace schématu index se omezí na operacích, které nevyžadují opětovné vytvoření indexu vyhledávání. Všechny aktualizace schéma, které vyžadují přeindexování, například změnit typy polí nepodporují aktuálně. Nové pole lze přidat kdykoli, i když existující pole nelze změnit nebo odstranit. Platí pro `suggesters`. Nové pole může přidat k suggester v době pole budou přidána, ale pole nelze odebraná z `suggesters` a nejde přidat existující pole `suggesters`.

Při přidávání nového pole do rejstříku, všechny existující dokumenty v indexu automaticky obsahovat hodnotu null tohoto pole. Žádná další úložný prostor bude potřeba, dokud nové dokumenty přidávají do indexu.

**Žádost o**

Protokol HTTPS je nutný pro všechny žádosti o služby. **Aktualizovat rejstřík** žádost je vytvořen pomocí protokolu HTTP umístění. S umístění je název indexu část adresy URL. Pokud index neexistuje, bude vytvořen. Pokud již existuje index, se aktualizuje na novou definici.

Název indexu musí být malá písmena, začínají na písmeno nebo číslo, bez lomítka nebo tečky a být menší než 128 znaků. Po název indexu začínající na písmeno nebo číslo, můžete zahrnout zbytek název písmeno, číslo a typ čáry, dokud pomlčky nejsou po sobě jdoucí.

`api-version=[string]`(povinné). Verze preview je `api-version=2015-02-28-Preview`. Podrobnosti a alternativní verze najdete v článku [Správa verzí vyhledávací služby](http://msdn.microsoft.com/library/azure/dn864560.aspx) .

**Žádost o záhlaví**

Následující seznam popisuje povinných a nepovinných žádost o záhlaví.

- `Content-Type`: Povinné. Nastavte`application/json`
- `api-key`: Povinné. `api-key` Slouží k ověření žádost vyhledávací služby. Je hodnotu řetězce jedinečné pro vaši službu. **Aktualizovat rejstřík** žádost musí obsahovat `api-key` záhlaví nastavit klíč správce (na rozdíl od dotazu klíče).

Budete taky potřebovat název služby vytvářet požadavku na adresu URL. Získejte název služby a `api-key` z řídicího panelu služby na portálu Azure. V tématu [Vytvoření Azure vyhledávací služby v portálu](search-create-service-portal.md) pro navigaci na stránce Nápověda.

**Žádost o textu syntaxi**

Při aktualizaci existující index, textu musí obsahovat původní definice schématu plus nových polí, kterou chcete přidat, jakož i změněné bodování profily, suggesters a CORS možnosti, pokud existuje. Pokud nejsou změny bodování profily a možnosti CORS, musí obsahovat původní ze kdy byl vytvořen index. Obecně nejlepší vzor pro účely aktualizace má načítat definici indexu s získáte, upravit a pak aktualizujte ji o umístění.

Syntaxe schématu použitá k vytvoření indexu je uveden tady pro usnadnění. Další informace najdete v článku [Vytvoření indexu](#CreateIndex) .

    {
      "name": (optional) "name_of_index",
      "fields": [
        {
          "name": "name_of_field",
          "type": "Edm.String | Collection(Edm.String) | Edm.Int32 | Edm.Int64 | Edm.Double | Edm.Boolean | Edm.DateTimeOffset | Edm.GeographyPoint",
          "searchable": true (default where applicable) | false (only Edm.String and Collection(Edm.String) fields can be searchable),
          "filterable": true (default) | false,
          "sortable": true (default where applicable) | false (Collection(Edm.String) fields cannot be sortable),
          "facetable": true (default where applicable) | false (Edm.GeographyPoint fields cannot be facetable),
          "key": true | false (default, only Edm.String fields can be keys),
          "retrievable": true (default) | false, 
          "analyzer": "name of the analyzer used for search and indexing", (only if 'searchAnalyzer' and 'indexAnalyzer' are not set)
          "searchAnalyzer": "name of the search analyzer", (only if 'indexAnalyzer' is set and 'analyzer' is not set)
          "indexAnalyzer": "name of the indexing analyzer" (only if 'searchAnalyzer' is set and 'analyzer' is not set)
        }
      ],
      "suggesters": [
        {
          "name": "name of suggester",
          "searchMode": "analyzingInfixMatching" (other modes may be added in the future),
          "sourceFields": ["field1", "field2", ...]
        }
      ],
      "scoringProfiles": [
        {
          "name": "name of scoring profile",
          "text": (optional, only applies to searchable fields) {
            "weights": {
              "searchable_field_name": relative_weight_value (positive numbers),
              ...
            }
          },
          "functions": (optional) [
            {
              "type": "magnitude | freshness | distance | tag",
              "boost": # (positive number used as multiplier for raw score != 1),
              "fieldName": "...",
              "interpolation": "constant | linear (default) | quadratic | logarithmic",
              "magnitude": {
                "boostingRangeStart": #,
                "boostingRangeEnd": #,
                "constantBoostBeyondRange": true | false (default)
              },
              "freshness": {
                "boostingDuration": "..." (value representing timespan leading to now over which boosting occurs)
              },
              "distance": {
                "referencePointParameter": "...", (parameter to be passed in queries to use as reference location, see "scoringParameter" for syntax details)
                "boostingDistance": # (the distance in kilometers from the reference location where the boosting range ends)
              },
              "tag": {
                "tagsParameter": "..." (parameter to be passed in queries to specify list of tags to compare against target field, see "scoringParameter" for syntax details)
              }
            }
          ],
          "functionAggregation": (optional, applies only when functions are specified)
            "sum (default) | average | minimum | maximum | firstMatching"
        }
      ],
      "analyzers":(optional)[ ... ],
      "charFilters":(optional)[ ... ],
      "tokenizers":(optional)[ ... ],
      "tokenFilters":(optional)[ ... ],
      "defaultScoringProfile": (optional) "...",
      "corsOptions": (optional) {
        "allowedOrigins": ["*"] | ["origin_1", "origin_2", ...],
        "maxAgeInSeconds": (optional) max_age_in_seconds (non-negative integer)
      }
    }


**Odpověď**

Pro úspěšné žádost: "204 žádný obsah".

Ve výchozím nastavení se vyprázdní obsah odpovědí. Ale pokud `Prefer` žádost o záhlaví je nastavený na `return=representation`, obsah odpovědí bude obsahovat JSON definice index, který byl aktualizován. V tomto případě stavový kód úspěchu bude "200 OK".

**Aktualizace definice indexu s vlastní analyzátory**

Jakmile je definován analyzer, tokenizátoru, tokenu filtru nebo filtru znak, nelze změnit. Nové lze přidat do existující index, jenom v případě `allowIndexDowntime` příznaku nastavena na true v žádosti o aktualizaci indexu: 

`PUT https://[search service name].search.windows.net/indexes/[index name]?api-version=[api-version]&allowIndexDowntime=true`

Všimněte si, že tuto operaci vloží index offline aspoň několik sekund, než, způsobují vaší indexování a dotazů selhání. Dostupnost výkonu a zapsat indexu může být zrakově několik minut po aktualizaci index nebo déle obrovské indexy.

<a name="ListIndexes"></a>
## <a name="list-indexes"></a>Seznam indexy

Operace **Seznamu indexy** vrátí seznam indexy aktuálně v Azure vyhledávací služby.

    GET https://[service name].search.windows.net/indexes?api-version=[api-version]
    api-key: [admin key]

**Žádost o**

Protokol HTTPS je nutný pro všechny žádosti o služby. Žádost o **Seznamu indexy** lze vytvořit pomocí metody GET.

`api-version=[string]`(povinné). Verze preview je `api-version=2015-02-28-Preview`. Podrobnosti a alternativní verze najdete v článku [Správa verzí vyhledávací služby](http://msdn.microsoft.com/library/azure/dn864560.aspx) .

**Žádost o záhlaví**

Následující seznam popisuje povinných a nepovinných žádost o záhlaví.

- `api-key`: Povinné. `api-key` Slouží k ověření žádost vyhledávací služby. Je hodnotu řetězce jedinečné pro vaši službu. Žádost o **Indexy seznam** musí obsahovat `api-key` nastavena na kód Product key pro správu (na rozdíl od dotazu klíče).

Budete taky potřebovat název služby vytvářet požadavku na adresu URL. Získejte název služby a `api-key` z řídicího panelu služby na portálu Azure. V tématu [Vytvoření Azure vyhledávací služby v portálu](search-create-service-portal.md) pro navigaci na stránce Nápověda.

**Žádost o textu**

Žádná.

**Odpověď**

Stavový kód: 200 OK budou vráceny úspěšné odpověď.

Tady je text odpovědi příkladu:

    {
      "value": [
        {
          "name": "Books",
          "fields": [
            {"name": "ISBN", ...},
            ...
          ]
        },
        {
          "name": "Games",
          ...
        },
        ...
      ]
    }

Všimněte si, že je můžete vyfiltrovat odpověď dolů jenom vlastnosti, které vás zajímá. Pokud chcete jenom seznam jmen index, pomocí například OData `$select` dotazu možnost:

    GET /indexes?api-version=2015-02-28-Preview&$select=name

V tomto případě odpověď z výše uvedený příklad vypadat takto:

    {
      "value": [
        {"name": "Books"},
        {"name": "Games"},
        ...
      ]
    }

Toto je užitečné Postup uložení šířku pásma máte spoustu indexy v vyhledávací služby.

<a name="GetIndex"></a>
## <a name="get-index"></a>Získání indexu

**Získání Index** operace získá definici indexu z Azure vyhledávání.

    GET https://[service name].search.windows.net/indexes/[index name]?api-version=[api-version]
    api-key: [admin key]

**Žádost o**

Protokol HTTPS je potřebný pro žádosti o služby. Žádost o **Získat Index** lze vytvořit pomocí metody GET.

[Název indexu] v žádosti o URI Určuje index, který se vrátíte z kolekce indexy.

`api-version=[string]`(povinné). Verze preview je `api-version=2015-02-28-Preview`. Podrobnosti a alternativní verze najdete v článku [Správa verzí vyhledávací služby](http://msdn.microsoft.com/library/azure/dn864560.aspx) .

**Žádost o záhlaví**

Následující seznam popisuje povinných a nepovinných žádost o záhlaví.

- `api-key`: Čím `api-key` slouží k ověření žádost vyhledávací služby. Je hodnotu řetězce jedinečné pro vaši službu. Žádost o **Získat indexu** musí obsahovat `api-key` nastavena na kód Product key pro správu (na rozdíl od dotazu klíče).

Budete taky potřebovat název služby vytvářet požadavku na adresu URL. Získejte název služby a `api-key` z řídicího panelu služby na portálu Azure. V tématu [Vytvoření Azure vyhledávací služby v portálu](search-create-service-portal.md) pro navigaci na stránce Nápověda.

**Žádost o textu**

Žádná.

**Odpověď**

Stavový kód: 200 OK budou vráceny úspěšné odpověď.

Podívejte se na příklad JSON [Vytvoření a aktualizace rejstříku](#CreateUpdateIndexExample) příklad užitečného odpověď.

<a name="DeleteIndex"></a>
## <a name="delete-index"></a>Odstranění indexu

Operace **Odstranění indexu** odebere rejstřík a související dokumenty z Azure vyhledávací služby. Název indexu můžete přepnout na řídicím panelu služby na portálu Azure nebo z rozhraní API. Podrobnosti najdete v části [Seznam indexy](#ListIndexes) .

    DELETE https://[service name].search.windows.net/indexes/[index name]?api-version=[api-version]
    api-key: [admin key]

**Žádost o**

Protokol HTTPS je potřebný pro žádosti o služby. **Odstranění indexu** žádost lze vytvořit pomocí metody odstranit.

[Název indexu] v žádosti o URI Určuje index, který chcete odstranit z kolekce indexy.

`api-version=[string]`(povinné). Verze preview je `api-version=2015-02-28-Preview`. Podrobnosti a alternativní verze najdete v článku [Správa verzí vyhledávací služby](http://msdn.microsoft.com/library/azure/dn864560.aspx) .

**Žádost o záhlaví**

Následující seznam popisuje povinných a nepovinných žádost o záhlaví.

- `api-key`: Povinné. `api-key` Slouží k ověření žádost vyhledávací služby. Je hodnotu řetězce jedinečné pro adresy URL služby. **Odstranění indexu** žádost musí obsahovat `api-key` záhlaví nastavit klíč správce (na rozdíl od dotazu klíče).

Budete taky potřebovat název služby vytvářet požadavku na adresu URL. Získejte název služby a `api-key` z řídicího panelu služby na portálu Azure. V tématu [Vytvoření Azure vyhledávací služby v portálu](search-create-service-portal.md) pro navigaci na stránce Nápověda.

**Žádost o textu**

Žádná.

**Odpověď**

Stavový kód: 204 bez obsahu budou vráceny úspěšné odpověď.

<a name="GetIndexStats"></a>
## <a name="get-index-statistics"></a>Získání statistických údajů indexu

Operace **Získání statistických údajů Index** vrátí z Azure vyhledávání počet dokumentů pro aktuální index plus využití úložiště.

    GET https://[service name].search.windows.net/indexes/[index name]/stats?api-version=[api-version]
    api-key: [admin key]

> [AZURE.NOTE] Statistických údajů o počtu a úložiště velikost dokumentu se shromažďují každých několik minut, ne v reálném čase. Proto statistiku vrácené toto rozhraní API nemusí odrážet změny způsobená poslední operace indexování.

**Žádost o**

Protokol HTTPS je nutný pro všechny žádosti o služby. Žádost o **Získání statistických údajů Index** lze vytvořit pomocí metody GET.

[Název indexu] v žádosti o URI říká službu vrátíte statistiku indexu pro zadaný index.

`api-version=[string]`(povinné). Verze preview je `api-version=2015-02-28-Preview`. Podrobnosti a alternativní verze najdete v článku [Správa verzí vyhledávací služby](http://msdn.microsoft.com/library/azure/dn864560.aspx) .


**Žádost o záhlaví**

Následující seznam popisuje povinných a nepovinných žádost o záhlaví.

- `api-key`: Čím `api-key` slouží k ověření žádost vyhledávací služby. Je hodnotu řetězce jedinečné pro vaši službu. Žádost o **Získání statistických údajů indexu** musí obsahovat `api-key` nastavena na kód Product key pro správu (na rozdíl od dotazu klíče).

Budete taky potřebovat název služby vytvářet požadavku na adresu URL. Získejte název služby a `api-key` z řídicího panelu služby na portálu Azure. V tématu [Vytvoření Azure vyhledávací služby v portálu](search-create-service-portal.md) pro navigaci na stránce Nápověda.

**Žádost o textu**

Žádná.

**Odpověď**

Stavový kód: 200 OK budou vráceny úspěšné odpověď.

Obsah odpovědí je v tomto formátu:

    {
      "documentCount": number,
      "storageSize": number (size of the index in bytes)
    }

<a name="TestAnalyzer"></a>
## <a name="test-analyzer"></a>Analýza test

Rozhraní **API analyzovat** ukazuje, jak analyzer konce textu do tokenů.

    POST https://[service name].search.windows.net/indexes/[index name]/analyze?api-version=[api-version]
    Content-Type: application/json
    api-key: [admin key]

**Žádost o**

Protokol HTTPS je nutný pro všechny žádosti o služby. Žádost o **Analýze rozhraní API** lze vytvořit pomocí metody příspěvek.

`api-version=[string]`(povinné). Verze preview je `api-version=2015-02-28-Preview`. Podrobnosti a alternativní verze najdete v článku [Správa verzí vyhledávací služby](http://msdn.microsoft.com/library/azure/dn864560.aspx) .


**Žádost o záhlaví**

Následující seznam popisuje povinných a nepovinných žádost o záhlaví.

- `api-key`: Čím `api-key` slouží k ověření žádost vyhledávací služby. Je hodnotu řetězce jedinečné pro vaši službu. Žádost o **Analýze rozhraní API** musí obsahovat `api-key` nastavena na kód Product key pro správu (na rozdíl od dotazu klíče).

Budete taky potřebovat název indexu a název služby vytvářet požadavku na adresu URL. Získejte název služby a `api-key` z řídicího panelu služby na portálu Azure. V tématu [Vytvoření Azure vyhledávací služby v portálu](search-create-service-portal.md) pro navigaci na stránce Nápověda.

**Žádost o textu**

    {
      "text": "Text to analyze",
      "analyzer": "analyzer_name"
    }

nebo

    {
      "text": "Text to analyze",
      "tokenizer": "tokenizer_name",
      "tokenFilters": (optional) [ "token_filter_name" ],
      "charFilters": (optional) [ "char_filter_name" ]
    }

`analyzer_name`, `tokenizer_name`, `token_filter_name` a `char_filter_name` musí být platné názvy předdefinovaný nebo vlastní analyzátory tokenizers, tokenu filtry a filtry znak pro index. Další informace o procesu lexikální analýzy najdete v článku [Analýza v Azure vyhledávání](https://aka.ms/azsanalysis).

**Odpověď**

Stavový kód: 200 OK budou vráceny úspěšné odpověď.

Obsah odpovědí je v tomto formátu:

    {
      "tokens": [
        {
          "token": string (token),
          "startOffset": number (index of the first character of the token),
          "endOffset": number (index of the last character of the token),
          "position": number (position of the token in the input text)
        },
        ...
      ]
    }

**Analýza rozhraní API příklad**

**Žádost o**

    {
      "text": "Text to analyze",
      "analyzer": "standard"
    }

**Odpověď**

    {
      "tokens": [
        {
          "token": "text",
          "startOffset": 0,
          "endOffset": 4,
          "position": 0
        },
        {
          "token": "to",
          "startOffset": 5,
          "endOffset": 7,
          "position": 1
        },
        {
          "token": "analyze",
          "startOffset": 8,
          "endOffset": 15,
          "position": 2
        }
      ]
    }

________________________________________
<a name="DocOps"></a>
## <a name="document-operations"></a>Operace dokumentu

V Azure vyhledávání rejstřík uložený v cloudu a pomocí JSON dokumenty, které uložíte do služby vyplněné. Všechny dokumenty, které jste nahráli zahrnuje souhrnu dat vyhledávání. Dokumenty obsahovat pole, některé z nich jsou tokenizovaný do hledané termíny jako jsou odeslány. `/docs` Segmentu URL v rozhraní API pro hledání Azure představuje sbírky sledovaných dokumentů v indexu. Všechny operace provádějí kolekci třeba nahrávání slučování, odstranění nebo dotaz na dokumenty dělat umístěte v kontextu jedné index, takže adresy URL pro tyto operace vždy začíná `/indexes/[index name]/docs` názvu dané index.

Kód aplikace musíte buď generovat JSON dokumenty na Azure vyhledávání nebo [indexování](https://msdn.microsoft.com/library/dn946891.aspx) umožňuje načítat dokumenty, pokud je zdroj dat databáze SQL Azure nebo DocumentDB. Indexy jsou obvykle vyplněné z jednoho datovou sadu, který zadáte.

Je třeba naplánovat na s jeden dokument u každé položky, kterou chcete prohledat. Pronájem aplikace movie může mít jeden dokument za film storefront aplikace může mít jeden dokument za SKU, aplikace online software může být jeden dokument podle kurzu, pevně výzkumu může mít jeden dokument pro každou vysokoškolských papíru v jejich úložiště atd.

Dokumenty se skládají z jednoho nebo více polí. Pole může obsahovat text, který je tokenizovaný hledáním Azure do hledaných slov, jakož i bez tokenizovaný nebo jiné než textové hodnoty, které lze použít filtry nebo bodování profily. Jména, datových typů a hledání podporované funkce pro každé pole jsou určena schématu index. Některého z polí v každém schématu indexu musí být určeny jako ID a hodnoty pro pole ID, které jednoznačně identifikuje tento dokument v indexu musí mít každý dokument. Všechny ostatní pole dokumentu jsou volitelné a bude výchozí na hodnotu null, pokud zadávat. Všimněte si, že hodnoty null neprovádět místo do indexu vyhledávání.

Před můžete odesílat dokumenty, třeba jste již vytvořili index na služby. Podrobnosti o tohoto prvního kroku najdete v článku [Vytvoření indexu](#CreateIndex) .

<a name="AddOrUpdateDocuments"></a>
## <a name="add-update-or-delete-documents"></a>Přidat, aktualizovat a odstraňovat dokumenty

Můžete nahrávat, dokumentů hromadné korespondence, hromadné korespondence nebo odeslání nebo odstranění ze zadaného indexu pomocí HTTP příspěvek. Pro většího počtu aktualizace dávky dokumentů nahoru (1000 dokumenty na dávku) nebo asi 16 MB na dávku doporučuje.

    POST https://[service name].search.windows.net/indexes/[index name]/docs/index?api-version=[api-version]
    Content-Type: application/json
    api-key: [admin key]

**Žádost o**

Protokol HTTPS je nutný pro všechny žádosti o služby. Můžete nahrávat, dokumentů hromadné korespondence, hromadné korespondence nebo odeslání nebo odstranění ze zadaného indexu pomocí HTTP příspěvek.

Žádost o URI obsahuje [název indexu] určující index, který chcete publikovat dokumenty. Můžete publikovat pouze dokumentů do jedné indexu.

`api-version=[string]`(povinné). Verze preview je `api-version=2015-02-28-Preview`. Podrobnosti a alternativní verze najdete v článku [Správa verzí vyhledávací služby](http://msdn.microsoft.com/library/azure/dn864560.aspx) .

**Žádost o záhlaví**

Následující seznam popisuje povinných a nepovinných žádost o záhlaví.

- `Content-Type`: Povinné. Nastavte`application/json`
- `api-key`: Povinné. `api-key` Slouží k ověření žádost vyhledávací služby. Je hodnotu řetězce jedinečné pro vaši službu. **Přidání dokumentů** žádost musí obsahovat `api-key` záhlaví nastavit klíč správce (na rozdíl od dotazu klíče).

Budete taky potřebovat název služby vytvářet požadavku na adresu URL. Získejte název služby a `api-key` z řídicího panelu služby na portálu Azure. V tématu [Vytvoření Azure vyhledávací služby v portálu](search-create-service-portal.md) pro navigaci na stránce Nápověda.

**Žádost o textu**

Textu žádosti o obsahuje jeden nebo více dokumentů chcete indexovat. Dokumenty jsou určeny jedinečné klíčem. Každý dokument je přidružená k akci: nahrávání, hromadné korespondence, mergeOrUpload nebo odstranit. Odeslání žádosti o musí obsahovat data dokumentu jako sady párů klíč/hodnota.

    {
      "value": [
        {
          "@search.action": "upload (default) | merge | mergeOrUpload | delete",
          "key_field_name": "unique_key_of_document", (key/value pair for key field from index schema)
          "field_name": field_value (key/value pairs matching index schema)
            ...
        },
        ...
      ]
    }

> [AZURE.NOTE] Klávesy dokumentů může obsahovat pouze písmena, čísla, pomlčky ("-"), podtržítka ("_") a rovnítko (="). Další informace najdete v tématu [zásady vytváření názvů pravidla](https://msdn.microsoft.com/library/azure/dn857353.aspx).

**Akce dokumentů**

- `upload`: Odeslání akce je podobný "upsert" dokument budou vloženy Pokud je nové a aktualizace a nahradit, pokud existuje. Všimněte si, že jsou všechna pole nahradit v případě aktualizace.
- `merge`: Sloučení aktualizuje existujícího dokumentu zadané pole. Pokud dokument neexistuje, se nepovede hromadné korespondence. Pole, které zadáte do sloučení nahradí existující pole v dokumentu. Jedná se o pole typu `Collection(Edm.String)`. Například, pokud dokument obsahuje pole "značky" s hodnotou `["budget"]` a nutná ke sloučení s hodnotou `["economy", "pool"]` "značek", bude výsledný hodnota pole "visačky" `["economy", "pool"]`. **Není** možné `["budget", "economy", "pool"]`.
- `mergeOrUpload`: se chová jako `merge` Pokud dokumentu s konci daného klíč již existuje v indexu. Pokud dokument neexistuje se chová jako funkce `upload` s novým dokumentem.
- `delete`: Odstranit Odstraní zadaný dokument z indexu. Všimněte si, že všechna pole určíte v `delete` operace než pole klíče budou ignorovat. Pokud chcete z dokumentu odebrat jednotlivá pole, použijte `merge` místo a jednoduše nastavte v poli explicitně na `null`.

**Odpověď**

Stavový kód 200 (OK) budou vráceny úspěšné odpověď, což znamená, že všechny položky byly úspěšně indexování. To je označen `status` vlastnost nastavena na hodnotu true pro všechny položky, stejně jako `statusCode` nastavenou na 201 (pro nově nahraný dokumenty) nebo 200 (pro sloučené nebo odstraněné dokumenty):

    {
      "value": [
        {
          "key": "unique_key_of_new_document",
          "status": true,
          "errorMessage": null,
          "statusCode": 201
        },
        {
          "key": "unique_key_of_merged_document",
          "status": true,
          "errorMessage": null,
          "statusCode": 200
        },
        {
          "key": "unique_key_of_deleted_document",
          "status": true,
          "errorMessage": null,
          "statusCode": 200
        }
      ]
    }  

Při alespoň jednu položku nebyla indexována úspěšně se vrátí stavový kód 207 (více stavů). Mají položky neindexovaných `status` pole nastavena na hodnotu false. `errorMessage` a `statusCode` vlastnosti výskyt znamená důvod, proč indexování chyby:

    {
      "value": [
        {
          "key": "unique_key_of_document_1",
          "status": false,
          "errorMessage": "The search service is too busy to process this document. Please try again later.",
          "statusCode": 503
        },
        {
          "key": "unique_key_of_document_2",
          "status": false,
          "errorMessage": "Document not found.",
          "statusCode": 404
        },
        {
          "key": "unique_key_of_document_3",
          "status": false,
          "errorMessage": "Index is temporarily unavailable because it was updated with the 'allowIndexDowntime' flag set to 'true'. Please try again later.",
          "statusCode": 422
        }
      ]
    }  

Následující tabulka uvádí různé kódy stav jednoho dokumentu, které může vrátit v odpovědi. Poznámka: některé označují problémy s žádostí o sebe sama, zatímco ostatní označují dočasné chybový stav. Ten, který se má opakovat zpožděním.

<table style="font-size:12">
    <tr>
        <th>Stavový kód</th>
        <th>Význam</th>
        <th>Opakovatelná</th>
        <th>Poznámky</th>
    </tr>
    <tr>
        <td>200</td>
        <td>Dokument byl úspěšně změnili nebo odstranili.</td>
        <td>není k dispozici</td>
        <td>Operace odstranění se <a href="https://en.wikipedia.org/wiki/Idempotence">idempotent</a>. To znamená i když není k dispozici klíč dokumentu v rejstříku, pokoušející operace odstranění se tento klíč povede 200 stavový kód.</td>
    </tr>
    <tr>
        <td>201</td>
        <td>Dokument byl úspěšně vytvořen.</td>
        <td>není k dispozici</td>
        <td></td>
    </tr>
    <tr>
        <td>400</td>
        <td>V dokumentu, který brání indexované došlo k chybě.</td>
        <td>Ne</td>
        <td>Chybová zpráva v odpovědi výskyt znamená, proč není správné snažit s dokumentem.</td>
    </tr>
    <tr>
        <td>404</td>
        <td>Dokument nelze sloučit, protože klávesu dané neexistuje v indexu.</td>
        <td>Ne</td>
        <td>K této chybě může dojít pro ukládání vzhledem k tomu vytvářet nové dokumenty a ho nedochází k odstranění protože jsou <a href="https://en.wikipedia.org/wiki/Idempotence">idempotent</a>.</td>
    </tr>
    <tr>
        <td>409</td>
        <td>Při pokusu o vytvoření rejstříku dokumentu byl zjištěn konflikt verzí.</td>
        <td>Ano</td>
        <td>K tomu může dojít, když se pokoušíte více než jednou souběžně indexovat do stejného dokumentu.</td>
    </tr>
    <tr>
        <td>422</td>
        <td>Index je dočasně nedostupný, protože se aktualizovaly se nastavit příznak "allowIndexDowntime" na "true".</td>
        <td>Ano</td>
        <td></td>
    </tr>
    <tr>
        <td>503</td>
        <td>Vyhledávací služba je dočasně nedostupný, případně kvůli velkém zatížení.</td>
        <td>Ano</td>
        <td>Kód čekat, než v tomto případě nebo rizika prodloužit nedostupnost služby.</td>
    </tr>
</table> 

**Poznámka**: Pokud váš kód klienta často zjistí 207 odpověď, důvodem je, že systému zatížení. To můžete ověřit kontrolou `statusCode` vlastnost 503. Pokud jde o případ, doporučujeme ***omezení indexování žádosti***. Jinak Pokud indexování přenosy nemá subside, systém může spustit odmítnutí všechny žádosti o s 503 chyby.

Stavový kód 429 označuje, že jste překročili kvóty na počet dokumentů na index. Vytvořit nový index nebo aktualizace pro vyšší omezení kapacity.

**Příklad:**

    {
      "value": [
        {
          "@search.action": "upload",
          "hotelId": "1",
          "baseRate": 199.0,
          "description": "Best hotel in town",
          "description_fr": "Meilleur hôtel en ville",
          "hotelName": "Fancy Stay",
          "category": "Luxury",
          "tags": ["pool", "view", "wifi", "concierge"],
          "parkingIncluded": false,
          "smokingAllowed": false,
          "lastRenovationDate": "2010-06-27T00:00:00Z",
          "rating": 5,
          "location": { "type": "Point", "coordinates": [-122.131577, 47.678581] }
        },
        {
          "@search.action": "upload",
          "hotelId": "2",
          "baseRate": 79.99,
          "description": "Cheapest hotel in town",
          "description_fr": "Hôtel le moins cher en ville",
          "hotelName": "Roach Motel",
          "category": "Budget",
          "tags": ["motel", "budget"],
          "parkingIncluded": true,
          "smokingAllowed": true,
          "lastRenovationDate": "1982-04-28T00:00:00Z",
          "rating": 1,
          "location": { "type": "Point", "coordinates": [-122.131577, 49.678581] }
        },
        {
          "@search.action": "merge",
          "hotelId": "3",
          "baseRate": 279.99,
          "description": "Surprisingly expensive",
          "lastRenovationDate": null
        },
        {
          "@search.action": "delete",
          "hotelId": "4"
        }
      ]
    }
________________________________________
<a name="SearchDocs"></a>
## <a name="search-documents"></a>Hledání dokumentů

Operace **vyhledávání** je jako požadavek GET nebo příspěvku a určuje parametry poskytující kritéria pro výběr odpovídající dokumenty.

    GET https://[service name].search.windows.net/indexes/[index name]/docs?[query parameters]
    api-key: [admin or query key]

    POST https://[service name].search.windows.net/indexes/[index name]/docs/search?api-version=[api-version]
    Content-Type: application/json
    api-key: [admin or query key]

**Kdy použít příspěvek místo GET**

Pokud používáte HTTP GET volání **hledání** rozhraní API, musíte mějte na paměti, že zkrátit adresu URL žádost o nesmí překročit 8 KB. Je to obvykle dostatečně u většiny aplikací. Některé aplikace předložit však obrovské dotazy nebo výrazy filtru OData. Pro tyto aplikace pomocí HTTP POST je lepší volbou, protože umožňuje větší filtrech a dotazech než NAČÍST. Počet termíny nebo klauzulí v dotazu se publikovat, je omezující faktor není velikost nezpracovanými dotazu, protože omezení velikosti příspěvku je přibližně 16 MB.

> [AZURE.NOTE] I když je omezení velikosti příspěvek obrovské, nemůže být libovolně složité vyhledávacích dotazů a výrazy filtru. Další informace o omezení hledání dotazu a filtr složitost naleznete v tématu [Lucene syntaxi dotazu](https://msdn.microsoft.com/library/mt589323.aspx) a [syntaxi výrazů OData](https://msdn.microsoft.com/library/dn798921.aspx) .

**Žádost o**

Protokol HTTPS je potřebný pro žádosti o služby. Požadavku **hledání** můžete vytvořen pomocí metody GET nebo příspěvek.

Identifikátor URI požadavku určuje index, který dotazu pro všechny dokumenty, které odpovídají parametry. Parametry jsou zadány v řetězci dotazu v případě požadavky GET a v žádosti o vyžaduje zadání textu v případě příspěvek.

Osvědčený při vytváření požadavky GET nezapomeňte [kódovat URL](https://msdn.microsoft.com/library/system.uri.escapedatastring.aspx) parametry specifické dotazu při volání rozhraní REST API přímo. Pro **vyhledávání** operací jedná se o:

- `$filter`
- `facet`
- `highlightPreTag`
- `highlightPostTag`
- `search`
- `moreLikeThis`

Adresa URL kódování doporučujeme pouze na výše uvedené parametry dotazu. Pokud se omylem URL kódovat řetězec celý dotazu (všechno po?), žádosti o přeruší.

Navíc kódování adresa URL je nutné pouze při volání získat přímo pomocí rozhraní REST API. Žádné kódování adres URL stačí při volání **vyhledávání** pomocí příspěvku nebo při použití [knihovny .NET klienta](https://msdn.microsoft.com/library/dn951165.aspx), který pracuje s kódování adres URL pro vás.

<a name="SearchQueryParameters"></a>
**Parametry dotazu**

**Hledání** přijme, zadejte kritéria dotazu a také určete nastavení hledání několik parametrů. Zadáte, že tyto parametry v adrese URL řetězec dotazu při volání **hledání** prostřednictvím GET a jako JSON vlastnosti v hlavním textu žádosti o při volání **hledání** prostřednictvím příspěvek. Syntaxe pro některé parametry je mírně liší GET a publikovat. Tyto rozdíly jsou označeny jako příslušných níže:

`search=[string]`(volitelné) – text, který chcete vyhledat. Všechny `searchable` pole se prohledávají ve výchozím nastavení, pokud `searchFields` není zadán. Při hledání `searchable` pole hledaný text samotné je tokenizovaný, takže více podmínek mohou být odděleny prázdného místa mezi stránkami (například: `search=hello world`). Vyhledat všechny termínů, použijte `*` (to může být užitečné pro dotazů boolean filtru). Vynechání tento parametr má stejný výsledek jako nastavení na `*`. Podrobnosti o syntaxi hledání najdete v článku [Jednoduché syntaxe dotazu](https://msdn.microsoft.com/library/dn798920.aspx) .

  - **Poznámka**: výsledky někdy může být překvapivé při dotazování přes `searchable` pole. Tokenizátoru obsahuje logiky pro zpracování případech společné v anglickém textu například apostrofy, čárky u čísel, atd. Například `search=123,456` se budou shodovat jeden termín 123,456 spíše než jednotlivé položky 123 a 456, protože čárky jsou používána jako oddělovače 1000 pro vysoké počty v angličtině. Proto doporučujeme používat prázdného místa mezi stránkami spíše než interpunkce k oddělení podmínek uvedených v `search` parametr.

`searchMode=any|all`(volitelné, ve výchozím nastavení `any`) – jestli některé nebo všechny hledané termíny musí odpovídat k sečtení dokument jako shodu.

`searchFields=[string]`(volitelné) – seznam jmen hodnotami oddělenými čárkou pole Hledat zadaný text. Cílová pole musí být označen jako `searchable`.

`queryType=simple|full`(volitelné, ve výchozím nastavení `simple`) - li nastavit na stav "jednoduchý" hledaný text je odkaz interpretován pomocí jednoduchého dotazovací jazyk, který umožňuje symboly jako +, * a "". Dotazy jsou vyhodnoceny ve všech oblastech vyhledávání (nebo pole vyznačené `searchFields`) ve všech dokumentech ve výchozím nastavení. Pokud je nastaven typ dotazu na `full` hledaný text je odkaz interpretován pomocí dotazovací jazyk Lucene, které umožňuje specifické pro pole a váženého vyhledávání. Zvláštnosti na syntaxe hledání najdete v článku [Jednoduché syntaxi dotazu](https://msdn.microsoft.com/library/dn798920.aspx) a [Lucene syntaxe dotazu](https://msdn.microsoft.com/library/mt589323.aspx) . 
 
> [AZURE.NOTE] Rozsah hledání v Lucene dotazovací jazyk není podporována ve prospěch $filter poskytující podobnou funkci obsahují.

`moreLikeThis=[key]`(volitelné) **Důležité:** Tato funkce je dostupná pouze v `2015-02-28-Preview`. Tuto možnost nelze použít v dotazu, který obsahuje parametr hledání text `search=[string]`. `moreLikeThis` Parametr najde dokumentů, které jsou podobné dokument určený klíčem dokumentu. Pokud je žádost o hledání volání s `moreLikeThis`, Seznam hledaných slov generováno podle frekvence a vzácnost termínů ve zdrojovém dokumentu. Tyto termíny se pak používají k provádění žádost. Ve výchozím nastavení celý obsah `searchable` pole se považuje za Pokud `searchFields` se používá k omezení polí, která se prohledávají.  

`$skip=#`(volitelné) - počet výsledků hledání přejděte; Nesmí být větší než 100 000. Pokud potřebujete skenování dokumentů v pořadí, ale nemůžete používat `$skip` z důvodu omezení, zvažte použití `$orderby` ke klíči úplně objednali a `$filter` s rozsahem dotazu místo.

> [AZURE.NOTE] Při volání **vyhledávání** pomocí příspěvek, tento parametr jmenuje `skip` namísto `$skip`.

`$top=#`(volitelné) - počet výsledků hledání můžete načíst. Můžete používá ve spojení s `$skip` implementovat stránkování na straně klienta výsledků hledání.

> [AZURE.NOTE] Při volání **vyhledávání** pomocí příspěvek, tento parametr jmenuje `top` namísto `$top`.

`$count=true|false`(volitelné, ve výchozím nastavení `false`)-určuje, zda chcete načíst celkový počet různých hodnot výsledků. Toto je počet všechny dokumenty, které odpovídají `search` a `$filter` parametry ignorování `$top` a `$skip`. Nastavení tuto hodnotu na `true` může mít vliv na výkon. Všimněte si, že počet vrátí přibližnou.

> [AZURE.NOTE] Při volání **vyhledávání** pomocí příspěvek, tento parametr jmenuje `count` namísto `$count`.

`$orderby=[string]`(volitelné) – seznam oddělený čárkami výrazů řadit výsledky podle. Každý výraz může být název pole nebo volání `geo.distance()` (funkce). Každý výraz můžete následovat `asc` uvést vzestupně, a `desc` označíte sestupně. Výchozí hodnota je vzestupně. Vazby bude rozdělené podle skóre POZVYHLEDAT dokumentů. Pokud ne `$orderby` není zadán výchozí pořadí řazení je sestupně podle skóre POZVYHLEDAT dokumentu. Existuje limit pro 32 klauzuli `$orderby`.

> [AZURE.NOTE] Při volání **vyhledávání** pomocí příspěvek, tento parametr jmenuje `orderby` namísto `$orderby`.

`$select=[string]`(volitelné) – seznam polí k načtení s hodnotami oddělenými čárkou. Pokud tento parametr, všechna pole označené jako zobrazitelné ve výsledcích vyhledávání ve schématu jsou však započítávány. Můžete také explicitně požádat o všechna pole nastavením tento parametr `*`.

> [AZURE.NOTE] Při volání **vyhledávání** pomocí příspěvek, tento parametr jmenuje `select` namísto `$select`.

`facet=[string]`(žádný nebo více) - pole omezující vlastnost tak, že. Volitelně řetězec může obsahovat parametrů přizpůsobit faceting vyjádřenou hodnotami oddělenými čárkou `name:value` dvojice. Jsou platné parametry:

- `count`(maximální počet podmínka podmínek; výchozí hodnota je 10). Existuje bez omezení, ale vyšší hodnoty vzniknou odpovídající snížení výkonu, zejména pokud působí pole obsahuje velký počet jedinečných podmínky.
  - Příklad: `facet=category,count:5` získá pět hlavních kategorií ve výsledcích podmínka.  
  - **Poznámka**: Pokud `count` parametr je menší než počet jedinečných podmínky, nemusí být přesné výsledky. Toto je kvůli způsob, jakým faceting dotazy jsou rozvržena shards. Zvětšení `count` obvykle zvyšuje přesnost počtu termínů, ale s platbou výkonu.
- `sort`(nějaká `count` seřadit *Sestupně* podle počtu, `-count` seřadit *Vzestupně* podle počtu, `value` seřadit *Vzestupně* podle hodnoty, nebo `-value` seřadit *Sestupně* podle hodnoty)
  - Příklad: `facet=category,count:3,sort:count` obdrží horní tři kategorie v podmínka výsledky sestupně podle počet dokumentů s jednotlivými jmény město. Například pokud horní tři kategorie jsou rozpočtu, model a luxusní, rozpočet má 5 přístupů, model obsahuje 6 a luxusní má 4, potom bloků bude v takovém pořadí, model, rozpočet, luxusní.
  - Příklad: `facet=rating,sort:-value` vytvoří bloků pro všechny možné hodnocení sestupně podle hodnoty. Například pokud hodnocení od 1 do 5, bloků bude objednat 5, 4, 3, 2, 1, bez ohledu na to, kolik dokumentů shodují každý hodnocení.
- `values`(odděleného znakem svislé numerické nebo `Edm.DateTimeOffset` hodnoty určující dynamické sadu hodnot položku podmínka)
  - Příklad: `facet=baseRate,values:10|20` vytvořeny tři bloky: jeden pro základní sazby 0 až, ale ne včetně 10, jednu 10 až s výjimkou 20 a druhý pro 20 nebo vyšší.
  - Příklad: `facet=lastRenovationDate,values:2010-02-01T00:00:00Z` vytvoří dva bloky: jeden pro hotely renovované před únor 2010 a jeden pro hotely renovované únor 1st, 2010 nebo novější.
- `interval`(interval celé číslo větší než 0 pro čísla, nebo `minute`, `hour`, `day`, `week`, `month`, `quarter`, `year` pro datum časových hodnot)
  - Příklad: `facet=baseRate,interval:100` vytvoří bloky založené na základním sazba oblastí velikosti 100. Například jsou-li základní sazby všechny mezi 60 $ a $600, bude bloky 0 až 100, 100 200 200 300, 300 400, 400-500 a 500 600.
  - Příklad: `facet=lastRenovationDate,interval:year` vytváří jeden blok pro každý rok po hotely byly renovovanou.
- `timeoffset`([+-] hh: mm, [+-] hh: mm, nebo [+-] [hh) `timeoffset` je nepovinný krok. Můžete zkombinovat jenom s `interval` možnost a pouze v případě, že použité u pole typu `Edm.DateTimeOffset`. Hodnota určuje časový posun UTC účtu při nastavování omezení času.
  - Příklad: `facet=lastRenovationDate,interval:day,timeoffset:-01:00` používá okraj den, který se spustí u UTC 01:00:00 (půlnoci v časovém pásmu cílový)
- **Poznámka**: `count` a `sort` kombinací ve specifikaci stejné podmínka, ale nejde zkombinovat s `interval` nebo `values`, a `interval` a `values` nelze společně.
- **Poznámka**: Interval fasetami na datum a čas jsou vyhodnocované podle času UTC Pokud `timeoffset` není zadán. Příklad: pro `facet=lastRenovationDate,interval:day`, den hranice začíná 00:00:00 UTC. 

> [AZURE.NOTE] Při volání **vyhledávání** pomocí příspěvek, tento parametr jmenuje `facets` namísto `facet`. Také můžete zadat ho jako pole JSON řetězce kde každý řetězec je výraz samostatném podmínka.

`$filter=[string]`(volitelné) - strukturovaných hledaný výraz v standardní syntaxi OData. Podrobné informace o podmnožinu gramatiky OData výraz, který podporuje Azure hledání najdete v článku [Syntaxi výrazů OData](#ODataExpressionSyntax) .

> [AZURE.NOTE] Při volání **vyhledávání** pomocí příspěvek, tento parametr jmenuje `filter` namísto `$filter`.

`highlight=[string]`(volitelné) - sadu názvy polí hodnotami oddělenými čárkou použité pro záznam nalezen se zvýrazní. Pouze `searchable` pole se dají použít ke zvýraznění přístupů.

`highlightPreTag=[string]`(volitelné, ve výchozím nastavení `<em>`) - A řetězec značkou, odvozeno strefíte zvýraznění. Je potřeba nastavit s `highlightPostTag`.

> [AZURE.NOTE] Při volání **vyhledávání** pomocí GET, vyhrazené znaky v adrese URL musí být v procentech kódovaný (například % 23 místo #).

`highlightPostTag=[string]`(volitelné, ve výchozím nastavení `</em>`)-řetězec značku, která přidá strefíte zvýraznění. Je potřeba nastavit s `highlightPreTag`.

> [AZURE.NOTE] Při volání **vyhledávání** pomocí GET, vyhrazené znaky v adrese URL musí být v procentech kódovaný (například % 23 místo #).

`scoringProfile=[string]`(volitelné) – název bodování profil, který chcete vyhodnotit musí odpovídat výsledků hledání shody dokumentů za účelem řazení výsledků.

`scoringParameter=[string]`(žádný nebo více) - označuje hodnoty pro každý parametr podle hodnocení funkci (například `referencePointParameter`) formátu `name-value1,value2,...`.

- Například pokud bodování profil definuje funkce s parametrem s názvem "mylocation" možnost řetězce dotazu by `&scoringParameter=mylocation--122.2,44.8`. První pomlčku odděluje název ze seznamu hodnot a druhý pomlčku je součástí první hodnotu (délky v tomto příkladu).
- Například pro značku zvýšení, která může obsahovat středníky, můžete bodování parametrů uniknout tyto hodnoty v seznamu pomocí jednoduchých uvozovek. Pokud hodnoty samotné obsahují apostrofy, můžete je uniknout předchozího.
  - Například, pokud máte značku zvýšení parametr s názvem "mytag" a chcete pomoct na značku hodnoty "Ahoj, O'Brien" a "Miklus", dotaz bude možnost řetězec `&scoringParameter=mytag-'Hello, O''Brien',Smith`. Všimněte si, že nabídek pouze potřebné pro hodnoty, které obsahují čárkami.

> [AZURE.NOTE] Při volání **vyhledávání** pomocí příspěvek, tento parametr jmenuje `scoringParameters` namísto `scoringParameter`. Zadáte také jako pole JSON řetězce kde každý řetězec je samostatná `name-values` pár.

`minimumCoverage`(volitelné, výchozí hodnota je 100)-číslo mezi 0 a 100 procentuální hodnota index, který musí být předmětem vyhledávacího dotazu v pořadí pro dotaz být hlášené jako úspěchu. Ve výchozím nastavení celý index musí být k dispozici nebo `Search` vrátí HTTP stavový kód 503. Pokud nastavíte `minimumCoverage` a `Search` úspěšné, bude vrácená HTTP 200 a zahrnout `@search.coverage` hodnotu na základě procentuální hodnota index, který jste obdrželi v dotazu.

> [AZURE.NOTE] Nastavení tento parametr hodnotou nižší než 100 může být užitečné pro zajištění dostupnosti hledání i pro služby s jedinou otevřené. Ale ne všech odpovídající dokumenty jsou zaručena prezentovat ve výsledcích hledání. Pokud hledání odvolání zprávy proběhne aplikaci důležitější než dostupnosti a pak je vhodné ponechat `minimumCoverage` na výchozí hodnotě 100.

`api-version=[string]`(povinné). Verze preview je `api-version=2015-02-28-Preview`. Podrobnosti a alternativní verze najdete v článku [Správa verzí vyhledávací služby](http://msdn.microsoft.com/library/azure/dn864560.aspx) .

Poznámka: pro tuto operaci `api-version` není zadán jako parametr dotazu v adrese URL bez ohledu na to, zda zavoláte **vyhledávání** s GET nebo příspěvek.

**Žádost o záhlaví**

Následující seznam popisuje povinných a nepovinných žádost o záhlaví.

- `api-key`: Čím `api-key` slouží k ověření žádost vyhledávací služby. Je hodnotu řetězce jedinečné pro adresy URL služby. Požadavku **hledání** můžete zadat kláves pro správu šipka nebo dotazu pro `api-key`.

Budete taky potřebovat název služby vytvářet požadavku na adresu URL. Získejte název služby a `api-key` z řídicího panelu služby na portálu Azure. V tématu [Vytvoření Azure vyhledávací služby v portálu](search-create-service-portal.md) pro navigaci na stránce Nápověda.

**Žádost o textu**

Pro získání: žádný.

Příspěvku:

    {
      "count": true | false (default),
      "facets": [ "facet_expression_1", "facet_expression_2", ... ],
      "filter": "odata_filter_expression",
      "highlight": "highlight_field_1, highlight_field_2, ...",
      "highlightPreTag": "pre_tag",
      "highlightPostTag": "post_tag",
      "minimumCoverage": # (% of index that must be covered to declare query successful; default 100),
      "moreLikeThis": "document_key" (mutually exclusive with "search" parameter),
      "orderby": "orderby_expression",
      "scoringParameters": [ "scoring_parameter_1", "scoring_parameter_2", ... ],
      "scoringProfile": "scoring_profile_name",
      "search": "simple_query_expression",
      "searchFields": "field_name_1, field_name_2, ...",
      "searchMode": "any" (default) | "all",
      "select": "field_name_1, field_name_2, ...",
      "skip": # (default 0),
      "top": #
    }

**Pokračování odpovědí částečné vyhledávání**

Někdy Azure hledání nemůže vrátit všechny požadované výsledky jedinou odpověď vyhledávání. K tomu může dojít různých důvodů, třeba když dotaz požaduje příliš mnoho dokumenty bez zadání `$top` nebo zadáním hodnoty pro `$top` , která je příliš velký. V těchto případech bude obsahovat Azure hledání `@odata.nextLink` poznámky v těle odpověď a také `@search.nextPageParameters` Pokud je žádost o příspěvek. Hodnoty tyto poznámky můžete formulace jiné požadavku hledání získat další části hledání odpovědí. Tento postup se nazývá ***pokračování*** původní požadavku hledání a poznámky obecně nazývají ***pokračování tokeny***. Viz [níže uvedeném příkladu](#SearchResponse) informace o syntaxi tyto poznámky a jejich umístění v těle odpovědi. 

Důvody proč Azure hledání může vrátit pokračování tokeny jsou specifické pro implementaci a může se změnit. Robustní klientů buďte vždy chtít zpracovat případech, kdy budou vráceny méně dokumentů než se očekává a token pokračování je součástí pokračovat v načítání dokumenty. Navíc nezapomeňte, že je nutné použít stejnou metodu HTTP jako původní žádost o pokračovat. Například, pokud jste odeslali požadavek GET, všechny žádosti o pokračování odesíláte musíte použít také GET (a podobně platí pro příspěvek).

<a name="SearchResponse"></a>
**Odpověď**

Stavový kód: 200 OK budou vráceny úspěšné odpověď.

    {
      "@odata.count": # (if $count=true was provided in the query),
      "@search.coverage": # (if minimumCoverage was provided in the query),
      "@search.facets": { (if faceting was specified in the query)
        "facet_field": [
          {
            "value": facet_entry_value (for non-range facets),
            "from": facet_entry_value (for range facets),
            "to": facet_entry_value (for range facets),
            "count": number_of_documents
          }
        ],
        ...
      },
      "@search.nextPageParameters": { (request body to fetch the next page of results if not all results could be returned in this response and Search was called with POST)
        "count": ... (value from request body if present),
        "facets": ... (value from request body if present),
        "filter": ... (value from request body if present),
        "highlight": ... (value from request body if present),
        "highlightPreTag": ... (value from request body if present),
        "highlightPostTag": ... (value from request body if present),
        "minimumCoverage": ... (value from request body if present),
        "moreLikeThis": ... (value from request body if present),
        "orderby": ... (value from request body if present),
        "scoringParameters": ... (value from request body if present),
        "scoringProfile": ... (value from request body if present),
        "search": ... (value from request body if present),
        "searchFields": ... (value from request body if present),
        "searchMode": ... (value from request body if present),
        "select": ... (value from request body if present),
        "skip": ... (page size plus value from request body if present),
        "top": ... (value from request body if present minus page size),
      },
      "value": [
        {
          "@search.score": document_score (if a text query was provided),
          "@search.highlights": {
            field_name: [ subset of text, ... ],
            ...
          },
          key_field_name: document_key,
          field_name: field_value (retrievable fields or specified projection),
          ...
        },
        ...
      ],
      "@odata.nextLink": (URL to fetch the next page of results if not all results could be returned in this response; Applies to both GET and POST)
    }

**Příklady:**

Další příklady najdete na stránce [Syntaxi výrazů OData mají být vyhledávány Azure](https://msdn.microsoft.com/library/azure/dn798921.aspx) .

1)  Vyhledávání v indexu seřazené sestupně podle data.


    ZÍSKÁNÍ /indexes/hotels/docs? hledání = * & $orderby = lastRenovationDate desc & verze rozhraní api = 2015 02 28 náhledu

    PŘÍSPĚVEK /indexes/hotels/docs/search? verze rozhraní api = 2015 02 28 náhled {"hledat": "*", "orderby": "lastRenovationDate desc"}

2)  V rámci působí vyhledávání vyhledávání v indexu a načíst fasetami kategorie, hodnocení, značky, i položky s baseRate v určité oblasti:


    ZÍSKÁNÍ /indexes/hotels/docs? hledání = test & podmínka = kategorie & podmínka = hodnocení & podmínka = značky a podmínka = baseRate hodnot: 80 | 150 | 220 & verze rozhraní api = 2015 02 28 náhledu

    PŘÍSPĚVEK /indexes/hotels/docs/search? verze rozhraní api = 2015 02 28 náhled {"hledat": "otestovat", "fasetami": ["kategorie", "hodnocení", "značky", "baseRate hodnot: 80 | 150 | 220"]}

3)  Použití filtru, omezit předchozí výsledky působí dotazu poté, co uživatel klikne na hodnocení 3 a kategorií "Model":


    ZÍSKÁNÍ /indexes/hotels/docs? hledání = test & podmínka = značky a podmínka = baseRate hodnot: 80 | 150 | 220 & $filter = hodnocení eq 3 a kategorie eq "model" & verze rozhraní api = 2015 02 28 náhledu

    PŘÍSPĚVEK /indexes/hotels/docs/search? verze rozhraní api = 2015 02 28 náhled {"hledat": "otestovat", "fasetami": ["značky", "baseRate hodnot: 80 | 150 | 220"], "filtr": "hodnocení eq 3 a kategorie eq 'Model'"}

4) V působí vyhledávání nastavte horní mez jedinečné výrazů v dotazu. Výchozí hodnota je 10, ale můžete zvýšit nebo snížit hodnotu pomocí `count` parametr `facet` atribut:


    ZÍSKÁNÍ /indexes/hotels/docs? hledání = test & podmínka = Město, počet: 5 a verze rozhraní api = 2015 02 28 náhledu

    PŘÍSPĚVEK /indexes/hotels/docs/search? verze rozhraní api = 2015 02 28 náhled {"hledat": "otestovat", "fasetami": ["Město, počet: 5"]}

5)  Vyhledávání v indexu v rámci určitých polí. Například specifické pro určitý jazyk pole:


    ZÍSKÁNÍ /indexes/hotels/docs? hledání = hôtel & searchFields = description_fr & verze rozhraní api = 2015 02 28 náhledu

    PŘÍSPĚVEK /indexes/hotels/docs/search? verze rozhraní api = 2015 02 28 náhled {"hledat": "hôtel", "searchFields": "description_fr"}

6) Vyhledávání v indexu přes více polí. Můžete třeba uložit a dotaz s možností vyhledávání pole ve více jazycích, všechny stejný index.  Jestliže angličtinu a Francouzština popisy existovat ve stejném dokumentu, můžete se vrátit některých nebo všech ve výsledcích dotazu:


    ZÍSKÁNÍ /indexes/hotels/docs? hledání = hotelu & searchFields = popis, description_fr & verze rozhraní api = 2015 02 28 náhledu

    PŘÍSPĚVEK /indexes/hotels/docs/search? verze rozhraní api = 2015 02 28 náhled {"hledat": "hotelu", "searchFields": "popis, description_fr"}

Poznámka: aby dotaz lze použít pouze jeden index najednou. Pokud budete chtít postupně po jednom dotazu nevytvářejte více indexy pro každý jazyk.

7)  Stránkovací - Get 1st stránky položky (velikost stránky je 10):


    ZÍSKÁNÍ /indexes/hotels/docs? hledání = * & $skip = 0 & $top = 10 & verze rozhraní api = 2015 02 28 náhledu

    PŘÍSPĚVEK /indexes/hotels/docs/search? verze rozhraní api = 2015 02 28 náhled {"hledat": "*", "Přeskočit": 0, "nejvyšší": 10}

8)  Stránkovací - Get stránce 2 položek (velikost stránky je 10):


    ZÍSKÁNÍ /indexes/hotels/docs? hledání = * & $skip = 10 & $top = 10 & verze rozhraní api = 2015 02 28 náhledu

    PŘÍSPĚVEK /indexes/hotels/docs/search? verze rozhraní api = 2015 02 28 náhled {"hledat": "*", "Přeskočit": 10, "nejvyšší": 10}

9)  Načtení konkrétní sadu polí:


    ZÍSKÁNÍ /indexes/hotels/docs? hledání = * & $select = hotelName, popis a verze rozhraní api = 2015 02 28 náhledu

    PŘÍSPĚVEK /indexes/hotels/docs/search? verze rozhraní api = 2015 02 28 náhled {"hledat": "*", "select": "hotelName, popis"}

10)  Načtení dokumentů, které vyhovují výrazu konkrétní filtru:


    ZÍSKÁNÍ /indexes/hotels/docs? $filter = (baseRate stránku 60 a baseRate lt 300) nebo hotelName eq "Efektních zůstat" & verze rozhraní api = 2015 02 28 náhledu

    PŘÍSPĚVEK /indexes/hotels/docs/search? verze rozhraní api = 2015 02 28 náhled {"filtr": "(baseRate stránku 60 a baseRate lt 300) nebo hotelName eq 'Efektních pobytu'"}

11) Hledání neúplné index a vrátit se přístupů zvýraznění


    ZÍSKÁNÍ /indexes/hotels/docs? hledání něco = & zvýraznění = Popis & verze rozhraní api = 2015 02 28 náhledu

    PŘÍSPĚVEK /indexes/hotels/docs/search? verze rozhraní api = 2015 02 28 náhled {"hledat": "něco", "zvýraznění": "Popis"}

12) Vyhledávání v indexu a vraťte se dokumenty seřazeny od blíž k dále od odkaz umístění


    ZÍSKÁNÍ /indexes/hotels/docs? hledání něco = & $orderby=geo.distance (umístění, geography'POINT(-122.12315 47.88121) ") & verze rozhraní api = 2015 02 28 náhledu

    PŘÍSPĚVEK /indexes/hotels/docs/search? verze rozhraní api = 2015 02 28 náhled {"hledat": "něco", "orderby": "geo.distance (umístění, geography'POINT(-122.12315 47.88121)") "}

13) Vyhledávání v indexu za předpokladu, že není bodování profil s názvem "geo" funkcí dvou vzdálenost bodování, jeden definování parametr s názvem "currentLocation" a jednu definování parametr s názvem "lastLocation"


    ZÍSKÁNÍ /indexes/hotels/docs? hledání něco = & scoringProfile = geo & scoringParameter = currentLocation – 122.123,44.77233 & scoringParameter = lastLocation – 121.499,44.2113 & verze rozhraní api = 2015 02 28 náhledu

    PŘÍSPĚVEK /indexes/hotels/docs/search? verze rozhraní api = 2015 02 28 náhled {"hledat": "něco", "scoringProfile": "geo", "scoringParameters": ["currentLocation – 122.123,44.77233", "lastLocation – 121.499,44.2113"]}

14) Hledání dokumentů v indexu pomocí [jednoduchého dotazu syntaxe](https://msdn.microsoft.com/library/dn798920.aspx). Tento dotaz vrátí hotely kde vyhledávání pole obsahují podmínek "pohodlí" a "umístění", ale ne "model":


    ZÍSKÁNÍ /indexes/hotels/docs? hledání = pohodlí + umístění-model & searchMode = all & verze rozhraní api = 2015 02 28 náhledu

    PŘÍSPĚVEK /indexes/hotels/docs/search? verze rozhraní api = 2015 02 28 náhled {"hledat": "pohodlí + umístění-model", "searchMode": "všechny"}

Poznámka: použití `searchMode=all` nad. Včetně tento parametr přepíše výchozí `searchMode=any`, zajištění, `-motel` znamená "A ne" místo "Nebo ne". Bez `searchMode=all`dostanete "Nebo není" který rozšíří než omezuje výsledky hledání, a to může být counter-intuitive někteří uživatelé.

15) Hledání dokumentů v indexu pomocí [Syntaxe dotazu lucene](https://msdn.microsoft.com/library/mt589323.aspx). Dotaz vrátí hotely kde pole kategorie obsahuje daný termín "rozpočet." a všechny vyhledávání polí, které obsahují frázi "naposledy renovovanou". Dokumenty, které obsahují frázi "naposledy renovované" řazení vyšší důsledku hodnotu zesílení termínů (3)

    ZÍSKÁNÍ /indexes/hotels/docs?search = kategorie: rozpočet a \"naposledy renovované\"^ 3 a searchMode = all & verze rozhraní api = 2015 02 28 Náhled & querytype = úplné

    PŘÍSPĚVEK /indexes/hotels/docs/search?api-version = 2015 02 28 náhled {"hledat": "kategorie: rozpočtu a \"naposledy renovovanou\"^ 3", "queryType": "celou", "searchMode": "všechny"}

<a name="LookupAPI"></a>
## <a name="lookup-document"></a>Vyhledávání dokumentu

Operace **Vyhledávání dokumentu** načte dokumentu z Azure vyhledávání. To je užitečná, když uživatel klikne na výsledek hledání a chcete-li vyhledat určité informace týkající se tohoto dokumentu.

    GET https://[service name].search.windows.net/indexes/[index name]/docs/[key]?[query parameters]
    api-key: [admin or query key]

**Žádost o**

Protokol HTTPS je potřebný pro žádosti o služby. Žádost o **Vyhledávání dokumentu** můžete vytvořen následujícím způsobem.

    GET /indexes/[index name]/docs/key?[query parameters]

Můžete taky můžete použít tradiční OData syntaxi klíčových vyhledávání:

    GET /indexes('[index name]')/docs('[key]')?[query parameters]

Žádost o URI obsahuje [název indexu] a [klíč] určující dokument, který má načíst z index, který. Můžete získat pouze jeden dokument. Pomocí **vyhledávání** do několika dokumentů v jednom požadavku.

**Parametry dotazu**

`$select=[string]`(volitelné) – seznam polí k načtení s hodnotami oddělenými čárkou. Pokud tento parametr nebo aby `*`, všechna pole označené jako zobrazitelné ve výsledcích vyhledávání ve schématu jsou součástí promítání.

`api-version=[string]`(povinné). Verze preview je `api-version=2015-02-28-Preview`. Podrobnosti a alternativní verze najdete v článku [Správa verzí vyhledávací služby](http://msdn.microsoft.com/library/azure/dn864560.aspx) .

Poznámka: pro tuto operaci `api-version` není zadán jako parametr dotazu.

**Žádost o záhlaví**

Následující seznam popisuje povinných a nepovinných žádost o záhlaví.

- `api-key`: Čím `api-key` slouží k ověření žádost vyhledávací služby. Je hodnotu řetězce jedinečné pro adresy URL služby. Žádost o **Vyhledávání dokumentu** můžete zadat kláves pro správu šipka nebo dotazu pro `api-key`.

Budete taky potřebovat název služby vytvářet požadavku na adresu URL. Získejte název služby a `api-key` z řídicího panelu služby na portálu Azure. V tématu [Vytvoření Azure vyhledávací služby v portálu](search-create-service-portal.md) pro navigaci na stránce Nápověda.

**Žádost o textu**

Žádná.

**Odpověď**

Stavový kód: 200 OK budou vráceny úspěšné odpověď.

    {
      field_name: field_value (fields matching the default or specified projection)
    }

**Příklad**

Vyhledávání dokumentů, která má klávesu "2"

    GET /indexes/hotels/docs/2?api-version=2015-02-28-Preview

Hledání dokumentů, která má klávesu pomocí syntaxe OData 3:

    GET /indexes('hotels')/docs('3')?api-version=2015-02-28-Preview

<a name="CountDocs"></a>
## <a name="count-documents"></a>Počet dokumentů

**Počet dokumentů** operace načte počet dokumentů v indexu vyhledávání. `$count` Syntaxe je součástí protokolu OData.

    GET https://[service name].search.windows.net/indexes/[index name]/docs/$count?api-version=[api-version]
    Accept: text/plain
    api-key: [admin or query key]

**Žádost o**

Protokol HTTPS je potřebný pro žádosti o služby. Zadání **Počtu dokumentů** můžete vytvořen pomocí metody GET.

[Název indexu] v žádosti o URI říká službu vrátí počet všechny položky v kolekci dokumenty zadaný index.

`api-version=[string]`(povinné). Verze preview je `api-version=2015-02-28-Preview`. Podrobnosti a alternativní verze najdete v článku [Správa verzí vyhledávací služby](http://msdn.microsoft.com/library/azure/dn864560.aspx) .

**Žádost o záhlaví**

Následující seznam popisuje povinných a nepovinných žádost o záhlaví.

- `Accept`: Tato hodnota musí být nastavena na `text/plain`.
- `api-key`: Čím `api-key` slouží k ověření žádost vyhledávací služby. Je hodnotu řetězce jedinečné pro adresy URL služby. Zadání **Počtu dokumentů** můžete zadat kláves pro správu šipka nebo dotazu pro `api-key`.

Budete taky potřebovat název služby vytvářet požadavku na adresu URL. Získejte název služby a `api-key` z řídicího panelu služby na portálu Azure. V tématu [Vytvoření Azure vyhledávací služby v portálu](search-create-service-portal.md) pro navigaci na stránce Nápověda.

**Žádost o textu**

Žádná.

**Odpověď**

Stavový kód: 200 OK budou vráceny úspěšné odpověď.

Obsah odpovědí obsahuje hodnoty počet jako celé číslo formátované jako prostý text.

<a name="Suggestions"></a>
## <a name="suggestions"></a>Návrhy

Operace **návrhy** načte návrhy na základě informací částečné vyhledávání. Obvykle se používá v vyhledávacích polí poskytovat automatickým dokončováním návrhy uživatelé zadávají hledané termíny.

Návrh požadavky cílem znázorňující cílových dokumentech, takže navrhované text může opakovat, pokud stejné vyhledávání při zadávání odpovídá více dokumentů candidate. Můžete použít `$select` k načítání dalších polí dokumentu (včetně klíč dokumentu) tak, že můžete zjistit, které dokument je zdroj pro každý návrh.

Operace **návrhy** vystaven jako požadavek GET nebo příspěvek.

    GET https://[service name].search.windows.net/indexes/[index name]/docs/suggest?[query parameters]
    api-key: [admin or query key]

    POST https://[service name].search.windows.net/indexes/[index name]/docs/suggest?api-version=[api-version]
    Content-Type: application/json
    api-key: [admin or query key]

**Kdy použít příspěvek místo GET**

Pokud používáte HTTP GET volání **návrhy** rozhraní API, musíte mějte na paměti, že zkrátit adresu URL žádost o nesmí překročit 8 KB. Je to obvykle dostatečně u většiny aplikací. Některé aplikace však vytvářet obrovské dotazy, konkrétně výrazy filtru OData. Pro tyto aplikace pomocí HTTP POST je lepší volbou, protože umožňuje větší filtry než NAČÍST. Počet klauzulí ve filtru se publikovat, je omezující faktor není velikost řetězec nezpracovanými filtru, protože omezení velikosti příspěvku je přibližně 16 MB.

> [AZURE.NOTE] I když je omezení velikosti příspěvek obrovské, nemůže být libovolně složité výrazy filtru. Další informace o omezeních složitost filtru najdete v článku [syntaxi výrazů OData](https://msdn.microsoft.com/library/dn798921.aspx) .

**Žádost o**

Protokol HTTPS je potřebný pro žádosti o služby. Žádost o **návrhy** lze vytvořit pomocí metody GET nebo příspěvek.

Identifikátor URI požadavku určuje název indexu dotazu. Parametry, například částečně vstupní klíčová slova jsou určeny v řetězci dotazu v případě požadavky GET a v pozvánce na žádosti o textu v případě příspěvek.

Osvědčený při vytváření požadavky GET nezapomeňte [kódovat URL](https://msdn.microsoft.com/library/system.uri.escapedatastring.aspx) parametry specifické dotazu při volání rozhraní REST API přímo. Pro operace **návrhů** jedná se o:

- `$filter`
- `highlightPreTag`
- `highlightPostTag`
- `search`

Adresa URL kódování doporučujeme pouze na výše uvedené parametry dotazu. Pokud se omylem URL kódovat řetězec celý dotazu (všechno po?), žádosti o přeruší.

Navíc kódování adresa URL je nutné pouze při volání získat přímo pomocí rozhraní REST API. Žádné kódování adres URL stačí při volání **návrhy** pomocí příspěvku nebo při použití [knihovny .NET klienta](https://msdn.microsoft.com/library/dn951165.aspx), který pracuje s kódování adres URL pro vás.

**Parametry dotazu**

**Návrhy** přijme, zadejte kritéria dotazu a také určete nastavení hledání několik parametrů. Zadáte, že tyto parametry v adrese URL řetězec dotazu při volání **návrhy** prostřednictvím GET a jako JSON vlastnosti v hlavním textu žádosti o při volání **návrhy** prostřednictvím příspěvek. Syntaxe pro některé parametry je mírně liší GET a publikovat. Tyto rozdíly jsou označeny jako příslušných níže:

`search=[string]`-hledaný text použít k návrhu dotazů. Musí být alespoň na úrovni 1 znak a velikosti maximálně 100 znaků.

`highlightPreTag=[string]`(volitelné) - řetězec značky, které označuje hledání přístupů. Je potřeba nastavit s `highlightPostTag`.

> [AZURE.NOTE] Při volání **návrhy** pomocí GET, vyhrazené znaky v adrese URL musí být v procentech kódovaný (například % 23 místo #).

`highlightPostTag=[string]`(volitelné) - řetězec značky, které připojí k hledání přístupů. Je potřeba nastavit s `highlightPreTag`.

> [AZURE.NOTE] Při volání **návrhy** pomocí GET, vyhrazené znaky v adrese URL musí být v procentech kódovaný (například % 23 místo #).

`suggesterName=[string]`– Název suggester v souladu s `suggesters` kolekce, která je součástí definici indexu. A `suggester` určuje polí, která jsou vyhledávány navrhované výrazy. Další informace najdete v článku [Suggesters](#Suggesters) .

`fuzzy=[boolean]`(volitelné, výchozí = false) – Pokud je nastavena na hodnotu true toto rozhraní API bude hledat návrhy to i v případě znak substituovaných neobsahují žádnou hodnotu do vyhledávacího textového. Když to poskytuje lepší v některých případech je výkon nákladů rozmazaný návrhy hledání jsou pomaleji a používat více zdrojů.

`searchFields=[string]`(volitelné) – seznam jmen hodnotami oddělenými čárkou pole vyhledat zadaný hledaný text. Cílových polí musí být povolený návrhy.

`$top=#`(volitelné, výchozí = 5)-spoustu návrhů načíst. Musí být číslo 1 až 100.

> [AZURE.NOTE] Při volání **návrhy** pomocí příspěvek, tento parametr jmenuje `top` namísto `$top`.

`$filter=[string]`(volitelné) - výraz, který filtruje dokumenty za návrhů.

> [AZURE.NOTE] Při volání **návrhy** pomocí příspěvek, tento parametr jmenuje `filter` namísto `$filter`.

`$orderby=[string]`(volitelné) – seznam oddělený čárkami výrazů řadit výsledky podle. Každý výraz může být název pole nebo volání `geo.distance()` (funkce). Každý výraz můžete následovat `asc` uvést vzestupně, a `desc` označíte sestupně. Výchozí hodnota je vzestupně. Existuje limit pro 32 klauzuli `$orderby`.

> [AZURE.NOTE] Při volání **návrhy** pomocí příspěvek, tento parametr jmenuje `orderby` namísto `$orderby`.

`$select=[string]`(volitelné) – seznam polí k načtení s hodnotami oddělenými čárkou. Pokud tento parametr, je vrácena pouze spolu s návrhy textu dokumentu. Můžete explicitně požádat o všechna pole nastavením tento parametr `*`.

> [AZURE.NOTE] Při volání **návrhy** pomocí příspěvek, tento parametr jmenuje `select` namísto `$select`.

`minimumCoverage`(volitelné, výchozí hodnota je 80)-číslo mezi 0 a 100 procentuální hodnota index, který musí být předmětem návrhy dotazu v pořadí pro dotaz být hlášené jako úspěchu. Ve výchozím nastavení nejméně 80 % indexu musí být k dispozici nebo `Suggest` vrátí HTTP stavový kód 503. Pokud nastavíte `minimumCoverage` a `Suggest` úspěšné, bude vrácená HTTP 200 a zahrnout `@search.coverage` hodnotu na základě procentuální hodnota index, který jste obdrželi v dotazu.

> [AZURE.NOTE] Nastavení tento parametr hodnotou nižší než 100 může být užitečné pro zajištění dostupnosti hledání i pro služby s jedinou otevřené. Ale ne všech odpovídající návrhy jsou zaručena prezentovat ve výsledcích. Pokud odvolání zprávy proběhne aplikaci důležitější než dostupnosti a pak je nejlepší Ztlumením `minimumCoverage` pod použita výchozí hodnota 80.

`api-version=[string]`(povinné). Verze preview je `api-version=2015-02-28-Preview`. Podrobnosti a alternativní verze najdete v článku [Správa verzí vyhledávací služby](http://msdn.microsoft.com/library/azure/dn864560.aspx) .

Poznámka: pro tuto operaci `api-version` není zadán jako parametr dotazu v adrese URL bez ohledu na to, zda zavoláte **návrhy** s GET nebo příspěvek.

**Žádost o záhlaví**

Následující seznam popisuje záhlaví povinných a nepovinných požadavků

- `api-key`: Čím `api-key` slouží k ověření žádost vyhledávací služby. Je hodnotu řetězce jedinečné pro adresy URL služby. Žádost o **návrhů** můžete zadat kláves pro správu šipka nebo dotazu jako `api-key`.

Budete taky potřebovat název služby vytvářet požadavku na adresu URL. Získejte název služby a `api-key` z řídicího panelu služby na portálu Azure. V tématu [Vytvoření Azure vyhledávací služby v portálu](search-create-service-portal.md) pro navigaci na stránce Nápověda.

**Žádost o textu**

Pro získání: žádný.

Příspěvku:

    {
      "filter": "odata_filter_expression",
      "fuzzy": true | false (default),
      "highlightPreTag": "pre_tag",
      "highlightPostTag": "post_tag",
      "minimumCoverage": # (% of index that must be covered to declare query successful; default 80),
      "orderby": "orderby_expression",
      "search": "partial_search_input",
      "searchFields": "field_name_1, field_name_2, ...",
      "select": "field_name_1, field_name_2, ...",
      "suggesterName": "suggester_name",
      "top": # (default 5)
    }

**Odpověď**

Stavový kód: 200 OK budou vráceny úspěšné odpověď.

    {
      "@search.coverage": # (if minimumCoverage was provided in the query),
      "value": [
        {
          "@search.text": "...",
          "<key field>": "..."
        },
        ...
      ]
    }

Pokud možnost projekce slouží k načtení pole, která jsou součástí každý prvek pole:

    {
      "@search.coverage": # (if minimumCoverage was provided in the query),
      "value": [
        {
          "@search.text": "...",
          "<key field>": "..."
          <other projected data fields>
        },
        ...
      ]
    }

**Příklad**

Načtení 5 návrhy kde vstupní částečné vyhledávání je "lux"

    GET /indexes/hotels/docs/suggest?search=lux&$top=5&suggesterName=sg&api-version=2015-02-28-Preview

    POST /indexes/hotels/docs/suggest?api-version=2015-02-28-Preview
    {
      "search": "lux",
      "top": 5,
      "suggesterName": "sg"
    }
