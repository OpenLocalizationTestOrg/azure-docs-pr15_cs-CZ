<properties
    pageTitle="Přesunutí dat do a z úložiště objektů Blob Azure pomocí Python | Microsoft Azure"
    description="Přesunutí dat do a z úložiště objektů Blob Azure pomocí Python"
    services="machine-learning,storage"
    documentationCenter=""
    authors="bradsev"
    manager="jhubbard"
    editor="cgronlun" />

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/14/2016"
    ms.author="bradsev" />

# <a name="move-data-to-and-from-azure-blob-storage-using-python"></a>Přesunutí dat do a z úložiště objektů Blob Azure pomocí Python

Toto téma popisuje, jak seznam, nahrávání a stahování objektů BLOB pomocí rozhraní API Python. Rozhraní API Python podle Azure SDK můžete:

- Vytvoření kontejneru
- Nahrání objektů blob do kontejneru
- Stáhněte si objekty BLOB
- Seznam objektů BLOB v kontejneru
- Odstranění objektů blob

Další informace o použití rozhraní API Python najdete v článku [používání služba úložiště objektů Blob z Python](../storage/storage-python-how-to-use-blob-storage.md).

Pokyny pro technologií pro server slouží k přesunutí dat do a/nebo z úložiště objektů Blob Azure jsou propojené tady:

[AZURE.INCLUDE [blob-storage-tool-selector](../../includes/machine-learning-blob-storage-tool-selector.md)]


> [AZURE.NOTE] Pokud používáte OM, který byl nastaven pomocí skriptů poskytovanou [dat pro výzkum virtuálních počítačích v Azure](machine-learning-data-science-virtual-machines.md), pak AzCopy je už nainstalovaná na OM.

> [AZURE.NOTE] Pro dokončení Úvod k úložišti objektů blob Azure odkažte na [Základy objektů Blob Azure](../storage/storage-dotnet-how-to-use-blobs.md) a službě [objektů Blob Azure](https://msdn.microsoft.com/library/azure/dd179376.aspx).


## <a name="prerequisites"></a>Zjistit předpoklady pro

Tento dokument předpokládá, že máte předplatné Azure, účtu úložiště a odpovídající klíč úložiště k tomuto účtu. Před nahrávání nebo stahování dat, musíte znát kód název a účet Azure úložiště účtu.

- Nastavení Azure předplatného, najdete v článku [bezplatnou zkušební verzi jeden měsíc](https://azure.microsoft.com/pricing/free-trial/).

- Pokyny pro vytvoření účtu úložiště a usnadňující účet a klíčové informace podívejte se na [účty adresářové služby Azure o úložiště](../storage/storage-create-storage-account.md).


## <a name="upload-data-to-blob"></a>Odeslání dat do objektů Blob

Přidejte následující úryvek horní jakéhokoli Python kódu, ve kterém chcete programově přístup k úložišti Azure:

    from azure.storage.blob import BlobService

Objekt **BlobService** umožňuje práci s objekty BLOB a kontejnery. Následující kód vytvoří objekt BlobService pomocí klíč účtu a název účtu úložiště. Nahraďte název účtu a klíč účtu s reálným účtu a klíčem.

    blob_service = BlobService(account_name="<your_account_name>", account_key="<your_account_key>")

Odeslání dat do objektů blob pomocí následujících metod:

1. umístění\_blok\_objektů blob\_z\_path (odešle obsah souboru z cesta)
2. umístění\_block_blob\_z\_souboru (odešle obsah z už otevřeného souboru/toku)
3. umístění\_blok\_objektů blob\_z\_bajtů (odesílání e maticových bajtů)
4. umístění\_blok\_objektů blob\_z\_text (odešle zadaný text hodnoty na základě zadané kódování)

Následující ukázkový kód, odešlou místní soubor kontejner:

    blob_service.put_block_blob_from_path("<your_container_name>", "<your_blob_name>", "<your_local_file_name>")

Následující ukázkový kód odešle všechny soubory (s výjimkou adresářů) v místním adresáři k úložišti objektů blob:

    from azure.storage.blob import BlobService
    from os import listdir
    from os.path import isfile, join

    # Set parameters here
    ACCOUNT_NAME = "<your_account_name>"
    ACCOUNT_KEY = "<your_account_key>"
    CONTAINER_NAME = "<your_container_name>"
    LOCAL_DIRECT = "<your_local_directory>"     

    blob_service = BlobService(account_name=ACCOUNT_NAME, account_key=ACCOUNT_KEY)
    # find all files in the LOCAL_DIRECT (excluding directory)
    local_file_list = [f for f in listdir(LOCAL_DIRECT) if isfile(join(LOCAL_DIRECT, f))]

    file_num = len(local_file_list)
    for i in range(file_num):
        local_file = join(LOCAL_DIRECT, local_file_list[i])
        blob_name = local_file_list[i]
        try:
            blob_service.put_block_blob_from_path(CONTAINER_NAME, blob_name, local_file)
        except:
            print "something wrong happened when uploading the data %s"%blob_name


## <a name="download-data-from-blob"></a>Stažení dat z objektů Blob

Stažení dat z objektů blob pomocí následujících metod:
1. získání\_objektů blob\_k\_cesta
2. získání\_objektů blob\_k\_souboru
3. získání\_objektů blob\_k\_bajtů
4. získání\_objektů blob\_k\_textu

Těchto postupů, které proveďte nezbytné bloků při velikost dat větší než 64 MB.

Následující ukázkový kód stáhne obsah objektů blob v kontejneru místní soubor:

    blob_service.get_blob_to_path("<your_container_name>", "<your_blob_name>", "<your_local_file_name>")

Následující ukázkový kód stáhne všechny objekty BLOB kontejner. Použití seznamu\_objektů BLOB získat seznam dostupných objektů BLOB v kontejneru a stáhne do místního adresáře.

    from azure.storage.blob import BlobService
    from os.path import join

    # Set parameters here
    ACCOUNT_NAME = "<your_account_name>"
    ACCOUNT_KEY = "<your_account_key>"
    CONTAINER_NAME = "<your_container_name>"
    LOCAL_DIRECT = "<your_local_directory>"     

    blob_service = BlobService(account_name=ACCOUNT_NAME, account_key=ACCOUNT_KEY)

    # List all blobs and download them one by one
    blobs = blob_service.list_blobs(CONTAINER_NAME)
    for blob in blobs:
        local_file = join(LOCAL_DIRECT, blob.name)
        try:
            blob_service.get_blob_to_path(CONTAINER_NAME, blob.name, local_file)
        except:
            print "something wrong happened when downloading the data %s"%blob.name
