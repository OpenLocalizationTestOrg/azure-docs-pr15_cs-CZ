
Abyste mohli nakonfigurovat zařízení VPN, musíte mít veřejnou IP adresu brány virtuální sítě ke konfiguraci sítě VPN zařízení místní. Práce s výrobce zařízení si pro informace o konfiguraci závislé a nakonfigurujte zařízení. Podívejte se do [Počítače v síti VPN](../articles/vpn-gateway/vpn-gateway-about-vpn-devices.md) Další informace o VPN zařízení, které dobře fungují s Azure.

Při hledání veřejnou IP adresu vaší virtuální sítě brány pomocí prostředí PowerShell, použijte v následujícím příkladu:

    Get-AzureRmPublicIpAddress -Name GW1PublicIP -ResourceGroupName TestRG

Můžete taky zobrazit na veřejnou IP adresu virtuální sítě bránu pro pomocí portálu Azure. Přejděte na **bran virtuální sítě**a potom klikněte na název brány.