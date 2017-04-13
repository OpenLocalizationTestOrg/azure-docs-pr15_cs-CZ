<properties
    pageTitle="Použití úložiště tabulek Azure z skutečné | Microsoft Azure"
    description="Obsahují strukturovaných dat v cloudu pomocí úložiště tabulek Azure, úložiště dat NoSQL."
    services="storage"
    documentationCenter="ruby"
    authors="tamram"
    manager="carmonm"
    editor=""/>
<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="ruby"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="tamram"/>


# <a name="how-to-use-azure-table-storage-from-ruby"></a>Použití úložiště tabulek Azure z skutečné

[AZURE.INCLUDE [storage-selector-table-include](../../includes/storage-selector-table-include.md)]
<br/>
[AZURE.INCLUDE [storage-try-azure-tools-queues](../../includes/storage-try-azure-tools-tables.md)]

## <a name="overview"></a>Základní informace

Tato příručka, uvidíte, jak provádět běžné scénáře pomocí služby Azure tabulka. Vzorky jsou vytvořeny pomocí rozhraní API skutečné. Scénáře nichž se uplatní zahrnují **vytváření a odstraňování tabulky, vložení a dotazování entity v tabulce**.

[AZURE.INCLUDE [storage-table-concepts-include](../../includes/storage-table-concepts-include.md)]

[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="create-a-ruby-application"></a>Vytvoření skutečné aplikace

Pokyny k vytvoření aplikace skutečné naleznete v tématu [skutečné na profilů webová aplikace na OM Azure](../virtual-machines/linux/classic/virtual-machines-linux-classic-ruby-rails-web-app.md).


## <a name="configure-your-application-to-access-storage"></a>Konfigurace aplikace pro přístup k úložišti

Použít Azure úložiště, budete muset stáhnout a použil funkci balíček skutečné azure, který obsahuje sadu knihovny snadno ovladatelné funkce, které komunikovat s ZBÝVAJÍCÍ úložiště služby.

### <a name="use-rubygems-to-obtain-the-package"></a>Použití RubyGems získání balíčku

1. Pomocí příkazového řádku rozhraní, jako je **prostředí PowerShell** (Windows), **terminálu** (Mac) nebo **flám** (Unix).

2. V okně příkazového nainstalovat gem a závislosti zadejte **gem instalace azure** .

### <a name="import-the-package"></a>Import balíčku

Použijte svůj oblíbený textovém editoru, přidejte následující text do horní části souboru skutečné místo, kam chcete použít úložiště:

    require "azure"

## <a name="set-up-an-azure-storage-connection"></a>Nastavení připojení k úložišti Azure

Modul azure přečte proměnné prostředí **AZURE\_úložiště\_účtu** a **AZURE\_úložiště\_přístup\_klíč** informace potřebné pro připojení ke svému účtu Azure úložiště. Pokud nejsou tyto proměnné, je nutné zadat informace o účtu před použitím **Azure::TableService** s kódem následující:

    Azure.config.storage_account_name = "<your azure storage account>"
    Azure.config.storage_access_key = "<your azure storage access key>"

Postup získání tyto hodnoty z classic nebo úložiště účtu správce na portálu Azure:

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

## <a name="create-a-table"></a>Vytvoření tabulky

Objekt **Azure::TableService** umožňuje práci s tabulkami a entity. Pokud chcete vytvořit tabulku, použijte **vytvořit\_table()** metody. Následující příklad vytvoří tabulku nebo vytiskněte chybu, pokud je některý.

    azure_table_service = Azure::TableService.new
    begin
      azure_table_service.create_table("testtable")
    rescue
      puts $!
    end

## <a name="add-an-entity-to-a-table"></a>Přidání entita do tabulky

Přidání entitu, nejprve vytvořte hash objekt, který definuje vlastnosti entity. Všimněte si, že každé entity je nutné zadat **PartitionKey** a **RowKey**. Toto jsou jedinečné identifikátory entity a hodnoty, které můžete vyhledávat mnohem rychleji než jiné vlastnosti. Azure úložiště používá **PartitionKey** rozdělí automaticky v tabulce entity mnoho úložiště uzlů. Entity s stejné **PartitionKey** jsou uložené ve stejném uzlu. **RowKey** je jedinečné ID entity v oddílu, který patří.

    entity = { "content" => "test entity",
      :PartitionKey => "test-partition-key", :RowKey => "1" }
    azure_table_service.insert_entity("testtable", entity)

## <a name="update-an-entity"></a>Aktualizace entita

Způsoby více dostupné aktualizace existující entitu:

* **Aktualizovat\_entity():** Aktualizujte existující entitu nahrazením.
* **korespondence\_entity():** Aktualizuje existující entitu sloučením nové nemovitostí s hodnotou do existující entitu.
* **Vložit\_nebo\_korespondence\_entity():** Aktualizuje existující entitu nahrazením. Pokud existuje žádná entita se vloží nový:
* **Vložit\_nebo\_nahradit\_entity():** Aktualizuje existující entitu sloučením nové nemovitostí s hodnotou do existující entitu. Pokud existuje žádná entita se vloží nový.

Následující příklad ukazuje aktualizace entity pomocí **Aktualizovat\_entity()**:

    entity = { "content" => "test entity with updated content",
      :PartitionKey => "test-partition-key", :RowKey => "1" }
    azure_table_service.update_entity("testtable", entity)

S **Aktualizovat\_entity()** a **korespondence\_entity()**, pokud entitu, kterou aktualizujete neexistuje pak operace aktualizace se nepovede. Proto pokud chcete ukládat entita bez ohledu na to, jestli už existuje, měli byste místo toho použít **Vložit\_nebo\_nahradit\_entity()** nebo **Vložit\_nebo\_korespondence\_entity()**.

## <a name="work-with-groups-of-entities"></a>Práce se skupinami entit

Někdy dává smysl můžou odeslat více operací pohromadě v jedné dávce zajistit atomová zpracování na serveru. Provádět, nejprve vytvořte **dávku** objektu a potom použijte **Spustit\_batch()** metoda na **TableService**. Následující příklad ukazuje odeslání dva typy entit s RowKey 2 a 3 v listu. Všimněte si, že funguje jenom pro entity s stejné PartitionKey.

    azure_table_service = Azure::TableService.new
    batch = Azure::Storage::Table::Batch.new("testtable",
      "test-partition-key") do
      insert "2", { "content" => "new content 2" }
      insert "3", { "content" => "new content 3" }
    end
    results = azure_table_service.execute_batch(batch)

## <a name="query-for-an-entity"></a>Dotaz entity

K vytvoření dotazu entita v tabulce, použít **získat\_entity()** metoda předáním název tabulky, **PartitionKey** a **RowKey**.

    result = azure_table_service.get_entity("testtable", "test-partition-key",
      "1")

## <a name="query-a-set-of-entities"></a>Dotaz sadu entity

K vytvoření dotazu sadu entity v tabulce, dotazu hash objekt vytvořit a používat **dotazu\_entities()** metody. V následujícím příkladu zobrazuje všechny entity s stejné **PartitionKey**:

    query = { :filter => "PartitionKey eq 'test-partition-key'" }
    result, token = azure_table_service.query_entities("testtable", query)

> [AZURE.NOTE] Pokud výsledek je příliš velký jednoho dotazu se vrátíte token pokračování budou vráceny sloužící k načtení další stránky.

## <a name="query-a-subset-of-entity-properties"></a>Dotaz podmnožinu entity vlastností

Dotaz do tabulky můžete načíst pár vlastnosti z entity. Tento postup s názvem "projekce" sníží šířka pásma a dosáhnout zvýšení výkonu dotazu, zejména u velkých entity. Použijte klauzuli select a předejte názvy vlastnosti, které chcete přenést klientovi.

    query = { :filter => "PartitionKey eq 'test-partition-key'",
      :select => ["content"] }
    result, token = azure_table_service.query_entities("testtable", query)

## <a name="delete-an-entity"></a>Odstranění entity

Můžete odstranit entita **Odstranit\_entity()** metody. Potřebujete předat název tabulky, která obsahuje entitu PartitionKey a RowKey entity.

        azure_table_service.delete_entity("testtable", "test-partition-key", "1")

## <a name="delete-a-table"></a>Odstranění tabulky

Odstranění tabulky, můžete **Odstranit\_table()** metoda a předejte název tabulky, který chcete odstranit.

        azure_table_service.delete_table("testtable")

## <a name="next-steps"></a>Další kroky

Další informace o složitější úlohy úložiště najdete na těchto odkazech:

- [Blog týmu Azure úložiště](http://blogs.msdn.com/b/windowsazurestorage/)
- [Azure SDK skutečné](http://github.com/WindowsAzure/azure-sdk-for-ruby) úložiště na GitHub
