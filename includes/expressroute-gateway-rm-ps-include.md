Postup pro tento úkol používá VNet podle níže uvedených hodnot. Další nastavení a názvy jsou uvedeny také v tomto seznamu. Není použijeme tento seznam přímo v některém z kroků, i když přičteme proměnné na základě hodnot v tomto seznamu. Můžete zkopírovat seznam má být použit jako odkaz na nahradit hodnoty vlastní.

Konfigurace seznam odkazů:
    
- Virtuální Network Name = "TestVNet"
- Virtuální sítě adresní prostor = 192.168.0.0/16
- Pole Skupina zdroje = "TestRG"
- Název Podsíť1 = "FrontEnd" 
- Podsíť1 adresní prostor = "192.168.0.0/16"
- Název brány podsítě: "GatewaySubnet" název musí být vždy brány podsítě *GatewaySubnet*.
- Brána podsítě adresní prostor = "192.168.200.0/26"
- Oblast = "Východoasijských USA"
- Název brány = "GW"
- Název brány IP = "GWIP"
- Konfigurace brány IP název = "gwipconf"
-  Typ = "ExpressRoute" Tento typ je nutný pro konfiguraci ExpressRoute.
- Název brány veřejnou IP = "gwpip"


## <a name="add-a-gateway"></a>Přidání brány

1. Připojení k předplatnému Azure. 

        Login-AzureRmAccount
        Get-AzureRmSubscription 
        Select-AzureRmSubscription -SubscriptionName "Name of subscription"

2. Deklarujte proměnných pro toto cvičení. V tomto příkladě se použít proměnných v následující ukázce. Ujistěte se, chcete-li upravit toto nastavení, které chcete použít. 
        
        $RG = "TestRG"
        $Location = "East US"
        $GWName = "GW"
        $GWIPName = "GWIP"
        $GWIPconfName = "gwipconf"
        $VNetName = "TestVNet"

3. Uložení virtuální sítě objekt jako proměnné.

        $vnet = Get-AzureRmVirtualNetwork -Name $VNetName -ResourceGroupName $RG

4. Připojení k síti virtuální podsítě brány. Podsítě brány musí s názvem "GatewaySubnet". Je vhodné vytvořit brány, která je /27 nebo větší (/ 26, / 25, atd.).
            
        Add-AzureRmVirtualNetworkSubnetConfig -Name GatewaySubnet -VirtualNetwork $vnet -AddressPrefix 192.168.200.0/26

5. Nastavení.

            Set-AzureRmVirtualNetwork -VirtualNetwork $vnet

6. Uložení podsítě brány jako proměnné.

        $subnet = Get-AzureRmVirtualNetworkSubnetConfig -Name 'GatewaySubnet' -VirtualNetwork $vnet

7. Požádat o veřejnou IP adresu. IP adresu žádá před vytvořením brány. Nelze zadat IP adresy, kterou chcete použít; dynamicky přidělené. Tuto IP adresu použijete v další části konfigurace. AllocationMethod musí být dynamické.

        $pip = New-AzureRmPublicIpAddress -Name gwpip -ResourceGroupName $RG -Location $Location -AllocationMethod Dynamic

8. Vytvoření konfiguraci brány. Konfigurace brány definuje podsítě a veřejnou IP adresu používat. V tomto kroku zadáváte konfiguraci, použijí se při vytváření brány. Tento krok není vytvořit skutečně brány objekt. Umožňuje vytvořit konfiguraci brány následující ukázce. 

        $ipconf = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GWIPconfName -Subnet $subnet -PublicIpAddress $pip

9. Vytvořte bránu. V tomto kroku **– GatewayType** záleží zvlášť. Je nutné použít hodnotu **ExpressRoute**. Všimněte si, že po dokončení těchto rutin, brány může trvat a 20 minut vytvořit.

        New-AzureRmVirtualNetworkGateway -Name $GWName -ResourceGroupName $RG -Location $Location -IpConfigurations $ipconf -GatewayType Expressroute -GatewaySku Standard

## <a name="verify-the-gateway-was-created"></a>Ověřte, jestli že byla vytvořená brány

Příkazem dole ověřit, že byl vytvořen brány.

    Get-AzureRmVirtualNetworkGateway -ResourceGroupName $RG

## <a name="resize-a-gateway"></a>Změna velikosti brány

Existuje celá řada [Skladové jednotky brány](../articles/expressroute/expressroute-about-virtual-network-gateways.md). Tento příkaz můžete kdykoli změnit SKU brány.

>[AZURE.IMPORTANT] Tento příkaz nefunguje UltraPerformance brány. Pokud chcete změnit brána k bráně UltraPerformance, nejprve odebrat existující ExpressRoute brány a vytvořte novou bránu UltraPerformance. Snížit verzi brány z UltraPerformance brány, nejdřív odeberte UltraPerformance brány a vytvořte novou bránu.

    $gw = Get-AzureRmVirtualNetworkGateway -Name $GWName -ResourceGroupName $RG
    Resize-AzureRmVirtualNetworkGateway -VirtualNetworkGateway $gw -GatewaySku HighPerformance

## <a name="remove-a-gateway"></a>Odebrání brány

Pomocí příkazu dole můžete odebrat brány

    Remove-AzureRmVirtualNetworkGateway -Name $GWName -ResourceGroupName $RG  
