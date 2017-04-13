<properties
    pageTitle="Vytvořte a uložte virtuální pevný disk Oracle Linux | Microsoft Azure"
    description="Zjistěte, jak vytvořit a nahrát Azure virtuální pevný disk (virtuální pevný disk), která obsahuje operační systém Oracle Linux."
    services="virtual-machines-linux"
    documentationCenter=""
    authors="szarkos"
    manager="timlt"
    editor="tysonn"
    tags="azure-service-management,azure-resource-manager" />

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/24/2016"
    ms.author="szark"/>

# <a name="prepare-an-oracle-linux-virtual-machine-for-azure"></a>Příprava virtuálního počítače Oracle Linux Azure

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

## <a name="prerequisites"></a>Zjistit předpoklady pro ##

V tomto článku se předpokládá, že jste již nainstalovali operační systém Oracle Linux virtuální pevný disk. Existují více nástroje pro vytváření VHD souborů, například řešení virtualizace například Hyper-V. Pokyny najdete v tématu [instalace Hyper-V roli a konfigurace virtuálního počítače](http://technet.microsoft.com/library/hh846766.aspx).


### <a name="oracle-linux-installation-notes"></a>Poznámky k instalaci Linux Oracle

- Další informace o přípravě Linux Azure také najdete [Obecné poznámky k instalaci Linux](virtual-machines-linux-create-upload-generic.md#general-linux-installation-notes) .

- Oracle červené klobouk kompatibilní jádra a jejich UEK3 (nerozbitného Enterprise jádra) jsou podporovány na Hyper-V a Azure. Nejlepších výsledků dosáhnete nezapomeňte aktualizovat na nejnovější jádra při přípravě vašeho virtuální pevný disk Oracle Linux.

- Oracle UEK2 nepodporuje Hyper-V a Azure jako nezahrnuje požadované ovladače.

- Formát VHDX není podporován v Azure pouze **pevnou virtuální pevný disk**.  Disk můžete převést do formátu virtuální pevný disk pomocí Hyper-V správce nebo rutinu převést virtuální pevný disk.

- Při instalaci systému Linux se doporučuje používat standardní oddíly spíše než LVM (často výchozí nastavení pro mnoho instalace). To bude konfliktům LVM název s klonovaný VMs, zejména pokud OS disku je třeba připojit k jiné OM návody na řešení. [LVM](virtual-machines-linux-configure-lvm.md) nebo [RAID](virtual-machines-linux-configure-raid.md) může být použít na discích data.

- NUMA nepodporuje větších OM kvůli chyby v níže uvedených 2.6.37 verzí jádra Linux. Tento problém primárně ovlivňuje distribuce pomocí odesílání dat červené klobouk 2.6.32 jádra. Ruční instalace agenta Azure Linux (waagent) automaticky zakáže NUMA v konfiguraci GRUB jádra Linux. Další informace najdete v následujících krocích.

- Konfigurovat oddíl vyměňovat na disku s operačním systémem. Vytvoření souboru vyměňovat na disku dočasné zdroje je možné konfigurovat agenta Linux.  Další informace najdete v následujících krocích.

- Všechny VHD musí mít velikosti, jež jsou násobky čísla 1 MB.

- Ujistěte se, že `Addons` úložiště je povolený. Upravte soubor `/etc/yum.repo.d/public-yum-ol6.repo`(Oracle Linux 6) nebo `/etc/yum.repo.d/public-yum-ol7.repo`(Oracle Linux) a změnit plnou čáru `enabled=0` k `enabled=1` v části **[ol6_addons]** a **[ol7_addons]** v tomto souboru.


## <a name="oracle-linux-64"></a>Oracle Linux 6.4 + ##

Musíte nejdřív udělat konkrétní kroků v operačním systému virtuální počítač běží v Azure.

1. V prostředním podokně s Hyper-V odpovědi vyberte virtuální počítač.

2. Klikněte na **Připojit** k otevřete okno pro virtuální počítač.

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

        # chkconfig network on

8. Nainstalujte python pyasn1 spuštěním následujícího příkazu:

        # sudo yum install python-pyasn1

9.  Úprava řádku jádra spouštění v konfiguraci grub mají být další jádra parametry Azure. Pokud chcete tuto otevřete "/ boot/grub/menu.lst" v textovém editoru a ujistěte se, že jádra výchozí zahrnuje následující parametry:

        console=ttyS0 earlyprintk=ttyS0 rootdelay=300 numa=off

    Také zajistíte tak všechny konzoly zprávy se odesílají první sériové port, který může pomoci Azure podporu s ladění problémy. Tímto zakážete NUMA kvůli chyby v kompatibilní jádra červené klobouk Oracle.

    Kromě výše uvedené doporučujeme *Odebrat* následujících parametrů:

        rhgb quiet crashkernel=auto

    Grafické a tichý spouštěcí nejsou užitečné v cloudu prostředí, které chceme všechny protokoly nechat zasílat sériový port.

    `crashkernel` Možnost může být vlevo nakonfigurované podle potřeby, ale Všimněte si, že tento parametr bude Zkraťte dostupnou pamětí v OM tak, že 128MB a více, které mohou způsobovat v menší velikosti OM.


10. Ujistěte se, že SSH server nainstalovali a nakonfigurovali zahájíte při spuštění.  Je to obvykle výchozí.

11. Spuštěním následujícího příkazu nainstalujte agenta Azure Linux. Nejnovější verze je 2.0.15.

        # sudo yum install WALinuxAgent

    Poznámka: při instalaci balíčku WALinuxAgent odeberete NetworkManager NetworkManager gnome balíčků Pokud nebyly už odebrat podle popisu v kroku 2.

12. Nevytvářejte vyměňovat místa na disku s operačním systémem.

    Azure Linux Agent můžete nakonfigurovat automaticky odkládací prostor pomocí místních zdrojů disku, který je připojen k OM po zajištění služby na Azure. Všimněte si, že disku místních zdrojů je *dočasný* disk a může vyprázdní při poskytování zrušeno OM. Po instalaci Azure Linux Agent (viz předchozím kroku), upravte pomocí následujících parametrů v /etc/waagent.conf řádně podporovat:

        ResourceDisk.Format=y
        ResourceDisk.Filesystem=ext4
        ResourceDisk.MountPoint=/mnt/resource
        ResourceDisk.EnableSwap=y
        ResourceDisk.SwapSizeMB=2048    ## NOTE: set this to whatever you need it to be.

13. Spusťte následující příkazy pro deprovision virtuálního počítače a příprava zřizování na Azure:

        # sudo waagent -force -deprovision
        # export HISTSIZE=0
        # logout

14. Klikněte na tlačítko **Akce -> vypnout dolů** ve Správci Hyper-V. Vaše Linux virtuálního pevného disku je nyní připravena k nahrát Azure.


----------


## <a name="oracle-linux-70"></a>Oracle Linux 7.0 + ##

**Změny v Oracle Linux 7**

Příprava Azure virtuálního počítače Oracle Linux 7 je velmi podobné 6 Linux Oracle, je však k dispozici několik důležitých rozdílů vhodné poznamenat:

 - Kompatibilní jádra červené klobouk a Oracle UEK3 podporují služby Azure.  Je vhodné jádra UEK3.
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

5.  Úprava udev pravidla pro omezení generování statické pravidla pro rozhraní Ethernet. Tato pravidla může způsobit potíže při kopírování virtuálního počítače v Microsoft Azure nebo Hyper-V:

        # sudo ln -s /dev/null /etc/udev/rules.d/75-persistent-net-generator.rules

6. Zajistěte, aby že služby síťového začnou při spuštění pomocí tohoto příkazu:

        # sudo chkconfig network on

7. Nainstalujte balíček python pyasn1 spuštěním následujícího příkazu:

        # sudo yum install python-pyasn1

8.  Spusťte tento příkaz zrušte aktuální metadata yum a nainstalujte všechny aktualizace:

        # sudo yum clean all
        # sudo yum -y update

9.  Úprava řádku jádra spouštění v konfiguraci grub mají být další jádra parametry Azure. Můžete to udělat otevřít "/ atd/výchozí/grub" v textovém editoru a úpravy `GRUB_CMDLINE_LINUX` parametru, například:

        GRUB_CMDLINE_LINUX="rootdelay=300 console=ttyS0 earlyprintk=ttyS0 net.ifnames=0"

    Také zajistíte tak všechny konzoly zprávy se odesílají první sériové port, který může pomoci Azure podporu s ladění problémy. Také vypnou nové OEL 7 konvence pro nic. Kromě výše uvedené doporučujeme *Odebrat* následujících parametrů:

        rhgb quiet crashkernel=auto

    Grafické a tichý spouštěcí nejsou užitečné v cloudu prostředí, které chceme všechny protokoly nechat zasílat sériový port.

    `crashkernel` Možnost může být vlevo nakonfigurované podle potřeby, ale Všimněte si, že tento parametr bude Zkraťte dostupnou pamětí v OM tak, že 128MB a více, které mohou způsobovat v menší velikosti OM.


10. Po dokončení úprav "/ atd/výchozí/grub" za výše, spusťte tento příkaz nové vytvoření grub konfigurace:

        # sudo grub2-mkconfig -o /boot/grub2/grub.cfg

11. Ujistěte se, že SSH server nainstalovali a nakonfigurovali zahájíte při spuštění.  Je to obvykle výchozí.

12. Nainstalujte Azure Linux Agent pomocí tohoto příkazu:

        # sudo yum install WALinuxAgent
        # sudo systemctl enable waagent

13. Nevytvářejte vyměňovat místa na disku s operačním systémem.

    Azure Linux Agent můžete nakonfigurovat automaticky odkládací prostor pomocí místních zdrojů disku, který je připojen k OM po zajištění služby na Azure. Všimněte si, že disku místních zdrojů je *dočasný* disk a může vyprázdní při poskytování zrušeno OM. Po instalaci Azure Linux Agent (viz v předchozím kroku), upravte pomocí následujících parametrů v /etc/waagent.conf řádně podporovat:

        ResourceDisk.Format=y
        ResourceDisk.Filesystem=ext4
        ResourceDisk.MountPoint=/mnt/resource
        ResourceDisk.EnableSwap=y
        ResourceDisk.SwapSizeMB=2048    ## NOTE: set this to whatever you need it to be.

14. Spusťte následující příkazy pro deprovision virtuálního počítače a příprava zřizování na Azure:

        # sudo waagent -force -deprovision
        # export HISTSIZE=0
        # logout

15. Klikněte na tlačítko **Akce -> vypnout dolů** ve Správci Hyper-V. Vaše Linux virtuálního pevného disku je nyní připravena k nahrát Azure.


## <a name="next-steps"></a>Další kroky
Teď budete chtít použít k vytvoření nové virtuálních počítačích v Azure VHD Oracle Linux. Pokud se jedná poprvé, že jste odesílá soubor VHD Azure, najdete v článku kroky 2 a 3 v [Vytvoření a odeslání virtuálního pevného disku, který obsahuje operační systém Linux](virtual-machines-linux-classic-create-upload-vhd.md).
