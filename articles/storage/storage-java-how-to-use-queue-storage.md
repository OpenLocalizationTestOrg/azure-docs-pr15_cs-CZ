<properties
    pageTitle="Použití úložiště fronty z Java | Microsoft Azure"
    description="Naučte se používat službu Azure fronty můžete vytvářet a odstraňovat fronty, vložení, získat a odstraňování zpráv. Vzorky napsané v Java."
    services="storage"
    documentationCenter="java"
    authors="robinsh"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="Java"
    ms.topic="article"
    ms.date="08/11/2016"
    ms.author="robinsh"/>

# <a name="how-to-use-queue-storage-from-java"></a>Použití úložiště fronty z Java

[AZURE.INCLUDE [storage-selector-queue-include](../../includes/storage-selector-queue-include.md)]
<br/>
[AZURE.INCLUDE [storage-try-azure-tools-queues](../../includes/storage-try-azure-tools-queues.md)]

## <a name="overview"></a>Základní informace

Tento průvodce vám ukáže, jak provádět běžné scénáře použití úložiště služby Azure fronty. Vzorky psaných Java a použít v [Azure úložiště SDK jazyka Java][]. Scénáře nichž se uplatní zahrnují **Vložení** **prohlížení**, **začíná**a **odstraňování** zpráv, jakož i **vytváření** a **odstraňování** fronty. Další informace o frontách naleznete v části [Další kroky](#Next-Steps) .

Poznámky: Sadu SDK je k dispozici pro vývojáře, kteří používají Azure úložiště na zařízení s Androidem. Další informace najdete v tématu [Azure SDK úložiště pro Android][].

[AZURE.INCLUDE [storage-queue-concepts-include](../../includes/storage-queue-concepts-include.md)]

[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="create-a-java-application"></a>Vytvoření aplikace Java

V této příručce použije funkce úložiště, které lze provádět v aplikaci Java místně nebo v kódu spuštění v rámci webu nebo pracovního role v Azure.

Chcete-li provést, musíte si nainstalovat Java Development Kit (JDK) a vytvořit účet Azure úložiště v Azure předplatné. Až budete mít Hotovo tak, je třeba ověřit, jestli vývoj systému splňuje minimální požadavky a závislosti, které jsou uvedené v úložišti [Azure úložiště SDK jazyka Java][] na GitHub. Pokud počítač splňuje tyto požadavky splnit, můžete postupovat podle pokynů ke stažení a instalaci knihoven úložiště Azure jazyka Java systému z této úložiště. Po dokončení těchto úkolů, budete moct vytvářet aplikace Java, který využívá příkladech v tomto článku.

## <a name="configure-your-application-to-access-queue-storage"></a>Konfigurace aplikace pro přístup k úložišti fronty

Přidejte následující příkazy importovat do horní části souboru Java místo, kam chcete použít Azure úložiště rozhraní API pro přístup k fronty:

    // Include the following imports to use queue APIs.
    import com.microsoft.azure.storage.*;
    import com.microsoft.azure.storage.queue.*;

## <a name="setup-an-azure-storage-connection-string"></a>Nastavení připojovací řetězec Azure úložiště

Azure úložiště klienta používá k ukládání koncové body a přihlašovací údaje pro přístup k dat správu přístupových připojovací řetězec úložiště. Při spuštění v klientské aplikaci, je nutné zadat připojovací řetězec úložiště ve formátu následující nazvanou podle této účtu úložiště a primární přístupová klávesa úložiště účtu uvedený na [Portálu Azure](https://portal.azure.com) pro hodnoty *název účtu* a *AccountKey* . Tento příklad ukazuje, jak můžou deklarovat statické pole pro uložení připojovací řetězec:

    // Define the connection-string with your values.
    public static final String storageConnectionString =
        "DefaultEndpointsProtocol=http;" +
        "AccountName=your_storage_account;" +
        "AccountKey=your_storage_account_key";

V aplikaci spuštění v rámci role v Microsoft Azure tento řetězec mohou být uloženy v souboru konfigurace služby *ServiceConfiguration.cscfg*a dají se otevřít hovoru metody **RoleEnvironment.getConfigurationSettings** . Tady je příklad získání připojovacího řetězce z **Nastavení** prvku s názvem *StorageConnectionString* v souboru konfigurace služby:

    // Retrieve storage account from connection-string.
    String storageConnectionString =
        RoleEnvironment.getConfigurationSettings().get("StorageConnectionString");

Následující příklady se předpokládá, že používáte jedním z následujících dvou způsobů jak získat připojovací řetězec úložiště.

## <a name="how-to-create-a-queue"></a>Postup: vytvoření fronty

Objekt **CloudQueueClient** slouží k získání odkazů na objekty fronty. Následující kód vytvoří objekt **CloudQueueClient** . (Poznámka: existují další způsoby, jak vytvořit **CloudStorageAccount** objekty; Další informace najdete v tématu **CloudStorageAccount** v [Azure úložiště klienta SDK Reference].)

Pomocí objektu **CloudQueueClient** odkaz na fronta, kterou chcete použít. Ve frontě můžete vytvořit, pokud neexistuje.

    try
    {
        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount =
           CloudStorageAccount.parse(storageConnectionString);

       // Create the queue client.
       CloudQueueClient queueClient = storageAccount.createCloudQueueClient();

       // Retrieve a reference to a queue.
       CloudQueue queue = queueClient.getQueueReference("myqueue");

       // Create the queue if it doesn't already exist.
       queue.createIfNotExists();
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }

## <a name="how-to-add-a-message-to-a-queue"></a>Postup: přidat zprávu do fronty

Vložit zprávu do existující fronty, nejprve vytvořte nové **CloudQueueMessage**. Dále zavolejte metodu **addMessage** . **CloudQueueMessage** může vytvořit řetězec (ve formátu UTF-8) nebo pole bajtů. Tady je kód, který vytvoří fronty (pokud neexistuje) a vloží se zpráva "Vítáme,".

    try
    {
        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount =
           CloudStorageAccount.parse(storageConnectionString);

        // Create the queue client.
        CloudQueueClient queueClient = storageAccount.createCloudQueueClient();

        // Retrieve a reference to a queue.
        CloudQueue queue = queueClient.getQueueReference("myqueue");

        // Create the queue if it doesn't already exist.
        queue.createIfNotExists();

        // Create a message and add it to the queue.
        CloudQueueMessage message = new CloudQueueMessage("Hello, World");
        queue.addMessage(message);
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }

## <a name="how-to-peek-at-the-next-message"></a>Postup: náhled na další zprávu

Zpráva začátku fronty můžete prohlížet bez odebrání ve frontě tak, že zavoláte **peekMessage**.

    try
    {
        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount =
           CloudStorageAccount.parse(storageConnectionString);

        // Create the queue client.
        CloudQueueClient queueClient = storageAccount.createCloudQueueClient();

        // Retrieve a reference to a queue.
        CloudQueue queue = queueClient.getQueueReference("myqueue");

        // Peek at the next message.
        CloudQueueMessage peekedMessage = queue.peekMessage();

        // Output the message value.
        if (peekedMessage != null)
        {
          System.out.println(peekedMessage.getMessageContentAsString());
       }
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }

## <a name="how-to-change-the-contents-of-a-queued-message"></a>Postup: změnit obsah zprávy ve frontě

Můžete změnit obsah zprávy v místě ve frontě. Pokud se zpráva představuje úkolu práce, může pomocí této funkce můžete aktualizovat stav pracovního úkolu. Následující kód aktualizuje zprávy fronty nový obsah a nastavuje časový limit viditelnost prodloužit jiný 60 sekund. Uloží stav práce přidružené ke zprávě a poskytuje klienta jiné minutu pokračovat v práci na zprávu. Tento postup můžete použít ke sledování pracovní postupy vícekrokové fronty zpráv, aniž byste museli začít od začátku, pokud krok zpracování selže kvůli chybě hardware a software. Obvykle byste měli mít počet opakování i a pokud zprávy je možné více než *n* časy, by ho odstranit. Je to ochrana proti zprávu, která spustí pokaždé, když je zpracování chyby aplikace.

Následující ukázkový hledání kód ve frontě zprávy, najde první zprávu, který odpovídá "Vítáme," pro obsah, a pak upravuje obsah zprávy a zavře.

    try
    {
        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount =
            CloudStorageAccount.parse(storageConnectionString);

        // Create the queue client.
        CloudQueueClient queueClient = storageAccount.createCloudQueueClient();

        // Retrieve a reference to a queue.
        CloudQueue queue = queueClient.getQueueReference("myqueue");

        // The maximum number of messages that can be retrieved is 32.
        final int MAX_NUMBER_OF_MESSAGES_TO_PEEK = 32;

        // Loop through the messages in the queue.
        for (CloudQueueMessage message : queue.retrieveMessages(MAX_NUMBER_OF_MESSAGES_TO_PEEK,1,null,null))
        {
            // Check for a specific string.
            if (message.getMessageContentAsString().equals("Hello, World"))
            {
                // Modify the content of the first matching message.
                message.setMessageContent("Updated contents.");
                // Set it to be visible in 30 seconds.
                EnumSet<MessageUpdateFields> updateFields =
                    EnumSet.of(MessageUpdateFields.CONTENT,
                    MessageUpdateFields.VISIBILITY);
                // Update the message.
                queue.updateMessage(message, 30, updateFields, null, null);
                break;
            }
        }
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }

Následující příklad kódu můžete taky aktualizuje jenom první viditelné zprávu ve frontě.

    try
    {
        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount =
           CloudStorageAccount.parse(storageConnectionString);

        // Create the queue client.
        CloudQueueClient queueClient = storageAccount.createCloudQueueClient();

        // Retrieve a reference to a queue.
        CloudQueue queue = queueClient.getQueueReference("myqueue");

        // Retrieve the first visible message in the queue.
        CloudQueueMessage message = queue.retrieveMessage();

        if (message != null)
        {
            // Modify the message content.
            message.setMessageContent("Updated contents.");
            // Set it to be visible in 60 seconds.
            EnumSet<MessageUpdateFields> updateFields =
                EnumSet.of(MessageUpdateFields.CONTENT,
                MessageUpdateFields.VISIBILITY);
            // Update the message.
            queue.updateMessage(message, 60, updateFields, null, null);
        }
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }

## <a name="how-to-get-the-queue-length"></a>Postup: získání délka fronty

Odhad počet zpráv může vstoupit do fronty. Metoda **downloadAttributes** zeptá fronty služby pro několik aktuální hodnoty, včetně počtu kolik zprávy jsou ve frontě. Počet totiž pouze přibližnou zprávy můžete přidat i odebrat po služba fronty odpovídá požadavku. Metoda **getApproximateMessageCount** hodnotu poslední načtené voláním **downloadAttributes**bez volání služba fronty.

    try
    {
        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount =
           CloudStorageAccount.parse(storageConnectionString);

        // Create the queue client.
        CloudQueueClient queueClient = storageAccount.createCloudQueueClient();

        // Retrieve a reference to a queue.
        CloudQueue queue = queueClient.getQueueReference("myqueue");

       // Download the approximate message count from the server.
        queue.downloadAttributes();

        // Retrieve the newly cached approximate message count.
        long cachedMessageCount = queue.getApproximateMessageCount();

        // Display the queue length.
        System.out.println(String.format("Queue length: %d", cachedMessageCount));
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }

## <a name="how-to-dequeue-the-next-message"></a>Postup: Dequeue další zprávu

Kód dequeues zprávu z fronty ve dvou krocích. Při volání **retrieveMessage**se zobrazí následující zpráva ve frontě. Zprávy vrácených **retrieveMessage** přestane být viditelné pro jiné kódy čtení zpráv z této fronty. Ve výchozím nastavení tato zpráva zobrazena neviditelné 30 sekund. Dokončete zprávu odebráním fronty musíte také zavolat **deleteMessage**. Krocích popsaných odebrání zprávy zajišťuje, že pokud kódu nepovede zpracuje zprávy kvůli chybě hardware a software, jiné instanci aplikace kódu můžete zobrazí stejná zpráva a začněte odznovu. Váš kód volá **deleteMessage** vpravo po zpracování zprávy.

    try
    {
        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount =
            CloudStorageAccount.parse(storageConnectionString);

        // Create the queue client.
        CloudQueueClient queueClient = storageAccount.createCloudQueueClient();

        // Retrieve a reference to a queue.
        CloudQueue queue = queueClient.getQueueReference("myqueue");

        // Retrieve the first visible message in the queue.
        CloudQueueMessage retrievedMessage = queue.retrieveMessage();

        if (retrievedMessage != null)
        {
            // Process the message in less than 30 seconds, and then delete the message.
            queue.deleteMessage(retrievedMessage);
        }
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }


## <a name="additional-options-for-dequeuing-messages"></a>Další možnosti pro vyřazení zprávy

Je možné upravit načítání zpráv z fronty dvěma způsoby: Nejdřív můžete získat dávku zprávy (až 32). Za druhé můžete nastavit časový limit prodloužit nebo zkrátit invisibility umožňující kódu více nebo méně času plně zpracuje každou zprávu.

Následující příklad používá metodu **retrieveMessages** získat 20 zprávy v jednom volání. Potom zpracuje každou zprávu pomocí **pro** smyčky. Také nastaví časový limit invisibility pět minut (300 sekund) pro každé zprávy. Všimněte si, že pět minut spustí pro všechny zprávy ve stejnou dobu, takže když pět minut uplynuly od volání **retrieveMessages**, zprávy, které nebyly odstraněny se bude zobrazovat znovu.

    try
    {
        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount =
            CloudStorageAccount.parse(storageConnectionString);

        // Create the queue client.
        CloudQueueClient queueClient = storageAccount.createCloudQueueClient();

        // Retrieve a reference to a queue.
        CloudQueue queue = queueClient.getQueueReference("myqueue");

        // Retrieve 20 messages from the queue with a visibility timeout of 300 seconds.
        for (CloudQueueMessage message : queue.retrieveMessages(20, 300, null, null)) {
            // Do processing for all messages in less than 5 minutes,
            // deleting each message after processing.
            queue.deleteMessage(message);
        }
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }

## <a name="how-to-list-the-queues"></a>Postup: seznam frontách

Získat seznam aktuální fronty, zavolejte na metodu **CloudQueueClient.listQueues()** , která vrátí kolekci **CloudQueue** objektů.

    try
    {
        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount =
            CloudStorageAccount.parse(storageConnectionString);

        // Create the queue client.
        CloudQueueClient queueClient =
            storageAccount.createCloudQueueClient();

        // Loop through the collection of queues.
        for (CloudQueue queue : queueClient.listQueues())
        {
            // Output each queue name.
            System.out.println(queue.getName());
        }
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }

## <a name="how-to-delete-a-queue"></a>Postup: odstranění fronty

Odstranit fronty a všech zpráv, na které se nacházejí v ní, zavolejte metodu **deleteIfExists** **CloudQueue** objektu.

    try
    {
        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount =
            CloudStorageAccount.parse(storageConnectionString);

        // Create the queue client.
        CloudQueueClient queueClient = storageAccount.createCloudQueueClient();

        // Retrieve a reference to a queue.
        CloudQueue queue = queueClient.getQueueReference("myqueue");

        // Delete the queue if it exists.
        queue.deleteIfExists();
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }

## <a name="next-steps"></a>Další kroky

Teď, když jste se naučili základy fronty úložiště, tyto odkazy vedou na další informace o složitější úlohy úložiště.

- [Azure úložiště SDK jazyka Java][]
- [Azure úložiště klienta SDK Reference][]
- [Služby Azure úložiště rozhraní REST API][]
- [Blog týmu Azure úložiště][]

[Azure SDK for Java]: http://go.microsoft.com/fwlink/?LinkID=525671
[Azure úložiště SDK jazyka Java]: https://github.com/azure/azure-storage-java
[Azure úložiště SDK pro Android]: https://github.com/azure/azure-storage-android
[Azure úložiště klienta SDK Reference]: http://dl.windowsazure.com/storage/javadoc/
[Služby Azure úložiště rozhraní REST API]: https://msdn.microsoft.com/library/azure/dd179355.aspx
[Blog týmu Azure úložiště]: http://blogs.msdn.com/b/windowsazurestorage/
