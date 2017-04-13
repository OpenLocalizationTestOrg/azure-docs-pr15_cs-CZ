<properties 
   pageTitle="Zálohování zásady StorSimple snímek správce | Microsoft Azure"
   description="Popisuje, jak používat modulu snap-in konzoly MMC StorSimple snímek správce k vytváření a správa záložní zásad, kterými se řídí plánované zálohy."
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
   ms.date="05/12/2016"
   ms.author="v-sharos" />

# <a name="use-storsimple-snapshot-manager-to-create-and-manage-backup-policies"></a>Použití Správce StorSimple snímku můžete vytvořit a spravovat záložní zásady

## <a name="overview"></a>Základní informace

Zásady zálohování vytvoří plán pro zálohování dat hlasitost místně nebo v cloudu. Při vytváření zásady zálohování můžete použít taky zásady uchovávání informací. (Maximálně 64 snímků můžete uchovávat.) Další informace o záložní zásad najdete v článku [zálohy typů](storsimple-what-is-snapshot-manager.md#backup-type) v [řady StorSimple 8000: hybridní cloudové řešení](storsimple-overview.md).

Tento kurz vysvětluje postup:

- Vytvoření záložní zásad 
- Úprava zásady zálohování 
- Odstranění zásady zálohování 

## <a name="create-a-backup-policy"></a>Vytvoření záložní zásad

Pomocí následujícího postupu vytvořte nové zálohování.

#### <a name="to-create-a-backup-policy"></a>Vytvoření zásady zálohování

1. Klikněte na ikony na ploše spuštění StorSimple snímek správce.

2. V podokně **obor** klikněte pravým tlačítkem myši **Zásady zálohování**a klikněte na **Vytvořit zásady zálohování**.

    ![Vytvoření záložní zásad](./media/storsimple-snapshot-manager-manage-backup-policies/HCS_SSM_Create_BU_policy.png)

    Zobrazí se dialogové okno **vytvořit zásady** . 

    ![Vytvoření zásad – obecné](./media/storsimple-snapshot-manager-manage-backup-policies/HCS_SSM_Create_policy_general.png)

3. Na kartě **Obecné** zadejte následující informace:

   1. Do textového pole **název** zadejte název zásady.

   2. Do textového pole **Hlasitost skupiny** zadejte název skupiny hlasitost přidružené k této zásadě.

   3. Vyberte **místní snímku** nebo **cloudu snímek**.

   4. Vyberte požadovanou velikost snímků zachovat. Pokud vyberete **všechny**, 64 snímky budou zachovají (maximálně). 

4. Klikněte na kartu **plán** .

    ![Vytvoření zásad – karta plán](./media/storsimple-snapshot-manager-manage-backup-policies/HCS_SSM_Create_policy_schedule.png)

5. Na kartě **plán** zadejte následující informace: 

   1. Zaškrtněte políčko **Povolit** naplánovat další zálohování.

   2. V části **Nastavení**vyberte **jednorázové**, **denní**, **týdenní**nebo **měsíční**. 

   3. Do pole **Spustit** klikněte na ikonu Kalendář a vyberte datum zahájení.

   4. V části **Upřesnit nastavení**můžete nastavit volitelné opakování plány a koncové datum.

   5. Klikněte na **OK**.

Po vytvoření záložní zásady se zobrazí v podokně **výsledků** následující informace:

- **Název** – název zásady zálohování.

- **Typ** – místní snímku nebo cloudu snímku.

- **Hlasitost skupiny** – skupině hlasitost přidružené k této zásadě.

- **Uchovávání informací** – počtu snímků zachován. Maximální hodnota je 64.

- **Vytvořeno** : datum vytvoření tuto zásadu.

- **Povoleno** – jestli zásady právě efekt: **True** značí, že je v podstatě; **False** označuje, že není platná. 

## <a name="edit-a-backup-policy"></a>Úprava zásady zálohování

Chcete-li upravit existující záložní zásady pomocí následujícího postupu.

#### <a name="to-edit-a-backup-policy"></a>Chcete-li upravit zásady zálohování

1. Klikněte na ikony na ploše spuštění StorSimple snímek správce. 

2. V podokně **obor** klikněte na uzel **Zásady zálohování** . Zálohování zásady se zobrazí v podokně **výsledků** . 

3. Klikněte pravým tlačítkem myši na zásadu, kterou chcete upravit a klikněte na **Upravit**. 

    ![Úprava zásady zálohování](./media/storsimple-snapshot-manager-manage-backup-policies/HCS_SSM_Edit_BU_policy.png) 

4. Po zobrazení okna **vytvořit zásady** , zadejte požadované změny a klikněte na **OK**. 

## <a name="delete-a-backup-policy"></a>Odstranění zásady zálohování

Chcete-li odstranit zásady zálohování pomocí následujícího postupu.

#### <a name="to-delete-a-backup-policy"></a>Chcete-li odstranit zásady zálohování

1. Klikněte na ikony na ploše spuštění StorSimple snímek správce. 

2. V podokně **obor** klikněte na uzel **Zásady zálohování** . Zálohování zásady se zobrazí v podokně **výsledků** . 

3. Klikněte pravým tlačítkem zásady zálohování, který chcete odstranit a pak klikněte na **Odstranit**.

4. Po zobrazení žádosti o potvrzení klepněte na tlačítko **Ano**.

    ![Zásady zálohování potvrzení odstranění](./media/storsimple-snapshot-manager-manage-backup-policies/HCS_SSM_Delete_BU_policy.png)

## <a name="next-steps"></a>Další kroky

- Zjistěte, jak lze [pomocí Správce snímek StorSimple ke správě StorSimple řešení](storsimple-snapshot-manager-admin.md).
- Zjistěte, jak lze [pomocí Správce snímek StorSimple zobrazení a Správa úloh zálohování](storsimple-snapshot-manager-manage-backup-jobs.md).
