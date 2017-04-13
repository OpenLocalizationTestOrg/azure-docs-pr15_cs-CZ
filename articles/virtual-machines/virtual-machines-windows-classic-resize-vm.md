<properties
    pageTitle="Změna velikosti klasické OM Windows | Microsoft Azure"
    description="Změna velikosti vytvoření v modelu klasické nasazení pomocí prostředí Powershell Azure Windows virtuálního počítače."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="Drewm3"
    manager="timlt"
    editor=""
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="na"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/19/2016"
    ms.author="drewm"/>


# <a name="resize-a-windows-vm-created-in-the-classic-deployment-model"></a>Změna velikosti Windows OM vytvoření v modelu klasické nasazení

Tento článek popisuje, jak změnit velikost Windows OM, vytvoření v modelu klasické nasazení pomocí prostředí Powershell Azure.

Při rozhodování, možnost změnit velikost virtuálního počítače, existují dva koncepty, které řídí škála rozměrů dostupné velikost virtuální počítač. První koncept je oblast, ve kterém je vaše OM nasazený. Seznam formátů OM v oblasti je na kartě služby Azure oblastí webové stránky. Druhá koncept je fyzické hardware aktuálně hostitelem vaší OM. Pole fyzicky servery hostingu VMs seskupeny v clusterů běžné fyzické hardwaru. Metoda změny velikosti OM se liší v závislosti na Pokud požadovanou velikost nové OM podporuje hardwaru clusteru aktuálně hostingu OM.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]Můžete také [změnit velikost OM vytvoření v modelu nasazení Správce prostředků](virtual-machines-windows-resize-vm.md).


## <a name="add-your-account"></a>Přidání účtu

Musíte nakonfigurovat Azure Powershellu pro práci s klasické Azure zdroje. Postupujte podle návodu pro nastavení Azure Powershellu ke správě klasické prostředků.

1. Na příkazovém řádku prostředí PowerShell zadejte `Add-AzureAccount` a klepněte na **Enter**. 
2. Zadejte e-mailovou adresu přidruženou k vašemu předplatnému Azure a klikněte na **pokračovat**. 
3. Zadejte heslo k vašemu účtu. 
4. Klikněte na **přihlásit**. 


## <a name="resize-in-the-same-hardware-cluster"></a>Změna velikosti ve stejném clusteru hardwaru

Pokud chcete změnit velikost OM k dispozici v hardwaru clusteru hostingu OM velikost, proveďte následující kroky.

1. Spusťte seznam OM velikosti dostupné v hardwaru clusteru hostingu cloudové služby, která obsahuje OM následujícího příkazu Powershellu.

    ```powershell
    Get-AzureService | where {$_.ServiceName -eq "<cloudServiceName>"}
    ```

2. Spusťte následující příkazy změnit velikost OM.

    ```powershell
    Get-AzureVM -ServiceName <cloudServiceName> -Name <vmName> | Set-AzureVMSize -InstanceSize <newVMSize> | Update-AzureVM
    ```

## <a name="resize-on-a-new-hardware-cluster"></a>Změna velikosti nové clusteru hardwaru

Pokud chcete změnit velikost OM velikost není k dispozici v hardwaru clusteru hostingu OM cloudu služby a všechny VMs do cloudové služby je nutné znovu vytvořit. Každý cloudové služby je hostovaný ve jednoho hardwaru obrázku tak, aby všechny VMs ve službě cloud musí mít velikost, který podporuje clusteru hardwaru. Podle těchto kroků se popisují, jak změnit velikost OM odstraněním a pak znovu vytvářeli cloudovou službu.

1. Spusťte seznam velikostí OM dostupných v oblasti následujícího příkazu Powershellu. 

    ```powershell
    Get-AzureLocation | where {$_.Name -eq "<locationName>"}
    ```

2. Poznamenejte si všechna nastavení konfigurace pro každý OM do cloudové služby, která obsahuje OM pro změnu velikosti. 
3. Odstraňte všechny VMs ve službě cloud výběr možnosti zachovat disků pro každou OM.
4. Znova vytvořte OM pro změnu velikosti pomocí požadovanou velikost OM.
5. Znova vytvořte další VMs, které byly ve službě cloud pomocí OM velikost k dispozici v hardwaru clusteru teď hostitelské služby cloudu.

Ukázka skriptu pro odstranění a znovu pomocí nových velikost OM do cloudové služby, najdete [tady](https://github.com/Azure/azure-vm-scripts). 


## <a name="next-steps"></a>Další kroky

- [Změna velikosti OM vytvoření v modelu nasazení Správce prostředků](virtual-machines-windows-resize-vm.md).