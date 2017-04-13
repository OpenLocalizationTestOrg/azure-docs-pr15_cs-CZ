 <properties
   pageTitle="Azure a Linux | Microsoft Azure"
   description="Popisuje služby Azure výpočtu úložiště a sítě s Linux virtuálních počítačích."
   services="virtual-machines-linux"
   documentationCenter="virtual-machines-linux"
   authors="vlivech"
   manager="timlt"
   editor=""/>

<tags
   ms.service="virtual-machines-linux"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="infrastructure"
   ms.date="09/14/2016"
   ms.author="v-livech"/>

# <a name="azure-and-linux"></a>Azure a Linux

Microsoft Azure je rostoucí sada integrované veřejné cloudových služeb včetně databází analýzy, virtuálních počítačích, mobilní sítě, úložiště a webových – ideální k hostování vašeho řešení.  Microsoft Azure poskytuje scalable platforma umožňující pouze zaplatit můžete použít, pokud budete chtít – bez nutnosti investovat do místního hardwaru.  Pokud jste do vašeho řešení škálování a se na jakékoliv měřítko nastavíte, že služby potřeb klienty Azure hotovou.

Pokud máte zkušenosti s různými funkcemi, které je Amazon AWS, můžete prozkoumat AWS Azure a [definice mapování dokumentu](https://azure.microsoft.com/campaigns/azure-vs-aws/mapping/).


## <a name="regions"></a>Oblasti
Microsoft Azure zdroje jsou rozvržena více zeměpisnými oblastmi světa.  "Oblasti" představuje více datacentrech v jednu zeměpisnou oblast.  Od 1. ledna 2016, jedná se o: 8 v Americe, 2 v Evropě, 6 v Asijsko-tichomořské, 2 v Číně a 3 v Indii.  Pokud chcete na celý seznam všech Azure oblastí, jsme udržovat že seznam stávající a nově se ohlásí, oblasti.

- [Azure oblastí](https://azure.microsoft.com/regions/)

## <a name="availability"></a>Dostupnost
Aby nasazení pro naše 99.95 OM úroveň smlouvách budete muset nasazení dvě nebo nastavit další VMs spuštění svého pracovního vytížení uvnitř dostupné. Zajistíte vaší VMs jsou rozvržena víc poruch domén v naší datacentrech i nasadit do hosts s windows různé údržbu. Úplné [Azure SLA](https://azure.microsoft.com/support/legal/sla/virtual-machines/v1_0/) vysvětluje zaručené dostupnost Azure jako celek.

## <a name="azure-virtual-machines--instances"></a>Azure virtuálních počítačích a instance
Microsoft Azure podporuje spuštění celá řada Oblíbené Linux distribuce spravovaný společností počet partnery.  Zjistíte distribuce třeba červeným klobouk Enterprise, CentOS, Debian, se systémem Ubuntu, jádro operačního systému, RancherOS, FreeBSD a dalších možností v Azure Marketplace. Aktivně Pracujeme s různými komunit Linux přidáte ještě víc provedeních do seznamu [že Azure potvrzeného Linux Distros](virtual-machines-linux-endorsed-distros.md) .

Pokud svůj preferovaný Linux distro volba není momentálně v galerii, je možné "přenést vlastní Linux" OM – [Vytvoření a odeslání virtuálního pevného disku Linux v Azure](virtual-machines-linux-create-upload-generic.md).

Azure virtuálních počítačích umožňují nasadit širokou škálu výpočetních řešení aktivní způsobem. Můžete nasadit libovolných pracovního vytížení a libovolný jazyk na téměř libovolném operační systém – Windows, Linux, nebo vlastní nevytvořili z jednoho z našich rostoucí seznam partnerů. Pořád nezobrazuje co hledáte?  Nedělejte si starosti: můžete také přenést vlastní obrázky z místního.

## <a name="vm-sizes"></a>OM velikosti
Při nasazení OM v Azure přecházíte vyberte OM velikost v jednom z našich série velikosti, která je vhodný k vaší pracovní zátěž. Velikost má dopad i na zpracování power paměti a úložiště objemu virtuální počítač. Je faktura podle dobu OM je spuštěná a využívající přiřazený zdroje. Úplný seznam [velikosti virtuálních počítačích](virtual-machines-linux-sizes.md).

Tady jsou některé základní pokyny pro výběr velikostí OM z jednoho z našich řady (A, D, Pošta, G a GS).

* VMs A řady jsou naše hodnotu ceny osobních VMs pro light pracovního vytížení a zařízením nebo zkoušení scénáře. Jsou dostup dostupný ve všech oblastech a můžete připojit a všech standardních články k dispozici pro virtuálních počítačích.
* Velikost A řady (A8: A11) jsou speciální výpočetním náročné konfigurace vhodná pro výkonné výpočetního clusteru aplikace.
* D řady VMs slouží ke spuštění aplikace, které požadovat vyšší výpočetního výkonu a výkonu dočasné disku. D řady VMs přidávejte k dočasné disku rychlejší procesorů, vyšší poměr paměti core a jednotce (SSD).
* Dv2 řady, je nejnovější verze naše D řady, funkce výkonnější procesoru. Procesor Dv2 řady je o 35 % rychlejší procesor D řady. Je založená na nejnovější generování 2,4 GHz Intel Xeon® E5-2673 v3 procesor (Haskell) a s technologií 2.0 Intel Turbo zesílení můžete přejít na 3,2 GHz. Dv2 řada má stejné konfigurace paměti a disku jako D řady.
* G řady VMs nabízí většina paměti a spustit hosts s rodinnými procesory Intel Xeon E5 V3.

Poznámka: DS řady a GS řady VMs mít přístup k základnímu úložišti Premium: Naše SSD záložní výkonných maximum, minimum latence úložiště pro náročné pracovního vytížení vstupu a výstupu. Premium úložiště je dostupné v některých oblastech. Podrobnosti najdete v tématu:

- [Úložiště Premium: Výkonné úložiště pro pracovního vytížení Azure virtuálního počítače](../storage/storage-premium-storage.md)

## <a name="automation"></a>Automatizace
K dosažení správné jazyková verze DevOps, musí být všechny infrastruktury kód.  Pokud všechny infrastrukturu jsou umístěná v kód, který je možné snadno k opětovnému vytvoření (Phoenix servery).  Azure spolupracuje hlavní automatizaci nástroje jako Ansible, Chef, SaltStack a Puppet.  Azure má i svůj vlastní nástrojů pro automatizaci:

- [Azure šablony](virtual-machines-linux-create-ssh-secured-vm-from-template.md)

- [Azure VMAccess](virtual-machines-linux-using-vmaccess-extension.md)

Azure je zavádění podpora [cloudu inicializace](http://cloud-init.io/) přes většina Linux Distros podporující ho.  Na Canonical systémem Ubuntu VMs jsou aktuálně nasazeny s cloudu inicializace standardně zapnutá.  RedHats RHEL, CentOS a Fedora podporují cloudu inicializace, ale Azure obrázky vedené RedHat nemají nainstalovaný cloudu inicializace.  Používání cloudové inicializace na RedHat řady s operačním systémem, musíte vytvořit vlastní obrázek s nainstalovanou cloudu inicializace.

- [Používání cloudové inicializace na Azure Linux VMs](virtual-machines-linux-using-cloud-init.md)

## <a name="quotas"></a>Kvóty
Každý Azure předplatné má výchozí kvóty na místě, které by mohly ovlivnit nasazení velkého počtu VMs plánu projektu. Aktuální omezení na základě předplatného za je 20 VMs jednotlivých oblastech.  Kvóty můžete dosáhnout tak, že používá se zařazování požadavek podpory můžete žádosti o zvýšení limit.  Podrobné informace o kvóty:

- [Omezení služby Azure předplatného](../azure-subscription-service-limits.md)


## <a name="partners"></a>Partneři

Microsoft spolupracuje s partneři zajistit aktualizace a optimalizované pro Azure runtime obrázků k dispozici.  Další informace o partneři zkontrolujte stránky své marketplace.

- [Linux na Azure potvrzeného rozdělení](virtual-machines-linux-endorsed-distros.md)

RedHat - [Azure Marketplace - RedHat Enterprise Linux 7.2](https://azure.microsoft.com/marketplace/partners/redhat/redhatenterpriselinux72/)

Kanonický - [Azure Marketplace - Server se systémem Ubuntu 16.04 l](https://azure.microsoft.com/marketplace/partners/canonical/ubuntuserver1604lts/)

Debian - [Azure Marketplace - 8 Debian "Klára"](https://azure.microsoft.com/marketplace/partners/credativ/debian8/)

FreeBSD - [Azure Marketplace - FreeBSD 10.3](https://azure.microsoft.com/marketplace/partners/microsoft/freebsd103/)

Jádro operačního systému - [Azure Marketplace - jádro operačního systému (stabilní)](https://azure.microsoft.com/marketplace/partners/coreos/coreosstable/)

RancherOS - [Azure Marketplace - RancherOS](https://azure.microsoft.com/marketplace/partners/rancher/rancheros/)

Bitnami - [Bitnami v knihovně Azure](https://azure.bitnami.com/)

Mezosféře - [Azure Marketplace - mezosféře Datacentrum/s operačním systémem na Azure](https://azure.microsoft.com/marketplace/partners/mesosphere/dcosdcos/)

Docker - [Azure Marketplace - služba Azure kontejneru s Docker roj](https://azure.microsoft.com/marketplace/partners/microsoft/acsswarms/)

Jenkins - [Azure Marketplace - CloudBees Jenkins platformy](https://azure.microsoft.com/marketplace/partners/cloudbees/jenkins-platformjenkins-platform/)


## <a name="getting-setup-on-azure"></a>Získání instalace na Azure
Pokud chcete začít Azure budete potřebovat účet Azure, Azure rozhraní příkazového řádku nainstalovaný a dvojice SSH veřejné a privátní klíče.

## <a name="sign-up-for-an-account"></a>Registrace účtu
Cílem prvního kroku při používání cloudové Azure je Azure účet.  Přejděte na stránku [Zapsat účet Azure](https://azure.microsoft.com/pricing/free-trial/) začít pracovat.

## <a name="install-the-cli"></a>Instalace rozhraní příkazového řádku
Vaše nového účtu služby Azure mohli rovnou začít okamžitě na portálu Azure, což je panelu Správce založeného na webu.  Ke správě cloudu Azure pomocí příkazového řádku, můžete nainstalovat `azure-cli`.  Instalace [Azure rozhraní příkazového řádku ](../xplat-cli-install.md)na počítači Mac a Linux.

## <a name="create-an-ssh-key-pair"></a>Vytvoření klíčových pár SSH
Nyní máte účet Azure Azure webového portálu a rozhraní příkazového řádku Azure.  Dalším krokem je vytvoření pár klíč SSH, který se používá k SSH do Linux bez použití hesla.  [Vytvoření SSH klávesám Mac a Linux](virtual-machines-linux-mac-create-ssh-keys.md) povolit lepší zabezpečení a heslo bez přihlášení.

## <a name="getting-started-with-linux-on-microsoft-azure"></a>Začínáme s Linux na Microsoft Azure
Pomocí nastavení účet Azure, nainstalovaný rozhraní příkazového řádku Azure a klíče SSH vytvořené nyní jste připraveni začít sestavování infrastrukturu v cloudu Azure.  První úkol je vytvořte páru VMs.

## <a name="create-a-vm-using-the-cli"></a>Vytvoření OM pomocí rozhraní příkazového řádku
Vytváření Linux OM pomocí rozhraní příkazového řádku je rychlý způsob, jak nasadit OM aniž byste museli opustit terminál právě pracujete.  Všechno, co můžete zadat na web portálu prostřednictvím příkazového řádku příznak nebo přepnout neexistuje.  

- [Vytvoření Linux OM pomocí rozhraní příkazového řádku](virtual-machines-linux-quick-create-cli.md)

## <a name="create-a-vm-in-the-portal"></a>Vytvoření virtuálního počítače na portálu
Vytváření Linux OM v Azure webového portálu je způsob, jak snadno klepnutím myší přes různé možnosti pro přístup k nasazení.  Místo použití příkazového řádku příznaky nebo přepínače, se zobrazíte rozložení hodní webové stránky různých možností a nastavení.  Všechno, co dostupné prostřednictvím rozhraní příkazového řádku je k dispozici na portálu.

- [Vytvoření Linux OM na portálu](virtual-machines-linux-quick-create-portal.md)

## <a name="login-using-ssh-without-a-password"></a>Přihlášení pomocí SSH bez hesla
OM je teď spuštěna Azure a budete chtít přihlásit.  Používání hesel při přihlášení pomocí SSH je zabezpečený a časově náročný.  Pomocí kláves se SSH je nejbezpečnější způsob i nejrychlejším způsobem, jak přihlášení.  Při vytváření můžete Linux OM prostřednictvím portálu nebo rozhraní příkazového řádku, máte dvě možnosti ověřování.  Pokud se rozhodnete heslo pro SSH Azure nakonfiguruje OM chcete povolit přihlášení pomocí hesla.  Pokud jste se rozhodli použít veřejný klíč SSH, Azure nakonfiguruje OM pouze povolit přihlášení pomocí klávesy SSH a zakáže přihlášení heslo. Zajistit Linux OM pouze povolením SSH klíčové přihlášení pomocí funkce SSH veřejné klíče při vytváření OM v portálu nebo rozhraní příkazového řádku.

- [Zakázání SSH hesla na Linux OM nakonfigurováním SSHD](virtual-machines-linux-mac-disable-ssh-password-usage.md)

## <a name="related-azure-components"></a>Související Azure součásti

## <a name="storage"></a>Úložiště

- [Úvod k základnímu úložišti Microsoft Azure](../storage/storage-introduction.md)

- [Přidání disku Linux OM pomocí azure-rozhraní příkazového řádku](virtual-machines-linux-add-disk.md)

- [Jak připojit disk dat Linux OM na portálu Azure](virtual-machines-linux-attach-disk-portal.md)

## <a name="networking"></a>Sítě

- [Přehled virtuální sítě](../virtual-network/virtual-networks-overview.md)

- [IP adresy v Azure](../virtual-network/virtual-network-ip-addresses-overview-arm.md)

- [Otevření portů Linux angličtině v Azure](virtual-machines-linux-nsg-quickstart.md)

- [Vytvoření plně kvalifikovaný název domény na portálu Azure](virtual-machines-linux-portal-create-fqdn.md)


## <a name="containers"></a>Kontejnery

- [Virtuálních počítačích a kontejnery v Azure](virtual-machines-linux-containers.md)

- [Úvod kontejneru služby Azure](../container-service/container-service-intro.md)

- [Nasazení služby Azure kontejneru obrázku](../container-service/container-service-deployment.md)

## <a name="next-steps"></a>Další kroky

Nyní máte přehled Linux na Azure.  Dalším krokem je ponoříte hlouběji a vytvářet několika VMs.

- [Vytvoření Linux OM na Azure pomocí portálu](virtual-machines-linux-quick-create-portal.md)

- [Vytvoření Linux OM na Azure pomocí rozhraní příkazového řádku](virtual-machines-linux-quick-create-cli.md)
