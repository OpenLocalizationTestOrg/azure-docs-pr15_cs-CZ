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

| **Poskytovatel služeb**  |**Microsoft Azure** | **Office 365 a CRM Online** | **Umístění** |
|-----------------------|--------------------|----------------|---------------|
| **AARNet** | Podporované | Podporované | Melbourne, Sydney |
| **[Aryaka sítě]( http://www.aryaka.com/)** | Podporované | Podporované | Amsterdam, křemíku sedla, Singapur Brno, WA Datacentrum |
| **[AT & T NetBond]( https://www.synaptic.att.com/clouduser/html/productdetail/ATT_NetBond.htm)** | Podporované | Podporované | Amsterdam Chicago, Domažlice, Londýn, křemíku sedla, Singapur, Sydney, WA Datacentrum |
| **[British Telecom]( http://www.globalservices.bt.com/uk/en/news/bt_to_provide_connectivity_to_microsoft_azure)** | Podporované | Podporované | Amsterdam Hongkong, Londýn, křemíku sedla, Singapur, Sydney, Brno, WA Datacentrum |
|**CenturyLink** | Již brzy | Již brzy| Křemíku sedla |
|**Čína Telecom globální** | Podporované | Nepodporovaná funkce | Hongkong |
|**[Cologix](http://www.cologix.com/solutions/cloud-connect/public-clouds/microsoft-cloud/)** | Podporované | Již brzy | Ostrava Dallasu, Montrealský + |
| **[Colt]( http://www.colt.net/uk/en/news/colt-announces-dedicated-cloud-access-for-microsoft-azure-services-en.htm)**  |  Podporované | Podporované | Amsterdam, Dublin, Londýn, Brno |
| **Společnosti Comcast** | Podporované | Podporované | Ostrava, křemíku sedla, WA Datacentrum |
| **[CoreSite](http://www.coresite.com/solutions/cloud-services/public-cloud-providers/microsoft-azure-expressroute)** | Podporované | Podporované | Olomouc | 
| **[Equinix](http://www.equinix.com/partners/microsoft-azure/)** | Podporované | Podporované | Amsterdam, Plzeň, Chicago, Domažlice, Hongkong, Londýn, Los Angeles, Melbourne, New York, Ósaka, Paříž +, Svatý Paulo, Seattlu, křemíku sedla, Singapur, Sydney, Brno, Ostrava, WA Datacentrum |
| **euNetworks** |  Podporované | Podporované | Amsterdam |
| **GÉANT** | Podporované | Podporované | Amsterdam |
| **[Internet iniciativa Japonsko Inc. - IIJ](http://www.iij.ad.jp/en/news/pressrelease/2015/1216-2.html)** |  Podporované | Podporované | Ósaka, Brno |
| **[InterCloud]( https://www.intercloud.com/)** | Podporované | Podporované | Amsterdam, Londýn, Singapur, WA Datacentrum |
| **Připojení Internet řešení - cloudu** | Podporované | Podporované | Amsterdam, Londýn |
| **[Interxion](http://www.interxion.com/why-interxion/colocate-with-the-clouds/colocated-hybrid-cloud/microsoft-azure/)**  | Podporované | Podporované | Amsterdam, Londýn, Paříž |
| **Jisc** | Podporované | Podporované | Londýn | 
| **[Komunikace úrovni 3]( http://your.level3.com/LP=882?WT.tsrc=02192014LP882AzureVanityAzureText)** | Podporované | Podporované | Amsterdam Chicago, Domažlice, Las Vegas +, Londýn, Seattlu, křemíku sedla, WA Datacentrum |
| **Megaport** | Podporované | Podporované | Domažlice, Hong Kong Las Vegas, Los Angeles Melbourne New York Seattlu, Singapur Sydney, WA Datacentrum |
| **MOUNTAIN** | Podporované | Podporované | Londýn |
| **Další Data generování** | Již brzy | Již brzy | Newport(Wales) + |
| **NEXTDC** | Podporované | Podporované | Melbourne, Sydney |
| **NTT komunikace** | Podporované | Podporované | Londýn, Los Angeles Ósaka Brno |
| **[Oranžová]( http://www.orange-business.com/en/products/business-vpn-galerie)** | Podporované | Podporované | Amsterdam, Hong Kong Londýn křemíku sedla, Singapur Sydney, WA Datacentrum |
| **Omezené PCCW globální** | Podporované | Podporované | Hongkong |
| **[SingTel]( http://info.singtel.com/about-us/news-releases/singtel-provide-secure-private-access-microsoft-azure-public-cloud)** |  Podporované | Podporované | Singapur |
| **Softbank** | Podporované | Podporované | Ósaka, Brno | 
| **[Tata komunikace](http://www.tatacommunications.com/lp/izo/azure/azure_index.html)** | Podporované | Podporované | Amsterdam, Čennaj, Hong Kong, Londýn, Mumbai křemíku sedla, Singapur, WA Datacentrum |
| **[Skupina TeleCity]( http://www.telecitygroup.com/investor-centre/news_details.htm?locid=03100500400b00d&xml)** | Podporované | Podporované | Amsterdam, Dublin, Londýn |
| **Telefonica** | Podporované | Podporované | Svatý Paulo |
| **Telenor** | Podporované | Podporované | Amsterdam, Londýn |
| **[Telstra Corporation]( http://www.telstra.com.au/business-enterprise/network-services/networks/cloud-direct-connect/)** | Podporované | Podporované | Melbourne, Sydney |
| **[Verizon](http://www.verizonenterprise.com/products/networking/secure-cloud-interconnect/)** | Podporované | Podporované | Amsterdam Hongkong, Londýn, křemíku sedla, Singapur, Sydney, Brno, WA Datacentrum |
| **Vodafone** | Podporované | Nepodporovaná funkce | Londýn | 
| **[Skupina Zayo]( http://www.zayo.com/solutions/industries/connect-to-cloud-data-centers/cloud-connectivity/microsoft-expressroute/)** | Podporované | Podporované | Ostrava, Los Angeles New York křemíku sedla Ostrava, WA Datacentrum |

 **+**označuje brzy k dispozici

### <a name="national-cloud-environments"></a>Cloud národní prostředí

#### <a name="us-government-cloud"></a>US Government cloud

| **Poskytovatel služeb**  |**Microsoft Azure** | **Office 365** | **Umístění** |
|-----------------------|--------------------|----------------|---------------|
| **[AT & T NetBond]( https://www.synaptic.att.com/clouduser/html/productdetail/ATT_NetBond.htm)** | Podporované | Podporované | Ostrava, WA Datacentrum |
| **[Equinix](http://www.equinix.com/partners/microsoft-azure/)** | Podporované | Podporované | Ostrava, Domažlice, New York, WA Datacentrum |
| **[Komunikace úrovni 3]( http://your.level3.com/LP=882?WT.tsrc=02192014LP882AzureVanityAzureText)** | Podporované | Podporované | Ostrava, New York +, WA Datacentrum |
| **[Verizon](http://news.verizonenterprise.com/2014/04/secure-cloud-interconnect-solutions-enterprise/)** | Podporované | Podporované | Ostrava, Domažlice + New York, WA Datacentrum |

#### <a name="china"></a>Číny

| **Poskytovatel služeb**  |**Microsoft Azure** | **Office 365** | **Umístění** |
|-----------------------|--------------------|----------------|---------------|
| **Čína Telecom** | Podporované | Nepodporovaná funkce | Peking, Shanghai|
Další informace najdete v tématu [ExpressRoute v Číně](http://www.windowsazure.cn/home/features/expressroute/).

#### <a name="germany"></a>Německo

| **Poskytovatel služeb**  |**Microsoft Azure** | **Office 365** | **Umístění** |
|-----------------------|--------------------|----------------|---------------|
| **[Colt]( http://www.colt.net/uk/en/news/colt-announces-dedicated-cloud-access-for-microsoft-azure-services-en.htm)** | Podporované | Nepodporovaná funkce | Berlínská +, Frankfurt|
| **[Equinix](http://www.equinix.com/partners/microsoft-azure/)** | Podporované | Nepodporovaná funkce | Frankfurt|
| **e sklem** | Podporované | Nepodporovaná funkce | Berlínská|
| **Interxion** | Podporované | Nepodporovaná funkce | Frankfurt|

## <a name="nonpartners"></a>Připojení přes poskytovatele služeb není uvedená

Pokud váš poskytovatel připojení není uveden v předchozí části, můžete pořád vytvořit připojení.

- Obraťte se na svého poskytovatele připojení jestlil jsou připojené k některým výměny v předchozí tabulce. Následující odkazy můžete získat další informace o službách nabízených zprostředkovatelé exchange, můžete zkontrolovat. Několik poskytovatelů připojení připojeni k výměnu Ethernet.

    - [Equinix Cloud Exchange](http://www.equinix.com/services/interconnection-connectivity/cloud-exchange/)
    - [TeleCity CloudIX](http://www.telecitygroup.com/colocation-services/cloud-ix.htm)
    - [Interxion](http://www.interxion.com/why-interxion/colocate-with-the-clouds/colocated-hybrid-cloud/microsoft-azure/)
    - [NextDC](http://www.nextdc.com/)
    - [CoreSite](http://www.coresite.com/)
    - [Cologix](http://www.cologix.com/)
- Aby vám poskytovatel připojení rozšíření sítě peering umístění podle výběru.
    - Ujistěte se, že váš poskytovatel připojení slouží k rozšíření připojení vysoce dostupné způsobem, aby obsahoval spíše žádné jedné hodnoty selhání.
- Objednejte ExpressRoute okruh s exchange jako poskytovatele připojení pro připojení k Microsoft.
    - Postupujte podle kroků v tématu [Vytvoření ExpressRoute okruh](expressroute-howto-circuit-classic.md) nastavení připojení.

|**Zprostředkovatel připojení**|**Exchange**|**Umístění**|
|---|---|---|
|**[1CLOUDSTAR](http://www.1cloudstar.com/service/cloudconnect-azure-expressroute/)**|Equinix|Singapur|
|**Aljaška komunikace**|Equinix|Olomouc|
|**[Lightower](http://www.lightower.com/network-solutions/cloud-connect/#microsoft-azure )**|Equinix|New York, WA Datacentrum|
|**[XO komunikace](http://www.xo.com/)**|Equinix|Křemíku sedla|


## <a name="expressroute-system-integrators"></a>ExpressRoute systém doplňky

Povolení soukromé připojení podle vašich potřeb může být obtížné, založené na měřítku sítě. Můžete pracovat pomocí kterékoli z systém doplňky uvedené v následující tabulce, která vám pomůže s rychlého připojení k ExpressRoute.

|**Systém integrátor**|**Kontinentu**|
|---|---|
|**[Avanade Inc.](http://www.avanade.com/)**| Země Asie, Europe, USA |
|**[DotNet řešení](http://www.dotnetsolutions.co.uk/)**| Europe |
|**[Equinix odborné služby](http://www.equinix.com/services/consulting/)**|US|
|**[OneAs1a](http://www.oneas1a.com/express-connect-any-cloud-ecac)** | Země Asie |
|**[Perficient](http://www.perficient.com/Partners/Microsoft/Cloud/Azure-ExpressRoute)** | US |
|**[Vedoucí projektu](http://www.projectleadership.net/azure)** | US |

## <a name="next-steps"></a>Další kroky

- Další informace o ExpressRoute najdete v tématu [Nejčastější dotazy týkající se ExpressRoute](expressroute-faqs.md).
- Ujistěte se, že jsou splněné všechny předpoklady. V tématu [požadavky ExpressRoute](expressroute-prerequisites.md).

<!--Image References-->
[0]: ./media/expressroute-locations/expressroute-locations-map.png "Mapa umístění"
