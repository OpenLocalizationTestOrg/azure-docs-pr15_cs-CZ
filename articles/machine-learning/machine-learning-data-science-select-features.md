<properties
    pageTitle="Funkce výběru v procesu týmu dat pro výzkum | Microsoft Azure" 
    description="Tento článek vysvětluje účel výběr funkcí a jsou uvedeny příklady jejich role v procesu rozšíření dat učení počítače."
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
    ms.date="09/19/2016"
    ms.author="zhangya;bradsev" />


# <a name="feature-selection-in-the-team-data-science-process-tdsp"></a>Výběr funkcí v týmu dat pro výzkum obrázku (TDSP)

Tento článek vysvětluje účely výběr funkcí a jsou uvedeny příklady jeho role v procesu rozšíření dat učení počítače. Tyto příklady jsou nakreslené z Azure počítače výukové Studio. 

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]


Toto téma vysvětluje účel výběr funkcí a jsou uvedeny příklady jeho role v procesu rozšíření dat učení počítače. Tyto příklady jsou nakreslené z Azure počítače výukové Studio. 

Inženýrské funkce a výběr funkcí je jedna část TDSP uvedené v [jak se postupuje týmu dat pro výzkum?](data-science-process-overview.md). Inženýrské funkce a výběr jsou části **vývoje funkce** krok TDSP.

* **inženýrské funkce**: Tento proces pokusí vytvořit další relevantní funkce z existujících nezpracovanými funkcí v datech a zvýšit předpovídat do algoritmu učení.

* **Výběr funkcí**: Tento proces vybere klíčové podmnožinu původní funkce dat při pokusu o zmenšení dimenze školení problém.

Obvykle **inženýrské funkce** platí nejprve získat další funkce a potom provedený krok **Výběr funkcí** eliminuje relevantní, nadbytečné nebo vysoce vzájemném vztahu funkce.


## <a name="filtering-features-from-your-data---feature-selection"></a>Filtrovací funkce z dat – výběr funkcí 

Výběr funkcí je proces běžně používaná stavbu datové sady školicí kurzy pro modelování prediktivní úkoly například klasifikace nebo regresní úkoly. Cílem je vybrat podmnožinu funkce z původního sady dat, která omezit jeho rozměry představuje maximální velikost odchylka v datech pomocí minimální sadu funkcí. Tento podmnožinu funkce jsou, pak pouze funkce zahrnuty školení modelu. Výběr funkcí má dva hlavní účely.

* Nejdřív výběr funkcí často zvyšuje klasifikace přesnost odstraněním relevantní, nadbytečné nebo vysoce vazba funkce.
* Za druhé snižuje se počet funkce, takže proces školení modelu efektivnější. Toto je velmi důležitým pro žáky, které jsou drahé školení například podpora vektorové počítačích.

Přestože výběr funkcí snažit snižte počet funkcí v sadě dat použít ke školení modelu, ho není uvedené obvykle termínem "dimenzionalitu snížení". Funkce Výběr metody extrahovat podmnožinu původní funkcí v datech beze změny je.  Způsobů snížení dimenzionalitu využívat analýzou funkce, které můžete transformace původní funkcí a tedy upravovat. Příklady způsobů snížení dimenzionalitu: hlavní komponenty analýzy, kanonický srovnávací analýza a jednotném rozložené hodnotu.

Mimo jiné jednu široce použité kategorii funkcí Výběr metody v rámci sledovaných jmenuje "Výběr funkcí Filtr". Určování korelace každou funkcí a cílový atribut, použití těchto postupů statistické míru přiřadit skóre každou funkcí. Funkce potom řazení podle skóre, které mohou být použity vám pomůže vytvořit mezní hodnoty pro zachování nebo odstranění určité funkce. Statistické rozměry použité v těchto metodách příkladem korelace, vzájemné informace a čtverců test Kuba uživatele.

V Azure počítače výukové Studio jsou modulech pro výběr funkcí. Jak je znázorněno na následujícím obrázku, moduly zahrnout [systémem filtru výběr funkcí] [ filter-based-feature-selection] a [Fisher lineární Discriminant analýza][fisher-linear-discriminant-analysis].

![Příklad funkce výběru](./media/machine-learning-data-science-select-features/feature-Selection.png)


Například, zvažte použití [systémem filtru výběr funkcí] [ filter-based-feature-selection] modul. Za účelem usnadnění doporučujeme dál používat příklad dolování textu výše uvedených. Předpokládejme, že chceme k vytvoření modelu regresní sadu 256 funkce vytvořené pomocí [Funkce algoritmus hash] [ feature-hashing] modul, že proměnnou odpověď je a "Sloupec1" představuje knize zkontrolujte hodnocení od 1 do 5. Nastavením "Funkce bodování metoda" je "Pearsonova korelačního", "cílovém sloupci" je "Sloupec1" a "Počet požadované funkce" až 50. Potom modulu [systémem filtru výběr funkcí] [ filter-based-feature-selection] vytvoří datovou sadu obsahující 50 funkcí spolu s cílový atribut "Sloupec1". Následující obrázek znázorňuje tok tento testu a vstupních parametrů, které právě popsány.

![Příklad funkce výběru](./media/machine-learning-data-science-select-features/feature-Selection1.png)

Následující obrázek znázorňuje výsledné datové sady. Každou funkcí je dosáhne založené na Pearsonova korelačního mezi sebe sama a cílový atribut "Sloupec1". Funkce s nejvyšší skóre zachovány.

![Příklad funkce výběru](./media/machine-learning-data-science-select-features/feature-Selection2.png)

Odpovídající skóre vybraných součástí jsou zobrazeny na následujícím obrázku.

![Příklad funkce výběru](./media/machine-learning-data-science-select-features/feature-Selection3.png)

Použitím tohoto [systémem filtru výběr funkcí] [ filter-based-feature-selection] modulu 50 mimo 256 funkce nejsou vybrány vzhledem k tomu, že mají nejčastěji vzájemném vztahu funkce bez proměnnou cílový "Sloupec1", na základě hodnocení způsobu "Pearsonova korelačního".

## <a name="conclusion"></a>Uzavření
Inženýrské funkce a funkce výběru jsou dvě obecně zpětnou analýzu a vybraných funkcí zvýšení efektivity proces školení, které pokusy o extrahování klíčové informace obsažené v datech. Také zvyšují power tyto modely přesně klasifikovat vstupních dat a předpovídání výsledky potřebné více robustní. Inženýrské funkce a výběru můžete také sloučit aby výuky více nelze tractable. Stejně tak zvýšení a snížení řadu funkcí, třeba kalibrovat nebo školení modelu. Matematicky mluvit funkce vybrané ke školení modelu jsou minimální sady proměnných, které popisují vzorky dat a potom úspěšně předpovídání výsledky.

Všimněte si, že není vždy nemusí provádět výběr funkcí inženýrské funkce a funkce. Jestli je to potřeba nebo ne závisí na jsme mít nebo shromáždit data, algoritmu, kterou vybereme a cíle testu.

<!-- Module References -->
[feature-hashing]: https://msdn.microsoft.com/library/azure/c9a82660-2d9c-411d-8122-4d9e0b3ce92a/
[filter-based-feature-selection]: https://msdn.microsoft.com/library/azure/918b356b-045c-412b-aa12-94a1d2dad90f/
[fisher-linear-discriminant-analysis]: https://msdn.microsoft.com/library/azure/dcaab0b2-59ca-4bec-bb66-79fd23540080/
 
