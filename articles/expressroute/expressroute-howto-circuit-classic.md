<properties
   pageTitle="Vytváření a změny ExpressRoute okruh pomocí klasické nasazení modelu a prostředí PowerShell | Microsoft Azure"
   description="Tento článek vás provede kroky pro vytváření a zřízení ExpressRoute okruh. Tento článek taky uvidíte, jak chcete zkontrolovat stav, aktualizace nebo odstranění a deprovision vaše obvodů."
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
   ms.author="ganesr;cherylmc"/>

# <a name="create-and-modify-an-expressroute-circuit"></a>Vytváření a změny ExpressRoute okruh

> [AZURE.SELECTOR]
[Azure portálu - správce prostředků](expressroute-howto-circuit-portal-resource-manager.md)
[prostředí PowerShell – správce prostředků](expressroute-howto-circuit-arm.md)
[prostředí PowerShell – klasické](expressroute-howto-circuit-classic.md)

Tento článek vás provede postup vytvoření Azure ExpressRoute okruh pomocí rutin prostředí PowerShell nebo klasická nasazení modelu. Tento článek se také vidíte, jak chcete zkontrolovat stav, aktualizace nebo odstranění a deprovision ExpressRoute okruh.

**Modely Azure nasazení**

[AZURE.INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)]


## <a name="before-you-begin"></a>Než začnete

### <a name="1-review-the-prerequisites-and-workflow-articles"></a>1. Seznamte se s požadavky a články pracovního postupu

Ujistěte se, že si přečtete [požadavky](expressroute-prerequisites.md) a [pracovní postupy](expressroute-workflows.md) před zahájením konfigurace.  


### <a name="2-install-the-latest-versions-of-the-azure-powershell-modules"></a>2. Nainstalujte nejnovější verzi Azure PowerShell moduly 

Postupujte podle pokynů v [tom, jak nainstalovat a nakonfigurovat Azure PowerShell](../powershell-install-configure.md) podrobné pokyny ke konfiguraci počítače k použití moduly Azure Powershellu.

### <a name="3-log-in-to-your-azure-account-and-select-a-subscription"></a>3. přihlásit ke svému účtu Azure a vyberte předplatné

1. Spusťte následující rutinu pomocí řádek se zvýšenými oprávněními prostředí Windows PowerShell:

        Add-AzureAccount
2. Na přihlašovací obrazovce, která se zobrazí Přihlaste se ke svému účtu.

3. Dostaňte seznam předplatné.

        Get-AzureSubscription
4. Vyberte předplatné, které chcete použít.
    
        Select-AzureSubscription -SubscriptionName "mysubscriptionname"

## <a name="create-and-provision-an-expressroute-circuit"></a>Vytvoření a zřízení ExpressRoute okruh

### <a name="1-import-the-powershell-modules-for-expressroute"></a>1. importovat moduly Powershellu pro ExpressRoute

 Pokud jste to ještě neudělali, je nutné importovat moduly Azure a ExpressRoute relaci Powershellu abyste mohli začít používat rutiny ExpressRoute. Import moduly z umístění, které se nainstalovaly ve vašem počítači. V závislosti na metodě, který jste použili k instalaci moduly umístění se liší od v následujícím příkladu. Podle potřeby můžete změnit v příkladu.  

    Import-Module 'C:\Program Files (x86)\Microsoft SDKs\Azure\PowerShell\ServiceManagement\Azure\Azure.psd1'
    Import-Module 'C:\Program Files (x86)\Microsoft SDKs\Azure\PowerShell\ServiceManagement\Azure\ExpressRoute\ExpressRoute.psd1'

### <a name="2-get-the-list-of-supported-providers-locations-and-bandwidths"></a>2. vytvořte požadovaný seznam podporovaných poskytovatelů, umístění a šířek pásma

Před vytvořením ExpressRoute okruh musíte seznam podporovaných připojení poskytovatelů, umístění a možnosti šířky pásma.

Rutiny prostředí PowerShell `Get-AzureDedicatedCircuitServiceProvider` vrátí tyto informace, které budete používat v dalších krocích:

    Get-AzureDedicatedCircuitServiceProvider

Zkontrolujte, pokud váš poskytovatel připojení tam uvedených. Poznamenejte si následující informace vzhledem k tomu, že budete chtít později při vytváření okruh:

- Jméno

- PeeringLocations

- BandwidthsOffered

Teď jste připravení na vytvoření ExpressRoute okruh.         

### <a name="3-create-an-expressroute-circuit"></a>3. ExpressRoute obvodů vytvořit

Následující příklad ukazuje, jak vytvořit MB / 200 ExpressRoute okruh prostřednictvím Equinix v křemíku sedla. Pokud používáte jiného poskytovatele a různým nastavením, když vyberete jednotlivé žádosti o nahraďte tyto informace.

>[AZURE.IMPORTANT] ExpressRoute okruh se vám nebudou účtovat poplatky od okamžiku vydání klíč služby. Ujistěte se, kdy je připraven zřízení obvod poskytovatel připojení k provedení této operace.


Následující obrázek je příklad žádost o nové klíč služby:

    $Bandwidth = 200
    $CircuitName = "MyTestCircuit"
    $ServiceProvider = "Equinix"
    $Location = "Silicon Valley"

    New-AzureDedicatedCircuit -CircuitName $CircuitName -ServiceProviderName $ServiceProvider -Bandwidth $Bandwidth -Location $Location -sku Standard -BillingType MeteredData

Nebo pokud chcete vytvořit ExpressRoute okruh s doplňkem premium, použijte v následujícím příkladu. Podívejte se na [Nejčastější dotazy týkající se ExpressRoute](expressroute-faqs.md) pro další informace o doplňku premium.

    New-AzureDedicatedCircuit -CircuitName $CircuitName -ServiceProviderName $ServiceProvider -Bandwidth $Bandwidth -Location $Location -sku Premium - BillingType MeteredData


Odpověď bude obsahovat klíč služby. Podrobný popis všech parametrů, můžete získat spuštěním těchto věcí:

    get-help new-azurededicatedcircuit -detailed

### <a name="4-list-all-the-expressroute-circuits"></a>4. ExpressRoute obvody seznam

Můžete spustit `Get-AzureDedicatedCircuit` příkaz zobrazte seznam všech obvody ExpressRoute, které jste vytvořili:


    Get-AzureDedicatedCircuit

Odpověď na bude podobně jako v následujícím příkladu:

    Bandwidth                        : 200
    CircuitName                      : MyTestCircuit
    Location                         : Silicon Valley
    ServiceKey                       : *********************************
    ServiceProviderName              : equinix
    ServiceProviderProvisioningState : NotProvisioned
    Sku                              : Standard
    Status                           : Enabled

Tyto informace kdykoli můžete načíst pomocí `Get-AzureDedicatedCircuit` rutiny. Uskutečnit hovor bez parametrů obsahuje seznam všech obvody. Do pole *klíč ServiceKey* se zobrazí kód služby.

    Get-AzureDedicatedCircuit

    Bandwidth                        : 200
    CircuitName                      : MyTestCircuit
    Location                         : Silicon Valley
    ServiceKey                       : *********************************
    ServiceProviderName              : equinix
    ServiceProviderProvisioningState : NotProvisioned
    Sku                              : Standard
    Status                           : Enabled

Podrobný popis všech parametrů, můžete získat spuštěním těchto věcí:

    get-help get-azurededicatedcircuit -detailed

### <a name="5-send-the-service-key-to-your-connectivity-provider-for-provisioning"></a>5. odešlete klíč služby poskytovatele připojení pro zřízení


*ServiceProviderProvisioningState* obsahuje informace o aktuálním stavu zřizování na straně poskytovatele služeb. *Stav* poskytuje stavu na straně Microsoft. Další informace o okruh zřizování států naleznete v článku [pracovní postupy](expressroute-workflows.md#expressroute-circuit-provisioning-states) .

Při vytváření nové okruh ExpressRoute obvod bude ve stavu následující:

    ServiceProviderProvisioningState : NotProvisioned
    Status                           : Enabled


Obvod budou přesměrovány na následující stát při připojení poskytovatele probíhá povolení za vás:

    ServiceProviderProvisioningState : Provisioning
    Status                           : Enabled

ExpressRoute obvodů musí být v následujícím stavem nebudou moct používat ho:

    ServiceProviderProvisioningState : Provisioned
    Status                           : Enabled


### <a name="6-periodically-check-the-status-and-the-state-of-the-circuit-key"></a>6. pravidelně Kontrola stavu a stav obvodů klávesy

Toto oprávnění umožňuje vědět, kdy poskytovatele povolil vaší okruh. Po konfiguraci obvod *ServiceProviderProvisioningState* se zobrazí jako *Provisioned* , jak je vidět v následujícím příkladu:

    Get-AzureDedicatedCircuit

    Bandwidth                        : 200
    CircuitName                      : MyTestCircuit
    Location                         : Silicon Valley
    ServiceKey                       : *********************************
    ServiceProviderName              : equinix
    ServiceProviderProvisioningState : Provisioned
    Sku                              : Standard
    Status                           : Enabled

### <a name="7-create-your-routing-configuration"></a>7. konfigurace směrování vytvořit

Odkazují [ExpressRoute okruh konfigurace směrování (vytvářet a upravovat okruh peerings)](expressroute-howto-routing-classic.md) článku podrobné pokyny.

>[AZURE.IMPORTANT] Tyto pokyny platí pouze pro obvody, které jsou vytvořené pomocí poskytovatele služeb, které nabízejí služby 2 připojení vrstvy. Pokud používáte poskytovatele služeb, který nabízí spravované layer 3 služby (obvykle IP VPN, jako je MPLS), bude váš poskytovatel připojení konfigurovat a spravovat směrování za vás.

### <a name="8-link-a-virtual-network-to-an-expressroute-circuit"></a>8. propojit virtuální sítě s obvodovou ExpressRoute

Pak propojte virtuální sítě ExpressRoute okruh. Podrobné pokyny v nápovědě k [propojení ExpressRoute obvodů virtuální sítí](expressroute-howto-linkvnet-classic.md) . Pokud je potřeba vytvořit virtuální sítě s využitím klasické nasazení modelu ExpressRoute, postupujte podle pokynů uvedených v tématu [vytvoření virtuální síť pro ExpressRoute](expressroute-howto-vnet-portal-classic.md) .

## <a name="getting-the-status-of-an-expressroute-circuit"></a>Zjišťování stavu obvodu ExpressRoute

Tyto informace kdykoli můžete načíst pomocí `Get-AzureCircuit` rutiny. Uskutečnit hovor bez parametrů obsahuje seznam všech obvody.

    Get-AzureDedicatedCircuit

    Bandwidth                        : 200
    CircuitName                      : MyTestCircuit
    Location                         : Silicon Valley
    ServiceKey                       : *********************************
    ServiceProviderName              : equinix
    ServiceProviderProvisioningState : Provisioned
    Sku                              : Standard
    Status                           : Enabled

    Bandwidth                        : 1000
    CircuitName                      : MyAsiaCircuit
    Location                         : Singapore
    ServiceKey                       : #################################
    ServiceProviderName              : equinix
    ServiceProviderProvisioningState : Provisioned
    Sku                              : Standard
    Status                           : Enabled

Získat informace o konkrétních okruh ExpressRoute předáním klíč služby jako parametr volání:

    Get-AzureDedicatedCircuit -ServiceKey "*********************************"

    Bandwidth                        : 200
    CircuitName                      : MyTestCircuit
    Location                         : Silicon Valley
    ServiceKey                       : *********************************
    ServiceProviderName              : equinix
    ServiceProviderProvisioningState : Provisioned
    Sku                              : Standard
    Status                           : Enabled


Podrobný popis všech parametrů, můžete získat spuštěním těchto věcí:

    get-help get-azurededicatedcircuit -detailed

## <a name="modifying-an-expressroute-circuit"></a>Úprava ExpressRoute okruh

Některé vlastnosti obvodu ExpressRoute můžete změnit bez vlivu na připojení.

Můžete udělat následující s žádné výpadek služeb:

- Povolení nebo zakázání doplněk premium ExpressRoute pro ExpressRoute okruh.
- Zvětšení šířky pásma obvodu ExpressRoute. Všimněte si, že přechodu šířku pásma okruh nepodporuje.
- Změna měření plán z dat podle objemu dat na neomezený Data. Všimněte si, že měření plán z neomezený dat podle objemu dat dat nepodporuje se změna.
- Můžete povolit a zakažte *Povolit klasické operace*.

Podívejte se na [Nejčastější dotazy týkající se ExpressRoute](expressroute-faqs.md) pro další informace o limitech a omezení.

### <a name="to-enable-the-expressroute-premium-add-on"></a>Chcete-li povolit doplněk ExpressRoute premium

Doplněk premium ExpressRoute pro existující okruh můžete povolit pomocí následující rutiny prostředí PowerShell:

    Set-AzureDedicatedCircuitProperties -ServiceKey "*********************************" -Sku Premium

    Bandwidth                        : 1000
    CircuitName                      : TestCircuit
    Location                         : Silicon Valley
    ServiceKey                       : *********************************
    ServiceProviderName              : equinix
    ServiceProviderProvisioningState : Provisioned
    Sku                              : Premium
    Status                           : Enabled

Vaše okruh bude mít povolený ExpressRoute prémiových doplněk funkcí. Všimněte si, že bude začneme hned po úspěšném spuštění příkazu fakturace pro funkci doplňku premium.

### <a name="to-disable-the-expressroute-premium-add-on"></a>Zákaz doplňku ExpressRoute premium

>[AZURE.IMPORTANT] Můžete se nezdaří při používání zdrojů, které jsou větší než co je povolen standardní okruhem.

Poznámka:

- Musíte se ujistit počet virtuálních sítí propojen obvod je menší než 10 před přejít na premium standardní. Pokud si to uděláte, žádosti o aktualizace se nepovede, a budete fakturované sazby premium.

- Je nutné zrušit všechny virtuální sítě v ostatních oblastech geopolitické. Pokud si to uděláte, žádosti o aktualizace se nepovede, a budete fakturované premium sazby.

- Směrování tabulky musí být menší než 4000 směruje pro soukromé prozkoumávání. Pokud velikost tabulky směrování je větší než 4 000 cesty, relace BGP bude uvolněte a nesmí být opětovně povolena dokud počet inzerovaný předpony přechází pod 4 000.

Doplněk premium ExpressRoute pro existující okruh můžete zakázat pomocí následující rutiny prostředí PowerShell:

    Set-AzureDedicatedCircuitProperties -ServiceKey "*********************************" -Sku Standard

    Bandwidth                        : 1000
    CircuitName                      : TestCircuit
    Location                         : Silicon Valley
    ServiceKey                       : *********************************
    ServiceProviderName              : equinix
    ServiceProviderProvisioningState : Provisioned
    Sku                              : Standard
    Status                           : Enabled



### <a name="to-update-the-expressroute-circuit-bandwidth"></a>Chcete-li aktualizovat šířky pásma s obvodovou ExpressRoute

Zaškrtněte políčko [Nejčastější dotazy týkající se ExpressRoute](expressroute-faqs.md) pro možnosti podporované šířky pásma pro vašeho poskytovatele. Můžete vybrat libovolné velikosti, která je větší než velikost existující okruh, dokud fyzické port (který vaší okruh vytvořen) umožňuje.

>[AZURE.IMPORTANT] Nelze zmenšení šířky pásma obvodu ExpressRoute bez přerušení. Snížení šířky pásma bude nutné deprovision ExpressRoute obvodu a potom spravovat (reprovision) nový okruh ExpressRoute.

Až se rozhodnete, jakou velikost budete potřebovat, můžete změnit velikost vaší obvodů tento příkaz:

    Set-AzureDedicatedCircuitProperties -ServiceKey ********************************* -Bandwidth 1000

    Bandwidth                        : 1000
    CircuitName                      : TestCircuit
    Location                         : Silicon Valley
    ServiceKey                       : *********************************
    ServiceProviderName              : equinix
    ServiceProviderProvisioningState : Provisioned
    Sku                              : Standard
    Status                           : Enabled

Vaše okruh bude mít byla velikosti na straně Microsoft. Musíte kontaktovat svého poskytovatele připojení k aktualizaci konfigurace na jejich straně podle této změny. Všimněte si, že bude začneme fakturace můžete pro možnost aktualizované šířky pásma od této chvíle na.

Pokud se zobrazí následující chyba při zvětšení šířky pásma s obvodovou, znamená to, tam dostatečnou šířku pásma na ještě zbývá fyzického portu tam, kde je existující okruh vytvořili. Budete muset odstranit tento obvodů a vytvořit nové obvodů velikost, které potřebujete. 

    Set-AzureDedicatedCircuitProperties : InvalidOperation : Insufficient bandwidth available to perform this circuit
    update operation
    At line:1 char:1
    + Set-AzureDedicatedCircuitProperties -ServiceKey ********************* ...
    + ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
        + CategoryInfo          : CloseError: (:) [Set-AzureDedicatedCircuitProperties], CloudException
        + FullyQualifiedErrorId : Microsoft.WindowsAzure.Commands.ExpressRoute.SetAzureDedicatedCircuitPropertiesCommand
    

## <a name="deprovisioning-and-deleting-an-expressroute-circuit"></a>Zrušení zajišťování a odstranění ExpressRoute obvodů

Poznámka:

- Je nutné zrušit všechny virtuální sítí okruh ExpressRoute pro tuto operaci proběhla úspěšně. Zkontrolujte, jestli máte virtuální sítě, které jsou propojené s obvod, pokud tento nezdaří.

- Když je ExpressRoute okruh služby poskytovatele zřizovací stav **Provisioning** nebo **Provisioned** musí spolupracujete poskytovatele služeb deprovision okruh na jejich straně. Budeme dál rezervovat prostředky a vám účtovat tak, aby poskytovatele služeb dokončí zrušení zajišťování obvodu a nám s upozorněním.

- Pokud poskytovatele služeb má poskytování zrušeno elektrický obvod (zřizovací stavu poskytovatele služby je nastavena na **není zřízení**) můžete odstraňte obvod. Tím se zastaví fakturace za obvod.

Spuštěním následujícího příkazu můžete odstranit ExpressRoute okruh:

    Remove-AzureDedicatedCircuit -ServiceKey "*********************************"



## <a name="next-steps"></a>Další kroky

Po vytvoření vaší okruh Ujistěte se, Uděláte to takhle:

- [Vytváření a změny směrování ExpressRoute okruhem](expressroute-howto-routing-classic.md)
- [Odkaz na ExpressRoute okruh virtuální sítě](expressroute-howto-linkvnet-classic.md)
