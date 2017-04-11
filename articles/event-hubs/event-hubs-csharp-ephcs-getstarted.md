<properties
    pageTitle="Začínáme s rozbočovače událostí v jazyce C# | Microsoft Azure"
    description="Postupujte podle kurzu, který začít používat Azure události rozbočovače s C# a používáním EventProcessorHost."
    services="event-hubs"
    documentationCenter=""
    authors="jtaubensee"
    manager="timlt"
    editor=""/>

<tags
    ms.service="event-hubs"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="09/02/2016"
    ms.author="jotaub;sethm"/>

# <a name="get-started-with-event-hubs"></a>Začínáme s rozbočovače události

[AZURE.INCLUDE [service-bus-selector-get-started](../../includes/service-bus-selector-get-started.md)]

## <a name="introduction"></a>Úvod

Událost rozbočovače je služba, zpracování velkých objemů dat události (telemetrie) z připojeného zařízení a aplikací. Po shromáždit data do události rozbočovače můžete ukládat data pomocí clusteru úložiště nebo transformaci pomocí zprostředkovatele v reálném čase analýzy. Tato možnost shromažďování a zpracování velké události je hlavní součástí moderní aplikace architektury včetně Internet věcí (IoT).

Tento kurz ukazuje, jak používat portál Azure klasické vytvořit rozbočovači události. Je také se dozvíte, jak shromažďovat zprávy do události rozbočovače použitím aplikace konzoly vytvořené v jazyce C# a jak načíst paralelní pomocí C# [Události procesor hostitele][] knihovna.

Tento kurz, musíte mít takto:

+ [Microsoft Visual Studio](http://visualstudio.com)

+ Účet Azure active. Pokud nemáte, můžete vytvořit bezplatný účet v jenom pár minut. Podrobnosti najdete v tématu [Bezplatnou zkušební verzi Azure](https://azure.microsoft.com/free/).

[AZURE.INCLUDE [event-hubs-create-event-hub](../../includes/event-hubs-create-event-hub.md)]

[AZURE.INCLUDE [service-bus-event-hubs-get-started-send-csharp](../../includes/service-bus-event-hubs-get-started-send-csharp.md)]

[AZURE.INCLUDE [service-bus-event-hubs-get-started-receive-ephcs](../../includes/service-bus-event-hubs-get-started-receive-ephcs.md)]

## <a name="run-the-applications"></a>Spuštění aplikace

Teď jste připraveni ke spuštění aplikace.

1. Z aplikace Visual Studio otevřete **sluchátko** projektu, který jste vytvořili.

2. Klikněte pravým tlačítkem myši na řešení **sluchátko** a pak klikněte na **Přidat**a pak klikněte na **Existující projekt**.
 
3. Vyhledejte soubor existující Sender.csproj a potom přidáte do řešení dvojím kliknutím.
 
4. Znovu klikněte pravým tlačítkem myši na řešení **příjemce** a potom klikněte na **Vlastnosti**. Zobrazí se stránka vlastností **příjemce** .

5. Klikněte na **Projekt při spuštění**a potom klikněte na tlačítko **více projektů po spuštění** . Nastavte **Akce** u **sluchátko** a **odesílatele** projektů na **Spustit**.

    ![][19]

6. Klikněte na tlačítko **závislosti projektů**. V dialogovém okně **projekty** klikněte na příkaz **odesílatele**. V dialogovém okně **na** zkontrolujte, že je zaškrtnuté políčko **příjemci** .

    ![][20]

7. Klikněte na **OK** zavřete dialogové okno **Vlastnosti** .

1.  Stisknutím klávesy F5 spustit **sluchátko** projektu v aplikaci Visual Studiu a počkejte, aby se spouštěla příjemce pro všechny oddíly.

    ![][21]

2.  **Odesílatel** projektu se spustí automaticky. Stisknutím klávesy **Enter** v okně konzoly a zobrazit události, které se zobrazí v okně příjemce.

    ![][22]

Stisknutím **Kombinace kláves Ctrl + C** v okně **odesílatele** ukončení aplikace odesílatele a stiskněte klávesu **Enter** v okně sluchátko vypnout tuto aplikaci.

## <a name="next-steps"></a>Další kroky

Teď nevytvoříte funkční aplikaci, která vytvoří rozbočovači událostí a umožňuje volání a přijímání dat, můžete na přesouvat v následujících případech:

- Kompletní [Ukázková aplikace, který využívá rozbočovače události][].
- Ukázka [měřítko, událostí zpracování s rozbočovače události][] .
- [Přehled rozbočovače událostí][]

<!-- Images. -->
[19]: ./media/event-hubs-csharp-ephcs-getstarted/create-eh-proj1.png
[20]: ./media/event-hubs-csharp-ephcs-getstarted/create-eh-proj2.png
[21]: ./media/event-hubs-csharp-ephcs-getstarted/run-csharp-ephcs1.png
[22]: ./media/event-hubs-csharp-ephcs-getstarted/run-csharp-ephcs2.png

<!-- Links -->
[Azure classic portal]: https://manage.windowsazure.com/
[Host (hostitel) procesor události]: https://www.nuget.org/packages/Microsoft.Azure.ServiceBus.EventProcessorHost
[Přehled rozbočovače událostí]: event-hubs-overview.md
[Ukázka aplikace, která používá rozbočovače události]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-286fd097
[Rozšiřování události zpracování s rozbočovače události]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-45f43fc3
[queued messaging solution]: ../service-bus-messaging/service-bus-dotnet-multi-tier-app-using-service-bus-queues.md
 
