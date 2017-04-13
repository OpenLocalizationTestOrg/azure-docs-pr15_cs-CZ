<properties
    pageTitle="Použití úložiště tabulek z Java | Microsoft Azure"
    description="Obsahují strukturovaných dat v cloudu pomocí úložiště tabulek Azure, úložiště dat NoSQL."
    services="storage"
    documentationCenter="java"
    authors="tamram"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="Java"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="tamram"/>


# <a name="how-to-use-table-storage-from-java"></a>Použití úložiště tabulek z Java

[AZURE.INCLUDE [storage-selector-table-include](../../includes/storage-selector-table-include.md)]
<br/>
[AZURE.INCLUDE [storage-try-azure-tools-queues](../../includes/storage-try-azure-tools-tables.md)]

## <a name="overview"></a>Základní informace

Tento průvodce vám ukáže, jak provádět běžné scénáře pomocí služba úložiště tabulek Azure. Vzorky psaných Java a použít v [Azure úložiště SDK jazyka Java][]. Scénáře nichž se uplatní zahrnují **Vytvoření**, **zobrazení**nebo **Odstranění** tabulky, jakož i **vkládání**, **dotazování**, **Úprava**a **Odstranění** entity v tabulce. Další informace o tabulkách naleznete v části [Další kroky](#Next-Steps) .

Poznámky: Sadu SDK je k dispozici pro vývojáře, kteří používají Azure úložiště na zařízení s Androidem. Další informace najdete v tématu [Azure SDK úložiště pro Android][].

[AZURE.INCLUDE [storage-table-concepts-include](../../includes/storage-table-concepts-include.md)]

[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="create-a-java-application"></a>Vytvoření aplikace Java

V této příručce použije funkce úložiště, které lze provádět v aplikaci Java místně nebo v kódu spuštění v rámci webu nebo pracovního role v Azure.

Chcete-li provést, musíte si nainstalovat Java Development Kit (JDK) a vytvořit účet Azure úložiště v Azure předplatné. Až budete mít Hotovo tak, je třeba ověřit, jestli vývoj systému splňuje minimální požadavky a závislosti, které jsou uvedené v úložišti [Azure úložiště SDK jazyka Java][] na GitHub. Pokud počítač splňuje tyto požadavky splnit, můžete postupovat podle pokynů ke stažení a instalaci knihoven úložiště Azure jazyka Java systému z této úložiště. Po dokončení těchto úkolů, budete moct vytvářet aplikace Java, který využívá příkladech v tomto článku.

## <a name="configure-your-application-to-access-table-storage"></a>Konfigurace aplikace pro přístup k úložiště tabulek

Přidejte následující příkazy importovat do horní části Java soubor, ve které chcete používat Microsoft Azure úložiště API pro přístup k tabulkám:

    // Include the following imports to use table APIs
    import com.microsoft.azure.storage.*;
    import com.microsoft.azure.storage.table.*;
    import com.microsoft.azure.storage.table.TableQuery.*;

## <a name="setup-an-azure-storage-connection-string"></a>Nastavení připojovací řetězec Azure úložiště

Azure úložiště klienta používá k ukládání koncové body a přihlašovací údaje pro přístup k dat správu přístupových připojovací řetězec úložiště. Při spuštění v klientské aplikaci, je nutné zadat připojovací řetězec úložiště ve formátu následující nazvanou podle této účtu úložiště a primární přístupová klávesa úložiště účtu uvedený na [Portálu Azure](https://portal.azure.com) pro hodnoty *název účtu* a *AccountKey* . Tento příklad ukazuje, jak můžou deklarovat statické pole pro uložení připojovací řetězec:

    // Define the connection-string with your values.
    public static final String storageConnectionString =
        "DefaultEndpointsProtocol=http;" +
        "AccountName=your_storage_account;" +
        "AccountKey=your_storage_account_key";

V aplikaci spuštění v rámci role v Microsoft Azure tento řetězec mohou být uloženy v souboru konfigurace služby *ServiceConfiguration.cscfg*a dají se otevřít hovoru metody **RoleEnvironment.getConfigurationSettings** . Tady je příklad získání připojovacího řetězce z **Nastavení** prvku s názvem *StorageConnectionString* v souboru konfigurace služby:

    // Retrieve storage account from connection-string.
    String storageConnectionString =
        RoleEnvironment.getConfigurationSettings().get("StorageConnectionString");

Následující příklady se předpokládá, že používáte jedním z následujících dvou způsobů jak získat připojovací řetězec úložiště.

## <a name="how-to-create-a-table"></a>Postup: vytvoření tabulky

Objekt **CloudTableClient** slouží k získání odkazů na objekty pro tabulky a entity. Následující kód vytvoří objekt **CloudTableClient** a používá k vytvoření nového **CloudTable** objektu, který představuje v tabulce s názvem "lidé". (Poznámka: existují další způsoby, jak vytvořit **CloudStorageAccount** objekty; Další informace najdete v tématu **CloudStorageAccount** v [Azure úložiště klienta SDK Reference].)

    try
    {
        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount =
           CloudStorageAccount.parse(storageConnectionString);

       // Create the table client.
       CloudTableClient tableClient = storageAccount.createCloudTableClient();

       // Create the table if it doesn't exist.
       String tableName = "people";
       CloudTable cloudTable = tableClient.getTableReference(tableName);
       cloudTable.createIfNotExists();
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }

## <a name="how-to-list-the-tables"></a>Postup: seznamu tabulek

K získání seznamu tabulek, zavolejte metodu **CloudTableClient.listTables()** načíst iterable seznam názvů tabulek.

    try
    {
        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount =
           CloudStorageAccount.parse(storageConnectionString);

        // Create the table client.
        CloudTableClient tableClient = storageAccount.createCloudTableClient();

        // Loop through the collection of table names.
        for (String table : tableClient.listTables())
        {
          // Output each table name.
          System.out.println(table);
       }
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }

## <a name="how-to-add-an-entity-to-a-table"></a>Postup: Přidání entita do tabulky

Mapa entity Java objektů pomocí vlastní třídy provádění **TableEntity**. Pro usnadnění třídy **TableServiceEntity** implementuje **TableEntity** a používá odraz mapování vlastností příjemce a nastavení metod s názvem vlastností. Entita do tabulky přidáte nejprve vytvořte třídy, která definuje vlastnosti vaší entity. Následující kód definuje entity třídy, která používá jméno zákazníka jako klíče řádku a příjmení jako klíč oddílu. Společně entity oddíl a klíče řádku jednoznačně identifikovat jednotku, v tabulce. Entity se stejným klíčem oddíl jde zpracovávat rychleji než s klávesami jiný oddíl.

    public class CustomerEntity extends TableServiceEntity {
        public CustomerEntity(String lastName, String firstName) {
            this.partitionKey = lastName;
            this.rowKey = firstName;
        }

        public CustomerEntity() { }

        String email;
        String phoneNumber;

        public String getEmail() {
            return this.email;
        }

        public void setEmail(String email) {
            this.email = email;
        }

        public String getPhoneNumber() {
            return this.phoneNumber;
        }

        public void setPhoneNumber(String phoneNumber) {
            this.phoneNumber = phoneNumber;
        }
    }

Operace s tabulkou týkající se entity vyžadují **TableOperation** objektu. Tento objekt definuje operaci, která má být provádějí entita, které lze provádět s objektem **CloudTable** . Následující kód vytvoří nové instance třídy **CustomerEntity** s daty zákazníků uložený. Kód další hovory **TableOperation.insertOrReplace** vytváření **TableOperation** objektů k vložení entita do tabulky a přidruží nové **CustomerEntity** . Nakonec kód volá **Spustit** metodu objektu **CloudTable** určující tabulku "lidé" a nové **TableOperation**, který potom odešle žádost o službu úložiště vložte nového entitu Zákazník do tabulky "lidé" a nahrazování entitu, pokud již existuje.

    try
    {
        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount =
           CloudStorageAccount.parse(storageConnectionString);

        // Create the table client.
        CloudTableClient tableClient = storageAccount.createCloudTableClient();

        // Create a cloud table object for the table.
        CloudTable cloudTable = tableClient.getTableReference("people");

        // Create a new customer entity.
        CustomerEntity customer1 = new CustomerEntity("Harp", "Walter");
        customer1.setEmail("Walter@contoso.com");
        customer1.setPhoneNumber("425-555-0101");

        // Create an operation to add the new customer to the people table.
        TableOperation insertCustomer1 = TableOperation.insertOrReplace(customer1);

        // Submit the operation to the table service.
        cloudTable.execute(insertCustomer1);
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }

## <a name="how-to-insert-a-batch-of-entities"></a>Postup: vložení listu entit

Vložení dávku entit tabulku Služba zápisu najednou. Následující kód vytvoří **TableBatchOperation** objekt a potom sečtením tři vkládáte operace k němu. Každý operace vložení se přidá vytvořením nového objektu entitu, nastavení své hodnoty a volání metody **Vložení** na objekt **TableBatchOperation** entitu přidružit Nová operace vložit. Potom kód volá **spuštění** objektu **CloudTable** určující tabulku "lidé" a **TableBatchOperation** objekt, který odešle dávky operací tabulku služba úložiště v jednom požadavku.

    try
    {
        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount =
           CloudStorageAccount.parse(storageConnectionString);

        // Create the table client.
        CloudTableClient tableClient = storageAccount.createCloudTableClient();

        // Define a batch operation.
        TableBatchOperation batchOperation = new TableBatchOperation();

        // Create a cloud table object for the table.
        CloudTable cloudTable = tableClient.getTableReference("people");

        // Create a customer entity to add to the table.
        CustomerEntity customer = new CustomerEntity("Smith", "Jeff");
        customer.setEmail("Jeff@contoso.com");
        customer.setPhoneNumber("425-555-0104");
        batchOperation.insertOrReplace(customer);

       // Create another customer entity to add to the table.
       CustomerEntity customer2 = new CustomerEntity("Smith", "Ben");
       customer2.setEmail("Ben@contoso.com");
       customer2.setPhoneNumber("425-555-0102");
       batchOperation.insertOrReplace(customer2);

       // Create a third customer entity to add to the table.
       CustomerEntity customer3 = new CustomerEntity("Smith", "Denise");
       customer3.setEmail("Denise@contoso.com");
       customer3.setPhoneNumber("425-555-0103");
       batchOperation.insertOrReplace(customer3);

       // Execute the batch of operations on the "people" table.
       cloudTable.execute(batchOperation);
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }

Některé věci, které je potřeba pamatovat na dávkové operace:

- Můžete provést až 100 vložení, odstranění, sloučení, nahradit, vložit nebo sloučit a vložit nebo nahrazení operace v libovolné kombinace v jedné dávce.
- Pokud je operaci jenom na listu, můžete mít operace dávku operace načíst.
- Všechny entity v operaci jedné dávce musí mít stejný klíč oddílu.
- Operace dávka se omezí na datová 4MB.

## <a name="how-to-retrieve-all-entities-in-a-partition"></a>Postup: načíst všechny entity v oddílu

K vytvoření dotazu tabulky entit v oddílu, můžete použít **TableQuery**. Zavolejte na linku **TableQuery.from** k vytvoření dotazu na určitou tabulku, která vrátí hodnotu typu zadaný výsledek. Následující kód určující filtr entit "Novák"-li klíč oddílu. **TableQuery.generateFilterCondition** je metodu Pomocníka pro vytváření filtrů pro dotazy. Zavolejte na **místo, kam** na vrátí metodou **TableQuery.from** filtr použijete pro dotaz. Při spuštění dotazu pomocí hovoru na objekt **CloudTable** **Spustit** vrátí **iterace** určeného typu výsledku **CustomerEntity** . Můžete použít **iterace** vrácený v pro každou smyčka výsledky využívat. Tento kód Vytiskne pole jednotlivé entity ve výsledcích dotazu ke konzole.

    try
    {
        // Define constants for filters.
        final String PARTITION_KEY = "PartitionKey";
        final String ROW_KEY = "RowKey";
        final String TIMESTAMP = "Timestamp";

        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount =
           CloudStorageAccount.parse(storageConnectionString);

        // Create the table client.
        CloudTableClient tableClient = storageAccount.createCloudTableClient();

       // Create a cloud table object for the table.
       CloudTable cloudTable = tableClient.getTableReference("people");

        // Create a filter condition where the partition key is "Smith".
        String partitionFilter = TableQuery.generateFilterCondition(
           PARTITION_KEY,
           QueryComparisons.EQUAL,
           "Smith");

       // Specify a partition query, using "Smith" as the partition key filter.
       TableQuery<CustomerEntity> partitionQuery =
           TableQuery.from(CustomerEntity.class)
           .where(partitionFilter);

        // Loop through the results, displaying information about the entity.
        for (CustomerEntity entity : cloudTable.execute(partitionQuery)) {
            System.out.println(entity.getPartitionKey() +
                " " + entity.getRowKey() +
                "\t" + entity.getEmail() +
                "\t" + entity.getPhoneNumber());
       }
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }

## <a name="how-to-retrieve-a-range-of-entities-in-a-partition"></a>Postup: načíst oblasti entit v oddílu

Pokud nechcete k vytvoření dotazu všechny entity v oddílu, můžete určit oblast pomocí relačních operátorů ve filtru. Následující kód spojuje dva filtry získat "Miklus" místo, kam klávesu řádku (křestní jméno) začíná písmenem, aby byl "E" všechny entity v oddílu abecedy. Potom vytiskne výsledků dotazu. Pokud používáte entity přidané do tabulky na listu vložit část tato příručka, budou vráceny pouze dvě entity tentokrát (Robert a Novák Karel); Petr Novák není však započítávány.

    try
    {
        // Define constants for filters.
        final String PARTITION_KEY = "PartitionKey";
        final String ROW_KEY = "RowKey";
        final String TIMESTAMP = "Timestamp";

        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount =
           CloudStorageAccount.parse(storageConnectionString);

       // Create the table client.
       CloudTableClient tableClient = storageAccount.createCloudTableClient();

       // Create a cloud table object for the table.
       CloudTable cloudTable = tableClient.getTableReference("people");

        // Create a filter condition where the partition key is "Smith".
        String partitionFilter = TableQuery.generateFilterCondition(
           PARTITION_KEY,
           QueryComparisons.EQUAL,
           "Smith");

        // Create a filter condition where the row key is less than the letter "E".
        String rowFilter = TableQuery.generateFilterCondition(
           ROW_KEY,
           QueryComparisons.LESS_THAN,
           "E");

        // Combine the two conditions into a filter expression.
        String combinedFilter = TableQuery.combineFilters(partitionFilter,
            Operators.AND, rowFilter);

        // Specify a range query, using "Smith" as the partition key,
        // with the row key being up to the letter "E".
        TableQuery<CustomerEntity> rangeQuery =
           TableQuery.from(CustomerEntity.class)
           .where(combinedFilter);

        // Loop through the results, displaying information about the entity
        for (CustomerEntity entity : cloudTable.execute(rangeQuery)) {
            System.out.println(entity.getPartitionKey() +
                " " + entity.getRowKey() +
                "\t" + entity.getEmail() +
                "\t" + entity.getPhoneNumber());
        }
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }

## <a name="how-to-retrieve-a-single-entity"></a>Postup: načíst jedinou osobou

Můžete vytvořit dotaz k načtení jedné konkrétní entita. Následující kód volání **TableOperation.retrieve** klíč oddílu a řádek klíčové parametry určující zákazník "Petr Novák" místo vytvoření **TableQuery** a použitím filtrů mít stejný výsledek. Při spuštění vrátí operaci obnovit jenom jedna osoba, nikoli kolekce. Metoda **getResultAsType** přetypuje výsledek typu přiřazení cílové **CustomerEntity** objektu. Pokud tento typ není kompatibilní s typem zadané pro dotaz, bude mít výjimka. Pokud žádná entita přesné oddíl a klíče řádku odpovídají bude vrácena hodnota null. Určení oddíl a řádek v dotazu je nejrychlejší způsob, jak načíst jedna entita z tabulky služby.

    try
    {
        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount =
           CloudStorageAccount.parse(storageConnectionString);

        // Create the table client.
        CloudTableClient tableClient = storageAccount.createCloudTableClient();

        // Create a cloud table object for the table.
        CloudTable cloudTable = tableClient.getTableReference("people");

        // Retrieve the entity with partition key of "Smith" and row key of "Jeff"
        TableOperation retrieveSmithJeff =
           TableOperation.retrieve("Smith", "Jeff", CustomerEntity.class);

       // Submit the operation to the table service and get the specific entity.
       CustomerEntity specificEntity =
            cloudTable.execute(retrieveSmithJeff).getResultAsType();

        // Output the entity.
        if (specificEntity != null)
        {
            System.out.println(specificEntity.getPartitionKey() +
                " " + specificEntity.getRowKey() +
                "\t" + specificEntity.getEmail() +
                "\t" + specificEntity.getPhoneNumber());
       }
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }

## <a name="how-to-modify-an-entity"></a>Postup: Úprava entity

Úprava entity načíst z tabulky služby, změňte objektu entity a ukládání změn zpátky na tabulku služba s operace nahradit nebo hromadné korespondence. Následující kód změní existující zákazníka telefonní číslo. Místo volání **TableOperation.insert** jako jsme udělali My vložení, tento kód volá **TableOperation.replace**. Metoda **CloudTable.execute** volá tabulku služba a nahradit entitu, pokud jiné aplikace změněn ho času ji přečetli této aplikace. Když k tomu dojde, k výjimce a entitu musí být načíst, upravit a opět uložen. Tento způsob opakování optimistické souběžné je běžné v systému distribuované úložiště.

    try
    {
        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount =
           CloudStorageAccount.parse(storageConnectionString);

        // Create the table client.
        CloudTableClient tableClient = storageAccount.createCloudTableClient();

        // Create a cloud table object for the table.
        CloudTable cloudTable = tableClient.getTableReference("people");

        // Retrieve the entity with partition key of "Smith" and row key of "Jeff".
        TableOperation retrieveSmithJeff =
           TableOperation.retrieve("Smith", "Jeff", CustomerEntity.class);

        // Submit the operation to the table service and get the specific entity.
        CustomerEntity specificEntity =
          cloudTable.execute(retrieveSmithJeff).getResultAsType();

        // Specify a new phone number.
        specificEntity.setPhoneNumber("425-555-0105");

        // Create an operation to replace the entity.
        TableOperation replaceEntity = TableOperation.replace(specificEntity);

        // Submit the operation to the table service.
        cloudTable.execute(replaceEntity);
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }

## <a name="how-to-query-a-subset-of-entity-properties"></a>Postup: podmnožinu entity vlastností dotazu

Dotaz do tabulky můžete načíst pár vlastnosti z entity. Tento postup s názvem projekce, snižuje šířky pásma a dosáhnout zvýšení výkonu dotazu, zejména u velkých entity. Dotaz následující kód používá **Vyberte** metodu k vrácení e-mailové adresy osob v tabulce. Výsledky jsou promítat do kolekce **řetězce** s pomocí **EntityResolver**, který provádí převod typu u entit vrácené ze serveru. Další informace o projekci v [tabulkami Azure: úvodní informace o Upsert a projekce dotazu][]. Všimněte si, že projekce není podporován emulátoru místní úložiště tak tento kód spuštěn pouze v případě, že pomocí účtu na tabulku služba.

    try
    {
        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount =
            CloudStorageAccount.parse(storageConnectionString);

        // Create the table client.
        CloudTableClient tableClient = storageAccount.createCloudTableClient();

        // Create a cloud table object for the table.
        CloudTable cloudTable = tableClient.getTableReference("people");

        // Define a projection query that retrieves only the Email property
        TableQuery<CustomerEntity> projectionQuery =
           TableQuery.from(CustomerEntity.class)
           .select(new String[] {"Email"});

        // Define a Entity resolver to project the entity to the Email value.
        EntityResolver<String> emailResolver = new EntityResolver<String>() {
            @Override
            public String resolve(String PartitionKey, String RowKey, Date timeStamp, HashMap<String, EntityProperty> properties, String etag) {
                return properties.get("Email").getValueAsString();
            }
        };

        // Loop through the results, displaying the Email values.
        for (String projectedString :
            cloudTable.execute(projectionQuery, emailResolver)) {
                System.out.println(projectedString);
        }
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }

## <a name="how-to-insert-or-replace-an-entity"></a>Postup: vložení nebo nahrazení entita

Často chcete přidat entita do tabulky bez vědět, pokud již existuje v tabulce. Vložení nebo nahradit operace je možné provádět jednoho požadavku, který vloží entitu pokud ho neexistuje nebo nahradit stávající Pokud ano. Následující kód vytváření na předchozí příklady, vložení nebo nahrazení entitu pro "Walter Harp". Po vytvoření nového entitu, tento kód volá metodu **TableOperation.insertOrReplace** . Tento kód pak vyzývá **Spustit** **CloudTable** objekt s tabulkou a vložit nebo nahrazení operace tabulka jako parametry. Pokud chcete aktualizovat jenom část entitu, metodu **TableOperation.insertOrMerge** lze místo. Poznámka: Tento vložit nebo nahrazování nepodporuje v místním úložišti PDC tak tento kód spuštěn pouze v případě, že pomocí účtu na tabulku služba. Se dozvíte další informace o vložení nebo nahradit a vložit nebo korespondence v tomto [tabulkami Azure: úvodní informace o Upsert a projekce dotazu][].

    try
    {
        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount =
            CloudStorageAccount.parse(storageConnectionString);

        // Create the table client.
        CloudTableClient tableClient = storageAccount.createCloudTableClient();

        // Create a cloud table object for the table.
        CloudTable cloudTable = tableClient.getTableReference("people");

        // Create a new customer entity.
        CustomerEntity customer5 = new CustomerEntity("Harp", "Walter");
        customer5.setEmail("Walter@contoso.com");
        customer5.setPhoneNumber("425-555-0106");

        // Create an operation to add the new customer to the people table.
        TableOperation insertCustomer5 = TableOperation.insertOrReplace(customer5);

        // Submit the operation to the table service.
        cloudTable.execute(insertCustomer5);
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }

## <a name="how-to-delete-an-entity"></a>Postup: Odstranění entity

Po načtení ho můžete snadno odstranit entity. Jakmile načtené entitu volejte **TableOperation.delete** entity odstranit. Potom **Spustit** na **CloudTable** objekt zavolejte. Následující kód vyhledá a odstraní entitu Zákazník.

    try
    {
        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount =
            CloudStorageAccount.parse(storageConnectionString);

        // Create the table client.
        CloudTableClient tableClient = storageAccount.createCloudTableClient();

        // Create a cloud table object for the table.
        CloudTable cloudTable = tableClient.getTableReference("people");

        // Create an operation to retrieve the entity with partition key of "Smith" and row key of "Jeff".
        TableOperation retrieveSmithJeff = TableOperation.retrieve("Smith", "Jeff", CustomerEntity.class);

        // Retrieve the entity with partition key of "Smith" and row key of "Jeff".
        CustomerEntity entitySmithJeff =
            cloudTable.execute(retrieveSmithJeff).getResultAsType();

        // Create an operation to delete the entity.
        TableOperation deleteSmithJeff = TableOperation.delete(entitySmithJeff);

        // Submit the delete operation to the table service.
        cloudTable.execute(deleteSmithJeff);
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }

## <a name="how-to-delete-a-table"></a>Postup: odstranění tabulky

Nakonec následující kód slouží k odstranění tabulky z účtu úložiště. Tabulky, který byl odstraněn přestane být dostupná vytvářejte dobu po odstranění, obvykle menší než 40 sekund.

    try
    {
        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount =
            CloudStorageAccount.parse(storageConnectionString);

        // Create the table client.
        CloudTableClient tableClient = storageAccount.createCloudTableClient();

        // Delete the table and all its data if it exists.
        CloudTable cloudTable = new CloudTable("people",tableClient);
        cloudTable.deleteIfExists();
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }

## <a name="next-steps"></a>Další kroky

Teď, když jste se naučili základy úložiště tabulek, postupujte podle těchto odkazů se dozvíte, jak provádět složitější úlohy úložiště.

- [Azure úložiště SDK jazyka Java][]
- [Azure úložiště klienta SDK Reference][]
- [Azure úložiště REST API][]
- [Blog týmu Azure úložiště][]

Další informace najdete v tématu taky [Středisko pro vývojáře Java](/develop/java/).


[Azure SDK for Java]: http://go.microsoft.com/fwlink/?LinkID=525671
[Azure úložiště SDK jazyka Java]: https://github.com/azure/azure-storage-java
[Azure úložiště SDK pro Android]: https://github.com/azure/azure-storage-android
[Azure úložiště klienta SDK Reference]: http://dl.windowsazure.com/storage/javadoc/
[Azure úložiště REST API]: https://msdn.microsoft.com/library/azure/dd179355.aspx
[Blog týmu Azure úložiště]: http://blogs.msdn.com/b/windowsazurestorage/
[Azure tabulky: Úvodní informace o Upsert a projekce dotazu]: http://blogs.msdn.com/b/windowsazurestorage/archive/2011/09/15/windows-azure-tables-introducing-upsert-and-query-projection.aspx
