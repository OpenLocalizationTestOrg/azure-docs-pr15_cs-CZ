<properties
 pageTitle="Získání nástrojů (Windows 7 +) | Microsoft Azure"
 description="Stáhnout a nainstalovat potřebné nástroje a software pro první ukázka aplikace pro váš pí ve Windows 7 a novější verze."
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

# <a name="12-get-the-tools-windows-7-"></a>1.2 získat nástrojů (Windows 7 +) 

> [AZURE.SELECTOR]
- [Windows 7 +](iot-hub-raspberry-pi-kit-node-lesson1-get-the-tools-win32.md)
- [Se systémem Ubuntu 16.04](iot-hub-raspberry-pi-kit-node-lesson1-get-the-tools-ubuntu.md)
- [macOS 10.10](iot-hub-raspberry-pi-kit-node-lesson1-get-the-tools-mac.md)

## <a name="121-what-you-will-do"></a>1.2.1 co bude dělat

Stáhněte si vývojového nástroje a software pro první ukázka aplikace pro malin pí 3. Pokud budete splňovat nějaké problémy, hledání řešení [Poradce při potížích s stránky](iot-hub-raspberry-pi-kit-node-troubleshooting.md).

## <a name="122-what-you-will-learn"></a>1.2.2 co jste se naučili
- Jak nainstalovat libovolná a Node.js
  - [Libovolná](https://git-scm.com) je systému správy verzí otevřít zdroj rozdělením. Ukázka aplikace pro tento výuky uložený na libovolná.
  - [Node.js](https://nodejs.org/en/) je modul runtime JavaScript s formátovaným balíčku ekosystému.
- Jak používat NPM nainstalovat další Node.js vývojového nástroje.
  - Minimální požadavky na verzi aplikace Node.js je 4,5 l.
  - [NPM](https://www.npmjs.com) je jednou z nadřízených balíček pro Node.js.

## <a name="123-what-you-need"></a>1.2.3 co byste měli

- Připojení k Internetu stahovat vývojového nástroje a software
- Počítač s Windows

## <a name="124-install-git-and-nodejs"></a>1.2.4 instalace libovolná a Node.js

Klikněte na odkazy dole si stáhněte a nainstalujte libovolná a Node.js l pro Windows.

- [Jak libovolná pro Windows](https://git-scm.com/download/win/)
- [Jak Node.js l pro Windows](https://nodejs.org/en/)

## <a name="125-install-additional-nodejs-development-tools"></a>1.2.5 instalace další Node.js vývojového nástroje

[Gulp.js](http://gulpjs.com) použít k automatizaci nasazení aplikace ukázkové vaší PI. Také pomocí [zařízení zjišťování-rozhraní do příkazového řádku.](https://github.com/Azure/device-discovery-cli) načítat sítě informace o IoT zařízení.

Spusťte příkazový řádek jako správce. Instalace `gulp` a `device-discovery-cli` spuštěním následujícího příkazu:

```bash
npm install -g device-discovery-cli gulp
```
    
Pokud k problémům s instalací Node.js a tyto další vývojového nástroje Node.js ve vašem počítači, přečtěte si článek [Poradce při potížích s Průvodce](iot-hub-raspberry-pi-kit-node-troubleshooting.md) pro řešení běžných problémů.

## <a name="126-install-visual-studio-code"></a>1.2.6 instalace aplikace Visual Studio kód

[Stažení](https://code.visualstudio.com/docs/setup/windows) a instalace aplikace Visual Studio kód. Visual Studio kód je editoru lightweight ale výkonné zdrojového kódu pro systém Windows, Linux a macOS. Pomocí tohoto editoru dál v tomto kurzu upravit kód vzorku.

## <a name="127-summary"></a>1.2.7 souhrn

Instalaci požadované vývojového nástroje a software pro první ukázková aplikace. V následující části Vytvoření, nasazení a spusťte ukázková aplikace na vaše pí.

## <a name="next-steps"></a>Další kroky

[1.3 vytvoření a nasazení aplikace ukázkové blikání](iot-hub-raspberry-pi-kit-node-lesson1-deploy-blink-app.md)
