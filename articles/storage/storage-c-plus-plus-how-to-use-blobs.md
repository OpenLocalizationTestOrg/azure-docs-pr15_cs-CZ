<properties
    pageTitle="Použití úložiště objektů blob (objektu úložiště) z C++ | Microsoft Azure"
    description="Obsahují Nestrukturovaná data v cloudu pomocí úložiště objektů Blob Azure (objektu úložiště)."
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

# <a name="how-to-use-blob-storage-from-c"></a>Použití úložiště objektů Blob z C++  

[AZURE.INCLUDE [storage-selector-blob-include](../../includes/storage-selector-blob-include.md)]
<br/>
[AZURE.INCLUDE [storage-try-azure-tools-blobs](../../includes/storage-try-azure-tools-blobs.md)]

## <a name="overview"></a>Základní informace

Úložiště objektů Blob Azure je služba, která ukládá jako objekty/objektů BLOB Nestrukturovaná data v cloudu. Úložiště objektů BLOB mohou být uloženy libovolné typu text nebo binární data, jako je dokument, soubor media nebo instalační program aplikace. Úložiště objektů blob se taky nazývá úložiště objektu.

Tato příručka ukazují, jak provádět běžné scénáře pomocí služba úložiště objektů Blob Azure. Vzorky napsané v C++ a používat [Azure úložiště klienta v knihovně C++](http://github.com/Azure/azure-storage-cpp/blob/master/README.md). Scénáře nichž se uplatní zahrnují **nahrávání** **záznam**, **stažení**a **Odstranění** objektů BLOB.  

>[AZURE.NOTE] Tato příručka cílovou knihovnu Azure úložiště klienta C++ verze 1.0.0 a nad. Doporučené verze je úložiště klienta knihovna 2.2.0, která je dostupná přes [NuGet](http://www.nuget.org/packages/wastorage) nebo [GitHub](https://github.com/Azure/azure-storage-cpp).

[AZURE.INCLUDE [storage-blob-concepts-include](../../includes/storage-blob-concepts-include.md)]
[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="create-a-c-application"></a>Vytvoření aplikace C++
V této příručce použijete úložiště funkce, které lze provádět aplikace C++.  

Chcete-li provést, musíte nainstalovat knihovnu Azure úložiště klienta pro C++ a vytvořit účet Azure úložiště v Azure předplatné.   

Instalace knihovnu Azure úložiště klienta C++, můžete pomocí následujících metod:

-   **Linux:** Postupujte podle pokynů na stránce [Azure úložiště klienta v knihovně C++ README](https://github.com/Azure/azure-storage-cpp/blob/master/README.md) .  
-   **Windows:** Ve Visual Studiu, klikněte na **Nástroje > Správce balíčků NuGet > Správce balíčků konzoly**. Do [konzoly NuGet balíčku správce](http://docs.nuget.org/docs/start-here/using-the-package-manager-console) zadejte následující příkaz a stiskněte klávesu **ENTER**.  

        Install-Package wastorage

## <a name="configure-your-application-to-access-blob-storage"></a>Konfigurace aplikace pro přístup k úložišti objektů Blob  
Přidejte že následující příkazy do horní části C++ soubor, ve které chcete použít pro přístup k objektů BLOB Azure úložiště rozhraní API zahrnout:  

    #include "was/storage_account.h"
    #include "was/blob.h"

## <a name="setup-an-azure-storage-connection-string"></a>Nastavení připojovací řetězec Azure úložiště
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

Pak získáte odkaz na třída **cloud_blob_client** jako umožňuje načítat objekty, které představují kontejnerů a objektů BLOB uložena v služba úložiště objektů Blob. Následující kód vytvoří objekt **cloud_blob_client** pomocí objektu účtu úložiště obnovená nad:  

    // Create the blob client.
    azure::storage::cloud_blob_client blob_client = storage_account.create_cloud_blob_client();  

## <a name="how-to-create-a-container"></a>Postup: vytvoření kontejneru

[AZURE.INCLUDE [storage-container-naming-rules-include](../../includes/storage-container-naming-rules-include.md)]

Tento příklad ukazuje, jak vytvořit kontejner, pokud už neexistuje:  

    try
    {
        // Retrieve storage account from connection string.
        azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

        // Create the blob client.
        azure::storage::cloud_blob_client blob_client = storage_account.create_cloud_blob_client();

        // Retrieve a reference to a container.
        azure::storage::cloud_blob_container container = blob_client.get_container_reference(U("my-sample-container"));

        // Create the container if it doesn't already exist.
        container.create_if_not_exists();
    }
    catch (const std::exception& e)
    {
        std::wcout << U("Error: ") << e.what() << std::endl;
    }  

Ve výchozím nastavení nového kontejneru je soukromá a třeba zadat kód úložiště přístup ke stažení objektů BLOB tento kontejner. Pokud chcete být soubory (objektů BLOB) v kontejneru k dispozici všem, můžete nastavit kontejneru byl veřejný pomocí následujícího kódu:  

    // Make the blob container publicly accessible.
    azure::storage::blob_container_permissions permissions;
    permissions.set_public_access(azure::storage::blob_container_public_access_type::blob);
    container.upload_permissions(permissions);  

Každý, kdo na Internetu zobrazit objektů BLOB v kontejneru veřejné, ale můžete upravit nebo odstranit jenom v případě, že máte odpovídající přístupové klávesy.  

## <a name="how-to-upload-a-blob-into-a-container"></a>Postup: nahrát objektů blob do kontejneru
Úložiště objektů Blob Azure podporuje objektů BLOB bloku a objekty BLOB stránky. Ve většině případů objektů blob blok je doporučený typ, který chcete použít.  

K nahrání souboru do bloku objektů blob, odkazovat kontejneru, můžete získat odkaz objektů blob blokovat. Až budete mít objektů blob odkaz, můžete všechny toku dat do ní nahrajete tak, že zavoláte metodu **upload_from_stream** . Tuto operaci vytvoří objektů blob, pokud ho neměli dříve neexistuje nebo přepsat, pokud existují. Následující příklad ukazuje, jak nahrát objektů blob do kontejneru a předpokládá, že kontejneru byl vytvořen.  

    // Retrieve storage account from connection string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

    // Create the blob client.
    azure::storage::cloud_blob_client blob_client = storage_account.create_cloud_blob_client();

    // Retrieve a reference to a previously created container.
    azure::storage::cloud_blob_container container = blob_client.get_container_reference(U("my-sample-container"));

    // Retrieve reference to a blob named "my-blob-1".
    azure::storage::cloud_block_blob blockBlob = container.get_block_blob_reference(U("my-blob-1"));

    // Create or overwrite the "my-blob-1" blob with contents from a local file.
    concurrency::streams::istream input_stream = concurrency::streams::file_stream<uint8_t>::open_istream(U("DataFile.txt")).get();
    blockBlob.upload_from_stream(input_stream);
    input_stream.close().wait();

    // Create or overwrite the "my-blob-2" and "my-blob-3" blobs with contents from text.
    // Retrieve a reference to a blob named "my-blob-2".
    azure::storage::cloud_block_blob blob2 = container.get_block_blob_reference(U("my-blob-2"));
    blob2.upload_text(U("more text"));

    // Retrieve a reference to a blob named "my-blob-3".
    azure::storage::cloud_block_blob blob3 = container.get_block_blob_reference(U("my-directory/my-sub-directory/my-blob-3"));
    blob3.upload_text(U("other text"));  

Můžete taky můžete metodu **upload_from_file** nahrání souboru do bloku objektů blob.

## <a name="how-to-list-the-blobs-in-a-container"></a>Postup: seznam objektů BLOB v kontejneru
Seznam objektů BLOB v kontejneru, nejprve získáte odkaz kontejner. Potom můžete pomocí metody **list_blobs** kontejneru k načtení objektů BLOB a/nebo adresáře v něm obsažené. Přístup k celá řada vlastností a metod pro vrácené **list_blob_item**, musíte volat metodu **list_blob_item.as_blob** k získání objektu **cloud_blob** nebo metodu **list_blob.as_directory** k získání objektu cloud_blob_directory. Následující kód ukazuje, jak načíst a výstupní URI jednotlivých položek v kontejneru **Moje ukázkové kontejner** :

    // Retrieve storage account from connection string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

    // Create the blob client.
    azure::storage::cloud_blob_client blob_client = storage_account.create_cloud_blob_client();

    // Retrieve a reference to a previously created container.
    azure::storage::cloud_blob_container container = blob_client.get_container_reference(U("my-sample-container"));

    // Output URI of each item.
    azure::storage::list_blob_item_iterator end_of_results;
    for (auto it = container.list_blobs(); it != end_of_results; ++it)
    {
        if (it->is_blob())
        {
            std::wcout << U("Blob: ") << it->as_blob().uri().primary_uri().to_string() << std::endl;
        }
        else
        {
            std::wcout << U("Directory: ") << it->as_directory().uri().primary_uri().to_string() << std::endl;
        }
    }

Podrobné informace o zobrazení operace tématech [seznamu Azure úložiště v C++](storage-c-plus-plus-enumeration.md).

## <a name="how-to-download-blobs"></a>Postup: Stáhněte si objekty BLOB
Ke stažení objektů BLOB, první načtení objektů blob odkazu a potom volat metodu **download_to_stream** . Metoda **download_to_stream** přenášet obsah objektů blob toku objekt, který pak můžete uchovávat místního souboru v následujícím příkladu.  

    // Retrieve storage account from connection string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

    // Create the blob client.
    azure::storage::cloud_blob_client blob_client = storage_account.create_cloud_blob_client();

    // Retrieve a reference to a previously created container.
    azure::storage::cloud_blob_container container = blob_client.get_container_reference(U("my-sample-container"));

    // Retrieve reference to a blob named "my-blob-1".
    azure::storage::cloud_block_blob blockBlob = container.get_block_blob_reference(U("my-blob-1"));

    // Save blob contents to a file.
    concurrency::streams::container_buffer<std::vector<uint8_t>> buffer;
    concurrency::streams::ostream output_stream(buffer);
    blockBlob.download_to_stream(output_stream);

    std::ofstream outfile("DownloadBlobFile.txt", std::ofstream::binary);
    std::vector<unsigned char>& data = buffer.collection();

    outfile.write((char *)&data[0], buffer.size());
    outfile.close();  

Můžete taky můžete metodu **download_to_file** stahování obsahu objektů blob do souboru.
Kromě toho můžete také metodu **download_text** si Pokud chcete stáhnout obsah objektů blob jako textový řetězec.  

    // Retrieve storage account from connection string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

    // Create the blob client.
    azure::storage::cloud_blob_client blob_client = storage_account.create_cloud_blob_client();

    // Retrieve a reference to a previously created container.
    azure::storage::cloud_blob_container container = blob_client.get_container_reference(U("my-sample-container"));

    // Retrieve reference to a blob named "my-blob-2".
    azure::storage::cloud_block_blob text_blob = container.get_block_blob_reference(U("my-blob-2"));

    // Download the contents of a blog as a text string.
    utility::string_t text = text_blob.download_text();

## <a name="how-to-delete-blobs"></a>Postup: odstranění objektů BLOB
Odstranění objektů blob, nejprve získat odkaz objektů blob a potom zavolat metodu **delete_blob** .  

    // Retrieve storage account from connection string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

    // Create the blob client.
    azure::storage::cloud_blob_client blob_client = storage_account.create_cloud_blob_client();

    // Retrieve a reference to a previously created container.
    azure::storage::cloud_blob_container container = blob_client.get_container_reference(U("my-sample-container"));

    // Retrieve reference to a blob named "my-blob-1".
    azure::storage::cloud_block_blob blockBlob = container.get_block_blob_reference(U("my-blob-1"));

    // Delete the blob.
    blockBlob.delete_blob();

## <a name="next-steps"></a>Další kroky
Teď, když jste se naučili základní informace o úložišti objektů blob, tyto odkazy vedou na další informace o úložišti Azure.  

-   [Použití úložiště fronty z C++](storage-c-plus-plus-how-to-use-queues.md)
-   [Použití úložiště tabulek z C++](storage-c-plus-plus-how-to-use-tables.md)
-   [Seznam zdrojů Azure úložiště v C++](storage-c-plus-plus-enumeration.md)
-   [Knihovny úložiště klienta pro referenci C++](http://azure.github.io/azure-storage-cpp)
-   [Si přečtěte následující dokumentaci Azure úložiště](https://azure.microsoft.com/documentation/services/storage/)
- [Přenos dat pomocí nástroje příkazového řádku AzCopy](storage-use-azcopy.md)
