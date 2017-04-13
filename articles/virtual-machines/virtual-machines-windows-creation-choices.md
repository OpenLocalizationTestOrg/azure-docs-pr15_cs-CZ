<properties
    pageTitle="Různé způsoby vytváření OM Windows | Microsoft Azure"
    description="Seznam různých způsobů vytvoření Windows virtuálního počítače pomocí Správce prostředků."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows"
    ms.workload="infrastructure-services"
    ms.date="09/27/2016"
    ms.author="cynthn"/>

# <a name="different-ways-to-create-a-windows-virtual-machine-with-resource-manager"></a>Různé způsoby vytvoření Windows virtuálního počítače pomocí Správce prostředků

Azure nabízí různé způsoby vytvoření virtuálního počítače, protože virtuálních počítačích jsou vhodné pro účely a jednotlivým uživatelům. To znamená, že potřebujete udělat několik možností o virtuálního počítače a jak ho vytvořit. Tento článek poskytuje přehled těchto možností a odkazy na pokyny.

## <a name="azure-portal"></a>Azure portálu

Pomocí portálu Azure je jednoduchý způsob, jak vyzkoušet virtuálního počítače, zejména pokud právě začínáte s Azure. 

[Vytvoření virtuálního počítače s Windows, na portálu](virtual-machines-windows-hero-tutorial.md)

## <a name="template"></a>Šablony

Virtuálních počítačích vyžadují kombinaci zdrojů (například dostupnost sady a úložiště účty). Místo zavádění a zvlášť správu jednotlivé zdroje, můžete vytvořit šablonu správce prostředků Azure, která nasazuje a zřídí všechny zdroje v operaci jediné, koordinovaný.

- [Vytvoření virtuálního počítače Windows se šablonou správce prostředků](virtual-machines-windows-ps-template.md)


## <a name="azure-powershell"></a>Azure Powershellu

Pokud chcete pracovat v příkazového prostředí, můžete Azure PowerShell.

- [Vytvoření Windows OM pomocí prostředí PowerShell](virtual-machines-windows-ps-create.md)


## <a name="visual-studio"></a>Visual Studio

Pomocí aplikace Visual Studio vytvářet a spravovat nasaďte VMs pomocí nástrojů Azure Visual Studia a Azure SDK.

[Azure Tools for Visual Studio](https://www.visualstudio.com/features/azure-tools-vs)

