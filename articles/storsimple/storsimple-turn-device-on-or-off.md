<properties 
   pageTitle="Zapnutí nebo vypnutí StorSimple zařízení | Microsoft Azure"
   description="Vysvětluje, jak zapnout na nové zařízení StorSimple, zapněte zařízení, který byl vypnout nebo ztratilo power a vypněte pracovního zařízení."
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
   ms.date="08/23/2016"
   ms.author="alkohli" />

# <a name="turn-your-storsimple-device-on-or-off"></a>Zapnutí nebo vypnutí StorSimple zařízení 

## <a name="overview"></a>Základní informace

Vypnutí zařízení Microsoft Azure StorSimple nepožaduje jako součást operace normální systému. Potřebujete však zapnutí na nové zařízení nebo zařízení, které bylo nutné vypnout. Vypnutí obecně, je potřeba v případech, ve kterých musí nahradit selhalo hardwaru, fyzicky přesunete jednotku nebo přenesete zařízení ze služby. Tento kurz určen požadované postup pro zapnutí a vypnutí StorSimple zařízení v různých scénářích.

Následující tabulka uvádí různé scénáře pro zapnutí a vypnutí StorSimple zařízení a obsahuje odkazy na v příslušných postupech.

|Scénář|Odkazy na témata|
|:-------|:---------------|
|Zapnutí na nové zařízení|[Zapnutí na nové zařízení](#turn-on-a-new-device)<ul><li>[Nové zařízení s primární přílohu](#new-device-with-primary-enclosure-only)</li><li>[Nové zařízení s EBOD přílohu](#new-device-with-ebod-enclosure)</li></ul>|
|Zapnutí zařízení po vypnutí|[Zapnutí zařízení po vypnutí](#turn-on-a-device-after-shutdown)<ul><li>[Zařízení s primární přílohu](#device-with-primary-enclosure-only)</li><li>[Zařízení s EBOD přílohu](#device-with-ebod-enclosure)</li></ul>|
|Zapnutí zařízení po ztrátě power|[Zapnutí zařízení po ztrátě power](#turn-on-a-device-after-a-power-loss)<ul><li>[Zařízení s primární přílohu](#8100)</li><li>[Zařízení s EBOD přílohu](#8600)</li></ul>|
|Zapnutí zařízení po primární přílohu a dojde ke ztrátě EBOD připojení|[Zapnutí zařízení po primární a dojde ke ztrátě připojení EBOD přílohu](#turn-on-a-device-after-the-primary-and-ebod-enclosure-connection-is-lost)|
|Ukončení pracovního zařízení|[Vypnutí pracovního zařízení](#turn-off-a-running-device)<ul><li>[Zařízení s primární přílohu](#8100a)</li><li>[Zařízení s EBOD přílohu](#8600a)</li></ul>|

## <a name="turn-on-a-new-device"></a>Zapnutí na nové zařízení

Postup zapnutí zařízení StorSimple poprvé se může lišit podle toho, zda je zařízení 8100 nebo datového modelu pro 8600. 8100 obsahuje jeden primární přílohu, že 8600 se dvěma přílohu zařízení s primární přílohu a přílohu EBOD. Podrobný postup pro oba modely jsou zahrnuty následující části.

- [Nové zařízení s primární přílohu](#new-device-with-primary-enclosure-only)

- [Nové zařízení s EBOD přílohu](#new-device-with-ebod-enclosure)

### <a name="new-device-with-primary-enclosure-only"></a>Nové zařízení s primární přílohu

Model StorSimple 8100 je zařízení jednu přílohu. Zařízení s i nadbytečné Power chlazení moduly (PCMs). Jak PCMs musí být nainstalovaná a připojení ke zdrojům různých power zajistit dostupnost.

Proveďte následující kroky k kabel zařízení power.

[AZURE.INCLUDE [storsimple-cable-8100-for-power](../../includes/storsimple-cable-8100-for-power.md)]

>[AZURE.NOTE]Nastavení dokončení zařízení a kabeláž pokyny najdete [instalace StorSimple 8100 zařízení](storsimple-8100-hardware-installation.md). Ujistěte se, přesně postupujte podle pokynů.

### <a name="new-device-with-ebod-enclosure"></a>Nové zařízení s EBOD přílohu

Model StorSimple 8600 má primární přílohu a přílohu EBOD. Při této akci musí jednotky, které mají být společně kabelové připojení sériové připojené SCSI (přidružení zabezpečení) a power.

Při nastavování toto zařízení poprvé, proveďte kroky pro přidružení zabezpečení kabeláž nejdřív a pak postupujte podle pokynů pro power kabeláž.

[AZURE.INCLUDE [storsimple-sas-cable-8600](../../includes/storsimple-sas-cable-8600.md)]

[AZURE.INCLUDE [storsimple-cable-8600-for-power](../../includes/storsimple-cable-8600-for-power.md)]

>[AZURE.NOTE]Nastavení dokončení zařízení a kabeláž pokyny najdete [instalace StorSimple 8600 zařízení](storsimple-8600-hardware-installation.md). Ujistěte se, přesně postupujte podle pokynů.

## <a name="turn-on-a-device-after-shutdown"></a>Zapnutí zařízení po vypnutí

Kroky pro zapnutí zařízení StorSimple po byl vypnout se můžou lišit podle toho, zda je zařízení 8100 nebo datového modelu pro 8600. 8100 obsahuje jeden primární přílohu, že 8600 se dvěma přílohu zařízení s primární přílohu a přílohu EBOD.

- [Zařízení s primární přílohu](#device-with-primary-enclosure-only)

- [Zařízení s EBOD přílohu](#device-with-ebod-enclosure)

### <a name="device-with-primary-enclosure-only"></a>Zařízení s primární přílohu

Po ukončení pomocí následujícího postupu můžete zapnout StorSimple zařízení s primární přílohu a bez EBOD přílohu.

#### <a name="to-turn-on-a-device-with-a-primary-enclosure-only"></a>Pokud chcete vypnout zařízení s jenom primární přílohu

1. Ujistěte se, že power kombinace kláves vymění na obou Power a chlazení moduly (PCMs) jsou do polohy vypnuto. Není-li přepínače do polohy vypnuto, překlopit do polohy Vypnuto a počkejte indikátory přejdete.

2. Zapněte zařízení tak, že překlopení power přepínače na obou PCMs do polohy zapnuto. Zařízení by měl zapnout.

3. Zkontrolujte podle následujících pokynů zkontrolujte, že zařízení plně na:

    1. OK LED na modulů PCM jsou zelené.

    2. Stav LED v obou řadiče jsou plné zelená.

    3. Modrá LED k některé z řadiče bliká, což znamená, že zařízení je aktivní.

    Pokud není některá z těchto podmínek splněná, pak vaše zařízení není v pořádku. Kontaktujte [Podporu společnosti Microsoft](storsimple-contact-microsoft-support.md).

### <a name="device-with-ebod-enclosure"></a>Zařízení s EBOD přílohu

Po ukončení pomocí následujícího postupu můžete zapnout StorSimple zařízení s primární přílohu a přílohu EBOD. Provedení všech kroků v posloupnosti přesně tak, jak je popsáno. Chyba při k tomu nevyzve může způsobit ztrátu dat.

#### <a name="to-turn-on-a-device-with-a-primary-and-an-ebod-enclosure"></a>Pokud chcete vypnout zařízení s primární a přílohu EBOD

1. Zkontrolujte, jestli přílohu EBOD je připojený k primární přílohu. Další informace najdete v tématu [instalace StorSimple 8600 zařízení](storsimple-8600-hardware-installation.md).

2. Přesvědčte se, že Power a chlazení moduly (PCMs) na EBOD a primární přílohy do polohy vypnuto. Není-li přepínače do polohy vypnuto, překlopit do polohy Vypnuto a počkejte indikátory přejdete.

3. Zapněte přílohu EBOD první překlopení power přepínače na obou PCMs do polohy zapnuto. LED PCM by měl být zelená. Zelené EBOD řadiči LED na tomto znamená, že na přílohu EBOD.

4. Zapněte primární přílohu překlopení power přepínače na obou PCMs do polohy zapnuto. Celý systém by se měla na.

5. Ověřte, zda LED přidružení zabezpečení zeleně, která zajišťuje, aby připojení mezi EBOD přílohu a primární přílohu dobrý.

## <a name="turn-on-a-device-after-a-power-loss"></a>Zapnutí zařízení po ztrátě power

Přerušení a výpadku můžete vypnout StorSimple zařízení. Výpadku může dojít na nějaká dodání power nebo obou napájecí. Obnovení kroky se můžou lišit podle toho, zda je 8100 nebo 8600 modelu zařízení. 8100 obsahuje jeden primární přílohu, že 8600 se dvěma přílohu zařízení s primární přílohu a přílohu EBOD. Tato část popisuje vymáhání pro jednotlivé scénáře.

- [Zařízení s primární přílohu](#8100)

- [Zařízení s EBOD přílohu](#8600)

### <a name="device-with-primary-enclosure-only-a-name8100"></a>Zařízení s primární přílohu<a name="8100">

Systém můžete dál normálním provozu při ztrátě power do jedné ze svých napájecí. Však zajistit dostupnost zařízení obnovte power napájení co nejdříve.

Pokud je výpadku nebo výpadku na obou napájení, systém ukončí uspořádaný a řízené způsobem. Po obnovení power systému se automaticky změní.

### <a name="device-with-ebod-enclosure-a-name8600"></a>Zařízení s EBOD přílohu<a name="8600">

#### <a name="power-loss-on-one-power-supply"></a>Zadat Power ztráty jeden power

Při ztrátě power do jedné ze svých napájecí na primární přílohu nebo přílohu EBOD systému normálním provozu pokračovat. Však zajistit dostupnost zařízení prosím obnovte si power napájení co nejdříve.

#### <a name="power-loss-on-both-power-supplies-on-primary-and-ebod-enclosures"></a>Ztráta Power z obou napájecí na primární a EBOD přílohy

Pokud je výpadku nebo výpadku na obou napájení, plášť EBOD ukončí okamžitě a primární přílohu ukončí uspořádaný a řízené způsobem. Po obnovení power zařízení se spustí automaticky.

Power vypnut ručně, pak pomocí následujících kroků obnovíte power systému.

1. Zapnutí EBOD přílohu.

2. Po přílohu EBOD zapnuté, zapněte primární přílohu.

### <a name="power-loss-on-both-power-supplies-on-ebod-enclosure"></a>Ztráta Power z obou napájecí na EBOD přílohu

Když nastavíte kabely, musíte se ujistit, že EBOD nikdy propojen samostatně samostatné PDU. Pokud EBOD a primární přílohu ve stejnou dobu, jestli chcete systém obnoví.

Pokud pouze přílohu EBOD nepovede na obou napájení, nebude automaticky obnovit systému. Pomocí následujících kroků zapnout systému a obnovení správný stavu:

1. Pokud je zapnutá primární přílohu, vypněte Power a chlazení moduly (PCMs).

2. Počkejte několik minut systému vypnout.

3. Zapnutí EBOD přílohu.

4. Po přílohu EBOD zapnuté, zapněte primární přílohu.

## <a name="turn-on-a-device-after-the-primary-and-ebod-enclosure-connection-is-lost"></a>Zapnutí zařízení po primární a dojde ke ztrátě připojení EBOD přílohu

Pokud mezi úsporném řadiče a odpovídající řadiče EBOD ztráty spojení se zařízení nadále pracovat. Pokud dojde ke ztrátě připojení mezi aktivní správce systému a odpovídající řadiče EBOD, by měla proběhnout převzetí a zařízení by měl pokračovat v práci jako obvykle.

Když oba kabely sériové připojené SCSI (přidružení zabezpečení) jsou odebrány nebo rozděleny připojení mezi EBOD přílohu a primární přílohu, zařízení přestanou fungovat. V tomto okamžiku proveďte následující kroky.

### <a name="to-turn-on-the-device-after-connection-is-lost"></a>Pokud chcete vypnout zařízení po dojde ke ztrátě připojení

1. Přístup na pozadí zařízení.

2. Pokud přidružení zabezpečení kabel propojení EBOD přílohu a primární přílohu se přeruší, přidružení zabezpečení dráhu všechny LED na přílohu EBOD budou vypnout.

3. Vypněte Power a chlazení moduly (PCMs) na přílohu EBOD a primární přílohu.

4. Počkejte, dokud vypnout všechny indikátory na zadní straně jak přílohy.

5. Vložte kabely přidružení zabezpečení a ujistěte se, že je dobré připojení mezi EBOD přílohu a primární přílohu.

6. Zapněte přílohu EBOD první překlopení oba přepínače PCM do polohy zapnuto.

7. Zajistěte, aby byl přílohu EBOD na kontrolou zelené LED je zapnuto.

8. Zapnutí primární přílohu.

9. Zajistěte, aby byl primární přílohu na kontrolou LED zařízení zeleného je zapnuto.

10. Ověřte připojení přílohu EBOD s primární přílohu dobré kontrolou dráhu přidružení zabezpečení LED (čtyři v jednom řadiče domény EBOD) jsou všechny zapnuto.

>[AZURE.IMPORTANT] Pokud jsou kabely přidružení zabezpečení vadný nebo připojení mezi EBOD přílohu a primární přílohu ještě to při zapnutí systému, přejde do režimu obnovení. Kontaktujte [Podporu společnosti Microsoft](storsimple-contact-microsoft-support.md) v takovém případě.

## <a name="turn-off-a-running-device"></a>Vypnutí pracovního zařízení

Průběžný zařízení StorSimple může být nutné vypnout, pokud je přesunutý, z provozu, nebo má nefunkční komponentu, která je třeba nahradit. Kroky se liší podle toho, zda je zařízení StorSimple 8100 nebo datového modelu pro 8600. 8100 obsahuje jeden primární přílohu, že 8600 se dvěma přílohu zařízení s primární přílohu a přílohu EBOD. Tato část obsahuje podrobnosti postup vypnutí pracovního zařízení.

- [Zařízení s primární přílohu](#8100a)

- [Zařízení s EBOD přílohu](#8600a)

### <a name="device-with-primary-enclosure-a-name8100a"></a>Zařízení s primární přílohu<a name="8100a"> 

Vypnutí zařízení uspořádaný a řízené způsobem, můžete to udělat pomocí portálu Azure klasické nebo prostřednictvím prostředí Windows PowerShell pro StorSimple. 

>[AZURE.IMPORTANT] Nelze vypnout pracovního zařízení pomocí tlačítka power na zadní straně zařízení.
>
>Před ukončením zařízení, aby byli všechny komponenty zařízení správný. Na portálu Azure klasické, přejděte na **zařízení** > **údržbu** > **Stav hardwaru**a ověřte, zda je stav všech složek zelené. To platí jenom pro správný systém. Pokud systému je právě vypnout a nahrazení nefunkční komponenty, uvidí selhalo (červená) nebo (žlutá) stav odpovídajících součástí **Stav hardwaru**sníženou kvalitu.

Po přistupujete k prostředí Windows PowerShell pro StorSimple nebo portálu Azure klasické, postupujte podle [vypnout StorSimple zařízení](storsimple-manage-device-controller.md#shut-down-a-storsimple-device). 

### <a name="device-with-ebod-enclosure-a-name8600a"></a>Zařízení s EBOD přílohu<a name="8600a">

>[AZURE.IMPORTANT] Před ukončením primární přílohu a EBOD přílohu, zajistěte, aby byly všechny komponenty zařízení správný. Na portálu Azure klasické, přejděte na **zařízení** > **údržbu** > **Stav hardwaru**a ověřte, jestli jsou všechny komponenty správný.

#### <a name="to-shut-down-a-running-device-with-ebod-enclosure"></a>Ukončení pracovního zařízení s EBOD přílohu

1. Proveďte všechny kroky uvedené v [vypnout StorSimple zařízení](storsimple-manage-device-controller.md#shut-down-a-storsimple-device) pro primární přílohu.

2. Po vypnutí primární přílohu vypněte EBOD tak, že překlopení vypnout přepínače Power a chlazení modul PCM ().

3. Abyste ověřili, že byl vypnout EBOD, zkontrolujte, že všechny indikátory na zadní straně přílohu EBOD vypnout.

>[AZURE.NOTE] Kabely přidružení zabezpečení, které se používá pro připojení k primární přílohu přílohu EBOD neměli odeberou až po vypnutí systému.

## <a name="next-steps"></a>Další kroky

[Kontaktování podpory společnosti Microsoft](storsimple-contact-microsoft-support.md) Pokud narazíte na problémy při zapnutí nebo vypnutí StorSimple zařízení.

