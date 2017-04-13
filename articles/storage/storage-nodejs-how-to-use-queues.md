<properties
    pageTitle="Použití úložiště fronty z Node.js | Microsoft Azure"
    description="Naučte se používat službu Azure fronty můžete vytvářet a odstraňovat fronty, vložení, získat a odstraňování zpráv. Vzorky napsané v Node.js."
    services="storage"
    documentationCenter="nodejs"
    authors="robinsh"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="nodejs"
    ms.topic="article"
    ms.date="08/11/2016"
    ms.author="robinsh"/>


# <a name="how-to-use-queue-storage-from-nodejs"></a>Použití úložiště fronty z Node.js

[AZURE.INCLUDE [storage-selector-queue-include](../../includes/storage-selector-queue-include.md)]
<br/>
[AZURE.INCLUDE [storage-try-azure-tools-queues](../../includes/storage-try-azure-tools-queues.md)]

## <a name="overview"></a>Základní informace

Tato příručka, uvidíte, jak provádět běžné scénáře pomocí služby Microsoft Azure fronty. Vzorky jsou vytvořeny pomocí rozhraní API Node.js. Scénáře nichž se uplatní zahrnují **Vložení**, **prohlížení**, **začíná**a **odstraňování** zpráv, jakož i **vytváření a odstraňování fronty**.

[AZURE.INCLUDE [storage-queue-concepts-include](../../includes/storage-queue-concepts-include.md)]

[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="create-a-nodejs-application"></a>Vytvoření aplikace Node.js

Vytvoření prázdné Node.js aplikace. Vytvoření aplikace pro Node.js najdete v článku [Vytvoření Node.js web app v aplikaci služby Azure], [sestavovat a nasazovat aplikace Node.js cloudové služby Azure] pomocí prostředí Windows PowerShell nebo [vytvořte a nasaďte Node.js web appu k Azure pomocí Web matice].

## <a name="configure-your-application-to-access-storage"></a>Konfigurace aplikace pro Access úložiště

Abyste mohli používat Azure úložiště, potřebujete SDK úložiště Azure pro Node.js, který obsahuje sadu pohodlí knihoven, které komunikaci se službami ZBÝVAJÍCÍ úložiště.

### <a name="use-node-package-manager-npm-to-obtain-the-package"></a>Použití uzel balíčku správce (NPM) získání balíčku

1.  Použití rozhraní příkazového řádku například **prostředí PowerShell** (Windows), **terminálu** (Mac) nebo **flám** (Unix), přejděte do složky místo, kam jste vytvořili aplikace vzorku.

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

Na začátek **server.js** souboru aplikace místo, kam chcete použít úložiště pomocí poznámkového bloku nebo jiném textovém editoru, přidejte následující:

    var azure = require('azure-storage');

## <a name="setup-an-azure-storage-connection"></a>Nastavení připojení k Azure úložiště

Modul azure přečte proměnné AZURE\_úložiště\_účet a AZURE\_úložiště\_přístup\_klíč nebo AZURE\_úložiště\_připojení\_řetězec informace potřebné pro připojení ke svému účtu Azure úložiště. Pokud nejsou tyto proměnné, je třeba určit informace o účtu při volání **createQueueService**.

Příklad nastavení proměnné prostředí [Azure portál](https://portal.azure.com) Azure webu najdete v článku [Node.js webové aplikace pomocí služby Azure tabulky].

## <a name="how-to-create-a-queue"></a>Postup: Vytvoření fronty

Následující kód vytvoří **QueueService** objekt, který umožňuje pracovat s fronty.

    var queueSvc = azure.createQueueService();

Použijte metodu **createQueueIfNotExists** , která vrací zadané frontě, pokud již existuje nebo vytvoří nové fronty se zadaným názvem, pokud už neexistuje.

    queueSvc.createQueueIfNotExists('myqueue', function(error, result, response){
      if(!error){
        // Queue created or exists
      }
    });

Pokud vytvořeno fronty `result.created` platí. Pokud existuje fronta `result.created` vyhodnotí jako NEPRAVDA.

### <a name="filters"></a>Filtry

Volitelné filtrování operace se dají použít k operací pomocí **QueueService**. Filtrování operace může obsahovat protokolování automaticky opakování, atd. Filtry jsou objekty, které implementovat metodu k podpisu:

    function handle (requestOptions, next)

Po provedení předzpracování na žádost o možnostech, metoda potřebuje volání "Další" předávání zpětné podpisem následující:

    function (returnObject, finalCallback, next)

V tomto zpětné a po zpracování returnObject (odpovědi žádosti o na serveru) musí zpětné vyvolat dál, pokud existuje pokračovat ve zpracování další filtry nebo vyvolat finalCallback jednoduše jinak skončily vyvolání služby.

Dva filtry, které implementace logiky opakovat jsou součástí SDK Azure Node.js, **ExponentialRetryPolicyFilter** a **LinearRetryPolicyFilter**. Následujícím příkladu je vytvořen **QueueService** objekt, který používá **ExponentialRetryPolicyFilter**:

    var retryOperations = new azure.ExponentialRetryPolicyFilter();
    var queueSvc = azure.createQueueService().withFilter(retryOperations);

## <a name="how-to-insert-a-message-into-a-queue"></a>Jak: Vložte do zprávy do fronty

Vložit zprávu do fronty, použijte metodu **createMessage** můžete vytvořit novou zprávu a přidat ho do fronty.

    queueSvc.createMessage('myqueue', "Hello world!", function(error, result, response){
      if(!error){
        // Message inserted
      }
    });

## <a name="how-to-peek-at-the-next-message"></a>Způsob: Náhled na další zprávu

Zpráva začátku fronty můžete prohlížet bez odebrání ve frontě tak, že zavoláte metodu **peekMessages** . Ve výchozím nastavení **peekMessages** prohlížením na podstatu sdělení.

    queueSvc.peekMessages('myqueue', function(error, result, response){
      if(!error){
        // Message text is in messages[0].messageText
      }
    });

`result` Obsahuje zprávu.

> [AZURE.NOTE] Používání **peekMessages** nejsou že žádné zprávy ve frontě nebude vrátí chybu, ale bez zprávy budou vráceny.

## <a name="how-to-dequeue-the-next-message"></a>Jak: Dequeue další zprávu

Zpracování zprávy je proces dvoufázový:

1. Dequeue zprávu.

2. Odstraní zprávu.

Chcete-li dequeue zprávu, použijte **getMessages**. Díky zprávy neviditelné ve frontě tak, aby ostatní klienti zpracovat. Po zpracování zprávy aplikace volání **deleteMessage** a vymažete ho ze fronty. Následující příklad získá zprávu a potom jej odstraní:

    queueSvc.getMessages('myqueue', function(error, result, response){
      if(!error){
        // Message text is in messages[0].messageText
        var message = result[0];
        queueSvc.deleteMessage('myqueue', message.messageId, message.popReceipt, function(error, response){
          if(!error){
            //message deleted
          }
        });
      }
    });

> [AZURE.NOTE] Ve výchozím nastavení zprávy pouze skryté 30 sekund, po které je viditelné pro ostatní klienti. Můžete zadat jinou hodnotu pomocí `options.visibilityTimeout` s **getMessages**.

> [AZURE.NOTE]
> Používání **getMessages** nejsou že žádné zprávy ve frontě nebude vrátí chybu, ale bez zprávy budou vráceny.

## <a name="how-to-change-the-contents-of-a-queued-message"></a>Postup: Změna obsah ve frontě zprávy

Můžete změnit obsah zprávy v místě ve frontě pomocí **updateMessage**. V následujícím příkladu se aktualizuje v textu zprávy:

    queueSvc.getMessages('myqueue', function(error, result, response){
      if(!error){
        // Got the message
        var message = result[0];
        queueSvc.updateMessage('myqueue', message.messageId, message.popReceipt, 10, {messageText: 'new text'}, function(error, result, response){
          if(!error){
            // Message updated successfully
          }
        });
      }
    });

## <a name="how-to-additional-options-for-dequeuing-messages"></a>: Další možnosti pro vyřazení zpráv

Je možné upravit načítání zpráv z fronty dvěma způsoby:

* `options.numOfMessages`– Načtení dávku zprávy (až 32.)
* `options.visibilityTimeout`– Nastavte časový limit invisibility prodloužit nebo zkrátit.

Metoda **getMessages** získat 15 zprávy v jednom volání v následujícím příkladu. Zpracuje každou zprávu pomocí a pak pro smyčky. Také nastaví časový limit invisibility pět minut pro všechny zprávy vrácené tímto způsobem.

    queueSvc.getMessages('myqueue', {numOfMessages: 15, visibilityTimeout: 5 * 60}, function(error, result, response){
      if(!error){
        // Messages retreived
        for(var index in result){
          // text is available in result[index].messageText
          var message = result[index];
          queueSvc.deleteMessage(queueName, message.messageId, message.popReceipt, function(error, response){
            if(!error){
              // Message deleted
            }
          });
        }
      }
    });

## <a name="how-to-get-the-queue-length"></a>Jak: Získání délka fronty

**GetQueueMetadata** vrátí metadata fronty, včetně přibližnou počet zprávy, které čekají ve frontě.

    queueSvc.getQueueMetadata('myqueue', function(error, result, response){
      if(!error){
        // Queue length is available in result.approximateMessageCount
      }
    });

## <a name="how-to-list-queues"></a>Jak: Seznam fronty

Načtení seznamu fronty, použijte **listQueuesSegmented**. Načtěte seznam filtrované podle určitých předponu, použijte **listQueuesSegmentedWithPrefix**.

    queueSvc.listQueuesSegmented(null, function(error, result, response){
      if(!error){
        // result.entries contains the list of queues
      }
    });

Pokud všechny fronty nemůže být vrácena, `result.continuationToken` mohou sloužit jako první parametr **listQueuesSegmented** nebo druhý parametr **listQueuesSegmentedWithPrefix** k vyhledání více výsledků.

## <a name="how-to-delete-a-queue"></a>Jak: Odstranění fronty

Pokud chcete odstranit fronty a všech zpráv, na které se nacházejí v ní, kontaktujte metodu **deleteQueue** fronty objektu.

    queueSvc.deleteQueue(queueName, function(error, response){
      if(!error){
        // Queue has been deleted
      }
    });

Pokud chcete vymazat všechny zprávy z fronty bez jeho odstranění, pomocí **clearMessages**.

## <a name="how-to-work-with-shared-access-signatures"></a>Postup: práce s sdílených přístup podpisy

Podpisy sdílené aplikace Access (přidružení zabezpečení) jsou bezpečný způsob podrobného zpřístupnit fronty bez zadání název účtu úložiště nebo klíče. Přidružení zabezpečení se často používá k poskytování omezený přístup k vaší fronty, například povolení mobilní aplikace předložit zprávy.

Důvěryhodné aplikace například do cloudové služby vygeneruje přidružení zabezpečení pomocí **generateSharedAccessSignature** **QueueService**a poskytne ji do aplikace nedůvěryhodných nebo částečně důvěryhodné. Například v mobilní aplikaci. Přidružení zabezpečení je generováno použití zásad, které popisuje počáteční a koncové datum, kdy přidružení je platný, stejně jako úroveň přístupu majiteli přidružení zabezpečení.

Následující příklad generuje nové zásady sdílený přístup, které vám umožní uchycení přidružení zabezpečení přidáte zprávy do fronty a vypršení platnosti 100 minut po o čase, který už je vytvořená.

    var startDate = new Date();
    var expiryDate = new Date(startDate);
    expiryDate.setMinutes(startDate.getMinutes() + 100);
    startDate.setMinutes(startDate.getMinutes() - 100);

    var sharedAccessPolicy = {
      AccessPolicy: {
        Permissions: azure.QueueUtilities.SharedAccessPermissions.ADD,
        Start: startDate,
        Expiry: expiryDate
      }
    };

    var queueSAS = queueSvc.generateSharedAccessSignature('myqueue', sharedAccessPolicy);
    var host = queueSvc.host;

Všimněte si, že informace hostitele je třeba zadat také, je to potřeba při uchycení přidružení zabezpečení pokusí o přístup k fronty.

Klientská aplikace je pak používá přidružení zabezpečení s **QueueServiceWithSAS** provádět operace proti fronty. V následujícím příkladu se připojí k fronty a vytvoří zprávu.

    var sharedQueueService = azure.createQueueServiceWithSas(host, queueSAS);
    sharedQueueService.createMessage('myqueue', 'Hello world from SAS!', function(error, result, response){
      if(!error){
        //message added
      }
    });

Protože přidružení zabezpečení byl vytvořený pomocí přidat přístup, pokud pokus o provedené číst, aktualizovat a odstraňovat zprávy, bude vrácena chyba.

### <a name="access-control-lists"></a>Seznamy řízení přístupu

Seznam řízení přístupu (ACL) můžete taky nastavit zásady přístupu pro přidružení zabezpečení. To je užitečné, pokud chcete povolit více klientů přístup k frontě, ale poskytují různé přístup zásady pro každého klienta.

ACL je implementovaná maticových zásady přístupu pomocí ID přidružené k každého zásadu. Následující příklad definuje dvě zásady; jeden 'uživatel 1' a jeden pro "uživatel2":

    var sharedAccessPolicy = {
      user1: {
        Permissions: azure.QueueUtilities.SharedAccessPermissions.PROCESS,
        Start: startDate,
        Expiry: expiryDate
      },
      user2: {
        Permissions: azure.QueueUtilities.SharedAccessPermissions.ADD,
        Start: startDate,
        Expiry: expiryDate
      }
    };

V následujícím příkladu je načtena aktuální ACL pro **Moje_fronta**a potom sečtením nové zásady použití **setQueueAcl**. Tento přístup vám umožňuje:

    var extend = require('extend');
    queueSvc.getQueueAcl('myqueue', function(error, result, response) {
      if(!error){
        var newSignedIdentifiers = extend(true, result.signedIdentifiers, sharedAccessPolicy);
        queueSvc.setQueueAcl('myqueue', newSignedIdentifiers, function(error, result, response){
          if(!error){
            // ACL set
          }
        });
      }
    });

Po nastavení ACL můžete pak vytvořit přidružení zabezpečení podle ID pro zásady. Následující příklad vytvoří nová přidružení zabezpečení "uživatel2":

    queueSAS = queueSvc.generateSharedAccessSignature('myqueue', { Id: 'user2' });

## <a name="next-steps"></a>Další kroky

Teď, když jste se naučili základy fronty úložiště, tyto odkazy vedou na další informace o složitější úlohy úložiště.

-   Navštivte [Blog týmu Azure úložiště][].
-   Navštivte [Azure úložiště SDK uzel][] úložiště na GitHub.

  [Azure úložiště SDK uzel]: https://github.com/Azure/azure-storage-node
  [using the REST API]: http://msdn.microsoft.com/library/azure/hh264518.aspx
  [Azure Portal]: https://portal.azure.com
  [Vytvoření webové aplikace Node.js v aplikaci služby Azure]: ../app-service-web/web-sites-nodejs-develop-deploy-mac.md
  [Node.js Cloud Service with Storage]: ../cloud-services/storage-nodejs-use-table-storage-cloud-service-app.md
  [Použití tabulku služba Azure Node.js web appu]: ../app-service-web/storage-nodejs-use-table-storage-web-site.md


  [Queue1]: ./media/storage-nodejs-how-to-use-queues/queue1.png
  [plus-new]: ./media/storage-nodejs-how-to-use-queues/plus-new.png
  [quick-create-storage]: ./media/storage-nodejs-how-to-use-queues/quick-storage.png



  [Vytvoření a nasazení aplikace Node.js cloudové služby Azure]: ../cloud-services/cloud-services-nodejs-develop-deploy-app.md
  [Blog týmu Azure úložiště]: http://blogs.msdn.com/b/windowsazurestorage/
  [Vytvořte a nasaďte do Node.js webových aplikací do Azure pomocí Web matice]: ../app-service-web/web-sites-nodejs-use-webmatrix.md
