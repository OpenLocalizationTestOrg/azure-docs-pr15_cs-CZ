## <a name="send-messages-to-event-hubs"></a>Odeslání zprávy rozbočovače události

V této části napíšete aplikace konzoly Windows, které odesílá události rozbočovače události.

1. Ve Visual Studiu vytvoření nového projektu Visual Basic plochy aplikace pomocí šablony **Aplikace konzoly** projektu. Zadejte název projektu **odesílatele**.

    ![](./media/service-bus-event-hubs-getstarted-send-csharp/create-sender-csharp1.png)

2. V Průzkumníku klikněte pravým tlačítkem myši na řešení a potom klikněte na **Spravovat NuGet balíčků řešení**. 

3. Klikněte na kartu **Procházet** a potom vyhledejte `Microsoft Azure Service Bus`. Ujistěte se, že je název projektu (**odesílatele**) zadané v poli **verze** . Klikněte na tlačítko **instalovat**a přijměte podmínky použití. 

    ![](./media/service-bus-event-hubs-getstarted-send-csharp/create-sender-csharp2.png)

    Visual Studio ke stažení, instalace a přidá odkaz na [balíček NuGet Bus služby Azure knihovny](https://www.nuget.org/packages/WindowsAzure.ServiceBus).

4. Přidejte následující `using` příkazy v horní části souboru **Program.cs** :

    ```
    using System.Threading;
    using Microsoft.ServiceBus.Messaging;
    ```

5. Následující pole přidáte do **programu** třídy nahrazení zástupného symbolu hodnoty název události centrální jste vytvořili v předchozí části a názvů úrovni připojovací řetězec, který jste si předtím uložili.

    ```
    static string eventHubName = "{Event Hub name}";
    static string connectionString = "{send connection string}";
    ```

6. Přidejte následující metodu třídy **aplikace** :

    ```
    static void SendingRandomMessages()
    {
        var eventHubClient = EventHubClient.CreateFromConnectionString(connectionString, eventHubName);
        while (true)
        {
            try
            {
                var message = Guid.NewGuid().ToString();
                Console.WriteLine("{0} > Sending message: {1}", DateTime.Now, message);
                eventHubClient.Send(new EventData(Encoding.UTF8.GetBytes(message)));
            }
            catch (Exception exception)
            {
                Console.ForegroundColor = ConsoleColor.Red;
                Console.WriteLine("{0} > Exception: {1}", DateTime.Now, exception.Message);
                Console.ResetColor();
            }

            Thread.Sleep(200);
        }
    }
    ```

    Tento způsob průběžně, odešle události rozbočovače událost s 200 ms zpoždění.

7. Nakonec přidejte následující řádky metody **hlavní** :

    ```
    Console.WriteLine("Press Ctrl-C to stop the sender process");
    Console.WriteLine("Press Enter to start now");
    Console.ReadLine();
    SendingRandomMessages();
    ```
