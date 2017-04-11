<properties
    pageTitle="Základní informace o protokolu IPv6 pro vyrovnávání zatížení Azure | Microsoft Azure"
    description="Principy podpora protokolu IPv6 u Azure nástroj pro vyrovnávání zatížení a vyrovnávání zatížení VMs."
    services="load-balancer"
    documentationCenter="na"
    authors="sdwheeler"
    manager="carmonm"
    editor=""
    keywords="protokol IPv6 Vyrovnávání zatížení azure, duální zásobníku, veřejnou ip, nativní protokol ipv6, mobilní telefon, iot"
/>
<tags
    ms.service="load-balancer"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="infrastructure-services"
    ms.date="09/14/2016"
    ms.author="sewhee"
/>

# <a name="overview-of-ipv6-for-azure-load-balancer"></a>Základní informace o protokolu IPv6 pro vyrovnávání zatížení Azure

Vyrovnávání zatížení internetového můžete nasadit s adresy IPv6. Kromě připojení pomocí protokolu IPv4 díky následující možnosti:

* Nativní začátku do konce IPv6 propojení mezi klienty veřejné sítě Internet a Azure virtuálních počítačích (VMs) prostřednictvím Vyrovnávání zatížení.
* Nativní začátku do konce odchozí připojení pomocí protokolu IPv6 mezi VMs a veřejné klientů s podporou Internetu IPv6.

Následující obrázek znázorňuje funkci IPv6 pro vyrovnávání zatížení Azure.

![Vyrovnávání zatížení Azure pomocí protokolu IPv6](./media/load-balancer-ipv6-overview/load-balancer-ipv6.png)

Po zavedení, klienta IPv4 nebo IPv6 s podporou Internetu komunikovat s veřejné adresy IPv4 nebo IPv6 (nebo názvy hostitelů) z vyrovnávání zatížení Azure internetové. Vyrovnávání zatížení směruje paketů IPv6 na soukromé adresy IPv6 VMs pomocí překládání adres (překladu síťových adres). Přímo s IPv6 address VMs nemůžete komunikovat ani klienta IPv6 přes Internet.

## <a name="features"></a>Funkce

Nativní podpora protokolu IPv6 u VMs nasazených pomocí Správce prostředků Azure obsahuje:

1. Vyrovnávání zatížení služby protokolu IPv6 u klientů protokolu IPv6 na Internetu
2. Nativní IPv6 a IPv4 koncové body VMs ("dva skládaný")
3. Příchozí a odchozí inicializovaný nativní připojení IPv6
4. Podporované protokoly TCP a UDP, HTTP (S) povolit široká řada architekturám služeb plochy

## <a name="benefits"></a>Výhody

Tato funkce umožňuje následující klíčové výhody:

* Zahájit vládní nařízení požadující klientům jen IPv6 přístupný nové aplikace
* Povolit mobile, přičemž Internet vývojářům věcí (IOT) pomocí dvou skládaný (IPv4 + IPv6) Azure virtuálních počítačích adresy rostoucí mobile & IOT trzích

## <a name="details-and-limitations"></a>Podrobnosti a omezení

Podrobnosti

* Služba Azure DNS obsahuje IPv4 A a IPv6 AAAA záznamy názvových a odpoví oba záznamy pro vyrovnávání zatížení. Klient zvolí jakou adresu (IPv4 nebo IPv6) komunikovat.
* Virtuálního počítače iniciuje připojení k veřejné zařízení připojeného k Internetu IPv6, zdroje OM IPv6 address při síťové adresy převést (překladu síťových adres) veřejné adresy IPv6 Vyrovnávání zatížení.
* VMs s operačním systémem Linux musí být nakonfigurované pro získání adresy IPv6 IP prostřednictvím DHCP. V mnoha Linux obrázky v galerii Azure jsou již nakonfigurovány podporuje protokol IPv6 beze změny. Další informace najdete v tématu [Konfigurace DHCPv6 pro Linux VMs](load-balancer-ipv6-for-linux.md)
* Pokud se rozhodnete sdělit nám zkušební stavu s Vyrovnávání zatížení, vytvořte zkušební IPv4 a ho použít s IPv4 a IPv6 koncové body. Pokud služby na svůj OM přejde, IPv4 a IPv6 koncové body přesměrováni mimo otočení.

Omezení

* Na portálu Azure nemůžete přidat pravidla Vyrovnávání zatížení IPv6. Pravidla můžete vytvořit pouze prostřednictvím šablony rozhraní příkazového řádku, Powershellu.
* Existující VMs používat adresy IPv6 pro nemusí upgradovat. Nasadíte nové VMs.
* Jeden IPv6 address můžete přidělovat jedné sítě rozhraní v jednotlivých OM.
* Veřejné adresy IPv6 nelze přiřadit virtuálního počítače. Budou můžete jenom přidělovat Vyrovnávání zatížení.
* Zpětné vyhledávání DNS nejde nakonfigurovat pro veřejné adresy IPv6.
* VMs s adresy IPv6 nemohou být členy cloudové služby Azure. Můžete připojení k síti virtuální Azure (VNet) a vzájemně komunikovat jejich adres protokolu IPv4.
* Adresy IPv6 soukromé může být nasazené na jednotlivé VMs ve skupině zdroje, ale nemůže být nasazené do skupiny zdrojů prostřednictvím sady měřítko.
* Azure VMs nemůžete připojit pomocí protokolu IPv6 u jiných VMs, jiných služeb Azure nebo místní zařízení. Pouze komunikovat s Vyrovnávání zatížení Azure s protokolem IPv6. Však mohli komunikovat s těchto dalších zdrojů pomocí IPv4.
* Ochranu skupiny zabezpečení síti (NSG) pro IPv4 podporují nasazení dvou zásobníku (IPv4 + IPv6). NSGs neplatí pro koncové body IPv6.
* Koncový bod IPv6 v OM, nebude vystaven přímo k Internetu. Je za vyrovnávání zatížení. Pouze porty podle pravidel Vyrovnávání zatížení máte přístup pomocí protokolu IPv6.
* Změna parametrů IdleTimeout protokolu IPv6 **není aktuálně podporován**. Výchozí hodnota je 4 minut.

## <a name="next-steps"></a>Další kroky

Naučte se nasadit Vyrovnávání zatížení pomocí protokolu IPv6.

* [Dostupnost IPv6 podle regionů](https://go.microsoft.com/fwlink/?linkid=828357)
* [Nasazení při vyrovnávání zatížení se IPv6 použít šablonu?](load-balancer-ipv6-internet-template.md)
* [Nasazení při vyrovnávání zatížení pomocí protokolu IPv6 pomocí prostředí PowerShell Azure](load-balancer-ipv6-internet-ps.md)
* [Nasazení při vyrovnávání zatížení pomocí protokolu IPv6 pomocí rozhraní příkazového řádku Azure](load-balancer-ipv6-internet-cli.md)
