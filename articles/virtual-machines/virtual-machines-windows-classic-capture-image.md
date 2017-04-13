<properties
    pageTitle="Zachyťte libovolný obraz z OM Windows Azure | Microsoft Azure"
    description="Zachyťte libovolný obraz z Azure Windows virtuálního počítače vytvořená pomocí klasické nasazení modelu."
    services="virtual-machines-windows"
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
    ms.date="09/27/2016"
    ms.author="cynthn"/>

#<a name="capture-an-image-of-an-azure-windows-virtual-machine-created-with-the-classic-deployment-model"></a>Zachyťte libovolný obraz z Azure Windows virtuálního počítače vytvořená pomocí klasické nasazení modelu.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]Správce prostředků modelu informace najdete v tématu [Vytvoření kopie Windows OM spuštěné v Azure](virtual-machines-windows-vhd-copy.md).


Tento článek ukazuje, jak zachytit Azure virtuální počítač s Windows, abyste mohli používat ho jako obrázek vytvořit další virtuálních počítačích. Tento obrázek obsahuje disk operačního systému a všechny disků dat připojených k virtuální počítač. Sociální sítě konfigurace neobsahuje, bude potřeba nakonfigurovat můžou být při vytváření virtuálních počítačích, které použít obrázek.

Azure ukládá obrázek v části **Moje obrázky**. Toto je na stejném místě, kde jsou uloženy všechny obrázky, které jste nahráli. Podrobnosti o obrázků najdete v tématu [o obrázky virtuálních počítačích](virtual-machines-linux-classic-about-images.md).

##<a name="before-you-begin"></a>Než začnete##

Tento postup předpokládá, že jste vytvořili Azure virtuálního počítače a nakonfigurovali operačního systému, včetně připojení všech dat discích. Pokud jste to ještě neudělali, přečtěte si tyto pokyny:

- [Vytvoření virtuálního počítače od obrázků.](virtual-machines-windows-classic-createportal.md)
- [Jak připojit disk dat do virtuálního počítače](virtual-machines-windows-classic-attach-disk.md)
- Zkontrolujte, že role serveru jsou podporovány u Sysprep. Další informace najdete v tématu [Sysprep podpora role serveru](https://msdn.microsoft.com/windows/hardware/commercialize/manufacture/desktop/sysprep-support-for-server-roles).

> [AZURE.WARNING] Tento proces odstraní původní virtuálního počítače po se zaznamenává. 

Před caputuring obrázek Azure virtuálního počítače doporučujeme zálohovat cílový virtuální počítač. Azure virtuálních počítačích lze zálohovat zálohování Azure. Podrobnosti najdete v tématu [obecnějším údajům Azure virtuálních počítačích](../backup/backup-azure-vms.md). Další řešení, jsou k dispozici certifikované partnery. Pokud chcete zjistit, co momentálně neexistuje, vyhledejte Azure Marketplace.


##<a name="capture-the-virtual-machine"></a>Zachycení virtuálního počítače

1. V [Azure klasické portál](http://manage.windowsazure.com), **Připojit** k virtuální počítač. Pokyny najdete v tématu [jak se přihlásit do virtuálního počítače serverem Windows] [].

2.  Jako správce otevřete okno příkazového řádku.

3.  Změna adresáře tak, aby `%windir%\system32\sysprep`, a pak spusťte sysprep.exe.

4.  Zobrazí se dialogové okno **Nástroj pro přípravu systému** . Postupujte takto:

    - V **Systému vyčištění akce**vyberte **prostředí mimo pole zadejte systém (počáteční nastavení počítače)** a ujistěte se, že je zaškrtnuté políčko **Generalizace** . Další informace o používání Sysprep najdete v tématu [způsob použití Sysprep: Úvod][].

    - **Ve skupinovém rámečku **Možnosti vypnutí**vyberte položku vypnout.**

    - Klikněte na **OK**.

    ![Spuštění Sysprep](./media/virtual-machines-windows-classic-capture-image/SysprepGeneral.png)

7.  Sysprep vypne virtuálního počítače stav virtuálního počítače v portálu Azure klasické se změní na **Zastavit**.

8.  Na portálu Azure klasické klikněte na **virtuálních počítačích** a vyberte virtuální počítač, který chcete zachytit.

9.  Na řádku nabídek klikněte na tlačítko **zachytit**.

    ![Zachycení virtuálního počítače](./media/virtual-machines-windows-classic-capture-image/CaptureVM.png)

    Zobrazí se dialogové okno **zachytit virtuální počítač** .

10. Do pole **Název**zadejte název pro nový obrázek.

11. Před přidáním systému Windows Server obrázku do sady vlastní obrázky musí být generalized spuštěním Sysprep podle pokynů uvedených v předchozích krocích. Klikněte na tlačítko **zorganizovat Sysprep ve počítače virtuální** označíte, že jste udělali.

12. Klikněte na značku zaškrtnutí zachyťte obrázek. Obrázek novinky je teď k dispozici v části **obrázky**.

    ![Obrázek snímku povedla](./media/virtual-machines-windows-classic-capture-image/VMCapturedImageAvailable.png)

##<a name="next-steps"></a>Další kroky

Obrázek hotovou použít k vytvoření virtuálních počítačích. K tomuto účelu vytvoříte virtuálního počítače pomocí položku **Z Galerie** nabídky a vyberte obrázek, který jste právě vytvořili. Pokyny najdete v tématu [Vytvoření virtuálního počítače z obrázku](virtual-machines-windows-classic-createportal.md).



[Jak se přihlásit do virtuálního počítače spuštěný Windows Server]: virtual-machines-windows-classic-connect-logon.md
[Jak používat Sysprep: Úvod]: http://technet.microsoft.com/library/bb457073.aspx
[Run Sysprep.exe]: ./media/virtual-machines-capture-image-windows-server/SysprepCommand.png
[Enter Sysprep.exe options]: ./media/virtual-machines-windows-classic-capture-image/SysprepGeneral.png
[The virtual machine is stopped]: ./media/virtual-machines-capture-image-windows-server/SysprepStopped.png
[Capture an image of the virtual machine]: ./media/virtual-machines-windows-classic-capture-image/CaptureVM.png
[Enter the image name]: ./media/virtual-machines-capture-image-windows-server/Capture.png
[Image capture successful]: ./media/virtual-machines-capture-image-windows-server/CaptureSuccess.png
[Use the captured image]: ./media/virtual-machines-capture-image-windows-server/MyImagesWindows.png
