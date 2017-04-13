<properties
    pageTitle="Použití úložiště objektů Blob z Node.js | Microsoft Azure"
    description="Obsahují Nestrukturovaná data v cloudu pomocí úložiště objektů Blob Azure (objektu úložiště)."
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



# <a name="how-to-use-blob-storage-from-nodejs"></a>Použití úložiště objektů Blob z Node.js

[AZURE.INCLUDE [storage-selector-blob-include](../../includes/storage-selector-blob-include.md)]
<br/>
[AZURE.INCLUDE [storage-try-azure-tools-queues](../../includes/storage-try-azure-tools-blobs.md)]

## <a name="overview"></a>Základní informace

V tomto článku se dozvíte, jak provádět běžné scénáře pomocí úložiště objektů Blob. Vzorky jsou napsali prostřednictvím rozhraní API Node.js. Scénáře nichž se uplatní zahrnují nahrát, seznam, stažení a odstranění objektů BLOB.

[AZURE.INCLUDE [storage-blob-concepts-include](../../includes/storage-blob-concepts-include.md)]

[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="create-a-nodejs-application"></a>Vytvoření aplikace Node.js

Pokyny k vytvoření aplikace Node.js najdete v článku [Vytvoření Node.js web app v aplikaci služby Azure], [sestavovat a nasazovat aplikace Node.js cloudové služby Azure] – pomocí prostředí Windows PowerShell nebo [vytvořte a nasaďte Node.js web appu k Azure pomocí Web matice].

## <a name="configure-your-application-to-access-storage"></a>Konfigurace aplikace pro přístup k úložišti

Abyste mohli používat Azure úložiště, potřebujete SDK úložiště Azure pro Node.js, který obsahuje sadu pohodlí knihoven, které komunikaci se službami ZBÝVAJÍCÍ úložiště.

### <a name="use-node-package-manager-npm-to-obtain-the-package"></a>Použití uzel balíčku správce (NPM) získání balíčku

1.  Pomocí příkazového řádku rozhraní, jako je **prostředí PowerShell** (Windows), **terminálu** (Mac) nebo **flám** (Unix), přejděte do složky, které jste vytvořili ukázková aplikace.

2.  Do příkazového řádku zadejte **npm instalace úložišti azure** . Výstup příkazu probíhá podobně jako v následujícím příkladu kódu.

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

3.  Můžete ručně spusťte příkaz **ls** ověřte, že **uzel\_moduly** složky byl vytvořen. V této složce najděte balíček **úložišti azure** , která obsahuje knihoven, které potřebujete pro přístup k úložišti.

### <a name="import-the-package"></a>Import balíčku

Na začátek **server.js** souboru aplikace místo, kam chcete použít úložiště pomocí poznámkového bloku nebo jiném textovém editoru, přidejte následující:

    var azure = require('azure-storage');

## <a name="set-up-an-azure-storage-connection"></a>Nastavení připojení k úložišti Azure

Modul Azure přečte proměnné prostředí `AZURE_STORAGE_ACCOUNT` a `AZURE_STORAGE_ACCESS_KEY`, nebo `AZURE_STORAGE_CONNECTION_STRING`, informace potřebné pro připojení ke svému účtu Azure úložiště. Pokud nejsou tyto proměnné, je třeba určit informace o účtu při volání **createBlobService**.

Příklad nastavení proměnné prostředí [Azure portálu](https://portal.azure.com) pro Azure webovou aplikaci najdete v článku [Node.js webové aplikace pomocí služby Azure tabulky].

## <a name="create-a-container"></a>Vytvoření kontejneru

Objekt **BlobService** umožňuje práci s objekty BLOB a kontejnery. Následující kód vytvoří objekt **BlobService** . Přidejte následující v horní části **server.js**:

    var blobSvc = azure.createBlobService();

> [AZURE.NOTE] Dostanete objektů blob anonymně pomocí **createBlobServiceAnonymous** a poskytování adresu hostitele. Příklad použití `var blobSvc = azure.createBlobServiceAnonymous('https://myblob.blob.core.windows.net/');`.

[AZURE.INCLUDE [storage-container-naming-rules-include](../../includes/storage-container-naming-rules-include.md)]

Pokud chcete vytvořit nové kontejneru, použijte **createContainerIfNotExists**. Následující příklad vytvoří nový kontejner s názvem "mycontainer":

    blobSvc.createContainerIfNotExists('mycontainer', function(error, result, response){
        if(!error){
          // Container exists and is private
        }
    });

Pokud je kontejner nově vytvořený, `result.created` platí. Pokud již existuje kontejneru `result.created` vyhodnotí jako NEPRAVDA. `response`obsahuje informace o práci, včetně informací ETag pro kontejner.

### <a name="container-security"></a>Kontejner zabezpečení

Ve výchozím nastavení nové kontejnery jsou soukromé a nemají přístup k anonymně. Zveřejnit kontejneru tak, aby se k němu přístup anonymně, můžete nastavit úroveň přístupu kontejneru **objektů blob** nebo **kontejner**.

* **objektů blob** – umožňuje anonymní přístup pro čtení obsahu objektů blob a metadata v tomto kontejneru, ale ne na kontejner metadat například výpis všech objektů BLOB uvnitř kontejneru

* **kontejner** – umožňuje anonymní přístup pro čtení obsahu objektů blob a metadata, jakož i kontejneru metadat

Následující příklad ukazuje nastavení úroveň přístupu k **objektů blob**:

    blobSvc.createContainerIfNotExists('mycontainer', {publicAccessLevel : 'blob'}, function(error, result, response){
        if(!error){
          // Container exists and allows
          // anonymous read access to blob
          // content and metadata within this container
        }
    });

Můžete taky můžete změnit úroveň přístupu kontejneru pomocí **setContainerAcl** můžete určit úroveň přístupu. Následující příklad změní úroveň přístupu na kontejner:

    blobSvc.setContainerAcl('mycontainer', null /* signedIdentifiers */, {publicAccessLevel : 'container'} /* publicAccessLevel*/, function(error, result, response){
      if(!error){
        // Container access level set to 'container'
      }
    });

Výsledek obsahuje informace o operace, včetně aktuální **ETag** kontejneru.

### <a name="filters"></a>Filtry

Volitelné filtrování operace aplikováním na operace prováděné pomocí **BlobService**. Filtrování operace může obsahovat protokolování automaticky opakování, atd. Filtry jsou objekty, které implementovat metodu k podpisu:

    function handle (requestOptions, next)

Po provedení předzpracování na žádost o možnostech, metodu potřebuje volat "pak" předávání zpětné podpisem následující:

    function (returnObject, finalCallback, next)

V tomto zpětné a po zpracování returnObject (odpovědi žádosti o na serveru) musí zpětné vyvolat dál, pokud existuje pokračovat ve zpracování další filtry nebo jednoduše vyvolat finalCallback ukončit volání služby.

Dva filtry, které implementace logiky opakovat jsou součástí SDK Azure Node.js, **ExponentialRetryPolicyFilter** a **LinearRetryPolicyFilter**. Následujícím příkladu je vytvořen **BlobService** objekt, který používá **ExponentialRetryPolicyFilter**:

    var retryOperations = new azure.ExponentialRetryPolicyFilter();
    var blobSvc = azure.createBlobService().withFilter(retryOperations);

## <a name="upload-a-blob-into-a-container"></a>Nahrání objektů blob do kontejneru

Existují tři typy objektů blob: zablokovat objektů BLOB, stránky objektů BLOB a připojit objektů BLOB. Objekty BLOB blok povolit že na více efektivní nahrajete velký data. Přidání objektů BLOB jsou optimalizovaná pro připojení operace. Objekty BLOB stránky jsou optimalizované pro operace pro čtení i zápis. Další informace najdete v tématu [Principy blok objektů BLOB, přidat objektů BLOB a objekty BLOB stránky](http://msdn.microsoft.com/library/azure/ee691964.aspx).

### <a name="block-blobs"></a>Objekty BLOB bloku

Odeslání dat do bloku objektů blob, následujícím způsobem:

* **createBlockBlobFromLocalFile** - vytvoří nový objektů blob bloku a odešle obsah souboru

* **createBlockBlobFromStream** - vytvoří nový objektů blob bloku a odešle obsah proudu

* **createBlockBlobFromText** - vytvoří nový objektů blob bloku a odešle obsah řetězce

* **createWriteStreamToBlockBlob** - poskytuje toku zápisu do bloku objektů blob

Následující příklad odešle obsah souboru **test.txt** do **myblob**.

    blobSvc.createBlockBlobFromLocalFile('mycontainer', 'myblob', 'test.txt', function(error, result, response){
      if(!error){
        // file uploaded
      }
    });

`result` Vrácené těchto postupů obsahuje informace o operaci, například **ETag** objektů blob.

### <a name="append-blobs"></a>Přidání objektů BLOB

Odeslání dat do nových objektů blob přidávacího, následujícím způsobem:

* **createAppendBlobFromLocalFile** - vytvoří nový objektů blob připojit a odešle obsah souboru

* **createAppendBlobFromStream** - vytvoří nový objektů blob připojit a odešle obsah proudu

* **createAppendBlobFromText** - vytvoří nový objektů blob připojit a odešle obsah řetězce

* **createWriteStreamToNewAppendBlob** - vytvoří nový objektů blob připojit a pak poskytuje toku zápisu

Následující příklad odešle obsah souboru **test.txt** do **myappendblob**.

    blobSvc.createAppendBlobFromLocalFile('mycontainer', 'myappendblob', 'test.txt', function(error, result, response){
      if(!error){
        // file uploaded
      }
    });

Přidání bloku do existující objektů blob přidávacího, použijte následující:

* **appendFromLocalFile** - ke stávající objektů blob přidávacího připojit obsah souboru

* **appendFromStream** - ke stávající objektů blob přidávacího připojit obsah proudu

* **appendFromText** - ke stávající objektů blob přidávacího připojit obsah řetězce

* **appendBlockFromStream** - ke stávající objektů blob přidávacího připojit obsah proudu

* **appendBlockFromText** - ke stávající objektů blob přidávacího připojit obsah řetězce

> [AZURE.NOTE] appendFromXXX rozhraní API udělá některé ověřování na straně klienta selhání rychle Chcete-li předejít unncessary serveru volání. appendBlockFromXXX nebude.

Následující příklad odešle obsah souboru **test.txt** do **myappendblob**.

    blobSvc.appendFromText('mycontainer', 'myappendblob', 'text to be appended', function(error, result, response){
      if(!error){
        // text appended
      }
    });


### <a name="page-blobs"></a>Objekty BLOB stránky

Odeslání dat do objektů blob stránky, následujícím způsobem:

* **createPageBlob** - vytvoří novou stránku blob určité délky

* **createPageBlobFromLocalFile** - vytvoří novou stránku blob a odešle obsah souboru

* **createPageBlobFromStream** - vytvoří novou stránku blob a odešle obsah proudu

* **createWriteStreamToExistingPageBlob** - poskytuje toku zápisu do existující objektů blob stránky

* **createWriteStreamToNewPageBlob** - vytvoří nový objektů blob stránky a potom poskytuje toku zápisu

Následující příklad odešle obsah souboru **test.txt** do **mypageblob**.

    blobSvc.createPageBlobFromLocalFile('mycontainer', 'mypageblob', 'test.txt', function(error, result, response){
      if(!error){
        // file uploaded
      }
    });

> [AZURE.NOTE] Objekty BLOB stránky sestávat 512 8bajtový typ stránek. Můžete dojde k chybě při odesílání dat s velikostí, která není násobkem 512.

## <a name="list-the-blobs-in-a-container"></a>Seznam objektů BLOB v kontejneru

Seznam objektů BLOB v kontejneru, použijte metodu **listBlobsSegmented** . Pokud chcete vrátit objektů BLOB konkrétní předponou, použijte **listBlobsSegmentedWithPrefix**.

    blobSvc.listBlobsSegmented('mycontainer', null, function(error, result, response){
      if(!error){
          // result.entries contains the entries
          // If not all blobs were returned, result.continuationToken has the continuation token.
      }
    });

`result` Obsahuje `entries` kolekce, která je polem objektů, které popisují každý objektů blob. Pokud všechny objekty BLOB nemůže být vrácena, `result` obsahuje také `continuationToken`, který může používat jako druhý parametr k načítání dalších položek.

## <a name="download-blobs"></a>Stáhněte si objekty BLOB

Stažení dat z objektů blob, následujícím způsobem:

* **getBlobToLocalFile** - zapíše objektů blob obsah do souboru

* **getBlobToStream** - zapíše obsah objektů blob proudu

* **getBlobToText** - zapíše obsah objektů blob řetězec

* **createReadStream** - poskytuje toku číst ze objektů blob

Následující příklad ukazuje použití **getBlobToStream** ke stažení obsah objektů blob **myblob** a uložení souboru **výstup.txt** pomocí datového proudu:

    var fs = require('fs');
    blobSvc.getBlobToStream('mycontainer', 'myblob', fs.createWriteStream('output.txt'), function(error, result, response){
      if(!error){
        // blob retrieved
      }
    });

`result` Obsahuje informace o objektů blob, včetně informací o **ETag** .

## <a name="delete-a-blob"></a>Odstranění objektů blob

Nakonec odstranit objektů blob, zavolejte **deleteBlob**. Následující příklad odstraní objektů blob s názvem **myblob**.

    blobSvc.deleteBlob(containerName, 'myblob', function(error, response){
      if(!error){
        // Blob has been deleted
      }
    });

## <a name="concurrent-access"></a>Souběžné přístup

Pro podporu souběžné přístupu k objektů blob z několika klientů nebo víc instancí obrázku, můžete použít **ETags** nebo **zapůjčení**.

* **Etag** – umožňuje zjišťování, který objektů blob nebo kontejneru upraven jiným procesem

* **Zapůjčení** – umožňuje získat výhradním, obnovitelné, psaní nebo odstranění přístupu k objektů blob pro určitého časového období

### <a name="etag"></a>ETag

Použití ETags v případě potřeby povolit více klientů nebo instance zapisovat do bloku objektů Blob nebo stránku kulatý současně. ETag umožňuje určit, pokud kontejneru nebo objektů blob změnila, protože původně číst nebo ji vytvořila, který umožňuje Vyhněte se přepisování změny u jiného klienta nebo obrázku.

Podmínky ETag můžete nastavit pomocí volitelné `options.accessConditions` parametr. Následující příklad pouze nahraje **test.txt** soubor, pokud již existuje objektů blob a má hodnotu ETag obsaženy v `etagToMatch`.

    blobSvc.createBlockBlobFromLocalFile('mycontainer', 'myblob', 'test.txt', { accessConditions: { EtagMatch: etagToMatch} }, function(error, result, response){
        if(!error){
        // file uploaded
      }
    });

Pokud používáte ETags, obecné vzorek je:

1. Získáte ETag jako výsledek vytvořit, seznam nebo operace získání.

2. Provedení akce, kontrola, zda nebyla upravena hodnota ETag.

Pokud hodnota změnila, znamená to, že jiného klienta nebo instance objektů blob nebo kontejneru od změněn jste získali hodnotu ETag.

### <a name="lease"></a>Zapůjčení

Získat novou zápůjčku metodou **acquireLease** , zadání objektů blob nebo kontejneru, kterou chcete získat na. Například následující kód získá zápůjčku na **myblob**.

    blobSvc.acquireLease('mycontainer', 'myblob', function(error, result, response){
      if(!error) {
        console.log('leaseId: ' + result.id);
      }
    });

Další operace na **myblob** musí zadat `options.leaseId` parametr. Zapůjčení ID vrácené jako `result.id` z **acquireLease**.

> [AZURE.NOTE] Ve výchozím nastavení je doba trvání zápůjčky neomezená. Doba trvání nekonečno (mezi 15 a 60 sekund) můžete určit pomocí `options.leaseDuration` parametr.

Zapůjčené, použijte **releaseLease**. Přerušení zapůjčenou, ale zabránit ostatním uživatelům nový zapůjčení dokud nevyprší původní doba trvání, použijte **breakLease**.

## <a name="work-with-shared-access-signatures"></a>Práce s sdílených přístup podpisy

Sdílený přístup podpisy (přidružení zabezpečení) jsou bezpečný způsob zpřístupnit podrobného objektů BLOB a kontejnery bez zadání název účtu úložiště nebo klíče. Podpisy sdílený přístup se často používá k poskytování omezený přístup k datům, například povolení mobilní aplikace pro přístup k objektů BLOB.

> [AZURE.NOTE] Když můžete taky povolit anonymní přístup k objektů BLOB, povolit sdílený přístup podpisy můžete uvést další řízený přístup, jako třeba generovat přidružení zabezpečení.

Důvěryhodné aplikace například do cloudové služby vygeneruje podpisy sdílený přístup pomocí **generateSharedAccessSignature** **BlobService**a poskytne ji do aplikace nedůvěryhodných nebo částečně důvěryhodné například v mobilní aplikaci. Sdílené přístup podpisy se vytvářejí pomocí zásad, které popisuje zahájení a koncové datum, ve kterých jsou platné podpisy sdílený přístup, a taky úroveň přístupu udělil uchycení podpisy sdílený přístup.

Následující příklad vygeneruje nové zásady sdílené přístupu, která umožňuje uchycení podpisy sdílený přístup k provedení operace čtení na objektů blob **myblob** a vypršení platnosti 100 minut po o čase, který už je vytvořená.

    var startDate = new Date();
    var expiryDate = new Date(startDate);
    expiryDate.setMinutes(startDate.getMinutes() + 100);
    startDate.setMinutes(startDate.getMinutes() - 100);

    var sharedAccessPolicy = {
      AccessPolicy: {
        Permissions: azure.BlobUtilities.SharedAccessPermissions.READ,
        Start: startDate,
        Expiry: expiryDate
      },
    };

    var blobSAS = blobSvc.generateSharedAccessSignature('mycontainer', 'myblob', sharedAccessPolicy);
    var host = blobSvc.host;

Všimněte si, že informace hostitele je třeba zadat také, je to potřeba při uchycení podpisy sdílený přístup pokusí o přístup k kontejneru.

Klientská aplikace je pak používá k provádění operací proti objektů blob podpisy sdílený přístup s **BlobServiceWithSAS** . Následující získání informací o **myblob**.

    var sharedBlobSvc = azure.createBlobServiceWithSas(host, blobSAS);
    sharedBlobSvc.getBlobProperties('mycontainer', 'myblob', function (error, result, response) {
      if(!error) {
        // retrieved info
      }
    });

Protože podpisy sdílený přístup byly vytvořeny s přístupem jen pro čtení, pokud je k pokusu změnit objektů blob, bude vrácena chyba.

### <a name="access-control-lists"></a>Seznamy řízení přístupu

Seznam řízení přístupu (ACL) můžete taky nastavit zásady přístupu pro přidružení zabezpečení. To je užitečné, pokud chcete povolit více klientů přístup kontejneru, ale poskytují různé přístup zásady pro každého klienta.

ACL je implementovaná maticových zásady přístupu pomocí ID přidružené k každého zásadu. Následující příklad definuje dvě zásady, jeden pro uživatel '1' a druhý pro "uživatel2":

    var sharedAccessPolicy = {
      user1: {
        Permissions: azure.BlobUtilities.SharedAccessPermissions.READ,
        Start: startDate,
        Expiry: expiryDate
      },
      user2: {
        Permissions: azure.BlobUtilities.SharedAccessPermissions.WRITE,
        Start: startDate,
        Expiry: expiryDate
      }
    };

Následujícím příkladu je načtena aktuální ACL pro **mycontainer**a potom je přidá nové zásady použití **setBlobAcl**. Tento přístup vám umožňuje:

    var extend = require('extend');
    blobSvc.getBlobAcl('mycontainer', function(error, result, response) {
      if(!error){
        var newSignedIdentifiers = extend(true, result.signedIdentifiers, sharedAccessPolicy);
        blobSvc.setBlobAcl('mycontainer', newSignedIdentifiers, function(error, result, response){
          if(!error){
            // ACL set
          }
        });
      }
    });

Když je nastavená ACL, můžete vytvořte podpisy sdíleného přístupu na základě ID pro zásady. Následující příklad vytvoří nový sdílený přístup podpisy "uživatel2":

    blobSAS = blobSvc.generateSharedAccessSignature('mycontainer', { Id: 'user2' });

## <a name="next-steps"></a>Další kroky

Další informace naleznete v následujících zdrojích.

-   [Azure úložiště SDK Reference rozhraní API uzel][]
-   [Blog týmu Azure úložiště][]
-   [Azure úložiště SDK uzel][] úložiště na GitHub
-   [Středisko pro vývojáře Node.js](/develop/nodejs/)
-   [Přenos dat pomocí nástroje příkazového řádku AzCopy](storage-use-azcopy.md)

[Azure úložiště SDK uzel]: https://github.com/Azure/azure-storage-node

[Vytvoření webové aplikace Node.js v aplikaci služby Azure]: ../app-service-web/web-sites-nodejs-develop-deploy-mac.md
[Node.js Cloud Service with Storage]: ../cloud-services/storage-nodejs-use-table-storage-cloud-service-app.md
[Použití tabulku služba Azure Node.js web appu]: ../app-service-web/storage-nodejs-use-table-storage-web-site.md
[Vytvořte a nasaďte do Node.js webových aplikací do Azure pomocí Web matice]: ../app-service-web/web-sites-nodejs-use-webmatrix.md
[Using the REST API]: http://msdn.microsoft.com/library/azure/hh264518.aspx
[Azure Portal]: https://portal.azure.com
[Vytvoření a nasazení aplikace Node.js cloudové služby Azure]: ../cloud-services/cloud-services-nodejs-develop-deploy-app.md
[Blog týmu Azure úložiště]: http://blogs.msdn.com/b/windowsazurestorage/
[Azure úložiště SDK Reference rozhraní API uzel]: http://dl.windowsazure.com/nodestoragedocs/index.html
