<properties
   pageTitle="Spouštění virtuálních Windows N-vrstvy architektury | Vytvořte odkaz architektura | Microsoft Azure"
   description="Jak implementovat architekturu vícevrstvého na Azure, platební zvláštní pozornost dostupnost, zabezpečení, škálovatelnost a možnosti správy zabezpečení."
   services=""
   documentationCenter="na"
   authors="MikeWasson"
   manager="christb"
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

# <a name="running-windows-vms-for-an-n-tier-architecture-on-azure"></a>Spouštění virtuálních Windows N-vrstvy architektury na Azure

> [AZURE.INCLUDE [pnp-header](../../includes/guidance-pnp-header-include.md)]

> [AZURE.SELECTOR]
- [Spouštění Linux virtuálních N-vrstvy architektury na Azure](guidance-compute-n-tier-vm-linux.md)
- [Spouštění virtuálních Windows N-vrstvy architektury na Azure](guidance-compute-n-tier-vm.md)

Tento článek shrnuje sadu osvědčené postupy pro systém Windows virtuálních počítačích (VMs) pro aplikaci N-vrstvy architektury. Vytvoří v následujícím článku [spouštění více virtuálních na Azure][multi-vm].

> [AZURE.NOTE] Azure obsahuje dva různé nasazení modely: [Správce prostředků] [ resource-manager-overview] a klasické. Tento článek používá správce zdroje, který Microsoft doporučuje nové nasazení.

## <a name="architecture-diagram"></a>Diagram architektury

Existuje varianty N-vrstvy architektury. Pro účely těmito doporučeními neměli převážně, důležité rozdíly. V tomto článku se předpokládá typické 3 osy web appu:

- **Web osy.** Zpracuje příchozí žádosti HTTP. Pomocí této osy se vracejí odpovědi.

- **Obchodní osy.** Implementuje obchodních procesů a jiné funkční logiky pro systém.

- **Vrstva databáze.** Poskytuje trvalý úložný prostor, použití [SQL Server vždy na dostupnost skupin] [ sql-alwayson] vysoké dostupnosti.

> Dokument aplikace Visio, který obsahuje tento diagram architektury je možné stáhnout z [webu služby Stažení softwaru][visio-download]. Tento diagram zapnuté vícestránkové osy (Windows) "výpočetním.

![[0]][0]

- **Dostupnost sady.** Vytvoření, [Nastavte dostupnost] [ azure-availability-sets] pro každou úroveň a zřízení aspoň dva VMs v každé osy. Tento přístup je potřeba kontaktovat dostupnost [SLA] [ vm-sla] pro VMs.

- **Podsítí.** Vytvoření samostatného podsítě pro každou úroveň. Zadejte adresu oblast a podsítě vstupní maska pomocí zápisu [CIDR] . 

- **Vyrovnávání zatížení.** Použití [Vyrovnávání zatížení internetového] [ load-balancer-external] k distribuci příchozí internetový provoz a webová vrstva [Vyrovnávání zatížení interní] [ load-balancer-internal] k distribuci v síti od osy web pro firmy osy.

- **Jumpbox**. _Jumpbox_, neboli [u chráněných hostitele]je virtuálního počítače v síti, které správcům umožňuje připojit k jiné VMs. Jumpbox má NSG, umožňující Vzdálená data pouze z povolené veřejných IP adres. NSG by měl povolení komunikace Vzdálená plocha (RDP).

- **Sledování**. Sledování software například [Nagios], [Zabbix]nebo [Icinga] můžou zobrazit přehled o doba odezvy OM provozu a celkový stav systému. Sledování software nainstalujte OM, který je umístěn v samostatném správy podsítě.

- **NSGs**. Používání [skupin zabezpečení sítě] [ nsg] (NSGs) omezení v síti v VNet. Například 3 vrstvy architektury ukazuje tato část, osy databáze nepřijímá adres webových front-end, jenom z osy firmy a správa podsítě.

- **SQL Server vždy na dostupnost skupiny.** Poskytuje dostupnost ve vrstvě dat povolením replikace a překlopení.

- **Služby active Directory Domain Services (AD DS) servery**. Active Directory Domain Services (AD DS) ukládá data adresáře a spravuje komunikaci mezi uživateli a domén, včetně přihlášení uživatele, ověřování a vyhledávání v adresáři. Řadiče domény Active Directory je server, na kterém běží služba AD DS. Před verzí systému Windows Server 2016 musí být vždy na skupiny dostupnost připojen k doméně. Je to proto skupiny dostupnost závisí na technologii Windows Server překlopení obrázku (WSFC). Windows serveru 2016 představuje schopnost vytvoření záložního clusteru bez služby Active Directory. Další informace najdete v tématu [Co je nového v převzetí clusterů ve Windows serveru 2016][wsfc-whats-new]

## <a name="recommendations"></a>Doporučení

Azure nabízí mnoho různých zdrojů a typů zdrojů, takže může být tento odkaz architektura zřízení mnoha různými způsoby. Správce prostředků Azure šablonu nainstalovat architektura odkaz, který následuje těmito doporučeními uvádíme. Pokud budete chtít vytvořit vlastní odkaz architektura těmito doporučeními řídit Pokud požadujete konkrétní doporučení příliš velký.

### <a name="vnet--subnets"></a>VNet / podsítí

Při vytváření VNet přiřaďte dost místa adres podsítí, které budete potřebovat. Zadejte VNet adresu oblast a podsítě masku pomocí zápisu [CIDR] . Použití adresní prostor, která spadají do standardní [soukromé blok adresy IP][private-ip-space], které jsou 10.0.0.0/8, 172.16.0.0/12 a 192.168.0.0/16.

V případě, že budete muset nastavit brána mezi VNet a v místní síti později, vyberte rozsah adres, který nepřekrývá k síti na pracovišti. Jakmile vytvoříte VNet, není možné změnit rozsah adres.

Navrhněte podsítí s funkcí a zabezpečení požadavky na paměti. Všechny VMs v rámci stejné osy nebo role patří do stejné podsítě, což může být hranici zabezpečení. Další informace o návrhu VNets a podsítí, najdete v článku [plánování a návrhů sítí virtuální Azure][plan-network].

Pro každý podsítě vyplnit adresní prostor podsítě CIDR zápisu. Například "10.0.0.0/24" vytvoří oblasti 256 IP adres. (VMs můžete použít 251 těchto; pět vyhrazená. [Virtuální sítě nejčastější dotazy týkající se][vnet faq].) Zkontrolujte, že rozsahy adres nepřekrývaly přes podsítí.

### <a name="network-security-groups"></a>Skupiny zabezpečení sítě

Pomocí pravidel NSG omezení komunikaci mezi úrovní. Například 3 vrstvy architektury nahoře, osy web není komunikovat přímo osy databáze. K jejímu vynucení, byste měli osy databáze zablokovat příchozích z podsítě osy web.  

  1. Vytvořte NSG a přidružit k podsítě osy databáze.

  2. Přidáte pravidlo, které zakazuje všechny příchozí data z VNet. (Použít `VIRTUAL_NETWORK` značku v pravidle.) 

  3. Přidání pravidla s vyšší prioritou umožňující příchozích z firmy podsítě osy. Toto pravidlo přepíše předchozí pravidlo a umožňuje osy firmy ke komunikaci s osy databáze.

  4. Přidáte pravidlo, které umožňuje příchozích z v databázi osy síť. Pokud chcete toto pravidlo umožňuje komunikaci mezi VMs ve vrstvě databáze, která je potřebná pro replikace databáze a překlopení.

  5. Přidáte pravidlo, které umožňuje RDP adres podsítí jumpbox. Pokud chcete toto pravidlo umožňuje správcům připojení k databázi osy z jumpbox.

  > [AZURE.NOTE] NSG má [výchozí pravidla] [ nsg-rules] , které umožňují všechny příchozí adres v VNet. Tato pravidla nelze odstranit, ale je možné přepsat vytvořením vyšší priority pravidel.

### <a name="load-balancers"></a>Vyrovnávání zatížení

Vyrovnávání zatížení externí distribuuje internetových přenosů na web osy. Vytvořte veřejnou IP adresu pro tento Vyrovnávání zatížení. V tématu [Vytvoření Vyrovnávání zatížení internetových][lb-external-create].

Vyrovnávání zatížení interní distribuuje v síti od osy web pro firmy osy. Tento Vyrovnávání zatížení soukromých IP adres, vytvořte konfiguraci IP frontend a přidružit podsítě pro firmy osy. V tématu [Začínáme vytváření Vyrovnávání zatížení vnitřní][lb-internal-create].

### <a name="sql-server-always-on-availability-groups"></a>SQL Server vždy na dostupnost skupiny

Doporučujeme [Vždy na skupiny dostupnost] [ sql-alwayson] vysoké dostupnosti SQL serveru. Vždy na skupiny dostupnost vyžaduje řadiče domény. Všechny uzly ve skupině dostupnost musí být v tu samou doménu AD.

Další úrovní připojení k databázi přes [dostupnost skupiny posluchače][sql-alwayson-listeners]. Posluchače klientovi SQL připojení bez znalosti název pole fyzicky instance serveru SQL Server. VMs, přístup k databázi musí být členem domény. Klient (v tomto případě jiné vrstvě) DNS posluchače virtuální síťový název na pomocí IP adres.

SQL Server vždy na konfigurujte následujícím způsobem:

- Vytvoření systému Windows Server převzetí clusterů (WSFC) obrázku a dostupnosti skupinu SQL Server vždy na. Další informace najdete v tématu [Začínáme s vždy na skupiny dostupnost][sql-alwayson-getting-started].

- Vytvoření Vyrovnávání zatížení vnitřní pomocí statické IP adresu soukromé.

- Vytvoření skupiny posluchače dostupnost a mapování název posluchače DNS na IP adresu Vyrovnávání zatížení vnitřní. 

- Vytvoření pravidla Vyrovnávání zatížení pro SQL Server listening port (port TCP 1433 ve výchozím nastavení). Pravidlo Vyrovnávání zatížení musí povolit _plovoucí IP_taky se mu říká přímé Server vrátit. To způsobí, že OM odpovědi přímo k desktopovému klientovi, což umožňuje přímé připojení k primární otevřené.

    > [AZURE.NOTE] Při zapnuté funkci plovoucí IP, musí být číslo portu front-end stejné jako číslo portu back-end v pravidlech Vyrovnávání zatížení.

Pokud klienta SQL pokusí o připojení, přesměruje Vyrovnávání zatížení na žádost o připojení, která je aktuální primární otevřené. Při přepnutí do jiného otevřené Vyrovnávání zatížení automaticky přesměrovává následující požadavky na nové primární otevřené. Další informace najdete v tématu [Konfigurace služba Vyrovnávání zatížení pro SQL vždy na][sql-alwayson-ilb].

Během převzetí uzavřeny existující připojení klientů. Po dokončení záložní nové připojení směrovány do nového primární otevřené.

Pokud aplikace udělá při čtení mnohem více než zápisy, může některé jen pro čtení dotazy na vedlejší otevřené převzít. V tématu [využití posluchače k připojení ke vedlejší jen pro čtení otevřené (jen pro čtení směrování)][sql-alwayson-read-only-routing].

Otestujte nasazení [vynuceného ruční převzetí][sql-alwayson-force-failover].

### <a name="jumpbox"></a>Jumpbox

Nepovolit RDP přístup z veřejného Internetu VMs spuštěné pracovní zátěž aplikace. Místo toho všechny RDP/SSH přístup k tyto VMs musí pocházet prostřednictvím jumpbox. Správce přihlásí jumpbox a potom zaznamená do jiných OM z jumpbox. Jumpbox umožňuje RDP přenos z Internetu, ale jenom z známé, povolené IP adresy.

Umístěte jumpbox ve stejné VNet jako ostatní VMs, ale v samostatných správy podsítě.

Vytvořte [veřejnou IP adresu] pro jumpbox.

Použijte malé velikost OM pro jumpbox, například standardní A1.

Konfigurace NSGs pro web osy, osy firmy a databáze osy podsítí umožňuje pro správu (RDP) přenosu přes z podsítě správy.

Zajistit jumpbox vytvořte NSG a použít ho na podsítě jumpbox. Přidáte pravidlo NSG, který umožňuje připojení RDP jenom ze sady povolené IP adresy.

NSG může být připojen k podsítě nebo jumpbox NIC. V tomto případě doporučujeme připojení k NIC, abyste RDP přenosy se jenom pro jumpbox, i když naopak přidáte další VMs stejné podsítě.

## <a name="availability-considerations"></a>Důležité informace o dostupnosti

Přepněte do sady samostatné dostupnost každé OM role nebo vrstvy. Nevkládejte VMs z různých úrovní do stejnou sadu dostupnosti. 

Ve vrstvě databáze s více VMs překlady automaticky do vysoce dostupné databáze. Relační databáze obvykle musíte používat replikace a převzetí k dosažení dostupnost. Pro systém SQL Server, doporučujeme vám použít [Vždy na skupiny dostupnost][sql-alwayson]. 

Pokud potřebujete vyšší dostupnost než [Azure SLA pro VMs] [ vm-sla] obsahuje, replikace aplikace dvou oblastí a pomocí Azure přenosy správce pro překlopení. Další informace najdete v tématu [Spuštění Windows VMs ve více oblastech vysoké dostupnosti][multi-dc].   

## <a name="security-considerations"></a>Otázky bezpečnosti pro

Šifrování data na ostatních. Použití [Azure klíč trezoru] [ azure-key-vault] Správa šifrovacího klíče databáze. Klíč trezoru mohou být uloženy šifrovacího klíče v hardwarové zabezpečení moduly (moduly hardwarového zabezpečení). Další informace najdete v tématu [Konfigurace Azure klíč trezoru integrace pro systém SQL Server na Azure VMs] [ sql-keyvault] také doporučujeme uchovávat tajemství aplikace, například řetězců připojení k databázi klíč trezoru.

Můžete do ní přidat virtuální zařízení síti (NVA) k vytvoření DMZ mezi veřejné Internet a Azure virtuální sítě. NVA je obecný termín pro virtuální zařízení, které můžete provádět úkoly související se sítí například brány firewall paketu kontroly, auditování, vlastní směrování a řadu dalších operací. Další informace najdete v tématu [implementace DMZ mezi Azure a Internetu][dmz].

## <a name="scalability-considerations"></a>Co byste měli zvážit škálovatelnost:

Vyrovnávání zatížení distribuovat v síti na web a business úrovní. Velikost ve vodorovném směru přidáním nové instance OM. Všimněte si, že je možné přizpůsobit úrovně web a business nezávisle na sobě, založené na načíst. Snížit možné komplikace způsobená potřeba zachovat spřažení, třeba příslušnosti VMs ve vrstvě web. VMs hostingu obchodní logiky by měly být také příslušnosti.

## <a name="manageability-considerations"></a>Důležité informace o možnostech správy

Zjednodušení správy celého systému pomocí nástrojů pro centrální správu například [Azure automatizaci][azure-administration], [Microsoft operace správy Suite][operations-management-suite], [Chef][chef], nebo [Puppet][puppet]. Tyto nástroje můžete sloučit diagnostic a stav informací zachycených z více VMs poskytnout celkové zobrazení systému.

## <a name="solution-deployment"></a>Nasazení řešení

Nasazení pro implementující těmito doporučeními architekturu odkaz je dostupný na [Github][github-folder]. Tento odkaz architektura obsahuje web osy, obchodní osy, osy dat i jumpbox OM a služby Active Directory řadiče domény.

Architektura odkaz můžete nasadit podle pokynů níže: 

1. Kliknutím na tlačítko.  
[! ["Nasazení Azure"] [1]][2]

2. Jakmile na odkaz v portálu Azure otevře, zadejte hodnoty sledovat: 
  - Název **pole Skupina zdroje** je definován v souboru parametrů, takže vyberte **Vytvořit nový** a zadejte `ra-ntier-sql-network-rg` do textového pole.
  - Vyberte oblast z **umístění** rozevíracím seznamu.
  - Úprava **Šablony kořenové Uri** nebo **Parametr kořenové Uri** textová pole.
  - Zkontrolujte podmínky a ujednání a potom klepnutím na zaškrtávací políčko **že můžu souhlasíte s podmínkami a ujednáními uvedené výše** .
  - Klikněte na tlačítko **koupit** .

3. Zkontrolujte, že Azure portálu oznámení zprávu nasazení je dokončeno.

4. Kliknutím na tlačítko.  
[! ["Nasazení Azure"] [1]][3]

5. Jakmile na odkaz v portálu Azure otevře, zadejte hodnoty sledovat: 
  - Název **pole Skupina zdroje** je definován v souboru parametrů, takže vyberte možnost **Použít existující** a zadejte `ra-ntier-sql-workload-rg` do textového pole.
  - Vyberte oblast z **umístění** rozevíracím seznamu.
  - Úprava **Šablony kořenové Uri** nebo **Parametr kořenové Uri** textová pole.
  - Zkontrolujte podmínky a ujednání a potom klepnutím na zaškrtávací políčko **že můžu souhlasíte s podmínkami a ujednáními uvedené výše** .
  - Klikněte na tlačítko **koupit** .

6. Zkontrolujte, že Azure portálu oznámení zprávu nasazení je dokončeno.

7. Kliknutím na tlačítko.  
[! ["Nasazení Azure"] [1]][4]

8. Jakmile na odkaz v portálu Azure otevře, zadejte hodnoty sledovat: 
  - Název **pole Skupina zdroje** je definován v souboru parametrů, takže vyberte **Vytvořit nový** a zadejte `ra-ntier-sql-network-rg` do textového pole.
  - Vyberte oblast z **umístění** rozevíracím seznamu.
  - Úprava **Šablony kořenové Uri** nebo **Parametr kořenové Uri** textová pole.
  - Zkontrolujte podmínky a ujednání a potom klepnutím na zaškrtávací políčko **že můžu souhlasíte s podmínkami a ujednáními uvedené výše** .
  - Klikněte na tlačítko **koupit** .

9. Zkontrolujte, že Azure portálu oznámení zprávu nasazení je dokončeno.

10. Parametr soubory obsahují pevně správce uživatelská jména a hesla a důrazně doporučujeme okamžitě změnit na všechny VMs obojí. Každý OM na portálu Azure klepnutím na o **resetování hesla** v zásuvné **podpory + Poradce při potížích** . Vyberte **Resetovat heslo** do pole rozevíracího seznamu **režim** a potom vyberte nové **uživatelské jméno** a **heslo**. Klikněte na tlačítko **Aktualizovat** pro zachování nové uživatelské jméno a heslo. 

Informace o dalších způsobech nasazení tento odkaz architektura najdete v tématu v souboru readme v [pokyny jednoduchým OM] [ github-folder] Github složky. 

## <a name="next-steps"></a>Další kroky

Abyste dosáhli dostupnost pro tento odkaz architekturu, doporučujeme, abyste [s nasazováním ve více oblastech][multi-dc].

<!-- links -->

[arm-templates]: https://azure.microsoft.com/documentation/articles/resource-group-authoring-templates/
[azure-administration]: ../automation/automation-intro.md
[azure-audit-logs]: ../resource-group-audit.md
[azure-availability-sets]: ../virtual-machines/virtual-machines-windows-manage-availability.md#configure-each-application-tier-into-separate-availability-sets
[azure-cli]: ../virtual-machines-command-line-tools.md
[azure-key-vault]: https://azure.microsoft.com/services/key-vault.md
[azure-load-balancer]: ../load-balancer/load-balancer-overview.md
[Host (hostitel) u chráněných]: https://en.wikipedia.org/wiki/Bastion_host
[CIDR]: https://en.wikipedia.org/wiki/Classless_Inter-Domain_Routing
[chef]: https://www.chef.io/solutions/azure/
[dmz]: guidance-iaas-ra-secure-vnet-dmz.md
[github-folder]: https://github.com/mspnp/reference-architectures/tree/master/guidance-compute-n-tier-sql
[lb-external-create]: ../load-balancer/load-balancer-get-started-internet-portal.md
[lb-internal-create]: ../load-balancer/load-balancer-get-started-ilb-arm-portal.md
[load-balancer-external]: ../load-balancer/load-balancer-internet-overview.md
[load-balancer-internal]: ../load-balancer/load-balancer-internal-overview.md
[multi-dc]: guidance-compute-multiple-datacenters.md
[multi-vm]: guidance-compute-multi-vm.md
[n-tier]: guidance-compute-n-tier-vm.md
[naming conventions]: guidance-naming-conventions.md
[nsg]: ../virtual-network/virtual-networks-nsg.md
[nsg-rules]: ../best-practices-resource-manager-security.md#network-security-groups
[operations-management-suite]: https://www.microsoft.com/en-us/server-cloud/operations-management-suite/overview.aspx
[plan-network]: ../virtual-network/virtual-network-vnet-plan-design-arm.md
[private-ip-space]: https://en.wikipedia.org/wiki/Private_network#Private_IPv4_address_spaces
[veřejnou IP adresu]: ../virtual-network/virtual-network-ip-addresses-overview-arm.md
[puppet]: https://puppetlabs.com/blog/managing-azure-virtual-machines-puppet
[resource-manager-overview]: ../azure-resource-manager/resource-group-overview.md
[sql-alwayson]: https://msdn.microsoft.com/en-us/library/hh510230.aspx
[sql-alwayson-force-failover]: https://msdn.microsoft.com/en-us/library/ff877957.aspx
[sql-alwayson-getting-started]: https://msdn.microsoft.com/en-us/library/gg509118.aspx
[sql-alwayson-ilb]: https://blogs.msdn.microsoft.com/igorpag/2016/01/25/configure-an-ilb-listener-for-sql-server-alwayson-availability-groups-in-azure-arm/
[sql-alwayson-listeners]: https://msdn.microsoft.com/en-us/library/hh213417.aspx
[sql-alwayson-read-only-routing]: https://technet.microsoft.com/en-us/library/hh213417.aspx#ConnectToSecondary
[sql-keyvault]: ../virtual-machines/virtual-machines-windows-ps-sql-keyvault.md
[vm-planned-maintenance]: ../virtual-machines/virtual-machines-windows-planned-maintenance.md
[vm-sla]: https://azure.microsoft.com/en-us/support/legal/sla/virtual-machines
[vnet faq]: ../virtual-network/virtual-networks-faq.md
[wsfc-whats-new]: https://technet.microsoft.com/windows-server-docs/failover-clustering/whats-new-in-failover-clustering
[Nagios]: https://www.nagios.org/
[Zabbix]: http://www.zabbix.com/
[Icinga]: http://www.icinga.org/
[VM-sizes]: https://azure.microsoft.com/documentation/articles/virtual-machines-windows-sizes/
[solution-script]: https://github.com/mspnp/reference-architectures/tree/master/guidance-compute-n-tier/Deploy-ReferenceArchitecture.ps1
[solution-script-bash]: https://github.com/mspnp/reference-architectures/tree/master/guidance-compute-n-tier/deploy-reference-architecture.sh
[vnet-parameters-windows]: https://github.com/mspnp/reference-architectures/tree/master/guidance-compute-n-tier/parameters/windows/virtualNetwork.parameters.json
[vnet-parameters-linux]: https://github.com/mspnp/reference-architectures/tree/master/guidance-compute-n-tier/parameters/linux/virtualNetwork.parameters.json
[nsg-parameters-windows]: https://github.com/mspnp/reference-architectures/tree/master/guidance-compute-n-tier/parameters/windows/networkSecurityGroups.parameters.json
[nsg-parameters-linux]: https://github.com/mspnp/reference-architectures/tree/master/guidance-compute-n-tier/parameters/linux/networkSecurityGroups.parameters.json
[webtier-parameters-windows]: https://github.com/mspnp/reference-architectures/tree/master/guidance-compute-n-tier/parameters/windows/webTier.parameters.json
[webtier-parameters-linux]: https://github.com/mspnp/reference-architectures/tree/master/guidance-compute-n-tier/parameters/linux/webTier.parameters.json
[businesstier-parameters-windows]: https://github.com/mspnp/reference-architectures/tree/master/guidance-compute-n-tier/parameters/windows/businessTier.parameters.json
[businesstier-parameters-linux]: https://github.com/mspnp/reference-architectures/tree/master/guidance-compute-n-tier/parameters/linux/businessTier.parameters.json
[datatier-parameters-windows]: https://github.com/mspnp/reference-architectures/tree/master/guidance-compute-n-tier/parameters/windows/dataTier.parameters.json
[datatier-parameters-linux]: https://github.com/mspnp/reference-architectures/tree/master/guidance-compute-n-tier/parameters/linux/dataTier.parameters.json
[azure-powershell-download]: https://azure.microsoft.com/documentation/articles/powershell-install-configure/
[visio-download]: http://download.microsoft.com/download/1/5/6/1569703C-0A82-4A9C-8334-F13D0DF2F472/RAs.vsdx
[0]: ./media/blueprints/compute-n-tier.png "N-vrstvy architektury pomocí Microsoft Azure"
[1]: ./media/blueprints/deploybutton.png 
[2]: https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fmspnp%2Freference-architectures%2Fmaster%2Fguidance-compute-n-tier-sql%2FvirtualNetwork.azuredeploy.json
[3]: https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fmspnp%2Freference-architectures%2Fmaster%2Fguidance-compute-n-tier-sql%2Fworkload.azuredeploy.json
[4]: https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fmspnp%2Freference-architectures%2Fmaster%2Fguidance-compute-n-tier-sql%2Fsecurity.azuredeploy.json