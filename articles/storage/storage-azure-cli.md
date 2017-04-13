<properties
    pageTitle="Azure rozhraní příkazového řádku službou Azure úložiště | Microsoft Azure"
    description="Naučte se používat rozhraní příkazového řádku Azure (Azure rozhraní příkazového řádku) s úložištěm Azure vytvářet a spravovat úložiště účty a práce s objekty BLOB Azure a soubory. Rozhraní příkazového řádku Azure je nástroj různé platformy "
    services="storage"
    documentationCenter="na"
    authors="micurd"
    manager="jahogg"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="micurd"/>

# <a name="using-the-azure-cli-with-azure-storage"></a>Azure rozhraní příkazového řádku službou Azure úložiště

## <a name="overview"></a>Základní informace

Rozhraní příkazového řádku Azure k dispozici sadu otevřít zdroj různé platformy příkazy pro práci s informacemi o platformě Azure. Poskytuje moc stejnou funkci najít v [Azure portál](https://portal.azure.com) , stejně jako funkce formátováním dat aplikace access.

V této příručce jsme budete zjistit, jak provádět různé vývoj a Správa úkolů s úložištěm Azure pomocí [rozhraní Azure příkazového řádku (Azure rozhraní příkazového řádku)](../xplat-cli-install.md) . Doporučujeme stáhnout a nainstalovat nebo upgradovat na nejnovější rozhraní příkazového řádku Azure před použitím tohoto průvodce.

Tato příručka předpokládá chápete základní koncepty Azure úložiště. Průvodce přináší celou řadu skripty, které demonstrují použití Azure rozhraní příkazového řádku s úložištěm Azure. Je potřeba aktualizovat proměnné skriptu na základě vaší konfigurace před spuštěním každý skript.

> [AZURE.NOTE] Průvodce jsou uvedeny příklady příkaz a skript Azure rozhraní příkazového řádku pro účty klasické úložiště. Najdete v tématu [použití Azure rozhraní příkazového řádku pro Mac a Linux, Windows s Azure řízení zdrojů](../virtual-machines/azure-cli-arm-commands.md#azure-storage-commands-to-manage-your-storage-objects) pro rozhraní příkazového řádku Azure příkazy pro účty úložiště správce prostředků.

## <a name="get-started-with-azure-storage-and-the-azure-cli-in-5-minutes"></a>Začínáme s Azure úložiště a Azure rozhraní příkazového řádku v 5 minut

Tato příručka, použije se systémem Ubuntu příklady, ale dalších platformách OS by měl provést podobně.

**Začínáte s Azure:** Pokud potřebujete předplatné Microsoft Azure a účet Microsoft spojený s, které předplatné. Informace o možnostech Azure nákupu najdete v článku [Bezplatnou zkušební verzi](https://azure.microsoft.com/pricing/free-trial/) [Možnostmi nákupu](https://azure.microsoft.com/pricing/purchase-options/)a [Nabízí člena](https://azure.microsoft.com/pricing/member-offers/) (u členů MSDN, Microsoft Partner Network a BizSpark a dalších aplikacích Microsoftu).

Další informace o předplatném najdete Azure naleznete v tématu [přiřazení rolí Správce služby Azure Active Directory (Azure AD)](https://msdn.microsoft.com/library/azure/hh531793.aspx) .

**Po vytvoření účtu a Microsoft Azure předplatného:**

1. Stáhněte a nainstalujte Azure rozhraní příkazového řádku, postupujte podle pokynů uvedených v [instalace Azure rozhraní příkazového řádku](../xplat-cli-install.md).
2. Po instalaci rozhraní příkazového řádku Azure budete moct příkazem azure z příkazového řádku rozhraní (flám, terminálu, příkazový) přístup k příkazům Azure rozhraní příkazového řádku. Typ `azure` příkaz a měli vidět následující výstup.

    ![Výstup Azure příkazu][Image1]

3. V rozhraní příkazového řádku zadejte `azure storage` seznam, všechny příkazy azure úložiště a získat první dojem funkcí Azure rozhraní příkazového řádku obsahuje. Můžete zadat název příkaz s parametrem **– h** (například `azure storage share create -h`) můžete zobrazit podrobnosti o syntaxe příkazu.
4. Teď můžeme se vám jednoduchý skript, který obsahuje základní příkazy Azure rozhraní příkazového řádku pro přístup k úložišti Azure. Skript nejdřív vás vyzve k nastavení dvě proměnné pro váš účet úložiště a klíče. Potom skript vytvoříte nové kontejneru do tohoto nového účtu úložiště a nahrát existující soubor obrázku (blob), které obsahují. Po skript uvádí všechny objekty BLOB v tomto kontejneru, stáhne soubor obrázku do cílového adresáře, které se nachází na místním počítači.

        #!/bin/bash
        # A simple Azure storage example

        export AZURE_STORAGE_ACCOUNT=<storage_account_name>
        export AZURE_STORAGE_ACCESS_KEY=<storage_account_key>

        export container_name=<container_name>
        export blob_name=<blob_name>
        export image_to_upload=<image_to_upload>
        export destination_folder=<destination_folder>

        echo "Creating the container..."
        azure storage container create $container_name

        echo "Uploading the image..."
        azure storage blob upload $image_to_upload $container_name $blob_name

        echo "Listing the blobs..."
        azure storage blob list $container_name

        echo "Downloading the image..."
        azure storage blob download $container_name $blob_name $destination_folder

        echo "Done"

5. V místním počítači otevřete upřednostňované textovém editoru (například vim). Zadejte výše skript, v textovém editoru.

6. Teď budete muset aktualizovat proměnné skriptu na základě nastavení konfigurace.

    - **< storage_account_name >** Použijte křestní jméno ve skriptu nebo zadejte nový název účtu úložiště. **Důležité:** Název účtu úložiště musí být jedinečný v Azure. Musí být malá písmena, příliš!

    - **< storage_account_key >** Přístupová klávesa účtu úložiště.

    - **< container_name >** Použijte křestní jméno ve skriptu nebo zadejte nový název pro váš kontejner.

    - **< image_to_upload >** Zadejte cestu k obrázku ve vašem počítači, například: "~ / images/HelloWorld.png".

    - **< destination_folder >** Zadejte cestu do místního adresáře ukládat soubory stažené ze Azure úložiště, jako je třeba: "~/downloadImages".

7. Po aktualizaci potřebné proměnných v vim stisknutím kombinace kláves "Esc,:, QW!" uložení skript.

8. Pokud chcete spustit tento skript, jednoduše zadejte název souboru skript v konzole flám. Po dokončení tohoto skriptu byste měli mít místní cílovou složku obsahující obrázek stažený soubor. Následující obrázek ukazuje příklad výstupu:

Po spuštění skript, byste měli mít místní cílovou složku obsahující soubor stažené obrázku.

## <a name="manage-storage-accounts-with-the-azure-cli"></a>Správa úložiště účtů s rozhraní příkazového řádku Azure

### <a name="connect-to-your-azure-subscription"></a>Připojení k předplatnému Azure

Většina příkazů úložiště bude práci bez předplatné Azure, doporučujeme, abyste se chcete připojit ke svému předplatnému z Azure rozhraní příkazového řádku. Abyste mohli nakonfigurovat Azure rozhraní příkazového řádku pro práci s vaším předplatným, postupujte podle pokynů v části [připojit ke Azure předplatné z Azure rozhraní příkazového řádku](../xplat-cli-connect.md).

### <a name="create-a-new-storage-account"></a>Vytvoření nového účtu úložiště

Použití Azure úložiště, budete potřebovat účet úložiště. Po dokončení konfigurace připojení k vašemu předplatnému, můžete vytvořit nový účet Azure úložiště.

        azure storage account create <account_name>

Název účtu úložiště musí být mezi 3 a 24 znaků a použijte čísla a malými písmeny.

### <a name="set-a-default-azure-storage-account-in-environment-variables"></a>Nastavení výchozí účet Azure úložiště v proměnné

Ve vašem předplatném může mít více účtů úložiště. Můžete zvolit jeden z nich a jeho nastavení v proměnné pro všechny příkazy úložiště ve stejné relaci. To umožňuje spuštění příkazu rozhraní příkazového řádku Azure úložiště bez zadání účtu úložiště a explicitně klíče.

        export AZURE_STORAGE_ACCOUNT=<account_name>
        export AZURE_STORAGE_ACCESS_KEY=<key>

Další způsob, jak nastavit výchozí úložiště účet používá připojovací řetězec. Za prvé získáte připojovací řetězec příkaz:

        azure storage account connectionstring show <account_name>

Potom zkopírujte připojovací řetězec výstup a nastavit proměnnou:

        export AZURE_STORAGE_CONNECTION_STRING=<connection_string>

## <a name="create-and-manage-blobs"></a>Vytváření a Správa objektů BLOB

Úložiště objektů Blob Azure je služba pro ukládání velkých objemů Nestrukturovaná data, například text nebo binární data, která je přístupný z kdekoli na světě prostřednictvím protokolu HTTP nebo HTTPS. Tato část se předpokládá, že znáte už koncepty úložiště objektů Blob Azure. Podrobné informace najdete v tématu [Začínáme s úložiště objektů Blob Azure pomocí .NET](storage-dotnet-how-to-use-blobs.md) a [Objektů Blob služby koncepty](http://msdn.microsoft.com/library/azure/dd179376.aspx).

### <a name="create-a-container"></a>Vytvoření kontejneru

Každý blob Azure úložiště musí být v kontejneru. Můžete vytvořit soukromé kontejneru pomocí `azure storage container create` příkaz:

        azure storage container create mycontainer

> [AZURE.NOTE] Existují tři úrovně anonymní přístup pro čtení: **vypnutí** **objektů Blob**a **kontejner**. Chcete-li zabránit anonymní přístup objektů BLOB, nastavte parametr oprávnění na **vypnout**. Ve výchozím nastavení nového kontejneru je soukromá a k nim získat přístup jenom vlastník účtu. Povolení anonymního přístupu ke čtení veřejné objektů blob zdroje, ale ne na kontejner metadata nebo do seznamu objektů BLOB v kontejneru nastavit parametr oprávnění **objektů Blob**. Umožňuje důkladné veřejné přístup pro čtení objektů blob zdrojů, kontejneru metadat a seznamu objektů BLOB v kontejneru nastavte parametr oprávnění na **kontejner**. Další informace najdete v tématu [Správa anonymní přístup pro čtení kontejnerů a objektů BLOB](storage-manage-access-to-resources.md).

### <a name="upload-a-blob-into-a-container"></a>Nahrání objektů blob do kontejneru

Úložiště objektů Blob Azure podporuje objektů BLOB bloku a objekty BLOB stránky. Další informace najdete v tématu [Principy blok objektů BLOB, přidat objektů BLOB a objekty BLOB stránky](http://msdn.microsoft.com/library/azure/ee691964.aspx).

K nahrání objektů BLOB v kontejneru, můžete použít `azure storage blob upload`. Ve výchozím nastavení tento příkaz, odešlou se místní soubory objektů blob blokovat. Pokud chcete zadat typ objektů blob, můžete použít `--blobtype` parametr.

        azure storage blob upload '~/images/HelloWorld.png' mycontainer myBlockBlob

### <a name="download-blobs-from-a-container"></a>Stáhněte si objektů blob z kontejneru

Následující příklad ukazuje, jak stáhnout objektů blob z kontejneru.

        azure storage blob download mycontainer myBlockBlob '~/downloadImages/downloaded.png'

### <a name="copy-blobs"></a>Kopírování objektů BLOB

Objekty BLOB v rámci nebo úložiště účty a oblastí můžete zkopírovat asynchronní.

Následující příklad ukazuje, jak zkopírovat z jednoho účtu úložiště objektů BLOB do druhého. V tomto příkladu jsme vytvořit kontejner, kde jsou veřejně, objekty BLOB anonymně přístupných osobám s postižením.

    azure storage container create mycontainer2 -a <accountName2> -k <accountKey2> -p Blob

    azure storage blob upload '~/Images/HelloWorld.png' mycontainer2 myBlockBlob2 -a <accountName2> -k <accountKey2>

    azure storage blob copy start 'https://<accountname2>.blob.core.windows.net/mycontainer2/myBlockBlob2' mycontainer

V tomto příkladu provede asynchronní kopírovat. Sledování stavu každý kopírování spuštěním `azure storage blob copy show` operace.

Všimněte si, že adresa URL zdroje k dispozici pro kopírování musí být veřejně přístupný nebo zahrnout token přidružení zabezpečení (sdílený přístup podpis).

### <a name="delete-a-blob"></a>Odstranění objektů blob

Můžete odstranit objektů blob pod příkaz:

        azure storage blob delete mycontainer myBlockBlob2

## <a name="create-and-manage-file-shares"></a>Vytváření a správa sdílené složky

Azure úložiště souborů nabízí sdílené úložiště pro aplikace, které používají protokol standardní SMB. Virtuálních počítačích Microsoft Azure a cloudové služby, stejně jako místní aplikací, můžete sdílet soubor data prostřednictvím připojené jejich počet. Můžete spravovat sdílené soubory a data souboru prostřednictvím rozhraní příkazového řádku Azure. Další informace o ukládání souborů Azure najdete v článku [Začínáme s úložiště souborů Azure v systému Windows](storage-dotnet-how-to-use-files.md) nebo [Použití úložiště souborů Azure s Linux](storage-how-to-use-files-linux.md).

### <a name="create-a-file-share"></a>Vytvoření sdílené složky

Sdílení souborů Azure je sdílení souborů SMB v Azure. Všechny soubory a adresáře musí být vytvořen ve sdílené složce. Účet může obsahovat neomezený počet sdílené položky a sdílené mohou být uloženy neomezený počet souborů, až limity kapacitu úložiště klienta. Následující příklad vytvoří sdílené složky s názvem **myshare**.

        azure storage share create myshare

### <a name="create-a-directory"></a>Vytvoření adresáře

Adresář poskytuje volitelné hierarchickou strukturu Azure sdílené složky. Následující příklad vytvoří adresář s názvem **adresář** ve sdílené složce.

        azure storage directory create myshare myDir

Poznámka: Tento adresář může obsahovat více úrovní, *například* **a b**. Musíte se ujistit, že existuje všech nadřazených adresářích. Například cesta **a b**, musí Tvorba adresář **** pak vytvořit adresář **b**.

### <a name="upload-a-local-file-to-directory"></a>Odeslat soubor místního adresáře

Následující příklad nahraje souboru z **~/temp/samplefile.txt** k adresáři **adresář** . Cesta k souboru upravte tak, aby ukazoval na platný soubor na místním počítači:

        azure storage file upload '~/temp/samplefile.txt' myshare myDir

Všimněte si, že soubor ve sdílené složce můžou mít velikost až 1 TB.

### <a name="list-the-files-in-the-share-root-or-directory"></a>Seznam souborů v kořenové sdílet nebo adresář

Můžete seznam souborů a podadresáře v kořenovém sdílet nebo adresáři pomocí následujícího příkazu:

        azure storage file list myshare myDir

Všimněte si, že název adresáře volitelné pro operaci seznam. Pokud není zadáno, seznamy příkazu obsah kořenovém adresáři sdílet.

### <a name="copy-files"></a>Kopírování souborů

Začínající 0.9.8 verzi Azure rozhraní příkazového řádku, můžete zkopírovat soubor do jiného souboru, souboru objektů blob nebo objektů blob do souboru. Tady vidíte jsme ukazují, jak provádět tyto operace Kopírovat pomocí příkazů rozhraní příkazového řádku. Kopírování souboru do nového adresáře:

    azure storage file copy start --source-share srcshare --source-path srcdir/hello.txt --dest-share destshare --dest-path destdir/hellocopy.txt --connection-string $srcConnectionString --dest-connection-string $destConnectionString

Kopírování objektů blob do adresáře souborů:

    azure storage file copy start --source-container srcctn --source-blob hello2.txt --dest-share hello --dest-path hellodir/hello2copy.txt --connection-string $srcConnectionString --dest-connection-string $destConnectionString

## <a name="next-steps"></a>Další kroky

Tady jsou některé související články a zdroje pro dozvědět další informace o úložišti Azure.

- [Si přečtěte následující dokumentaci Azure úložiště](https://azure.microsoft.com/documentation/services/storage/)
- [Azure úložiště REST API Reference](https://msdn.microsoft.com/library/azure/dd179355.aspx)


[Image1]: ./media/storage-azure-cli/azure_command.png
