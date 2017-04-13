<properties 
    pageTitle="Konfigurace LVM na virtuální počítač Linux | Microsoft Azure" 
    description="Informace o konfiguraci LVM Linux v Azure." 
    services="virtual-machines-linux" 
    documentationCenter="na" 
    authors="szarkos"  
    manager="timlt" 
    editor="tysonn"
    tag="azure-service-management,azure-resource-manager" />

<tags 
    ms.service="virtual-machines-linux" 
    ms.workload="infrastructure-services" 
    ms.tgt_pltfrm="vm-linux" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/24/2016" 
    ms.author="szark"/>


# <a name="configure-lvm-on-a-linux-vm-in-azure"></a>Konfigurace LVM na Linux OM v Azure

Tento dokument popisuje postup při konfiguraci logické hlasitost správce (LVM) v počítači Azure virtuální. Když je možné konfigurovat LVM na libovolném disku připojené k virtuální počítač, ve výchozím nastavení většina cloudu obrázky nemají LVM nakonfigurovaný na disku s operačním systémem. Toto je předejít problémům s duplicitními hlasitost skupinami Pokud disku OS je někdy připojen k jiné OM stejné distribuce a typ, tedy během obnovení situace. Proto doporučujeme pouze pro použití LVM na discích data.


## <a name="linear-vs-striped-logical-volumes"></a>Lineární porovnání Prokládané logické svazky

LVM mohou sloužit k sloučení počet discích do jednoho úložiště hlasitost. Ve výchozím nastavení LVM obvykle vytvoří lineární logické svazky, což znamená, že je společně spojeny fyzické úložiště. V tomto případě operace pro čtení i zápis obvykle jenom odešle na jeden disk. Naopak můžete taky vytvoříme Prokládané logické svazky kde čtení a zápis jsou úměrně více disků obsažené ve skupině objem (tedy podobně jako 0). Z hlediska výkonu, které bude pravděpodobně budete chtít prokládanou logické svazky tak, aby čtení a zápis využívání všech disků připojená data.

Tohoto dokumentu bude popisují, jak zkombinovat několik disků data do jednoho hlasitost skupiny a pak vytvořte Prokládané logické hlasitost. Postupem jsou poněkud generalized pro práci s většina rozdělení. Ve většině případů nástroje a pracovní postupy pro správu LVM na Azure nejsou zásadně liší od jiných prostředí. Jako obvykle také najdete dodavatele Linux si přečtěte následující dokumentaci a osvědčené postupy pro používání LVM konkrétní rozdělení.


## <a name="attaching-data-disks"></a>Připojení dat disků
Obvykle jednu budete chtít projekt začít dvou nebo více discích prázdné dat při použití LVM. Podle potřeby vstupu a výstupu, můžete připojit disků, které jsou uložené v naší standardní úložiště s až 500 vstupu a výstupu/ps pro na disku nebo naše Premium úložiště s až 5 000 vstupu a výstupu/ps na disku. Tento článek nebude obsahují podrobnosti o tom, jak zřídit a připojíte disků dat k virtuálního počítače Linux. Najdete Microsoft Azure článek [Připojit disk](virtual-machines-linux-add-disk.md) podrobné informace o tom, jak připojit disku prázdné dat do virtuálního počítače Linux na Azure.

## <a name="install-the-lvm-utilities"></a>Instalace nástroje pro práci s LVM

- **Se systémem Ubuntu**

        # sudo apt-get update
        # sudo apt-get install lvm2

- **RHEL, CentOS a Oracle Linux**

        # sudo yum install lvm2

- **SLES 12 a openSUSE**

        # sudo zypper install lvm2

- **SLES 11**

        # sudo zypper install lvm2

    Na SLES11, musíte taky upravovat /etc/sysconfig/lvm a nastavit `LVM_ACTIVATED_ON_DISCOVERED` umožňující "":

        LVM_ACTIVATED_ON_DISCOVERED="enable" 


## <a name="configure-lvm"></a>Konfigurace LVM
V této příručce bude předpokladu, že jste přiřadili tři discích dat, které budeme označovat jako `/dev/sdc`, `/dev/sdd` a `/dev/sde`. Všimněte si, že tyto nemusí být vždy stejné cestami ve vaší OM. Můžete spustit "`sudo fdisk -l`" nebo seznam dostupných disků podobné příkazu.

1. Příprava fyzické svazky:

        # sudo pvcreate /dev/sd[cde]
          Physical volume "/dev/sdc" successfully created
          Physical volume "/dev/sdd" successfully created
          Physical volume "/dev/sde" successfully created


2.  Umožňuje vytvořte skupinu hlasitost. V tomto příkladu jsme voláte hlasitost skupiny "dat – vg01":

        # sudo vgcreate data-vg01 /dev/sd[cde]
          Volume group "data-vg01" successfully created


3. Vytvoření logické svazky. Příkaz pod jsme vytvoří jednu logické hlasitost s názvem "dat – lv01" rozsahu skupině celého svazku, ale Všimněte si, že je také možné vytvořit více logické svazky ve skupině hlasitost.

        # sudo lvcreate --extents 100%FREE --stripes 3 --name data-lv01 data-vg01
          Logical volume "data-lv01" created.


4. Formátování logický svazku

        # sudo mkfs -t ext4 /dev/data-vg01/data-lv01

  >[AZURE.NOTE] Použití SLES11 "-t ext3" místo ext4. SLES11 podporuje pouze přístup k ext4 filesystems jen pro čtení.


## <a name="add-the-new-file-system-to-etcfstab"></a>Přidání nového systému souborů /etc/fstab

**Upozornění:** Nesprávně úpravy souboru /etc/fstab může vést k systému nelze spustit. Pokud jistí, najdete informace v nápovědě k funkci rozdělení informace o tom, jak správně upravit tento soubor. Doporučujeme také vytvořené záložní kopii souboru /etc/fstab před úpravou.

1. Vytvoření bodu požadované připojení k systému souborů nové například:

        # sudo mkdir /data


2. Vyhledejte cestu logické hlasitosti

        # lvdisplay
        --- Logical volume ---
        LV Path                /dev/data-vg01/data-lv01
        ....


3. Otevřete /etc/fstab v textovém editoru a přidejte položku pro nové systému souborů, například:

        /dev/data-vg01/data-lv01  /data  ext4  defaults  0  2

    Pak uložte a zavřete /etc/fstab.


4. Otestujte správnost položka fstab/atd /:

        # sudo mount -a

    Pokud tento příkaz má za následek chybovou zprávu Zkontrolujte syntaxi v souboru fstab/atd /.

    Další spuštění `mount` příkaz zajistit připojení k systému souborů:

        # mount
        ......
        /dev/mapper/data--vg01-data--lv01 on /data type ext4 (rw)


5. (Volitelné) Bezporuchový spouštěcí parametrů v /etc/fstab

    Zahrnout mnoho distribuce buď `nobootwait` nebo `nofail` připojit parametrů, které může být přidán do souboru fstab/atd /. Tyto parametry umožňoval k chybám při připojení k systému konkrétní souborů a povolit systému Linux nadále spuštění i v případě, že se nepodařilo správně připojit RAID systému souborů. Zkontrolujte informace v nápovědě k vaší distribuční Další informace o těchto parametrů.

    Příklad (se systémem Ubuntu):

        /dev/data-vg01/data-lv01  /data  ext4  defaults,nobootwait  0  2
