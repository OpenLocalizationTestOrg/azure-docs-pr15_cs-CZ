<properties
    pageTitle="Vytvořte a uložte se systémem Ubuntu Linux virtuální pevný disk v Azure"
    description="Zjistěte, jak vytvořit a nahrát Azure virtuální pevný disk (virtuální pevný disk), která obsahuje operační systém systémem Ubuntu Linux."
    services="virtual-machines-linux"
    documentationCenter=""
    authors="szarkos"
    manager="timlt"
    editor="tysonn"
    tags="azure-resource-manager,azure-service-management"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/24/2016"
    ms.author="szark"/>

# <a name="prepare-an-ubuntu-virtual-machine-for-azure"></a>Příprava virtuální počítač se systémem Ubuntu Azure

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

## <a name="official-ubuntu-cloud-images"></a>Úřední systémem Ubuntu cloudu obrázky
Se systémem Ubuntu teď publikuje oficiální VHD Azure ke stažení na [http://cloud-images.ubuntu.com/](http://cloud-images.ubuntu.com/). Pokud potřebujete k vytvoření specializované obrázek se systémem Ubuntu pro Azure, spíše než pomocí následujícího postupu ruční doporučuje se tyto známé práce VHD a upravit ho podle potřeby. Nejnovější verze obrázku vždy najdete v těchto umístěních:

 - Se systémem Ubuntu 12.04/přesné: [ubuntu-12.04-server-cloudimg-amd64-disk1.vhd.zip](http://cloud-images.ubuntu.com/releases/precise/release/ubuntu-12.04-server-cloudimg-amd64-disk1.vhd.zip)
 - Se systémem Ubuntu 14.04/Trusty: [ubuntu-14.04-server-cloudimg-amd64-disk1.vhd.zip](http://cloud-images.ubuntu.com/releases/trusty/release/ubuntu-14.04-server-cloudimg-amd64-disk1.vhd.zip)
 - Se systémem Ubuntu 16.04/Xenial: [ubuntu-16.04-server-cloudimg-amd64-disk1.vhd.zip](http://cloud-images.ubuntu.com/releases/xenial/release/ubuntu-16.04-server-cloudimg-amd64-disk1.vhd.zip)


## <a name="prerequisites"></a>Zjistit předpoklady pro

V tomto článku se předpokládá, že jste již nainstalovali operační systém systémem Ubuntu Linux virtuální pevný disk. Existují více nástroje pro vytváření VHD souborů, například řešení virtualizace například Hyper-V. Pokyny najdete v tématu [instalace Hyper-V roli a konfigurace virtuálního počítače](http://technet.microsoft.com/library/hh846766.aspx).

**Poznámky k instalaci se systémem Ubuntu**

- Další informace o přípravě Linux Azure také najdete [Obecné poznámky k instalaci Linux](virtual-machines-linux-create-upload-generic.md#general-linux-installation-notes) .
- Formát VHDX není podporován v Azure pouze **pevnou virtuální pevný disk**.  Disk můžete převést do formátu virtuální pevný disk pomocí Hyper-V správce nebo rutinu převést virtuální pevný disk.
- Při instalaci systému Linux se doporučuje používat standardní oddíly spíše než LVM (často výchozí nastavení pro mnoho instalace). To bude konfliktům LVM název s klonovaný VMs, zejména pokud OS disku je třeba připojit k jiné OM návody na řešení. [LVM](virtual-machines-linux-configure-lvm.md) nebo [RAID](virtual-machines-linux-configure-raid.md) může být použít na discích data.
- Konfigurovat oddíl vyměňovat na disku s operačním systémem. Vytvoření souboru vyměňovat na disku dočasné zdroje je možné konfigurovat agenta Linux.  Další informace najdete v následujících krocích.
- Všechny VHD musí mít velikosti, jež jsou násobky čísla 1 MB.


## <a name="manual-steps"></a>Ruční

> [AZURE.NOTE] Před vytvořením vlastní obrázek se systémem Ubuntu pro Azure, zvažte místo toho použít obrázky z [http://cloud-images.ubuntu.com/](http://cloud-images.ubuntu.com/) .


1. V prostředním podokně s Hyper-V odpovědi vyberte virtuální počítač.

2. Klikněte na **Připojit** k otevřete okno pro virtuální počítač.

3.  Nahraďte aktuální úložištích v obraze pomocí Azure repo se systémem Ubuntu společnosti. Kroky mírně lišit v závislosti na verzi se systémem Ubuntu.

    Než začnete upravovat /etc/apt/sources.list, se doporučuje vytvořit zálohu:

        # sudo cp /etc/apt/sources.list /etc/apt/sources.list.bak

    Se systémem Ubuntu 12.04:

        # sudo sed -i "s/[a-z][a-z].archive.ubuntu.com/azure.archive.ubuntu.com/g" /etc/apt/sources.list
        # sudo apt-get update

    Se systémem Ubuntu 14.04:

        # sudo sed -i "s/[a-z][a-z].archive.ubuntu.com/azure.archive.ubuntu.com/g" /etc/apt/sources.list
        # sudo apt-get update

4. Obrázky se systémem Ubuntu Azure jsou teď sledovat jádra *hardwaru oprávnění* (HWE). Aktualizace operačního systému do nejnovější jádra spuštěním následující příkazy:

    Se systémem Ubuntu 12.04:

        # sudo apt-get update
        # sudo apt-get install linux-image-generic-lts-trusty linux-cloud-tools-generic-lts-trusty
        # sudo apt-get install hv-kvp-daemon-init
        (recommended) sudo apt-get dist-upgrade

        # sudo reboot

    Se systémem Ubuntu 14.04:

        # sudo apt-get update
        # sudo apt-get install linux-image-virtual-lts-vivid linux-lts-vivid-tools-common
        # sudo apt-get install hv-kvp-daemon-init
        (recommended) sudo apt-get dist-upgrade

        # sudo reboot


5. Úprava řádku jádra spouštěcí pro Grub mají být další jádra parametry Azure. Můžete to udělat otevřít "/ atd/výchozí/grub" v textovém editoru, najděte proměnná s názvem `GRUB_CMDLINE_LINUX_DEFAULT` (nebo ho můžete přidat v případě potřeby) a upravte tak, aby obsahovala následujících parametrů:

        GRUB_CMDLINE_LINUX_DEFAULT="console=tty1 console=ttyS0,115200n8 earlyprintk=ttyS0,115200 rootdelay=300"

    Uložte soubor zavřít a znovu spusťte "`sudo update-grub`". Zajistíte, že všechny konzoly zprávy se odesílají první sériové port, který může pomoci Azure technickou podporu s ladění problémy.

6.  Ujistěte se, že SSH server nainstalovali a nakonfigurovali zahájíte při spuštění.  Je to obvykle výchozí.

7.  Nainstalujte Azure Linux Agent:

        # sudo apt-get update
        # sudo apt-get install walinuxagent

    Všimněte si, že instalace `walinuxagent` balíčku odeberete `NetworkManager` a `NetworkManager-gnome` balíčky, pokud jsou nainstalovány.

8.  Spusťte následující příkazy pro deprovision virtuálního počítače a příprava zřizování na Azure:

        # sudo waagent -force -deprovision
        # export HISTSIZE=0
        # logout

9. Klikněte na tlačítko **Akce -> vypnout dolů** ve Správci Hyper-V. Vaše Linux virtuálního pevného disku je nyní připravena k nahrát Azure.


## <a name="next-steps"></a>Další kroky
Teď budete chtít použít k vytvoření nové virtuálních počítačích v Azure virtuální disku se systémem Ubuntu Linux. Pokud se jedná poprvé, že jste odesílá soubor VHD Azure, najdete v článku kroky 2 a 3 v [Vytvoření a odeslání virtuálního pevného disku, který obsahuje operační systém Linux](virtual-machines-linux-classic-create-upload-vhd.md).

## <a name="references"></a>Odkazy ##

Jádra oprávnění (HWE) se systémem Ubuntu hardwaru:

- [http://blog.utlemming.org/2015/01/Ubuntu-1404-Azure-Images-Now-Tracking.HTML](http://blog.utlemming.org/2015/01/ubuntu-1404-azure-images-now-tracking.html)
- [http://blog.utlemming.org/2015/02/1204-Azure-Cloud-Images-Now-Using-hwe.HTML](http://blog.utlemming.org/2015/02/1204-azure-cloud-images-now-using-hwe.html)
