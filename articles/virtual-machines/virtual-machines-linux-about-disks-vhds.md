<properties
    pageTitle="O disků a VHD Linux VMs | Microsoft Azure"
    description="Přečtěte si o základních funkcích disků a VHD pro Linux virtuálních počítačích v Azure."
    services="virtual-machines-linux"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    editor="tysonn"
    tags="azure-resource-manager,azure-service-management"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="06/16/2016"
    ms.author="cynthn"/>

# <a name="about-disks-and-vhds-for-azure-virtual-machines"></a>O disků a VHD Azure virtuálních počítačích

Stejně jako jakékoli jiné počítače virtuálních počítačích v Azure pomocí disků jako místa uložení operační systém, aplikací a data. Všechny Azure virtuálních počítačích mít aspoň dva disků – disk operačního systému Linux (v případě Linux OM) a dočasné disku. Disk operačního systému vytvořit z obrázku a disk operačního systému a obrázek jsou ve skutečnosti virtuální pevných discích (VHD) uložené na účtu Azure úložiště. Virtuálních počítačích také disků může být jeden nebo víc dat, uložených jako VHD. Tento článek je také k dispozici pro [virtuálních počítačích Windows](virtual-machines-windows-about-disks-vhds.md).

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

> [AZURE.NOTE] Pokud máte chvíli, Pomozte nám zlepšit dokumentace OM Linux Azure pomocí tohoto [rychlého průzkumu](https://aka.ms/linuxdocsurvey) vašeho prostředí. Každý odpovědí pomáhá pomáhají vaši práci.

## <a name="operating-system-disk"></a>Operační systém disku

Každý počítač virtuální má jeden připojených operační systém disk. Má registrovaná jako jednotka SATA a textem je označená jako jednotku C: ve výchozím nastavení. Disk kapacitou maximální 1023 gigabajtů (GB). 

##<a name="temporary-disk"></a>Dočasné disku

Dočasné disku se vytvoří automaticky. Na virtuálních počítačích Linux disk je obvykle /dev/sdb a formátované a připevněna k /mnt/resource agenta Azure Linux.

Velikost dočasné disku se liší v závislosti na velikosti virtuálního počítače. Další informace najdete v tématu [velikosti Linux virtuálních počítačích](virtual-machines-linux-sizes.md).

>[AZURE.WARNING] Neukládejte dat na dočasné disku. Poskytuje dočasné úložiště pro aplikace a procesy a je určen pouze uložit data například stránky nebo vyměňovat soubory. 

Další informace o použití dočasné disku Azure najdete v článku [Principy dočasné jednotka na virtuálních počítačích Microsoft Azure](https://blogs.msdn.microsoft.com/mast/2013/12/06/understanding-the-temporary-drive-on-windows-azure-virtual-machines/)

## <a name="data-disk"></a>Disk dat

Disk dat je virtuálního pevného disku, který je připojen k virtuální počítači pro uložení dat aplikace nebo jiná data, které chcete zachovat. Disků dat jsou registrovány jako SCSI jednotky a jsou označené písmeno, které zvolíte.  Každý disk dat kapacitou maximální 1023 GB. Velikost virtuálního počítače určuje, kolik disků dat můžete připojit a typ úložiště můžete hostovat discích.

>[AZURE.NOTE] Podrobné informace o kapacity virtuálních počítačích najdete v článku [velikosti Linux virtuálních počítačích](virtual-machines-linux-sizes.md).

Azure vytvoří operační systém disku při vytváření virtuálního počítače z obrázku. Když použít obrázek, který obsahuje data disků Azure vytvoří také disků dat vytvoří virtuální počítač. V opačném přidáte disků data po vytvoření virtuálního počítače.

Disků dat můžete přidat do virtuálního počítače kdykoli **připojení** disk do virtuálního počítače. Můžete použít virtuálního pevného disku, která jste nahráli nebo zkopírují do svého účtu úložiště, nebo který Azure vytvoří automaticky. Připojení dat disku přidružuje virtuální pevný disk soubor ze svého účtu úložiště virtuálního počítače tak, že zaškrtnete "zapůjčení na virtuální pevný disk tak, aby ho nelze odstranit z úložiště je stále připojen.

## <a name="about-vhds"></a>O virtuálních pevných discích

VHD použité v Azure jsou VHD souborů uložených jako objekty BLOB stránku účtu standardní nebo premium úložiště v Azure. Podrobnosti o objektů BLOB stránky najdete v tématu [Principy blok objektů BLOB a objekty BLOB stránky](https://msdn.microsoft.com/library/ee691964.aspx). Podrobnosti o premium úložiště najdete v tématu [Premium úložiště: výkonné úložiště pro Azure virtuálního počítače úloh](../storage/storage-premium-storage.md).

Azure podporuje formát virtuální pevný disk pevný disk. Pevný formát rozložení logický disk lineárně v souboru, takže tohoto disku Posun X je uložený na objektů blob posunu X. Malé zápatí na konci objektů blob popisuje vlastnosti virtuální pevný disk. Často pevný formát zabírají místo vzhledem k tomu Většina disků velké nepoužitý oblastí v nich. Azure ukládá však VHD soubory ve formátu sparse tak získáte výhody pevné a dynamických disků ve stejnou dobu. Další informace najdete v tématu [Začínáme s virtuální pevných discích](https://technet.microsoft.com/library/dd979539.aspx).

Všechny soubory VHD v Azure, který chcete použít jako zdroj pro vytvoření disků nebo obrázky jsou jen pro čtení. Při vytváření souboru disku nebo obrázku díky Azure kopie souborů VHD. Tyto kopie může být jen pro čtení nebo pro čtení a zápisu, podle toho, jak používat virtuální pevný disk.

Při vytváření virtuálního počítače z obrázek Azure vytvoří disk pro virtuální počítač, který je kopií souboru VHD zdroje. K ochraně proti nechtěným odstraněním, umístí Azure zapůjčenou na všech souborů VHD zdroje, které slouží k vytvoření obrázku, disk operačního systému nebo data disk.

Chcete-li odstranit zdrojový soubor VHD, musíte odebrat zapůjčení odstraněním disku nebo obrázek. Odstranit VHD soubor, který používá virtuálního počítače jako operační systém disk, můžete odstranit virtuální počítač disk operačního systému a zdrojový soubor VHD v celém dokumentu odstraněním virtuálního počítače a odstraňování všech problémů disků. Však odstranění VHD soubor, který je zdrojem dat disku vyžaduje několik kroků v nastavení pořadí – odpojení disku v počítači virtuální odstranit disk a pak odstraňte soubor VHD.

>[AZURE.WARNING] Odstranění zdrojového souboru VHD z úložiště nebo odstranění účtu úložiště, Microsoft nelze obnovit tato data za vás.


## <a name="troubleshooting"></a>Řešení potíží
[AZURE.INCLUDE [virtual-machines-linux-lunzero](../../includes/virtual-machines-linux-lunzero.md)]

## <a name="next-steps"></a>Další kroky

-  [Připojit disk](virtual-machines-linux-add-disk.md) a přidat další úložiště pro vaše OM.
-  [Konfigurace software RAID](virtual-machines-linux-configure-raid.md) redundance.
-  Možnost [zachytit Linux virtuálního počítače](virtual-machines-linux-classic-capture-image.md) , můžete rychle nasadit další VMs.


