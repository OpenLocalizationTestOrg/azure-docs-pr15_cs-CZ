<properties 
   pageTitle="Správa služby StorSimple správce | Microsoft Azure"
   description="Naučte se spravovat vaše zařízení StorSimple pomocí službu StorSimple správce na portálu Azure klasické."
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
   ms.date="09/21/2016"
   ms.author="alkohli" />

# <a name="use-the-storsimple-manager-service-to-administer-your-storsimple-device"></a>Umožňuje spravovat zařízení s StorSimple službu StorSimple správce

## <a name="overview"></a>Základní informace

Tento článek popisuje StorSimple Správce služby rozhraní, včetně jak připojit je k dispozici různé možnosti a odkazy, na konkrétní pracovní postupy, které lze provést pomocí tohoto uživatelského rozhraní. Platí tyto pokyny pro obě; fyzické StorSimple a virtuální zařízení.

Po přečtení v tomto článku se dozvíte na:

- Připojení ke službě StorSimple správce
- Navigace v uživatelském rozhraní StorSimple správce
- Správa zařízení StorSimple prostřednictvím služby StorSimple správce


## <a name="connect-to-storsimple-manager-service"></a>Připojení ke službě StorSimple správce

Služba StorSimple správce spustí v Microsoft Azure a pak se připojí na několika zařízeních StorSimple. Používáte ke správě tato zařízení centrální klasické portál Microsoft Azure spuštěné v prohlížeči. Připojení ke službě StorSimple správce, postupujte takto.

#### <a name="to-connect-to-the-service"></a>Připojení ke službě

1. Přejděte na [https://manage.windowsazure.com/](https://manage.windowsazure.com/).

1. Pomocí svých přihlašovacích údajů účtu Microsoft, přihlaste se k portálu Microsoft Azure klasické (umístěný v pravé horní části podokna).

1. Posuňte se dolů v levém navigačním podokně pro přístup ke službě StorSimple správce.


## <a name="navigate-storsimple-manager-service-ui"></a>Přejděte StorSimple Správce služby UI

Navigační hierarchie pro službu StorSimple správce uživatelské rozhraní se zobrazují v následující tabulce.

- **Správce StorSimple** cílovou stránku přejdete na úrovni služeb stránky uživatelského rozhraní pro všechna zařízení v rámci služby.

- Stránky **zařízení** přenese vás na stránky uživatelského rozhraní zařízení – úrovně k určitému zařízení.

- **Hlasitost kontejnery** stránky přejdete na stránku objem, zobrazující všechny svazky přidružené k zařízení.


#### <a name="storsimple-manager-service-navigational-hierarchy"></a>Navigační hierarchie StorSimple Správce služeb

|Cílová stránka|Stránky úrovně služeb|Úroveň zařízení stránky|Úroveň zařízení stránky|
|---|---|---|---|
|Služba StorSimple správce|Řídicí panel služeb|Řídicí panel zařízení||
||Zařízení →|Sledování|
||Zálohování katalogu|Hlasitost containers→|Svazky|
||Konfigurace (služba)|Zálohování zásady||
||Úlohy|Konfigurace (zařízení)|
||Upozornění|Údržby|

![Video k dispozici](./media/storsimple-manager-service-administration/Video_icon.png) **Video k dispozici**

Podívejte se na video, který vás provede uživatelského rozhraní StorSimple Správce služby, klikněte [sem](https://azure.microsoft.com/documentation/videos/storsimple-manager-service-overview/).

## <a name="administer-storsimple-device-using-storsimple-manager-service"></a>Správa zařízení StorSimple pomocí StorSimple Správce služeb

Následující tabulka obsahuje souhrn všech běžné úlohy správy a složitých pracovních postupů, které lze provádět v rámci StorSimple Správce služby UI. Tyto úkoly jsou uspořádány založené na stránkách uživatelského rozhraní, ve kterých jsou spuštěná.

Další informace o jednotlivých pracovního postupu klikněte na odpovídající pomocí postupu v tabulce.

#### <a name="storsimple-manager-workflows"></a>Pracovní postupy StorSimple správce

|Pokud chcete, můžete to udělat...|Přejděte na této stránce uživatelského rozhraní...|Pomocí tohoto postupu.|
|---|---|---|
|Vytvoření služby</br>Odstranění služby</br>Získání služby registračního klíče</br>Obnovit služby registračního klíče|Služba StorSimple správce|[Nasazení StorSimple Správce služeb](storsimple-manage-service.md)
|Změna šifrovací klíč datové služby</br>Zobrazení protokolů operace|Řídicí panel StorSimple → Správce služeb|[Použití řídicí panel StorSimple Správce služeb](storsimple-service-dashboard.md)|
|Deaktivace zařízení</br>Odstranění zařízení|StorSimple Správce služby → zařízení|[Deaktivace nebo odstranění zařízení](storsimple-deactivate-and-delete-device.md)|
|Další informace o převzetí havárie využití a zařízení</br>Přepnutí do fyzické zařízení</br>Přepnutí do virtuálního zařízení</br>Obchodní kontinuitu havárie obnovení (BCDR)|StorSimple Správce služby → zařízení|[Převzetí a havárie obnovení StorSimple zařízení](storsimple-device-failover-disaster-recovery.md)|
|Seznam zálohy svazku</br>Vyberte sadu zálohování</br>Odstranění záložní sady|Katalog zálohování StorSimple → Správce služeb|[Správa zálohování](storsimple-manage-backup-catalog.md)|
|Klonovat svazku|Katalog zálohování StorSimple → Správce služeb|[Klonovat svazku](storsimple-clone-volume.md)|
|Obnovení záložní sady|Katalog zálohování StorSimple → Správce služeb|[Obnovení záložní sady](storsimple-restore-from-backup-set.md)|
|Informace o účtech úložiště</br>Přidání účtu úložiště</br>Úprava účtu úložiště</br>Odstranění účtu úložiště</br>Klíčové otočení v prostoru úložiště účtů|Konfigurace služby → StorSimple správce|[Správa úložiště účtů](storsimple-manage-storage-accounts.md)|
|Šablony šířky pásma</br>Přidání šablony šířky pásma</br>Úprava šablony šířky pásma</br>Odstranění šablony šířky pásma</br>Použít výchozí šablonu šířky pásma</br>Vytvoření celodenní zvláštní šablony šířky pásma, začínajícím v určeném časovém|Konfigurace služby → StorSimple správce|[Správa šablon šířky pásma](storsimple-manage-bandwidth-templates.md)|
|Informace o záznamů kontrol přístup</br>Vytvoření záznamu řízení přístupu</br>Upravit záznam řízení přístupu</br>Odstranění záznamu řízení přístupu|Konfigurace služby → StorSimple správce|[Správa záznamů kontrol přístup](storsimple-manage-acrs.md)|
|Zobrazení podrobností o projektu</br>Zrušení úlohy|StorSimple Správce služby → úlohy|[Správa úloh](storsimple-manage-jobs.md)
|Přijímání oznámení</br>Správa upozornění</br>Revize upozornění|Oznámení služby → StorSimple správce|[Zobrazení a správa StorSimple upozornění](storsimple-manage-alerts.md)
|Zobrazení připojeného iniciátory</br>Vyhledání pořadové číslo zařízení</br>Vyhledání cílové IQN|StorSimple Správce služby → zařízení → řídicího panelu|[Používání řídicích panelů StorSimple zařízení](storsimple-device-dashboard.md)|
|Vytváření sledování grafů|StorSimple Správce služby → zařízení → sledovat|[Sledování StorSimple zařízení](storsimple-monitor-device.md)|
|Přidání kontejneru hlasitosti</br>Upravit hlasitost kontejneru</br>Odstranění kontejneru hlasitosti|StorSimple Správce služby → zařízení → hlasitost kontejnery|[Správa hlasitost kontejnery](storsimple-manage-volume-containers.md)|
|Přidání svazku</br>Úprava svazku</br>Objem jako offline</br>Odstranění svazku</br>Sledování svazku|StorSimple Správce služby → zařízení → hlasitost kontejnery → svazky|[Správa svazky](storsimple-manage-volumes.md)|
|Úprava nastavení zařízení</br>Úprava nastavení času</br>Změna nastavení DNS.md</br>Konfigurace sítě rozhraní|StorSimple Správce služby → zařízení → konfigurace|[Změna konfigurace zařízení pro zařízení s StorSimple](storsimple-modify-device-config.md)|
|Nastavení proxy serveru webové zobrazení|StorSimple Správce služby → zařízení → konfigurace|[Konfigurace proxy serveru webové pro zařízení](storsimple-configure-web-proxy.md)|
|Změna hesla správce zařízení</br>Změnit heslo správce StorSimple snímku|StorSimple Správce služby → zařízení → konfigurace|[Změna hesla StorSimple](storsimple-change-passwords.md)|
|Konfigurace Vzdálená správa|StorSimple Správce služby → zařízení → konfigurace|[Připojení vzdálených StorSimple zařízení](storsimple-remote-connect.md)|
|Konfigurace nastavení oznámení|StorSimple Správce služby → zařízení → konfigurace|[Zobrazení a správa StorSimple upozornění](storsimple-manage-alerts.md)|
|Konfigurace CHAP pro zařízení s StorSimple|StorSimple Správce služby → zařízení → konfigurace|[Konfigurace CHAP pro zařízení s StorSimple](storsimple-configure-chap.md)|
|Přidejte zásady zálohování</br>Přidat nebo změnit plán</br>Odstranění zásady zálohování</br>Provést ruční zálohování</br>Vytvoření vlastní zásady zálohování s více svazky a plány|StorSimple Správce služby → zařízení → zálohování zásady|[Správa zásad zálohování](storsimple-manage-backup-policies.md)|
|Ukončení řadiče zařízení</br>Restartujte zařízení řadiče</br>Vypněte řadiče zařízení</br>Obnovit výchozí nastavení zařízení</br>(Výše jsou pouze zařízení místní)|StorSimple Správce služby → zařízení → údržby|[Správa řadiče domény StorSimple zařízení](storsimple-manage-device-controller.md)|
|Další informace o StorSimple hardwarové součásti</br>Sledování stavu hardwaru</br>(Výše jsou pouze zařízení místní)|StorSimple Správce služby → zařízení → údržby|[Sledování hardwarové součásti](storsimple-monitor-hardware-status.md)|
|Vytvoření balíčku podpory|StorSimple Správce služby → zařízení → údržby|[Vytváření a Správa balíčku podpory](storsimple-create-manage-support-package.md)|
|Instalace aktualizací|StorSimple Správce služby → zařízení → údržby|[Aktualizovat zařízení](storsimple-update-device.md)|


##<a name="next-steps"></a>Další kroky
Pokud máte problémy s operaci každodenní StorSimple zařízení nebo pomocí kterékoli z jeho hardwarové součásti, označovat:

- [Poradce při potížích s provozní zařízení](storsimple-troubleshoot-operational-device.md)
- [Použití StorSimple sledování indikátor LED](storsimple-monitoring-indicators.md)

Pokud nemůžete vyřešit problémy a chcete vytvořit žádost o službu, v nápovědě k [Kontaktovat podporu Microsoftu](storsimple-contact-microsoft-support.md).
