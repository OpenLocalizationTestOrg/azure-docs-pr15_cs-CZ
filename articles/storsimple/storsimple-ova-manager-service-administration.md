<properties 
   pageTitle="Pole virtuální StorSimple správce správy | Microsoft Azure"
   description="Naučte se spravovat své StorSimple místní virtuální pole pomocí službu StorSimple správce na portálu Azure klasické."
   services="storsimple"
   documentationCenter=""
   authors="alkohli"
   manager="carmonm"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/11/2016"
   ms.author="alkohli" />

# <a name="use-the-storsimple-manager-service-to-administer-your-storsimple-virtual-array"></a>Umožňuje spravovat své virtuální pole StorSimple službu StorSimple správce

![tok procesu instalace](./media/storsimple-ova-manager-service-administration/manage4.png)

## <a name="overview"></a>Základní informace

Tento článek popisuje StorSimple Správce služby rozhraní, včetně způsobu připojení a možnosti k dispozici a poskytuje odkazy na konkrétní pracovní postupy, které lze provést pomocí tohoto uživatelského rozhraní. 

Po přečtení v tomto článku se dozvíte, jak:

- Připojení ke službě StorSimple správce
- Navigace v uživatelském rozhraní StorSimple správce
- Spravovat své StorSimple virtuální pole prostřednictvím služby StorSimple správce

> [AZURE.NOTE] Správa dostupné možnosti pro zařízení řady StorSimple 8000 zobrazíte přejděte na článek [použití službu StorSimple správce ke správě StorSimple zařízení](storsimple-manager-service-administration.md).

## <a name="connect-to-the-storsimple-manager-service"></a>Připojení ke službě StorSimple správce

Služba StorSimple správce spustí v Microsoft Azure a připojuje k více StorSimple virtuální matice. Používáte ke správě tato zařízení centrální klasické portál Microsoft Azure spuštěné v prohlížeči. Připojení ke službě StorSimple správce, postupujte takto.

#### <a name="to-connect-to-the-service"></a>Připojení ke službě

1. Přejděte na [https://manage.windowsazure.com/](https://manage.windowsazure.com/).

2. Pomocí svých přihlašovacích údajů účtu Microsoft, přihlaste se k portálu Microsoft Azure klasické (umístěný v pravé horní části podokna).

3. Posuňte se dolů v levém navigačním podokně pro přístup ke službě StorSimple správce.

    ![Přejděte do služby](./media/storsimple-ova-manager-service-administration/admin-scroll.png)

## <a name="navigate-the-storsimple-manager-service-ui"></a>Přejděte StorSimple Správce služby UI

Navigační hierarchie pro službu StorSimple správce uživatelské rozhraní se zobrazují v následující tabulce.

- Cílová stránka **StorSimple správce** přenese vás na stránky úroveň služby UI k dispozici virtuální matic v rámci služby.

- Na stránce **zařízení** přejdete na stránky uživatelského rozhraní zařízení – úrovně k dispozici zvláštní virtuální pole.

#### <a name="storsimple-manager-service-navigational-hierarchy"></a>Navigační hierarchie StorSimple Správce služeb

|Cílová stránka|Stránky úrovně služeb|Úroveň zařízení stránky|
|---|---|---|
|Služba StorSimple správce|Řídicí panel (služba)|Řídicí panel (zařízení)|
||Zařízení →|Sledování|
||Zálohování katalogu|Sdílené složky (souborovém serveru) nebo </br>Svazky (iSCSI server)|
||Konfigurace (služba)|Konfigurace (zařízení)|
||Úlohy|Údržby|
||Upozornění|

## <a name="use-the-storsimple-manager-service-to-perform-management-tasks"></a>Umožňuje provádět úlohy správy službu StorSimple správce

Následující tabulka obsahuje souhrn všech běžné úlohy správy a složitých pracovních postupů, které lze provádět v rámci StorSimple Správce služby UI. Tyto úkoly jsou uspořádány založené na stránkách uživatelského rozhraní, ve kterých jsou spuštěná.

Další informace o jednotlivých pracovního postupu klikněte na odpovídající pomocí postupu v tabulce.

#### <a name="storsimple-manager-workflows"></a>Pracovní postupy StorSimple správce

|Pokud chcete, můžete to udělat...|Přejděte na této stránce uživatelského rozhraní...|Tímto postupem|
|---|---|---|
|Vytvoření služby</br>Odstranění služby</br>Získání klíč registrace služby</br>Obnovit klíč registrace služby|Služba StorSimple správce|[Nasazení službu StorSimple správce](storsimple-ova-manage-service.md)|
|Změna šifrovací klíč datové služby</br>Zobrazení protokolů operace|Řídicí panel StorSimple → Správce služeb|[Použití řídicí panel služeb StorSimple](storsimple-ova-service-dashboard.md)|
|Deaktivace virtuální matice</br>Odstranění virtuální matice|StorSimple Správce služby → zařízení|[Deaktivace nebo odstranění virtuální matice](storsimple-ova-deactivate-and-delete-device.md)|
|Převzetí havárie využití a zařízení</br>Zjistit předpoklady pro překlopení</br>Přepnutí do virtuálního zařízení</br>Obchodní kontinuitu havárie obnovení (BCDR)</br>Chyby při obnovení havárie|StorSimple Správce služby → zařízení|[Obnovení a zařízení převzetí havárie pro své StorSimple virtuální pole](storsimple-ova-failover-dr.md)|
|Obecnějším údajům sdílené položky a svazky</br>Provést ruční zálohování</br>Změna plánu zálohování</br>Zobrazení existujícího zálohování|Katalog zálohování → StorSimple Správce služeb|[Obecnějším údajům své StorSimple virtuální pole](storsimple-ova-backup.md)|
|Obnovení sdílené položky ze záložní sady</br>Obnovení svazky ze záložní sady</br>Obnovení položky úrovně (pouze souborový server)|Katalog zálohování StorSimple → Správce služeb|[Obnovení ze zálohy své StorSimple virtuální pole](storsimple-ova-restore.md)|
|Informace o účtech úložiště</br>Přidání účtu úložiště</br>Úprava účtu úložiště</br>Odstranění účtu úložiště|Konfigurace služby → StorSimple správce|[Správa úložiště účtů pro pole virtuální StorSimple](storsimple-ova-manage-storage-accounts.md)|
|Informace o záznamů kontrol přístup</br>Přidání nebo úprava záznamu řízení přístupu </br>Odstranění záznamu řízení přístupu|Konfigurace služby → StorSimple správce|[Správa záznamů kontrol přístup pro pole virtuální StorSimple](storsimple-ova-manage-acrs.md)|
|Zobrazení podrobností o projektu|StorSimple Správce služby → úlohy| [Správa úloh StorSimple virtuální matice](storsimple-ova-manage-jobs.md)|
|Konfigurace nastavení oznámení</br>Přijímání oznámení</br>Správa upozornění</br>Revize upozornění|Oznámení služby → StorSimple správce|[Zobrazení a Správa oznámení pro pole virtuální StorSimple](storsimple-ova-manage-alerts.md)|
|Změna hesla správce zařízení|StorSimple Správce služby → zařízení → konfigurace|[Změna hesla správce zařízení StorSimple virtuální matice](storsimple-ova-change-device-admin-password.md)|
|Instalace aktualizací|StorSimple Správce služby → zařízení → údržby|[Aktualizace virtuální matice](storsimple-ova-install-update-01.md)|

>[AZURE.NOTE] Je nutné použít [místní web uživatelského rozhraní](storsimple-ova-web-ui-admin.md) pro tyto věci:
>
>- [Načtení šifrovací klíč datové služby](storsimple-ova-web-ui-admin.md#get-the-service-data-encryption-key)
>- [Vytvoření balíčku podpory](storsimple-ova-web-ui-admin.md#generate-a-log-package)
>- [Zastavení a nové spuštění virtuální matice](storsimple-ova-web-ui-admin.md#shut-down-and-restart-your-device)

##<a name="next-steps"></a>Další kroky
Informace o webu uživatelského rozhraní a jak se používá najdete v [prostřednictvím](storsimple-ova-web-ui-admin.md)webu StorSimple uživatelského rozhraní pro spravovat své StorSimple virtuální pole.
