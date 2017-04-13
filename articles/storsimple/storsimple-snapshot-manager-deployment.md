<properties 
   pageTitle="Nasazení StorSimple snímek správce | Microsoft Azure"
   description="Zjistěte, jak si stáhněte a nainstalujte vedoucímu snímek StorSimple modulu snap-in konzoly MMC pro správu funkce protection a zálohování dat StorSimple."
   services="storsimple"
   documentationCenter="NA"
   authors="SharS"
   manager="carmonm"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="TBD"
   ms.date="05/24/2016"
   ms.author="v-sharos" />

# <a name="deploy-the-storsimple-snapshot-manager-mmc-snap-in"></a>Nasazení modulu snap-in konzoly MMC StorSimple snímek správce

## <a name="overview"></a>Základní informace

Správce snímek StorSimple je snap-in konzoly MMC (Microsoft Management), který zjednodušuje ochrana dat a správu zálohování v prostředí Microsoft Azure StorSimple. Pomocí Správce StorSimple snímku můžete spravovat Microsoft Azure StorSimple místní a úložiště v cloudu jako kdyby ho systém plně integrovaný úložiště, tedy zjednodušení zálohování a obnovení procesů a snížit náklady. 

Tento kurz popisuje požadavky na konfiguraci, jakož i postupy pro instalaci, odebrání a upgradu StorSimple snímek správce.

>[AZURE.NOTE] 
>
>- Správce snímek StorSimple nelze použít ke správě Microsoft Azure StorSimple virtuální pole (označovaná taky jako StorSimple místní virtuální zařízení).
>
>- Pokud chcete nainstalovat StorSimple aktualizace 2 na zařízení s StorSimple, nezapomeňte si stáhnout nejnovější verzi aplikace StorSimple snímek Manager a nainstalujte ji **před instalací StorSimple aktualizace 2**. Nejnovější verzi StorSimple snímek Manager je zpětně kompatibilní a spolupracuje všech verzích Microsoft Azure StorSimple. Pokud používáte dřívější verzi StorSimple snímek Manager, musíte aktualizovat (nepotřebujete před nainstalovat novou verzi odinstalovat v předchozí verzi).

## <a name="storsimple-snapshot-manager-installation"></a>Instalace StorSimple snímek správce

Správce snímek StorSimple možné nainstalovat na počítačích se systémem Windows Server 2008 R2 SP1, Windows Server 2012 nebo operační systém Windows serveru 2012 R2. Na serverech se systémem Windows 2008 R2 musíte nainstalovat Windows Server 2008 s aktualizací SP1 a Windows Management Framework 3.0. 

Před instalací nebo upgrade modulu snap-in StorSimple snímek správce pro konzoly Microsoft Management (MMC), ujistěte se, správně nakonfigurované serveru Microsoft Azure StorSimple Host (hostitel) a zařízení. 

## <a name="configure-prerequisites"></a>Konfigurace požadavky

V následujících krocích všeobecný přehled konfigurace úkoly, musíte nejdřív udělat před instalací Správce StorSimple snímek. Dokončení konfigurace Microsoft Azure StorSimple a informace o nastavení, včetně systémové požadavky a podrobné pokyny najdete v článku [StorSimple zařízení v místním nasazení](storsimple-deployment-walkthrough.md).

>[AZURE.IMPORTANT]Než začnete, přečtěte si [Kontrolní seznam konfigurace nasazení](storsimple-deployment-walkthrough.md#deployment-configuration-checklist) a a [nasazení požadavky](storsimple-deployment-walkthrough.md#deployment-prerequisites) v [zařízení StorSimple v místním nasazení](storsimple-deployment-walkthrough.md).
> <br>
 
### <a name="before-you-install-storsimple-snapshot-manager"></a>Před instalací StorSimple snímek správce

1. Rozbalení, připojit a připojte zařízení Microsoft Azure StorSimple podle popisu v [nainstalovat zařízení StorSimple 8100](storsimple-8100-hardware-installation.md) nebo [StorSimple 8600 zařízení](storsimple-8600-hardware-installation.md).

2. Ujistěte se, že hostitelském počítači běží jednu z následujících operačních systémů:

    - Windows Server 2008 R2 (na serverech musí běžet Windows 2008 R2, musíte taky nainstalovat Windows Server 2008 s aktualizací SP1 a Windows Management Framework 3.0)
    - Windows Server 2012
    - Windows serveru 2012 R2
 
    Pro StorSimple virtuální zařízení musí být hostiteli Microsoft Azure virtuálního počítače. 

3. Ujistěte se, splňují všechny požadavky Microsoft Azure StorSimple konfigurace. Další informace přejděte na [požadavky na nasazení](storsimple-deployment-walkthrough.md#deployment-prerequisites).

4. Připojte zařízení k hostiteli a provedení počáteční konfigurace. Další informace přejděte na [kroků nasazení](storsimple-deployment-walkthrough.md#deployment-steps).

5. Vytvářet svazky na zařízení a jejich přiřazení k hostiteli a ověřte, zda hostiteli můžete připojit a jejich použití. Správce snímek StorSimple podporuje následující typy svazky: 

    - Základní svazky
    - Jednoduché svazky
    - Dynamické svazky
    - Zrcadlený dynamické svazky (RAID 1)
    - Sdílené clusteru svazky
 
    Informace o vytváření svazky StorSimple zařízení nebo StorSimple virtuální zařízení, přejděte na [Krok 6: vytvoření svazku](storsimple-deployment-walkthrough.md#step-6-create-a-volume), v [zařízení StorSimple v místním nasazení](storsimple-deployment-walkthrough.md).

## <a name="install-a-new-storsimple-snapshot-manager"></a>Nainstalujte nový snímek správce StorSimple

Před instalací StorSimple snímek správce, ujistěte se, svazky vytvořený StorSimple zařízení nebo StorSimple virtuální zařízení jsou připojených inicializován a formátované podle popisu v [požadavky na konfigurovat](#configure-prerequisites).

>[AZURE.IMPORTANT]
>
>- Pro StorSimple virtuální zařízení musí být hostiteli Microsoft Azure virtuálního počítače. 
>
>- Hostiteli musí běžet Windows 2008 R2, Windows Server 2012 nebo Windows Server 2012 R2. Pokud váš server běží Windows Server 2008 R2, musíte taky nainstalovat Windows Server 2008 s aktualizací SP1 a Windows Management Framework 3.0.
>
>- Připojení iSCSI hostiteli StorSimple zařízení musíte nakonfigurovat před připojením zařízení správci snímek StorSimple.

Tímto postupem dokončit instalaci aplikace StorSimple snímek Manager. Pokud instalujete upgradovat, přejděte na [Upgrade nebo přeinstalujte StorSimple snímek správce](#upgrade-or-reinstall-storsimple-snapshot-manager).

- Krok 1: Instalace StorSimple snímek správce 
- Krok 2: Připojte StorSimple snímek Správce zařízení 
- Krok 3: Ověření připojení k zařízení 

###<a name="step-1-install-storsimple-snapshot-manager"></a>Krok 1: Instalace StorSimple snímek správce

Pomocí následujících kroků instalace StorSimple snímek správce.

#### <a name="to-install-storsimple-snapshot-manager"></a>Chcete-li nainstalovat StorSimple snímek správce

1. Stažení softwaru StorSimple snímek správce (Přejít na [Snímek správce StorSimple](https://www.microsoft.com/download/details.aspx?id=44220) na webu Microsoft Download Center) a uložte jej místně na hostiteli.

2. V Průzkumníkovi souborů klikněte pravým tlačítkem myši na mu tuhle zkomprimovanou složku a potom klikněte na **extrahovat všechny**.

3. V okně **extrahovat mu tuhle zkomprimovanou (Zipped) složky** v dialogovém okně **Vyberte cíl a extrahujte soubory** zadejte nebo přejděte na místo, kam chcete soubor extrahovat cestu. 

       >[AZURE.IMPORTANT] Správce snímek StorSimple musíte nainstalovat na jednotku C:.
 
4. Zaškrtněte políčko **Zobrazit extrahované soubory až budete hotovi** a potom klikněte na **extrahovat**.

    ![Extrahujte soubory dialogovým oknem](./media/storsimple-snapshot-manager-deployment/HCS_SSM_extract_files.png) 

4. Po extrakci otevře cílovou složku. Poklikejte na ikonu nastavení aplikace, která se zobrazí do cílové složky.

5. Až se zobrazí zpráva **Nastavení úspěšné** , klikněte na **Zavřít**. Ikona StorSimple snímek správce byste měli vidět na ploše.

    ![ikony na ploše](./media/storsimple-snapshot-manager-deployment/HCS_SSM_desktop_icon.png) 

### <a name="step-2-connect-storsimple-snapshot-manager-to-a-device"></a>Krok 2: Připojte StorSimple snímek Správce zařízení

Pomocí následujících kroků připojení StorSimple snímek správce k StorSimple zařízení.

#### <a name="to-connect-storsimple-snapshot-manager-to-a-device"></a>Připojení StorSimple snímek Správce zařízení

1. Klikněte na ikonu StorSimple snímek správce na ploše. Zobrazí se okno Správce StorSimple snímek. Okno obsahuje **obor** podokno podokno **výsledků** a podokno **Akce** . 

    ![Správce snímek StorSimple uživatelské rozhraní](./media/storsimple-snapshot-manager-deployment/HCS_SSM_gui_panes.png) 

    - Podokno **obor** (v levém podokně) obsahuje seznam uzlů uspořádána ve stromové struktuře. Můžete rozbalit některé uzly vyberte zobrazení nebo určitá data týkající se uzel. Klikněte na šipku pro rozbalení nebo sbalení uzel. Klikněte pravým tlačítkem myši na položku v podokně **obor** zobrazíte seznam dostupných akcí pro danou položku. 

    - Podokno **výsledků** (středovém podokně) obsahuje podrobný stav informace o uzel, zobrazení nebo data, která jste vybrali v podokně **obor** .

    - V podokně **akcí** zobrazují operace, které lze provádět na uzel, zobrazení nebo data, která jste vybrali v podokně **obor** .

    Detailní popis uživatelského rozhraní StorSimple snímek správce najdete v článku [Správce snímek StorSimple uživatelského rozhraní](storsimple-use-snapshot-manager.md).

2. V podokně **obor** klikněte pravým tlačítkem myši na uzel **zařízení** a pak klikněte na **Konfigurovat zařízení**. Zobrazí se dialogové okno **Konfigurovat zařízení** .

    ![Nastavení zařízení](./media/storsimple-snapshot-manager-deployment/HCS_SSM_config_device.png) 

3. V seznamu **zařízení** vyberte IP adresu Microsoft Azure StorSimple nebo virtuální zařízení. Do textového pole **heslo** zadejte heslo správce StorSimple snímek, který jste vytvořili pro zařízení Azure klasické portálu. Klikněte na **OK**.

4. Správce snímek StorSimple vyhledá zařízení, které jste určili. Pokud je k dispozici, přidá StorSimple snímek Správce připojení. Můžete [ověřit připojení k zařízení](#to-verify-the-connection) na zkontrolujte připojení byla přidaná úspěšně.

    Pokud zařízení není k dispozici z nějakého důvodu, vrátí funkce StorSimple snímek správce chybová zpráva. Kliknutím na **OK** zavřete chybovou zprávu a potom klikněte na **Zrušit** a zavřete dialogové okno **Konfigurovat zařízení** .

5. Při připojení k zařízení, správce snímek StorSimple importuje každá skupina hlasitost nakonfigurován pro toto zařízení za předpokladu, že skupině hlasitost přidruženou zálohy. Hlasitost skupin, které nemají přidružené zálohy nebudou importovány. Kromě toho nebudou importovány záložní zásad, které jsou vytvořené pro skupinu hlasitost. Importované skupiny zobrazíte klikněte pravým tlačítkem myši nejvyšší úrovni **Hlasitost skupiny** v podokně **obor** a klikněte na **přepínač importovat skupiny**.

### <a name="step-3-verify-the-connection-to-the-device"></a>Krok 3: Ověření připojení k zařízení

Pomocí následujících kroků můžete ověřit, jestli správce snímek StorSimple je připojený k StorSimple zařízení.

#### <a name="to-verify-the-connection"></a>Ověřte připojení

1. V podokně **obor** klikněte na uzel **zařízení** .

    ![Stav StorSimple snímek Správce zařízení](./media/storsimple-snapshot-manager-deployment/HCS_SSM_Device_status.png) 

2. Podívejte se do podokna **výsledků** : 

   - Pokud se zobrazí na ikoně zařízení zeleného indikátoru a **online** se zobrazí ve sloupci **Stav** , pak je zařízení připojené. 

   - Pokud se zobrazí červený indikátor nad ikonou zařízení a není k dispozici ve sloupci **Stav** , není připojený zařízení. 

   - Pokud **aktualizací** se zobrazí ve sloupci **Stav** , správce snímek StorSimple je načítání hlasitost skupiny a přidružená zálohy pro připojení zařízení.

## <a name="upgrade-or-reinstall-storsimple-snapshot-manager"></a>Upgrade nebo přeinstalujte StorSimple snímek správce

Správce snímek StorSimple měli úplně odinstalovat před upgrade nebo přeinstalovat software. 

Před přeinstalací StorSimple snímek správce, obecnějším údajům existující databázi StorSimple snímek správce na hostitelském počítači. To ukládá záložní zásady a informace o konfiguraci, takže se na tato data můžete snadno obnovení ze zálohy.

Pokud jsou upgrade nebo přeinstalací StorSimple snímek správce, postupujte takto:

- Krok 1: Odinstalace StorSimple snímek správce 
- Krok 2: Vytvoření zálohy databáze StorSimple snímek správce 
- Krok 3: Přeinstalujte StorSimple snímek správce a obnovení databáze 

### <a name="step-1-uninstall-storsimple-snapshot-manager"></a>Krok 1: Odinstalace StorSimple snímek správce

Pomocí následujících kroků odinstalovat StorSimple snímek správce.

#### <a name="to-uninstall-storsimple-snapshot-manager"></a>Odinstalace StorSimple snímek správce

1. Na hostitelském počítači otevřete **Ovládací panely**, klikněte na **programy**a potom klikněte na **programy a funkce**.

2. V levém podokně klikněte na **odinstalovat nebo změnit program**.

3. Klikněte pravým tlačítkem myši **StorSimple snímek správce**a potom klikněte na **odinstalovat**.

4. Spustí se StorSimple snímek správce instalačního programu. Klikněte na položku **Změnit nastavení**a pak klikněte na **odinstalovat**.

    >[AZURE.NOTE] Pokud jsou všechny MMC procesy běží na pozadí, například StorSimple snímek správce nebo Správa disků se nepovede odinstalace a dostanou zprávu, zavřete všechny výskyty MMC před pokusem o odinstalovat program. Vyberte **automaticky zavřete aplikace a pokusí se je po dokončení instalace znovu spustit**a klikněte na **OK**.
 
5. Po dokončení procesu odinstalace **Úspěšně nastavení** zpráva zobrazí. Klikněte na tlačítko **Zavřít**.

### <a name="step-2-back-up-the-storsimple-snapshot-manager-database"></a>Krok 2: Vytvoření zálohy databáze StorSimple snímek správce

Pomocí následujících kroků můžete vytvořit a uložit kopii databáze StorSimple snímek správce.

#### <a name="to-back-up-the-database"></a>Zálohování databáze

1. Zastavte službu pro správu Microsoft StorSimple:

   1. Spusťte správce serveru.

   2. Na řídicím panelu Správce serveru, v nabídce **Nástroje** vyberte **služby**.

   3. Na stránce **služby** vyberte **Microsoft StorSimple Management Service**.

   4. V pravém podokně v části **Microsoft StorSimple Management Service**, klikněte na **Zastavit službu**.

        ![Zastavit službu StorSimple správce](./media/storsimple-snapshot-manager-deployment/HCS_SSM_stop_service.png)

2. Přejděte na C:\ProgramData\Microsoft\StorSimple\BACatalog. 

    >[AZURE.NOTE] ProgramData je skryté.

3. Najděte soubor XML katalogu, zkopírujte soubor a uložit kopii bezpečné místo nebo do cloudu.

    ![Soubor StorSimple záložní katalogu](./media/storsimple-snapshot-manager-deployment/HCS_SSM_bacatalog.png)

4. Restartujte službu pro správu Microsoft StorSimple: 

    1. Na řídicím panelu Správce serveru, v nabídce **Nástroje** vyberte **služby**.

    2. Na stránce **služby** vyberte **Microsoft StorSimple správy Statistika**e.

    3. V pravém podokně v části **Microsoft StorSimple Management Service**, klikněte na **Restartovat službu**. 

### <a name="step-3-reinstall-storsimple-snapshot-manager-and-restore-the-database"></a>Krok 3: Přeinstalujte StorSimple snímek správce a obnovení databáze

K přeinstalaci StorSimple snímek správce, postupujte podle [nainstalujte nový snímek správce StorSimple](#install-a-new-storsimple-snapshot-manager). Potom postupujte následujícím způsobem obnovit databázi StorSimple snímek správce.

#### <a name="to-restore-the-database"></a>Obnovení databáze

1. Zastavte službu pro správu Microsoft StorSimple:

    1. Spusťte správce serveru.

    2. Na řídicím panelu Správce serveru, v nabídce **Nástroje** vyberte **služby**.

    3. Na stránce **služby** vyberte **Microsoft StorSimple Management Service**.

    4. V pravém podokně v části **Microsoft StorSimple Management Service**, klikněte na **Zastavit službu**.

2. Přejděte na C:\ProgramData\Microsoft\StorSimple\BACatalog. 

     >[AZURE.NOTE] ProgramData je skryté.

3. Odstraňte soubor XML katalogu a nahradit je verze, kterou jste si předtím uložili.

4. Restartujte službu pro správu Microsoft StorSimple: 

    1. Na řídicím panelu Správce serveru, v nabídce **Nástroje** vyberte **služby**.

    2. Na stránce **služby** vyberte **Microsoft StorSimple Management Service**.

    3. V pravém podokně v části **Microsoft StorSimple Management Service**, klikněte na **Restartovat službu**.

## <a name="next-steps"></a>Další kroky

- Další informace o StorSimple snímek správce, přejděte na [Co je správce snímek StorSimple?](storsimple-what-is-snapshot-manager.md).

- Další informace o uživatelského rozhraní StorSimple snímek správce, přejděte na [Správce snímek StorSimple uživatelského rozhraní](storsimple-use-snapshot-manager.md).

- Další informace o použití StorSimple snímek správce, přejděte na [Správce snímek StorSimple použít ke správě StorSimple řešení](storsimple-snapshot-manager-admin.md).
