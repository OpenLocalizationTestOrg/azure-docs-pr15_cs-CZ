<properties
    pageTitle="Zpracování zpráv zařízení do cloudu IoT centrální (.Net) | Microsoft Azure"
    description="Postupujte podle kurzu, který další užitečné vzorků pro zpracování zpráv IoT Centrum zařízení do cloudu."
    services="iot-hub"
    documentationCenter=".net"
    authors="dominicbetts"
    manager="timlt"
    editor=""/>

<tags
     ms.service="iot-hub"
     ms.devlang="csharp"
     ms.topic="article"
     ms.tgt_pltfrm="na"
     ms.workload="na"
     ms.date="10/05/2016"
     ms.author="dobett"/>

# <a name="tutorial-how-to-process-iot-hub-device-to-cloud-messages-using-net"></a>Kurz: Způsobu zpracování zpráv IoT Centrum zařízení do cloudu pomocí .net

[AZURE.INCLUDE [iot-hub-selector-process-d2c](../../includes/iot-hub-selector-process-d2c.md)]

## <a name="introduction"></a>Úvod

Azure centrální IoT je plně spravovaných služba, která umožňuje spolehlivý a zabezpečené obousměrnou komunikaci mezi miliony IoT zařízení a aplikace serverová. Další kurzy ([začít pracovat s IoT centrální] a [posílání zpráv cloudu zařízení s IoT centrální][lnk-c2d]) ukazují, jak použít základní funkce zařízení do cloudu a cloudu zařízení zpráv IoT rozbočovače.

Tento kurz je založena na kód zobrazený v kurz [Seznámení s IoT centrální] a zobrazí dvě scalable vzorky, které můžete použít ke zpracování zpráv zařízení do cloudu:

- Spolehlivé ukládání zpráv zařízení do cloudu v [úložišti objektů blob Azure]. Běžné situace je *studenou cestu* analýzy, ve které máte uložené telemetrickými daty v objektů BLOB můžete předávat na vstupu do analýzy procesů. Tyto procesy můžou úsilím pomocí nástrojů ATP [Factory dat Azure] [HDInsight (Hadoop)] vrstvě.

- Spolehlivé zpracování zpráv *interaktivní* zařízení do cloudu. Zařízení do cloudu zprávy jsou interaktivní okamžiku, kdy je okamžité aktivační události pro sady akcí v aplikaci back-end. Zařízení může třeba poslat zprávu upozornění, která aktivuje vkládání lístek na CRM systému. Naopak *datový bod* zprávy jednoduše kanálu sociální sítě do modul analýzy. Teplotní telemetrie z zařízení, který je uložený pro pozdější analýzu je například zprávu datový bod.

Protože IoT Centrum poskytuje [Události centrální][lnk-event-hubs]-kompatibilní koncový bod, aby se zprávy zařízení do cloudu, tohoto návodu používá instanci [EventProcessorHost] . Tato instance:

* Problémy se spolehlivým ukládá *datový bod* zprávy v úložišti objektů blob Azure.
* K předávání *interaktivní* zařízení do cloudu zpráv Azure [fronty Bus služby] pro zpracování okamžitě.

Služba Bus zajistit spolehlivé zpracování interaktivní zpráv poskytuje jednotlivé zprávy kontroly a rušení duplikace na základě okno čas.

> [AZURE.NOTE] Instanci **EventProcessorHost** je pouze jeden způsob zpracování interaktivní zpráv. Další možnosti zahrnují [Struktury služby Azure] [ lnk-service-fabric] a [Analýzy toku Azure][lnk-stream-analytics].

Na konci tohoto kurzu spusťte tři aplikace konzoly Windows:

* **SimulatedDevice**, upravenou verzi aplikace vytvořený v kurz [Seznámení s IoT centrální] odešle data čárky zařízení do cloudu zprávy každý druhý a interaktivní zprávy zařízení do cloudu každých 10 sekund. Tato aplikace používá pro komunikaci s IoT centrální protokol AMQP.
* **ProcessDeviceToCloudMessages** používá třídu [EventProcessorHost] pro vzkazy z koncového bodu kompatibilní s prohlížečem události centrální. Potom problémy se spolehlivým ukládá zprávy bodů dat v úložišti objektů blob Azure a předá interaktivní zprávy do fronty Bus služby.
* **ProcessD2CInteractiveMessages** vypnutí fronty interaktivní zprávy od fronty Bus služby.

> [AZURE.NOTE] Rozbočovač IoT podporuje SDK pro mnoho platformy zařízení a jazyků, včetně C, Java a JavaScript. Zjistěte, jak chcete nahradit simulovaný zařízení v tomto kurzu fyzické zařízení a jak se připojit k rozbočovači IoT zařízení, najdete v článku [Středisko pro vývojáře IoT Azure].

Tento kurz přímo platí pro další způsoby, jak používat kompatibilní s prohlížečem události Centrum zpráv, třeba projekty [HDInsight (Hadoop)] . Další informace najdete v [příručce vývojář Azure IoT centrální - zařízení do cloudu].

Tento kurz, je potřeba k provedení těchto věcí:

+ Microsoft Visual Studio 2015.

+ Účet Azure active. <br/>Pokud nemáte účet, můžete vytvořit [bezplatný účet](https://azure.microsoft.com/free/) v jenom pár minut.

Měli byste mít některé základní znalost [Azure úložiště] a [Bus služby Azure].


## <a name="send-interactive-messages-from-a-simulated-device"></a>Odesílání interaktivní zpráv z simulovaný zařízení

V této části můžete změnit simulovaný zařízení aplikaci, kterou jste vytvořili v kurzu [Začínáme s IoT centrální] interaktivní zařízení do cloudu zprávy odešlete centru IoT.

1. Ve Visual Studiu v projektu **SimulatedDevice** dodejte následujícího postupu třídy **Program** .

    ```
    private static async void SendDeviceToCloudInteractiveMessagesAsync()
    {
      while (true)
      {
        var interactiveMessageString = "Alert message!";
        var interactiveMessage = new Message(Encoding.ASCII.GetBytes(interactiveMessageString));
        interactiveMessage.Properties["messageType"] = "interactive";
        interactiveMessage.MessageId = Guid.NewGuid().ToString();

        await deviceClient.SendEventAsync(interactiveMessage);
        Console.WriteLine("{0} > Sending interactive message: {1}", DateTime.Now, interactiveMessageString);

        Task.Delay(10000).Wait();
      }
    }
    ```

    Tento způsob je podobný metodu **SendDeviceToCloudMessagesAsync** **SimulatedDevice** projektu. Pouze rozdíly se, že teď nastavte vlastnost systém **MessageId** , vlastnosti uživatele s názvem **Typ zprávy**.
    Kód přiřadí vlastnost **MessageId** globálně jedinečný identifikátor (GUID). Služba Bus umožňuje tento identifikátor odstranit duplicitní zpráv, které obdrží. Příklad používá vlastnost **Typ zprávy** rozlišit interaktivní z datového bodu zprávy. Aplikace předá tyto informace v dialogovém okně Vlastnosti zprávy, ne v těle zprávy tak, aby procesor událostí nemusí rekonstrukci zprávy provádět směrování zpráv.

    > [AZURE.NOTE] Je důležité pro vytvoření **MessageId** lze odstranit duplicitní interaktivní zprávy v kódu zařízení. Komunikace chybovému sítě nebo jiné chyby, může vést k více oznámeními stejné zprávy od toto zařízení. Můžete taky použít ID sémantického zprávy, například hash příslušné zprávy datová pole, místo identifikátor GUID.

2. Přidejte následující metodu v metodu **hlavní** přímo před `Console.ReadLine()` řádek:

    ````
    SendDeviceToCloudInteractiveMessagesAsync();
    ````

    > [AZURE.NOTE] Za účelem zjednodušení tento kurz není implementovat zásady libovolný opakovat. Ve výrobním kódu měli byste implementovat zásadu opakovat například exponenciální zdvojnásobení navržené v článku MSDN [Přechodná zpracování poruch].

## <a name="process-device-to-cloud-messages"></a>Zpracování zpráv zařízení do cloudu

V této části vytvoříte konzoly aplikace pro Windows zpracování zpráv zařízení do cloudu z centrální IoT. Rozbočovač IOT zpřístupňuje [události rozbočovači]-kompatibilní koncový bod povolení aplikace čtení zpráv zařízení do cloudu. Tento kurz používá třídu [EventProcessorHost] zpracuje tyto zprávy v aplikaci konzoly. Další informace o tom, jak zpracování zpráv z rozbočovače události najdete v tématu kurz [Seznámení s rozbočovače události] .

Ověřovací kód při implementace spolehlivé úložiště zpráv datový bod nebo předávání interaktivní zpráv je zpracování události závisí na příjemce zprávy stanovit kontroly průběhu. Kromě toho dosáhnout vysoký výkon při čtení z události rozbočovače by měl obsahovat kontroly ve velkých listů. Tento přístup vytvoří možnost duplicitní zpracování velké množství zpráv je-li selhání a návrat do předchozí kontroly. V tomto kurzu najdete v článku synchronizace zápisy Azure úložiště a služby Bus rušení duplikace windows se **EventProcessorHost** kontroly.

K psaní zpráv k základnímu úložišti Azure problémy se spolehlivým, vzorku používá tuto funkci potvrdit jednotlivé blok s [objekty BLOB blok][Azure Block Blobs]. Procesor událostí sečteny zprávy v paměti, dokud je potřeba zadat kontrolní bod. Například po Akumulované vyrovnávací paměť zpráv dosáhne maximální velikosti 4 MB, nebo odinstalaci duplikace Bus služby uplyne časového intervalu. Potom před kontrolu, kód potvrdí blok nový objekt blob.

Procesor událostí používá události rozbočovače odsadí zprávu jako blokovat ID. Tento postup umožňuje procesoru události kontrolovat rušení duplikace před potvrdí nový blok k základnímu úložišti, starat možné zhroucení mezi potvrzování bloku a kontrolu.

> [AZURE.NOTE] Tento kurz používá jeden účet Azure úložiště k zápisu všechny zprávy načtené z centrální IoT. Zjištění potřebnosti používat více účtů Azure úložiště ve vašem řešení, najdete v článku [úložišti Azure škálovatelnost pokyny].

Chcete-li předejít duplicitní položky při zpracování interaktivní zprávy používá aplikace služby Bus rušení duplikování. Simulovaný zařízení razítkem každé interaktivní zprávy s jedinečnými **MessageId**. Tato ID povolit službu Bus zajistit, že v okně čas zadaný rušení duplikace žádné dva s stejné **MessageId** doručování zpráv příjemce. Tento rušení duplikace společně s sémantiku dokončení jednotlivých zpráv poskytovaných služeb Bus fronty snadno implementovat spolehlivé zpracování interaktivní zpráv.

Abyste měli jistotu, že se žádná zpráva znovu mimo oknem rušení duplikace, kód synchronizuje mechanismus kontrolní bod **EventProcessorHost** s oknem rušení duplikace fronty Bus služby. Tato synchronizace vystavila společnost vynucení kontrolní bod aspoň jednou pokaždé, když má oknem rušení duplikace (v tomto kurzu okno je jedna hodina).

> [AZURE.NOTE] Tento kurz používá jediné rozdělený služby Bus fronty procesu interaktivní zprávy načtená z kontingenčního seznamu IoT centrální. Další informace o použití služby Bus fronty požadavkům škálovatelnost řešení najdete v dokumentaci [Bus služby Azure] .

### <a name="provision-an-azure-storage-account-and-a-service-bus-queue"></a>Zřízení účtu Azure úložiště a služby Bus fronty
Použití třídy [EventProcessorHost] , musíte mít účet Azure úložiště povolit **EventProcessorHost** pořizovat záznam kontrolní bod informace. Můžete použít existující účet Azure úložiště nebo postupujte podle pokynů v [O úložišti Azure] a vytvořte nový účet. Poznamenejte si úložišti Azure účtu připojovací řetězec.

> [AZURE.NOTE] Když zkopírujete a vložíte připojovací řetězec účet Azure úložiště, ujistěte se, jsou zahrnuty bez mezer.

Budete potřebovat služby Bus fronty umožníte spolehlivé zpracování interaktivní zpráv. Můžete vytvořit fronty programově s oknem rušení duplikace hodinu, způsobem popsaným v tématu [jak používat službu Bus fronty][fronty Bus služby]. Můžete taky můžete použít [Azure klasické portál][lnk-classic-portal], pomocí následujících kroků:

1. V levém dolním rohu klikněte na **Nový** . Klikněte na položku **Aplikace služby** > **Služby Bus** > **fronty** > **Vytvořit vlastní**. Zadejte název **d2ctutorial**, vyberte oblast a použijte existující obor názvů nebo vytvořte novou. Na další stránce zaškrtněte políčko **Povolit duplicit**a nastavte **duplicitní zjišťování historie časového intervalu** na 1 hodinu. Klepněte na značku zaškrtnutí v pravém dolním rohu uložení fronty konfigurace.

    ![Vytvoření fronty Azure portálu][30]

2. V seznamu fronty Bus služby klikněte na **d2ctutorial**a potom klikněte na **Konfigurovat**. Vytvoření dvě zásady sdílený přístup jeden jen **Odeslat** s oprávnění **Odeslat** a jeden jen **Poslech** s oprávněními **Poslech** . Až budete hotovi, klepněte na tlačítko **Uložit** dole.

    ![Konfigurace fronty Azure portálu][31]

3. Klikněte na **řídicí panel** na začátku a potom na **informace o připojení** dole. Poznamenejte si řetězce dvě připojení.

    ![Řídicí panel fronty Azure portálu][32]

### <a name="create-the-event-processor"></a>Vytvoření procesor událostí

1. V aktuálním řešení Visual Studiu vytvořte projekt Visual Basic Windows pomocí šablony **Aplikace konzoly** projektu, klikněte na **soubor** > **Přidat** > **Nový projekt**. Nastavit, že je verze .NET Framework 4.5.1 nebo novější. Název projektu **ProcessDeviceToCloudMessages**a klikněte na **OK**.

    ![Nový projekt ve Visual Studiu][10]

2. V Průzkumníku klikněte pravým tlačítkem **ProcessDeviceToCloudMessages** projektu a potom klikněte na **Spravovat balíčků NuGet**. Zobrazí se dialogové okno **Správce balíčků NuGet** .

3. Vyhledejte **WindowsAzure.ServiceBus**, klikněte na tlačítko **instalovat**a přijměte podmínky použití. Tuto operaci ke stažení, instalace a přidá odkaz na [NuGet Bus služby Azure balíčku](https://www.nuget.org/packages/WindowsAzure.ServiceBus)s jeho závislosti.

4. Vyhledejte **Microsoft.Azure.ServiceBus.EventProcessorHost**, klikněte na tlačítko **instalovat**a přijměte podmínky použití. Tuto operaci ke stažení, instalace a přidá odkaz na [Azure služby Bus události rozbočovač - EventProcessorHost NuGet balíčku](https://www.nuget.org/packages/Microsoft.Azure.ServiceBus.EventProcessorHost), se všechny závislostmi.

5. Klikněte pravým tlačítkem **ProcessDeviceToCloudMessages** projektu, klikněte na tlačítko **Přidat**a potom klikněte na **Onenotové**. Pojmenování nové třídy **StoreEventProcessor**a potom klikněte na **OK** vytvořte předmětu.

6. V horní části souboru StoreEventProcessor.cs přidejte následující příkazy:

    ```
    using System.IO;
    using System.Diagnostics;
    using System.Security.Cryptography;
    using Microsoft.ServiceBus.Messaging;
    using Microsoft.WindowsAzure.Storage;
    using Microsoft.WindowsAzure.Storage.Blob;
    ```

7. Nahraďte následující kód pro obsah třídy:

    ```
    class StoreEventProcessor : IEventProcessor
    {
      private const int MAX_BLOCK_SIZE = 4 * 1024 * 1024;
      public static string StorageConnectionString;
      public static string ServiceBusConnectionString;

      private CloudBlobClient blobClient;
      private CloudBlobContainer blobContainer;
      private QueueClient queueClient;

      private long currentBlockInitOffset;
      private MemoryStream toAppend = new MemoryStream(MAX_BLOCK_SIZE);

      private Stopwatch stopwatch;
      private TimeSpan MAX_CHECKPOINT_TIME = TimeSpan.FromHours(1);

      public StoreEventProcessor()
      {
        var storageAccount = CloudStorageAccount.Parse(StorageConnectionString);
        blobClient = storageAccount.CreateCloudBlobClient();
        blobContainer = blobClient.GetContainerReference("d2ctutorial");
        blobContainer.CreateIfNotExists();
        queueClient = QueueClient.CreateFromConnectionString(ServiceBusConnectionString);
      }

      Task IEventProcessor.CloseAsync(PartitionContext context, CloseReason reason)
      {
        Console.WriteLine("Processor Shutting Down. Partition '{0}', Reason: '{1}'.", context.Lease.PartitionId, reason);
        return Task.FromResult<object>(null);
      }

      Task IEventProcessor.OpenAsync(PartitionContext context)
      {
        Console.WriteLine("StoreEventProcessor initialized.  Partition: '{0}', Offset: '{1}'", context.Lease.PartitionId, context.Lease.Offset);

        if (!long.TryParse(context.Lease.Offset, out currentBlockInitOffset))
        {
          currentBlockInitOffset = 0;
        }
        stopwatch = new Stopwatch();
        stopwatch.Start();

        return Task.FromResult<object>(null);
      }

      async Task IEventProcessor.ProcessEventsAsync(PartitionContext context, IEnumerable<EventData> messages)
      {
        foreach (EventData eventData in messages)
        {
          byte[] data = eventData.GetBytes();

          if (eventData.Properties.ContainsKey("messageType") && (string) eventData.Properties["messageType"] == "interactive")
          {
            var messageId = (string) eventData.SystemProperties["message-id"];

            var queueMessage = new BrokeredMessage(new MemoryStream(data));
            queueMessage.MessageId = messageId;
            queueMessage.Properties["messageType"] = "interactive";
            await queueClient.SendAsync(queueMessage);

            WriteHighlightedMessage(string.Format("Received interactive message: {0}", messageId));
            continue;
          }

          if (toAppend.Length + data.Length > MAX_BLOCK_SIZE || stopwatch.Elapsed > MAX_CHECKPOINT_TIME)
          {
            await AppendAndCheckpoint(context);
          }
          await toAppend.WriteAsync(data, 0, data.Length);

          Console.WriteLine(string.Format("Message received.  Partition: '{0}', Data: '{1}'",
            context.Lease.PartitionId, Encoding.UTF8.GetString(data)));
        }
      }

      private async Task AppendAndCheckpoint(PartitionContext context)
      {
        var blockIdString = String.Format("startSeq:{0}", currentBlockInitOffset.ToString("0000000000000000000000000"));
        var blockId = Convert.ToBase64String(ASCIIEncoding.ASCII.GetBytes(blockIdString));
        toAppend.Seek(0, SeekOrigin.Begin);
        byte[] md5 = MD5.Create().ComputeHash(toAppend);
        toAppend.Seek(0, SeekOrigin.Begin);

        var blobName = String.Format("iothubd2c_{0}", context.Lease.PartitionId);
        var currentBlob = blobContainer.GetBlockBlobReference(blobName);

        if (await currentBlob.ExistsAsync())
        {
          await currentBlob.PutBlockAsync(blockId, toAppend, Convert.ToBase64String(md5));
          var blockList = await currentBlob.DownloadBlockListAsync();
          var newBlockList = new List<string>(blockList.Select(b => b.Name));

          if (newBlockList.Count() > 0 && newBlockList.Last() != blockId)
          {
            newBlockList.Add(blockId);
            WriteHighlightedMessage(String.Format("Appending block id: {0} to blob: {1}", blockIdString, currentBlob.Name));
          }
          else
          {
            WriteHighlightedMessage(String.Format("Overwriting block id: {0}", blockIdString));
          }
          await currentBlob.PutBlockListAsync(newBlockList);
        }
        else
        {
          await currentBlob.PutBlockAsync(blockId, toAppend, Convert.ToBase64String(md5));
          var newBlockList = new List<string>();
          newBlockList.Add(blockId);
          await currentBlob.PutBlockListAsync(newBlockList);

          WriteHighlightedMessage(String.Format("Created new blob", currentBlob.Name));
        }

        toAppend.Dispose();
        toAppend = new MemoryStream(MAX_BLOCK_SIZE);

        // checkpoint.
        await context.CheckpointAsync();
        WriteHighlightedMessage(String.Format("Checkpointed partition: {0}", context.Lease.PartitionId));

        currentBlockInitOffset = long.Parse(context.Lease.Offset);
        stopwatch.Restart();
      }

      private void WriteHighlightedMessage(string message)
      {
        Console.ForegroundColor = ConsoleColor.Yellow;
        Console.WriteLine(message);
        Console.ResetColor();
      }
    }
    ```

    Třídy **EventProcessorHost** hovorů této třídy zpracuje zařízení do cloudu zprávy přijaté od IoT centrální. Kód v této třídy používá logických ukládání zpráv problémy se spolehlivým v kontejneru objektů blob a předávání interaktivní zpráv do fronty Bus služby.

    Metoda **OpenAsync** inicializuje proměnné **currentBlockInitOffset** sleduje aktuální odsazení první zprávu jen pro čtení procesoru události. Myslete na to, že procesorech je zodpovědný za jeden oddíl.

    Metoda **ProcessEventsAsync** dostává sadu zpráv z centrální IoT a zpracuje takto: přesměruje interaktivní zprávy do fronty Bus služby a připojí zprávy bodů dat s názvem **toAppend**vyrovnávací paměť. Pokud vyrovnávací paměť dosáhne 4 MB limit nebo odinstalaci duplikace čas windows uplyne (hodinu po kontrolních bodů v tomto kurzu), aplikaci spustí kontrolní bod.

    Metoda **AppendAndCheckpoint** nejdřív vygeneruje blockId bloku chcete připojit. Azure úložiště vyžaduje všechny blokovat ID mít stejnou délku tak metodu ohraničí posun počátečními nulami - `currentBlockInitOffset.ToString("0000000000000000000000000")`. Potom pokud blok s tímto ID je už objektů blob, metodu přepíše ho aktuální obsah vyrovnávací paměť.

    > [AZURE.NOTE] Zjednodušit kód, používá tento kurz jednoho objektů blob na oddíl pro uložení zpráv. Skutečné řešení implementovat postupných vytvořením další soubory za určitou dobu nebo když dostanete určitou velikost souboru. Myslete na to, že objektů blob Azure bloku může obsahovat maximálně 195 GB data.

8. Ve třídě **Program** přidejte tento příkaz **pomocí** nahoře:

    ```
    using Microsoft.ServiceBus.Messaging;
    ```

9. Upravte metodu **hlavní** ve třídě **Program** následujícím způsobem. Nahraďte **{iot centrální připojovací řetězec}** **iothubowner** připojovacího řetězce z kurz [Seznámení s IoT centrální] . Nahraďte připojovací řetězec úložiště připojovací řetězec, který je uvedeno na začátek této části. Nahraďte připojovací řetězec služby Bus oprávnění **posílat** fronty s názvem **d2ctutorial** poznamenat na začátek této části:

    ```
    static void Main(string[] args)
    {
      string iotHubConnectionString = "{iot hub connection string}";
      string iotHubD2cEndpoint = "messages/events";
      StoreEventProcessor.StorageConnectionString = "{storage connection string}";
      StoreEventProcessor.ServiceBusConnectionString = "{service bus send connection string}";

      string eventProcessorHostName = Guid.NewGuid().ToString();
      EventProcessorHost eventProcessorHost = new EventProcessorHost(eventProcessorHostName, iotHubD2cEndpoint, EventHubConsumerGroup.DefaultGroupName, iotHubConnectionString, StoreEventProcessor.StorageConnectionString, "messages-events");
      Console.WriteLine("Registering EventProcessor...");
      eventProcessorHost.RegisterEventProcessorAsync<StoreEventProcessor>().Wait();

      Console.WriteLine("Receiving. Press enter key to stop worker.");
      Console.ReadLine();
      eventProcessorHost.UnregisterEventProcessorAsync().Wait();
    }
    ```

    > [AZURE.NOTE] Za účelem zjednodušení používá tento kurz jedna instance třídy [EventProcessorHost] . Další informace najdete v tématu [Průvodce rozbočovače programování událostí].

## <a name="receive-interactive-messages"></a>Interaktivní zprávy
V této části napíšete aplikace konzoly Windows, která přijímá interaktivní zprávy ve frontě Bus služby. Další informace o tom, jak Architektonické řešení pomocí služby Bus najdete v článku [vytvoření vícevrstvého aplikace služby Bus][].

1. V aktuálním řešení Visual Studiu vytvořte projekt Visual Basic Windows pomocí šablony **Aplikace konzoly** projektu. Zadejte název projektu **ProcessD2CInteractiveMessages**.

2. V Průzkumníku klikněte pravým tlačítkem **ProcessD2CInteractiveMessages** projektu a potom klikněte na **Spravovat balíčků NuGet**. Tuto operaci zobrazí okno **Správce balíčků NuGet** .

3. Vyhledejte **WindowsAzure.ServiceBus**, klikněte na tlačítko **instalovat**a přijměte podmínky použití. Tuto operaci ke stažení, instalace a přidá odkaz na [Bus služby Azure](https://www.nuget.org/packages/WindowsAzure.ServiceBus)s jeho závislosti.

4. Přidejte následující příkazy **pomocí** v horní části souboru **Program.cs** :

    ```
    using System.IO;
    using Microsoft.ServiceBus.Messaging;
    ```

5. Nakonec přidejte následující řádky metody **hlavní** . Nahraďte připojovací řetězec s **Poslech** oprávnění s názvem **d2ctutorial**fronty:

    ```
    Console.WriteLine("Process D2C Interactive Messages app\n");

    string connectionString = "{service bus listen connection string}";
    QueueClient Client = QueueClient.CreateFromConnectionString(connectionString);

    OnMessageOptions options = new OnMessageOptions();
    options.AutoComplete = false;
    options.AutoRenewTimeout = TimeSpan.FromMinutes(1);

    Client.OnMessage((message) =>
    {
      try
      {
        var bodyStream = message.GetBody<Stream>();
        bodyStream.Position = 0;
        var bodyAsString = new StreamReader(bodyStream, Encoding.ASCII).ReadToEnd();

        Console.WriteLine("Received message: {0} messageId: {1}", bodyAsString, message.MessageId);

        message.Complete();
      }
      catch (Exception)
      {
        message.Abandon();
      }
    }, options);

    Console.WriteLine("Receiving interactive messages from SB queue...");
    Console.WriteLine("Press any key to exit.");
    Console.ReadLine();
    ```

## <a name="run-the-applications"></a>Spuštění aplikace

Teď jste připraveni ke spuštění aplikace.

1.  Ve Visual Studiu v Průzkumníku řešení pravým tlačítkem vyberte **Nastavení spuštění projekty**. Výběr **více projektů po spuštění**a pak vyberte **Spustit** jako akce pro projekty **ProcessDeviceToCloudMessages**, **SimulatedDevice**a **ProcessD2CInteractiveMessages** .

2.  Stisknutím klávesy **F5** spuštění tři konzoly aplikací. Aplikace **ProcessD2CInteractiveMessages** zpracovat všechny interaktivní zprávy odeslané z aplikace **SimulatedDevice** .

  ![Tři konzoly aplikace][50]

> [AZURE.NOTE] Pokud chcete zobrazit aktualizace vaší objektů blob, budete muset zmenšit konstanta **MAX_BLOCK_SIZE** ve třídě **StoreEventProcessor** menší hodnotu, například **1 024**. Tato změna je užitečná, protože trvá určitou dobu kontaktovat limit velikosti blok s daty odeslaný simulovaný zařízení. Menší velikosti blok nepotřebujete až za dlouhou zobrazíte objektů blob vytvořit a aktualizovat. Ale s větší velikostí blok způsobí, že aplikace větší škálovatelnost.

## <a name="next-steps"></a>Další kroky

V tomto kurzu se naučíte problémy se spolehlivým procesu datového bodu a interaktivní zprávy zařízení do cloudu pomocí [EventProcessorHost] předmětu.

[Odeslání zprávy cloudu zařízení s IoT centrální] [ lnk-c2d] se dozvíte, jak posílat zprávy do svého zařízení ze svého back-end.

Příklady úplné řešení začátku do konce, které používají IoT centrální najdete v tématu [Azure IoT sadu][lnk-suite].

Další informace o vytváření řešení s IoT centrální, najdete v článku [IoT centrální vývojář Průvodce].

<!-- Images. -->
[50]: ./media/iot-hub-csharp-csharp-process-d2c/run1.png
[10]: ./media/iot-hub-csharp-csharp-process-d2c/create-identity-csharp1.png

[30]: ./media/iot-hub-csharp-csharp-process-d2c/createqueue2.png
[31]: ./media/iot-hub-csharp-csharp-process-d2c/createqueue3.png
[32]: ./media/iot-hub-csharp-csharp-process-d2c/createqueue4.png

<!-- Links -->

[Úložiště objektů blob Azure]: ../storage/storage-dotnet-how-to-use-blobs.md
[Azure Data Factory]: https://azure.microsoft.com/documentation/services/data-factory/
[HDInsight (Hadoop)]: https://azure.microsoft.com/documentation/services/hdinsight/
[Služba Bus fronty]: ../service-bus-messaging/service-bus-dotnet-get-started-with-queues.md

[Azure IoT centrální vývojář Průvodce - zařízení do cloudu]: iot-hub-devguide-messaging.md

[Azure úložiště]: https://azure.microsoft.com/documentation/services/storage/
[Bus služby Azure]: https://azure.microsoft.com/documentation/services/service-bus/

[Příručka IoT centrální vývojář]: iot-hub-devguide.md
[Začínáme s IoT centrální]: iot-hub-csharp-csharp-getstarted.md
[Středisko pro vývojáře IoT Azure]: https://azure.microsoft.com/develop/iot
[lnk-service-fabric]: https://azure.microsoft.com/documentation/services/service-fabric/
[lnk-stream-analytics]: https://azure.microsoft.com/documentation/services/stream-analytics/
[lnk-event-hubs]: https://azure.microsoft.com/documentation/services/event-hubs/
[Zpracování přechodná poruch]: https://msdn.microsoft.com/library/hh675232.aspx

<!-- Links -->
[Informace o Azure úložiště]: ../storage/storage-create-storage-account.md#create-a-storage-account
[Začínáme s rozbočovače události]: ../event-hubs/event-hubs-csharp-ephcs-getstarted.md
[Azure úložiště škálovatelnost pravidla]: ../storage/storage-scalability-targets.md
[Azure Block Blobs]: https://msdn.microsoft.com/library/azure/ee691964.aspx
[Event Hubs]: ../event-hubs/event-hubs-overview.md
[EventProcessorHost]: http://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventprocessorhost(v=azure.95).aspx
[Událost rozbočovače programování Průvodce]: ../event-hubs/event-hubs-programming-guide.md
[Zpracování přechodná poruch]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx
[Vytvoření vícevrstvého aplikace služby Bus]: ../service-bus-messaging/service-bus-dotnet-multi-tier-app-using-service-bus-queues.md

[lnk-classic-portal]: https://manage.windowsazure.com
[lnk-c2d]: iot-hub-csharp-csharp-process-d2c.md
[lnk-suite]: https://azure.microsoft.com/documentation/suites/iot-suite/
