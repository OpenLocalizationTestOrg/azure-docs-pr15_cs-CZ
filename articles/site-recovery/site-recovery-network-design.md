<properties
    pageTitle="Navrhování vaši síťovou infrastrukturu pro obnovení havárie | Microsoft Azure"
    description="Tento článek popisuje sítě navrhování obnovení webu Azure"
    services="site-recovery"
    documentationCenter=""
    authors="prateek9us"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="site-recovery"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="storage-backup-recovery"
    ms.date="09/19/2016"
    ms.author="pratshar"/>

#  <a name="designing-your-network-infrastructure-for-disaster-recovery"></a>Navrhování vaši síťovou infrastrukturu havárie obnovení

Tento článek je směrovány odborníky, kteří jsou zodpovědní za architecting, provádění a podporu nepřerušený a havárie obnovení (BCDR) infrastruktury a kteří chtějí využít Microsoft Azure webu obnovení (automatickým obnovením systému) pro podporu a vylepšení služeb BCDR. Tento dokument popisuje praktické aspektech pro nasazení serveru správce pro System Center virtuálního počítače, klady a zápory natažené podsítí porovnání podsítě převzetí a jak strukturovat havárie obnovení na virtuální weby v Microsoft Azure.

## <a name="overview"></a>Základní informace

[Obnovení automatickým obnovením (Azure webu systému)](https://azure.microsoft.com/services/site-recovery/) je služba Microsoft Azure, která orchestrates protection a obnovení virtualizovaném aplikací pro účely firmy kontinuitu havárie obnovení (BCDR). V tomto dokumentu má průvodce čtenáře vytváření sítích, zaměřené na architecting rozsahy IP adres a podsítí na webu obnovení havárie při replikace virtuálních počítačích (VMs) pomocí obnovení webu.

Kromě toho tento článek ukazuje, jak obnovení webu umožňuje architecting a provádění více pracovišť virtuální datacentra pro podporu služeb BCDR v době test nebo havárie.

Světě tam, kde všichni očekává 24/7 připojení je důležité než kdykoli dřív nepřijít infrastruktury a aplikace a spuštění. Nepřerušený a havárie obnovení (BCDR) slouží k obnovení selhalo součástí tak organizace můžete rychle vrátit k normálně. Vývoj strategie obnovení havárie řešení pravděpodobné, zhoubné událostí je velmi složité. Toto je kvůli inherentní obtížnosti předpovídání budoucnost, zejména jako to se týká nepravděpodobná událostí a vysoké náklady poskytnout adekvátní množství míry ochrany proti dalekosáhlou catastrophes. 

Základem BCDR plánování obnovení čas cíle (RTO) a cíle operace (obnovení čárky RPO) je třeba definovat jako součást plán obnovy havárie. Když selhání odpovídá zákazníka datovém centru, pomocí obnovení webu Azure, zákazníci se dají rychle (zhoršeným RTO) přeneste online replikovanou virtuálních počítačích umístěnou v sekundárního datového centra nebo Microsoft Azure se ztrátou minimální dat (zhoršeným operace RPO). 

Převzetí je nastavená jako možné automatickým obnovením systému, který původně zkopíruje určené virtuálních počítačích z primární datovém centru sekundární datacentrem nebo Azure (podle scénáře) a pravidelně aktualizuje replik. Během infrastruktury plánování, návrh sítě považuje se za potenciální kritický, který můžete zabránit jste ze společnosti schůzky RTO a operace RPO cílů.  

Správci plánujete nasazení řešení obnovení havárie, je při jedním z klíčové otázky v své rozhodnutí jak virtuální počítač bude dostupná po dokončení záložní. Funkci Automatické obnovení systému umožňuje správcům výběr sítě, ke kterému by virtuální počítač připojen k po překlopení. Pokud primární webu se spravuje serveru VMM potom dosahuje se používání mapování sítě. Další podrobnosti najdete v části [připravit na mapování sítě](site-recovery-network-mapping.md) .

Při návrhu síť pro obnovení webů, správce má dvě možnosti:

- Použití různých rozsah IP adres na síť při obnovení webu. V tomto scénáři virtuálního počítače po převzetí pošle nové IP adresa a správce muset udělat aktualizace DNS. Přečtěte si informace o tom, jak se DNS aktualizovat [tady](site-recovery-vmm-to-vmm.md#test-your-deployment) 
- Použití stejné adresy IP na síť při obnovení webu. V některých scénářích raději správcům uchovávat IP adresy, které mají na primární webu i po záložní. V normálním firem správce musel aktualizace postupů k označení nové umístění IP adres. Ale scénář, kde natažené VLAN nasazený mezi hlavní stránku předlohy a obnovení webů, ponechá IP adresy virtuálních počítačích bude atraktivní možnost. Zachování stejné IP adresy zjednodušuje obnovení – stačí kliknout jakákoli síť související příspěvek převzetí kroky.


Správci plánujete nasazení řešení obnovení havárie, jednu z klíčové otázky v jejich paměti při jak aplikace budou dostupná po dokončení záložní. Moderní aplikace závisejí téměř vždy o sítích do určité míry, takže fyzicky přesunutí že webů služby představuje sítě ověřovací kód. Existují dva hlavní způsoby, které tento problém se zabývá havárie obnovení řešení. První přístup je udržovat pevných IP adres. Bez ohledu na služby přesouvání a hostingu servery jsou na různých místech fyzické aplikací trvat konfiguraci adresy IP s nimi do nového umístění. Druhý přístup zahrnuje úplně změnu IP adresu během přechodu do obnoveného webu. Každý přístup má několik provedení změny, které je uveden níže.

Při návrhu síť pro obnovení webů, správce má dvě možnosti:

## <a name="option-1-retain-ip-addresses"></a>Možnost 1: Zachovat IP adres 

Z hlediska havárie obnovení procesů pomocí pevné IP adresy se zdá být nejjednodušší metoda sdílení, ale má počet potenciální problémy, které ve skutečnosti ho nejméně oblíbené přístup. Obnovení Azure webu poskytuje možnosti zachovat IP adresy ve všech případech. Než jednu rozhoduje o uchovávání IP, třeba odpovídající myšlenkových omezení ukládá na převzetí funkce. Dejte nám podívejte faktory, které může pomoct při rozhodování uchovávání IP adresy nebo ne. To dosáhnout dvěma způsoby natažené podsítě, nebo můžete udělat úplné podsítě přepojení.

### <a name="stretched-subnet"></a>Natažené podsítě

Tady je k dispozici současně v primární a umístění DR podsítě. Jednoduše řečeno to znamená serveru a konfigurace IP (Layer 3) můžete přesunout do druhého webu a sítě se provoz směrovat na nové místo automaticky. Toto je důvodu řešení z hlediska serveru, ale má číslo stránky:

- Z hlediska vrstvy 2 (dat odkaz layer) bude vyžadovat síťových zařízení, které můžete spravovat natažené VLAN, ale to stalo menší problém, jako je teď k dispozici. Druhé a obtížnější je to, natažením VLAN potenciální chybu domény se rozšíří do obě lokality v podstatě stává selhání v jednom místě. Když to je pravděpodobně výskyt, se stalo, že vysílání bouře spustili, ale nemůže být izolovaný. Jsme našli smíšených názory problém poslední a zobrazila počet úspěšných implementací stejně jako "jsme nikdy provede této technologie".
- Natažené podsítě není možné v případě, že používáte Microsoft Azure jako web DR.


### <a name="subnet-failover"></a>Převzetí podsítě

Je možné provádět podsítě převzetí získat výhody řešení natažené podsítě jsme je popsali výše bez roztažením podsítě na více webech. Tady dané podsítě by měly tvořit na webu 1 nebo 2 webu, ale nikdy v obou sítích současně. Pro zachování adresní prostor IP v případě selhání, je možné zajistit programově infrastrukturu směrovači k přesunutí podsítí mezi weby. Ve scénáři převzetí, které by podsítí pohyboval spolu s přidružené zamknutí VMs. Hlavní nevýhodou tento přístup je v případě se nepovede, budete muset přesunout celé podsítě, které mohou být OK, ale může ovlivnit aspektech granularity překlopení. 

Podívejme se podrobněji způsob je moct replikovat jeho VMs jinam obnovení při selhání přes celou podsítě fiktivní organizace s názvem Contoso. Bude nejdřív podíváme na způsob je moct spravovat jejich podsítí během replikace VMs mezi dvěma místního umístění a potom bude probereme tato témata podsítě převzetí fungování kdy Contoso [že Azure slouží jako havárie obnovení webu](#failover-to-azure).

#### <a name="failover-to-a-secondary-on-premises-site"></a>Přepnutí vedlejší místních webů

Dejte nám podívejte se na situace pokud chceme zachovat IP všech VMs a před selháním dokončení podsítě společně. Aplikace spuštěné v podsítě 192.168.1.0/24 má primární Web. Důvody chování záložní všechny virtuálních počítačích, které jsou součástí dané podsítě se nezdaří přes web obnovení a zachovat jeho IP adres. Směruje budou muset být řádně podporovat upravit tak, aby odrážela tomu, že všechny virtuálních počítačích patřící do podsítě 192.168.1.0/24 teď přesunuli jste obnovení webu. 

Na následujícím obrázku směruje mezi primární webu a obnovení webů, třetí a primární webu a třetí webu a obnovení webem se bude nutné řádně podporovat změnit. 

Následující obrázky zobrazuje podsítí před záložní. Podsítě 192.168.0.1/24 aktivní na stránce primární před záložní a obnovení webu aktivuje po záložní 

![Před překlopení](./media/site-recovery-network-design/network-design2.png)

Před překlopení


Na následujícím obrázku sítí a podsítí po překlopení.
    
![Po překlopení](./media/site-recovery-network-design/network-design3.png)

Po překlopení

Sekundární webu se místně a používáte VMM server pro správu ho pak při aktivaci ochraně pro konkrétní virtuální počítač, automatickým obnovením systému bude agregovat sítě podle následujících pracovního postupu:

- Funkci Automatické obnovení systému přidělovat IP adresu pro každé síťové rozhraní v počítači virtuální z fondu statických IP adresu definované na příslušné síť pro jednotlivé instance System Center VMM.
- Pokud správce definuje stejné fondu IP adres v síti na webu obnovení jako fondu IP adres sítě primární portálu během rozdělování IP adresu počítače virtuální otevřené automatickým obnovením systému by přidělit stejnou IP adresu jako primární virtuálního počítače.  IP je rezervován v VMM, ale není nastavena jako převzetí IP. Převzetí IP nastavenou těsně před záložní.

![Zachovat IP adresa](./media/site-recovery-network-design/network-design4.png)
    
Obrázek 5

Obrázek 5 zobrazuje převzetí TCP/IP nastavení otevřené virtuálního počítače (v konzole pro Hyper-V). Tato nastavení by vyplněné, stačí před tím, než virtuální počítač po selhání

Pokud stejné IP není k dispozici, by automatickým obnovením systému přidělit některé další dostupné IP adresu z fondu definovaný IP adres. 

Po OM aktivované řešení ochrany můžete sledovat ukázka skriptu pro ověření adresy IP, která byla rozdělena virtuálního počítače. Stejné IP chcete nastavit jako IP převzetí a v čase převzetí přiřazen bude v angličtině:

        $vm = Get-SCVirtualMachine -Name <VM_NAME>
        $na = $vm[0].VirtualNetworkAdapters>
        $ip = Get-SCIPAddress -GrantToObjectID $na[0].id
        $ip.address  

>[AZURE.NOTE] V případě kterém virtuálních počítačích používat DHCP Správa IP adresy úplně je mimo ovládací prvek automatickým obnovením systému. Správce musí zajistit, že server DHCP podávání IP adresy na webu obnovení si můžete vytisknout z na stejnou oblast, která primární webu.

#### <a name="failover-to-azure"></a>Přepnutí do Azure

Azure webu obnovení (funkci Automatické obnovení systému) umožňuje jako web obnovení havárie určené virtuálních počítačích Microsoft Azure. V tomto případě se potřebujete zacházet s jeden další omezení. 

Podívejme se podrobněji situace fiktivní společnost s názvem Woodgrove Bank má místní infrastruktury hostingu jejich aplikací line of business, kde jsou hostingu jejich mobilních aplikací na Azure. Propojení mezi VMs banky Woodgrove v Azure a místních serverů je k dispozici ve webu (S2S) virtuální privátní sítě (VPN). S2S VPN umožňuje virtuální sítě Woodgrove banky v Azure se měly považovat za prodloužení Woodgrove banky v místní síti. Tento komunikace aktivované tak, že S2S VPN mezi Woodgrove Bank okraje a Azure virtuální sítě. Teď Woodgrove chce replikovat jeho pracovní vytížení v jeho datacentra na Azure pomocí automatickým obnovením systému. Tuto možnost vyhovuje Woodgrove, který chce vyplatí možnost DR a ukládat data v prostředí veřejné cloudu. Woodgrove má zacházet s aplikací a uspořádání, které jsou závislé na pevně IP adresy, tedy mají povinnost uchovávat IP adres pro jejich aplikace při přechodu na Azure.

Woodgrove rozhodl přiřadit IP adres z rozsah IP adres (172.16.1.0/24, 172.16.2.0/24) na její zdroje spuštěné v Azure.

Pro Woodgrove mohli replikovat virtuálních počítačích Azure a ponechá IP adresy musí vytvořit virtuální sítě Azure. Rozšíření místních sítě by mělo být tak, že aplikace můžete převezme z místních webů Azure bezproblémové. Azure umožňuje přidat připojení VPN k webu, stejně jako čárky webu virtuální sítě vytvořené v Azure. Při nastavování připojení k webu Azure sítě můžete jenom v případě, že rozsah IP adres se liší od místního rozsah IP adres, protože Azure, které nejsou podporovány roztažení podsítí provoz směrovat na místního pracoviště (Azure volání místní sítě).  To znamená, že máte podsítě 192.168.1.0/24 místní, nemůžete přidat 192.168.1.0/24 místní síti v Azure sítě. Očekává se, protože Azure poslal, neví, podsítě jsou žádné aktivní VMs a že se podsítě vznikne pouze pro účely DR. Abyste mohli správně směrování přenosu z Azure sítě podsítí v síti a v místní síti nesmí být v konfliktu. 

![Před podsítě překlopení](./media/site-recovery-network-design/network-design7.png)

Před překlopení

Aby Woodgrove splnění své firmy požadavky, potřebujeme provádět následující postupy:

- Vytvořit další síť, dejte nám zavolejte ho obnovení sítě, tam, kde by se nepodařilo převrácení virtuálních počítačích vytvořili.
- Abyste měli jistotu, že IP virtuálního počítače zůstane po selhání, přejděte na kartu konfigurovat ve skupinovém rámečku vlastnosti OM, zadejte stejné IP OM má místní a potom klikněte na Uložit. Pokud selhání OM obnovení webu Azure bude přiřadit zadané IP virtuální počítač. 

![Vlastnosti sítě](./media/site-recovery-network-design/network-design8.png)

Po aktivaci záložní a virtuálních počítačích vytvořené v síti obnovení požadované IP, můžete připojení k síti vytvořeno pomocí [Vnet Vnet připojení](../vpn-gateway/virtual-networks-configure-vnet-to-vnet-connection.md). V případě potřeby můžete skriptovány tuto akci.  Jak můžeme bylo popsáno v předchozí části o převzetí podsítě i v případě převezme Azure, směruje musel řádně podporovat upravit tak, aby odpovídaly že této 192.168.1.0/24 má nyní přesune do Azure. 

![Po podsítě překlopení](./media/site-recovery-network-design/network-design9.png)

Po překlopení

Pokud nemáte v síti Azure, jak je vidět na obrázku nahoře. Vytvoření připojení vpn sítěmi mezi "Primární webu" a obnovení sítě po záložní.  


## <a name="option-2-changing-ip-addresses"></a>Možnost 2: Změna IP adres

Tento postup se zdá být nejčastěji rozšířené podle co jsme viděli. Trvá formuláři změny IP adresu každého OM, která je součástí záložní. Vrácení tento postup vyžaduje příchozí sítě naučit ' "aplikace, která byla na IPx je teď v IPy. I v případě IPx a IPy logické názvy, položky DNS obvykle muset změnit nebo vyprázdnění v síti a režim cached položky v tabulkách sítě mají být aktualizovány nebo vyprázdnit, tedy prostoje by se měly považovat podle toho, jak DNS infrastruktury zjištění nastavení. Těmto problémům může zmírnit pomocí nejnižší hodnoty TTL v případě aplikace v síti intranet a pomocí [Správce přenosy Azure s automatickým obnovením systému](http://azure.microsoft.com/blog/2015/03/03/reduce-rto-by-using-azure-traffic-manager-with-azure-site-recovery/) pro internetové na základě aplikace

### <a name="changing-the-ip-addresses---illustration"></a>Změna adresy IP - obrázku

Dejte nám podívejte se na scénáře místo, kam se chystáte používat různé IP adresy přes hlavní stránku předlohy a obnovení webů. V následujícím příkladu jsme také mít u jiného webu z hostitelem aplikace na primární nebo obnovení webů můžete k nim získat přístup.

![Různé IP - před překlopení](./media/site-recovery-network-design/network-design10.png)

Obrázek 11

Obrázek 11 existují některé aplikace použitý ve podsítě 192.168.1.0/24 podsítě na primární webu a byly nakonfigurovány přijít na webu obnovení v podsítě 172.16.1.0/24 po přepojení. Směruje připojení/sítě VPN nakonfigurovány řádně podporovat tak, aby všechny tři weby můžete získat přístup k sobě.
 
Jak zjistit 12 zobrazuje při přechodu myší jeden nebo více aplikací, bude obnoven v podsítě obnovení. V tomto případě jsme není omezena pouze převzetí celý podsítě ve stejnou dobu. Žádné změny musí změnit konfiguraci sítě VPN nebo síť cest. Selhání a některé aktualizace záznamů DNS bude zkontrolujte, že aplikace zůstávají přístupných osobám s postižením. Pokud DNS nakonfigurovaný umožníte dynamické aktualizace potom virtuálních počítačích by zaregistrovat pomocí nového IP zahájenou přepojení. 

![Různé IP – po překlopení](./media/site-recovery-network-design/network-design11.png)

Obrázek 12

Po selhání přes virtuální počítač otevřené pravděpodobně IP adresu, která není totéž jako IP adresu primární virtuálního počítače. Virtuálních počítačích se aktualizuje serveru DNS, který používá po spuštění. Položky DNS obvykle muset změnit nebo vyprázdnění v síti a režim cached položek v tabulkách sítě muset aktualizovat nebo vyprázdnit, tak je běžné před sebou prostoj při proběhnout tyto změny stavu. Tento problém můžete zmírnit tak, že:

- Použití nejnižší hodnoty TTL pro aplikace v síti intranet.
- Pomocí Správce přenosy Azure s automatickým obnovením systému pro internet pomocí aplikace.
- Použití tento skript v plánu obnovení má aktualizovat Server DNS zajistit včasná aktualizace (skript není povinný případě nakonfigurování registrace dynamického serveru DNS)

        string]$Zone,
        [string]$name,
        [string]$IP
        )
        $Record = Get-DnsServerResourceRecord -ZoneName $zone -Name $name
        $newrecord = $record.clone()
        $newrecord.RecordData[0].IPv4Address  =  $IP
        Set-DnsServerResourceRecord -zonename $zone -OldInputObject $record -NewInputObject $Newrecord


### <a name="changing-the-ip-addresses--dr-to-azure"></a>Změna adresy IP – web DR pro Azure

Příspěvek na blog [Síťové infrastruktury nastavení pro Microsoft Azure jako web obnovení havárie](http://azure.microsoft.com/blog/2014/09/04/networking-infrastructure-setup-for-microsoft-azure-as-a-disaster-recovery-site/) vysvětluje, jak nastavit požadované Azure síťové infrastruktury při zachování IP adres není povinné. Začíná popisující aplikace a podívejte se na to, jak nastavit sítě místních a na Azure a potom uzavírání postup přepojení test a plánované přepojení.



## <a name="next-steps"></a>Další kroky

[Přečtěte si víc](site-recovery-network-mapping.md) využití obnovení webu mapy zdrojové a cílové sítě VMM server pro správu primární webu. 
