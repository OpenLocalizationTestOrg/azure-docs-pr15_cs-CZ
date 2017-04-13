<properties 
   pageTitle="Virtuální pole StorSimple záložní kurz | Microsoft Azure"
   description="Popisuje, jak k obecnějším údajům akcie StorSimple virtuální matice a svazky."
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
   ms.date="06/07/2016"
   ms.author="alkohli" />

# <a name="back-up-your-storsimple-virtual-array"></a>Obecnějším údajům své StorSimple virtuální pole

## <a name="overview"></a>Základní informace 

Tento kurz se týká verzi Microsoft Azure StorSimple virtuální pole (označovaná taky jako StorSimple místní virtuální zařízení nebo StorSimple virtuální zařízení) pracovního dne 2016 všeobecně dostupná (GA) nebo novější verze.

Virtuální pole StorSimple je hybridní cloudové úložiště místní virtuální zařízení, které lze nastavit jako souborový server nebo iSCSI serveru. Můžete vytvořit zálohování, obnovení systému pomocí zálohy a provedení zařízení převzetí případnému obnovení havárie. Po nakonfigurování jako souborovém serveru, umožňuje také obnovení úrovni položek. Tento kurz popisuje, jak vytvořit plánované a ruční zálohování své StorSimple virtuální pole pomocí portálu Azure klasické nebo web StorSimple uživatelského rozhraní.


## <a name="back-up-shares-and-volumes"></a>Obecnějším údajům sdílené položky a svazky

Zálohování ochrany v okamžiku, zlepšit zotavení: a minimalizace obnovení času u sdílené položky a svazky. Můžete zálohovat sdílené složky nebo hlasitosti na vašem zařízení StorSimple dvěma způsoby: **naplánováno** nebo **ručně**. V následujících částech jsou uvedeny všech metod.

> [AZURE.NOTE] V této verzi plánované zálohy vytváří výchozí zásady, která běží každý den v určeném časovém a zálohovat všechny sdílené složky nebo svazky na zařízení. Není možné vytvořit vlastní zásady plánované záloh v současné době.

## <a name="change-the-backup-schedule"></a>Změna plánu zálohování

Virtuální zařízení StorSimple má výchozí zásady zálohování začínající zadaný časový den (22:30) a zálohovat všechny sdílené složky nebo svazky na zařízení jednou za den. Můžete změnit čas niž záložní spustí, ale četnost a uchovávání informací (určující počet zálohy uchovávání) nelze změnit. Během zálohy je celý virtuální zařízení zálohovala; Proto doporučujeme naplánovat tyto zálohy pro největšího.

Proveďte následující kroky [Azure klasické portálu](https://manage.windowsazure.com/) změnit výchozí záložní čas.

#### <a name="to-change-the-start-time-for-the-default-backup-policy"></a>Chcete-li změnit počáteční čas výchozí zásady zálohování

1. Přejděte na kartu zařízení **Konfigurace** .

2. V části **záložní** zadejte počáteční datum denní zálohování.

3. Klikněte na **Uložit**.

### <a name="take-a-manual-backup"></a>Provést ruční zálohování

Kromě plánované zálohy můžete využít ručním (na vyžádání) záložní kdykoli.

#### <a name="to-create-a-manual-on-demand-backup"></a>Vytvoření ruční zálohování (na vyžádání)

1. Přejděte na kartu **sdílené složky** nebo **svazky** .

2. V dolní části stránky klikněte na tlačítko **zálohování všechny**. Zobrazí se výzva k ověření, že chcete provést zálohování. Klikněte na ikonu kontrola ![zaškrtněte ikony](./media/storsimple-ova-backup/image3.png) pokračujte zálohování.

    ![zálohování potvrzení](./media/storsimple-ova-backup/image4.png)

    Zobrazí se oznámení, že je spuštění úlohy zálohování.

    ![zálohování spuštění](./media/storsimple-ova-backup/image5.png)

    Zobrazí se oznámení, že úloha byla úspěšně vytvořena.

    ![Vytvoření úloh zálohování](./media/storsimple-ova-backup/image7.png)

3. Sledování průběhu projektu, klikněte na **Zobrazení projektu**.

4. Po dokončení úloh zálohování přejděte na kartu **katalog zálohování** . Měli byste vidět zálohování dokončený.

    ![Dokončené zálohování](./media/storsimple-ova-backup/image8.png)

5. Nastavte výběr filtru příslušné zařízení, zásady zálohování a časový rozsah a klikněte na ikonu zaškrtnutí ![Zaškrtněte ikony](./media/storsimple-ova-backup/image3.png).

    Zálohování by se měly v seznamu sad záložní, která se zobrazí v katalogu.

## <a name="view-existing-backups"></a>Zobrazení existujícího zálohování

Proveďte následující kroky v portálu Azure klasické zobrazení existující zálohy.

#### <a name="to-view-existing-backups"></a>Chcete-li zobrazit stávající záložní kopie

1. Na stránce Správce StorSimple služby klikněte na kartu **katalog zálohování** .

2. Vyberte záložní nastavit takto:

    1. Vyberte požadované zařízení.

    2. V rozevíracím seznamu vyberte sdílet nebo hlasitost zálohy, kterou chcete vybrat.

    3. Zadejte časový rozsah.

    4. Klikněte na ikonu kontrola ![](./media/storsimple-ova-backup/image3.png) k provedení tohoto dotazu.

    Zálohy spojený se sdílenou vybrané nebo objem by se měly v seznamu sad zálohování.

![video_icon](./media/storsimple-ova-backup/video_icon.png) **Video k dispozici**

Podívejte se na video a zjistěte, jak můžete vytvářet sdílené složky, obecnějším údajům sdílené položky a obnovte data na pole virtuální StorSimple.

> [AZURE.VIDEO use-the-storsimple-virtual-array]

## <a name="next-steps"></a>Další kroky

Další informace o [správě své StorSimple virtuální pole](storsimple-ova-web-ui-admin.md).
