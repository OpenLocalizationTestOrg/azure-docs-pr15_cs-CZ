<properties
   pageTitle="Jak se označuje virtuálního počítače | Microsoft Azure"
   description="Další informace o označování virtuálního počítače Windows vytvořené v Azure pomocí Správce prostředků nasazení modelu"
   services="virtual-machines-windows"
   documentationCenter=""
   authors="mmccrory"
   manager="timlt"
   editor="tysonn"
   tags="azure-resource-manager"/>

<tags
   ms.service="virtual-machines-windows"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-windows"
   ms.workload="infrastructure-services"
   ms.date="07/05/2016"
   ms.author="memccror"/>

# <a name="how-to-tag-a-windows-virtual-machine-in-azure"></a>Jak se označuje virtuálního počítače Windows Azure


Tento článek popisuje různé způsoby značku virtuálního počítače Windows Azure prostřednictvím nasazení modelu správce prostředků. Značky jsou páry uživatelem definovaných klíč/hodnota, která mohou být umístěny přímo na zdroj nebo skupina zdroje. Azure v současné době podporuje až 15 značky na zdroj a pole Skupina zdroje. Značky může být umístěny na zdroj při vytváření nebo přidali do existujícího zdroje. Všimněte si, že značky nejsou podporované pro zdroje vytvořili, pomocí Správce prostředků nasazení modelu jenom. Pokud chcete označit Linux virtuálního počítače, zjistěte, [jak označení Linux virtuálního počítače v Azure](virtual-machines-linux-tag.md).

[AZURE.INCLUDE [virtual-machines-common-tag](../../includes/virtual-machines-common-tag.md)]

## <a name="tagging-with-powershell"></a>Označování pomocí prostředí PowerShell

Pokud chcete vytvořit, přidejte a odstraňte značky pomocí prostředí PowerShell, nejprve musíte nastavit [prostředí PowerShell pomocí Správce prostředků Azure][]. Po dokončení instalace, můžete umístit značky na výpočetním, sítě a úložiště zdroje vytváření minimálně zdroje je vytvořený prostřednictvím Powershellu. Tento článek se soustřeďte se na zobrazení a úpravy značky na virtuálních počítačích.

Nejdřív přejděte do virtuálního počítače prostřednictvím `Get-AzureRmVM` rutiny.

        PS C:\> Get-AzureRmVM -ResourceGroupName "MyResourceGroup" -Name "MyTestVM"

Pokud počítač virtuální již obsahuje značky, zobrazí se všechny značky na zdroj:

        Tags : {
                "Application": "MyApp1",
                "Created By": "MyName",
                "Department": "MyDepartment",
                "Environment": "Production"
               }

Pokud chcete přidat značky pomocí prostředí PowerShell, můžete `Set-AzureRmResource` příkaz. Poznámka: při aktualizaci značky pomocí prostředí PowerShell, značky se automaticky aktualizují, jako celek. Takže pokud jedna značka přidáváte zdroj, který už je značky, bude potřebujete zahrnout všechny značky, které chcete umístit na zdroje. Tady je příklad toho, jak přidat další značky k prostředku pomocí rutin prostředí PowerShell.

Tato rutina první nastaví všechny značky, kterým se *MyTestVM* proměnné *$tags* pomocí `Get-AzureRmResource` a `Tags` vlastnost.

        PS C:\> $tags = (Get-AzureRmResource -ResourceGroupName MyResourceGroup -Name MyTestVM).Tags

Druhý příkaz zobrazí značky dané proměnné.

        PS C:\> $tags

        Name        Value
        ----                           -----
        Value       MyDepartment
        Name        Department
        Value       MyApp1
        Name        Application
        Value       MyName
        Name        Created By
        Value       Production
        Name        Environment

Příkaz třetí přidá další značku *$tags* proměnné. Všimněte si **+=** přidání nový klíč/hodnota pár do seznamu *$tags* .

        PS C:\> $tags += @{Name="Location";Value="MyLocation"}

Příkaz čtvrtý nastaví všechny značky definice v proměnné *$tags* pro daný zdroj. V tomto případě je MyTestVM.

        PS C:\> Set-AzureRmResource -ResourceGroupName MyResourceGroup -Name MyTestVM -ResourceType "Microsoft.Compute/VirtualMachines" -Tag $tags

Příkaz pátý zobrazí všechny značky na zdroje. Jak vidíte, *umístění* je teď tato pole definovány jako značka *MyLocation* jako hodnota.

        PS C:\> (Get-AzureRmResource -ResourceGroupName MyResourceGroup -Name MyTestVM).Tags

        Name        Value
        ----                           -----
        Value       MyDepartment
        Name        Department
        Value       MyApp1
        Name        Application
        Value       MyName
        Name        Created By
        Value       Production
        Name        Environment
        Value       MyLocation
        Name        Location

Další informace o označování pomocí prostředí PowerShell, najdete v tématech [Rutiny Azure zdroje][].

[AZURE.INCLUDE [virtual-machines-common-tag-usage](../../includes/virtual-machines-common-tag-usage.md)]

## <a name="next-steps"></a>Další kroky

* Další informace o označování Azure materiály, najdete v článku [Přehled Správce prostředků Azure][] a [Pomocí značek k uspořádání Azure zdroje][].
* Se pokud chcete podívat, jak můžete pomocí značek Správa použití Azure materiály, najdete v článku [Principy faktuře Azure][] a [získat podstatu využití prostředků Microsoft Azure][].

[Prostředí PowerShell pomocí Správce prostředků Azure]: ../powershell-azure-resource-manager.md
[Rutiny pro Azure zdroje]: https://msdn.microsoft.com/library/azure/dn757692.aspx
[Azure Přehled Správce zdrojů]: ../azure-resource-manager/resource-group-overview.md
[Používání značek k uspořádání svých prostředcích Azure]: ../resource-group-using-tags.md
[Principy faktuře Azure]: ../billing/billing-understand-your-bill.md
[Získat další informace na využití prostředků Microsoft Azure]: ../billing-usage-rate-card-overview.md
