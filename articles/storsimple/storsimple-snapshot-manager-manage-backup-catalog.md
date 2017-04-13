<properties 
   pageTitle="Zálohování katalogu StorSimple snímek správce | Microsoft Azure"
   description="Popisuje, jak používat modulu snap-in konzoly MMC StorSimple snímek správce k zobrazení a Správa katalogu zálohy."
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
   ms.date="04/26/2016"
   ms.author="v-sharos" />

# <a name="use-storsimple-snapshot-manager-to-manage-the-backup-catalog"></a>Použití StorSimple snímek správce pro správu záložní katalogu

## <a name="overview"></a>Základní informace

Primární funkcí aplikace StorSimple snímek Manager je umožňují vytvářet aplikace konzistentní záložní kopie StorSimple svazky ve formě snímky. Snímky se zobrazí v souboru XML s názvem *záložní katalogu*. Zálohování katalogu slouží k uspořádání snímků skupinou hlasitost a potom podle místního snímku nebo cloudu snímku. 

Tento kurz popisuje, jak můžete pomocí uzel **Katalog zálohování** udělali úkoly podle těchto pokynů:

- Obnovení svazku 
- Klonovat hlasitost nebo skupinu hlasitosti 
- Odstranění zálohy 
- Obnovení souboru
- Obnovení databáze Storsimple snímek správce

Zobrazení katalogu záložní rozbalením uzel **Záložní katalogu** v podokně **obor** či rozbalení skupiny hlasitost.

- Když kliknete na název skupiny objem, v podokně **výsledků** zobrazuje počet místní snímky a snímky cloud k dispozici objemu skupiny. 

- Pokud kliknete na **Místní snímku** nebo **Cloudu snímek**v podokně **výsledků** se zobrazí tyto informace o jednotlivých záložní snímek (v závislosti na nastavení **zobrazení** ): 

    - **Název** – čas, kdy byla přijata snímek. 

    - **Typ** – jestli to je místní snímku nebo snímku cloudu. 

    - **Vlastník** – vlastník obsahu. 

    - **K dispozici** – jestli snímek momentálně neexistuje. **True** označuje, že snímku je k dispozici a obnovit je může; **False** označuje, že snímek už není dostupná. 

    - **Importované** – jestli importu zálohování. **True** označuje, že zálohování bylo importovaný z službu StorSimple správce v době, kdy zařízení nakonfigurovanou v StorSimple snímek správce; **Nepravda** znamená, že nebyl naimportován, ale byl vytvořen správcem snímek StorSimple. (Umožňuje snadno identifikovat skupinu importovaných hlasitost příponu je přidán, která určuje zařízení, ze kterého byla skupině hlasitost importovány.)

    ![Zálohování katalogu](./media/storsimple-snapshot-manager-manage-backup-catalog/HCS_SSM_Backup_catalog.png)

- Pokud rozbalte **Místní snímku** nebo **Cloudu snímek**a potom klikněte na název jednotlivých snímku, v podokně **výsledků** zobrazuje tyto informace o snímek, který jste vybrali:

    - **Název** – hlasitost označenými písmeno. 

    - **Název lokální** – název místní jednotku (Pokud je k dispozici). 

    - **Zařízení** – název zařízení, na kterém je umístěn hlasitost. 

    - **K dispozici** – jestli snímek momentálně neexistuje. **True** označuje, že snímku je k dispozici a obnovit je může; **False** označuje, že snímek už není dostupná. 


## <a name="restore-a-volume"></a>Obnovení svazku

Obnovení svazku ze zálohy pomocí následujícího postupu.

#### <a name="prerequisites"></a>Zjistit předpoklady pro

Pokud jste to ještě neudělali, vytvořte hlasitost a hlasitost skupiny a potom odstraňte hlasitost. Ve výchozím nastavení správce snímek StorSimple zálohovat svazku před umožňující pro odstranění. Toto opatření můžete předešli ztrátě dat, pokud omylem odstranili hlasitost nebo jestli data potřebují napřed má být vrácena z nějakého důvodu. 

StorSimple snímek správce, zobrazí se následující zpráva vytvoří bezpečnostní zálohování.

![Automatické snímek zprávy](./media/storsimple-snapshot-manager-manage-backup-catalog/HCS_SSM_Automatic_snap.png) 

>[AZURE.IMPORTANT]Nelze odstranit svazku, který je součástí skupiny hlasitost. Příkaz Odstranit je k dispozici. <br>

#### <a name="to-restore-a-volume"></a>Chcete-li obnovit svazku

1. Klikněte na ikony na ploše spuštění StorSimple snímek správce. 

2. V podokně **obor** rozbalte uzel **Katalog zálohování** , rozbalte skupinu hlasitost a potom klikněte na **Místní snímky** nebo **Snímky cloudu**. V podokně **výsledků** se zobrazí seznam záložních snímků. 

3. Vyhledání zálohy, kterou chcete obnovit, klikněte pravým tlačítkem myši a potom klikněte na **Obnovit**. 

    ![Obnovení záložní katalogu](./media/storsimple-snapshot-manager-manage-backup-catalog/HCS_SSM_Restore_BU_catalog.png) 

4. Na stránce potvrzení zkontrolujte podrobnosti, zadejte **Potvrdit**a klikněte na **OK**. Správce snímek StorSimple používá zálohování obnovit hlasitost. 

    ![Obnovení potvrzovací zprávy](./media/storsimple-snapshot-manager-manage-backup-catalog/HCS_SSM_Restore_volume_msg.png) 

5. Akce Obnovit můžete sledovat, jak ho spustí. V podokně **obor** rozbalte uzel **úloh** a potom klikněte na **systém**. Podrobnosti projektu se zobrazí v podokně **výsledků** . Po dokončení obnovení úlohy podrobnosti projektu se převedou na seznam **poslední 24 hodin** .

## <a name="clone-a-volume-or-volume-group"></a>Klonovat hlasitost nebo skupinu hlasitosti

Pomocí následujícího postupu vytvořit její duplikát (klonovat) hlasitost nebo objem skupiny.

#### <a name="to-clone-a-volume-or-volume-group"></a>Klonovat hlasitost nebo skupinu hlasitosti

1. Klikněte na ikony na ploše spuštění StorSimple snímek správce.

2. V podokně **obor** rozbalte uzel **Katalog zálohování** , rozbalte skupinu hlasitost a potom klikněte na **Snímky cloudu**. V podokně **výsledků** se zobrazí seznam zálohy.

3. Najděte hlasitost nebo objem skupiny, který chcete zkopírovat, klikněte pravým tlačítkem myši na hlasitost nebo název skupiny hlasitost a klikněte na **vytvořit kopii**. Zobrazí se dialogové okno **Klonovat cloudu snímek** .

    ![Kopírovat snímek cloudu](./media/storsimple-snapshot-manager-manage-backup-catalog/HCS_SSM_Clone.png) 

4. Vyplňte dialogové okno **Klonovat cloudu snímek** následujícím způsobem: 

    1. Do pole **název** zadejte název klonovaný hlasitost. Tento název se zobrazí v uzel **svazky** . 

    2. (Volitelné) vyberte **jednotku**a v rozevíracím seznamu vyberte písmeno. 

    3. (Volitelné) vyberte **Složka (NTFS)**, zadejte cestu ke složce nebo klikněte na tlačítko Procházet a vyberte umístění složky. 

    4. Klikněte na **vytvořit**.

5. Po dokončení procesu klonování musí inicializace klonovaný hlasitost. Spusťte správce serveru a začněte Správa disků. Podrobné pokyny najdete v tématu [připojit svazky](storsimple-snapshot-manager-manage-volumes.md#mount-volumes). Po inicializaci, hlasitost uvedené v části uzel **svazky** v podokně **obor** . Pokud nevidíte hlasitost uvedené, aktualizujte seznam svazky (klikněte pravým tlačítkem myši na uzel **svazky** a potom na tlačítko **Aktualizovat**).

## <a name="delete-a-backup"></a>Odstranění zálohy

Chcete-li odstranit snímek z katalogu záložní pomocí následujícího postupu. 

>[AZURE.NOTE]Odstranění snímku odstraní zálohované data spojená se snímku. Proces čištění dat z cloudu však může chvíli trvat.<br>
 
#### <a name="to-delete-a-backup"></a>Chcete-li odstranit zálohy

1. Klikněte na ikony na ploše spuštění StorSimple snímek správce.

2. V podokně **obor** rozbalte uzel **Katalog zálohování** , rozbalte skupinu hlasitost a potom klikněte na **Místní snímky** nebo **Snímky cloudu**. V podokně **výsledků** se zobrazí seznam snímky. 

3. Klikněte pravým tlačítkem myši na snímek, který chcete odstranit a potom klikněte na **Odstranit**.

4. Po zobrazení žádosti o potvrzení klikněte na **OK**. 

## <a name="recover-a-file"></a>Obnovení souboru

Pokud soubor je omylem odstranili ze svazku, můžete obnovit soubor načítání snímek předem data odstranění použití snímek k vytvoření klonovat hlasitost a zkopírujte soubor z klonovaný hlasitost na původní objem.

#### <a name="prerequisites"></a>Zjistit předpoklady pro

Než začnete, ujistěte se, jestli máte aktuální zálohu skupině hlasitost. Odstraňte soubor uložený v jednom z svazky v dané skupině hlasitost. Nakonec pomocí následujících kroků obnovit odstraněný soubor ze zálohy. 

#### <a name="to-recover-a-deleted-file"></a>Obnovení odstraněné souboru

1. Klikněte na ikonu StorSimple snímek správce na ploše. Zobrazí se okno konzoly StorSimple snímek správce. 

2. V podokně **obor** rozbalte uzel **Katalog zálohování** a přejděte na snímek, který obsahuje odstraněný soubor. Obvykle byste měli vybrat snímek, který byl vytvořený těsně před odstranění. 

3. Vyhledání hlasitost, který chcete zkopírovat, klikněte pravým tlačítkem myši a klikněte na **vytvořit kopii**. Zobrazí se dialogové okno **Klonovat cloudu snímek** .

    ![Kopírovat snímek cloudu](./media/storsimple-snapshot-manager-manage-backup-catalog/HCS_SSM_Clone.png) 

4. Vyplňte dialogové okno **Klonovat cloudu snímek** následujícím způsobem: 

   1. Do pole **název** zadejte název klonovaný hlasitost. Tento název se zobrazí v uzel **svazky** . 

   2. (Volitelné) Vyberte **jednotku**a v rozevíracím seznamu vyberte písmeno. 

   3. (Volitelné) Vyberte **Složku (NTFS)**, zadejte cestu ke složce nebo klikněte na tlačítko **Procházet** a vyberte umístění složky. 

   4. Klikněte na **vytvořit**. 

5. Po dokončení procesu klonování musí inicializace klonovaný hlasitost. Spusťte správce serveru a začněte Správa disků. Podrobné pokyny najdete v tématu [připojit svazky](storsimple-snapshot-manager-manage-volumes.md#mount-volumes). Po inicializaci, hlasitost uvedené v části uzel **svazky** v podokně **obor** . 

    Pokud nevidíte hlasitost uvedené, aktualizujte seznam svazky (klikněte pravým tlačítkem myši na uzel **svazky** a potom na tlačítko **Aktualizovat**).

6. Otevřete složku NTFS, která obsahuje klonovaný hlasitost, rozbalte uzel **svazky** a pak otevřete klonovaný hlasitost. Vyhledejte soubor, který chcete obnovit a jeho zkopírování do primární hlasitost.

7. Po obnovení souboru můžete odstranit NTFS složky, která obsahuje klonovaný hlasitost.

## <a name="restore-the-storsimple-snapshot-manager-database"></a>Obnovení databáze StorSimple snímek správce

Na hostitelském počítači byste měli pravidelně zálohovat databázi StorSimple snímek správce. Pokud dojde k selhání nebo z nějakého důvodu selže hostitelském počítači, ho můžete obnovit ze zálohy. Vytvoření zálohy databáze je ručního procesu.

#### <a name="to-back-up-and-restore-the-database"></a>Zálohování a obnovení databáze

1. Zastavte službu pro správu Microsoft StorSimple:

    1. Spusťte správce serveru.

    2. Na řídicím panelu Správce serveru, v nabídce **Nástroje** vyberte **služby**.

    3. V okně **služby** vyberte **Microsoft StorSimple Management Service**.

    4. V pravém podokně v části **Microsoft StorSimple Management Service**, klikněte na **Zastavit službu**.

2. Na hostitelském počítači vyhledejte C:\ProgramData\Microsoft\StorSimple\BACatalog. 

    >[AZURE.NOTE] ProgramData je skryté.
 
3. Najděte soubor XML katalogu, zkopírujte soubor a uložit kopii bezpečné místo nebo do cloudu. Pokud hostitele nepovede, můžete tento záložní soubor pomáhá při obnovení záložní zásad, které jste vytvořili ve Správci StorSimple snímek.

    ![Azure StorSimple katalogu záložní soubor](./media/storsimple-snapshot-manager-manage-backup-catalog/HCS_SSM_bacatalog.png)

4. Restartujte službu pro správu Microsoft StorSimple: 

    1. Na řídicím panelu Správce serveru, v nabídce **Nástroje** vyberte **služby**.
    
    2. V okně **služby** vyberte **Microsoft StorSimple Management Service**.

    3. V pravém podokně v části **Microsoft StorSimple Management Service**, klikněte na **Restartovat službu**.

5. Na hostitelském počítači vyhledejte C:\ProgramData\Microsoft\StorSimple\BACatalog. 

6. Odstranění souboru XML katalogu a nahradit je záložní verzi, kterou jste vytvořili. 

7. Klikněte na ikony na ploše StorSimple snímek správce spuštění StorSimple snímek správce. 

## <a name="next-steps"></a>Další kroky

- Další informace o [použití Správce snímek StorSimple ke správě StorSimple řešení](storsimple-snapshot-manager-admin.md).
- Další informace o [StorSimple snímek Správce úloh a pracovní postupy](storsimple-snapshot-manager-admin.md#storsimple-snapshot-manager-tasks-and-workflows).
