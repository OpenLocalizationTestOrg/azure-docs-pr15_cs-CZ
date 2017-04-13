<properties
    pageTitle="Místní aplikaci úložiště objektů blob (Java) | Microsoft Azure"
    description="Naučte se vytvářet aplikace konzoly Azure, odešlou se obrázek, který zobrazí obrázek v prohlížeči. Ukázky v Java."
    services="storage"
    documentationCenter="java"
    authors="tamram"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="Java"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="tamram"/>

# <a name="on-premises-application-with-blob-storage"></a>Místní aplikaci úložiště objektů blob

## <a name="overview"></a>Základní informace

Následující příklad ukazuje, jak můžete Azure úložiště pro ukládání obrázků v Azure. Kód v tomto článku je pro aplikace konzoly Azure, odešlou se obrázek, který vytvoří soubor ve formátu HTML, která se zobrazí obrázek v prohlížeči.

## <a name="prerequisites"></a>Zjistit předpoklady pro

- Je nainstalovaná Java vývojář Kit (JDK), verze 1,6 nebo novější.
- Azure SDK nainstalovaný.
- SKLENICE pro knihovny Azure Java a všechny použitelné závislost sklenic po g nainstalovaných a jsou cesty buildu použít tak, že překladač Java. Informace o instalaci knihoven Azure jazyka Java najdete v tématu [stažení SDK Azure jazyka Java](java-download-azure-sdk.md).
- Účet Azure úložiště je nastavené. Název účtu a účtu klíč účtu úložiště použijí kód v tomto článku. Postup [Vytvoření účtu úložiště](storage-create-storage-account.md#create-a-storage-account) informace o vytváření účtu úložiště a [zobrazení a kopírování úložiště přístupových kláves](storage-create-storage-account.md#view-and-copy-storage-access-keys) informace o načítání klíč účtu.

- Vytvoření souboru místní obrázku s názvem uložené c: cestu\\myimages\\image1.jpg. Můžete taky Upravte konstruktor   **FileInputStream** v příkladu pro použití jiným obrázkem cestu a název souboru.

[AZURE.INCLUDE [create-account-note](../../includes/create-account-note.md)]

## <a name="to-use-azure-blob-storage-to-upload-a-file"></a>Použití úložišti objektů blob Azure k odeslání souboru

Zde jsou uvedeny podrobný postup. Pokud chcete přejít dále, celý kód jsou uvedeny dál v tomto článku.

Začněte včetně importy Azure základní třídy úložiště, tříd klienta objektů blob Azure, tříd jazyka Java/v a třídy **URISyntaxException** kód.

    import com.microsoft.azure.storage.*;
    import com.microsoft.azure.storage.blob.*;
    import java.io.*;
    import java.net.URISyntaxException;

Deklarovat třídu s názvem **StorageSample**a levou závorku **{**obsahovat.

    public class StorageSample {

V rámci třídy **StorageSample** deklarujte proměnnou řetězec obsahující výchozí koncový bod protokol, váš název účtu úložiště a kód přístup úložiště uvedený ve vašem účtu Azure úložiště. Nahrazení hodnot zástupný symbol **vaše\_účtu\_název** a **vaše\_účtu\_klíč** vlastními účet název a účet klíč, respektive.

    public static final String storageConnectionString =
           "DefaultEndpointsProtocol=http;" +
               "AccountName=your_account_name;" +
               "AccountKey=your_account_name";

Přidat v prohlášení o **hlavní**obsahovat blok **akci** , zahrnout potřebné otevřít závorky **{**.

    public static void main(String[] args)
    {
        try
        {

Deklarujte proměnné tento typ (popisy jsou pro jejich použití v tomto příkladu):

-   **CloudStorageAccount**: Inicializace objektu účtu s Azure úložiště název svého účtu a klíč, a vytvořit klienta objektů blob.
-   **CloudBlobClient**: použít pro přístup ke službě objektů blob.
-   **CloudBlobContainer**: slouží k vytvoření kontejneru objektů blob, seznamu objektů BLOB v kontejneru a odstranit kontejner.
-   **CloudBlockBlob**: použili k nahrání souboru místní obrázku do kontejneru.

<!-- -->

    CloudStorageAccount account;
    CloudBlobClient serviceClient;
    CloudBlobContainer container;
    CloudBlockBlob blob;

Přiřazení hodnoty proměnnou **účtu** .

    account = CloudStorageAccount.parse(storageConnectionString);

Přiřazení hodnoty **serviceClient** proměnné.

    serviceClient = account.createCloudBlobClient();

Přiřazení hodnoty proměnnou **kontejner** . Budeme se odkaz na kontejner **gettingstarted**.

    // Container name must be lower case.
    container = serviceClient.getContainerReference("gettingstarted");

Vytvoření kontejneru. Tento způsob vytvoří kontejneru, pokud ho není existují (a vrátí **hodnotu true**). Pokud kontejneru neexistuje, vrátí **hodnotu false**. Alternativy **createIfNotExists** je způsob **Vytvoření** (který vrátí chybu, pokud již existuje kontejneru).

    container.createIfNotExists();

Nastavte anonymní přístup pro kontejner.

    // Set anonymous access on the container.
    BlobContainerPermissions containerPermissions;
    containerPermissions = new BlobContainerPermissions();
    containerPermissions.setPublicAccess(BlobContainerPublicAccessType.CONTAINER);
    container.uploadPermissions(containerPermissions);

Získáte odkaz na objektů blob blok, který bude zastupovat objektů blob v Azure úložiště.

    blob = container.getBlockBlobReference("image1.jpg");

Odkaz na místně vytvořený soubor, který bude nahrajete pomocí konstruktoru **soubor** . Ujistěte se, že jste vytvořili tento soubor před spuštěním kód.

    File fileReference = new File ("c:\\myimages\\image1.jpg");

Místní soubor pomocí volání metodu **CloudBlockBlob.upload** nahrajte. První parametr metodu **CloudBlockBlob.upload** je **FileInputStream** objekt, který představuje místní soubor, který se nahraje Azure úložiště. Druhý parametr je velikost, v bajtech souboru.

    blob.upload(new FileInputStream(fileReference), fileReference.length());

Volat Pomocníka s názvem **MakeHTMLPage**k vytvoření základní stránky HTML, který obsahuje ** &lt;obrázek&gt; ** prvek se zdrojem hodnotu, která je teď ve vašem účtu Azure úložiště objektů blob. Kód **MakeHTMLPage** diskuze dál v tomto článku.

    MakeHTMLPage(container);

Vytiskněte zprávu o stavu a informace o stránce vytvořené ve formátu HTML.

    System.out.println("Processing complete.");
    System.out.println("Open index.html to see the images stored in your storage account.");

Ukončit blokování **zkuste** vložením pravá hranatá závorka: **}**

Zpracování následujícími výjimkami:

-   **FileNotFoundException**: můžete vyvolané konstruktory **FileInputStream** nebo **FileOutputStream** .
-   **StorageException**: můžete vyvolané knihovně úložiště Azure klienta.
-   **URISyntaxException**: můžete vyvolané metodou **ListBlobItem.getUri** .
-   **Výjimky**: Obecné výjimek.

<!-- -->

    catch (FileNotFoundException fileNotFoundException)
    {
        System.out.print("FileNotFoundException encountered: ");
        System.out.println(fileNotFoundException.getMessage());
        System.exit(-1);
    }
    catch (StorageException storageException)
    {
        System.out.print("StorageException encountered: ");
        System.out.println(storageException.getMessage());
        System.exit(-1);
    }
    catch (URISyntaxException uriSyntaxException)
    {
        System.out.print("URISyntaxException encountered: ");
        System.out.println(uriSyntaxException.getMessage());
        System.exit(-1);
    }
    catch (Exception e)
    {
        System.out.print("Exception encountered: ");
        System.out.println(e.getMessage());
        System.exit(-1);
    }

Zavřete **hlavní** vložením pravá hranatá závorka: **}**

Vytvořte metodu s názvem **MakeHTMLPage** k vytvoření základní stránky HTML. Tento způsob zahrnuje parametr typu **CloudBlobContainer**, která se použije pro iteraci seznamu nahraný objektů BLOB. Tento způsob vyvolají výjimky typu **FileNotFoundException**, které můžete vyvolání **FileOutputStream** konstruktor a **URISyntaxException**, které můžou být vyvolané **ListBlobItem.getUri** metodou. Zahrnout levá hranatá závorka **{**.

    public static void MakeHTMLPage(CloudBlobContainer container) throws FileNotFoundException, URISyntaxException
    {

Vytvoření místní soubor s názvem **index.html**.

    PrintStream stream;
    stream = new PrintStream(new FileOutputStream("index.html"));

Zapisovat do místního souboru přidání v ** &lt;html&gt;**, ** &lt;záhlaví&gt;**, a ** &lt;textu&gt; ** prvky.

    stream.println("<html>");
    stream.println("<header/>");
    stream.println("<body>");

Iteraci seznamu nahraný objektů BLOB. U každé objektů blob stránky HTML vytvořte ** &lt;img&gt; ** prvek, který obsahuje atribut **src** posílat URI objektů blob podle existuje ve vašem účtu Azure úložiště.
I když jste přidali pouze jeden obrázek v tomto příkladu, pokud jste přidali více, způsobem iteraci tento kód všem poznámkám.

Pro zjednodušení v tomto příkladu předpokládá, že každé kulatý nahrané je obrázek. Pokud jste aktualizovali objektů BLOB než obrázky nebo objekty BLOB stránky namísto objektů BLOB blok, upravte kód v případě potřeby.

    // Enumerate the uploaded blobs.
    for (ListBlobItem blobItem : container.listBlobs()) {
    // List each blob as an <img> element in the HTML body.
    stream.println("<img src='" + blobItem.getUri() + "'/><br/>");
    }

Zavřít ** &lt;textu&gt; ** prvek a ** &lt;html&gt; ** prvek.

    stream.println("</body>");
    stream.println("</html>");

Zavřete místní soubor.

    stream.close();

Zavřete **MakeHTMLPage** vložením pravá hranatá závorka: **}**

Zavřete **StorageSample** vložením pravá hranatá závorka: **}**

Následující obrázek je dokončeno kódem v tomto příkladu. Nezapomeňte změňte hodnoty zástupný symbol **vaše\_účtu\_název** a **vaše\_účtu\_klíč** aby použili váš název účtu a klíč účtu.

    import com.microsoft.azure.storage.*;
    import com.microsoft.azure.storage.blob.*;
    import java.io.*;
    import java.net.URISyntaxException;

    // Create an image, c:\myimages\image1.jpg, prior to running this sample.
    // Alternatively, change the value used by the FileInputStream constructor
    // to use a different image path and file that you have already created.
    public class StorageSample {

        public static final String storageConnectionString =
                "DefaultEndpointsProtocol=http;" +
                       "AccountName=your_account_name;" +
                       "AccountKey=your_account_name";

        public static void main(String[] args) {
            try {
                CloudStorageAccount account;
                CloudBlobClient serviceClient;
                CloudBlobContainer container;
                CloudBlockBlob blob;

                account = CloudStorageAccount.parse(storageConnectionString);
                serviceClient = account.createCloudBlobClient();
                // Container name must be lower case.
                container = serviceClient.getContainerReference("gettingstarted");
                container.createIfNotExists();

                // Set anonymous access on the container.
                BlobContainerPermissions containerPermissions;
                containerPermissions = new BlobContainerPermissions();
                containerPermissions.setPublicAccess(BlobContainerPublicAccessType.CONTAINER);
                container.uploadPermissions(containerPermissions);

                // Upload an image file.
                blob = container.getBlockBlobReference("image1.jpg");

                File fileReference = new File("c:\\myimages\\image1.jpg");
                blob.upload(new FileInputStream(fileReference), fileReference.length());

                // At this point the image is uploaded.
                // Next, create an HTML page that lists all of the uploaded images.
                MakeHTMLPage(container);

                System.out.println("Processing complete.");
                System.out.println("Open index.html to see the images stored in your storage account.");

            } catch (FileNotFoundException fileNotFoundException) {
                System.out.print("FileNotFoundException encountered: ");
                System.out.println(fileNotFoundException.getMessage());
                System.exit(-1);
            } catch (StorageException storageException) {
                System.out.print("StorageException encountered: ");
                System.out.println(storageException.getMessage());
                System.exit(-1);
            } catch (URISyntaxException uriSyntaxException) {
                System.out.print("URISyntaxException encountered: ");
                System.out.println(uriSyntaxException.getMessage());
                System.exit(-1);
            } catch (Exception e) {
                System.out.print("Exception encountered: ");
                System.out.println(e.getMessage());
                System.exit(-1);
            }
        }

        // Create an HTML page that can be used to display the uploaded images.
        // This example assumes all of the blobs are for images.
        public static void MakeHTMLPage(CloudBlobContainer container) throws FileNotFoundException, URISyntaxException
        {
            PrintStream stream;
            stream = new PrintStream(new FileOutputStream("index.html"));

            // Create the opening <html>, <header>, and <body> elements.
            stream.println("<html>");
            stream.println("<header/>");
            stream.println("<body>");

            // Enumerate the uploaded blobs.
            for (ListBlobItem blobItem : container.listBlobs()) {
                // List each blob as an <img> element in the HTML body.
                stream.println("<img src='" + blobItem.getUri() + "'/><br/>");
            }

            stream.println("</body>");

            // Complete the <html> element and close the file.
            stream.println("</html>");
            stream.close();
        }
    }

Kromě odeslání souboru místní obrázek Azure úložiště, kód příklad vytvoří namedindex.html místní soubor, který můžete otevřít v prohlížeči zobrazit nahrané obrázek.

Protože kód obsahuje název účtu a klíč účtu, zajistěte, aby byl váš zdrojový kód zabezpečené.

## <a name="to-delete-a-container"></a>Chcete-li odstranit kontejneru

Protože vám bude účtovaná za úložiště, můžete odstranit kontejneru **gettingstarted** po dokončení experimentovat se v tomto příkladu. Pokud chcete odstranit kontejneru, použijte metodu **CloudBlobContainer.delete** .

    container = serviceClient.getContainerReference("gettingstarted");
    container.delete();

Kontaktování metodu **CloudBlobContainer.delete** proces inicializace **CloudStorageAccount** **ClodBlobClient**a **CloudBlobContainer** objektů je stejný jako u metody **createIfNotExist** . Následujícím obrázku je kompletní příklad, který slouží k odstranění kontejner **gettingstarted**.

    import com.microsoft.azure.storage.*;
    import com.microsoft.azure.storage.blob.*;

    public class DeleteContainer {

        public static final String storageConnectionString =
                "DefaultEndpointsProtocol=http;" +
                   "AccountName=your_account_name;" +
                   "AccountKey=your_account_key";

        public static void main(String[] args)
        {
            try
            {
                CloudStorageAccount account;
                CloudBlobClient serviceClient;
                CloudBlobContainer container;

                account = CloudStorageAccount.parse(storageConnectionString);
                serviceClient = account.createCloudBlobClient();
                // Container name must be lower case.
                container = serviceClient.getContainerReference("gettingstarted");
                container.delete();

                System.out.println("Container deleted.");

            }
            catch (StorageException storageException)
            {
                System.out.print("StorageException encountered: ");
                System.out.println(storageException.getMessage());
                System.exit(-1);
            }
            catch (Exception e)
            {
                System.out.print("Exception encountered: ");
                System.out.println(e.getMessage());
                System.exit(-1);
            }
        }
    }

Základní informace o jiných třídy úložiště objektů blob a metod najdete v článku [Použití úložiště objektů Blob z Java](storage-java-how-to-use-blob-storage.md).

## <a name="next-steps"></a>Další kroky

Tyto odkazy vedou na další informace o složitější úlohy úložiště.

- [Azure úložiště SDK jazyka Java](https://github.com/azure/azure-storage-java)
- [Azure úložiště klienta SDK Reference](http://dl.windowsazure.com/storage/javadoc/)
- [Služby Azure úložiště rozhraní REST API](https://msdn.microsoft.com/library/azure/dd179355.aspx)
- [Blog týmu Azure úložiště](http://blogs.msdn.com/b/windowsazurestorage/)
