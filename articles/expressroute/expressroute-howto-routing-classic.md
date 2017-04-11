<properties
   pageTitle="Postup při konfiguraci směrování okruhem ExpressRoute pro klasické nasazení modelu pomocí prostředí PowerShell | Microsoft Azure"
   description="Tento článek vás provede kroky pro vytváření a zřízení soukromé, veřejných a Microsoft prozkoumávání obvodu ExpressRoute. Tento článek taky uvidíte, jak chcete zkontrolovat stav, aktualizace nebo odstranění peerings vaše okruhem."
   documentationCenter="na"
   services="expressroute"
   authors="ganesr"
   manager="carmonm"
   editor=""
   tags="azure-service-management"/>
<tags
   ms.service="expressroute"
   ms.devlang="na"
   ms.topic="article" 
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/10/2016"
   ms.author="ganesr"/>

# <a name="create-and-modify-routing-for-an-expressroute-circuit"></a>Vytváření a změny směrování ExpressRoute okruhem

> [AZURE.SELECTOR]
[Azure portálu - správce prostředků](expressroute-howto-routing-portal-resource-manager.md)
[prostředí PowerShell – správce prostředků](expressroute-howto-routing-arm.md)
[prostředí PowerShell – klasické](expressroute-howto-routing-classic.md)



Tento článek vás provede kroky k vytváření a správě konfigurace směrování ExpressRoute okruhem pomocí prostředí PowerShell a modelu klasické nasazení. Postupem zobrazí také můžete zkontrolovat stav, aktualizace nebo odstranění a deprovision peerings ExpressRoute okruhem.


**Modely Azure nasazení**

[AZURE.INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)] 

## <a name="configuration-prerequisites"></a>Konfigurace požadavky

- Budete potřebovat nejnovější verzi modulu Azure Powershellu. Modul nejnovější prostředí PowerShell můžete stáhnout z prostředí PowerShell část [stránky souborů ke stažení Azure](https://azure.microsoft.com/downloads/). Postupujte podle pokynů na stránce [instalace a konfigurace prostředí PowerShell Azure](../powershell-install-configure.md) podrobné pokyny ke konfiguraci počítače k použití moduly Azure Powershellu. 
- Ujistěte se, že si přečtete stránce [požadavky](expressroute-prerequisites.md) , [směrování požadavky](expressroute-routing.md) stránkou a stránkou [pracovních postupů](expressroute-workflows.md) před zahájením konfigurace.
- Máte aktivní okruh ExpressRoute. Postupujte podle pokynů a [vytvořte ExpressRoute okruh](expressroute-howto-circuit-classic.md) a mít okruh povolit tak, že váš poskytovatel připojení před pokračováním. Okruh ExpressRoute musí být ve stavu zřizování a zapnutí abyste mohli spouštět rutiny píše níže.

>[AZURE.IMPORTANT] Tyto pokyny platí pouze pro obvody vytvářené pomocí poskytovatelů služby nabízející služby připojení vrstvy 2. Pokud používáte poskytovatele služby nabízející spravovaných Layer 3 služeb (obvykle IPVPN, jako je MPLS), váš poskytovatel připojení konfigurovat a spravovat směrování za vás.

Můžete nakonfigurovat jedna, dvě nebo všechny tři peerings (Azure osobní, Azure veřejnosti a Microsoft) ExpressRoute okruhem. Konfigurace peerings v pořadí, které zvolíte. Přesvědčte se, že dokončete konfiguraci peering postupně po jednom. 

## <a name="azure-private-peering"></a>Azure soukromé prozkoumávání

Tato část obsahuje pokyny o tom, jak vytvořit, získáte, aktualizovat a odstraňovat Azure soukromé peering konfigurace ExpressRoute okruhem. 

### <a name="to-create-azure-private-peering"></a>Vytvoření Azure soukromé prozkoumávání

1. **Importujte modul Powershellu pro ExpressRoute.**
    
    Abyste mohli začít používat rutiny ExpressRoute, je nutné importovat do relaci Powershellu Azure a ExpressRoute moduly. Spusťte následující příkazy moduly Azure a ExpressRoute naimportujte relaci Powershellu.  

        Import-Module 'C:\Program Files (x86)\Microsoft SDKs\Azure\PowerShell\ServiceManagement\Azure\Azure.psd1'
        Import-Module 'C:\Program Files (x86)\Microsoft SDKs\Azure\PowerShell\ServiceManagement\Azure\ExpressRoute\ExpressRoute.psd1'

2. **Vytvoření ExpressRoute okruhem.**
    
    Postupujte podle pokynů k vytvoření [ExpressRoute obvodu](expressroute-howto-circuit-classic.md) a mít zřízení poskytovatelem připojení. Pokud váš poskytovatel připojení nabízí spravovaných služeb Layer 3, můžete požádat o poskytovatele připojení k povolení Azure soukromé prozkoumávání za vás. V takovém případě nebudete muset postupujte podle pokynů uvedených v dalších částech. Pokud váš poskytovatel připojení správu směrování, po vytvoření obvodu, musíte tyto kroky udělat. 

3. **Zaškrtněte políčko okruhem ExpressRoute zajistit, že máte k dispozici.**

    Až po zaškrtnutí zobrazíte Pokud okruh ExpressRoute zřízení a také povolené. Viz následující příklad.

        PS C:\> Get-AzureDedicatedCircuit -ServiceKey "*********************************"

        Bandwidth                        : 200
        CircuitName                      : MyTestCircuit
        Location                         : Silicon Valley
        ServiceKey                       : *********************************
        ServiceProviderName              : equinix
        ServiceProviderProvisioningState : Provisioned
        Sku                              : Standard
        Status                           : Enabled

    Ujistěte se, že obvod zobrazuje jako Provisioned a povoleno. Pokud ne, pracujete s poskytovatelem připojení k získání okruh požadovaného stavu a stav.

        ServiceProviderProvisioningState : Provisioned
        Status                           : Enabled


4. **Konfigurace Azure soukromé prozkoumávání pro obvod.**

    Zkontrolujte, jestli následující položky před pokračováním další kroky:

    - /30 podsítě primární odkazu. To nesmí být součástí všech adresní prostor rezervovaná virtuálních sítí.
    - /30 podsítě sekundární odkazu. To nesmí být součástí všech adresní prostor rezervovaná virtuálních sítí.
    - Platné ID VLAN stanovit prozkoumávání na. Zajistit, aby žádné další prozkoumávání v obvod používala stejné VLAN ID.
    - JAKO číslo prozkoumávání. Můžete použít 2bajtové i 4bajtový jako čísla. Můžete použít osobní jako číslo pro tento prozkoumávání. Ujistěte se, že nepoužíváte 65515.
    - Pokud se rozhodnete sdělit nám jednu hash MD5. **Toto je nepovinný krok**.
    
    Můžete spustit následující rutinu konfigurace Azure soukromé prozkoumávání vaší okruhem.

        New-AzureBGPPeering -AccessType Private -ServiceKey "*********************************" -PrimaryPeerSubnet "10.0.0.0/30" -SecondaryPeerSubnet "10.0.0.4/30" -PeerAsn 1234 -VlanId 100

    Pokud se rozhodnete sdělit nám algoritmus hash MD5 můžete použít rutinu dole.

        New-AzureBGPPeering -AccessType Private -ServiceKey "*********************************" -PrimaryPeerSubnet "10.0.0.0/30" -SecondaryPeerSubnet "10.0.0.4/30" -PeerAsn 1234 -VlanId 100 -SharedKey "A1B2C3D4"

    >[AZURE.IMPORTANT] Zajištění, zadejte své číslo jako prozkoumávání ASN zákazníka ASN.

### <a name="to-view-azure-private-peering-details"></a>Chcete-li zobrazit podrobnosti Azure prozkoumávání soukromé

Se zobrazí podrobnosti o konfiguraci pomocí následující rutiny

    Get-AzureBGPPeering -AccessType Private -ServiceKey "*********************************"
    
    AdvertisedPublicPrefixes       : 
    AdvertisedPublicPrefixesState  : Configured
    AzureAsn                       : 12076
    CustomerAutonomousSystemNumber : 
    PeerAsn                        : 1234
    PrimaryAzurePort               : 
    PrimaryPeerSubnet              : 10.0.0.0/30
    RoutingRegistryName            : 
    SecondaryAzurePort             : 
    SecondaryPeerSubnet            : 10.0.0.4/30
    State                          : Enabled
    VlanId                         : 100


### <a name="to-update-azure-private-peering-configuration"></a>Chcete-li aktualizovat Azure soukromé konfiguraci prozkoumávání

Je možné aktualizovat libovolnou část konfiguraci pomocí následující rutiny. V následujícím příkladu ID VLAN obvodu je aktualizován ze 100 na 500.

    Set-AzureBGPPeering -AccessType Private -ServiceKey "*********************************" -PrimaryPeerSubnet "10.0.0.0/30" -SecondaryPeerSubnet "10.0.0.4/30" -PeerAsn 1234 -VlanId 500 -SharedKey "A1B2C3D4"

### <a name="to-delete-azure-private-peering"></a>Chcete-li odstranit Azure soukromé prozkoumávání

Odebrání konfiguraci peering spusťte následující rutinu.

>[AZURE.WARNING] Ujistěte se, že jsou všechny virtuální sítě nepropojené z okruh ExpressRoute před spuštěním tuto rutinu. 

    Remove-AzureBGPPeering -AccessType Private -ServiceKey "*********************************"


## <a name="azure-public-peering"></a>Azure veřejné prozkoumávání

Tato část obsahuje pokyny o tom, jak vytvořit, získáte, aktualizovat a odstraňovat Azure veřejné peering konfigurace ExpressRoute okruhem.

### <a name="to-create-azure-public-peering"></a>Vytvoření Azure veřejné prozkoumávání

1. **Importujte modul Powershellu pro ExpressRoute.**
    
    Abyste mohli začít používat rutiny ExpressRoute, je nutné importovat do relaci Powershellu Azure a ExpressRoute moduly. Spusťte následující příkazy moduly Azure a ExpressRoute naimportujte relaci Powershellu. 

        Import-Module 'C:\Program Files (x86)\Microsoft SDKs\Azure\PowerShell\ServiceManagement\Azure\Azure.psd1'
        Import-Module 'C:\Program Files (x86)\Microsoft SDKs\Azure\PowerShell\ServiceManagement\Azure\ExpressRoute\ExpressRoute.psd1'

2. **Vytvoření ExpressRoute okruh**
    
    Postupujte podle pokynů k vytvoření [ExpressRoute obvodu](expressroute-howto-circuit-classic.md) a mít zřízení poskytovatelem připojení. Pokud váš poskytovatel připojení nabízí spravovaných služeb Layer 3, můžete požádat o poskytovatele připojení k povolení Azure veřejné prozkoumávání za vás. V takovém případě nebudete muset postupujte podle pokynů uvedených v dalších částech. Pokud váš poskytovatel připojení správu směrování, po vytvoření obvodu, musíte tyto kroky udělat.

3. **Kontrola ExpressRoute okruh zajistit, že máte k dispozici**

    Až po zaškrtnutí zobrazíte Pokud okruh ExpressRoute zřízení a také povolené. Viz následující příklad.

        PS C:\> Get-AzureDedicatedCircuit -ServiceKey "*********************************"

        Bandwidth                        : 200
        CircuitName                      : MyTestCircuit
        Location                         : Silicon Valley
        ServiceKey                       : *********************************
        ServiceProviderName              : equinix
        ServiceProviderProvisioningState : Provisioned
        Sku                              : Standard
        Status                           : Enabled

    Ujistěte se, že obvod zobrazuje jako Provisioned a povoleno. Pokud ne, pracujete s poskytovatelem připojení k získání okruh požadovaného stavu a stav.

        ServiceProviderProvisioningState : Provisioned
        Status                           : Enabled

    

4. **Konfigurace Azure veřejné prozkoumávání pro obvod**

    Zajistěte si tyto informace se nejprve dál.

    - /30 podsítě primární odkazu. Musí být platná veřejné předpony IPv4.
    - /30 podsítě sekundární odkazu. Musí být platná veřejné předpony IPv4.
    - Platné ID VLAN stanovit prozkoumávání na. Zajistit, aby žádné další prozkoumávání v obvod používala stejné VLAN ID.
    - JAKO číslo prozkoumávání. Můžete použít 2bajtové i 4bajtový jako čísla.
    - Pokud se rozhodnete sdělit nám jednu hash MD5. **Toto je nepovinný krok**.
    
    Spusťte následující rutinu konfigurace Azure veřejné prozkoumávání vaší okruhem

        New-AzureBGPPeering -AccessType Public -ServiceKey "*********************************" -PrimaryPeerSubnet "131.107.0.0/30" -SecondaryPeerSubnet "131.107.0.4/30" -PeerAsn 1234 -VlanId 200

    Pokud se rozhodnete sdělit nám algoritmus hash MD5, můžete použít rutinu dole

        New-AzureBGPPeering -AccessType Public -ServiceKey "*********************************" -PrimaryPeerSubnet "131.107.0.0/30" -SecondaryPeerSubnet "131.107.0.4/30" -PeerAsn 1234 -VlanId 200 -SharedKey "A1B2C3D4"

    >[AZURE.IMPORTANT] Ujistěte se, že zadáte své číslo jako prozkoumávání ASN a ne zákazníků ASN.

### <a name="to-view-azure-public-peering-details"></a>Chcete-li zobrazit podrobnosti Azure prozkoumávání veřejné

Se zobrazí podrobnosti o konfiguraci pomocí následující rutiny

    Get-AzureBGPPeering -AccessType Public -ServiceKey "*********************************"
    
    AdvertisedPublicPrefixes       : 
    AdvertisedPublicPrefixesState  : Configured
    AzureAsn                       : 12076
    CustomerAutonomousSystemNumber : 
    PeerAsn                        : 1234
    PrimaryAzurePort               : 
    PrimaryPeerSubnet              : 131.107.0.0/30
    RoutingRegistryName            : 
    SecondaryAzurePort             : 
    SecondaryPeerSubnet            : 131.107.0.4/30
    State                          : Enabled
    VlanId                         : 200


### <a name="to-update-azure-public-peering-configuration"></a>Chcete-li aktualizovat Azure veřejné konfigurace prozkoumávání

Je možné aktualizovat libovolnou část konfiguraci pomocí následující rutiny

    Set-AzureBGPPeering -AccessType Public -ServiceKey "*********************************" -PrimaryPeerSubnet "131.107.0.0/30" -SecondaryPeerSubnet "131.107.0.4/30" -PeerAsn 1234 -VlanId 600 -SharedKey "A1B2C3D4"

ID VLAN obvodu je aktualizován 200 na 600 ve výše uvedeném příkladu.

### <a name="to-delete-azure-public-peering"></a>Chcete-li odstranit Azure veřejné prozkoumávání

Odebrání konfiguraci peering spusťte následující rutinu

    Remove-AzureBGPPeering -AccessType Public -ServiceKey "*********************************"

## <a name="microsoft-peering"></a>Prozkoumávání společnosti Microsoft

Tato část obsahuje pokyny o tom, jak vytvořit, získáte, aktualizovat a odstraňovat peering konfigurace Microsoft ExpressRoute okruhem. 

### <a name="to-create-microsoft-peering"></a>Vytvoření prozkoumávání společnosti Microsoft

1. **Importujte modul Powershellu pro ExpressRoute.**
    
    Abyste mohli začít používat rutiny ExpressRoute, je nutné importovat do relaci Powershellu Azure a ExpressRoute moduly. Spusťte následující příkazy moduly Azure a ExpressRoute naimportujte relaci Powershellu.  

        Import-Module 'C:\Program Files (x86)\Microsoft SDKs\Azure\PowerShell\ServiceManagement\Azure\Azure.psd1'
        Import-Module 'C:\Program Files (x86)\Microsoft SDKs\Azure\PowerShell\ServiceManagement\Azure\ExpressRoute\ExpressRoute.psd1'

2. **Vytvoření ExpressRoute okruh**
    
    Postupujte podle pokynů k vytvoření [ExpressRoute obvodu](expressroute-howto-circuit-classic.md) a mít zřízení poskytovatel připojení. Pokud váš poskytovatel připojení nabízí spravovaných služeb Layer 3, můžete požádat o poskytovatele připojení k povolení Azure soukromé prozkoumávání za vás. V takovém případě nebudete muset postupujte podle pokynů uvedených v dalších částech. Pokud váš poskytovatel připojení správu směrování, po vytvoření obvodu, musíte tyto kroky udělat.

3. **Kontrola ExpressRoute okruhem zajistit, že máte k dispozici**

    Nejdřív musí zkontrolujte, zda okruh ExpressRoute je ve stavu Provisioned a povoleno.

        PS C:\> Get-AzureDedicatedCircuit -ServiceKey "*********************************"

        Bandwidth                        : 200
        CircuitName                      : MyTestCircuit
        Location                         : Silicon Valley
        ServiceKey                       : *********************************
        ServiceProviderName              : equinix
        ServiceProviderProvisioningState : Provisioned
        Sku                              : Standard
        Status                           : Enabled

    Ujistěte se, že obvod zobrazuje jako Provisioned a povoleno. Pokud ne, pracujete s poskytovatelem připojení k získání okruh požadovaného stavu a stav.

        ServiceProviderProvisioningState : Provisioned
        Status                           : Enabled


4. **Konfigurace Microsoft prozkoumávání pro obvod**

    Zkontrolujte, jestli tyto informace, než budete pokračovat.

    - /30 podsítě primárního odkazu. Musí být platná veřejné IPv4 předponu vlastníte a registrován RIR / míra.výnosnosti.
    - /30 podsítě sekundární odkazu. Musí být platná veřejné IPv4 předponu vlastníte a registrován RIR / míra.výnosnosti.
    - Platné ID VLAN stanovit prozkoumávání na. Zajistit, aby žádné další prozkoumávání v obvodu používala stejné VLAN ID.
    - JAKO číslo prozkoumávání. Můžete použít 2bajtové a 4bajtový jako čísla.
    - Inzerovaných předpony: je nutné zadat seznam všech předpony plánujete inzerce BGP relace. Jenom veřejné předpony adres IP přijaté. Pokud chcete odeslat sadu předpony, můžete mu poslat čárkou oddělený seznam. Tyto předpony musí být registrována pro vás v RIR / míra.
    - Zákazník ASN: Pokud jste předpony zobrazování reklam, které nejsou registrované na prozkoumávání jako číslo, můžete zadat jako číslo, na které jsou registrované. **Toto je nepovinný krok**.
    - Název směrování registru: Můžete zadat RIR / míra.výnosnosti proti které AS jsou registrované číslo a předpony.
    - Algoritmus hash MD5, pokud se rozhodnete sdělit nám jednoho. **Toto je nepovinný krok.**
    
    Spusťte následující rutinu konfigurace pering Microsoft pro vaše okruh

        New-AzureBGPPeering -AccessType Microsoft -ServiceKey "*********************************" -PrimaryPeerSubnet "131.107.0.0/30" -SecondaryPeerSubnet "131.107.0.4/30" -VlanId 300 -PeerAsn 1234 -CustomerAsn 2245 -AdvertisedPublicPrefixes "123.0.0.0/30" -RoutingRegistryName "ARIN" -SharedKey "A1B2C3D4"


### <a name="to-view-microsoft-peering-details"></a>Chcete-li zobrazit podrobnosti prozkoumávání společnosti Microsoft

Podrobnosti o konfiguraci pomocí následující rutinu, můžete získat.

    Get-AzureBGPPeering -AccessType Microsoft -ServiceKey "*********************************"
    
    AdvertisedPublicPrefixes       : 123.0.0.0/30
    AdvertisedPublicPrefixesState  : Configured
    AzureAsn                       : 12076
    CustomerAutonomousSystemNumber : 2245
    PeerAsn                        : 1234
    PrimaryAzurePort               : 
    PrimaryPeerSubnet              : 10.0.0.0/30
    RoutingRegistryName            : ARIN
    SecondaryAzurePort             : 
    SecondaryPeerSubnet            : 10.0.0.4/30
    State                          : Enabled
    VlanId                         : 300


### <a name="to-update-microsoft-peering-configuration"></a>Aktualizaci Microsoft prozkoumávání konfigurace

Je možné aktualizovat libovolnou část konfiguraci pomocí následující rutiny.

        Set-AzureBGPPeering -AccessType Microsoft -ServiceKey "*********************************" -PrimaryPeerSubnet "131.107.0.0/30" -SecondaryPeerSubnet "131.107.0.4/30" -VlanId 300 -PeerAsn 1234 -CustomerAsn 2245 -AdvertisedPublicPrefixes "123.0.0.0/30" -RoutingRegistryName "ARIN" -SharedKey "A1B2C3D4"

### <a name="to-delete-microsoft-peering"></a>Chcete-li odstranit prozkoumávání společnosti Microsoft

Odebrání konfiguraci peering spusťte následující rutinu.

    Remove-AzureBGPPeering -AccessType Microsoft -ServiceKey "*********************************"

## <a name="next-steps"></a>Další kroky

Další, [odkaz VNet ExpressRoute obvodu](expressroute-howto-linkvnet-classic.md).


-  Další informace o pracovních postupech najdete v tématu [ExpressRoute pracovní postupy](expressroute-workflows.md).
-  Další informace o okruhem prozkoumávání najdete v článku [ExpressRoute obvody a směrování domény](expressroute-circuit-peerings.md).

