<properties
    pageTitle="Začínáme s úložiště tabulek a Visual Studio připojené služby (cloudovým službám) | Microsoft Azure"
    description="Jak začít používat úložiště tabulek Azure v aplikaci project cloudové služby ve Visual Studiu po připojení k úložišti účtu pomocí aplikace Visual Studio připojené služby"
    services="storage"
    documentationCenter=""
    authors="TomArcher"
    manager="douge"
    editor=""/>

<tags
    ms.service="storage"
    ms.workload="web"
    ms.tgt_pltfrm="vs-getting-started"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/18/2016"
    ms.author="tarcher"/>

# <a name="getting-started-with-azure-table-storage-and-visual-studio-connected-services-cloud-services-projects"></a>Začínáme s úložiště tabulek Azure a Visual Studia připojené služby (cloud services projektů)

[AZURE.INCLUDE [storage-try-azure-tools-tables](../../includes/storage-try-azure-tools-tables.md)]

##<a name="overview"></a>Základní informace

Tento článek popisuje, jak začít používat úložiště tabulek Azure ve Visual Studiu, když jste vytvořili nebo odkazuje účet Azure úložiště v aplikaci project služby cloudu pomocí dialogového okna aplikace Visual Studio **Přidat připojené služby** . **Přidání připojené služby** operace nainstaluje odpovídajících balíčků NuGet pro přístup k Azure úložiště v projektu a přidá připojovací řetězec účtu úložiště souborů konfigurace projektu.

Služba úložiště tabulek Azure umožňuje ukládat velké množství strukturovaná data. Služba je NoSQL úložiště, která přijímá ověřené hovory z uvnitř a mimo Azure cloudu. Azure tabulky jsou ideální pro ukládání strukturovaných, není relačních dat.

Začněte tím, musíte nejdřív vytvořit tabulku ve vašem účtu úložiště. Ukážeme vám jak vytvořit Azure table v kódu a taky jak provádět základní tabulka a entity operací, jako je přidání, úprava, čtení a čtení tabulky entity. Vzorky jsou napsané v jazyce C\# kódu a používat [knihovnu úložišti tabulek Microsoft Azure klienta pro .NET](https://msdn.microsoft.com/library/azure/dn261237.aspx).

**Poznámka:** Některé rozhraní API provádějící volání se k základnímu úložišti Azure jsou asynchronní. Další informace najdete v tématu [asynchronní programování s asynchronní a Await](http://msdn.microsoft.com/library/hh191443.aspx) . Následující kód předpokládá, že použily asynchronní programovací metody.

- Další informace o programově práci s tabulkami najdete v článku [Začínáme s úložiště tabulek Azure pomocí .NET](storage-dotnet-how-to-use-tables.md) .
- Obecné informace o úložišti Azure najdete v článku [si přečtěte následující dokumentaci úložiště](https://azure.microsoft.com/documentation/services/storage/) .
- Obecné informace o Azure cloudovým službám najdete v článku [si přečtěte následující dokumentaci Cloudovým službám](https://azure.microsoft.com/documentation/services/cloud-services/) .
- Další informace o programování ASP.NET aplikací najdete v článku [technologie ASP.NET](http://www.asp.net) .

## <a name="access-tables-in-code"></a>Tabulky aplikace Access v kódu

Přístup k tabulek na projektech cloudové služby, potřebujete zahrnout následující položky, které chcete všechny C# zdrojové soubory, které přístup ke úložiště tabulek Azure.

1. Zkontrolujte, že oboru názvů prohlášení v horní části souboru C# obsahuje tyto příkazy **pomocí** .

        using Microsoft.Framework.Configuration;
        using Microsoft.WindowsAzure.Storage;
        using Microsoft.WindowsAzure.Storage.Table;
        using System.Threading.Tasks;
        using LogLevel = Microsoft.Framework.Logging.LogLevel;

2. Pokud potřebujete **CloudStorageAccount** objekt, který představuje informací o účtu úložiště. Pomocí následující kód úložiště připojovacího řetězce a informace o účtu úložiště z konfiguraci služby Azure.

         CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
           CloudConfigurationManager.GetSetting("<storage account name>
         _AzureStorageConnectionString"));
> [AZURE.NOTE]  Pomocí výše uvedených kód před kód v následujících vzorcích.

3. Získejte objekt **CloudTableClient** jako odkaz na tabulku objekty ve vašem účtu úložiště.

         // Create the table client.
         CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

4. Pokud potřebujete objekt **CloudTable** referenční neodkazuje na konkrétní tabulku a entity.

        // Get a reference to a table named "peopleTable".
        CloudTable peopleTable = tableClient.GetTableReference("peopleTable");

## <a name="create-a-table-in-code"></a>Vytvoření tabulky v kódu

Pokud chcete vytvořit Azure table, stačí přidejte volání **CreateIfNotExistsAsync** k po získání objektu **CloudTable** podle popisu v části "Přístup k tabulkám v kódu".

    // Create the CloudTable if it does not exist.
    await peopleTable.CreateIfNotExistsAsync();

## <a name="add-an-entity-to-a-table"></a>Přidání entita do tabulky

Entita do tabulky přidáte vytvořte třídy, která definuje vlastnosti vaší entity. Následující kód definuje třídu entity s názvem **CustomerEntity** používající křestní jméno zákazníka jako klíč řádku jméno a příjmení jako klíč oddílu.

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

Operace s tabulkou týkající se entity dělají pomocí **CloudTable** objektu, který jste vytvořili v "přístup k tabulkám v kódu." Objekt **TableOperation** představuje operaci zbývá dokončit. Následující příklad ukazuje, jak vytvořit **CloudTable** objektu a objekt **CustomerEntity** . Příprava operaci, vytvoří se **TableOperation** vložit entitu Zákazník do tabulky. Nakonec na provedení operace tak, že zavoláte **CloudTable.ExecuteAsync**.

    // Create a new customer entity.
    CustomerEntity customer1 = new CustomerEntity("Harp", "Walter");
    customer1.Email = "Walter@contoso.com";
    customer1.PhoneNumber = "425-555-0101";

    // Create the TableOperation that inserts the customer entity.
    TableOperation insertOperation = TableOperation.Insert(customer1);

    // Execute the insert operation.
    await peopleTable.ExecuteAsync(insertOperation);


## <a name="insert-a-batch-of-entities"></a>Vložení listu entit

Vložení více entit do tabulky v operaci jednoho zápisu. Následující příklad vytvoří dva objekty entity ("Petr Novák" a "Petr Novák"), přidá k objektu **TableBatchOperation** metodou vložit a potom začne operace tak, že zavoláte **CloudTable.ExecuteBatchAsync**.

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
    await peopleTable.ExecuteBatchAsync(batchOperation);

## <a name="get-all-of-the-entities-in-a-partition"></a>Získání všech osob do oddílu

K vytvoření dotazu tabulku u všech osob do oddílu, použijte **TableQuery** objekt. Následující příklad určující filtr entit "Novák"-li klíč oddílu. V tomto příkladě se vytiskne pole jednotlivé entity ve výsledcích dotazu ke konzole.

    // Construct the query operation for all customer entities where PartitionKey="Smith".
    TableQuery<CustomerEntity> query = new TableQuery<CustomerEntity>()
        .Where(TableQuery.GenerateFilterCondition("PartitionKey", QueryComparisons.Equal, "Smith"));

    // Print the fields for each customer.
    TableContinuationToken token = null;
    do
    {
        TableQuerySegment<CustomerEntity> resultSegment = await peopleTable.ExecuteQuerySegmentedAsync(query, token);
        token = resultSegment.ContinuationToken;

        foreach (CustomerEntity entity in resultSegment.Results)
        {
            Console.WriteLine("{0}, {1}\t{2}\t{3}", entity.PartitionKey, entity.RowKey,
            entity.Email, entity.PhoneNumber);
        }
    } while (token != null);

    return View();


## <a name="get-a-single-entity"></a>Získání jedinou osobou

Můžete napsat k získání jediné, konkrétní entity. Následující kód používá objekt **TableOperation** k určení zákazníků s názvem Petr Novák. Tento způsob vrátí jenom jedna osoba, nikoli kolekce a vrácená hodnota v **TableResult.Result** je objekt **CustomerEntity** . Určení oddíl a řádek v dotazu je nejrychlejší způsob, jak načíst jedna entita z **tabulky** služby.

    // Create a retrieve operation that takes a customer entity.
    TableOperation retrieveOperation = TableOperation.Retrieve<CustomerEntity>("Smith", "Ben");

    // Execute the retrieve operation.
    TableResult retrievedResult = await peopleTable.ExecuteAsync(retrieveOperation);

    // Print the phone number of the result.
    if (retrievedResult.Result != null)
       Console.WriteLine(((CustomerEntity)retrievedResult.Result).PhoneNumber);
    else
       Console.WriteLine("The phone number could not be retrieved.");

## <a name="delete-an-entity"></a>Odstranění entity
Po vyhledání poklikáte, můžete odstranit entity. Následující kód nezačne vyhledávat entitu Zákazník s názvem "Petr Novák" a pokud najde, odstraní se.

    // Create a retrieve operation that expects a customer entity.
    TableOperation retrieveOperation = TableOperation.Retrieve<CustomerEntity>("Smith", "Ben");

    // Execute the operation.
    TableResult retrievedResult = peopleTable.Execute(retrieveOperation);

    // Assign the result to a CustomerEntity object.
    CustomerEntity deleteEntity = (CustomerEntity)retrievedResult.Result;

    // Create the Delete TableOperation and then execute it.
    if (deleteEntity != null)
    {
       TableOperation deleteOperation = TableOperation.Delete(deleteEntity);

       // Execute the operation.
       await peopleTable.ExecuteAsync(deleteOperation);

       Console.WriteLine("Entity deleted.");
    }

    else
       Console.WriteLine("Couldn't delete the entity.");

## <a name="next-steps"></a>Další kroky

[AZURE.INCLUDE [vs-storage-dotnet-tables-next-steps](../../includes/vs-storage-dotnet-tables-next-steps.md)]
