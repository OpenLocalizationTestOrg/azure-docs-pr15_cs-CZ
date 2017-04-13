<properties
    pageTitle="Zkopírování nebo přesunutí dat do úložiště s AzCopy | Microsoft Azure"
    description="Přesunutí nebo zkopírování dat z objektů blob, tabulky a obsah souboru nebo použijte nástroj AzCopy. Kopírování dat k základnímu úložišti Azure z místní soubory nebo kopírování dat v rámci nebo mezi účty úložiště. Snadno přenést data na Azure úložiště."
    services="storage"
    documentationCenter=""
    authors="micurd"
    manager="jahogg"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/02/2016"
    ms.author="micurd"/>

# <a name="transfer-data-with-the-azcopy-command-line-utility"></a>Přenos dat pomocí nástroje příkazového řádku AzCopy

## <a name="overview"></a>Základní informace

AzCopy je nástroj příkazového řádku Windows navržené ke kopírování dat z úložiště objektů Blob Microsoft Azure, soubor a tabulky pomocí jednoduchých příkazů optimální výkon a. Data můžete zkopírovat z jednoho objektu do druhého v rámci svého účtu úložiště nebo mezi účty úložiště.

> [AZURE.NOTE] Tato příručka předpokládá, že znáte už [Azure úložiště](https://azure.microsoft.com/services/storage/). V opačném případě čtení dokumentace [Úvod k základnímu úložišti Azure](storage-introduction.md) bude užitečné. Co je nejdůležitější musíte vytvořit [účet úložiště](storage-create-storage-account.md#create-a-storage-account) abyste mohli začít používat AzCopy.

## <a name="download-and-install-azcopy"></a>Stažení a instalace AzCopy

### <a name="windows"></a>Windows

Stáhněte si [nejnovější verzi AzCopy](http://aka.ms/downloadazcopy).

### <a name="maclinux"></a>Mac a Linux

AzCopy není k dispozici pro Mac a Linux OSs. Rozhraní příkazového řádku Azure však je vhodné než ke kopírování dat z Azure úložiště a. Přečtěte si [pomocí rozhraní příkazového řádku Azure s úložištěm Azure](storage-azure-cli.md) Další informace.

## <a name="writing-your-first-azcopy-command"></a>Psaní prvního příkazu AzCopy

Základní syntaxe AzCopy příkazy je:

    AzCopy /Source:<source> /Dest:<destination> [Options]

Otevřete okno příkazového řádku a přejděte do adresáře instalace AzCopy v počítači – místo, kam `AzCopy.exe` spustitelný soubor nachází. Pokud budete chtít, můžete přidat umístění instalace AzCopy na cestu systému. Ve výchozím nastavení je nainstalovaný AzCopy `%ProgramFiles(x86)%\Microsoft SDKs\Azure\AzCopy` nebo `%ProgramFiles%\Microsoft SDKs\Azure\AzCopy`.

Následující příklady ukazují řadu možností pro zkopírování dat do a z objektů BLOB Microsoft Azure, soubory a tabulky. Přečtěte si část [AzCopy parametry](#azcopy-parameters) podrobný popis parametrů použitých v každém vzorku.

## <a name="blob-download"></a>Kulatý: stáhnout

### <a name="download-single-blob"></a>Stáhněte si jednoho objektů blob

    AzCopy /Source:https://myaccount.blob.core.windows.net/mycontainer /Dest:C:\myfolder /SourceKey:key /Pattern:"abc.txt"

Všimněte si, že pokud složku `C:\myfolder` neexistuje, vytvoříte ho a stáhněte si AzCopy `abc.txt ` do nové složky.

### <a name="download-single-blob-from-secondary-region"></a>Stáhněte si jednoho objektů blob z sekundární oblast

    AzCopy /Source:https://myaccount-secondary.blob.core.windows.net/mynewcontainer /Dest:C:\myfolder /SourceKey:key /Pattern:abc.txt

Všimněte si, že musíte mít přístup pro čtení geo nadbytečné úložiště povolené.

### <a name="download-all-blobs"></a>Stáhněte si všechny objekty BLOB

    AzCopy /Source:https://myaccount.blob.core.windows.net/mycontainer /Dest:C:\myfolder /SourceKey:key /S

Předpokládejme, že následující objekty BLOB nacházejí v zadaném kontejneru:  

    abc.txt
    abc1.txt
    abc2.txt
    vd1\a.txt
    vd1\abcd.txt

Po stažení operace v adresáři `C:\myfolder` bude obsahovat následující soubory:

    C:\myfolder\abc.txt
    C:\myfolder\abc1.txt
    C:\myfolder\abc2.txt
    C:\myfolder\vd1\a.txt
    C:\myfolder\vd1\abcd.txt

Pokud nezadáte možnost `/S`, bez objektů BLOB bude možné stáhnout.

### <a name="download-blobs-with-specified-prefix"></a>Stáhněte si objektů BLOB zadaný předponou

    AzCopy /Source:https://myaccount.blob.core.windows.net/mycontainer /Dest:C:\myfolder /SourceKey:key /Pattern:a /S

Předpokládejme, že následující objekty BLOB nacházejí v zadaném kontejneru. Všechny objekty BLOB začínající předponou `a` stáhnou:

    abc.txt
    abc1.txt
    abc2.txt
    xyz.txt
    vd1\a.txt
    vd1\abcd.txt

Po stažení operace složce `C:\myfolder` bude obsahovat následující soubory:

    C:\myfolder\abc.txt
    C:\myfolder\abc1.txt
    C:\myfolder\abc2.txt

Tuto předponu platí pro virtuální adresář, který tvoří první část názvu objektů blob. V předchozím příkladu virtuální adresář se neshoduje s předponou zadaný tak, aby se nestahuje. Kromě toho pokud možnost `\S` není zadán AzCopy nebude stahovat všechny objekty BLOB.

### <a name="set-the-last-modified-time-of-exported-files-to-be-same-as-the-source-blobs"></a>Nastavit čas poslední změny exportovaného souborů, které chcete být stejný jako objekty BLOB zdroje

    AzCopy /Source:https://myaccount.blob.core.windows.net/mycontainer /Dest:C:\myfolder /SourceKey:key /MT

Objekty BLOB můžete také vyloučit z operaci stahování podle jejich čas poslední změny. Pokud chcete, aby se vyloučila objektů BLOB čas jejichž poslední změny je například stejné nebo novější než cílového souboru přidat `/XN` možnost:

    AzCopy /Source:https://myaccount.blob.core.windows.net/mycontainer /Dest:C:\myfolder /SourceKey:key /MT /XN

Nebo pokud chcete, aby se vyloučila objektů BLOB čas jejichž poslední změny stejné nebo starší než cílového souboru přidat `/XO` možnost:

    AzCopy /Source:https://myaccount.blob.core.windows.net/mycontainer /Dest:C:\myfolder /SourceKey:key /MT /XO

## <a name="blob-upload"></a>Kulatý: odeslání

### <a name="upload-single-file"></a>Nahrání jedním souborem

    AzCopy /Source:C:\myfolder /Dest:https://myaccount.blob.core.windows.net/mycontainer /DestKey:key /Pattern:"abc.txt"

Pokud kontejneru určení neexistuje, AzCopy ji vytvořit a nahrajte soubor do ní.

### <a name="upload-single-file-to-virtual-directory"></a>Nahrání jednoho souboru do virtuálního adresáře

    AzCopy /Source:C:\myfolder /Dest:https://myaccount.blob.core.windows.net/mycontainer/vd /DestKey:key /Pattern:abc.txt

Pokud zadané virtuální adresář neexistuje, AzCopy bude nahrát soubor zahrnout virtuální adresář v názvu (*například* `vd/abc.txt` v předchozím příkladu).

### <a name="upload-all-files"></a>Odeslat všechny soubory.

    AzCopy /Source:C:\myfolder /Dest:https://myaccount.blob.core.windows.net/mycontainer /DestKey:key /S

Určení možnost `/S` zpětně úložiště objektů Blob, což znamená, že podsložky a soubory se nahraje i, odešlou se obsah zadaný adresář. Předpokládejme například nacházet následující soubory ve složce `C:\myfolder`:

    C:\myfolder\abc.txt
    C:\myfolder\abc1.txt
    C:\myfolder\abc2.txt
    C:\myfolder\subfolder\a.txt
    C:\myfolder\subfolder\abcd.txt

Po provedení operace Odeslat kontejneru zahrnuje následující soubory:

    abc.txt
    abc1.txt
    abc2.txt
    subfolder\a.txt
    subfolder\abcd.txt

Pokud nezadáte možnost `/S`, AzCopy nebude nahrát zpětně. Po provedení operace Odeslat kontejneru zahrnuje následující soubory:

    abc.txt
    abc1.txt
    abc2.txt

### <a name="upload-files-matching-specified-pattern"></a>Nahrání souborů odpovídající zadanému vzoru

    AzCopy /Source:C:\myfolder /Dest:https://myaccount.blob.core.windows.net/mycontainer /DestKey:key /Pattern:a* /S

Předpokládejme nacházet následující soubory ve složce `C:\myfolder`:

    C:\myfolder\abc.txt
    C:\myfolder\abc1.txt
    C:\myfolder\abc2.txt
    C:\myfolder\xyz.txt
    C:\myfolder\subfolder\a.txt
    C:\myfolder\subfolder\abcd.txt

Po provedení operace Odeslat kontejneru zahrnuje následující soubory:

    abc.txt
    abc1.txt
    abc2.txt
    subfolder\a.txt
    subfolder\abcd.txt

Pokud nezadáte možnost `/S`, AzCopy pouze nahraje objektů BLOB, které nejsou uložena v virtuální adresář:

    C:\myfolder\abc.txt
    C:\myfolder\abc1.txt
    C:\myfolder\abc2.txt

### <a name="specify-the-mime-content-type-of-a-destination-blob"></a>Zadejte MIME obsahu objektů blob cíl

Ve výchozím nastavení AzCopy nastaví typ obsahu cíl objektů blob k `application/octet-stream`. Začínající verze 3.1.0 explicitně určením typu obsahu prostřednictvím možnost `/SetContentType:[content-type]`. Tuto syntaxi sad typu obsahu pro všechny objekty BLOB v operaci nahrát.

    AzCopy /Source:C:\myfolder\ /Dest:https://myaccount.blob.core.windows.net/myContainer/ /DestKey:key /Pattern:ab /SetContentType:video/mp4

Pokud zadáte `/SetContentType` bez určité hodnotě, pak AzCopy nastaví každý objektů blob nebo typu obsahu souboru podle přípona souboru.

    AzCopy /Source:C:\myfolder\ /Dest:https://myaccount.blob.core.windows.net/myContainer/ /DestKey:key /Pattern:ab /SetContentType

## <a name="blob-copy"></a>Kulatý: kopie

### <a name="copy-single-blob-within-storage-account"></a>Kopírování jednoho objektů blob v rámci účtu úložiště

    AzCopy /Source:https://myaccount.blob.core.windows.net/mycontainer1 /Dest:https://myaccount.blob.core.windows.net/mycontainer2 /SourceKey:key /DestKey:key /Pattern:abc.txt

Při kopírování objektů blob v rámci účtu úložiště je provedena operace [serverovou kopii](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-asynchronous-cross-account-copy-blob.aspx) .

### <a name="copy-single-blob-across-storage-accounts"></a>Kopírování jednoho objektů blob mezi účty úložiště

    AzCopy /Source:https://sourceaccount.blob.core.windows.net/mycontainer1 /Dest:https://destaccount.blob.core.windows.net/mycontainer2 /SourceKey:key1 /DestKey:key2 /Pattern:abc.txt

Když zkopírujete objektů blob u účtů úložiště, je provedena operace [serverovou kopii](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-asynchronous-cross-account-copy-blob.aspx) .

### <a name="copy-single-blob-from-secondary-region-to-primary-region"></a>Kopírovat jednoho objektů blob z sekundární oblasti primární oblasti

    AzCopy /Source:https://myaccount1-secondary.blob.core.windows.net/mynewcontainer1 /Dest:https://myaccount2.blob.core.windows.net/mynewcontainer2 /SourceKey:key1 /DestKey:key2 /Pattern:abc.txt

Všimněte si, že musíte mít přístup pro čtení geo nadbytečné úložiště povolené.

### <a name="copy-single-blob-and-its-snapshots-across-storage-accounts"></a>Kopírování jednoho objektů blob a jeho snímků mezi účty úložiště

    AzCopy /Source:https://sourceaccount.blob.core.windows.net/mycontainer1 /Dest:https://destaccount.blob.core.windows.net/mycontainer2 /SourceKey:key1 /DestKey:key2 /Pattern:abc.txt /Snapshot

Po zkopírování bude obsahovat cílového kontejneru objektů blob a jeho snímků. Za předpokladu, že objektů blob v předchozím příkladu obsahuje dva snímky, kontejneru zahrne následující objektů blob a snímků:

    abc.txt
    abc (2013-02-25 080757).txt
    abc (2014-02-21 150331).txt

### <a name="synchronously-copy-blobs-across-storage-accounts"></a>Synchronní kopírovat mezi účty úložiště objektů BLOB

AzCopy ve výchozím nastavení slouží ke kopírování dat mezi dvěma body úložiště asynchronní. Zkopírování proto poběží v pozadí pomocí kapacitu náhradní šířky pásma, která má žádné SLA z hlediska jak rychle budou zkopírovány objektů blob a AzCopy bude pravidelně kontrolovat Kopírovat stav až kopírování po dokončení nebo se nezdařila.

`/SyncCopy` Možnost zaručuje, že zkopírování pošle konzistentní rychlost. AzCopy provede výtisku synchronní instituce objektů BLOB zkopírovat z zadaný zdroj do místní paměti a uložte je do cílového úložiště objektů Blob.

    AzCopy /Source:https://myaccount1.blob.core.windows.net/myContainer/ /Dest:https://myaccount2.blob.core.windows.net/myContainer/ /SourceKey:key1 /DestKey:key2 /Pattern:ab /SyncCopy

`/SyncCopy`mohou generovat další výstupní náklady ve srovnání s asynchronní kopírovat, doporučuje se tuto možnost použít v Azure OM, která je ve stejné oblasti jako účtu zdroj úložiště chcete-li předejít výstupní náklady.

## <a name="file-download"></a>Soubor: stáhnout

### <a name="download-single-file"></a>Stažení jedním souborem

    AzCopy /Source:https://myaccount.file.core.windows.net/myfileshare/myfolder1/ /Dest:C:\myfolder /SourceKey:key /Pattern:abc.txt

Pokud zadaný zdroj je Azure sdílené složky, a přesný název souboru, je třeba určit buď (*například* `abc.txt`) a stáhnout tvořená jedním souborem, vyberte možnost `/S` stahovat všechny soubory v zpětně sdílet. Pokoušející stanovit vzor souboru i možnost `/S` společně bude mít za následek chybu.

### <a name="download-all-files"></a>Stáhnout všechny soubory

    AzCopy /Source:https://myaccount.file.core.windows.net/myfileshare/ /Dest:C:\myfolder /SourceKey:key /S

Všimněte si, že nebude možné stáhnout prázdné složky.

## <a name="file-upload"></a>Soubor: odeslání

### <a name="upload-single-file"></a>Nahrání jedním souborem

    AzCopy /Source:C:\myfolder /Dest:https://myaccount.file.core.windows.net/myfileshare/ /DestKey:key /Pattern:abc.txt

### <a name="upload-all-files"></a>Odeslat všechny soubory.

    AzCopy /Source:C:\myfolder /Dest:https://myaccount.file.core.windows.net/myfileshare/ /DestKey:key /S

Všimněte si, že nebude možné odeslat prázdné složky.

### <a name="upload-files-matching-specified-pattern"></a>Nahrání souborů odpovídající zadanému vzoru

    AzCopy /Source:C:\myfolder /Dest:https://myaccount.file.core.windows.net/myfileshare/ /DestKey:key /Pattern:ab* /S

## <a name="file-copy"></a>Soubor: kopie

### <a name="copy-across-file-shares"></a>Kopírování mezi sdílených souborů

    AzCopy /Source:https://myaccount1.file.core.windows.net/myfileshare1/ /Dest:https://myaccount2.file.core.windows.net/myfileshare2/ /SourceKey:key1 /DestKey:key2 /S

### <a name="copy-from-file-share-to-blob"></a>Kopírování ze sdílené složky na objektů blob

    AzCopy /Source:https://myaccount1.file.core.windows.net/myfileshare/ /Dest:https://myaccount2.blob.core.windows.net/mycontainer/ /SourceKey:key1 /DestKey:key2 /S

Všimněte si, že není podporována asynchronní kopírování ze souboru úložiště objektů Blob stránky.

### <a name="copy-from-blob-to-file-share"></a>Kopírování z objektů blob do sdílené složky

    AzCopy /Source:https://myaccount1.blob.core.windows.net/mycontainer/ /Dest:https://myaccount2.file.core.windows.net/myfileshare/ /SourceKey:key1 /DestKey:key2 /S

### <a name="synchronously-copy-files"></a>Synchronní kopírování souborů

Můžete zadat `/SyncCopy` možnost ke kopírování dat z úložiště souborů do úložiště souborů, z úložiště souborů k úložišti objektů Blob a od úložiště objektů Blob do úložiště souborů synchronní, AzCopy dělá stažením zdrojová data do místní paměti a znovu ho nahrajte do cíle.

    AzCopy /Source:https://myaccount1.file.core.windows.net/myfileshare1/ /Dest:https://myaccount2.file.core.windows.net/myfileshare2/ /SourceKey:key1 /DestKey:key2 /S /SyncCopy

Když kopírujete z úložiště souborů k úložišti objektů Blob, výchozí typ objektů blob objektů blob blok je uživatel může zadat možnost `/BlobType:page` Změna typu objektů blob cíl.

Všimněte si, že `/SyncCopy` mohou generovat další výstupní nákladů porovnání asynchronní kopii, doporučuje se tuto možnost použít v OM Azure, což je ve stejné oblasti jako účtu zdroj úložiště chcete-li předejít výstupní náklady.

## <a name="table-export"></a>Tabulka: Export

### <a name="export-table"></a>Export tabulky

    AzCopy /Source:https://myaccount.table.core.windows.net/myTable/ /Dest:C:\myfolder\ /SourceKey:key

AzCopy zapíše do určené cílové složky souboru seznamu. Soubor je používán importu najít potřebné datové soubory a dělat ověření dat. Soubor je ve výchozím nastavení používá následující konvence:

    <account name>_<table name>_<timestamp>.manifest

Uživatel může zadat také možnost `/Manifest:<manifest file name>` nastavit název seznamu soubor.

    AzCopy /Source:https://myaccount.table.core.windows.net/myTable/ /Dest:C:\myfolder\ /SourceKey:key /Manifest:abc.manifest

### <a name="split-export-into-multiple-files"></a>Export rozděleného do víc souborů

    AzCopy /Source:https://myaccount.table.core.windows.net/mytable/ /Dest:C:\myfolder /SourceKey:key /S /SplitSize:100

AzCopy používá *Hlasitost indexu* v názvech souborů rozdělení data k rozlišení najednou několik souborů. Index hlasitost se skládá ze dvou částí, *index klíčové oblasti oddílu* a *rozdělení index souborů*. Obě indexy jsou od nuly.

Index klíčové oblasti oddílu bude 0, pokud uživatel zadat není možnost `/PKRS`.

Předpokládejme například, AzCopy vygeneruje dvou datových souborů, poté, co uživatel zadá možnost `/SplitSize`. Výsledné názvy souborů dat může být:

    myaccount_mytable_20140903T051850.8128447Z_0_0_C3040FE8.json
    myaccount_mytable_20140903T051850.8128447Z_0_1_0AB9AC20.json

Všimněte si, že minimální nejnižší hodnota parametru `/SplitSize` 32 MB. Pokud je Zadaná cílová úložiště objektů Blob, AzCopy rozdělení datového souboru po jeho velikosti dosáhne omezení velikosti objektů blob (200GB), bez ohledu na tom, jestli v možnostech `/SplitSize` byla specifikována uživatelem.

### <a name="export-table-to-json-or-csv-data-file-format"></a>Export tabulky do JSON nebo souboru CSV formátu souboru

AzCopy ve výchozím nastavení exportuje tabulek JSON datové soubory. Zadáte možnost `/PayloadFormat:JSON|CSV` Export tabulky jako JSON nebo souboru CSV.

    AzCopy /Source:https://myaccount.table.core.windows.net/myTable/ /Dest:C:\myfolder\ /SourceKey:key /PayloadFormat:CSV

Při určování datové formát CSV, AzCopy také generovat schématu soubor s příponou `.schema.csv` jednotlivých datových souborů.

### <a name="export-table-entities-concurrently"></a>Export tabulky entity současně

    AzCopy /Source:https://myaccount.table.core.windows.net/myTable/ /Dest:C:\myfolder\ /SourceKey:key /PKRS:"aa#bb"

AzCopy začnou souběžné operace exportu entity uživatel zadá možnost `/PKRS`. Každá operace exportuje jeden oddíl klíčové oblasti.

Všimněte si, že počet operací souběžně také řídil možnost `/NC`. AzCopy počet procesorů core používá jako výchozí hodnoty `/NC` při kopírování entity tabulky, i když `/NC` nebyla zadána. Pokud uživatel zadá možnost `/PKRS`, AzCopy používá menší z těchto dvou hodnot – klíčové oblasti oddílu a implicitně nebo výslovně zadaný souběžné operace - na určete počet souběžné operace začít. Další informace, zadejte `AzCopy /?:NC` příkazovém řádku.

### <a name="export-table-to-blob"></a>Export tabulky do objektů blob

    AzCopy /Source:https://myaccount.table.core.windows.net/myTable/ /Dest:https://myaccount.blob.core.windows.net/mycontainer/ /SourceKey:key1 /Destkey:key2

AzCopy vygeneruje JSON datového souboru do kontejneru objektů blob s po pojmenování:

    <account name>_<table name>_<timestamp>_<volume index>_<CRC>.json

Generované datový soubor JSON Formát datové části pro minimální metadata. Další informace v tomto formátu data najdete v článku [Formát datové části pro tabulku služba operace](http://msdn.microsoft.com/library/azure/dn535600.aspx).

Všimněte si, že při exportu tabulek do objektů BLOB, AzCopy bude stáhnout entity tabulky do místní dočasné datové soubory a pak nahrajte entit objektů blob. Tyto soubory dočasná data se budou zobrazovat do složky deníku soubor s výchozí cestu "<code>%LocalAppData%\Microsoft\Azure\AzCopy</code>", můžete nastavit možnost/Z: [složku Deník soubor] změnit deníku souboru umístění složky a tedy změnit umístění souborů dočasná data. Dočasné datové soubory velikost rozhodne tabulkové entity velikost a velikost, které jste zadali s /SplitSize možnost, i když dočasný datový soubor v místní disk se odstraní hned po nahraná na objekt blob, ujistěte se, že máte dost místem na disku pro ukládání tyto soubory dočasná data před sami odstranili.

## <a name="table-import"></a>Tabulka: Import

### <a name="import-table"></a>Import tabulky

    AzCopy /Source:C:\myfolder\ /Dest:https://myaccount.table.core.windows.net/mytable1/ /DestKey:key /Manifest:"myaccount_mytable_20140103T112020.manifest" /EntityOperation:InsertOrReplace

Možnost `/EntityOperation` označuje, jak vložit entity do tabulky. Možné hodnoty jsou:

- `InsertOrSkip`: Vynechá existující entitu nebo vloží novou entitu, pokud není k dispozici v tabulce.
- `InsertOrMerge`: Vloží novou entitu, pokud není k dispozici v tabulce nebo sloučí existující entity.
- `InsertOrReplace`: Nahradí existující entitu nebo vloží novou entitu, pokud není k dispozici v tabulce.

Poznámka: parametr nelze zadat `/PKRS` ve scénáři importovat. Na rozdíl od export scénáře, ve kterém je nutné zadat `/PKRS` souběžné operace zahájíte AzCopy ve výchozím nastavení začnou souběžné operace při importu tabulky. Výchozí počet souběžné operace zahájené je rovno počet procesorů základní; však můžete zadat rozdílný počet současně s možností `/NC`. Další informace, zadejte `AzCopy /?:NC` příkazovém řádku.

Všimněte si, že AzCopy podporuje pouze import pro JSON, ne CSV. AzCopy neobsahuje podporují importy tabulky z uživatel vytvořil JSON a manifest soubory. Oba tyto soubory musí pocházet ze export tabulky AzCopy. Aby nedocházelo k chybám, prosím neupravujte exportovaného JSON a soubor manifestu.

### <a name="import-entities-to-table-using-blobs"></a>Importovat entity do tabulky pomocí objekty BLOB

Předpokládejme kontejneru objektů Blob obsahuje následující: A JSON soubor představující tabulky Azure a jeho podklady seznamu.

    myaccount_mytable_20140103T112020.manifest
    myaccount_mytable_20140103T112020_0_0_0AF395F1DC42E952.json

Spuštěním následujícího příkazu Importovat entity do tabulky pomocí seznamu souboru v tomto kontejneru objektů blob:

    AzCopy /Source:https://myaccount.blob.core.windows.net/mycontainer /Dest:https://myaccount.table.core.windows.net/mytable /SourceKey:key1 /DestKey:key2 /Manifest:"myaccount_mytable_20140103T112020.manifest" /EntityOperation:"InsertOrReplace"

## <a name="other-azcopy-features"></a>Další funkce AzCopy

### <a name="only-copy-data-that-doesnt-exist-in-the-destination"></a>Kopírovat pouze data, která neexistuje v cílovém

`/XO` a `/XN` parametry umožňují vyloučení kopírovaný, respektive prostředků zdroj starší nebo novější. Pokud chcete kopírovat zdroj prostředky, které nejsou v cílovém, je možné zadat obou parametrů v příkazu AzCopy:

    /Source:http://myaccount.blob.core.windows.net/mycontainer /Dest:C:\myfolder /SourceKey:<sourcekey> /S /XO /XN

    /Source:C:\myfolder /Dest:http://myaccount.file.core.windows.net/myfileshare /DestKey:<destkey> /S /XO /XN

    /Source:http://myaccount.blob.core.windows.net/mycontainer /Dest:http://myaccount.blob.core.windows.net/mycontainer1 /SourceKey:<sourcekey> /DestKey:<destkey> /S /XO /XN

Poznámka: Toto není podporované, když zdrojového nebo cílového je tabulka.

### <a name="use-a-response-file-to-specify-command-line-parameters"></a>Zadejte parametry příkazového řádku pomocí souboru odpovědí

    AzCopy /@:"C:\responsefiles\copyoperation.txt"

Můžete zahrnout všechny parametry příkazového řádku AzCopy v souboru odpověď. AzCopy zpracuje parametry v souboru jako by měla byla specifikována do příkazového řádku provedení přímé nahrazení s obsahem souboru.

Předpokládejme soubor odpověď s názvem `copyoperation.txt`, který obsahuje následující řádky. Každý parametr AzCopy může být zadán na jednom řádku

    /Source:http://myaccount.blob.core.windows.net/mycontainer /Dest:C:\myfolder /SourceKey:<sourcekey> /S /Y

nebo na samostatné řádky:

    /Source:http://myaccount.blob.core.windows.net/mycontainer
    /Dest:C:\myfolder
    /SourceKey:<sourcekey>
    /S
    /Y

AzCopy selže, pokud rozdělíte parametr mezi dva řádky, jak ukazuje tato část pro `/sourcekey` parametr:

    http://myaccount.blob.core.windows.net/mycontainer
    C:\myfolder
    /sourcekey:
    <sourcekey>
    /S
    /Y

### <a name="use-multiple-response-files-to-specify-command-line-parameters"></a>Zadejte parametry příkazového řádku pomocí najednou několik souborů odpověď

Předpokládejme soubor odpověď s názvem `source.txt` určující kontejneru zdroje:

    /Source:http://myaccount.blob.core.windows.net/mycontainer

A soubor odpověď s názvem `dest.txt` číslo, které určuje cílovou složku v systému souborů:

    /Dest:C:\myfolder

A soubor odpověď s názvem `options.txt` určující možnosti pro AzCopy:

    /S /Y

Volání AzCopy s těmito odpověď soubory, které nacházet v adresáři `C:\responsefiles`, použijte tento příkaz:

    AzCopy /@:"C:\responsefiles\source.txt" /@:"C:\responsefiles\dest.txt" /SourceKey:<sourcekey> /@:"C:\responsefiles\options.txt"   

Stejně jako kdybyste zahrnuty všechny jednotlivé parametry příkazového řádku, zpracovává AzCopy tento příkaz:

    AzCopy /Source:http://myaccount.blob.core.windows.net/mycontainer /Dest:C:\myfolder /SourceKey:<sourcekey> /S /Y

### <a name="specify-a-shared-access-signature-sas"></a>Zadat podpis sdílený přístup (přidružení zabezpečení)

    AzCopy /Source:https://myaccount.blob.core.windows.net/mycontainer1 /Dest:https://myaccount.blob.core.windows.net/mycontainer2 /SourceSAS:SAS1 /DestSAS:SAS2 /Pattern:abc.txt

Můžete také zadat přidružení zabezpečení na kontejner URI:

    AzCopy /Source:https://myaccount.blob.core.windows.net/mycontainer1/?SourceSASToken /Dest:C:\myfolder /S

### <a name="journal-file-folder"></a>Soubor složku Deník

Pokaždé, když vydávat příkazu AzCopy, zjistí, zda souboru deníku existuje do výchozí složky nebo jestli existuje ve složce, kterou jste zadali prostřednictvím tuto možnost. Pokud soubor deníku neexistuje na jednom místě, AzCopy pokládá operace nové a vytvoří nový soubor deníku.

Pokud soubor deníku neexistuje, AzCopy zkontroluje, zda příkazového řádku, který je zadat odpovídá příkazového řádku v souboru deníku. Pokud tyto dvě čáry příkaz odpovídá, AzCopy obnoví neúplné operace. Pokud se neshodují, se výzva k buď přepsání deníku souboru můžete zahájit novou operaci nebo zrušit aktuální.

Pokud chcete použít výchozí umístění souboru deníku:

    AzCopy /Source:C:\myfolder /Dest:https://myaccount.blob.core.windows.net/mycontainer /DestKey:key /Z

Pokud nezadáte možnost `/Z`, nebo zadat na možnost `/Z` bez cestu ke složce uvedené výše, AzCopy vytvoří deník soubor ve výchozím umístění, což je `%SystemDrive%\Users\%username%\AppData\Local\Microsoft\Azure\AzCopy`. Pokud soubor deníku už existuje, AzCopy obnoví operaci na základě souboru deníku.

Pokud chcete zadat vlastní umístění souboru deníku:

    AzCopy /Source:C:\myfolder /Dest:https://myaccount.blob.core.windows.net/mycontainer /DestKey:key /Z:C:\journalfolder\

Tento příklad vytvoří soubor deníku, pokud už neexistuje. Pokud existují, AzCopy obnoví operaci na základě souboru deníku.

Pokud chcete pokračovat v operaci AzCopy:

    AzCopy /Z:C:\journalfolder\

V tomto příkladu obnoví poslední operaci, která se nezdařilo pravděpodobně dokončit.

### <a name="generate-a-log-file"></a>Vytvoření souboru protokolu

    AzCopy /Source:C:\myfolder /Dest:https://myaccount.blob.core.windows.net/mycontainer /DestKey:key /V

Pokud zadáte možnost `/V` bez zadání souborovou cestu pro úplné protokolování, AzCopy vytvoří soubor protokolu ve výchozím umístění, což je `%SystemDrive%\Users\%username%\AppData\Local\Microsoft\Azure\AzCopy`.

Jinak můžete vytvořit soubor protokolu ve vlastním umístění:

    AzCopy /Source:C:\myfolder /Dest:https://myaccount.blob.core.windows.net/mycontainer /DestKey:key /V:C:\myfolder\azcopy1.log

Poznámka: Pokud zadáte relativní cestu sledovat možnost `/V`, například `/V:test/azcopy1.log`, pak podrobný protokol vytvořený v adresáři aktuální pracovní v podsložce s názvem `test`.

### <a name="specify-the-number-of-concurrent-operations-to-start"></a>Určit počet souběžné operace spuštění

Možnost `/NC` určuje počet operací souběžné kopírovat. Ve výchozím nastavení se spustí AzCopy počet souběžné operací zvýšit výkon přenos dat. Pro operace s tabulkou je rovno počet procesorů, ke kterým máte počet souběžné operací. Pro objektů Blob soubor operací a počet souběžné operací je rovno 8 časy počet procesorů, ke kterým máte. Pokud používáte AzCopy v pomalé síti, můžete určit nižší číslo pro parametr /NC Chcete-li předejít chyba způsobená soutěže zdroje.

### <a name="run-azcopy-against-azure-storage-emulator"></a>Spustí AzCopy hledání emulátoru Azure úložiště

Spusťte AzCopy proti [Azure úložiště emulátoru](storage-use-emulator.md) pro objekty BLOB:

    AzCopy /Source:https://127.0.0.1:10000/myaccount/mycontainer/ /Dest:C:\myfolder /SourceKey:key /SourceType:Blob /S

a tabulky:

    AzCopy /Source:https://127.0.0.1:10002/myaccount/mytable/ /Dest:C:\myfolder /SourceKey:key /SourceType:Table

## <a name="azcopy-parameters"></a>AzCopy parametry

Parametry pro AzCopy jsou uvedeny níže. Můžete také zadat jeden z následujících příkazů z příkazového řádku pro pomoc při používání AzCopy:

- Nápovědu podrobné příkazového řádku pro AzCopy následujícím způsobem:`AzCopy /?`
- Podrobnou nápovědu k zadání parametru AzCopy:`AzCopy /?:SourceKey`
- Příklady příkazového řádku:`AzCopy /?:Samples`

### <a name="sourcesource"></a>/ Zdroje: "zdrojový"

Určuje zdrojová data ze které chcete zkopírovat. Zdroje můžou být adresář systému souborů, kontejneru objektů blob, objektů blob virtuální adresář, sdílené složky úložiště, adresář úložiště souborů nebo Azure table.

**k dispozici:** Objekty BLOB, soubory, tabulek

### <a name="destdestination"></a>/ Cíl: "cílový"

Cílová zkopírovat. Cíl může být adresář systému souborů, kontejneru objektů blob, objektů blob virtuální adresář, sdílené složky úložiště, adresář úložiště souborů nebo Azure table.

**k dispozici:** Objekty BLOB, soubory, tabulek

### <a name="patternfile-pattern"></a>/ Vzorek: "vzor souboru"

Určuje vzorek soubor, který označuje soubory, které chcete kopírovat. Chování /Pattern parametr, je určený podle umístění zdroje dat a sledování stavu možnost režimu rekurzivní. Rekurzivní režim není zadán prostřednictvím možnost parametrem/s.

Jestliže je zadané zdrojem v adresáři v systému souborů, pak standardního zástupné znaky jsou k dispozici v podstatě a soubor vzorek poskytnutý porovnány s souborů v rámci adresáře. Pokud možnost je zadán, pak AzCopy taky shodu zadaný u všech souborů v podsložek pod adresář.

Pokud zadané zdroje je kontejner objektů blob nebo virtuální adresář, nejsou použity zástupné znaky. Pokud možnost /S není zadán, pak AzCopy interpretovat vzorek zadaný soubor jako předponu objektů blob. Pokud možnost /S není zadán, pak AzCopy shodu soubor podle přesné objektů blob jmen.

Pokud zadaný zdroj je Azure sdílené složky a pak musíte zadat přesný název souboru, (například abc.txt) zkopírujte do jednoho souboru, nebo vyberte možnost /S zkopírujte všechny soubory v zpětně sdílet. Pokoušející stanovit vzor souboru i možnost /S společně způsobí chybu.

AzCopy používá velká a malá písmena shodu při / zdroj objektů blob kontejneru nebo objektů blob virtuální adresář a používá porovnávání ve všech ostatních případech.

Vzorec souboru výchozí používaný Pokud není zadán žádný vzor souboru je *.* pro umístění v systému souborů nebo prázdná předponu pro úložiště Azure. Zadání více vzorů soubor není podporovaná.

**k dispozici:** Objekty BLOB, soubory

### <a name="destkeystorage-key"></a>/ DestKey: "klíč úložiště"

Určuje klíč účtu úložiště pro cílové zdrojů.

**k dispozici:** Objekty BLOB, soubory, tabulek

### <a name="destsassas-token"></a>/ DestSAS: "přidružení zabezpečení token"

Určuje sdílených přístup podpisu (SAS) s oprávněními pro čtení a zápis pro cíle (když to jde). Jak může obsahuje speciální znaky příkazového řádku uzavírají přidružení s dvojitých uvozovkách.

Určení zdroje je kontejner objektů blob, sdílené složky nebo tabulku, můžete určit tuto možnost a za ním uveďte token přidružení zabezpečení, zda přidružení zabezpečení můžete zadat jako součást kontejneru objektů blob cíle, sdílené složky nebo identifikátor URI tabulky bez tuto možnost.

Pokud zdrojová i cílová jsou oba objekty BLOB, musí být objektů blob cíl umístěn v rámci jednoho účtu úložiště jako objektů blob zdroje.

**k dispozici:** Objekty BLOB, soubory, tabulek

### <a name="sourcekeystorage-key"></a>/ SourceKey: "klíč úložiště"

Určuje klíč účtu úložiště pro tento zdroj zdroj.

**k dispozici:** Objekty BLOB, soubory, tabulek

### <a name="sourcesassas-token"></a>/ SourceSAS: "přidružení zabezpečení token"

Určuje přístup podpis sdílené s oprávněními pro čtení a seznamu pro tento zdroj (když to jde). Jak může obsahuje speciální znaky příkazového řádku uzavírají přidružení s dvojitých uvozovkách.

Pokud je kontejner objektů blob zdroj zdroj a klíč ani přidružení zabezpečení je k dispozici, pak kontejneru objektů blob číst prostřednictvím anonymní přístup.

Jestliže je zdrojem sdílené složky nebo tabulce, musí být uvedeny klíč nebo přidružení zabezpečení.

**k dispozici:** Objekty BLOB, soubory, tabulek

### <a name="s"></a>/S

Určuje rekurzivní režim pro operace Kopírovat. V režimu rekurzivní AzCopy zkopírujete všechny objekty BLOB nebo soubory, které odpovídají zadanému souboru vzorku včetně podsložek.

**k dispozici:** Objekty BLOB, soubory

### <a name="blobtypeblock--page--append"></a>/ BlobType: "blok" | "stránka" | "připojit"

Určuje, zda objektů blob cíl blok objektů blob, objektů blob stránky nebo objektů blob připojit. Tato možnost je platný pouze v případě, že ukládáte objektů blob. V opačném se zobrazí chybová zpráva. Pokud je cílová objektů blob a tato možnost není zadán, ve výchozím nastavení vytvoří AzCopy objektů blob blok.

**k dispozici:** Objekty BLOB

### <a name="checkmd5"></a>/ CheckMD5

Vypočítá algoritmus hash MD5 stažený dat a ověří, že MD5 hash uložené v objektů blob nebo vlastnost MD5 obsah souboru odpovídá počítané hash. Je nutné zadat tuto možnost, chcete-li při stahování dat provést kontrolu MD5 zaškrtnutí MD5 je normálně vypnuté.

Všimněte si, že úložišti Azure nezaručuje, jestli máte aktuální hash MD5 uložené objektů blob nebo soubor. Je odpovědností klienta aktualizovat MD5 změně objektů blob nebo souboru je.

AzCopy vždy vlastnost MD5 obsahu pro aplikaci sady objektů blob Azure po odeslání služby.  

**k dispozici:** Objekty BLOB, soubory

### <a name="snapshot"></a>/ Snímku

Označuje, zda chcete přepojit snímky. Tato možnost je platný pouze po zdroji objektů blob.

Snímky přenesených objektů blob jsou přejmenované v tomto formátu: .extension názvů objektů blob (snímek času)

Ve výchozím nastavení se nezkopírují snímky.

**k dispozici:** Objekty BLOB

### <a name="vverbose-log-file"></a>/ V: [podrobného souboru protokolu]

Výstupy podrobného stavu zprávy do souboru protokolu.

Ve výchozím nastavení je podrobného souboru protokolu s názvem AzCopyVerbose.log v `%LocalAppData%\Microsoft\Azure\AzCopy`. Pokud zadáte existující umístění souboru pro tuto možnost, bude připojen úplné protokolování na tento soubor.  

**k dispozici:** Objekty BLOB, soubory, tabulek

### <a name="zjournal-file-folder"></a>/ Z: [složku Deník soubor]

Určí složku Deník souboru pro návrat k operaci.

AzCopy vždy podporuje obnovení Pokud byla přerušena operaci.

Pokud tato možnost není zadán nebo není zadán bez cestu ke složce, pak AzCopy vytvoří ve výchozím umístění, což je % LocalAppData%\Microsoft\Azure\AzCopy soubor deníku.

Pokaždé, když vydávat příkazu AzCopy, zjistí, zda souboru deníku existuje do výchozí složky nebo jestli existuje ve složce, kterou jste zadali prostřednictvím tuto možnost. Pokud soubor deníku neexistuje na jednom místě, AzCopy pokládá operace nové a vytvoří nový soubor deníku.

Pokud soubor deníku neexistuje, AzCopy zkontroluje, zda příkazového řádku, který je zadat odpovídá příkazového řádku v souboru deníku. Pokud tyto dvě čáry příkaz odpovídá, AzCopy obnoví neúplné operace. Pokud se neshodují, se výzva k buď přepsání deníku souboru můžete zahájit novou operaci nebo zrušit aktuální.

Po úspěšném operace odstranění souboru deníku.

Všimněte si, že obnovení operaci deníku soubor vytvořený pomocí předchozí verze AzCopy nepodporuje.

**k dispozici:** Objekty BLOB, soubory, tabulek

### <a name="parameter-file"></a>/@:"parameter-file"

Určuje soubor, který obsahuje parametry. AzCopy zpracuje parametry v souboru stejně, jako kdyby vám to byla specifikována do příkazového řádku.

V souboru odpovědí můžete zadat více parametrů v jednom řádku nebo na samostatném řádku zadat každý parametr. Všimněte si, že jednotlivé parametr nemůže zahrnovat více řádků.

Soubory odpovědí může obsahovat řádky komentáře, které začínají symbolem #.

Můžete zadat více souborů odpověď. Všimněte si, že AzCopy nepodporuje ale vnořené soubory odpovědí.

**k dispozici:** Objekty BLOB, soubory, tabulek

### <a name="y"></a>/ Y

Potlačí všechny výzvy potvrzení AzCopy.

**k dispozici:** Objekty BLOB, soubory, tabulek

### <a name="l"></a>L

Určuje operace seznam. žádná data zkopírována.

AzCopy budou interpretovat pomocí této možnosti simulace pro spuštění příkazového řádku bez možnost l a spočítat, kolik objekty budou zkopírovány, můžete nastavit možnost /V současně zkontrolovat objektů, které budou zkopírovány úplné protokolování.

Chování tato možnost určuje také umístění zdroje dat a sledování stavu rekurzivní režim možnost /S soubor vzorek možnosti /Pattern.

AzCopy vyžaduje oprávnění seznamu a ČÍST toto umístění zdroje při použití tuto možnost.

**k dispozici:** Objekty BLOB, soubory

### <a name="mt"></a>/MT

Nastaví stažený soubor datum poslední změny mohlo být stejné jako zdroj blob nebo souboru.

**k dispozici:** Objekty BLOB, soubory

### <a name="xn"></a>/XN

Vyloučí novější zdroj zdroj. Zdroje se nezkopírují Pokud časem poslední úpravy zdroje je stejný nebo vyšší než cíl.

**k dispozici:** Objekty BLOB, soubory

### <a name="xo"></a>/XO

Vyloučí starší zdroje zdroje. Zdroje se nezkopírují Pokud časem poslední úpravy zdroje je stejný nebo starší než cíl.

**k dispozici:** Objekty BLOB, soubory

### <a name="a"></a>/A

Odešle pouze soubory, které je atribut Archivovat nastavení.

**k dispozici:** Objekty BLOB, soubory

### <a name="iarashcnetoi"></a>/ I: [RASHCNETOI]

Odešle pouze soubory, které nemají množiny zadané atributy.

K dispozici atributy patří:

- R = soubory určené jen pro čtení
- A = soubory připravené k archivaci
- S = systémové soubory
- H = skryté soubory
- C = komprimovaná soubory
- N = normální soubory
- E = šifrovaný soubory
- T = dočasných souborů
- O = Offline souborů
- Můžu = indexované nejsou soubory

**k dispozici:** Objekty BLOB, soubory

### <a name="xarashcnetoi"></a>/ XA: [RASHCNETOI]

Vyloučí soubory, které mají jednu sadu zadané atributy.

K dispozici atributy patří:

- R = soubory určené jen pro čtení
- A = soubory připravené k archivaci
- S = systémové soubory
- H = skryté soubory
- C = komprimovaná soubory
- N = normální soubory
- E = šifrovaný soubory
- T = dočasných souborů
- O = Offline souborů
- Můžu = indexované nejsou soubory

**k dispozici:** Objekty BLOB, soubory

### <a name="delimiterdelimiter"></a>/ Oddělovače: "Oddělovače"

Určuje znak oddělovače, který vymezení virtuálních v názvech objektů blob.

Ve výchozím nastavení používá AzCopy / jako znak oddělovače. Však AzCopy podporuje pomocí běžné znak (například @, # nebo %) jako oddělovač. Pokud potřebujete zahrnout jeden ze speciálních znaků do příkazového řádku, uzavřete název souboru s dvojitých uvozovkách.

Tato možnost platí pouze pro její stažení objektů BLOB.

**k dispozici:** Objekty BLOB

### <a name="ncnumber-of-concurrent-operations"></a>/ NC: "číslo--souběžné operací"

Určuje počet souběžné operací.

AzCopy ve výchozím nastavení se spustí počet souběžné operací zvýšit výkon přenos data. Všimněte si, že velkého počtu souběžné operace v prostředí pomalé může overwhelm síťové připojení a zabránit operace plně dokončení. Omezení souběžné operace založené na skutečný dostupná šířka pásma sítě.

Horní mez pro souběžné operace je 512.

**k dispozici:** Objekty BLOB, soubory, tabulek

### <a name="sourcetypeblob--table"></a>/ SourceType: "Objektů Blob" | "Tabulky"

Určuje, že `source` blob k dispozici v místním vývojové prostředí aplikaci emulátoru úložiště je zdroj.

**k dispozici:** Objekty BLOB tabulek

### <a name="desttypeblob--table"></a>/ DestType: "Objektů Blob" | "Tabulky"

Určuje, že `destination` blob k dispozici v místním vývojové prostředí aplikaci emulátoru úložiště je zdroj.

**k dispozici:** Objekty BLOB tabulek

### <a name="pkrskey1key2key3"></a>/ PKRS: "klíč1 # klíč2 klíč3 #..."

Rozdělí klíčové oblasti oddílu povolit exportu dat tabulky současně, což zvyšuje rychlost operace exportu.

Pokud tato možnost není zadán, AzCopy používá podproces Export tabulky entity. Řekněme, že uživatel zadá /PKRS: "aa #bb" a potom AzCopy spustí tři souběžné operace.

Každá operace: Exportuje nějaká tři klíčové oblasti oddílu, jak je ukázáno v následujícím příkladu:

  [klíč první oddílu, aa)

  [aa, bb)

  [bb, klíč poslední oddílu]

**k dispozici:** Tabulky

### <a name="splitsizefile-size"></a>/ SplitSize: "velikost souboru"

Určuje exportovaný soubor rozdělení velikost MB, minimální hodnota povolená 32.

Pokud tato možnost není zadán, AzCopy exportu dat tabulky do jednoho souboru.

Pokud exportu dat tabulky na objekt blob a velikost exportovaného souboru dosáhne 200 GB limit velikosti objektů blob, pak AzCopy rozdělí exportovaný soubor, i když není zadaný tuto možnost.

**k dispozici:** Tabulky

### <a name="entityoperationinsertorskip--insertormerge--insertorreplace"></a>/ EntityOperation: "InsertOrSkip" | "InsertOrMerge" | "InsertOrReplace"

Určuje chování tabulky data importovat.

- InsertOrSkip - přeskočí existující entitu nebo vloží novou entitu, pokud není k dispozici v tabulce.

- InsertOrMerge - vloží novou entitu, pokud není k dispozici v tabulce nebo sloučí existující entity.

- InsertOrReplace - nahradí existující entitu nebo vloží novou entitu, pokud není k dispozici v tabulce.

**k dispozici:** Tabulky

### <a name="manifestmanifest-file"></a>/ Seznamu: "manifestu souboru"

Určuje soubor v tabulce exportu a importu.

Tato možnost je nepovinné během operace exportu, AzCopy vygeneruje seznamu soubor s má předdefinovaný název, pokud není zadán tuto možnost.

Tato možnost je potřeba během operace importu pro vyhledání datových souborů.

**k dispozici:** Tabulky

### <a name="synccopy"></a>/ SyncCopy

Označuje, zda synchronní kopírování objektů BLOB nebo souborů mezi dvěma body Azure úložiště.

AzCopy ve výchozím nastavení používá asynchronní serverovou kopii. Zadejte tuto možnost, chcete-li provést kopii synchronní, které odešlou je Azure úložiště a soubory ke stažení objektů BLOB nebo soubory do místní paměti.

Tuto možnost, můžete použít při kopírování souborů v úložišti objektů Blob, v rámci úložiště souborů nebo z úložiště objektů Blob úložiště souborů nebo naopak.

**k dispozici:** Objekty BLOB, soubory

### <a name="setcontenttypecontent-type"></a>/ SetContentType: "typ obsahu"

Určuje typ obsahu MIME objektů BLOB cíl nebo souborů.

AzCopy nastaví typu obsahu pro objektů blob nebo soubor aplikace/osmičkové stream ve výchozím nastavení. Typ obsahu pro všechny objekty BLOB nebo soubory můžete nastavit explicitně zadáním hodnoty pro tuto možnost.

Pokud zadáte tuto možnost bez hodnoty, AzCopy bude nastavení jednotlivých objektů blob nebo typu obsahu souboru podle přípona souboru.

**k dispozici:** Objekty BLOB, soubory

### <a name="payloadformatjson--csv"></a>/ PayloadFormat: "JSON" | "CSV"

Určuje formát souboru tabulky exportovaná data.

Pokud tato možnost není zadán,: Exportuje AzCopy ve výchozím nastavení soubor dat tabulky ve formátu JSON.

**k dispozici:** Tabulky

## <a name="known-issues-and-best-practices"></a>Známé problémy a doporučené postupy

### <a name="limit-concurrent-writes-while-copying-data"></a>Omezit souběžné zápisy při kopírování dat

Když zkopírujete objektů BLOB nebo soubory s AzCopy, mějte na paměti, že jiné aplikace mění data během kopírujete ho. Pokud je to možné Ujistěte se, že data, která kopírujete není upravuje během operace kopírování. Například při kopírování virtuálního pevného disku spojené s Azure virtuálního počítače, ujistěte se, že ostatní aplikace jsou aktuálně psaní virtuální pevný disk. Spolehlivých způsobů, jak to udělat je leasing zdroj zkopírována. Případně můžete nejdřív vytvořit snímek virtuálního pevného disku a zkopírujte snímek.

Pokud nemůžete zabránit jiných aplikací psaní objektů BLOB nebo soubory se kopírují, pak mějte na paměti, že v okamžiku dokončení projektu zkopírovaný zdroje už pravděpodobně úplné dostupná ke zdrojům zdroje.

### <a name="run-one-azcopy-instance-on-one-machine"></a>Spusťte jedna instance AzCopy na jednom počítači.
AzCopy slouží k maximalizace využití prostředek počítač urychlit přenos dat, doporučujeme spouštět jenom jedna instance AzCopy na jednom počítači a určete možnost `/NC` v případě potřeby více souběžné operace. Další informace, zadejte `AzCopy /?:NC` na příkazovém řádku.

### <a name="enable-fips-compliant-md5-algorithms-for-azcopy-when-you-use-fips-compliant-algorithms-for-encryption-hashing-and-signing"></a>Povolení vyhovující standardu MD5 FIPS AzCopy při můžete "použití FIPS algoritmů kompatibilních pro šifrování, algoritmus hash a podepisování".
AzCopy ve výchozím nastavení používá implementaci .NET MD5 pro výpočet MD5 při kopírování objektů, ale existují některé požadavky na zabezpečení, které potřebují AzCopy FIPS kompatibilní se standardem MD5 nastavení povolit.

Můžete vytvořit konfigurační soubor `AzCopy.exe.config` s vlastností `AzureStorageUseV1MD5` a umístí jej odložené s AzCopy.exe.

    <?xml version="1.0" encoding="utf-8" ?>
    <configuration>
      <appSettings>
        <add key="AzureStorageUseV1MD5" value="false"/>
      </appSettings>
    </configuration>

• Vlastnost "AzureStorageUseV1MD5" True - nebo rovná hodnotě AzCopy použije .NET MD5 implementaci.
• NEPRAVDA – AzCopy použije FIPS kompatibilní se standardem MD5 algoritmus.

Všimněte si, že je ve výchozím nastavení na počítači se systémem Windows zakázaná vyhovující standardu FIPS můžete v okně Spustit zadejte příkaz secpol.msc a zkontrolovat -> Tento přepínač nastavení zabezpečení místních zásad -> zabezpečení možnosti -> systém šifrování: použití vyhovující standardu FIPS pro šifrování, algoritmus hash a podepisování.

## <a name="next-steps"></a>Další kroky

Další informace o úložišti Azure a AzCopy naleznete v následujících zdrojích.

### <a name="azure-storage-documentation"></a>Azure úložiště si přečtěte následující dokumentaci:

- [Úvod k základnímu úložišti Azure](storage-introduction.md)
- [Použití úložiště objektů Blob z .NET](storage-dotnet-how-to-use-blobs.md)
- [Použití úložiště souborů z .NET](storage-dotnet-how-to-use-files.md)
- [Použití úložiště tabulek z .NET](storage-dotnet-how-to-use-tables.md)
- [Jak vytvářet, spravovat nebo odstraněním účtu úložiště](storage-create-storage-account.md)

### <a name="azure-storage-blog-posts"></a>Azure příspěvků na blogu úložiště:
- [Úvodní informace o náhled knihovny pohyb Azure úložiště dat](https://azure.microsoft.com/blog/introducing-azure-storage-data-movement-library-preview-2/)
- [AzCopy: Zavádění synchronní kopírovat a vlastní typ obsahu](http://blogs.msdn.com/b/windowsazurestorage/archive/2015/01/13/azcopy-introducing-synchronous-copy-and-customized-content-type.aspx)
- [AzCopy: Obecné dostupnost AzCopy 3.0 plus verzi preview AzCopy 4.0 s podporou tabulky a soubor oznámení dostupnosti aplikace](http://blogs.msdn.com/b/windowsazurestorage/archive/2014/10/29/azcopy-announcing-general-availability-of-azcopy-3-0-plus-preview-release-of-azcopy-4-0-with-table-and-file-support.aspx)
- [AzCopy: Optimalizované pro rozsáhlé kopírovat scénáře](http://go.microsoft.com/fwlink/?LinkId=507682)
- [AzCopy: Podpora geo nadbytečné úložiště přístup pro čtení](http://blogs.msdn.com/b/windowsazurestorage/archive/2014/04/07/azcopy-support-for-read-access-geo-redundant-account.aspx)
- [AzCopy: Data se znovu spustitelná režimu a přidružení zabezpečení token přenášet](http://blogs.msdn.com/b/windowsazurestorage/archive/2013/09/07/azcopy-transfer-data-with-re-startable-mode-and-sas-token.aspx)
- [AzCopy: Použití více účtů kopírovat Blob](http://blogs.msdn.com/b/windowsazurestorage/archive/2013/04/01/azcopy-using-cross-account-copy-blob.aspx)
- [AzCopy: Nahrávání nebo stahování souborů pro objekty BLOB Azure](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/12/03/azcopy-uploading-downloading-files-for-windows-azure-blobs.aspx)
