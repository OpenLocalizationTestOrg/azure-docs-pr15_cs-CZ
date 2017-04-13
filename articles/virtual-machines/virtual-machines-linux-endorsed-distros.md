<properties
    pageTitle="Potvrzené rozdělení Linux | Microsoft Azure"
    description="Další informace o Linux na potvrzeného Azure rozdělení, včetně pokyny pro systémem Ubuntu, OpenLogic, Oracle a SUSE."
    services="virtual-machines-linux"
    documentationCenter=""
    authors="szarkos"
    manager="timlt"
    editor="tysonn"
    tags="azure-service-management,azure-resource-manager"
    />

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/17/2016"
    ms.author="szark"/>



#<a name="linux-on-azure-endorsed-distributions"></a>Linux na Azure potvrzeného rozdělení

> [AZURE.NOTE] Pokud máte chvíli, Pomozte nám zlepšit dokumentace OM Linux Azure pomocí tohoto [rychlého průzkumu](https://aka.ms/linuxdocsurvey) vašeho prostředí. Každý odpovědí pomáhá pomáhají vaši práci.

Na některých obrázcích Linux v galerii Azure nebo Marketplace, jsou k dispozici počtem partnery a pracujeme s různými komunit Linux ještě víc provedeních přidáte potvrzeného distribučního seznamu. Mezitím pro distribuci není k dispozici v galerii vždycky můžete přenést-e-vlastní-Linux podle pokynů uvedených na [této stránce](virtual-machines-linux-classic-create-upload-vhd.md).

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]


## <a name="supported-distributions--versions"></a>Podporované distribuce a verze ##

Následující tabulka uvádí Linux distribuce a verze, které jsou podporovány na Azure. Taky získáte domovské stránce [podpory pro Linux obrázky v Microsoft Azure](https://support.microsoft.com/en-us/kb/2941892) pro podrobnější informace.

Ovladače Linux Integration Services (seznam) pro Hyper-V a Azure jsou jádra moduly, které Microsoft přispívá přímo do odesílání dat jádra Linux.  SEZNAM ovladače jsou buď vestavená v hodnotu rozdělení jádra ve výchozím nastavení, nebo starší RHEL/CentOS-based distribuce jsou k dispozici jako samostatný soubor ke stažení [tady](http://go.microsoft.com/fwlink/?LinkID=403033&clcid=0x409).  Naleznete [v tomto článku](virtual-machines-linux-create-upload-generic.md#linux-kernel-requirements) najdete další informace o ovladače seznam.

Azure Linux Agent je už jsou předinstalované ve Azure Galerie obrázků a řeší obvykle z úložiště balíčku hodnotu rozdělení.  Zdrojový kód se nachází na [GitHub](https://github.com/azure/walinuxagent).

Rozdělení.|Verze|Ovladače|Agent
---|---|---|---
CentOS tak, že OpenLogic | CentOS 6.3 + 7.0 + | CentOS 6.3: [Stáhněte si seznam](http://go.microsoft.com/fwlink/?LinkID=403033&clcid=0x409)<p>CentOS 6.4 +: V jádra | Balíček: V [OpenLogic repo](http://olcentgbl.trafficmanager.net/openlogic/6/openlogic/x86_64/RPMS/) v části "WALinuxAgent" <br/>Zdrojový kód: [GitHub](https://github.com/Azure/WALinuxAgent)
[Jádro operačního systému](https://coreos.com/docs/running-coreos/cloud-providers/azure/) | 494.4.0+ | V jádra | Zdrojový kód: [GitHub](https://github.com/coreos/coreos-overlay/tree/master/app-emulation/wa-linux-agent)
Debian | Debian 7.9 +, 8.2 + | V jádra | Balíček: V repo v části "waagent" <br/>Zdrojový kód: [GitHub](https://github.com/Azure/WALinuxAgent)
Oracle Linux | 6.4 +, 7.0 + | V jádra | Balíček: V repo v části "WALinuxAgent" <br/>Zdrojový kód: [GitHub](http://go.microsoft.com/fwlink/p/?LinkID=250998)
Červené klobouk Enterprise Linux | RHEL 6,7 + 7.1 + | V jádra|Balíček: V repo v části "WALinuxAgent" <br/>Zdrojový kód: [GitHub](https://github.com/Azure/WALinuxAgent)
SUSE Linux Enterprise | SLES 11 SP4, SLES 12 SP1 + a <p> SLES pro rozhraní SAP 11 SP3 + | V jádra | Balíček: V [Cloudu: nástroje](https://build.opensuse.org/project/show/Cloud:Tools) repo v části "python agenta azure" <br/>Zdrojový kód: [GitHub](http://go.microsoft.com/fwlink/p/?LinkID=250998)
openSUSE | openSUSE 13.2 + | V jádra | Balíček: V [Cloudu: nástroje](https://build.opensuse.org/project/show/Cloud:Tools) repo v části "python agenta azure" <br/>Zdrojový kód: [GitHub](https://github.com/Azure/WALinuxAgent)
Se systémem Ubuntu|Se systémem Ubuntu 12.04, 14.04, 16.04, 16.10 | V jádra | Balíček: V repo v části "walinuxagent" <br/>Zdrojový kód: [GitHub](https://github.com/Azure/WALinuxAgent)


## <a name="partners"></a>Partneři

### <a name="openlogic"></a>OpenLogic
[http://www.openlogic.com/Azure](http://www.openlogic.com/azure)

OpenLogic je počáteční poskytovatele řešení otevřít zdroj organizace – pro cloudu a datovém centru. OpenLogic pomáhá stovky vést enterprise napříč celou řadu odvětví bezpečně získat podporu a řídit otevřít zdroj software. OpenLogic nabízí náhrada 600 otevřít zdroj balíčků podporovaným odborníků komunity OpenLogic, včetně enterprise úrovně podpory CentOS jakož spuštění partnera pro poskytující na základě CentOS obrázky v Azure a technickou podporu obchodní testu.

### <a name="coreos"></a>Jádro operačního systému
[https://coreos.com/docs/running-coreos/cloud-providers/Azure/](https://coreos.com/docs/running-coreos/cloud-providers/azure/)

Na webu jádro operačního systému:

*Jádro operačního systému je určený pro zabezpečení, konzistenci a spolehlivosti. Jádro operačního systému a nikoli jen samotnou balíčků prostřednictvím yum nebo výstižný, používá Linux kontejnery pro správu služby na vyšší úrovni odběru. V kontejneru, která poběží na jednu nebo více počítačů jádro operačního systému balíčku kód jednoho služby a všechny závislosti.*


### <a name="credativ"></a>Credativ
[http://www.credativ.co.uk/credativ-blog/debian-Images-Microsoft-Azure](http://www.credativ.co.uk/credativ-blog/debian-images-microsoft-azure)

Credativ je nezávislý konzultační a služby společnost, vývoj a provádění profesionální řešení použitím bezplatný software. Jako počáteční otevřít zdroj odborníků Credative má mezinárodní rozpoznávání s mnoha IT oddělení pomocí jejich podpory. Spolu s Microsoft Credativ je aktuálně Příprava odpovídající Debian obrázky Debian 8 (Klára) a Debian před 7 (Wheezy), které jsou navrženy speciálně dělat Azure a je možné snadno spravovat pomocí platformu. Credativ se také podporují údržbu dlouhodobou a aktualizace Debian obrázků pro Azure prostřednictvím centra podpory jeho otevřít zdroj.

### <a name="oracle"></a>Oracle
[http://www.Oracle.com/technetwork/topics/cloud/FAQ-1963009.HTML](http://www.oracle.com/technetwork/topics/cloud/faq-1963009.html)

Strategie Oracle je nabízet obecných portfolia řešení pro veřejných a privátních mračnech při pojmenování zákazníci volba a pružnost při jejich nasazení software Oracle mračnech Oracle, jakož i ostatní mračnech.  Oracle partnerství s Microsoftem umožňuje zákazníkům nasazení software Oracle v Microsoft veřejných a privátních mračnech spolehlivé certifikace a podpora společnosti Oracle.  Potvrzení a investic do veřejných a privátních cloudové řešení Oracle Oracle se nezmění.

### <a name="red-hat"></a>Červené klobouk
[http://www.RedHat.com/en/partners/strategic-Alliance/Microsoft](http://www.redhat.com/en/partners/strategic-alliance/microsoft)

Světová organizace počáteční poskytovatele otevřít zdroj řešení, červené klobouk pomáhá více než 90 % Fortune 500 společností řešení podniku, zarovnat jejich IT a obchodní strategie a připravte pro budoucí technologie. Červené klobouk to dělá zadáním řešení zabezpečení prostřednictvím modelu otevřít business a datového modelu pro dostupnou, předvídatelná předplatného.

### <a name="suse"></a>SUSE
[http://www.SuSE.com/SuSE-Linux-Enterprise-Server-on-Azure](http://www.suse.com/suse-linux-enterprise-server-on-azure)

SUSE Linux Enterprise Server na Azure je osvědčené platformy, která poskytuje nadřazené spolehlivost a zabezpečení pro cloudu výpočetních. Na SUSE univerzální Linux platformy Bezproblémová integruje Azure cloudovým službám předvádění prostředí snadno ovladatelným cloudu. A s více než 9,200 certifikované aplikace z více než 1 800 nezávislí dodavatelé softwaru pro SUSE Linux Enterprise Server SUSE zajišťuje, že pracovní vytížení podporovaných v datovém centru lze jistotou nasadit na Azure.

### <a name="canonical"></a>Kanonický
[http://www.Ubuntu.com/cloud/Azure](http://www.ubuntu.com/cloud/azure)

Kanonický a návrh otevřená komunita zásady správného řízení jednotka se systémem Ubuntu společnosti úspěch klienta, serveru cloud computing, včetně osobních cloudovým službám pro uživatele. Kanonický společnosti zrakem sjednocené bezplatné platformy v, se systémem Ubuntu z telefonu do cloudu pomocí řady souvislý rozhraní pro telefon, tablet, televizního a plochy, díky systémem Ubuntu první volba různorodého instituce z veřejné mraků poskytovatelů pracovníkům elektronických příjemce a jako oblíbené položky mezi jednotlivé technologists.

Vývojáři a technickým centra ve světě, Canonical jedinečně umístěn partnerovi s tvůrci hardwaru, poskytovatelé obsahu a vývojáři softwaru přenést řešení se systémem Ubuntu na trh, z počítače na servery a přenosných zařízeních.

