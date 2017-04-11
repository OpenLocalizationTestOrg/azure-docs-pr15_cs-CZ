<properties
 pageTitle="Čtení zpráv zachován v úložišti Azure | Microsoft Azure"
 description="Zprávy zařízení do cloudu sledujte, jak se zapsat k Azure table storage."
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

# <a name="33-read-messages-persisted-in-azure-storage"></a>3.3 číst zprávy zachován v úložišti Azure

## <a name="331-what-will-you-do"></a>3.3.1 co bude dělat

Sledování zpráv zařízení do cloudu, které jsou odeslány z malin pí 3 do rozbočovače IoT zprávy jsou aby došlo k zápisu úložiště tabulek Azure. Pokud budete splňovat nějaké problémy, hledání řešení [Poradce při potížích s stránky](iot-hub-raspberry-pi-kit-node-troubleshooting.md).

## <a name="332-what-will-you-learn"></a>3.3.2 co bude jste se naučili

- Jak používat úkolu číst zprávy gulp čtení zpráv zachován v úložišti tabulek Azure.

## <a name="333-what-do-you-need"></a>3.3.3 co je potřeba

- Musíte úspěšně jste dokončili v předchozí části [Spustit aplikaci ukázkové Azure blikání na malin pí 3](iot-hub-raspberry-pi-kit-node-lesson3-run-azure-blink.md).

## <a name="334-read-new-messages-from-your-storage-account"></a>3.3.4 Přečtěte si nové zprávy z vašeho účtu úložiště

V předchozí části spustili ukázku aplikace na vaše pí. Ukázka zprávy aplikace odeslané k rozbočovači Azure IoT. Zprávy poslané na vaše Centrum IoT jsou uložené v úložišti tabulek Azure přes aplikaci Azure (funkce). Je třeba úložiště Azure připojovací řetězec čtení zpráv z úložiště tabulek Azure.

Čtení zpráv uložených v úložišti tabulek Azure, postupujte takto:

1. Získání připojovacího řetězce spuštěním následující příkazy:

    ```bash
    az storage account list -g iot-sample --query [].name
    az storage account show-connection-string -g iot-sample -n {storage name}
    ```

    První příkaz načte `storage name` příkaz v druhém, který se používá k získání připojovacího řetězce. `iot-sample`je výchozí hodnota `{resource group name}` změníte neměli hodnotu v lekci 2.

2. Otevřete soubor konfigurace `config-raspberrypi.json` soubor ve Visual Studiu kódu spuštěním následujícího příkazu:

    ```bash
    # For Windows command prompt
    code %USERPROFILE%\.iot-hub-getting-started\config-raspberrypi.json

    # For macOS or Ubuntu
    code ~/.iot-hub-getting-started/config-raspberrypi.json
    ```

3. Nahrazení `[Azure Storage connection string]` pomocí připojovacího řetězce jste získali v kroku 1.
4. Uložení `config-raspberrypi.json` soubor.
5. Odeslání zprávy znovu a číst z úložiště tabulek Azure pomocí tohoto příkazu:

    ```bash
    gulp run --read-storage
    ```

    Logika pro čtení z úložiště tabulek Azure je v `azure-table.js` soubor.

    ![spuštění – gulp úložiště pro čtení](media/iot-hub-raspberry-pi-lessons/lesson3/gulp_read_message.png)

## <a name="335-summary"></a>3.3.5 souhrn

Jste úspěšně připojené vaše pí rozbočovače IoT v cloudu a použít ukázková aplikace blikání k odesílání zpráv zařízení do cloudu. Azure funkce aplikace se také používá k ukládání příchozích zpráv centrální IoT k Azure table storage. Můžete přesunout další lekce o tom, jak posílat zprávy cloudu zařízení z rozbočovače IoT do svého pí.

## <a name="next-steps"></a>Další kroky

[Lekce 4: Posílání zpráv cloudu zařízení](iot-hub-raspberry-pi-kit-node-lesson4-send-cloud-to-device-messages.md)
