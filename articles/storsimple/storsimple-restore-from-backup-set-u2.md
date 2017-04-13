<properties 
   pageTitle="Obnovení ze zálohy hlasitost StorSimple | Microsoft Azure"
   description="Vysvětluje, jak na stránce Správce StorSimple služby katalogu zálohy můžete obnovit hlasitost StorSimple ze záložní sady."
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

# <a name="restore-a-storsimple-volume-from-a-backup-set-update-2"></a>Obnovení StorSimple hlasitost ze záložní sady (aktualizace 2)

[AZURE.INCLUDE [storsimple-version-selector-restore-from-backup](../../includes/storsimple-version-selector-restore-from-backup.md)]

## <a name="overview"></a>Základní informace

Na stránce **Záložní katalogu** zobrazí všechny záložní sady, které jsou vytvořené po ručního nebo automatické zálohy se neprojevily. Na této stránce můžete seznam všech záloh pro zásady zálohování nebo objem, vyberte nebo odstranění zálohy nebo obnovení nebo klonovat svazku pomocí zálohy.

 ![Zálohování stránky katalogu](./media/storsimple-restore-from-backup-set-u2/restore.png)

Tento kurz vysvětluje, jak na stránce **Katalogu zálohy** můžete obnovit zařízení ze záložní sady.

Můžete obnovit hlasitost z místního nebo zálohování cloudu. V obou případech obnovení přináší funkce hlasitost online když data se stáhne na pozadí. 

Než zahájíte obnovení, byste měli znát z následujících akcí:

- **Že je nutné provést hlasitost offline** – opět převzít hlasitost offline na hostiteli a zařízení před spustit operaci obnovení. I když obnovení automaticky přenese hlasitost online na zařízení, je nutné ručně přenést zařízení online na hostiteli. Můžete si přenést hlasitost online na hostiteli hned hlasitost je online na zařízení. (Není potřebujete počkejte na dokončení obnovení.) Postupy přejděte na [Vezměte objem offline](storsimple-manage-volumes-u2.md#take-a-volume-offline).

- **Typ svazku po obnovení** – odstranění svazky se obnoví různým typům ve snímku; To znamená svazky, které byly místně připnuté obnoveny jako místně připnuté svazky a svazky, které byly vrstveny obnoveny jako vrstvené svazky.

    Pro existující svazky aktuální typ použití objemu přepíše typ, který je uložený ve snímku. Třeba při obnovení svazku ze snímku, který byl vzít při vrstveny typu svazku a typ hlasitost je teď místně připnuté (kvůli operaci převodní proběhlo), potom hlasitost bude obnoven jako místně připnuté hlasitost. Pokud již existujícího místně připnuté svazku byla rozbalená a následně obnovit ze starší snímku vzít při hlasitost byl menší, podobně obnovená hlasitost zachová aktuální rozbalené velikost.

    Převést nejde svazku z vrstvené hlasitosti místně připnuté hlasitost nebo místně připnuté hlasitost vrstvené objem během právě obnovována hlasitost. Počkejte na dokončení operace obnovit, a následný převod hlasitost na jiný typ. Informace o převedení svazku přejděte k části [Změna typu svazku](storsimple-manage-volumes-u2.md#change-the-volume-type). 

- **Velikost svazku se projeví v obnovených hlasitost** – to je důležitá Poznámka Pokud obnovujete místně připnuté objem, který byl odstraněn (protože místně připnuté svazky jsou plně zřízení). Ujistěte se, že máte dostatek místa před pokusem o obnovení místně připnuté svazku, která byla dříve odstraněna. 

- **Že nelze rozbalit hlasitost během právě obnovována** – Počkejte, až dokončení operace obnovení před pokusem o rozbalte hlasitost. Informace o rozbalování svazku přejděte na [změnit svazku](storsimple-manage-volumes-u2.md#modify-a-volume).

- **Vytvořením zálohy během obnovujete místní objem** -postupy najdete [pomocí StorSimple Správce služby Správa záložní zásad](storsimple-manage-backup-policies.md).

- **Že zrušíte obnovení** – Pokud předplatné zrušíte obnovení projektu a potom hlasitost bude vrácena zpět na stav, který byl předtím, než jste začali obnovení. Postupy přejděte na [Zrušení úlohy](storsimple-manage-jobs-u2.md#cancel-a-job).

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

Na stránce **Záložní katalogu** obnovíte hlasitost StorSimple z konkrétní zálohy. Mějte na paměti, ale, obnovení svazku vrátí hlasitost stavu, který byl při vytvoření zálohy. Všechna data, která byla vkládají zálohování budou ztraceny.

> [AZURE.WARNING] Obnovení ze zálohy nahradí existující svazky ze zálohy. To může způsobit ztrátu všechna data, která napsané po vytvoření zálohy.

### <a name="to-restore-your-volume"></a>Chcete-li obnovit hlasitost

1. Na stránce Správce StorSimple služby klikněte na kartu **katalog zálohování** .

    ![Zálohování katalogu](./media/storsimple-restore-from-backup-set-u2/restore.png)

2. Vyberte záložní nastavit takto:
  1. Vyberte příslušné zařízení.
  2. V rozevíracím seznamu vyberte hodnotu hlasitost nebo zálohování zásad pro zálohy, kterou chcete vybrat.
  3. Zadejte časový rozsah.
  4. Klikněte na ikonu zaškrtnutí ![Zaškrtněte ikony](./media/storsimple-restore-from-backup-set-u2/HCS_CheckIcon.png) k provedení tohoto dotazu.
 
    Zálohy přidružený k vybrané hlasitost nebo zásady zálohování by se měly v seznamu sad zálohování.

3. Rozbalte zálohu chcete-li zobrazit související svazky. Tyto svazky třeba offline v hostiteli a zařízení před jejich obnovení. Přístup k svazky na stránce **Hlasitost kontejnery** a pak postupujte podle kroků v tématu přepněte do režimu offline [Vezměte objem offline](storsimple-manage-volumes-u2.md#take-a-volume-offline) .

    > [AZURE.IMPORTANT] Ujistěte se, že jste provedli svazky offline v hostiteli nejdřív před zpřístupněním svazky offline v zařízení. Pokud není provedete svazky offline v hostiteli, může potenciálně vést k poškození dat.

4. Přechod zpět na kartě **Záložní katalogu** a vyberte záložní nastavit.

5. Klikněte na tlačítko **Obnovit** v dolní části stránky.

6. Zobrazí se výzva k potvrzení. Zkontrolujte informace obnovit a potom zaškrtněte políčko potvrzení.

    ![Potvrzovací stránku](./media/storsimple-restore-from-backup-set-u2/ConfirmRestore.png)

7. Klikněte na ikonu kontrola ![zaškrtněte ikony](./media/storsimple-restore-from-backup-set-u2/HCS_CheckIcon.png). To zahájí obnovení úlohu, kterou můžete zobrazit tak, že přístup ke stránce **úlohy** . 

8. Po dokončení obnovení můžete ověřit, že obsah svazky nahrazují svazky ze zálohy.

![Video k dispozici](./media/storsimple-restore-from-backup-set-u2/Video_icon.png) **Video k dispozici**

Podívejte se na video, který ukazuje, jak můžete použít klonovat a obnovení funkce StorSimple chcete obnovit odstraněné soubory, klikněte [sem](https://azure.microsoft.com/documentation/videos/storsimple-recover-deleted-files-with-storsimple/).

## <a name="if-the-restore-fails"></a>Pokud se nezdaří obnovení

Pokud z nějakého důvodu selže obnovení obdržíte upozornění. V takovém případě aktualizace seznamu zálohování potvrďte, že zálohování je pořád platná. Pokud platí zálohování a obnovování z cloudu, pak problémy s připojením může být příčinou. 

Dokončete obnovení prohlédněte hlasitost offline v hostiteli a opakujte obnovit. Všimněte si, že budou ztraceny všechny změny, které byly provedeny během obnovení dat hlasitost.

## <a name="next-steps"></a>Další kroky

- Zjistěte, jak [Spravovat StorSimple svazky](storsimple-manage-volumes-u2.md).

- Zjistěte, jak [používat službu StorSimple správce ke správě StorSimple zařízení](storsimple-manager-service-administration.md).
