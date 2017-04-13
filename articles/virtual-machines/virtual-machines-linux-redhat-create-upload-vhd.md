<properties
    pageTitle="Vytvořte a uložte červené klobouk Enterprise Linux virtuálního pevného disku pro použití v Azure"
    description="Zjistěte, jak vytvořit a nahrát Azure virtuální pevný disk (virtuální pevný disk), která obsahuje operační systém červená klobouk Linux."
    services="virtual-machines-linux"
    documentationCenter=""
    authors="SuperScottz"
    manager="timlt"
    editor="tysonn"
    tags="azure-resource-manager,azure-service-management"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="02/17/2016"
    ms.author="mingzhan"/>


# <a name="prepare-a-red-hat-based-virtual-machine-for-azure"></a>Příprava virtuálního počítače červené klobouk založené na Azure

V tomto článku se dozvíte, jak připravit počítač virtuální červené klobouk Enterprise Linux (RHEL) pro použití v Azure. Verze RHEL, které jsou uvedené v tomto článku jsou 6,7, 7.1 a 7.2. Hypervisory pro přípravu, které jsou uvedené v tomto článku jsou Hyper-V, na základě jádra virtuálního počítače (KVM) a VMware. Další informace o požadavky na oprávněnost pro účast v programu přístup ke cloudu klobouk červené najdete v článku [Web přístup ke cloudu červené klobouk](http://www.redhat.com/en/technologies/cloud-computing/cloud-access) a [Systémem RHEL na Azure](https://access.redhat.com/articles/1989673).

[Příprava virtuálního počítače RHEL 6,7 od nadřízeného Hyper-V](#rhel67hyperv)

[Příprava virtuálního počítače RHEL 7.1/7.2 od nadřízeného Hyper-V](#rhel7xhyperv)

[Příprava RHEL 6,7 virtuálního počítače z KVM](#rhel67kvm)

[Příprava RHEL 7.1/7.2 virtuálního počítače z KVM](#rhel7xkvm)

[Příprava RHEL 6,7 virtuálního počítače z VMware](#rhel67vmware)

[Příprava RHEL 7.1/7.2 virtuálního počítače z VMware](#rhel7xvmware)

[Příprava RHEL 7.1/7.2 virtuální počítač ze souboru kickstart](#rhel7xkickstart)


## <a name="prepare-a-red-hat-based-virtual-machine-from-hyper-v-manager"></a>Příprava na základě červené klobouk virtuálního počítače od nadřízeného Hyper-V
### <a name="prerequisites"></a>Zjistit předpoklady pro
V této části předpokládá, že jste již máte nainstalovanou RHEL obrázek (z ISO souboru, který jste získali od červené klobouk webu) virtuální pevný disk (virtuální pevný disk). Další informace o používání Hyper-V správci nainstalovat obrázek operační systém, najdete v článku [instalace Hyper-V roli a konfigurace virtuálního počítače](http://technet.microsoft.com/library/hh846766.aspx).

**Poznámky k instalaci RHEL**

- Další informace o přípravě Linux Azure také najdete [Obecné poznámky k instalaci Linux](virtual-machines-linux-create-upload-generic.md#general-linux-installation-notes) .

- Novější formát VHDX není podporován v Azure. Disk můžete převést do formátu virtuální pevný disk pomocí Hyper-V správce nebo rutiny prostředí PowerShell **převést virtuální pevný disk** .

- VHD musí být vytvořeny jako "pevnou" – dynamické VHD nejsou podporované.

- Pokud instalujete systém Linux, doporučujeme použít standardní oddíly spíše než LVM (často výchozí nastavení pro mnoho instalace). To bude konfliktům LVM název s klonovaný VMs, zejména pokud OS disku je třeba připojit k jiné OM návody na řešení. LVM nebo [RAID](virtual-machines-linux-configure-raid.md) může být použít na discích data.

- Konfigurovat oddíl vyměňovat na disku s operačním systémem. Můžete nakonfigurovat Linux agent pro vytvoření souboru vyměňovat na disku dočasné zdroje. Další informace o tomto je k dispozici v následujících krocích.

- Všechny VHD musí mít velikosti, jež jsou násobky čísla 1 MB.

- Pokud používáte **qemu img** převést disku obrázky ve formátu virtuálního pevného disku, Všimněte si, že je známý problém verze qemu img 2.2.1 nebo novější. Tato chyba výsledkem nesprávně formátované virtuální pevný disk. Tento problém se má opraví v nadcházející verzi qemu img. Nyní, doporučujeme použít qemu img verze 2.2.0 nebo starším.

### <a id="rhel67hyperv"> </a>Příprava virtuálního počítače RHEL 6,7 od nadřízeného Hyper-V###


1.  Ve Správci Hyper-V vyberte virtuální počítač.

2.  Klikněte na **Připojit** k otevřete okno konzoly pro virtuální počítač.

3.  Odinstalace NetworkManager spuštěním následujícího příkazu:

        # sudo rpm -e --nodeps NetworkManager

    Všimněte si, že pokud balíčku už není nainstalovaná, tento příkaz se nepovede s chybovou zprávou. Očekává se, že.

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

6.  Přesunout nebo odstranit udev pravidla pro omezení generování statické pravidla pro rozhraní Ethernet. Tato pravidla později způsobit problémy při klonovat virtuálního počítače v Microsoft Azure nebo Hyper-V:

        # sudo mkdir -m 0700 /var/lib/waagent
        # sudo mv /lib/udev/rules.d/75-persistent-net-generator.rules /var/lib/waagent/
        # sudo mv /etc/udev/rules.d/70-persistent-net.rules /var/lib/waagent/

7.  Zajistit, aby se síťové služby zahájen při spuštění spuštěním následujícího příkazu:

        # sudo chkconfig network on

8.  Registrace předplatného červené klobouk povolení instalace balíčků z úložiště RHEL spuštěním následujícího příkazu:

        # sudo subscription-manager register --auto-attach --username=XXX --password=XXX

9.  Balíček WALinuxAgent `WALinuxAgent-<version>` bylo posunuto do úložiště extra červené klobouk. Povolení úložišti extra spuštěním následujícího příkazu:

        # subscription-manager repos --enable=rhel-6-server-extras-rpms

10. Úprava řádku jádra spouštění v konfiguraci grub mají být další jádra parametry Azure. K tomuto účelu otevřete `/boot/grub/menu.lst` v textovém editoru a ujistěte se, že jádra výchozí zahrnuje následující parametry:

        console=ttyS0
        earlyprintk=ttyS0
        rootdelay=300
        numa=off

    Také zajistíte tak, že všechny zprávy konzoly posílají první sériové port, který může pomoci Azure podporu s ladění problémy. Tímto zakážete NUMA kvůli chyby ve verzi jádra používanou RHEL 6.

    Kromě výše akci doporučujeme vám odebrat následujících parametrů.

        rhgb quiet crashkernel=auto

    Grafické a tichý spouštěcí nejsou užitečné v cloudu prostředí, které chceme všechny protokoly nechat zasílat sériový port.

    Možnost crashkernel může být vlevo nakonfigurované podle potřeby, ale Všimněte si, že tento parametr zkrátíte paměti v OM tak, že 128 MB a více. To může způsobovat problémy v menší velikosti OM.

11. Ujistěte se, že SSH server nainstalovali a nakonfigurovali zahájíte při spuštění. Je to obvykle výchozí. Změna /etc/ssh/sshd_config zadejte následující řádek:

        ClientAliveInterval 180

12. Nainstalujte Azure Linux Agent pomocí tohoto příkazu:

        # sudo yum install WALinuxAgent
        # sudo chkconfig waagent on

    Poznámka: při instalaci balíčku WALinuxAgent odeberete NetworkManager NetworkManager gnome balíčků Pokud nebyly už odebrat podle popisu v kroku 2.

13. Nevytvářejte vyměňovat místa na disku s operačním systémem.
Azure Linux Agent můžete automaticky nakonfigurovat odkládací prostor místních zdrojů disku, který je připojen k OM po OM máte k dispozici v Azure. Všimněte si, že disku místních zdrojů je dočasný disk a může vyprázdní při poskytování zrušeno OM. Po instalaci Azure Linux Agent (viz v předchozím kroku), upravte pomocí následujících parametrů v /etc/waagent.conf řádně podporovat:

        ResourceDisk.Format=y
        ResourceDisk.Filesystem=ext4
        ResourceDisk.MountPoint=/mnt/resource
        ResourceDisk.EnableSwap=y
        ResourceDisk.SwapSizeMB=2048    ## NOTE: set this to whatever you need it to be.

14. Zrušení registrace předplatné (v případě potřeby) spusťte tento příkaz:

        # sudo subscription-manager unregister

15. Spusťte následující příkazy pro deprovision virtuálního počítače a příprava zřizování na Azure:

        # sudo waagent -force -deprovision
        # export HISTSIZE=0
        # logout

16. Klikněte na **Akce > vypnout** ve Správci Hyper-V. Vaše Linux virtuálního pevného disku je nyní připravena k nahrát Azure.
 

### <a id="rhel7xhyperv"> </a>Příprava virtuálního počítače RHEL 7.1/7.2 od nadřízeného Hyper-V###

1.  Ve Správci Hyper-V vyberte virtuální počítač.

2.  Klikněte na **Připojit** k otevřete okno konzoly pro virtuální počítač.

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

5.  Zajistit, aby se síťové služby zahájen při spuštění spuštěním následujícího příkazu:

        # sudo chkconfig network on

6.  Registrace předplatného červené klobouk povolení instalace balíčků z úložiště RHEL spuštěním následujícího příkazu:

        # sudo subscription-manager register --auto-attach --username=XXX --password=XXX

7.  Úprava řádku jádra spouštění v konfiguraci grub mají být další jádra parametry Azure. K tomuto účelu otevřete `/etc/default/grub` v textovém editoru a upravit parametr **GRUB_CMDLINE_LINUX** . Příklad:

        GRUB_CMDLINE_LINUX="rootdelay=300
        console=ttyS0
        earlyprintk=ttyS0"

    Také zajistíte tak, že všechny zprávy konzoly posílají první sériové port, který může pomoci Azure podporu s ladění problémy. Kromě výše akci doporučujeme vám odebrat následujících parametrů.

        rhgb quiet crashkernel=auto

    Grafické a tichý spouštěcí nejsou užitečné v cloudu prostředí, které chceme všechny protokoly nechat zasílat sériový port.
    Možnost crashkernel může být vlevo nakonfigurované podle potřeby, ale Všimněte si, že tento parametr zkrátíte paměti v OM tak, že 128 MB a více. To může způsobovat problémy v menší velikosti OM.

8.  Po dokončení úprav `/etc/default/grub`, spusťte tento příkaz nové vytvoření grub konfigurace:

        # sudo grub2-mkconfig -o /boot/grub2/grub.cfg

9.  Ujistěte se, že SSH server nainstalovali a nakonfigurovali zahájíte při spuštění. Je to obvykle výchozí. Úprava `/etc/ssh/sshd_config` zahrnout následující řádek:

        ClientAliveInterval 180

10. Balíček WALinuxAgent `WALinuxAgent-<version>` bylo posunuto do úložiště extra červené klobouk. Povolení úložišti extra spuštěním následujícího příkazu:

        # subscription-manager repos --enable=rhel-7-server-extras-rpms

11. Nainstalujte Azure Linux Agent pomocí tohoto příkazu:

        # sudo yum install WALinuxAgent
        # sudo systemctl enable waagent.service

12. Nevytvářejte vyměňovat místa na disku s operačním systémem. Azure Linux Agent můžete automaticky nakonfigurovat odkládací prostor místních zdrojů disku, který je připojen k OM po OM máte k dispozici v Azure. Všimněte si, že disku místních zdrojů je dočasný disk a může vyprázdní při poskytování zrušeno OM. Po instalaci Azure Linux Agent (viz v předchozím kroku), upravte pomocí následujících parametrů v `/etc/waagent.conf` řádně podporovat:

        ResourceDisk.Format=y
        ResourceDisk.Filesystem=ext4
        ResourceDisk.MountPoint=/mnt/resource
        ResourceDisk.EnableSwap=y
        ResourceDisk.SwapSizeMB=2048    ## NOTE: set this to whatever you need it to be.

13. Pokud budete chtít unregister předplatné, spusťte tento příkaz:

        # sudo subscription-manager unregister

14. Spusťte následující příkazy pro deprovision virtuálního počítače a příprava zřizování na Azure:

        # sudo waagent -force -deprovision
        # export HISTSIZE=0
        # logout

15. Klikněte na **Akce > vypnout** ve Správci Hyper-V. Vaše Linux virtuálního pevného disku je nyní připravena k nahrát Azure.
 


## <a name="prepare-a-red-hat-based-virtual-machine-from-kvm"></a>Příprava na základě červené klobouk virtuálního počítače z KVM

### <a id="rhel67kvm"> </a>Připravit virtuálního počítače RHEL 6,7 KVM###


1.  Obrázek KVM RHEL 6,7 stáhněte z webu červené klobouk.

2.  Nastavení hesla kořenové.

    Generování šifrované heslo a zkopírujte výstup příkazu:

        # openssl passwd -1 changeme

    Nastavení hesla kořenové s guestfish:

        # guestfish --rw -a <image-name>
        ><fs> run
        ><fs> list-filesystems
        ><fs> mount /dev/sda1 /
        ><fs> vi /etc/shadow
        ><fs> exit

    Změna druhé pole Kořenový uživateli "nepoužívejte!" šifrované heslem.

3.  Vytvořte virtuálního počítače v KVM z obrázku qcow2, nastavte typ disku **qcow2**a nastavte model virtuální sítě rozhraní zařízení na **virtio**. Potom spusťte virtuálního počítače a přihlaste se jako kořenovou.

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

6.  Přesunout nebo odstranit udev pravidla pro omezení generování statické pravidla pro rozhraní Ethernet. Tato pravidla později způsobit problémy při klonovat virtuálního počítače v Microsoft Azure nebo Hyper-V:

        # mkdir -m 0700 /var/lib/waagent
        # mv /lib/udev/rules.d/75-persistent-net-generator.rules /var/lib/waagent/
        # mv /etc/udev/rules.d/70-persistent-net.rules /var/lib/waagent/

7.  Zajistit, aby se síťové služby zahájen při spuštění spuštěním následujícího příkazu:

        # chkconfig network on

8.  Registrace předplatného červené klobouk povolení instalace balíčků z úložiště RHEL spuštěním následujícího příkazu:

        # subscription-manager register --auto-attach --username=XXX --password=XXX

9.  Úprava řádku jádra spouštění v konfiguraci grub mají být další jádra parametry Azure. K tomuto účelu otevřete `/boot/grub/menu.lst` v textovém editoru a ujistěte se, že jádra výchozí zahrnuje následující parametry:

        console=ttyS0 earlyprintk=ttyS0 rootdelay=300 numa=off

    Také zajistíte tak, že všechny zprávy konzoly posílají první sériové port, který může pomoci Azure podporu s ladění problémy. Tímto zakážete NUMA kvůli chyby ve verzi jádra používanou RHEL 6.

    Kromě výše akci doporučujeme vám odebrat následujících parametrů.

        rhgb quiet crashkernel=auto

    Grafické a tichý spouštěcí nejsou užitečné v cloudu prostředí, které chceme všechny protokoly nechat zasílat sériový port.
    Možnost crashkernel může být vlevo nakonfigurované podle potřeby, ale Všimněte si, že tento parametr zkrátíte paměti v OM tak, že 128 MB a více. To může způsobovat problémy v menší velikosti OM.

10. Přidáte moduly pro Hyper-V do initramfs:  

    Úprava `/etc/dracut.conf` a přidávání obsahu: add_drivers += "hv_vmbus hv_netvsc hv_storvsc"

    Opětovné sestavení initramfs: # dracut – f - v

11. Odinstalace cloudu spuštění:

        # yum remove cloud-init

12. Ujistěte se, že SSH server nainstalovali a nakonfigurovali zahájíte při spuštění:

        # chkconfig sshd on

    Změna /etc/ssh/sshd_config zahrnuty následující řádky:

        PasswordAuthentication yes
        ClientAliveInterval 180

    Restartujte sshd:

        # service sshd restart

13. Balíček WALinuxAgent `WALinuxAgent-<version>` bylo posunuto do úložiště extra červené klobouk. Povolení úložišti extra spuštěním následujícího příkazu:

        # subscription-manager repos --enable=rhel-6-server-extras-rpms

14. Nainstalujte Azure Linux Agent pomocí tohoto příkazu:

        # yum install WALinuxAgent
        # chkconfig waagent on

15. Azure Linux Agent můžete automaticky nakonfigurovat odkládací prostor místních zdrojů disku, který je připojen k OM po OM máte k dispozici v Azure. Všimněte si, že disku místních zdrojů je dočasný disk a může vyprázdní při poskytování zrušeno OM. Po instalaci Azure Linux Agent (viz v předchozím kroku), upravte pomocí následujících parametrů v **/etc/waagent.conf** řádně podporovat:

        ResourceDisk.Format=y
        ResourceDisk.Filesystem=ext4
        ResourceDisk.MountPoint=/mnt/resource
        ResourceDisk.EnableSwap=y
        ResourceDisk.SwapSizeMB=2048    ## NOTE: set this to whatever you need it to be.

16. Zrušení registrace předplatné (v případě potřeby) spusťte tento příkaz:

        # subscription-manager unregister

17. Spusťte následující příkazy pro deprovision virtuálního počítače a příprava zřizování na Azure:

        # waagent -force -deprovision
        # export HISTSIZE=0
        # logout

18. Vypnutí OM v KVM.

19. Obrázek qcow2 převeďte do formátu virtuální pevný disk.
    Nejprve převeďte obrázek původním formátu:

         # qemu-img convert -f qcow2 –O raw rhel-6.7.qcow2 rhel-6.7.raw
    Ujistěte se, že velikost nezpracovanými obrázek zarovnán 1 MB. V opačném zaokrouhlení nahoru velikost vpravo zarovnáte 1 MB: # MB = $((1024*1024)) # velikost = $(qemu img informace -f nezpracovanými – výstup json "rhel-6.7.raw" | \ gawk "podle (0 Kč /"virtuální velikost": ([0 – 9] +), /, val) {tisk val[1]}') # rounded_size = $((($size/$MB + 1)*$MB))

         # qemu-img resize rhel-6.7.raw $rounded_size

    Převedení nepoužitý disk na pevnou velikostí virtuálního pevného disku:

         # qemu-img convert -f raw -o subformat=fixed -O vpc rhel-6.7.raw rhel-6.7.vhd


### <a id="rhel7xkvm"> </a>Připravit virtuálního počítače RHEL 7.1/7.2 KVM###


1.  Stáhněte z webu červené klobouk KVM obrázek RHEL 7.1 (nebo 7.2). Jako v příkladu použijeme RHEL 7.1.

2.  Nastavení hesla kořenové.

    Generování šifrované heslo a zkopírujte výstup příkazu:

        # openssl passwd -1 changeme

    Nastavení hesla kořenové s guestfish.

        # guestfish --rw -a <image-name>
        ><fs> run
        ><fs> list-filesystems
        ><fs> mount /dev/sda1 /
        ><fs> vi /etc/shadow
        ><fs> exit

    Změna druhé pole Kořenový uživateli "nepoužívejte!" šifrované heslem.

3.  Vytvořte virtuálního počítače v KVM z obrázku qcow2, nastavte typ disku **qcow2**a nastavte model virtuální sítě rozhraní zařízení na **virtio**. Potom spusťte virtuálního počítače a přihlaste se jako kořenovou.

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

6.  Zajistit, aby se síťové služby zahájen při spuštění spuštěním následujícího příkazu:

        # chkconfig network on

7.  Registrace předplatného červené klobouk povolit instalaci balíčků z úložiště RHEL spuštěním následujícího příkazu:

        # subscription-manager register --auto-attach --username=XXX --password=XXX

8.  Úprava řádku jádra spouštění v konfiguraci grub mají být další jádra parametry Azure. K tomuto účelu otevřete `/etc/default/grub` v textovém editoru a upravit parametr **GRUB_CMDLINE_LINUX** . Příklad:

        GRUB_CMDLINE_LINUX="rootdelay=300
        console=ttyS0
        earlyprintk=ttyS0"

    Také zajistíte tak, že všechny zprávy konzoly posílají první sériové port, který může pomoci Azure podporu s ladění problémy. Kromě výše akci doporučujeme vám odebrat následujících parametrů.

        rhgb quiet crashkernel=auto

    Grafické a tichý spouštěcí nejsou užitečné v cloudu prostředí, které chceme všechny protokoly nechat zasílat sériový port.
    Možnost crashkernel může být vlevo nakonfigurované podle potřeby, ale Všimněte si, že tento parametr zkrátíte paměti v OM tak, že 128 MB a více. To může způsobovat problémy v menší velikosti OM.

9.  Po dokončení úprav `/etc/default/grub`, spusťte tento příkaz nové vytvoření grub konfigurace:

        # grub2-mkconfig -o /boot/grub2/grub.cfg

10. Přidáte moduly pro Hyper-V do initramfs:

    Úprava `/etc/dracut.conf` a přidávání obsahu:

        add_drivers+=”hv_vmbus hv_netvsc hv_storvsc”

    Opětovné sestavení initramfs:

        # dracut –f -v

11. Odinstalace cloudu spuštění:

        # yum remove cloud-init

12. Ujistěte se, že SSH server nainstalovali a nakonfigurovali zahájíte při spuštění:

        # systemctl enable sshd

    Změna /etc/ssh/sshd_config zahrnuty následující řádky:

        PasswordAuthentication yes
        ClientAliveInterval 180

    Restartujte sshd:

        systemctl restart sshd

13. Balíček WALinuxAgent `WALinuxAgent-<version>` bylo posunuto do úložiště extra červené klobouk. Povolení úložišti extra spuštěním následujícího příkazu:

        # subscription-manager repos --enable=rhel-7-server-extras-rpms

14. Nainstalujte Azure Linux Agent pomocí tohoto příkazu:

        # yum install WALinuxAgent

    Povolte službu waagent:

        # systemctl enable waagent.service

15. Nevytvářejte vyměňovat místa na disku s operačním systémem. Azure Linux Agent můžete automaticky nakonfigurovat odkládací prostor místních zdrojů disku, který je připojen k OM po OM máte k dispozici v Azure. Všimněte si, že disku místních zdrojů je dočasný disk a může vyprázdní při poskytování zrušeno OM. Po instalaci Azure Linux Agent (viz v předchozím kroku), upravte pomocí následujících parametrů v `/etc/waagent.conf` řádně podporovat:

        ResourceDisk.Format=y
        ResourceDisk.Filesystem=ext4
        ResourceDisk.MountPoint=/mnt/resource
        ResourceDisk.EnableSwap=y
        ResourceDisk.SwapSizeMB=2048    ## NOTE: set this to whatever you need it to be.

16. Zrušení registrace předplatné (v případě potřeby) spusťte tento příkaz:

        # subscription-manager unregister

17. Spusťte následující příkazy pro deprovision virtuálního počítače a příprava zřizování na Azure:

        # sudo waagent -force -deprovision
        # export HISTSIZE=0
        # logout

18. Vypnutí virtuálního počítače v KVM.

19. Obrázek qcow2 převeďte do formátu virtuální pevný disk.

    Nejprve převeďte obrázek původním formátu:

         # qemu-img convert -f qcow2 –O raw rhel-7.1.qcow2 rhel-7.1.raw

    Ujistěte se, že velikost nezpracovanými obrázek zarovnán 1 MB. V opačném zaokrouhlení nahoru velikost vpravo zarovnáte 1 MB:

         # MB=$((1024*1024))
         # size=$(qemu-img info -f raw --output json "rhel-7.1.raw" | \
                  gawk 'match($0, /"virtual-size": ([0-9]+),/, val) {print val[1]}')
         # rounded_size=$((($size/$MB + 1)*$MB))

         # qemu-img resize rhel-7.1.raw $rounded_size

    Převedení nepoužitý disk na pevnou velikostí virtuálního pevného disku:

         # qemu-img convert -f raw -o subformat=fixed -O vpc rhel-7.1.raw rhel-7.1.vhd


## <a name="prepare-a-red-hat-based-virtual-machine-from-vmware"></a>Příprava na základě červené klobouk virtuálního počítače z VMware
### <a name="prerequisites"></a>Zjistit předpoklady pro
Tato část se předpokládá, že jste již nainstalovali RHEL virtuálního počítače v VMware. Další informace o tom, jak nainstalovat operační systém v VMware [VMware hosta operační systém instalační](http://partnerweb.vmware.com/GOSIG/home.html)příručce.

- Když nainstalujete operační systém Linux, doporučujeme použít standardní oddíly spíše než LVM (často výchozí nastavení pro mnoho instalace). To bude konfliktům LVM název s klonovaný VMs, zejména pokud OS disku je třeba připojit k jiné OM návody na řešení. LVM nebo RAID lze na dat discích Pokud dáváte přednost.

- Konfigurovat oddíl vyměňovat na disku s operačním systémem. Můžete nakonfigurovat Linux agent pro vytvoření souboru vyměňovat na disku dočasné zdroje. Další informace najdete v následujících krocích.

- Při vytváření virtuálních pevného disku, vyberte **úložiště virtuální disk jako jeden soubor**.



### <a id="rhel67vmware"> </a>Připravit virtuálního počítače RHEL 6,7 VMware###

1.  Odinstalace NetworkManager spuštěním následujícího příkazu:

         # sudo rpm -e --nodeps NetworkManager

    Všimněte si, že pokud balíčku už není nainstalovaná, tento příkaz se nepovede s chybovou zprávou. Očekává se, že.

2.  Vytvoření souboru s názvem **** síť/etc/sysconfig/adresář, který obsahuje následující text:

        NETWORKING=yes
        HOSTNAME=localhost.localdomain

3.  Vytvoření souboru s názvem **ifcfg eth0** v /etc/sysconfig/network-scripts / adresář, který obsahuje následující text:

        DEVICE=eth0
        ONBOOT=yes
        BOOTPROTO=dhcp
        TYPE=Ethernet
        USERCTL=no
        PEERDNS=yes
        IPV6INIT=no

4.  Přesunout nebo odstranit udev pravidla pro omezení generování statické pravidla pro rozhraní Ethernet. Tato pravidla později způsobit problémy při klonovat virtuálního počítače v Microsoft Azure nebo Hyper-V:

        # sudo mkdir -m 0700 /var/lib/waagent
        # sudo mv /lib/udev/rules.d/75-persistent-net-generator.rules /var/lib/waagent/
        # sudo mv /etc/udev/rules.d/70-persistent-net.rules /var/lib/waagent/

5.  Zajistit, aby se síťové služby zahájen při spuštění spuštěním následujícího příkazu:

        # sudo chkconfig network on

6.  Registrace předplatného červené klobouk povolení instalace balíčků z úložiště RHEL spuštěním následujícího příkazu:

        # sudo subscription-manager register --auto-attach --username=XXX --password=XXX

7.  Balíček WALinuxAgent `WALinuxAgent-<version>` bylo posunuto do úložiště extra červené klobouk. Povolení úložišti extra spuštěním následujícího příkazu:

        # subscription-manager repos --enable=rhel-6-server-extras-rpms

8.  Úprava řádku jádra spouštění v konfiguraci grub mají být další jádra parametry Azure. K tomuto účelu otevřete "/ boot/grub/menu.lst" v textovém editoru a ujistěte se, že jádra výchozí zahrnuje následující parametry:

        console=ttyS0
        earlyprintk=ttyS0
        rootdelay=300
        numa=off

    Také zajistíte tak, že všechny zprávy konzoly posílají první sériové port, který může pomoci Azure podporu s ladění problémy. Tímto zakážete NUMA kvůli chyby ve verzi jádra používanou RHEL 6.
    Kromě výše akci doporučujeme vám odebrat následujících parametrů.

        rhgb quiet crashkernel=auto

    Grafické a tichý spouštěcí nejsou užitečné v cloudu prostředí, které chceme všechny protokoly nechat zasílat sériový port.
    Možnost crashkernel může být vlevo nakonfigurované podle potřeby, ale Všimněte si, že tento parametr zkrátíte paměti v OM tak, že 128 MB a více. To může způsobovat problémy v menší velikosti OM.

9.  Přidáte moduly pro Hyper-V do initramfs:

        Edit `/etc/dracut.conf` and add content:

            add_drivers+=”hv_vmbus hv_netvsc hv_storvsc”

        Rebuild initramfs:

            # dracut –f -v

10. Ujistěte se, že SSH server nainstalovali a nakonfigurovali zahájíte při spuštění. Je to obvykle výchozí. Úprava `/etc/ssh/sshd_config` zahrnout následující řádek:

        ClientAliveInterval 180

11. Nainstalujte Azure Linux Agent pomocí tohoto příkazu:

        # sudo yum install WALinuxAgent
        # sudo chkconfig waagent on

12. Nevytvářejte vyměňovat místa na disku operační systém:

    Azure Linux Agent můžete automaticky nakonfigurovat odkládací prostor místních zdrojů disku, který je připojen k OM po OM máte k dispozici v Azure. Všimněte si, že disku místních zdrojů je dočasný disk a může vyprázdní při poskytování zrušeno OM. Po instalaci Azure Linux Agent (viz v předchozím kroku), upravte pomocí následujících parametrů v `/etc/waagent.conf` řádně podporovat:

        ResourceDisk.Format=y
        ResourceDisk.Filesystem=ext4
        ResourceDisk.MountPoint=/mnt/resource
        ResourceDisk.EnableSwap=y
        ResourceDisk.SwapSizeMB=2048    ## NOTE: set this to whatever you need it to be.

13. Zrušení registrace předplatné (v případě potřeby) spusťte tento příkaz:

        # sudo subscription-manager unregister

14. Spusťte následující příkazy pro deprovision virtuálního počítače a příprava zřizování na Azure:

        # sudo waagent -force -deprovision
        # export HISTSIZE=0
        # logout

15. Vypněte OM a VMDK soubor můžete převést na soubor VHD.

    Nejprve převeďte obrázek původním formátu:

        # qemu-img convert -f vmdk –O raw rhel-6.7.vmdk rhel-6.7.raw

    Ujistěte se, že velikost nezpracovanými obrázek zarovnán 1 MB. V opačném zaokrouhlení nahoru velikost vpravo zarovnáte 1 MB:

        # MB=$((1024*1024))
        # size=$(qemu-img info -f raw --output json "rhel-6.7.raw" | \
                gawk 'match($0, /"virtual-size": ([0-9]+),/, val) {print val[1]}')
        # rounded_size=$((($size/$MB + 1)*$MB))
        # qemu-img resize rhel-6.7.raw $rounded_size

    Převedení nepoužitý disk na pevnou velikostí virtuálního pevného disku:

        # qemu-img convert -f raw -o subformat=fixed -O vpc rhel-6.7.raw rhel-6.7.vhd


### <a id="rhel7xvmware"> </a>Připravit virtuálního počítače RHEL 7.1/7.2 VMware###

1.  Vytvoření souboru s názvem **** síť/etc/sysconfig/adresář, který obsahuje následující text:

        NETWORKING=yes
        HOSTNAME=localhost.localdomain

2.  Vytvoření souboru s názvem **ifcfg eth0** v /etc/sysconfig/network-scripts / adresář, který obsahuje následující text:

        DEVICE=eth0
        ONBOOT=yes
        BOOTPROTO=dhcp
        TYPE=Ethernet
        USERCTL=no
        PEERDNS=yes
        IPV6INIT=no

3.  Zajistit, aby se síťové služby zahájen při spuštění spuštěním následujícího příkazu:

        # sudo chkconfig network on

4.  Registrace předplatného červené klobouk povolení instalace balíčků z úložiště RHEL spuštěním následujícího příkazu:

        # sudo subscription-manager register --auto-attach --username=XXX --password=XXX

5.  Úprava řádku jádra spouštění v konfiguraci grub mají být další jádra parametry Azure. K tomuto účelu otevřete `/etc/default/grub` v textovém editoru a upravit parametr **GRUB_CMDLINE_LINUX** . Příklad:

        GRUB_CMDLINE_LINUX="rootdelay=300
        console=ttyS0
        earlyprintk=ttyS0"

    Také zajistíte tak, že všechny zprávy konzoly posílají první sériové port, který může pomoci Azure podporu s ladění problémy. Kromě výše akci doporučujeme vám odebrat následujících parametrů.

        rhgb quiet crashkernel=auto

    Grafické a tichý spouštěcí nejsou užitečné v cloudu prostředí, které chceme všechny protokoly nechat zasílat sériový port.
    Možnost crashkernel může být vlevo nakonfigurované podle potřeby, ale Všimněte si, že tento parametr zkrátíte paměti v OM tak, že 128 MB a více. To může způsobovat problémy v menší velikosti OM.

6.  Po dokončení úprav `/etc/default/grub`, spusťte tento příkaz nové vytvoření grub konfigurace:

         # sudo grub2-mkconfig -o /boot/grub2/grub.cfg

7.  Přidáte moduly pro Hyper-V do initramfs:

    Úprava `/etc/dracut.conf`, přidání obsahu:

        add_drivers+=”hv_vmbus hv_netvsc hv_storvsc”

    Opětovné sestavení initramfs:

        # dracut –f -v

8.  Ujistěte se, že SSH server nainstalovali a nakonfigurovali zahájíte při spuštění. Je to obvykle výchozí. Úprava `/etc/ssh/sshd_config` zahrnout následující řádek:

        ClientAliveInterval 180

9.  Balíček WALinuxAgent `WALinuxAgent-<version>` bylo posunuto do úložiště extra červené klobouk. Povolení úložišti extra spuštěním následujícího příkazu:

        # subscription-manager repos --enable=rhel-7-server-extras-rpms

10. Nainstalujte Azure Linux Agent pomocí tohoto příkazu:

        # sudo yum install WALinuxAgent
        # sudo systemctl enable waagent.service

11. Nevytvářejte vyměňovat místa na disku s operačním systémem. Azure Linux Agent můžete automaticky nakonfigurovat odkládací prostor místních zdrojů disku, který je připojen k OM po OM máte k dispozici v Azure. Všimněte si, že disku místních zdrojů je dočasný disk a může vyprázdní při poskytování zrušeno OM. Po instalaci Azure Linux Agent (viz v předchozím kroku), upravte pomocí následujících parametrů v `/etc/waagent.conf` řádně podporovat:

        ResourceDisk.Format=y
        ResourceDisk.Filesystem=ext4
        ResourceDisk.MountPoint=/mnt/resource
        ResourceDisk.EnableSwap=y
        ResourceDisk.SwapSizeMB=2048    ## NOTE: set this to whatever you need it to be.

12. Pokud budete chtít unregister předplatné, spusťte tento příkaz:

        # sudo subscription-manager unregister

13. Spusťte následující příkazy pro deprovision virtuálního počítače a příprava zřizování na Azure:

        # sudo waagent -force -deprovision
        # export HISTSIZE=0
        # logout

14. Vypněte OM a VMDK soubor můžete převést na formát virtuální pevný disk.

    Nejprve převeďte obrázek původním formátu:

        # qemu-img convert -f vmdk –O raw rhel-7.1.vmdk rhel-7.1.raw

    Ujistěte se, že velikost nezpracovanými obrázek zarovnán 1 MB. V opačném zaokrouhlení nahoru velikost vpravo zarovnáte 1 MB:

        # MB=$((1024*1024))
        # size=$(qemu-img info -f raw --output json "rhel-7.1.raw" | \
                 gawk 'match($0, /"virtual-size": ([0-9]+),/, val) {print val[1]}')
        # rounded_size=$((($size/$MB + 1)*$MB))
        # qemu-img resize rhel-7.1.raw $rounded_size

    Převedení nepoužitý disk na pevnou velikostí virtuálního pevného disku:

        # qemu-img convert -f raw -o subformat=fixed -O vpc rhel-7.1.raw rhel-7.1.vhd


## <a name="prepare-a-red-hat-based-virtual-machine-from-an-iso-by-using-a-kickstart-file-automatically"></a>Příprava na základě červené klobouk virtuálního počítače z ISO pomocí souboru kickstart automaticky


### <a id="rhel7xkickstart"> </a>Příprava RHEL 7.1/7.2 virtuální počítač ze souboru kickstart###


1.  Vytvoření souboru kickstart s pod obsahem a uložení souboru. Podrobnosti o instalaci kickstart najdete v tématu [Průvodce instalací Kickstart](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/7/html/Installation_Guide/chap-kickstart-installations.html).



        # Kickstart for provisioning a RHEL 7 Azure VM

        # System authorization information
        auth --enableshadow --passalgo=sha512

        # Use graphical install
        text

        # Do not run the Setup Agent on first boot
        firstboot --disable

        # Keyboard layouts
        keyboard --vckeymap=us --xlayouts='us'

        # System language
        lang en_US.UTF-8

        # Network information
        network  --bootproto=dhcp

        # Root password
        rootpw --plaintext "to_be_disabled"

        # System services
        services --enabled="sshd,waagent,NetworkManager"

        # System timezone
        timezone Etc/UTC --isUtc --ntpservers 0.rhel.pool.ntp.org,1.rhel.pool.ntp.org,2.rhel.pool.ntp.org,3.rhel.pool.ntp.org

        # Partition clearing information
        clearpart --all --initlabel

        # Clear the MBR
        zerombr

        # Disk partitioning information
        part /boot --fstype="xfs" --size=500
        part / --fstyp="xfs" --size=1 --grow --asprimary

        # System bootloader configuration
        bootloader --location=mbr

        # Firewall configuration
        firewall --disabled

        # Enable SELinux
        selinux --enforcing

        # Don't configure X
        skipx

        # Power down the machine after install
        poweroff



        %packages
        @base
        @console-internet
        chrony
        sudo
        parted
        -dracut-config-rescue

        %end

        %post --log=/var/log/anaconda/post-install.log

        #!/bin/bash

        # Register Red Hat Subscription
        subscription-manager register --username=XXX --password=XXX --auto-attach --force

        # Install latest repo update
        yum update -y

        # Enable extras repo
        subscription-manager repos --enable=rhel-7-server-extras-rpms

        # Install WALinuxAgent
        yum install -y WALinuxAgent

        # Unregister Red Hat subscription
        subscription-manager unregister

        # Enable waaagent at boot-up
        systemctl enable waagent

        # Disable the root account
        usermod root -p '!!'

        # Configure swap in WALinuxAgent
        sed -i 's/^\(ResourceDisk\.EnableSwap\)=[Nn]$/\1=y/g' /etc/waagent.conf
        sed -i 's/^\(ResourceDisk\.SwapSizeMB\)=[0-9]*$/\1=2048/g' /etc/waagent.conf

        # Set the cmdline
        sed -i 's/^\(GRUB_CMDLINE_LINUX\)=".*"$/\1="console=tty1 console=ttyS0 earlyprintk=ttyS0 rootdelay=300"/g' /etc/default/grub

        # Enable SSH keepalive
        sed -i 's/^#\(ClientAliveInterval\).*$/\1 180/g' /etc/ssh/sshd_config

        # Build the grub cfg
        grub2-mkconfig -o /boot/grub2/grub.cfg

        # Configure network
        cat << EOF > /etc/sysconfig/network-scripts/ifcfg-eth0
        DEVICE=eth0
        ONBOOT=yes
        BOOTPROTO=dhcp
        TYPE=Ethernet
        USERCTL=no
        PEERDNS=yes
        IPV6INIT=no
        NM_CONTROLLED=yes
        EOF

        # Deprovision and prepare for Azure
        waagent -force -deprovision

        %end

2.  Vložte soubor kickstart místo přístupný z instalace systému.

3.  Ve Správci Hyper-V vytvořte nové OM. Na stránce **připojit virtuální pevný Disk** vyberte **připojit později virtuální pevný disk**a dokončete Průvodce vytvořením virtuálního počítače.

4.  Otevřete OM nastavení:

    na.  Připojte nová virtuální pevný disk bude v angličtině. Zkontrolujte, že vyberte **Formát virtuálního pevného disku** a **Pevnou velikost**.

    b.  Instalace ISO připojte z disku DVD.

    c.  K nastavení nastavte režim CD-ROM.

5.  Začněte OM. Když se zobrazí Průvodce instalací, stisknutím klávesy **Tab** pro nastavení možností spuštění.

6.  ENTER `inst.ks=<the location of the kickstart file>` na konci možnosti spuštění a stiskněte klávesu **Enter**.

7.  Počkejte na dokončení instalace. Po dokončení, OM bude ukončena automaticky. Vaše Linux virtuálního pevného disku je nyní připravena k nahrát Azure.

## <a name="known-issues"></a>Známé problémy
Existují známé problémy při použití RHEL 7.1 v Hyper-V a Azure.

### <a name="disk-io-freeze"></a>Ukotvení vstupu a výstupu disku

K tomuto problému může dojít během časté úložiště disku vstupu a výstupu aktivit s RHEL 7.1 v Hyper-V a Azure.   

Zkopírujte sazba:

Tento problém dochází přerušovaně. Však k rizikové situaci dojde více často průběhu častěji disku operací v Hyper-V a Azure.   


[AZURE.NOTE] Tento známý problém má již byly kanceláři řešili červené klobouk. Pokud chcete nainstalovat přidružené opravy, spusťte tento příkaz:

    # sudo yum update

### <a name="the-hyper-v-driver-could-not-be-included-in-the-initial-ram-disk-when-using-a-non-hyper-v-hypervisor"></a>Ovladač Hyper-V nelze nezahrnou počáteční disk RAM při použití hypervisor Hyper V

V některých případech Linux instalační nemusí zahrnout ovladačů pro Hyper-V počáteční disk RAM (initrd nebo initramfs) Pokud zjistí, že je spuštěná v prostředí pro Hyper-V.

Příprava obrázek Linux při práci v různých virtualizace systému (tedy Virtualbox Xen, atd.), možná budete muset znovu vytvořit initrd zajistit, aby aspoň moduly jádra hv_vmbus a hv_storvsc jsou dostupné na počáteční RAM disku. Toto je známý problém aspoň systémech podle nadřazeného červené klobouk rozdělení.

Tento problém můžete vyřešit, budete muset přidat moduly pro Hyper-V do initramfs a znovu ho:

Úprava `/etc/dracut.conf` a přidávání obsahu:

        add_drivers+=”hv_vmbus hv_netvsc hv_storvsc”

Opětovné sestavení initramfs:

        # dracut –f -v

Další informace najdete v tématu informace o [opětovném initramfs](https://access.redhat.com/solutions/1958).

## <a name="next-steps"></a>Další kroky
Teď jste připraveni vytvoření nové virtuálních počítačích v Azure pomocí virtuální disku červené klobouk Enterprise Linux. Pokud se jedná poprvé, že jste odesílá soubor VHD Azure, najdete v článku kroky 2 a 3 v [Vytvoření a odeslání virtuálního pevného disku, který obsahuje operační systém Linux](virtual-machines-linux-classic-create-upload-vhd.md).

Podrobné informace o hypervisory, které mají oprávnění ke spuštění červené klobouk Enterprise Linux najdete [na webu červené klobouk](https://access.redhat.com/certified-hypervisors).
