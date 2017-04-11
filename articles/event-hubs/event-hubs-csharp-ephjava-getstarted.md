<properties
    pageTitle="Začínáme s rozbočovače událostí v jazyce C# | Microsoft Azure"
    description="Postupujte podle kurzu, který začít používat Azure události rozbočovače; odeslání událostí v jazyce C# a dostávat do Java s využitím EventProcessorHost."
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
    ms.date="09/27/2016"
    ms.author="jotaub;sethm"/>

# <a name="get-started-with-event-hubs"></a>Začínáme s rozbočovače události

[AZURE.INCLUDE [service-bus-selector-get-started](../../includes/service-bus-selector-get-started.md)]

## <a name="introduction"></a>Úvod

Událost rozbočovače je služba, zpracování velkých objemů dat události (telemetrie) z připojeného zařízení a aplikací. Po shromáždit data do události rozbočovače můžete ukládat data pomocí clusteru úložiště nebo transformaci pomocí zprostředkovatele v reálném čase analýzy. Tato možnost shromažďování a zpracování velké události je hlavní součástí moderní aplikace architektury včetně Internet věcí (IoT).

Tento kurz ukazuje, jak používat portál Azure klasické vytvořit rozbočovači události. Je také se dozvíte, jak shromažďovat zprávy do události rozbočovače použitím aplikace konzoly vytvořené v jazyce C# a jak načíst paralelní pomocí jazyka Java události procesor hostitelská knihovna.

Tento kurz, musíte mít takto:

+ [Microsoft Visual Studio](http://visualstudio.com)

+ Účet Azure active. <br/>Pokud nemáte, můžete vytvořit bezplatný účet v jenom pár minut. Podrobnosti najdete v tématu [Bezplatnou zkušební verzi Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdevelop%2Fmobile%2Ftutorials%2Fget-started%2F target="_blank").

[AZURE.INCLUDE [event-hubs-create-event-hub](../../includes/event-hubs-create-event-hub.md)]

[AZURE.INCLUDE [service-bus-event-hubs-get-started-send-csharp](../../includes/service-bus-event-hubs-get-started-send-csharp.md)]

[AZURE.INCLUDE [service-bus-event-hubs-get-started-receive-ephjava](../../includes/service-bus-event-hubs-get-started-receive-ephjava.md)]

## <a name="run-the-applications"></a>Spuštění aplikace

Teď jste připraveni ke spuštění aplikace.

1.  Spusťte project **příjemce** .

    ![][21]

2.  Spusťte project **odesílatele** .

    ![][22]

## <a name="next-steps"></a>Další kroky

Teď nevytvoříte funkční aplikaci, která vytvoří rozbočovači událostí a umožňuje volání a přijímání dat, můžete na přesouvat v následujících případech:

- Kompletní [Ukázková aplikace, který využívá rozbočovače události][].
- Ukázka [měřítko, událostí zpracování s rozbočovače události][] .
- [Přehled rozbočovače událostí][]

<!-- Images. -->
[21]: ./media/event-hubs-csharp-ephjava-getstarted/ephjava.png
[22]: ./media/event-hubs-csharp-ephjava-getstarted/cs-send.png

<!-- Links -->
[Azure classic portal]: https://manage.windowsazure.com/
[Přehled rozbočovače událostí]: event-hubs-overview.md
[Ukázka aplikace, která používá rozbočovače události]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-286fd097
[Rozšiřování události zpracování s rozbočovače události]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-45f43fc3
