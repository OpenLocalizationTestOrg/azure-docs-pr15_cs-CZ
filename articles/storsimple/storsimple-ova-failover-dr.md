<properties
   pageTitle="Obnovení a zařízení převzetí havárie pro své StorSimple virtuální pole"
   description="Další informace o tom, jak převzetí své StorSimple virtuální pole."
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

# <a name="disaster-recovery-and-device-failover-for-your-storsimple-virtual-array"></a>Obnovení a zařízení převzetí havárie pro své StorSimple virtuální pole


## <a name="overview"></a>Základní informace

Tento článek popisuje obnovení pro vašeho webu virtuální pole pro Microsoft Azure StorSimple (označovaná taky jako StorSimple místní virtuální zařízení) včetně podrobný postup požadované převzetí na jiné zařízení virtuální v případě selhání. Přepojení umožňuje přenést data ze *zdroje* zařízení v datacentra na jiné zařízení *cílové* umístěné ve stejném nebo jiném geografické polohy. Převzetí zařízení je určený pro celé zařízení. Při selhání data cloudu pro zařízení zdroj změní vlastnictví, cílového zařízení.

Převzetí zařízení orchestrated prostřednictvím funkce havárie obnovení (DR) a je spuštěná z stránce **zařízení** . Na této stránce podporován všech zařízeních StorSimple připojení ke službě StorSimple správce. U každého zařízení se zobrazují popisný název, stav, zřizování a maximální kapacitu, typ a modelu.

![](./media/storsimple-ova-failover-dr/image15.png)

V tomto článku platí pro virtuální matice StorSimple pouze. Převzetí zařízení 8000 řady, přejděte na [převzetí a obnovení StorSimple zařízení](storsimple-device-failover-disaster-recovery.md).


## <a name="what-is-disaster-recovery"></a>Co je havárie obnovení?

V případě havárie obnovení (DR) primárního zařízení přestane fungovat. V takovém případě můžete přesouvat cloudu data spojená se selhalo zařízení na jiné zařízení použití primární zařízení jako *zdroj* a zadáním jiné zařízení jako *cíl*. Tento proces se nazývá *překlopení*. Při selhání všechny svazky nebo sdílené položky ze zdrojového zařízení změnit vlastnictví a převádějí na cílové zařízení. Bez filtrování dat jsou povolené.

Web DR modelovat jako obnovení úplné zařízení pomocí tepelná založených na mapě vrstvení a sledování. Tepelná mapa je definován přiřazení hodnoty tepelné data v závislosti na čtení a zápis vzorků. Tento tepelné namapujte potom úrovní nejnižší bloky dat tepelné do cloudu nejdřív zachováním bloky vysoké tepelné (nejpoužívanějších) dat v místním osy. Během web DR tepelné mapy slouží k obnovení a rehydrate data z cloudu. Zařízení načte všechny svazky/sdílené položky v poslední posledního zálohování (stanovené interně) a provede obnovení z této zálohy. Celý web DR proces orchestrated zařízením.


## <a name="prerequisites-for-device-failover"></a>Požadavky pro zařízení překlopení


### <a name="prerequisites"></a>Zjistit předpoklady pro

Pro všechny převzetí zařízení by měl být splněny následující požadavky:

- Zdrojového zařízení musí být ve stavu **deaktivovány** .

- Cílové zařízení musí zobrazit jako **aktivní** na portálu Azure klasické. Potřebujete vytvořit virtuální zařízení cílové objemu stejné nebo vyšší. Místní web uživatelského rozhraní, by měl používat ke konfiguraci a úspěšně zaregistrovat virtuální zařízení.

    > [AZURE.IMPORTANT] Není pokusu o konfiguraci registrovaných virtuální zařízení pomocí služby po kliknutí na **Nastavení dokončení zařízení**. Konfigurace bez zařízení se provádí pomocí služby.

- Zdrojové a cílové zařízení musí být stejného typu. Pouze může selhat přes virtuální zařízení nakonfigurování jako souborovém serveru na jiný soubor server. Platí totéž i pro iSCSI server.

- Souborovém serveru DR doporučujeme spojte cílové zařízení stejné domény jako zdroje, aby se automaticky přeložena oprávnění ke sdílení. V této verzi je podporována pouze přepnutí do cílové zařízení ve stejné domény.

### <a name="other-considerations"></a>Další informace

- Doporučujeme provést svazky nebo sdílené složky v offline zařízení zdroje.

- Je-li plánované selhání, doporučujeme provést zálohy zařízení a pak pokračujte záložní minimalizovat ztrátou dat. Je-li neplánované převzetí, nejnovější zálohy se použijí k obnovení zařízení.

- Jsou k dispozici cílové zařízení pro web DR zařízení, které mají stejné nebo větší kapacita ve srovnání s zdrojového zařízení. Zařízení, která jsou připojení ke službě, ale ne kritériím dostatek místa nebudou dostupné jako cílová zařízení.

### <a name="dr-prechecks"></a>Web DR prechecks

Před zahájením DR prechecks provádí v zařízení. Tyto kontroly pomáhají zajistit, že bez chyb se projeví při DR začíná. Prechecks patří:

- Ověření účtu úložiště

- Kontrola cloudu připojení k Azure

- Kontrola volného místa na cílové zařízení

- Kontrola Pokud zařízení zdrojový server iSCSI má platné názvy ACR IQN (nejvýše 220 znaků) a heslo CHAP (12 a 16 znaků) přidružené svazky

Pokud některý z výše uvedených prechecks nezdaří, nemůže pokračovat DR. Potřebujete vyřešit problémy a opakujte DR.

Po úspěšném dokončení DR převodu vlastnictví cloudu údajů o zařízení zdroj cílové zařízení. Zdrojového zařízení potom už není k dispozici na portálu. Přístup k všechny svazky/sdílených zařízení zdroje je blokován a aktivuje cílové zařízení.

> [AZURE.IMPORTANT]
> 
> V případě, že zařízení už není dostupná, virtuální počítač, který zřízení na hostitelském systému stále spotřebovává zdroje. Po úspěšném dokončení DR můžete odstranit tento virtuální počítač ze svého hostitele systému.

## <a name="fail-over-to-a-virtual-array"></a>Převzetí virtuální matici

Doporučujeme, abyste měli jiného pole StorSimple virtuální zřízení nakonfigurované přes místní web uživatelského rozhraní a registrovaný u službu StorSimple správce před spuštěním tohoto postupu.


> [AZURE.IMPORTANT]
> 
> - Nemáte oprávnění k předání z řady zařízení StorSimple 8000 1200 virtuální zařízení.
> - Můžete převzít ze virtuální zařízení informace standardní FIPS (Federal Processing) povolené nasazenou v portálu pro státní správu virtuální zařízení Azure klasické portálu. Platí také PRAVDA.

Proveďte následující kroky obnovíte cílové virtuální zařízení StorSimple zařízení.

1. V hostiteli prohlédněte svazky/sdílené složky v offline režimu. Postupujte podle pokynů specifické pro operační systém na hostiteli umožní svazky sdílené složky v offline režimu. Pokud už offline, budete muset přijmout všechny svazky/akcie offline na zařízení tak, že přejdete do **zařízení > akcie** (nebo **zařízení > svazky**). Vyberte sdílet/svazku a klikněte na možnost **převést do offline** v dolní části stránky. Po zobrazení výzvy k potvrzení klikněte na **Ano**. Tenhle postup opakujte pro všechny sdílené složky nebo svazky na zařízení.

2. Na stránce **zařízení** vyberte zdrojového zařízení převzetí a klikněte na tlačítko **deaktivovat**. 
    ![](./media/storsimple-ova-failover-dr/image16.png)

3. Zobrazí se výzva k potvrzení. Deaktivace zařízení je trvalý proces, který nelze je vrátit zpět. Také být upozorněni na požadovanou akcie/svazky offline v hostiteli.

    ![](./media/storsimple-ova-failover-dr/image18.png)

3. Po potvrzení začnou deaktivaci. Po deaktivaci se úspěšně dokončilo, zobrazí se oznámení.

    ![](./media/storsimple-ova-failover-dr/image19.png)

4. Na stránce **zařízení** se stav zařízení teď změní **deaktivovány**.

    ![](./media/storsimple-ova-failover-dr/image20.png)

5. Vyberte deaktivovaný zařízení a v dolní části stránky klikněte na **překlopení**.

6. V okně Potvrdit převzetí průvodce, který se objeví postupujte takto:

    1. V rozevíracím seznamu dostupných zařízení, vyberte **cílové zařízení.** V rozevíracím seznamu jsou zobrazeny pouze zařízení, která mít dostatečnou kapacitu.

    2. Prohlédněte si podrobnosti přidružené k zařízení zdroj, třeba název zařízení, celkovou kapacitu a názvy jejich počet neúspěšných přes.

        ![](./media/storsimple-ova-failover-dr/image21.png)

7. Zaškrtněte políčko **můžu souhlasíte, že převzetí je trvalé a po úspěšném dokončení záložní zdrojového zařízení, se odeberou**.

8. Klikněte na ikonu kontrola ![](./media/storsimple-ova-failover-dr/image1.png).


9. Bude spuštěná úloha převzetí a zobrazí se oznámení. Klikněte na **zobrazení projektu** sledovat záložní.

    ![](./media/storsimple-ova-failover-dr/image22.png)

10. V dialogovém okně **úlohy** uvidíte převzetí úlohy vytvořené pro zařízení zdroje. Tuto úlohu provede DR prechecks.

    ![](./media/storsimple-ova-failover-dr/image23.png)

    Po úspěšném DR prechecks úlohy převzetí spustit úloh obnovení pro každou sdílet/rozlohu na zařízení s zdroje.

    ![](./media/storsimple-ova-failover-dr/image24.png)

11. Po dokončení záložní přejděte na stránku **zařízení** .

    na. Vyberte požadované StorSimple virtuální zařízení byla použita jako cílové zařízení pro překlopení obrázku.

    b. Přejděte na stránku **akcie** (nebo **svazky** Pokud iSCSI server). Teď by měla být vedená všechny sdílené položky (svazky) ze starého zařízení.
    
    ![](./media/storsimple-ova-failover-dr/image25.png)

![](./media/storsimple-ova-failover-dr/video_icon.png)**Video k dispozici**

Toto video ukazuje, jak můžete převzít StorSimple místní virtuální zařízení na jiné virtuální zařízení.

> [AZURE.VIDEO storsimple-virtual-array-disaster-recovery]

## <a name="business-continuity-disaster-recovery-bcdr"></a>Obchodní kontinuitu havárie obnovení (BCDR)

Scénáři firmy kontinuitu havárie obnovení (BCDR) dojde, když celý Azure datacentra přestane fungovat. To může ovlivnit StorSimple Správce služby a související StorSimple zařízení.

Pokud jsou zařízení StorSimple registrovaných těsně před došlo k selhání, tato zařízení StorSimple možná muset odeberou. Po havárie můžete znovu vytvořit a nakonfigurovat těchto zařízení.

## <a name="errors-during-dr"></a>Chyby při DR

**Shluk připojení výpadku během DR**

Pokud po spuštění DR přerušení propojení cloudu před dokončením obnovení zařízení, se nepovede DR a budete upozorněni. Cílové zařízení, která byla použita pro web DR potom označen jako *nelze použít.* Stejné cílové zařízení není možné pak použít pro budoucí DRs.

**Zařízení není kompatibilní cílové**

Máte k dispozici cílová zařízení není dostatek místa, zobrazí se chyba o tom, že nejsou kompatibilní cílové zařízení.

**Precheck k chybám**

Pokud není splněna jedna z prechecks, uvidíte precheck k chybám.

## <a name="next-steps"></a>Další kroky

Další informace o tom, jak [Spravovat své StorSimple virtuální pole pomocí místní web uživatelského rozhraní](storsimple-ova-web-ui-admin.md).
