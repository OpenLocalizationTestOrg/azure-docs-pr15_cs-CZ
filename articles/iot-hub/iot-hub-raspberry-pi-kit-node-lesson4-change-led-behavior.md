<properties
 pageTitle="Nepovinný oddíl – změna zapnutým a vypnutým chování LED | Microsoft Azure"
 description="Přizpůsobení zprávy, které chcete změnit LED zapínat a vypínat chování."
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

# <a name="42-optional-section-change-the-on-and-off-behavior-of-the-led"></a>4.2 Nepovinný oddíl: Změna zapnutým a vypnutým chování LED

## <a name="421-what-you-will-do"></a>4.2.1 co bude dělat

Přizpůsobení zprávy, které chcete změnit LED zapínat a vypínat chování. Pokud budete splňovat nějaké problémy, hledání řešení [Poradce při potížích s stránky](iot-hub-raspberry-pi-kit-node-troubleshooting.md).

## <a name="422-what-you-will-learn"></a>4.2.2 co jste se naučili

Pomocí dalších funkcí Node.js můžete změnit LED zapínat a vypínat chování.

## <a name="423-what-you-need"></a>4.2.3 co byste měli

Musíte úspěšně jste dokončili [4.1 spustit aplikaci vzorku na malin pí pro příjem obláčkem, která zařízení zprávy](iot-hub-raspberry-pi-kit-node-lesson4-send-cloud-to-device-messages.md).

## <a name="424-add-nodejs-functions"></a>4.2.4 přidat Node.js funkce

1. Otevřete aplikaci vzorku ve Visual Studiu kódu spuštěním následující příkazy:

    ```bash
    cd Lesson4
    code .
    ```

2. Otevřít `app.js` souboru a na konci přidejte následující funkce:

    ```javascript
    function turnOnLED() {
      wpi.digitalWrite(CONFIG_PIN, 1);
    }

    function turnOffLED() {
      wpi.digitalWrite(CONFIG_PIN, 0);
    }
    ```

    ![aktualizace app.js](media/iot-hub-raspberry-pi-lessons/lesson4/updated_app_js.png)

3. Přidejte následující podmínky před výchozí jednu v případě přepínače bloku `receiveMessageCallback` funkce:

    ```javascript
    case 'on':
      turnOnLED();
      break;
    case 'off':
      turnOffLED();
      break;
    ```

    Nyní máte nastavený ukázková aplikace odpovědět na další pokyny prostřednictvím zpráv. Pokyn "na" zapne LED a pokyn "vypnout" vypne LED.

4. Otevřete soubor gulpfile.js a pak přidejte nové funkce před funkci `sendMessage`:

    ```javascript
    var buildCustomMessage = function (messageId) {
      if ((messageId & 1) && (messageId < MAX_MESSAGE_COUNT)) {
        return new Message(JSON.stringify({ command: 'on', messageId: messageId }));
      } else if (messageId < MAX_MESSAGE_COUNT) {
        return new Message(JSON.stringify({ command: 'off', messageId: messageId }));
      } else {
        return new Message(JSON.stringify({ command: 'stop', messageId: messageId }));
      }
    }
    ```

    ![aktualizace gulpfile](media/iot-hub-raspberry-pi-lessons/lesson4/updated_gulpfile.png)

5. V `sendMessage` fungovat, nahraďte řádku `var message = buildMessage(sentMessageCount);` s nový řádek v následující ukázce:

    ```javascript
    var message = buildCustomMessage(sentMessageCount);
    ```

6. Uložte všechny změny.

### <a name="425-deploy-and-run-the-sample-application"></a>4.2.5 nasazení a spustit aplikaci ukázka

Nasazení a spuštěním ukázková aplikace na vaše pí tento příkaz:

```bash
gulp
```

Měli byste vidět LED zapnutá po dobu 2 sekund a potom vypnou a zůstanou vypnuté jiného dvě sekundy. Poslední zprávu "přestat" přestal spuštění aplikace vzorku.

![Zapnutí a vypnutí](media/iot-hub-raspberry-pi-lessons/lesson4/gulp_on_and_off.png)

Blahopřejeme! Úspěšně přizpůsobíte zprávy, které jsou odeslané PI z rozbočovače IoT.

### <a name="427-summary"></a>4.2.7 souhrn

Tato část volitelné demos přizpůsobení zprávy tak, že ukázková aplikace můžete řídit zapnutým a vypnutým chování LED jiným způsobem.

