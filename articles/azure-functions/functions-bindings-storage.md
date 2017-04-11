<properties
    pageTitle="Azure aktivace funkcí a vazby Azure úložištěm | Microsoft Azure"
    description="Pochopte, jak pomocí aktivačních událostí Azure úložiště a vazby ve funkcích Azure."
    services="functions"
    documentationCenter="na"
    authors="christopheranderson"
    manager="erikre"
    editor=""
    tags=""
    keywords="Azure funguje, funkce a zpracování události, dynamické výpočetním bez serveru architektura"/>

<tags
    ms.service="functions"
    ms.devlang="multiple"
    ms.topic="reference"
    ms.tgt_pltfrm="multiple"
    ms.workload="na"
    ms.date="08/22/2016"
    ms.author="chrande"/>

# <a name="azure-functions-triggers-and-bindings-for-azure-storage"></a>Azure aktivace funkcí a vazby úložištěm Azure

[AZURE.INCLUDE [functions-selector-bindings](../../includes/functions-selector-bindings.md)]

Tento článek vysvětluje, jak nakonfigurovat a kód úložišti Azure aktivačních událostí a vazby ve funkcích Azure. 

[AZURE.INCLUDE [intro](../../includes/functions-bindings-intro.md)] 

## <a id="storagequeuetrigger"></a>Aktivace fronty Azure úložiště

#### <a name="functionjson-for-storage-queue-trigger"></a>Function.JSON aktivační události fronty úložiště

Soubor *function.json* udává následující vlastnosti.

- `name`: Proměnná Název použitý v kódu funkce fronty nebo fronty zprávy. 
- `queueName`: Název fronty tak, aby hlasování. Pravidla pojmenování fronty najdete v článku [pojmenování fronty a Metadata](https://msdn.microsoft.com/library/dd179349.aspx).
- `connection`: Název aplikace nastavení, která obsahuje připojovací řetězec úložiště. Pokud necháte `connection` prázdná, aktivační událost fungovat s výchozí úložiště připojovací řetězec pro aplikaci funkce, které nastavil AzureWebJobsStorage nastavení aplikace.
- `type`: Musíte nastavit *queueTrigger*.
- `direction`: Musíte nastavit *v*. 

Příklad *function.json* aktivační události fronty úložiště:

```json
{
    "disabled": false,
    "bindings": [
        {
            "name": "myQueueItem",
            "queueName": "myqueue-items",
            "connection":"",
            "type": "queueTrigger",
            "direction": "in"
        }
    ]
}
```

#### <a name="queue-trigger-supported-types"></a>Podporované typy fronty aktivační událost

Zpráva fronty může být rekonstruováno k některým z následujících typů:

* Objekt (JSON)
* Řetězec
* Pole bajtů 
* `CloudQueueMessage`(C#) 

#### <a name="queue-trigger-metadata"></a>Metadata fronty aktivační událost

Metadata fronty ve vaší funkci dostanete pomocí těchto názvech proměnných:

* expirationTime
* insertionTime
* nextVisibleTime
* ID
* popReceipt
* dequeueCount
* queueTrigger (jiný způsob, jak získat text zprávy fronty jako řetězec)

Tento příklad kódu C# načte a protokoly fronty metadat:

```csharp
public static void Run(string myQueueItem, 
    DateTimeOffset expirationTime, 
    DateTimeOffset insertionTime, 
    DateTimeOffset nextVisibleTime,
    string queueTrigger,
    string id,
    string popReceipt,
    int dequeueCount,
    TraceWriter log)
{
    log.Info($"C# Queue trigger function processed: {myQueueItem}\n" +
        $"queueTrigger={queueTrigger}\n" +
        $"expirationTime={expirationTime}\n" +
        $"insertionTime={insertionTime}\n" +
        $"nextVisibleTime={nextVisibleTime}\n" +
        $"id={id}\n" +
        $"popReceipt={popReceipt}\n" + 
        $"dequeueCount={dequeueCount}");
}
```

#### <a name="handling-poison-queue-messages"></a>Poškozená zpracování zpráv

Zprávy, jejíž obsah způsobí, že funkce selhání nazývají *poškozená zprávy*. Při funkci nepovede, zpráva fronty není odstraněna a nakonec je převzít znovu příčinou obrázku opakovat. V SDK můžete automaticky vás nemají rušit, cyklu po omezenou počtu iterací nebo potřebujete k tomu ručně.

V SDK budou volání funkce až 5 číslem fronty zprávu zpracovat. Když na pátý vyzkoušet nepovede, zpráva se přesune do poškozená fronty.

Poškozená fronty jmenuje *{originalqueuename}*-poškozená. Můžete psát, že není potřeba funkci, kterou chcete zpracování zpráv z poškozená fronty protokolování je nebo odesláním oznámení, které ruční pozornost. 

Pokud chcete ručně zpracovat poškozená zpráv, můžete získat počet vybral zprávy pro zpracování kontrolou `dequeueCount`.

## <a id="storagequeueoutput"></a>Azure úložiště fronty Výstupní vazba

#### <a name="functionjson-for-storage-queue-output-binding"></a>Function.JSON úložiště fronty Výstupní vazba

Soubor *function.json* udává následující vlastnosti.

- `name`: Proměnná Název použitý v kódu funkce fronty nebo fronty zprávy. 
- `queueName`: Název fronty. Pravidla pojmenování fronty najdete v článku [pojmenování fronty a Metadata](https://msdn.microsoft.com/library/dd179349.aspx).
- `connection`: Název aplikace nastavení, která obsahuje připojovací řetězec úložiště. Pokud necháte `connection` prázdná, aktivační událost fungovat s výchozí úložiště připojovací řetězec pro aplikaci funkce, které nastavil AzureWebJobsStorage nastavení aplikace.
- `type`: Musíte nastavit *fronty*.
- `direction`: Musíte nastavit *se*. 

Příklad *function.json* fronty úložiště výstup vazba, který používá aktivační fronty a zapíše do fronty zprávu:

```json
{
  "bindings": [
    {
      "queueName": "myqueue-items",
      "connection": "MyStorageConnection",
      "name": "myQueueItem",
      "type": "queueTrigger",
      "direction": "in"
    },
    {
      "name": "myQueue",
      "queueName": "samples-workitems-out",
      "connection": "MyStorageConnection",
      "type": "queue",
      "direction": "out"
    }
  ],
  "disabled": false
}
``` 

#### <a name="queue-output-binding-supported-types"></a>Typ vazby výstup podporované fronty

`queue` Vazby můžete serializovat těmihle do fronty zprávy:

* Objekt (`out T` v jazyce C# vytvoří zprávu s objekt null, pokud parametr je null, když skončí platnost funkce)
* Řetězec (`out string` v jazyce C# vytvoří fronty zprávu, pokud je hodnota parametru jinou hodnotu než null po ukončení funkce)
* Pole bajtů (`out byte[]` v jazyce C# funguje jako řetězec) 
* `out CloudQueueMessage`(C#, funguje řetězec) 

V jazyce C# můžete taky svázat `ICollector<T>` nebo `IAsyncCollector<T>` kde `T` je jeden z podporovaných typů.

#### <a name="queue-output-binding-code-examples"></a>Příklady kódu vazby výstup fronty

Tento příklad kódu C# zapisuje zprávu fronty jeden výstup pro každou zprávu vstupní fronty.

```csharp
public static void Run(string myQueueItem, out string myOutputQueueItem, TraceWriter log)
{
    myOutputQueueItem = myQueueItem + "(next step)";
}
```

Tento příklad kódu C# zapíše více zpráv pomocí `ICollector<T>` (použít `IAsyncCollector<T>` ve funkci asynchronní):

```csharp
public static void Run(string myQueueItem, ICollector<string> myQueue, TraceWriter log)
{
    myQueue.Add(myQueueItem + "(step 1)");
    myQueue.Add(myQueueItem + "(step 2)");
}
```

## <a id="storageblobtrigger"></a>Azure aktivační událost úložiště objektů blob

#### <a name="functionjson-for-storage-blob-trigger"></a>Function.JSON úložiště objektů blob aktivační události

Soubor *function.json* udává následující vlastnosti.

- `name`: Proměnná Název použitý ve funkci kódu pro objektů blob. 
- `path`: Cestu, která určuje kontejneru ke sledování, případně název vzorek objektů blob.
- `connection`: Název aplikace nastavení, která obsahuje připojovací řetězec úložiště. Pokud necháte `connection` prázdná, aktivační událost fungovat s výchozí úložiště připojovací řetězec pro aplikaci funkce, které nastavil AzureWebJobsStorage nastavení aplikace.
- `type`: Musíte nastavit *blobTrigger*.
- `direction`: Musíte nastavit *v*.

Příklad *function.json* aktivační události úložiště objektů blob, která sleduje objektů BLOB přidané do kontejneru vzorky workitems:

```json
{
    "disabled": false,
    "bindings": [
        {
            "name": "myBlob",
            "type": "blobTrigger",
            "direction": "in",
            "path": "samples-workitems",
            "connection":""
        }
    ]
}
```

#### <a name="blob-trigger-supported-types"></a>Podporované typy objektů BLOB aktivační událost

Objektů blob může být rekonstruováno k některým z následujících typů ve funkcích uzel nebo C#:

* Objekt (JSON)
* Řetězec

V jazyce C# funkce můžete taky vytvořit vazbu s některou z následujících typů:

* `TextReader`
* `Stream`
* `ICloudBlob`
* `CloudBlockBlob`
* `CloudPageBlob`
* `CloudBlobContainer`
* `CloudBlobDirectory`
* `IEnumerable<CloudBlockBlob>`
* `IEnumerable<CloudPageBlob>`
* Další typy rekonstruovány [ICloudBlobStreamBinder](../app-service-web/websites-dotnet-webjobs-sdk-storage-blobs-how-to.md#icbsb) 

#### <a name="blob-trigger-c-code-example"></a>Kulatý aktivační událost C# příklad

Tento příklad kódu C# protokoly obsah každý objektů blob, která Přibyla do kontejneru sledované.

```csharp
public static void Run(string myBlob, TraceWriter log)
{
    log.Info($"C# Blob trigger function processed: {myBlob}");
}
```

#### <a name="blob-trigger-name-patterns"></a>Vzorů názvů objektů BLOB aktivační událost

Můžete zadat název vzorek objektů blob v `path` vlastnost. Příklad:

```json
"path": "input/original-{name}",
```

Tato cesta by najít objektů blob s názvem *Originál Blob1.txt* v kontejneru *zadávání* a hodnoty `name` bude proměnná v kódu funkce `Blob1`.

Jiný příklad:

```json
"path": "input/{blobname}.{blobextension}",
```

Tato cesta by také najít objektů blob s názvem *Originál Blob1.txt*a hodnotu `blobname` a `blobextension` proměnných v kódu funkce bude *Originál Blob1* a *txt*.

Je možné omezit typy objektů BLOB, které aktivovat funkci zadáním vzorek s pevnou hodnotu pro přípony souboru. Pokud nastavíte `path` *k/vzorky / {název} .png*, bude spouštět jenom *.png* objektů BLOB v kontejneru *vzorky* tuto funkci.

Pokud je třeba zadat název vzor pro názvy objektů blob, které mají složené závorky v názvu, dvojité složené závorky. Například, pokud chcete najít objektů BLOB v kontejneru *obrázky* , které mají názvy takto:

        {20140101}-soundfile.mp3

používá `path` vlastnosti:

        images/{{20140101}}-{name}

V tomto příkladu `name` hodnotu proměnné bude *soundfile.mp3*. 

#### <a name="blob-receipts"></a>Potvrzení objektů BLOB

Runtime modul Azure funkce zajišťuje, že žádná funkce aktivační událost objektů blob získá s názvem víckrát pro stejné objektů blob nový nebo aktualizovaný. Je to tak, že správa *objektů blob příjmy* abyste mohli určit, zda byla zpracována verze dané objektů blob.

Objektů BLOB příjmy jsou uložené v kontejneru s názvem *azure webjobs hosts* v okně účet Azure úložiště nastavil AzureWebJobsStorage připojovací řetězec. Oznámení o objektů blob obsahuje následující informace:

* Funkce, která označovalo jako pro objektů blob ("*{název funkce aplikace}*. Funkce. *{název funkce}*", například:"functionsf74b96f7. Functions.CopyBlob")
* Jméno container
* Typ objektů blob ("BlockBlob" nebo "PageBlob")
* Název objektů blob
* ETag (identifikátor verze objektů blob, například: "0x8D1DC6E70A277EF")

Pokud chcete vynutit opakovaného zpracování z objektů blob, můžete ručně odstranit objektů blob potvrzení pro tuto objektů blob *azure webjobs hosts* kontejneru.

#### <a name="handling-poison-blobs"></a>Zpracování poškozená objekty BLOB

Selhání aktivační funkce objektů blob SDK hovorů ho znovu, v případě, že selhání způsobil přechodná chyby. Pokud chyba způsobená obsahu objektů blob, funkce se nezdaří pokaždé, když se pokusí ke zpracování objektů blob. Ve výchozím nastavení v SDK hovorů funkci až 5 číslem pro dané objektů blob. Když na pátý vyzkoušet nepovede, SDK přidá zprávu do fronty s názvem *webjobs blobtrigger poison*.

Zpráva fronty pro poškozená objekty BLOB je JSON objekt, který obsahuje následující vlastnosti:

* FunctionId (ve formátu *{název funkce aplikace}*. Funkce. *{název funkce}*)
* BlobType ("BlockBlob" nebo "PageBlob")
* Název_kontejneru
* BlobName
* ETag (identifikátor verze objektů blob, například: "0x8D1DC6E70A277EF")

#### <a name="blob-polling-for-large-containers"></a>Kulatý dotazování velké kontejnerů

Pokud v kontejneru objektů blob sledování aktivační událost obsahuje víc než 10 000 objektů BLOB, prohledávání za běhu funkce soubory sledovat nové nebo změněné objektů BLOB protokolu. Tento proces není v reálném čase; funkce nemusí získat spouštěný do několika minut delších než po vytvoření objektů blob. Navíc [úložiště, které protokoly vytvořené ve "maximální úsilí"](https://msdn.microsoft.com/library/azure/hh343262.aspx) základna; je možné, že budou zachyceny všechny události. Za určitých podmínek propásli protokoly. Pokud rychlost a spolehlivost omezení objektů blob aktivační události pro velké kontejnery nejsou přípustné aplikace, doporučujeme vytvořit fronty zprávu při vytvoření objektů blob a použijte aktivační událost fronty místo aktivační objektů blob zpracuje objektů blob.
 
## <a id="storageblobbindings"></a>Úložiště objektů blob Azure vstupní a výstupní vazby

#### <a name="functionjson-for-a-storage-blob-input-or-output-binding"></a>Function.JSON úložiště objektů blob vstupní a výstupní vazba

Soubor *function.json* udává následující vlastnosti.

- `name`: Proměnná Název použitý ve funkci kódu pro objektů blob. 
- `path`: Cestu, která určuje kontejneru objektů blob z načtení nebo zápisu dat objektů blob k, případně název vzorek objektů blob.
- `connection`: Název aplikace nastavení, která obsahuje připojovací řetězec úložiště. Pokud necháte `connection` prázdná, vazby fungovat s výchozí úložiště připojovací řetězec pro aplikaci funkce, které nastavil AzureWebJobsStorage nastavení aplikace.
- `type`: Musíte nastavit *objektů blob*.
- `direction`: Nastavte *či *oddálení** . 

Příklad *function.json* úložiště objektů blob vstupní nebo výstupní vazba, kopírování objektů blob pomocí aktivační události fronty:

```json
{
  "bindings": [
    {
      "queueName": "myqueue-items",
      "connection": "MyStorageConnection",
      "name": "myQueueItem",
      "type": "queueTrigger",
      "direction": "in"
    },
    {
      "name": "myInputBlob",
      "type": "blob",
      "path": "samples-workitems/{queueTrigger}",
      "connection": "MyStorageConnection",
      "direction": "in"
    },
    {
      "name": "myOutputBlob",
      "type": "blob",
      "path": "samples-workitems/{queueTrigger}-Copy",
      "connection": "MyStorageConnection",
      "direction": "out"
    }
  ],
  "disabled": false
}
``` 

#### <a name="blob-input-and-output-supported-types"></a>Vstupní a výstupní podporované typy objektů BLOB

`blob` Vazba můžete serializaci nebo deserializaci následující typy funkcí Node.js a C#:

* Objekt (`out T` v jazyce C# pro výstup blob: vytvoří objektů blob jako objekt null, pokud parametr hodnotu null po ukončení funkce)
* Řetězec (`out string` v jazyce C# pro výstup blob: vytvoří objektů blob pouze v případě parametr řetězec jinou hodnotu než null až vrátí hodnotu funkce)

V jazyce C# funkcí můžete také navázat k těmto typům:

* `TextReader`(pouze pro vstup)
* `TextWriter`(pouze výstup)
* `Stream`
* `CloudBlobStream`(pouze výstup)
* `ICloudBlob`
* `CloudBlockBlob` 
* `CloudPageBlob` 

#### <a name="blob-output-c-code-example"></a>Výstup objektů BLOB C# příklad kódu

Tento příklad kódu C# slouží ke kopírování objektů blob jmenoval přijetí frontě zprávy.

```csharp
public static void Run(string myQueueItem, string myInputBlob, out string myOutputBlob, TraceWriter log)
{
    log.Info($"C# Queue trigger function processed: {myQueueItem}");
    myOutputBlob = myInputBlob;
}
```

## <a id="storagetablesbindings"></a>Azure úložiště tabulek vstupní a výstupní vazby

#### <a name="functionjson-for-storage-tables"></a>Function.JSON úložiště tabulek

*Function.json* určuje následující vlastnosti.

- `name`: Proměnná Název použitý ve funkci kódu pro Vazba tabulky. 
- `tableName`: Název tabulky.
- `partitionKey`a `rowKey` : použít společně číst jedna entita ve funkci C# nebo uzel nebo zapisovat jedna entita ve funkci uzel.
- `take`: Maximální počet řádků, které chcete číst tabulka ve funkci uzel při zadávání.
- `filter`: Výraz filtru OData pro tabulka ve funkci uzel při zadávání.
- `connection`: Název aplikace nastavení, která obsahuje připojovací řetězec úložiště. Pokud necháte `connection` prázdná, vazby fungovat s výchozí úložiště připojovací řetězec pro aplikaci funkce, které nastavil AzureWebJobsStorage nastavení aplikace.
- `type`: Musí být nastavena na *tabulky*.
- `direction`: Nastavte *či *oddálení** . 

Následující příklad *function.json* používá fronty aktivační události pro čtení řádek jednu tabulku. Ve formátu JSON obsahuje hodnoty klíče pevně oddíl a určuje klávesu řádek pochází fronty zprávy.

```json
{
  "bindings": [
    {
      "queueName": "myqueue-items",
      "connection": "MyStorageConnection",
      "name": "myQueueItem",
      "type": "queueTrigger",
      "direction": "in"
    },
    {
      "name": "personEntity",
      "type": "table",
      "tableName": "Person",
      "partitionKey": "Test",
      "rowKey": "{queueTrigger}",
      "connection": "MyStorageConnection",
      "direction": "in"
    }
  ],
  "disabled": false
}
```

#### <a name="storage-tables-input-and-output-supported-types"></a>Úložiště tabulek vstupní a výstupní podporované typy

`table` Vazby můžete serializaci nebo deserializaci objekty ve funkcích Node.js nebo C#. Objekty mají RowKey a PartitionKey vlastnosti. 

V jazyce C# funkcí můžete taky svážete k těmto typům:

* `T`kde `T` implementuje`ITableEntity`
* `IQueryable<T>`(pouze pro vstup)
* `ICollector<T>`(pouze výstup)
* `IAsyncCollector<T>`(pouze výstup)

#### <a name="storage-tables-binding-scenarios"></a>Úložiště tabulek scénáře vazeb

Vazba tabulky podporuje následujících situacích:

* Přečtěte si do jednoho řádku ve funkci C# nebo uzel.

    Nastavení `partitionKey` a `rowKey`. `filter` a `take` vlastnosti nepoužívaná v tomto scénáři.

* Přečtěte si víc řádků ve funkci C#.

    Poskytuje funkce runtime `IQueryable<T>` objekt vázaný na tabulku. Typ `T` pocházet z `TableEntity` nebo implementaci `ITableEntity`. `partitionKey`, `rowKey`, `filter`, A `take` vlastnosti nepoužívaná v tomto scénáři můžete použít `IQueryable` objekt filtrování povinné. 

* Přečtěte si víc řádků ve funkci uzel.

    Nastavit `filter` a `take` vlastnosti. Není nastavena `partitionKey` nebo `rowKey`.

* Napište jeden nebo více řádků C# (funkce).

    Poskytuje funkce runtime `ICollector<T>` nebo `IAsyncCollector<T>` vázaný na tabulku, kde `T` určuje schématu osob, které chcete přidat. Obvykle zadejte `T` je odvozena z `TableEntity` nebo implementuje `ITableEntity`, ale není potřeba. `partitionKey`, `rowKey`, `filter`, A `take` vlastnosti nepoužívaná v tomto scénáři.

#### <a name="storage-tables-example-read-a-single-table-entity-in-c-or-node"></a>Úložiště tabulek příklad: Číst entity jednu tabulku v jazyce C# nebo uzel

Následující příklad kódu C# pracuje s předchozím *function.json* soubor uvedené výše číst entity jednu tabulku. Zpráva fronty obsahuje hodnoty klíče řádku a entity tabulky čte na typ, která je definovaná v souboru *run.csx* . Včetně typu `PartitionKey` a `RowKey` vlastnosti a není odvozeno z `TableEntity`. 

```csharp
public static void Run(string myQueueItem, Person personEntity, TraceWriter log)
{
    log.Info($"C# Queue trigger function processed: {myQueueItem}");
    log.Info($"Name in Person entity: {personEntity.Name}");
}

public class Person
{
    public string PartitionKey { get; set; }
    public string RowKey { get; set; }
    public string Name { get; set; }
}
```

Následující příklad kódu F # taky označené jako pracuje s předchozím *function.json* soubor zobrazíte entity jednu tabulku.

```fsharp
[<CLIMutable>]
type Person = {
  PartitionKey: string
  RowKey: string
  Name: string
}

let Run(myQueueItem: string, personEntity: Person) =
    log.Info(sprintf "F# Queue trigger function processed: %s" myQueueItem)
    log.Info(sprintf "Name in Person entity: %s" personEntity.Name)
```

Následující příklad uzel funguje taky s předchozím *function.json* soubor zobrazíte entity jednu tabulku.

```javascript
module.exports = function (context, myQueueItem) {
    context.log('Node.js queue trigger function processed work item', myQueueItem);
    context.log('Person entity name: ' + context.bindings.personEntity.Name);
    context.done();
};
```

#### <a name="storage-tables-example-read-multiple-table-entities-in-c"></a>Úložiště tabulek příklad: Přečtěte si víc entity tabulky v C# 

Následující *function.json* a C# kód příklad čtení entity klíč oddílu zadané ve frontě zprávě.

```json
{
  "bindings": [
    {
      "queueName": "myqueue-items",
      "connection": "MyStorageConnection",
      "name": "myQueueItem",
      "type": "queueTrigger",
      "direction": "in"
    },
    {
      "name": "tableBinding",
      "type": "table",
      "connection": "MyStorageConnection",
      "tableName": "Person",
      "direction": "in"
    }
  ],
  "disabled": false
}
```

Kód C# přidá odkaz na úložiště SDK Azure tak, aby typ entity můžete jsou odvozeny od `TableEntity`.

```csharp
#r "Microsoft.WindowsAzure.Storage"
using Microsoft.WindowsAzure.Storage.Table;

public static void Run(string myQueueItem, IQueryable<Person> tableBinding, TraceWriter log)
{
    log.Info($"C# Queue trigger function processed: {myQueueItem}");
    foreach (Person person in tableBinding.Where(p => p.PartitionKey == myQueueItem).ToList())
    {
        log.Info($"Name: {person.Name}");
    }
}

public class Person : TableEntity
{
    public string Name { get; set; }
}
``` 

#### <a name="storage-tables-example-create-table-entities-in-c"></a>Úložiště tabulek příklad: vytvoření tabulky entity v C# 

*Function.json* a *run.csx* tento příklad ukazuje, jak psát entity tabulky v jazyce C#.

```json
{
  "bindings": [
    {
      "name": "input",
      "type": "manualTrigger",
      "direction": "in"
    },
    {
      "tableName": "Person",
      "connection": "MyStorageConnection",
      "name": "tableBinding",
      "type": "table",
      "direction": "out"
    }
  ],
  "disabled": false
}
```

```csharp
public static void Run(string input, ICollector<Person> tableBinding, TraceWriter log)
{
    for (int i = 1; i < 10; i++)
        {
            log.Info($"Adding Person entity {i}");
            tableBinding.Add(
                new Person() { 
                    PartitionKey = "Test", 
                    RowKey = i.ToString(), 
                    Name = "Name" + i.ToString() }
                );
        }

}

public class Person
{
    public string PartitionKey { get; set; }
    public string RowKey { get; set; }
    public string Name { get; set; }
}

```

#### <a name="storage-tables-example-create-table-entities-in-f"></a>Úložiště tabulek příklad: vytvoření tabulky entity v F#

*Function.json* a *run.fsx* tento příklad ukazuje, jak psát entity tabulky v F #.

```json
{
  "bindings": [
    {
      "name": "input",
      "type": "manualTrigger",
      "direction": "in"
    },
    {
      "tableName": "Person",
      "connection": "MyStorageConnection",
      "name": "tableBinding",
      "type": "table",
      "direction": "out"
    }
  ],
  "disabled": false
}
```

```fsharp
[<CLIMutable>]
type Person = {
  PartitionKey: string
  RowKey: string
  Name: string
}

let Run(input: string, tableBinding: ICollector<Person>, log: TraceWriter) =
    for i = 1 to 10 do
        log.Info(sprintf "Adding Person entity %d" i)
        tableBinding.Add(
            { PartitionKey = "Test"
              RowKey = i.ToString()
              Name = "Name" + i.ToString() })
```

#### <a name="storage-tables-example-create-a-table-entity-in-node"></a>Úložiště tabulek příklad: vytvoření tabulky entity v uzel

*Function.json* a *run.csx* tento příklad ukazuje, jak psát entity tabulky v uzel.

```json
{
  "bindings": [
    {
      "queueName": "myqueue-items",
      "connection": "MyStorageConnection",
      "name": "myQueueItem",
      "type": "queueTrigger",
      "direction": "in"
    },
    {
      "tableName": "Person",
      "partitionKey": "Test",
      "rowKey": "{queueTrigger}",
      "connection": "MyStorageConnection",
      "name": "personEntity",
      "type": "table",
      "direction": "out"
    }
  ],
  "disabled": true
}
```

```javascript
module.exports = function (context, myQueueItem) {
    context.log('Node.js queue trigger function processed work item', myQueueItem);
    context.bindings.personEntity = {"Name": "Name" + myQueueItem }
    context.done();
};
```

## <a name="next-steps"></a>Další kroky

[AZURE.INCLUDE [next steps](../../includes/functions-bindings-next-steps.md)] 
