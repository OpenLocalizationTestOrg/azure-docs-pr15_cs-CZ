<properties 
   pageTitle="Nahrazení battery na zařízení StorSimple | Microsoft Azure"
   description="Popisuje, jak odebrat, nahrazení a udržovat modulu záložní battery na zařízení s StorSimple."
   services="storsimple"
   documentationCenter=""
   authors="alkohli"
   manager="carmonm"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="TBD"
   ms.date="08/17/2016"
   ms.author="alkohli" />

# <a name="replace-the-backup-battery-module-on-your-storsimple-device"></a>Nahrazení modulu záložní battery na zařízení s StorSimple

## <a name="overview"></a>Základní informace

Primární přílohu Power a chlazení modul (PCM) na vašem zařízení Microsoft Azure StorSimple obsahuje sady pro další battery. Tato sada obsahuje power tak, aby zařízení StorSimple data můžete uložit při ztrátě napájení na primární přílohu. Tento battery pack nazývá *záložní battery modul*. Modul záložní battery existuje pouze pro primární přílohu v zařízení StorSimple (pláště EBOD neobsahuje modulu záložní battery). 

Tento kurz vysvětluje postup:

- Odebrání modulu záložní battery 
- Instalace modulu nové záložní battery
- Udržovat modulu záložní battery

>[AZURE.IMPORTANT] Před odeberte nebo nahraďte modulu záložní battery, přečtěte si informace zabezpečení v článku [Úvod k StorSimple hardwaru součást náhradní](storsimple-hardware-component-replacement.md).

## <a name="remove-the-backup-battery-module"></a>Odebrání modulu záložní battery

Modul záložní battery pro zařízení s StorSimple je pole výměnné jednotka. Před hned po instalaci používat v PCM, mají být modulu battery uložené v původním balení. Proveďte následující kroky odebrat záložní battery.

#### <a name="to-remove-the-backup-battery-module"></a>Chcete-li odebrat modulu záložní battery

1. Na portálu Azure klasické, přejděte na **zařízení** > **údržbu** > **Stav hardwaru**. V seznamu **Sdílené součásti**podívejte se na stav battery.

2. Zjistěte, ve kterém battery selže PCM. Obrázek 1 zobrazuje zpět StorSimple zařízení.

    ![Základní modulů primární přílohu zařízení](./media/storsimple-battery-replacement/IC740994.png)

    **Obrázek 1** Zpátky s primární zařízení zobrazující moduly PCM a řadiče domény

  	|Popisek|Popis|
  	|:----|:----------|
  	|1|PCM 0|
  	|2|PCM 1|
  	|3|Řadiče 0|
  	|4|Řadiči 1|

    Jak je uvedeno číslo 3 na obrázku 2, svítit indikátor sledování Indikátor na PCM 0, který odpovídá **Battery poruch** .

    ![Základní LED indikátor sledování PCM zařízení](./media/storsimple-battery-replacement/IC740992.png)

    **Obrázek 2** Zpátky s PCM zobrazující indikátor sledování LED

  	|Popisek|Popis|
  	|:---|:-----------|
  	|1|AC dodávky elektrického proudu|
  	|2|Chyba při ventilace|
  	|3|Battery poruch|
  	|4|PCM OK|
  	|5|Dodávky elektrického proudu Datacentrum|
  	|6|Battery správný|

3. Odebrat PCM s selhalo battery, postupujte podle kroků v [Odebrat PCM](storsimple-power-cooling-module-replacement.md#remove-a-pcm).

4. S PCM odebrány zvedněte úchyt pro otočení battery modul nahoru vyznačené na následujícím obrázku a přetáhněte je do odebrat battery.

    ![Odebrání Battery PCM](./media/storsimple-battery-replacement/IC741019.png)

    **Obrázek 3** Odebrání battery PCM

5. Umístěte modulu do pole výměnné jednotky balení.

6. Vrácení vadný jednotky společnosti Microsoft pro správné obsluze a zpracování.

## <a name="install-a-new-backup-battery-module"></a>Instalace modulu nové záložní battery

Proveďte následující kroky k instalaci modulu battery náhradní v PCM v primární přílohu StorSimple zařízení.

#### <a name="to-install-the-battery-module"></a>Instalace modulu battery

1. Místo modulu záložní battery ve správné orientaci v PCM.

2. Stisknutím klávesy dolů úchyt modul battery úplně místo spojnice.

3. Podle pokynů uvedených v [Nahradit chlazení modulu na zařízení s StorSimple a Power](storsimple-power-cooling-module-replacement.md)nahraďte PCM v primární přílohu.

4. Po dokončení nahradit přejděte na **zařízení** > **údržbu** > **Stav hardwaru** Azure klasické portálu. Zkontrolujte stav battery, abyste měli jistotu, že instalace byl úspěšný. Zelený stav značí, že je battery správný.

## <a name="maintain-the-backup-battery-module"></a>Udržovat modulu záložní battery

V zařízení StorSimple modul záložní battery obsahuje power řadiči během událost nastavit jako power ztráty. Je možné zařízení StorSimple uložte důležitá data před vypnutí řízené způsobem. S dvěma plně účtovaných bateriemi v PCMs systém zpracovat dvě po sobě jdoucí ztráty události.

Na portálu Azure klasické **Stav hardwaru** na stránce **správy** označuje, zda battery nepracuje nebo se blíží konec životností. Stav battery je označen **Battery v PCM 0** nebo **Battery v PCM 1** v seznamu **Sdílené součásti**. Tato stránka se zobrazí stav **SNÍŽENÝ** pro blíží konec životnost a dosažení **VADNÝ** pro ukončení životnost. 

>[AZURE.NOTE] Battery nahlásit **VADNÝ** ji jednoduše potřebujete-li platit.
 
Pokud se zobrazí stav **SNÍŽENÝ** , doporučujeme kurzu následující akce:

- Systém pravděpodobně došlo k poslední ztrátě power nebo baterie může být prováděna pravidelných Údržba. Sledujte systému 12 hodin, než budete pokračovat.

    - Pokud stav stále **SNÍŽENÝ** po 12 hodin nepřetržité připojení k elektrické s řadiče a PCMs spuštění, battery musí nahradit. Ujistěte se, [Kontaktujte podporu od Microsoftu](storsimple-contact-microsoft-support.md) pro modulu záložní battery náhradní.

    - Pokud stav změní OK po 12 hodin, battery je funkční a stačí náklady údržbu.

- Pokud nebyla přidružené ztráty napájení, PCM je zapnutý a připojení k elektrické battery je třeba nahradit. [Kontaktování podpory společnosti Microsoft](storsimple-contact-microsoft-support.md) k objednejte modulu záložní battery náhradní.

>[AZURE.IMPORTANT] Mít selhalo battery podle předpisů pro vnitrostátní a místní. 

## <a name="next-steps"></a>Další kroky

Další informace o [StorSimple hardwaru součást náhradní](storsimple-hardware-component-replacement.md).
