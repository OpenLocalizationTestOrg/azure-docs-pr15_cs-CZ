<properties
   pageTitle="Změna místní síti brány IP adresu předpony nebo brány | Microsoft Azure"
   description="Tento článek vás provede s Změna předpony IP adres pro místní síti brány"
   services="vpn-gateway"
   documentationCenter="na"
   authors="cherylmc"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager"/>

<tags
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="08/08/2016"
   ms.author="cherylmc"/>

# <a name="modify-local-network-gateway-settings-using-powershell"></a>Úprava nastavení místní sítě brány pomocí prostředí PowerShell

Někdy se změnit nastavení místní sítě brána AddressPrefix nebo GatewayIPAddress. Následující pokyny vám pomůže změnit nastavení místní sítě brány. Můžete také změnit nastavení na portálu Azure.

## <a name="before-you-begin"></a>Než začnete
    
Musíte nainstalovat nejnovější verzi rutiny prostředí PowerShell Azure správce prostředků. Zjistěte, [Jak nainstalovat a nakonfigurovat Azure PowerShell](../powershell-install-configure.md) Další informace o instalaci rutiny prostředí PowerShell.

## <a name="to-modify-ip-address-prefixes"></a>Chcete-li změnit předpony IP adres

[AZURE.INCLUDE [vpn-gateway-modify-ip-prefix-rm](../../includes/vpn-gateway-modify-ip-prefix-rm-include.md)]

## <a name="to-modify-the-gateway-ip-address"></a>Chcete-li změnit IP adresu brány

[AZURE.INCLUDE [vpn-gateway-modify-lng-gateway-ip-rm](../../includes/vpn-gateway-modify-lng-gateway-ip-rm-include.md)]

## <a name="next-steps"></a>Další kroky

Můžete ověřit připojení brány. Přečtěte si téma [ověření připojení brány](vpn-gateway-verify-connection-resource-manager.md).

