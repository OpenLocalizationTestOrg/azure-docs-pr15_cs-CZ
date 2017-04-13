<properties
   pageTitle="Ladění aplikace ve Visual Studiu | Microsoft Azure"
   description="Vylepšit spolehlivosti a výkonu služby vývoje a ladění ve Visual Studiu místní vývoj clusteru."
   services="service-fabric"
   documentationCenter=".net"
   authors="vturecek"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="06/21/2016"
   ms.author="vturecek;mikhegn"/>

# <a name="debug-your-service-fabric-application-by-using-visual-studio"></a>Ladění aplikace služby struktury pomocí aplikace Visual Studio

## <a name="debug-a-local-service-fabric-application"></a>Ladění místní aplikace služby struktury

Nasazení a ladění aplikace struktury služby Azure clusteru vývoj místního počítače můžete ušetřit čas a peníze. Visual Studio můžete nasadit aplikaci do místní obrázku a automaticky připojila ladění na všechny instance aplikace.

1. Spusťte clusteru místní rozvoj podle pokynů uvedených v části [Nastavení služby struktury vývojové prostředí](service-fabric-get-started.md).

2. Stisknutím klávesy **F5** nebo klikněte na **ladění** > **Spustit ladění**.

    ![Spuštění ladění aplikace][startdebugging]

3. Nastavit zarážky kód a pokaždé procházet všemi kroky aplikace kliknutím na příkazy v nabídce **ladění** .

    > [AZURE.NOTE] Připojí se k všechny instance aplikace Visual Studio. Když máte krokování kódu, mohou získat zarážky zasažení více procesů výsledkem současných relací. Zkuste zakázat zarážce poté, že máte, tím, že každé zarážky podmíněné ID vlákna nebo pomocí diagnostických událostí.

4. Okno **Diagnostických událostí** automaticky otevře, můžete zobrazit diagnostické události v reálném čase.

    ![Zobrazení diagnostických událostí v reálném čase][diagnosticevents]

5. Můžete také otevřete okno **Diagnostických událostí** v Průzkumníku cloudu.  V části **Služba struktury**klikněte pravým tlačítkem na libovolný uzel a zvolte **Zobrazení datových proudů trasování**.

    ![Otevřete okno diagnostických událostí][viewdiagnosticevents]

    Pokud chcete filtrovat zaznamenané trasování konkrétní služby nebo aplikace, jednoduše povolte streamování trasování v této konkrétní služby nebo aplikace.

6. Diagnostické událostí se zobrazí v automaticky generované **ServiceEventSource.cs** a nazývají z aplikace kódu.

    ```csharp
    ServiceEventSource.Current.ServiceMessage(this, "My ServiceMessage with a parameter {0}", result.Value.ToString());
    ```

7. Okno **Diagnostických událostí** podporuje filtry, pozastavení a kontrola událostí v reálném čase.  Filtr je jednoduchý řetězec hledání zprávy o události, včetně jeho obsah.

    ![Filtrování, pozastavení a obnovení nebo kontrola událostí v reálném čase][diagnosticeventsactions]

8. Ladění služby je jako ladění jakékoli jiné aplikace. Běžným způsobem nastavíte zarážky pomocí aplikace Visual Studio pro snadné ladění. Přestože spolehlivost kolekce replikace více uzlů, budou pořád Implementace IEnumerable. To znamená, že můžete zobrazit výsledky ve Visual Studiu při ladění najdete v článku jste uložili do. Jednoduše zarážku kdekoli v kódu.

    ![Spuštění ladění aplikace][breakpoint]

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->

## <a name="debug-a-remote-service-fabric-application"></a>Ladění aplikace vzdálené služby struktury

Pokud aplikace služby struktury jsou výpočetnímu clusteru struktury služby v Azure, budete moci vzdáleně ladění tyto prvky, které přímo z aplikace Visual Studio.

> [AZURE.NOTE] Tato funkce vyžaduje [služby struktury SDK 2.0](http://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=MicrosoftAzure-ServiceFabric-VS2015) a [Azure SDK.NET 2.9](https://azure.microsoft.com/downloads/).    

<!-- -->
> [AZURE.WARNING] Vzdálené ladění je určen pro vývojáře nebo zkoušení scénáře a se nemusí používat v provozním prostředí, protože jejich dopad na spuštěné aplikace.

1. Přejděte na svůj cluster v **Cloudu Průzkumník Windows**, klikněte pravým tlačítkem myši a zvolte **Povolit ladění**

    ![Povolení vzdáleného ladění][enableremotedebugging]

    To bude zahájení nepracovní proces povolení vzdáleného ladění rozšíření na vaše uzlech, jakož i konfiguraci požadované sítě.

2. Klikněte pravým tlačítkem myši obrázku v **Průzkumníku cloudu**a vyberte **Připojit ladění**

    ![Připojit ladění][attachdebugger]

3. V dialogovém okně **připojení zpracuje** zvolte procesu, který chcete ladění a klikněte na tlačítko **Připojit**

    ![Výběr obrázku][chooseprocess]

    Název procesu, který chcete připojit, je rovno na název svého služby project sestavení.

    Ladění se připojit k všechny uzly proces.
    - V případě, kde jsou ladění příslušnosti služby všechny instance služby ve všech uzlech jsou součástí relace ladění.
    - Pokud ladění stavová služba jenom primární otevřené oddíl budou aktivní a tedy odlovená podle něj. Pokud primární otevřené přesune během relace ladění, zpracování této otevřené nebude součástí relace ladění.
    - Chcete-li zachytit jenom relevantní oddíly nebo výskyty dané služby, můžete použít podmíněné zarážky pouze přerušit konkrétní oddíl nebo instance.

    ![Podmíněné zarážku][conditionalbreakpoint]

    > [AZURE.NOTE] Ladění služby struktury obrázku s několika instancích se stejným názvem spustitelný služby současnosti nepodporujeme.

4. Po dokončení ladění aplikace můžete zakázat vzdálené ladění rozšíření tak, že pravým tlačítkem myši na obrázku v **Průzkumníku cloudu** a zvolte **Zakázat ladění**

    ![Zakázání vzdálené ladění][disableremotedebugging]

## <a name="streaming-traces-from-a-remote-cluster-node"></a>Přenos trasování ze vzdáleného clusteru

Je také možné na toku stopy přímo ze vzdáleného clusteru uzel Visual Studio. Tato funkce umožňuje toku trasování událostí pro Windows trasování událostí vyrobeno na služby struktury clusteru, přímo ve Visual Studiu.

> [AZURE.NOTE] Tato funkce vyžaduje [služby struktury SDK 2.0](http://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=MicrosoftAzure-ServiceFabric-VS2015) a [Azure SDK.NET 2.9](https://azure.microsoft.com/downloads/).

<!-- -->
> [AZURE.WARNING]Přenos trasování je určen pro vývojáře nebo zkoušení scénáře a se nemusí používat v provozním prostředí, protože jejich dopad na spuštěné aplikace.
> Ve scénáři výrobní by měl spolehnout přesměrování události pomocí diagnostických nástrojů Azure.

1. Přejděte na svůj cluster v **Cloudu Explorer**, klikněte pravým tlačítkem myši a zvolte **Povolit streamování trasování**

    ![Povolení vzdáleného streamování trasování][enablestreamingtraces]

    To bude zahájení tak proces s povolením koncovku streamování trasování na vaše uzlech, jakož i konfiguraci požadované sítě.

2. Rozbalte položku element **uzlů** v **Cloudu Explorer**, klikněte pravým tlačítkem myši na uzel, kterou chcete vysílat od a vyberte **Zobrazení datových proudů trasování**

    ![Zobrazení vzdálené streamování trasování][viewremotestreamingtraces]

    Zopakujte krok 2 pro tolik uzlů hledáte od. Každý uzly toku se zobrazí v okně vyhrazené.

    Můžete se teď zobrazuje trasování, které služby struktury a služeb. Pokud chcete filtrovat události, aby se zobrazily pouze dané aplikace, stačí zadejte název aplikace v okně Filtr.

    ![Zobrazení datových proudů trasování][viewingstreamingtraces]

4. Jakmile dokončíte streamování trasování ze svého obrázku, můžete zakázat vzdálené streamování trasování, tak, že pravým tlačítkem myši na obrázku v **Průzkumníku cloudu** a vyberte **Zakázat streamování trasování**

    ![Zakázání vzdálené streamování trasování][disablestreamingtraces]

## <a name="next-steps"></a>Další kroky

- [Test služby struktury služby](service-fabric-testability-overview.md).
- [Správa aplikací služby struktury ve Visual Studiu](service-fabric-manage-application-in-visual-studio.md).

<!--Image references-->
[startdebugging]: ./media/service-fabric-debugging-your-application/startdebugging.png
[diagnosticevents]: ./media/service-fabric-debugging-your-application/diagnosticevents.png
[viewdiagnosticevents]: ./media/service-fabric-debugging-your-application/viewdiagnosticevents.png
[diagnosticeventsactions]: ./media/service-fabric-debugging-your-application/diagnosticeventsactions.png
[breakpoint]: ./media/service-fabric-debugging-your-application/breakpoint.png
[enableremotedebugging]: ./media/service-fabric-debugging-your-application/enableremotedebugging.png
[attachdebugger]: ./media/service-fabric-debugging-your-application/attachdebugger.png
[chooseprocess]: ./media/service-fabric-debugging-your-application/chooseprocess.png
[conditionalbreakpoint]: ./media/service-fabric-debugging-your-application/conditionalbreakpoint.png
[disableremotedebugging]: ./media/service-fabric-debugging-your-application/disableremotedebugging.png
[enablestreamingtraces]: ./media/service-fabric-debugging-your-application/enablestreamingtraces.png
[viewingstreamingtraces]: ./media/service-fabric-debugging-your-application/viewingstreamingtraces.png
[viewremotestreamingtraces]: ./media/service-fabric-debugging-your-application/viewremotestreamingtraces.png
[disablestreamingtraces]: ./media/service-fabric-debugging-your-application/disablestreamingtraces.png
