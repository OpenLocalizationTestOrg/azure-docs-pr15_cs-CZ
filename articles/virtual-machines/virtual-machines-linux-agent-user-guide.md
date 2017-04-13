<properties 
    pageTitle="Linux Agent User Guide | Microsoft Azure" 
    description="Zjistěte, jak nainstalovat a nakonfigurovat Linux Agent (waagent) ke správě virtuálního počítače interakce s Azure struktury řadiče." 
    services="virtual-machines-linux" 
    documentationCenter="" 
    authors="szarkos" 
    manager="timlt" 
    editor=""
    tags="azure-service-management,azure-resource-manager" />

<tags 
    ms.service="virtual-machines-linux" 
    ms.workload="infrastructure-services" 
    ms.tgt_pltfrm="vm-linux" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/17/2016" 
    ms.author="szark"/>



# <a name="azure-linux-agent-user-guide"></a>Azure Linux Agent User Guide

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

## <a name="introduction"></a>Úvod

Microsoft Azure Linux Agent (waagent) spravuje Linux & FreeBSD zřizování a OM interakce s řadiče struktury Azure. Poskytuje následující funkce pro Linux a FreeBSD IaaS nasazení:

> [AZURE.NOTE] Viz Azure Linux agent [Soubor Readme pro](https://github.com/Azure/WALinuxAgent/blob/master/README.md) další podrobnosti.

* **Obrázek zřízení**
  - Vytvoření uživatelského účtu
  - Konfigurace SSH typů ověřování
  - Nasazení SSH veřejné klíče a dvojice klíčů
  - Nastavení název hostitele
  - Publikování název hostitele DNS platformy
  - Vytváření sestav SSH hostitele klíčové otisku platformy
  - Správa disků zdroje
  - Formátování a připojování disku zdroje
  - Konfigurace vyměňovat místa

* **Sítě**
  - Spravuje směruje ke zlepšení kompatibility s servery platformy
  - Zajišťuje stability síťový název rozhraní

* **Jádra**
  - Konfiguruje virtuální NUMA (zakázat pro jádra < 2.6.37)
  - Spotřebovává Hyper-V entropie pro /dev/random
  - Konfiguruje SCSI časové limity pro kořenové zařízení (který může být i vzdálený)

* **Diagnostika**
  - Přesměrování konzoly sériový port

* **SCVMM nasazení**
    - Rozpozná a bootstraps agenta VMM Linux při spuštění v prostředí System Center virtuálního počítače správce 2012 R2

* **Rozšíření OM**
    - Vloží součást vytvořené společností Microsoft a partnery do Linux OM (IaaS) povolit software a automatické konfigurace
    - Rozšíření OM implementaci odkaz na [https://github.com/Azure/azure-linux-extensions](https://github.com/Azure/azure-linux-extensions)


## <a name="communication"></a>Komunikace
Tok informací z platformu agenta probíhá pomocí dvou kanálech:

* Čas spuštění připojených DVD IaaS nasazení. Tento disk DVD obsahuje vyhovujících zápisu OVF konfigurační soubor, který obsahuje všechny informace zřizovací než skutečné keypairs SSH.
* Koncový bod TCP vystavení rozhraní REST API slouží k získání nasazení a konfiguraci topologie.


## <a name="requirements"></a>Požadavky
Následující systémy mít otestování a je známo, pracovat s agentem Linux Azure:

> [AZURE.NOTE] Tento seznam se může lišit od seznamu oficiální podporované systémy na platformu Microsoft Azure podle níže uvedeného popisu: [http://support.microsoft.com/kb/2805216](http://support.microsoft.com/kb/2805216)

* Jádro operačního systému
* CentOS 6.3 +
* Červené klobouk Enterprise Linux 6,7 +
* Debian 7.0 +
* Systémem Ubuntu 12.04 +
* openSUSE 12.3 +
* SLES 11 SP3 +
* Oracle Linux 6.4 +

Další podporované systémy:

* FreeBSD 10 + Azure Linux Agent v2.0.10 +)

Linux agent závisí na některé systém balíčky ke správnému:

* Python 2.6 +
* OpenSSL 1.0 +
* Funkci OpenSSH 5.3 +
* Systém souborů nástroje: čtvrceny sfdisk, fdisk, mkfs,
* Heslo nástroje: chpasswd sudo
* Nástroje pro zpracování textu: sed grep
* Síťové nástroje: trasy ip
* Jádra podporu připojení UDF filesystems.

## <a name="installation"></a>Instalace
Instalace pomocí ot nebo balíčku DEB z vaší distribuční balíčku úložiště je upřednostňovaný způsob instalace a upgrade agenta Azure Linux. Všechny [potvrzeného rozdělení poskytovatelů](virtual-machines-linux-endorsed-distros.md) integrovat do svých obrázky a úložištích balíček agenta Azure Linux.

Podívejte se do dokumentace v [Azure Linux Agent repo na Github](https://github.com/Azure/WALinuxAgent) pro Upřesnit možnosti instalace, například instalaci vlastní umístění a předpony nebo od zdroje.


## <a name="command-line-options"></a>Parametry příkazového řádku

### <a name="flags"></a>Příznaky

- podrobný: zvětšit podrobností zadaný příkaz
- Vynutit: přeskočit interaktivní potvrzení pro některé příkazy

### <a name="commands"></a>Příkazy

- Nápověda: obsahuje podporované příkazy a příznaků.

- identitách: Pokus o vyčištění systému a jeho vhodné pro znovu zřizování. Tuto operaci odstranit takto:
 * Všechny SSH klíče hostitele (Pokud Provisioning.RegenerateSshHostKeyPair je "y" v souboru konfigurace).
 * Konfigurace NameServer ve /etc/resolv.conf
 * Heslo kořenové od/etc/shadow (Pokud Provisioning.DeleteRootPassword je "y" v souboru konfigurace).
 * Režim Cached DHCP zapůjčení
 * Název hostitele obnovíte do localhost.localdomain


> [AZURE.WARNING] Zrušení zajišťování nezaručuje, že je zaškrtnuté všechny citlivé informace a vhodné pro redistribuci obrázku.


- identitách + uživatele: provádí všechny položky v části - identitách (nahoře) a také odstraní poslední zřizování uživatelský účet (získané /var/lib/waagent) a související data. Tento parametr je při odstraňování zřizování obrázek, který dříve zřizování na Azure, může být zaznamenávání a znovu použít.

- verze: Zobrazuje verzi waagent

- serialconsole: nakonfiguruje GRUB označíte ttyS0 (první sériové port) jako konzole spuštění. Zajistíte, že jsou odeslané na sériové port a k dispozici pro ladění jádra spuštění protokolování.

- Démon: spustit waagent jako démon ke správě interakce s informacemi o platformě. Tento argument je určen k waagent skriptu inicializace waagent.

- spuštění: spustit waagent jako pozadí obrázku


## <a name="configuration"></a>Konfigurace

Konfigurační soubor (/ etc/waagent.conf) určuje akce waagent. Ukázkový soubor konfigurace je zobrazeno níže:

    Provisioning.Enabled=y
    Provisioning.DeleteRootPassword=n
    Provisioning.RegenerateSshHostKeyPair=y
    Provisioning.SshHostKeyPairType=rsa
    Provisioning.MonitorHostName=y
    Provisioning.DecodeCustomData=n
    Provisioning.ExecuteCustomData=n
    Provisioning.PasswordCryptId=6
    Provisioning.PasswordCryptSaltLength=10
    ResourceDisk.Format=y
    ResourceDisk.Filesystem=ext4
    ResourceDisk.MountPoint=/mnt/resource
    ResourceDisk.MountOptions=None
    ResourceDisk.EnableSwap=n
    ResourceDisk.SwapSizeMB=0
    LBProbeResponder=y
    Logs.Verbose=n
    OS.RootDeviceScsiTimeout=300
    OS.OpensslPath=None
    HttpProxy.Host=None
    HttpProxy.Port=None

Možnosti konfigurace jsou píše níže. Konfigurace možností jsou tři typy; Logická hodnota, řetězec nebo celé číslo. Možnosti způsob konfigurace může být zadán jako "y" nebo "n". Speciální klíčové slovo, které "Žádná" mohou být použity pro některé řetězec typ položky konfigurace popsané níže.

**Provisioning.Enabled:**  
Typ: logické  
Výchozí: y

To umožňuje uživatelům povolit nebo zakázat funkci zřizovací v agent. Platné hodnoty jsou "y" nebo "n". Pokud zřizování zakázané, se zachovají SSH hostitele uživatele klávesám na obrázku a ignorovat všechny konfigurace zadaná v Azure zřizování rozhraní API.

> [AZURE.NOTE] `Provisioning.Enabled` Výchozí hodnota parametru "n" na obrázky cloudu systémem Ubuntu využívající cloudové inicializace pro zřizování.

  
**Provisioning.DeleteRootPassword:**  
Typ: logické  
Výchozí: n

Je-li nastavit heslo kořenové v souboru stín/atd/se vymažou během procesu zřizovací.


**Provisioning.RegenerateSshHostKeyPair:**  
Typ: logické  
Výchozí: y

Pokud během procesu zřizovací z/etc/ssh/odstraňují nastavení, všechny SSH hostitele klíčové dvojice (ecdsa, dsa a rsa). A bude vytvořena pár jedné nové klíče.

Typ šifrování svěže pár klíčů lze konfigurovat položkou Provisioning.SshHostKeyPairType. Všimněte si, že některé distribuce znovu vytvoří dvojice klíčů SSH pro libovolné typy chybějící šifrování démona SSH po restartování (například po restartování).


**Provisioning.SshHostKeyPairType:**  
Typ: řetězce  
Výchozí: rsa

To můžete nastavit šifrování algoritmus typu podporovaný démona SSH na virtuální počítač. Obvykle podporované hodnoty jsou "rsa", "dsa" a "ecdsa". Všimněte si, že "putty.exe" v systému Windows nepodporuje "ecdsa". Ano Pokud chcete použít putty.exe v systému Windows pro připojení k Linux nasazení, stiskněte klávesovou zkratku "rsa" nebo "dsa".


**Provisioning.MonitorHostName:**  
Typ: logické  
Výchozí: y

Pokud nastavit, waagent bude sledovat virtuálního počítače Linux hostname změny (jako vrácené příkazem "hostname") a automaticky aktualizovat konfiguraci sítě obrázku tak, aby odrážely změny. Abyste mohli použít Změna názvu pro servery DNS, budou sítě nerestartuje ve počítače virtuální. To stručně řečeno dojde ke ztrátě připojení k Internetu.


**Provisioning.DecodeCustomData**  
Typ: logické  
Výchozí: n

Pokud nastavit, waagent bude dekódovat CustomData z ve formátu Base 64.


**Provisioning.ExecuteCustomData**  
Typ: logické  
Výchozí: n

Pokud nastavit, waagent provede CustomData po zajištění služby.


**Provisioning.PasswordCryptId**  
Řetězec typu:  
Výchozí: 6

Algoritmus používaný crypt při generování algoritmus hash hesla.  
 1 – MD5  
 2 – Blowfish  
 5 – SHA-256  
 6 – SHA-512  


**Provisioning.PasswordCryptSaltLength**  
Řetězec typu:  
Výchozí: 10

Délka náhodné soli při generování algoritmus hash hesla.


**ResourceDisk.Format:**  
Typ: logické  
Výchozí: y

Pokud nastavit, disk prostředku poskytovanou platformu bude zformátováno a připevněna waagent Pokud je požadována uživatelem v "ResourceDisk.Filesystem" typ systému souborů než "ntfs". Jeden oddíl typu Linux (83) budou dostupné na disku. Poznámka: Tento oddíl nebude naformátovat Pokud jej lze úspěšně připojit.


**ResourceDisk.Filesystem:**  
Typ: řetězce  
Výchozí: ext4

To určuje typ systému souborů pro disk zdroje. Podporované hodnoty se liší podle distribučních Linux. Pokud je řetězec X, pak mkfs. X by měly tvořit nad snímkem Linux. Obrázky SLES 11 obvykle používejte "ext3". FreeBSD obrázků, používejte "ufs2" tady.


**ResourceDisk.MountPoint:**  
Typ: řetězce  
Výchozí: / mnt/zdroje 

To určuje cestu niž je připojen disku zdroje. Všimněte si, že disku zdroje je *dočasný* disk a může vyprázdní při poskytování zrušeno OM.


**ResourceDisk.MountOptions**  
Typ: řetězce  
Výchozí: žádné

Určuje možnosti připojení disku předávat příkazu -o připojení. Toto je seznam hodnot, hodnoty s oddělovači. "nodev, nosuid". V tématu mount(8) podrobnosti.


**ResourceDisk.EnableSwap:**  
Typ: logické  
Výchozí: n

Pokud nastavení odkládací soubor (/ odkládacímu souboru) je na disku zdroje vytvořené a přidané do odkládací prostor systému.


**ResourceDisk.SwapSizeMB:**  
Typ: celé číslo  
Výchozí: 0

Velikost souboru vyměňovat v megabajtech.


**Logs.Verbose:**  
Typ: logické  
Výchozí: n

Pokud je zesílen nastavení protokolu podrobností. Waagent zaznamená do /var/log/waagent.log a využití logrotate funkčnosti systému otočit protokoly.


**OPERAČNÍ SYSTÉM. EnableRDMA**  
Typ: logické  
Výchozí: n

Pokud nastavit, agent pokusí nainstalujete a zaveďte ovladač jádra RDMA, která odpovídá verzi firmwaru na základním hardwaru.


**OPERAČNÍ SYSTÉM. RootDeviceScsiTimeout:**  
Typ: celé číslo  
Výchozí: 300

Tímto postupem nakonfigurováno časový limit SCSI v sekundách na jednotkách disk a dat s operačním systémem. Není-li nastavit, systému, který se používají výchozí hodnoty.


**OPERAČNÍ SYSTÉM. OpensslPath:**  
Typ: řetězce  
Výchozí: žádné

Lze použít k určení alternativní cesty pro openssl binární pro účely cryptographic operace.


**HttpProxy.Host HttpProxy.Port**  
Typ: řetězce  
Výchozí: žádné

Pokud nastavit, agent použije tento proxy server pro přístup k Internetu. 


## <a name="ubuntu-cloud-images"></a>Obrázky se systémem Ubuntu cloudu

Všimněte si, že se systémem Ubuntu cloudu obrázky využít [cloudu inicializace](https://launchpad.net/ubuntu/+source/cloud-init) k provádění mnoha úkolů konfigurace, které by jinak spravován Linux agentem Azure.  Dejte pozor následující rozdíly:


- **Provisioning.Enabled** ve výchozím nastavení "n" na obrázky cloudu systémem Ubuntu využívající cloudové inicializace provádět zřizovací úkoly.

- Následujících parametrů konfigurace nemá vliv na obrázky cloudu systémem Ubuntu využívající cloudové inicializace spravovat zdroje disku a zaměnit místa:

 - **ResourceDisk.Format**
 - **ResourceDisk.Filesystem**
 - **ResourceDisk.MountPoint**
 - **ResourceDisk.EnableSwap**
 - **ResourceDisk.SwapSizeMB**

- Naleznete v následujících zdrojích ke konfiguraci bodu připojení zdroje disku a zaměnit místa na obrázky cloudu systémem Ubuntu během vytváření:

 - [Se systémem Ubuntu wikiwebu: Konfigurace odkládací oddíl](http://go.microsoft.com/fwlink/?LinkID=532955&clcid=0x409)
 - [Vložení vlastních dat do Azure virtuálního počítače](virtual-machines-windows-classic-inject-custom-data.md)

