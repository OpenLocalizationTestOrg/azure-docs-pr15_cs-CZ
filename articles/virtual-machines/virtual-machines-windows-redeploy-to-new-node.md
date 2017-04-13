<properties 
    pageTitle="Přeinstalujte virtuálních počítačích Windows | Microsoft Azure" 
    description="Popisuje, jak přeinstalujte virtuálních počítačích Windows zmírnit problémy s připojením RDP." 
    services="virtual-machines-windows" 
    documentationCenter="virtual-machines" 
    authors="iainfoulds" 
    manager="timlt"
    tags="azure-resource-manager,top-support-issue" 
/>
    

<tags 
    ms.service="virtual-machines-windows" 
    ms.devlang="na" 
    ms.topic="support-article" 
    ms.tgt_pltfrm="vm-windows"
    ms.workload="infrastructure" 
    ms.date="09/19/2016" 
    ms.author="iainfou" 
/>


# <a name="redeploy-virtual-machine-to-new-azure-node"></a>Přeinstalujte virtuální počítač nový Azure uzel

Pokud máte byla protilehlé potíže poradce při potížích s vzdálené plochy připojení nebo aplikace přístup k serveru s Windows Azure virtuálního počítače (OM) znovu nasazení OM pomohou. Když přeinstalujte virtuálního počítače slouží k přesunutí OM nový uzel v rámci Azure infrastruktury a zajišťuje ho znovu zapněte možnosti konfigurace a přidružené prostředky. Tento článek ukazuje, jak přeinstalujte OM pomocí prostředí PowerShell Azure nebo portálu Azure.

> [AZURE.NOTE] Po přeinstalujte virtuálního počítače dočasné disku budou ztracena a dynamické IP adresy přidružené k rozhraní virtuální sítě se automaticky aktualizují. 

## <a name="using-azure-powershell"></a>Pomocí prostředí PowerShell Azure

Zkontrolujte, jestli nejnovější Azure PowerShell 1.x nainstalované v počítači. Další informace najdete v tématu [instalace a konfigurace prostředí PowerShell Azure](../powershell-install-configure.md).

Tímto příkazem prostředí PowerShell Azure přeinstalujte virtuálního počítače:

    Set-AzureRmVM -Redeploy -ResourceGroupName $rgname -Name $vmname 


[AZURE.INCLUDE [virtual-machines-common-redeploy-to-new-node](../../includes/virtual-machines-common-redeploy-to-new-node.md)]


## <a name="next-steps"></a>Další kroky
Pokud máte problémy s připojením k vaší OM, můžete najít nápovědu pro [řešení potíží s připojení RDP](virtual-machines-windows-troubleshoot-rdp-connection.md) nebo [RDP podrobné návody na řešení potíží](virtual-machines-windows-detailed-troubleshoot-rdp.md). Pokud nemáte přístup ke spuštění aplikace v vaší OM, si můžete přečíst [řešíte potíže s aplikací](virtual-machines-windows-troubleshoot-app-connection.md).