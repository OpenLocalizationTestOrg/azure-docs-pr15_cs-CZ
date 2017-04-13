<properties
   pageTitle="Stavová diagnostiky spolehlivé služby | Microsoft Azure"
   description="Diagnostické funkce spolehlivé služby stavovou"
   services="service-fabric"
   documentationCenter=".net"
   authors="AlanWarwick"
   manager="timlt"
   editor=""/>

<tags
   ms.service="Service-Fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="05/17/2016"
   ms.author="alanwar"/>

# <a name="diagnostic-functionality-for-stateful-reliable-services"></a>Diagnostické funkce spolehlivé služby stavovou
Stavová spolehlivé služby StatefulServiceBase třídy posílá [Zdroj_události](https://msdn.microsoft.com/library/system.diagnostics.tracing.eventsource.aspx) události, které mohou sloužit k ladění službu, a poskytují podstatu fungování modul runtime je a pomoci při řešení.

## <a name="eventsource-events"></a>ZDROJ_UDÁLOSTI události
Název ZDROJ_UDÁLOSTI stavovou spolehlivé služby StatefulServiceBase předmětu je "Služby společnosti Microsoft ServiceFabric". Události z tohoto zdroje událostí se zobrazí v okně [Diagnostických nástrojů události](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md#view-service-fabric-system-events-in-visual-studio) při službu je [ladění ve Visual Studiu](service-fabric-debugging-your-application.md).

Nástroje a technologií pro server, které usnadnit shromažďování a/nebo zobrazení ZDROJ_UDÁLOSTI událostí příklady [PerfView](http://www.microsoft.com/download/details.aspx?id=28567) [Diagnostických nástrojů Microsoft Azure](../cloud-services/cloud-services-dotnet-diagnostics.md)a [Microsoft TraceEvent knihovny](http://www.nuget.org/packages/Microsoft.Diagnostics.Tracing.TraceEvent).

## <a name="events"></a>Události

|Název události|ID události|Úroveň|Popis události|
|----------|--------|-----|-----------------|
|StatefulRunAsyncInvocation|1|Informační|Když je spuštěn služby RunAsync úkolu|
|StatefulRunAsyncCancellation|2|Informační|Když je zrušená služby RunAsync úkolu|
|StatefulRunAsyncCompletion|3|Informační|Když se po dokončení úkolu RunAsync služby|
|StatefulRunAsyncSlowCancellation|4|Upozornění|Když se úkol RunAsync služby trvá příliš dlouho dokončete zrušení|
|StatefulRunAsyncFailure|5|Chyba|Když se výjimku služby RunAsync úkolu|

## <a name="interpret-events"></a>Interpretace události

StatefulRunAsyncInvocation StatefulRunAsyncCompletion a StatefulRunAsyncCancellation události, které jsou užitečné pro zápis služby pochopit životním cyklu služby –, jakož i časování při zahájení, zrušen nebo dokončit službu. To může být užitečné při ladění problémy se službou nebo vysvětlení životního cyklu služby.

Autoři služby by měly zaměřit na StatefulRunAsyncSlowCancellation a StatefulRunAsyncFailure události protože označují problémy se službou.

StatefulRunAsyncFailure je vyzařovaného kdykoli úkolu RunAsync() služby výjimku. Obvykle výjimce označuje chyby nebo chyby ve službě. Kromě toho výjimku způsobí, že k chybě, tak, aby se přesune do jiného uzlu. Může být operaci drahé a zpozdit příchozí žádosti při přesunutí službu. Autoři služby by měl zjistit příčinu výjimky a pokud je to možné, zmírnit.

StatefulRunAsyncSlowCancellation je vyzařovaného pokaždé, když na žádost o zrušení pro daný úkol RunAsync trvá déle než čtyř sekund. Když do služby trvá příliš dlouho dokončete zrušení, vliv možnost pro službu rychle restartovat na jiném uzlu. To má vliv celkové dostupnosti služby.
