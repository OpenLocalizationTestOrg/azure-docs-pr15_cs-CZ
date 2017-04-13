<properties
    pageTitle="Použití úložiště souborů z Java | Microsoft Azure"
    description="Naučte se používat službu Azure soubor uložit, stáhnout, seznam, a odstraňte soubory. Vzorky napsané v Java."
    services="storage"
    documentationCenter="java"
    authors="robinsh"
    manager="carmonm"
    editor="tysonn" />

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="Java"
    ms.topic="article"
    ms.date="08/11/2016"
    ms.author="robinsh"/>

# <a name="how-to-use-file-storage-from-java"></a>Použití úložiště souborů z Java

[AZURE.INCLUDE [storage-selector-file-include](../../includes/storage-selector-file-include.md)]
<br/>
[AZURE.INCLUDE [storage-try-azure-tools-queues](../../includes/storage-try-azure-tools-files.md)]

## <a name="overview"></a>Základní informace

V této příručce se dozvíte, jak provádět základní operace na služby pro ukládání souborů Microsoft Azure. Prostřednictvím ukázky napsané v Java se dozvíte, jak vytvářet sdílené položky a adresáře, nahrát, seznam a odstraňovat soubory. Pokud začínáte úložiště souborů ve službě Microsoft Azure, bude velmi užitečné pro porozumění vzorky filtrované pomocí koncepty v následujících částech.

[AZURE.INCLUDE [storage-file-concepts-include](../../includes/storage-file-concepts-include.md)]

[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="create-a-java-application"></a>Vytvoření aplikace Java

Vytvoření ukázky, budete potřebovat Java Development Kit (JDK) a [[Azure úložiště SDK jazyka Java]]. By měl taky nevytvoříte účet Azure úložiště.

## <a name="setup-your-application-to-use-file-storage"></a>Instalace aplikace pomocí úložiště souborů

Použití Azure úložiště rozhraní API, přidejte do horní části souboru Java místo, kam chcete, aby byla pro přístup k služba úložiště z následující příkaz.

    // Include the following imports to use blob APIs.
    import com.microsoft.azure.storage.*;
    import com.microsoft.azure.storage.file.*;

## <a name="setup-an-azure-storage-connection-string"></a>Nastavení připojovací řetězec Azure úložiště

Použití úložiště souborů, budete muset připojit ke svému účtu Azure úložiště. Cílem prvního kroku je konfigurace připojovací řetězec, které budeme používat pro přístup ke svému účtu úložiště. Pojďme definovat statická proměnná to udělat.

    // Configure the connection-string with your values
    public static final String storageConnectionString =
        "DefaultEndpointsProtocol=http;" +
        "AccountName=your_storage_account_name;" +
        "AccountKey=your_storage_account_key";

> [AZURE.NOTE] Nahraďte your_storage_account_name a your_storage_account_key skutečné hodnoty pro váš účet úložiště.

## <a name="connecting-to-an-azure-storage-account"></a>Připojení k účtu Azure úložiště

Pokud chcete připojit ke svému účtu úložiště, budete muset pomocí objektu **CloudStorageAccount** předávání připojovací řetězec do jeho **analyzovat** metodu.

    // Use the CloudStorageAccount object to connect to your storage account
    try {
        CloudStorageAccount storageAccount = CloudStorageAccount.parse(storageConnectionString);
    } catch (InvalidKeyException invalidKey) {
        // Handle the exception
    }

**CloudStorageAccount.parse** vyvolá InvalidKeyException, takže budete muset umístěte do bloku zkuste/skutečné.

## <a name="how-to-create-a-share"></a>Postup: Vytvoření sdílené složky

Všechny soubory a adresáře z úložiště souborů uložena v kontejneru s názvem **sdílet**. Váš účet úložiště můžete mít tolik podílů podle účtu kapacita umožňuje. Získat přístup ke sdílené složce a její obsah, budete muset používat klienta úložiště souborů.

    // Create the file storage client.
    CloudFileClient fileClient = storageAccount.createCloudFileClient();

Použití úložiště klienta souboru, můžete získat, odkaz sdílené.

    // Get a reference to the file share
    CloudFileShare share = fileClient.getShareReference("sampleshare");

Pokud chcete vytvořit skutečně sdílet, použijte metodu **createIfNotExists** CloudFileShare objektu.

    if (share.createIfNotExists()) {
        System.out.println("New share created");
    }

V tomto okamžiku **sdílení** obsahuje odkaz na sdílet s názvem **sampleshare**.

## <a name="how-to-upload-a-file"></a>Postup: odeslání souboru

Azure sdílené úložiště obsahuje minimálně, kořenového adresáře, které mohou být umístěny soubory. V této části se dozvíte, jak nahrát soubor z místního úložiště na kořenovém adresáři sdílet.

Prvním krokem při nahrávání souboru je získat odkaz na adresář, kde by měly nacházet. Můžete to udělat tak, že zavoláte metodu **getRootDirectoryReference** objektu sdílet.

    //Get a reference to the root directory for the share.
    CloudFileDirectory rootDir = share.getRootDirectoryReference();

Teď, když máte odkaz do kořenové složky sdílené složky, můžete nahrát soubor do pomocí následující kód.

    // Define the path to a local file.
    final String filePath = "C:\\temp\\Readme.txt";

    CloudFile cloudFile = rootDir.getFileReference("Readme.txt");
    cloudFile.uploadFromFile(filePath);

## <a name="how-to-create-a-directory"></a>Postup: vytvoření adresáře

Úložiště taky organizujete vložení souborů do podadresáře nechcete všem poznámkám v kořenovém adresáři. Služba úložiště Azure soubor umožňuje vytvořit tolik adresáře, které umožní váš účet. Následující kód vytvoří dílčí adresář s názvem **sampledir** v kořenovém adresáři.

    //Get a reference to the root directory for the share.
    CloudFileDirectory rootDir = share.getRootDirectoryReference();

    //Get a reference to the sampledir directory
    CloudFileDirectory sampleDir = rootDir.getDirectoryReference("sampledir");

    if (sampleDir.createIfNotExists()) {
        System.out.println("sampledir created");
    } else {
        System.out.println("sampledir already exists");
    }

## <a name="how-to-list-files-and-directories-in-a-share"></a>Postup: seznam souborů a adresářů ve sdílené složce

Získání seznamu souborů a adresářů ve sdílené složce snadno probíhá tak, že zavoláte na odkaz CloudFileDirectory **listFilesAndDirectories** . Metoda vrátí seznam ListFileItem objekty, které můžete zapracovávat ji do. Jako příklad následující kód seznam souborů a adresářů v kořenovém adresáři.

    //Get a reference to the root directory for the share.
    CloudFileDirectory rootDir = share.getRootDirectoryReference();

    for ( ListFileItem fileItem : rootDir.listFilesAndDirectories() ) {
        System.out.println(fileItem.getUri());
    }


## <a name="how-to-download-a-file"></a>Postup: Stažení souboru

Jednou z častější operace, které budete provádět proti úložiště souborů je stahovat soubory. V následujícím příkladu kód stáhne SampleFile.txt a zobrazí jeho obsah.

    //Get a reference to the root directory for the share.
    CloudFileDirectory rootDir = share.getRootDirectoryReference();

    //Get a reference to the directory that contains the file
    CloudFileDirectory sampleDir = rootDir.getDirectoryReference("sampledir");

    //Get a reference to the file you want to download
    CloudFile file = sampleDir.getFileReference("SampleFile.txt");

    //Write the contents of the file to the console.
    System.out.println(file.downloadText());

## <a name="how-to-delete-a-file"></a>Postup: odstranění souboru

Jiná společná operace úložiště soubor je odstraňování souborů. Následující kód odstraní do souboru nazvaného SampleFile.txt uložené uvnitř adresář s názvem **sampledir**.


    // Get a reference to the root directory for the share.
    CloudFileDirectory rootDir = share.getRootDirectoryReference();

    // Get a reference to the directory where the file to be deleted is in
    CloudFileDirectory containerDir = rootDir.getDirectoryReference("sampledir");

    String filename = "SampleFile.txt"
    CloudFile file;

    file = containerDir.getFileReference(filename)
    if ( file.deleteIfExists() ) {
        System.out.println(filename + " was deleted");
    }


## <a name="how-to-delete-a-directory"></a>Postup: odstranění adresáře

Odstranění adresáře je velmi jednoduché úkol, i když je nutné poznamenat nelze odstranit nebo adresář, který stále obsahuje soubory nebo jiných adresářů.

    // Get a reference to the root directory for the share.
    CloudFileDirectory rootDir = share.getRootDirectoryReference();

    // Get a reference to the directory you want to delete
    CloudFileDirectory containerDir = rootDir.getDirectoryReference("sampledir");

    // Delete the directory
    if ( containerDir.deleteIfExists() ) {
        System.out.println("Directory deleted");
    }


## <a name="how-to-delete-a-share"></a>Postup: odstranění sdílení

Odstranění sdílené probíhá tak, že zavoláte metodu **deleteIfExists** CloudFileShare objektu. Tady je ukázka kód, který nemá.

    try
    {
        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount = CloudStorageAccount.parse(storageConnectionString);

        // Create the file client.
       CloudFileClient fileClient = storageAccount.createCloudFileClient();

       // Get a reference to the file share
       CloudFileShare share = fileClient.getShareReference("sampleshare");

       if (share.deleteIfExists()) {
           System.out.println("sampleshare deleted");
       }
    } catch (Exception e) {
        e.printStackTrace();
    }

## <a name="next-steps"></a>Další kroky

Pokud chcete další informace o dalších Azure úložiště rozhraní API, postupujte podle těchto odkazech.

- [Středisko pro vývojáře Java](http://azure.microsoft.com/develop/java/)
- [Azure úložiště SDK jazyka Java](https://github.com/azure/azure-storage-java)
- [Azure úložiště SDK pro Android](https://github.com/azure/azure-storage-android)
- [Azure úložiště klienta SDK Reference](http://dl.windowsazure.com/storage/javadoc/)
- [Služby Azure úložiště rozhraní REST API](https://msdn.microsoft.com/library/azure/dd179355.aspx)
- [Blog týmu Azure úložiště](http://blogs.msdn.com/b/windowsazurestorage/)
- [Přenos dat pomocí nástroje příkazového řádku AzCopy](storage-use-azcopy.md)
