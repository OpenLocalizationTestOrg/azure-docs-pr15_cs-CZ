<properties 
   pageTitle="Automatizace DR pro sdílené složky na StorSimple pomocí obnovení webu Azure | Microsoft Azure"
   description="Popisuje kroky a osvědčené postupy pro vytváření řešení obnovení havárie pro sdílené složky hostitelem StorSimple úložiště."
   services="storsimple"
   documentationCenter="NA"
   authors="vidarmsft"
   manager="syadav"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="05/16/2016"
   ms.author="vidarmsft" />

# <a name="automated-disaster-recovery-solution-using-azure-site-recovery-for-file-shares-hosted-on-storsimple"></a>Automatické obnovení havárie řešení pomocí obnovení webu Azure pro hostitelem StorSimple sdílených souborů

## <a name="overview"></a>Základní informace

Microsoft Azure StorSimple je řešení hybridní cloudové úložiště prostředních složitostí Nestrukturovaná data běžně spojené s sdílených souborů. StorSimple používá cloudové úložiště jako rozšíření místních řešení a automaticky úrovní dat přes místní úložiště a cloudového úložiště. Integrovaný ochranu dat s místní a cloudových snímky, nemusí rozšiøujícího infrastruktura úložiště.

[Obnovení webu Azure](../site-recovery/site-recovery-overview.md) je služba založené na Azure, která poskytuje možnosti obnovení (DR) havárie tak, že orchestrating replikace, převzetí a obnovení virtuálních počítačích. Obnovení Azure webu podporuje počet technologie replikace konzistentní replikovat, zamknout a bezproblémově selhání virtuálních počítačích a aplikací soukromá/veřejná nebo hostovanou mračnech.

Obnovení webu Azure, replikace virtuálního počítače a StorSimple cloudu snímek možnosti můžete chránit prostředí serveru celý soubor. V případě přerušení můžete jediným klepnutím na zkrácení online sdílené složky v Azure pár minut.

Tento dokument podrobně vysvětluje, jak můžete vytvořit řešení obnovení havárie pro vaše sdílené složky hostitelem StorSimple úložiště a provádět plánované neplánované a otestujte převzetí služeb při selhání pomocí plán obnovy jedním kliknutím. V podstatě ukazuje, jak můžete upravit obnovení plánování v trezoru obnovení webu Azure povolit převzetí služeb při selhání StorSimple během havárie scénáře. Kromě toho popisuje podporované konfigurace a požadavcích. Tento dokument předpokládá, že znáte základy obnovení webu Azure a StorSimple architektury.

## <a name="supported-azure-site-recovery-deployment-options"></a>Podporované možnosti nasazení obnovení webu Azure

Zákazníci můžete nasadit servery soubor jako fyzické servery nebo virtuálních počítačích (VMs) na Hyper-V VMware a pak vytvořte sdílených souborů z svazky carved mimo StorSimple úložiště. Obnovení Azure webu můžete chránit fyzické a virtuální nasazení webem sekundární nebo Azure. Tento dokument obsahuje podrobnosti o DR řešení s Azure jako obnovení webů pro souborovém serveru, který OM hostitelem Hyper-V a s sdílených souborů v úložišti StorSimple. Ostatních případech, kdy souborový server OM nachází OM VMware nebo pole fyzicky počítače lze provést podobně.

## <a name="prerequisites"></a>Zjistit předpoklady pro

Provádění řešení obnovení havárie jedním kliknutím, které využívá obnovení webu Azure pro sdílené složky hostitelem StorSimple úložiště má následující požadavky:

-   Místního serveru Windows Server 2012 R2 soubor, který OM hostitelem Hyper-V nebo VMware nebo pole fyzicky počítače

-   StorSimple úložiště zařízení místní registrovaný u správce Azure StorSimple

-   Zařízení cloudu StorSimple vytvořené v Azure StorSimple správce (to můžete uchovávat v vypnutí stavu)

-   Sdílené složky hostované objemů nakonfigurovaný na paměťové zařízení StorSimple

-   [Obnovení webu azure služby trezoru](../site-recovery/site-recovery-vmm-to-vmm.md) vytvořené v Microsoft Azure předplatného

Kromě toho-li Azure obnovení webů, spusťte [nástroj Azure virtuálního počítače připravenosti hodnocení](http://azure.microsoft.com/downloads/vm-readiness-assessment/) v VMs zajistit, že jsou kompatibilní se službou Azure VMs a obnovení webu Azure services.

Chcete-li předejít latence Ujistěte se, vytvořit StorSimple cloudu spotřebiče, automatizaci účtu a úložiště (které můžou mít za následek vyšší náklady) problémech se dočtete víc účtů ve stejné oblasti.

## <a name="enable-dr-for-storsimple-file-shares"></a>Povolení DR pro StorSimple sdílené složky  

Jednotlivé součásti místního prostředí, musí být chráněny povolit úplná replikace a obnovení. Tato část popisuje postup:

-   Nastavení služby Active Directory a DNS replikace (volitelné)

-   Umožňuje povolit ochranu souborový server OM obnovení webu Azure

-   Povolení ochrany objemu StorSimple

-   Konfigurace sítě

### <a name="set-up-active-directory-and-dns-replication-optional"></a>Nastavení služby Active Directory a DNS replikace (volitelné)

Pokud chcete zamknout počítačích službou Active Directory a DNS tak, aby byly k dispozici na web DR, musíte explicitně chránit (tak, aby servery souborů přístupných osobám s postižením po převzetí s ověřováním). Podle složitost místního prostředí zákazníka dvěma doporučenými způsoby.

#### <a name="option-1"></a>Možnost 1

Pokud zákazník musí malým počtem poštovních aplikací, jednoho řadiče domény pro celé místních webů a bude nedaří myši celého webu, doporučujeme používat obnovení webu Azure replikace replikovat počítače řadiče domény na vedlejší web (to platí pro na webu a webu Azure) a pak.

#### <a name="option-2"></a>Možnost 2

Pokud zákazník obsahuje velké množství aplikací běží struktuře služby Active Directory a bude nedaří myši několik aplikací najednou a pak doporučujeme nastavení dalšího řadiče domény na web DR (sekundární webu nebo v Azure).

Získáte [Automatické DR řešení služby Active Directory a DNS pomocí obnovení webu Azure](../site-recovery/site-recovery-active-directory.md) pokyny při řadiče domény k dispozici na web DR. Zbývající část tohoto dokumentu jsme se předpokládá, že doménu je dostupná na web DR.

### <a name="use-azure-site-recovery-to-enable-protection-of-the-file-server-vm"></a>Umožňuje povolit ochranu souborový server OM obnovení webu Azure

Tento krok vyžaduje Příprava místního prostředí serveru soubor, vytvořit a připravit trezoru obnovení webu Azure a povolení Ochrana souborů OM.

#### <a name="to-prepare-the-on-premises-file-server-environment"></a>Příprava místního prostředí serveru soubor

1.  Nastavte **Řízení uživatelských účtů** **nikdy**upozornění. To je nutné, aby mohli používat Azure automatizaci skripty připojení cíle iSCSI po selhání úplně od začátku obnovení webu Azure.

    1.  Stiskněte klávesy Windows + Q a vyhledejte **nástroje Řízení uživatelských účtů**.

    2.  Vyberte **Řízení uživatelských účtů změnit nastavení**.

    3.  Přetáhněte posuvník dolů směrem k **Nikdy upozornit**.

    4.  Klikněte na tlačítko **OK** a pak vyberte **Ano** po zobrazení výzvy.

        ![](./media/storsimple-dr-using-asr/image1.png)

1.  Nainstalujte agenta OM jednotlivých hodnot souborový server VMs. To je nutné, takže můžete spustit skripty Azure automatizaci na selhalo přes VMs.

    1.  [Stáhněte si agent](http://aka.ms/vmagentwin) `C:\\Users\\<username>\\Downloads`.

    2.  Otevřete v režimu správce (Spustit jako správce) prostředí Windows PowerShell a zadejte tento příkaz přejděte do umístění ke stažení:

        `cd C:\\Users\\<username>\\Downloads\\WindowsAzureVmAgent.2.6.1198.718.rd\_art\_stable.150415-1739.fre.msi`

        > [AZURE.NOTE] Název souboru lišit v závislosti na verzi.

1.  Klikněte na tlačítko **Další**.

2.  Přijměte **Podmínky smlouvy** a klikněte na tlačítko **Další**.

3.  Klikněte na **Dokončit**.


1.  Vytvoření sdílené složky pomocí svazky carved mimo StorSimple úložiště. Další informace najdete v tématu [použití službu StorSimple správce pro správu svazky](storsimple-manage-volumes.md).

    1.  Na místní VMs stiskněte klávesy Windows + Q a vyhledejte **iSCSI**.

    2.  Vyberte **iSCSI vyzývající**.

    3.  Vyberte kartu **Konfigurace** a zkopírujte vyzývající název.

    4.  Přihlaste se k [Azure klasické portálu](https://manage.windowsazure.com/).

    5.  Vyberte kartu **StorSimple** a vyberte službu StorSimple správce, který obsahuje fyzické zařízení.

    6.  Vytvoření hlasitost nádoba a pak vytvořte svazky. (Tyto svazky jsou u sdílené položky souboru na souborovém serveru VMs). Zkopírujte vyzývající název a zadejte příslušný název záznamů kontrol přístup při vytváření svazky.

    7.  Vyberte kartu **Konfigurovat** a poznámky dolů IP adresu zařízení.

    8.  Na svůj místní VMs znovu přejít **iSCSI vyzývající** a zadejte adresy IP v části Rychlé připojení. Klikněte na tlačítko **Rychlé připojení** (zařízení by měl být připojili).

    9.  Otevřete portál Správa Azure a vyberte kartu **svazky a zařízení** . Klikněte na tlačítko **Automatická konfigurace**. By se měly svazku, který jste právě vytvořili.

    10. Na portálu, vyberte kartu **zařízení** a pak vyberte **vytvořit nové virtuální zařízení.** (Tento virtuální bude použito zařízení Pokud dojde k selhání). Toto nové virtuální zařízení můžete uchovávat ve stavu offline chcete-li předejít navíc náklady. Chcete-li jako offline virtuální zařízení, přejděte k části **virtuálních počítačích** na portálu a vypnout.

    11. Přejděte zpátky do místního VMs a otevřete Správa disků (stisknutím klávesy Windows + X a klikněte na **Správa disku**).

    12. Zjistíte některé další disků (podle počtu svazky, kterou jste vytvořili). Klikněte pravým tlačítkem myši první z nich vyberte **Inicializace disku**a vyberte **OK**. Klikněte pravým tlačítkem myši oddíl **Unallocated** , vyberte **Nové jednoduché hlasitost**, přiřaďte písmeno a dokončení průvodce.

    13. Opakujte krok l u všech discích. Nyní můžete vidět všech discích na **Tento počítač** v Průzkumníku Windows.

    14. Vytvoření sdílené složky na tyto svazky pomocí role souboru a úložiště služby.

#### <a name="to-create-and-prepare-an-azure-site-recovery-vault"></a>Vytvoření a připravit trezoru obnovení webu Azure

Vyhledejte [obnovení webu Azure si přečtěte následující dokumentaci](../site-recovery/site-recovery-hyper-v-site-to-azure.md) pro seznámení s obnovení webu Azure před ochrana souborový server OM.

#### <a name="to-enable-protection"></a>Povolení ochrany

1.  Odpojení od VMs místní, které chcete zamknout prostřednictvím obnovení webu Azure iSCSI cíle:

    1.  Stiskněte klávesy Windows + Q a vyhledejte **iSCSI**.

    2.  Vyberte **Nastavení iSCSI vyzývající**.

    3.  Odpojte zařízení StorSimple, které už předtím připojili. Můžete taky můžete vypnout souborový server pro několik minut po povolení ochrany.

    > [AZURE.NOTE] Dojde k sdílených souborů dočasně nedostupný

1.  [Povolení ochrany virtuálního počítače](../site-recovery/site-recovery-hyper-v-site-to-azure.md##step-6-enable-replication) souborového serveru OM z portálu Microsoft Azure obnovení webu.

2.  Při zahájení počáteční synchronizace se můžete připojit cíl znovu. Přejděte na vyzývající iSCSI vyberte StorSimple zařízení a klikněte na **Připojit**.

3.  Po dokončení synchronizace a stav OM je **chráněn**, vyberte OM, vyberte kartu **Konfigurovat** a aktualizace sítě OM proto (Toto je sítě, ke které selhalo přes VM(s) bude součástí). Pokud v síti nezobrazuje, znamená to, že synchronizace stále probíhá.

### <a name="enable-protection-of-storsimple-volumes"></a>Povolení ochrany objemu StorSimple

Pokud jste nevybrali možnost **Povolit zálohy výchozí pro tento hlasitost** pro svazky StorSimple, přejděte na **Záložní zásady** ve službě StorSimple správce a vytvoření vhodné záložní zásad pro všechny svazky. Doporučujeme nastavit četnost zálohování na cíl obnovení čárky (operace RPO), který chcete najdete v článku aplikace.

### <a name="configure-the-network"></a>Konfigurace sítě

Pro souborový server OM konfigurovat nastavení sítě tak, aby sítích OM jsou připojené k správné síti DR po převzetí v obnovení webu Azure.

Můžete zvolit OM v **Cloudu VMM** nebo **Ochranu skupiny** konfigurovat nastavení sítě, jak je znázorněno na následujícím obrázku.

![](./media/storsimple-dr-using-asr/image2.png)

## <a name="create-a-recovery-plan"></a>Vytvoření plánu pro obnovení

Vytvoření plánu pro obnovení ve funkci Automatické obnovení systému k automatizaci převzetí s sdílených souborů. Pokud dojde k přerušení, můžete si přenést sdílených souborů za několik okamžiků s jediným klepnutím. Chcete-li tento automatizaci, budete potřebovat účet Azure automatizaci.

#### <a name="to-create-the-account"></a>Vytvořte účet

1.  Přejděte na portál Azure klasické a přejděte k části **automatizaci** .

1.  Vytvoření nového účtu automatizaci. V jednoduchosti je ve stejné geo/oblasti, ve které jsou vytvořené StorSimple cloudu zařízení a úložiště účty.

2.  Klikněte na **Nový** &gt; **Aplikace služby** &gt; **automatizaci** &gt; **postupu Runbook** &gt; **Z Galerie** k importu všech požadovaných runbooks zohledňovala automatizaci.

    ![](./media/storsimple-dr-using-asr/image3.png)

1.  Přidejte následující runbooks z podokna **Havárie obnovení** v galerii:

    -   Převzetí StorSimple hlasitost kontejnery

    -   Vyčistit objemu StorSimple po převzetí Test (TFO)

    -   Připojte svazky na zařízení StorSimple po překlopení

    -   Zahájení StorSimple virtuální zařízení

    -   Odinstalace vlastní skript rozšíření v Azure OM

        ![](./media/storsimple-dr-using-asr/image4.png)


1.  Publikujte tyto skripty výběrem postupu runbook automatizaci účtu a přejdete na kartu **Autor** . Po dokončení tohoto kroku se zobrazí karta **Runbooks** následujícím způsobem:

     ![](./media/storsimple-dr-using-asr/image5.png)

1.  Automatizace účtu přejděte na kartu **prostředky** , klepněte na tlačítko **Přidat nastavení** &gt; **Přidat pověření**a přidání přihlašovacích údajů Azure – název materiálů AzureCredential.

    Pomocí přihlašovacích údajů Windows PowerShell. Je vhodné přihlašovací údaje, který obsahuje ID organizace uživatelské jméno a heslo mají přístup k tomuto předplatnému Azure a vícefaktorové ověřování zakázané. Toto je potřeba k ověření jménem uživatele během převzetí služeb při selhání a vyvoláte svazky serveru souborů na web DR.

1.  V okně automatizaci účet vyberte kartu **prostředky** a klikněte na **Přidat nastavení** &gt; **Přidat proměnnou** a přidejte následující proměnné. Můžete k šifrování těchto prostředky. Tyto proměnné jsou obnovení specifické pro plán. Pokud vaše obnovení plánování (který vytvoříte v dalším kroku) název je TestPlan a pak proměnných by měl být TestPlan StorSimRegKey TestPlan AzureSubscriptionName a tak dál.

    -   *RecoveryPlanName* **-StorSimRegKey**: registrace klíč pro službu StorSimple správce.

    -   *RecoveryPlanName* **-AzureSubscriptionName**: název Azure předplatného.

    -   *RecoveryPlanName* **-ResourceName**: název StorSimple prostředek, který má StorSimple zařízení.

    -   *RecoveryPlanName* **Název-zařízení**: zařízení, které chcete překlopit.

    -   *RecoveryPlanName* **-TargetDeviceName**: cloudu zařízení StorSimple na kterém mají selhání kontejnery.

    -   *RecoveryPlanName* **-VolumeContainers**: řetězec hodnotami oddělenými čárkou hlasitost kontejnerů prezentovat na zařízení, které je třeba selhání; například volcon1 volcon2, volcon3.

    -   RecoveryPlanName**-TargetDeviceDnsName**: název služby cílového zařízení (najdete je v části **virtuálního počítače** : název služby je shodný s názvem DNS).

    -   *RecoveryPlanName* **-StorageAccountName**: název účtu úložiště, ve kterém budou uloženy skript (který má dělat selhalo přes OM). Může to být jakémukoli účtu úložiště má místo pro ukládání skript dočasně.

    -   *RecoveryPlanName* **-StorageAccountKey**: přístupová klávesa výše účtu úložiště.

    -   *RecoveryPlanName* **-ScriptContainer**: jméno container, ve kterém bude skript uložený v cloudu. Pokud kontejneru neexistuje, bude vytvořena.

    -   *RecoveryPlanName* **-VMGUIDS**: při ochraně virtuálního počítače, obnovení webu Azure přiřadí každé OM jedinečné ID, které poskytuje podrobné informace o selhalo přes OM. Získat VMGUID, vyberte kartu **Služby Recovery** a poté klikněte na **Položku chráněné** &gt; **Ochranu skupiny** &gt; **počítačích** &gt; **Vlastnosti**. Pokud máte víc VMs, přidejte GUID jako řetězec hodnotami oddělenými čárkou.

    -   *RecoveryPlanName* **-AutomationAccountName** – název účtu automatizaci, do které jste přidali runbooks a aktiva.

    Například pokud je v poli Název plánu obnovy fileServerpredayRP, potom **prostředky** kartu by měl vypadat takto po přidání všech prostředky.

    ![](./media/storsimple-dr-using-asr/image6.png)


1.  Přejděte do části **Obnovení služby** a vyberte trezoru obnovení webu Azure, který jste vytvořili.

2.  Vyberte kartu **Obnovení plány jednotného zasílání zpráv** a vytvoření nového plánu obnovení následujícím způsobem:

    na.  Zadejte název a vyberte odpovídající **Ochranu skupiny**.

    b.  Vyberte VMs ochranu skupiny, který chcete zahrnout do plánu obnovy.

    c.  Po obnovení plán vytvořit, vyberte jej otevřete zobrazení pro vlastní plán obnovení.

    d.  Vyberte položku Vypnout **všechny skupiny**, klikněte na **index**a vyberte **Přidat primární skriptu před všechny skupiny vypnutí**.

    e.  Vyberte účet automatizaci (ve které jste přidali runbooks) a potom vyberte **nepovede přes StorSimple hlasitost kontejnerů** postupu runbook.

    f.  Klikněte na **skupiny 1: Spusťte**, zvolte **virtuálních počítačích**a přidejte VMs, které mají být chráněny v plánu obnovení.

    g.  Klikněte na **skupiny 1: Spusťte**, zvolte **skriptu**a přidejte následující skripty v pořadí jako kroky **Po skupiny 1** .

    - Zahájení-StorSimple-virtuální-zařízení postupu runbook
    - Selhání postupu runbook přes StorSimple hlasitost kontejnerů
    - Postupu runbook připojení svazky po překlopení
    - Odinstalace vlastní skript rozšíření postupu runbook

1.  Přidání ručního akce za výše uvedených 4 skripty ve stejném **skupiny 1: po kroky** oddíl. Tato akce je čárky niž ověříte, že všechno funguje správně. Tato akce je potřeba přidat pouze jako součást test převzetí (tak jenom vyberte **Testovat převzetí** políčko).

2.  Po ruční akci přidejte skript vyčištění stejným způsobem, který jste použili pro ostatní runbooks. Uložení plánu obnovy.

    > [AZURE.NOTE] Při spuštění selhání test, ověřte všechno v kroku ruční akce protože svazky StorSimple, které vám to klonovat na cílové zařízení, se odeberou jako součást vymazání po dokončení ruční akce.

    ![](./media/storsimple-dr-using-asr/image7.png)

## <a name="perform-a-test-failover"></a>Provedení přepojení test

Při selhání testovat v nápovědě k Průvodce companion [Řešení Active Directory DR](../site-recovery/site-recovery-active-directory.md) pro konkrétní aspekty ke službě Active Directory. Místní nastavení není narušen vůbec, když dojde k selhání testu. Připojené k OM místní svazky StorSimple jsou klonovat Spotřebiče cloudu StorSimple na Azure. OM pro účely testování aktualizovány v Azure a klonovaný svazky jsou připojené bude v angličtině.

#### <a name="to-perform-the-test-failover"></a>Chcete-li provést převzetí test

1.  Na portálu Azure klasické vyberte trezoru obnovení webu.

1.  Klikněte na plán obnovení vytvořený soubor serveru OM.

2.  Klikněte na **testovat převzetí**.

3.  Vyberte virtuální síť, spusťte proces překlopení testu.

    ![](./media/storsimple-dr-using-asr/image8.png)

1.  Je-li sekundární prostředí nahoru, můžete provádět vaší ověření.

2.  Po dokončení ověření klikněte na tlačítko **Ověření dokončeno**. Bude vyčistit testovacím prostředí převzetí a operaci TFO bude dokončen.

## <a name="perform-an-unplanned-failover"></a>Provedení neplánované překlopení

Při neplánované selhání svazky StorSimple se nepodařilo přes virtuální zařízení, otevřené OM budou aktualizovány na Azure a svazky jsou připojené bude v angličtině.

#### <a name="to-perform-an-unplanned-failover"></a>Chcete-li provést neplánované překlopení

1.  Na portálu Azure klasické vyberte trezoru obnovení webu.

1.  Klikněte na možnost plán obnovení vytvoří souborovém serveru OM.

2.  Klikněte na **převzetí** a pak vyberte **Neplánované převzetí**.

    ![](./media/storsimple-dr-using-asr/image9.png)

1.  Vyberte cílové síti a pak klikněte na ikonu kontrola ✓ spusťte proces překlopení.

## <a name="perform-a-planned-failover"></a>Provedení plánované selhání

Při plánované selhání serveru, na který OM se neukončí a obláčkem pořízení záložní snímek svazky na zařízení StorSimple souborů místní. Svazky StorSimple se nepodařilo přes virtuální zařízení, otevřené OM aktualizovány na Azure a svazky jsou připojené bude v angličtině.

#### <a name="to-perform-a-planned-failover"></a>Chcete-li provést plánované selhání

1.  Na portálu Azure klasické vyberte trezoru obnovení webu.

1.  Klikněte na plán obnovení vytvořený soubor serveru OM.

2.  Klikněte na **převzetí** a pak vyberte **Plánované překlopení**.

3.  Vyberte cílové síti a pak klikněte na ikonu kontrola ✓ spusťte proces překlopení.

## <a name="perform-a-failback"></a>Provedení navrácení

Během navrácení StorSimple hlasitost kontejnery jsou nepodařilo přes zpět fyzické zařízení po zálohy, se považuje.

#### <a name="to-perform-a-failback"></a>Chcete-li provést navrácení

1.  Na portálu Azure klasické vyberte trezoru obnovení webu.

1.  Klikněte na plán obnovení vytvořený soubor serveru OM.

2.  Klikněte na **převzetí** a vyberte **Plánovaná převzetí** nebo **neplánované převzetí**.

3.  Klikněte na **změnit směr**.

4.  Vyberte vhodné datové synchronizace a možnosti vytváření OM.

5.  Klikněte na ikonu kontrola ✓ zahájení procesu navrácení.

    ![](./media/storsimple-dr-using-asr/image10.png)

## <a name="best-practices"></a>Doporučené postupy

### <a name="capacity-planning-and-readiness-assessment"></a>Kapacita hodnocení plánování a připravenosti


#### <a name="hyper-v-site"></a>Web pro Hyper-V

Pomocí [nástroje Plánovač uživatele kapacita](http://www.microsoft.com/download/details.aspx?id=39057) navrhnout serveru, ukládání a síťovou infrastrukturu pro prostředí pro Hyper-V otevřené.

#### <a name="azure"></a>Azure

[Nástroj hodnocení připravenost počítače virtuální Azure](http://azure.microsoft.com/downloads/vm-readiness-assessment/) možné spouštět na VMs zajistit, že jsou kompatibilní se službou Azure VMs a obnovení služby Azure webu. Hodnocení nástroj pro přípravu zkontroluje OM konfigurace a konfigurace jsou kompatibilní s Azure, zobrazí upozornění. Například ho upozornění jednotku C: je větší než 127 GB.


Plánování kapacity je tvořen nejméně dvě důležité procesů:

-   Mapování místní VMs Hyper-V Azure OM formáty (například A6 A7, A8 a A9).

-   Určení požadované šířky pásma Internetu.

## <a name="limitations"></a>Omezení

- V současné době jenom 1 StorSimple zařízení lze překlopit (do jednoho StorSimple cloudu zařízení). Scénář souborovém serveru, který přesahuje několik StorSimple zařízení není zatím podporované.

- Pokud dojde k chybě při povolení ochrany virtuálního počítače, ujistěte se, že jste odpojeni cíle iSCSI.

- Hlasitost kontejnery, které mají společná seskupené kvůli záložní zásady trvající přes volume kontejnery se nezdaří přes společně.

- Všechny svazky v kontejnerech hlasitost, kterou jste zvolili se nezdaří myší.

- Svazky, které dohromady tvoří více než 64 TB nemůže být se nezdařila přes maximální kapacita jednoho zařízení cloudu StorSimple je 64 TB.

- Pokud se nezdaří plánované neplánované převzetí a VMs vytvořené v Azure, pak ne vyčištění VMs. Místo toho udělejte navrácení. Pokud byste odstranili VMs potom VMs místní nejde zapnout znovu.

- Po selhání Pokud nejste neuvidíte svazky, přejděte na VMs, otevřete Správa disků, disků a přepněte je online.

- V některých případech může být písmena v na web DR jiný než písmena na pracovišti. Pokud k tomu dojde, budete muset ručně problém po dokončení záložní.

- Vícefaktorové ověřování by měl být zakázaný Azure přihlašovací údaje zadané v účtu automatizaci jako datový zdroj. Pokud je tento ověřování není zakázané, skripty nebude moct automatické spuštění a obnovení plán se nezdaří.

- Časový limit převzetí úlohy: StorSimple skript vyprší platnost převzetí hlasitost kontejnery trvá déle než limit obnovení webu Azure na skript (aktuálně 120 minut).

- Časový limit záložní úlohy: StorSimple skript vyprší její časový limit zálohování svazky trvá déle než limit obnovení webu Azure na skript (aktuálně 120 minut).
 
    > [AZURE.IMPORTANT] Ruční spuštění zálohování z portálu Microsoft Azure a pak znova spusťte plánu obnovy.

- Klonovat časový limit úlohy: StorSimple skript vyprší její časový limit klonováním svazky trvá déle než limit obnovení webu Azure na skript (aktuálně 120 minut).

- Čas chyby synchronizace: StorSimple skripty chyby, oznamující, že byly zálohy neúspěšné i v případě, že zálohování se úspěšně na portálu. Možná příčina pro to může být zařízení StorSimple čas pravděpodobně se nesynchronizují s aktuální čas v časovém pásmu.
 
    > [AZURE.IMPORTANT] Synchronizace čas zařízení s aktuální čas v časovém pásmu.

- Chyba převzetí zařízení: StorSimple skript může selhat, pokud je zařízení selhání při spuštění plánu obnovy.
    
    > [AZURE.IMPORTANT] Spusťte plánu obnovy po dokončení převzetí zařízení.

## <a name="summary"></a>Souhrn

Použití obnovení webu Azure, můžete vytvořit plán obnovení úplné automatické havárie souborovém serveru OM s sdílených souborů hostitelem StorSimple úložiště. Zahájení záložní vyvolané z libovolného místa v případě narušení a získání aplikací nahoru a spuštěnou za několik minut.
