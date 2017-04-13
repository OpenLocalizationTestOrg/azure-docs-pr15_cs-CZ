<properties
    pageTitle="Replikovat Hyper-V VMs v cloudu VMM sekundární web s obnovení webu Azure pomocí SAN | Microsoft Azure"
    description="Tento článek popisuje, jak replikovat Hyper-V virtuálních počítačích mezi dvěma weby s obnovení webu Azure pomocí služby replikace SAN."
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
    ms.date="07/06/2016"
    ms.author="raynew"/>

# <a name="replicate-hyper-v-vms-in-a-vmm-cloud-to-a-secondary-site-with-azure-site-recovery-using-san"></a>Replikovat Hyper-V VMs v cloudu VMM sekundární web s obnovení webu Azure pomocí SAN

V tomto článku se dozvíte, jak nasadit obnovení webu automatizovat SAN replikace a převzetí pro Hyper-V virtuálních počítačích umístěn v System Center VMM mračnech sekundární web VMM a organizovat.

Po přečtení tento příspěvek článek komentáře nebo dotazy v dolní části tohoto článku nebo na [Fórum služby Azure obnovení](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr). 


## <a name="overview"></a>Základní informace

Organizace potřebovat strategii firmy kontinuitu a havárie obnovení (BCDR), určuje, jak zůstat spuštěn a k dispozici během plánované a neplánované prostoje aplikace, pracovního vytížení a dat a běžných pracovních podmínek obnovit co nejdříve. Vaše BCDR strategie centra kolem řešení, které obchodní data bezpečných a obnovitelné a úloh stále k dispozici, když dojde k havárie.

Obnovení webu je služba Azure podíl strategie BCDR tak, že orchestrating replikace místní fyzické servery a virtuálních počítačích s cloudem (Azure) nebo vedlejší datacentra. Pokud dojde k výpadků ve svém hlavním pracovišti, můžete převzít sekundární webu k synchronizaci aplikace a úloh k dispozici. Můžete být zpět na svém hlavním pracovišti při vrátí na normálně. Obnovení webu lze použít v různých scénářích a můžete chránit celá řada úloh. Další informace najdete v [Co je obnovení webu Azure?](site-recovery-overview.md)

Tento článek obsahuje pokyny k nastavení replikace Hyper-V VMs mezi weby VMM do jiného pomocí SAN relication. Obsahuje přehled architektury a nasazení předpoklady a pokyny. Budete Seznamte se s a klasifikaci SAN úložiště na VMM, poskytnutí logické a přidělování úložiště clusterů Hyper-V. Byl dokončen testováním převzetí a ujistěte se, všechno funguje očekávaným způsobem.


## <a name="why-replicate-with-san"></a>Proč replikovat SAN?

Tady je tento scénář je:

- Poskytuje podnikové řešení scalable replikace automatické obnovení webu.
- Využívá SAN možnosti replikace poskytovanou enterprise úložiště partnery mezi oběma vláken kanálu a úložišť iSCSI. Viz náš [SAN úložiště partnery](http://social.technet.microsoft.com/wiki/contents/articles/28317.deploying-azure-site-recovery-with-vmm-and-san-supported-storage-arrays.aspx).
- Využití stávající infrastrukturu SAN chránit důležitých nasazené Hyper-V clusterů.
- Podporuje clusterů Host.
- Zajišťuje konzistenci replikace v rámci různých úrovní aplikaci synchronizované replikace zhoršeným RTO a operace RPO a synchronního replikace vysoké flexibilitu, v závislosti na úložiště maticových funkcí.  
- Lze integrovat s VMM nabízí SAN správy v konzole VMM "a" SMI – ne v VMM zjistí existující úložiště.  

## <a name="architecture"></a>Architektura

Tento scénář chrání svého pracovního vytížení tak, že zálohování Hyper-V virtuálních počítačích mezi weby VMM místní do jiného pomocí SAN replikace.

![SAN architektura](./media/site-recovery-vmm-san/architecture.png)

Součásti v tomto scénáři patří:

- **Místní virtuálních počítačích**– serverech Hyper-V místním spravovaných na soukromé mračnech VMM obsahují virtuálních počítačích chcete zamknout.
- **Místní VMM servery**– můžete jeden nebo více VMM servery na primární webu chcete zamknout a na vedlejší webu.
- **SAN úložiště**– pole A SAN na primární webu a jednu vedlejší webu
-  **Obnovení webu Azure trezoru**– trezoru souřadnic a orchestrates otevřené dat, převzetí a obnovení mezi místním weby.
- **Poskytovatel obnovení webu Azure**– zprostředkovatel se instaluje na všech serverech VMM.

## <a name="before-you-start"></a>Než začnete

Zkontrolujte, jestli že budete mít tyto požadavky na místě:

**Zjistit předpoklady pro** | **Podrobnosti** 
--- | ---
**Azure**| Musíte mít účet [Microsoft Azure](https://azure.microsoft.com/) . Můžete začít s [bezplatnou zkušební verzi](https://azure.microsoft.com/pricing/free-trial/). [Další informace](https://azure.microsoft.com/pricing/details/site-recovery/) o obnovení webu ceny. 
**VMM** | Musíte mít alespoň jeden server VMM nasazený jako samostatný fyzické nebo virtuální server nebo jako virtuální obrázku. <br/><br/>VMM server by měla běžet System Center 2012 R2 s nejnovějšími kumulativními aktualizacemi.<br/><br/>Musíte mít alespoň jeden cloudu nakonfigurovaný na primární VMM serveru, který chcete zamknout a jeden cloudu nakonfigurovaný na vedlejší VMM serveru, který chcete použít pro ochranu a obnovení<br/><br/>Cloudové zdroje, které chcete zamknout musí obsahovat jednu nebo více skupin VMM Host (hostitel).<br/><br/>Všechny shluky VMM musí mít profilu kapacity Hyper-V nastavení.<br/><br/>Další informace o nastavení VMM mračnech při [konfiguraci struktury cloudu VMM](https://msdn.microsoft.com/library/azure/dn469075.aspx#BKMK_Fabric), a [Návod: Vytvoření soukromého mračnech s System Center 2012 SP1 VMM](http://blogs.technet.com/b/keithmayer/archive/2013/04/18/walkthrough-creating-private-clouds-with-system-center-2012-sp1-virtual-machine-manager-build-your-private-cloud-in-a-month.aspx).
**Hyper-V** | Musíte mít jednu nebo více Hyper-V clusterů na webech primárních a sekundárních a jeden nebo více VMs clusteru Hyper-V zdroje. Skupiny VMM Host (hostitel) v umístění primárních a sekundárních musí mít jeden nebo více clusterů Hyper-V v jednotlivých skupin.<br/><br/>Host (hostitel) a cílové servery Hyper-V musí běžet aspoň Windows Server 2012 s rolí Hyper-V a nainstalovali nejnovější aktualizace.<br/><br/>Všechny Hyper-V server obsahující VMs chcete zamknout musí být umístěné v cloudu VMM.<br/><br/>Pokud používáte Hyper-V v poznámce clusteru této clusteru zprostředkovatele není automaticky vytvoří Pokud máte statickou IP adresu clusteru založený. Musíte ruční konfiguraci zprostředkovatele obrázku. [Další informace](https://www.petri.com/use-hyper-v-replica-broker-prepare-host-clusters) v Aidan Finn blogu.
**SAN úložiště** | Použití SAN replikace můžete replikovat skupinový hosta virtuálních počítačích s iSCSI vláken channel úložiště nebo pomocí sdílené virtuální pevných discích (vhdx).<br/><br/>Budete potřebovat dvou matic SAN nastavení – jeden primární webu a druhý ve vedlejším.<br/><br/>Síťovou infrastrukturu by měl nastavit mezi matice. Prozkoumávání a replikace by měl být nakonfigurované. Replikace licence by měl nastavit v souladu s požadavky na úložiště pole.<br/><br/>Sítě by měl být nastavit mezi servery Hyper-V Host (hostitel) a pole úložiště tak, aby hosts mohli komunikovat s úložiště logické pomocí ISCSI nebo optického.<br/><br/> Zaškrtněte políčko seznam [podporovaných úložištích](http://social.technet.microsoft.com/wiki/contents/articles/28317.deploying-azure-site-recovery-with-vmm-and-san-supported-storage-arrays.aspx).<br/><br/>Měli byste mít nainstalované SMI S poskytovatelů od výrobců úložiště pole a pole SAN by měly být spravovány zprostředkovatele. Nastavení poskytovatele v souladu s jejich si přečtěte následující dokumentaci.<br/><br/>Zajistěte, aby byl poskytovatele SMI S polem na serveru, serveru VMM můžete přistupovat přes síť, protože IP adresa nebo plně kvalifikovaný název domény.<br/><br/>Každé pole SAN by měla být jeden nebo více skupin úložiště k dispozici pro použití v tomto nasazení. Server VMM primární webu budou potřebovat pro správu pole primární a sekundární server VMM bude spravovat sekundární matice.<br/><br/>Server VMM na primární lokalitě by měl spravovat pole primární a sekundární server VMM by měl spravovat sekundární matice.
**Mapování sítě** | Můžete nakonfigurovat mapování sítě a ujistěte se, že replikovanou virtuálních počítačích optimálně umístěn na vedlejší serverech hostitele Hyper-V po převzetí a, můžete připojit k odpovídající OM sítím. Pokud není konfigurace sítě mapování otevřené VMs nesmí být připojeni k jakákoli síť po překlopení.<br/><br/>Nastavení sítě mapování během nasazení zkontrolujte, že virtuálních počítačích na hostitelském serveru zdroj Hyper-V jsou připojené k síti VMM OM. Síť by měl být propojené s logická síť, která je přidružená k cloud. < br /<br/>Cílové cloudu na vedlejší VMM serveru, který používáte pro obnovení by měl mít odpovídající sítě OM nakonfigurované a ho zase by měl být propojené s odpovídající logická síť, která je přidružená k cílové cloudu.<br/><br/>[Další informace](site-recovery-network-mapping.md) o mapování sítě.


## <a name="step-1-prepare-the-vmm-infrastructure"></a>Krok 1: Příprava infrastruktury VMM

Příprava infrastrukturu VMM potřebujete:

1. Zajistěte, aby nastavená VMM mraky
2. Integrace a klasifikovat SAN úložiště na VMM
3. Vytvoření logické a přidělit úložiště
4. Vytvoření skupiny replikace
5. Nastavení sítě OM

### <a name="ensure-vmm-clouds-are-set-up"></a>Zajistěte, aby nastavená VMM mraky

Obnovení webu orchestrates ochranu virtuálních počítačích umístěných na serverech hostitele Hyper-V v VMM mračnech. Musíte ověřit, jestli tyto mračnech jsou nastavené správně než začnete nasazení obnovení webu. Další informace v tématu [Vytvoření soukromého mračnech](http://blogs.technet.com/b/keithmayer/archive/2013/04/18/walkthrough-creating-private-clouds-with-system-center-2012-sp1-virtual-machine-manager-build-your-private-cloud-in-a-month.aspx) na Dryml Mayer blogu.

### <a name="integrate-and-classify-san-storage-in-vmm"></a>Integrace a klasifikovat SAN úložiště na VMM

Přidání třídění SANs v konzole VMM:

1. V pracovním prostoru **struktury** kliknout na **úložiště**. Klikněte na kartu **Domů** > **Přidat zdroje** > **Úložiště zařízení** a spusťte Průvodce přidáním zařízení úložiště.
2. Na stránce **Vyberte typ zprostředkovatele úložiště** vyberte **zařízení SAN a NAS zjistil a spravuje poskytovatel SMI-ne**.

    ![Typ zprostředkovatele](./media/site-recovery-vmm-san/provider-type.png)

3. Na stránce **Zadejte protokol a adresu SMI S poskytovatelem úložiště** vyberte **CIMXML SMI – ne** a nastavení pro připojení k poskytovatele.
4. V **poskytovatele IP adresa nebo plně kvalifikovaný název domény** a **TCP/IP portem**nastavení pro připojení k zprostředkovatele. Můžete použít protokol SSL pro SMI S CIMXML pouze.

    ![Zprostředkovatel připojení](./media/site-recovery-vmm-san/connect-settings.png)

5. Do pole **Spustit jako účet** zadejte VMM spustit jako účet, můžete získat přístup na poskytovatele nebo vytvořte nový účet.
6. Na stránce **Shromáždění informací** VMM automaticky se snaží zjišťovat a importujte informace o zařízení úložiště. Opakovat zjišťování, klikněte na **Prohledat poskytovatele**. Pokud je proces zjišťování úspěšný, zjištěnou úložištích, úložiště fondů výrobce, model a kapacita najdete na stránce.

    ![Seznamte se s úložiště](./media/site-recovery-vmm-san/discover.png)

7. V seznamu **Vyberte úložiště fondů umístěte v části Správa a přiřadit klasifikace**vyberte fondů úložiště, že bude VMM spravovat a jejich přiřazení klasifikace. Z fondu úložiště bude importován logické jednotky informace. Vytvoření logické podle aplikací je potřeba zamknout, jejich požadavkům a svého požadavku na co je potřeba replikovat společně.

    ![Klasifikovat úložiště](./media/site-recovery-vmm-san/classify.png)

### <a name="create-luns-and-allocate-storage"></a>Vytvoření logické a přidělit úložiště

1. Po SAN úložiště je součástí VMM vytvoříte (poskytování) logické jednotky (logických):

    - [Postup při výběru metodu pro vytváření logické jednotky v VMM](https://technet.microsoft.com/library/gg610624.aspx)
    - [Zřízení logické jednotek úložiště ve VMM](https://technet.microsoft.com/library/gg696973.aspx)

    >[AZURE.NOTE] Poté, co jste replikace povolit pro počítač byste neměli přidávat VHD ho logické, které nejsou uložené ve skupině replikace obnovení webu. Pokud neuděláte nebude zjišťování tak, že obnovení webu.

2. Tak, aby VMM můžete nasadit dat k základnímu úložišti zřizování, má přidělte kapacitu úložiště clusteru Hyper-V Host (hostitel):

    - Před přidělování úložiště clusteru musíte přidělit skupině VMM Host (hostitel), na kterém je umístěn obrázku. V tématu [jak přidělit úložiště logické jednotky ke skupině Host (hostitel)](https://technet.microsoft.com/library/gg610686.aspx) a [jak přidělit úložiště fondů ke skupině Host (hostitel)](https://technet.microsoft.com/library/gg610635.aspx). </a>.
    - Potom přidělte kapacitu úložiště clusteru podle popisu v tématu [jak nakonfigurovat úložiště clusteru hostitele Hyper-V v VMM](https://technet.microsoft.com/library/gg610692.aspx). </a>.

### <a name="create-replication-groups"></a>Vytvoření skupiny replikace

Vytvoření skupiny replikace zahrnující všech logických, které budou muset replikovat společně.

1. V konzole VMM otevřete kartu **Skupiny replikace** vlastností pole úložiště a klikněte na **Nový**.
2. Vytvořte skupiny replikace.

    ![SAN replikace skupiny](./media/site-recovery-vmm-san/rep-group.png)

### <a name="set-up-networks"></a>Nastavení sítě

Pokud chcete ke konfiguraci sítě mapování proveďte následující akce:

1. [Přečtěte si informace o](site-recovery-network-mapping.md) mapování sítě.
2. Příprava OM sítě VMM:

    - [Nastavení logických sítí](https://technet.microsoft.com/library/jj721568.aspx).
    - [Nastavení sítě OM](https://technet.microsoft.com/library/jj721575.aspx).

## <a name="step-2-create-a-vault"></a>Krok 2: Vytvoření trezoru


1. Přihlaste se k [Portálu Správa](https://portal.azure.com) z VMM server, ke kterému se chcete zaregistrovat.

2. Rozbalte položku **Datové služby** > **Obnovení služby** a klikněte na **Webu obnovení trezoru**.

3. Klikněte na **vytvořit nový** > **vytvořit**.

4. Do pole **název**zadejte popisný název k identifikaci trezoru.

5. V **oblasti** vyberte zeměpisná oblast pro trezoru. Kontrola podporovaných oblastí najdete v článku zeměpisné dostupnost v [Azure webu obnovení ceny podrobností](https://azure.microsoft.com/pricing/details/site-recovery/).

6. Klikněte na **vytvořit trezoru**.

    ![Nové trezoru](./media/site-recovery-vmm-san/create-vault.png)

Zkontrolujte na stavovém řádku potvrďte, že byla úspěšně vytvořena trezoru. Trezoru se zobrazí jako **aktivní** na hlavní stránce **Obnovení služby** .


### <a name="register-the-vmm-servers"></a>Zaregistrujte servery VMM

1. Na stránce **Služby Recovery** otevřete stránku rychlý Start. Představení aplikace můžete otevřít také kdykoli pomocí ikony.

    ![Ikona rychlým startem](./media/site-recovery-vmm-san/quick-start-icon.png)

2. V rozevíracím seznamu vyberte **mezi Hyper-V místním webu pomocí účtu replikace pole**.

    ![Registrační klíč](./media/site-recovery-vmm-san/select-san.png)


3. V části **připravit VMM servery**stáhněte nejnovější verzi souboru instalace zprostředkovatele obnovení webů Azure.
4. Spusťte tento soubor na serveru VMM zdroje. Pokud VMM nasazenou v clusteru a instalujete poskytovatele první nainstalovat na aktivní uzel a dokončete instalaci registrace serveru VMM v trezoru. V jednotlivých uzlech nainstalujte na poskytovatele. Poznámka: Pokud upgradujete na poskytovatele budete muset upgradovat na všechny uzly vzhledem k tomu, že se mají všechny běžet ve stejné verzi poskytovatele.
5. Instalační program má několik **Zaškrtněte políčko před požadavky** a žádosti o oprávnění zastavit službu VMM zahájíte instalace zprostředkovatele. Po dokončení instalace je bude třeba restartovat službu VMM automaticky. Pokud instalujete clusteru VMM budete výzva k ukončení roli obrázku.
6. Ve **Službě Microsoft Update** můžete se rozhodnout v aktualizací. Při tomto nastavení povolené poskytovatele podle předpisů rozhraní Microsoft Update zásad bude instalovat aktualizace.

    ![Aktualizace společnosti Microsoft](./media/site-recovery-vmm-san/ms-update.png)

7. Umístění instalace nastavena na ** <SystemDrive>\Program Files\Microsoft System Center 2012 R2\Virtual počítače Manager\bin**. Klikněte na tlačítko nainstalovat, aby se spustila instalace zprostředkovatele.

    ![InstallLocation](./media/site-recovery-vmm-san/install-location.png)

8. Po dokončení instalace zprostředkovatele tlačítko "zaregistrovat k registraci serveru v trezoru.

    ![InstallComplete](./media/site-recovery-vmm-san/install-complete.png)

9. Připojení k **Internetu** určete způsob poskytovatele na serveru VMM připojení k Internetu. Vyberte možnost **použít výchozí systém proxy nastavení** používat výchozí nastavení připojení k Internetu nakonfigurovaný na serveru.

    ![Nastavení sítě Internet](./media/site-recovery-vmm-san/proxy-details.png)

    - Pokud chcete použít vlastní proxy by měl nastavením před instalací zprostředkovatele. Při konfiguraci nastavení serveru proxy vlastní test se spustí při kontrole připojení proxy k.
    - Pokud používáte vlastní proxy nebo výchozí proxy server vyžaduje ověření budete muset zadávat podrobnosti proxy, včetně adresy proxy serveru a portu.
    - Sledovat URL by měl být přístupné z informací o serveru VMM a hosts Hyper-v
        - *. hypervrecoverymanager.windowsazure.com
        - *. accesscontrol.windows.net
        - *. backup.windowsazure.com
        - *. blob.core.windows.net
        - *. store.core.windows.net
    - Povolte IP adresy podle [Rozsahy IP Azure datacentru](https://www.microsoft.com/download/confirmation.aspx?id=41653) a HTTPS (443) Protocol (protokol). Máte by a rozsahy IP adres bílé seznamu Azure oblasti, kterou chcete použít, západ USA.
    - Pokud používáte vlastní proxy VMM RunAs účtu (DRAProxyAccount) se vytvoří automaticky pomocí přihlašovacích údajů zadané proxy. Konfigurace proxy server tak, aby tento účet může ověřovat úspěšně. Nastavení účtu VMM RunAs můžete měnit v konzole VMM. K tomuto účelu nastavení pracovní prostor otevřít, rozbalte zabezpečení, klikněte na Spustit jako účty a potom změnit heslo pro DRAProxyAccount. Je potřeba restartovat službu VMM tak, aby toto nastavení se projeví.

10. **Registrační klíč**vyberte stažené ze obnovení webu Azure a zkopírovala do VMM serveru.
11. Do pole **název trezoru**ověřte název trezoru, ve kterém bude registrován serveru. 

    ![Registrace serveru](./media/site-recovery-vmm-san/vault-creds.png)

12. Nastavení šifrování se používá pouze pro VMM Azure scénáři, pokud jsou pro vás VMM VMM pouze uživateli a pak tuto obrazovku můžete ignorovat.

    ![Registrace serveru](./media/site-recovery-vmm-san/encrypt.png)

13. V poli **název serveru**zadejte popisný název k identifikaci VMM serveru v trezoru. V konfiguraci clusteru zadejte název VMM clusteru role.
14. Při **synchronizaci metadat cloudu počáteční** zadejte popisný název serveru, který se zobrazí v trezoru a vyberte, jestli se má synchronizovat metadat pro všechny shluky na serveru VMM s trezoru. Tato akce jenom s jednou na všech serverech musí. Pokud nechcete synchronizovat všechny shluky, můžete nechat toto nastavení zaškrtnuté a synchronizovat každý cloudu jednotlivě ve vlastnostech cloudu v konzole VMM.

    ![Registrace serveru](./media/site-recovery-vmm-san/friendly-name.png)

15. Klikněte na **Další** a dokončit. Po registraci metadata ze serveru VMM načítané obnovení webu Azure. Server se zobrazí na kartě *Servery VMM* na stránce **servery** v trezoru.

### <a name="command-line-installation"></a>Přepínač příkazového řádku instalace

Obnovení poskytovatel webu Azure lze také nainstalovat pomocí následující příkazový řádek. Tato metoda slouží k instalaci zprostředkovatele na serveru CORE pro Windows Server 2012 R2.

1. Stáhněte instalační soubor poskytovatele a registračního klíče do složky Říkejte C:\ASR
2. Zastavit službu System Center Virtual Machine Manager
3. Extrahování instalační program poskytovatele spuštěním následujícího z příkazového řádku s oprávněními **Správce** :

        C:\Windows\System32> CD C:\ASR
        C:\ASR> AzureSiteRecoveryProvider.exe /x:. /q

4. Instalace zprostředkovatele spuštěním těchto věcí:

        C:\ASR> setupdr.exe /i

5. Zaregistrujte zprostředkovatele spuštěním těchto věcí:

        CD C:\Program Files\Microsoft System Center 2012 R2\Virtual Machine Manager\bin
        C:\Program Files\Microsoft System Center 2012 R2\Virtual Machine Manager\bin\> DRConfigurator.exe /r  /Friendlyname <friendly name of the server> /Credentials <path of the credentials file> /EncryptionEnabled <full file name to save the encryption certificate>         

Kde jsou parametry:

 - **/Credentials** : povinný parametr, který určuje umístění, ve kterém je umístěn soubor klíčové registrace  
 - **/FriendlyName** : povinný parametr název serveru hostitele Hyper-V, která se zobrazí v portálu obnovení webu Azure.
 - **/EncryptionEnabled** : volitelný parametr, který je potřeba použít pouze v VMM Azure scénář v případě potřeby šifrování virtuálních počítačích, na v klidu v Azure. Ověřte, zda, název souboru, který zadáte s příponou **.pfx** .
 - **/proxyAddress** : volitelný parametr, který určuje adresy proxy serveru.
 - **/proxyport** : volitelný parametr, který určuje port proxy serveru.
 - **/proxyUsername** : volitelný parametr, který určuje Proxy uživatelské jméno (pokud proxy server vyžaduje ověření).
 - **/proxyPassword** : volitelný parametr, který určuje heslo pro ověřování proxy serveru (pokud proxy server vyžaduje ověření).


## <a name="step-3-map-storage-arrays-and-pools"></a>Krok 3: Namapování úložištích a skupin

Mapování polí můžete určit, které fond sekundárním úložiště obdrží replikace dat z fondu primární. Úložiště by měl namapujte před konfigurovat ochranu shluk, protože při povolit ochranu skupiny replikace slouží informace mapování.

Před zahájením zkontrolujte, zda Oblaka se zobrazí v trezoru. Mračnech jsou zjištěny tak, že vyberete k synchronizaci všechny shluky při instalaci zprostředkovatele nebo tak, že vyberete k synchronizaci konkrétní cloudu na kartě **Obecné** vlastnosti cloudu v konzole VMM. Potom mapování úložištích následujícím způsobem:

1. Klikněte na **zdroje** > **serveru úložiště** > **mapy zdrojové a cílové pole**.
    ![Registrace serveru](./media/site-recovery-vmm-san/storage-map.png)
2. Vyberte pole úložiště na primární webu a namapujte je na úložištích na vedlejší webu.
3.  Namapujte zdrojové a cílové skupiny úložiště v polích. K tomuto účelu v **Úložiště fondů** vyberte zdrojové a cílové fondu úložiště mapování.

    ![Registrace serveru](./media/site-recovery-vmm-san/storage-map-pool.png)

## <a name="step-4-configure-cloud-protection-settings"></a>Krok 4: Nakonfigurujte cloudu nastavení ochrany

Po VMM servery jsou registrovány, můžete nakonfigurovat nastavení ochrany cloudu. Možnost **synchronizovat data cloudu s trezoru** povolili, když jste si nainstalovali zprostředkovatele tak všechny shluky na serveru VMM se zobrazí na kartě <b>Chráněné položky</b> v trezoru.

![Publikované cloudu](./media/site-recovery-vmm-san/clouds-list.png)

1. Na stránce Snadné spuštění klikněte na **Nastavení ochrany VMM mračnech**.
2. Na kartě **Chráněné položky** vyberte cloudu, kterou chcete konfigurovat a přejděte na kartu **Konfigurace** . Všimněte si, že:
3. V **cílové**vyberte **VMM**.
4. V **cílové umístění**vyberte na místě VMM server, který má na starosti cloudu, který chcete použít k obnovení.
5. V **cílové cloudu**vyberte cílovou cloudu, kterou chcete použít pro překlopení virtuálních počítačích v cloudu zdroje. Všimněte si, že:
    - Doporučujeme, abyste vybrali cílové obláčkem, která vyhovuje požadavkům pro obnovení na virtuálních počítačích, které budete zamknout.
    - Shluk můžete přiřadit pouze pár jednoho cloudu – buď jako primární nebo cílové cloudu.
6. Obnovení Azure webu ověří mračnech mít přístup k SAN replikace může úložiště a, že jsou peered matice úložiště. Zúčastněné kolegové pole se zobrazí.
7. Pokud bude ověření úspěšné, písmem **replikace**, vyberte **SAN**.

Po uložení nastavení úlohy se vytvoří a můžete sledovat na kartě **projekty** . Na kartě **Konfigurovat** lze upravovat nastavení cloudu. Pokud chcete změnit cílové umístění nebo cílové cloudu musíte odebrat konfiguraci cloudu a překonfigurovat cloudu.

## <a name="step-5-enable-network-mapping"></a>Krok 5: Povolení mapování sítě

1. Na stránce Snadné spuštění klikněte na **mapovat sítě**.
2. Vyberte zdrojový server VMM ze které chcete propojit sítě a potom do kterého sítí namapované cílový VMM server. Zobrazí seznam zdroj sítě a jeho přidružený cílový sítě. Zobrazí se prázdná buňka sítích, které nejsou namapované aktuálně. Klikněte na informační ikonu u názvů sítě zdrojové a cílové zobrazení podsítí pro každou síť.
3. Vyberte síť v **síti na zdroj**a klikněte na položku **Mapa**. Služba zjistí sítí OM na cílovém serveru a zobrazí jejich.

    ![SAN architektura](./media/site-recovery-vmm-san/network-map1.png)

4. V dialogovém okně, která se zobrazí vyberte jeden sítí OM cílový VMM server.

    ![SAN architektura](./media/site-recovery-vmm-san/network-map2.png)

5. Po výběru cílové síti se zobrazují chráněné mračnech používající Zdrojová síť. Zobrazí se také k dispozici cílové sítí, které souvisí s mračnech pro ochranu. Doporučujeme vybrat cílové síti, která je dostupná pro všechny shluky, které používáte pro ochranu.
6.  Klikněte na značku zaškrtnutí dokončete proces mapování. Úlohy začne sledovat průběh mapování. Můžete ji zobrazit na kartě **projekty** .


## <a name="step-6-enable-replication-for-replication-groups"></a>Krok 6: Povolení replikace replikace skupin

Před povolením ochranu virtuálních počítačích, budete muset povolit replikace skupin replikace úložiště.

1. Na portálu obnovení webu Azure ve vlastnostech otevřete straně primární cloudu kartu **virtuálních počítačích** . Klikněte na **Přidat skupinu replikace**.
2. Vyberte jeden nebo více skupin replikace VMM, které jsou přidružené k cloudu, ověřte zdrojových a cílových polí a zadejte požadovanou frekvenci opakování.

Po dokončení operace obnovení webu Azure společně s VMM poskytovatelů SMI S zřízení úložiště cílového webu logické a povolte úložiště. Pokud už replikace skupiny replikace obnovení webu Azure opakované používání existující relací replikace a automaticky aktualizuje informace o obnovení webu Azure.

## <a name="step-7-enable-protection-for-virtual-machines"></a>Krok 7: Povolte ochranu virtuálních počítačích

Po úložiště skupiny replikace, povolte ochranu virtuálních počítačích v konzole VMM některým z následujících metod:

- **Nový virtuální počítač**– v VMM konzoly při vytváření nového počítače virtuální povolení obnovení webu Azure ochrany a virtuální počítač přidružit skupiny replikace.
Pomocí této možnosti VMM používá inteligentní umístění optimálně umístit virtuálního počítače úložiště na logickým replikační skupiny. Obnovení Azure webu orchestrates vystavením stín virtuálního počítače na vedlejší webu a přidělí kapacita tak, aby otevřené virtuálních počítačích mohli začít po převzetí.
- **Existující virtuálního počítače**– Pokud virtuálního počítače je nasadili jste v VMM, můžete povolit ochranu obnovení webu Azure a proveďte migraci úložiště do skupiny replikace. Po dokončení VMM obnovení webu Azure zjišťovat novém počítači virtuální a správou v obnovení webu Azure ochranu. Virtuální počítač stín je vytvořen na vedlejší webu a přidělené kapacitu virtuálního počítače otevřené mohli začít po převzetí.

    ![Povolení ochrany](./media/site-recovery-vmm-san/enable-protect.png)

Po virtuálních počítačích spuštěných pro ochranu se zobrazí v konzole obnovení webu Azure. Můžete zobrazovat vlastnosti virtuálního počítače, sledování stavu a selhání replikace skupin, které obsahují více virtuálních počítačích. Všimněte si, že v SAN replikace všechny virtuálních počítačích přidružené ke skupině replikace musí převzít společně. Je to proto selháním ve vrstvě úložiště nejdřív. Je důležité zařadit do skupiny skupiny replikace správně a místo společně pouze související virtuálních počítačích.

>[AZURE.NOTE] Poté, co jste replikace povolit pro počítač byste neměli přidávat VHD ho logické, které nejsou uložené ve skupině replikace obnovení webu. Pokud neuděláte nebude zjišťování tak, že obnovení webu.

Můžete sledovat průběh povolit ochranu akci na kartě **projekty** , včetně počáteční replikace. Po dlouhodobě spuštěná úloha dokončení ochrany je připraven k převzetí virtuálního počítače.

![Virtuální počítač ochranu projektu](./media/site-recovery-vmm-san/job-props.png)

## <a name="step-8-test-the-deployment"></a>Krok 8: Otestujte nasazení

Otestujte nasazení aby zkontrolovala, jestli virtuálních počítačích a data nepovede přes podle očekávání. Můžete to udělat vytvoříte plán obnovy tak, že vyberete replikace skupiny. Spusťte test přepojení ve plánu.

1. Na kartě **Obnovení plány jednotného zasílání zpráv** klikněte na možnost **Vytvořit plán obnovení**.
2. Zadejte název pro obnovení plán a zdrojové a cílové VMM servery. Zdrojovému serveru musí mít virtuálních počítačích, kteří mají povolenou převzetí a obnovení. Vyberte **SAN** zobrazíte jenom mračnech nakonfigurované pro SAN replikace.
3.
    ![Vytvoření plánu pro obnovení](./media/site-recovery-vmm-san/r-plan.png)

4. V **Vyberte virtuálního počítače**vyberte replikace skupiny. Všechny virtuálních počítačích přidružené ke skupině replikace bude vybraná a přidá do plánu obnovy. Tyto virtuálních počítačích se přidají do výchozí skupiny plán obnovení – skupiny 1. v případě potřeby můžete přidat další skupiny. Všimněte si, že po replikace virtuálních počítačích začnou podle pořadí skupin plán pro obnovení.

    ![Přidání virtuálních počítačích](./media/site-recovery-vmm-san/r-plan-vm.png)
5. Po obnovení plán byl vytvořen, se zobrazí v seznamu na kartě **Obnovení plány jednotného zasílání zpráv** .
6. Na kartě **Obnovení plány jednotného zasílání zpráv** vyberte plán a klikněte na **Testovat převzetí**.
7. Na stránce **Potvrďte převzetí Test** vyberte **žádné**. Všimněte si, že vyberete tuto možnost povolit selhalo přes otevřené virtuálních počítačích nesmí být připojeni k jakákoli síť. Tím otestujete virtuální počítač selhání podle očekávání že testování prostředí replikace sítě. Podívejte se na postup [spusťte test přepojení](site-recovery-failover.md#run-a-test-failover) podrobné informace o tom, jak použít různé možnosti sítě.


    ![Vyberte testovací sítě](./media/site-recovery-vmm-san/test-fail1.png)

8. Test virtuálního počítače se vytvoří ve stejném hostiteli jako host, na kterém se nachází otevřené virtuálního počítače. Není přidána do cloudu, ve kterém se nachází otevřené virtuálního počítače.
9. Po replikace otevřené virtuální počítač bude IP adresu, která není totéž jako IP adresu primární virtuálního počítače. Pokud jste vydání adresy z DHCP pak bude automaticky aktualizován. Pokud nepoužíváte DHCP a chcete, aby zkontrolovala, jestli že jsou adresy stejné musíte spustit několik skriptů.
10. Spusťte tento ukázkový skript k načtení IP adresu.

        $vm = Get-SCVirtualMachine -Name <VM_NAME>
        $na = $vm[0].VirtualNetworkAdapters>
        $ip = Get-SCIPAddress -GrantToObjectID $na[0].id
        $ip.address  

11. Spusťte tento ukázkový skript aktualizovat DNS zadání adresy IP se obnovená pomocí předchozího ukázka skriptu.

        [string]$Zone,
        [string]$name,
        [string]$IP
        )
        $Record = Get-DnsServerResourceRecord -ZoneName $zone -Name $name
        $newrecord = $record.clone()
        $newrecord.RecordData[0].IPv4Address  =  $IP
        Set-DnsServerResourceRecord -zonename $zone -OldInputObject $record -NewInputObject $Newrecord

## <a name="next-steps"></a>Další kroky

Jakmile se dostanete přepojení test zkontrolovat že prostředí funguje očekávaným způsobem, [informace o](site-recovery-failover.md) různých typů převzetí služeb při selhání.
