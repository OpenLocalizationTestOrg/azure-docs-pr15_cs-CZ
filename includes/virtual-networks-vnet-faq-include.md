## <a name="virtual-network-basics"></a>Základní informace o virtuální sítě

### <a name="what-is-an-azure-virtual-network-vnet"></a>Co je Azure virtuální sítě (VNet)?

Můžete použít VNets trvat a spravovat virtuální privátní sítě (VPN) v Azure a, případně odkaz VNets s jiných VNets v Azure nebo se vaše místní IT infrastrukturu můžete vytvářet hybridní nebo více místní řešení. Každý VNet vytvořená má bloku CIDR a jde propojit jiných VNets a místní sítích, dokud není kolidují CIDR bloky. Máte taky ovládacích prvků nastavení serveru DNS pro VNets a segmentace VNet do podsítí.

Použití VNets na:

- Vytvoření vyhrazené soukromé jen cloudu virtuální sítě

    Někdy nevyžadují křížově místní konfigurace řešení pro. Při vytváření VNet vašich služeb a VMs v rámci vašeho VNet můžete přímo a bezpečné vzájemně komunikovat v cloudu. Pořád přenosy bezpečně v VNet, ale pořád můžete nakonfigurovat připojení koncového bodu VMs a služby, které vyžadují komunikace po Internetu jako součást vašeho řešení.

- Bezpečné rozšíření datového centra

    S VNets je možné vytvářet tradiční VPN (S2S) na webu bezpečně zobrazit kapacitu datacentra. Virtuální privátní sítě S2S pomocí IPSEC zabezpečené připojení mezi vaší podnikové sítě VPN brány a Azure.

- Povolení hybridní cloudu scénáře

    VNets pružnost na podporu oblasti hybridní cloud scénáře. Můžete zajistila zabezpečené připojení cloudové aplikace libovolný typ místní systém například sálové počítače a systémy Unix.

### <a name="how-do-i-know-if-i-need-a-virtual-network"></a>Jak poznám, pokud potřebuju virtuální sítě?

Navštivte [Virtuální přehled sítě](../articles/virtual-network/virtual-networks-overview.md) , které najdete v článku rozhodnutí tabulky, které vám pomohou při rozhodování, nejlepší volbou návrh sítě pro vás.

### <a name="how-do-i-get-started"></a>Jak mám začít?

Navštivte si přečtěte následující [dokumentaci virtuální sítě](https://azure.microsoft.com/documentation/services/virtual-network/) začít pracovat. Tato stránka obsahuje odkazy na běžné kroky konfigurace i informace, které vám pomohou porozumět věcí, které musíte vzít v úvahu při návrhu virtuální sítě.

### <a name="what-services-can-i-use-with-vnets"></a>Na jaké služby můžete používat s VNets?

VNets lze použít s mnoha různých Azure služby, jako je Cloudovým službám (PaaS), virtuálních počítačích a webových aplikací Web Apps. Můžou ale nastat několik služby, které nejsou podporovány v VNet. Zkontrolujte, zda konkrétní službu, kterou chcete použít a ověřte, zda je kompatibilní.

### <a name="can-i-use-vnets-without-cross-premises-connectivity"></a>Můžete použít VNets bez křížově místní připojení?

Ano. Můžete VNet bez použití připojení k webu. Toto je užitečné, pokud chcete spustit řadiče domény a farmách serverů SharePoint v Azure.

## <a name="virtual-network-configuration"></a>Konfigurace virtuální sítě

### <a name="what-tools-do-i-use-to-create-a-vnet"></a>Jaké nástroje pro použít k vytvoření VNet?

Následující nástroje můžete vytvořit nebo konfigurovat virtuální sítě:

- Azure portál (pro classic a správce prostředků VNets).

- Síť konfiguračního souboru (netcfg - pro klasické VNets). Přečtěte si téma [Konfigurace virtuální sítě pomocí konfiguračního souboru sítě](../articles/virtual-network/virtual-networks-using-network-configuration-file.md).

- Prostředí PowerShell (pro classic a správce prostředků VNets).

- Azure rozhraní příkazového řádku (pro classic a správce prostředků VNets).

### <a name="what-address-ranges-can-i-use-in-my-vnets"></a>Jaké oblasti adresy můžete použít v mém VNets?

Můžete použít veřejný rozsahy IP adres a všechny adresy IP podle [RFC 1918](http://tools.ietf.org/html/rfc1918).

### <a name="can-i-have-public-ip-addresses-in-my-vnets"></a>Můžete definovat veřejnou IP adres v mém VNets?

Ano. Další informace o veřejných rozsahy IP adres najdete v tématu [veřejnou IP adresu místa v síti virtuální (VNet)](../articles/virtual-network/virtual-networks-public-ip-within-vnet.md). Mějte na paměti, že veřejnou IP adresy nebude přístupný přímo z Internetu.

### <a name="is-there-a-limit-to-the-number-of-subnets-in-my-virtual-network"></a>Je nějak omezený počet podsítě v síti virtuální?

Neexistuje žádná omezení počtu podsítí, které používáte v rámci VNet. Všechny podsítě musí být plně součástí adresní prostor virtuální sítě a neměli překrývat k sobě navzájem.

### <a name="are-there-any-restrictions-on-using-ip-addresses-within-these-subnets"></a>Nejdou nějaké omezení použití IP adres v těchto podsítí?

Azure si vyhrazuje některé IP adres v rámci každé podsítě. První a poslední IP adres podsítí rezervováno pro protokol shodu, spolu s 3 Další adresy používané služby Azure.

### <a name="how-small-and-how-large-can-vnets-and-subnets-be"></a>Jak malých a velkých jak můžete být VNets a podsítí?

Nejmenší podsítě, kterou podporujeme je /29 a mřížku nejřidší /8 (pomocí CIDR podsítě definice).

### <a name="can-i-bring-my-vlans-to-azure-using-vnets"></a>Se dají přenést svůj VLAN na Azure pomocí VNets?

Ne. VNets jsou překrytí Layer 3. Azure nepodporuje všechny sémantiku vrstvy-2.

### <a name="can-i-specify-custom-routing-policies-on-my-vnets-and-subnets"></a>Můžete zadat vlastní zásady směrování na Moje VNets a podsítí?

Ano. Můžete uživatel definované směrování (UDR). Další informace o UDR Navštěvujte blog o [směruje definované uživatele a IP přesměrování](../articles/virtual-network/virtual-networks-udr-overview.md).

### <a name="do-vnets-support-multicast-or-broadcast"></a>Podporují VNets vícesměrové nebo všesměrové vysílání?

Ne. Nepodporujeme vícesměrové nebo všesměrové vysílání.

### <a name="what-protocols-can-i-use-within-vnets"></a>K čemu protokoly můžete používat v rámci VNets?

Můžete použít standardní protokoly IP v rámci VNets. Však vícesměrového vysílat, IP v IP formát pakety a obecný směrování zapouzdření (Přenášet) pakety jsou blokovány v rámci VNets. Standardní protokolů, které fungují, patří:

- TCP
- UDP
- ICMP

### <a name="can-i-ping-my-default-routers-within-a-vnet"></a>Můžu pomocí příkazu ping Moje výchozí směrovači v rámci VNet?

Ne.

### <a name="can-i-use-tracert-to-diagnose-connectivity"></a>Dá se pomocí tracert Diagnostika připojení?

Ne.

### <a name="can-i-add-subnets-after-the-vnet-is-created"></a>Můžete přidat po vytvoření VNet podsítí?

Ano. Podsítí můžete přidat VNets kdykoli, dokud adresu podsítě není součástí jiné podsítě v VNet.

### <a name="can-i-modify-the-size-of-my-subnet-after-i-create-it"></a>Můžete změnit velikost Moje podsítě, po jeho vytvoření?

Můžete přidat, odebrat, rozbalte nebo zmenšit podsítě, pokud nejsou žádné VMs nebo služby používaný v ní pomocí rutin prostředí PowerShell nebo NETCFG soubor. Můžete taky přidat, odebrat, rozbalte nebo zmenšit všech směrových čísel, dokud podsítí, které obsahují VMs nebo služby neovlivní změny.

### <a name="can-i-modify-subnets-after-i-created-them"></a>Můžete změnit podsítí, když jsem vytvořil(a)?

Ano. Můžete přidat, odstranit a upravit bloků CIDR používaných VNet.

### <a name="can-i-connect-to-the-internet-if-i-am-running-my-services-in-a-vnet"></a>Můžu připojit k Internetu Pokud používám Moje služby v VNet?

Ano. Všechny služby používaný v rámci VNet se můžete připojit k Internetu. Každý cloudové služby nasazenou v Azure má veřejně s možností zadání VIP přiřazenou. Budete muset definují vstupní koncové body PaaS rolí a koncové body virtuálních počítačích povolení těchto služeb přijímat připojení z Internetu.

### <a name="do-vnets-support-ipv6"></a>Podporují VNets IPv6?

Ne. Protokol IPv6 s VNets nelze použít v současné době.

### <a name="can-a-vnet-span-regions"></a>Můžete VNet rozsahu oblastí?

Ne. VNet se omezí na jedné oblasti.

### <a name="can-i-connect-a-vnet-to-another-vnet-in-azure"></a>Můžete VNet připojit k jiné VNet v Azure?

Ano. Můžete vytvořit VNet VNet komunikace pomocí rozhraní REST API nebo prostředí Windows PowerShell. Taky můžete připojit VNets prostřednictvím prozkoumávání VNet. Další informace o prozkoumávání [zde.](../articles/virtual-network/virtual-network-peering-overview.md)

## <a name="name-resolution-dns"></a>Překládání (DNS)

### <a name="what-are-my-dns-options-for-vnets"></a>Jaké mám možnosti DNS pro VNets?

Umožňuje tabulce rozhodnutí na stránce [Překlad VMs a rolí instance](../articles/virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md) vás provede DNS k dispozici všechny možnosti.

### <a name="can-i-specify-dns-servers-for-a-vnet"></a>Můžete zadat servery DNS pro VNet?

Ano. V nastavení VNet určit DNS server IP adres. Tím se použije jako servery DNS výchozí pro všechny VMs v VNet.

### <a name="how-many-dns-servers-can-i-specify"></a>Kolik servery DNS můžete zadat?

Můžete zadat až 12 serverů DNS.

### <a name="can-i-modify-my-dns-servers-after-i-have-created-the-network"></a>Můžete upravit servery DNS, když jste vytvořili v síti?

Ano. Kdykoliv můžete změnit seznam serverů DNS pro váš VNet. Pokud změníte seznamu serveru DNS, bude potřeba restartovat všech VMs ve vaší VNet je vystopovat nový DNS server.


### <a name="what-is-azure-provided-dns-and-does-it-work-with-vnets"></a>Co je Azure-za předpokladu, že DNS a to funguje s VNets?

Je to DNS Azure-za předpokladu, že služba DNS více klienta nabízená Microsoft. Azure zaregistruje všech instancí rolí a VMs v této službě. Tato služba poskytuje překlad hostname VMs a rolí instancí obsažené v rámci stejné cloudové služby tak plně kvalifikovaný název domény pro VMs a instancí role ve stejném VNet.

> [AZURE.NOTE] Je v současné době první 100 cloudových služeb v virtuální sítě pro klienta křížově překlad pomocí Azure-za předpokladu, že DNS. Pokud používáte serveru DNS, toto omezení, nebudou mít vliv.

### <a name="can-i-override-my-dns-settings-on-a-per-vm--service-basis"></a>Můžete změnit nastavení DNS na za-virtuálního počítače / služby základna?

Ano. Servery DNS můžete nastavit na základě za cloudové služby přepsat výchozí nastavení sítě. Ale doporučujeme používat co nejvíce celé sítě DNS.

### <a name="can-i-bring-my-own-dns-suffix"></a>Můžete přenést svého vlastního přípony DNS?

Ne. Nelze zadat vlastní přípony DNS pro vaše VNets.

## <a name="vnets-and-vms"></a>VNets a VMs

### <a name="can-i-deploy-vms-to-a-vnet"></a>Můžete nasadit VMs u VNet?

Ano.

### <a name="can-i-deploy-linux-vms-to-a-vnet"></a>Můžete nasadit Linux VMs u VNet?

Ano. Můžete nasadit všechny distro Linux nepodporuje Azure.

### <a name="what-is-the-difference-between-a-public-vip-and-an-internal-ip-address"></a>Jaký je rozdíl mezi veřejné VIP a interní IP adresu?

- Vnitřní IP adresu je IP adresa přiřazená každý OM v rámci VNet DHCP. Není veřejných webových. Pokud jste vytvořili VNet, vnitřní IP adresa přiřazena z oblasti, která jste zadali v nastavení podsítě vaší VNet. Pokud nemáte VNet, vnitřní IP adresu stále přiřazeno. Vnitřní IP adresu zůstane s OM k životnosti, pokud, OM odebrána.

- Veřejné VIP je veřejnou IP adresu přiřazená vaše cloudové služby nebo k načtení vyrovnávání. Není přiřazen přímo do vaší OM NIC. VIP zůstane s cloudové služby, které přidělený do všech VMs v, že dojde ke cloudové služby uvolnit nebo odstranění. Uvolnění daném okamžiku.

### <a name="what-ip-address-will-my-vm-receive"></a>K čemu IP adresu Moje OM, dostanou?

- **Vnitřní IP adresa –** Pokud VNet nasazení virtuálního počítače, OM obdrží interní IP adresu z fondu interní IP adresy, které zadáte. VMs komunikovat v VNets pomocí interní IP adres. I když Azure přiřadí dynamické interní IP adresu, můžete požádat o statickou adresu pro vaše OM. Další informace o statický interní IP adresy, navštěvujte blog o [tom, jak nastavit statické IP vnitřní](../articles/virtual-network/virtual-networks-reserved-private-ip.md).

- **Virtuální-** Vaše OM přidružen taky VIP, i když VIP nikdy přiřazen OM přímo. VIP je veřejnou IP adresu, přiřazené cloudové služby. Rezervovat VIP volitelně cloudové službě.

- **ILPIP-** Můžete taky nakonfigurovat úrovni instance veřejnou IP adresu (ILPIP). ILPIPs jsou přímo přidružené OM, nikoli cloudovou službu. Další informace o ILPIPs, navštěvujte blog o [Úrovni Instance veřejnou IP přehled](../articles/virtual-network/virtual-networks-instance-level-public-ip.md).

### <a name="can-i-reserve-an-internal-ip-address-for-a-vm-that-i-will-create-at-a-later-time"></a>Můžete rezervovat interní IP adresu pro OM, které můžu vytvořit později?

Ne. Nelze rezervovat interní IP adresu. Pokud je k dispozici interní IP adresu ho přiřadíte OM nebo role instanci serverem DHCP. Tento může nebo nemusí být ten, který chcete přiřadit k interním IP adresu. Vnitřní IP adresu již byly vytvořeny OM můžete, ale změnit všechny dostupné interní IP adresu.

### <a name="do-internal-ip-addresses-change-for-vms-in-a-vnet"></a>Proveďte interní změn IP adres pro VMs v VNet?

Ano. Vnitřní IP adresy dobu s OM životnosti Pokud OM odebrána. Při uvolnit virtuálního počítače interní IP adresu vydání, pokud jste definovali statické interní IP adresu pro vaše OM. Pokud OM je jednoduše zastavit (a nikoli umístění v poli Stav **Zastaveno (Deallocated)**) IP adresa zůstane přiřazen bude v angličtině.

### <a name="can-i-manually-assign-ip-addresses-to-nics-in-vms"></a>Můžu ručně přiřazovat IP adresy nic v VMs?

Ne. Všechny rozhraní vlastnosti VMs nesmí změnit. Všechny změny může vést k potenciálně ztrátě připojení bude v angličtině.

### <a name="what-happens-to-my-ip-addresses-if-i-shut-down-a-vm"></a>Co se stane IP adresy, pokud můžu vypnout virtuálního počítače?

Nic. IP adresy (veřejné VIP i vnitřní IP adresa) zůstanou se cloudové služby nebo OM.

> [AZURE.NOTE] Pokud chcete jednoduše vypnout OM, nepoužívejte portálu pro správu k tomu nevyzve. Tlačítko Vypnout bude v současné době zrušit virtuální počítač.

### <a name="can-i-move-vms-from-one-subnet-to-another-subnet-in-a-vnet-without-re-deploying"></a>Můžu přejít VMs z jedné podsítě do jiné podsítě v VNet bez znovu nasazení?

Ano. Další informace najdete [tady](../articles/virtual-network/virtual-networks-move-vm-role-to-subnet.md).

### <a name="can-i-configure-a-static-mac-address-for-my-vm"></a>Můžete nakonfigurovat statickou MAC adresu pro svůj OM?

Ne. Adresa MAC se nedají konfigurovat staticky.

### <a name="will-the-mac-address-remain-the-same-for-my-vm-once-it-has-been-created"></a>Adresa MAC zůstane pro svůj OM po jeho vytvoření?

Ano, adresu MAC zůstane pro virtuálního počítače i když OM zastavil (zrušeny, takže) a relaunched.

### <a name="can-i-connect-to-the-internet-from-a-vm-in-a-vnet"></a>Můžete připojit k Internetu z OM v VNet?

Ano. Všechny služby používaný v rámci VNet se můžete připojit k Internetu. Kromě toho každé cloudové služby nasazenou v Azure má veřejně s možností zadání VIP přiřazenou. Potřebujete definovat vstupní koncové body PaaS rolí a koncové body VMs povolení těchto služeb přijímat připojení z Internetu.

## <a name="vnets-and-services"></a>VNets a služby

### <a name="what-services-can-i-use-with-vnets"></a>Na jaké služby můžete používat s VNets?

Můžete použít jenom pro využití služeb v rámci VNets. Služby výpočetní se omezí na cloudovými službami (role pro web a pracovní) a VMs.

### <a name="can-i-use-web-apps-with-virtual-network"></a>Můžete použít Web Apps s virtuální sítě?

Ano. Můžete nasadit Web Apps uvnitř VNet pomocí pomocného mechanismu řízení (prostředí aplikace služby). Přidání, který se Web Apps můžete bezpečně připojit a přístup k prostředkům v Azure VNet, pokud máte bodu server nakonfigurovaný pro vaše VNet. Další informace najdete v těchto článcích:


- [Vytváření webových aplikací v prostředí aplikace služby](../articles/app-service-web/app-service-web-how-to-create-a-web-app-in-an-ase.md)

- [Integrace virtuální sítě webové aplikace](https://azure.microsoft.com/blog/2014/09/15/azure-websites-virtual-network-integration/)

- [Integrace VNet a hybridní připojení pomocí webové aplikace](https://azure.microsoft.com/blog/2014/10/30/using-vnet-or-hybrid-conn-with-websites/)

- [Integrovat do webových aplikací virtuální sítě Azure](../articles/app-service-web/web-sites-integrate-with-vnet.md)

### <a name="can-i-deploy-cloud-services-with-web-and-worker-roles-paas-in-a-vnet"></a>Můžete nasadit služby cloudu pomocí webové pracovní rolí a (PaaS) v VNet?

Ano. Můžete nasadit služby PaaS v rámci VNets.

### <a name="how-do-i-deploy-paas-roles-to-a-vnet"></a>Jak můžu nasadit PaaS role VNet?

Toho můžete dosáhnout zadáním názvu VNet a mapování /subnet role v části síť konfigurace konfigurace služby. Aktualizujte některé z vaší binární nepotřebujete.

### <a name="can-i-move-my-services-in-and-out-of-vnets"></a>Můžete přesunout Moje služby a odhlášení z VNets?

Ne. Nelze přesunout služby a odhlášení z VNets. Budete muset odstranit a znovu nasadit služby přejdete k jiné VNet.

## <a name="vnets-and-security"></a>VNets a zabezpečení

### <a name="what-is-the-security-model-for-vnets"></a>Co je model zabezpečení pro VNets?

VNets jsou úplně izolovaný od vzájemně a dalších služeb hostované v Azure infrastrukturu. VNet je hranici zabezpečení.

### <a name="can-i-define-acls-or-nsgs-on-my-vnets"></a>Můžete definovat ACL nebo NSGs na můj VNets?

Ne. Nelze propojit ACL nebo NSGs VNets. Však ACL je to možné definovat ve vstupním koncové body pro VMs, které již byly nasazeny VNets a NSGs může být přidružené k podsítí nebo nic.

### <a name="is-there-a-vnet-security-whitepaper"></a>Dá se dokument zabezpečení VNet?

Ano. Můžete si ji [tady](http://go.microsoft.com/fwlink/?LinkId=386611).

## <a name="apis-schemas-and-tools"></a>Rozhraní API, schémata a nástroje

### <a name="can-i-manage-vnets-from-code"></a>Můžete spravovat VNets z kód?

Ano. Rozhraní REST API slouží ke správě VNets a křížově místní připojení. Další informace najdete [tady](http://go.microsoft.com/fwlink/?LinkId=296833).

### <a name="is-there-tooling-support-for-vnets"></a>Existuje nástrojů podpora VNets?

Ano. Prostředí PowerShell a přepínač příkazového řádku nástroje můžete použít pro různé platformy. Další informace najdete [tady](http://go.microsoft.com/fwlink/?LinkId=317721).
