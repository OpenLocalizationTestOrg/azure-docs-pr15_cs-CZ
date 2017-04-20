<properties
    pageTitle="Zvolte parametry Optimalizujte algoritmy v Azure Machine Learning | Microsoft Azure"
    description="Vysvětluje, jak zvolit optimální parametr nastavení algoritmu v Azure Machine Learning."
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


# <a name="choose-parameters-to-optimize-your-algorithms-in-azure-machine-learning"></a>Zvolte parametry Optimalizujte algoritmy v Azure Machine Learning

Toto téma popisuje, jak zvolit správné hyperparameter nastavení algoritmu v Azure Machine Learning. Většina algoritmů učení stroje mají parametry nastavení. K výuce modelu je nutné zadat hodnoty pro tyto parametry. Účinnost vyškolených modelu závisí na parametry modelu, které zvolíte. Proces hledání optimální sadu parametrů je znám jako *Výběr modelu*.

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

Výběr modelu různými způsoby. Ve strojovém učení křížového ověřování je jedním z nejrozšířenějších prostředků pro výběr modelu a je výchozím mechanismem výběru modelu v učení Azure Machine. Protože Azure Machine Learning podporuje R a Python, můžete implementovat vlastní mechanismy výběru modelu vždy pomocí R nebo Python.

Existují čtyři kroky hledání nejlepší sadu parametrů:

1.  **Definování parametru místa**: algoritmus, nejprve rozhodnout, přesný parametr hodnoty, které chcete vzít v úvahu.
2.  **Nastavení definovat křížového ověřování**: rozhodnout, jak zvolit přeloženími křížového ověřování pro datovou sadu.
3.  **Definování metriky**: rozhodnout, jaké metriky pro určení nejlepší sadu parametrů, jako je přesnost, spolehlivosti kořenové střední chyba, přesnost, odvolání nebo f skóre.
4.  **Vlak, vyhodnocení a porovnat**: pro každou jedinečnou kombinaci hodnot parametrů křížového ověřování prováděné a založené na definování metriky chyby. Po vyhodnocení a porovnání můžete nejlépe nejvýkonnější model.

Následující obrázek ukazuje, jak toho lze dosáhnout v Azure Machine Learning ukazuje.

![Najít nejlepší nastavení parametru](./media/machine-learning-algorithm-parameters-optimize/fig1.png)

## <a name="define-the-parameter-space"></a>Definování parametru místa
Můžete definovat parametr nastaven na krok inicializace modelu. Podokno parametrů všech algoritmů učení počítač má dva režimy trainer: *Jeden parametr* a *Rozsah parametru*. Zvolte režim rozsah parametru. V režimu rozsah parametru můžete zadat více hodnot pro každý parametr. Do textového pole můžete zadat hodnoty oddělené čárkami.

![Dvě třídy zesílen rozhodovacího stromu, jeden parametr](./media/machine-learning-algorithm-parameters-optimize/fig2.png)

 Alternativně můžete definovat maximální a minimální body mřížky a celkový počet bodů pomocí **Tvůrce rozsah použití**. Ve výchozím nastavení jsou hodnoty parametrů generována na lineární stupnici. Ale pokud je zaškrtnuto políčko **Logaritmické měřítko** , hodnoty jsou generovány v logaritmického měřítka (poměr přilehlé body je stálé místo jejich rozdíl). Pro parametry, celé číslo můžete definovat rozsah pomocí pomlčky. Například "1-10" znamená, že všechna celá čísla od 1 do 10 (oba včetně) tvoří sadu parametrů. Kombinovaný režim je také podporován. Například, nastavte parametr "1-10, 20, 50" by měla zahrnovat celá čísla 1-10, 20 a 50.

![Dvě třídy zesílen rozhodovací strom, rozsah parametru](./media/machine-learning-algorithm-parameters-optimize/fig3.png)

## <a name="define-cross-validation-folds"></a>Definovat přeloženími křížového ověřování
V [oddílu a ukázkové] [ partition-and-sample] modul lze použít přeložení náhodně přiřadit data. V následující ukázkové konfigurace modulu definovat pět přeloženími jsme náhodně přiřazení čísla skládání instance vzorku.

![Oddíl a vzorku](./media/machine-learning-algorithm-parameters-optimize/fig4.png)


## <a name="define-the-metric"></a>Definování metriky
[Optimalizace modelu Hyperparameters] [ tune-model-hyperparameters] modul poskytuje podporu pro empirically výběr nejlepší sadu parametrů pro daný algoritmus a dataset. Kromě dalších informací týkající se školení modelu, v podokně **Vlastnosti** tohoto modulu obsahuje metriku pro určení nejlepší sadu parametrů. To má dva různé rozevírací seznamy algoritmů klasifikace a regrese. Pokud uvažované algoritmus je algoritmus klasifikace, regresní metrika je ignorován a naopak. V tomto příkladu konkrétní metriky je **Přesnost**.   

![Shrnutí parametrů](./media/machine-learning-algorithm-parameters-optimize/fig5.png)

## <a name="train-evaluate-and-compare"></a>Vlak, vyhodnocení a porovnání  
Stejného [Ladění modelu Hyperparameters] [ tune-model-hyperparameters] modul soupravy všechny modely, které odpovídají nastavení parametru, vyhodnocuje různé metriky a potom vytvoří nejlépe vyškolených model založený na metriky, které zvolíte. Tento modul má dva povinné vstupy:

* Nezkušený student
* Objekt dataset

Volitelné dataset vstup má také modul. Připojení přeložená informace na vstup povinný dataset objektu dataset. Pokud objekt dataset není přiřazeno žádné informace o přeložení, pak 10-fold křížového ověřování automaticky proveden ve výchozím nastavení. Neprovádí přiřazení skládání a ověření dataset se v přístavu volitelný objekt dataset, je zvolen režim zkušebního vlaku a první sady dat se používá k vlaku model pro každou kombinaci parametrů.

![Zesílen rozhodnutí stromu třídění](./media/machine-learning-algorithm-parameters-optimize/fig6a.png)

Model je potom vyhodnocena sady dat, ověřování. Levé výstupní port modulu ukazuje různé metriky jako funkce hodnoty parametrů. Na pravém výstupní port dává vyškolených modelu, který odpovídá nejlépe nejvýkonnější model podle vybrané metriky (v tomto případě**Přesnost** ).  

![Ověření sady dat](./media/machine-learning-algorithm-parameters-optimize/fig6b.png)

Můžete zobrazit přesné parametry zvolené vizualizací na pravém výstupní port. Tento model lze použít v bodování testovací sadu nebo v operationalized webové služby po uložení jako vyškolených modelu.

<!-- Module References -->
[partition-and-sample]: https://msdn.microsoft.com/library/azure/a8726e34-1b3e-4515-b59a-3e4a475654b8/
[tune-model-hyperparameters]: https://msdn.microsoft.com/library/azure/038d91b6-c2f2-42a1-9215-1f2c20ed1b40/
