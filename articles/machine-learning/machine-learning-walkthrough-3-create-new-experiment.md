<properties
    pageTitle="Krok 3: Vytvoření nového počítače výukové pokus | Microsoft Azure"
    description="Krok 3 / vývoje návod prediktivní řešení: Vytvořte nový experiment školení v Azure počítače výukové Studio."
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
    ms.date="10/05/2016" 
    ms.author="garye"/>


# <a name="walkthrough-step-3-create-a-new-azure-machine-learning-experiment"></a>Návod krok 3: Vytvoření nové experiment výukové počítače Azure

Toto je třetí krok návodu [vývoje řešení prediktivní analýzy Azure počítač přečíst](machine-learning-walkthrough-develop-predictive-solution.md)


1.  [Vytvoření pracovního prostoru aplikace Groove výukové počítače](machine-learning-walkthrough-1-create-ml-workspace.md)
2.  [Nahrát existující data](machine-learning-walkthrough-2-upload-data.md)
3.  **Vytvoření nového testu**
4.  [Školení a vyhodnocení modely](machine-learning-walkthrough-4-train-and-evaluate-models.md)
5.  [Nasazení webové služby](machine-learning-walkthrough-5-publish-web-service.md)
6.  [Přístup k webové službě](machine-learning-walkthrough-6-access-web-service.md)

----------

V tomto návodu dalším krokem je vytvoření pokus ve počítače výukové studiu, který používá datovou sadu, kterou jsme nahráli.  

1.  V Studio klikněte na **+ Nový** v dolní části okna.
2.  Vyberte **testu**a pak vyberte "Prázdné Experiment". Vyberte výchozí název experiment v horní části plátno a přejmenujte ho na určité akce

    > [AZURE.TIP] Je vhodné vyplňte **Souhrn** a **Popis** pro testu v podokně **Vlastnosti** . Tyto vlastnosti umožňují možnost dokumentu testu tak, aby každý, kdo ho později sleduje pochopí cíle a metodologii služby.

3.  Na paletě modul nalevo od testu plátno, rozbalte položku **Uložené datové sady**.
4.  Najděte datovou sadu, kterou jste vytvořili v části **Moje datové sady** a přetáhněte jej na plochu. Zadáním názvu do pole **Hledat** nad palety taky najdete datové sady.  

##<a name="prepare-the-data"></a>Příprava dat
Kliknutím na výstupní port datovou sadu (malé kruh dole) a výběrem **vizualizace**můžete zobrazit prvních 100 řádků dat a některé statistické informace pro celou sadu.  

Protože datový soubor neměli jsou součástí záhlaví sloupců, Studio dodala obecný nadpisy *(Sloupec1, Sloupec2 atd.)*. Dobrý nadpisy nejsou důležité pro vytvoření modelu, ale budou usnadňují práci s daty v testu. Také při jsme postupně publikovat tento model webové služby, nadpisy vám pomůže identifikovat sloupců tak, aby uživatel služby.  

Přičteme záhlaví sloupců pomocí [Upravit Metadata] [ edit-metadata] modul.
Použití [Upravit Metadata] [ edit-metadata] modulu, který chcete změnit přidružená datovou sadu metadata. V tomto případě může poskytovat další popisných názvů pro záhlaví sloupců. 

Můžete [Upravit Metadata][edit-metadata], nejprve určit sloupce, které chcete změnit (v tomto případě všem poznámkám.) Pak zadáte akci, kterou chcete provést na tyto sloupce (v tomto případě změny záhlaví sloupce.)

1.  Na paletě modulu zadejte "metadat" do **vyhledávacího** pole. Zobrazí se [Upravit Metadata] [ edit-metadata] zobrazí v seznamu modul.
2.  Kliknutím a přetažením [Upravit Metadata] [ edit-metadata] modul na plátno a umístěte ho pod datovou sadu jsme přidali výše.
3.  Datové připojení k [Upravit Metadata][edit-metadata]: klikněte na výstupní port datovou sadu (malé kruh v dolní části datové sady.) Pak tažením vstupní port [Upravit Metadata] [ edit-metadata] (malé kruh v horní části modulu), potom uvolněte tlačítko myši. Modul a datovou sadu zůstat připojení, přestože přesouváte buď na plátno.

    Testu nyní by měl vypadat nějak takto:  

    ![Přidání upravit Metadata][2]
    
    Červený vykřičník označuje, že jsme ještě nenastavili vlastností pro tento modul. Můžeme udělat tuto další.
    
    > [AZURE.TIP] Přidat komentář na modul poklikáním na modul a zadávání textu. Můžete prohlédnout modulu udělat v vaší testu. V tomto případě poklikejte na položku [Upravit Metadata] [ edit-metadata] modul a zadejte komentář "přidat záhlaví sloupců". Klikněte kamkoli jinam na plátno ukončíte textové pole. Pokud chcete zobrazit komentář, klikněte na šipku dolů na modul.

4.  Vyberte [Upravit Metadata][edit-metadata], klikněte v podokně **Vlastnosti** napravo od plátno na **spuštění volič sloupce**.
5.  V dialogovém okně **Výběr sloupců,** vyberte všechny řádky v seznamu **Dostupné sloupce** a klikněte na > přesunout do **Vybrané sloupce**.
Dialogové okno by měl vypadat takto: ![selektor sloupce s všech vybraných sloupců][4]
7.  Klikněte na **OK** zaškrtnutí.
8.  Po návratu do podokna **Vlastnosti** vyhledejte parametr **nové názvy sloupců** . V tomto poli zadejte seznam jmen 21 sloupců v sadě dat, oddělené čárkami a v pořadí sloupců. Názvy sloupců můžete získat z dokumentace datovou sadu na webu uc nebo pro usnadnění můžete zkopírovat a vložit v následujícím seznamu:  

          Status of checking account, Duration in months, Credit history, Purpose, Credit amount, Savings account/bond, Present employment since, Installment rate in percentage of disposable income, Personal status and sex, Other debtors, Present residence since, Property, Age in years, Other installment plans, Housing, Number of existing credits, Job, Number of people providing maintenance for, Telephone, Foreign worker, Credit risk  

    V podokně vlastnosti bude vypadat takhle:

    ![Vlastnosti pro úpravy Metadata][1]

> [AZURE.TIP] Pokud chcete ověřit záhlaví sloupců, spusťte testu (pod klikněte na **Spustit** plátno experiment). Po dokončení spuštění (zelené zaškrtnutí se zobrazí na [Upravit Metadata][edit-metadata]), klikněte na výstupní port [Upravit Metadata] [ edit-metadata] modul a vyberte **vizualizace**. Zobrazit výstup všech modul stejnou výzvu k zobrazení průběhu dat prostřednictvím testu.

##<a name="create-training-and-test-datasets"></a>Vytvoření školení a otestujte datové sady

Dalším krokem testu je generovat samostatné datové sady, které budeme používat u školení a testování našeho modelu.

K tomuto účelu [Rozdělenými daty] použijeme[ split] modul.  

1.  Hledání [Rozdělenými daty] [ split] modul, přetáhněte ho na plátno a připojte ji k poslední [Úpravy metadat] [ edit-metadata] modul.
2.  Ve výchozím nastavení poměr rozdělení je 0,5 a nastavit parametr **rozdělení Randomized** . To znamená, že náhodné polovinu dat výstupu přes jeden port [Rozdělenými daty] [ split] modul a polovinu prostřednictvím druhé. Můžete upravit tyto, stejně jako parametr **Počáteční hodnota náhodná** změnit rozdělení školení a testování data. V tomto příkladu jsme si necháte jako-je.
    
    > [AZURE.TIP] Vlastnost **zlomek řádků v první sadě dat výstupu** Určuje, kolik dat výstupu přes levé výstupní port. Například pokud nastavíte poměr 0,7, 70 % dat je výstupu přes levé port a 30 % prostřednictvím správné portu.  
    
3. Poklikejte na [Rozdělenými daty] [ split] modul a zadejte komentář, "školení/testování dat rozdělení 50 %". 

Používáme výstupy [Rozdělenými daty] [ split] modul však jsme líbí, ale Pojďme rozhodnete sdělit nám levé výstup jako dat školení a vpravo výstupní jako testování data.  

Jak je uvedeno na webu UC, náklady misclassifying rizika vysoké platební co nejnižší přesahuje pětkrát náklady misclassifying rizika zhoršeným platební jako Vysoká. Chcete-li počítat, jsme generovat nové datovou sadu, která odpovídá tuto funkci náklady. V nové sadě dat je příkladech vysokým rizikem replikovat pětkrát, když není replikovat příkladech nízké riziko.   

Tato replikace jsme můžete dělat pomocí R kódu:  

1.  Vyhledání a přetáhněte [Spustit skript R] [ execute-r-script] modul na testu plátno a připojte levé výstupní port [Rozdělenými daty] [ split] modul první vstupní port ("Dataset1") [Spustit skript R] [ execute-r-script] modul.
2. Poklikejte na položku [Spustit skript R] [ execute-r-script] modul a zadejte komentář, "Nastavení úprava nákladů".
2.  V podokně **Vlastnosti** odstranit výchozí text v parametru **Skript R** a zadejte tento skript:

          dataset1 <- maml.mapInputPort(1)
          data.set<-dataset1[dataset1[,21]==1,]
          pos<-dataset1[dataset1[,21]==2,]
          for (i in 1:5) data.set<-rbind(data.set,pos)
          maml.mapOutputPort("data.set")


Potřebujeme udělat tuto stejnou operaci replikace pro každý výstup [Rozdělenými daty] [ split] modul tak, aby data školení a testování obsahovat stejný úpravy náklady.

1.  Klikněte pravým tlačítkem myši [Spustit skript R] [ execute-r-script] modul a vyberte **Kopírovat**.
2.  Klikněte pravým tlačítkem myši na plochu pokus a vyberte **Vložit**.
3.  Připojení první vstupní port [Spustit skript R] [ execute-r-script] modul vpravo výstup port [Rozdělenými daty] [ split] modul. 
4.  V dolní části plátna klikněte na **Spustit**. 

> [AZURE.TIP] Kopie modulu spustit skript R obsahuje stejné skript jako modul původní. Když zkopírujete a vložíte modulu na plátno, výtisku zachová všechny vlastnosti původní.  

Náš experiment teď vypadá přibližně takto:

![Přidání modulu rozdělení a R skriptů][3]

Další informace o použití R skripty ve vaší pokusy najdete v článku [rozšířit experimentovat s R](machine-learning-extend-your-experiment-with-r.md).

**Dále: [vlaku a vyhodnotíte modely](machine-learning-walkthrough-4-train-and-evaluate-models.md)**


[1]: ./media/machine-learning-walkthrough-3-create-new-experiment/create1.png
[2]: ./media/machine-learning-walkthrough-3-create-new-experiment/create2.png
[3]: ./media/machine-learning-walkthrough-3-create-new-experiment/create3.png
[4]: ./media/machine-learning-walkthrough-3-create-new-experiment/columnselector.png


<!-- Module References -->
[execute-r-script]: https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/
[edit-metadata]: https://msdn.microsoft.com/library/azure/370b6676-c11c-486f-bf73-35349f842a66/
[split]: https://msdn.microsoft.com/library/azure/70530644-c97a-4ab6-85f7-88bf30a8be5f/
