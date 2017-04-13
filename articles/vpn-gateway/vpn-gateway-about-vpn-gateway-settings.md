<properties 
   pageTitle="O nastavení brány virtuální sítě VPN brány | Microsoft Azure"
   description="Informace o nastavení brány VPN Azure virtuální sítě."
   services="vpn-gateway"
   documentationCenter="na"
   authors="cherylmc"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager,azure-service-management"/>
<tags 
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/18/2016"
   ms.author="cherylmc" />

# <a name="about-vpn-gateway-settings"></a>Informace o nastavení brány VPN

Řešení připojení VPN brány závisí na konfigurace více zdrojů mohli poslat síťová komunikace mezi virtuálních sítí a místního umístění. Jednotlivé zdroje obsahuje konfigurovat nastavení. Kombinace zdrojů a nastavení určuje výsledek připojení.

V částech v tomto článku jsou uvedeny zdrojů a nastavení, které se vztahují k VPN bránu v modelu nasazení **Správce prostředků** . Se může to nějak výrazně pomohlo zobrazit dostupné konfigurace pomocí připojení topologie diagramů. Popisy a diagramy topologie najdete pro každé připojení řešení v článku [O Brána VPN](vpn-gateway-about-vpngateways.md) . 

## <a name="gwtype"></a>Typy brány

Každý virtuální sítě může obsahovat pouze jeden virtuální sítě brány každého typu. Při vytváření virtuálních síťové brány, musíte správnost typu brána pro konfiguraci.

K dispozici pro - GatewayType jsou k dispozici: 

- Virtuální privátní sítě
- ExpressRoute

Vyžaduje Brána VPN `-GatewayType` *Vpn*.  

Příklad:

    New-AzureRmVirtualNetworkGateway -Name vnetgw1 -ResourceGroupName testrg `
    -Location 'West US' -IpConfigurations $gwipconfig -GatewayType Vpn `
    -VpnType RouteBased
 

## <a name="gwsku"></a>Skladové jednotky brány


[AZURE.INCLUDE [vpn-gateway-gwsku-include](../../includes/vpn-gateway-gwsku-include.md)]

### <a name="configuring-the-gateway-sku"></a>Konfigurace brány SKU

**Určení brány SKU na portálu Azure**

Pokud použijete portálu Azure k vytvoření brány virtuální sítě Správce prostředků, můžete vybrat brány SKU pomocí rozevíracího seznamu. Možnosti, které se zobrazí odpovídají typu brána a typ VPN, kterou jste vybrali.

Například pokud vyberete typ brány "VPN" a VPN typu "zásady založené na", uvidíte jenom "Základní" SKU protože to je jediný SKU umožňující PolicyBased VPN. Pokud vyberete "Směrování na základě", můžete vybírat z Basic, standardních a vysoce skladové jednotky. 


**Určení brány SKU pomocí prostředí PowerShell**


Následující příklad prostředí PowerShell Určuje `-GatewaySku` jako *Standardní*.

    New-AzureRmVirtualNetworkGateway -Name vnetgw1 -ResourceGroupName testrg `
    -Location 'West US' -IpConfigurations $gwipconfig -GatewaySku Standard `
    -GatewayType Vpn -VpnType RouteBased

**Změna brány SKU**

Pokud chcete upgradovat brány SKU výkonnější skladové jednotky (z Basic/standardní, vysoce) můžete `Resize-AzureRmVirtualNetworkGateway` rutiny prostředí PowerShell. Můžete brány SKU velikost pomocí tuto rutinu downgradovat.

Následující příklad prostředí PowerShell zobrazuje brány změnu velikosti na vysoce skladové jednotky.

    $gw = Get-AzureRmVirtualNetworkGateway -Name vnetgw1 -ResourceGroupName testrg
    Resize-AzureRmVirtualNetworkGateway -VirtualNetworkGateway $gw -GatewaySku HighPerformance

### <a name="estimated-aggregate-throughput-by-gateway-sku-and-type"></a>Pole Předpokládaná agregační výkonu brána SKU a typ

Následující tabulka zobrazuje typy brány a odhadovaná agregační výkon. V této tabulce platí pro správce zdrojů a modelů klasické nasazení.

[AZURE.INCLUDE [vpn-gateway-table-gwtype-aggthroughput](../../includes/vpn-gateway-table-gwtype-aggtput-include.md)] 


## <a name="connectiontype"></a>Typy připojení

Správce prostředků nasazení modelu každý konfigurace vyžaduje konkrétní virtuální brány typ připojení. K dispozici Powershellu správce prostředků hodnoty `-ConnectionType` jsou:

- IPsec
- Vnet2Vnet
- ExpressRoute
- VPNClient

V následujícím příkladu prostředí PowerShell vytvoříme S2S připojení, které vyžaduje typ připojení *IPsec*.

    New-AzureRmVirtualNetworkGatewayConnection -Name localtovon -ResourceGroupName testrg `
    -Location 'West US' -VirtualNetworkGateway1 $gateway1 -LocalNetworkGateway2 $local `
    -ConnectionType IPsec -RoutingWeight 10 -SharedKey 'abc123'


## <a name="vpntype"></a>Typy VPN

Když vytvoříte brány virtuální sítě pro konfiguraci brány VPN, je třeba určit typ VPN. Typ VPN, které zvolíte, závisí na topologii připojení, který chcete vytvořit. Například P2S připojení vyžaduje typu RouteBased VPN. Typ VPN mohou také závisí na hardware, který budete používat. Konfigurace S2S vyžadují VPN zařízení. Některá VPN zařízení podporují pouze určitý typ VPN.

Typ VPN, který jste vybrali musí splňovat všechny připojení požadavky na řešení, že které chcete vytvořit. Například pokud chcete vytvořit připojení S2S VPN brány a připojení k síti VPN P2S brány pro stejný virtuální sítě, by použijete VPN typ *RouteBased* protože P2S vyžaduje typu RouteBased VPN. Potřebujete by také můžete ověřit, že zařízení VPN podporuje připojení RouteBased VPN. 

Po vytvoření brány virtuální sítě VPN typ nelze změnit. Budete muset odstranit Brána virtuální sítě a vytvořte nový účet. Existují dva typy VPN:

[AZURE.INCLUDE [vpn-gateway-vpntype](../../includes/vpn-gateway-vpntype-include.md)]


Následující příklad prostředí PowerShell Určuje `-VpnType` jako *RouteBased*. Při vytváření brány, musíte správnost - VpnType konfiguraci. 

    New-AzureRmVirtualNetworkGateway -Name vnetgw1 -ResourceGroupName testrg `
    -Location 'West US' -IpConfigurations $gwipconfig `
    -GatewayType Vpn -VpnType RouteBased

##  <a name="requirements"></a>Požadavky na brány

[AZURE.INCLUDE [vpn-gateway-table-requirements](../../includes/vpn-gateway-table-requirements-include.md)] 


## <a name="gwsub"></a>Podsítě brány

Konfigurace brány virtuální sítě, musíte nejdřív pro vaše VNet vytvořit podsítě brány. Podsítě brány musí mít název *GatewaySubnet* správně fungovat. Tento název umožňuje Azure vědět, že tento podsítě bude použito brány.

Minimální velikost vaší podsítě brány zcela závisí na konfiguraci, které chcete vytvořit. Přestože je možné vytvořit brány podsítě nejnižší /29, doporučujeme, abyste vytvoření brány podsítě /28 nebo větší (/ 28, /27, /26, atd.). 

Vytvoření brány větší neumožňuje spuštěn s omezení velikosti brány. Například jste vytvořili Brána virtuální sítě s velikostí podsítě brány /29 pro připojení k S2S. Je nyní chcete konfigurovat S2S/ExpressRoute být nainstalovány konfigurace. Tuto konfiguraci vyžaduje minimální velikost podsítě brány /28. Pokud chcete vytvořit konfiguraci, je třeba změnit podsítě brány tak, aby zahrnoval minimální požadavky pro připojení, což je /28.

Následující příklad správce prostředků Powershellu zobrazuje brány podsítě s názvem GatewaySubnet. Vidíte, že zápisu CIDR určuje /27, který umožňuje dost IP adres pro většinu konfigurace, které aktuálně existuje.

    Add-AzureRmVirtualNetworkSubnetConfig -Name 'GatewaySubnet' -AddressPrefix 10.0.3.0/27

[AZURE.INCLUDE [vpn-gateway-no-nsg](../../includes/vpn-gateway-no-nsg-include.md)] 


## <a name="lng"></a>Místní síti brány

Při vytváření brány configuration VPN, brány místní síti často představuje místního pracoviště. Klasický nasazení modelu byla brány místní síti označovány jako místní síti. 

Název místní síti brány, veřejnou IP adresu zařízení místní virtuální privátní sítě a zadejte adresu předpony balíčků umístěných na místního pracoviště. Azure sleduje předpony cílovou adresu pro v síti, požádá konfiguraci, která jste zadali místní síti bránu pro a směrování paketů příslušným způsobem. Můžete také určit místní síti brány pro VNet VNet konfigurace, které používají připojení VPN brány.

Následující příklad prostředí PowerShell vytvoří novou bránu místní sítě:

    New-AzureRmLocalNetworkGateway -Name LocalSite -ResourceGroupName testrg `
    -Location 'West US' -GatewayIpAddress '23.99.221.164' -AddressPrefix '10.5.51.0/24'

Někdy potřebujete změnit nastavení brány místní síti. Třeba při přidání nebo změna rozsahu adres, nebo pokud se změní IP adresu zařízení virtuální privátní sítě. Pro klasické VNet můžete změnit nastavení na portálu klasické na stránce místní sítě. Pro správce prostředků najdete v článku [nastavení brány místní síti změnit pomocí prostředí PowerShell](vpn-gateway-modify-local-network-gateway.md).

## <a name="resources"></a>Rutiny pro rozhraní REST API a prostředí PowerShell

Další technické prostředky a požadavky na zvláštní syntaxi při používání rozhraní REST API a rutinách Powershellu pro konfigurace brány VPN najdete v tématu na následujících stránkách:

|**Klasický** | **Správce prostředků**|
|-----|----|
|[Prostředí PowerShell](https://msdn.microsoft.com/library/mt270335.aspx)|[Prostředí PowerShell](https://msdn.microsoft.com/library/mt163510.aspx)|
|[ROZHRANÍ REST API](https://msdn.microsoft.com/library/jj154113.aspx)|[ROZHRANÍ REST API](https://msdn.microsoft.com/library/mt163859.aspx)|


## <a name="next-steps"></a>Další kroky

Další informace o konfiguraci k dispozici připojení najdete v článku [O Brána VPN](vpn-gateway-about-vpngateways.md) . 







 
