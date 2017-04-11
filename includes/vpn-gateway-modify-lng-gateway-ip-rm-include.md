Změnit adresu IP, můžete `New-AzureRmVirtualNetworkGatewayConnection` rutiny. Když si necháte název místní síti brány přesně stejné jako stávající název, dojde k přepsání nastavení. V současné době rutinu "Nastavit" nelze použít zadanou změnách brány IP adres.

### <a name="gwipnoconnection"></a>Jak změnit brány IP adresu – bez připojení brány

Aktualizace brány IP adresa brána místní síti, která dosud neobsahuje připojení, použijte následujícím příkladu. Můžete taky aktualizovat předpony adres ve stejnou dobu. Nastavení, které zadáte přepíše existující nastavení. Je nutné použít existující název místní síti brány. Pokud vytváříte novou bránu místní síti, není přepsat existující.

Použijte následující příklad nahradit hodnoty pro vlastní.

    New-AzureRmLocalNetworkGateway -Name MyLocalNetworkGWName `
    -Location "West US" -AddressPrefix @('10.0.0.0/24','20.0.0.0/24','30.0.0.0/24') `
    -GatewayIpAddress "5.4.3.2" -ResourceGroupName MyRGName


### <a name="gwipwithconnection"></a>Jak změnit IP adresu brány - existující připojení brány

Pokud brána připojení už existuje, musíte nejdřív odeberte připojení. Pak můžete upravit IP adresu brány a znovu vytvoří nové připojení. Výsledkem bude některé výpadek služeb pro připojení k síti VPN.


>[AZURE.IMPORTANT] Neodstraňujte Brána VPN. Pokud uděláte, budete muset znovu projděte kroky začínat, jakož i překonfigurovat směrovači místní s IP adresy přiřazené k bráně pro nově vytvořený.
 

1. Odeberte připojení. Najděte název připojení pomocí `Get-AzureRmVirtualNetworkGatewayConnection` rutiny.

        Remove-AzureRmVirtualNetworkGatewayConnection -Name MyGWConnectionName `
        -ResourceGroupName MyRGName

2. Upravte hodnotu GatewayIpAddress. Můžete také upravit předpony adres v současné době v případě potřeby. Všimněte si, že tímto přepíšete existující brány nastavení místní sítě. Použití stávající název místní síti brány při úpravách tak, aby nastavení přepíšete. Pokud vytváříte novou bránu místní síti, není změna existující úrovně.

        New-AzureRmLocalNetworkGateway -Name MyLocalNetworkGWName `
        -Location "West US" -AddressPrefix @('10.0.0.0/24','20.0.0.0/24','30.0.0.0/24') `
        -GatewayIpAddress "104.40.81.124" -ResourceGroupName MyRGName

3. Vytvoření připojení. V tomto příkladu jsme konfigurovat typ spojení IPsec. Když znovu připojíte pomocí typ připojení, který není zadán konfiguraci. Typy další připojení najdete na stránce [rutiny prostředí PowerShell](https://msdn.microsoft.com/library/mt603611.aspx) .  K získání názvu VirtualNetworkGateway, můžete spustit `Get-AzureRmVirtualNetworkGateway` rutiny.

    Nastavení proměnných:

        $local = Get-AzureRMLocalNetworkGateway -Name MyLocalNetworkGWName -ResourceGroupName MyRGName `
        $vnetgw = Get-AzureRmVirtualNetworkGateway -Name RMGateway -ResourceGroupName MyRGName

    Vytvoření připojení:
    
        New-AzureRmVirtualNetworkGatewayConnection -Name MyGWConnectionName -ResourceGroupName MyRGName `
        -Location "West US" `
        -VirtualNetworkGateway1 $vnetgw `
        -LocalNetworkGateway2 $local `
        -ConnectionType IPsec -RoutingWeight 10 -SharedKey 'abc123'

