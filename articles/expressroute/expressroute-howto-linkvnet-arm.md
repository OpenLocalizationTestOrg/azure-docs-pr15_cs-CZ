<properties 
   pageTitle="Propojit virtuální sítě ExpressRoute okruh pomocí prostředí PowerShell | Microsoft Azure"
   description="Tento dokument obsahuje přehled o tom, jak propojit virtuálních sítí (VNets) do ExpressRoute obvody pomocí Správce prostředků nasazení modelu a Powershellu."
   services="expressroute"
   documentationCenter="na"
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
   ms.author="ganesr" />

# <a name="link-a-virtual-network-to-an-expressroute-circuit"></a>Odkaz na ExpressRoute okruh virtuální sítě

> [AZURE.SELECTOR]
- [Azure portálu - správce prostředků](expressroute-howto-linkvnet-portal-resource-manager.md)
- [Prostředí PowerShell – správce](expressroute-howto-linkvnet-arm.md)
- [Prostředí PowerShell – klasické](expressroute-howto-linkvnet-classic.md)


Tento článek vám pomůže propojit virtuálních sítí (VNets) Azure ExpressRoute obvody pomocí Správce prostředků nasazení modelu a Powershellu. Virtuální sítě můžou být ve stejném předplatném nebo část jiné předplatné.

**Modely Azure nasazení**

[AZURE.INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)] 

## <a name="configuration-prerequisites"></a>Konfigurace požadavky

- Potřebujete nejnovější verzi modulu Azure PowerShell (minimální verze 1.0). Zjistěte, [Jak nainstalovat a nakonfigurovat Azure PowerShell](../powershell-install-configure.md) Další informace o instalaci rutiny prostředí PowerShell.
- Potřebujete zkontrolovat [požadavky](expressroute-prerequisites.md), [požadavky na směrování](expressroute-routing.md)a [pracovní postupy](expressroute-workflows.md) před zahájením konfigurace.
- Máte aktivní okruh ExpressRoute. 
    - Postupujte podle pokynů a [vytvořte ExpressRoute okruh](expressroute-howto-circuit-arm.md) a mít okruh povolit tak, že váš poskytovatel připojení. 
    - Zajištění Azure soukromé prozkoumávání nakonfigurovaná pro vaše okruh. V tématu [Konfigurace směrování](expressroute-howto-routing-arm.md) pokyny v článku směrování. 
    - Zajistěte, že Azure soukromé prozkoumávání nakonfigurovaný a BGP prozkoumávání mezi sítí a Microsoft nahoru, čímž povolíte připojení začátku do konce.
    - Ujistěte se, že máte virtuální sítě a Brána virtuální sítě vytvořené a plně zřízení. Postupujte podle pokynů k vytvoření [Brána VPN](../articles/vpn-gateway/vpn-gateway-create-site-to-site-rm-powershell.md), ale je nutné použít `-GatewayType ExpressRoute`.

Až 10 virtuálních sítí můžete propojit standardní okruh ExpressRoute. Všechny virtuální sítě musí být ve stejné oblasti geopolitické při použití standardní okruh ExpressRoute. 

Propojení virtuální sítí mimo oblasti geopolitické obvodu ExpressRoute nebo k ExpressRoute okruhem připojit většího počtu virtuální sítě, pokud jste povolili doplněk premium ExpressRoute. Zaškrtněte políčko [Nejčastější dotazy týkající se](expressroute-faqs.md) podrobné informace o doplňku premium.

## <a name="connect-a-virtual-network-in-the-same-subscription-to-a-circuit"></a>Připojení k okruh virtuální sítě v rámci stejného předplatného

Virtuální sítě brány tak, aby ExpressRoute okruhem můžete se připojit pomocí následující rutiny. Ujistěte se, že brány virtuální sítě se vytvoří a je připraven k propojení před spuštěním rutiny:

    $circuit = Get-AzureRmExpressRouteCircuit -Name "MyCircuit" -ResourceGroupName "MyRG"
    $gw = Get-AzureRmVirtualNetworkGateway -Name "ExpressRouteGw" -ResourceGroupName "MyRG"
    $connection = New-AzureRmVirtualNetworkGatewayConnection -Name "ERConnection" -ResourceGroupName "MyRG" -Location "East US" -VirtualNetworkGateway1 $gw -PeerId $circuit.Id -ConnectionType ExpressRoute

## <a name="connect-a-virtual-network-in-a-different-subscription-to-a-circuit"></a>Připojení k okruh virtuální sítě v jiné předplatné

ExpressRoute okruhem můžete sdílet ve víc předplatných. Následující obrázek znázorňuje jednoduchou schéma způsobu sdílení prací ExpressRoute obvody přes víc předplatných.

Každou menší mraků ve velké cloudu slouží k představují předplatných, která patří různá oddělení v rámci organizace. Svoje předplatné všech oddělení v rámci organizace můžete použít pro nasazení služeb – ale můžete sdílet jednoho okruh ExpressRoute připojení zpátky do vaší místní síti. Jednoho oddělení (v tomto příkladu: IT) můžete vlastní okruh ExpressRoute. Další předplatná v rámci organizace můžete použít okruh ExpressRoute.

>[AZURE.NOTE] Připojení a šířku pásma poplatky vyhrazené okruhem použije se vlastníka okruh ExpressRoute. Všechny virtuální sítě sdílet stejnou šířku pásma.

![Připojení mezi předplatného](./media/expressroute-howto-linkvnet-classic/cross-subscription.png)

### <a name="administration"></a>Správa

*Vlastník okruh* je uživatel oprávněnými power ExpressRoute okruh zdroje. Vlastník okruh můžete vytvořit povolení, které lze uplatnit *okruh*uživatelů. *Okruh uživatelů* jsou vlastníky virtuální sítě bran (, které nejsou ve stejném předplatném jako okruh ExpressRoute). *Okruh uživatelů* můžete uplatnit povolení (jeden se tak mohli ověřovat za virtuální sítě).

*Vlastník okruh* má snadno změnit nebo odvolat povolení kdykoli. Odvolání se tak mohli ověřovat výsledkem spojení se odstraní z předplatného, jehož přístup byl odvolán.

### <a name="circuit-owner-operations"></a>Operace s obvodovou vlastník 

#### <a name="creating-an-authorization"></a>Vytvoření ověření
    
Vlastník okruh vytvoří povolení. Výsledkem vystavením se tak mohli ověřovat klíč, který lze uživatelem okruh připojení jejich virtuální sítě bran obvodu ExpressRoute. Povolení je platný pouze jeden připojení.

Následující rutinu úryvek ukazuje, jak vytvořit povolení:

    $circuit = Get-AzureRmExpressRouteCircuit -Name "MyCircuit" -ResourceGroupName "MyRG"
    Add-AzureRmExpressRouteCircuitAuthorization -ExpressRouteCircuit $circuit -Name "MyAuthorization1"
    Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $circuit

        $circuit = Get-AzureRmExpressRouteCircuit -Name "MyCircuit" -ResourceGroupName "MyRG"
    $auth1 = Get-AzureRmExpressRouteCircuitAuthorization -ExpressRouteCircuit $circuit -Name "MyAuthorization1"
        

Odpověď na to bude obsahovat klávesu se tak mohli ověřovat spolu s stavu:

    Name                   : MyAuthorization1
    Id                     : /subscriptions/&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&/resourceGroups/ERCrossSubTestRG/providers/Microsoft.Network/expressRouteCircuits/CrossSubTest/authorizations/MyAuthorization1
    Etag                   : &&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&& 
    AuthorizationKey       : ####################################
    AuthorizationUseStatus : Available
    ProvisioningState      : Succeeded

        

#### <a name="reviewing-authorizations"></a>Kontrola povolení

Vlastník okruh můžete zkontrolovat všechny povolení vydané na určitou s obvodovou spuštěné následující rutinu:

    $circuit = Get-AzureRmExpressRouteCircuit -Name "MyCircuit" -ResourceGroupName "MyRG"
    $authorizations = Get-AzureRmExpressRouteCircuitAuthorization -ExpressRouteCircuit $circuit
    

#### <a name="adding-authorizations"></a>Přidání povolení

Vlastník okruh můžete přidat povolení pomocí následující rutinu:

    $circuit = Get-AzureRmExpressRouteCircuit -Name "MyCircuit" -ResourceGroupName "MyRG"
    Add-AzureRmExpressRouteCircuitAuthorization -ExpressRouteCircuit $circuit -Name "MyAuthorization2"
    Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $circuit
    
    $circuit = Get-AzureRmExpressRouteCircuit -Name "MyCircuit" -ResourceGroupName "MyRG"
    $authorizations = Get-AzureRmExpressRouteCircuitAuthorization -ExpressRouteCircuit $circuit

    
#### <a name="deleting-authorizations"></a>Odstranění povolení

Vlastník okruh můžete odebrat nebo odstranění povolení uživateli spusťte následující rutinu:

    Remove-AzureRmExpressRouteCircuitAuthorization -Name "MyAuthorization2" -ExpressRouteCircuit $circuit
    Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $circuit    

### <a name="circuit-user-operations"></a>Operace okruh uživatelů

Uživatel s obvodovou musí ID partnera a kód Product key pro povolení od majitele okruh. Klávesy se tak mohli ověřovat je GUID.

ID partnera je, může být vrácený se z následujícího příkazu.

    Get-AzureRmExpressRouteCircuit -Name "MyCircuit" -ResourceGroupName "MyRG"

#### <a name="redeeming-connection-authorizations"></a>Uplatnění povolení připojení

Okruh uživatelů můžete spustit následující rutinu pro uplatnění povolení odkaz:

    $id = "/subscriptions/********************************/resourceGroups/ERCrossSubTestRG/providers/Microsoft.Network/expressRouteCircuits/MyCircuit"  
    $gw = Get-AzureRmVirtualNetworkGateway -Name "ExpressRouteGw" -ResourceGroupName "MyRG"
    $connection = New-AzureRmVirtualNetworkGatewayConnection -Name "ERConnection" -ResourceGroupName "RemoteResourceGroup" -Location "East US" -VirtualNetworkGateway1 $gw -PeerId $id -ConnectionType ExpressRoute -AuthorizationKey "^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^"

#### <a name="releasing-connection-authorizations"></a>Uvolnění povolení připojení

Uvolněte povolení odstraněním připojení propojující okruh ExpressRoute virtuální sítě.

## <a name="next-steps"></a>Další kroky

Další informace o ExpressRoute najdete v tématu [Nejčastější dotazy týkající se ExpressRoute](expressroute-faqs.md).
