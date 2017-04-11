<properties 
    pageTitle="Vyhodnocení modelu výkonu ve počítače výukové | Microsoft Azure" 
    description="Vysvětluje, jak chcete zjistit modelu výkonu Azure počítač přečíst." 
    services="machine-learning"
    documentationCenter="" 
    authors="garyericson" 
    manager="jhubbard" 
    editor="cgronlun"/>

<tags 
    ms.service="machine-learning" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/19/2016" 
    ms.author="bradsev;garye" />


# <a name="how-to-evaluate-model-performance-in-azure-machine-learning"></a>Jak zjistit modelu výkonu Azure počítač přečíst

Toto téma ukazuje, jak chcete zjistit výkonu modelu v Azure počítače výukové Studio a poskytuje stručný metriky k dispozici pro daný úkol. Tři obvyklé scénáře sledovaných výukové jsou uvedeny: 

* Analytický nástroj Regrese
* binární klasifikace 
* multiclass klasifikace

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]


Vyhodnocení výkonu modelu je taková základní fáze procesu vědy data. Znamená to úspěšnost bodování (předpovědí) sady dat byl školení model. 

Azure výukové počítač podporuje model hodnocení až dva jeho hlavního počítače výukové moduly: [Vyhodnocení modelu] [ evaluate-model] a [Ověřte křížově Model][cross-validate-model]. Moduly umožňují najdete v článku jak modelu provádí z hlediska celá řada metriky, které se běžně používají ve počítače učení a statistiky.

##<a name="evaluation-vs-cross-validation"></a>Hodnocení porovnání křížově ověření##
Vyhodnocení a křížové ověření způsoby standardní měření výkonu modelu. Obě vytvářejí metriky hodnocení, můžete zkontrolovat nebo porovnání ty ostatní modely.

[Vyhodnocení modelu] [ evaluate-model] očekává scored datovou sadu jako vstup (nebo 2 v případě chcete porovnat 2 různých modelů). To znamená, že budete muset školení modelu pomocí [Školení modelu] [ train-model] modul a provedete předpovědí na některé datovou sadu pomocí [Modelu skóre] [ score-model] modul, před vyhodnocení výsledků. Vyhodnocení je na základě scored popisky/pravděpodobnosti spolu s true popisky, které jsou všechny výstup [Skóre modelu] [ score-model] modul.

Můžete taky můžete křížového ověření konfigurovat celou řadu možností vlaku výsledek vyhodnocení operace (10 přeložení) automaticky na různých podmnožiny vstupní data. Zadávání dat je rozdělen do 10 části, kde jedna je vyhrazena pro účely testování, a další 9 pro vzdělávání. Tento postup se opakuje 10krát a průměr metriky hodnocení se. Tato funkce umožňuje určit, jak by modelu generalize do nové datové sady. [Ověření křížově modelu] [ cross-validate-model] modul zabírá v modelu nevyškoleným a některé datové sady s popisky a zobrazí výsledky vyhodnocení všech 10 přeložení kromě průměrné výsledky.

V následujících částech můžeme vytvořit jednoduchý regresní a klasifikace modely a vyhodnocení jejich výkonu pomocí obou [Vyhodnocení modelu] [ evaluate-model] a [Ověřte křížově modelu] [ cross-validate-model] moduly.

##<a name="evaluating-a-regression-model"></a>Vyhodnocení regresní Model##
Předpokládejme, že chceme předpovídání auta price pomocí některé funkce, například rozměry, koňské síly, specifikace engine a tak dál. Toto je typické regresní problém, kde proměnnou cílové (*cena*) nepřetržitý číselnou hodnotu. Společnost Microsoft může obsahovat prostá lineární regrese modelu, vzhledem k hodnotám funkce určitých auta, můžete předpovídání cenu této auta. Tento model regresní mohou sloužit k skóre stejné datovou sadu, kterou jsme školení na. Jakmile máme předpovídané ceny jednotlivých automobilů, můžete vyhodnocení výkonu modelu pohledem kolik předpovědí liší od skutečné ceny průměrně. Pro znázornění je používáme *datovou sadu dat (suroviny) cena Auto* k dispozici v části **Uložit datové sady** v Azure počítače výukové Studio.
 
###<a name="creating-the-experiment"></a>Vytváření testu###
Přidání modulů do pracovního prostoru v Azure počítače výukové Studio:

- Údaje o cenách automobilů (suroviny)
- [Lineární regrese][linear-regression]
- [Vlaku modelu][train-model]
- [Model skóre][score-model]
- [Vyhodnocení modelu][evaluate-model]


Připojení porty viz dole na obrázku 1 a nastavení popisek sloupce [Vlaku modelu] [ train-model] modul *cena*.
 
![Vyhodnocení regresní Model](media/machine-learning-evaluate-model-performance/1.png)

Obrázek 1. Vyhodnocení regresní Model.

###<a name="inspecting-the-evaluation-results"></a>Kontrola výsledků hodnocení###
Po dokončení testu, kliknete na výstupní port [Vyhodnocení modelu] [ evaluate-model] modul a vyberte *vizualizace* a zobrazte výsledky vyhodnocení. Jsou k dispozici pro regresní modely hodnocení metriky: *Nechtěli absolutní chyby* *Kořenové nechtěli absolutní chyby*, *Relativní absolutní chyba*, *Relativní spolehlivosti chyby*a *Koeficient stanovení*.

Termín "Chyba" tady znázorňuje rozdíl mezi předpovězené hodnoty a skutečnou hodnotou. Absolutní hodnota nebo druhou mocninu tento rozdíl jsou obvykle vypočítanou k zaznamenání celkové velikosti chyby ve všech instancích jako rozdíl mezi hodnotu předpovídané a true může být v některých případech záporné. Metriky chyby měření prediktivní výkonu regresní model z hlediska střední odchylka jejich předpovědí z skutečné hodnoty. Nižší chybové hodnoty znamenají, že do modelu je přesnější při předpovědí. Celková míru chyb 0 znamená modelu přesně odpovídá datům.

Koeficient rozhodnutí, která se nazývá také R spolehlivosti, je také standardní způsob, jak měřit chod modelu vešel data. Můžete interpretován jako část variant vysvětlení modelem. Vyšší poměr stran je v takovém případě lepší kde 1 označuje vzájemně přizpůsobit.
 
![Metriky hodnocení lineární regrese](media/machine-learning-evaluate-model-performance/2.png)

Obrázek 2. Metriky hodnocení lineární regrese.

###<a name="using-cross-validation"></a>Použití křížového ověření###
Jak jsme zmínili dříve, můžete provádět opakované školení, bodování a hodnocení automaticky na základě [Křížové ověřit modelu] [ cross-validate-model] modul. V tomto případě potřebujete stačí datovou sadu, nevyškoleným model a [Ověřte křížově modelu] [ cross-validate-model] modul (viz následující obrázek). Poznámka: je potřeba nastavit popisek sloupce *cena* v [Křížově ověřování modelu] [ cross-validate-model] modulu vlastnosti.

![Křížové ověřování modelu regresní](media/machine-learning-evaluate-model-performance/3.png)

Obrázek 3. Ověřování modelu regresní více.

Po dokončení testu, můžete zkontrolovat výsledky vyhodnocení kliknutím na pravý výstupní port [Ověřit křížově modelu] [ cross-validate-model] modul. To poskytne podrobný přehled metriky jednotlivých iteracích (brožura) a průměrné výsledky každého metriky (obrázek 4).
 
![Ověření více výsledky regresní Model](media/machine-learning-evaluate-model-performance/4.png)

Obrázek 4. Ověření více výsledky regresní Model.

##<a name="evaluating-a-binary-classification-model"></a>Vyhodnocení modelu binární klasifikace##
Ve scénáři binární klasifikace proměnnou cílové obsahuje pouze dvou možných výstupů, například: {0; 1} nebo {NEPRAVDA, PRAVDA}, {záporné, kladné}. Předpokládejme, budete mít datovou sadu dospělého zaměstnanců s některými demografické a zaměstnání proměnných, a můžete být požádáni odhadnout úrovně příjmu binární proměnná s hodnotami {"< = 50K", "> 50K"}. Jinými slovy záporné třídy představuje zaměstnanců, kteří menší nebo rovna hodnotě 50K za rok a kladné třídy představuje všechny ostatní zaměstnance. Jako scénář regresní jsme by školení modelu skóre některá data a vyhodnocení výsledků. Hlavní rozdíl je výběr metriky, které vypočítá Azure počítače výukové a výstupů. Ke znázornění scénář úrovně předpovědí příjem používáme [dospělého](http://archive.ics.uci.edu/ml/datasets/Adult) datovou sadu a vytvořte pokus Azure počítače výukové vyhodnocení výkonu modelu logistickou regresní dvě třídy běžně používaných binární třídění.

###<a name="creating-the-experiment"></a>Vytváření testu###
Přidání modulů do pracovního prostoru v Azure počítače výukové Studio:

- Dospělého datovou sadu sčítání příjem binární klasifikace
- [Regresní logistickou dvě třídy][two-class-logistic-regression]
- [Vlaku modelu][train-model]
- [Model skóre][score-model]
- [Vyhodnocení modelu][evaluate-model]

Připojení porty viz dole na obrázku 5 a nastavení popisek sloupce [Vlaku modelu] [ train-model] modul *příjem*.

![Vyhodnocení modelu binární klasifikace](media/machine-learning-evaluate-model-performance/5.png)

Obrázek 5. Vyhodnocení modelu binární klasifikace.

###<a name="inspecting-the-evaluation-results"></a>Kontrola výsledků hodnocení###
Po dokončení testu, kliknete na výstupní port [Vyhodnocení modelu] [ evaluate-model] modul a vyberte *vizualizace* a zobrazte výsledky vyhodnocení (obrázek 7). Jsou k dispozici pro modely binární klasifikace hodnocení metriky: *Přesnost* *Přesnost*, *odvolat*, *F1 skóre*a *AUC*. Kromě toho modulu výstupy zmatky matice počet true pozitivní, NEPRAVDA záporů, falešně pozitivní a true záporů, jakož i *ROC* *Přesnost/odvolání*a *zvedněte* křivky znázorňující.

Přesnost je jednoduše poměr stran správně klasifikované instancí. Je obvykle první míru, které se podíváte na při vyhodnocování třídění. Když testovací data je však nevyvážené (kde většina instancí se vztahují k jednomu tříd) nebo vás zajímají další výkonu na kterýkoli z nich tříd přesnost nemá skutečně zachytit účinnosti třídění. V případě úroveň klasifikace příjem předpokládá, že testujete na některá data, kde 99 % instancí představují lidé, kteří získat menší nebo rovna hodnotě 50 K za rok. Je možné k dosažení 0.99 přesnost podle předpovídání předmětu "< = 50K" pro všechny instance. Třídění v tomto případě vypadá to, která by výborně celkové, ale ve skutečnosti nezdaří ke klasifikaci některou vysokými příjmy lidí (1 %) správně.

Z tohoto důvodu je užitečné pro výpočet další metriky, které zachytit další zvláštní aspekty hodnocení. Před přechodem na podrobné informace o těchto metriky, je důležité pochopit matice zmatky vyhodnocení binární klasifikace. Popisky třídy v sadě školení lze provádět se jenom 2 možnými hodnotami, které obvykle označovány jako kladné nebo záporné. Kladné a záporné instance, které třídění předpovídá správně je nazýván true pozitivní (podokna nástrojů) a true záporů (TN). Podobně nesprávně klasifikované instance nazývají falešně pozitivní (dokončených produktů) a NEPRAVDA záporů (FN). Záměna matice je jednoduše tabulka s počet instance, které spadají každé z těchto 4 kategorií. Azure výukové počítači automaticky rozhodne dvou tříd v datové sadě tedy kladné předmětu. Pokud popisky třídy jsou logické hodnoty nebo celá čísla, mají přiřazenou instance s popiskem "true" nebo "1" kladné předmětu. Pokud popisky jsou řetězce, stejně jako v případě příjem datovou sadu štítků jsou seřazená abecedně a první úrovně je vybrán je záporné předmětu, zatímco druhé úrovně je kladné předmětu.

![Binární klasifikace Zmatky matice](media/machine-learning-evaluate-model-performance/6a.png)

Obrázek 6. Binární klasifikace Zmatky matice.

Přejdete zpět na problém klasifikace příjem, by chceme požádejte několik otázek hodnocení, které softwaru a služeb srozumitelný výkonu třídění použít. Je velmi natural otázku: "od osob, kterým modelu předpokladů být získávat > 50 K (podokna nástrojů + předponou formátu) kolik jsou klasifikované správně (podokna nástrojů)?" Tato otázka můžete odpovědět prohlížíte **Přesnost** modelu, který je část pozitivní, které jsou klasifikované správně: TP/(TP+FP). Je další běžné otázky "mimo všechny maximum získávat zaměstnanců s příjem > 50 k (podokna nástrojů + FN), kolik třídění klasifikovat správně (podokna nástrojů)". To je skutečně **odvolat**, nebo true kladné sazba: TP/(TP+FN) třídění. Může se stát, že je zjevných poměr přesnost a odvolání. Například možnost relativně rovnováha datovou sadu, třídění, které odhadne převážně kladné instance, byste měli vysoké odvolání, ale raději zhoršeným přesnost, kolik instancí záporné by misclassified výsledkem velkého počtu falešně pozitivních výsledků. Zobrazované o tom, jak tyto dva metriky být trochu jiný, zobrazíte klepnutím na "Přesnost/odvolání" křivka na stránce výstup výsledek vyhodnocení (nahoře vlevo část Obrázek 7).

![Binární klasifikace hodnocení výsledků](media/machine-learning-evaluate-model-performance/7.png) obrázek 7. Binární klasifikace hodnocení výsledků.

Další související míru často používaný je **F1 skóre**, které má přesnost a odvolání v úvahu. Je harmonický průměr těchto 2 metriky a počítá jako takové: F1 = 2 (odvolání přesnost x) / (přesnost + odvolání). Skóre F1 je vhodný k sumarizaci hodnocení v jedné řadě, ale je vždy vhodné se podívat na přesnost a odvolání společně a snadněji pochopit, jak se chová třídění.

Kromě toho jednu zkontrolovat true kladné sazba a NEPRAVDA kladné kurz křivka **Sluchátko provozní vlastnostmi (ROC)** a odpovídající hodnotou **Oblasti v části křivky (AUC)** . Bližší této křivky je do levého horního rohu, tím lepší výkon třídění (který je maximalizace rychlosti true kladné při minimalizace rychlosti falešně pozitivní). Křivky, které se blíží šikmo grafu výsledku třídění, které mají za následek zkontrolujte předpovědí, které se blíží náhodné uhodnout.

###<a name="using-cross-validation"></a>Použití křížového ověření###
Jako příklad regresní provedeme křížového ověření a opakovaně školení, výsledek vyhodnocení různých podmnožin dat automaticky. Podobně, můžete použít [Více ověřování modelu] [ cross-validate-model] modul nevyškoleným logistickou regresní model a datovou sadu. Popisek sloupce musí být nastaven na *příjem* ve [Více ověřování modelu] [ cross-validate-model] modulu vlastnosti. Po spuštění testu a kliknete na pravém výstupní port [Ověřit křížově modelu] [ cross-validate-model] modul, vidíme binární klasifikace metrických hodnoty pro každý brožura navíc střední hodnota a směrodatná odchylka každého. 
 
![Křížové ověřování modelu binární klasifikace](media/machine-learning-evaluate-model-performance/8.png)

Obrázek 8. Křížové ověřování modelu binární klasifikace.

![Ověření více výsledků binární třídění](media/machine-learning-evaluate-model-performance/9.png)

Obrázek 9. Ověření více výsledků binární třídění.

##<a name="evaluating-a-multiclass-classification-model"></a>Vyhodnocení modelu Multiclass klasifikace##
V tomto pokus použijeme Oblíbené datové sadě(http://archive.ics.uci.edu/ml/datasets/Iris "Iris") [Iris]obsahující výskyty různé typy 3 (třídy) rostliny iris. Existuje 4 hodnoty funkce (sepal délka/šířka a délka Okvětní lístek/šířka) pro jednotlivé instance. V předchozí pokusy školení jsme testováno modelů pomocí stejného datové sady. Tady, použijeme [Rozdělenými daty] [ split] modul k vytváření 2 podmnožin dat, školení na první z nich a počet bodů a jsou vyhodnoceny jako druhou. Datová sada Iris veřejně neexistuje [Uc počítače výukové úložiště](http://archive.ics.uci.edu/ml/index.html)a se dají stáhnout pomocí [Importovat Data] [ import-data] modul.

###<a name="creating-the-experiment"></a>Vytváření testu###
Přidání modulů do pracovního prostoru v Azure počítače výukové Studio:

- [Import dat][import-data]
- [Rozhodnutí o struktuře multiclass][multiclass-decision-forest]
- [Oddělení buněk][split]
- [Vlaku modelu][train-model]
- [Model skóre][score-model]
- [Vyhodnocení modelu][evaluate-model]

Porty připojení, jak je znázorněno níže obrázek 10.

Nastavit sloupec index popisek [Vlaku modelu] [ train-model] modul až 5. Datové má bez záhlaví ale nám vědět, že třídy popisky v pátém sloupci.

Klikněte na [Importovat Data] [ import-data] modul a nastavte vlastnost *zdroj dat* *Webová adresa URL prostřednictvím protokolu HTTP*a adresu *URL* http://archive.ics.uci.edu/ml/machine-learning-databases/iris/iris.data.

Nastavení zlomek instance, které chcete použít pro vzdělávání v [Rozdělenými daty] [ split] modul (0,7 například).
 
![Vyhodnocení Multiclass třídění](media/machine-learning-evaluate-model-performance/10.png)

Obrázek 10. Vyhodnocení Multiclass třídění

###<a name="inspecting-the-evaluation-results"></a>Kontrola výsledků hodnocení###
Spuštění testu a klikněte na příkaz výstupní port [Vyhodnocení modelu][evaluate-model]. Výsledky vyhodnocení nejsou zobrazeny ve formě obrázku zapomíná a to v tomto případě. Matice zobrazuje skutečnost a předpovídané instance pro všechny 3 třídy.
 
![Výsledky vyhodnocení multiclass klasifikace](media/machine-learning-evaluate-model-performance/11.png)

Obrázek 11. Výsledky vyhodnocení multiclass klasifikace.

###<a name="using-cross-validation"></a>Použití křížového ověření###
Jak jsme zmínili dříve, můžete provádět opakované školení, bodování a hodnocení automaticky na základě [Křížové ověřit modelu] [ cross-validate-model] modul. Měli byste datovou sadu, nevyškoleným model a [Ověřte křížově modelu] [ cross-validate-model] modul (viz následující obrázek). Znovu je potřeba nastavit popisek sloupce [Ověřit křížově modelu] [ cross-validate-model] modul (index sloupce 5 v tomto případě). Po spuštění testu a kliknete na pravém výstup port [Ověřit křížově modelu][cross-validate-model], můžete zkontrolovat metrických hodnoty pro každý brožura, jakož i střed_hodn a standardní odchylka. Metriky zobrazena zde se podobají z nich popisované v případě binární klasifikace. Však, že v multiclass klasifikace výpočetních true pozitivní/záporů a falešně pozitivní/záporů vystavila společnost počítání na základě podle předmětu je třída celkové kladnou nebo zápornou. Třeba při výpočtu přesnost nebo odvolání třídy "Iris setosa", předpokládá se, že se jedná kladné tříd a všechny ostatní jako záporné.
 
![Křížové ověřování modelu Multiclass klasifikace](media/machine-learning-evaluate-model-performance/12.png)

Obrázek 12. Křížové ověřování modelu Multiclass klasifikace.


![Výsledky křížově ověřování modelu Multiclass klasifikace](media/machine-learning-evaluate-model-performance/13.png)

Obrázek 13. Výsledky křížového ověřování modelu Multiclass klasifikace.


<!-- Module References -->
[cross-validate-model]: https://msdn.microsoft.com/library/azure/75fb875d-6b86-4d46-8bcc-74261ade5826/
[evaluate-model]: https://msdn.microsoft.com/library/azure/927d65ac-3b50-4694-9903-20f6c1672089/
[linear-regression]: https://msdn.microsoft.com/library/azure/31960a6f-789b-4cf7-88d6-2e1152c0bd1a/
[multiclass-decision-forest]: https://msdn.microsoft.com/library/azure/5e70108d-2e44-45d9-86e8-94f37c68fe86/
[import-data]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/
[score-model]: https://msdn.microsoft.com/library/azure/401b4f92-e724-4d5a-be81-d5b0ff9bdb33/
[split]: https://msdn.microsoft.com/library/azure/70530644-c97a-4ab6-85f7-88bf30a8be5f/
[train-model]: https://msdn.microsoft.com/library/azure/5cc7053e-aa30-450d-96c0-dae4be720977/
[two-class-logistic-regression]: https://msdn.microsoft.com/library/azure/b0fd7660-eeed-43c5-9487-20d9cc79ed5d/
 
