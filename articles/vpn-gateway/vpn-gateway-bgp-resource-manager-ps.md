<properties
   pageTitle="Postup při konfiguraci BGP na Azure VPN brány pomocí Správce prostředků Azure a prostředí PowerShell | Microsoft Azure"
   description="Tento článek vás provede konfigurace BGP s Azure VPN brány pomocí Správce prostředků Azure a Powershellu."
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
   ms.date="04/15/2016"
   ms.author="yushwang"/>

# <a name="how-to-configure-bgp-on-azure-vpn-gateways-using-azure-resource-manager-and-powershell"></a>Postup při konfiguraci BGP na Azure VPN brány pomocí Správce prostředků Azure a prostředí PowerShell

Tento článek vás provede kroky povolíte BGP u připojení VPN webu (S2S) mezi místní a VNet VNet připojení s protokolem správce prostředků nasazení modelu a Powershellu.


**Modely Azure nasazení**

[AZURE.INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)] 

## <a name="about-bgp"></a>Informace o BGP

BGP je standardní směrování protokol běžně používaných v Internetu na exchange informace o směrování a dostupnosti mezi dvěma nebo víc sítí. BGP umožňuje bran VPN Azure a místních VPN zařízení, označovaný taky jako BGP kolegové nebo sousední na exchange "přesměrovává", které informuje obě brány na dostupnost a dostupnosti pro tyto předpony projít bran nebo směrovači souvisejících. BGP můžete taky povolit dopravní směrování mezi více sítí šíření trasy, které brány BGP učí z jedné mezi dvěma účastníky BGP jiných BGP partnery.

Další informace o výhodách BGP a porozumět technické požadavky a aspektech používání BGP najdete v článku [Přehled BGP s Azure VPN brány](./vpn-gateway-bgp-overview.md) .

## <a name="getting-started-with-bgp-on-azure-vpn-gateways"></a>Začínáme s BGP Azure VPN brány

Tento článek vás provede jednotlivými kroky dělat tyto věci:

- [Část 1 - povolit BGP na bránu Azure VPN](#enablebgp)

- [Část 2 – navázat spojení mezi místní s BGP](#crossprembgp)

- [Část 3 – připojení VNet VNet s BGP](#v2vbgp)

Každou část pokynů tvoří základní stavební blok pro povolení BGP v připojení k síti. Pokud dokončit všechny tři části vytvoříte topologii, jak je znázorněno na následujícím obrázku:

![Topologie BGP](./media/vpn-gateway-bgp-resource-manager-ps/bgp-crosspremv2v.png)

Zkombinováním tyto můžete vytvořit složitější, více víře, dopravní síť, které vyhovují vašim potřebám.

## <a name ="enablebgp"></a>Část 1: Konfigurace BGP brány Azure sítě VPN

Následující kroky konfigurace bude nastavení parametrů BGP brány Azure VPN, jak je znázorněno na následujícím obrázku:

![BGP brány](./media/vpn-gateway-bgp-resource-manager-ps/bgp-gateway.png)

### <a name="before-you-begin"></a>Než začnete

- Zkontrolujte, jestli máte předplatné Azure. Pokud ještě nemáte předplatné Azure, můžete aktivovat [MSDN odběratele výhod](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) neboli přihlašovací si [bezplatný účet](https://azure.microsoft.com/pricing/free-trial/).
    
- Budete muset nainstalovat rutiny prostředí PowerShell Azure správce prostředků. Zjistěte, [Jak nainstalovat a nakonfigurovat Azure PowerShell](../powershell-install-configure.md) Další informace o instalaci rutiny prostředí PowerShell.

### <a name="step-1---create-and-configure-vnet1"></a>Krok 1: vytvoření a konfigurace VNet1 

#### <a name="1-declare-your-variables"></a>1. vaše proměnné deklarovat

U tohoto cvičení začneme bude deklarací naše proměnné. Následujícím příkladu dojde deklarování proměnné pomocí hodnoty pro tento cvičení. Ujistěte se, že nahradit hodnoty vlastní při konfiguraci výroby. Pokud používáte kroky vám pomůžou seznámit s tímto typem konfigurace mimo můžete použít tyto proměnné. Úprava proměnných a potom zkopírujte a vložte do konzoly PowerShell.

    $Sub1          = "Replace_With_Your_Subcription_Name"
    $RG1           = "TestBGPRG1"
    $Location1     = "East US"
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
    $GWIPName1     = "VNet1GWIP"
    $GWIPconfName1 = "gwipconf1"
    $Connection12  = "VNet1toVNet2"
    $Connection15  = "VNet1toSite5"

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

### <a name="step-2---create-the-vpn-gateway-for-testvnet1-with-bgp-parameters"></a>Krok 2 – Vytvoření brány sítě VPN pro TestVNet1 s parametry BGP

#### <a name="1-create-the-ip-and-subnet-configurations"></a>1. konfigurace IP a podsítě vytvořit

Požádat o veřejné IP adresu, který se má přidělit brány vytvořené pro vaše VNet. Budete taky definovat podsítí a konfigurace IP povinné. 

    $gwpip1    = New-AzureRmPublicIpAddress -Name $GWIPName1 -ResourceGroupName $RG1 -Location $Location1 -AllocationMethod Dynamic
    
    $vnet1     = Get-AzureRmVirtualNetwork -Name $VNetName1 -ResourceGroupName $RG1
    $subnet1   = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet1
    $gwipconf1 = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GWIPconfName1 -Subnet $subnet1 -PublicIpAddress $gwpip1

#### <a name="2-create-the-vpn-gateway-with-the-as-number"></a>2. Vytvořte bránu VPN jako číslo

Vytvoření brány virtuální sítě pro TestVNet1. Všimněte si, že BGP vyžaduje brány na základě směrování VPN a přidání parametru - Asn, pokud chcete nastavit ASN (číslo) pro TestVNet1. Vytvoření brány může chvíli trvat (nejméně 30 minut dokončete).

    New-AzureRmVirtualNetworkGateway -Name $GWName1 -ResourceGroupName $RG1 -Location $Location1 -IpConfigurations $gwipconf1 -GatewayType Vpn -VpnType RouteBased -GatewaySku HighPerformance -Asn $VNet1ASN

#### <a name="3-obtain-the-azure-bgp-peer-ip-address"></a>3. získá Azure BGP mezi dvěma účastníky IP adresu

Po vytvoření brány bude muset získá BGP mezi dvěma účastníky IP adresu na Brána VPN Azure. Tuto adresu je potřeba ke konfiguraci brány VPN Azure jako partner BGP zařízení VPN místní.

    $vnet1gw = Get-AzureRmVirtualNetworkGateway -Name $GWName1 -ResourceGroupName $RG1
    $vnet1gw.BgpSettingsText

Posledního příkazu zobrazí odpovídající BGP konfigurace brány sítě VPN Azure; Příklad:

    $vnet1gw.BgpSettingsText
    {
        "Asn": 65010,
        "BgpPeeringAddress": "10.12.255.30",
        "PeerWeight": 0
    }

Po vytvoření brány slouží k vytvoření připojení mezi místní nebo VNet VNet s BGP tuto bránu. V následujících částech provede jednotlivými kroky k dokončení výkonu.

## <a name ="crossprembbgp"></a>Část 2 – navázat spojení mezi místní s BGP

Navázat spojení mezi místní, je potřeba vytvořit místní brána sítě představující zařízení místní VPN a připojení Azure VPN brány brána místní síti. Rozdíl mezi pokyny v tomto článku je nutné určit parametry konfigurace BGP další vlastnosti.

![BGP pro místní více](./media/vpn-gateway-bgp-resource-manager-ps/bgp-crossprem.png)

Než budete pokračovat, ujistěte se, že jste dokončili [část 1](#enablebgp) tohoto cvičení.

### <a name="step-1---create-and-configure-the-local-network-gateway"></a>Krok 1: vytvoření a konfigurace místní síti brány

#### <a name="1-declare-your-variables"></a>1. vaše proměnné deklarovat

Tento cvičení zůstanou sestavit konfigurace v diagramu. Ujistěte se, chcete-li nahradit hodnoty ty, které chcete použít pro konfiguraci.

    $RG5           = "TestBGPRG5"
    $Location5     = "East US 2"
    $LNGName5      = "Site5"
    $LNGPrefix50   = "10.52.255.254/32"
    $LNGIP5        = "Your_VPN_Device_IP"
    $LNGASN5       = 65050
    $BGPPeerIP5    = "10.52.255.254"

Několik věcí, které poznámka týkající se parametry brány místní sítě:

- Místní síti brány může být ve stejném nebo jiném umístění a pole Skupina zdroje jako brány sítě VPN. Tento příklad ukazuje je do jiné skupiny prostředků na různých místech.

- Minimální předponu, které chcete deklarovat brány místní síti je adresa host BGP mezi dvěma účastníky IP adresy na zařízení s VPN. V tomto případě je /32 předpona "10.52.255.254/32".

- Připomínáme je nutné použít jiný BGP avíza expedice zboží mezi místní sítě a Azure VNet. Pokud jsou stejná, budete muset změnit VNet ASN používáte zařízení VPN místní už ASN rovnocenných počítačů sousední jiných BGP.
    
Než budete pokračovat, prosím zkontrolujte, jestli že jste pořád připojení k odběru 1.

#### <a name="2-create-the-local-network-gateway-for-site5"></a>2. Vytvořte bránu místní síti pro Site5

Je potřeba vytvořit skupiny zdrojů, pokud není vytvořený před vytvořením brány místní síti. Všimněte si dvou další parametry brány místní síti: Asn a BgpPeerAddress.

    New-AzureRmResourceGroup -Name $RG5 -Location $Location5

    New-AzureRmLocalNetworkGateway -Name $LNGName5 -ResourceGroupName $RG5 -Location $Location5 -GatewayIpAddress $LNGIP5 -AddressPrefix $LNGPrefix50 -Asn $LNGASN5 -BgpPeeringAddress $BGPPeerIP5

### <a name="step-2---connect-the-vnet-gateway-and-local-network-gateway"></a>Krok 2 – VNet brány a místní síti brány připojení

#### <a name="1-get-the-two-gateways"></a>1. získat dvě brány

        $vnet1gw = Get-AzureRmVirtualNetworkGateway -Name $GWName1  -ResourceGroupName $RG1
        $lng5gw  = Get-AzureRmLocalNetworkGateway -Name $LNGName5 -ResourceGroupName $RG5

#### <a name="2-create-the-testvnet1-to-site5-connection"></a>2. Vytvořte TestVNet1 Site5 připojení

V tomto kroku vytvoříte připojení z TestVNet1 k Site5. Je nutné zadat "-EnableBGP $True" Chcete-li povolit BGP u tohoto připojení. Jak bylo popsáno dříve, je možné používat BGP i bez BGP připojení stejné brány VPN Azure. Pokud BGP aktivované ve vlastnosti připojení, nepovolí Azure u tohoto připojení BGP, i když BGP parametrů jsou již nakonfigurovány na obou brány.

    New-AzureRmVirtualNetworkGatewayConnection -Name $Connection15 -ResourceGroupName $RG1 -VirtualNetworkGateway1 $vnet1gw -LocalNetworkGateway2 $lng5gw -Location $Location1 -ConnectionType IPsec -SharedKey 'AzureA1b2C3' -EnableBGP $True


V příkladu níže jsou uvedeny parametrů, které zadáte do části Konfigurace BGP na zařízení VPN místní tento výkon:

    - Site5 ASN: 65050
    - Site5 BGP IP: 10.52.255.254
    - Předpony pro oznámení: (například) 10.51.0.0/16 a 10.52.0.0/16
    - Azure VNet ASN: 65010
    - Azure VNet BGP IP: 10.12.255.30
    - Statická trasa: přidat trasa 10.12.255.30/32, s přesměrování právě rozhraní tunelem VPN na vašem zařízení
    - eBGP pokus: zajistit, aby možnost "pokus" eBGP je povolena na vašem zařízení, v případě potřeby

Připojení stanovit za několik minut a peering relace BGP spustí se po připojení IPsec.
 
## <a name ="v2vbgp"></a>Část 3 – připojení VNet VNet s BGP

Tato část přidá VNet VNet připojení k BGP, jak ukazuje následující obrázek. 

![BGP VNet VNet](./media/vpn-gateway-bgp-resource-manager-ps/bgp-vnet2vnet.png)

Tyto kroky udělat dál z předchozích kroků výše uvedené. Musíte nejdřív udělat [I části](#enablebgp) vytvoření a konfigurace TestVNet1 a VPN brány s BGP. 

### <a name="step-1---create-testvnet2-and-the-vpn-gateway"></a>Krok 1: vytvoření TestVNet2 a Brána VPN

Je důležité, abyste měli jistotu, že adresní prostor IP nové virtuální sítě TestVNet2, nepřekrývá pomocí kterékoli z oblastí VNet.

V tomto příkladu virtuálních sítí patří do stejného předplatného. Nastavení připojení VNet VNet mezi různých předplatných; získáte [Konfigurovat VNet VNet připojení](./vpn-gateway-vnet-vnet-rm-ps.md) se dozvíte víc informací. Ujistěte se, přidáte "-EnableBgp $True" po vytvoření připojení k povolení BGP.

#### <a name="1-declare-your-variables"></a>1. vaše proměnné deklarovat

Ujistěte se, chcete-li nahradit hodnoty ty, které chcete použít pro konfiguraci.

    $RG2           = "TestBGPRG2"
    $Location2     = "West US"
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
    $GWIPName2     = "VNet2GWIP"
    $GWIPconfName2 = "gwipconf2"
    $Connection21  = "VNet2toVNet1"
    $Connection12  = "VNet1toVNet2"

#### <a name="2-create-testvnet2-in-the-new-resource-group"></a>2. Vytvořte TestVNet2 do nové skupiny prostředků

    New-AzureRmResourceGroup -Name $RG2 -Location $Location2
    
    $fesub2 = New-AzureRmVirtualNetworkSubnetConfig -Name $FESubName2 -AddressPrefix $FESubPrefix2
    $besub2 = New-AzureRmVirtualNetworkSubnetConfig -Name $BESubName2 -AddressPrefix $BESubPrefix2
    $gwsub2 = New-AzureRmVirtualNetworkSubnetConfig -Name $GWSubName2 -AddressPrefix $GWSubPrefix2

    New-AzureRmVirtualNetwork -Name $VNetName2 -ResourceGroupName $RG2 -Location $Location2 -AddressPrefix $VNetPrefix21,$VNetPrefix22 -Subnet $fesub2,$besub2,$gwsub2

#### <a name="3-create-the-vpn-gateway-for-testvnet2-with-bgp-parameters"></a>3. Vytvořte brány sítě VPN pro TestVNet2 s parametry BGP

Požádat o veřejnou IP adresu přiřazen k bráně vytvořené pro vaše VNet. Budete taky definovat podsítí a konfigurace IP povinné. 

    $gwpip2    = New-AzureRmPublicIpAddress -Name $GWIPName2 -ResourceGroupName $RG2 -Location $Location2 -AllocationMethod Dynamic

    $vnet2     = Get-AzureRmVirtualNetwork -Name $VNetName2 -ResourceGroupName $RG2
    $subnet2   = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet2
    $gwipconf2 = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GWIPconfName2 -Subnet $subnet2 -PublicIpAddress $gwpip2

Vytvoření brány VPN jako číslo. Všimněte si, že musíte přepsat výchozí ASN na Azure VPN brány. Avíza expedice zboží pro připojení VNets musejí být různé povolení směrování přenosu a BGP.

    New-AzureRmVirtualNetworkGateway -Name $GWName2 -ResourceGroupName $RG2 -Location $Location2 -IpConfigurations $gwipconf2 -GatewayType Vpn -VpnType RouteBased -GatewaySku Standard -Asn $VNet2ASN

### <a name="step-2---connect-the-testvnet1-and-testvnet2-gateways"></a>Krok 2 – připojení TestVNet1 a TestVNet2 brány

V tomto příkladu jsou obě brány v rámci stejného předplatného. Tento krok může udělat ve stejné relaci Powershellu.

#### <a name="1-get-both-gateways"></a>1. obě brány získat

Ujistěte se, přihlásit a připojte se k odběru 1.

    $vnet1gw = Get-AzureRmVirtualNetworkGateway -Name $GWName1 -ResourceGroupName $RG1
    $vnet2gw = Get-AzureRmVirtualNetworkGateway -Name $GWName2 -ResourceGroupName $RG2
    
#### <a name="2-create-both-connections"></a>2. obě připojení vytvořit

V tomto kroku vytvoříte spojení z TestVNet1 TestVNet2 a připojení z TestVNet2 k TestVNet1.

    New-AzureRmVirtualNetworkGatewayConnection -Name $Connection12 -ResourceGroupName $RG1 -VirtualNetworkGateway1 $vnet1gw -VirtualNetworkGateway2 $vnet2gw -Location $Location1 -ConnectionType Vnet2Vnet -SharedKey 'AzureA1b2C3' -EnableBgp $True

    New-AzureRmVirtualNetworkGatewayConnection -Name $Connection21 -ResourceGroupName $RG2 -VirtualNetworkGateway1 $vnet2gw -VirtualNetworkGateway2 $vnet1gw -Location $Location2 -ConnectionType Vnet2Vnet -SharedKey 'AzureA1b2C3' -EnableBgp $True

>[AZURE.IMPORTANT] Nezapomeňte povolit BGP pro obě připojení.

Po dokončení těchto kroků, připojení se vytvoří za několik minut a peering relace BGP bude jednou připojení VNet VNet dokončit.

Pokud jste dokončili všechny tři části tohoto cvičení, bude zajistíte topologie sítě jak je ukázáno v následujícím příkladu:

![BGP VNet VNet](./media/vpn-gateway-bgp-resource-manager-ps/bgp-crosspremv2v.png)

## <a name="next-steps"></a>Další kroky

Po dokončení připojení lze virtuálních počítačích vašich virtuální sítí. Postup v tématu [Vytvoření virtuálního počítače](../virtual-machines/virtual-machines-windows-hero-tutorial.md) .

