<properties
 pageTitle="Získání Azure nástroje (Windows 7 +) | Microsoft Azure"
 description="Nainstalujte Python a rozhraní Azure příkazového řádku (Azure rozhraní příkazového řádku) v systému Windows 7 a novějších verzích."
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

# <a name="21-get-azure-tools-windows-7-"></a>2.1 získat Azure nástroje (Windows 7 +)

> [AZURE.SELECTOR]
- [Windows 7 +](iot-hub-raspberry-pi-kit-node-lesson2-get-azure-tools-win32.md)
- [Se systémem Ubuntu 16.04](iot-hub-raspberry-pi-kit-node-lesson2-get-azure-tools-ubuntu.md)
- [macOS 10.10](iot-hub-raspberry-pi-kit-node-lesson2-get-azure-tools-mac.md)

## <a name="211-what-you-will-do"></a>2.1.1 co bude dělat

Instalace Python a příkazového řádku Azure rozhraní (Azure rozhraní příkazového řádku). Pokud budete splňovat nějaké problémy, hledání řešení [Poradce při potížích s stránky](iot-hub-raspberry-pi-kit-node-troubleshooting.md).

## <a name="212-what-you-will-learn"></a>2.1.2 co jste se naučili

- Jak nainstalovat Python.
- Jak nainstalovat Azure rozhraní příkazového řádku.

## <a name="213-what-you-need"></a>2.1.3 co byste měli

- Počítače s Windows s připojením k Internetu
- Aktivní Azure předplatné. Pokud nemáte účet, můžete vytvořit [bezplatný účet](https://azure.microsoft.com/free/) v jenom pár minut.

## <a name="214-install-python"></a>2.1.4 instalace Python

Instalace Python na počítači s Windows. Můžete nainstalovat Python 2.7, 3.4 nebo 3.5. Tento kurz se podle Python 2.7. Pokud jste si už nainstalovali Python, přejděte na oddíl 2.1.5.

[Jak Python pro Windows](https://www.python.org/downloads/)

Bude potřeba přidat cestu složky nainstalovanou python.exe a pip.exe systému `PATH` proměnné. Ve výchozím nastavení je nainstalovaný python.exe v `C:\Python27` a pip.exe je nainstalovaný v `C:\Python27\Scripts`.

## <a name="215-install-the-azure-cli"></a>2.1.5 instalace Azure rozhraní příkazového řádku

Rozhraní příkazového řádku Azure poskytuje s více platformami příkazového řádku prostředí Azure, která vám umožní pracovat přímo z příkazového řádku trvat a přidávání a používání zdrojů.

Pokud chcete nainstalovat Azure rozhraní příkazového řádku, postupujte takto:

1. Jako správce otevřete okno příkazového řádku.
2. Spusťte následující příkazy:

    ```bash
    pip install azure-cli-core==0.1.0b7 azure-cli-vm==0.1.0b7 azure-cli-storage==0.1.0b7 azure-cli-role==0.1.0b7 azure-cli-resource==0.1.0b7 azure-cli-profile==0.1.0b7 azure-cli-network==0.1.0b7 azure-cli-iot==0.1.0b7 azure-cli-feedback==0.1.0b7 azure-cli-configure==0.1.0b7 azure-cli-component==0.1.0b7 azure-cli==0.1.0b7
    ```
3. Instalace ověřte spuštěním následujícího příkazu:

    ```bash
    az iot -h
    ```

Pokud je instalace úspěšné, zobrazí se následující výstup.

![AZ iot -h](media/iot-hub-raspberry-pi-lessons/lesson2/az_iot_help_win.png)

## <a name="216-summary"></a>2.1.6 souhrn

Instalaci Azure rozhraní příkazového řádku. Pokračujte v další části Vytvoření vaši identitu Azure IoT centrální a zařízení pomocí rozhraní příkazového řádku Azure.

## <a name="next-steps"></a>Další kroky

[2.2 vytvořit rozbočovače IoT a zaregistrovat malin pí 3](iot-hub-raspberry-pi-kit-node-lesson2-prepare-azure-iot-hub.md)
