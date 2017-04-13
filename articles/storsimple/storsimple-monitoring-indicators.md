<properties 
    pageTitle="Sledování indikátory StorSimple | Microsoft Azure" 
    description="Popisuje light – vysílání diody (LED) a zvuková upozornění použít ke sledování stavu StorSimple zařízení."
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
    ms.date="08/18/2016"
    ms.author="alkohli" />

# <a name="use-storsimple-monitoring-indicators-to-manage-your-device"></a>Umožňuje spravovat vaše zařízení StorSimple sledování indikátory   

## <a name="overview"></a>Základní informace

Zařízení s StorSimple zahrnuje světla – vysílání diody (LED) a poplašná, který slouží ke sledování moduly a celkový stav zařízení StorSimple. Sledování indikátory se nachází na hardware součástí primárního přílohu mobilního zařízení a EBOD přílohu. Sledování indikátory může být LED nebo zvuková upozornění.

Existují tři stavy LED slouží k indikaci stavu modulu: zelené, blikající zelené červená – žlutý nebo červený oranžová.  

- Zelená LED představují správný stav.  
- Přerušované zelené červená oranžová LED představují stavu nekritické podmínky, které může vyžadovat zásahu uživatele.  
- Červená oranžová LED označuje, že je prezentovat v modulu kritické chyby.  

Zbývající Tento článek popisuje různé sledování indikátor LED, jejich umístění v StorSimple zařízení, se stav zařízení podle států LED a všechny přidružené zvuková upozornění.

## <a name="front-panel-indicator-leds"></a>Indikátor přední LED

Přední panel, nazývaný také *operace panelu* nebo *ops panel*zobrazí agregační stav všech modulů v systému. Přední panel je stejný na primární StorSimple a EBOD přílohu a je zobrazeno níže.  

   ![Přední panel zařízení][1]
 
Přední panel obsahuje především:  

1. Tlačítko Ztlumit
2. Indikátor Power LED (zelená/červená žlutá)
3. Indikátor chyby modul vedené (na červená – oranžová nebo vypnuto)
4. Indikátor logické poruch vedené (na červená – žlutý/vypnuto
5. Zobrazení ID jednotky  

Hlavní rozdíl mezi přední panel LED pro zařízení a pro přílohu EBOD je zobrazené na LED displeji **Systém jednotku identifikačního čísla** . Výchozí jednotky ID zobrazeného na zařízení je **00**, zatímco ID výchozí jednotky zobrazené na přílohu EBOD je **01**. Umožňuje provádět rychle odlišit zařízení a přílohu EBOD při zapnuté zařízení. Pokud vaše zařízení je normálně vypnuté, použijte informace uvedené v [Zapnutí na nové zařízení](storsimple-turn-device-on-or-off.md#turn-on-a-new-device) k odlišení zařízení od EBOD přílohu.  

## <a name="front-panel-led-status"></a>Přední panel Indikátor stavu  

Umožňuje určit stav označen LED na panelu přední pro zařízení nebo EBOD přílohu v následující tabulce.  

|Systém power | Modul poruch | Logické poruch | Upozornění | Stav|
|-------------|---------------|-----------------|-------|-------|
|Červená oranžová | VYPNUTÍ     | VYPNUTÍ | NENÍ K DISPOZICI | Elektrické ztratili, pracující na záložní power nebo elektrické na a na zařízení, které byly odebrány moduly.|
|Zelená | ZAPNUTO | ZAPNUTO | NENÍ K DISPOZICI | Panel OPS je zapnuta (5s s) testování stavu|
|Zelená | VYPNUTÍ | VYPNUTÍ | NENÍ K DISPOZICI | Zapněte všechny funkce dobré|
|Zelená | ZAPNUTO |NENÍ K DISPOZICI | Poruchy PCM LED, ventilace poruch LED | Všechny PCM poruch, ventilace poruch, nad nebo pod teplotu|
| Zelená | ZAPNUTO | NENÍ K DISPOZICI | Modul vstupu a výstupu LED  | Všechny řadiče domény modulu poruch|
| Zelená | ZAPNUTO | NENÍ K DISPOZICI | NENÍ K DISPOZICI | Použití logických operátorů poruch přílohu|
| Zelená | Flash | NENÍ K DISPOZICI | Stav modulu LED na modul řadiče. Poruchy PCM LED, ventilace poruch LED | Typ modul Neznámý řadiče nainstalovali, I2C bus chyby řadiče domény modulu zásadní produktu dat (VPD) konfigurace |

## <a name="power-cooling-module-pcm-indicator-leds"></a>Power chlazení modul (PCM) indikátor LED   

Indikátor modul (PCM) Power chlazení LED najdete na zadní straně primární přílohu nebo přílohu EBOD v jednotlivých modulech PCM. Toto téma popisuje, jak používat následující LED ke sledování stavu StorSimple zařízení.  

- PCM LED pro primární přílohu
- PCM LED pro EBOD přílohu

## <a name="pcm-leds-for-the-primary-enclosure"></a>PCM LED pro primární přílohu  

Zařízení StorSimple obsahuje modul 764W PCM s další battery. Následující obrázek znázorňuje panelu LED zařízení.  

   ![PCM LED na primární přílohu][2]

Indikátor Legenda:

1. AC dodávky elektrického proudu
2. Chyba při ventilace
3. Battery poruch
4. PCM OK
5. Chyba při Datacentrum
6. Battery dobré  

Stav PCM je uveden na panelu Indikátor. Panel PCM LED zařízení má šest LED. Čtyři tyto LED zobrazující stav napájení a ventilátory. Zbývající dva LED indikaci stavu modulu záložní battery v PCM. V následujících tabulkách můžete použít ke zjištění stavu PCM.  

### <a name="pcm-indicator-leds-for-power-supply-and-fan"></a>Indikátor PCM LED napájení a ventilace
| Stav | OK PCM (zelený) | AC selhání (oranžové) | Selhalo: ventilace (oranžové) | Selhalo: Datacentrum (oranžové) |
|--------|----------------|-----------------------|------------------|----------------------|
| Žádné elektrické (Pokud chcete přílohu) | VYPNUTÍ | VYPNUTÍ | VYPNUTÍ | VYPNUTÍ|
| Žádné elektrické (jenom tento PCM) | VYPNUTÍ | ZAPNUTO | VYPNUTÍ | ZAPNUTO |
| AC prezentovat PCM dál - OK     | ZAPNUTO | VYPNUTÍ | VYPNUTÍ | VYPNUTÍ |
| Selhalo: PCM (ventilace selhání) | VYPNUTÍ | VYPNUTÍ | ZAPNUTO | NENÍ K DISPOZICI |
| Poruchy PCM (přes amp přepětí, přes aktuální) | VYPNUTÍ | ZAPNUTO | ZAPNUTO | ZAPNUTO |
| PCM (ventilace mimo odchylky) | ZAPNUTO | VYPNUTÍ | VYPNUTÍ | ZAPNUTO |
| Úsporném režimu | Přerušované | VYPNUTÍ | VYPNUTÍ | VYPNUTÍ |
| Stažení firmwaru PCM | VYPNUTÍ | Přerušované | Přerušované | Přerušované |

### <a name="pcm-indicator-leds-for-the-backup-battery"></a>Indikátor PCM LED pro záložní battery  

| Stav | Dobrý Battery (zelený) | Poruchy Battery (žlutá) |
|--------|----------------------|-----------------------|
| Battery není k dispozici | VYPNUTÍ | VYPNUTÍ |
| Battery prezentovat a účtovaných | ZAPNUTO | VYPNUTÍ |
| Uvolnění Battery nabíjí nebo údržby | Přerušované | VYPNUTÍ |
| Battery "softwarová" Chyba (obnovitelné) | VYPNUTÍ | Přerušované |
| Battery "pevný" poruch (bez obnovit po) | VYPNUTÍ | ZAPNUTO |
| S odstraněnými Battery | Přerušované | VYPNUTÍ |

## <a name="pcm-leds-for-the-ebod-enclosure"></a>PCM LED pro EBOD přílohu  

Přílohu EBOD má 580W PCM a žádné další battery. Panel PCM pro přílohu EBOD má indikátor LED jenom pro napájecí zdroje a ventilátory. Následující obrázek znázorňuje tyto LED.

   ![PCM LED na přílohu EBOD][3] 
 
V následující tabulce můžete použít ke zjištění stavu PCM.  

| Stav | OK PCM (zelený) | AC selhání (oranžové) | Selhalo: ventilace (oranžové) | Selhalo: Datacentrum (oranžové) |
|--------|---------------|------------------------|------------------|----------------------|
| Žádné elektrické (Pokud chcete přílohu) | VYPNUTÍ | VYPNUTÍ | VYPNUTÍ | VYPNUTÍ |
| Žádné elektrické (jenom tento PCM) | VYPNUTÍ | ZAPNUTO | VYPNUTÍ | ZAPNUTO |
| AC prezentovat PCM zapnuto – OK | ZAPNUTO | VYPNUTÍ | VYPNUTÍ | VYPNUTÍ |
| Selhalo: PCM (ventilace selhání) | VYPNUTÍ | VYPNUTÍ | ZAPNUTO | X |
| Poruchy PCM (přes amp přepětí, přes aktuální | VYPNUTÍ | ZAPNUTO | ZAPNUTO | ZAPNUTO |
| PCM (ventilace mimo odchylky) | ZAPNUTO | VYPNUTÍ | VYPNUTÍ | ZAPNUTO |
| Úsporném modelu | Přerušované | VYPNUTÍ | VYPNUTÍ | VYPNUTÍ |
| Stažení firmwaru PCM | VYPNUTÍ | Přerušované | Přerušované | Přerušované |

## <a name="controller-module-indicator-leds"></a>Indikátor modul řadiče LED  

Zařízení StorSimple obsahuje LED primární správce a moduly EBOD řadiče domény.   

### <a name="monitoring-leds-for-the-primary-controller"></a>Sledování LED primární správce
Na následujícím obrázku budete moci určit LED na primární správce. (Všechny prvky jsou uvedeny na podporu na šířku).  

   ![Sledování LED - primárního zařízení][4]
 
Pomocí v následující tabulce zjistíte, zda modulu řadiče domény funguje správně.  

### <a name="controller-indicator-leds"></a>Indikátor řadiče LED  

| INDIKÁTOR | Popis                                                                            
|---- | ----------- |
| Indikátor ID (modrý) | Označuje, že je je označen modulu. Pokud modré LED bliká pracovního řadiči, správce je aktivní řadiče a druhý je úsporném správce. Další informace přečtěte si část [rozpoznání aktivní řadiče na vašem zařízení](storsimple-controller-replacement.md#identify-the-active-controller-on-your-device). |
| Poruchy LED (žlutá) | Označuje chybu v zařízení.        
| OK LED (zelený) | Rovnoměrné zelené znamená, že správce OK. Přerušované zelené označuje řadiči Chyba konfigurace VPD. |
| Přidružení zabezpečení aktivity LED (zelený) | Rovnoměrné zelené označuje připojení k žádné aktuální činnosti. Přerušované zelené znamená, že připojení neobsahuje probíhající aktivity. |
| Stav Ethernet LED | Pravé označuje činnosti odkaz/sítě: (stabilní zelený) odkaz aktivní (blýskavé zelené) síťové aktivity. Levé označuje rychlosti sítě: (žlutá) 1 000 Mb/s (zelené) 100 Mb/s a (vypnuto) 10 Mb/s. V závislosti na modelu může tento light zablikat i když není povolený rozhraní sítě. |
| PŘÍSPĚVEK LED | Průběh spouštěcí označuje, když je zapnutý správce. Pokud StorSimple zařízení se nepodařilo spustit, tento Indikátor vám pomůže identifikovat bodu v procesu spouštění niž došlo k chybě Support Microsoft. |

>[AZURE.IMPORTANT] 
Pokud rozsvíceno poruch LED došlo k potížím s modulu řadiče domény, který může vyřešit restartováním správce. Pokud restartování správce se tento problém nevyřeší, kontaktujte prosím Microsoft Support.  


### <a name="monitoring-leds-for-the-ebod-ebod-enclosure"></a>Sledování LED EBOD (EBOD přílohu)  

Každá z 6 řadiče Gb/s přidružení zabezpečení EBOD má LED, které označují stav jak je znázorněno na následujícím obrázku.  

  ![Sledování LED - EBOD přílohu][5]

Umožňuje určit, zda modul řadiče EBOD funguje obvykle v následující tabulce.  

### <a name="ebod-controller-module-indicator-leds"></a>EBOD řadiče domény modulu indikátor LED  

|Stav | Modul vstupu a výstupu OK (zelený) | Poruchy modul vstupu a výstupu (žlutá) | Aktivity portů hostitele (zelený) |
|-------|----------------------|-------------------------------|----------------------------|
| Modul řadiče OK | ZAPNUTO | VYPNUTÍ | - |
| Poruchy modul řadiče | VYPNUTÍ | ZAPNUTO | - |
| Žádné externí hostitele port připojení | - | - | VYPNUTÍ |
| Připojení externího hostitele port – žádná aktivita | - | - | ZAPNUTO |
| Host (hostitel) externí port připojení – aktivity | - | - | Přerušované |
| Chyba metadat modulu řadiče domény | Přerušované | - | - |

## <a name="disk-drive-indicator-leds-for-the-primary-enclosure-and-ebod-enclosure"></a>Indikátor diskovou jednotku LED primární přílohu a EBOD přílohu

Zařízení StorSimple má diskových jednotek součástí primárního přílohu a EBOD přílohu. Každého disku obsahuje sledování indikátor LED, jak je uvedeno v této části. 

Pro diskových jednotek stav jednotky je označen zeleným LED a oranžová červený Indikátor připojených na přední jednotlivých modulech jednotka carrier. Následující obrázek znázorňuje tyto LED.

  ![LED diskovou jednotku][6]
 
Umožňuje určit stav každé diskové jednotce, která zase ovlivňuje celkové přední panel Indikátor stavu v následující tabulce.  

### <a name="disk-drive-indicator-leds-for-the-ebod-enclosure"></a>Indikátor diskovou jednotku LED EBOD přílohu  

| Stav | Indikátor OK aktivity (zelený) | Poruchy LED (červená žlutá) | Spojené ops panely LED |
|-------|--------------------------|----------------------|-------------------------|
| Bez disku nainstalovaný | VYPNUTÍ | VYPNUTÍ | Žádná |
| Jednotka nainstalovaný a funkční | Přerušované zapnuto nebo vypnuto s aktivity | X | Žádná |
| Sada identity zařízení SCSI přílohu služby (se) | ZAPNUTO | Přerušované 1 druhé na/1 druhé vypnutí | Žádná |
| Sada bit poruch se zařízení | ZAPNUTO | ZAPNUTO | Logické poruch (červená) |
| Ovládací prvek s obvodovou výpadku | VYPNUTÍ | ZAPNUTO | Modul poruch (červená) |

## <a name="audible-alarms"></a>Zvuková upozornění  

Zařízení s StorSimple obsahuje zvuková upozornění související s primární přílohu a EBOD přílohu. Zvuková upozornění se nachází na přední panely (nazývané také panelu ops) obou přílohy. Zvuková upozornění označuje při podmínku poruch prezentovat. Následující podmínky aktivuje upozornění:  

- Poruchy ventilace nebo selhání
- Napětí mimo rozsah
- Nad nebo pod teploty podmínku
- Teplotní přetečení
- Systémové chyby
- Logické poruch
- Chyba Power dodávky
- Odebrání zadanou mocninu chlazení modul (PCM)  

Následující tabulka popisuje jednotlivé stavy upozornění.  

### <a name="alarm-states"></a>Upozornění v USA  

| Stav upozornění | Akce | Akce se stisknutým tlačítkem ztlumení |
|------------|---------|---------------------------------|
| S0 | Normální režim: Tichý | Dvakrát ZvukovýSignál |
| S1 | Režim poruch: 1 druhé na/1 druhé vypnutí | Přechod na S2 nebo S3 (viz poznámky) |
| S2 | Připomeňte režim: chybovému ZvukovýSignál | Žádná |
| S3 | Ikona ztlumeného režim: Tichý | Žádná |
| S4 | Režim kritické chyby: nepřetržitý upozornění | Není k dispozici: Ztlumit není aktivní |

> [AZURE.NOTE] 

>  - V upozornění stavu S1 Pokud není stisknete ztlumení v rámci 2 minuty stavu automaticky přechází do S2 nebo S3.  
>  - Upozornění státy S1 S4 vrátit S0 po vymazání poruch podmínku.  
>  - Stát kritické chyby S4 může být zadán z jiných států.  

Zvuková upozornění můžete Ztlumit tak, že stisknete tlačítko Ztlumit na panelu ops. Automatické ztlumení po dvě minuty Pokud dojde přepínačem Ztlumit není spravovaný ručně. Upozornění ztlumený, budou dál zvuk s krátkou chybovému signálem označíte, že problém stále existuje. Upozornění budou pasivní při vymažou všechny problémy.  

Následující tabulka popisuje různé podmínky upozornění.  

### <a name="alarm-conditions"></a>Podmínky upozornění  

| Stav | Závažnosti | Upozornění | OPS panel LED |
|--------|---------|--------|----------------|
| Upozornění PCM – ztráty Datacentrum power z jednoho PCM | Poruchy – bez ztráty redundance | S1 | Modul poruch|
|Upozornění PCM – ztráty Datacentrum power z jednoho PCM | Poruchy – ztráty redundance | S1 | Modul poruch |
| Selhalo: ventilace PCM | Poruchy – ztráty redundance | S1 | Modul poruch |
| Modul SBB detekovaný PCM poruch | Poruch | S1 | Modul poruch |
| Odebrání PCM | Chyba konfigurace | Žádná | Modul poruch |
| Chyba konfigurace přílohu | Poruchy – kritické | S1 | Modul poruch |
| Nízké teploty upozornění upozornění | Upozornění | S1 | Modul poruch |
| Vysoký upozornění teploty upozornění | Upozornění | S1 | Modul poruch |
| Přes teploty upozornění | Poruchy – kritické | S1 | Modul poruch |
| Chyba při bus I2C | Poruchy – ztráty redundance | S1 | Modul poruch |
| OPS panel chyba komunikace (I2C) | Poruchy – kritické     | S1 | Modul poruch |
| Chyba řadiče | Poruchy – kritické | S1 | Modul poruch |
| SBB rozhraní modul poruch | Poruchy – kritické | S1 | Modul poruch |
| SBB rozhraní modul poruch – žádné funkční moduly zbývající | Poruchy – kritické | S4 | Modul poruch |
| Modul rozhraní SBB odebrána | Upozornění | Žádná | Modul poruch |
| Jednotka power ovládací prvek poruch | Varování – bez ztráty power jednotka | S1 | Modul poruch |
| Jednotka power ovládací prvek poruch | Poruchy – kritický; ztráta jednotky výkonu | S1 | Modul poruch |
| Jednotka byla odebrána. | Upozornění | Žádná | Modul poruch |
| Nedostatečná power k dispozici | Upozornění | žádná | Modul poruch |

## <a name="next-steps"></a>Další kroky

Další informace o [součástech StorSimple hardware a stav](storsimple-monitor-hardware-status.md).

[1]: ./media/storsimple-monitoring-indicators/storsimple-monitoring-indicators-IMAGE01.png
[2]: ./media/storsimple-monitoring-indicators/storsimple-monitoring-indicators-IMAGE02.png
[3]: ./media/storsimple-monitoring-indicators/storsimple-monitoring-indicators-IMAGE03.png
[4]: ./media/storsimple-monitoring-indicators/storsimple-monitoring-indicators-IMAGE04.png
[5]: ./media/storsimple-monitoring-indicators/storsimple-monitoring-indicators-IMAGE05.png
[6]: ./media/storsimple-monitoring-indicators/storsimple-monitoring-indicators-IMAGE06.png

 
