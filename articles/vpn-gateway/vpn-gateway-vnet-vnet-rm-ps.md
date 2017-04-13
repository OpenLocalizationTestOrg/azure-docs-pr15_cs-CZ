<properties
   pageTitle="Připojení Azure VNets s Brána VPN a prostředí PowerShell | Microsoft Azure"
   description="Tento článek vás provede připojení virtuální sítě společně s použitím správce prostředků Azure a Powershellu."
   services="vpn-gateway"
   documentationCenter="na"
   authors="cherylmc"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager"/>

<tags
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="08/31/2016"
   ms.author="cherylmc"/>

# <a name="configure-a-vnet-to-vnet-connection-for-resource-manager-using-powershell"></a>Konfigurace připojení VNet VNet pro správce prostředků pomocí prostředí PowerShell

> [AZURE.SELECTOR]
- [Správce prostředků - Azure portálu](vpn-gateway-howto-vnet-vnet-resource-manager-portal.md)
- [Správce prostředků - prostředí PowerShell](vpn-gateway-vnet-vnet-rm-ps.md)
- [Klasický – klasické portálu](virtual-networks-configure-vnet-to-vnet-connection.md)

Tento článek vás provede kroky k vytvoření připojení mezi VNets v modelu nasazení Správce prostředků pomocí VPN brány. Virtuální sítě může být ve stejném nebo jiném oblastech a od předplatná stejném nebo jiném.


![v2v diagram](./media/vpn-gateway-vnet-vnet-rm-ps/v2vrmps.png)

### <a name="deployment-models-and-methods-for-vnet-to-vnet-connections"></a>Modely nasazení a metody pro VNet VNet připojení

[AZURE.INCLUDE [deployment models](../../includes/vpn-gateway-deployment-models-include.md)]

Následující tabulka zobrazuje modelů momentálně neexistuje nasazení a metody konfigurace VNet VNet. Při článek s kroky konfigurace je k dispozici, jsme propojit přímo ji z této tabulky.

[AZURE.INCLUDE [vpn-gateway-table-vnet-vnet](../../includes/vpn-gateway-table-vnet-to-vnet-include.md)]

#### <a name="vnet-peering"></a>Prozkoumávání VNet

[AZURE.INCLUDE [vpn-gateway-vnetpeeringlink](../../includes/vpn-gateway-vnetpeeringlink-include.md)]


## <a name="about-vnet-to-vnet-connections"></a>Informace o připojení VNet VNet

Připojení k jiné síti virtuální virtuální sítě je podobný připojení VNet k umístění webu místní (VNet VNet). Oba typy připojení se k zajištění zabezpečené tunelem pomocí IPsec/IKE používají Azure VPN brány. V různých oblastech může být VNets, ke kterému se můžete připojit. Nebo v různých předplatných. Můžete dokonce kombinovat VNet VNet komunikace s konfigurací více webu. Toto oprávnění umožňuje vytvořit topologie sítě, které kombinují křížově místní připojení s připojením mezi virtuální sítě, jak je znázorněno na následujícím obrázku:


![Informace o připojení](./media/vpn-gateway-vnet-vnet-rm-ps/aboutconnections.png)
 
### <a name="why-connect-virtual-networks"></a>Proč připojit virtuální sítě?

Můžete chtít připojit virtuální sítě z těchto důvodů:

- **Křížové oblast geo redundance a geo informací o stavu**
    - Můžete nastavit vlastní geo replikace a synchronizace s zabezpečené připojení nemusíte přecházet prostřednictvím internetového koncové body.
    - Správce přenosy Azure a vyrovnávání zatížení můžete nastavit vysoce dostupné úlohu s geo redundance napříč několika Azure oblastí. Příkladem důležité je nastavení SQL vždy na skupinám dostupnost šíření napříč několika Azure oblastí.

- **Místní aplikací vícevrstvého s izolace nebo administrativní hranice**
    - Ve stejné oblasti můžete nastavit vícevrstvého aplikací s více virtuální sítí propojeny kvůli izolace nebo požadavky pro správu.


### <a name="vnet-to-vnet-faq"></a>Nejčastější dotazy týkající se VNet VNet

[AZURE.INCLUDE [vpn-gateway-vnet-vnet-faq](../../includes/vpn-gateway-vnet-vnet-faq-include.md)] 


## <a name="which-set-of-steps-should-i-use"></a>Jaký kroků mám použít?

V tomto článku se zobrazí dvě sady jednoduchých kroků. Jednu sadu kroků pro [VNets uložených v rámci stejného předplatného](#samesub)a jiné pro [VNets uložených v různých předplatných](#difsub). Klíčové rozdíl mezi výše uvedené množiny je zda můžete vytvořit a nakonfigurovat všechny virtuální sítě a brány zdroje v rámci stejné relaci Powershellu.

Kroky v tomto článku pomocí proměnných deklarované na začátku každého oddílu. Pokud už pracujete s existující VNets, upravte proměnné tak, aby odrážely nastavení vlastního prostředí. 

![Obě připojení](./media/vpn-gateway-vnet-vnet-rm-ps/differentsubscription.png)


## <a name="samesub"></a>Jak se připojit VNets, které jsou ve stejném předplatném

![v2v diagram](./media/vpn-gateway-vnet-vnet-rm-ps/v2vrmps.png)

### <a name="before-you-begin"></a>Než začnete
    
Než začnete, budete potřebovat k instalaci rutiny prostředí PowerShell Azure správce prostředků. Zjistěte, [Jak nainstalovat a nakonfigurovat Azure PowerShell](../powershell-install-configure.md) Další informace o instalaci rutiny prostředí PowerShell.

### <a name="Step1"></a>Krok 1: plánování rozsahy IP adres


V následujících krocích vytvoříme dvě virtuální sítě spolu s jejich podsítí odpovídajících brány a konfigurace. Potom vytvoříme připojení VPN mezi dvěma VNets. Je důležité plánovat rozsahy IP adres pro konfiguraci sítě. Mějte na paměti, ujistěte se, že žádný z oblasti VNet nebo místní síti oblasti překrývat žádným způsobem.

V příkladech používáme následující hodnoty:

**Hodnoty pro TestVNet1:**

- Název VNet: TestVNet1
- Pole Skupina zdroje: TestRG1
- Umístění: Východní USA
- TestVNet1: 10.11.0.0/16 & 10.12.0.0/16
- FrontEnd: 10.11.0.0/24
- Back-end: 10.12.0.0/24
- GatewaySubnet: 10.12.255.0/27
- DNS Server: 8.8.8.8
- Názevbrány: VNet1GW
- Veřejné IP: VNet1GWIP
- VPNType: RouteBased
- Connection(1to4): VNet1toVNet4
- Connection(1to5): VNet1toVNet5
- ConnectionType: VNet2VNet


**Hodnoty pro TestVNet4:**

- Název VNet: TestVNet4
- TestVNet2: 10.41.0.0/16 & 10.42.0.0/16
- FrontEnd: 10.41.0.0/24
- Back-end: 10.42.0.0/24
- GatewaySubnet: 10.42.255.0/27
- Pole Skupina zdroje: TestRG4
- Umístění: Západní USA
- DNS Server: 8.8.8.8
- Názevbrány: VNet4GW
- Veřejné IP: VNet4GWIP
- VPNType: RouteBased
- Připojení: VNet4toVNet1
- ConnectionType: VNet2VNet



### <a name="Step2"></a>Krok 2: vytvoření a konfigurace TestVNet1

1. Deklarovat proměnných

    Začněte tím, že deklarování proměnných. Tomto příkladu dojde deklarování proměnné pomocí hodnoty pro tento cvičení. Ve většině případů měli byste nahradit hodnoty vlastní. Pokud používáte kroky vám pomůžou seznámit s tímto typem konfigurace mimo však může použít tyto proměnné. Úprava proměnných v případě potřeby a pak zkopírujte a vložte do konzoly Powershellu.

        $Sub1 = "Replace_With_Your_Subcription_Name"
        $RG1 = "TestRG1"
        $Location1 = "East US"
        $VNetName1 = "TestVNet1"
        $FESubName1 = "FrontEnd"
        $BESubName1 = "Backend"
        $GWSubName1 = "GatewaySubnet"
        $VNetPrefix11 = "10.11.0.0/16"
        $VNetPrefix12 = "10.12.0.0/16"
        $FESubPrefix1 = "10.11.0.0/24"
        $BESubPrefix1 = "10.12.0.0/24"
        $GWSubPrefix1 = "10.12.255.0/27"
        $DNS1 = "8.8.8.8"
        $GWName1 = "VNet1GW"
        $GWIPName1 = "VNet1GWIP"
        $GWIPconfName1 = "gwipconf1"
        $Connection14 = "VNet1toVNet4"
        $Connection15 = "VNet1toVNet5"

2. Připojení k vašemu předplatnému

    Přepněte do režimu Powershellu pro používání rutin Správce prostředků. Spusťte konzolu prostředí PowerShell a připojit se ke svému účtu. Pomocí následující příklad můžete připojit:

        Login-AzureRmAccount

    Zaškrtněte políčko předplatná pro účet.

        Get-AzureRmSubscription 

    Určení předplatné, ke které chcete použít.

        Select-AzureRmSubscription -SubscriptionName $Sub1

3. Vytvoření nové skupiny prostředků

        New-AzureRmResourceGroup -Name $RG1 -Location $Location1

4. Vytvořit konfigurace podsítě pro TestVNet1

    Tento příklad vytvoří virtuální sítě s názvem TestVNet1 a tři podsítě jeden jen GatewaySubnet, jeden jen FrontEnd a jeden jen back-end. Nahrazení hodnot, je důležité, vždy název vaší podsítě brány konkrétně GatewaySubnet. Pokud zadáte název je něco jiného, vytvoření brány se nezdaří. 

    Následující příklad využívá proměnné, které jste nastavili dříve. V tomto příkladu používá podsítě brány /27. Když je možné vytvořit brány podsítě nejnižší /29, doporučujeme vytvoření větší podsítě obsahující víc adres tak, že vyberete aspoň /28 nebo /27. Toto nastavení umožňuje dost adresy tak, aby pokryly možné konfigurace další požadované může v budoucnosti. 

        $fesub1 = New-AzureRmVirtualNetworkSubnetConfig -Name $FESubName1 -AddressPrefix $FESubPrefix1
        $besub1 = New-AzureRmVirtualNetworkSubnetConfig -Name $BESubName1 -AddressPrefix $BESubPrefix1
        $gwsub1 = New-AzureRmVirtualNetworkSubnetConfig -Name $GWSubName1 -AddressPrefix $GWSubPrefix1


5. Vytvoření TestVNet1

        New-AzureRmVirtualNetwork -Name $VNetName1 -ResourceGroupName $RG1 `
        -Location $Location1 -AddressPrefix $VNetPrefix11,$VNetPrefix12 -Subnet $fesub1,$besub1,$gwsub1

6. Požadavek veřejnou IP adresu

    Požádat o veřejnou IP adresu přiřazen k bráně vytvořené pro vaše VNet. Všimněte si, že AllocationMethod dynamické. Nelze zadat IP adresy, kterou chcete použít. Je to dynamicky přidělit brány. 

        $gwpip1 = New-AzureRmPublicIpAddress -Name $GWIPName1 -ResourceGroupName $RG1 `
        -Location $Location1 -AllocationMethod Dynamic

7. Vytvoření konfiguraci brány

    Konfigurace brány definuje podsítě a veřejnou IP adresu používat. V příkladu slouží k vytvoření konfiguraci brány. 

        $vnet1 = Get-AzureRmVirtualNetwork -Name $VNetName1 -ResourceGroupName $RG1
        $subnet1 = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet1
        $gwipconf1 = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GWIPconfName1 `
        -Subnet $subnet1 -PublicIpAddress $gwpip1

8. Vytvoření brány pro TestVNet1

    V tomto kroku vytvoříte brány virtuální sítě pro vaše TestVNet1. Konfigurace VNet VNet vyžadují RouteBased VpnType. Vytvoření brány může chvíli trvat (45 minut nebo déle).

        New-AzureRmVirtualNetworkGateway -Name $GWName1 -ResourceGroupName $RG1 `
        -Location $Location1 -IpConfigurations $gwipconf1 -GatewayType Vpn `
        -VpnType RouteBased -GatewaySku Standard

### <a name="step-3---create-and-configure-testvnet4"></a>Krok 3: vytvoření a konfigurace TestVNet4

Po konfiguraci TestVNet1 vytvořte TestVNet4. Pomocí následujících kroků, nahradit hodnoty vlastní potřeby. Tento krok můžete vykonat v rámci stejné relaci Powershellu, protože je ve stejném předplatném.

1. Deklarovat proměnných

    Ujistěte se, chcete-li nahradit hodnoty ty, které chcete použít pro konfiguraci.

        $RG4 = "TestRG4"
        $Location4 = "West US"
        $VnetName4 = "TestVNet4"
        $FESubName4 = "FrontEnd"
        $BESubName4 = "Backend"
        $GWSubName4 = "GatewaySubnet"
        $VnetPrefix41 = "10.41.0.0/16"
        $VnetPrefix42 = "10.42.0.0/16"
        $FESubPrefix4 = "10.41.0.0/24"
        $BESubPrefix4 = "10.42.0.0/24"
        $GWSubPrefix4 = "10.42.255.0/27"
        $DNS4 = "8.8.8.8"
        $GWName4 = "VNet4GW"
        $GWIPName4 = "VNet4GWIP"
        $GWIPconfName4 = "gwipconf4"
        $Connection41 = "VNet4toVNet1"

    Než budete pokračovat, zkontrolujte, jestli že jste pořád připojení k odběru 1.

2. Vytvoření nové skupiny prostředků

        New-AzureRmResourceGroup -Name $RG4 -Location $Location4

3. Vytvořit konfigurace podsítě pro TestVNet4

        $fesub4 = New-AzureRmVirtualNetworkSubnetConfig -Name $FESubName4 -AddressPrefix $FESubPrefix4
        $besub4 = New-AzureRmVirtualNetworkSubnetConfig -Name $BESubName4 -AddressPrefix $BESubPrefix4
        $gwsub4 = New-AzureRmVirtualNetworkSubnetConfig -Name $GWSubName4 -AddressPrefix $GWSubPrefix4

4. Vytvoření TestVNet4

        New-AzureRmVirtualNetwork -Name $VnetName4 -ResourceGroupName $RG4 `
        -Location $Location4 -AddressPrefix $VnetPrefix41,$VnetPrefix42 -Subnet $fesub4,$besub4,$gwsub4

5. Požadavek veřejnou IP adresu

        $gwpip4 = New-AzureRmPublicIpAddress -Name $GWIPName4 -ResourceGroupName $RG4 `
        -Location $Location4 -AllocationMethod Dynamic

6. Vytvoření konfiguraci brány

        $vnet4 = Get-AzureRmVirtualNetwork -Name $VnetName4 -ResourceGroupName $RG4
        $subnet4 = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet4
        $gwipconf4 = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GWIPconfName4 -Subnet $subnet4 -PublicIpAddress $gwpip4

7. Vytvoření brány TestVNet4

        New-AzureRmVirtualNetworkGateway -Name $GWName4 -ResourceGroupName $RG4 `
        -Location $Location4 -IpConfigurations $gwipconf4 -GatewayType Vpn `
        -VpnType RouteBased -GatewaySku Standard

### <a name="step-4---connect-the-gateways"></a>Krok 4 – Připojte brány

1. Získání obě brány virtuální sítě

    V tomto příkladu protože ve stejném předplatném, jsou obě brány tento krok lze provést ve stejné relaci Powershellu.

        $vnet1gw = Get-AzureRmVirtualNetworkGateway -Name $GWName1 -ResourceGroupName $RG1
        $vnet4gw = Get-AzureRmVirtualNetworkGateway -Name $GWName4 -ResourceGroupName $RG4


2. Vytvoření TestVNet1 TestVNet4 připojení

    V tomto kroku vytvoříte připojení z TestVNet1 TestVNet4. Zobrazí se odkazuje v příkladech sdíleného klíče. Můžete použít vlastní hodnoty pro klávesu sdílené. Důležité je, že klávesu sdílené se musí shodovat pro obě připojení. Vytvoření připojení můžete chvíli krátké dokončete.

        New-AzureRmVirtualNetworkGatewayConnection -Name $Connection14 -ResourceGroupName $RG1 `
        -VirtualNetworkGateway1 $vnet1gw -VirtualNetworkGateway2 $vnet4gw -Location $Location1 `
        -ConnectionType Vnet2Vnet -SharedKey 'AzureA1b2C3'

3. Vytvoření TestVNet4 TestVNet1 připojení

    Tento krok je podobná té výše, s výjimkou vytvoříte připojení z TestVNet4 k TestVNet1. Zkontrolujte, jestli že sdílené klíče shodují.

        New-AzureRmVirtualNetworkGatewayConnection -Name $Connection41 -ResourceGroupName $RG4 `
        -VirtualNetworkGateway1 $vnet4gw -VirtualNetworkGateway2 $vnet1gw -Location $Location4 `
        -ConnectionType Vnet2Vnet -SharedKey 'AzureA1b2C3'

    Připojení stanovit po několika minutách.

4. Ověřte připojení. Naleznete v části [ověření připojení](#verify).


## <a name="difsub"></a>Jak se připojit VNets, které jsou v různých předplatných


![v2v diagram](./media/vpn-gateway-vnet-vnet-rm-ps/v2vdiffsub.png)

V tomto případě jsme TestVNet1 a připojení TestVNet5. TestVNet1 a TestVNet5 uložena v jiné předplatné. Kroky pro tuto konfiguraci přidání připojení k další VNet VNet budete moct připojit TestVNet1 k TestVNet5. 

Rozdíl tady je několik kroků, konfiguraci potřeba provést v samostatném relaci Powershellu v kontextu druhý předplatného. Hlavně když dvě předplatná patří k jiným organizacím. 

Pokračujte v z předchozích kroků uvedených pokynů. [Krok 1](#Step1) a [2 krok](#Step2) můžete vytvořit a nakonfigurovat TestVNet1 a Brána VPN pro TestVNet1, musíte nejdřív udělat. Po dokončení kroku 1 a 2 krok, pokračujte krokem 5 a vytvořte TestVNet5.

### <a name="step-5---verify-the-additional-ip-address-ranges"></a>Krok 5 – ověření Další rozsahy IP adres

Je důležité, abyste měli jistotu, že adresní prostor IP nové virtuální sítě TestVNet5, nepřekrývá pomocí kterékoli z oblasti VNet nebo oblasti brány místní síti. 

V tomto příkladu může virtuálních sítí patří k jiným organizacím. U tohoto cvičení můžete provádět následující hodnoty TestVNet5:

**Hodnoty pro TestVNet5:**

- Název VNet: TestVNet5
- Pole Skupina zdroje: TestRG5
- Umístění: Východ Japonsko
- TestVNet5: 10.51.0.0/16 & 10.52.0.0/16
- FrontEnd: 10.51.0.0/24
- Back-end: 10.52.0.0/24
- GatewaySubnet: 10.52.255.0.0/27
- DNS Server: 8.8.8.8
- Názevbrány: VNet5GW
- Veřejné IP: VNet5GWIP
- VPNType: RouteBased
- Připojení: VNet5toVNet1
- ConnectionType: VNet2VNet

**Další hodnoty pro TestVNet1:**

- Připojení: VNet1toVNet5


### <a name="step-6---create-and-configure-testvnet5"></a>Krok 6: vytváření a konfigurace TestVNet5

Tento krok mohou provést v kontextu nové předplatné. Tato část může provádět správce v různých organizace, která vlastní předplatné.

1. Deklarovat proměnných

    Ujistěte se, chcete-li nahradit hodnoty ty, které chcete použít pro konfiguraci.

        $Sub5 = "Replace_With_the_New_Subcription_Name"
        $RG5 = "TestRG5"
        $Location5 = "Japan East"
        $VnetName5 = "TestVNet5"
        $FESubName5 = "FrontEnd"
        $BESubName5 = "Backend"
        $GWSubName5 = "GatewaySubnet"
        $VnetPrefix51 = "10.51.0.0/16"
        $VnetPrefix52 = "10.52.0.0/16"
        $FESubPrefix5 = "10.51.0.0/24"
        $BESubPrefix5 = "10.52.0.0/24"
        $GWSubPrefix5 = "10.52.255.0/27"
        $DNS5 = "8.8.8.8"
        $GWName5 = "VNet5GW"
        $GWIPName5 = "VNet5GWIP"
        $GWIPconfName5 = "gwipconf5"
        $Connection51 = "VNet5toVNet1"

2. Připojení k předplatnému 5

    Spusťte konzolu prostředí PowerShell a připojit se ke svému účtu. K připojení použijte v následujícím příkladu:

        Login-AzureRmAccount

    Zaškrtněte políčko předplatná pro účet.

        Get-AzureRmSubscription 

    Určení předplatné, ke které chcete použít.

        Select-AzureRmSubscription -SubscriptionName $Sub5

3. Vytvoření nové skupiny prostředků

        New-AzureRmResourceGroup -Name $RG5 -Location $Location5

4. Vytvořit konfigurace podsítě pro TestVNet4
    
        $fesub5 = New-AzureRmVirtualNetworkSubnetConfig -Name $FESubName5 -AddressPrefix $FESubPrefix5
        $besub5 = New-AzureRmVirtualNetworkSubnetConfig -Name $BESubName5 -AddressPrefix $BESubPrefix5
        $gwsub5 = New-AzureRmVirtualNetworkSubnetConfig -Name $GWSubName5 -AddressPrefix $GWSubPrefix5

5. Vytvoření TestVNet5

        New-AzureRmVirtualNetwork -Name $VnetName5 -ResourceGroupName $RG5 -Location $Location5 `
        -AddressPrefix $VnetPrefix51,$VnetPrefix52 -Subnet $fesub5,$besub5,$gwsub5

6. Požadavek veřejnou IP adresu

        $gwpip5 = New-AzureRmPublicIpAddress -Name $GWIPName5 -ResourceGroupName $RG5 `
        -Location $Location5 -AllocationMethod Dynamic


7. Vytvoření konfiguraci brány

        $vnet5 = Get-AzureRmVirtualNetwork -Name $VnetName5 -ResourceGroupName $RG5
        $subnet5  = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet5
        $gwipconf5 = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GWIPconfName5 -Subnet $subnet5 -PublicIpAddress $gwpip5

8. Vytvoření brány TestVNet5

        New-AzureRmVirtualNetworkGateway -Name $GWName5 -ResourceGroupName $RG5 -Location $Location5 `
        -IpConfigurations $gwipconf5 -GatewayType Vpn -VpnType RouteBased -GatewaySku Standard

### <a name="step-7---connecting-the-gateways"></a>Krok 7 – připojení brány

V tomto příkladu vzhledem k tomu, že jsou brány v různých předplatných jsme jste můžete rozdělit tento krok dva prostředí PowerShell relace označená jako [předplatné 1] a [předplatné 5].

1. **[Předplatné 1]** Získat brány virtuální sítě pro předplatné 1

    Ujistěte se, přihlaste se a připojovat se k odběru 1.

        $vnet1gw = Get-AzureRmVirtualNetworkGateway -Name $GWName1 -ResourceGroupName $RG1

    Zkopírujte výstup následující prvky a odeslat tyto správci 5 předplatné prostřednictvím e-mailu nebo jiné metody.

        $vnet1gw.Name
        $vnet1gw.Id

    Tyto dva prvky bude mít hodnoty podobně jako následující příklad výstupu:

        PS D:\> $vnet1gw.Name
        VNet1GW
        PS D:\> $vnet1gw.Id
        /subscriptions/b636ca99-6f88-4df4-a7c3-2f8dc4545509/resourceGroupsTestRG1/providers/Microsoft.Network/virtualNetworkGateways/VNet1GW

2. **[Předplatné 5]** Získat brány virtuální sítě pro předplatné 5

    Ujistěte se, přihlaste se a připojovat se k odběru 5.

        $vnet5gw = Get-AzureRmVirtualNetworkGateway -Name $GWName5 -ResourceGroupName $RG5

    Zkopírujte výstup následující prvky a odeslat tyto správci 1 předplatné prostřednictvím e-mailu nebo jiné metody.

        $vnet5gw.Name
        $vnet5gw.Id

    Tyto dva prvky bude mít hodnoty podobně jako následující příklad výstupu:

        PS C:\> $vnet5gw.Name
        VNet5GW
        PS C:\> $vnet5gw.Id
        /subscriptions/66c8e4f1-ecd6-47ed-9de7-7e530de23994/resourceGroups/TestRG5/providers/Microsoft.Network/virtualNetworkGateways/VNet5GW

3. **[Předplatné 1]** Vytvoření TestVNet1 TestVNet5 připojení

    V tomto kroku vytvoříte připojení z TestVNet1 TestVNet5. Rozdíl je, že tento vnet5gw $ nelze získat přímo, protože se nachází v jiné předplatné. Bude potřeba vytvořit nový objekt prostředí PowerShell s hodnotami oznamují od 1 předplatné výše popsaných kroků. Pomocí níže uvedeném příkladu. Nahraďte název, Id a sdílené klíč vlastní hodnoty. Důležité je, že klávesu sdílené se musí shodovat pro obě připojení. Vytvoření připojení můžete chvíli krátké dokončete.

    Zkontrolujte, že připojit k odběru 1. 
    
        $vnet5gw = New-Object Microsoft.Azure.Commands.Network.Models.PSVirtualNetworkGateway
        $vnet5gw.Name = "VNet5GW"
        $vnet5gw.Id   = "/subscriptions/66c8e4f1-ecd6-47ed-9de7-7e530de23994/resourceGroups/TestRG5/providers/Microsoft.Network/virtualNetworkGateways/VNet5GW"
        $Connection15 = "VNet1toVNet5"
        New-AzureRmVirtualNetworkGatewayConnection -Name $Connection15 -ResourceGroupName $RG1 -VirtualNetworkGateway1 $vnet1gw -VirtualNetworkGateway2 $vnet5gw -Location $Location1 -ConnectionType Vnet2Vnet -SharedKey 'AzureA1b2C3'

4. **[Předplatné 5]** Vytvoření TestVNet5 TestVNet1 připojení

    Tento krok je podobná té výše, s výjimkou vytvoříte připojení z TestVNet5 k TestVNet1. Stejný postup vytvoření prostředí PowerShell objektu na základě hodnot získané od 1 předplatné platí tady taky. V tomto kroku Ujistěte se, že odpovídají sdílené klíče.

    Zkontrolujte, že připojíte k odběru 5.

        $vnet1gw = New-Object Microsoft.Azure.Commands.Network.Models.PSVirtualNetworkGateway
        $vnet1gw.Name = "VNet1GW"
        $vnet1gw.Id = "/subscriptions/b636ca99-6f88-4df4-a7c3-2f8dc4545509/resourceGroups/TestRG1/providers/Microsoft.Network/virtualNetworkGateways/VNet1GW "
        New-AzureRmVirtualNetworkGatewayConnection -Name $Connection51 -ResourceGroupName $RG5 -VirtualNetworkGateway1 $vnet5gw -VirtualNetworkGateway2 $vnet1gw -Location $Location5 -ConnectionType Vnet2Vnet -SharedKey 'AzureA1b2C3'

## <a name="verify"></a>Ověření připojení


[AZURE.INCLUDE [vpn-gateway-no-nsg-include](../../includes/vpn-gateway-no-nsg-include.md)]


[AZURE.INCLUDE [verify connection powershell](../../includes/vpn-gateway-verify-connection-ps-rm-include.md)] 


## <a name="next-steps"></a>Další kroky

- Po dokončení připojení lze virtuálních počítačích vašich virtuální sítí. Postup v tématu [Vytvoření virtuálního počítače](../virtual-machines/virtual-machines-windows-hero-tutorial.md) .
- Informace o BGP najdete v tématu [Přehled BGP](vpn-gateway-bgp-overview.md) a [Postup při konfiguraci BGP](vpn-gateway-bgp-resource-manager-ps.md). 

