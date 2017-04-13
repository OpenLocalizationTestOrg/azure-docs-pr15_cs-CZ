<properties
    pageTitle="Nasazení aplikace na virtuální počítač měřítko sady | Microsoft Azure"
    description="Nasazení aplikace na sady měřítko virtuálního počítače"
    services="virtual-machine-scale-sets"
    documentationCenter=""
    authors="gbowerman"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machine-scale-sets"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/13/2016"
    ms.author="guybo"/>


# <a name="upgrade-a-virtual-machine-scale-set"></a>Upgrade sady měřítko virtuálního počítače

Tento článek popisuje, jak můžete zavedením aktualizaci OS měřítku Azure virtuální počítač nastavit bez jakékoli výpadek služeb. V tomto kontextu aktualizaci OS zahrnuje změně verze nebo SKU operačního systému nebo změně URI vlastní obrázek. Aktualizace bez prostředků prostoj při aktualizaci virtuálních počítačích jeden po druhém nebo ve skupinách (například jednu doménu poruch současně) místo v celém dokumentu. Tímto způsobem můžete ponechat spuštěná virtuálních počítačích, které nejsou probíhá upgrade.

Chcete-li předejít nejednoznačnosti, Pojďme rozlišení tři typy aktualizace operačního systému, který chcete provést:

- Změna verze nebo SKU platformy obrázek. Příklad změny se systémem Ubuntu 14.04.2-LTS verzi z 14.04.201506100 14.04.201507060, nebo změnou 15.10/nejnovější systémem Ubuntu SKU 16.04.0-LTS/latest. Tento scénář je popsán v tomto článku.

- Změna URI odkazující na novou verzi vlastní obrázek se integrované (**Vlastnosti** > **virtualMachineProfile** > **storageProfile** > **osDisk** > **Obrázek** > **uri**). Tento scénář je popsán v tomto článku.

- Oprava OS z uvnitř virtuálního počítače (příkladem instalace opravy zabezpečení a spuštění Windows Update). Tento scénář je podporované, ale není uvedené v tomto článku.

První dvě možnosti jsou podporované požadavky uvedené v tomto článku. Je potřeba vytvořit nové měřítko nastavit provést třetí možnost.

Virtuální počítač měřítko sady nasazené jako součást [Struktury služby Azure](https://azure.microsoft.com/services/service-fabric/) clusteru nezahrnuje tady.

Základní posloupnost pro změnu operační systém verze/SKU platformy obrázku nebo URI vlastní obrázek vypadá takto:

1. Pokud potřebujete modelu sady měřítko virtuálního počítače.

2. Změna verze, SKU nebo identifikátor URI hodnoty v modelu.

3. Aktualizujte modelu.

4. Proveďte *manualUpgrade* volání na virtuálních počítačích v sadě měřítko. Tento krok platí pouze pokud je nastaveno *upgradePolicy* **Ruční** v sadě měřítko. Pokud je nastavena na hodnotu **automaticky**, budou se virtuálních počítačích upgradovat najednou, což způsobuje výpadek služeb.


Pomocí těchto informací pozadí na paměti Podíváme se, jak může aktualizovat verzi měřítko nastavení v prostředí PowerShell a pomocí rozhraní REST API. Tyto příklady nevztahuje na obrázku platformy, ale tento článek obsahuje dost informací můžete přizpůsobit tento proces vlastní obrázek.

## <a name="powershell"></a>Prostředí PowerShell##

V tomto příkladě se aktualizuje měřítko virtuálního počítače Windows nastavena na novou verzi 4.0.20160229. Po aktualizaci modelu, stejně jedna virtuálního počítače instance aktualizace najednou.

```powershell
$rgname = "myrg"
$vmssname = "myvmss"
$newversion = "4.0.20160229"
$instanceid = "1"

# get the VMSS model
$vmss = Get-AzureRmVmss -ResourceGroupName $rgname -VMScaleSetName $vmssname

# set the new version in the model data
$vmss.virtualMachineProfile.storageProfile.imageReference.version = $newversion

# update the virtual machine scale set model
Update-AzureRmVmss -ResourceGroupName $rgname -Name $vmssname -VirtualMachineScaleSet $vmss

# now start updating instances
Update-AzureRmVmssInstance -ResourceGroupName $rgname -VMScaleSetName $vmssname -InstanceId $instanceId
```

Pokud aktualizujete URI vlastní obrázek nemusíte měnit image verze platformy, nahraďte řádku "nastavit novou verzi" nějak takto:

```powershell
# set the new version in the model data
$vmss.virtualMachineProfile.storageProfile.osDisk.image.uri= $newURI
```


## <a name="the-rest-api"></a>Rozhraní REST API

Tady je několik příkladů Python, které umožňuje rozhraní REST API Azure zavedením aktualizace verze operačního systému. Obě pomocí knihovnu lightweight [azurerm](https://pypi.python.org/pypi/azurerm) Azure REST API obálky funkcí můžete provádět získáte na modelu sady měřítko, následovaný umístění s aktualizovanými modelu. Zobrazují se taky ve počítače virtuální instance zobrazení k identifikaci virtuálních počítačích doménou aktualizace.

### <a name="vmssupgrade"></a>Vmssupgrade

 [Vmssupgrade](https://github.com/gbowerman/vmsstools) je, že Python skript, který se používá k zavedením OS upgrade pracovního měřítko virtuálního počítače nastavený jednu doménu aktualizace najednou.

![Skript Vmssupgrade pro výběr virtuálních počítačích nebo aktualizace doménu](./media/virtual-machine-scale-sets-upgrade-scale-set/vmssupgrade-screenshot.png)

Tento skript můžete vybrat konkrétní virtuálních počítačích k aktualizaci nebo zadejte doménu aktualizace. Podporuje změně image verze platformy nebo změně URI vlastní obrázek.

### <a name="vmsseditor"></a>Vmsseditor

[Vmsseditor](https://github.com/gbowerman/vmssdashboard) je obecný editor pro virtuální počítač měřítko sady zobrazující stav virtuálního počítače jako heatmap kde jeden řádek představuje jednu doménu aktualizace. Mimo jiné můžete aktualizovat modelu pro sadu měřítko s novou verzi, SKU nebo vlastní obrázek URI a vyberte poruch domény upgradovat. Po výběru virtuálních počítačích v této doméně aktualizace upgradovat na nový model. Můžete taky můžete udělat inovace na základě velikosti dávku podle svého výběru.  

Následující obrázek ukazuje modelu měřítko pro systémem Ubuntu 14.04-2LTS verze 14.04.201507060. Mnoho dalších možností byly přidány tento nástroj od byla přijata tento snímek.

![Model Vmsseditor rozsahu nastavte pro systémem Ubuntu 14.04-2LTS](./media/virtual-machine-scale-sets-upgrade-scale-set/vmssEditor1.png)

Když kliknete na **upgradovat** a **Zobrazí podrobnosti o**virtuálních počítačích v UD 0 spusťte aktualizovat.

![Vmsseditor zobrazující aktualizace průběhu](./media/virtual-machine-scale-sets-upgrade-scale-set/vmssEditor2.png)
