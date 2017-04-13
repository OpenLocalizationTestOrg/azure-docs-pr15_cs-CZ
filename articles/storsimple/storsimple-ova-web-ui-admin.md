<properties 
   pageTitle="Virtuální pole StorSimple web správy uživatelského rozhraní | Microsoft Azure"
   description="Popisuje, jak provádět základní zařízení úlohy správy prostřednictvím virtuální pole StorSimple webu uživatelského rozhraní."
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
   ms.date="04/07/2016"
   ms.author="alkohli" />

# <a name="use-the-web-ui-to-administer-your-storsimple-virtual-array"></a>Umožňuje spravovat své StorSimple virtuální pole uživatelské rozhraní webu

![tok procesu instalace](./media/storsimple-ova-web-ui-admin/manage4.png)

## <a name="overview"></a>Základní informace

Kurzy v tomto článku vyrovnat Microsoft Azure StorSimple virtuální pole (označovaná taky jako StorSimple místní virtuální zařízení) spuštěna verze 2016 březen všeobecně dostupná (GA). Tento článek popisuje některé složité pracovní postupy a úkoly správy, které lze provádět na pole virtuální StorSimple. Spravujete StorSimple virtuální matice pomocí StorSimple Správce služby UI (jako takzvaný portálu uživatelského rozhraní) a místní web uživatelského rozhraní pro zařízení. Tento článek se zaměřuje na úkoly, které lze provádět webového uživatelského rozhraní.

Tento článek obsahuje následující kurzy:

- Získání šifrovací klíč datové služby
- Poradce při potížích nastavením uživatelské rozhraní webu
- Vytvoření balíčku protokolu
- Vypnutí nebo restartujte zařízení

## <a name="get-the-service-data-encryption-key"></a>Získání šifrovací klíč datové služby

Služba dat šifrovací klíč dojde zaregistrovat zařízení první ke službě StorSimple správce. Tento klíč bude nutné s klávesou registrace služby registraci další zařízení se službou StorSimple správce.

Pokud máte chybně umístěné služby dat šifrovacího klíče a potřebujete ho vyvolat, proveďte následující kroky v místní web uživatelského rozhraní zařízení registrované s vašimi službami.

#### <a name="to-get-the-service-data-encryption-key"></a>Chcete-li získat šifrovací klíč datové služby

1. Připojení k místní web uživatelského rozhraní. Přejděte na **Konfigurace** > **cloudu nastavení**.
  

2. V dolní části stránky klikněte na **získat služby dat šifrovacího klíče**. Klávesy se zobrazí. Zkopírujte a uložte tento klíč.
    
    ![získání služby dat šifrovací klíč 1](./media/storsimple-ova-web-ui-admin/image27.png)
   


## <a name="troubleshoot-web-ui-setup-errors"></a>Poradce při potížích nastavením uživatelské rozhraní webu

V některých případech při konfiguraci zařízení pomocí místní web uživatelského rozhraní, můžete narazit na chyby. Diagnostika a řešení těchto chyb, můžete spustit testů diagnostických nástrojů.

#### <a name="to-run-the-diagnostic-tests"></a>Ke spuštění diagnostických testů

1. V místní web uživatelského rozhraní, přejděte na **Poradce při potížích** > **diagnostických testů**.

    ![Diagnostika 1](./media/storsimple-ova-web-ui-admin/image29.png)

2. V dolní části stránky klikněte na **Spustit diagnostických testů**. To bude zahájení testů Diagnostika možné problémy s vaší síti, zařízení, proxy serveru webové čas nebo nastavení cloudu. Zobrazí se oznámení, že zařízení používá testů.

3. Pokud zkoušku dokončili, zobrazí se výsledky. Následující příklad zobrazuje výsledky diagnostických testů. Poznámka: nastavení proxy serveru webové nebyly nakonfigurovaný na tomto zařízení, a proto nebyl spusťte test proxy serveru webové. Všechny ostatní testy pro nastavení sítě, DNS server a nastavení časového byly úspěšně.

    ![Diagnostika 2](./media/storsimple-ova-web-ui-admin/image30.png)

## <a name="generate-a-log-package"></a>Vytvoření balíčku protokolu

Balíček protokolu se skládá z příslušné protokolů, které vám mohou pomoci Microsoft Support s odstraňování všech problémů, zařízení. V této verzi lze vytvořit balíček protokolu přes místní web uživatelského rozhraní.

#### <a name="to-generate-the-log-package"></a>Generování balíček protokolu

1. V místní web uživatelského rozhraní, přejděte na **Poradce při potížích** > **Protokoly systému**.

    ![Vytvoření balíčku protokolu 1](./media/storsimple-ova-web-ui-admin/image31.png)

2. V dolní části stránky klikněte na **vytvořit balíček pro protokolu**. Bude vytvořen balíček protokoly systému. Bude trvat několik minut.

    ![Vytvoření balíčku protokolu 2](./media/storsimple-ova-web-ui-admin/image32.png)

    Zobrazí se oznámení po úspěšném vytvoření balíčku a na stránce se aktualizují označíte, čas a datum vytvoření balíčku.

    ![Vytvoření balíčku protokolu 3](./media/storsimple-ova-web-ui-admin/image33.png)

3. Klikněte na tlačítko **Stáhnout protokolu**. Balíčku ZIP bude stáhnout do počítače.

    ![Vytvoření balíčku protokolu 4](./media/storsimple-ova-web-ui-admin/image34.png)

4. Můžete rozbalit balíček stažený protokolu a zobrazit protokoly systému.

## <a name="shut-down-and-restart-your-device"></a>Ukončete a restartujte zařízení

Můžete vypnout nebo restartujte virtuální zařízení pomocí místní web uživatelského rozhraní. Jsme doporučujeme, než je znovu spustit, udělejte si svazky nebo sdílené složky v režimu offline v hostiteli a potom zařízení. Tím se minimalizuje možné poškození dat. 

#### <a name="to-shut-down-your-virtual-device"></a>Vypnutí virtuální zařízení

1. V místní web uživatelského rozhraní, přejděte na **údržbu** > **Power nastavení**.

2. V dolní části stránky klepněte na možnost **vypnout**.

    ![vypnutí zařízení 1](./media/storsimple-ova-web-ui-admin/image36.png)

3. Zobrazí se upozornění, informacemi o tom, že vypnutí zařízení přerušíte všechny vstupu a výstupu, které byly v průběhu, výsledkem je výpadek služeb. Klikněte na ikonu zaškrtnutí ![Zaškrtněte ikony](./media/storsimple-ova-web-ui-admin/image3.png).

    ![vypnutí upozornění zařízení](./media/storsimple-ova-web-ui-admin/image37.png)

    Zobrazí se oznámení, že byla spuštěná vypnutí.

    ![vypnutí zařízení Začínáme](./media/storsimple-ova-web-ui-admin/image38.png)

    Teď se vypne zařízení. Pokud chcete začít zařízení, musíte to udělat pomocí správce pro Hyper-V.

#### <a name="to-restart-your-virtual-device"></a>Restartujte zařízení virtuální

1. V místní web uživatelského rozhraní, přejděte na **údržbu** > **Power nastavení**.

2. V dolní části stránky klikněte na **Restartovat**.

    ![restartování zařízení](./media/storsimple-ova-web-ui-admin/image36.png)

3. Zobrazí se upozornění, informacemi o tom, že restartování zařízení přerušíte všechny IOs, které byly v průběhu, výsledkem je výpadek služeb. Klikněte na ikonu zaškrtnutí ![Zaškrtněte ikony](./media/storsimple-ova-web-ui-admin/image3.png).

    ![Restartujte upozornění](./media/storsimple-ova-web-ui-admin/image37.png)

    Zobrazí se oznámení, že byla spuštěná restartování.

    ![restartování počítače, které iniciuje](./media/storsimple-ova-web-ui-admin/image39.png)

    Restartování v průběhu, dojde ke ztrátě připojení k uživatelského rozhraní. Restartování můžete sledovat pomocí pravidelně aktualizace uživatelského rozhraní. Můžete taky můžete sledovat stav restart zařízení pomocí správce pro Hyper-V.

## <a name="next-steps"></a>Další kroky

Zjistěte, jak [používat službu StorSimple správce pro správu zařízení](storsimple-manager-service-administration.md).
