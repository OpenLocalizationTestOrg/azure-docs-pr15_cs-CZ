<properties 
   pageTitle="Možnosti překladu názvů DNS pro Linux VMs v Azure"
   description="Název služby DNS scénáře Linux VMs v Azure IaaS, včetně podle rozlišení, hybridní externí DNS a přenést svůj vlastní DNS server."
   services="virtual-machines"
   documentationCenter="na"
   authors="RicksterCDN"
   manager="timlt"
   editor="tysonn" />
<tags 
   ms.service="virtual-machines-linux"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/19/2016"
   ms.author="rclaus" />

# <a name="dns-name-resolution-options-for-linux-vms-in-azure"></a>Možnosti překladu názvů DNS pro Linux VMs v Azure

Azure služeb překládá názvy DNS název obsahuje ve výchozím nastavení pro všechny VMs obsažená v jedné virtuální sítě. Je možné implementovat vlastní DNS název rozlišení řešení nakonfigurováním služby DNS na Azure hostovanou VMs. V následujících scénářích potřebné informace vyhledat v zvolit, které z nich funguje lépe pro konkrétní situaci.

- [Pokud Azure překlad](#azure-provided-name-resolution)

- [Překlad pomocí serveru DNS](#name-resolution-using-your-own-dns-server) 

Typ překlad, které můžete použít závisí na tom, jak VMs a rolí instance muset vzájemně komunikovat.

**Následující tabulka popisuje scénáře a odpovídající řešení rozlišení název:**

| **Scénář** | **Řešení** | **Přípona** |
|--------------|--------------|----------|
| Překlad mezi role instancí nebo VMs umístěny ve stejné síti virtuální | [Pokud Azure překlad](#azure-provided-name-resolution)| název hostitele nebo plně kvalifikovaný název domény |
| Překlad mezi role instancí nebo VMs umístěné v jiném virtuální sítě | Zákazník Správa přístupových práv servery DNS přesměrování dotazů mezi vnets pro překlad Azure (proxy serveru DNS).  v části [názvů pomocí serveru DNS](#name-resolution-using-your-own-dns-server)| Pouze plně kvalifikovaný název domény |
| Rozlišení místního počítače a služby jmen z role instancí nebo VMs v Azure | Zákazník Správa přístupových práv servery DNS (například místním doménovém, místního jen pro čtení řadiče domény nebo vedlejší DNS synchronizovali pomocí přenosy zóny).  V části [názvů pomocí serveru DNS](#name-resolution-using-your-own-dns-server)|Pouze plně kvalifikovaný název domény |
| Rozlišení Azure názvy hostitelů z místních počítačů | Předávání dotazů zákazníků Správa přístupových práv proxy serveru DNS odpovídajícím vnet, proxy serveru předá dotazů Azure pro překlad. V části [názvů pomocí serveru DNS](#name-resolution-using-your-own-dns-server)| Pouze plně kvalifikovaný název domény |
| Stornovat DNS pro interní IP adresy | [Překlad pomocí serveru DNS](#name-resolution-using-your-own-dns-server) | není k dispozici |

## <a name="azure-provided-name-resolution"></a>Pokud Azure překlad

Spolu s rozlišení veřejné názvy DNS poskytuje Azure interní překlad VMs a rolí instance, které se nacházejí v rámci stejné virtuální sítě.  V procesor arm virtuální přípony DNS odpovídá virtuální síti (abyste není potřeba FQDN) a názvy DNS můžete přidělovat nic a VMs. Ačkoli Azure-za předpokladu, že překlad nevyžaduje žádnou konfiguraci, není požadovaný všechny scénářů nasazení, jak je vidět v předchozí tabulce.

### <a name="features-and-considerations"></a>Funkce a co byste měli zvážit

**Funkce:**

- Snadnější použití: není nutná žádná konfigurace používat Azure-za předpokladu, že překlad.

- Pokud Azure překládání neexistuje vysoce, ušetříte potřeba vytvářet a spravovat clusterů serverů DNS.

- Lze spolu s serverů DNS vyřešit místních i Azure názvy hostitelů.

- Překlad není uvedený mezi VMs ve virtuálních sítí bez nutnosti úplný název domény. 

- Můžete použít názvy hostitelů, které nejlépe popisují nasazení vašeho pracovat s automaticky generované názvy.

**Důležité informace:**

- Přípona DNS vytvořili Azure nelze změnit.

- Vlastní záznamy nemůže registrovat ručně.

- Služba WINS a NetBIOS nejsou podporované.

- Názvy hostitelů musí být kompatibilní s prohlížečem DNS (musí používat jenom 0 – 9 z a "-" a nesmí začínat ani končit "-". V části RFC 3696 2.)

- Pro každý OM je omezena přenosu dotazů DNS. To nemá vliv na to, většinou aplikací.  Pokud žádost o omezení je, ujistěte se, že je povolená mezipaměti na straně klienta.  Další informace najdete v tématu [vytěžit maximum z Azure-za předpokladu, že překlad](#Getting-the-most-from-Azure-provided-name-resolution).


### <a name="getting-the-most-from-azure-provided-name-resolution"></a>Maximální využití služby Azure-za předpokladu, že překlad
**Použití mezipaměti klienta:**

Některé DNS dotazu se pošle v síti.  Mezipaměť na straně klienta pomůže snížit latence a zlepšit odolnost blips sítě tak, že řešení opakované DNS dotazy z místní mezipaměti.  DNS záznamy obsahovat Time To Live (TTL) umožňující mezipaměti uložit záznam tak dlouho bez vlivu na záznamu aktuálnost.  Z toho důvodu mezipaměti na straně klienta je vhodný pro většinu situací.

Některé Linux distros nezahrnujte ukládání do mezipaměti ve výchozím nastavení.  Doporučujeme, že přidejte jedno pro každé OM Linux (po kontrola, zda místní mezipaměti už není).

Existuje několik různých DNS mezipaměti balíčků k dispozici, například dnsmasq, tady najdete postup, jak nainstalovat dnsmasq nejběžnější distros:

- **(Používá resolvconf) se systémem Ubuntu**:
    - Nainstalujte balíček dnsmasq ("sudo výstižný získání instalace dnsmasq").
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
    - Přidání "připojte domény názvové servery 127.0.0.1;" k "/etc/dhclient-eth0.conf"
    - Restartujte síťové služby ("služby sítě restart") k nastavení mezipaměti jako místní překládání DNS

> [AZURE.NOTE]: Balíček "dnsmasq" je jenom jedna z mnoha mezipaměť DNS umožňující Linux.  Než ho začnete používat, zkontrolujte jeho vhodnosti pro vašim potřebám a, které máte nainstalované žádné mezipaměti.

**Opakování klienta:**

Je to DNS primárně protokolu UDP.  Jako protokol UDP nezaručuje doručení zprávy, má na starosti Logika opakování v protokol DNS.  Každý klient DNS (systém) vykazovat logika různých opakování podle preferencí tvůrci:

 - Operační systémy Windows opakovat po jednu druhé a potom ještě jednou po jiné dvě, čtyři a jiné čtyř sekund. 
 - Výchozí Linux nastavení opakování po pěti sekundách.  Změňte tento postup opakujte pětkrát intervalech jeden druhé.  

Chcete-li zkontrolovat aktuální nastavení na Linux OM, kočka /etc/resolv.conf a pohled na řádku "možnosti", například:

    options timeout:1 attempts:5

Soubor resolv.conf je automaticky generované a by se neměl editovat.  Přesné kroky pro přidání řádku "možnosti" se liší podle distro:

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
Existuje několik situací, kdy vašim potřebám rozlišení název může dokonalejší funkce za předpokladu, že tak, že Azure, třeba když potřebujete služeb překládá názvy DNS mezi virtuálních sítí (vnets).  Pokrýval tento scénář Azure umožňují použít vlastní serverů DNS.  

Servery DNS v síti virtuální můžete předávat dotazy DNS na Azure je rekurzivní překládání řešení názvy hostitelů v rámci tohoto virtuální sítě.  Například DNS serveru v Azure můžete odpovědět na dotazy DNS pro vlastní soubory zóny DNS a všechny ostatní dotazy předat Azure.  Díky VMs zobrazíte obou položek v zóně soubory a Azure-za předpokladu, že názvy hostitelů (přes předávání).  Přístup k jeho Azure rekurzivní překládání je k dispozici prostřednictvím virtuální IP 168.63.129.16.

Přesměrování DNS také umožňuje mimo vnet služeb překládá názvy DNS a umožňuje počítače místní: Chcete-li vyřešit Azure-za předpokladu, že názvy hostitelů.  Hostname OM vyřešíte DNS server OM musí být umístěny ve stejné síti virtuální a nakonfigurovat tak, aby dopředu hostname dotazů Azure.  Je v každé vnet jiné přípony DNS můžete použít pravidla pro předávání podmíněné odešlete správné vnet překládání DNS dotazů.  Následující obrázek znázorňuje dva vnets a síť místní plnění služeb překládá názvy DNS mimo vnet tímto způsobem:

![DNS mimo vnet](./media/virtual-machines-linux-azure-dns/inter-vnet-dns.png)

Při použití Azure-za předpokladu, že překlad, přípona vnitřní DNS je k dispozici pro každý OM pomocí DHCP.  Pokud chcete použít vlastní název rozlišení řešení, tato přípona není předán VMs vzhledem k tomu, že to narušuje jiným architekturám plochy DNS.  Odkázat na počítačích tak, že plně kvalifikovaný název domény, nebo pro konfiguraci vašeho VMs příponu, přípony možné stanovit pomocí prostředí PowerShell nebo rozhraní API:

-  Řízení zdrojů Azure spravovaných vnets, přípony je k dispozici prostřednictvím zdroje [síťové karty](https://msdn.microsoft.com/library/azure/mt163668.aspx) nebo spuštěním příkazu `azure network public-ip show <resource group> <pip name>` zobrazit podrobnosti o svoji veřejnou IP, včetně plně kvalifikovaný název domény síťovou    


Pokud předávání dotazů Azure nemá vašim potřebám, budete muset zadat vlastní DNS řešení.  Řešení DNS vyžaduje:

-  Zadejte příslušný rozlišení názvů, například prostřednictvím [DDNS](../virtual-network/virtual-networks-name-resolution-ddns.md).  Osobní zprávu, pokud pomocí DDNS budete muset zakázat úklid záznamů DNS na Azure DHCP zapůjčení jsou velmi dlouhý a čištění předčasné odebrat záznamy DNS. 
-  Překladu odpovídající rekurzivní umožňuje překlad jmen externí domény.
-  Přístupných osobám s postižením (TCP a UDP portů 53) klientů, který slouží a přístup k Internetu.
-  Zabezpečit před přístupem z Internetu a zmírnění hrozeb představované externí agentů.

> [AZURE.NOTE] Pro dosažení nejlepších výsledků při použití Azure VMs jako servery DNS, by měly být zakázány IPv6 a [Veřejnou IP úrovni Instance](../virtual-network/virtual-networks-instance-level-public-ip.md) by měly být přiřazeny na všechny servery DNS OM.  

