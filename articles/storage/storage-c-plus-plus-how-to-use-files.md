<properties
    pageTitle="Použití úložiště souborů z C++ | Microsoft Azure"
    description="Uložení datového souboru v cloudu pomocí úložiště souborů Azure."
    services="storage"
    documentationCenter=".net"
    authors="seguler"
    manager="jahogg"
    editor="tysonn" />

<tags ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/10/2016"
    ms.author="seguler" />

# <a name="how-to-use-file-storage-from-c"></a>Použití úložiště souborů z C++

[AZURE.INCLUDE [storage-selector-file-include](../../includes/storage-selector-file-include.md)]
<br/>
[AZURE.INCLUDE [storage-try-azure-tools-files](../../includes/storage-try-azure-tools-files.md)]
[AZURE.INCLUDE [storage-file-overview-include](../../includes/storage-file-overview-include.md)]

## <a name="about-this-tutorial"></a>O tomto kurzu

V tomto kurzu se dozvíte, jak provádět základní operace na služby pro ukládání souborů Microsoft Azure. Prostřednictvím výběry napsané v C++ se dozvíte, jak vytvářet sdílené položky a adresáře, nahrát, seznam a odstraňovat soubory. Pokud začínáte úložiště souborů ve službě Microsoft Azure, bude velmi užitečné pro porozumění vzorky filtrované pomocí koncepty v následujících částech.

[AZURE.INCLUDE [storage-file-concepts-include](../../includes/storage-file-concepts-include.md)]

[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="create-a-c-application"></a>Vytvoření aplikace C++

Vytvoření ukázky, budou muset nainstalovat Azure úložiště klienta knihovny 2.4.0 c++. By měl taky nevytvoříte účet Azure úložiště.

Instalace klienta úložiště Azure 2.4.0 C++, můžete použít jednu z těchto způsobů:

-   **Linux:** Postupujte podle pokynů na stránce [Azure úložiště klienta v knihovně C++ README](https://github.com/Azure/azure-storage-cpp/blob/master/README.md) .

-   **Windows:** Ve Visual Studiu, klikněte na **Nástroje &gt; Správce balíčků NuGet &gt; Správce balíčků konzoly**. Do [konzoly NuGet balíčku správce](http://docs.nuget.org/docs/start-here/using-the-package-manager-console) zadejte následující příkaz a stiskněte klávesu **ENTER**.

    Instalační balíček wastorage

## <a name="set-up-your-application-to-use-file-storage"></a>Nastavení aplikace pomocí úložiště souborů

Přidejte že následující příkazy do horní části C++ soubor, ve které chcete používat Azure úložiště rozhraní API pro přístup k souborům zahrnout:

    #include "was/storage_account.h"
    #include "was/file.h"

## <a name="set-up-an-azure-storage-connection-string"></a>Nastavení připojovací řetězec Azure úložiště

Použití úložiště souborů, budete muset připojit ke svému účtu Azure úložiště. Cílem prvního kroku je konfigurace připojovací řetězec, které budeme používat pro přístup ke svému účtu úložiště. Pojďme definovat statická proměnná to udělat.

    // Define the connection-string with your values.
    const utility::string_t 
    storage_connection_string(U("DefaultEndpointsProtocol=https;AccountName=your_storage_account;AccountKey=your_storage_account_key"));

## <a name="connecting-to-an-azure-storage-account"></a>Připojení k účtu Azure úložiště

Třídy **cloud_storage_account** lze znázornit informací o účtu úložiště. Načtení informací o účtu úložiště z úložiště připojovací řetězec, můžete použít metodu **analyzovat** .

    // Retrieve storage account from connection string. 
    azure::storage::cloud_storage_account storage_account = 
      azure::storage::cloud_storage_account::parse(storage_connection_string);

## <a name="how-to-create-a-share"></a>Postup: Vytvoření sdílené složky

Všechny soubory a adresáře z úložiště souborů uložena v kontejneru s názvem **sdílet**. Váš účet úložiště můžete mít tolik podílů podle účtu kapacita umožňuje. Získat přístup ke sdílené složce a její obsah, budete muset používat klienta úložiště souborů.

    // Create the file storage client.
    azure::storage::cloud_file_client file_client = 
      storage_account.create_cloud_file_client();

Použití úložiště klienta souboru, můžete získat, odkaz sdílené.

    // Get a reference to the file share
    azure::storage::cloud_file_share share = 
      file_client.get_share_reference(_XPLATSTR("my-sample-share"));

Pokud chcete vytvořit sdílet, použijte metodu **create_if_not_exists** **cloud_file_share** objektu.

    if (share.create_if_not_exists()) { 
        std::wcout << U("New share created") << std::endl;  
    }

V tomto okamžiku **sdílení** obsahuje odkaz na sdílet s názvem **Moje ukázkové sdílet**.

## <a name="how-to-upload-a-file"></a>Postup: odeslání souboru

Minimálně obsahuje Azure sdílené úložiště kořenového adresáře, které mohou být umístěny soubory. V této části se dozvíte, jak nahrát soubor z místního úložiště na kořenovém adresáři sdílet.

Prvním krokem při nahrávání souboru je získat odkaz na adresář, kde by měly nacházet. Můžete to udělat tak, že zavoláte metodu **get_root_directory_reference** objektu sdílet.

    //Get a reference to the root directory for the share.
    azure::storage::cloud_file_directory root_dir = share.get_root_directory_reference();

Teď, když máte odkaz do kořenové složky sdílené složky, můžete nahrát soubor na něj. V tomto příkladu, odešlou se ze souboru, textu a od proudu.

    // Upload a file from a stream.
    concurrency::streams::istream input_stream = 
      concurrency::streams::file_stream<uint8_t>::open_istream(_XPLATSTR("DataFile.txt")).get();

    azure::storage::cloud_file file1 = 
      root_dir.get_file_reference(_XPLATSTR("my-sample-file-1"));
    file1.upload_from_stream(input_stream);

    // Upload some files from text.
    azure::storage::cloud_file file2 = 
      root_dir.get_file_reference(_XPLATSTR("my-sample-file-2"));
    file2.upload_text(_XPLATSTR("more text"));

    // Upload a file from a file.
    azure::storage::cloud_file file4 = 
      root_dir.get_file_reference(_XPLATSTR("my-sample-file-3"));
    file4.upload_from_file(_XPLATSTR("DataFile.txt"));  

## <a name="how-to-create-a-directory"></a>Postup: vytvoření adresáře

Úložiště taky organizujete vložení souborů do podadresáře nechcete všem poznámkám v kořenovém adresáři. Služba úložiště Azure soubor umožňuje vytvořit tolik adresáře, které umožní váš účet. Následující kód vytvoří adresář s názvem **Moje ukázkové adresáře** v kořenovém adresáři, jakož i podadresáře s názvem **Moje ukázkové podadresáře**.
    
    // Retrieve a reference to a directory
    azure::storage::cloud_file_directory directory = share.get_directory_reference(_XPLATSTR("my-sample-directory"));
    
    // Return value is true if the share did not exist and was successfully created.
    directory.create_if_not_exists();
    
    // Create a subdirectory.
    azure::storage::cloud_file_directory subdirectory = 
      directory.get_subdirectory_reference(_XPLATSTR("my-sample-subdirectory"));
    subdirectory.create_if_not_exists();

## <a name="how-to-list-files-and-directories-in-a-share"></a>Postup: seznam souborů a adresářů ve sdílené složce

Získání seznamu souborů a adresářů ve sdílené složce snadno probíhá tak, že zavoláte na odkaz **cloud_file_directory** **list_files_and_directories** . Přístup k celá řada vlastností a metod pro vrácené **list_file_and_directory_item**, musíte volat metodu **list_file_and_directory_item.as_file** k získání objektu **cloud_file** nebo metodu **list_file_and_directory_item.as_directory** k získání objektu **cloud_file_directory** .

Následující kód ukazuje, jak načíst a výstup URI jednotlivých položek v kořenovém adresáři sdílet.

    //Get a reference to the root directory for the share.
    azure::storage::cloud_file_directory root_dir = 
      share.get_root_directory_reference();
    
    // Output URI of each item.
    azure::storage::list_file_and_diretory_result_iterator end_of_results;
    
    for (auto it = directory.list_files_and_directories(); it != end_of_results; ++it)
    {
        if(it->is_directory())
        {
            ucout << "Directory: " << it->as_directory().uri().primary_uri().to_string() << std::endl;
        }
        else if (it->is_file())
        {
            ucout << "File: " << it->as_file().uri().primary_uri().to_string() << std::endl;
        }       
    }
    

## <a name="how-to-download-a-file"></a>Postup: Stažení souboru

Stahovat soubory, nejprve získat odkaz na soubor a potom volat metodu **download_to_stream** přenášet obsah souboru toku objekt, který pak můžete uchovávat místního souboru. Můžete taky můžete metodu **download_to_file** stáhnout obsah souboru na místní soubor. Použijte metodu **download_text** si Pokud chcete stáhnout obsah souboru jako textový řetězec.

Metody **download_to_stream** a **download_text** prokázat stahování souborů, které byly vytvořeny v předchozích částech v následujícím příkladu.

    // Download as text
    azure::storage::cloud_file text_file = 
      root_dir.get_file_reference(_XPLATSTR("my-sample-file-2"));
    utility::string_t text = text_file.download_text();
    ucout << "File Text: " << text << std::endl;
    
    // Download as a stream.
    azure::storage::cloud_file stream_file = 
      root_dir.get_file_reference(_XPLATSTR("my-sample-file-3"));
    
    concurrency::streams::container_buffer<std::vector<uint8_t>> buffer;
    concurrency::streams::ostream output_stream(buffer);
    stream_file.download_to_stream(output_stream);
    std::ofstream outfile("DownloadFile.txt", std::ofstream::binary);
    std::vector<unsigned char>& data = buffer.collection();
    outfile.write((char *)&data[0], buffer.size());
    outfile.close();

## <a name="how-to-delete-a-file"></a>Postup: odstranění souboru

Jiná společná operace úložiště soubor je odstraňování souborů. Následující kód odstraní do souboru nazvaného my ukázkové soubor-3 uložené v kořenovém adresáři.

    // Get a reference to the root directory for the share. 
    azure::storage::cloud_file_share share = 
      file_client.get_share_reference(_XPLATSTR("my-sample-share"));
    
    azure::storage::cloud_file_directory root_dir = 
      share.get_root_directory_reference();
    
    azure::storage::cloud_file file = 
      root_dir.get_file_reference(_XPLATSTR("my-sample-file-3"));
    
    file.delete_file_if_exists();

## <a name="how-to-delete-a-directory"></a>Postup: odstranění adresáře

Odstranění adresáře je jednoduchý úkol, i když je nutné poznamenat nelze odstranit nebo adresář, který stále obsahuje soubory nebo jiných adresářů.
    
    // Get a reference to the share.
    azure::storage::cloud_file_share share = 
      file_client.get_share_reference(_XPLATSTR("my-sample-share"));
    
    // Get a reference to the directory.
    azure::storage::cloud_file_directory directory = 
      share.get_directory_reference(_XPLATSTR("my-sample-directory"));
    
    // Get a reference to the subdirectory you want to delete.
    azure::storage::cloud_file_directory sub_directory =
      directory.get_subdirectory_reference(_XPLATSTR("my-sample-subdirectory"));
    
    // Delete the subdirectory and the sample directory.
    sub_directory.delete_directory_if_exists();
    
    directory.delete_directory_if_exists();

## <a name="how-to-delete-a-share"></a>Postup: odstranění sdílení

Odstranění sdílené probíhá tak, že zavoláte metodu **delete_if_exists** cloud_file_share objektu. Tady je ukázka kód, který nemá.
    
    // Get a reference to the share.
    azure::storage::cloud_file_share share = 
      file_client.get_share_reference(_XPLATSTR("my-sample-share"));
    
    // delete the share if exists
    share.delete_share_if_exists();

## <a name="set-the-maximum-size-for-a-file-share"></a>Nastavit maximální velikost ve sdílené složce

Ve sdílené složce v gigabajty můžete nastavit kvótu (nebo maximální velikost). Můžete taky zaškrtnout zobrazíte množství zpracovávaných dat je momentálně uložený ve sdílené složce.

Nastavením kvóty pro sdílené složky můžete omezit celkovou velikost soubory uložené ve sdílené složce. Pokud celková velikost souborů ve sdílené složce překročí kvótu nastavit ve sdílené složce, pak klienti nebudou moci zvětšit existujících souborů nebo vytváření nových souborů, pokud tyto soubory jsou prázdné.

Následující příklad ukazuje, jak zjistit aktuální použití pro sdílené a nastavení kvóty pro sdílenou složku.

    // Parse the connection string for the storage account.
    azure::storage::cloud_storage_account storage_account = 
      azure::storage::cloud_storage_account::parse(storage_connection_string);
    
    // Create the file client.
    azure::storage::cloud_file_client file_client = 
      storage_account.create_cloud_file_client();
    
    // Get a reference to the share.
    azure::storage::cloud_file_share share = 
      file_client.get_share_reference(_XPLATSTR("my-sample-share"));
    if (share.exists())
    {
        std::cout << "Current share usage: " << share.download_share_usage() << "/" << share.properties().quota();
    
        //This line sets the quota to 2560GB
        share.resize(2560);
    
        std::cout << "Quota increased: " << share.download_share_usage() << "/" << share.properties().quota();
    
    }

## <a name="generate-a-shared-access-signature-for-a-file-or-file-share"></a>Vytvoření podpisu sdílený přístup k souboru nebo sdílení souborů

Si můžete vygenerovat sdílený přístup k podpisu (přidružení zabezpečení) pro sdílené složky nebo jednotlivých souborů. Můžete taky vytvořit zásady sdílený přístup ve sdílené složce spravovat sdílené aplikace access podpisy. Vytvoření zásad sdílené přístupu se doporučuje, protože poskytuje prostředek odvolání přidružení zabezpečení, pokud mají být ohroženo.

Následující příklad vytvoří sdílené přístupu ve sdílené složce a pak tuto zásadu používá k poskytování omezení pro přidružení zabezpečení u souboru ve sdílené složce.

    // Parse the connection string for the storage account.
    azure::storage::cloud_storage_account storage_account = 
      azure::storage::cloud_storage_account::parse(storage_connection_string);
    
    // Create the file client and get a reference to the share
    azure::storage::cloud_file_client file_client = 
      storage_account.create_cloud_file_client();
    
    azure::storage::cloud_file_share share = 
      file_client.get_share_reference(_XPLATSTR("my-sample-share"));
    
    if (share.exists())
    {
        // Create and assign a policy
        utility::string_t policy_name = _XPLATSTR("sampleSharePolicy");

        azure::storage::file_shared_access_policy sharedPolicy = 
          azure::storage::file_shared_access_policy();

        //set permissions to expire in 90 minutes
        sharedPolicy.set_expiry(utility::datetime::utc_now() + 
          utility::datetime::from_minutes(90));

        //give read and write permissions
        sharedPolicy.set_permissions(azure::storage::file_shared_access_policy::permissions::write | azure::storage::file_shared_access_policy::permissions::read);

        //set permissions for the share
        azure::storage::file_share_permissions permissions; 

        //retrieve the current list of shared access policies
        azure::storage::shared_access_policies<azure::storage::file_shared_access_policy> policies;
        
        //add the new shared policy
        policies.insert(std::make_pair(policy_name, sharedPolicy));

        //save the updated policy list
        permissions.set_policies(policies);
        share.upload_permissions(permissions);

        //Retrieve the root directory and file references
        azure::storage::cloud_file_directory root_dir = 
          share.get_root_directory_reference();
        azure::storage::cloud_file file = 
          root_dir.get_file_reference(_XPLATSTR("my-sample-file-1"));

        // Generate a SAS for a file in the share 
        //  and associate this access policy with it.       
        utility::string_t sas_token = file.get_shared_access_signature(sharedPolicy);
        
        // Create a new CloudFile object from the SAS, and write some text to the file.     
        azure::storage::cloud_file file_with_sas(azure::storage::storage_credentials(sas_token).transform_uri(file.uri().primary_uri()));
        utility::string_t text = _XPLATSTR("My sample content");        
        file_with_sas.upload_text(text);        
        
        //Download and print URL with SAS.
        utility::string_t downloaded_text = file_with_sas.download_text();      
        ucout << downloaded_text << std::endl;      
        ucout << azure::storage::storage_credentials(sas_token).transform_uri(file.uri().primary_uri()).to_string() << std::endl;
    
    }

Další informace o vytváření a používání podpisů sdílený přístup najdete v tématu [Používání sdílených přístup podpisy (přidružení zabezpečení)](storage-dotnet-shared-access-signature-part-1.md).

## <a name="next-steps"></a>Další kroky

Další informace o úložišti Azure, prostudujte si tyto materiály:

-   [Úložiště klienta v knihovně C++](https://github.com/Azure/azure-storage-cpp)

-   [Průzkumník Azure úložiště](http://go.microsoft.com/fwlink/?LinkID=822673&clcid=0x409)

-   [Si přečtěte následující dokumentaci Azure úložiště](https://azure.microsoft.com/documentation/services/storage/)
