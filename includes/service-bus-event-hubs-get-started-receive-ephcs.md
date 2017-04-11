## <a name="receive-messages-with-eventprocessorhost"></a>Zprávy s EventProcessorHost

[EventProcessorHost][] je třída .NET, který usnadňuje přijímání události z rozbočovače událost tak, že správa trvalý kontrolních bodů a současně přijímání tyto rozbočovače události. Pomocí [EventProcessorHost][], můžete rozdělit události napříč několika příjemce, i když hostované v různých uzlů. Tento příklad ukazuje, jak můžete [EventProcessorHost][] u jednoho příjemce. [Zpracování měřítkem se události][] příklad ukazuje, jak používat [EventProcessorHost][] s více příjemcům.

Pokud chcete používat [EventProcessorHost][], musíte mít [účet Azure úložiště][]:

1. Přihlaste se k [portálu Azure][]a klikněte na tlačítko **Nový** v horní levé části obrazovky.

2. Klikněte na **Data + úložiště**a potom klikněte na **účet úložiště**.

    ![](./media/service-bus-event-hubs-getstarted-receive-ephcs/create-storage1.png)

3. Do zásuvné **vytvořit úložiště účtu** zadejte název účtu úložiště. Zvolte Azure předplatného, skupina zdroje a umístění, do kterého chcete vytvořit zdroj. Klikněte na **vytvořit**.

    ![](./media/service-bus-event-hubs-getstarted-receive-ephcs/create-storage2.png)

4. V seznamu úložiště účtů klikněte na úložiště nově vytvořený účet.

5. V zásuvné účtu úložiště klikněte na **přístupových kláves z verze**. Zkopírujte hodnotu **klíč1** můžete dál v tomto kurzu.

    ![](./media/service-bus-event-hubs-getstarted-receive-ephcs/create-storage3.png)

4. Ve Visual Studiu vytvoření nového projektu Visual Basic plochy aplikace pomocí šablony **Aplikace konzoly** projektu. Zadejte název projektu **příjemce**.

    ![](./media/service-bus-event-hubs-getstarted-receive-ephcs/create-receiver-csharp1.png)

5. V Průzkumníku klikněte pravým tlačítkem myši na řešení a potom klikněte na **Spravovat NuGet balíčků řešení**.

6. Klikněte na kartu **Procházet** a potom vyhledejte `Microsoft Azure Service Bus Event Hub - EventProcessorHost`. Ujistěte se, že je název projektu (**sluchátko**) zadané v poli **verze** . Klikněte na tlačítko **instalovat**a přijměte podmínky použití.

    ![](./media/service-bus-event-hubs-getstarted-receive-ephcs/create-eph-csharp1.png)

    Visual Studio ke stažení, instalace a přidá odkaz na [Azure služby Bus události rozbočovače - EventProcessorHost NuGet balíčku](https://www.nuget.org/packages/Microsoft.Azure.ServiceBus.EventProcessorHost), se všechny závislostmi.

7. Klikněte pravým tlačítkem **sluchátko** projektu, klikněte na **Přidat**a potom klikněte na **Onenotové**. Pojmenování nové třídy **SimpleEventProcessor**a potom klikněte na tlačítko **Přidat** předmětu.

    ![](./media/service-bus-event-hubs-getstarted-receive-ephcs/create-receiver-csharp2.png)

8. V horní části souboru SimpleEventProcessor.cs přidejte následující příkazy:

    ```
    using Microsoft.ServiceBus.Messaging;
    using System.Diagnostics;
    ```

    Potom nahraďte následující kód pro obsah třídy:

    ```
    class SimpleEventProcessor : IEventProcessor
    {
        Stopwatch checkpointStopWatch;

        async Task IEventProcessor.CloseAsync(PartitionContext context, CloseReason reason)
        {
            Console.WriteLine("Processor Shutting Down. Partition '{0}', Reason: '{1}'.", context.Lease.PartitionId, reason);
            if (reason == CloseReason.Shutdown)
            {
                await context.CheckpointAsync();
            }
        }

        Task IEventProcessor.OpenAsync(PartitionContext context)
        {
            Console.WriteLine("SimpleEventProcessor initialized.  Partition: '{0}', Offset: '{1}'", context.Lease.PartitionId, context.Lease.Offset);
            this.checkpointStopWatch = new Stopwatch();
            this.checkpointStopWatch.Start();
            return Task.FromResult<object>(null);
        }

        async Task IEventProcessor.ProcessEventsAsync(PartitionContext context, IEnumerable<EventData> messages)
        {
            foreach (EventData eventData in messages)
            {
                string data = Encoding.UTF8.GetString(eventData.GetBytes());

                Console.WriteLine(string.Format("Message received.  Partition: '{0}', Data: '{1}'",
                    context.Lease.PartitionId, data));
            }

            //Call checkpoint every 5 minutes, so that worker can resume processing from 5 minutes back if it restarts.
            if (this.checkpointStopWatch.Elapsed > TimeSpan.FromMinutes(5))
            {
                await context.CheckpointAsync();
                this.checkpointStopWatch.Restart();
            }
        }
    }
    ```

    Tato třída volaná **EventProcessorHost** na události proces dostali z centra události. Všimněte si, že `SimpleEventProcessor` třídy používá stopky pravidelně volat metodu kontrolní bod v kontextu **EventProcessorHost** . Zajistíte tím, že pokud se nerestartuje příjemce, dojde ke ztrátě maximálně pět minut zpracování práce.

9. Ve třídě **Program** přidejte následující text `using` údajů v horní části souboru:

    ```
    using Microsoft.ServiceBus.Messaging;
    ```

    Potom nahraďte `Main` metoda `Program` třídy s následující kód zaokrouhlením názvu události centrální a názvů úrovni připojovací řetězec, který jste si uložili v předchozích verzích a úložiště účtu a klíčem, kterou jste zkopírovali v předchozích částech. 

    ```
    static void Main(string[] args)
    {
      string eventHubConnectionString = "{Event Hub connection string}";
      string eventHubName = "{Event Hub name}";
      string storageAccountName = "{storage account name}";
      string storageAccountKey = "{storage account key}";
      string storageConnectionString = string.Format("DefaultEndpointsProtocol=https;AccountName={0};AccountKey={1}", storageAccountName, storageAccountKey);

      string eventProcessorHostName = Guid.NewGuid().ToString();
      EventProcessorHost eventProcessorHost = new EventProcessorHost(eventProcessorHostName, eventHubName, EventHubConsumerGroup.DefaultGroupName, eventHubConnectionString, storageConnectionString);
      Console.WriteLine("Registering EventProcessor...");
      var options = new EventProcessorOptions();
      options.ExceptionReceived += (sender, e) => { Console.WriteLine(e.Exception); };
      eventProcessorHost.RegisterEventProcessorAsync<SimpleEventProcessor>(options).Wait();

      Console.WriteLine("Receiving. Press enter key to stop worker.");
      Console.ReadLine();
      eventProcessorHost.UnregisterEventProcessorAsync().Wait();
    }
    ```

> [AZURE.NOTE] Tento kurz používá jedna instance [EventProcessorHost][]. Pokud chcete zvýšit výkon, doporučujeme spustit více instancí [EventProcessorHost][]uvedeno v ukázkové [měřítkem, zpracování události][] . V těchto případech jednotlivé instance automaticky koordinaci s ostatními na Vyrovnávání zatížení přijaté události. Pokud budete potřebovat víc příjemcům každý proces *všechny* události, je nutné použít **ConsumerGroup** koncept. Při příjmu události z různých počítačích, může být užitečné můžete určit názvy [EventProcessorHost][] instance založené na počítačích (nebo role), ve kterém jsou nasazeny. Další informace o tématech najdete v tématech [Přehled rozbočovače událostí][] a [Průvodce rozbočovače programování událostí][] .

<!-- Links -->
[Přehled rozbočovače událostí]: ../articles/event-hubs/event-hubs-overview.md
[Událost rozbočovače programování Průvodce]: ../articles/event-hubs/event-hubs-programming-guide.md
[Měřítko, zpracování události]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-45f43fc3
[Účet Azure úložiště]: ../articles/storage/storage-create-storage-account.md
[EventProcessorHost]: http://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventprocessorhost(v=azure.95).aspx
[Azure portálu]: https://portal.azure.com