<properties
    pageTitle="Replikovat Hyper-V virtuálních počítačích v VMM mračnech sekundární web VMM | Microsoft Azure"
    description="Tento článek popisuje, jak replikovat Hyper-V VMs v VMM mračnech sekundární VMM web s obnovení webu Azure."
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

# <a name="replicate-hyper-v-virtual-machines-in-vmm-clouds-to-a-secondary-vmm-site"></a>Replikovat Hyper-V virtuálních počítačích v VMM mračnech sekundárním VMM Web

> [AZURE.SELECTOR]
- [Azure portálu](site-recovery-vmm-to-vmm.md)
- [Klasický portálu](site-recovery-vmm-to-vmm-classic.md)
- [Prostředí PowerShell – správce](site-recovery-vmm-to-vmm-powershell-resource-manager.md)

Obnovení webu Azure služby přispívá k strategie firmy kontinuitu a havárie obnovení (BCDR) tak, že orchestrating replikace, převzetí a obnovení virtuálních počítačích a fyzické servery. Počítačích replikovat Azure nebo vedlejší místního datovém centru. Rychlý přehled číst [Co je obnovení webu Azure?](site-recovery-overview.md)

## <a name="overview"></a>Základní informace

Tento článek popisuje, jak replikovat Hyper-V virtuálních počítačích na serverech hostitele Hyper-V spravovaných na VMM mračnech k sekundárním VMM webu pomocí účtu obnovení webu Azure.

Tento článek obsahuje požadavky, se dozvíte, jak nastavení trezoru obnovení webu, instalace zprostředkovatele obnovení webů Azure na zdroj a obrázku VMM serverů, zaregistrujte servery v trezoru, nastavení ochrany VMM mračnech a pak ji povolit ochranu VMs Hyper-V. Dokončete nastavení testováním převzetí aby zkontrolovala, jestli že všechno funguje očekávaným způsobem.

V dolní části tohoto článku nebo na [Fórum služby Azure obnovení](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr)pokládat komentáře nebo dotazy.

## <a name="architecture"></a>Architektura

Na následujícím obrázku komunikačním kanály a porty používané obnovení webu Azure průběhu a replikace

![Topologie e2e](./media/site-recovery-vmm-to-vmm-classic/e2e-topology.png)

## <a name="before-you-start"></a>Než začnete

Zkontrolujte, jestli že budete mít tyto požadavky na místě:

**Zjistit předpoklady pro** | **Podrobnosti**
--- | ---
**Azure**| Je třeba účet [Microsoft Azure](https://azure.microsoft.com/) . Můžete začít s [bezplatnou zkušební verzi](https://azure.microsoft.com/pricing/free-trial/). [Další informace](https://azure.microsoft.com/pricing/details/site-recovery/) o obnovení webu ceny.
**VMM** | Potřebujete alespoň jeden VMM server.<br/><br/>VMM server by měla běžet aspoň System Center 2012 SP1 s nejnovějšími kumulativními aktualizacemi.<br/><br/>Pokud chcete nastavení ochrany s jedním serverem VMM, musíte aspoň dva mračnech nakonfigurovaný na serveru.<br/><br/>Pokud chcete nasadit ochranu se dvěma VMM servery, každý server musí mít aspoň jeden cloudu nakonfigurovaný na primární VMM serveru, který chcete zamknout a jeden cloudu nakonfigurovaný na vedlejší VMM serveru, který chcete použít pro ochranu a obnovení<br/><br/>Všechny shluky VMM musí profil Hyper-V možnost nastavení.<br/><br/>Cloudové zdroje, které chcete zamknout musí obsahovat jednu nebo více skupin VMM Host (hostitel).<br/><br/>Další informace o nastavení VMM mračnech v [Návod: Vytvoření soukromého mračnech s System Center 2012 SP1 VMM](http://blogs.technet.com/b/keithmayer/archive/2013/04/18/walkthrough-creating-private-clouds-with-system-center-2012-sp1-virtual-machine-manager-build-your-private-cloud-in-a-month.aspx) na Dryml Mayer blogu.
**Hyper-V** | Potřebujete jednu Hyper-V hostitelské servery ve skupinách hostitele VMM primárních a sekundárních a virtuálních počítačích na všech serverech hostitele Hyper-V.<br/><br/>Host (hostitel) a cílové servery Hyper-V musí běžet aspoň Windows Server 2012 s rolí Hyper-V a nainstalovali nejnovější aktualizace.<br/><br/>Všechny Hyper-V server obsahující VMs chcete zamknout musí být umístěné v cloudu VMM.<br/><br/>Pokud používáte Hyper-V clusteru, Poznámka: Tento zprostředkovatel obrázku není automaticky vytvoří, pokud máte statické IP adresy clusteru založený. Budete muset ručně nakonfigurovat zprostředkovatele obrázku. [Další informace](https://www.petri.com/use-hyper-v-replica-broker-prepare-host-clusters) v Aidan Finn blogu.
**Mapování sítě** | Můžete nakonfigurovat mapování sítě a ujistěte se, že replikovanou virtuálních počítačích optimálně umístěn na vedlejší serverech hostitele Hyper-V po převzetí a, můžete připojit k odpovídající OM sítím. Pokud nechcete konfigurovat mapování sítě, otevřené VMs nebudete připojení k jakákoli síť po překlopení.<br/><br/>Pokud chcete nastavit mapování sítě během nasazení, ujistěte se, že virtuálních počítačích na hostitelském serveru zdroj Hyper-V jsou připojené k síti VMM OM. Síť by měl být propojené s logická síť, která je přidružená k cloud. < br /<br/>Cílové cloudu na vedlejší VMM serveru, který používáte pro obnovení by měl mít odpovídající sítě OM nakonfigurované a ho zase by měl být propojené s odpovídající logická síť, která je přidružená k cílové cloudu.<br/><br/>[Další informace](site-recovery-network-mapping.md) o mapování sítě.
**Mapování úložiště** | Ve výchozím nastavení při replikovat virtuálního počítače na hostitelském serveru Hyper-V zdroj k serveru hostitele Hyper-V cílové replikovanou data jsou uložena ve výchozím umístění, který je označený cílového hostitele Hyper-V ve Správci Hyper-V. Větší kontrolu nad kam se ukládají replikovanou data můžete nakonfigurovat úložiště mapování<br/><br/> Abyste mohli nakonfigurovat úložiště mapování, potřebujete nastavení klasifikace úložiště ve zdroji a cílového VMM servery, než začnete nasazení. [Další informace](site-recovery-storage-mapping.md).


## <a name="step-1-create-a-site-recovery-vault"></a>Krok 1: Vytvoření trezoru obnovení webu

1. Přihlaste se k [Portálu Správa](https://portal.azure.com) z VMM server, ke kterému se chcete zaregistrovat.

2. Rozbalte položku **Datové služby** > **Obnovení služby** a klikněte na **Webu obnovení trezoru**.

3. Klikněte na **vytvořit nový** > **vytvořit**.

4. Do pole **název**zadejte popisný název k identifikaci trezoru.

5. V **oblasti** vyberte zeměpisná oblast pro trezoru. Kontrola podporovaných oblastí najdete v článku zeměpisné dostupnost v [Azure webu obnovení ceny podrobností](http://go.microsoft.com/fwlink/?LinkId=389880).

6. Klikněte na **vytvořit trezoru**.

    ![Vytvoření trezoru](./media/site-recovery-vmm-to-vmm-classic/create-vault.png)

Vrácení se změnami na stavovém řádku, aby byl vytvořen trezoru. Trezoru se zobrazí jako **aktivní** na hlavní stránce obnovení služby.

## <a name="step-2-generate-a-vault-registration-key"></a>Krok 2: Vygenerování trezoru registračního klíče

Generování registračního klíče v trezoru. Po stažení poskytovatel obnovení webu Azure a nainstalujte ji na VMM server použijete tento klíč k registraci VMM serveru v trezoru.

1. Na stránce **Služby Recovery** na trezoru a otevře se stránka rychlý Start. Představení aplikace můžete otevřít také kdykoli pomocí ikony.

    ![Ikona rychlým startem](./media/site-recovery-vmm-to-vmm-classic/quick-start-icon.png)

2. V rozevíracím seznamu vyberte **mezi dvěma místní VMM weby**.
3. V části **Připravit servery VMM**klikněte na **Generovat registrace klíčové soubor**. Soubor s klíčem je automaticky generované a platí 5 dní po vygenerování. Pokud nepoužíváte přístup k portálu Azure ze serveru VMM musíte zkopírovat tohoto souboru na server.

    ![Registrační klíč](./media/site-recovery-vmm-to-vmm-classic/register-key.png)

## <a name="step-3-install-the-azure-site-recovery-provider"></a>Krok 3: Instalace zprostředkovatele obnovení Azure webů

4. Na stránce **Rychlý Start** v **VMM připraví servery**klikněte na **Stáhnout Microsoft Azure obnovení poskytovatel webu pro instalaci na serverech VMM** nejnovější verzi souboru instalace zprostředkovatele.

2. Spusťte tento soubor na serveru VMM zdroje.

    >[AZURE.NOTE] Pokud VMM nasazenou v clusteru a instalujete poskytovatele první nainstalovat na aktivní uzel a dokončete instalaci registrace serveru VMM v trezoru. V jednotlivých uzlech nainstalujte na poskytovatele. Poznámka: Pokud upgradujete na poskytovatele budete muset upgradovat na všech uzlech, protože se mají všechny běžet ve stejné verzi poskytovatele.

3. Instalační program má několik **Zaškrtněte políčko před požadavky** a žádosti o oprávnění zastavit službu VMM zahájíte instalace zprostředkovatele. Po dokončení instalace je bude třeba restartovat službu VMM automaticky. Pokud instalujete clusteru VMM budete výzva k ukončení roli obrázku.

4. Ve **Službě Microsoft Update** můžete se rozhodnout v aktualizací. Při tomto nastavení povolené poskytovatele podle předpisů rozhraní Microsoft Update zásad bude instalovat aktualizace.

    ![Aktualizace společnosti Microsoft](./media/site-recovery-vmm-to-vmm-classic/ms-update.png)

5. Umístění instalace nastavena na ** <SystemDrive>\Program Files\Microsoft System Center 2012 R2\Virtual počítače Manager\bin**. Klikněte na tlačítko nainstalovat, aby se spustila instalace zprostředkovatele.

    ![InstallLocation](./media/site-recovery-vmm-to-vmm-classic/install-location.png)

6. Po dokončení instalace zprostředkovatele klikněte na **zaregistrovat** k registraci serveru v trezoru.

    ![InstallComplete](./media/site-recovery-vmm-to-vmm-classic/install-complete.png)
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


8. **Registrační klíč**vyberte klíč, který jste si stáhli z obnovení webu Azure a zkopírují do VMM serveru.


10.  Nastavení šifrování se používá pouze když jste replikace Hyper-V VMs v VMM mračnech Azure. Pokud jste replikace sekundární web nepoužívá.

11.  V poli **název serveru**zadejte popisný název k identifikaci VMM serveru v trezoru. V konfiguraci clusteru zadejte název VMM clusteru role.
12.  V poli vyberte **synchronizovat cloudu metadat** jestli chcete k synchronizaci metadat pro všechny shluky na serveru VMM s trezoru. Tato akce jenom s jednou na všech serverech musí. Pokud nechcete synchronizovat všechny shluky, můžete nechat toto nastavení zaškrtnuté a synchronizovat každý cloudu jednotlivě ve vlastnostech cloudu v konzole VMM.

13.  Klikněte na **Další** a dokončit. Po registraci metadata ze serveru VMM načítané obnovení webu Azure. Server je zobrazen v **VMM serverů** > **serverů** v trezoru.

    ![Serverů](./media/site-recovery-vmm-to-vmm-classic/provider13.PNG)

### <a name="command-line-installation"></a>Přepínač příkazového řádku instalace

Obnovení poskytovatel webu Azure lze také nainstalovat z příkazového řádku. Tato metoda slouží k instalaci zprostředkovatele na serveru CORE pro Windows Server 2012 R2.

1. Stáhněte instalační soubor a registrace klíč poskytovatele do složky. Dejme tomu C:\ASR.
2. Zastavit službu System Center Virtual Machine Manager
3. Extrahování instalační program poskytovatele spuštěním tyto příkazy z příkazového řádku s oprávněními **Správce** :

        C:\Windows\System32> CD C:\ASR
        C:\ASR> AzureSiteRecoveryProvider.exe /x:. /q

4. Instalace zprostředkovatele spuštěním:

        C:\ASR> setupdr.exe /i

5. Zprostředkovatel zaregistrujte spuštěním:

        CD C:\Program Files\Microsoft System Center 2012 R2\Virtual Machine Manager\bin
        C:\Program Files\Microsoft System Center 2012 R2\Virtual Machine Manager\bin\> DRConfigurator.exe /r  /Friendlyname <friendly name of the server> /Credentials <path of the credentials file> /EncryptionEnabled <full file name to save the encryption certificate>     

Kde jsou parametry:

 - **/Credentials**: povinný parametr, který určuje umístění, ve kterém je umístěn soubor klíčové registrace  
 - **/FriendlyName**: povinný parametr název serveru hostitele Hyper-V, která se zobrazí v portálu obnovení webu Azure.
 - **/EncryptionEnabled**: volitelný parametr, který je potřeba použít pouze v VMM Azure scénář v případě potřeby šifrování virtuálních počítačích, na v klidu v Azure. Ověřte, zda, název souboru, který zadáte s příponou **.pfx** .
 - **/proxyAddress**: volitelný parametr, který určuje adresy proxy serveru.
 - **/proxyport**: volitelný parametr, který určuje port proxy serveru.
 - **/proxyUsername**: volitelný parametr, který určuje Proxy uživatelské jméno (pokud proxy server vyžaduje ověření).
 - **/proxyPassword**: volitelný parametr, který určuje heslo pro ověřování proxy serveru (pokud proxy server vyžaduje ověření).  

## <a name="step-4-configure-cloud-protection-settings"></a>Krok 4: Nakonfigurujte cloudu nastavení ochrany

Po VMM servery jsou registrovány, můžete nakonfigurovat nastavení ochrany cloudu. Pokud tuto možnost **synchronizovat data cloudu s trezoru** povolili, když jste nainstalovali zprostředkovatele tak všechny shluky na serveru VMM se zobrazí na kartě **Chráněné položky** v trezoru. Pokud jste neměli se můžou synchronizovat konkrétní cloudu s obnovení webu Azure na kartě **Obecné** na stránce Vlastnosti cloudu v konzole VMM.

![Publikované cloudu](./media/site-recovery-vmm-to-vmm-classic/clouds-list.png)

1. Na stránce Snadné spuštění klikněte na **Nastavení ochrany VMM mračnech**.
2. Na kartě **VMM mračnech** vyberte cloudu, kterou chcete konfigurovat a přejděte na kartu **Konfigurace** .
3. V **cílové**vyberte **VMM**.
4. V **cílové umístění**vyberte na místě VMM server, který má na starosti cloudu, který chcete použít k obnovení.
4. V **cílové cloudu**vyberte cílovou cloudu, kterou chcete použít pro překlopení virtuálních počítačích v cloudu zdroje. Všimněte si, že:

    - Doporučujeme, abyste vybrali cílové obláčkem, která vyhovuje požadavkům pro obnovení na virtuálních počítačích, které budete zamknout.
    - Shluk můžete přiřadit pouze pár jednoho cloudu – buď jako primární nebo cílové cloudu.

5. Do pole **Kopírovat četnost**, určit, jak často data by měla být synchronizovat mezi 5he zdrojové a cílové umístění. Všimněte si, že toto nastavení platí jenom při hostiteli Hyper-V běží Windows Server 2012 R2. U jiných serverů bude použita výchozí nastavení pět minut.
6. V **Další obnovení body**určete, jestli chcete vytvořit další obnovení body. Výchozí nulové hodnoty označuje, že jenom nejnovější bod obnovení primární virtuálního počítače se ukládají na server hostitele otevřené. Povolení více bodů obnovení vyžaduje další úložiště k snímky, které jsou uložené v jednotlivých bodech obnovení poznámku. Ve výchozím nastavení obnovení body se vytvářejí každou hodinu tak, aby jednotlivých bodech obnovení obsahuje za hodinu hodnotě data. Hodnota bodu obnovení, který přiřadíte virtuálního počítače v konzole VMM nesmí být menší než hodnota, kterou jste přiřadili v konzole obnovení webu Azure.
7. V **Četnost konzistenci aplikací snímky**zadejte, jak často se má vytvořit konzistenci aplikací snímky. Pro Hyper-V použití dva typy snímků – standardní snímek, který obsahuje dílčí snímek celý virtuálního počítače a konzistenci aplikací snímek, který vytvoří v okamžiku snímek dat aplikace do virtuálního počítače. Aplikace konzistentní snímky umožňuje zajistit, aby aplikace do konzistentního stavu po snímku, se považuje hlasitost stín kopie služby (VSS). Všimněte si, že pokud povolíte konzistenci aplikací snímky, bude to mít vliv výkonu aplikacím na zdroj virtuálních počítačích. Zajistěte, aby byl hodnotu, kterou nastavíte menší než počet bodů další obnovení, které jste nakonfigurovali.

    ![Konfigurace nastavení ochrany](./media/site-recovery-vmm-to-vmm-classic/cloud-settings.png)

8. V **přenosu komprese dat**určete, zda by měly replikovanou data, která jsou přenášena komprimovány.
9. V **ověřování**zadejte, jak ověření přenosů mezi servery hostitele Hyper-V primární a obnovení. Pokud máte pracovní prostředí protokol Kerberos nakonfigurovali, vyberte HTTPS. Obnovení Azure webu automaticky nakonfiguruje certifikáty pro ověřování HTTPS. Není nutná žádná ruční konfigurace. Pokud vyberete pomocí protokolu Kerberos, lístek protokolu Kerberos se použije pro vzájemné ověřování serverů Host (hostitel). Ve výchozím nastavení se otevře v bráně Firewall systému Windows na serverech hostitele Hyper-V port 8083 a 8084 (pro certifikáty). Všimněte si, že toto nastavení platí jenom pro Hyper-V hostiteli serverech běží v systému Windows Server 2012 R2.
10. V poli **Port**změňte číslo portu, podle kterého hostitele počítačů zdrojové a cílové poslouchat replikace. Pokud chcete použít službu (QoS) sítě omezení šířky pásma pro replikace například může upravit nastavení. Zkontrolujte, že port nepoužívá jakékoli jiné aplikace a zda je otevřen v nastavení brány firewall.
11. Ve skupinovém rámečku **metodu replikace**určete způsob počáteční replikace data ze zdroje do cílové umístění bude zpracování, před spuštěním běžná replikace:

    - **Přes síť**– kopírování dat v síti může být časově náročný a prostředky. Doporučujeme tuto možnost použijte, pokud cloudu obsahuje virtuálních počítačích s relativně malým virtuální pevných discích a primární webu je připojený sekundární web připojení k širokou šířka pásma. Můžete zadat, že by měl výtisku začít okamžitě, nebo vyberte čas. Pokud používáte replikace sítě, doporučujeme naplánovat dobu největšího.
    - **Offline**– tato metoda určuje, že počáteční replikace proběhne pomocí externí média. To je užitečné, pokud chcete zabránit snížení výkonu sítě nebo pro geograficky vzdáleném umístění. Tímto způsobem zadejte umístění pro export v cloudu zdroje a import umístění v cloudu cílové. Po povolení ochrany virtuálního počítače virtuální pevný disk zkopírována do umístění zadaný exportu. Odešlete cílovém webu a jeho zkopírování do umístění importu. Systém zkopíruje importované informace na virtuálních počítačích otevřené.

12. Vyberte **Odstranit otevřené virtuálního počítače** můžete určit, zda chcete-li zrušit ochrana virtuální počítač tak, že vyberete možnost **Odstranit ochranu virtuálního počítače** na kartě virtuálních počítačích vlastnosti cloudu možné odstranit otevřené virtuálního počítače. Toto nastavení povoleno, při vypnutí možnosti ochrany virtuální počítač odebrali Azure webu obnovení nastavení obnovení webu pro virtuální počítač budou odebrány konzole VMM a otevřené odstraněna.

    ![Konfigurace nastavení ochrany](./media/site-recovery-vmm-to-vmm-classic/cloud-settings-replica.png)

Po uložení nastavení úlohy se vytvoří a můžete sledovat na kartě **projekty** . Všech serverů hostitele Hyper-V v cloudu zdroj VMM bude nakonfigurované replikace. Na kartě **Konfigurovat** lze upravovat nastavení cloudu. Pokud chcete změnit cílové umístění nebo cílové cloudu musíte odebrat konfiguraci cloudu a překonfigurovat cloudu.

### <a name="prepare-for-offline-initial-replication"></a>Příprava na offline počáteční replikace

Musíte provést následující akce a připravit počáteční replikace v offline režimu:

- Na zdrojovém serveru zadáte umístění, ze které bude export dat proběhnout. Přiřadíte oprávnění NTFS a sdílení ke službě VMM na cestě pro export úplné řízení. Na cílovém serveru zadáte umístění, ze kterého dojde k importu dat. Přiřazení stejná oprávnění na cesta k importu.
- Pokud je sdílený import nebo export cestu, přiřaďte správce, Power uživatele, tisk operátor nebo operátor serveru členství ve skupinách VMM účtu služby ve vzdáleném počítači, na kterém sdíleného nachází.
- Pokud používáte všechny spustit jako účty, které pokud chcete přidat hosts na cestách import a export přiřazení pro čtení a zápis spustit jako v seznamu účty VMM.
- Import a export akcie neměli nachází na kterýkoliv počítač používá jako host server Hyper-V, protože smyčky konfigurace neposkytuje podporu pro Hyper-V.
- Ve službě Active Directory v jednotlivých Hyper-V vynucené serveru, který obsahuje virtuálních počítačích, které chcete zamknout, povolení a konfigurace delegování s informacemi o důvěryhodnosti vzdálených počítačů, na kterých import a export cesty nacházejí, následujícím způsobem:
    1. U řadiče domény otevřete **uživatele služby Active Directory a počítačů**.
    2. Ve stromové nabídce klikněte na **název_domény** > **počítačů**.
    3. Klikněte pravým tlačítkem na název serveru hostitele Hyper-V > **Vlastnosti**.
    4. Na kartě **delegování** klikněte na T**koroze tento počítač pro delegování určeným službám**.
    5. Klikněte na tlačítko **použít libovolný protokol pro ověřování**.
    6. Klikněte na tlačítko **Přidat** > **uživatelů a počítačů**.
    7. Zadejte název počítače hostujícího cestě pro export > **OK**. Ze seznamu dostupných služeb, podržte stisknutou klávesu CTRL a klikněte na **cifs** > **OK**. Opakujte pro název počítače hostujícího cesta importu. Opakujte podle potřeby pro další Hyper-V hostitelské servery.

## <a name="step-5-configure-network-mapping"></a>Krok 5: Konfigurace mapování sítě
1. Na stránce Snadné spuštění klikněte na **mapovat sítě**.
2. Vyberte zdrojový server VMM ze které chcete propojit sítě a potom do kterého sítí namapované cílový VMM server. Zobrazí seznam zdroj sítě a jeho přidružený cílový sítě. Zobrazí se prázdná buňka sítích, které nejsou namapované aktuálně.
3. Vyberte síť v **síti na zdroj** > **Mapa**. Služba zjistí sítí OM na cílovém serveru a zobrazí jejich. Klikněte na informační ikonu u názvů sítě zdrojové a cílové zobrazení podsítí pro každou síť.

    ![Konfigurace mapování sítě](./media/site-recovery-vmm-to-vmm-classic/network-mapping1.png)

4. V dialogovém okně vyberte jeden sítí OM cílový VMM server.

    ![Vyberte cílový síť](./media/site-recovery-vmm-to-vmm-classic/network-mapping2.png)

5. Po výběru cílové síti se zobrazují chráněné mračnech používající Zdrojová síť. Zobrazí se také k dispozici cílové sítí, které souvisí s mračnech pro ochranu. Doporučujeme vybrat cílové síti, která je dostupná pro všechny shluky, které používáte pro ochranu. Nebo můžete přejít na server VMM a upravit vlastnosti cloudu, které chcete přidat logické síť odpovídající OM sítě, ke které chcete vybrat.
6. Klikněte na značku zaškrtnutí dokončete proces mapování. Úlohy začne sledovat průběh mapování. Můžete ji zobrazit na kartě **projekty** .


## <a name="step-6-configure-storage-mapping"></a>Krok 6: Konfigurace mapování úložiště
Ve výchozím nastavení při replikovat virtuálního počítače na hostitelském serveru Hyper-V zdroj k serveru hostitele Hyper-V cílové replikovanou data jsou uložena ve výchozím umístění, který je označený cílového hostitele Hyper-V ve Správci Hyper-V. Větší kontrolu nad kam se ukládají replikovanou data můžete nakonfigurovat úložiště mapování následujícím způsobem:



1. Definování klasifikace úložiště na serverech VMM zdrojové a cílové. [Další informace](https://technet.microsoft.com/library/gg610685.aspx). Klasifikace musí být k dispozici pro Hyper-V hostitelské servery ve zdrojové a cílové mračnech. Klasifikace nemusíte mít stejný druh úložiště. Můžete třeba namapovat klasifikace zdroje, která obsahuje sdílené položky SMB pro cílové klasifikace, která obsahuje CSVs.
2. Po klasifikace na místě je vytvořit mapování. Uděláte to takto: na stránce **Rychlý Start** > **Mapa úložiště**.
3. Klikněte na kartu **úložiště** > **Mapa klasifikace úložiště**.
4. Na kartě **mapování úložiště klasifikace** vyberte klasifikace ve zdroji a cílového webu VMM servery. Uložte nastavení.

    ![Vyberte cílový síť](./media/site-recovery-vmm-to-vmm-classic/storage-mapping.png)


## <a name="step-7-enable-virtual-machine-protection"></a>Krok 7: Povolit ochranu virtuálního počítače
Po dokončení serverů, mračnech a sítí konfigurace správně, můžete povolit ochranu virtuálních počítačích v cloudu.

1. Na kartě **virtuálních počítačích** v cloudu, ve kterém se nachází virtuálního počítače klikněte na **Povolit ochranu** > **Přidat virtuálních počítačích**.
2. V seznamu virtuálních počítačích v cloudu vyberte tu, kterou chcete zamknout.

    ![Povolení ochrany virtuálního počítače](./media/site-recovery-vmm-to-vmm-classic/enable-protection.png)

3. Sledování průběhu povolit ochranu akci na kartě **projekty** , včetně počáteční replikace. Po dlouhodobě spuštěná úloha dokončení ochrany je připraven k převzetí virtuálního počítače. Když je povolena ochrana a replikovat virtuálních počítačích, budete moct tyto informace zobrazit v Azure.

    ![Virtuální počítač ochranu projektu](./media/site-recovery-vmm-to-vmm-classic/vm-jobs.png)

>[AZURE.NOTE] Můžete taky povolit ochranu virtuálních počítačích v konzole VMM. Klikněte na **Povolit ochranu** na panelu nástrojů na kartě **Obnovení webu Azure** ve vlastnosti virtuálního počítače.

### <a name="on-board-existing-virtual-machines"></a>Integrovaný existující virtuálních počítačích

Pokud máte existující virtuálních počítačích v VMM, které jsou replikace Hyper-V otevřené musíte integrovaný jejich obnovení webu Azure ochranu následujícím způsobem:

1. Zkontrolujte, jestli že máte primárních a sekundárních mračnech. Ujistěte se, že server pro Hyper-V hostování existující virtuálního počítače je umístěné v cloudu, primární a serveru pro Hyper-V hostování otevřené virtuálního počítače je umístěn v sekundární cloudu. Ujistěte se, máte nastavený nastavení ochrany pro shluky. Nastavení by měly odpovídat můžou být konfigurovaná pro Hyper-V otevřené. Replikace virtuální počítač nemusí fungovat jinak podle očekávání.
2. Povolte ochranu primární virtuálního počítače. Azure obnovení webu a VMM zajistí, že ke zjištění stejného hostitele otevřené a virtuálního počítače a obnovení webu Azure se znovu použít a obnovit replikace pomocí nastavením během konfigurace cloudu.


## <a name="test-your-deployment"></a>Testování nasazení

Chcete-li otestovat nasazení můžete spustit přepojení test pro jeden počítač virtuální nebo vytvořit plán obnovení tvořené několika virtuálních počítačích a spusťte test přepojení plánu.  Převzetí test napodobuje si převzetí a obnovení mechanismus izolovaná síť.

### <a name="create-a-recovery-plan"></a>Vytvoření plánu pro obnovení

1. Na kartě **Obnovení plány jednotného zasílání zpráv** klikněte na možnost **Vytvořit plán obnovení**.
2. Zadejte název pro obnovení plán a zdrojové a cílové VMM servery. Zdrojovému serveru musí mít virtuálních počítačích, kteří mají povolenou převzetí a obnovení. Vyberte **Pro Hyper-V** zobrazíte jenom mračnech nakonfigurované pro Hyper-V replikace.

    ![Vytvoření plánu pro obnovení](./media/site-recovery-vmm-to-vmm-classic/recovery-plan1.png)

3. V **Vyberte virtuálního počítače**vyberte replikace skupiny. Všechny virtuálních počítačích přidružené ke skupině replikace bude vybraná a přidá do plánu obnovy. Tyto virtuálních počítačích se přidají do výchozí skupiny plán obnovení – skupiny 1. v případě potřeby můžete přidat další skupiny. Všimněte si, že po replikace virtuálních počítačích se otevřou v souladu s pořadí skupin plán obnovení.

    ![Přidání virtuálních počítačích](./media/site-recovery-vmm-to-vmm-classic/recovery-plan2.png)

Po obnovení plán byl vytvořen, se zobrazí v seznamu na kartě **Obnovení plány jednotného zasílání zpráv** .

###<a name="run-a-test-failover"></a>Spusťte test selhání

1. Na kartě **Obnovení plány jednotného zasílání zpráv** vyberte plán a klikněte na **Testovat převzetí**.
2. Na stránce **Potvrďte převzetí Test** vyberte **žádné**. Všimněte si, že vyberete tuto možnost povolit selhalo přes otevřené virtuálních počítačích nesmí být připojeni k jakákoli síť. Tím otestujete virtuální počítač selhání podle očekávání že testování prostředí replikace sítě. Podívejte se na postup [spusťte test přepojení](site-recovery-failover.md#run-a-test-failover) podrobné informace o tom, jak použít různé možnosti sítě.
3. Test virtuálního počítače se vytvoří ve stejném hostiteli jako host, na kterém se nachází otevřené virtuálního počítače. Pole bude přidáno do stejné cloudu, ve kterém se nachází otevřené virtuálního počítače.

### <a name="run-a-recovery-plan"></a>Spuštění plánu obnovy
Po replikace otevřené virtuální počítač nemusí mít IP adresu, která je stejná jako IP adresu primární virtuálního počítače. Virtuálních počítačích se aktualizuje serveru DNS, který používá po spuštění. Můžete také přidat skript má aktualizovat Server DNS zajistit včasná aktualizace.

#### <a name="script-to-retrieve-the-ip-address"></a>Skript k načtení IP adresa
Spusťte tento ukázkový skript k načtení IP adresu.

        $vm = Get-SCVirtualMachine -Name <VM_NAME>
        $na = $vm[0].VirtualNetworkAdapters>
        $ip = Get-SCIPAddress -GrantToObjectID $na[0].id
        $ip.address  

#### <a name="script-to-update-dns"></a>Skript k aktualizaci služby DNS
Spusťte tento ukázkový skript aktualizovat DNS zadání adresy IP se obnovená pomocí předchozího ukázka skriptu.

        string]$Zone,
        [string]$name,
        [string]$IP
        )
        $Record = Get-DnsServerResourceRecord -ZoneName $zone -Name $name
        $newrecord = $record.clone()
        $newrecord.RecordData[0].IPv4Address  =  $IP
        Set-DnsServerResourceRecord -zonename $zone -OldInputObject $record -NewInputObject $Newrecord



## <a name="privacy-information-for-site-recovery"></a>Osobní údaje pro obnovení webu

Tato část obsahuje další osobní informace o obnovení webu Microsoft Azure služby (""). Zásady ochrany osobních údajů pro služby Microsoft Azure naleznete v tématu [Microsoft Azure zásady ochrany osobních údajů](http://go.microsoft.com/fwlink/?LinkId=324899)

**Funkce: registrace**

- **Akce**: zaregistruje serveru se službou, takže můžete být chráněny virtuálních počítačích
- **Shromažďované**: po registraci služby shromažďuje, zpracuje a správa certifikátů informace ze serveru VMM, která obsahuje určený k poskytování havárie obnovení s použitím služby název VMM serveru a název mraků virtuálního počítače na VMM server.
- **Použití informací**:
    - Osvědčení o řízení – slouží k určení a ověření registrovaných VMM serveru pro přístup ke službě. Služba používá k zabezpečení token registrovaných serveru VMM můžete získat přístup k veřejné klíčové části certifikát. Serveru musí používat tento token získat přístup k funkcím služby.
    - Název serveru VMM – název serveru VMM požaduje pro její identifikovat a odpovídající serverem VMM nacházejí shluky komunikovat.
    - Jména ze serveru VMM v cloudu – je nutné zadat název cloudu, při používání služby cloudu párování/unpairing funkce píše níže. Pokud se rozhodnete spárování cloudu z primární datovém centru s jinou cloudu v datovém centru obnovení, jsou uvedeny názvy mraků všechny v datovém centru obnovení.

- **Volba**: tyto informace jsou základní součást procesu registrace služby vzhledem k tomu, že je dobré je a službách identifikovat VMM server, u kterého chcete ochrany obnovení webu Azure, stejně jako identifikovat správnému registrovaných VMM serveru. Pokud nechcete odeslat tyto informace ke službě, se nepoužívá tuto službu. Pokud registrovat váš server a potom později chtít unregister ho můžete provedete tak, že odstraníte informace o serveru VMM z portálu služby (což je portál Azure).

**Funkce: Povolit ochranu obnovení webu Azure**

- **Akce**: Azure webu obnovení poskytovatel nainstalovat na VMM server je přenos komunikace se službou. Zprostředkovatel je dynamické knihovny (DLL) hostované v procesu VMM. Po instalaci na poskytovatele získá v konzole pro správu VMM povolené funkce "Datacentra obnovení". Vlastnost s názvem "Datacentra obnovení" se můžete pomoci chrá virtuálního počítače lze povolit virtuálních počítačích v obláčkem všechny nové nebo existující. Po nastavení této vlastnosti odešle zprostředkovatel jméno a ID virtuálního počítače ke službě. Virtuální ochrana je povolena technologii, Windows Server 2012 nebo Windows Server 2012 R2 Hyper-V replikace. Virtuální počítač data získá replikovat z jednoho hostitele Hyper-V do druhého (obvykle umístěné v datovém centru různých "využití").

- **Shromažďované**: službu shromažďuje zpracuje a přenáší metadat pro virtuální počítač, který obsahuje název, ID, virtuální sítě a název cloudu, ke které patří.

- **Použití informací**: službu používá výše uvedených informací k naplnění informací virtuálního počítače na portálu služby.

- **Volba**: Toto je důležitou součástí službu a nelze vypnout. Pokud nechcete, aby tato informace odesílané do služby, není zamknout obnovení webu Azure pro všechny virtuálních počítačích. Všimněte si, že všechna data odeslána poskytovatelem služby odesílaná HTTPS.

**Funkci: Plán obnovy**

- **Akce**: Tato funkce umožňuje sestavit plán průběhu datového centra "využití". Můžete určit pořadí, ve kterém by měl být zahájen virtuálních počítačích nebo skupinu virtuálních počítačích na webu obnovení. Můžete taky určit automatické skripty být běžel nebo všechny ruční provést akci, v době obnovení pro každý virtuální počítač. Převzetí (uvedené v následující části) se obvykle při spuštění aktivují na úrovni plánování obnovení koordinovaný obnovení.

- **Shromažďované**: službu shromažďuje zpracuje a přenáší metadat pro obnovení plánu, včetně metadata virtuálního počítače a metadata automatizaci skripty a ruční akce poznámky.

- **Použití informací**: metadata jsme je popsali výše slouží k vytvoření plánu pro obnovení v portálu služby.

- **Volba**: Toto je důležitou součástí službu a nelze vypnout. Pokud nechcete, aby tato informace odesílané do služby, není sestavit obnovení plány jednotného zasílání zpráv v této službě.

**Funkci: Mapování sítě**

- **Akce**: Tato funkce umožňuje mapovat sítě informace z primární datovém centru datacentrem obnovení. Když jsou na webu obnovení obnovit virtuálních počítačích, toto mapování pomáhá při vytváření připojení k síti, je.

- **Shromažďované**: jako součást funkci mapování sítě, služba shromažďuje zpracuje a přenáší metadat logických sítí pro každý web (primární a datacentra).

- **Použití informací**: službu používá metadata k naplnění portálu služby kde mohli porovnat informace o síti.

- **Volba**: Toto je důležitou součástí službu a nelze vypnout. Pokud nechcete, aby tato informace odesílané do služby, nebudete používat funkci mapování sítě.

**Funkce: Převzetí - plánované neplánované, zkouška**

- **Akce**: Tato funkce umožňuje převzetí virtuálního počítače z jednoho VMM spravovaných datacentrem jiné VMM spravovaných datacentra. Převzetí akce je spuštěná uživatelem na jejich portálu. Možné příčiny selhání zahrnout neplánovanou událost (třeba v případě přírodní disaster0; plánované události (například datacentra Vyrovnávání zatížení); přepojení test (třeba zkouška obnovení plán).

Zprostředkovatel na serveru VMM dostane oznámení o události ze služby a provede akci převzetí na hostiteli Hyper-V prostřednictvím rozhraní VMM. Skutečné převzetí virtuálního počítače z jednoho hostitele Hyper-V do druhého (obvykle používají v datovém centru různých "využití") je uskutečněných jednotlivými replikace technologií systému Windows Server 2012 nebo Windows Server 2012 R2 Hyper-V. Po dokončení záložní poskytovatele nainstalovat na server VMM datového centra "využití" odešle informace úspěch služby.

- **Shromažďované**: službu používá výše uvedených informací k naplnění stav informace o převzetí akci na portálu služby.

- **Použití informací**: službu používá výše uvedených informací následujícím způsobem:

    - Osvědčení o řízení – slouží k určení a ověření registrovaných VMM serveru pro přístup ke službě. Služba používá k zabezpečení token registrovaných serveru VMM můžete získat přístup k veřejné klíčové části certifikát. Serveru musí používat tento token získat přístup k funkcím služby.
    - Název serveru VMM – název serveru VMM požaduje pro její identifikovat a odpovídající serverem VMM nacházejí shluky komunikovat.
    - Jména ze serveru VMM v cloudu – je nutné zadat název cloudu, při používání služby cloudu párování/unpairing funkce píše níže. Pokud se rozhodnete spárování cloudu z primární datovém centru s jinou cloudu v datovém centru obnovení, jsou uvedeny názvy mraků všechny v datovém centru obnovení.

- **Volba**: Toto je důležitou součástí službu a nelze vypnout. Pokud nechcete, aby tato informace odesílané do služby, nebudete používat tuto službu.

## <a name="next-steps"></a>Další kroky

Jakmile se dostanete přepojení test zkontrolovat že prostředí funguje očekávaným způsobem, [informace o](site-recovery-failover.md) různých typů převzetí služeb při selhání.
