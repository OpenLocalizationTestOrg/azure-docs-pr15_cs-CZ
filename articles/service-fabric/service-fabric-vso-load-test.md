<properties
    pageTitle="Načtení testovat aplikace Visual Studio týmu podnikové | Microsoft Azure"
    description="Naučte se zdůrazňují test aplikace struktury služby Azure pomocí aplikace Visual Studio týmovou."
    services="service-fabric"
    documentationCenter="na"
    authors="cawams"
    manager="timlt"
    editor="" />

<tags
    ms.service="multiple"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="multiple"
    ms.date="07/29/2016"
    ms.author="cawa" />

# <a name="load-test-your-application-by-using-visual-studio-team-services"></a>Načtení otestujte aplikaci služby Visual Studio týmu

Tento článek ukazuje, jak můžete zdůrazňují test pomocí Microsoft Visual Studio zatížení test funkcí aplikace. Použije struktury služby Azure stavová služba back-end a příslušnosti služby front-end webového. Příklad aplikace, tady je simulator umístění letadla ve. Zadáte letadla ve ID, odletu a cíl. Aplikace back-end zpracovává požadavky a zobrazí front-end na mapě letadla ve splňující kritéria.

Následující obrázek znázorňuje služby struktury aplikace, která se bude testování.

![Diagram aplikace příklad letadla ve umístění][0]

## <a name="prerequisites"></a>Zjistit předpoklady pro
Než začnete, je potřeba postupujte takto:

- Zřízení účtu Visual Studio týmovou. Můžete získat postupně zdarma [Visual Studio týmovou](https://www.visualstudio.com).
- Získání a instalace aplikace Visual Studio 2013 nebo Visual Studio 2015. Tento článek používá Visual Studio 2015 Enterprise edition, ale Visual Studio 2013 a dalších edice by měly fungovat podobně.
- Nasazení aplikace do pracovního prostředí. Postup [nasazení aplikace vzdálené clusteru pomocí aplikace Visual Studio](service-fabric-publish-app-remote-cluster.md) získat informace o tomto.
- Princip způsob použití aplikace. Tyto informace slouží tak, aby napodobily vzorek načíst.
- Princip cíl z hlediska zatížení testování. Pomůže vám to interpretovat a analýza výsledků testu načíst.

## <a name="create-and-run-the-web-performance-and-load-test-project"></a>Vytvoření a spuštění aplikace project Web výkon a načíst Test

### <a name="create-a-web-performance-and-load-test-project"></a>Vytvoření projektu Web výkon a načíst Test

1. Otevřete aplikaci Visual Studio 2015. Zvolte **soubor** > **Nový** > **projektu** na řádku nabídek pro otevření dialogového okna **Nový projekt** .

2. Rozbalte položku **Visual Basic** a vyberte **Test** > **Web výkon a načíst testovací projekt**. Pojmenujte projektu a potom klikněte na tlačítko **OK** .

    ![Snímek obrazovky s dialogovým oknem nového projektu][1]

    Měli byste vidět nový Web výkon a načíst Test projekt v okně Průzkumník.

    ![Snímek obrazovky Průzkumníka řešení zobrazující nového projektu][2]

### <a name="record-a-web-performance-test"></a>Záznam test výkonu web

1. Otevřete projekt .webtest.

2. Vyberte ikonu **Přidání záznamu** pro zahájení relace záznam v prohlížeči.

    ![Snímek obrazovky s ikonou přidat záznam v prohlížeči][3]

    ![Snímek obrazovky s tlačítkem záznam v prohlížeči][4]

3. Přejděte do aplikace služby struktury. Panel záznam byste měli vidět webové žádosti.

    ![Snímek obrazovky s žádostí o web na panelu záznam][5]

4. Proveďte řadu akcí, které se budou uživatelé provádět. Tyto akce se používají jako vzorek generovat načíst.

5. Až budete hotovi, klikněte na tlačítko **Zastavit** záznam.

    ![Snímek obrazovky s tlačítkem ukončit][6]

    .Webtest projektu ve Visual Studiu by měl zaznamenali řadu žádosti. Dynamické parametrů jsou automaticky nahradit. V tomto okamžiku můžete odstranit všechny navíc, opakované závislost požadavky, které nejsou součástí test nefunguje.

6. Uložte projekt a pak pomocí příkazu **Spusťte Test** testu web výkonu místně a ujistěte se, zda že vše funguje správně.

    ![Snímek obrazovky s příkazem spustit Test][7]

### <a name="parameterize-the-web-performance-test"></a>Parametrizovat test výkonu web

Test výkonu web můžete parametrizovat jeho převodu do test výkonu zakódovaný web a potom upravit kód. Jako alternativu můžete svážete test výkonu web seznamu dat tak, že test prochází data. Podrobnosti o tom, jak převést zkušební výkon webové zakódovaný testu naleznete v tématu [Generovat a spusťte test výkonu zakódovaný web](https://msdn.microsoft.com/library/ms182552.aspx) . Informace o tom, jak svázat test výkonu web dat najdete v článku [Přidání zdroje dat s výkonem web otestovat](https://msdn.microsoft.com/library/ms243142.aspx) .

V tomto příkladu jsme budete převést zkušební výkon webové zakódovaný test tak ID letadla ve nahraďte vygenerovaných GUID a přidat další žádosti o odeslání lety na různých místech.

### <a name="create-a-load-test-project"></a>Vytvoření projektu test zatížení

Načíst test projekt se skládá z jedné nebo více scénáře označená web zkouška a Jednotková testovací spolu s další zadaný zatížení test nastavení. Podle těchto kroků ukazují, jak vytvořit projekt test zatížení:

1. V místní nabídce Web výkon a načíst Test projektu, zvolte **Přidat** > **Test načíst**. V průvodci **Načíst testování** zvolte konfigurace test nastavení na tlačítko **Další** .

2. V části **Načíst** vyberte, jestli má zatížení konstantní uživatele nebo načtení krok, který spustí s několika uživateli a zvyšuje uživatelů v čase.

    Pokud máte dobré odhad počtu uživatelů zatížení a chcete se podívat, jak provede aktuální systém, zvolte **Konstantní načíst**. Pokud se dozvíte, zda systém provede konzistentní podle různých zatížení, je zvolte **Krok načíst**.

3. V části **Testování kombinace** klikněte na tlačítko **Přidat** a potom vyberte test, který chcete zahrnout ve sloupci Podmínka načíst. **Rozdělení** sloupce můžete použít k určení procenta z celkové testů paměti pro každý test.

4. V části **Nastavení spustit** zadejte dobu trvání test načíst.

    >[AZURE.NOTE] **Testování iterací** možnost je k dispozici pouze v případě, že spustíte test zatížení místně pomocí aplikace Visual Studio.

5. V části **umístění** možnosti **Spustit**zadejte na místo, kde se vytvářejí žádostí o zkušební načítání. Průvodce můžete být vyzváni k přihlášení k účtu služby týmu. Přihlaste se a pak zvolte zeměpisné polohy. Až budete hotovi, klikněte na tlačítko **Dokončit** .

6. Po vytvoření test zatížení otevření .loadtest projektu a zvolte aktuální směrový řetězec nastavení, například **Nastavení spuštění** > **Spustit Settings1 [aktivní]**. Nastavení spuštění se otevře v okně **Vlastnosti** .

7. V části **výsledky** okna **Spustit nastavení** vlastnosti nastavení **Časování podrobnosti úložiště** měli **žádná** jako výchozí hodnotu. Změňte tuto hodnotu na **Všech podrobných informací** chcete získat další informace o načíst výsledky testů. Další informace o tom, jak připojit k aplikaci Visual Studio týmovou a spusťte test zatížení najdete v článku [Načíst testování](https://www.visualstudio.com/load-testing.aspx) .

### <a name="run-the-load-test-by-using-visual-studio-team-services"></a>Spusťte test zatížení ve službách Visual Studio týmu

Výběr příkazu **Spusťte Test zatížení** spusťte test spustit.

![Snímek obrazovky s příkaz Spustit Test zatížení][8]

## <a name="view-and-analyze-the-load-test-results"></a>Zobrazení a analýza výsledků testu zatížení

Jako načíst testovat v průběhu vývoje, informace o výkonu jsou vynesena do grafu. Měli byste vidět podobnou následující graf.

![Snímek obrazovky s graf výkonu pro výsledky testů zatížení][9]

1. Zvolte odkaz **Stáhnout sestavu** v horní části stránky. Po stažení sestavy klikněte na tlačítko **Zobrazit zprávu** .

    Na kartě **graf** uvidíte grafy pro různé výkonnosti. Na kartě **souhrnné informace** se zobrazují celkové výsledky testů. Karta **tabulky** zobrazuje celkový počet testů předané a selhání načítání.

2. Po kliknutí na čísla odkazy na **testovací** > **Vadný** a **chyby** > **počet** sloupců tak, aby viděli podrobnosti o chybě.

    Kartu **Podrobnosti** zobrazuje virtuální uživatele a otestujte scénář informace o nezdařeném uložení žádosti. Tato data může být užitečné, pokud zkoušku zatížení obsahuje více scénáře.

Další informace o prohlížení výsledky testů zatížení najdete v článku [Analýza načíst testování výsledky v zobrazení grafy Analyzer načíst otestovat](https://www.visualstudio.com/load-testing.aspx) .

## <a name="automate-your-load-test"></a>Automatizace zatížení test

Visual Studio týmu služby načítání Test poskytuje rozhraní API, aby vám usnadní správu testů načítání a analýza výsledků týmovou účtu. Další informace najdete v tématu [Cloudu zatížení testování Rest API](http://blogs.msdn.com/b/visualstudioalm/archive/2014/11/03/cloud-load-testing-rest-apis-are-here.aspx) .

## <a name="next-steps"></a>Další kroky
- [Sledování a diagnostice služby v nastavení vývoj místního počítače](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md)

[0]: ./media/service-fabric-vso-load-test/OverviewDiagram.png
[1]: ./media/service-fabric-vso-load-test/NewProjectDialog.png
[2]: ./media/service-fabric-vso-load-test/Project.png
[3]: ./media/service-fabric-vso-load-test/AddRecording.png
[4]: ./media/service-fabric-vso-load-test/AddRecording2.png
[5]: ./media/service-fabric-vso-load-test/ActionSequence.png
[6]: ./media/service-fabric-vso-load-test/StopRecording.png
[7]: ./media/service-fabric-vso-load-test/RunTest.png
[8]: ./media/service-fabric-vso-load-test/RunTest2.png
[9]: ./media/service-fabric-vso-load-test/Graph.png
