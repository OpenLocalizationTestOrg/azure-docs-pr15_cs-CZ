<properties
    pageTitle="Vytvoření obrázku OM z Azure OM | Microsoft Azure"
    description="Naučte se vytvářet generalized OM obrázek z existující OM Azure vytvoření v modelu nasazení Správce prostředků"
    services="virtual-machines-windows"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/10/2016"
    ms.author="cynthn"/>


# <a name="download-the-template-for-a-vm"></a>Stažení šablony pro virtuálního počítače

Když vytvoříte virtuálního počítače v Azure pomocí portálu nebo Powershellu, se automaticky vytvoří šablonu správce prostředků. Pomocí této šablony můžete rychle duplikovat na nasazení. Šablona obsahuje informace o všech zdrojů v skupina zdroje. Pro virtuální počítač to znamená, kontejnerů šablony všechno, co, která se vytvoří o OM v dané skupině zdrojů, včetně sítě zdrojů.

## <a name="download-the-template-using-the-portal"></a>Stažení šablony na portálu

1. Přihlaste se k [portálu Azure](https://portal.azure.com/).
2. Vyberte jednu nabídku centrální **virtuálních počítačích**.
3. Vyberte virtuální počítač ze seznamu.
5. Vyberte **skript automatizace**.
6. Zaškrtněte políčko **stahovat** a soubor ZIP uložit do místního počítače.
7. Otevřete soubor ZIP a extrahujte soubory a složky. Soubor ZIP bude obsahovat:
    
    - Deploy.ps1
    - Deploy.SH 
    - deployer.RB
    - DeploymentHelper.cs
    - Parameters.JSON
    - Template.JSON

Soubor .json je šablona.
    
## <a name="download-the-template-using-powershell"></a>Stažení šablony pomocí prostředí PowerShell

Můžete také stáhnout soubor šablony .json pomocí rutiny [AzureRMResourceGroup exportovat](https://msdn.microsoft.com/library/mt715427.aspx) . Můžete použít `-path` parametr zadejte název souboru a cestu k souboru .json. Tento příklad ukazuje, jak stáhnout šablonu pro skupina zdroje s názvem **myResourceGroup** **C:\users\public\downloads** složky na místním počítači.

```powershell
    Export-AzureRmResourceGroup -ResourceGroupName "myResourceGroup" -Path "C:\users\public\downloads"
```

## <a name="next-steps"></a>Další kroky

Další informace o nasazení zdrojů pomocí šablony, [kroků](../resource-manager-template-walkthrough.md)najdete v článku správce prostředků šablony.