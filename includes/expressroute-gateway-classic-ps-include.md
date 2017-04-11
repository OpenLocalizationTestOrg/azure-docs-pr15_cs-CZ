Je nutné vytvořit VNet a podsítě brány poprvé, než začnete pracovat na následující úkoly. Naleznete v článku [Konfigurace virtuální sítě pomocí portálu klasické](../articles/expressroute/expressroute-howto-vnet-portal-classic.md) Další informace.   

## <a name="add-a-gateway"></a>Přidání brány

Vytvoření brány pomocí příkazu dole. Je nutné nahrazovat žádné hodnoty pro vlastní.

    New-AzureVirtualNetworkGateway -VNetName "MyAzureVNET" -GatewayName "ERGateway" -GatewayType Dedicated -GatewaySKU  Standard

## <a name="verify-the-gateway-was-created"></a>Ověřte, jestli že byla vytvořená brány

Příkazem dole ověřit, že byl vytvořen brány. Tento příkaz také načte ID brány, které budete potřebovat pro další akce.

    Get-AzureVirtualNetworkGateway

## <a name="resize-a-gateway"></a>Změna velikosti brány

Existuje celá řada [Skladové jednotky brány](../articles/expressroute/expressroute-about-virtual-network-gateways.md). Tento příkaz můžete kdykoli změnit SKU brány.

>[AZURE.IMPORTANT] Tento příkaz nefunguje UltraPerformance brány. Pokud chcete změnit brána k bráně UltraPerformance, nejprve odebrat existující ExpressRoute brány a vytvořte novou bránu UltraPerformance. Snížit verzi brány z UltraPerformance brány, nejdřív odeberte UltraPerformance brány a vytvořte novou bránu. 

    Resize-AzureVirtualNetworkGateway -GatewayId <Gateway ID> -GatewaySKU HighPerformance

## <a name="remove-a-gateway"></a>Odebrání brány

Pomocí příkazu dole můžete odebrat brány

    Remove-AzureVirtualNetworkGateway -GatewayId <Gateway ID>