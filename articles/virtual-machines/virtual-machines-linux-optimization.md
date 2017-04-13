<properties
    pageTitle="Optimalizace Linux OM na Azure | Microsoft Azure"
    description="Přečtěte si několik tipů pro optimalizaci aby zkontrolovala, jestli že jste nastavili Linux OM optimální výkon Azure"
    keywords="Linux virtuálního počítače linux virtuální počítač se systémem ubuntu virtuálního počítače" 
    services="virtual-machines-linux"
    documentationCenter=""
    authors="rickstercdn"
    manager="timlt"
    editor="tysonn"
    tags="azure-resource-manager" />

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/06/2016"
    ms.author="rclaus"/>

# <a name="optimize-your-linux-vm-on-azure"></a>Optimalizace Linux OM na Azure

Vytváření virtuálních počítačů Linux (OM) je snadné z příkazového řádku nebo z portálu. Tento kurz se dozvíte, jak zajistit jste ho vytvořili optimalizovat výkon platformy Microsoft Azure. Toto téma používá OM serveru se systémem Ubuntu, ale můžete taky vytvořit Linux virtuálního počítače pomocí [vlastní obrázky jako šablony](virtual-machines-linux-create-upload-generic.md).  

## <a name="prerequisites"></a>Zjistit předpoklady pro

Toto téma předpokládá už máte pracovní Azure předplatného ([bezplatná zkušební registrace](https://azure.microsoft.com/pricing/free-trial/)), [nainstalovaný Azure rozhraní příkazového řádku](../xplat-cli-install.md) a už zřízení virtuálního počítače do vašeho předplatného Azure. Před tím cokoli s Azure – je potřeba ověřit k předplatnému. Postup s Azure rozhraní příkazového řádku tu jednoduše napsat `azure login` ke spuštění interaktivní procesu. 

## <a name="azure-os-disk"></a>Azure disku OS

Po vytvoření Linux OM v Azure má dvě disků s ním spojené. /dev/sda je OS disk, které /dev/sdb dočasné disku.  Nepoužívejte hlavních OS disku (/ vývojáře/sda) něco kromě operačního systému, jako je optimalizována pro rychlé OM spuštění a neposkytuje dobrý výkon u svého pracovního vytížení. Chcete připojit jeden nebo více disků OM získat trvalý a optimalizované úložiště pro vaše data. 

## <a name="adding-disks-for-size-and-performance-targets"></a>Přidání disků pro cíle velikost a výkon 

Na základě velikosti OM můžete připojit až 16 další disk na A řady, 32 disků na základě série D a 64 disků na základě série G počítače – každý až 1 TB velikost. Podle potřeby na místa a procesorů požadavky přidejte navíc discích. Každý disk má výkonu cíl 500 procesorů standardní úložiště a až 5000 procesorů na disku pro Premium úložiště.  Další informace o disků Premium úložiště najdete v příručce [Premium úložiště: výkonné úložiště pro Azure VMs](../storage/storage-premium-storage.md)

Abyste dosáhli nejvyšší procesorů na místo, kam jejich mezipaměti nastavení "Jen pro čtení" nebo "Žádná" discích Premium úložiště, musíte zakázat "překážky" při připojování systému souborů v Linux. Protože jsou zápisy Premium úložiště záložní disků trvalých nastavení mezipaměti nepotřebujete překážky.

- Používáte **reiserFS**, můžete zakázat pomocí možností připojení překážky "překážku = žádný" (umožňující překážky použít "překážku = vyprázdnění")
- Pokud používáte **ext3/ext4**, zakázat překážky pomocí možností připojení "překážku = 0" (umožňující překážky použít "překážku = 1")
- Pokud používáte **XFS**, zakázat překážky pomocí možností připojení "nobarrier" (pro povolení překážky, použijte možnost "překážku")

## <a name="storage-account-considerations"></a>Důležité informace o účtu úložiště

Při vytváření Linux OM v Azure by měl zkontrolujte, že připojíte disků z účtů úložiště umístěných ve stejné oblasti jako OM zajistit blízko a minimalizace latence sítě.  Každý účet standardní úložiště má maximálně 20 k procesorů a kapacitu velikost 500 TB.  To funguje 40 často používané disků včetně disku OS a všechny disků dat, které vytvoříte. U účtů Premium úložiště není omezena maximální procesorů ale existuje limit velikosti 32 TB. 

Když se zabývá maximum procesorů pracovního vytížení a zvolili standardní úložiště disku, může potřebujete rozdělení disků u více účtů úložiště, aby zkontrolovala, jestli že ještě přístupů 20 000 procesorů limit úložiště standardní účty. Vaše OM může obsahovat kombinaci disků z přes různé úložiště účtů a jejich typů úložiště k dosažení optimálního konfigurace. 

## <a name="your-vm-temporary-drive"></a>Dočasné OM jednotka

Ve výchozím nastavení při vytváření OM, Azure umožňuje s operačním systémem disku (/ vývojáře/sda) a dočasné disku (/ vývojáře/sdb).  Všechny další disk, které přidáte zobrazit jako /dev/sdc, /dev/sdd, /dev/sde a tak dál. Všechna data na disku dočasné (/ vývojáře/sdb) nejsou trvalé a může dojít ke ztrátě zvláštní události jako OM Změna velikosti, nové nasazení, nebo údržbu vynutí restart vaší OM.  Velikost a zadejte dočasné disku souvisí s OM velikost, kterou jste zvolili při nasazení. V případě jednotlivých velikost premium VMs (Pošta, G a DS_V2 řad) dočasné jednotka získáváte místní SSD další výkonu až 48 k procesorů. 

## <a name="linux-swap-file"></a>Odkládací Linux soubor

Pokud Azure OM od obrázků se systémem Ubuntu nebo jádro operačního systému, můžete použít CustomData cloudu konfigurace odešlete inicializace cloudu. Pokud jste [nahráli vlastní obrázek Linux](virtual-machines-linux-upload-vhd.md) využívající cloudové spuštění, můžete taky nakonfigurovat odkládací oddíl pomocí inicializace cloudu.

V systémem Ubuntu cloudu obrázky je nutné použít cloudu inicializace nakonfigurujte vyměňovat oddíl. Podívejte se na následující stránku na wikiwebu systémem Ubuntu podrobné informace: [AzureSwapPartitions](https://wiki.ubuntu.com/AzureSwapPartitions).

Obrázky bez podpory cloudu inicializace OM obrázky nasazeny z Azure Marketplace mít agenta Linux OM integrovaný s operačním systémem. Tento agent umožňuje OM chcete provést interakci s různé služby Azure. Za předpokladu, že jste nasadili obrázku standardních z Azure Marketplace, je by potřeba se správně nakonfigurovat nastavení Linux vyměňovat souborů takto:

Vyhledání a změny dvě položky v souboru **/etc/waagent.conf** . Budou řídit existenci vyhrazené odkládací soubor a velikost souboru vyměňovat. Parametry hledáte upravit `ResourceDisk.EnableSwap=N` a`ResourceDisk.SwapSizeMB=0` 

Potřebujete změnit následujícím způsobem:

* ResourceDisk.EnableSwap=Y
* ResourceDisk.SwapSizeMB={size MB vlastním potřebám} 

Jakmile jste provedli změny, musíte se restartování waagent nebo Linux OM tak, aby odrážely provedené změny.  Znáte byly provedeny změny a odkládací soubor byl vytvořen při použití `free` příkaz zobrazíte volného místa. V příkladu níže obsahuje soubor vyměňovat o velikosti 512MB vytvořili důsledku úpravách souboru waagent.conf.

    admin@mylinuxvm:~$ free
                total       used       free     shared    buffers     cached
    Mem:       3525156     804168    2720988        408       8428     633192
    -/+ buffers/cache:     162548    3362608
    Swap:       524284          0     524284
    admin@mylinuxvm:~$
 
## <a name="io-scheduling-algorithm-for-premium-storage"></a>Plánování algoritmus vstupu a výstupu Premium úložiště

S 2.6.18 Linux jádra výchozí vstupu a výstupu plánování algoritmus změnil z konečného termínu na CFQ (úplně veletrhu fronty algoritmus). Vzorky vstupu a výstupu RAM není zanedbatelný rozdíl výkonu rozdíly mezi CFQ a konečného termínu.  Na základě SSD disků kde vzorek vstupu a výstupu disku je převážně sekvenční přechod zpátky do algoritmu nedostupný (NoOp) nebo termín můžete dosáhnout lepší výkon vstupu a výstupu.

### <a name="view-the-current-io-scheduler"></a>Zobrazit aktuální Plánovač vstupu a výstupu

Použijte tento příkaz:  

    admin@mylinuxvm:~# cat /sys/block/sda/queue/scheduler

Zobrazí se následující výstup, který označuje aktuální plánovač.  

    noop [deadline] cfq

###<a name="change-the-current-device-devsda-of-io-scheduling-algorithm"></a>Změna na aktuální zařízení (/ vývojáře/sda) vstupu a výstupu plánování algoritmus

Pomocí následujících příkazů:  

    azureuser@mylinuxvm:~$ sudo su -
    root@mylinuxvm:~# echo "noop" >/sys/block/sda/queue/scheduler
    root@mylinuxvm:~# sed -i 's/GRUB_CMDLINE_LINUX=""/GRUB_CMDLINE_LINUX_DEFAULT="quiet splash elevator=noop"/g' /etc/default/grub
    root@mylinuxvm:~# update-grub

>[AZURE.NOTE] Toto nastavení pro /dev/sda samotný není užitečné. Je třeba nastavit na všech dat discích kde vstupu a postupné výstupu dominuje vzorek vstupu a výstupu.  

Měli byste vidět následující výstup označovat tak, že tento grub.cfg byl úspěšně znovu a že výchozí plánovač aktualizovaný nedostupný (NoOp).  

    Generating grub configuration file ...
    Found linux image: /boot/vmlinuz-3.13.0-34-generic
    Found initrd image: /boot/initrd.img-3.13.0-34-generic
    Found linux image: /boot/vmlinuz-3.13.0-32-generic
    Found initrd image: /boot/initrd.img-3.13.0-32-generic
    Found memtest86+ image: /memtest86+.elf
    Found memtest86+ image: /memtest86+.bin
    done

Pro rodinu rozdělení Redhat stačí tento příkaz:   

    echo 'echo noop >/sys/block/sda/queue/scheduler' >> /etc/rc.local

## <a name="using-software-raid-to-achieve-higher-iops"></a>Použití softwaru RAID k dosažení vyšší můžu / Ops

Pokud svého pracovního vytížení vyžadují více procesorů, než lze zadat jeden disk, budete muset používat software konfiguraci RAID více disků. Protože Azure ještě provádí odolnost proti chybám disku ve vrstvě místní struktury, můžete dosáhnout nejvyšší úrovně výkonu z konfigurace prokládání RAID 0.  Budete muset zřízení a vytvoření nových disků v Azure prostředí a připojte k Linux OM před oddílů, formátování a připojování jednotky.  Další informace o konfiguraci nastavení RAID software na Linux OM v azure najdete v dokumentu **[Konfigurace Software RAID na Linux](virtual-machines-linux-configure-raid.md)** .


## <a name="next-steps"></a>Další kroky

Nezapomeňte, že se všechny optimalizace diskuse, potřebujete k provádění testů před a po každé změně změřit jaký vliv bude mít změny.  Optimalizace je proces krok za krokem, který bude mít odlišné výsledky v různých počítačích ve vašem prostředí.  Co vyhovovat konfigurací nemusí fungovat pro ostatní.

Některé užitečné odkazy na další materiály: 

- [Úložiště Premium: Výkonné úložiště pro pracovního vytížení Azure virtuálního počítače](../storage/storage-premium-storage.md)
- [Azure Linux Agent User Guide](virtual-machines-linux-agent-user-guide.md)
- [Optimalizace výkonu MySQL na Azure Linux VMs](virtual-machines-linux-classic-optimize-mysql.md)
- [Konfigurace Software RAID na Linux](virtual-machines-linux-configure-raid.md)
