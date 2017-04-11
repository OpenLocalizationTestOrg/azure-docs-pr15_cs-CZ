<properties
    pageTitle="Jak modelu počítače výukové přejde z pokus operationalized webové služby | Microsoft Azure"
    description="Základní informace o mechanismy z jak vaše Azure počítače výukové modelu v průběhu vývoje z vývojového experimentovat operationalized webové služby."
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


# <a name="how-a-machine-learning-model-progresses-from-an-experiment-to-an-operationalized-web-service"></a>Jak modelu počítače výukové přejde z pokus operationalized webové služby

Azure Studio výukové počítače poskytuje interaktivní plátnem, které umožňuje vývoj, spustit, otestujte a zapracovávat ji ***experimentovat*** představující modelu prediktivní analýzy. Existují širokou škálu moduly, které můžete:

* Zadávání dat do vašeho testu
* Pracovat s data
* Školení modelu pomocí počítače výukové algoritmů
* Skóre modelu
* Vyhodnocení výsledků
* Výstup výsledné hodnoty

Až budete spokojeni s vaší testu, můžete ho nasadit jako ***Klasická Azure počítače výukové webové služby*** nebo ***nové Azure počítače výukové webová služba*** tak, aby uživatelé mohli odesláním nová data a získat zpět výsledky.

V tomto článku nabízíme přehled mechanismy z jak váš počítač výukové modelu v průběhu vývoje z vývojového experimentovat operationalized webové služby.

>[AZURE.NOTE] Existují další způsoby vytvoření a nasazení modely výukové počítače, ale tento článek se zaměřuje na používání Studio výukové počítače. Informace o tom, jak vytvořit klasické prediktivní webové služby pomocí R, najdete v blogovém příspěvku [Tvůrce dotazů a nasazení prediktivní webové aplikace pomocí RStudio a Azure ML](http://blogs.technet.com/b/machinelearning/archive/2015/09/25/build-and-deploy-a-predictive-web-app-using-rstudio-and-azure-ml.aspx).

Během Azure počítače výukové Studio usnadňuje můžete vytvořit a nasazení *modelu prediktivní analýzy*, je možné použít Studio se dají pokus nezahrnuje modelu prediktivní analýzy. Například pokus může jenom pro zadávání dat, s ním pracovat a výstup výsledky. Stejně jako prediktivní analýzy experiment nasadíte tento-prediktivní experiment jako webové služby, ale je jednodušší proces, protože testu není školení nebo bodování modelu výukové počítače. I když není typické použití Studio tímto způsobem, jsme budete ji připojit v diskusi tak, aby bylo možné přidělit úplné vysvětlení fungování Studio.

## <a name="developing-and-deploying-a-predictive-web-service"></a>Vývoj a zavádění prediktivní webové služby

Tady jsou fáze, které typické řešení sleduje a vývoj nasazena pomocí počítače výukové Studio:

![Tok nasazení](media\machine-learning-model-progression-experiment-to-web-service\model-stages-from-experiment-to-web-service.png)

*Obrázek 1 – stadií modelu typické prediktivní analýzy*

### <a name="the-training-experiment"></a>Experiment školení

***Školení experimentovat*** je počáteční fáze vývoje webová služba ve počítače výukové studiu. Má experiment školení účel vám místo a vývoj otestovat, zapracovávat ji a nakonec školení počítače výukové modelu. Je můžete dokonce školení více modelů současně vyhledejte nejlepším řešením, ale až budete mít Hotovo experimentovat se budete vyberte typu single školení model a eliminujete tak zbytek z testu. Příklad vývoje prediktivní analýzy experimentovat najdete v tématu [vývoje prediktivní analýzy vyřešíte platební rizik Azure počítač přečíst](machine-learning-walkthrough-develop-predictive-solution.md).

### <a name="the-predictive-experiment"></a>Prognostické testu

Až budete mít modelu školení na školení testu, klikněte na **Nastavení webové služby** a vyberte **Prediktivní webová služba** ve počítače výukové studiu zahajte proces převodu školení experiment ***prediktivní testu***. Má prediktivní testu účel pomocí školení modelu skóre nová data, s cílem postupně stále operationalized jako Azure webové služby.

Tento převod probíhá za vás provede následujícími kroky:

-   Převedení sada modulů pro vzdělávání do jediného modulu a uložit ho jako modelu školení

-   Odstranit všechny nadbytečné moduly, které nesouvisí s bodování

-   Přidání vstupní a výstupní porty používající případné webové služby

Doména může obsahovat další změny chcete udělat připravit prediktivní experiment nasazení jako webové služby. Například pokud chcete webové služby pro ukládání výstupu jen podmnožinu výsledky, můžete přidat modul filtrování před výstupní port.

V tomto procesu převodu není zrušit školení testu. Po dokončení procesu, máte dvě karty v Studio: jedno pro experiment školení a jedno pro prediktivní testu. Tímto způsobem můžete provádět změny testu školení před nasazení webové služby a opětovné sestavení prediktivní testu. Nebo můžete uložit kopii experiment školení zahájíte další řádek hodnocení.

>[AZURE.NOTE] Když kliknete na **Prediktivní webová služba** spustit automatické proces převést školení experiment prediktivní pokus a to funguje dobře ve většině případů. Pokud školení pokus je složité (například máte více cest cvičení, ve kterém je spojení), můžete chtít udělat tento převod ručně. Další informace najdete v tématu [Převod počítače výukové školení, pokus prediktivní experiment](machine-learning-convert-training-experiment-to-scoring-experiment.md).

### <a name="the-web-service"></a>Webové služby

Až budete spokojeni, že prediktivní pokus je připravená, můžete nasadit služby jako buď klasické webová služba nebo nové webové služby založené na Azure správce. Pokud chcete umožňují modelu nasazením jako *Klasická počítače výukové webové služby*, klikněte na **Nasazení webové služby** a vyberte **Nasazení webová služba [klasické]**. Abyste mohli nasadit jako *Nový počítač výukové webové služby*, klikněte na **Nasazení webové služby** a vyberte **Nasazení [New] webové služby**. Uživatele můžete nyní odešlete datového modelu pomocí webová služba REST API a přijímat zpět výsledky. Další informace najdete v článku [jak používat Azure počítače výukové webové služby, které byly nasazeny z počítače výukové testu](machine-learning-consume-web-services.md).

## <a name="the-non-typical-case-creating-a-non-predictive-web-service"></a>Než typický případ: vytváření-prediktivní webové služby

Pokud vaše experiment není školení modelu Prediktivní analýza a potom není potřeba vytvářet experiment školení a bodování experiment – je jediným testu a můžete ho nasadit jako webové služby. Počítač výukové Studio zjistí, zda vaše experiment obsahuje prediktivní model analýzou modulů kontroly, které jste použili.

Poté, co jste vstupní o vaší pokusu a spokojeni s ním:

1.  Klikněte na **Nastavení webové služby** a vyberte **Přeškolení webové služby** – vstupní a výstupní uzly se automaticky přidají

2.  Klikněte na příkaz **Spustit**

3. Klikněte na **Nasazení webové služby** a vyberte **Nasazení webová služba [klasické]** **Nasazení [New] webové služby** v závislosti na prostředí, do kterého chcete nasadit.

Webová služba je teď nasazeném a můžete zpřístupnit a spravovat stejně jako prediktivní webové služby.

## <a name="updating-your-web-service"></a>Aktualizace webové služby

Teď jste nainstalovali experiment jako webové služby, co dělat, když potřebujete aktualizovat ho?

To záleží na co je potřeba aktualizovat:

**Chcete-li změnit vstupní a výstupní nebo chcete změnit, jak webová služba zpracuje data**

Pokud nejsou změn v modelu, ale jenom změnit zpracování data webové služby, můžete upravit prediktivní testu a potom klikněte na **Nasazení webové služby** a znovu vyberte **Nasazení webová služba [klasické]** nebo **Nasadit [New] webové služby** . Přerušili webové služby, aktualizované prediktivní testu nasazeném a restartování webové služby.

Tady je příklad: Předpokládejme prediktivní experiment vrátí změní celý řádek daného zadávání dat pomocí předpovídané výsledek. Můžete se rozhodnout, že chcete webové služby chcete pouze vrátit výsledek. Tak můžete přidat modul **Projektu sloupce** v prediktivní experiment přímo před výstupní port vyloučení sloupců než výsledek. Klikněte na **Nasazení webové služby** a znovu vyberete **Nasazení webová služba [klasické]** nebo **Nasadit [New] webové službě** , se aktualizuje webové služby.

**Chcete-li retrain modelu novými daty**

Pokud chcete zachovat počítači výukové model, ale chcete retrain novými daty, máte dvě možnosti:

1.  **Retrain modelu je spuštěná webové služby** – Pokud chcete retrain modelu při prediktivní se službou Web, můžete to udělat provedením několika úprav experiment školení snažíme usnadnit jeho ***rekvalifikaci testu***a pak můžete ho nasadit jako ** *rekvalifikaci webové* služby**. Další informace o tom, jak to udělat najdete v článku [Retrain výukové počítače modely programově](machine-learning-retrain-models-programmatically.md).

2.  **Přejděte zpátky na původní experiment školení a použijte jiný školení data se dají modelu** - prediktivní experiment je propojená s webové služby, ale experiment školení nesouvisející přímo tímto způsobem. Pokud změníte původní testu školení a klikněte na **Nastavení webové služby**, vytvoří *Nový* prediktivní experimentovat, po nasazení vytvoří *nové* webové služby. Jenom neaktualizuje původní webové služby.

    Pokud potřebujete změnit experiment školení, otevřete ho a klikněte na **Uložit jako** a zkopírujte. Tím ponechat beze změn původní experiment školení prediktivní testu a webové služby. Teď můžete vytvářet nové webové služby vašimi změnami. Jakmile jste nasazené nové webové služby se můžete rozhodnout, zda zastavit službu předchozí webu nebo v jednoduchosti je spuštěný společně se nový obrázek.

**Chcete-li školení modelu jiné**

Pokud chcete změny vaší původní prediktivní testu, například výběr algoritmu výukové jiného počítače vyzkoušení metoda různých školení, atd., a nepotřebujete sledovat druhého postupu popsaný nahoře pro přeškolení modelu: Otevřete experiment školení, klikněte na **Uložit jako** a vytvořte kopii a začněte dolů novou cestu vývoje modelu , vytváření prediktivní testu a nasazení webové služby. Tím vytvoříte nový Web služby nesouvisejících k původnímu – můžete se rozhodnout, který z nich nebo obojí můžete ponechat spuštěné.

## <a name="next-steps"></a>Další kroky

Podrobné informace o proces vývoje a pokus naleznete v následujících článcích:

-   Převod testu - [Převést počítači výukové školení pokus prediktivní pokus](machine-learning-convert-training-experiment-to-scoring-experiment.md)

-   nasazení webové služby – [nasazení webové služby Azure počítače výukové](machine-learning-publish-a-machine-learning-web-service.md)

-   přeškolení modelu - [Retrain výukové počítače modely programově](machine-learning-retrain-models-programmatically.md)

Příklady celého procesu najdete v článku:

-   [Počítač výukového kurzu: vytvoření první pokus v Azure počítače výukové Studio](machine-learning-create-experiment.md)

-   [Návod: Vyvíjet řešení prediktivní analýzy platební rizik Azure počítač přečíst](machine-learning-walkthrough-develop-predictive-solution.md)