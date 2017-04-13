<properties
    pageTitle="Vytvořte a uložte virtuálního pevného disku Linux v Azure"
    description="Zjistěte, jak vytvořit a nahrát Azure virtuální pevný disk (virtuální pevný disk), která obsahuje operační systém Linux."
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
    ms.date="09/23/2016"
    ms.author="szark"/>

# <a name="information-for-non-endorsed-distributions"></a>Informace o-potvrzeného rozdělení #

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]


**Důležité**: Azure platformy SLA platí pro virtuálních počítačích Linux operační systém pouze v případě, že je použit jeden z [potvrzeného rozdělení](virtual-machines-linux-endorsed-distros.md) . Všechny Linux distribuce, které jsou k dispozici v galerii Azure obrázek jsou schválené rozdělení se požadovaná konfigurace.

- [Linux na Azure - potvrzeného rozdělení](virtual-machines-linux-endorsed-distros.md)
- [Podpora pro Linux obrázky v Microsoft Azure](https://support.microsoft.com/kb/2941892)

Všechny distribuce spuštěna Azure muset zahájit počet požadavky na možnost správně spuštěny v rámci platformy.  Tento článek je určený podle ne prostředky komplexní při každé rozdělení se liší. a je úplně možné, že i když splňovaly všechna kritéria dole se potřebujete na výrazně zpřesnění systému Linux zajistit, že správně funguje na platformě.

Je z tohoto důvodu, doporučujeme spouštět jednoho z našich [Linux na Azure potvrzeného distribuce](virtual-machines-linux-endorsed-distros.md) Pokud je to možné. V následujících článcích se vás jak připravit různých schválené Linux distribuce, které jsou podporovány na Azure:

- **[Na základě centOS rozdělení](virtual-machines-linux-create-upload-centos.md)**
- **[Debian Linux](virtual-machines-linux-debian-create-upload-vhd.md)**
- **[Oracle Linux](virtual-machines-linux-oracle-create-upload-vhd.md)**
- **[Červené klobouk Enterprise Linux](virtual-machines-linux-redhat-create-upload-vhd.md)**
- **[SLES & openSUSE](virtual-machines-linux-suse-create-upload-vhd.md)**
- **[Se systémem Ubuntu](virtual-machines-linux-create-upload-ubuntu.md)**

Ve zbývající části tohoto článku se zaměřuje na obecné pokyny pro spuštění Linux rozdělení na Azure.


## <a name="general-linux-installation-notes"></a>Poznámky k instalaci obecné Linux ##

- Formát VHDX není podporován v Azure pouze **pevnou virtuální pevný disk**.  Disk můžete převést do formátu virtuální pevný disk pomocí Hyper-V správce nebo rutinu převést virtuální pevný disk. Pokud používáte VirtualBox znamená to, výběr **pevnou velikost** namísto výchozí dynamicky přidělený při vytváření disku.

- Při instalaci systému Linux je *vhodné* používat standardní oddíly spíše než LVM (často výchozí nastavení pro mnoho instalace). To bude konfliktům LVM název s klonovaný VMs, zejména pokud OS disku je třeba připojit k jiné identickými OM návody na řešení. [LVM](virtual-machines-linux-configure-lvm.md) nebo [RAID](virtual-machines-linux-configure-raid.md) lze na dat discích.

- Podpora jádra připojení systémy souborů UDF je povinný. Při prvním spuštění na Azure předána zřizovací konfigurace OM Linux pomocí formátu UDF média, která je připojen k hosta. Agent Azure Linux musíte mít připojení souborů UDF její konfiguraci a zřízení OM.

- Verze jádra Linux pod 2.6.37 nepodporují NUMA na Hyper-V s OM větších. Tento problém především dopady starší distribuce pomocí odesílání dat červené klobouk 2.6.32 jádra a byla pevně v RHEL 6.6 (jádra 504 2.6.32). Systémy vlastní jádra starší než 2.6.37, nebo na základě RHEL jádra starší než 2.6.32-504 musíte nastavit parametr spuštění `numa=off` na příkazového řádku v grub.conf jádra. Další informace najdete v článku červené klobouk [436883 znalostní bázi Knowledge Base](https://access.redhat.com/solutions/436883).

- Konfigurovat oddíl vyměňovat na disku s operačním systémem. Vytvoření souboru vyměňovat na disku dočasné zdroje je možné konfigurovat agenta Linux.  Další informace najdete v následujících krocích.

- Všechny VHD musí mít velikosti, jež jsou násobky čísla 1 MB.


### <a name="installing-linux-without-hyper-v"></a>Instalace Linux bez Hyper-V ###

V některých případech Linux instalační nemusí zahrnout ovladačů pro Hyper-V počáteční disku RAM (initrd nebo initramfs) Pokud zjistí, že je spuštěn Hyper-V prostředí.  Při použití různých virtualizace systému (tedy Virtualbox KVM, atd.) Příprava obrázek Linux, budete muset znovu vytvořit initrd zajistit, aby alespoň `hv_vmbus` a `hv_storvsc` moduly jádra jsou dostupné na počáteční disku RAM.  Toto je známý problém aspoň systémech podle nadřazeného červené klobouk rozdělení.

Mechanismus pro opětovné vytvoření initrd nebo initramfs obrázku se může lišit v závislosti na hodnotu rozdělení. V dokumentaci k vaší rozdělení nebo podpory k správného řízení.  Tady je příklad informace týkající se znovu vytvořit pomocí initrd `mkinitrd` nástroje:

Nejdřív obecnějším údajům na existující obrázek initrd:

    # cd /boot
    # sudo cp initrd-`uname -r`.img  initrd-`uname -r`.img.bak

Pak znovu vytvořit initrd s `hv_vmbus` a `hv_storvsc` moduly jádra:

    # sudo mkinitrd --preload=hv_storvsc --preload=hv_vmbus -v -f initrd-`uname -r`.img `uname -r`


### <a name="resizing-vhds"></a>Změna velikosti VHD ###

Virtuální pevný disk obrázky na Azure musí mít virtuální velikost zarovnané 1MB.  Obvykle VHD vytvořené pomocí Hyper-V už zarovnání správně.  Pokud není správně zarovnaný virtuální pevný disk může při pokusu o vytvoření *Obrázek* ze svého virtuální pevný disk zobrazit chybová zpráva podobná této:

    "The VHD http://<mystorageaccount>.blob.core.windows.net/vhds/MyLinuxVM.vhd has an unsupported virtual size of 21475270656 bytes. The size must be a whole number (in MBs).”

Aby k tomu nedošlo, můžete změnit velikost OM pomocí účtu správce pro Hyper-V konzole nebo rutiny prostředí Powershell [Změny velikosti virtuální pevný disk](http://technet.microsoft.com/library/hh848535.aspx) .  Pokud používáte v prostředí Windows, je vhodné použít qemu img převést (v případě potřeby) a změnit velikost virtuální pevný disk.

> [AZURE.NOTE] Je známý problém verze qemu img > = 2.2.1, jehož výsledkem nesprávně formátované virtuální pevný disk. Tento problém byl odstraněn v QEMU 2.6. Doporučujeme použít qemu img 2.2.0 nebo nižší nebo aktualizovat 2.6 nebo vyšší. Odkaz: https://bugs.launchpad.net/qemu/+bug/1490611.


 1. Změna velikosti virtuální pevný disk přímo pomocí nástroje, například `qemu-img` nebo `vbox-manage` nelze spustit virtuální pevný disk, můžete mít.  Takže doporučuje se první převést virtuální pevný disk na obrázek nepoužitý disk.  Pokud obrázek OM byl vytvořen jako NEZPRACOVANÁ bitové (výchozí nastavení pro některé hypervisory například KVM) může přeskočit tento krok:

        # qemu-img convert -f vpc -O raw MyLinuxVM.vhd MyLinuxVM.raw

 2. Výpočet požadovaná velikost obrázku disku zajistit, že velikost virtuální zarovnán 1MB.  Následující skriptu prostředí flám může pomoci s tímto.  Skript používá "`qemu-img info`" k určení virtuální velikost obrázku disku a následnému přepočtu velikost další 1 MB:

        rawdisk="MyLinuxVM.raw"
        vhddisk="MyLinuxVM.vhd"

        MB=$((1024*1024))
        size=$(qemu-img info -f raw --output json "$rawdisk" | \
               gawk 'match($0, /"virtual-size": ([0-9]+),/, val) {print val[1]}')

        rounded_size=$((($size/$MB + 1)*$MB))
        echo "Rounded Size = $rounded_size"

 3. Změna velikosti nepoužitý disk pomocí $rounded_size podle výše uvedených skriptu:

        # qemu-img resize MyLinuxVM.raw $rounded_size

 4. Teď převeďte nepoužitý disk zpět pevnou velikostí virtuálního pevného disku:

        # qemu-img convert -f raw -o subformat=fixed -O vpc MyLinuxVM.raw MyLinuxVM.vhd



## <a name="linux-kernel-requirements"></a>Požadavky na Linux jádra ##

Ovladače Linux Integration Services (seznam) pro Hyper-V a Azure jsou přispěla přímo do odesílání dat jádra Linux. Mnoho distribuce, které obsahují poslední verzi jádra Linux (tedy 3.x) se už máte k dispozici tyto ovladače nebo jinak přeneseny zpět verze těchto ovladačů poskytnout jejich jádra.  Tyto ovladače neustále aktualizované nadřazeného jádra s funkcemi a nové opravy, pokud je to možné je vhodné spustit [potvrzeného rozdělení](virtual-machines-linux-endorsed-distros.md) zahrnující tyto opravy a aktualizace.

Pokud používáte varianty červené klobouk Enterprise Linux verze **6.0 6.3**, budete muset nainstalovat nejnovější ovladače seznam pro Hyper-V. Ovladače najdete [na tomto místě](http://go.microsoft.com/fwlink/p/?LinkID=254263&clcid=0x409). RHEL **6.4 +** (a deriváty) seznam jsou už součástí jádra a proto jsou žádné další balíčků potřebné ke spuštění tyto systémy na Azure.

Pokud vlastní jádra je potřeba, doporučujeme používat nejnovější verzi jádra (tedy **3.8 +**). Vhodným rozdělení nebo Dodavatelé, kteří spravovat svoje vlastní jádra některé plánování řízené úsilí bude muset pravidelně backport ovladače seznam z nadřazeného jádra do vlastní jádra.  I když už používáte relativně nejnovější verzi jádra, důrazně doporučujeme ke sledování nadřazeného opravy seznam ovladače a backport můžou být podle potřeby. Umístění souborů seznam ovladač zdroje je k dispozici v souboru [programu](https://git.kernel.org/cgit/linux/kernel/git/torvalds/linux.git/tree/MAINTAINERS) ve stromu Linux jádra zdroje:

    F:  arch/x86/include/asm/mshyperv.h
    F:  arch/x86/include/uapi/asm/hyperv.h
    F:  arch/x86/kernel/cpu/mshyperv.c
    F:  drivers/hid/hid-hyperv.c
    F:  drivers/hv/
    F:  drivers/input/serio/hyperv-keyboard.c
    F:  drivers/net/hyperv/
    F:  drivers/scsi/storvsc_drv.c
    F:  drivers/video/fbdev/hyperv_fb.c
    F:  include/linux/hyperv.h
    F:  tools/hv/

Velmi minimální absence následujících oprav byly známé způsobuje potíže s Azure a tak tyto musí být součástí jádra. Tady je podle ne prostředky vyčerpávající nebo dokončení všech rozdělení:

- [ata_piix: pozdržet disků ovladačů pro Hyper-V ve výchozím nastavení](https://git.kernel.org/cgit/linux/kernel/git/torvalds/linux.git/commit/drivers/ata/ata_piix.c?id=cd006086fa5d91414d8ff9ff2b78fbb593878e3c)
- [miniport storvsc: účtu na cestě paketů v parametru path obnovit](https://git.kernel.org/cgit/linux/kernel/git/torvalds/linux.git/commit/drivers/scsi/storvsc_drv.c?id=5c1b10ab7f93d24f29b5630286e323d1c5802d5c)
- [miniport storvsc: Vyhněte se použití WRITE_SAME](https://git.kernel.org/cgit/linux/kernel/git/next/linux-next.git/commit/drivers/scsi/storvsc_drv.c?id=3e8f4f4065901c8dfc51407e1984495e1748c090)
- [miniport storvsc: zakázat NAPIŠTE stejné RAID a ovladače adaptér virtuální Host (hostitel)](https://git.kernel.org/cgit/linux/kernel/git/next/linux-next.git/commit/drivers/scsi/storvsc_drv.c?id=54b2b50c20a61b51199bedb6e5d2f8ec2568fb43)
- [miniport storvsc: ukazatel NULL přistoupit přes ukazatel fix](https://git.kernel.org/cgit/linux/kernel/git/next/linux-next.git/commit/drivers/scsi/storvsc_drv.c?id=b12bb60d6c350b348a4e1460cd68f97ccae9822e)
- [miniport storvsc: vyzvánění vyrovnávací paměť chyby, můžete mít ukotvení vstupu a výstupu](https://git.kernel.org/cgit/linux/kernel/git/next/linux-next.git/commit/drivers/scsi/storvsc_drv.c?id=e86fb5e8ab95f10ec5f2e9430119d5d35020c951)
- [scsi_sysfs: ochrana proti dvojité provádění __scsi_remove_device](https://git.kernel.org/cgit/linux/kernel/git/next/linux-next.git/commit/drivers/scsi/scsi_sysfs.c?id=be821fd8e62765de43cc4f0e2db363d0e30a7e9b)


## <a name="the-azure-linux-agent"></a>Azure Linux Agent ##

[Azure Linux Agent](virtual-machines-linux-agent-user-guide.md) (waagent) je potřeba se správně zřídit Linux virtuálního počítače v Azure. Získáte na nejnovější verzi, problémy s souboru nebo odeslání vyžádané požadavky na [repo Linux Agent GitHub](https://github.com/Azure/WALinuxAgent).

- Linux agent vydání klikněte v části licence Apache 2.0. Mnoho distribuce už poskytnout ot nebo deb balíčků agenta, a to v některých případech to lze nainstalovat a aktualizují malé plánování řízené úsilí.

- Azure Linux Agent vyžaduje Python v2.6 +.

- Agent také vyžaduje modul python pyasn1. Většina distribuce poskytuje samostatný balíček, který je možné nainstalovat.

- V některých případech nemusí být kompatibilní s NetworkManager agenta Azure Linux. Spoustu ot/Deb balíčků poskytovanou distribuce konfigurace NetworkManager jako konflikt waagent zabalit a tedy odinstaluje NetworkManager při instalaci balíček agenta Linux.


## <a name="general-linux-system-requirements"></a>Obecné Linux systémové požadavky ##

- Úprava řádku spouštěcí jádra GRUB nebo GRUB2 zahrnout následujících parametrů. Také zajistíte tak všechny konzoly zprávy se odesílají první sériové port, který může pomoci Azure podporu s ladění problémy:

        console=ttyS0,115200n8 earlyprintk=ttyS0,115200 rootdelay=300

    Také zajistíte tak všechny konzoly zprávy se odesílají první sériové port, který může pomoci Azure podporu s ladění problémy.

    Kromě výše uvedené doporučujeme *Odebrat* následujících parametrů Pokud existují:

        rhgb quiet crashkernel=auto

    Grafické a tichý spouštěcí nejsou užitečné v cloudu prostředí, které chceme všechny protokoly nechat zasílat sériový port.

    `crashkernel` Možnost může být vlevo nakonfigurované podle potřeby, ale Všimněte si, že tento parametr bude Zkraťte dostupnou pamětí v OM tak, že 128MB a více, které mohou způsobovat v menší velikosti OM.

- Instalace Azure Linux Agent

    Azure Linux Agent je nutný pro zřizování Linux obrázek na Azure.  Mnoho distribuce poskytují agent jako balíček ot nebo Deb (balíček obvykle jmenuje "WALinuxAgent" nebo "walinuxagent").  Agent můžete taky nainstalovat ručně podle kroků v [Průvodce Agent Linux](virtual-machines-linux-agent-user-guide.md).

- Ujistěte se, že SSH server nainstalovali a nakonfigurovali zahájíte při spuštění.  Je to obvykle výchozí.

- Nevytvářejte vyměňovat místa na disku OS

    Azure Linux Agent můžete nakonfigurovat automaticky odkládací prostor pomocí místních zdrojů disku, který je připojen k OM po zajištění služby na Azure. Všimněte si, že disku místních zdrojů je *dočasný* disk a může vyprázdní při poskytování zrušeno OM. Po instalaci Azure Linux Agent (viz předchozím kroku), upravte pomocí následujících parametrů v /etc/waagent.conf řádně podporovat:

        ResourceDisk.Format=y
        ResourceDisk.Filesystem=ext4
        ResourceDisk.MountPoint=/mnt/resource
        ResourceDisk.EnableSwap=y
        ResourceDisk.SwapSizeMB=2048    ## NOTE: set this to whatever you need it to be.

- Jako poslední krok spusťte následující příkazy pro deprovision virtuálního počítače:

        # sudo waagent -force -deprovision
        # export HISTSIZE=0
        # logout

    >[AZURE.NOTE] Na Virtualbox může zobrazit chybová zpráva po spuštění "waagent-vynutit - identitách": `[Errno 5] Input/output error`. Tato chybová zpráva není důležitá a můžete ho ignorovat.

- Musíte se pak vypnutí virtuálního počítače a nahrajte virtuální pevný disk na Azure.
