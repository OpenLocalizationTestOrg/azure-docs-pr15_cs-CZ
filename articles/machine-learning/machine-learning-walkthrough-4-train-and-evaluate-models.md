<properties
    pageTitle="Krok 4: Školení a vyhodnotíte prediktivní analytický modely | Microsoft Azure"
    description="Krok 4 vývoje prediktivní řešení návod: vlaku, skóre a vyhodnotíte více modelů v Azure počítače výukové Studio."
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
    ms.date="10/04/2016"
    ms.author="garye"/>


# <a name="walkthrough-step-4-train-and-evaluate-the-predictive-analytic-models"></a>Návod krok 4: Školení a vyhodnotíte prediktivní analytický modely

Toto téma obsahuje čtvrtým krokem, který návodu [vývoje řešení prediktivní analýzy Azure počítač přečíst](machine-learning-walkthrough-develop-predictive-solution.md)


1.  [Vytvoření pracovního prostoru aplikace Groove výukové počítače](machine-learning-walkthrough-1-create-ml-workspace.md)
2.  [Nahrát existující data](machine-learning-walkthrough-2-upload-data.md)
3.  [Vytvoření nového testu](machine-learning-walkthrough-3-create-new-experiment.md)
4.  **Školení a vyhodnocení modely**
5.  [Nasazení webové služby](machine-learning-walkthrough-5-publish-web-service.md)
6.  [Přístup k webové službě](machine-learning-walkthrough-6-access-web-service.md)

----------

Jednou z výhod používání Azure počítače výukové Studio pro vytváření modelů výukové počítače je možnost zkuste víc než jeden typ modelu současně v pokus a dále porovnají výsledky. Tento typ hodnocení vám pomůže najít nejlepším řešením pro váš problém.

V testu, kterou jsme sestavování v tomto návodu jsme vytvoříte dva různé typy modelů a porovnejte jejich hodnocení výsledků se rozhodnout, který chcete použít v naší konečný experiment algoritmus.  

Společnost Microsoft může určenou z různých modely. Modely k dispozici, zobrazíte rozbalte uzel **Výukové počítače** na paletě modul a potom rozbalte položku **Inicializace modelu** a uzly pod ní. Pro účely tohoto experiment jsme budete vyberte podpory vektorové počítače (SVM) a moduly stromové struktury rozhodování zesílen dvě předmětu.    

> [AZURE.TIP] Chcete-li pomoc s rozhodováním, který počítač výukové algoritmus nejlepší vyhovuje určitého problému se snažíte vyřešit, najdete v tématu [Zvolte algoritmy pro Microsoft Azure počítače Learning](machine-learning-algorithm-choice.md).

##<a name="train-the-models"></a>Školení modely
Nejdřív vytvoříme zesílen rozhodnutí strom modelu:  

1.  Najděte [Dva třídy zesílen rozhodovacího stromu] [ two-class-boosted-decision-tree] modul v modulu palety a přetáhněte ho na plátno.
2.  Najít [Vlaku modelu] [ train-model] modul, přetáhněte na plochu a připojte k levé vstupní port ("nevyškoleným model") [Vlaku modelu] výstup modulu zesílen rozhodovací strom[ train-model] modul.
    
    [Dvě třídy zesílen rozhodovacího stromu] [ two-class-boosted-decision-tree] modul inicializuje obecný model a [Školení modelu] [ train-model] používá dat školení k školení modelu. 
     
3.  Připojení levé výstupu ("výsledek datovou sadu") levého [Spustit skript R] [ execute-r-script] modul vpravo vstupní port ("datovou sadu") [Vlaku modelu] [ train-model] modul.

    > [AZURE.TIP] Jsme nemusí mít dvě vstupní hodnoty a jednoho z výstupů [Spustit skript R] [ execute-r-script] modul pro tento testu, proto jsme jejich ponechání nepřipojené. 

4.  Vyberte [Model vlaku] [ train-model] modul. V podokně **Vlastnosti** klikněte na **volič spuštění**, vyberte **Všechny typy** v rozevíracím seznamu dolů v seznamu **Dostupné sloupce** a zadejte "Kreditní rizika" do textového pole. **Všechny typy** vyberte v rozevíracím seznamu v části **Vybrané sloupce**. Vyberte "Kreditní rizika" a klikněte na tlačítko zvýrazněná šipka přejdete k **Vybrané sloupce**. 
5.  Klikněte na **Uložit**.


Tato část testu teď vypadá přibližně takto:  

![Školení modelu][1]

Dále jsme nastavit SVM model.  

Nejdřív uvedeme nevelká vysvětlení SVM. Stromové struktury rozhodování zesílen fungovat taky s funkcemi jakéhokoli typu. Protože modulu SVM vygeneruje lineární třídění, model, který generuje má však nejlepší Chyba testu všechny číselné funkce obsahujících stejné měřítko. Pokud chcete převést všechny číselné funkce stejné měřítko, používáme transformace "TGH" ( [Normalizace dat] [ normalize-data] modul.) To transformací naše čísla do oblasti [0,1]. Funkce String se převedou modulem SVM kategorií funkcí a potom na binární 0 a 1 funkce jsme nemuseli ručně transformace řetězec funkce. Navíc nechceme transformace riziko sloupci (sloupec 21) – je číselné, ale je hodnota jsme jste modelu odhadnout, školení, potřebujeme a zavřete okno samostatně.  

Pokud chcete nastavit SVM model, postupujte takto:

1.  Najděte [Dva třídy podpory vektorové počítače] [ two-class-support-vector-machine] modul v modulu palety a přetáhněte ho na plátno.
2.  Klikněte pravým tlačítkem myši na [Model vlaku] [ train-model] modul, zvolte **Kopírovat**, klikněte pravým tlačítkem myši na plochu a zvolte **Vložit**. Kopii [Vlaku modelu] [ train-model] modul má stejný výběr sloupců jako má původní.
3.  Připojte výstup modulu SVM k levé vstupní portu ("nevyškoleným model") druhý [Vlaku modelu] [ train-model] modul.
4.  Hledání [Normalizace dat] [ normalize-data] modul a přetáhněte ho na plátno.
5.  Připojení k levé výstupu levé [Spustit skript R] vstupní tento modul[ execute-r-script] modul (Všimněte si, že výstupní port modulu může být připojen k více než jednoho modulu).
6.  Připojení levé výstupní port ("transformovat datovou sadu") [Normalizace dat] [ normalize-data] modul vpravo vstupní port ("datovou sadu") druhý [Vlaku modelu] [ train-model] modul.
7.  V podokně **Vlastnosti** pro [Normalizace dat] [ normalize-data] modul, vyberte **TGH** pro parametr **transformace metody** .
8.  Klikněte na **spustit selektor sloupců**, vyberte "Žádné sloupce" pro **Začít s**, vyberte **Zahrnout** v prvním rozevíracím seznamu, v druhém rozevíracího seznamu vyberte **Typ sloupce** a vyberte v rozevíracím seznamu třetí **číselné** . Určuje Transformovat všechny číselné sloupce (a pouze číselné).
9.  Klepněte na znaménko plus (+) napravo od row: vytvoří řadu rozevírací seznamy. V prvním rozevíracím seznamu vyberte možnost **vyloučit** , výběr **názvů sloupců** ve druhém rozevírací seznam a klikněte na textové pole a vyberte "Kreditní rizika" ze seznamu sloupců. To určuje, že by měl ignorovány sloupci riziko (potřebujeme můžete to udělat, protože je v tomto sloupci číselné a proto by jinak Transformovat).
10. Klikněte na **OK**.  


[Normalizace dat] [ normalize-data] modul nastavená teď provádět transformace TGH všechny číselné sloupce s výjimkou sloupci platební rizika.  

Tato část naše experiment nyní by měl vypadat nějak takto:  

![Školení druhý model][2]  

##<a name="score-and-evaluate-the-models"></a>Počet bodů a vyhodnotíte modely

Používáme testování data, která byla oddělit [Rozdělenými daty] [ split] modul skóre naše školení modely. Společnost Microsoft pak můžete porovnat výsledky dva modely zobrazíte, které vygenerovalo lepších výsledků při.  

1.  Najít [Skóre modelu] [ score-model] modul a přetáhněte ho na plátno.
2.  Připojení [Vlaku modelu] [ train-model] moduly, které je připojen k [Dvě třídy zesílen rozhodovacího stromu] [ two-class-boosted-decision-tree] modul vlevo vstupní port [Skóre modelu] [ score-model] modul.
3.  Připojení správné vstupní port [Skóre modelu] [ score-model] modul levé výstupu správné [Spustit skript R] [ execute-r-script] modul.

    [Model skóre] [ score-model] modul můžete nyní se Dal informace z testování dat, spustit prostřednictvím modelu a porovnání předpovědí modelu vygeneruje se sloupcem rizika skutečné platební testování dat.

4.  Kopírování a vkládání [Skóre modelu] [ score-model] modulu, který chcete vytvořit kopii nebo nový modul přetáhněte na plochu.
5.  Připojení k modelu SVM levé vstupní port tento modul (to znamená připojení výstupní port [Vlaku modelu] [ train-model] moduly, které je připojen k [Dvě třídy podpory vektorové počítače] [ two-class-support-vector-machine] modul).
6.  Pro SVM model máme proveďte stejné transformace do testovací data podle kroků uvedených na školení data. Takže kopírování a vkládání [Normalizace dat] [ normalize-data] modulu, který chcete vytvořit kopii a připojte ji k levé výstupu správné [Spustit skript R] [ execute-r-script] modul.
7.  Připojení správné vstupní port [Skóre modelu] [ score-model] modul do výstupu levé [Normalizace dat] [ normalize-data] modul.  

Pokud chcete zjistit dvěma bodování výsledky, používáme [Vyhodnocení modelu] [ evaluate-model] modul.  

1.  Najít [Vyhodnocení modelu] [ evaluate-model] modul a přetáhněte ho na plátno.
2.  Připojení levé vstupní port výstupní port [Skóre modelu] [ score-model] modul přidružených k zesílen rozhodovací strom modelu.
3.  Spojte správné vstupní port jiný [Skóre modelu] [ score-model] modul.  

Testu spustíte kliknutím na tlačítko **Spustit** pod plátno. Může trvat několik minut. Indikátor zaneprázdnění v jednotlivých modulech zobrazí, ve kterých se nepoužívá, a pak zelená značka zaškrtnutí po skončení modulu. Všechny moduly si zaškrtnuté, v případě pokusu byl ukončen.

Testu nyní by měl vypadat nějak takto:  

![Vyhodnocení oba modely][3]

Zkontrolujte výsledky, klikněte na výstupní port [Vyhodnocení modelu] [ evaluate-model] modul a vyberte **vizualizace**.  

[Vyhodnocení modelu] [ evaluate-model] modul vytvoří dvojici křivky a metriky, které umožňují porovnají výsledky dva scored modely. Můžete zobrazit výsledky jako křivky sluchátko operátor vlastnostmi (ROC), přesnost/odvolání křivky nebo křivky výtah. Další data zobrazená obsahuje matici zmatky, souhrnné hodnoty pro oblasti ve skupinovém rámečku křivky (AUC) a jiné metriky. Můžete změnit mezní hodnota posunutím posuvníku doleva nebo doprava a zjistit, jak ovlivní sadu metriky.  

Napravo od grafu klikněte na **Scored datovou sadu** nebo **sadu Scored a umožňuje porovnávat** zvýrazněte přidružené křivky a zobrazíte přidružené metriky dole. V legendě pro křivky "Dosáhne datovou sadu" odpovídá levé vstupní port [Vyhodnocení modelu] [ evaluate-model] modul – v našem případě jedná se o zesílen rozhodovací strom model. "Dosáhne datovou sadu a umožňuje porovnávat" odpovídá správné vstupní port - modelu SVM v našem případě. Po kliknutí na jeden z těchto popisků křivky pro tento model zvýrazněná a zobrazte odpovídající metriky, jak je znázorněno na následujícím obrázku.  

![ROC křivky pro modely][4]

Porovnáním tyto hodnoty, můžete se rozhodnout, jaký model je nejblíž k pojmenování výsledky, které jste hledali. Můžete přejít zpět a zapracovávat ji do svého experiment změnou hodnoty v různých modelů. 

> [AZURE.TIP] Pokaždé, když spustíte testu záznam této iterace bude k dispozici v historii spustit. Můžete zobrazit tyto iterací a vraťte se do libovolné z nich, kliknutím na **Zobrazit HISTORII spustit** pod plátno. **Předchozí spustit** můžete taky kliknout v podokně **Vlastnosti** se vraťte do bezprostředně předchozí iterace jeden máte otevřený.
> 
Vytvořit kopii všechny iterace vaší experiment kliknutím na **Uložit jako** pod plátno. Pomocí tohoto testu **Shrnutí** a **Popis** vlastností zaznamenat co jste doposud vyzkoušeli ve vaší iteracích testu.

>  Další podrobnosti najdete v tématu [Správa experimentovat iterací v Azure počítače výukové Studio](machine-learning-manage-experiment-iterations.md).  


----------

**Dále: [nasazení webové služby](machine-learning-walkthrough-5-publish-web-service.md)**

[1]: ./media/machine-learning-walkthrough-4-train-and-evaluate-models/train1.png
[2]: ./media/machine-learning-walkthrough-4-train-and-evaluate-models/train2.png
[3]: ./media/machine-learning-walkthrough-4-train-and-evaluate-models/train3.png
[4]: ./media/machine-learning-walkthrough-4-train-and-evaluate-models/train4.png


<!-- Module References -->
[evaluate-model]: https://msdn.microsoft.com/library/azure/927d65ac-3b50-4694-9903-20f6c1672089/
[execute-r-script]: https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/
[normalize-data]: https://msdn.microsoft.com/library/azure/986df333-6748-4b85-923d-871df70d6aaf/
[score-model]: https://msdn.microsoft.com/library/azure/401b4f92-e724-4d5a-be81-d5b0ff9bdb33/
[train-model]: https://msdn.microsoft.com/library/azure/5cc7053e-aa30-450d-96c0-dae4be720977/
[two-class-boosted-decision-tree]: https://msdn.microsoft.com/library/azure/e3c522f8-53d9-4829-8ea4-5c6a6b75330c/
[two-class-support-vector-machine]: https://msdn.microsoft.com/library/azure/12d8479b-74b4-4e67-b8de-d32867380e20/
[split]: https://msdn.microsoft.com/library/azure/70530644-c97a-4ab6-85f7-88bf30a8be5f/
