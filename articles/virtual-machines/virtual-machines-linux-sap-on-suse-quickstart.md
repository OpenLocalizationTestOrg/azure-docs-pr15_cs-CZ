<properties
   pageTitle="Testování NetWeaver SAP na Microsoft Azure SUSE Linux VMs | Microsoft Azure"
   description="Testování NetWeaver SAP na Microsoft Azure SUSE Linux VMs"
   services="virtual-machines-linux"
   documentationCenter=""
   authors="hermanndms"
   manager="timlt"
   editor=""
   tags="azure-resource-manager"
   keywords=""/>
<tags
   ms.service="virtual-machines-linux"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="infrastructure-services"
   ms.date="09/15/2016"
   ms.author="hermannd"/>

# <a name="running-sap-netweaver-on-microsoft-azure-suse-linux-vms"></a>Průběžný NetWeaver SAP na Microsoft Azure SUSE Linux VMs


Tento článek popisuje různé co je potřeba zvážit při provozu SAP NetWeaver na virtuálních počítačích Microsoft Azure SUSE Linux (VMs). K 19 2016 květen SAP NetWeaver oficiálně podporované v SUSE Linux VMs Azure. Všechny podrobnosti týkající se Linux verze, SAP jádra verzí a tak dále najdete v SAP poznámku 1928533 "aplikací SAP na Azure: podporované produkty a typy Azure OM".
Další dokumentace o SAP na Linux VMs najdete tady: [Pomocí SAP na Linux virtuálních počítačích (VMs)](virtual-machines-linux-sap-get-started.md).


Tyto informace by vám pomůžou předejít některé potenciálních problémech.

## <a name="suse-images-on-azure-for-running-sap"></a>SUSE obrázky na Azure výpočetnímu SAP

Výpočetnímu SAP NetWeaver Azure, použijte pouze SUSE Linux Enterprise Server SLES 12 (SPx) – najdete v článku SAP poznámku 1928533. Speciální SUSE obrázek je v Azure Marketplace ("SLES 11 SP3 pro SAP CAL"), ale to není určená pro obecné spotřebu. Tento obrázek nepoužívá, protože je rezervován řešení [SAP cloudu zařízení knihovny](https://cal.sap.com/) .  

Správce prostředků Azure byste měli použít u všech nových testů a zařízení na Azure. Pokud chcete najít obrázky SUSE SLES a verze pomocí prostředí PowerShell Azure nebo rozhraní Azure příkazového řádku (rozhraní příkazového řádku), použijte následující příkazy. Pak můžete výstupu, například k definování obrázek OS v šabloně JSON pro nasazení nové OM SUSE Linux.
Tyto příkazy Powershellu jsou platné pro verzi Azure PowerShell 1.0.1 a vyšší.

* Vyhledejte existující vydavatelé, včetně SUSE:

   ```
   PS  : Get-AzureRmVMImagePublisher -Location "West Europe"  | where-object { $_.publishername -like "*US*"  }
   CLI : azure vm image list-publishers westeurope | grep "US"
   ```

* Vyhledejte existující nabídky z SUSE:

   ```
   PS  : Get-AzureRmVMImageOffer -Location "West Europe" -Publisher "SUSE"
   CLI : azure vm image list-offers westeurope SUSE
   ```

* Vyhledejte SUSE SLES nabídky:

   ```
   PS  : Get-AzureRmVMImageSku -Location "West Europe" -Publisher "SUSE" -Offer "SLES"
   CLI : azure vm image list-skus westeurope SUSE SLES
   ```

* Najděte konkrétní verzi SLES SKU:

   ```
   PS  : Get-AzureRmVMImage -Location "West Europe" -Publisher "SUSE" -Offer "SLES" -skus "12"
   CLI : azure vm image list westeurope SUSE SLES 12
   ```

## <a name="installing-walinuxagent-in-a-suse-vm"></a>Instalace WALinuxAgent v SUSE OM

Agent s názvem WALinuxAgent je součástí SLES obrázky v Azure Marketplace. Informace o instalaci ručně (například při odesílání s operačním systémem SLES virtuální pevný disk (virtuální pevný disk) z místního) najdete v tématu:

- [OpenSUSE] (http://software.opensuse.org/package/WALinuxAgent)

- [Azure] (virtuální počítačích linux potvrzeného distros.md)

- [SUSE] (https://www.suse.com/communities/blog/suse-linux-enterprise-server-configuration-for-windows-azure/)

## <a name="sap-enhanced-monitoring"></a>SAP "rozšířené sledování"

SAP "rozšířeného, sledování" je povinný předpoklad ke spuštění SAP na Azure. Zkontrolujte, že informace v SAP Poznámka 2191498 "SAP na Linux s Azure: rozšířené sledování".

## <a name="attaching-azure-data-disks-to-an-azure-linux-vm"></a>Připojení dat Azure disků OM Linux Azure

Měli byste nikdy disků dat Azure připojení k OM Linux Azure pomocí ID zařízení. Místo toho používejte identifikátor (UUID). Když používáte grafické nástroje disků Azure připojení dat, například buďte opatrní. Zkontrolujte položky v /etc/fstab.

Problém s ID zařízení je, že můžou změnit a pak můžou OM Azure reagovat v procesu spouštění. Zmírnění problém, můžete přidat parametr nofail v /etc/fstab. Ale pozor s nofail proto, že aplikace může používat bodu připojení jako před a může v případě, že disk externích dat Azure nebyl připojených během spuštění napište do kořenové systému souborů.

Jedinou výjimkou připojování přes UUID připojení s operačním systémem disku pro odstraňování potíží, jak je uvedeno v následující části.

## <a name="troubleshooting-a-suse-vm-that-isnt-accessible-anymore"></a>Poradce při potížích s SUSE OM, který už není dostupná

Může nastat situace, kdy SUSE OM na Azure přestane reagovat v procesu spouštění (například, zobrazí se chybová zpráva týkající se připojení disků). Tento problém můžete ověřit pomocí funkce diagnostiky spouštěcí Azure virtuálních počítačích v2 Azure portálu. Další informace najdete v tématu [spouštěcí diagnostiky] (https://azure.microsoft.com/blog/boot-diagnostics-for-virtual-machines-v2/).

Jedním ze způsobů problém vyřešit, je připojte OS disk z poškozené OM do jiného OM SUSE na Azure. Proveďte požadované změny jako /etc/fstab pro úpravy nebo odstranění pravidel udev sítě popsané v následující části.

Je důležité mít na paměti, zvažte. Nasazení několik VMs SUSE z stejný obrázek z webu Azure Marketplace (například SLES 11 SP4), bude s operačním systémem disku, který má být vždy připevněna stejné UUID. Proto pomocí UUID připojit z různých OM, která byla nasazena pomocí stejný obrázek z webu Azure Marketplace s operačním systémem disku povede dvou identickými UUID. Způsobuje problémy a znamenat, že bude OM určeny Poradce při potížích ve skutečnosti spustit z disku OS připojených a poškozeného místo toho původní.

Chcete-li této situaci předejít dvěma způsoby:

* Použití jiným obrázkem z Azure Marketplace pro řešení potíží OM (například SLES 11 SPx místo SLES 12).
* Není poškozený disk OS připojit z jiného OM pomocí UUID – použití něco dalšího.

## <a name="uploading-a-suse-vm-from-on-premises-to-azure"></a>Nahrávání SUSE OM z místního na Azure

Popis kroků, které nahrát SUSE OM z místního na Azure, najdete v článku [Příprava SLES nebo openSUSE virtuálního počítače pro Azure] (virtual-machines-linux-suse-create-upload-vhd.md).

Pokud chcete nahrát OM bez krok identitách na konci (například pokud chcete zachovat stávající instalaci SAP, stejně jako název hostitele), zkontrolujte následující položky:

* Ujistěte se, že OS disk připojen pomocí UUID a ne identifikátor zařízení Změna UUID jenom v /etc/fstab není dost OS disku. Navíc nezapomeňte přizpůsobení spuštění zavaděč pomocí YaST nebo úpravou /boot/grub/menu.lst.
* Použijete-li formátu VHDX disku SUSE OS a převést na virtuální pevný disk pro odeslání do Azure, je velmi pravděpodobné, že zařízení sítě se změní ze eth0 eth1. Chcete-li se vyhnout potížím s při při spouštění na Azure později, přejděte zpátky eth0 podle popisu v [řešení eth0 v klonovaný VMware 11 SLES] (https://dartron.wordpress.com/2013/09/27/fixing-eth1-in-cloned-sles-11-vmware/).

Kromě toho, co je popsaná v článku znalostní báze doporučujeme ale odebrat takto:

   /lib/udev/rules.d/75-persistent-NET-Generator.Rules

Můžete taky nainstalovat Azure Linux Agent (waagent) vám pomůžou předejít problémům, dokud více nic nejsou.

## <a name="deploying-a-suse-vm-on-azure"></a>Nasazení SUSE OM na Azure

Pomocí JSON šablony souborů v novém modelu správce prostředků Azure byste měli vytvořit nové SUSE VMs. Po vytvoření souboru šablony JSON můžete nasadit OM pomocí následujícího příkazu rozhraní příkazového řádku jako alternativu k Powershellu:

   ```
   azure group deployment create "<deployment name>" -g "<resource group name>" --template-file "<../../filename.json>"

   ```
Další podrobnosti o JSON: soubory šablon najdete v tématu [vytváření správce prostředků Azure šablony] (zdroje skupiny – vytváření templates.md) a [Azure rychlý úvod šablony] (https://azure.microsoft.com/documentation/templates/).

Další informace o rozhraní příkazového řádku a správce prostředků Azure, najdete v článku [Azure rozhraní příkazového řádku pro Mac, Linux a Windows pomocí Správce prostředků Azure] (xplat-rozhraní příkazového řádku azure zdroje manager.md).

## <a name="sap-license-and-hardware-key"></a>SAP licence a hardwaru klíč

Úřední certifikace SAP Azure byla zavedená nový mechanismus při výpočtu klíč hardwaru SAP, který se používá k této licenci SAP. SAP jádra měli přizpůsobit aby této. Dřívější verze jádra SAP Linux neobsahuje tato změna kódu. Proto v určitých případech (například Azure OM velikosti), změněn klíč hardwaru SAP a SAP licenci byla přestane platit. To dvojitého v nejnovější jádra SAP Linux. Podrobnosti získáte SAP poznámku 1928533.

## <a name="suse-sapconf-package--tuned-adm"></a>Balíček sapconf SUSE / správně zapnuli adm

SUSE obsahuje balíček s názvem "sapconf", která spravuje sadu SAP specifické nastavení. Další podrobnosti o co dělá toto balíčku a jak nainstalovat a používat ho, najdete v článku [pomocí sapconf Příprava serveru SUSE Linux Enterprise spustíte systémům SAP] (https://www.suse.com/communities/blog/using-sapconf-to-prepare-suse-linux-enterprise-server-to-run-sap-systems/) a [co je sapconf nebo jak připravit SUSE Linux Enterprise Server pro spuštění systémům SAP?] (http://scn.sap.com/community/linux/blog/2014/03/31/what-is-sapconf-or-how-to-prepare-a-suse-linux-enterprise-server-for-running-sap-systems).

Mezitím není nového nástroje, které nahrazují sapconf - správně zapnuli adm. Jednu můžete najít podrobnější informace o tomto nástroji následující dva odkazy dole.

SLES dokumentace o profilu správně zapnuli adm sap-hana najdete [tady](https://www.suse.com/documentation/sles-for-sap-12/book_s4s/data/sec_s4s_configure_sapconf.html) 

Ladění systémy SAP úloh s správně zapnuli adm najdete [tady](https://www.suse.com/documentation/sles-for-sap-12/pdfdoc/book_s4s/book_s4s.pdf) v kapitola 6.2


## <a name="nfs-share-in-distributed-sap-installations"></a>Sdílení NFS v distribuované instalace SAP

Pokud máte distribuované instalace – například, ve které chcete nainstalovat databázi a servery aplikace SAP v samostatném VMs – můžete sdílet adresáři /sapmnt prostřednictvím systému souborů NFS. Pokud máte potíže s tímto postupem instalace po vytvoření sdílené složky NFS pro /sapmnt, zkontrolujte, jestli je nastavená "no_root_squash" pro sdílenou složku.

## <a name="logical-volumes"></a>Logické svazky

V minulosti podle potřeby jednu velké množství logické různých disků Azure data (například k databázi SAP), se doporučuje používat mdadm lvm nebyl ověřených plně ještě na Azure. Naučte se nastavit Linux RAID na Azure pomocí mdadm, najdete v článku [Konfigurace software RAID na Linux](virtual-machines-linux-configure-raid.md). Mezitím od začátku roku 2016 mohou také lvm je plně podporován v Azure a mohou sloužit jako alternativu k mdadm. Další informace týkající se lvm na Azure najdete v článku [Konfigurace LVM na Linux OM v Azure](virtual-machines-linux-configure-lvm.md).


## <a name="azure-suse-repository"></a>Azure SUSE úložiště

Pokud máte problém s přístupem ke standardní Azure SUSE úložiště, můžete ho resetovat jednoduchého příkazu. Zhroucení může dojít, pokud vytvoření obrázku soukromé OS v Azure oblasti a potom vložte snímek do jiné oblasti, kde chcete nasadit nové VMs, které jsou založeny na tento soukromé obrázek s operačním systémem. Jenom spusťte tento příkaz uvnitř OM:

   ```
   service guestregister restart
   ```

## <a name="gnome-desktop"></a>Gnome plochy

Pokud chcete používat plochu Gnome nainstalovat úplný systém ukázku SAP do jednoho OM – včetně SAP grafické prohlížeče a SAP Konzola pro správu – pomocí tohoto tip nainstalovat na Azure SLES obrázky:

   Pro SLES 11:

   ```
   zypper in -t pattern gnome
   ```

   Pro SLES 12:

   ```
   zypper in -t pattern gnome-basic
   ```

## <a name="sap-support-for-oracle-on-linux-in-the-cloud"></a>SAP podpora Oracle na Linux v cloudu

Neexistuje podpora omezení z Oracle na Linux ve virtualizovaném prostředí. Není to však téma specifické Azure, je důležité pochopit. SAP nepodporuje Oracle na SUSE nebo červené klobouk ve veřejných mraků jako Azure. Diskutovat o tomto tématu, obraťte se na Oracle přímo.
