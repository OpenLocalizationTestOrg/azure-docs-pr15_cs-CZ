<properties
    pageTitle="Odesílání zpráv cloudu zařízení s IoT centrální | Microsoft Azure"
    description="Postupujte podle tohoto kurzu se dozvíte, jak k odesílání zpráv cloudu zařízení používáte rozbočovač IoT Azure s C#."
    services="iot-hub"
    documentationCenter=".net"
    authors="fsautomata"
    manager="timlt"
    editor=""/>

<tags
     ms.service="iot-hub"
     ms.devlang="dotnet"
     ms.topic="article"
     ms.tgt_pltfrm="na"
     ms.workload="na"
     ms.date="06/23/2016"
     ms.author="elioda"/>

# <a name="tutorial-how-to-send-cloud-to-device-messages-with-iot-hub-and-net"></a>Kurz: Postup při odesílání zprávy cloudu zařízení s IoT centrální a .net

[AZURE.INCLUDE [iot-hub-selector-c2d](../../includes/iot-hub-selector-c2d.md)]

## <a name="introduction"></a>Úvod

Azure centrální IoT je plně spravovaných služba, která pomáhá povolit spolehlivý a zabezpečené obousměrné komunikaci mezi miliony IoT zařízení a ukončení aplikace zpět. Kurz [Seznámení s IoT centrální] ukazuje, jak vytvořit rozbočovači IoT zřízení zařízení identit v něm a kód simulovaný zařízení, které odesílá zprávy zařízení do cloudu.

Tento kurz je založena na [Začínáme s IoT rozbočovači]. To se dozvíte, jak chcete:

- Z vaší aplikace cloudu back-end odešlete zprávy cloudu zařízení jednoho zařízení pomocí IoT rozbočovače.
- Přijímání zpráv cloudu zařízení na zařízení.
- Z aplikace cloudu serverová, požadovat potvrzení o doručení (*zpětnou vazbu*) u zpráv odeslaných z centrální IoT zařízení.

Další informace o zprávách cloudu zařízení můžete najít v [IoT centrální Developer Guide][IoT Hub Developer Guide - C2D].

Na konci tohoto kurzu se spustí dvě aplikace konzoly Windows:

* **SimulatedDevice**, upravenou verzi aplikace vytvořené v [začít pracovat s IoT centrální], která se připojí k centrální IoT volání a přijímání zpráv cloudu zařízení.
* **SendCloudToDevice**, která odešle zprávu cloudu zařízení simulovaný zařízení pomocí IoT rozbočovače a dostane jeho potvrzení doručení.

> [AZURE.NOTE] Centrální IoT podporuje SDK pro mnoho platformy zařízení a jazyky (včetně C, Java a Javascript) až SDK Azure IoT zařízení. Podrobný návod k připojení zařízení tento kurz kód a obecně k rozbočovači IoT Azure naleznete v článku [Středisko pro vývojáře IoT Azure].

Tento kurz, musíte mít takto:

+ Microsoft Visual Studio 2015

+ Účet Azure active. (Pokud nemáte účet, můžete vytvořit [bezplatný účet] [ lnk-free-trial] v jenom pár minut.)

## <a name="receive-messages-on-the-simulated-device"></a>Přijímání zpráv na simulovaný zařízení

V této části budete změnit simulovaný zařízení aplikaci vytvořené v části [Začínáme s IoT centrální] do cloudu zařízení zprávy z centra IoT.

1. Ve Visual Studiu v projektu **SimulatedDevice** dodejte následujícího postupu třídy **Program** .

        private static async void ReceiveC2dAsync()
        {
            Console.WriteLine("\nReceiving cloud to device messages from service");
            while (true)
            {
                Message receivedMessage = await deviceClient.ReceiveAsync();
                if (receivedMessage == null) continue;

                Console.ForegroundColor = ConsoleColor.Yellow;
                Console.WriteLine("Received message: {0}", Encoding.ASCII.GetString(receivedMessage.GetBytes()));
                Console.ResetColor();

                await deviceClient.CompleteAsync(receivedMessage);
            }
        }

    `ReceiveAsync` Asynchronní vrátí metoda přijaté zprávy v době, které se vztahují zařízení. Vrátí *hodnotu null* po určité době specifiable vypršení časového limitu (v tomto případě je použita výchozí jednu minutu). V takovém případě kód nadále čekání na nové zprávy. Toto je důvod `if (receivedMessage == null) continue` řádku.

    Volání `CompleteAsync()` IoT centrální úspěšně zpracovat zprávu s upozorněním. Zprávu můžete bezpečně odebrat z fronty zařízení. Pokud něco stalo zabraňující zařízení aplikaci z dokončování zpracování zprávy, IoT Centrum poskytuje ho znovu. Je pak důležité, aby logiky pro zpracování zprávy v aplikaci zařízení *idempotent*, aby příjem stejná zpráva tisknutím vytvoří stejný výsledek. Aplikaci můžete taky dočasně opustit zprávu, která má za následek IoT centrální ponechá zprávu ve frontě pro budoucí spotřebu. Nebo aplikaci můžete odmítnout zprávu, která zprávu trvale odebere z fronty. Další informace o životním cyklu zpráva cloudu zařízení najdete v článku [IoT rozbočovači vývojář příručka][IoT Hub Developer Guide - C2D].

    > [AZURE.NOTE] Při použití protokolu HTTP místo MQTT nebo AMQP jako přenos, `ReceiveAsync` metoda vrátí okamžitě. Podporované vzor pro zprávy cloudu zařízení s podporou protokolu HTTP se občas připojená zařízení, které kontrola zprávy zřídka (nižší než každých 25 minut). Vystavení další HTTP obdrží výsledky v centrální IoT omezení žádosti. Další informace o rozdílech mezi MQTT, AMQP a HTTP podporu a IoT centrální omezení, najdete v článku [IoT centrální Developer Guide][IoT Hub Developer Guide - C2D].

2. Přidejte následující metodu v metodu **hlavní** přímo před `Console.ReadLine()` řádek:

        ReceiveC2dAsync();

> [AZURE.NOTE] V jednoduchosti saké tento kurz není implementovat zásady libovolný opakovat. Ve výrobním kódu, měli byste zásady opakovat (například exponenciální zdvojnásobení), jako navrhované v článku MSDN [Přechodná zpracování chyby].

## <a name="send-a-cloud-to-device-message"></a>Odeslání zprávy cloudu zařízení

V této části napíšete aplikace konzoly Windows, zasílání zpráv cloudu zařízení aplikaci simulovaný zařízení.

1. V aktuálním Visual Studio řešení vytvoření nového projektu Visual Basic plochy aplikace pomocí šablony **Aplikace konzoly** projektu. Zadejte název projektu **SendCloudToDevice**.

    ![Nový projekt ve Visual Studiu][20]

2. V Průzkumníku klikněte pravým tlačítkem myši na řešení a potom klikněte na **Spravovat NuGet balíčků řešení …**. 

    Otevře se okno **Správa balíčků NuGet** .

3. Vyhledejte `Microsoft Azure Devices`, klikněte na tlačítko **instalovat**a přijměte podmínky použití. 

    To ke stažení, instalaci a přidá odkaz na [Azure IoT - služby SDK NuGet balíčku].

4. Přidejte následující `using` údajů v horní části souboru **Program.cs** :

        using Microsoft.Azure.Devices;

5. Následující pole přidáte do **programu** předmětu. Nahraďte zástupný symbol hodnotou s IoT centrální připojovacího řetězce z [začít pracovat s IoT centrální]:

        static ServiceClient serviceClient;
        static string connectionString = "{iot hub connection string}";

6. Přidejte následující metodu třídy **aplikace** :

        private async static Task SendCloudToDeviceMessageAsync()
        {
            var commandMessage = new Message(Encoding.ASCII.GetBytes("Cloud to device message."));
            await serviceClient.SendAsync("myFirstDevice", commandMessage);
        }

    Tento způsob odešle novou zprávu cloudu zařízení na zařízení s ID `myFirstDevice`. Měnit tento parametr, v případě, že jste změnili z použitého v [začít pracovat s IoT centrální].

7. Nakonec přidejte následující řádky metody **hlavní** :

        Console.WriteLine("Send Cloud-to-Device message\n");
        serviceClient = ServiceClient.CreateFromConnectionString(connectionString);

        Console.WriteLine("Press any key to send a C2D message.");
        Console.ReadLine();
        SendCloudToDeviceMessageAsync().Wait();
        Console.ReadLine();

8. Z aplikace Visual Studio klikněte pravým tlačítkem myši řešení a vyberte **Nastavení spuštění projekty...**. Výběr **více projektů po spuštění**a pak vyberte **Spustit** akci **ProcessDeviceToCloudMessages**, **SimulatedDevice**a **SendCloudToDevice**.

9.  Stiskněte klávesu **F5**. Všechny tři aplikace by měly začít. Vyberte **SendCloudToDevice** windows a stiskněte klávesu **Enter**. Zobrazí se zpráva Probíhá podle simulovaný aplikace byste měli vidět.

    ![Přijímání zpráv aplikace][21]

## <a name="receive-delivery-feedback"></a>Získávat názory ostatních doručení
Je možné k žádosti o doručení (nebo vypršení platnosti) potvrzení od IoT rozbočovače pro každou zprávu cloudu zařízení. Díky cloudu back-end snadno informovat použití logických operátorů opakovat nebo vyrovnání. Další informace o svůj názor cloudu zařízení najdete v článku [IoT rozbočovači vývojář příručka][IoT Hub Developer Guide - C2D].

V této části upravíte aplikaci **SendCloudToDevice** požádat o svůj názor a přijímat z centrální IoT.

1. Ve Visual Studiu v projektu **SendCloudToDevice** dodejte následujícího postupu třídy **Program** .
   
        private async static void ReceiveFeedbackAsync()
        {
            var feedbackReceiver = serviceClient.GetFeedbackReceiver();

            Console.WriteLine("\nReceiving c2d feedback from service");
            while (true)
            {
                var feedbackBatch = await feedbackReceiver.ReceiveAsync();
                if (feedbackBatch == null) continue;

                Console.ForegroundColor = ConsoleColor.Yellow;
                Console.WriteLine("Received feedback: {0}", string.Join(", ", feedbackBatch.Records.Select(f => f.StatusCode)));
                Console.ResetColor();

                await feedbackReceiver.CompleteAsync(feedbackBatch);
            }
        }

    Všimněte si, že vzorek přijímání stejné použitým přijímat cloudu zařízení zprávy z aplikace zařízení.

2. Přidejte následující metodu v metodu **hlavní** ihned po `serviceClient = ServiceClient.CreateFromConnectionString(connectionString)` řádek:

        ReceiveFeedbackAsync();

3. Požádat o svůj názor doručení zprávy cloudu zařízení, budete muset zadat vlastnost v metodu **SendCloudToDeviceMessageAsync** . Přidejte následující řádek ihned po `var commandMessage = new Message(...);` řádek:

        commandMessage.Ack = DeliveryAcknowledgement.Full;

4.  Spuštění aplikace stisknutím klávesy **F5**. Měli byste vidět všechny tři aplikace start. Vyberte **SendCloudToDevice** windows a stiskněte klávesu **Enter**. Měli byste vidět právě přijaté zprávy podle simulovaný aplikace a po pár sekund, zpráva názory přijímané aplikací **SendCloudToDevice** .

    ![Přijímání zpráv aplikace][22]

> [AZURE.NOTE] V jednoduchosti saké tento kurz není implementovat zásady libovolný opakovat. Ve výrobním kódu, měli byste zásady opakovat (například exponenciální zdvojnásobení), jako navrhované v článku MSDN [Přechodná zpracování chyby].

## <a name="next-steps"></a>Další kroky

V tomto kurzu se naučíte odesílání a přijímání zpráv cloudu zařízení. 

Příklady úplné řešení začátku do konce, které používají IoT centrální najdete [Azure IoT sadu].

Další informace o vytváření řešení s IoT rozbočovači, najdete v článku [IoT rozbočovači vývojář Průvodce].

<!-- Images -->
[20]: ./media/iot-hub-csharp-csharp-c2d/create-identity-csharp1.png
[21]: ./media/iot-hub-csharp-csharp-c2d/sendc2d1.png
[22]: ./media/iot-hub-csharp-csharp-c2d/sendc2d2.png

<!-- Links -->

[Azure IoT - služby SDK NuGet balíčku]: https://www.nuget.org/packages/Microsoft.Azure.Devices/
[Zpracování přechodná poruch]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx

[IoT Hub Developer Guide - C2D]: iot-hub-devguide-messaging.md

[Příručka IoT centrální vývojář]: iot-hub-devguide.md
[Začínáme s IoT centrální]: iot-hub-csharp-csharp-getstarted.md
[Středisko pro vývojáře IoT Azure]: http://www.azure.com/develop/iot
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[Azure IoT Suite]: https://azure.microsoft.com/documentation/suites/iot-suite/