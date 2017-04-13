<properties
   pageTitle="Rozšířené spolehlivé služby využívá | Microsoft Azure"
   description="Informace o rozšířené použití služby struktury spolehlivé služby pro přidávat ještě flexibilněji svých služeb."
   services="Service-Fabric"
   documentationCenter=".net"
   authors="vturecek"
   manager="timlt"
   editor="masnider"/>

<tags
   ms.service="Service-Fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="10/19/2016"
   ms.author="vturecek"/>

# <a name="advanced-usage-of-the-reliable-services-programming-model"></a>Rozšířené možnosti použití spolehlivé služby programování modelu
Azure struktury služby zjednodušuje psaní a správa spolehlivé příslušnosti a stavové služby. Tato příručka, bude si o rozšířené použití spolehlivé služby získat lepší kontrolu a pružnost přes svých služeb. Před čtení Tato příručka, seznamte se s [Spolehlivé programovacího modelu služeb](service-fabric-reliable-services-introduction.md).

Stavová a příslušnosti služby mají dva hlavní vstupní body pro uživatele kód:

 - `RunAsync`je obecný vstupní bod pro kód služby.
 - `CreateServiceReplicaListeners`a `CreateServiceInstanceListeners` je určený pro otevírání komunikace posluchače požadavků klientů.
 
Většina státní jsou tyto dvě vstupní body dostatečné. V některých případech je-li větší kontrolu nad životního cyklu do služby vyžadováno, další životního cyklu události, které jsou k dispozici.

## <a name="stateless-service-instance-lifecycle"></a>Cyklus instance příslušnosti služby

Cyklus příslušnosti služby je velmi jednoduché. Příslušnosti služby lze pouze otevření, zavření nebo přerušena. `RunAsync`ve službě příslušnosti se provádí instanci služby je otevřen a zrušen při instanci služby je dojde k uzavření nebo přerušena. 

I když `RunAsync` by měl být v dostatečném skoro všech případech, otevření, zavření, a přerušení události příslušnosti služby, které jsou také k dispozici:

- `Task OnOpenAsync(IStatelessServicePartition, CancellationToken)`
  OnOpenAsync se nazývá při instance příslušnosti služby chystá použít. Rozšířené služby inicializace úkoly mohli začít v současné době.

- `Task OnCloseAsync(CancellationToken)`
  OnCloseAsync se nazývá při instance příslušnosti služby bude nutné řádně vypnout. Může dojít při probíhá upgrade kód služby, instance služby je přesunutý kvůli Vyrovnávání zatížení nebo ke zjištění přechodná poruch. OnCloseAsync mohou sloužit k bezpečně zavřete zdroje, zastavit zpracování pozadí, dokončení ukládání externí stavu nebo dolů existující připojení.

- `void OnAbort()`
  OnAbort nazývá při instance příslušnosti služby se vynutí vypíná. Obecně se používá při trvalá chyba ke zjištění na uzel nebo služby struktury spravovat nedají problémy se spolehlivým životního cyklu instanci služby kvůli k interním chybám.

## <a name="stateful-service-replica-lifecycle"></a>Stavová služba otevřené cyklus

Stavová služba otevřené cyklus je mnohem složitější než instance příslušnosti služby. Kromě toho otevření, zavření a přerušit události, projde otevřené stavová služba role změn během životnosti. Když otevřené stavová služba se změní role `OnChangeRoleAsync` události:

- `Task OnChangeRoleAsync(ReplicaRole, CancellationToken)`
  OnChangeRoleAsync se nazývá při otevřené stavová služba je změně role, například primární a sekundární. Primární repliky jsou uvedeny stav zápisu (jsou povoleno vytvářet a zapisovat do důvěryhodného kolekce). Sekundární repliky jsou uvedeny stav přečtení (může číst pouze z existující spolehlivé kolekce). Většina práce ve stavové služby provádí na primární otevřené. Sekundární repliky můžete provádět jen pro čtení ověření, generování sestav, dolování dat nebo jiné úlohy jen pro čtení.

Ve stavové služby jenom primární otevřené má přístup pro zápis stavu a tedy obecně službu je při provádění skutečná práce. `RunAsync` Metoda stavová služba je spuštěn pouze v případě otevřené stavová služba je primární. `RunAsync` Způsob je zrušena, jakmile se změní role primární otevřené od primární i během události zavřít a přerušení. 

Použití `OnChangeRoleAsync` události umožňuje provádět práce v závislosti na otevřené role i v roli změny.

Stavová služba také poskytuje události jako služby příslušnosti se stejnou sémantiku a případy použití životnosti čtyři:

- `Task OnOpenAsync(IStatefulServicePartition, CancellationToken)`
- `Task OnCloseAsync(CancellationToken)`
- `void OnAbort()`



## <a name="next-steps"></a>Další kroky
Další rozšířené témat týkajících se struktury služby najdete v následujících článcích:

- [Konfigurace stavové spolehlivé služby](service-fabric-reliable-services-configuration.md)

- [Úvod stavu služby struktury](service-fabric-health-introduction.md)

- [Pomocí sestav stavu systému pro řešení potíží](service-fabric-understand-and-troubleshoot-with-system-health-reports.md)

- [Konfigurace služby s struktury clusteru zdroje Správce služeb](service-fabric-cluster-resource-manager-configure-services.md)
