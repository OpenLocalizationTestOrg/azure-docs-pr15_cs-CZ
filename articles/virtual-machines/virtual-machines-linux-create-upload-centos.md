<properties
    pageTitle="Vytvořte a uložte na základě CentOS Linux virtuální pevný disk v Azure"
    description="Zjistěte, jak vytvořit a nahrát Azure virtuální pevný disk (virtuální pevný disk), která obsahuje operační systém na základě CentOS Linux."
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
    ms.date="05/09/2016"
    ms.author="szarkos"/>

# <a name="prepare-a-centos-based-virtual-machine-for-azure"></a>Příprava virtuálního počítače CentOS založené na Azure

- [Příprava CentOS 6.x virtuálního počítače Azure](#centos-6x)
- [Příprava virtuálního počítače CentOS 7.0 + Azure](#centos-70)

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

## <a name="prerequisites"></a>Zjistit předpoklady pro ##

V tomto článku se předpokládá, že jste již nainstalovali CentOS (nebo podobné odvozená) operační systém Linux virtuální pevný disk. Existují více nástroje pro vytváření VHD souborů, například řešení virtualizace například Hyper-V. Pokyny najdete v tématu [instalace Hyper-V roli a konfigurace virtuálního počítače](http://technet.microsoft.com/library/hh846766.aspx).


**Poznámky k instalaci centOS**

- Další informace o přípravě Linux Azure také najdete [Obecné poznámky k instalaci Linux](virtual-machines-linux-create-upload-generic.md#general-linux-installation-notes) .

- Formát VHDX není podporován v Azure pouze **pevnou virtuální pevný disk**.  Disk můžete převést do formátu virtuální pevný disk pomocí Hyper-V správce nebo rutinu převést virtuální pevný disk.

- Při instalaci systému Linux se doporučuje používat standardní oddíly spíše než LVM (často výchozí nastavení pro mnoho instalace). To bude konfliktům LVM název s klonovaný VMs, zejména pokud OS disku je třeba připojit k jiné OM návody na řešení.  [LVM](virtual-machines-linux-configure-lvm.md) nebo [RAID](virtual-machines-linux-configure-raid.md) může být použít na discích data.

- NUMA nepodporuje větších OM kvůli chyby v níže uvedených 2.6.37 verzí jádra Linux. Tento problém primárně ovlivňuje distribuce pomocí odesílání dat červené klobouk 2.6.32 jádra. Ruční instalace agenta Azure Linux (waagent) automaticky zakáže NUMA v konfiguraci GRUB jádra Linux. Další informace najdete v následujících krocích.

- Konfigurovat oddíl vyměňovat na disku s operačním systémem. Vytvoření souboru vyměňovat na disku dočasné zdroje je možné konfigurovat agenta Linux.  Další informace najdete v následujících krocích.

- Všechny VHD musí mít velikosti, jež jsou násobky čísla 1 MB.


## <a name="centos-6x"></a>CentOS 6.x ##

1. Ve Správci Hyper-V vyberte virtuální počítač.

2. Klikněte na **Připojit** k otevřete okno konzoly pro virtuální počítač.

3. Odinstalace NetworkManager spuštěním následujícího příkazu:

        # sudo rpm -e --nodeps NetworkManager

    **Poznámka:** Pokud už není nainstalovaná balíček, tento příkaz se nepovede s chybovou zprávou. Očekává se, že.

4.  Vytvoření souboru s názvem **síť** v `/etc/sysconfig/` adresář, který obsahuje následující text:

        NETWORKING=yes
        HOSTNAME=localhost.localdomain

5.  Vytvoření souboru s názvem **ifcfg eth0** v `/etc/sysconfig/network-scripts/` adresář, který obsahuje následující text:

        DEVICE=eth0
        ONBOOT=yes
        BOOTPROTO=dhcp
        TYPE=Ethernet
        USERCTL=no
        PEERDNS=yes
        IPV6INIT=no

6.  Úprava udev pravidla pro omezení generování statické pravidla pro rozhraní Ethernet. Tato pravidla může způsobit potíže při kopírování virtuálního počítače v Microsoft Azure nebo Hyper-V:

        # sudo ln -s /dev/null /etc/udev/rules.d/75-persistent-net-generator.rules
        # sudo rm -f /etc/udev/rules.d/70-persistent-net.rules


7. Zajistěte, aby že služby síťového začnou při spuštění pomocí tohoto příkazu:

        # sudo chkconfig network on


8. **CentOS 6.3 pouze**: instalace ovladačů pro Linux Integration Services (seznam).

    **Důležité: Kroku je pouze platná pro CentOS 6.3 a starší.**  V CentOS 6.4 + jsou *už dostupné ve standardní jádra*služby integrace Linux.

    - Postupujte podle pokynů k instalaci na [seznam stáhnout stránky](https://www.microsoft.com/en-us/download/details.aspx?id=46842) a nainstalujte OTÁČKY na obrázek.  


9. Nainstalujte balíček python pyasn1 spuštěním následujícího příkazu:

        # sudo yum install python-pyasn1

10. Pokud chcete použít, které jsou hostované v Azure datacentrech zrcadlí OpenLogic, nahraďte soubor /etc/yum.repos.d/CentOS-Base.repo následujících úložištích.  Taky Toto přidá **[openlogic]** úložiště, ke kterému patří balíčků Azure Linux agent:

        [openlogic]
        name=CentOS-$releasever - openlogic packages for $basearch
        baseurl=http://olcentgbl.trafficmanager.net/openlogic/$releasever/openlogic/$basearch/
        enabled=1
        gpgcheck=0

        [base]
        name=CentOS-$releasever - Base
        baseurl=http://olcentgbl.trafficmanager.net/centos/$releasever/os/$basearch/
        gpgcheck=1
        gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-6

        #released updates
        [updates]
        name=CentOS-$releasever - Updates
        baseurl=http://olcentgbl.trafficmanager.net/centos/$releasever/updates/$basearch/
        gpgcheck=1
        gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-6

        #additional packages that may be useful
        [extras]
        name=CentOS-$releasever - Extras
        baseurl=http://olcentgbl.trafficmanager.net/centos/$releasever/extras/$basearch/
        gpgcheck=1
        gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-6

        #additional packages that extend functionality of existing packages
        [centosplus]
        name=CentOS-$releasever - Plus
        baseurl=http://olcentgbl.trafficmanager.net/centos/$releasever/centosplus/$basearch/
        gpgcheck=1
        enabled=0
        gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-6

        #contrib - packages by Centos Users
        [contrib]
        name=CentOS-$releasever - Contrib
        baseurl=http://olcentgbl.trafficmanager.net/centos/$releasever/contrib/$basearch/
        gpgcheck=1
        enabled=0
        gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-6

    **Poznámka:** Zbývající část tohoto průvodce se předpokládá, že používáte aspoň repo [openlogic], která se bude používat pro instalaci agenta Azure Linux.


11. Přidejte do /etc/yum.conf následující řádek:

        http_caching=packages

    A **CentOS 6.3 jenom** přidat následující řádek:

        exclude=kernel*

12. Zakázání modulu yum "fastestmirror" úpravou souboru "/ etc/yum/pluginconf.d/fastestmirror.conf" a v části `[main]`, zadejte následující:

        set enabled=0

13. Spusťte tento příkaz zrušte aktuální yum metadat:

        # yum clean all

14. **Na CentOS 6.3 pouze**aktualizovat jádra pomocí následujícího příkazu:

        # sudo yum --disableexcludes=all install kernel

15. Úprava řádku jádra spouštění v konfiguraci grub mají být další jádra parametry Azure. K tomuto účelu otevřete "/ boot/grub/menu.lst" v textovém editoru a ujistěte se, že jádra výchozí zahrnuje následující parametry:

        console=ttyS0 earlyprintk=ttyS0 rootdelay=300 numa=off

    Také zajistíte tak všechny konzoly zprávy se odesílají první sériové port, který může pomoci Azure podporu s ladění problémy. Tímto zakážete NUMA kvůli chyby v jádra verzi, kterou CentOS 6.

    Kromě výše uvedené doporučujeme *Odebrat* následujících parametrů:

        rhgb quiet crashkernel=auto

    Grafické a tichý spouštěcí nejsou užitečné v cloudu prostředí, které chceme všechny protokoly nechat zasílat sériový port.

    `crashkernel` Možnost může být vlevo nakonfigurované podle potřeby, ale Všimněte si, že tento parametr bude Zkraťte dostupnou pamětí v OM tak, že 128MB a více, které mohou způsobovat v menší velikosti OM.


16. Ujistěte se, že SSH server nainstalovali a nakonfigurovali zahájíte při spuštění.  Je to obvykle výchozí.

17. Nainstalujte Azure Linux Agent pomocí tohoto příkazu:

        # sudo yum install WALinuxAgent

    Poznámka: při instalaci balíčku WALinuxAgent odeberete NetworkManager NetworkManager gnome balíčků Pokud nebyly už odebrat podle popisu v kroku 2.

18. Nevytvářejte vyměňovat místa na disku s operačním systémem.

    Azure Linux Agent můžete nakonfigurovat automaticky odkládací prostor pomocí místních zdrojů disku, který je připojen k OM po zajištění služby na Azure. Všimněte si, že disku místních zdrojů je *dočasný* disk a může vyprázdní při poskytování zrušeno OM. Po instalaci Azure Linux Agent (viz předchozím kroku), upravte pomocí následujících parametrů v /etc/waagent.conf řádně podporovat:

        ResourceDisk.Format=y
        ResourceDisk.Filesystem=ext4
        ResourceDisk.MountPoint=/mnt/resource
        ResourceDisk.EnableSwap=y
        ResourceDisk.SwapSizeMB=2048    ## NOTE: set this to whatever you need it to be.

19. Spusťte následující příkazy pro deprovision virtuálního počítače a příprava zřizování na Azure:

        # sudo waagent -force -deprovision
        # export HISTSIZE=0
        # logout

20. Klikněte na tlačítko **Akce -> vypnout dolů** ve Správci Hyper-V. Vaše Linux virtuálního pevného disku je nyní připravena k nahrát Azure.


----------


## <a name="centos-70"></a>CentOS 7.0 + ##

**Změny v CentOS 7 (a podobné deriváty)**

Příprava Azure virtuálního počítače CentOS 7 je velmi podobné CentOS, 6, je však k dispozici několik důležitých rozdílů vhodné poznamenat:

 - Balíček NetworkManager už není v konfliktu s prací agenta Azure Linux. Ve výchozím nastavení je nainstalovaný balíček a doporučujeme, aby se odstraní.
 - GRUB2 teď používá jako výchozí zaváděcím tak postup pro úpravu jádra parametry se takto změnila (viz níže).
 - XFS je teď výchozí systém souborů. Systém souborů ext4 pořád lze podle potřeby.


**Kroky konfigurace**

1. Ve Správci Hyper-V vyberte virtuální počítač.

2. Klikněte na **Připojit** k otevřete okno konzoly pro virtuální počítač.

3.  Vytvoření souboru s názvem **síť** v `/etc/sysconfig/` adresář, který obsahuje následující text:

        NETWORKING=yes
        HOSTNAME=localhost.localdomain

4.  Vytvoření souboru s názvem **ifcfg eth0** v `/etc/sysconfig/network-scripts/` adresář, který obsahuje následující text:

        DEVICE=eth0
        ONBOOT=yes
        BOOTPROTO=dhcp
        TYPE=Ethernet
        USERCTL=no
        PEERDNS=yes
        IPV6INIT=no

6.  Úprava udev pravidla pro omezení generování statické pravidla pro rozhraní Ethernet. Tato pravidla může způsobit potíže při kopírování virtuálního počítače v Microsoft Azure nebo Hyper-V:

        # sudo ln -s /dev/null /etc/udev/rules.d/75-persistent-net-generator.rules

6. Zajistěte, aby že služby síťového začnou při spuštění pomocí tohoto příkazu:

        # sudo chkconfig network on

7. Nainstalujte balíček python pyasn1 spuštěním následujícího příkazu:

        # sudo yum install python-pyasn1

8. Pokud chcete použít, které jsou hostované v Azure datacentrech zrcadlí OpenLogic, nahraďte soubor /etc/yum.repos.d/CentOS-Base.repo následujících úložištích.  Taky Toto přidá **[openlogic]** úložiště, ke kterému patří balíčků Azure Linux agent:

        [openlogic]
        name=CentOS-$releasever - openlogic packages for $basearch
        baseurl=http://olcentgbl.trafficmanager.net/openlogic/$releasever/openlogic/$basearch/
        enabled=1
        gpgcheck=0

        [base]
        name=CentOS-$releasever - Base
        baseurl=http://olcentgbl.trafficmanager.net/centos/$releasever/os/$basearch/
        gpgcheck=1
        gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-7

        #released updates
        [updates]
        name=CentOS-$releasever - Updates
        baseurl=http://olcentgbl.trafficmanager.net/centos/$releasever/updates/$basearch/
        gpgcheck=1
        gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-7

        #additional packages that may be useful
        [extras]
        name=CentOS-$releasever - Extras
        baseurl=http://olcentgbl.trafficmanager.net/centos/$releasever/extras/$basearch/
        gpgcheck=1
        gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-7

        #additional packages that extend functionality of existing packages
        [centosplus]
        name=CentOS-$releasever - Plus
        baseurl=http://olcentgbl.trafficmanager.net/centos/$releasever/centosplus/$basearch/
        gpgcheck=1
        enabled=0
        gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-7

        #contrib - packages by Centos Users
        [contrib]
        name=CentOS-$releasever - Contrib
        baseurl=http://olcentgbl.trafficmanager.net/centos/$releasever/contrib/$basearch/
        gpgcheck=1
        enabled=0
        gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-7



    **Poznámka:** Zbývající část tohoto průvodce se předpokládá, že používáte aspoň repo [openlogic], která se bude používat pro instalaci agenta Azure Linux.

9.  Spusťte tento příkaz zrušte aktuální metadata yum a nainstalujte všechny aktualizace:

        # sudo yum clean all
        # sudo yum -y update

10. Úprava řádku jádra spouštění v konfiguraci grub mají být další jádra parametry Azure. Uděláte to takto: otevřít "/ atd/výchozí/grub" v textovém editoru a úpravy `GRUB_CMDLINE_LINUX` parametru, například:

        GRUB_CMDLINE_LINUX="rootdelay=300 console=ttyS0 earlyprintk=ttyS0 net.ifnames=0"

    Také zajistíte tak všechny konzoly zprávy se odesílají prvnímu sériové portu, které mohou pomoci Azure podporu s ladění problémy. Také vypnou nové CentOS 7 konvence pro nic. Kromě výše uvedené doporučujeme *Odebrat* následujících parametrů:

        rhgb quiet crashkernel=auto

    Grafické a tichý spouštěcí nejsou užitečné v cloudu prostředí, které chceme všechny protokoly nechat zasílat sériový port.

    `crashkernel` Možnost může být vlevo nakonfigurované podle potřeby, ale Všimněte si, že tento parametr bude Zkraťte dostupnou pamětí v OM tak, že 128MB a více, které mohou způsobovat v menší velikosti OM.

11. Po dokončení úprav "/ atd/výchozí/grub" za výše, spusťte tento příkaz nové vytvoření grub konfigurace:

        # sudo grub2-mkconfig -o /boot/grub2/grub.cfg

12. Ujistěte se, že SSH server nainstalovali a nakonfigurovali zahájíte při spuštění.  Je to obvykle výchozí.

13. **Jenom v případě vytváření obrázek z VMWare, VirtualBox nebo KVM:** Přidáte moduly pro Hyper-V do initramfs:

    Úprava `/etc/dracut.conf`, přidání obsahu:

        add_drivers+=”hv_vmbus hv_netvsc hv_storvsc”

    Opětovné sestavení initramfs:

        # dracut –f -v

14. Nainstalujte Azure Linux Agent pomocí tohoto příkazu:

        # sudo yum install WALinuxAgent
        # sudo systemctl enable waagent

15. Nevytvářejte vyměňovat místa na disku s operačním systémem.

    Azure Linux Agent můžete nakonfigurovat automaticky odkládací prostor pomocí místních zdrojů disku, který je připojen k OM po zajištění služby na Azure. Všimněte si, že disku místních zdrojů je *dočasný* disk a může vyprázdní při poskytování zrušeno OM. Po instalaci Azure Linux Agent (viz předchozím kroku), upravte pomocí následujících parametrů v /etc/waagent.conf řádně podporovat:

        ResourceDisk.Format=y
        ResourceDisk.Filesystem=ext4
        ResourceDisk.MountPoint=/mnt/resource
        ResourceDisk.EnableSwap=y
        ResourceDisk.SwapSizeMB=2048    ## NOTE: set this to whatever you need it to be.

16. Spusťte následující příkazy pro deprovision virtuálního počítače a příprava zřizování na Azure:

        # sudo waagent -force -deprovision
        # export HISTSIZE=0
        # logout

17. Klikněte na tlačítko **Akce -> vypnout dolů** ve Správci Hyper-V. Vaše Linux virtuálního pevného disku je nyní připravena k nahrát Azure.

## <a name="next-steps"></a>Další kroky
Teď budete chtít použít k vytvoření nové virtuálních počítačích v Azure virtuální disku CentOS Linux. Pokud se jedná poprvé, že jste odesílá soubor VHD Azure, najdete v článku kroky 2 a 3 v [Vytvoření a odeslání virtuálního pevného disku, který obsahuje operační systém Linux](virtual-machines-linux-classic-create-upload-vhd.md).
