<properties
   pageTitle="ExpressRoute Příručka pro řešení potíží: získání ARP tabulkami | Microsoft Azure"
   description="Tato stránka obsahuje pokyny k získání ARP tabulkami ExpressRoute okruhem."
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

# <a name="expressroute-troubleshooting-guide-getting-arp-tables-in-the-classic-deployment-model"></a>Příručka ExpressRoute pro řešení potíží: získání ARP tabulkami v modelu klasické nasazení

> [AZURE.SELECTOR]
[Prostředí PowerShell – správce prostředků](expressroute-troubleshooting-arp-resource-manager.md)
[prostředí PowerShell – klasické](expressroute-troubleshooting-arp-classic.md)

Tento článek vás provede kroky pro získání adresy rozlišení ARP (Protocol) tabulek Azure ExpressRoute okruhem.

>[AZURE.IMPORTANT] Tento dokument je určen vám Diagnostika a oprava problémů s jednoduché. Není určená k jako náhrada technické podpory společnosti Microsoft. Pokud se nemůžete problém nevyřeší pomocí následující pokyny, otevřete žádost o podporu s [Microsoft Azure nápovědy + podpory](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).

## <a name="address-resolution-protocol-arp-and-arp-tables"></a>Adresa rozlišení ARP (Protocol) a ARP tabulkami
ARP je vrstvy 2 protokol, který je definován v [RFC 826](https://tools.ietf.org/html/rfc826). Slouží k mapování Ethernet adresa (MAC) na IP adresu.

Tabulka ARP obsahuje mapování adresy IPv4 a adresa MAC pro konkrétní prozkoumávání. V tabulce ARP okruh ExpressRoute prozkoumávání poskytuje následující údaje pro každý rozhraní (primárních a sekundárních):

1. Mapování adresy místní směrovači rozhraní IP adresu MAC.
2. Mapování adresy ExpressRoute směrovači rozhraní IP adresu MAC.
3. Stáří mapování

ARP tabulkami můžou pomoct s ověření konfigurace 2 vrstvy a řešení potíží s základní problémy s připojením vrstvy 2.

Následuje příklad ARP tabulky:

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
         10 On-Prem           10.0.0.1 ffff.eeee.dddd
          0 Microsoft         10.0.0.2 aaaa.bbbb.cccc


V následující části obsahuje informace o tom, jak zobrazit ARP tabulky, které se zobrazují směrovači ExpressRoute okraje.

## <a name="prerequisites-for-using-arp-tables"></a>Požadavky pro práci s ARP tabulkami

Zajistěte si následující před pokračováním:

 - Platné obvod ExpressRoute nakonfigurovaný s alespoň jeden prozkoumávání. Obvod musí být plně nakonfigurované tak, že poskytovatel připojení. Vy (nebo váš poskytovatel připojení) nakonfigurujte alespoň jedno z peerings (Azure veřejná Azure, soukromé nebo Microsoft) na tento okruh.

 - Rozsahy IP adres, které se používají pro konfiguraci peerings (Azure osobní, Azure veřejných a Microsoft). Prohlédněte si IP adresu přiřazení příklady [ExpressRoute směrování požadavky na stránku](expressroute-routing.md) získat pochopení jak jsou namapované rozhraní na vaše aise a na straně ExpressRoute IP adres. Získat informace o peering konfiguraci kontrolou [ExpressRoute prozkoumávání konfigurace stránky](expressroute-howto-routing-classic.md).

 - Informace z poskytovatele týmu nebo připojení sítě adresy MAC rozhraní, které se používají s tyto IP adres.

 - Nejnovější modul Windows PowerShell pro Azure (verze 1.50 nebo novější).

## <a name="arp-tables-for-your-expressroute-circuit"></a>ARP tabulkami pro ExpressRoute okruh
Tato část obsahuje pokyny k tomu, jak zobrazit ARP tabulkami u jednotlivých typů prozkoumávání pomocí prostředí PowerShell. Předtím, než budete pokračovat, vy nebo váš poskytovatel připojení potřebuje konfigurace prozkoumávání. Každý okruh má dvě cesty (primárních a sekundárních). Můžete zkontrolovat, že tabulku ARP pro každou cestu nezávisle na sobě.

### <a name="arp-tables-for-azure-private-peering"></a>ARP tabulkami pro Azure soukromé prozkoumávání
Následující rutinu poskytuje ARP tabulkami Azure soukromé prozkoumávání:

        # Required variables
        $ckt = "<your Service Key here>

        # ARP table for Azure private peering--primary path
        Get-AzureDedicatedCircuitPeeringArpInfo -ServiceKey $ckt -AccessType Private -Path Primary

        # ARP table for Azure private peering--secondary path
        Get-AzureDedicatedCircuitPeeringArpInfo -ServiceKey $ckt -AccessType Private -Path Secondary

Následuje ukázkový výstup pro jeden z postupů:

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
         10 On-Prem           10.0.0.1 ffff.eeee.dddd
          0 Microsoft         10.0.0.2 aaaa.bbbb.cccc


### <a name="arp-tables-for-azure-public-peering"></a>ARP tabulkami pro Azure veřejné prozkoumávání:
Následující rutinu poskytuje ARP tabulkami Azure veřejné prozkoumávání:

        # Required variables
        $ckt = "<your Service Key here>

        # ARP table for Azure public peering--primary path
        Get-AzureDedicatedCircuitPeeringArpInfo -ServiceKey $ckt -AccessType Public -Path Primary

        # ARP table for Azure public peering--secondary path
        Get-AzureDedicatedCircuitPeeringArpInfo -ServiceKey $ckt -AccessType Public -Path Secondary

Následuje příklad výstupu jeden z postupů:

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
         10 On-Prem           10.0.0.1 ffff.eeee.dddd
          0 Microsoft         10.0.0.2 aaaa.bbbb.cccc


Následuje příklad výstupu jeden z postupů:

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
         10 On-Prem           64.0.0.1 ffff.eeee.dddd
          0 Microsoft         64.0.0.2 aaaa.bbbb.cccc


### <a name="arp-tables-for-microsoft-peering"></a>ARP tabulkami pro Microsoft prozkoumávání
Následující rutinu poskytuje ARP tabulkami prozkoumávání Microsoft:

    # ARP table for Microsoft peering--primary path
    Get-AzureDedicatedCircuitPeeringArpInfo -ServiceKey $ckt -AccessType Microsoft -Path Primary

    # ARP table for Microsoft peering--secondary path
    Get-AzureDedicatedCircuitPeeringArpInfo -ServiceKey $ckt -AccessType Microsoft -Path Secondary


Ukázkový výstup se zobrazují pod pro jeden z postupů:

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
         10 On-Prem           65.0.0.1 ffff.eeee.dddd
          0 Microsoft         65.0.0.2 aaaa.bbbb.cccc


## <a name="how-to-use-this-information"></a>Jak používat tyto informace
Tabulka ARP prozkoumávání mohou sloužit k ověření konfigurace 2 vrstvy a připojení. Tato část obsahuje základní informace o vzhled ARP tabulkami v různých scénářích.

### <a name="arp-table-when-a-circuit-is-in-an-operational-expected-state"></a>Tabulku ARP po okruh ve stavu provozní (očekávané)

 - Tabulku ARP má položka boční místní platnou adresou IP a MAC a podobně jako vstup pro strany společnosti Microsoft.
 - Poslední osmičkové místní IP adresa je vždy liché číslo.
 - Poslední osmičkové Microsoft IP adresu je vždy sudé číslo.
 - Použít stejnou adresu MAC se zobrazí na straně Microsoft pro všechny tři peerings (primární a sekundární).


        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
         10 On-Prem           65.0.0.1 ffff.eeee.dddd
          0 Microsoft         65.0.0.2 aaaa.bbbb.cccc

### <a name="arp-table-when-its-on-premises-or-when-the-connectivity-provider-side-has-problems"></a>Tabulku ARP zápisu místní nebo po straně připojení zprostředkovatele má problémy

 Pouze jedna položka se zobrazí v tabulce ARP. Zobrazuje mapování mezi adresu MAC a IP adresu, který se používá na straně Microsoft.

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
          0 Microsoft         65.0.0.2 aaaa.bbbb.cccc

>[AZURE.NOTE] Pokud se vyskytne problém takto, otevřete žádost o podporu u poskytovatele připojení ji řešit.


### <a name="arp-table-when-the-microsoft-side-has-problems"></a>Tabulku ARP po straně Microsoft problémy

 - Neuvidíte tabulku ARP vidíte prozkoumávání případě potíží na straně Microsoft.
 -  Otevřete žádost o podporu s [Microsoft Azure nápovědy + podpory](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade). Určete, jestli máte problém s připojením vrstvy 2.

## <a name="next-steps"></a>Další kroky

 - Ověřte konfiguraci Layer 3 pro ExpressRoute okruh:
     - Pokud potřebujete postup souhrnné ke zjištění stavu BGP relace.
     - Pokud potřebujete tabulku směrování a zjistit, jaký předpony inzerovány přes ExpressRoute.
 - Přenos dat ověřte kontrolou bajtů nebo zmenšit.
 - Otevřete žádost o podporu s [Microsoft Azure nápovědy + podpory](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) Pokud stále dochází k problémům.
