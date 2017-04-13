<properties
    pageTitle="Přesunutí Linux OM | Microsoft Azure"
    description="Přesunutí OM Linux jiné Azure předplatné nebo skupiny prostředků v modelu nasazení Správce prostředků."
    services="virtual-machines-linux"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/08/2016"
    ms.author="cynthn"/>

    


# <a name="move-a-linux-vm-to-another-subscription-or-resource-group"></a>Přesunutí Linux OM do jiné skupiny předplatného nebo zdrojů

Tento článek vás provede přesunutí Linux OM mezi skupinami zdrojů nebo předplatného. Přesunutí virtuálního počítače mezi předplatných můžete být užitečné, pokud jste vytvořili virtuálního počítače v osobní předplatné a teď chcete přesunout na jiné místo k předplatnému vaší společnosti.

> [AZURE.NOTE] Nové ID zdroje vytvářejí jako součást přesunout. Po přesunutí OM budete muset aktualizovat nástroje a skripty nové ID prostředků. 


## <a name="use-the-azure-cli-to-move-a-vm"></a>Přejdete OM pomocí rozhraní příkazového řádku Azure 

Úspěšně přesunout OM, budete muset přesunout OM a jeho podpůrné materiály. Pomocí příkazu **azure skupiny zobrazit** seznam všech zdrojů v skupiny zdrojů a jejich ID. Je dobré kanálu výstup tohoto příkazu do souboru, takže můžete zkopírovat a vložit ID na pozdější příkazy.

    azure group show <resourceGroupName>

Přesunout do jiné skupiny zdrojů virtuálního počítače a jeho zdroje, pomocí příkazu **Přesunout azure zdroje** rozhraní příkazového řádku. Následující příklad ukazuje, jak přesunout virtuálního počítače a nejběžnější prostředky, které se musí. Použít parametr **– i** jsme předat v seznamu oddělenými čárkami (bez mezer) k přesunutí ID pro zdroje.

    
    vm=/subscriptions/<sourceSubscriptionID>/resourceGroups/<sourceResourceGroup>/providers/Microsoft.Compute/virtualMachines/<vmName>
    nic=/subscriptions/<sourceSubscriptionID>/resourceGroups/<sourceResourceGroup>/providers/Microsoft.Network/networkInterfaces/<nicName>
    nsg=/subscriptions/<sourceSubscriptionID>/resourceGroups/<sourceResourceGroup>/providers/Microsoft.Network/networkSecurityGroups/<nsgName>
    pip=/subscriptions/<sourceSubscriptionID>/resourceGroups/<sourceResourceGroup>/providers/Microsoft.Network/publicIPAddresses/<publicIPName>
    vnet=/subscriptions/<sourceSubscriptionID>/resourceGroups/<sourceResourceGroup>/providers/Microsoft.Network/virtualNetworks/<vnetName>
    diag=/subscriptions/<sourceSubscriptionID>/resourceGroups/<sourceResourceGroup>/providers/Microsoft.Storage/storageAccounts/<diagnosticStorageAccountName>
    storage=/subscriptions/<sourceSubscriptionID>/resourceGroups/<sourceResourceGroup>/providers/Microsoft.Storage/storageAccounts/<storageAcountName>      
    
    azure resource move --ids $vm,$nic,$nsg,$pip,$vnet,$storage,$diag -d "<destinationResourceGroup>"
    
Pokud chcete přesunout na jiné předplatné OM a jeho zdroje, přidejte **– cíl subscriptionId #60; destinationSubscriptionID & #62;** parametr můžete určit cílový předplatného.

Pokud pracujete z příkazového řádku na počítači s Windows, budete muset přidat **$** před názvech proměnných, když je deklarovat. Toto není potřeba v Linux.

Zobrazí se výzva k potvrzení, že chcete přesunout je daný zdroj. Typ **Y** potvrdíte, že přejdete zdroje.
    

[AZURE.INCLUDE [virtual-machines-common-move-vm](../../includes/virtual-machines-common-move-vm.md)]

## <a name="next-steps"></a>Další kroky

Přesunutí mnoha různých typů zdrojů mezi skupinami zdroje a předplatná. Další informace najdete v tématu [přesunutí zdrojů do nové skupiny prostředků nebo předplatného](../resource-group-move-resources.md).    