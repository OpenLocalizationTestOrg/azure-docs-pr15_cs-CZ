<properties 
    pageTitle="Co je Azure počítače výukové Studio? | Microsoft Azure"
    description="Základní informace o Azure ML Studio a přetažením nástroj pro rychlé vytváření modelů z knihovny připravené k použití algoritmů a moduly."
    keywords="Azure počítače výuka, azure ml ml studio"
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
    ms.topic="get-started-article"
    ms.date="09/09/2016"
    ms.author="garye"/>

# <a name="what-is-azure-machine-learning-studio"></a>Co je Azure počítače výukové Studio?

Microsoft Azure počítače výukové Studio je nástroj pro spolupráci, a přetažením, které můžete použít k vytváření, otestujte a nasazení řešení prediktivní technologie pro analýzu svých dat. Počítač výukové Studio publikuje modely jako webové služby, které můžete snadno spotřebované vlastních aplikací nebo nástroje BI třeba Excel.

Počítač výukové Studio je místo, kam dat pro výzkum, prediktivní analýzy, cloudové zdroje a vaše data splňují.

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <a name="the-machine-learning-studio-interactive-workspace"></a>Pracovní prostor interaktivní počítače výukové Studio

Se dají modelu prediktivní analýzy, můžete obvykle pomocí dat z jedné nebo více zdrojů, transformace a analýza dat pomocí různých manipulaci s daty statistických funkcí a a generovat sadu výsledků. Vývoj modelu takto je opakovaný proces. Při změnách různé funkce a jejich parametry výsledky konvergovat až budete spokojeni, že máte školení a efektivní modelu.

**Azure počítače výukové Studio** vám interaktivního, vizuální prostoru můžete snadno vytvářet, otestujte a zapracovávat ji do modelu prediktivní analýzy. Je a přetažením ***sady dat*** a analýza ***moduly*** na interaktivní plátno, jejich dohromady a ***experimentovat***, kterého lze spustit v počítači výukové Studio propojením. Zapracovávat ji do návrhu modelu že upravit testu, uložit kopii podle potřeby a znovu ho spusťte. Jakmile budete připraveni, můžete převést ***prediktivní experiment***vaší ***Experimentujte školení*** a pak publikovat jako ***webové služby*** tak, aby modelu můžete přistupovat ostatní uživatelé.

>[AZURE.TIP] Stáhnout a vytisknout diagram, který obsahuje přehled funkcí aplikace počítače výukové Studio, najdete v článku [Přehled některých dalších funkcí Azure počítače výukové Studio možnosti](machine-learning-studio-overview-diagram.md).

Existuje bez nutnosti programování, stačí vizuálně připojení modulů vytvářet modelu Prediktivní analýza a datové sady.

![Azure diagram ML Studio: vytvoření pokusy, číst data pro hodně zdrojů, zápis dosáhne dat, napište modely.][ml-studio-overview]

## <a name="get-started-with-machine-learning-studio"></a>Začínáme s počítače výukové Studio

Když zadáte [Studio výukové počítače](https://studio.azureml.net) vidíte **domovské** stránce. Tady můžete zobrazovat si přečtěte následující dokumentaci, videa, webináře a najít další cenný zdroje.

Existují tři karty v horní části: **Domů** (Pokud začnete) **Studio**a **Galerie**.

### <a name="studio"></a>Studio

Klikněte na kartu **Studio** a zobrazí se výzva, abyste přihlásit pomocí účtu Microsoft nebo pracovního nebo školního účtu. Když se přihlásíte, uvidíte na levé straně následujících kartách:

- **Projekty** – kolekce pokusy, datové sady, poznámkových bloků a další zdroje, které představuje jeden projekt
- **POKUSY** - pokusy, které byly vytvořené, spustit a uložit jako koncept
- **Webové služby** – webové služby, které jste nainstalovali z vaší pokusů
- **Poznámkové BLOKY** - Jupyter poznámkové bloky, které jste vytvořili
- **Datové sady** - datové sady, které jste nahráli do Studio
- **Školení modely** - modely, které jste školení pokusů a uložili v Studio
- **Nastavení** – kolekce nastavení, které můžete použít pro nastavení účtu a zdroje.

### <a name="gallery"></a>Galerie

Klikněte na kartu **Galerie** a jste se dostali do Galerie Intelligence Cortana. V galerii je místo, kde komunity vědeckých dat a vývojáři můžete sdílet řešení vytvořené pomocí komponent analytických nástrojů sady Cortana.

Další informace o galerii najdete v tématu [sdílet a seznamte se s řešení v galerii Intelligence Cortana](machine-learning-gallery-how-to-use-contribute-publish.md).

## <a name="components-of-an-experiment"></a>Součástí pokus

Pokus se skládá ze sady dat, která zdrojem dat analytical moduly, které propojení vytvářet modelu prediktivní analýzy. Konkrétně platné experiment má tyto vlastnosti:

- Testu obsahuje alespoň jednu datovou sadu a jeden modul
- Datové sady může být připojen pouze na moduly
- Moduly může být připojen k sady dat nebo jiných modulů
- Všechny vstupní porty modulů musí obsahovat některé připojení k toku dat
- Všechna požadovaná parametrů pro každý modul musí být nastavený

Pokus můžete vytvořit úplně od začátku nebo pokus existující ukázkové můžete použít jako šablonu. Další informace najdete v tématu [pokusy Ukázka použití k vytvoření nové pokusy](machine-learning-sample-experiments.md).

Příklad vytvoření simple pokusy najdete v článku [Vytvoření jednoduchého experiment v Azure počítače výukové Studio](machine-learning-create-experiment.md).

Podrobnější informace o vytváření řešení prediktivní analýzy najdete v článku [vývoje prediktivní řešení s Azure počítače učení](machine-learning-walkthrough-develop-predictive-solution.md).

### <a name="datasets"></a>Datové sady

Datovou sadu jsou data, která obsahuje nahraná na počítači výukové Studio tak, aby lze použít v procesu modelování. Řada ukázkové datové sady je obsažena v počítači výukové Studio můžete experimentovat s a další datové sady v případě potřeby můžete nahrát. Tady je pár příkladů však započítávány datové sady:

- **MPG data automobily různých** – km / galon (MPG) hodnoty automobily označenými číslem lahve, koňské síly atd.
- **Zadní rakoviny dat** – zadní rakoviny diagnostiky data.
- **Datové struktury aktivována** – doménové fire velikostí v severovýchodní oblasti Portugalsko.

Při vytváření pokus můžete ze seznamu dostupných datové sady nalevo od plátno.

Seznam ukázkové datové sady zahrnuty ve počítače výukové studiu najdete v tématu [použití výše uvedené množiny dat vzorku v Azure počítače výukové Studio](machine-learning-use-sample-datasets.md).

### <a name="modules"></a>Moduly

Modul je algoritmus, který můžete provádět s daty. Počítač výukové Studio má několik modulů od funkce průniku dat školení bodování a procesy ověření. Tady je pár příkladů však započítávány moduly:

- [Převod na ARFF] [ convert-to-arff] -převede datovou sadu .NET serializován do formátu souboru atribut vztah (ARFF).
- [Výpočet základní statistiky] [ elementary-statistics] -vypočítá základní statistiky například střední směrodatnou odchylku, atd.
- [Lineární regrese] [ linear-regression] -vytvoří model online přechodu na základě sestup lineární regrese.
- [Skóre modelu] [ score-model] -hodnocení školení klasifikace nebo regresní modelu.

Při vytváření pokus můžete ze seznamu dostupných modulů nalevo od plátno.  

Modul pravděpodobně sadu parametrů, které můžete použít pro nastavení modulu interní algoritmů. Po výběru modulu na plátno modulu parametry se zobrazí v podokně **Vlastnosti** napravo od plátno. Můžete změnit parametrů v dané podokno optimalizovat modelu.

Některé Nápověda k procházení velkých knihovny algoritmů výukové počítači k dispozici, najdete v článku [jak zvolte algoritmy pro Microsoft Azure počítače učení](machine-learning-algorithm-choice.md).

## <a name="deploying-a-predictive-analytics-web-service"></a>Nasazení prediktivní analýzy webové služby

Po prediktivní analýzy modelu je připravená, můžete ho nasadit jako webové služby přímo z počítače výukové Studio. Další informace o tomto postupu najdete v článku [nasazení webové služby Azure počítače výukové](machine-learning-publish-a-machine-learning-web-service.md).

[ml-studio-overview]:./media/machine-learning-what-is-ml-studio/azure-ml-studio-diagram.jpg

<!-- Module References -->
[convert-to-arff]: https://msdn.microsoft.com/library/azure/62d2cece-d832-4a7a-a0bd-f01f03af0960/
[elementary-statistics]: https://msdn.microsoft.com/library/azure/3086b8d4-c895-45ba-8aa9-34f0c944d4d3/
[linear-regression]: https://msdn.microsoft.com/library/azure/31960a6f-789b-4cf7-88d6-2e1152c0bd1a/
[score-model]: https://msdn.microsoft.com/library/azure/401b4f92-e724-4d5a-be81-d5b0ff9bdb33/
