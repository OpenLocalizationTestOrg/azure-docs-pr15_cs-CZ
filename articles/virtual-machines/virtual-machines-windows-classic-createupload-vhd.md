<properties
    pageTitle="Vytvořte a uložte obrázek OM pomocí prostředí Powershell | Microsoft Azure"
    description="Zjistěte, jak vytvořit a nahrát generalized systému Windows Server obrázku (virtuální pevný disk) pomocí klasické nasazení modelu a Azure Powershellu."
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
    ms.date="07/21/2016"
    ms.author="cynthn"/>

# <a name="create-and-upload-a-windows-server-vhd-to-azure"></a>Vytvořte a uložte virtuálního pevného disku serveru Windows Azure

V tomto článku se dozvíte, jak nahrát generalized obrázek OM jako virtuální pevný disk (virtuální pevný disk), abyste ho mohli použít k vytváření virtuálních počítačích. Další informace o disků a VHD v Microsoft Azure najdete v článku [o disků a VHD virtuálních počítačích](virtual-machines-linux-about-disks-vhds.md).


[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]. Můžete taky [Odeslat](virtual-machines-windows-upload-image.md) virtuálního počítače pomocí modelu správce prostředků. 

## <a name="prerequisites"></a>Zjistit předpoklady pro

V tomto článku se předpokládá, že máte:

- **Azure předplatné** – Pokud nemáte, můžete [Otevřít účet Azure zdarma](/pricing/free-trial/?WT.mc_id=A261C142F).

- **[Microsoft Azure PowerShell](../powershell-install-configure.md)** - máte modul služeb Microsoft Azure PowerShell nainstalovali a nakonfigurovali používat předplatného. 

- **A . Virtuální pevný disk soubor** - podporované Windows operační systém uložené v souboru VHD a připojené k virtuálního počítače. Zkontrolujte, pokud role serveru spuštěných virtuální pevný disk podporují tímto programem. Další informace najdete v tématu [Sysprep podpora role serveru](https://msdn.microsoft.com/windows/hardware/commercialize/manufacture/desktop/sysprep-support-for-server-roles).

> [AZURE.IMPORTANT] Formát VHDX není podporován v Microsoft Azure. Disk můžete převést do formátu virtuální pevný disk pomocí Hyper-V správce nebo [rutina převést virtuální pevný disk](http://technet.microsoft.com/library/hh848454.aspx). Podrobnosti najdete v tématu tento [blogpost](http://blogs.msdn.com/b/virtual_pc_guy/archive/2012/10/03/using-powershell-to-convert-a-vhd-to-a-vhdx.aspx).

## <a name="step-1-prep-the-vhd"></a>Krok 1: Příprava virtuální pevný disk 

Před odesláním virtuálního pevného disku Azure, je nutné generalized pomocí nástroje Sysprep. Připraví virtuální pevný disk má být použit jako obrázek. Podrobnosti o Sysprep najdete v tématu [způsob použití Sysprep: Úvod](http://technet.microsoft.com/library/bb457073.aspx). Před spuštěním Sysprep obecnějším údajům OM.

Virtuální počítače, který byl nainstalovaný operační systém postupujte takhle:

1. Přihlaste se k operačního systému.

2. Jako správce otevřete okno příkazového řádku. Přejděte v adresáři **%windir%\system32\sysprep**a znovu spusťte `sysprep.exe`.

    ![Otevřete okno příkazového řádku](./media/virtual-machines-windows-classic-createupload-vhd/sysprep_commandprompt.png)

3.  Zobrazí se dialogové okno **Nástroj pro přípravu systému** .

    ![Zahájení Sysprep](./media/virtual-machines-windows-classic-createupload-vhd/sysprepgeneral.png)

4.  **Nástroj pro přípravu systému**vyberte **Zadejte systém mimo pole prostředí (počáteční nastavení počítače)** a ujistěte se, že je zaškrtnuté políčko **Generalizace** .

5.  **Ve skupinovém rámečku **Možnosti vypnutí**vyberte položku vypnout.**

6.  Klikněte na **OK**.

## <a name="step-2-create-a-storage-account-and-a-container"></a>Krok 2: Vytvoření účtu úložiště a kontejneru

Potřebujete účet úložiště v Azure tak, abyste měli místo a nahrajte soubor VHD. Tento krok ukazuje, jak vytvořit účet, nebo získejte informace, které bude nutné z existujícího účtu. Nahrazení proměnných v &lsaquo; závorky &rsaquo; vlastními informacemi.

1. Přihlášení

        Add-AzureAccount

1. Nastavte Azure předplatné.

        Select-AzureSubscription -SubscriptionName <SubscriptionName> 

2. Vytvoření nového účtu úložiště. Název účtu úložiště musí být v jedinečný, 3 24 znaky. Název může být libovolnou kombinací písmen a číslic. Budete taky muset zadejte umístění, třeba "Východ NAS"
        
        New-AzureStorageAccount –StorageAccountName <StorageAccountName> -Location <Location>

3. Nastavení nového účtu úložiště jako výchozí.
        
        Set-AzureSubscription -CurrentStorageAccountName <StorageAccountName> -SubscriptionName <SubscriptionName>

4. Vytvoření kontejneru nové.

        New-AzureStorageContainer -Name <ContainerName> -Permission Off

 

## <a name="step-3-upload-the-vhd-file"></a>Krok 3: Odeslání souboru VHD

Umožňuje [Přidat AzureVhd](http://msdn.microsoft.com/library/dn495173.aspx) nahrát virtuální pevný disk.

V okně Azure PowerShell jste použili v předchozím kroku, zadejte následující příkaz a nahradit proměnných v &lsaquo; závorky &rsaquo; vlastními informacemi.

        Add-AzureVhd -Destination "https://<StorageAccountName>.blob.core.windows.net/<ContainerName>/<vhdName>.vhd" -LocalFilePath <LocalPathtoVHDFile>


## <a name="step-4-add-the-image-to-your-list-of-custom-images"></a>Krok 4: Přidání obrázku do seznamu vlastní obrázky

Přidání obrázku do seznamu vlastní obrázky získáte pomocí rutiny [AzureVMImage přidat](https://msdn.microsoft.com/library/mt589167.aspx) .

        Add-AzureVMImage -ImageName <ImageName> -MediaLocation "https://<StorageAccountName>.blob.core.windows.net/<ContainerName>/<vhdName>.vhd" -OS "Windows"


## <a name="next-steps"></a>Další kroky

Můžete [vytvořit vlastní OM](virtual-machines-windows-classic-createportal.md) pomocí obrázek, který jste nahráli.

