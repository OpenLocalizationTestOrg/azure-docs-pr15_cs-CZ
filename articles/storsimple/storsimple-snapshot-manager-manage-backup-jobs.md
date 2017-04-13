<properties 
   pageTitle="StorSimple snímek Správce úloh zálohování | Microsoft Azure"
   description="Popisuje, jak používat modulu snap-in konzoly MMC StorSimple snímek správce k zobrazení a Správa úloh zálohování plánované aktuálně spuštěných a dokončené."
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


# <a name="use-storsimple-snapshot-manager-to-view-and-manage-backup-jobs"></a>Pomocí Správce snímek StorSimple zobrazení a Správa úloh zálohování

## <a name="overview"></a>Základní informace

Uzel **úlohy** v podokně **obor** zobrazuje **Naplánované**, **posledních 24 hodin**a **spuštění** úlohy zálohování, které jste začali interaktivně nebo nakonfigurované zásady. 

Tento kurz vysvětluje, jak můžete pomocí uzel **úlohy** k zobrazení informací o plánované poslední a aktuálně spuštěných úlohy zálohování. (V podokně **výsledků** se zobrazí seznam úlohy a odpovídající informací.) Kromě toho kliknete pravým tlačítkem úloh a najdete v článku místní nabídky, která jsou uvedeny dostupné akce.

## <a name="view-scheduled-jobs"></a>Zobrazení naplánovaných úloh

Pomocí následujícího postupu zobrazíte naplánovaných úloh zálohování.

#### <a name="to-view-scheduled-jobs"></a>Chcete-li zobrazit naplánovaných úloh

1. Klikněte na ikony na ploše spuštění StorSimple snímek správce. 

2. V podokně **obor** rozbalte uzel **úlohy** a klikněte na **plánu**. V podokně **výsledků** se zobrazí následující informace:

    - **Název** – název plánovaného snímku

    - **Další spuštění** – datum a čas na další snímek naplánované

    - **Poslední spustit** – datum a čas poslední plánované snímku

    >[AZURE.NOTE] Pro jednorázové pouze snímky **Další spuštění** a **Poslední spuštění** budou stejné. 
 
    ![Naplánovaných úloh zálohování](./media/storsimple-snapshot-manager-manage-backup-jobs/HCS_SSM_Jobs_scheduled.png) 
 
3. Abyste mohli provést další akce v určitém projektu, klikněte pravým tlačítkem myši na název projektu v podokně **výsledků** a vyberte jednu z možností nabídky.

## <a name="view-recent-jobs"></a>Zobrazení posledních úlohy

Pomocí následujícího postupu zobrazení zálohování a obnovení úloh, které byly dokončeny za posledních 24 hodin.

#### <a name="to-view-recent-jobs"></a>Pokud chcete zobrazit poslední úlohy

1. Klikněte na ikony na ploše spuštění StorSimple snímek správce.

2. V podokně **obor** rozbalte uzel **úlohy** a klikněte na **poslední 24 hodin**. Podokno **výsledků** zobrazuje úlohy zálohování posledních 24 hodin (Chcete-li maximálně 64 úlohy). V podokně **výsledků** v závislosti na možnostech **zobrazení** , které zadáte se zobrazí následující informace:

    - **Název** – název plánované snímek.
 
    - **Začínáme** – datum a čas, kdy vzniku snímku.

    - **Zastaveno** – datum a čas dokončení nebo byl ukončen snímek.

    - Časový limit **Uplynulý** – dobu mezi **Začínáme** a **Zastaveno** .

    - **Stav** – stav naposledy dokončeného projektu. **Úspěšné** označuje, že zálohování bylo úspěšně vytvořeno. **Neúspěšné** označuje, že úlohy nebyla úspěšně spuštěna.

    - **Informace o** – důvod chyby.

    - **Bajtů zpracování (MB)** – množství dat ze skupiny svazku, které byl zpracována (MB). 

    ![Úlohy, které spustili za posledních 24 hodin](./media/storsimple-snapshot-manager-manage-backup-jobs/HCS_SSM_Jobs_Last_24_hours.png) 

3. Abyste mohli provést další akce v určitém projektu, klikněte pravým tlačítkem myši na název projektu v podokně **výsledků** a vyberte jednu z možností nabídky.

    ![Odstranění úlohy](./media/storsimple-snapshot-manager-manage-backup-catalog/HCS_SSM_Delete_backup.png) 
     
## <a name="view-currently-running-jobs"></a>Zobrazení aktuálně spuštěných úloh

Pomocí následujícího postupu pro zobrazení úloh, které jsou aktuálně spuštěných.

#### <a name="to-view-currently-running-jobs"></a>Chcete-li zobrazit aktuálně spuštěných úloh

1. Klikněte na ikony na ploše spuštění StorSimple snímek správce.

2. V podokně **obor** rozbalte uzel **úlohy** a klikněte na **systém**. V závislosti na možnostech **zobrazení** , které zadáte tyto informace se zobrazí v podokně **výsledků** : 

    - **Název** – název plánované snímek.

    - **Začínáme** – datum a čas, kdy vzniku snímku.

    - **Kontrolní bod** – aktuální akce zálohování.

    - **Stav** – procenta dokončení.
    
    - **Uplynulý** – množství času, který uplynul od začátku zálohování. 

    - **Průměrná výkon (MB)** – poměr celkový počet bajtů dat zpracovaných, celkovou dobu potřebnou zpracování (MB).

    - **Bajtů zpracování (MB)** – celkový počet bajtů dat zpracovaných (v MB).

    - **Bajtů zapsán (MB)** – celkový počet bajtů dat psaných (MB). Obsahuje data, jakož i metadata a je obvykle větší než polích zpracování bajtů.

    ![Úlohy aktuálně spuštěných](./media/storsimple-snapshot-manager-manage-backup-jobs/HCS_SSM_Jobs_running.png)

3. Abyste mohli provést další akce v určitém projektu, klikněte pravým tlačítkem myši na název projektu v podokně **výsledků** a vyberte jednu z možností nabídky.

## <a name="next-steps"></a>Další kroky

- Zjistěte, jak lze [pomocí Správce snímek StorSimple ke správě StorSimple řešení](storsimple-snapshot-manager-admin.md).
- Zjistěte, jak lze [pomocí Správce snímek StorSimple ke správě záložní katalogu](storsimple-snapshot-manager-manage-backup-catalog.md).















            


 

