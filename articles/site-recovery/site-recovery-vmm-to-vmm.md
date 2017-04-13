<properties
    pageTitle="Replikovat Hyper-V virtuálních počítačích v VMM mračnech sekundární VMM web pomocí portálu Azure | Microsoft Azure"
    description="Popisuje, jak nasazení obnovení webu Azure k organizovat replikace, převzetí a obnovení Hyper-V VMs v VMM mračnech sekundární VMM web pomocí portálu Azure."
    services="site-recovery"
    documentationCenter=""
    authors="rayne-wiselman"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="site-recovery"
    ms.workload="backup-recovery"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/23/2016"
    ms.author="raynew"/>

# <a name="replicate-hyper-v-virtual-machines-in-vmm-clouds-to-a-secondary-vmm-site-using-the-azure-portal"></a>Replikovat Hyper-V virtuálních počítačích v VMM mračnech sekundární VMM web pomocí portálu Azure

> [AZURE.SELECTOR]
- [Azure portálu](site-recovery-vmm-to-vmm.md)
- [Klasický portálu](site-recovery-vmm-to-vmm-classic.md)
- [Prostředí PowerShell – správce](site-recovery-vmm-to-vmm-powershell-resource-manager.md)

Vítá vás obnovení Azure webu! Použití tohoto článku, pokud chcete replikovat místní Hyper-V virtuálních počítačích spravovaný v mračnech System Center virtuálního počítače Manager (VMM) na vedlejší Web. Tento článek popisuje, jak nastavit replikace pomocí obnovení webu Azure Azure portálu.

> [AZURE.NOTE] Azure obsahuje dva různé [modelů nasazení](../resource-manager-deployment-model.md) pro vytváření grafů a práci s prostředky: Správce prostředků Azure a klasické. Azure má i dvou portály – Azure klasické portál, který podporuje modelu klasické nasazení a portálu Azure s podporou pro oba modely nasazení.


Azure obnovení webu Azure portálu obsahuje několik nových funkcí:

- V Azure portál Azure zálohování a obnovení webu Azure služby zkombinují do jednoho trezoru služby Recovery tak, aby se nastavení a správa nepřerušený a obnovení (BCDR) z jednoho místa. Jednotné řídicího panelu umožňuje sledovat a spravovat operace různých místních webů a Azure veřejné cloudu.
- Uživatelé, kteří mají Azure předplatná zřízení v aplikaci cloudové řešení poskytovatele (CSP) můžete spravovat teď operace obnovení webu Azure portálu.


Po přečtení v tomto článku, pokládat dole v komentářích Disqus komentář. Zeptejte se technické ve [Fóru služby Azure obnovení](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).


## <a name="overview"></a>Základní informace

Organizace potřebovat strategii BCDR, která určuje, jak zůstat spuštěn a k dispozici během plánované a neplánované prostoje aplikace, pracovního vytížení a dat a běžných pracovních podmínek obnovit co nejdříve. Strategie BCDR by měly zachovat obchodní data bezpečných a obnovitelné a zajistit nepřetržitě dostupnost pracovního vytížení případě havárie.

Obnovení webu je služba Azure podíl strategie BCDR tak, že orchestrating replikace místní fyzických serverů a virtuálních počítačích s cloudem (Azure) nebo vedlejší datacentra. Pokud dojde k výpadků ve svém hlavním pracovišti, můžete převzít sekundární umístění, aby aplikace a úloh k dispozici. Můžete být zpět na svém hlavním pracovišti při vrátí na normálně. Další informace najdete v [Co je obnovení webu Azure?](site-recovery-overview.md)

Tento článek obsahuje všechny informace, které je potřeba replikovat místní Hyper-V VMs v VMM mračnech sekundární VMM Web. Obsahuje přehled architektury plánování informace a nasazení kroků pro konfiguraci místní servery nastavení replikace a plánování kapacity. Poté, co jste nastavili infrastruktury můžete povolit replikace v počítačích, které chcete zamknout a zkontrolujte, že tento převzetí funguje.

## <a name="business-advantages"></a>Výhody Business

- Obnovení webu poskytuje ochranu firmy pracovního vytížení a aplikace spuštěné v Hyper-V VMs replikace do vedlejší Hyper-V server.
- Portál služeb obnovení obsahuje jedno místo pro nastavení a správa a sledování replikace, převzetí a obnovení.
- Snadno spustit převzetí služeb při selhání lze spustit z infrastrukturu primární místního sekundární webu a navrácení (obnovení) z sekundární webu na primární.
- Obnovení plány můžete nakonfigurovat s více počítačů tak, aby společně selhání pracovního vytížení vrstvené aplikace.

## <a name="scenario-architecture"></a>Scénář architektura

Toto jsou součástí scénáře:

- **Primární webu**: primární webu existuje jedna nebo více Hyper-V hostitelské servery spouštění virtuálních zdroj, který chcete replikovat. Tyto primární hostitelské servery najdete v cloudu soukromé VMM.
- **Sekundární webu**: sekundární webu jsou jeden nebo více Hyper-V hostitele servery spouštění cílové virtuálních, ke kterým replikovat primární VMs. Tyto hostitelské servery najdete v cloudu soukromé VMM. Cloudu může být na primárním serveru (Pokud máte jenom jednu serveru VMM) nebo na vedlejší VMM serveru.
- **Zprostředkovatel**: během obnovení webu nasazení instalace zprostředkovatele obnovení Azure webů na serverech VMM a zaregistrovat tyto servery služby Recovery trezoru. Zprostředkovatel na serveru VMM informuje uživatele o s obnovení webu přes HTTPS 443 replikovat průběhu. Data replikace mezi servery hostitele Hyper-V primárních a sekundárních. Replikovanou dat zůstává v rámci místního weby a sítě a neodešle Azure. Další informace o [ochraně osobních údajů](#privacy-information-for-site-recovery).

![Topologie e2e](./media/site-recovery-vmm-to-vmm/architecture.png)

### <a name="data-privacy-overview"></a>Přehled datových ochrany osobních údajů

Tato tabulka shrnuje, jak jsou uložena data v takovém případě:
****
Akce | **Podrobnosti** | **Sběr dat** | **Použití** | **Povinné**
--- | --- | --- | --- | ---
**Registrace** | Zaregistrujte VMM serveru služby Recovery trezoru. Pokud chcete později unregister serveru, můžete provedete tak, že odstraníte informace o serveru z portálu Microsoft Azure. | Po registraci serveru VMM obnovení webu shromažďuje, procesy a přenos metadata VMM serveru a název mraků VMM zjištěná obnovení webu. | Data se používá k identifikaci a komunikovat s příslušnou VMM serveru a konfigurace nastavení pro příslušný mračnech VMM. | Tato funkce je potřeba. Pokud nechcete odeslat tyto informace k obnovení webu byste neměli používat službu obnovení webu.
**Povolení replikace** | Poskytovatel obnovení webu Azure je nainstalovaná na serveru VMM a je přenos komunikace se službou obnovení webu. Zprostředkovatel je dynamické knihovny (DLL) hostované v procesu VMM. Po instalaci na poskytovatele získá v konzole pro správu VMM povolené funkce "Datacentra obnovení". Novým i dosavadním VMs můžete povolit tuto funkci povolit ochranu virtuálního počítače. | S touto vlastností nastavenou odešle zprostředkovatel jméno a ID OM obnovení webu.  Ve Windows serveru 2012 nebo Windows Server 2012 R2 Hyper-V otevřené je povolena replikace. Virtuální počítač data získá replikovat z jednoho hostitele Hyper-V do druhého (obvykle umístěné v datovém centru různých "využití"). | Obnovení webu používá metadata k naplnění informací OM Azure portálu. | Tato funkce je důležitou součástí službu a nelze vypnout. Pokud nechcete odeslat tyto informace, není zamknout obnovení webu pro VMs. Všimněte si, že všechna data odeslána poskytovatelem obnovení webu odesílaná HTTPS.
**Obnovení plán** | Obnovení plány můžete vytvořit plán průběhu datového centra obnovení. Můžete určit pořadí, ve kterém by měl být zahájen VMs nebo skupinu virtuálních počítačích na webu obnovení. Můžete taky určit automatické skripty být běžel nebo všechny ruční provést akci, v době obnovení pro každý OM. Převzetí se obvykle při spuštění aktivují na úrovni plán obnovení koordinovaný obnovení. | Obnovení webu shromažďuje, zpracuje a přenáší metadat pro obnovení plánu, včetně metadata virtuálního počítače a metadata automatizaci skripty a ruční akce poznámky. | Metadata slouží k vytvoření plánu pro obnovení na portálu Azure. | Tato funkce je důležitou součástí službu a nelze vypnout. Pokud nechcete odeslat tyto informace k obnovení webu, nevytvářejte obnovení plány.
**Mapování sítě** | Mapy sítě informace z primární datovém centru datacentrem obnovení. Když VMs jsou obnovit na webu obnovení, mapování sítě pomáhá při vytváření připojení k síti. | Obnovení webu shromažďuje, zpracuje a přenáší metadat logických sítí pro každý web (primární a datacentra). | Metadata slouží k naplnění nastavení sítě tak, aby mohli porovnat údaje sítě. | Tato funkce je důležitou součástí službu a nelze vypnout. Pokud nechcete odeslat tyto informace k obnovení webu, nepoužívejte mapování sítě.
**Převzetí (plánované nebo neplánované/zkoušení)** | Převzetí selhání VMs z jednoho VMM Správa přístupových práv datacentra. Převzetí akce se při spuštění aktivují ručně Azure portálu. | Zprostředkovatel na serveru VMM proběhne události převzetí obnovení webu a spustí akci převzetí na hostiteli Hyper-V prostřednictvím rozhraní VMM. Skutečné převzetí virtuálního počítače je z jednoho hostitele Hyper-V jiné a zpracování systému Windows Server 2012 nebo Windows Server 2012 R2 Hyper-V otevřené. Po dokončení převzetí zprostředkovatele na serveru VMM v datovém centru obnovení odesílá úspěch informace o obnovení webu. | Obnovení webu použije informace odesílané do naplnění stav informace o převzetí akci Azure portálu. | Tato funkce je důležitou součástí službu a nelze vypnout. Pokud nechcete odeslat tyto informace k obnovení webu, nepoužívejte překlopení.


## <a name="azure-prerequisites"></a>Požadavky na Azure

Tady je co budete potřebovat v Azure nasazení situaci:

**Zjistit předpoklady pro** | **Podrobnosti**
--- | ---
**Azure**| Je třeba účet [Microsoft Azure](http://azure.microsoft.com/) . Můžete začít s [bezplatnou zkušební verzi](https://azure.microsoft.com/pricing/free-trial/). [Další informace](https://azure.microsoft.com/pricing/details/site-recovery/) o obnovení webu ceny.


## <a name="on-premises-prerequisites"></a>Zjistit předpoklady pro místní

Tady je, co je potřeba na webech primárních a sekundárních místního nasazení situaci:

**Zjistit předpoklady pro** | **Podrobnosti**
--- | ---
**VMM** | Doporučujeme že nasazení serveru VMM primární webu a serveru VMM sekundární webu.<br/><br/> Můžete také [replikovat mezi mračnech na jednom VMM serveru](site-recovery-single-vmm.md). K tomu potřebujete alespoň dvě mračnech nakonfigurovaný na serveru VMM.<br/><br/> Servery VMM by měla běžet aspoň System Center 2012 SP1 s nejnovějšími aktualizacemi.<br/><br/> Každý VMM server musí mít na jednom nebo více mračnech nakonfigurované a všechny shluky musí mít profilu kapacity Hyper-V nastavení. <br/><br/>Mračnech musí obsahovat jednu nebo více skupin VMM Host (hostitel).<br/><br/>Další informace o nastavení VMM mračnech při [konfiguraci struktury cloudu VMM](https://msdn.microsoft.com/library/azure/dn469075.aspx#BKMK_Fabric), a [Návod: Vytvoření soukromého mračnech s System Center 2012 SP1 VMM](http://blogs.technet.com/b/keithmayer/archive/2013/04/18/walkthrough-creating-private-clouds-with-system-center-2012-sp1-virtual-machine-manager-build-your-private-cloud-in-a-month.aspx).<br/><br/> Servery VMM potřebovat přístup k Internetu.
**Hyper-V** | Pro Hyper-V serverech musí běžet aspoň Windows Server 2012 s rolí Hyper-V a nainstalovali nejnovější aktualizace.<br/><br/> Server pro Hyper-V smí obsahovat jeden nebo více VMs.<br/><br/>  Hostitelské Hyper-V servery musí být umístěny v hostitele skupin v VMM mračnech primárních a sekundárních.<br/><br/> Pokud používáte Hyper-V clusteru v systému Windows Server 2012 R2 byste si měli nainstalovat [aktualizace 2961977](https://support.microsoft.com/kb/2961977)<br/><br/> Pokud používáte Hyper-V clusteru v systému Windows Server 2012, Poznámka: Tento zprostředkovatel obrázku není automaticky vytvoří, pokud máte statické IP adresy clusteru založený. Budete muset ručně nakonfigurovat zprostředkovatele obrázku. [Přečtěte si další](http://social.technet.microsoft.com/wiki/contents/articles/18792.configure-replica-broker-role-cluster-to-cluster-replication.aspx).
**Zprostředkovatel** | Během obnovení webu nasazení instalace zprostředkovatele obnovení webů Azure na serverech VMM. Zprostředkovatel informuje uživatele o s obnovení webu přes HTTPS 443 za účelem organizovat replikace. Data replikace mezi servery Hyper-V primárních a sekundárních přes síť LAN nebo připojení VPN.<br/><br/> Zprostředkovatel na serveru VMM potřebuje přístup k tyto adresy URL: *. hypervrecoverymanager.windowsazure.com; *. AccessControl.Windows.NET; *. backup.windowsazure.com; *. BLOB.Core.Windows.NET; *. store.core.windows.net.<br/><br/> Kromě toho komunikovat bránu firewall od serverů VMM [Azure datacentra rozsahy IP adres](https://www.microsoft.com/download/confirmation.aspx?id=41653) a povolte protokol HTTPS (443).

## <a name="prepare-for-deployment"></a>Připravte pro nasazení

Příprava nasazení je potřeba:

1. [Příprava serveru VMM](#prepare-the-vmm-server) nasazení obnovení webu.
2. [Příprava na mapování sítě](#prepare-for-network-mapping). Nastavení sítě tak, že můžete konfigurovat mapování sítě.

### <a name="prepare-the-vmm-server"></a>Příprava VMM serveru

Ujistěte se, že VMM server splňuje [požadavky](#on-premises-prerequisites) a přístup k seznamu adres URL.


### <a name="prepare-for-network-mapping"></a>Příprava na mapování sítě

Mapy sítí mapování mezi VMM OM sítí na serverech VMM primárních a sekundárních a:

- Optimální včasné otevřené VMs sekundárním hosts Hyper-V po překlopení.
- Otevřené VMs připojte k odpovídající OM sítě.
- Pokud není konfigurace sítě mapování otevřené VMs nesmí být připojeni k jakákoli síť po překlopení.
- Pokud chcete nastavit síť mapování během obnovení webu nasazení tady je budete potřebovat:

    - Ujistěte se, že jsou VMs na hostitelském serveru Hyper-V zdroj připojení k síti VMM OM. Síť by měl být propojené s logická síť, která je přidružená k cloudu.
    - Ověřte, zda sekundární cloudu, který použijete pro obnovení síti odpovídající OM nakonfigurované. Síť OM by měl být propojené s logické sítě, který máte přidružený k sekundární cloudu.

- [Další informace](site-recovery-network-mapping.md) o tom, jak funguje mapování sítě.

## <a name="prepare-for-deployment-with-a-single-vmm-server"></a>Příprava na nasazení se serverem jednoho VMM

Pokud máte jenom jednu VMM serveru můžete replikovat VMs v Hyper-V hosts v cloudu VMM [Azure](site-recovery-vmm-to-azure.md) nebo vedlejší VMM cloudu. Doporučujeme, abyste že první možnost protože replikace mezi mračnech není bezproblémové, ale pokud je potřeba provést tady je co musíte udělat:

1. **Nastavení VMM na OM Hyper-V**. V takovém případě to měli byste že umístěním instanci systému SQL Server používané VMM na stejné OM (kolokací). Tím ušetříte čas, dokud jedinou OM má vytvořit. Pokud chcete použít dojde k vzdálené instance serveru SQL Server a výpadku, je potřeba obnovit tuto instanci před můžete obnovit VMM.
2. **Zkontrolujte, zda serveru VMM obsahuje aspoň dva mračnech nakonfigurované**. Jeden cloudu bude obsahovat VMs chcete replikovat a v cloudu bude sloužit jako sekundární umístění. [Požadavky](#on-premises-prerequisites)musí odpovídat v cloudu, který obsahuje VMs chcete zamknout.
3. Nastavte si obnovení webu popsaným v tomto článku. Vytvoření a zaregistrujte serveru VMM v trezoru, nastavení zásad replikace a povolte replikace. Měli byste určit, že počáteční replikace probíhá v síti.
4. Když nastavíte mapování sítě namapujte OM síť pro primární cloudu OM síť pro sekundární cloudu.
5. V konzole pro Hyper-V správce povolit Hyper-V otevřené na hostiteli Hyper-V obsahující VMM OM a replikace v OM. Zkontrolujte, že není přidáte virtuálního počítače VMM shluky, které jsou chráněné technologií vybírání webu, ujistěte se, že nastavení Hyper-V otevřené přepsáno tak, že obnovení webu.
6. Pokud vytvoříte obnovení plány pro překlopení používat stejný server VMM pro zdrojový a cílové.
7. Převzetí a obnovit takto:

    - Ruční převzetí OM VMM sekundární web pomocí plánované přepojení Hyper-V správce.
    - Převzetí VMs.
    - Po -> primární, který byl obnoven VMM OM, přihlaste se k portálu Azure služby Recovery trezoru a spusťte neplánované převzetí VMs z sekundární webu na primární Web.
    - Po dokončení neplánované převzetí všechny zdroje můžete k nim získat přístup z primární webu znovu.


### <a name="create-a-recovery-services-vault"></a>Vytvoření trezoru obnovení služby

1. Přihlaste se k [portálu Azure](https://portal.azure.com).
2. Klikněte na **Nový** > **správy** > **obnovení služby**. Můžete taky můžete kliknutím na **Procházet** > **Služby Recovery** trezorů > **Přidat**.

    ![Nové trezoru](./media/site-recovery-vmm-to-vmm/new-vault3.png)

3. Do pole **název** zadejte popisný název k identifikaci trezoru. Pokud máte víc předplatných, vyberte jednu z nich.
4. [Vytvoření nové skupiny prostředků](../resource-group-template-deploy-portal.md) nebo vyberte stávající a zadejte Azure oblast. K této oblasti bude replikovat počítačích. Chcete-li zkontrolovat podporovaných oblastí naleznete v tématu zeměpisné dostupnost podrobně [Azure webu obnovení ceny](https://azure.microsoft.com/pricing/details/site-recovery/)
4. Pokud chcete rychle zobrazit trezoru na řídicím panelu klikněte na **Připnout na řídicí panel** > **vytvořit trezoru**.

    ![Nové trezoru](./media/site-recovery-vmm-to-vmm/new-vault-settings.png)

Nové trezoru se zobrazí na **řídicím panelu** > **všechny zdroje**a na hlavní **služby Recovery trezorů** zásuvné.




## <a name="getting-started"></a>Začínáme

Obnovení webu poskytuje začínáte pracovat, který vám pomůže možnosti nasazení co nejdříve. Začínáme zkontroluje požadavky a provede vás obnovení webu nasazení kroky ve správném pořadí.

Při zahájení vyberte typ strojů chcete replikovat a místo, kam chcete replikovat do. Nastavte si místní servery, vytvořit zásady replikace a plánovat kapacity. Když jste nastavili infrastrukturu povolíte replikace VMs. Spuštění převzetí služeb při selhání pro konkrétní počítače nebo vytvořte plány obnovy překlopení více počítačů.

Začněte Začínáme – zvolte, jak chcete nasadit obnovení webu. Začínáme toku změny mírně v závislosti na vašim požadavkům replikace.

## <a name="step-1-choose-your-protection-goal"></a>Krok 1: Volba cíl ochrany

Vyberte, co chcete replikovat a místo, kam chcete replikovat do.

1. V zásuvné **služby Recovery trezorů** vyberte trezoru a klikněte na **Nastavení**.
2. V **dialogovém okně Nastavení** > **Začínáme** klikněte na **Obnovení webu** > **Krok 1: Příprava infrastruktury** > **cíl zámek**.
3. V **ochranu hledání** vyberte **obnovení web**a vyberte **Ano, pomocí Hyper-V**.
4. Vyberte možnost **Ano** označíte, že používáte VMM spravovat hosts Hyper-V a vyberte **Ano** , pokud máte sekundární VMM server. Pokud jste nasazení replikace mezi mračnech na jednom serveru VMM klepnete na **Ne**. Klikněte na **OK**.

    ![Volba cíle](./media/site-recovery-vmm-to-vmm/choose-goals.png)


## <a name="step-2-set-up-the-source-environment"></a>Krok 2: Nastavení zdrojového prostředí

Instalace zprostředkovatele obnovení Azure webů na serverech VMM a zaregistrujte servery v trezoru.

1. Klikněte na **Krok 2: Příprava infrastruktury** > **zdroje**.

    ![Nastavení zdroje](./media/site-recovery-vmm-to-vmm/goals-source.png)

2. **Příprava** zdroje klikněte na **+ VMM** přidání VMM serveru.

    ![Nastavení zdroje](./media/site-recovery-vmm-to-vmm/set-source1.png)

2. V zobrazené **Přidat Server** zásuvné políčko **System Center VMM serveru** **Typ serveru** a VMM server splňuje [požadavky a požadavky na adresu URL](#on-premises-prerequisites).
4. Poskytovatel obnovení webu Azure instalace budou moct soubor stáhněte.
5. Stáhněte si registračního klíče. Musíte to při spuštění instalačního programu. Klíč platí 5 dní po jeho vytvoření.

    ![Nastavení zdroje](./media/site-recovery-vmm-to-vmm/set-source3.png)

6. Obnovení poskytovatel webu Azure nainstalujte na VMM server.

> [AZURE.NOTE] Nemusíte explicitně instalovat nic na serverech hostitele Hyper-V.


### <a name="set-up-the-azure-site-recovery-provider"></a>Nastavení poskytovatele obnovení webu Azure

1. Spusťte instalační soubor na všech serverech VMM zprostředkovatele. Pokud VMM nasazenou v clusteru a instalujete poskytovatele první nainstalovat na aktivní uzel a dokončete instalaci registrace serveru VMM v trezoru. V jednotlivých uzlech nainstalujte na poskytovatele. Všechny uzlech by měla běžet stejnou verzi aplikace na poskytovatele.
2. Instalační program spustí několik prerequirement kontroly a žádosti o oprávnění k zastavit službu VMM. Po dokončení instalace je bude třeba restartovat službu VMM automaticky. Pokud instalujete clusteru VMM budete výzva k ukončení roli obrázku.

2.  Ve **Službě Microsoft Update** můžete zvolit v aktualizace tak, aby se mají instalovat aktualizace poskytovatele v souladu se zásadami Microsoft Update.
3. Při **instalaci** přijměte nebo změnit výchozí umístění instalace zprostředkovatele a klikněte na tlačítko **nainstalovat**.

    ![Umístění instalace](./media/site-recovery-vmm-to-vmm/provider-location.png)

3. Po dokončení instalace klikněte na **zaregistrovat** k registraci serveru v trezoru.

    ![Umístění instalace](./media/site-recovery-vmm-to-vmm/provider-register.png)

9. Do pole **název trezoru**ověřte název trezoru, ve kterém bude registrován serveru. Klikněte na tlačítko *Další*.

    ![Registrace serveru](./media/site-recovery-vmm-to-vmm-classic/vaultcred.PNG)

7. Připojení k **Internetu** určete způsob poskytovatele na serveru VMM připojení k Internetu. Vyberte **připojení s nastavením proxy serveru existující** používat výchozí nastavení připojení k Internetu nakonfigurovaný na serveru.

    ![Nastavení sítě Internet](./media/site-recovery-vmm-to-vmm-classic/proxydetails.PNG)

    - Pokud chcete použít vlastní proxy by měl nastavením před instalací zprostředkovatele. Při konfiguraci nastavení serveru proxy vlastní test se spustí při kontrole připojení proxy k.
    - Pokud používáte vlastní proxy nebo výchozí proxy server vyžaduje ověření budete muset zadat proxy podrobnosti, včetně adresy proxy serveru a portu.
    - Sledovat URL by měl být přístupné z informací o serveru VMM a hosts Hyper-v
        - *. hypervrecoverymanager.windowsazure.com
        - *. accesscontrol.windows.net
        - *. backup.windowsazure.com
        - *. blob.core.windows.net
        - *. store.core.windows.net
    - Povolte IP adresy popsané v [Rozsahu IP Azure datacentru](https://www.microsoft.com/download/confirmation.aspx?id=41653) a HTTPS (443) Protocol (protokol). Máte by a rozsahy IP adres bílé seznamu Azure oblasti, kterou chcete použít, západ USA.
    - Pokud používáte vlastní proxy VMM RunAs účtu (DRAProxyAccount) se vytvoří automaticky pomocí přihlašovacích údajů zadané proxy. Konfigurace proxy server tak, aby tento účet může ověřovat úspěšně. Nastavení účtu VMM RunAs můžete měnit v konzole VMM. K tomuto účelu **Nastavení** pracovní prostor otevřít, rozbalte **zabezpečení**, klikněte na **Spustit jako účty**a potom změnit heslo pro DRAProxyAccount. Je potřeba restartovat službu VMM tak, aby toto nastavení se projeví.


8. **Registrační klíč**vyberte klíč, který stahujete obnovení webu Azure a zkopírovala do VMM serveru.


10.  Nastavení šifrování se používá pouze když jste replikace Hyper-V VMs v VMM mračnech Azure. Pokud jste replikace sekundární web nepoužívá.

11.  V poli **název serveru**zadejte popisný název k identifikaci VMM serveru v trezoru. V konfiguraci clusteru zadejte název VMM clusteru role.
12.  V poli vyberte **synchronizovat cloudu metadat** jestli chcete k synchronizaci metadat pro všechny shluky na serveru VMM s trezoru. Tato akce jenom s jednou na všech serverech musí. Pokud nechcete synchronizovat všechny shluky, můžete nechat toto nastavení zaškrtnuté a synchronizovat každý cloudu jednotlivě ve vlastnostech cloudu v konzole VMM.

13.  Klikněte na **Další** a dokončit. Po registraci metadata ze serveru VMM načítané obnovení webu Azure. Server se zobrazí na kartě **Servery VMM** na stránce **servery** v trezoru.

    ![Server](./media/site-recovery-vmm-to-vmm-classic/provider13.PNG)

11. Po server je k dispozici v konzole obnovení webu ve **zdroji** > **připravit zdroj** vyberte VMM serveru a ve kterém se nachází hostiteli Hyper-V cloudu. Klikněte na **OK**.

#### <a name="command-line-installation"></a>Přepínač příkazového řádku instalace

Obnovení Provider webu Azure můžete nainstalovat z příkazového řádku. Tato metoda slouží k instalaci zprostředkovatele na Server Core pro Windows Server 2012 R2.

1. Stáhněte instalační soubor a registrace klíč poskytovatele do složky. Dejme tomu C:\ASR.
2. Zastavte službu System Center virtuálního počítače správce.
3. Na příkazovém řádku, spusťte tyto příkazy extrahovat instalační program poskytovatele:

        C:\Windows\System32> CD C:\ASR
        C:\ASR> AzureSiteRecoveryProvider.exe /x:. /q

4. Spusťte tento příkaz instalace zprostředkovatele:

        C:\ASR> setupdr.exe /i

5. Spusťte tyto příkazy k registraci serveru v trezoru:

        CD C:\Program Files\Microsoft System Center 2012 R2\Virtual Machine Manager\bin
        C:\Program Files\Microsoft System Center 2012 R2\Virtual Machine Manager\bin\> DRConfigurator.exe /r  /Friendlyname <friendly name of the server> /Credentials <path of the credentials file> /EncryptionEnabled <full file name to save the encryption certificate>     

Kde jsou parametry:

 - **/Credentials**: povinný parametr, který určuje umístění, ve kterém je umístěn soubor klíčové registrace  
 - **/FriendlyName**: povinný parametr název serveru hostitele Hyper-V, která se zobrazí v portálu obnovení webu Azure.
 - **/EncryptionEnabled**: volitelný parametr, který používáte jenom při replikace z VMM Azure.
 - **/proxyAddress**: volitelný parametr, který určuje adresy proxy serveru.
 - **/proxyport**: volitelný parametr, který určuje port proxy serveru.
 - **/proxyUsername**: volitelný parametr, který určuje Proxy uživatelské jméno (pokud proxy server vyžaduje ověření).
 - **/proxyPassword**: volitelný parametr, který určuje heslo pro ověřování proxy serveru (pokud proxy server vyžaduje ověření).  

## <a name="step-3-set-up-the-target-environment"></a>Krok 3: Nastavení cílové prostředí

Vyberte cílový VMM server a cloudu.

1. Klikněte na **připravit infrastruktury** > **cílové** a vyberte cílový server VMM chcete použít.
2.  Zobrazí se mračnech na serveru, které jsou synchronizovány s obnovení webu. Vyberte cílový cloudu.

    ![Cíl](./media/site-recovery-vmm-to-vmm/target-vmm.png)

## <a name="step-4-set-up-replication-settings"></a>Krok 4: Nastavení replikace

1. Vytvořit nové replikace zásady klikněte na možnost **připravit infrastruktury** > **Nastavení replikace** > **+ vytvořit a přidružit**.

    ![Sítě](./media/site-recovery-vmm-to-vmm/gs-replication.png)

2. **Vytvoření** a přiřazení zásad zadejte název zásady. Typ zdrojové a cílové by měl být **Hyper-V**.
3. V **Hyper-V host verze** vyberte operační systém, který běží v hostiteli.

    > [AZURE.NOTE] VMM cloud může obsahovat Hyper-V hosts s různými verzemi (podporované) systému Windows Server, ale zásady replikace je použité hosts stejný operační systém. Pokud máte hosts s více než jeden operační systém verze vytvořte samostatné replikace zásady.

4. **Typ ověřování** a **ověřování port** určete, jak přenosy ověřené mezi servery hostitele Hyper-V primární a obnovení. Vyberte **certifikát** , pokud máte pracovní prostředí protokol Kerberos. Obnovení Azure webu automaticky nakonfiguruje certifikáty pro ověřování HTTPS. Nemusíte udělat nic ručně. Ve výchozím nastavení se otevře v bráně Firewall systému Windows na serverech hostitele Hyper-V port 8083 a 8084 (pro certifikáty). **Pomocí protokolu Kerberos**, vyberete lístek protokolu Kerberos se použijí k vzájemnému ověření hostitelské servery. Všimněte si, že toto nastavení platí jenom pro Hyper-V hostiteli serverech běží v systému Windows Server 2012 R2.
3. V **kopírování počet_plateb** určete, jak často chcete replikovat delta data po počáteční replikace (každých 30 sekund, 5 až 15 minut).
4. V **obnovení přejděte uchovávání informací**zadejte do několika hodin doby okně uchovávání informací pro vás bude každý obnovení bod. Chráněný počítačích můžete obnovit až kamkoli do okna.
6. V **aplikaci konzistentní snímek četnost** zadejte frekvence (1 až 12 hodin) obnovení body obsahující konzistenci aplikací snímky bude vytvořen. Pro Hyper-V použití dva typy snímků – standardní snímek, který obsahuje dílčí snímek celý virtuálního počítače a konzistenci aplikací snímek, který vytvoří v okamžiku snímek dat aplikace do virtuálního počítače. Aplikace konzistentní snímky umožňuje zajistit, aby aplikace do konzistentního stavu po snímku, se považuje hlasitost stín kopie služby (VSS). Všimněte si, že pokud povolíte konzistenci aplikací snímky, bude to mít vliv výkonu aplikacím na zdroj virtuálních počítačích. Zajistěte, aby byl hodnotu, kterou nastavíte menší než počet bodů další obnovení, které jste nakonfigurovali.
7. V **Komprese přenosu dat** určete, zda by měly replikovanou data, která jsou přenášena komprimovány.
8. Zaškrtněte políčko **Odstranit otevřené OM** můžete určit, že virtuální počítač otevřené by měl odeberou, když zakážete ochranu pro tento zdroj OM. Je-li toto nastavení používají, když zakážete ochranu pro tento zdroj OM, je odebraná z konzoly obnovení webu, obnovení webu nastavení VMM budou odebrány z konzoly VMM a odstranit otevřené.
3. **Počáteční replikace** metody Pokud jste replikace v síti určete, zda začínat počáteční replikace a naplánujte její. Uložte šířka pásma můžete naplánovat mimo zaneprázdněné hodin. Klikněte na **OK**.

    ![Zásady replikace](./media/site-recovery-vmm-to-vmm/gs-replication2.png)

6. Při vytváření nové zásady máte automaticky přidružený k VMM cloudu. **Replikace** zásady klikněte na **OK**. Můžete přiřadit další mračnech VMM (a VMs v nich) tuto zásadu replikace v **dialogovém okně Nastavení** > **replikace** > název zásady > **Přidružení VMM cloudu**.

    ![Zásady replikace](./media/site-recovery-vmm-to-vmm/policy-associate.png)

### <a name="prepare-for-offline-initial-replication"></a>Příprava na offline počáteční replikace

Offline replikace kopie počátečního data můžete udělat. Funkce usnadnění můžete připravit to takto:

- Na zdrojovém serveru zadáte umístění, ze které bude export dat proběhnout. Přiřadíte oprávnění NTFS a sdílení ke službě VMM na cestě pro export úplné řízení. Na cílovém serveru zadáte umístění, ze kterého dojde k importu dat. Přiřazení stejná oprávnění na cesta k importu.
- Pokud je sdílený import nebo export cestu, přiřaďte správce, Power uživatele, tisk operátor nebo operátor serveru členství ve skupinách VMM účtu služby ve vzdáleném počítači, na kterém sdíleného nachází.
- Pokud používáte všechny spustit jako účty, které pokud chcete přidat hosts na cestách import a export přiřazení pro čtení a zápis spustit jako v seznamu účty VMM.
- Import a export akcie neměli nachází na kterýkoliv počítač používá jako host server Hyper-V, protože smyčky konfigurace neposkytuje podporu pro Hyper-V.
- Ve službě Active Directory v jednotlivých Hyper-V vynucené serveru, který obsahuje virtuálních počítačích, které chcete zamknout, povolení a konfigurace delegování s informacemi o důvěryhodnosti vzdálených počítačů, na kterých import a export cesty nacházejí, následujícím způsobem:
    1. U řadiče domény otevřete **uživatele služby Active Directory a počítačů**.
    2. Ve stromové nabídce klikněte na **název_domény** > **počítačů**.
    3. Klikněte pravým tlačítkem na název serveru hostitele Hyper-V > **Vlastnosti**.
    4. Na kartě **delegování** klikněte na **Důvěřovat tomuto počítači pro delegování určeným službám**.
    5. Klikněte na tlačítko **použít libovolný protokol pro ověřování**.
    6. Klikněte na tlačítko **Přidat** > **uživatelů a počítačů**.
    7. Zadejte název počítače hostujícího cestě pro export > **OK**. Ze seznamu dostupných služeb, podržte stisknutou klávesu CTRL a klikněte na **cifs** > **OK**. Opakujte pro název počítače hostujícího cesta importu. Opakujte podle potřeby pro další Hyper-V hostitelské servery.


### <a name="configure-network-mapping"></a>Konfigurace mapování sítě

Nastavte si síť mapování mezi zdrojovým a cílovým sítě.

- [Pro čtení](#prepare-for-network-mapping) pro rychlý přehled toho, jaké mapování sítě nepodporuje. V přidání [Přečtěte si](site-recovery-network-mapping.md) podrobnější vysvětlení.
- Ověřte, že jsou virtuálních počítačích na serverech VMM připojení k síti OM.

Konfigurace mapování následujícím způsobem:

1. **Nastavení** > **Webu obnovení infrastruktury** > **Mapování sítě** > **sítě mapování** , klikněte na tlačítko **+ mapování sítě**.

    ![Mapování sítě](./media/site-recovery-vmm-to-azure/network-mapping1.png)

2. Na kartě **Přidat síťové mapování** vyberte zdroj výsledků a cílového webu VMM servery. OM sítě přidružené k serverům VMM načtením.
3. **Zdrojová síť**vyberte sítě, kterou chcete použít ze seznamu OM sítě přidružené k primární VMM server.
6. V **cílové síti** vyberte síť, kterou chcete použít na vedlejší VMM serveru. Klikněte na **OK**.

    ![Mapování sítě](./media/site-recovery-vmm-to-vmm/network-mapping2.png)

Tady je, co se stane, když začíná mapování sítě:

- Všechny existující otevřené virtuálních počítačích, které odpovídají zdrojová OM síť bude připojen k síti OM cílové.
- Nové virtuálních počítačích, která jsou připojená k síti OM zdroj bude připojen k síti cílové namapované po replikace.
- Pokud změníte existující mapování s novou síť, otevřené virtuálních počítačích připojení pomocí nové nastavení.
- Pokud cílové síti má více podsítí a jednu z těchto podsítí má stejný název jako podsítě nachází virtuálního počítače zdroje, bude možné virtuální počítač otevřené připojení k této cílové podsítě po překlopení. Pokud žádná cílové podsítě s odpovídajícím názvem virtuální počítač připojen k první podsítě v síti.

### <a name="configure-storage-mapping"></a>Konfigurace mapování úložiště
Ve výchozím nastavení při replikovat virtuálního počítače na hostitelském serveru Hyper-V zdroj k serveru hostitele Hyper-V cílové replikovanou data jsou uložena ve výchozím umístění, který je označený cílového hostitele Hyper-V ve Správci Hyper-V. Větší kontrolu nad kam se ukládají replikovanou data můžete nakonfigurovat úložiště mapování<br/><br/> Abyste mohli nakonfigurovat úložiště mapování, potřebujete nastavení klasifikace úložiště ve zdroji a cílového VMM servery, než začnete nasazení. Mapování úložiště prostřednictvím nového Azure portálu aktuálně nepodporuje. Však může být užitečné prostřednictvím Powershellu. [Další informace](site-recovery-vmm-to-vmm-powershell-resource-manager.md#step-6-configure-storage-mapping).

## <a name="step-5-capacity-planning"></a>Krok 5: Plánování kapacity

Teď, když máte v basic infrastruktury nastavení se může přemýšlet o plánování kapacity a zjistit, jestli budete potřebovat další zdroje informací.

Obnovení webu poskytuje aplikace Excel kapacity Plánovač vám přidělit správné zdroje na prostředí zdroje, součásti obnovení webů, sítě a úložiště. Plánování můžete spustit v rychlém režimu odhady podle průměrný počet VMs, disků a úložišť nebo v podrobné režimu, ve kterém se vstupní hodnoty na úrovni zátěží na projektu. Než začnete musíte:

- Shromážděte informace o prostředí replikace, včetně VMs disků za VMs a úložiště na disku.
- Odhad rychlosti denní změnit (konve), které budete mít k dispozici pro replikovanou delta data. [Plánování doby pro Hyper-V otevřené](https://www.microsoft.com/download/details.aspx?id=39057) umožňuje vám to udělat.

1.  Klikněte na **Stáhnout** pro stažení nástroje a pak ho spusťte. [Přečtěte si tento článek](site-recovery-capacity-planner.md) , který doprovází nástroj.
2.  Až to budete mít v **jste spustili plánovači kapacita**výběrem **Ano** ?

    ![Plánování kapacity](./media/site-recovery-vmm-to-azure/gs-capacity-planning.png)

### <a name="control-bandwidth"></a>Ovládací prvek pásma

Po získaných v reálném čase delta replikace informací pomocí kapacita Plánovač Hyper-V otevřené nástroje Plánovač kapacita založený na aplikaci Excel můžete vypočítat šířku pásma, které potřebujete pro replikace (počáteční a delta). K řízení počtu šířky pásma pro replikační můžete nakonfigurovat NetQos zásady použití zásad skupiny nebo prostředí Windows PowerShell. Můžete to udělat několika způsoby:

**Prostředí PowerShell** | **Podrobnosti**
--- | ---
**Nové - NetQosPolicy "QoS podsítě cílový"** | Omezení adres Hyper-V host spuštěný Windows Server 2012 R2 sekundární podsítě. Použijte, pokud primárních a sekundárních podsítí se liší.
**Nové - NetQosPolicy "QoS cílový port"** | Omezení přenosy hostiteli Hyper-V systémem Windows Server 2012 R2 cílový port.
**Nové - NetQosPolicy "Omezení adres VMM"** | Omezení adres vmms.exe. To bude omezení Hyper-V přenosy a Live přenosy migrace. Je možné porovnávat IP protokoly a porty Upřesnit.

Můžete použít nastavení šířky pásma weight (váha) nebo omezit přenosy bitů sekundární. Pokud používáte clusteru je potřeba provést na všechny clusteru. Další informace najdete tady:


- Blog Kutěj Maurer zatížení [omezení Hyper-V otevřené](http://www.thomasmaurer.ch/2013/12/throttling-hyper-v-replica-traffic/)
- Další informace o [rutinu New-NetQosPolicy](https://technet.microsoft.com/library/hh967468.aspx).


## <a name="step-6-enable-replication"></a>Krok 6: Povolit replikace

Teď povolte následujícím způsobem:

1. Klikněte na **Krok 2: replikovat aplikace** > **zdroje**. Po povolení replikace poprvé kliknete na **+ replikovat** trezoru povolit replikace pro další počítače.

    ![Povolení replikace](./media/site-recovery-vmm-to-vmm/enable-replication1.png)

2. V zásuvné **zdroj** > Vybrat VMM serveru a v cloudu, ve kterém chcete replikovat hosts Hyper-V nacházejí. Klikněte na **OK**.

    ![Povolení replikace](./media/site-recovery-vmm-to-vmm/enable-replication2.png)

3. V **cílové** zásuvné ověřte sekundární VMM server a cloudu.
4. Ve **virtuálních počítačích** vyberte VMs chcete zamknout ze seznamu.

    ![Povolení ochrany virtuálního počítače](./media/site-recovery-vmm-to-vmm/enable-replication5.png)

Můžete sledovat průběh **Povolit ochranu** akce v dialogovém okně Nastavení > **úlohy** > **úloh obnovení webu**. Po dlouhodobě spuštěná úloha **Dokončení ochrany** je připraven k převzetí virtuálního počítače.


>[AZURE.NOTE] Můžete taky povolit ochranu virtuálních počítačích v konzole VMM. Klikněte na **Povolit ochranu** na panelu nástrojů v vlastnosti virtuálního počítače > karta **Obnovení webu Azure** .

Když jste povolili replikace můžete zobrazit vlastnosti pro OM v **dialogovém okně Nastavení** > **Replikovat položky** > OM název. Na řídicím panelu **Essentials** uvidíte informace o zásady replikace OM a její stav. Další podrobnosti klikněte na **Vlastnosti** .

### <a name="onboard-existing-virtual-machines"></a>Integrovaný existující virtuálních počítačích

Pokud máte existující virtuálních počítačích v VMM, které jsou replikace Hyper-V otevřené můžete provést tyto akce palubě jejich obnovení webu Azure ochranu takto:

1. Ujistěte se, že server pro Hyper-V hostování existující OM je umístěné v cloudu, primární a serveru pro Hyper-V hostování otevřené virtuálního počítače je umístěn v sekundární cloudu.
2. Zkontrolujte, že nakonfigurované zásady replikace primární VMM cloudu.
2. Povolte pro primární virtuální počítač. Azure obnovení webu a VMM zajistí, že ke zjištění stejného hostitele otevřené a virtuálního počítače a obnovení webu Azure se znovu použít a obnovit replikace pomocí zadané nastavení.


## <a name="step-7-test-your-deployment"></a>Krok 7: Testování nasazení

Testování nasazení můžete spustit přepojení test pro jeden počítač virtuální nebo vytvořte obnovení plán, který obsahuje jeden nebo více virtuálních počítačích.

### <a name="prepare-for-failover"></a>Příprava překlopení

- Plně otestovat nasazení musíte infrastrukturu pro replikovanou počítač tak, aby fungovaly očekávaným. Pokud chcete testovat Active Directory a DNS můžete vytvořit virtuální počítač jako řadiče domény se službou DNS a replikovat to Azure pomocí obnovení webu Azure. Přečtěte si další v [aspektech převzetí test pro služby Active Directory](site-recovery-active-directory.md#considerations-for-test-failover).
- Pokyny v tomto článku se dočtete, jak pomocí přepojení test žádná síť. Tuto možnost, bude otestovat, aby OM selhání, ale nebude otestujte nastavení sítě pro OM. [Další informace](site-recovery-failover.md#run-a-test-failover) o jiných možnostech.
- Pokud chcete spustit neplánované převzetí místo selhání test mějte na paměti následující:

    - Pokud je to možné měli vypnete primární počítačích před spuštěním neplánované převzetí. Zajistíte tak, že nemáte do něj zdrojovou a otevřené počítače se systémem ve stejnou dobu.
    - Když spustíte neplánované převzetí zastaví replikace dat z primární počítačů tak všechny delta data nebude dojít ke ztrátě určitého po zahájení neplánované převzetí. Kromě toho neplánované převzetí plán obnovení ho bude spustit až do dokončení, i když dojde k chybě.




### <a name="run-a-test-failover"></a>Spusťte test selhání

1. Převzetí jednoho OM v **dialogovém okně Nastavení** > **Replikovat položky**, klikněte OM > **+ převzetí testu**.

    ![Převzetí test](./media/site-recovery-vmm-to-vmm/test-failover.png)

2. Překlopení obnovení plán, v **dialogovém okně Nastavení** > jednotného zasílání zpráv**Obnovení plány jednotného zasílání zpráv**, klikněte pravým tlačítkem myši plán > **Převzetí testu**. Vytvoření obnovení plánování, [postupujte podle těchto pokynů](site-recovery-create-recovery-plans.md).
2. Ve **Zkušební překlopení**vyberte **žádné**. Pomocí této možnosti otestovat, aby OM selhání podle očekávání ale replikovanou OM nesmí být připojeni k jakákoli síť.

    ![Převzetí test](./media/site-recovery-vmm-to-vmm/test-failover1.png)

3. Klepnutím na **OK** spusťte záložní. Sledování průběhu kliknutím na OM zobrazíte její vlastnosti nebo na **Test převzetí** úkolu v **dialogovém okně Nastavení** > **úlohy** > **úloh obnovení webu**.
4. Dosáhne úlohy převzetí fáze **testování dokončeno** , postupujte takto:

    -  Zobrazte otevřené OM sekundární VMM cloudu.
    -  Klikněte na **Dokončit test** a dokončete nastavení převzetí test.
    -  Kliknutím na **poznámky** můžete zaznamenat a uložit všechny připomínky přidružený k převzetí testu.

5. Test virtuálního počítače se vytvoří ve stejném hostiteli jako host, na kterém se nachází otevřené virtuálního počítače. Pole bude přidáno do stejné cloudu, ve kterém se nachází otevřené virtuálního počítače.
6. Po úspěšném ověření spuštěné VMs klikněte na **dokončení testu překlopení**. V této fázi budou odstraněny všechny prvky vytvářeny automaticky službami obnovení webu při selhání testu.  

    > [AZURE.NOTE] Pokud test přepojení trvají delší než dva týdny dokončení silou.



### <a name="update-dns-with-the-replica-vm-ip-address"></a>Aktualizace DNS u otevřené OM IP adresa

Po převzetí otevřené OM nemusí mít stejnou IP adresu jako primární virtuálního počítače.

- Virtuálních počítačích se aktualizuje serveru DNS, který používá po spuštění.
- Můžete taky ručně aktualizovat DNS následujícím způsobem:

#### <a name="retrieve-the-ip-address"></a>Načtení IP adresa

Spusťte tento ukázkový skript k načtení IP adresu.

        $vm = Get-SCVirtualMachine -Name <VM_NAME>
        $na = $vm[0].VirtualNetworkAdapters>
        $ip = Get-SCIPAddress -GrantToObjectID $na[0].id
        $ip.address  

#### <a name="update-dns"></a>Aktualizace DNS

Spusťte tento ukázkový skript aktualizovat DNS zadání adresy IP se obnovená pomocí předchozího ukázka skriptu.

        string]$Zone,
        [string]$name,
        [string]$IP
        )
        $Record = Get-DnsServerResourceRecord -ZoneName $zone -Name $name
        $newrecord = $record.clone()
        $newrecord.RecordData[0].IPv4Address  =  $IP
        Set-DnsServerResourceRecord -zonename $zone -OldInputObject $record -NewInputObject $Newrecord

## <a name="next-steps"></a>Další kroky

Po zavedení nastavenou a systém, [Přečtěte si další](site-recovery-failover.md) informace o různých typů převzetí služeb při selhání.
