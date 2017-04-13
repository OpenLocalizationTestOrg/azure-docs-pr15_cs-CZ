<properties
    pageTitle="Vytvoření virtuálního počítače v portálu klasické | Microsoft Azure"
    description="Vytvoření virtuálního počítače Windows Azure klasické portálu."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    editor=""
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="cynthn"/>

# <a name="create-a-virtual-machine-running-windows-in-the-azure-classic-portal"></a>Vytvoření virtuálního počítače s Windows Azure klasické portálu

> [AZURE.SELECTOR]
- [Azure klasické portálu](virtual-machines-windows-classic-tutorial.md)
- [Powershellu: Nasazení klasické](virtual-machines-windows-classic-create-powershell.md)

<br>

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]Zjistěte, jak [pomocí těchto kroků pomocí Správce prostředků nasazení modelu](virtual-machines-windows-hero-tutorial.md) pomocí **nového Azure portálu**. 

Tento kurz se dozvíte, jak vytvořit Azure virtuálního počítače (OM) s Windows Azure klasické portálu. Použijeme systému Windows Server obrázek jako příklad, ale, jenom jeden z mnoha obrázky, které nabízí Azure. Všimněte si, že váš obrázek volby závisí vašeho předplatného. Windows desktop obrázky například může být k dispozici pro předplatitele.

V této části se dozvíte, jak používat volbu **Z Galerie** na portálu Azure klasické vytvořit virtuální počítač. Tato možnost nabízí další možnosti konfigurace než možnost **Vytvořit** . Pokud se chcete připojit virtuálního počítače k síti virtuální, je potřeba používat volbu **Z Galerie** .

Můžete taky vytvořit VMs pomocí [vlastní obrázky](virtual-machines-windows-classic-createupload-vhd.md). Informace o těchto i jiné metody, zjistíte, [jakými způsoby vytvoření virtuálního počítače Windows](virtual-machines-windows-creation-choices.md).



## <a name="video-walkthrough"></a>Videa návod

Tady je návod tohoto kurzu.

[AZURE.VIDEO creating-a-windows-vm-on-microsoft-azure-classic-portal]

## <a id="createvirtualmachine"> </a>Vytvořit virtuální počítač

[AZURE.INCLUDE [virtual-machines-create-WindowsVM](../../includes/virtual-machines-create-windowsvm.md)]

## <a name="next-steps"></a>Další kroky

- Zjistěte, jak [vytvořit OM pomocí Správce prostředků nasazení modelu](virtual-machines-windows-hero-tutorial.md) v novém portálu Azure. 

- Přihlaste se do virtuálního počítače. Pokyny najdete v tématu [přihlášení k do virtuálního počítače se systémem Windows Server](virtual-machines-windows-classic-connect-logon.md).

- Připojte disku, který chcete uložit data. Můžete připojit prázdné disků a disků, které obsahují data. Pokyny najdete v tématu [připojení disk dat do virtuálního počítače Windows vytvořené pomocí klasické nasazení modelu](virtual-machines-windows-classic-attach-disk.md).
