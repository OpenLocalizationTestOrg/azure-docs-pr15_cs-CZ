<properties 
   pageTitle="Konfigurace vynuceného tunneling pro připojení k webu pomocí Správce prostředků nasazení modelu | Microsoft Azure"
   description="Jak přesměrovat nebo "Vynutit všechny přenosy Internet vazbou zpátky do místního pracoviště."
   services="vpn-gateway"
   documentationCenter="na"
   authors="cherylmc"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager"/>
<tags 
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="08/10/2016"
   ms.author="cherylmc" />

# <a name="configure-forced-tunneling-using-the-azure-resource-manager-deployment-model"></a>Konfigurace vynuceného tunneling pomocí nasazení modelu správce prostředků Azure

> [AZURE.SELECTOR]
- [Prostředí PowerShell – klasické](vpn-gateway-about-forced-tunneling.md)
- [Prostředí PowerShell – správce](vpn-gateway-forced-tunneling-rm.md)

Vynuceného tunneling umožňuje přesměrovat nebo "platnost" všechny přenosy vazbou na Internetu zpět do místního pracoviště přes VPN k webu tunelem pro kontrolu a auditování. Toto je důležité zabezpečení požadavku na většině podnikové zásady.

Bez vynuceného tunneling vazbou internetových adres VMs v Azure vždy prochází z Azure síťovou infrastrukturu přímo se k Internetu, bez možnost umožňuje zkontrolovat nebo auditování přenos. Neoprávněnému přístupu Internet může vést k informacím a další typy porušení zabezpečení

Tento článek vás provede konfigurace vynucená tunneling virtuálních sítí vytvořené pomocí modelu nasazení Správce prostředků.

**Modely Azure nasazení**

[AZURE.INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)] 

**Modely nasazení a nástroje pro vynuceného tunneling**

Vynuceného tunelového připojení je možné konfigurovat pro klasické nasazení modelu a nasazení modelu správce prostředků. V tématu Další informace v následující tabulce. Jakmile nové články, nové modely nasazení a další nástroje dostupné pro tuto konfiguraci aktualizujeme v této tabulce. Při článek neexistuje, jsme propojit přímo ji z tabulky.

[AZURE.INCLUDE [vpn-gateway-table-forced-tunneling](../../includes/vpn-gateway-table-forcedtunnel-include.md)] 


## <a name="about-forced-tunneling"></a>Blíží vynucená tunneling


Následující obrázek znázorňuje, jak vynuceného tunelování funguje. 

![Vynucená Tunneling](./media/vpn-gateway-forced-tunneling-rm/forced-tunnel.png)

Ve výše uvedeném příkladu Frontend není vynucená podsítě vytvořena. Úloh v podsítě Frontend můžete dál přijmout a odpovědět na požadavků zákazníků z Internetu přímo. Vynucená polovině osy a back-end podsítí prostřednictvím těchto propojení. Odchozí připojení k Internetu tyto dva podsítě budou vynucené nebo přesměrovat zpátky do místního webu pomocí některého tunelů S2S VPN.

Díky zakázání a zkontrolovat přístup k Internetu z virtuálních počítačích a cloudových služeb v Azure, můžete nadále povolit architektuře vícevrstvého služby povinné. Můžete taky použít vynuceného tunneling celý virtuální sítím, pokud v síti virtuální jsou bez internetového úloh.

## <a name="requirements-and-considerations"></a>Požadavky a co byste měli zvážit

Vynuceného tunneling v Azure nakonfigurovaný prostřednictvím virtuální sítě uživatelem definované trasy. Přesměrovat komunikaci k místnímu webu vyjádřený trasu výchozí na brána Azure VPN. Další informace o směrování definované uživatelem a virtuální sítě najdete v článku [Směrování a předávání IP definované uživatelem](../virtual-network/virtual-networks-udr-overview.md).

- Každý virtuální podsítě přiřazenou tabulkou směrování předdefinované systému. Systém směrování tabulka obsahuje následující tři skupiny postupů:

    - **Místní VNet přesměrovává:** Přímo do cíle VMs ve stejné síti virtuální
    
    - **Místní směruje:** K bráně Azure VPN
    
    - **Výchozí směrování:** Přímo na Internetu. Dojde ke ztrátě pakety určené soukromé IP adresy nevztahuje předchozí dvě trasy.

-  Tento postup směruje definované uživatelem (UDR) slouží k vytváření směrování tabulky přidat výchozí směrování a připojte ji tabulce směrování do VNet subnet(s) povolit vynuceného tunneling na tyto podsítě.

- Vynuceného tunneling musí být přidružené VNet obsahující VPN brány na základě směrování. Budete muset nastavit "výchozí web" mezi místních webů křížově místní připojení k virtuální sítě.

- ExpressRoute vynucená tunneling není nakonfigurován prostřednictvím tento mechanismus, ale místo toho je povolené reklamní výchozí směrování prostřednictvím ExpressRoute BGP peering relace. Naleznete v [Dokumentaci ExpressRoute](https://azure.microsoft.com/documentation/services/expressroute/) pro další informace.

## <a name="configuration-overview"></a>Přehled konfigurace

Následující postup vám pomůže vytvořit skupinu zdrojů a VNet. Potom vytvoříte Brána VPN a konfigurace vynuceného tunneling. V tomto postupu virtuální sítě "MultiTier VNet" obsahuje 3 podsítí: *Frontend* *Midtier*a *back-end*s 4 křížově místní připojení: *DefaultSiteHQ*a 3 *poboček*.

Kroky postupu *DefaultSiteHQ* nastavit jako výchozí web připojení vynucená tunneling a konfigurace Midtier a back-end podsítí používat vynucená tunneling.

    
## <a name="before-you-begin"></a>Než začnete

Zkontrolujte, jestli máte následující položky před zahájením konfiguraci.

- Předplatné Azure. Pokud ještě nemáte předplatné Azure, můžete aktivovat [MSDN odběratele výhod](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) neboli přihlašovací si [bezplatný účet](https://azure.microsoft.com/pricing/free-trial/).

- Musíte nainstalovat nejnovější verzi rutiny prostředí PowerShell správce prostředků Azure (1.0 nebo novější). Zjistěte, [Jak nainstalovat a nakonfigurovat Azure PowerShell](../powershell-install-configure.md) Další informace o instalaci rutiny prostředí PowerShell.


## <a name="configure-forced-tunneling"></a>Konfigurace vynuceného tunneling

1. V konzole prostředí PowerShell přihlaste ke svému účtu Azure. Tato rutina výzvu pro přihlašovací údaje pro váš účet Azure. Po přihlášení se stáhne nastavení účtu tak, aby byly k dispozici pro Azure PowerShell.

        Login-AzureRmAccount 

2. Dostaňte seznam předplatné Azure.

        Get-AzureRmSubscription

2. Určení předplatné, ke které chcete použít. 

        Select-AzureRmSubscription -SubscriptionName "Replace_with_your_subscription_name"
        
3. Vytvoření skupiny prostředků.

        New-AzureRmResourceGroup -Name "ForcedTunneling" -Location "North Europe"

4. Vytvořit virtuální sítě a určení podsítí. 

        $s1 = New-AzureRmVirtualNetworkSubnetConfig -Name "Frontend" -AddressPrefix "10.1.0.0/24"
        $s2 = New-AzureRmVirtualNetworkSubnetConfig -Name "Midtier" -AddressPrefix "10.1.1.0/24"
        $s3 = New-AzureRmVirtualNetworkSubnetConfig -Name "Backend" -AddressPrefix "10.1.2.0/24"
        $s4 = New-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -AddressPrefix "10.1.200.0/28"
        $vnet = New-AzureRmVirtualNetwork -Name "MultiTier-VNet" -Location "North Europe" -ResourceGroupName "ForcedTunneling" -AddressPrefix "10.1.0.0/16" -Subnet $s1,$s2,$s3,$s4

5. Vytvoření brány místní síti.

        $lng1 = New-AzureRmLocalNetworkGateway -Name "DefaultSiteHQ" -ResourceGroupName "ForcedTunneling" -Location "North Europe" -GatewayIpAddress "111.111.111.111" -AddressPrefix "192.168.1.0/24"
        $lng2 = New-AzureRmLocalNetworkGateway -Name "Branch1" -ResourceGroupName "ForcedTunneling" -Location "North Europe" -GatewayIpAddress "111.111.111.112" -AddressPrefix "192.168.2.0/24"
        $lng3 = New-AzureRmLocalNetworkGateway -Name "Branch2" -ResourceGroupName "ForcedTunneling" -Location "North Europe" -GatewayIpAddress "111.111.111.113" -AddressPrefix "192.168.3.0/24"
        $lng4 = New-AzureRmLocalNetworkGateway -Name "Branch3" -ResourceGroupName "ForcedTunneling" -Location "North Europe" -GatewayIpAddress "111.111.111.114" -AddressPrefix "192.168.4.0/24"
        
6. Vytvoření tabulky postupu a pravidla směrování.

        New-AzureRmRouteTable –Name "MyRouteTable" -ResourceGroupName "ForcedTunneling" –Location "North Europe"
        $rt = Get-AzureRmRouteTable –Name "MyRouteTable" -ResourceGroupName "ForcedTunneling" 
        Add-AzureRmRouteConfig -Name "DefaultRoute" -AddressPrefix "0.0.0.0/0" -NextHopType VirtualNetworkGateway -RouteTable $rt
        Set-AzureRmRouteTable -RouteTable $rt


7. Přidružení tabulce směrování Midtier a back-end podsítí.

        $vnet = Get-AzureRmVirtualNetwork -Name "MultiTier-Vnet" -ResourceGroupName "ForcedTunneling"
        Set-AzureRmVirtualNetworkSubnetConfig -Name "MidTier" -VirtualNetwork $vnet -AddressPrefix "10.1.1.0/24" -RouteTable $rt
        Set-AzureRmVirtualNetworkSubnetConfig -Name "Backend" -VirtualNetwork $vnet -AddressPrefix "10.1.2.0/24" -RouteTable $rt
        Set-AzureRmVirtualNetwork -VirtualNetwork $vnet

8. Vytvoření brány pomocí výchozího webu. Tento krok trvá některé dobu, někdy 45 minut nebo víc, protože jsou vytváření a konfigurace brány.<br> `-GatewayDefaultSite` Je rutina parametr, který umožňuje vynuceného konfigurace směrování do práce, takže starat ke konfiguraci nastavení správně. Tento parametr je dostupná v prostředí PowerShell 1.0 nebo novější.

        $pip = New-AzureRmPublicIpAddress -Name "GatewayIP" -ResourceGroupName "ForcedTunneling" -Location "North Europe" -AllocationMethod Dynamic
        $gwsubnet = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet
        $ipconfig = New-AzureRmVirtualNetworkGatewayIpConfig -Name "gwIpConfig" -SubnetId $gwsubnet.Id -PublicIpAddressId $pip.Id
        New-AzureRmVirtualNetworkGateway -Name "Gateway1" -ResourceGroupName "ForcedTunneling" -Location "North Europe" -IpConfigurations $ipconfig -GatewayType Vpn -VpnType RouteBased -GatewayDefaultSite $lng1 -EnableBgp $false

9. Vytvoření připojení VPN k webu.

        $gateway = Get-AzureRmVirtualNetworkGateway -Name "Gateway1" -ResourceGroupName "ForcedTunneling"
        $lng1 = Get-AzureRmLocalNetworkGateway -Name "DefaultSiteHQ" -ResourceGroupName "ForcedTunneling" 
        $lng2 = Get-AzureRmLocalNetworkGateway -Name "Branch1" -ResourceGroupName "ForcedTunneling" 
        $lng3 = Get-AzureRmLocalNetworkGateway -Name "Branch2" -ResourceGroupName "ForcedTunneling" 
        $lng4 = Get-AzureRmLocalNetworkGateway -Name "Branch3" -ResourceGroupName "ForcedTunneling" 

        New-AzureRmVirtualNetworkGatewayConnection -Name "Connection1" -ResourceGroupName "ForcedTunneling" -Location "North Europe" -VirtualNetworkGateway1 $gateway -LocalNetworkGateway2 $lng1 -ConnectionType IPsec -SharedKey "preSharedKey"
        New-AzureRmVirtualNetworkGatewayConnection -Name "Connection2" -ResourceGroupName "ForcedTunneling" -Location "North Europe" -VirtualNetworkGateway1 $gateway -LocalNetworkGateway2 $lng2 -ConnectionType IPsec -SharedKey "preSharedKey"
        New-AzureRmVirtualNetworkGatewayConnection -Name "Connection3" -ResourceGroupName "ForcedTunneling" -Location "North Europe" -VirtualNetworkGateway1 $gateway -LocalNetworkGateway2 $lng3 -ConnectionType IPsec -SharedKey "preSharedKey"
        New-AzureRmVirtualNetworkGatewayConnection -Name "Connection4" -ResourceGroupName "ForcedTunneling" -Location "North Europe" -VirtualNetworkGateway1 $gateway -LocalNetworkGateway2 $lng4 -ConnectionType IPsec -SharedKey "preSharedKey"

        Get-AzureRmVirtualNetworkGatewayConnection -Name "Connection1" -ResourceGroupName "ForcedTunneling"
        



