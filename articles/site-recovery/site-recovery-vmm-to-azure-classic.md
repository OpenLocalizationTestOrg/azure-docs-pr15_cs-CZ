<properties
    pageTitle="Replikovat Hyper-V virtuálních počítačích v VMM mračnech Azure | Microsoft Azure"
    description="Tento článek popisuje, jak replikovat Hyper-V virtuálních počítačích na Hyper-V hosts vyhledali v System Center VMM mračnech na Azure."
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
    ms.topic="hero-article"
    ms.date="05/06/2016"
    ms.author="raynew"/>

#  <a name="replicate-hyper-v-virtual-machines-in-vmm-clouds-to-azure"></a>Replikace Hyper-V virtuálních počítačích v VMM mračnech na Azure

> [AZURE.SELECTOR]
- [Azure portálu](site-recovery-vmm-to-azure.md)
- [Prostředí PowerShell – správce](site-recovery-vmm-to-azure-powershell-resource-manager.md)
- [Klasický portálu](site-recovery-vmm-to-azure-classic.md)
- [Prostředí PowerShell – klasické](site-recovery-deploy-with-powershell.md)



Obnovení webu Azure služby přispívá k strategie firmy kontinuitu a havárie obnovení (BCDR) tak, že orchestrating replikace, převzetí a obnovení virtuálních počítačích a fyzické servery. Počítačích replikovat Azure nebo vedlejší místního datovém centru. Rychlý přehled číst [Co je obnovení webu Azure?](site-recovery-overview.md).

## <a name="overview"></a>Základní informace

Tento článek popisuje, jak nasazení obnovení webu replikovat Hyper-V virtuálních počítačích na Hyper-V hostitelské servery, které máte uložené v soukromých mračnech VMM na Azure.

Tento článek obsahuje požadavky pro scénář a ukazuje, jak nastavit trezoru obnovení webu, zjištění poskytovatel obnovení webu Azure nainstalovaným zdrojovému serveru VMM registrace serveru v trezoru, přidat účet Azure úložiště, instalace služby Azure Recovery agent na serverech hostitele Hyper-V, nastavení ochrany VMM shluky, které se použije na všechny chráněné virtuálních počítačích a pak ji povolit ochranu pro tyto virtuálních počítačích. Dokončete nastavení testováním převezme zkontrolujte, jestli že všechno funguje očekávaným způsobem.

V dolní části tohoto článku nebo na [Fórum služby Azure obnovení](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr)pokládat komentáře nebo dotazy.

## <a name="architecture"></a>Architektura

![Architektura](./media/site-recovery-vmm-to-azure-classic/topology.png)

- Obnovení poskytovatel webu Azure nainstalovaných VMM během obnovení webu nasazení a VMM server registrován trezoru obnovení webu. Zprostředkovatel komunikaci se obnovení webu zpracovávání replikace průběhu.
- Agent služby Azure Recovery nainstalovaný na serverech hostitele Hyper-V během obnovení webu nasazení. Zpracovává replikace dat k základnímu úložišti Azure.


## <a name="azure-prerequisites"></a>Požadavky na Azure

Tady je budete potřebovat v Azure.

**Předpoklady** | **Podrobnosti**
--- | ---
**Účet Azure**| Musíte mít účet [Microsoft Azure](https://azure.microsoft.com/) . Můžete začít s [bezplatnou zkušební verzi](https://azure.microsoft.com/pricing/free-trial/). [Další informace](https://azure.microsoft.com/pricing/details/site-recovery/) o obnovení webu ceny.
**Azure úložiště** | Musíte mít účet Azure úložiště, který chcete uložit replikovanou data. Replikovanou data se ukládají v Azure úložiště a Azure VMs jsou nespředený nahoru, když dojde k selhání. <br/><br/>Potřebujete [Standardní geo nadbytečné úložiště účtu](../storage/storage-redundancy.md#geo-redundant-storage). Účet, musíte ve stejné oblasti jako služba obnovení webu a být přidružené stejném předplatném. Všimněte si, že replikace k účtům úložiště premium není aktuálně podporován a není určená k použití.<br/><br/>[Přečtěte si informace o](../storage/storage-introduction.md) Azure úložiště.
**Azure sítě** | Budete potřebovat Azure virtuální sítě, který Azure VMs připojí k selháním. Azure virtuální sítě musí být ve stejné oblasti jako trezoru obnovení webu.

## <a name="on-premises-prerequisites"></a>Zjistit předpoklady pro místní

Tady je co budete potřebovat místní.

**Předpoklady** | **Podrobnosti**
--- | ---
**VMM** | Musíte mít alespoň jeden server VMM nasazený jako samostatný fyzické nebo virtuální server nebo jako virtuální obrázku. <br/><br/>VMM server by měla běžet System Center 2012 R2 s nejnovějšími kumulativními aktualizacemi.<br/><br/>Musíte mít alespoň jeden cloudu nakonfigurovaný na serveru VMM.<br/><br/>Cloudové zdroje, které chcete zamknout musí obsahovat jednu nebo více skupin VMM Host (hostitel).<br/><br/>Další informace o nastavení VMM mračnech v [Návod: Vytvoření soukromého mračnech s System Center 2012 SP1 VMM](http://blogs.technet.com/b/keithmayer/archive/2013/04/18/walkthrough-creating-private-clouds-with-system-center-2012-sp1-virtual-machine-manager-build-your-private-cloud-in-a-month.aspx) na Dryml Mayer blogu.
**Hyper-V** | Budete potřebovat pro Hyper-V hostitelské servery nebo clusterů v cloudu VMM. By měla být serveru a jeden nebo více VMs. <br/><br/>Server pro Hyper-V musí běžet aspoň **Windows serveru 2012 R2** s rolí Hyper-V nebo **Microsoft Hyper-V serveru 2012 R2** a nainstalovali nejnovější aktualizace.<br/><br/>Všechny Hyper-V server obsahující VMs chcete zamknout musí být umístěné v cloudu VMM.<br/><br/>Pokud používáte Hyper-V v poznámce clusteru této clusteru zprostředkovatele není automaticky vytvoří Pokud máte statickou IP adresu clusteru založený. Musíte ruční konfiguraci zprostředkovatele obrázku. [Další informace](https://www.petri.com/use-hyper-v-replica-broker-prepare-host-clusters) v Aidan Finn blogu.
**Chráněný počítačích** | VMs, které chcete zamknout musí odpovídat požadavkům na [Azure](site-recovery-best-practices.md#azure-virtual-machine-requirements).


## <a name="network-mapping-prerequisites"></a>Předpoklady mapování sítě
Když uložíte chránit virtuálních počítačích v mapách Azure sítě mapování mezi OM sítí na zdrojovém VMM serveru a obrázku Azure sítí povolit následující:

- Které převzetí ve stejné síti se můžete připojit k sobě, bez ohledu na to, jaký plán obnovení spadají do všech počítačů.
- Pokud brána sítě je nastavena na cílovém Azure sítě, virtuálních počítačích můžete připojit k jiné místní virtuálních počítačích.
- Pokud není konfigurace sítě mapování pouze virtuálních počítačích, které selhání v plánu stejného obnovení bude moct připojit k sobě po přepnutí Azure.

Pokud chcete nasadit mapování sítě budete potřebovat takto:

- Virtuálních počítačích, které chcete zamknout na zdrojovém VMM serveru by měl být připojení k síti OM. Síť by měl být propojené s logická síť, která je přidružená k cloudu.
- Azure sítě, ke kterému se můžete připojit replikovanou virtuálních počítačích po převzetí. V době převzetí vyberete této sítě. Ve stejné oblasti jako předplatné obnovení webu Azure by měl být v síti.

Příprava sítě mapování následujícím způsobem:

1. [Přečtěte si informace o](site-recovery-network-mapping.md) požadavky mapování sítě.
2. Příprava OM sítě VMM:

    - [Nastavení logických sítí](https://technet.microsoft.com/library/jj721568.aspx).
    - [Nastavení sítě OM](https://technet.microsoft.com/library/jj721575.aspx).


## <a name="step-1-create-a-site-recovery-vault"></a>Krok 1: Vytvoření trezoru obnovení webu

1. Přihlaste se k [Portálu Správa](https://portal.azure.com) z VMM server, ke kterému se chcete zaregistrovat.
2. Klikněte na tlačítko **datové služby** > **služby Recovery** > **webu obnovení trezoru**.
3. Klikněte na **vytvořit nový** > **vytvořit**.
4. Do pole **název**zadejte popisný název k identifikaci trezoru.
5. V **oblasti**vyberte zeměpisná oblast pro trezoru. Kontrola podporovaných oblastí najdete v článku zeměpisné dostupnost v [Azure webu obnovení ceny podrobností](https://azure.microsoft.com/pricing/details/site-recovery/).
6. Klikněte na **vytvořit trezoru**.

    ![Nové trezoru](./media/site-recovery-vmm-to-azure-classic/create-vault.png)

Zkontrolujte na stavovém řádku potvrďte, že byla úspěšně vytvořena trezoru. Trezoru se zobrazí jako **aktivní** na hlavní stránce obnovení služby.

## <a name="step-2-generate-a-vault-registration-key"></a>Krok 2: Vygenerování trezoru registračního klíče

Generování registračního klíče v trezoru. Po stažení poskytovatel obnovení webu Azure a nainstalujte ji na VMM server použijete tento klíč k registraci VMM serveru v trezoru.

1. Na stránce **Služby Recovery** na trezoru a otevře se stránka rychlý Start. Představení aplikace můžete otevřít také kdykoli pomocí ikony.

    ![Ikona rychlým startem](./media/site-recovery-vmm-to-azure-classic/qs-icon.png)

2. V rozevíracím seznamu vyberte **mezi webem VMM místní a Microsoft Azure**.
3. V části **Připravit VMM servery**klikněte na soubor **Generovat registračního klíče** . Soubor s klíčem je automaticky generované a platí 5 dní po vygenerování. Pokud nepoužíváte přístup k portálu Azure ze serveru VMM musíte zkopírovat tohoto souboru na server.

    ![Registrační klíč](./media/site-recovery-vmm-to-azure-classic/register-key.png)

## <a name="step-3-install-the-azure-site-recovery-provider"></a>Krok 3: Instalace zprostředkovatele obnovení Azure webů

1. V **Rychlém** > **VMM připraví servery**, klikněte na **Stáhnout Microsoft Azure obnovení poskytovatel webu pro instalaci na serverech VMM** nejnovější verzi souboru instalace zprostředkovatele.
2. Spusťte tento soubor na serveru VMM zdroje.

    >[AZURE.NOTE] Pokud VMM nasazenou v clusteru a instalujete poskytovatele první nainstalovat na aktivní uzel a dokončete instalaci registrace serveru VMM v trezoru. V jednotlivých uzlech nainstalujte na poskytovatele. Poznámka: Pokud upgradujete na poskytovatele budete muset upgradovat na všechny uzly vzhledem k tomu, že se mají všechny běžet ve stejné verzi poskytovatele.

3. Instalační program provede kontrolu prerequirements a žádosti o oprávnění k zastavit službu VMM zahájíte instalace zprostředkovatele. Po dokončení instalace je bude třeba restartovat službu VMM automaticky. Pokud instalujete clusteru VMM budete výzva k ukončení roli obrázku.

4. Ve **Službě Microsoft Update** můžete se rozhodnout v aktualizací. Při tomto nastavení povolené poskytovatele podle předpisů rozhraní Microsoft Update zásad bude instalovat aktualizace.

    ![Aktualizace společnosti Microsoft](./media/site-recovery-vmm-to-azure-classic/updates.png)


5.  Umístění instalace zprostředkovatele nastavena na ** <SystemDrive>\Program Files\Microsoft System Center 2012 R2\Virtual počítače Manager\bin**. Klikněte na **instalovat**.

    ![InstallLocation](./media/site-recovery-vmm-to-azure-classic/install-location.png)

6. Po dokončení instalace zprostředkovatele klikněte na **zaregistrovat** k registraci serveru v trezoru.

    ![InstallComplete](./media/site-recovery-vmm-to-azure-classic/install-complete.png)

9. Do pole **název trezoru**ověřte název trezoru, ve kterém bude registrován serveru. Klikněte na tlačítko *Další*.

    ![Registrace serveru](./media/site-recovery-vmm-to-azure-classic/vaultcred.PNG)

7. Připojení k **Internetu** určete způsob poskytovatele na serveru VMM připojení k Internetu. Vyberte **připojení s nastavením proxy serveru existující** používat výchozí nastavení připojení k Internetu nakonfigurovaný na serveru.

    ![Nastavení sítě Internet](./media/site-recovery-vmm-to-azure-classic/proxydetails.PNG)

    - Pokud chcete použít vlastní proxy by měl nastavením před instalací zprostředkovatele. Při konfiguraci nastavení serveru proxy vlastní test se spustí při kontrole připojení proxy k.
    - Pokud používáte vlastní proxy nebo výchozí proxy server vyžaduje ověření budete muset zadávat podrobnosti proxy, včetně adresy proxy serveru a portu.
    - Sledovat URL by měl být přístupné z informací o serveru VMM a hosts Hyper-v
        - *. hypervrecoverymanager.windowsazure.com
        - *. accesscontrol.windows.net
        - *. backup.windowsazure.com
        - *. blob.core.windows.net
        - *. store.core.windows.net
    - Povolte IP adresy podle [Rozsahy IP Azure datacentru](https://www.microsoft.com/download/confirmation.aspx?id=41653) a HTTPS (443) Protocol (protokol). Máte by a rozsahy IP adres bílé seznamu Azure oblasti, kterou chcete použít, západ USA.
    - Pokud používáte vlastní proxy VMM RunAs účtu (DRAProxyAccount) se vytvoří automaticky pomocí přihlašovacích údajů zadané proxy. Konfigurace proxy server tak, aby tento účet může ověřovat úspěšně. Nastavení účtu VMM RunAs můžete měnit v konzole VMM. K tomuto účelu **Nastavení** pracovní prostor otevřít, rozbalte **zabezpečení**, klikněte na **Spustit jako účty**a potom změnit heslo pro DRAProxyAccount. Je potřeba restartovat službu VMM tak, aby toto nastavení se projeví.


8. **Registrační klíč**vyberte klávesy, která stažené ze obnovení webu Azure a zkopírovala do VMM serveru.


10.  Nastavení šifrování se používá pouze když jste replikace Hyper-V VMs v VMM mračnech Azure. Pokud jste replikace sekundární web nepoužívá.

11.  V poli **název serveru**zadejte popisný název k identifikaci VMM serveru v trezoru. V konfiguraci clusteru zadejte název VMM clusteru role.
12.  V poli vyberte **synchronizovat cloudu metadat** jestli chcete k synchronizaci metadat pro všechny shluky na serveru VMM s trezoru. Tato akce jenom s jednou na všech serverech musí. Pokud nechcete synchronizovat všechny shluky, můžete nechat toto nastavení zaškrtnuté a synchronizovat každý cloudové jednotlivě ve vlastnostech cloudové v konzole VMM.

13.  Klikněte na **Další** a dokončit. Po registraci metadata ze serveru VMM načítané obnovení webu Azure. Server se zobrazí na kartě **Servery VMM** na stránce **servery** v trezoru.

    ![LastPage](./media/site-recovery-vmm-to-azure-classic/provider13.PNG)

Po registraci metadata ze serveru VMM načítané obnovení webu Azure. Server se zobrazí na kartě **Servery VMM** na stránce **servery** v trezoru.

### <a name="command-line-installation"></a>Přepínač příkazového řádku instalace

Obnovení poskytovatel webu Azure lze také nainstalovat pomocí následující příkazový řádek. Tato metoda slouží k instalaci zprostředkovatele na serveru Core pro Windows Server 2012 R2.

1. Stáhněte instalační soubor a registrace klíč poskytovatele do složky. Příklad: C:\ASR.
2. Zastavit službu správce pro System Center virtuálního počítače
3. Příkazový řádek se zvýšenými oprávněními extrahujte poskytovatele Instalační služby systému pomocí následujících příkazů:

        C:\Windows\System32> CD C:\ASR
        C:\ASR> AzureSiteRecoveryProvider.exe /x:. /q

4. Instalace zprostředkovatele následujícím způsobem:

        C:\ASR> setupdr.exe /i

5. Zprostředkovatel zaregistrujte následujícím způsobem:

        CD C:\Program Files\Microsoft System Center 2012 R2\Virtual Machine Manager\bin
        C:\Program Files\Microsoft System Center 2012 R2\Virtual Machine Manager\bin\> DRConfigurator.exe /r  /Friendlyname <friendly name of the server> /Credentials <path of the credentials file> /EncryptionEnabled <full file name to save the encryption certificate>       

Kde jsou parametry následujícím způsobem:

 - **/Credentials** : povinný parametr, který určuje umístění, ve kterém je umístěn soubor klíčové registrace  
 - **/FriendlyName** : povinný parametr název serveru hostitele Hyper-V, která se zobrazí v portálu obnovení webu Azure.
 - **/EncryptionEnabled** : volitelný parametr můžete určit, jestli chcete šifrování virtuálních počítačích v Azure (u ostatních šifrování). Název souboru by mít příponu **.pfx** .
 - **/proxyAddress** : volitelný parametr, který určuje adresy proxy serveru.
 - **/proxyport** : volitelný parametr, který určuje port proxy serveru.
 - **/proxyUsername** : volitelný parametr, který určuje proxy uživatelské jméno.
 - **/proxyPassword** : volitelný parametr, který určuje heslo proxy serveru.  


## <a name="step-4-create-an-azure-storage-account"></a>Krok 4: Vytvoření účet Azure úložiště

1. Pokud nemáte účet Azure úložiště klikněte na **Přidat účet Azure úložiště** si vytvořit účet.
2. Vytvořte účet s geo replikace povolené. Musíte ve stejné oblasti jako služba obnovení webu Azure a být přidružené stejném předplatném.

    ![Úložiště účtu](./media/site-recovery-vmm-to-azure-classic/storage.png)

> [AZURE.NOTE] [Migrace z úložiště účtů](../resource-group-move-resources.md) přes skupiny zdrojů ve stejném předplatném nebo přes předplatná není podporována u účtů úložiště použít pro nasazení obnovení webu.

## <a name="step-5-install-the-azure-recovery-services-agent"></a>Krok 5: Instalace agenta služby Azure obnovení

Nainstalujte agenta služby Azure Recovery na všech serverech hostitele Hyper-V v cloudu VMM.

1. Klikněte na tlačítko **Rychlý Start** > **Stáhnout služby agenta obnovení Azure webu a nainstalujte na hosts** nejnovější verzi agent instalační soubor.

    ![Instalace agenta obnovování služby](./media/site-recovery-vmm-to-azure-classic/install-agent.png)

2. Spuštění instalačního souboru na všech serverech hostitele Hyper-V.
3. Na stránce **Zkontrolujte požadavky** na tlačítko **Další**. Chybějící požadavky automaticky nainstaluje.

    ![Zjistit předpoklady pro obnovení služby Agent](./media/site-recovery-vmm-to-azure-classic/agent-prereqs.png)

4. Na stránce **Nastavení instalace** zadejte místo, kam chcete nainstalovat agenta a vyberte umístění mezipaměti, ve kterém záložní metadat nainstalovaný. Klikněte na **nainstalovat**.
5. Po dokončení instalace klikněte na **Zavřít** ukončete průvodce.

    ![Registrace MARS Agent](./media/site-recovery-vmm-to-azure-classic/agent-register.png)

### <a name="command-line-installation"></a>Přepínač příkazového řádku instalace

Agent služby Microsoft Azure obnovení si taky můžete nainstalovat z příkazového řádku pomocí tohoto příkazu:

    marsagentinstaller.exe /q /nu

## <a name="step-6-configure-cloud-protection-settings"></a>Krok 6: Konfigurace cloudu nastavení ochrany

Po registraci VMM serveru můžete nakonfigurovat nastavení ochrany cloudu. Možnost **synchronizovat data cloudu s trezoru** povolili, když jste si nainstalovali zprostředkovatele tak všechny shluky na serveru VMM se zobrazí na kartě <b>Chráněné položky</b> v trezoru.

![Publikované cloudu](./media/site-recovery-vmm-to-azure-classic/clouds-list.png)

1. Na stránce Snadné spuštění klikněte na **Nastavení ochrany VMM mračnech**.
2. Na kartě **Chráněné položky** klikněte na cloudu, který chcete konfigurovat a přejděte na kartu **Konfigurace** .
3. V **cílové** vyberte **Azure**.
4. **Úložiště** účtu vyberte účet Azure úložiště, který používáte pro replikace.
5. Nastavte **zašifrovat uložená data** **vypnout**. Tohle nastavení určuje, že data zašifrovat replikovat mezi místních webů a Azure.
6. Do pole **zkopírovat počet_plateb** ponechejte výchozí nastavení. Tato hodnota určuje, jak často synchronizována data mezi zdrojovým a cílovým umístění.
7. **Zachovat obnovení bodem, pokud**ponechejte výchozí nastavení. S výchozí hodnotou nula jenom nejnovější bod obnovení primární virtuálního počítače uložené na serveru hostitele otevřené.
8. V **Četnost konzistenci aplikací snímky**ponechejte výchozí nastavení. Tato hodnota určuje, jak často se má vytvořit snímky. Snímky umožňuje zajistit, aby aplikace do konzistentního stavu po snímku, se považuje hlasitost stín kopie služby (VSS).  Pokud nastavíte hodnotu, zkontrolujte, jestli že je menší než počet bodů další obnovení, které jste nakonfigurovali.
9. V **replikace počáteční čas**zadejte při počáteční replikace dat za účelem Azure by měly začít. Časové pásmo na hostitelském serveru Hyper-V se použijí. Doporučujeme naplánovat počáteční replikace dobu největšího.

    ![Nastavení replikace cloudu](./media/site-recovery-vmm-to-azure-classic/cloud-settings.png)

Po uložení nastavení úlohy se vytvoří a můžete sledovat na kartě **projekty** . Všech serverů hostitele Hyper-V v cloudu zdroj VMM bude nakonfigurované replikace.

Po uložení lze upravovat cloudu nastavení na kartě **Konfigurovat** . Chcete-li změnit cílové umístění nebo cílového úložiště účtu musíte odebrat konfiguraci cloudu a potom znovu nakonfigurovat cloudu. Všimněte si, že se při změně účtu úložiště změnu použito pouze pro virtuálních počítačích, kteří mají povolenou ochranu po upraven účtu úložiště. Existující virtuálních počítačích se nepřenášejí do nového účtu úložiště.

## <a name="step-7-configure-network-mapping"></a>Krok 7: Konfigurace mapování sítě
Než začnete mapování sítě ověřte, že virtuálních počítačích na zdrojovém VMM serveru jsou připojené k síti OM. Kromě toho vytvořte jednu nebo víc sítí Azure virtuální. Všimněte si, že víc sítí OM možné namapovat jedné sítě Azure.

1. Na stránce Snadné spuštění klikněte na **mapovat sítě**.
2. Na kartě **sítě** vyberte **umístění zdroje**, zdrojový server VMM. V **cílové umístění** vyberte Azure.
3. V sítích **zdroje** se zobrazí seznam OM sítě přidružené k serveru VMM. V **cílové** sítě se zobrazují Azure sítě přidružené k předplatnému.
4. Vyberte zdrojová OM síť a klikněte na položku **Mapa**.
5. Na stránce **Vybrat cílové síti** vyberte cíl Azure sítě, který chcete použít.
6. Klikněte na značku zaškrtnutí dokončete proces mapování.

    ![Nastavení replikace cloudu](./media/site-recovery-vmm-to-azure-classic/map-networks.png)

Po uložení nastavení úlohy spustí sledovat průběh mapování a můžete sledovat na kartě projekty. Všechny existující otevřené virtuálních počítačích, které odpovídají zdrojová OM síť bude připojen k cílové Azure sítě. Nové virtuálních počítačích, která jsou připojená k síti OM zdroj bude připojen k Azure síťovou po replikace. Pokud změníte existující mapování s novou síť, otevřené virtuálních počítačích připojení pomocí nové nastavení.

Poznámka: Pokud cílové síti má více podsítí a jednu z těchto podsítí má stejný název jako podsítě dnem nachází virtuálního počítače zdroje a potom otevřené virtuální počítač bude připojen k této cílové podsítě po překlopení. Pokud žádná cílové podsítě s odpovídajícím názvem virtuální počítač připojen k první podsítě v síti.

> [AZURE.NOTE] [Migrace sítí](../resource-group-move-resources.md) přes skupiny zdrojů ve stejném předplatném nebo přes předplatná není podporována pro sítě použít pro nasazení obnovení webu.

## <a name="step-8-enable-protection-for-virtual-machines"></a>Krok 8: Povolte ochranu virtuálních počítačích

Po dokončení serverů, mračnech a sítí konfigurace správně, můžete povolit ochranu virtuálních počítačích v cloudu. Poznámka:

- [Azure požadavky](site-recovery-best-practices.md#azure-virtual-machine-requirements)musí splňovat virtuálních počítačích.
- Povolení ochrany operačního systému a operační systém musí být vlastnosti disku nastavená pro virtuální počítač. Při vytváření virtuálních počítačů v VMM pomocí šablony virtuálního počítače můžete nastavit vlastnost. Na kartě **Obecné** a **Konfigurace hardwaru** vlastnosti virtuálního počítače můžete nastavit taky těchto vlastností pro existující virtuálních počítačích. Pokud nenastavíte těchto vlastností v VMM si budete moct při konfiguraci v portálu obnovení webu Azure.

    ![Vytvoření virtuálního počítače](./media/site-recovery-vmm-to-azure-classic/enable-new.png)

    ![Upravit vlastnosti virtuálního počítače](./media/site-recovery-vmm-to-azure-classic/enable-existing.png)


1. Povolení ochrany na kartě **virtuálních počítačích** v cloudu, ve kterém virtuální počítač nachází, klikněte na **Povolit ochranu** > **Přidat virtuálních počítačích**.
2. V seznamu virtuálních počítačích v cloudu vyberte tu, kterou chcete zamknout.

    ![Povolení ochrany virtuálního počítače](./media/site-recovery-vmm-to-azure-classic/select-vm.png)

    Sledování průběhu **Povolit ochranu** akci na kartě **projekty** , včetně počáteční replikace. Po dlouhodobě spuštěná úloha **Dokončení ochrany** je připraven k převzetí virtuálního počítače. Když je povolena ochrana a replikovat virtuálních počítačích, budete moct tyto informace zobrazit v Azure.


    ![Virtuální počítač ochranu projektu](./media/site-recovery-vmm-to-azure-classic/vm-jobs.png)

3. Zkontrolujte vlastnosti virtuálního počítače a podle potřeby proveďte změny.

    ![Ověření virtuálních počítačích](./media/site-recovery-vmm-to-azure-classic/vm-properties.png)


4. Na kartě **Konfigurovat** vlastnosti virtuálního počítače můžete změnit následující vlastnosti sítě.





- **Počet síťové adaptéry v cílovém virtuální počítači** – počet síťové adaptéry je dáno zadaná na počítači cílového virtuální velikost. Zaškrtněte políčko [virtuálního počítače velikost Specifikace](../virtual-machines/virtual-machines-linux-sizes.md#size-tables) počet adaptéry nepodporuje velikost virtuálního počítače. Když změníte velikost virtuálního počítače a uložte nastavení, se změní počet síťový adaptér, když otevřete stránku **Konfigurovat** při příštím. Počet síťové adaptéry cílové virtuálních počítačích je minimální počet síťové adaptéry na zdroj virtuálního počítače a maximální počet síťové adaptéry nepodporuje velikost virtuální počítač vybrali, následujícím způsobem:

    - Pokud argument číslo síťové adaptéry zdrojového počítače menší nebo rovna hodnotě počet adaptéry povolená velikost v počítači cílového, pak cíl bude mít stejný počet adaptéry jako zdroj.
    - Pokud počet adaptéry virtuálního počítače zdroj překročí povolený Cílová velikost a pak maximální velikost cílové bude použito počet.
    - Například pokud cílová velikost počítač podporuje čtyři zdrojového počítače obsahuje dva síťové adaptéry, cílový počítač bude mít dva adaptéry. Pokud zdrojového počítače obsahuje dva adaptéry ale podporované Cílová velikost podporuje pouze jednu cílový počítač bude mít jenom jeden adaptér.    

- **Síť virtuálního počítače cílové** - sítě, ke kterému virtuální počítač se připojí k je určený podle mapování sítě sítě zdroj virtuálního počítače. Pokud virtuální počítač zdroje obsahuje více než jeden síťový adaptér a zdroj sítě jsou namapované různých sítích podle plánu, je potřeba zvolit jednu cílové sítí.
- **Podsítě každý síťový adaptér** - pro každý síťový adaptér vyberete podsítě, ke kterému selhalo přes virtuální počítač by připojení k.
- **Cíl IP adresa** – Pokud síťový adaptér zdroj virtuálního počítače nakonfigurovaný tak, aby pomocí statické IP adresa zadejte IP adresu cílové virtuálního počítače a pak. Pomocí této funkce zachovat IP adresu počítače virtuální zdroje po přepojení. Pokud je k dispozici žádné IP adresu pak všechny dostupné IP adresa zůstane síťový adaptér v době překlopení. Pokud cílová IP adresa není zadán, ale již používá jiný virtuální počítač spuštěné v Azure převzetí nezdaří.  

    ![Úprava vlastností sítě](./media/site-recovery-vmm-to-azure-classic/multi-nic.png)

>[AZURE.NOTE] Linux virtuálních počítačích s statické IP adresy nejsou podporované.

## <a name="test-the-deployment"></a>Testování nasazení

Chcete-li otestovat nasazení můžete spustit přepojení test pro jeden počítač virtuální nebo vytvořit plán obnovení tvořené několika virtuálních počítačích a spusťte test přepojení plánu.  

Převzetí test napodobuje si převzetí a obnovení mechanismus izolovaná síť. Všimněte si, že:

- Pokud chcete připojit k počítači virtuální v Azure pomocí vzdálené plochy po záložní, povolte připojení ke vzdálené ploše počítače virtuální před spuštěním testovací překlopení.
- Po převzetí použijete veřejnou IP adresu pro připojení k virtuálního počítače v Azure pomocí vzdálené plochy. Pokud chcete, můžete to udělat, zajistěte, aby že se vám nezobrazuje žádné zásady domény, kvůli kterým jste připojení k virtuálního počítače pomocí veřejné adresy.

>[AZURE.NOTE] Nejlepší možný výkon zobrazí, když provedete přepojení Azure, zajistit nainstalované agenta Azure v zamknutém počítače. Pomáhají při spuštění rychleji a taky pomůže v diagnostice pro nečekané problémy. Linux agent může být nalezených [tady](https://github.com/Azure/WALinuxAgent) - a Windows agent najdete [tady](http://go.microsoft.com/fwlink/?LinkID=394789)

### <a name="create-a-recovery-plan"></a>Vytvoření plánu pro obnovení

1. Na kartě **Obnovení plány jednotného zasílání zpráv** přidáte nový plán. Zadejte název, **VMM** **Typ zdroje**a zdrojovému serveru VMM ve **zdroji**, cíl bude Azure.

    ![Vytvoření plánu pro obnovení](./media/site-recovery-vmm-to-azure-classic/recovery-plan1.png)

2. Na stránce **Vyberte virtuálních počítačích** vyberte virtuálních počítačích přidejte do plánu obnovení. Tyto virtuálních počítačích se přidají do výchozí skupiny plán obnovení – skupiny 1. Maximálně 100 virtuálních počítačích v plánu jednoho obnovení jste otestovali.

- Pokud chcete ověřit vlastnosti virtuálního počítače před přidáním do plánu, klikněte na virtuální počítač v okně vlastností stránky v cloudu, ve kterém je umístěn. Můžete taky nakonfigurujete vlastnosti virtuálního počítače v konzole VMM.
- Všechny virtuálních počítačích, které jsou zobrazeny mají povolené zámek. Seznam obsahuje obou virtuálních počítačích, kteří mají povolenou pro ochranu a počáteční replikace dokončila a ty, které jsou povolené pro ochranu s velkými replikace čeká na vyřízení. Pouze virtuálních počítačích s počáteční replikace dokončena můžete převzít jako součást plán obnovení.

    ![Vytvoření plánu pro obnovení](./media/site-recovery-vmm-to-azure-classic/select-rp.png)

Po jeho vytvoření plánu pro obnovení se zobrazí na kartě **Obnovení plány jednotného zasílání zpráv** . Můžete také přidat [runbooks Azure automatické](site-recovery-runbook-automation.md) obnovení plán k automatizaci akce při selhání.

### <a name="run-a-test-failover"></a>Spusťte test selhání

Existují dva způsoby spuštění selhání test Azure.

- **Testování převzetí bez Azure sítě**– tento typ test převzetí zkontroluje, aby virtuální počítač se zobrazí správně v Azure. Virtuální počítač nebudete připojení k Azure sítě po překlopení.
- **Testování převzetí s Azure sítě**– tento typ převzetí kontroly až podle očekávání pochází celý replikace prostředí a který selhání virtuálních počítačích bude připojen k cílové Azure sítě. Pro zpracování podsítě pro zkušební převzetí podsítě test virtuální počítač bude být sebou podle podsítě otevřené virtuálního počítače. Při je to různé běžná replikace podsítě otevřené virtuálního počítače je založená na podsítě virtuálního počítače zdroje.

Pokud chcete spustit přepojení test pro virtuální počítač povoleno pro ochranu Azure bez zadání Azure cílové síti nemusíte nic připravit. Spusťte test selhání s cílovou Azure sítě musíte vytvořit novou Azure síť, který má samostatný z Azure sítě (výchozí chování při vytváření novou síť v Azure). Podívejte se na [spusťte test selhání](site-recovery-failover.md#run-a-test-failover) další podrobnosti.


Taky musíte nastavit infrastrukturu pro replikovanou virtuální počítač očekávaným způsobem. Například virtuálního počítače s řadiče domény a DNS replikovat do Azure pomocí obnovení webu Azure a nelze vytvořit v síti test pomocí testování překlopení. Podívejte se na [testování důležité informace o převzetí služby active directory](site-recovery-active-directory.md#considerations-for-test-failover) v části Další podrobnosti.

Spuštění převzetí udělejte test následující:

1. Na kartě **Obnovení plány jednotného zasílání zpráv** vyberte plán a klikněte na **Testovat převzetí**.
2. Na stránce **Potvrďte převzetí Test** vyberte **žádný** konkrétní Azure síti.  Všimněte si, že pokud vyberete možnost žádný převzetí test kontroloval správně replikovat virtuálního počítače na Azure ale nekontroluje konfiguraci replikace sítě.

    ![Žádná síť](./media/site-recovery-vmm-to-azure-classic/test-no-network.png)

3. Pokud šifrování aktivované řešení v cloudu, v **Šifrovací klíč** vyberte certifikát, který byl vydán během instalace zprostředkovatele na serveru VMM, když je zapnutá možnost povolit šifrování dat pro obláčkem.
4. Na kartě **úlohy** můžete sledovat průběh překlopení. By měly být také neuvidíte otevřené test virtuálního počítače na portálu Azure. Pokud jste nastavili přístup virtuálních počítačích z místní síti můžete spustit připojení ke vzdálené ploše virtuálního počítače.
5. Fáze **testování dokončeno** dosáhne záložní kliknutím na tlačítko skončí zkušební převzetí **Dokončení otestovat** . Můžete procházet hierarchii na kartě **projektu** sledovat průběh převzetí a stav a proveďte akce, které jsou potřebné.
6. Po převzetí si budete moct zobrazit otevřené test virtuálního počítače na portálu Azure. Pokud jste nastavili přístup virtuálních počítačích z místní síti můžete spustit připojení ke vzdálené ploše virtuálního počítače. Postupujte takto:

    1. Ověřte, že virtuálních počítačích aplikace úspěšně spuštěna.
    2. Pokud chcete připojit k počítači virtuální v Azure pomocí vzdálené plochy po záložní, povolte připojení ke vzdálené ploše počítače virtuální před spuštěním testovací překlopení. Budete taky musíte přidat koncový bod RDP v počítači virtual. Můžete využít [Azure automatizaci Runbooks](site-recovery-runbook-automation.md) to udělat.
    3. Po převzetí Pokud používáte veřejnou IP adresu pro připojení k virtuálního počítače v Azure pomocí vzdálené plochy, ujistěte se, nemáte žádné zásady domény, kvůli kterým jste připojení k virtuálního počítače pomocí veřejné adresy.

7.  Po dokončení testování takto:
    - Klikněte na **dokončení testu překlopení**. Vyčistěte testovacím prostředí automaticky vypněte a odstraňovat test virtuálních počítačích.
    - Kliknutím na **poznámky** můžete zaznamenat a uložit všechny připomínky přidružený k převzetí testu.

>

## <a name="next-steps"></a>Další kroky

Informace o [nastavování obnovení plány](site-recovery-create-recovery-plans.md) a [překlopení](site-recovery-failover.md).
