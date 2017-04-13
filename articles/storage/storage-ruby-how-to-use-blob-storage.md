<properties
    pageTitle="Použití úložiště objektů Blob (objektu úložiště) z skutečné | Microsoft Azure"
    description="Obsahují Nestrukturovaná data v cloudu pomocí úložiště objektů Blob Azure (objektu úložiště)."
    services="storage"
    documentationCenter="ruby"
    authors="tamram"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="ruby"
    ms.topic="article"
    ms.date="08/11/2016"
    ms.author="tamram"/>


# <a name="how-to-use-blob-storage-from-ruby"></a>Použití úložiště objektů Blob z skutečné

[AZURE.INCLUDE [storage-selector-blob-include](../../includes/storage-selector-blob-include.md)]
<br/>
[AZURE.INCLUDE [storage-try-azure-tools-queues](../../includes/storage-try-azure-tools-blobs.md)]

## <a name="overview"></a>Základní informace

Úložiště objektů Blob Azure je služba, která ukládá jako objekty/objektů BLOB Nestrukturovaná data v cloudu. Úložiště objektů BLOB mohou být uloženy libovolné typu text nebo binární data, jako je dokument, soubor media nebo instalační program aplikace. Úložiště objektů blob se taky nazývá úložiště objektu.

Tento průvodce vám ukáže, jak provádět běžné scénáře pomocí úložiště objektů Blob. Vzorky jsou vytvořeny pomocí rozhraní API skutečné. Scénáře nichž se uplatní zahrnují **nahrávání, uvádějící, stahování** a **Odstranění** objektů BLOB.

[AZURE.INCLUDE [storage-blob-concepts-include](../../includes/storage-blob-concepts-include.md)]

[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="create-a-ruby-application"></a>Vytvoření skutečné aplikace

Vytvoření aplikace skutečné. Pokyny najdete v tématu [skutečné na profilů webová aplikace na OM Azure](../virtual-machines/linux/classic/virtual-machines-linux-classic-ruby-rails-web-app.md)

## <a name="configure-your-application-to-access-storage"></a>Konfigurace aplikace pro přístup k úložišti

Použít úložišti Azure, musíte stáhnout a používat skutečné azure balíček, který obsahuje sadu pohodlí knihoven, které komunikaci se službami ZBÝVAJÍCÍ úložiště.

### <a name="use-rubygems-to-obtain-the-package"></a>Použití RubyGems získání balíčku

1. Pomocí příkazového řádku rozhraní, jako je **prostředí PowerShell** (Windows), **terminálu** (Mac) nebo **flám** (Unix).

2. Do okna příkazového nainstalovat gem a závislosti zadejte "gem nainstalovat azure".

### <a name="import-the-package"></a>Import balíčku

V seznamu Oblíbené textovém editoru, přidejte následující text do horní části souboru skutečné místo, kam chcete použít úložiště:

    require "azure"

## <a name="setup-an-azure-storage-connection"></a>Nastavení připojení k Azure úložiště

Modul azure přečte proměnné prostředí **AZURE\_úložiště\_účtu** a **AZURE\_úložiště\_ACCESS_KEY** informace potřebné pro připojení ke svému účtu Azure úložiště. Pokud nejsou tyto proměnné, je nutné zadat informace o účtu před použitím **Azure::Blob::BlobService** s kódem takto:

    Azure.config.storage_account_name = "<your azure storage account>"
    Azure.config.storage_access_key = "<your azure storage access key>"


Postup získání tyto hodnoty z classic nebo Správce úložiště účet Azure portálu:

1. Přihlaste se k [portálu Azure](https://portal.azure.com).
2. Přejděte na úložiště účet, který chcete použít.
3. V nastavení zásuvné na pravé straně klikněte na **Přístupových kláves z verze**.
4. V zásuvné klíče přístup, která se zobrazí zobrazí se přístupová klávesa 1 a 2 přístupové klávesy. Můžete použít jednu z těchto. 
5. Klikněte na ikonu Kopírovat zkopírujte klávesu do schránky. 

Postup získání tyto hodnoty z účtu na klasické úložiště na klasické Azure portálu:

1. Přihlaste se k [klasické Azure portálu](https://manage.windowsazure.com).
2. Přejděte na úložiště účet, který chcete použít.
3. Klikněte na **Správa PŘÍSTUPOVÝCH kláves** v dolní části navigačního podokna.
4. V zobrazeném dialogovém okně zobrazí se název účtu úložiště, primárního přístupové klávesy a vedlejší přístupové klávesy. Pro přístupové klávesy můžete použít primární a sekundární tu. 
5. Klikněte na ikonu Kopírovat zkopírujte klávesu do schránky.

## <a name="create-a-container"></a>Vytvoření kontejneru

[AZURE.INCLUDE [storage-container-naming-rules-include](../../includes/storage-container-naming-rules-include.md)]

Objekt **Azure::Blob::BlobService** umožňuje práci s objekty BLOB a kontejnery. Chcete-li vytvořit kontejner, použijte **vytvořit\_container()** metody.

Následující příklad vytvoří kontejneru nebo vytiskněte chybu pokud je některý.

    azure_blob_service = Azure::Blob::BlobService.new
    begin
      container = azure_blob_service.create_container("test-container")
    rescue
      puts $!
    end

Pokud budete chtít zveřejnění soubory v kontejneru, můžete nastavit oprávnění kontejneru.

Můžete jednoduše změnit <strong>vytvořit\_container()</strong> volání předat **: veřejné\_přístup\_úroveň** možnost:

    container = azure_blob_service.create_container("test-container",
      :public_access_level => "<public access level>")


Platné hodnoty **: veřejné\_přístup\_úroveň** možnost jsou:

* **objektů blob:** Určuje důkladné veřejné přístup pro čtení pro kontejner a objektů blob data. Klienti můžete vytvořit výčet objektů BLOB v kontejneru prostřednictvím anonymní požadavku, ale nelze vytvořit výčet kontejnery v rámci účtu úložiště.

* **kontejner:** Určuje veřejné přístup pro čtení pro objekty BLOB. Objektů BLOB data v tomto kontejneru můžete číst prostřednictvím anonymní požadavku, ale kontejneru dat není k dispozici. Klienti nelze vytvořit výčet objektů BLOB v kontejneru prostřednictvím anonymní žádosti.

Můžete taky změnit úroveň přístupu veřejné kontejneru pomocí **nastavit\_kontejneru\_acl()** způsob, jak určit úroveň přístupu veřejnosti.

Následující příklad změny úroveň přístupu veřejné **kontejner**:

    azure_blob_service.set_container_acl('test-container', "container")

## <a name="upload-a-blob-into-a-container"></a>Nahrání objektů blob do kontejneru

Uložení obsahu do objektů blob, můžete **vytvořit\_blok\_blob()** způsob, jak vytvořit objektů blob, použít řetězce nebo souboru jako obsah objektů blob.

Následující kód odešle soubor **test.png** jako nový objektů blob s názvem "obrázek – objektů blob" v kontejneru.

    content = File.open("test.png", "rb") { |file| file.read }
    blob = azure_blob_service.create_block_blob(container.name,
      "image-blob", content)
    puts blob.name

## <a name="list-the-blobs-in-a-container"></a>Seznam objektů BLOB v kontejneru

Seznam kontejnerů, použijte metodu **list_containers()** .
Objekty BLOB uvnitř kontejneru, použijte **seznamu\_blobs()** metody.

To výstupy adresy URL všechny objekty BLOB ve všech kontejnerů pro účet.

    containers = azure_blob_service.list_containers()
    containers.each do |container|
      blobs = azure_blob_service.list_blobs(container.name)
      blobs.each do |blob|
        puts blob.name
      end
    end

## <a name="download-blobs"></a>Stáhněte si objekty BLOB

Stáhnout objektů BLOB, můžete **získat\_blob()** způsob, jak získat obsah.

Následující příklad ukazuje, jak pomocí **získat\_blob()** stáhnout obsah "obrázek objektů blob" a zapisovat do místní soubor.

    blob, content = azure_blob_service.get_blob(container.name,"image-blob")
    File.open("download.png","wb") {|f| f.write(content)}

## <a name="delete-a-blob"></a>Odstranění objektů Blob
Nakonec objektů blob, můžete odstranit **Odstranit\_blob()** metody. Následující příklad ukazuje, jak odstranit objektů blob.

    azure_blob_service.delete_blob(container.name, "image-blob")

## <a name="next-steps"></a>Další kroky

Další informace o složitější úlohy úložiště najdete na těchto odkazech:

- [Blog týmu Azure úložiště](http://blogs.msdn.com/b/windowsazurestorage/)
- [Azure SDK skutečné](https://github.com/WindowsAzure/azure-sdk-for-ruby) úložiště na GitHub
- [Přenos dat pomocí nástroje příkazového řádku AzCopy](storage-use-azcopy.md)
