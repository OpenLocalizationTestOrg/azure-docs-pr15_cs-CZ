<properties 
    pageTitle="Přehled analýzy v aplikaci přehledy | Microsoft Azure" 
    description="Odkaz pro příkazy v analýzy, nástroj výkonné vyhledávání aplikace přehledy. " 
    services="application-insights" 
    documentationCenter=""
    authors="alancameronwills" 
    manager="douge"/>

<tags 
    ms.service="application-insights" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="ibiza" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/27/2016" 
    ms.author="awills"/>

# <a name="reference-for-analytics"></a>Odkaz pro analýzy

[Technologie pro analýzu](app-insights-analytics.md) je funkce výkonné vyhledávání [Aplikace přehledy](app-insights-overview.md). Tyto stránky popisují dotazovací jazyk analýzy.

> [AZURE.NOTE] [Vyzkoušení analýzy na naše simulovaný data](https://analytics.applicationinsights.io/demo) při aplikace není odesílání dat pro přehledy aplikaci ještě.

## <a name="index"></a>Index


**Povolení** [Povolení](#let-clause)


**Dotazy a operátory** [počet](#count-operator)  |  [vyhodnocení](#evaluate-operator) | [rozšíření](#extend-operator) | [spojení](#join-operator) | [limit](#limit-operator) | [mvexpand](#mvexpand-operator) | [analyzovat](#parse-operator) | [projektu](#project-operator) | [projektu zase](#project-away-operator) | [oblast](#range-operator) | [snížit](#reduce-operator) | [vykreslování směrnice](#render-directive) | [omezit klauzule](#restrict-clause) | [řazení](#sort-operator) | [sumarizovat](#summarize-operator) | [trvat](#take-operator) | [horní](#top-operator) | [vnořené horní](#top-nested-operator) | [unie](#union-operator) | [místo, kam](#where-operator) | [where v](#where-in-operator)

**Agregace** [všechny](#any)  |  [argmax](#argmax) | [argmin](#argmin) | [avg](#avg) | [buildschema](#buildschema) | [počet](#count) | [countif](#countif) | [DPOČET](#dcount) | [dcountif](#dcountif) | [makelist](#makelist) | [makeset](#makeset) | [max](#max) | [min](#min) | [percentilu](#percentile) | [percentily](#percentiles) | [percentilesw](#percentilesw) | [percentilew](#percentilew) | [SMODCH.výběr](#stdev) | [Součet](#sum) | [Odchylka](#variance)

**Skalárů** [Logická literálů](#boolean-literals)  |  [Logické operátory](#boolean-operators) | [nádechy](#casts) | [Skalární porovnání](#scalar-comparisons) | [gettype](#gettype) | [hash](#hash) | [iff](#iff) | [isnotnull](#isnotnull) | [isnull](#isnull) | [notnull](#notnull) | [toscalar](#toscalar)

**Čísla** [Aritmetické operátory](#arithmetic-operators)  |  [Číselné literály](#numeric-literals) | [abs](#abs) | [Koš](#bin) | [exp](#exp) | [prostorového uspořádání](#floor) | [gama](#gamma) | [protokolu](#log) | [NÁHČÍSLO](#rand) | [odmocnina](#sqrt) | [todouble](#todouble) | [toint](#toint) | [tolong](#tolong)

**Datum a čas** [Datum a čas výrazy](#date-and-time-expressions)  |  [Datum a čas literály](#date-and-time-literals) | [před](#ago) | [datepart](#datepart) | [dayofmonth](#dayofmonth) | [dayofweek](#dayofweek) | [dayofyear](#dayofyear) | [endofday](#endofday) | [endofmonth](#endofmonth) | [endofweek](#endofweek) | [endofyear](#endofyear) | [getmonth](#getmonth) | [getyear](#getyear) | [teď](#now) | [startofday](#startofday) | [startofmonth](#startofmonth) | [startofweek](#startofweek) | [startofyear](#startofyear) | [todatetime](#todatetime) | [totimespan](#totimespan) | [weekofyear](#weekofyear)

**Řetězec** [GUID](#guids)  |  [Zakódovány řetězcových](#obfuscated-string-literals) | [Řetězcových](#string-literals) | [porovnávání řetězců](#string-comparisons) | [countof](#countof) | [extrahovat](#extract) | [isempty](#isempty) | [isnotempty](#isnotempty) | [notempty](#notempty)| [parseurl](#parseurl) | [Nahradit](#replace) | [rozdělení](#split) | [strcat](#strcat) | [strlen](#strlen) | [podřetězec](#substring) | [tolower](#tolower) | [toupper](#toupper)

**Matice, objekty a dynamické** [Matice a objekt literálů](#array-and-object-literals)  |  [Funkce dynamické objektu](#dynamic-object-functions) | [dynamických objektů v upozorní klauzule](#dynamic-objects-in-let-clauses) | [výrazech JSON Path](#json-path-expressions) | [názvy](#names) | [arraylength](#arraylength) | [extractjson](#extractjson) | [parsejson](#parsejson) | [oblast](#range) | [todynamic](#todynamic) | [treepath](#treepath)



## <a name="let"></a>Povolení

### <a name="let-clause"></a>povolení klauzule

**Tabulkové nechat - pojmenování tabulky**

    let recentReqs = requests | where timestamp > ago(3d); 
    recentReqs | count

**Skalární nechat - pojmenování hodnotu**

    let interval = 3d; 
    requests | where timestamp > ago(interval)

**Lambda nechat - pojmenování funkce**

    let Recent = 
       (interval:timespan) { requests | where timestamp > ago(interval) };
    Recent(3h) | count

    let us_date = (t:datetime) { strcat(getmonth(t),'/',dayofmonth(t),'/',getyear(t)) }; 
    requests | summarize count() by bin(timestamp, 1d) | project count_, day=us_date(timestamp)

Upozorní klauzule vytvoří vazbu [název](#names) tabulky výsledek, skalární hodnotu a funkce. Klauzule je předpony dotazu a je oborem vazby tohoto dotazu. (Povolit nenabízí možnost, jak s tím, co jméno, které používáte dál v relaci.)

**Syntaxe**

    let name = scalar_constant_expression ; query

    let name = query ; query

    let name = (parameterName : type [, ...]) { plain_query }; query

    let name = (parameterName : type [, ...]) { scalar_expression }; query

* *Typ:* `bool`, `int`, `long`, `double`, `string`, `timespan`, `datetime`, `guid`,[`dynamic`](#dynamic-type)
* *plain_query:* Dotaz není předponou klauzuli povolit.

**Příklady**

    let rows = (n:long) { range steps from 1 to n step 1 };
    rows(10) | ...


Vlastní spojení:

    let Recent = events | where timestamp > ago(7d);
    Recent | where name contains "session_started" 
  	| project start = timestamp, session_id
  	| join (Recent 
        | where name contains "session_ended" 
        | project stop = timestamp, session_id)
      on session_id
  	| extend duration = stop - start 


## <a name="queries-and-operators"></a>Dotazy a operátory

Dotaz na vaše telemetrie je tvořen odkaz na zdroj toku, následovaný kanálu filtry. Příklad:


```AIQL
requests // The request table starts this pipeline.
| where client_City == "London" // filter the records
   and timestamp > ago(3d)
| count 
```
    
Jednotlivé filtry předchází znakem `|` instancí *operátor*, některé parametry. Vstupní s operátorem je tabulky, která je výsledkem předchozího kanálu. Ve většině případů všechny parametry překračují sloupce vstupní [Skalární výrazů](#scalars) . V určitých případech jsou názvy sloupců, vstupní parametry a v určitých případech je parametrem druhé tabulky. Výsledek dotazu je vždy tabulky, i když pouze jeden sloupec a jeden řádek.

Dotazy může obsahovat konce jeden řádek, ale ukončí prázdný řádek. Mohou obsahovat komentáře mezi `//` a konec řádku.

Dotazu může být předchází jeden nebo více [nechat věty](#let-clause), které definují skalárů tabulkami a funkce, které lze použít v dotazu.

```AIQL

    let interval = 3d ;
    let city = "London" ;
    let req = (city:string) {
      requests
      | where client_City == city and timestamp > ago(interval) };
    req(city) | count
```

> `T`v následujících příkladech dotazu se používá k označení předchozí tabulce kanálem k odesílání zpráv nebo zdroje.
> 

### <a name="count-operator"></a>počet operátor

`count` Operátor vrátí počet záznamů (řádků) v zadávání sadě záznamů.

**Syntaxe**

    T | count

**Argumenty**

* *T*: tabulková data jsou jehož záznamy, které mají být spočítány.

**Vrátí**

Tato funkce vrátí tabulku s jedním záznamem a sloupec typu `long`. Pouze buňky hodnotu počtu zobrazených záznamů *T*. 

**Příklad**

```AIQL
requests | count
```

### <a name="evaluate-operator"></a>vyhodnocení operátor

`evaluate`je mechanismus rozšíření, který umožňuje specializované algoritmů připojen k dotazům.

`evaluate`musí být poslední operátor v kanálu dotazu (s výjimkou případnou `render`). Nesmí zobrazí v těle funkce.

[vyhodnocení autocluster](#evaluate-autocluster) | [vyhodnocení košík](#evaluate-basket) | [vyhodnocení diffpatterns](#evaluate-diffpatterns) | [vyhodnocení extractcolumns](#evaluate-extractcolumns)

#### <a name="evaluate-autocluster"></a>vyhodnocení autocluster

     T | evaluate autocluster()

AutoCluster najde běžné vzorce samostatné atributů (rozměry) v datech a sníží výsledků původní dotazu (jestli je 100 nebo 100 kB řádků) malým počtem poštovních vzorků. AutoCluster vyvinutý pomůže analyzovat selhání (například výjimek, dojde k chybě), ale potenciálně pracovat na libovolnou vyfiltrované množině dat. 

**Syntaxe**

    T | evaluate autocluster( arguments )

**Vrátí**

AutoCluster vrátí (obvykle) malého vzorů, které zachytit částem dat s sdílených běžné hodnot napříč několika samostatné atributy. Každý vzorek představuje jeden řádek v výsledky. 

První dva sloupce se počet procent řádků z původní dotaz, které se nezaznamenávají vzorkem. Zbývající sloupce jsou z původního dotazu a jeho hodnota je buď určité hodnoty ze sloupce nebo "*" znamená proměnnými hodnotami. 

Všimněte si, že není nesouvislé vzory: můžou být překrývající se a obvykle se nevztahují na původní řádky. Některé řádky nemusí spadají všechny vzorku.

**Tipy**

* Použití `where` a `project` v zadávání kanálu omezit data jenom co vás zajímá.
* Jakmile hledaný zajímavé řádku, možná budete chtít přejít k podrobnostem dále přidáním jeho určitých hodnot do svého `where` filtr.

**Argumenty (nepovinné)**

* `output=all | values | minimal` 

    Formát výsledky. Počet a procentuální hodnoty sloupce se zobrazí vždy ve výsledcích. 

 * `all`– všechny sloupce ze vstupní jsou výstup
 * `values`– filtruje sloupce pouze "*" ve výsledcích
 * `minimal`-taky filtruje sloupců, které jsou stejné pro všechny řádky v původním dotazu. 


* `min_percent=`*dvojité* (výchozí: 1)

    Minimální procento rozsah vygenerovaných řádků.

    Příklad:`T | evaluate autocluster("min_percent=5.5")`


* `num_seeds=`*Funkce INT* (výchozí: 25) 

    Počet semen určuje počet bodů počáteční místního vyhledávání algoritmu. V některých případech, podle toho, struktury dat zvýšení počtu semen zvýší číslo (nebo kvality) výsledky prostřednictvím lepší hledání prostor pro pomaleji kompromis dotazu. Num_seeds argument má snižuje výsledky v obou směrech, takže zmenšení nižší než 5 dosáhnout zvýšení výkonu zanedbatelné zvýšení větší než 50 jen zřídka vygeneruje další vzorky.

    Příklad:`T | evaluate autocluster("num_seeds=50")`


* `size_weight=`*0 < dvojité < 1*+ (výchozí: 0,5)

    Umožňuje některé kontrolu nad poměr pole Obecné (vysoké pokrytí) a informativní (mnoho sdílené hodnoty). Zvýšení size_weight obvykle snižuje počet vzorků a může každý vzorek pokrýval větší procento. Zmenšení size_weight obvykle vytváří konkrétnější vzorce s více sdílené hodnoty a menší rozsah procentuální hodnotu. Rozšířená vzorec je váženého geometrický průměr mezi normalizovaných skóre obecné a informativní skóre s size_weight a 1 size_weight jako hmotnost. 

    Příklad:`T | evaluate autocluster("size_weight=0.8")`


* `weight_column=`*COLUMN_NAME*

    Byly použity každý řádek ve vstupním podle zadaného tloušťky (ve výchozím nastavení obsahuje každý řádek tloušťku '1'), je běžné využití weight (váha) sloupce s ohledem na odběr účet nebo bucketing/agregace dat, která je již vložený do každého řádku.

    Příklad:`T | evaluate autocluster("weight_column=sample_Count")` 



#### <a name="evaluate-basket"></a>vyhodnocení košík

     T | evaluate basket()

Košík najde všechny časté vzorky abstraktní atributů (rozměry) v datech a vrátí všechny časté vzorky, které mezní počet_plateb předaný původní dotaz. Košík zaručené zobrazíte všechny časté vzorky dat, ale není zaručené mít polynomická runtime. Běhu dotazu je lineární v počet řádků, ale v některých případech může být exponenciální v poli Počet sloupců (rozměry). Košík vychází z algoritmu Apriori původně vyvinutý pro dolování dat analýzy košík. 

**Vrátí**

Všechny vzorky zobrazená ve více než zadaný zlomek (výchozí nastavení 0,05) události.

**Argumenty (nepovinné)**


* `threshold=`*0.015 < dvojité < 1* (výchozí: 0,05) 

    Nastaví minimální poměr řádky, které považuje za časté (nejsou k dispozici vzorků s menší poměr).

    Příklad:`T | evaluate basket("threshold=0.02")`


* `weight_column=`*COLUMN_NAME*

    Byly použity každý řádek ve vstupním podle zadaného tloušťky (ve výchozím nastavení obsahuje každý řádek tloušťku '1'), je běžné využití weight (váha) sloupce s ohledem na odběr účet nebo bucketing/agregace dat, která je již vložený do každého řádku.

    Příklad: T | vyhodnocení basket("weight_column=sample_Count")


* `max_dims=`*1 < int* (výchozí: 5)

    Nastaví maximální počet bez korelace nejsou rozměry za košík omezený ve výchozím nastavení snížit runtime dotazu.


* `output=minimize` | `all` 

    Formát výsledky. Počet a procentuální hodnoty sloupce se zobrazí vždy ve výsledcích.

 * `minimize`– filtruje sloupce pouze "*" ve výsledcích.
 * `all`– všechny sloupce ze vstupní jsou výstupu.




#### <a name="evaluate-diffpatterns"></a>vyhodnocení diffpatterns

     requests | evaluate diffpatterns("split=success")

Diffpatterns porovnání dvou množinách dat. stejné struktury a najde vzorků samostatné atributů (rozměry), které určení rozdíly mezi dvěma množinami dat. Diffpatterns vyvinutý pomůže analyzovat selhání (například pomocí porovnání selhání při bez chyb v dané určeném časovém rozmezí), ale potenciálně najdete rozdíly mezi libovolné dvou množinách dat. stejnou strukturu. 

**Syntaxe**

`T | evaluate diffpatterns("split=`*BinaryColumn*`" [, arguments] )`

**Vrátí**

Diffpatterns vrátí (obvykle) malého vzorů, které zaznamenat různé části dat ve dvou množinách (tedy vzorek zachycení velké procento využití řádků v první sadě dat a nízké řádků v sadě druhý). Každý vzorek představuje jeden řádek v výsledky.

První čtyři sloupce počet a procent řádků z původní dotazu, které se nezaznamenávají vzorkem ve všech sadách, pátý sloupec je mezi dvěma množinami rozdíl (absolutní procent). Zbývající sloupce jsou z původního dotazu a jeho hodnota je buď určité hodnoty ze sloupce nebo * znamená proměnnými hodnotami. 

Všimněte si, že není různých vzorky: můžou být překrývající se a obvykle se nevztahují na původní řádky. Některé řádky nemusí spadají všechny vzorku.

**Tipy**

* Použití where a projektu v zadávání kanálu omezit data jenom co vás zajímá.

* Jakmile hledaný zajímavé řádku, můžete přejít k podrobnostem dále přidáním určitých hodnot do svého where filtrovat.

**Argumenty**

* `split=`*název sloupce* (povinné)

    Sloupci musí obsahovat přesně dvě hodnoty. V případě potřeby vytvořte tyto sloupce:

    `requests | extend fault = toint(resultCode) >= 500` <br/>
    `| evaluate diffpatterns("split=fault")`

* `target=`*řetězec*

    Říká algoritmu pouze při hledání vzorky, které mají vyšší procentuální hodnotu v cílovém uvedenou množinu dat, musí být cíl jednu ze dvou hodnot rozdělit sloupec.

    `requests | evaluate diffpatterns("split=success", "target=false")`

* `threshold=`*0.015 < dvojité < 1* (výchozí: 0,05) 

    Nastaví minimální vzorek (poměr) rozdíl mezi dvěma množinami.

    `requests | evaluate diffpatterns("split=success", "threshold=0.04")`

* `output=minimize | all`

    Formát výsledky. Počet a procentuální hodnoty sloupce se zobrazí vždy ve výsledcích. 

 * `minimize`– filtruje sloupce pouze "*" ve výsledcích
 * `all`– všechny sloupce ze vstupní jsou výstup

* `weight_column=`*COLUMN_NAME*

    Byly použity každý řádek ve vstupním podle zadaného tloušťky (ve výchozím nastavení každý řádek má tloušťku '1'). Společné použití sloupce weight (váha) je promítnutí do účtu odběru nebo bucketing/agregace dat, která je již vložený do každého řádku.

    `requests | evaluate autocluster("weight_column=itemCount")`






#### <a name="evaluate-extractcolumns"></a>vyhodnocení extractcolumns

     exceptions | take 1000 | evaluate extractcolumns("details=json") 

Extractcolumns slouží k obohacení tabulku s několika jednoduchých sloupce, které jsou dynamicky extrahované mimo (skupinou) strukturovaných sloupce podle jejich typu. Nyní podporuje pouze, json sloupce obou dynamické a řetězce serializace jsons.


* `max_columns=`*Funkce INT* (výchozí: 10) 

    Počet nové přidaného sloupce je dynamické a může být příliš velký (ve skutečnosti je počet různých kláves ve všech záznamech json), doporučujeme nutné omezit. Nové sloupce řazené sestupně podle jejich četnost a až max_columns jsou přidané do tabulky.

    `T | evaluate extractcolumns("json_column_name=json", "max_columns=30")`


* `min_percent=`*dvojité* (výchozí: 10.0) 

    Další způsob, jak omezit nové sloupce ignorování sloupce jejichž četnost je menší než min_percent.

    `T | evaluate extractcolumns("json_column_name=json", "min_percent=60")`


* `add_prefix=`*logická hodnota* (výchozí: PRAVDA) 

    True název složité sloupce se přidá jako předponu názvům extrahované sloupců.


* `prefix_delimiter=`*řetězec* (výchozí: "_") 

    Pokud add_prefix = true tento parametr určuje oddělovač použitý ke zřetězení názvy nové sloupce.

    `T | evaluate extractcolumns("json_column_name=json",` <br/>
    `"add_prefix=true", "prefix_delimiter=@")`


* `keep_original=`*logická hodnota* (výchozí: false) 

    True původní sloupce (json) bude nacházet v tabulce výstupu.


* `output=query | table` 

    Formát výsledky. 

 * `table`-Výstup je stejné tabulky jako přijaté mínus zadané vstupní sloupce plus nové sloupce, které byly extrahovaných z vstupní sloupce.
 * `query`-Výstup je typu string představující dotaz, který byste změnili k získání požadovaného výsledku jako tabulku. 




### <a name="extend-operator"></a>rozšíření operátor

     T | extend duration = stopTime - startTime

Přidání jednoho nebo více počítané sloupce do tabulky. 


**Syntaxe**

    T | extend ColumnName = Expression [, ...]

**Argumenty**

* *T:* Vstupní tabulka.
* *Sloupci ColumnName:* Název sloupce, které chcete přidat. [Názvy](#names) se rozlišují velká a můžou obsahovat abecední, číselné nebo "_" znaků. Použití `['...']` nebo `["..."]` klíčová slova nebo názvy s jinými znaky v uvozovkách.
* *Výraz:* Výpočet přes stávajících sloupců.

**Vrátí**

Kopie vstupní tabulka s zadané další sloupce.

**Tipy**

* Použití [`project`](#project-operator) místo toho podle potřeby můžete taky přetáhnout nebo přejmenování některé sloupce.
* Nepoužívejte `extend` jednoduše zobrazíte kratší název, který chcete používat ve výrazu dlouhé. `...| extend x = anonymous_user_id_from_client | ... func(x) ...` 

    Nativní sloupců v tabulce mít indexované; název nového definuje dalšího sloupce, který není indexované, tak, aby se pravděpodobně poběží pomaleji dotaz.

**Příklad**

```AIQL
traces
| extend
    Age = now() - timestamp
```


### <a name="join-operator"></a>operátor spojení

    Table1 | join (Table2) on CommonColumn

Sloučí řádky dvě tabulky tak, že odpovídající hodnoty zadaného sloupce.


**Syntaxe**

    Table1 | join [kind=Kind] (Table2) on CommonColumn [, ...]

**Argumenty**

* *Tabulka1* - "levou stranu" spojení.
* *Tabulka2* - pravé straně spojení. Může být výraz vnořené dotazu, který výstupy tabulky.
* *CommonColumn* - sloupec, který má stejný název ve dvou tabulkách.
* *Typ* - Určuje, jak mají být spárované řádků z obou tabulek.

**Vrátí**

Tabulka se:

* Sloupec pro každý sloupec v každém z obou tabulek, včetně odpovídající klíče. Sloupce (vpravo) se automaticky přejmenovat, pokud jsou tu uvedené střetům název.
* Řádek pro každý shodu mezi vstupních tabulek. Shoda tvoří jeden řádek vybrány z jedné tabulky, která má stejné hodnoty pro všechny `on` pole obsažena jako jeden řádek v druhé tabulce. 

* `Kind`Tento parametr

    Pouze jeden řádek z levé strany spárována pro každou hodnotu `on` klíče. Výstup obsahuje řádek pro každou shoduje s konkrétní řádek s řádky zprava.

* `Kind=inner`
 
     Tvoří jeden řádek záhlaví pro každou kombinaci odpovídajících řádků z doleva a doprava.

* `kind=leftouter`(or `kind=rightouter` or `kind=fullouter`)

     Kromě vnitřní shody není řádek pro každý řádek na doleva (nebo vpravo), i když má nedošlo ke shodě. V takovém případě neshodných výstup buňka obsahuje hodnoty Null.

* `kind=leftanti`

     Vrátí všechny záznamy z levé strany, které nemají odpovídající zprava. Výsledná tabulka obsahuje jenom sloupců z levé strany. 
 
Pokud existuje více řádků se stejnými hodnotami pro tato pole obsažena, dostanete pro všechny kombinace řádků.

**Tipy**

Pro dosažení nejlepších výsledků:

* Použití `where` a `project` ke snížení počtu řádků a sloupců v tabulce zadávání před `join`. 
* Pokud jednu tabulku je vždy menší než druhý, můžete ho použijte jako (vytvoření kanálu) levé části spojení.
* Sloupce pro porovnání spojení musí mít stejný název. V případě potřeby přejmenujte sloupec v jednom z tabulky, použijte operátor projektu.

**Příklad**

Získání obnovována protokol některé položky značka začátek a konec aktivitu aktivity. 

```AIQL
    let Events = MyLogTable | where type=="Event" ;
    Events
  	| where Name == "Start"
  	| project Name, City, ActivityId, StartTime=timestamp
  	| join (Events
           | where Name == "Stop"
           | project StopTime=timestamp, ActivityId)
        on ActivityId
  	| project City, ActivityId, StartTime, StopTime, Duration, StopTime, StartTime

```


### <a name="limit-operator"></a>limit operátor

     T | limit 5

Vrátí nahoru na zadaný počet řádků v tabulce zadávání. Není nijak zaručené záznamy, které vracejí. (Pokud chcete vrátit konkrétních záznamů, použijte [`top`](#top-operator).)

**Alias**`take`

**Syntaxe**

    T | limit NumberOfRows


**Tipy**

`Take`je jednoduchý a efektivně způsob, jak zjistit vzorek výsledky při spolupráci interaktivně. Mějte na paměti, že nezaručuje výsledkem žádné určité řádky nebo předložit je v určitém pořadí.

Existuje implicitní maximální počet řádků vrácených klientovi, i když nebudete používat `take`. Zrušit toto omezení, použít `notruncation` možnost požadavku klienta.



### <a name="mvexpand-operator"></a>operátor mvexpand

    T | mvexpand listColumn 

Rozbalí seznam v buňce zadané dynamické (JSON) tak, že každá položka má na samostatném řádku. Ostatních buněk v rozbalené řádku jsou duplicitní. 

(Viz také [`summarize makelist`](#summarize-operator) která provede funkci opačný.)

**Příklad**

Předpokládejme, že je vstupní tabulka:

|A:int|B:String|D:Dynamic|
|---|---|---|
|1|"Ahoj"|{"klávesy": "hodnotu"}|
|2|"světa"|[0,1, "k", "v"]|

    mvexpand D

Výsledek vypadat takto:

|A:int|B:String|D:Dynamic|
|---|---|---|
|1|"Ahoj"|{"klávesy": "hodnotu"}|
|2|"světa"|0|
|2|"světa"|1|
|2|"světa"|"k"|
|2|"světa"|"v"|


**Syntaxe**

    T | mvexpand  [bagexpansion=(bag | array)] ColumnName [limit Rowlimit]

    T | mvexpand  [bagexpansion=(bag | array)] [Name =] ArrayExpression [to typeof(Typename)] [limit Rowlimit]

**Argumenty**

* *Sloupci ColumnName:* Ve výsledku pole ve sloupci pojmenované rozbaleny více řádků. 
* *ArrayExpression:* Získávání matice výraz. Pokud tento formulář slouží, je přidán nový sloupec a existující se zachová.
* *Název:* Název pro nový sloupec.
* *Typename:* Přetypuje rozbalené výraz, který má určitého typu
* *RowLimit:* Maximální počet řádků generovaných každý řádek původní. Výchozí hodnota je 128.

**Vrátí**

Více řádků pro jednotlivé hodnoty v libovolné pole ve sloupci pojmenované nebo matice výraz.

Rozbalené sloupec vždy má dynamické typ. Použít cast, jako je `todatetime()` nebo `toint()` Pokud chcete vypočítat nebo agregačních hodnot.

Jsou dva způsoby rozšíření balík vlastností podporované:

* `bagexpansion=bag`: Vlastnost vaky rozbaleny do jedné položky vlastnost vaky. Toto je výchozí rozšíření.
* `bagexpansion=array`: Vlastnost vaky rozbaleny do dvou prvek `[` *klíč*`,`*hodnotu* `]` pole struktury, povolení jednotný přístup k klíče a hodnoty (stejně jako, například systém agregace počtu jedinečných položek na vlastnost jméno). 

**Příklady**


    exceptions | take 1 
  	| mvexpand details[0]

Záznam výjimky rozdělí řádků pro každou položku v poli Podrobnosti.



### <a name="parse-operator"></a>operátor analýzy

    T | parse "I got 2 socks for my birthday when I was 63 years old" 
    with * "got" counter:long " " present "for" * "was" year:long *


    T | parse kind=relaxed
          "I got no socks for my birthday when I was 63 years old" 
    with * "got" counter:long " " present "for" * "was" year:long * 

    T |  parse kind=regex "I got socks for my 63rd birthday" 
    with "(I|She) got" present "for .*?" year:long * 

Extrahuje hodnot z řetězce. Použít jednoduchý nebo běžná výraz odpovídající.

**Syntaxe**

    T | parse [kind=regex|relaxed] SourceText 
        with [Match | Column [: Type [*]] ]  ...

**Argumenty**

* `T`: Vstupní tabulka.
* `kind`: 
 * `simple`(výchozí): `Match` řetězce jsou jednoduchým řetězce.
 * `relaxed`: Pokud text nelze analyzovat jako typ sloupce, sloupci je nastavený na hodnotu null a analýzy budou dál problémy 
 * `regex`: `Match` regulární výrazy jsou řetězce.
* `Text`: Sloupce nebo jiných výraz, který je vyhodnocen jako nebo může být převeden na řetězec.
* *POZVYHLEDAT:* Odpovídají další část řetězce a zahodit.
* *Sloupce:* Další část řetězce přiřadíte tento sloupec. Sloupce se vytvoří, pokud není k dispozici.
* *Typ:* Analyzovat další části řetězec zadané jako například int, datum, dvojité. 


**Vrátí**

Vstupní tabulka rozšířené podle seznamu sloupců.

Prvky v `with` klauzule jsou porovnány s zdrojový text zase. Každý prvek chews vypnout bloku textu zdroje: 

* Literál typu řetězec nebo regulárních výrazů kurzor odpovídající délkou porovnat.
* V regex analýzy regulárních výrazů pomocí operátoru minimalizace "?" co nejdříve přesunete následující POZVYHLEDAT.
* Název sloupce k typu analyzuje zadaný jako text. Pokud typ = naleznete, neúspěšná analýzy zruší platnost odpovídající celé vzorci.
* Název sloupce bez typu nebo s typem "řetězec", zkopíruje minimální počet znaků pro přístup k následujícím porovnat.
* " *" Přeskočí minimální počet znaků pro přístup k následujícím POZVYHLEDAT. Můžete použít "*" na začátku a konci vzorku, nebo za typu než řetězec nebo mezi řetězců odpovídajících.

Všechny prvky ve vzorci analýzy se musí shodovat se správně; v ostatních případech bude vyrobeno žádné výsledky. Výjimkou tohoto pravidla je, že když druhu = naleznete, když analýza proměnnou zadaný nepovede, budou dál problémy zbytek analyze.

**Příklady**

*Jednoduchý:*

```AIQL

// Test without reading a table:
 range x from 1 to 1 step 1 
 | parse "I got 2 socks for my birthday when I was 63 years old" 
    with 
     *   // skip until next match
     "got" 
     counter: long // read a number
     " " // separate fields
     present // copy string up to next match
     "for" 
     *  // skip until next match
     "was" 
     year:long // parse number
     *  // skip rest of string
```

x | Čítač | prezentace | Rok
---|---|---|---
1 | 2 | SOCKS (ponožky) | 63

*Zmírněny:*

Po zadání obsahuje správné shodu každý zadaný sloupec, naleznete analýzy vytvoří stejné výsledky jako jednoduchý analýzy. Ale pokud jeden z zadaný sloupců není správně analyzovat, naleznete analýzy zpracuje zbytek vzoru, že jednoduché analýzy zastaví a přestane generovat některý z výsledků.


```AIQL

// Test without reading a table:
 range x from 1 to 1 step 1 
 | parse kind="relaxed"
        "I got several socks for my birthday when I was 63 years old" 
    with 
     *   // skip until next match
     "got" 
     counter: long // read a number
     " " // separate fields
     present // copy string up to next match
     "for" 
     *  // skip until next match
     "was" 
     year:long // parse number
     *  // skip rest of string
```


x  | prezentace | Rok
---|---|---
1 |  SOCKS (ponožky) | 63


*Regex:*

```AIQL

// Run a test without reading a table:
range x from 1 to 1 step 1 
// Test string:
| extend s = "Event: NotifySliceRelease (resourceName=Scheduler, totalSlices=27, sliceNumber=16, lockTime=02/17/2016 08:41, releaseTime=02/17/2016 08:41:00, previousLockTime=02/17/2016 08:40:00)" 
// Parse it:
| parse kind=regex s 
  with ".*?[a-zA-Z]*=" resource 
       ", total.*?sliceNumber=" slice:long *
       "lockTime=" lock
       ",.*?releaseTime=" release 
       ",.*?previousLockTime=" previous:date 
       ".*\\)"
| project-away x, s
```

zdroje | výseč | Zámek | Uvolněte | předchozí
---|---|---|---|---
Plánovač | 16 | 02/17/2016 08:41:00 | 02/17/2016 08:41 | 2016: 02-17T08:40:00Z

### <a name="project-operator"></a>operátor projektu

    T | project cost=price*quantity, price

Vyberte sloupce, které chcete zahrnout, přejmenovat nebo přetáhnout a vložte nového vypočítané sloupce. Pořadí sloupců ve výsledku není zadán pořadím argumenty. Pouze sloupce, podle argumenty jsou zahrnuty ve výsledku: všechny ostatní ve vstupním se nezobrazí.  (Viz také `extend`.)


**Syntaxe**

    T | project ColumnName [= Expression] [, ...]

**Argumenty**

* *T:* Vstupní tabulka.
* *Sloupci ColumnName:* Název sloupce se zobrazí v výstupu. Pokud je bez *výraz*sloupce s tímto názvem musí být ve vstupním. [Názvy](#names) se rozlišují velká a můžou obsahovat abecední, číselné nebo "_" znaků. Použití `['...']` nebo `["..."]` klíčová slova nebo názvy s jinými znaky v uvozovkách.
* *Výraz:* Volitelné skalární výraz, který odkazuje vstupní sloupce. 

    Je možné vrátit novém počítaném sloupci se stejným názvem jako existující sloupec ve vstupním.

**Vrátí**

Tabulku obsahující sloupce s názvem jako argumenty a, kolik řádků jako vstupní tabulka.

**Příklad**

Následující příklad ukazuje několik typů manipulace, které lze provést pomocí `project` operátor. Vstupní tabulka `T` má tři sloupce typu `int`: `A`, `B`, a `C`. 

```AIQL
T
| project
    X=C,               // Rename column C to X
    A=2*B,             // Calculate a new column A from the old B
    C=strcat("-",tostring(C)), // Calculate a new column C from the old C
    B=2*B,              // Calculate a new column B from the old B
    ['where'] = client_City // rename, using a keyword as a column name
```

### <a name="project-away-operator"></a>Projekt zase operátor

    T | project-away column1, column2, ...

Vyloučení zadané sloupce. Výsledek obsahuje všechny vstupní sloupce s výjimkou těch, které zadáte název.

### <a name="range-operator"></a>Operátor oblasti

    range LastWeek from ago(7d) to now() step 1d

Vytvoří tabulku jedním sloupcem hodnot. Všimněte si, že nemá kanálu při zadávání. 

|LastWeek|
|---|
|2015 12 05 09:10:04.627|
|2015-12-06 09:10:04.627|
|...|
|09:10:04.627 2015 12 12|



**Syntaxe**

    range ColumnName from Start to Stop step Step

**Argumenty**

* *Sloupci ColumnName:* Název jednoho sloupce v tabulce výstupu.
* *Start:* Nejmenší hodnota z výstupu.
* *Zastavit:* Nejvyšší hodnotu generovaná v výstup (nebo vázaný na nejvyšší hodnotu, pokud *Krok* kroky přes tuto hodnotu).
* *Krok:* Rozdíl mezi dvěma hodnotami po sobě jdoucí. 

Argumenty musí být numerické, data nebo časového rozpětí hodnoty. Budou nelze odkazovat na sloupce žádnou tabulku. (Pokud chcete spočítat rozsah založený na vstupní tabulka, pomocí této [oblasti *funkce*](#range)možná [mvexpand operátor](#mvexpand-operator).) 

**Vrátí**

Tabulka s jedním sloupcem s názvem *sloupci ColumnName*, jehož hodnoty jsou *Spustit*, *Spusťte* + *Krok*... do a včetně *Zastavit*.

**Příklad**  

```AIQL
range Steps from 1 to 8 step 3
```

Tabulka s jedním sloupcem nazývá `Steps` typem `long` a jehož hodnoty jsou `1`, `4`, a `7`.

**Příklad**

    range LastWeek from bin(ago(7d),1d) to now() step 1d

Tabulka půlnoci posledních sedmi dnů. Funkce koše (podlahy) snižuje pokaždé, když na začátek dne.

**Příklad**  

```AIQL
range timestamp from ago(4h) to now() step 1m
| join kind=fullouter
  (traces
      | where timestamp > ago(4h)
      | summarize Count=count() by bin(timestamp, 1m)
  ) on timestamp
| project Count=iff(isnull(Count), 0, Count), timestamp
| render timechart  
```

Ukazuje, jak `range` operátor můžete použít k vytvoření malé, ad-hoc, dimenze tabulky, která je pak použít představující nul kde zdrojových dat neobsahuje žádné hodnoty.

### <a name="reduce-operator"></a>zmenšení operátor

    exceptions | reduce by outerMessage

Pokusí se seskupení tak podobně jako záznamy. Pro každou skupinu operátor výstupy `Pattern` sbor nejvíce odpovídá danou skupinu a `Count` záznamů v dané skupině.


![](./media/app-insights-analytics-reference/reduce.png)

**Syntaxe**

    T | reduce by  ColumnName [ with threshold=Threshold ]

**Argumenty**

* *Sloupci ColumnName:* Sloupec prozkoumat. Musí být typu řetězec.
* *Mezní hodnota:* Hodnoty v oblasti {0..1}. Výchozí hodnota je 0,001. Pro velké vstupy by měly být stručné mezní hodnota. 

**Vrátí**

Dva sloupce `Pattern` a `Count`. V mnoha případech bude vzorek dokončení hodnotu ve sloupci. V některých případech může identifikovat společné termíny nebo nahrazení proměnné části s "*".

Příklad výsledku `reduce by city` patří: 

|Vzor | Počet |
|---|---|
| SAN * | 5182 |
| Svatý * | 2846 |
| Moskva | 3726 |
| \*-na-\* | 2730 |
| Paříž | 27163 |


### <a name="render-directive"></a>vykreslení směrnice

    T | render [ table | timechart  | barchart | piechart ]

Vykreslení směrovat tak, aby prezentace vrstvy zobrazení v tabulce. Je třeba poslední prvek kanálu. Je vhodné alternativy pomocí ovládacích prvků na zobrazení umožňuje uložit dotaz pomocí metody určité prezentace.

### <a name="restrict-clause"></a>omezit klauzule 

Určuje množinu k dispozici pro operátory, které následují za názvy tabulek. Příklad:

    let e1 = requests | project name, client_City;
    let e2 =  requests | project name, success;
    // Exclude predefined tables from the union:
    restrict access to (e1, e2);
    union * |  take 10 

### <a name="sort-operator"></a>operátor řazení 

    T | sort by country asc, price desc

Řazení řádků vstupní tabulka do pořadí podle jednoho nebo víc sloupců.

**Alias**`order`

**Syntaxe**

    T  | sort by Column [ asc | desc ] [ `,` ... ]

**Argumenty**

* *T:* Tabulka vstupní seřadit.
* *Sloupce:* Sloupec typu *T* , podle kterého chcete řadit. Typ hodnoty musí být číselné, datum, čas nebo řetězec.
* `asc`Řazení podle do vzestupně, nejnižší na hodnotu Vysoká. Výchozí hodnota je `desc`, sestupně nejvyšší po nejnižší.

**Příklad**

```AIQL
Traces
| where ActivityId == "479671d99b7b"
| sort by Timestamp asc
```
Všechny řádky v trasování tabulky, které mají konkrétní `ActivityId`, seřazené podle jejich časové razítko.

### <a name="summarize-operator"></a>Souhrn operátor

Vytvoří tabulku, která sloučí obsah vstupní tabulka.
 
    requests
  	| summarize count(), avg(duration), makeset(client_City) 
      by client_CountryOrRegion

Tabulka, která zobrazí číslo, doba trvání průměr požadavku a sada měst v příslušné země. V seznamu výstup u všech států, distinct tvoří jeden řádek. Výstupní sloupce zobrazit počet, průměr doba trvání, měst a země. Všechny ostatní vstupní sloupce jsou ignorovány.


    T | summarize count() by price_range=bin(price, 10.0)

Tabulky, který ukazuje počet kupovaných položek mít ceny v každém intervalu [0,10.0], [10.0,20.0], a tak dál. V tomto příkladu má sloupec za počet a cena pojmenované oblasti. Všechny ostatní vstupní sloupce jsou ignorovány.


**Syntaxe**

    T | summarize
         [  [ Column = ] Aggregation [ `,` ... ] ]
         [ by
            [ Column = ] GroupExpression [ `,` ... ] ]

**Argumenty**

* *Sloupce:* Volitelné název sloupce výsledek. Výchozí název odvozeno z výrazu. [Názvy](#names) se rozlišují velká a můžou obsahovat abecední, číselné nebo "_" znaků. Použití `['...']` nebo `["..."]` klíčová slova nebo názvy s jinými znaky v uvozovkách.
* *Agregace:* Volání funkce agregace jako `count()` nebo `avg()`, s názvy sloupců jako argumenty. V tématu [agregace](#aggregations).
* *GroupExpression:* Výraz nad velikostí, který obsahuje sadu různých hodnot. Obvykle je to buď název sloupce, který už poskytuje omezené sadu hodnot, nebo `bin()` se sloupcem číselné nebo času jako argument. 

Pokud zadáte výraz číselné nebo časového bez použití `bin()`, analýzy automaticky platí s intervalem `1h` pro situace, nebo `1.0` pro čísla.

Pokud neposkytnete *GroupExpression,* celé tabulky je uveden v řádku jeden výstup.



**Vrátí**

Zadávání řádky jsou uspořádány do skupin mají stejné hodnoty `by` výrazů. Potom zadaný agregační funkce jsou vyhodnocované nad každou skupinu vytváření řádek pro každou skupinu. Výsledek obsahuje `by` sloupců a také alespoň jeden sloupec pro jednotlivá pole vyhodnocované agregaci. (Některé funkce agregace vrátit víc sloupců).

Výsledek nemá tolik řádky jsou odlišné kombinací `by` hodnoty. Pokud chcete sumarizovat nad oblastí číselné hodnoty, použijte `bin()` zmenšit oblastí na samostatné hodnoty.

**Poznámka:**

Přestože zadáte libovolného výrazy pro agregaci a výrazy seskupování je výhodnější používat názvy simple sloupců nebo můžete použít `bin()` číselné sloupce.



### <a name="take-operator"></a>Prohlédněte operátor

Alias [limit](#limit-operator)


### <a name="top-operator"></a>začátek operátor

    T | top 5 by Name desc nulls first

Vrátí první *N* záznamy seřazené podle zadané sloupce.


**Syntaxe**

    T | top NumberOfRows by Sort_expression [ `asc` | `desc` ] [`nulls first`|`nulls last`] [, ... ]

**Argumenty**

* *NumberOfRows:* Počet řádků *T* vrátit.
* *Sort_expression:* Výraz, podle kterého chcete řadit řádky. Je to obvykle jenom název sloupce. Můžete zadat víc než jeden sort_expression.
* `asc`nebo `desc` (výchozí) se může zobrazit ovládací prvek zdali výběru skutečně pochází z "dole" nebo "nahoře" oblasti.
* `nulls first`nebo `nulls last` ovládací prvky, které se zobrazí hodnoty null. `First`Výchozí nastavení pro `asc`, `last` je výchozí `desc`.


**Tipy**

`top 5 by name`odpovídá na první pohled `sort by name | take 5`. Však pracovat rychleji a vždy vrátí seřazené výsledky, že `take` zajišťuje nijak zaručené.

### <a name="top-nested-operator"></a>horní vnořené operátor

    requests 
  	| top-nested 5 of name by count()  
    , top-nested 3 of performanceBucket by count() 
    , top-nested 3 of client_CountryOrRegion by count()
  	| render barchart 

Poskytuje hierarchické výsledky, kde je přechodu z předchozí úrovni jednotlivých úrovní. Je vhodné pro odpovědi na otázky, které zvukové jako "co jsou nejčastější 5 požadavky pro každý z nich, jaké jsou bloky horním 3 výkonu a pro každou z nich, které jsou zemí horní 3 požadavky pocházet z?"

**Syntaxe**

   T | horní vnořené N ze sloupce podle AGREGAČNÍ [; …]

**Argumenty**

* N:int - počet řádků, které chcete vrátit nebo předání na vyšší úroveň. Celkový počet řádků v dotazu se třemi úrovněmi kde N je 5, 3 a 3, bude 45.
* SLOUPEC – sloupce na seskupit podle pro agregaci. 
* AGREGACE – [funkce agregace](#aggregations) použít pro každou skupinu řádků. Výsledky těchto agregace bude určovat, horní skupiny zobrazovat.


### <a name="union-operator"></a>operátor sjednocení

     Table1 | union Table2, Table3

Má dvě nebo více tabulek a vrátí řádky z nich. 

**Syntaxe**

    T | union [ kind= inner | outer ] [ withsource = ColumnName ] Table2 [ , ...]  

    union [ kind= inner | outer ] [ withsource = ColumnName ] Table1, Table2 [ , ...]  

**Argumenty**

* *Tabulka1*, *tabulka2* ...
 *  Název tabulky, jako například `requests`, nebo tabulce určené v [nechat klauzule](#let-clause); nebo
 *  A dotazu výraz, jako například`(requests | where success=="True")`
 *  Sada tabulek zadaným zástupný znak. Například `e*` tvoří sjednocení všechny tabulky podle předchozího upozorní klauzule jmenoval začalo "e" a 'výjimky' tabulka.
* `kind`: 
 * `inner`– Výsledek je podmnožinu sloupců, které jsou společná pro všechny vstupních tabulek.
 * `outer`– Výsledek obsahuje všechny sloupce probíhajících v některém z vstupní hodnoty. Buňky, které nebyly definované vstupního řádku jsou nastavené `null`.
* `withsource=`*Sloupci ColumnName:* Pokud je zadaná, výstup bude obsahovat sloupce s názvem *sloupci ColumnName* jejichž hodnota označuje, které zdrojové tabulky přispěla každý řádek.

**Vrátí**

Tabulka s tolik řádků, že jsou ve všech tabulkách zadávání a všechny sloupce, protože to je jedinečný sloupec jmen v vstupní hodnoty.

**Příklad**

```AIQL

let ttrr = requests | where timestamp > ago(1h);
let ttee = exceptions | where timestamp > ago(1h);
union tt* | count
```
Union všech tabulek, jejichž názvy začínají "tt".


**Příklad**

```AIQL

union withsource=SourceTable kind=outer Query, Command
| where Timestamp > ago(1d)
| summarize dcount(UserId)
```
Počet jedinečných uživatelé, kteří mají vyrobeno buď `exceptions` událost nebo `traces` událost myší na předchozí den. Ve výsledku bude označovat sloupci 'SourceTable' "Dotaz" nebo "Příkaz".

```AIQL
exceptions
| where Timestamp > ago(1d)
| union withsource=SourceTable kind=outer 
   (Command | where Timestamp > ago(1d))
| summarize dcount(UserId)
```

Tato verze efektivnější vytvoří stejný výsledek. Všechny tabulky jsou filtrovány před vytvořením unie.

### <a name="where-operator"></a>kde operátor

     requests | where resultCode==200

Filtrování tabulky umožňuje podmnožinu řádky, které splňují predikátem.

**Alias**`filter`

**Syntaxe**

    T | where Predicate

**Argumenty**

* *T:* Tabulkové vstupní jejichž záznamy mají být filtrována.
* *Predikátu:* A `boolean` [výraz](#boolean) nad velikostí *T*. Je vyhodnocen pro každý řádek v *T*.

**Vrátí**

Řádky v *T* , pro kterou *predikátu* hodnotu `true`.

**Tipy**

Chcete-li získat nejrychlejší výkon:

* **Použití jednoduchého porovnání** mezi názvy sloupců a konstanty. (Konstanta"znamená, že konstanta myši na tabulku - tak, aby `now()` a `ago()` jsou v pořádku, a proto jsou skalární hodnoty přiřazené pomocí [ `let` klauzule](#let-clause).)

    Například raději `where Timestamp >= ago(1d)` k `where floor(Timestamp, 1d) == ago(1d)`.

* **Simplest podmínky první**: Pokud máte víc klauzule conjoined s `and`, vložte nejdřív věty, které se týkají pouze jeden sloupec. Takže `Timestamp > ago(1d) and OpId == EventId` je lepší než opačným způsobem.


**Příklad**

```AIQL
traces
| where Timestamp > ago(1h)
    and Source == "Kuskus"
    and ActivityId == SubActivityIt 
```

Záznamy, které jsou již starší než 1 hodinu přijdou ze zdroje s názvem "Kuskus" a mít dva sloupce stejné hodnoty. 

Všimněte si, že jsme umístění porovnání mezi dva sloupce poslední, nemůže využít index a vynutí součtu.


### <a name="where-in-operator"></a>kde operátor

    requests | where resultCode !in (200, 201)

    requests | where resultCode in (403, 404)

**Syntaxe**

    T | where col in (expr1, expr2, ...)
    T | where col !in (expr1, expr2, ...)

**Argumenty**

* `col`: Sloupce v tabulce.
* `expr1`...: Seznam skalární výrazů.

Použití `in` slouží k zahrnout pouze řádky, ve kterém `col` je rovno jedním z výrazů `expr1...`.

Použití `!in` zahrnout pouze řádky, které `col` není rovno některý z výrazů `expr1...`.  


## <a name="aggregations"></a>Agregace

Agregace se funkcí sloužící ke kombinování hodnot ve skupinách vytvořené v [souhrnu operace](#summarize-operator). V tomto dotazu je dcount() například funkce agregace:

    requests | summarize dcount(name) by success

### <a name="any"></a>všechny 

    any(Expression)

Náhodně vybere jeden řádek skupiny a vrátí hodnotu zadaný výraz.

To je užitečné, třeba když nějaký sloupec obsahuje velké množství podobné hodnoty (například sloupec "Chyba text") a chcete-li ukázkový tento sloupec jednou za jedinečné hodnoty klíče složeného skupiny. 

**Příklad**  

```

traces 
| where timestamp > now(-15min)  
| summarize count(), any(message) by operation_Name 
| top 10 by count_level desc 
```

<a name="argmin"></a>
<a name="argmax"></a>
### <a name="argmin-argmax"></a>argmin argmax

    argmin(ExprToMinimize, * | ExprToReturn  [ , ... ] )
    argmax(ExprToMaximize, * | ExprToReturn  [ , ... ] ) 

Najde řádek ve skupině, která slouží k minimalizaci/maximalizace *ExprToMaximize*a vrátí hodnotu *ExprToReturn* (nebo `*` se vrátíte na celý řádek).

**Tip**: předané prostřednictvím sloupce se automaticky přejmenovat. Abyste měli jistotu, že používáte správné názvy, Kontrola výsledků pomocí `take 5` před kanálu výsledků do jiného operátora.

**Příklady**

Každý název žádost o zobrazte, kdy došlo k zadání nejdelší:

    requests | summarize argmax(duration, timestamp) by name

Zobrazení všech podrobností pozvánce na schůzku nejdelší, nikoli pouze časové razítko:

    requests | summarize argmax(duration, *) by name


Najde nejnižší hodnotu každý míru spolu s jeho časového razítka a dalších dat:

    metrics 
  	| summarize minValue=argmin(value, *) 
      by name


![](./media/app-insights-analytics-reference/argmin.png)
 


### <a name="avg"></a>AVG

    avg(Expression)

Vypočítá průměr *výrazů* ve skupině.

### <a name="buildschema"></a>buildschema

    buildschema(DynamicExpression)

Vrátí minimální schéma, které všechny hodnoty *DynamicExpression*přijímá. 

Sloupec typ parametrů by měl být `dynamic` -maticových nebo vlastnosti od. 

**Příklad**

    exceptions | summarize buildschema(details)

Výsledek:

    { "`indexer`":
     {"id":"string",
       "parsedStack":
       { "`indexer`": 
         {  "level":"int",
            "assembly":"string",
            "fileName":"string",
            "method":"string",
            "line":"int"
         }},
      "outerId":"string",
      "message":"string",
      "type":"string",
      "rawStack":"string"
    }}

Všimněte si, že `indexer` se používá k označení, které byste měli použít číselné indexu. Pro toto schéma některé platné cesty by být (za předpokladu, že tyto indexy příkladu jsou v oblasti):

    details[0].parsedStack[2].level
    details[0].message
    arraylength(details)
    arraylength(details[0].parsedStack)

**Příklad**

Předpokládejme, že vstupní sloupec obsahuje tři dynamické hodnoty:

| |
|---|
|`{"x":1, "y":3.5}`
|`{"x":"somevalue", "z":[1, 2, 3]}`
|`{"y":{"w":"zzz"}, "t":["aa", "bb"], "z":["foo"]}`


Výsledný schématu budou:

    { 
      "x":["int", "string"], 
      "y":["double", {"w": "string"}], 
      "z":{"`indexer`": ["int", "string"]}, 
      "t":{"`indexer`": "string"} 
    }

Schéma víme, který:

* Kořenový objekt je kontejner s čtyři vlastnosti s názvem x, y, z a t.
* Vlastnost s názvem "x" může být buď typu "int" nebo "řetězec".
* Vlastnost s názvem používá písmeno "a", která může buď typu "dvojité" nebo jiný kontejner vlastnost s názvem "w" typ "řetězec".
* ``indexer`` Klíčové slovo označuje, že "z" a "t" matice.
* Každá položka v poli "z" je int nebo řetězec.
* "t" je pole řetězců.
* Každý vlastnost je implicitně volitelné a jakékoliv pole může být prázdné.

##### <a name="schema-model"></a>Model schéma

Syntaxe schématu vrácené je:

    Container ::= '{' Named-type* '}';
    Named-type ::= (name | '"`indexer`"') ':' Type;
    Type ::= Primitive-type | Union-type | Container;
    Union-type ::= '[' Type* ']';
    Primitive-type ::= "int" | "string" | ...;

Jsou odpovídající podmnožinu stroji typ poznámky, kódovaný jako dynamické hodnoty. Na stroji budou schématu příkladu:

    var someobject: 
    { 
      x?: (number | string), 
      y?: (number | { w?: string}), 
      z?: { [n:number] : (int | string)},
      t?: { [n:number]: string } 
    }


### <a name="count"></a>počet

    count([ Predicate ])

Vrátí počet řádků, pro které *predikátu* vyhodnocena jako `true`. Pokud není zadán žádný *predikátu* , vrátí celkový počet záznamů ve skupině. 

**Tip výkon**: použití `summarize count(filter)` místo`where filter | summarize count()`

> [AZURE.NOTE] Nepoužívejte count() ke zjištění počtu požadavky, výjimky nebo jiné události, ke kterým došlo. Pokud [odběr](app-insights-sampling.md) je operace, bude počet datových bodů jsou zachovány v aplikaci přehledy menší než počet původní událostí. Místo toho použít `summarize sum(itemCount)...`. Vlastnost itemCount odpovídá počtu původní události, které představují všechny dosažených datové body.

### <a name="countif"></a>COUNTIF

    countif(Predicate)

Vrátí počet řádků, pro které *predikátu* vyhodnocena jako `true`.

**Tip výkon**: použití `summarize countif(filter)` místo`where filter | summarize count()`

> [AZURE.NOTE] Nepoužívejte countif() ke zjištění počtu žádosti o výjimky a dalších událostí, ke kterým došlo. Pokud [odběr](app-insights-sampling.md) je operace, bude počet datových bodů menší než počet skutečných událostí. Místo toho použít `summarize sum(itemCount)...`. Vlastnost itemCount odpovídá počtu původní události, které představují všechny dosažených datové body.

### <a name="dcount"></a>DPOČET

    dcount( Expression [ ,  Accuracy ])

Vrátí odhad počet různých hodnot *výraz* ve skupině. (Pro seznam různých hodnot, použijte [`makeset`](#makeset).)

*Přesnost*, pokud je zadaná, řídí zůstatek mezi rychlostí a přesnosti.

 * `0`= nejméně přesné a nejrychlejší výpočtu.
 * `1`Výchozí zůstatky přesnosti a výpočtu čas; informace o chybě 0,8 %.
 * `2`= nejčastěji přesné a nejnižší výpočtu; informace o chybě 0.4 %.

**Příklad**

    pageViews 
  	| summarize cities=dcount(client_City) 
      by client_CountryOrRegion

![](./media/app-insights-analytics-reference/dcount.png)


### <a name="dcountif"></a>dcountif

    dcountif( Expression, Predicate [ ,  Accuracy ])

Vrátí odhad počet různých hodnot *výraz* řádky ve skupině, pro kterou *predikátu* hodnotu true. (Pro seznam různých hodnot, použijte [`makeset`](#makeset).)

*Přesnost*, pokud je zadaná, řídí zůstatek mezi rychlostí a přesnosti.

 * `0`= nejméně přesné a nejrychlejší výpočtu.
 * `1`Výchozí zůstatky přesnosti a výpočtu čas; informace o chybě 0,8 %.
 * `2`= nejčastěji přesné nejnižší výpočty a; informace o chybě 0.4 %.

**Příklad**

    pageViews 
  	| summarize cities=dcountif(client_City, client_City startswith "St") 
      by client_CountryOrRegion


### <a name="makelist"></a>makelist

    makelist(Expr [ ,  MaxListSize ] )

Vrátí `dynamic` (JSON) pole všech hodnot *výraz* ve skupině. 

* *MaxListSize* je volitelné celé číslo omezený na maximální počet prvků vrácených (výchozí hodnota je *128*).

### <a name="makeset"></a>makeset

    makeset(Expression [ , MaxSetSize ] )

Vrátí `dynamic` matice (JSON) množiny různých hodnot, které přebírají *výraz* ve skupině. (Tip: počítat pouze jedinečné hodnoty, použijte [`dcount`](#dcount).)
  
*  *MaxSetSize* je volitelné celé číslo omezený na maximální počet prvků vrácených (výchozí hodnota je *128*).

**Příklad**

    pageViews 
  	| summarize cities=makeset(client_City) 
      by client_CountryOrRegion

![](./media/app-insights-analytics-reference/makeset.png)

Viz také [ `mvexpand` operátor](#mvexpand-operator) opačný funkce.


### <a name="max-min"></a>Max, min

    max(Expr)

Vypočítá maximální *výraz*.
    
    min(Expr)

Vypočítá minimální *výraz*.

**Tip**: to je k dispozici min nebo max na samostatném – například, nejvyšší nebo nejnižší ceny. Ale pokud chcete, aby ostatní sloupce v řádku – například název dodavateli s nejnižší ceny - pomocí [argmin nebo argmax](#argmin-argmax).


<a name="percentile"></a>
<a name="percentiles"></a>
<a name="percentilew"></a>
<a name="percentilesw"></a>
### <a name="percentile-percentiles-percentilew-percentilesw"></a>percentil percentily, percentilew, percentilesw

    percentile(Expression, Percentile)

Vrátí odhad *výraz* zadaný percentil ve skupině. Přesnost závisí na hustotu populace v oblasti percentil.
    
    percentiles(Expression, Percentile1 [ , Percentile2 ...] )

Jako `percentile()`, ale vypočítá počet hodnot percentilu (což je rychlejší než výpočtu každý percentilu jednotlivě).

    percentilew(Expression, WeightExpression, Percentile)

Váženého percentilu. Pomocí tohoto příkazu pro předem souhrnné údaje.  `WeightExpression`je celé číslo, které určuje, kolik řádků původní představují všech souhrnných řádků.

    percentilesw(Expression, WeightExpression, Percentile1, [, Percentile2 ...])

Jako `percentilew()`, ale vypočítá počet hodnot percentilu.

**Příklady**


Hodnota `duration` , která je větší než 95 % vzorku nastavovat a menší než 5 % množiny ukázkové výpočtu pro každý požadavek název:

    request 
  	| summarize percentile(duration, 95)
      by name

Vynechat "tak, že..." při výpočtu pro celou tabulku.

Výpočet současně několik percentily pro různé žádosti názvy:

    
    requests 
  	| summarize 
        percentiles(duration, 5, 20, 50, 80, 95) 
      by name

![](./media/app-insights-analytics-reference/percentiles.png)

Výsledky ukážou, který pro /Events/Index žádost o 5 % žádostí o jsou odpovědí na za méně než 2.44s, polovina roku je v 3.52s, a 5 % jsou nižší než 6.85s.

Výpočet více Statistika:

    requests 
  	| summarize 
        count(), 
        avg(Duration),
        percentiles(Duration, 5, 50, 95)
      by name

#### <a name="weighted-percentiles"></a>Váženého percentily

Použití funkcí váženého percentilu v případech, kdy má nebyly data předem agregované. 

Předpokládejme například, aplikace provádí mnoho tisíce operací za sekundu a budete chtít zjistit jejich latence. Jednoduché řešení bude generovat žádost přehledy aplikace nebo vlastní událost pro každou akci. To byste si vytvořili spoustu přenosy, sice adaptivní odběr by se projeví zmenšit ho. Ale pak se rozhodnete provádět ještě lepších řešení: budete psát některé kód v aplikaci agregovat data před odesláním interpretace aplikace. Souhrnné souhrn se budou posílat v pravidelných intervalech, možná snížení sazeb dat pro několik bodů za minutu.

Kód trvá toku rozměry zpoždění v milisekundách. Příklad:
    
     { 15, 12, 2, 21, 2, 5, 35, 7, 12, 22, 1, 15, 18, 12, 26, 7 }

Vrátí hodnoty v následujících intervalů:`{ 10, 20, 30, 40, 50, 100 }`

Pravidelně dává řadu TrackEvent hovory, jedno pro každý blok s vlastní rozměry v každé volání: 

    foreach (var latency in bins.Keys)
    { telemetry.TrackEvent("latency", null, 
         new Dictionary<string, double>
         ({"latency", latency}, {"opCount", bins[latency]}}); }

V analýzy uvidíte jedné tyto skupiny události takto:

`opCount` | `latency`| význam
---|---|---
8 | 10 | = 8 operace v koši 10 MS
6 | 20 | = 6 operace v koši 20ms
3 | 30 | = 3 operace v koši 30ms
1 | 40 | = 1 operace v koši 40ms

Přesné vzhledu původní výskyt události čekacích dob získáte používáme `percentilesw`:

    customEvents | summarize percentilesw(latency, opCount, 20, 50, 80)

Výsledky budou vráceny stejné jako kdyby použijeme prostý `percentiles` na původní nastavení měrné jednotky.

> [AZURE.NOTE] Váženého percentily není k dispozici [vzorky dat](app-insights-sampling.md), přičemž každý vybraných řádek představuje náhodně původní řádky, nikoli Koš. Funkce PERCENTIL jednoduchým jsou vhodné pro vybraných dat.

#### <a name="estimation-error-in-percentiles"></a>Chyba odhadu v percentily

Agregace percentily poskytuje přibližná hodnota pomocí [T Digest](https://github.com/tdunning/t-digest/blob/master/docs/t-digest-paper/histo.pdf). 

Několik důležitých aspektů: 

* Hranice na chyby odhadu lišit s hodnotou požadované percentilu. Změnit velikost, percentily 0 a 100 jsou přesné minimální a maximální hodnoty rozdělení, je nejlepší přesnost na konci [0..100]. Přesnost postupně snižuje směrem uprostřed měřítka. Je nejhorší na střední hodnotu a uzavřeny v 1 %. 
* Jsou hranice chybové pozorovanými na pořadí, ne na hodnotu. Předpokládejme percentil (X, 50) vrátí hodnotu Xm. Odhad zaručuje, že aspoň 49 % a ve většině 51 % hodnot X je menší než Xm. Na rozdíl mezi Xm a skutečné střední hodnota X je neomezeno teoretický.

### <a name="stdev"></a>SMODCH.výběr

     stdev(Expr)

Vrátí směrodatnou odchylku *výraz* přes skupiny.

### <a name="variance"></a>Odchylka

    variance(Expr)

Vrátí odchylku *výraz* přes skupiny.

### <a name="sum"></a>SUMA

    sum(Expr)

Vrátí součet druhých *výraz* přes skupiny.                      


## <a name="scalars"></a>Skalárů

[nádechy](#casts) | [porovnání](#scalar-comparisons)
<br/>
[GETTYPE](#gettype) | [hash](#hash) | [iff](#iff) |  [isnull](#isnull) | [isnotnull](#isnotnull) | [notnull](#notnull) | [toscalar](#toscalar)

Podporované typy jsou:

| Typ      | Další jména   | Odpovídající typu .NET |
| --------- | -------------------- | -------------------- |
| `bool`    | `boolean`            | `System.Boolean`     |
| `datetime`| `date`               | `System.DateTime`    |
| `dynamic` |                      | `System.Object`      |
| `guid`    | `uuid`, `uniqueid`   | `System.Guid`        |
| `int`     |                      | `System.Int32`       |
| `long`    |                      | `System.Int64`       |
| `double`  | `real`               | `System.Double`      |
| `string`  |                      | `System.String`      |
| `timespan`| `time`               | `System.TimeSpan`    |

### <a name="casts"></a>Nádechy

Z jednoho typu můžete převést na jiný. Obecně Pokud převod smysl, to funguje:

    todouble(10), todouble("10.6")
    toint(10.6) == 11
    floor(10.6) == 10
    toint("200")
    todatetime("2016-04-28 13:02")
    totimespan("1.5d"), totimespan("1.12:00:00")
    toguid("00000000-0000-0000-0000-000000000000")
    tostring(42.5)
    todynamic("{a:10, b:20}")

Zkontrolujte, zda může být řetězec převeden na určitý typ:

    iff(notnull(todouble(customDimensions.myValue)),
       ..., ...)

### <a name="scalar-comparisons"></a>Skalární porovnání

||
---|---
`<` |Méně
`<=`|Je menší nebo rovno
`>` |Větší
`>=`|Větší nebo rovno
`<>`|Není rovno
`!=`|Není rovno 
`in`| Levý operand je rovno některého z jeho prvků pravý operand je polem (dynamické).
`!in`| Pravý operand polem (dynamické) a levý operand není rovno jeho prvky.




### <a name="gettype"></a>GETTYPE

**Vrátí**

Řetězec vyjadřující nadřazený typ úložiště jeden argument. To je užitečné, pokud máte hodnoty identifikovat `dynamic`: v tomto případě `gettype()` odkryjete jak zakódovaný hodnotu.

**Příklady**

|||
---|---
`gettype("a")` |`"string" `
`gettype(111)` |`"long" `
`gettype(1==1)` |`"int8"`
`gettype(now())` |`"datetime" `
`gettype(1s)` |`"timespan" `
`gettype(parsejson('1'))` |`"int" `
`gettype(parsejson(' "abc" '))` |`"string" `
`gettype(parsejson(' {"abc":1} '))` |`"dictionary"` 
`gettype(parsejson(' [1, 2, 3] '))` |`"array"` 
`gettype(123.45)` |`"real" `
`gettype(guid(12e8b78d-55b4-46ae-b068-26d7a0080254))` |`"guid"` 
`gettype(parsejson(''))` |`"null"`
`gettype(1.2)==real` | `true`

### <a name="hash"></a>algoritmus hash

**Syntaxe**

    hash(source [, mod])

**Argumenty**

* *zdroje*: zdroji skalární algoritmus hash je vypočítána na.
* *MOD*: modulo hodnotu, která má být použity na výsledek hash.

**Vrátí**

Hodnota xxhash (dlouhé) dané skalární modulo hodnotu dané mod (Pokud je zadána).

**Příklady**

```
hash("World")                   // 1846988464401551951
hash("World", 100)              // 51 (1846988464401551951 % 100)
hash(datetime("2015-01-01"))    // 1380966698541616202
```
### <a name="iff"></a>IFF

`iff()` Funkce vyhodnotí prvním argumentu (predikát) a vrátí hodnotu druhé a třetí argumenty v závislosti na tom, jestli predikát `true` nebo `false`. Druhé a třetí argumenty musí být stejného typu.

**Syntaxe**

    iff(predicate, ifTrue, ifFalse)


**Argumenty**

* *predikát:* Výraz, který je vyhodnocen jako `boolean` hodnotu.
* *ifTrue:* Výraz, který se vyhodnotí a jeho hodnotu vrácených z funkce Pokud *predikát* vyhodnocen jako `true`.
* *ifFalse:* Výraz, který se vyhodnotí a jeho hodnotu vrácených z funkce Pokud *predikát* vyhodnocen jako `false`.

**Vrátí**

Tato funkce vrátí hodnotu *ifTrue* Pokud vyhodnocena jako *predikát* `true`, nebo hodnotu *ifFalse* jinak.

**Příklad**

```
iff(floor(timestamp, 1d)==floor(now(), 1d), "today", "anotherday")
```

<a name="isnull"/></a>
<a name="isnotnull"/></a>
<a name="notnull"/></a>
### <a name="isnull-isnotnull-notnull"></a>Funkce IsNull isnotnull, notnull

    isnull(parsejson("")) == true

Má jeden argument a určuje, zda je null.

**Syntaxe**


    isnull([value])


    isnotnull([value])


    notnull([value])  // alias for isnotnull

**Vrátí**

True nebo false v závislosti zda hodnotu null nebo není prázdné.


|x|IsNull(x)
|---|---
| "" | NEPRAVDA
|"x" | NEPRAVDA
|parsejson("")|PRAVDA
|parsejson("[]")|NEPRAVDA
|parsejson("{}")|NEPRAVDA

**Příklad**

    T | where isnotnull(PossiblyNull) | count

Všimněte si, že jsou další způsoby, jak dosáhnout tento efekt:

    T | summarize count(PossiblyNull)

### <a name="toscalar"></a>toscalar

Vyhodnotí dotazu nebo výraz a vrátí výsledek jako jednu hodnotu. Tato funkce je užitečné pro fázované výpočty; Příklad výpočtu celkový počet událostí a pak použitím jako Směrný plán.

**Syntaxe**

    toscalar(query)
    toscalar(scalar)

**Vrátí**

Argument vyhodnocené. Pokud je argument tabulka, vrátí první sloupec v prvním řádku. (Vhodné je zajistit, že má argument pouze jeden sloupec a řádek).

**Příklad**

```AIQL

    // Get the count of requests 5 days ago:
    let baseline = toscalar(requests  
        | where floor(timestamp, 1d) == floor(ago(5d),1d) | count);
    // List the counts relative to that baseline:
    requests | summarize daycount = count() by floor(timestamp, 1d)  
  	| extend relative = daycount - baseline
```




### <a name="boolean-literals"></a>Logická literálů

    true == 1
    false == 0
    gettype(true) == "int8"
    typeof(bool) == typeof(int8)

### <a name="boolean-operators"></a>Logické operátory

    and 
    or 

    

## <a name="numbers"></a>Čísla

[abs](#abs) | [bin](#bin) | [exp](#exp) | [floor](#floor) | [gamma](#gamma) |[log](#log) | [rand](#rand) | [range](#range) | [sqrt](#sqrt) 
| [todouble](#todouble) | [toint](#toint) | [tolong](#tolong)

### <a name="numeric-literals"></a>Číselné literálů

|||
|---|---
|`42`|`long`
|`42.0`|`real`

### <a name="arithmetic-operators"></a>Aritmetické operátory

|| |
|---|-------------|
| + | Přidání         |
| - | Odčítání    |
| * | Násobení    |
| / | Dělit      |
| % | Modulo      |
||
|`<` |Méně
|`<=`|Je menší nebo rovno
|`>` |Větší
|`>=`|Větší nebo rovno
|`<>`|Není rovno
|`!=`|Není rovno 


### <a name="abs"></a>Abs

**Syntaxe**

    abs(x)

**Argumenty**

* x - celé číslo, reálné nebo časového rozpětí

**Vrátí**

    iff(x>0, x, -x)

<a name="bin"></a><a name="floor"></a>
### <a name="bin-floor"></a>Koš, prostorového uspořádání

Zaokrouhlí číslo dolů na celé hodnoty násobek velikosti dané Koš. Použít mnohem v [`summarize by`](#summarize-operator) dotazu. Pokud máte roztroušených množiny hodnot, budou seskupené do něco míň určitých hodnot.

Alias `floor`.

**Syntaxe**

     bin(value, roundTo)
     floor(value, roundTo)

**Argumenty**

* *hodnota:* Číslo, datum nebo časový interval. 
* *roundTo:* "Koš velikost". Číslo, datum nebo časový interval, který *je dělitelem*. 

**Vrátí**

Na nejbližší násobek čísla *roundTo* nižší než *hodnota*.  
 
    (toint((value/roundTo)-0.5)) * roundTo

**Příklady**

Výraz | Výsledek
---|---
`bin(4.5, 1)` | `4.0`
`bin(time(16d), 7d)` | `14d`
`bin(datetime(1953-04-15 22:25:07), 1d)`|  `datetime(1953-04-15)`


Následující výraz vypočítá histogram doby trvání, s velikostí intervalu 1 druhé:

```AIQL

    T | summarize Hits=count() by bin(Duration, 1s)
```

### <a name="exp"></a>Exp

    exp(v)   // e raised to the power v
    exp2(v)  // 2 raised to the power v
    exp10(v) // 10 raised to the power v


### <a name="floor"></a>ZAOKR.dolů

Alias pro [`bin()`](#bin).

### <a name="gamma"></a>Funkce gamma

[Funkce gamma](https://en.wikipedia.org/wiki/Gamma_function)

**Syntaxe**

    gamma(x)

**Argumenty**

* *x:* Reálné číslo

Pro kladné celá čísla `gamma(x) == (x-1)!` například `gamma(5) == 4 * 3 * 2 * 1`.

Další informace najdete v článku [loggamma](#loggamma).


### <a name="log"></a>log

    log(v)    // Natural logarithm of v
    log2(v)   // Logarithm base 2 of v
    log10(v)  // Logarithm base 10 of v


`v`by měl být reálné číslo > 0. V ostatních případech bude vrácena hodnota null.

### <a name="loggamma"></a>loggamma


Přirozený logaritmus absolutní hodnotu [funkce gama rozdělení](#gamma).

**Syntaxe**

    loggamma(x)

**Argumenty**

* *x:* Reálné číslo


### <a name="rand"></a>NÁHČÍSLO

Generátor pseudonáhodných čísel.

* `rand()`-reálné číslo mezi 0,0 a 1.0
* `rand(n)`-celé číslo od 0 do n-1




### <a name="sqrt"></a>Odmocnina

Funkce druhou odmocninu.  

**Syntaxe**

    sqrt(x)

**Argumenty**

* *x:* Reálné číslo > = 0.

**Vrátí**

* Kladné číslo tak, aby`sqrt(x) * sqrt(x) == x`
* `null`Pokud argument záporný nebo nelze převést na `real` hodnotu. 




### <a name="toint"></a>toint

    toint(100)        // cast from long
    toint(20.7) == 21 // nearest int from double
    toint(20.4) == 20 // nearest int from double
    toint("  123  ")  // parse string
    toint(a[0])       // cast from dynamic
    toint(b.c)        // cast from dynamic

### <a name="tolong"></a>tolong

    tolong(20.7) == 21 // conversion from double
    tolong(20.4) == 20 // conversion from double
    tolong("  123  ")  // parse string
    tolong(a[0])       // cast from dynamic
    tolong(b.c)        // cast from dynamic


### <a name="todouble"></a>todouble

    todouble(20) == 20.0 // conversion from long or int
    todouble(" 12.34 ")  // parse string
    todouble(a[0])       // cast from dynamic
    todouble(b.c)        // cast from dynamic



## <a name="date-and-time"></a>Datum a čas


[před](#ago) | [dayofmonth](#dayofmonth) | [dayofweek](#dayofweek) |  [dayofyear](#dayofyear) |[datepart](#datepart) | [endofday](#endofday) | [endofmonth](#endofmonth) | [endofweek](#endofweek) | [endofyear](#endofyear) | [getmonth](#getmonth)|  [getyear](#getyear) | [teď](#now) | [startofday](#startofday) | [startofmonth](#startofmonth) | [startofweek](#startofweek) | [startofyear](#startofyear) | [todatetime](#todatetime) | [totimespan](#totimespan) | [weekofyear](#weekofyear)

### <a name="date-and-time-literals"></a>Datum a čas literálů

|||
---|---
**data a času**|
`datetime("2015-12-31 23:59:59.9")`<br/>`datetime("2015-12-31")`|Čas jsou vždy UTC. Vynechání datum zobrazí čas dnes.
`now()`|Aktuální čas.
`now(`-*časového rozpětí*`)`|`now()-`*časového rozpětí*
`ago(`*časového rozpětí*`)`|`now()-`*časového rozpětí*
**časového rozpětí**|
`2d`|dva dny
`1.5h`|1,5 hodinu 
`30m`|30 minut
`10s`|10 sekund
`0.1s`|0,1 druhé
`100ms`| 100 milisekund
`10microsecond`|
`1tick`|100 ns
`time("15 seconds")`|
`time("2")`| dva dny
`time("0.12:34:56.7")`|`0d+12h+34m+56.7s`

### <a name="date-and-time-expressions"></a>Výrazy datum a čas

Výraz |Výsledek
---|---
`datetime("2015-01-02") - datetime("2015-01-01")`| `1d`
`datetime("2015-01-01") + 1d`| `datetime("2015-01-02")`
`datetime("2015-01-01") - 1d`| `datetime("2014-12-31")`
`2h * 24` | `2d`
`2d` / `2h` | `24`
`datetime("2015-04-15T22:33") % 1d` | `timespan("22:33")`
`bin(datetime("2015-04-15T22:33"), 1d)` | `datetime("2015-04-15T00:00")`
||
`<` |Méně
`<=`|Je menší nebo rovno
`>` |Větší
`>=`|Větší nebo rovno
`<>`|Není rovno
`!=`|Není rovno 




### <a name="ago"></a>Před

Odečte zadaného časového rozpětí od aktuálního času UTC hodiny. Jako `now()`tuto funkci lze použít tisknutím ve výrazu a UTC Čas odkazovaného budou shodovat s pro všechny konkretizací.

**Syntaxe**

    ago(a_timespan)

**Argumenty**

* *a_timespan*: Interval odečíst od aktuální čas UTC (`now()`).

**Vrátí**

    now() - a_timespan

**Příklad**

Všechny řádky s časovým razítkem za poslední hodinu:

```AIQL

    T | where timestamp > ago(1h)
```

### <a name="datepart"></a>Funkce DatePart

    datepart("Day", datetime(2015-12-14)) == 14

Extrahuje zadanou část datum jako celé číslo.

**Syntaxe**

    datepart(part, datetime)

**Argumenty**

* `part:String`-{"Rok", "Měsíc", "Den", "Hodinu", "Minute", "Druhý", "Milisekund", "Microsecond", "Nanosecond"}
* `datetime`

**Vrátí**

Long představující zadanou část.


### <a name="dayofmonth"></a>dayofmonth

    dayofmonth(datetime("2016-05-15")) == 15 

Řadová číslovka den v měsíci.

**Syntaxe**

    dayofmonth(a_date)

**Argumenty**

* `a_date`: A `datetime`.


### <a name="dayofweek"></a>DayOfWeek

    dayofweek(datetime("2015-12-14")) == 1d  // Monday

Celé číslo počet dní od předchozí neděle, jako `timespan`.

**Syntaxe**

    dayofweek(a_date)

**Argumenty**

* `a_date`: A `datetime`.

**Vrátí**

`timespan` Od půlnoci na začátku předchozí neděle, zaokrouhleno dolů na celé číslo dnů.

**Příklady**

```AIQL
dayofweek(1947-11-29 10:00:05)  // time(6.00:00:00), indicating Saturday
dayofweek(1970-05-11)           // time(1.00:00:00), indicating Monday
```

### <a name="dayofyear"></a>DAYOFYEAR

    dayofyear(datetime("2016-05-31")) == 152 
    dayofyear(datetime("2016-01-01")) == 1 

Řadová číslovka dne v roce.

**Syntaxe**

    dayofyear(a_date)

**Argumenty**

* `a_date`: A `datetime`.

<a name="endofday"></a><a name="endofweek"></a><a name="endofmonth"></a><a name="endofyear"></a>
### <a name="endofday-endofweek-endofmonth-endofyear"></a>endofday endofweek, endofmonth, endofyear

    dt = datetime("2016-05-23 12:34")

    endofday(dt) == 2016-05-23T23:59:59.999
    endofweek(dt) == 2016-05-28T23:59:59.999 // Saturday
    endofmonth(dt) == 2016-05-31T23:59:59.999 
    endofyear(dt) == 2016-12-31T23:59:59.999 


### <a name="getmonth"></a>getMonth

Pokud potřebujete číslo měsíce (1 až 12) z datetime.

**Příklad**

    ... | extend month = getmonth(datetime(2015-10-12))

    --> month == 10

### <a name="getyear"></a>GetYear

Zobrazí rok z datetime.

**Příklad**

    ... | extend year = getyear(datetime(2015-10-12))

    --> year == 2015

### <a name="now"></a>Nyní

    now()
    now(-2d)

Aktuální čas UTC, pokud chcete posun podle zadaného časového rozpětí. Tuto funkci lze použít tisknutím ve výrazu a čas odkazovaného budou shodovat s pro všechny instance.

**Syntaxe**

    now([offset])

**Argumenty**

* *Posun:* A `timespan`, přidat k aktuální čas UTC. Výchozí: 0.

**Vrátí**

Aktuální čas UTC jako `datetime`.

    now() + offset

**Příklad**

Určuje interval od události označenými predikát:

```AIQL
T | where ... | extend Elapsed=now() - timestamp
```

<a name="startofday"></a><a name="startofweek"></a><a name="startofmonth"></a><a name="startofyear"></a>
### <a name="startofday-startofweek-startofmonth-startofyear"></a>startofday startofweek, startofmonth, startofyear

    date=datetime("2016-05-23 12:34:56")

    startofday(date) == datetime("2016-05-23")
    startofweek(date) == datetime("2016-05-22") // Sunday
    startofmonth(date) == datetime("2016-05-01")
    startofyear(date) == datetime("2016-01-01")



### <a name="todatetime"></a>ToDateTime

Alias `datetime()`.

     todatetime("2016-03-28")
     todatetime("03/28/2016")
     todatetime("2016-03-28 14:34:00")
     todatetime("03/28/2016 2:34pm")
     todatetime("2016-03-28T14:34.5Z")
     todatetime(a[0]) 
     todatetime(b.c) 

Zkontrolujte, jestli je řetězec platná kalendářní data:

     iff(notnull(todatetime(customDimensions.myDate)),
         ..., ...)


### <a name="totimespan"></a>ToTimeSpan

Alias `timespan()`.

    totimespan("21d")
    totimespan("21h")
    totimespan(request.duration)

### <a name="weekofyear"></a>weekofyear

    weekofyear(datetime("2016-05-14")) == 21
    weekofyear(datetime("2016-01-03")) == 1
    weekofyear(datetime("2016-12-31")) == 53

Celé číslo výsledek představuje číslo týdne podle formátu ISO 8601 standardní. První den týdne neděli označeným je první týden roku v týdnu, který obsahuje tento rok první čtvrtek. (Poslední dny v roce proto obsahovat některé dny v týdnu 1 následujícím roce, nebo první dny může obsahovat některé týdnu 52 nebo 53 předchozího roku)


## <a name="string"></a>Řetězec

[countof](#countof) | [extrahovat](#extract) | [extractjson](#extractjson)  | [isempty](#isempty) | [isnotempty](#isnotempty) | [notempty](#notempty) | [parseurl](#parseurl) | [Nahradit](#replace) | [rozdělení](#split) | [strcat](#strcat) | [strlen](#strlen) | [podřetězec](#substring) | [tolower](#tolower) | [tostring](#tostring) | [toupper](#toupper)


### <a name="string-literals"></a>Řetězcových

Pravidla jsou stejné jako v JavaScriptu.

Řetězce může být uzavřeny v jedné nebo znaky dvojitých uvozovek. 

Zpětné lomítko (`\`) slouží k řídicí znaky jako `\t` (karta) `\n` (nový řádek) a výskyty orámování znak uvozovky.

* `'this is a "string" literal in single \' quotes'`
* `"this is a 'string' literal in double \" quotes"`
* `@"C:\backslash\not\escaped\with @ prefix"`

### <a name="obfuscated-string-literals"></a>Zastaralý řetězcových

Zastaralý řetězcových jsou řetězce, které analýzy bude překrývat při výstupu řetězec (například při trasování). Proces předstírání nahradí všechny zastaralý znaky tak, že zahájení (`*`) znak.

K vytvoření zastaralý řetězcová konstanta, připojte `h` nebo "H". Příklad:

```
h'hello'
h@'world' 
h"hello"
```

### <a name="string-comparisons"></a>Porovnávání řetězců

Operátor|Popis|Malá a velká písmena|Příklad PRAVDA
---|---|---|---
`==`|Rovná se |Ano| `"aBc" == "aBc"`
`<>` `!=`|Není rovno|Ano| `"abc" <> "ABC"`
`=~`|Rovná se |Ne| `"abc" =~ "ABC"`
`!~`|Není rovno |Ne| `"aBc" !~ "xyz"`
`has`|Pravé-stranu straně (RHS) je celé termín v levou-stranu-strany|Ne| `"North America" has "america"`
`!has`|RHS není úplné termín v LHS|Ne|`"North America" !has "amer"` 
`hasprefix`|RHS je předponu termín v LHS|Ne|`"North America" hasprefix "ame"`
`!hasprefix`|RHS není předpony všechny termínu v LHS|Ne|`"North America" !hasprefix "mer"`
`hassuffix`|RHS je příponu termín v LHS|Ne|`"North America" hassuffix "rth"`
`!hassuffix`|RHS není příponu všechny termín v LHS|Ne|`"North America" !hassuffix "mer"`
`contains` | RHS nastane jako podřetězec LHS|Ne| `"FabriKam" contains "BRik"`
`!contains`| RHS nedochází v LHS|Ne| `"Fabrikam" !contains "xyz"`
`containscs` | RHS nastane jako podřetězec LHS|Ano| `"FabriKam" contains "Kam"`
`!containscs`| RHS nedochází v LHS|Ano| `"Fabrikam" !contains "Kam"`
`startswith`|RHS je počáteční podřetězec LHS.|Ne|`"Fabrikam" startswith "fab"`
`!startswith`|RHS není počáteční podřetězec LHS.|Ne|`"Fabrikam" !startswith "abr"`
`endswith`|RHS je terminálu podřetězec LHS.|Ne|`"Fabrikam" endswith "kam"`
`!endswith`|RHS není terminálu podřetězec LHS.|Ne|`"Fabrikam" !endswith "ka"`
`matches regex`|LHS obsahuje shodu RHS|Ano| `"Fabrikam" matches regex "b.*k"`
`in`|Rovná prvky|Ano|`"abc" in ("123", "345", "abc")`
`!in`|Nerovná se všechny prvky|Ano|`"bc" !in ("123", "345", "abc")`

Použití `has` nebo `in` Pokud jste testování přítomnost úplně lexikální termínu – to znamená, symbol nebo alfanumerické word ohraničená jiné než alfanumerické znaky nebo začátek nebo konec pole. `has`provádí rychleji než `contains`, `startswith` nebo `endswith`. První tyto dotazy pracovat rychleji:

    EventLog | where continent has "North" | count;
    EventLog | where continent contains "nor" | count





### <a name="countof"></a>countof

    countof("The cat sat on the mat", "at") == 3
    countof("The cat sat on the mat", @"\b.at\b", "regex") == 3

Vrátí počet výskytů podřetězec v řetězci. Prostý řetězců odpovídajících překrývat, Regex shody nepodporují.

**Syntaxe**

    countof(text, search [, kind])

**Argumenty**

* *text:* Řetězec.
* *hledání:* Prostý řetězec nebo regulárních výrazů podle uvnitř *textu*.
* *identifikovat:* `"normal"|"regex"` Výchozí `normal`. 

**Vrátí**

Počet s hledaným řetězcem můžete odpovídat v kontejneru. Prostý řetězců odpovídajících překrývat, Regex shody nepodporují.

**Příklady**

|||
|---|---
|`countof("aaa", "a")`| 3 
|`countof("aaaa", "aa")`| 3 (ne 2!)
|`countof("ababa", "ab", "normal")`| 2
|`countof("ababa", "aba")`| 2
|`countof("ababa", "aba", "regex")`| 1
|`countof("abcabc", "a.c", "regex")`| 2
    



### <a name="extract"></a>extrahování

    extract("x=([0-9.]+)", 1, "hello x=45.6|wo") == "45.6"

Pokud potřebujete shodu [pravidelných výraz](#regular-expressions) z textového řetězce. V případě potřeby klikněte převádí extrahované podřetězec na zvolený typ.

**Syntaxe**

    extract(regex, captureGroup, text [, typeLiteral])

**Argumenty**

* *regex:* [Regulárních výrazů](#regular-expressions).
* *captureGroup:* U kladné `int` konstanta označující skupině zachycení extrahovat. 0 je zkratka pro celou shodu, 1 pro hodnotu odpovídající první "(" okrouhlá závorka")" regulárních výrazů, 2 nebo více pro následující závorky.
* *text:* A `string` hledání.
* *typeLiteral:* Literál typu volitelné (například `typeof(long)`). Pokud je určené extrahované podřetězec se převede na tento typ. 

**Vrátí**

Pokud *regex* najde shodu v *textu*: podřetězec proti skupiny určené zachycení *captureGroup*, pokud chcete převést na *typeLiteral*.

Pokud je nedošlo ke shodě nebo převod typu neúspěšné: `null`. 

**Příklady**

Příklad řetězce `Trace` vyhledány definici `Duration`. Porovnat se převede na `real`, pak vynásobí konstanty čas (`1s`) tak, aby `Duration` je typu `timespan`. V tomto příkladu je rovno 123.45 sekund:

```AIQL
...
| extend Trace="A=1, B=2, Duration=123.45, ..."
| extend Duration = extract("Duration=([0-9.]+)", 1, Trace, typeof(real)) * time(1s) 
```

V tomto příkladu je shodný s obsahem `substring(Text, 2, 4)`:

```AIQL
extract("^.{2,2}(.{4,4})", 1, Text)
```

<a name="notempty"></a>
<a name="isnotempty"></a>
<a name="isempty"></a>
### <a name="isempty-isnotempty-notempty"></a>IsEmpty, isnotempty, notempty

    isempty("") == true

True, pokud je argument prázdný řetězec nebo je null.
Další informace najdete v článku [isnull](#isnull).


**Syntaxe**

    isempty([value])


    isnotempty([value])


    notempty([value]) // alias of isnotempty

**Vrátí**

Označuje, zda je argument prázdný řetězec nebo isnull.

|x|IsEmpty(x)
|---|---
| "" | PRAVDA
|"x" | NEPRAVDA
|parsejson("")|PRAVDA
|parsejson("[]")|NEPRAVDA
|parsejson("{}")|NEPRAVDA


**Příklad**


    T | where isempty(fieldName) | count


### <a name="parseurl"></a>parseurl

Rozdělte její část adresy URL.

**Syntaxe**

    parseurl(urlstring)

**Argumenty**

* *urlstring:* ADRESA URL.

**Vrátí**

Objekt obsahující části jako řetězce.

**Příklad**

    parseurl("http://user:pass@contoso.com/icecream/buy.aspx?a=1&b=2#tag")

    {
    "Scheme" : "http",
    "Host" : "contoso.com",
    "Port" : "80",
    "Path" : "/icecream/buy.aspx",
    "Username" : "user",
    "Password" : "pass",
    "Query Parameters" : {"a":"1","b":"2"},
    "Fragment" : "tag"
    }

### <a name="replace"></a>nahrazení

Nahraďte všechny shody regex jiným řetězcem.

**Syntaxe**

    replace(regex, rewrite, text)

**Argumenty**

* *regex:* [Regulárních výrazů](https://github.com/google/re2/wiki/Syntax) hledání *textu*. Může obsahovat zachycení skupin v "("závorky")". 
* *Revize:* Nahrazení regex žádné shody, které udělali *matchingRegex*. Použití `\0` odkázat na celé shody `\1` o skupině prvního snímku `\2` a tak dále skupin následující snímek.
* *text:* Řetězec.

**Vrátí**

*text* za nahrazení všechny shody *regex* hodnocení *Přepsat*. Odpovídá nepřekrývaly.

**Příklad**

Tento příkaz:

```AIQL
range x from 1 to 5 step 1
| extend str=strcat('Number is ', tostring(x))
| extend replaced=replace(@'is (\d+)', @'was: \1', str)
```

Obsahuje následující výsledky:

| x    | Funkce Str | nahradit|
|---|---|---|
| 1    | Číslo je 1.000000  | Číslo bylo: 1.000000|
| 2    | Číslo je 2.000000  | Číslo bylo: 2.000000|
| 3    | Číslo je 3.000000  | Číslo bylo: 3.000000|
| 4    | Číslo je 4.000000  | Číslo bylo: 4.000000|
| 5    | Číslo je 5.000000  | Číslo bylo: 5.000000|
 



### <a name="split"></a>rozdělení

    split("aaa_bbb_ccc", "_") == ["aaa","bbb","ccc"]

Rozdělení zadaného řetězce podle zadaného oddělovače a vrátí řetězců se dílčí conatined řetězce. Volitelně můžete být konkrétní podřetězec vrácené pokud existuje.

**Syntaxe**

    split(source, delimiter [, requestedIndex])

**Argumenty**

* *zdroje*: zdrojový řetězec, který bude obsahovat rozděleného podle zadaného oddělovač.
* *Oddělovač*: Oddělovač použitý k rozdělení řetězec zdroje.
* *requestedIndex*: volitelné indexem `int`. Pokud je určené pole vrácený řetězec bude obsahovat požadované podřetězec pokud existuje. 

**Vrátí**

Pole, která obsahuje podřetězec řetězec daného zdroje, které jsou odděleny dané oddělovač.

**Příklady**

```
split("aa_bb", "_")           // ["aa","bb"]
split("aaa_bbb_ccc", "_", 1)  // ["bbb"]
split("", "_")                // [""]
split("a__b")                 // ["a","","b"]
split("aabbcc", "bb")         // ["aa","cc"]
```




### <a name="strcat"></a>strcat

    strcat("hello", " ", "world")

Spojí mezi 1 až 16 argumenty, které musí být řetězce.

### <a name="strlen"></a>strlen

    strlen("hello") == 5

Délka řetězce.

### <a name="substring"></a>podřetězec

    substring("abcdefg", 1, 2) == "bc"

Extrahování podřetězec z daného zdroje řetězce počínaje dané index. V případě potřeby lze určit délku požadované podřetězec.

**Syntaxe**

    substring(source, startingIndex [, length])

**Argumenty**

* *Zdroj:* Zdrojový řetězec podřetězec budou odebrány.
* *startingIndex:* Od nuly znak pozice začátku požadované podřetězec.
* *Délka:* Volitelný parametr, který slouží k určení požadovaného počtu znaků v podřetězec. 

**Vrátí**

Podřetězec ze zadaného řetězce. Podřetězec začínající znakem startingIndex (od nuly) a pokud zadaný pokračuje na konec znaky řetězce nebo délka.

**Příklady**

```
substring("123456", 1)        // 23456
substring("123456", 2, 2)     // 34
substring("ABCD", 0, 2)       // AB
```

### <a name="tolower"></a>ToLower

    tolower("HELLO") == "hello"

Převede řetězci na malá písmena.

### <a name="toupper"></a>ToUpper

    toupper("hello") == "HELLO"

Převede řetězce na velká písmena.



### <a name="guids"></a>GUID

    guid(00000000-1111-2222-3333-055567f333de)


## <a name="arrays-objects-and-dynamic"></a>Matice, objekty a dynamické

[literály](#dynamic-literals) | [hlasováním](#casting-dynamic-objects) | [operátory](#operators) | [nechat klauzule](#dynamic-objects-in-let-clauses)
<br/>
[arraylength](#arraylength) | [extractjson](#extractjson) | [parsejson](#parsejson) | [oblast](#range) | [treepath](#treepath) | [todynamic](#todynamic) | [zip](#zip)


Tady je výsledek dotazu na výjimku přehledy aplikace. Hodnota v `details` je matici.

![](./media/app-insights-analytics-reference/310.png)

**Indexování:** Pole index a objekty jenom jako JavaScript:

    exceptions | take 1
  	| extend 
        line = details[0].parsedStack[0].line,
        stackdepth = arraylength(details[0].parsedStack)

* Ale použít `arraylength` a dalších funkcí analýzy (ne ".length"!)

**Práce** V některých případech je nutné odevzdat tedy prvek, který extrahuje z objektu, protože jeho typ může být různé. Například `summarize...to` potřebuje jenom konkrétní typ:

    exceptions 
  	| summarize count() 
      by toint(details[0].parsedStack[0].line)

    exceptions 
  	| summarize count() 
      by tostring(details[0].parsedStack[0].assembly)

**Literálů** Vytvoření explicitního matici nebo balík vlastností objektu, napište ho jako řetězce JSON a cast:

    todynamic('[{"x":"1", "y":"32"}, {"x":"6", "y":"44"}]')


**mvexpand:** Chcete-li roztažení grafu vlastnosti objektu do samostatných řádků, použijte mvexpand:

    exceptions | take 1 
  	| mvexpand details[0].parsedStack[0]


![](./media/app-insights-analytics-reference/410.png)


**treepath:** Najít všechny cesty v složité objektu:

    exceptions | take 1 | project timestamp, details 
  	| extend path = treepath(details) 
  	| mvexpand path


![](./media/app-insights-analytics-reference/420.png)

**buildschema:** Jak vyhledat minimální schéma, které přijímá všechny hodnoty výrazu v tabulce:

    exceptions | summarize buildschema(details)

Výsledek:

    { "`indexer`":
     {"id":"string",
       "parsedStack":
       { "`indexer`": 
         {  "level":"int",
            "assembly":"string",
            "fileName":"string",
            "method":"string",
            "line":"int"
         }},
      "outerId":"string",
      "message":"string",
      "type":"string",
      "rawStack":"string"
    }}

Všimněte si, že `indexer` se používá k označení, které byste měli použít číselné indexu. Pro toto schéma některé platné cesty by být (za předpokladu, že tyto indexy příkladu jsou v oblasti):

    details[0].parsedStack[2].level
    details[0].message
    arraylength(details)
    arraylength(details[0].parsedStack)



### <a name="array-and-object-literals"></a>Matice a objekt literálů

Pokud chcete vytvořit dynamické literál, použijte `parsejson` (alias `todynamic`) s JSON řetězec argument:

* `parsejson('[43, 21, 65]')`-matici čísel
* `parsejson('{"name":"Alan", "age":21, "address":{"street":432,"postcode":"JLK32P"}}')` 
* `parsejson('21')`-jednu hodnotu dynamické typu obsahující číslo
* `parsejson('"21"')`-jednu hodnotu dynamické typu obsahující řetězec

Poznámku, na rozdíl od JavaScript, JSON vyžaduje použití dvojité uvozovky (`"`) kolem řetězce. Je tedy obecně snadněji nabídka řetězec kódovaný JSON literály pomocí jednoho nabídek (`'`).

Tento příklad vytvoří dynamické hodnoty a potom pomocí jejích polí:

```

T
| extend person = parsejson('{"name":"Alan", "age":21, "address":{"street":432,"postcode":"JLK32P"}}')
| extend n = person.name, add = person.address.street
```


### <a name="dynamic-object-functions"></a>Funkce dynamické objektu

|||
|---|---|
| *hodnota* `in` *matice*| True, pokud je prvek *pole* , která == *hodnota*<br/>`where City in ('London', 'Paris', 'Rome')`
| *hodnota* `!in` *matice*| True, pokud se žádný prvek *pole* , která == *hodnota*
|[`arraylength(`pole`)`](#arraylength)| Hodnota Null, pokud není matice
|[`extractjson(`cesta, objekt`)`](#extractjson)|Použití cesta přejděte do objektu.
|[`parsejson(`zdroje`)`](#parsejson)| Změní řetězec formátu JSON dynamický objekt.
|[`range(`od kroku`)`](#range)| Pole hodnot
|[`mvexpand`listColumn](#mvexpand-operator) | Zreplikuje řádek pro každou hodnotu v seznamu v určité buňky.
|[`summarize buildschema(`sloupec`)`](#buildschema) |Odvozeny schéma typu ze sloupce
|[`summarize makelist(`sloupec`)`](#makelist)| Sloučí skupiny řádky a umístí hodnotami ve sloupci v matici.
|[`summarize makeset(`sloupec`)`](#makeset) | Sloučí skupiny řádky a umístí hodnotami ve sloupci maticových bez duplicity.

### <a name="dynamic-objects-in-let-clauses"></a>Dynamické objekty v upozorní věty


[Nechat klauzule](#let-clause) úložiště dynamické hodnoty jako řetězce, takže tyto dvě klauzule jsou odpovídající a obojí `parsejson` (nebo `todynamic`) před použitím:

    let list1 = '{"a" : "somevalue"}';
    let list2 = parsejson('{"a" : "somevalue"}');

    T | project parsejson(list1).a, parsejson(list2).a




### <a name="arraylength"></a>arraylength

Počet prvků dynamické matice.

**Syntaxe**

    arraylength(array)

**Argumenty**

* *pole:* A `dynamic` hodnotu.

**Vrátí**

Počet prvků v *matici*nebo `null` Pokud je *argument matice* není matici.

**Příklady**

```
arraylength(parsejson('[1, 2, 3, "four"]')) == 4
arraylength(parsejson('[8]')) == 1
arraylength(parsejson('[{}]')) == 1
arraylength(parsejson('[]')) == 0
arraylength(parsejson('{}')) == null
arraylength(parsejson('21')) == null
```



### <a name="extractjson"></a>extractjson

    extractjson("$.hosts[1].AvailableMB", EventText, typeof(int))

Pokud potřebujete zadaný element mimo JSON textu pomocí výrazu cesty. Pokud chcete převeďte extrahované řetězec určitý typ.


**Syntaxe**

```

    string extractjson(jsonPath, dataSource) 
    resulttype extractjson(jsonPath, dataSource, typeof(resulttype))
```


**Vrátí**

Tato funkce provede JsonPath dotazu na zdroj dat, která obsahuje platný řetězcový JSON, pokud chcete převádět daná hodnota na jiný typ v závislosti na třetí argument.



**Příklad**

Zápis [závorka] a tečkami jsou odpovídající:

    ... | extend AvailableMB = extractjson("$.hosts[1].AvailableMB", EventText, typeof(int)) | ...

    ... | extend AvailableMD = extractjson("$['hosts'][1]['AvailableMB']", EventText, typeof(int)) | ...



**Tipy pro výkon**

* Použití klauzule where před použitím`extractjson()`
* Zvažte použití regulárních výrazů shodu s [extrahovat](#extract) . Mohlo by umožnit spuštění velmi rychleji a je efektivní Pokud ve formátu JSON pochází ze šablony.
* Použití `parsejson()` v případě potřeby extrahovat víc než jednu hodnotu z ve formátu JSON.
* Zvažte, zda JSON analyzovat při požití deklarací typ sloupce dynamické.

### <a name="json-path-expressions"></a>Cesta JSON výrazů

|||
|---|---|
|`$`|Kořenový objekt|
|`@`|Aktuální objekt|
|`[0]`|Pole index|
|`.`nebo`[0]` | Podřízené|

*(Jsme není implementovat aktuálně zástupné znaky, rekurze, unie nebo výsečí.)*




### <a name="parsejson"></a>parsejson

Interpretovat `string` jako by měla [hodnotu JSON](http://json.org/)) a vrátí hodnotu jako `dynamic`. Je nadřazené pomocí `extractjson()` potřebujete-li extrahovat více než jeden prvek JSON složeného objektu.

**Syntaxe**

    parsejson(json)

**Argumenty**

* *json:* Dokument JSON.

**Vrátí**

Objekt typu `dynamic` nastavil *json*.

**Příklad**

V následujícím příkladu když `context_custom_metrics` je `string` , vypadá takhle: 

```
{"duration":{"value":118.0,"count":5.0,"min":100.0,"max":150.0,"stdDev":0.0,"sampledValue":118.0,"sum":118.0}}
```

Následující fragment načte přínosu a pak `duration` úsek v objektu a od že načte dvou slotech `duration.value` a  `duration.min` (`118.0` a `110.0`v).

```AIQL
T
| ...
| extend d=parsejson(context_custom_metrics) 
| extend duration_value=d.duration.value, duration_min=d["duration"]["min"]
```



### <a name="range"></a>oblast

`range()` Funkce (nezaměňujte s `range` operátor) generuje dynamické pole blokování řadě rovnoměrně z vnější řádkování.

**Syntaxe**

    range(start, stop, step)

**Argumenty**

* *start:* Hodnota první prvek výsledná matice. 
* *Zastavit:* Hodnota poslední prvek výsledná matice nebo nejmenší hodnotu, která je větší než poslední prvek výsledná matice v v rámci celé násobek *Krok* z nabídky *start*.
* *krok:* Rozdíl mezi dvěma po sobě jdoucí prvky pole.

**Příklady**

Následující příklad vrátí `[1, 4, 7]`:

```AIQL
range(1, 8, 3)
```

Následující příklad vrátí matici blokování všech pracovních dnů v roce 2015:

```AIQL

    range(datetime(2015-01-01), datetime(2015-12-31), 1d)
```

### <a name="todynamic"></a>todynamic

    todynamic('{"a":"a1", "b":["b1", "b2"]}')

Převede řetězec na hodnotu dynamické.

### <a name="treepath"></a>treepath

    treepath(dynamic_object)

Zobrazí všechny cesty výrazy identifikující listy dynamický objekt. 

**Vrátí**

Pole Cesta výrazů.

**Příklady**

    treepath(parsejson('{"a":"b", "c":123}')) 
    =>       ["['a']","['c']"]
    treepath(parsejson('{"prop1":[1,2,3,4], "prop2":"value2"}'))
    =>       ["['prop1']","['prop1'][0]","['prop2']"]
    treepath(parsejson('{"listProperty":[100,200,300,"abcde",{"x":"y"}]}'))
    =>       ["['listProperty']","['listProperty'][0]","['listProperty'][0]['x']"]

Poznámka: "[0]" označuje přítomnost maticových, ale není zadán indexu používaného určitou cestu.

### <a name="zip"></a>PSČ

    zip(list1, list2, ...)

Kombinuje množiny seznamů do jednoho seznamu záznamů.

* `list1...`: Seznam hodnot

**Příklady**

    zip(parsejson('[1,3,5]'), parsejson('[2,4,6]'))
    => [ [1,2], [3,4], [5,6] ]

    
    zip(parsejson('[1,3,5]'), parsejson('[2,4]'))
    => [ [1,2], [3,4], [5,null] ]


### <a name="names"></a>Názvy

Názvy lze až 1 024 znaků. Se rozlišují velká a může obsahovat písmena, číslice a podtržítka (`_`). 

Nabídka název pomocí ["... '] or [" ... "] zahrnout dalších znaků, nebo použijte klíčové slovo jako název. Příklad:

```AIQL

    requests | 
    summarize  ["distinct urls"] = dcount(name) // non-alphanumerics
    by  ['where'] = client_City, // using a keyword as a name
        ['outcome!'] = success // non-alphanumerics
```


|||
|---|---|
|["cestu\\file\n\'x\'"] | Použití \ řídicí znaky|
|["d-e.=/f#\n"] | |
|[@'path\file'] | Žádné únik - \ je literál typu|
|[@"\now& potom\"] | |
|[Pokud] | Použití klíčového slova jazyka jako název|

[AZURE.INCLUDE [app-insights-analytics-footer](../../includes/app-insights-analytics-footer.md)]


