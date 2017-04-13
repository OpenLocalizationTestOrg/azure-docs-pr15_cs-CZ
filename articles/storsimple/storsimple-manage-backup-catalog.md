<properties 
   pageTitle="Správa katalogu záložní StorSimple | Microsoft Azure"
   description="Vysvětluje, jak na stránce Správce StorSimple služby záložní katalogu seznamu, vyberte a odstraňovat záložní sady svazku."
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
   ms.date="04/28/2016"
   ms.author="v-sharos" />

# <a name="use-the-storsimple-manager-service-to-manage-your-backup-catalog"></a>Správa katalogu záložní pomocí službu StorSimple správce

## <a name="overview"></a>Základní informace

Na stránce StorSimple Správce služby **Záložní katalogu** zobrazí záložní sady, které se vytvářejí při zálohování ručním nebo plánované. Na této stránce můžete seznam všech záloh pro zásady zálohování nebo objem, vyberte nebo odstranění zálohy nebo obnovení nebo klonovat svazku pomocí zálohy.

Tento kurz vysvětluje, jak chcete vyberte a odstranění záložní sady. Informace o obnovení svého zařízení ze zálohy, přejděte k [obnovení vašeho zařízení ze záložní sady](storsimple-restore-from-backup-set.md). Naučte se vytvořit kopii svazku, přejděte na [klonovat StorSimple hlasitost](storsimple-clone-volume.md).

![Zálohování katalogu](./media/storsimple-manage-backup-catalog/backupcatalog.png) 

Stránky **Katalogu zálohy** poskytuje dotaz zúžit zálohování sadu výběru. Můžete filtrovat záložní sady, které jsou načtena podle následujících parametrů:

- **Zařízení** – zařízení, na kterém byla vytvořená sadu zálohování.

- **Zásady zálohování nebo objem** – zásady zálohování nebo objem přidružený k zálohování nastavenou.

- **Od a do** – rozsah dat a časů při vytvoření záložní nastavení.

Filtrované záložní sady se jsou: potom tabulkový podle následujícími atributy:

- **Název** – název zásady zálohování nebo objem přidružený k sadě zálohování.

- **Velikost** – skutečná velikost záložní sady.

- **Vytvořeno** : datum a čas, kdy byly vytvořené zálohy. 

- **Typ** – zálohování sady lze místní snímky nebo snímky v cloudu. Místní snímek je zálohu všech dat hlasitost uložená místně na zařízení, že snímek cloudu odkazuje na zálohování dat hlasitost umístěné v cloudu. Místní snímky umožňují rychlejší přístup, že cloudu snímky jsou zvolené odolnosti data.

- **Spuštěná tak, že** – zálohy se dá spouštět automaticky podle plánu nebo ručně uživatelem. Zásady zálohování můžete naplánovat zálohování. Můžete taky můžete **převzít záložní** možnost provést ruční zálohování.

## <a name="list-backup-sets-for-a-volume"></a>Seznam zálohování nastaví svazku
 
Proveďte následující kroky a výpis všech záloh svazku.

#### <a name="to-list-backup-sets"></a>Seznam záložní sady

1. Na stránce Správce StorSimple služby klikněte na kartu **katalog zálohování** .

2. Výběr filtru následujícím způsobem:

    1. Vyberte příslušné zařízení.

    2. V rozevíracím seznamu vyberte hlasitost zobrazíte odpovídající zálohy.

    3. Zadejte časový rozsah.

    4. Klikněte na ikonu zaškrtnutí ![Ikona kontroly](./media/storsimple-manage-backup-catalog/HCS_CheckIcon.png) k provedení tohoto dotazu.
 
    Zálohování přidružený k vybrané hlasitost by se měly v seznamu sad zálohování.

## <a name="select-a-backup-set"></a>Vyberte sadu zálohování

Proveďte následující kroky a vyberte záložní nastavit hlasitost nebo zásady zálohování.

#### <a name="to-select-a-backup-set"></a>Chcete-li vybrat zálohu

1. Na stránce Správce StorSimple služby klikněte na kartu **katalog zálohování** .

2. Výběr filtru následujícím způsobem:

    1. Vyberte příslušné zařízení.

    2. V rozevíracím seznamu vyberte hodnotu hlasitost nebo zálohování zásad pro zálohy, kterou chcete vybrat.

    3. Zadejte časový rozsah.

    4. Klikněte na ikonu zaškrtnutí ![Ikona kontroly](./media/storsimple-manage-backup-catalog/HCS_CheckIcon.png) k provedení tohoto dotazu.

    Zálohy přidružený k vybrané hlasitost nebo zásady zálohování by se měly v seznamu sad zálohování.

3. Vyberte a rozbalte zálohu. Zobrazí se možnosti **Obnovit** a **Odstranit** v dolní části stránky. Lze provést tyto akce na zálohu do, který jste vybrali.

## <a name="delete-a-backup-set"></a>Odstranění záložní sady

Odstraňte zálohy, pokud již nechcete uchovávat data spojená s ním. Proveďte následující postup zálohování sadu odstranit.

#### <a name="to-delete-a-backup-set"></a>Chcete-li odstranit zálohu

1. Na stránce Správce StorSimple služby klikněte na **kartu katalog zálohování**.

2. Výběr filtru následujícím způsobem:

    1. Vyberte příslušné zařízení.

    2. V rozevíracím seznamu vyberte hodnotu hlasitost nebo zálohování zásad pro zálohy, kterou chcete vybrat.

    3. Zadejte časový rozsah.

    4. Klikněte na ikonu zaškrtnutí ![Ikona kontroly](./media/storsimple-manage-backup-catalog/HCS_CheckIcon.png) k provedení tohoto dotazu.

    Zálohy přidružený k vybrané hlasitost nebo zásady zálohování by se měly v seznamu sad zálohování.

3. Vyberte a rozbalte zálohu. Zobrazí se možnosti **Obnovit** a **Odstranit** v dolní části stránky. Klikněte na **Odstranit**.

4. Zobrazí se oznámení po odstranění v průběhu a kdy se úspěšně dokončil. Po dokončení odstranění aktualizuje dotaz na této stránce. Odstraněné zálohu se už nezobrazuje v seznamu záložní sady.

## <a name="next-steps"></a>Další kroky

- Zjistěte, jak [pomocí katalogu záložní obnovíte zařízení ze záložní sady](storsimple-restore-from-backup-set.md).

- Zjistěte, jak [používat službu StorSimple správce ke správě StorSimple zařízení](storsimple-manager-service-administration.md).
