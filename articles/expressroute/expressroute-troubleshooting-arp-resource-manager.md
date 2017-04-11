<properties 
   pageTitle="Poradce při potížích Průvodce ExpressRoute - začíná ARP tabulkami | Microsoft Azure"
   description="Tato stránka obsahuje pokyny na získání ARP tabulkami ExpressRoute okruhem"
   documentationCenter="na"
   services="expressroute"
   authors="ganesr"
   manager="carolz"
   editor="tysonn"/>
<tags 
   ms.service="expressroute"
   ms.devlang="na"
   ms.topic="article" 
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services" 
   ms.date="10/10/2016"
   ms.author="ganesr"/>

#<a name="expressroute-troubleshooting-guide---getting-arp-tables-in-the-resource-manager-deployment-model"></a>Poradce při potížích s ExpressRoute Průvodce - začíná ARP tabulkami v modelu nasazení Správce prostředků

> [AZURE.SELECTOR]
[Prostředí PowerShell – správce prostředků](expressroute-troubleshooting-arp-resource-manager.md)
[prostředí PowerShell – klasické](expressroute-troubleshooting-arp-classic.md)

Tento článek vás provede kroky pro další tabulky ARP pro ExpressRoute okruh. 

>[AZURE.IMPORTANT] Tento dokument je určen vám Diagnostika a oprava problémů s jednoduché. Není určená k jako náhrada technické podpory společnosti Microsoft. Pokud nejste schopni problém nevyřeší pomocí níže uvedených pokyny, musíte otevřít požadavek podpory můžete s [podporou společnosti Microsoft](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) .

## <a name="address-resolution-protocol-arp-and-arp-tables"></a>Adresa rozlišení ARP (Protocol) a ARP tabulkami
Rozlišení ARP (Address Protocol) je definován v [RFC 826](https://tools.ietf.org/html/rfc826)protokol vrstvy 2. Slouží k namapovat Ethernet adresa (MAC) s adresou.

Tabulka ARP obsahuje mapování adresy ipv4 a adresa MAC pro konkrétní prozkoumávání. V tabulce ARP okruh ExpressRoute prozkoumávání poskytuje následující údaje pro každý rozhraní (primárních a sekundárních)

1. Mapování místní směrovači rozhraní adresy ip adresu MAC
2. Mapování ExpressRoute směrovači rozhraní adresy ip adresu MAC
3. Stáří mapování

Poradce při potížích s základní vrstvy 2 problémy s připojením a ARP tabulkami můžou pomoct ověřte konfiguraci vrstvy 2. 

Tabulka s příkladem – ARP: 

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
         10 On-Prem           10.0.0.1 ffff.eeee.dddd
          0 Microsoft         10.0.0.2 aaaa.bbbb.cccc


V následující části obsahuje informace o zobrazení tabulky ARP vidět směrovači ExpressRoute okraje. 

## <a name="prerequisites-for-learning-arp-tables"></a>Požadavky pro výuku ARP tabulkami

Zajistěte si následující před o průběhu dál

 - Platné ExpressRoute okruh nakonfigurována alespoň jeden prozkoumávání. Obvod musí být plně nakonfigurované tak, že poskytovatel připojení. Vy (nebo váš poskytovatel připojení) musí nakonfigurovali alespoň jedno z peerings (Azure osobní, Azure veřejných a Microsoft) na tento okruh.
 - Rozsahy IP adres slouží ke konfiguraci peerings (Azure osobní, Azure veřejných a Microsoft). Prohlédněte si ip adresu přiřazení příklady [ExpressRoute směrování požadavky na stránku](expressroute-routing.md) získat pochopení jak jsou namapované rozhraní na vaší straně a na straně ExpressRoute ip adres. Získat informace o peering konfigurace kontrolou [ExpressRoute prozkoumávání konfigurace stránky](expressroute-howto-routing-arm.md).
 - Informace ze sítě týmu / poskytovatel připojení na MAC adresy rozhraní použít s tyto IP adres.
 - Musí mít nejnovější modul Powershellu pro Azure (verze 1.50 nebo novější).

## <a name="getting-the-arp-tables-for-your-expressroute-circuit"></a>Získání ARP tabulkami ExpressRoute okruhem
Tato část obsahuje pokyny zobrazení ARP tabulkami na prozkoumávání pomocí Powershellu. Vy nebo váš poskytovatel připojení musí nakonfigurovali prozkoumávání před další průběh. Každý okruh má dvě cesty (primárních a sekundárních). Můžete zkontrolovat, že tabulku ARP pro každou cestu nezávisle na sobě.

### <a name="arp-tables-for-azure-private-peering"></a>ARP tabulkami pro Azure soukromé prozkoumávání
Následující rutinu poskytuje ARP tabulkami Azure soukromé prozkoumávání

        # Required Variables
        $RG = "<Your Resource Group Name Here>"
        $Name = "<Your ExpressRoute Circuit Name Here>"
        
        # ARP table for Azure private peering - Primary path
        Get-AzureRmExpressRouteCircuitARPTable -ResourceGroupName $RG -ExpressRouteCircuitName $Name -PeeringType AzurePrivatePeering -DevicePath Primary
        
        # ARP table for Azure private peering - Secodary path
        Get-AzureRmExpressRouteCircuitARPTable -ResourceGroupName $RG -ExpressRouteCircuitName $Name -PeeringType AzurePrivatePeering -DevicePath Secondary 

Ukázkový výstup je dole vidíte některé z cest

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
         10 On-Prem           10.0.0.1 ffff.eeee.dddd
          0 Microsoft         10.0.0.2 aaaa.bbbb.cccc


### <a name="arp-tables-for-azure-public-peering"></a>ARP tabulkami pro Azure veřejné prozkoumávání
Následující rutinu poskytuje ARP tabulkami Azure veřejné prozkoumávání

        # Required Variables
        $RG = "<Your Resource Group Name Here>"
        $Name = "<Your ExpressRoute Circuit Name Here>"
        
        # ARP table for Azure public peering - Primary path
        Get-AzureRmExpressRouteCircuitARPTable -ResourceGroupName $RG -ExpressRouteCircuitName $Name -PeeringType AzurePublicPeering -DevicePath Primary
        
        # ARP table for Azure public peering - Secodary path
        Get-AzureRmExpressRouteCircuitARPTable -ResourceGroupName $RG -ExpressRouteCircuitName $Name -PeeringType AzurePublicPeering -DevicePath Secondary 


Ukázkový výstup je dole vidíte některé z cest

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
         10 On-Prem           64.0.0.1 ffff.eeee.dddd
          0 Microsoft         64.0.0.2 aaaa.bbbb.cccc


### <a name="arp-tables-for-microsoft-peering"></a>ARP tabulkami pro Microsoft prozkoumávání
Následující rutinu poskytuje ARP tabulkami prozkoumávání společnosti Microsoft

        # Required Variables
        $RG = "<Your Resource Group Name Here>"
        $Name = "<Your ExpressRoute Circuit Name Here>"
        
        # ARP table for Microsoft peering - Primary path
        Get-AzureRmExpressRouteCircuitARPTable -ResourceGroupName $RG -ExpressRouteCircuitName $Name -PeeringType MicrosoftPeering -DevicePath Primary
        
        # ARP table for Microsoft peering - Secodary path
        Get-AzureRmExpressRouteCircuitARPTable -ResourceGroupName $RG -ExpressRouteCircuitName $Name -PeeringType MicrosoftPeering -DevicePath Secondary 


Ukázkový výstup je dole vidíte některé z cest

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
         10 On-Prem           65.0.0.1 ffff.eeee.dddd
          0 Microsoft         65.0.0.2 aaaa.bbbb.cccc


## <a name="how-to-use-this-information"></a>Jak používat tyto informace
Tabulka ARP prozkoumávání mohou sloužit k určení ověřte konfiguraci vrstvy 2 a připojení. Tato část obsahuje základní informace o vzhled ARP tabulkami v různých scénářích.

### <a name="arp-table-when-a-circuit-is-in-operational-state-expected-state"></a>Tabulku ARP po okruh v provozní stavu (očekávané stavu)

 - Tabulku ARP bude mít položka boční místní s platnou IP adresu a adresu MAC a podobně jako vstup pro strany společnosti Microsoft. 
 - Poslední osmičkové místní ip adresa vždycky liché číslo.
 - Poslední osmičkové ip adresu Microsoft vždycky sudé číslo.
 - Použít stejnou adresu MAC se zobrazí na straně Microsoft pro všechny 3 peerings (primární a sekundární). 


        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
         10 On-Prem           65.0.0.1 ffff.eeee.dddd
          0 Microsoft         65.0.0.2 aaaa.bbbb.cccc

### <a name="arp-table-when-on-premises--connectivity-provider-side-has-problems"></a>ARP tabulky, kdy je místní / straně poskytovatel připojení má problémy

 - Pouze jedna položka se zobrazí v tabulce ARP. Zobrazí mapování mezi adresu MAC a IP adresy používané strany společnosti Microsoft. 

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
          0 Microsoft         65.0.0.2 aaaa.bbbb.cccc

>[AZURE.NOTE] Otevřete žádost o podporu u poskytovatele připojení ladění takových problémů. 


### <a name="arp-table-when-microsoft-side-has-problems"></a>Tabulku ARP po straně Microsoft problémy

 - Neuvidíte tabulce ARP vidíte prozkoumávání případě potíží na straně Microsoft. 
 -  Otevřete požadavek podpory můžete s [podporou společnosti Microsoft](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade). Určete, jestli máte problém s připojením vrstvy 2. 

## <a name="next-steps"></a>Další kroky

 - Ověřte konfiguraci Layer 3 ExpressRoute okruhem
     - Získání směrování souhrnné ke zjištění stavu BGP relací 
     - Zobrazí směrovací tabulky a zjistit, jaký předpony inzerovány přes ExpressRoute
 - Ověřit kontrolou bajtů / se přenos dat
 - Otevřete požadavek podpory můžete s [podporou společnosti Microsoft](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) , pokud stále dochází k problémům.
