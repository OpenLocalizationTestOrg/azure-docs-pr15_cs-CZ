### <a name="to-verify-your-connection-by-using-powershell"></a>Chcete-li ověřit připojíte pomocí prostředí PowerShell

Můžete ověřit úspěšnou připojíte pomocí `Get-AzureRmVirtualNetworkGatewayConnection` rutina pomocí hesla nebo bez `-Debug`. 

1. Pomocí rutiny příklad konfigurace hodnoty, které odpovídají vlastní. Pokud se zobrazí výzva, aby spouštět "všechny vyberte"A". V příkladu `-Name` odkazuje na název připojení, které jste vytvořili a chcete testovat.

        Get-AzureRmVirtualNetworkGatewayConnection -Name MyGWConnection -ResourceGroupName MyRG

2. Po dokončení rutinu zobrazení hodnot. V následujícím příkladu stav připojení se zobrazí jako "Připojit", uvidíte průniku a výstupním bajtů.

        Body:
        {
          "name": "MyGWConnection",
          "id":
        "/subscriptions/086cfaa0-0d1d-4b1c-94544-f8e3da2a0c7789/resourceGroups/MyRG/providers/Microsoft.Network/connections/MyGWConnection",
          "properties": {
            "provisioningState": "Succeeded",
            "resourceGuid": "1c484f82-23ec-47e2-8cd8-231107450446b",
            "virtualNetworkGateway1": {
              "id":
        "/subscriptions/086cfaa0-0d1d-4b1c-94544-f8e3da2a0c7789/resourceGroups/MyRG/providers/Microsoft.Network/virtualNetworkGa
        teways/vnetgw1"
            },
            "localNetworkGateway2": {
              "id":
        "/subscriptions/086cfaa0-0d1d-4b1c-94544-f8e3da2a0c7789/resourceGroups/MyRG/providers/Microsoft.Network/localNetworkGate
        ways/LocalSite"
            },
            "connectionType": "IPsec",
            "routingWeight": 10,
            "sharedKey": "abc123",
            "connectionStatus": "Connected",
            "ingressBytesTransferred": 33509044,
            "egressBytesTransferred": 4142431
          }

### <a name="to-verify-your-connection-by-using-the-azure-portal"></a>Pro ověření vaší připojení pomocí portálu Azure

Na portálu Azure můžete zobrazit stav připojení tak, že přejdete na připojení. Můžete to udělat několika způsoby. Jedním ze způsobů přejděte na připojení a ověřte následující postup.

1. [Azure portal](http://portal.azure.com)klikněte na **všechny zdroje** a přejděte na brány virtuální sítě.
2. Na zásuvné brány virtuální sítě klikněte na **připojení**. Můžete zobrazit stav každé připojení.
3. Klikněte na název připojení, které chcete ověřit otevřete **Essentials**. V Essentials můžete zobrazit další informace o připojení. **Stav** je "Byl úspěšný" a "Připojení" po úspěšném připojení.

    ![Ověřte připojení](./media/vpn-gateway-verify-connection-rm-include/connectionsucceeded.png)