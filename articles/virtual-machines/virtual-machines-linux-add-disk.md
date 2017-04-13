<properties
    pageTitle="Přidání disku Linux OM | Microsoft Azure"
    description="Naučte se přidat trvalý disk do Linux OM"
    keywords="virtuální počítač Linux přidat zdroje disk"
    services="virtual-machines-linux"
    documentationCenter=""
    authors="rickstercdn"
    manager="timlt"
    editor="tysonn"
    tags="azure-resource-manager" />

<tags
    ms.service="virtual-machines-linux"
    ms.topic="article"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.date="09/06/2016"
    ms.author="rclaus"/>

# <a name="add-a-disk-to-a-linux-vm"></a>Přidání disku Linux OM

Tento článek ukazuje, jak připojit trvalý disku svého v angličtině tak, že můžete chránit vaše data - i v případě, že vaše OM reprovisioned kvůli údržby nebo změna velikosti. Pokud chcete přidat na disk, musíte [Azure rozhraní příkazového řádku](../xplat-cli-install.md) v režimu správce prostředků nakonfigurovat (`azure config mode arm`).  

## <a name="quick-commands"></a>Rychlé příkazy

V následujících příkladech příkaz nahradit hodnoty mezi &lt; a &gt; s hodnotami z vlastní prostředí.

```bash
azure vm disk attach-new <myuniquegroupname> <myuniquevmname> <size-in-GB>
```

## <a name="attach-a-disk"></a>Připojte disk

Připojení nového disku je snadné. Typ `azure vm disk attach-new <myuniquegroupname> <myuniquevmname> <size-in-GB>` a vytvořte nový disk GB pro vaše OM. Pokud úložiště účet není výslovně identifikovat, disku, kterou vytvoříte se uloží do jednoho účtu úložiště, kde jsou uložená na disku s operačním systémem.  By měl vypadat nějak takto:

```bash
azure vm disk attach-new myuniquegroupname myuniquevmname 5
```

Výstup

```bash
info:    Executing command vm disk attach-new
+ Looking up the VM "myuniquevmname"
info:    New data disk location: https://cliexxx.blob.core.windows.net/vhds/myuniquevmname-20150526-0xxxxxxx43.vhd
+ Updating VM "myuniquevmname"
info:    vm disk attach-new command OK
```

## <a name="connect-to-the-linux-vm-to-mount-the-new-disk"></a>Připojení k OM Linux připojení nového disku

> [AZURE.NOTE] Toto téma se připojí k OM pomocí uživatelská jména a hesla. Použití veřejných a privátních klíčů dvojice komunikovat s vaší OM, najdete v článku [jak používat SSH s Linux na Azure](virtual-machines-linux-mac-create-ssh-keys.md). Úprava připojení **SSH** VMs vytvořená pomocí `azure vm quick-create` příkazu pomocí `azure vm reset-access` příkaz Obnovit **SSH** přístup úplně, přidat nebo odebrat uživatele nebo přidat veřejné soubory klíčů zajistit přístup.

Můžete třeba SSH do Azure OM do oddílu, formátování a připojení nového disku, abyste ho mohli použít OM Linux. Pokud si nejste zkušenosti s připojením pomocí **ssh**, příkaz je představována `ssh <username>@<FQDNofAzureVM> -p <the ssh port>`a vypadá následovně:

```bash
ssh ops@myuni-westu-1432328437727-pip.westus.cloudapp.azure.com -p 22
```

Výstup

```bash
The authenticity of host 'myuni-westu-1432328437727-pip.westus.cloudapp.azure.com (191.239.51.1)' can't be established.
ECDSA key fingerprint is bx:xx:xx:xx:xx:xx:xx:xx:xx:x:x:x:x:x:x:xx.
Are you sure you want to continue connecting (yes/no)? yes
Warning: Permanently added 'myuni-westu-1432328437727-pip.westus.cloudapp.azure.com,191.239.51.1' (ECDSA) to the list of known hosts.
ops@myuni-westu-1432328437727-pip.westus.cloudapp.azure.com's password:
Welcome to Ubuntu 14.04.2 LTS (GNU/Linux 3.16.0-37-generic x86_64)

* Documentation:  https://help.ubuntu.com/

System information as of Fri May 22 21:02:32 UTC 2015

System load: 0.37              Memory usage: 2%   Processes:       207
Usage of /:  41.4% of 1.94GB   Swap usage:   0%   Users logged in: 0

Graph this data and manage this system at:
  https://landscape.canonical.com/

Get cloud support with Ubuntu Advantage Cloud Guest:
  http://www.ubuntu.com/business/services/cloud

0 packages can be updated.
0 updates are security updates.

The programs included with the Ubuntu system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Ubuntu comes with ABSOLUTELY NO WARRANTY, to the extent permitted by
applicable law.

ops@myuniquevmname:~$
```

Teď, když jste připojeni k vaší OM, budete chtít připojit disk.  Najděte si nejdřív disku, pomocí `dmesg | grep SCSI` (metoda použijete k zjišťovat nové disku se mohou lišit). V tomto případě vypadá nějak:

```bash
dmesg | grep SCSI
```

Výstup

```bash
[    0.294784] SCSI subsystem initialized
[    0.573458] Block layer SCSI generic (bsg) driver version 0.4 loaded (major 252)
[    7.110271] sd 2:0:0:0: [sda] Attached SCSI disk
[    8.079653] sd 3:0:1:0: [sdb] Attached SCSI disk
[ 1828.162306] sd 5:0:0:0: [sdc] Attached SCSI disk
```

a v případě v tomto tématu `sdc` disk je ten, který chceme. Teď oddílů disku s `sudo fdisk /dev/sdc` – za předpokladu, že v váš případ disk byl `sdc`a primární disk v oddílu 1 a přijměte jiné výchozí hodnoty.

```bash
sudo fdisk /dev/sdc
```

Výstup

```bash
Device contains neither a valid DOS partition table, nor Sun, SGI or OSF disklabel
Building a new DOS disklabel with disk identifier 0x2a59b123.
Changes will remain in memory only, until you decide to write them.
After that, of course, the previous content won't be recoverable.

Warning: invalid flag 0x0000 of partition table 4 will be corrected by w(rite)

Command (m for help): n
Partition type:
   p   primary (0 primary, 0 extended, 4 free)
   e   extended
Select (default p): p
Partition number (1-4, default 1): 1
First sector (2048-10485759, default 2048):
Using default value 2048
Last sector, +sectors or +size{K,M,G} (2048-10485759, default 10485759):
Using default value 10485759
```

Vytvoření oddílu zadáním `p` příkazového řádku:

```bash
Command (m for help): p

Disk /dev/sdc: 5368 MB, 5368709120 bytes
255 heads, 63 sectors/track, 652 cylinders, total 10485760 sectors
Units = sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disk identifier: 0x2a59b123

   Device Boot      Start         End      Blocks   Id  System
/dev/sdc1            2048    10485759     5241856   83  Linux

Command (m for help): w
The partition table has been altered!

Calling ioctl() to re-read partition table.
Syncing disks.
```

A systému souborů do oddílu pomocí příkazu **mkfs** Určuje typ systému souborů a název zařízení. V tomto tématu Pracujeme s programem `ext4` a `/dev/sdc1` shora:

```bash
sudo mkfs -t ext4 /dev/sdc1
```

Výstup

```bash
mke2fs 1.42.9 (4-Feb-2014)
Discarding device blocks: done
Filesystem label=
OS type: Linux
Block size=4096 (log=2)
Fragment size=4096 (log=2)
Stride=0 blocks, Stripe width=0 blocks
327680 inodes, 1310464 blocks
65523 blocks (5.00%) reserved for the super user
First data block=0
Maximum filesystem blocks=1342177280
40 block groups
32768 blocks per group, 32768 fragments per group
8192 inodes per group
Superblock backups stored on blocks:
    32768, 98304, 163840, 229376, 294912, 819200, 884736
Allocating group tables: done
Writing inode tables: done
Creating journal (32768 blocks): done
Writing superblocks and filesystem accounting information: done
```

Teď můžeme vytvořit adresář, který chcete připojit systému souborů pomocí `mkdir`:

```bash
sudo mkdir /datadrive
```

A připojíte pomocí adresáře `mount`:

```bash
sudo mount /dev/sdc1 /datadrive
```

Disk dat je nyní připravena k použití jako `/datadrive`.

```bash
ls
```

Výstup

```bash
bin   datadrive  etc   initrd.img  lib64       media  opt   root  sbin  sys  usr  vmlinuz
boot  dev        home  lib         lost+found  mnt    proc  run   srv   tmp  var
```

Zajistit že jednotku se znovu připojit automaticky po restartování musí být přidán do souboru fstab/atd /. Kromě toho důrazně doporučujeme, aby UUID (všeobecně jedinečný identifikátor) se používá v/atd/fstab odkázat na jednotku, nikoli pouze název zařízení (například `/dev/sdc1`). Pokud operační systém zjistí chyba disk během spouštění, použitím UUID vyhnete nesprávné disku je připojen do zadaného umístění. Pole zbývající data disků pak bude přiřazeno ty stejné ID zařízení. Při hledání UUID nové jednotky, použijte nástroj **blkid** :

```bash
sudo -i blkid
```

Výstup vypadá nějak takto:

```bash
/dev/sda1: UUID="11111111-1b1b-1c1c-1d1d-1e1e1e1e1e1e" TYPE="ext4"
/dev/sdb1: UUID="22222222-2b2b-2c2c-2d2d-2e2e2e2e2e2e" TYPE="ext4"
/dev/sdc1: UUID="33333333-3b3b-3c3c-3d3d-3e3e3e3e3e3e" TYPE="ext4"
```

>[AZURE.NOTE] Nesprávně úpravy souboru **/etc/fstab** může vést k systému nelze spustit. Pokud si jisti, informace v nápovědě k funkci rozdělení informace o tom, jak správně upravit tento soubor. Doporučujeme také vytvořené záložní kopii souboru /etc/fstab před úpravou.

Potom otevřete soubor **/etc/fstab** v textovém editoru:

```bash
sudo vi /etc/fstab
```

V tomto příkladu použijeme hodnotu UUID **/dev/sdc1** zařízení, která byla vytvořená v předchozí kroky a BodPřipojení **/datadrive**. Přidejte následující řádek na konec **/etc/fstab** souboru:

```bash
UUID=33333333-3b3b-3c3c-3d3d-3e3e3e3e3e3e   /datadrive   ext4   defaults,nofail   1   2
```

>[AZURE.NOTE] Později odebrání data disk bez úprav fstab může způsobit OM nepodařilo spustit. Většina distribuce poskytují buď `nofail` a/nebo `nobootwait` fstab možnosti. Tyto možnosti umožňují spuštění i v případě, že disk nepodaří připojit při spuštění systému. Další informace o těchto parametrech naleznete v dokumentaci vaší rozdělení.

>[AZURE.NOTE] Možnost **nofail** zaručuje, že OM spustí i v případě souborů je poškozený nebo disk neexistuje při spuštění. Bez tuto možnost můžete narazit na chování podle [Nelze SSH Linux angličtině důsledku FSTAB chyb](https://blogs.msdn.microsoft.com/linuxonazure/2016/07/21/cannot-ssh-to-linux-vm-after-adding-data-disk-to-etcfstab-and-rebooting/)


### <a name="trimunmap-support-for-linux-in-azure"></a>Střih/UNMAP podpora Linux v Azure
Některé Linux jádra podporují střih/UNMAP operace kliknutím na Zrušit nepoužívané bloky na disku. To je užitečné především ve standardní úložiště informovat Azure, která odstraněné stránky už nejsou platné a můžete zrušit. Když vytvoříte velkých souborů a potom odstraňte je to můžete uložit náklady.

Existují dva způsoby, jak povolit střih podpory OM Linux. Jako obvykle naleznete distribučního doporučený postup:

- Použití `discard` připojit možnost v `/etc/fstab`, například:

        UUID=33333333-3b3b-3c3c-3d3d-3e3e3e3e3e3e   /datadrive   ext4   defaults,discard   1   2

- Případně můžete spustit `fstrim` příkaz ručně z příkazového řádku nebo přidat do crontab pravidelně spuštění:

    **Se systémem Ubuntu**

        # sudo apt-get install util-linux
        # sudo fstrim /datadrive

    **RHEL/CentOS**

        # sudo yum install util-linux
        # sudo fstrim /datadrive

## <a name="troubleshooting"></a>Řešení potíží
[AZURE.INCLUDE [virtual-machines-linux-lunzero](../../includes/virtual-machines-linux-lunzero.md)]

## <a name="next-steps"></a>Další kroky

- Mějte na paměti, že nový disk není k dispozici angličtině Pokud restartuje Pokud napíšete tyto informace do souboru [fstab](http://en.wikipedia.org/wiki/Fstab) .
- Abyste měli jistotu, že je správně nastavený Linux OM, přečtěte si doporučení [optimalizovat výkon počítače Linux](virtual-machines-linux-optimization.md) .
- Rozbalte kapacitu úložiště přidáním dalších disků a [konfigurace RAID](virtual-machines-linux-configure-raid.md) další výkon.
