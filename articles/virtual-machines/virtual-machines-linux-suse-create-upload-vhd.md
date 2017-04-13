<properties
    pageTitle="Vytvořte a uložte virtuální pevný disk Linux SUSE v Azure"
    description="Zjistěte, jak vytvořit a nahrát Azure virtuální pevný disk (virtuální pevný disk), která obsahuje operační systém SUSE Linux."
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

# <a name="prepare-a-sles-or-opensuse-virtual-machine-for-azure"></a>Příprava virtuálního počítače SLES nebo openSUSE Azure

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

## <a name="prerequisites"></a>Zjistit předpoklady pro ##

V tomto článku se předpokládá, že jste již nainstalovali SUSE nebo openSUSE operační systém Linux virtuální pevný disk. Existují více nástroje pro vytváření VHD souborů, například řešení virtualizace například Hyper-V. Pokyny najdete v tématu [instalace Hyper-V roli a konfigurace virtuálního počítače](http://technet.microsoft.com/library/hh846766.aspx).

### <a name="sles--opensuse-installation-notes"></a>SLES / poznámky k instalaci openSUSE

- Další informace o přípravě Linux Azure také najdete [Obecné poznámky k instalaci Linux](virtual-machines-linux-create-upload-generic.md#general-linux-installation-notes) .

- Formát VHDX není podporován v Azure pouze **pevnou virtuální pevný disk**.  Disk můžete převést do formátu virtuální pevný disk pomocí Hyper-V správce nebo rutinu převést virtuální pevný disk.

- Při instalaci systému Linux se doporučuje používat standardní oddíly spíše než LVM (často výchozí nastavení pro mnoho instalace). To bude konfliktům LVM název s klonovaný VMs, zejména pokud OS disku je třeba připojit k jiné OM návody na řešení. [LVM](virtual-machines-linux-configure-lvm.md) nebo [RAID](virtual-machines-linux-configure-raid.md) může být použít na discích data.

- Konfigurovat oddíl vyměňovat na disku s operačním systémem. Vytvoření souboru vyměňovat na disku dočasné zdroje je možné konfigurovat agenta Linux.  Další informace najdete v následujících krocích.

- Všechny VHD musí mít velikosti, jež jsou násobky čísla 1 MB.


## <a name="use-suse-studio"></a>Použití SUSE Studio
[SUSE Studio](http://www.susestudio.com) můžete snadno vytvářet a spravovat SLES a openSUSE obrázky Azure a Hyper-V. Toto je doporučený postup pro přizpůsobení vlastní SLES a openSUSE obrázky.

Jako alternativu k vytváření vlastních virtuální pevný disk SUSE také publikuje obrázky BYOS (přenést svůj vlastní předplatného) pro SLES na [VMDepot](https://vmdepot.msopentech.com/User/Show?user=1007).


## <a name="prepare-suse-linux-enterprise-server-11-sp4"></a>Příprava SUSE Linux Enterprise Server 11 SP4 ##

1. V prostředním podokně s Hyper-V odpovědi vyberte virtuální počítač.

2. Klikněte na **Připojit** k otevřete okno pro virtuální počítač.

3. Zaregistrujte systému SUSE Linux Enterprise povolit aktualizace stáhnout a nainstalovat balíčků.

4. Aktualizace pomocí nejnovějších aktualizací systému:

        # sudo zypper update

5. Nainstalujte Azure Linux Agent z úložiště SLES:

        # sudo zypper install WALinuxAgent

6. Vrácení se změnami chkconfig Pokud waagent nastavena na "na" a pokud ne, povolení automatické spuštění:
               
        # sudo chkconfig waagent on

7. Zaškrtněte, pokud přihlašovacích údajů waagent je spuštěná a pokud ne, spusťte ho: 

        # sudo service waagent start
                
8. Úprava řádku jádra spouštění v konfiguraci grub mají být další jádra parametry Azure. Pokud chcete tuto otevřete "/ boot/grub/menu.lst" v textovém editoru a ujistěte se, že jádra výchozí zahrnuje následující parametry:

        console=ttyS0 earlyprintk=ttyS0 rootdelay=300

    Zajistíte tím všechny konzoly zprávy se odesílají první sériové port, který může pomoci Azure podporu s ladění problémy.

9. Potvrďte, že /boot/grub/menu.lst /etc/fstab i referenční a disk pomocí jeho UUID (tak, že uuid) místo na disku ID (podle id). 

    Získat disk UUID
    
        # ls /dev/disk/by-uuid/

    Pokud /dev/disk/by-id / je použili, aktualizujte /boot/grub/menu.lst i/atd/fstab s velkými tak, že uuid hodnotou

    Před změnou
    
        root=/dev/disk/by-id/SCSI-xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxxx-part1

    Po změně
    
        root=/dev/disk/by-uuid/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx

10. Úprava udev pravidla pro omezení generování statické pravidla pro rozhraní Ethernet. Tato pravidla může způsobit potíže při kopírování virtuálního počítače v Microsoft Azure nebo Hyper-V:

        # sudo ln -s /dev/null /etc/udev/rules.d/75-persistent-net-generator.rules
        # sudo rm -f /etc/udev/rules.d/70-persistent-net.rules

11. Je doporučený k úpravě souboru "/ atd/sysconfig/sítě/dhcp" a změnit `DHCLIENT_SET_HOSTNAME` parametr následující:

        DHCLIENT_SET_HOSTNAME="no"

12. "/ Atd/sudoers" poznámky nebo odeberte následující řádky, pokud existují:

        Defaults targetpw   # ask for the password of the target user i.e. root
        ALL    ALL=(ALL) ALL   # WARNING! Only use this together with 'Defaults targetpw'!

13. Ujistěte se, že SSH server nainstalovali a nakonfigurovali zahájíte při spuštění.  Je to obvykle výchozí.

14. Nevytvářejte vyměňovat místa na disku s operačním systémem.

    Azure Linux Agent můžete nakonfigurovat automaticky odkládací prostor pomocí místních zdrojů disku, který je připojen k OM po zajištění služby na Azure. Všimněte si, že disku místních zdrojů je *dočasný* disk a může vyprázdní při poskytování zrušeno OM. Po instalaci Azure Linux Agent (viz předchozím kroku), upravte pomocí následujících parametrů v /etc/waagent.conf řádně podporovat:

        ResourceDisk.Format=y
        ResourceDisk.Filesystem=ext4
        ResourceDisk.MountPoint=/mnt/resource
        ResourceDisk.EnableSwap=y
        ResourceDisk.SwapSizeMB=2048    ## NOTE: set this to whatever you need it to be.

15. Spusťte následující příkazy pro deprovision virtuálního počítače a příprava zřizování na Azure:

        # sudo waagent -force -deprovision
        # export HISTSIZE=0
        # logout

16. Klikněte na tlačítko **Akce -> vypnout dolů** ve Správci Hyper-V. Vaše Linux virtuálního pevného disku je nyní připravena k nahrát Azure.


----------

## <a name="prepare-opensuse-131"></a>Příprava openSUSE 13.1 + ##

1. V prostředním podokně s Hyper-V odpovědi vyberte virtuální počítač.

2. Klikněte na **Připojit** k otevřete okno pro virtuální počítač.

3. V prostředí, příkaz "`zypper lr`". Pokud tento příkaz vrátí výstup podobnou následující a pak úložištích nakonfigurované podle očekávání – žádné úpravy je třeba (Všimněte si, že čísla verze se může lišit):

        # | Alias                 | Name                  | Enabled | Refresh
        --+-----------------------+-----------------------+---------+--------
        1 | Cloud:Tools_13.1      | Cloud:Tools_13.1      | Yes     | Yes
        2 | openSUSE_13.1_OSS     | openSUSE_13.1_OSS     | Yes     | Yes
        3 | openSUSE_13.1_Updates | openSUSE_13.1_Updates | Yes     | Yes

    Pokud příkaz vrátí hodnotu "Bez úložištích definované …" použijte následující příkazy přidáte tyto repo:

        # sudo zypper ar -f http://download.opensuse.org/repositories/Cloud:Tools/openSUSE_13.1 Cloud:Tools_13.1
        # sudo zypper ar -f http://download.opensuse.org/distribution/13.1/repo/oss openSUSE_13.1_OSS
        # sudo zypper ar -f http://download.opensuse.org/update/13.1 openSUSE_13.1_Updates

    Pak můžete ověřit databází byly přidány pomocí příkazu "`zypper lr`" znovu. V případě, že jeden úložištích důležité aktualizace není povolená, můžete ji povolte se tento příkaz:

        # sudo zypper mr -e [NUMBER OF REPOSITORY]


4. Aktualizace jádra na nejnovější verzi k dispozici:

        # sudo zypper up kernel-default

    Nebo aktualizaci systému s nejnovějšími aktualizacemi:

        # sudo zypper update

5.  Nainstalujte Azure Linux Agent.

        # sudo zypper install WALinuxAgent

6.  Úprava řádku jádra spouštění v konfiguraci grub mají být další jádra parametry Azure. K tomuto účelu otevřete "/ boot/grub/menu.lst" v textovém editoru a ujistěte se, že jádra výchozí zahrnuje následující parametry:

        console=ttyS0 earlyprintk=ttyS0 rootdelay=300

    Zajistíte tím všechny konzoly zprávy se odesílají první sériové port, který může pomoci Azure podporu s ladění problémy. Kromě toho odeberte následujících parametrů z jádra spouštěcím řádku, pokud existují:

        libata.atapi_enabled=0 reserve=0x1f0,0x8

7.  Je doporučený k úpravě souboru "/ atd/sysconfig/sítě/dhcp" a změnit `DHCLIENT_SET_HOSTNAME` parametr následující:

        DHCLIENT_SET_HOSTNAME="no"

8.  **Důležité:** "/ Atd/sudoers" poznámky nebo odeberte následující řádky, pokud existují:

        Defaults targetpw   # ask for the password of the target user i.e. root
        ALL    ALL=(ALL) ALL   # WARNING! Only use this together with 'Defaults targetpw'!

9.  Ujistěte se, že SSH server nainstalovali a nakonfigurovali zahájíte při spuštění.  Je to obvykle výchozí.

10. Nevytvářejte vyměňovat místa na disku s operačním systémem.

    Azure Linux Agent můžete nakonfigurovat automaticky odkládací prostor pomocí místních zdrojů disku, který je připojen k OM po zajištění služby na Azure. Všimněte si, že disku místních zdrojů je *dočasný* disk a může vyprázdní při poskytování zrušeno OM. Po instalaci Azure Linux Agent (viz předchozím kroku), upravte pomocí následujících parametrů v /etc/waagent.conf řádně podporovat:

        ResourceDisk.Format=y
        ResourceDisk.Filesystem=ext4
        ResourceDisk.MountPoint=/mnt/resource
        ResourceDisk.EnableSwap=y
        ResourceDisk.SwapSizeMB=2048    ## NOTE: set this to whatever you need it to be.

11. Spusťte následující příkazy pro deprovision virtuálního počítače a příprava zřizování na Azure:

        # sudo waagent -force -deprovision
        # export HISTSIZE=0
        # logout

12. Zajistěte, aby že agent Linux Azure spustí při spuštění:

        # sudo systemctl enable waagent.service

13. Klikněte na tlačítko **Akce -> vypnout dolů** ve Správci Hyper-V. Vaše Linux virtuálního pevného disku je nyní připravena k nahrát Azure.

## <a name="next-steps"></a>Další kroky
Teď budete chtít použít k vytvoření nové virtuálních počítačích v Azure virtuální disku SUSE Linux. Pokud se jedná poprvé, že jste odesílá soubor VHD Azure, najdete v článku kroky 2 a 3 v [Vytvoření a odeslání virtuálního pevného disku, který obsahuje operační systém Linux](virtual-machines-linux-classic-create-upload-vhd.md).
