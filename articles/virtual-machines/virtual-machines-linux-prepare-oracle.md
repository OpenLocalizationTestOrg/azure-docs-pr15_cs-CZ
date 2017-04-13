<properties
pageTitle="Příprava Oracle Linux virtuálního počítače Azure | Microsoft Azure"
description="Krok za krokem konfigurace Oracle virtuální počítač Linux v Microsoft Azure."
services="virtual-machines-linux"
authors="bbenz"
manager="timlt"
documentationCenter="virtual-machines"
tags="azure-service-management,azure-resource-manager"
/>

<tags
ms.service="virtual-machines-linux"
ms.devlang="na"
ms.topic="article"
ms.tgt_pltfrm="vm-linux"
ms.workload="infrastructure-services"
ms.date="06/22/2015"
ms.author="bbenz" />

# <a name="prepare-an-oracle-linux-virtual-machine-for-azure"></a>Příprava virtuálního počítače Oracle Linux Azure

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]


-   [Příprava virtuálního počítače Oracle Linux 6.4 + Azure](virtual-machines-linux-oracle-create-upload-vhd.md)

-   [Příprava virtuálního počítače Oracle Linux 7.0 + Azure](virtual-machines-linux-oracle-create-upload-vhd.md)

## <a name="prerequisites"></a>Zjistit předpoklady pro
V tomto článku se předpokládá, že jste již nainstalovali operační systém Oracle Linux virtuální pevný disk. Existují více nástroje pro vytváření VHD souborů, například řešení virtualizace například Hyper-V. Pokyny najdete v tématu [instalace pro Hyper-V a vytvoření virtuálního počítače](http://technet.microsoft.com/library/hh846766.aspx).

**Poznámky k instalaci Linux Oracle**

- Oracle červené klobouk kompatibilní jádra a jeho UEK3 (nerozbitného Enterprise jádra) jsou podporovány na Hyper-V a Azure. Nejlepších výsledků dosáhnete nezapomeňte aktualizovat na nejnovější jádra při přípravě vašeho virtuální pevný disk Oracle Linux.

- Oracle UEK2 nepodporuje Hyper-V a Azure jako nezahrnuje požadované ovladače.

- Novější formát VHDX není podporován v Azure. Disk můžete převést do formátu virtuální pevný disk pomocí Hyper-V správce nebo rutinu převést virtuální pevný disk.

- Pokud instalujete systém Linux, doporučujeme použít standardní oddíly spíše než LVM (často výchozí nastavení pro mnoho instalace). To bude konfliktům LVM název s klonovaný VMs, zejména pokud OS disku je třeba připojit k jiné OM návody na řešení. LVM nebo [RAID](virtual-machines-linux-configure-raid.md) může být použít na discích data.

- NUMA nepodporuje větších OM kvůli chyby v níže uvedených 2.6.37 verzí jádra Linux. Tento problém primárně ovlivňuje distribuce, které používají odesílání dat červené klobouk 2.6.32 jádra. Ruční instalace agenta Azure Linux (waagent) automaticky zakáže NUMA v konfiguraci GRUB jádra Linux. Další informace najdete v následujících krocích.

- Konfigurovat oddíl vyměňovat na disku s operačním systémem. Vytvoření souboru vyměňovat na disku dočasné zdroje je možné konfigurovat agenta Linux. Další informace najdete v následujících krocích.

- Všechny VHD musí mít velikosti, jež jsou násobky čísla 1 MB.

- Ujistěte se, že `Addons` úložiště je povolený. Zvolte Upravit `/etc/yum.repo.d/public-yum-ol6.repo`(Oracle Linux 6) nebo `/etc/yum.repo.d/public-yum-ol7.repo`(Oracle Linux) a změnit plnou čáru `enabled=0` k `enabled=1` v části **[ol6_addons]** a **[ol7_addons]** v tomto souboru.


## <a name="oracle-linux-64"></a>Oracle Linux 6.4 +
Musíte nejdřív udělat konkrétní kroků v operačním systému virtuální počítač běží v Azure.

1. V prostředním podokně s Hyper-V odpovědi vyberte virtuální počítač.

2. Klikněte na **Připojit** k otevřete okno pro virtuální počítač.

3. Odinstalace NetworkManager spuštěním následujícího příkazu:

        # sudo rpm -e --nodeps NetworkManager

    >[AZURE.NOTE] Pokud už není nainstalovaná balíček, tento příkaz se nepovede s chybovou zprávou. Očekává se, že.

4. Vytvoření souboru s názvem **** síť/etc/sysconfig/adresář, který obsahuje následující text:

    `NETWORKING=yes`  
    `HOSTNAME=localhost.localdomain`

5.  Vytvoření souboru s názvem **ifcfg eth0** v /etc/sysconfig/network-scripts / adresář, který obsahuje následující text:

        DEVICE=eth0
        ONBOOT=yes
        BOOTPROTO=dhcp
            TYPE=Ethernet
            USERCTL=no
            PEERDNS=yes
        IPV6INIT=no

6.  Přesunout nebo odstranit udev pravidla pro omezení generování statické pravidla pro rozhraní Ethernet. Tato pravidla později způsobit problémy při kopírujete virtuálního počítače v Azure nebo Hyper-V:

        # sudo mkdir -m 0700 /var/lib/waagent
        # sudo mv /lib/udev/rules.d/75-persistent-net-generator.rules /var/lib/waagent/ 2\>/dev/null
        # sudo mv /etc/udev/rules.d/70-persistent-net.rules /var/lib/waagent/ 2\>/dev/null

7.  Zajistit, aby se síťové služby zahájen při spuštění spuštěním následujícího příkazu:

        # chkconfig network on

8.  Nainstalujte python pyasn1 spuštěním následujícího příkazu:

        # sudo yum install python-pyasn1

9.  Úprava řádku jádra spouštění v konfiguraci grub mají být další jádra parametry Azure. K tomuto účelu otevřete "/ boot/grub/menu.lst" v textovém editoru a ujistěte se, že jádra výchozí zahrnuje následující parametry:

        console=ttyS0 earlyprintk=ttyS0 rootdelay=300 numa=off

    Také zajistíte tak, že všechny zprávy konzoly posílají první sériové port, který může pomoci Azure podporu s ladění problémy. Tímto zakážete NUMA kvůli chyby v kompatibilní jádra červené klobouk Oracle.

    Kromě výše uvedené, doporučujeme ho *Odebrat* následujících parametrů:

        rhgb quiet crashkernel=auto

    Grafické a tichý spouštěcí nejsou užitečné v cloudu prostředí, které chceme všechny protokoly nechat zasílat sériový port.

    `crashkernel` Možnost může být vlevo nakonfigurované podle potřeby, ale Všimněte si, že tento parametr zkrátíte paměti v OM tak, že 128 MB a více. To může způsobovat problémy v menší velikosti OM.

10.  Ujistěte se, že SSH server nainstalovali a nakonfigurovali zahájíte při spuštění. Je to obvykle výchozí.

11.  Nainstalujte Azure Linux Agent pomocí tohoto příkazu:

        # <a name="sudo-yum-install-walinuxagent"></a>instalace yum sudo WALinuxAgent

    Poznámka: při instalaci balíčku WALinuxAgent odeberete NetworkManager NetworkManager gnome balíčků Pokud nebyly už odebrat podle popisu v kroku 2.

12.  Nevytvářejte vyměňovat místa na disku s operačním systémem.

    Azure Linux Agent můžete automaticky nakonfigurovat odkládací prostor místních zdrojů disku, který je připojen k OM po zajištění služby na Azure. Všimněte si, že místních zdrojů je disk *dočasné* a může být nevyprázdnili při poskytování zrušeno OM. Po instalaci Azure Linux Agent (viz předchozím kroku), upravte pomocí následujících parametrů v /etc/waagent.conf řádně podporovat:

        ResourceDisk.Format=y

        ResourceDisk.Filesystem=ext4

        ResourceDisk.MountPoint=/mnt/resource

        ResourceDisk.EnableSwap=y

        ResourceDisk.SwapSizeMB=2048 ## Poznámka: Toto nastavení výběrem něco jiného, co potřebujete mít.

13.  Spusťte následující příkazy pro deprovision virtuálního počítače a příprava zřizování na Azure:

        # <a name="sudo-waagent--force--deprovision"></a>sudo waagent – vynucení - identitách
        # <a name="export-histsize0"></a>Export HISTSIZE = 0
        # <a name="logout"></a>odhlášení

14.  Klikněte na **Akce -\> vypnout** ve Správci Hyper-V. Vaše Linux virtuálního pevného disku je nyní připravena k nahrát Azure.

## <a name="oracle-linux-70"></a>Oracle Linux 7.0 +
**Změny v Oracle Linux 7**

Příprava Azure virtuálního počítače Oracle Linux 7 je velmi podobné proces pro Oracle Linux 6. Můžou ale nastat několik důležitých rozdílů vhodné poznamenat:

-   Kompatibilní jádra červené klobouk a Oracle UEK3 podporují služby Azure. Doporučujeme jádra UEK3.

-   Balíček NetworkManager už není v konfliktu s prací agenta Azure Linux. Ve výchozím nastavení je nainstalovaný balíček a doporučujeme, aby se odstraní.

-   GRUB2 teď používá jako výchozí zaváděcím tak postup pro úpravu jádra parametry se takto změnila (viz níže).

-   XFS je teď výchozí systém souborů. Systém souborů ext4 pořád lze podle potřeby.

**Kroky konfigurace**

1.  Ve Správci Hyper-V vyberte virtuální počítač.

2.  Klikněte na **Připojit** k otevřete okno konzoly pro virtuální počítač.

3.  Vytvoření souboru s názvem **** síť/etc/sysconfig/adresář, který obsahuje následující text:

        NETWORKING=yes
        HOSTNAME=localhost.localdomain

4.  Vytvoření souboru s názvem **ifcfg eth0** v /etc/sysconfig/network-scripts / adresář, který obsahuje následující text:

        DEVICE=eth0
        ONBOOT=yes
        BOOTPROTO=dhcp
        TYPE=Ethernet
        USERCTL=no
            PEERDNS=yes
        IPV6INIT=no

5.  Přesunout nebo odstranit udev pravidla pro omezení generování statické pravidla pro rozhraní Ethernet. Tato pravidla způsobit problémy, když kopírujete virtuálního počítače v Microsoft Azure nebo Hyper-V.

        # sudo mkdir -m 0700 /var/lib/waagent
        # sudo mv /lib/udev/rules.d/75-persistent-net-generator.rules /var/lib/waagent/ 2>/dev/null
        # sudo mv /etc/udev/rules.d/70-persistent-net.rules /var/lib/waagent/ 2>/dev/null

6.  Zajistit, aby se síťové služby zahájen při spuštění spuštěním následujícího příkazu:

        # sudo chkconfig network on

7.  Nainstalujte balíček python pyasn1 spuštěním následujícího příkazu:

        # sudo yum install python-pyasn1

8.  Spusťte tento příkaz zrušte aktuální metadata yum a nainstalujte všechny aktualizace:

        # sudo yum clean all
        # sudo yum -y update

9.  Úprava řádku jádra spouštění v konfiguraci grub mají být další jádra parametry Azure. Uděláte to takto: otevřít "/ atd/výchozí/grub" v textovém editoru a úpravy GRUB\_CMDLINE\_LINUX parametru, například:

        GRUB\_CMDLINE\_LINUX="rootdelay=300 console=ttyS0 earlyprintk=ttyS0"

    Také zajistíte tak všechny konzoly zprávy se odesílají první sériové port, který může pomoci Azure podporu s ladění problémy. Kromě výše uvedené, doporučujeme ho *Odebrat* následujících parametrů:

        rhgb quiet crashkernel=auto

    Grafické a tichý spouštěcí nejsou užitečné v cloudu prostředí, které chceme všechny protokoly nechat zasílat sériový port.

    `crashkernel` Možnost může být vlevo nakonfigurované podle potřeby, ale Všimněte si, že tento parametr zkrátíte paměti v OM tak, že 128 MB a více. To může způsobovat problémy v menší velikosti OM.

10.  Po dokončení úprav "/ atd/výchozí/grub", spusťte tento příkaz nové vytvoření grub konfigurace:

        # <a name="sudo-grub2-mkconfig--o-bootgrub2grubcfg"></a>sudo grub2 mkconfig - o /boot/grub2/grub.cfg

11.  Ujistěte se, že SSH server nainstalovali a nakonfigurovali zahájíte při spuštění. Je to obvykle výchozí.

12.  Nainstalujte Azure Linux Agent pomocí tohoto příkazu:

        # <a name="sudo-yum-install-walinuxagent"></a>instalace yum sudo WALinuxAgent

13.  Nevytvářejte vyměňovat místa na disku s operačním systémem.

    Azure Linux Agent můžete automaticky nakonfigurovat odkládací prostor místních zdrojů disku, který je připojen k OM po zajištění služby na Azure. Všimněte si, že místních zdrojů je disk *dočasné* a může být nevyprázdnili při poskytování zrušeno OM. Po instalaci Azure Linux Agent (viz předchozím kroku), upravte pomocí následujících parametrů v /etc/waagent.conf řádně podporovat:

        ResourceDisk.Format=y ResourceDisk.Filesystem=ext4 ResourceDisk.MountPoint=/mnt/resource ResourceDisk.EnableSwap=y ResourceDisk.SwapSizeMB=2048 ## Poznámka: Toto nastavení výběrem něco jiného, co potřebujete mít.

14.  Spusťte následující příkazy pro deprovision virtuálního počítače a příprava zřizování na Azure:

        # <a name="sudo-waagent--force--deprovision"></a>sudo waagent – vynucení - identitách
        # <a name="export-histsize0"></a>Export HISTSIZE = 0
        # <a name="logout"></a>odhlášení

15.  Klikněte na **Akce -\> vypnout** ve Správci Hyper-V. Vaše Linux virtuálního pevného disku je nyní připravena k nahrát Azure.
