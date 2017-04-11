<properties
 pageTitle="Konfigurace zařízení | Microsoft Azure"
 description="Konfigurace vaší malin pí 3 pro první použití a nainstalujte Raspbian OS bezplatné operačním systémem, která je optimalizovaná pro hardware malin pí."
 services="iot-hub"
 documentationCenter=""
 authors="shizn"
 manager="timlt"
 tags=""
 keywords=""/>

<tags
 ms.service="iot-hub"
 ms.devlang="multiple"
 ms.topic="article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="10/21/2016"
 ms.author="xshi"/>

# <a name="11-configure-your-device"></a>1.1 konfigurace zařízení

## <a name="111-what-you-will-do"></a>1.1.1 co bude dělat

Konfigurace pí pro první použití a nainstalujte Raspbian operační systém, bezplatné operačním systémem, která je optimalizovaná pro hardware malin pí. Pokud budete splňovat nějaké problémy, hledání řešení [Poradce při potížích s stránky](iot-hub-raspberry-pi-kit-node-troubleshooting.md).

## <a name="112-what-you-will-learn"></a>1.1.2 co jste se naučili

V této části se dozvíte:

- Jak nainstalovat Raspbian na vaše pí
- Jak power si svůj pí pomocí USB kabelu
- Jak se připojit k síti pomocí ethernetového kabelu nebo Wi-Fi vaší pí
- Jak přidat LED breadboard a připojte ji k vaší pí

## <a name="113-what-you-need"></a>1.1.3 co byste měli

V této části, je potřeba k provedení následujících částí z vaší malin pí 3 Starter Kit:

- Vývěsky malin pí 3
- Karta MicroSD 16GB
- Power 2 a 5 poskytnout USB kabel malých šest nohy
- Breadboard
- Vedení spojnice
- Odporem 560 Ohm
- Difusní 10mm LED
- Ethernetového kabelu

![Co je v sadě Starter](media/iot-hub-raspberry-pi-lessons/lesson1/starter_kit.jpg)

Budete potřebovat:

- Vytvoření pevné nebo bezdrátové připojení pí pro připojení k
- USB ss adaptér nebo ss mini karta vypálit obrázek OS na kartě MicroSD.
- Počítač se systémem Windows, Macintosh nebo Linux. Počítači se používá k instalaci Raspbian na kartě MicroSD.
- Připojení k Internetu, stáhněte si potřebné nástroje a software

## <a name="114-install-raspbian-on-the-microsd-card"></a>1.1.4 na kartě MicroSD nainstalovat Raspbian

Připravte se na kartě MicroSD psát Raspbian obrázku.

1. Stáhněte si Raspbian.
  1. [Stáhněte si](https://www.raspberrypi.org/downloads/raspbian/) soubor zip Raspbian Klára s bod.
  2. Extrahování Raspbian obrázek do složky ve vašem počítači.
2. Nainstalujte Raspbian na kartu MicroSD.
  1. [Stáhněte si](https://www.etcher.io) a nainstalujte nástroj vypalovačku Etcher ss karty.
  2. Spusťte Etcher a vyberte Raspbian obrázek, který jste extrahovali v kroku 1.
  3. Vyberte kartu jednotku MicroSD.
    Poznámka: Etcher může vybrali správnou jednotku.
  4. Klepněte na Flash nainstalovat Raspbian MicroSD kartu.
  5. Odeberte z karty MicroSD ze svého počítače po dokončení.
    Poznámka: Je bezpečné odebrat z karty MicroSD přímo, protože Etcher automaticky vysune nebo odpojí karty MicroSD po dokončení.
  6. Vložte kartu MicroSD do vašeho čísla pí.

![Vložení ss karty](media/iot-hub-raspberry-pi-lessons/lesson1/insert_sdcard.jpg)

## <a name="115-power-on-your-pi"></a>1.1.5 Zapněte vaší pí

Zapněte pí pomocí malých USB kabel a napájení.

![Zapněte](media/iot-hub-raspberry-pi-lessons/lesson1/micro_usb_power_on.jpg)

> [AZURE.NOTE] Je důležité použít napájení v sadě pro, která je nejméně 2, abyste měli jistotu, že vaše malin krmených dost power fungovat správně.

## <a name="116-connect-your-raspberry-pi-3-to-the-network"></a>1.1.6 pí 3 malin připojení k síti

Vaše pí připojením drátovou síť nebo bezdrátové sítě. Zkontrolujte, jestli je vaše pí připojený k stejné síti s vaším počítačem. Například připojením vaší pí přepínači je počítač připojený k.

### <a name="1161-connect-to-a-wired-network"></a>1.1.6.1 připojení k drátovou síť

Připojení vaší pí k drátovou síť pomocí ethernetového kabelu. Dva LED na vaše pí zapnout Pokud připojení.

![Připojení pomocí ethernetového kabelu](media/iot-hub-raspberry-pi-lessons/lesson1/connect_ethernet.jpg)

### <a name="1162-connect-to-a-wireless-network"></a>1.1.6.2 připojení k bezdrátové sítě

Postupujte podle [pokynů](https://www.raspberrypi.org/learning/software-guide/wifi/) z Foundation pí malin připojení vaší pí k bezdrátové sítě. Tyto pokyny vyžadují, abyste se nejprve připojit monitoru a klávesnice vaší PI.

## <a name="117-connect-the-led-to-your-pi"></a>1.1.7 připojení LED k vaší pí

K provedení této úlohy, použití [breadboard](https://learn.sparkfun.com/tutorials/how-to-use-a-breadboard), kabely spojnice, LED a se odpor. Je připojení k [univerzální vstupní a výstupní](https://www.raspberrypi.org/documentation/usage/gpio/) (GPIO) porty vašeho čísla pí. 

![Breadboard LED a odpor](media/iot-hub-raspberry-pi-lessons/lesson1/breadboard_led_resistor.jpg)

1. Připojte kratší nohy LED **GPIO zem (Pin 6)**.
2. Připojení k jedné nohy se odpor delší nohy LED.
3. Připojení jiných nohy se odpor **GPIO**4 (PIN kód 7).

Všimněte si, že polarita Indikátor důležité. Toto nastavení používají polarita je známý jako aktivní minimum.

![Uspořádání kolíků](media/iot-hub-raspberry-pi-lessons/lesson1/pinout_breadboard.png)

Blahopřání! Jste úspěšně nakonfigurován vaší pí.

## <a name="118-summary"></a>1.1.8 souhrn

V této části jste se naučili konfiguraci vašeho pí nainstalujte Raspbian, připojení k síti vaší pí a připojení LED k vaší pí. Všimněte si, že LED není zatím částečné nahoru. V následující části můžete nainstalovat potřebné nástroje a software při přípravě výpočetnímu ukázku aplikace vaší pí.

![Hardwarová je připravený](media/iot-hub-raspberry-pi-lessons/lesson1/hardware_ready.jpg)

## <a name="next-steps"></a>Další kroky

[1.2 získat potřebné nástroje](iot-hub-raspberry-pi-kit-node-lesson1-get-the-tools-win32.md)
