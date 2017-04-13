<properties 
   pageTitle="Rozlišení VMs a instancí rolí"
   description="Pojmenování rozlišení scénáře pro Azure IaaS hybridní řešení mezi jiné cloudové služby, služby Active Directory a pomocí serveru DNS "
   services="virtual-network"
   documentationCenter="na"
   authors="GarethBradshawMSFT"
   manager="carmonm"
   editor="tysonn" />
<tags 
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="08/31/2016"
   ms.author="telmos" />

# <a name="name-resolution-for-vms-and-role-instances"></a>Překlad VMs a instance rolí

Podle toho, jak používat Azure hostovat IaaS PaaS a hybridní řešení budete muset povolit VMs a instance role, které vytvoříte, abyste mohli vzájemně komunikovat. I když tento komunikace lze provést pomocí IP adresy, je jednodušší použít názvy můžete snadno pamatovat, která se nemění. 

Role instance a VMs hostované v Azure potřeby překládal názvy domén interní IP adresy mohou použít jedním ze dvou způsobů:

- [Pokud Azure překlad](#azure-provided-name-resolution)

- [Překlad pomocí serveru DNS](#name-resolution-using-your-own-dns-server) (který může předávat dotazy na servery DNS Azure-za předpokladu, že) 

Typ překlad, které můžete použít závisí na tom, jak VMs a rolí instance muset vzájemně komunikovat.

**Následující tabulka popisuje scénáře a odpovídající řešení rozlišení název:**

| **Scénář** | **Řešení** | **Přípona** |
|--------------|--------------|----------|
| Překlad mezi role instancí nebo VMs umístěny ve stejné cloudové služby nebo virtuální sítě | [Pokud Azure překlad](#azure-provided-name-resolution)| název hostitele nebo plně kvalifikovaný název domény |
| Překlad mezi role instancí nebo VMs umístěné v jiném virtuální sítě | Zákazník Správa přístupových práv servery DNS přesměrování dotazů mezi vnets pro překlad Azure (proxy serveru DNS).  v části [názvů pomocí serveru DNS](#name-resolution-using-your-own-dns-server)| Pouze plně kvalifikovaný název domény |
| Rozlišení místního počítače a služby jmen z role instancí nebo VMs v Azure | Zákazník Správa přístupových práv servery DNS (například místním doménovém, místního jen pro čtení řadiče domény nebo vedlejší DNS synchronizovaném pomocí přenosy zóny).  V části [názvů pomocí serveru DNS](#name-resolution-using-your-own-dns-server)|Pouze plně kvalifikovaný název domény |
| Rozlišení Azure názvy hostitelů z místních počítačů | Předávání dotazů zákazníků Správa přístupových práv proxy serveru DNS odpovídajícím vnet, proxy serveru předá dotazů Azure pro překlad. V části [názvů pomocí serveru DNS](#name-resolution-using-your-own-dns-server)| Pouze plně kvalifikovaný název domény |
| Stornovat DNS pro interní IP adresy | [Překlad pomocí serveru DNS](#name-resolution-using-your-own-dns-server) | není k dispozici |
| Překlad mezi VMs nebo role instance umístěn v jiné cloudové služby, není v síti virtuální| Nejde použít. Propojení mezi VMs a instancí role v jiné cloudové služby nepodporuje mimo virtuální sítě.| není k dispozici |



## <a name="azure-provided-name-resolution"></a>Pokud Azure překlad

Spolu s rozlišení veřejné názvy DNS poskytuje Azure interní překlad VMs a instance role, které se nacházejí ve stejné síti virtuální nebo cloudové služby.  VMs/instancí služby cloudu sdílet stejné přípona DNS (abyste stačí samotný název hostitele), ale v jiné cloudové klasické virtuálních sítí služby mají různé přípony DNS tak, aby plně kvalifikovaný název domény je potřeba k jmen mezi různých cloudovým službám.  V virtuálních sítí v modelu nasazení Správce prostředků přípony DNS je konzistentní přes virtuální sítě (abyste FQDN není potřeba) a názvy DNS můžete přidělovat nic a VMs. Ačkoli Azure-za předpokladu, že překlad nevyžaduje žádnou konfiguraci, není požadovaný všechny scénářů nasazení, jak je vidět v předchozí tabulce.

> [AZURE.NOTE] V případě web a pracovní role taky zpřístupníte interní IP adresy instancí role na základě rolí název a instanci čísla pomocí rozhraní REST API Azure služby správy. Další informace najdete v článku Principy [služby správy REST API](https://msdn.microsoft.com/library/azure/ee460799.aspx).

### <a name="features-and-considerations"></a>Funkce a co byste měli zvážit

**Funkce:**

- Snadnější použití: není nutná žádná konfigurace mohli použít Azure-za předpokladu, že překlad.

- Pokud Azure překládání neexistuje vysoce, ušetříte potřeba vytvářet a spravovat clusterů serverů DNS.

- Lze ve spojení s serverů DNS vyřešit místních i Azure názvy hostitelů.

- Překlad není uvedený mezi role instance/VMs v rámci stejné cloudovou službu bez nutnosti plně kvalifikovaný název domény.

- Překlad není uvedený mezi VMs v virtuální sítích, které používají nasazení modelu správce prostředků, bez nutnosti úplný název domény. Virtuální sítě v modelu klasické nasazení vyžadují FQDN při určování názvy v jiné cloudové služby. 

- Můžete použít názvy hostitelů, které nejlépe popisují nasazení vašeho pracovat s automaticky generované názvy.

**Důležité informace:**

- Přípona DNS vytvořili Azure nelze změnit.

- Vlastní záznamy nemůže registrovat ručně.

- Služba WINS a NetBIOS nejsou podporované. (Nevidíte VMs v Průzkumníkovi.)

- Názvy hostitelů musí být kompatibilní s prohlížečem DNS (musí používat jenom 0 – 9 z a "-" a nesmí začínat ani končit "-". V části RFC 3696 2.)

- Pro každý OM je omezena přenosu dotazů DNS. To nemá vliv na to, většinou aplikací.  Pokud žádost o omezení je, ujistěte se, že je povolená mezipaměti na straně klienta.  Další informace najdete v tématu [vytěžit maximum z Azure-za předpokladu, že překlad](#Getting-the-most-from-Azure-provided-name-resolution).

- Pouze VMs v první 180 cloudových služeb jsou registrované pro každou virtuální síť v modelu klasické nasazení. To neplatí pro virtuální sítě modelů nasazení Správce prostředků.


### <a name="getting-the-most-from-azure-provided-name-resolution"></a>Maximální využití služby Azure-za předpokladu, že překlad
**Použití mezipaměti klienta:**

Některé DNS dotazu musí nechat zasílat v síti.  Mezipaměť na straně klienta pomůže snížit latence a zlepšit odolnost blips sítě tak, že řešení opakované DNS dotazy z místní mezipaměti.  DNS záznamy obsahovat Time To Live (TTL) umožňující mezipaměti uložit záznam tak dlouho bez vlivu záznamu aktuálnost tak mezipaměti na straně klienta je vhodné pro většinu situací.

Výchozí klient DNS Windows má mezipaměť DNS předdefinované.  Některé Linux distros nezahrnujte ukládání do mezipaměti ve výchozím nastavení se doporučuje, že jeden vkládá každý v angličtině Linux (Kontrola, zda místní mezipaměti už není).

Existuje několik různých DNS mezipaměti balíčků k dispozici, například dnsmasq, tady najdete postup, jak nainstalovat dnsmasq nejběžnější distros:

- **(Používá resolvconf) se systémem Ubuntu**:
    - pouze instalace balíček dnsmasq ("sudo výstižný získání instalace dnsmasq").
- **SUSE (používá netconf)**:
    - instalace balíček dnsmasq ("sudo zypper nainstalovat dnsmasq") 
    - Povolte službu dnsmasq ("systemctl povolit dnsmasq.service") 
    - spustíte službu dnsmasq ("systemctl start dnsmasq.service") 
    - Úprava "/ atd/sysconfig/sítě a konfigurace" a změňte NETCONFIG_DNS_FORWARDER = "" k "dnsmasq"
    - aktualizace resolv.conf ("netconfig aktualizaci") nastavení mezipaměti jako místní překládání DNS
- **OpenLogic (používá NetworkManager)**:
    - instalace balíček dnsmasq ("sudo yum nainstalovat dnsmasq")
    - Povolte službu dnsmasq ("systemctl povolit dnsmasq.service")
    - spustíte službu dnsmasq ("systemctl start dnsmasq.service")
    - Přidání "Připojte-servery DNS 127.0.0.1;" k "/etc/dhclient-eth0.conf"
    - Restartujte síťové služby ("služby sítě restart") k nastavení mezipaměti jako místní překládání DNS

> [AZURE.NOTE] Balíček "dnsmasq" je jenom jedna z mnoha mezipaměť DNS umožňující Linux.  Než ho začnete používat, zkontrolujte jeho vhodnosti pro vašim potřebám a, které máte nainstalované žádné mezipaměti.

**Opakování klienta:**

Je to DNS primárně protokolu UDP.  Jako protokol UDP nezaručuje doručení zprávy, má na starosti Logika opakování v protokol DNS.  Každý klient DNS (systém) vykazovat logika různých opakování podle preferencí tvůrci:

 - Operační systémy Windows opakovat po 1 druhé a potom ještě jednou za jiný 2, 4 a jiné 4 sekundy. 
 - Výchozí Linux nastavení opakování po 5 sekund.  Doporučuje se tento postup opakujte 5 číslem intervalech 1 druhého změnit.  

Kontrola aktuální nastavení na Linux OM, kočka /etc/resolv.conf a pohled na řádku "možnosti", například:

    options timeout:1 attempts:5

Soubor resolv.conf je obvykle automaticky generované a by se neměl editovat.  Přesné kroky pro přidání řádku "možnosti" se liší podle distro:

- **Se systémem Ubuntu** (používá resolvconf):
    - Přidání řádku možnosti "/ etc/resolveconf/resolv.conf.d/head" 
    - spuštění "resolvconf -a" aktualizovat
- **SUSE** (používá netconf):
    - Přidání "timeout:1 pokusy: 5" NETCONFIG_DNS_RESOLVER_OPTIONS = "" parametrů v/atd/sysconfig/sítě a konfigurace 
    - spuštění netconfig update aktualizovat
- **OpenLogic** (používá NetworkManager):
    - Přidání "ozvěnu"timeout:1 možnosti pokusy: 5"" "/ etc/NetworkManager/dispatcher.d/11-dhclient" 
    - spuštění "služby sítě restart" aktualizovat

## <a name="name-resolution-using-your-own-dns-server"></a>Překlad pomocí serveru DNS
Pokud vašim potřebám rozlišení název může přejít za funkcí poskytovaná službou Azure, například při používání služby Active Directory domén nebo pokud požadujete služeb překládá názvy DNS mezi virtuálních sítí (vnets) existuje celá řada situací.  Pokrýval podobnému sledu Azure umožňují použít vlastní serverů DNS.  

Servery DNS v síti virtuální můžete předávat dotazy DNS na Azure je rekurzivní překládání řešení názvy hostitelů v rámci tohoto virtuální sítě.  Například domény řadiče domény (Datacentrum) spuštěné v Azure můžete odpovědět na dotazy DNS své domény a všechny ostatní dotazy předat Azure.  Díky VMs zobrazíte místních zdrojů (přes řadiče domény) a Azure-za předpokladu, že názvy hostitelů (přes předávání).  Přístup k jeho Azure rekurzivní překládání je k dispozici prostřednictvím virtuální IP 168.63.129.16.

Přesměrování DNS také umožňuje mimo vnet služeb překládá názvy DNS a umožňuje počítače místní: Chcete-li vyřešit Azure-za předpokladu, že názvy hostitelů.  Server DNS OM za účelem vyřešení hostname OM, musí být umístěny ve stejné síti virtuální a nakonfigurovat tak, aby dopředu hostname dotazů Azure.  Je v každé vnet jiné přípony DNS můžete použít pravidla pro předávání podmíněné odešlete správné vnet překládání DNS dotazů.  Následující obrázek znázorňuje dva vnets a síť místní plnění služeb překládá názvy DNS mimo vnet tímto způsobem.  Předávání DNS příkladu je k dispozici v [galerii šablon Azure rychlý úvod](https://azure.microsoft.com/documentation/templates/301-dns-forwarder/) a [GitHub](https://github.com/Azure/azure-quickstart-templates/tree/master/301-dns-forwarder).

![DNS mimo vnet](./media/virtual-networks-name-resolution-for-vms-and-role-instances/inter-vnet-dns.png)

Při použití Azure-za předpokladu, že překlad, přípona vnitřní DNS (*. internal.cloudapp.net) je k dispozici pro každý OM pomocí DHCP.  Díky rozlišení názvů jsou v zóně internal.cloudapp.net záznamů název hostitele.  Pokud chcete použít vlastní název rozlišení řešení, přípony IDNS není předán VMs vzhledem k tomu, že to narušuje jiným architekturám plochy DNS (třeba doméně scénáře).  Místo toho nabízíme zástupného symbolu nefunkční (reddog.microsoft.com).  

V případě potřeby přípona vnitřní DNS možné stanovit pomocí prostředí PowerShell nebo rozhraní API:

-  Virtuální sítě modelů nasazení Správce prostředků je k dispozici prostřednictvím zdroje [síťové karty](https://msdn.microsoft.com/library/azure/mt163668.aspx) nebo pomocí rutiny [Get-AzureRmNetworkInterface](https://msdn.microsoft.com/library/mt619434.aspx) přípony.    
-  V klasickém nasazení modelů, přípony neexistuje prostřednictvím volání [Získat rozhraní API nasazení](https://msdn.microsoft.com/library/azure/ee460804.aspx) nebo pomocí [Get AzureVM-ladění](https://msdn.microsoft.com/library/azure/dn495236.aspx) rutiny.


Pokud předávání dotazů Azure nemá vašim potřebám, musíte zadat vlastní DNS řešení.  Řešení DNS bude potřebovat:

-  Zadejte příslušný rozlišení názvů, například prostřednictvím [DDNS](virtual-networks-name-resolution-ddns.md).  Osobní zprávu, pokud pomocí DDNS budete muset zakázat úklid záznamů DNS na Azure DHCP zapůjčení jsou velmi dlouhý a čištění předčasné odebrat záznamy DNS. 
-  Překladu odpovídající rekurzivní umožňuje překlad jmen externí domény.
-  Přístupných osobám s postižením (TCP a UDP portů 53) klientů, který slouží a přístup k Internetu.
-  Zabezpečit před přístupem z Internetu a zmírnění hrozeb představované externí agentů.

> [AZURE.NOTE] Pro dosažení nejlepších výsledků při použití Azure VMs jako servery DNS, by měly být zakázány IPv6 a [Veřejnou IP úrovni Instance](virtual-networks-instance-level-public-ip.md) by měly být přiřazeny na všechny servery DNS OM.  Pokud se rozhodnete sdělit nám serveru Windows Server DNS, [Tento článek](http://blogs.technet.com/b/networking/archive/2015/08/19/name-resolution-performance-of-a-recursive-windows-dns-server-2012-r2.aspx) obsahuje analýzu další výkonu a optimalizace.


### <a name="specifying-dns-servers"></a>Zadání serverů DNS

Pokud chcete použít vlastní servery DNS, Azure umožňuje zadat více serverů DNS na virtuální sítě nebo na síti rozhraní (Správce zdrojů) / cloudových služeb (klasický).  Servery DNS určené pro rozhraní služby/sítě cloudu získat priorit přes uvedené u virtuální sítě.

> [AZURE.NOTE] Vlastnosti připojení sítě, například na server DNS IP adresy, by se neměl editovat přímo v prostředí Windows VMs jako mohou získat vymazání při poskytování služby retušovat po získá nahrazení virtuální síťový adaptér. 


Při použití nasazení modelu správce prostředků, může být zadán servery DNS v portálu rozhraní API/šablony ([vnet](https://msdn.microsoft.com/library/azure/mt163661.aspx), [nic](https://msdn.microsoft.com/library/azure/mt163668.aspx)) nebo Powershellu ([vnet](https://msdn.microsoft.com/library/mt603657.aspx), [nic](https://msdn.microsoft.com/library/mt619370.aspx)).

Při použití klasické nasazení modelu, servery DNS pro virtuální síť, může být zadán v portálu nebo v [souboru *Konfigurace sítě* ](https://msdn.microsoft.com/library/azure/jj157100).  Cloudové služby byla vyplněná servery DNS prostřednictvím [ *Služby konfiguračního* souboru](https://msdn.microsoft.com/library/azure/ee758710) nebo v prostředí PowerShell ([AzureVM nový](https://msdn.microsoft.com/library/azure/dn495254.aspx)).

> [AZURE.NOTE] Pokud změníte nastavení serveru DNS pro virtuální sítě/virtuální počítač, který je již nainstalovali, musíte restartovat každý problémového OM změny se projeví.


## <a name="next-steps"></a>Další kroky

Správce prostředků nasazení modelu:

- [Vytvoření nebo aktualizace virtuální sítě](https://msdn.microsoft.com/library/azure/mt163661.aspx)
- [Vytvoření nebo aktualizace síťová karta](https://msdn.microsoft.com/library/azure/mt163668.aspx)
- [Nové AzureRmVirtualNetwork](https://msdn.microsoft.com/library/mt603657.aspx)
- [Nové AzureRmNetworkInterface](https://msdn.microsoft.com/library/mt619370.aspx)

 
Klasický nasazení modelu:

- [Schéma konfigurace služby Azure](https://msdn.microsoft.com/library/azure/ee758710)
- [Schéma konfigurace virtuální sítě](https://msdn.microsoft.com/library/azure/jj157100)
- [Konfigurace virtuální sítě pomocí konfiguračního souboru sítě](virtual-networks-using-network-configuration-file.md) 

