<properties
    pageTitle="Azure IoT základem C# Začínáme | Microsoft Azure"
    description="Azure centrální IoT s C# začíná začali kurz. Pomocí Azure IoT centrální a C# SDK Microsoft Azure IoT implementovat řešení pro Internet věci."
    services="iot-hub"
    documentationCenter=".net"
    authors="dominicbetts"
    manager="timlt"
    editor=""/>

<tags
     ms.service="iot-hub"
     ms.devlang="dotnet"
     ms.topic="hero-article"
     ms.tgt_pltfrm="na"
     ms.workload="na"
     ms.date="09/12/2016"
     ms.author="dobett"/>

# <a name="get-started-with-azure-iot-hub-for-net"></a>Začínáme s Azure IoT Centrum pro .NET

[AZURE.INCLUDE [iot-hub-selector-get-started](../../includes/iot-hub-selector-get-started.md)]

Na konci tohoto kurzu máte tři aplikace konzoly Windows:

* **CreateDeviceIdentity**, která vytvoří zařízení identit a klíč přidružené zabezpečení připojení simulovaný zařízení.
* **ReadDeviceToCloudMessages**, které zobrazuje telemetrie odeslaný simulovaný zařízení.
* **SimulatedDevice**, který připojí k rozbočovači IoT se zařízení identit dříve vytvořili a odešle zprávu telemetrie za sekundu pomocí protokolu AMQP.

> [AZURE.NOTE] Informace o různých SDK, které můžete použít k vytvoření obou spouštět aplikace na zařízeních a vašeho řešení back-end najdete v tématu [IoT centrální SDK][lnk-hub-sdks].

Tento kurz, je potřeba k provedení těchto věcí:

+ Microsoft Visual Studio 2015.

+ Účet Azure active. (Pokud nemáte účet, můžete vytvořit [bezplatný účet] [ lnk-free-trial] v jenom pár minut.)

[AZURE.INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

Teď jste vytvořili rozbočovače IoT a máte hostname a připojovací řetězec, které musíte udělat zbytku tohoto kurzu.

## <a name="create-a-device-identity"></a>Vytvoření zařízení identity

V této části vytvoříte konzoly aplikace pro Windows vytvoří zařízení identit v registru identity rozbočovače IoT. Zařízení nemůže připojit k centrální IoT, pokud má položka registru identity zařízení. Další informace naleznete v části "Zařízení identity registru" [IoT centrální Developer Guide][lnk-devguide-identity]. Při spuštění této aplikace konzoly vygeneruje jedinečný zařízení ID a klávesy, která zařízení můžete použít k identifikaci k zasílání zpráv zařízení do cloudu k rozbočovači IoT.

1. Ve Visual Studiu přidáte projekt Visual Basic klasické počítač s Windows do aktuálního řešení pomocí šablony **Aplikace konzoly** projektu. Nastavit, že je verze .NET Framework 4.5.1 nebo novější. Zadejte název projektu **CreateDeviceIdentity**.

    ![Nový projekt Visual Basic klasické počítač s Windows][10]

2. V Průzkumníku klikněte pravým tlačítkem **CreateDeviceIdentity** projektu a potom klikněte na **Spravovat balíčků Nuget**.

3. V okně **Správce balíčků Nuget** vyberte **Procházet**, vyhledejte **microsoft.azure.devices**, vyberte **nainstalovat** a nainstalujte balíček **Microsoft.Azure.Devices** a přijměte podmínky použití. Tento postup ke stažení, instalace a přidá odkaz na [Microsoft Azure IoT služby SDK] [ lnk-nuget-service-sdk] Nuget balíčku a jejich závislosti.

    ![Okno Nuget balíčku správce][11]

4. Přidejte následující `using` příkazy v horní části souboru **Program.cs** :

        using Microsoft.Azure.Devices;
        using Microsoft.Azure.Devices.Common.Exceptions;

5. Následující pole přidáte do **programu** předmětu. Nahrazení zástupného symbolu hodnoty připojovací řetězec pro centrální IoT, který jste vytvořili v předchozí části.

        static RegistryManager registryManager;
        static string connectionString = "{iot hub connection string}";

6. Přidejte následující metodu třídy **aplikace** :

        private static async Task AddDeviceAsync()
        {
            string deviceId = "myFirstDevice";
            Device device;
            try
            {
                device = await registryManager.AddDeviceAsync(new Device(deviceId));
            }
            catch (DeviceAlreadyExistsException)
            {
                device = await registryManager.GetDeviceAsync(deviceId);
            }
            Console.WriteLine("Generated device key: {0}", device.Authentication.SymmetricKey.PrimaryKey);
        }

    Tento způsob vytvoří zařízení identit s ID **myFirstDevice**. (Pokud tento ID zařízení již existuje v registru, kód jednoduše načte existující informací o zařízení.) Aplikace zobrazí primární klíč pro tuto identitu. Použijete tento klíč simulovaný zařízení k připojení k centrální IoT.

7. Nakonec přidejte následující řádky metody **hlavní** :

        registryManager = RegistryManager.CreateFromConnectionString(connectionString);
        AddDeviceAsync().Wait();
        Console.ReadLine();

8. Spuštění této aplikace a poznamenejte si klávesu zařízení.

    ![Klíč zařízení generovaný aplikací][12]

> [AZURE.NOTE] Registru identity IoT centrální pouze ukládá zařízení identit povolit zabezpečený přístup k centru. Slouží k ukládání ID zařízení a klávesy se šipkami použít jako zabezpečovací přihlašovací údaje a povolit nebo zakázat příznak, který slouží k zakázání přístupu pro jednotlivé zařízení. Pokud aplikace potřebuje k ukládání ostatních metadatech specifické pro zařízení, používejte úložiště specifické pro aplikaci. Další informace najdete v příručce [IoT centrální vývojář][lnk-devguide-identity].

## <a name="receive-device-to-cloud-messages"></a>Zprávy zařízení do cloudu

V této části Vytvoření aplikace konzoly Windows, který bude číst zprávy zařízení do cloudu z centrální IoT. Rozbočovači IoT zpřístupňuje [Azure události rozbočovače][lnk-event-hubs-overview]-kompatibilní koncový bod umožňující přečtených zprávách: zařízení do cloudu. Jednodušší jednoduché, vytvoří tento kurz základní reader, který není vhodné pro nasazení vysoký výkon. Informace o zpracování zpráv zařízení do cloudu ve velkém měřítku, najdete v článku [zprávy zařízení do cloudu proces] [ lnk-process-d2c-tutorial] kurz. Další informace o tom, jak zpracování zpráv z rozbočovače události najdete v článku [Začínáme s události rozbočovače] [ lnk-eventhubs-tutorial] kurz. (Tento kurz platí pro koncové body kompatibilní s prohlížečem IoT centrální události centrální.)

> [AZURE.NOTE] Kompatibilní s prohlížečem události centrální koncový bod pro čtení zpráv zařízení do cloudu vždy používá protokol AMQP.

1. Ve Visual Studiu přidáte projekt Visual Basic klasické počítač s Windows do aktuálního řešení pomocí šablony **Aplikace konzoly** projektu. Nastavit, že je verze .NET Framework 4.5.1 nebo novější. Zadejte název projektu **ReadDeviceToCloudMessages**.

    ![Nový projekt Visual Basic klasické počítač s Windows][10]

2. V Průzkumníku klikněte pravým tlačítkem **ReadDeviceToCloudMessages** projektu a potom klikněte na **Spravovat balíčků Nuget**.

3. V okně **Správce balíčků Nuget** vyhledejte **WindowsAzure.ServiceBus**, vyberte **nainstalovat**a přijměte podmínky použití. Tento postup ke stažení, instalace a přidá odkaz na [Bus služby Azure][lnk-servicebus-nuget], s jeho závislosti. Tento balíček umožňuje aplikaci pro připojení k kompatibilní s prohlížečem události centrální koncového bodu na vaše Centrum IoT.

4. Přidejte následující `using` příkazy v horní části souboru **Program.cs** :

        using Microsoft.ServiceBus.Messaging;
        using System.Threading;

5. Následující pole přidáte do **programu** předmětu. Nahrazení zástupného symbolu hodnoty připojovací řetězec pro centrální IoT, kterou jste vytvořili v části "Vytvoření rozbočovači IoT".

        static string connectionString = "{iothub connection string}";
        static string iotHubD2cEndpoint = "messages/events";
        static EventHubClient eventHubClient;

6. Přidejte následující metodu třídy **aplikace** :

        private static async Task ReceiveMessagesFromDeviceAsync(string partition, CancellationToken ct)
        {
            var eventHubReceiver = eventHubClient.GetDefaultConsumerGroup().CreateReceiver(partition, DateTime.UtcNow);
            while (true)
            {
                if (ct.IsCancellationRequested) break;
                EventData eventData = await eventHubReceiver.ReceiveAsync();
                if (eventData == null) continue;

                string data = Encoding.UTF8.GetString(eventData.GetBytes());
                Console.WriteLine("Message received. Partition: {0} Data: '{1}'", partition, data);
            }
        }

    Tento způsob používá instanci **EventHubReceiver** přijímat zprávy od všech IoT Centrum zařízení na cloudu přijímání oddíly. Všimněte si, jak předáte `DateTime.Now` parametr při vytváření **EventHubReceiver** objekt, takže jenom obdrží zprávám odesílaným po spuštění. Tento filtr se hodí v testovacím prostředí, takže uvidíte aktuální sadu zpráv. V pracovním prostředí kódu zkontrolujte, že zpracuje všechny zprávy. Další informace najdete v článku [jak zpracování zpráv IoT Centrum zařízení do cloudu] [ lnk-process-d2c-tutorial] kurz.

7. Nakonec přidejte následující řádky metody **hlavní** :

        Console.WriteLine("Receive messages. Ctrl-C to exit.\n");
        eventHubClient = EventHubClient.CreateFromConnectionString(connectionString, iotHubD2cEndpoint);

        var d2cPartitions = eventHubClient.GetRuntimeInformation().PartitionIds;

        CancellationTokenSource cts = new CancellationTokenSource();

        System.Console.CancelKeyPress += (s, e) =>
        {
          e.Cancel = true;
          cts.Cancel();
          Console.WriteLine("Exiting...");
        };

        var tasks = new List<Task>();
        foreach (string partition in d2cPartitions)
        {
           tasks.Add(ReceiveMessagesFromDeviceAsync(partition, cts.Token));
        }  
        Task.WaitAll(tasks.ToArray());

## <a name="create-a-simulated-device-app"></a>Vytvoření aplikace simulovaný zařízení

V této části vytvoříte konzoly aplikace pro Windows napodobuje zařízení, které odesílá zařízení do cloudu zprávy k rozbočovači IoT.

1. Ve Visual Studiu přidáte projekt Visual Basic klasické počítač s Windows do aktuálního řešení pomocí šablony **Aplikace konzoly** projektu. Nastavit, že je verze .NET Framework 4.5.1 nebo novější. Zadejte název projektu **SimulatedDevice**.

    ![Nový projekt Visual Basic klasické počítač s Windows][10]

2. V Průzkumníku klikněte pravým tlačítkem **SimulatedDevice** projektu a potom klikněte na **Spravovat balíčků Nuget**.

3. V okně **Správce balíčků Nuget** vyberte **Procházet**, vyhledejte **Microsoft.Azure.Devices.Client**, vyberte **nainstalovat** a nainstalujte balíček **Microsoft.Azure.Devices.Client** a přijměte podmínky použití. Tento postup ke stažení, instalace a přidá odkaz na [Azure IoT - zařízení SDK Nuget balíčku] [ lnk-device-nuget] a jeho závislosti.

4. Přidejte následující `using` údajů v horní části souboru **Program.cs** :

        using Microsoft.Azure.Devices.Client;
        using Newtonsoft.Json;

5. Následující pole přidáte do **programu** předmětu. Nahraďte zástupný symbol hodnoty s hostname centrální IoT, obnovená v části "Vytvořit rozbočovači IoT" a klávesu zařízení načíst v části "Vytvoření identity zařízení".

        static DeviceClient deviceClient;
        static string iotHubUri = "{iot hub hostname}";
        static string deviceKey = "{device key}";

6. Přidejte následující metodu třídy **aplikace** :

        private static async void SendDeviceToCloudMessagesAsync()
        {
            double avgWindSpeed = 10; // m/s
            Random rand = new Random();

            while (true)
            {
                double currentWindSpeed = avgWindSpeed + rand.NextDouble() * 4 - 2;

                var telemetryDataPoint = new
                {
                    deviceId = "myFirstDevice",
                    windSpeed = currentWindSpeed
                };
                var messageString = JsonConvert.SerializeObject(telemetryDataPoint);
                var message = new Message(Encoding.ASCII.GetBytes(messageString));

                await deviceClient.SendEventAsync(message);
                Console.WriteLine("{0} > Sending message: {1}", DateTime.Now, messageString);

                Task.Delay(1000).Wait();
            }
        }

    Tento způsob odešle novou zprávu zařízení do cloudu za sekundu. Zpráva obsahuje objekt serializován JSON s ID zařízení a náhodně generovaným číslem tak, aby napodobily senzoru rychlost větru.

7. Nakonec přidejte následující řádky metody **hlavní** :

        Console.WriteLine("Simulated device\n");
        deviceClient = DeviceClient.Create(iotHubUri, new DeviceAuthenticationWithRegistrySymmetricKey("myFirstDevice", deviceKey));

        SendDeviceToCloudMessagesAsync();
        Console.ReadLine();

  Ve výchozím nastavení vytvoří metodu **vytvořit** instance **DeviceClient** , který používá protokol AMQP komunikovat s IoT centrální. Protokol HTTP použijete přepsání metody **vytvořit** , který umožňuje určit protokol. Pokud používáte protokolu HTTP, by měl také přidat balíček Nuget **Microsoft.AspNet.WebApi.Client** do projektu zahrnout oboru **System.Net.Http.Formatting** .

Tento kurz vás provede kroky k vytvoření klientem IoT Centrum zařízení. Můžete také [Připojené služby Azure IoT rozbočovače] [ lnk-connected-service] rozšíření Visual Studia přidat kód potřeby ke klientské aplikaci zařízení.


> [AZURE.NOTE] Jednodušší jednoduché, tento kurz není implementovat zásady libovolný opakovat. Ve výrobním kódu, měli byste zásady opakovat (například exponenciální zdvojnásobení) jako navrhované v článku MSDN [Přechodná zpracování poruch][lnk-transient-faults].

## <a name="run-the-applications"></a>Spuštění aplikace

Teď jste připraveni ke spuštění aplikace.

1.  Ve Visual Studiu v Průzkumníku klikněte pravým tlačítkem řešení a potom klikněte na **Nastavení spuštění projekty**. Vyberte **více projektů po spuštění**a pak vyberte **Spustit** jako akce pro **ReadDeviceToCloudMessages** a **SimulatedDevice** projekty.

    ![Vlastnosti projektu při spuštění][41]

2.  Stisknutím klávesy **F5** spuštění obou aplikace spuštěné. Výstup konzoly z aplikace **SimulatedDevice** zobrazí zprávy, které zařízení simulovaný rozešle rozbočovače IoT. Výstup konzoly z aplikace **ReadDeviceToCloudMessages** zobrazuje zprávy, které vaše Centrum IoT přijímá.

    ![Výstup konzoly z aplikací][42]

3. **Použití** dlaždice [Azure portál] [ lnk-portal] zobrazuje počet odeslaných k Centru zpráv:

    ![Azure portálu použití dlaždic][43]


## <a name="next-steps"></a>Další kroky

V tomto kurzu nakonfigurované rozbočovači IoT Azure portálu a vytvořili zařízení identit v Centrum identity registru. K povolení aplikace simulovaný zařízení k odesílání zpráv zařízení do cloudu k rozbočovači použít tuto identitu zařízení. Aplikaci, která se zobrazí na zprávy přijaté centru taky vytvořili. 

Pokračujte začínáte pracovat s IoT centrální a prozkoumání ostatních případech IoT naleznete v tématu:

- [Připojení zařízení][lnk-connect-device]
- [Začínáme používat správu zařízení][lnk-device-management]
- [Začínáme s SDK IoT brány][lnk-gateway-SDK]

Informace o prodloužení IoT řešení a zpracování zpráv zařízení do cloudu ve velkém měřítku, najdete v článku [zpracování zpráv zařízení do cloudu] [ lnk-process-d2c-tutorial] kurz.

<!-- Images. -->
[41]: ./media/iot-hub-csharp-csharp-getstarted/run-apps1.png
[42]: ./media/iot-hub-csharp-csharp-getstarted/run-apps2.png
[43]: ./media/iot-hub-csharp-csharp-getstarted/usage.png
[10]: ./media/iot-hub-csharp-csharp-getstarted/create-identity-csharp1.png
[11]: ./media/iot-hub-csharp-csharp-getstarted/create-identity-csharp2.png
[12]: ./media/iot-hub-csharp-csharp-getstarted/create-identity-csharp3.png

<!-- Links -->
[lnk-process-d2c-tutorial]: iot-hub-csharp-csharp-process-d2c.md

[lnk-hub-sdks]: iot-hub-devguide-sdks.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-portal]: https://portal.azure.com/

[lnk-eventhubs-tutorial]: ../event-hubs/event-hubs-csharp-ephcs-getstarted.md
[lnk-devguide-identity]: iot-hub-devguide-identity-registry.md
[lnk-servicebus-nuget]: https://www.nuget.org/packages/WindowsAzure.ServiceBus
[lnk-event-hubs-overview]: ../event-hubs/event-hubs-overview.md

[lnk-nuget-service-sdk]: https://www.nuget.org/packages/Microsoft.Azure.Devices/
[lnk-device-nuget]: https://www.nuget.org/packages/Microsoft.Azure.Devices.Client/
[lnk-transient-faults]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx
[lnk-connected-service]: https://visualstudiogallery.msdn.microsoft.com/e254a3a5-d72e-488e-9bd3-8fee8e0cd1d6
[lnk-device-management]: iot-hub-device-management-get-started.md
[lnk-gateway-SDK]: iot-hub-linux-gateway-sdk-get-started.md
[lnk-connect-device]: https://azure.microsoft.com/develop/iot/
