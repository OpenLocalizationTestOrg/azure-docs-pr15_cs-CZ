<properties 
   pageTitle="Zobrazení a Správa úloh virtuální pole StorSimple | Microsoft Azure"
   description="Popisuje stránce StorSimple Správce služby úlohy a jak se používá ke sledování úloh poslední a aktuální pole virtuální StorSimple."
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
   ms.workload="na"
   ms.date="06/07/2016"
   ms.author="alkohli" />

# <a name="use-the-storsimple-manager-service-to-view-jobs-for-the-storsimple-virtual-array"></a>Slouží k zobrazení úloh pole virtuální StorSimple službu StorSimple správce

## <a name="overview"></a>Základní informace

Na stránce **úlohy** obsahuje portálem jednotného centrální pro zobrazení a Správa úloh, které jsou spuštěním virtuální pole (označovaná taky jako místní virtuální zařízení), který je spojený s StorSimple Správce služby. Můžete zobrazit úlohy pracovního dokončený a nezdařeném uložení více virtuální zařízení. Výsledky jsou uvedeny v tabulkovém formátu. 

![Úlohy](./media/storsimple-ova-manage-jobs/ovajobs1.png)

Můžete rychle najít úlohy, které mají zájem pomocí filtrování na základě polí jako:

- **Stav** – můžete vyhledat všechny úlohy pracovního, dokončené nebo selhalo.
- **Od a do** – úlohy můžete filtrovat podle rozsah dat a časů.
- **Typ** – typ úlohy lze all, zálohování, obnovení, převzetí, stáhněte si aktualizace nebo instalovat aktualizace.
- **Zařízení** – úlohy zahájí na konkrétní zařízení připojení ke službě. Filtrované úlohy se pak: tabulkový na základě následujícími atributy:

    - **Typ** – typ úlohy lze all, zálohování, obnovení, převzetí, stáhněte si aktualizace nebo instalovat aktualizace.

    - **Stav** – úlohy může být všechny spuštěné, Dokončeno nebo se nezdařila.

    - **Entita** – úlohy může být přidružené ke objem, sdílet nebo zařízení. 

    - **Zařízení** – název zařízení, na které byl spuštěn projektu.

    - **Začínáme na** – čas zahájení projektu.

    - **Průběh** – procento dokončení spuštěná úloha. Dokončené práce to buďte vždy 100 %.

Seznam úloh aktualizaci každých 30 sekund.

## <a name="view-job-details"></a>Zobrazení podrobností o projektu

Takto zobrazíte podrobnosti o všech projektu.

#### <a name="to-view-job-details"></a>Chcete-li zobrazit podrobnosti projektu

1. Na stránce **úlohy** zobrazte úlohy, které mají zájem spuštěním dotazu s příslušnou filtry. Můžete vyhledávat dokončené nebo pracovního úloh.

2. Vyberte úkol ze seznamu tabulkových úloh.

3. V dolní části stránky klepněte na tlačítko **Podrobnosti**.

4. V dialogovém okně **Podrobnosti** můžete zobrazit stav, podrobnosti a Statistika doby. Následující obrázek ukazuje příklad dialogového okna **Podrobnosti úlohy zálohování** .
 
    ![Stránka Podrobnosti projektu](./media/storsimple-ova-manage-jobs/ovajobs2.png)

#### <a name="job-failures-when-the-virtual-machine-is-paused-in-the-hypervisor"></a>Chyby úlohy v případě virtuální počítač je v hypervisor

Při práci v průběh vašeho StorSimple virtuální matice a zařízení (virtuálního počítače v hypervisoru zřízení) se pozastaví pro větší než 15 minut, úlohy se nezdaří. To vzhledem k času virtuální pole StorSimple probíhá nejsou synchronizovány s časem Microsoft Azure. Následující snímek obrazovky je znázorněn příklad chyby úlohy obnovit.

![Obnovení selhání projektu](./media/storsimple-ova-manage-jobs/restorejobfailure.png)

Použijí se tyto chyby na úlohy zálohování, obnovení, aktualizace a překlopení. Pokud váš počítač virtuální máte k dispozici v Hyper-V, počítači postupně synchronizovat čas s vaší hypervisoru. Když k tomu dojde, můžete restartovat práce. 

## <a name="next-steps"></a>Další kroky

[Naučte se používat místní web uživatelského rozhraní pro spravovat své StorSimple virtuální pole](storsimple-ova-web-ui-admin.md).
