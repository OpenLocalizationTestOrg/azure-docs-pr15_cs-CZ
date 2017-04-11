<properties
    pageTitle="Začínáme s rozbočovače událostí v Java | Microsoft Azure"
    description="Postupujte podle kurzu, který začít používat Azure události rozbočovače; odesílání událostí pomocí jazyka Java a přijímání při používání EventProcessorHost."
    services="event-hubs"
    documentationCenter=""
    authors="jtaubensee"
    manager="timlt"
    editor=""/>

<tags
    ms.service="event-hubs"
    ms.workload="core"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/27/2016"
    ms.author="jotaub;sethm"/>

# <a name="get-started-with-event-hubs"></a>Začínáme s rozbočovače události

[AZURE.INCLUDE [service-bus-selector-get-started](../../includes/service-bus-selector-get-started.md)]

## <a name="introduction"></a>Úvod

Událost rozbočovače je velmi scalable požití systém, který můžete příjmu miliony událostí sekundu povolení aplikace pro zpracování a analyzovat větší dat vytvořené pomocí připojení zařízení nebo aplikací. Jakmile získány do rozbočovače událost, můžete transformace a ukládání dat pomocí zprostředkovatelů v reálném čase analýzy nebo obrázku úložiště.

Další informace najdete v tématu [Přehled rozbočovače události][].

Tento kurz ukazuje, jak jedí zprávy do události rozbočovače pomocí aplikace konzoly v Java a načíst je paralelní pomocí jazyka Java události procesor hostitelská knihovna.

Dokončení tohoto kurzu budete potřebovat:

+ Vývojové prostředí Java. Pro účely tohoto návodu jsme převezme [zatmění](https://www.eclipse.org/).

+ Účet Azure active. <br/>Pokud nemáte účet, můžete vytvořit bezplatný účet v jenom pár minut. Podrobnosti najdete v tématu <a href="http://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdevelop%2Fmobile%2Ftutorials%2Fget-started%2F" target="_blank">Bezplatnou zkušební verzi Azure</a>.

[AZURE.INCLUDE [event-hubs-create-event-hub](../../includes/event-hubs-create-event-hub.md)]

[AZURE.INCLUDE [service-bus-event-hubs-get-started-send-java](../../includes/service-bus-event-hubs-get-started-send-java.md)]

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

Další informace najdete v tématu [Středisko pro vývojáře Java](/develop/java/).

<!-- Images. -->
[21]: ./media/event-hubs-java-ephjava-getstarted/ephjava.png
[22]: ./media/event-hubs-java-ephjava-getstarted/java-send.png

<!-- Links -->
[Azure classic portal]: https://manage.windowsazure.com/
[Přehled rozbočovače událostí]: event-hubs-overview.md
[Ukázka aplikace, která používá rozbočovače události]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-286fd097
[Rozšiřování události zpracování s rozbočovače události]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-45f43fc3
 