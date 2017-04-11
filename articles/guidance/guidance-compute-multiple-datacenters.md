<properties
   pageTitle="Spouštění Windows virtuálních ve více oblastech vysoké dostupnosti | Vytvořte odkaz architektura | Microsoft Azure"
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

# <a name="running-windows-vms-in-multiple-regions-for-high-availability"></a>Spouštění Windows virtuálních ve více oblastech vysoké dostupnosti

[AZURE.INCLUDE [pnp-header](../../includes/guidance-pnp-header-include.md)]

> [AZURE.SELECTOR]
- [Spouštění Linux virtuálních ve více oblastech vysoké dostupnosti](guidance-compute-multiple-datacenters-linux.md)
- [Spouštění Windows virtuálních ve více oblastech vysoké dostupnosti](guidance-compute-multiple-datacenters.md)

V tomto článku doporučujeme sadu postupy v několika Azure oblastí, dosáhnout dostupnost a obnovení infrastrukturu robustní havárie programovacím virtuálních počítačích Windows (VMs).

> [AZURE.NOTE] Azure obsahuje dva různé nasazení modely: [Správce prostředků] [ resource groups] a klasické. Tento článek používá správce zdroje, který Microsoft doporučuje nové nasazení.

Architektura více oblastech může poskytovat vyšší dostupnost než nasadíte pro jednu oblast. Pokud výpadku místní blokování automaticky otevíraných primární oblasti, můžete použít [Správce přenosy] [ traffic-manager] přenesou do oblasti sekundární. Tato architektura můžou pomoct taky selže podsystému jednotlivé aplikace. 

K dosažení dostupnost přes datacentrech několik obecné způsoby:

- Aktivní/pasivní s režimu. Přenosy přejde na jednom regionu při další čeká v úsporném režimu. VMs v oblasti sekundárním se přidělené pracovního vždy.

- Aktivní/pasivní s studenou úsporném režimu. Stejné, ale VMs v oblasti sekundární nejsou přidělit až potřebné pro překlopení. Tento přístup náklady menší spustíte, ale bude obecně již dolů během se nepovede.

- Aktivní/aktivní. Obě oblasti jsou aktivní a požadavky rozloženy mezi nimi. Pokud jeden datovém centru nebude k dispozici, se považuje mimo otočení. 

Tato architektura se zaměřuje na aktivní/pasivní s žádanou úsporném režimu správce přenosy pro překlopení. Všimněte si, že můžete nasadit malým počtem poštovních VMs pro režimu a potom rozšiřování podle potřeby.

## <a name="architecture-diagram"></a>Diagram architektury

Na následujícím obrázku je založena na architektura v [Přidání spolehlivost N-vrstvy architektury na Azure](guidance-compute-n-tier-vm.md).

> Dokument aplikace Visio, který obsahuje tento diagram architektury je možné stáhnout z [webu služby Stažení softwaru][visio-download]. Tento diagram je v oblasti (Windows) vícestránkové "výpočetním.

[! [0]][0]

- **Primární a sekundární oblastí**. Tato architektura používá k dosažení vyšší dostupnost dvou oblastí. Reprodukujte primární oblast. Běžného provozu v síti směrován k oblasti primární. Ale pokud, nebude k dispozici, přenosy směrován k oblasti sekundární.

- ** [Azure přenosy správce] [ traffic-manager] ** přesměrovává příchozí žádosti na primární oblast. Pokud dané oblasti nebude k dispozici, správce přenosy selhání na vedlejší oblast. Další informace naleznete v části [Konfigurace přenosy správce](#configuring-traffic-manager).

- **Skupiny zdrojů**. Vytvoření samostatného [skupiny zdrojů] [ resource groups] oblasti primární oblasti sekundární pro přenos správce a. To vám umožní spravovat každou oblast jako jediná kolekce zdrojů. Například může přeinstalujte jednom regionu přitom dolů jinou. [Odkaz na skupiny zdrojů][resource-group-links]tak, aby spustíte dotaz na seznam zdrojů pro aplikaci.

- **VNets**. Vytvoření samostatného VNet pro každou oblast. Zkontrolujte, že adresa mezery nepřekrývaly. 

- **SQL Server vždy na dostupnost skupiny**. Pokud používáte SQL serveru, doporučujeme [SQL vždy na Availabilty skupiny] [ sql-always-on] vysoké dostupnosti. Vytvořte skupinu jednoho dostupnost, která obsahuje instance serveru SQL Server v obou oblastí. Další informace naleznete v části [Konfigurace dostupnosti skupině SQL Server vždy na](#configuring-the-sql-server-alwayson-availability-group ).

> [AZURE.NOTE] Zvažte také [Databáze SQL Azure][azure-sql-db], která poskytuje relační databáze jako do cloudové služby. S databáze SQL nemusíte Konfigurace dostupnosti skupiny nebo spravovat překlopení.  

- **VPN bran**: vytvoření [Brána VPN] [ vpn-gateway] v jednotlivých VNet a konfigurace [připojení VNet VNet][vnet-to-vnet], chcete-li povolit síťová komunikace mezi dvěma VNets. Toto je nutný pro skupiny dostupnosti SQL vždy na.

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

- **Směrování.** Přenosy správce podporuje několik [algoritmy směrování][tm-routing]. Scénář popisované v tomto článku využívat _Priorita_ směrování (dříve označovaného jako _převzetí_ směrování). S toto nastavení používají, přenosy správce rozešle všechny žádosti o primární oblasti, pokud oblasti primární bude dostupný. V tomto okamžiku automaticky dojde k chybě myší na vedlejší oblast. Najdete v článku [směrování metody konfigurace převzetí][tm-configure-failover].

- **Zkušební stavu.** Používá správce přenosy protokolu HTTP (nebo HTTPS) [zkušební] [ tm-monitoring] sledovat dostupnost každou oblast. Zkušební hledá odpověď HTTP 200 pro zadanou adresu URL. Osvědčený vytvořte koncového sestavy celkový stav aplikace a použít tento koncový bod pro zkušební stavu. V opačném zkušební "pořádku" koncový bod při hlásit selhává skutečně důležité část aplikace. Další informace najdete v tématu [Vzorek sledování stavu koncový bod][health-endpoint-monitoring-pattern].   

Při selhání přenosy Manager je určité době, kdy klientů nelze kontaktovat aplikace, což může být několik minut. Dva faktory vliv na celkovou dobou trvání:

- Zkušební stav musíte zjistit primární datovém centru stal nedostupný.

- Servery DNS musíte aktualizovat záznamy z mezipaměti DNS na IP adresu, která závisí DNS time to live (TTL). Výchozí hodnota TTL je 300 sekund (5 minut), ale můžete nakonfigurovat tuto hodnotu při vytváření profilu přenosy správce.

Další informace najdete v tématu [O přenosu správce sledování][tm-monitoring]. 

Pokud správce přenosy selhání, doporučujeme provedení ruční navrácení, spíše než automaticky selhání zpět. Ověřte, zda všechny aplikace podsystémy správný nejdřív. Jinak můžete vytvořit situaci, kdy aplikace překlápět sebou poslalo mezi datacentrech.

Ve výchozím nastavení přenosy správce automaticky nemůže zpět. Nechcete, aby toho ručně nižší priorita primární oblasti po převzetí události. Předpokládejme například, oblasti primární je priorita záznamů 1 a sekundární je priorita záznamů 2. Jakmile přepojení nastavte primární oblast Priorita 3, kterou chcete zabránit tomu, aby automatické. Až budete chtít přepnout zpátky, aktualizujte priority 1.

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

### <a name="sql-server-always-on-configuration"></a>SQL Server vždy na konfigurace

SQL Server vždy na skupiny dostupnost vyžaduje řadiče domény. Všechny uzly ve skupině dostupnost musí být v tu samou doménu AD. Následující body obsahují pokyny týkající se konfigurace dostupnosti skupinu SQL Server vždy na:

- Minimálně umístěte dva řadiče domény v jednotlivých oblastech. 

- Udělit každý řadiče domény statické IP adresy.

- Vytvořte si připojení VNet VNet povolit komunikaci mezi VNets.

- Každý VNet přidáním IP adresy řadiče domény (obě oblasti) do seznamu serveru DNS. Můžete použít příkaz rozhraní příkazového řádku. Další informace najdete v tématu [Správa DNS servery používané virtuální sítě (VNet)][vnet-dns].

```bat
azure network vnet set --resource-group dc01-rg --name dc01-vnet --dns-servers "10.0.0.4,10.0.0.6,172.16.0.4,172.16.0.6"
```

- Vytvoření [Clusterů selhání systému Windows Server] [ wsfc] clusteru (WSFC), který obsahuje instance serveru SQL Server v obou oblastí. 

- Vytvoření SQL Server vždy na dostupnost skupiny, který v primárních a sekundárních oblasti obsahuje instance serveru SQL Server. Postup najdete v článku [Rozšíření vždy na skupiny dostupnosti k vzdálené Azure datacentru (Powershellu)](https://blogs.msdn.microsoft.com/sqlcat/2014/09/22/extending-alwayson-availability-group-to-remote-azure-datacenter-powershell/) . 

- V oblasti primární umístění primární otevřené.

- V oblasti primární umístění jedné nebo více sekundární replikách. Nakonfigurujte pomocí služby Automatická převzetí synchronní potvrzení.

- Umístění jedné nebo více sekundární replikách v oblasti sekundární. Nakonfigurujte používat *asynchronní* potvrdit důvodů výkonu. (V opačném všechny transakce SQL projevit až na doby odezvy v síti k oblasti sekundární.) 

> [AZURE.NOTE] Asynchronní potvrdit repliky nepodporují automatické převzetí. 

Další informace najdete v tématu [Spuštění Windows VMs N-vrstvy architektury na Azure](guidance-compute-n-tier-vm.md#SQL-AlwaysOn-Availability-Group).

## <a name="availability-considerations"></a>Důležité informace o dostupnosti

Komplexní aplikaci N-osy a nemusí potřebujete replikovat celou aplikaci v oblasti sekundární. Můžete místo toho jenom replikovat kritické subsystémem, který je potřeba k podpoře nepřerušený.

Přenosy Manager je bod možné selhání systému. Když službu nepovede, klienti nemají přístup k aplikaci během výpadku. Kontrola [Přenosy správce SLA][tm-sla]a zjistit, zda správce přenosy samotný splňuje vaše obchodní požadavky vysoké dostupnosti. Pokud ne, můžete do ní přidat další řešení pro správu přenosy jako navrácení. Pokud službu Azure přenosy správce nepovede, změňte CNAME záznamů u DNS tak, aby ukazovaly na jiných přenosy službu pro správu. (Tento krok musíte provést ruční a aplikace nebude k dispozici, dokud šíření změn DNS.) 

Clusteru serveru SQL Server existují dva převzetí scénářů k zvážení:

1. Všechny repliky SQL v oblasti hlavní fungovat. Například tato situace může nastat během výpadku místní. V takovém případě se musí ručně převzít SQL dostupnost skupinu, i když je správce přenosy automaticky přejde na front-end. Postupujte podle kroků v tématu [provedení ruční přepojení vynucená skupiny dostupnost serveru SQL](https://msdn.microsoft.com/library/ff877957.aspx), který popisuje, jak provádět vynuceného přepojení pomocí SQL Server Management Studio v jazyce Transact-SQL a Powershellu v SQL serveru 2016. 

    > [AZURE.WARNING] Při vynuceného převzetí není riziko ztráty dat. Po návrat do online režimu oblasti primární udělejte si snímek databáze a [tablediff] najdete rozdíly.

2. Přenosy správce selhání na vedlejší oblast, ale je pořád dostupná primární otevřené SQL. Například front-end osy může selhat, aniž by došlo k ovlivnění SQL VMs. V takovém případě internetový provoz směrován k oblasti sekundární a této oblasti můžete pořád připojení k primární otevřené SQL. Však nebude lepší latence, protože připojení SQL chodí různých oblastí. V takovém případě by měl provést ruční přepojení následujícím způsobem: 

    - Dočasně přepněte SQL otevřené v oblasti sekundární *synchronní* potvrzení. Zajistíte, že nebude ztrátou dat během záložní.

    - Převzetí této otevřené SQL. 
    
    - Nezdaří zpátky na primární oblast-li obnovte nastavení asynchronní potvrzení. 

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
[azure-sql-db]: https://azure.microsoft.com/en-us/documentation/services/sql-database/
[health-endpoint-monitoring-pattern]: https://msdn.microsoft.com/library/dn589789.aspx
[hybrid-vpn]: guidance-hybrid-network-vpn.md
[regional-pairs]: ../best-practices-availability-paired-regions.md
[resource groups]: ../azure-resource-manager/resource-group-overview.md
[resource-group-links]: ../resource-group-link-resources.md
[services-by-region]: https://azure.microsoft.com/en-us/regions/#services
[sql-always-on]: https://msdn.microsoft.com/en-us/library/hh510230.aspx
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
[0]: ./media/blueprints/compute-multi-dc.png "Architektura sítě vysoce k dispozici pro aplikace Azure N-osy"