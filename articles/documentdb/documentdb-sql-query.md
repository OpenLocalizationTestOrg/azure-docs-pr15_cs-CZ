<properties 
    pageTitle="Syntaxe jazyka SQL a SQL dotazu pro DocumentDB | Microsoft Azure" 
    description="Další informace o syntaxi jazyka SQL databáze koncepty a dotazy SQL DocumentDB NoSQL databáze. SQL lze použít jako což je dotazovací jazyk JSON v DocumentDB." 
    keywords="Syntaxe jazyka SQL, dotaz sql zobrazený, sql, json dotazovací jazyk, základní pojmy databáze a sql dotazy, agregační funkce"
    services="documentdb" 
    documentationCenter="" 
    authors="arramac" 
    manager="jhubbard" 
    editor="monicar"/>

<tags 
    ms.service="documentdb" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/27/2016" 
    ms.author="arramac"/>

# <a name="sql-query-and-sql-syntax-in-documentdb"></a>Dotaz SQL zobrazený a syntaxe jazyka SQL v DocumentDB
Microsoft Azure DocumentDB podporuje zjišťování dokumenty pomocí jazyka SQL (Structured Query Language) jako což je dotazovací jazyk JSON. DocumentDB nevyskytují opravdu schéma. Na základě zavázala JSON datového modelu přímo v rámci databázový stroj poskytuje automatické indexování JSON dokumenty bez nutnosti explicitního schématu nebo vystavením sekundární indexy. 

Při návrhu dotazovací jazyk pro DocumentDB bylo dva cíle myslet:

-   Místo inventing nový jazyk dotazu JSON, chtěli jsme podporují SQL. SQL je jedním z dotazu jazyků známé a Oblíbené. DocumentDB SQL poskytuje formální programovací model bohaté dotazů přes JSON dokumenty.
-   Jako JSON dokumentu databáze může provést JavaScript přímo v databázový stroj chtěli jsme programovací model jazyka JavaScript použít jako základ pro naše dotazovací jazyk. Tento kód SQL DocumentDB se zobrazuje v JavaScriptu na typ systému, vyhodnocení výrazu a vyvolání funkce. Tento v zapnout poskytuje natural programovací model pro relační projekce, hierarchické navigaci v rámci JSON dokumentů, vlastní spojení, prostorové dotazů a vyvolání funkce definované uživatelem (UDF) úplně psaných JavaScript, mimo jiné funkce. 

Jsme myslíte, že tyto funkce jsou klíčové ke snížení tření mezi aplikací a databáze a jsou důležité pro produktivita.

Doporučujeme Začínáme následující videa, kde Aravind Ramachandran zobrazuje dotazu funkce DocumentDB společnosti, tak návštěva našeho [Hřišť dotazu](http://www.documentdb.com/sql/demo), kde můžete vyzkoušet DocumentDB a spouštění dotazů SQL proti naše datovou sadu.

> [AZURE.VIDEO dataexposedqueryingdocumentdb]

Vraťte se na tento článek kde začneme bude s kurz dotaz SQL, který vás provede některé jednoduché JSON dokumenty a příkazů SQL.

## <a name="getting-started-with-sql-commands-in-documentdb"></a>Začínáme s příkazy SQL v DocumentDB
Zobrazíte DocumentDB SQL v práci Pojďme začínat pár jednoduchých JSON dokumenty a projděte si některé jednoduché dotazům. Zvažte tyto dva dokumenty JSON o dvě skupiny. Poznámka: s DocumentDB, není potřeba vytvářet schémat nebo vedlejší indexy explicitně. Jednoduše potřebujeme vložení JSON dokumentů do kolekce DocumentDB a následně dotazu. Tady máme jednoduché JSON dokumentů pro rodinu Andersen, nadřazených, dětí (a jejich domácích), adresa a registrační informace. Dokument obsahuje řetězce, čísla, logické hodnoty, polí a vnořených vlastnosti. 

**Dokument**  

    {
        "id": "AndersenFamily",
        "lastName": "Andersen",
        "parents": [
           { "firstName": "Thomas" },
           { "firstName": "Mary Kay"}
        ],
        "children": [
           {
               "firstName": "Henriette Thaulow", "gender": "female", "grade": 5,
               "pets": [{ "givenName": "Fluffy" }]
           }
        ],
        "address": { "state": "WA", "county": "King", "city": "seattle" },
        "creationDate": 1431620472,
        "isRegistered": true
    }


Tady je druhá dokument s jedním drobným rozdílem – `givenName` a `familyName` se používá místo `firstName` a `lastName`.

**Dokument**  

    {
        "id": "WakefieldFamily",
        "parents": [
            { "familyName": "Wakefield", "givenName": "Robin" },
            { "familyName": "Miller", "givenName": "Ben" }
        ],
        "children": [
            {
                "familyName": "Merriam", 
                "givenName": "Jesse", 
                "gender": "female", "grade": 1,
                "pets": [
                    { "givenName": "Goofy" },
                    { "givenName": "Shadow" }
                ]
            },
            { 
                "familyName": "Miller", 
                 "givenName": "Lisa", 
                 "gender": "female", 
                 "grade": 8 }
        ],
        "address": { "state": "NY", "county": "Manhattan", "city": "NY" },
        "creationDate": 1431620462,
        "isRegistered": false
    }



Teď Pojďme zkusit pár dotazy týkající se tato data bychom si vysvětlit některé klíčové aspekty DocumentDB SQL. Například následující dotaz vrátí dokumenty kde pole id odpovídá `AndersenFamily`. Protože se jedná `SELECT *`, výstup dotazu je dokončeno JSON dokument:

**Dotaz**

    SELECT * 
    FROM Families f 
    WHERE f.id = "AndersenFamily"

**Výsledky**

    [{
        "id": "AndersenFamily",
        "lastName": "Andersen",
        "parents": [
           { "firstName": "Thomas" },
           { "firstName": "Mary Kay"}
        ],
        "children": [
           {
               "firstName": "Henriette Thaulow", "gender": "female", "grade": 5,
               "pets": [{ "givenName": "Fluffy" }]
           }
        ],
        "address": { "state": "WA", "county": "King", "city": "seattle" },
        "creationDate": 1431620472,
        "isRegistered": true
    }]


Nyní zvažte velikost písmen, které je třeba přeformátovat výstupu JSON různé obrazce. Tento dotaz projekty nový objekt JSON s dvěma vybraných polí název a Město, když na adresu město má stejný název jako stavu. V tomto případě odpovídá "NY, NY".

**Dotaz**   

    SELECT {"Name":f.id, "City":f.address.city} AS Family 
    FROM Families f 
    WHERE f.address.city = f.address.state

**Výsledky**

    [{
        "Family": {
            "Name": "WakefieldFamily", 
            "City": "NY"
        }
    }]


Následující dotaz vrátí křestní jména dětí ve skupině shoduje s id `WakefieldFamily` seřazené podle města pobytu.

**Dotaz**

    SELECT c.givenName 
    FROM Families f 
    JOIN c IN f.children 
    WHERE f.id = 'WakefieldFamily'
    ORDER BY f.address.city ASC

**Výsledky**

    [
      { "givenName": "Jesse" }, 
      { "givenName": "Lisa"}
    ]


Rádi přitáhnout pozornost ke několik stojí za pozornost aspekty dotazovací jazyk DocumentDB prostřednictvím příklady, které jste viděli, pokud:  
 
-   Protože DocumentDB SQL označené jako pracuje na JSON hodnoty, zabývá strom ve tvaru entity místo řádků a sloupců. Proto jazyk umožňuje v nápovědě k uzly stromu libovolného hloubku, třeba `Node1.Node2.Node3…..Nodem`, podobně jako relační SQL odkazující na odkaz na dvou částí `<table>.<column>`.   
-   Jazyk structured query language pracuje s daty schématu menší. Proto systému typ potřebuje dynamicky vázat. Stejný výraz může vytvořit různé typy na různé dokumenty. Výsledek dotazu je platnou hodnotu JSON, ale není zaručena pevné schématu.  
-   DocumentDB podporuje pouze striktně JSON dokumenty. To znamená, že typ systému a výrazy jsou omezeny pouze řešit JSON typy. Získáte [JSON specifikace](http://www.json.org/) další podrobnosti.  
-   Kolekce DocumentDB je schématu Uvolnit kontejner JSON dokumentů. Vztahy v entit dat v rámci a mezi dokumenty v kolekci ukládány implicitně podle uzavření a ne podle primární klíč a cizí klíčové vztahy. Toto je důležitým aspektem jmění poukázání na základě spojení uvnitř dokumentu popsáno dále v tomto článku.

## <a name="documentdb-indexing"></a>Indexování DocumentDB

Před jsme získáte v syntaxi jazyka SQL DocumentDB je vhodné využít designu indexování v DocumentDB. 

Účelem indexy databáze je poskytovat dotazy v různých formulářů a obrazce s spotřebu minimální zdrojů (třeba vytížení procesoru a vstupní a výstupní) zároveň dobrý výkon a nízké latence. Volba správného indexu pro zahájení dotazu v databázi často vyžaduje mnoho plánování a hodnocení. Tento postup představuje ověřovací kód pro menší schématu databáze kam neodpovídá striktně schéma a vývoj rychle data. 

Proto při jsme navržený DocumentDB indexování podsystém, jsme nastavit následující cíle:

-   Index dokumenty bez nutnosti schématu: podsystém indexování vyžadují všechny informace o schématu nebo vytvořit žádný odhad o schématu dokumentů. 

-   Podpora pro efektivní, bohaté hierarchické a relačních dotazů: index podporuje dotazovací jazyk DocumentDB efektivní, včetně podpory pro průzkumy hierarchické a relační.

-   Podpora pro konzistentní dotazy in face of trvalý objemu zápisy: pro vysokou zápisu výkon úloh s konzistentní dotazy, index se aktualizuje postupně efektivní a online při trvalý objemu zápisy. Aktualizace konzistentní index je nezbytné k poskytovat dotazy na úrovni konzistence, ve kterém uživatel nakonfigurován služby dokumentů.

-   Podpora pro víceklientská: možnost rezervaci na základě modelu pro zásady správného řízení zdrojů mezi klienty, aktualizace rejstřík provádí v rámci rozpočtu systémové prostředky (procesoru paměti a vstupní a výstupní operace sekundu) přidělený na otevřené. 

-   Úložiště efektivity: náklady efektivity nároky úložiště na disku indexu po ohraničenou a předvídatelná. Důvodem je důležité DocumentDB umožňuje vývojářům dělat změny nákladů na základě mezi index nároky vzhledem k výkonu dotazu.  

Ukázky znázorňující, jak nakonfigurovat indexování zásady pro kolekci v nápovědě k [DocumentDB ukázky](https://github.com/Azure/azure-documentdb-net) na webu MSDN. Teď nastavíme na podrobné informace o syntaxi jazyka SQL DocumentDB.


## <a name="basics-of-a-documentdb-sql-query"></a>Základní informace o DocumentDB SQL dotazu
Každý dotaz obsahuje klauzuli SELECT a volitelné FROM a WHERE klauzulí za organizace pro standardy ANSI SQL. Obvykle pro každou dotazu na zdroj v klauzuli FROM výčtu. Klikněte na zdroj k načtení podmnožinu JSON dokumenty použije filtr v klauzuli WHERE. Nakonec klauzule SELECT slouží k projektu v seznamu vyberte požadované hodnoty JSON.
    
    SELECT [TOP <top_expression>] <select_list> 
    [FROM <from_specification>] 
    [WHERE <filter_condition>]
    [ORDER BY <sort_specification]    


## <a name="from-clause"></a>Klauzule FROM
`FROM <from_specification>` Klauzule je nepovinný krok není-li zdroj filtrované nebo plánované později v dotazu. Má tato klauzule účel určit zdroji dat na kterých musí dotaz fungovat. Běžně celé kolekce je zdrojem, ale jednu místo zadat podmnožinu kolekci. 

Dotaz jako `SELECT * FROM Families` značí, že v celé kolekci rodiny je zdrojem jakého umožňuje zobrazit výčet. Zvláštní identifikátor KOŘENOVÉ lze znázornit kolekci místo názvu kolekce. Následující seznam obsahuje pravidla, která se nevynucují na dotaz:

- V kolekci může být aliasu, například `SELECT f.id FROM Families AS f` nebo jednoduše `SELECT f.id FROM Families f`. Tady `f` odpovídá `Families`. `AS`je volitelné klíčové slovo pro alias identifikátor.

-   Poznámka: to jednou aliasu, původního zdroje nelze vázat. Například `SELECT Families.id FROM Families f` je syntakticky neplatný, protože již nelze přeložit identifikátor "Rodiny".

-   Všechny vlastnosti, které je potřeba odkazovat musí být plně kvalifikovaný. Při nepřítomnosti striktně schématu dodržování je nevynucují Chcete-li předejít nejednoznačné vazby. Proto `SELECT id FROM Families f` je syntakticky neplatný od vlastnost `id` není vazby.
    
### <a name="sub-documents"></a>Dílčí dokumentů
Zdroje můžete taky sníží menší části. Například pro výčet dílčí strom ve všech dokumentech, dílčí kořenové může stát zdroji, jak je vidět v následujícím příkladu.

**Dotaz**

    SELECT * 
    FROM Families.children

**Výsledky**  

    [
      [
        {
            "firstName": "Henriette Thaulow",
            "gender": "female",
            "grade": 5,
            "pets": [
              {
                  "givenName": "Fluffy"
              }
            ]
        }
      ],
      [
        {
            "familyName": "Merriam",
            "givenName": "Jesse",
            "gender": "female",
            "grade": 1
        },
        {
            "familyName": "Miller",
            "givenName": "Lisa",
            "gender": "female",
            "grade": 8
        }
      ]
    ]

Zatímco výše uvedený příklad používá maticových jako zdroj, může jako zdroj, který je obsah zobrazený v následujícím příkladu používají objektu. Libovolný platný JSON hodnotu (Nedefinováno), najdete ve zdroji se považuje za zahrnuty ve výsledku dotazu. Pokud nemáte některé rodiny `address.state` hodnotu, bude vyloučit ve výsledku dotazu.

**Dotaz**

    SELECT * 
    FROM Families.address.state

**Výsledky**

    [
      "WA", 
      "NY"
    ]


## <a name="where-clause"></a>Klauzule WHERE
Klauzule WHERE (**`WHERE <filter_condition>`**) je nepovinný krok. Určuje, že podmínky, kterých byly čerpány dokumenty JSON zdrojem splňovat, aby mohla být součástí výsledek. Všechny dokumenty JSON musí být zadané podmínky "true" považuje výsledku. Klauzule WHERE používanou vrstvy index s cílem určit absolutní nejmenší podmnožinu zdrojové dokumenty, které mohou být součástí výsledek. 

Následující dotaz žádosti dokumenty, které obsahují vlastnost název, jejichž hodnota je `AndersenFamily`. Každý jiný dokument, který nemá vlastnost název nebo kde hodnotu neodpovídá `AndersenFamily` je vyloučené. 

**Dotaz**

    SELECT f.address
    FROM Families f 
    WHERE f.id = "AndersenFamily"

**Výsledky**

    [{
      "address": {
        "state": "WA", 
        "county": "King", 
        "city": "seattle"
      }
    }]


V předchozím příkladu ukázal jednoduché rovnosti dotazu. DocumentDB SQL také podporují různé skalární výrazů. Nejčastěji používaná jsou binární a unární výrazů. Vlastnost odkazy ze zdrojového JSON objektu je také platné výrazů. 

Následující binární operátory připojení se aktuálně podporuje a lze použít v dotazech, jak je vidět v následujícím příkladu:  
<table>
<tr>
<td>Aritmetický</td> 
<td>+,-,*,/,%</td>
</tr>
<tr>
<td>Bitové</td>    
<td>|, &, ^, <<, >>, >>> (pravý shift nula výplně) </td>
</tr>
<tr>
<td>Logické</td>
<td>A, NEBO NE</td>
</tr>
<tr>
<td>Porovnání</td> 
<td>=, !=, &lt;, &gt;, &lt;=, &gt;=, <></td>
</tr>
<tr>
<td>Řetězec</td> 
<td>|| (zřetězení)</td>
</tr>
</table>  

Podívejme se na některé dotazů pomocí binární operátory.

    SELECT * 
    FROM Families.children[0] c
    WHERE c.grade % 2 = 1     -- matching grades == 5, 1
    
    SELECT * 
    FROM Families.children[0] c
    WHERE c.grade ^ 4 = 1    -- matching grades == 5
    
    SELECT *
    FROM Families.children[0] c
    WHERE c.grade >= 5     -- matching grades == 5


Unární operátory +,-, ~ není jsou taky podporované a mohou sloužit uvnitř dotazů, jak je vidět v následujícím příkladu:

    SELECT *
    FROM Families.children[0] c
    WHERE NOT(c.grade = 5)  -- matching grades == 1
    
    SELECT *
    FROM Families.children[0] c
    WHERE (-c.grade = -5)  -- matching grades == 5



Kromě binární a unární operátorů mohou také odkazy na vlastnosti. Například `SELECT * FROM Families f WHERE f.isRegistered` vrátí JSON dokument obsahující vlastnost `isRegistered` jejichž vlastnosti hodnota je rovno ve formátu JSON `true` hodnotu. Všechny ostatní hodnoty (NEPRAVDA, null, Nedefinováno, `<number>`, `<string>`, `<object>`, `<array>`atd) vede ve zdrojovém dokumentu vyloučené ze výsledek. 

### <a name="equality-and-comparison-operators"></a>Rovnosti a relační operátory
Následující tabulka zobrazuje výsledky porovnání rovnosti v jazyce SQL DocumentDB mezi libovolné dva typy JSON.
<table style = "width:300px">
   <tbody>
      <tr>
         <td valign="top">
            <strong>Místní verze</strong>
         </td>
         <td valign="top">
            <strong>Nedefinováno</strong>
         </td>
         <td valign="top">
            <strong>Hodnota Null</strong>
         </td>
         <td valign="top">
            <strong>Logická hodnota</strong>
         </td>
         <td valign="top">
            <strong>Číslo</strong>
         </td>
         <td valign="top">
            <strong>Řetězec</strong>
         </td>
         <td valign="top">
            <strong>Objekt</strong>
         </td>
         <td valign="top">
            <strong>Pole</strong>
         </td>
      </tr>
      <tr>
         <td valign="top">
            <strong>Nedefinováno<strong>
         </td>
         <td valign="top">
Nedefinováno </td>
         <td valign="top">
Nedefinováno </td>
         <td valign="top">
Nedefinováno </td>
         <td valign="top">
Nedefinováno </td>
         <td valign="top">
Nedefinováno </td>
         <td valign="top">
Nedefinováno </td>
         <td valign="top">
Nedefinováno </td>
      </tr>
      <tr>
         <td valign="top">
            <strong>Hodnota Null<strong>
         </td>
         <td valign="top">
Nedefinováno </td>
         <td valign="top">
            <strong>Ok</strong>
         </td>
         <td valign="top">
Nedefinováno </td>
         <td valign="top">
Nedefinováno </td>
         <td valign="top">
Nedefinováno </td>
         <td valign="top">
Nedefinováno </td>
         <td valign="top">
Nedefinováno </td>
      </tr>
      <tr>
         <td valign="top">
            <strong>Logická hodnota<strong>
         </td>
         <td valign="top">
Nedefinováno </td>
         <td valign="top">
Nedefinováno </td>
         <td valign="top">
            <strong>Ok</strong>
         </td>
         <td valign="top">
Nedefinováno </td>
         <td valign="top">
Nedefinováno </td>
         <td valign="top">
Nedefinováno </td>
         <td valign="top">
Nedefinováno </td>
      </tr>
      <tr>
         <td valign="top">
            <strong>Číslo<strong>
         </td>
         <td valign="top">
Nedefinováno </td>
         <td valign="top">
Nedefinováno </td>
         <td valign="top">
Nedefinováno </td>
         <td valign="top">
            <strong>Ok</strong>
         </td>
         <td valign="top">
Nedefinováno </td>
         <td valign="top">
Nedefinováno </td>
         <td valign="top">
Nedefinováno </td>
      </tr>
      <tr>
         <td valign="top">
            <strong>Řetězec<strong>
         </td>
         <td valign="top">
Nedefinováno </td>
         <td valign="top">
Nedefinováno </td>
         <td valign="top">
Nedefinováno </td>
         <td valign="top">
Nedefinováno </td>
         <td valign="top">
            <strong>Ok</strong>
         </td>
         <td valign="top">
Nedefinováno </td>
         <td valign="top">
Nedefinováno </td>
      </tr>
      <tr>
         <td valign="top">
            <strong>Objekt<strong>
         </td>
         <td valign="top">
Nedefinováno </td>
         <td valign="top">
Nedefinováno </td>
         <td valign="top">
Nedefinováno </td>
         <td valign="top">
Nedefinováno </td>
         <td valign="top">
Nedefinováno </td>
         <td valign="top">
            <strong>Ok</strong>
         </td>
         <td valign="top">
Nedefinováno </td>
      </tr>
      <tr>
         <td valign="top">
            <strong>Pole<strong>
         </td>
         <td valign="top">
Nedefinováno </td>
         <td valign="top">
Nedefinováno </td>
         <td valign="top">
Nedefinováno </td>
         <td valign="top">
Nedefinováno </td>
         <td valign="top">
Nedefinováno </td>
         <td valign="top">
Nedefinováno </td>
         <td valign="top">
            <strong>Ok</strong>
         </td>
      </tr>
   </tbody>
</table>

Pro jiné relační operátory, jako je třeba >, > =,! =, < a < = následující pravidla použít:   

-   Porovnání různých typů výsledkem Nedefinováno.
-   Porovnání dvou objektů nebo dvou maticových výsledkem Nedefinováno.   

Pokud je výsledek skalární výraz, který ve filtru Nedefinováno, odpovídající dokumentu by být zahrnuty ve výsledku, protože Nedefinováno nemá dotazovací logicky "true".

### <a name="between-keyword"></a>MEZI klíčové slovo
Můžete taky klíčového slova BETWEEN vyjádřit dotazů rozsahy hodnot jako v ANSI SQL. MEZI lze použít v řetězci nebo čísla.

Například tento dotaz vrací všechny řady dokumenty, ve kterých je první podsložky testu mezi 1-5 (obě včetně). 

    SELECT *
    FROM Families.children[0] c
    WHERE c.grade BETWEEN 1 AND 5

Na rozdíl od v ANSI SQL, můžete také klauzule BETWEEN v klauzuli FROM podobně jako v následujícím příkladu.

    SELECT (c.grade BETWEEN 0 AND 10)
    FROM Families.children[0] c

Zkrácení doby spuštění dotazu nezapomeňte k vytvoření indexování zásad, které používá formát oblasti index proti všechny číselné vlastnosti/cesty, filtrované v klauzuli BETWEEN. 

Hlavní rozdíl mezi používáním BETWEEN v DocumentDB a ANSI SQL je, že express oblast dotazů vlastnosti smíšených typů – například může být "testu" je číslo (5) v některé dokumenty a řetězce v jiných ("grade4"). V těchto případech podobně jako v JavaScript, porovnání mezi dvěma různými typy výsledky "Nedefinováno" a dokumentu budou vynechány.

### <a name="logical-and-or-and-not-operators"></a>Logická (a, nebo a ne) operátory
Logické operátory pracují logické hodnoty. V následujících tabulkách jsou zobrazeny logické pravdy tabulky pro tyto operátory.

NEBO|PRAVDA|NEPRAVDA|Nedefinováno
---|---|---|---
PRAVDA|PRAVDA|PRAVDA|PRAVDA
NEPRAVDA|PRAVDA|NEPRAVDA|Nedefinováno
Nedefinováno|PRAVDA|Nedefinováno|Nedefinováno

A|PRAVDA|NEPRAVDA|Nedefinováno
---|---|---|---
PRAVDA|PRAVDA|NEPRAVDA|Nedefinováno
NEPRAVDA|NEPRAVDA|NEPRAVDA|NEPRAVDA
Nedefinováno|Nedefinováno|NEPRAVDA|Nedefinováno

NOT|  |
---|---
PRAVDA|NEPRAVDA
NEPRAVDA|PRAVDA
Nedefinováno|Nedefinováno

### <a name="in-keyword"></a>U klíčových slov
Po klíčovém slovu IN lze použít ke kontrole, jestli zadanou hodnotu porovnává libovolnou hodnotu v seznamu. Například tento dotaz vrátí všechny řady dokumenty, kde id je jedním ze "WakefieldFamily" nebo "AndersenFamily". 
 
    SELECT *
    FROM Families 
    WHERE Families.id IN ('AndersenFamily', 'WakefieldFamily')

V tomto příkladu vrátí všechny dokumenty kde stav všech zadané hodnoty.

    SELECT *
    FROM Families 
    WHERE Families.address.state IN ("NY", "WA", "CA", "PA", "OH", "OR", "MI", "WI", "MN", "FL")

### <a name="ternary--and-coalesce--operators"></a>Unární (?) a operátorů v rámci slučovacího (?)
Unární a v rámci slučovacího operátory slouží k vytváření podmíněných výrazů, podobně jako oblíbené jazyky jako C# a JavaScript. 

Operátor unární (?) může být velmi užitečné při vytváření nové vlastnosti JSON rychlé úpravy. Například teď můžete psát dotazy ke klasifikaci třídy úrovně do oblasti lidských zdrojů čitelné jako Začátečník Intermediate/Upřesnit jak je ukázáno v následujícím příkladu.
 
     SELECT (c.grade < 5)? "elementary": "other" AS gradeLevel 
     FROM Families.children[0] c

Můžete také vnořit volání operátor like v dotazu dole.
 
    SELECT (c.grade < 5)? "elementary": ((c.grade < 9)? "junior": "high")  AS gradeLevel 
    FROM Families.children[0] c

Jako další operátory dotazu chybí odkazované vlastností v podmíněný výraz v libovolném otevřeném dokumentu, případně typy jsou ve srovnání se liší, potom tyto dokumenty budou vyloučeny ve výsledcích dotazu.

Operátor v rámci slučovacího (?) lze zjistit efektivní přítomnost vlastnost (také je definován) v dokumentu. To je užitečná, když dotazování částečně strukturovaná údaji smíšených typů. Tento dotaz například vrátí "Příjmení", pokud je k dispozici nebo "Příjmení", pokud není prezentovat.

    SELECT f.lastName ?? f.surname AS familyName
    FROM Families f

### <a name="quoted-property-accessor"></a>Přístupové uvozovkách vlastnost
Taky zpřístupníte vlastnosti pomocí operátoru uvozovkách vlastnost `[]`. Například `SELECT c.grade` a `SELECT c["grade"]` jsou ekvivalentní. Tuto syntaxi je užitečná, když potřebujete uniknout vlastnost, která obsahuje mezery, speciální znaky, nebo se stane s mít stejný název jako SQL klíčového slova nebo rezervované slovo.

    SELECT f["lastName"]
    FROM Families f
    WHERE f["id"] = "AndersenFamily"


## <a name="select-clause"></a>Klauzule SELECT
Klauzule SELECT (**`SELECT <select_list>`**) je povinný a určuje, co hodnoty budou načtená z kontingenčního seznamu dotaz, stejně jako v ANSI SQL. Podsady filtrována nad zdrojové dokumenty předávají na fáze projekce, kde zadaný JSON načítání a nový objekt JSON je vytvořen, pro každý vstup předaný na něj. 

Následující příklad ukazuje typické výběrového dotazu. 

**Dotaz**

    SELECT f.address
    FROM Families f 
    WHERE f.id = "AndersenFamily"

**Výsledky**

    [{
      "address": {
        "state": "WA", 
        "county": "King", 
        "city": "seattle"
      }
    }]


### <a name="nested-properties"></a>Vnořené vlastnosti
V následujícím příkladu budeme dvěma vnořených vlastnosti projekci `f.address.state` a `f.address.city`.

**Dotaz**

    SELECT f.address.state, f.address.city
    FROM Families f 
    WHERE f.id = "AndersenFamily"

**Výsledky**

    [{
      "state": "WA", 
      "city": "seattle"
    }]


Promítání taky podporuje JSON výrazy, jak je vidět v následujícím příkladu.

**Dotaz**

    SELECT { "state": f.address.state, "city": f.address.city, "name": f.id }
    FROM Families f 
    WHERE f.id = "AndersenFamily"

**Výsledky**

    [{
      "$1": {
        "state": "WA", 
        "city": "seattle", 
        "name": "AndersenFamily"
      }
    }]


Podívejme se na role `$1` tady. `SELECT` Klauzule je potřeba vytvořit objekt JSON a od není uvedený žádný klíč, používáme názvech proměnných implicitní argument počínaje `$1`. Například tento dotaz vrátí dvě proměnné implicitní argument s označením `$1` a `$2`.

**Dotaz**

    SELECT { "state": f.address.state, "city": f.address.city }, 
           { "name": f.id }
    FROM Families f 
    WHERE f.id = "AndersenFamily"

**Výsledky**

    [{
      "$1": {
        "state": "WA", 
        "city": "seattle"
      }, 
      "$2": {
        "name": "AndersenFamily"
      }
    }]


### <a name="aliasing"></a>Zástupný
Teď Pojďme rozšíření výše uvedeném příkladu s explicitní aliasu hodnot. Co je klíčového slova pro aliasu. Všimněte si, že je nepovinný krok, jak je znázorněno při projekci druhé hodnoty jako `NameInfo`. 

V případě, že dotaz má dvě vlastnosti se stejným názvem, třeba aliasu použít k přejmenování jednu nebo obě z vlastností tak, aby se jednoznačně rozlišit očekávaného výsledku.

**Dotaz**

    SELECT 
           { "state": f.address.state, "city": f.address.city } AS AddressInfo, 
           { "name": f.id } NameInfo
    FROM Families f 
    WHERE f.id = "AndersenFamily"

**Výsledky**

    [{
      "AddressInfo": {
        "state": "WA", 
        "city": "seattle"
      }, 
      "NameInfo": {
        "name": "AndersenFamily"
      }
    }]


### <a name="scalar-expressions"></a>Skalární výrazů
Kromě odkazů na vlastnost klauzule SELECT taky podporuje skalární výrazy jako konstanty aritmetické výrazů, logických výrazů, atd. Tady je příklad jednoduchého dotazu "Ahoj světe".

**Dotaz**

    SELECT "Hello World"

**Výsledky**

    [{
      "$1": "Hello World"
    }]


Tady je příklad složitější, který používá skalární výraz.

**Dotaz**

    SELECT ((2 + 11 % 7)-2)/3   

**Výsledky**

    [{
      "$1": 1.33333
    }]


V následujícím příkladu skalární výraz, který vyhodnotí jako logická hodnota.

**Dotaz**

    SELECT f.address.city = f.address.state AS AreFromSameCityState
    FROM Families f 

**Výsledky**

    [
      {
        "AreFromSameCityState": false
      }, 
      {
        "AreFromSameCityState": true
      }
    ]


### <a name="object-and-array-creation"></a>Vytvoření objektu a pole
Jiné klíčové funkce DocumentDB SQL je vytvoření pole nebo objektu. V předchozím příkladu Všimněte si, že jsme vytvořili nový objekt JSON. Podobně jednu můžete taky vytvořit matice jak je vidět v následujícím příkladu.

**Dotaz**

    SELECT [f.address.city, f.address.state] AS CityState 
    FROM Families f 

**Výsledky**  

    [
      {
        "CityState": [
          "seattle", 
          "WA"
        ]
      }, 
      {
        "CityState": [
          "NY", 
          "NY"
        ]
      }
    ]

### <a name="value-keyword"></a>Hodnota klíčového slova
**Hodnota** klíčového slova umožňuje vrátit hodnotu JSON. Například ukázáno v následujícím příkladu vracel skalární `"Hello World"` namísto `{$1: "Hello World"}`.

**Dotaz**

    SELECT VALUE "Hello World"

**Výsledky**

    [
      "Hello World"
    ]


Následující dotaz vrátí hodnotu JSON bez `"address"` popisek ve výsledcích.

**Dotaz**

    SELECT VALUE f.address
    FROM Families f 

**Výsledky**  

    [
      {
        "state": "WA", 
        "county": "King", 
        "city": "seattle"
      }, 
      {
        "state": "NY", 
        "county": "Manhattan", 
        "city": "NY"
      }
    ]

Následující příklad rozšiřuje to pro předvedení JSON základní hodnoty (úroveň listu stromu JSON). 

**Dotaz**

    SELECT VALUE f.address.state
    FROM Families f 

**Výsledky**

    [
      "WA",
      "NY"
    ]


###<a name="-operator"></a>* Operátor
Zvláštní operátor (*) je podporován do projektu dokument jako-je. Při použití, musí být jenom plánované pole. Při dotazu jako `SELECT * FROM Families f` je platný, `SELECT VALUE * FROM Families f ` a `SELECT *, f.id FROM Families f ` nejsou platné.

**Dotaz**

    SELECT * 
    FROM Families f 
    WHERE f.id = "AndersenFamily"

**Výsledky**

    [{
        "id": "AndersenFamily",
        "lastName": "Andersen",
        "parents": [
           { "firstName": "Thomas" },
           { "firstName": "Mary Kay"}
        ],
        "children": [
           {
               "firstName": "Henriette Thaulow", "gender": "female", "grade": 5,
               "pets": [{ "givenName": "Fluffy" }]
           }
        ],
        "address": { "state": "WA", "county": "King", "city": "seattle" },
        "creationDate": 1431620472,
        "isRegistered": true
    }]

###<a name="top-operator"></a>ZAČÁTEK operátor
ZAČÁTEK klíčové slovo mohou sloužit k omezení počtu hodnot z dotazu. Pokud horní se používá ve spojení s klauzule ORDER BY, sadu výsledků se omezí na první N počet uspořádaných hodnot. opačném případě vrátí první N počet výsledků v Nedefinováno pořadí. Osvědčený u příkazu SELECT vždy použijte klauzule ORDER s nejvyšší klauzulí. Toto je jediný způsob, jak předvídatelností určujících, které řádky jsou ovlivněny NAHOŘE. 


**Dotaz**

    SELECT TOP 1 * 
    FROM Families f 

**Výsledky**

    [{
        "id": "AndersenFamily",
        "lastName": "Andersen",
        "parents": [
           { "firstName": "Thomas" },
           { "firstName": "Mary Kay"}
        ],
        "children": [
           {
               "firstName": "Henriette Thaulow", "gender": "female", "grade": 5,
               "pets": [{ "givenName": "Fluffy" }]
           }
        ],
        "address": { "state": "WA", "county": "King", "city": "seattle" },
        "creationDate": 1431620472,
        "isRegistered": true
    }]

HORNÍ lze s konstanta (jak je znázorněno nad) nebo hodnotu proměnné pomocí parametry dotazů. Další informace najdete v článku parametry dotazů dole.

## <a name="order-by-clause"></a>Klauzule ORDER BY
Podobně jako v ANSI SQL, můžete zahrnout volitelná klauzule Order By při dotazu. Klauzule mohou obsahovat volitelný argument ASC/DESC určete pořadí, ve kterém nutné načíst výsledky. Podrobnější prohlédnout Order podívejte se do [DocumentDB pořadí podle návodu](documentdb-orderby.md).

Tady je příklad dotazu, která vrací rodiny v pořadí název rezidentní města.

**Dotaz**

    SELECT f.id, f.address.city
    FROM Families f 
    ORDER BY f.address.city
    
**Výsledky**
    
    [
      {
        "id": "WakefieldFamily",
        "city": "NY"
      },
      {
        "id": "AndersenFamily",
        "city": "Seattle"   
      }
    ]

A tady je dotaz, který načítá rodiny popořádku datum vytvoření, která je uložena jako číslo představující epocha času, tj, uplynulého času od 1 ledna 1970 v sekundách.

**Dotaz**

    SELECT f.id, f.creationDate
    FROM Families f 
    ORDER BY f.creationDate DESC
    
**Výsledky**
    
    [
      {
        "id": "WakefieldFamily",
        "creationDate": 1431620462
      },
      {
        "id": "AndersenFamily",
        "creationDate": 1431620472  
      }
    ]
    
## <a name="advanced-database-concepts-and-sql-queries"></a>Pokročilé databázové koncepty a dotazy SQL
### <a name="iteration"></a>Opakování
Nové konstrukce jsme přidali prostřednictvím po klíčovém slovu **IN** v jazyce SQL DocumentDB podporovat iterace JSON matice. Zdroji od podporuje iterace. Začněme v následujícím příkladu:

**Dotaz**

    SELECT * 
    FROM Families.children

**Výsledky**  

    [
      [
        {
          "firstName": "Henriette Thaulow", 
          "gender": "female", 
          "grade": 5, 
          "pets": [{ "givenName": "Fluffy"}]
        }
      ], 
      [
        {
            "familyName": "Merriam", 
            "givenName": "Jesse", 
            "gender": "female", 
            "grade": 1
        }, 
        {
            "familyName": "Miller", 
            "givenName": "Lisa", 
            "gender": "female", 
            "grade": 8
        }
      ]
    ]

Teď Pojďme se podívat na jiné dotazu, která provádí iterace přes dětí v kolekci. Všimněte si rozdílu výstupní pole. V tomto příkladu rozdělí `children` a sloučí výsledků do jednoho pole.  

**Dotaz**

    SELECT * 
    FROM c IN Families.children

**Výsledky**  

    [
      {
          "firstName": "Henriette Thaulow",
          "gender": "female",
          "grade": 5,
          "pets": [{ "givenName": "Fluffy" }]
      },
      {
          "familyName": "Merriam",
          "givenName": "Jesse",
          "gender": "female",
          "grade": 1
      },
      {
          "familyName": "Miller",
          "givenName": "Lisa",
          "gender": "female",
          "grade": 8
      }
    ]

Tato další slouží k filtrování na každou položku pole, jak je vidět v následujícím příkladu.

**Dotaz**

    SELECT c.givenName
    FROM c IN Families.children
    WHERE c.grade = 8

**Výsledky**  

    [{
      "givenName": "Lisa"
    }]

### <a name="joins"></a>Spojení
Relační databáze je třeba spojovat tabulky velmi důležité. Je logické přiznané k návrhu normalizovanou schémat. Na rozdíl od toho se zabývá DocumentDB nenormalizované datového modelu uvolnit schématu dokumentů. Toto je logické stejně jako v "vlastní spojení".

Syntaxe jazyka podporuje je < from_source1 > spojení < from_source2 > spojení... Připojit se ke < from_sourceN >. Celkově tento příkaz vrátí sadu **N**- n-ticové (n-tici s hodnotami **N** ). Každou n-tici obsahuje vytvořené pomocí všechny kolekce aliasy provádí iterace sady odpovídajících hodnot. Jinými slovy Toto je celé křížový součin sad účast ve spojení.

Následující příklady ukazují, jak lze použít klauzuli spojení. V následujícím příkladu výsledek prázdné od křížový součin každý dokument ze zdroje a sady vyprázdnit je prázdná.

**Dotaz**

    SELECT f.id
    FROM Families f
    JOIN f.NonExistent

**Výsledky**  

    [{
    }]


V následujícím příkladu je spojení mezi kontextový a `children` podsložek kořenové. Je křížový součin mezi dvěma JSON objekty. Skutečnost, že je děti maticových není efektivní ve spojení protože jsme se zabývají jedné kořenové, která je pole dětí. Proto výsledek obsahuje pouze dvěma výsledky, protože křížový součin každý dokument s polem dává přesně pouze jeden dokument.

**Dotaz**

    SELECT f.id
    FROM Families f
    JOIN f.children
 
**Výsledky**

    [
      {
        "id": "AndersenFamily"
      }, 
      {
        "id": "WakefieldFamily"
      }
    ]


Následující příklad ukazuje více běžných spojení:

**Dotaz**

    SELECT f.id
    FROM Families f
    JOIN c IN f.children 

**Výsledky**

    [
      {
        "id": "AndersenFamily"
      }, 
      {
        "id": "WakefieldFamily"
      }, 
      {
        "id": "WakefieldFamily"
      }
    ]



První věc, kterou je potřeba pamatovat se tedy `from_source` **připojení** klauzule je iterace. Ano, řízení toku v tomto případě je takto:  

-   Rozbalte každý podřízený prvek **c** v poli.
-   Použití křížový součin kořene dokument **f** každý podřízený prvek **c** , která byla sloučí v prvním kroku.
-   Nakonec projektu vlastnost název objektu **f** root samostatně. 

První dokument (`AndersenFamily`) obsahuje pouze jeden podřízený prvek, takže výsledná sada obsahuje pouze jeden objekt odpovídající tento dokument. Druhý dokument (`WakefieldFamily`) obsahuje dva dětí. Ano křížový součin vytvoří samostatný objekt pro každý podřízený, čímž výsledkem dvou objektů pro každý podřízený odpovídající tento dokument. Všimněte si, že budou kořenové polí v obou těchto dokumentů stejného, stejně jako byste čekali v křížový součin.

Nástroj skutečné spojení je n-ticové formuláře z křížový součin v obrazci, který je v opačném případě obtížné projektu. Kromě toho jsme bude vidět v následujícím příkladu, můžete vyfiltrovat v kombinaci řazené kolekce členů, že umožňuje uživateli rozhodli podmínky splnit n-ticové celkové.

**Dotaz**

    SELECT 
        f.id AS familyName,
        c.givenName AS childGivenName,
        c.firstName AS childFirstName,
        p.givenName AS petName 
    FROM Families f 
    JOIN c IN f.children 
    JOIN p IN c.pets
 
**Výsledky**

    [
      {
        "familyName": "AndersenFamily", 
        "childFirstName": "Henriette Thaulow", 
        "petName": "Fluffy"
      }, 
      {
        "familyName": "WakefieldFamily", 
        "childGivenName": "Jesse", 
        "petName": "Goofy"
      }, 
      {
       "familyName": "WakefieldFamily", 
       "childGivenName": "Jesse", 
       "petName": "Shadow"
      }
    ]



V tomto příkladu je přirozený rozšíření v předchozím příkladu a provede dvojité spojení. Ano křížový součin uvidí jako následující pseudo kód.

    for-each(Family f in Families)
    {   
        for-each(Child c in f.children)
        {
            for-each(Pet p in c.pets)
            {
                return (Tuple(f.id AS familyName, 
                  c.givenName AS childGivenName, 
                  c.firstName AS childFirstName,
                  p.givenName AS petName));
            }
        }
    }

`AndersenFamily`má jeden dítě, který obsahuje jeden pet. Ano, křížový součin výnosů jeden řádek (1*1*1) z této skupině. WakefieldFamily však má dvě dětí, ale jen jeden podřízený "Jesse" má domácích. Jesse má 2 domácích přes. Proto křížový součin výnosů 1*1*2 = 2 řádky z této skupině.

V následujícím příkladu je další filtr na `pet`. Nezahrnuje všech záznamů, kde název pet není "Stínů". Všimněte si, že jsme můžou vytvářet n-ticové z polí, filtru pro všechny prvky n-tici a project libovolnou kombinací prvky. 

**Dotaz**

    SELECT 
        f.id AS familyName,
        c.givenName AS childGivenName,
        c.firstName AS childFirstName,
        p.givenName AS petName 
    FROM Families f 
    JOIN c IN f.children 
    JOIN p IN c.pets
    WHERE p.givenName = "Shadow"

**Výsledky**

    [
      {
       "familyName": "WakefieldFamily", 
       "childGivenName": "Jesse", 
       "petName": "Shadow"
      }
    ]


## <a name="javascript-integration"></a>Integrace JavaScript
DocumentDB poskytuje programovací model pro provádění logických aplikace JavaScriptu na základě přímo na kolekce z hlediska uložené procedury a aktivace. Tato funkce umožňuje obě:

-   Možnost transakční operace CRUD vysoký výkon a dotazů dokumentům v kolekci na základě rozsáhlá integrace JavaScript runtime přímo v rámci databázový stroj. 
-   Přírodní modelování řízení toku, proměnné definování rozsahu a přiřazení a integrace zpracování základní s databázové transakce výjimek. Další informace o podpoře DocumentDB pro integraci JavaScript naleznete v JavaScriptu dokumentaci k serveru straně programovacích.

###<a name="user-defined-functions-udfs"></a>Funkce definované uživatelem
Spolu s typy definovaný v tomto článku DocumentDB SQL podporuje pro uživatele definované funkce (UDF). Skalární funkce definované uživatelem zejména podporuje místo, kam můžete vývojáři předat žádný nebo více argumentů a vrátí výsledek jeden argument zpět. Každá z těchto argumentů vyhledány jsou platné JSON hodnoty.  

Syntaxe jazyka SQL DocumentDB rozšířit o podporu logiku vlastních aplikací pomocí těchto uživatelsky definované funkce. Funkce definované uživatelem můžete registrovaný u DocumentDB a potom odkazovat jako součást dotaz SQL zobrazený. Ve skutečnosti funkce definované uživatelem exquisitely mají uplatnit pomocí dotazů. Funkce definované uživatelem jako přiznané na tento výběr, nemají přístup kontextu objekt, který mají další JavaScript typy (uložené procedury a aktivačních událostí). Protože dotazů provést jen pro čtení, můžete spustit na primární nebo na vedlejší repliky. Proto funkce definované uživatelem jsou určeny na vedlejší repliky na rozdíl od jiných typů JavaScript.

Tady je příklad jak můžou uživatelem definovanou FUNKCI registrovaná u DocumentDB databáze, konkrétně v rámci kolekce dokumentu.

   
       UserDefinedFunction regexMatchUdf = new UserDefinedFunction
       {
           Id = "REGEX_MATCH",
           Body = @"function (input, pattern) { 
                       return input.match(pattern) !== null;
                   };",
       };
       
       UserDefinedFunction createdUdf = client.CreateUserDefinedFunctionAsync(
           UriFactory.CreateDocumentCollectionUri("testdb", "families"), 
           regexMatchUdf).Result;  
                                                                             
Výše uvedený příklad vytvoří uživatelem definovanou FUNKCI, jejichž název je `REGEX_MATCH`. Přijme dvou řetězcových hodnot JSON `input` a `pattern` a kontroluje, zda první shody vzorce zadané ve druhém pomocí funkce string.match() jazyka JavaScript.


Můžete teď používáme tento UDF dotazu v projekci. Funkce definované uživatelem musí být určen s předponou velká a malá písmena "udf." Při volání z v rámci dotazů. 

>[AZURE.NOTE] Před 3/17/2015 DocumentDB podporuje volání UDF bez "udf." Předpona jako vyberte REGEX_MATCH(). Tento volání vzorek se nepoužívá.  

**Dotaz**

    SELECT udf.REGEX_MATCH(Families.address.city, ".*eattle")
    FROM Families

**Výsledky**

    [
      {
        "$1": true
      }, 
      {
        "$1": false
      }
    ]

UDF lze také uvnitř filtru uvedeno v příkladu níže, taky kvalifikované s "udf." prefix:

**Dotaz**

    SELECT Families.id, Families.address.city
    FROM Families
    WHERE udf.REGEX_MATCH(Families.address.city, ".*eattle")

**Výsledky**

    [{
        "id": "AndersenFamily",
        "city": "Seattle"
    }]


Funkce definované uživatelem v podstatě jsou platné skalární výrazy a lze použít v průzkumy a filtry. 

Rozbalte mocniny funkce definované uživatelem, Podívejme se na jiný příklad s podmíněným logických operátorů:

       UserDefinedFunction seaLevelUdf = new UserDefinedFunction()
       {
           Id = "SEALEVEL",
           Body = @"function(city) {
                switch (city) {
                    case 'seattle':
                        return 520;
                    case 'NY':
                        return 410;
                    case 'Chicago':
                        return 673;
                    default:
                        return -1;
                    }"
            };

            UserDefinedFunction createdUdf = await client.CreateUserDefinedFunctionAsync(
                UriFactory.CreateDocumentCollectionUri("testdb", "families"), 
                seaLevelUdf);
    
    
Tady je příklad vykonává souboru UDF.

**Dotaz**

    SELECT f.address.city, udf.SEALEVEL(f.address.city) AS seaLevel
    FROM Families f 

**Výsledky**

     [
      {
        "city": "seattle", 
        "seaLevel": 520
      }, 
      {
        "city": "NY", 
        "seaLevel": 410
      }
    ]


Jak předchozích příkladech prezentovat, funkce definované uživatelem power jazyka JavaScript integrovat SQL DocumentDB poskytnout bohaté programovatelnou rozhraní dělat komplexní postupu, podmíněné logiky pomocí funkcí runtime interních JavaScript.

DocumentDB SQL poskytuje argumenty funkce definované uživatelem pro každý dokument ve zdroji ve fázi aktuální (klauzuli WHERE nebo klauzule SELECT) zpracování souboru UDF. Výsledek je součástí celkové spouštěcím kanálu bezproblémové. Pokud vlastnosti uvedené ve souboru UDF nejsou k dispozici v hodnotě JSON parametry, se považuje za parametr Nedefinováno a tedy vyvolání UDF úplně vynechán. Podobně-li výsledek souboru UDF Nedefinováno, není součástí výsledek. 

Funkce definované uživatelem v souhrn, jsou skvělých nástrojů dělat složité obchodní logiky jako součást dotaz.

### <a name="operator-evaluation"></a>Operátor hodnocení
DocumentDB, důsledku se databázi JSON vykreslena rovnoběžkách s JavaScript operátory a jeho hodnocení sémantiku. Zatímco DocumentDB pokouší zachovat JavaScript sémantiku z hlediska JSON podpory, hodnocení operace odlišné v některých případech.

V DocumentDB SQL na rozdíl od ve tradiční SQL typy hodnot často není znám dokud hodnoty jsou ve skutečnosti načtená z kontingenčního seznamu databáze. Abyste mohli efektivně spouštění dotazů, většina operátorů má požadavky striktně typu. 

DocumentDB SQL nechová implicitním převodu na rozdíl od JavaScript. Například dotazu jako `SELECT * FROM Person p WHERE p.Age = 21` odpovídá dokumenty, které obsahují proceduru věk, jejichž hodnota je 21. Jako každý jiný dokument, jehož vlastnost stáří odpovídá případně nekonečné varianty řetězec "21" nebo jiné "021", "21.0", "0021", "00021", nebude odpovídat atd. Toto je naopak JavaScript, kde jsou hodnoty řetězce implicitně převedena na čísla (podle operátor: ==). Tato možnost je důležité pro efektivní index odpovídající v DocumentDB SQL. 

## <a name="parameterized-sql-queries"></a>Parametry dotazy SQL
DocumentDB podporuje dotazů s parametry vyjádřený se známými @ zápisu. Parametry SQL poskytuje robustní zpracování a úniku uživatele vstup, obrana před nechtěným zobrazení dat pomocí vkládáním příkazu SQL. 

Můžete například vytvořit dotaz, který používá jako parametry příjmení a adresa Stav a potom spustit pro různé hodnoty příjmení a adresa Stav na základě informací uživatele.

    SELECT * 
    FROM Families f
    WHERE f.lastName = @lastName AND f.address.state = @addressState

Tento požadavek můžou potom posílat DocumentDB jako parametry dotazu JSON jako je ukázáno v následujícím příkladu.

    {      
        "query": "SELECT * FROM Families f WHERE f.lastName = @lastName AND f.address.state = @addressState",     
        "parameters": [          
            {"name": "@lastName", "value": "Wakefield"},         
            {"name": "@addressState", "value": "NY"},           
        ] 
    }

Argument začátek lze nastavit pomocí parametry dotazů je ukázáno v následujícím příkladu.

    {      
        "query": "SELECT TOP @n * FROM Families",     
        "parameters": [          
            {"name": "@n", "value": 10},         
        ] 
    }

Hodnoty parametrů může být platná JSON (řetězce, čísla, logické hodnoty null, dokonce i matice nebo vnořené JSON). Také od DocumentDB je schématu menší, parametry nejsou ověřena libovolný typ.

##<a name="built-in-functions"></a>Předdefinované funkce
DocumentDB taky podporuje několik předdefinovaných funkcí pro společná operace, které lze použít v dotazech například funkce definované uživatelem (UDF).

<table>
<tr>
<td>Matematické funkce</td> 
<td>ABS CEILING, EXP, prostorového uspořádání, protokolu, LOG10, POWER, ZAOKROUHLIT, znak, odmocnina, ČTVEREC, USEKNOUT, ARCCOS, ARCSIN, ARCTG, ATN2, COS, COT STUPŇŮ, PÍ, RADIÁNECH, SIN a TAN</td>
</tr>
<tr>
<td>Typ Kontrola funkcí</td>    
<td>IS_ARRAY IS_BOOL, IS_NULL, IS_NUMBER, IS_OBJECT, IS_STRING, IS_DEFINED a IS_PRIMITIVE</td>
</tr>
<tr>
<td>Funkce String</td>   
<td>PROPOJIT, obsahuje, ENDSWITH, INDEX_OF, vlevo, délka, nižší, funkce LTRIM, nahradit, REPLIKACE, obrácené pořadí, zprava, RTRIM, STARTSWITH, PODŘETĚZEC a HORNÍM</td>
</tr>
<tr>
<td>Funkce Array</td>    
<td>ARRAY_CONCAT, ARRAY_CONTAINS, ARRAY_LENGTH a ARRAY_SLICE</td>
</tr>
<tr>
<td>Prostorové funkce</td>  
<td>ST_DISTANCE, ST_WITHIN, ST_ISVALID a ST_ISVALIDDETAILED</td>
</tr>
</table>  

Pokud aktuálně používáte funkce definované uživatelem (UDF) jehož předdefinované funkce je teď k dispozici, používejte odpovídající předdefinované funkce jako ho bude rychlejší spustit a mnohem efektivněji. 

### <a name="mathematical-functions"></a>Matematické funkce
Matematické funkce provádět výpočty, obvykle na základě zadávání hodnot, které jsou k dispozici jako argumenty a vrácení číselné hodnoty. Tady je tabulka podporovaných předdefinované matematických funkcí.

<table>
<tr>
<td><strong>Použití</strong></td>
<td><strong>Popis</strong></td>
</tr>
<tr>
<td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_abs">ABS (num_expr)</a></td> 
<td>Vrátí absolutní hodnotu (kladné hodnoty) zadaný číselný výraz.</td>
</tr>
<tr>
<td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_ceiling">ZAOKR.nahoru (num_expr)</a></td> 
<td>Vrátí nejmenší hodnotu celé číslo větší než nebo rovno zadaný číselný výraz.</td>
</tr>
<tr>
<td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_floor">ZAOKR.dolů (num_expr)</a></td> 
<td>Vrátí největší celé číslo menší nebo rovna zadaný číselný výraz.</td>
</tr>
<tr>
<td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_exp">EXP (num_expr)</a></td> 
<td>Vrátí exponent zadaný číselný výraz.</td>
</tr>
<tr>
<td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_log">LOG (num_expr [, base])</a></td> 
<td>Vrátí přirozený logaritmus zadaný číselný výraz nebo logaritmu pomocí zadaném základu.</td>
</tr>
<tr>
<td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_log10">LOG10 (num_expr)</a></td> 
<td>Vrátí hodnotu logaritmické základem 10 zadaný číselný výraz.</td>
</tr>
<tr>
<td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_round">ZAOKROUHLIT (num_expr)</a></td> 
<td>Vrátí číselnou hodnotu, zaokrouhleno na nejbližší celočíselnou hodnotu.</td>
</tr>
<tr>
<td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_trunc">USEKNOUT (num_expr)</a></td> 
<td>Vrátí číselnou hodnotu zkrácený na nejbližší celočíselnou hodnotu.</td>
</tr>
<tr>
<td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_sqrt">Odmocnina (num_expr)</a></td>   
<td>Vrátí druhou odmocninu zadaný číselný výraz.</td>
</tr>
<tr>
<td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_square">ČTVEREC (num_expr)</a></td>   
<td>Vrátí druhou mocninu zadaný číselný výraz.</td>
</tr>
<tr>
<td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_power">POWER (num_expr, num_expr)</a></td>   
<td>Vrátí power zadaný číselný výraz určený hodnotou.</td>
</tr>
<tr>
<td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_sign">ZNAMÉNKO (num_expr)</a></td>   
<td>Vrátí znak hodnotu (-1, 0, 1) zadaný číselný výraz.</td>
</tr>
<tr>
<tr>
<td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_acos">ARCCOS (num_expr)</a></td>   
<td>Vrátí úhel, v radiánech, jehož kosinus je zadaný číselný výraz. taky se mu říká arkuskosinus čísla.</td>
</tr>
<tr>
<td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_asin">ARCSIN (num_expr)</a></td>   
<td>Vrátí úhel, v radiánech, jehož sinus je zadaný číselný výraz. Tato funkce nazývá arkussinus čísla.</td>
</tr>
<tr>
<td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_atan">ARCTG (num_expr)</a></td>   
<td>Vrátí úhel, v radiánech, jehož tangens je zadaný číselný výraz. Tato funkce nazývá arkustangens.</td>
</tr>
<tr>
<td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_atn2">ATN2 (num_expr)</a></td>   
<td>Vrátí úhel, v radiánech, mezi kladné osy x a s paprsky z původ čárky (y; x), kde x a y jsou hodnoty ze dvou výrazů zadaný plovoucí.</td>
</tr>
<tr>
<td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_cos">COS (num_expr)</a></td> 
<td>Vrátí trigonometrické kosinus úhlu zadaného v radiánech v zadaný výraz.</td>
</tr>
<tr>
<td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_cot">COT (num_expr)</a></td> 
<td>Vrátí trigonometrické kotangens úhlu zadaného v radiánech v zadaném číselný výraz.</td>
</tr>
<tr>
<td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_degrees">DEGREES (num_expr)</a></td> 
<td>Vrátí odpovídající úhel ve stupních úhlu zadaného v radiánech.</td>
</tr>
<tr>
<td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_pi">PI)</a></td>   
<td>Vrátí hodnotu konstanty pí.</td>
</tr>
<tr>
<td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_radians">RADIANS (num_expr)</a></td> 
<td>Vrátí radiánech při zadávání číselný výraz, ve stupních.</td>
</tr>
<tr>
<td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_sin">SIN (num_expr)</a></td> 
<td>Vrátí trigonometrické sinus úhlu zadaného v radiánech v zadaný výraz.</td>
</tr>
<tr>
<td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_tan">Funkce TAN (num_expr)</a></td> 
<td>Vrátí tangens vstupní výrazu v zadaný výraz.</td>
</tr>

</table> 

Například můžete teď spouštění dotazů takto:

**Dotaz**

    SELECT VALUE ABS(-4)

**Výsledky**

    [4]

Hlavní rozdíl mezi jeho DocumentDB funkcí ve srovnání s ANSI SQL je, že jsou navržený pro práci s data bez schématu a smíšenými schémat. Pokud máte otevřený dokument, kde vlastnost velikost chybí, nebo není číselná hodnota, například "Neznámý stav" pak dokument vynechán, místo vracející chybu.

### <a name="type-checking-functions"></a>Typ Kontrola funkcí
Funkce kontroly typu umožňují kontrolovat typ výrazu v dotazech SQL. Funkce kontroly typu lze zjistit typ vlastnosti v dokumentech rychlé úpravy, až bude proměnná, nebo neznámý. Tady je tabulka podporované předdefinované typu Kontrola funkcí.

<table>
<tr>
  <td><strong>Použití</strong></td>
  <td><strong>Popis</strong></td>
</tr>
<tr>
  <td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_is_array">IS_ARRAY (výraz)</a></td>
  <td>Vrátí logickou hodnotu označující, pokud je typ hodnoty matici.</td>
</tr>
<tr>
  <td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_is_bool">IS_BOOL (výraz)</a></td>
  <td>Vrátí logickou hodnotu označující, pokud typ hodnoty je logická hodnota.</td>
</tr>
<tr>
  <td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_is_null">IS_NULL (výraz)</a></td>
  <td>Vrátí logickou hodnotu označující, pokud je typ hodnoty null.</td>
</tr>
<tr>
  <td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_is_number">IS_NUMBER (výraz)</a></td>
  <td>Vrátí logickou hodnotu označující, pokud je typu hodnotu čísla.</td>
</tr>
<tr>
  <td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_is_object">IS_OBJECT (výraz)</a></td>
  <td>Vrátí logickou hodnotu označující, pokud typ hodnoty je objekt JSON.</td>
</tr>
<tr>
  <td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_is_string">IS_STRING (výraz)</a></td>
  <td>Vrátí logickou hodnotu označující, pokud je typ hodnoty řetězce.</td>
</tr>
<tr>
  <td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_is_defined">IS_DEFINED (výraz)</a></td>
  <td>Vrátí logickou hodnotu označující, pokud je vlastnost přiřadila hodnotu.</td>
</tr>
<tr>
  <td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_is_primitive">IS_PRIMITIVE (výraz)</a></td>
  <td>Vrátí logickou hodnotu označující, pokud je typ hodnoty řetězce, číslo, logické hodnoty nebo hodnota null.</td>
</tr>

</table>

Pomocí těchto funkcí, můžete teď spouštění dotazů takto:

**Dotaz**

    SELECT VALUE IS_NUMBER(-4)

**Výsledky**

    [true]

### <a name="string-functions"></a>Funkce String
Následující skalární funkce provést operaci vstupní hodnotu řetězce a vrátí řetězec, číselné nebo logické hodnoty. Tady je tabulka předdefinované řetězcových funkcí:

Použití|Popis
---|---
[Délka (str_expr)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_length)|Vrátí počet znaků zadaný řetězcový výraz
[PROPOJIT (str_expr, str_expr [, str_expr])](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_concat)|Vrátí řetězec, který je výsledkem zřetězení dvou nebo více řetězcových hodnot.
[PODŘETĚZEC (str_expr, num_expr num_expr.)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_substring)|Vrátí část řetězcový výraz.
[STARTSWITH (str_expr, str_expr)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_startswith)|Vrátí hodnotu typu Boolean označující zda první řetězcový výraz má na konci druhé
[ENDSWITH (str_expr, str_expr)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_endswith)|Vrátí hodnotu typu Boolean označující zda první řetězcový výraz má na konci druhé
[OBSAHUJE (str_expr, str_expr)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_contains)|Vrátí hodnotu typu Boolean označující zda první řetězcový výraz obsahuje druhé.
[INDEX_OF (str_expr, str_expr)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_index_of)|Vrátí počáteční pozici prvního výskytu druhého řetězcový výraz v prvním zadaný řetězcový výraz nebo -1, pokud tento řetězec není nalezen.
[LEFT (str_expr, num_expr)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_left)|Vrátí do levé části řetězec s zadaný počet znaků.
[RIGHT (str_expr, num_expr)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_right)|Vrátí pravé části řetězec s zadaný počet znaků.
[Funkce LTRIM (str_expr)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_ltrim)|Vrátí řetězcový výraz po odebere počátečními nulami.
[Funkce RTRIM (str_expr)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_rtrim)|Vrátí řetězcový výraz po zkrácení všechny koncové mezery.
[NIŽŠÍ (str_expr)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_lower)|Vrátí řetězcový výraz po převedení dat velká znak na malá písmena.
[VELKÁ (str_expr)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_upper)|Vrátí řetězcový výraz po převedení dat malé písmeno na velká písmena.
[NAHRAZENÍ (str_expr, str_expr str_expr)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_replace)|Nahradí všechny výskyty zadaný řetězec s jinou hodnotu řetězce.
[REPLIKACE (str_expr, num_expr)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_replicate)|Zopakuje řetězcovou hodnotu zadaného počtu opakování.
[Obrácené pořadí (str_expr)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_reverse)|Vrátí obráceném pořadí hodnotu řetězce.

Pomocí těchto funkcí, můžete teď spouštění dotazů například následující. Například se můžete vrátit příjmení velkými písmeny následujícím způsobem:

**Dotaz**

    SELECT VALUE UPPER(Families.id)
    FROM Families

**Výsledky**

    [
        "WAKEFIELDFAMILY", 
        "ANDERSENFAMILY"
    ]

Nebo zřetězení řetězců podobně jako v tomto příkladu:

**Dotaz**

    SELECT Families.id, CONCAT(Families.address.city, ",", Families.address.state) AS location
    FROM Families

**Výsledky**

    [{
      "id": "WakefieldFamily",
      "location": "NY,NY"
    },
    {
      "id": "AndersenFamily",
      "location": "seattle,WA"
    }]


Řetězcových funkcích lze také použít v klauzuli WHERE k filtrování výsledků, podobně jako v následujícím příkladu:

**Dotaz**

    SELECT Families.id, Families.address.city
    FROM Families
    WHERE STARTSWITH(Families.id, "Wakefield")

**Výsledky**

    [{
      "id": "WakefieldFamily",
      "city": "NY"
    }]

### <a name="array-functions"></a>Funkce Array
Následující skalární funkce provést operaci vstupní hodnoty pole a vrácení číselné, hodnota logická hodnota nebo matici. Tady je tabulka předdefinované maticových funkcí:

Použití|Popis
---|---
[ARRAY_LENGTH (arr_expr)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_array_length)|Vrátí počet prvků zadaný matice výraz.
[ARRAY_CONCAT (arr_expr arr_expr [, arr_expr])](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_array_concat)|Vrátí matici, která je výsledkem zřetězení dvou nebo více hodnot pole.
[ARRAY_CONTAINS (arr_expr; výraz)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_array_contains)|Vrátí logickou hodnotu označující, zda pole obsahuje určité hodnoty.
[ARRAY_SLICE (arr_expr num_expr [, num_expr])](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_array_slice)|Vrátí část matice výraz.

Funkce Array mohou sloužit k manipulaci s nimi matice v rámci JSON. Tady je příklad dotazu, který vrací všechny dokumenty kde jednoho z rodičů "Petra Wakefield". 

**Dotaz**

    SELECT Families.id 
    FROM Families 
    WHERE ARRAY_CONTAINS(Families.parents, { givenName: "Robin", familyName: "Wakefield" })

**Výsledky**

    [{
      "id": "WakefieldFamily"
    }]

Tady máme jiný příklad, který používá ARRAY_LENGTH k získání počtu dětí za řady.

**Dotaz**

    SELECT Families.id, ARRAY_LENGTH(Families.children) AS numberOfChildren
    FROM Families 

**Výsledky**

    [{
      "id": "WakefieldFamily",
      "numberOfChildren": 2
    },
    {
      "id": "AndersenFamily",
      "numberOfChildren": 1
    }]

### <a name="spatial-functions"></a>Prostorové funkce

DocumentDB geografická dotazování podporuje následující předdefinované funkce Otevřít geografická Consortium (OGC). Podrobné informace o geografická podpory pro DocumentDB, najdete v tématu [práce s geografická data v Azure DocumentDB](documentdb-geospatial.md). 

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

Prostorové funkcí lze provádět blízkost aplikace proti prostorových dat. Tady je příklad dotazu, která vrací všechny řady dokumenty, které spadají do 30 km zadaného umístění pomocí předdefinované funkce ST_DISTANCE. 

**Dotaz**

    SELECT f.id 
    FROM Families f 
    WHERE ST_DISTANCE(f.location, {'type': 'Point', 'coordinates':[31.9, -4.8]}) < 30000

**Výsledky**

    [{
      "id": "WakefieldFamily"
    }]

Jestliže zahrnete prostorové indexování v indexování zásad, potom "vzdálenost dotazů" bude zasílat efektivní prostřednictvím indexu. Podrobné informace o prostorových indexování najdete v části. Pokud nemáte prostorové indexu pro zadané cesty, můžete pořád dělat prostorové dotazů zadáním `x-ms-documentdb-query-enable-scan` hlavičky se sadou hodnotu "true". V .NET stačí předáním volitelný argument **FeedOptions** dotazů se sadou [EnableScanInQuery](https://msdn.microsoft.com/library/microsoft.azure.documents.client.feedoptions.enablescaninquery.aspx#P:Microsoft.Azure.Documents.Client.FeedOptions.EnableScanInQuery) true (pravda). 

ST_WITHIN lze použít ke kontrole, pokud bod ležel mnohoúhelník. Běžně mnohoúhelníku slouží k představují hranice jako poštovní směrovací čísla, stát ohraničení a natural sestavy. Znovu Jestliže zahrnete prostorové indexování v indexování zásad, potom "v" dotazů bude zasílat efektivní prostřednictvím index. 

Mnohoúhelník argumentů ST_WITHIN může obsahovat pouze jeden zvonění, tedy mnohoúhelníku nesmí obsahovat mezery v nich. Zaškrtněte políčko [DocumentDB limity](documentdb-limits.md) pro maximální počet bodů pro dotaz ST_WITHIN povolené v mnohoúhelník.

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
    
Které zalamuje prostorové funkcí a syntaxe jazyka SQL pro DocumentDB. Teď si přečtěte si téma Jak LINQ dotazování funguje a interakci se syntaxí jsme viděli zatím.

## <a name="linq-to-documentdb-sql"></a>LINQ DocumentDB SQL
LINQ je .NET programovací model, která vyjadřuje výpočtu jako dotazy na datových proudů objektů. DocumentDB poskytuje klienta straně knihovny tak, aby rozhraní s LINQ usnadněním převod mezi JSON a .NET objekty a mapování z podmnožinu LINQ dotazy na DocumentDB dotazy. 

Následujícím obrázku je architektura podpůrných LINQ dotazů pomocí DocumentDB.  Pomocí klienta DocumentDB vývojáři objekt můžete vytvořit **IQueryable** přímo zadávání dotazů DocumentDB poskytovatele dotazu, který klikněte převádí LINQ dotaz na DocumentDB dotazu. Dotaz je pak předána DocumentDB serveru načíst sadu výsledků ve formátu JSON. Do datového proudu .NET objektů na straně klienta se rekonstruován vrácených výsledků.

![Architektura podpůrných LINQ dotazy, které používají DocumentDB - syntaxe jazyka SQL, JSON dotazovací jazyk, základní pojmy databáze a dotazy SQL][1]
 


### <a name="net-and-json-mapping"></a>.NET a JSON mapování
Mapování mezi objekty .NET a JSON dokumentů je přirozený - jednotlivých datových polí člena namapované na objekt JSON, kde název pole namapovat na "klávesy" část objektu a části "hodnotu" je zpětně namapované část hodnoty objektu. Zvažte následující příklad. Jak je ukázáno v následujícím příkladu řady objekt vytvořený namapovala JSON dokumentu. A naopak namapovala dokumentu JSON zpátky k objektu .NET.

**C# třídy**

    public class Family
    {
        [JsonProperty(PropertyName="id")]
        public string Id;
        public Parent[] parents;
        public Child[] children;
        public bool isRegistered;
    };
    
    public struct Parent
    {
        public string familyName;
        public string givenName;
    };
    
    public class Child
    {
        public string familyName;
        public string givenName;
        public string gender;
        public int grade;
        public List<Pet> pets;
    };
    
    public class Pet
    {
        public string givenName;
    };
    
    public class Address
    {
        public string state;
        public string county;
        public string city;
    };
    
    // Create a Family object.
    Parent mother = new Parent { familyName= "Wakefield", givenName="Robin" };
    Parent father = new Parent { familyName = "Miller", givenName = "Ben" };
    Child child = new Child { familyName="Merriam", givenName="Jesse", gender="female", grade=1 };
    Pet pet = new Pet { givenName = "Fluffy" };
    Address address = new Address { state = "NY", county = "Manhattan", city = "NY" };
    Family family = new Family { Id = "WakefieldFamily", parents = new Parent [] { mother, father}, children = new Child[] { child }, isRegistered = false };


**JSON**  

    {
        "id": "WakefieldFamily",
        "parents": [
            { "familyName": "Wakefield", "givenName": "Robin" },
            { "familyName": "Miller", "givenName": "Ben" }
        ],
        "children": [
            {
                "familyName": "Merriam", 
                "givenName": "Jesse", 
                "gender": "female", 
                "grade": 1,
                "pets": [
                    { "givenName": "Goofy" },
                    { "givenName": "Shadow" }
                ]
            },
            { 
              "familyName": "Miller", 
              "givenName": "Lisa", 
              "gender": "female", 
              "grade": 8 
            }
        ],
        "address": { "state": "NY", "county": "Manhattan", "city": "NY" },
        "isRegistered": false
    };



### <a name="linq-to-sql-translation"></a>LINQ SQL překladu
Zprostředkovatel dotazu DocumentDB provádí nejlepší mapování plánování řízené úsilí z dotazu LINQ do dotazu DocumentDB SQL. V následující popis jsme předpokládá, že má základní znalosti LINQ Čtenář.

Nejdřív pro systém typ podporujeme všechny JSON základní typy – číselné typy, logická hodnota, řetězec a hodnota null. Pouze tyto typy JSON nejsou podporované. Následující skalární výrazy jsou podporovány.

-   Konstanty – zahrnuje tyto konstanty základní datových typů v době, kdy dotaz Vyhodnocená každá její položka.

-   Vlastnost/pole index výrazech tyto výrazů v nápovědě k vlastnosti objektu nebo prvku pole.

        family.Id;
        family.children[0].familyName;
        family.children[0].grade;
        family.children[n].grade; //n is an int variable

-   Aritmetické výrazů – jedná se o výrazy aritmetické číselných a logických hodnot. Úplný seznam najdete v příručce specifikaci SQL.

        2 * family.children[0].grade;
        x + y;

-   Řetězcový výraz porovnání – jedná se o porovnávání řetězec s nějakou hodnotou konstantní řetězec.  
 
        mother.familyName == "Smith";
        child.givenName == s; //s is a string variable

-   Objekt/matice výraz pro vytvoření – tyto výrazy zpáteční objektu anonymní typů nebo složeného hodnotu nebo matici těchto objektů. Může být vnořeno do tyto hodnoty.

        new Parent { familyName = "Smith", givenName = "Joe" };
        new { first = 1, second = 2 }; //an anonymous type with 2 fields              
        new int[] { 3, child.grade, 5 };

### <a name="list-of-supported-linq-operators"></a>Seznam podporovaných LINQ operátorů
Tady je seznam podporovaných operátorů LINQ LINQ zprostředkovatel zahrnutý v sadě DocumentDB .NET SDK.

-   **Vyberte**: průzkumy přeložit do SQL vyberte včetně vytváření objektů
-   **Kde**: filtry přeložit do SQL WHERE a podpora překladu mezi & &, || a! k operátory jazyka SQL
-   **Označit více**: umožňuje prohlášení matice klauzule SQL připojení. Slouží k řetěz/nest výrazů filtrovat podle prvků pole
-   **Řadit podle a OrderByDescending**: se převádí na Order Vzestupně/sestupně:
-   **CompareTo**: se převádí na oblast porovnání. Běžně používaných řetězců, protože nejsou srovnatelná v .NET
-   **Prohlédněte**: se převádí na horní SQL omezit výsledky dotazu
-   **Matematické funkce**: podporuje překlad z. ARCSIN Abs ARCCOS, je čistého ARCTG Ceiling Cos Exp, prostorového uspořádání, protokolu, Log10, Pow, ZAOKROUHLIT, odhlásit, Sin, odmocnina, Tan, Truncate odpovídající předdefinované funkce SQL.
-   **Řetězcových funkcích**: podporuje překlad z. Na čistého propojit, obsahuje EndsWith IndexOf, počet, ToLower, TrimStart, nahradit, obrácené pořadí, TrimEnd, StartsWith, podřetězec, ToUpper odpovídající předdefinované funkce SQL.
-   **Funkce Array**: podporuje překlad z. Na čistého propojit obsahuje a počet odpovídající předdefinované funkce SQL.
-   **Funkce rozšíření geografická**: podporuje překlad z zástupná procedura metody vzdálenosti, IsValid a IsValidDetailed odpovídající předdefinované funkce SQL.
-   **Uživatel funkce přípony definovanými**: podporuje překladu z metody zástupná procedura UserDefinedFunctionProvider.Invoke odpovídající uživatelem definované funkce.
-   **Různé**: podporuje překlad v rámci slučovacího a podmíněné operátory. Obsahuje řetězec obsahuje, ARRAY_CONTAINS nebo v SQL v závislosti na kontextu, můžete přeložit.

### <a name="sql-query-operators"></a>Operátory dotazu SQL
Tady je pár příkladů, které ilustrují, jak některé standardní operátory dotazu LINQ převést dolů DocumentDB dotazů.

#### <a name="select-operator"></a>Vyberte operátor
Syntaxe je `input.Select(x => f(x))`, kde `f` je skalární výraz.

**LINQ lambda výrazu**

    input.Select(family => family.parents[0].familyName);

**SQL** 

    SELECT VALUE f.parents[0].familyName
    FROM Families f



**LINQ lambda výrazu**

    input.Select(family => family.children[0].grade + c); // c is an int variable


**SQL** 

    SELECT VALUE f.children[0].grade + c
    FROM Families f 



**LINQ lambda výrazu**

    input.Select(family => new
    {
        name = family.children[0].familyName,
        grade = family.children[0].grade + 3
    });


**SQL** 

    SELECT VALUE {"name":f.children[0].familyName, 
                  "grade": f.children[0].grade + 3 }
    FROM Families f



#### <a name="selectmany-operator"></a>Označit více operátor
Syntaxe je `input.SelectMany(x => f(x))`, kde `f` je skalární výraz, který vrací typu kolekce.

**LINQ lambda výrazu**

    input.SelectMany(family => family.children);

**SQL** 

    SELECT VALUE child
    FROM child IN Families.children



#### <a name="where-operator"></a>Kde operátor
Syntaxe je `input.Where(x => f(x))`, kde `f` je skalární výraz, který vrátí hodnotu typu Boolean.

**LINQ lambda výrazu**

    input.Where(family=> family.parents[0].familyName == "Smith");

**SQL** 

    SELECT *
    FROM Families f
    WHERE f.parents[0].familyName = "Smith" 



**LINQ lambda výrazu**

    input.Where(
        family => family.parents[0].familyName == "Smith" && 
        family.children[0].grade < 3);

**SQL** 

    SELECT *
    FROM Families f
    WHERE f.parents[0].familyName = "Smith"
    AND f.children[0].grade < 3


### <a name="composite-sql-queries"></a>Složené dotazy SQL
Výše uvedené operátory se můžou skládat tvoří výkonnější dotazů. Protože DocumentDB podporuje vnořené kolekce, složení můžete být spojeny nebo vnořené.

#### <a name="concatenation"></a>Zřetězení 

Syntaxe je `input(.|.SelectMany())(.Select()|.Where())*`. Zřetězený dotazu můžete začít s volitelným `SelectMany` dotazu následované více `Select` nebo `Where` operátory.


**LINQ lambda výrazu**

    input.Select(family=>family.parents[0])
        .Where(familyName == "Smith");

**SQL**

    SELECT *
    FROM Families f
    WHERE f.parents[0].familyName = "Smith"



**LINQ lambda výrazu**

    input.Where(family => family.children[0].grade > 3)
        .Select(family => family.parents[0].familyName);

**SQL** 

    SELECT VALUE f.parents[0].familyName
    FROM Families f
    WHERE f.children[0].grade > 3



**LINQ lambda výrazu**

    input.Select(family => new { grade=family.children[0].grade}).
        Where(anon=> anon.grade < 3);
            
**SQL** 

    SELECT *
    FROM Families f
    WHERE ({grade: f.children[0].grade}.grade > 3)



**LINQ lambda výrazu**

    input.SelectMany(family => family.parents)
        .Where(parent => parents.familyName == "Smith");

**SQL** 

    SELECT *
    FROM p IN Families.parents
    WHERE p.familyName = "Smith"



#### <a name="nesting"></a>Vnořené funkce

Syntaxe je `input.SelectMany(x=>x.Q())` Q-li `Select`, `SelectMany`, nebo `Where` operátor.

V vnořené dotazu vnitřní dotaz bude použit pro každý prvek kolekci vnější. Jiný, který je důležitým aspektem, vnitřní dotaz mohou odkazovat na pole prvky v kolekci vnější jako spojení sama.

**LINQ lambda výrazu**

    input.SelectMany(family=> 
        family.parents.Select(p => p.familyName));

**SQL** 

    SELECT VALUE p.familyName
    FROM Families f
    JOIN p IN f.parents


**LINQ lambda výrazu**

    input.SelectMany(family => 
        family.children.Where(child => child.familyName == "Jeff"));
            
**SQL** 

    SELECT *
    FROM Families f
    JOIN c IN f.children
    WHERE c.familyName = "Jeff"



**LINQ lambda výrazu**
            
    input.SelectMany(family => family.children.Where(
        child => child.familyName == family.parents[0].familyName));

**SQL** 

    SELECT *
    FROM Families f
    JOIN c IN f.children
    WHERE c.familyName = f.parents[0].familyName


## <a name="executing-sql-queries"></a>Spouštění dotazů jazyka SQL
DocumentDB zpřístupňuje zdrojů prostřednictvím rozhraní REST API, kterou můžete volat jazykem schopný provádět žádosti o protokolu HTTP/HTTPS. Kromě toho DocumentDB nabízí programovací knihoven pro několik Oblíbené jazyky například .NET Node.js, JavaScript a Python. Rozhraní REST API různých knihoven podporují a dotazování přes SQL. .NET SDK podporuje LINQ dotazování kromě SQL.

Následující příklady ukazují, jak vytvořit dotaz a odešlete ji před účet DocumentDB databáze.

### <a name="rest-api"></a>ROZHRANÍ REST API
DocumentDB nabízí otevřít RESTful programovací model přes HTTP. Účty databáze můžete zřízení používáte Azure předplatné. Model prostředků DocumentDB tvořeno sady zdrojů pod účtem databáze, přičemž každý z nich je s možností zadání použití logického a stabilní URI. Nastavení zdroje je uvedené jako informační kanál v tomto dokumentu. Účet databáze se skládá ze sady databází, každá obsahuje víc kolekcí všech které v zapnout obsahovat dokumenty, funkce definované uživatelem a další typy prostředků.

Základní interakce s těmito prostředky se model prostřednictvím akce protokolu HTTP GET, umístění, publikovat a odstranit s jejich standardní interpretaci. Příkaz příspěvek se používá pro vytvoření nového zdroje pro spuštění uložené procedury a pro vydávání DocumentDB dotazu. Dotazy se budou číst vždy pouze operace s žádné straně efekty.

Následující příklady ilustrují příspěvku pro DocumentDB dotaz proti kolekci obsahující dvě ukázkové dokumenty, že jsme jste zatím zkontrolována. Dotaz obsahuje jednoduchý filtr na vlastnost název JSON. Všimněte si `x-ms-documentdb-isquery` a typ obsahu: `application/query+json` záhlaví k označení, že je operace dotazu.


**Žádost o**

    POST https://<REST URI>/docs HTTP/1.1
    ...
    x-ms-documentdb-isquery: True
    Content-Type: application/query+json

    {      
        "query": "SELECT * FROM Families f WHERE f.id = @familyId",     
        "parameters": [          
            {"name": "@familyId", "value": "AndersenFamily"}         
        ] 
    }
    

**Výsledky**

    HTTP/1.1 200 Ok
    x-ms-activity-id: 8b4678fa-a947-47d3-8dd3-549a40da6eed
    x-ms-item-count: 1
    x-ms-request-charge: 0.32
    
    <indented for readability, results highlighted>
    
    {  
       "_rid":"u1NXANcKogE=",
       "Documents":[  
          {  
             "id":"AndersenFamily",
             "lastName":"Andersen",
             "parents":[  
                {  
                   "firstName":"Thomas"
                },
                {  
                   "firstName":"Mary Kay"
                }
             ],
             "children":[  
                {  
                   "firstName":"Henriette Thaulow",
                   "gender":"female",
                   "grade":5,
                   "pets":[  
                      {  
                         "givenName":"Fluffy"
                      }
                   ]
                }
             ],
             "address":{  
                "state":"WA",
                "county":"King",
                "city":"seattle"
             },
             "_rid":"u1NXANcKogEcAAAAAAAAAA==",
             "_ts":1407691744,
             "_self":"dbs\/u1NXAA==\/colls\/u1NXANcKogE=\/docs\/u1NXANcKogEcAAAAAAAAAA==\/",
             "_etag":"00002b00-0000-0000-0000-53e7abe00000",
             "_attachments":"_attachments\/"
          }
       ],
       "count":1
    }


Druhý příklad ukazuje složitější dotaz, který vrací výsledkem je více hodnot z spojení.

**Žádost o**

    POST https://<REST URI>/docs HTTP/1.1
    ...
    x-ms-documentdb-isquery: True
    Content-Type: application/query+json
    
    {      
        "query": "SELECT 
                     f.id AS familyName, 
                     c.givenName AS childGivenName, 
                     c.firstName AS childFirstName, 
                     p.givenName AS petName 
                  FROM Families f 
                  JOIN c IN f.children 
                  JOIN p in c.pets",     
        "parameters": [] 
    }


**Výsledky**

    HTTP/1.1 200 Ok
    x-ms-activity-id: 568f34e3-5695-44d3-9b7d-62f8b83e509d
    x-ms-item-count: 1
    x-ms-request-charge: 7.84
    
    <indented for readability, results highlighted>
    
    {  
       "_rid":"u1NXANcKogE=",
       "Documents":[  
          {  
             "familyName":"AndersenFamily",
             "childFirstName":"Henriette Thaulow",
             "petName":"Fluffy"
          },
          {  
             "familyName":"WakefieldFamily",
             "childGivenName":"Jesse",
             "petName":"Goofy"
          },
          {  
             "familyName":"WakefieldFamily",
             "childGivenName":"Jesse",
             "petName":"Shadow"
          }
       ],
       "count":3
    }


Pokud výsledcích dotazu nelze vešel do jedné stránky výsledků a rozhraní REST API vrátí token pokračování až `x-ms-continuation-token` záhlaví odpověď. Klienti můžete stránkování výsledky s využitím záhlaví na pozdější výsledky. Počet výsledků na stránku můžete taky se řídí `x-ms-max-item-count` číselná záhlaví.

Správa zásad konzistence dat pro dotazy, můžete `x-ms-consistency-level` záhlaví jako všechny žádosti o rozhraní REST API. Relace konzistence je nutná k také vypsat nejnovější `x-ms-session-token` záhlaví souborů Cookie v pozvánce na dotaz. Všimněte si, že indexování zásady dotazovaném kolekce můžete taky ovlivnit konzistence výsledků dotazu. Ve výchozím nastavení zásad indexování Collections index je vždy aktuální s obsahem dokumentu a výsledků dotazu se budou shodovat konzistence zvolený pro data. Pokud chcete opožděné je zmírněny indexování zásad, můžete vrátit dotazů zastaralé výsledky. Další informace najdete v příručce [DocumentDB konzistence úrovně] [consistency-levels].

Pokud nakonfigurovaná indexování zásada v kolekci nepodporuje zadaný dotaz, DocumentDB vrátí 400 "Chybný požadavek". To je vrácena oblast dotazů cesty konfigurace vyhledávání hash (rovnosti) a cest explicitně vyloučené ze indexování. `x-ms-documentdb-query-enable-scan` Záhlaví může být zadán umožníte dotazu k provedení prohledávání při rejstřík není k dispozici.

### <a name="c-net-sdk"></a>C# (.NET) SDK
.NET SDK podporuje LINQ a SQL dotazu. Následující příklad ukazuje, jak provádět jednoduchý filtr dotazu zavádí dříve v tomto dokumentu.


    foreach (var family in client.CreateDocumentQuery(collectionLink, 
        "SELECT * FROM Families f WHERE f.id = \"AndersenFamily\""))
    {
        Console.WriteLine("\tRead {0} from SQL", family);
    }
    
    SqlQuerySpec query = new SqlQuerySpec("SELECT * FROM Families f WHERE f.id = @familyId");
    query.Parameters = new SqlParameterCollection();
    query.Parameters.Add(new SqlParameter("@familyId", "AndersenFamily"));

    foreach (var family in client.CreateDocumentQuery(collectionLink, query))
    {
        Console.WriteLine("\tRead {0} from parameterized SQL", family);
    }

    foreach (var family in (
        from f in client.CreateDocumentQuery(collectionLink)
        where f.Id == "AndersenFamily"
        select f))
    {
        Console.WriteLine("\tRead {0} from LINQ query", family);
    }
    
    foreach (var family in client.CreateDocumentQuery(collectionLink)
        .Where(f => f.Id == "AndersenFamily")
        .Select(f => f))
    {
        Console.WriteLine("\tRead {0} from LINQ lambda", family);
    }


Tato ukázka porovnání dvou vlastností rovnosti v rámci každý dokument a používá anonymní prognózy. 


    foreach (var family in client.CreateDocumentQuery(collectionLink,
        @"SELECT {""Name"": f.id, ""City"":f.address.city} AS Family 
        FROM Families f 
        WHERE f.address.city = f.address.state"))
    {
        Console.WriteLine("\tRead {0} from SQL", family);
    }
    
    foreach (var family in (
        from f in client.CreateDocumentQuery<Family>(collectionLink)
        where f.address.city == f.address.state
        select new { Name = f.Id, City = f.address.city }))
    {
        Console.WriteLine("\tRead {0} from LINQ query", family);
    }
    
    foreach (var family in
        client.CreateDocumentQuery<Family>(collectionLink)
        .Where(f => f.address.city == f.address.state)
        .Select(f => new { Name = f.Id, City = f.address.city }))
    {
        Console.WriteLine("\tRead {0} from LINQ lambda", family);
    }


Následující příklad ukazuje spojení vyjádřené pomocí označit LINQ více.


    foreach (var pet in client.CreateDocumentQuery(collectionLink,
          @"SELECT p
            FROM Families f 
                 JOIN c IN f.children 
                 JOIN p in c.pets 
            WHERE p.givenName = ""Shadow"""))
    {
        Console.WriteLine("\tRead {0} from SQL", pet);
    }
    
    // Equivalent in Lambda expressions
    foreach (var pet in
        client.CreateDocumentQuery<Family>(collectionLink)
        .SelectMany(f => f.children)
        .SelectMany(c => c.pets)
        .Where(p => p.givenName == "Shadow"))
    {
        Console.WriteLine("\tRead {0} from LINQ lambda", pet);
    }



Klient .NET automaticky prochází všechny stránky výsledků dotazu v blocích foreach uvedené výše. Možnosti dotazu zavedená v části rozhraní REST API nabízí taky pomocí .NET SDK `FeedOptions` a `FeedResponse` tříd v metodu CreateDocumentQuery. Počet stránek, můžete řídit pomocí `MaxItemCount` nastavení. 

Můžete také explicitně ovládací prvek stránkování vytvořením `IDocumentQueryable` pomocí `IQueryable` objekt a pak v tématu` ResponseContinuationToken` hodnoty a jejich předávání zpět jako `RequestContinuationToken` v `FeedOptions`. `EnableScanInQuery`můžete nastavit povolit prohledávání při dotaz nepodporuje nakonfigurované zásady indexování. Pro oddíly kolekce, můžete použít `PartitionKey` spusťte dotaz proti typu single oddílů (když DocumentDB můžete automaticky extrahovat to u textu dotaz), a `EnableCrossPartitionQuery` spustit dotazy, které možná muset kontrolovat více oddílů. 

Další ukázky obsahující dotazy v nápovědě k [vzorky DocumentDB .NET](https://github.com/Azure/azure-documentdb-net) . 

### <a name="javascript-server-side-api"></a>Serverový API JavaScript 
DocumentDB poskytuje programovací model pro provádění logických aplikace JavaScriptu na základě přímo na kolekcí pomocí uložené procedury a aktivace. Logika JavaScript registrovaná na úrovni kolekce můžete vydá operace databáze operací na dokumenty v dané kolekci. Tyto operace jsou převázané okolního kyseliny transakce.

Následující příklad předvedení použití dokumenty dotazu na serveru JavaScript rozhraní API dotazy z vnitřní uložené procedury a aktivace.


    function businessLogic(name, author) {
        var context = getContext();
        var collectionManager = context.getCollection();
        var collectionLink = collectionManager.getSelfLink()
    
        // create a new document.
        collectionManager.createDocument(collectionLink,
            { name: name, author: author },
            function (err, documentCreated) {
                if (err) throw new Error(err.message);
    
                // filter documents by author
                var filterQuery = "SELECT * from root r WHERE r.author = 'George R.'";
                collectionManager.queryDocuments(collectionLink,
                    filterQuery,
                    function (err, matchingDocuments) {
                        if (err) throw new Error(err.message);
    context.getResponse().setBody(matchingDocuments.length);
    
                        // Replace the author name for all documents that satisfied the query.
                        for (var i = 0; i < matchingDocuments.length; i++) {
                            matchingDocuments[i].author = "George R. R. Martin";
                            // we don't need to execute a callback because they are in parallel
                            collectionManager.replaceDocument(matchingDocuments[i]._self,
                                matchingDocuments[i]);
                        }
                    })
            });
    }

## <a name="aggregate-functions"></a>Agregační funkce

Nativní podpora pro agregační funkce není v programu works, ale pokud potřebujete funkce počet nebo součet mezitím, můžete dosáhnout stejný výsledek různými způsoby.  

Na další cestě:

- Načtení dat a udělat počet místně můžete provádět agregační funkce jazyka. Sdělil používat projekce levný dotazu jako `SELECT VALUE 1` spíše než celý dokument jako `SELECT * FROM c`. Tímto způsobem, jak dosáhnout maximální počet dokumentů zpracovávají v každé stránce výsledků, a tím, že další opakované služby v případě potřeby.
- Můžete taky uložená procedura minimalizovat sítě latence na cestách opakované zaokrouhlit. Ukázka uložená procedura, které vypočítá počet pro dané filtr dotazu, najdete v článku [Count.js](https://github.com/Azure/azure-documentdb-js-server/blob/master/samples/stored-procedures/Count.js). Uložená procedura můžete povolit uživatelům kombinovat bohaté obchodní logiky spolu s udělat agregace v efektivně.

Na cestě zápisu:

- Jiné běžné vzorku je předem agregovat výsledky v parametru path "zápisu". Při je to zejména atraktivní hlasitost "číst" žádosti o je vyšší než požadavků na "napište". Jednou předem agregovanou, výsledky jsou k dispozici v jednom místě o přečtení.  Nastavení aktivační vyvolání každý "zápisu" a aktualizovat metadata dokument, který obsahuje nejnovější výsledky dotazu, který je právě nastala se nejlíp seznámíte předem agregace v DocumentDB. Například podívejte se na ukázku [UpdateaMetadata.js](https://github.com/Azure/azure-documentdb-js-server/blob/master/samples/triggers/UpdateMetadata.js) , která aktualizuje minSize, maxSize a totalSize dokument metadat pro kolekci. Vzorku lze rozšířit aktualizovat čítač, SUMA, atd.

##<a name="references"></a>Odkazy
1.  [Úvod k Azure DocumentDB][introduction]
2.  [Specifikace DocumentDB SQL](http://go.microsoft.com/fwlink/p/?LinkID=510612)
3.  [DocumentDB .NET vzorky](https://github.com/Azure/azure-documentdb-net)
4.  [DocumentDB konzistence úrovně][consistency-levels]
5.  ANSI SQL 2011 [http://www.iso.org/iso/iso_catalogue/catalogue_tc/catalogue_detail.htm?csnumber=53681](http://www.iso.org/iso/iso_catalogue/catalogue_tc/catalogue_detail.htm?csnumber=53681)
6.  JSON [http://json.org/](http://json.org/)
7.  Specifikace jazyka JavaScript [http://www.ecma-international.org/publications/standards/Ecma-262.htm](http://www.ecma-international.org/publications/standards/Ecma-262.htm) 
8.  LINQ [http://msdn.microsoft.com/library/bb308959.aspx](http://msdn.microsoft.com/library/bb308959.aspx) 
9.  Vyhodnocení dotazu technik pro velké databáze [http://dl.acm.org/citation.cfm?id=152611](http://dl.acm.org/citation.cfm?id=152611)
10. Zpracování v systémech paralelní relačních databází, stiskněte společnosti IEEE počítače 1994 dotazů
11. Funkce Tan lu, Ooi, zpracování v systémech paralelní relačních databází, stiskněte společnosti IEEE počítače 1994 dotazů.
12. Kryštofem Olston Robert číts, Utkarsh Srivastava, Ravi Kumar Andrew Tomkins: Prasátko latinka: není tak, aby cizím jazyce pro zpracování dat, SIGMOD 2008.
13.     G. Graefe. Předloha Cascades optimalizace dotazu. IEEE dat Eng. Bull., 18(3): 1995.


[1]: ./media/documentdb-sql-query/sql-query1.png
[introduction]: documentdb-introduction.md
[consistency-levels]: documentdb-consistency-levels.md
 
