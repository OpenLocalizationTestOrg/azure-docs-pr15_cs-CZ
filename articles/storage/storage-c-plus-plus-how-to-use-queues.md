<properties
    pageTitle="Použití úložiště fronty (C++) | Microsoft Azure"
    description="Naučte se používat službu fronty úložiště v Azure. Příklady jsou napsané v C++."
    services="storage"
    documentationCenter=".net"
    authors="dineshmurthy"
    manager="jahogg"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="dineshm"/>

# <a name="how-to-use-queue-storage-from-c"></a>Použití úložiště fronty z C++  

[AZURE.INCLUDE [storage-selector-queue-include](../../includes/storage-selector-queue-include.md)]
<br/>
[AZURE.INCLUDE [storage-try-azure-tools-queues](../../includes/storage-try-azure-tools-queues.md)]

## <a name="overview"></a>Základní informace
Tento průvodce vám ukáže, jak provádět běžné scénáře použití úložiště služby Azure fronty. Vzorky napsané v C++ a používat [Azure úložiště klienta v knihovně C++](http://github.com/Azure/azure-storage-cpp/blob/master/README.md). Scénáře nichž se uplatní zahrnují **Vložení**, **prohlížení**, **začíná**a **odstraňování** zpráv, jakož i **vytváření a odstraňování fronty**.

>[AZURE.NOTE] Tato příručka cílovou knihovnu Azure úložiště klienta C++ verze 1.0.0 a nad. Doporučené verze je úložiště klienta knihovna 2.2.0, která je dostupná přes [NuGet](http://www.nuget.org/packages/wastorage) nebo [GitHub](http://github.com/Azure/azure-storage-cpp/).

[AZURE.INCLUDE [storage-queue-concepts-include](../../includes/storage-queue-concepts-include.md)]
[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="create-a-c-application"></a>Vytvoření aplikace C++  
V této příručce použijete úložiště funkce, které lze provádět aplikace C++.

Chcete-li provést, musíte nainstalovat knihovnu Azure úložiště klienta pro C++ a vytvořit účet Azure úložiště v Azure předplatné.

Instalace knihovnu Azure úložiště klienta C++, můžete pomocí následujících metod:

-   **Linux:** Postupujte podle pokynů na stránce [Azure úložiště klienta v knihovně C++ README](https://github.com/Azure/azure-storage-cpp/blob/master/README.md) .
-   **Windows:** Ve Visual Studiu, klikněte na **Nástroje > Správce balíčků NuGet > Správce balíčků konzoly**. Do [konzoly NuGet balíčku správce](http://docs.nuget.org/docs/start-here/using-the-package-manager-console) zadejte následující příkaz a stiskněte klávesu **ENTER**.

        Install-Package wastorage

## <a name="configure-your-application-to-access-queue-storage"></a>Konfigurace aplikace pro přístup k úložišti fronty
Přidejte že následující příkazy do horní části C++ soubor, ve které chcete použít Azure úložiště rozhraní API pro přístup k fronty zahrnout:  

    #include "was/storage_account.h"
    #include "was/queue.h"

## <a name="set-up-an-azure-storage-connection-string"></a>Nastavení připojovací řetězec Azure úložiště

Azure úložiště klienta používá k ukládání koncové body a přihlašovací údaje pro přístup k dat správu přístupových připojovací řetězec úložiště. Při spuštění v klientské aplikaci, je nutné zadat připojovací řetězec úložiště ve formátu následující nazvanou podle této účtu úložiště a přístupové klávesy úložiště úložiště účtu uvedený na [Portálu Azure](https://portal.azure.com) pro hodnoty *název účtu* a *AccountKey* . Informace o účtech úložiště a přístupové klávesy najdete v článku [O úložiště účty adresářové služby Azure](storage-create-storage-account.md). Tento příklad ukazuje, jak můžou deklarovat statické pole pro uložení připojovací řetězec:  

    // Define the connection-string with your values.
    const utility::string_t storage_connection_string(U("DefaultEndpointsProtocol=https;AccountName=your_storage_account;AccountKey=your_storage_account_key"));

Pokud chcete vyzkoušet aplikace ve vašem počítači Windows, můžete použít Microsoft Azure [emulátoru úložiště](storage-use-emulator.md) , které máte nainstalované s [Azure SDK](https://azure.microsoft.com/downloads/). Emulátoru úložiště je nástroj, který napodobuje k dispozici v Azure v počítači místní vývoje služby objektů Blob, fronty a tabulky. Následující příklad ukazuje, jak můžou deklarovat statické pole pro uložení připojovací řetězec pro místní úložiště emulátoru:  

    // Define the connection-string with Azure Storage Emulator.
    const utility::string_t storage_connection_string(U("UseDevelopmentStorage=true;"));  

Spuštění emulátoru Azure úložiště, klikněte na tlačítko **Start** a stiskněte klávesu **Windows** . Začněte psát **Azure úložiště emulátoru**a vyberte **Microsoft Azure úložiště emulátoru** ze seznamu aplikací.

Následující příklady se předpokládá, že používáte jedním z následujících dvou způsobů jak získat připojovací řetězec úložiště.

## <a name="retrieve-your-connection-string"></a>Získání připojovacího řetězce
Třídy **cloud_storage_account** lze znázornit informací o účtu úložiště. Načtení informací o účtu úložiště z úložiště připojovací řetězec, můžete použít metodu **analyzovat** .

    // Retrieve storage account from connection string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

## <a name="how-to-create-a-queue"></a>Postup: vytvoření fronty
Objekt **cloud_queue_client** slouží k získání odkazů na objekty fronty. Následující kód vytvoří objekt **cloud_queue_client** .

    // Retrieve storage account from connection string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

    // Create a queue client.
    azure::storage::cloud_queue_client queue_client = storage_account.create_cloud_queue_client();

Pomocí objektu **cloud_queue_client** odkaz na fronta, kterou chcete použít. Ve frontě můžete vytvořit, pokud neexistuje.

    // Retrieve a reference to a queue.
    azure::storage::cloud_queue queue = queue_client.get_queue_reference(U("my-sample-queue"));

    // Create the queue if it doesn't already exist.
    queue.create_if_not_exists();  

## <a name="how-to-insert-a-message-into-a-queue"></a>Postup: vložte do zprávy do fronty
Vložit zprávu do existující fronty, nejprve vytvořte nové **cloud_queue_message**. Dále zavolejte metodu **add_message** . **Cloud_queue_message** může vytvořit řetězec nebo pole **bajtů** . Tady je kód, který vytvoří fronty (pokud neexistuje) a vloží se zpráva "Vítáme,":

    // Retrieve storage account from connection-string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

    // Create the queue client.
    azure::storage::cloud_queue_client queue_client = storage_account.create_cloud_queue_client();

    // Retrieve a reference to a queue.
    azure::storage::cloud_queue queue = queue_client.get_queue_reference(U("my-sample-queue"));

    // Create the queue if it doesn't already exist.
    queue.create_if_not_exists();

    // Create a message and add it to the queue.
    azure::storage::cloud_queue_message message1(U("Hello, World"));
    queue.add_message(message1);  

## <a name="how-to-peek-at-the-next-message"></a>Postup: náhled na další zprávu
Zpráva začátku fronty můžete prohlížet bez odebrání ve frontě tak, že zavoláte metodu **peek_message** .

    // Retrieve storage account from connection-string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

    // Create the queue client.
    azure::storage::cloud_queue_client queue_client = storage_account.create_cloud_queue_client();

    // Retrieve a reference to a queue.
    azure::storage::cloud_queue queue = queue_client.get_queue_reference(U("my-sample-queue"));

    // Peek at the next message.
    azure::storage::cloud_queue_message peeked_message = queue.peek_message();

    // Output the message content.
    std::wcout << U("Peeked message content: ") << peeked_message.content_as_string() << std::endl;

## <a name="how-to-change-the-contents-of-a-queued-message"></a>Postup: změnit obsah zprávy ve frontě
Můžete změnit obsah zprávy v místě ve frontě. Pokud se zpráva představuje úkolu práce, může pomocí této funkce můžete aktualizovat stav pracovního úkolu. Následující kód aktualizuje zprávy fronty nový obsah a nastavuje časový limit viditelnost prodloužit jiný 60 sekund. Uloží stav práce přidružené ke zprávě a poskytuje klienta jiné minutu pokračovat v práci na zprávu. Tento postup můžete použít ke sledování pracovní postupy vícekrokové fronty zpráv, aniž byste museli začít od začátku, pokud krok zpracování selže kvůli chybě hardware a software. Obvykle byste měli mít počet opakování i a pokud se zpráva, je možné víc než n časy, by ho odstranit. Je to ochrana proti zprávu, která spustí pokaždé, když je zpracování chyby aplikace.

    // Retrieve storage account from connection-string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_conection_string);

    // Create the queue client.
    azure::storage::cloud_queue_client queue_client = storage_account.create_cloud_queue_client();

    // Retrieve a reference to a queue.
    azure::storage::cloud_queue queue = queue_client.get_queue_reference(U("my-sample-queue"));

    // Get the message from the queue and update the message contents.
    // The visibility timeout "0" means make it visible immediately.
    // The visibility timeout "60" means the client can get another minute to continue
    // working on the message.
    azure::storage::cloud_queue_message changed_message = queue.get_message();

    changed_message.set_content(U("Changed message"));
    queue.update_message(changed_message, std::chrono::seconds(60), true);

    // Output the message content.
    std::wcout << U("Changed message content: ") << changed_message.content_as_string() << std::endl;  

## <a name="how-to-de-queue-the-next-message"></a>Postup: zrušit fronty další zprávu
Kód zrušte fronty zprávu z fronty ve dvou krocích. Při volání **get_message**se zobrazí následující zpráva ve frontě. Zprávy vrácených **get_message** přestane být viditelné pro jiné kódy čtení zpráv z této fronty. Dokončete zprávu odebráním fronty musíte také zavolat **delete_message**. Krocích popsaných odebrání zprávy zajišťuje, že pokud kódu nepovede zpracuje zprávy kvůli chybě hardware a software, jiné instanci aplikace kódu můžete zobrazí stejná zpráva a začněte odznovu. Váš kód volá **delete_message** vpravo po zpracování zprávy.

    // Retrieve storage account from connection-string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

    // Create the queue client.
    azure::storage::cloud_queue_client queue_client = storage_account.create_cloud_queue_client();

    // Retrieve a reference to a queue.
    azure::storage::cloud_queue queue = queue_client.get_queue_reference(U("my-sample-queue"));

    // Get the next message.
    azure::storage::cloud_queue_message dequeued_message = queue.get_message();
    std::wcout << U("Dequeued message: ") << dequeued_message.content_as_string() << std::endl;

    // Delete the message.
    queue.delete_message(dequeued_message);

## <a name="how-to-leverage-additional-options-for-de-queuing-messages"></a>Postup: využít další možnosti pro vypnutí řazení zpráv
Je možné upravit načítání zpráv z fronty dvěma způsoby: Nejdřív můžete získat dávku zprávy (až 32). Za druhé můžete nastavit časový limit prodloužit nebo zkrátit invisibility umožňující kódu více nebo méně času plně zpracuje každou zprávu. Následující příklad používá metodu **get_messages** získat 20 zprávy v jednom volání. Potom zpracuje každou zprávu pomocí **pro** smyčky. Také nastaví časový limit invisibility pět minut pro každou zprávu. Všimněte si, že 5 minut spustí pro všechny zprávy ve stejnou dobu, tak až poté, co jste 5 minut uplynuly volání **get_messages**, zprávy, které nebyly odstraněny se bude zobrazovat znovu.

    // Retrieve storage account from connection-string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

    // Create the queue client.
    azure::storage::cloud_queue_client queue_client = storage_account.create_cloud_queue_client();

    // Retrieve a reference to a queue.
    azure::storage::cloud_queue queue = queue_client.get_queue_reference(U("my-sample-queue"));

    // Dequeue some queue messages (maximum 32 at a time) and set their visibility timeout to
    // 5 minutes (300 seconds).
    azure::storage::queue_request_options options;
    azure::storage::operation_context context;

    // Retrieve 20 messages from the queue with a visibility timeout of 300 seconds.
    std::vector<azure::storage::cloud_queue_message> messages = queue.get_messages(20, std::chrono::seconds(300), options, context);

    for (auto it = messages.cbegin(); it != messages.cend(); ++it)
    {
        // Display the contents of the message.
        std::wcout << U("Get: ") << it->content_as_string() << std::endl;
    }

## <a name="how-to-get-the-queue-length"></a>Postup: získání délka fronty
Odhad počet zpráv může vstoupit do fronty. Metoda **download_attributes** zeptá služba fronty načtení fronty atributů, včetně počtu zpráv. Metoda **approximate_message_count** získá přibližnou počet zpráv ve frontě.

    // Retrieve storage account from connection-string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

    // Create the queue client.
    azure::storage::cloud_queue_client queue_client = storage_account.create_cloud_queue_client();

    // Retrieve a reference to a queue.
    azure::storage::cloud_queue queue = queue_client.get_queue_reference(U("my-sample-queue"));

    // Fetch the queue attributes.
    queue.download_attributes();

    // Retrieve the cached approximate message count.
    int cachedMessageCount = queue.approximate_message_count();

    // Display number of messages.
    std::wcout << U("Number of messages in queue: ") << cachedMessageCount << std::endl;  

## <a name="how-to-delete-a-queue"></a>Postup: odstranění fronty
Pokud chcete odstranit fronty a všech zpráv, na které se nacházejí v ní, kontaktujte metodu **delete_queue_if_exists** fronty objektu.

    // Retrieve storage account from connection-string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

    // Create the queue client.
    azure::storage::cloud_queue_client queue_client = storage_account.create_cloud_queue_client();

    // Retrieve a reference to a queue.
    azure::storage::cloud_queue queue = queue_client.get_queue_reference(U("my-sample-queue"));

    // If the queue exists and delete it.
    queue.delete_queue_if_exists();  

## <a name="next-steps"></a>Další kroky

Teď, když jste se naučili základy fronty úložiště, tyto odkazy vedou na další informace o úložišti Azure.

-   [Použití úložiště objektů Blob z C++](storage-c-plus-plus-how-to-use-blobs.md)
-   [Použití úložiště tabulek z C++](storage-c-plus-plus-how-to-use-tables.md)
-   [Seznam zdrojů Azure úložiště v C++](storage-c-plus-plus-enumeration.md)
-   [Knihovny úložiště klienta pro referenci C++](http://azure.github.io/azure-storage-cpp)
-   [Si přečtěte následující dokumentaci Azure úložiště](https://azure.microsoft.com/documentation/services/storage/)

