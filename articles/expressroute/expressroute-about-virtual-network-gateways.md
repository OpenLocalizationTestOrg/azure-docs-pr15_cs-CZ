<properties 
   pageTitle="Informace o ExpressRoute virtuální sítě bran | Microsoft Azure"
   description="Další informace o virtuální sítě bran ExpressRoute."
   services="expressroute"
   documentationCenter="na"
   authors="cherylmc"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager, azure-service-management"/>
<tags 
   ms.service="expressroute"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/03/2016"
   ms.author="cherylmc" />

# <a name="about-virtual-network-gateways-for-expressroute"></a>Informace o bran virtuální sítě pro ExpressRoute


Virtuální sítě brány použitý k odeslání síťová komunikace mezi Azure virtuálních sítí a využití místního umístění. Při konfiguraci připojení ExpressRoute musí vytvořit a nakonfigurujte bránu virtuální sítě a virtuální síťové připojení brány.

Když vytvoříte Brána virtuální sítě, zadáte několik nastavení. Požadovaná nastavení určuje, zda brány se použije pro přenos ExpressRoute nebo VPN na webu. Správce prostředků nasazení modelu, nastavena na hodnotu "-GatewayType".

Po odeslání v síti vyhrazené soukromé připojení použijete typu brána "ExpressRoute". Tím se taky nazývá ExpressRoute brány. Pokud v síti přenášena jsou zašifrované veřejné Internet, použijte typ brány "Vpn". Takto označujeme jako brána VPN. Webu na webu, čárky webu a VNet VNet připojení všech používat bránu VPN. 

Každý virtuální sítě může mít jenom jednu bránu virtuální sítě podle typu brány. Můžete třeba mít jedné sítě virtuální bránu využívající - GatewayType Vpn a ten, který používá - GatewayType ExpressRoute. Tento článek se zaměřuje na brány ExpressRoute virtuální sítě.

## <a name="gwsku"></a>Skladové jednotky brány

[AZURE.INCLUDE [expressroute-gwsku-include](../../includes/expressroute-gwsku-include.md)]

Pokud chcete upgradovat vaše výkonnější brány SKU, ve většině případů, můžete použít rutinu Powershellu "Změny velikosti – AzureRmVirtualNetworkGateway". To fungovalo upgradech do standardních a vysoce skladové jednotky. Upgradovat na UltraPerformance SKU, musíte ale obnovit bránu.

###  <a name="aggthroughput"></a>Pole Předpokládaná agregační výkon brána SKU


Následující tabulka zobrazuje typy brány a odhadovaná agregační výkon. V této tabulce platí pro správce zdrojů a modelů klasické nasazení.

[AZURE.INCLUDE [expressroute-table-aggthroughput](../../includes/expressroute-table-aggtput-include.md)] 


## <a name="resources"></a>Rutiny pro rozhraní REST API a prostředí PowerShell

Další technické prostředky a požadavky na zvláštní syntaxi při používání rozhraní REST API a rutinách Powershellu pro konfiguraci brány virtuální sítě najdete v tématu na následujících stránkách:

|**Klasický** | **Správce prostředků**|
|-----|----|
|[Prostředí PowerShell](https://msdn.microsoft.com/library/mt270335.aspx)|[Prostředí PowerShell](https://msdn.microsoft.com/library/mt163510.aspx)|
|[ROZHRANÍ REST API](https://msdn.microsoft.com/library/jj154113.aspx)|[ROZHRANÍ REST API](https://msdn.microsoft.com/library/mt163859.aspx)|


## <a name="next-steps"></a>Další kroky

Další informace o konfiguraci k dispozici připojení naleznete v tématu [Přehled ExpressRoute](expressroute-introduction.md) . 







 
