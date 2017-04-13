<properties
    pageTitle="Interpretace modelu výsledky ve počítače výukové | Microsoft Azure"
    description="Postup výběru optimální parametr nastaven algoritmu pomocí a vizualizace skóre modelu výstupů."
    services="machine-learning"
    documentationCenter=""
    authors="bradsev"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/12/2016"
    ms.author="bradsev" />


# <a name="interpret-model-results-in-azure-machine-learning"></a>Interpretace modelu výsledky Azure počítač přečíst

Toto téma vysvětluje, jak vizualizace a interpretaci předpovědí výsledky v Azure počítače výukové Studio. Po školení modelu a mít předpovědí sobě ("dosáhne modelu") je třeba porozumět a interpretaci výsledků předpovědí.

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

Existují čtyři hlavní typy počítače výukové modely dozvědět Azure počítače:

* Klasifikace
* Clusterů
* Analytický nástroj Regrese
* Doporučení systémy

Moduly pro předpověď na tyto modely jsou:

* [Skóre modelu] [ score-model] modulu pro klasifikaci a regresní
* [Přiřadit clusterů] [ assign-to-clusters] modul pro clusterů
* [Skóre Matchbox doporučení] [ score-matchbox-recommender] pro systémy doporučení

Tento dokument vysvětluje, jak interpretovat předpovědí výsledků pro každou z těchto moduly. Základní informace o těchto modulů kontroly najdete v článku [jak zvolte parametry optimalizovat algoritmy Azure počítač přečíst](machine-learning-algorithm-parameters-optimize.md).

Toto téma adresy předpovědí interpretaci, ale ne model hodnocení. Další informace o tom, jak zjistit modelu, najdete v tématu [vyhodnocení modelu výkonu Azure počítač přečíst](machine-learning-evaluate-model-performance.md).

Pokud začínáte s Azure počítače výukové a potřebujete pomoc s vytvořením simple pokusy začít, najdete v článku [Vytvoření jednoduchého experiment v Azure počítače výukové Studio](machine-learning-create-experiment.md) v Azure počítače výukové Studio.

## <a name="classification"></a>Klasifikace ##
Existují dva podkategoriemi klasifikace problémů:

* Problémy s jenom dvě třídy (dvě předmětu nebo binární klasifikace)
* Problémy s více než dvě třídy (více třídy klasifikace)

Azure výukové počítače má různé moduly jednotlivé typy klasifikace řešení, ale metody interpretaci jejich předpovědí výsledků se podobají.

### <a name="two-class-classification"></a>Dvě třídy klasifikace###
**Příklad testu**

Příklad klasifikace dvě třídy problém je klasifikace iris květin. Úkol je ke klasifikaci iris květiny na základě jejich funkcí. Uvedenou množinu dat Iris k dispozici v Azure počítače výukové je podmnožinu Oblíbené [Iris uvedenou množinu dat](http://en.wikipedia.org/wiki/Iris_flower_data_set) obsahující výskyty jen dvěma druhy květinou (třídy 0 a 1). Existují čtyři funkce pro každý květina (sepal délka sepal šířka, délka Okvětní lístek a Okvětní lístek šířku).

![Snímek obrazovky s iris testu](./media/machine-learning-interpret-model-results/1.png)

Obrázek 1. Experiment problém Iris dvě třídy klasifikace

Pokus optického řešení potíží, jak je vidět na obrázku 1. Model stromu dvě třídy zesílen rozhodnutí byl školení a dosáhne. Teď můžete vizualizovat výsledky prediktivní vkládání z [Modelu skóre] [ score-model] modul kliknutím na výstupní port [Skóre modelu] [ score-model] modul a pak kliknete na **vizualizace**.

![Modul modelu skóre](./media/machine-learning-interpret-model-results/1_1.png)

Tím se vyvolá hodnocení výsledků jak je vidět na obrázku 2.

![Výsledky experiment iris dvě třídy klasifikace](./media/machine-learning-interpret-model-results/2.png)

Obrázek 2. Vizualizace výsledku skóre modelu ve dvou třídy klasifikace

**Výsledek vyhodnocení**

Existuje šest sloupců v tabulce výsledky. Levé čtyř sloupců jsou čtyři funkce. Vpravo sloupcích dosáhne popisky a dosáhne pravděpodobnosti jsou výsledky předpovědí. Sloupec dosáhne pravděpodobnosti zobrazuje pravděpodobnost patřící květiny kladné třídy (třídy 1). První číslo v sloupce (0.028571) znamená, že je například 0.028571 pravděpodobnost, že první květinou patří třídy 1. Dosáhne popisky sloupec zobrazuje předpovídané předmětu pro každou květin. To je založena na sloupec dosáhne pravděpodobnosti. Pokud scored pravděpodobnost květiny je větší než 0,5, je předpovídat jako třídy 1. V ostatních případech je předpovídat jako třídy 0.

**Publikování webové služby**

Výsledky prediktivní vkládání již byly srozumitelné a považují za zvuku, testu publikováním jako webové služby, aby mohli nasadit v různých aplikacích a zavolejte ho získat předpovědí třídy na všechny nové iris květin. Zjistěte, jak změnit experiment školení do bodování testu a potom ji publikujte jako webové služby, najdete v článku [publikovat webové služby Azure počítače výukové](machine-learning-walkthrough-5-publish-web-service.md). Tento postup vám poskytne bodování experiment jak je vidět na obrázku 3.

![Snímek obrazovky s bodovým testu](./media/machine-learning-interpret-model-results/3.png)

Obrázek 3. Hodnocení experiment iris dvě třídy klasifikace problém

Teď budete muset nastavit vstupní a výstupní webové služby. Zadání je správné vstupní port [Skóre]modelu[score-model], tedy květinou Iris funkce input. Volba výstup závisí na tom, jestli vás zajímají předpovídané třídy (dosáhne popisek) a scored pravděpodobnost. V tomto příkladu předpokládá se, že jste zúčastněnými v obou. Vyberte požadované výstupní sloupce, můžete [Vybrat sloupce v sadě dat] [ select-columns] modul. Klikněte na [Vybrat sloupce v sadě dat][select-columns], klikněte na **Spustit volič sloupce**a vyberte **Dosáhne popisky** a **Dosáhne pravděpodobnosti**. Po nastavení výstupní port [Vybrat sloupce v sadě dat] [ select-columns] a znovu spustit, měli byste být připraveni publikovat bodování testu jako webové služby kliknutím na **Publikovat webové služby**. Konečný testu vypadá jako obrázek 4.

![Experiment iris dvě třídy klasifikace](./media/machine-learning-interpret-model-results/4.png)

Obrázek 4. Konečný bodování experiment problému s iris dvě třídy klasifikace

Po spuštění webové služby a zadejte některé funkce hodnoty testovací instance vrátí výsledek dvou čísel. První číslo je scored popisek a druhý je scored pravděpodobnost. Tento květinou je předpovídat jako třídy 1 s 0.9655 pravděpodobnost.

![Testování interpretace skóre modelu](./media/machine-learning-interpret-model-results/4_1.png)

![Hodnocení výsledky testů](./media/machine-learning-interpret-model-results/5.png)

Obrázek 5. Web služby výsledek iris dvě třídy klasifikace

### <a name="multi-class-classification"></a>Více třídy klasifikace
**Příklad testu**

V této experiment provedete úkol písmeno rozpoznávání jako příklad klasifikace multiclass. Třídění pokusí se předpovídání určitých písmeno (třídy) založené na některé ručně psaných atributů extrahovaných z obrázků ručně psaných.

![Příklad rozpoznávání písmeno](./media/machine-learning-interpret-model-results/5_1.png)

V okně školení data jsou 16 funkce extrahovaných z webu obrázky ručně psaných písmeno. 26 písmen formuláře naše 26 třídy. Obrázek 6 zobrazuje pokus, který bude školení multiclass klasifikace modelu doplňku pro rozpoznávání písmeno a předpovídání na stejnou funkci nastavení se sadou dat testu.

![Písmeno rozpoznávání multiclass klasifikace testu](./media/machine-learning-interpret-model-results/6.png)

Obrázek 6. Písmeno rozpoznávání multiclass klasifikace problém testu

Vizualizace výsledky [Skóre modelu] [ score-model] modul kliknutím na výstupní port [Skóre] modelu[ score-model] modul a pak kliknete na **vizualizace**, byste měli vidět obsah, jak ukazuje obrázek 7.

![Výsledky modelu skóre](./media/machine-learning-interpret-model-results/7.png)

Obrázek 7. Vizualizace skóre modelu má za následek více třídy klasifikace

**Výsledek vyhodnocení**

Levé 16 sloupců představují hodnoty funkce množiny testu. Sloupce s názvy jako dosáhne pravděpodobnosti třídy "XX", jsou jenom jako sloupci dosáhne pravděpodobnosti v případě dvou předmětu. Zobrazit pravděpodobnost odpovídající položku určitém do určité třídy. Například pro první položku je 0.003571 pravděpodobnost, že je to "A" 0.000451 pravděpodobnost, že je "B" a tak dále. Poslední sloupec (dosáhne popisky) je stejná jako popisky dosáhne v případě dvou předmětu. Vybere třídy s největší pravděpodobností scored jako předpovídané třídy odpovídající položku. Například pro první položku scored popisek je "F" protože má největší pravděpodobností být "F" (0.916995).

**Publikování webové služby**

Můžete taky dostanete scored popisek pro každou položku a pravděpodobnosti scored popisek. Základní logika je najít největší pravděpodobností mezi scored pravděpodobnosti. K tomuto účelu je potřeba [Provést skript R] [ execute-r-script] modul. Kód R je vidět na obrázku 8 a výsledků testu se zobrazují v obrázek 9.

![Příklad R](./media/machine-learning-interpret-model-results/8.png)

Obrázek 8. R kódu pro extrahování dosáhne popisky a spojena pravděpodobnost popisků

![Experiment výsledek](./media/machine-learning-interpret-model-results/9.png)

Obrázek 9. Konečný bodování experiment problému písmeno rozpoznávání multiclass klasifikace

Po publikování a spuštění webové služby a zadejte některé funkce vstupní hodnoty, vrácený výsledek vypadá jako obrázek 10. Tento vlastnoruční dopis s extrahované 16 funkcemi předpovídat jako "D" s 0.9715 pravděpodobnost.

![Interpretace modul skóre test](./media/machine-learning-interpret-model-results/9_1.png)

![Testování výsledek](./media/machine-learning-interpret-model-results/10.png)

Obrázek 10. Web služby výsledek multiclass klasifikace

## <a name="regression"></a>Analytický nástroj Regrese

Regresní problémy se liší od klasifikace problémy. V klasifikace problém se snažíte předpovídání samostatné třídy, například které patří třídy iris květin. Ale můžete vidět v následujícím příkladu regresní problém, se snažíte předpovídání nepřetržitý proměnné, například cenu auta.

**Příklad testu**

Použijte automobilů cena předpovědí jako vašeho příkladu regresní. Snažíte se předpovídání cena Auto na základě jeho funkcí, včetně nastavení, druh paliva, typ textu a jednotka kola. Obrázek 11 zobrazený testu.

![Experiment regresní automobilů cena](./media/machine-learning-interpret-model-results/11.png)

Obrázek 11. Experiment problém regresní automobilů cena

Vizualizace s používáním [Modelu skóre] [ score-model] modul, výsledek bude vypadat obrázek 12.

![Hodnocení výsledků problému předpovědí automobilů cena](./media/machine-learning-interpret-model-results/12.png)

Obrázek 12. Hodnocení výsledků problému s předpovědí automobilů cena

**Výsledek vyhodnocení**

Popisky dosáhne je sloupci výsledek v bodování výsledek. Předpokládané cena pro každou auta jsou čísla.

**Publikování webové služby**

Můžete publikovat experiment regresní do webové služby a zavolejte ho pro předpovědi automobilů cena stejným způsobem jako v případě použití dvou třídy klasifikace.

![Hodnocení experiment automobilů cena regresní problém](./media/machine-learning-interpret-model-results/13.png)

Obrázek 13. Hodnocení experiment problém regresní automobilů cena

Služba web vrácený výsledek vypadá jako obrázek 14. Předpokládané cena pro tento auta je $15,085.52.

![Interpretace bodování modul test](./media/machine-learning-interpret-model-results/13_1.png)

![Hodnocení výsledků modul](./media/machine-learning-interpret-model-results/14.png)

Obrázek 14. Web služby výsledků problému regresní automobilů cena

## <a name="clustering"></a>Clusterů

**Příklad testu**

Pojďme uvedenou množinu dat Iris znovu použít k vytvoření clusterů testu. Tady můžete filtrovat, předmětu popisky v uvedenou množinu dat tak, aby jenom má funkce a se dá použít pro clusterů. V tomto iris případu použití, zadejte počet clusterů být dvě během procesu školení, což znamená, že by clusteru květiny do dvou tříd. Obrázek 15 zobrazený testu.

![Vyzkoušejte Iris clusterů problém](./media/machine-learning-interpret-model-results/15.png)

Obrázek 15. Vyzkoušejte Iris clusterů problém

Clusterů se liší od klasifikace, že uvedenou množinu dat školení nemá základu pravdy popisky samostatně. Clusterů skupiny instancí uvedenou množinu dat školení do různých clusterů. Během procesu školení modelu popisky položek ve výukových rozdíly mezi jejich funkce. Až to školení modelu mohou sloužit k další klasifikaci budoucí položky. Tvoří dvě části výsledek, který jsme zajímají v rámci clusterů problém. První část je popisků uvedenou množinu dat školení a druhý je klasifikaci novou sadu dat s školení modelu.

První část výsledek můžete vizualizovat kliknutím na levém výstupní port [vlaku clusterů] modelu[ train-clustering-model] a pak kliknete na **vizualizace**. Vizualizace se zobrazují v obrázek 16.

![Clusterů výsledek](./media/machine-learning-interpret-model-results/16.png)

Obrázek 16. Vizualizace clusterů výsledek pro uvedenou množinu dat školení

Výsledek druhé části clusterů nové položky s školení clusterů modelu se zobrazuje obrázek 17.

![Vizualizace clusterů výsledek](./media/machine-learning-interpret-model-results/17.png)

Obrázek 17. Vizualizace clusterů výsledek na novou sadu dat

**Výsledek vyhodnocení**

Přestože výsledky ze dvou částí přerušili z různých experiment fází, vypadat stejně a interpretace stejným způsobem. První čtyři sloupce jsou funkce. Poslední sloupec přiřazení, je výsledek předpovídání pomocí. Položky přiřazené stejné číslo budou předpovídat byli ve stejném clusteru, to znamená, že mají být sdíleny podobnosti nějak (Tento experiment používá míru Euclidean vzdálenost výchozí). Protože zadaný počet clusterů 2 označené položky v přiřazení 0 či 1.

**Publikování webové služby**

Můžete publikovat clusterů testu do webové služby a zavolejte ho pro clusterů předpovědí případu použití, stejně jako klasifikace dvě předmětu.

![Hodnocení experiment iris clusterů problém](./media/machine-learning-interpret-model-results/18.png)

Obrázek 18. Hodnocení experiment clusterů problém iris

Po spuštění webové služby vrácený výsledek bude vypadat 19 obrázek. Tento květina předpovídat být clusteru 0.

![Test interpretovat bodování modul](./media/machine-learning-interpret-model-results/18_1.png)

![Hodnocení modul výsledek](./media/machine-learning-interpret-model-results/19.png)

Obrázek 19. Web služby výsledek iris dvě třídy klasifikace

## <a name="recommender-system"></a>Doporučení systému
**Příklad testu**

Doporučení systému, můžete použít problém doporučení restaurace jako příklad: doporučujeme restaurace pro zákazníky se sídlem na historii hodnocení. Zadávání dat se skládá ze tří částí:

* Hodnocení restaurace od zákazníků
* Data o zákaznících funkce
* Data funkcí restaurace

Existuje několik způsobů jsme můžete dělat s [Vlaku Matchbox doporučení] [ train-matchbox-recommender] modul Azure počítač přečíst:

- Předpovídání hodnocení pro daný uživatel a položky
- Doporučujeme, abyste položek pro daného uživatele
- Najít uživatele související s daný uživatel
- Hledání položky vztahující se k dané položce

Můžete zvolit, co chcete udělat tak, že vyberete ze čtyř možností v nabídce **doporučení předpovědí identifikovat** . Tady můžete projděte si všechny čtyři scénáře.

![Doporučení matchbox](./media/machine-learning-interpret-model-results/19_1.png)

Typické experiment Azure počítače výukové pro systém doporučení vypadá jako obrázek 20. Informace o použití těchto doporučení systémové moduly najdete v tématu [vlaku matchbox doporučení] [ train-matchbox-recommender] a [doporučení matchbox skóre][score-matchbox-recommender].

![Experiment systém doporučení](./media/machine-learning-interpret-model-results/20.png)

Obrázek 20. Experiment systém doporučení

**Výsledek vyhodnocení**

**Předpovídání hodnocení pro daný uživatel a položky**

Výběrem **Předpovědí hodnocení** ve skupinovém rámečku **Typ předpovědí doporučení**pokládáte systému doporučení odhadnout hodnocení pro daný uživatel a položky. Vizualizace [Skóre Matchbox doporučení] [ score-matchbox-recommender] výstup vypadá obrázek 21.

![Skóre výsledek systému doporučení – hodnocení předpovědí](./media/machine-learning-interpret-model-results/21.png)

Obrázek 21. Vizualizace výsledku skóre systému doporučení – hodnocení předpovědí

První dva sloupce jsou položky uživatele páry poskytovanou vstupní data. Třetí sloupec je předpovídané hodnocení uživatele určité položky. Například v prvním řádku zákazníka U1048 je předpovídat pro kurz restaurace 135026 jako 2.

**Doporučujeme, abyste položek pro daného uživatele**

Výběrem **Položky doporučení** ve skupinovém rámečku **Typ předpovědí doporučení**žádáte systému doporučení doporučit položek pro daného uživatele. Poslední parametr zvolte v tomto scénáři je *Výběr položek doporučené*. Možnost **Z hodnocení položek (model hodnocení)** je určen pro model hodnocení během procesu školení. U této fázi prediktivní vkládání jsme zvolit **Ze všech položek**. Vizualizace [Skóre Matchbox doporučení] [ score-matchbox-recommender] výstup vypadá obrázek 22.

![Výsledek skóre systému doporučení – položku doporučení](./media/machine-learning-interpret-model-results/22.png)

Obrázek 22. Vizualizace skóre výsledek systému doporučení – položku doporučení

První šest sloupce představuje daný uživatel ID doporučit položek, podle vstupní data. Pět sloupců představují položky doporučené uživateli sestupně podle relevance. Doporučené restaurace pro zákazníka U1048 je v prvním řádku 134986, následovaný 135018, 134975, 135021 a 132862.

**Najít uživatele související s daný uživatel**

Výběrem **Související uživatelé** v části **Typ předpovědí doporučení**žádáte systému doporučení zobrazíte související uživatelům daného uživatele. Související uživatelé můžou uživatelé, kteří mají podobný předvolby. Poslední parametr zvolte v tomto scénáři je *Výběr související uživatele*. Možnost **Z, že hodnocení položky uživatelů (pro model hodnocení)** je určen pro model hodnocení během procesu školení. Vyberte **Z všichni uživatelé** fázi předpovědí. Vizualizace [Skóre Matchbox doporučení] [ score-matchbox-recommender] výstup vypadá obrázek 23.

![Výsledek skóre systému doporučení – související uživatelé](./media/machine-learning-interpret-model-results/23.png)

Obrázek 23. Zobrazení výsledků skóre systému doporučení – související uživatelé

První šest sloupce obsahuje daný uživatel že ID potřebné k vyhledání související uživatelé podle vstupní data. Pět sloupců obsahují předpovídané související uživatelé uživatele sestupně podle relevance. Nejrelevantnější zákazníka pro zákazníka U1048 je v prvním řádku U1051, následovaný U1066, U1044, U1017 a U1072.

**Hledání položky vztahující se k dané položce**

Výběrem **Související položky** ve skupinovém rámečku **Typ předpovědí doporučení**pokládáte systému doporučení najdete související položky pro danou položku. Související položky jsou nejčastěji se dali lajk stejné uživatelem položky. Poslední parametr zvolte v tomto scénáři je *Výběr související položky*. Možnost **Z hodnocení položek (model hodnocení)** je určen pro model hodnocení během procesu školení. Jsme zvolte **Ze všech položek** pro tuto fázi předpovědí. Vizualizace [Skóre Matchbox doporučení] [ score-matchbox-recommender] výstup vypadá 24 obrázek.

![Výsledek skóre systému doporučení – souvisejících položek](./media/machine-learning-interpret-model-results/24.png)

Obrázek 24. Zobrazení výsledků skóre systému doporučení – souvisejících položek

První šest sloupce představuje dané položky ID potřebné k vyhledání souvisejících položek podle vstupní data. Pět sloupců obsahují předpovídané související položky položky v sestupném pořadí z hlediska relevance. Například v prvním řádku je nejrelevantnější položka u položky 135026 135074, následovaný 135035, 132875, 135055 a 134992.

**Publikování webové služby**

U každé čtyři scénáře podobná procesu publikování těmito pokusy jako webovým službám získat předpovědí. Tady jsme trvat druhý scénář (doporučená položek pro daného uživatele) jako příklad. Můžete použijte stejný postup pomocí těchto tří.

Uložení systému školení doporučení modelu školení a filtrování zadávání dat ve sloupci ID jednoho uživatele na vyžádání, můžete připojit testu jako obrázek 25 a publikovat jako webové služby.

![Hodnocení experiment problém doporučení restaurace](./media/machine-learning-interpret-model-results/25.png)

Obrázek 25. Hodnocení experiment problém doporučení restaurace

Systém webové služby, vrácený výsledek bude vypadat 26 obrázek. Pět doporučené restaurace uživateli U1048 jsou 134986 135018, 134975, 135021 a 132862.

![Ukázka doporučení systémová služba](./media/machine-learning-interpret-model-results/25_1.png)

![Ukázka výsledků testu](./media/machine-learning-interpret-model-results/26.png)

Obrázek 26. Web služby výsledků problému doporučení restaurace


<!-- Module References -->
[assign-to-clusters]: https://msdn.microsoft.com/library/azure/eed3ee76-e8aa-46e6-907c-9ca767f5c114/
[execute-r-script]: https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/
[select-columns]: https://msdn.microsoft.com/library/azure/1ec722fa-b623-4e26-a44e-a50c6d726223/
[score-matchbox-recommender]: https://msdn.microsoft.com/library/azure/55544522-9a10-44bd-884f-9a91a9cec2cd/
[score-model]: https://msdn.microsoft.com/library/azure/401b4f92-e724-4d5a-be81-d5b0ff9bdb33/
[train-clustering-model]: https://msdn.microsoft.com/library/azure/bb43c744-f7fa-41d0-ae67-74ae75da3ffd/
[train-matchbox-recommender]: https://msdn.microsoft.com/library/azure/fa4aa69d-2f1c-4ba4-ad5f-90ea3a515b4c/
