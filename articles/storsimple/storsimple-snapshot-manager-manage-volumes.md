<properties 
   pageTitle="Správce snímek StorSimple a svazky | Microsoft Azure"
   description="Popisuje, jak pomocí modulu snap-in konzoly MMC StorSimple snímek správce zobrazovat a spravovat svazky a konfigurace zálohování."
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
   ms.date="04/18/2016"
   ms.author="v-sharos" />

# <a name="use-storsimple-snapshot-manager-to-view-and-manage-volumes"></a>Pomocí Správce snímek StorSimple zobrazovat a spravovat svazky

## <a name="overview"></a>Základní informace

Můžete uzel StorSimple snímek správce **svazky** (v podokně **obor** ) zaškrtněte svazky a zobrazení informací o nich znáte. Svazky naznačují jednotky, které odpovídají svazky připevněna hostiteli. Uzel **svazky** zobrazuje místní svazky a hlasitost typů podporovaných službami StorSimple, včetně objemů zjistil použitím iSCSI a zařízení. 

Další informace o podporovaných svazky přejděte na [podporu více typů hlasitost](storsimple-what-is-snapshot-manager.md#support-for-multiple-volume-types).

![Hlasitost seznamu v podokně výsledků](./media/storsimple-snapshot-manager-manage-volumes/HCS_SSM_Volume_node.png)

Uzel **svazky** umožňuje taky prohledat nebo odstraňovat svazky StorSimple snímek správce zjistí je. 

Tento kurz vysvětluje, jak můžete připojit, inicializace a formátovat a potom použijte StorSimple snímek správce:

- Zobrazení informací o svazky 
- Odstranit svazky
- Prohledat svazky 
- Konfigurace základní hlasitost nebo obecnějším údajům
- Konfigurace dynamické zrcadlený hlasitost nebo obecnějším údajům

>[AZURE.NOTE] Všechny akce uzel **Hlasitost** jsou k dispozici v podokně **akcí** .
 
## <a name="mount-volumes"></a>Připojte svazky

Pomocí následujícího postupu připojit, inicializace a formátovat StorSimple. Tento postup používá Správa disků systémový nástroj pro správu pevných discích a odpovídající svazky nebo oddílů. Další informace o správě disku přejděte na [Správa disků](https://technet.microsoft.com/library/cc770943.aspx) na webu Microsoft TechNet.

#### <a name="to-mount-volumes"></a>Chcete-li připojit svazky

1. Na hostitelském počítači spusťte vyzývající iSCSI Microsoft.

2. Zadat jeden z těchto rozhraní IP adresy jako cílový portál nebo zjišťování IP adresu a připojte zařízení. Po připojení zařízení svazky budou mít přístup k systému Windows. Další informace o používání vyzývající iSCSI Microsoft, přejděte do části "Připojením k zařízení cílové iSCSI" v [instalace a konfigurace aplikace Microsoft iSCSI vyzývající][1].

3. Vyberte některou z následujících možností spuštění Správa disků:

    - Do pole **Spustit** zadejte Diskmgmt.msc.

    - Spusťte správce serveru, rozbalte uzel **úložiště** a potom klikněte na **Správa disku**.

    - Spuštění **Nástroje pro správu**, rozbalte uzel **Správa počítače** a potom klikněte na **Správa disku**. 

    >[AZURE.NOTE] Spustit nástroj Správa disků musíte pomocí oprávnění správce.
 
4. Proveďte svazky online:

   1. V části Správa disku klikněte pravým tlačítkem myši svazky označené jako **Offline**.

   2. Klikněte na tlačítko **Aktivovat Disk**. Po aktivaci by měl být disk označen jako **Online** .

5. Inicializace svazky:

   1. Klikněte pravým tlačítkem myši zjištěnou svazky.

   2. V nabídce vyberte **Inicializace disku**.

   3. V dialogovém okně **Inicializace disku** vyberte disků, které chcete inicializace a potom klikněte na **OK**.

6. Formátovat je jednoduchá:

   1. Klikněte pravým tlačítkem myši svazku, který chcete formátovat.

   2. V nabídce vyberte **Nový jednoduchý hlasitost**.

   3. Pomocí Průvodce nové jednoduché hlasitost formátování hlasitost:

      - Určete velikost hlasitost.
      - Zadejte písmeno.
      - Výběr souborů NTFS.
      - Určete velikost jednotku přidělení 64 KB.
      - Proveďte rychlé formátování.

7. Formátovat více oddílů. Pokyny přejděte k části "Oddíly a svazky" v [Implementace správy disku](https://msdn.microsoft.com/library/dd163556.aspx).

## <a name="view-information-about-your-volumes"></a>Zobrazení informací o svazky

Následující postup slouží k zobrazení informací o místní a Azure StorSimple svazky.

#### <a name="to-view-volume-information"></a>Chcete-li zobrazit informace o svazku

1. Klikněte na ikony na ploše spuštění StorSimple snímek správce. 

2. V podokně **obor** klikněte na uzel **svazky** . V podokně **výsledků** se zobrazí seznam svazky místní a připojené, včetně všech objemů Azure StorSimple. Sloupce v podokně **výsledků** jsou, která dokáže nahradit. (Klikněte pravým tlačítkem myši na uzel **svazky** vyberte **zobrazení**a klikněte na **Přidat nebo odebrat sloupce**.)

    ![Konfigurace sloupce](./media/storsimple-snapshot-manager-manage-volumes/HCS_SSM_View_volumes.png)

    Výsledky sloupce | Popis 
    :--------------|:-------------
    Jméno           | Sloupec **název** obsahuje písmeno přiřazená každý zjištěnou hlasitost.
    Zařízení         | **Zařízení** sloupec obsahuje adresy IP zařízení připojené k hostitelském počítači.
    Název Hlasitost zařízení | Sloupec **Název Hlasitost zařízení** obsahuje název Hlasitost zařízení, ke kterému patří vybrané hlasitost. To je název hlasitost definované Azure klasické portálu pro konkrétní svazku.
    Přístupové cesty   | Sloupec **Přístupové cesty** zobrazuje přístupová cesta hlasitost. Toto je jednotka písmeno nebo připojení bod niž hlasitost je dostupný na hostitelském počítači.
 
## <a name="delete-a-volume"></a>Odstranění svazku

Pomocí následujícího postupu odstranění svazku StorSimple snímek ve Správci systému.

>[AZURE.NOTE] Objem nelze odstranit, pokud je součástí žádné skupiny hlasitost. (Možnost odstranit není k dispozici pro svazky, které jsou členy skupiny hlasitost.) Je potřeba odstranit celý hlasitost skupinu odstranit hlasitost.


#### <a name="to-delete-a-volume"></a>Chcete-li odstranit svazku

1. Klikněte na ikony na ploše spuštění StorSimple snímek správce.

2. V podokně **obor** klikněte na uzel **svazky** . 

3. V podokně **výsledků** klikněte pravým tlačítkem hlasitosti, který chcete odstranit.

4. V nabídce klikněte na **Odstranit**. 

    ![Odstranění svazku](./media/storsimple-snapshot-manager-manage-volumes/HCS_SSM_Delete_volume.png) 

5. Zobrazí se dialogové okno **Odstranit hlasitost** . Do textového pole zadejte **Potvrdit** a klikněte na **OK**.

6. Ve výchozím nastavení správce snímek StorSimple zálohovat svazku potom jej odstraní. Toto opatření můžete chránit před ztrátou dat v případě odstranění nežádoucí. Správce snímek StorSimple zobrazí zprávu průběh **Automatické snímku** během zálohovat hlasitost. 

    ![Automatické snímek zprávy](./media/storsimple-snapshot-manager-manage-volumes/HCS_SSM_Automatic_snap.png) 

## <a name="rescan-volumes"></a>Prohledat svazky

Pomocí následujícího postupu prohledat svazky připojené k StorSimple snímek správce.

#### <a name="to-rescan-the-volumes"></a>Jestliže chcete prohledat svazky

1. Klikněte na ikony na ploše spuštění StorSimple snímek správce.

2. V podokně **obor** klikněte pravým tlačítkem myši **svazky**a potom klikněte na **Prohledat svazky**.

    ![Prohledat svazky](./media/storsimple-snapshot-manager-manage-volumes/HCS_SSM_Rescan_volumes.png)
 
    Tento postup synchronizuje seznam hlasitost pomocí StorSimple snímek správce. Všechny změny, například nové svazky nebo odstraněné svazky se projeví ve výsledcích.

## <a name="configure-and-back-up-a-basic-volume"></a>Konfigurace a obecnějším údajům základní hlasitosti

Použijte následující postup konfigurace záložní kopii základní objem a potom spustit zálohy okamžitě nebo vytvoření zásad pro plánované zálohování.

### <a name="prerequisites"></a>Zjistit předpoklady pro

Než začnete:

- Ujistěte se, správně nakonfigurované StorSimple zařízení a hostitele počítač. Další informace přejděte na [zařízení StorSimple v místním nasazení](storsimple-deployment-walkthrough-u2.md).

- Nainstalujte a nakonfigurujte StorSimple snímek správce. Další informace přejděte na [Správce StorSimple snímek nasazení](storsimple-snapshot-manager-deployment.md).

#### <a name="to-configure-backup-of-a-basic-volume"></a>Konfigurace zálohování základní hlasitosti

1. Vytvoření základní hlasitosti na StorSimple zařízení.

2. Připojit, inicializace a naformátujte hlasitost podle popisu v [připojit svazky](#mount-volumes). 

3. Klikněte na ikonu StorSimple snímek správce na ploše. Zobrazí se okno Správce StorSimple snímek. 

4. V podokně **obor** klikněte pravým tlačítkem myši na uzel **svazky** a klikněte na **Prohledat svazky**. Po dokončení vyhledávání seznam svazky objevit v podokně **výsledků** . 

5. V podokně **výsledků** klikněte pravým tlačítkem hlasitosti a pak vyberte **Vytvořit skupinu hlasitost**. 

    ![Vytvoření skupiny hlasitosti](./media/storsimple-snapshot-manager-manage-volumes/HCS_SSM_Create_volume_group.png) 

6. V dialogovém okně **Vytvořit skupinu hlasitost** zadejte název skupiny objem, přiřaďte svazky a klikněte na **OK**.

7. V podokně **obor** rozbalte uzel **Hlasitost skupiny** . Novou skupinu hlasitost by se měly uzlu **Hlasitost skupiny** . 

8. Klikněte pravým tlačítkem myši na název skupiny hlasitost.

    - Začít interaktivní úlohy zálohování (na vyžádání), klikněte na **Přijmout záložní**. 

    - Pokud chcete naplánovat automatické zálohy, klikněte na **Vytvořit zásady zálohování**. Na kartě **Obecné** vyberte ze seznamu skupinu hlasitost. Na stránce **plán** zadejte podrobnosti plánu. Až budete hotovi, klikněte na **OK**. 

9. Potvrďte, že vaše úloha zálohování spustila rozbalte uzel **úlohy** v podokně **obor** a potom klikněte na uzel **spuštěný** . V podokně **výsledků** se zobrazí seznam aktuálně spuštěných úlohy. 

## <a name="configure-and-back-up-a-dynamic-mirrored-volume"></a>Konfigurace a obecnějším údajům dynamického zrcadlového svazku

Proveďte následující postup pro nastavení zálohování dynamická zrcadlený svazku:

- Krok 1: Použití nástroje Správa disků vytvořit dynamické zrcadlený hlasitost. 

- Krok 2: Použití StorSimple snímek Správce konfigurace zálohování.

### <a name="prerequisites"></a>Zjistit předpoklady pro

Než začnete:

- Ujistěte se, správně nakonfigurované StorSimple zařízení a hostitele počítač. Další informace přejděte na [zařízení StorSimple v místním nasazení](storsimple-deployment-walkthrough-u2.md).

- Nainstalujte a nakonfigurujte StorSimple snímek správce. Další informace přejděte na [Správce StorSimple snímek nasazení](storsimple-snapshot-manager-deployment.md).

- Konfigurace dva svazky na StorSimple zařízení. (V příkladech dostupné svazky jsou **disku 1** a **2 disku**). 

### <a name="step-1-use-disk-management-to-create-a-dynamic-mirrored-volume"></a>Krok 1: Použití disku správy vytvoření dynamického zrcadlového svazku

Správa disků je systémový nástroj pro správu pevných discích a svazky nebo oddílů, které obsahují. Další informace o správě disku přejděte na [Správa disků](https://technet.microsoft.com/library/cc770943.aspx) na webu Microsoft TechNet.

#### <a name="to-create-a-dynamic-mirrored-volume"></a>Vytvoření dynamického zrcadlového svazku

1. Vyberte některou z následujících možností spuštění Správa disků: 

   - Otevřete okno **Spustit** , zadejte **Diskmgmt.msc**a stiskněte klávesu Enter.

   - Spusťte správce serveru, rozbalte uzel **úložiště** a potom klikněte na **Správa disku**. 

   - Spuštění **Nástroje pro správu**, rozbalte uzel **Správa počítače** a potom klikněte na **Správa disku**. 

2. Ujistěte se, že máte k dispozici dva svazky na zařízení StorSimple. (V tomto příkladu dostupné svazky jsou **disku 1** a **Disk 2**.) 

3. V okně Správa disků v pravém sloupci v dolním podokně klikněte pravým tlačítkem **1 disku** a vyberte **Nové zobrazuje zrcadlově hlasitost**. 

    ![Nové zrcadlený hlasitosti](./media/storsimple-snapshot-manager-manage-volumes/HCS_SSM_New_mirrored_volume.png) 

4. Na stránce **Nové zobrazuje zrcadlově hlasitost** průvodce klikněte na **Další**.

5. Na stránce **Vyberte disků** vyberte **disku 2** v podokně **vybrané** , klikněte na tlačítko **Přidat**a klikněte na tlačítko **Další**. 

6. Na stránce **přiřadit písmeno jednotky nebo cestu** přijměte výchozí hodnoty a klikněte na tlačítko **Další**. 

7. Na stránce **Hlasitost formát** vyberte v poli **Velikost jednotky přidělení** **64 kB**. Zaškrtněte políčko **Rychlé formátování** a potom na tlačítko **Další**. 

8. Na stránce **dokončení nového zobrazuje zrcadlově hlasitost** zkontrolovat nastavení a klikněte na tlačítko **Dokončit**. 

9. Vyznačení, že základní disk se převedou na dynamický zobrazí se zpráva. Klikněte na **Ano**.

    ![Dynamický převodu zprávy](./media/storsimple-snapshot-manager-manage-volumes/HCS_SSM_Disk_management_msg.png) 

10. V části Správa disku ověřte, že Disk 1 a 2 disku se zobrazí jako dynamické zrcadlové svazky. (**Dynamické** objevit ve sloupci Stav a Barva pruhu kapacita změňte na červené označující zrcadlený hlasitost.) 

    ![Dynamické disků disku správy zobrazuje zrcadlově](./media/storsimple-snapshot-manager-manage-volumes/HCS_SSM_Verify_dynamic_disks_2.png) 
 
### <a name="step-2-use-storsimple-snapshot-manager-to-configure-backup"></a>Krok 2: Použití StorSimple snímek Správce konfigurace zálohování

Použijte následující postup konfigurace dynamického zrcadlového svazku a potom spustit zálohy okamžitě nebo vytvoření zásad pro plánované zálohování.

#### <a name="to-configure-backup-of-a-dynamic-mirrored-volume"></a>Konfigurace zálohování dynamická zrcadlený svazku

1. Klikněte na ikonu StorSimple snímek správce na ploše. Zobrazí se okno Správce StorSimple snímek. 

2. V podokně **obor** klikněte pravým tlačítkem myši na uzel **svazky** a vyberte **Prohledat svazky**. Po dokončení vyhledávání seznam svazky objevit v podokně **výsledků** . Dynamické zrcadlený hlasitost je uvedená jako jeden hlasitost. 

3. V podokně **výsledků** klikněte pravým tlačítkem dynamické zrcadlený hlasitost a potom klikněte na **Vytvořit skupinu hlasitost**. 

4. **V dialogovém okně **Vytvořit skupinu hlasitost** zadejte název skupiny hlasitost a přiřadit dynamického zrcadlového svazku této skupině.** 

5. V podokně **obor** rozbalte uzel **Hlasitost skupiny** . Novou skupinu hlasitost by se měly uzlu **Hlasitost skupiny** . 

6. Klikněte pravým tlačítkem myši na název skupiny hlasitost. 

    - Začít interaktivní úlohy zálohování (na vyžádání), klikněte na **Přijmout záložní**. 

    - Pokud chcete naplánovat automatické zálohy, klikněte na **Vytvořit zásady zálohování**. Na kartě **Obecné** vyberte ze seznamu skupinu hlasitost. Na stránce **plán** zadejte podrobnosti plánu. Až budete hotovi, klikněte na **OK**. 

7. Úlohy zálohování můžete sledovat, jak ho spustí. V podokně **obor** rozbalte uzel **úloh** a potom klikněte na **systém**, podrobnosti projektu se zobrazí v podokně **výsledků** . Po dokončení úloh zálohování podrobnosti se převedou na seznam **poslední 24** hodin projektu. 

## <a name="next-steps"></a>Další kroky

- Zjistěte, jak lze [pomocí Správce snímek StorSimple ke správě StorSimple řešení](storsimple-snapshot-manager-admin.md).
- Zjistěte, jak lze [pomocí Správce snímek StorSimple k vytváření a Správa skupin hlasitost](storsimple-snapshot-manager-manage-volume-groups.md).

<!--Reference links-->
[1]: https://msdn.microsoft.com/library/ee338480(v=ws.10).aspx
