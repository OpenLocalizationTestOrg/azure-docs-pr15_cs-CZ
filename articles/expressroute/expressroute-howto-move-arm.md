<properties
   pageTitle="Přesouvat ExpressRoute obvody klasické správci zdrojů | Microsoft Azure"
   description="Tato stránka popisuje, jak přesunout klasické okruh nasazení modelu správce prostředků."
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


# <a name="move-expressroute-circuits-from-the-classic-to-the-resource-manager-deployment-model"></a>Přesunutí ExpressRoute obvody z klasickou nasazení modelu správce prostředků

## <a name="configuration-prerequisites"></a>Konfigurace požadavky

- Potřebujete nejnovější verzi modulu Azure PowerShell (minimální verze 1.0).
- Ujistěte se, že zkontrolování [požadavky](expressroute-prerequisites.md), [požadavky směrování](expressroute-routing.md)a [pracovní postupy](expressroute-workflows.md) před zahájením konfigurace.
- Před před další, zkontrolujte informace, které je k dispozici v části [Přesunutí ExpressRoute okruh z klasické správci zdrojů](expressroute-move.md). Ujistěte se, že máte plně rozumí limity a omezení toho, co je možné.
- Pokud chcete přesunout Azure ExpressRoute okruhem z modelu klasické nasazení modelu nasazení Správce prostředků Azure, musíte mít okruhem plně nakonfigurované a provozní v modelu klasické nasazení.
- Ujistěte se, že máte skupina prostředků, která byla vytvořená v modelu nasazení Správce prostředků.

## <a name="move-the-expressroute-circuit-to-the-resource-manager-deployment-model"></a>Přesunutí okruhem ExpressRoute nasazení modelu správce prostředků

ExpressRoute okruh je potřeba přesunout do modelu nasazení Správce prostředků tak, aby jej lze použít v celém klasickou a nasazení modely správce prostředků. Lze to provést spuštěním následující příkazy Powershellu.

### <a name="step-1-gather-circuit-details-from-the-classic-deployment-model"></a>Krok 1: Shromáždění okruh podrobnosti z modelu klasické nasazení

Je třeba nejprve získat informace o ExpressRoute okruh.

Přihlaste se k Azure klasické prostředí a shromáždění klíč služby. Následující úryvek prostředí PowerShell umožňuje shromáždění informací:

    # Sign in to your Azure account
    Add-AzureAccount

    # Select the appropriate Azure subscription
    Select-AzureSubscription "<Enter Subscription Name here>"

    # Import the PowerShell modules for Azure and ExpressRoute
    Import-Module 'C:\Program Files (x86)\Microsoft SDKs\Azure\PowerShell\ServiceManagement\Azure\Azure.psd1'
    Import-Module 'C:\Program Files (x86)\Microsoft SDKs\Azure\PowerShell\ServiceManagement\Azure\ExpressRoute\ExpressRoute.psd1'

    # Get the service keys of all your ExpressRoute circuits
    Get-AzureDedicatedCircuit

Zkopírujte **klíč služby** obvodu, který chcete přesunout do modelu nasazení Správce prostředků.

### <a name="step-2-sign-in-to-the-resource-manager-environment-and-create-a-new-resource-group"></a>Krok 2: Přihlaste se k prostředí správce prostředků a vytvoření nové skupiny prostředků

Vytvoření nové skupiny prostředků pomocí následující fragment kódu:

    # Sign in to your Azure Resource Manager environment
    Login-AzureRmAccount

    # Select the appropriate Azure subscription
    Get-AzureRmSubscription -SubscriptionName "<Enter Subscription Name here>" | Select-AzureRmSubscription

    #Create a new resource group if you don't already have one
    New-AzureRmResourceGroup -Name "DemoRG" -Location "West US"

Pokud ještě nemáte můžete existující skupiny zdrojů.

### <a name="step-3-move-the-expressroute-circuit-to-the-resource-manager-deployment-model"></a>Krok 3: Přesunutí okruh ExpressRoute nasazení modelu správce prostředků

Teď jste připraveni přesunout ExpressRoute okruh z klasickou do modelu nasazení Správce prostředků. Zkontrolujte informace podle [Přesunutí ExpressRoute okruh od klasického nasazení modelu správce prostředků](expressroute-move.md) před pokračováním.

Spuštěním následující úryvek můžete udělat toto:

    Move-AzureRmExpressRouteCircuit -Name "MyCircuit" -ResourceGroupName "DemoRG" -Location "West US" -ServiceKey "<Service-key>"

>[AZURE.NOTE] Po dokončení přesunutí nový název, který je uveden v předchozí rutina se použijí k adresu zdroje. Obvod bude v podstatě přejmenovat.

## <a name="enable-an-expressroute-circuit-for-both-deployment-models"></a>Povolení okruh ExpressRoute pro oba modely nasazení

ExpressRoute okruh potřeba přesunout do modelu nasazení Správce prostředků před řízení přístupu k nasazení modelu.

Spusťte následující rutinu povolení přístupu k oběma modely nasazení:

    # Get details of the ExpressRoute circuit
    $ckt = Get-AzureRmExpressRouteCircuit -Name "DemoCkt" -ResourceGroupName "DemoRG"

    #Set "Allow Classic Operations" to TRUE
    $ckt.AllowClassicOperations = $true

    # Update circuit
    Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt

Po této operaci je úspěšně dokončený, budete moct zobrazit obvod v modelu klasické nasazení.

Spusťte následující podrobnosti obvodu ExpressRoute získáte:

    get-azurededicatedcircuit

Musíte mít zobrazíte klíč služby uvedené. Teď můžete spravovat odkazy na používání vaší standardní klasické nasazení modelu pro klasické VNets a váš standardní ARM příkazy pro ARM VNETs okruh ExpressRoute. V následujících článcích vás provede jednotlivými Správa odkazů na okruh ExpressRoute:

- [Propojení virtuální sítě ExpressRoute okruh v modelu nasazení Správce prostředků](expressroute-howto-linkvnet-arm.md)
- [Propojení virtuální sítě ExpressRoute okruh v modelu klasické nasazení](expressroute-howto-linkvnet-classic.md)


## <a name="disable-the-expressroute-circuit-to-the-classic-deployment-model"></a>Zakázání okruh ExpressRoute klasické nasazení modelu

Spusťte následující rutinu zakázání přístupu k modelu klasické nasazení:

    # Get details of the ExpressRoute circuit
    $ckt = Get-AzureRmExpressRouteCircuit -Name "DemoCkt" -ResourceGroupName "DemoRG"

    #Set "Allow Classic Operations" to FALSE
    $ckt.AllowClassicOperations = $false

    # Update circuit
    Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt

Po této operaci je úspěšně dokončený, nebudete moct dívat obvod v modelu klasické nasazení.

## <a name="next-steps"></a>Další kroky

Po vytvoření vaší okruh Ujistěte se, Uděláte to takhle:

- [Vytváření a změny směrování ExpressRoute okruhem](expressroute-howto-routing-arm.md)
- [Odkaz na ExpressRoute okruh virtuální sítě](expressroute-howto-linkvnet-arm.md)
