<properties
    pageTitle="Hledání přihlásit protokolu analýzy | Microsoft Azure"
    description="Hledání protokolu umožňují kombinovat a sladit všechny počítače dat z různých zdrojů ve vašem prostředí."
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
    ms.date="10/10/2016"
    ms.author="banders"/>

# <a name="log-searches-in-log-analytics"></a>Protokol vyhledávání v protokolu analýzy

Základem analýzy protokolu je funkci hledání protokol, který umožňuje kombinovat a sladit všechny počítače dat z různých zdrojů ve vašem prostředí. Řešení se také používá technologii protokolu vyhledávání a pokusí se vrátíte metriky sklopí okolo části určitého problému.

Na stránce hledání můžete vytvořit dotaz a potom při hledání, můžete filtrovat výsledky podle pomocí ovládacích prvků podmínka. Rozšířené dotazy na transformace, sestavy a filtr můžete vytvořit taky na výsledky.

Běžné protokolu vyhledávacích dotazů zobrazit na většině stránkách řešení. V konzole OMS můžete klikněte vedle sebe nebo u jiných položek a zobrazit podrobnosti o položky pomocí funkce hledání protokolu procházení.

V tomto kurzu se projdeme příklady pokrýval se základy při použití protokolu vyhledávání.

Budeme se začínat jednoduché, praktických příkladech a pak vytvoříte na nich, abyste mohli udělat pochopení případy praktické použití o tom, jak extrahovat přehledech, které chcete z dat pomocí následující syntaxe.

Poté, co jste zkušenosti s způsobů vyhledávání, můžete zkontrolovat [analýzy protokol přihlášení hledání odkaz](log-analytics-search-reference.md).

## <a name="use-basic-filters"></a>Použití základních filtrů

První věc vědět, je, že první část hledání dotazu před některou "|" svislé znakem, je vždy *Filtr*. Si můžete představit ho jako klauzuli WHERE TSQL – určí, *Jaký* podmnožinu dat můžete načítat mimo OMS úložiště. Hledání v úložišti dat je převážně o zadání vlastnosti data, která chcete extrahovat, takže je přirozený dotazu by začínat klauzule WHERE.

Základní filtry, které můžete použít jsou *klíčová slova*, například "Chyba" nebo "časový limit" nebo název počítače. Tyto typy jednoduchých obecně vrací různorodého obrazce dat v rámci sady stejný výsledek. Je to proto analýzy protokolu má různé *typy* dat v systému.


### <a name="to-conduct-a-simple-search"></a>Provádět jednoduché hledání
1. Na portálu OMS klikněte na **Hledat protokolu**.  
    ![dlaždice hledání](./media/log-analytics-log-searches/oms-overview-log-search.png)
2. V poli dotaz zadejte `error` a potom klikněte na **Hledat**.  
    ![Chyba vyhledávání](./media/log-analytics-log-searches/oms-search-error.png)  
    Příklad dotazu pro `error` na následujícím obrázku vrácené 100 000 záznamů **události** (shromážděná Správa protokolu), 18 **ConfigurationAlert** záznamů (generované konfigurace hodnocení) a 12 **ConfigurationChange** záznamů (zachycených sledování změn).   
    ![výsledky hledání](./media/log-analytics-log-searches/oms-search-results01.png)  

Tyto filtry nejsou skutečně typy/třídy objektů. *Typ* je jenom značku vlastnost nebo řetězec/název/kategorii, který je připojen k část dat. Některé dokumenty systému jsou označeny jako **Typ: ConfigurationAlert** a některé jsou označeny jako **Typ: výkon**nebo **Typ: událost**a tak dále. Jednotlivé výsledky hledání, dokumentu, záznam nebo položka zobrazí jako nezpracovaná vlastnosti a jejich hodnoty pro každou z těchto částí dat a můžete tyto názvy polí můžete určit ve filtru, když chcete obnovit jenom ty záznamy v poli mají, dané hodnoty.

*Typ* je pouze pole, které mají všechny záznamy, se liší od jiné pole. To byla založeny na hodnotu pole Typ. Tento záznam bude mít jiného formuláře nebo obrazec. Náhodně **Typ = výkon**, nebo **Typ = události** je také syntaxi, která vám hodit přečíst si k vytvoření dotazu pro události nebo data o výkonu.

Můžete dvojtečka (:) nebo rovnítko (=) za názvem pole a před hodnotu. **Typ: událostí** a **Typ = události** jsou odpovídající významem, můžete zvolit požadovaný styl dáváte přednost.

Ano, pokud typ = výkon záznamy obsahují pole s názvem "Název_čítače" a pak můžete psát dotazu podobné `Type=Perf CounterName="% Processor Time"`.

To vám umožní pouze údaje o výkonu kde název čítače výkonu je "% času procesoru".

### <a name="to-search-for-processor-time-performance-data"></a>Pokud chcete hledat data o výkonu Procesor času
- Dotaz do vyhledávacího pole zadejte`Type=Perf CounterName="% Processor Time"`

Můžete vybrat určité a používat **Název_instance = _ "Celkem"** v dotazu, který je čítače výkonu systému Windows. Můžete také vybrat motivem fazeta a jinou **hodnotu: pole**. Filtr se automaticky přidají do filtru na panelu dotazu. Zobrazí se tato na následujícím obrázku. Zobrazuje umístění na tlačítko Přidat **Název_instance: "_Total"** dotazu i bez zadání nic.

![podmínka vyhledávání](./media/log-analytics-log-searches/oms-search-facet.png)

Dotaz nyní změní`Type=Perf CounterName="% Processor Time" InstanceName="_Total"`

V tomto příkladu nemusíte zadat **Typ = výkon** přejděte na tento výsledek. Protože pole název_čítače a InstanceName existovat pouze pro záznamy typu = výkon, dotaz je dostatečně specifické pro vrátí stejné výsledky jako delší a předchozí relace:
```
CounterName="% Processor Time" InstanceName="_Total"
```

Je to proto všech filtrů v dotazu jsou vyhodnoceny jako *a* navzájem. Efektivní, více polí přidáte kritéria, dostanete menší, konkrétní a kontrast výsledky.

Příklad dotazu `Type=Event EventLog="Windows PowerShell"` přesně odpovídá `Type=Event AND EventLog="Windows PowerShell"`. Vrátí všechny události, které byly přihlášení a odebrané v protokolu událostí systému Windows PowerShell. Přidáte-li filtr tisknutím opakovaně výběrem stejné podmínka řešení tohoto problému je čistě symbolické – a pak ji může složky nepotřebné panelem hledání, ale pořád vrátí stejné výsledky implicitní operátoru je vždy tam.

Implicitní a operátor snadno vrátíte pomocí operátor NOT explicitně. Příklad:

`Type:Event NOT(EventLog:"Windows PowerShell")`nebo ekvivalent `Type=Event EventLog!="Windows PowerShell"` vrátit všechny události ze všech protokolů, které nejsou protokol prostředí Windows PowerShell.

Nebo můžete použít jiné logický operátor jako "Nebo". Následující dotaz vrací záznamy, ke kterým protokolu událostí systému nebo buď aplikace.

```
EventLog=Application OR EventLog=System
```

Pomocí výše uvedený dotaz, zobrazí se položky pro obě protokolů ve stejném sadu výsledků.

Ale pokud odeberete poli se Seznamem necháte implicitní a v místě, pak následující dotaz nevytvoří žádné výsledky protože není k dispozici v protokolu událostí položky, která patří oba protokoly. Každá položka v protokolu událostí vytvořené pomocí pouze jednu ze dvou protokoly.

```
EventLog=Application EventLog=System
```


## <a name="use-additional-filters"></a>Použít další filtry

Následující dotaz vrátí položky 2 protokoly událostí u všech počítačů, které jste poslali data.

```
EventLog=Application OR EventLog=System
```

![výsledky hledání](./media/log-analytics-log-searches/oms-search-results03.png)

Výběrem jedné z polí nebo filtry zúží dotazu k určitému počítači, s výjimkou všechny ostatní z nich. Výsledný dotaz bude vypadat takto.

```
EventLog=Application OR EventLog=System Computer=SERVER1.contoso.com
```

Které odpovídá následující akci z důvodu implicitní AND.

```
EventLog=Application OR EventLog=System AND Computer=SERVER1.contoso.com
```

V následujícím pořadí explicitní je vyhodnocen obou dotazů. Poznámka: v závorkách.

```
(EventLog=Application OR EventLog=System) AND Computer=SERVER1.contoso.com
```

Stejně jako pole v protokolu událostí můžete data načtete jenom pro sadu určitých počítačů přidáním nebo. Příklad:

```
(EventLog=Application OR EventLog=System) AND (Computer=SERVER1.contoso.com OR Computer=SERVER2.contoso.com OR Computer=SERVER3.contoso.com)
```

Podobně tento následující dotaz vrátí **% časového využití procesoru** pouze vybrané dvou počítačů.

```
CounterName="% Processor Time"  AND InstanceName="_Total" AND (Computer=SERVER1.contoso.com OR Computer=SERVER2.contoso.com)
```


### <a name="boolean-operators"></a>Logické operátory
Data a času a číselná pole, můžete hledat hodnoty pomocí *větší než* *menší než*, a *menší nebo rovna*. Můžete použít jednoduchý operátory jako >, <>; =, < =,! = panelu hledání dotazu.


Dotaz konkrétního protokolu událostí na určité časové období. Například následující výraz mnemonické vyjádřený posledních 24 hodin.

```
EventLog=System TimeGenerated>NOW-24HOURS
```


#### <a name="to-search-using-a-boolean-operator"></a>Pokud chcete hledat použití logických operátorů
- Dotaz do vyhledávacího pole zadejte`EventLog=System TimeGenerated>NOW-24HOURS"`  
    ![hledání s logická hodnota](./media/log-analytics-log-searches/oms-search-boolean.png)

Přestože můžete určit časový interval graficky a většině případů, můžete chtít udělat, jsou výhody včetně filtrem čas přímo do dotazu. Například to funguje skvěle s odkazem řídicí panely, které je možné přepsat času pro každou dlaždici, bez ohledu na volič *globální* času na stránku řídicího panelu. Další informace najdete v tématu [Otázky čas na řídicím panelu](http://cloudadministrator.wordpress.com/2014/10/19/system-center-advisor-restarted-time-matters-in-dashboard-part-6/).

Při filtrování podle času, vezměte v úvahu získat výsledky pro *průnik* dvou časová období: je uvedeno v portálu OMS (S1) a je uvedeno v dotazu (S2).

![průnik](./media/log-analytics-log-searches/oms-search-intersection.png)

To znamená, když časová období neprotínají, třeba na portálu OMS místo, kam se rozhodnete **Tento týden** a v dotazu, kde můžete definovat **minulý týden**, pak je průsečík a nebudete dostávat žádné výsledky.

Relační operátory pro pole TimeGenerated jsou také užitečné v ostatních případech. Například s číselná pole.

Například je dostali, Upozornění konfigurace hodnocení máte následující závažnosti hodnoty:

- 0 = informace
- 1 = upozornění
- 2 = kritický

Můžete vyhledat pro upozornění na upozornění a kritické a také vyloučit informační z nich se následující dotaz:

```
Type=ConfigurationAlert  Severity>=1
```


Můžete taky dotazy oblast. To znamená, že je zadat začátek a konec rozsah hodnot v pořadí. Například pokud budete potřebovat události z protokolu událostí Operations Manager, kde EventID je větší než nebo rovno 2100, ale není větší než. 2199, pak následující dotaz vrátí je.

```
Type=Event EventLog="Operations Manager" EventID:[2100..2199]
```


>[AZURE.NOTE] Syntaxe oblast musíte použít je hodnota pole: oddělovač dvojtečka (:) a *nikoli* rovná se (=). Uzavření horní a dolní konec rozsahu do hranatých závorek a oddělte je s dvěma obdobími (.).

## <a name="manipulate-search-results"></a>Pracovat s výsledky hledání

Když hledáte data, je vhodné k upřesnění vyhledávání a dobré úroveň kontrolu nad výsledky. Při načtení výsledky, můžete použít příkazy pro transformací.

Příkazy v protokolu analýzy hledání, *musí* sledovat po kanálu svislé čáry (|). Filtr musí být vždy první část řetězce dotazu. Definuje uvedenou množinu dat, se kterou pracujete s a potom "potrubí" těchto výsledků do příkazu. Chcete-li přidat další příkazy můžete kanálu. Toto je volně podobný kanálu prostředí Windows PowerShell.

Obecně jazyk vyhledávání protokolu analýzy se snaží prostředí PowerShell styl a pokyny snažíme usnadnit jeho podobně jako oboru IT a usnadnit křivky výuka podle.

Příkazy mají názvy akcí, takže můžete snadno zjistit, co dělat.  

### <a name="sort"></a>Řazení

Příkaz Seřadit umožňuje určit pořadí řazení podle jednoho nebo více polí. I když nepoužíváte, ve výchozím nastavení, čas sestupně vynucené. Posledních výsledky jsou vždy v horní části výsledky hledání. To znamená, že při spuštění vyhledávání s `Type=Event EventID=1234` co je pro vás skutečně spouštět je:

```
Type=Event EventID=1234 **| Sort TimeGenerated desc**
```

Je proto, že je typ prostředí, které znáte v protokolech. Například v prohlížeči událostí systému Windows.

Chcete-li změnit způsob, jakým výsledky jsou vráceny můžete seřadit. Následující příklady ukazují, jak to funguje.

```
Type=Event EventID=1234 | Sort TimeGenerated asc
```

```
Type=Event EventID=1234 | Sort Computer asc
```

```
Type=Event EventID=1234 | Sort Computer asc,TimeGenerated desc
```


Jednoduchý výše uvedené příklady ukazují, fungování příkazy – Změna obrazce výsledky, které vracejí filtr.

### <a name="limit-and-top"></a>Omezení a nahoře
Další méně známé příkaz je OMEZENÝ. Limit je slovesné profesionálové Powershellu. Limit je stejně jako začátek příkaz. Následující dotazy vracet stejných výsledků.

```
Type=Event EventID=600 | Limit 1
```

```
Type=Event EventID=600 | Top 1
```


#### <a name="to-search-using-top"></a>Hledání s použitím horní
- Dotaz do vyhledávacího pole zadejte`Type=Event EventID=600 | Top 1`   
    ![horní hledání](./media/log-analytics-log-searches/oms-search-top.png)

Na obrázku nahoře, jsou záznamy 358 1000 s EventID = 600. Pole, fasetami a filtry na levé straně vždy zobrazit informace o výsledky vrácené *části Filtr* dotazu, který je součástí před libovolnému znaku kanálu. Podokno **výsledků** pouze Umocní posledních 1, protože příklad ve tvaru a transformovat výsledky.

### <a name="select"></a>Vyberte

Příkaz SELECT se chová jako objekt vyberte v prostředí PowerShell. Vrátí filtrovaných výsledků, které nejsou k dispozici všechny své původní vlastnosti. Místo toho vybere pouze vlastnosti, které zadáte.

#### <a name="to-run-a-search-using-the-select-command"></a>Spusťte vyhledávání pomocí příkazu select

1. Do pole hledání zadejte `Type=Event` a potom klikněte na **Hledat**.
2. Klikněte na tlačítko **+ Zobrazit další** v jednom z výsledků můžete zobrazit všechny vlastnosti, které mají výsledky.
3. Vyberte některé z těchto explicitně a dotaz se změní na `Type=Event | Select Computer,EventID,RenderedDescription`.  
    ![Vyberte hledání](./media/log-analytics-log-searches/oms-search-select.png)

Toto je příkaz užitečné, když budete chtít řízení výstupu vyhledávání a zvolte pouze části dat, které skutečně důležité informace, které často není úplný záznam. To je také užitečné při záznamy různých typů obsahují *některé* běžné vlastnosti, ale ne *všech* jejich vlastností běžné. Můžete generovat výstup snadnější vypadá tabulky nebo můžete pracovat, i když exportovat do souboru CSV a potom massaged v aplikaci Excel.



## <a name="use-the-measure-command"></a>Použití příkazu míra

MÍRA je jednou z nejčastěji univerzální příkazy v protokolu analýzy vyhledávání. Umožňuje použít statistických *funkcí* dat a agregovat výsledky seskupené tak, že dané pole. Existuje více statistických funkcí, které podporuje míra.

### <a name="measure-count"></a>Míra count()

První statistických funkcí a práce s z nejjednodušší pochopit je funkce *count()* .

Výsledky z libovolné vyhledávacího dotazu jako `Type=Event`, zobrazit filtry taky se mu říká fasetami na levé straně výsledků hledání. Filtry zobrazení rozdělení množiny hodnot tak, že dané pole výsledků hledání spouštět.

![počítání míra hledání](./media/log-analytics-log-searches/oms-search-measure-count01.png)

Například na obrázku nahoře uvidíte pole **počítače** a ho ukazuje, že v rámci téměř 739 1000 události ve výsledcích jsou 68 jedinečný a odlišné hodnoty pro pole **počítače** v těchto záznamech. Dlaždici pouze ukazuje, horní 5, která jsou nejčastější 5 hodnoty napsané v polích **počítač** ), seřazené podle počet dokumentů, které obsahují určité hodnotu do tohoto pole. Na obrázku uvidíte, že – mezi tyto téměř 369 1000 události – 90 1000 pocházet z počítače OpsInsights04.contoso.com 83 1000 z počítače DB03.contoso.com a tak dál.


Co dělat, když chcete zobrazit všechny hodnoty, protože na dlaždici se zobrazí, jenom pouze 5 hlavních?

Je to, co dělat s funkcí count() příkazu míra. Tato funkce nepoužívá všechny parametry. Stačí zadat pole, podle kterého chcete seskupit – pole **počítače** v tomto případě:

`Type=Event | Measure count() by Computer`

![počítání míra hledání](./media/log-analytics-log-searches/oms-search-measure-count-computer.png)

Však **počítače** je právě pole použité *v* každou část dat – zahrnuje žádné relační databáze a že žádné zvláštní objekt **počítače** kdekoli. Jenom hodnot *v* datech můžete popsat, které entity generovaného je a řadu dalších vlastností a aspekty dat – tedy termínů *Podmínka*. Však můžete taky jenom seskupovat data podle dalších polí. Vzhledem k tomu původní výsledků prakticky 739 1000 události, které jsou přesměrovat do příkazu míra také pole s názvem **EventID**, můžete použít stejné techniky Seskupit podle tohoto pole a získat počet soutěží v jednotlivých EventID:

```
Type=Event | Measure count() by EventID
```

Pokud vás zajímá není počet skutečné záznamů, které obsahují určité hodnotě, ale místo toho požadovaná pouze seznam hodnot, sami, můžete přidat příkaz *Select* na konci od něj a právě vyberte první sloupec:

```
Type=Event | Measure count() by EventID | Select EventID
```

Pak můžete získat další komplikované a prvky můžete předem setřídit výsledky v dotazu nebo příliš můžete sloupce v tabulce, stačí kliknout.

```
Type=Event | Measure count() by EventID | Select EventID | Sort EventID asc
```

#### <a name="to-search-using-measure-count"></a>Hledání s použitím počítání míra

- Dotaz do vyhledávacího pole zadejte`Type=Event | Measure count() by EventID`
- Připojení `| Select EventID` za účelem dotaz.
- Nakonec připojit `| Sort EventID asc` za účelem dotaz.


Existuje několik důležitých aspektů, které Všimněte si a zdůraznit:

Nejdřív výsledky, které uvidíte nejsou původní nezpracovanými výsledky už. Místo toho, jsou souhrnné výsledky – v podstatě skupin výsledků. To není problém, ale je třeba si uvědomit, že jste interakce s obrazcem velmi liší dat, která se liší od původní nezpracovanými obrazce, vytvořenou získá rychlé úpravy prostřednictvím funkce agregace/statistické.

Za druhé **počítání míra** aktuálně vrátí jenom 100 nejvyšších odlišné výsledky. Toto omezení, nebudou mít vliv na jiné statistických funkcí. Ano obvykle musíte nejdřív pomocí přesnější filtru můžete hledat konkrétní položky před použitím míra count().

## <a name="use-the-max-and-min-functions-with-the-measure-command"></a>Funkce min a max pomocí příkazu míra

Existují různé scénáře, kde jsou užitečné **Míra Max()** a **Měření Min()** . Protože jednotlivých funkcí je opačný navzájem, jsme budete ilustrují Max() a můžete experimentovat s Min() si vlastní.

Pokud dotaz na události zabezpečení mají **úroveň** vlastnost, která se může lišit. Příklad:

```
Type=SecurityEvent
```

![hledání míra počet start](./media/log-analytics-log-searches/oms-search-measure-max01.png)

Pokud chcete zobrazit nejvyšší hodnota u všech zabezpečení události zadaných běžné počítač seskupit podle pole, můžete použít

```
Type=ConfigurationAlert | Measure Max(Level) by Computer
```

![Vyhledání míra maximální počítače](./media/log-analytics-log-searches/oms-search-measure-max02.png)

Bude zobrazení pro počítače, ve kterých **úrovni** záznamů, většinu z nich že minimální úroveň 8, mnoho měli úrovně 16.

```
Type=ConfigurationAlert | Measure Max(Severity) by Computer
```

![maximální dobu hledání míra generovaného počítače](./media/log-analytics-log-searches/oms-search-measure-max03.png)

Tato funkce funguje dobře s čísly, fungují ale i u polí data a času. Slouží k vyhledání poslední nebo posledních časové razítko libovolného dat indexovat pro každý počítač. Příklad: Pokud byl posledních událost zabezpečení vykázaného za jednotlivé počítače?

```
Type=ConfigurationChange | Measure Max(TimeGenerated) by Computer
```

## <a name="use-the-avg-function-with-the-measure-command"></a>Funkce avg pomocí příkazu míra

Statistických funkcí Avg() použít s míra umožňuje vypočítá průměrnou hodnotu některé pole a seskupení výsledků podle pole stejné nebo jiné. To je užitečné v mnoha případech, například data o výkonu.

Začneme bude s data o výkonu. Všimněte si, že OMS aktuálně shromažďuje výkonnosti u počítačů s Windows a Linux.

Vyhledat *všechny* data o výkonu, je základní dotazu:

```
Type=Perf
```

![zahájení hledání avg](./media/log-analytics-log-searches/oms-search-avg01.png)

První věc, všimnete si je, že protokolu analýzy uvidíte tři perspektivy: seznam, který ukazuje, která zobrazuje skutečné záznamy za grafech; Tabulka, která zobrazuje tabulkového zobrazení jeho výkon údaje; a metriky, která se zobrazí graf výkonnosti.

Na obrázku nahoře jsou dvě sady pole označená, které označují následující:

- První sadě identifikuje název čítače výkonu systému Windows, název objektu a název Instance filtru dotazu. Jedná se o pole, které pravděpodobně nejčastěji použijete jako fasetami/filtry
- **Přepočtené** aj je skutečná hodnota čítače. V tomto příkladu je hodnotě *75*.
- **TimeGenerated** je 12:51 ve formátu času 24 hodin.

Tady je přehled metriky v grafu.

![zahájení hledání avg](./media/log-analytics-log-searches/oms-search-avg02.png)

Po čtení o obrazci záznamu výkon a máte další informace o jiných způsobech hledání můžete míra Avg() agregovat tento typ číselná data.

Tady je jednoduchý příklad:

```
Type=Perf  ObjectName:Processor  InstanceName:_Total  CounterName:"% Processor Time" | Measure Avg(CounterValue) by Computer
```

![hledání avg samplevalue](./media/log-analytics-log-searches/oms-search-avg03.png)

V tomto příkladu vyberte výkonu celkový čas procesoru čítač a průměr ve počítače. Pokud chcete omezit výsledky pouze 6 hodin, lze použít ovládací prvek filtru čas nebo zadat dotaz následujícím způsobem:

```
Type=Perf  ObjectName:Processor  InstanceName:_Total  CounterName:"% Processor Time" TimeGenerated>NOW-6HOURS | Measure Avg(CounterValue) by Computer
```

### <a name="to-search-using-the-avg-function-with-the-measure-command"></a>Hledání s použitím funkce avg pomocí příkazu míra
- Dotaz do vyhledávacího pole zadejte `Type=Perf  ObjectName:Processor  InstanceName:_Total  CounterName:"% Processor Time" TimeGenerated>NOW-6HOURS | Measure Avg(CounterValue) by Computer`.


Můžete agregovat a sladit dat *ve* počítače. Představte si, například, že máte sadu hosts v některých seřadit farmy kde jednotlivých uzlech je rovno jiných některý jenom stejné zadají práce a zhruba Vyrovnávání zatížení. Můžete získat jejich čítačů všechny v jednom přesměrovány s tímto dotazu a získání průměry pro celou farmu. Můžete začít výběrem počítačích s následujícím příkladu:

```
Type=Perf AND (Computer="AzureMktg01" OR Computer="AzureMktg02" OR Computer="AzureMktg03")
```

Teď, když máte počítači, taky chcete vybrat dva klíčové ukazatele výkonu (KPI): vytížení procesoru a % volného místa na disku. Ano bude tuhle část Dotaz:

```
Type=Perf InstanceName:_Total  ((ObjectName:Processor AND CounterName:"% Processor Time") OR (ObjectName="LogicalDisk" AND CounterName="% Free Space")) AND TimeGenerated>NOW-4HOURS
```

Teď můžete přidat počítače a čítače s následujícím příkladu:

```
Type=Perf InstanceName:_Total  ((ObjectName:Processor AND CounterName:"% Processor Time") OR (ObjectName="LogicalDisk" AND CounterName="% Free Space")) AND TimeGenerated>NOW-4HOURS AND (Computer="AzureMktg01" OR Computer="AzureMktg02" OR Computer="AzureMktg03")
```

Protože nemáte specifických výběr příkazu **Míra Avg()** se vrátit průměr není ve počítače, ale ve farmě, jednoduše seskupování podle název_čítače. Příklad:

```
Type=Perf  InstanceName:_Total  ((ObjectName:Processor AND CounterName:"% Processor Time") OR (ObjectName="LogicalDisk" AND CounterName="% Free Space")) AND TimeGenerated>NOW-4HOURS AND (Computer="AzureMktg01" OR Computer="AzureMktg02" OR Computer="AzureMktg03") | Measure Avg(CounterValue) by CounterName
```

To vám užitečné Kompaktní zobrazení několika vašeho prostředí klíčových ukazatelů výkonu.

![hledání avg seskupení](./media/log-analytics-log-searches/oms-search-avg04.png)


Můžete snadno vyhledávacího dotazu v řídicím panelu. Můžete třeba hledání uložit a vytvoření řídicího panelu z něho s názvem *Webu farmy klíčových ukazatelů výkonu*. Další informace o používání řídicích panelech najdete v tématu [Vytvoření vlastní řídicí panel v protokolu analýzy](log-analytics-dashboards.md).

![řídicí panel hledání avg](./media/log-analytics-log-searches/oms-search-avg05.png)

### <a name="use-the-sum-function-with-the-measure-command"></a>Pomocí funkce SUMA pomocí příkazu míra

Funkce SUMA funkce je podobná jiných příkazu míra. Zobrazí příklad o tom, jak pomocí funkce SUMA na [W3C IIS protokoly hledání v Microsoft Azure provozní přehledy](http://blogs.msdn.com/b/dmuscett/archive/2014/09/20/w3c-iis-logs-search-in-system-center-advisor-limited-preview.aspx).

Můžete Max() a Min() s čísla, data a času a textové řetězce. Textové řetězce jsou seřazená abecedně a získat první a poslední.

Však nelze použít Sum() s jinou hodnotu než číselná pole. To se týká rovněž k Avg().

### <a name="use-the-percentile-function-with-the-measure-command"></a>Funkce PERCENTIL pomocí příkazu míra

Funkce PERCENTIL podobná Avg() a Sum() v tom, můžete ho použít jenom u číselných polí. Můžete použít libovolný percentilu mezi 1 do 99 na číselné pole. Můžete taky použít příkazy **percentilu** a **procenta** . Tady je několik příkladů:  

```
Type:Perf CounterName:"DiskTransers/sec" |measure percentile95(CurrentValue) by Computer
```
```
Type:Perf ObjectName=LogicalDisk CounterName="Current Disk Queue Length" Computer="MyComputerName" | measure pct65(CurrentValue) by InstanceName
```

## <a name="use-the-where-command"></a>Použití where příkazu

-Li příkaz funguje jako filtr, ale můžete použít v kanálu dál filtrovat souhrnné výsledky, které byly vytvořené pomocí příkazu míra – namísto nezpracovanými výsledky, které jsou filtrovány na začátku tohoto dotazu.

Příklad:

```
Type=Perf  CounterName="% Processor Time"  InstanceName="_Total" | Measure Avg(CounterValue) as AVGCPU by Computer
```

Můžete přidat další kanálu "|" znak a kam příkaz pouze získat počítačů, jejichž průměr CPU je vyšší než 80 %, v následujícím příkladu:

```
Type=Perf  CounterName="% Processor Time"  InstanceName="_Total" | Measure Avg(CounterValue) as AVGCPU by Computer | Where AVGCPU>80
```

Pokud znáte Microsoft System Center - Operations Manager si můžete představit příkazu where řečeno management pack. Kdyby příkladu pravidla, první část Dotaz bude zdroje dat a kde příkaz bude zjišťování stavu.

Můžete dotaz jako dlaždici v **Vlastní řídicí panel**jako monitor řazení a zjistit, jestli jsou přetíženy procesory počítače. Další informace o řídicích panelech najdete v tématu [Vytvoření vlastní řídicí panel v protokolu analýzy](log-analytics-dashboards.md). Můžete taky vytvořit a používat řídicí panely pomocí mobilní aplikace. Další informace najdete v tématu [OMS mobilní aplikaci ](http://www.windowsphone.com/en-us/store/app/operational-insights/4823b935-83ce-466c-82bb-bd0a3f58d865). V dolní dvě dlaždice na následujícím obrázku, zobrazí se monitor zobrazí seznam a jako číslo. V podstatě vždy má číslo je nula a seznam na prázdné. V opačném označuje podmínku upozornění. V případě potřeby, můžete ji použijete k podívejte se na kterých počítačích jsou stlačení.

![mobilní řídicího panelu](./media/log-analytics-log-searches/oms-search-mobile.png)

## <a name="use-the-in-operator"></a>Použijte operátor in

Operátor *IN* , spolu s *NOT IN* umožňuje subsearches, které jsou výsledky hledání, které obsahují jiné hledání jako argument. Jsou obsaženy v složených závorek {} v rámci jiné *primární* nebo *vnější* vyhledávání. Výsledek subsearch často seznam odlišné výsledky, je pak použít jako argument v jeho primární vyhledávací.

Můžete subsearches podle podmnožin dat, nejde popisují přímo v hledaný výraz, ale kterého lze vytvořit z hledání. Například pokud vás zajímá pomocí jednoho vyhledávání zobrazíte všechny události z *počítače chybějící aktualizace zabezpečení*, pak musíte navrhnout subsearch identifikující nejdřív tohoto *počítače chybějící aktualizace zabezpečení* před uvedenu události patřící do těchto hostitelů.

Ano může express *počítačů aktuálně chybějící požadované aktualizace zabezpečení* se následující dotaz:

```
Type:Update UpdateState=Needed Optional=false Classification="Security Updates" TimeGenerated>NOW-24HOURS | measure count() by Computer
```    

![V příkladu hledání](./media/log-analytics-log-searches/oms-search-in01-revised.png)

Až budete mít v seznamu, můžete hledat vnitřní vyhledávání k naplnění seznamu počítačů do vnější (primární) hledání, které vyhledá událostech těchto počítačů. To uděláte tak, že orámování vnitřní vyhledávání do složených závorek a podávání jeho výsledky jako možnými hodnotami a pole filtru v vnější vyhledávání pomocí operátor. Dotaz by mohl vypadat například:

```
Type=Event Computer IN {Type:Update UpdateState=Needed Optional=false Classification="Security Updates" TimeGenerated>NOW-24HOURS | measure count() by Computer}
```
![V příkladu hledání](./media/log-analytics-log-searches/oms-search-in02-revised.png)


Také oznámení filtru čas použít vnitřní hledat, protože systém hodnocení aktualizace trvá snímek všech počítačů každých 24 hodin. Můžete pak dělat vnitřní dotaz lightweight a přesné vyhledáním jenom na den. Vnější hledání použije výběr časového v uživatelském rozhraní načítání událostí z posledních 7 dnech. Další informace o době operátory najdete v článku [logické operátory](#boolean-operators) .

Vzhledem k tomu budete skutečně pouze výsledky hledání vnitřní jako filtr hodnota pro vnější tu, můžete také použít příkazy vnější Hledat. Například můžete pořád seskupujete výše událostí pomocí jiného míra příkazu:

```
Type=Event Computer IN {Type:Update UpdateState=Needed Optional=false Classification="Security Updates" TimeGenerated>NOW-24HOURS | measure count() by Computer} | measure count() by Source
```

![V příkladu hledání](./media/log-analytics-log-searches/oms-search-in03-revised.png)


Obecně vzato chcete vnitřní dotaz spustit rychle, protože analýzy protokolu má časové limity straně služby pro něj a vrátíte malou výsledků. Pokud vnitřní dotaz vrátí více výsledků, dostane zkrácený seznamu výsledků, který může způsobit vnější hledání tak, aby vracel nesprávné výsledky.

Další pravidlo je, že vnitřní hledání aktuálně musí poskytovat *souhrnné* výsledky. Jinými slovy musí obsahovat *Míra* příkaz; jako nezpracovaná výsledky nelze kanálu aktuálně na vnější vyhledávání.

Navíc může být pouze jeden operátor IN a musí být poslední filtr v dotazu. Nesmí být více operátorů ve nebo by – to v podstatě zabrání spuštění více subsearches: důležité je pouze jeden sub/vnitřní hledání je možné pro každé vnější vyhledávání.

I tyto limity v umožňuje různé druhy vzájemném vztahu vyhledávání a umožňuje definovat podobnou skupin například počítačů, uživatelů a soubory – bez ohledu pole ve vašich datech obsahují. Tady jsou další příklady:

**Všechny aktualizace chybí počítačích, kde je zakázán nastavení automatických aktualizací**

```
Type=Update UpdateState=Needed Optional=false Computer IN {Type=UpdateSummary WindowsUpdateSetting=Manual | measure count() by Computer} | measure count() by KBID
```

**Všechny chyby události z počítače se systémem SQL Server (= místo, kam má spustit hodnocení SQL)**

```
Type=Event EventLevelName=error Computer IN {Type=SQLAssessmentRecommendation | measure count() by Computer}
```

**Všechny události zabezpečení z počítače, které jsou řadiče domény se (= místo, kam má AD vyhodnocování spustit)**

```
Type=SecurityEvent Computer IN { Type=ADAssessmentRecommendation | measure count() by Computer }
```

**Které dalších účtů jste přihlášeni do stejné počítačů místo, kam má účet BACONLAND\jochan přihlášen?**

```
Type=SecurityEvent EventID=4624   Account!="BACONLAND\\jochan" Computer IN { Type=SecurityEvent EventID=4624   Account="BACONLAND\\jochan" | measure count() by Computer } | measure count() by Account
```

## <a name="use-the-distinct-command"></a>Použití příkazu distinct

Jak naznačuje název tohoto příkazu obsahuje seznam různých hodnot pro pole. Je překvapivě jednoduché, ale velmi užitečné. Stejný lze dosáhnout pomocí příkazu count() míra stejně, jak je ukázáno v následujícím příkladu.

```
Type=Event | Measure count() by Computer
```

![Příklad příkazu DISTINCT hledání](./media/log-analytics-log-searches/oms-search-distinct01-revised.png)

Pokud však všechny máte zájem je jenom seznam různých hodnot a ne počet různých hodnot dokumentů, které máte, můžete zadat hodnoty a pak DISTINCT čistší a snadněji číst výstup kratší syntaxe, jak je znázorněno níže.

```
Type=Event | Distinct Computer
```
![Příklad příkazu DISTINCT hledání](./media/log-analytics-log-searches/oms-search-distinct02-revised.png)

## <a name="use-the-countdistinct-function-with-the-measure-command"></a>Použít funkci countdistinct pomocí příkazu míra
Funkce countdistinct spočítá počet různých hodnot v rámci jednotlivých skupin. Například může být použít ke zjištění počet jedinečných vytváření sestav pro každý typ:

```
* | measure countdistinct(Computer) by Type
```

![OMS countdistinct](./media/log-analytics-log-searches/oms-countdistinct.png)

## <a name="use-the-measure-interval-command"></a>Použití příkazu míra interval
S poblíž shromažďování dat v reálném čase výkonu, můžete shromažďovat a vizualizovat všechny čítače v protokolu analýzy. Jednoduše zadání dotazu, který **Typ: výkon** vrátí tisíce metrických grafy na základě počtu čítačů a servery v prostředí protokol analýzy. S okamžitou metrických agregace můžete si může samostatně prohlížet celkové metriky ve vašem prostředí na vysoké úrovni a hloubkové postupy na podrobnější data, jako je třeba.

Řekněme, že budete chtít zjistit, co je průměrná procesoru mezi vašimi počítači. Prohlížíte průměr procesoru pro každý počítač nemusí být užitečné, protože mohou získat Vyhladit výsledky. Pokud chcete najít do víc informací, můžete příkladu agregujete výsledek v menší čas okno bloky a podívejte se na do časovou řadu. přes různé rozměry. Například můžete dělat hodinové průměr využití procesoru mezi vašimi počítači následujícím způsobem:

```
Type:Perf CounterName="% Processor Time" InstanceName="_Total" | measure avg(CounterValue) by Computer Interval 1HOUR
```

![míra průměr interval](./media/log-analytics-log-searches/oms-measure-avg-interval.png)

Ve výchozím nastavení těchto výsledky zobrazí se ve více řadami interaktivní spojnicovém grafu.  Tento graf podporuje řady přepínání (s osou y změny měřítka), tvořené a umístíte ukazatel myši.  Možnost zobrazení tabulky je pořád dostupná kvůli zobrazení nezpracovanými daty v případě potřeby.

Taky můžete seskupovat data podle dalších polí. V tomto příkladu mám hledat v % peaks pro jeden počítač konkrétní a budu chtít vědět, co je percentily hodinové 70 každé čítače:

```
Type:Perf Computer=beefpatty4 CounterName=%* InstanceName=_Total | measure percentile70(CounterValue) by CounterName Interval 1HOUR
```
Poznámka je, že tyto dotazy není omezena na výkonnosti. Je můžete použít na všechny metrické. V tomto příkladu mám hledat na serveru IIS W3C protokoly. Budu chtít vědět, co je maximální časové náročnosti přes 5 minut pro zpracování každý požadavek:

```
Type:W3CIISLog | measure max(TimeTaken) by csMethod Interval 5MINUTES
```

### <a name="use-multiple-aggregates-in-one-query"></a>Použití více agregace v jednom dotazu
V příkazu míra určit více agregační klauzulí.  Každý z nich lze alias nezávisle na sobě.  Pokud není možnost alias výsledný název pole budou agregační funkci, která byla použita (tedy "avg(CounterValue)" pro avg(CounterValue)).

 ```
Type=WireData | measure avg(ReceivedBytes), avg(SentBytes) by Direction interval 1hour
```
![OMS multiaggregates1](./media/log-analytics-log-searches/oms-multiaggregates1.png)

Tady máme jiný příklad:
 ```
* | measure countdistinct(Computer) as Computers, count() as TotalRecords by Type
```


## <a name="next-steps"></a>Další kroky

Další informace o protokolu hledání najdete v tématu:

- Použijte [vlastní pole v protokolu analýzy](log-analytics-custom-fields.md) rozšířit vyhledávání protokolu.
- Prohlédněte si [analýzy protokol přihlášení odkaz hledání](log-analytics-search-reference.md) můžete zobrazit všechny prohledávat a fasetami k dispozici v protokolu analýzy.
