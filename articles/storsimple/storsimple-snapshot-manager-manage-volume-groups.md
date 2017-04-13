<properties 
   pageTitle="Správce snímek StorSimple hlasitost skupiny | Microsoft Azure"
   description="Popisuje, jak používat modulu snap-in konzoly MMC StorSimple snímek správce k vytváření a Správa skupin hlasitost."
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

# <a name="use-storsimple-snapshot-manager-to-create-and-manage-volume-groups"></a>Použití StorSimple snímek správce k vytváření a Správa skupin hlasitosti

## <a name="overview"></a>Základní informace

Můžete uzel **Hlasitost skupiny** v podokně **obor** přiřazení svazky skupinám hlasitost, zobrazit informace o skupině hlasitost, plánování zálohování a upravit hlasitost skupiny. 

Hlasitost skupiny jsou fondů související svazky umožňuje zajistit, aby zálohování konzistenci aplikací. Další informace najdete v tématu [svazky a hlasitost skupiny](storsimple-what-is-snapshot-manager.md#volumes-and-volume-groups) a [Integrace se službou Windows hlasitost stín kopie](storsimple-what-is-snapshot-manager.md#integration-with-windows-volume-shadow-copy-service).

>[AZURE.IMPORTANT] 
>
> * Všechny svazky ve skupině hlasitost musí pocházet z jednoho cloudu poskytovatele služeb.
> 
> * Při konfiguraci hlasitost skupin není kombinovat sdílené clusteru svazky (CSVs) a jiných CSVs ve stejné skupině hlasitost. Správce snímek StorSimple nepodporuje kombinaci CSVs a jiných CSVs ve stejném snímku.
 
![Hlasitost skupiny uzel](./media/storsimple-snapshot-manager-manage-volume-groups/HCS_SSM_Volume_groups.png)

**Obrázek 1: StorSimple snímek správce hlasitost skupiny uzel** 

Tento kurz vysvětluje, jak můžete pomocí StorSimple snímek správce:

- Zobrazit informace o skupinách hlasitosti 
- Vytvoření skupiny hlasitosti
- Obecnějším údajům skupinu hlasitosti
- Úprava skupiny hlasitosti
- Odstranění skupiny hlasitosti

Všechny tyto akce jsou dostupné v podokně **akcí** .
 
## <a name="view-volume-groups"></a>Zobrazit hlasitost skupiny

Když kliknete na uzel **Hlasitost skupin** , v podokně **výsledků** zobrazí tyto informace o skupinách hlasitost podle toho, výběr sloupců, které vytvoříte. (V podokně **výsledků** obsahuje následující sloupce, která dokáže nahradit. Klikněte pravým tlačítkem myši **svazky** uzel vyberte **zobrazení**a pak vyberte **Přidat nebo odebrat sloupce**.)

Výsledky sloupce | Popis 
:--------------|:------------ 
Jméno           | Sloupec **název** obsahuje název skupiny hlasitost.
Aplikace    | Sloupec **aplikace** zobrazuje počet VSS autory aktuálně nainstalovanou a spuštěnou na hostiteli Windows.
Vybraná       | **Vybrané** sloupce se zobrazí číslo svazky obsažené ve skupině hlasitost. Nula (0) označuje přidružené k svazky ve skupině hlasitost žádná aplikace.
Importovat       | Sloupec **importovaných** zobrazuje počet importovaných svazky. Kdy nastavena na **hodnotu True**, tento sloupec označuje, že skupinu hlasitost importovaného z portálu Microsoft Azure klasické a nebyl vytvořený v StorSimple snímek správce.
 
>[AZURE.NOTE] Správce snímek StorSimple hlasitost skupiny jsou také zobrazené na kartě **Zálohování zásady** Azure klasické portálu.
 
## <a name="create-a-volume-group"></a>Vytvoření skupiny hlasitosti

Vytvoření skupiny hlasitost pomocí následujícího postupu.

#### <a name="to-create-a-volume-group"></a>Pokud chcete vytvořit skupinu hlasitosti

1. Klikněte na ikony na ploše spuštění StorSimple snímek správce. 

2. V podokně **obor** klikněte pravým tlačítkem **Hlasitosti skupiny**a potom klikněte na **Vytvořit skupinu hlasitost**. 

    ![Vytvoření skupiny hlasitosti](./media/storsimple-snapshot-manager-manage-volume-groups/HCS_SSM_Create_volume_group.png)
 
    Zobrazí se dialogové okno **vytvořit skupinu hlasitost** . 

    ![Hlasitost skupiny dialogové okno Vytvořit novou](./media/storsimple-snapshot-manager-manage-volume-groups/HCS_SSM_CreateVolumeGroup_dialog.png) 

3.  Zadejte následující informace: 

    1. Do pole **název** zadejte jedinečný název pro novou skupinu hlasitost. 

    2. V seznamu **aplikací** vyberte aplikací přidružených svazky, které budete přidávat do skupiny hlasitost. 

        Seznamy **aplikace** pole jenom aplikace, které používají StorSimple svazky a mají služby VSS povoleno pro ně. Zápis VSS. aktivované pouze v případě všechny svazky, které známa pisatele StorSimple svazky. Pokud je aplikace pole prázdné, jsou nainstalované žádné aplikace používající Azure StorSimple svazky a mít podporované služby VSS. (V současnosti Azure StorSimple podporuje Microsoft Exchange a SQL Server) Další informace o služby VSS najdete v článku [Integrace se službou Windows hlasitost stín kopie](storsimple-what-is-snapshot-manager.md#integration-with-windows-volume-shadow-copy-service).

        Pokud vyberte aplikaci automaticky vybrány všechny svazky přidružená. Naopak svazky přidružené k dané aplikace, vyberete aplikace automaticky se vybere v seznamu **aplikací** . 

    3. V dialogovém okně **svazky** zaškrtněte svazky StorSimple přidejte do skupiny hlasitost. 

      - Můžete zahrnout svazky jeden nebo více oddílů. (Víc oddílů svazky může být dynamických disků nebo základní disků s více oddílů.) Objem, která obsahuje více oddílů zpracován jako jednu jednotku. Proto pokud přidáte jenom jedna z oddíly do skupiny hlasitost všechny oddíly se automaticky přidají do příslušné skupiny hlasitost ve stejnou dobu. Přidat více oddílů hlasitost ke skupině hlasitost více oddílů hlasitost zůstane po použije jako jednu jednotku.

      - Prázdný hlasitost skupiny můžete vytvořit tak, že není svazky jim přiřadíte. 

      - Není kombinovat sdílené clusteru svazky (CSVs) a jiných CSVs ve stejné skupině hlasitost. Správce snímek StorSimple nepodporuje kombinaci CSV a -CSV svazky ve stejném snímku. 

4. Kliknutím na **OK** uložte skupině hlasitost.

## <a name="back-up-a-volume-group"></a>Obecnějším údajům skupinu hlasitosti

Pomocí následujícího postupu k obecnějším údajům skupinu hlasitost.

#### <a name="to-back-up-a-volume-group"></a>K obecnějším údajům skupinu hlasitosti

1. Klikněte na ikony na ploše spuštění StorSimple snímek správce.

2. V podokně **obor** rozbalte uzel **Hlasitost skupin** , klikněte pravým tlačítkem na název skupiny a hlasitost a potom klikněte na tlačítko **Přijmout zálohování**. 

    ![Vytvoření zálohy hlasitost skupiny okamžitě](./media/storsimple-snapshot-manager-manage-volume-groups/HCS_SSM_Take_backup.png)

3. V dialogovém okně **Přijmout zálohování** vyberte **Místní snímku** nebo **Cloudu snímek**a potom klikněte na **vytvořit**. 

    ![Prohlédněte dialogové okno zálohy](./media/storsimple-snapshot-manager-manage-volume-groups/HCS_SSM_TakeBackup_dialog.png) 

4. K potvrzení, jestli je spuštěný zálohování, rozbalte uzel **úloh** a potom klikněte na **systém**. Zálohování by měl být uveden.

5. Zobrazení dokončené snímku rozbalte uzel **Katalog zálohování** , rozbalte název skupiny hlasitost a potom klikněte na **Místní snímku** nebo **Cloudu snímku**. Zálohování se zobrazí, pokud je úspěšně dokončený. 

## <a name="edit-a-volume-group"></a>Úprava skupiny hlasitosti

Chcete-li upravit skupinu hlasitost pomocí následujícího postupu.

#### <a name="to-edit-a-volume-group"></a>Chcete-li upravit skupinu hlasitosti

1. Klikněte na ikony na ploše spuštění StorSimple snímek správce.

2. V podokně **obor** rozbalte **Skupiny hlasitost** , klikněte pravým tlačítkem na název skupiny a hlasitost a potom klikněte na **Upravit**. 

3. Zobrazí se dialogové okno **vytvořit skupinu hlasitost **. Můžete změnit položky **název**, **aplikací**a **svazky** . 

4. Klikněte na **OK** uložte provedené změny.

## <a name="delete-a-volume-group"></a>Odstranění skupiny hlasitosti

Pokud chcete odstranit skupinu hlasitost pomocí následujícího postupu. 

>[AZURE.WARNING] Odstraněny také všechny zálohy přidružené ke skupině hlasitost.

#### <a name="to-delete-a-volume-group"></a>Pokud chcete odstranit skupinu hlasitosti

1. Klikněte na ikony na ploše spuštění StorSimple snímek správce. 

2. V podokně **obor** rozbalte **Skupiny hlasitost** , klikněte pravým tlačítkem na název skupiny a hlasitost a potom klikněte na **Odstranit**. 

3. Zobrazí se dialogové okno **Odstranit skupinu hlasitost** . Do textového pole zadejte **Potvrdit** a klikněte na **OK**. 

    Skupina odstraněné hlasitost zmizí ze seznamu v podokně **výsledků** a budou odstraněny všechny zálohy, které jsou přidružené k této skupině hlasitost ze záložní katalogu.

## <a name="next-steps"></a>Další kroky

- Zjistěte, jak lze [pomocí Správce snímek StorSimple ke správě StorSimple řešení](storsimple-snapshot-manager-admin.md).
- Zjistěte, jak lze [pomocí Správce StorSimple snímku můžete vytvořit a spravovat záložní zásady](storsimple-snapshot-manager-manage-backup-policies.md).
