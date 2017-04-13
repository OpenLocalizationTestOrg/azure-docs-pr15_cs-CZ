<properties
   pageTitle="Klonovat hlasitost StorSimple | Microsoft Azure"
   description="Popisuje různé klonovat typů a jejich použití a vysvětluje, jak můžete pomocí zálohy nastavit klonovat jednotlivé hlasitost."
   services="storsimple"
   documentationCenter="NA"
   authors="alkohli"
   manager="carmonm"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="TBD"
   ms.date="07/27/2016"
   ms.author="alkohli" />

# <a name="use-the-storsimple-manager-service-to-clone-a-volume-update-2"></a>Pomocí služby StorSimple správce klonovat objem (aktualizace 2)

[AZURE.INCLUDE [storsimple-version-selector-clone-volume](../../includes/storsimple-version-selector-clone-volume.md)]

## <a name="overview"></a>Základní informace

Na stránce StorSimple Správce služby **Záložní katalogu** zobrazí záložní sady, které jsou vytvořeny v případě ručního nebo automatické zálohy přesměrováni. Na této stránce můžete seznam všech záloh pro zásady zálohování nebo objem, vyberte nebo odstranění zálohy nebo obnovení nebo klonovat svazku pomocí zálohy.

![Stránka záložní katalogu](./media/storsimple-clone-volume-u2/backupCatalog.png)  

Tento kurz popisuje, jak můžete pomocí zálohy nastavit klonovat jednotlivé hlasitost. Vysvětluje také rozdíl mezi *přechodná* a *trvalé* klony.

>[AZURE.NOTE] 
>
>Místně připnuté hlasitost bude klonovat jako vrstvené hlasitost. V případě potřeby klonovaný hlasitosti místně připnuté můžete po úspěšném dokončení operace klonovat převést klonovat místně připnuté hlasitost. Informace o převedení vrstvené hlasitost místně připnuté objem přejděte k části [Změna typu svazku](storsimple-manage-volumes-u2.md#change-the-volume-type).
>
>Pokud se pokusíte převést klonovaný hlasitost z vrstveny k místně připnuté bezprostředně za klonováním (Pokud je pořád přechodná klonovat), převod se nepovede se tato chybová zpráva:
>
>`Unable to modify the usage type for volume {0}. This can happen if the volume being modified is a transient clone and hasn’t been made permanent. Take a cloud snapshot of this volume and then retry the modify operation.` 
>
>Tato chyba přijetí jenom v případě, že jsou klonováním k jiné zařízení. Můžete převést úspěšně hlasitosti místně připnuté Pokud nejprve převést přechodná klonovat trvalé klonovat. Pokud chcete převést přechodná klonovat trvalé klonovat, snímek cloudu ji.

## <a name="create-a-clone-of-a-volume"></a>Vytvoření klonovat svazku

Můžete vytvořit kopií na stejné zařízení, jiné zařízení nebo dokonce virtuálního počítače pomocí místní nebo cloudu snímku.

#### <a name="to-clone-a-volume"></a>Vytvořit kopii svazku

1. Na stránce Správce StorSimple služby klikněte na kartu **katalog zálohování** a vyberte záložní nastavit.

2. Rozbalte zálohu chcete-li zobrazit související svazky. Klikněte na a vyberte svazku ze záložní sady.

     ![Klonovat svazku](./media/storsimple-clone-volume-u2/CloneVol.png) 

3. Klikněte na tlačítko **klonovat** zahájíte klonováním vybrané hlasitost.

4. V Průvodci klonovat hlasitost podle **zadat název a umístění**:

  1. Označte cíl zařízení. Toto je umístění, kde bude vytvořen klonovat. Lze vybrat stejné zařízení nebo zadat na jiné zařízení. Pokud se rozhodnete hlasitost spojené s jinými poskytovateli služeb cloudu (ne Azure), rozevíracím seznamu – pro cílové zařízení se zobrazí pouze fyzických zařízení. Nelze klonovat hlasitost spojené s jinými poskytovateli služeb cloudu na virtuální zařízení.

        >[AZURE.NOTE] Zkontrolujte, že kapacitu požadovanou pro klonovat je nižší než kapacitu k dispozici v cílové zařízení.

  2. Zadejte název jedinečný hlasitost vašeho klonovat. Název musí obsahovat 3 až 127 znaků. 
    
        >[AZURE.NOTE] I když jsou klonováním místně připnuté objem, bude pole **Klonovat hlasitost jako** **Tiered** . Toto nastavení; nemůžete změnit však v případě potřeby klonovaný svazku, který má být místně připnuté taky můžete převést klonovat místně připnuté objem úspěšně vytvořeném klonovat. Informace o převedení vrstvené hlasitost místně připnuté objem přejděte k části [Změna typu svazku](storsimple-manage-volumes-u2.md#change-the-volume-type).

        ![Průvodce klonovat 1](./media/storsimple-clone-volume-u2/clone1.png) 

  3. Klikněte na ikonu se šipkou ![Ikona šipky](./media/storsimple-clone-volume-u2/HCS_ArrowIcon.png) Pokračujte na další stránku.

5. V části **hosts určit, které můžete použít tento hlasitost**:

  1. Určení záznam řízení přístupu (ACR) pro klonovat. Můžete přidat nové ACR nebo zvolte z existujícího seznamu.

        ![Průvodce klonovat 2](./media/storsimple-clone-volume-u2/clone2.png) 

  2. Klikněte na ikonu zaškrtnutí ![Ikona kontroly](./media/storsimple-clone-volume-u2/HCS_CheckIcon.png)Chcete-li dokončit.

6. Bude spuštěná úloha klonovat a budete upozorněni úspěšné vytvoření klonovat. Klikněte na **Zobrazení projektu** sledovat klonovat úlohy na stránce **úlohy** . Zobrazí se tato zpráva a po dokončení klonovat úlohy:

    ![Vytvořit kopii zprávy](./media/storsimple-clone-volume-u2/CloneMsg.png) 

7. Po dokončení klonovat úlohy:

  1. Přejděte na stránku **zařízení** a vyberte kartu **Kontejnery hlasitost** . 
  2. Vyberte, který bude přidružený svazku zdroj, který jste klonovat kontejner hlasitost. V seznamu svazky byste měli vidět klonovat, kterou jste právě vytvořili.

>[AZURE.NOTE] Sledování a výchozí zálohování jsou automaticky zakázány klonovaný svazku.

Klonovat vytvořený tímto způsobem je přechodná klonovat. Další informace o typech klonovat najdete v článku [přechodná porovnání trvalé klony](#transient-vs.-permanent-clones).

Tento klonovat odteď umožňuje zadávání běžná hlasitost a jakoukoli operaci, která je možné svazku bude k dispozici pro klonovat. Je třeba konfigurovat tato hlasitost všechny záloh.

## <a name="transient-vs-permanent-clones"></a>Přechodové porovnání trvalé klony

Přechodné klony vzniká pouze v případě, že jsou klonováním na jiné zařízení. Můžete zkopírovat konkrétní hlasitost ze zálohy nastavit jiné zařízení spravovat StorSimple správce. Přechodná klonovat bude obsahovat odkazy na data v původní hlasitost a tato data bude používat pro čtení a zápis místně na cílové zařízení. 

Po provedení cloudu snímek přechodná klonovat bude výsledný klonovat *trvalé* klonovat. Během tohoto procesu vytvořena kopie data v cloudu a čas zkopírujte tato data, je určený podle velikosti data a Azure čekacích dob (Toto je Azure Azure kopie). To může trvat dnů na zobrazení týdnů. Přechodné klonovat může být trvalé klonovat tímto způsobem a nemá všechny odkazy na původní hlasitost data, která byla klonovat z. 

## <a name="scenarios-for-transient-and-permanent-clones"></a>Scénáře pro přechodná a trvalé klony

Následující oddíly popisují příklad situace, ve kterých mohou sloužit přechodná a trvalé klony.

### <a name="item-level-recovery-with-a-transient-clone"></a>Obnovení úroveň položek s přechodná klonovat

Budete muset obnovit soubor prezentace aplikace PowerPoint starých jeden rok. Správce IT identifikuje konkrétní zálohu z této časový rámec a vyfiltruje hlasitost. Správce pak klonuje hlasitost, najde soubor, který hledáte a poskytne ji můžete. V tomto případě bude použita přechodná klonovat. 
 
![Video k dispozici](./media/storsimple-clone-volume-u2/Video_icon.png) **Video k dispozici**

Podívejte se na video, který ukazuje, jak můžete použít klonovat a obnovení funkce StorSimple chcete obnovit odstraněné soubory, klikněte [sem](https://azure.microsoft.com/documentation/videos/storsimple-recover-deleted-files-with-storsimple/).

### <a name="testing-in-the-production-environment-with-a-permanent-clone"></a>Testování v provozním prostředí s trvalé klonovat

Je třeba ověřit testování chyb v provozním prostředí. Vytvoření klonovat hlasitost v provozním prostředí a pak udělejte si snímek cloudu tento klonovat vytvořit nezávislé klonovaný hlasitost. V tomto případě bude použita trvalé klonovat.  

## <a name="next-steps"></a>Další kroky
- Zjistěte, jak [Obnovit StorSimple hlasitost ze záložní sady](storsimple-restore-from-backup-set-u2.md).

- Zjistěte, jak [používat službu StorSimple správce ke správě StorSimple zařízení](storsimple-manager-service-administration.md).

 
