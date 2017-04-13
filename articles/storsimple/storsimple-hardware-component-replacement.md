<properties 
   pageTitle="Výměna součástí hardwaru StorSimple | Microsoft Azure"
   description="Popisuje, jak bezpečně nahrazení PCMs, battery, moduly řadiče domény, EBOD řadiče, diskových jednotek a rámu StorSimple zařízení."
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
   ms.date="10/11/2016"
   ms.author="alkohli" />

# <a name="storsimple-hardware-component-replacement"></a>Výměna součástí hardwaru StorSimple

## <a name="overview"></a>Základní informace

Výukové programy pro nahrazení hardware součásti popisu hardwaru součástí řady zařízení Microsoft Azure StorSimple 8000 a kroky potřebné k odebrání a nahradit je. Tento článek popisuje ikony zabezpečení, obsahuje odkazy na podrobné výukové programy a seznamem součástí, které jsou výměnné.

>[AZURE.IMPORTANT] Než se pokusíte odstranit nebo nahradit libovolnou součást StorSimple, ujistěte se, kontrolujte [konvence ikonu zabezpečení](#safety-icon-conventions) a další [bezpečnostní](storsimple-safety.md).
 
### <a name="safety-icon-conventions"></a>Konvence ikonu zabezpečení

Následující tabulka popisuje ikony bezpečnosti používaným v těchto kurzech. Zaměřte se na tyto ikony bezpečnosti průběžně kroky odebrat a nahradit součásti zařízení.

| Ikona | Text | Další informace |
|:---- |:---- |:-----------|
|![Ikona upozornění](./media/storsimple-hardware-component-replacement/Warning.png)| **NEBEZPEČÍ!** | Označuje nebezpečné situaci, pokud není vyhnout, bude mít za následek smrt nebo vážně škod. Toto slovo signálu se omezí na největší situací.|
|![Ikona upozornění](./media/storsimple-hardware-component-replacement/Warning.png)| **UPOZORNĚNÍ** | Označuje nebezpečné situaci, pokud není vyhnout, může mít za následek smrt nebo vážně škod.|
|![Ikona upozornění](./media/storsimple-hardware-component-replacement/Caution.png)| **UPOZORNĚNÍ!** |Označuje nebezpečné situaci, pokud není vyhnout, může vést k menší a střední škod.|
|![Ikona oznámení](./media/storsimple-hardware-component-replacement/NoticeIcon.png)| **POZNÁMKA:** | Označuje považuje za důležité, ale není riziko související informace.|
|![Ikona elektroinstalace absorbér](./media/storsimple-hardware-component-replacement/Electric.png) | **Elektrické absorbér rizika** | Označuje nejvyšší hodnota.|
|![Ikona hodně weight (váha)](./media/storsimple-hardware-component-replacement/Weight.png)| **Hodně weight (váha)**| |
|![Neobsahuje ikonu nacházet části uživatele](./media/storsimple-hardware-component-replacement/NoUserServiceableParts.png)| **Žádné části měnit uživatele** | K přístup, pokud správně školení.|
|![Ikona pokyny pro čtení](./media/storsimple-hardware-component-replacement/ReadInstructions.png)|**Všechny pokyny nejdřív přečíst**| |
|![Ikona nebezpečí tipu](./media/storsimple-hardware-component-replacement/TipHazard.png)|**Tip: rizika**| |

### <a name="before-you-begin"></a>Než začnete

Seznamte se s zabezpečení informací o zabezpečení a zařízení ikony použité v tomto kurzu. Přejděte na [bezpečně instalaci a používání zařízení StorSimple](storsimple-safety.md) podrobné informace. Je potřeba zkontrolovat [bezpečnostní](storsimple-safety.md#handling-precautions) před zpracovat StorSimple zařízení. 

Před pokusem o nahrazení komponenty, zvažte následující informace.

![Ikona varování](./media/storsimple-hardware-component-replacement/Warning.png) ![elektroinstalace absorbér ikonu](./media/storsimple-hardware-component-replacement/Electric.png) **upozornění!** 

- Světlá sami správně pomocí elektrostatické uvolnění nebo antistatická použití materiálu při zpracování moduly a komponent StorSimple zařízení.

- Není klepněte na tlačítko všechny obvod. Použití předaném úchytů a vodítek při zpracování součástí, které může mít vystaven obvod.

![Ikona varování](./media/storsimple-hardware-component-replacement/Warning.png) ![Všimněte si ikonu](./media/storsimple-hardware-component-replacement/NoticeIcon.png) **oznámení:**

Pokud nahradíte modulu **nikdy nechte prázdné pozice v zadní přílohu**. Získejte náhradní nebo prázdná modul před odebráním části problém.

## <a name="hardware-component-replacement-procedures"></a>Hardwarová součást náhradní postupy

Zařízení s řady StorSimple 8000 se skládá z několika moduly plug-in v primární a/nebo EBOD přílohy. 8100 obsahuje jeden primární přílohu, že 8600 duální přílohu zařízení s primární přílohu a přílohu EBOD.

V následujících tabulkách jsou shrnuty hlavní hardwarových součástí vašeho zařízení. Klikněte na odkaz ve sloupci **Postup nahrazení** přejdete na přidružený výukového programu.

|Součásti|# Prezentace|Modul plug-in?|Postup nahrazení
|:---------|:--------|:--------------|:---------------------|
| Šasi|1|Ne|[Nahrazení skříni na zařízení s StorSimple](storsimple-chassis-replacement.md) |
|Primární zařízení|2|Ano| [Nahrazení modulu řadiče domény na zařízení s StorSimple](storsimple-controller-replacement.md) |
|764W Power a chlazení moduly (PCMs)|2|Ano| [Nahrazení Power a chlazení modul na zařízení s StorSimple](storsimple-power-cooling-module-replacement.md) |
|Zálohování battery|2|Ano| [Nahrazení modulu záložní battery na zařízení s StorSimple](storsimple-battery-replacement.md) |
|Diskových jednotek|12|Ano| [Nahrazení na pevný disk na zařízení s StorSimple](storsimple-disk-drive-replacement.md) |

**Tabulka 1** Hardwarová součástí primárního přílohu

Primární přílohu a přílohu EBOD lišit v jejich vstupu a výstupu moduly. Dále PCMs mít různých příkonem. PCMs v primární přílohu jsou 764 W, že v přílohu EBOD jsou 580 W. PCMs v primární přílohu také obsahovat modulu záložní battery.

|Součásti|# Prezentace|Modul plug-in?| Postup nahrazení
|:---------|:--------|:--------------|:---------------------|
|Šasi|1|Ne| [Nahrazení skříni na zařízení s StorSimple](storsimple-chassis-replacement.md) |
|EBOD řadiče|2|Ano| [Nahrazení EBOD řadiče na zařízení s StorSimple](storsimple-ebod-controller-replacement.md) |
|580W Power a chlazení moduly (PCMs)|2|Ano| [Nahrazení Power a chlazení modul na zařízení s StorSimple](storsimple-power-cooling-module-replacement.md) |
|Diskových jednotek|12|Ano| [Nahrazení na pevný disk na zařízení s StorSimple](storsimple-disk-drive-replacement.md) |

**Tabulka 2** Hardwarová součástí EBOD přílohu

V následujících přední a zadní diagramy jsou zvýrazněné moduly plug-in na zařízení. Můžete tyto diagramy a zjistěte si polohu jednotlivé moduly plug-in je-li náhradní třeba. Přední diagram znázorňuje diskových jednotek a zadní diagramy EBOD přílohu a zobrazit primární přílohu moduly plug-in.

![Frontplane zařízení s diskových jednotek](./media/storsimple-hardware-component-replacement/IC741028.png)

**Obrázek 1** Přední zařízení

|Popisek|Popis|
|:----|:----------|
|0 - 11|Diskové jednotky (celkem 12)|

Primární přílohu a přílohu EBOD mají jednotka carrier moduly. Skříni máte dvanáct 3,5" diskových jednotek uspořádaných do formátu 3 tak, že 4.

![Základní modulů primární přílohu zařízení](./media/storsimple-hardware-component-replacement/IC740994.png)

**Obrázek 2** Zadní strana primární přílohu

|Popisek|Popis|
|:----|:----------|
|1|PCM 0|
|2|PCM 1|
|3|Řadiče 0|
|4|Řadiči 1|

![Základní zařízení EBOD přílohu moduly plug-in](./media/storsimple-hardware-component-replacement/IC769599.png)

**Obrázek 3** Zadní strana okna EBOD přílohu

|Popisek|Popis|
|:----|:----------|
|1|PCM 0|
|2|PCM 1|
|3|EBOD řadiče 0|
|4|EBOD řadiči 1|

## <a name="field-replaceable-units"></a>Pole výměnné jednotky

Následující pole výměnné jednotky (FRU) jsou k dispozici pro zařízení s StorSimple:

- Šasi (včetně panelu integrované operace)

- 764 W AC PCM

- 580 W AC PCM

- Jednotku pevného disku modul carrier jednotka

- Modul řadiče

- Modul řadiče EBOD

- Zálohování battery modul

- Připojení železniční kit regálů

Ujistěte se, [Kontaktujte podporu od Microsoftu](storsimple-contact-microsoft-support.md) objednání některou z těchto náhradní jednotky.

## <a name="next-steps"></a>Další kroky

Zkontrolujte všechny [informace o zabezpečení](storsimple-safety.md) před pokusem o nahrazení komponenty StorSimple hardwaru.
