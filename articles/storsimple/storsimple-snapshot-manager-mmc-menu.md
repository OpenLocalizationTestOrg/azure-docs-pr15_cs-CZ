<properties 
   pageTitle="Akcí z nabídky Správce snímek StorSimple MMC | Microsoft Azure"
   description="Popisuje, jak správce standardní nabídka Akce Microsoft Management Console (MMC) v StorSimple snímek."
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
   ms.date="04/25/2016"
   ms.author="v-sharos" />

# <a name="use-the-mmc-menu-actions-in-storsimple-snapshot-manager"></a>Správce MMC nabídka Akce ve StorSimple snímku

## <a name="overview"></a>Základní informace

Ve Správci snímek StorSimple zobrazí se následující akce uvedená na všechny akce nabídky a všechny změny v podokně **akcí** . 

- Zobrazení
- Nové okno postupovat dál? 
- Aktualizace 
- Export seznamu 
- Pomoc 

Tyto akce jsou součástí Microsoft Management Console (MMC) a nejsou specifické pro nástroj StorSimple snímek správce. Tento kurz popisuje tyto akce a vysvětluje, jak používat každý z nich ve Správci StorSimple snímek.

## <a name="view"></a>Zobrazení

Možnost **zobrazení** můžete změnit zobrazení podokna **výsledků** a změňte zobrazení okna konzoly. 

#### <a name="to-change-the-results-pane-view"></a>Pokud chcete změnit zobrazení podokna výsledků

1. Klikněte na ikony na ploše spuštění StorSimple snímek správce.

2. V podokně **obor** klikněte pravým tlačítkem na libovolný uzel nebo rozbalte uzel klikněte pravým tlačítkem myši na položku v podokně **výsledků** a potom klikněte na požadovanou možnost **zobrazení** . 

3. Pokud chcete přidat nebo odebrat sloupce, které se zobrazí v podokně **výsledků** , klikněte na **Přidat nebo odebrat sloupce**. Zobrazí se dialogové okno **Přidat nebo odebrat sloupce** .

    ![Přidání nebo odebrání sloupců z podokna výsledků](./media/storsimple-snapshot-manager-mmc-menu/HCS_SSM_Add_remove_columns.png) 

4. Vyplňte formulář následujícím způsobem:

    - Vyberte položky ze seznamu **dostupné** sloupce a klikněte na tlačítko **Přidat** si je přidat do seznamu **zobrazené sloupce** . 

    - Klikněte na položky v seznamu **zobrazené sloupce** a klikněte na **Odebrat** ze seznamu odebrat. 

    - Vyberte nějakou položku v seznamu **zobrazí** sloupce a klikněte na **Přesunout nahoru** nebo **Dolů** položku přesunout nahoru nebo dolů v seznamu. 

    - Klikněte na **Obnovit výchozí nastavení** se vraťte do výchozí konfigurace podokno **výsledky** . 

5. Až skončíte s výběrem položek, klikněte na **OK**. 

#### <a name="to-change-the-console-window-view"></a>Pokud chcete změnit zobrazení okna konzoly

1. Klikněte na ikony na ploše spuštění StorSimple snímek správce.

2. V podokně **obor** klikněte pravým tlačítkem na libovolný uzel, klikněte na kartu **zobrazení**a pak klikněte na **Přizpůsobit**. Zobrazí se dialogové okno **Přizpůsobit** .

    ![Přizpůsobení okna konzoly](./media/storsimple-snapshot-manager-mmc-menu/HCS_SSM_Customize.png) 

3. Zaškrtněte nebo zrušte zaškrtnutí políček zobrazit nebo skrýt položky v tomto okně. Až skončíte s výběrem položek, klikněte na **OK**.

## <a name="new-window-from-here"></a>Nové okno postupovat dál?

Možnost **Nové okno** slouží k otevření nového okna.

#### <a name="to-open-a-new-console-window"></a>Otevření nového okna

1. Klikněte na ikony na ploše spuštění StorSimple snímek správce.

2. V podokně **obor** klikněte pravým tlačítkem na libovolný uzel a potom klikněte na **Nové okno**. 

    Zobrazí se nové okno zobrazující pouze obor, že jste vybrali. Například pokud pravým tlačítkem myši na uzel **Záložní zásad** , nové okno se zobrazí pouze uzel **Záložní zásady** podokna **rozsahu** a seznam definovaný záložní zásad v podokně **výsledků** . Viz následující příklad.

    ![Nové okno postupovat dál?](./media/storsimple-snapshot-manager-mmc-menu/HCS_SSM_NewWindow.png) 
 
## <a name="refresh"></a>Aktualizace

Akce **Aktualizovat** slouží k aktualizaci okna konzoly.

#### <a name="to-update-the-console-window"></a>Aktualizace okna konzoly

1. Klikněte na ikony na ploše spuštění StorSimple snímek správce.

2. V podokně **obor** klikněte pravým tlačítkem na libovolný uzel nebo rozbalte a klikněte pravým tlačítkem myši na položku v podokně **výsledků** a potom klikněte na **Aktualizovat**. 

## <a name="export-list"></a>Export seznamu

Pomocí akce **Exportovat seznam** můžete uložit do seznamu v souboru textový soubor s oddělovači (CSV). Například můžete exportovat seznam záložní zásad nebo záložní katalogu. Soubor CSV pak můžete importovat do s tabulkovou aplikací pro analýzu.

#### <a name="to-save-a-list-in-a-comma-separated-value-csv-file"></a>Chcete-li seznam uložit do souboru textový soubor s oddělovači (CSV)

1. Klikněte na ikony na ploše spuštění StorSimple snímek správce. 

2. V podokně **obor** klikněte pravým tlačítkem na libovolný uzel nebo rozbalte a klikněte pravým tlačítkem myši na položku v podokně **výsledků** a potom klikněte na **Exportovat seznam**. 

3. Zobrazí se dialogové okno **Exportovat seznam** . Vyplňte formulář následujícím způsobem: 

    1. Do pole **název souboru** zadejte název souboru CSV nebo klikněte na šipku a vyberte z rozevíracího seznamu.

    2. V dialogovém okně **Uložit jako typ** klikněte na šipku a vyberte z rozevíracího seznamu Typ souboru.

    3. Uložit pouze vybrané položky, vyberte řádky a zaškrtněte políčko **Uložit pouze vybrané řádky** . Pokud chcete uložit všechny exportované seznamy, zrušte zaškrtnutí políčka **Uložit pouze vybrané řádky** .

    4. Klikněte na **Uložit**.

    ![Export seznamu do souboru CSV](./media/storsimple-snapshot-manager-mmc-menu/HCS_SSM_Export_List.png) 
 
## <a name="help"></a>Pomoc

Chcete-li zobrazit dostupné online nápověda pro správce snímek StorSimple a konzoly MMC můžete nabídku **Nápověda** .

#### <a name="to-view-available-online-help"></a>Chcete-li zobrazit dostupné online nápověda

1. Klikněte na ikony na ploše spuštění StorSimple snímek správce.

2. V podokně **obor** klikněte pravým tlačítkem na libovolný uzel nebo rozbalte uzel a klikněte pravým tlačítkem myši na položku v podokně **výsledků** a potom klikněte na **Nápověda**. 

## <a name="next-steps"></a>Další kroky

- Další informace o [Správci snímek StorSimple uživatelského rozhraní](storsimple-use-snapshot-manager.md).
- Další informace o [použití Správce snímek StorSimple ke správě StorSimple řešení](storsimple-snapshot-manager-admin.md).
