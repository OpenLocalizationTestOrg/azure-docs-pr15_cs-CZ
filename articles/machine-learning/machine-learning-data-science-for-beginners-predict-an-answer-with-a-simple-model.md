<properties
   pageTitle="Předpovídání odpověď s jednoduchého modelu - regresní model | Microsoft Azure"
   description="Jak vytvořit jednoduchý regresní model odhadnout cenou v jiné vědecké dat pro začátečníky video 4. Obsahuje lineární regrese cílové daty."                                  
   keywords="Vytvoření modelu, jednoduchého modelu, předpovědí cena, jednoduché regresní model"
   services="machine-learning"
   documentationCenter="na"
   authors="cjgronlund"
   manager="jhubbard"
   editor="cjgronlund"/>

<tags
   ms.service="machine-learning"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/20/2016"
   ms.author="cgronlun;garye"/>

# <a name="predict-an-answer-with-a-simple-model"></a>Předpovídání odpověď s jednoduchého modelu

## <a name="video-4-data-science-for-beginners-series"></a>Video 4: Výzkum Data pro začátečníky řady

Zjistěte, jak vytvořit jednoduchý regresní model odhadnout cena kosočtverce v jiné vědecké dat pro začátečníky video 4. Jsme odebíráním regresní model cílové daty.

Využívejte řadě, můžete sledujte všechny. [Přejděte do seznamu videí](#other-videos-in-this-series)

> [AZURE.VIDEO data-science-for-beginners-series-predict-an-answer-with-a-simple-model]

## <a name="other-videos-in-this-series"></a>Další videa v této řadě

*Výzkum dat pro začátečníky* je rychlý úvod a jiné vědecké dat v pěti krátká videa.

  * Video 1: [Jiné vědecké dat 5 otázky a odpovědi](machine-learning-data-science-for-beginners-the-5-questions-data-science-answers.md) *(5 minut 14 sekund)*
  * Video 2: [hotovou dat pro výzkum dat?](machine-learning-data-science-for-beginners-is-your-data-ready-for-data-science.md) *(4 minut 56 sekund)*
  * Video 3: [Zadání dotazu můžete odpovědět s daty](machine-learning-data-science-for-beginners-ask-a-question-you-can-answer-with-data.md) *(4 17 s min)*
  * Video 4: Odpověď pomocí jednoduchého modelu předpovídání
  * Video 5: [Kopírování ostatních lidí práce pro výzkum dat](machine-learning-data-science-for-beginners-copy-other-peoples-work-to-do-data-science.md) *(3 18 s min)*

## <a name="transcript-predict-an-answer-with-a-simple-model"></a>Přepis: Odpověď pomocí jednoduchého modelu předpovídání

Vítá vás čtvrtý videa "Data pro výzkum pro začátečníky" řady. V tuto položku jsme budete vytvoření jednoduchého modelu a předpověď.

*Model* je zjednodušené dlouhé zprávy o naše data. Můžu se zobrazí můžu významu.

## <a name="collect-relevant-accurate-connected-enough-data"></a>Shromáždit důležité, přesné, připojení, dost dat

Předpokládejme, že chci středisko pro kosočtverce. Mám zvonění patřil Moje NO@LOC s nastavením pro kosočtverce 1.35 stříšky a chcete získat přehled o kolik to bude stát. Brát pera a Poznámkový blok v úložišti špercích a můžu zapisovat cenu všech kosočtverce v případě a kolik porovnali v carats. Počínaje prvním kosočtverec - 1.01 carats a $7,366.

Teď můžu Procházet a postup pro všechny ostatní kosočtverce v úložišti.

![Sloupce dat kosočtverec](./media/machine-learning-data-science-for-beginners-predict-an-answer-with-a-simple-model/diamond-data.png)

Všimněte si, že naše seznam obsahuje dva sloupce. Každý sloupec oblasti má jiný atribut - weight (váha) v carats a cena - a každý řádek je jeden datový bod, který představuje jeden kosočtverce.

Ve skutečnosti vytvořili jsme malý dat tady si můžete nastavit - tabulky. Všimněte si, že splňuje naše kritéria kvality:

* Data jsou **důležité** - weight (váha) jasné souvisí s cenou
* Je **přesné** – jsme double-checked ceny, které jsme zapisovat
* Je **připojení** – nejsou žádné prázdné mezery v některém z těchto sloupců
* A jak si ukážeme, je **dostatečně** data, která chcete odpovědět na naše otázku

## <a name="ask-a-sharp-question"></a>Zeptejte se ostrý

Teď můžeme budete představovat naše otázku ostrý způsobem: "kolik to bude stát koupit kosočtverce 1.35 stříšky?"

Náš seznam nemá kosočtverce 1.35 stříšky, takže máme budete používat zbytek naše data dostanete odpověď na dotaz.

## <a name="plot-the-existing-data"></a>Vykreslovat existující data

První věc, kterou jsme odvedou je nakreslení vodorovné čáry číslo s názvem OS do grafu gramáží. Oblast gramáží je 0 až 2, takže jsme odebíráním řádek, které bude pokrývat oblasti a umístění značky pro každou půlkruhy stříšky.

Další jsme odebíráním svislá osa nahrajte cenu a připojte ji k weight (váha) vodorovné osy. To bude v jednotkách korun. Teď máme sadu souřadnic osy.

![Weight (váha) a cena a osy](./media/machine-learning-data-science-for-beginners-predict-an-answer-with-a-simple-model/weight-and-price-axes.png)

Chceme se tato data a přeměna do *bodového vykreslit*. Toto je skvělý způsob, jak vizualizace číselné datové sady.

Pro prvním datovém bodě jsme eyeball svislou čáru na 1.01 carats. Potom jsme eyeball vodorovnou čáru na $7,366. Jestliže splňují, jsme nakreslete tečky. Představuje naše první kosočtverec.

Teď projít jednotlivé kosočtverec v tomto seznamu a dělají totéž. Když je nám to až, je to jsme získat: spoustu tečky, pro každou kosočtverec.

![Bodový graf](./media/machine-learning-data-science-for-beginners-predict-an-answer-with-a-simple-model/scatter-plot.png)

## <a name="draw-the-model-through-the-data-points"></a>Kreslení modelu pomocí datových bodů

Teď když si prohlédnete tečky a squint, kolekci vypadá tuku rozmazaný čáru. Můžeme udělat naše značka a nakreslit rovnou čáru přes něj.

Nakreslením čáry jsme vytvořili *modelu*. Přemýšlejte jako přijímat skutečné a provádění zneužívající vlastností prohlížeče kreslený verzi souboru. Teď kreslený je špatně - řádku nemá absolvovat všem datovým bodům. Ale je užitečné zjednodušení.

![Lineární regresní čáry.](./media/machine-learning-data-science-for-beginners-predict-an-answer-with-a-simple-model/linear-regression-line.png)

To, že všechny tečky nejsou přesně absolvovat řádku je v pořádku. Data vědeckých vysvětlit to tak, že oznamující, že modelu, -, která je řádku - a potom má každá tečka některých *šum* nebo *Odchylka* přidružená. Je základní správný vztah a pak je krupičnaté, real světě, která přidá šum a nejistoty.

Protože jsme snažíte k odpovídání otázek *kolik?* je místo toho *regresní*. A pracujeme s programem rovnou čáru, a proto je *lineární regrese*.

## <a name="use-the-model-to-find-the-answer"></a>Použití modelu odpovědi

Teď máme modelu a jsme zeptejte se ho naše: jaké budou kosočtverce 1.35 stříšky náklady?

Přijmout naše otázku, jsme oka 1.35 carats a nakreslete svislou čáru. Pokud se ve výkresu protínají řádku modelu jsme eyeball vodorovné čáry na osu Kč. Narazí doprava na 10 000. Výložníku! To je odpověď: kosočtverce 1.35 stříšky náklady asi 10 000 Kč.

![Nalezení správné odpovědi na modelu](./media/machine-learning-data-science-for-beginners-predict-an-answer-with-a-simple-model/find-the-answer.png)

## <a name="create-a-confidence-interval"></a>Vytvoření intervalu spolehlivosti

Je přirozený zajímat, jak přesné je tento předpovědí. Je dobré vědět, jestli kosočtverec 1.35 stříšky budou velmi podobné 10 000 Kč, nebo výrazně nahoru nebo dolů. K určení prodejce to zjistit, Pojďme nakreslete obálku kolem regresní přímky obsahující nejčastěji teček. Tento obálky se nazývá naše *interval spolehlivosti*: protože jsme poměrně jistotu, že ceny spadají do této obálky v uplynulém většina z nich. Jsme upoutat dvou více vodorovných čar ze kdy 1.35 stříšky přímka protíná horní a dolní části této obálky.

![Interval spolehlivosti](./media/machine-learning-data-science-for-beginners-predict-an-answer-with-a-simple-model/confidence-interval.png)

Teď můžete říkáme něco o naše interval spolehlivosti: můžete říkáme jistotou, že je cena kosočtverce 1.35 stříšky asi $ 10 000 - ale může být co nejnižší 8 000 Kč a může být dosahovat $ 12 000.

## <a name="were-done-with-no-math-or-computers"></a>Jsme hotovi, bez matematické a počítače

My sami, jaká data vědeckých platby udělat a my sami pouhým výkresu:

* Jsme dotaz otázku, abychom mohli odpovědět s daty
* Jsme vytvořili *modelu* pomocí *lineární regrese*
* Jsme udělali *předpovědí*, včetně *intervalu spolehlivosti*

A neměli používáme matematické nebo počítače na to.

Teď můžeme vám to měli další informace, jako...

* Střih kosočtverec
* barevné variace (jak blízko kosočtverce se kterým bílých)
* počet zahrnutí v kosočtverec

.. .pak znova jsme byste měli další sloupce. V takovém případě matematické se změní na užitečné. Pokud máte víc než dva sloupce, je špatně nakreslete tečky na papír. Matematický výraz umožňuje přizpůsobit tento řádek nebo že rovině k datům velmi dobře.

Také pokud místo jenom hrstku kosočtverce, bylo dva tisíce nebo 2 000 a potom můžete udělat, které fungují rychleji s počítačem.

Dnes jsme jste kontakt o tom, jak se lineární regrese a jsme udělali předpověď pomocí data.

Ujistěte se, podívejte se na další videa v "Data pro výzkum pro začátečníky" z webu Microsoft Azure počítače Learning.



## <a name="next-steps"></a>Další kroky

  * [Opakujte první pokusy vědy dat s počítače výukové Studio](machine-learning-create-experiment.md)
  * [Získat Úvod do počítače výukové na Microsoft Azure](machine-learning-what-is-machine-learning.md)
