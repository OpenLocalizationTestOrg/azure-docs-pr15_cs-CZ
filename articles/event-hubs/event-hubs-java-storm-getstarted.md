<properties
    pageTitle="Začínáme s rozbočovače událostí v Java s Apache bouře | Microsoft Azure"
    description="Postupujte podle kurzu, který začít používat Azure události rozbočovače; událostí pomocí jazyka Java odesílání a přijímání je v Apache bouře obrázku."
    services="event-hubs"
    documentationCenter=""
    authors="fsautomata"
    manager="timlt"
    editor=""/>

<tags
    ms.service="event-hubs"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/06/2016"
    ms.author="sethm"/>

# <a name="get-started-with-event-hubs"></a>Začínáme s rozbočovače události

[AZURE.INCLUDE [service-bus-selector-get-started](../../includes/service-bus-selector-get-started.md)]

## <a name="introduction"></a>Úvod

Událost rozbočovače je velmi scalable požití systém, který můžete příjmu miliony událostí sekundu povolení aplikace pro zpracování a analyzovat větší dat vytvořené pomocí připojení zařízení nebo aplikací. Jakmile získány do rozbočovače událost, můžete transformace a ukládání dat pomocí zprostředkovatelů v reálném čase analýzy nebo obrázku úložiště.

Další informace najdete v tématu [Přehled rozbočovače událostí][].

Tento kurz popisuje, jak shromažďování zpráv do události rozbočovače pomocí aplikace konzoly v Java a načíst je paralelně pomocí Apache bouře.

Tento kurz budete potřebovat:

+ Java prostředí pro vývoj nakonfigurován pro spuštění [Maven](http://maven.apache.org/). Pro účely tohoto návodu předpokladu, že [zatmění](https://www.eclipse.org/).

+ Účet Azure active. Pokud nemáte účet, můžete vytvořit bezplatný účet zkušební v jenom pár minut. Podrobnosti najdete v tématu [Bezplatnou zkušební verzi Azure](https://azure.microsoft.com/pricing/free-trial/).

[AZURE.INCLUDE [event-hubs-create-event-hub](../../includes/event-hubs-create-event-hub.md)]

[AZURE.INCLUDE [service-bus-event-hubs-get-started-send-java](../../includes/service-bus-event-hubs-get-started-send-java.md)]


[AZURE.INCLUDE [service-bus-event-hubs-get-started-receive-storm](../../includes/service-bus-event-hubs-get-started-receive-storm.md)]

## <a name="run-the-applications"></a>Spuštění aplikace

Teď jste připraveni ke spuštění aplikace.

1.  Spuštění třídy **LogTopology** z zatmění a počkejte, aby se spouštěla příjemce pro všechny oddíly.

2.  Spuštění projektu **odesílatele** , stiskněte klávesu **Enter** v okně konzoly a najdete v článku události, které se zobrazí v okně příjemce.

    ![][22]

> [AZURE.NOTE] V tomto kurzu pouze pomocí bouře v místním režimu k vývoji. Naleznete [HDInsight bouře přehled][] a oficiální [Apache bouře][] dokumentaci další informace bouře nasazení a vzorků.

## <a name="next-steps"></a>Další kroky

V následujících zdrojích jsou k dispozici pro vývoj aplikací integrace rozbočovače událostí a bouře.

- [Analýza dat senzor s bouře a HDInsight] je celé scénář kurz pomocí rozbočovače události, bouře a HBase jedí data snímače v Hadoop obrázku.
- [Zpracování dat aplikace s SCP.NET a C# na bouře a HDInsight datových proudů vývoje][] je návod k zápisu bouře potrubí pomocí C#.

<!-- Images. -->
[22]: ./media/event-hubs-java-storm-getstarted/receive-storm2.png

<!-- Links -->
[Azure classic portal]: https://manage.windowsazure.com/
[Event Processor Host]: https://www.nuget.org/packages/Microsoft.Azure.ServiceBus.EventProcessorHost
[Přehled rozbočovače událostí]: event-hubs-overview.md

[Apache bouře]: https://storm.incubator.apache.org
[Přehled bouře HDInsight]: ../hdinsight/hdinsight-storm-overview.md
[Analýza dat senzor s bouře a HDInsight]: ../hdinsight/hdinsight-storm-sensor-data-analysis.md
[Můžete vyvíjet aplikace zpracování dat s SCP.NET a C# na bouře a HDInsight datových proudů]: ../hdinsight/hdinsight-storm-develop-csharp-visual-studio-topology.md
 