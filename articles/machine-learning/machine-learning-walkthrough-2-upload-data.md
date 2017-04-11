<properties
    pageTitle="Krok 2: Odeslat data do počítače výukové experiment | Microsoft Azure"
    description="Krok 2 / vývoje prediktivní řešení návod: nahrát uložené ve veřejných datech do výukového Studio Azure počítače."
    services="machine-learning"
    documentationCenter=""
    authors="garyericson"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="machine-learning"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/16/2016" 
    ms.author="garye"/>


# <a name="walkthrough-step-2-upload-existing-data-into-an-azure-machine-learning-experiment"></a>Návod krok 2: Nahrajte existující data do pokus výukové počítače Azure

Toto je druhý krok tohoto návodu [vývoje řešení prediktivní analýzy Azure počítač přečíst](machine-learning-walkthrough-develop-predictive-solution.md)


1.  [Vytvoření pracovního prostoru aplikace Groove výukové počítače](machine-learning-walkthrough-1-create-ml-workspace.md)
2.  **Nahrát existující data**
3.  [Vytvoření nového testu](machine-learning-walkthrough-3-create-new-experiment.md)
4.  [Školení a vyhodnocení modely](machine-learning-walkthrough-4-train-and-evaluate-models.md)
5.  [Nasazení webové služby](machine-learning-walkthrough-5-publish-web-service.md)
6.  [Přístup k webové službě](machine-learning-walkthrough-6-access-web-service.md)

----------

Se dají prediktivní modelu doplňku pro platební rizika, potřebujeme dat, který používáme k školení a otestujte modelu. V tomto návodu používáme "UC Statlog (německé platební údaje) uvedenou množinu dat" z úložiště výukové počítače UC. Najdete ji zde:  
<a href="http://archive.ics.uci.edu/ml/datasets/Statlog+(German+Credit+Data)">http://archive.ICS.uci.edu/ml/datasets/Statlog+(German+Credit+data)</a>

Použijeme souboru nazvaného **german.data**. Stáhněte si tento soubor na místní pevný disk.  

Tento datovou sadu obsahuje řádky 20 proměnných 1000 minulých uchazečů o platební. Tyto 20 proměnné představují vektoru datovou sadu funkcí, který obsahuje identifikační vlastnosti pro každý platební uchazeče. Nějaký další sloupec pro každý řádek představuje uchazeče počítané platební riziko s 700 žadateli identifikované jako zhoršeným platební rizika a 300 jako vysokým rizikem.

Na webu UC obsahuje popis atributy vektorové funkce, které obsahují finančních informací, platební historii, stav a osobní údaje. Každý uchazeče binární hodnocení po dané označující, zda jsou nízké nebo vysoké kreditní rizika.  

Tyto údaje použijeme jak proškolit modelu prediktivní analýzy. Až jsme hotovi, našeho modelu by měla potvrďte informace pro nové uživatele a předpovídání, zda jsou nízké nebo vysoké platební rizika.  

Tady je jeden zajímavé zákrutu. Popis datové vysvětluje, misclassifying osoba je 5 číslem nákladnější finanční instituce než misclassifying rizika zhoršeným platební jako vysoké zhoršeným riziko okamžiku, kdy je skutečně rizika vysoké platební. Jednoduchý způsob vzít v úvahu v naší experiment je duplikování (5 číslem) ty položky, které představují někdo s vysokou platební rizika. Potom Pokud model misclassifies rizika vysoké platební co nejnižší, udělá této chybnou 5 číslem jednou pro každý duplikovat. Tím se zvýší náklady tato chyba ve výsledcích školení.  

##<a name="convert-the-dataset-format"></a>Převedení formátu datové sady
Původní datovou sadu používá formát oddělené prázdnou hodnotu. Počítač výukové Studio funguje lépe pomocí souboru s oddělovači (CSV), proto jsme budete převést datovou sadu nahrazením mezery čárkami.  

Chcete-li převést tato data mnoha různými způsoby. Pomocí následujícího příkazu prostředí Windows PowerShell je jedním ze způsobů:   

    cat german.data | %{$_ -replace " ",","} | sc german.csv  

Další možností je použití příkazu sed Unix:  

    sed 's/ /,/g' german.data > german.csv  

V obou případech jsme vytvořili hodnotami oddělenými čárkou verzi data do souboru nazvaného **german.csv** , které budeme používat v naší testu.

##<a name="upload-the-dataset-to-machine-learning-studio"></a>Nahrání datové do počítače výukové Studio

Po převedení dat ve formátu CSV potřebujeme uložit do počítače výukové Studio. Další informace o začínáte pracovat s Studio výukové počítače najdete v článku [Microsoft Azure počítače výukové Studio Domů](https://studio.azureml.net/).

1.  Otevřete počítače výukové Studio ([https://studio.azureml.net](https://studio.azureml.net)). Po zobrazení výzvy k přihlášení, použijte účet, který jste zadali jako vlastník pracovního prostoru.
1.  Klikněte na kartu **Studio** v horní části okna.
1.  Klikněte na **+ Nový** v dolní části okna.
1.  Vyberte **datovou sadu**.
1.  Vyberte **z místní soubor**.
1.  V dialogovém okně **Odeslat novou datovou sadu** klikněte na tlačítko **Procházet** a vyhledejte **german.csv** soubor, který jste vytvořili.
1.  Zadejte název pro datovou sadu. V tomto návodu jsme budete zavolejte ho "UC němčina platební kartou Data".
1.  Datový typ vyberte **Obecný soubor CSV s bez záhlaví (. nh.csv)**.
1.  Pokud chcete přidáte popis.
1.  Klikněte na **OK**.  

![Nahrání datové sady][1]  


To odešle data do datovou sadu moduly, které můžete použít v pokus.

> [AZURE.TIP] Ke správě datové sady, které jste nahráli do Studio, klikněte na kartu **datové sady** na levé straně okna Studio.

Další informace o importu různých typů dat do pokus najdete v článku [Import dat školení do výukového Studio Azure počítače](machine-learning-data-science-import-data.md).

**Dále: [Vytvoření nového testu](machine-learning-walkthrough-3-create-new-experiment.md)**

[1]: ./media/machine-learning-walkthrough-2-upload-data/upload1.png
