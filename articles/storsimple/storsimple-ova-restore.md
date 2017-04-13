<properties
   pageTitle="Obnovení ze zálohy své StorSimple virtuální pole"
   description="Další informace o tom, jak obnovit zálohu své StorSimple virtuální pole."
   services="storsimple"
   documentationCenter="NA"
   authors="alkohli"
   manager="carmonm"
   editor=""/>

<tags
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="06/07/2016"
   ms.author="alkohli"/>

# <a name="restore-from-a-backup-of-your-storsimple-virtual-array"></a>Obnovení ze zálohy své StorSimple virtuální pole

## <a name="overview"></a>Základní informace 

Tento článek se týká verzi Microsoft Azure StorSimple virtuální pole (označovaná taky jako StorSimple místní virtuální zařízení nebo StorSimple virtuální zařízení) pracovního dne 2016 všeobecně dostupná (GA) nebo novější. Tento článek popisuje podrobné obnovení ze zálohy sady sdílené složky nebo svazky na své StorSimple virtuální pole. V článku také podrobnosti, jak obnovení úrovni položky označené jako pracuje na vaší StorSimple virtuální pole nakonfigurovaný jako souborový server.


## <a name="restore-shares-from-a-backup-set"></a>Obnovení sdílené položky ze záložní sady


**Před pokusem o obnovit sdílené složky, ujistěte se, že máte dostatek místa na zařízení pro dokončení operace.** Obnovení ze zálohy, [Azure klasické portál](https://manage.windowsazure.com/), proveďte následující kroky.

#### <a name="to-restore-a-share"></a>Chcete-li obnovit sdílené složky

1.  Přejděte do **katalogu zálohování**. Filtrování podle potřeby zařízení a časový rozsah hledání záloh. Klikněte na ikonu kontrola ![](./media/storsimple-ova-restore/image1.png) následujících situací.


1.  V seznamu sad záložní zobrazí klikněte na a vyberte konkrétní zálohu. Rozbalte položku zálohování zobrazíte různé akcie pod ní. Klikněte na a vyberte sdílené položky, kterou chcete obnovit.

2.  V dolní části stránky klikněte na **Obnovit jako nové**.

3.  To zahájí Průvodce **obnovením jako nový sdílet** . Na stránce **Zadejte název a umístění** :


    1.  Zkontrolujte zařízení názvu zdroje. Je vhodné zařízení, které obsahuje příkaz sdílet, kterou chcete obnovit. Výběr zařízení je šedá. K výběru jiného zdroje zařízení, musíte průvodce ukončit a znovu vyberte zálohu znovu.

    2.  Zadejte název sdílené složky. Název musí být 3 až 127 znaků.

    3.  Seznamte se s velikost, typ a oprávnění přidružená k sdílet, který chcete obnovit. Bude moct upravit vlastnosti sdílené položky pomocí Průzkumníka systému Windows po dokončení obnovení.

    4.  Klikněte na ikonu kontrola ![](./media/storsimple-ova-restore/image1.png).

        ![](./media/storsimple-ova-restore/image9.png)

1.  Po dokončení obnovení projektu na Obnovit se spustí a uvidíte další oznámení. Sledování průběhu obnovit, klikněte na **zobrazení projektu**. Tím přejdete na stránku **úlohy** .

2.  Můžete sledovat průběh úloh obnovení. Po obnovení 100 % dokončení, přejděte zpátky na stránku **sdílené složky** na svém zařízení.

3.  Teď můžete zobrazit nové obnovené sdílené složky v seznamu sdílené složky na svém zařízení. Poznámka: Tento obnovení probíhá do stejného typu sdílet. Vrstvené sdílet se obnoví jako vrstveny a místně připnuté sdílet jako místně připnuté sdílet.

Teď dokončení konfigurace zařízení a naučili zálohovat nebo obnovit sdílené. 


## <a name="restore-volumes-from-a-backup-set"></a>Obnovení svazky ze záložní sady


Obnovení ze zálohy, na portálu Azure klasické, proveďte následující kroky. Obnovení obnoví zálohování nové hlasitosti na stejné zařízení virtuální; nebude možné obnovit na jiné zařízení.

#### <a name="to-restore-a-volume"></a>Chcete-li obnovit svazku

1.  Přejděte do **katalogu zálohování**. Filtrování podle potřeby zařízení a časový rozsah hledání záloh. Klikněte na ikonu kontrola ![](./media/storsimple-ova-restore/image1.png) následujících situací.

2.  V seznamu sad záložní zobrazí klikněte na a vyberte konkrétní zálohu. Rozbalte položku zálohování zobrazíte různé svazky pod ní. Vyberte hlasitost, kterou chcete obnovit. 

5.  V dolní části stránky klikněte na **Obnovit jako nové**. Spustí se průvodce **Obnovit jako nový hlasitost** .

1.  Na stránce **Zadejte název a umístění** :


    1.  Zkontrolujte zařízení názvu zdroje. Je vhodné zařízení, které obsahuje svazku, u kterého chcete obnovit. Výběr zařízení není k dispozici. K výběru jiného zdroje zařízení, musíte průvodce ukončit a znovu vyberte zálohu znovu.

    2.  Zadejte název hlasitost hlasitost obnovována jako nové. Název svazku musí být 3 až 127 znaků.

    3.  Klikněte na ikonu se šipkou.

        ![](./media/storsimple-ova-restore/image12.png)

1.  Na stránce **Zadejte hosts, které můžete použít tento hlasitost** vyberte odpovídající ACRs z rozevíracího seznamu.

    ![](./media/storsimple-ova-restore/image13.png)

1.  Klikněte na ikonu kontrola ![](./media/storsimple-ova-restore/image1.png). To zahájí úlohy obnovení a zobrazí se následující upozornění, které probíhá projektu.

2.  Po dokončení obnovení projektu na Obnovit se spustí a uvidíte další oznámení. Sledování průběhu obnovit, klikněte na **zobrazení projektu**. Tím přejdete na stránku **úlohy** .

3.  Můžete sledovat průběh úloh obnovení. Přejděte zpátky na stránku **svazky** na vašem zařízení.

4.  Teď můžete zobrazit nové obnovená hlasitost v seznamu svazky na vašem zařízení. Všimněte si, že obnovení probíhá do stejného typu objem. Vrstvené hlasitost se obnoví jako vrstveny a místně připnuté hlasitost se obnoví jako místně připnuté hlasitost.

5.  Až se zobrazí hlasitost online v seznamu svazky, hlasitost je k dispozici.  Na hostiteli vyzývající iSCSI aktualizujte seznam cílů v okně vlastností vyzývající iSCSI.  Nový cíl, který obsahuje název obnovená hlasitost by měl vypadat jako "neaktivní" ve sloupci stav.

6.  Vyberte cíl a klikněte na **Připojit**.   Po připojení vyzývající byste do cíle by měl stav změní na **Připojit**. 

7.  V okně **Správa disků** připojené svazky zobrazí, jak je znázorněno na následujícím obrázku. Klikněte pravým tlačítkem myši zjištěnou hlasitost (klikněte na název disku) a potom klikněte na **Online**.

> [AZURE.IMPORTANT] Při pokusu o obnovení ze zálohy svazku nebo sdílené když úlohy obnovení nepovede, nastavte hlasitost cílové nebo sdílet můžou být pořád vytvoření na portálu. Je důležité odstranit tento cílové hlasitost nebo sdílet na portálu minimalizovat všechny budoucí problémy vyplývající z tohoto prvku.

## <a name="item-level-recovery-ilr"></a>Obnovení položky úrovně (ILR)

Tato verze uvádí obnovení úrovni položek (ILR) na virtuální zařízení StorSimple nakonfigurování jako souborovém serveru. Tato funkce umožňuje provádět podrobného obnovení souborů a složek ze zálohy cloudu všechny sdílené složky na StorSimple zařízení. Uživatelé lze obnovit odstraněné soubory z poslední doby nějaké zálohy pomocí modelu samoobslužných funkcí.

Každý sdílet má *.backups* složku obsahující nejčastěji poslední doby nějaké zálohy. Uživatele můžete přejděte do požadované zálohování, zkopírujte příslušných souborů a složek ze zálohy a jejich obnovení. Díky hovory správcům pro obnovení souborů systému pomocí zálohy.

1.  Při provádění ILR, můžete zobrazit zálohy pomocí Průzkumníka Windows. Klikněte na konkrétní sdílet, kterou chcete prohlédnout zálohování pro. Zobrazí se do složky *.backups* vytvořené v části sdílet, který uchovává všechny zálohy. Rozbalte složku *.backups* zobrazíte zálohy. Složky se zobrazí pohledem celý záložní hierarchie. Toto zobrazení se vytvoří na vyžádání a obvykle trvá jenom pár sekund, než se vytvořit.

    Posledních 5 zálohy zobrazují tímto způsobem a mohou sloužit k obnovení úrovni položek. 5 poslední doby nějaké zálohy patří výchozí naplánované a ruční zálohování.

    
    -   **Zálohování naplánované** stejný název jako &lt;název zařízení&gt;DailySchedule RRRRMMDD HHMMSS UTC.

    -   **Ruční zálohování** stejný název jako Ad-hoc RRRRMMDD-HHMMSS-UTC.
    
        ![](./media/storsimple-ova-restore/image14.png)

1.  Označte zálohu obsahující nejnovější verzi odstraněný soubor. Když název složky obsahuje časové razítko UTC ve všech výše uvedených případech, je čas, pro niž složce vytvořila skutečné zařízení čas zahájení zálohování. Časové razítko složky umožňuje vyhledat a určit zálohy.

2.  Vyhledejte složku nebo soubor, který chcete obnovit v zálohy, kterou jste určili v předchozím kroku. Poznámka: můžete zobrazit pouze soubory nebo složky, které máte oprávnění pro. Pokud si nejste mít přístup k některé soubory nebo složky, budete muset kontaktovat správce sdílet, který můžete pomocí Průzkumníka Windows upravit oprávnění ke sdílení a umožňují přístup k určité souboru nebo složky. Je doporučený osvědčený postup, že je možné sdílet správci skupiny uživatelů místo jednoho uživatele.

3.  Kopírování souboru nebo složky do příslušné sdílené složky na souborovém serveru StorSimple.

![video_icon](./media/storsimple-ova-restore/video_icon.png) **Video k dispozici**

Podívejte se na video a zjistěte, jak můžete vytvářet sdílené složky, obecnějším údajům sdílené položky a obnovte data na pole virtuální StorSimple.

> [AZURE.VIDEO use-the-storsimple-virtual-array]

## <a name="next-steps"></a>Další kroky

Další informace o tom, jak [Spravovat své StorSimple virtuální pole pomocí místní web uživatelského rozhraní](storsimple-ova-web-ui-admin.md).
