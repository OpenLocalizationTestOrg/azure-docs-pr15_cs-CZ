<properties
    pageTitle="Přesunutí Windows OM | Microsoft Azure"
    description="Přesunutí OM Windows jiné Azure předplatné nebo skupiny prostředků v modelu nasazení Správce prostředků."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/08/2016"
    ms.author="cynthn"/>

    


# <a name="move-a-windows-vm-to-another-azure-subscription-or-resource-group"></a>Přesunout OM Windows Azure jiné předplatné nebo zdroje skupiny 

Tento článek vás provede přesunutí Windows OM mezi skupinami zdrojů nebo předplatného. Přesouvání mezi předplatná může být užitečné, pokud jste v osobním předplatné původně vytvořili virtuálního počítače a teď chcete přesunout na jiné místo na vaší společnosti předplatné pokračovat v práci.

> [AZURE.NOTE] Jako součást přesunutí se vytvoří nová ID zdroje. Po přesunutí OM musíte aktualizovat nástroje a skripty nové ID prostředků. 


[AZURE.INCLUDE [virtual-machines-common-move-vm](../../includes/virtual-machines-common-move-vm.md)]


## <a name="use-powershell-to-move-a-vm"></a>Přejdete OM pomocí prostředí Powershell

Virtuální počítač přesunout do jiné skupiny zdrojů, budete muset Ujistěte se, musíte přesunout také všechny závislá zdroje. Pokud chcete použít rutinu přesunout AzureRMResource, musíte název zdroje a typ zdroje. Můžete získat z rutinu AzureRMResource najít.

    Find-AzureRMResource -ResourceGroupNameContains "<sourceResourceGroupName>"
    

Chcete-li přesunout OM potřebujeme přesunout více zdrojů. Můžeme jednoduše si ji vytvořte samostatné proměnné pro jednotlivé zdroje a potom seznamu. V tomto příkladu obsahuje většinu základní zdrojů pro virtuálního počítače, ale můžete přidat informace podle potřeby.

    $sourceRG = "<sourceResourceGroupName>"
    $destinationRG = "<destinationResourceGroupName>"
    
    $vm = Get-AzureRmResource -ResourceGroupName $sourceRG -ResourceType "Microsoft.Compute/virtualMachines" -ResourceName "<vmName>"
    $storageAccount = Get-AzureRmResource -ResourceGroupName $sourceRG -ResourceType "Microsoft.Storage/storageAccounts" -ResourceName "<storageAccountName>"
    $diagStorageAccount = Get-AzureRmResource -ResourceGroupName $sourceRG -ResourceType "Microsoft.Storage/storageAccounts" -ResourceName "<diagnosticStorageAccountName>"
    $vNet = Get-AzureRmResource -ResourceGroupName $sourceRG -ResourceType "Microsoft.Network/virtualNetworks" -ResourceName "<vNetName>"
    $nic = Get-AzureRmResource -ResourceGroupName $sourceRG -ResourceType "Microsoft.Network/networkInterfaces" -ResourceName "<nicName>"
    $ip = Get-AzureRmResource -ResourceGroupName $sourceRG -ResourceType "Microsoft.Network/publicIPAddresses" -ResourceName "<ipName>"
    $nsg = Get-AzureRmResource -ResourceGroupName $sourceRG -ResourceType "Microsoft.Network/networkSecurityGroups" -ResourceName "<nsgName>"
    
    Move-AzureRmResource -DestinationResourceGroupName $destinationRG -ResourceId $vm.ResourceId, $storageAccount.ResourceId, $diagStorageAccount.ResourceId, $vNet.ResourceId, $nic.ResourceId, $ip.ResourceId, $nsg.ResourceId

Přejdete prostředky k jiné předplatné zahrňte parametr **- DestinationSubscriptionId** . 

    Move-AzureRmResource -DestinationSubscriptionId "<destinationSubscriptionID>" -DestinationResourceGroupName $destinationRG -ResourceId $vm.ResourceId, $storageAccount.ResourceId, $diagStorageAccount.ResourceId, $vNet.ResourceId, $nic.ResourceId, $ip.ResourceId, $nsg.ResourceId



Zobrazí se výzva k potvrzení, že chcete přesunout zadané zdroje. Typ **Y** potvrdíte, že přejdete zdroje.

  
## <a name="next-steps"></a>Další kroky

Přesunutí mnoha různých typů zdrojů mezi skupinami zdroje a předplatná. Další informace najdete v tématu [přesunutí zdrojů do nové skupiny prostředků nebo předplatného](../resource-group-move-resources.md).    