<properties
    pageTitle="Přesunutí dat do a z úložiště objektů Blob Azure pomocí AzCopy | Microsoft Azure"
    description="Přesunutí dat do a z úložiště objektů Blob Azure pomocí AzCopy"
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

# <a name="move-data-to-and-from-azure-blob-storage-using-azcopy"></a>Přesunutí dat do a z úložiště objektů Blob Azure pomocí AzCopy

AzCopy je určený pro odesílání, stahování a kopírování dat z objektů blob Microsoft Azure, soubor a úložiště tabulek a příkazového řádku nástroj.

Pokyny k instalaci AzCopy a další informace o použití s informacemi o platformě Azure přečtěte si článek [Začínáme s nástroje příkazového řádku AzCopy](../storage/storage-use-azcopy.md).

Pokyny pro technologií pro server slouží k přesunutí dat do a/nebo z úložiště objektů Blob Azure jsou propojené tady:

[AZURE.INCLUDE [blob-storage-tool-selector](../../includes/machine-learning-blob-storage-tool-selector.md)]


> [AZURE.NOTE] Pokud používáte OM, který byl nastaven pomocí skriptů poskytovanou [dat pro výzkum virtuálních počítačích v Azure](machine-learning-data-science-virtual-machines.md), pak AzCopy je už nainstalovaná na OM.

> [AZURE.NOTE] Pro dokončení Úvod k úložišti objektů blob Azure odkažte na [Základy objektů Blob Azure](../storage/storage-dotnet-how-to-use-blobs.md) a službě [objektů Blob Azure](https://msdn.microsoft.com/library/azure/dd179376.aspx).


## <a name="prerequisites"></a>Zjistit předpoklady pro

Tento dokument předpokládá, že máte předplatné Azure, účtu úložiště a odpovídající klíč úložiště k tomuto účtu. Před nahrávání nebo stahování dat, musíte znát kód název a účet Azure úložiště účtu.

- Nastavení Azure předplatného, najdete v článku [bezplatnou zkušební verzi jeden měsíc](https://azure.microsoft.com/pricing/free-trial/).

- Pokyny pro vytvoření účtu úložiště a usnadňující účet a klíčové informace podívejte se na [účty adresářové služby Azure o úložiště](../storage/storage-create-storage-account.md).


## <a name="run-azcopy-commands"></a>Spusťte AzCopy příkazy

Ke spuštění AzCopy příkazů, otevřete okno příkazového řádku a přejděte do adresáře instalace AzCopy ve vašem počítači, kde je uložena spustitelný AzCopy.exe. 

Základní syntaxe AzCopy příkazy je:

    AzCopy /Source:<source> /Dest:<destination> [Options]

>[AZURE.NOTE] Můžete přidat umístění instalace AzCopy na cestu systému a potom spustit příkazy z libovolného adresáře. Ve výchozím nastavení je nainstalovaný AzCopy *% ProgramFiles(x86) %\Microsoft SDKs\Azure\AzCopy* nebo *%ProgramFiles%\Microsoft SDKs\Azure\AzCopy*.

## <a name="upload-files-to-an-azure-blob"></a>Nahrání souborů na objektů blob Azure

Pokud chcete nahrát soubor, použijte tento příkaz:

    # Upload from local file system
    AzCopy /Source:<your_local_directory> /Dest: https://<your_account_name>.blob.core.windows.net/<your_container_name> /DestKey:<your_account_key> /S


## <a name="download-files-from-an-azure-blob"></a>Stahování souborů z objektů blob Azure

Pro stažení souboru z objektů blob Azure, použijte tento příkaz:

    # Downloading blobs to local file system
    AzCopy /Source:https://<your_account_name>.blob.core.windows.net/<your_container_name>/<your_sub_directory_at_blob>  /Dest:<your_local_directory> /SourceKey:<your_account_key> /Pattern:<file_pattern> /S


## <a name="transfer-blobs-between-azure-containers"></a>Přenos objektů BLOB mezi Azure kontejnery

Převést objektů BLOB mezi Azure kontejnerů, použijte tento příkaz:

    # Transferring blobs between Azure containers
    AzCopy /Source:https://<your_account_name1>.blob.core.windows.net/<your_container_name1>/<your_sub_directory_at_blob1> /Dest:https://<your_account_name2>.blob.core.windows.net/<your_container_name2>/<your_sub_directory_at_blob2> /SourceKey:<your_account_key1> /DestKey:<your_account_key2> /Pattern:<file_pattern> /S

    <your_account_name>: your storage account name
    <your_account_key>: your storage account key
    <your_container_name>: your container name
    <your_sub_directory_at_blob>: the sub directory in the container
    <your_local_directory>: directory of local file system where files to be uploaded from or the directory of local file system files to be downloaded to
    <file_pattern>: pattern of file names to be transferred. The standard wildcards are supported


## <a name="tips-for-using-azcopy"></a>Tipy pro použití AzCopy

> [AZURE.TIP]   
> 1. **Nahrávání** souborů */S* při odesílání souborů zpětně. Tento parametr nejsou nahrané soubory v podadresáře.  
> 2. Při **stahování** souboru */S* hledání zpětně kontejneru až do všech souborů v zadaném adresář a jeho podadresáře nebo všechny soubory, které odpovídají zadanému vzorku v dané adresář a jeho podadresáře, stahování.  
> 3.  Nelze zadat **konkrétní objektů blob soubor** , který chcete stáhnout pomocí *parametr/zdroj* . Pokud si Pokud chcete stáhnout konkrétním souborem, zadejte název souboru objektů blob ke stažení pomocí parametru */Pattern* . **/S** parametru lze mít AzCopy vyhledejte zpětně vzorek název souboru. Bez parametru vzorek AzCopy ke stažení pro všechny soubory v adresáři.
