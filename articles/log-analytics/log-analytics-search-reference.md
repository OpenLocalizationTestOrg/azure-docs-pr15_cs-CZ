<properties
    pageTitle="Protokol analýzy hledání odkaz | Microsoft Azure"
    description="Protokol analýzy hledání odkaz popisuje jazyk vyhledávání a poskytuje syntaxi dotazu Obecné možnosti lze použít při vyhledávání dat a filtrování výrazů zúžit vyhledávání."
    services="log-analytics"
    documentationCenter=""
    authors="bandersmsft"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="log-analytics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/25/2016"
    ms.author="banders"/>


# <a name="log-analytics-search-reference"></a>Přihlaste se odkaz hledání analýzy

V následující části Přehled o hledání jazyku uvedeny dostupné možnosti syntaxe obecné dotazu lze použít při vyhledávání dat a filtrování výrazy zúžit hledání. Popisuje taky příkazy, které můžete provádět akce na data, času.

Další informace o polích vrácený v hledání a fasetami, které vám pomůžou kopru-do podobné kategorií dat do [vyhledávacího pole a podmínka části](#search-field-and-facet-reference).

## <a name="general-query-syntax"></a>Syntaxe obecné dotazu

Syntaxe:

```
filterExpression | command1 | command2 …
```

Pro výraz filter (`filterExpression`) definuje "umístění" podmínka dotazu. Příkaz Vyrovnat výsledky dotazu. Více příkazů musí být odděleny znakem svislé čáry (|).

### <a name="general-syntax-examples"></a>Příklady syntaxe obecné

Příklady:

```
system
```

Dotaz vrátí výsledky, které obsahují slovo "systém" do libovolného pole, která obsahuje indexované pro celý text nebo podmínek vyhledávání.

>[AZURE.NOTE] Všechna pole indexována tímto způsobem, ale nejběžnější textové pole (například popisy a názvy) obvykle by.

```
system error
```

Tento dotaz vrátí výsledky, které obsahují slova *systému* a *chyby*.

```
system error | sort ManagementGroupName, TimeGenerated desc | top 10
```

Tento dotaz vrátí výsledky, které obsahují slova *systému* a *chyby*. Potom seřadí výsledky podle pole *ManagementGroupName* (vzestupně) a potom podle *TimeGenerated* (sestupně). Trvá pouze prvních 10 výsledky.

>[AZURE.IMPORTANT] Všechny názvy polí a hodnoty pro pole řetězce a text se malá a velká písmena.

## <a name="filter-expression"></a>Pro výraz Filter

Následující pododdílu popisují výrazy filtru.

### <a name="string-literals"></a>Řetězcových

Literál typu řetězec je řetězec, který se bohužel nerozpoznal analyzátor jako klíčové slovo nebo předdefinované datový typ (například číslo nebo datum).

Příklady:

```
These all are string literals
```

Tento dotaz hledá výsledky, které obsahují výskyty všech pět slov. Vyhledávání složité řetězec, uzavřete ho řetězcová konstanta do uvozovek, například:

```
" Windows Server"
```

Vrátí pouze výsledků pomocí přesné shody "V systému Windows Server".

### <a name="numbers"></a>Čísla

Analyzátor podporuje desetinné číslo a číslo s plovoucí desetinnou čárkou syntaxi u číselných polí.

Příklady:

```
Type:Perf 0.5
```

```
HTTP 500
```

### <a name="datetime"></a>Datum a čas

Každá část data v systému má *TimeGenerated* vlastnost, která představuje původní datum a čas záznamu. Některé typy dat můžete navíc mají další pole data a času (například *změněno*).

Volič časovou osu grafu a čas v protokolu analýzy zobrazuje rozdělení výsledků (podle aktuálního dotazu spuštěný) založené na poli *TimeGenerated* . Datum a čas pole s formátem určitý řetězec, který lze v dotazech omezit dotaz na konkrétní časový rámec. Můžete taky syntaxe odkázat na relativní časových intervalů (například "mezi třemi dny a 2 hodinami").

Syntaxe:

```
yyyy-mm-ddThh:mm:ss.dddZ
```

```
yyyy-mm-ddThh:mm:ss.ddd
```

```
yyyy-mm-ddThh:mm:ss
```

```
yyyy-mm-ddThh:mm:ss
```

```
yyyy-mm-ddThh:mm
```

```
yyyy-mm-dd
```


Příklad:

```
TimeGenerated:2013-10-01T12:20
```

Příkaz předchozí záznamy pouze s hodnotou *TimeGenerated* přesně 12:20 na 1 říjen 2013. Není pravděpodobné, že vám poskytne některý z výsledků, ale je jasné myšlence.

Analyzátor taky nyní podporuje mnemonické hodnoty data/času.


Znovu je pravděpodobné, že to přinese některý z výsledků, protože data nemá být celém systému to rychlé.

Tyto příklady jsou stavebních bloků pro účely relativní a absolutní kalendářní data. V následující tři pododdílu jsme budete popisují, jak je lze používat v rozšířených filtrů s příklady, které používají relativní rozsahů.

### <a name="datetime-math"></a>Matematické kalendářních dat/časů

Pomocí matematické operátory data/času odsazení nebo Zaokrouhlí hodnotu data a času pomocí jednoduchých výpočtů datum a čas.

Syntaxe:

```
datetime/unit
```

```
datetime[+|-]count unit
```

|Operátor|Popis|
|---|---|
|/|Zaokrouhlí na jednotce určené datum a čas. Příklad: Nyní / den zaokrouhlí aktuální datum a čas na půlnoc dnešního dne.|
|+ nebo –|Posouvá datum a čas zadaný počet jednotek. Příklady: nyní + 1 HODINU odsadí aktuální datum a čas podle hodinu ahead.2013-10 – 01T12:00-10 dnů odsadí hodnoty data zpět tak, že 10 dnů.|



Můžete matematické operátory datum a čas společně zřetězit, například:

```
NOW+1HOUR-10MONTHS/MINUTE
```

V následující tabulce jsou podporované jednotky datum a čas.

Jednotka kalendářních dat/časů|Popis
---|---
ROK, ROKY|Zaokrouhlí číslo na aktuální rok nebo posun o zadaný počet roků.
MĚSÍCE|Zaokrouhlí aktuální měsíc nebo odsadí o zadaný počet měsíců.
DATUM DNE, DNŮ|Zaokrouhlí číslo dnešního dne v měsíci nebo odsadí o zadaný počet dnů.
HODINY, HODIN|Zaokrouhlí číslo na aktuální hodiny nebo posun o zadaný počet hodin.
FUNKCE MINUTE MINUT|Zaokrouhlí číslo na minutu aktuální nebo odsadí o zadaný počet minut.
ZA DRUHÉ SEKUND|Zaokrouhlí číslo aktuální druhé nebo odsadí o zadaný počet sekund.
MILISEKUND MILISEKUNDÁCH MILLI MILLIS|Zaokrouhlí číslo na aktuální milisekund nebo posun o zadaný počet milisekund.


### <a name="field-facets"></a>Pole fasetami

Pomocí pole fasetami můžete určit podmínku vyhledávání konkrétních polí a jejich přesné hodnoty, namísto psaní "volné" dotazů pro různé podmínky celý index. Tuto syntaxi jsme jste již používali v několik příkladů v předchozím odstavci. Připravili jsme tady, složitější příklady.

**Syntaxe**

```
field:value
```

```
field=value
```

**Popis**

Hledá v poli pro určité hodnoty. Hodnota může být literál řetězce, číslo nebo datum a čas.

Příklad:

```
TimeGenerated:NOW
```

```
ObjectDisplayName:"server01.contoso.com"
```

```
SampleValue:0.3
```

**Syntaxe**

*pole > hodnota*

*pole < hodnota*

*pole > = hodnota*

*pole < = hodnota*

*pole! = hodnota*

**Popis**

Poskytuje srovnání.

Příklad:

```
TimeGenerated>NOW+2HOURS
```

**Syntaxe**

```
field:[from..to]
```

**Popis**

Poskytuje faceting oblast.

Příklad:

```
TimeGenerated:[NOW..NOW+1DAY]
```

```
SampleValue:[0..2]
```

### <a name="logical-operators"></a>Logické operátory

Jazyky dotazu podporují logické operátory (*a*, *nebo*a *Ne*) a jejich styl C aliases (*&&*, *||*a *!*) v tomto pořadí. Pomocí závorek zařadit do skupiny tyto operátory.

Příklady:

```
system OR error

```

```
Type:Alert AND NOT(Severity:1 OR ObjectId:"8066bbc0-9ec8-ca83-1edc-6f30d4779bcb8066bbc0-9ec8-ca83-1edc-6f30d4779bcb")
```

Je možné vynechat s logickým operátorem pro argumenty nejvyšší úrovně filtr. V tomto případě se dosadí operátoru AND.


Pro výraz Filter|Odpovídá
---|---
Chyba systému|systém a chyby
systém1 "Systému Windows Server" nebo závažnosti:|systém a ("Systému Windows Server" nebo závažnosti: 1)

### <a name="wildcarding"></a>Použití zástupných znaků

Podporuje dotazovací jazyk (*\*) znak představující jeden nebo víc znaků pro hodnotu v dotazu.

Příklady:

 Vyhledání všech počítačích s "SQL" v názvu, například "Redmond SQL".

```
Type=Event Computer=*SQL*
```

>[AZURE.NOTE] Zástupné znaky se nedají použít v rámci nabídky dnes. Zpráva =`"*This text*"` zvážíme (\*) použité jako literál (\*) znak.
## <a name="commands"></a>Příkazy

Příkaz Vyrovnat výsledky, které jsou vracená dotazem. Umožňuje použít příkaz načtená výsledky znakem svislé čáry (|). Více příkazů musí být odděleny znakem.

>[AZURE.NOTE] Příkaz jména můžete napsaný v velká a malá písmena, na rozdíl od názvy polí a data.

### <a name="sort"></a>Řazení

Syntaxe:

    sort field1 asc|desc, field2 asc|desc, …

Seřadí výsledky podle určitého polí. Asc/desc předponu je nepovinný krok. Pokud jsou vynechán, předpokládá se *asc* pořadí řazení. Pokud dotaz explicitně používat příkaz *Seřadit* , seřadit **TimeGenerated** desc je výchozí chování a vždy vrátí poslední výsledky nejdřív.

### <a name="toplimit"></a>Horní/Limit

Syntaxe:

    top number


    limit number
Omezuje odpověď výsledky horních N.

Příklad:

    Type:Alert errors detected | top 10

Vrací horních 10 odpovídající výsledky.

### <a name="skip"></a>Přeskočit

Syntaxe:

    skip number

Vynechá počet výsledků.

Příklad:

    Type:Alert errors detected | top 10 | skip 200

Vrací horních 10 odpovídající výsledky počínaje výsledek 200.

### <a name="select"></a>Vyberte

Syntaxe:

    select field1, field2, ...

Omezení výsledků na pole, které zvolíte.

Příklad:

    Type:Alert errors detected | select Name, Severity

Omezení vrácených výsledků polí *název* a *závažnosti*.

### <a name="measure"></a>Míra

Příkaz *Míra* slouží k použití statistických funkcí jako nezpracovaná výsledky. To je velmi užitečné získat zobrazení *Seskupit podle* nad daty. Při použití příkazu *Míra* protokolu analýzy vyhledávání zobrazuje tabulku s agregovanými výsledky.

Syntaxe:

    measure aggregateFunction1([aggregatedField]) [as fieldAlias1] [, aggregateFunction2([aggregatedField2]) [as fieldAlias2] [, ...]] by groupField1 [, groupField2 [, groupField3]]  [interval interval]


    measure aggregateFunction1([aggregatedField]) [as fieldAlias1] [, aggregateFunction2([aggregatedField2]) [as fieldAlias2] [, ...]]  interval interval



Sloučí výsledky podle *skupinové pole* a agregovaná míra hodnoty vypočítá pomocí *aggregatedField*.


|Míra statistické funkce|Popis|
|---|---|
|*aggregateFunction*|Název funkce agregace (velkých písmen). Jsou podporované tyto funkce agregace: počet maximální počet MIN součet AVG STDDEV COUNTDISTINCT PERCENTILU ## nebo Procenta ## (## je libovolné číslo od 1 do 99)|
|*aggregatedField*|Pole, které je právě agregované. Toto pole není povinné agregační funkce počet, ale musí být existující číselné pole pro sčítání, MAX, MIN, AVG STDDEV PERCENTILU ## nebo Procenta ## (## je libovolné číslo od 1 do 99). AggregatedField lze také některou z těchto funkcích rozšířit podporované.|
|*fieldAlias*|(Volitelné) alias pro počítané agregovanou hodnotu. Pokud není zadán, bude mít pole Název AggregatedValue.|
|*skupinové pole*|Název pole, která výsledek seskupena podle.|
|*Interval*|Časový interval ve formátu:**nnnNAME** Where: nnn je kladné celé číslo. **Název** je název interval. Podporované interval názvy obsahují (malá a velká písmena): MILISEKUND [S] druhého [S] MINUTE [S] den HODINU [S] [S] měsíce [S] rok [S]|


Možnost nastavení intervalu lze použít pouze v polích typu datum a čas skupiny (například *TimeGenerated* a *TimeCreated*). V současné době to není nevynucují službou, ale pole bez datum a čas, které je do back-end způsobí chybu runtime. Při provádění ověřování schématu rozhraní API služeb odmítne dotazy, které používají pole bez data a času pro interval agregaci. Aktuální implementace *Míra* podporuje interval seskupení agregační funkce.

Pokud vynecháte argument klauzuli podle ale interval není zadán (jako druhý syntaxe), předpokládá se, že pole *TimeGenerated* je ve výchozím nastavení.

Příklady:

**Příklad 1**

    Type:Alert | measure count() as Count by ObjectId

*Vysvětlení*

Seskupí upozornění tak, že *objektu* a vypočítá počet upozornění pro každou skupinu. Agregovaná hodnota je vrácena jako pole *počet* (alias).

**Příklad 2**

    Type:Alert | measure count() interval 1HOUR

*Vysvětlení*

Upozornění na seskupí podle 1 hodinu intervalů pomocí pole *TimeGenerated* a vrátí hodnotu upozornění v každém intervalu.

**Příklad 3**

    Type:Alert | measure count() as AlertsPerHour interval 1HOUR

*Vysvětlení*

Stejně jako v předchozím příkladu, ale s alias agregované pole (*AlertsPerHour*).

**Příklad 4:**

    * | změřit count() TimeCreated interval 5DAYS

*Vysvětlení*

Seskupí výsledky podle intervalů 5 dnů pomocí pole *TimeCreated* a vrátí počet výsledků v každém intervalu.

**Příklad 5**

    Type:Alert | measure max(Severity) by WorkflowName

*Vysvětlení*

Skupiny upozornění na název pracovního vytížení a vrátí hodnotu maximální upozornění závažnosti pro každý pracovní postup.

**Příklad 6**

    Type:Alert | measure min(Severity) by WorkflowName

*Vysvětlení*

Stejně jako v předchozím příkladu, ale s *Min* agregované (funkce).

**Příklad 7**

    Type:Perf | measure avg(CounterValue) by Computer

*Vysvětlení*

Seskupí výkonu ve počítače a vypočítá průměr (avg).

**Příklad 8**

    Type:Perf | measure sum(CounterValue) by Computer

*Vysvětlení*

Stejně jako v předchozím příkladu, ale používá funkce *Součet*.

**Příklad 9**

    Type:Perf | measure stddev(CounterValue) by Computer

*Vysvětlení*

Stejně jako v předchozím příkladu, ale používá *STDDEV*.

**Příklad 10**

    Type:Perf | measure percentile70(CounterValue) by Computer

*Vysvětlení*

Stejně jako v předchozím příkladu, ale používá *PERCENTILE70*.

**Příklad 11**

    Type:Perf | measure pct70(CounterValue) by Computer

*Vysvětlení*

Stejně jako v předchozím příkladu, ale používá *PCT70*. Všimněte si, že *Procenta ##* pouze alias *PERCENTILU ##* (funkce).

**Příklad 12**

    Type:Perf | measure avg(CounterValue) by Computer, CounterName

*Vysvětlení*

Nejdřív seskupuje výkonu ve počítače a potom název_čítače a vypočítá průměr (avg).

**Příklad 13**

    Type:Alert | measure count() as Count by WorkflowName | sort Count desc | top 5

*Vysvětlení*

Získá horní pět pracovní postupy s maximální počet upozornění.

**Příklad 14**

    * | míra countdistinct(Computer) podle typu

*Vysvětlení*

Určí počet jedinečných vykazování u jednotlivých typů.

**Příklad 15**

    * | míra countdistinct(Computer) intervalu 1 HODINU

*Vysvětlení*

Určí počet jedinečných vytváření sestav pro každou hodinu.

**Příklad 16**

```
Type:Perf CounterName=”% Processor Time” InstanceName=”_Total” | measure avg(CounterValue) by Computer Interval 1HOUR
```

*Vysvětlení*

Seskupí % času procesoru počítače a vrátí průměrnou hodnotu pro každou 1 hodinu.

**Příklad 17**

    Type:W3CIISLog | measure max(TimeTaken) by csMethod Interval 5MINUTES

*Vysvětlení*

Skupiny W3CIISLog metodou a vrátí maximální každých 5 minut.

**Příklad 18**

```
Type:Perf CounterName=”% Processor Time” InstanceName=”_Total”  | measure min(CounterValue) as MIN, avg(CounterValue) as AVG, percentile75(CounterValue) as PCT75, max(CounterValue) as MAX by Computer Interval 1HOUR
```

*Vysvětlení*

Seskupí % času procesoru počítače a vrátí minimální, průměr, 75 procent a maximální pro každou 1 hodinu.

**Příklad 19**

```
Type:Perf CounterName=”% Processor Time”  | measure min(CounterValue) as MIN, avg(CounterValue) as AVG, percentile75(CounterValue) as PCT75, max(CounterValue) as MAX by Computer, InstanceName Interval 1HOUR
```

*Vysvětlení*

Seskupí % času procesoru nejdřív ve počítače a potom podle názvu Instance a vrátí minimální, průměr, 75 procent a maximální pro každou 1 hodinu

**Příklad 20**

```
Type= Perf CounterName="Disk Writes/sec" Computer="BaconDC01.BaconLand.com" | measure max(product(CounterValue,60)) as MaxDWPerMin by InstanceName Interval 1HOUR
```

*Vysvětlení*

Vypočítá že maximální disku zapíše za minutu pro každý disku v počítači

### <a name="where"></a>Kde

Syntaxe:

```
**where** AggregatedValue>20
```

Pouze lze po provedení příkazu *Míra* dále filtrovat souhrnné výsledky, které má vyrobeno funkce agregace *Míra* .

Příklady:

    Type:Perf CounterName:"% Total Run Time" | Measure max(CounterValue) as MAXCPU by Computer

    Type:Perf CounterName:"% Total Run Time" | Measure max(CounterValue) by Computer | where (AggregatedValue>50 and AggregatedValue<90)

### <a name="in"></a>V

Syntaxe:

```
(Outer Query) (Field to use with inner query results) IN {Inner query | measure count() by (Field to send to outer query)} (rest  of outer query)  
```

Popis: Tuto syntaxi umožňuje vytvářet agregaci a kanálu seznamu hodnot z této agregace do jiného vnější hledání (primární), které vyhledá události se tyto hodnoty. To uděláte tak, že orámování vnitřní vyhledávání do složených závorek a podávání jeho výsledky jako možné hodnoty pole vnější hledat pomocí operátor.

Vnitřní dotaz příklad: *počítačů aktuálně chybějící aktualizace zabezpečení* se následující dotaz agregace:

```
Type:Update Classification="Security Updates"  UpdateState=needed TimeGenerated>NOW-25HOURS | measure count() by Computer
```    

Výsledný dotaz, který najde *všech událostí Windows pro počítače aktuálně chybějící aktualizace zabezpečení* by mohl vypadat například:

```
Type=Event Computer IN {Type:Update Classification="Security Updates"  UpdateState=needed TimeGenerated>NOW-25HOURS | measure count() by Computer}
```

### <a name="dedup"></a>Dedup

**Syntaxe**

    Dedup FieldName

**Popis** Vrátí první dokument nalezený pro všechny jedinečné hodnoty daného pole.

**Příklad**

    Type=Event | sort TimeGenerated DESC | Dedup EventID

Výše uvedený příklad vrací jednu událost (posledních, protože používáme DESC na TimeGenerated) za EventID


### <a name="extend"></a>Rozšíření

**Popis** Umožňuje vytvořit běhu pole v dotazech. Můžete také příkaz míra po rozšířit Pokud chcete provést agregaci.

**Příklad 1**

    Type=SQLAssessmentRecommendation | Extend product(RecommendationScore, RecommendationWeight) AS RecommendationWeightedScore
Zobrazit skóre váženého doporučení pro hodnocení SQL doporučení

**Příklad 2**

    Type=Perf CounterName="Private Bytes" | EXTEND div(CounterValue,1024) AS KBs | Select CounterValue,Computer,KBs
Zobrazit hodnoty čítače v kB místo bajtů

**Příklad 3**

    Type=WireData | EXTEND scale(TotalBytes,0,100) AS ScaledTotalBytes | Select ScaledTotalBytes,TotalBytes | SORT TotalBytes DESC
Změna velikosti přínosu WireData TotalBytes tak, aby se všechny výsledky leží mezi 0 a 100.

**Příklad 4:**

```
Type=Perf CounterName="% Processor Time" | EXTEND if(map(CounterValue,0,50,0,1),"HIGH","LOW") as UTILIZATION
```
Označení jako vysoký výkon hodnoty menší než 50 % las minimum a další

**Příklad 5**

```
Type= Perf CounterName="Disk Writes/sec" Computer="BaconDC01.BaconLand.com" | Extend product(CounterValue,60) as DWPerMin| measure max(DWPerMin) by InstanceName Interval 1HOUR
```
Vypočítá že maximální disku zapíše za minutu pro každý disku v počítači

**Podporované funkce**


| (Funkce) | Popis | Příklady syntaxe |
|---------|---------|---------|
| Abs | Vrátí absolutní hodnotu zadanou hodnotu nebo funkce. | `abs(x)` <br> `abs(-5)` |
| ARCCOS | Vrátí arkuskosinus hodnotu nebo funkci | `acos(x)` |
| a | Vrátí hodnotu true a pouze v případě všechny operandů jsou vyhodnoceny jako PRAVDA. | `and(not(exists(popularity)),exists(price))` |
| ARCSIN | Vrátí arkussinus hodnotu nebo funkci | `asin(x)` |
| ARCTG | Vrátí arkustangens hodnotu nebo funkci | `atan(x)` |
| ARCTG2 |  Vrátí úhel vznikly převodem rectangular souřadnice x, y polární souřadnice | `atan2(x,y)` |
| cbrt | Třetí odmocninu |  `cbrt(x)` |
| ceil | Zaokrouhlí číslo na celé číslo | `ceil(x)`  <br> `ceil(5.6)`-Vrátí hodnotu 6 |
| Cos | Vrátí kosinus úhlu. | `cos(x)` |
| COSH | Vrátí hyperbolický kosinus úhlu. | `cosh(x)` |
| rozlišení | definice je zkratka pro výchozí. Vrátí hodnotu "pole" nebo v případě pole neexistuje, vrátí výchozí hodnota zadaná a výsledkem je první hodnota, kde: `exists()==true`. | `def(rating,5)`– Tato funkce def() vrátí hodnocení nebo pokud žádné hodnocení zadané v dokumentu, vrátí funkce 5 <br> `def(myfield, 1.0)`-shodný s obsahem`if(exists(myfield),myfield,1.0)` |
| stupňů | Převést radiány na stupně. |  `deg(x)` |
| div | `div(x,y)`vydělí x, y. | `div(1,y)` <br> `div(sum(x,100),max(y,1))` |
| DIST | Vrátí vzdálenost mezi dvěma vektory, (body) n dimenzionální místa. Přenese v power plus dva nebo více ValueSource instance a vypočítá vzdálenosti mezi dvěma způsoby. Každý ValueSource musí být čísla. Musí být sudé číslo instancí ValueSource předaný a metodu předpokládá, že v první polovině představují prvním vektoru a druhé poloviny představují druhý vektorové.  | `dist(2, x, y, 0, 0)`-Vypočítá Euclidean vzdálenost between,(0,0) a (x, y) pro každý dokument. <br> `dist(1, x, y, 0, 0)`-Vypočítá Manhattan (taxicab) vzdálenost mezi (0,0) a (x, y) pro každý dokument. <br> `dist(2,,x,y,z,0,0,0)`-Euclidean vzdálenost mezi (0,0,0) a (x, y, z) pro každý dokument.<br>`dist(1,x,y,z,e,f,g)`-Manhattan vzdálenost mezi (x, y, z) a (e, f, g), kde každý znak je název pole. |
| existuje | Vrátí hodnotu PRAVDA, pokud všechny členem pole existuje. | `exists(author)`– Vrátí hodnotu TRUE pro všechny dokumenty má hodnotu v poli "author".<br>`exists(query(price:5.00))`– Vrátí hodnotu PRAVDA, pokud odpovídá "cena", "5.00". |
| Exp | Vrátí hodnotu prvku Eulerova číslo umocněné na x | `exp(x)` |
| ZAOKR.dolů | Zaokrouhlí číslo dolů na celé číslo | `floor(x)`  <br> `floor(5.6)`-Vrátí hodnotu 5 |
| hypo | Vrátí sqrt(sum(pow(x,2),pow(y,2))) bez intermediate přetečení a podtečení | `hypo(x,y)`  <br> ` |
| Když | Umožňuje podmíněné funkce dotazů. V `if(test,value1,value2)` -testu je nebo odkazuje na logickou hodnotu nebo výraz, který vrací logickou hodnotou (TRUE nebo FALSE).  `value1`je hodnota, která je vrácené funkcí, pokud test dává hodnotu PRAVDA. `value2`je hodnota, která je vrácené funkcí, pokud test dává NEPRAVDA. Výraz může mít libovolnou funkci, která výstupy logické hodnoty nebo i funkce, které vracejí číselné hodnoty, ve kterých bude případu hodnotu 0 interpretován jako NEPRAVDA nebo řetězce v takovém případě prázdný řetězec je odkaz interpretován jako NEPRAVDA. | `if(termfreq(cat,'electronics'),popularity,42)`– Tato funkce zkontroluje každý dokument pro chcete zobrazit, pokud obsahuje daný termín "elektronických" v poli kočka. Pokud ano, pak je vrácena hodnota pole oblíbenosti, jinak hodnota čísla 42 je vrácena. |
| lineární | Implementuje `m*x+c` m a c jsou konstanty kde x je libovolného funkce. Toto je shodný s obsahem `sum(product(m,x),c)`, ale mírně zlepšují účinnost prováděn jako jednu funkci. | `linear(x,m,c) linear(x,2,4)`Vrátí`2*x+4` |
| ln| Vrátí natural protokolu určité funkce |  `ln(x)` |
| log | Vrátí protokol určité funkce se základem 10. | `log(x)   log(sum(x,100))` |
| mapy | Mapy všechny hodnoty funkce input x, které spadají do min a max včetně pro vybraný cíl. Argumenty funkce min a max musí být konstanty. Argumenty cílové a výchozí může být konstanty nebo funkce. Pokud hodnota x není patří mezi funkce min a max, poté je vrácena hodnota x nebo výchozí hodnota je vrácena, pokud zadaný jako hodnota argumentu 5.. |  `map(x,min,max,target) map(x,0,0,1)`– Změní se všechny hodnoty 0 a 1. To může být užitečné při zpracování hodnoty 0.<br> `map(x,min,max,target,default)    map(x,0,100,1,-1)`– Změní všechny hodnoty mezi 0 a 100 až 1 a všechny ostatní hodnoty -1.<br>  `map(x,0,100,sum(x,599),docfreq(text,solr))`– Všechny hodnoty mezi 0 a 100 x + 599 a všechny ostatní hodnoty se změní na četnost termín "solr" v textovém poli. |
| Max | Vrátí maximální hodnotu číselné více vnořených funkcí nebo konstant, které jsou zadány jako argumenty: `max(x,y,...)`. Funkce max může být také užitečné pro "bottoming" jinou funkci nebo zadat konstantu pole na některé.  Použití `field(myfield,max)` syntaxe pro výběr maximální hodnotu z jednoho pole s více hodnotami.  | `max(myfield,myotherfield,0)` |
| min | Vrátí minimální hodnotu číselných konstant, které jsou zadány jako argumenty více vnořených funkcí: `min(x,y,...)`. Funkce min může být také užitečné při poskytování "horní mez" na funkci použití konstanty. Použití `field(myfield,min)` syntaxe pro výběr minimální hodnotu z jednoho pole s více hodnotami. | `min(myfield,myotherfield,0)` |
| MOD | Vypočítá modul funkci x, y (funkce). |`mod(1,x)` <br> `mod(sum(x,100), max(y,1))`   |
| MS | Vrátí rozdíl mezi argumenty v milisekundách. Data jsou relativní Unix nebo POSIX epocha čas, půlnoci, 1. ledna 1970 UTC. Argumenty může být název indexované TrieDateField nebo matematické kalendářních dat na základě konstantních data nebo teď. `ms()`je shodný s obsahem `ms(NOW)`, počet milisekund od epocha. `ms(a)`Vrátí počet milisekund od epocha představující argument. `ms(a,b)`Vrátí počet milisekund od tohoto b předchází, což je `a - b`.| `ms(NOW/DAY)`<br>`ms(2000-01-01T00:00:00Z)`<br>`ms(mydatefield)`<br>`ms(NOW,mydatefield)`<br>`ms(mydatefield,2000-01-01T00:00:00Z)`<br>`ms(datefield1,datefield2)` |
| Not | Logicky Negované hodnotu zalomený funkce. | `not(exists(author))`-PLATÍ pouze v případě `exists(author)` vyhodnotí jako NEPRAVDA. |
| nebo | Logický součet. | `or(value1,value2)`-Hodnotu PRAVDA, pokud platí hodnota1 nebo hodnota2. |
| Pow | Vyvolá zadaném základu na zadanou mocninu. `pow(x,y)`Vyvolá x power y. |  `pow(x,y)`<br>`pow(x,log(y))`<br>`pow(x,0.5)`-Stejná jako funkce sqrt. |
| produktu | Vrací součin více hodnot nebo funkce, které jsou uvedené v poli seznam oddělený čárkami. `mul(...)`mohou také sloužit jako alias pro tuto funkci. | `product(x,y,...)`<br>`product(x,2)`<br>`product(x,y)`<br>`mul(x,y)` |
| recip | Vzájemné funkci s `recip(x,m,a,b)` provádění `a/(m*x+b)` kde m, a, b jsou konstanty a x je libovolně komplexní funkci. Když a b jsou rovná a x > = 0, tuto funkci obsahuje maximální hodnota 1, která vynechává x zvýšení. Zvětšení hodnoty a b společně za následek pohybu funkce celý plošší část křivky. Tyto vlastnosti nastavit ideální funkce pro zvýšení více poslední dokumenty, pokud argument x není `rord(datefield)`. | `recip(myfield,m,a,b)`<br>`recip(rord(creationDate),1,1000,1000)` |
| rad | Převést stupně na radiány. | `rad(x)` |
| Tisknout| Zaokrouhlí číslo na nejbližší celé číslo | `rint(x)`  <br> `rint(5.6)`-Vrátí hodnotu 6 |
| Funkce Sin | Vrátí sinus úhlu. | `sin(x)` |
| SINH | Vrátí hyperbolický sinus úhlu. | `sinh(x)` |
| stupnice | Stupnice hodnot ve funkci x, která spadají mezi zadaný minTarget a maxTarget včetně. Aktuální implementaci projde všech hodnot funkce získat min a max, takže můžete vybrat správné měřítko. Aktuální implementaci nerozlišuje po odstranění dokumenty nebo dokumenty, které nemají hodnotu. Takovýchto případech používá 0.0 hodnoty. To znamená, že pokud jsou obvykle všechny větší 0,0 hodnoty, jednu můžete pořád nakonec znamenat 0,0 jako nejmenší hodnota mapování. V těchto případech odpovídající `map()` funkci lze použít jako alternativu můžete změnit 0,0 na hodnoty v oblasti skutečné znázorněná na obrázku:`scale(map(x,0,0,5),1,2)` | `scale(x,minTarget,maxTarget)`<br>`scale(x,1,2)`– Změní tak, aby všechny hodnoty budou mezi 1 a 2 včetně hodnoty x. |
| Odmocnina | Vrátí druhou odmocninu zadanou hodnotu nebo funkce. | `sqrt(x)`<br>`sqrt(100)`<br>`sqrt(sum(x,100))` |
| strdist | Výpočet vzdálenost mezi dva řetězce. Používá rozhraní Lucene kontrola pravopisu StringDistance a podporuje všechny dostupné v daném balíčku implementace plus umožňuje, aby zapojte svoje vlastní pomocí společnosti Solr prostředků možnosti načtení aplikace. Přenese strdist `(string1, string2, distance measure)`. Možné hodnoty vzdálenost míry jsou: <br>jw: Jaro Winkler<br>úpravy: Levenstein nebo upravit vzdálenost<br>ngram: NGramDistance-li, můžete volitelně předat velikosti ngram příliš. Výchozí hodnota je 2.<br>FQN: Plně kvalifikovaný název pro implementaci rozhraní StringDistance třídy. Musí mít žádný argument konstruktor.|`strdist("SOLR",id,edit)` |
| Sub | Vrátí hodnotu x a y z `sub(x,y)`. | `sub(myfield,myfield2)`<br>`sub(100,sqrt(myfield))` |
| SUMA | Vrátí součet druhých více hodnot nebo funkce, které jsou uvedené v poli seznam oddělený čárkami. `add(...)`mohou sloužit jako alias pro tuto funkci. | `sum(x,y,...)`<br>`sum(x,1)`<br>`sum(x,y)`<br>`sum(sqrt(x),log(y),z,0.5)`<br>`add(x,y)`|
|termfreq | Vrátí počet termín se zobrazí v poli pro tento dokument. | termfreq(text,'memory')|
| Funkce Tan | Vrátí tangens úhlu. | `tan(x)` |
| TGH | Vrátí hyperbolický tangens úhlu. | `tanh(x)` |



## <a name="search-field-and-facet-reference"></a>Vyhledávací pole a podmínka odkaz

Při použití protokolu hledání k vyhledání dat zobrazí výsledky různá pole a fasetami. Některé informace, které se zobrazí však nemusí zobrazovat velmi popisný. Můžete tyto informace vám pomůže pochopit výsledky.



|Pole|Typ hledání|Popis|
|---|---|---|
|TenantId|Všechny|Slouží k data oddílu|
|TimeGenerated|Všechny|Slouží k řízení na časovou osu, timeselectors (v hledání a další dialogová okna). Představuje při generování část dat (obvykle na agent). Čas vyjádřena ISO formátu a je vždy UTC. V případě "typů", které jsou založeny na existující přístrojového vybavení (tedy události v protokolu) je to obvykle reálném čase položka/řádek/záznamu protokolu přihlášeného pro některé z dalších typů vytvořené pomocí management Pack nebo v cloudu – tedy doporučení/upozornění/updateagent/atd, to je čas shromažďované tento nový část dat, která se snímkem konfigurací některých řazení nebo byl vytvořen doporučení/upozorněním na základě ho.|
|EventID|Události|EventID v protokolu událostí systému Windows|
|Protokol událostí|Události|Pokud byla událost zaznamenané Windows v protokolu událostí|
|EventLevelName|Události|Kritické / upozornění / informace / úspěch|
|EventLevel|Události|Číselná hodnota pro kritické / upozornění / informace / success (EventLevelName místo toho použít pro snadnější/čitelnější dotazů)|
|SourceSystem|Všechny|Kde se berou data (z hlediska režimu "připojit ke službě – tedy Operations Manager OMS (= data generováno v cloudu), Azure úložiště (dat pocházejících z WAD) atd.|
|Název objektu|PerfHourly|Název objektu výkonu systému Windows|
|Název_instance|PerfHourly|Název instance čítače výkonu systému Windows|
|CounteName|PerfHourly|Název čítače výkonu systému Windows|
|ObjectDisplayName|PerfHourly ConfigurationObjectProperty ConfigurationAlert, ConfigurationObject,|Zobrazit název objektu určené kolekci pravidlem výkonu v Operations Manager nebo barvy objektu zjistil provozní přehledy nebo proti upozornění vygenerované|
|RootObjectName|PerfHourly ConfigurationObjectProperty ConfigurationAlert, ConfigurationObject,|Zobrazovaný název nadřazeného nadřazeného (v dvojitých hostingu relaci: tedy SqlDatabase hostitelem SqlInstance hostované ve počítače Windows) objektu určené kolekci pravidlem výkonu v Operations Manager nebo barvy objektu zjistil provozní přehledy nebo proti upozornění vygenerované|
|Počítač|U většiny typů|Název počítače, který data patří|
|Název zařízení|ProtectionStatus|Název počítače data patří (totéž jako"")|
|DetectionId|ProtectionStatus| |
|ThreatStatusRank|ProtectionStatus|Pořadí stav hrozbou, že je číselných znázornění hrozbou, že stav a podobně jako kódy odpověď HTTP jsme už opustili mezery mezi čísly (což je proč žádné hrozeb je 150 a 100 ani 0) tak, aby zde máme pár místa pro přidání nových státy. Zahrnutí hrozbou, že stav a stav ochrany v takovém případě chceme Zobrazit nejhorší stav, který byl počítač v během vybraného časového období. Řadit různé stavy tak jsme vyhledejte záznam s nejvyšší používáme čísla.|
|ThreatStatus|ProtectionStatus|Popis ThreatStatus, mapy 1:1 s ThreatStatusRank|
|TypeofProtection|ProtectionStatus|Produkt proti malwaru, který je zjištěno v počítači: žádný, nástroj společnosti Microsoft proti malwaru, Forefront a tak dále|
|ScanDate|ProtectionStatus| |
|SourceHealthServiceId|ProtectionStatus RequiredUpdate|ID HealthService pro tento počítač agent|
|HealthServiceId|U většiny typů|ID HealthService pro tento počítač agent|
|ManagementGroupName|U většiny typů|Název skupiny správy pro Operations Manager připojené agentů; v opačném případě bude hodnota null nebo prázdné|
|Vztah mezi typem objektu|ConfigurationObject|Typ (jako Operations Manager management pack "typ" / třídy) pro tento objekt zjistí protokolu technologie pro analýzu konfigurace hodnocení|
|UpdateTitle|RequiredUpdate|Název nalezený aktualizace není nainstalován|
|PublishDate|RequiredUpdate|Kdy byla aktualizace publikované na webu Microsoft update?|
|Server|RequiredUpdate|Název počítače data patří (totéž jako"")|
|Produktu|RequiredUpdate|Produkt, který odpovídá aktualizace|
|UpdateClassification|RequiredUpdate|Typ aktualizace (kumulativní aktualizace, s aktualizací Service pack atd.)|
|KBID|RequiredUpdate|ID článek KB popisující tento nejlepší praxe nebo aktualizace|
|WorkflowName|ConfigurationAlert|Název pravidla nebo monitor, který vytvořené upozornění|
|Závažnosti|ConfigurationAlert|Závažnosti upozornění|
|Priority (priorita)|ConfigurationAlert|Priority (priorita) oznámení|
|IsMonitorAlert|ConfigurationAlert|Toto oznámení vytváří monitoru (pravda) nebo pravidla (NEPRAVDA)?|
|AlertParameters|ConfigurationAlert|XML s parametry upozornění protokolu analýzy|
|Kontext|ConfigurationAlert|Jazyk XML s "kontextu" upozornění protokolu analýzy|
|Pracovní zátěž|ConfigurationAlert|Technologie nebo "pracovní zátěž" odkazující na upozornění|
|AdvisorWorkload|Doporučení|Technologie nebo "pracovní zátěž" odkazující doporučení|
|Popis|ConfigurationAlert|Popis oznámení (krátké)|
|DaysSinceLastUpdate|UpdateAgent|Kolik dny (relativní "TimeGenerated" tohoto záznamu) Tento agent nainstalovali všechny aktualizace WSUS/Microsoft Update?|
|DaysSinceLastUpdateBucket|UpdateAgent|Podle DaysSinceLastUpdate kategorizace v času bloky o tom, jak dlouho nedávno byl počítač poslední nainstalované všechny aktualizace WSUS/Microsoft Update|
|AutomaticUpdateEnabled|UpdateAgent|Je kontrola automatické aktualizace povolit nebo zakázat na tento agent?
|AutomaticUpdateValue|UpdateAgent|Automatická aktualizace kontroluje nastavit na stav automaticky stáhnout a nainstalovat, jenom stáhnout a pouze zkontrolujte?|
|WindowsUpdateAgentVersion|UpdateAgent|Číslo verze Microsoft Update agent|
|WSUSServer|UpdateAgent|Který WSUS server je tento update agent zacílení?|
|OSVersion|UpdateAgent|Verze operačního systému tento update agent běží|
|Jméno|Doporučení, ConfigurationObjectProperty|Názvu doporučení nebo název vlastnosti z protokolu technologie pro analýzu konfigurace hodnocení|
|Hodnota|ConfigurationObjectProperty|Hodnota vlastnosti z protokolu technologie pro analýzu konfigurace hodnocení|
|KBLink|Doporučení|Adresa URL pro článek ve znalostní bázi Knowledge Base popisující tento nejlepší praxe nebo aktualizace|
|RecommendationStatus|Doporučení|Doporučení patří mezi několik typů jejichž záznamy "aktualizují', nejenom přidávají do indexu vyhledávání. Tenhle stav se změní zda doporučení je aktivní nebo otevřít nebo pokud protokolu analýzy zjistí, že už uběhlo přeložit.|
|RenderedDescription|Události|Vykreslená popis (znovu používaný text s vyplněné parametry) událostí systému Windows|
|ParameterXml|Události|XML s parametry v části "data za rok" událostí systému Windows (jak je vidět v prohlížeči událostí)|
|EventData|Události|Jazyk XML s celou "data za rok" část událostí systému Windows (jak je vidět v prohlížeči událostí)|
|Zdroje|Události|Zdroj protokolu událostí, které vygenerovalo události|
|EventCategory|Události|Kategorie události přímo z protokolu událostí systému Windows|
|Uživatelské jméno|Události|Uživatelské jméno událostí systému Windows (obvykle NT AUTHORITY\LOCALSYSTEM)|
|SampleValue|PerfHourly|Průměrná hodnota pro každou hodinu agregace čítače výkonu|
|Min|PerfHourly|Minimální hodnota v hodinách intervalu výkonu hodinové aggregate čítač|
|Max|PerfHourly|Maximální hodnota v hodinách intervalu výkonu hodinové aggregate čítač|
|Percentile95|PerfHourly|95. hodnota percentilu pro každou hodinu interval výkonu hodinové aggregate čítač|
|SampleCount|PerfHourly|Kolik ukázky čítač "nezpracovanými" výkonu byla použita k plodiny tohoto hodinové agregační záznamu|
|Riziko|ProtectionStatus|Název malware najít|
|StorageAccount|W3CIISLog|Účet Azure úložiště protokol byl číst z|
|AzureDeploymentID|W3CIISLog|ID Azure nasazení cloudové služby protokolu patří|
|Role|W3CIISLog|Role cloudové služby Azure patří protokolu|
|RoleInstance|W3CIISLog|RoleInstance Azure role, které patří protokolu|
|sSiteName|W3CIISLog|Webového serveru IIS, které protokol patří (metabáze notation); pole s název webu v původní protokolu|
|sComputerName|W3CIISLog|Pole s název_počítače v původní protokolu|
|sIP|W3CIISLog|Server IP adresu HTTP žádost byla určena. V poli s ip v původní protokolu|
|csMethod|W3CIISLog|Metoda HTTP (GET/POST/atd) používá klienta v pozvánce na HTTP. Metoda cs v původní protokolu|
|cIP|W3CIISLog|Klient IP adresu HTTP žádost pochází od. Pole c ip v původní protokolu|
|csUserAgent|W3CIISLog|User-Agent protokolu HTTP deklarováno klientem (prohlížeče nebo jinak). Cs uživatelského agenta do původní protokolu|
|scStatus|W3CIISLog|Nastavit informace HTTP Vrácený stavový kód (200/403/500/atd) serverem klientovi. Cs stavu u protokolu původní|
|TimeTaken|W3CIISLog|Jak dlouho (v milisekundách), který žádost přijal dokončete. Pole timetaken v původní protokolu|
|csUriStem|W3CIISLog|Relativní identifikátor Uri (bez hostovat adresu, tedy "/ hledat"), které bylo požadováno. Pole cs uristem v původní protokolu|
|csUriQuery|W3CIISLog|Identifikátor URI dotazu. Identifikátor URI dotazy jsou potřebné pouze pro dynamické stránky, například ASP stránky tak, aby toto pole obvykle obsahuje pomlčku statické stránek.|
|sPort|W3CIISLog|Port serveru, která byla odeslána žádost HTTP (a přijímá služby IIS, protože ho vybral)|
|csUserName|W3CIISLog|Ověření uživatelské jméno, pokud je žádost o ověřené a ne anonymní|
|csVersion|W3CIISLog|Použít v žádosti o verzi protokolu HTTP (tedy "protokolu HTTP / 1.1')|
|csCookie|W3CIISLog|Soubor cookie informace|
|csReferer|W3CIISLog|Web, který uživatel poslední navštívenou. Tento web k dispozici odkaz pro aktuální web.|
|csHost|W3CIISLog|Host (hostitel) záhlaví (tedy "www.mysite.com"), která byla požadovaná|
|scSubStatus|W3CIISLog|Kód chyby podřízeného stavu|
|scWin32Status|W3CIISLog|Windows stavový kód|
|csBytes|W3CIISLog|Bajtů v žádosti o z klienta na server|
|scBytes|W3CIISLog|Vrátí zpět v odpovědi ze serveru klientovi bajtů|
|ConfigChangeType|ConfigurationChange|Typ změny (WindowsServices / Software / atd)|
|ChangeCategory|ConfigurationChange|Kategorie změnit (změněno / Přidat / odebrat)|
|SoftwareType|ConfigurationChange|Typ softwaru (aktualizace / aplikace)|
|SoftwareName|ConfigurationChange|Název (platí jenom pro změny software) softwaru|
|Aplikace Publisher|ConfigurationChange|Dodavatel, který publikuje software (platí jenom pro změny software)|
|SvcChangeType|ConfigurationChange|Typ změny, který byl použitý na službu systému Windows (stát / StartupType / cestu / Účet_služby) – jenom pro změny služeb Windows|
|SvcDisplayName|ConfigurationChange|Zobrazované jméno službu naposledy změnil|
|SvcName|ConfigurationChange|Název službu naposledy změnil|
|SvcState|ConfigurationChange|Nové (aktuální) stav služby|
|SvcPreviousState|ConfigurationChange|Předchozí známé stav služby (pouze použít ke změně stavu služby)|
|SvcStartupType|ConfigurationChange|Typ spouštění služby|
|SvcPreviousStartupType|ConfigurationChange|Předchozí typ spouštění služby (pouze použít ke změně typ spouštění služby)|
|SvcAccount|ConfigurationChange|Účet služby|
|SvcPreviousAccount|ConfigurationChange|Předchozí účtu služby (pouze použít ke změně účet služby)|
|SvcPath|ConfigurationChange|Cesta k spustitelný soubor službu systému Windows|
|SvcPreviousPath|ConfigurationChange|Předchozí cestu spustitelný soubor pro službu systému Windows (platí jenom ho ke změně)|
|SvcDescription|ConfigurationChange|Popis služby|
|Předchozí|ConfigurationChange|Předchozí stav tento software (nainstalované / není nainstalovaný / předchozí verze)|
|Aktuální hodnota|ConfigurationChange|Nejnovější stav tento software (nainstalované / není nainstalovaný / aktuální verze)|

## <a name="next-steps"></a>Další kroky
Další informace o protokolu hledání:

- Seznámení s [protokolu hledání](log-analytics-log-searches.md) zobrazíte podrobné informace shromážděné řešení.
- Použijte [vlastní pole v protokolu analýzy](log-analytics-custom-fields.md) rozšířit vyhledávání protokolu.
