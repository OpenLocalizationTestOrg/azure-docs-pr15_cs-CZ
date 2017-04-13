<properties 
   pageTitle="Obnovení ze zálohy hlasitost StorSimple | Microsoft Azure"
   description="Vysvětluje, jak na stránce Správce StorSimple služby katalogu zálohy můžete obnovit hlasitost StorSimple ze záložní sady."
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
   ms.date="08/17/2016"
   ms.author="alkohli" />

# <a name="restore-a-storsimple-volume-from-a-backup-set"></a>Obnovení StorSimple hlasitost ze záložní sady

[AZURE.INCLUDE [storsimple-version-selector-restore-from-backup](../../includes/storsimple-version-selector-restore-from-backup.md)]

## <a name="overview"></a>Základní informace

Na stránce **Záložní katalogu** zobrazí všechny záložní sady, které jsou vytvořené po ručního nebo automatické zálohy se neprojevily. Na této stránce můžete seznam všech záloh pro zásady zálohování nebo objem, vyberte nebo odstranění zálohy nebo obnovení nebo klonovat svazku pomocí zálohy.

 ![Zálohování stránky katalogu](./media/storsimple-restore-from-backup-set/HCS_BackupCatalog.png)

Tento kurz vysvětluje, jak na stránce **Katalogu zálohy** můžete obnovit hlasitosti na vašem zařízení ze záložní sady.

## <a name="how-to-use-the-backup-catalog"></a>Použití zálohování katalogu 

Stránky **Katalogu zálohy** poskytuje dotaz, který vám pomůže při filtrování zálohování sadu výběru. Můžete filtrovat záložní sady, které jsou načtena podle následujících parametrů:

- **Zařízení** – zařízení, na kterém byla vytvořená sadu zálohování.
- **Zálohování zásad** nebo **objem** – zásady zálohování nebo objem přidružený k zálohování nastavenou.
- **Od** a **do** – rozsah dat a časů při vytvoření záložní nastavení.

Filtrované záložní sady se jsou: potom tabulkový podle následujícími atributy:

- **Název** – název zásady zálohování nebo objem přidružený k sadě zálohování.
- **Velikost** – skutečná velikost záložní sady.
- **Vytvoření** – datum a čas, kdy byly vytvořené zálohy. 
- **Typ** – zálohování sady lze místní snímky nebo snímky v cloudu. Místní snímek je zálohu všech dat hlasitost uložená místně na zařízení, že snímek cloudu odkazuje na zálohování dat hlasitost umístěné v cloudu. Místní snímky umožňují rychlejší přístup, že cloudu snímky jsou zvolené odolnosti data.
- **Zahájil** – zálohy se dá spouštět automaticky podle plánu ručně nebo uživatelem. (Slouží zásady zálohování naplánovat zálohování. Můžete taky můžete **převzít záložní** možnost přijmout interaktivní zálohování.)

## <a name="how-to-restore-your-storsimple-volume-from-a-backup"></a>Jak obnovit hlasitost StorSimple ze zálohy

Na stránce **Záložní katalogu** obnovíte hlasitost StorSimple z konkrétní zálohy. 

> [AZURE.WARNING] Obnovení ze zálohy nahradí existující svazky ze zálohy. To může způsobit ztrátu všechna data, která napsané po vytvoření zálohy.

Než zahájíte obnovení svazku, zajistěte, aby byl hlasitost offline. Budete muset přijmout hlasitost offline v hostiteli první a potom na zařízení. Postupujte podle kroků v [Vezměte objem offline](storsimple-manage-volumes.md#take-a-volume-offline). Proveďte následující kroky k obnovení svazku ze záložní sady.

### <a name="to-restore-from-a-backup-set"></a>Obnovení ze zálohy sady

1. Na stránce Správce StorSimple služby klikněte na kartu **katalog zálohování** .

    ![Zálohování katalogu](./media/storsimple-restore-from-backup-set/HCS_Restore.png)

2. Vyberte záložní nastavit takto:
  1. Vyberte příslušné zařízení.
  2. V rozevíracím seznamu vyberte hodnotu hlasitost nebo zálohování zásad pro zálohy, kterou chcete vybrat.
  3. Zadejte časový rozsah.
  4. Klikněte na ikonu zaškrtnutí ![Zaškrtněte ikony](./media/storsimple-restore-from-backup-set/HCS_CheckIcon.png) k provedení tohoto dotazu.
 
    Zálohy přidružený k vybrané hlasitost nebo zásady zálohování by se měly v seznamu sad zálohování.

3. Rozbalte zálohu chcete-li zobrazit související svazky. Tyto svazky třeba offline v hostiteli a zařízení před jejich obnovení. Postupujte podle kroků v [Vezměte objem offline](storsimple-manage-volumes.md#take-a-volume-offline).

    >  [AZURE.IMPORTANT] Ujistěte se, že jste provedli svazky offline v hostiteli nejdřív před zpřístupněním svazky offline v zařízení. Pokud není provedete svazky offline v hostiteli, může potenciálně vést k poškození dat.

4. Vyberte sadu zálohování. Klikněte na tlačítko **Obnovit** v dolní části stránky.

6. Zobrazí se výzva k potvrzení. 

    ![Potvrzovací stránku](./media/storsimple-restore-from-backup-set/HCS_ConfirmRestore.png)

7. Zkontrolujte informace o obnovení a klikněte na ikonu kontrola ![zaškrtněte ikony](./media/storsimple-restore-from-backup-set/HCS_CheckIcon.png). To zahájí obnovení úlohu, kterou můžete zobrazit tak, že přístup ke stránce **úlohy** . 

8. Po dokončení obnovení můžete ověřit, že obsah svazky nahrazují svazky ze zálohy.

![Video k dispozici](./media/storsimple-restore-from-backup-set/Video_icon.png) **Video k dispozici**

Podívejte se na video, který ukazuje, jak můžete použít klonovat a obnovení funkce StorSimple chcete obnovit odstraněné soubory, klikněte [sem](https://azure.microsoft.com/documentation/videos/storsimple-recover-deleted-files-with-storsimple/).

## <a name="next-steps"></a>Další kroky

- Zjistěte, jak [Spravovat StorSimple svazky](storsimple-manage-volumes.md).

- Zjistěte, jak [používat službu StorSimple správce ke správě StorSimple zařízení](storsimple-manager-service-administration.md).
