<properties
    pageTitle="Správa anonymního přístupu ke čtení kontejnery a objekty BLOB | Microsoft Azure"
    description="Zjistěte, jak zpřístupnit kontejnerů a objektů blob pro anonymní přístup a jak se k nim programově."
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

# <a name="manage-anonymous-read-access-to-containers-and-blobs"></a>Správa anonymního přístupu ke čtení kontejnery a objekty BLOB

## <a name="overview"></a>Základní informace

Ve výchozím nastavení může jenom vlastník účtu úložiště přístup k prostředkům úložiště v rámci tohoto účtu. Úložiště objektů Blob oprávnění se dají nastavit kontejneru pro povolení anonymního přístupu ke čtení kontejneru a jeho objektů BLOB tak, aby udělit přístup k těmto zdrojům přitom sdílet svůj klíč účtu.

Anonymní přístup je nejvhodnější pro scénáře, kde se má určité objekty BLOB vždy je k dispozici pro anonymní přístup pro čtení. Ovládacího prvku podrobně odstupňovaná můžete vytvořit sdílený přístup podpis, který vám umožní přístup delegáta omezení použití jiná oprávnění a přes zadaný časový interval. Další informace o vytváření podpisů sdílený přístup najdete v tématu [Používání sdílených přístup podpisy (přidružení zabezpečení)](storage-dotnet-shared-access-signature-part-1.md).

## <a name="grant-anonymous-users-permissions-to-containers-and-blobs"></a>Udělení oprávnění anonymní uživatelé kontejnery a objekty BLOB

Ve výchozím nastavení kontejneru a všechny objekty BLOB v něm obsažené může být přístupné jenom vlastník účtu úložiště. Udělit anonymním uživatelům oprávnění kontejneru a jeho objektů BLOB jen pro čtení, můžete nastavit kontejneru oprávnění, která chcete zpřístupnit veřejnosti. Anonymní uživatelé mohou číst objektů BLOB uvnitř kontejneru veřejně přístupný bez ověření žádost.

Kontejnery k dispozici následující možnosti pro správu přístupu kontejner:

- **Plný veřejné přístup pro čtení:** Kontejner a objektů blob dat může číst prostřednictvím anonymní žádosti. Klienti můžete vytvořit výčet objektů BLOB v kontejneru prostřednictvím anonymní požadavku, ale nelze vytvořit výčet kontejnery v rámci účtu úložiště.

- **Veřejného přístupem pro objekty BLOB pouze pro čtení:** Objektů BLOB data v tomto kontejneru můžete číst prostřednictvím anonymní požadavku, ale kontejneru dat není k dispozici. Klienti nelze vytvořit výčet objektů BLOB v kontejneru prostřednictvím anonymní žádosti.

- **Žádné veřejné číst přístup:** Kontejner a objektů blob dat může číst jenom vlastník účtu.

Kontejner oprávnění můžete nastavit takto:

- Z [Azure portálu](https://portal.azure.com).
- Programu pomocí knihovně úložiště klienta nebo rozhraní REST API.
- Pomocí prostředí PowerShell. Další informace o nastavení oprávnění kontejneru z Azure Powershellu najdete v tématu [přes Azure s úložištěm Azure](storage-powershell-guide-full.md#how-to-manage-azure-blobs).

### <a name="setting-container-permissions-from-the-azure-portal"></a>Nastavení oprávnění kontejneru z portálu Microsoft Azure

Pokud chcete nastavit oprávnění kontejneru z [Portálu Azure](https://portal.azure.com), postupujte takto:

1. Přejděte na řídicí panel účtu úložiště.
2. Vyberte jméno container ze seznamu. Klepnutím na název zpřístupňuje objektů BLOB ve zvoleném kontejneru
3. Na panelu nástrojů vyberte **zásady přístupu** .
4. V poli **Typ přístupu** vyberte požadovanou úroveň oprávnění, jak je znázorněno níže snímek.

    ![Úprava metadat kontejneru dialogu](./media/storage-manage-access-to-resources/storage-manage-access-to-resources-0.png)

### <a name="setting-container-permissions-programmatically-using-net"></a>Nastavení oprávnění kontejneru programově pomocí .NET

Nastavení oprávnění pro kontejneru pomocí klienta knihovny .NET, nejdřív načítejte existující oprávnění kontejneru tak, že zavoláte metodu **GetPermissions** . Nastavte vlastnost **PublicAccess** **BlobContainerPermissions** objektu, který vám vrátil metodu **GetPermissions** . Nakonec zavolejte metodu **měli** s aktualizovanými oprávnění.

Následující příklad nastaví kontejneru oprávnění Úplné veřejné přístup pro čtení. Nastavení oprávnění veřejnou přístup pro čtení pro objekty BLOB pouze, nastavte vlastnost **PublicAccess** na **BlobContainerPublicAccessType.Blob**. Chcete-li odebrat všechna oprávnění pro anonymní uživatele, nastavte vlastnost na **BlobContainerPublicAccessType.Off**.

    public static void SetPublicContainerPermissions(CloudBlobContainer container)
    {
        BlobContainerPermissions permissions = container.GetPermissions();
        permissions.PublicAccess = BlobContainerPublicAccessType.Container;
        container.SetPermissions(permissions);
    }

## <a name="access-containers-and-blobs-anonymously"></a>Anonymní přístup k kontejnery a objekty BLOB

Klienta, který má přístup k kontejnerů a objektů BLOB anonymně můžete použít konstruktorů, které není nutné zadávat přihlašovací údaje. Následující příklady ilustrují několika různými způsoby anonymně neodkazuje objektů Blob služby zdrojů.

### <a name="create-an-anonymous-client-object"></a>Vytvoření objektu anonymních

Můžete vytvořit nový objekt klienta služby pro anonymní přístup zadáním koncový bod služby objektů Blob pro účet. Musíte však také znát jméno container v tomto účtu, který je k dispozici pro anonymní přístup.

    public static void CreateAnonymousBlobClient()
    {
        // Create the client object using the Blob service endpoint.
        CloudBlobClient blobClient = new CloudBlobClient(new Uri(@"https://storagesample.blob.core.windows.net"));

        // Get a reference to a container that's available for anonymous access.
        CloudBlobContainer container = blobClient.GetContainerReference("sample-container");

        // Read the container's properties. Note this is only possible when the container supports full public read access.
        container.FetchAttributes();
        Console.WriteLine(container.Properties.LastModified);
        Console.WriteLine(container.Properties.ETag);
    }

### <a name="reference-a-container-anonymously"></a>Vytvořte odkaz kontejneru anonymně

Pokud máte adresu URL do kontejneru anonymně neexistuje, můžete ho přímo odkazují kontejner.

    public static void ListBlobsAnonymously()
    {
        // Get a reference to a container that's available for anonymous access.
        CloudBlobContainer container = new CloudBlobContainer(new Uri(@"https://storagesample.blob.core.windows.net/sample-container"));

        // List blobs in the container.
        foreach (IListBlobItem blobItem in container.ListBlobs())
        {
            Console.WriteLine(blobItem.Uri);
        }
    }


### <a name="reference-a-blob-anonymously"></a>Odkaz objektů blob anonymně

Pokud máte adresu URL objektů blob, který je k dispozici pro anonymní přístup, můžete odkázat objektů blob přímo pomocí této adresy URL:

    public static void DownloadBlobAnonymously()
    {
        CloudBlockBlob blob = new CloudBlockBlob(new Uri(@"https://storagesample.blob.core.windows.net/sample-container/logfile.txt"));
        blob.DownloadToFile(@"C:\Temp\logfile.txt", System.IO.FileMode.Create);
    }

## <a name="features-available-to-anonymous-users"></a>Funkce dostupné pro anonymní uživatele

Následující tabulka uvádí operací, které lze svolat anonymní uživatelé při nastavení kontejner ACL zpřístupnit veřejnosti.

| ZBÝVAJÍCÍ operace                                         | Oprávnění pro čtení důkladné veřejné | Oprávnění s veřejné přístup pro čtení pro objekty BLOB pouze |
|--------------------------------------------------------|-----------------------------------------|---------------------------------------------------|
| Seznam kontejnery                                        | Jenom vlastník                              | Jenom vlastník                                        |
| Vytvoření kontejneru                                       | Jenom vlastník                              | Jenom vlastník                                        |
| Získání kontejneru vlastnosti                               | Všechny                                     | Jenom vlastník                                        |
| Získání kontejneru metadat                                 | Všechny                                     | Jenom vlastník                                        |
| Nastavit kontejneru Metadata                                 | Jenom vlastník                              | Jenom vlastník                                        |
| Získání kontejneru ACL                                      | Jenom vlastník                              | Jenom vlastník                                        |
| Nastavení kontejneru ACL                                      | Jenom vlastník                              | Jenom vlastník                                        |
| Odstranění kontejneru                                       | Jenom vlastník                              | Jenom vlastník                                        |
| Seznam objekty BLOB                                             | Všechny                                     | Jenom vlastník                                        |
| Umístění objektů Blob                                               | Jenom vlastník                              | Jenom vlastník                                        |
| Získání objektů Blob                                               | Všechny                                     | Všechny                                               |
| Získání objektů Blob vlastnosti                                    | Všechny                                     | Všechny                                               |
| Nastavení vlastností objektů Blob                                    | Jenom vlastník                              | Jenom vlastník                                        |
| Získat objektů Blob Metadata                                      | Všechny                                     | Všechny                                               |
| Nastavit objektů Blob Metadata                                      | Jenom vlastník                              | Jenom vlastník                                        |
| Vložit blok                                              | Jenom vlastník                              | Jenom vlastník                                        |
| Získání seznamu blokovaných adres (pouze potvrzené bloky)                 | Všechny                                     | Všechny                                               |
| Získání seznamu blokovaných adres (nepotvrzené bloky nebo všechny bloky) | Jenom vlastník                              | Jenom vlastník                                        |
| Vložit – seznam blokování                                         | Jenom vlastník                              | Jenom vlastník                                        |
| Odstranění objektů Blob                                            | Jenom vlastník                              | Jenom vlastník                                        |
| Kopírování objektů Blob                                              | Jenom vlastník                              | Jenom vlastník                                        |
| Snímek Blob                                          | Jenom vlastník                              | Jenom vlastník                                        |
| Kulatý zapůjčení                                             | Jenom vlastník                              | Jenom vlastník                                        |
| Vložit stránku                                               | Jenom vlastník                              | Jenom vlastník                                        |
| Získání rozsahů                                        | Všechny                                     | Všechny                                                  |
| Přidání objektů Blob                                            | Jenom vlastník                              | Jenom vlastník                                                  |


## <a name="see-also"></a>Viz taky

- [Ověřování služby Azure úložiště](https://msdn.microsoft.com/library/azure/dd179428.aspx)
- [Používání podpisů sdílený přístup (přidružení zabezpečení)](storage-dotnet-shared-access-signature-part-1.md)
- [Delegování přístupu k podpisu sdílený přístup](https://msdn.microsoft.com/library/azure/ee395415.aspx)
