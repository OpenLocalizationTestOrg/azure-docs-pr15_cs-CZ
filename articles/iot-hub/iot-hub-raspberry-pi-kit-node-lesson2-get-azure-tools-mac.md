<properties
 pageTitle="Získání Azure nástroje (macOS 10.10) | Microsoft Azure"
 description="Nainstalujte macOS Python a rozhraní Azure příkazového řádku (Azure rozhraní příkazového řádku)."
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

# <a name="21-get-azure-tools-macos-1010"></a>Získání Azure nástroje 2.1 (macOS 10.10)

> [AZURE.SELECTOR]
- [Windows 7 +](iot-hub-raspberry-pi-kit-node-lesson2-get-azure-tools-win32.md)
- [Se systémem Ubuntu 16.04](iot-hub-raspberry-pi-kit-node-lesson2-get-azure-tools-ubuntu.md)
- [macOS 10.10](iot-hub-raspberry-pi-kit-node-lesson2-get-azure-tools-mac.md)

## <a name="211-what-you-will-do"></a>2.1.1 co bude dělat

Instalace Azure rozhraní příkazového řádku (Azure rozhraní příkazového řádku). Pokud budete splňovat nějaké problémy, hledání řešení [Poradce při potížích s stránky](iot-hub-raspberry-pi-kit-node-troubleshooting.md).

## <a name="212-what-you-will-learn"></a>2.1.2 co jste se naučili

- Jak nainstalovat Azure rozhraní příkazového řádku.
- Jak přidat IoT podskupinu Azure rozhraní příkazového řádku.

## <a name="213-what-you-need"></a>2.1.3 co byste měli

- Mac s připojením k Internetu
- Aktivní Azure předplatné. Pokud nemáte účet, můžete vytvořit [bezplatný účet](https://azure.microsoft.com/free/) v jenom pár minut.

## <a name="214-install-python"></a>2.1.4 instalace Python

Přestože macOS je součástí Python 2.7 mimo pole, doporučujeme nainstalovat Python prostřednictvím Homebrew. V tématu [Instalace Python na macOS](http://docs.python-guide.org/en/latest/starting/install/osx/).

Nainstalujte Python a pip spuštěním následujícího příkazu:

```bash
brew install python
```

## <a name="215-install-the-azure-cli"></a>2.1.5 instalace Azure rozhraní příkazového řádku

Rozhraní příkazového řádku Azure poskytuje s více platformami příkazového řádku prostředí Azure, která vám umožní pracovat přímo z příkazového řádku trvat a přidávání a používání zdrojů. 

Pokud chcete nainstalovat nejnovější rozhraní příkazového řádku Azure, postupujte takto:

1. Spusťte následující příkazy v okně terminálu. Může trvat pět minut nainstalovat Azure rozhraní příkazového řádku.

    ```bash
    pip install azure-cli-core==0.1.0b7 azure-cli-vm==0.1.0b7 azure-cli-storage==0.1.0b7 azure-cli-role==0.1.0b7 azure-cli-resource==0.1.0b7 azure-cli-profile==0.1.0b7 azure-cli-network==0.1.0b7 azure-cli-iot==0.1.0b7 azure-cli-feedback==0.1.0b7 azure-cli-configure==0.1.0b7 azure-cli-component==0.1.0b7 azure-cli==0.1.0b7
    ```

2. Instalace ověřte spuštěním následujícího příkazu:

    ```bash
    az iot -h
    ```
  
Pokud instalace proběhne úspěšně byste měli vidět následující výstup.

![AZ iot -h](media/iot-hub-raspberry-pi-lessons/lesson2/az_iot_help_osx.png)

## <a name="215-summary"></a>2.1.5 souhrn

Instalaci Azure rozhraní příkazového řádku. Pokračujte v další části Vytvoření vaši identitu Azure IoT centrální a zařízení pomocí rozhraní příkazového řádku Azure.

## <a name="next-steps"></a>Další kroky

[2.2 vytvořit rozbočovače IoT a zaregistrovat malin pí 3](iot-hub-raspberry-pi-kit-node-lesson2-prepare-azure-iot-hub.md)
