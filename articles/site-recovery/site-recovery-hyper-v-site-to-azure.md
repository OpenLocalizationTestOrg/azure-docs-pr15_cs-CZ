<properties
    pageTitle="Replikace Hyper-V virtuálních počítačích (bez VMM) na Azure obnovení webu Azure pomocí portálu Azure | Microsoft Azure"
    description="Popisuje, jak nasazení obnovení webu Azure k organizovat replikace, převzetí a obnovení VMs Hyper-V místní, které nejsou spravuje VMM Azure pomocí portálu Azure"
    services="site-recovery"
    documentationCenter=""
    authors="rayne-wiselman"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="site-recovery"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="storage-backup-recovery"
    ms.date="09/19/2016"
    ms.author="raynew"/>


# <a name="replicate-hyper-v-virtual-machines-without-vmm-to-azure-using-azure-site-recovery-with-the-azure-portal"></a>Replikace Hyper-V virtuálních počítačích (bez VMM) na Azure obnovení webu Azure pomocí portálu Azure

> [AZURE.SELECTOR]
- [Azure portálu](site-recovery-hyper-v-site-to-azure.md)
- [Klasický Azure](site-recovery-hyper-v-site-to-azure-classic.md)
- [Prostředí PowerShell – správce](site-recovery-deploy-with-powershell-resource-manager.md)



Vítá vás obnovení Azure webu! Použití tohoto článku, pokud chcete replikovat místní Hyper-V virtuální stroje, které **nejsou** spravovaný v mračnech System Center virtuálních počítačích Manager (VMM) na Azure. Tento článek popisuje, jak nastavit replikace pomocí obnovení webu Azure Azure portálu.

> [AZURE.NOTE] Azure obsahuje dva různé [modelů nasazení](../resource-manager-deployment-model.md) pro vytváření grafů a práci s prostředky: Správce prostředků Azure a klasické. Azure má i dvou portály – Azure klasické portál, který podporuje modelu klasické nasazení a portálu Azure s podporou pro oba modely nasazení.

> Obnovení Azure webu podporuje obnovení a migrace pro Hyper-V virtuálních počítačích na Azure. Kroky v tomto článku platí stejně jako při konfiguraci replikace Azure havárie obnovení nebo pro migraci VMs Azure

Azure obnovení webu Azure portálu nabízí celou řadu nových funkcí:

- V Azure portál, Azure zálohování a obnovení webu Azure služby zkombinují do jednoho trezoru služby Recovery tak, aby se nastavení a správa nepřerušený a obnovení (BCDR) z jednoho místa. Jednotné řídicího panelu umožňuje sledovat a spravovat operace různých místních webů a Azure veřejné cloudu.
- Uživatelé, kteří mají Azure předplatná zřízení v aplikaci cloudové řešení poskytovatele (CSP) můžete spravovat teď operace obnovení webu Azure portálu.
- Obnovení webu Azure portálu replikovat počítačích k účtům úložiště správce prostředků. Obnovení webu na selhání vytvoří na základě správce prostředků VMs v Azure.
- Obnovení webu i nadále podporuje replikace klasické úložiště účtů a převzetí VMs pomocí klasické nasazení modelu.


Po přečtení tento příspěvek článek dole v oddílu komentáře Disqus svůj názor. Zeptejte se technické ve [Fóru služby Azure obnovení](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).


## <a name="overview"></a>Základní informace


Organizace potřebovat strategii BCDR, která určuje, jak zůstat spuštěn a k dispozici během plánované a neplánované prostoje aplikace, pracovního vytížení a dat a běžných pracovních podmínek obnovit co nejdříve. Strategie BCDR budou bezpečných a obnovitelné zachovat obchodní data a zrušte zaškrtnutí pracovního vytížení stále k dispozici případě havárie.

Obnovení webu je služba Azure podíl strategie BCDR tak, že orchestrating replikace místní fyzických serverů a virtuálních počítačích s cloudem (Azure) nebo vedlejší datacentra. Pokud dojde k výpadků ve svém hlavním pracovišti, může selhat přes sekundární umístění k synchronizaci aplikace a k dispozici pracovního vytížení. Zpět na svém hlavním pracovišti může selhat, když se vrátí normálně. Další informace najdete v [Co je obnovení webu Azure?](site-recovery-overview.md)

Tento článek obsahuje všechny informace, které je potřeba replikovat místní Hyper-V virtuální stroje, které **nejsou** spravovaný v mračnech System Center virtuálních počítačích Manager (VMM) na Azure. Obsahuje přehled architektury plánování informace a nasazení kroků pro konfiguraci místní servery, Azure, zásady replikace a plánování kapacity. Poté, co jste nastavili infrastruktury povolíte replikace v počítačích, které chcete zamknout a provést přepojení test k ověření nastavení. Můžete taky migrovat vaší VMs na Azure první provedením plánované převzetí a provedení migrace.

## <a name="business-advantages"></a>Výhody Business

- Poskytuje mimo převzetí (Azure) pro firmy pracovního vytížení a spuštěné v Hyper-V virtuálních počítačích.
- Poskytuje jediné konzoly služby Recovery jednoduché nastavení a správě replikace, převzetí a obnovení procesů.
- Umožňuje snadno spustit převzetí služeb při selhání z místního infrastruktury Azure a selhání zpětného (obnovení) z Azure k místnímu webu.
- Obnovení plány můžete nakonfigurovat s více počítačů, tak, aby společně selhání pracovního vytížení vrstvené aplikace.

## <a name="scenario-architecture"></a>Scénář architektura

Toto jsou součástí scénáře:

- **Host (hostitel) Hyper-V nebo obrázku**: místní Hyper-V hostitelské servery nebo seskupení. Hyper-V hosts spouštění virtuálních chcete zamknout se shromažďují do logické webů Hyper-V během obnovení webu nasazení.
- **Poskytovatel obnovení webu Azure a obnovení služby agent**: během nasazení instalaci poskytovatel obnovení webu Azure a agenta Microsoft Azure obnovení služby na serverech hostitele Hyper-V. Zprostředkovatel informuje uživatele o s obnovení webu Azure přes HTTPS 443 replikovat průběhu. Agent na hostitelském serveru Hyper-V zreplikuje dat k základnímu úložišti Azure přes HTTPS 443 ve výchozím nastavení.
- **Azure**: potřebujete předplatné Azure, účet Azure úložiště, který chcete uložit replikovat data a Azure virtuální sítě tak, aby Azure VMs připojení k síti po překlopení.

![Architektura webu Hyper-V](./media/site-recovery-hyper-v-site-to-azure/architecture.png)



## <a name="azure-prerequisites"></a>Požadavky na Azure

Tady je, co budete potřebovat v Azure nasazení tento scénář.

**Předpoklady** | **Podrobnosti**
--- | ---
**Účet Azure**| Musíte mít účet [Microsoft Azure](http://azure.microsoft.com/) . Můžete začít s [bezplatnou zkušební verzi](https://azure.microsoft.com/pricing/free-trial/). [Další informace](https://azure.microsoft.com/pricing/details/site-recovery/) o obnovení webu ceny.
**Azure úložiště** | Musíte mít účet standardní úložiště. Můžete použít LRS nebo GRS účtu úložiště. Doporučujeme, abyste GRS tak, aby data pružné místní výpadek, zda oblasti primární možné obnovit. [Další informace](../storage/storage-redundancy.md). Účet musí být ve stejné oblasti jako služby Recovery trezoru.<br/><br/> Úložiště Premium nepodporuje.<br/><br/> Replikovanou data se ukládají v Azure úložiště a Azure VMs se vytvoří, když dojde k selhání.<br/><br/> [Přečtěte si informace o](../storage/storage-introduction.md) Azure úložiště.
**Azure sítě** | Budete potřebovat Azure virtuální sítě, který Azure VMs připojí k selháním. Azure virtuální sítě musí být ve stejné oblasti jako služby Recovery trezoru.

## <a name="on-premises-prerequisites"></a>Zjistit předpoklady pro místní

Tady je co budete potřebovat místní.

**Předpoklady** | **Podrobnosti**
--- | ---
**Hyper-V** | Nejméně jeden místní serverech běží **Windows Server 2012 R2** s nejnovější aktualizace a role Hyper-V povoleno nebo **Microsoft Hyper-V serveru 2012 R2**.<br/><br/>Server pro Hyper-V smí obsahovat jeden nebo více virtuálních počítačích.<br/><br/>Servery Hyper-V by měl být připojeni k Internetu, přímo nebo prostřednictvím proxy serveru.<br/><br/>Servery Hyper-V by měla být opravy podle [KB2961977](https://support.microsoft.com/en-us/kb/2961977 "KB2961977") nainstalovaný.
**Zprostředkovatel a agent** | Během obnovení webu Azure nasazení nainstalujete poskytovatel obnovení webu Azure. Instalace zprostředkovatele také nainstaluje agenta služby Azure obnovení na každý Hyper-V serveru virtuálních počítačích, které chcete zamknout. Všechny servery Hyper-V trezoru obnovení webu by měl mít stejnou verzi poskytovatele a agent.<br/><br/>Zprostředkovatel muset připojit k obnovení webu Azure přes Internet. Přenosy odesílat přímo nebo prostřednictvím proxy serveru. Všimněte si, že není podporováno HTTPS na základě proxy. Proxy serveru musí umožnit přístup k: <br/><br/> *. hypervrecoverymanager.windowsazure.com <br/> <br/> *. accesscontrol.windows.net <br/><br/> *. backup.windowsazure.com <br/> <br/> *. blog.core.windows.net <br/><br/> *Store.Core.Windows.NET <br/><br/> https://www.msftncsi.com/ncsi.txt<br/><br/>Pokud máte IP adresu brány firewall pravidla na serveru, zkontrolujte, jestli tato pravidla sdělení Azure. Musíte povolit [Rozsahy IP Azure Datacentra](https://www.microsoft.com/download/confirmation.aspx?id=41653) a port HTTPS (443).<br/><br/>Počkejte, až rozsahy IP adres pro Azure oblast předplatného a západní USA.

## <a name="protected-machine-prerequisites"></a>Požadavky na zamknutém počítače


**Předpoklady** | **Podrobnosti**
--- | ---
**Chráněný VMs** | Než převzetí virtuálního počítače musíte Ujistěte se, že název, který bude přiřazeno OM Azure vyhovuje [Azure požadavky](site-recovery-best-practices.md#azure-virtual-machine-requirements). Název můžete upravit poté, co jste povolili replikace OM.<br/><br/> Jednotlivé kapacita disku chráněné počítačích nesmí být více než 1023 GB. Virtuálního počítače můžete mít až 16 disků (tedy 16 TB).<br/><br/> Sdílené hosta disku, které nejsou podporovány clusterů.<br/><br/> Pokud má zdroj OM týmové NIC se převede na jeden NIC po přepnutí Azure.<br/><br/>Ochrana VMs systémem Linux pomocí statické IP adresy není podporovaná.

## <a name="prepare-for-deployment"></a>Připravte pro nasazení

Příprava na nasazení budete potřebovat:

1. [Nastavení služby Azure sítě](#set-up-an-azure-network) ve kterém Azure VMs budou umístěné když jste vytvořili po překlopení.
2. [Nastavit účet Azure úložiště](#set-up-an-azure-storage-account) pro replikovanou data.
3. [Příprava Hyper-V hosts](#prepare-the-hyper-v-hosts) zajistit, že k že němu přístup požadované URL.

### <a name="set-up-an-azure-network"></a>Nastavte si síť Azure

Nastavte si síť Azure. Musíte to tak, aby Azure VMs vytvořených po převzetí připojení k síti.

- Síť by měl být v oblasti stejný jako ten, ve kterém se nasadit služby Recovery trezoru.
- V závislosti na modelu zdroje, který chcete použít pro selhání Azure VMs nastavíte Azure sítě [Správce prostředků](../virtual-network/virtual-networks-create-vnet-arm-pportal.md) nebo [klasického režimu](../virtual-network/virtual-networks-create-vnet-classic-pportal.md).
- Doporučujeme, aby že se nastavení sítě, než začnete. Pokud si budete muset udělat během obnovení webu nasazení.

> [AZURE.NOTE] [Migrace sítí](../resource-group-move-resources.md) přes skupiny zdrojů ve stejném předplatném nebo přes předplatná není podporována pro sítě použít pro nasazení obnovení webu.

### <a name="set-up-an-azure-storage-account"></a>Nastavit účet Azure úložiště

- Musíte mít účet standardní Azure úložiště pro uložení dat replikovat na Azure.
- V závislosti na modelu zdroje, který chcete použít pro selhání Azure VMs nastavíte účet v [Správce prostředků](../storage/storage-create-storage-account.md) nebo [klasického režimu](../storage/storage-create-storage-account-classic-portal.md).
- Doporučujeme nastavení účtu úložiště než začnete. Pokud si budete muset udělat během obnovení webu nasazení. Účty musí být ve stejné oblasti jako služby Recovery trezoru.

> [AZURE.NOTE] [Migrace z úložiště účtů](../resource-group-move-resources.md) přes skupiny zdrojů ve stejném předplatném nebo přes předplatná není podporována u účtů úložiště použít pro nasazení obnovení webu.

### <a name="prepare-the-hyper-v-hosts"></a>Příprava hosts Hyper-V

- Ujistěte se, že hosts Hyper-V souladu s [požadavky](#on-premises-prerequisites).

### <a name="create-a-recovery-services-vault"></a>Vytvoření trezoru obnovení služby

1. Přihlaste se k [portálu Azure](https://portal.azure.com).
2. Klikněte na **Nový** > **správy** > **zálohování a obnovení webu (OMS)**. Můžete taky můžete kliknutím na **Procházet** > **Služby Recovery** trezorů > **Přidat**.

    ![Nové trezoru](./media/site-recovery-hyper-v-site-to-azure/new-vault3.png)

3. Do pole **název** zadejte popisný název k identifikaci trezoru. Pokud máte víc předplatných, vyberte jednu z nich.
4. [Vytvoření nové skupiny prostředků](../resource-group-template-deploy-portal.md) nebo vyberte stávající a zadejte Azure oblast. K této oblasti bude replikovat počítačích. Chcete-li zkontrolovat podporovaných oblastí naleznete v tématu zeměpisné dostupnost podrobně [Azure webu obnovení ceny](https://azure.microsoft.com/pricing/details/site-recovery/)
4. Pokud chcete rychle zobrazit trezoru na řídicím panelu klikněte na **Připnout na řídicí panel** a potom klikněte na **vytvořit trezoru**.

    ![Nové trezoru](./media/site-recovery-hyper-v-site-to-azure/new-vault-settings.png)

Nové trezoru se zobrazí na **řídicím panelu** > **všechny zdroje**a na hlavní **služby Recovery trezorů** zásuvné.

## <a name="getting-started"></a>Začínáme

Obnovení webu poskytuje začínáte pracovat, který vám pomůže možnosti nasazení co nejdříve. Začínáme zkontroluje požadavky a provede vás obnovení webu nasazení kroky ve správném pořadí.

Při zahájení vyberte typ strojů chcete replikovat a místo, kam chcete replikovat do. Nastavte si místní servery, Azure úložiště účty a sítě. Vytvořit zásady replikace a plánovat kapacity. Když jste nastavili infrastrukturu povolíte replikace VMs. Spuštění převzetí služeb při selhání pro konkrétní počítače nebo vytvořte plány obnovy překlopení více počítačů.

Začněte Začínáme – zvolte, jak chcete nasadit obnovení webu. Začínáme toku změny mírně v závislosti na vašim požadavkům replikace.



## <a name="step-1-choose-your-protection-goals"></a>Krok 1: Volba cíle ochrany

Vyberte, co chcete replikovat a místo, kam chcete replikovat do.

1. V zásuvné **služby Recovery trezorů** vyberte trezoru a klikněte na **Nastavení**.
2. V **dialogovém okně Nastavení** > **Začínáme** klikněte na **Obnovení webu** > **Krok 1: Příprava infrastruktury** > **cíl zámek**.

    ![Volba cíle](./media/site-recovery-hyper-v-site-to-azure/choose-goals.png)

3. V **ochranu hledání** vyberte **Azure na**a vyberte **Ano, pomocí Hyper-V**. Vyberte možnost **bez** potvrďte, že nepoužíváte VMM. Klikněte na **OK**.

    ![Volba cíle](./media/site-recovery-hyper-v-site-to-azure/choose-goals2.png)


## <a name="step-2-set-up-the-source-environment"></a>Krok 2: Nastavení zdrojového prostředí

Nastavte pro Hyper-V webu, instalace zprostředkovatele obnovení webů Azure a agenta služby Azure Recovery na hosts Hyper-V a zaregistrovat hostitelů v trezoru.


1. Klikněte na **Krok 2: Příprava infrastruktury** > **zdroje**. Přidat nový web Hyper-V jako kontejner pro Hyper-V hosts nebo seskupení, klikněte na **+ Hyper-V webu**.

    ![Nastavení zdroje](./media/site-recovery-hyper-v-site-to-azure/set-source1.png)

2. Na **webu vytvořit pro Hyper-V** zásuvné zadejte název pro daný web. Klikněte na **OK**. Vyberte web, který jste právě vytvořili.

    ![Nastavení zdroje](./media/site-recovery-hyper-v-site-to-azure/set-source2.png)

3. Klikněte na tlačítko **+ Hyper-V Server** serveru přidáte na web.
4. V okně **Přidat Server** > **Typ serveru** zkontrolujte, že se zobrazí **Hyper-V server** . Ujistěte se, že Hyper-V server, ke kterému chcete přidat v souladu s [požadavky](#on-premises-prerequisites) a získat přístup k zadané adresy URL.
4. Poskytovatel obnovení webu Azure instalace budou moct soubor stáhněte. Spuštěním instalace zprostředkovatele a agenta služby Recovery v každém Hyper-V hostiteli tohoto souboru.
5. Stáhněte si registračního klíče. Musíte mít takto při spuštění instalačního programu. Klíč platí 5 dní po jeho vytvoření.

    ![Nastavení zdroje](./media/site-recovery-hyper-v-site-to-azure/set-source3.png)

6. Spusťte instalační soubor u každého hostitele, které jste přidali na web pro Hyper-V zprostředkovatele. Pokud instalujete Hyper-V clusteru, spusťte instalační program v jednotlivých uzlech obrázku. Instalace a registrace jednotlivých uzlech Hyper-V zaručuje, že zachovány virtuálních počítačích chráněné i v případě migrovat mezi uzly.

### <a name="install-the-provider-and-agent"></a>Instalace zprostředkovatele a agent

1. Zprostředkovatel spusťte instalační soubor.
2. Ve **Službě Microsoft Update** můžete zvolit v aktualizace tak, aby se mají instalovat aktualizace poskytovatele v souladu se zásadami Microsoft Update.
3. Při **instalaci** přijměte nebo změnit výchozí umístění instalace zprostředkovatele a klikněte na tlačítko **nainstalovat**.
4. Na stránce **Nastavení trezoru** klikněte na **Procházet** a vyberte stažený soubor klíčové trezoru. Zadejte předplatné obnovení webu Azure, názvu trezoru a Hyper-V webu, ke kterému patří Hyper-V server.

    ![Registrace serveru](./media/site-recovery-hyper-v-site-to-azure/provider3.png)

5. v **Dialogovém okně Nastavení proxy serveru** určete, jak bude zprostředkovateli na serveru být nainstalovány připojovat k obnovení webu Azure přes internet.

- Pokud chcete poskytovatele, který má přímé připojení vyberte **připojení přímo bez proxy**.
- Pokud se chcete připojit k proxy, která je aktuálně nastavena na serveru vyberte **připojit k existující nastavení proxy**.
- Pokud existující proxy server vyžaduje ověření nebo chcete použít vlastní proxy připojení vyberte poskytovatel **připojení s nastavením vlastní proxy serveru**.
- Pokud používáte vlastní proxy, budete muset zadat adresu, portů a přihlašovací údaje
- Pokud používáte proxy vytvářecího zkontrolujte adresy URL popsané v [požadavcích](#on-premises-prerequisites) jsou povoleny přes něj.

    ![Internet](./media/site-recovery-hyper-v-site-to-azure/provider7.PNG)

6. Po dokončení instalace klikněte na **zaregistrovat** k registraci serveru v trezoru.

![Umístění instalace](./media/site-recovery-hyper-v-site-to-azure/provider2.png)

7. po registraci dokončení metadat z Hyper-V serveru načítané obnovení webu Azure a serveru se zobrazí na stránce **Nastavení** > **Webu obnovení infrastruktury** > zásuvné**Hyper-V Hosts** .


### <a name="command-line-installation"></a>Přepínač příkazového řádku instalace

Poskytovatel obnovení webu Azure a agent lze také nainstalovat pomocí následující příkazový řádek. Tato metoda slouží k instalaci zprostředkovatele na serveru Core pro Windows Server 2012 R2.

1. Stáhněte instalační soubor a registrace klíč poskytovatele do složky. Dejme tomu C:\ASR.
2. Z příkazovém řádku spusťte tyto příkazy extrahovat poskytovatele instalační program:

            C:\Windows\System32> CD C:\ASR
            C:\ASR> AzureSiteRecoveryProvider.exe /x:. /q
3. Spusťte tento příkaz instalace součásti:

            C:\ASR> setupdr.exe /i

4. Spusťte tyto příkazy k registraci serveru v trezoru: CD C:\Program Files\Microsoft Azure webu obnovení Provider\
            Obnovení poskytovatel C:\Program Files\Microsoft Azure webu\> DRConfigurator.exe /r /Friendlyname <friendly name of the server> /pověření<path of the credentials file>

<br/>
Kde je:

- **/Credentials** : povinný parametr, který určuje umístění, ve kterém je umístěn soubor klíčové registrace  
- **/FriendlyName** : povinný parametr název serveru hostitele Hyper-V, která se zobrazí v portálu obnovení webu Azure.
- **/proxyAddress** : volitelný parametr, který určuje adresy proxy serveru.
- **/proxyport** : volitelný parametr, který určuje port proxy serveru.
- **/proxyUsername** : volitelný parametr, který určuje Proxy uživatelské jméno (pokud proxy server vyžaduje ověření).
- **/proxyPassword** : volitelný parametr, který určuje heslo pro ověřování proxy serveru (pokud proxy server vyžaduje ověření).


## <a name="step-3-set-up-the-target-environment"></a>Krok 3: Nastavení cílové prostředí

Zadejte účet Azure úložiště, který chcete použít pro replikace a Azure sítě, ke kterému Azure VMs připojí po převzetí.

1.  Klikněte na **připravit infrastruktury** > **cílové** a vyberte Azure předplatné, které chcete použít.
2.  Určení nasazení modelu, který chcete použít pro VMs po překlopení.
3.  Obnovení webu zkontroluje, jestli máte jedno nebo více účtů kompatibilní Azure úložiště a sítě.

    ![Úložiště](./media/site-recovery-hyper-v-site-to-azure/select-target.png)

4.  Pokud jste ještě nevytvořili účet úložiště a chcete ji vytvořit pomocí Správce zdrojů klikněte na **+ úložiště** účet, který chcete provést tento vložený. Na zásuvné **vytvořit úložiště účtu** zadejte název účtu, typ, předplatné a umístění. Účet by měl být ve stejném umístění jako služby Recovery trezoru.

    ![Úložiště](./media/site-recovery-hyper-v-site-to-azure/gs-createstorage.png)

    Pokud chcete vytvořit účet úložiště pomocí klasického modelu můžete udělat modul [Azure portálu](../storage/storage-create-storage-account-classic-portal.md).

5.  Pokud jste ještě nevytvořili Azure sítě a chcete ji vytvořit pomocí Správce zdrojů klikněte na **+ sítě** se tento vložený. Na zásuvné **vytvořit virtuální síť** zadejte název sítě, rozsah adres, podsítě podrobnosti, předplatné a umístění. Síť by měl být ve stejném umístění jako služby Recovery trezoru.

    ![Sítě](./media/site-recovery-hyper-v-site-to-azure/gs-createnetwork.png)

    Pokud chcete vytvořit síť pomocí klasického modelu můžete udělat modul [Azure portálu](../virtual-network/virtual-networks-create-vnet-classic-pportal.md).


## <a name="step-4-set-up-replication-settings"></a>Krok 4: Nastavení replikace

1. Vytvořit nové replikace zásady klikněte na možnost **připravit infrastruktury** > **Nastavení replikace** > **+ vytvořit a přidružit**.

    ![Sítě](./media/site-recovery-hyper-v-site-to-azure/gs-replication.png)

2. **Vytvoření** a přiřazení zásad zadejte název zásady.
3. V **kopírování počet_plateb** určete, jak často chcete replikovat delta data po počáteční replikace (každých 30 sekund, 5 až 15 minut).
4. V **obnovení přejděte uchovávání informací**zadejte do několika hodin doby okně uchovávání informací pro vás bude každý obnovení bod. Chráněný počítačích můžete obnovit až kamkoli do okna.
6. V **aplikaci konzistentní snímek četnost** zadejte frekvence (1 až 12 hodin) obnovení body obsahující konzistenci aplikací snímky bude vytvořen. Pro Hyper-V použití dva typy snímků – standardní snímek, který obsahuje dílčí snímek celý virtuálního počítače a konzistenci aplikací snímek, který vytvoří v okamžiku snímek dat aplikace do virtuálního počítače. Aplikace konzistentní snímky umožňuje zajistit, aby aplikace do konzistentního stavu po snímku, se považuje hlasitost stín kopie služby (VSS). Všimněte si, že pokud povolíte konzistenci aplikací snímky, bude to mít vliv výkonu aplikacím na zdroj virtuálních počítačích. Zajistěte, aby byl hodnotu, kterou nastavíte menší než počet bodů další obnovení, které jste nakonfigurovali.
3. V **Počáteční čas zahájení replikace** určete, kdy spustíte počáteční replikace. Replikace bude přes šířky pásma Internetu tak můžete chtít naplánovat mimo zaneprázdněná hodin. Klikněte na **OK**.

    ![Zásady replikace](./media/site-recovery-hyper-v-site-to-azure/gs-replication2.png)

Při vytváření nové zásady máte automaticky přidružený k webu Hyper-V. Klikněte na **OK**. Můžete přidružit Hyper-V webu (a VMs v něm) více zásad replikace v **dialogovém okně Nastavení** > **replikace** > název zásady > **Přidružení Hyper-V webu**.

## <a name="step-5-capacity-planning"></a>Krok 5: Plánování kapacity

Teď, když máte v basic infrastruktury nastavení se může přemýšlet o plánování kapacity a zjistit, jestli budete potřebovat další zdroje informací.

Obnovení webu poskytuje kapacita plánování vám přidělit správné zdroje na prostředí zdroje, součásti obnovení webů, sítě a úložiště. Plánování můžete spustit v rychlém režimu odhady podle průměrný počet VMs, disků a úložišť nebo v podrobné režimu, ve kterém se vstupní hodnoty na úrovni zátěží na projektu. Než začnete musíte:

- Shromážděte informace o prostředí replikace, včetně VMs disků za VMs a úložiště na disku.
- Odhad denní změnit sazba (konve), které budete mít k dispozici pro replikovanou data. [Plánování doby pro Hyper-V otevřené](https://www.microsoft.com/download/details.aspx?id=39057) umožňuje vám to udělat.

1.  Klikněte na **Stáhnout** pro stažení nástroje a pak ho spusťte. [Přečtěte si tento článek](site-recovery-capacity-planner.md) , který doprovází nástroj.
2.  Až to budete mít v **jste spustili plánovači kapacita**výběrem **Ano** ?

    ![Plánování kapacity](./media/site-recovery-hyper-v-site-to-azure/gs-capacity-planning.png)

### <a name="network-bandwidth-considerations"></a>Aspektech šířky pásma sítě

Nástroj Plánovač kapacita slouží k výpočtu šířku pásma, které potřebujete pro replikace (počáteční replikace a potom delta). K řízení počtu využití šířky pásma pro replikace máte několik možností:

- **Omezení šířky pásma**: přenosy Hyper-V zreplikuje na Azure prochází určitému Hyper-V hostiteli. Můžete šířku pásma na hostitelském serveru.
- **Doladit šířky pásma**: může mít vliv šířku pásma pro replikace pomocí několika jednoduchých klíče registru.

#### <a name="throttle-bandwidth"></a>Omezení šířky pásma

1. Otevřete modulu snap-in konzoly MMC Zálohování Microsoft Azure na hostitelském serveru Hyper-V. Ve výchozím nastavení je k dispozici na stolním počítači nebo v C:\Program Files\Microsoft Azure obnovení služby Agent\bin\wabadmin zástupce na ploše pro aplikaci Microsoft Azure zálohování.
2. V modulu snap-in klikněte na **Změnit vlastnosti**.
3. Na kartě **Throttling** zaškrtněte políčko **Povolit omezení pro zálohování využití šířky pásma Internetu**a nastavit omezení pro práci a není funkční hodiny. Platné oblasti mají z 512 kB/s 102 MB / sekundu.

    ![Omezení šířky pásma](./media/site-recovery-hyper-v-site-to-azure/throttle2.png)

Taky můžete použít rutinu [Set-OBMachineSetting](https://technet.microsoft.com/library/hh770409.aspx) nastavit omezení. Tady je ukázka:

    $mon = [System.DayOfWeek]::Monday
    $tue = [System.DayOfWeek]::Tuesday
    Set-OBMachineSetting -WorkDay $mon, $tue -StartWorkHour "9:00:00" -EndWorkHour "18:00:00" -WorkHourBandwidth  (512*1024) -NonWorkHourBandwidth (2048*1024)

**Nastavení OBMachineSetting NoThrottle** označuje, že není potřeba žádná omezení.


#### <a name="influence-network-bandwidth"></a>Vliv šířka pásma

1. V registru přejděte **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure Backup\Replication**.
    - K ovlivnění šířky pásma přenosy na replikace disku, změňte hodnotu **UploadThreadsPerVM**nebo vytvořte klávesu Pokud neexistuje.
    - Chcete-li ovlivnit šířky pásma pro přenos navrácení z Azure, změňte hodnotu **DownloadThreadsPerVM**.
2. Výchozí hodnota je 4. V síti "overprovisioned" by měl být tyto klíče registru změnil z výchozích hodnot. Maximální hodnota je 32. Sledování provozu optimalizovat hodnotu.

## <a name="step-6-enable-replication"></a>Krok 6: Povolit replikace

Teď povolte následujícím způsobem:

1. Klikněte na **Krok 2: replikovat aplikace** > **zdroje**. Poté, co jste povolili replikace poprvé budete klikněte na **+ replikovat** trezoru povolit replikace pro další počítače.

    ![Povolení replikace](./media/site-recovery-hyper-v-site-to-azure/enable-replication.png)

2. V zásuvné **zdroj** > vyberte web Hyper-V. Klikněte na **OK**.
3. V **cílové** vyberte předplatné trezoru a převzetí modelu, který chcete použít v Azure (Správa classic nebo zdroje) po překlopení.
4. Vyberte úložiště účet, který chcete použít. Pokud chcete používat účet jiné úložiště než těch, kterým jste můžete [vytvořit](#set-up-an-azure-storage-account). Vytvoření úložiště účtu pomocí Správce prostředků modelu klikněte na **vytvořit nový**. Pokud chcete vytvořit účet úložiště pomocí klasického modelu můžete udělat modul [Azure portálu](../storage/storage-create-storage-account-classic-portal.md). Klikněte na **OK**.
5.  Vyberte Azure síťových připojení a podsítě, ke kterému Azure VMs připojí, když jste nespředený nahoru po překlopení. Vyberte **konfigurovat nyní u vybraných počítačů** použít nastavení sítě do všech počítačů, které můžete vybrat ochranu. Vyberte **Konfigurovat později** vyberte Azure sítě jednotlivé počítače. Pokud chcete použít jiné síti od těch, kterým jste můžete [vytvořit](#set-up-an-azure-network). Pokud chcete vytvořit síť pomocí Správce prostředků modelu klikněte na **vytvořit nový**. Pokud chcete vytvořit síť pomocí klasického modelu můžete udělat modul [Azure portálu](../virtual-network/virtual-networks-create-vnet-classic-pportal.md). V případě potřeby vyberte podsítě. Klikněte na **OK**.

    ![Povolení replikace](./media/site-recovery-hyper-v-site-to-azure/enable-replication11.png)

6. Ve **virtuálních počítačích** > **Vyberte virtuálních počítačích** klikněte na a vyberte každý počítač chcete replikovat. Můžete zvolit pouze počítačích, pro které lze replikace povoleno. Klikněte na **OK**.

    ![Povolení replikace](./media/site-recovery-hyper-v-site-to-azure/enable-replication5.png)

11. V **dialogovém okně Vlastnosti** > **Konfigurovat vlastnosti**vyberte operační systém, pro vybraný VMs a disku s operačním systémem. Ověření, že Azure OM název (cíl) splňuje požadavky na [Azure virtuálního počítače](site-recovery-best-practices.md#azure-virtual-machine-requirements) a v případě potřeby ho upravte. Klikněte na **OK**. Můžete nastavit další vlastnosti později.

    ![Povolení replikace](./media/site-recovery-hyper-v-site-to-azure/enable-replication6.png)

12. V **dialogovém okně nastavení replikace** > **nastavení replikace konfigurovat**, vyberte zásadu replikace chcete použít pro chráněné VMs. Klikněte na **OK**. Můžete změnit zásady replikace v **dialogovém okně Nastavení** > **replikace zásady** > název zásady > **Upravit nastavení**. Změny, které použijete bude sloužit k stroje, které jsou již replikace a nové počítačích.

    ![Povolení replikace](./media/site-recovery-hyper-v-site-to-azure/enable-replication7.png)

Můžete sledovat průběh **Povolit ochranu** úlohy v **dialogovém okně Nastavení** > **úlohy** > **úloh obnovení webu**. Když v počítači spuštěná úloha **Dokončení ochrany** je připraven k převzetí.

### <a name="view-and-manage-vm-properties"></a>Zobrazit a spravovat vlastnosti OM

Doporučujeme ověřit vlastnosti zdrojového počítače.

1. Klikněte na **Nastavení** > **Chráněné položky** > **Replikovat položky** > a vyberte počítač.

    ![Povolení replikace](./media/site-recovery-hyper-v-site-to-azure/test-failover1.png)

2. V **dialogovém okně Vlastnosti** můžete zobrazit informace o OM replikace a překlopení.

    ![Povolení replikace](./media/site-recovery-hyper-v-site-to-azure/test-failover2.png)

3. Ve **výpočetním a síťové** > **Výpočet vlastnosti** můžete zadat Azure OM název a cílové velikost. Název dodržovat Azure požadavky v případě potřeby změňte. Můžete taky zobrazit a upravit informace o cílové síti, podsítě a IP adresy přiřazené k OM Azure. Poznámka:

    - Můžete nastavit cílová IP adresa. Pokud neposkytnete adresu, selhalo přes počítač bude používat DHCP. Pokud jste nastavili adresu, která není k dispozici na převzetí, záložní se nezdaří. Stejné cílová IP adresa lze test převzetí Pokud je k dispozici v síti převzetí test na adresu.
    - Počet síťové adaptéry je dáno velikost zadané pro cílové virtuální počítač, následujícím způsobem:

        - Pokud argument číslo síťové adaptéry zdrojového počítače menší nebo rovna hodnotě počet adaptéry povolená velikost v počítači cílového, pak cíl bude mít stejný počet adaptéry jako zdroj.
        - Pokud počet adaptéry virtuálního počítače zdroj překročí povolený Cílová velikost a pak maximální velikost cílové bude použito počet.
        - Pokud například zdrojového počítače obsahuje dva síťové adaptéry a cílová velikost počítač podporuje čtyři, cílový počítač bude mít dva adaptéry. Pokud zdrojového počítače obsahuje dva adaptéry ale podporované Cílová velikost podporuje pouze jednu cílový počítač bude mít jenom jeden adaptér.     
        - Pokud OM má více síťové adaptéry budou všechny připojí ke stejné síti.

    ![Povolení replikace](./media/site-recovery-hyper-v-site-to-azure/test-failover4.png)

5.  Na **disku** uvidíte operačního systému a disků dat na OM, který bude replikovat.


## <a name="step-7-test-the-deployment"></a>Krok 7: Testování nasazení

Chcete-li otestovat nasazení můžete spustit přepojení test pro jeden počítač virtuální nebo obnovení plán, který obsahuje jeden nebo více virtuálních počítačích.


### <a name="prepare-for-test-failover"></a>Příprava na test překlopení

- Spusťte test selhání doporučujeme vytvořit novou Azure síť, který má samostatný z Azure sítě (Toto je výchozí chování v Azure vytvoříte novou síť). [Další informace](site-recovery-failover.md#run-a-test-failover) o tom, jak převzetí služeb při selhání testu.
- Nejlepší možný výkon při můžete převzít Azure získáte nainstalujte agenta Azure na zamknutém počítač. Převede spuštění rychleji a pomáhá slouží k řešení potíží. Nainstalujte agent [Linux](https://github.com/Azure/WALinuxAgent) nebo [Windows](http://go.microsoft.com/fwlink/?LinkID=394789) .
- Plně otestovat nasazení musíte mít infrastrukturu pro replikovanou počítač tak, aby fungovaly očekávaným. Pokud chcete testovat Active Directory a DNS můžete vytvořit virtuální počítač jako řadiče domény se službou DNS a replikovat to Azure pomocí obnovení webu Azure. Přečtěte si další v [aspektech převzetí test pro služby Active Directory](site-recovery-active-directory.md#considerations-for-test-failover).
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

- Přidejte veřejnou IP adresu na NIC přidružené OM Azure umožňuje RDP.
- Ujistěte se, že nemáte žádné zásady domény, kvůli kterým jste připojení k virtuálního počítače pomocí veřejné adresy.
- Pokuste se připojit. Pokud se nemůžete spojit zkontrolujte, jestli je spuštěný OM. Další tipy pro řešení problémů v tomto [článku](http://social.technet.microsoft.com/wiki/contents/articles/31666.troubleshooting-remote-desktop-connection-after-failover-using-asr.aspx).

Pokud chcete získat přístup Azure OM systémem Linux po převzetí pomocí zabezpečené prostředí klienta (ssh), postupujte takto:

**V místním počítači před převzetí**:

- Ověřte, zda službu zabezpečené Shell na OM Azure aby se spouštěl automaticky při spuštění systému.
- Zkontrolujte, jestli pravidla brány firewall SSH připojení k němu.

**V Azure OM po převzetí služeb**:

- Pravidla skupiny zabezpečení sítě o nezdařeném uložení přes OM a Azure podsítí, ke kterému je připojen muset povolit SSH port pro příchozí připojení.
- Veřejné koncový bod má vytvořit za účelem povolení příchozí připojení na SSH port (port TCP 22 ve výchozím nastavení).
- Pokud OM k nim získat přístup přes připojení virtuální privátní sítě (VPN na webu nebo Express směrování) potom klienta mohou sloužit přímo připojit přes SSH bude v angličtině.

### <a name="run-a-test-failover"></a>Spusťte test selhání

Chcete-li spusťte test převzetí udělejte následující:

1. Převzetí jednoho OM v **dialogovém okně Nastavení** > **Replikovat položky**, klikněte OM > **+ převzetí testu**.

    ![Převzetí test](./media/site-recovery-hyper-v-site-to-azure/run-failover1.png)

2. Překlopení obnovení plán, v **dialogovém okně Nastavení** > jednotného zasílání zpráv**Obnovení plány jednotného zasílání zpráv**, klikněte pravým tlačítkem myši plán > **Převzetí testu**. Vytvoření obnovení plánování, [postupujte podle těchto pokynů](site-recovery-create-recovery-plans.md).

3. V **Testu převzetí** vyberte Azure sítě, ke kterému bude připojený Azure VMs, když dojde k selhání.

    ![Převzetí test](./media/site-recovery-hyper-v-site-to-azure/run-failover2.png)

4. Klepnutím na **OK** spusťte záložní. Sledování průběhu kliknutím na OM otevřete jeho properies nebo na **Test převzetí** úkolu v **dialogovém okně Nastavení** > **úloh obnovení webu**.
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


## <a name="failover"></a>Překlopení

Po dokončení pro počítače počáteční replikace vyvoláte převzetí služeb při selhání podle potřeby. Obnovení webu podporuje různé typy převzetí služeb při selhání - Test převzetí, plánovaná převzetí a neplánované převzetí.
[Další informace](site-recovery-failover.md) o různých typů převzetí služeb při selhání a podrobný popis ze kdy a jak provádět každý z nich.

> [AZURE.NOTE] -Li váš záměr virtuálních počítačích migrovat do Azure, doporučujeme použít [plánované převzetí operace](site-recovery-failover.md#run-a-planned-failover-primary-to-secondary) migrace virtuálních počítačích do Azure. Po ověření migrované aplikace v Azure pomocí převzetí test postupujte podle pokynů uvedených v části [Dokončení migrace](#Complete-migration-of-your-virtual-machines-to-Azure) k dokončení migrace virtuálních počítačích. Nemusíte provádět potvrdit nebo odstranit. Dokončení migrace dokončí migrace, odstraní ochranu virtuálního počítače a přestane obnovení webu Azure fakturace na počítači.


###<a name="run-an-unplanned-failover"></a>Spuštění neplánované překlopení

To je třeba zvolit, až bude hlavní web přístupný z důvodu neočekávané incidentu, například power výpadku nebo virový útok. Tento postup ke spuštění "neplánované převzetí" pro plán obnovení. Převzetí jednoho virtuálního počítače můžete taky můžete spustit na kartě virtuálních počítačích. Než začnete, zkontrolujte, že všechny virtuálních počítačích požadované převzetí dokončili počáteční replikace.

1. Vyberte **obnovení plány jednotného zasílání zpráv > recoveryplan_name**.
2. Na zásuvné plán obnovení, klikněte na **Neplánované převzetí**.
3. Na stránce **neplánované převzetí** zvolte zdrojové a cílové umístění.
4. Vyberte **vypnout virtuálních počítačích a synchronizovat nejnovější data** můžete určit, že obnovení webu pokuste ukončit chráněné virtuálních počítačích a synchronizovat data tak, aby se nezdaří nejnovější verzi data nad.
5. Převzetí virtuálních počítačích po v potvrdit nevyřízená.  Klikněte na tlačítko **Potvrdit** potvrďte záložní.

[Víc se uč](site-recovery-failover.md#run-an-unplanned-failover)

## <a name="complete-migration-of-your-virtual-machines-to-azure"></a>Provedení migrace virtuálních počítačích na Azure


>[AZURE.NOTE] Následující postup použít jenom v případě, že migrujete virtuálních počítačích Azure

1. Zastupování plánované převzetí uvedené [v tomto poli](site-recovery-failover.md)
2. V **Nastavení > replikovat položky**, klikněte pravým tlačítkem myši virtuálního počítače a vyberte **Úplné migrace**

    ![completemigration](./media/site-recovery-hyper-v-site-to-azure/migrate.png)

2. Klikněte na **OK** dokončete migrace. Kliknutím na OM zobrazíte její vlastnosti nebo pomocí dokončení migrace úlohy v můžete sledovat průběh **Nastavení > úloh obnovení webu**.


## <a name="monitor-your-deployment"></a>Sledování nasazení

Tady je, jak můžete sledovat nastavení, stavu a stavu obnovení webu nasazení:

1. Klikněte na název trezoru pro přístup k řídicím panelu **Essentials** . V tomto řídicím panelu můžete úloh obnovení webu, stav replikace, plány obnovy, stavu serveru a události.  Můžete přizpůsobit Essentials zobrazíte dlaždic a rozložení, které jsou pro vás, včetně stavu ostatních zálohování a obnovení webu trezorů nejvhodnější.

    ![Základy](./media/site-recovery-hyper-v-site-to-azure/essentials.png)

2. V této dlaždici **stavu** můžete sledovat web serverech dochází k problému a události vyvolané obnovení webu za posledních 24 hodin.
3. Správa a sledování replikace v **Replikovat položky**jednotného zasílání zpráv **Obnovení plány jednotného zasílání zpráv**, a dlaždice **Úlohy obnovení webů** . Můžete procházení datových krychlí úlohy v **dialogovém okně Nastavení** -> **úlohy** -> **Úlohy obnovení webů**.
