<properties
   pageTitle="Obnovení havárie Azure služby struktury | Microsoft Azure"
   description="Azure struktury služby nabízí možnosti nutné zabývat se všemi typy havárie. Tento článek popisuje typy havárie, které se mohou vyskytnout a jak zacházet s nimi."
   services="service-fabric"
   documentationCenter=".net"
   authors="seanmck"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotNet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="08/10/2016"
   ms.author="seanmck"/>

# <a name="disaster-recovery-in-azure-service-fabric"></a>Obnovení havárie struktury služby Azure

Důležitou součástí předvádění aplikace vysoké dostupnosti cloudu zajistit, že ho zachován všechny různé typy chyby, včetně těch, které nejsou z ovládacího prvku. Tento článek popisuje fyzické rozložení struktury služby Azure obrázku v kontextu potenciální havárie a obsahuje pokyny, jak řešit takové havárie k omezení nebo vyloučení rizika prostoje nebo ztrátu dat.

## <a name="physical-layout-of-service-fabric-clusters-in-azure"></a>Pole fyzicky rozložení služby struktury clusterů v Azure

Abyste pochopili riziko způsobené různé typy potíží, je vhodné vědět, jak clusterů fyzicky rozloženy v Azure.

Když vytvoříte služby struktury cluster v Azure, je nutné vybrat oblast, kde hostovaný. Azure infrastruktury pak ustanovení zdroji pro daný cluster v oblasti, zejména počet virtuálních počítačích (VMs) požaduje. Podívejme se podrobněji na jak a kam se tyto VMs zřízení.

### <a name="fault-domains"></a>Poruchy domén

Ve výchozím nastavení jsou VMs clusteru rovnoměrně rozšířit mezi logických skupinách jmenoval poruch domény (FDs), které segmentech počítačích podle možné chyby v hardwaru Host (hostitel). Konkrétně Pokud jsou dvě VMs umístěny ve dvou různých FDs, chcete mít jistotu, že budou Nesdílet stejné zdroje nebo síti vypínač. V důsledku toho místní síti nebo dodávky elektrického proudu došlo k ovlivnění jeden OM nemají vliv na jinou, povolení služby struktury pracovního vytížení odpovídat počítači v rámci clusteru.

Vizualizace rozložení svůj cluster v doménách poruchy pomocí funkce Mapa clusteru uvedenou v [Průzkumníkovi struktury služby](service-fabric-visualizing-your-cluster.md):

![Uzly rozšířit mezi poruch domén v Průzkumníkovi struktury služby][sfx-cluster-map]

>[AZURE.NOTE] Další osy v mapě clusteru zobrazuje upgradu domény které logicky uzly podle aktivity plánované údržby skupin. Clusterů struktury služby Azure vždy rozloženy přes pět upgradu domény.

### <a name="geographic-distribution"></a>Zeměpisná rozdělení.

Jsou aktuálně [26 Azure oblasti po celém světě][azure-regions], s několika více ohlásí. Jednotlivé oblast může obsahovat jedno nebo více fyzické datacentrech v závislosti na vyžádání a dostupnosti vhodných místech, kromě jiných faktorů. Osobní zprávu, ale, že i v oblasti, které obsahují více fyzické datacentrech je zaručit, že svůj cluster VMs bude rovnoměrně rozšířit mezi těmito fyzické umístění. Nakonec v současné době všechny VMs pro dané clusteru jsou k dispozici v rámci jednoho pole fyzicky webu.

## <a name="dealing-with-failures"></a>Práce s chybami

Existuje několik typů chyby, ke kterým může být ovlivněné svůj cluster, oba objekty mají vlastní omezení rizik. Podíváme se na jejich pořadí pravděpodobnost problémy.

### <a name="individual-machine-failures"></a>Jednotlivé počítače k chybám

Jak je uvedeno, jednotlivé počítače k chybám v rámci OM nebo v hardwaru nebo hostitelské v doméně poruch představovat žádné riziko na vlastní. Služba struktury se obvykle rozpoznat selhání vyvolané zpráv a odpovídání příslušným způsobem podle stavu clusteru. Například pokud uzel byl hostitelem primární repliky oddílem, nové primární volbou z sekundární repliky oddílu. Když Azure vyvolá selhalo počítače zpět, bude automaticky znovu připojit ke clusteru a opět převzít její sdílení pracovní zátěž.

### <a name="multiple-concurrent-machine-failures"></a>Více k chybám souběžné počítače

Během poruch domény výrazně snížit riziko selhání souběžné počítače, je vždy možné riziko více náhodná selhání na několika počítačích v clusteru snížilo současně.

Obecně dokud většinou uzly zůstávají k dispozici, clusteru zůstanou pracovat, i když v dolním postavení stavové repliky získat zabalit do něco míň strojů a méně příslušnosti instance jsou k dispozici roztažením načíst.

#### <a name="quorum-loss"></a>Ztráty kvora

Pokud většinou repliky stavová služba oddílu přejděte, tento oddíl vloží její stav jmenoval "ztráty kvora." V tomto okamžiku služby struktury se zastaví povolení zápisy na tento oddíl zajistit, aby zůstal stavu konzistentní a spolehlivé. Ve skutečnosti jsme výběru přijměte období nedostupnost zajistit oznámí klientů nejsou jde, že data uložený ve skutečnosti době, kdy není. Poznámka: Pokud jste se přihlásily k povolení čtení z sekundární repliky pro stavové službu, můžete nadále provádět můžou být operací v tomto režimu čtení. Oddíl zůstane v ztráty kvora dokud dostatečný počet repliky vrátit nebo Správce clusteru vynutí systému k přesunutí na pomocí rozhraní [API opravit ServiceFabricPartition][repair-partition-ps].

>[AZURE.WARNING] Provedení akce opravit při primární otevřené dolů dojde ke ztrátě dat.

Systémové služby také může dojít ztráty kvora působivé jsou specifické pro příslušné služby. Například ztráty kvora ve službě naming bude mít vliv na překlad, že ztráty kvora ve službě Správce převzetí zablokuje vytvoření nové služby a převzetí služeb při selhání. Všimněte si, že na rozdíl od vlastní služby pokusíte opravit systémové služby *nedoporučuje* . Místo toho je výhodnější jednoduše Počkejte, až vrátí dolů repliky.

#### <a name="minimizing-the-risk-of-quorum-loss"></a>Minimalizace rizika ztráty kvora

Minimalizujte riziko kvora ztráty zvětšením cílové otevřené sadu službě. Je vhodné přemýšlet o počtu replik potřebujete z hlediska počtu není k dispozici uzlů můžete nevadí na vám po zůstává k dispozici pro zápis, uchovávání v buďte aplikace nebo obrázku upgrady udělat uzly dočasně nedostupný kromě selhání hardwaru.

Příklady za předpokladu, že máte nastavený služeb mít MinReplicaSetSize ze tří minimální hodnotu doporučené služby výroby. S TargetReplicaSetSize ze tří (primární a dvě druhotné), selhání hardwaru během upgradu (dvě repliky dolů) dojde ke ztrátě kvora a službě nepůjde jen pro čtení. Můžete taky máte pět repliky by budete moci nebyly dvou chyby během upgradu (tři replik dolů) jako zbývajících dvou replikách pořád formulář hlasování v rámci sady minimální otevřené.

### <a name="data-center-outages-or-destruction"></a>Datové centrum výpadků nebo zničení

V některých případech může být někdy fyzické datacentrech dočasně nedostupný z důvodu ztrátě připojení k power nebo k síti. V těchto případech clusterů služby struktury a aplikací Podobně nebudou dostupní, ale zachová data. V rámci clusterů spuštěné v Azure můžete zobrazit aktualizace na výpadků na [stránce Azure stav][azure-status-dashboard].

V případě velmi pravděpodobné, že je odstraněna celé pole fyzicky datacentrem všechny služby struktury clusterů hostované tam se ztratí, spolu s jejich stavu.

K ochraně proti tuto možnost, je ale zásadně důležité pravidelně [Zálohovat váš stav](service-fabric-reliable-services-backup-restore.md) geo nadbytečné Store a ujistěte se, že ověření možnost obnovit. Jak často vytvořením zálohy budou závislý na vaším cílem bodu obnovení (operace RPO). I nemáte-li plně zálohování a obnovení ještě, měli byste je používat obslužné rutiny `OnDataLoss` událost tak, aby se můžete přihlásit dojde jako následuje:

```c#
protected virtual Task<bool> OnDataLoss(CancellationToken cancellationToken)
{
  ServiceEventSource.Current.ServiceMessage(this, "OnDataLoss event received.");
  return Task.FromResult(true);
}
```


### <a name="software-failures-and-other-sources-of-data-loss"></a>Software selhání a jiných zdrojů ztrátu dat

Příčinou ztrátou dat jsou nejčastěji používaných než rozšířených dat centra selhání vady kód služby, lidské provozní chyby a narušením zabezpečení. Ve všech případech strategii obnovení je však stejné: prohlédněte pravidelného zálohování všech stavové služeb a neuplatnění možnost obnovit tento stav.

## <a name="next-steps"></a>Další kroky

- Zjistěte, jak tak, aby napodobily různých chyby pomocí [testování framework](service-fabric-testability-overview.md)
- Přečtěte si další havárie využití a vysoké dostupnosti zdroje. Microsoft publikovala velké množství pokyny v těchto tématech. Během některé tyto dokumenty odkaz na určité technik pro použití v ostatních produktů, obsahují mnoho obecné doporučené postupy, které lze použít v rámci služby struktury:
 - [Kontrolní seznam dostupnosti](../best-practices-availability-checklist.md)
 - [Obnovení podrobností havárie](../sql-database/sql-database-disaster-recovery-drills.md)
 - [Obnovení havárie a dostupnost pro Azure aplikace][dr-ha-guide]


<!-- External links -->

[repair-partition-ps]: https://msdn.microsoft.com/library/mt163522.aspx
[azure-status-dashboard]:https://azure.microsoft.com/status/
[azure-regions]: https://azure.microsoft.com/regions/
[dr-ha-guide]: https://msdn.microsoft.com/library/azure/dn251004.aspx


<!-- Images -->

[sfx-cluster-map]: ./media/service-fabric-disaster-recovery/sfx-clustermap.png
