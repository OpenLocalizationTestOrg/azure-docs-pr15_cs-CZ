<properties
 pageTitle="Vytvoření a nasazení aplikace blikání | Microsoft Azure"
 description="Klonovat ukázková Node.js aplikace z Github a gulp nasazení tuto aplikaci k vývěsce malin pí 3. Tato ukázková aplikace bliká LED připojení na vývěsku každé dvě sekundy."
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

# <a name="13-create-and-deploy-the-blink-application"></a>1.3 vytvoření a nasazení aplikace blikání

## <a name="131-what-you-will-do"></a>1.3.1 co bude dělat

Klonovat ukázková Node.js aplikace z Github a gulp nástrojem pro nasazení aplikace ukázkové malin pí 3. Ukázková aplikace bliká LED připojení na vývěsku každé dvě sekundy. Pokud budete splňovat nějaké problémy, hledání řešení [Poradce při potížích s stránky](iot-hub-raspberry-pi-kit-node-troubleshooting.md).

## <a name="132-what-you-will-learn"></a>1.3.2 co jste se naučili

- Jak používat `device-discover-cli` nástroj načítat sítě informace o vaší pí.
- Jak se nasazení a spustit aplikaci ukázkové vaší pí.
- Nasazení a ladění aplikacím vzdáleně na vaše pí.

## <a name="133-what-you-need"></a>1.3.3 co byste měli

Musíte byla úspěšně dokončena sledovat částech lekci 1:

- [Konfigurace zařízení](iot-hub-raspberry-pi-kit-node-lesson1-configure-your-device.md)
- [Získání nástrojů](iot-hub-raspberry-pi-kit-node-lesson1-get-the-tools-win32.md)

## <a name="134-obtain-the-ip-address-and-host-name-of-your-pi"></a>1.3.4 získá IP adresu a Host (hostitel) název vašeho čísla pí

Otevřete okno příkazového řádku v systému Windows nebo okno terminálu macOS nebo se systémem Ubuntu a znovu spusťte tento příkaz:

```bash
devdisco list --eth
```

Měli byste vidět výstup, která je podobná této:

![zjišťování zařízení](media/iot-hub-raspberry-pi-lessons/lesson1/device_discovery.png)

Poznamenejte si `IP address` a `hostname` vašeho čísla pí. Potřebujete tyto informace dál v tomto oddílu.

> [AZURE.NOTE] Zkontrolujte, jestli je vaše pí připojený k stejné síti s vaším počítačem. Například váš počítač připojen k bezdrátové sítě vaší pí připojeni k drátovou síť, nemusí se IP adresu do výstupu devdisco.

## <a name="135-clone-the-sample-application"></a>1.3.5 klonovat ukázková aplikace

Otevřete ukázkový kód, postupujte takto:

1. Ukázka úložiště z Github klonovat spuštěním následujícího příkazu:

    ```bash
    git clone https://github.com/Azure-Samples/iot-hub-node-raspberrypi-getting-started.git
    ```

2. Otevřete aplikaci vzorku ve Visual Studiu kódu spuštěním následující příkazy:

    ```bash
    cd iot-hub-node-raspberrypi-getting-started
    cd Lesson1
    code .
    ```

![Struktura repo](media/iot-hub-raspberry-pi-lessons/lesson1/vscode-blink-mac.png)

`app.js` Souborů v `app` podsložky je klíčové zdrojového souboru, který obsahuje kód pro ovládací prvek LED.

### <a name="136-install-application-dependencies"></a>1.3.6 instalace závislostí aplikací

Nainstalujte knihoven a jiných modulů kontroly, které potřebujete pro ukázkovou aplikaci spuštěním následujícího příkazu:

```bash
npm install
```

## <a name="137-configure-the-device-connection"></a>1.3.7 nakonfiguruje připojení zařízení

Konfigurace připojení zařízení, postupujte takto:

1. Vygenerujte konfiguračního souboru zařízení pomocí tohoto příkazu:

    ```bash
    gulp init
    ```

    Konfigurační soubor `config-raspberrypi.json` obsahuje přihlašovací údaje uživatele slouží k přihlášení svůj pí. Chcete-li předejít únik přihlašovací údaje uživatele, vygeneruje se konfiguračního souboru do podsložky `.iot-hub-getting-started` výchozí složky ve vašem počítači.

2. Otevřete soubor konfigurace zařízení ve Visual Studiu kódu spuštěním následujícího příkazu:

    ```bash
    # For Windows command prompt
    code %USERPROFILE%\.iot-hub-getting-started\config-raspberrypi.json

    # For macOS or Ubuntu
    code ~/.iot-hub-getting-started/config-raspberrypi.json
    ```

3. Nahrazení zástupného symbolu `[device hostname or IP address]` s IP adresu nebo název hostitele, který se zobrazí v části 1.3.4.

    ![Config.JSON](media/iot-hub-raspberry-pi-lessons/lesson1/vscode-config-mac.png)

Blahopřejeme! Jste si vytvořili úspěšně první ukázka aplikace pro váš pí.

## <a name="138-deploy-and-run-the-sample-application"></a>1.3.8 nasazení a spusťte aplikaci ukázka

### <a name="1381-install-nodejs-and-npm-on-your-pi"></a>1.3.8.1 nainstalovat Node.js a NPM vaše pí

Nainstalujte Node.js a NPM vaše pí spuštěním následujícího příkazu:

```bash
gulp install-tools
```

Deset minut poprvé spustíte tento úkol může trvat.

### <a name="1382-deploy-and-run-the-sample-app"></a>1.3.8.2 nasazení a spusťte aplikaci ukázka

Nasazení a spuštěním ukázková aplikace tento příkaz:

```bash
gulp deploy && gulp run
```

### <a name="1383-verify-the-app-works"></a>1.3.8.3 ověření aplikace works

Teď byste měli vidět LED vaší pí blikající každé dvě sekundy.  Pokud nevidíte blikající LED, přečtěte si článek [Poradce při potížích s Průvodce](iot-hub-raspberry-pi-kit-node-troubleshooting.md) pro řešení běžných problémů.
![Indikátor blikající](media/iot-hub-raspberry-pi-lessons/lesson1/led_blinking.jpg)

> [AZURE.NOTE] Použití `Ctrl + C` ukončíte aplikaci.

## <a name="139-summary"></a>1.3.9 souhrn

Jste nainstalovali požadované nástroje pro práci s vaší pí a nasazení aplikace ukázkové vaší PI blikání LED. Teď můžete přesunout na Další lekce k vytváření, nasazení a spuštění jiné aplikace vzorku, ke kterému je připojen váš pí Azure IoT centrální odesílat a přijímat zprávy.

## <a name="next-steps"></a>Další kroky

Nyní jste připraveni začít lekci 2, který začíná se [zobrazí nástroje Azure](iot-hub-raspberry-pi-kit-node-lesson2-get-azure-tools-win32.md)
