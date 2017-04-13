<properties 
   pageTitle="Změna režimu zařízení StorSimple | Microsoft Azure"
   description="Popisuje režimy zařízení StorSimple a vysvětluje, jak používat Windows PowerShell pro StorSimple Změna režimu zařízení."
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
   ms.date="06/17/2016"
   ms.author="alkohli" />

# <a name="change-the-device-mode-on-your-storsimple-device"></a>Změna režimu zařízení na zařízení s StorSimple

Tento článek obsahuje stručný popis různých režimy, ve kterých můžete pracovat StorSimple zařízení. Zařízení s StorSimple fungovat v třemi režimy: Normální, údržbu a obnovení. 

Po přečtení v tomto článku se dozvíte:

- K čemu režimy StorSimple zařízení jsou
- Jak zjistit, jaký režim zařízení StorSimple probíhá
- Jak lze změnit na normální režim údržby a *naopak*


Výše uvedené úlohy správy lze provést pouze prostřednictvím rozhraní prostředí Windows PowerShell StorSimple zařízení.

## <a name="about-storsimple-device-modes"></a>Režimy StorSimple zařízení

Zařízení s StorSimple můžete pracovat v režimu Normální, údržbu nebo obnovení. Každá z těchto režimů stručně popsána níže.

### <a name="normal-mode"></a>Normální režim

Je definována jako normální provozní režim pro plně nakonfigurovaném StorSimple zařízení. Ve výchozím nastavení vašeho zařízení třeba do normálního režimu.

### <a name="maintenance-mode"></a>Režim údržby

Zařízení StorSimple někdy může být nutné umístit do režimu údržby. Tohoto režimu umožňuje provádět údržba zařízení a aktualizace vyloučit, třeba můžou být související s diskem firmware.

Systém můžete vložit do režimu údržby jenom přes Windows PowerShell pro StorSimple. Pozastaví se všechny požadavky v/v v tomto režimu. Služeb, jako je paměti RAM stálé (NVRAM) nebo službu clusterů taky ukončit. Když zadáte nebo ukončit tento režim restartování obou řadiče. Po ukončení režimu údržby všechny služby bude pokračovat a by měl být správný. To může trvat několik minut.

>[AZURE.NOTE]Režim údržby **je podporována pouze na zařízení správně funguje. Není podporováno v zařízení, ve kterém jednu nebo obě řadiče nefungují.**
</br>

### <a name="recovery-mode"></a>Obnovení režimu

Obnovení režimu lze popsat jako "Nouzový režim pro Windows s podporou sítě". Obnovení režim věnuje Microsoft Support týmu a umožňuje provádět Diagnostika systému. Primární cíle režimu obnovení je získat protokoly systému.

Pokud systému přejde do režimu obnovení, kontaktujte Microsoft Support k dalším krokům. Další informace přejděte na [Kontaktovat podporu Microsoftu](storsimple-contact-microsoft-support.md).

>[AZURE.NOTE] **V režimu obnovení nelze umístit zařízení. Pokud zařízení je v nevyhovujícím stavu, režimu obnovení pokusí se získat zařízení do stavu, ve kterém můžete zkoumat Microsoft Support pracovníci ho.**

## <a name="determine-storsimple-device-mode"></a>Určit StorSimple zařízení režim

#### <a name="to-determine-the-current-device-mode"></a>Chcete-li zjistit aktuální režim zařízení

1. Přihlaste se ke konzole sériové zařízení pomocí následujících kroků v tématu [Použití nátěrové připojení konzoly sériové zařízení](storsimple-deployment-walkthrough.md#use-putty-to-connect-to-the-device-serial-console).

2. Podívejte se na nápis zprávu v nabídce sériové konzola zařízení. Tato zpráva explicitně označuje, zda je zařízení v režimu údržby nebo obnovení. Pokud se zpráva neobsahuje žádné konkrétní informace týkající se režim systému, zařízení se normálního režimu.

## <a name="change-the-storsimple-device-mode"></a>Změna způsobu StorSimple zařízení 

Zařízení StorSimple můžete umístit do režimu údržby (z normálního režimu) údržbě nebo instalace aktualizací režimu údržbu. Postupujte podle následujících pokynů a zadejte ukončit režim na údržbu.

> [AZURE.IMPORTANT] Před zadáním režimu údržby, ověřte, jestli jsou oba řadiče zařízení správný přístupem **Stav hardwaru** na stránce **Údržba** Azure klasické portálu. Pokud jeden nebo oba řadiče není správný, obraťte se na Microsoft Support k dalším krokům. Další informace přejděte na [Kontaktovat podporu Microsoftu](storsimple-contact-microsoft-support.md).

#### <a name="to-enter-maintenance-mode"></a>Do režimu údržby

1. Přihlaste se ke konzole sériové zařízení pomocí následujících kroků v tématu [Použití nátěrové připojení konzoly sériové zařízení](storsimple-deployment-walkthrough.md#use-putty-to-connect-to-the-device-serial-console).

2. V nabídce sériové konzola možnost 1, **Přihlaste se pomocí plný přístup**. Po zobrazení výzvy zadejte **heslo správce zařízení**. Je výchozí heslo: `Password1`.

3. Na příkazovém řádku zadejte 

    `Enter-HcsMaintenanceMode`

4. Zobrazí se zpráva s upozorněním, že bude režimu údržby přerušit všechny požadavky v/v a server připojení k portálu Azure klasické a zobrazí se výzva k potvrzení. Typ **Y** do režimu údržbu.

5. Obě řadiče restartuje. Po dokončení restartování zobrazí další zprávy označující, že zařízení je v režimu údržby.


#### <a name="to-exit-maintenance-mode"></a>Chcete-li ukončit režim na údržbu

1. Přihlaste se ke konzole sériové zařízení. Zkontrolujte z nápis zprávu, která je vaše zařízení v režimu údržby.

2. Na příkazovém řádku zadejte:

    `Exit-HcsMaintenanceMode`

3. Zobrazí se zpráva s upozorněním a zprávu s potvrzením. Typ **Y** ukončíte režim údržby.

4. Obě řadiče restartuje. Po dokončení restartování zobrazí další zprávy označující, že zařízení je do normálního režimu.


## <a name="next-steps"></a>Další kroky

Zjistěte, jak [používat normální a údržba aktualizace režimu](storsimple-update-device.md) na zařízení s StorSimple.

