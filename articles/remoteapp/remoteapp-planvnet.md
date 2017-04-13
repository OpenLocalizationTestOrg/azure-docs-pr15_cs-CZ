<properties
    pageTitle="Jak naplánovat sítě virtuální pro kolekci Azure RemoteApp | Microsoft Azure"
    description="Informace o plánování virtuální sítě pro kolekci Azure RemoteApp."
    services="remoteapp"
    documentationCenter="" 
    authors="mghosh1616"
    manager="mbaldwin" />

<tags
    ms.service="remoteapp"
    ms.workload="compute"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/15/2016"
    ms.author="elizapo" />

# <a name="how-to-plan-your-virtual-network-for-azure-remoteapp"></a>Plánování sítě virtuální Azure RemoteApp

> [AZURE.IMPORTANT]
> Azure RemoteApp je právě ukončené. Přečtěte si [oznámení](https://go.microsoft.com/fwlink/?linkid=821148) podrobnosti.

Tento dokument popisuje, jak nastavit síti Azure virtuální (VNET) a podsítí pro Azure RemoteApp. Pokud neznáte Azure virtuálních sítí, je to funkci, která vám pomůže při virtualizaci vaši síťovou infrastrukturu do cloudu a můžete vytvářet hybridní řešení s Azure a místních zdrojů. Další informace [v tomto poli](../virtual-network/virtual-networks-overview.md).

Pokud chcete definovat zásady zabezpečení pro přenos (příchozí a odchozí pošty) v síti virtuální kde nasazujete Azure RemoteApp, důrazně doporučujeme vytváření samostatné podsítě Azure RemoteApp před stavebním blokem nasazení v Azure virtuální sítě. Další informace o tom, jak definovat zásady zabezpečení na vaší podsítě Azure virtuální sítě, přečtěte si prosím [Co je skupina zabezpečení síti (NSG)?](../virtual-network/virtual-networks-nsg.md).

## <a name="types-of-azure-remoteapp-collections-with-azure-virtual-networks"></a>Typy kolekcí Azure RemoteApp s Azure virtuálních sítí

Na následujících obrázcích zobrazit dvě možnosti jiné kolekce, pokud chcete použít virtuální sítě.

### <a name="azure-remoteapp-cloud-collection-with-vnet"></a>Azure kolekce cloudu RemoteApp s VNET

 ![Azure RemoteApp - cloudu kolekce s VNET](./media/remoteapp-planvpn/ra-cloudvpn.png)

Představuje kolekci Azure RemoteApp, kde jsou všechny zdroje, které relace RemoteApp hosts se potřebují dostat k nasazenou v Azure. Může být ve stejném VNET jako RemoteApp VNET nebo jiné VNET v Azure.

### <a name="azure-remoteapp-hybrid-collection-with-vnet"></a>Azure kolekce hybridní RemoteApp s VNET

![Azure RemoteApp - hybridní kolekce s VNET](./media/remoteapp-planvpn/ra-hybridvpn.png)

Představuje kolekci Azure RemoteApp, kde část prostředky, které hostitele relace RemoteApp se potřebují dostat k jsou nasazeném místního. RemoteApp VNET je propojená s místní síti technologie Azure hybridní jako virtuální privátní sítě nebo Express směrování na webu.


## <a name="how-the-system-works"></a>Jak funguje systému

Na pozadí Azure RemoteApp nasadí Azure virtuálních počítačích (s nahraný obrázek) podsítě virtuální sítě, kterou jste si vybrali při zřizování. Pokud jste kolekce hybridní webové aplikace, pokusíme vyřešit plně kvalifikovaný název domény řadiče domény, které jste zadali v pracovním postupu zřizovací serveru DNS podle virtuální sítě.  
Pokud se připojujete k existující virtuální síti, zkontrolujte, že vystavit nezbytné porty v sítě skupin zabezpečení do vaší podsítě Azure RemoteApp. 

Doporučujeme že použít [dostatečně velký k tomu podsítě Azure RemoteApp](remoteapp-vnetsizing.md). Mřížku nejřidší nepodporuje Azure virtuální sítě je /8 (pomocí CIDR podsítě definice). Vaší podsítě by měl být dostatečně velký k tomu tak, aby zahrnoval všechny VMs RemoteApp Azure během měřítko zdola nahoru, když další uživatelé přistupují aplikace. 

Tady jsou věci, které je třeba povolit na vaší podsítě virtuální sítě: 

2.  Odchozí přenosy z podsítě by měl být limit povolený na rozsah portů 10101 10175 Document komunikovat s některým z vnitřní služby Azure RemoteApp.
3.  Odchozí přenosy mohou z vaší podsítě připojení k úložišti Azure na port 443
4.  Pokud máte hostované v Azure Active Directory, zkontrolujte, že všechny OM v rámci virtuální podsítě Azure RemoteApp je možné se připojit k této domény řadiče domény. DNS v virtuální sítě by měl být plně kvalifikovaný název domény řadiče vyřešit.


## <a name="virtual-network-with-forced-tunneling"></a>Virtuální sítě s vynuceného tunneling

[Vynucená tunneling](../vpn-gateway/vpn-gateway-about-forced-tunneling.md) teď podporuje pro všechny nové kolekce Azure RemoteApp. Momentálně není podporujeme migrace existující kolekce podporuje vynucený tunneling.  Budete muset odstranit všechny existující kolekce pomocí VNET, který propojujete Azure RemoteApp a vytvořte novou získat vynucená tunneling povolili na vaše kolekce. 
