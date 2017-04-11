<properties
    pageTitle="Začínáme s rozbočovače událostí v C | Microsoft Azure"
    description="Postupujte podle kurzu, který začít používat Azure události rozbočovače; odeslání události C a dostávat do jazyka Java pomocí Host procesor události."
    services="event-hubs"
    documentationCenter=""
    authors="jtaubensee"
    manager="timlt"
    editor=""/>

<tags
    ms.service="event-hubs"
    ms.workload="na"
    ms.tgt_pltfrm="c"
    ms.devlang="csharp"
    ms.topic="article"
    ms.date="09/27/2016"
    ms.author="jotaub;sethm"/>

# <a name="get-started-with-event-hubs"></a>Začínáme s rozbočovače události

[AZURE.INCLUDE [service-bus-selector-get-started](../../includes/service-bus-selector-get-started.md)]

## <a name="introduction"></a>Úvod

Událost rozbočovače je velmi scalable požití systém, který můžete příjmu miliony událostí sekundu povolení aplikace pro zpracování a analyzovat větší dat vytvořené pomocí připojení zařízení nebo aplikací. Jakmile získány do rozbočovače událost, můžete transformace a ukládání dat pomocí zprostředkovatelů v reálném čase analýzy nebo obrázku úložiště.

Další informace najdete [Přehled rozbočovače události][].

V tomto kurzu se dozvíte, jak jedí zprávy do události rozbočovače pomocí aplikace konzoly v C a načíst je paralelní pomocí C# [Události procesor hostitele][] knihovna.

Pro dokončení tohoto kurzu budete potřebovat:

+ Vývojové prostředí C. Pro účely tohoto návodu jsme převezme zásobníku RSZ na [Azure Linux OM](../virtual-machines/virtual-machines-linux-quick-create-cli.md) se systémem Ubuntu 14.04. Externí odkazy budou uvedeny pokyny pro jiná prostředí.

+ Microsoft Visual Studio Express pro Windows

+ Účet Azure active. <br/>Pokud nemáte účet, můžete vytvořit bezplatný účet v jenom pár minut. Podrobnosti najdete v tématu <a href="http://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdevelop%2Fmobile%2Ftutorials%2Fget-started%2F" target="_blank">Bezplatnou zkušební verzi Azure</a>.

[AZURE.INCLUDE [event-hubs-create-event-hub](../../includes/event-hubs-create-event-hub.md)]

[AZURE.INCLUDE [service-bus-event-hubs-get-started-send-c](../../includes/service-bus-event-hubs-get-started-send-c.md)]

[AZURE.INCLUDE [service-bus-event-hubs-get-started-receive-ephjava](../../includes/service-bus-event-hubs-get-started-receive-ephjava.md)]

## <a name="run-the-applications"></a>Spuštění aplikace

Teď jste připraveni ke spuštění aplikace.

1.  Spusťte project **příjemce** .

    ![][21]

2.  Spuštění programu **odesílatele** a Všimněte si události, které se zobrazí v okně příjemce.

    ![][24]

## <a name="next-steps"></a>Další kroky

Teď nevytvoříte funkční aplikaci, která vytvoří rozbočovači událostí a umožňuje volání a přijímání dat, můžete na přesouvat v následujících případech:

- Kompletní [Ukázková aplikace, který využívá rozbočovače události][].
- Ukázka [měřítko, událostí zpracování s rozbočovače události][] .
- [Přehled rozbočovače událostí][]

<!-- Images. -->
[21]: ./media/event-hubs-c-ephjava-getstarted/ephjava.png
[24]: ./media/event-hubs-c-ephjava-getstarted/receive-eph-c.png

<!-- Links -->
[Azure classic portal]: https://manage.windowsazure.com/
[Host (hostitel) procesor události]: https://www.nuget.org/packages/Microsoft.Azure.ServiceBus.EventProcessorHost
[Přehled rozbočovače událostí]: event-hubs-overview.md
[Ukázka aplikace, která používá rozbočovače události]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-286fd097
[Rozšiřování události zpracování s rozbočovače události]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-45f43fc3
