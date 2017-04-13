<properties
    pageTitle="Hodnocení profily (Azure hledání ZBÝVAJÍCÍ verze rozhraní API 2015-02-28 – verze Preview) | Microsoft Azure | Náhled Azure hledání rozhraní API"
    description="Azure hledání se hostovanou cloudu vyhledávací služba, která podporuje ladění seřazené výsledky podle uživatelem definovaných bodování profily."
    services="search"
    documentationCenter=""
    authors="HeidiSteen"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="search"
    ms.devlang="rest-api"
    ms.workload="search"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.author="heidist"
    ms.date="08/29/2016" />

# <a name="scoring-profiles-azure-search-rest-api-version-2015-02-28-preview"></a>Hodnocení profily (Azure hledání ZBÝVAJÍCÍ verze rozhraní API 2015-02-28 – verze Preview)

> [AZURE.NOTE] Tento článek popisuje bodování profilů v [2015 02 28 náhled](search-api-2015-02-28-preview.md). Momentálně není žádný rozdíl mezi `2015-02-28` verze uvedených na [webu MSDN](http://msdn.microsoft.com/library/azure/mt183328.aspx) a `2015-02-28-Preview` verze popsané, ale tento dokument nabízíme přesto k pokrytí dokumentu přes celou rozhraní API.

## <a name="overview"></a>Základní informace

Hodnocení se vztahuje k výpočtu hledání skóre pro každé položky vrácené ve výsledcích hledání. Skóre je indikátorem důležité položky v kontextu aktuálního operace vyhledávání. Vyšší skóre, více příslušnou položku. Ve výsledcích hledání položky jsou pořadí objednali od nejvyššího k nízká; podle hledání skóre výpočtu pro každou položku.

Azure hledání používá výchozí bodování pro výpočet počáteční skóre, ale je možné upravit výpočtu pomocí bodování profilu. Hodnocení profily umožňují získat větší kontrolu nad řazení položek ve výsledcích hledání. Můžete třeba pomoct položek podle jejich potenciální výnosy, zvýšit novější položky nebo možná pomoct položky, které byly v zásob příliš dlouho.

Profil bodování je součástí definice indexu skládající se ze polí, funkce a parametry.

Abyste měli přehled o vypadá bodování profilu, následující příklad ukazuje jednoduchý profil s názvem "geo". Tuto položku zvyšuje položky, které mají hledaný výraz v `hotelName` pole. Taky používá `distance` funkce kompresi položky, které jsou v rámci deseti km aktuálního umístění. Pokud někdo hledá termín "DIČ" a "DIČ" se stane s být součástí Název hotelu, zobrazí se dokumenty, které obsahují hotely s "DIČ" vyšší ve výsledcích hledání.

    "scoringProfiles": [
      {
        "name": "geo",
        "text": {
          "weights": { "hotelName": 5 }
        },
        "functions": [
          {
            "type": "distance",
            "boost": 5,
            "fieldName": "location",
            "interpolation": "logarithmic",
            "distance": {
              "referencePointParameter": "currentLocation",
              "boostingDistance": 10
            }
          }
        ]
      }
    ]

Použít bodování profil, je připravený dotazu tak, aby určení profilu v řetězci dotazu. Do dotazu Všimněte si parametr dotazu `scoringProfile=geo` v pozvánce.

    GET /indexes/hotels/docs?search=inn&scoringProfile=geo&scoringParameter=currentLocation--122.123,44.77233&api-version=2015-02-28-Preview

Tento dotaz hledá termín "DIČ" a předá v aktuálním umístění. Všimněte si, že tento dotaz zahrnuje další parametry jako `scoringParameter`. Parametry dotazu jsou popsané v [Dokumentech Search (hledání Azure rozhraní API)](search-api-2015-02-28-preview.md#SearchDocs).

Klikněte na [Příklad](#example) Chcete-li zobrazit více podrobných příkladů bodování profilu.

## <a name="what-is-default-scoring"></a>Co je výchozí bodování?

Hodnocení vypočítá hledání skóre pro každou položku v sadě rank uspořádaných výsledků. Jednotlivé položky v sadě výsledků hledání přiřazené výsledek hledání a pak zařazených jako nejvyšší po nejnižší. Položky s vyšší výsledky jsou vráceny do aplikace. Ve výchozím nastavení se vracejí horní 50, ale můžete použít `$top` parametr vrátíte menší nebo větší počet položek (až 1000 jedinou odpověď).

Ve výchozím nastavení se vypočítává skóre vyhledávání podle statistické vlastnosti dat a dotaz. Azure hledání najde dokumenty, které obsahují hledaných slov v řetězci dotazu (některých nebo všech, v závislosti na `searchMode`), určování preferencí dokumenty, které obsahují mnoho výskyty hledaného výrazu. Výsledek hledání, půjde ještě vyšší-li termín méně častých přes souhrnu dat, ale běžné v dokumentu. Základem tento přístup do výpočetního relevance jmenoval TF IDF nebo (termínů inverzní k distribuční funkci počet_plateb dokumentu frekvence).

Za předpokladu, že je žádná vlastní řazení, pak řazení výsledků podle hledání skóre předtím, než se vracejí volající aplikaci. Pokud `$top` není zadán 50 položek, které mají nejvyšší vyhledávání vrátí výsledek.

Hledání skóre hodnoty můžete opakovat v sadě výsledků. Například, může mít 10 položek se skóre 1.2 20 položky se skóre 1.0 a 20 položky se skóre čísla 0,5. Když několik přístupů mít stejný výsledek hledání, objednávání položky stejným dosáhne není definované a není stabilní. Spustit dotaz znova a může se zobrazit položky pozici shift. Možnost dvě položky s identickými skóre, není nijak zaručené, který z nich je první.

## <a name="when-to-use-custom-scoring"></a>Kdy použít vlastní bodování

Po výchozí chování řazení není úplně dost v schůzky cílů firmy je třeba vytvořit jeden nebo více bodování profily. Například rozhodnout, že by měl hledání relevance kompresi nově přidané položky. Podobně platí bude pravděpodobně nutné pole obsahující hrubá sazba nebo jiné pole určující, potenciální výnosy. Zvýšení přístupů, které přenést výhod pro vaše podnikání může být faktor důležitých při rozhodování o použití bodování profilů.

Na základě důležitosti řazení je také prostřednictvím bodování profily implementovaná. Zvažte stránkách výsledků hledání, které jste použili v minulosti, které umožňují řadit podle cena, datum, hodnocení nebo relevance. Do pole hledání Azure jednotka bodování profily možnost "relevance". Definice relevance řídí se zabezpečuje pomocí vhodné obchodních cílů a typ rozhraní pro hledání, které chcete předvádění.

<a name="example"></a>
## <a name="example"></a>Příklad

Jak je uvedeno, vlastní bodování prováděna prostřednictvím bodování profily definované do schématu index.

Tento příklad ukazuje schématu index s dva bodování profily (`boostGenre`, `newAndHighlyRated`). Dotazy týkající se tento index, který obsahuje buď profil jako k dotazu parametr bude používat profil skóre sadu výsledků.

[Zkuste tento příklad](search-get-started-scoring-profiles.md).

    {
      "name": "musicstoreindex",
      "fields": [
        { "name": "key", "type": "Edm.String", "key": true },
        { "name": "albumTitle", "type": "Edm.String" },
        { "name": "albumUrl", "type": "Edm.String", "filterable": false },
        { "name": "genre", "type": "Edm.String" },
        { "name": "genreDescription", "type": "Edm.String", "filterable": false },
        { "name": "artistName", "type": "Edm.String" },
        { "name": "orderableOnline", "type": "Edm.Boolean" },
        { "name": "rating", "type": "Edm.Int32" },
        { "name": "tags", "type": "Collection(Edm.String)" },
        { "name": "price", "type": "Edm.Double", "filterable": false },
        { "name": "margin", "type": "Edm.Int32", "retrievable": false },
        { "name": "inventory", "type": "Edm.Int32" },
        { "name": "lastUpdated", "type": "Edm.DateTimeOffset" }
      ],
      "scoringProfiles": [
        {
          "name": "boostGenre",
          "text": {
            "weights": {
              "albumTitle": 1.5,
              "genre": 5,
              "artistName": 2
            }
          }
        },
        {
          "name": "newAndHighlyRated",
          "functions": [
            {
              "type": "freshness",
              "fieldName": "lastUpdated",
              "boost": 10,
              "interpolation": "quadratic",
              "freshness": {
                "boostingDuration": "P365D"
              }
            },
            {
              "type": "magnitude",
              "fieldName": "rating",
              "boost": 10,
              "interpolation": "linear",
              "magnitude": {
                "boostingRangeStart": 1,
                "boostingRangeEnd": 5,
                "constantBoostBeyondRange": false
              }
            }
          ]
        }
      ],
      "suggesters": [
        {
          "name": "sg",
          "searchMode": "analyzingInfixMatching",
          "sourceFields": ["albumTitle", "artistName"]
        }
      ]
    }


## <a name="workflow"></a>Pracovní postup

Implementovat vlastní bodování chování, přidejte do schématu, který definuje index bodování profil. Máte několik bodování profilů v rejstříku, ale můžete zadat pouze jeden profil v době v zadaném dotazu.

Použití [šablony](#bkmk_template) uvedených v tomto tématu.

Zadejte název. Bodování profily jsou volitelné, ale pokud chcete přidat jednu, název je povinný. Dodržujte konvence pro pole (začíná písmenem, vyhnete zvláštních znaků a rezervovaná slova). Další informace najdete v tématu [Pravidla pojmenování](http://msdn.microsoft.com/library/azure/dn857353.aspx) .

Část bodování profil je vytvořen z váženého polí a funkce.

### <a name="weights"></a>Hmotnost ###

`weights` Vlastnost bodování profilu určuje párů název hodnota, která pole přiřadit relativní důležitost. V [příkladu](#example)albumTitle žánr a artistName pole jsou zesílen 1,5 5 a 2, respektive. Proč je žánr zesílen tolik vyšší než ostatní? Pokud se nad daty, která je poněkud homogenní provádí vyhledávání (stejně jako v případě s "žánr" v `musicstoreindex`), může být nutné větší odchylka v relativní tloušťky. Například v `musicstoreindex`, "rock" zobrazí jako obou žánr a v popisech žánr stejně obsahuje jiné spojení. Pokud chcete ke převažují nad žánr popis, bude nutné pole žánr mnohem vyšší relativní důležitost.

### <a name="functions"></a>Funkce ###

Použití funkcí když jsou potřeba pro konkrétní kontexty další výpočty. Jsou platné funkce typy `freshness`, `magnitude`, `distance` a `tag`. Každá funkce má parametrů, které jsou jedinečné pro ho.

  - `freshness`Pokud chcete pomoct ve způsobu, jakým nových a starých položka je. Tuto funkci lze použít pouze u polí data a času (`Edm.DataTimeOffset`). Poznámka `boostingDuration` atribut se používá pouze funkce aktuálnost.
  - `magnitude`bude použito při Chcete-li zvýšit na základě jak vysoké nebo nízké číselnou hodnotu. Scénáře, které volají pro tuto funkci zahrnout zvýšení hrubá sazba, nejvyšší cena, nejnižší ceny nebo počtu stahování. Oblasti nejvyššího k nejnižším, můžete vrátit, pokud chcete, aby inverzní vzorek (třeba položky zesílení nižší cenou víc než vyšší cenou položky). Věnovat oblasti ceny z $100 $1, byste měli nastavit `boostingRangeStart` 100 a `boostingRangeEnd` od 1 do pomoct nižší cenou položky. Tuto funkci lze použít pouze s poli poklepejte a celé číslo.
  - `distance`bude použito když budete chtít pomoct blízkost nebo zeměpisné polohy. Tuto funkci lze použít pouze s `Edm.GeographyPoint` pole.
  - `tag`bude použito když budete chtít pomoct podle klíčových slov společné dokumenty a vyhledávacích dotazů. Tuto funkci lze použít pouze s `Edm.String` a `Collection(Edm.String)` pole.
  
#### <a name="rules-for-using-functions"></a>Pravidla pro používání funkce ####

  - Funkce typ (aktuálnost, velikost, vzdálenost, značka) musí být samá velká písmena.
  - Funkce nesmí obsahovat hodnotu null nebo prázdná. Konkrétně Jestliže zahrnete název pole, musíte nastavit na jinou hodnotu.
  - Funkce lze použít pouze pro filtrovatelné pole. Další informace o filtrovatelné polí naleznete v tématu [Vytvoření indexu](search-api-2015-02-28-preview.md#CreateIndex) .
  - Funkce lze použít pouze na pole, které jsou definované v kolekci polí indexu.

Po definování index vytvoříte rejstřík tak, že odesílání index schématu sledované dokumenty. Pokyny pro tyto operace naleznete v tématu [Create Index](search-api-2015-02-28-preview.md#CreateIndex) a [Přidat nebo aktualizovat dokumenty](search-api-2015-02-28-preview.md#AddOrUpdateDocuments) . Vytvořenou index byste si měli funkční bodování profil, který pracuje s daty vyhledávání.

<a name="bkmk_template"></a>
## <a name="template"></a>Šablony
Tento oddíl vysvětluje syntaxe a šablon hodnocení profily. Podívejte se do [indexu atribut odkaz](#bkmk_indexref) v následující části Popis atributy.

    ...
    "scoringProfiles": [
      {
        "name": "name of scoring profile",
        "text": (optional, only applies to searchable fields) {
          "weights": {
            "searchable_field_name": relative_weight_value (positive #'s),
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
            }

            // (- or -)

            "freshness": {
              "boostingDuration": "..." (value representing timespan over which boosting occurs)
            }

            // (- or -)

            "distance": {
              "referencePointParameter": "...", (parameter to be passed in queries to use as reference location)
              "boostingDistance": # (the distance in kilometers from the reference location where the boosting range ends)
            }

            // (- or -)

            "tag": {
              "tagsParameter": "..." (parameter to be passed in queries to specify list of tags to compare against target field)
            }
          }
        ],
        "functionAggregation": (optional, applies only when functions are specified)
            "sum (default) | average | minimum | maximum | firstMatching"
      }
    ],
    "defaultScoringProfile": (optional) "...",
    ...

<a name="bkmk_indexref"></a>
## <a name="scoring-profile-property-reference"></a>Hodnocení odkaz vlastnost profilu

**Poznámka:** Hodnocení funkci lze použít pouze na pole, které jsou filtrovatelné.

| Vlastnost | Popis |
|----------|-------------|
| `name`   | Povinné. Toto je název bodování profilu. Následuje stejné konvence pole. Musí začínat písmenem, nesmí obsahovat tečky, dvojteček nebo @ symboly a nesmí začínat věty "azureSearch" (malá a velká písmena). |
| `text` | Obsahuje vlastnost gramáží. |
| `weights` | Volitelné. Pár název hodnota, která určuje název pole a relativní důležitost. Relativní důležitost musí být kladné celé číslo nebo číslo s plovoucí desetinnou čárkou. Můžete zadat název pole bez odpovídající tloušťka čáry. Gramáží slouží k označení důležitost jedno pole relativní jiného. |
| `functions` | Volitelné. Všimněte si, že bodování funkce se dají použít pouze pro pole, které jsou filtrovatelné. |
| `type` | Požadovaný pro bodování funkcí. Označuje typ funkce používat. Platné hodnoty jsou `magnitude`, `freshness`, `distance` a `tag`. V každém bodování profilu můžete zahrnout více než jednu funkci. Název funkce musí být samá velká písmena. |
| `boost` | Požadovaný pro bodování funkcí. Kladné číslo jako násobek určené hrubé skóre. Nesmí být roven 1. |
| `fieldName` | Požadovaný pro bodování funkcí. Hodnocení funkci lze použít pouze pro pole, které jsou součástí kolekci polí indexu, které jsou filtrovatelné. Kromě toho jednotlivých typů funkce uvádí dodatečnými omezeními (aktuálnost je použit u polí data a času, velikosti celé číslo nebo dvojité polí, vzdálenost s poli umístění a značkou řetězec nebo řetězec kolekci polí). Můžete zadat pouze v jednom poli za definice funkce. Například můžete velikost dvakrát po stejný jako profil by potřebujete zahrnout dva definice velikosti, jedno pro každé pole. |
| `interpolation` | Požadovaný pro bodování funkcí. Definuje sklon jehož skóre zvýšení zvýšení od začátku oblast na konec oblasti. Platné hodnoty jsou `linear` (výchozí), `constant`, `quadratic`, a `logarithmic`. Další informace najdete v článku [Nastavení interpolations](#bkmk_interpolation) . |
| `magnitude` | Velikost bodování funkce se používá ke změně pořadí podle rozsahu hodnot pro číselné pole. Některé nejčastější příklady použití tohoto jsou:<ul><li>Hodnocení hvězdičkami: Změnit bodování na základě hodnoty v poli "Hvězda hodnocení". Jakmile dvě položky jsou důležité, zobrazí se nejprve položky s vyšší hodnocení.</li><li>Okraje: Při relevantní dvou dokumentů v maloobchodě chtít pomoct dokumenty, které mají vyšší okraje nejdřív.</li><li>Klikněte na počty: prostřednictvím akce produktů a stránek, klikněte na aplikace, které sledovat, můžete použít velikosti zesílení položek, které mají za následek získat nejvíce přenosy.</li><li>Stáhněte si počty: For applications, sledování stahování umožňuje funkce velikost zvýšíte položky, které se dají stáhnout nejčastěji.</li></ul> |
| `magnitude:boostingRangeStart` | Nastaví počáteční hodnotu oblasti, které velikosti dosáhne. Hodnota musí být celé číslo nebo číslo s plovoucí desetinnou čárkou. Hodnocení hvězdičkami 1 až 4 by to bylo 1. Okraje víc než 50 % by to bylo 50. |
| `magnitude:boostingRangeEnd` | Nastaví hodnotu na konci oblasti, které velikosti dosáhne. Hodnota musí být celé číslo nebo číslo s plovoucí desetinnou čárkou. Hodnocení hvězdičkami 1 až 4 by to bylo 4. |
| `magnitude:constantBoostBeyondRange` | Jsou platné hodnoty true nebo false (výchozí). Pokud je nastavena na hodnotu true, úplné zesílení bude používat k dokumentům, které obsahují hodnotu pro cíl pole, které je vyšší než horní konec oblasti. Pokud je hodnota NEPRAVDA, zesílení tato funkce se nepoužijí k dokumentům máte hodnotu pro pole Cíl spadající mimo rozsah. |
| `freshness` | Aktuálnost bodování funkce se používá ke změně skóre řazení položek na základě hodnot v polích DateTimeOffset. Například položky s více nejpozdější datum mohou být zařazených jako vyšší než starších položek. (Vezměte v úvahu, že je také možné pořadí položek jako událostí kalendáře pomocí budoucí data tak, aby se blíže položek do prezentace můžete hodnotit vyšší než položky dále v budoucnu.) V současné verzi služby jeden konec oblasti opravíme aktuální čas. Druhou stranu je čas v minulosti na základě `boostingDuration`. Pokud chcete pomoct velké množství času v budoucnosti použijte zápornou `boostingDuration`. Rychlost zvýšení změní z maximální a minimální oblast záleží interpolací použité u bodování profilu (viz následující obrázek). Chcete-li převrátit zvýšení koeficient použitý, zvolte faktoru zesílení menší než 1. |
| `freshness:boostingDuration` | Sady dobu platnosti, po které zvýšení přestane v určitém dokumentu. Přečtěte si téma [sadu boostingDuration] [#bkmk_boostdur] v následující části syntaxi a příklady. |
| `distance` | Zavřete požadovanou vzdálenost bodování funkce se používá ovlivňovat skóre dokumenty založené na způsob nebo úplně jsou relativní odkaz zeměpisné polohy. Umístění odkazu je uveden jako součást dotazu v parametru (pomocí `scoringParameter` dotazu parametr) jako fyzický pevný disk, lat argumentů. |
| `distance:referencePointParameter` | Parametr předávat v dotazech použít jako odkaz umístění. scoringParameter je parametr dotazu. Popisy parametry dotazu najdete v článku [Hledání dokumentů](search-api-2015-02-28-preview.md#SearchDocs) . |
| `distance:boostingDistance` | Číslo, které udává vzdálenost v km z umístění odkazu ukončení zvýšení oblast. |
| `tag` | Značka bodování funkce se používá ovlivňovat skóre dokumenty založené na značky v dokumentech a vyhledávacích dotazů. Dokumenty, které obsahují značky společně s vyhledávací dotaz bude zesílen. Klíčová slova pro hledání dotazu je k dispozici jako parametr bodování v každém požadavku hledání (pomocí `scoringParameter` dotazu parametr). |
| `tag:tagsParameter` | Parametr předávat v dotazech můžete určit, značky pro konkrétní žádost. `scoringParameter`je parametr dotazu. Popisy parametry dotazu najdete v článku [Hledání dokumentů](search-api-2015-02-28-preview.md#SearchDocs) . |
| `functionAggregation` | Volitelné. Platí jenom při jsou uvedeny funkce. Platné hodnoty jsou: `sum` (výchozí), `average`, `minimum`, `maximum`, a `firstMatching`. Hledání skóre je jedna hodnota je vypočítána z více proměnných, včetně víc funkcí. Tento atributy Určuje, jak zvyšuje všechny funkce jsou sloučena do jednoho agregační zesílení, pak přiřazené skóre základní dokument. Základní skóre je založena na hodnotu tf idf vypočítanou z dokumentu a vyhledávacího dotazu. |
| `defaultScoringProfile` | Při provádění požadavku hledání, pokud není zadán žádný bodování profil, pak výchozí bodování je použité (tf-idf pouze). Výchozí bodování název profilu můžete nastavit, způsobují Azure vyhledávání a pokusí se provádějte žádný konkrétní profil se udává v požadavku hledání profilu. |

<a name="bkmk_interpolation"></a>
## <a name="set-interpolations"></a>Nastavení interpolations

Interpolations umožňují definovat sklon jehož skóre zvýšení zvýšení od začátku oblast na konec oblasti. Můžete použít následující interpolations:

  - `Linear`: U položek, které jsou v rozsahu maximální a minimální hodnota zesílení použité na položku provede neustále poklesu přiblížit. Lineární je výchozí interpolací bodování profilu.
  - `Constant`: U položek, které jsou v rámci počáteční a koncovou oblast konstantní zesílení použije se řazení výsledků.
  - `Quadratic`: Ve srovnání s lineární interpolací obsahující neustále poklesu zesílení kvadratického sníží původně menší tempem a pak když se blíží konec oblasti, se zmenší podstatně vyšší intervalech. Tato možnost interpolací není povolena u značky bodování funkcí.
  - `Logarithmic`: Ve srovnání s lineární interpolací obsahující neustále poklesu zesílení logaritmické sníží původně vyšší tempem a potom když se blíží konec oblasti, ho zmenší na množství menší interval. Tato možnost interpolací není povolena u značky bodování funkcí.

<a name="Figure1"></a>
 ![][1]

<a name="bkmk_boostdur"></a>
## <a name="set-boostingduration"></a>Nastavení boostingDuration

`boostingDuration`představuje atribut funkce aktuálnost. Slouží k nastavení vypršení období, po které zvýšení přestanou se vám v určitém dokumentu. Například pomoct produktu čáry nebo značku propagační dobu 10 dnů, zadáte 10 den dobu trvání jako "P10D" tyto dokumenty. Nebo pokud chcete pomoct nadcházející události dalšího týdne zadejte "-P7D".

`boostingDuration`musí být formátováno jako hodnotu "dayTimeDuration" XSD (s omezeným přístupem podmnožinu hodnotu doby trvání formátu ISO 8601). Vzor pro tuto: `[-]P[nD][T[nH][nM][nS]]`.

Následující tabulka obsahuje několik příkladů.

| Doba trvání | boostingDuration |
|----------|------------------|
| 1 den | "P1D" |
| 2 dnů a 12 hodin | "P2DT12H" |
| 15 minut | "PT15M" |
| 30 dní, 5 hodin, 10 minut, a 6.334 sekund | "P30DT5H10M6.334S" |

Další příklady naleznete v tématu [schématu XML: datové typy (adrese W3.org webu)](http://www.w3.org/TR/xmlschema11-2/).

**Viz taky**
[rozhraní REST API Azure vyhledávací služby](http://msdn.microsoft.com/library/azure/dn798935.aspx) na webu MSDN <br/>
[Vytvoření indexu (Azure hledání rozhraní API)](http://msdn.microsoft.com/library/azure/dn798941.aspx) na webu MSDN<br/>
[Přidání hodnocení profilu do indexu vyhledávání](http://msdn.microsoft.com/library/azure/dn798928.aspx) na webu MSDN<br/>

<!--Image references-->
[1]: ./media/search-api-scoring-profiles-2015-02-28-Preview/scoring_interpolations.png
