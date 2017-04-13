<properties
    pageTitle="Vytvoření snímku jen pro čtení objektů blob | Microsoft Azure"
    description="Naučte se vytvořit snímek objektů blob k obecnějším údajům objektů blob dat v daném okamžiku v čase. Porozumět tomu, jak je faktura snímky a jak používat minimalizovat kapacita poplatky."
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

# <a name="create-a-blob-snapshot"></a>Vytvoření snímku objektů blob

## <a name="overview"></a>Základní informace

Snímek je jen pro čtení verze objektů blob, které využívá v určitém bodě v čase. Snímky jsou užitečné pro zálohování objektů BLOB. Po vytvoření snímku můžete číst, kopírovat nebo ji odstranit, ale nemůžete změnit.

Snímek objektů blob je stejná jako jeho základní objektů blob objektů blob URI má hodnotu data a **času** přidaným k objektů blob URI abychom označili čas, pro niž byla přijata snímek. Pokud stránku objektů blob URI je například `http://storagesample.core.blob.windows.net/mydrives/myvhd`, snímek URI je podobný `http://storagesample.core.blob.windows.net/mydrives/myvhd?snapshot=2011-03-09T01:42:34.9360000Z`. 

> [AZURE.NOTE] Všechny snímky sdílet základní objektů blob URI. Pouze rozdílu mezi základní objektů blob a snímek hodnotu připojených **data a času** .

Objekt blob může mít libovolný počet snímků. Snímky potrvají, dokud nebude explicitně se odstraní. Snímek nelze outlive základní blob. Umožňuje zobrazit výčet snímky přidružené základní objektů blob ke sledování aktuální snímky.

Při vytváření snímek objektů blob vlastnosti systému objektů blob zkopírují do snímku se stejnými hodnotami. Základní objektů blob metadat zkopíruje se i na snímek, pokud nezadáte samostatné metadat pro snímek po jeho vytvoření.

Všechny zapůjčení přidružené základní objektů blob nemají vliv na snímku. Nelze získat zapůjčenou na snímku.

## <a name="create-a-snapshot"></a>Vytvoření snímku

Následující příklad ukazuje, jak vytvořit snímek v .NET. V tomto příkladu určuje samostatné metadat pro snímek, pokud už je vytvořená.

    private static async Task CreateBlockBlobSnapshot(CloudBlobContainer container)
    {
        // Create a new block blob in the container.
        CloudBlockBlob baseBlob = container.GetBlockBlobReference("sample-base-blob.txt");

        // Add blob metadata.
        baseBlob.Metadata.Add("ApproxBlobCreatedDate", DateTime.UtcNow.ToString());

        try
        {
            // Upload the blob to create it, with its metadata.
            await baseBlob.UploadTextAsync(string.Format("Base blob: {0}", baseBlob.Uri.ToString()));

            // Sleep 5 seconds.
            System.Threading.Thread.Sleep(5000);

            // Create a snapshot of the base blob.
            // Specify metadata at the time that the snapshot is created to specify unique metadata for the snapshot.
            // If no metadata is specified when the snapshot is created, the base blob's metadata is copied to the snapshot.
            Dictionary<string, string> metadata = new Dictionary<string, string>();
            metadata.Add("ApproxSnapshotCreatedDate", DateTime.UtcNow.ToString());
            await baseBlob.CreateSnapshotAsync(metadata, null, null, null);
        }
        catch (StorageException e)
        {
            Console.WriteLine(e.Message);
            Console.ReadLine();
            throw;
        }
    }
 

## <a name="copy-snapshots"></a>Zkopírujte snímky

Kopírování týkající se snímky a objekty BLOB řiďte se těmihle kroky:

- Snímek můžete zkopírovat základní blob. Podpora snímek místo základní objektů blob, můžete obnovit dřívější verzi aplikace objektů blob. Snímek zůstane, ale základu objektů blob se přepíše zapisovatelný kopii snímku.

- Snímek můžete zkopírovat do cílového objektů blob s jiným názvem. Výsledný objektů blob cíl je zapisovatelný objektů blob a ne snímek.

- Kopírování objektů blob zdroj všechny snímky objektů blob zdroj nezkopírují do cíle. Při určení objektů blob dojde k přepsání kopii, snímky přidružený k původní objektů blob cíl zůstat beze změny.

- Při vytváření snímek objektů blob blok zkopíruje se i objektů blob – seznam blokování potvrzené ve snímku. Nepotvrzené bloky nezkopírují.

## <a name="specify-an-access-condition"></a>Podmínka přístup

Aplikace access můžete podmínka tak, aby snímek je vytvořen jenom v případě splnění podmínky. Chcete-li zadat podmínku přístup, použijte vlastnost **AccessCondition** . Pokud není splněny zadané podmínky, snímek nevytvoří a službu objektů Blob vrátí stavový kód HTTPStatusCode.PreconditionFailed.

## <a name="delete-snapshots"></a>Odstranění snímků

Kulatý se snímky nelze odstranit, pokud budou odstraněny také snímky. Odstranění snímku jednotlivě nebo určit, že všechny snímky odeberou, když se odstraní objektů blob zdroje. Pokud se pokusíte odstranit objektů blob pořád obsahující snímky, bude výsledkem chyba.

Následující příklad ukazuje, jak odstranit objektů blob a jeho snímků v .NET, kde `blockBlob` je proměnné typu **CloudBlockBlob**:

    await blockBlob.DeleteIfExistsAsync(DeleteSnapshotsOption.IncludeSnapshots, null, null, null);

## <a name="snapshots-with-azure-premium-storage"></a>Snímky s úložištěm Azure Premium

Použití snímků s sledovat úložiště Premium tato pravidla:

- Maximální počet snímků na stránce blob v účtu úložiště premium je 100. Pokud toto omezení je překročena, operaci objektů Blob snímek vrátí kód chyby 409 (**SnapshotCountExceeded**).

- Snímek stránky objektů blob může trvat v účtu úložiště premium každých 10 minut. Pokud překročení tato sazba operace objektů Blob snímek vrátí kód chyby 409 (**SnaphotOperationRateExceeded**).

- Získání objektů Blob číst snímek objektů blob stránky v účtu úložiště premium nelze volat. Volání získat objektů Blob na snímek v účtu úložiště premium vrátí kód chyby 400 (**InvalidOperation**). Však můžete volat získat vlastnosti objektů Blob a získat Metadata objektů Blob proti snímek v účtu úložiště premium.

- Číst snímku, můžete kopírovat Blob operace Kopírovat snímek do jiného objektů blob stránky v okně účet. Určení objektů blob pro kopírování nesmí mít všechny stávající snímky. Máte objektů blob cíl snímky, operaci objektů Blob kopírovat vrátí kód chyby 409 (**SnapshotsPresent**).

## <a name="return-the-absolute-uri-to-a-snapshot"></a>Vrátí absolutní identifikátor URI do snímku

Tento příklad kódu C# vytvoří snímek a píše absolutní identifikátor URI primární umístění.

    //Create the blob service client object.
    const string ConnectionString = "DefaultEndpointsProtocol=https;AccountName=account-name;AccountKey=account-key";

    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(ConnectionString);
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

    //Get a reference to a container.
    CloudBlobContainer container = blobClient.GetContainerReference("sample-container");
    container.CreateIfNotExists();

    //Get a reference to a blob.
    CloudBlockBlob blob = container.GetBlockBlobReference("sampleblob.txt");
    blob.UploadText("This is a blob.");

    //Create a snapshot of the blob and write out its primary URI.
    CloudBlockBlob blobSnapshot = blob.CreateSnapshot();
    Console.WriteLine(blobSnapshot.SnapshotQualifiedStorageUri.PrimaryUri);

## <a name="understand-how-snapshots-accrue-charges"></a>Vysvětlení, jak snímky nabíhání nákladů

Vytvoření snímku, který je jen pro čtení kopii objektů blob, můžete mít za následek poplatky za další data úložiště ke svému účtu. Při návrhu aplikace, je důležité Uvědomte si, jak tyto náklady mohou metodu nabíhání nákladů tak, že můžete minimalizovat zbytečných nákladů.

### <a name="important-billing-considerations"></a>Důležité fakturace

Následující seznam obsahuje klíčové body vzít v úvahu při vytváření snímku.

- Úložiště účtu poněkud poplatky za jedinečné bloky nebo stránky, ať už jde o objekt blob nebo na snímku. Váš účet není vzniknou dalším poplatkům k snímky přidružené objektů blob, dokud nezměníte objektů blob, na které jsou založeny. Po aktualizaci základní objektů blob diverguje ze svých snímků. V takovém případě vám bude účtovaná za jedinečné bloky nebo na záložních stránkách v každém objektů blob nebo snímku.

- Pokud nahradíte blok v rámci bloku objektů blob, tento blok následně Účtovaná jako blok jedinečný. To platí i v případě blokování má stejné ID bloku a stejná data má na snímku. Po blokování znovu diverguje z jeho protějšky na libovolný snímek a vám bude účtovaná za její data. Platí pro stránku v stránky objektů blob, který bude aktualizována pomocí stejná data.

- Nahrazení objektů blob blok tak, že zavoláte metodu **UploadFile**, **UploadText**, **UploadStream**nebo **UploadByteArray** nahradí všechny bloků v objektů blob. Pokud máte snímek přidružený k této objektů blob všechny bloků v základní objektů blob a snímek teď odchýlit a bude platit pro všechny bloků v obou objektů BLOB. To platí to i v případě dat v základní objektů blob a snímek zůstávají stejné.

- Služba objektů Blob Azure nemá prostředky a zjistit, zda dva bloky obsahují stejná data. Každý blok, který je nahrávání a potvrzeného zpracován jako jedinečné, i když má stejná data a stejné ID blokovat. Protože poplatky metodu nabíhání nákladů pro jedinečné bloky, je důležité zvážit, aktualizace objektů blob, který má snímku za následek další jedinečné bloků a dalším poplatkům.

> [AZURE.NOTE] Doporučené postupy diktování spravovat snímky pečlivě kvůli nižším poplatkům za navíc. Doporučujeme spravovat snímky následujícím způsobem:

> - Odstraňte a znovu vytvořit snímky přidružené objektů blob při každé aktualizaci objektů blob, i když aktualizujete s stejná data, pokud návrhu aplikace vyžaduje udržovat snímky. Odstranění a znova vytvářet snímky objektů blob, zajistíte, že není odchýlit objektů blob a snímků.

> - Pokud udržujete snímků objektů blob a vyhněte se volání **UploadFile** **UploadText**, **UploadStream**nebo **UploadByteArray** aktualizovat objektů blob. Tyto metody Nahradit vše bloků v objektů blob, takže základní objektů blob a snímky odchýlit výrazně. Místo toho co nejnižší počet bloků aktualizace pomocí metody **PutBlock** a **PutBlockList** .


### <a name="snapshot-billing-scenarios"></a>Snímek fakturace scénáře


Tyto scénáře názorně demonstrují způsobu nabíhání nákladů pro blok objektů blob a jeho snímků.

V případě 1 základní objektů blob nebyl aktualizován po pořízení snímku tak, aby náklady jsou za pouze jedinečné bloky 1, 2 a 3.

![Azure prostředků úložiště](./media/storage-blob-snapshots/storage-blob-snapshots-billing-scenario-1.png)

V části Scénář 2 aktualizoval základní objektů blob, ale snímek nebyl. Aktualizováno 3 blokovat, a i když je v ní stejná data a stejné ID, není stejný jako blokovat 3 ve snímku. Účet je výsledkem účtovaná za čtyři bloky.

![Azure prostředků úložiště](./media/storage-blob-snapshots/storage-blob-snapshots-billing-scenario-2.png)

V případě 3 aktualizoval základní objektů blob, ale snímek nebyl. Blok 3 byla nahrazená funkcí bloku 4 v základní objektů blob, ale snímek odráží blok 3. Účet je výsledkem účtovaná za čtyři bloky.

![Azure prostředků úložiště](./media/storage-blob-snapshots/storage-blob-snapshots-billing-scenario-3.png)

V případě 4 základní objektů blob úplně aktualizace a obsahuje žádná z jeho původní bloky. V důsledku toho účet platit pro všechny osm jedinečné bloky. Tento scénář může dojít, pokud používáte metodu aktualizace například **UploadFile**, **UploadText**, **UploadFromStream**nebo **UploadByteArray**, protože těchto postupů nahradit celý obsah objektů blob.

![Azure prostředků úložiště](./media/storage-blob-snapshots/storage-blob-snapshots-billing-scenario-4.png)

## <a name="next-steps"></a>Další kroky

Další příklady použití úložiště objektů Blob najdete v článku [Ukázky Azure](https://azure.microsoft.com/documentation/samples/?service=storage&term=blob). Můžete stáhnout ukázku aplikace a spuštění nebo vyhledat kód na GitHub. 
