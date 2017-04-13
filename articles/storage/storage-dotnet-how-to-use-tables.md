<properties
    pageTitle="Začínáme s úložiště tabulek Azure pomocí .NET | Microsoft Azure"
    description="Obsahují strukturovaných dat v cloudu pomocí úložiště tabulek Azure, úložiště dat NoSQL."
    services="storage"
    documentationCenter=".net"
    authors="tamram"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="hero-article"
    ms.date="10/18/2016"
    ms.author="tamram"/>


# <a name="get-started-with-azure-table-storage-using-net"></a>Začínáme s úložiště tabulek Azure pomocí .NET

[AZURE.INCLUDE [storage-selector-table-include](../../includes/storage-selector-table-include.md)]
<br/>
[AZURE.INCLUDE [storage-try-azure-tools-tables](../../includes/storage-try-azure-tools-tables.md)]

## <a name="overview"></a>Základní informace

Úložiště tabulek Azure je služba, která ukládá strukturovaná data NoSQL v cloudu. Úložiště tabulek je úložiště klíč/atribut schemaless návrh. Úložiště tabulek je schemaless, a proto není těžké si přizpůsobení dat jako potřeb evolve aplikace. Přístup k datům je rychlé a efektivní pro všechny typy aplikací. Úložiště tabulek je obvykle výrazně nižší náklady tradiční syntaxe jazyka SQL pro podobné objemy dat.

Úložiště tabulek můžete použít k ukládání flexibilní datové sady, například uživatelská data pro webové aplikace, adresáře, informace o zařízení a jiný typ metadata, která vyžaduje službu. V tabulce mohou být uloženy libovolné číslo entity a účet úložiště může obsahovat libovolný počet tabulek, až limit kapacitu úložiště klienta.

### <a name="about-this-tutorial"></a>O tomto kurzu

Tento kurz ukazuje, jak napsat kód .NET pro některé běžné scénáře pomocí úložiště tabulek Azure, včetně vytváření odstranění tabulky, vložení, aktualizace, odstranění a dotaz na data v tabulce.

**Předpokládaná doba dokončete:** 45 minut

**Prerequisities:**

- [Microsoft Visual Studio](https://www.visualstudio.com/en-us/visual-studio-homepage-vs.aspx)
- [Knihovna Azure úložiště klienta pro .NET](https://www.nuget.org/packages/WindowsAzure.Storage/)
- [Azure Správce konfigurace pro .NET](https://www.nuget.org/packages/Microsoft.WindowsAzure.ConfigurationManager/)
- [Účet Azure úložiště](storage-create-storage-account.md#create-a-storage-account)

[AZURE.INCLUDE [storage-dotnet-client-library-version-include](../../includes/storage-dotnet-client-library-version-include.md)]

### <a name="more-samples"></a>Další příklady

Další příklady použití úložiště tabulek najdete v článku [Začínáme s úložiště tabulek Azure v .NET](https://azure.microsoft.com/documentation/samples/storage-table-dotnet-getting-started/). Můžete stáhnout ukázková aplikace a spuštění nebo vyhledat kód na GitHub.


[AZURE.INCLUDE [storage-table-concepts-include](../../includes/storage-table-concepts-include.md)]

[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

[AZURE.INCLUDE [storage-development-environment-include](../../includes/storage-development-environment-include.md)]

### <a name="add-namespace-declarations"></a>Přidání deklarace oboru názvů

Přidejte následující `using` příkazů do horní části `program.cs` souboru:

    using Microsoft.Azure; // Namespace for CloudConfigurationManager
    using Microsoft.WindowsAzure.Storage; // Namespace for CloudStorageAccount
    using Microsoft.WindowsAzure.Storage.Table; // Namespace for Table storage types

### <a name="parse-the-connection-string"></a>Analyzovat připojovacího řetězce

[AZURE.INCLUDE [storage-cloud-configuration-manager-include](../../includes/storage-cloud-configuration-manager-include.md)]

### <a name="create-the-table-service-client"></a>Vytvořit tabulku služba klienta

**CloudTableClient** předmětu umožňuje načtení tabulky a entity uložené v úložišti tabulek. Tady je jedním ze způsobů vytvoření klienta služby:

    // Create the table client.
    CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

Teď jste připraveni zadat kód, který načítá data z a data zapisuje úložiště tabulek.

## <a name="create-a-table"></a>Vytvoření tabulky

Tento příklad ukazuje, jak vytvořit tabulku, pokud už neexistuje:

    // Retrieve the storage account from the connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the table client.
    CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

    // Retrieve a reference to the table.
    CloudTable table = tableClient.GetTableReference("people");

    // Create the table if it doesn't exist.
    table.CreateIfNotExists();

## <a name="add-an-entity-to-a-table"></a>Přidání entita do tabulky

Entity namapovat C\# objektů pomocí vlastní třídy odvozeno z **TableEntity**. Entita do tabulky přidáte vytvořte třídy, která definuje vlastnosti vaší entity. Následující kód definuje entity třídu, která používá jméno zákazníka jako klíče řádku a příjmení jako klíč oddílu. Společně entity oddíl a klíče řádku bylo možné jednoznačně identifikovat entitu v tabulce. Entity se stejným klíčem oddíl jde zpracovávat rychleji než s klávesami jiný oddíl, ale pomocí kláves se různorodého oddíl umožňuje větší škálovatelnost paralelní operace.  Některé z vlastností uložené ve službě tabulky, musí být vlastnost veřejnou vlastnost podporovaný typ, který poskytuje obě `get` a `set`.
Navíc vaše entity typ, *musíte* vystavit konstruktor bez parametrů.

    public class CustomerEntity : TableEntity
    {
        public CustomerEntity(string lastName, string firstName)
        {
            this.PartitionKey = lastName;
            this.RowKey = firstName;
        }

        public CustomerEntity() { }

        public string Email { get; set; }

        public string PhoneNumber { get; set; }
    }

Tabulky, které zahrnují entity provádějí pomocí **CloudTable** objekt, který jste vytvořili v části "Vytvořit tabulku". Provést operaci reprezentován **TableOperation** objektu.  Následující příklad ukazuje vytvoření **CloudTable** objektu a potom objekt **CustomerEntity** .  Příprava operaci, je vytvořen objekt **TableOperation** vložit entitu Zákazník do tabulky.  Nakonec na provedení operace tak, že zavoláte **CloudTable.Execute**.

    // Retrieve the storage account from the connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
       CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the table client.
    CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

    // Create the CloudTable object that represents the "people" table.
    CloudTable table = tableClient.GetTableReference("people");

    // Create a new customer entity.
    CustomerEntity customer1 = new CustomerEntity("Harp", "Walter");
    customer1.Email = "Walter@contoso.com";
    customer1.PhoneNumber = "425-555-0101";

    // Create the TableOperation object that inserts the customer entity.
    TableOperation insertOperation = TableOperation.Insert(customer1);

    // Execute the insert operation.
    table.Execute(insertOperation);

## <a name="insert-a-batch-of-entities"></a>Vložení listu entit

Vložení listu entit do tabulky v jednom kroku zápisu. Některé další poznámky na dávkové operace:

-  Provádění aktualizací, odstraní a vloží stejnou operaci jedné dávce.
-  Operace jedné dávce mohou obsahovat až 100 entity.
-  Všechny entity v operaci jedné dávce musí mít stejný klíč oddílu.
-  Když je možné provádět dotazu jako operace dávku, musí být operaci jenom na listu.

<!-- -->
Následující příklad vytvoří dva objekty entita a přidá na jednotlivých **TableBatchOperation** metodou **Vložit** . Potom **CloudTable.Execute** je místo toho možnost spustit operaci.

    // Retrieve the storage account from the connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the table client.
    CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

    // Create the CloudTable object that represents the "people" table.
    CloudTable table = tableClient.GetTableReference("people");

    // Create the batch operation.
    TableBatchOperation batchOperation = new TableBatchOperation();

    // Create a customer entity and add it to the table.
    CustomerEntity customer1 = new CustomerEntity("Smith", "Jeff");
    customer1.Email = "Jeff@contoso.com";
    customer1.PhoneNumber = "425-555-0104";

    // Create another customer entity and add it to the table.
    CustomerEntity customer2 = new CustomerEntity("Smith", "Ben");
    customer2.Email = "Ben@contoso.com";
    customer2.PhoneNumber = "425-555-0102";

    // Add both customer entities to the batch insert operation.
    batchOperation.Insert(customer1);
    batchOperation.Insert(customer2);

    // Execute the batch operation.
    table.ExecuteBatch(batchOperation);

## <a name="retrieve-all-entities-in-a-partition"></a>Načtení všechny entity v oddílu

K vytvoření dotazu tabulku pro všechny entity v oddílu, použijte **TableQuery** objekt.
Následující příklad určující filtr entit "Novák"-li klíč oddílu. V tomto příkladě se vytiskne pole jednotlivé entity ve výsledcích dotazu ke konzole.

    // Retrieve the storage account from the connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the table client.
    CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

    // Create the CloudTable object that represents the "people" table.
    CloudTable table = tableClient.GetTableReference("people");

    // Construct the query operation for all customer entities where PartitionKey="Smith".
    TableQuery<CustomerEntity> query = new TableQuery<CustomerEntity>().Where(TableQuery.GenerateFilterCondition("PartitionKey", QueryComparisons.Equal, "Smith"));

    // Print the fields for each customer.
    foreach (CustomerEntity entity in table.ExecuteQuery(query))
    {
        Console.WriteLine("{0}, {1}\t{2}\t{3}", entity.PartitionKey, entity.RowKey,
            entity.Email, entity.PhoneNumber);
    }

## <a name="retrieve-a-range-of-entities-in-a-partition"></a>Načtení oblasti entit v oddílu

Pokud nechcete k vytvoření dotazu všechny entity v oddílu, můžete určit oblast kombinací filtr klíčových oddílu s filtrem klíčové řádku. Následující příklad využívá dva filtry zobrazíte všechny entity v oddílu "Novák", kde klávesu řádku (křestní jméno) starší než "E" abecedy začíná písmenem a následně tisk výsledků dotazu.

    // Retrieve the storage account from the connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the table client.
    CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

    // Create the CloudTable object that represents the "people" table.
    CloudTable table = tableClient.GetTableReference("people");

    // Create the table query.
    TableQuery<CustomerEntity> rangeQuery = new TableQuery<CustomerEntity>().Where(
        TableQuery.CombineFilters(
            TableQuery.GenerateFilterCondition("PartitionKey", QueryComparisons.Equal, "Smith"),
            TableOperators.And,
            TableQuery.GenerateFilterCondition("RowKey", QueryComparisons.LessThan, "E")));

    // Loop through the results, displaying information about the entity.
    foreach (CustomerEntity entity in table.ExecuteQuery(rangeQuery))
    {
        Console.WriteLine("{0}, {1}\t{2}\t{3}", entity.PartitionKey, entity.RowKey,
            entity.Email, entity.PhoneNumber);
    }

## <a name="retrieve-a-single-entity"></a>Načtení jedné entity

Můžete vytvořit dotaz k načtení jedné konkrétní entita. Následující kód používá **TableOperation** k určení zákazníka Petr Novák.
Tento způsob vrátí jediným entita spíše než kolekce a vrácená hodnota v **TableResult.Result** **CustomerEntity** objektu.
Určení oddíl a řádek v dotazu je nejrychlejší způsob, jak načíst jedna entita z tabulky služby.

    // Retrieve the storage account from the connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the table client.
    CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

    // Create the CloudTable object that represents the "people" table.
    CloudTable table = tableClient.GetTableReference("people");

    // Create a retrieve operation that takes a customer entity.
    TableOperation retrieveOperation = TableOperation.Retrieve<CustomerEntity>("Smith", "Ben");

    // Execute the retrieve operation.
    TableResult retrievedResult = table.Execute(retrieveOperation);

    // Print the phone number of the result.
    if (retrievedResult.Result != null)
       Console.WriteLine(((CustomerEntity)retrievedResult.Result).PhoneNumber);
    else
       Console.WriteLine("The phone number could not be retrieved.");

## <a name="replace-an-entity"></a>Nahrazení entita

Aktualizace entita načíst z tabulky služby, měnit objekt entita a ukládání změn zpátky na tabulku služba. Následující kód změní existující zákazníka telefonní číslo. Místo připojí telefonicky, **Vložení**, používá tento kód **Nahradit**. To způsobí, že entita plně nahrazuje na serveru, pokud entity na serveru změnila byl obnoven, v takovém případě se nezdaří.  Tato chyba je nechcete, aby aplikace omylem přepsat změny mezi načítání a aktualizace jinou součástí aplikace.  VELKÁ2 zpracování tato chyba je načíst znovu entitu, proveďte požadované změny (Pokud je pořád platná) a potom operaci jiné **Nahradit** .  Jak přepsat toto chování se zobrazí v další části.

    // Retrieve the storage account from the connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the table client.
    CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

    // Create the CloudTable object that represents the "people" table.
    CloudTable table = tableClient.GetTableReference("people");

    // Create a retrieve operation that takes a customer entity.
    TableOperation retrieveOperation = TableOperation.Retrieve<CustomerEntity>("Smith", "Ben");

    // Execute the operation.
    TableResult retrievedResult = table.Execute(retrieveOperation);

    // Assign the result to a CustomerEntity object.
    CustomerEntity updateEntity = (CustomerEntity)retrievedResult.Result;

    if (updateEntity != null)
    {
       // Change the phone number.
       updateEntity.PhoneNumber = "425-555-0105";

       // Create the Replace TableOperation.
       TableOperation updateOperation = TableOperation.Replace(updateEntity);

       // Execute the operation.
       table.Execute(updateOperation);

       Console.WriteLine("Entity updated.");
    }

    else
       Console.WriteLine("Entity could not be retrieved.");

## <a name="insert-or-replace-an-entity"></a>Vložení nebo nahrazování entita

**Nahrazení** operace se nezdaří došlo ke změně entitu od získána ze serveru.  Kromě toho musí načtete entitu ze serveru nejdřív v operaci **Nahradit** byl úspěšný.
Někdy ale nevíte Pokud subjekt existuje na serveru a aktuální hodnot uložených v něm nejsou relevantní. Aktualizace přepsat všechny.  K tomu byste použili operaci **InsertOrReplace** .  Tuto operaci vloží entitu neexistuje nebo nahradí, pokud ano, bez ohledu na to, kdy byla vytvořena poslední aktualizace.  V následujícím příkladu entitu Zákazník Petr Novák pořád načíst, ale pak se dokument uloží na server prostřednictvím **InsertOrReplace**.  Všechny aktualizace entity mezi operacemi načítání a aktualizace bude možné přepsat.

    // Retrieve the storage account from the connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the table client.
    CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

    // Create the CloudTable object that represents the "people" table.
    CloudTable table = tableClient.GetTableReference("people");

    // Create a retrieve operation that takes a customer entity.
    TableOperation retrieveOperation = TableOperation.Retrieve<CustomerEntity>("Smith", "Ben");

    // Execute the operation.
    TableResult retrievedResult = table.Execute(retrieveOperation);

    // Assign the result to a CustomerEntity object.
    CustomerEntity updateEntity = (CustomerEntity)retrievedResult.Result;

    if (updateEntity != null)
    {
       // Change the phone number.
       updateEntity.PhoneNumber = "425-555-1234";

       // Create the InsertOrReplace TableOperation.
       TableOperation insertOrReplaceOperation = TableOperation.InsertOrReplace(updateEntity);

       // Execute the operation.
       table.Execute(insertOrReplaceOperation);

       Console.WriteLine("Entity was updated.");
    }

    else
       Console.WriteLine("Entity could not be retrieved.");

## <a name="query-a-subset-of-entity-properties"></a>Dotaz podmnožinu entity vlastností

Tabulka dotazu můžete načíst jen pomocí několika vlastnosti z entity místo všechny vlastnosti entity. Tento postup s názvem projekce, snižuje šířky pásma a dosáhnout zvýšení výkonu dotazu, zejména u velkých entity. Následující kód vracel e-mailové adresy osob v tabulce. Důvodem je pomocí dotazu **DynamicTableEntity** i **EntityResolver**. Je další informace o projekce na [úvodní informace o Upsert a projekce dotazu blogu příspěvek][]. Všimněte si, že projekce nepodporuje v místním úložišti PDC tak tento kód spuštěn pouze v případě, že používáte účet na tabulku služba.

    // Retrieve the storage account from the connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the table client.
    CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

    // Create the CloudTable that represents the "people" table.
    CloudTable table = tableClient.GetTableReference("people");

    // Define the query, and select only the Email property.
    TableQuery<DynamicTableEntity> projectionQuery = new TableQuery<DynamicTableEntity>().Select(new string[] { "Email" });

    // Define an entity resolver to work with the entity after retrieval.
    EntityResolver<string> resolver = (pk, rk, ts, props, etag) => props.ContainsKey("Email") ? props["Email"].StringValue : null;

    foreach (string projectedEmail in table.ExecuteQuery(projectionQuery, resolver, null, null))
    {
        Console.WriteLine(projectedEmail);
    }

## <a name="delete-an-entity"></a>Odstranění entity

Po načtení, pomocí stejného vzoru vidíte aktualizace entita můžete snadno odstranit entity.  Následující kód vyhledá a odstraní entitu Zákazník.

    // Retrieve the storage account from the connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the table client.
    CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

    // Create the CloudTable that represents the "people" table.
    CloudTable table = tableClient.GetTableReference("people");

    // Create a retrieve operation that expects a customer entity.
    TableOperation retrieveOperation = TableOperation.Retrieve<CustomerEntity>("Smith", "Ben");

    // Execute the operation.
    TableResult retrievedResult = table.Execute(retrieveOperation);

    // Assign the result to a CustomerEntity.
    CustomerEntity deleteEntity = (CustomerEntity)retrievedResult.Result;

    // Create the Delete TableOperation.
    if (deleteEntity != null)
    {
       TableOperation deleteOperation = TableOperation.Delete(deleteEntity);

       // Execute the operation.
       table.Execute(deleteOperation);

       Console.WriteLine("Entity deleted.");
    }

    else
       Console.WriteLine("Could not retrieve the entity.");

## <a name="delete-a-table"></a>Odstranění tabulky

Následující příklad nakonec odstraní tabulku z účtu úložiště. Tabulka, která byla Odstraněná přestane být dostupná za pro určité době po odstranění znovu vytvořit.

    // Retrieve the storage account from the connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the table client.
    CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

    // Create the CloudTable that represents the "people" table.
    CloudTable table = tableClient.GetTableReference("people");

    // Delete the table it if exists.
    table.DeleteIfExists();

## <a name="retrieve-entities-in-pages-asynchronously"></a>Asynchronní načíst entity na stránkách

Pokud čtete velkého počtu entity, kde chcete proces/zobrazení entity jako načtením než čekání pro všechny vrátit, můžete načtete entity pomocí Segmentovaný dotazu. Tento příklad ukazuje, jak se na stránkách pomocí očekávat asynchronní vzorek tak, aby spuštění blokován během čekání pro velké množství výsledků vrátit. Podrobné informace o použití vzorku asynchronní Await v .NET najdete v článku [asynchronní programování s asynchronní a Await (C# a Visual Basic)](https://msdn.microsoft.com/library/hh191443.aspx).

    // Initialize a default TableQuery to retrieve all the entities in the table.
    TableQuery<CustomerEntity> tableQuery = new TableQuery<CustomerEntity>();

    // Initialize the continuation token to null to start from the beginning of the table.
    TableContinuationToken continuationToken = null;

    do
    {
        // Retrieve a segment (up to 1,000 entities).
        TableQuerySegment<CustomerEntity> tableQueryResult =
            await table.ExecuteQuerySegmentedAsync(tableQuery, continuationToken);

        // Assign the new continuation token to tell the service where to
        // continue on the next iteration (or null if it has reached the end).
        continuationToken = tableQueryResult.ContinuationToken;

        // Print the number of rows retrieved.
        Console.WriteLine("Rows retrieved {0}", tableQueryResult.Results.Count);

    // Loop until a null continuation token is received, indicating the end of the table.
    } while(continuationToken != null);

## <a name="next-steps"></a>Další kroky

Teď, když jste se naučili základy úložiště tabulek, tyto odkazy vedou na další informace o složitější úlohy úložiště:

- Zobrazit další úložiště ukázky tabulky při [Zahájení používání úložiště tabulek Azure v .NET](https://azure.microsoft.com/documentation/samples/storage-table-dotnet-getting-started/)
- Zobrazte tabulku služba dokumentaci úplné podrobnosti o dostupných rozhraní API:
    - [Knihovna úložiště klienta pro referenci .NET](http://go.microsoft.com/fwlink/?LinkID=390731&clcid=0x409)
    - [Odkaz rozhraní REST API](http://msdn.microsoft.com/library/azure/dd179355)
- Zjistěte, jak zjednodušit kód, který napíšete pro práci s úložištěm Azure pomocí [Azure WebJobs SDK](../app-service-web/websites-dotnet-webjobs-sdk-get-started.md)
- Zobrazte další funkce příručky pro další informace o další možnosti pro ukládání dat v Azure.
    - [Začínáme s úložiště objektů Blob Azure pomocí .NET](storage-dotnet-how-to-use-blobs.md) pro ukládání Nestrukturovaná data.
    - [Připojení k databázi SQL pomocí .NET (C#)](../sql-database/sql-database-develop-dotnet-simple.md) pro ukládání relačních dat.

  [Download and install the Azure SDK for .NET]: /develop/net/
  [Creating an Azure Project in Visual Studio]: http://msdn.microsoft.com/library/azure/ee405487.aspx

  [Blob5]: ./media/storage-dotnet-how-to-use-table-storage/blob5.png
  [Blob6]: ./media/storage-dotnet-how-to-use-table-storage/blob6.png
  [Blob7]: ./media/storage-dotnet-how-to-use-table-storage/blob7.png
  [Blob8]: ./media/storage-dotnet-how-to-use-table-storage/blob8.png
  [Blob9]: ./media/storage-dotnet-how-to-use-table-storage/blob9.png

  [Příspěvek na blog zavedením Upsert a projekce dotazu]: http://blogs.msdn.com/b/windowsazurestorage/archive/2011/09/15/windows-azure-tables-introducing-upsert-and-query-projection.aspx
  [.NET Client Library reference]: http://go.microsoft.com/fwlink/?LinkID=390731&clcid=0x409
  [Azure Storage Team blog]: http://blogs.msdn.com/b/windowsazurestorage/
  [Configure Azure Storage connection strings]: http://msdn.microsoft.com/library/azure/ee758697.aspx
  [OData]: http://nuget.org/packages/Microsoft.Data.OData/5.0.2
  [Edm]: http://nuget.org/packages/Microsoft.Data.Edm/5.0.2
  [Spatial]: http://nuget.org/packages/System.Spatial/5.0.2
  [How to: Programmatically access Table storage]: #tablestorage
