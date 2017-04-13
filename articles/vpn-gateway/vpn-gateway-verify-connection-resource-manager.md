<properties
   pageTitle="Ověřte připojení brány | Microsoft Azure"
   description="V tomto článku se dozvíte, jak ověřit připojení brány v modelu nasazení Správce prostředků"
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
   ms.date="10/14/2016"
   ms.author="cherylmc"/>

# <a name="verify-a-gateway-connection"></a>Ověřte připojení brány

Ověřte připojení k bráně několika různými způsoby. Tento článek vám ukáže, jak zkontrolujte stav brány připojení správce prostředků pomocí portálu Azure nebo pomocí Powershellu.


## <a name="verify-using-powershell"></a>Ověření pomocí prostředí PowerShell

Musíte nainstalovat nejnovější verzi rutiny prostředí PowerShell Azure správce prostředků. Informace o instalaci rutiny prostředí PowerShell najdete v tématu [jak instalace a konfigurace prostředí PowerShell Azure](../powershell-install-configure.md). Další informace o používání rutin Správce prostředků najdete v článku [použití Windows pomocí Správce prostředků](../powershell-azure-resource-manager.md).

### <a name="step-1-log-in-to-your-azure-account"></a>Krok 1: Přihlášení k účtu Azure

1. Spusťte konzolu prostředí PowerShell se zvýšenými oprávněními a připojit se ke svému účtu.

        Login-AzureRmAccount

2. Zaškrtněte políčko předplatná pro účet.

        Get-AzureRmSubscription 

3. Určení předplatné, ke které chcete použít.

        Select-AzureRmSubscription -SubscriptionName "Replace_with_your_subscription_name"

### <a name="step-2-verify-your-connection"></a>Krok 2: Ověřte připojení


[AZURE.INCLUDE [verify connection powershell](../../includes/vpn-gateway-verify-connection-ps-rm-include.md)] 


## <a name="verify-using-the-azure-portal"></a>Ověření pomocí portálu Azure

[AZURE.INCLUDE [verify connection portal](../../includes/vpn-gateway-verify-connection-portal-rm-include.md)] 


## <a name="next-steps"></a>Další kroky

- Přidání virtuálních počítačích virtuální sítí. Postup v tématu [Vytvoření virtuálního počítače](../virtual-machines/virtual-machines-windows-hero-tutorial.md) .

