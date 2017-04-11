<properties
    pageTitle="Začínáme s události rozbočovače s C a Apache bouře | Microsoft Azure"
    description="Postupujte podle kurzu, který začít používat Azure události rozbočovače; odeslání události C a dostávat do Apache bouře obrázku."
    services="event-hubs"
    documentationCenter=""
    authors="jtaubensee"
    manager="timlt"
    editor=""/>

<tags
    ms.service="event-hubs"
    ms.workload="na"
    ms.tgt_pltfrm="c"
    ms.devlang="java"
    ms.topic="article"
    ms.date="08/16/2016"
    ms.author="jotaub;sethm"/>

# <a name="get-started-with-event-hubs"></a>Začínáme s rozbočovače události

[AZURE.INCLUDE [service-bus-selector-get-started](../../includes/service-bus-selector-get-started.md)]

## <a name="introduction"></a>Úvod

Událost rozbočovače je velmi scalable požití systém, který můžete příjmu miliony událostí sekundu povolení aplikace pro zpracování a analyzovat větší dat vytvořené pomocí připojení zařízení nebo aplikací. Jakmile získány do rozbočovače událost, můžete transformace a ukládání dat pomocí zprostředkovatelů v reálném čase analýzy nebo obrázku úložiště.

Další informace najdete [Přehled rozbočovače události].

V tomto kurzu se dozvíte, jak jedí zprávy do události rozbočovače pomocí aplikace konzoly v C a načíst je paralelně pomocí Apache bouře.

Tento kurz budete potřebovat:

+ Vývojové prostředí C. Pro účely tohoto návodu jsme převezme zásobníku RSZ na [Azure Linux OM](../virtual-machines/virtual-machines-linux-quick-create-cli.md) se systémem Ubuntu 14.04. Externí odkazy budou uvedeny pokyny pro jiná prostředí.

+ Java prostředí pro vývoj nakonfigurován pro spuštění [Maven](http://maven.apache.org/). Pro účely tohoto návodu jsme převezme [zatmění](https://www.eclipse.org/).

+ Účet Azure active. Pokud nemáte účet, můžete vytvořit bezplatný účet v jenom pár minut. Podrobnosti najdete v tématu [Bezplatnou zkušební verzi Azure](https://azure.microsoft.com/pricing/free-trial/).

[AZURE.INCLUDE [event-hubs-create-event-hub](../../includes/event-hubs-create-event-hub.md)]

[AZURE.INCLUDE [service-bus-event-hubs-get-started-send-c](../../includes/service-bus-event-hubs-get-started-send-c.md)]

[AZURE.INCLUDE [service-bus-event-hubs-get-started-receive-storm](../../includes/service-bus-event-hubs-get-started-receive-storm.md)]

## <a name="run-the-applications"></a>Spuštění aplikace

Teď jste připraveni ke spuštění aplikace.

1.  Spuštění třídy **LogTopology** z zatmění a počkejte, aby se spouštěla příjemce pro všechny oddíly.

2.  Spuštění programu **odesílatele** a zjistěte, události, které se zobrazí v okně příjemce.

    ![][23]

> [AZURE.NOTE] V tomto kurzu jenom pomocí bouře v místním režimu k vývoji. Najdete [Přehled HDInsight bouře] a oficiální [Apache bouře] si přečtěte následující dokumentaci pro další informace bouře nasazení a vzorků.

## <a name="next-steps"></a>Další kroky

V následujících zdrojích jsou k dispozici pro vývoj aplikací integrace rozbočovače událostí a bouře.

- [Analýza dat senzor s bouře a HDInsight][] je celé scénář kurz pomocí rozbočovače události, bouře a HBase jedí data snímače v Hadoop obrázku.
- [Zpracování dat aplikace s SCP.NET a C# na bouře a HDInsight datových proudů vývoje][] je návod k zápisu bouře potrubí pomocí C#.

<!-- Images. -->
[23]: ./media/event-hubs-c-storm-getstarted/receive-storm3.png

<!-- Links -->
[Azure classic portal]: https://manage.windowsazure.com/
[Event Processor Host]: https://www.nuget.org/packages/Microsoft.Azure.ServiceBus.EventProcessorHost
[Přehled rozbočovače událostí]: event-hubs-overview.md

[Apache bouře]: https://storm.incubator.apache.org
[Přehled bouře HDInsight]: ../hdinsight/hdinsight-storm-overview.md/
[Analýza dat senzor s bouře a HDInsight]: ../hdinsight/hdinsight-storm-sensor-data-analysis.md
[Můžete vyvíjet aplikace zpracování dat s SCP.NET a C# na bouře a HDInsight datových proudů]: ../hdinsight/hdinsight-storm-develop-csharp-visual-studio-topology.md
