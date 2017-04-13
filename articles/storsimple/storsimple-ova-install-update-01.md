<properties 
   pageTitle="Instalace aktualizací virtuální matici StorSimple | Microsoft Azure"
   description="Popisuje, jak pomocí webového virtuální pole StorSimple uživatelského rozhraní použití aktualizací metodou portálem a hotfix"
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
   ms.date="09/07/2016"
   ms.author="alkohli" />

# <a name="install-updates-on-your-storsimple-virtual-array"></a>Instalace aktualizací na své StorSimple virtuální pole

## <a name="overview"></a>Základní informace

Tento článek popisuje postup potřebný k instalaci aktualizací vaší StorSimple virtuální matice, přes místní web uživatelského rozhraní a prostřednictvím portálu Azure klasické. Je potřeba používat software aktualizací nebo oprav pravidelná své StorSimple virtuální pole. 

Mějte na paměti, který instalaci aktualizace nebo hotfix restartuje zařízení. Že je argument matice virtuální StorSimple jeden uzel zařízení, přerušení vstupu a všechny výstupu v průběhu a zařízení dojde k výpadek služeb. 

Před instalací aktualizace, doporučujeme, abyste podniknout svazky nebo sdílené složky v offline režimu na hostiteli první a potom na zařízení. To minimalizuje možné poškození dat.

> [AZURE.IMPORTANT] Pokud používáte 0,1 GA software verze nebo, musíte nainstalovat aktualizaci 0,3 použijte metodu hotfix přes místní web uživatelského rozhraní. Pokud používáte aktualizace 0,2, doporučujeme nainstalovat aktualizace prostřednictvím stránek portálu Azure klasické.

## <a name="use-the-local-web-ui"></a>Použití místní web uživatelského rozhraní 
 
Existují dva kroky při používání místní web uživatelského rozhraní:

- Stáhněte si aktualizaci nebo hotfix
- Nainstalujte aktualizaci nebo hotfix

### <a name="download-the-update-or-the-hotfix"></a>Stáhněte si aktualizaci nebo hotfix

Provedení následujících kroků můžete stáhnout aktualizace softwaru z katalogu Microsoft Update.

#### <a name="to-download-the-update-or-the-hotfix"></a>Stáhněte si aktualizaci nebo hotfix

1. Spusťte Internet Explorer a přejděte na [http://catalog.update.microsoft.com](http://catalog.update.microsoft.com).

2. Pokud je to poprvé pomocí katalogu Microsoft Update na tomto počítači, klikněte na **nainstalovat** po zobrazení výzvy k instalaci doplňku Microsoft Update katalogu.
  
3. Do pole Hledat katalog služby Microsoft Update zadejte číslo znalostní bázi Knowledge Base (KB) opravy, které chcete stáhnout. Zadejte **3182061** aktualizace 0,3 a klikněte na **Hledat**.

    Seznam hotfix se zobrazí, například **StorSimple virtuální pole aktualizace 0,3**.

    ![Katalog vyhledávání](./media/storsimple-ova-install-update-01/download1.png)

4. Klikněte na **Přidat**. Aktualizace se přidá do košíku.

5. Klikněte na **Zobrazit košík**.

6. Klikněte na **Stáhnout**. Určení nebo **vyhledejte** místní místo, kam chcete soubory ke stažení zobrazovat. Aktualizace stáhli do zadaného umístění a umístí do podsložky se stejným názvem jako aktualizace. Složku můžete také kopírovat do sdílené síťové složky, která je dostupná ze zařízení.

7. Otevřete složku zkopírovaný, byste měli vidět soubor balíčku samostatného Microsoft Update `WindowsTH-KB3011067-x64`. Tento soubor slouží k instalaci aktualizace nebo hotfix.


### <a name="install-the-update-or-the-hotfix"></a>Nainstalujte aktualizaci nebo hotfix

Před instalací aktualizace nebo hotfix Přesvědčte se, že máte aktualizaci ani nestáhli hotfix buď místně na hostitele nebo dostupné přes síťové složky. 

Použijte tento způsob instalace aktualizací na zařízení s GA nebo aktualizovat verze 0.1 softwaru. Tento postup trvá menší než 2 minut. Proveďte následující kroky a nainstalujte aktualizace nebo hotfix.


#### <a name="to-install-the-update-or-the-hotfix"></a>Chcete-li nainstalovat aktualizace nebo hotfix

1. V místní web uživatelského rozhraní, přejděte na **údržbu** > **Aktualizace softwaru**.

    ![aktualizace zařízení](./media/storsimple-ova-install-update-01/update1m.png)

2. V **aktualizaci cesta k souboru**zadejte název souboru pro aktualizaci nebo hotfix. Můžete taky přejdete instalační soubor aktualizace nebo hotfix Pokud umístit do sdílené síťové složky. Klikněte na tlačítko **použít**.

    ![aktualizace zařízení](./media/storsimple-ova-install-update-01/update2m.png)

3.  Zobrazí se upozornění. Zadaný to je jediný uzel zařízení po aktualizaci použít, restartuje zařízení a je výpadek služeb. Klikněte na ikonu zaškrtnutí.

    ![aktualizace zařízení](./media/storsimple-ova-install-update-01/update3m.png)

4. Spustí se aktualizace. Po úspěšném aktualizaci zařízení restartuje. Místní uživatelské rozhraní není dostupné v tomto doby trvání.

    ![aktualizace zařízení](./media/storsimple-ova-install-update-01/update5m.png)

5. Po dokončení restartování přejdete na stránku **přihlášení** . Pokud chcete ověřit, že software zařízení aktualizoval, v místní web uživatelského rozhraní, přejděte na **údržbu** > **Aktualizace softwaru**. Verze zobrazeného software by měla být **10.0.0.0.0.10288.0** aktualizace 0,3.

    > [AZURE.NOTE] Verze softwaru jsme sestava v mírně odlišnou způsobem místní web uživatelského rozhraní a Azure klasické portálu. Například místní web uživatelského rozhraní sestavy **10.0.0.0.0.10288** portálu Azure klasické sestavách a **10.0.10288.0** pro stejnou verzi. 

    ![aktualizace zařízení](./media/storsimple-ova-install-update-01/update6m.png)





## <a name="use-the-azure-classic-portal"></a>Používat portál Azure klasické

Pokud aktualizace 0,2, doporučujeme nainstalovat aktualizace prostřednictvím portálu Azure klasické. Portál postup vyžaduje uživatelům orientaci, stáhněte a nainstalujte aktualizace. Tento postup trvá asi 7 minut. Proveďte následující kroky a nainstalujte aktualizace nebo hotfix.

[AZURE.INCLUDE [storsimple-ova-install-update-via-portal](../../includes/storsimple-ova-install-update-via-portal.md)]

Po dokončení instalace (jako je určený podle stavu úlohy v měřítku 100 %), přejděte na **zařízení > Údržba > aktualizace softwaru**. Verze zobrazeného software by měla být 10.0.10288.0.

![aktualizace zařízení](./media/storsimple-ova-install-update-01/azupdate12m.png)

## <a name="next-steps"></a>Další kroky

Další informace o [správě své StorSimple virtuální pole](storsimple-ova-web-ui-admin.md).
