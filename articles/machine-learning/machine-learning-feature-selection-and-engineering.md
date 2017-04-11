<properties
    pageTitle="Funkce inženýrské a výběr Azure počítač přečíst | Microsoft Azure"
    description="Tento článek vysvětluje účely výběr funkcí a inženýrské funkce a jsou uvedeny příklady jejich role v procesu rozšíření dat učení počítače."
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
    ms.author="zhangya;bradsev" />


# <a name="feature-engineering-and-selection-in-azure-machine-learning"></a>Inženýrské funkce a výběr Azure počítač přečíst

Toto téma vysvětluje účely inženýrské funkce a funkce výběru v procesu rozšíření dat učení počítače. Ukazuje, co tyto procesy zahrnují pomocí příklady poskytovanou Azure počítače výukové Studio.

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

Školení data použitá v počítači výukové je možné vylepšovat často výběru nebo extrahování funkcí jako nezpracovaná dat shromážděných. Příklad analýzou funkce v souvislosti se naučíte klasifikovat obrázky ručně psané znaky je vytvořen z data distribuce nezpracovanými bit mapou bit hustoty. Mapa můžete vyhledání okraji znaky mnohem efektivněji než nezpracovanými rozdělení.

Funkce analýzou a je zaškrtnuté zvýšení efektivity procesu školení pokusí extrahovat klíčové informace obsažené v datech. Také zvyšují power tyto modely přesně klasifikovat vstupních dat a předpovídání výsledky potřebné více robustní. Inženýrské funkce a výběru můžete také sloučit aby výuky více nelze tractable. Stejně tak zvýšení a snížení řadu funkcí, třeba kalibrovat nebo školení modelu. Matematicky mluvit funkce vybrané ke školení modelu jsou minimální sady proměnných, které popisují vzorky dat a potom úspěšně předpovídání výsledky.

Inženýrské funkce a výběr funkcí je jedna část větší proces, který obvykle se skládá ze čtyř kroků:

* Shromažďování dat
* Rozšíření dat
* Konstrukce modelu
* Po zpracování

Inženýrské funkce a výběr tvoří krok rozšíření dat učení počítače. Pro naše potřeby může rozlišují tři aspekty tohoto procesu:

* **Předzpracování dat**: Tento proces pokusí zajištění shromážděná data vyčistit a konzistentní. Zahrnuje úloh, jako je integrace více datových sad zpracování chybějící data, zpracování nekonzistentní data a převod datových typů.
* **Inženýrské funkce**: Tento proces pokusí vytvořit další relevantní funkce z existujících nezpracovanými funkcí v datech a zvýšit předpovídat do algoritmu učení.
* **Výběr funkcí**: Tento postup slouží k výběru klíčové podmnožinu původní data funkce zmenšit dimenze školení problém.

Toto téma popisuje pouze inženýrské funkce a funkce výběru aspekty procesu rozšíření dat. Další informace o kroku předem zpracování dat najdete v článku [předzpracování dat v Azure počítače výukové Studio](https://azure.microsoft.com/documentation/videos/preprocessing-data-in-azure-ml-studio/).


## <a name="creating-features-from-your-data--feature-engineering"></a>Vytváření funkce z dat – funkce inženýrské funkce

Školení data obsahují matice skládající se ze příklady (záznamů nebo poznámky, uložený v řádcích), z nichž každá obsahuje sadu funkcí (proměnné nebo pole uložené ve sloupcích). Očekává se, že funkce zadané v pokusné návrhu určení vzorky dat. Přestože spoustu nezpracovanými datových polí mohou být přímo součástí sady vybranou funkci použít ke školení modelu, analýzou funkce často potřeba vytvořen funkcí v původní data, která generovat sadu dat rozšířené školení.

Jaké funkce má vytvořen k vylepšení uvedenou množinu dat při školení modelu? Analýzou funkce, které vylepšují školení zadejte informace, lépe rozlišuje vzorky dat. Očekáváte nových funkcí, které poskytují další informace, které se nezaznamenávají, jasně nebo snadno poznat v sadě původní nebo existující funkce, ale tento proces se hodí obrázek. Zvuk a produktivní rozhodnutí často vyžadují některá všechno pod kontrolou domény.

Když začnete s Azure počítače výuka, je nejjednodušší pochopit tento proces namítají pomocí uvedené ve počítače výukové studiu. Dva příklady jsou uvedeny v tomto tématu:

* Příklad regresní ([předpověď počet bike splátky](http://gallery.cortanaintelligence.com/Experiment/Regression-Demand-estimation-4)) v kontrolované experiment kde je známo cílové hodnoty
* Příklad klasifikace dolování text pomocí [Funkce algoritmus hash][feature-hashing]

### <a name="example-1-adding-temporal-features-for-a-regression-model"></a>Příklad 1: Přidání časový funkcí pro regresní model ###

Ukázku funkcí pro daný úkol regresní analýzu, použijeme pokus "služba předpovídání jízdní kola" v Azure počítače výukové Studio. Cílem tento experiment je předpovídání služba za jízdní kola, to znamená počet bike splátky v určitém měsíci, den nebo hodinu. **Bike pronájem UC uvedenou množinu dat** uvedenou množinu dat se použije jako nezpracovaná vstupní data.

Této sadě dat na základě skutečné dat. z velké Bikeshare společnost, která spravuje sítě pronájem bike WA Datacentrum v USA. Uvedenou množinu dat představuje počet bike splátky v rámci konkrétní hodinu dne, 2011, aby 2012, která obsahuje 17379 řádků a sloupců 17. Sada funkcí jako nezpracovaná obsahuje počasí (teplotu, vlhkost, rychlost větru) a typ den (svátek nebo den v týdnu). Pole do předpovídání je **oznámení**, počet, která představuje splátky bike v zadané hodiny a který rozsahu od 1 do 977.

Vytvářet efektivní funkce ve vzdělávání dat, jsou čtyři modely regresní vytvořené pomocí stejného algoritmu, ale s čtyři různé školení datové sady. Čtyři datové sady představují stejný jako nezpracovaná zadávání dat, ale s rostoucí řadu funkcí, nastavit. Tyto funkce jsou seskupené do čtyř kategorií:

1. A = počasí svátků + den v týdnu + víkend funkcí pro předpovídané den
2. B = počet jízdní kola, které byly Propachtovaná v žádném z předchozích 12 hodin
3. C = počet jízdní kola, které byly Propachtovaná v žádném z předchozích 12 dnů v stejné hodinu
4. D = počet jízdní kola, které byly Propachtovaná v žádném z předchozí 12 týdnů na stejné hodiny a na stejný den

Kromě A sadu funkcí, které již existuje v původní nezpracovanými daty, jsou tyto tři sady funkcí vytvořený prostřednictvím funkci konstrukce proces. Funkce nastavit B zachycení poslední služba kola. Funkce nastavit zachycený obsah C služba za jízdní kola v konkrétní hodinu. Funkce nastavení D zachycený obsah služba za jízdní kola na konkrétní hodina a na určitý den v týdnu. Každý čtyři datové sady školení obsahuje sady funkcí A, A + B, A + B + C a A + B + C + D, respektive.

V testu Azure počítače výukové jsou tyto čtyři datové sady školení vytvořený prostřednictvím čtyři větve z předem zpracovaných vstupní uvedenou množinu dat. S výjimkou větvi vlevo jednotlivých větví obsahuje [Spustit skript R] [ execute-r-script] modul, ve kterém odvozeno sadu funkcí (funkce sady B, C a D) je vytvořen a přidaným k importovaných uvedenou množinu dat. Následující obrázek znázorňuje R skript slouží k vytváření sadu funkcí B na druhé levého větve.

![Vytvoření sady funkcí](./media/machine-learning-feature-selection-and-engineering/addFeature-Rscripts.png)

Následující tabulka shrnuje porovnání výsledky čtyři modelů. Nejlepších výsledků se zobrazí funkcemi A + B + C. Všimněte si, že míru chyb sníží po sady další funkce jsou součástí školení data. Tím se ověří naše předpokládá, že sady funkcí B a C zadejte další relevantní informace pro daný úkol regresní. Přidání sadu funkcí D nefunguje zadejte jakékoli další snížení sazeb chyby.

![Výsledky porovnání](./media/machine-learning-feature-selection-and-engineering/result1.png)

### <a name="example2"></a>Příklad 2: Vytvoření funkcí v dolování textu  

Inženýrské funkce platí široce ve složce úkoly týkající se dolování text, například dokument klasifikace a myšlenkou analýzy. Když budete chtít klasifikaci dokumentů do několika kategorií, typické předpokladů je například slova nebo fráze zahrnuté v jedné kategorii dokumentu se méně ke kterým může dojít v jiném dokumentu kategorii. Jinými slovy četnost rozdělení slovo nebo slovní spojení je možné k určení kategorie jiným dokumentem. V aplikacích dolování text funkci konstrukce proces potřebné k vytvoření funkce týkající se frekvencí slovo nebo slovní spojení, protože jednotlivé použitelné části obsahu text obvykle slouží jako vstupní data.

Dosáhnout tento úkol postup s názvem *algoritmus hash funkce* platí pro efektivní přeměna indexy funkce libovolný text. Místo konkrétní Indexovat tato metoda funkce použitím funkce hash funkcí a jejich hodnoty hash jako indexy přímo pomocí přidružení každou funkcí text (slova nebo fráze).

Azure počítač přečíst, je [Funkce algoritmus hash] [ feature-hashing] moduly, které vytvoří tyto funkce slovo nebo frázi. Následující obrázek ukazuje příklad použití tento modul. Zadávání uvedenou množinu dat obsahuje dva sloupce: knihy hodnocení od 1 do 5 a skutečné kontrola obsahu. Cílem této [Funkce algoritmus hash] [ feature-hashing] modul je k načtení nových funkcí, které ukazují četnost výskyt odpovídající slova nebo fráze v rámci revize určitý sešit. Pokud chcete použít tento modul, budete muset proveďte následující kroky:

1. Vyberte sloupec obsahující zadávání textu (**Sloupec2** v tomto příkladu).
2. Nastavit *Hashing bitsize* 8, což znamená, 2 ^ 8 = 256 vytvořené funkcí. Slovo nebo slovní spojení v textu je pak hešováno 256 indexy. Parametr *Hashing bitsize* rozsahu od 1 do 31. Pokud parametr nastaven na většího počtu, zadaná slova nebo fráze mívají méně hash do stejné index.
3. Nastavení parametrů *N-g* 2. To načte výskyt četnost unigrams (funkce pro každého jednoho slova) a bigrams (funkce pro každou dvojici sousedních slov) ze zadávání textu. Parametr *N-g* rozsah od 0 do 10, což znamená maximální počet sekvenční slova, která chcete zahrnout funkci.  

![Funkce hash modul](./media/machine-learning-feature-selection-and-engineering/feature-Hashing1.png)

Následující obrázek znázorňuje, jak tyto nové funkce vypadat.

![Příklad funkce hash](./media/machine-learning-feature-selection-and-engineering/feature-Hashing2.png)

## <a name="filtering-features-from-your-data--feature-selection"></a>Funkce z dat – výběr funkcí filtrování  ##

*Výběr funkcí* je proces běžně používaná konstrukce sady dat školení pro úkoly prediktivní modelování například klasifikace a regresní úkolů. Cílem je vybrat podmnožinu funkce z původního uvedenou množinu dat, která sníží jeho rozměry pomocí minimální sadu funkcí, která představuje maximální velikost odchylka v datech. Tato část funkce obsahuje pouze funkce chcete zahrnout do školení modelu. Výběr funkcí má dva hlavní účely:

* Výběr funkcí často zvyšuje klasifikace přesnosti vyloučením relevantní, nadbytečné nebo vysoce vazba funkce.
* Výběr funkcí snižuje řadu funkcí, které je proces školení modelu efektivnější. Toto je velmi důležitým pro žáky, které jsou drahé školení například podpora vector počítačích.

Přestože výběr funkcí se snaží snižte počet funkce uvedenou množinu dat použít ke školení modelu, ho není uvedená obvykle výrazem *dimenzionalitu snížení.* Funkce Výběr metody extrahovat podmnožinu původní funkcí v datech beze změny je.  Způsobů snížení dimenzionalitu využívat analýzou funkce, které můžete transformace původní funkcí a tedy upravovat. Příklady způsobů snížení dimenzionalitu: hlavní komponenty analýzy, kanonický srovnávací analýza a dekompozice jednotném hodnotu.

Jednu široce použité kategorii funkcí Výběr metody v rámci sledovaných je výběr systémem filtru funkcí. Určování korelace každou funkcí a cílový atribut, použití těchto postupů statistické míru přiřadit skóre každou funkcí. Funkce jsou potom jde řadit podle skóre, které slouží k nastavení mezní hodnoty pro zachování nebo odstranění určité funkce. Statistické rozměry použité v těchto metodách příkladem Pearsonova korelačního, vzájemné informace a rozdělení chí-testu.

Azure Studio výukové počítače obsahuje moduly pro výběr funkcí. Jak je znázorněno na následujícím obrázku, moduly zahrnout [systémem filtru výběr funkcí] [ filter-based-feature-selection] a [Fisher lineární Discriminant analýza][fisher-linear-discriminant-analysis].

![Příklad funkce výběru](./media/machine-learning-feature-selection-and-engineering/feature-Selection.png)


[Výběr funkce založený na filtr] ji například používat[ filter-based-feature-selection] modulu s příklad dolování textu uvedené dříve. Předpokládejme, že budete chtít vytvořit regresní model sadu 256 funkcí je vytvořený prostřednictvím [Funkce algoritmus hash] [ feature-hashing] modul, že proměnnou odpověď je a **Sloupec1** představuje knize zkontrolujte hodnocení od 1 do 5. Nastavit **funkce bodování metoda** **Pearsonova korelačního**, **cílovém sloupci** , který chcete **Sloupec1**a **počet požadované funkce** **50**. Modul [systémem filtru výběr funkcí] [ filter-based-feature-selection] vytvoří k sadám dat obsahujících 50 funkcí spolu s cílový atribut **Sloupec1**. Následující obrázek znázorňuje tok tento testu a vstupních parametrů.

![Příklad funkce výběru](./media/machine-learning-feature-selection-and-engineering/feature-Selection1.png)

Následující obrázek znázorňuje výsledné datové sady. Každou funkcí je dosáhne založené na Pearsonova korelačního mezi sebe sama a cílový atribut **Sloupec1**. Funkce s nejvyšší skóre zachovány.

![Sady dat na základě filtr funkce výběru](./media/machine-learning-feature-selection-and-engineering/feature-Selection2.png)

Následující obrázek znázorňuje odpovídající skóre vybrané funkcí.

![Vybranou funkci skóre](./media/machine-learning-feature-selection-and-engineering/feature-Selection3.png)

Použitím tohoto [systémem filtru výběr funkcí] [ filter-based-feature-selection] modulu 50 mimo 256 funkce nejsou vybrány, protože mají nejvíce funkce vazba s cílem proměnné **Sloupec1** založené na metodě bodování **Pearsonova korelačního**.

## <a name="conclusion"></a>Uzavření
Inženýrské funkce a funkce výběru jsou dva kroky běžně za účelem Příprava dat školení při vytváření modelu výukové počítače. Za normálních okolností inženýrské funkce se použije první generovat další funkce a potom provedený krok výběru funkce eliminuje relevantní, nadbytečné nebo vysoce vzájemném vztahu funkce.

Není vždy nemusí provádět výběr funkcí inženýrské funkce a funkce. Jestli je potřeba závisí na data či shromáždit, algoritmu, které jste vybrali a cíle testu.


<!-- Module References -->
[execute-r-script]: https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/
[feature-hashing]: https://msdn.microsoft.com/library/azure/c9a82660-2d9c-411d-8122-4d9e0b3ce92a/
[filter-based-feature-selection]: https://msdn.microsoft.com/library/azure/918b356b-045c-412b-aa12-94a1d2dad90f/
[fisher-linear-discriminant-analysis]: https://msdn.microsoft.com/library/azure/dcaab0b2-59ca-4bec-bb66-79fd23540080/
