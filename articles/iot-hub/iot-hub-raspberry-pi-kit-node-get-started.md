<properties
 pageTitle="Začínáme s Malinová pí 3 | Microsoft Azure"
 description="Začínáme s malin pí 3, vytvoříte rozbočovače IoT Azure a připojení k centru IoT vaše pí"
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

# <a name="get-started-with-raspberry-pi-3"></a>Začínáme s Malinová pí 3

V tomto kurzu zahájíte seznámení se základy práce s malin pí 3 tohoto pracovního Raspbian. Potom informace o bezproblémové připojení ke cloudu s [Azure IoT centrální](iot-hub-what-is-iot-hub.md)svých zařízeních. Pro Windows 10 IoT Core výběry Navštěvujte blog o [windowsondevices.com](http://www.windowsondevices.com/).

## <a name="lesson-1-configure-your-device"></a>Lekce 1: Konfigurace zařízení

![Diagram E2E Lekce 1](media/iot-hub-raspberry-pi-lessons/e2e-lesson1.png)

V této lekci konfigurace malin pí 3 zařízení s operačním systémem, nastavte si vývojové prostředí a nasazení aplikace pí.

### <a name="configure-your-device"></a>Konfigurace zařízení

Konfigurace vaší malin pí 3 pro první použití a nainstalujte Raspbian OS bezplatné operačním systémem, která je optimalizovaná pro hardware malin pí.

*Předpokládaná doba potřebná k dokončení: 30 minut* 

[Přejděte do svého zařízení konfigurovat](iot-hub-raspberry-pi-kit-node-lesson1-configure-your-device.md)

### <a name="get-the-tools"></a>Získání nástrojů
Stáhněte si nástroje a software sestavovat a nasazovat první aplikace pro malin pí 3.

*Předpokládaná doba potřebná k dokončení: 20 minut* 

[Přejděte na "Získat potřebné nástroje"](iot-hub-raspberry-pi-kit-node-lesson1-get-the-tools-win32.md)

### <a name="create-and-deploy-the-blink-application"></a>Vytvoření a nasazení aplikace blikání

Klonovat ukázková Node.js aplikace z Github a gulp nasazení tuto aplikaci k vývěsce malin pí 3. Tato ukázková aplikace bliká LED připojení na vývěsku každé dvě sekundy.

*Předpokládaná doba potřebná k dokončení: 5 minut* 

[Přejděte na "Vytvoření a nasazení aplikace blikání"](iot-hub-raspberry-pi-kit-node-lesson1-deploy-blink-app.md)

## <a name="lesson-2-create-your-iot-hub"></a>Lekce 2: Vytvoření rozbočovače IoT

![Diagram E2E Lekce 2](media/iot-hub-raspberry-pi-lessons/e2e-lesson2.png)

V této lekci vytvořit bezplatný účet Azure zřízení rozbočovače Azure IoT a vytvořte první zařízení v centrální Azure IoT.

Dokončení 1 výuky před zahájením tohoto lekce.

### <a name="get-the-azure-tools"></a>Získání Azure nástroje

Instalace Azure rozhraní příkazového řádku (Azure rozhraní příkazového řádku).

*Předpokládaná doba potřebná k dokončení: 10 minut* 

[Přejděte na získat Azure nástroje](iot-hub-raspberry-pi-kit-node-lesson2-get-azure-tools-win32.md)

### <a name="create-your-iot-hub-and-register-your-raspberry-pi-3"></a>Vytvořit centrum IoT a zaregistrovat malin pí 3

Vytvořte skupina zdroje, zřízení první rozbočovač Azure IoT a přidejte první zařízení k rozbočovači IoT Azure pomocí rozhraní příkazového řádku Azure. 

*Předpokládaná doba potřebná k dokončení: 10 minut* 

[Přejděte na "Vytvořit rozbočovače IoT a zaregistrovat malin pí 3"](iot-hub-raspberry-pi-kit-node-lesson2-prepare-azure-iot-hub.md)


## <a name="lesson-3-send-device-to-cloud-messages"></a>Lekce 3: Odeslání zprávy zařízení do cloudu

![Diagram E2E Lekce 3](media/iot-hub-raspberry-pi-lessons/e2e-lesson3.png)

V této lekci odesílání zpráv z vaší pí k rozbočovači IoT. Lze také vytvořit aplikaci Azure funkci, která vyzvedne příchozí zprávy od rozbočovače IoT a zapíše do úložiště tabulek Azure.

Před zahájením tohoto výuky dokončete lekcí 1 a 2.

### <a name="create-an-azure-function-app-and-azure-storage-account"></a>Vytvoření Azure funkce aplikace a účet Azure úložiště

Pomocí Správce prostředků Azure šablony můžete vytvořit aplikaci pro Azure funkce a účet Azure úložiště.

*Předpokládaná doba potřebná k dokončení: 10 minut* 

[Přejděte na "Vytvoření Azure funkce aplikace a úložišti Azure účtu"](iot-hub-raspberry-pi-kit-node-lesson3-deploy-resource-manager-template.md)

### <a name="run-sample-application-to-send-device-to-cloud-messages"></a>Spuštění aplikace vzorku k odesílání zpráv zařízení do cloudu

Nasazení a spusťte ukázku aplikace do zařízení malin pí 3 IoT centrální zasílání zpráv.

*Předpokládaná doba potřebná k dokončení: 10 minut* 

[Přejděte na "spuštění aplikace vzorku k odesílání zpráv zařízení do cloudu.](iot-hub-raspberry-pi-kit-node-lesson3-run-azure-blink.md)

### <a name="read-messages-persisted-in-azure-storage"></a>Čtení zpráv zachován v úložišti Azure
Sledujte zprávy zařízení do cloudu, jak zápisu úložišti Azure.

*Předpokládaná doba potřebná k dokončení: 5 minut* 

[Přejděte na "číst zprávy zachován v úložišti Azure"](iot-hub-raspberry-pi-kit-node-lesson3-read-table-storage.md)


## <a name="lesson-4-send-cloud-to-device-messages"></a>Lekce 4: Posílání zpráv cloudu zařízení

![Diagram E2E Lekce 4](media/iot-hub-raspberry-pi-lessons/e2e-lesson4.png)

V této lekci demos postup odesílat zprávy z Azure IoT centrální malin pí 3. Zprávy řídit zapnutým a vypnutým chování LED, kterému je připojen váš pí. Ukázková aplikace je připraven můžete dosáhnout tohoto úkolu.

Dokončení lekcí 1, 2 a 3 před zahájením tohoto lekce.

### <a name="run-the-sample-application-to-receive-cloud-to-device-messages"></a>Spusťte aplikaci Ukázka zprávy cloudu zařízení

Ukázka aplikaci lekci 4 kompatibilní s vaší pí a sleduje příchozí zprávy od rozbočovače IoT. Nový úkol gulp odesílat zprávy vaše pí z rozbočovače IoT blikání LED.

*Předpokládaná doba potřebná k dokončení: 10 minut* 

[Přejděte na "Spustit aplikaci Ukázka zprávy cloudu zařízení"](iot-hub-raspberry-pi-kit-node-lesson4-send-cloud-to-device-messages.md)

### <a name="optional-section-change-the-on-and-off-behavior-of-the-led"></a>Nepovinný oddíl: Změna zapnutým a vypnutým chování LED

Přizpůsobení zprávy, které chcete změnit LED zapínat a vypínat chování.

*Předpokládaná doba potřebná k dokončení: 10 minut* 

[Přejděte na "nepovinný oddíl: Změna zapnutým a vypnutým chování LED"](iot-hub-raspberry-pi-kit-node-lesson4-change-led-behavior.md)


## <a name="troubleshooting"></a>Řešení potíží

Pokud budete splňovat jakékoli potíže při během lekcí můžete požádat o řešení na této stránce.

[Přejděte na "Poradce při potížích.](iot-hub-raspberry-pi-kit-node-troubleshooting.md)
