<properties
    pageTitle="Začínáme s SDK IoT centrální brány | Microsoft Azure"
    description="Tento návod Azure IoT brány SDK používá ke znázornění klíčové koncepty, které je třeba porozumět při použití v Azure IoT brány SDK Linux."
    services="iot-hub"
    documentationCenter=""
    authors="chipalost"
    manager="timlt"
    editor=""/>

<tags
     ms.service="iot-hub"
     ms.devlang="cpp"
     ms.topic="get-started-article"
     ms.tgt_pltfrm="na"
     ms.workload="na"
     ms.date="08/25/2016"
     ms.author="andbuc"/>


# <a name="azure-iot-gateway-sdk-beta---get-started-using-linux"></a>Azure IoT SDK brány (verze beta) – Začínáme s používáním Linux

[AZURE.INCLUDE [iot-hub-gateway-sdk-getstarted-selector](../../includes/iot-hub-gateway-sdk-getstarted-selector.md)]

## <a name="how-to-build-the-sample"></a>Postup vytvoření ukázky

Než začnete, musíte [nastavit vývojové prostředí] [ lnk-setupdevbox] pro práci s SDK na Linux.

1. Otevřete prostředí.
2. Přejděte do kořenové složky v místní kopii **azure iot brány sdk** úložiště.
3. Spusťte skript **tools/build.sh** . Tento skript používá nástroj **cmake** a vytvořte složku s názvem **Vytvoření** v kořenové složce vaší místní kopie úložišti **azure iot brány sdk** generovat soubor pravidel. Skript potom vytvoří řešení a spustí testů.

> [AZURE.NOTE]  Pokaždé, když spustíte **build.sh** skript, odstranit a pak znovu vytvořit **Vytvoření** složky v kořenové složce vaší místní kopie **azure iot brány sdk** úložiště.

## <a name="how-to-run-the-sample"></a>K tomu výběru

1. Ve složce **vytvořit** místní kopii úložišti **azure iot brány sdk** generuje skript **build.sh** jeho výstup. Tato volba zahrnuje dva modulů používaných v tomto příkladu.

    Skript sestavení umístí **liblogger_hl.so** v **sestavení/moduly/protokolování/** složku a **libhello_world_hl.so** v **sestavení/moduly/hello_world/** složky. Tyto cesty **modul cestu** hodnotu použijte uvedeno v souboru s nastavením JSON dole.

2. Soubor **hello_world_lin.json** ve složce **Ukázky/hello_world/src** je příklad JSON nastavení souboru Linux použitém ke spuštění vzorku. Nastavení JSON příklad ukázáno v následujícím příkladu se předpokládá, že se spustí výběru z kořenové složky místní kopii **azure iot brány sdk** úložiště.

3. V části **argumenty** modulu **logger_hl** nastavte název a cestu k souboru, který bude obsahovat data protokolu hodnoty **název souboru** .

    Toto je příklad souboru JSON nastavení, která bude zapisovat **log.txt** do složky, kde spustíte vzorku Linux.

    ```
    {
      "modules" :
      [ 
        {
          "module name" : "logger_hl",
          "module path" : "./build/modules/logger/liblogger_hl.so",
          "args" : 
          {
            "filename":"./log.txt"
          }
        },
        {
          "module name" : "hello_world",
          "module path" : "./build/modules/hello_world/libhello_world_hl.so",
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

3. V prostředí přejděte do složky **azure iot brány sdk** .
4. Spusťte tento příkaz:
  
  ```
  ./build/samples/hello_world/hello_world_sample ./samples/hello_world/src/hello_world_lin.json
  ``` 

[AZURE.INCLUDE [iot-hub-gateway-sdk-getstarted-code](../../includes/iot-hub-gateway-sdk-getstarted-code.md)]

<!-- Links -->
[lnk-setupdevbox]: https://github.com/Azure/azure-iot-gateway-sdk/blob/master/doc/devbox_setup.md
