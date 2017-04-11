<properties 
    pageTitle="Ladění modelu Azure počítač přečíst | Microsoft Azure" 
    description="Vysvětluje, jak ladění modelu Azure počítač přečíst." 
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
    ms.date="09/09/2016" 
    ms.author="bradsev;garye" />

# <a name="debug-your-model-in-azure-machine-learning"></a>Ladění modelu Azure počítač přečíst

Tento článek vysvětluje, jak ladění modely Microsoft Azure počítač přečíst. Konkrétně zahrnuje potenciální důvodů, proč některou z následujících dvou scénářích selhání může dojít při spuštění modelu:

* [Model vlaku] [ train-model] modul vyvolá chybu 
* [Model skóre] [ score-model] modul produkuje nesprávné výsledky 

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <a name="train-model-module-throws-an-error"></a>Modul modelu vlaku vyvolá chybu

![obrazek1](./media/machine-learning-debug-models/train_model-1.png)

[Model vlaku] [ train-model] modul očekává následující 2 vstupů:

1. Typ klasifikace/regresní Model z kolekce modely poskytovanou výukové počítače Azure
2. Školení dat pomocí zadaného popisek sloupce. Popisek sloupce Určuje proměnnou odhadnout. Zbytek sloupce zahrnuty automaticky za ně dosadí funkce.

Tento modul vyvolá chybu v těchto případech:

1. Popisek sloupce je nesprávně zadaný, protože víc než jednoho sloupce výběru jako popisek nebo nesprávné sloupec index. Druhý případ by například vztahovat v případě indexu sloupce 30 byl použit s vstupní datovou sadu, který obsahoval jenom 25 sloupce.

2. Datové neobsahuje žádné funkce sloupce. Například pokud vstupní datovou sadu má jenom 1 sloupec, který je označený jako popisek sloupce, bude žádné funkce, se kterým lze vytvořit modelu. V tomto případě [Vlaku modelu] [ train-model] modul vyvolá chybu.

3. Zadávání datovou sadu (funkce nebo popisek) obsahují nekonečnem jako by měla hodnotu.


## <a name="score-model-module-does-not-produce-correct-results"></a>Modul modelu skóre nejsou správné výsledky

![obrazek2](./media/machine-learning-debug-models/train_test-2.png)

Typické školení/testování graphu pro kontrolována výuka, [Rozdělenými daty] [ split] modul rozdělí původní datovou sadu na dvě části: část, která se používá k školení modelu a části, která je rezervován bodování chod školení modelu provádí dat ho není školení na. Školení modelu se pak používá ke skóre testovací data, po jejímž uplynutí budou výsledky vyhodnocovat k určení přesnost modelu.

[Model skóre] [ score-model] modul vyžaduje dva vstupy:

1. Školení modelu výstup z [Modelu vlaku] [ train-model] modul
2. Hodnocení datovou sadu není, který model nebyl školení na

Může se stát, že i v případě pokusu úspěšném [Skóre modelu] [ score-model] modul produkuje nesprávné výsledky. Několik příčin může způsobit, že to bylo možné:

1. Pokud je zadaný popisků kategorií a regresní model školení na datech, nesprávný výstup [Skóre modelu] [ score-model] modul. Je to proto regresní vyžaduje proměnnou nepřetržitý odpověď. V tomto případě by měl být vhodnější použít modelu klasifikace. 
2. Podobně pokud model klasifikace nastavena na datovou sadu s plovoucí desetinnou čárkou ve sloupci popisek, může mít nežádoucí výsledky. Je to proto klasifikace vyžaduje proměnnou samostatné odpověď, která umožňuje pouze hodnoty této oblasti konečných a obvykle něco malé sady tříd.
3. Pokud bodování datovou sadu neobsahuje všechny funkce verze použít ke školení model [Skóre modelu] [ score-model] způsobí chybu.
4. [Model skóre] [ score-model] by vytvořit žádný výstup odpovídající jeden řádek v bodování sady dat, která obsahuje na chybějící nebo nekonečné hodnotu pro jeho funkcí.
5. [Model skóre] [ score-model] může způsobit identickými výstupy pro všechny řádky ve bodování datovou sadu. Tato situace může nastat, například v části při pokusu o klasifikace pomocí rozhodnutí strukturami, pokud minimální počet vzorky na uzel listu je vybrán být vyšší než počet příklady školení k dispozici.


<!-- Module References -->
[score-model]: https://msdn.microsoft.com/library/azure/401b4f92-e724-4d5a-be81-d5b0ff9bdb33/
[split]: https://msdn.microsoft.com/library/azure/70530644-c97a-4ab6-85f7-88bf30a8be5f/
[train-model]: https://msdn.microsoft.com/library/azure/5cc7053e-aa30-450d-96c0-dae4be720977/
 
