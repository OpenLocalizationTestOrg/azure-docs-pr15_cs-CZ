<properties
   pageTitle="Je datům umístěným dat pro výzkum? Hodnocení dat | Microsoft Azure"
   description="Přečtěte si 4 kritéria pro data do provozu pro výzkum data. Výzkum dat pro začátečníky video 2 má konkrétní příklady pomoci při vyhodnocování základní údajů."
   keywords="relevantních dat, práci s daty, připravte data, kritéria dat, data připravená"
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


# <a name="is-your-data-ready-for-data-science"></a>Je datům umístěným dat pro výzkum?

## <a name="video-2-data-science-for-beginners-series"></a>Video 2: Výzkum Data pro začátečníky řady

Zjistěte, jak chcete zjistit data zkontrolujte, že splňuje základní kritéria, jste připraveni na dat pro výzkum.

Využívejte řadě, můžete sledujte všechny. [Přejděte do seznamu videí](#other-videos-in-this-series)

> [AZURE.VIDEO data-science-for-beginners-series-is-your-data-ready-for-data-science]

## <a name="other-videos-in-this-series"></a>Další videa v této řadě

*Výzkum dat pro začátečníky* je rychlý úvod a jiné vědecké dat v pěti krátká videa.

  * Video 1: [Jiné vědecké dat 5 otázky a odpovědi](machine-learning-data-science-for-beginners-the-5-questions-data-science-answers.md) *(5 minut 14 sekund)*
  * Video 2: Hotovou dat pro výzkum dat?
  * Video 3: [Zadání dotazu můžete odpovědět s daty](machine-learning-data-science-for-beginners-ask-a-question-you-can-answer-with-data.md) *(4 17 s min)*
  * Video 4: [Předpověď odpověď s jednoduchého modelu](machine-learning-data-science-for-beginners-predict-an-answer-with-a-simple-model.md) *(7 42 s min)*
  * Video 5: [Kopírování ostatních lidí práce pro výzkum dat](machine-learning-data-science-for-beginners-copy-other-peoples-work-to-do-data-science.md) *(3 18 s min)*

## <a name="transcript-is-your-data-ready-for-data-science"></a>Přepis: Hotovou dat pro výzkum dat?

Vítá vás "Je datům umístěným dat pro výzkum?" Druhá video v řadě *Pro výzkum dat pro začátečníky*.  

Než vědy data můžete zobrazit odpovědi, které chcete, musíte ji pojmenovat suroviny některé vysoce kvalitní pro práci s. Stejně jako vytváření pizza, tím lepší složek, kterou můžete začít, tím lepší výsledný dokument.

## <a name="criteria-for-data"></a>Kritéria pro data

Ano v případě vědy dat, jsou některé složek, které chceme seskupte.

Potřebujeme data, která jsou:

  * Důležité
  * Připojení
  * Přesné
  * Nestačí pro práci s

## <a name="is-your-data-relevant"></a>Je důležité datům?

Za první složka - potřebujeme data, která je důležité.

![Důležité údaje a důležitá data – vyhodnocení dat.](./media/machine-learning-data-science-for-beginners-is-your-data-ready-for-data-science/machine-learning-data-science-relevant-and-irrelevant-data.png)

Podívejte se tabulku na levé straně. Jsme došlo ke splnění sedm lidí mimo Boston pruhy, měřeno úroveň alkohol sledování krevního, průměr batting červené Sox v poslední hra a cena mléčné v úložišti nejbližší usnadnění.

Toto jsou všechny dokonale legitimní data. Pouze poruch je, že není důležité. Neexistuje žádný zjevných vztah mezi těchto čísel. Pokud se daly aktuální cenu mléčné a průměr batting Sox červené, nejde žádným způsobem může uhodnout Moje sledování krevního obsahu.

Teď podívejte se tabulku na pravé straně. Tentokrát jsme měří každé osoby základní hmotnost a počítá počet nápojů jste měli.  Čísla v každém řádku jsou teď důležité vzájemně. Pokud se zobrazila Moje textu hmotnost a počet jejích Margaritas jste měli byste mohli provést odhad na můj sledování krevního alkohol obsahu.

## <a name="do-you-have-connected-data"></a>Se připojujete data?

Další složky je připojená data.

![Připojení dat porovnání odpojeno data – kritéria dat data připravená](./media/machine-learning-data-science-for-beginners-is-your-data-ready-for-data-science/machine-learning-data-science-connected-vs-disconnected-data.png)

Tady je několik relevantních dat na kvalitu hamburgers: gril teploty, patty weight (váha) a hodnocení v místním jídla katalogu. Ale Všimněte si mezery v tabulce na levé straně.

Většina sady dat chybějí některé hodnoty. Způsoby informace k alternativním řešením je je běžné mít holes takto. Ale pokud je moc chybí, datům začne vypadat Švýcarsko sýr.

Když se podíváte na tabulku na levé straně, je tak mnohem chybějící data, je těžké si přichází s jakýmkoli vztah mezi mřížkou teplotní a patty weight (váha) Toto je příklad odpojeno data.

Tabulku na pravé straně však je už plná a vyplňte - příkladem připojená data.

## <a name="is-your-data-accurate"></a>Jsou vaše data přesné?

Další složky, které potřebujeme je přesnosti. Tady jsou čtyři cílů, které byste rádi za šipky.

![Přesná data porovnání nesprávná data - kritéria dat](./media/machine-learning-data-science-for-beginners-is-your-data-ready-for-data-science/machine-learning-data-science-inaccurate-vs-accurate-data.png)

Podívejte se na cíl v pravém horním rohu. Zde máme těsné seskupení vpravo kolem terč. Že je samozřejmě přesné. Oddly v jazyce dat pro výzkum naše výkonu na pravé straně cílové pod ním se taky považují za přesné.

Pokud by bylo potřeba namapovat na střed tyto šipky, uvidí, že jde o velmi zavřít terč. Šipky jsou rozložené všechny kolem cíl, se považuje za nepřesná, takže jste střed kolem terč, takže jsou považované za přesné.

Teď si prohlédněte cílové levou. Tady naše šipky přístupů velmi blízko u sebe, těsné seskupení. Budou přesné, ale jsou umístěné nesprávné, protože centra je způsob, jak vypnout terč. A samozřejmě na šipku v levé dolní cílovou nepřesných a nepřesná. Tento archer potřebuje více praktické cvičení.

## <a name="do-you-have-enough-data-to-work-with"></a>Máte dost k práci data?

Nakonec složka #4 - potřebujeme dost daty.

![Máte dost dat pro analýzu? Hodnocení dat](./media/machine-learning-data-science-for-beginners-is-your-data-ready-for-data-science/machine-learning-data-science-barely-enough-data.png)

Přemýšlejte všechny datové body v tabulce jako tahu štětce v malování. Pokud máte jenom některých z nich, Malování může být poměrně rozmazaný – je těžké rozlišit mezi funkcemi ho.

Pokud chcete přidat některé další tahy štětce, vaše prezentace s obrazem spustí získat trochu ostřejší.

Když budete mít stěží dost tahů, uvidíte právě tak, abyste obecných rozhodovat. To je někde, které možná budete chtít Navštěvujte blog o? Vypadá světlé, která vypadá vyčistit vodu – Ano, která je místo, kam budu na dovolenou.

Postupně přidávat další data, obrázek bude přehlednější a se můžete rozhodovat podrobnější. Teď můžu si může samostatně prohlížet tři hotely na levém banky. Víte, můžu skutečně, jako je architektonické funkce je v popředí. Můžu se pořád, na třetí prostorového uspořádání.

Data, která je důležité, připojení a přesná a dostatečně, můžeme mít všech složek potřebujeme proveďte některé vědy vysoce kvalitní data.

Ujistěte se, podívejte se na další čtyři videa ve *Vědy dat pro začátečníky* z webu Microsoft Azure počítače Learning.




## <a name="next-steps"></a>Další kroky

  * [Opakujte první pokusy vědy dat s počítače výukové Studio](machine-learning-create-experiment.md)
  * [Získat Úvod do počítače výukové na Microsoft Azure](machine-learning-what-is-machine-learning.md)
