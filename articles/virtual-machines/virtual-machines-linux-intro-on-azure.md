<properties
    pageTitle="Úvod k Linux v Azure | Microsoft Azure"
    description="Zjistěte, jak používat Linux virtuálních počítačích na Azure."
    services="virtual-machines-linux"
    documentationCenter="python"
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

#<a name="introduction-to-linux-on-azure"></a>Úvod k Linux na Azure

Toto téma obsahuje přehled některých aspektech používání Linux virtuálních počítačích v Azure cloudu. Nasazení Linux virtuálního počítače je jednoduchý proces, který používá obrázek z galerie.


## <a name="authentication-usernames-passwords-and-ssh-keys"></a>Ověření: Uživatelská jména hesla a SSH klíče

Při vytváření virtuálních počítačů Linux na portálu Azure klasické, budete vyzváni k zadání uživatelské jméno, heslo nebo veřejným klíčem SSH. Volba uživatelské jméno pro nasazení Linux virtuálního počítače na Azure podléhá následujících omezení: názvy systémový účet (UID < 100) už prezentace ve počítače virtuální nejsou povolené "root" například.


 - Podívejte se, [vytvořit virtuální počítač Linux](virtual-machines-linux-quick-create-cli.md)
 - Informace [o použití SSH s Linux na Azure](virtual-machines-linux-mac-create-ssh-keys.md)


## <a name="obtaining-superuser-privileges-using-sudo"></a>Získání oprávnění Superuser pomocí`sudo`

Uživatelský účet, který není zadán během nasazení instance virtuálního počítače na Azure je privilegovaných účet. Tento účet nakonfigurovaný agentem Linux Azure mohli zvyšovat oprávnění k používání kořenové (superuser účet) `sudo` nástroj. Po přihlášení pomocí účtu tohoto uživatele, bude moct spouštět příkazy jako kořenovou pomocí syntaxe příkazů

    # sudo <COMMAND>

Volitelně můžete získat kořenové prostředí pomocí **sudo -ne**.

- V tématu [použití oprávnění root na virtuálních počítačích Linux v Azure](virtual-machines-linux-use-root-privileges.md)


## <a name="firewall-configuration"></a>Konfigurace brány firewall

Azure poskytuje příchozí paket filtr, který omezením připojení k porty určené Azure klasické portálu. Ve výchozím nastavení je pouze povolené port SSH. Můžete otevřít dostupný přístup k další porty v počítači virtuální Linux nakonfigurováním koncové body Azure klasické portálu:

 - Viz: [jak nastavit koncové body do virtuálního počítače](virtual-machines-windows-classic-setup-endpoints.md)

Na některých obrázcích Linux v galerii Azure bránu *iptables* firewall ve výchozím nastavení. Pokud budete chtít, mohou k poskytnutí dodatečného filtrování nakonfigurované brány firewall.


## <a name="hostname-changes"></a>Hostname změny

Při nasazení původně instanci Linux obrázek, je nutné zadat název hostitele virtuální počítač. Po spuštění virtuální počítač tohoto názvu hostitele je publikován na servery DNS platformy tak, aby více virtuálních počítačích připojené k sobě může provádět vyhledávání adresy IP pomocí názvy hostitelů.

Podle potřeby hostname změny se po nasazení virtuálního počítače, použijte příkaz

    # sudo hostname <newname>

Azure Linux Agent obsahuje funkce automaticky zjišťovat tato změna názvu a konfiguraci virtuálního počítače přetrvávají tato změna a publikovat tuto změnu na servery DNS platformu.

 - [Azure Linux Agent User Guide](virtual-machines-linux-agent-user-guide.md)

### <a name="cloud-init"></a>Inicializace cloudu
Obrázky **se systémem Ubuntu** a **jádro operačního systému** využívat cloud inicializace na Azure, který poskytuje další možnosti pro zavádění virtuálního počítače.

 - [Jak založit vlastních dat](virtual-machines-windows-classic-inject-custom-data.md)
 - [Vlastní Data a cloudu inicializace na Microsoft Azure](https://azure.microsoft.com/blog/2014/04/21/custom-data-and-cloud-init-on-windows-azure/)
 - [Vytvoření oddílů Azure vyměňovat pomocí inicializace cloudu](https://wiki.ubuntu.com/AzureSwapPartitions)
 - [Používání jádro operačního systému na Azure](https://coreos.com/os/docs/latest/booting-on-azure.html)


## <a name="virtual-machine-image-capture"></a>Zachycení obrázku virtuálního počítače

Azure poskytuje možnost zachytit stav existující virtuálního počítače do obrázku, který následně lze nasazení instance další virtuálního počítače. Azure Linux Agent může být slouží k vrácení část kustomizace, které bylo provedeno během procesu zřizovací. Může podle následujících kroků k zaznamenání virtuálního počítače jako obrázek:

1. Spuštění **waagent-identitách** vrátit zřizovací přizpůsobení. Nebo **waagent-identitách + uživatele** volitelně odstranění uživatelského účtu zadaného během zřizovací a všechny přidružené data.

2. Vypnutí dolů/vypnutí virtuální počítač.

3. Klikněte na *zachycení* na portálu Azure klasické nebo pomocí prostředí Powershell nebo rozhraní příkazového řádku nástroje zachytit virtuálního počítače jako obrázek.

 - Viz: [jak zachytit virtuální počítač Linux sloužit jako šablony](virtual-machines-linux-classic-capture-image.md)


## <a name="attaching-disks"></a>Připojení disků

Každý počítač virtuální má dočasné, místní připojeného *disku zdroje* . Protože data na disku zdroje nemusí být trvalé po restartování počítače, je často používá tak, že aplikace a procesy v počítači virtuální přechodové a **dočasné** uložení dat. Slouží také k ukládání stránky nebo zaměnit souborů pro operační systém.

Na Linux disku zdroje obvykle spravuje Azure Linux Agent a automaticky připojit **/mnt/resource** (nebo **/mnt** na systémem Ubuntu obrázky).


>[AZURE.NOTE] Všimněte si, že disk zdroje je **dočasný** disk a může odstranění a přeformátovaný po restartování OM.

Na Linux disku dat může být s názvem tak, že jádra jako `/dev/sdc`, a uživatelé budou muset oddílů, formátování a připojte daný zdroj. To je součástí podrobné pokyny v tomto kurzu: [jak připojit Disk dat do virtuálního počítače](virtual-machines-linux-classic-attach-disk.md).

 - **Viz taky:** [Konfigurace Software RAID na Linux](virtual-machines-linux-configure-raid.md)  &  [Konfigurace LVM na Linux OM v Azure](virtual-machines-linux-configure-lvm.md)

