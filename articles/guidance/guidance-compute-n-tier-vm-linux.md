<properties
   pageTitle="Spouštění Linux virtuálních N-vrstvy architektury na Azure | Microsoft Azure"
   description="Jak spustit Linux VMs N-vrstvy architektury v Microsoft Azure."
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
   ms.date="10/20/2016"
   ms.author="mwasson"/>

# <a name="running-linux-vms-for-an-n-tier-architecture-on-azure"></a>Spouštění Linux virtuálních N-vrstvy architektury na Azure

[AZURE.INCLUDE [pnp-header](../../includes/guidance-pnp-header-include.md)]


> [AZURE.SELECTOR]
- [Spouštění Linux virtuálních N-vrstvy architektury na Azure](guidance-compute-n-tier-vm-linux.md)
- [Spouštění virtuálních Windows N-vrstvy architektury na Azure](guidance-compute-n-tier-vm.md)

Tento článek shrnuje sadu osvědčené postupy pro spuštění Linux virtuálních počítačích (VMs) pro aplikaci N-vrstvy architektury. Vytvoří v následujícím článku [spouštění více virtuálních na Azure][multi-vm].

> [AZURE.NOTE] Azure obsahuje dva různé nasazení modely: [Správce prostředků] [ resource-manager-overview] a klasické. Tento článek pomocí Správce zdroje, který Microsoft doporučuje nové nasazení.

## <a name="architecture-diagram"></a>Diagram architektury

Existuje varianty N-vrstvy architektury. Pro účely těmito doporučeními neměli převážně, důležité rozdíly. V tomto článku se předpokládá typické 3 osy web appu:

- **Web osy.** Zpracuje příchozí žádosti HTTP. Odpovědi na ně budou vráceny pomocí této osy.

- **Obchodní osy.** Implementuje obchodních procesů a jiné funkční logiky pro systém.

- **Vrstva databáze.** Poskytuje trvalý úložný prostor, pomocí Apache Cassandra vysoké dostupnosti.

> Dokument aplikace Visio, který obsahuje tento diagram architektury je možné stáhnout z [webu služby Stažení softwaru][visio-download]. Tento diagram zapnuté vícestránkové osy (Linux) "výpočetním.

![[0]][0]

- **Dostupnost sady.** Vytvoření, [Nastavte dostupnost] [ azure-availability-sets] pro každou úroveň a zřízení aspoň dva VMs v každé osy. Tento přístup je potřeba kontaktovat dostupnost [SLA] [ vm-sla] pro VMs.

- **Podsítí.** Vytvoření samostatného podsítě pro každou úroveň. Zadejte adresu oblast a podsítě vstupní maska pomocí zápisu [CIDR] . 

- **Vyrovnávání zatížení.** Použití [Vyrovnávání zatížení internetového] [ load-balancer-external] k distribuci příchozí internetový provoz a webová vrstva [Vyrovnávání zatížení interní] [ load-balancer-internal] k distribuci v síti od osy web pro firmy osy.

- **Jumpbox**. _Jumpbox_, neboli [u chráněných hostitele]je virtuálního počítače v síti, které správcům umožňuje připojit k jiné VMs. Jumpbox má NSG, umožňující Vzdálená data pouze z povolené veřejných IP adres. NSG by měl povolení komunikace zabezpečené prostředí (SSH).

- **Sledování**. Sledování software například [Nagios], [Zabbix]nebo [Icinga] můžou zobrazit přehled o doba odezvy OM provozu a celkový stav systému. Sledování software nainstalujte OM, který je umístěn v samostatném správy podsítě.

- **NSGs**. Používání [skupin zabezpečení sítě] [ nsg] (NSGs) omezení v síti v VNet. Například 3 vrstvy architektury ukazuje tato část, osy databáze nepřijímá adres webových front-end, jenom z osy firmy a správa podsítě.

- **Apache Cassandra databáze**. Poskytuje dostupnost ve vrstvě dat povolením replikace a překlopení.

## <a name="recommendations"></a>Doporučení

Azure nabízí mnoho různých zdrojů a typů zdrojů, takže může být tento odkaz architektura zřízení mnoha různými způsoby. Správce prostředků Azure šablonu nainstalovat architektura odkaz, který následuje těmito doporučeními uvádíme. Pokud budete chtít vytvořit vlastní odkaz architektura těmito doporučeními řídit Pokud požadujete konkrétní doporučení příliš velký.

### <a name="vnet--subnets"></a>VNet / podsítí

Při vytváření VNet přiřaďte dost místa adres podsítí, které budete potřebovat. Zadejte VNet adresu oblast a podsítě masku pomocí zápisu [CIDR] . Použití adresní prostor, která spadají do standardní [soukromé blok adresy IP][private-ip-space], které jsou 10.0.0.0/8, 172.16.0.0/12 a 192.168.0.0/16.

V případě, že budete muset nastavit brána mezi VNet a v místní síti později, vyberte rozsah adres, který nepřekrývá k síti na pracovišti. Jakmile vytvoříte VNet, není možné změnit rozsah adres.

Navrhněte podsítí s funkcí a zabezpečení požadavky na paměti. Všechny VMs v rámci stejné osy nebo role patří do stejné podsítě, což může být hranici zabezpečení. Další informace o návrhu VNets a podsítí, najdete v článku [plánování a návrhů sítí virtuální Azure][plan-network].

Pro každý podsítě vyplnit adresní prostor podsítě CIDR zápisu. Například "10.0.0.0/24" vytvoří oblasti 256 IP adres. (VMs můžete použít 251 těchto; pět vyhrazená. [Virtuální sítě nejčastější dotazy týkající se][vnet faq].) Zkontrolujte, že rozsahy adres nepřekrývaly přes podsítí.

### <a name="load-balancers"></a>Vyrovnávání zatížení

Vyrovnávání zatížení externí distribuuje internetových přenosů na web osy. Vytvořte veřejnou IP adresu pro tento Vyrovnávání zatížení. V tématu [Vytvoření Vyrovnávání zatížení internetového][lb-external-create].

Vyrovnávání zatížení interní distribuuje v síti od osy web pro firmy osy. Tento Vyrovnávání zatížení soukromých IP adres, vytvořte konfiguraci IP frontend a přidružit podsítě pro firmy osy. V tématu [Začínáme vytváření Vyrovnávání zatížení vnitřní][lb-internal-create].

### <a name="cassandra"></a>Cassandra

Doporučujeme [DataStax Enterprise] [ datastax] výrobní použití, ale těmito doporučeními platí pro všechny Cassandra edition. Další informace o spuštění DataStax v Azure v tématu [Průvodce nasazením DataStax Enterprise pro Azure][cassandra-in-azure]. 

Umístěte VMs clusteru Cassandra sadu dostupnost, aby repliky Cassandra rozvržena víc domén poruch a upgrade domény. Další informace o doménách poruch a upgrade domén, přečtěte si téma [Správa dostupnosti virtuálních počítačích][availability-sets-manage]. 

Konfigurace poruch domén 3 (maximálně) na dostupnost sadu. 

Konfigurace 18 upgradu domény na dostupnost sadu. To vám dá maximální počet upgradu domény než můžete pořád být rovnoměrně v doménách poruch.   

Konfigurace uzlů v kompatibilních regálů režimu. Mapování poruch domény na stojany v `cassandra-rackdc.properties` soubor.

Nemusíte Vyrovnávání zatížení před clusteru. Přímo na uzel clusteru připojení klienta.

### <a name="jumpbox"></a>Jumpbox

Umístěte jumpbox ve stejné VNet jako ostatní VMs, ale v samostatných správy podsítě.

Vytvořte [veřejnou IP adresu] pro jumpbox.

Použijte malé velikost OM pro jumpbox, například standardní A1.

Konfigurace NSGs pro web osy, osy firmy a databáze osy podsítí umožňuje pro správu (SSH) přenosu přes z podsítě správy.

Zajistit jumpbox vytvořte NSG a použít ho na podsítě jumpbox. Přidáte pravidlo NSG, který umožňuje SSH připojení jenom ze sady povolené IP adresy.

NSG může být připojen k podsítě nebo jumpbox NIC. V tomto případě doporučujeme připojení k NIC, abyste SSH přenosy se jenom pro jumpbox, i když naopak přidáte další VMs stejné podsítě.

Nepovolit SSH přístup z veřejného Internetu VMs spuštěné pracovní zátěž aplikace. Místo toho všechny SSH přístup k tyto VMs musí pocházet prostřednictvím jumpbox. Správce přihlásí jumpbox a potom zaznamená do jiných OM z jumpbox. Jumpbox umožňuje SSH přenos z Internetu, ale jenom z známé, povolené IP adresy.

## <a name="availability-considerations"></a>Důležité informace o dostupnosti

Přepněte do sady samostatné dostupnost každé OM role nebo vrstvy. Nevkládejte VMs z různých úrovní do stejnou sadu dostupnosti. 

Ve vrstvě databáze s více VMs překlady automaticky do vysoce dostupné databáze. Relační databáze obvykle musíte používat replikace a převzetí k dosažení dostupnost.  

Pokud potřebujete vyšší dostupnost než [Azure SLA pro VMs] [ vm-sla] obsahuje, replikace aplikace dvou oblastí a pomocí Azure přenosy správce pro překlopení. Další informace najdete v tématu [Spuštění Linux VMs ve více oblastech vysoké dostupnosti][multi-dc].  

## <a name="security-considerations"></a>Otázky bezpečnosti pro

Pomocí pravidel NSG omezení komunikaci mezi úrovní. Například 3 vrstvy architektury nahoře, osy web není komunikovat přímo osy databáze. K jejímu vynucení, byste měli osy databáze zablokovat příchozích z podsítě osy web.  

  1. Vytvořte NSG a přidružit k podsítě osy databáze.

  2. Přidáte pravidlo, které zakazuje všechny příchozí data z VNet. (Použít `VIRTUAL_NETWORK` značku v pravidle.) 

  3. Přidání pravidla s vyšší prioritou umožňující příchozích z firmy podsítě osy. Toto pravidlo přepíše předchozí pravidlo a umožňuje osy firmy ke komunikaci s osy databáze.

  4. Přidáte pravidlo, které umožňuje příchozích z v databázi osy síť. Pokud chcete toto pravidlo umožňuje komunikaci mezi VMs ve vrstvě databáze, která je potřebná pro replikace databáze a překlopení.

  5. Přidáte pravidlo, které umožňuje SSH adres podsítí jumpbox. Pokud chcete toto pravidlo umožňuje správcům připojení k databázi osy z jumpbox.

  > [AZURE.NOTE] NSG má [výchozí pravidla] [ nsg-rules] , které umožňují všechny příchozí adres v VNet. Tato pravidla nelze odstranit, ale je možné přepsat vytvořením vyšší priority pravidel.

Můžete do ní přidat virtuální zařízení síti (NVA) k vytvoření DMZ mezi veřejné Internet a Azure virtuální sítě. NVA je obecný termín pro virtuální zařízení, které můžete provádět úkoly související se sítí například brány firewall paketu kontroly, auditování, vlastní směrování a řadu dalších operací. Další informace najdete v tématu [implementace DMZ mezi Azure a Internetu][dmz].

## <a name="scalability-considerations"></a>Škálovatelnost: co byste měli zvážit

Vyrovnávání zatížení distribuovat v síti na web a business úrovní. Velikost ve vodorovném směru přidáním nové instance OM. Všimněte si, že je možné přizpůsobit úrovně web a business nezávisle na sobě, založené na načíst. Snížit možné komplikace způsobená potřeba zachovat spřažení, třeba příslušnosti VMs ve vrstvě web. VMs hostingu obchodní logiky by měly být také příslušnosti.

## <a name="manageability-considerations"></a>Důležité informace o možnostech správy

Zjednodušení správy celého systému pomocí nástrojů pro centrální správu například [Azure automatizaci][azure-administration], [Microsoft operace správy Suite][operations-management-suite], [Chef][chef], nebo [Puppet][puppet]. Tyto nástroje můžete sloučit diagnostic a stav informací zachycených z více VMs poskytnout celkové zobrazení systému.

## <a name="solution-deployment"></a>Nasazení řešení

Nasazení pro implementující těmito doporučeními architekturu odkaz je dostupný na [Github][github-folder]. Tento odkaz architektura obsahuje web osy, obchodní osy a osy dat.

1. Kliknutím na tlačítko.  
[! ["Nasazení Azure"] [1]][2]

2. Jakmile na odkaz v portálu Azure otevře, zadejte hodnoty sledovat: 
  - Název **pole Skupina zdroje** je definován v souboru parametrů, takže vyberte **Vytvořit nový** a zadejte `ra-ntier-sql-network-rg` do textového pole.
  - Vyberte oblast z **umístění** rozevíracím seznamu.
  - Úprava **Šablony kořenové Uri** nebo **Parametr kořenové Uri** textová pole.
  - Zkontrolujte podmínky a ujednání a potom klepnutím na zaškrtávací políčko **že můžu souhlasíte s podmínkami a ujednáními uvedené výše** .
  - Klikněte na tlačítko **koupit** .

3. Zkontrolujte, že Azure portálu oznámení zprávu nasazení je dokončeno.

4. Parametr soubory obsahují pevně správce uživatelská jména a hesla a důrazně doporučujeme okamžitě změnit na všechny VMs. Každý OM na portálu Azure klepnutím na o **resetování hesla** v zásuvné **podpory + Poradce při potížích** . Vyberte **Resetovat heslo** do pole rozevíracího seznamu **režim** a potom vyberte nové **uživatelské jméno** a **heslo**. Klikněte na tlačítko **Aktualizovat** pro zachování nové uživatelské jméno a heslo.

## <a name="next-steps"></a>Další kroky

Abyste dosáhli dostupnost pro tento odkaz architekturu, doporučujeme, abyste [s nasazováním ve více oblastech][multi-dc].

<!-- links -->

[azure-administration]: ../automation/automation-intro.md
[azure-availability-sets]: ../virtual-machines/virtual-machines-windows-manage-availability.md#configure-each-application-tier-into-separate-availability-sets
[availability-sets-manage]: ../virtual-machines/virtual-machines-windows-manage-availability.md
[Host (hostitel) u chráněných]: https://en.wikipedia.org/wiki/Bastion_host
[cassandra-consistency]: http://docs.datastax.com/en/cassandra/2.0/cassandra/dml/dml_config_consistency_c.html
[cassandra-consistency-usage]: http://medium.com/@foundev/cassandra-how-many-nodes-are-talked-to-with-quorum-also-should-i-use-it-98074e75d7d5#.b4pb4alb2
[cassandra-in-azure]: https://docs.datastax.com/en/datastax_enterprise/4.5/datastax_enterprise/install/installAzure.html
[cassandra-replication]: http://www.planetcassandra.org/data-replication-in-nosql-databases-explained/
[CIDR]: https://en.wikipedia.org/wiki/Classless_Inter-Domain_Routing
[chef]: https://www.chef.io/solutions/azure/
[datastax]: http://www.datastax.com/products/datastax-enterprise
[dmz]: guidance-iaas-ra-secure-vnet-dmz.md
[github-folder]: https://github.com/mspnp/reference-architectures/tree/master/guidance-compute-n-tier
[lb-external-create]: ../load-balancer/load-balancer-get-started-internet-portal.md
[lb-internal-create]: ../load-balancer/load-balancer-get-started-ilb-arm-portal.md
[load-balancer-external]: ../load-balancer/load-balancer-internet-overview.md
[load-balancer-internal]: ../load-balancer/load-balancer-internal-overview.md
[multi-dc]: guidance-compute-multiple-datacenters-linux.md
[multi-vm]: guidance-compute-multi-vm.md
[naming conventions]: guidance-naming-conventions.md
[nsg]: ../virtual-network/virtual-networks-nsg.md
[nsg-rules]: ../best-practices-resource-manager-security.md#network-security-groups
[operations-management-suite]: https://www.microsoft.com/en-us/server-cloud/operations-management-suite/overview.aspx
[plan-network]: ../virtual-network/virtual-network-vnet-plan-design-arm.md
[private-ip-space]: https://en.wikipedia.org/wiki/Private_network#Private_IPv4_address_spaces
[veřejnou IP adresu]: ../virtual-network/virtual-network-ip-addresses-overview-arm.md
[puppet]: https://puppetlabs.com/blog/managing-azure-virtual-machines-puppet
[resource-manager-overview]: ../azure-resource-manager/resource-group-overview.md
[vm-sla]: https://azure.microsoft.com/en-us/support/legal/sla/virtual-machines
[vnet faq]: ../virtual-network/virtual-networks-faq.md
[visio-download]: http://download.microsoft.com/download/1/5/6/1569703C-0A82-4A9C-8334-F13D0DF2F472/RAs.vsdx
[Nagios]: https://www.nagios.org/
[Zabbix]: http://www.zabbix.com/
[Icinga]: http://www.icinga.org/
[0]: ./media/blueprints/compute-n-tier-linux.png "N-vrstvy architektury pomocí Microsoft Azure"
[1]: ./media/blueprints/deploybutton.png 
[2]: https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fmspnp%2Freference-architectures%2Fmaster%2Fguidance-compute-n-tier%2Fazuredeploy.json


