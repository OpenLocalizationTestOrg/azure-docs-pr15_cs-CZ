<properties
    pageTitle="Začínáme s SDK IoT centrální brány | Microsoft Azure"
    description="Znázornění klíčové koncepty, že je třeba porozumět při použití SDK brány IoT Azure pomocí Windows Azure návodu IoT brány SDK."
    services="iot-hub"
    documentationCenter=""
    authors="chipalost"
    manager="timlt"
    editor=""/>

<tags
     ms.service="iot-hub"
     ms.devlang="cpp"
     ms.topic="article"
     ms.tgt_pltfrm="na"
     ms.workload="na"
     ms.date="08/25/2016"
     ms.author="andbuc"/>


# <a name="azure-iot-gateway-sdk-beta---get-started-using-windows"></a>Azure IoT SDK brány (verze beta) - seznamovat s Windows

[AZURE.INCLUDE [iot-hub-gateway-sdk-getstarted-selector](../../includes/iot-hub-gateway-sdk-getstarted-selector.md)]

## <a name="how-to-build-the-sample"></a>Postup vytvoření ukázky

Než začnete, musíte [nastavit vývojové prostředí] [ lnk-setupdevbox] pro práci s SDK v systému Windows.

1. Otevřete okno příkazového řádku **Vývojář příkazového řádku pro VS2015** .
2. Přejděte do kořenové složky v místní kopii **azure iot brány sdk** úložiště.
3. Spustit **Nástroje\\build.cmd** skriptu. Tento skript vytvoří soubor řešení Visual Studiu, vytvoří řešení a spustí testů. Řešení Visual Studio můžete najít v **Vytvoření** složky v místní kopii **azure iot brány sdk** úložiště.

## <a name="how-to-run-the-sample"></a>K tomu výběru

1. Skript **build.cmd** vytvoří složku nazvanou **Sestavit** v místní kopii úložiště. Tato složka obsahuje dva modulů používaných v tomto příkladu.

    Skript sestavení umístí **logger_hl.dll** v **Sestavit\\moduly\\protokolování\\ladění** složku a **hello_world_hl.dll** v **Sestavit\\moduly\\hello_world\\ladění** složky. Tyto cesty **modul cestu** hodnotu použijte uvedeno v souboru s nastavením JSON dole.

2. Soubor **hello_world_win.json** v **vzorky\\hello_world\\src** příklad JSON nastavení souboru pro Windows, které použijete ke spuštění vzorku je složka. Příklad nastavení JSON ukázáno v následujícím příkladu se předpokládá, klonovat úložišti IoT brány SDK kořenové jednotku **C:** . Pokud jste stáhli do jiného umístění, budete muset upravit hodnoty **modul cesta** v **vzorky\\hello_world\\src\\hello_world_win.json** souboru příslušným způsobem.

3. V části **argumenty** modulu **logger_hl** nastavte název a cestu k souboru, který bude obsahovat data protokolu hodnoty **název souboru** .

    Toto je příklad souboru JSON nastavení systému Windows, která bude zapisovat do souboru **log.txt** v kořenovém jednotku **C:** .

    ```
    {
      "modules" :
      [
        {
          "module name" : "logger_hl",
          "module path" : "C:\\azure-iot-gateway-sdk\\build\\modules\\logger\\Debug\\logger_hl.dll",
          "args" : 
          {
            "filename":"C:\\log.txt"
          }
        },
        {
          "module name" : "hello_world",
          "module path" : "C:\\azure-iot-gateway-sdk\\build\\\\modules\\hello_world\\Debug\\hello_world_hl.dll",
          "args" : null
        }
      ],
      "links" :
      [
        {
          "source": "hello_world",
          "sink": "logger_hl"
        }
      ]
    }
    ```

3. Na příkazovém řádku přejděte do kořenové složce vaší místní kopie **azure iot brány sdk** úložiště.
4. Spusťte tento příkaz:
  
  ```
  build\samples\hello_world\Debug\hello_world_sample.exe samples\hello_world\src\hello_world_win.json
  ```

[AZURE.INCLUDE [iot-hub-gateway-sdk-getstarted-code](../../includes/iot-hub-gateway-sdk-getstarted-code.md)]

<!-- Links -->
[lnk-setupdevbox]: https://github.com/Azure/azure-iot-gateway-sdk/blob/master/doc/devbox_setup.md