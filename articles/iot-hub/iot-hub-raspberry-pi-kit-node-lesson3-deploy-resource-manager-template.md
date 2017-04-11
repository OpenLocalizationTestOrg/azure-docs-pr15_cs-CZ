<properties
 pageTitle="Vytvoření Azure funkce aplikace a účet Azure úložiště | Microsoft Azure"
 description="Aplikaci Azure funkce okolním Azure IoT centrální události, zpracuje příchozích zpráv a zapíše do úložiště tabulek Azure."
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

# <a name="31-create-an-azure-function-app-and-azure-storage-account"></a>3.1 Vytvoření Azure funkce aplikace a účet Azure úložiště

[Funkce Azure](../../articles/azure-functions/functions-overview.md) je řešení pro snadné spuštění malé částí kódu, s názvem "funkce" v cloudu. Aplikace Azure funkce hostuje provádění funkce v Azure.

## <a name="311-what-will-you-do"></a>3.1.1 co bude dělat

Pomocí Správce prostředků Azure šablony můžete vytvořit aplikaci pro Azure funkce a účet Azure úložiště. Aplikaci Azure funkce okolním Azure IoT centrální události, zpracuje příchozích zpráv a zapíše do úložiště tabulek Azure. Pokud budete splňovat nějaké problémy, hledání řešení [Poradce při potížích s stránky](iot-hub-raspberry-pi-kit-node-troubleshooting.md).

## <a name="312-what-will-you-learn"></a>3.1.2 co bude jste se naučili

- Jak lze pomocí [Správce zdrojů Azure](../../articles/azure-resource-manager/resource-group-overview.md) nasazení Azure zdroje.
- Jak používat aplikaci Azure funkce zpracování IoT Centrum zpráv a zapisovat do tabulky v úložiště tabulek Azure.

## <a name="313-what-do-you-need"></a>3.1.3 co je potřeba

- Musíte byla úspěšně dokončena předchozí spolupráci: [Začínáme s malin pí 3](iot-hub-raspberry-pi-kit-node-get-started.md) a [Vytvoření rozbočovače Azure IoT](iot-hub-raspberry-pi-kit-node-get-started.md).

## <a name="314-open-the-sample-app"></a>3.1.4 Otevřete aplikaci ukázka

Otevřete ukázkový projekt ve Visual Studiu kódu spuštěním následující příkazy:

```bash
cd Lesson3
code .
```

![Struktura repo](media/iot-hub-raspberry-pi-lessons/lesson3/repo_structure.png)

- `app.js` Souborů v `app` podsložky je klíčové zdrojového souboru. Zdrojový soubor obsahuje kód a odešlete zprávu 20 časy rozbočovače IoT blink LED pro každou zprávu, kterou pošle.
- `arm-template.json` Soubor je správce prostředků Azure šablonu, která obsahuje aplikace Azure funkce a účet Azure úložiště.
- `arm-template-param.json` Soubor je konfiguračního souboru šablonou správce prostředků Azure.
- `ReceiveDeviceMessages` Podsložky obsahuje kód Node.js Azure funkce.

## <a name="315-configure-azure-resource-manager-templates-and-create-resources-in-azure"></a>3.1.5 Konfigurace Azure správce prostředků šablon a vytvoření zdrojů v Azure

Aktualizace `arm-template-param.json` soubor ve Visual Studiu kódu.

![Azure parametry šablony správce prostředků](media/iot-hub-raspberry-pi-lessons/lesson3/arm_para.png)

- Nahraďte **[vaše jméno IoT centrální]** **{mé jméno centrální}** , která jste zadali do [lekci 2](iot-hub-raspberry-pi-kit-node-lesson2-prepare-azure-iot-hub.md).
- Nahraďte **[řetězec předponu u nových zdrojů]** předponou, které chcete. Tuto předponu zajišťuje, že název zdroje globálně jedinečné pro zabránění konfliktu. Nepoužívejte přerušované čáry nebo číslo počáteční v předponu.

> [AZURE.NOTE] Nemusíte `azure_storage_connection_string` v této části. V jednoduchosti je je.

Po aktualizaci `arm-template-param.json` soubor, nasazení zdroje Azure pomocí tohoto příkazu:

```bash
az resource group deployment create --template-file-path arm-template.json --parameters-file-path arm-template-param.json -g iot-sample -n mydeployment
```

Vytvoření tyto materiály trvá asi pět minut. Vytvoření zdrojů v průběhu, můžete přesunout v další části.

## <a name="316-summary"></a>3.1.6 souhrn

Jste si vytvořili Azure funkce aplikace zpracuje IoT Centrum zpráv a účet Azure úložiště pro ukládání tyto zprávy. Můžete přesunout do další části nasadit a spustit vzorku k odesílání zpráv zařízení do cloudu na vaše pí.

## <a name="next-steps"></a>Další kroky

[3,2 spuštění aplikace vzorku k odesílání zpráv zařízení do cloudu na malin pí 3](iot-hub-raspberry-pi-kit-node-lesson3-run-azure-blink.md)

