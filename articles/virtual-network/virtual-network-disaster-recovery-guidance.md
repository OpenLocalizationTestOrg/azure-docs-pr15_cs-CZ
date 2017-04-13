<properties
    pageTitle="Akce: v případě přerušení služby Azure ovlivňující ochranu virtuálních sítí Azure | Microsoft Azure"
    description="Přečtěte si postup v případě přerušení služby Azure ovlivňující ochranu Azure virtuální sítě."
    services="virtual-network"
    documentationCenter=""
    authors="NarayanAnnamalai"
    manager="jefco"
    editor=""/>

<tags
    ms.service="virtual-network"
    ms.workload="virtual-network"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="05/16/2016"
    ms.author="narayan;aglick"/>

#<a name="virtual-network--business-continuity"></a>Virtuální sítě – nepřerušený provoz

##<a name="overview"></a>Základní informace

Virtuální síť (VNet) je logické zastoupení síti v cloudu. Umožňuje definovat vlastní soukromé prostor adres IP a segmentech sítě do podsítí. VNets slouží jako hranici zabezpečení hostovat výpočetním zdroje jako je třeba Azure virtuálních počítačích a Cloudovým službám (webu nebo pracovního role). VNet umožňuje přímé soukromé IP komunikaci mezi prostředky, jejichž hostitelem je. Virtuální sítě můžou taky propojené s místní síti až jednu z možností hybridní například Brána VPN nebo ExpressRoute.
 
Vytvoří se VNet v rozsahu oblast. Vytvoříte VNets s stejné adresní prostor ve dvou různých oblastí (tedy nám východ a západ nás, ale nemůžete je vzájemně přímé připojení). 

##<a name="business-continuity"></a>Nepřerušený provoz

Je možné, že aplikace může přerušení několika různými způsoby. Danou oblast může úplně oříznutí kvůli přírodní katastrofě nebo selhání částečné selhání víc zařízeních a služeb. Dopad na službu VNet se liší v žádném z těchto situací.

**Otázka: co můžete dělat v případě výpadku celé oblasti? tedy pokud oblasti úplně přerušení kvůli přírodní katastrofě? Co se stane s virtuálních sítí hostované v oblasti?**

Odpověď: virtuální sítě a prostředky ovlivněné oblasti zůstane nedostupné v době přerušení služby.

![Jednoduchý virtuální síťového diagramu](./media/virtual-network-disaster-recovery-guidance/vnet.png)

**Otázka: Co mám znovu vytvořit stejnou virtuální sítě v jiné oblasti?**

Odpověď: virtuální síť (VNet) je poměrně lightweight zdroje. Vyvoláte Azure rozhraní API pro vytvoření VNet s stejné adresní prostor v jiné oblasti. Pokud chcete znovu vytvořit stejnou prostředí, které bylo účastní ovlivněné oblasti, budete muset volat rozhraní API pro znovu nasazení cloudovými službami (webu nebo pracovního role) a virtuálních počítačích, které jste měli. Budete taky muset číselník přes VPN brány a připojení k síti místní, pokud máte místní připojení (třeba v hybridním nasazení).

Pokyny k vytváření VNet najdete [tady](./virtual-networks-create-vnet-arm-pportal.md). 

**Otázka: otevřené VNet v dané oblasti lze znova vytvořené v jiné oblasti předem?**

Odpověď: Ano, můžete vytvořit dvěma VNets použití stejné soukromé místo IP adresa a zdrojů ve dvou různých oblastí předem. Pokud zákazník byl hostitelem internetové služby v VNet, se může nastavení aplikace přenosy Manager ke směrování geo dat do oblasti, která je aktivní. Však zákazníka, nedají čárou propojit dva VNets stejné adresní prostor k jejich místní síti jako způsobí směrování problémy. V době selhání a ztráty VNet v jedné oblasti zákazníka připojení dalších VNet v oblasti k dispozici s odpovídajícími adresního prostoru v místní síti.

Pokyny k vytváření VNet najdete [tady](./virtual-networks-create-vnet-arm-pportal.md).
