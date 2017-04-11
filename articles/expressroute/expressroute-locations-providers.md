<properties
   pageTitle="ExpressRoute umístění | Microsoft Azure"
   description="Tento článek obsahuje podrobný přehled umístění, kde jsou nabízené služby a jak se připojit k Azure oblastí."
   services="expressroute"
   documentationCenter="na"
   authors="cherylmc"
   manager="carmonm"
   editor="" />
<tags
   ms.service="expressroute"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/21/2016"
   ms.author="cherylmc" />

# <a name="expressroute-partners-and-peering-locations"></a>ExpressRoute partnery a prozkoumávání umístění

Informace o ExpressRoute připojení poskytovatelům ExpressRoute zeměpisné pokrytí cloudovým službám Microsoftu podporované nad ExpressRoute a ExpressRoute systém doplňky (SIs) v tabulkách v tomto článku.

## <a name="partners"></a>Poskytovatelé ExpressRoute připojení

ExpressRoute je podporována ve všech Azure oblastí a umístění. Následující mapa poskytuje seznamu Azure regionů a ExpressRoute umístění. ExpressRoute umístění označují těm, kde Microsoft peers s několika poskytovatele služeb.

![Mapa umístění][0]

Pokud jste připojení k alespoň jeden ExpressRoute umístění v rámci oblasti geopolitické, bude mít přístup k služby Azure ve všech oblastech geopolitické oblasti. Následující tabulka obsahuje mapu regionů Azure ExpressRoute umístění v rámci geopolitické oblasti.

|**Geopolitické oblast**|**Azure oblastí**|**ExpressRoute umístění**|
|---|---|---|
|**Severní Amerika**|Východní USA, západ USA, východoasijských US 2, centrální USA, Jižní centrální USA, severní centrální USA, Kanada Kanadě Střední východ|Plzeň Chicago, Domažlice, Las Vegas, Los Angeles, New York, Seattlu, křemíku sedla, WA Datacentrum, Montrealský +, Quebec Město +, Ostrava|
|**Jižní Amerika**|Brazílie jih|Svatý Paulo|
|**Europe**|Severní Europe západní Europe UK západní jih Spojené království|Amsterdam Dublin, Londýn, Newport(Wales) +, Paříž|
|**Země Asie**|Východní Asie jihovýchodní Asie|Hongkong, Singapur|
|**Japonsko**|Japonsko Západ, Japonska východ|Ósaka, Brno|
|**Austrálie**|Austrálie jihovýchodní Austrálie východ|Melbourne, Sydney|
|**Indie**|Indie Západní Indie centrální, Indie jih|Čennaj, Mumbai|



Následující tabulka obsahuje informace o oblastí a geopolitické omezení pro vnitrostátní mračnech.

|**Geopolitické oblast**|**Azure oblastí**|**ExpressRoute umístění**|
|---|---|---|---|
|**US Government cloud**|US Gov Iowa, USA Gov Virginie|Ostrava, Domažlice, New York, WA Datacentrum|
|**Číny**|Severní, Číny Číny východ|Peking, Shanghai|
|**Německo**|Německo centrální, Německo východ|Berlínská Frankfurt|


Připojení přes geopolitické oblastí není podporována ve standardní SKU ExpressRoute. Bude třeba povolit doplněk premium ExpressRoute pro podporu globální připojení. Připojení ke cloudové národní prostředí nepodporuje. Pokud tyto potřeby můžete spolupracovat s poskytovatel připojení.


## <a name="connectivity-provider-locations"></a>Připojení poskytovatele umístění

> [AZURE.SELECTOR]
[Umístění poskytovatelem](expressroute-locations.md#connectivity-provider-locations)
[poskytovatelů podle umístění](expressroute-locations-providers.md#connectivity-provider-locations)

### <a name="production-azure"></a>Výrobní Azure
| **Umístění**  | **Poskytovatelé** |
|---------------|-----------------------|
| **Amsterdam** | Aryaka sítích, AT & T NetBond British Telecom, Colt, Equinix, euNetworks, GÉANT, InterCloud, Internet řešení - cloudu připojit, Interxion, Verizon komunikace, oranžové, Tata komunikace, TeleCity skupina Telenor, úrovně 3 |
| **Plzeň** | Equinix |
| **Čennaj** | Tata komunikace |
| **Chicago** | AT & T NetBond společnosti Comcast, Equinix, úrovně 3 komunikace, Zayo skupiny |
| **Domažlice** | AT & T NetBond, Cologix, Equinix, úrovně 3 komunikace Megaport |
| **Dublinu** | Colt, Telecity skupiny |
| **Hongkong** | British Telecom Čína Telecom globální Equinix, Megaport, oranžové, PCCW globální omezený, komunikace Tata Verizon |
| **Londýn** | AT & T NetBond British Telecom, Colt, Equinix, InterCloud, Internet řešení - cloudu připojit, Interxion, Jisc, úrovně 3 komunikace, Mountain, NTT komunikace, oranžové, Tata komunikace, Telecity skupina Telenor Verizon, Vodafone |
| **Las Vegas** | Úroveň 3 komunikace + Megaport
| **Olomouc** | CoreSite, Equinix, Megaport, NTT, Zayo skupina |
| **Melbourne** | AARNet, Equinix, Megaport, NEXTDC, Telstra Corporation |
| **New York** | Equinix, Megaport, Zayo skupina |
| **Newport(Wales) +** | Další generování dat + |
| **Montrealský** | Cologix + |
| **Mumbai** | Tata komunikace |
| **Ósaka** | Equinix, Internet iniciativa Japonsko Inc. - IIJ, NTT komunikace, Softbank |
| **Paříž** | Interxion, Equinix + |
| **Svatý Paulo** | Equinix Telefonica |
| **Olomouc** | Megaport Equinix, komunikace úrovni 3 |
| **Křemíku sedla** | Aryaka sítích, AT & T NetBond British Telecom, CenturyLink +, společnosti Comcast, Equinix, úrovně 3 komunikace, oranžové, Tata komunikace, Verizon, Zayo skupina |
| **Singapur** | Aryaka sítích, AT & T NetBond British Telecom, Equinix, InterCloud, Megaport, oranžové, SingTel, komunikace Tata, Verizon |
| **Sydney** | AARNet, AT & T NetBond, British Telecom, Equinix, Megaport, NEXTDC, oranžové, Telstra Corporation, Verizon |
| **Brno** | Aryaka sítí, British Telecom, Colt, Equinix, Internet iniciativu Japonska Inc. - IIJ, NTT komunikace, Softbank, Verizon |
| **Ostrava** | Cologix, Equinix, Zayo skupina |
| **Datacentrum Washington** | Aryaka sítích, AT & T NetBond, British Telecom, společnosti Comcast, Equinix, InterCloud, úrovně 3 komunikace, Megaport, oranžové, Tata komunikace, Verizon, Zayo skupinu |

 **+**označuje brzy k dispozici

### <a name="national-cloud-environments"></a>Cloud národní prostředí

#### <a name="us-government-cloud"></a>US Government cloud

| **Umístění**  |**Poskytovatelé** |
|---------------|--------------------|
| **Chicago** | AT & T NetBond Equinix, úrovně 3 komunikace Verizon |
| **Domažlice** |  Equinix, Verizon + |
| **New York** | Equinix, úrovně 3 komunikaci + Verizon |
| **Datacentrum Washington** | AT & T NetBond Equinix, úrovně 3 komunikace Verizon |

#### <a name="china"></a>Číny

| **Umístění**  | **Poskytovatelé** |
|---------------|-----------------------|
| **Peking** | Čína Telecom |
| **Shanghai** |  Čína Telecom |
Další informace najdete v tématu [ExpressRoute v Číně](http://www.windowsazure.cn/home/features/expressroute/)

#### <a name="germany"></a>Německo

| **Umístění**  | **Poskytovatelé** |
|---------------|-----------------------|
| **Berlínská** | Colt + e sklem |
| **Frankfurt** | Interxion Colt, Equinix, |

## <a name="nonpartners"></a>Připojení přes poskytovatele služeb není uvedená

Pokud váš poskytovatel připojení není uveden v předchozí části, můžete pořád vytvořit připojení.

- Obraťte se na svého poskytovatele připojení jestlil jsou připojené k některým výměny v předchozí tabulce. Následující odkazy můžete získat další informace o službách nabízených zprostředkovatelé exchange, můžete zkontrolovat. Několik poskytovatelů připojení připojeni k výměnu Ethernet.

    - [Equinix Cloud Exchange](http://www.equinix.com/services/interconnection-connectivity/cloud-exchange/)
    - [TeleCity CloudIX](http://www.telecitygroup.com/colocation-services/cloud-ix.htm)
    - [InterXion](http://www.interxion.com/)
    - [NextDC](http://www.nextdc.com/)
    - [CoreSite](http://www.coresite.com/)
    - [Cologix](http://www.cologix.com/)
- Aby vám poskytovatel připojení rozšíření sítě peering umístění podle výběru.
    - Ujistěte se, že váš poskytovatel připojení slouží k rozšíření připojení vysoce dostupné způsobem, aby obsahoval spíše žádné jedné hodnoty selhání.
- Objednejte ExpressRoute okruh s exchange jako poskytovatele připojení pro připojení k Microsoft.
    - Postupujte podle kroků v tématu [Vytvoření ExpressRoute okruh](expressroute-howto-circuit-classic.md) nastavení připojení.

|**Umístění**|**Exchange**|**Poskytovatelé připojení**|
|-------------|------------|-------------------------|
| **New York** | Equinix | Lightower |
| **Olomouc** | Equinix | Aljaška komunikace |
| **Křemíku sedla** | Equinix | XO komunikace |
| **Singapur** | Equinix | 1CLOUDSTAR |
| **Datacentrum Washington** | Equinix | Lightower |

## <a name="expressroute-system-integrators"></a>ExpressRoute systém doplňky

Povolení soukromé připojení podle vašich potřeb může být obtížné, založené na měřítku sítě. Můžete pracovat pomocí kterékoli z systém doplňky uvedené v následující tabulce, která vám pomůže s rychlého připojení k ExpressRoute.

|**Kontinentu**|**Systém doplňky**|
|-------------|---------------------|
| **Země Asie** | Avanade Inc. OneAs1a|
| **Europe** | Avanade Inc. Dotnet řešení|
| **US** | Avanade Inc. Equinix odborné služby, Perficient, vedoucí projektu|

## <a name="next-steps"></a>Další kroky

- Další informace o ExpressRoute najdete v tématu [Nejčastější dotazy týkající se ExpressRoute](expressroute-faqs.md).
- Ujistěte se, že jsou splněné všechny předpoklady. V tématu [požadavky ExpressRoute](expressroute-prerequisites.md).

<!--Image References-->
[0]: ./media/expressroute-locations/expressroute-locations-map.png "Mapa umístění"
