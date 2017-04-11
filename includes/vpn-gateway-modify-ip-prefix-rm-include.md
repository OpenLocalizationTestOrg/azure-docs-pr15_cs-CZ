### <a name="noconnection"></a>Postup přidání nebo odebrání předpony – bez připojení brány

- **Přidání** předpony další adresa na místní sítě brány, kterou jste vytvořili, ale, který není dosud brány připojení, použijte v příkladu níže. Ujistěte se, pokud chcete změnit hodnoty do vlastního.

        $local = Get-AzureRmLocalNetworkGateway -Name MyLocalNetworkGWName -ResourceGroupName MyRGName `
        Set-AzureRmLocalNetworkGateway -LocalNetworkGateway $local `
        -AddressPrefix @('10.0.0.0/24','20.0.0.0/24','30.0.0.0/24')

- **Chcete-li odebrat** předponu adresy z místní síti brány, který nemá připojení VPN, použijte v příkladu níže. Nechte se předpony, které už nepotřebujete. V tomto příkladu jsme už není potřeba předpona 20.0.0.0/24 (z předchozího příkladu), takže budeme aktualizovat místní sítě brány a vyloučit tuto předponu.

        $local = Get-AzureRmLocalNetworkGateway -Name MyLocalNetworkGWName -ResourceGroupName MyRGName `
        Set-AzureRmLocalNetworkGateway -LocalNetworkGateway $local `
        -AddressPrefix @('10.0.0.0/24','30.0.0.0/24')

### <a name="withconnection"></a>Postup přidání nebo odebrání předpony - existující připojení brány

Pokud jste vytvořili připojení brány a chcete přidat nebo odebrat předpony IP adres obsažené v místní síti brány, musíte udělat tyto kroky v pořadí. Výsledkem bude některé prostoje pro připojení k síti VPN. Při aktualizaci vaší předpony, budete nejprve zrušit spojení, upravte předpony a pak vytvořit nové připojení. V příkladech dál je potřeba změnit hodnoty na vlastní.

>[AZURE.IMPORTANT] Neodstraňujte Brána VPN. Pokud uděláte, budete muset znovu projděte kroky začínat, jakož i překonfigurovat směrovači místní nové nastavení.
 
1. Odeberte připojení.

        Remove-AzureRmVirtualNetworkGatewayConnection -Name MyGWConnectionName -ResourceGroupName MyRGName

2. Úprava předpony adres brány místní síti.

    Nastavte proměnnou pro LocalNetworkGateway.

        $local = Get-AzureRmLocalNetworkGateway -Name MyLocalNetworkGWName -ResourceGroupName MyRGName

    Úprava předpony.

        Set-AzureRmLocalNetworkGateway -LocalNetworkGateway $local `
        -AddressPrefix @('10.0.0.0/24','20.0.0.0/24','30.0.0.0/24')

4. Vytvoření připojení. V tomto příkladu jsme konfigurovat typ spojení IPsec. Když znovu připojíte pomocí typ připojení, který není zadán konfiguraci. Typy další připojení najdete na stránce [rutiny prostředí PowerShell](https://msdn.microsoft.com/library/mt603611.aspx) .

    Nastavte proměnnou pro VirtualNetworkGateway.

        $gateway1 = Get-AzureRmVirtualNetworkGateway -Name RMGateway  -ResourceGroupName MyRGName

    Vytvoření připojení. Všimněte si, že v tomto příkladu proměnnou $local nastavené v předchozím kroku.


        New-AzureRmVirtualNetworkGatewayConnection -Name MyGWConnectionName `
        -ResourceGroupName MyRGName -Location 'West US' `
        -VirtualNetworkGateway1 $gateway1 -LocalNetworkGateway2 $local `
        -ConnectionType IPsec `
        -RoutingWeight 10 -SharedKey 'abc123'
