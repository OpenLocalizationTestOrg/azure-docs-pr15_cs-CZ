<properties
 pageTitle="Rozšíření virtuálního počítače a funkce | Microsoft Azure"
 description="Zjistěte, jaké rozšíření umožňující Azure virtuálních počítačích seskupené podle funkcí, které budou poskytují nebo vylepšit."
 services="virtual-machines-windows"
 documentationCenter=""
 authors="neilpeterson"
 manager="timlt"
 editor=""
 tags="azure-service-management,azure-resource-manager"/>

<tags
 ms.service="virtual-machines-windows"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="vm-windows"
 ms.workload="infrastructure-services"
 ms.date="09/30/2016"
 ms.author="nepeters"/>

# <a name="about-virtual-machine-extensions-and-features"></a>O funkcích a rozšíření virtuálního počítače

## <a name="azure-vm-extensions"></a>Rozšíření Azure OM

Malé aplikace, které obsahují příspěvek nasazení konfigurace a automatizace úkolů na virtuálních počítačích Azure jsou Azure rozšíření virtuálního počítače. Například pokud virtuální počítač vyžaduje software je nainstalovaný, antivirové ochrany nebo Docker konfigurace, příponu OM mohou sloužit k dokončení těchto úkolů. Azure rozšíření OM spuštěním pomocí Azure rozhraní příkazového řádku, Powershellu, spravovat zdroje šablony a portálu Azure. Rozšíření můžete součástí nového nasazení virtuálního počítače nebo spustí hledání stávající systém.

Tento dokument obsahuje požadavky pro rozšíření Azure virtuálního počítače a pokyny o tom, jak zjistit dostupná OM rozšíření. 

## <a name="azure-vm-agent"></a>Agent Azure OM

Agent OM Azure spravuje interakce mezi Azure virtuální počítač a zařízení struktury Azure. Agent OM je zodpovědný za funkční aspekty nasazení a správu Azure virtuálních počítačích, včetně spuštění OM rozšíření. Agent OM Azure je předinstalovaný v Azure Galerie obrázků a je možné nainstalovat na podporované operační systémy. 

Informace o podporované operační systémy a pokyny k instalaci najdete v tématu [Agent Azure virtuálního počítače](./virtual-machines-windows-classic-agents-and-extensions.md).

## <a name="discover-vm-extensions"></a>Seznamte se s OM rozšíření

Mnoho různých rozšíření OM jsou k dispozici pro použití s Azure virtuálních počítačích. Pokud chcete zobrazit úplný seznam, spusťte tento příkaz s Azure rozhraní příkazového řádku, nahrazení umístění umístění podle výběru.

```none
Get-AzureVMAvailableExtension | Select ExtensionName, Version
```

<br />

## <a name="common-vm-extensions"></a>Běžné OM rozšíření

|Název rozšíření   |Popis   |Další informace   |
|---|---|---|
|Rozšíření vlastní skript pro Windows  | Spustit skripty proti Azure virtuálního počítače  |[Rozšíření vlastní skript pro Windows](./virtual-machines-windows-extensions-customscript.md)   |
|DSC rozšíření pro Windows | Rozšíření prostředí PowerShell DSC (požadovaný stav konfigurace).  | [Rozšíření docker OM](./virtual-machines-windows-extensions-dsc-overview.md)  |
|Rozšíření Azure diagnostiky | Správa Azure diagnostiky |[Rozšíření Azure diagnostiky](https://azure.microsoft.com/blog/windows-azure-virtual-machine-monitoring-with-wad-extension/) |
