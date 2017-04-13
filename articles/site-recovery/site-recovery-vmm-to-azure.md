<properties
    pageTitle="Replikovat Hyper-V virtuálních počítačích v VMM mračnech Azure obnovení webu pomocí portálu Azure | Microsoft Azure"
    description="Popisuje, jak nasazení obnovení webu Azure k organizovat replikace, převzetí a obnovení Hyper-V VMs v VMM mračnech k Azure pomocí portálu Azure"
    services="site-recovery"
    documentationCenter=""
    authors="rayne-wiselman"
    manager="jwhit"
    editor="tysonn"/>

<tags
    ms.service="site-recovery"
    ms.workload="backup-recovery"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="09/16/2016"
    ms.author="raynew"/>

# <a name="replicate-hyper-v-virtual-machines-in-vmm-clouds-to-azure-using-azure-site-recovery-with-the-azure-portal--microsoft-azure"></a>Replikovat Hyper-V virtuálních počítačích v VMM mračnech Azure obnovení webu Azure pomocí portálu Azure | Microsoft Azure

> [AZURE.SELECTOR]
- [Azure portálu](site-recovery-vmm-to-azure.md)
- [Klasický Azure](site-recovery-vmm-to-azure-classic.md)
- [Správce prostředků prostředí PowerShell](site-recovery-vmm-to-azure-powershell-resource-manager.md)
- [Klasický prostředí PowerShell](site-recovery-deploy-with-powershell.md)

Vítá vás obnovení Azure webu! Použití tohoto článku, pokud chcete replikovat místní Hyper-V virtuálních počítačích spravovaný v mračnech System Center virtuálního počítače Manager (VMM) na Azure pomocí obnovení webu Azure v Azure portálu.

> [AZURE.NOTE]Azure obsahuje dva různé [modelů nasazení](../resource-manager-deployment-model
> ) pro vytváření grafů a práci s prostředky: Správce prostředků Azure a klasické. Azure má i dvou portály – Azure klasické portál, který podporuje modelu klasické nasazení a portálu Azure s podporou pro oba modely nasazení.


Azure obnovení webu Azure portálu obsahuje několik nových funkcí:

- Na portálu Azure služby Azure zálohování a obnovení webu Azure jsou sloučena do jednoho trezoru služby Recovery tak, aby se nastavení a správa nepřerušený a obnovení (BCDR) z jednoho místa. Jednotné řídicího panelu umožňuje sledovat a spravovat operace různých místních webů a Azure veřejné cloudu.
- Uživatelé, kteří mají Azure předplatná zřízení v aplikaci cloudové řešení poskytovatele (CSP) můžete spravovat teď operace obnovení webu Azure portálu.
- Obnovení webu Azure portálu replikovat počítačích k účtům úložiště správce prostředků Azure. Obnovení webu na selhání vytvoří na základě správce prostředků VMs v Azure.
- Obnovení webu i nadále podporuje replikace k účtům klasické úložiště. Obnovení webu na selhání vytvoří VMs pomocí klasického modelu.


Po přečtení v tomto článku, pokládat dole v komentářích Disqus komentář. Zeptejte se technické ve [Fóru služby Azure obnovení](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

## <a name="overview"></a>Základní informace

Organizace potřebovat strategii BCDR, která určuje, jak zůstat spuštěn a k dispozici během plánované a neplánované prostoje aplikace, pracovního vytížení a dat a běžných pracovních podmínek obnovit co nejdříve. Strategie BCDR by měly zachovat obchodní data bezpečných a obnovitelné a zajistit nepřetržitě dostupnost pracovního vytížení případě havárie.

Obnovení webu je služba Azure podíl strategie BCDR orchestrating replikace místní fyzických serverů a virtuálních počítačích s cloudem (Azure), nebo vedlejší datacentra. Pokud dojde k výpadků ve svém hlavním pracovišti, můžete převzít sekundární umístění, aby aplikace a úloh k dispozici. Můžete být zpět na svém hlavním pracovišti při vrátí na normálně. Další informace najdete v [Co je obnovení webu Azure?](site-recovery-overview.md)

Tento článek obsahuje všechny informace, které je potřeba replikovat místní Hyper-V VMs v VMM mračnech na Azure. Obsahuje přehled architektury plánování informace a nasazení kroků pro konfiguraci Azure místní servery, nastavení replikace a plánování kapacity. Poté, co jste nastavili infrastruktury můžete povolit replikace v počítačích, které chcete zamknout a zkontrolujte, že tento převzetí funguje.


## <a name="business-advantages"></a>Výhody Business

- Obnovení webu poskytuje mimo ochranu firmy pracovního vytížení a spuštěné v Hyper-V VMs.
- Portál služeb obnovení obsahuje jedno místo pro nastavení a správa a sledování replikace, převzetí a obnovení.
- Můžete snadno spustit převzetí služeb při selhání z místního infrastruktury Azure a navrácení (obnovení) z Azure Hyper-V hostitelské servery v místní síti.
- Obnovení plány můžete nakonfigurovat s více počítačů tak, aby společně selhání pracovního vytížení vrstvené aplikace.


## <a name="scenario-architecture"></a>Scénář architektura


Toto jsou součástí scénáře:

- **VMM server**: server místní VMM s jeden nebo více mračnech.
- **Host (hostitel) Hyper-V nebo obrázku**: Hyper-V hostitelské servery nebo seskupení spravovaný v VMM mračnech.
- **Poskytovatel obnovení webu Azure a obnovení služby agent**: během nasazení nainstalujete poskytovatel obnovení webu Azure VMM server a agenta Microsoft Azure obnovení služby na serverech hostitele Hyper-V. Zprostředkovatele na serveru VMM informuje uživatele o s obnovení webu přes HTTPS 443 replikovat průběhu. Agent na hostitelském serveru Hyper-V zreplikuje dat k základnímu úložišti Azure přes HTTPS 443 ve výchozím nastavení.
- **Azure**: potřebujete předplatné Azure, účet Azure úložiště, který chcete uložit replikovat data a Azure virtuální sítě tak, aby Azure VMs připojení k síti po překlopení.

![Topologie E2A](./media/site-recovery-vmm-to-azure/architecture.png)


## <a name="azure-prerequisites"></a>Požadavky na Azure

Tady je, co je potřeba v Azure nasazení tento scénář.

**Předpoklady** | **Podrobnosti**
--- | ---
**Účet Azure**| Je třeba účet [Microsoft Azure](http://azure.microsoft.com/) . Můžete začít s [bezplatnou zkušební verzi](https://azure.microsoft.com/pricing/free-trial/). [Další informace](https://azure.microsoft.com/pricing/details/site-recovery/) o obnovení webu ceny.
**Azure úložiště** | Potřebujete účet standardní Azure úložiště uložit replikovanou data. Můžete použít LRS nebo GRS účtu úložiště. Doporučujeme, abyste GRS tak, aby data pružné místní výpadek, zda oblasti primární možné obnovit. [Další informace](../storage/storage-redundancy.md). Účet musí být ve stejné oblasti jako služby Recovery trezoru.<br/><br/>Úložiště Premium nepodporuje.<br/><br/> Replikovanou data se ukládají v Azure úložiště a Azure VMs se vytvoří, když dojde k selhání. <br/><br/> [Přečtěte si informace o](../storage/storage-introduction.md) Azure úložiště.
**Azure sítě** | Potřebujete Azure virtuální sítě, který Azure VMs připojení dojde k selhání. Síť musí být ve stejné oblasti jako služby Recovery trezoru.

## <a name="on-premises-prerequisites"></a>Zjistit předpoklady pro místní

Tady je budete potřebovat místní

**Předpoklady** | **Podrobnosti**
--- | ---
**VMM**| Nejméně jedna VMM serverech musí běžet na System Center 2012 R2. Každý server VMM by měla být jeden nebo více mračnech nakonfigurované. Shluk by měl obsahovat:<br/><br/> Nejméně jedna VMM skupiny hostitelů.<br/><br/> Jeden nebo několik Hyper-V hostitelské servery a clusterů v každé skupiny Host (hostitel).<br/><br/>[Další informace](http://www.server-log.com/blog/2011/8/26/vmm-2012-and-the-clouds.html) o nastavení VMM mračnech.
**Hyper-V** | Host (hostitel) pro Hyper-V serverů musí běžet aspoň **Windows serveru 2012 R2** s rolí Hyper-V nebo **Microsoft Hyper-V serveru 2012 R2** a nainstalovali nejnovější aktualizace.<br/><br/> Server pro Hyper-V smí obsahovat jeden nebo více VMs.<br/><br/> Hyper-V serveru nebo obrázku, který obsahuje VMs chcete replikovat musí spravovaný v cloudu VMM.<br/><br/>Servery Hyper-V by měl být připojeni k Internetu, přímo nebo prostřednictvím proxy serveru.<br/><br/>Servery Hyper-V by měla být opravy uvedených v článku [2961977](https://support.microsoft.com/kb/2961977) nainstalovaný.<br/><br/>Pro data replikace Azure Hyper-V hostitelské servery vyžaduje přístup k Internetu.
**Zprostředkovatel a agent** | Během obnovení webu Azure nasazení nainstalujete poskytovatel obnovení webu Azure na serveru VMM a agenta služby Recovery na Hyper-V hosts. Zprostředkovatel a agent potřeba se připojit k Azure přes internet přímo nebo prostřednictvím proxy serveru. Všimněte si, že není podporována proxy využívající protokol HTTPS. Proxy serveru na serveru VMM a Hyper-V hostiteli musí umožnit přístup k: <br/><br/> *. hypervrecoverymanager.windowsazure.com <br/> <br/> *. accesscontrol.windows.net <br/><br/> *. backup.windowsazure.com <br/> <br/> *. blob.core.windows.net <br/><br/> *. store.core.windows.net<br/><br/>Pokud máte IP adresu brány firewall pravidla serveru VMM, zkontrolujte, jestli tato pravidla sdělení Azure. Budete muset povolit [Rozsahy IP Azure Datacentra](https://www.microsoft.com/download/confirmation.aspx?id=41653) a port HTTPS (443).<br/><br/>Počkejte, až rozsahy IP adres pro Azure oblast předplatného a západní USA.<br/><br/>Navíc. proxy server na serveru VMM potřebuje přístup k https://www.msftncsi.com/ncsi.txt


## <a name="protected-machine-prerequisites"></a>Požadavky na zamknutém počítače


**Předpoklady** | **Podrobnosti**
--- | ---
**Chráněný VMs** | Než převzetí virtuálního počítače, ujistěte se, název, který je přiřazen k OM Azure splňuje [požadavky na Azure](site-recovery-best-practices.md#azure-virtual-machine-requirements). Název můžete upravit poté, co jste povolili replikace OM. <br/><br/> Jednotlivé kapacita disku chráněné počítačích nesmí být více než 1023 GB. Virtuálního počítače můžete mít až 16 disků (tedy 16 TB).<br/><br/> Sdílené hosta disku, které nejsou podporovány clusterů.<br/><br/> Jednotné Extensible Firmware rozhraní (UEFI) / spouštěcí Extensible Interface(EFI) firmwaru není podporovaná.<br/><br/> Pokud má zdroj OM týmové NIC se převede na jeden NIC po přepnutí Azure.<br/><br/>Ochrana VMs systémem Linux pomocí statické IP adresy není podporovaná.

## <a name="prepare-for-deployment"></a>Připravte pro nasazení

Příprava nasazení je potřeba:

1. [Nastavení služby Azure sítě](#set-up-an-azure-network) ve kterém Azure VMs budou umístěné po překlopení.
2. [Nastavit účet Azure úložiště](#set-up-an-azure-storage-account) pro replikovanou data.
4. [Příprava serveru VMM](#prepare-the-vmm-server) nasazení obnovení webu.
5. [Příprava na mapování sítě](#prepare-for-network-mapping). Nastavení sítě tak, že můžete konfigurovat mapování sítě během obnovení webu nasazení.

### <a name="set-up-an-azure-network"></a>Nastavte si síť Azure

Potřebujete Azure sítě tak, aby Azure VMs vytvořených po převzetí připojí k němu.

- Síť by měl být v oblasti stejný jako ten, ve kterém můžete nasadit služby Recovery trezoru.
- V závislosti na modelu zdroje, který chcete použít pro selhání Azure VMs nastavíte Azure sítě [Správce prostředků](../virtual-network/virtual-networks-create-vnet-arm-pportal.md) nebo [klasického režimu](../virtual-network/virtual-networks-create-vnet-classic-pportal.md).
- Doporučujeme, aby že se nastavení sítě, než začnete. Pokud nechcete, musíte udělat během obnovení webu nasazení.

> [AZURE.NOTE] [Migrace sítí](../resource-group-move-resources.md) přes skupiny zdrojů ve stejném předplatném nebo přes předplatná není podporována pro sítě použít pro nasazení obnovení webu.


### <a name="set-up-an-azure-storage-account"></a>Nastavit účet Azure úložiště

- Musíte mít účet standardní Azure úložiště pro uložení dat replikovat na Azure. Účet musí být ve stejné oblasti jako služby Recovery trezoru.
- V závislosti na modelu zdroje, který chcete použít pro selhání Azure VMs nastavení účtu v [Správce prostředků](../storage/storage-create-storage-account.md) nebo [klasického režimu](../storage/storage-create-storage-account-classic-portal.md).
- Doporučujeme, než začnete nastavení účtu. Pokud nechcete, musíte udělat během obnovení webu nasazení.

> [AZURE.NOTE] [Migrace z úložiště účtů](../resource-group-move-resources.md) přes skupiny zdrojů ve stejném předplatném nebo přes předplatná není podporována u účtů úložiště použít pro nasazení obnovení webu.

### <a name="prepare-the-vmm-server"></a>Příprava VMM serveru

- Ujistěte se, že VMM server splňuje [požadavky](#on-premises-prerequisites).
- Během obnovení webu nasazení můžete určit, že všechny shluky na serveru VMM by měl být k dispozici v Azure portálu. Pokud chcete jenom určité mračnech zobrazit v portálu, můžete povolit nastavení v cloudu v konzole pro správu VMM.


### <a name="prepare-for-network-mapping"></a>Příprava na mapování sítě

Budete muset nastavit mapování sítě během obnovení webu nasazení. Mapování sítě mapy mezi zdroj VMM OM sítě a cílového webu Azure sítí povolit takto:

- Stroje, které převzetí ve stejné síti můžete připojit k sobě, i když nejste selhání stejným způsobem, nebo stejným plánem obnovení.
- Pokud brána sítě nastavenou na cílovém Azure sítě, Azure virtuálních počítačích můžete připojit k místní virtuálních počítačích.
- Nastavení sítě mapování tady je co je potřeba připravit:

    - Ujistěte se, že jsou VMs na hostitelském serveru Hyper-V zdroj připojení k síti VMM OM. Síť by měl být propojené s logická síť, která je přidružená k cloudu.
    - Azure sítě jako popsané [výše](#set-up-an-azure-network)

- [Další informace](site-recovery-network-mapping.md) o tom, jak funguje mapování sítě.


## <a name="create-a-recovery-services-vault"></a>Vytvoření trezoru obnovení služby

1. Přihlaste se k [portálu Azure](https://portal.azure.com).
2. Klikněte na **Nový** > **správy** > **obnovení služby**. Můžete taky můžete kliknutím na **Procházet** > **Služby Recovery** trezorů > **Přidat**.

    ![Nové trezoru](./media/site-recovery-vmm-to-azure/new-vault3.png)

3. Do pole **název**zadejte popisný název k identifikaci trezoru. Pokud máte víc předplatných, vyberte jednu z nich.
4. [Vytvoření skupiny zdrojů](../resource-group-template-deploy-portal.md), nebo vyberte stávající. Zadejte Azure oblast. K této oblasti bude replikovat počítačích. Chcete-li zkontrolovat podporovaných oblastí naleznete v tématu zeměpisné dostupnost podrobně [Azure webu obnovení ceny](https://azure.microsoft.com/pricing/details/site-recovery/)
4. Pokud chcete rychle zobrazit trezoru na řídicím panelu, klikněte na **Připnout na řídicí panel** > **vytvořit trezoru**.

    ![Nové trezoru](./media/site-recovery-vmm-to-azure/new-vault-settings.png)

Nové trezoru se zobrazí na **řídicím panelu** > **všechny zdroje**a na hlavní **služby Recovery trezorů** zásuvné.

## <a name="getting-started"></a>Začínáme

Obnovení webu poskytuje začínáte pracovat, který vám pomůže možnosti nasazení co nejdříve. Začínáme zkontroluje požadavky a provede vás obnovení webu nasazení kroky ve správném pořadí.

Při zahájení vyberte typ strojů chcete replikovat a místo, kam chcete replikovat do. Nastavte si místní servery, Azure úložiště účty a sítě. Vytvořit zásady replikace a plánovat kapacity. Když jste nastavili infrastrukturu povolíte replikace VMs. Spuštění převzetí služeb při selhání pro konkrétní počítače nebo vytvořte plány obnovy překlopení více počítačů.

Začněte Začínáme – zvolte, jak chcete nasadit obnovení webu. Začínáme toku změny mírně v závislosti na vašim požadavkům replikace.



## <a name="step-1-choose-your-protection-goals"></a>Krok 1: Volba cíle ochrany

Vyberte, co chcete replikovat a místo, kam chcete replikovat do.

1. V zásuvné **služby Recovery trezorů** vyberte trezoru a klikněte na **Nastavení**.
2. Při **Zahájení** klikněte na tlačítko **Obnovení webu** > **Krok 1: Příprava infrastruktury** > **cíl zámek**.

    ![Volba cíle](./media/site-recovery-vmm-to-azure/choose-goals.png)

3. V **ochranu hledání** vyberte **Azure na**a vyberte **Ano, pomocí Hyper-V**. Vyberte možnost **Ano** potvrďte, že používáte VMM ke správě Hyper-V hosts a využití webu. Klikněte na **OK**.

    ![Volba cíle](./media/site-recovery-vmm-to-azure/choose-goals2.png)



## <a name="step-2-set-up-the-source-environment"></a>Krok 2: Nastavení zdrojového prostředí

Instalace zprostředkovatele obnovení webů Azure na serveru VMM a registrace serveru v trezoru. Agent služby Azure Recovery nainstalujte Hyper-V hosts.

1. Klikněte na **Krok 2: Příprava infrastruktury** > **zdroje**.

    ![Nastavení zdroje](./media/site-recovery-vmm-to-azure/set-source1.png)

2. **Příprava** zdroje klikněte na **+ VMM** přidání VMM serveru.

    ![Nastavení zdroje](./media/site-recovery-vmm-to-azure/set-source2.png)

3. V zobrazené **Přidat Server** zásuvné políčko **System Center VMM serveru** **Typ serveru** a VMM server splňuje [požadavky a požadavky na adresu URL](#on-premises-prerequisites).
4. Poskytovatel obnovení webu Azure instalace budou moct soubor stáhněte.
5. Stáhněte si registračního klíče. Musíte to při spuštění instalačního programu. Klíč platí pět dní po jeho vytvoření.

    ![Nastavení zdroje](./media/site-recovery-vmm-to-azure/set-source3.png)

6. Obnovení poskytovatel webu Azure nainstalujte na VMM server.


### <a name="set-up-the-azure-site-recovery-provider"></a>Nastavení poskytovatele obnovení webu Azure

1.  Zprostředkovatel spusťte instalační soubor.
2. Ve **Službě Microsoft Update** můžete zvolit v aktualizace tak, aby se mají instalovat aktualizace poskytovatele v souladu se zásadami Microsoft Update.
3. Při **instalaci**přijměte nebo změnit výchozí umístění instalace zprostředkovatele a klikněte na tlačítko **nainstalovat**.

    ![Umístění instalace](./media/site-recovery-vmm-to-azure/provider2.png)

4. Po dokončení instalace klikněte na **zaregistrovat** si zaregistrovat serveru VMM v trezoru.
5. Na stránce **Nastavení trezoru** klikněte na **Procházet** a vyberte soubor s klíčem trezoru. Zadejte Azure webu obnovení předplatného a název trezoru.

    ![Registrace serveru](./media/site-recovery-vmm-to-azure/provider10.PNG)

6. **Připojení k Internetu**určete, jak bude poskytovatele na serveru VMM připojit k obnovení webu přes internet.

    - Pokud chcete poskytovatele, který má přímé připojení vyberte **přímé připojení k obnovení webu Azure bez na proxy server**.
    - Pokud existující proxy server vyžaduje ověření nebo chcete použít vlastní proxy vyberte **připojit k obnovení webu Azure pomocí proxy server**.
    - Pokud používáte vlastní proxy server, zadejte adresu, portů a přihlašovací údaje.
    - Pokud používáte proxy server, kterou jste měli už povolili adresy URL podle [požadavky](#on-premises-prerequisites).
    - Pokud používáte vlastní proxy VMM RunAs účtu (DRAProxyAccount) se vytvoří automaticky pomocí přihlašovacích údajů zadané proxy. Konfigurace proxy server tak, aby tento účet může ověřovat úspěšně. Nastavení účtu VMM RunAs můžete měnit v konzole VMM. Ve skupinovém rámečku **Nastavení**rozbalte **zabezpečení** > **Účty spustit**a potom změnit heslo pro DRAProxyAccount. Je potřeba restartovat službu VMM tak, aby toto nastavení se projeví.

    ![Internet](./media/site-recovery-vmm-to-azure/provider13.PNG)

7. Přijetí nebo změnit umístění certifikát SSL, automaticky vygenerovaný pro šifrování. Pokud povolíte šifrování dat pro obláčkem chráněny Azure na portálu obnovení webu Azure se používá certifikát. Zabezpečit certifikát. Když spustíte přepojení Azure, musíte ho dešifrovat, pokud je povoleno šifrování.


8. V poli **název serveru**zadejte popisný název k identifikaci VMM serveru v trezoru. V konfiguraci clusteru zadejte název VMM clusteru role.
9. Pokud chcete synchronizovat metadat pro všechny shluky na serveru VMM s trezoru povolte **Sync cloudu metadata** . Tato akce jenom s jednou na všech serverech musí. Pokud nechcete synchronizovat všechny shluky, můžete nechat toto nastavení zaškrtnuté a synchronizovat každý cloudu jednotlivě ve vlastnostech cloudu v konzole VMM. Klikněte na tlačítko dokončete proces **Registrace** .

    ![Registrace serveru](./media/site-recovery-vmm-to-azure/provider16.PNG)

10. Spustí se registrace. Po dokončení zápisu serveru se zobrazí na stránce **Nastavení** > zásuvné**servery** v trezoru.


#### <a name="command-line-installation-for-the-azure-site-recovery-provider"></a>Přepínač příkazového řádku instalace zprostředkovatele obnovení webu Azure

Obnovení Provider webu Azure můžete nainstalovat z příkazového řádku. Tato metoda slouží k instalaci zprostředkovatele na Server Core pro Windows Server 2012 R2.

1. Stáhněte instalační soubor a registrace klíč poskytovatele do složky. Například C:\ASR.
2. Na příkazovém řádku, spusťte tyto příkazy extrahovat instalační program poskytovatele:

            C:\Windows\System32> CD C:\ASR
            C:\ASR> AzureSiteRecoveryProvider.exe /x:. /q
3. Spusťte tento příkaz instalace součásti:

            C:\ASR> setupdr.exe /i

4. Spusťte tyto příkazy k registraci serveru v trezoru:

        CD C:\Program Files\Microsoft System Center 2012 R2\Virtual Machine Manager\bin
        C:\Program Files\Microsoft System Center 2012 R2\Virtual Machine Manager\bin\> DRConfigurator.exe /r  /Friendlyname <friendly name of the server> /Credentials <path of the credentials file> /EncryptionEnabled <full file name to save the encryption certificate>       

Kde je:

- **/Credentials**: povinný parametr, která určuje, kde je umístěný klíčové soubor registrace.  
- **/FriendlyName**: povinný parametr název serveru hostitele Hyper-V, která se zobrazí v portálu obnovení webu Azure.
- - **/EncryptionEnabled**: volitelný parametr, když jste replikace Hyper-V VMs v VMM mračnech na Azure. Určete, jestli chcete šifrovat virtuálních počítačích v Azure (u ostatních šifrování). Ujistěte se, že název souboru s příponou **.pfx** . Šifrování je ve výchozím nastavení vypnuto.
- **/proxyAddress**: volitelný parametr, který určuje adresy proxy serveru.
- **/proxyport**: volitelný parametr, který určuje port proxy serveru.
- **/proxyUsername**: volitelný parametr, který určuje proxy uživatelské jméno (pokud proxy server vyžaduje ověření).
- **/proxyPassword**: volitelný parametr, který určuje heslo pro ověřování proxy serveru (pokud proxy server vyžaduje ověření).


### <a name="install-the-azure-recovery-services-agent-on-hyper-v-hosts"></a>Instalace služby Azure Recovery agent na hosts Hyper-V

1. Po jste nastavili zprostředkovatele, je třeba stáhnout instalační soubor agenta služby Azure Recovery. Spusťte instalační program na všech serverech Hyper-V v cloudu VMM.

    ![Weby pro Hyper-V](./media/site-recovery-vmm-to-azure/hyperv-agent1.png)

2. Na stránce **Zkontrolujte požadavky** na tlačítko **Další**. Chybějící požadavky automaticky nainstaluje.

    ![Zjistit předpoklady pro obnovení služby Agent](./media/site-recovery-vmm-to-azure/hyperv-agent2.png)

3. Na stránce **Nastavení instalace** přijměte nebo změňte umístění instalace a umístění mezipaměti. Konfigurace mezipaměti na jednotku, která obsahuje alespoň na úrovni 5 GB úložiště, ale doporučujeme jednotku mezipaměti s minimálně 600 GB volného místa. Klikněte na **nainstalovat**.
4. Po dokončení instalace klikněte na **Zavřít** na Dokončit.

    ![Registrace MARS Agent](./media/site-recovery-vmm-to-azure/hyperv-agent3.png)

#### <a name="command-line-installation-for-azure-site-recovery-services-agent"></a>Přepínač příkazového řádku instalace služby Azure webu Recovery agenta

Agent služby Microsoft Azure obnovení si můžete nainstalovat z příkazového řádku pomocí následujícího příkazu:

     marsagentinstaller.exe /q /nu

#### <a name="set-up-internet-proxy-access-to-site-recovery-from-hyper-v-hosts"></a>Nastavit přístup k Internetu proxy k obnovení webu z hosts Hyper-V

Agent obnovení služeb spuštěných pro Hyper-V hostiteli potřebuje přístup k Internetu k Azure OM replikace. Pokud máte přístup k Internetu přes na proxy server, nastavte ho následujícím způsobem:

1. Otevřete modulu snap-in konzoly MMC Zálohování Microsoft Azure na hostiteli Hyper-V. Ve výchozím nastavení je k dispozici na stolním počítači nebo v C:\Program Files\Microsoft Azure obnovení služby Agent\bin\wabadmin zástupce na ploše pro aplikaci Microsoft Azure zálohování.
2. V modulu snap-in klikněte na **Změnit vlastnosti**.
3. Na kartě **Konfigurací Proxy** zadejte proxy informace o serveru.

    ![Registrace MARS Agent](./media/site-recovery-vmm-to-azure/mars-proxy.png)

4. Zajistit, aby agent přístup adresy URL podle [požadavky](#on-premises-prerequisites).


## <a name="step-3-set-up-the-target-environment"></a>Krok 3: Nastavení cílové prostředí

Zadejte účet Azure úložiště, který chcete použít pro replikace a Azure sítě, ke kterému Azure VMs připojí po převzetí.

1.  Klikněte na **připravit infrastruktury** > **cílové** a vyberte Azure předplatné, které chcete použít.
2.  Určení nasazení modelu, který chcete použít pro VMs po překlopení.
3.  Obnovení webu zkontroluje, jestli máte jedno nebo více účtů kompatibilní Azure úložiště a sítě.

    ![Úložiště](./media/site-recovery-vmm-to-azure/compatible-storage.png)

4.  Pokud jste ještě nevytvořili účet úložiště a chcete ji vytvořit pomocí Správce zdrojů, klikněte na **+ účet úložiště** udělat tento vložený.  Na zásuvné **vytvořit úložiště účtu** zadejte název účtu, typ, předplatné a umístění. Účet by měl být ve stejném umístění jako služby Recovery trezoru.

    ![Úložiště](./media/site-recovery-vmm-to-azure/gs-createstorage.png)

    Všimněte si, že:

    - Pokud chcete vytvořit účet úložiště pomocí klasického modelu, můžete to udělat v portálu Azure. [Víc se uč](../storage/storage-create-storage-account-classic-portal.md)
    - Pokud používáte účet premium úložiště pro replikovanou data, musíte nastavit účet další standardní úložiště uložit protokoly replikace, které zachytit probíhající změny místních dat.

4.  Pokud jste ještě nevytvořili Azure sítě a chcete ji vytvořit pomocí Správce zdrojů klikněte na **+ sítě** se tento vložený. Na zásuvné **vytvořit virtuální síť** zadejte název sítě, rozsah adres, podsítě podrobnosti, předplatné a umístění. Síť by měl být ve stejném umístění jako služby Recovery trezoru.

    ![Sítě](./media/site-recovery-vmm-to-azure/gs-createnetwork.png)

    Pokud chcete vytvořit síť pomocí klasického modelu se to udělat v portálu Azure. [Další informace](../virtual-network/virtual-networks-create-vnet-classic-pportal.md).

### <a name="configure-network-mapping"></a>Konfigurace mapování sítě

- [Čtení](#prepare-for-network-mapping) rychlý přehled toho, jaké mapování sítě nepodporuje. [Přečtěte si toto](site-recovery-network-mapping.md) podrobnější vysvětlení.
- Ověřte, že virtuálních počítačích na serveru VMM jsou připojené k síti OM a jestli jste vytvořili aspoň jeden Azure virtuální sítě. Víc sítí OM možné namapovat jedné sítě Azure.

Konfigurace mapování následujícím způsobem:

1. V **dialogovém okně Nastavení** > **Webu obnovení infrastruktury** > **sítě mapování** > **Mapování sítě**, klepněte na ikonu **+ mapování sítě** .

    ![Mapování sítě](./media/site-recovery-vmm-to-azure/network-mapping1.png)

2. Na **Přidat mapování sítě** vyberte zdroj VMM serveru a **Azure** jako cíl.
3. Po dokončení převzetí ověřte předplatné a nasazení modelu.
4. **Zdrojová síť**vyberte síť OM místní zdroje, který chcete propojit ze seznamu přiřazené k serveru VMM.
5. V **cílové síti**vyberte Azure síť, ve které otevřené Azure VMs budou umístěné když jste vytvořili. Klikněte na **OK**.

    ![Mapování sítě](./media/site-recovery-vmm-to-azure/network-mapping2.png)

Tady je, co se stane, když začíná mapování sítě:

- Existující VMs v síti OM zdroj připojení k síti cílové při mapování, nebude zahájen. Při replikace nové VMs připojení k síti OM zdroj připojeni k síťovou Azure.
- Je-li změnit existující mapování sítě otevřené virtuálních počítačích připojení pomocí nové nastavení.
- Pokud cílové síti má více podsítí a jednu z těchto podsítí má stejný název jako podsítě nachází virtuálního počítače zdroje, bude možné virtuální počítač otevřené připojení k této cílové podsítě po překlopení.
- Pokud žádná cílové podsítě s odpovídajícím názvem virtuální počítač připojen k první podsítě v síti.



## <a name="step-4-set-up-replication-settings"></a>Krok 4: Nastavení replikace


1. Pokud chcete vytvořit nové zásady replikace, klikněte na **připravit infrastruktury** > **Nastavení replikace** > **+ vytvořit a přidružit**.

    ![Sítě](./media/site-recovery-vmm-to-azure/gs-replication.png)

2. **Vytvoření a přiřazení zásad**zadejte název zásady.
3. Do pole **Kopírovat četnost**zadejte požadovanou četnost replikovat delta data po počáteční replikace (každých 30 sekund, 5 až 15 minut).
4. V **obnovení přejděte uchovávání informací**zadejte do několika hodin doby okně uchovávání informací pro vás bude každý obnovení bod. Chráněný počítačích můžete obnovit až kamkoli do okna.
6. V **aplikaci konzistentní počet_plateb snímku**, zadejte frekvence (1 až 12 hodin) obnovení body obsahující konzistenci aplikací snímky bude vytvořen. Pro Hyper-V použití dva typy snímků – standardní snímek, který obsahuje dílčí snímek celý virtuálního počítače a konzistenci aplikací snímek, který vytvoří v okamžiku snímek dat aplikace do virtuálního počítače. Aplikace konzistentní snímky umožňuje zajistit, aby aplikace do konzistentního stavu po snímku, se považuje hlasitost stín kopie služby (VSS). Všimněte si, že pokud povolíte konzistenci aplikací snímky, bude to mít vliv výkonu aplikacím na zdroj virtuálních počítačích. Zajistěte, aby byl hodnotu, kterou nastavíte menší než počet bodů další obnovení, které jste nakonfigurovali.
3. V **Počáteční čas zahájení replikace**určete, kdy spustíte počáteční replikace. Replikace bude přes šířky pásma Internetu tak můžete chtít naplánovat mimo zaneprázdněná hodin.
5. **Šifrování dat uložených v Azure**určete, zda chcete šifrovat na ostatní data v Azure úložiště. Klikněte na **OK**.

    ![Zásady replikace](./media/site-recovery-vmm-to-azure/gs-replication2.png)

6. Při vytváření nové zásady máte automaticky přidružený k VMM cloudu. Klikněte na **OK**. Můžete přiřadit další mračnech VMM (a VMs v nich) tuto zásadu replikace v **dialogovém okně Nastavení** > **replikace** > název zásady > **Přidružení VMM cloudu**.

    ![Zásady replikace](./media/site-recovery-vmm-to-azure/policy-associate.png)

## <a name="step-5-capacity-planning"></a>Krok 5: Plánování kapacity

Teď, když máte v basic infrastruktury nastavení se může přemýšlet o plánování kapacity a zjistit, jestli budete potřebovat další zdroje informací.

Obnovení webu poskytuje kapacita plánování vám přidělit správné zdroje na prostředí zdroje, součásti obnovení webů, sítě a úložiště. Plánování můžete spustit v rychlém režimu odhady podle průměrný počet VMs, disků a úložišť nebo v podrobné režimu, ve kterém se vstupní hodnoty na úrovni zátěží na projektu. Než začnete musíte:

- Shromážděte informace o prostředí replikace, včetně VMs disků za VMs a úložiště na disku.
- Odhad rychlosti denní změnit (konve), které budete mít k dispozici pro replikovanou data. [Plánování doby pro Hyper-V otevřené](https://www.microsoft.com/download/details.aspx?id=39057) umožňuje vám to udělat.

1.  Klikněte na **Stáhnout** pro stažení nástroje a pak ho spusťte. [Přečtěte si tento článek](site-recovery-capacity-planner.md) , který doprovází nástroj.
2.  Až to budete mít v **jste spustili plánovači kapacita**výběrem **Ano** ?

    ![Plánování kapacity](./media/site-recovery-vmm-to-azure/gs-capacity-planning.png)

### <a name="network-bandwidth-considerations"></a>Aspektech šířky pásma sítě

Nástroj Plánovač kapacita slouží k výpočtu šířku pásma, které potřebujete pro replikace (počáteční replikace a potom delta). K řízení počtu využití šířky pásma pro replikace máte několik možností:

- **Omezení šířky pásma**: přenosy Hyper-V zreplikuje sekundárním web prochází určitému Hyper-V hostiteli. Můžete šířku pásma na hostitelském serveru.
- **Doladit šířky pásma**: může mít vliv šířku pásma pro replikace pomocí několika jednoduchých klíče registru.

#### <a name="throttle-bandwidth"></a>Omezení šířky pásma

1. Otevřete modulu snap-in konzoly MMC Zálohování Microsoft Azure na hostitelském serveru Hyper-V. Ve výchozím nastavení je k dispozici na stolním počítači nebo v C:\Program Files\Microsoft Azure obnovení služby Agent\bin\wabadmin zástupce na ploše pro aplikaci Microsoft Azure zálohování.
2. V modulu snap-in klikněte na **Změnit vlastnosti**.
3. Na kartě **Throttling** zaškrtněte políčko **Povolit omezení pro zálohování využití šířky pásma Internetu**a nastavit omezení pro práci a není funkční hodiny. Platné oblasti mají z 512 kB/s 102 MB / sekundu.

    ![Omezení šířky pásma](./media/site-recovery-vmm-to-azure/throttle2.png)

Taky můžete použít rutinu [Set-OBMachineSetting](https://technet.microsoft.com/library/hh770409.aspx) nastavit omezení. Tady je ukázka:

    $mon = [System.DayOfWeek]::Monday
    $tue = [System.DayOfWeek]::Tuesday
    Set-OBMachineSetting -WorkDay $mon, $tue -StartWorkHour "9:00:00" -EndWorkHour "18:00:00" -WorkHourBandwidth  (512*1024) -NonWorkHourBandwidth (2048*1024)

**Nastavení OBMachineSetting NoThrottle** označuje, že není potřeba žádná omezení.


#### <a name="influence-network-bandwidth"></a>Vliv šířka pásma

Hodnotu registru **UploadThreadsPerVM** řídí počet podprocesy, které se používají pro přenos dat (počáteční nebo delta replikace) disku. Vyšší hodnota zvýší šířka pásma pro replikační. Hodnotu registru **DownloadThreadsPerVM** určuje počet podprocesů používaný pro přenos dat během navrácení.

1. V registru přejděte **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure Backup\Replication**.

    - Změňte hodnotu **UploadThreadsPerVM** (nebo vytvořte klávesu Pokud neexistuje) do ovládacího prvku podprocesů používaných replikace disku.
    - Změňte hodnotu **DownloadThreadsPerVM** (nebo vytvořte klávesu Pokud neexistuje) do ovládacího prvku podprocesů používaných pro přenos navrácení z Azure.
2. Výchozí hodnota je 4. V síti "overprovisioned" by měl být tyto klíče registru změnil z výchozích hodnot. Maximální hodnota je 32. Sledování provozu optimalizovat hodnotu.

## <a name="step-6-enable-replication"></a>Krok 6: Povolit replikace

Teď povolte následujícím způsobem:

1. Klikněte na **Krok 2: replikovat aplikace** > **zdroje**. Poté, co jste povolili replikace poprvé kliknete na **+ replikovat** trezoru povolit replikace pro další počítače.

    ![Povolení replikace](./media/site-recovery-vmm-to-azure/enable-replication1.png)

2. V zásuvné **zdroj** > vyberte VMM serveru a ve které se nacházejí hosts Hyper-V cloudu. Klikněte na **OK**.

    ![Povolení replikace](./media/site-recovery-vmm-to-azure/enable-replication-source.png)

3. V **cílové** vyberte předplatné, příspěvek selhání nasazení modelu a úložiště účet, který používáte pro replikovanou data.

    ![Povolení replikace](./media/site-recovery-vmm-to-azure/enable-replication-target.png)

4. Vyberte úložiště účet, který chcete použít. Pokud chcete používat účet jiné úložiště než těch, kterým jste můžete [vytvořit](#set-up-an-azure-storage-account). Vytvoření úložiště účtu pomocí Správce prostředků modelu klikněte na **vytvořit nový**. Pokud chcete vytvořit účet úložiště pomocí klasického modelu, provedete modul [Azure portálu](../storage/storage-create-storage-account-classic-portal.md). Klikněte na **OK**.
5. Vyberte Azure síťových připojení a podsítě, ke kterému Azure VMs připojí, když jste nespředený nahoru po překlopení. Vyberte **konfigurovat nyní u vybraných počítačů** použít nastavení sítě do všech počítačů, které můžete vybrat ochranu. Vyberte **Konfigurovat později** vyberte Azure sítě jednotlivé počítače. Pokud chcete použít jiné síti od těch, kterým jste můžete [vytvořit](#set-up-an-azure-network). Pokud chcete vytvořit síť pomocí Správce prostředků modelu klikněte na **vytvořit nový**. Pokud chcete vytvořit síť pomocí klasického modelu můžete udělat modul [Azure portálu](../virtual-network/virtual-networks-create-vnet-classic-pportal.md). V případě potřeby vyberte podsítě. Klikněte na **OK**.
6. Ve **virtuálních počítačích** > **Vyberte virtuálních počítačích** klikněte na a vyberte každý počítač chcete replikovat. Můžete zvolit pouze počítačích, pro které lze replikace povoleno. Klikněte na **OK**.

    ![Povolení replikace](./media/site-recovery-vmm-to-azure/enable-replication5.png)

5. V **dialogovém okně Vlastnosti** > **Konfigurovat vlastnosti**vyberte operační systém, pro vybraný VMs a disku s operačním systémem. Klikněte na **OK**. Můžete nastavit další vlastnosti později.

    ![Povolení replikace](./media/site-recovery-vmm-to-azure/enable-replication6.png)


12. V **dialogovém okně nastavení replikace** > **nastavení replikace konfigurovat**, vyberte zásadu replikace chcete použít pro chráněné VMs. Klikněte na **OK**. Můžete změnit zásady replikace v **dialogovém okně Nastavení** > **replikace zásady** > název zásady > **Upravit nastavení**. Změny, které použijete se používají pro stroje, které jsou již replikace a nové počítačích.

    ![Povolení replikace](./media/site-recovery-vmm-to-azure/enable-replication7.png)

Můžete sledovat průběh **Povolit ochranu** úlohy v **dialogovém okně Nastavení** > **úlohy** > **úloh obnovení webu**. Když v počítači spuštěná úloha **Dokončení ochrany** je připraven k převzetí.

### <a name="view-and-manage-vm-properties"></a>Zobrazit a spravovat vlastnosti OM

Doporučujeme ověřit vlastnosti zdrojového počítače. Myslete na to, že název Azure OM by měly odpovídat požadavkům [Azure virtuálního počítače](site-recovery-best-practices.md#azure-virtual-machine-requirements).

1. Klikněte na **Nastavení** > **Chráněné položky** > **Replikovat položky** > a vyberte počítač zobrazíte její podrobnosti.

    ![Povolení replikace](./media/site-recovery-vmm-to-azure/vm-essentials.png)

2. V **dialogovém okně Vlastnosti** můžete zobrazit informace o OM replikace a překlopení.

    ![Povolení replikace](./media/site-recovery-vmm-to-azure/test-failover2.png)

3. Ve **výpočetním a síťové** > **Výpočet vlastnosti** můžete zadat Azure OM název a cílové velikost. Název dodržovat [Azure požadavky](site-recovery-best-practices.md#azure-virtual-machine-requirements) v případě potřeby změňte. Můžete taky zobrazit a upravit informace o cílové síti, podsítě a IP adresy, kterou je přiřazen k OM Azure. Poznámka:

    - Můžete nastavit cílová IP adresa. Pokud neposkytnete adresu, selhalo přes počítač bude používat DHCP. Pokud jste nastavili adresu, která není k dispozici na převzetí, záložní se nezdaří. Stejné cílová IP adresa lze test převzetí Pokud je k dispozici v síti převzetí test na adresu.
    - Počet síťové adaptéry je dáno velikost zadané pro cílové virtuální počítač, následujícím způsobem:

        - Pokud argument číslo síťové adaptéry zdrojového počítače menší nebo rovna hodnotě počet adaptéry povolená velikost v počítači cílového, pak cíl bude mít stejný počet adaptéry jako zdroj.
        - Pokud počet adaptéry virtuálního počítače zdroj překročí povolený Cílová velikost pak Cílová velikost maximální počet se používá.
        - Pokud například zdrojového počítače obsahuje dva síťové adaptéry a cílová velikost počítač podporuje čtyři, cílový počítač bude mít dva adaptéry. Pokud zdrojového počítače obsahuje dva adaptéry ale podporované Cílová velikost podporuje pouze jednu cílový počítač bude mít jenom jeden adaptér.     
        - Pokud OM má více síťové adaptéry budou všechny připojí ke stejné síti.

    ![Povolení replikace](./media/site-recovery-vmm-to-azure/test-failover4.png)

5.  Na **disku** uvidíte operačního systému a disků dat na OM, který bude replikovat.



## <a name="step-7-test-your-deployment"></a>Krok 7: Testování nasazení

Chcete-li otestovat nasazení můžete spustit přepojení test pro jeden počítač virtuální nebo obnovení plán, který obsahuje jeden nebo více virtuálních počítačích.


### <a name="prepare-for-failover"></a>Příprava překlopení

- Spusťte test selhání doporučujeme vytvořit novou Azure síť, který má samostatný z Azure sítě (Toto je výchozí chování v Azure vytvoříte novou síť). [Další informace](site-recovery-failover.md#run-a-test-failover) o tom, jak převzetí služeb při selhání testu.
- Nejlepší možný výkon při můžete převzít Azure získáte nainstalujte agenta Azure na zamknutém počítač. Převede spuštění rychleji a pomáhá slouží k řešení potíží. Nainstalujte agent [Linux](https://github.com/Azure/WALinuxAgent) nebo [Windows](http://go.microsoft.com/fwlink/?LinkID=394789) .
- Plně otestovat nasazení musíte infrastrukturu pro replikovanou počítač tak, aby fungovaly očekávaným. Pokud chcete testovat Active Directory a DNS můžete vytvořit virtuální počítač jako řadiče domény se službou DNS a replikovat to Azure pomocí obnovení webu Azure. Přečtěte si další v [aspektech převzetí test pro služby Active Directory](site-recovery-active-directory.md#considerations-for-test-failover).
- Pokud chcete spustit neplánované převzetí místo selhání test mějte na paměti následující:

    - Pokud je to možné měli vypnete primární počítačích před spuštěním neplánované převzetí. Zajistíte tak, že nemáte do něj zdrojovou a otevřené počítače se systémem ve stejnou dobu.
    - Když spustíte neplánované převzetí zastaví replikace dat z primární počítačů tak všechny delta data nebude dojít ke ztrátě určitého po zahájení neplánované převzetí. Kromě toho neplánované převzetí plán obnovení ho bude spustit až do dokončení, i když dojde k chybě.

### <a name="prepare-to-connect-to-azure-vms-after-failover"></a>Příprava přechodu na připojit k Azure VMs po překlopení

Pokud chcete připojit k VMs Azure pomocí RDP po překlopení, ujistěte se, že uděláte to takhle:

**V místním počítači před převzetí**:

- Pro přístup přes internet povolit RDP zajistit přidaná TCP a UDP pravidla pro **veřejné**a zajistit, aby se v bráně **Firewall systému Windows**RDP -> **povolených aplikací a funkcí** pro všechny profily.
- Pro přístup přes připojení k webu povolit RDP v počítači a zajistit, aby se v bráně **Firewall systému Windows**RDP -> **povolených aplikací a funkcí** pro **doménu** a **privátní** sítě.
- Nainstalujte [Azure OM agent](http://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409) v místním počítači.
- Ujistěte se, že je operační systém SAN zásady nastaveno OnlineAll. [Víc se uč]( https://support.microsoft.com/kb/3031135)
- Vypnutí služby IPSec před spuštěním záložní.

**V Azure OM po převzetí služeb**:

- Přidání veřejné koncový bod pro protokol RDP (port 3389) a zadejte přihlašovací údaje pro přihlášení.
- Ujistěte se, že nemáte žádné zásady domény, kvůli kterým jste připojení k virtuálního počítače pomocí veřejné adresy.
- Pokuste se připojit. Pokud se nemůžete spojit, zkontrolujte, jestli je spuštěný OM. Další tipy pro řešení problémů v tomto [článku](http://social.technet.microsoft.com/wiki/contents/articles/31666.troubleshooting-remote-desktop-connection-after-failover-using-asr.aspx).

Pokud chcete získat přístup Azure OM systémem Linux po převzetí pomocí zabezpečené prostředí klienta (ssh), postupujte takto:

**V místním počítači před převzetí**:

- Ověřte, zda službu zabezpečené prostředí v Azure OM aby se spouštěl automaticky při spuštění systému.
- Zkontrolujte, jestli pravidla brány firewall SSH připojení k němu.

**V Azure OM po převzetí služeb**:

- Pravidla skupiny zabezpečení sítě o nezdařeném uložení přes OM a Azure podsítí, ke kterému je připojen muset povolit SSH port pro příchozí připojení.
- Veřejné koncový bod má vytvořit za účelem povolení příchozí připojení na SSH port (port TCP 22 ve výchozím nastavení).
- Pokud OM k nim získat přístup přes připojení virtuální privátní sítě (VPN sítěmi nebo Express směrování) potom klienta mohou sloužit přímo připojit přes SSH bude v angličtině.


### <a name="run-a-test-failover"></a>Spusťte test selhání

Chcete-li spusťte test převzetí udělejte následující:

1. Převzetí jednoho OM v **dialogovém okně Nastavení** > **Replikovat položky**, klikněte OM > **+ převzetí testu**.
2. Překlopení obnovení plán, v **dialogovém okně Nastavení** > jednotného zasílání zpráv**Obnovení plány jednotného zasílání zpráv**, klikněte pravým tlačítkem myši plán > **Převzetí testu**. Vytvoření obnovení plánování, [postupujte podle těchto pokynů](site-recovery-create-recovery-plans.md).

3. V **Testu převzetí** vyberte Azure sítě, ke které Azure VMs připojit po dojde k selhání.
4. Klepnutím na **OK** spusťte záložní. Sledování průběhu kliknutím na OM zobrazíte její vlastnosti nebo na **Test převzetí** úkolu v **dialogovém okně Nastavení** > **úloh obnovení webu**.
5. Dosáhne záložní fáze **testování dokončeno** , postupujte takto:

    1. Zobrazení virtuálního počítače otevřené Azure portálu. Ověřte úspěšně spustí virtuální počítač.
    2. Pokud jste nastavili přístup virtuálních počítačích z místní síti můžete spustit připojení ke vzdálené ploše virtuálního počítače.
    3. Klikněte na **Dokončit test** ji dokončit.
    4. Kliknutím na **poznámky** můžete zaznamenat a uložit všechny připomínky přidružený k převzetí testu.
    5. Klikněte na **dokončení testu překlopení**. Vyčistěte testovacím prostředí automaticky vypněte a odstraňovat test virtuálního počítače.
    6. V této fázi budou odstraněny všechny prvky nebo VMs vytvářeny automaticky službami obnovení webu při selhání test. Další prvky, které jste vytvořili pro překlopení test se neodstraní.

    > [AZURE.NOTE] Pokud test přepojení trvají delší než dva týdny dokončení silou.

6. Po dokončení záložní je také třeba neuvidíte otevřené Azure počítače se zobrazí v portálu Azure > **virtuálních počítačích**. Nezapomeňte, že OM je vhodné velikosti a připojeného k příslušné sítě a, jestli je spuštěný.
7. Pokud jste [připraveni připojení po převzetí](#prepare-to-connect-to-Azure-VMs-after-failover) by je možné se připojit k OM Azure.


## <a name="monitor-your-deployment"></a>Sledování nasazení

Tady je, jak můžete sledovat nastavení, stavu a stavu obnovení webu nasazení:

1. Klikněte na název trezoru pro přístup k řídicím panelu **Essentials** . V tomto řídicím panelu můžete úloh obnovení webu, stav replikace, plány obnovy, stavu serveru a události.  Můžete přizpůsobit Essentials zobrazíte dlaždic a rozložení, které jsou pro vás, včetně stavu ostatních zálohování a obnovení webu trezorů nejvhodnější.

    ![Základy](./media/site-recovery-vmm-to-azure/essentials.png)

2. V této dlaždici **stavu** můžete sledovat Web servery (VMM nebo konfigurace), ve kterých dochází problém a události vyvolané obnovení webu za posledních 24 hodin.
3. Správa a sledování replikace v **Replikovat položky**jednotného zasílání zpráv **Obnovení plány jednotného zasílání zpráv**, a dlaždice **Úlohy obnovení webů** . Můžete procházení datových krychlí úlohy v **dialogovém okně Nastavení** -> **úlohy** -> **Úlohy obnovení webů**.


## <a name="next-steps"></a>Další kroky

Po zavedení nastavenou a systém, [Přečtěte si další](site-recovery-failover.md) informace o různých typů překlopení.
