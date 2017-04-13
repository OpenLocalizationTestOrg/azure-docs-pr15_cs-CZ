
<properties
   pageTitle="Prozkoumávání Azure virtuální sítě | Microsoft Azure"
   description="Informace o VNet prozkoumávání v Azure."
   services="virtual-network"
   documentationCenter="na"
   authors="NarayanAnnamalai"
   manager="jefco"
   editor="tysonn" />
<tags
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/17/2016"
   ms.author="narayan" />

# <a name="vnet-peering"></a>Prozkoumávání VNet

Prozkoumávání VNet je mechanismus, který propojuje dva virtuální sítí (VNets) ve stejné oblasti prostřednictvím Azure páteřní síti. Jakmile peered, dvě virtuální sítě se zobrazí jako jeden z důvodů všechna připojení. Stále je spravováno jako samostatný zdroje, ale virtuálních počítačích v těchto virtuální sítě můžou vzájemně komunikovat přímo pomocí soukromé IP adres.

Přenos mezi virtuálních počítačích v peered virtuální sítě je směrovaná přes Azure infrastruktury podobně jako směrovat přenosy mezi VMs ve stejné síti virtuální. Výhody používání VNet prozkoumávání patří:

- Latence minimum, maximum šířkou pásma připojení mezi zdrojů v různých virtuální sítě.
- Možnost pro použití při přenosu šifrovaná bodů peered VNet zdroje, jako jsou spotřebiče sítě a VPN brány.
- Možnost připojení virtuální síti, která používá správce prostředků Azure model virtuální síti, která používá modelu klasické nasazení a povolte úplné propojení mezi zdrojů v těchto virtuální sítě.

Požadavky a klíčové aspekty prozkoumávání VNet:

- Dvě virtuální sítě, které jsou peered by měl být stejný Azure oblasti.
- Virtuální sítě, které jsou peered měli nepřekrývající IP adresu mezery.
- Prozkoumávání VNet je mezi dvěma virtuálních sítí a žádný odvozené přenosné vztah. Například se pokud virtuální sítě A peered s virtuální sítě B, a pokud virtuální sítě B je peered s virtuální sítě C, ho není přeložit do virtuálního sítě, peered s virtuální sítě C.
- Prozkoumávání lze vytvořit mezi virtuálních sítí ve dvou různých předplatných jak dlouho privilegovaných uživatelů z obou předplatných povolí prozkoumávání a předplatným jsou spojeny do stejné tenanta služby Active Directory. 
- Prozkoumávání mezi virtuální sítě v modelu správce zdrojů a klasické nasazení modelu vyžaduje, aby VNets by měl být v rámci stejného předplatného.
- Virtuální síti, která používá správce prostředků nasazení modelu můžete peered s jinou virtuální sítě, který používá tento model nebo s virtuální sítě, který využívá modelu klasické nasazení. Virtuální sítích, které používají modelu klasické nasazení nelze však peered vzájemně.
- Když komunikace mezi virtuálních počítačích v peered virtuální sítě má bez omezení další šířku pásma, šířku pásma zakončení na základě velikosti OM pořád slouží k použití.


![Základní prozkoumávání VNet](./media/virtual-networks-peering-overview/figure01.png)

## <a name="connectivity"></a>Připojení
Po dvou virtuální sítě jsou peered, virtuálního počítače (webu nebo pracovního role) ve virtuální sítě můžete přímo připojit pomocí dalších virtuálních počítačích v peered virtuální sítě. Tyto dvě sítě mají úplnou IP úroveň připojení.

Latence sítě pro doby odezvy mezi dvěma virtuálních počítačích v peered virtuální sítě je stejná jako doby odezvy v rámci místní virtuální sítě. Výkon sítě je založena na šířku pásma umožňující poměru k velikosti virtuálního počítače. Není k dispozici žádné další omezení šířky pásma.

Přenos mezi virtuálních počítačích v peered virtuálních sítí směrován přímo prostřednictvím Azure infrastruktury back-end a ne brány.

Virtuálních počítačích v síti virtuální přístup k interním Vyrovnávání zatížení (ILB) koncové body v peered virtuální sítě. Skupiny zabezpečení síti (NSGs) se dají použít v obou virtuální sítě zablokovat přístup k jiné virtuální sítě nebo podsítí podle potřeby.

Když uživatelé nakonfigurovat prozkoumávání, jejich otevření nebo zavření pravidla NSG mezi virtuální sítě. Pokud uživatel vybere otevřete celou propojení mezi peered virtuálních sítí (což je výchozí možnost), potom použitím NSGs na konkrétní podsítě nebo virtuálních počítačích blokování nebo odepřít konkrétní přístup.

Pokud Azure interní DNS překlad virtuálních počítačích nefunguje peered virtuální sítích. Virtuálních počítačích mají interní názvy DNS, které jsou možností vyřešení pouze v rámci místní virtuální sítě. Uživatelé však můžete nakonfigurovat virtuálních počítačích spuštěné v peered virtuálních sítí jako servery DNS pro virtuální sítě.

## <a name="service-chaining"></a>Služba zřetězení
Uživatele můžete nakonfigurovat uživatelem definovaných směrování tabulkami přejděte na virtuálních počítačích v peered virtuální sítě jako "přesměrování" IP adresu, jak je vidět v diagramu dál v tomto článku. To umožňuje uživatelům dosažení služby zřetězení, pomocí kterých budou směrování adres z jedné sítě virtuální virtuální zařízení, na kterém běží v síti peered virtuální pomocí uživatelem definovaných směrování tabulek.

Uživatelé mohou také efektivně vytvářet střed a paprsek typ prostředí kde centru můžete hostovat součástí infrastruktury například virtuální zařízení sítě. Všechny virtuální sítě paprsek můžete pak druhé strany se ho, jakož i podmnožinu umožnění datových přenosů do zařízení, která běží v centrální virtuální sítě. Stručně řečeno, VNet prozkoumávání umožňuje další směrování IP adresu na "uživatelem definované směrování tabulky" je IP adresa virtuálního počítače v peered virtuální sítě.

## <a name="gateways-and-on-premises-connectivity"></a>Brány a místní připojení
Každý virtuální sítě, bez ohledu na to, zda je peered s jinou virtuální sítě, můžete pořád mít vlastní brány a použijte ji k připojení k místní. Uživatele můžete taky nakonfigurovat [VNet VNet připojení](../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md) brány, i když jsou peered virtuální sítě.

Pokud jsou nakonfigurovány obou možnostech dopracování virtuální sítě, přenos mezi virtuálních sítí prochází peering konfigurace (to znamená Azure páteřní).

Když jsou peered virtuálních sítí, uživatelé mohou také konfigurovat brány v peered virtuální sítě jako na bod při přenosu šifrovaná místní. V tomto případě virtuální síť, která používá bránu vzdálené barvou své bráně. Jeden virtuální sítě můžou obsahovat jedinou brány. Může to být brána místní nebo vzdálené brány (v peered virtuální síti), jak je znázorněno na následujícím obrázku.

Při přenosu šifrovaná brány není podporovaná peering vztah mezi virtuální sítí pomocí Správce prostředků modelu a můžou být pomocí klasické nasazení modelu. Obě virtuálních sítí peering vztah muset nasazení modelu správce prostředků pro brány dopravní práce.

Když jsou peered virtuální sítě, které sdílíte jednoho připojení Azure ExpressRoute přenosy mezi nimi prochází peering relace (to znamená prostřednictvím Azure páteřní síti). Uživatele můžete dál používat místní bran v jednotlivých virtuální sítě se připojit k místní okruh. Můžete taky můžete použít bránu sdílené a konfigurace při přenosu šifrovaná místní připojení.

![Prozkoumávání dopravní VNet](./media/virtual-networks-peering-overview/figure02.png)

## <a name="provisioning"></a>Zřízení
Prozkoumávání VNet je privilegovaných operace. Je samostatná funkce podle oboru VirtualNetworks. Uživatel může být zadán určitého práva k ověření prozkoumávání. Uživatel, který má přístup pro čtení i zápis k virtuální sítě zdědí tato práva automaticky.

Uživatel, který je buď správce nebo jiný uživatel peering možnost můžete spustit operaci peering na jiné VNet. Pokud je odpovídající žádost o prozkoumávání na druhé straně, a pokud jsou splněny ostatní požadavky prozkoumávání zřídí.

Naleznete v článcích v části "Další kroky" zobrazíte další informace o tom, jak vytvořit VNet prozkoumávání mezi dvěma virtuální sítě.

## <a name="limits"></a>Omezení
Existuje omezení počtu peerings povolených pro jednoho virtuální sítě. Další informace naleznete v [Azure sítě omezení](../azure-subscription-service-limits.md#networking-limits) .

## <a name="pricing"></a>Ceny
Prozkoumávání VNet budou zdarma období revize. Po uvolnění, budete moct nominal poplatků zatížení průniku a výstupním, který využívá prozkoumávání. Další informace najdete v příručce [ceny stránky](https://azure.microsoft.com/pricing/details/virtual-network).


## <a name="next-steps"></a>Další kroky
- [Nastavení prozkoumávání mezi virtuální sítě](virtual-networks-create-vnetpeering-arm-portal.md).
- Informace o [NSGs](virtual-networks-nsg.md).
- Informace o [IP předávání a uživatelem definovaných trasy](virtual-networks-udr-overview.md).
