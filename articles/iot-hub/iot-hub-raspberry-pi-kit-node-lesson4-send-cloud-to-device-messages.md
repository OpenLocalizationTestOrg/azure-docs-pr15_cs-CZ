<properties
 pageTitle="Spusťte aplikaci Ukázka zprávy cloudu zařízení | Microsoft Azure"
 description="Ukázka aplikaci lekci 4 kompatibilní s vaší pí a sleduje příchozí zprávy od rozbočovače IoT. Nový úkol gulp odesílat zprávy vaše pí z rozbočovače IoT blikání LED."
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

# <a name="41-run-the-sample-application-to-receive-cloud-to-device-messages"></a>4.1 spustíte aplikaci Ukázka zprávy cloudu zařízení

V této části nasazení aplikace vzorku na malin pí 3. Ukázková aplikace sleduje příchozí zprávy od rozbočovače IoT. Můžete taky spuštění úlohy gulp ve počítače k odesílání zpráv do vaší pí z rozbočovače IoT. Po přijetí zprávy, bliká ukázková aplikace LED. Pokud budete splňovat nějaké problémy, hledání řešení [Poradce při potížích s stránky](iot-hub-raspberry-pi-kit-node-troubleshooting.md).

## <a name="411-what-you-will-do"></a>4.1.1 co bude dělat

- Připojení aplikace ukázkové k centrální IoT.
- Nasazení a spuštění aplikace vzorku.
- Odešlete zprávy z rozbočovače IoT PI blikání LED.

## <a name="412-what-you-will-learn"></a>4.1.2 co jste se naučili

- Jak sledovat příchozí zprávy od rozbočovače IoT.
- Jak odešlete cloudu zařízení zprávy z rozbočovače IoT vaše pí. 

## <a name="413-what-do-you-need"></a>4.1.3 co je potřeba

- 3 pí malin, která je nastavena pro použití. Pokud chcete zjistit, jak nastavit svůj pí, přečtěte si téma [lekci 1: Začínáme s zařízení malin pí 3](iot-hub-raspberry-pi-kit-node-get-started.md)
- IoT centrální vytvořenou v Azure předplatné. Naučte se vytvářet rozbočovače IoT Azure, najdete v článku [lekci 2: vytvoření rozbočovače IoT Azure](iot-hub-raspberry-pi-kit-node-get-started.md)

## <a name="414-connect-the-sample-application-to-your-iot-hub"></a>4.1.4 připojení aplikace ukázkové k centrální IoT

1. Zkontrolujte, že jsou ve složce repo `iot-hub-node-raspberrypi-getting-started`. Otevřete aplikaci vzorku ve Visual Studiu kódu spuštěním následující příkazy:

    ```bash
    cd Lesson4
    code .
    ```

    Oznámení `app.js` souborů v `app` podsložky. `app.js` Klíčové zdrojového souboru, který obsahuje kód ke sledování příchozí zprávy od IoT centrální je soubor. `blinkLED` Funkce bliká LED.

    ![Struktura repo](media/iot-hub-raspberry-pi-lessons/lesson4/repo_structure.png)

2. Inicializace konfiguračního souboru pomocí následujících příkazů:

    ```bash
    npm install
    gulp init
    ```

    Pokud jste dokončili lekci 3 na tomto počítači, se všechny konfigurace dědí, můžete přejít ke kroku 4.1.5. Pokud jste dokončili lekci 3 na jiném počítači, budete muset nahraďte zástupný text v `config-raspberrypi.json` soubor. `config-raspberrypi.json` Soubor je v podsložce výchozí složky.

    ![Konfigurace](media/iot-hub-raspberry-pi-lessons/lesson4/config_raspberrypi.png)

- **[IP adresa nebo zařízení hostname]** nahraďte vaší pí IP address or nebo název hostitele dostanete tak, že spuštění příkazu`devdisco list --eth`
- Nahraďte **[IoT zařízení připojovací řetězec]** zařízení připojovací řetězec, který se zobrazí spuštěním příkazu `az iot hub show-connection-string --name {my hub name} --resource-group {resource group name}`.
- Nahraďte **[IoT centrální připojovací řetězec]** IoT centrální připojovací řetězec, který se zobrazí spuštěním příkazu `az iot device show-connection-string --hub {my hub name} --device-id {device id} --resource-group {resource group name}`.

## <a name="415-deploy-and-run-the-sample-application"></a>4.1.5 nasazení a spustit aplikaci ukázka

Nasazení a spuštěním ukázková aplikace na vaše pí následující příkazy:
  
```
gulp
```

Instalace nástroje úkolu spustí příkaz gulp nejdřív. Potom nasazuje ukázkové aplikaci vaší pí. Nakonec spuštění aplikace na vaše pí a samostatných úkolů na hostitelském počítači k odesílání zpráv 20 blikání vaší PI z rozbočovače IoT.

Po spuštění aplikace vzorku, spustí listening zprávy od rozbočovače IoT. Mezitím úkolu gulp pošle více zpráv "blikání" z rozbočovače IoT vaše pí. U všech přijatých zpráv blikání ukázková aplikace volá funkci blinkLED blikání LED.

Měli byste vidět LED blikající každé dvě sekundy jako gulp úkolu je odesílají 20 zprávy z rozbočovače IoT vaše pí. Poslední je zpráva "přestat", která sděluje, že aplikace přestala systém.

![Gulp](media/iot-hub-raspberry-pi-lessons/lesson4/gulp_blink.png)

## <a name="416-summary"></a>4.1.6 souhrn

Byl úspěšně odeslán zprávy od IoT rozbočovače pí blikání LED. Dál je volitelný oddíl, který se dozvíte, jak změnit zapnutým a vypnutým chování LED.

## <a name="next-steps"></a>Další kroky

[Nepovinný oddíl: Změna zapnutým a vypnutým chování LED](iot-hub-raspberry-pi-kit-node-lesson4-change-led-behavior.md)