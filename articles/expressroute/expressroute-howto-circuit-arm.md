<properties
   pageTitle="Vytváření a změny ExpressRoute okruh pomocí Správce zdrojů a prostředí PowerShell | Microsoft Azure"
   description="Tento článek popisuje, jak vytvořit zřízení, ověřte, aktualizovat, odstranit a deprovision ExpressRoute okruh."
   documentationCenter="na"
   services="expressroute"
   authors="ganesr"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager"/>
<tags
   ms.service="expressroute"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/10/2016"
   ms.author="ganesr"/>


# <a name="create-and-modify-an-expressroute-circuit"></a>Vytváření a změny ExpressRoute okruh

> [AZURE.SELECTOR]
[Azure portálu - správce prostředků](expressroute-howto-circuit-portal-resource-manager.md)
[prostředí PowerShell – správce prostředků](expressroute-howto-circuit-arm.md)
[prostředí PowerShell – klasické](expressroute-howto-circuit-classic.md)


Tento článek popisuje, jak vytvořit Azure ExpressRoute okruh pomocí rutin prostředí Windows PowerShell a nasazení modelu správce prostředků Azure. Tento článek se také vidíte, jak můžete zkontrolovat stav obvodu, aktualizovat, nebo odstranit a deprovision ho.

**Modely Azure nasazení**

[AZURE.INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)]

## <a name="before-you-begin"></a>Než začnete


- Nejnovější verzi modulu Azure PowerShell (minimální verze 1.0). Podrobné pokyny pro nastavení počítače k použití Powershellu moduly postupujte podle pokynů v [článku jak instalace a konfigurace prostředí PowerShell Azure](../powershell-install-configure.md).

- Seznamte se s [požadavky](expressroute-prerequisites.md) a [pracovní postupy](expressroute-workflows.md) před zahájením konfigurace.

## <a name="create-and-provision-an-expressroute-circuit"></a>Vytvoření a zřízení ExpressRoute okruh

### <a name="1-sign-in-to-your-azure-account-and-select-your-subscription"></a>1. přihlaste ke svému účtu Azure a vyberte předplatného

Začněte konfiguraci, přihlaste se k účtu Azure. Další informace o Powershellu najdete v tématu [Pomocí Windows Powershellu pomocí Správce prostředků](../powershell-azure-resource-manager.md). Pomocí následujících příkladech můžete připojit:

    Login-AzureRmAccount

Zkontrolujte předplatná pro účet:

    Get-AzureRmSubscription

Vyberte předplatné, které chcete vytvořit okruh ExpressRoute pro:

    Select-AzureRmSubscription -SubscriptionId "<subscription ID>"

### <a name="2-get-the-list-of-supported-providers-locations-and-bandwidths"></a>2. vytvořte požadovaný seznam podporovaných zprostředkovatelé, umístění a šířek pásma

Před vytvořením ExpressRoute okruh musíte seznam podporovaných připojení poskytovatelů, umístění a možnosti šířky pásma.

Rutiny prostředí PowerShell `Get-AzureRmExpressRouteServiceProvider` vrátí tyto informace, které budete používat na pozdější kroky:

    Get-AzureRmExpressRouteServiceProvider

Zkontrolujte, pokud váš poskytovatel připojení tam uvedených. Poznamenejte si následující informace vzhledem k tomu, že budete chtít později při vytváření okruh:

- Jméno

- PeeringLocations

- BandwidthsOffered

Teď jste připravení na vytvoření ExpressRoute okruh.   

### <a name="3-create-an-expressroute-circuit"></a>3. ExpressRoute okruhem vytvořit

Pokud ještě nemáte skupina zdroje, můžete třeba vytvořit před vytvořením ExpressRoute okruh. Můžete to udělat pomocí tohoto příkazu:


    New-AzureRmResourceGroup -Name "ExpressRouteResourceGroup" -Location "West US"


Následující příklad ukazuje, jak vytvořit MB / 200 ExpressRoute okruh prostřednictvím Equinix v křemíku sedla. Pokud používáte jiného poskytovatele a různým nastavením, když vyberete jednotlivé žádosti o nahraďte tyto informace. Následující obrázek je příklad žádost o nové klíč služby:

    New-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup" -Location "West US" -SkuTier Standard -SkuFamily MeteredData -ServiceProviderName "Equinix" -PeeringLocation "Silicon Valley" -BandwidthInMbps 200

Ujistěte se, zadejte správná SKU osy a SKU řady:

- SKU osy Určuje, zda je povolena ExpressRoute standardní nebo doplněk premium ExpressRoute. Můžete použít *Standardní* získat standardní SKU nebo *Premium* pro doplněk premium.

- SKU řady Určuje typ fakturace. Zadání *Metereddata* pro plán s měřením dat a *Unlimiteddata* pro neomezený datový tarif. Poznámka: můžete změnit typ billing z *Metereddata* *Unlimiteddata*, ale nemůžete změnit typ z *Unlimiteddata* *Metereddata*.


>[AZURE.IMPORTANT] ExpressRoute okruhem se vám nebudou účtovat poplatky od okamžiku vydání klíč služby. Ujistěte se, kdy je připraven zřízení obvod poskytovatelem připojení k provedení této operace.

Odpověď obsahuje klíč služby. Podrobný popis všech parametrů, můžete získat spuštěním těchto věcí:


    get-help New-AzureRmExpressRouteCircuit -detailed


### <a name="4-list-all-expressroute-circuits"></a>4. všechny obvody ExpressRoute seznam

Pokud chcete získat seznam všech obvody ExpressRoute, které jste vytvořili, spusťte `Get-AzureRmExpressRouteCircuit` příkaz:


    Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"

Odpověď na bude vypadat podobně jako v následujícím příkladu:


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
    CircuitProvisioningState          : Enabled
    ServiceProviderProvisioningState  : NotProvisioned
    ServiceProviderNotes              :
    ServiceProviderProperties         : {
                                          "ServiceProviderName": "Equinix",
                                          "PeeringLocation": "Silicon Valley",
                                          "BandwidthInMbps": 200
                                        }
    ServiceKey                        : **************************************
    Peerings                          : []

Tyto informace kdykoli můžete načíst pomocí `Get-AzureRmExpressRouteCircuit` rutiny. Uskutečnit hovor bez parametrů obsahuje seznam všech obvody. Do pole *klíč ServiceKey* se zobrazí kód služby:


    Get-AzureRmExpressRouteCircuit


Odpověď na bude vypadat podobně jako v následujícím příkladu:


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
    ServiceProviderProvisioningState : NotProvisioned
    ServiceProviderNotes             :
    ServiceProviderProperties        : {
                                         "ServiceProviderName": "Equinix",
                                         "PeeringLocation": "Silicon Valley",
                                         "BandwidthInMbps": 200
                                       }
    ServiceKey                       : **************************************
    Peerings                         : []


Podrobný popis všech parametrů, můžete získat spuštěním těchto věcí:


    get-help Get-AzureRmExpressRouteCircuit -detailed

### <a name="5-send-the-service-key-to-your-connectivity-provider-for-provisioning"></a>5. odešlete klíč služby svého poskytovatele připojení pro zřízení

*ServiceProviderProvisioningState* obsahuje informace o aktuálním stavu zřizování na straně poskytovatele služeb. Stav poskytuje stavu na straně Microsoft. Další informace o okruh zřizování států naleznete v článku [pracovní postupy](expressroute-workflows.md#expressroute-circuit-provisioning-states) .

Při vytváření nové okruh ExpressRoute obvod bude ve stavu následující:


    ServiceProviderProvisioningState : NotProvisioned
    CircuitProvisioningState         : Enabled



Obvod se změní na následující stav, když právě povolení pro vás poskytovatel připojení:

    ServiceProviderProvisioningState : Provisioning
    Status                           : Enabled

Abyste mohli používat ExpressRoute okruh musí být v tomto stavu:

    ServiceProviderProvisioningState : Provisioned
    CircuitProvisioningState         : Enabled

### <a name="6-periodically-check-the-status-and-the-state-of-the-circuit-key"></a>6. pravidelně Kontrola stavu a stav okruh klávesy

Kontrola stavu a stav klávesy okruh značí při poskytovatele povolil vaší okruh. Po obvodu nakonfigurované, *ServiceProviderProvisioningState* zobrazuje se jako *Provisioned*, jak je vidět v následujícím příkladu:


    Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"


Odpověď na bude vypadat podobně jako v následujícím příkladu:


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

### <a name="7-create-your-routing-configuration"></a>7. konfigurace směrování vytvořit

Podrobné pokyny naleznete v článku [Konfigurace směrování okruh ExpressRoute](expressroute-howto-routing-arm.md) vytvářet a upravovat okruh peerings.


>[AZURE.IMPORTANT] Tyto pokyny platí pouze pro obvody, které jsou vytvořené pomocí poskytovatele služeb, které nabízejí služby 2 připojení vrstvy. Pokud používáte poskytovatele služeb, který nabízí spravované layer 3 služby (obvykle IP VPN, jako je MPLS), bude váš poskytovatel připojení konfigurovat a spravovat směrování za vás.

### <a name="8-link-a-virtual-network-to-an-expressroute-circuit"></a>8. propojit virtuální sítě ExpressRoute okruhem

Pak propojte virtuální sítě ExpressRoute okruhem. Použijte v článku [virtuální sítě odkazování na ExpressRoute obvody](expressroute-howto-linkvnet-arm.md) při práci s nasazení modelu správce prostředků.

## <a name="getting-the-status-of-an-expressroute-circuit"></a>Zjišťování stavu obvodu ExpressRoute

Tyto informace kdykoli můžete načíst pomocí `Get-AzureRmExpressRouteCircuit` rutiny. Uskutečnit hovor bez parametrů obsahuje seznam všech obvody.

    Get-AzureRmExpressRouteCircuit


Odpověď na bude vypadat podobně jako v následujícím příkladu:


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


Získat informace o konkrétních okruh ExpressRoute předáním zdroje skupinu název a název okruh jako parametr volání:


    Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"


Odpověď na bude vypadat podobně jako v následujícím příkladu:


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


Podrobný popis všech parametrů, můžete získat spuštěním těchto věcí:

    get-help get-azurededicatedcircuit -detailed


## <a name="modify"></a>Úprava ExpressRoute okruh

Některé vlastnosti obvodu ExpressRoute můžete změnit bez vlivu na připojení.

Můžete udělat následující s žádné výpadek služeb:

- Povolení nebo zakázání doplněk premium ExpressRoute pro ExpressRoute okruh.
- Zvětšení šířky pásma obvodu ExpressRoute. Všimněte si, že přechodu šířku pásma okruh není podporována.
- Změna měření plán z dat podle objemu dat na neomezený Data. Všimněte si, že měření plán z neomezený dat podle objemu dat dat nepodporuje se změna.
-  Můžete povolit a zakažte *Povolit klasické operace*.

Další informace o limitech a omezení podívejte se do [Nejčastější dotazy týkající se ExpressRoute](expressroute-faqs.md).

### <a name="to-enable-the-expressroute-premium-add-on"></a>Chcete-li povolit doplněk ExpressRoute premium

Povolit doplněk premium ExpressRoute existující okruhem pomocí následující úryvek Powershellu:

    $ckt = Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"

    $ckt.Sku.Tier = "Premium"
    $ckt.sku.Name = "Premium_MeteredData"

    Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt


Obvod bude mít povolený ExpressRoute prémiových doplněk funkcí. Všimněte si, že jsme začne se hned po úspěšném spuštění příkazu fakturace pro funkci doplňku premium.

### <a name="to-disable-the-expressroute-premium-add-on"></a>Zákaz doplňku ExpressRoute premium

>[AZURE.IMPORTANT] Můžete se nezdaří při používání zdrojů, které jsou větší než co je povolen standardní okruhem.

Poznámka:

- Než můžete přejít na premium standardní, musíte se ujistit, že počet virtuální sítě, které jsou propojené s obvod je menší než 10. Není to uděláte, žádosti o aktualizaci selže a můžeme vám bude účtovat sazbou premium.

- Je nutné zrušit všechny virtuální sítě v ostatních oblastech geopolitické. Není to uděláte, žádosti o aktualizace se nepovede, a můžeme vám bude účtovat sazbou premium.

- Směrování tabulky musí být menší než 4000 směruje pro soukromé prozkoumávání. Pokud velikost tabulky směrování je větší než 4 000 cesty, relace BGP vynechává a nesmí být opětovně povolena dokud počet inzerovaný předpony přechází pod 4 000.

Doplněk premium ExpressRoute existující okruhem můžete zakázat pomocí následující rutiny prostředí PowerShell:


    $ckt = Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"

    $ckt.Sku.Tier = "Standard"
    $ckt.sku.Name = "Standard_MeteredData"

    Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt


### <a name="to-update-the-expressroute-circuit-bandwidth"></a>Chcete-li aktualizovat šířky pásma s obvodovou ExpressRoute

Možnosti podporované šířky pásma pro vašeho poskytovatele najdete [Nejčastější dotazy týkající se ExpressRoute](expressroute-faqs.md). Můžete vybrat libovolné velikosti větší velikost existující okruh.

>[AZURE.IMPORTANT] Nelze zmenšení šířky pásma obvodu ExpressRoute bez přerušení. Snížení šířky pásma vyžaduje deprovision ExpressRoute obvodu a potom spravovat (reprovision) nový okruh ExpressRoute.

Až se rozhodnete, jakou velikost budete potřebovat, použijte následující příkaz změnit velikost vaší okruh:


    $ckt = Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"

    $ckt.ServiceProviderProperties.BandwidthInMbps = 1000

    Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt


Vaše okruh budou velké na straně Microsoft. Potom musíte kontaktovat svého poskytovatele připojení na aktualizovat konfigurace na jejich straně podle této změně. Když uděláte toto oznámení, jsme začne se fakturace pro možnost aktualizované šířky pásma.


### <a name="to-move-the-sku-from-metered-to-unlimited"></a>Chcete-li přesunout SKU z podle objemu dat na neomezeno

SKU obvodu ExpressRoute můžete změnit pomocí následující úryvek Powershellu:

    $ckt = Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"

    $ckt.Sku.Family = "UnlimitedData"
    $ckt.sku.Name = "Premium_UnlimitedData"

    Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt

### <a name="to-control-access-to-the-classic-and-resource-manager-environments"></a>Řízení přístupu k klasickou a správce prostředků prostředí  

Kontrola pokynů uvedených v článku [Přesunutí ExpressRoute obvody od klasického nasazení modelu správce prostředků](expressroute-howto-move-arm.md).  


## <a name="deprovisioning-and-deleting-an-expressroute-circuit"></a>Zrušení zajišťování a odstranění ExpressRoute okruh

Poznámka:

- Je nutné zrušit všechny virtuální sítí okruh ExpressRoute. Pokud tento nezdaří, zkontrolujte, pokud virtuální sítě jsou propojené s obvod.

- Když je ExpressRoute okruhem service provider zřizovací stav **Provisioning** nebo **Provisioned** musí spolupracujete poskytovatele služeb deprovision okruhem na jejich straně. Budeme dál rezervovat prostředky a vám účtovat tak, aby poskytovatel služeb dokončí zrušení zajišťování obvodu a nám s upozorněním.

- Pokud poskytovatele služeb má poskytování zrušeno elektrický obvod (zřizovací stavu poskytovatele služby je nastavena na **není zřízení**) můžete odstraňte obvod. Přestanou se vám fakturace okruhem

Spuštěním následujícího příkazu můžete odstranit ExpressRoute okruh:

    Remove-AzureRmExpressRouteCircuit -ResourceGroupName "ExpressRouteResourceGroup" -Name "ExpressRouteARMCircuit"



## <a name="next-steps"></a>Další kroky

Po vytvoření vaší okruh Ujistěte se, Uděláte to takhle:

- [Vytváření a změny směrování ExpressRoute okruhem](expressroute-howto-routing-arm.md)
- [Odkaz na ExpressRoute okruh virtuální sítě](expressroute-howto-linkvnet-arm.md)
