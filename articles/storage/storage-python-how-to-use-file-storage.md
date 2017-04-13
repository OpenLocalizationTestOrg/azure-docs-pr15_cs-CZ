<properties
    pageTitle="Použití úložiště souborů Azure z Python | Microsoft Azure"
    description="Naučte se používat úložiště souborů Azure z Python nahrát, seznam, stažení a odstraňovat soubory."
    services="storage"
    documentationCenter="python"
    authors="robinsh"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="python"
    ms.topic="article"
    ms.date="09/20/2016"
    ms.author="robinsh"/>

# <a name="how-to-use-azure-file-storage-from-python"></a>Použití úložiště souborů Azure z Python

[AZURE.INCLUDE [storage-selector-file-include](../../includes/storage-selector-file-include.md)]
<br/>
[AZURE.INCLUDE [storage-try-azure-tools-files](../../includes/storage-try-azure-tools-files.md)]

## <a name="overview"></a>Základní informace

Tento článek vám ukáže, jak provádět běžné scénáře pomocí úložiště souborů. Vzorky psaných Python a používat [Microsoft Azure úložiště SDK Python]. Scénáře nichž se uplatní zahrnují nahrávání záznam, stahování a odstraňování souborů.

[AZURE.INCLUDE [storage-file-concepts-include](../../includes/storage-file-concepts-include.md)]

[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="create-a-share"></a>Vytvoření sdílené složky

Objekt **FileService** umožňuje pracovat s sdílených položek, adresářů a soubory. Následující kód vytvoří objekt **FileService** . Přidejte následující v horní části jakéhokoli Python souboru, ve kterém chcete programově přístup k úložišti Azure.

    from azure.storage.file import FileService

Následující kód vytvoří objekt **FileService** pomocí klíč účtu a název účtu úložiště.  Nahraďte "Můj účet" a "mykey" název účtu a klíče.

    file_service = **FileService** (account_name='myaccount', account_key='mykey')

Objekt **FileService** v následujícím příkladu slouží k vytvoření sdílet, pokud ho neexistuje.

    file_service.create_share('myshare')

## <a name="upload-a-file-into-a-share"></a>Nahrání souboru do sdílené složky

Azure sdílená úložiště obsahuje minimálně, kořenového adresáře, které mohou být umístěny soubory. V této části se dozvíte, jak nahrát soubor z místního úložiště na kořenovém adresáři sdílet.

Vytvoření souboru a nahrajte dat, můžete **vytvořit\_soubor\_z\_cestu**, **vytvořit\_soubor\_z\_toku**, **vytvořit\_soubor\_z\_bajtů** nebo **vytvořit\_soubor\_z\_text** metody. Budou způsoby nejvyšší úrovně, které proveďte nezbytné bloků při velikost dat větší než 64 MB.

**vytvoření\_soubor\_z\_cestu** odešle obsah souboru z určené cesty a **vytvořit\_soubor\_z\_toku** odešle obsah z už otevřeného souboru/proudu. **vytvoření\_soubor\_z\_bajtů** odešle maticových bajty, a **vytvořit\_soubor\_z\_text** odešle zadaný text hodnoty na základě zadaný (jako výchozí kódování UTF-8).

Následující příklad nahraje obsah souboru **sunset.png** do souboru **myfile** .

    from azure.storage.file import ContentSettings
    file_service.create_file_from_path(
        'myshare',
        None, # We want to create this blob in the root directory, so we specify None for the directory_name
        'myfile',
        'sunset.png',
        content_settings=ContentSettings(content_type='image/png'))

## <a name="how-to-create-a-directory"></a>Postup: vytvoření adresáře

Úložiště taky organizujete vložení souborů do podadresáře nechcete všem poznámkám v kořenovém adresáři. Služba úložiště Azure soubor umožňuje vytvořit tolik adresáře, které umožní váš účet. Následující kód vytvoří dílčí adresář s názvem **sampledir** v kořenovém adresáři.

    file_service.create_directory('myshare', 'sampledir')

## <a name="how-to-list-files-and-directories-in-a-share"></a>Postup: seznam souborů a adresářů ve sdílené složce

Seznam souborů a adresářů ve sdílené složce, můžete **seznamu\_adresářům\_a\_soubory** metody. Tento způsob vrátí generátor. Následující kód výstupy **název** jednotlivých souborů a adresářů ve sdílené složce v konzole.

    generator = file_service.list_directories_and_files('myshare')
    for file_or_dir in generator:
        print(file_or_dir.name)

## <a name="download-files"></a>Stahování souborů

Stažení dat ze souboru, použijte **získat\_soubor\_k\_cestu**, **získat\_soubor\_k\_toku**, **získat\_soubor\_k\_bajtů**, nebo **získat\_soubor\_k\_text**. Budou způsoby nejvyšší úrovně, které proveďte nezbytné bloků při velikost dat větší než 64 MB.

Následující příklad ukazuje, jak pomocí **získat\_soubor\_k\_cestu** ke stažení obsah souboru **myfile** a uložení souboru **mimo sunset.png** .

    file_service.get_file_to_path('myshare', None, 'myfile', 'out-sunset.png')

## <a name="delete-a-file"></a>Odstranění souboru

Nakonec při odstraňování souboru, zavolejte **delete_file**.

    file_service.delete_file('myshare', None, 'myfile')

## <a name="next-steps"></a>Další kroky

Teď, když jste se naučili základní informace o ukládání souborů, tyto odkazy vedou na další informace.

- [Středisko pro vývojáře Python](/develop/python/)
- [Služby Azure úložiště rozhraní REST API](http://msdn.microsoft.com/library/azure/dd179355)
- [Blog týmu Azure úložiště]
- [Microsoft Azure úložiště SDK Python]

[Blog týmu Azure úložiště]: http://blogs.msdn.com/b/windowsazurestorage/
[Microsoft Azure úložiště SDK Python]: https://github.com/Azure/azure-storage-python
