<properties 
   pageTitle="Správa zásad záložní vaší StorSimple | Microsoft Azure"
   description="Vysvětluje, jak můžete použít službu StorSimple správce můžete vytvořit a spravovat ruční zálohování záložní plány a zálohování uchovávání informací."
   services="storsimple"
   documentationCenter="NA"
   authors="SharS"
   manager="carmonm"
   editor=""/>
<tags 
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="TBD"
   ms.date="05/10/2016"
   ms.author="v-sharos"/>

# <a name="use-the-storsimple-manager-service-to-manage-backup-policies-update-2"></a>Použití službu StorSimple správce ke správě záložní zásady (aktualizace 2)

[AZURE.INCLUDE [storsimple-version-selector-manage-backup-policies](../../includes/storsimple-version-selector-manage-backup-policies.md)]

## <a name="overview"></a>Základní informace

Tento kurz vysvětluje, jak na stránce Správce StorSimple služby **Záložní zásady** určit procesů zálohování a zálohování uchovávání informací pro StorSimple svazky. Také popisuje, jak provést ruční zálohování.

Když obecnějším údajům svazku můžete vytvořit místní snímku nebo snímku cloudu. Pokud zálohujete místně připnuté objem, doporučujeme zadat snímek cloudu. Pořizování velkého počtu místní snímky místně připnuté objemu svázáno s sadu dat, která obsahuje hodně konve povede situace, ve kterém můžete rychle spustit z místní místa. Pokud se rozhodnete provádět místní snímky, doporučujeme provést méně denní snímky zálohovat poslední stav je uchovávat v den a odstraňte je.

Udělejte si snímek cloudu místně připnuté objemu kopírujete pouze změněná data do cloudu, kde je deduplicated a komprimovány. 

## <a name="the-backup-policies-page"></a>Na stránce zásady zálohování

Na stránce **Záložní zásady** umožňuje spravovat záložní zásady a naplánovat místních i cloudových snímky. (Záložní zásady umožňují konfigurovat záložní plány a zálohování uchovávání informací pro kolekci svazky.) Zálohování zásady umožňují snímek současně více svazky. To znamená, že zálohování vytvořil zásady zálohování pád konzistentní kopie. Na stránce **Záložní zásady** zobrazí záložní zásad, jejich typů, související svazky, počet zálohy zachovají a možnost povolit tyto zásady.

Na stránce **Záložní zásady** také vám umožní filtrovat existující záložní zásady jedním nebo více z následujících polí:

- **Název zásady** – název přidružené k této zásadě. Různé druhy zásad, patří:

   - Plánované zásady, které se výslovně vytvářejí uživatelem.
   - Automatické zásady, které jsou vytvořeny v případě zálohy výchozí pro tuto možnost hlasitost byla povolena při vytváření hlasitost. Tyto zásady jsou pojmenovaných jako *název_svazku*_Default kde *název_svazku* odkazuje na název objemu StorSimple nakonfigurované uživatelem v portálu Azure klasické. Automatické zásady za následek denní snímky cloudu počínaje čas zařízení 22:30.
   - Importované zásady, které jste původně vytvořil ve snímku správci StorSimple. Mají značku popisující importovaných z zásady hostiteli StorSimple snímek správce.

- **Svazky** – svazky přidružené k této zásadě. Všechny svazky přidružené k zásady zálohování seskupeny při vytvoření zálohy.

- **Poslední úspěšné zálohy** – datum a čas poslední úspěšné zálohy, která byla pořízené tuto zásadu.

- **Další zálohování** – datum a čas následující naplánované zálohování, která bude spuštěná tak, že tuto zásadu.

- **Plány** – počet plány přidružené zásady zálohování.

Jsou často používané operace, které lze provádět na této stránce:

- Přidejte zásady zálohování 
- Přidat nebo změnit plán 
- Odstranění zásady zálohování 
- Provést ruční zálohování 
- Vytvoření vlastní zásady zálohování s více svazky a plány 

## <a name="add-a-backup-policy"></a>Přidejte zásady zálohování

Přidejte zásady zálohování automaticky naplánovat zálohování. Na portálu Azure klasické přidání záložní zásady pro zařízení s StorSimple proveďte následující kroky. Po přidání zásady můžete definovat plán (viz [Přidat nebo změnit plán](#add-or-modify-a-schedule)).

[AZURE.INCLUDE [storsimple-add-backup-policy-u2](../../includes/storsimple-add-backup-policy-u2.md)]

![Video k dispozici](./media/storsimple-manage-backup-policies-u2/Video_icon.png) **Video k dispozici**

Přehrát video, které ukazuje, jak vytvořit místního nebo cloudového zásady zálohování, klikněte [sem](https://azure.microsoft.com/documentation/videos/create-storsimple-backup-policies/).


## <a name="add-or-modify-a-schedule"></a>Přidat nebo změnit plán

Můžete přidat nebo změnit plán, který je připojen k existující záložní zásady na zařízení s StorSimple. Proveďte následující kroky v Azure klasické portál a přidat nebo změnit plán.

[AZURE.INCLUDE [storsimple-add-modify-backup-schedule](../../includes/storsimple-add-modify-backup-schedule-u2.md)]

## <a name="delete-a-backup-policy"></a>Odstranění zásady zálohování

Na portálu Azure klasické odstranění záložní zásady na zařízení s StorSimple proveďte následující kroky.

[AZURE.INCLUDE [storsimple-delete-backup-policy](../../includes/storsimple-delete-backup-policy.md)]


## <a name="take-a-manual-backup"></a>Provést ruční zálohování

Na portálu Azure klasické vytvořit zálohu na vyžádání (ruční) pro jednoho hlasitost proveďte následující kroky.

[AZURE.INCLUDE [storsimple-create-manual-backup](../../includes/storsimple-create-manual-backup.md)]

## <a name="create-a-custom-backup-policy-with-multiple-volumes-and-schedules"></a>Vytvoření vlastní zásady zálohování s více svazky a plány

Proveďte následující kroky v portálu Azure klasické k vytvoření vlastní záložní zásad, které obsahuje více svazky a plány.

[AZURE.INCLUDE [storsimple-create-custom-backup-policy](../../includes/storsimple-create-custom-backup-policy-u2.md)]


## <a name="next-steps"></a>Další kroky

Další informace o [použití služby StorSimple správce ke správě StorSimple zařízení](storsimple-manager-service-administration.md).
