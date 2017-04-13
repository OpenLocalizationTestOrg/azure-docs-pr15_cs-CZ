<properties
   pageTitle="Testování: Služby komunikace | Microsoft Azure"
   description="Komunikace služby služby představuje bod kritické integrace služby struktury aplikace. Tento článek popisuje navrhování a testování postupy."
   services="service-fabric"
   documentationCenter=".net"
   authors="vturecek"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="07/06/2016"
   ms.author="vturecek"/>

# <a name="service-fabric-testability-scenarios-service-communication"></a>Scénáře služby struktury testování: služby komunikace

Microservices a orientovaných na služby architektonické styly povrch přirozeným v struktury služby Azure. V těchto typech distribuované architektury komponentních microservice aplikace jsou obvykle skládající se ze více služeb, které je potřeba vzájemné komunikaci. V i nejjednodušší případech obecně mít aspoň příslušnosti webové služby a služba úložiště stavové dat, které je třeba komunikovat.

Komunikace služby služby představuje bod kritické integrace aplikace, protože každé služby zpřístupňuje vzdálené rozhraní API pro další služby. Práce s sada rozhraní API omezení, která zahrnuje vstupu a výstupu obecně vyžaduje některé péče s dobré počtu testování a ověření.

Existují spoustu informace týkající se při těchto omezení služeb jsou společná drátové distribuované systému:

 - *Transport Protocol (protokol)*. Budete používat HTTP pro lepší interoperability nebo vlastní binární protokol pro maximální výkon?
 - *Zpracování chyb*. Jak se bude trvalý a přechodná chyb přistupovat? Co se stane, když službu přesune do jiného uzlu?
 - *Časové limity a latence*. V aplikacích možných jak bude každé vrstvy služby zpracovat latence procházení skupiny a pro uživatele?

Jestli použijete některý z předdefinovaných služby komunikační součásti poskytovaných služeb struktury nebo vytvořit vlastní, testování interakce mezi vašich služeb, je naprosto zásadní zajištění odolnost proti chybám v aplikaci.

## <a name="prepare-for-services-to-move"></a>Příprava na přesunout služeb

Instancí služby může pohyb po určitou dobu. To platí zejména pokud budou nakonfigurována zatížení metriky pro vyrovnávání vlastní přizpůsobený optimální zdroje. Služba struktury přesune instancí služby zobrazíte jejich dostupnost i během upgradů, převzetí služeb při selhání, škálování a jiných situacích probíhajících přes životnost distribuovaného systému.

Jak služby pohyb v clusteru, klienty a dalších služeb máme ještě počítat pracovat s dvěma scénáře při mluvit do služby:

- Služba instance nebo oddílu otevřené přesunul od posledního kontakt do ní. Toto je normální součástí životního cyklu služby a je třeba očekávat, stane po dobu platnosti aplikace.
- Služba instance nebo oddílu otevřené je přesouvány. I když v služby struktury, dojde k velmi rychle selhání jeden uzel služby, může existovat zpoždění v dostupnosti při pomalé zahájíte komunikace součástí služby.

Zpracování podobnému sledu řádně je důležitých hladkého spuštění systému. Postup platit toto:

- Každé služby, které jde připojit k má *adresu* , která sleduje na (například HTTP nebo WebSockets). Přesun instance služby nebo oddílu, změní se jeho adresa koncového bodu. (Slouží k přesunutí k jiné uzel s jinou adresou IP.) Pokud používáte předdefinované komunikační součásti, jsou pro vás zpracuje adres opětovně řešení služby.
- Doména může obsahovat dočasné zvýšení latence služby jako spuštěním instancí služby si jeho posluchače znovu. To záleží na tom, jak rychle službu otevře posluchače po přesunutí instance služby.
- Existující připojení muset zavřít a znovu otevřít po službu, otevře se na nový uzel. Vypnutí bezproblémové uzel nebo restart umožňuje dobu existující připojení se neukončí.

### <a name="test-it-move-service-instances"></a>Vyzkoušejte: Přesunutí instancí služby

Pomocí nástrojů služeb struktury testování vy vytváříte scénáři test testování těchto situací různými způsoby:

1. Přesunutí stavová služba primární otevřené.

    Primární otevřené oddíl stavová služba přesouvat pro libovolný počet důvodů. Slouží k obrázku primární otevřené konkrétní oddíl zobrazíte, jak služeb reagovat na cestách velmi řízené způsobem.

    ```powershell

    PS > Move-ServiceFabricPrimaryReplica -PartitionId 6faa4ffa-521a-44e9-8351-dfca0f7e0466 -ServiceName fabric:/MyApplication/MyService

    ```

2. Vypnout uzel.

    Po zastavení uzel služby struktury slouží k přesunutí všech instancí služby nebo oddílů, které byly v daném uzlu na jeden z dalších uzly k dispozici v clusteru. Slouží k testování situaci, kdy dojde ke ztrátě ze svého obrázku a všechny instance služby uzel a repliky na uzel potřeba přesunout.

    Ukončení uzel pomocí rutiny prostředí PowerShell **Zastavit ServiceFabricNode** :

    ```powershell

    PS > Restart-ServiceFabricNode -NodeName Node_1

    ```

## <a name="maintain-service-availability"></a>Udržovat dostupnost služby

Jako platformy struktury služby umožňuje dostupnost svých služeb. Ale v krajní případech může způsobit problémy související infrastruktury pořád nedostupnost. Je důležité k testování podobnému sledu příliš.

Stavová služba umožňuje systémem kvora replikovat stavem dostupnost. To znamená, že hlasování replik musí být k dispozici provádět operace zápisu. Hlasování replik výjimečně, například k neúspěšnému rozšířených hardware nemusí být dostupná. V těchto případech nebude možné provádět operace zápisu, ale pořád moct provádět operace čtení.

### <a name="test-it-write-operation-unavailability"></a>Vyzkoušejte: psaní nedostupnost operace

Pomocí nástroje testování do struktury služby vložíte chybu indukuje ztráty kvora jako test. Sice méně častých tato situace, je důležité, klienty a služby, které jsou závislé na stavová služba jsou připravené zvládnout situace, kdy nelze provádění zapisovat žádosti o. Také je důležité, stavová služba sama známa tuto možnost a řádně komunikovat ho pro volající.

Pomocí rutiny prostředí PowerShell **Vyvolat ServiceFabricPartitionQuorumLoss** může způsobit ztráty kvora:

```powershell

PS > Invoke-ServiceFabricPartitionQuorumLoss -ServiceName fabric:/Myapplication/MyService -QuorumLossMode QuorumReplicas -QuorumLossDurationInSeconds 20

```

V tomto příkladu jsme nastavit `QuorumLossMode` k `QuorumReplicas` označíte, že chceme vyvolat ztráty kvora přitom dolů ostatními. Tímto způsobem operace čtení je možné, že. Testování scénáři, kde je k dispozici celý oddíl, můžete nastavit tento přepínač, aby `AllReplicas`.

## <a name="next-steps"></a>Další kroky

[Další informace o testování akce](service-fabric-testability-actions.md)

[Další informace o testování scénáře](service-fabric-testability-scenarios.md)
