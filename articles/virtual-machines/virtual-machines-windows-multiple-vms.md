<properties
    pageTitle="Vytvoření více virtuálních počítačích | Microsoft Azure"
    description="Možnosti pro vytváření více virtuálních počítačích v systému Windows"
    services="virtual-machines-windows"
    documentationCenter=""
    authors="gbowerman"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/25/2016"
    ms.author="guybo"/>

# <a name="create-multiple-azure-virtual-machines"></a>Vytvoření více Azure virtuálních počítačích

Existuje mnoha případech, kdy je potřeba vytvořit velkého počtu podobné virtuálních počítačích (VMs). Příklady výkonné výpočetní HPC (), analýza velkých dat, scalable často příslušnosti vícevrstvé nebo back-end servery a (například webservers) a distribuované databází.

Tento článek popisuje dostupné možnosti pro vytváření více VMs Azure. Tyto možnosti dokonalejší jednoduché případy kde ručně vytvořit řadu VMs. Pokud chcete vytvořit mnoho VMs, není procesů, které obvykle používají měřítko i v případě, že je potřeba vytvořit více než hrstku VMs.

Jedním ze způsobů vytvoření hodně podobný VMs je použití konstrukce správce prostředků Azure z _cykly zdroje_.

## <a name="resource-loops"></a>Smyčky zdroje

Smyčky zdroje jsou syntaktických zkrácený v správce prostředků Azure šablony. Smyčky zdroje můžete vytvořit sadu podobně nakonfigurované prostředků ve smyčce. Zdroje smyčky slouží k vytváření diagramů více účtů úložiště, síťových rozhraní, virtuálních počítačích. Další informace o zdroje smyčky najdete v příručce [vytvořit VMs v dostupnosti nastaví pomocí smyčky zdroje](https://azure.microsoft.com/documentation/templates/201-vm-copy-index-loops/).

## <a name="challenges-of-scale"></a>Problémy při stupnice

Ačkoli zdroje smyčky usnadnit její vytvoření cloudové infrastruktury ve velkém měřítku a předložit stručnější šablony, zůstávají určité problémy. Například pokud použijete smyčku zdroje k vytvoření 100 virtuálních počítačích, musíte sladit sítě rozhraní (NIC) s účty úložiště a odpovídající VMs. Protože počet VMs je pravděpodobně liší od čísla účtů úložiště, budete muset příliš zacházet s velikostí smyčka různých zdrojů. Toto jsou solvable problémy, ale zvyšuje složitost výrazně s měřítkem.

Ověřovací kód programu jiné dochází, když potřebujete infrastrukturu, která upraví elastically. Můžete infrastrukturu automatické měřítko, která automaticky zvýší nebo sníží počet VMs odpověď zátěží na projektu. VMs neposkytnete integrované mechanismus měnit číslo (měřítko a velkém měřítku v). Pokud změníte velikost v odebráním VMs, je těžké zajistily dostupnost a ujistěte se, že jsou VMs rovnoměrně mezi aktualizace a poruch domény.

Nakonec při použití smyčku zdroje více volání, abyste vytvořili zdroje přejděte podkladového struktury. Vytvoření více volání stejné prostředky, má Azure implicitní možnost po tomto návrhu zlepšení a optimalizace nasazení spolehlivosti a výkonu. Je to, kde _virtuálního počítače měřítko nastaví_ příchod.

## <a name="virtual-machine-scale-sets"></a>Virtuální počítač měřítko sady

Virtuální počítač měřítko sady jsou zdroje Azure výpočet nasazovat a spravovat sady identickými VMs. S všechny VMs nakonfigurované stejné, měřítko OM, který se snadno změnit velikost v a rozšiřování sady. Jednoduše změnit počet VMs v sadě. Můžete taky nakonfigurovat OM měřítko sady a automatické měřítko na základě požadavků pracovní zátěž.

U aplikací, které je potřeba měřítko pro využití prostředků a jsou operace měřítko implicitně rovnoměrně mezi poruch a aktualizace domény.

Místo korelace více zdroje, jako jsou nic a VMs, obsahuje sady měřítko OM sítě, úložiště, virtuálního počítače a rozšíření vlastnosti, které je možné konfigurovat centrálně.

Úvod k sadám měřítko OM v příručce [virtuálního počítače měřítko nastaví stránky produktu](https://azure.microsoft.com/services/virtual-machine-scale-sets/). Podrobnější informace přejděte na [virtuálních počítačích měřítko nastaví si přečtěte následující dokumentaci](https://azure.microsoft.com/documentation/services/virtual-machine-scale-sets/).
