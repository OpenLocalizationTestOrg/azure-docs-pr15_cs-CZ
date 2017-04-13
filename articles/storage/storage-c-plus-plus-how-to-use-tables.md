<properties
    pageTitle="Použití úložiště tabulek (C++) | Microsoft Azure"
    description="Obsahují strukturovaných dat v cloudu pomocí úložiště tabulek Azure, úložiště dat NoSQL."
    services="storage"
    documentationCenter=".net"
    authors="dineshmurthy"
    manager="jahogg"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="dineshm"/>

# <a name="how-to-use-table-storage-from-c"></a>Použití úložiště tabulek z C++

[AZURE.INCLUDE [storage-selector-table-include](../../includes/storage-selector-table-include.md)]
<br/>
[AZURE.INCLUDE [storage-try-azure-tools-queues](../../includes/storage-try-azure-tools-tables.md)]

## <a name="overview"></a>Základní informace  
Tento průvodce vám ukáže, jak provádět běžné scénáře pomocí služba úložiště tabulek Azure. Vzorky napsané v C++ a používat [Azure úložiště klienta v knihovně C++](https://github.com/Azure/azure-storage-cpp/blob/master/README.md). Scénáře nichž se uplatní zahrnují **vytváření a odstraňování tabulky** a **pracovat s ním tabulkové entity**.

>[AZURE.NOTE] Tato příručka cílovou knihovnu Azure úložiště klienta C++ verze 1.0.0 a nad. Doporučené verze je úložiště klienta knihovna 2.2.0, která je dostupná přes [NuGet](http://www.nuget.org/packages/wastorage) nebo [GitHub](https://github.com/Azure/azure-storage-cpp/).

[AZURE.INCLUDE [storage-table-concepts-include](../../includes/storage-table-concepts-include.md)]
[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]


## <a name="create-a-c-application"></a>Vytvoření aplikace C++  
V této příručce použije funkce úložiště spuštěné aplikace C++. Chcete-li provést, musíte nainstalovat knihovnu Azure úložiště klienta pro C++ a vytvořit účet Azure úložiště v Azure předplatné.  

Instalace knihovnu Azure úložiště klienta C++, můžete pomocí následujících metod:

-   **Linux:** Postupujte podle pokynů na stránce [Azure úložiště klienta knihovny pro soubor Readme pro C++](https://github.com/Azure/azure-storage-cpp/blob/master/README.md) .  
-   **Windows:** Ve Visual Studiu, klikněte na **Nástroje > Správce balíčků NuGet > Správce balíčků konzoly**. Do [konzoly NuGet balíčku správce](http://docs.nuget.org/docs/start-here/using-the-package-manager-console) zadejte následující příkaz a stiskněte klávesu Enter.  

        Install-Package wastorage

## <a name="configure-your-application-to-access-table-storage"></a>Konfigurace aplikace pro přístup k úložiště tabulek  
Přidejte že následující příkazy do horní části C++ soubor, ve které chcete používat Azure úložiště rozhraní API pro přístup k tabulkám zahrnout:  

    #include "was/storage_account.h"
    #include "was/table.h"

## <a name="set-up-an-azure-storage-connection-string"></a>Nastavení připojovací řetězec Azure úložiště  
Azure úložiště klienta používá k ukládání koncové body a přihlašovací údaje pro přístup k dat správu přístupových připojovací řetězec úložiště. Při spuštění aplikace klienta, je nutné zadat připojovací řetězec úložiště v v tomto formátu. Použijte název účtu úložiště a přístupové klávesy úložiště úložiště účtu uvedený na [Portálu Azure](https://portal.azure.com) pro hodnoty *název účtu* a *AccountKey* . Informace o účtech úložiště a přístupové klávesy najdete v článku [o Azure úložiště účty](storage-create-storage-account.md). Tento příklad ukazuje, jak můžou deklarovat statické pole pro uložení připojovací řetězec:  

    // Define the connection string with your values.
    const utility::string_t storage_connection_string(U("DefaultEndpointsProtocol=https;AccountName=your_storage_account;AccountKey=your_storage_account_key"));

Chcete-li otestovat aplikace ve vašem počítači serveru s Windows, můžete Azure [emulátoru úložiště](storage-use-emulator.md) , které máte nainstalované s [Azure SDK](https://azure.microsoft.com/downloads/). Emulátoru úložiště je nástroj, který napodobuje služby objektů Blob Azure, fronty a tabulky ve vašem počítači místní rozvoj. Následující příklad ukazuje, jak můžou deklarovat statické pole pro uložení připojovací řetězec pro místní úložiště emulátoru:  

    // Define the connection string with Azure storage emulator.
    const utility::string_t storage_connection_string(U("UseDevelopmentStorage=true;"));  

Spuštění emulátoru Azure úložiště, klikněte na tlačítko **Start** a stiskněte klávesu Windows. Začněte psát **Azure úložiště emulátoru**a potom vyberte **Microsoft Azure úložiště emulátoru** ze seznamu aplikací.  

Následující příklady se předpokládá, že používáte jedním z následujících dvou způsobů jak získat připojovací řetězec úložiště.  

## <a name="retrieve-your-connection-string"></a>Získání připojovacího řetězce  
Třídy **cloud_storage_account** lze znázornit informací o účtu úložiště. Načtení informací o účtu úložiště z úložiště připojovací řetězec, můžete použít metodu analýzy.

    // Retrieve the storage account from the connection string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

Pak získáte odkaz na třída **cloud_table_client** jako umožňuje chybová odkazů na objekty pro tabulky a entity uložena v tabulku služba úložiště. Následující kód vytvoří objekt **cloud_table_client** pomocí objektu účtu úložiště obnovená nad:  

    // Create the table client.
    azure::storage::cloud_table_client table_client = storage_account.create_cloud_table_client();

## <a name="create-a-table"></a>Vytvoření tabulky
Objekt **cloud_table_client** slouží k získání odkazů na objekty pro tabulky a entity. Následující kód vytvoří objekt **cloud_table_client** a používá k vytvoření nové tabulky.

    // Retrieve the storage account from the connection string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);  

    // Create the table client.
    azure::storage::cloud_table_client table_client = storage_account.create_cloud_table_client();

    // Retrieve a reference to a table.
    azure::storage::cloud_table table = table_client.get_table_reference(U("people"));

    // Create the table if it doesn't exist.
    table.create_if_not_exists();  

## <a name="add-an-entity-to-a-table"></a>Přidání entita do tabulky
Přidání entita do tabulky, vytvoříte nový objekt **table_entity** a předat **table_operation::insert_entity**. Následující kód používá jméno zákazníka jako klíče řádku a příjmení jako klíč oddílu. Společně entity oddíl a klíče řádku jednoznačně identifikovat jednotku, v tabulce. Entity se stejným klíčem oddíl jde zpracovávat rychleji než s klávesami jiný oddíl, ale pomocí kláves se různorodého oddíl umožňuje větší škálovatelnost paralelní operace. Další informace najdete v tématu [Microsoft Azure úložiště výkon a škálovatelnost kontrolního seznamu](storage-performance-checklist.md).

Následující kód vytvoří nové instance **table_entity** s daty zákazníků uložený. Kód další hovory **table_operation::insert_entity** vytváření **table_operation** objektů k vložení entita do tabulky a přidruží nové tabulky entity. Nakonec kód volá metodu execute **cloud_table** objektu. A nové **table_operation** odešle žádost o službu tabulky pro vložení nového entitu Zákazník do tabulky "lidé".  

    // Retrieve the storage account from the connection string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

    // Create the table client.
    azure::storage::cloud_table_client table_client = storage_account.create_cloud_table_client();

    // Retrieve a reference to a table.
    azure::storage::cloud_table table = table_client.get_table_reference(U("people"));

    // Create the table if it doesn't exist.
    table.create_if_not_exists();

    // Create a new customer entity.
    azure::storage::table_entity customer1(U("Harp"), U("Walter"));

    azure::storage::table_entity::properties_type& properties = customer1.properties();
    properties.reserve(2);
    properties[U("Email")] = azure::storage::entity_property(U("Walter@contoso.com"));

    properties[U("Phone")] = azure::storage::entity_property(U("425-555-0101"));

    // Create the table operation that inserts the customer entity.
    azure::storage::table_operation insert_operation = azure::storage::table_operation::insert_entity(customer1);

    // Execute the insert operation.
    azure::storage::table_result insert_result = table.execute(insert_operation);

## <a name="insert-a-batch-of-entities"></a>Vložení listu entit
Vložení dávku entit tabulku Služba zápisu najednou. Následující kód vytvoří **table_batch_operation** objekt a potom je přidá tři vkládáte operace k němu. Každý operace vložení přidal vytvoření nového objektu entitu, nastavení své hodnoty a volání metody vložení na objekt **table_batch_operation** entitu přidružit Nová operace vložit. Potom **cloud_table.execute** je místo toho možnost spustit operaci.  

    // Retrieve the storage account from the connection string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

    // Create the table client.
    azure::storage::cloud_table_client table_client = storage_account.create_cloud_table_client();

    // Create a cloud table object for the table.
    azure::storage::cloud_table table = table_client.get_table_reference(U("people"));

    // Define a batch operation.
    azure::storage::table_batch_operation batch_operation;

    // Create a customer entity and add it to the table.
    azure::storage::table_entity customer1(U("Smith"), U("Jeff"));

    azure::storage::table_entity::properties_type& properties1 = customer1.properties();
    properties1.reserve(2);
    properties1[U("Email")] = azure::storage::entity_property(U("Jeff@contoso.com"));
    properties1[U("Phone")] = azure::storage::entity_property(U("425-555-0104"));

    // Create another customer entity and add it to the table.
    azure::storage::table_entity customer2(U("Smith"), U("Ben"));

    azure::storage::table_entity::properties_type& properties2 = customer2.properties();
    properties2.reserve(2);
    properties2[U("Email")] = azure::storage::entity_property(U("Ben@contoso.com"));
    properties2[U("Phone")] = azure::storage::entity_property(U("425-555-0102"));

    // Create a third customer entity to add to the table.
    azure::storage::table_entity customer3(U("Smith"), U("Denise"));

    azure::storage::table_entity::properties_type& properties3 = customer3.properties();
    properties3.reserve(2);
    properties3[U("Email")] = azure::storage::entity_property(U("Denise@contoso.com"));
    properties3[U("Phone")] = azure::storage::entity_property(U("425-555-0103"));

    // Add customer entities to the batch insert operation.
    batch_operation.insert_or_replace_entity(customer1);
    batch_operation.insert_or_replace_entity(customer2);
    batch_operation.insert_or_replace_entity(customer3);

    // Execute the batch operation.
    std::vector<azure::storage::table_result> results = table.execute_batch(batch_operation);

Některé věci, které je potřeba pamatovat na dávkové operace:  

-   Až 100 vložení, odstranění, hromadné korespondence, nahradit, vložení nebo sloučit a vložit nebo nahradit operace se dají dělat v libovolné kombinace v jedné dávce.  
-   Pokud je operaci jenom na listu, můžete mít operace dávku operace načíst.  
-   Všechny entity v operaci jedné dávce musí mít stejný klíč oddílu.  
-   Operace dávka se omezí na datová 4 MB.  

## <a name="retrieve-all-entities-in-a-partition"></a>Načtení všechny entity v oddílu
K vytvoření dotazu tabulku pro všechny entity v oddílu, použijte **table_query** objekt. Následující příklad určující filtr entit "Novák"-li klíč oddílu. V tomto příkladě se vytiskne pole jednotlivé entity ve výsledcích dotazu ke konzole.  

    // Retrieve the storage account from the connection string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

    // Create the table client.
    azure::storage::cloud_table_client table_client = storage_account.create_cloud_table_client();

    // Create a cloud table object for the table.
    azure::storage::cloud_table table = table_client.get_table_reference(U("people"));

    // Construct the query operation for all customer entities where PartitionKey="Smith".
    azure::storage::table_query query;

    query.set_filter_string(azure::storage::table_query::generate_filter_condition(U("PartitionKey"), azure::storage::query_comparison_operator::equal, U("Smith")));

    // Execute the query.
    azure::storage::table_query_iterator it = table.execute_query(query);

    // Print the fields for each customer.
    azure::storage::table_query_iterator end_of_results;
    for (; it != end_of_results; ++it)
    {
        const azure::storage::table_entity::properties_type& properties = it->properties();

        std::wcout << U("PartitionKey: ") << it->partition_key() << U(", RowKey: ") << it->row_key()
            << U(", Property1: ") << properties.at(U("Email")).string_value()
            << U(", Property2: ") << properties.at(U("Phone")).string_value() << std::endl;
    }  

Dotaz v tomto příkladu přináší všech entit, které splňují daná kritéria filtru. Pokud máte rozsáhlých tabulek a potřebujete ke stažení tabulkové entity často, doporučujeme, aby data uložena v Azure úložiště objektů BLOB místo.

## <a name="retrieve-a-range-of-entities-in-a-partition"></a>Načtení oblasti entit v oddílu
Pokud nechcete k vytvoření dotazu všechny entity v oddílu, můžete určit oblast kombinací filtr klíčových oddílu s filtrem klíčové řádku. Následující příklad využívá dva filtry zobrazíte všechny entity v oddílu "Novák", kde klávesu řádku (křestní jméno) starší než "E" abecedy začíná písmenem a následně tisk výsledků dotazu.  

    // Retrieve the storage account from the connection string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

    // Create the table client.
    azure::storage::cloud_table_client table_client = storage_account.create_cloud_table_client();

    // Create a cloud table object for the table.
    azure::storage::cloud_table table = table_client.get_table_reference(U("people"));

    // Create the table query.
    azure::storage::table_query query;

    query.set_filter_string(azure::storage::table_query::combine_filter_conditions(
        azure::storage::table_query::generate_filter_condition(U("PartitionKey"),
        azure::storage::query_comparison_operator::equal, U("Smith")),
        azure::storage::query_logical_operator::op_and,
        azure::storage::table_query::generate_filter_condition(U("RowKey"), azure::storage::query_comparison_operator::less_than, U("E"))));

    // Execute the query.
    azure::storage::table_query_iterator it = table.execute_query(query);

    // Loop through the results, displaying information about the entity.
    azure::storage::table_query_iterator end_of_results;
    for (; it != end_of_results; ++it)
    {
        const azure::storage::table_entity::properties_type& properties = it->properties();

        std::wcout << U("PartitionKey: ") << it->partition_key() << U(", RowKey: ") << it->row_key()
            << U(", Property1: ") << properties.at(U("Email")).string_value()
            << U(", Property2: ") << properties.at(U("Phone")).string_value() << std::endl;
    }  

## <a name="retrieve-a-single-entity"></a>Načtení jedné entity
Můžete vytvořit dotaz k načtení jedné konkrétní entita. Následující kód používá **table_operation::retrieve_entity** k určení zákazníka Petr Novák. Tento způsob vrátí jenom jedna osoba, nikoli kolekce a vrácená hodnota je v **table_result**. Určení oddíl a řádek v dotazu je nejrychlejší způsob, jak načíst jedna entita z tabulky služby.  

    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

    // Create the table client.
    azure::storage::cloud_table_client table_client = storage_account.create_cloud_table_client();

    // Create a cloud table object for the table.
    azure::storage::cloud_table table = table_client.get_table_reference(U("people"));

    // Retrieve the entity with partition key of "Smith" and row key of "Jeff".
    azure::storage::table_operation retrieve_operation = azure::storage::table_operation::retrieve_entity(U("Smith"), U("Jeff"));
    azure::storage::table_result retrieve_result = table.execute(retrieve_operation);

    // Output the entity.
    azure::storage::table_entity entity = retrieve_result.entity();
    const azure::storage::table_entity::properties_type& properties = entity.properties();

    std::wcout << U("PartitionKey: ") << entity.partition_key() << U(", RowKey: ") << entity.row_key()
        << U(", Property1: ") << properties.at(U("Email")).string_value()
        << U(", Property2: ") << properties.at(U("Phone")).string_value() << std::endl;

## <a name="replace-an-entity"></a>Nahrazení entita
Nahrazení entita načíst z tabulky služby, měnit objekt entita a ukládání změn zpátky na tabulku služba. Následující kód změní existující zákazníka telefonní číslo a e-mailovou adresu. Místo volání **table_operation::insert_entity**, používá tento kód **table_operation::replace_entity**. To způsobí, že entita plně nahrazuje na serveru, pokud entity na serveru změnila byl obnoven, v takovém případě se nezdaří. Tato chyba je nechcete, aby aplikace omylem přepsat změny mezi načítání a aktualizace jinou součástí aplikace. VELKÁ2 zpracování tato chyba je načíst znovu entitu, proveďte požadované změny (Pokud je pořád platná) a potom operaci jiné **table_operation::replace_entity** . Jak přepsat toto chování se zobrazí v další části.  

    // Retrieve the storage account from the connection string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

    // Create the table client.
    azure::storage::cloud_table_client table_client = storage_account.create_cloud_table_client();

    // Create a cloud table object for the table.
    azure::storage::cloud_table table = table_client.get_table_reference(U("people"));

    // Replace an entity.
    azure::storage::table_entity entity_to_replace(U("Smith"), U("Jeff"));
    azure::storage::table_entity::properties_type& properties_to_replace = entity_to_replace.properties();
    properties_to_replace.reserve(2);

    // Specify a new phone number.
    properties_to_replace[U("Phone")] = azure::storage::entity_property(U("425-555-0106"));

    // Specify a new email address.
    properties_to_replace[U("Email")] = azure::storage::entity_property(U("JeffS@contoso.com"));

    // Create an operation to replace the entity.
    azure::storage::table_operation replace_operation = azure::storage::table_operation::replace_entity(entity_to_replace);

    // Submit the operation to the Table service.
    azure::storage::table_result replace_result = table.execute(replace_operation);

## <a name="insert-or-replace-an-entity"></a>Vložení nebo nahrazování entita
operace **table_operation::replace_entity** se nezdaří došlo ke změně entitu od získána ze serveru. Kromě toho musí načtete entitu ze serveru nejdřív v pořadí **table_operation::replace_entity** byl úspěšný. Někdy ale nevíte Pokud subjekt existuje na serveru a aktuální hodnot uložených v něm jsou relevantní – aktualizace přepsat všechny. K tomu použijete operace **table_operation::insert_or_replace_entity** . Tuto operaci vloží entitu neexistuje nebo nahradí, pokud ano, bez ohledu na to, kdy byla vytvořena poslední aktualizace. V následujícím příkladu pořád načtené entitu Zákazník Petr Novák, ale pak se dokument uloží na server prostřednictvím **table_operation::insert_or_replace_entity**. Všechny aktualizace entity mezi načítání a operace aktualizace bude možné přepsat.  

    // Retrieve the storage account from the connection string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

    // Create the table client.
    azure::storage::cloud_table_client table_client = storage_account.create_cloud_table_client();

    // Create a cloud table object for the table.
    azure::storage::cloud_table table = table_client.get_table_reference(U("people"));

    // Insert-or-replace an entity.
    azure::storage::table_entity entity_to_insert_or_replace(U("Smith"), U("Jeff"));
    azure::storage::table_entity::properties_type& properties_to_insert_or_replace = entity_to_insert_or_replace.properties();

    properties_to_insert_or_replace.reserve(2);

    // Specify a phone number.
    properties_to_insert_or_replace[U("Phone")] = azure::storage::entity_property(U("425-555-0107"));

    // Specify an email address.
    properties_to_insert_or_replace[U("Email")] = azure::storage::entity_property(U("Jeffsm@contoso.com"));

    // Create an operation to insert-or-replace the entity.
    azure::storage::table_operation insert_or_replace_operation = azure::storage::table_operation::insert_or_replace_entity(entity_to_insert_or_replace);

    // Submit the operation to the Table service.
    azure::storage::table_result insert_or_replace_result = table.execute(insert_or_replace_operation);

## <a name="query-a-subset-of-entity-properties"></a>Dotaz podmnožinu entity vlastností  
Dotaz do tabulky můžete načíst pár vlastnosti z entity. Dotaz následující kód používá metodu **table_query::set_select_columns** k vrácení e-mailové adresy osob v tabulce.  

    // Retrieve the storage account from the connection string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

    // Create the table client.
    azure::storage::cloud_table_client table_client = storage_account.create_cloud_table_client();

    // Create a cloud table object for the table.
    azure::storage::cloud_table table = table_client.get_table_reference(U("people"));

    // Define the query, and select only the Email property.
    azure::storage::table_query query;
    std::vector<utility::string_t> columns;

    columns.push_back(U("Email"));
    query.set_select_columns(columns);

    // Execute the query.
    azure::storage::table_query_iterator it = table.execute_query(query);

    // Display the results.
    azure::storage::table_query_iterator end_of_results;
    for (; it != end_of_results; ++it)
    {
        std::wcout << U("PartitionKey: ") << it->partition_key() << U(", RowKey: ") << it->row_key();

        const azure::storage::table_entity::properties_type& properties = it->properties();
        for (auto prop_it = properties.begin(); prop_it != properties.end(); ++prop_it)
        {
            std::wcout << ", " << prop_it->first << ": " << prop_it->second.str();
        }

        std::wcout << std::endl;
    }

>[AZURE.NOTE] Dotaz na několik vlastnosti z entity je operace efektivnější než načtení všechny vlastnosti.

## <a name="delete-an-entity"></a>Odstranění entity
Po načtení ho můžete snadno odstranit entity. Jakmile načtené entitu volejte **table_operation::delete_entity** s entity odstranit. Zavolejte metodu **cloud_table.execute** . Následující kód vyhledá a odstraní entita s klíč oddílu "Miklus" a na řádku klávesy "Jan".  

    // Retrieve the storage account from the connection string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

    // Create the table client.
    azure::storage::cloud_table_client table_client = storage_account.create_cloud_table_client();

    // Create a cloud table object for the table.
    azure::storage::cloud_table table = table_client.get_table_reference(U("people"));

    // Create an operation to retrieve the entity with partition key of "Smith" and row key of "Jeff".
    azure::storage::table_operation retrieve_operation = azure::storage::table_operation::retrieve_entity(U("Smith"), U("Jeff"));
    azure::storage::table_result retrieve_result = table.execute(retrieve_operation);

    // Create an operation to delete the entity.
    azure::storage::table_operation delete_operation = azure::storage::table_operation::delete_entity(retrieve_result.entity());

    // Submit the delete operation to the Table service.
    azure::storage::table_result delete_result = table.execute(delete_operation);  

## <a name="delete-a-table"></a>Odstranění tabulky
Následující příklad nakonec odstraní tabulku z účtu úložiště. Tabulka, která byla Odstraněná přestane být dostupná za pro určité době po odstranění znovu vytvořit.  

    // Retrieve the storage account from the connection string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

    // Create the table client.
    azure::storage::cloud_table_client table_client = storage_account.create_cloud_table_client();

    // Create a cloud table object for the table.
    azure::storage::cloud_table table = table_client.get_table_reference(U("people"));

    // Create an operation to retrieve the entity with partition key of "Smith" and row key of "Jeff".
    azure::storage::table_operation retrieve_operation = azure::storage::table_operation::retrieve_entity(U("Smith"), U("Jeff"));
    azure::storage::table_result retrieve_result = table.execute(retrieve_operation);

    // Create an operation to delete the entity.
    azure::storage::table_operation delete_operation = azure::storage::table_operation::delete_entity(retrieve_result.entity());

    // Submit the delete operation to the Table service.
    azure::storage::table_result delete_result = table.execute(delete_operation);

## <a name="next-steps"></a>Další kroky
Teď, když jste se naučili základy úložiště tabulek, tyto odkazy vedou na další informace o úložišti Azure:  

-   [Použití úložiště objektů Blob z C++](storage-c-plus-plus-how-to-use-blobs.md)
-   [Použití úložiště fronty z C++](storage-c-plus-plus-how-to-use-queues.md)
-   [Seznam zdrojů Azure úložiště v C++](storage-c-plus-plus-enumeration.md)
-   [Knihovny úložiště klienta pro referenci C++](http://azure.github.io/azure-storage-cpp)
-   [Následující dokumentaci pro Azure úložiště](https://azure.microsoft.com/documentation/services/storage/)
