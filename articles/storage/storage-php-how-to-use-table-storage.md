<properties
    pageTitle="Použití úložiště tabulek z PHP | Microsoft Azure"
    description="Naučte se používat službu tabulky z PHP k vytváření a odstranit tabulku, vložení, odstranění a dotaz v tabulce."
    services="storage"
    documentationCenter="php"
    authors="tamram"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="php"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="tamram"/>


# <a name="how-to-use-table-storage-from-php"></a>Použití úložiště tabulek z PHP

[AZURE.INCLUDE [storage-selector-table-include](../../includes/storage-selector-table-include.md)]
<br/>
[AZURE.INCLUDE [storage-try-azure-tools-queues](../../includes/storage-try-azure-tools-tables.md)]

## <a name="overview"></a>Základní informace

Tato příručka, uvidíte, jak provádět běžné scénáře pomocí služby Azure tabulka. Vzorky psaných PHP a používat [Azure SDK PHP][download]. Scénáře nichž se uplatní zahrnují **vytváření a odstranění tabulky, vložení, odstranění a dotazování entity v tabulce**. Další informace o službě tabulky Azure naleznete v části [Další kroky](#next-steps) .

[AZURE.INCLUDE [storage-table-concepts-include](../../includes/storage-table-concepts-include.md)]

[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="create-a-php-application"></a>Vytvoření aplikace PHP

Pouze požadavku na vytvoření aplikace PHP, přistupovat ke službě tabulky Azure je odkazování tříd v Azure SDK pro PHP z vašeho kódu. Vytvoření aplikace, včetně Poznámkový blok můžete použít libovolný vývojářské nástroje.

V této příručce pomocí funkce služby tabulky, které můžete volat z aplikace PHP místně, nebo kód spuštěný v rámci Azure web role, kolegy nebo Web.

## <a name="get-the-azure-client-libraries"></a>Získání knihoven Azure klienta

[AZURE.INCLUDE [get-client-libraries](../../includes/get-client-libraries.md)]

## <a name="configure-your-application-to-access-the-table-service"></a>Konfigurace aplikace pro přístup ke službě tabulky

Používání služby Azure tabulky rozhraní API, musíte:

1. Odkaz na soubor zavaděč pomocí [require_once] [ require_once] údajů, a
2. Odkaz na všechny třídy, které používáte.

Následující příklad ukazuje, jak vložit soubor zavaděč a odkaz na třídu **ServicesBuilder** .

> [AZURE.NOTE] V tomto příkladu (a další příklady v tomto článku) se předpokládá, že jste nainstalovali knihoven PHP klienta pro Azure prostřednictvím autora. Pokud jste si nainstalovali ručně knihoven, budete muset odkazovat <code>WindowsAzure.php</code> zavaděč soubor.

    require_once 'vendor/autoload.php';
    use WindowsAzure\Common\ServicesBuilder;


V následujících příkladech `require_once` údajů vždy zobrazený, ale odkazuje pouze třídy potřebné třeba provádět.

## <a name="set-up-an-azure-storage-connection"></a>Nastavení připojení Azure úložiště

Pro vytvoření instance klienta služby Azure tabulky, musíte nejdřív máte platný připojovací řetězec. Formát tabulku služba připojovací řetězec je:

Pro přístup k živé služby:

    DefaultEndpointsProtocol=[http|https];AccountName=[yourAccount];AccountKey=[yourKey]

Pro přístup k úložišti emulátoru:

    UseDevelopmentStorage=true


Vytvoření libovolného klienta služby Azure, musíte použít třídu **ServicesBuilder** . Můžeš:

* předat připojovací řetězec přímo nebo
* zjistit víc externích zdrojů pro připojovací řetězec pomocí **CloudConfigurationManager (CCM)** :
    * ve výchozím nastavení součástí Podpora externích zdrojů – proměnné prostředí
    * přidávání nových zdrojů prodloužením **ConnectionStringSource** třídy

Příklady zde uváděného předávat připojovací řetězec, bude přímo.

    require_once 'vendor/autoload.php';

    use WindowsAzure\Common\ServicesBuilder;

    $tableRestProxy = ServicesBuilder::getInstance()->createTableService($connectionString);


## <a name="create-a-table"></a>Vytvoření tabulky

Objekt **TableRestProxy** můžete vytvořit tabulku se metodu **createTable** . Pokud vytvoříte tabulku, můžete nastavit časový limit tabulku služba. (Podrobnosti o tabulku služba vypršení časového limitu, najdete v článku [Nastavení časových limitů pro tabulku služba operace][table-service-timeouts].)

    require_once 'vendor\autoload.php';

    use WindowsAzure\Common\ServicesBuilder;
    use MicrosoftAzure\Storage\Common\ServiceException;

    // Create table REST proxy.
    $tableRestProxy = ServicesBuilder::getInstance()->createTableService($connectionString);

    try {
        // Create table.
        $tableRestProxy->createTable("mytable");
    }
    catch(ServiceException $e){
        $code = $e->getCode();
        $error_message = $e->getMessage();
        // Handle exception based on error codes and messages.
        // Error codes and messages can be found here:
        // http://msdn.microsoft.com/library/azure/dd179438.aspx
    }

Informace o omezeních na názvy tabulek, najdete v článku [Principy datového modelu tabulku služba][table-data-model].

## <a name="add-an-entity-to-a-table"></a>Přidání entita do tabulky

Entita do tabulky přidáte vytvořit nový objekt **Entita** a předejte ho **TableRestProxy -> insertEntity**. Všimněte si, že když vytvoříte entitu, je nutné zadat `PartitionKey` a `RowKey`. Toto jsou identifikátorů entity a hodnoty, které můžete vyhledávat mnohem rychleji než jiné vlastnosti entity. Použije systém `PartitionKey` rozdělí automaticky v tabulce entity mnoho úložiště uzlů. Entity se stejnými `PartitionKey` jsou uložené ve stejném uzlu. (Provádět operace s více entit uložený ve stejném uzlu lepší než u entit uložené v různých uzlech.) `RowKey` Je jedinečné ID entita v oddílu.

    require_once 'vendor/autoload.php';

    use WindowsAzure\Common\ServicesBuilder;
    use MicrosoftAzure\Storage\Common\ServiceException;
    use MicrosoftAzure\Storage\Table\Models\Entity;
    use MicrosoftAzure\Storage\Table\Models\EdmType;

    // Create table REST proxy.
    $tableRestProxy = ServicesBuilder::getInstance()->createTableService($connectionString);

    $entity = new Entity();
    $entity->setPartitionKey("tasksSeattle");
    $entity->setRowKey("1");
    $entity->addProperty("Description", null, "Take out the trash.");
    $entity->addProperty("DueDate",
                         EdmType::DATETIME,
                         new DateTime("2012-11-05T08:15:00-08:00"));
    $entity->addProperty("Location", EdmType::STRING, "Home");

    try{
        $tableRestProxy->insertEntity("mytable", $entity);
    }
    catch(ServiceException $e){
        // Handle exception based on error codes and messages.
        // Error codes and messages are here:
        // http://msdn.microsoft.com/library/azure/dd179438.aspx
        $code = $e->getCode();
        $error_message = $e->getMessage();
    }

Informace o vlastnosti tabulky a typy, najdete v článku [Principy datového modelu tabulku služba][table-data-model].

Třídy **TableRestProxy** nabízí dvě alternativní metody pro vložení entity: **insertOrMergeEntity** a **insertOrReplaceEntity**. Pomocí těchto postupů, vytvořte novou **Entita** a jej předá jako parametr obě metody. Pokud není k dispozici, obou metod vloží entity. Pokud již existuje entitu, **insertOrMergeEntity** aktualizace nemovitostí s hodnotou vlastnosti existovat a přidá nové vlastnosti, pokud neexistují, zatímco **insertOrReplaceEntity** úplně nahradí existující entity. Následující příklad ukazuje, jak používat **insertOrMergeEntity**. Pokud entita s `PartitionKey` "tasksSeattle" a `RowKey` "1" dosud neexistuje, se vloží. Ale pokud ji dřív byl vložen (jak je uvedeno v předchozím příkladu), `DueDate` vlastnost bude aktualizován a `Status` přidá vlastnost. `Description` a `Location` taky aktualizovat vlastnosti, ale s hodnotami, které efektivně necháte beze změny. Pokud tyto poslední dvě vlastnosti nebyly přidali uvedeno v příkladu, ale v cílové entity existuje, by zůstat beze změny existujících hodnot.

    require_once 'vendor/autoload.php';

    use WindowsAzure\Common\ServicesBuilder;
    use MicrosoftAzure\Storage\Common\ServiceException;
    use MicrosoftAzure\Storage\Table\Models\Entity;
    use MicrosoftAzure\Storage\Table\Models\EdmType;

    // Create table REST proxy.
    $tableRestProxy = ServicesBuilder::getInstance()->createTableService($connectionString);

    //Create new entity.
    $entity = new Entity();

    // PartitionKey and RowKey are required.
    $entity->setPartitionKey("tasksSeattle");
    $entity->setRowKey("1");

    // If entity exists, existing properties are updated with new values and
    // new properties are added. Missing properties are unchanged.
    $entity->addProperty("Description", null, "Take out the trash.");
    $entity->addProperty("DueDate", EdmType::DATETIME, new DateTime()); // Modified the DueDate field.
    $entity->addProperty("Location", EdmType::STRING, "Home");
    $entity->addProperty("Status", EdmType::STRING, "Complete"); // Added Status field.

    try {
        // Calling insertOrReplaceEntity, instead of insertOrMergeEntity as shown,
        // would simply replace the entity with PartitionKey "tasksSeattle" and RowKey "1".
        $tableRestProxy->insertOrMergeEntity("mytable", $entity);
    }
    catch(ServiceException $e){
        // Handle exception based on error codes and messages.
        // Error codes and messages are here:
        // http://msdn.microsoft.com/library/azure/dd179438.aspx
        $code = $e->getCode();
        $error_message = $e->getMessage();
        echo $code.": ".$error_message."<br />";
    }


## <a name="retrieve-a-single-entity"></a>Načtení jedné entity

Metoda **TableRestProxy -> getEntity** umožňuje načítat jedna entita pomocí dotazu na jeho `PartitionKey` a `RowKey`. V dalším příkladu vrací klíč oddílu `tasksSeattle` a klíče řádku `1` předávají metody **getEntity** .

    require_once 'vendor/autoload.php';

    use WindowsAzure\Common\ServicesBuilder;
    use MicrosoftAzure\Storage\Common\ServiceException;

    // Create table REST proxy.
    $tableRestProxy = ServicesBuilder::getInstance()->createTableService($connectionString);

    try {
        $result = $tableRestProxy->getEntity("mytable", "tasksSeattle", 1);
    }
    catch(ServiceException $e){
        // Handle exception based on error codes and messages.
        // Error codes and messages are here:
        // http://msdn.microsoft.com/library/azure/dd179438.aspx
        $code = $e->getCode();
        $error_message = $e->getMessage();
        echo $code.": ".$error_message."<br />";
    }

    $entity = $result->getEntity();

    echo $entity->getPartitionKey().":".$entity->getRowKey();

## <a name="retrieve-all-entities-in-a-partition"></a>Načtení všechny entity v oddílu

Entita dotazů je vytvořen pomocí filtrů (Další informace najdete v tématu [dotazování tabulek a entity][filters]). K načtení všechny entity v oddílu, použijte filtr "PartitionKey eq *název_oddílu*". Následující příklad ukazuje, jak získat všechny entity v `tasksSeattle` oddíl předáním filtru metodě **queryEntities** .

    require_once 'vendor/autoload.php';

    use WindowsAzure\Common\ServicesBuilder;
    use MicrosoftAzure\Storage\Common\ServiceException;

    // Create table REST proxy.
    $tableRestProxy = ServicesBuilder::getInstance()->createTableService($connectionString);

    $filter = "PartitionKey eq 'tasksSeattle'";

    try {
        $result = $tableRestProxy->queryEntities("mytable", $filter);
    }
    catch(ServiceException $e){
        // Handle exception based on error codes and messages.
        // Error codes and messages are here:
        // http://msdn.microsoft.com/library/azure/dd179438.aspx
        $code = $e->getCode();
        $error_message = $e->getMessage();
        echo $code.": ".$error_message."<br />";
    }

    $entities = $result->getEntities();

    foreach($entities as $entity){
        echo $entity->getPartitionKey().":".$entity->getRowKey()."<br />";
    }

## <a name="retrieve-a-subset-of-entities-in-a-partition"></a>Načtení podmnožiny entit v oddílu

Stejné vzorku použitého v předchozím příkladu mohou sloužit k načtení podmnožinu entit v oddílu. Podmnožinu entity načtete jsou určena filtr použijete (Další informace najdete v tématu [dotazování tabulek a entity][filters]). Následující příklad ukazuje, jak pomocí filtru k načtení všechny entity s určitým `Location` a `DueDate` menší než k určitému datu.

    require_once 'vendor/autoload.php';

    use WindowsAzure\Common\ServicesBuilder;
    use MicrosoftAzure\Storage\Common\ServiceException;

    // Create table REST proxy.
    $tableRestProxy = ServicesBuilder::getInstance()->createTableService($connectionString);

    $filter = "Location eq 'Office' and DueDate lt '2012-11-5'";

    try {
        $result = $tableRestProxy->queryEntities("mytable", $filter);
    }
    catch(ServiceException $e){
        // Handle exception based on error codes and messages.
        // Error codes and messages are here:
        // http://msdn.microsoft.com/library/azure/dd179438.aspx
        $code = $e->getCode();
        $error_message = $e->getMessage();
        echo $code.": ".$error_message."<br />";
    }

    $entities = $result->getEntities();

    foreach($entities as $entity){
        echo $entity->getPartitionKey().":".$entity->getRowKey()."<br />";
    }

## <a name="retrieve-a-subset-of-entity-properties"></a>Načtení podmnožinu entity vlastností

Dotaz můžete získat podmnožinu vlastnosti entity. Tento postup s názvem *projekce*, snižuje šířky pásma a dosáhnout zvýšení výkonu dotazu, zejména u velkých entity. Chcete-li zadat vlastnosti, které chcete načíst, název vlastnosti **dotazu -> addSelectField** metodě předána. Můžete tato metoda víckrát voláte Pokud chcete přidat další vlastnosti. Po provedení **TableRestProxy -> queryEntities**, budete mít vrácené entity pouze vybrané vlastnosti. (V Pokud budete chtít vrátit podmnožinu entity tabulky, použijte filtr podle výše uvedených dotazy.)

    require_once 'vendor/autoload.php';

    use WindowsAzure\Common\ServicesBuilder;
    use MicrosoftAzure\Storage\Common\ServiceException;
    use MicrosoftAzure\Storage\Table\Models\QueryEntitiesOptions;

    // Create table REST proxy.
    $tableRestProxy = ServicesBuilder::getInstance()->createTableService($connectionString);

    $options = new QueryEntitiesOptions();
    $options->addSelectField("Description");

    try {
        $result = $tableRestProxy->queryEntities("mytable", $options);
    }
    catch(ServiceException $e){
        // Handle exception based on error codes and messages.
        // Error codes and messages are here:
        // http://msdn.microsoft.com/library/azure/dd179438.aspx
        $code = $e->getCode();
        $error_message = $e->getMessage();
        echo $code.": ".$error_message."<br />";
    }

    // All entities in the table are returned, regardless of whether
    // they have the Description field.
    // To limit the results returned, use a filter.
    $entities = $result->getEntities();

    foreach($entities as $entity){
        $description = $entity->getProperty("Description")->getValue();
        echo $description."<br />";
    }

## <a name="update-an-entity"></a>Aktualizace entita

Existující entitu aktualizovat lze pomocí metody **Entity -> NastavitVlastnost** a **Entity -> AddProperty vlastnosti** na entita a volání **TableRestProxy -> updateEntity** Následující příklad načte entita upravuje jednu vlastnost, odebere jiné vlastnosti a přidá novou vlastnost. Všimněte si, že odeberete vlastnost nastavením jeho hodnotu na **hodnotu null**.

    require_once 'vendor/autoload.php';

    use WindowsAzure\Common\ServicesBuilder;
    use MicrosoftAzure\Storage\Common\ServiceException;
    use MicrosoftAzure\Storage\Table\Models\Entity;
    use MicrosoftAzure\Storage\Table\Models\EdmType;

    // Create table REST proxy.
    $tableRestProxy = ServicesBuilder::getInstance()->createTableService($connectionString);

    $result = $tableRestProxy->getEntity("mytable", "tasksSeattle", 1);

    $entity = $result->getEntity();

    $entity->setPropertyValue("DueDate", new DateTime()); //Modified DueDate.

    $entity->setPropertyValue("Location", null); //Removed Location.

    $entity->addProperty("Status", EdmType::STRING, "In progress"); //Added Status.

    try {
        $tableRestProxy->updateEntity("mytable", $entity);
    }
    catch(ServiceException $e){
        // Handle exception based on error codes and messages.
        // Error codes and messages are here:
        // http://msdn.microsoft.com/library/azure/dd179438.aspx
        $code = $e->getCode();
        $error_message = $e->getMessage();
        echo $code.": ".$error_message."<br />";
    }

## <a name="delete-an-entity"></a>Odstranění entity

Pokud chcete odstranit entitu, předávání názvu tabulky a subjektu `PartitionKey` a `RowKey` metody **TableRestProxy -> deleteEntity** .

    require_once 'vendor/autoload.php';

    use WindowsAzure\Common\ServicesBuilder;
    use MicrosoftAzure\Storage\Common\ServiceException;

    // Create table REST proxy.
    $tableRestProxy = ServicesBuilder::getInstance()->createTableService($connectionString);

    try {
        // Delete entity.
        $tableRestProxy->deleteEntity("mytable", "tasksSeattle", "2");
    }
    catch(ServiceException $e){
        // Handle exception based on error codes and messages.
        // Error codes and messages are here:
        // http://msdn.microsoft.com/library/azure/dd179438.aspx
        $code = $e->getCode();
        $error_message = $e->getMessage();
        echo $code.": ".$error_message."<br />";
    }

Všimněte si, že souběžné kontroly, můžete nastavit Etag entity odstraněno pomocí metody **DeleteEntityOptions -> setEtag** a předejte objekt **DeleteEntityOptions** do **deleteEntity** jako čtvrtý parametr.

## <a name="batch-table-operations"></a>Operace s tabulkou dávku

Metoda **TableRestProxy -> dávku** umožňuje provádět více operací v jednom požadavku. Tady vzorek zahrnuje přidáním operace **BatchRequest** objektu a potom předejte objekt **BatchRequest** metody **TableRestProxy -> dávku** . Můžete přidat operaci **BatchRequest** objektu, můžete některou z následujících metod víckrát voláte:

* **addInsertEntity** (přidá operaci insertEntity)
* **addUpdateEntity** (přidá operaci updateEntity)
* **addMergeEntity** (přidá operace mergeEntity)
* **addInsertOrReplaceEntity** (přidá operaci insertOrReplaceEntity)
* **addInsertOrMergeEntity** (přidá operaci insertOrMergeEntity)
* **addDeleteEntity** (přidá operace deleteEntity)

Následující příklad ukazuje, jak provádět operace **insertEntity** a **deleteEntity** v jednom požadavku:

    require_once 'vendor/autoload.php';

    use WindowsAzure\Common\ServicesBuilder;
    use MicrosoftAzure\Storage\Common\ServiceException;
    use MicrosoftAzure\Storage\Table\Models\Entity;
    use MicrosoftAzure\Storage\Table\Models\EdmType;
    use MicrosoftAzure\Storage\Table\Models\BatchOperations;

    // Create table REST proxy.
    $tableRestProxy = ServicesBuilder::getInstance()->createTableService($connectionString);

    // Create list of batch operation.
    $operations = new BatchOperations();

    $entity1 = new Entity();
    $entity1->setPartitionKey("tasksSeattle");
    $entity1->setRowKey("2");
    $entity1->addProperty("Description", null, "Clean roof gutters.");
    $entity1->addProperty("DueDate",
                          EdmType::DATETIME,
                          new DateTime("2012-11-05T08:15:00-08:00"));
    $entity1->addProperty("Location", EdmType::STRING, "Home");

    // Add operation to list of batch operations.
    $operations->addInsertEntity("mytable", $entity1);

    // Add operation to list of batch operations.
    $operations->addDeleteEntity("mytable", "tasksSeattle", "1");

    try {
        $tableRestProxy->batch($operations);
    }
    catch(ServiceException $e){
        // Handle exception based on error codes and messages.
        // Error codes and messages are here:
        // http://msdn.microsoft.com/library/azure/dd179438.aspx
        $code = $e->getCode();
        $error_message = $e->getMessage();
        echo $code.": ".$error_message."<br />";
    }

Další informace o dávky operace s tabulkou, najdete v článku [Provádění transakce skupiny Entity][entity-group-transactions].

## <a name="delete-a-table"></a>Odstranění tabulky

Nakonec pro odstranění tabulky, předáte název tabulky metodu **TableRestProxy -> deleteTable** .

    require_once 'vendor/autoload.php';

    use WindowsAzure\Common\ServicesBuilder;
    use MicrosoftAzure\Storage\Common\ServiceException;

    // Create table REST proxy.
    $tableRestProxy = ServicesBuilder::getInstance()->createTableService($connectionString);

    try {
        // Delete table.
        $tableRestProxy->deleteTable("mytable");
    }
    catch(ServiceException $e){
        // Handle exception based on error codes and messages.
        // Error codes and messages are here:
        // http://msdn.microsoft.com/library/azure/dd179438.aspx
        $code = $e->getCode();
        $error_message = $e->getMessage();
        echo $code.": ".$error_message."<br />";
    }

## <a name="next-steps"></a>Další kroky

Teď, když jste se naučili základy služby Azure tabulka, tyto odkazy vedou na další informace o složitější úlohy úložiště.

- Navštivte [blog týmu úložišti Azure](http://blogs.msdn.com/b/windowsazurestorage/)

Další informace najdete v tématu taky [Středisko pro vývojáře PHP](/develop/php/).

[download]: http://go.microsoft.com/fwlink/?LinkID=252473
[require_once]: http://php.net/require_once
[table-service-timeouts]: http://msdn.microsoft.com/library/azure/dd894042.aspx

[table-data-model]: http://msdn.microsoft.com/library/azure/dd179338.aspx
[filters]: http://msdn.microsoft.com/library/azure/dd894031.aspx
[entity-group-transactions]: http://msdn.microsoft.com/library/azure/dd894038.aspx
