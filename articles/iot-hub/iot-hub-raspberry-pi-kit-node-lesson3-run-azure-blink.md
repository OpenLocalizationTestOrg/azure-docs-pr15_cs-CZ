<properties
 pageTitle="Spuštění aplikace vzorku k odesílání zpráv zařízení do cloudu | Microsoft Azure"
 description="Nasazení a spuštění aplikace ukázkové vaší malin pí 3, zasílání zpráv IoT centrální a LED bliká."
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

# <a name="32-run-sample-application-to-send-device-to-cloud-messages"></a>3,2 spuštění aplikace vzorku k odesílání zpráv zařízení do cloudu

## <a name="321-what-you-will-do"></a>3.2.1 co bude dělat

Nasazení a spuštění aplikace na ukázkové vaší malin pí 3, zasílání zpráv rozbočovače IoT. Pokud budete splňovat nějaké problémy, hledání řešení [Poradce při potížích s stránky](iot-hub-raspberry-pi-kit-node-troubleshooting.md).

## <a name="322-what-you-will-learn"></a>3.2.2 co jste se naučili

- Jak používat nástroj gulp nasazení a spuštění aplikace Node.js vzorku na vaše pí.

## <a name="323-what-you-need"></a>3.2.3 co byste měli

- Musíte byla úspěšně dokončena s předchozím oddílem v této lekci: [Vytvoření aplikace pro Azure funkce a účet Azure úložiště pro zpracování a ukládání IoT Centrum zpráv](iot-hub-raspberry-pi-kit-node-lesson3-deploy-resource-manager-template.md).

## <a name="324-get-your-iot-hub-and-device-connection-strings"></a>3.2.4 získáte IoT centrální a zařízení připojovací řetězce

Připojovací řetězec zařízení se používá pro připojení pí rozbočovače IoT. Rozbočovač IoT připojení řetězec se používá pro připojení rozbočovače IoT pro identitu zařízení, která představuje Pi v centru IoT.

- Získáte připojovací řetězec centrální IoT spuštěním následujícího příkazu Azure rozhraní příkazového řádku:

```bash
az iot hub show-connection-string --name {my hub name} --resource-group iot-sample
```

`{my hub name}`je název, který jste zadali v lekci 2. Použití `iot-sample` jako hodnota `{resource group name}` změníte neměli hodnotu v lekci 2.

- Získání připojovacího řetězce zařízení spuštěním následujícího příkazu:

```bash
az iot device show-connection-string --hub {my hub name} --device-id myraspberrypi --resource-group iot-sample
```

`{my hub name}`Přenese stejnou hodnotu jako použitým pomocí příkazu předchozí. Použití `iot-sample` jako hodnota `{resource group name}` a používat `myraspberrypi` jako hodnota `{device id}` změníte neměli hodnotu v lekci 2.

## <a name="325-configure-the-device-connection"></a>3.2.5 nakonfiguruje připojení zařízení

1. Inicializace konfiguračního souboru spuštěním následující příkazy:

    ```bash
    npm install
    gulp init
    ```

2. Otevřete soubor konfigurace zařízení `config-raspberrypi.json` ve Visual Studio kódu pomocí tohoto příkazu:

    ```bash
    # For Windows command prompt
    code %USERPROFILE%\.iot-hub-getting-started\config-raspberrypi.json
  
    # For macOS or Ubuntu
    code ~/.iot-hub-getting-started/config-raspberrypi.json
    ```

    ![config.JSON](media/iot-hub-raspberry-pi-lessons/lesson3/config.png)

3. Zkontrolujte následující nahrazení v `config-raspberrypi.json` souboru:

  - **[IP adresa nebo zařízení hostname]** nahraďte zařízení IP adresa nebo název hostitele jste získali od `device-discovery-cli` nebo s hodnotou dědí od nakonfigurováno v lekci 1.
  - Nahrazení **[IoT zařízení připojovací řetězec]** s `device connection string` můžete získat.
  - Nahrazení **[IoT centrální připojovací řetězec]** s `iot hub connection string` můžete získat.

Aktualizaci `config-raspberrypi.json` souboru tak, aby nasadíte ukázková aplikace ze svého počítače.

## <a name="326-deploy-and-run-the-sample-application"></a>3.2.6 nasazení a spustit aplikaci ukázka

Nasazení a spuštěním ukázková aplikace na vaše pí tento příkaz:

```bash
gulp
```

> [AZURE.NOTE] Spuštění úlohy gulp výchozí `install-tools`, `deploy`, a `run` úkoly s sebou. V části [lekci 1](iot-hub-raspberry-pi-kit-node-lesson1-deploy-blink-app.md)jste spustili tyto úkoly jeden po druhém samostatně.

## <a name="327-verify-the-sample-application-works"></a>3.2.7 ověření ukázkové aplikace works

Měli byste vidět LED, kterému je připojen váš pí blikající každé dvě sekundy. Pokaždé, když LED bliká, ukázková aplikace odešle zprávu rozbočovače IoT a ověří, že zprávy byly úspěšně odeslal rozbočovače IoT. Kromě toho u každé zprávy přijaté centru IoT zprávu tisku v tomto okně. Ukázková aplikace automaticky ukončí po odeslání 20 zprávy.

![](media/iot-hub-raspberry-pi-lessons/lesson3/gulp_run.png)

## <a name="328-summary"></a>3.2.8 souhrn

Jste nasazeném a spuštění nové blikání ukázkové pí k odesílání zpráv zařízení do cloudu na vaše Centrum IoT. Sledování zpráv, jako jsou napsali na účtu v úložišti Azure můžete přesunout do další části.

## <a name="next-steps"></a>Další kroky

[3.3 číst zprávy zachován v úložišti Azure](iot-hub-raspberry-pi-kit-node-lesson3-read-table-storage.md)