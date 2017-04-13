<properties
    pageTitle="Přiložení disk virtuálního počítače | Microsoft Azure"
    description="Připojte disk dat do virtuálního počítače Windows vytvořené pomocí klasické nasazení modelu a inicializace ho."
    services="virtual-machines-windows, storage"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    editor="tysonn"
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="06/27/2016"
    ms.author="cynthn"/>

# <a name="attach-a-data-disk-to-a-windows-virtual-machine-created-with-the-classic-deployment-model"></a>Připojení dat disku k Windows virtuálního počítače vytvořená pomocí klasické nasazení modelu

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]Pokud chcete používat portál nový, najdete v tématu [připojení dat disku, který chcete OM Windows Azure portálu](virtual-machines-windows-attach-disk-portal.md).

Pokud budete potřebovat další data disk, můžete připojit prázdný disk nebo existující disk s daty angličtině. V obou případech jsou disků VHD soubory, které se nacházejí v účet Azure úložiště. V případě nový disk po připojení disku, taky musíte inicializace, takže je připraven k použití OM Windows.

Další podrobnosti o discích najdete v tématu [o disků a VHD virtuálních počítačích](virtual-machines-windows-about-disks-vhds.md).


[AZURE.INCLUDE [howto-attach-disk-windows-linux](../../includes/howto-attach-disk-windows-linux.md)]

## <a name="initialize-the-disk"></a>Inicializace disku

1. Připojení k virtuální počítač. Pokyny najdete v tématu [jak se přihlásit do virtuálního počítače serverem Windows][logon].

2. Po přihlášení do virtuálního počítače spusťte **Správce serveru**. V levém podokně vyberte **soubor a úložiště služby**.

    ![Otevření správce serveru](./media/virtual-machines-windows-classic-attach-disk/fileandstorageservices.png)

3. Rozbalení nabídky a vyberte **discích**.

4. V části **disků** seznamy discích. Ve většině případů bude mít disk 0, disk 1 a disk 2. 0 je disk operačním systému, 1 jedná dočasné disk a 2 je disk data, která jste připojili jenom angličtině. Nový datový disk zobrazí seznam oddílu jako **Neznámý**. Klikněte pravým tlačítkem myši disku a vyberte **Inicializace**.

5.  Zobrazí se upozornění, že všechna data se vymažou při inicializaci disku. Klikněte na **Ano** vezměte na vědomí upozornění a inicializace disku. Po dokončení Partion vedená jako **GPT**. Klikněte pravým tlačítkem disku a vyberte **Nové hlasitost**.

6.  Dokončete průvodce pomocí výchozích hodnot. Po dokončení průvodce **svazky** části jsou uvedeny nové hlasitost. Disk je teď online a připravená k ukládání dat.

    ![Hlasitost úspěšném obnovení výchozího](./media/virtual-machines-windows-classic-attach-disk/newvolumecreated.png)

> [AZURE.NOTE] Velikost OM Určuje, kolik disků můžete připojit k němu. Podrobnosti najdete v tématu [velikosti virtuálních počítačích](virtual-machines-linux-sizes.md).

## <a name="additional-resources"></a>Další zdroje informací

[Jak se odpojit disk z Windows virtuálního počítače](virtual-machines-windows-classic-detach-disk.md)

[O disků a VHD virtuálních počítačích](virtual-machines-linux-about-disks-vhds.md)

[logon]: virtual-machines-windows-classic-connect-logon.md
