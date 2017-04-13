<properties
    pageTitle="Nastavení a načtení vlastnosti a metadata pro objekty v úložišti Azure | Microsoft Azure"
    description="Uložit vlastní metadata k objektům v úložišti Azure a nastavte a načíst vlastnosti systému."
    services="storage"
    documentationCenter=""
    authors="tamram"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="tamram"/>

# <a name="set-and-retrieve-properties-and-metadata"></a>Nastavení a načtení vlastnosti a Metadata #

## <a name="overview"></a>Základní informace

Objekty v dialogovém okně Vlastnosti systému podpory Azure úložiště a metadata definované uživatelem, kromě data, která obsahují:

*   **Vlastnosti systému.** Vlastnosti systému existují na jednotlivé úložiště zdroje. Některé z nich může číst nebo nastavit, za jiné se jen pro čtení. V části titulní stránky některé vlastnosti systému odpovídají záhlaví určitých standardních HTTP. Knihovna Azure úložiště klienta udržuje tyto za vás.  

*   **Uživatelem definované metadata.** Uživatelem definované metadat je metadata, která je zadat na daný zdroj, v podobě pár název hodnota. Používání metadat pro ukládání další hodnoty s úložiště prostředku. Tyto hodnoty jsou vlastní pouze pro účely a nemají vliv na chování zdroje.  

Načtení vlastnosti a metadata hodnoty zdroje úložiště je dvou krocích. Předtím, než si můžete přečíst tyto hodnoty, můžete je explicitně načíst tak, že zavoláte metodu **FetchAttributes** .

> [AZURE.IMPORTANT] Pokud voláte metod **FetchAttributes** nejsou vyplněné vlastnost a metadata hodnoty úložiště zdroje. 

## <a name="setting-and-retrieving-properties"></a>Nastavení a ukládání vlastností

K načtení nemovitostí s hodnotou, zavolejte metodu **FetchAttributes** na objektů blob nebo kontejneru k naplnění vlastnosti a potom další hodnoty.

Nastavit vlastnosti objektu, zadejte hodnotu a pak volat metodu **SetProperties** .

Následující příklad vytvoří kontejneru a některé její nemovitostí s hodnotou: zapíše do okna konzoly:

    //Parse the connection string for the storage account.
    const string ConnectionString = "DefaultEndpointsProtocol=https;AccountName=account-name;AccountKey=account-key";
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(ConnectionString);
    
    //Create the service client object for credentialed access to the Blob service.
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

    // Retrieve a reference to a container. 
    CloudBlobContainer container = blobClient.GetContainerReference("mycontainer");

    // Create the container if it does not already exist.
    container.CreateIfNotExists();

    // Fetch container properties and write out their values.
    container.FetchAttributes();
    Console.WriteLine("Properties for container {0}", container.StorageUri.PrimaryUri.ToString());
    Console.WriteLine("LastModifiedUTC: {0}", container.Properties.LastModified.ToString());
    Console.WriteLine("ETag: {0}", container.Properties.ETag);
    Console.WriteLine();

## <a name="setting-and-retrieving-metadata"></a>Nastavení a načítání metadat

Metadata můžete zadat jako jednu nebo více název dvojice objektů blob nebo kontejneru zdroje. Nastavovaná metadata přidejte název dvojice do kolekce **metadat** na zdroje a potom volat metodu **SetMetadata** uložit hodnoty do služby.

> [AZURE.NOTE] Název vaší metadat musí odpovídat konvence pro C# identifikátory.
 
Následující příklad nastaví metadata v kontejneru. Jedna hodnota nastavenou pomocí metody **Přidat** do kolekce. Pomocí syntaxe implicitní klíč/hodnota nastavena hodnotu. Jak jsou platné.

    public static void AddContainerMetadata(CloudBlobContainer container)
    {
        //Add some metadata to the container.
        container.Metadata.Add("docType", "textDocuments");
        container.Metadata["category"] = "guidance";

        //Set the container's metadata.
        container.SetMetadata();
    }

K načtení metadata, zavolejte metodu **FetchAttributes** na objektů blob nebo kontejneru k naplnění kolekce **metadat** a pak si hodnoty, jak je vidět v následujícím příkladu.

    public static void ListContainerMetadata(CloudBlobContainer container)
    {
        //Fetch container attributes in order to populate the container's properties and metadata.
        container.FetchAttributes();

        //Enumerate the container's metadata.
        Console.WriteLine("Container metadata:");
        foreach (var metadataItem in container.Metadata)
        {
            Console.WriteLine("\tKey: {0}", metadataItem.Key);
            Console.WriteLine("\tValue: {0}", metadataItem.Value);
        }
    }

## <a name="see-also"></a>Viz taky  

- [Knihovna Azure úložiště klienta pro referenci .NET](http://msdn.microsoft.com/library/azure/wa_storage_30_reference_home.aspx)
- [Azure úložiště klienta v knihovně .NET balíčku](https://www.nuget.org/packages/WindowsAzure.Storage/) 
