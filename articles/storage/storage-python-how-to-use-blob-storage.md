<properties
    pageTitle="Použití úložiště objektů Blob Azure (objektu úložiště) z Python | Microsoft Azure"
    description="Obsahují Nestrukturovaná data v cloudu pomocí úložiště objektů Blob Azure (objektu úložiště)."
    services="storage"
    documentationCenter="python"
    authors="tamram"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="python"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="tamram"/>

# <a name="how-to-use-azure-blob-storage-from-python"></a>Použití úložiště objektů Blob Azure z Python

[AZURE.INCLUDE [storage-selector-blob-include](../../includes/storage-selector-blob-include.md)]
<br/>
[AZURE.INCLUDE [storage-try-azure-tools-blobs](../../includes/storage-try-azure-tools-blobs.md)]

## <a name="overview"></a>Základní informace

Úložiště objektů Blob Azure je služba, která ukládá jako objekty/objektů BLOB Nestrukturovaná data v cloudu. Úložiště objektů BLOB mohou být uloženy libovolné typu text nebo binární data, jako je dokument, soubor media nebo instalační program aplikace. Úložiště objektů blob se taky nazývá úložiště objektu.

Tento článek vám ukáže, jak provádět běžné scénáře pomocí úložiště objektů Blob. Vzorky psaných Python a používat [Microsoft Azure úložiště SDK Python]. Scénáře nichž se uplatní zahrnují nahrávání záznam, stažení a odstranění objektů BLOB.

[AZURE.INCLUDE [storage-blob-concepts-include](../../includes/storage-blob-concepts-include.md)]

[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="create-a-container"></a>Vytvoření kontejneru

Závislosti na použitém typu objektů blob, kterou chcete použít, vytvořte **BlockBlobService**, **AppendBlobService**nebo **PageBlobService** objektu. Následující kód používá **BlockBlobService** objekt. Přidejte následující v horní části libovolný soubor Python, ve kterém chcete programově přístup k úložišti objektů Blob Azure blokovat.

    from azure.storage.blob import BlockBlobService

Následující kód vytvoří objekt **BlockBlobService** pomocí klíč účtu a název účtu úložiště.  Nahraďte "Můj účet" a "mykey" název účtu a klíče.

    block_blob_service = BlockBlobService(account_name='myaccount', account_key='mykey')

[AZURE.INCLUDE [storage-container-naming-rules-include](../../includes/storage-container-naming-rules-include.md)]

V následujícím příkladu můžete objekt **BlockBlobService** vytvořit kontejner, pokud ho neexistuje.

    block_blob_service.create_container('mycontainer')

Ve výchozím nastavení je soukromé, nového kontejneru, ke které je nutné zadat přístupová klávesa úložiště (stejně jako výše) si chcete stáhnout objektů BLOB tento kontejner. Pokud chcete zpřístupnit objektů BLOB v kontejneru, můžete vytvořit kontejner a předejte úroveň přístupu veřejné pomocí následující kód.

    from azure.storage.blob import PublicAccess
    block_blob_service.create_container('mycontainer', public_access=PublicAccess.Container)

Můžete také upravit kontejneru po vytvoření pomocí následující kód.

    block_blob_service.set_container_acl('mycontainer', public_access=PublicAccess.Container)

Po provedení této změny, kdo na Internetu zobrazit objektů BLOB v kontejneru veřejné, ale jenom můžete upravit nebo odstranit.

## <a name="upload-a-blob-into-a-container"></a>Nahrání objektů blob do kontejneru

Vytvořte objektů blob bloku a uložte data, můžete **vytvořit\_objektů blob\_z\_cestu**, **vytvořit\_objektů blob\_z\_toku**, **vytvořit\_objektů blob\_z\_bajtů** nebo **vytvořit\_objektů blob\_z\_text** metody. Budou způsoby nejvyšší úrovně, které proveďte nezbytné bloků při velikost dat větší než 64 MB.

**vytvoření\_objektů blob\_z\_cestu** odešle obsah souboru z určené cesty a **vytvořit\_objektů blob\_z\_toku** odešle obsah z už otevřeného souboru/proudu. **vytvoření\_objektů blob\_z\_bajtů** odešle maticových bajty, a **vytvořit\_objektů blob\_z\_text** odešle zadaný text hodnoty na základě zadaný (jako výchozí kódování UTF-8).

Následující příklad odešle obsah souboru **sunset.png** do objektů blob **myblob** .

    from azure.storage.blob import ContentSettings
    block_blob_service.create_blob_from_path(
        'mycontainer',
        'myblockblob',
        'sunset.png',
        content_settings=ContentSettings(content_type='image/png')
                )

## <a name="list-the-blobs-in-a-container"></a>Seznam objektů BLOB v kontejneru

Seznam objektů BLOB v kontejneru, můžete **seznamu\_objektů BLOB** metody. Tento způsob vrátí generátor. Následující kód výstupy **název** každého objektů blob v kontejneru ke konzole.

    generator = block_blob_service.list_blobs('mycontainer')
    for blob in generator:
        print(blob.name)

## <a name="download-blobs"></a>Stáhněte si objekty BLOB

Stažení dat z objektů blob, můžete **získat\_objektů blob\_k\_cestu**, **získat\_objektů blob\_k\_toku**, **získat\_objektů blob\_k\_bajtů**, nebo **získat\_objektů blob\_k\_text**. Budou způsoby nejvyšší úrovně, které proveďte nezbytné bloků při velikost dat větší než 64 MB.

Následující příklad ukazuje, jak pomocí **získat\_objektů blob\_k\_cestu** ke stažení obsah objektů blob **myblob** a uložení souboru **mimo sunset.png** .

    block_blob_service.get_blob_to_path('mycontainer', 'myblockblob', 'out-sunset.png')

## <a name="delete-a-blob"></a>Odstranění objektů blob

Nakonec odstranit objektů blob, zavolejte **delete_blob**.

    block_blob_service.delete_blob('mycontainer', 'myblockblob')

## <a name="writing-to-an-append-blob"></a>Psaní objektů blob připojit

Přidat blob je optimalizována pro připojený operací, jako je protokolování. Jako blok blob objektů blob připojit se skládá ze bloky, ale při přidání nového bloku objektů blob přidávacího je vždy připojen za účelem objektů blob. Nelze aktualizovat nebo odstranit existující bloku v objektů blob připojit. Blokovat ID pro připojený blob nejsou přístupné, stejně jako jsou blok objektů blob.

Každý blok objektů blob přidávacího může být různé velikosti až do velikosti 4 MB a objektů blob přidávacího může obsahovat maximálně 50 000 bloky. Maximální velikosti doručovaných objektů blob přidávacího tedy něco víc než 195 GB (4 MB × 50 000 bloků).

V příkladu níže vytvoří nový objektů blob připojit a připojí některá data, simulace operace jednoduché protokolování.

    from azure.storage.blob import AppendBlobService
    append_blob_service = AppendBlobService(account_name='myaccount', account_key='mykey')

    # The same containers can hold all types of blobs
    append_blob_service.create_container('mycontainer')

    # Append blobs must be created before they are appended to
    append_blob_service.create_blob('mycontainer', 'myappendblob')
    append_blob_service.append_blob_from_text('mycontainer', 'myappendblob', u'Hello, world!')

    append_blob = append_blob_service.get_blob_to_text('mycontainer', 'myappendblob')

## <a name="next-steps"></a>Další kroky

Teď, když jste se naučili základní informace o úložišti objektů Blob, tyto odkazy vedou na další informace.

- [Středisko pro vývojáře Python](/develop/python/)
- [Služby Azure úložiště rozhraní REST API](http://msdn.microsoft.com/library/azure/dd179355)
- [Blog týmu Azure úložiště]
- [Microsoft Azure úložiště SDK Python]

[Blog týmu Azure úložiště]: http://blogs.msdn.com/b/windowsazurestorage/
[Microsoft Azure úložiště SDK Python]: https://github.com/Azure/azure-storage-python
