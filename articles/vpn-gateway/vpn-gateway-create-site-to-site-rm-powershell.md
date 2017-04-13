<properties
   pageTitle="Vytvořit virtuální sítě s připojení VPN k webu pomocí Správce prostředků Azure a prostředí PowerShell | Microsoft Azure"
   description="Tento článek vás provede vytváření VNet pomocí Správce prostředků nasazení modelu a připojení k síti místní místního pomocí připojení S2S VPN brány."
   services="vpn-gateway"
   documentationCenter="na"
   authors="cherylmc"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager"/>

<tags
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/14/2016"
   ms.author="cherylmc"/>

# <a name="create-a-vnet-with-a-site-to-site-connection-using-powershell"></a>Vytvoření VNet s připojení k webu pomocí prostředí PowerShell

> [AZURE.SELECTOR]
- [Správce prostředků - Azure portálu](vpn-gateway-howto-site-to-site-resource-manager-portal.md)
- [Správce prostředků - prostředí PowerShell](vpn-gateway-create-site-to-site-rm-powershell.md)
- [Klasický – klasické portálu](vpn-gateway-site-to-site-create.md)

Tento článek vás provede vytváření virtuálních sítí a připojení VPN k webu brány k sítí v místním nasazení modelu správce prostředků Azure. Připojení k webu se dá použít pro více místní a hybridní konfigurace.

![Diagram webu webů] (./media/vpn-gateway-create-site-to-site-rm-powershell/s2srmps.png "na webu") 


### <a name="deployment-models-and-methods-for-site-to-site-connections"></a>Modely nasazení a metody pro připojení k webu

[AZURE.INCLUDE [deployment models](../../includes/vpn-gateway-deployment-models-include.md)] 

Následující tabulka zobrazuje modelů momentálně neexistuje nasazení a metody konfigurace webů serveru. Při článek s kroky konfigurace je k dispozici, jsme propojit přímo ji z této tabulky. 

[AZURE.INCLUDE [site-to-site table](../../includes/vpn-gateway-table-site-to-site-include.md)]

#### <a name="additional-configurations"></a>Další konfigurace

Pokud se chcete připojit VNets dohromady, ale nejsou vytvoření připojení k místního pracoviště, přečtěte si téma [Konfigurace VNet VNet připojení](vpn-gateway-vnet-vnet-rm-ps.md). Pokud chcete přidat připojení k webu VNet kterém už je připojení, najdete v článku [Přidání S2S připojení k VNet pomocí existujícího připojení VPN brány](vpn-gateway-howto-multi-site-to-site-resource-manager-portal.md).

## <a name="before-you-begin"></a>Než začnete

Zkontrolujte, jestli máte následující položky před zahájením konfigurace.

- Kompatibilní zařízení VPN a někoho, kdo je možné ji nakonfigurovat. Přečtěte si [téma VPN zařízení](vpn-gateway-about-vpn-devices.md). Pokud nejste obeznámení s konfigurací zařízení virtuální privátní sítě nebo neznají IP adresu oblastech umístěny v konfiguraci sítě vaší místní, budete muset koordinaci s někým, kdo může poskytnout tyto údaje za vás.

- Externě vystaveného veřejnou IP adresa pro zařízení s VPN. Tato IP adresa nebyla nalezena za službou NAT.
    
- Předplatné Azure. Pokud ještě nemáte předplatné Azure, můžete aktivovat [MSDN odběratele výhod](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) neboli přihlašovací si [bezplatný účet](https://azure.microsoft.com/pricing/free-trial/).
    
- Nejnovější verze rutiny prostředí PowerShell Azure správce prostředků. Zjistěte, [Jak nainstalovat a nakonfigurovat Azure PowerShell](../powershell-install-configure.md) Další informace o instalaci rutiny prostředí PowerShell.


## <a name="Login"></a>1. připojit se ke svému předplatnému 

Zkontrolujte, že přejdete do režimu Powershellu pro používání rutin Správce prostředků. Další informace najdete v tématu [Pomocí Windows Powershellu pomocí Správce prostředků](../powershell-azure-resource-manager.md).

Spusťte konzolu prostředí PowerShell a připojit se ke svému účtu. K připojení použijte v následujícím příkladu:

    Login-AzureRmAccount

Zaškrtněte políčko předplatná pro účet.

    Get-AzureRmSubscription 

Určení předplatné, ke které chcete použít.

    Select-AzureRmSubscription -SubscriptionName "Replace_with_your_subscription_name"

## <a name="VNet"></a>2. Vytvořte virtuální sítě a podsítě brány

V příkladech se používá bránu podsítí /28. Když je možné vytvořit brány podsítě nejnižší /29, doporučujeme vytvoření větší podsítě obsahující víc adres tak, že vyberete aspoň /28 nebo /27. Toto nastavení umožňuje dost adresy tak, aby pokryly možné konfigurace další požadované může v budoucnosti.

Pokud už máte virtuální sítě s podsítí brány, který je/29 nebo větší, můžete můžete přejít na [Přidat brány místní síti](#localnet).


[AZURE.INCLUDE [vpn-gateway-no-nsg](../../includes/vpn-gateway-no-nsg-include.md)]  

### <a name="to-create-a-virtual-network-and-a-gateway-subnet"></a>Jak vytvořit virtuální sítě a podsítě brány

Umožňuje vytvořit virtuální sítě a podsítě brány v následujícím příkladu. Nahraďte hodnoty pro vlastní. 

Nejprve vytvořte skupina zdroje:
    
    New-AzureRmResourceGroup -Name testrg -Location 'West US'

Dále vytvořte virtuální sítě. Ověřte, že mezery adresu zadanou nepřekrývaly některou z adresy mezery, ke kterým máte ve vaší místní síti.

Následující příklad vytvoří virtuální sítě s názvem *testvnet* a dvě podsítí, jen jeden *GatewaySubnet* a druhý s názvem *Podsíť1*. Je třeba vytvořit jednu podsítě s názvem konkrétně *GatewaySubnet*. Pokud zadáte název je něco jiného, se nepovede konfigurace připojení. 

Nastavení proměnných.

    $subnet1 = New-AzureRmVirtualNetworkSubnetConfig -Name 'GatewaySubnet' -AddressPrefix 10.0.0.0/28
    $subnet2 = New-AzureRmVirtualNetworkSubnetConfig -Name 'Subnet1' -AddressPrefix '10.0.1.0/28'

Vytvořte VNet.

    New-AzureRmVirtualNetwork -Name testvnet -ResourceGroupName testrg `
    -Location 'West US' -AddressPrefix 10.0.0.0/16 -Subnet $subnet1, $subnet2

### <a name="gatewaysubnet"></a>Chcete-li virtuální sítě, které jste už vytvořili přidat podsítě brány

Tento krok je vyžadován jenom v případě, že budete muset přidat brány podsítě VNet, který jste vytvořili.

Vytvoření brány podsítě pomocí v následujícím příkladu. Ujistěte se, že název brány podsítě "GatewaySubnet". Pokud zadáte název je něco jiného, vytvoření podsítě, ale Azure nebude takovým s ním zacházet jako podsítě brány.

Nastavení proměnných.

    $vnet = Get-AzureRmVirtualNetwork -ResourceGroupName testrg -Name testvnet

Vytvoření podsítě brány.

    Add-AzureRmVirtualNetworkSubnetConfig -Name 'GatewaySubnet' -AddressPrefix 10.0.3.0/28 -VirtualNetwork $vnet

Nastavení. 

    Set-AzureRmVirtualNetwork -VirtualNetwork $vnet

## 3. <a name="localnet"> </a>přidat místní síti brány

V síti virtuální brány místní síti obvykle odkazuje místního pracoviště. Na web zadejte název prodloužením doby Azure můžete odkazují na ni a také zadat předponu místo adresy brány místní síti. 

Azure používá předponu IP adresy, kterou zadáte určit které přenos odešlete místního pracoviště. To znamená, že budete muset nastavit každý předponu adresy, která chcete přidružit brány místní síti. Tyto předpony můžete snadno aktualizovat, pokud se změní v místní síti. 

Pokud chcete použít příklady Powershellu, mějte na paměti:
    
- *GatewayIPAddress* je IP adresa místní VPN zařízení. Zařízení s VPN nebyla nalezena za službou NAT. 
- *AddressPrefix* je místní adresní prostor.

Chcete-li přidat brány místní síti s předponou jedné adresy:

    New-AzureRmLocalNetworkGateway -Name LocalSite -ResourceGroupName testrg `
    -Location 'West US' -GatewayIpAddress '23.99.221.164' -AddressPrefix '10.5.51.0/24'

Chcete-li přidat brána místní síti s více předpony adres:

    New-AzureRmLocalNetworkGateway -Name LocalSite -ResourceGroupName testrg `
    -Location 'West US' -GatewayIpAddress '23.99.221.164' -AddressPrefix @('10.0.0.0/24','20.0.0.0/24')

### <a name="to-modify-ip-address-prefixes-for-your-local-network-gateway"></a>Chcete-li změnit předpony IP adres pro místní síti brány

Někdy se změnit předpon brány místní sítě. Kroky, kterými můžete upravit předpony adres IP, závisí na tom, jestli jste vytvořili připojení VPN brány. Naleznete v části [Upravit IP adresu předpony brány místní síti pro](#modify) tohoto článku.


## <a name="PublicIP"></a>4. požadavek veřejnou IP adresa Brána VPN

Pak požádat o veřejné IP adresu, který se má přidělit bránu Azure VNet VPN. Toto není stejný IP adresy, kterou je přiřazen k zařízení VPN; raději přidělený brána Azure VPN samotné. Nelze zadat IP adresy, kterou chcete použít. Je to dynamicky přidělit brány. Používáte tuto IP adresu při konfiguraci zařízení VPN místní připojení k bráně.

Brána Azure VPN pro nasazení modelu správce prostředků aktuálně podporuje pouze veřejné adresy IP pomocí dynamického přidělení metody. Však neznamená, že se změní na IP adresu. Pouze Azure VPN brány IP adresu změny při odstranění a opětovné vytvoření brány. Brána veřejnou IP adresu nezmění přes Změna velikosti, obnovení nebo jiné interní údržbu/inovace Azure VPN brány.

Použijte v následujícím příkladu Powershellu:

    $gwpip= New-AzureRmPublicIpAddress -Name gwpip -ResourceGroupName testrg -Location 'West US' -AllocationMethod Dynamic

## <a name="GatewayIPConfig"></a>5. vytvořit IP adresách konfiguraci brány

Konfigurace brány definuje podsítě a veřejnou IP adresu používat. Umožňuje vytvořit konfiguraci brány v následujícím příkladu.

    $vnet = Get-AzureRmVirtualNetwork -Name testvnet -ResourceGroupName testrg
    $subnet = Get-AzureRmVirtualNetworkSubnetConfig -Name 'GatewaySubnet' -VirtualNetwork $vnet
    $gwipconfig = New-AzureRmVirtualNetworkGatewayIpConfig -Name gwipconfig1 -SubnetId $subnet.Id -PublicIpAddressId $gwpip.Id 

## <a name="CreateGateway"></a>6. Vytvořte bránu virtuální sítě

V tomto kroku vytvoříte brány virtuální sítě. Vytvoření brány může trvat dlouho k dokončení. Často 45 minut zabere nejméně. 

Použijte tyto hodnoty:

- *-GatewayType* Konfigurace webů webu je *virtuální privátní sítě*. Typ brány je vždy specifické pro konfiguraci, která jsou implementace. Například ostatní brány konfigurace může vyžadovat - GatewayType ExpressRoute. 

- *-VpnType* může být *RouteBased* (jako takzvaný dynamické bránu v některých si přečtěte následující dokumentaci) nebo *PolicyBased* (jako takzvaný statické bránu v některých si přečtěte následující dokumentaci). Další informace o typech brány VPN najdete v článku [O bran VPN](vpn-gateway-about-vpngateways.md#vpntype).
- *-GatewaySku* může být *základní* *Standardní*nebo *vysoce*.   

        New-AzureRmVirtualNetworkGateway -Name vnetgw1 -ResourceGroupName testrg `
        -Location 'West US' -IpConfigurations $gwipconfig -GatewayType Vpn `
        -VpnType RouteBased -GatewaySku Standard

## <a name="ConfigureVPNDevice"></a>7. zařízení VPN konfigurace

V tomto okamžiku musíte veřejnou IP adresu brány virtuální sítě pro konfiguraci místního VPN zařízení. Práce s výrobce zařízení si pro informace o konfiguraci závislé. Můžete odkázat na [Počítače v síti VPN](vpn-gateway-about-vpn-devices.md) Další informace.

Při hledání veřejnou IP adresu brány virtuální sítě, použijte v následujícím příkladu:

    Get-AzureRmPublicIpAddress -Name gwpip -ResourceGroupName testrg

## <a name="CreateConnection"></a>8. připojení k síti VPN vytvořit

Dále vytvořte připojení VPN k webu mezi brány virtuální sítě a zařízení VPN. Ujistěte se, že nahradit hodnoty vlastní. Sdílený klíč se musí shodovat hodnotu, kterou jste použili pro konfiguraci VPN zařízení. Všimněte si, že `-ConnectionType` pro na web je *IPsec*. 

Nastavení proměnných.

    $gateway1 = Get-AzureRmVirtualNetworkGateway -Name vnetgw1 -ResourceGroupName testrg
    $local = Get-AzureRmLocalNetworkGateway -Name LocalSite -ResourceGroupName testrg

Vytvoření připojení.

    New-AzureRmVirtualNetworkGatewayConnection -Name localtovon -ResourceGroupName testrg `
    -Location 'West US' -VirtualNetworkGateway1 $gateway1 -LocalNetworkGateway2 $local `
    -ConnectionType IPsec -RoutingWeight 10 -SharedKey 'abc123'

Po chvíli při připojení zřídí. 

## <a name="toverify"></a>Chcete-li ověřit připojení VPN

Ověřte připojení k síti VPN několika různými způsoby.

[AZURE.INCLUDE [vpn-gateway-verify-connection-rm](../../includes/vpn-gateway-verify-connection-rm-include.md)]

## <a name="modify"></a>Chcete-li změnit předpony IP adres pro místní síti brány

Pokud potřebujete změnit předpony pro místní síti brány, postupujte podle následujících pokynů. Dvě sady pokyny jsou k dispozici. Pokyny, které zvolíte, závisí na tom, jestli jste už vytvořili připojení k bráně. 

[AZURE.INCLUDE [vpn-gateway-modify-ip-prefix-rm](../../includes/vpn-gateway-modify-ip-prefix-rm-include.md)]

## <a name="modifygwipaddress"></a>Chcete-li změnit brány IP adresa brány místní síti

[AZURE.INCLUDE [vpn-gateway-modify-lng-gateway-ip-rm](../../includes/vpn-gateway-modify-lng-gateway-ip-rm-include.md)]

## <a name="next-steps"></a>Další kroky

- Přidání virtuálních počítačích virtuální sítí. Postup v tématu [Vytvoření virtuálního počítače](../virtual-machines/virtual-machines-windows-hero-tutorial.md) .

- Informace o BGP najdete v tématu [Přehled BGP](vpn-gateway-bgp-overview.md) a [Postup při konfiguraci BGP](vpn-gateway-bgp-resource-manager-ps.md).

