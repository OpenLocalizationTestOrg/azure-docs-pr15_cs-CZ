<properties
   pageTitle="Postup při konfiguraci směrování ExpressRoute okruhem | Microsoft Azure"
   description="Tento článek vás provede kroky pro vytváření a zřízení soukromé, veřejných a Microsoft prozkoumávání obvodu ExpressRoute. Tento článek taky uvidíte, jak chcete zkontrolovat stav, aktualizace nebo odstranění peerings vaše okruhem."
   documentationCenter="na"
   services="expressroute"
   authors="ganesr"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager"/>
<tags
   ms.service="expressroute"
   ms.devlang="na"
   ms.topic="hero-article" 
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/05/2016"
   ms.author="ganesr"/>

# <a name="create-and-modify-routing-for-an-expressroute-circuit"></a>Vytváření a změny směrování ExpressRoute okruhem


> [AZURE.SELECTOR]
[Azure portálu - správce prostředků](expressroute-howto-routing-portal-resource-manager.md)
[prostředí PowerShell – správce prostředků](expressroute-howto-routing-arm.md)
[prostředí PowerShell – klasické](expressroute-howto-routing-classic.md)



Tento článek vás provede kroky k vytváření a správě konfigurace směrování ExpressRoute okruhem pomocí prostředí PowerShell a nasazení modelu správce prostředků Azure.  Postupem zobrazí také můžete zkontrolovat stav, aktualizace nebo odstranění a deprovision peerings ExpressRoute okruhem. 


**Modely Azure nasazení**

[AZURE.INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)] 

## <a name="configuration-prerequisites"></a>Konfigurace požadavky

- Budete potřebovat nejnovější verzi modulu Azure PowerShell verze 1.0 nebo novější. 
- Ujistěte se, že si přečtete stránce [požadavky](expressroute-prerequisites.md) , [směrování požadavky](expressroute-routing.md) stránkou a stránkou [pracovních postupů](expressroute-workflows.md) před zahájením konfigurace.
- Máte aktivní okruh ExpressRoute. Postupujte podle pokynů na téma [vytváření ExpressRoute obvodu](expressroute-howto-circuit-arm.md) a mít okruh povolit tak, že váš poskytovatel připojení před pokračováním. Okruh ExpressRoute musí být ve stavu zřizování a zapnutí abyste mohli spouštět rutiny píše níže.

Tyto pokyny platí pouze pro obvody vytvářené pomocí poskytovatelů služby nabízející služby připojení vrstvy 2. Pokud používáte poskytovatele služby nabízející spravovaných Layer 3 služeb (obvykle IPVPN, jako je MPLS), váš poskytovatel připojení konfigurovat a spravovat směrování za vás.

>[AZURE.IMPORTANT] Jsme momentálně není uvedena peerings nakonfigurovaný tak, že poskytovatele služeb prostřednictvím portálu pro správu služby. Pracujeme na povolení této možnosti brzy bude k dispozici. U vašeho poskytovatele služeb před zkontrolujte konfigurace BGP peerings.

Můžete nakonfigurovat jedna, dvě nebo všechny tři peerings (Azure osobní, Azure veřejnosti a Microsoft) ExpressRoute okruhem. Konfigurace peerings v pořadí, které zvolíte. Přesvědčte se, že dokončete konfiguraci peering postupně po jednom. 

## <a name="azure-private-peering"></a>Azure soukromé prozkoumávání

Tato část obsahuje pokyny o tom, jak vytvořit, získáte, aktualizovat a odstraňovat Azure soukromé peering konfigurace ExpressRoute okruhem. 

### <a name="to-create-azure-private-peering"></a>Vytvoření Azure soukromé prozkoumávání

1. Importujte modul Powershellu pro ExpressRoute.
    
    Musíte nainstalovat nejnovější instalační program prostředí PowerShell z [Galerie prostředí PowerShell](http://www.powershellgallery.com/) a naimportujte moduly správce prostředků Azure relaci Powershellu abyste mohli začít používat rutiny ExpressRoute. Je třeba prostředí PowerShell spustili s oprávněními správce.

        Install-Module AzureRM

        Install-AzureRM

    Importovat všechny moduly AzureRM.* rozsahu známé sémantického verze

        Import-AzureRM

    Můžete taky jen import modulu vyberte rozsahu známé sémantického verze 
        
        Import-Module AzureRM.Network 

    Přihlášení k vašemu účtu

        Login-AzureRmAccount

    Vyberte předplatné, které chcete vytvořit ExpressRoute okruh
        
        Select-AzureRmSubscription -SubscriptionId "<subscription ID>"

2. Vytvoření ExpressRoute okruhem.
    
    Postupujte podle pokynů k vytvoření [ExpressRoute obvodu](expressroute-howto-circuit-arm.md) a mít zřízení poskytovatelem připojení. 

    Pokud váš poskytovatel připojení nabízí spravovaných služeb Layer 3, můžete požádat o poskytovatele připojení k povolení Azure soukromé prozkoumávání za vás. V takovém případě nebudete muset postupujte podle pokynů uvedených v dalších částech. Pokud váš poskytovatel připojení správu směrování, po vytvoření obvodu, musíte tyto kroky udělat. 

3. Zaškrtněte políčko okruhem ExpressRoute zajistit, že máte k dispozici.

    Až po zaškrtnutí zobrazíte Pokud okruh ExpressRoute zřízení a také povolené. Viz následující příklad.

        Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"

    Odpověď na bude podobně jako v příkladu níže:

        Name                             : ExpressRouteARMCircuit
        ResourceGroupName                : ExpressRouteResourceGroup
        Location                         : westus
        Id                               : /subscriptions/***************************/resourceGroups/ExpressRouteResourceGroup/providers/Microsoft.Network/expressRouteCircuits/ExpressRouteARMCircuit
        Etag                             : W/"################################"
        ProvisioningState                : Succeeded
        Sku                              : {
                                             "Name": "Standard_MeteredData",
                                             "Tier": "Standard",
                                             "Family": "MeteredData"
                                           }
        CircuitProvisioningState         : Enabled
        ServiceProviderProvisioningState : Provisioned
        ServiceProviderNotes             : 
        ServiceProviderProperties        : {
                                             "ServiceProviderName": "Equinix",
                                             "PeeringLocation": "Silicon Valley",
                                             "BandwidthInMbps": 200
                                           }
        ServiceKey                       : **************************************
        Peerings                         : []


4. Konfigurace Azure soukromé prozkoumávání pro obvod.

    Zkontrolujte, jestli následující položky před pokračováním další kroky:

    - /30 podsítě primární odkazu. To nesmí být součástí všech adresní prostor rezervovaná virtuálních sítí.
    - /30 podsítě sekundární odkazu. To nesmí být součástí všech adresní prostor rezervovaná virtuálních sítí.
    - Platné ID VLAN stanovit prozkoumávání na. Zajistit, aby žádné další prozkoumávání v obvod používala stejné VLAN ID.
    - JAKO číslo prozkoumávání. Můžete použít 2bajtové i 4bajtový jako čísla. Můžete použít osobní jako číslo pro tento prozkoumávání. Ujistěte se, že nepoužíváte 65515.
    - Pokud se rozhodnete sdělit nám jednu hash MD5. **Toto je nepovinný krok**.
    
    Můžete spustit následující rutinu konfigurace Azure soukromé prozkoumávání vaší okruhem.

        Add-AzureRmExpressRouteCircuitPeeringConfig -Name "AzurePrivatePeering" -Circuit $ckt -PeeringType AzurePrivatePeering -PeerASN 100 -PrimaryPeerAddressPrefix "10.0.0.0/30" -SecondaryPeerAddressPrefix "10.0.0.4/30" -VlanId 200

        Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt

    Pokud se rozhodnete sdělit nám algoritmus hash MD5 můžete použít rutinu dole.

        Add-AzureRmExpressRouteCircuitPeeringConfig -Name "AzurePrivatePeering" -Circuit $ckt -PeeringType AzurePrivatePeering -PeerASN 100 -PrimaryPeerAddressPrefix "10.0.0.0/30" -SecondaryPeerAddressPrefix "10.0.0.4/30" -VlanId 200  -SharedKey "A1B2C3D4"

        Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt

    >[AZURE.IMPORTANT] Zajištění, zadejte své číslo jako prozkoumávání ASN zákazníka ASN.

### <a name="to-view-azure-private-peering-details"></a>Chcete-li zobrazit podrobnosti Azure prozkoumávání soukromé

Se zobrazí podrobnosti o konfiguraci pomocí následující rutiny

        $ckt = Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"

        Get-AzureRmExpressRouteCircuitPeeringConfig -Name "AzurePrivatePeering" -Circuit $ckt   


### <a name="to-update-azure-private-peering-configuration"></a>Chcete-li aktualizovat Azure soukromé konfiguraci prozkoumávání

Je možné aktualizovat libovolnou část konfiguraci pomocí následující rutiny. V následujícím příkladu ID VLAN obvodu je aktualizován ze 100 na 500.

    Set-AzureRmExpressRouteCircuitPeeringConfig -Name "AzurePrivatePeering" -ExpressRouteCircuit $ckt -PeeringType AzurePrivatePeering -PeerASN 100 -PrimaryPeerAddressPrefix "10.0.0.0/30" -SecondaryPeerAddressPrefix "10.0.0.4/30" -VlanId 200

    Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt


### <a name="to-delete-azure-private-peering"></a>Chcete-li odstranit Azure soukromé prozkoumávání

Odebrání konfiguraci peering spusťte následující rutinu.

>[AZURE.WARNING] Ujistěte se, že jsou všechny virtuální sítě nepropojené z okruh ExpressRoute před spuštěním tuto rutinu. 

    Remove-AzureRmExpressRouteCircuitPeeringConfig -Name "AzurePrivatePeering" -ExpressRouteCircuit $ckt
    Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt



## <a name="azure-public-peering"></a>Azure veřejné prozkoumávání

Tato část obsahuje pokyny o tom, jak vytvořit, získáte, aktualizovat a odstraňovat Azure veřejné peering konfigurace ExpressRoute okruhem.

### <a name="to-create-azure-public-peering"></a>Vytvoření Azure veřejné prozkoumávání

1. Importujte modul Powershellu pro ExpressRoute.
    
    Musíte nainstalovat nejnovější instalační program prostředí PowerShell z [Galerie prostředí PowerShell](http://www.powershellgallery.com/) a naimportujte moduly správce prostředků Azure relaci Powershellu abyste mohli začít používat rutiny ExpressRoute. Je třeba prostředí PowerShell spustili s oprávněními správce.

        Install-Module AzureRM

        Install-AzureRM

    Importovat všechny moduly AzureRM.* rozsahu známé sémantického verze

        Import-AzureRM

    Můžete taky jen importovat vyberte modulu rozsahu známé sémantického verze 
        
        Import-Module AzureRM.Network 

    Přihlášení k vašemu účtu

        Login-AzureRmAccount

    Vyberte předplatné, které chcete vytvořit ExpressRoute okruh
        
        Select-AzureRmSubscription -SubscriptionId "<subscription ID>"

2. Vytvoření ExpressRoute okruh.
    
    Postupujte podle pokynů k vytvoření [ExpressRoute obvodu](expressroute-howto-circuit-arm.md) a mít zřízení poskytovatel připojení. 

    Pokud váš poskytovatel připojení nabízí spravovaných služeb Layer 3, můžete požádat o poskytovatele připojení k povolení Azure veřejné prozkoumávání za vás. V takovém případě nebudete muset postupujte podle pokynů uvedených v dalších částech. Pokud váš poskytovatel připojení správu směrování, po vytvoření obvodu, musíte tyto kroky udělat.

3. Zaškrtněte políčko ExpressRoute okruh zajistit, že máte k dispozici.

    Až po zaškrtnutí zobrazíte-li okruhem ExpressRoute zřízení a také povolené. Viz následující příklad.

        Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"

    Odpověď na bude podobně jako v příkladu níže:

        Name                             : ExpressRouteARMCircuit
        ResourceGroupName                : ExpressRouteResourceGroup
        Location                         : westus
        Id                               : /subscriptions/***************************/resourceGroups/ExpressRouteResourceGroup/providers/Microsoft.Network/expressRouteCircuits/ExpressRouteARMCircuit
        Etag                             : W/"################################"
        ProvisioningState                : Succeeded
        Sku                              : {
                                             "Name": "Standard_MeteredData",
                                             "Tier": "Standard",
                                             "Family": "MeteredData"
                                           }
        CircuitProvisioningState         : Enabled
        ServiceProviderProvisioningState : Provisioned
        ServiceProviderNotes             : 
        ServiceProviderProperties        : {
                                             "ServiceProviderName": "Equinix",
                                             "PeeringLocation": "Silicon Valley",
                                             "BandwidthInMbps": 200
                                           }
        ServiceKey                       : **************************************
        Peerings                         : []   

4. Konfigurace Azure veřejné prozkoumávání pro obvod.

    Zajistěte si tyto informace se nejprve dál.

    - /30 podsítě primární odkazu. Musí být platná veřejné předpony IPv4.
    - /30 podsítě sekundární odkazu. Musí být platná veřejné předpony IPv4.
    - Platné ID VLAN stanovit prozkoumávání na. Zajistit, aby žádné další prozkoumávání v obvod používala stejné VLAN ID.
    - JAKO číslo prozkoumávání. Můžete použít 2bajtové a 4bajtový jako čísla.
    - Pokud se rozhodnete sdělit nám jednu hash MD5. **Toto je nepovinný krok**.
    
    Spusťte následující rutinu konfigurace Azure veřejné prozkoumávání vaší okruhem

        Add-AzureRmExpressRouteCircuitPeeringConfig -Name "AzurePublicPeering" -ExpressRouteCircuit $ckt -PeeringType AzurePublicPeering -PeerASN 100 -PrimaryPeerAddressPrefix "12.0.0.0/30" -SecondaryPeerAddressPrefix "12.0.0.4/30" -VlanId 100

        Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt

    Pokud se rozhodnete sdělit nám algoritmus hash MD5, můžete použít rutinu dole

        Add-AzureRmExpressRouteCircuitPeeringConfig -Name "AzurePublicPeering" -ExpressRouteCircuit $ckt -PeeringType AzurePublicPeering -PeerASN 100 -PrimaryPeerAddressPrefix "12.0.0.0/30" -SecondaryPeerAddressPrefix "12.0.0.4/30" -VlanId 100  -SharedKey "A1B2C3D4"

        Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt


    >[AZURE.IMPORTANT] Ujistěte se, že zadáte své číslo jako prozkoumávání ASN a ne zákazníků ASN.

### <a name="to-view-azure-public-peering-details"></a>Chcete-li zobrazit podrobnosti Azure prozkoumávání veřejné

Se zobrazí podrobnosti o konfiguraci pomocí následující rutiny

        $ckt = Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"

        Get-AzureRmExpressRouteCircuitPeeringConfig -Name "AzurePublicPeering" -Circuit $ckt


### <a name="to-update-azure-public-peering-configuration"></a>Chcete-li aktualizovat Azure veřejné konfiguraci prozkoumávání

Je možné aktualizovat libovolnou část konfiguraci pomocí následující rutiny

    Set-AzureRmExpressRouteCircuitPeeringConfig  -Name "MicrosoftPeering" -ExpressRouteCircuit $ckt -PeeringType MicrosoftPeering -PeerASN 100 -PrimaryPeerAddressPrefix "123.0.0.0/30" -SecondaryPeerAddressPrefix "123.0.0.4/30" -VlanId 600 

    Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt

ID VLAN obvodu je aktualizován 200 na 600 ve výše uvedeném příkladu.

### <a name="to-delete-azure-public-peering"></a>Chcete-li odstranit Azure veřejné prozkoumávání

Odebrání konfiguraci peering spusťte následující rutinu

    Remove-AzureRmExpressRouteCircuitPeeringConfig -Name "AzurePublicPeering" -ExpressRouteCircuit $ckt
    Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt

## <a name="microsoft-peering"></a>Prozkoumávání společnosti Microsoft

Tato část obsahuje pokyny o tom, jak vytvořit, získáte, aktualizovat a odstraňovat peering konfigurace Microsoft ExpressRoute okruhem. 

### <a name="to-create-microsoft-peering"></a>Vytvoření prozkoumávání společnosti Microsoft

1. Importujte modul Powershellu pro ExpressRoute.
    
    Musíte nainstalovat nejnovější instalační program prostředí PowerShell z [Galerie prostředí PowerShell](http://www.powershellgallery.com/) a naimportujte moduly správce prostředků Azure relaci Powershellu abyste mohli začít používat rutiny ExpressRoute. Je třeba prostředí PowerShell spustili s oprávněními správce.

        Install-Module AzureRM

        Install-AzureRM

    Importovat všechny moduly AzureRM.* rozsahu známé sémantického verze

        Import-AzureRM

    Můžete taky jen import modulu vyberte rozsahu známé sémantického verze 
        
        Import-Module AzureRM.Network 

    Přihlášení k vašemu účtu

        Login-AzureRmAccount

    Vyberte předplatné, které chcete vytvořit ExpressRoute okruh
        
        Select-AzureRmSubscription -SubscriptionId "<subscription ID>"

2. Vytvoření ExpressRoute okruhem.
    
    Postupujte podle pokynů k vytvoření [ExpressRoute obvodu](expressroute-howto-circuit-arm.md) a mít zřízení poskytovatelem připojení. 

    Pokud váš poskytovatel připojení nabízí spravovaných služeb Layer 3, můžete požádat o poskytovatele připojení k povolení Azure soukromé prozkoumávání za vás. V takovém případě nebudete muset postupujte podle pokynů uvedených v dalších částech. Pokud váš poskytovatel připojení správu směrování, po vytvoření obvodu, musíte tyto kroky udělat.

3. Zaškrtněte políčko ExpressRoute okruh zajistit, že máte k dispozici.

    Až po zaškrtnutí zobrazíte Pokud okruh ExpressRoute zřízení a také povolené. Viz následující příklad.

        Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"

    Odpověď na bude podobně jako v příkladu níže:

        Name                             : ExpressRouteARMCircuit
        ResourceGroupName                : ExpressRouteResourceGroup
        Location                         : westus
        Id                               : /subscriptions/***************************/resourceGroups/ExpressRouteResourceGroup/providers/Microsoft.Network/expressRouteCircuits/ExpressRouteARMCircuit
        Etag                             : W/"################################"
        ProvisioningState                : Succeeded
        Sku                              : {
                                             "Name": "Standard_MeteredData",
                                             "Tier": "Standard",
                                             "Family": "MeteredData"
                                           }
        CircuitProvisioningState         : Enabled
        ServiceProviderProvisioningState : Provisioned
        ServiceProviderNotes             : 
        ServiceProviderProperties        : {
                                             "ServiceProviderName": "Equinix",
                                             "PeeringLocation": "Silicon Valley",
                                             "BandwidthInMbps": 200
                                           }
        ServiceKey                       : **************************************
        Peerings                         : []   
4. Konfigurace Microsoft prozkoumávání pro obvod.

    Zkontrolujte, jestli tyto informace, než budete pokračovat.

    - /30 podsítě primární odkazu. Musí být platná veřejné IPv4 předponu vlastníte a registrován RIR / míra.výnosnosti.
    - /30 podsítě sekundární odkazu. Musí být platná veřejné IPv4 předponu vlastníte a registrován RIR / míra.výnosnosti.
    - Platné ID VLAN stanovit prozkoumávání na. Zajistit, aby žádné další prozkoumávání v obvod používala stejné VLAN ID.
    - JAKO číslo prozkoumávání. Můžete použít 2bajtové i 4bajtový jako čísla.
    - Inzerovaných předpony: je nutné zadat seznam všech předpony plánujete inzerce BGP relace. Jenom veřejné předpony adres IP přijaté. Pokud chcete odeslat sadu předpony, můžete mu poslat čárkou oddělený seznam. Tyto předpony musí registrované pro vás v RIR / míra.výnosnosti.
    - Zákazník ASN: Pokud jste předpony zobrazování reklam, které nejsou registrované na prozkoumávání jako číslo, můžete zadat jako číslo, ke kterému jsou registrované. **Toto je nepovinný krok**.
    - Název směrování registru: Můžete zadat RIR / míra.výnosnosti proti které AS jsou registrované číslo a předpony.
    - Hash MD5, pokud se rozhodnete sdělit nám jednu. **Toto je nepovinný krok.**
    
    Spusťte následující rutinu pro nastavení Microsoft prozkoumávání vaší okruhem

        Add-AzureRmExpressRouteCircuitPeeringConfig -Name "MicrosoftPeering" -ExpressRouteCircuit $ckt -PeeringType MicrosoftPeering -PeerASN 100 -PrimaryPeerAddressPrefix "123.0.0.0/30" -SecondaryPeerAddressPrefix "123.0.0.4/30" -VlanId 300 -MicrosoftConfigAdvertisedPublicPrefixes "123.1.0.0/24" -MicrosoftConfigCustomerAsn 23 -MicrosoftConfigRoutingRegistryName "ARIN"

        Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt


### <a name="to-get-microsoft-peering-details"></a>Chcete-li získat Microsoft prozkoumávání podrobnosti

Podrobnosti o konfiguraci pomocí následující rutinu, můžete získat.

        $ckt = Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"

        Get-AzureRmExpressRouteCircuitPeeringConfig -Name "MicrosoftPeering" -ExpressRouteCircuit $ckt


### <a name="to-update-microsoft-peering-configuration"></a>Chcete-li aktualizovat konfigurace prozkoumávání Microsoft

Je možné aktualizovat libovolnou část konfiguraci pomocí následující rutiny.

        Set-AzureRmExpressRouteCircuitPeeringConfig  -Name "MicrosoftPeering" -ExpressRouteCircuit $ckt -PeeringType MicrosoftPeering -PeerASN 100 -PrimaryPeerAddressPrefix "123.0.0.0/30" -SecondaryPeerAddressPrefix "123.0.0.4/30" -VlanId 300 -MicrosoftConfigAdvertisedPublicPrefixes "124.1.0.0/24" -MicrosoftConfigCustomerAsn 23 -MicrosoftConfigRoutingRegistryName "ARIN"

        Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt
        

### <a name="to-delete-microsoft-peering"></a>Chcete-li odstranit prozkoumávání společnosti Microsoft

Odebrání konfiguraci peering spusťte následující rutinu.

    Remove-AzureRmExpressRouteCircuitPeeringConfig -Name "MicrosoftPeering" -ExpressRouteCircuit $ckt

    Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt

## <a name="next-steps"></a>Další kroky

Dalším krokem [odkaz VNet ExpressRoute obvodu](expressroute-howto-linkvnet-arm.md).

-  Další informace o pracovních postupech ExpressRoute najdete v článku [pracovní postupy ExpressRoute](expressroute-workflows.md).

-  Další informace o okruh prozkoumávání najdete v článku [ExpressRoute obvody a směrování domény](expressroute-circuit-peerings.md).

-  Další informace o práci s virtuálních sítí najdete v článku [Přehled virtuální sítě](../virtual-network/virtual-networks-overview.md).

