<properties 
   pageTitle="Zobrazení a Správa úloh StorSimple | Microsoft Azure"
   description="Popisuje stránce StorSimple Správce služby úlohy a jak se používá ke sledování poslední aktuální a naplánovaných úloh zálohování."
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
   ms.workload="TBD"
   ms.date="08/17/2016"
   ms.author="alkohli" />

# <a name="use-the-storsimple-manager-service-to-view-and-manage-storsimple-jobs"></a>K zobrazení a správa StorSimple úloh použijte službu StorSimple správce

[AZURE.INCLUDE [storsimple-version-selector-manage-jobs](../../includes/storsimple-version-selector-manage-jobs.md)]

## <a name="overview"></a>Základní informace

Stránce **úlohy** poskytuje jediné centrální portál pro zobrazení a Správa úloh zahájené na zařízeních připojení ke službě StorSimple správce. Můžete zobrazit plánované pracovního, dokončený a selhalo úlohy pro několika zařízeních. Výsledky jsou uvedeny v tabulkovém formátu. 

![Úlohy](./media/storsimple-manage-jobs/HCS_JobsPage.png)

Můžete rychle najít úlohy, které mají zájem pomocí filtrování na základě polí jako:

- **Stav** – úlohy můžete běžet plánované, selhalo, dokončené, zrušení nebo zrušená.

- **Typ** – úlohy se dají vytvářet důsledku naplánované nebo zálohu na vyžádání (**Trvat záložní**), klonováním, obnovení zařízení nebo operace aktualizace.

- **Zařízení** – úlohy zahájí na určitých zařízení připojení ke službě.

- **Od a do** – úlohy můžete filtrovat podle rozsah dat a časů.

Filtrované úlohy se pak: tabulkový na základě následujícími atributy:

- **Typ** – zálohování, klonovat, obnovení, převzetí nebo aktualizovat.

- **Stav** – plánované, selhalo, dokončené, zrušení nebo zrušená.

- **Entita** – projektů, které jde přidružit svazku, zásady zálohování nebo zařízení. Úlohy klonovat přidružen objem, že je přidružená k zásady zálohování zálohovací úlohy. Obnovení havárie (DR) nebo obnovení je vytvořen projektu zařízení.

- **Zařízení** – název zařízení, na které byl spuštěn projektu.

- **Začínáme na** – čas zahájení projektu.

- **Průběh** – procento dokončení spuštěná úloha. Dokončené práce to buďte vždy 100 %.

Seznam úloh aktualizaci každých 30 sekund.

Na této stránce můžete provádět následující akce související s projektem:

- Zobrazení podrobností o projektu

- Zrušení úlohy

## <a name="view-job-details"></a>Zobrazení podrobností o projektu

Takto zobrazíte podrobnosti o všech projektu.

#### <a name="to-view-job-details"></a>Chcete-li zobrazit podrobnosti projektu

1. Na stránce **úlohy** zobrazte úlohy, které mají zájem spuštěním dotazu s příslušnou filtry. Dokončení prohledávání systém nebo zrušit úlohy.

2. Vyberte úkol.

3. V dolní části stránky klepněte na tlačítko **Podrobnosti**.

4. V dialogovém okně **Podrobnosti úlohy zálohování** můžete zobrazit stav, podrobnosti, Statistika doby a Statistika data.

## <a name="cancel-a-job"></a>Zrušení úlohy

Proveďte následující kroky pracovního zrušíte.

### <a name="to-cancel-a-job"></a>Chcete-li zrušit projektu

1. Na stránce **úlohy** zobrazte pracovního úlohy, kterou chcete zrušit spuštěním dotazu s příslušnou filtry.

1. Vyberte úlohu.

1. V dolní části stránky klikněte na **Zrušit**.

1. Po zobrazení výzvy k potvrzení klikněte na **Ano**.

Tento úkol je nyní zrušeno.

## <a name="next-steps"></a>Další kroky

- Zjistěte, jak [Spravovat pravidla záložní StorSimple](storsimple-manage-backup-policies.md).

- Zjistěte, jak [používat službu StorSimple správce ke správě StorSimple zařízení](storsimple-manager-service-administration.md).
