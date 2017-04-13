<properties
    pageTitle="Použití úložiště tabulek Azure z Node.js | Microsoft Azure"
    description="Obsahují strukturovaných dat v cloudu pomocí úložiště tabulek Azure, úložiště dat NoSQL."
    services="storage"
    documentationCenter="nodejs"
    authors="tamram"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="nodejs"
    ms.topic="article"
    ms.date="08/11/2016"
    ms.author="tamram"/>


# <a name="how-to-use-azure-table-storage-from-nodejs"></a>Použití úložiště tabulek Azure z Node.js

[AZURE.INCLUDE [storage-selector-table-include](../../includes/storage-selector-table-include.md)]
<br/>
[AZURE.INCLUDE [storage-try-azure-tools-queues](../../includes/storage-try-azure-tools-tables.md)]

## <a name="overview"></a>Základní informace

Toto téma ukazuje, jak provádět běžné scénáře použití služby Azure tabulka v aplikaci Node.js.

Příklady v tomto tématu se předpokládá, že už máte Node.js aplikace. Informace o tom, jak vytvořit aplikaci Node.js v Azure najdete některé z těchto témat:

- [Vytvoření webové aplikace Node.js v aplikaci služby Azure](../app-service-web/web-sites-nodejs-develop-deploy-mac.md)
- [Vytvořte a nasaďte do Node.js webových aplikací do Azure pomocí WebMatrix](../app-service-web/web-sites-nodejs-use-webmatrix.md)
- [Vytvoření a nasazení aplikace Node.js cloudové služby Azure](../cloud-services/cloud-services-nodejs-develop-deploy-app.md) (pomocí Windows Powershellu)


[AZURE.INCLUDE [storage-table-concepts-include](../../includes/storage-table-concepts-include.md)]

[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]


## <a name="configure-your-application-to-access-azure-storage"></a>Konfigurace aplikace pro přístup k úložišti Azure

Použít úložišti Azure, musíte SDK úložiště Azure pro Node.js, který obsahuje sadu pohodlí knihoven, které komunikaci se službami ZBÝVAJÍCÍ úložiště.

### <a name="use-node-package-manager-npm-to-install-the-package"></a>Použití uzel balíčku správce (NPM) k instalaci balíčku

1.  Pomocí příkazového řádku rozhraní, jako je **prostředí PowerShell** (Windows), **terminálu** (Mac) nebo **flám** (Unix) a přejděte do složky, kde jste aplikaci vytvořili.

2.  Do příkazového řádku zadejte **npm instalace úložišti azure** . Výstup příkazu probíhá podobně jako v následujícím příkladu.

        azure-storage@0.5.0 node_modules\azure-storage
        +-- extend@1.2.1
        +-- xmlbuilder@0.4.3
        +-- mime@1.2.11
        +-- node-uuid@1.4.3
        +-- validator@3.22.2
        +-- underscore@1.4.4
        +-- readable-stream@1.0.33 (string_decoder@0.10.31, isarray@0.0.1, inherits@2.0.1, core-util-is@1.0.1)
        +-- xml2js@0.2.7 (sax@0.5.2)
        +-- request@2.57.0 (caseless@0.10.0, aws-sign2@0.5.0, forever-agent@0.6.1, stringstream@0.0.4, oauth-sign@0.8.0, tunnel-agent@0.4.1, isstream@0.1.2, json-stringify-safe@5.0.1, bl@0.9.4, combined-stream@1.0.5, qs@3.1.0, mime-types@2.0.14, form-data@0.2.0, http-signature@0.11.0, tough-cookie@2.0.0, hawk@2.3.1, har-validator@1.8.0)

3.  Můžete ručně spusťte příkaz **ls** ověřte, že **uzel\_moduly** složky byl vytvořen. V této složce zjistíte balíček **úložišti azure** , která obsahuje knihovnu potřebujete získat přístup k úložišti.

### <a name="import-the-package"></a>Import balíčku

Přidejte následující kód do horní části souboru **server.js** v aplikaci:

    var azure = require('azure-storage');

## <a name="set-up-an-azure-storage-connection"></a>Nastavení připojení k úložišti Azure

Modul azure přečte proměnné AZURE\_úložiště\_účet a AZURE\_úložiště\_přístup\_klíč nebo AZURE\_úložiště\_připojení\_řetězec informace potřebné pro připojení ke svému účtu Azure úložiště. Pokud nejsou tyto proměnné, je třeba určit informace o účtu při volání **TableService**.

Příklad nastavení proměnné prostředí [Azure portál](https://portal.azure.com) Azure webu najdete v článku [Node.js webové aplikace pomocí služby Azure tabulky].

## <a name="create-a-table"></a>Vytvoření tabulky

Následující kód vytvoří objekt **TableService** a používá k vytvoření nové tabulky. Přidejte následující v horní části **server.js**.

    var tableSvc = azure.createTableService();

Volání **createTableIfNotExists** vytvoří novou tabulku se zadaným názvem, pokud už neexistuje. Následující příklad vytvoří nová tabulka s názvem "tabulka", pokud už neexistuje:

    tableSvc.createTableIfNotExists('mytable', function(error, result, response){
      if(!error){
        // Table exists or created
      }
    });

`result.created` Bude `true` Pokud vytvořena nová tabulka a `false` Pokud tabulka již existuje. `response` Bude obsahovat informace o žádosti.

### <a name="filters"></a>Filtry

Volitelné filtrování operace se dají použít k operací pomocí **TableService**. Filtrování operace může obsahovat protokolování automaticky opakování, atd. Filtry jsou objekty, které implementovat metodu k podpisu:

    function handle (requestOptions, next)

Po provedení předzpracování na žádost o možnostech, metodu potřebuje volat "pak" předávání zpětné podpisem následující:

    function (returnObject, finalCallback, next)

V tomto zpětné a po zpracování returnObject (odpovědi žádosti o na serveru) musí zpětné vyvolat dál, pokud existuje pokračovat ve zpracování další filtry nebo vyvolat finalCallback jednoduše jinak ukončit volání služby.

Dva filtry, které implementace logiky opakovat jsou součástí SDK Azure Node.js, **ExponentialRetryPolicyFilter** a **LinearRetryPolicyFilter**. Následujícím příkladu je vytvořen **TableService** objekt, který používá **ExponentialRetryPolicyFilter**:

    var retryOperations = new azure.ExponentialRetryPolicyFilter();
    var tableSvc = azure.createTableService().withFilter(retryOperations);

## <a name="add-an-entity-to-a-table"></a>Přidání entita do tabulky

Přidat entitu, nejprve vytvořte objekt, který definuje vlastnosti entity. Všechny osoby musí obsahovat **PartitionKey** a **RowKey**, které jsou jedinečné identifikátory entity.

* **PartitionKey** - určuje entitu uložený v oddílu

* **RowKey** - jednoznačně identifikuje entity v oddílu

Řetězcové hodnoty musí být **PartitionKey** a **RowKey** . Další informace najdete v článku [Principy datového modelu tabulku služba](http://msdn.microsoft.com/library/azure/dd179338.aspx).

Následující obrázek je příkladem definování entity. Poznámka: Tento **Příklad** je definována jako typu **Edm.DateTime**. Zadejte vynechán, a typy bude mít odvozeny, pokud není zadána.

    var task = {
      PartitionKey: {'_':'hometasks'},
      RowKey: {'_': '1'},
      description: {'_':'take out the trash'},
      dueDate: {'_':new Date(2015, 6, 20), '$':'Edm.DateTime'}
    };

> [AZURE.NOTE] Je také pole **časového razítka** pro každý záznam, který je nastaven tak, že Azure entita se vložené nebo aktualizovala.

**EntityGenerator** můžete taky vytvořit entity. Následující příklad vytvoří stejný subjekt úkolu pomocí **entityGenerator**.

    var entGen = azure.TableUtilities.entityGenerator;
    var task = {
      PartitionKey: entGen.String('hometasks'),
      RowKey: entGen.String('1'),
      description: entGen.String('take out the trash'),
      dueDate: entGen.DateTime(new Date(Date.UTC(2015, 6, 20))),
    };

Entita do tabulky přidáte předáte metodu **insertEntity** objektu entity.

    tableSvc.insertEntity('mytable',task, function (error, result, response) {
      if(!error){
        // Entity inserted
      }
    });

Pokud je operace úspěšná, `result` bude obsahovat [ETag](http://en.wikipedia.org/wiki/HTTP_ETag) vloženého záznamu a `response` bude obsahovat informace o funkci.

Příklad odpověď:

    { '.metadata': { etag: 'W/"datetime\'2015-02-25T01%3A22%3A22.5Z\'"' } }

> [AZURE.NOTE] Ve výchozím nastavení **insertEntity** nevrací vložený entity jako součást `response` informace. Pokud chtít provádění dalších operací na tuto entitu nebo chcete informací do mezipaměti, může být užitečné ji vrácené jako součást `result`. Povolením **echoContent** můžete udělat toto:
>
> `tableSvc.insertEntity('mytable', task, {echoContent: true}, function (error, result, response) {...}`

## <a name="update-an-entity"></a>Aktualizace entita

Způsoby více dostupné aktualizace existující entitu:

* **replaceEntity** - aktualizuje existující entitu nahrazením

* **mergeEntity** - aktualizuje existující entitu sloučením nové nemovitostí s hodnotou do existující entitu

* **insertOrReplaceEntity** - aktualizuje existující entitu nahrazením. Pokud existuje žádná entita se vloží nový

* **insertOrMergeEntity** - aktualizuje existující entitu stávající sloučení nový nemovitostí s hodnotou. Pokud existuje žádná entita se vloží nový

Následující příklad ukazuje aktualizace entita pomocí **replaceEntity**:

    tableSvc.replaceEntity('mytable', updatedTask, function(error, result, response){
      if(!error) {
        // Entity updated
      }
    });

> [AZURE.NOTE] Ve výchozím nastavení aktualizace entita nekontroluje zobrazíte-li data aktualizaci dříve upraven jiným procesem. K podpoře současných aktualizací:
>
> 1. Pokud potřebujete ETag objekt je aktualizován. To je vrácena jako součást `response` pro každý subjekt související operace a můžete získat prostřednictvím `response['.metadata'].etag`.
>
> 2. Při provádění operace aktualizace na entitu, přidejte informace ETag dříve načíst nový subjekt. Příklad:
>
>     `entity2['.metadata'].etag = currentEtag;`
>
> 3. Proveďte operace aktualizace. Pokud entitu upraven od získané hodnoty ETag, například jiné instanci aplikace, `error` , bude vrácena informacemi o tom, že aktualizace uvedené v žádosti o nebyl splněny.

**ReplaceEntity** a **mergeEntity**Pokud osoba, která je aktualizován neexistuje, pak operace aktualizace se nepovede. Proto pokud chcete ukládat entita bez ohledu na to jestli už existuje, použijte **insertOrReplaceEntity** nebo **insertOrMergeEntity**.

`result` Pro úspěšné aktualizace operace bude obsahovat **Etag** aktualizované entity.

## <a name="work-with-groups-of-entities"></a>Práce se skupinami entit

Někdy dává smysl můžou odeslat více operací pohromadě v jedné dávce zajistit atomová zpracování na serveru. Provádět, pomocí třídy **TableBatch** vytvoříte list a pak použijte metodu **executeBatch** **TableService** provádět dávkový operace.

 Následující příklad ukazuje odeslání dvě entity v listu:

    var task1 = {
      PartitionKey: {'_':'hometasks'},
      RowKey: {'_': '1'},
      description: {'_':'Take out the trash'},
      dueDate: {'_':new Date(2015, 6, 20)}
    };
    var task2 = {
      PartitionKey: {'_':'hometasks'},
      RowKey: {'_': '2'},
      description: {'_':'Wash the dishes'},
      dueDate: {'_':new Date(2015, 6, 20)}
    };

    var batch = new azure.TableBatch();

    batch.insertEntity(task1, {echoContent: true});
    batch.insertEntity(task2, {echoContent: true});

    tableSvc.executeBatch('mytable', batch, function (error, result, response) {
      if(!error) {
        // Batch completed
      }
    });

Pro úspěšné dávku operace `result` bude obsahovat informace pro každou akci na listu.

### <a name="work-with-batched-operations"></a>Práce s dávkové zpracování

Operace přidali do listu můžete zkontrolovat podle zobrazení `operations` vlastnost. Můžete také následujících metod pro práci s operace:

* **Vymazání** - vymaže všechny operace v listu

* **getOperations** - získá operaci listu

* **hasOperations** – vrátí hodnotu PRAVDA, pokud dávku obsahuje operace

* **removeOperations** - odebere operaci

* **velikost** – vrátí počet operací na listu

## <a name="retrieve-an-entity-by-key"></a>Načtení entita klíčem

Pokud chcete vrátit konkrétní entity na základě **PartitionKey** a **RowKey**, použijte metodu **retrieveEntity** .

    tableSvc.retrieveEntity('mytable', 'hometasks', '1', function(error, result, response){
      if(!error){
        // result contains the entity
      }
    });

Po dokončení operace `result` bude obsahovat entity.

## <a name="query-a-set-of-entities"></a>Dotaz sadu entity

K vytvoření dotazu tabulky, použijte objekt **TableQuery** si vybudovat výrazu dotazu pomocí následující klauzule:

* **Vyberte** - pole, která má být vrácena z dotazu

* **kde** – kde klauzule

    * **a** - `and` podmínka where

    * **nebo** - `or` podmínka where

* **horní** – počet položek, abyste vzdáleně používali


Následující příklad vytvoří dotaz, který vrátí prvních pět položek s PartitionKey "hometasks".

    var query = new azure.TableQuery()
      .top(5)
      .where('PartitionKey eq ?', 'hometasks');

Protože **Vyberte** není použit, bude vrácena všechna pole. Abyste mohli provést dotaz na tabulky, použijte **queryEntities**. Tento dotaz k vrácení entity z "tabulka" v následujícím příkladu.

    tableSvc.queryEntities('mytable',query, null, function(error, result, response) {
      if(!error) {
        // query was successful
      }
    });

Pokud je úspěšná, `result.entries` bude obsahovat maticových osob, které vyhovují dotazu. Pokud byl dotaz nelze vrátit všechny entity `result.continuationToken` bude není*null* a mohou sloužit jako parametr třetí **queryEntities** k vyhledání více výsledků. Počáteční dotazu vyberte *null* třetí parametr.

### <a name="query-a-subset-of-entity-properties"></a>Dotaz podmnožinu entity vlastností

Dotaz s tabulkou načítat pár polí z entity.
Tento postup šířky pásma a dosáhnout zvýšení výkonu dotazu, zejména u velkých entity. Použití klauzule **select** a předejte názvy polí, která má být vrácena. Například následující dotaz vrátí pouze pole **Popis** a **Příklad** .

    var query = new azure.TableQuery()
      .select(['description', 'dueDate'])
      .top(5)
      .where('PartitionKey eq ?', 'hometasks');

## <a name="delete-an-entity"></a>Odstranění entity

Můžete odstranit entita použít jeho oddílů a řádek. V tomto příkladu **task1** objekt s hodnotami **RowKey** a **PartitionKey** entity odeberou. Klikněte na objekt předána metodu **deleteEntity** .

    var task = {
      PartitionKey: {'_':'hometasks'},
      RowKey: {'_': '1'}
    };

    tableSvc.deleteEntity('mytable', task, function(error, response){
      if(!error) {
        // Entity deleted
      }
    });

> [AZURE.NOTE] Zvažte možnost ETags při odstraňování položek, ujistěte se, že položka nebyla upravena jiným procesem. Informace o použití ETags najdete v článku [aktualizace entity](#update-an-entity) .

## <a name="delete-a-table"></a>Odstranění tabulky

Následující kód odstraní tabulku z účtu úložiště.

    tableSvc.deleteTable('mytable', function(error, response){
        if(!error){
            // Table deleted
        }
    });

Pokud si nejste jisti, jestli existuje tabulky, použijte **deleteTableIfExists**.

## <a name="use-continuation-tokens"></a>Použít tokeny pokračování

Když se dotazujete tabulky pro velké množství výsledků, vyhledejte pokračování tokeny. K dispozici pro dotaz, který nemusí zjistíte, pokud není vytvořit rozpoznat token pokračování neobsahuje data může být velké objemy dat.

Výsledky objekt během dotazování entity sady vrácena `continuationToken` vlastnost když tyto token neobsahuje data. Pak můžete to při provádění dotazu nadále posunujte se přes entity oddíl a tabulky.

Při dotazu, parametr continuationToken může mezi být k dispozici instance objektu dotazu a funkci zpětné:

```
var nextContinuationToken = null;
dc.table.queryEntities(tableName,
    query,
    nextContinuationToken,
    function (error, results) {
        if (error) throw error;

        // iterate through results.entries with results

        if (results.continuationToken) {
            nextContinuationToken = results.continuationToken;
        }

    });
```

Pokud byste zkontrolovat `continuationToken` objekt, zjistíte vlastnosti jako `nextPartitionKey`, `nextRowKey` a `targetLocation`, které mohou sloužit k iteraci všechny výsledky.

Je také ukázkové pokračování v rámci repo Azure úložiště Node.js na GitHub. Vyhledejte `examples/samples/continuationsample.js`.

## <a name="work-with-shared-access-signatures"></a>Práce s sdílených přístup podpisy

Sdílený přístup podpisy (přidružení zabezpečení) jsou bezpečný způsob podrobného zpřístupnit tabulky bez poskytování název účtu úložiště nebo klíče. Přidružení zabezpečení se často používá k poskytování omezený přístup k datům, například povolení mobilní aplikaci vyhledejte záznamy.

Důvěryhodné aplikace například do cloudové služby vygeneruje přidružení zabezpečení pomocí **generateSharedAccessSignature** **TableService**a poskytne ji do aplikace nedůvěryhodných nebo částečně důvěryhodné například v mobilní aplikaci. Přidružení zabezpečení je generováno použití zásad, které popisuje počáteční a koncové datum, kdy přidružení je platný, stejně jako úroveň přístupu majiteli přidružení zabezpečení.

Následující příklad generuje nové zásady sdílený přístup, které vám umožní uchycení přidružení zabezpečení dotazu ("r") v tabulce a vypršení platnosti 100 minut po o čase, který už je vytvořená.

    var startDate = new Date();
    var expiryDate = new Date(startDate);
    expiryDate.setMinutes(startDate.getMinutes() + 100);
    startDate.setMinutes(startDate.getMinutes() - 100);

    var sharedAccessPolicy = {
      AccessPolicy: {
        Permissions: azure.TableUtilities.SharedAccessPermissions.QUERY,
        Start: startDate,
        Expiry: expiryDate
      },
    };

    var tableSAS = tableSvc.generateSharedAccessSignature('mytable', sharedAccessPolicy);
    var host = tableSvc.host;

Všimněte si, že informace Host (hostitel) je třeba zadat také, jako je vyžadován při uchycení přidružení zabezpečení pokusí o přístup k tabulce.

Klientská aplikace je pak používá přidružení zabezpečení s **TableServiceWithSAS** provádět operace v tabulce. Následující příklad připojí se k tabulce a provede dotaz.

    var sharedTableService = azure.createTableServiceWithSas(host, tableSAS);
    var query = azure.TableQuery()
      .where('PartitionKey eq ?', 'hometasks');

    sharedTableService.queryEntities(query, null, function(error, result, response) {
      if(!error) {
        // result contains the entities
      }
    });

Protože přidružení zabezpečení byl vytvořen se jenom dotazu přístup, pokud jste pokus o vložení, aktualizace nebo odstranění entity, bude vrácena chyba.

### <a name="access-control-lists"></a>Seznamy řízení přístupu

Seznam řízení přístupu (ACL) můžete taky nastavit zásady přístupu pro přidružení zabezpečení. To je užitečné, pokud chcete povolit více klientů přístupu k tabulce, ale poskytují různé přístup zásady pro každého klienta.

ACL je implementovaná maticových zásady přístupu pomocí ID přidružené k každého zásadu. Následující příklad definuje dvě zásady, jeden pro uživatel '1' a druhý pro "uživatel2":

    var sharedAccessPolicy = {
      user1: {
        Permissions: azure.TableUtilities.SharedAccessPermissions.QUERY,
        Start: startDate,
        Expiry: expiryDate
      },
      user2: {
        Permissions: azure.TableUtilities.SharedAccessPermissions.ADD,
        Start: startDate,
        Expiry: expiryDate
      }
    };

V následujícím příkladu je načtena aktuální ACL pro tabulce **hometasks** a přidá nové zásady použití **setTableAcl**. Tento přístup vám umožňuje:

    var extend = require('extend');
    tableSvc.getTableAcl('hometasks', function(error, result, response) {
    if(!error){
        var newSignedIdentifiers = extend(true, result.signedIdentifiers, sharedAccessPolicy);
        tableSvc.setTableAcl('hometasks', newSignedIdentifiers, function(error, result, response){
          if(!error){
            // ACL set
          }
        });
      }
    });

Po nastavení ACL můžete pak vytvořit přidružení zabezpečení podle ID pro zásady. Následující příklad vytvoří nová přidružení zabezpečení "uživatel2":

    tableSAS = tableSvc.generateSharedAccessSignature('hometasks', { Id: 'user2' });

## <a name="next-steps"></a>Další kroky

Další informace naleznete v následujících zdrojích.

-   [Blog týmu azure úložiště][].
-   [Azure úložiště SDK uzel][] úložiště na GitHub.
-   [Středisko pro vývojáře Node.js](/develop/nodejs/)

  [Azure úložiště SDK uzel]: https://github.com/Azure/azure-storage-node
  [OData.org]: http://www.odata.org/
  [Using the REST API]: http://msdn.microsoft.com/library/azure/hh264518.aspx
  [Azure Portal]: portal.azure.com

  [Node.js Cloud Service]: ../cloud-services-nodejs-develop-deploy-app.md
  [Blog týmu Azure úložiště]: http://blogs.msdn.com/b/windowsazurestorage/
  [Website with WebMatrix]: ../web-sites-nodejs-use-webmatrix.md
  [Node.js Cloud Service with Storage]: ../storage-nodejs-use-table-storage-cloud-service-app.md
  [Použití tabulku služba Azure Node.js web appu]: ../storage-nodejs-use-table-storage-web-site.md
  [Create and deploy a Node.js application to an Azure website]: ../web-sites-nodejs-develop-deploy-mac.md
