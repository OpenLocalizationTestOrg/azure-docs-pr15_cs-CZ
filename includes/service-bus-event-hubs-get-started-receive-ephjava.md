## <a name="receive-messages-with-eventprocessorhost-in-java"></a>Zprávy s EventProcessorHost Java

EventProcessorHost je třída Java, která zjednodušuje přijímání události z rozbočovače událost tak, že správa trvalý kontrolních bodů a paralelní přijímání tyto rozbočovače události. Pomocí EventProcessorHost můžete rozdělit události napříč několika příjemce, i když hostované v různých uzlů. Tento příklad ukazuje, jak můžete EventProcessorHost u jednoho příjemce.

###<a name="create-a-storage-account"></a>Vytvoření účtu úložiště

Abyste mohli používat EventProcessorHost, musíte mít [účet Azure úložiště][]:

1. Přihlaste se k [portálu Azure klasické][]a klikněte na tlačítko **Nový** v dolní části obrazovky.

2. Klikněte na **Datové služby**, pak **úložiště**a pak **Vytvořit**a potom zadejte název účtu úložiště. Vyberte požadovanou oblast a potom klikněte na **Vytvořit účet úložiště**.

    ![][11]

3. Klikněte na úložiště nově vytvořený účet a klikněte na **Správa přístupových kláves z verze**:

    ![][12]

    Zkopírujte přístup primární klíč, který používá dál v tomto kurzu.

###<a name="create-a-java-project-using-the-eventprocessor-host"></a>Vytvoření projektu Java v hostiteli EventProcessor

Knihovna Java klienta pro událost rozbočovače není k dispozici pro použití v Maven projekty v [Centrálním úložišti Maven][Maven Package]a můžete odkázat pomocí následující deklarace závislost do souboru projektu Maven:    

``` XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-eventhubs</artifactId>
    <version>{VERSION}</version>
</dependency>
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-eventhubs-eph</artifactId>
    <version>{VERSION}</version>
</dependency>
```
 
Pro různé typy sestavení prostředí explicitně získáte nejnovější vydanou SKLENICE soubory z [Centrálním úložišti Maven] [ Maven Package] a od [vydání rozdělení chvíle GitHub](https://github.com/Azure/azure-event-hubs/releases).  

1. V následujícím příkladu nejprve vytvořte nový projekt Maven konzoly/prostředí aplikace ve vašem Oblíbené vývojové prostředí Java. Volaná předmětu ```ErrorNotificationHandler```.     

    ``` Java
    import java.util.function.Consumer;
    import com.microsoft.azure.eventprocessorhost.ExceptionReceivedEventArgs;

    public class ErrorNotificationHandler implements Consumer<ExceptionReceivedEventArgs>
    {
        @Override
        public void accept(ExceptionReceivedEventArgs t)
        {
            System.out.println("SAMPLE: Host " + t.getHostname() + " received general error notification during " + t.getAction() + ": " + t.getException().toString());
        }
    }
    ```

2. Použijte následující kód pro vytvoření nového třídy s názvem ```EventProcessor```.

    ```Java
    import com.microsoft.azure.eventhubs.EventData;
    import com.microsoft.azure.eventprocessorhost.CloseReason;
    import com.microsoft.azure.eventprocessorhost.IEventProcessor;
    import com.microsoft.azure.eventprocessorhost.PartitionContext;

    public class EventProcessor implements IEventProcessor
    {
        private int checkpointBatchingCount = 0;

        @Override
        public void onOpen(PartitionContext context) throws Exception
        {
            System.out.println("SAMPLE: Partition " + context.getPartitionId() + " is opening");
        }

        @Override
        public void onClose(PartitionContext context, CloseReason reason) throws Exception
        {
            System.out.println("SAMPLE: Partition " + context.getPartitionId() + " is closing for reason " + reason.toString());
        }
        
        @Override
        public void onError(PartitionContext context, Throwable error)
        {
            System.out.println("SAMPLE: Partition " + context.getPartitionId() + " onError: " + error.toString());
        }

        @Override
        public void onEvents(PartitionContext context, Iterable<EventData> messages) throws Exception
        {
            System.out.println("SAMPLE: Partition " + context.getPartitionId() + " got message batch");
            int messageCount = 0;
            for (EventData data : messages)
            {
                System.out.println("SAMPLE (" + context.getPartitionId() + "," + data.getSystemProperties().getOffset() + "," +
                        data.getSystemProperties().getSequenceNumber() + "): " + new String(data.getBody(), "UTF8"));
                messageCount++;
                
                this.checkpointBatchingCount++;
                if ((checkpointBatchingCount % 5) == 0)
                {
                    System.out.println("SAMPLE: Partition " + context.getPartitionId() + " checkpointing at " +
                        data.getSystemProperties().getOffset() + "," + data.getSystemProperties().getSequenceNumber());
                    context.checkpoint(data);
                }
            }
            System.out.println("SAMPLE: Partition " + context.getPartitionId() + " batch size was " + messageCount + " for host " + context.getOwner());
        }
    }
    ```

3. Vytvořte si ho jako konečný třídy ```EventProcessorSample```, pomocí následujícího kódu.

    ```Java
    import com.microsoft.azure.eventprocessorhost.*;
    import com.microsoft.azure.servicebus.ConnectionStringBuilder;
    import com.microsoft.azure.eventhubs.EventData;

    public class EventProcessorSample
    {
        public static void main(String args[])
        {
            final String consumerGroupName = "$Default";
            final String namespaceName = "----ServiceBusNamespaceName-----";
            final String eventHubName = "----EventHubName-----";
            final String sasKeyName = "-----SharedAccessSignatureKeyName-----";
            final String sasKey = "---SharedAccessSignatureKey----";

            final String storageAccountName = "---StorageAccountName----";
            final String storageAccountKey = "---StorageAccountKey----";
            final String storageConnectionString = "DefaultEndpointsProtocol=https;AccountName=" + storageAccountName + ";AccountKey=" + storageAccountKey;
            
            ConnectionStringBuilder eventHubConnectionString = new ConnectionStringBuilder(namespaceName, eventHubName, sasKeyName, sasKey);
            
            EventProcessorHost host = new EventProcessorHost(eventHubName, consumerGroupName, eventHubConnectionString.toString(), storageConnectionString);
            
            System.out.println("Registering host named " + host.getHostName());
            EventProcessorOptions options = new EventProcessorOptions();
            options.setExceptionNotification(new ErrorNotificationHandler());
            try
            {
                host.registerEventProcessor(EventProcessor.class, options).get();
            }
            catch (Exception e)
            {
                System.out.print("Failure while registering: ");
                if (e instanceof ExecutionException)
                {
                    Throwable inner = e.getCause();
                    System.out.println(inner.toString());
                }
                else
                {
                    System.out.println(e.toString());
                }
            }

            System.out.println("Press enter to stop");
            try
            {
                System.in.read();
                host.unregisterEventProcessor();
                
                System.out.println("Calling forceExecutorShutdown");
                EventProcessorHost.forceExecutorShutdown(120);
            }
            catch(Exception e)
            {
                System.out.println(e.toString());
                e.printStackTrace();
            }
            
            System.out.println("End of sample");
        }
    }
    ```

4. Následující pole nahraďte hodnoty při vytvoření události centrální a úložiště účtu.

    ``` Java
    final String namespaceName = "----ServiceBusNamespaceName-----";
    final String eventHubName = "----EventHubName-----";

    final String sasKeyName = "-----SharedAccessSignatureKeyName-----";
    final String sasKey = "---SharedAccessSignatureKey----";

    final String storageAccountName = "---StorageAccountName----"
    final String storageAccountKey = "---StorageAccountKey----";
    ```

> [AZURE.NOTE] Tento kurz používá jedna instance EventProcessorHost. Pokud chcete zvýšit výkon, se doporučuje spustit více instancí EventProcessorHost. V těchto případech jednotlivé instance automaticky koordinaci vzájemně k vyrovnávání zatížení přijaté události. Pokud budete potřebovat víc příjemcům každý proces *všechny* události, je nutné použít **ConsumerGroup** koncept. Při příjmu události z různých počítačích, může být užitečné můžete určit názvy EventProcessorHost instance založené na počítačích (nebo role), ve kterém jsou nasazeny.

<!-- Links -->
[Event Hubs overview]: ../articles/event-hubs/event-hubs-overview.md
[Účet Azure úložiště]: ../articles/storage/storage-create-storage-account.md
[Azure klasické portálu]: http://manage.windowsazure.com
[Maven Package]: https://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-eventhubs-eph%22

<!-- Images -->
[11]: ./media/service-bus-event-hubs-get-started-receive-ephjava/create-eph-csharp2.png
[12]: ./media/service-bus-event-hubs-get-started-receive-ephjava/create-eph-csharp3.png

