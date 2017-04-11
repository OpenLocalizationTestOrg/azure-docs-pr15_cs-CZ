<properties
    pageTitle="Převedení experiment školení počítače výukové prediktivní pokus | Microsoft Azure"
    description="Jak převést počítače výukové školení testu, používá pro vzdělávání svůj prediktivní analýzy model a prediktivní testu, které můžou být nasazené jako webové služby."
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
    ms.author="garye"/>

# <a name="convert-a-machine-learning-training-experiment-to-a-predictive-experiment"></a>Převedení experiment školení počítače výukové prediktivní pokus

Azure výukové počítače umožňuje vytvářet a testování a nasazení řešení prediktivní analýzy.

Jakmile jste vytvořili a vstupní na *školení experimentovat* školení modelu prediktivní technologie pro analýzu a budete chtít použít k skóre nová data, budete muset připravit a zjednodušit experiment hodnocení. Tak, aby uživatelé mohli odeslat data do modelu a dostávat do modelu předpovědí nástroje můžete nasazovat potom tento *prediktivní experiment* jako Azure webové služby.

Když převedete na prediktivní pokus dostáváte školení modelu připravena k nasazení jako webové služby. Uživatelé služby web odešle zadávání dat do modelu a modelu odešle zpět výsledky prediktivní vkládání. Tak při převodu na prediktivní experiment můžete mít na paměti, jak očekáváte modelu pro použití v ostatním.

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

Proces převodu školení experiment prediktivní experiment zahrnuje tři kroky:

1.  Uložte model výukové počítače, který jste školení a nahradit počítače výukové algoritmus a [Vlaku modelu] [ train-model] moduly s uložené školení modelu.
2.  Střih pokus pouze moduly, které jsou potřebné pro hodnocení. Experiment školení obsahuje řadu moduly, které jsou potřebné pro vzdělávání ale nejsou potřebné po modelu školení a je připraven k použití pro hodnocení.
3.  Definování místo, kam se ve vaší experiment přijetí data z webové služby uživatele a jaká data budou vráceny.

## <a name="set-up-web-service-button"></a>Tlačítko Nastavení webové služby

Po spuštění experiment (tlačítko**Spustit** v dolní části plátno experiment) provede na tlačítko **Nastavení webové služby** (vyberte možnost **Prediktivní webová služba** ) pro vás tři kroky o převedení experiment školení na prediktivní experiment:

1.  Uloží školení modelu jako modulu v části **Školení modely** palety modul (nalevo od plátno experiment), pak nahradí počítače výukové algoritmus a [Vlaku modelu] [ train-model] moduly s uložené školení modelu.
2.  Odebere modulů kontroly, které nejsou jasně potřeba. V našem příkladu tato volba zahrnuje [Rozdělenými daty][split], 2 [Skóre modelu][score-model]a [Zjistit Model] [ evaluate-model] moduly.
3.  Vytvoří webové služby vstupní a výstupní moduly a přidá do výchozích umístění ve vaší testu.

Například následující testu soupravy modelu strom rozhodnutí zesílen dvě třídy pomocí ukázkových sčítání dat:

![Experiment školení][figure1]

Moduly uvedené v této experiment dělat v podstatě čtyři různé funkce:

![Funkce modulu][figure2]

Když převedete tento kurz experiment prediktivní testu, některé z těchto modulů už nepotřebujete nebo mají jiný účel:

- **Data** – data v této ukázkové datové sady není používaný během bodování: uživatel webová služba dodá data, která mají být. Metadata z této sady dat, například datové typy, ale používá školení model. Proto je potřeba mít datové na prediktivní testu tak, aby dát jí tak tento metadata.

- **Vývojové** – podle toho, data, která bude odeslána hodnocení, moduly může nebo nemusí být nutné zpracovat příchozí data.

    Například v tomto příkladu ukázkové datové pravděpodobně chybějících hodnot a obsahuje sloupce, které nejsou potřebné k školení modelu. Takže [Vyčistit chybějící Data] [ clean-missing-data] modul byl zahrnut do obchodu s chybějících hodnot a [Vybrat sloupce v sadě dat] [ select-columns] modul byl zahrnut z toku dat vyloučit tyto další sloupce. Pokud víte, že data, která bude odeslána hodnocení pomocí webové služby nebudou mít chybějících hodnot a pak můžete odebrat [Vyčistit chybějící Data] [ clean-missing-data] modul. Však od [Vybrat sloupce v sadě dat] [ select-columns] modul pomáhá definovat sadu funkcí právě dosáhne, modulu, musí zůstat.

- **Vlaku** – po modelu byla úspěšně školení, je uložit jako jediný model školení modulu. Můžete nahradit tyto jednotlivé moduly uložené školení modelu.

- **Skóre** – v tomto příkladu modulu rozdělit slouží k rozdělit sadu testovací data a dat školení datového proudu. V prediktivní pokusy není potřeba a je možné odebrat. Podobně 2 [Skóre modelu] [ score-model] modul a [Vyhodnocení modelu] [ evaluate-model] modul slouží k porovnání výsledky testovací data tak moduly nepotřebujete také v prediktivní testu. Zbývající [Skóre modelu] [ score-model] , ale není potřeba modul výsledek skóre pomocí webové služby.

Tady je naše vypadat třeba takhle po kliknutí na příkaz **Nastavení webové služby**:

![Převedeny experimentovat prediktivní][figure3]

To může být Příprava pokus o nasazení jako webové služby. Chcete však udělat některé další práce specifickou pro vašeho testu.

### <a name="adjust-input-and-output-modules"></a>Úprava vstupní a výstupní moduly

Ve vaší experiment školení používá množiny dat školení a nebyla nějaké zpracování přístup k datům ve formuláři potřeby algoritmu výukové počítače. Pokud data, která jste očekávat pomocí webové služby, nemusí tento zpracování, můžete přesunout **Vstupní modul webové služby** do různých uzel ve vaší testu.

**Nastavení webové služby** umístí ve výchozím nastavení modulu **webové služby vstupní** v horní části vaší toku dat, jako na obrázku nahoře. Ale pokud zadávání dat, nemusí tento zpracování, pak můžete ručně umístit **webové služby vstupní** dřívější moduly zpracování dat:

![Přesunutí vstupní webové služby][figure4]

Zadávání dat poskytovanou webová služba předá teď přímo do modulu skóre modelu bez jakékoli předzpracování.

Podobně ve výchozím **Nastavení webové služby** umístí modulu webových služeb výstup v dolní části toku dat. V tomto příkladu webová služba vrátí uživateli výstup [Skóre modelu] [ score-model] modul obsahující vector dokončení zadávání dat plus hodnocení výsledků.

Ale pokud chcete vrátit něco jiného – například, jenom hodnocení výsledků a ne celou vektorové zadávání dat – potom můžete vložit [Vybrat sloupce v sadě dat] [ select-columns] modul vyloučit všechny sloupce kromě hodnocení výsledků. Pak posuňte modulu **webové služby výstup** do výstupu [Vybrat sloupce v sadě dat] [ select-columns] modulu:

![Přesunutí výstupu webové služby][figure5]

### <a name="add-or-remove-additional-data-processing-modules"></a>Přidání nebo odebrání moduly pro další zpracování dat.

Pokud jsou vaše testu, o kterých víte, nesmí být potřeba během bodování další modulů kontroly, můžete odebrat tyto. Například, protože jsme přesunuli modulu **vstupní webové služby** na bod po moduly zpracování dat, jsme odebráním [Vyčistit chybějící Data] [ clean-missing-data] modul z prediktivní testu.

Náš prediktivní experiment teď vypadá takto:

![Odebrání další modul][figure6]

### <a name="add-optional-web-service-parameters"></a>Přidání volitelné parametry webové služby

V některých případech je vhodné umožnit uživatelům webová služba změnit chování moduly při přístupu k službě. *Parametry webové služby* vám umožní dělat toto.

Běžné příklad se nastavuje [Importovat Data] [ import-data] modul tak, aby uživatel nasazeném webová služba zadat jiný zdroj dat při přístupu k webové služby. Nebo konfigurace [Exportovat Data] [ export-data] modul tak, aby lze zadat jinou cílovou.

Můžete definovat parametry webové služby přidružit jeden nebo více parametrů modul, a můžete určit, zda jsou požadovány nebo volitelné. Uživatel webová služba potom můžete zadat hodnoty pro tyto parametry při přístupu k službě a akce modul bude upravena.

Další informace o parametry webové služby, najdete v článku [Použití parametry webové služby Azure počítače výukové ] [ webserviceparameters].

[webserviceparameters]: machine-learning-web-service-parameters.md


## <a name="deploy-the-predictive-experiment-as-a-web-service"></a>Nasazení prediktivní testu jako webové služby

Teď dostatečně připravený prediktivní testu, můžete ho nasadit jako Azure webové služby. Pomocí webové služby, uživatelé odesílat datového modelu a modelu vrátí jejich předpovědí.

Další informace o procesu dokončení nasazení najdete v článku [nasazení webové služby Azure počítače výukové][deploy]

[deploy]: machine-learning-publish-a-machine-learning-web-service.md


<!-- Images -->
[figure1]:./media/machine-learning-convert-training-experiment-to-scoring-experiment/figure1.png
[figure2]:./media/machine-learning-convert-training-experiment-to-scoring-experiment/figure2.png
[figure3]:./media/machine-learning-convert-training-experiment-to-scoring-experiment/figure3.png
[figure4]:./media/machine-learning-convert-training-experiment-to-scoring-experiment/figure4.png
[figure5]:./media/machine-learning-convert-training-experiment-to-scoring-experiment/figure5.png
[figure6]:./media/machine-learning-convert-training-experiment-to-scoring-experiment/figure6.png


<!-- Module References -->
[clean-missing-data]: https://msdn.microsoft.com/library/azure/d2c5ca2f-7323-41a3-9b7e-da917c99f0c4/
[evaluate-model]: https://msdn.microsoft.com/library/azure/927d65ac-3b50-4694-9903-20f6c1672089/
[select-columns]: https://msdn.microsoft.com/library/azure/1ec722fa-b623-4e26-a44e-a50c6d726223/
[import-data]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/
[score-model]: https://msdn.microsoft.com/library/azure/401b4f92-e724-4d5a-be81-d5b0ff9bdb33/
[split]: https://msdn.microsoft.com/library/azure/70530644-c97a-4ab6-85f7-88bf30a8be5f/
[train-model]: https://msdn.microsoft.com/library/azure/5cc7053e-aa30-450d-96c0-dae4be720977/
[export-data]: https://msdn.microsoft.com/library/azure/7a391181-b6a7-4ad4-b82d-e419c0d6522c/
