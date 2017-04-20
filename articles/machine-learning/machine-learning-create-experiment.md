<properties
    pageTitle="Jednoduchý experiment v Machine Learning Studio | Microsoft Azure"
    description="Tento kurz machine learning vás provede pokus věda snadno data. Jsme budete odhadnout cenu auta pomocí regrese algoritmus."
    keywords="Experimentujte, lineární regrese, machine learning algoritmy, machine learning kurz, technik prediktivní modelování, data science experiment"
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
    ms.topic="hero-article"
    ms.date="07/14/2016"
    ms.author="garye"/>

# <a name="machine-learning-tutorial-create-your-first-data-science-experiment-in-azure-machine-learning-studio"></a>Machine learning kurz: vytvoření vaší první science experiment data v Studio učení Azure Machine

Tento kurz machine learning vás provede pokus věda snadno data. Vytvoříme lineární regresní model, který předpovídá cenu automobilu na základě různých proměnných, jako jsou například provést a technické specifikace. K tomu používáme Azure Machine Learning Studio při vývoji a iterovat na jednoduchý prediktivní, analytics experimentovat.

*Prognostické analýzy* je druh vědy dat, který používá aktuální data předpovědět budoucí výsledky. Velmi jednoduchý příklad prognostické analýzy, sledování dat vědy pro začátečníky 4 videa: [odpověď pomocí jednoduchého modelu předpovědět](machine-learning-data-science-for-beginners-predict-an-answer-with-a-simple-model.md) (runtime: 7:42).

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <a name="how-does-machine-learning-studio-help"></a>Jak pomáhá Machine Learning Studio?

Machine Learning Studio snadné nastavit pokus pomocí naprogramovaných s technikami prediktivní modelování a přetažení modulů. Spustit váš experiment a předvídat odpověď, použijete Machine Learning Studio k *Vytvoření modelu*, *model vlaku*a *skóre a Testovací model*.

Zadejte Machine Learning Studio: [https://studio.azureml.net](https://studio.azureml.net). Pokud jste přihlášeni Machine Learning Studio před, klepněte na odkaz **Přihlásit se zde**. V opačném případě klepněte na tlačítko **Sign Up** a zvolit možnosti bezplatných i placených.

Další obecné informace o Machine Learning Studio naleznete v tématu [Co je Machine Learning Studio?](machine-learning-what-is-ml-studio.md)

## <a name="five-steps-to-create-an-experiment"></a>Pět kroků k vytvoření experimentem

V tomto počítači studijní kurz provedením experimentem v počítači výukové Studio pro vytvoření, vlaku a skóre model vytvořit pět základních kroků:

- Vytvoření modelu
    - [Krok 1: Získání dat]
    - [Krok 2: Předběžné zpracování dat]
    - [Krok 3: Definování funkcí]
- Model vlaku
    - [Krok 4: Výběr a použití algoritmu učení]
- Bodové hodnocení a testování modelu
    - [Krok 5: Předpověď ceny nových automobilů]

[Krok 1: Získání dat]: #step-1-get-data
[Krok 2: Předběžné zpracování dat]: #step-2-preprocess-data
[Krok 3: Definování funkcí]: #step-3-define-features
[Krok 4: Výběr a použití algoritmu učení]: #step-4-choose-and-apply-a-learning-algorithm
[Krok 5: Předpověď ceny nových automobilů]: #step-5-predict-new-automobile-prices


## <a name="step-1-get-data"></a>Krok 1: Získání dat

Existuje několik ukázkových datových sad součástí Machine Learning Studio, které si můžete vybrat z, a můžete importovat data z mnoha zdrojů. V tomto příkladu použijeme ukázkovou datovou sadu **automobilu cenové údaje (Raw)**.
Tento objekt dataset obsahuje položky pro počet jednotlivých automobily, včetně informací, jako je například značka, model, technické specifikace a cenu.

1. Nový pokus spustit klepnutím na tlačítko **+ Nový** ve spodní části okna Machine Learning Studio, vyberte **EXPERIMENTOVAT**a potom vyberte **Prázdné experimentovat**. Vyberte výchozí název experimentu v horní části plátna a přejmenujte jej na určité akce, například **předpovědi ceny automobilu**.

2. Nalevo od experimentu plátno je paleta modulů a datových sad. Typ **automobilu** do vyhledávacího pole v horní části této palety najít objekt dataset označené **Auto cenové údaje (Raw)**.

    ![Hledat palety][screen1a]

3. Přetáhněte objekt dataset na plátno experimentu.

    ![Objekt DataSet][screen1]

Chcete-li zobrazit vzhled dat, klepněte na tlačítko výstupní port v dolní části automobilů dataset a vyberte **vizualizace**.

![Modul výstupního portu][screen1c]

Proměnné v objektu dataset se zobrazí jako sloupce, a každá instance automobilu, se zobrazí jako řádek. Ve sloupci zcela vpravo (sloupec 26 a s názvem "cena") je cílovou proměnnou, kterou přejdeme předpovědět.

![Vizualizace objektu DataSet][screen1b]

Klepnutím na tlačítko "**x**" v pravém horním rohu zavřete okno vizualizace.

## <a name="step-2-preprocess-data"></a>Krok 2: Předběžné zpracování dat

Objekt dataset obvykle vyžaduje některé úpravě před zpracováním před lze analyzovat. Možná jste si všimli chybějících hodnot ve sloupcích jednotlivých řádků. Tyto chybějící hodnoty je třeba vyčistit tak model správně analyzovat data. V našem případě jsme se odebrat všechny řádky, které mají chybějící hodnoty. Také obsahuje sloupec **normalizované ztráty** velkou část chybějící hodnoty, tak jsme se vyloučit sloupec zcela z modelu.

> [AZURE.TIP] Čištění chybějící hodnoty ze vstupních dat je předpokladem pro používání většiny modulů.

Nejprve budete odebrat sloupec **normalizované ztráty** a pak budete odeberte řádky chybějící data.

1. Zadejte do vyhledávacího pole v horní části modulu paleta [Vyberte sloupce v Dataset] **Vyberte sloupce** [ select-columns] modul a potom přetáhněte jeho experimentu plátno a připojení na výstupní port DataSet **data cena automobilu (Raw)** . Tento modul umožňuje vybrat sloupce dat, které chceme zahrnout nebo vyloučit v modelu.

2. Vyberte [Vybrat sloupce v Dataset] [ select-columns] modul a klepněte na tlačítko **spustit selektor sloupců** v podokně **Vlastnosti** .

    - Na levé straně klepněte na tlačítko **s pravidly**
    - **Začít s**klepněte **všechny sloupce**. To nařizuje [Vybrat sloupce v Dataset] [ select-columns] přes všechny sloupce (s výjimkou těch, které jsme se chystáte vyloučit).
    - Z rozevírací nabídky vyberte možnost **vyloučit** a **názvy sloupců**a potom klepněte dovnitř textového pole. Zobrazí se seznam sloupců. Vyberte **normalizované ztráty**a bude přidán do textového pole.
    - Klepněte na tlačítko zaškrtnutí (OK) zavřete voliče sloupců.

    ![Vybrat sloupce][screen3]

    V podokně vlastnosti **Vyberte** sloupce v Dataset označuje všechny sloupce se budou předány z objektu dataset, kromě **ztráty normalizované**.

    ![Vyberte sloupce v dialogovém okně Vlastnosti datové sady][screen4]

    > [AZURE.TIP] Poklepáním na modul a zadávání textu můžete přidat komentář k modulu. Můžete prohlédnout co dělá modul do vašeho experimentu. V tomto případě dvakrát klikněte na položku [Vybrat sloupce v Dataset] [ select-columns] modul a typ Poznámka "vyloučit normalizované ztráty."

3. Přetáhněte [Čisté chybějící Data] [ clean-missing-data] modul testu plátna a připojit jej k [Vybrat sloupce v Dataset] [ select-columns] modulu. V podokně **Vlastnosti** vyberte **Odstranit celý řádek** v **režimu čištění** čištění dat odebráním řádky, které jsou chybějící hodnoty. Poklepejte na modul a zadejte komentář "Odebrat chybějící hodnoty řádků."

    ![Vlastnosti čisté chybějící Data][screen4a]

4. Pokus spustíte klepnutím na tlačítko **Spustit** v části experimentu plátno.

Po dokončení testu všechny moduly mají zelená značka zaškrtnutí označuje, že dokončena úspěšně. Všimněte si také stav **dokončeno spuštění** v pravém horním rohu.

![První pokus spustit][screen5]

Všechny, které jsme učinili v experimentu do tohoto okamžiku je vyčistit data. Pokud chcete zobrazit vyčištěných dataset, klepněte na tlačítko levé výstupní port [Čisté chybějící Data] [ clean-missing-data] modul ("Cleaned dataset") a vyberte možnost **vizualizace**. Všimněte si, že **normalizované ztráty** sloupec je již zahrnuta, a neexistují žádné chybějící hodnoty.

Data je čistý, je připraven určete, jaké vlastnosti chceme použít prediktivní model.

## <a name="step-3-define-features"></a>Krok 3: Definování funkcí

*Funkce* v počítači učení, jsou jednotlivé měřitelné vlastnosti objektu, které vás zajímají. V našem objektu dataset každý řádek představuje jedno Auto a každý sloupec je součástí tohoto automobilu.

Hledání zboží vyžaduje sadu funkcí pro vytváření prediktivní model experimenty a znalosti o problému, který chcete vyřešit. Některé funkce jsou lepší pro odhad cílové než ostatní. Také některé funkce mají silnou korelaci s dalšími funkcemi (například město mpg versus komunikacích mpg), takže mnoho nových informací se nesmí přidat do modelu a mohou být odstraněny.

Pojďme vytvořit model, který používá podmnožinu funkcí v našem objektu dataset. Můžete vrátit a vybrat různé funkce, znovu spustit experimentu a naleznete, pokud můžete získat lepší výsledky. Ale pokud chcete spustit, zkuste následující funkce:

    make, body-style, wheel-base, engine-size, horsepower, peak-rpm, highway-mpg, price


1. Přetáhněte další [Vybrat sloupce v Dataset] [ select-columns] modul testu plátna a připojit jej k levé výstupní port [Čisté chybějící Data] [ clean-missing-data] modulu. Poklepejte na modul a zadejte "Select funkce předpověď."

2. V podokně **Vlastnosti** klepněte na tlačítko **spustit selektor sloupců** .

3. Klepněte **s pravidly**.

4. Ve skupinovém rámečku **Začít s** **žádné sloupce,**klepněte na tlačítko a pak vyberte **Zahrnout** a **názvy sloupců** v řádku filtru. Zadejte seznam náš názvy sloupců. To určí, že modul průchod pouze sloupce, které je určen.

    > [AZURE.TIP] Spuštěním testu jsme jste zajistit, že definice sloupců pro naše data procházejí z dataset [Čisté chybějící Data] [ clean-missing-data] modulu. To znamená, že informace z této sady dat má také jiné moduly, ke kterým se připojujete.

5. Klepněte na tlačítko zaškrtnutí (OK).

![Vybrat sloupce][screen6]

To vytváří objekt dataset, který bude použit v algoritmu učení v dalších krocích. Později můžete vrátit a znovu s výběrem různých funkcí.

## <a name="step-4-choose-and-apply-a-learning-algorithm"></a>Krok 4: Výběr a použití algoritmu učení

Nyní, že data jsou připravena, sestavením prediktivní model se skládá ze školení a testování. Použijeme naše data modelu a poté otestovat model, jak blízko je možné předvídat ceny. Nyní se nemusíte zabývat proč potřebujeme vlaku a následným spuštěním testu modelu.

*Klasifikace* a *regrese* jsou dva typy učení techniky provedení kontroly počítače. Klasifikace předpovídá na odpověď od sada kategorií, například barev (červené, modré nebo zelené). Regrese lze předpovědět číslo.

Protože chceme odhadnout cenu, což je číslo, použijeme regresní model. V tomto příkladu jsme se naučit jednoduché *lineární regrese* model a v dalším kroku, jsme bude testování.

1. Naše data používáme pro školení a testování rozdělení do samostatných školení a testování sad. Vyberte a přetáhněte [Rozdělit Data] [ split] modul testu plátno a připojení k výstupu poslední [Vybrat sloupce v Dataset] [ select-columns] modulu. Nastavit **řádky v objektu dataset první výstupní frakce** 0,75. Tímto způsobem můžeme budete školit modelu pomocí 75 procent dat a podržte zpět 25 procent pro testování.

    > [AZURE.TIP] Změnou parametru **náhodné osiva** mohou vytvářet různé náhodné vzorky pro školení a testování. Tento parametr řídí financování pseudonáhodná čísla generátor.

2. Spuštění testu. To umožňuje [Vybrat sloupce v Dataset] [ select-columns] a [Rozdělená Data] [ split] moduly předat moduly postupně sem budou přidávány další definice sloupců.  

3. Vyberte algoritmus učení, rozbalte kategorii **Výukové stroje** v paletě modulu vlevo na plátno a potom rozbalte položku **Inicializovat Model**. Zobrazí se několik kategorií modulů, které slouží k inicializaci machine learning algoritmy.

    Pro tento experiment vyberte [Lineární regrese] [ linear-regression] modulu v kategorii **regrese** (můžete také vyhledat modul zadáním "lineární regrese" do vyhledávacího pole paleta) a přetáhněte ji na plátně experimentu.

4. Vyhledejte a přetáhněte [Model vlaku] [ train-model] modul na plátno experimentu. Připojit k výstupu [Lineární regresi] levé vstupní port[ linear-regression] modulu. Připojit k výstupu dat školení (levé port) [Rozdělená Data] vpravo vstupní port[ split] modulu.

5. Vyberte [Model vlaku] [ train-model] modulu, klepněte na tlačítko **spustit selektor sloupců** v podokně **Vlastnosti** a pak vyberte sloupec **cena** . Jedná se o hodnotu, který bude našeho modelu předpovědět.

    ![Vyberte sloupec "cena"][screen7]

6. Spuštění testu.

Výsledkem je vyškolená regresního modelu, který lze použít k získání nové ukázky, chcete-li předpovědi.

![Použití algoritmu učení stroje][screen8]

## <a name="step-5-predict-new-automobile-prices"></a>Krok 5: Předpověď ceny nových automobilů

Teď jsme jste vyškolených modelu s použitím 75 procent našich dat, jsme slouží k skóre 25 procent dat. Chcete-li zjistit, jak dobře naše funkce modelu.

1. Vyhledejte a přetáhněte [Model skóre] [ score-model] modul testu plátna a levém vstupní port připojit výstup [Model vlaku] [ train-model] modulu. Spojte port vpravo vstupní test výstupu dat (port vpravo) [Rozdělená Data] [ split] modulu.  

    ![Výsledek modelu modulu][screen8a]

2. Pokus spustit a zobrazit výstup z [Modelu skóre] [ score-model] modulu, klepněte na tlačítko výstupní port a potom vyberte **vizualizace**. Výstup zobrazuje předpovídané hodnoty pro cenu a známé hodnoty z testovací data.  

3. Nakonec pokud chcete otestovat kvalitu výsledků, vyberte a přetáhněte [Model hodnocení] [ evaluate-model] modul testu plátno a připojení k výstupu [Modelu skóre] vlevo vstupní port[ score-model] modulu. (Protože jsou dva vstupní porty [Vyhodnotit Model] [ evaluate-model] modul lze použít k porovnání dvou modelů.)

4. Spuštění testu.

Chcete-li zobrazit výstup z [Modelu hodnocení] [ evaluate-model] modulu, klepněte na tlačítko výstupní port a potom vyberte **vizualizace**. Náš model se zobrazují následující statistické údaje:

- **Střední absolutní chyba** (Odpověď): průměrná absolutní chyby ( *chyby* je rozdíl mezi hodnotou předpovězené a skutečné hodnoty).
- **Kořenové střední kvadratických chyb** (RMSE): odmocninu průměrné kvadratických chyb předpovědi na testovací objekt dataset.
- **Absolutní relativní chyba**: průměrná absolutní chyby vzhledem k absolutní rozdíl mezi skutečnými hodnotami a průměr všech hodnot.
- **Relativní chyba ve čtverci**: průměrné kvadratických chyb vzhledem k druhá mocnina rozdílu mezi skutečnými hodnotami a průměr všech hodnot.
- **Koeficient určení**: také známý jako **hodnota spolehlivosti R**, je to statistický metriku označující, jak dobře modelu odpovídá datům.

Pro každou chybu statistiky, menší je lepší. Menší hodnota označuje, předpovědi lépe odpovídaly skutečné hodnoty. Pro **Koeficient určení**, čím blíže jeho hodnota je do jedné (1.0), tím lepší předpovědi.

![Výsledky hodnocení][screen9]

Poslední experiment by měl vypadat takto:

![Machine learning kurz: dokončení experimentu lineární regrese, který využívá technik prediktivní modelování.][screen10]

## <a name="next-steps"></a>Další kroky

Nyní, když jste dokončili první kurz machine learning a mít váš pokus nastavit, můžete iterovat a pokuste se zlepšit v modelu. Například můžete změnit funkce, které používáte ve vašem předpovědí. Nebo můžete upravit vlastnosti [Lineární regrese] [ linear-regression] algoritmus nebo zkuste zcela jiný algoritmus. Dokonce můžete přidat více počítači najednou studium algoritmů v experimentu a porovnat dva pomocí [Modelu hodnocení] [ evaluate-model] modulu.

> [AZURE.TIP] Pomocí tlačítka **Uložit jako** pod plátno experimentu všechny iterace v experimentu. Iterace v experimentu zobrazíte klepnutím na tlačítko **Zobrazit HISTORII spustit** pod plátno. Viz [Správa experimentovat iterací v Azure Machine Learning Studio] [ runhistory] další podrobnosti.

[runhistory]: machine-learning-manage-experiment-iterations.md

Pokud jste spokojeni s modelem, je možné nasadit jako webovou službu, která lze předpovědět pomocí nových dat, ceny automobilů. Viz [nasazení webové služby Azure Machine Learning] [ publish] další podrobnosti.

[publish]: machine-learning-publish-a-machine-learning-web-service.md

Další rozsáhlý a podrobný návod prediktivní modelování techniky vytváření školení, hodnocení a nasazení modelu, naleznete v [vývoj prediktivní řešení pomocí Azure Machine Learning][walkthrough].

[walkthrough]: machine-learning-walkthrough-develop-predictive-solution.md

<!-- Images -->
[screen1]:./media/machine-learning-create-experiment/screen1.png
[screen1a]:./media/machine-learning-create-experiment/screen1a.png
[screen1b]:./media/machine-learning-create-experiment/screen1b.png
[screen1c]: ./media/machine-learning-create-experiment/screen1c.png
[screen2]:./media/machine-learning-create-experiment/screen2.png
[screen3]:./media/machine-learning-create-experiment/screen3.png
[screen4]:./media/machine-learning-create-experiment/screen4.png
[screen4a]:./media/machine-learning-create-experiment/screen4a.png
[screen5]:./media/machine-learning-create-experiment/screen5.png
[screen6]:./media/machine-learning-create-experiment/screen6.png
[screen7]:./media/machine-learning-create-experiment/screen7.png
[screen8]:./media/machine-learning-create-experiment/screen8.png
[screen8a]:./media/machine-learning-create-experiment/screen8a.png
[screen9]:./media/machine-learning-create-experiment/screen9.png
[screen10]:./media/machine-learning-create-experiment/complete-linear-regression-experiment.png


<!-- Module References -->
[evaluate-model]: https://msdn.microsoft.com/library/azure/927d65ac-3b50-4694-9903-20f6c1672089/
[linear-regression]: https://msdn.microsoft.com/library/azure/31960a6f-789b-4cf7-88d6-2e1152c0bd1a/
[clean-missing-data]: https://msdn.microsoft.com/library/azure/d2c5ca2f-7323-41a3-9b7e-da917c99f0c4/
[select-columns]: https://msdn.microsoft.com/library/azure/1ec722fa-b623-4e26-a44e-a50c6d726223/
[score-model]: https://msdn.microsoft.com/library/azure/401b4f92-e724-4d5a-be81-d5b0ff9bdb33/
[split]: https://msdn.microsoft.com/library/azure/70530644-c97a-4ab6-85f7-88bf30a8be5f/
[train-model]: https://msdn.microsoft.com/library/azure/5cc7053e-aa30-450d-96c0-dae4be720977/
