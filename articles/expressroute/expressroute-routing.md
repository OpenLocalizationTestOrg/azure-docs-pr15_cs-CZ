<properties
   pageTitle="Směrování požadavky pro ExpressRoute | Microsoft Azure"
   description="Tato stránka obsahuje podrobné požadavky pro konfiguraci a správě směrování ExpressRoute obvody."
   documentationCenter="na"
   services="expressroute"
   authors="osamazia"
   manager="ganesr"
   editor=""/>
<tags
   ms.service="expressroute"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/19/2016"
   ms.author="osamazia"/>


# <a name="expressroute-routing-requirements"></a>ExpressRoute směrování požadavky  

Pokud chcete připojit ke cloudovým službám společnosti Microsoft pomocí ExpressRoute, musíte nastavit a spravovat směrování. Někteří poskytovatelé připojení nabízejí nastavení a správě směrování jako spravované služby. Obraťte se na svého poskytovatele připojení a zjistěte, pokud je tato služba. Pokud ne, musí dodržovat následující požadavky. 

Přečtěte si článek [obvody a směrování domény](expressroute-circuit-peerings.md) popis směrování relací, které je potřeba nastavit usnadnit připojení.

>[AZURE.NOTE] Microsoft nepodporuje všechny protokoly redundance směrovači (například HSRP, VRRP) pro dostupnost konfigurace. Jsme spolehnout nadbytečné dvojice BGP relací na prozkoumávání vysoké dostupnosti.

## <a name="ip-addresses-used-for-peerings"></a>IP adresy používané pro peerings

Budete muset rezervovat několik bloků IP adres pro nastavení směrování mezi sítě a směrovači okraje (MSEEs) společnosti Microsoft Enterprise. Tato část obsahuje seznam požadavky a popisuje pravidla týkající se způsob tyto IP adresy musí být získali a.

### <a name="ip-addresses-used-for-azure-private-peering"></a>IP adresy používané pro Azure soukromé prozkoumávání

Konfigurace peerings můžete soukromé IP adresy nebo veřejných IP adres. Oblast adresu slouží ke konfiguraci cest nesmí překrývat pomocí rozsahů adres slouží k vytváření virtuálních sítí na Azure. 

 - Musíte rezervovat /29 podsítě nebo dvou /30 podsítí pro rozhraní směrování.
 - Podsítí používá se pro směrování může být privátní IP adresy nebo veřejných IP adres.
 - Podsítí nesmí být v konfliktu s rozsahem vyhrazeno pro zákazníka pro použití v cloudu společnosti Microsoft.
 - Pokud /29 podsítě se používá, bude jde rozdělit na dvě /30 podsítí. 
     - První /30 podsítě bude sloužit k primárního odkaz a druhý/30 podsítě bude sloužit k sekundární odkaz.
     - U každé /30 podsítí, je nutné použít první IP adresu /30 podsítě na směrovači. Použijeme druhý IP adresu /30 podsítě nastavení BGP relace.
     - Musíte nastavit obou BGP relací pro naše [dostupnost SLA](https://azure.microsoft.com/support/legal/sla/) platit.  

#### <a name="example-for-private-peering"></a>Příklad v soukromé prozkoumávání

Pokud se rozhodnete sdělit nám a.b.c.d/29 nastavit prozkoumávání, bude jde rozdělit na dvě /30 podsítí. V následujícím příkladu se podíváme na použití podsítě a.b.c.d/29. 

a.b.c.d/29 bude rozděleno a.b.c.d/30 a a.b.c.d+4/30 a předán společnosti Microsoft prostřednictvím zřizovací rozhraní API. Použijete a.b.c.d+1 jako VRF IP pro primární f a Microsoft bude využívat a.b.c.d+2 jako IP VRF pro primární MSEE. Použijete a.b.c.d+5 jako VRF IP pro sekundární f a použijeme a.b.c.d+6 jako VRF IP pro sekundární MSEE.

Zvažte případu vyberete 192.168.100.128/29 stanovit prozkoumávání soukromé. 192.168.100.128/29 zahrnuje adresy z 192.168.100.128 tak, aby 192.168.100.135, mezi nimiž:

- 192.168.100.128/30 přiřadíte k link1, zprostředkovateli pomocí 192.168.100.129 a 192.168.100.130 společnosti Microsoft.
- 192.168.100.132/30 přiřadíte k link2, zprostředkovateli pomocí 192.168.100.133 a 192.168.100.134 společnosti Microsoft.

### <a name="ip-addresses-used-for-azure-public-and-microsoft-peering"></a>IP adresy používané Azure veřejné a prozkoumávání společnosti Microsoft

Veřejné IP adresy, které vlastníte je nutné použít pro nastavení BGP relace. Microsoft musí být ověřit vlastnictví adresy IP pomocí směrování internetových registry a registry směrování internetových. 

- Je nutné použít jedinečný/29 podsítě nebo dvou /30 podsítí stanovit prozkoumávání pro každou prozkoumávání za ExpressRoute BGP elektrický obvod (Pokud máte víc než jednu). 
- Pokud /29 podsítě se používá, bude jde rozdělit na dvě /30 podsítí. 
    - První /30 podsítě bude sloužit k primární propojení a druhý/30 podsítě bude sloužit k sekundárním odkaz.
    - U každé /30 podsítí, je nutné použít první IP adresu /30 podsítě na směrovači. Použijeme druhý IP adresu /30 podsítě nastavení BGP relace.
    - Musíte nastavit obou BGP relací pro naše [dostupnost SLA](https://azure.microsoft.com/support/legal/sla/) platit.

## <a name="public-ip-address-requirement"></a>Veřejnou IP adresu požadavek 

### <a name="private-peering"></a>Soukromé prozkoumávání 

Můžete používat veřejné nebo soukromé adresy IPv4 pro soukromé prozkoumávání. Připravili jsme začátku do konce izolace přenosy pro vaši tak, aby překrývající se adres s ostatními zákazníky není možné v případě soukromá prozkoumávání. Tyto adresy instalační program k Internetu. 

### <a name="public-peering"></a>Veřejné prozkoumávání

Azure veřejné peering cestu umožňuje připojit se ke všem službám hostované v Azure nad jejich veřejnou IP adres. Jedná se o uvedené v [Nejčastější dotazy týkající se ExpessRoute](expressroute-faqs.md) a všechny služby hostované pomocí tvůrce programů na Microsoft Azure. Připojení ke službám Microsoft Azure na veřejné prozkoumávání je vždy, které iniciuje ze sítě do sítě Microsoft. Je nutné použít veřejnou IP adres pro přenos určený k síti Microsoft.

### <a name="microsoft-peering"></a>Prozkoumávání společnosti Microsoft

Cesta peering Microsoft umožňuje připojit se ke cloudovým službám společnosti Microsoft, které nejsou podporované prostřednictvím Azure veřejné peering cesty. Seznam služeb obsahuje služby Office 365, například Exchange Online, SharePoint Online Skype pro firmy a CRM Online. Společnost Microsoft podporuje obousměrné připojení na prozkoumávání Microsoft. Přenosy určené ke cloudovým službám společnosti Microsoft používají platné veřejné adresy IPv4 před připojením Microsoft sítě.

Ujistěte se, že svoji IP adresu a jako číslo jsou registrované pro vás v jednom z registry vypsané dole.

- [ARIN](https://www.arin.net/)
- [APNIC](https://www.apnic.net/)
- [AFRINIC](https://www.afrinic.net/)
- [LACNIC](http://www.lacnic.net/)
- [RIPENCC](https://www.ripe.net/)
- [RADB](http://www.radb.net/)
- [ALTDB](http://altdb.net/)

>[AZURE.IMPORTANT] Veřejné IP adresy oznámení společnosti Microsoft prostřednictvím ExpressRoute nesmí být oznámení na Internetu. Tato akce může přerušit připojení k další služby od Microsoftu. Veřejné IP adresy používané serverech ve vaší síti komunikovat s O365 koncové body v Microsoft může však vyhlašují přes ExpressRoute. 

## <a name="dynamic-route-exchange"></a>Dynamické směrování exchange

Směrování exchange budou přes eBGP protokol. Relace EBGP jsou vytvořeny mezi MSEEs a vaše směrovači. Ověřování BGP relací není povinné. V případě potřeby je možné konfigurovat algoritmus hash MD5. Podívejte se na [konfigurovat směrování](expressroute-howto-routing-classic.md) a [okruhem zřizujete pracovní postupy a států okruhem](expressroute-workflows.md) informace o konfiguraci BGP relace.

## <a name="autonomous-system-numbers"></a>Samostatný systém čísel

Použijeme AS 12076 pro Azure veřejnosti, Azure soukromé a prozkoumávání Microsoft. Avíza expedice zboží z 65515 jsme máte vyhrazená pro 65520 pro interní potřebu. Jsou podporované 16 a 32 bit jako čísla.

Neexistují žádné požadavky kolem symetrie přenos data. Přeposílání a zpáteční cesty může procházet dvojice různých směrovači. Stejné směruje musí vyhlašují z obou stran napříč několika párů okruh patřící můžete. Směrování metriky nemusí být stejný.

## <a name="route-aggregation-and-prefix-limits"></a>Agregace směrování a předponu omezení

Podporujeme až 4000 předpony nabízené námi prostřednictvím Azure soukromé prozkoumávání. To je možné zvýšit až 10 000 předpony Pokud je povolený doplněk premium ExpressRoute. Jsme přijmout až 200 předpony relace BGP pro Azure veřejné a prozkoumávání Microsoft. 

Relace BGP dojde ke ztrátě Pokud počet předpony překračuje limit. Jsme přijímá výchozích tras na pouze soukromé peering odkaz. Zprostředkovatel musí odfiltrovat výchozí směrování a soukromé IP adresy (RFC 1918) z Azure veřejné a Microsoft prozkoumávání cest. 

## <a name="transit-routing-and-cross-region-routing"></a>Směrování přenosu a směrování více oblastí

ExpressRoute se nedají konfigurovat jako při přenosu šifrovaná směrovači. Budete muset spolehnout se na svého poskytovatele připojení pro směrování služeb.

## <a name="advertising-default-routes"></a>Zobrazování reklam výchozí postupy

Výchozí trasy jsou povoleny pouze na Azure soukromé relace prozkoumávání. V takovém případě jsme bude směrovat všechny přenosy z přidružené virtuální sítě k síti. Reklamní výchozí trasy do soukromé prozkoumávání bude mít za následek cestu internet z Azure blokování. Musíte spolehnout vaší podnikové okraj chcete směrovat přenosy v síti od a do Internetu pro službu hostované v Azure. 

 Povolit připojení k další Azure a infrastruktury služby, třeba zkontrolujte, jestli jedním z následujících položek je na místě:

 - Azure veřejné prozkoumávání aktivované provoz směrovat na veřejné koncové body
 - Pomocí uživatelem definovaných směrování povolit připojení k Internetu pro každé podsítě vyžadují připojení k Internetu.
 
>[AZURE.NOTE] Reklamní výchozí trasy přerušíte Windows a dalších aktivace OM licence. Postupujte podle pokynů [tady](http://blogs.msdn.com/b/mast/archive/2015/05/20/use-azure-custom-routes-to-enable-kms-activation-with-forced-tunneling.aspx) vyřešit.

## <a name="support-for-bgp-communities-preview"></a>Podpora pro BGP komunit (verze Preview)


Tato část obsahuje základní informace o použití BGP komunit s ExpressRoute. Microsoft vydává směruje ve veřejných a cest prozkoumávání Microsoft s postupy označený hodnoty odpovídající komunity. Při tom a podrobnosti o komunity hodnoty jsou uvedeny níže. Microsoft, ale nebude přijmout všechny hodnoty komunity příznakem postupech oznámení společnosti Microsoft.

Pokud se připojujete k Microsoft prostřednictvím ExpressRoute kdekoli jeden peering geopolitické oblasti, bude mít přístup ke všem službám cloudu společnosti Microsoft ve všech oblastech v rámci geopolitické okraj. 

Například pokud jste připojení k Microsoft v Amsterodamu prostřednictvím ExpressRoute, bude mít přístup ke všem službám Microsoft cloud použitý ve Severní Evropa a západní Evropa. 

Odkaz na stránku [ExpressRoute partnery a prozkoumávání umístění](expressroute-locations.md) podrobný seznam geopolitické oblastí, přidružené Azure oblastí a odpovídající ExpressRoute prozkoumávání umístění.

Máte možnost si zakoupit více okruh ExpressRoute za geopolitické oblast. Máte víc připojení nabízí spoustu značných výhod na dostupnost kvůli geo redundance. V případech, kdy mají víc obvody ExpressRoute zobrazí se stejná skupina předpon inzerovaných od společnosti Microsoft na veřejné prozkoumávání a Microsoft prozkoumávání cest. To znamená, že bude obsahovat více cest ze sítě do aplikace Microsoft. Potenciálně to může způsobit optimální směrovací provedené ve vaší síti. Výsledkem je může docházet k optimální připojení prostředí pro různé služby. 

Microsoft budou označení předpony nabízené prostřednictvím veřejné prozkoumávání a Microsoft prozkoumávání s příslušnými hodnotami komunity BGP označující oblasti předpony jejichž hostitelem je. Můžete spolehnout na hodnoty komunity rozhodovat odpovídající směrování nabízet [optimální směrování zákazníkům](expressroute-optimize-routing.md).

| **Geopolitické oblast** | **Oblast Microsoft Azure** | **Hodnota BGP komunity** |
|---|---|---|
| **Severní Amerika** |    |  |
|    | Východní USA | 12076:51004 |
|    | Východní USA 2 | 12076:51005 |
|    | Západ USA | 12076:51006 |
|    | Západ USA 2 | 12076:51026 |
|    | Západní centrální USA | 12076:51027 |
|    | Severní centrální USA | 12076:51007 |
|    | Jižní centrální USA | 12076:51008 |
|    | Centrální USA | 12076:51009 |
|    | Centrální Kanada | 12076:51020 |
|    | Kanada východ | 12076:51021 |
| **Jižní Amerika** |  |  |
|    | Brazílie jih | 12076:51014 |
| **Europe** |    |  |
|    | Severní Evropě | 12076:51003 |
|    | Západní Evropě | 12076:51002 |
| **Asijsko-tichomořské** |    |   |
|    | Východní Asie | 12076:51010 |
|    | Jihovýchodní Asie | 12076:51011 |
| **Japonsko** |     |   |
|    | Japonsko východ | 12076:51012 |
|    | Japonsko západní | 12076:51013 |
| **Austrálie** |    |   | 
|    | Austrálie východ | 12076:51015 |
|    | Austrálie jihovýchodní | 12076:51016 |
| **Indie** |    |   |
|    | Indie jih | 12076:51019 |
|    | Indie západní | 12076:51018 |
|    | Indie centrální | 12076:51017 |

Všechny tras oznamovaných od společnosti Microsoft se označené hodnotu odpovídající komunity. 

>[AZURE.IMPORTANT] Globální předpony bude označen hodnotu odpovídající komunity a inzerovány budou pouze v případě, že je povolený doplněk ExpressRoute premium.


Kromě výše uvedeného Microsoft se také označení předpony založené na službě patří. To platí jenom pro Microsoft prozkoumávání. Následující tabulka obsahuje mapování služby hodnotě BGP komunity.

| **Služba** | **Hodnota BGP komunity** |
|---|---|
| **Exchange** | 12076:5010 |
| **Služby SharePoint** | 12076:5020 |
| **Skype pro firmy** | 12076:5030 |
| **CRM Online** | 12076:5040 |
| **Další služby Office 365** | 12076:5100 |

>[AZURE.NOTE] Microsoft nerespektuje BGP komunity hodnoty, které jste nastavili podle směruje oznámení společnosti Microsoft.

## <a name="next-steps"></a>Další kroky

- Konfigurace připojení k ExpressRoute.

    - [Vytvoření okruh ExpressRoute pro klasické nasazení modelu](expressroute-howto-circuit-classic.md) nebo [vytvořit a upravit ExpressRoute okruh pomocí Správce prostředků Azure](expressroute-howto-circuit-arm.md)
    - [Konfigurace směrování modelu klasické nasazení](expressroute-howto-routing-classic.md) nebo [konfigurovat směrování pro nasazení modelu správce prostředků](expressroute-howto-routing-arm.md)
    - [Klasický VNet ExpressRoute obvodu](expressroute-howto-linkvnet-classic.md) [odkaz](expressroute-howto-linkvnet-arm.md) nebo správce prostředků VNet ExpressRoute obvodu


