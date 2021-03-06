<properties
    pageTitle="Začínáme s úložiště fronty a Visual Studio připojené služby (ASP.NET) | Microsoft Azure"
    description="Jak začít používat úložiště Azure fronty v projektu ASP.NET ve Visual Studiu po připojení k úložišti účtu pomocí aplikace Visual Studio připojené služby"
    services="storage"
    documentationCenter=""
    authors="TomArcher"
    manager="douge"
    editor=""/>

<tags
    ms.service="storage"
    ms.workload="web"
    ms.tgt_pltfrm="vs-getting-started"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/15/2016"
    ms.author="tarcher"/>

# <a name="get-started-with-azure-queue-storage-and-visual-studio-connected-services"></a>Začínáme s Azure fronty úložiště a Visual Studio připojené služby

[AZURE.INCLUDE [storage-try-azure-tools-queues](../../includes/storage-try-azure-tools-queues.md)]

## <a name="overview"></a>Základní informace

Tento článek popisuje, jak začít používat úložiště Azure fronty ve Visual Studiu, když jste vytvořili nebo odkazuje účet Azure úložiště v projektu ASP.NET pomocí dialogového okna aplikace Visual Studio **Přidat připojené služby** .

Ukážeme vám jak můžete vytvářet a používat Azure fronty ve vašem účtu úložiště. Také Ukážeme vám jak provádět základní fronty operací, jako je třeba přidávání, úpravy, čtení a odstraňování zpráv. Vzorky napsané v jazyce C# kód a používat [Microsoft Azure úložiště klienta knihovny pro .NET](https://msdn.microsoft.com/library/azure/dn261237.aspx). Další informace o ASP.NET najdete v tématu [ASP.NET](http://www.asp.net).

Azure fronty úložiště je služba pro ukládání většího počtu zpráv, které jsou přístupné z kdekoli na světě prostřednictvím ověřené volání pomocí protokolu HTTP nebo HTTPS. Jedna fronta zprávy můžou obsahovat až 64 KB a fronty může obsahovat miliony zprávy až limit celkovou kapacitu úložiště účtu.

## <a name="access-queues-in-code"></a>Access fronty v kódu

Přístup k fronty ASP.NET projektů, potřebujete zahrnout všechny zdrojový soubor C# následující položky, které získat přístup k úložišti fronty Azure.

1. Zkontrolujte, že oboru názvů prohlášení v horní části souboru C# obsahuje tyto příkazy **pomocí** .

        using Microsoft.Framework.Configuration;
        using Microsoft.WindowsAzure.Storage;
        using Microsoft.WindowsAzure.Storage.Queue;

2. Pokud potřebujete **CloudStorageAccount** objekt, který představuje informací o účtu úložiště. Pomocí následující kód úložiště připojovacího řetězce a informace o účtu úložiště z konfiguraci služby Azure.

         CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
           CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));

3. Získejte objekt **CloudQueueClient** neodkazuje objekty ve vašem účtu úložiště.  

        // Create the CloudQueueClient object for this storage account.
        CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();

4. Získejte objekt **CloudQueue** neodkazuje konkrétní fronty.

        // Get a reference to a queue named "messageQueue"
        CloudQueue messageQueue = queueClient.GetQueueReference("messageQueue");


**Poznámka:** Pomocí výše uvedených kód před kód v následujících vzorcích.

## <a name="create-a-queue-in-code"></a>Vytvoření fronty v kódu

Vytvoření Azure fronty v kódu, stačí dodejte volání **CreateIfNotExists** kódu výše.

    // Create the messageQueue if it does not exist
    messageQueue.CreateIfNotExists();

## <a name="add-a-message-to-a-queue"></a>Přidejte zprávu do fronty

Vložit zprávu do existující fronty, vytvořte nový objekt **CloudQueueMessage** a potom volat metodu **AddMessage** .

Objekt **CloudQueueMessage** může vytvořit řetězec (ve formátu UTF-8) nebo pole bajtů.

Tady je příklad, který vloží zprávu "Vítáme,".

    // Create a message and add it to the queue.
    CloudQueueMessage message = new CloudQueueMessage("Hello, World");
    messageQueue.AddMessage(message);

## <a name="read-a-message-in-a-queue"></a>Číst zprávy ve frontě

Zpráva začátku fronty můžete prohlížet bez odebrání ve frontě tak, že zavoláte metodu PeekMessage().

    // Peek at the next message
    CloudQueueMessage peekedMessage = messageQueue.PeekMessage();

## <a name="read-and-remove-a-message-in-a-queue"></a>Čtení a odebrání zprávy ve frontě

Kódu můžete odstranit (deaktivovat fronty) na zprávu od fronty ve dvou krocích.
1. Zavolejte na linku GetMessage() získat další zprávu ve frontě. Zprávy vrácených GetMessage() přestane být viditelné pro jiné kódy čtení zpráv z této fronty. Ve výchozím nastavení tato zpráva zobrazena neviditelné 30 sekund.
2.  Dokončete zprávu odebráním fronty volání **DeleteMessage**.

Krocích popsaných odebrání zprávy zajišťuje, že pokud kódu nepovede zpracuje zprávy kvůli chybě hardware a software, jiné instanci aplikace kódu můžete zobrazí stejná zpráva a začněte odznovu. Následující kód volá **DeleteMessage** vpravo po zpracování zprávy.

    // Get the next message in the queue.
    CloudQueueMessage retrievedMessage = messageQueue.GetMessage();

    // Process the message in less than 30 seconds

    // Then delete the message.
    await messageQueue.DeleteMessage(retrievedMessage);


## <a name="use-additional-options-for-de-queuing-messages"></a>Používat další možnosti pro vypnutí řazení zpráv

Je možné upravit načítání zpráv z fronty dvěma způsoby:
Nejdřív můžete získat dávku zprávy (až 32). Za druhé můžete nastavit časový limit prodloužit nebo zkrátit invisibility umožňující kódu více nebo méně času plně zpracuje každou zprávu. Následující příklad používá metodu **GetMessages** získat 20 zprávy v jednom volání. Potom zpracuje každou zprávu pomocí smyčku **foreach** . Také nastaví časový limit invisibility pět minut pro každou zprávu. Všimněte si, že 5 minut spustí pro všechny zprávy ve stejnou dobu, tak až poté, co jste 5 minut uplynuly volání **GetMessages**, zprávy, které nebyly odstraněny se bude zobrazovat znovu.

    // Create the queue client.
    CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();

    // Retrieve a reference to a queue.
    CloudQueue queue = queueClient.GetQueueReference("myqueue");

    foreach (CloudQueueMessage message in queue.GetMessages(20, TimeSpan.FromMinutes(5)))
    {
        // Process all messages in less than 5 minutes, deleting each message after processing.
        queue.DeleteMessage(message);
    }

## <a name="get-the-queue-length"></a>Získání délka fronty

Odhad počet zpráv může vstoupit do fronty. Metoda **FetchAttributes** zeptá queueservice načtení fronty atributů, včetně počtu zpráv. Vlastnost **ApproximateMethodCount** vrátí poslední hodnotu načítané metodu **FetchAttributes** bez volání queueservice.

    // Fetch the queue attributes.
    messageQueue.FetchAttributes();

    // Retrieve the cached approximate message count.
    int? cachedMessageCount = messageQueue.ApproximateMessageCount;

    // Display number of messages.
    Console.WriteLine("Number of messages in queue: " + cachedMessageCount);

## <a name="use-async-await-pattern-with-common-queueapis"></a>Použití vzorek běžné queueAPIs asynchronní očekávat

Tento příklad ukazuje, jak pomocí běžné queueAPIs vzorek asynchronní očekávat. Volá ukázková verze asynchronní každého z dané metody, to je viditelná po oprava asynchronní z obou metod. Při použití metody asynchronní asynchronní-očekávat vzorek místní spuštění pozastaví až do dokončení volání. Toto chování umožňuje aktuální vlákna dělat další práci, která pomáhá zabránit kritické a zlepšit celkový rychlostí reakce aplikace. Další informace o použití vzorku asynchronní Await v .NET najdete v článku [asynchronní a Await (C# a Visual Basic)] (https://msdn.microsoft.com/library/hh191443.aspx)

    // Create a message to put in the queue
    CloudQueueMessage cloudQueueMessage = new CloudQueueMessage("My message");

    // Async enqueue the message
    await messageQueue.AddMessageAsync(cloudQueueMessage);
    Console.WriteLine("Message added");

    // Async dequeue the message
    CloudQueueMessage retrievedMessage = await messageQueue.GetMessageAsync();
    Console.WriteLine("Retrieved message with content '{0}'", retrievedMessage.AsString);

    // Async delete the message
    await messageQueue.DeleteMessageAsync(retrievedMessage);
    Console.WriteLine("Deleted message");

## <a name="delete-a-queue"></a>Odstranění fronty

Pokud chcete odstranit fronty a všech zpráv, na které se nacházejí v ní, kontaktujte metodu **Odstranit** fronty objektu.

    // Delete the queue.
    messageQueue.Delete();

## <a name="next-steps"></a>Další kroky

[AZURE.INCLUDE [vs-storage-dotnet-queues-next-steps](../../includes/vs-storage-dotnet-queues-next-steps.md)]