<properties
    pageTitle="Úkoly Příprava dat pro rozšířeného počítače výukové | Microsoft Azure"
    description="Předem procesu a vyčistit data a připravit ji výukové počítače."
    services="machine-learning"
    documentationCenter=""
    authors="bradsev"
    manager="jhubbard"
    editor="cgronlun" />

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/19/2016" 
    ms.author="bradsev" />


# <a name="tasks-to-prepare-data-for-enhanced-machine-learning"></a>Úkoly Příprava dat pro rozšířeného počítače výuka

Předzpracování a čištění dat jsou důležité úkoly, které obvykle se musí provést před datovou sadu lze efektivně výukové počítače. Neformátovaná data je často vysokou úroveň šumu a nedůvěryhodných a by mohla chybět hodnoty. Pomocí těchto dat pro modelování můžete výsledkům zavádějící. Tyto úkoly jsou součástí aplikace týmu dat pro výzkum obrázku (TDSP) a obvykle podle počáteční průzkum datovou sadu slouží k zjišťování a plánování předzpracování povinné. Podrobnější pokyny v procesu TDSP najdete v tématu kroků uvedených v [Týmu dat pro výzkum obrázku](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/).

Předzpracování a čištění úkoly, jako je úkol průzkum dat můžete provádět širokou škálu prostředí, například SQL nebo podregistru nebo Azure počítače výukové Studio a pomocí různých nástrojů a jazyce, například R nebo Python, v závislosti, kde je uložena data a jeho formátování. Protože TDSP je iterativních v přírodě, tyto úkoly může proběhnout na různé kroky v pracovním postupu.

Tento článek uvádí různé zpracování dat koncepty a úkoly, které lze provádět před nebo po ingesting dat do Azure počítače výukové.

Příklad průzkum a předzpracování Hotovo uvnitř Azure počítače výukové studio najdete v článku [předzpracování dat v Azure počítače výukové Studio](https://azure.microsoft.com/documentation/videos/preprocessing-data-in-azure-ml-studio/) video.


## <a name="why-pre-process-and-clean-data"></a>Proč předem procesu a vymazání data?

Reálné dat shromážděných z různých zdrojů a procesy a může obsahovat nesrovnalosti nebo poškozená data nebezpečné kvalitu datové sady. Problémy s kvalitou typické dat, které můžou nastat jsou:

* **Neúplné**: Data neobsahují atributy nebo obsahující chybějících hodnot.
* **Noisy**: Data obsahují chybné záznamů nebo odlehlé hodnoty.
* **Nekonzistentní**: Data obsahují konfliktní záznamů nebo rozdíly.

Zlepšení kvality dat je předpokladem prediktivní modely kvality. Vyhnout "uvolnění v uvolnění," a zlepšit kvalitu dat a tedy model výkonu, je nezbytné vedení obrazovky data stavu všimnete problémům s daty nejdříve možné a rozhodují o odpovídající zpracování dat a čištění kroky.

## <a name="what-are-some-typical-data-health-screens-that-are-employed"></a>Jaké jsou některé obrazovky typického datového stavu, které jsou použity?

Zaškrtnutím jsme můžete zkontrolovat obecné kvality dat:

* Počet **záznamů**.
* Počet **(nebo **funkce**)** .
* Atribut **datových typů** (nominal, pořadí nebo nepřetržitý).
* Počet **chybějících hodnot**.
* **A formedness** data.
    * Pokud data jsou ve TSV nebo souboru CSV, zkontrolujte, že oddělovače sloupec a řádek oddělovače vždy správně oddělit sloupců a řádků.
    * Pokud data jsou ve formátu HTML nebo XML, zkontrolujte, jestli data je správném založené na jejich příslušné úrovně.
    * Analýza možná by vás nutné extrahovat strukturované informace z částečně strukturovaná a Nestrukturovaná data.
* **Nekonzistentní záznamy**. Zaškrtněte políčko jsou povoleny v rozsahu hodnot. Například pokud data obsahují student GPA, zkontrolujte, zda je GPA v oblasti určené vyslovte 0 ~ 4.

Jakmile hledaný problémy s daty, **zpracování kroky** potřebné který zahrnuje často čištění chybějících hodnot normalizace dat, discretization, zpracování k odebrání nebo nahrazení vložené znaky, které by mohly ovlivnit zarovnání dat textu, smíšených datových typů společné a další pole.

**Výukové počítače azure spotřebovává ve správném formátu tabulková data**.  -Li data už ve formátu tabulky, před zpracování dat můžete provést přímo Azure počítače výukové ve počítače výukové studiu.  Nejsou-li data ve formátu tabulky, vyslovte, který je ve formátu XML, analýza může být potřeba k data převést do formátu tabulky.  

## <a name="what-are-some-of-the-major-tasks-in-data-pre-processing"></a>Jaké hlavní úkoly před zpracování dat?

* **Čištění dat**: Zadejte nebo chybějící hodnoty, vyhledání a odstranění vysokou úroveň šumu dat a odlehlé hodnoty.
* **Transformace**: normalizace dat zmenšit rozměry a šum.
* **Snížení dat**: Ukázka záznamy data nebo atributy pro snadnější zpracování dat.
* **Data discretization**: převést nepřetržitý atributy kategorií atributy pro snadnější použití s určité způsoby výukové počítače.
* **Text čištění**: odstranění vložený znaky, které tuto chybu může způsobovat chybné zarovnání dat, například vložené karty v odděleného tabulátory datového souboru, vložené nové řádky, které můžou přerušit záznamy, atd.

Následující části obsahují podrobné některé z těchto kroků zpracování dat.

## <a name="how-to-deal-with-missing-values"></a>Jak zacházet s chybějící hodnoty?

Řešení chybějících hodnot, je nejlepší nejprve určit důvod chybějících hodnot úchyt pro lepší problém. Typické chybějící hodnotu zpracování způsoby:

* **Odstranění**: odeberte záznamy s chybějících hodnot
* **Nahrazení figurínu**: nahrazení chybějících hodnot formální hodnotou: např, _Neznámý_ kategorií nebo 0 pro číselné hodnoty.
* **Nahrazení na mysli**: Pokud chybějící data není číselný, nahrazení chybějících hodnot střední hodnoty.
* **Časté nahrazení**: Pokud chybějící data kategorií, nahrazení chybějících hodnot nejčastěji položky
* **Nahrazení regresní**: použijte metodu regresní k nahrazení chybějících hodnot znovu zahrnut hodnotami.  

## <a name="how-to-normalize-data"></a>Jak se dá normalizace dat?

Normalizace dat znovu měřítko číselné hodnoty zadané oblasti. Metody normalizace Oblíbené dat patří:

* **Normalizace min a Max**: Lineárně transformace dat do oblasti, Řekněme mezi 0 a 1, kde zachován minimální hodnota 0 a maximální hodnotu 1.
* **Normalizace Z skóre**: změnit velikost dat na základě střední hodnota a směrodatná odchylka: dělení rozdíl mezi daty a střední směrodatnou odchylku.
* **Decimal měřítko**: měřítko data přesunutím desetinnou hodnotu atributu.  

## <a name="how-to-discretize-data"></a>Jak se dá discretize dat?

Data můžete discretized převodem nepřetržitý hodnoty nominal atributy a intervalů. Jsou některé způsoby, jak to provést:

* **Znaménko rovná šířkou Binning**: Rozdělit řadu všemi možnými hodnotami výrazu atribut do skupin N stejné velikosti a přiřazení hodnoty, které spadají do koše číslem Koš.
* **Výška rovná Binning**: Rozdělit řadu všemi možnými hodnotami výrazu atribut N skupiny každý obsahující stejný počet výskytů a potom přiřazení hodnoty, které spadají do koše číslem Koš.  

## <a name="how-to-reduce-data"></a>Jak zmenšit dat?

Zmenšete velikost dat pro snadnější data zpracování různými způsoby. V závislosti na velikosti dat a doménu se dají použít následujících metod:

* **Analytický nástroj vzorkování záznam**: Ukázka záznamů dat a pouze zástupce podmnožina vybírat data.
* **Analytický nástroj vzorkování atribut**: Vyberte jen podmnožinu nejdůležitější atributy data.  
* **Agregace**: data rozdělit do skupin a uložit čísla pro každou skupinu. Denní čísel výnosů řetězu restaurace za posledních let výrazné 20 můžete například agregované na měsíční příjmy zmenšit velikost data.  

## <a name="how-to-clean-text-data"></a>Jak si vyčistit textová data?

**Textová pole v tabulková data** může obsahovat znaky, které ovlivňují zarovnání a/nebo záznamu hranic sloupců. Například vložené tabulátory synchronizace odděleného tabulátory soubor příčina sloupce a vložené konce znaky nového řádku záznamu řádky. Kódování zpracování nesprávné text při psaní nebo čtení textu vede ke ztrátě informací, neúmyslné Úvod přečíst znaky, například hodnoty Null, a může také vliv text analýza. Pozor, analýza a úpravy může být potřeba k vyčistit textových polí pro správné zarovnání a/nebo chcete extrahovat strukturovanými data z textu nestrukturovaná nebo částečně strukturovaná data.

**Průzkum** nabízí nejdříve možné zobrazení do data. Počet dat problémů může nezakrytým během tohoto kroku a odpovídající metody se dají použít k řešení těchto problémů.  Je důležité pokládat otázky třeba co je zdroji problém a jak tento problém mohly být zahrnuty. Díky tomu také při rozhodování o krocích zpracování dat, které je potřeba vzít vyřešit. Typ přehledech, které jednu úmyslu jsou odvozeny od dat lze také nastavit jejich priority úsilí zpracování dat.

## <a name="references"></a>Odkazy

>*Dolování dat: koncepty a techniky*, třetí vydání, Nováková Kaufmann 2011 Jiawei Hanu Micheline Kamber a Jian Pei
