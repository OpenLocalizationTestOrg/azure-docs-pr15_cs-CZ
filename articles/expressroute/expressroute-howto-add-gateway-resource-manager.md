<properties
   pageTitle="Přidání VNet brány tak, aby virtuální sítě pro ExpressRoute pomocí Správce zdrojů a prostředí PowerShell | Microsoft Azure"
   description="Tento článek vás provede s přidáním Vnet brány tak, aby již byly vytvořeny VNet správce prostředků pro ExpressRoute"
   documentationCenter="na"
   services="expressroute"
   authors="charwen"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager"/>

<tags 
   ms.service="expressroute"
   ms.devlang="na"
   ms.topic="article" 
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services" 
   ms.date="10/10/2016"
   ms.author="charwen"/>

# <a name="configure-a-virtual-network-gateway-for-expressroute-using-resource-manager-and-powershell"></a>Konfigurace brány virtuální sítě pro ExpressRoute pomocí Správce zdrojů a prostředí PowerShell


> [AZURE.SELECTOR]
- [Prostředí PowerShell – správce](expressroute-howto-add-gateway-resource-manager.md)
- [Prostředí PowerShell – klasické](expressroute-howto-add-gateway-classic.md)


Tento článek vás provede jednotlivými kroky, přidání, změna velikosti a odebrat Brána virtuální sítě (VNet) u stávajících VNet. Kroky pro tuto konfiguraci jsou specifické pro VNets, které byly vytvořené pomocí **Správce prostředků nasazení modelu** a které bude použít v konfiguraci ExpressRoute. 

**Modely Azure nasazení**

[AZURE.INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)] 

## <a name="before-beginning"></a>Před zahájením

Ověřte, že máte nainstalovanou rutiny prostředí PowerShell Azure potřebné pro tuto konfiguraci (1.0.2 nebo novější). Pokud jste nenainstalovali rutin, musíte to udělat před zahájením postup konfigurace. Další informace o instalaci Azure Powershellu najdete v článku [Jak nainstalovat a nakonfigurovat Azure Powershellu](../powershell-install-configure.md).


[AZURE.INCLUDE [expressroute-gateway-rm-ps](../../includes/expressroute-gateway-rm-ps-include.md)]

    
## <a name="next-steps"></a>Další kroky

Po vytvoření brány VNet můžete propojit váš VNet ExpressRoute okruh. Odkaz [Virtuální sítě ExpressRoute obvodu](expressroute-howto-linkvnet-arm.md).
