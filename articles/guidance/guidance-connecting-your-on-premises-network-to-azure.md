<properties
   pageTitle="Připojení vaší místní síti k Azure | Microsoft Azure"
   description="Tento článek vysvětluje a srovnání různých metod k dispozici pro připojení ke cloudovým službám společnosti Microsoft například Azure, Office 365 a Dynamics CRM Online."
   services=""
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor=""
   tags=""/>
<tags
   ms.service="guidance"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/25/2016"
   ms.author="jdial"/>
   
# <a name="connecting-your-on-premises-network-to-azure"></a>Připojení vaší místní síti k Azure

Microsoft poskytuje několik typů cloudovým službám. Když můžete připojit ke všem službám veřejné Internetu, můžete také připojit k některé z těchto služeb pomocí tunelem virtuální privátní sítě (VPN) přes Internet nebo prostřednictvím přímý a soukromé připojení společnosti Microsoft. Tento článek vám pomůže určit kterou možnost připojení se nejlépe potřebám založené na typech cloudovým službám společnosti Microsoft, které můžete používat. Většina organizací využít více typů připojení píše níže.

## <a name="connecting-over-the-public-internet"></a>Připojení přes veřejnou Internet

Tento typ připojení poskytuje přístup ke cloudovým službám společnosti Microsoft přímo prostřednictvím Internetu, jak je ukázáno v následujícím příkladu.

![Internet] (./media/guidance-connecting-your-on-premises-network-to-azure/internet.png "Internet")

Toto připojení je obvykle první typ používá pro připojení ke cloudovým službám Microsoftu. Následující tabulka uvádí výhody a nevýhody tohoto typu připojení.



| **Výhody**| **Co byste měli zvážit**|
|---------|---------|
|Vyžaduje žádné změny v místní síti, dokud všech klientských zařízení máte neomezený přístup ke všem IP adres a porty na Internetu.|Když data je často šifrovaná pomocí HTTPS, mohou být zachyceny na cestě, protože projde veřejného Internetu.|
|Můžete se připojit ke všem službám cloudu společnosti Microsoft na veřejné Internetu.|Neočekávané latence protože projde připojení k Internetu.|
|Pomocí existujícího připojení k Internetu.||
|Nevyžaduje správy všechna připojení zařízení.||

Toto připojení má bez připojení nebo šířku pásma náklady, protože pomocí existujícího připojení k Internetu. 

## <a name="connecting-with-a-point-to-site-connection"></a>Připojení pomocí připojení čárky webu

Tento typ připojení umožňuje přístup k některé cloudovým službám Microsoftu tunelem Tunneling protokol SSTP (Secure Socket) přes Internet, jak je ukázáno v následujícím příkladu.

![P2S] Připojení k (./media/guidance-connecting-your-on-premises-network-to-azure/p2s.png "bodu webu")

Připojení je volání na váš stávající připojení k Internetu, ale je potřeba použít Brána VPN Azure. Následující tabulka uvádí výhody a nevýhody tohoto typu připojení.

| **Výhody**| **Co byste měli zvážit**|
|---------|---------|
|Vyžaduje žádné změny v místní síti, dokud všech klientských zařízení máte neomezený přístup ke všem IP adres a porty na Internetu.|Když přenosu šifrovaná propojení, mohou být zachyceny na cestě, protože projde veřejného Internetu.|
|Pomocí existujícího připojení k Internetu.|Neočekávané latence protože projde připojení k Internetu.|
|Výkon až 200 Mb/s za brány.|Vyžaduje vytváření a správu samostatná připojení mezi každé zařízení ve vaší místní síti a každý brány, který každého zařízení, musí se připojit k.|
|Slouží k připojení k Azure služby, které jde připojit k virtuální sítím Azure (VNet) například Azure virtuálních počítačích a Azure Cloud Services.|Vyžaduje minimální probíhající správu brány VPN Azure.|
||Nelze se připojit k aplikaci Microsoft Office 365 nebo Dynamics CRM Online.
||Nelze se připojit k Azure služby, které nelze připojit k VNet.|

Další informace o službě [Brána VPN](../vpn-gateway/vpn-gateway-about-vpngateways.md) , [ceny](https://azure.microsoft.com/pricing/details/vpn-gateway)a odchozí dat přenos [ceny](https://azure.microsoft.com/pricing/details/data-transfers).

## <a name="connecting-with-a-site-to-site-connection"></a>Připojení k připojení k webu

Tento typ připojení umožňuje přístup k některé cloudovým službám Microsoftu tunelem IPSec přes Internet, jak je ukázáno v následujícím příkladu.

![S2S] (./media/guidance-connecting-your-on-premises-network-to-azure/s2s.png "Připojení k webu")

Připojení je volání na váš stávající připojení k Internetu, ale je potřeba použít Brána VPN Azure s jejími přidruženými ceny a přenos odchozí dat ceny. Následující tabulka uvádí výhody a nevýhody tohoto typu připojení.

| **Výhody**| **Co byste měli zvážit**|
|---------|---------|
|Všechna zařízení ve vaší místní síti komunikovat s Azure služby připojení k VNet, je třeba konfigurovat jednotlivá připojení pro každé zařízení.|Když přenosu šifrovaná propojení, mohou být zachyceny na cestě, protože projde veřejného Internetu.|
|Pomocí existujícího připojení k Internetu.|Neočekávané latence protože projde připojení k Internetu.|
|Mohou sloužit k připojení k Azure připojené k VNet například virtuálních počítačích a cloudové služby.|Konfigurace a správa ověřený VPN zařízení * místní.|
|Výkon až 200 Mb/s za brány.|Vyžaduje minimální probíhající správu brány VPN Azure.|
|Můžete vynutit odchozí přenosy zahájená z cloudu virtuálních počítačích v místní síti pro kontrolu a protokolování pomocí uživatelem definovaných směruje nebo okraj brány (BGP Protocol) **.|Nelze se připojit k aplikaci Microsoft Office 365 nebo Dynamics CRM Online.|
||Nelze se připojit k Azure služby, které nelze připojit k VNet.|
||Pokud používáte služby, které zahájit připojení zpátky do místního zařízení a to vyžadují zásady zabezpečení, možná budete muset mezi místní síti a Azure brána firewall.|

- * Zobrazení seznamu [Ověřit VPN zařízení](../vpn-gateway/vpn-gateway-about-vpn-devices.md#validated-vpn-devices).
- ** Další informace o používání [uživatelem definovaných trasy](../vpn-gateway/vpn-gateway-forced-tunneling-rm.md) nebo [BGP](../vpn-gateway/vpn-gateway-bgp-overview.md) vynutit směrování z Azure VNets na zařízení s místní.

## <a name="connecting-with-a-dedicated-private-connection"></a>Připojení pomocí vyhrazené soukromé připojení

Tohoto typu připojení poskytuje přístup k všechny cloudovým službám společnosti Microsoft prostřednictvím vyhrazené soukromé připojení Microsoftu, která není přecházet na Internetu, jak je ukázáno v následujícím příkladu.

![ER] (./media/guidance-connecting-your-on-premises-network-to-azure/er.png "ExpressRoute připojení")

Připojení vyžaduje použití služeb ExpressRoute a připojení k poskytovatel připojení. Následující tabulka uvádí výhody a nevýhody tohoto typu připojení.

| **Výhody**| **Co byste měli zvážit**|
|---------|---------|
|Přenosy nelze zachytit při přenosu šifrovaná veřejné Internetu od použití vyhrazené připojení prostřednictvím poskytovatele služeb.|Vyžaduje místní směrovači správy.|
|Šířka pásma až 10 Gb/s za ExpressRoute obvodů a výkon až 2 Gb/s každou bránu.|Vyžaduje vyhrazené připojení k poskytovatel připojení.|
|Předvídatelná latence protože je vyhrazené připojení Microsoftu, která není přecházet na Internetu.|Může vyžadovat minimální trvalou správu jeden nebo více Azure VPN brány (Pokud připojíte obvod VNets).|
|Není nutné zadávat šifrované komunikace, když můžete zašifrujete přenos, pokud budete chtít.| Pokud používáte cloudové služby, které zahájit připojení zpátky do místního zařízení, možná budete muset mezi místní síti a Azure brána firewall.|
|Můžete připojit přímo k všechny cloudovým službám společnosti Microsoft, s několika výjimky *.|Vyžaduje překládání adres (překladu síťových adres) části místní IP adresy zadáním datacentrech Microsoft služeb, které nemůže být připojeni k VNet.* *|
|Můžete vynutit odchozí přenosy zahájená z cloudu virtuálních počítačích v místní síti pro kontrolu a přihlášení pomocí BGP.|

- * Zobrazení [seznam služeb](../expressroute/expressroute-faqs.md#supported-services) , které se nedají použít s ExpressRoute. Předplatné Azure musí schválit připojení k Office 365.  Naleznete v článku [Azure ExpressRoute pro Office 365](https://support.office.com/article/Azure-ExpressRoute-for-Office-365-6d2534a2-c19c-4a99-be5e-33a0cee5d3bd?ui=en-US&rs=en-US&ad=US&fromAR=1) podrobnosti.
- ** Další informace o ExpressRoute [překladu síťových adres](../expressroute/expressroute-nat.md) požadavky.

Další informace o [ExpressRoute](../expressroute/expressroute-introduction.md)jeho přidružený [ceny](https://azure.microsoft.com/pricing/details/expressroute)a [poskytovatelů připojení](../expressroute/expressroute-locations.md#connectivity-provider-locations).

## <a name="additional-considerations"></a>Další informace

- Několik výše uvedených možností mají různé maximální limity, které podporují pro VNet připojení brány připojení a dalších kritérií. Doporučujeme, abyste si Azure [sítě omezuje](../azure-subscription-service-limits.md#networking-limits) pochopit, pokud libovolné z nich mít vliv na typy připojení, které se rozhodnete sdělit nám. 
- Pokud budete chtít propojte brány připojení VPN k webu stejné VNet jako ExpressRoute brány, byste si měli přečíst důležité omezení nejdřív. V tématu [Konfigurace ExpressRoute a existujících připojení k webu](../expressroute/expressroute-howto-coexist-resource-manager.md#limits-and-limitations) další podrobnosti.

## <a name="next-steps"></a>Další kroky

Níže uvedených zdrojích popisují, jak implementovat typy připojení uvedené v tomto článku.

-   [Implementace připojení čárky webu](../vpn-gateway/vpn-gateway-howto-point-to-site-rm-ps.md)
-   [Implementace připojení k webu](guidance-hybrid-network-vpn.md)
-   [Implementace vyhrazené soukromé připojení](guidance-hybrid-network-expressroute.md)
-   [Implementace vyhrazené soukromé připojení k vytvoření připojení k webu dostupnost](guidance-hybrid-network-expressroute-vpn-failover.md)
