<properties
   pageTitle="Konfigurace brány VNet pro ExpressRoute pomocí prostředí PowerShell | Microsoft Azure"
   description="Konfigurace brány VNet klasické nasazení modelu VNet pomocí prostředí PowerShell ExpressRoute konfigurace."
   documentationCenter="na"
   services="expressroute"
   authors="charwen"
   manager="carmonm"
   editor=""
   tags="azure-service-management"/>
<tags
   ms.service="expressroute"
   ms.devlang="na"
   ms.topic="article" 
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/03/2016"
   ms.author="charwen"/>

# <a name="configure-a-virtual-network-gateway-for-expressroute-using-the-classic-deployment-model-and-powershell"></a>Konfigurace brány virtuální sítě pro ExpressRoute pomocí klasické nasazení modelu a prostředí PowerShell

> [AZURE.SELECTOR]
- [Prostředí PowerShell – správce](expressroute-howto-add-gateway-resource-manager.md)
- [Prostředí PowerShell – klasické](expressroute-howto-add-gateway-classic.md)

Tento článek vás provede jednotlivými kroky, přidání, změna velikosti a odebrat Brána virtuální sítě (VNet) u stávajících VNet. Kroky pro tuto konfiguraci jsou speciálně pro VNets, které byly vytvořené pomocí **klasické nasazení modelu** a které bude použít v konfiguraci ExpressRoute. 

**Modely Azure nasazení**

[AZURE.INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)] 

## <a name="before-beginning"></a>Před zahájením

Ověřte, že máte nainstalovanou rutiny prostředí PowerShell Azure potřebné pro tuto konfiguraci (1.0.2 nebo novější). Pokud jste nenainstalovali rutin, musíte to udělat před zahájením postup konfigurace. Další informace o instalaci Azure Powershellu najdete v článku [Jak nainstalovat a nakonfigurovat Azure Powershellu](../powershell-install-configure.md).


[AZURE.INCLUDE [expressroute-gateway-classic-ps](../../includes/expressroute-gateway-classic-ps-include.md)]

    
## <a name="next-steps"></a>Další kroky

Po vytvoření brány VNet můžete propojit váš VNet ExpressRoute okruh. Odkaz [Virtuální sítě ExpressRoute obvodu](expressroute-howto-linkvnet-classic.md).
