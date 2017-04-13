<properties
    pageTitle="Příprava Debian virtuální pevný disk Linux | Microsoft Azure"
    description="Naučte se vytvářet Debian 7 a 8 virtuální pevný disk souborů pro nasazení v Azure."
    services="virtual-machines-linux"
    documentationCenter=""
    authors="szarkos"
    manager="timlt"
    editor=""
    tags="azure-resource-manager,azure-service-management"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/24/2016"
    ms.author="szark"/>



# <a name="prepare-a-debian-vhd-for-azure"></a>Příprava Debian virtuálního pevného disku Azure

## <a name="prerequisites"></a>Zjistit předpoklady pro
Tato část se předpokládá, že jste již nainstalovali operační systém Debian Linux ze souboru ISO stahují z [Debian webu](https://www.debian.org/distrib/) do virtuálního pevného disku. Existují více nástroje pro vytváření souborů VHD; Pro Hyper-V je pouze jeden příklad. Pokyny pro Hyper-V použití naleznete v tématu [instalace Hyper-V roli a konfigurace virtuálního počítače](https://technet.microsoft.com/library/hh846766.aspx).


## <a name="installation-notes"></a>Poznámky k instalaci

- Další informace o přípravě Linux Azure také najdete [Obecné poznámky k instalaci Linux](virtual-machines-linux-create-upload-generic.md#general-linux-installation-notes) .
- Novější formát VHDX není podporován v Azure. Disk můžete převést do formátu virtuální pevný disk pomocí Hyper-V správce nebo rutinu **převést virtuální pevný disk** .
- Při instalaci systému Linux se doporučuje používat standardní oddíly spíše než LVM (často výchozí nastavení pro mnoho instalace). To bude konfliktům LVM název s klonovaný VMs, zejména pokud OS disku je třeba připojit k jiné OM návody na řešení. [LVM](virtual-machines-linux-configure-lvm.md) nebo [RAID](virtual-machines-linux-configure-raid.md) může být použít na discích data.
- Konfigurovat oddíl vyměňovat na disku s operačním systémem. Vytvoření souboru vyměňovat na disku dočasné zdroje je možné konfigurovat agenta Azure Linux. Další informace najdete v následujících krocích.
- Všechny VHD musí mít velikosti, jež jsou násobky čísla 1 MB.


## <a name="use-azure-manage-to-create-debian-vhds"></a>Umožňuje spravovat Azure vytvořit Debian VHD

Dostupné jsou nástroje pro vytváření Debian VHD Azure, například [azure – Správa](https://github.com/credativ/azure-manage) skriptů [credativ](http://www.credativ.com/). Toto je doporučený postup srovnání vytvoření obrázek od začátku. Například vytvořit Debian virtuálního pevného disku 8 spusťte následující příkazy ke stažení azure spravovat (a závislosti) a spusťte azure_build_image skript:

    # sudo apt-get update
    # sudo apt-get install git qemu-utils mbr kpartx debootstrap

    # sudo apt-get install python3-pip python3-dateutil python3-cryptography
    # sudo pip3 install azure-storage azure-servicemanagement-legacy azure-common pytest pyyaml
    # git clone https://github.com/credativ/azure-manage.git
    # cd azure-manage
    # sudo pip3 install .

    # sudo azure_build_image --option release=jessie --option image_size_gb=30 --option image_prefix=debian-jessie-azure section


## <a name="manually-prepare-a-debian-vhd"></a>Ručně připravit Debian virtuální pevný disk

1. Ve Správci Hyper-V vyberte virtuální počítač.

2. Klikněte na **Připojit** k otevřete okno konzoly pro virtuální počítač.

3. Komentáře se řádek pro **deb CD-ROM** v `/etc/apt/source.list` Pokud nastavíte OM proti souboru ISO.

4. Upravit `/etc/default/grub` souborů a změnit parametr **GRUB_CMDLINE_LINUX** takto mají být další jádra parametry Azure.

        GRUB_CMDLINE_LINUX="console=tty0 console=ttyS0,115200 earlyprintk=ttyS0,115200 rootdelay=30"

5. Opětovné sestavení grub a spuštění:

        # sudo update-grub

6. Přidání prvku Debian Azure úložištích /etc/apt/sources.list pro Debian 7 nebo 8:

    **Debian 7.x "Wheezy"**

        deb http://debian-archive.trafficmanager.net/debian wheezy-backports main
        deb-src http://debian-archive.trafficmanager.net/debian wheezy-backports main
        deb http://debian-archive.trafficmanager.net/debian-azure wheezy main
        deb-src http://debian-archive.trafficmanager.net/debian-azure wheezy main


    **Debian 8.x "Klára"**

        deb http://debian-archive.trafficmanager.net/debian jessie-backports main
        deb-src http://debian-archive.trafficmanager.net/debian jessie-backports main
        deb http://debian-archive.trafficmanager.net/debian-azure jessie main
        deb-src http://debian-archive.trafficmanager.net/debian-azure jessie main


7. Nainstalujte Azure Linux Agent:

        # sudo apt-get update
        # sudo apt-get install waagent

8. Pro Debian 7 je nutné spustit jádra na základě 3.16 z úložiště wheezy backports. Nejdřív vytvořte soubor s názvem /etc/apt/preferences.d/linux.pref pomocí následujícího obsahu:

        Package: linux-image-amd64 initramfs-tools
        Pin: release n=wheezy-backports
        Pin-Priority: 500

    Potom spusťte "sudo výstižný získání instalace linux obrázek – amd64" nainstalovat novou jádra.

8. Deprovision virtuálního počítače a příprava zřizování na Azure a spuštění:

        # sudo waagent –force -deprovision
        # export HISTSIZE=0
        # logout

9. Klikněte na **Akce** dolů ve Správci Hyper-V-> vypnout. Vaše Linux virtuálního pevného disku je nyní připravena k nahrát Azure.


## <a name="next-steps"></a>Další kroky

Teď jste připraveni vytvořit nové virtuálních počítačích v Azure pomocí Debian virtuální pevném disku. Pokud se jedná poprvé, že jste odesílá soubor VHD Azure, najdete v článku kroky 2 a 3 v [Vytvoření a odeslání virtuálního pevného disku, který obsahuje operační systém Linux](virtual-machines-linux-classic-create-upload-vhd.md).
