<properties
    pageTitle="Jak zvolit algoritmy učení stroje | Microsoft Azure"
    description="Jak zvolit algoritmy učení Azure Machine pro učení pod dohledem a závadou v experimentech clustering klasifikace a regrese."
    services="machine-learning"
    documentationCenter=""
    authors="brohrer"
    manager="jhubbard"
    editor="cgronlun"
    tags=""/>
    
<tags
    ms.service="machine-learning"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="data-services"
    ms.date="08/09/2016"
    ms.author="brohrer;garye" />

# <a name="how-to-choose-algorithms-for-microsoft-azure-machine-learning"></a>Jak zvolit algoritmy pro Microsoft Azure Machine Learning

Odpověď na otázku "Jaký počítač učení algoritmus použít?" je vždy "Záleží." Záleží na velikosti, kvality a povaze údajů. Záleží, co chcete udělat s odpovědí. To závisí na jak Matematika algoritmu byla přeložena do pokynů pro počítač, který používáte. A záleží na jak dlouho je k dispozici. I nejzkušenější vědců dat nelze zjistit, který algoritmus provede před pokusem je nejlepší.

## <a name="the-machine-learning-algorithm-cheat-sheet"></a>Cheaty na strojovém učení algoritmus list

**Microsoft Azure Machine učení algoritmus cheaty list** umožňuje vybrat pravý stroj učení algoritmus pro prognostické analýzy řešení z knihovny Microsoft Azure Machine studium algoritmů.
Tento článek vás provede jak ji používat.

> [AZURE.NOTE] Stáhnout list podvést a postupovat podle tohoto článku, pokračujte [učení algoritmus podvést list pro Microsoft Azure Machine výukové Studio počítače](machine-learning-algorithm-cheat-sheet.md).

Tento podvést list má velmi specifické cílové skupiny v paměti: začátek data vědci se studentům úroveň machine learning pokusu začít zvolit algoritmus v Studio učení Azure Machine. To znamená, že dává některé Generalizace a oversimplifications, že bude bod, který ve směru bezpečné. Znamená to také, že je mnoho algoritmů, které zde nejsou uvedeny. S narůstajícím Azure Machine Learning zahrnuje více úplnou sadu dostupných metod, přidáme jim.

Tato doporučení jsou zkompilované názory a tipy z velké množství dat vědců a odborníků výukové stroje. Nebyly jsme se dohodli na vše, ale zkoušel jsem harmonizovat naše stanoviska do surové konsensu. Většinu příkazů nesouhlasu začínají "Záleží..."

### <a name="how-to-use-the-cheat-sheet"></a>Jak používat podvést list

Načíst cestu a algoritmus popisky v grafu jako "pro * &lt;popis cesty&gt; * pomocí * &lt;algoritmus&gt;*." "Pro *rychlost* pomocí například *dvě třídy logistickou regresí*." V některých případech více než jednu větev bude platit.
Žádná z nich někdy budou vzájemně přizpůsobit. Jste má být pravidlo palce doporučení, takže se nemusíte zabývat bylo přesné.
Několik vědců data, které jsem mluvili s uvedeným které pouze se způsobem, jak najít ten nejlepší algoritmus je vyzkoušet je všechny.

Zde je příklad z [Galerie Cortana Intelligence](http://gallery.cortanaintelligence.com/) experimentu, který se pokusí proti stejná data několika algoritmů a porovnává výsledky: [porovnat více tříd třídění: písmeno uznání](http://gallery.cortanaintelligence.com/Details/a635502fc98b402a890efe21cec65b92).

>[AZURE.TIP] Stáhnout a vytisknout diagram, který poskytuje přehled možností Machine Learning Studio naleznete v tématu [diagram přehledu funkcí Azure Machine Learning Studio](machine-learning-studio-overview-diagram.md).

## <a name="flavors-of-machine-learning"></a>Typy flavor o strojovém učení

### <a name="supervised"></a>Pod dohledem

Algoritmy pro učení pod dohledem provést předpovědi, založené na řadě příkladů. Například historické ceny akcií lze riziko pokusů v budoucích cenách. Každý příklad, který používá pro vzdělávání je označena hodnota úroků – v tomto případě cenu akcií. Pod dohledem učení algoritmus vyhledává vzorky tyto popisky hodnot. Může použít jakékoli informace, které by mohly být důležité – den v týdnu, sezony, společnosti, finanční data, průmyslových, přítomnost geopolicitical rušivé události – a každý algoritmus hledá různých typů vzorků. Po algoritmus našel nejlepší vzorek může, vypracovat předpovědi pro neoznačené zkušební data pomocí tohoto vzorku – zítřejší ceny.

To je oblíbený druh užitečné strojovém učení. S jedinou výjimkou všechny moduly v učení Azure Machine jsou kontrolovány algoritmy učení. Existuje několik typů zvláštní dozor, učení, které jsou zastoupeny v rámci učení Azure Machine: detekce anomálií, klasifikace a regrese.

* **Klasifikace**. Při data jsou používány předpovědět kategorie, pod dohledem učení je rovněž používán termín klasifikace. To nastane při přiřazování obrázek jako obrázek "kočka" nebo "pes". Existují pouze dvě možnosti, se nazývá **dvě třídy** nebo **binomický klasifikace**. Pokud je další kategorie jako předpověď Vítěz NCAA března Madness turnaj, tento problém je označován jako **více tříd klasifikace**.

* **Regrese**. Pod dohledem učení je právě předpovědět hodnotu, stejně jako u cen akcií, se nazývá regrese.

* **Detekce anomálií**. Cílem je někdy datových bodů, které neobvyklé, jednoduše identifikovat. V odhalováním podvodů například všechny vzorky výdajů vysoce neobvyklé platební karty jsou podezřelé. Možné varianty jsou tak početné a vypadá tak málo, že není možné zjistit jaké podvodné činnosti vzdělávání příklady. Přístup, který bere detekce anomálií je jednoduše zjistit, jaké běžnou činnost vypadá (pomocí-podvodné transakce historie) a identifikovat cokoli, co se výrazně liší.

### <a name="unsupervised"></a>Bez dozoru

Datové body mají v závadou učení, bez popisků s nimi spojené. Místo toho je cílem závadou učení algoritmus data nějakým způsobem uspořádat a popsat jeho strukturu. To může znamenat seskupení do clusterů nebo hledání různých způsobů prohlížení komplexních dat tak, aby se snadněji a více uspořádaný.

### <a name="reinforcement-learning"></a>Posílení vzdělávání

V posílení učení získá algoritmus vybrat akci v reakci na každý datový bod. Výukový algoritmus také přijímá signály odměnu krátkou dobu později, označující jak dobré rozhodnutí.
Na základě toho se upravuje algoritmus své strategie pro dosažení nejvyšší odměnu. Nyní nejsou k dispozici žádné posílení učení algoritmus moduly Azure Machine dozvědět. Posílení učení je běžné v robotics sada senzor měření v jednom bodě v čase je datový bod, kde algoritmus musí vybrat další akci robot. Je také že vhodné přírodní pro Internet věcí aplikací.

## <a name="considerations-when-choosing-an-algorithm"></a>Důležité informace pro výběr algoritmu

### <a name="accuracy"></a>Přesnost

Získání co nejpřesnějších odpovědí možná není vždy nezbytné.
Někdy je přiměřené, podle toho, co chcete použít pro sblížení. Pokud tomu tak je, je možné vyjmout času zpracování výrazně tím provedením vykrvovacího vpichu s další přibližné metody. Další výhodou další přibližné metody je, že jsou přirozeně mají tendenci vyhnout se [overfitting](https://youtu.be/DQWI1kvmwRg).

### <a name="training-time"></a>Čas přípravy

Počet minut nebo hodin nutné školit modelu se liší značnou část algoritmy. Čas školení je často úzce spojena přesnost – jeden druhého obvykle doprovází. Kromě toho některé algoritmy jsou citlivější na počet bodů než ostatní.
Po omezenou dobu je jednotka vybrat algoritmus, zejména, pokud soubor dat je velký.

### <a name="linearity"></a>Linearita

Vytvořit velké množství machine learning algoritmy použít linearity. Lineární klasifikace algoritmy předpokládají, že mohou být třídy odděleny rovné čáry (nebo její vyšší rozměrovou analog). Patří logistickou regresí a podporují vektorové stroje (jak je implementováno v Azure Machine Learning).
Lineární regrese algoritmy předpokládají, že trendy dat postupujte podle rovné čáry. Tyto předpoklady nejsou některé problémy neměli, ale na ostatní se snížilo přesnost.

![Nelineární třídy bounday][1]

***Nelineární hranice třída*** *-spoléhání na algoritmus lineární zařazení by mělo za následek nízkou přesnost*

![Data s nelineární trend][2]

***Data s nelineární trend*** *-pomocí metody lineární regrese by generovat mnohem větší chyby, než je nezbytně nutné*

I přes jejich nebezpečí lineární algoritmy jsou velmi populární jako první linii útoku. Tyto jsou obvykle algorithmically jednoduché a rychlé na vlak.

### <a name="number-of-parameters"></a>Počet parametrů.

Parametry jsou knoflíky, získává vědci dat. Chcete-li při nastavování algoritmus. Jsou to čísla, která ovlivňují chování v algoritmu, například tolerance chyb nebo počet iterací nebo možnosti mezi variantami chování algoritmu. Čas na školení a přesnost algoritmus může být někdy poměrně citlivé na získání správné nastavení. Algoritmy s parametry velkého obvykle vyžadují většinu metodu pokusů a omylů najít správnou kombinaci.

Ve strojovém učení Azure, který automaticky všechny kombinace parametrů na jakékoli rozlišení zvolíte je také modul blok [komínů parametr](machine-learning-algorithm-parameters-optimize.md) . To sice skvěle, ujistěte se, že jste rozložených parametrů místa, čas potřebný k vlaku model zvyšuje exponenciálně s počtem parametrů.

Vzhůru, je s mnoha parametry obvykle označuje, že algoritmus má větší flexibilitu. Často může dosáhnout velmi dobrá přesnost. Pokud že můžete najít správnou kombinaci nastavení parametrů.

### <a name="number-of-features"></a>Počet funkcí

U některých typů dat, počet funkcí mohou být velmi velké ve srovnání s počet datových bodů. To je často případ s genetika nebo textová data. Mnoho funkcí lze bog dolů některé algoritmy učení, provádění školení unfeasibly dlouhá doba. Podpora vektorových stroje jsou zvláště vhodné pro tento případ (viz níže).

### <a name="special-cases"></a>Zvláštní případy

Některé algoritmy učení provést konkrétní předpoklady o struktuře dat nebo požadované výsledky. Pokud můžete najít takovou, která vyhovuje vašim potřebám, je vám může užitečnější výsledky přesnější předpovědi a zkrácení doby školení.

|**Algoritmus**|**Přesnost**|**Čas přípravy**|**Linearita**|**Parametry**|**Poznámky**|
|---|:---:|:---:|:---:|:---:|---|
|**Dvě třídy klasifikace**| | | | | |
|[logistickou regresí](https://msdn.microsoft.com/library/azure/dn905994.aspx)                    | |●|●|5| |
|[rozhodnutí doménové struktury](https://msdn.microsoft.com/library/azure/dn906008.aspx)|●|○| |6| |
|[rozhodnutí Džungle](https://msdn.microsoft.com/library/azure/dn905976.aspx)|●|○| |6|Nedostatek paměti půdorys|
|[zesílen rozhodovací strom](https://msdn.microsoft.com/library/azure/dn906025.aspx)|●|○| |6|Velké obsazeného|
|[neural sítě](https://msdn.microsoft.com/library/azure/dn905947.aspx)|●| | |9|[Další vlastní nastavení je možné](http://go.microsoft.com/fwlink/?LinkId=402867)|
|[průměrné perceptron](https://msdn.microsoft.com/library/azure/dn906036.aspx)|○|○|●|4| |
|[Podpora vektorových stroje](https://msdn.microsoft.com/library/azure/dn905835.aspx)| |○|●|5|Je vhodný pro velké funkce sady|
|[počítač místně obsáhlou podporu vektor](https://msdn.microsoft.com/library/azure/dn913070.aspx)|○| | |8|Je vhodný pro velké funkce sady|
|[Bayes' bod stroje](https://msdn.microsoft.com/library/azure/dn905930.aspx)| |○|●|3| |
|**Více tříd klasifikace**| | | | | |
|[logistickou regresí](https://msdn.microsoft.com/library/azure/dn905853.aspx)| |●|●|5| |
|[rozhodnutí doménové struktury](https://msdn.microsoft.com/library/azure/dn906015.aspx)|●|○| |6| |
|[rozhodnutí Džungle](https://msdn.microsoft.com/library/azure/dn905963.aspx)|●|○| |6|Nedostatek paměti půdorys|
|[neural sítě](https://msdn.microsoft.com/library/azure/dn906030.aspx)|●| | |9|[Další vlastní nastavení je možné](http://go.microsoft.com/fwlink/?LinkId=402867)|
|[1 v-all](https://msdn.microsoft.com/library/azure/dn905887.aspx)|-|-|-|-|Zobrazí vlastnosti vybrané metodě dvě třídy|
|**Regrese**| | | | | |
|[lineární](https://msdn.microsoft.com/library/azure/dn905978.aspx)| |●|●|4| |
|[Hodnota bayesovského lineární](https://msdn.microsoft.com/library/azure/dn906022.aspx)| |○|●|2| |
|[rozhodnutí doménové struktury](https://msdn.microsoft.com/library/azure/dn905862.aspx)|●|○| |6| |
|[zesílen rozhodovací strom](https://msdn.microsoft.com/library/azure/dn905801.aspx)|●|○| |5|Velké obsazeného|
|[quantile rychlé doménové struktury](https://msdn.microsoft.com/library/azure/dn913093.aspx)|●|○| |9|Distribuce, spíše než bod předpovědi|
|[neural sítě](https://msdn.microsoft.com/library/azure/dn905924.aspx)|●| | |9|[Další vlastní nastavení je možné](http://go.microsoft.com/fwlink/?LinkId=402867)|
|[Funkce POISSON](https://msdn.microsoft.com/library/azure/dn905988.aspx)| | |●|5|Technicky protokolu lineární. Pro odhad počtu|
|[pořadové číslo](https://msdn.microsoft.com/library/azure/dn906029.aspx)| | | |0|Pro odhad pořadí řazení|
|**Detekce anomálií**| | | | | |
|[Podpora vektorových stroje](https://msdn.microsoft.com/library/azure/dn913103.aspx)|○|○| |2|Obzvlášť vhodná pro velké funkce sady|
|[Detekce anomálií na základě DPS](https://msdn.microsoft.com/library/azure/dn913102.aspx)| |○|●|3| |
|[Prostředky K](https://msdn.microsoft.com/library/azure/5049a09b-bd90-4c4e-9b46-7c87e3a36810/)| |○|●|4|Clustering algoritmus|


**Vlastnosti algoritmu:**

**●** - ukazuje vynikající přesnost, čas rychlé vzdělávání a využívání linearity

**○** - ukazuje dobrou přesnost a časy střední vzdělávání

## <a name="algorithm-notes"></a>Algoritmus poznámky

### <a name="linear-regression"></a>Lineární regrese

Jak bylo uvedeno dříve, přizpůsobí [lineární regresní](https://msdn.microsoft.com/library/azure/dn905978.aspx) čáry (nebo roviny nebo hyperplane) sady. Je centrem, jednoduché a rychlé, ale může být příliš zneužívající vlastností prohlížeče pro některé problémy.
Zde vyhledejte [kurz lineární regrese](machine-learning-linear-regression-in-azure.md).

![Data pomocí lineárního trendu.][3]

***Data pomocí lineárního trendu.***

### <a name="logistic-regression"></a>Logistickou regresí

I když obsahuje confusingly "regrese" v názvu, logistickou regresí je skutečně výkonný nástroj pro [dvě třídy](https://msdn.microsoft.com/library/azure/dn905994.aspx) a [multiclass](https://msdn.microsoft.com/library/azure/dn905853.aspx) klasifikace. Je rychlý a jednoduchý. Skutečnost, že se používá s "-tvarované křivky místo rovné čáry je přírodní fit pro rozdělení dat do skupin. Hranice lineární třída poskytuje logistickou regresí, tak použijete, ujistěte se, že lineární aproximace je něco, co žijete s.

![Dvě třídy data pouze pro jednu funkci logistickou regresí][4]

***Logistickou regresí dvě třídy dat s jedinou funkcí*** *-hranice třída je bod, kde je křivka logistiky právě co nejblíže obě třídy*

### <a name="trees-forests-and-jungles"></a>Jungles, stromy a lesy

Rozhodnutí lesů ([regrese](https://msdn.microsoft.com/library/azure/dn905862.aspx), [dvě třídy](https://msdn.microsoft.com/library/azure/dn906008.aspx)a [multiclass](https://msdn.microsoft.com/library/azure/dn906015.aspx)), jungles rozhodnutí ([dvě třídy](https://msdn.microsoft.com/library/azure/dn905976.aspx) a [multiclass](https://msdn.microsoft.com/library/azure/dn905963.aspx)) a zesílen rozhodnutí stromy ([regresní](https://msdn.microsoft.com/library/azure/dn905801.aspx) a [dvě třídy](https://msdn.microsoft.com/library/azure/dn906025.aspx)) jsou založeny na rozhodovací stromy, základní strojní koncepce učení. Existuje mnoho variant stromy rozhodnutí, ale všechny jsou určeny totéž – funkce místo rozdělte regiony s většinou stejný popisek. Mohou to být oblasti jednotné kategorie nebo konstantní hodnoty v závislosti na tom, zda provádíte třídění nebo regrese.

![Rozhodovací strom rozděluje prostor funkce][5]

***Rozhodovací strom rozděluje funkce místo do oblasti zhruba jednotné hodnoty***

Protože funkce prostor lze rozdělit na libovolně malé oblasti, je snadné si představit rozdělení dostatečně jemně mít jeden datový bod v jednotlivých oblastech – extrémní příklad overfitting. Aby se zabránilo to, rozsáhlé sady stromů vznikly speciální matematické péči poskytnutou, že stromy nejsou vazba. Průměrná hodnota tohoto "rozhodnutí lesa" je strom, který zabraňuje overfitting. Rozhodnutí lesů lze použít velké množství paměti. Rozhodnutí jungles je varianta, která spotřebovává méně paměti na úkor poněkud delší dobu výuky.

Zesílen rozhodnutí stromy overfitting vyhnout omezením kolikrát lze také rozdělením a v každé oblasti jsou povoleny jak jen několik datových bodů. Algoritmus Konstruuje posloupnost stromy, z nichž každý se učí kompenzovat chyby po stromu před. Výsledkem je velmi přesná student, který se obvykle používá velké množství paměti. Pro úplný technický popis rezervujte [Friedman's původní papír](http://www-stat.stanford.edu/~jhf/ftp/trebst.pdf).

[Regresní quantile rychlé doménové struktury](https://msdn.microsoft.com/library/azure/dn913093.aspx) je varianta stromové struktury rozhodování pro zvláštní případ, kdy chcete znát nejen typické (střední) hodnotu dat v rámci regionu, ale také jeho distribuci ve formě quantiles.

### <a name="neural-networks-and-perceptrons"></a>Neural sítí a perceptrons

Neural sítě jsou mozek inspirovaných studium algoritmů zahrnující [multiclass](https://msdn.microsoft.com/library/azure/dn906030.aspx), [dvě třídy](https://msdn.microsoft.com/library/azure/dn905947.aspx)a [regresní](https://msdn.microsoft.com/library/azure/dn905924.aspx) problémy. V nekonečné řadě pocházejí, ale neural sítí v rámci Azure Machine Learning jsou všechny formy Acyklické řízené grafy. Který znamená, že vstupní funkce jsou předány vpřed (nikdy dozadu) řadou vrstev před zapnutí do výstupů. V každé vrstvě jsou vstupy váha v různých kombinacích, sčítají a předán do následující vrstvy. Tato kombinace jednoduchých výpočtů vede zdánlivě informace složitější třídy hranice a data trendů pomocí kouzelná. Více vrstev sítě toto řazení provést "hluboké učení," které paliva tolik tech hlášení a science fiction.

Tento vysoký výkon již není zdarma, ale přijde. Neural sítě může trvat dlouhou dobu na vlak, zvláště u velkých datových sad s mnoha funkcí. Mají také více parametrů než většina algoritmů, což znamená parametr (vymetání) komínů značnou část rozšiřuje dobu výuky.
A pro ty overachievers, kteří chtějí [zadat vlastní síťovou strukturu](http://go.microsoft.com/fwlink/?LinkId=402867), možnosti inexhaustible.

<a name="boundaries-learned-by-neural-networks6"></a>![Hranice se naučili neural sítě][6]
---------------------------

***Hranice se naučili pomocí neural sítí mohou být složité a nepravidelné***

[Dvě třídy jako průměr perceptron](https://msdn.microsoft.com/library/azure/dn906036.aspx) je odpověď skyrocketing časy školení neural sítě. Používá síťovou strukturu, který dává hranice tříd lineární. Je téměř primitivní podle dnešních norem, ale má dlouhou historii práce robustní a je dostatečně malá, aby se rychle naučit.

### <a name="svms"></a>SVMs

Podpora vektorových stroje (SVMs) najít hranici, která odděluje třídy podle jako široký okraj co nejvíce. Pokud dvě třídy nemohou být zřetelně odděleny, algoritmy nalézt nejlepší hranice, kterou mohou. Zapsaný v Azure Machine Learning, [SVM dvě třídy](https://msdn.microsoft.com/library/azure/dn905835.aspx) nemá s rovnou čáru. (V mluvení SVM používá lineární jádra.) Proto toto lineární sbližování, je možné velmi rychlé. Pokud ji skutečně září je funkce intenzivního data jako text nebo genom. V těchto případech jsou schopny samostatné třídy rychleji a s méně overfitting než většina jiné algoritmy, spolu s vyžadováním pouze mírné množství paměti SVMs.

![Hranici podpory vektorové počítače třída][7]

***Hranici typické podpory počítače vector třídy maximalizuje rozpětí mezi dvěma třídami***

Jiný produkt Microsoft Research, [dvě třídy místně hluboké SVM](https://msdn.microsoft.com/library/azure/dn913070.aspx) je nelineární varianta SVM, který zachová většinu účinnost verze lineární rychlost a paměť. Je ideální pro případy, kde není lineární přístup přidělit dostatečně přesné odpovědi. Vývojáři drží jej rychle problém rozdělit do menších problémů lineární SVM spoustu. Přečtěte si [úplný popis](http://research.microsoft.com/um/people/manik/pubs/Jose13.pdf) podrobné informace o jak jsou vyžádány vypnout tento trik.

Pomocí inteligentní rozšíření nelineární SVMs, [SVM jedna třída](https://msdn.microsoft.com/library/azure/dn913103.aspx) kreslí hranici důkladně popisuje celé datové sady. Je užitečné pro detekci anomálií. Všechny nové datové body, které daleko přesahují této hranice neobvyklé, být pozoruhodné.

### <a name="bayesian-methods"></a>Hodnota bayesovského metody

Hodnota bayesovského metody mají vysoce žádoucí kvality: vyhnou overfitting. Je to tím, že některé předpoklady o pravděpodobné šíření odpověď předem. Jiné byproduct tohoto přístupu je, že mají velmi málo parametrů. Azure Machine Learning je i hodnota bayesovského algoritmů pro klasifikaci ([dvě třídy Bayes zařízení](https://msdn.microsoft.com/library/azure/dn905930.aspx)) a regrese ([lineární regrese Hodnota bayesovského](https://msdn.microsoft.com/library/azure/dn906022.aspx)).
Všimněte si, že tyto předpokládá, že data můžete rozdělit nebo přizpůsobit rovnou čárou.

Na historické poznámky Bayes' bod stroje byly vyvinuty v Microsoft Research. Mají některé výjimečně krásné teoretické práce za ně. [Původní článek v JMLR](http://jmlr.org/papers/volume1/herbrich01a/herbrich01a.pdf) a [kvalifikovaná blogu podle Chris Bishop](http://blogs.technet.com/b/machinelearning/archive/2014/10/30/embracing-uncertainty-probabilistic-inference.aspx)přesměrován zúčastněných studentů.

### <a name="specialized-algorithms"></a>Speciální algoritmy

Pokud máte velmi konkrétní cíl může být štěstí. V rámci kolekce Azure Machine Learning jsou algoritmy, které se specializují na nej předpověď ([ordinální regrese](https://msdn.microsoft.com/library/azure/dn906029.aspx)), počet předpovědi ([Poissonova regrese](https://msdn.microsoft.com/library/azure/dn905988.aspx)) a detekce anomálií, (jeden na základě [analýzy hlavních komponent](https://msdn.microsoft.com/library/azure/dn913102.aspx) a jeden založený na [podporu vektorové stroje](https://msdn.microsoft.com/library/azure/dn913103.aspx)s).
A osamělé clustering algoritmus také ([prostředky K](https://msdn.microsoft.com/library/azure/5049a09b-bd90-4c4e-9b46-7c87e3a36810/)).

![Detekce anomálií na základě DPS][8]

***Detekce anomálií na základě DPS*** *-převážná většina dat, které patří do stereotypical distribuce; body přesahující výrazně, aby distribuce jsou podezřelé*

![Sada dat, které jsou seskupeny pomocí prostředků K][9]

***Sada dat je rozdělena do 5 clusterů prostředky K***

Je také komplet [one vše v multiclass třídění](https://msdn.microsoft.com/library/azure/dn905887.aspx), kterým problém klasifikační třídy N rozdělí na dvě třídy klasifikace problémů N-1. Přesnost, čas přípravy a linearita vlastností jsou určeny dvě třídy třídění používá.

![Dvě třídy třídění spolu tvoří tři třídy třídění][10]

***Zkombinovat pár dvě třídy třídění tvoří tři třídy třídění***

Výuka Azure Machine také přístup k učení rámec výkonné počítače pod názvem [Vowpal Wabbit](https://msdn.microsoft.com/library/azure/8383eb49-c0a3-45db-95c8-eb56a1fef5bf).
VW prosíme, abyste kategorizace sem, protože můžete zjistit problémy klasifikace a regrese a můžete dokonce získat informace z částečně bez popisku dat. Můžete nakonfigurovat jej, aby používal některý počet výukových algoritmů, ztráta funkce a algoritmy optimalizace. Byla navržena od základů až být efektivní, paralelní a velmi rychlý. Zpracovává funkce ridiculously velké sady s málo patrné úsilí.
Spuštění a vedená společností Microsoft Research na vlastní Jan Langford, VW je jeden vzorec položky v poli algoritmy populace auta. Ne každý problém odpovídá VW, ale bezdrátové sítě je možné vyplatit na stoupání Křivka osvojování znalostí v rozhraní. Je také k dispozici jako [samostatné otevřený zdrojový kód](https://github.com/JohnLangford/vowpal_wabbit) v několika jazycích.


<!-- Media -->

[1]: ./media/machine-learning-algorithm-choice/image1.png
[2]: ./media/machine-learning-algorithm-choice/image2.png
[3]: ./media/machine-learning-algorithm-choice/image3.png
[4]: ./media/machine-learning-algorithm-choice/image4.png
[5]: ./media/machine-learning-algorithm-choice/image5.png
[6]: ./media/machine-learning-algorithm-choice/image6.png
[7]: ./media/machine-learning-algorithm-choice/image7.png
[8]: ./media/machine-learning-algorithm-choice/image8.png
[9]: ./media/machine-learning-algorithm-choice/image9.png
[10]: ./media/machine-learning-algorithm-choice/image10.png
