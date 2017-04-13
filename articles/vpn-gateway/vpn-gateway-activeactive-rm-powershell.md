<properties
   pageTitle="Konfigurace připojení VPN S2S aktivní službou Azure VPN brány pomocí Správce prostředků Azure a prostředí PowerShell | Microsoft Azure"
   description="Tento článek vás provede konfigurace aktivní připojení s Azure VPN brány pomocí Správce prostředků Azure a Powershellu."
   services="vpn-gateway"
   documentationCenter="na"
   authors="yushwang"
   manager="rossort"
   editor=""
   tags="azure-resource-manager"/>

<tags
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="09/26/2016"
   ms.author="yushwang"/>

# <a name="configure-active-active-s2s-vpn-connections-with-azure-vpn-gateways-using-azure-resource-manager-and-powershell"></a>Konfigurace připojení S2S VPN aktivní s Azure VPN brány pomocí Správce prostředků Azure a prostředí PowerShell

Tento článek vás provede kroky k vytvoření křížově místní aktivní a VNet VNet připojení pomocí Správce prostředků nasazení modelu a Powershellu.


**Modely Azure nasazení**

[AZURE.INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)] 

## <a name="about-highly-available-cross-premises-connections"></a>Informace o vysoce k dispozici více místní připojení

K dosažení dostupnost pro více místní organizaci a VNet VNet připojení, by měl nasazení několika bran VPN a připojení více paralelní mezi sítě a Azure. Základní informace o možnosti připojení a topologie najdete [vysoce k dispozici více místní organizaci a VNet VNet připojení](./vpn-gateway-highlyavailable.md) .

Tento článek obsahuje pokyny k nastavení připojení VPN křížově místní aktivní a aktivní spojení mezi dvěma virtuální sítě:

- [Část 1: vytvoření a konfigurace bránu Azure VPN v režimu aktivní](#aagateway)

- [Část 2: vytvoření připojení mezi místní aktivní](#aacrossprem)

- [Část 3 – připojení VNet VNet aktivní](#aav2v)

- [Část 4: aktualizace stávající brány mezi aktivní a aktivní úsporném režimu](#aaupdate)

Zkombinováním tyto můžete vytvářet složitější a vysoce dostupné síťové topologie, která odpovídá vašim potřebám.

>[AZURE.IMPORTANT] Dejte pozor, aby aktivní režimu jde použít pouze v vysoce SKU


## <a name ="aagateway"></a>Část 1: vytvoření a konfigurace brány VPN aktivní

Podle těchto kroků můžete nakonfigurovat bránu Azure VPN v režimu aktivní. Základní rozdíly mezi bran aktivní a aktivní úsporném režimu:

- Je potřeba vytvořit dvě konfigurace brány se dvěma veřejných IP adres
- Potřebujete nastavit příznak EnableActiveActiveFeature
- Vysoce musí být brána SKU

Další vlastnosti jsou stejné jako aktivní aktivní brány. 

### <a name="before-you-begin"></a>Než začnete

- Zkontrolujte, jestli máte předplatné Azure. Pokud ještě nemáte předplatné Azure, můžete aktivovat [MSDN odběratele výhod](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) neboli přihlašovací si [bezplatný účet](https://azure.microsoft.com/pricing/free-trial/).
    
- Budete muset nainstalovat rutiny prostředí PowerShell Azure správce prostředků. Zjistěte, [Jak nainstalovat a nakonfigurovat Azure PowerShell](../powershell-install-configure.md) Další informace o instalaci rutiny prostředí PowerShell.

### <a name="step-1---create-and-configure-vnet1"></a>Krok 1: vytvoření a konfigurace VNet1

#### <a name="1-declare-your-variables"></a>1. vaše proměnné deklarovat

U tohoto cvičení začneme bude deklarací naše proměnné. Následujícím příkladu dojde deklarování proměnné pomocí hodnoty pro tento cvičení. Ujistěte se, že nahradit hodnoty vlastní při konfiguraci výroby. Pokud používáte kroky vám pomůžou seznámit s tímto typem konfigurace mimo můžete použít tyto proměnné. Úprava proměnných a potom zkopírujte a vložte do konzoly PowerShell.

    $Sub1          = "Ross"
    $RG1           = "TestAARG1"
    $Location1     = "West US"
    $VNetName1     = "TestVNet1"
    $FESubName1    = "FrontEnd"
    $BESubName1    = "Backend"
    $GWSubName1    = "GatewaySubnet"
    $VNetPrefix11  = "10.11.0.0/16"
    $VNetPrefix12  = "10.12.0.0/16"
    $FESubPrefix1  = "10.11.0.0/24"
    $BESubPrefix1  = "10.12.0.0/24"
    $GWSubPrefix1  = "10.12.255.0/27"
    $VNet1ASN      = 65010
    $DNS1          = "8.8.8.8"
    $GWName1       = "VNet1GW"
    $GW1IPName1    = "VNet1GWIP1"
    $GW1IPName2    = "VNet1GWIP2"
    $GW1IPconf1    = "gw1ipconf1"
    $GW1IPconf2    = "gw1ipconf2"
    $Connection12  = "VNet1toVNet2"
    $Connection151 = "VNet1toSite5_1"
    $Connection152 = "VNet1toSite5_2"

#### <a name="2-connect-to-your-subscription-and-create-a-new-resource-group"></a>2. připojit se ke svému předplatnému a vytvoření nové skupiny prostředků

Zkontrolujte, že přejdete do režimu Powershellu pro používání rutin Správce prostředků. Další informace najdete v tématu [Pomocí Windows Powershellu pomocí Správce prostředků](../powershell-azure-resource-manager.md).

Spusťte konzolu prostředí PowerShell a připojit se ke svému účtu. K připojení použijte v následujícím příkladu:

    Login-AzureRmAccount
    Select-AzureRmSubscription -SubscriptionName $Sub1
    New-AzureRmResourceGroup -Name $RG1 -Location $Location1

#### <a name="3-create-testvnet1"></a>3. vytvořit TestVNet1

V příkladu níže je vytvořeno virtuální sítě s názvem TestVNet1 a tři podsítě jeden jen GatewaySubnet, jeden jen FrontEnd a jeden jen back-end. Nahrazení hodnot, je důležité, vždy název vaší podsítě brány konkrétně GatewaySubnet. Pokud zadáte název je něco jiného, vytvoření brány se nezdaří. 

    $fesub1 = New-AzureRmVirtualNetworkSubnetConfig -Name $FESubName1 -AddressPrefix $FESubPrefix1
    $besub1 = New-AzureRmVirtualNetworkSubnetConfig -Name $BESubName1 -AddressPrefix $BESubPrefix1
    $gwsub1 = New-AzureRmVirtualNetworkSubnetConfig -Name $GWSubName1 -AddressPrefix $GWSubPrefix1

    New-AzureRmVirtualNetwork -Name $VNetName1 -ResourceGroupName $RG1 -Location $Location1 -AddressPrefix $VNetPrefix11,$VNetPrefix12 -Subnet $fesub1,$besub1,$gwsub1

### <a name="step-2---create-the-vpn-gateway-for-testvnet1-with-active-active-mode"></a>Krok 2 – Vytvoření brány sítě VPN pro TestVNet1 s režimem aktivní

#### <a name="1-create-the-public-ip-addresses-and-gateway-ip-configurations"></a>1. Vytvořte veřejné brány a IP adresy IP konfigurace

Požádat o dva veřejné adresy IP přiřazen k bráně vytvořené pro vaše VNet. Budete taky definovat podsítí a konfigurace IP povinné. 

    $gw1pip1    = New-AzureRmPublicIpAddress -Name $GW1IPName1 -ResourceGroupName $RG1 -Location $Location1 -AllocationMethod Dynamic
    $gw1pip2    = New-AzureRmPublicIpAddress -Name $GW1IPName2 -ResourceGroupName $RG1 -Location $Location1 -AllocationMethod Dynamic

    $vnet1      = Get-AzureRmVirtualNetwork -Name $VNetName1 -ResourceGroupName $RG1
    $subnet1    = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet1
    $gw1ipconf1 = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GW1IPconf1 -Subnet $subnet1 -PublicIpAddress $gw1pip1
    $gw1ipconf2 = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GW1IPconf2 -Subnet $subnet1 -PublicIpAddress $gw1pip2

#### <a name="2-create-the-vpn-gateway-with-active-active-configuration"></a>2. Vytvořte Brána VPN s konfigurací aktivní

Vytvoření brány virtuální sítě pro TestVNet1. Všimněte si, že jsou dvě položky GatewayIpConfig a nastavit příznak EnableActiveActiveFeature. Aktivní v režimu vyžaduje bránu na základě směrování VPN vysoce SKU. Vytvoření brány může chvíli trvat (nejméně 30 minut dokončete).

    New-AzureRmVirtualNetworkGateway -Name $GWName1 -ResourceGroupName $RG1 -Location $Location1 -IpConfigurations $gw1ipconf1,$gw1ipconf2 -GatewayType Vpn -VpnType RouteBased -GatewaySku HighPerformance -Asn $VNet1ASN -EnableActiveActiveFeature -Debug

#### <a name="3-obtain-the-gateway-public-ip-addresses-and-the-bgp-peer-ip-address"></a>3. získejte brány veřejné IP adresy a adresy IP BGP mezi dvěma účastníky

Po vytvoření brány bude muset získá BGP mezi dvěma účastníky IP adresu na Brána VPN Azure. Tuto adresu je potřeba ke konfiguraci brány VPN Azure jako partner BGP zařízení VPN místní.

    $gw1pip1 = Get-AzureRmPublicIpAddress -Name $GW1IPName1 -ResourceGroupName $RG1
    $gw1pip2 = Get-AzureRmPublicIpAddress -Name $GW1IPName2 -ResourceGroupName $RG1
    $vnet1gw = Get-AzureRmVirtualNetworkGateway -Name $GWName1 -ResourceGroupName $RG1

Umožňuje znázornit dva veřejné IP adresy přidělený VPN brány a jejich odpovídající BGP mezi dvěma účastníky IP adresy pokaždé brány tyto rutiny:

    PS D:\> $gw1pip1.IpAddress
    40.112.190.5

    PS D:\> $gw1pip2.IpAddress
    138.91.156.129

    PS D:\> $vnet1gw.BgpSettingsText
    {
      "Asn": 65010,
      "BgpPeeringAddress": "10.12.255.4,10.12.255.5",
      "PeerWeight": 0
    }

Pořadí veřejnou IP adres pro instancí bran a odpovídající BGP prozkoumávání adresy jsou stejné. V tomto příkladu brány OM s veřejnou IP 40.112.190.5 použije 10.12.255.4 jako jeho adresu BGP prozkoumávání a bráně pro 138.91.156.129 použije 10.12.255.5. Tyto informace: není nutná když nastavíte připojení k bráně aktivní na zařízení VPN místní. Bránu se zobrazují v diagramu pod všechny adresy SMTP:

![aktivní brány](./media/vpn-gateway-activeactive-rm-powershell/active-active-gw.png)

Po vytvoření brány můžete vytvořit více místní aktivní nebo VNet VNet připojení tuto bránu. V následujících částech provede jednotlivými kroky k dokončení výkonu.


## <a name ="aacrossprem"></a>Část 2: vytvoření připojení mezi místní aktivní

Navázat spojení mezi místní, je potřeba vytvořit místní brána sítě představující zařízení místní VPN a připojení Azure VPN brány brána místní síti. V tomto příkladu je brána Azure VPN v režimu aktivní. V důsledku toho i v případě, že existuje pouze jeden místní zařízení virtuální privátní sítě (místní síti brány) a jedno připojení zdroje, obě instancí bran Azure VPN nastaví S2S VPN tunelů s místním zařízením.

Než budete pokračovat, ujistěte se, že jste dokončili [část 1](#aagateway) tohoto cvičení.

### <a name="step-1---create-and-configure-the-local-network-gateway"></a>Krok 1: vytvoření a konfigurace místní síti brány

#### <a name="1-declare-your-variables"></a>1. vaše proměnné deklarovat

Tento cvičení zůstanou sestavit konfigurace v diagramu. Ujistěte se, chcete-li nahradit hodnoty ty, které chcete použít pro konfiguraci.

    $RG5           = "TestAARG5"
    $Location5     = "West US"
    $LNGName51     = "Site5_1"
    $LNGPrefix51   = "10.52.255.253/32"
    $LNGIP51       = "131.107.72.22"
    $LNGASN5       = 65050
    $BGPPeerIP51   = "10.52.255.253"

Několik věcí, které poznámka týkající se parametry brány místní sítě:

- Místní síti brány může být ve stejném nebo jiném umístění a pole Skupina zdroje jako brány sítě VPN. Tento příklad ukazuje je do jiné skupiny prostředků, ale ve stejném umístění Azure.

- Pokud existuje pouze jeden místní VPN zařízení uvedené výše, aktivní připojení můžete pracovat pomocí hesla nebo bez BGP protokol. Tento příklad používá BGP křížově místní připojení.

- Pokud BGP aktivované, předpona, které chcete deklarovat brány místní síti je adresu hostitele BGP mezi dvěma účastníky IP adresy na zařízení s VPN. V tomto případě je /32 předpona "10.52.255.253/32".

- Připomínáme je nutné použít jiný BGP avíza expedice zboží mezi místní sítě a Azure VNet. Pokud jsou stejná, budete muset změnit VNet ASN Pokud zařízení VPN místní už používá ASN rovnocenných počítačů sousední jiných BGP.

#### <a name="2-create-the-local-network-gateway-for-site5"></a>2. Vytvořte bránu místní síti pro Site5
    
Než budete pokračovat, prosím zkontrolujte, jestli že jste pořád připojení k odběru 1. Pokud není ještě nevytvořili, vytvořte skupina zdroje.

    New-AzureRmResourceGroup       -Name $RG5 -Location $Location5
    New-AzureRmLocalNetworkGateway -Name $LNGName51 -ResourceGroupName $RG5 -Location $Location5 -GatewayIpAddress $LNGIP51 -AddressPrefix $LNGPrefix51 -Asn $LNGASN5 -BgpPeeringAddress $BGPPeerIP51

### <a name="step-2---connect-the-vnet-gateway-and-local-network-gateway"></a>Krok 2 – VNet brány a místní síti brány připojení

#### <a name="1-get-the-two-gateways"></a>1. získat dvě brány

    $vnet1gw = Get-AzureRmVirtualNetworkGateway -Name $GWName1  -ResourceGroupName $RG1
    $lng5gw1 = Get-AzureRmLocalNetworkGateway   -Name $LNGName51 -ResourceGroupName $RG5

#### <a name="2-create-the-testvnet1-to-site5-connection"></a>2. Vytvořte TestVNet1 Site5 připojení

V tomto kroku vytvoříte připojení z TestVNet1 k Site5_1 s "EnableBGP" nastavena na $True.

    New-AzureRmVirtualNetworkGatewayConnection -Name $Connection151 -ResourceGroupName $RG1 -VirtualNetworkGateway1 $vnet1gw -LocalNetworkGateway2 $lng5gw1 -Location $Location1 -ConnectionType IPsec -SharedKey 'AzureA1b2C3' -EnableBGP True

#### <a name="3-vpn-and-bgp-parameters-for-your-on-premises-vpn-device"></a>3. parametry VPN a BGP pro zařízení s místním VPN

V příkladu níže jsou uvedeny parametrů, které zadáte do části Konfigurace BGP na zařízení VPN místní tento výkon:

    - Site5 ASN: 65050
    - Site5 BGP IP: 10.52.255.253
    - Předpony pro oznámení: (například) 10.51.0.0/16 a 10.52.0.0/16
    - Azure VNet ASN: 65010
    - Azure VNet BGP IP 1: 10.12.255.4 pro tunelem 40.112.190.5
    - Azure VNet BGP IP 2: 10.12.255.5 pro tunelem 138.91.156.129
    - Statické trasy: určení 10.12.255.4/32, přesměrování tunelem VPN rozhraní 40.112.190.5 cíl 10.12.255.5/32 přesměrování tunelem VPN rozhraní 138.91.156.129
    - eBGP pokus: zajistit, aby možnost "pokus" eBGP je povolena na vašem zařízení, v případě potřeby

Připojení stanovit za několik minut a peering relace BGP spustí se po připojení IPsec. V tomto příkladu, pokud má nakonfigurovanou jejich jedinou zařízení VPN místní výsledkem diagram ukázáno v následujícím příkladu:

![aktivní aktivní crossprem](./media/vpn-gateway-activeactive-rm-powershell/active-active.png)

### <a name="step-3---connect-two-on-premises-vpn-devices-to-the-active-active-vpn-gateway"></a>Krok 3 – připojení dvě zařízení VPN místní brána VPN aktivní

Pokud máte dvě VPN zařízení ve stejné síti místní, můžete dosáhnout duální redundance připojení Azure VPN brány do druhé VPN zařízení.

#### <a name="1-create-the-second-local-network-gateway-for-site5"></a>1. vytvoření druhého brány místní síti pro Site5

Všimněte si, že brány IP adresu, předpony adresy a BGP prozkoumávání adresy druhý brány místní síti nesmí překrývat předchozí místní síti Brána pro stejný místní síti. 

    $LNGName52     = "Site5_2"
    $LNGPrefix52   = "10.52.255.254/32"
    $LNGIP52       = "131.107.72.23"
    $BGPPeerIP52   = "10.52.255.254"

    New-AzureRmLocalNetworkGateway -Name $LNGName52 -ResourceGroupName $RG5 -Location $Location5 -GatewayIpAddress $LNGIP52 -AddressPrefix $LNGPrefix52 -Asn $LNGASN5 -BgpPeeringAddress $BGPPeerIP52
 
#### <a name="2-connect-the-vnet-gateway-and-the-second-local-network-gateway"></a>2. VNet brány a připojení druhého brány místní síti

Vytvoření připojení z TestVNet1 Site5_2 s "EnableBGP" nastavena na $True

    $lng5gw2 = Get-AzureRmLocalNetworkGateway   -Name $LNGName52 -ResourceGroupName $RG5

    New-AzureRmVirtualNetworkGatewayConnection -Name $Connection152 -ResourceGroupName $RG1 -VirtualNetworkGateway1 $vnet1gw -LocalNetworkGateway2 $lng5gw2 -Location $Location1 -ConnectionType IPsec -SharedKey 'AzureA1b2C3' -EnableBGP True

#### <a name="3-vpn-and-bgp-parameters-for-your-second-on-premises-vpn-device"></a>3. VPN a BGP parametrů pro druhý zařízení VPN místní

Podobně pod seznamy parametry budete zadávat do druhé VPN zařízení:

    - Site5 ASN: 65050
    - Site5 BGP IP: 10.52.255.254
    - Předpony pro oznámení: (například) 10.51.0.0/16 a 10.52.0.0/16
    - Azure VNet ASN: 65010
    - Azure VNet BGP IP 1: 10.12.255.4 pro tunelem 40.112.190.5
    - Azure VNet BGP IP 2: 10.12.255.5 pro tunelem 138.91.156.129
    - Statické trasy: určení 10.12.255.4/32, přesměrování tunelem VPN rozhraní 40.112.190.5 cíl 10.12.255.5/32 přesměrování tunelem VPN rozhraní 138.91.156.129
    - eBGP pokus: zajistit, aby možnost "pokus" eBGP je povolena na vašem zařízení, v případě potřeby

Jakmile se vytvořit připojení (tunelů), budete mít duální nadbytečné zařízení VPN a tunelů připojení v místní síti a Azure:

![dvou redundance crossprem](./media/vpn-gateway-activeactive-rm-powershell/dual-redundancy.png)


## <a name ="aav2v"></a>Část 3 – připojení VNet VNet aktivní

V této části vytvoří připojení VNet VNet aktivní s BGP. 

Tyto kroky udělat dál z předchozích kroků výše uvedené. Musíte nejdřív udělat [část 1](#aagateway) vytvořit a nakonfigurovat BGP TestVNet1 a VPN brány. 

### <a name="step-1---create-testvnet2-and-the-vpn-gateway"></a>Krok 1: vytvoření TestVNet2 a Brána VPN

Je důležité, abyste měli jistotu, že adresní prostor IP nové virtuální sítě TestVNet2, nepřekrývá pomocí kterékoli z oblastí VNet.

V tomto příkladu virtuálních sítí patří do stejného předplatného. Můžete nastavit VNet VNet připojení mezi různých předplatných; získáte [Konfigurovat VNet VNet připojení](./vpn-gateway-vnet-vnet-rm-ps.md) se dozvíte víc informací. Ujistěte se, přidáte "-EnableBgp $True" po vytvoření připojení k povolení BGP.

#### <a name="1-declare-your-variables"></a>1. vaše proměnné deklarovat

Ujistěte se, chcete-li nahradit hodnoty ty, které chcete použít pro konfiguraci.

    $RG2           = "TestAARG2"
    $Location2     = "East US"
    $VNetName2     = "TestVNet2"
    $FESubName2    = "FrontEnd"
    $BESubName2    = "Backend"
    $GWSubName2    = "GatewaySubnet"
    $VNetPrefix21  = "10.21.0.0/16"
    $VNetPrefix22  = "10.22.0.0/16"
    $FESubPrefix2  = "10.21.0.0/24"
    $BESubPrefix2  = "10.22.0.0/24"
    $GWSubPrefix2  = "10.22.255.0/27"
    $VNet2ASN      = 65020
    $DNS2          = "8.8.8.8"
    $GWName2       = "VNet2GW"
    $GW2IPName1    = "VNet2GWIP1"
    $GW2IPconf1    = "gw2ipconf1"
    $GW2IPName2    = "VNet2GWIP2"
    $GW2IPconf2    = "gw2ipconf2"
    $Connection21  = "VNet2toVNet1"
    $Connection12  = "VNet1toVNet2"

#### <a name="2-create-testvnet2-in-the-new-resource-group"></a>2. Vytvořte TestVNet2 do nové skupiny prostředků

    New-AzureRmResourceGroup -Name $RG2 -Location $Location2

    $fesub2 = New-AzureRmVirtualNetworkSubnetConfig -Name $FESubName2 -AddressPrefix $FESubPrefix2
    $besub2 = New-AzureRmVirtualNetworkSubnetConfig -Name $BESubName2 -AddressPrefix $BESubPrefix2
    $gwsub2 = New-AzureRmVirtualNetworkSubnetConfig -Name $GWSubName2 -AddressPrefix $GWSubPrefix2

    New-AzureRmVirtualNetwork -Name $VNetName2 -ResourceGroupName $RG2 -Location $Location2 -AddressPrefix $VNetPrefix21,$VNetPrefix22 -Subnet $fesub2,$besub2,$gwsub2

#### <a name="3-create-the-active-active-vpn-gateway-for-testvnet2"></a>3. Vytvořte brány VPN aktivní pro TestVNet2

Požádat o dva veřejné adresy IP přiřazen k bráně vytvořené pro vaše VNet. Budete taky definovat podsítí a konfigurace IP povinné. 

    $gw2pip1    = New-AzureRmPublicIpAddress -Name $GW2IPName1 -ResourceGroupName $RG2 -Location $Location2 -AllocationMethod Dynamic
    $gw2pip2    = New-AzureRmPublicIpAddress -Name $GW2IPName2 -ResourceGroupName $RG2 -Location $Location2 -AllocationMethod Dynamic

    $vnet2      = Get-AzureRmVirtualNetwork -Name $VNetName2 -ResourceGroupName $RG2
    $subnet2    = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet2
    $gw2ipconf1 = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GW2IPconf1 -Subnet $subnet2 -PublicIpAddress $gw2pip1
    $gw2ipconf2 = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GW2IPconf2 -Subnet $subnet2 -PublicIpAddress $gw2pip2

Vytvoření brány VPN s číslem AS a příznak "EnableActiveActiveFeature". Všimněte si, že musíte přepsat výchozí ASN na Azure VPN brány. Avíza expedice zboží pro připojení VNets musejí být různé povolení směrování přenosu a BGP.

    New-AzureRmVirtualNetworkGateway -Name $GWName2 -ResourceGroupName $RG2 -Location $Location2 -IpConfigurations $gw2ipconf1,$gw2ipconf2 -GatewayType Vpn -VpnType RouteBased -GatewaySku HighPerformance -Asn $VNet2ASN -EnableActiveActiveFeature

### <a name="step-2---connect-the-testvnet1-and-testvnet2-gateways"></a>Krok 2 – připojení TestVNet1 a TestVNet2 brány

V tomto příkladu jsou obě brány v rámci stejného předplatného. Tento krok může udělat ve stejné relaci Powershellu.

#### <a name="1-get-both-gateways"></a>1. obě brány získat

Ujistěte se, přihlaste se a připojovat se k odběru 1.

    $vnet1gw = Get-AzureRmVirtualNetworkGateway -Name $GWName1 -ResourceGroupName $RG1
    $vnet2gw = Get-AzureRmVirtualNetworkGateway -Name $GWName2 -ResourceGroupName $RG2
    
#### <a name="2-create-both-connections"></a>2. obě připojení vytvořit

V tomto kroku vytvoříte spojení z TestVNet1 TestVNet2 a připojení z TestVNet2 k TestVNet1.

    New-AzureRmVirtualNetworkGatewayConnection -Name $Connection12 -ResourceGroupName $RG1 -VirtualNetworkGateway1 $vnet1gw -VirtualNetworkGateway2 $vnet2gw -Location $Location1 -ConnectionType Vnet2Vnet -SharedKey 'AzureA1b2C3' -EnableBgp $True

    New-AzureRmVirtualNetworkGatewayConnection -Name $Connection21 -ResourceGroupName $RG2 -VirtualNetworkGateway1 $vnet2gw -VirtualNetworkGateway2 $vnet1gw -Location $Location2 -ConnectionType Vnet2Vnet -SharedKey 'AzureA1b2C3' -EnableBgp $True

>[AZURE.IMPORTANT] Nezapomeňte povolit BGP pro obě připojení.

Po dokončení těchto kroků připojení vytvoří v několik minut a BGP prozkoumávání relace bude po dokončení VNet VNet připojení se dvěma redundance:

![aktivní aktivní v2v](./media/vpn-gateway-activeactive-rm-powershell/vnet-to-vnet.png)

## <a name ="aaupdate"></a>Část 4: aktualizace stávající brány mezi aktivní a aktivní úsporném režimu

Části poslední bude popisují, jak nakonfigurovat existující brána Azure VPN ze záložní aktivní do režimu aktivní nebo naopak.

>[AZURE.IMPORTANT] Dejte pozor, aby aktivní režimu jde použít pouze v vysoce SKU

### <a name="configure-an-active-standby-gateway-to-active-active-gateway"></a>Konfigurace brány aktivní záložní k bráně pro aktivní

#### <a name="1-gateway-parameters"></a>1. parametry brány

Následující příklad převede brány aktivní záložní brány aktivní. Je potřeba vytvořit jiné veřejnou IP adresu a potom přidejte druhý konfiguraci brány. Níže jsou uvedeny parametry použít:

    $GWName     = "TestVNetAA1GW"
    $VNetName   = "TestVNetAA1"
    $RG         = "TestVPNActiveActive01"
    $GWIPName2  = "gwpip2"
    $GWIPconf2  = "gw1ipconf2"

    $vnet       = Get-AzureRmVirtualNetwork -Name $VNetName -ResourceGroupName $RG
    $subnet     = Get-AzureRmVirtualNetworkSubnetConfig -Name 'GatewaySubnet' -VirtualNetwork $vnet
    $gw         = Get-AzureRmVirtualNetworkGateway -Name $GWName -ResourceGroupName $RG
    $location   = $gw.Location

#### <a name="2-create-the-public-ip-address-then-add-the-second-gateway-ip-configuration"></a>2. vytvořit veřejnou IP adresu a potom přidejte druhý IP konfiguraci brány

    $gwpip2     = New-AzureRmPublicIpAddress -Name $GWIPName2 -ResourceGroupName $RG -Location $location -AllocationMethod Dynamic
    Add-AzureRmVirtualNetworkGatewayIpConfig -VirtualNetworkGateway $gw -Name $GWIPconf2 -Subnet $subnet -PublicIpAddress $gwpip2 

#### <a name="3-enable-active-active-mode-and-update-the-gateway"></a>3. povolit režim aktivní a aktualizujte bránu

Objekt brány musíte nastavit v prostředí PowerShell aktivovat skutečné aktualizace. SKU objekt brány musí být také změněno k vysoce, protože byla dříve vytvořena jako standardní.

    Set-AzureRmVirtualNetworkGateway -VirtualNetworkGateway $gw -EnableActiveActiveFeature -GatewaySku HighPerformance

Tato aktualizace může trvat až 30 a 45 minut.

### <a name="configure-an-active-active-gateway-to-active-standby-gateway"></a>Konfigurace brány aktivní k bráně pro aktivní úsporném režimu

#### <a name="1-gateway-parameters"></a>1. parametry brány

Použít stejné parametry jako výše uvedené, zjištění názvu konfigurace IP, který chcete odebrat.

    $GWName     = "TestVNetAA1GW"
    $RG         = "TestVPNActiveActive01"

    $gw         = Get-AzureRmVirtualNetworkGateway -Name $GWName -ResourceGroupName $RG
    $ipconfname = $gw.IpConfigurations[1].Name

#### <a name="2-remove-the-gateway-ip-configuration-and-disable-the-active-active-mode"></a>2. odebrat IP konfiguraci brány a zakázání režimu aktivní

Podobně musíte nastavit objekt brány v prostředí PowerShell aktivovat skutečné aktualizace.

    Remove-AzureRmVirtualNetworkGatewayIpConfig -Name $ipconfname -VirtualNetworkGateway $gw
    Set-AzureRmVirtualNetworkGateway -VirtualNetworkGateway $gw -DisableActiveActiveFeature

Tato aktualizace může trvat až 30 45 minut.


## <a name="next-steps"></a>Další kroky

Po dokončení připojení lze virtuálních počítačích vašich virtuální sítí. Postup v tématu [Vytvoření virtuálního počítače](../virtual-machines/virtual-machines-windows-hero-tutorial.md) .

