<properties
    pageTitle="Připojení dat disku Linux OM | Microsoft Azure"
    description="Jak připojit nové nebo existující data disku Linux OM na portálu Azure pomocí nasazení modelu správce prostředků."
    services="virtual-machines-linux"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/06/2016"
    ms.author="cynthn"/>

# <a name="how-to-attach-a-data-disk-to-a-linux-vm-in-the-azure-portal"></a>Jak připojit disk dat Linux OM na portálu Azure

V tomto článku se dozvíte, jak připojit novým i dosavadním disků k počítači virtuální Linux prostřednictvím portálu Azure. Také se můžete [Připojit disku dat, který chcete OM Windows Azure portálu](virtual-machines-windows-attach-disk-portal.md). Než to uděláte, přečtěte si tyto tipy:

- Velikost virtuálního počítače řídí kolik disků dat můžete připojit. Podrobnosti najdete v tématu [velikosti virtuálních počítačích](virtual-machines-linux-sizes.md).
- Použití úložiště Premium, musíte mít virtuálního počítače DS řady nebo GS řady. Disků z Premium a standardní úložiště účtů můžete pomocí těchto virtuálních počítačích. Premium úložiště je dostupné v některých oblastech. Podrobnosti najdete v tématu [Premium úložiště: výkonné úložiště pro Azure virtuálního počítače úloh](../storage/storage-premium-storage.md).
- Disků připojené k virtuálních počítačích jsou skutečně VHD soubory uložené ve účet Azure úložiště. Podrobnosti najdete v tématu [o disků a VHD virtuálních počítačích](virtual-machines-linux-about-disks-vhds.md).
- Pro nový disk nemusíte udělat ho nejdřív, protože Azure vytvoří se po připojení.
- Pro existující disk musí být souboru VHD Azure úložiště účet dostupné. VHD, která už je můžete použít, pokud není připojený k jiné virtuálního počítače nebo nahrát vlastní VHD zařaďte do účtu úložiště.


[AZURE.INCLUDE [virtual-machines-common-attach-disk-portal](../../includes/virtual-machines-common-attach-disk-portal.md)]

## <a name="next-steps"></a>Další kroky

Po přidání disku, budete muset připravte pro použití. Podívejte se v počítači virtuální operační systém: "jak: Inicializace nového disku dat v Linux" v tomto [článku](virtual-machines-linux-classic-attach-disk.md#how-to-initialize-a-new-data-disk-in-linux).
