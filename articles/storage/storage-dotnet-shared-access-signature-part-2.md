<properties
    pageTitle="Kam zmizely přidružení zabezpečení s úložiště objektů Blob | Microsoft Azure"
    description="Tento kurz se dozvíte, jak vytvořit podpisy sdílený přístup pro použití s úložiště objektů Blob a jak používat v klientských aplikacích."
    services="storage"
    documentationCenter=""
    authors="tamram"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="tamram"/>


# <a name="shared-access-signatures-part-2-create-and-use-a-sas-with-blob-storage"></a>Sdílené podpisy Accessu, část 2: Vytvoření a používání přidružení zabezpečení s úložiště objektů Blob

## <a name="overview"></a>Základní informace

[Část 1](storage-dotnet-shared-access-signature-part-1.md) tento kurz prozkoumat sdílený přístup podpisy (přidružení zabezpečení) a vysvětlení osvědčené postupy pro používání je. Část 2 se dozvíte, jak vytvořit a pak používat sdílený přístup podpisy úložiště objektů Blob. Příklady napsané v jazyce C# a použít knihovnu Azure úložiště klienta pro .NET. Scénáře nichž se uplatní zahrnout tyto aspekty práce s sdílených přístup podpisy:

- Generování podpis sdílený přístup do kontejneru
- Generování podpisu sdílený přístup na objekt blob
- Vytvoření zásad uložené přístupu ke správě podpisů v kontejneru na zdroje
- Testování podpisy sdílený přístup z klientské aplikace

## <a name="about-this-tutorial"></a>O tomto kurzu

V tomto kurzu budeme se zaměřit na vytváření podpisů sdílený přístup pro kontejnery s objekty BLOB vytvořením dvě aplikace konzoly. První aplikace konzoly vygeneruje podpisy sdílený přístup na kontejner a na objekt blob. Tato aplikace obsahuje informace o účtu klíčích úložiště. Druhý aplikace konzoly, které bude sloužit jako klientské aplikaci, přistupovat ke kontejneru a objektů blob zdroje používání podpisů sdílené access vytvořená pomocí první aplikace. Tato aplikace používá podpisy sdílený přístup jenom pro ověření přístup do kontejneru a objektů blob zdroje: nemuselo mít informace o účtu klíčích.

## <a name="part-1-create-a-console-application-to-generate-shared-access-signatures"></a>Část 1: Vytvoření aplikace konzoly generovat sdílený přístup podpisy

Nejdřív zajištění knihovnu Azure úložiště klienta pro .NET nainstalovaný. Nainstalujete [NuGet balíčku](http://nuget.org/packages/WindowsAzure.Storage/ "NuGet balíček") obsahující aktuální sestavení pro knihovnu klienta; Toto je doporučeno pro zajištění posledních opravy. Můžete také stáhnout knihovnu klienta jako součást nejnovější verzi [Azure SDK pro .NET](https://azure.microsoft.com/downloads/).

Ve Visual Studiu vytvořte nové aplikace konzoly Windows a pojmenujte ho **GenerateSharedAccessSignatures**. Přidání odkazů na **Microsoft.WindowsAzure.Configuration.dll** a **Microsoft.WindowsAzure.Storage.dll**, některým z následujících postupů:

-   Pokud chcete nainstalovat balíček NuGet, nejprve nainstalujte [NuGet klienta](https://docs.nuget.org/consume/installing-nuget). Ve Visual Studiu, vyberte **projektu | Správa NuGet balíčků**Hledat online **Úložiště Azure**a postupujte podle pokynů nainstalujte.
-   Můžete taky vyhledejte sestavení v nainstalované Azure SDK a přidejte odkazy na ně.

V horní části souboru Program.cs přidejte následující příkazy **pomocí** :

    using System.IO;    
    using Microsoft.WindowsAzure;
    using Microsoft.WindowsAzure.Storage;
    using Microsoft.WindowsAzure.Storage.Blob;

Úprava konfiguračního souboru tak, aby obsahoval nastavení s připojovací řetězec, který odkazuje na účtu úložiště. Soubor app.config by měla vypadat podobně jako tento:

    <configuration>
      <startup>
        <supportedRuntime version="v4.0" sku=".NETFramework,Version=v4.5" />
      </startup>
      <appSettings>
        <add key="StorageConnectionString" value="DefaultEndpointsProtocol=https;AccountName=myaccount;AccountKey=mykey"/>
      </appSettings>
    </configuration>

### <a name="generate-a-shared-access-signature-uri-for-a-container"></a>Generování sdílený přístup podpisu URI pro kontejneru

Na začátku přidáme způsob, jak generovat podpis sdílený přístup do nového kontejneru. V tomto případě podpis je přidružené k uložené přístupu, takže vykonává identifikátor URI informace uveďte její čas vypršení a oprávnění, která zajišťuje.

Nejdřív přidejte kód **Main()** metody ověřování přístup ke svému účtu úložiště a vytvořit nové kontejner:

    static void Main(string[] args)
    {
        //Parse the connection string and return a reference to the storage account.
        CloudStorageAccount storageAccount = CloudStorageAccount.Parse(CloudConfigurationManager.GetSetting("StorageConnectionString"));

        //Create the blob client object.
        CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

        //Get a reference to a container to use for the sample code, and create it if it does not exist.
        CloudBlobContainer container = blobClient.GetContainerReference("sascontainer");
        container.CreateIfNotExists();

        //Insert calls to the methods created below here...

        //Require user input before closing the console window.
        Console.ReadLine();
    }

Dále přidejte nová metoda vygeneruje podpis sdílený přístup pro kontejner a vrátí podpis URI:

    static string GetContainerSasUri(CloudBlobContainer container)
    {
        //Set the expiry time and permissions for the container.
        //In this case no start time is specified, so the shared access signature becomes valid immediately.
        SharedAccessBlobPolicy sasConstraints = new SharedAccessBlobPolicy();
        sasConstraints.SharedAccessExpiryTime = DateTime.UtcNow.AddHours(24);
        sasConstraints.Permissions = SharedAccessBlobPermissions.Write | SharedAccessBlobPermissions.List;

        //Generate the shared access signature on the container, setting the constraints directly on the signature.
        string sasContainerToken = container.GetSharedAccessSignature(sasConstraints);

        //Return the URI string for the container, including the SAS token.
        return container.Uri + sasContainerToken;
    }

Přidejte následující řádky v dolní části Metoda **Main()** před voláním **Console.ReadLine()**volat **GetContainerSasUri()** a podepsat URI do okna konzoly:

    //Generate a SAS URI for the container, without a stored access policy.
    Console.WriteLine("Container SAS URI: " + GetContainerSasUri(container));
    Console.WriteLine();

Kompilace a spusťte výstup sdílený přístup podpis URI pro nové kontejner. Identifikátor URI bude vypadat podobně jako následující URI:       

    https://storageaccount.blob.core.windows.net/sascontainer?sv=2012-02-12&se=2013-04-13T00%3A12%3A08Z&sr=c&sp=wl&sig=t%2BbzU9%2B7ry4okULN9S0wst%2F8MCUhTjrHyV9rDNLSe8g%3D

Po spuštění kód sdílený přístup podpis, který jste vytvořili v kontejneru bude platit pro další 24 hodin. Podpis udělí oprávnění klienta do seznamu objektů BLOB v kontejneru a napsat nový objektů blob do kontejneru.

### <a name="generate-a-shared-access-signature-uri-for-a-blob"></a>Generování sdílený přístup podpisu URI pro objekt Blob

Pak budete psaní podobný kód vytváření nových objektů blob v kontejneru a generovat podpis sdílený přístup pro něj. Tento sdílený přístup podpis přidružen není zásadu uložené přístup tak, aby obsahoval čas zahájení, čas vypršení a informace o oprávněních v identifikátor URI.

Přidejte nový metodu, která vytvoří novou objektů blob a napište nějaký text, pak vygeneruje podpis sdílený přístup a vrátí podpis URI:

    static string GetBlobSasUri(CloudBlobContainer container)
    {
        //Get a reference to a blob within the container.
        CloudBlockBlob blob = container.GetBlockBlobReference("sasblob.txt");

        //Upload text to the blob. If the blob does not yet exist, it will be created.
        //If the blob does exist, its existing content will be overwritten.
        string blobContent = "This blob will be accessible to clients via a Shared Access Signature.";
        MemoryStream ms = new MemoryStream(Encoding.UTF8.GetBytes(blobContent));
        ms.Position = 0;
        using (ms)
        {
            blob.UploadFromStream(ms);
        }

        //Set the expiry time and permissions for the blob.
        //In this case the start time is specified as a few minutes in the past, to mitigate clock skew.
        //The shared access signature will be valid immediately.
        SharedAccessBlobPolicy sasConstraints = new SharedAccessBlobPolicy();
        sasConstraints.SharedAccessStartTime = DateTime.UtcNow.AddMinutes(-5);
        sasConstraints.SharedAccessExpiryTime = DateTime.UtcNow.AddHours(24);
        sasConstraints.Permissions = SharedAccessBlobPermissions.Read | SharedAccessBlobPermissions.Write;

        //Generate the shared access signature on the blob, setting the constraints directly on the signature.
        string sasBlobToken = blob.GetSharedAccessSignature(sasConstraints);

        //Return the URI string for the container, including the SAS token.
        return blob.Uri + sasBlobToken;
    }

V dolní části Metoda **Main()** přidejte následující řádky volání **GetBlobSasUri()**před voláním **Console.ReadLine()**a zapsat podpis sdílený přístup URI do okna konzoly:    

    //Generate a SAS URI for a blob within the container, without a stored access policy.
    Console.WriteLine("Blob SAS URI: " + GetBlobSasUri(container));
    Console.WriteLine();


Kompilace a spusťte výstup sdílený přístup podpis URI pro nové objektů blob. Identifikátor URI bude vypadat podobně jako následující URI:

    https://storageaccount.blob.core.windows.net/sascontainer/sasblob.txt?sv=2012-02-12&st=2013-04-12T23%3A37%3A08Z&se=2013-04-13T00%3A12%3A08Z&sr=b&sp=rw&sig=dF2064yHtc8RusQLvkQFPItYdeOz3zR8zHsDMBi4S30%3D

### <a name="create-a-stored-access-policy-on-the-container"></a>Vytvoření zásad uložené přístup v kontejneru

Teď na kontejner, který určí omezení pro všechny sdílené přístup podpisy, které jsou přidružené k jeho vytvoření uložené přístupu.

V předchozích příkladech jsme zadaný počáteční čas (implicitně nebo explicitně), doby vypršení platnosti a oprávnění na sdílený přístup podpis URI samotné. V následujícím příkladu budeme specifikovat tyto na zásady přístupu uložené a ne na podpis sdílený přístup. Umožní nám chcete-li změnit tato omezení bez opakovaným podpis sdílený přístup.

Je možné nastala některá omezení pro sdílené přístup podpisu a zbytek na uložené přístupu. Však můžete zadáte pouze čas zahájení, dobu platnosti a oprávnění v jednom místě nebo jiné; například nelze zadat oprávnění na podpis sdílený přístup a je taky zadat na uložené přístupu.

Všimněte si, že při přidání zásady přístupu ke kontejneru musíte získání kontejneru existující oprávnění, přidat nové zásady přístupu a pak nastavení oprávnění na kontejner.

Přidání nového způsobu vytvoří nové zásady přístupu uložené v kontejneru a vrátí název zásady:

    static void CreateSharedAccessPolicy(CloudBlobClient blobClient, CloudBlobContainer container,
        string policyName)
    {
        //Create a new shared access policy and define its constraints.
        SharedAccessBlobPolicy sharedPolicy = new SharedAccessBlobPolicy()
        {
            SharedAccessExpiryTime = DateTime.UtcNow.AddHours(24),
            Permissions = SharedAccessBlobPermissions.Write | SharedAccessBlobPermissions.List | SharedAccessBlobPermissions.Read
        };

        //Get the container's existing permissions.
        BlobContainerPermissions permissions = container.GetPermissions();

        //Add the new policy to the container's permissions, and set the container's permissions.
        permissions.SharedAccessPolicies.Add(policyName, sharedPolicy);
        container.SetPermissions(permissions);
    }

V dolní části Metoda **Main()** před voláním **Console.ReadLine()**přidejte následující řádky na první vymazat všechny existující zásady přístupu a potom volat metodu **CreateSharedAccessPolicy()** :    

    //Clear any existing access policies on container.
    BlobContainerPermissions perms = container.GetPermissions();
    perms.SharedAccessPolicies.Clear();
    container.SetPermissions(perms);

    //Create a new access policy on the container, which may be optionally used to provide constraints for
    //shared access signatures on the container and the blob.
    string sharedAccessPolicyName = "tutorialpolicy";
    CreateSharedAccessPolicy(blobClient, container, sharedAccessPolicyName);

Všimněte si, že po vymazání zásady přístupu na kontejner musíte nejdřív získání kontejneru existující oprávnění, a pak zrušte zaškrtnutí oprávnění a potom znova nastavit oprávnění.

### <a name="generate-a-shared-access-signature-uri-on-the-container-that-uses-an-access-policy"></a>Generování sdílený přístup podpisu URI na kontejner, který používá zásady přístupu

Dále vytvoříme jiného podpisu sdílený přístup na kontejner, který jsme vytvořili dříve, ale tentokrát jsme budete podpis přidružit zásady přístupu, kterou jsme vytvořili v předchozím příkladu.

Přidání nového způsobu ke generování jiného podpisu sdílený přístup na kontejner:

    static string GetContainerSasUriWithPolicy(CloudBlobContainer container, string policyName)
    {
        //Generate the shared access signature on the container. In this case, all of the constraints for the
        //shared access signature are specified on the stored access policy.
        string sasContainerToken = container.GetSharedAccessSignature(null, policyName);

        //Return the URI string for the container, including the SAS token.
        return container.Uri + sasContainerToken;
    }

V dolní části Metoda **Main()** před voláním **Console.ReadLine()**přidejte následující řádky volat metodu **GetContainerSasUriWithPolicy** :

    //Generate a SAS URI for the container, using a stored access policy to set constraints on the SAS.
    Console.WriteLine("Container SAS URI using stored access policy: " + GetContainerSasUriWithPolicy(container, sharedAccessPolicyName));
    Console.WriteLine();

### <a name="generate-a-shared-access-signature-uri-on-the-blob-that-uses-an-access-policy"></a>Generování sdílený přístup podpisu URI na objektů Blob používající zásady přístupu

Nakonec přidáme podobné metody můžete vytvořit jiné objektů blob a generovat sdílený přístup podpis, který máte přidružený k zásady přístupu.

Přidání nového způsobu a vytvořte objektů blob generovat sdílený přístup podpis:

    static string GetBlobSasUriWithPolicy(CloudBlobContainer container, string policyName)
    {
        //Get a reference to a blob within the container.
        CloudBlockBlob blob = container.GetBlockBlobReference("sasblobpolicy.txt");

        //Upload text to the blob. If the blob does not yet exist, it will be created.
        //If the blob does exist, its existing content will be overwritten.
        string blobContent = "This blob will be accessible to clients via a shared access signature. " +
        "A stored access policy defines the constraints for the signature.";
        MemoryStream ms = new MemoryStream(Encoding.UTF8.GetBytes(blobContent));
        ms.Position = 0;
        using (ms)
        {
            blob.UploadFromStream(ms);
        }

        //Generate the shared access signature on the blob.
        string sasBlobToken = blob.GetSharedAccessSignature(null, policyName);

        //Return the URI string for the container, including the SAS token.
        return blob.Uri + sasBlobToken;
    }

V dolní části Metoda **Main()** před voláním **Console.ReadLine()**přidejte následující řádky volat metodu **GetBlobSasUriWithPolicy** :    

    //Generate a SAS URI for a blob within the container, using a stored access policy to set constraints on the SAS.
    Console.WriteLine("Blob SAS URI using stored access policy: " + GetBlobSasUriWithPolicy(container, sharedAccessPolicyName));
    Console.WriteLine();

Metoda **Main()** by měl vypadat takto zrušíme. Ho spusťte a zapsat podpis sdílený přístup URI do okna konzoly a pak zkopírujte a vložte do textového souboru pro použití v druhé části tohoto kurzu.

    static void Main(string[] args)
    {
        //Parse the connection string and return a reference to the storage account.
        CloudStorageAccount storageAccount = CloudStorageAccount.Parse(CloudConfigurationManager.GetSetting("StorageConnectionString"));

        //Create the blob client object.
        CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

        //Get a reference to a container to use for the sample code, and create it if it does not exist.
        CloudBlobContainer container = blobClient.GetContainerReference("sascontainer");
        container.CreateIfNotExists();

        //Generate a SAS URI for the container, without a stored access policy.
        Console.WriteLine("Container SAS URI: " + GetContainerSasUri(container));
        Console.WriteLine();

        //Generate a SAS URI for a blob within the container, without a stored access policy.
        Console.WriteLine("Blob SAS URI: " + GetBlobSasUri(container));
        Console.WriteLine();

        //Clear any existing access policies on container.
        BlobContainerPermissions perms = container.GetPermissions();
        perms.SharedAccessPolicies.Clear();
        container.SetPermissions(perms);

        //Create a new access policy on the container, which may be optionally used to provide constraints for
        //shared access signatures on the container and the blob.
        string sharedAccessPolicyName = "tutorialpolicy";
        CreateSharedAccessPolicy(blobClient, container, sharedAccessPolicyName);

        //Generate a SAS URI for the container, using a stored access policy to set constraints on the SAS.
        Console.WriteLine("Container SAS URI using stored access policy: " + GetContainerSasUriWithPolicy(container, sharedAccessPolicyName));
        Console.WriteLine();

        //Generate a SAS URI for a blob within the container, using a stored access policy to set constraints on the SAS.
        Console.WriteLine("Blob SAS URI using stored access policy: " + GetBlobSasUriWithPolicy(container, sharedAccessPolicyName));
        Console.WriteLine();

        Console.ReadLine();
    }

Při spuštění aplikace konzoly GenerateSharedAccessSignatures uvidíte výstup v okně konzoly následující kroky. Toto jsou sdílený přístup podpisy, které budete používat v části 2 kurzu.

![přidružení zabezpečení konzoly výstup 1][sas-console-output-1]

## <a name="part-2-create-a-console-application-to-test-the-shared-access-signatures"></a>Část 2: Vytvoření aplikace konzoly otestovat podpisy sdílený přístup

Testování sdílený přístup podpisy vytvořené v předchozích příkladech vytvoříme druhé aplikaci konzoly, která používá podpisy provádět operace v kontejneru a objektů blob.

> [AZURE.NOTE] Pokud delším než 24 hodin uplynou vzhledem k tomu, že jste dokončili první část kurzu, podpisy vytvořeného už bude platit. V tomto případě by měla běžet kód v první aplikaci konzoly generovat podpisy svěže sdílený přístup pro použití v druhé části kurzu.

Ve Visual Studiu vytvořte nové aplikace konzoly Windows a pojmenujte ho **ConsumeSharedAccessSignatures**. Přidáte odkazy na **Microsoft.WindowsAzure.Configuration.dll** a **Microsoft.WindowsAzure.Storage.dll**, stejně jako při dříve.

V horní části souboru Program.cs přidejte následující příkazy **pomocí** :

    using System.IO;
    using Microsoft.WindowsAzure.Storage;
    using Microsoft.WindowsAzure.Storage.Blob;

V těle metodu **Main()** přidejte následující konstanty a aktualizujte jejich hodnoty podpisy sdílený přístup vytvořených v části 1 kurzu.

    static void Main(string[] args)
    {
        string containerSAS = "<your container SAS>";
        string blobSAS = "<your blob SAS>";
        string containerSASWithAccessPolicy = "<your container SAS with access policy>";
        string blobSASWithAccessPolicy = "<your blob SAS with access policy>";
    }

### <a name="add-a-method-to-try-container-operations-using-a-shared-access-signature"></a>Přidání metody zkusit kontejneru operace podpisem sdílený přístup

Budeme dál přidávat metodu, která testuje některé zástupce kontejneru operace podpisem sdílený přístup na kontejner. Všimněte si, že sdílený přístup podpis se používá k vrátit odkaz na kontejner ověřování přístup ke kontejneru založené na samotný podpis.

Přidejte Program.cs následujícím způsobem:

    static void UseContainerSAS(string sas)
    {
        //Try performing container operations with the SAS provided.

        //Return a reference to the container using the SAS URI.
        CloudBlobContainer container = new CloudBlobContainer(new Uri(sas));

        //Create a list to store blob URIs returned by a listing operation on the container.
        List<ICloudBlob> blobList = new List<ICloudBlob>();

        //Write operation: write a new blob to the container.
        try
        {
            CloudBlockBlob blob = container.GetBlockBlobReference("blobCreatedViaSAS.txt");
            string blobContent = "This blob was created with a shared access signature granting write permissions to the container. ";
            MemoryStream msWrite = new MemoryStream(Encoding.UTF8.GetBytes(blobContent));
            msWrite.Position = 0;
            using (msWrite)
            {
                blob.UploadFromStream(msWrite);
            }
            Console.WriteLine("Write operation succeeded for SAS " + sas);
            Console.WriteLine();
        }
        catch (StorageException e)
        {
            Console.WriteLine("Write operation failed for SAS " + sas);
            Console.WriteLine("Additional error information: " + e.Message);
            Console.WriteLine();
        }

        //List operation: List the blobs in the container.
        try
        {
            foreach (ICloudBlob blob in container.ListBlobs())
            {
                blobList.Add(blob);
            }
            Console.WriteLine("List operation succeeded for SAS " + sas);
            Console.WriteLine();
        }
        catch (StorageException e)
        {
            Console.WriteLine("List operation failed for SAS " + sas);
            Console.WriteLine("Additional error information: " + e.Message);
            Console.WriteLine();
        }

        //Read operation: Get a reference to one of the blobs in the container and read it.
        try
        {
            CloudBlockBlob blob = container.GetBlockBlobReference(blobList[0].Name);
            MemoryStream msRead = new MemoryStream();
            msRead.Position = 0;
            using (msRead)
            {
                blob.DownloadToStream(msRead);
                Console.WriteLine(msRead.Length);
            }
            Console.WriteLine("Read operation succeeded for SAS " + sas);
            Console.WriteLine();
        }
        catch (StorageException e)
        {
            Console.WriteLine("Read operation failed for SAS " + sas);
            Console.WriteLine("Additional error information: " + e.Message);
            Console.WriteLine();
        }
        Console.WriteLine();

        //Delete operation: Delete a blob in the container.
        try
        {
            CloudBlockBlob blob = container.GetBlockBlobReference(blobList[0].Name);
            blob.Delete();
            Console.WriteLine("Delete operation succeeded for SAS " + sas);
            Console.WriteLine();
        }
        catch (StorageException e)
        {
            Console.WriteLine("Delete operation failed for SAS " + sas);
            Console.WriteLine("Additional error information: " + e.Message);
            Console.WriteLine();
        }        
    }


Aktualizujte **Main()** způsob kontaktování **UseContainerSAS()** obou podpisů sdílený přístup, které jste vytvořili v kontejneru:

    static void Main(string[] args)
    {
        string containerSAS = "<your container SAS>";
        string blobSAS = "<your blob SAS>";
        string containerSASWithAccessPolicy = "<your container SAS with access policy>";
        string blobSASWithAccessPolicy = "<your blob SAS with access policy>";

        //Call the test methods with the shared access signatures created on the container, with and without the access policy.
        UseContainerSAS(containerSAS);
        UseContainerSAS(containerSASWithAccessPolicy);

        Console.ReadLine();
    }


### <a name="add-a-method-to-try-blob-operations-using-a-shared-access-signature"></a>Přidání metody zkusit objektů Blob operace podpisem sdílený přístup

Nakonec přidáme metodu, která testuje některé zástupce objektů blob operace podpisem sdílený přístup v objektů blob. V tomto případě používáme konstruktoru **CloudBlockBlob(String)**, předejte podpis sdílený přístup vrátit odkaz na objekt blob. Jiné ověřování požaduje; je založena na samotný podpis.

Přidejte Program.cs následujícím způsobem:

    static void UseBlobSAS(string sas)
    {
        //Try performing blob operations using the SAS provided.

        //Return a reference to the blob using the SAS URI.
        CloudBlockBlob blob = new CloudBlockBlob(new Uri(sas));

        //Write operation: Write a new blob to the container.
        try
        {
            string blobContent = "This blob was created with a shared access signature granting write permissions to the blob. ";
            MemoryStream msWrite = new MemoryStream(Encoding.UTF8.GetBytes(blobContent));
            msWrite.Position = 0;
            using (msWrite)
            {
                blob.UploadFromStream(msWrite);
            }
            Console.WriteLine("Write operation succeeded for SAS " + sas);
            Console.WriteLine();
        }
        catch (StorageException e)
        {
            Console.WriteLine("Write operation failed for SAS " + sas);
            Console.WriteLine("Additional error information: " + e.Message);
            Console.WriteLine();
        }

        //Read operation: Read the contents of the blob.
        try
        {
            MemoryStream msRead = new MemoryStream();
            using (msRead)
            {
                blob.DownloadToStream(msRead);
                msRead.Position = 0;
                using (StreamReader reader = new StreamReader(msRead, true))
                {
                    string line;
                    while ((line = reader.ReadLine()) != null)
                    {
                        Console.WriteLine(line);
                    }
                }
            }
            Console.WriteLine("Read operation succeeded for SAS " + sas);
            Console.WriteLine();
        }
        catch (StorageException e)
        {
            Console.WriteLine("Read operation failed for SAS " + sas);
            Console.WriteLine("Additional error information: " + e.Message);
            Console.WriteLine();
        }

        //Delete operation: Delete the blob.
        try
        {
            blob.Delete();
            Console.WriteLine("Delete operation succeeded for SAS " + sas);
            Console.WriteLine();
        }
        catch (StorageException e)
        {
            Console.WriteLine("Delete operation failed for SAS " + sas);
            Console.WriteLine("Additional error information: " + e.Message);
            Console.WriteLine();
        }        
    }


Aktualizujte **Main()** způsob kontaktování **UseBlobSAS()** obou podpisů sdílený přístup, které jste vytvořili v objektů blob:

    static void Main(string[] args)
    {
        string containerSAS = "<your container SAS>";
        string blobSAS = "<your blob SAS>";
        string containerSASWithAccessPolicy = "<your container SAS with access policy>";
        string blobSASWithAccessPolicy = "<your blob SAS with access policy>";

        //Call the test methods with the shared access signatures created on the container, with and without the access policy.
        UseContainerSAS(containerSAS);
        UseContainerSAS(containerSASWithAccessPolicy);

        //Call the test methods with the shared access signatures created on the blob, with and without the access policy.
        UseBlobSAS(blobSAS);
        UseBlobSAS(blobSASWithAccessPolicy);

        Console.ReadLine();
    }

Spusťte aplikaci konzoly a sledujte výstupu zobrazíte operací, které jsou povoleny pro které podpisy. Výstup v okně konzoly bude vypadat podobně jako tento:

![přidružení zabezpečení – konzoly výstup-2][sas-console-output-2]

## <a name="next-steps"></a>Další kroky

[Sdílené podpisy Accessu, část 1: Principy modelu přidružení zabezpečení](storage-dotnet-shared-access-signature-part-1.md)

[Správa anonymního přístupu ke čtení kontejnery a objekty BLOB](storage-manage-access-to-resources.md)

[Delegování přístupu k podpisu sdílený přístup (REST API)](http://msdn.microsoft.com/library/azure/ee395415.aspx)

[Úvod do tabulky a fronty přidružení zabezpečení](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-table-sas-shared-access-signature-queue-sas-and-update-to-blob-sas.aspx)

[sas-console-output-1]: ./media/storage-dotnet-shared-access-signature-part-2/sas-console-output-1.PNG
[sas-console-output-2]: ./media/storage-dotnet-shared-access-signature-part-2/sas-console-output-2.PNG
