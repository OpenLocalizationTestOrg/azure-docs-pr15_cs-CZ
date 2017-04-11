<properties
    pageTitle="Začínáme s rozbočovače událostí v jazyce C# s Apache bouře | Microsoft Azure"
    description="Postupujte podle kurzu, který začít používat Azure události rozbočovače; odeslání události C# a dostávat do Apache bouře obrázku."
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
    ms.topic="article" 
    ms.date="09/06/2016"
    ms.author="jotaub;sethm"/>

# <a name="get-started-with-event-hubs"></a>Začínáme s rozbočovače události

[AZURE.INCLUDE [service-bus-selector-get-started](../../includes/service-bus-selector-get-started.md)]

## <a name="introduction"></a>Úvod

Událost rozbočovače je velmi scalable požití systém, který můžete příjmu miliony událostí sekundu povolení aplikace pro zpracování a analyzovat větší dat vytvořené pomocí připojení zařízení nebo aplikací. Jakmile získány do rozbočovače událost, můžete transformace a ukládání dat pomocí zprostředkovatelů v reálném čase analýzy nebo obrázku úložiště.

Další informace najdete [Přehled rozbočovače události].

V tomto kurzu se dozvíte, jak jedí zprávy do události rozbočovače pomocí aplikace konzoly v jazyce C# a načíst je paralelně pomocí Apache bouře.

Pro dokončení tohoto kurzu budete potřebovat:

+ [Microsoft Visual Studio](http://visualstudio.com)

+ Java prostředí pro vývoj nakonfigurován pro spuštění [Maven](http://maven.apache.org/). Pro účely tohoto návodu jsme převezmou [zatmění](https://www.eclipse.org/)

+ Účet Azure active. <br/>Pokud nemáte účet, můžete vytvořit bezplatný účet v jenom pár minut. Podrobnosti najdete v tématu <a href="http://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdevelop%2Fmobile%2Ftutorials%2Fget-started%2F" target="_blank">Bezplatnou zkušební verzi Azure</a>.

[AZURE.INCLUDE [event-hubs-create-event-hub](../../includes/event-hubs-create-event-hub.md)]

[AZURE.INCLUDE [service-bus-event-hubs-get-started-send-csharp](../../includes/service-bus-event-hubs-get-started-send-csharp.md)]


[AZURE.INCLUDE [service-bus-event-hubs-get-started-receive-storm](../../includes/service-bus-event-hubs-get-started-receive-storm.md)]

## <a name="run-the-applications"></a>Spuštění aplikace

Teď jste připraveni ke spuštění aplikace.

1.  Spuštění třídy **LogTopology** z zatmění a počkejte, aby se spouštěla příjemce pro všechny oddíly.

2.  Spuštění projektu **odesílatele** , stiskněte klávesu **Enter** v okně konzoly a najdete v článku události, které se zobrazí v okně příjemce.

    ![][22]

## <a name="next-steps"></a>Další kroky

Teď nevytvoříte funkční aplikaci, která vytvoří rozbočovači událostí a umožňuje volání a přijímání dat, můžete na přesouvat v následujících případech:

- Kompletní [Ukázková aplikace, který využívá rozbočovače události][].
- Ukázka [měřítko, událostí zpracování s rozbočovače události][] .

<!-- Images. -->
[22]: ./media/event-hubs-csharp-storm-getstarted/receive-storm1.png

<!-- Links -->
[Azure classic portal]: https://manage.windowsazure.com/
[Přehled rozbočovače událostí]: event-hubs-overview.md
[Ukázka aplikace, která používá rozbočovače události]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-286fd097
[Rozšiřování události zpracování s rozbočovače události]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-45f43fc3
 