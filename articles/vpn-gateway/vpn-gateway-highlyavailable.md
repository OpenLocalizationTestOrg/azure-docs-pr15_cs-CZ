<properties
   pageTitle="Základní informace o vysoce dostupné konfigurace s Azure VPN bran | Microsoft Azure"
   description="Tento článek obsahuje přehled vysoce dostupných možností konfigurace pomocí Azure VPN brány."
   services="vpn-gateway"
   documentationCenter="na"
   authors="yushwang"
   manager="rossort"
   editor=""
   tags=""/>

<tags
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="09/24/2016"
   ms.author="yushwang"/>

# <a name="highly-available-cross-premises-and-vnet-to-vnet-connectivity"></a>Vysoce k dispozici více místní organizaci a VNet VNet připojení

Tento článek obsahuje přehled vysoce dostupné možnosti konfigurace křížově místní a VNet VNet připojení pomocí Azure VPN brány.

## <a name = "activestandby"></a>Informace o redundance brány Azure VPN

Každý Azure VPN brány se skládá z dvě instance v úsporném režimu aktivní konfiguraci. Plánované údržby nebo neplánované narušení, která se stane s aktivní instanci by úsporném instance automaticky převzít řízení (převzetí) a pokračujte S2S VPN nebo VNet VNet připojení. Přepnout přes způsobí stručný přerušení. K plánované údržbě by měl být propojení obnoven vyvolané 10 až 15. U neplánované problémech se dočtete víc obnovení připojení bude déle, asi minutu 1 a půl minut nejhorší. P2S klienta sítě VPN brány se od ní odpojilo P2S připojení a uživatelé potřebují se znovu připojit v klientských počítačích.

![Aktivní úsporném režimu](./media/vpn-gateway-highlyavailable/active-standby.png)

## <a name="highly-available-cross-premises-connectivity"></a>Vysoce k dispozici více místní připojení

Chcete-li poskytnout lepší dostupnost křížového místní připojení, máte několik možností k dispozici:

- Více místní VPN zařízení
- Aktivní Azure VPN brány
- Kombinace obou

### <a name = "activeactiveonprem"></a>Více místní VPN zařízení

Můžete několika zařízeních VPN z místní síti se připojit k brány Azure VPN jak je znázorněno na následujícím obrázku:

![Více místní VPN](./media/vpn-gateway-highlyavailable/multiple-onprem-vpns.png)

Konfigurace obsahuje více Aktivní tunely z stejné brány Azure VPN k místním zařízením ve stejném umístění. Existují některé požadavky a omezeními:

1. Potřebujete vytvořit více S2S sítě VPN z vašich zařízení VPN Azure. Když spojíte několika zařízeních VPN ze stejného v místní síti Azure, je potřeba vytvořit jednu bránu místní síti u každého zařízení VPN a jedno připojení z bránu Azure VPN k bráně pro místní síti.

2. Místní síti bran odpovídající VPN zařízení musí mít jedinečný veřejnou IP adres ve vlastnosti "GatewayIpAddress".

3. BGP je nutný pro tuto konfiguraci. Každou místní síti bránu představující VPN zařízení musí mít jedinečný BGP mezi dvěma účastníky IP adresu zadaná ve vlastnosti "BgpPeerIpAddress".

4. Pole vlastností AddressPrefix v každé místní síti brány nesmí překrývat. Je třeba zadat "BgpPeerIpAddress" v /32 CIDR formát v poli AddressPrefix, například 10.200.200.254/32.

5. BGP používejte inzerce stejné předpony stejných místní síti předpony pro bránu Azure VPN a provoz předávány tyto tunely současně.

6. U každého připojení se počítá proti maximální počet tunelů bránu Azure VPN 10 Basic a standardní skladové jednotky a čísla 30 vysoce SKU. 

V této konfiguraci brány Azure VPN stále probíhá aktivní úsporném režimu tak stejné chování převzetí a stručný přerušení přesto proběhne jako popsaných [výše](#activestandby). Ale toto nastavení chrání před selhání nebo vyrušování v místní síti a VPN zařízení.
 
### <a name="active-active-azure-vpn-gateway"></a>Aktivní Azure VPN brány

Teď můžete vytvářet brány Azure VPN v konfiguraci aktivní, kde obě instance VMs brány nastaví tunelů S2S VPN na zařízení s místním VPN, jak je znázorněno na následujícím obrázku:

![Aktivní](./media/vpn-gateway-highlyavailable/active-active.png)

V této konfiguraci pokaždé Azure brány bude obsahovat jedinečné veřejnou IP adresu a zřídit IPsec/IKE S2S VPN tunelem do zařízení VPN místní zadanými v místní síti brány a připojení. Všimněte si, že obou VPN tunelů skutečně část stejné připojení. Bude pro nastavení místní sítě VPN zařízení přijmout nebo vytvořit dvěma tunelů S2S VPN na těchto dvou Azure VPN brány veřejné IP adresy potřebujete.

Vzhledem k tomu instancí bran Azure v konfiguraci aktivní, přenosy v síti Azure virtuální k místní síti budou směrovány obou tunely současně, i v případě zařízení s místním VPN mohou kompresi jeden tunelem přednost před druhou. Poznámka: když stejné TCP a UDP tok prochází vždy stejné tunelem nebo cestu, pokud údržbu události dojde k některé z instance.

Pokud plánované údržby nebo neplánovanou událost se stane s instancí jednu bránu, budou odpojeni tunelem IPsec z této instance do místního VPN zařízení. Odpovídajícím tras na vašich zařízeních VPN by měly být odebrány nebo odebráno automaticky, takže přenos bude přepnutí na jiné aktivní tunelem IPsec. Na straně Azure přepínačem myši se stane automaticky z problémového instance aktivní instance.

### <a name="dual-redundancy-active-active-vpn-gateways-for-both-azure-and-on-premises-networks"></a>Dvou redundance: aktivní VPN bran Azure a místních sítí

Nejčastěji spolehlivé možností je kombinovat brány aktivní v síti a Azure, jak je vidět v diagramu pod.

![Obrázek dvojího redundance](./media/vpn-gateway-highlyavailable/dual-redundancy.png)

Tady vytvoření a nastavení brány Azure VPN v konfiguraci aktivní a vytvořte dvě místní síti brány a dvě připojení pro vaše dvě místního počítače v síti VPN ve výše uvedeném. Výsledkem je celé mřížky připojení 4 tunelů IPsec mezi Azure virtuální sítě a v místní síti.

Všechny brány a tunely jsou aktivní z Azure strany tak přenos budou rozloženy mezi všechny 4 tunelů současně, i když každý TCP a UDP tok znovu následovat stejné tunelem nebo cestu z Azure strany. I když je tak, že přenos, uvidí mírně lepší výkon přes tunelů IPsec, primární cíle této konfiguraci je vysoké dostupnosti. Ani z důvodu statistické druh šíření obtížné zajistit, že měření na různých podmínek provozu aplikace ovlivní agregační výkon.

Tento topologii vyžaduje dvě místní síti brány a dvě připojení k podpoře pár místní VPN zařízení a BGP je nutný pro povolení dvou připojení ke stejné místní síti. Tyto požadavky jsou stejná jako [výše](#activeactiveonprem). 

## <a name="highly-available-vnet-to-vnet-connectivity-through-azure-vpn-gateways"></a>Vysoce dostupné VNet VNet připojení prostřednictvím sítě VPN Azure brány

Stejné konfigurace aktivní můžete taky použít na Azure VNet VNet připojení. Můžete vytvořit aktivní VPN bran pro obě virtuální sítě a propojit je do formuláře stejné připojení úplné mřížky 4 tunelů mezi dvěma VNets, jak je vidět v diagramu pod:

![VNet VNet](./media/vpn-gateway-highlyavailable/vnet-to-vnet.png)

Zajistíte, že jsou vždy dvojice tunelů mezi dvěma virtuální sítěmi pro všechny události plánované údržby poskytující ještě lepších dostupnosti. I když je stejný topologie pro více místní připojení vyžaduje dva připojení, topologii VNet VNet uveden nad potřebovat jedinou připojení pro každou bránu. Kromě toho BGP vynechán Pokud směrování přenosu přes připojení VNet VNet není potřeba.


## <a name="next-steps"></a>Další kroky

V tématu [Konfigurace brány VPN aktivní křížově místní a VNet VNet připojení](vpn-gateway-activeactive-rm-powershell.md) postup konfigurace křížově místní aktivní a VNet VNet připojení.
