<properties
 pageTitle="Poradce při potížích s | Microsoft Azure"
 description="Poradce při potížích s stránky pro malin pí Node.js prostředí"
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

# <a name="troubleshooting"></a>Řešení potíží

## <a name="hardware-issues"></a>Problémy s hardwarem

### <a name="the-application-runs-well-but-the-led-is-not-blinking"></a>Spuštění aplikace a LED není blikající, ale

Tento problém souvisí vždy připojení okruh hardwaru. Pomocí následujících kroků k identifikaci problémů.

1. Zaškrtněte, pokud se rozhodnete správné **GPIO** na vaší vývěsky. Dva porty v této lekci by měl být **GPIO zem (6 PIN kód)** a **GPIO 04 (PIN kód 7)**.
2. Kontrola správnosti polarita vaší Indikátor. Delší nohy by měl obsahovat **kladné**, anod pin.
3. Používání **3,3 přidržet** a **zem připnout** na Malinová pí 3. Vaše pí považovat za Power řadiče domény. Zjistit, jestli LED funguje správně.

![Indikátor specifikace](media/iot-hub-raspberry-pi-lessons/troubleshooting/led_spec.png)

### <a name="other-hardware-issues"></a>Jiné problémy s hardwarem

Informace o řešení běžných problémů v pí 3 malin odkaz na [stránku oficiální Poradce při potížích](http://elinux.org/R-Pi_Troubleshooting).

## <a name="nodejs-package-issues"></a>Node.js balíčku problémy

### <a name="no-response-during-gulp-tasks"></a>Žádná odpověď během gulp úkolů

Pokud při používání narazíte na problémy se spuštěním gulp úkoly, můžete přidat `--verbose` možnosti ladění. Zkuste ukončit aktuální gulp úkoly s `Ctrl + C` a spusťte následující příkaz v okně konzoly zobrazíte ladění zprávy. Může se zobrazit podrobné chybové zprávy tisknutých v konzole výstupu. 

```bash
gulp --verbose
```

### <a name="device-discovery-issues"></a>Zjišťování problémů zařízení

Nápovědu k řešení běžných problémů s `devdisco` příkaz, zkontrolujte [Soubor Readme pro](https://github.com/Azure/device-discovery-cli/blob/develop/readme.md).

### <a name="other-npm-issues"></a>Jiné problémy NPM

Snažte udržovat NPM balíčku s Tento příkaz:

```bash
npm install -g npm
```

Pokud problém přetrvává, komentář na konci tohoto článku nebo vytváření Github problém v naší [Ukázkové úložiště](https://github.com/Azure-Samples/iot-hub-node-raspberrypi-getting-started)

## <a name="remote-debugging"></a>Vzdálené ladění

### <a name="run-the-sample-application-in-debug-mode"></a>Spusťte aplikaci vzorku v režimu ladění

```bash
gulp run --debug
```

Pokud modul ladění hotovou, byste měli vidět ```Debugger listening on port 5858``` do konzoly výstupu.

### <a name="configure-vs-code-to-connect-to-the-remote-device"></a>Konfigurace kódem a připojení ke vzdálené zařízení

Otevřete panel **ladění** na levé straně.

Tlačítko zelené **Spustit ladění** (F5). Kód a otevře **launch.json** soubor, který byste potřebovali aktualizovat.

Aktualizujte soubor **launch.json** následující obsah. Nahrazení `[device hostname or IP address]` skutečné zařízení IP adresu nebo název hostitele.   

```json
{
    "version": "0.2.0",
    "configurations": [
        {
            "name": "Attach",
            "type": "node",
            "request": "attach",
            "port": 5858,
            "address": "[device hostname or IP address]",
            "restart": false,
            "sourceMaps": false,
            "outDir": null,
            "localRoot": "${workspaceRoot}",
            "remoteRoot": null
        }
    ]
}
```

![Konfigurace vzdálené ladění](media/iot-hub-raspberry-pi-lessons/troubleshooting/remote_debugging_configuration.png)

### <a name="attach-to-the-remote-application"></a>Připojení ke vzdálené aplikaci

Kliknutím na zelenou **Spustit ladění** (F5) tlačítko ladění spustit. 

Přečtěte si další informace o ladění [JavaScript v kódu a](https://code.visualstudio.com/docs/languages/javascript#_debugging) .

![Vzdálené ladění interaktivní](media/iot-hub-raspberry-pi-lessons/troubleshooting/remote_debugging_interactive.png)

## <a name="azure-cli-issues"></a>Problémy s Azure rozhraní příkazového řádku

Azure rozhraní příkazového řádku se náhled Tvůrce dotazů. Můžete odkázat na [Náhled instalace Průvodce](https://github.com/Azure/azure-cli/blob/master/doc/preview_install_guide.md) do hledání řešení.

Pokud narazíte na všechny chyby pomocí nástroje, souborů v části **problémy** GitHub repo [problém](https://github.com/Azure/azure-cli/issues) .

Nápovědu k řešení běžných potíží zkontrolujte [Soubor Readme pro](https://github.com/Azure/azure-cli/blob/master/README.rst).

## <a name="python-installation-issues"></a>Problémy při instalaci Python

### <a name="legacy-installation-issues-macos"></a>Problémy při instalaci starších verzí (macOS)

Při instalaci **pip**oprávnění se chyba při starší balíčků, které jsou instalovány spolu **SH** oprávnění. Tato situace příčinou předchozí instalace Python pomocí brew (macOS) není úplně odinstalovat. Některé **pip** balíčky z předchozí instalace vytvořených kořenový adresář, který způsobuje chybu oprávnění. Řešení je odebrat balíčky nainstalovaný kořenovou. K provedení tohoto úkolu použijte podle těchto kroků:

1. Přejděte na /usr/local/lib/python2.7/site-packages
2. Seznam balíčků vytvořit kořenovou:`ls -l | grep root`
3. Odinstalujte balíčků krok 2:`sudo rm -rf {package name}`
4. Přeinstalace Python.

## <a name="azure-iot-hub-issues"></a>Azure IoT centrální problémy

Pokud jste úspěšně zřízení Azure IoT centrální s `azure-cli`, a musíte nástroj pro správu zařízení připojení k centrální IoT, vyzkoušejte následující nástroje:

### <a name="device-explorer"></a>Průzkumník zařízení

[Průzkumník zařízení](https://github.com/Azure/azure-iot-sdks/blob/master/tools/DeviceExplorer/doc/how_to_use_device_explorer.md) běží na místním počítači Windows a připojí se k centrální IoT v Azure. Komunikaci se následující [IoT centrální koncové body](iot-hub-devguide.md):

- *Správa identit zařízení* můžete vytvořit a spravovat zařízení registrovaný u rozbočovače IoT.
- *Přijímání zařízení cloudu* , která umožňují sledovat zprávy odeslané z vašeho zařízení rozbočovače IoT.
- *Odeslání cloudu zařízení* umožňují zpráv zařízení rozbočovače IoT.

Konfigurace vaší `IoT hub connection string` v tomto nástroji používat všechny funkce.

### <a name="iot-hub-explorer"></a>Rozbočovač IoT Průzkumníka

[Rozbočovač IoT Explorer](https://github.com/Azure/azure-iot-sdks/blob/master/tools/iothub-explorer/readme.md) je ukázkový s více platformami rozhraní příkazového řádku nástroj ke správě klientů zařízení. Nástroj umožňuje Správa zařízení v registru identity, sledování zpráv zařízení do cloudu a poslat příkazy cloudu zařízení.

A nainstalujte nejnovější verzi (předběžné) nástroje iothub Průzkumníka, spusťte tento příkaz v prostředí příkazového řádku:

```
npm install -g iothub-explorer@latest
```

Chcete-li získat další nápovědu k všechny příkazy iothub explorer a jejich parametry můžete tento příkaz:

```bash
iothub-explorer help
```

### <a name="use-azure-portal-to-manage-your-resources"></a>Použití Azure portál ke správě svých prostředcích

Ve všech těchto lekcí úplné rozhraní příkazového řádku prostředí slouží k vytváření a správa všechny Azure zdroje. Můžete taky pomocí [Azure portál](../azure-portal-overview.md) nápovědy poskytování, Správa a ladění Azure zdroje.

## <a name="azure-storage-issues"></a>Problémy s Azure úložiště

[Microsoft Azure úložiště Explorer (verze Preview)](http://storageexplorer.com) je samostatné aplikace od Microsoftu, který umožňuje pracovat s daty Azure úložiště v systému Windows, macOS a Linux. Tento nástroj umožňuje připojit se k tabulce a zobrazení dat v něm. Tento nástroj slouží k řešení problémů Azure úložiště.
