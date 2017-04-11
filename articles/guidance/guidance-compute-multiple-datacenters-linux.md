<properties
   pageTitle="Spouštění Linux virtuálních ve více oblastech vysoké dostupnosti | Vytvořte odkaz architektura | Microsoft Azure"
   description="Postup pro nasazení VMs ve více oblastech na Azure dostupnost a odolnost proti chybám."
   services=""
   documentationCenter="na"
   authors="MikeWasson"
   manager="roshar"
   editor=""
   tags=""/>

<tags
   ms.service="guidance"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/21/2016"
   ms.author="mwasson"/>

# <a name="running-linux-vms-in-multiple-regions-for-high-availability"></a>Spouštění Linux virtuálních ve více oblastech vysoké dostupnosti

[AZURE.INCLUDE [pnp-header](../../includes/guidance-pnp-header-include.md)]

> [AZURE.SELECTOR]
- [Spouštění Linux virtuálních ve více oblastech vysoké dostupnosti](guidance-compute-multiple-datacenters-linux.md)
- [Spouštění Windows virtuálních ve více oblastech vysoké dostupnosti](guidance-compute-multiple-datacenters.md)

V tomto článku doporučujeme sadu postupy v několika Azure oblastí, dosáhnout dostupnost a obnovení infrastrukturu robustní havárie programovacím Linux virtuálních počítačích (VMs).

> [AZURE.NOTE] Azure obsahuje dva různé nasazení modely: [Správce prostředků] [ resource groups] a klasické. Tento článek pomocí Správce zdroje, který Microsoft doporučuje nové nasazení.

Architektura více oblastech může poskytovat vyšší dostupnost než nasadíte pro jednu oblast. Pokud výpadku místní blokování automaticky otevíraných primární oblasti, můžete použít [Správce dopravy] [ traffic-manager] přenesou do oblasti sekundární. Tato architektura můžou pomoct taky selže podsystému jednotlivé aplikace.

K dosažení dostupnost napříč datacentrech několik obecné způsoby:   
  
- Aktivní/pasivní s režimu. Přenosy přejde na jednom regionu při další čeká v úsporném režimu. VMs v oblasti sekundární se přidělené pracovního vždy.

- Aktivní/pasivní s studenou úsporném režimu. Stejné, ale VMs v oblasti sekundární nejsou přidělit až potřebné pro překlopení. Tento přístup náklady menší spustíte, ale bude obecně již dolů během se nepovede.

- Aktivní/aktivní. Obě oblasti jsou aktivní a požadavky rozloženy mezi nimi. Pokud jeden datovém centru nebude k dispozici, se považuje mimo otočení.

Tato architektura se zaměřuje na aktivní/pasivní s žádanou úsporném režimu správce přenosy pro překlopení. Všimněte si, že můžete nasadit malým počtem poštovních VMs pro režimu a potom rozšiřování podle potřeby.

## <a name="architecture-diagram"></a>Diagram architektury

Na následujícím obrázku je založena na architektura v [Přidání spolehlivost N-vrstvy architektury na Azure](guidance-compute-n-tier-vm-linux.md). 

> Dokument aplikace Visio, který obsahuje tento diagram architektury je možné stáhnout z [webu služby Stažení softwaru][visio-download]. Tento diagram je v oblasti (Linux) vícestránkové "výpočetním.

![[0]][0]

- **Primární a sekundární oblastí**. Tato architektura používá k dosažení vyšší dostupnost dvou oblastí. Reprodukujte primární oblast. Běžného provozu v síti směrován k oblasti primární. Ale pokud, nebude k dispozici, budou návštěvníci směrován k oblasti sekundární.

- ** [Azure přenosy správce] [ traffic-manager] ** přesměrovává příchozí žádosti na primární oblast. Pokud dané oblasti nebude k dispozici, správce přenosy selhání na vedlejší oblast. Další informace naleznete v části [Konfigurace přenosy správce](#configuring-traffic-manager).

- **Skupiny zdrojů**. Vytvoření samostatného [skupiny zdrojů] [ resource groups] oblasti primární oblasti sekundární pro přenos správce a. To vám umožní spravovat každou oblast jako jediná kolekce zdrojů. Například může přeinstalujte jednom regionu přitom dolů jinou. [Odkaz na skupiny zdrojů][resource-group-links]tak, aby spustíte dotaz na seznam zdrojů pro aplikaci.

- **VNets**. Vytvoření samostatného VNet pro každou oblast. Zkontrolujte, že adresa mezery nepřekrývaly.

- **Apache Cassandra** nasazené dat centra přes Azure oblastí. Cassandra datacentrech zavedení v různých oblastech Azure vysoké dostupnosti. V jednotlivých oblastech uzly nakonfigurováno v kompatibilních regálů režimu s poruch a upgrade domény odolnosti uvnitř oblasti.

## <a name="recommendations"></a>Doporučení

Azure nabízí mnoho různých zdrojů a typů zdrojů, takže může být tento odkaz architektura zřízení mnoha různými způsoby. Správce prostředků Azure šablonu nainstalovat architektura odkaz, který následuje těmito doporučeními uvádíme. Pokud budete chtít vytvořit vlastní odkaz architektura těmito doporučeními řídit Pokud požadujete konkrétní doporučení příliš velký.

### <a name="regional-pairing"></a>Místní párování

Každou oblast Azure je spárované s jiné oblasti v rámci stejné zeměpisná oblast. Obecně vyberte oblastí stejné místní pár (například východoasijských USA 2 a nám centrální). Výhody tím patří:

- Pokud existuje obecných výpadku, obnovení alespoň jeden oblasti mimo každou dvojici prioritu.

- Aktualizace plánované Azure systému rozšiřují párových oblasti postupně, na možné prostoje.

- Dvojice nacházejí v rámci stejné zeměpisných dat sídlo požadavkům.

Přesvědčte, že obě oblasti podporují všechny potřebné pro aplikaci služby Azure (viz [služby podle regionů][services-by-region]). Další informace o místní dvojice najdete v tématu [firmy kontinuitu a havárie obnovení (BCDR): Azure párovaný oblastí][regional-pairs].

### <a name="traffic-manager-configuration"></a>Konfigurace přenosy správce

Zvažte následující možnosti při konfiguraci přenosy správce pro nefunguje:

- **Směrování.** Přenosy správce podporuje několik [algoritmy směrování][tm-routing]. Scénář popisované v tomto článku využívat _Priorita_ směrování (dříve označovaného jako _převzetí_ směrování). S toto nastavení používají, přenosy správce rozešle všechny žádosti o oblasti primární Pokud oblasti primární bude dostupný. V tomto okamžiku automaticky dojde k chybě myší na vedlejší oblast. Najdete v článku [směrování metody konfigurace převzetí][tm-configure-failover].

- **Zkušební stavu.** Používá správce přenosy protokolu HTTP (nebo HTTPS) [zkušební] [ tm-monitoring] sledovat dostupnost každou oblast. Zkušební hledá odpověď HTTP 200 pro zadanou adresu URL. Osvědčený vytvořte koncového sestavy celkový stav aplikace a použít tento koncový bod pro zkušební stavu. V opačném zkušební "pořádku" koncový bod při hlásit selhává skutečně důležité část aplikace. Další informace najdete v tématu [Vzorek monitorování stavu koncový bod][health-endpoint-monitoring-pattern].

Při selhání přenosy Manager je určité době, kdy klientů nelze kontaktovat aplikace, což může být několik minut. Dva faktory vliv na celkovou dobou trvání:

- Zkušební stav musíte zjistit primární datovém centru stal nedostupný.

- Servery DNS musíte aktualizovat záznamy z mezipaměti DNS na IP adresu, která závisí DNS time to live (TTL). Výchozí hodnota TTL je 300 sekund (5 minut), ale můžete nakonfigurovat tuto hodnotu při vytváření profilu přenosy správce.

Další informace najdete v tématu [O sledování přenosy správce][tm-monitoring].

Pokud správce přenosy selhání, doporučujeme provedení ruční navrácení, spíše než automaticky selhání zpět. Ověřte, zda všechny aplikace podsystémy správný nejdřív. Jinak můžete vytvořit situaci, kdy aplikace překlápět sebou poslalo mezi datacentrech.

Ve výchozím nastavení přenosy správce automaticky nemůže zpět. Nechcete, aby toho ručně nižší priorita primární oblasti po převzetí události. Předpokládejme například, oblasti primární je priorita záznamů 1 a sekundární je priorita záznamů 2. Po převzetí nastavte primární oblast Priorita 3, kterou chcete zabránit tomu, aby automatické. Až budete chtít přepnout zpátky, aktualizujte priority 1.

Příkaz rozhraní příkazového řádku Azure aktualizuje priority:

```bat
azure network traffic-manager  endpoint set --resource-group <resource-group> --profile-name <profile>
    --name <traffic-manager-name> --type AzureEndpoints --priority 3
```    

Další způsob, jak zabránit klopný je dočasném vypnutí na koncový bod:

```bat
azure network traffic-manager  endpoint set --resource-group <resource-group> --profile-name <profile>
    --name <traffic-manager-name> --type AzureEndpoints --status Disabled
```    

V závislosti na příčinou selhání můžete zkusit redploy zdrojů v oblasti. Než zpět, proveďte test provozní připravenosti. Test ověřte, například:

- VMs jsou správně nakonfigurované. (Všechny požadované software nainstalovaný, IIS spuštěná, atd.)

- Aplikace podsystémy jsou funkční.

- Testování funkční. (Například osy databáze je k nim přistupovat z webová vrstva.)

### <a name="cassandra-deployment-across-multiple-regions"></a>Nasazení Cassandra napříč několika oblastí

Cassandra datacentrech jsou dělení pracovního vytížení: skupiny související uzlů nakonfigurované pohromadě v rámci obrázku pro replikace a pracovní zátěž oddělení.

Doporučujeme [DataStax Enterprise] [ datastax] dosáhnete výroby. Další informace o spuštění DataStax v Azure v tématu [Průvodce nasazením DataStax Enterprise pro Azure][cassandra-in-azure]. Následující doporučení Obecné platí pro všechny Cassandra edition.

- Přiřaďte veřejnou IP adresu jednotlivých uzlech. Díky seskupení komunikaci v oblastech pomocí infrastruktury Azure páteřní poskytování vysoce výkonných platbou nízké.

- Zabezpečené uzlů pomocí příslušné brány firewall a konfigurace NSG přenos pouze a od známé hostitelů, včetně klientů a uzlů clusteru. Všimněte si, že Cassandra pomocí různých porty komunikace, OpsCenter, Spark a tak dále. Použití portu v Cassandra, najdete v článku [Konfigurace brány firewall port přístup][cassandra-ports].

- Šifrování SSL pro všechny [Uzel Klient] [ ssl-client-node] a [mezi uzly] [ ssl-node-node] komunikace.

- V oblasti postupujte podle pokynů v [Cassandra doporučení](guidance-compute-n-tier-vm-linux.md#cassandra-recommendations).

## <a name="availability-considerations"></a>Důležité informace o dostupnosti

Komplexní aplikaci N-osy a nemusí potřebujete replikovat celou aplikaci v oblasti sekundární. Můžete místo toho jenom replikovat kritické subsystémem, který je potřeba k podpoře nepřerušený.

Správce dopravy představuje bod možné selhání systému. Když službu nepovede, klienti nemají přístup k aplikaci během prostoje. Kontrola [Přenosy správce SLA][tm-sla]a zjistit, zda správce přenosy samotný splňuje vaše obchodní požadavky vysoké dostupnosti. Pokud ne, můžete do ní přidat další řešení pro správu přenosy jako navrácení. Pokud službu Azure přenosy správce nepovede, změňte záznamy CNAME DNS tak, aby ukazovaly na jiných přenosy službu pro správu. (Tento krok musíte provést ruční a aplikace nebude k dispozici, dokud změn DNS se rozšíří.)

U obrázku Cassandra převzetí scénářů k zvážení závisí na úrovně konzistence používané aplikace, jakož i počtu replik použít. Úrovně konzistenci a použití v Cassandra najdete v tématu [konzistence dat konfigurace] [ cassandra-consistency] a [Cassandra: kolik uzlů jsou kontakt k s kvora?][cassandra-consistency-usage] Dostupnost dat v Cassandra je určený podle úroveň konzistence používané aplikace a replikační mechanismus. Replikace v Cassandra, najdete v článku [Replikace dat v NoSQL databázích vysvětlení][cassandra-replication].

## <a name="manageability-considerations"></a>Důležité informace o možnostech správy

Když aktualizujete nasazení aktualizujte oblastí najednou, snížit šanci selhání globální z nesprávná konfigurace nebo chybu v aplikaci.

Otestujte odolnost proti chybám systému k chybám. Tady jsou některé běžné scénáře selhání vyzkoušet:

- Ukončete OM instance.

- Tlaku zdroje, jako jsou procesoru a paměti.

- Odpojení/zpoždění síť.

- Docházet k chybám procesů.

- Platnost certifikáty.

- Simulovat chyby hardwaru.

- Vypnutí služby DNS na řadiče domény.

Změřte časy obnovení a ověřte, jestli že nevyhovují vašim požadavkům pro firmy. Testování kombinace režimů selhání, stejně.

## <a name="next-steps"></a>Další kroky

Tato řada obsahuje zaměřit na čistého cloudu nasazení. Pole organizace často nevyžadují hybridní síťové připojení v místní síti s Azure virtuální sítě. Naučte se vytvořit hybridní síť, najdete v článku [provádění hybridní architektura sítě s Azure a místních VPN][hybrid-vpn].

<!-- Links -->

[azure-sla]: https://azure.microsoft.com/support/legal/sla/
[cassandra-in-azure]: https://academy.datastax.com/resources/deployment-guide-azure
[cassandra-consistency]: http://docs.datastax.com/en/cassandra/2.0/cassandra/dml/dml_config_consistency_c.html
[cassandra-replication]: http://www.planetcassandra.org/data-replication-in-nosql-databases-explained/
[cassandra-consistency-usage]: https://medium.com/@foundev/cassandra-how-many-nodes-are-talked-to-with-quorum-also-should-i-use-it-98074e75d7d5#.b4pb4alb2
[cassandra-ports]: http://docs.datastax.com/en/latest-dse/datastax_enterprise/sec/secConfFirePort.html
[datastax]: https://www.datastax.com/products/datastax-enterprise
[health-endpoint-monitoring-pattern]: https://msdn.microsoft.com/library/dn589789.aspx
[hybrid-vpn]: guidance-hybrid-network-vpn.md
[regional-pairs]: ../best-practices-availability-paired-regions.md
[resource groups]: ../azure-resource-manager/resource-group-overview.md
[resource-group-links]: ../resource-group-link-resources.md
[services-by-region]: https://azure.microsoft.com/en-us/regions/#services
[ssl-client-node]: http://docs.datastax.com/en/cassandra/2.0/cassandra/security/secureSSLClientToNode_t.html
[ssl-node-node]: http://docs.datastax.com/en/cassandra/2.0/cassandra/security/secureSSLNodeToNode_t.html
[tablediff]: https://msdn.microsoft.com/en-us/library/ms162843.aspx
[tm-configure-failover]: ../traffic-manager/traffic-manager-configure-failover-routing-method.md
[tm-monitoring]: ../traffic-manager/traffic-manager-monitoring.md
[tm-routing]: ../traffic-manager/traffic-manager-routing-methods.md
[tm-sla]: https://azure.microsoft.com/en-us/support/legal/sla/traffic-manager/v1_0/
[traffic-manager]: https://azure.microsoft.com/en-us/services/traffic-manager/
[visio-download]: http://download.microsoft.com/download/1/5/6/1569703C-0A82-4A9C-8334-F13D0DF2F472/RAs.vsdx
[vnet-dns]: ../virtual-network/virtual-networks-manage-dns-in-vnet.md
[vnet-to-vnet]: ../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md
[vpn-gateway]: ../vpn-gateway/vpn-gateway-about-vpngateways.md
[wsfc]: https://msdn.microsoft.com/en-us/library/hh270278.aspx
[0]: ./media/blueprints/compute-multi-dc-linux.png "Architektura sítě vysoce k dispozici pro aplikace Azure N-osy"
