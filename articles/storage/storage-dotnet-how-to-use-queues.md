<properties
    pageTitle="Začínáme s úložištěm fronty Azure pomocí .NET | Microsoft Azure"
    description="Azure fronty poskytují spolehlivé, asynchronní zasílání zpráv mezi součástí aplikace. Cloud zpráv umožňují součástí aplikace zobrazit nezávisle na sobě."
    services="storage"
    documentationCenter=".net"
    authors="robinsh"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="hero-article"
    ms.date="10/12/2016"
    ms.author="robinsh"/>

# <a name="get-started-with-azure-queue-storage-using-net"></a>Začínáme s úložištěm fronty Azure pomocí .NET

[AZURE.INCLUDE [storage-selector-queue-include](../../includes/storage-selector-queue-include.md)]
<br/>
[AZURE.INCLUDE [storage-try-azure-tools-queues](../../includes/storage-try-azure-tools-queues.md)]

## <a name="overview"></a>Základní informace

Azure úložiště fronty obsahuje cloudu zasílání zpráv mezi součástí aplikace. Při návrhu aplikace pro měřítko, součástí aplikace jsou často oddělené, takže lze přizpůsobit nezávisle na sobě. Úložiště fronty poskytuje asynchronní zasílání zpráv pro komunikaci mezi součástí aplikace, zda jsou spuštěné v cloudu, na stolním počítači, na místního serveru nebo na mobilním zařízení. Úložiště fronty také podporují správu asynchronní úkoly a vytváření pracovních postupů práce.

### <a name="about-this-tutorial"></a>O tomto kurzu

Tento kurz ukazuje, jak napsat kód .NET pro některé běžné scénáře použití Azure fronty úložiště. Scénáře nichž se uplatní zahrnují vytváření a odstraňování fronty a přidání, čtení a odstraňování zpráv.

**Předpokládaná doba dokončete:** 45 minut

**Prerequisities:**

- [Microsoft Visual Studio](https://www.visualstudio.com/en-us/visual-studio-homepage-vs.aspx)
- [Knihovna Azure úložiště klienta pro .NET](https://www.nuget.org/packages/WindowsAzure.Storage/)
- [Azure Správce konfigurace pro .NET](https://www.nuget.org/packages/Microsoft.WindowsAzure.ConfigurationManager/)
- [Účet Azure úložiště](storage-create-storage-account.md#create-a-storage-account)


[AZURE.INCLUDE [storage-dotnet-client-library-version-include](../../includes/storage-dotnet-client-library-version-include.md)]

[AZURE.INCLUDE [storage-queue-concepts-include](../../includes/storage-queue-concepts-include.md)]

[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

[AZURE.INCLUDE [storage-development-environment-include](../../includes/storage-development-environment-include.md)]

### <a name="add-namespace-declarations"></a>Přidání deklarace oboru názvů

Přidejte následující `using` příkazů do horní části `program.cs` souboru:

    using Microsoft.Azure; // Namespace for CloudConfigurationManager
    using Microsoft.WindowsAzure.Storage; // Namespace for CloudStorageAccount
    using Microsoft.WindowsAzure.Storage.Queue; // Namespace for Queue storage types

### <a name="parse-the-connection-string"></a>Analyzovat připojovacího řetězce

[AZURE.INCLUDE [storage-cloud-configuration-manager-include](../../includes/storage-cloud-configuration-manager-include.md)]

### <a name="create-the-queue-service-client"></a>Vytvoření klienta služby fronty

**CloudQueueClient** předmětu umožňuje načítat fronty uložené v úložišti fronty. Tady je jedním ze způsobů vytvoření klienta služby:

    CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();

Teď jste připraveni zadat kód, který načítá data z a zápisu dat do fronty úložiště.

## <a name="create-a-queue"></a>Vytvoření fronty

Tento příklad ukazuje, jak vytvořit fronty, pokud už neexistuje:

    // Retrieve storage account from connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the queue client.
    CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();

    // Retrieve a reference to a container.
    CloudQueue queue = queueClient.GetQueueReference("myqueue");

    // Create the queue if it doesn't already exist
    queue.CreateIfNotExists();

## <a name="insert-a-message-into-a-queue"></a>Vložte do zprávy do fronty

Vložit zprávu do existující fronty, nejprve vytvořte nové **CloudQueueMessage**. Dále zavolejte metodu **AddMessage** . **CloudQueueMessage** může vytvořit řetězec (ve formátu UTF-8) nebo pole **bajtů** . Tady je kód, který vytvoří fronty (pokud neexistuje) a vloží se zpráva "Vítáme,":

    // Retrieve storage account from connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the queue client.
    CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();

    // Retrieve a reference to a queue.
    CloudQueue queue = queueClient.GetQueueReference("myqueue");

    // Create the queue if it doesn't already exist.
    queue.CreateIfNotExists();

    // Create a message and add it to the queue.
    CloudQueueMessage message = new CloudQueueMessage("Hello, World");
    queue.AddMessage(message);

## <a name="peek-at-the-next-message"></a>Náhled na další zprávu

Zpráva začátku fronty můžete prohlížet bez odebrání ve frontě tak, že zavoláte metodu **PeekMessage** .

    // Retrieve storage account from connection string
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the queue client
    CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();

    // Retrieve a reference to a queue
    CloudQueue queue = queueClient.GetQueueReference("myqueue");

    // Peek at the next message
    CloudQueueMessage peekedMessage = queue.PeekMessage();

    // Display message.
    Console.WriteLine(peekedMessage.AsString);

## <a name="change-the-contents-of-a-queued-message"></a>Změnit obsah zprávy ve frontě

Můžete změnit obsah zprávy v místě ve frontě. Pokud se zpráva představuje úkolu práce, může pomocí této funkce můžete aktualizovat stav pracovního úkolu. Následující kód aktualizuje zprávy fronty nový obsah a nastavuje časový limit viditelnost prodloužit jiný 60 sekund. Uloží stav práce přidružené ke zprávě a poskytuje klienta jiné minutu pokračovat v práci na zprávu. Tento postup můžete použít ke sledování pracovní postupy vícekrokové fronty zpráv, aniž byste museli začít od začátku, pokud krok zpracování selže kvůli chybě hardware a software. Obvykle byste měli mít počet opakování i a pokud zprávy je možné více než *n* časy, by ho odstranit. Je to ochrana proti zprávu, která spustí pokaždé, když je zpracování chyby aplikace.

    // Retrieve storage account from connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the queue client.
    CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();

    // Retrieve a reference to a queue.
    CloudQueue queue = queueClient.GetQueueReference("myqueue");

    // Get the message from the queue and update the message contents.
    CloudQueueMessage message = queue.GetMessage();
    message.SetMessageContent("Updated contents.");
    queue.UpdateMessage(message,
        TimeSpan.FromSeconds(60.0),  // Make it visible for another 60 seconds.
        MessageUpdateFields.Content | MessageUpdateFields.Visibility);

## <a name="de-queue-the-next-message"></a>Zrušit fronty další zprávu

Kód zrušte fronty zprávu z fronty ve dvou krocích. Při volání **GetMessage**se zobrazí následující zpráva ve frontě. Zprávy vrácených **GetMessage** přestane být viditelné pro jiné kódy čtení zpráv z této fronty. Ve výchozím nastavení tato zpráva zobrazena neviditelné 30 sekund. Dokončete zprávu odebráním fronty musíte také zavolat **DeleteMessage**. Krocích popsaných odebrání zprávy zajišťuje, že pokud kódu nepovede zpracuje zprávy kvůli chybě hardware a software, jiné instanci aplikace kódu můžete zobrazí stejná zpráva a začněte odznovu. Váš kód volá **DeleteMessage** vpravo po zpracování zprávy.

    // Retrieve storage account from connection string
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the queue client
    CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();

    // Retrieve a reference to a queue
    CloudQueue queue = queueClient.GetQueueReference("myqueue");

    // Get the next message
    CloudQueueMessage retrievedMessage = queue.GetMessage();

    //Process the message in less than 30 seconds, and then delete the message
    queue.DeleteMessage(retrievedMessage);

## <a name="use-async-await-pattern-with-common-queue-storage-apis"></a>Použití vzorku očekávat asynchronní s běžné úložištěm fronty rozhraní API

Tento příklad ukazuje, jak použít vzorek očekávat asynchronní běžné fronty úložiště rozhraní API. Vzorku hovorů asynchronní verzi všech metody dané podle *asynchronní* přípony obou metod. Při použití metody asynchronní, asynchronní-očekávat vzorek místní spuštění pozastaví až do dokončení volání. Toto chování umožňuje aktuální vlákna dělat další práci, která pomáhá zabránit kritické a zlepšit celkový rychlostí reakce aplikace. Další informace o použití vzorku asynchronní Await v .NET najdete v článku [asynchronní a Await (C# a Visual Basic)](https://msdn.microsoft.com/library/hh191443.aspx)

    // Create the queue if it doesn't already exist
    if(await queue.CreateIfNotExistsAsync())
    {
        Console.WriteLine("Queue '{0}' Created", queue.Name);
    }
    else
    {
        Console.WriteLine("Queue '{0}' Exists", queue.Name);
    }

    // Create a message to put in the queue
    CloudQueueMessage cloudQueueMessage = new CloudQueueMessage("My message");

    // Async enqueue the message
    await queue.AddMessageAsync(cloudQueueMessage);
    Console.WriteLine("Message added");

    // Async dequeue the message
    CloudQueueMessage retrievedMessage = await queue.GetMessageAsync();
    Console.WriteLine("Retrieved message with content '{0}'", retrievedMessage.AsString);

    // Async delete the message
    await queue.DeleteMessageAsync(retrievedMessage);
    Console.WriteLine("Deleted message");

## <a name="leverage-additional-options-for-de-queuing-messages"></a>Využít další možnosti pro vypnutí řazení zpráv

Je možné upravit načítání zpráv z fronty dvěma způsoby:
Nejdřív můžete získat dávku zprávy (až 32). Za druhé můžete nastavit časový limit prodloužit nebo zkrátit invisibility umožňující kódu více nebo méně času plně zpracuje každou zprávu. Následující příklad používá metodu **GetMessages** získat 20 zprávy v jednom volání. Potom zpracuje každou zprávu pomocí smyčku **foreach** . Také nastaví časový limit invisibility pět minut pro každou zprávu. Všimněte si, že 5 minut spustí pro všechny zprávy ve stejnou dobu, tak až poté, co jste 5 minut uplynuly volání **GetMessages**, zprávy, které nebyly odstraněny se bude zobrazovat znovu.

    // Retrieve storage account from connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

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

Odhad počet zpráv může vstoupit do fronty. Metoda **FetchAttributes** zeptá služba fronty načtení fronty atributů, včetně počtu zpráv. Vlastnost **ApproximateMessageCount** vrátí poslední hodnotu načítané metodu **FetchAttributes** bez volání služba fronty.

    // Retrieve storage account from connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the queue client.
    CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();

    // Retrieve a reference to a queue.
    CloudQueue queue = queueClient.GetQueueReference("myqueue");

    // Fetch the queue attributes.
    queue.FetchAttributes();

    // Retrieve the cached approximate message count.
    int? cachedMessageCount = queue.ApproximateMessageCount;

    // Display number of messages.
    Console.WriteLine("Number of messages in queue: " + cachedMessageCount);

## <a name="delete-a-queue"></a>Odstranění fronty

Pokud chcete odstranit fronty a všech zpráv, na které se nacházejí v ní, kontaktujte metodu **Odstranit** fronty objektu.

    // Retrieve storage account from connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the queue client.
    CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();

    // Retrieve a reference to a queue.
    CloudQueue queue = queueClient.GetQueueReference("myqueue");

    // Delete the queue.
    queue.Delete();

## <a name="next-steps"></a>Další kroky

Teď, když jste se naučili základy fronty úložiště, tyto odkazy vedou na další informace o složitější úlohy úložiště.

- Zobrazte odkaz dokumentaci fronty úplné podrobnosti o dostupných rozhraní API:
    - [Knihovna úložiště klienta pro referenci .NET](http://go.microsoft.com/fwlink/?LinkID=390731&clcid=0x409)
    - [Odkaz rozhraní REST API](http://msdn.microsoft.com/library/azure/dd179355)
- Zjistěte, jak zjednodušit kód, který napíšete pro práci s úložištěm Azure pomocí [Azure WebJobs SDK](../app-service-web/websites-dotnet-webjobs-sdk.md).
- Zobrazte další funkce příručky pro další informace o další možnosti pro ukládání dat v Azure.
    - [Začínáme s úložiště tabulek Azure pomocí .NET](storage-dotnet-how-to-use-tables.md) pro ukládání strukturovaná data.
    - [Začínáme s úložiště objektů Blob Azure pomocí .NET](storage-dotnet-how-to-use-blobs.md) pro ukládání Nestrukturovaná data.
    - [Připojení k databázi SQL pomocí .NET (C#)](../sql-database/sql-database-develop-dotnet-simple.md) pro ukládání relačních dat.

  [Download and install the Azure SDK for .NET]: /develop/net/
  [.NET client library reference]: http://go.microsoft.com/fwlink/?LinkID=390731&clcid=0x409
  [Creating a Azure Project in Visual Studio]: http://msdn.microsoft.com/library/azure/ee405487.aspx
  [Azure Storage Team Blog]: http://blogs.msdn.com/b/windowsazurestorage/
  [OData]: http://nuget.org/packages/Microsoft.Data.OData/5.0.2
  [Edm]: http://nuget.org/packages/Microsoft.Data.Edm/5.0.2
  [Spatial]: http://nuget.org/packages/System.Spatial/5.0.2
