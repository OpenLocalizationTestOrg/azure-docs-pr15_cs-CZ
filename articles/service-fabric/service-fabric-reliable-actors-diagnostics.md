<properties
   pageTitle="Diagnostika účastníky a sledování | Microsoft Azure"
   description="Tento článek popisuje diagnostických nástrojů a funkcí v modulu runtime služby struktury spolehlivé účastníky, včetně událostí a které ho výkonnosti sledování výkonu."
   services="service-fabric"
   documentationCenter=".net"
   authors="abhishekram"
   manager="timlt"
   editor="vturecek"/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="07/05/2016"
   ms.author="abhisram"/>

# <a name="diagnostics-and-performance-monitoring-for-reliable-actors"></a>Nástroje pro diagnostiku a sledování výkonu pro spolehlivé účastníky
Modul runtime spolehlivé účastníky posílá [Zdroj_události](https://msdn.microsoft.com/library/system.diagnostics.tracing.eventsource.aspx) událostí a [výkonnosti](https://msdn.microsoft.com/library/system.diagnostics.performancecounter.aspx). Tyto poskytují podstatu jak modul runtime pracuje a pomáhají při řešení potíží a sledování výkonu.

## <a name="eventsource-events"></a>ZDROJ_UDÁLOSTI události
Název poskytovatele ZDROJ_UDÁLOSTI runtime spolehlivé účastníky je "Microsoft účastníky ServiceFabric". Události z tohoto zdroje událostí se zobrazí v okně [Diagnostických nástrojů události](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md#view-service-fabric-system-events-in-visual-studio) při actor aplikace, která má být [ladění ve Visual Studiu](service-fabric-debugging-your-application.md).

Nástroje a technologií pro server, které usnadnit shromažďování a/nebo zobrazení ZDROJ_UDÁLOSTI událostí příklady [PerfView](http://www.microsoft.com/download/details.aspx?id=28567) [Diagnostiky Azure](../cloud-services/cloud-services-dotnet-diagnostics.md), [Sémantickou protokolování](https://msdn.microsoft.com/library/dn774980.aspx)a [Microsoft TraceEvent knihovny](http://www.nuget.org/packages/Microsoft.Diagnostics.Tracing.TraceEvent).

### <a name="keywords"></a>Klíčová slova
Všechny události, které patří k spolehlivé ZDROJ_UDÁLOSTI objekty actor spojená se jeden nebo více klíčových slov. Díky filtrování událostí, které byly shromážděny. Jsou definované následující bitů klíčových slov.

|Bit|Popis|
|---|---|
|0x1|Sada důležité události, jež shrnují operace runtime struktury účastníky.|
|0x2|Nastavení zvláštní události, které popisují volání metody actor Další informace najdete v tématu [úvodní téma na účastníky](service-fabric-reliable-actors-introduction.md#actors).|
|0x4|Nastavení události týkající se stavu actor. Další informace naleznete v tématu na [Správa stavu actor](service-fabric-reliable-actors-state-management.md).|
|0x8|Nastavení události týkající se systémem zapnout souběžné v actor. Další informace naleznete v tématu na [souběžné](service-fabric-reliable-actors-introduction.md#concurrency).|

## <a name="performance-counters"></a>Výkonnosti
Modul runtime spolehlivé účastníky definuje těchto kategorií čítač výkonu.

|Kategorie|Popis|
|---|---|
|Služba struktury Actor|Čítače specifické pro struktury služby Azure účastníky, například času věnovat uložení actor stavu|
|Metoda Actor struktury služby|Čítače specifické pro metody prováděným služby struktury účastníky, například jak často metodu actor vyvolání|

Každá z výše uvedených kategorií má jeden nebo více čítačů.

[Sledování výkonu systému Windows](https://technet.microsoft.com/library/cc749249.aspx) aplikace, která je k dispozici ve výchozím nastavení v operačním systému Windows mohou sloužit k shromažďovat a zobrazit data čítačů. [Diagnostika Azure](../cloud-services/cloud-services-dotnet-diagnostics.md) je jinou možnost pro shromažďování dat čítačů výkonu a nahrávat na Azure tabulek.

### <a name="performance-counter-instance-names"></a>Názvy instance čítačů výkonu
Clusteru, který obsahuje velké množství actor služeb nebo oddíly služby actor bude mít velkého počtu instance čítačů výkonu actor. Názvy instance čítačů výkonu můžou pomoct při určování konkrétní [oddíl](service-fabric-reliable-actors-platform.md#service-fabric-partition-concepts-for-actors) a actor metoda (pokud existuje) přidružené instanci čítače výkonu.

#### <a name="service-fabric-actor-category"></a>Kategorie Actor struktury služby
Kategorie `Service Fabric Actor`, jsou čítač instance jména v tomto formátu:

`ServiceFabricPartitionID_ActorsRuntimeInternalID`

*ServiceFabricPartitionID* je řetězec představuje ID oddílu Service struktury, kterému je přidružen instanci čítače výkonu. ID oddílu je GUID a vyjádření řetězec generováno prostřednictvím [`Guid.ToString`](https://msdn.microsoft.com/library/97af8hh4.aspx) metoda specifikátorem formátu "D".

*ActorRuntimeInternalID* je číselné řetězce 64-bit celé číslo, které je generováno aplikací struktury účastníky runtime pro interní potřebu. To je součástí název instance čítače výkonu zajistí jeho jedinečnost a aby nedošlo ke konfliktu s názvy instance čítačů jiných výkonu. Uživatelé neměli pokusíte interpretovat tuto část název instance čítače výkonu.

Následuje příklad název instance čítač čítač, který patří `Service Fabric Actor` kategorie:

`2740af29-78aa-44bc-a20b-7e60fb783264_635650083799324046`

Ve výše uvedeném příkladu `2740af29-78aa-44bc-a20b-7e60fb783264` řetězec znázorňuje ID oddílu Service struktury a `635650083799324046` pomocí 64-bit ID, které je generováno pro modul runtime je vnitřní.

#### <a name="service-fabric-actor-method-category"></a>Metoda Actor struktury služby kategorie
Kategorie `Service Fabric Actor Method`, jsou čítač instance jména v tomto formátu:

`MethodName_ActorsRuntimeMethodId_ServiceFabricPartitionID_ActorsRuntimeInternalID`

*MethodName* je název objektu actor metody, kterému je přidružen instanci čítače výkonu. Formát pod názvem metody závisí na některé logiky runtime struktury účastníky, která zůstatky čitelnost název u omezujících maximální délka názvy instance čítačů výkonu ve Windows.

*ActorsRuntimeMethodId* je číselné řetězce 32bitové celé číslo, které je generováno aplikací struktury účastníky runtime pro interní potřebu. To je součástí název instance čítače výkonu zajistí jeho jedinečnost a aby nedošlo ke konfliktu s názvy instance čítačů jiných výkonu. Uživatelé neměli pokusíte interpretovat tuto část název instance čítače výkonu.

*ServiceFabricPartitionID* je číselné řetězce ID oddílu Service struktury, kterému je přidružen instanci čítače výkonu. ID oddílu je GUID a vyjádření řetězec generováno prostřednictvím [`Guid.ToString`](https://msdn.microsoft.com/library/97af8hh4.aspx) metoda specifikátorem formátu "D".

*ActorRuntimeInternalID* je číselné řetězce 64-bit celé číslo, které je generováno aplikací struktury účastníky runtime pro interní potřebu. To je součástí název instance čítače výkonu zajistí jeho jedinečnost a aby nedošlo ke konfliktu s názvy instance čítačů jiných výkonu. Uživatelé neměli pokusíte interpretovat tuto část název instance čítače výkonu.

Následuje příklad název instance čítač čítač, který patří `Service Fabric Actor Method` kategorie:

`ivoicemailboxactor.leavemessageasync_2_89383d32-e57e-4a9b-a6ad-57c6792aa521_635650083804480486`

Ve výše uvedeném příkladu `ivoicemailboxactor.leavemessageasync` je název metody `2` je 32bitová verze ID generované pro interní použití modul runtime `89383d32-e57e-4a9b-a6ad-57c6792aa521` řetězec znázorňuje ID oddílu Service struktury a `635650083804480486` pomocí ID 64-bit generované pro modul runtime je vnitřní.

## <a name="list-of-events-and-performance-counters"></a>Seznam události a výkonnosti

### <a name="actor-method-events-and-performance-counters"></a>Objekt actor metoda událostí a výkonnosti
Modul runtime spolehlivé účastníky posílá následujících událostí souvisejících s [actor metody](service-fabric-reliable-actors-introduction.md#actors).

|Název události|ID události|Úroveň|Klíčové slovo|Popis|
|---|---|---|---|---|
|ActorMethodStart|7|Podrobný|0x2|Objekty actor runtime je vyvolat metodu actor.|
|ActorMethodStop|8|Podrobný|0x2|Metody actor dokončí. To znamená modul runtime asynchronní volání metodu actor vrátil a dokončí Úloha vrácená metodu actor.|
|ActorMethodThrewException|9|Upozornění|0x3|Došlo k výjimce během provádění metody actor během modul runtime asynchronní volání metody actor nebo při provádění úkol vrácené metodou actor. Tato událost označuje určitý druh Chyba instalace v actor kód, který potřebuje vyšetřování.|

Modul runtime spolehlivé účastníky publikuje následující vyvolaly související s spuštění actor metody.

|Název kategorie|Název čítače|Popis|
|---|---|---|
|Metoda Actor struktury služby|Vyvolání/Sec|Počet metodu služby actor vyvolat za sekundu|
|Metoda Actor struktury služby|Průměrná milisekundy na volání|Doba k provedení metodu actor služby v milisekundách|
|Metoda Actor struktury služby|Vyvolání výjimky/Sec|Počet actor služby metoda došlo k výjimce za sekundu|

### <a name="concurrency-events-and-performance-counters"></a>Souběžné událostí a výkonnosti
Modul runtime spolehlivé účastníky posílá následujících událostí souvisejících s [souběžné](service-fabric-reliable-actors-introduction.md#concurrency).

|Název události|ID události|Úroveň|Klíčové slovo|Popis|
|---|---|---|---|---|
|ActorMethodCallsWaitingForLock|12|Podrobný|0x8|Na začátku každé nové otočení objektu actor zapsání této události. Obsahuje počet čekajících actor hovory, které čekají na získání zámku za actor, který vynucuje na základě zapnout souběžné.|

Modul runtime spolehlivé účastníky publikuje následující vyvolaly související s souběžné.

|Název kategorie|Název čítače|Popis|
|---|---|---|
|Služba struktury Actor|# volání actor čekání actor zámek|Počet čekajících actor hovory čekající na získání zámku za actor, který vynucuje na základě zapnout souběžné|
|Služba struktury Actor|Průměrná milisekundy na lock čekání|Doba trvání (v milisekundách) k získání zámku za actor, který vynucuje na základě zapnout souběžné|
|Služba struktury Actor|Průměrná milisekund actor uzamčení|Čas (v milisekundách), pro kterou směřuje lock za actor|

### <a name="actor-state-management-events-and-performance-counters"></a>Objekt actor stavu řízení událostí a výkonnosti
Modul runtime spolehlivé účastníky posílá následujících událostí souvisejících se [správou stavu actor](service-fabric-reliable-actors-state-management.md).

|Název události|ID události|Úroveň|Klíčové slovo|Popis|
|---|---|---|---|---|
|ActorSaveStateStart|10|Podrobný|0x4|Objekty actor runtime je uložit stav actor.|
|ActorSaveStateStop|11|Podrobný|0x4|Objekty actor runtime proběhla ukládání stavu actor.|

Modul runtime spolehlivé účastníky publikuje následující vyvolaly související se správou stavu actor.

|Název kategorie|Název čítače|Popis|
|---|---|---|
|Služba struktury Actor|Průměrná milisekundy na Uložit činnost stavu|Čas potřebný k uložení actor stavu v milisekundách|
|Služba struktury Actor|Průměrná milisekundy na stav operace načítání|Čas potřebný k načtení actor stavu v milisekundách|

### <a name="events-related-to-actor-replicas"></a>Události související s repliky actor
Modul runtime spolehlivé účastníky posílá následujících událostí souvisejících s [repliky actor](service-fabric-reliable-actors-platform.md#service-fabric-partition-concepts-for-stateful-actors).

|Název události|ID události|Úroveň|Klíčové slovo|Popis|
|---|---|---|---|---|
|ReplicaChangeRoleToPrimary|1|Informační|0x1|Objekt actor otevřené změněné role na primární. To znamená, že objekty actor pro tento oddíl se vytvoří uvnitř tohoto otevřené.|
|ReplicaChangeRoleFromPrimary|2|Informační|0x1|Otevřené actor změněné role na jiná než primární. To znamená, že objekty actor pro tento oddíl už se vytvoří uvnitř tohoto otevřené. Do již vytvořené v rámci tohoto otevřené objekty actor budete dostávat žádné nových žádostí o. Účastníky bude odstraněna po provedení všech požadavků v průběhu.|

### <a name="actor-activation-and-deactivation-events-and-performance-counters"></a>Objekt actor aktivací a deaktivací událostí a výkonnosti
Modul runtime spolehlivé účastníky posílá následujících událostí souvisejících s [actor aktivací a deaktivací](service-fabric-reliable-actors-lifecycle.md).

|Název události|ID události|Úroveň|Klíčové slovo|Popis|
|---|---|---|---|---|
|ActorActivated|5|Informační|0x1|Objekt actor již aktivován.|
|ActorDeactivated|6|Informační|0x1|Objekt actor má deaktivovat.|

Modul runtime spolehlivé účastníky publikuje následující výkonu čítače actor aktivací a deaktivací.

|Název kategorie|Název čítače|Popis|
|---|---|---|
|Služba struktury Actor|Výpočet průměru OnActivateAsync milisekund|Doba k provedení OnActivateAsync metoda v milisekundách|

### <a name="actor-request-processing-performance-counters"></a>Zpracování žádostí o actor výkonnosti
Pokud klienta vyvolá metodu prostřednictvím objektu actor proxy, výsledkem zprávě s žádostí o odesílaného v síti ke službě actor. Služba zprávu žádost zpracuje a odešle odpověď zpátky ke klientovi. Modul runtime spolehlivé účastníky publikuje následující vyvolaly týkající se zpracování žádostí o actor.

|Název kategorie|Název čítače|Popis|
|---|---|---|
|Služba struktury Actor|# Zbývající žádostí o|Počet požadavků zpracovávání ve službě|
|Služba struktury Actor|Průměrná milisekundy na žádost o|Doba trvání (v milisekundách) službou zpracuje žádost|
|Služba struktury Actor|Průměrná milisekundách pro žádost o deserializace|Doba trvání (v milisekundách) deserializovat zprávě s žádostí o actor dala po přijetí ve službách|
|Služba struktury Actor|Průměrná milisekundách pro odpověď serializaci|Doba trvání (v milisekundách) serializovat odpověď actor ve službách před odesláním odpovědi na klienta|

## <a name="next-steps"></a>Další kroky
 - [Použití služby struktury platformy spolehlivé účastníky](service-fabric-reliable-actors-platform.md)
 - [Objekt actor rozhraní API dokumentaci](https://msdn.microsoft.com/library/azure/dn971626.aspx)
 - [Ukázkový kód](https://github.com/Azure/servicefabric-samples)
