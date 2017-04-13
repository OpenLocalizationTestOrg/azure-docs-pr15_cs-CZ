<properties 
   pageTitle="Správce snímek StorSimple uživatelské rozhraní | Microsoft Azure"
   description="Popisuje uživatelské rozhraní StorSimple snímek správce a vysvětluje, jak se používá ke správě úlohy zálohování a zálohování katalogu."
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

# <a name="storsimple-snapshot-manager-user-interface"></a>Správce snímek StorSimple uživatelské rozhraní

## <a name="overview"></a>Základní informace

Správce snímek StorSimple má intuitivní uživatelské rozhraní, můžete převzít a spravovat zálohy. Tento kurz obsahuje úvod do uživatelského rozhraní a potom vysvětluje, jak používat každý z součásti. Podrobný popis Manager snímek StorSimple najdete v článku [Co je správce snímek StorSimple?](storsimple-what-is-snapshot-manager.md)

### <a name="console-description"></a>Popis konzoly

Uživatelské rozhraní zobrazíte kliknutím na ikonu StorSimple snímek správce na ploše. Zobrazí se okno konzoly, jak je znázorněno na následujícím obrázku.

![Správce snímek StorSimple příček](./media/storsimple-use-snapshot-manager/HCS_SSM_gui_panes.png)

Okno konzoly obsahuje pět hlavních prvky. Klikněte na příslušný odkaz pro podrobný popis jednotlivých prvků.

- [Řádek nabídek](#menu-bar) 
- [Panel nástrojů](#tool-bar) 
- [Podokno přehledu](#scope-pane) 
- [Podokno výsledků](#results-pane) 
- [Podokno Akce](#actions-pane) 

Kromě toho správce snímek StorSimple podporuje [Navigace pomocí klávesnice a řadu zástupců](#keyboard-navigation-and-shortcuts).

### <a name="console-accessibility"></a>Usnadnění konzoly

Uživatelské rozhraní StorSimple snímek správce podporuje přístupnost poskytovaných operačního systému Windows a Microsoft Management Console (MMC), jakož i některé specifické pro správce snímek StorSimple klávesové zkratky. 

- Popis funkce usnadnění systému Windows přejděte na [klávesové zkratky pro Windows](https://support.microsoft.com/kb/126449). 

- Popis funkcí usnadnění MMC přejděte na [přístupnost pro MMC 3.0](https://technet.microsoft.com/library/cc766075.aspx)

- Popis funkcí usnadnění StorSimple snímek správce přejděte do [Navigace pomocí klávesnice a klávesové zkratky](#keyboard-navigation-and-shortcuts).

## <a name="menu-bar"></a>Řádek nabídek

Řádek nabídek v horní části okna konzoly obsahuje [soubor](#file-menu) [Akce](#action-menu), [zobrazení](#view-menu), [Oblíbené položky](#favorites-menu), [okno](#window-menu)a [pomoct](#help-menu) nabídky.

Klikněte na libovolnou položku na řádku nabídek zobrazíte seznam dostupných příkazů, které v nabídce. Následující příklad nabídka **zobrazení** v řádku nabídek.

![Vybraná nabídka zobrazení](./media/storsimple-use-snapshot-manager/HCS_SSM_View_menu.png)

### <a name="file-menu"></a>Nabídka Soubor

Nabídka **soubor** obsahuje standardní příkazy Microsoft Management Console (MMC).

#### <a name="menu-access"></a>Přístup k nabídce

Pokud chcete zobrazit nabídku **soubor** , klikněte na **soubor** na řádek nabídek. Zobrazí se následující nabídka.

![Nabídka Soubor StorSimple snímek správce](./media/storsimple-use-snapshot-manager/HCS_SSM_FileMenu.png) 

#### <a name="menu-description"></a>Nabídka popis

Následující tabulka popisuje položky, které se zobrazí v nabídce **soubor** .

| Položka nabídky | Popis |
|:----------|:-------------|
| Nový       | Klikněte na **Nový** k vytvoření nové konzoly založené na snímku správci StorSimple. |
| Otevřít      | Klikněte na **Otevřít** otevřete stávající konzoly. |
| Uložení      | Klepněte na tlačítko **Uložit** uložte konzole aktuální. |
| Uložit jako   | Klikněte na **Uložit jako** a vytvořte nový, přejmenované instanci konzole aktuální. Možnost **Uložit jako** slouží k přizpůsobení zobrazení a uložit pro pozdější načítání. Můžete například vytvořit StorSimple snímek správce moduly, které ukazují konkrétní servery. |
| Přidat nebo odebrat modulu Snap-in | Klikněte na **Přidat nebo odebrat modulu Snap-in** k přidání nebo odebrání modulu snap in a uspořádání uzlů v podokně **obor** . Další informace přejděte na [Přidat, odebrat a uspořádat modulu Snap in a rozšíření v MMC 3.0](https://technet.microsoft.com/library/cc722035.aspx). |
| Možnosti   | Klikněte na **Možnosti** změnit ikonu konzoly, zadat režimů přístupu uživatelů a oprávnění a odstraňovat soubory konzoly zvětšíte volného místa na disku. |
| Seznam cesty k souboru | Klikněte na cesty v číslovaném seznamu znovu otevřete soubor, který jste nedávno otevřeli. |
| Ukončení      | Klikněte na **Konec** ukončíte nabídky **soubor** . |
 
### <a name="action-menu"></a>Nabídka Akce

Pomocí nabídky **Akce** vyberte z dostupných akcí. Dostupné položky, závisí na výběr, které provedete v okně **obor** nebo **výsledky** .

#### <a name="menu-access"></a>Přístup k nabídce

Zobrazení v nabídce **Akce** , proveďte jednu z následujících akcí:

- Klikněte pravým tlačítkem myši na položku v okně **obor** nebo **výsledky** .

- Vyberte nějakou položku v okně **obor** nebo **výsledky** a pak na řádku nabídek klikněte na tlačítko **Akce** . 

Například pokud v podokně **obor** vyberte na nejvyšší uzel a pak klikněte pravým tlačítkem myši nebo klikněte na tlačítko **Akce** v řádku nabídek, se zobrazí následující nabídky.
 
![Nabídka Akce StorSimple snímek správce](./media/storsimple-use-snapshot-manager/HCS_SSM_Action_menu.png)

Podokno **Akce** (na pravé straně konzole) obsahuje stejný seznam akcí jako v nabídce **Akce** . V podokně **akcí** obsahuje také možnosti nabídky **zobrazení** , které umožňují vytvářet vlastní zobrazení v podokně **výsledků** .

![Otevřete podokno akce se nabídka zobrazení](./media/storsimple-use-snapshot-manager/HCS_SSM_ActionsPane_Results.png)

#### <a name="menu-description"></a>Nabídka popis

Následující tabulka obsahuje abecední seznam StorSimple snímek správce akce. 

- Ve sloupci **Akce** jsou uvedeny kroky, kterými můžete dělat uzly a výsledky. 

- Sloupec **Navigace** vysvětluje, jak zobrazíte příslušný příkaz nabídky **Akce** tak, že vyberete požadovanou akci. Některé akce se zobrazí ve více nabídkách **Akce** . Tyto akce vyberte jednu možnost **Navigace** ze seznamu s odrážkami. 

- Sloupec **popisu** popisuje, jak používat každý akci v nabídce **Akce** nebo podokno akce a vysvětluje, jak funguje.

>[AZURE.NOTE] Podokno **Akce** a nabídky **Akce** obsahují další možnosti, třeba **zobrazení** **Nové okno**, **aktualizace**, **Exportovat seznam**a **Nápověda**. Tyto možnosti jsou k dispozici v rámci konzoly MMC a nejsou specifické pro nástroj StorSimple snímek správce. Tabulka obsahuje popis těhle možností.
 
| Akce  | Navigace  | Popis  |
|:--------|:------------|:-------------|
| Ověření | Klikněte na uzel **zařízení** a klikněte pravým tlačítkem myši na zařízení v podokně **výsledků** . | Klikněte na **Ověřit** k zadání hesla, které jste nakonfigurovali zařízení. |
| Klonovat  | Rozbalení **Záložní katalogu**, rozbalte **Cloudu snímky**, klikněte na ze zálohy a vyberte svazku v podokně **výsledků** . | Klikněte na **vytvořit kopii** chcete vytvořit kopii snímku cloudu a uložte ho do umístění, do kterého se určuje. |
| Nastavení zařízení | Klikněte pravým tlačítkem myši na uzel **zařízení** . | Klikněte na tlačítko **Konfigurace zařízení** pro nastavení jednoho nebo více zařízení pro připojení k hostiteli Windows. |
| Vytvoření záložní zásad | Udělejte jednu z následujících akcí:<ul><li>Klikněte pravým tlačítkem myši **záložní zásady**.</li><li>Klikněte na nebo rozbalit **Skupiny hlasitost**a kliknutím pravým tlačítkem hlasitosti skupiny.</li><li>Klikněte na nebo rozbalte **Katalog zálohování**a pak klikněte pravým tlačítkem na skupinu hlasitost.</li></ul> | Klikněte na **Vytvořit zásady zálohování** konfigurovat plánované zálohování pro skupinu hlasitost. |
| Vytvoření skupiny hlasitosti | Udělejte jednu z následujících akcí:<ul><li>Klikněte na uzel **svazky** a potom klikněte pravým tlačítkem hlasitosti v podokně **výsledků** .</li><li>Klikněte pravým tlačítkem myši na uzel **Hlasitost skupiny** .</li></ul> | Klikněte na **Vytvořit skupinu objem** , které chcete přiřadit svazky skupině hlasitost. |
| Odstranění | Klikněte na uzel nebo výsledek (Tato položka se zobrazí na mnoha nabídky **Akce** a podokna **akcí** .) | Klikněte na **Odstranit** odstraníte uzel nebo výsledek, který jste vybrali. Pokud se zobrazí dialogové okno potvrzení, potvrďte nebo zrušení odstranění. |
| Podrobnosti | Klikněte na uzel **zařízení** a pak klikněte pravým tlačítkem myši na zařízení v podokně **výsledků** . | Klikněte na **Podrobnosti** a zobrazit podrobnosti konfigurace pro zařízení. |
| Úpravy | Klikněte na položku **Zálohování zásady**a klikněte pravým tlačítkem myši zásadu v podokně **výsledků** . | Klikněte na **Upravit** změnit záložní časový plán pro skupinu hlasitost. |
| Export seznamu | Klikněte na uzel nebo výsledek (Tato položka se zobrazí na všech nabídky **Akce** a podokna **akcí** .) | Klikněte na **Exportovat seznam** uložit do seznamu v souboru textový soubor s oddělovači (CSV). Tento soubor pak můžete importovat do s tabulkovou aplikací pro analýzu. |
| Pomoc | Klikněte na uzel nebo výsledek. (Tato položka se zobrazí na všech nabídky **Akce** a podokna **akcí** .) | Klikněte na **Nápověda** zobrazíte online nápověda v samostatném okně prohlížeče. |
| Nové okno postupovat dál? | Klikněte na uzel nebo výsledek (Tato položka se zobrazí na všech nabídky **Akce** a podokna **akcí** .) | Klikněte na tlačítko **Nové okno** otevřete nové okno Správce StorSimple snímek.|
| Aktualizace | Klikněte na uzel nebo výsledek (Tato položka se zobrazí na všech nabídky **Akce** a podokna **akcí** .) | Klikněte na tlačítko **Aktualizovat** aktuálně zobrazeného okna StorSimple snímek správce. |
| Aktualizace zařízení | Klikněte na uzel **zařízení** a klikněte pravým tlačítkem myši na zařízení v podokně **výsledků** . | Klikněte na **Aktualizovat zařízení** synchronizovat konkrétní připojené zařízení s StorSimple snímek správce. |
| Aktualizace zařízení | Klikněte pravým tlačítkem myši na uzel **zařízení** . | Klikněte na **Aktualizovat zařízení** synchronizovat s aplikací StorSimple snímek Manager seznamu připojených zařízení. |
| Prohledat svazky | Klikněte pravým tlačítkem myši na uzel **svazky** . | Klikněte na **Prohledat svazky** aktualizujte seznam svazky, která se zobrazí v podokně **výsledků** . |
| Obnovení | Rozbalení **Záložní katalogu**rozbalte skupinu hlasitost, rozšíření **Místních snímky** nebo **Snímky cloudu**a klikněte pravým tlačítkem myši zálohy. | Klikněte na Nahradit aktuální seskupení dat hlasitost data z vybrané zálohy **Obnovit** . |
| Prohlédněte zálohování | Udělejte jednu z následujících akcí:<ul><li>Rozbalit **Skupiny hlasitost**a pak klikněte pravým tlačítkem na skupinu hlasitost.</li><li>Rozbalte **Katalog zálohování**a pak klikněte pravým tlačítkem na skupinu hlasitost.</li></ul> | Klikněte na tlačítko **Přijmout záložní** spuštění úlohy zálohování okamžitě. |
| Přepnout importy zobrazení | Klikněte pravým tlačítkem myši na nejvyšší uzel podokna **rozsahu** (uzel **StorSimple snímek správce** v příkladech). | Klikněte na **Přepnout importy zobrazení** k zobrazení nebo skrytí skupin hlasitost a přidružená zálohy importovaných na řídicím panelu Správce StorSimple služby. |

### <a name="view-menu"></a>Nabídka zobrazení

Vytvoření vlastního zobrazení obsahu podokně **výsledků** pomocí nabídky **Zobrazit** . Nabídka **zobrazení** obsahuje možnosti **Přidat nebo odebrat sloupce** a **Přizpůsobit** .

#### <a name="menu-access"></a>Přístup k nabídce

Přístup k nabídce **Zobrazit** na řádku nabídek nebo v podokně **akcí** .

![Nabídka zobrazení StorSimple snímek správce](./media/storsimple-use-snapshot-manager/HCS_SSM_View_menu.png) 

#### <a name="menu-description"></a>Nabídka popis

Následující tabulka popisuje položky, které se zobrazí na **kartě zobrazení** .

| Položka nabídky  | Popis |
|:-----------|:-------------|
| Přidat nebo odebrat sloupce | Klikněte na **Přidat nebo odebrat sloupce** k přidání nebo odebrání sloupců v podokně **výsledků** . |
| Přizpůsobení | Klikněte na **Přizpůsobit** zobrazení nebo skrytí položky v okně konzoly StorSimple snímek správce. |

### <a name="favorites-menu"></a>Oblíbené

Pomocí nabídky **Oblíbené položky** k přidání, odebrání a uspořádání zobrazení stránky a úkoly, které často používáte. 

#### <a name="menu-access"></a>Přístup k nabídce

Přístup k nabídce **Oblíbené položky** na řádku nabídek.

![Správce snímek StorSimple Oblíbené](./media/storsimple-use-snapshot-manager/HCS_SSM_FavoritesMenu.png)

#### <a name="menu-description"></a>Nabídka popis

Následující tabulka popisuje položky, které se zobrazí v nabídce **Oblíbené** .

| Položka nabídky |  Popis |
|:----------|:-------------|
| Přidat k oblíbeným položkám | Klikněte na **Přidat k oblíbeným položkám** přidáte aktuální zobrazení na seznam oblíbených položek. |
| Uspořádání oblíbených položek | Klikněte na **Uspořádat oblíbené položky** uspořádat obsah složky Oblíbené položky. |

### <a name="window-menu"></a>Nabídku okna

K přidání a změna uspořádání StorSimple snímek správce konzoly windows použijte nabídku **okna** .

#### <a name="menu-access"></a>Přístup k nabídce

Přístup k nabídce **okno** na řádek nabídek.

![Okno Správce snímek StorSimple nabídky](./media/storsimple-use-snapshot-manager/HCS_SSM_WindowMenu.png)

Číslovaný seznam v dolní části nabídky zobrazuje okénka, která jsou v současnosti otevřete. Klepněte na jakékoli okno v tomto seznamu do okna přenést do popředí. 

#### <a name="menu-description"></a>Nabídka popis

Následující tabulka popisuje položky, které se zobrazí v nabídce okno.

| Položka nabídky  | Popis |
|:-----------|:-------------|
| Nové okno | Klikněte na **Nové okno** pro otevření nového okna konzoly (kromě stávajícím okně). |
| Kaskádové   | Klikněte na tlačítko **kaskádové** zobrazíte otevřené konzoly windows v šablona stylů. |
| Vedle sebe ve vodorovném směru | Klikněte na tlačítko **Vedle sebe ve vodorovném směru** zobrazíte otevřené konzoly windows ve formátu dlaždice (nebo mřížky). |
| Ikony | Pokud máte víc konzoly windows otevřete a rozmístěny v počítači, je minimalizovat a potom klikněte na **Seřadit ikony** a uspořádejte je do vodorovné řádku v dolní části obrazovky. |

### <a name="help-menu"></a>Nabídka Nápověda

Umožňuje zobrazit dostupné online nápověda pro správce snímek StorSimple a konzoly MMC nabídku **Nápověda** . Můžete taky zobrazit informace o MMC a správce snímek StorSimple verze software nainstalovaných v počítači. 

Přístup k nabídce **Nápověda** na řádek nabídek. Témata nápovědy pro správce snímek StorSimple se dostanete z podokna **akcí** .

![Nápověda ke Správci snímek StorSimple nabídky](./media/storsimple-use-snapshot-manager/HCS_SSM_HelpMenu.png)

#### <a name="menu-description"></a>Nabídka popis

Následující tabulka popisuje položky, které se zobrazí v nabídce Nápověda.

| Položka nabídky  | Popis  |
|:-----------|:-------------|
| Nápověda pro správce StorSimple snímku | Klepněte **na StorSimple snímek vedoucímu pomoct** otevřete StorSimple snímek nápovědy v samostatném okně. |
| Témata nápovědy |Klikněte na **témata nápovědy** k nápovědě MMC online v samostatném okně. |
| Web TechCenter o webu | Klikněte na **webu TechCenter** otevřete domovskou stránku Microsoft TechNet Tech Center v samostatném okně. |
| Microsoft Management Console | Klikněte na **Microsoft Management Console** jakou verzi systému Microsoft Management Console je nainstalovaný v počítači. |
| Informace o StorSimple snímek správce | Klikněte na **Informace o StorSimple snímek správce** jakou verzi modulu snap-in je nainstalovaný v počítači. |

## <a name="tool-bar"></a>Panel nástrojů

Panelu nástrojů umístěné pod panelem nabídek obsahuje ikony navigace a úkol. Každé ikony je zástupcem s konkrétním úkolem.

### <a name="icon-descriptions"></a>Popisy ikon

Následující tabulka popisuje ikony, které se zobrazí na panelu nástrojů. 

| Ikona  | Popis  |
|:------|:-------------| 
| ![Šipka vlevo](./media/storsimple-use-snapshot-manager/HCS_SSM_LeftArrow.png) | Klikněte na šipku vlevo se vrátíte na předchozí stránku. |
| ![Šipka vpravo](./media/storsimple-use-snapshot-manager/HCS_SSM_RightArrow.png) | Klikněte na šipku doprava přejdete na další stránku (je-li na šipku šedé, akce není k dispozici). |
| ![Ikona nahoru](./media/storsimple-use-snapshot-manager/HCS_SSM_Up.png) | Klikněte na ikonu nahoru přejít o jednu úroveň ve stromové (podokno **obor** ). |
| ![Zobrazit nebo skrýt stromové](./media/storsimple-use-snapshot-manager/HCS_SSM_ShowConsoleTree.png) | Klepnutím na ikonu Zobrazit/skrýt konzoly stromového zobrazení nebo skrytí podokna **rozsahu** . |
| ![Export seznamu](./media/storsimple-use-snapshot-manager/HCS_SSM_ExportListIcon.png) | Kliknutím na ikonu seznamu exportu export seznamu do souboru CSV, který určíte. |
| ![Ikona Nápověda](./media/storsimple-use-snapshot-manager/HCS_SSM_HelpIcon.png)  |Klikněte na ikonu Nápověda otevřete online téma nápovědy MMC. |
| ![Zobrazit nebo skrýt podokno akce](./media/storsimple-use-snapshot-manager/HCS_SSM_ShowAction.png) | Klikněte na tlačítko Zobrazit/skrýt podokno ikonu **Akce** pro zobrazení nebo skrytí podokna **akcí** . 
 
## <a name="scope-pane"></a>Podokno přehledu

V podokně **obor** je podokně vlevo v uživatelském rozhraní StorSimple snímek správce. Obsahuje stromu console (nebo uzel) a je mechanismus primární navigaci pro StorSimple snímek správce. 
 
### <a name="scope-pane-structure"></a>Obor podokno struktury

V podokně **obor** obsahuje řadu možností kliknutí objektů (uzly) uspořádána ve stromové struktuře. 

![Podokno přehledu](./media/storsimple-use-snapshot-manager/HCS_SSM_Scope_pane.png) 

- Pokud chcete rozbalit nebo Sbalit uzel, klikněte na šipku vedle názvu uzel.

- Stav nebo obsahu uzel zobrazíte kliknutím na název uzlu. Informace se zobrazí v podokně **výsledků** . 

Podokno **obor** obsahuje následující uzly: 

- [Zařízení](#devices-node) 
- [Svazky uzel](#volumes-node) 
- [Hlasitost skupiny uzel](#volume-groups-node) 
- [Zálohování uzel Zásady](#backup-policies-node) 
- [Zálohování uzel katalogu](#backup-catalog-node) 
- [Úlohy uzel](#jobs-node) 

### <a name="scope-pane-tasks"></a>Obor podokně úkoly

Použití podokna **rozsahu** k dokončení akce určitého uzlu. Vyberte úkol, proveďte jednu z následujících akcí:

- Klikněte pravým tlačítkem myši na uzel a pak vyberte úkol v nabídce, která se zobrazí.

- Klikněte na uzel a potom na řádku nabídek klikněte na tlačítko **Akce** . Vyberte úkol v nabídce, která se zobrazí.

- Klikněte na uzel a vyberte požadovanou akci v podokně **akcí** .

Když vyberte uzel a vyberte některou z těchto postupů zobrazíte seznam úkolů, zobrazí se pouze akce, které lze provádět na uzel.

### <a name="devices-node"></a>Zařízení

Uzel **zařízení** představuje StorSimple zařízení a StorSimple virtuální zařízení připojených k StorSimple snímek správce. Vyberte tento uzel pro připojení a nastavení zařízení a importujte jeho související svazky svazky skupiny a existující záložní kopie. Jednoho hostitele můžete připojených víc zařízení.

- Rozbalte uzel, klikněte na šipku vedle **zařízení**.

- Nabídce dostupných akcí, klikněte pravým tlačítkem myši na uzel **zařízení** nebo klepněte pravým tlačítkem myši, které se zobrazí v zobrazení rozbaleného uzlů.

- Seznam nakonfigurované zařízení zobrazíte kliknutím na **zařízení** v podokně **obor** . V podokně **výsledků** se zobrazí seznam zařízení společně s informacemi o každém zařízení.

### <a name="volumes-node"></a>Svazky uzel

Uzel **svazky** představuje jednotky, které odpovídají na objemy připevněna hostiteli, včetně těch, které zjistil prostřednictvím iSCSI a můžou být zjistil prostřednictvím zařízení. Tento uzel umožňuje zobrazit seznam k dispozici objemu a přiřadit jednotlivé svazky skupinám hlasitost.

- Rozbalte uzel, klikněte na šipku vedle **svazky**.

- Nabídce dostupných akcí, klikněte pravým tlačítkem myši na uzel **svazky** nebo klikněte pravým tlačítkem myši, které se zobrazí v zobrazení rozbaleného uzlů.

- Seznam svazky zobrazíte kliknutím na **svazky** v podokně **obor** . V podokně **výsledků** se zobrazí seznam svazky společně s informací o každém svazku.

### <a name="volume-groups-node"></a>Hlasitost skupiny uzel

Hlasitost skupiny jsou označovaná taky jako konzistence skupiny. Každá skupina hlasitost je fondu svazky týkající se aplikace, která vám pomůže zajistit soulad aplikace během operace zálohování. Umožňuje uzel **Hlasitost skupin** pro nastavení těchto skupin a vzít interaktivní zálohy nebo vytvořte záložní plány. 

- Rozbalte uzel, klikněte na šipku vedle **Skupiny hlasitost**.

- Nabídce dostupných akcí, klikněte pravým tlačítkem myši na uzel **Hlasitost skupiny** nebo klikněte pravým tlačítkem myši, které se zobrazí v zobrazení rozbaleného uzlů.

- Seznam skupin hlasitost zobrazíte kliknutím na **Hlasitost skupiny** v podokně **obor** . V podokně **výsledků** se zobrazí seznam skupin hlasitost společně s informace o skupinách hlasitost.

### <a name="backup-policies-node"></a>Zálohování uzel Zásady

Zálohování zásad jsou plány projektu u místních i cloudových snímky. Použití uzel **Záložní zásady** můžete určit, jak často se vytvoří zálohy a jak dlouho zálohy měl být zachován. 

- Rozbalte uzel, klikněte na šipku vedle **Zásady zálohování**.

- Nabídce dostupných akcí, klikněte pravým tlačítkem myši na uzel **Zálohování zásady** nebo klikněte pravým tlačítkem na jednotlivých uzlech, které se zobrazí v zobrazení rozbaleného.

- Seznam záložní zásady zobrazíte kliknutím na **Záložní zásady** v podokně **obor** . V podokně **výsledků** se zobrazí seznam záložní zásady společně s informacemi o každého zásadu.

>[AZURE.NOTE] Můžete zachovat maximálně 64 zálohy.


### <a name="backup-catalog-node"></a>Zálohování uzel katalogu

**Katalog zálohování** uzel obsahuje seznamy na místě a mimo zálohování objemu Azure StorSimple. Tento uzel uspořádaný podle objemu skupiny a každý kontejner skupiny hlasitost obsahuje samostatné struktury pro místní snímky (uzel s **Místní snímku**) a cloudu snímků (uzly **Cloudu snímky** ). Po rozbalení každý kontejner skupiny hlasitost seznamy úspěšné zálohy, které byly odebrány interaktivně nebo nakonfigurované zásady.

- Rozbalte uzel, klikněte na šipku vedle **Katalog zálohování**.

- Nabídce dostupných akcí, klikněte pravým tlačítkem myši na uzel **Katalog zálohování** nebo klikněte pravým tlačítkem myši, které se zobrazí v zobrazení rozbaleného uzlů.

- Seznam záložní snímky zobrazíte kliknutím na **Záložní katalogu** v podokně **obor** . V podokně **výsledků** se zobrazí seznam snímky a informace o jednotlivých snímku.

### <a name="local-snapshots-node"></a>Místní uzel snímky

**Místní snímky** uzel seznamy místní snímků pro skupinu konkrétní hlasitost. Uzel se nachází v části **Katalog zálohování** uzel v podokně **obor** . Místní snímky jsou v okamžiku kopií hlasitost data, která jsou uložená v Azure StorSimple zařízení. Tento typ zálohování obvykle lze vytvořit a rychle obnovit. Místní snímku můžete použít jako místní záložní kopie.

- Rozbalte uzel, klikněte na šipku vedle **Místní snímky**.

- Nabídce dostupných akcí, klikněte pravým tlačítkem myši na uzel **Místní snímky** nebo klikněte pravým tlačítkem myši, které se zobrazí v zobrazení rozbaleného uzlů.

- Seznam místní snímky zobrazíte kliknutím na **Místní snímky** v podokně **obor** . V podokně **výsledků** se zobrazí seznam snímky a informace o jednotlivých snímku.

### <a name="cloud-snapshots-node"></a>Shluk snímky uzel

Uzel **Cloudu snímky** seznam snímky cloudu pro skupinu konkrétní hlasitost. Uzel se nachází v části **Katalog zálohování** uzel v podokně **obor** . Snímky cloudu jsou v okamžiku kopií hlasitost dat uložených v cloudu. Snímek cloudu odpovídá snímek replikovat na jiné, mimo úložiště systému. Snímky cloudu jsou užitečné zejména v případech obnovení havárie.

- Rozbalte uzel, klikněte na šipku vedle položky **Snímky cloudu**.

- Nabídce dostupných akcí, klikněte pravým tlačítkem myši na uzel **Cloudu snímky** nebo klikněte pravým tlačítkem na jednotlivých uzlech, které se zobrazí v zobrazení rozbaleného.

- Seznam cloudu snímky zobrazíte kliknutím na **Snímky cloudu** v podokně **obor** . V podokně **výsledků** se zobrazí seznam snímky a informace o jednotlivých snímku.

### <a name="jobs-node"></a>Úlohy uzel

**Úlohy** uzel obsahuje informace o plánované pracovního a naposledy dokončené úlohy zálohování. 

- Rozbalte uzel, klikněte na šipku vedle **úlohy**.

- Nabídce dostupných akcí, klikněte pravým tlačítkem myši na uzel **úlohy** nebo klikněte pravým tlačítkem myši, které se zobrazí v zobrazení rozbaleného uzlů.

- Zobrazíte seznam naplánovaných úloh rozbalte uzel **úlohy** a klikněte na **Naplánované**. V podokně **výsledků** se zobrazí seznam dříve konfigurované úlohy a informace o jednotlivých úloh. 

- Zobrazíte seznam naposledy dokončené projekty rozbalte uzel **úlohy** a klikněte na **poslední 24 hodin**. V podokně **výsledků** se zobrazí seznam úloh, které byly dokončeny za posledních 24 hodin. Podokno **výsledků** obsahuje také informace o každé dokončení projektu.

- Zobrazíte seznam úloh, které jsou aktuálně spuštěných rozbalte uzel **úloh** a potom klikněte na **systém**. V podokně **výsledků** se zobrazí seznam aktuálně spuštěných úlohy a informace o jednotlivých úloh.

## <a name="results-pane"></a>Podokno výsledků

Podokno **výsledků** se v prostředním podokně uživatelského rozhraní StorSimple snímek správce. Obsahuje seznamy podrobných informací o stavu a pro uzel vybrané v podokně **obor** .

### <a name="example"></a>Příklad

Následující příklad zobrazíte kliknutím na uzel **Hlasitost skupiny** v podokně **obor** . Podokno **výsledků** zobrazí seznam skupin hlasitost s podrobnostmi o jednotlivých skupin.

![Podokno výsledků](./media/storsimple-use-snapshot-manager/HCS_SSM_Results_pane.png) 

Konfigurace informace zobrazené v podokně **výsledků** : klikněte pravým tlačítkem myši uzel v podokně **obor** , klikněte na kartu **zobrazení**a potom klikněte na **Přidat nebo odebrat sloupce**.

## <a name="actions-pane"></a>Podokno Akce

V podokně **akcí** je pravém podokně v uživatelském rozhraní StorSimple snímek správce. Nabídka operacích, které lze provádět na uzel, zobrazení nebo data, která vyberete v okně **obor** nebo **výsledky** v ní. V podokně **akcí** obsahuje stejné příkazy jako nabídky **Akce** , které jsou k dispozici pro položky v **oboru** a **výsledky** podokno. Popis jednotlivých akce najdete v tabulce v části nabídky **Akce** .

### <a name="examples"></a>Příklady

Viz následující příklad v podokně **obor** , rozbalte uzel **úlohy** a klepněte na **Naplánované**. Panel **akcí** zobrazuje dostupné akce pro uzel **Naplánované** .

![Příklad použití naplánovaných úloh v podokně akcí](./media/storsimple-use-snapshot-manager/HCS_SSM_ActionsPane.png) 

Zobrazit další možnosti v podokně **obor** rozbalte uzel **úlohy** , kliknutím na **Naplánované**a potom klikněte na úlohy v podokně **výsledků** . Panel **akcí** zobrazuje dostupné akce pro úlohy, jak je vidět v následujícím příkladu.

![Příklad použití akce úloh v podokně akcí](./media/storsimple-use-snapshot-manager/HCS_SSM_ActionsPane_Results.png)

## <a name="keyboard-navigation-and-shortcuts"></a>Navigace pomocí klávesnice a klávesové zkratky

Správce snímek StorSimple umožňuje funkce usnadnění ve operačního systému Windows a Microsoft Management Console (MMC). Obsahuje taky některé funkce navigace a klávesové zkratky, které jsou specifické pro správce snímek StorSimple podle popisu v následujících částech.
 
- [Klávesy pro navigaci na klávesnice](#keyboard-navigation-keys) 
- [Řádek nabídek klávesových zkratek](#menu-bar-shortcut-keys) 
- [Klávesové zkratky pro podokno oboru](#scope-pane-shortcut-keys) 

### <a name="keyboard-navigation-keys"></a>Klávesy pro navigaci na klávesnice

Následující tabulka popisuje klávesy, pomocí kterých můžete přejít uživatelského rozhraní StorSimple snímek správce. 

| Navigační klávesa  | Akce  |
|:----------------|:--------| 
| Šipka dolů | Použijte klávesy se šipkou dolů ve svislém směru přesunout na další položku v nabídce nebo v podokně. |
| Zadejte | Stisknutím klávesy Enter dokončete akce a potom přejděte k dalšímu kroku. Můžete například stisknutím klávesy Enter vyberte **Další**, **OK**nebo **vytvořit**a potom přejděte k dalšímu kroku v průvodci.|
| ESC | Stisknutím klávesy Esc zavřete nabídku nebo zrušit a zavřete stránku.|
| F1 | Stiskněte klávesu F1 zobrazíte téma nápovědy pro aktuálně aktivní okno.|
| F5 | Stisknutím klávesy F5 aktualizace uzel. |
| F6 | Stisknutím klávesy F6 přejděte do podokna **výsledky** v podokně **obor** .|
| F10 | Stisknutím klávesy F10 přejděte na řádek nabídek. |
| Šipka vlevo | Pomocí klávesy šipka vlevo přejděte ve vodorovném směru z nabídky Možnosti panel na předchozí možnost. Přesunutí na předchozí položku na řádku nabídek v nabídce Akce (nebo kontext) pro předchozí položku nabídne. |
| Šipka vpravo | Pomocí klávesy šipka vpravo přejdete vodorovně z jedné nabídky úsečku na další. Při přechodu na další položku na řádku nabídek, zobrazí se nabídka Akce (nebo kontext) pro nové položky.
| Klávesy TAB | Pomocí klávesy Tab přesuňte do podokna Další na konzole nebo další výběru textového pole nebo na stránce. |
| Klávesa Šipka nahoru | Šipka nahoru přejdete pomocí svisle na předchozí položku v nabídce nebo podokna. |

### <a name="menu-bar-shortcut-keys"></a>Řádek nabídek klávesových zkratek

Následující tabulka popisuje kombinace klávesových zkratek pro řádek nabídek. Po stisknutím klávesové zkratky a v nabídce otevře, můžete pomocí klávesové zkratky pro nabídku (podtržené klíče na nabídku). Další informace o řádek nabídek přejděte na [řádek nabídek](#menu-bar).

| Zástupce | Výsledek                    | Klávesa místní nabídky | Výsledek          |
|:---------|:--------------------------|:------------------|:----------------|
| ALT + F    | Otevře nabídku **soubor** .  | N | Otevře se nové instance konzoly.   |
|          |                           | O | Otevře se stránka **Nástroje pro správu** . |
|          |                           | S | Uloží konzole StorSimple snímek správce.|
|          |                           | A | Otevře se stránka **Uložit jako** . |
|          |                           | M | Otevře stránku **modulu Snap-in Přidat/odebrat** .|
|          |                           | P | Otevře se stránka **Možnosti** . |
|          |                           | H | Otevře nápovědu.|
| ALT + A    | Otevře nabídku **Akce** .| Můžu | Zapnutí a vypnutí: Zapne zobrazení možnost importovat.|
|          |                           | W | Otevře se nové konzoly StorSimple snímek správce.|
|          |                           | F | Aktualizuje konzole StorSimple snímek správce.|
|          |                           | L | Otevření stránky **Exportovat** . 
|          |                           | H | Otevře nápovědu.|
| KOMBINACE KLÁVES ALT + Z    | Otevře nabídku **Zobrazit** .  | A | Otevře se stránka **Přidat nebo odebrat sloupce** . |
|          |                           | U | Otevře stránku **Vlastní nastavení zobrazení** . |
| ALT + O    | Otevře nabídku **Oblíbené položky** . | A | Otevře se stránka **Přidat k oblíbeným položkám** . |
|          |                           | O | Otevře stránku **Uspořádat oblíbené položky** .|
| ALT + W    | Otevře nabídku **okna** .| N | Otevře okno jiné StorSimple snímek správce.|
|          |                           | C | Zobrazí všechny otevřené konzoly windows šablona stylů.|
|          |                           | T | Zobrazí všechny otevřené konzoly windows ve vzorci mřížky. |
|          |                           | Můžu | Seřadí ikony ve vodorovném řádku v dolní části obrazovky.|
| ALT + H    | Otevře nabídku **Nápověda** .  | H | Otevře nápovědu.|
|          |                           | T | Otevře webovou stránku společnosti Microsoft TechNet Tech Center.|
|          |                           | A | Otevře se stránka **Microsoft Management Console** . |
 
### <a name="scope-pane-shortcut-keys"></a>Klávesové zkratky pro podokno oboru

V následujících tabulkách zobrazit zástupce kombinace kláves pro každý uzel v podokně **obor** . 

- [Klávesové zkratky pro zařízení uzel](#devices-node-shortcut-keys)
- [Svazky uzel klávesových zkratek](#volumes-node-shortcut-keys)
- [Hlasitost skupiny uzel klávesových zkratek](#volume-groups-node-shortcut-keys)
- [Zálohování zásady uzel klávesových zkratek](#backup-policies-node-shortcut-keys)
- [Zálohování katalogu uzel klávesových zkratek](#backup-catalog-node-shortcut-keys)
- [Úlohy uzel klávesových zkratek](#jobs-node-shortcut-keys)

#### <a name="devices-node-shortcut-keys"></a>Klávesové zkratky pro zařízení uzel

| Místní nabídka | Výsledek                               |
|:--------------|:-------------------------------------|
| C             | Otevře stránku **Konfigurovat zařízení** . |
| D             | Aktualizuje seznam zařízení a podrobnosti o zařízení.|
| V             | Otevře nabídku **Zobrazit** . |
| W             | Otevře nový konzoly StorSimple snímek správce zaměřené na uzel **Podrobnosti** . |
| F             | Aktualizuje konzole StorSimple snímek správce. |
| L             | Otevření stránky **Exportovat** . 
| H             | Otevře nápovědu.|
 

#### <a name="volumes-node-shortcut-keys"></a>Svazky uzel klávesových zkratek

| Místní nabídka   | Výsledek                              |
|:----------------|:------------------------------------|
| V               | Aktualizace seznamu svazky.        |
| V (stiskněte dvakrát) | Otevře nabídku **Zobrazit** .            |
| W               | Otevře nový konzoly StorSimple snímek správce zaměřené na uzel **svazky** .|
| F               | Aktualizuje konzole StorSimple snímek správce.|
| L               | Otevření stránky **Exportovat** . 
| H               | Otevře nápovědu.|
 
#### <a name="volume-groups-node-shortcut-keys"></a>Hlasitost skupiny uzel klávesových zkratek

| Místní nabídka   | Výsledek                              |
|:----------------|:------------------------------------|
| G               | Otevře stránce **vytvořit skupinu hlasitost** . |
| V               | Otevře nabídku **Zobrazit** . |
| W               | Otevře novou konzolu StorSimple snímek Správce zaměření na uzel **Hlasitost skupiny** .|
| F               | Aktualizuje konzole StorSimple snímek správce. |
| L               | Otevření stránky **Exportovat** . |
| H               | Otevře nápovědu.|

#### <a name="backup-policies-node-shortcut-keys"></a>Zálohování zásady uzel klávesových zkratek

| Místní nabídka   | Výsledek                              |
|:----------------|:------------------------------------|
| B               | Otevře se stránka **vytvořit zásady** . |
| V               | Otevře nabídku **Zobrazit** .            |
| W               | Otevře novou konzolu StorSimple snímek Správce zaměření na uzel **Hlasitost skupiny** .|
| F               | Aktualizuje konzole StorSimple snímek správce.|
| L               | Otevření stránky **Exportovat **. 
| H               | Otevře nápovědu.|
 
#### <a name="backup-catalog-node-shortcut-keys"></a>Zálohování katalogu uzel klávesových zkratek

| Místní nabídka   | Výsledek                              |
|:----------------|:------------------------------------|
| W               | Otevře novou konzolu StorSimple snímek Správce zaměření na uzel **Hlasitost skupiny** . |
| F               | Aktualizuje konzole StorSimple snímek správce. |
| H               | Otevře nápovědu.|
 
#### <a name="jobs-node-shortcut-keys"></a>Úlohy uzel klávesových zkratek

| Místní nabídka   | Výsledek                              |
|:----------------|:------------------------------------|
| V               | Otevře nabídku **Zobrazit** .            |
| W               | Otevře nový konzoly StorSimple snímek správce zaměřené na uzel **úlohy** .|
| F               | Aktualizuje konzole StorSimple snímek správce.|
| L               | Otevření stránky **Exportovat** .     |
| H               | Otevře online nápověda                   |
 
## <a name="next-steps"></a>Další kroky

- Zjistěte, jak lze [pomocí Správce snímek StorSimple ke správě StorSimple řešení](storsimple-snapshot-manager-admin.md).
- Zjistěte, jak lze [pomocí Správce snímek StorSimple se můžete připojit a správa zařízení](storsimple-snapshot-manager-manage-devices.md).
