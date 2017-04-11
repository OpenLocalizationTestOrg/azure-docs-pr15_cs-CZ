<properties
    pageTitle="Začínáme s twins | Microsoft Azure"
    description="Tento kurz se dozvíte, jak používat twins"
    services="iot-hub"
    documentationCenter="node"
    authors="fsautomata"
    manager="timlt"
    editor=""/>

<tags
     ms.service="iot-hub"
     ms.devlang="node"
     ms.topic="article"
     ms.tgt_pltfrm="na"
     ms.workload="na"
     ms.date="09/13/2016"
     ms.author="elioda"/>

# <a name="tutorial-get-started-with-device-twins-preview"></a>Kurz: Začínáme s zařízení twins (verze preview)

[AZURE.INCLUDE [iot-hub-selector-twin-get-started](../../includes/iot-hub-selector-twin-get-started.md)]

Na konci tohoto kurzu budete mít .NET a aplikace konzoly Node.js:

* **AddTagsAndQuery.sln**, .NET aplikace chtěli při spuštění z back-end, která přidá značky a dotazy twins zařízení.
* **TwinSimulatedDevice.js**, Node.js aplikace, která napodobuje zařízení připojené k rozbočovači IoT s zařízení identit dříve vytvořili a sestav stavu připojení.

> [AZURE.NOTE] Článek [IoT centrální SDK] [ lnk-hub-sdks] obsahuje informace o různých SDK, můžete použít k vytváření zařízení a back-end aplikací.

Tento kurz je potřeba k provedení těchto věcí:

+ Microsoft Visual Studio 2015.

+ Verze Node.js 0.10.x nebo novější.

+ Účet Azure active. (Pokud nemáte účet, můžete vytvořit [bezplatný účet] [ lnk-free-trial] v jenom pár minut.)

[AZURE.INCLUDE [iot-hub-get-started-create-hub-pp](../../includes/iot-hub-get-started-create-hub-pp.md)]

## <a name="create-the-service-app"></a>Vytvoření aplikace služby

V této části vytvoříte aplikace konzoly Node.js, které umístění metadata dvojitých přidružené **myDeviceId**. Ho pak dotazů twins uložené v centru výběr zařízení umístěn v USA a sloupci vykazování mobilní připojení.

1. Ve Visual Studiu přidáte projekt Visual Basic klasické počítač s Windows do aktuálního řešení pomocí šablony **Aplikace konzoly** projektu. Zadejte název projektu **AddTagsAndQuery**.

    ![Nový projekt Visual Basic klasické počítač s Windows][img-createapp]

2. V Průzkumníku klikněte pravým tlačítkem **AddTagsAndQuery** projektu a potom klikněte na **Spravovat balíčků Nuget**.

3. V okně **Správce balíčků Nuget** zkontrolujte, zda je zaškrtnuto políčko **Zahrnout zkušební** , vyhledejte **microsoft.azure.devices**, vyberte **nainstalovat** a nainstalujte *zkušební* verzi **Microsoft.Azure.Devices** balíček a přijměte podmínky použití. Tento postup ke stažení, instalace a přidá odkaz na [Microsoft Azure IoT služby SDK] [ lnk-nuget-service-sdk] Nuget balíčku a jejich závislosti.

    ![Okno Nuget balíčku správce][img-servicenuget]

4. Přidejte následující `using` příkazy v horní části souboru **Program.cs** :

        using Microsoft.Azure.Devices;

5. Následující pole přidáte do **programu** předmětu. Nahrazení zástupného symbolu hodnoty připojovací řetězec pro centrální IoT, který jste vytvořili v předchozí části.

        static RegistryManager registryManager;
        static string connectionString = "{iot hub connection string}";

6. Přidejte následující metodu třídy **aplikace** :

        public static async Task AddTagsAndQuery()
        {
            var twin = await registryManager.GetTwinAsync("myDeviceId");
            var patch =
                @"{
                    tags: {
                        location: {
                            region: 'US',
                            plant: 'Redmond43'
                        }
                    }
                }";
            await registryManager.UpdateTwinAsync(twin.DeviceId, patch, twin.ETag);

            var query = registryManager.CreateQuery("SELECT * FROM devices WHERE tags.location.plant = 'Redmond43'", 100);
            var twinsInRedmond43 = await query.GetNextAsTwinAsync();
            Console.WriteLine("Devices in Redmond43: {0}", string.Join(", ", twinsInRedmond43.Select(t => t.DeviceId)));

            query = registryManager.CreateQuery("SELECT * FROM devices WHERE tags.location.plant = 'Redmond43' AND properties.reported.connectivity.type = 'cellular'", 100);
            var twinsInRedmond43UsingCellular = await query.GetNextAsTwinAsync();
            Console.WriteLine("Devices in Redmond43 using cellular network: {0}", string.Join(", ", twinsInRedmond43UsingCellular.Select(t => t.DeviceId)));
        }

    Třídy **RegistryManager** poskytuje všechny metody povinné chcete provést interakci s twins zařízení ze služby. Předchozí kód nejdřív inicializuje **registryManager** objekt a pak načte dvojitých **myDeviceId**a nakonec aktualizuje její značky informacemi požadovaného umístění.

    Po aktualizaci, provede dva dotazy: pouze twins zařízení zařízení najdete v podniku **Redmond43** vybere první a druhé také upravuje dotazu vyberte zařízení, které jsou také připojené prostřednictvím mobilní sítě.

    Všimněte si, že předchozího kódu, při vytváření objekt **dotazu** určuje maximální počet vrácených dokumentů. Objekt **dotaz** obsahuje **HasMoreResults** boolean vlastnost, která můžete volat metody **GetNextAsTwinAsync** tisknutím k načtení všechny výsledky. Metoda s názvem **GetNextAsJson** je k dispozici pro výsledky, které nejsou zařízení twins například výsledky agregace dotazů.

7. Nakonec přidejte následující řádky metodě **hlavní** :

        registryManager = RegistryManager.CreateFromConnectionString(connectionString);
        AddTagsAndQuery().Wait();
        Console.WriteLine("Press Enter to exit.");
        Console.ReadLine();

8. Spuštění této aplikace a byste měli vidět jednoho zařízení ve výsledcích dotazu s žádostí o všech zařízeních nachází v **Redmond43** a žádná dotazu s omezením výsledky k zařízení, které používají mobilní síť.

    ![Výsledky dotazu v okně][img-addtagapp]

V následující části Vytvoření aplikace zařízení se ohlásí informace o připojení a mění výsledků dotazu v předchozí části.

## <a name="create-the-device-app"></a>Vytvoření aplikace zařízení

V této části vytvoříte Node.js konzoly aplikace, která se připojí k centrální jako **myDeviceId**a pak aktualizujete jeho dvojitých vykázaného vlastnosti s informacemi, že je připojený prostřednictvím mobilní sítě.

1. Vytvořte novou prázdnou složku s názvem **reportconnectivity**. Ve složce **reportconnectivity** vytvoření nového souboru package.json pomocí následujícího příkazu příkazového řádku. Přijmete všechny výchozí hodnoty:

    ```
    npm init
    ```

2. Vaše příkazového řádku ve složce **reportconnectivity** spusťte tento příkaz nainstalovat **azure iot zařízení**a **azure iot zařízení mqtt** balíček:

    ```
    npm install azure-iot-device@dtpreview azure-iot-device-mqtt@dtpreview --save
    ```

3. V textovém editoru, vytvoření nového souboru **ReportConnectivity.js** ve složce **reportconnectivity** .

4. Přidejte následující kód k souboru **ReportConnectivity.js** a nahradit **{zařízení připojovací řetězec}** zástupným připojovací řetězec, který jste zkopírovali jste vytvořili identitě **myDeviceId** zařízení:

        'use strict';
        var Client = require('azure-iot-device').Client;
        var Protocol = require('azure-iot-device-mqtt').Mqtt;

        var connectionString = '{device connection string}';
        var client = Client.fromConnectionString(connectionString, Protocol);

        client.open(function(err) {
        if (err) {
            console.error('could not open IotHub client');
        }  else {
            console.log('client opened');

            client.getTwin(function(err, twin) {
            if (err) {
                console.error('could not get twin');
            } else {
                var patch = {
                    connectivity: {
                        type: 'cellular'
                    }
                };

                twin.properties.reported.update(patch, function(err) {
                    if (err) {
                        console.error('could not update twin');
                    } else {
                        console.log('twin state reported');
                        process.exit();
                    }
                });
            }
            });
        }
        });

    Objekt **klienta** poskytuje všechny metody, kterou požadujete Pokud chcete provést interakci s twins zařízení ze zařízení. Kód předchozí po inicializuje objekt **klientského** načte dvojitých **myDeviceId** a aktualizace vlastností nahlášeného s informacemi o připojení.

5. Spusťte aplikaci zařízení

        node ReportConnectivity.js

    Zobrazí se zpráva `twin state reported`.

6. Teď zařízení uvedeny informace o připojení, by se měly v obou dotazů. Spusťte aplikaci.NET **AddTagsAndQuery** znovu spustit dotazy. Tento času **myDeviceId** by se měly v obou výsledků dotazu.

    ![][img-addtagapp2]

## <a name="next-steps"></a>Další kroky
V tomto kurzu nakonfigurované rozbočovači nové IoT Azure portálu a vytvoří zařízení identit v registru identity centrální. Přidá zařízení metadata jako značky z aplikace back-end a napsali simulovaný zařízení aplikaci a informace o připojení zařízení sestavu v dvojitých zařízení. Můžete taky naučili k vytvoření dotazu tyto informace pomocí centra IoT SQL profesionálové dotazovací jazyk.

Použijte v následujících zdrojích informací se dozvíte, jak:

- odeslání telemetrie ze zařízení s [začít pracovat s IoT centrální] [ lnk-iothub-getstarted] kurzu
- Konfigurace zařízení pomocí [použít požadované vlastnosti konfigurace zařízení] v dvojitých požadované vlastnosti[ lnk-twin-how-to-configure] kurzu
- ovládací prvek zařízení interaktivně (například Zapnutí ventilace z app uživatelem řízené) s [použijte přímé metody] [ lnk-methods-tutorial] kurz.

<!-- images -->
[img-servicenuget]: media/iot-hub-csharp-node-twin-getstarted/servicesdknuget.png
[img-createapp]: media/iot-hub-csharp-node-twin-getstarted/createnetapp.png
[img-addtagapp]: media/iot-hub-csharp-node-twin-getstarted/addtagapp.png
[img-addtagapp2]: media/iot-hub-csharp-node-twin-getstarted/addtagapp2.png

<!-- links -->
[lnk-hub-sdks]: iot-hub-devguide-sdks.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-nuget-service-sdk]: https://www.nuget.org/packages/Microsoft.Azure.Devices/1.1.0-preview-004

[lnk-d2c]: iot-hub-devguide-messaging.md#device-to-cloud-messages
[lnk-methods]: iot-hub-devguide-direct-methods.md
[lnk-twins]: iot-hub-devguide-device-twins.md
[lnk-query]: iot-hub-devguide-query-language.md
[lnk-identity]: iot-hub-devguide-identity-registry.md

[lnk-iothub-getstarted]: iot-hub-node-node-getstarted.md
[lnk-methods-tutorial]: iot-hub-c2d-methods.md
[lnk-twin-how-to-configure]: iot-hub-csharp-node-twin-how-to-configure.md

[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/node-devbox-setup.md

