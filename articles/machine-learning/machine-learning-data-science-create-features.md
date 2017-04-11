<properties
    pageTitle="Funkce inženýrské v procesu analýzy Cortana | Microsoft Azure" 
    description="Tento článek vysvětluje účely inženýrské funkce a jsou uvedeny příklady jeho role v procesu rozšíření dat učení počítače."
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


# <a name="feature-engineering-in-the-cortana-analytics-process"></a>Inženýrské funkce v procesu Cortana analýzy 

Toto téma vysvětluje účely inženýrské funkce a obsahuje příklady jeho role v procesu rozšíření dat učení počítače. Příklady použité ke znázornění tento proces jsou nakreslené z Azure počítače výukové Studio. 

[AZURE.INCLUDE [cap-create-features-data-selector](../../includes/cap-create-features-selector.md)]

Této **nabídce** odkazů na témata, která popisují, jak vytvořit funkcí pro data v různých prostředích. Tento úkol je kroku v [Týmu dat pro výzkum obrázku (TDSP)](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/).

Funkce technickým pokusy zvětšíte předpovídat výukové algoritmů vytvořením funkce z nezpracovaná data, která usnadňují procesem naučná. Inženýrské funkce a výběr funkcí je jedna část TDSP uvedené v [jak se postupuje týmu dat pro výzkum?](data-science-process-overview.md) Inženýrské funkce a výběr jsou části **vývoje funkce** krok TDSP. 

* **inženýrské funkce**: Tento proces pokusí vytvořit další relevantní funkce z existujících nezpracovanými funkcí v datech a zvýšit předpovídat algoritmu učení.

* **Výběr funkcí**: Tento proces vybere klíčové podmnožinu původní funkce dat při pokusu o zmenšení dimenze školení problém.

Obvykle **inženýrské funkce** platí nejprve získat další funkce a potom provedený krok **Výběr funkcí** eliminuje relevantní, nadbytečné nebo vysoce vzájemném vztahu funkce.

Školení data použitá v počítači výukové je možné vylepšovat často tak, že extrahování funkcí jako nezpracovaná dat shromážděných. Vytvoření trochu hustoty mapy vytvořen z data jako nezpracovaná bit distribuce je například analýzou funkce v souvislosti se naučíte klasifikovat obrázky ručně psané znaky. Mapa můžete vyhledání okraji znaky mnohem efektivněji než jednoduše přímo pomocí nezpracovanými rozdělení.


[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]


## <a name="creating-features-from-your-data---feature-engineering"></a>Vytváření funkce z dat – funkce inženýrské funkce

Školení data obsahují matice skládající se ze příklady (záznamů nebo poznámky, uložený v řádcích), z nichž každá obsahuje sadu funkcí (proměnné nebo pole uložené ve sloupcích). Očekává se, že funkce zadané v pokusné návrhu určení vzorky dat. Počet polí neformátovaná data přímo zahrnout sadu vybranou funkci použít ke školení modelu, je často v případě, že charakteristikami (analýzou), musí vytvořen funkcí v původní data, která generovat datovou sadu rozšířené školení.

Jaké funkce má vytvořen k vylepšení datové při školení modelu? Analýzou funkce, které vylepšují školení zadejte informace, lépe rozlišuje vzorky dat. Nové funkce, které poskytují další informace, které není jasně zachycený nebo snadno zřejmé, v sadě původní nebo existující funkce očekávat. Ale tento proces se hodí obrázek. Zvuk a produktivní rozhodnutí často vyžadují některá všechno pod kontrolou domény.

Když začnete s Azure počítače výuka, je nejjednodušší pochopit tento proces namítají pomocí vzorků uvedených v Studio. Dva příklady jsou uvedeny v tomto tématu:

* Příklad regresní [předpověď počet bike splátky](http://gallery.cortanaintelligence.com/Experiment/Regression-Demand-estimation-4) v kontrolované experiment kde je známo cílové hodnoty
* Příklad klasifikace dolování textu pomocí [Funkce algoritmus hash](https://msdn.microsoft.com/library/azure/c9a82660-2d9c-411d-8122-4d9e0b3ce92a/)

### <a name="example-1-adding-temporal-features-for-regression-model"></a>Příklad 1: Přidání časový funkcí pro regresní Model ###

Použijeme pokus "služba předpovídání jízdní kola" v Azure počítače výukové Studio ukázku funkcí pro daný úkol regresní analýzu. Cílem tento experiment je předpovídání služba za jízdní kola, to znamená počet bike splátky v rámci konkrétní měsíc/den/hodiny. Datové "Bike pronájem datovou sadu UC" slouží jako nezpracovaná vstupní data. Tento datovou sadu vychází z skutečné data z kapitálu Bikeshare společnost, která spravuje sítě pronájem bike WA Datacentrum v USA. Datové představuje počet bike splátky v rámci konkrétní hodinu dne v roce 2011 a roku 2012 a obsahuje 17379 řádků a sloupců 17. Sada funkcí jako nezpracovaná obsahuje počasí (rychlost teploty/vlhkost/větru) a typ den (dovolené/den v týdnu). Pole do předpovídání je "oznámení", počet znázorňující splátky bike v zadané hodiny a které oblasti rozmezí od 1 do 977.

S cílem konstrukce efektivní funkcí v datech školení, čtyři regresní, které jsou modely vytvořené pomocí stejného algoritmu, ale s čtyři různé školení datové sady. Čtyři datové sady představují stejný jako nezpracovaná zadávání dat, ale s rostoucí řadu funkcí, nastavit. Tyto funkce jsou seskupené do čtyř kategorií:

1. A = počasí svátků + den v týdnu + víkend funkcí pro předpovídané den
2. B = počet jízdní kola, které byly Propachtovaná v žádném z předchozích 12 hodin
3. C = počet jízdní kola, které byly Propachtovaná v žádném z předchozích 12 dnů v stejné hodinu
4. D = počet jízdní kola, které byly Propachtovaná v žádném z předchozí 12 týdnů na stejné hodiny a na stejný den

Kromě A sadu funkcí, které už v původní nezpracovanými daty, jsou tyto tři sady funkcí vytvořený prostřednictvím funkci konstrukce obrázku. Funkce nastavit zachycený obsah B velmi poslední služba kola. Funkce nastavit zachycený obsah C služba za jízdní kola v konkrétní hodinu. Funkce nastavení D zachycený obsah služba za jízdní kola na konkrétní hodina a na určitý den v týdnu. Čtyři školení datové sady každý obsahuje funkce sadě A, A + B, A + B + C a A + B + C + D.

V testu Azure počítače výukové jsou tyto čtyři datové sady školení vytvořený prostřednictvím čtyři větve z předem zpracovaných vstupní datovou sadu. Kromě vlevo, který obsahuje většina pobočky jednotlivých větví modul [spouštět R skript](https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/) , ve kterém odvozeno sadu funkcí (funkce nastavení B, C a D) jsou vytvořen a přidaným k importovaného datovou sadu. Následující obrázek znázorňuje R skript slouží k vytváření sadu funkcí B na druhé levého větve.

![Vytvoření funkce](./media/machine-learning-data-science-create-features/addFeature-Rscripts.png)

Porovnání výsledky čtyři modelů jsou shrnuté v následující tabulce. Nejlepších výsledků se zobrazí funkcemi A + B + C. Poznámka míru chyb snížení při další funkce jsou součástí školení data. Ověří, náš předpokládá, že sadu funkcí B, C zadejte další relevantní informace pro daný úkol regresní. Ale přidání funkci D nefunguje zadejte jakékoli další snížení sazeb chyby.

![výsledky porovnání](./media/machine-learning-data-science-create-features/result1.png)

### <a name="example2"></a>Příklad 2: Vytvoření funkcí v dolování textu  

Inženýrské funkce platí široce ve složce úkoly týkající se dolování text, například dokument klasifikace a myšlenkou analýzy. Třeba když chceme klasifikaci dokumentů do několika kategorií, typické předpokladů je, že slova nebo fráze zahrnuté v jedné kategorii dokumentu méně ke kterým může dojít v jiném dokumentu kategorie. V jiném slova je možné k určení jiným dokumentem kategorie četnost rozdělení slova nebo fráze. V aplikacích dolování text vzhledem k tomu jednotlivé použitelné části obsahu text obvykle slouží jako zadávání dat, funkci konstrukce proces flexibilní vytvořit funkce týkající se slovo nebo frázi frekvencí.

Dosáhnout tento úkol postup s názvem **algoritmus hash funkce** platí pro efektivní přeměna indexy funkce libovolný text. Místo přidružení každou funkcí text (slova nebo fráze) do určitého indexu této funkce metody použitím funkce hash funkcí a jejich hodnoty hash podle indexy přímo.

Azure počítač přečíst je [Algoritmus hash funkce](https://msdn.microsoft.com/library/azure/c9a82660-2d9c-411d-8122-4d9e0b3ce92a/) moduly, které vytvoří tyto slovo nebo frázi funkce pohodlně. Následující obrázek ukazuje příklad použití tento modul. Zadávání datovou sadu obsahuje dva sloupce: knihy hodnocení od 1 do 5 a obsahem skutečné revize. Cílem tento modul [Algoritmus hash funkce](https://msdn.microsoft.com/library/azure/c9a82660-2d9c-411d-8122-4d9e0b3ce92a/) je k načtení spoustu nových funkcí, které zobrazit četnost výskyt odpovídající slova / kontrola phrase(s) v rámci určitý sešit. Pokud chcete použít tento modul, potřebujeme proveďte následující kroky:

* Nejdřív vyberte sloupec, který obsahuje zadávání textu ("Sloupec2" v tomto příkladu).
* Za druhé hodnotu "Hashing bitsize" 8, což znamená, 2 ^ 8 = 256 funkce se vytvoří. Do 256 indexy použít algoritmus hash word/fáze všechen text. Parametr "Hashing bitsize" oblastí od 1 do 31. Slova / phrase(s) pravděpodobně nižší hash do stejné index, pokud nastavení, aby byl vyšší hodnoty.
* Třetí nastavte parametr "N-g" 2. To získá výskyt četnost unigrams (funkce pro každého jednoho slova) a bigrams (funkce pro každou dvojici sousedních slov) zadávání textu. Parametr "N-g" rozsah od 0 do 10, který určuje maximální počet sekvenční slova, která chcete zahrnout funkci.  

!["Funkce Hashing" modul](./media/machine-learning-data-science-create-features/feature-Hashing1.png)

Na následujícím obrázku vidíte, co tyto nové funkce vzhled jako.

![Příklad "Funkce Hashing"](./media/machine-learning-data-science-create-features/feature-Hashing2.png)


## <a name="conclusion"></a>Uzavření

Funkce analýzou a je zaškrtnuté zvýšení efektivity proces školení, které pokusí vyčíst klíčové informace obsažené v datech. Také zvyšují power tyto modely přesně klasifikovat vstupních dat a předpovídání výsledky potřebné více robustní. Inženýrské funkce a výběru můžete také sloučit aby výuky více nelze tractable. Stejně tak zvýšení a snížení řadu funkcí, třeba kalibrovat nebo školení modelu. Matematicky mluvit funkce vybrané ke školení modelu jsou minimální sady proměnných, které popisují vzorky dat a potom úspěšně předpovídání výsledky.

Všimněte si, že není vždy nemusí provádět výběr funkcí inženýrské funkce a funkce. Jestli je to potřeba nebo ne závisí na jsme mít nebo shromáždit data, algoritmu, kterou vybereme a cíle testu.
 
