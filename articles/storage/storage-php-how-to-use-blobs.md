<properties
    pageTitle="Použití úložiště objektů blob (objektu úložiště) z PHP | Microsoft Azure"
    description="Obsahují Nestrukturovaná data v cloudu pomocí úložiště objektů Blob Azure (objektu úložiště)."
    documentationCenter="php"
    services="storage"
    authors="tamram"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="PHP"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="tamram"/>

# <a name="how-to-use-blob-storage-from-php"></a>Použití úložiště objektů blob z PHP

[AZURE.INCLUDE [storage-selector-blob-include](../../includes/storage-selector-blob-include.md)]
<br/>
[AZURE.INCLUDE [storage-try-azure-tools-queues](../../includes/storage-try-azure-tools-blobs.md)]

## <a name="overview"></a>Základní informace

Úložiště objektů Blob Azure je služba, která ukládá jako objekty/objektů BLOB Nestrukturovaná data v cloudu. Úložiště objektů BLOB mohou být uloženy libovolné typu text nebo binární data, jako je dokument, soubor media nebo instalační program aplikace. Úložiště objektů blob se taky nazývá úložiště objektu.

Tato příručka, uvidíte, jak provádět běžné scénáře pomocí služby objektů blob Azure. Vzorky psaných PHP a používat [Azure SDK PHP] [download]. Scénáře nichž se uplatní zahrnují **nahrávání** **záznam**, **stažení**a **Odstranění** objektů BLOB. Další informace o objektů BLOB naleznete v části [Další kroky](#next-steps) .

[AZURE.INCLUDE [storage-blob-concepts-include](../../includes/storage-blob-concepts-include.md)]

[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="create-a-php-application"></a>Vytvoření aplikace PHP

Pouze požadavku na vytvoření aplikace PHP, který má přístup ke službě objektů blob Azure je odkazování tříd v Azure SDK pro PHP z vašeho kódu. Vytvoření aplikace, včetně Poznámkový blok můžete použít libovolný vývojářské nástroje.

V této příručce pomocí funkce služby, které můžete volat aplikace PHP místně nebo v kódu spuštění v rámci Azure web role, kolegy nebo Web.

## <a name="get-the-azure-client-libraries"></a>Získání knihoven Azure klienta

[AZURE.INCLUDE [get-client-libraries](../../includes/get-client-libraries.md)]

## <a name="configure-your-application-to-access-the-blob-service"></a>Konfigurace aplikace pro přístup ke službě objektů blob

Pokud chcete používat službu objektů blob Azure rozhraní API, musíte:

1. Odkaz na soubor zavaděč pomocí příkazu [require_once] a
2. Odkaz na všechny třídy, které používáte.

Následující příklad ukazuje, jak vložit soubor zavaděč a odkaz na třídu **ServicesBuilder** .

> [AZURE.NOTE] V tomto příkladu (a další příklady v tomto článku) se předpokládá, že jste nainstalovali knihoven PHP klienta pro Azure prostřednictvím autora. Pokud jste si nainstalovali ručně knihoven, budete muset odkazovat `WindowsAzure.php` zavaděč soubor.

    require_once 'vendor/autoload.php';
    use WindowsAzure\Common\ServicesBuilder;


V následujících příkladech `require_once` údajů se vždycky zobrazí, ale odkazuje pouze třídy potřebné třeba provádět.

## <a name="set-up-an-azure-storage-connection"></a>Nastavení připojení Azure úložiště

Pro vytvoření instance klienta služby objektů blob Azure, musíte nejdřív máte platný připojovací řetězec. Formát objektů blob služby připojovací řetězec je:

Pro přístup k živé služby:

    DefaultEndpointsProtocol=[http|https];AccountName=[yourAccount];AccountKey=[yourKey]

Pro přístup k emulátoru úložiště:

    UseDevelopmentStorage=true


Vytvoření libovolného klienta služby Azure, musíte použít třídu **ServicesBuilder** . Můžeš:

* Předat připojovací řetězec přímo nebo
* Zjistit víc externích zdrojů pro připojovací řetězec pomocí **CloudConfigurationManager (CCM)** :
    * Ve výchozím nastavení je součástí Podpora externích zdrojů – proměnné prostředí.
    * Přidávání nových zdrojů prodloužením **ConnectionStringSource** předmětu.

Příklady zde uváděného předávat připojovací řetězec, bude přímo.

    require_once 'vendor/autoload.php';

    use WindowsAzure\Common\ServicesBuilder;

    $blobRestProxy = ServicesBuilder::getInstance()->createBlobService($connectionString);

## <a name="create-a-container"></a>Vytvoření kontejneru

[AZURE.INCLUDE [storage-container-naming-rules-include](../../includes/storage-container-naming-rules-include.md)]

Objekt **BlobRestProxy** umožňuje vytvořit kontejner objektů blob metodou **createContainer** . Při vytváření kontejneru, můžete nastavit možnosti v kontejneru, ale to je vlastně není potřeba. (V příkladu níže ukazuje, jak nastavit kontejneru seznam řízení přístupu (ACL) a metadata kontejner.)

    require_once 'vendor\autoload.php';

    use WindowsAzure\Common\ServicesBuilder;
    use MicrosoftAzure\Storage\Blob\Models\CreateContainerOptions;
    use MicrosoftAzure\Storage\Blob\Models\PublicAccessType;
    use MicrosoftAzure\Storage\Common\ServiceException;

    // Create blob REST proxy.
    $blobRestProxy = ServicesBuilder::getInstance()->createBlobService($connectionString);


    // OPTIONAL: Set public access policy and metadata.
    // Create container options object.
    $createContainerOptions = new CreateContainerOptions();

    // Set public access policy. Possible values are
    // PublicAccessType::CONTAINER_AND_BLOBS and PublicAccessType::BLOBS_ONLY.
    // CONTAINER_AND_BLOBS:
    // Specifies full public read access for container and blob data.
    // proxys can enumerate blobs within the container via anonymous
    // request, but cannot enumerate containers within the storage account.
    //
    // BLOBS_ONLY:
    // Specifies public read access for blobs. Blob data within this
    // container can be read via anonymous request, but container data is not
    // available. proxys cannot enumerate blobs within the container via
    // anonymous request.
    // If this value is not specified in the request, container data is
    // private to the account owner.
    $createContainerOptions->setPublicAccess(PublicAccessType::CONTAINER_AND_BLOBS);

    // Set container metadata.
    $createContainerOptions->addMetaData("key1", "value1");
    $createContainerOptions->addMetaData("key2", "value2");

    try {
        // Create container.
        $blobRestProxy->createContainer("mycontainer", $createContainerOptions);
    }
    catch(ServiceException $e){
        // Handle exception based on error codes and messages.
        // Error codes and messages are here:
        // http://msdn.microsoft.com/library/azure/dd179439.aspx
        $code = $e->getCode();
        $error_message = $e->getMessage();
        echo $code.": ".$error_message."<br />";
    }

Volání **setPublicAccess (PublicAccessType::CONTAINER\_a\_objektů BLOB)** zajišťuje kontejner a objektů blob dat dostupné přes anonymní požadavky. Volání **setPublicAccess(PublicAccessType::BLOBS_ONLY)** zajišťuje pouze data objektů blob přístupných anonymní požadavky. Podrobnosti o kontejneru ACL, najdete v článku [Nastavení kontejneru ACL (REST API)][container-acl].

Další informace o kódy chyb služby objektů Blob najdete v tématu [Kódy chyb služby objektů Blob][error-codes].

## <a name="upload-a-blob-into-a-container"></a>Nahrání objektů blob do kontejneru

Pokud chcete odeslat soubor jako objekt blob, použijte metodu **BlobRestProxy -> createBlockBlob** . Tuto operaci vytvoří objektů blob neexistuje nebo přepíše, pokud ano. Následující příklad předpokládá, že kontejneru již byly vytvořeny a používá [fopen] [ fopen] k otevření souboru jako proudu.

    require_once 'vendor/autoload.php';

    use WindowsAzure\Common\ServicesBuilder;
    use MicrosoftAzure\Storage\Common\ServiceException;

    // Create blob REST proxy.
    $blobRestProxy = ServicesBuilder::getInstance()->createBlobService($connectionString);


    $content = fopen("c:\myfile.txt", "r");
    $blob_name = "myblob";

    try {
        //Upload blob
        $blobRestProxy->createBlockBlob("mycontainer", $blob_name, $content);
    }
    catch(ServiceException $e){
        // Handle exception based on error codes and messages.
        // Error codes and messages are here:
        // http://msdn.microsoft.com/library/azure/dd179439.aspx
        $code = $e->getCode();
        $error_message = $e->getMessage();
        echo $code.": ".$error_message."<br />";
    }

Všimněte si, že předchozí ukázka nahraje objektů blob jako tok. Však objektů blob je možné také uložit jako na řetězec pomocí, například [soubor\_získat\_obsah] [ file_get_contents] (funkce). Chcete-li provést pomocí předchozího vzorku, změňte `$content = fopen("c:\myfile.txt", "r");` k `$content = file_get_contents("c:\myfile.txt");`.

## <a name="list-the-blobs-in-a-container"></a>Seznam objektů BLOB v kontejneru

Seznam objektů BLOB v kontejneru, použijte metodu **BlobRestProxy -> listBlobs** s **foreach** smyčka na projděte výsledek. Následující kód zobrazuje název každého objektů blob jako výstup v kontejneru a zobrazí jeho URI do prohlížeče.

    require_once 'vendor/autoload.php';

    use WindowsAzure\Common\ServicesBuilder;
    use MicrosoftAzure\Storage\Common\ServiceException;

    // Create blob REST proxy.
    $blobRestProxy = ServicesBuilder::getInstance()->createBlobService($connectionString);


    try {
        // List blobs.
        $blob_list = $blobRestProxy->listBlobs("mycontainer");
        $blobs = $blob_list->getBlobs();

        foreach($blobs as $blob)
        {
            echo $blob->getName().": ".$blob->getUrl()."<br />";
        }
    }
    catch(ServiceException $e){
        // Handle exception based on error codes and messages.
        // Error codes and messages are here:
        // http://msdn.microsoft.com/library/azure/dd179439.aspx
        $code = $e->getCode();
        $error_message = $e->getMessage();
        echo $code.": ".$error_message."<br />";
    }


## <a name="download-a-blob"></a>Stáhněte si objektů blob

Je možné stáhnout objektů blob volat metodu **BlobRestProxy -> getBlob** a potom na výsledné objekt **GetBlobResult** zavolat metodu **getContentStream** .

    require_once 'vendor/autoload.php';

    use WindowsAzure\Common\ServicesBuilder;
    use MicrosoftAzure\Storage\Common\ServiceException;

    // Create blob REST proxy.
    $blobRestProxy = ServicesBuilder::getInstance()->createBlobService($connectionString);


    try {
        // Get blob.
        $blob = $blobRestProxy->getBlob("mycontainer", "myblob");
        fpassthru($blob->getContentStream());
    }
    catch(ServiceException $e){
        // Handle exception based on error codes and messages.
        // Error codes and messages are here:
        // http://msdn.microsoft.com/library/azure/dd179439.aspx
        $code = $e->getCode();
        $error_message = $e->getMessage();
        echo $code.": ".$error_message."<br />";
    }

Všimněte si, že z příkladu výše se objektů blob jako prostředek toku (výchozí nastavení). Však může použít [toku\_získat\_obsah] [ stream-get-contents] funkce převést vrácené proudu řetězec.

## <a name="delete-a-blob"></a>Odstranění objektů blob

Odstranění objektů blob předáte jméno container a objektů blob název **BlobRestProxy -> deleteBlob**.

    require_once 'vendor/autoload.php';

    use WindowsAzure\Common\ServicesBuilder;
    use MicrosoftAzure\Storage\Common\ServiceException;

    // Create blob REST proxy.
    $blobRestProxy = ServicesBuilder::getInstance()->createBlobService($connectionString);


    try {
        // Delete blob.
        $blobRestProxy->deleteBlob("mycontainer", "myblob");
    }
    catch(ServiceException $e){
        // Handle exception based on error codes and messages.
        // Error codes and messages are here:
        // http://msdn.microsoft.com/library/azure/dd179439.aspx
        $code = $e->getCode();
        $error_message = $e->getMessage();
        echo $code.": ".$error_message."<br />";
    }

## <a name="delete-a-blob-container"></a>Odstranění objektů blob kontejneru

Nakonec odstranit kontejneru objektů blob, předáte jméno container **BlobRestProxy -> deleteContainer**.

    require_once 'vendor/autoload.php';

    use WindowsAzure\Common\ServicesBuilder;
    use MicrosoftAzure\Storage\Common\ServiceException;

    // Create blob REST proxy.
    $blobRestProxy = ServicesBuilder::getInstance()->createBlobService($connectionString);


    try {
        // Delete container.
        $blobRestProxy->deleteContainer("mycontainer");
    }
    catch(ServiceException $e){
        // Handle exception based on error codes and messages.
        // Error codes and messages are here:
        // http://msdn.microsoft.com/library/azure/dd179439.aspx
        $code = $e->getCode();
        $error_message = $e->getMessage();
        echo $code.": ".$error_message."<br />";
    }

## <a name="next-steps"></a>Další kroky

Teď, když jste se naučili základní informace o službě objektů blob Azure, tyto odkazy vedou na další informace o složitější úlohy úložiště.

- Navštivte [blog týmu úložišti Azure](http://blogs.msdn.com/b/windowsazurestorage/)
- Podívejte se na [Příklad objektů blob blok PHP](https://github.com/WindowsAzure/azure-sdk-for-php-samples/blob/master/storage/BlockBlobExample.php).
- Podívejte se na [Příklad objektů blob PHP stránky](https://github.com/WindowsAzure/azure-sdk-for-php-samples/blob/master/storage/PageBlobExample.php).
- [Přenos dat pomocí nástroje příkazového řádku AzCopy](storage-use-azcopy.md)
 
Další informace najdete v tématu taky [Středisko pro vývojáře PHP](/develop/php/).


[download]: http://go.microsoft.com/fwlink/?LinkID=252473
[container-acl]: http://msdn.microsoft.com/library/azure/dd179391.aspx
[error-codes]: http://msdn.microsoft.com/library/azure/dd179439.aspx
[file_get_contents]: http://php.net/file_get_contents
[require_once]: http://php.net/require_once
[fopen]: http://www.php.net/fopen
[stream-get-contents]: http://www.php.net/stream_get_contents
