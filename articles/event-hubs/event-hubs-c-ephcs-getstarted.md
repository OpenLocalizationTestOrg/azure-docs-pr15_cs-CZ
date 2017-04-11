<properties
    pageTitle="Začínáme s rozbočovače událostí v C a C# | Microsoft Azure"
    description="Postupujte podle kurzu, který začít používat Azure události rozbočovače; odeslání události C a příjem okraje v jazyce C# pomocí EventProcessorHost."
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
    ms.date="08/16/2016"
    ms.author="jotaub;sethm"/>

# <a name="get-started-with-event-hubs"></a>Začínáme s rozbočovače události

[AZURE.INCLUDE [service-bus-selector-get-started](../../includes/service-bus-selector-get-started.md)]

## <a name="introduction"></a>Úvod

Událost rozbočovače je velmi scalable požití systém, který můžete příjmu miliony událostí sekundu povolení aplikace pro zpracování a analyzovat větší dat vytvořené pomocí připojení zařízení nebo aplikací. Jakmile získány do rozbočovače událost, můžete transformace a ukládání dat pomocí zprostředkovatelů v reálném čase analýzy nebo obrázku úložiště.

Další informace najdete [Přehled rozbočovače událostí][].

V tomto kurzu se dozvíte, jak jedí zprávy do události rozbočovače pomocí aplikace konzoly v C a načíst je paralelní pomocí C# [Události procesor hostitele][] knihovna.

Tento kurz budete potřebovat:

+ Vývojové prostředí C. Pro účely tohoto návodu jsme převezme zásobníku RSZ na [Azure Linux OM](../virtual-machines/virtual-machines-linux-quick-create-cli.md) se systémem Ubuntu 14.04. Externí odkazy budou uvedeny pokyny pro jiná prostředí.

+ Microsoft Visual Studio Express pro Windows

+ Účet Azure active. Pokud nemáte účet, můžete vytvořit bezplatný účet zkušební v jenom pár minut. Podrobnosti najdete v tématu [Bezplatnou zkušební verzi Azure](https://azure.microsoft.com/pricing/free-trial/).

[AZURE.INCLUDE [event-hubs-create-event-hub](../../includes/event-hubs-create-event-hub.md)]

[AZURE.INCLUDE [service-bus-event-hubs-get-started-send-c](../../includes/service-bus-event-hubs-get-started-send-c.md)]

[AZURE.INCLUDE [service-bus-event-hubs-get-started-receive-ephcs](../../includes/service-bus-event-hubs-get-started-receive-ephcs.md)]

## <a name="run-the-applications"></a>Spuštění aplikace

Teď jste připraveni ke spuštění aplikace.

1.  Spustit **sluchátko** projektu v aplikaci Visual Studiu a počkejte, aby se spouštěla příjemce pro všechny oddíly.

    ![][21]

2.  Spuštění programu **odesílatele** a zjistěte, události, které se zobrazí v okně příjemce.

    ![][24]

## <a name="next-steps"></a>Další kroky

Teď nevytvoříte funkční aplikaci, která vytvoří rozbočovači událostí a umožňuje volání a přijímání dat, můžete na přesouvat v následujících případech:

- Kompletní [Ukázková aplikace, který využívá rozbočovače události][].
- Ukázka [měřítko, událostí zpracování s rozbočovače události][] .
- [Přehled rozbočovače událostí][]

<!-- Images. -->
[21]: ./media/event-hubs-c-ephcs-getstarted/run-csharp-ephcs1.png
[24]: ./media/event-hubs-c-ephcs-getstarted/receive-eph-c.png

<!-- Links -->
[Azure classic portal]: https://manage.windowsazure.com/
[Host (hostitel) procesor události]: https://www.nuget.org/packages/Microsoft.Azure.ServiceBus.EventProcessorHost
[Přehled rozbočovače událostí]: event-hubs-overview.md
[Ukázka aplikace, která používá rozbočovače události]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-286fd097
[Rozšiřování události zpracování s rozbočovače události]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-45f43fc3
