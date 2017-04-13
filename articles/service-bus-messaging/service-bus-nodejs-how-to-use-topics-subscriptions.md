<properties 
    pageTitle="Použití služby Bus témata s Node.js | Microsoft Azure" 
    description="Naučte se používat službu Bus témata a předplatná v Azure pomocí funkčně Node.js aplikace." 
    services="service-bus" 
    documentationCenter="nodejs" 
    authors="sethmanheim" 
    manager="timlt" 
    editor=""/>

<tags 
    ms.service="service-bus" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="nodejs" 
    ms.topic="article" 
    ms.date="10/04/2016" 
    ms.author="sethm"/>


# <a name="how-to-use-service-bus-topics-and-subscriptions"></a>Postup použití služby Bus témata a předplatného

[AZURE.INCLUDE [service-bus-selector-topics](../../includes/service-bus-selector-topics.md)]

Tato příručka popisuje, jak používat službu Bus témata a předplatná z Node.js aplikací. Scénáře nichž se uplatní zahrnují **vytváření témata a předplatná**, **vytváření filtrů předplatného**, **posílání zpráv** tématu **přijímat zprávy z předplatného**a **Odstranění témata a předplatná**. Další informace o témata a předplatná naleznete v části [Další kroky](#next-steps) .

[AZURE.INCLUDE [howto-service-bus-topics](../../includes/howto-service-bus-topics.md)]

## <a name="create-a-nodejs-application"></a>Vytvoření aplikace Node.js

Vytvoření prázdné Node.js aplikace. Pokyny týkající se vytváření aplikace Node.js najdete v tématu [Vytvoření a nasazení aplikace Node.js k webovému serveru Azure], [Node.js cloudové služby][] pomocí WebMatrix prostředí Windows PowerShell nebo Web.

## <a name="configure-your-application-to-use-service-bus"></a>Konfigurace aplikace pomocí služby Bus

Použití služby Bus, stáhněte balíček Node.js Azure. Tento balíček obsahuje sadu knihoven, které komunikaci se službami ZBÝVAJÍCÍ Bus služby.

### <a name="use-node-package-manager-npm-to-obtain-the-package"></a>Použití uzel balíčku správce (NPM) získání balíčku

1.  Použití rozhraní příkazového řádku například **prostředí PowerShell** (Windows), **terminálu** (Mac) nebo **flám** (Unix), přejděte do složky místo, kam jste vytvořili aplikace vzorku.

2.  V okně příkaz, který by měl mít za následek následující výstup zadejte **npm instalace azure** :

    ```
        azure@0.7.5 node_modules\azure
    ├── dateformat@1.0.2-1.2.3
    ├── xmlbuilder@0.4.2
    ├── node-uuid@1.2.0
    ├── mime@1.2.9
    ├── underscore@1.4.4
    ├── validator@1.1.1
    ├── tunnel@0.0.2
    ├── wns@0.5.3
    ├── xml2js@0.2.7 (sax@0.5.2)
    └── request@2.21.0 (json-stringify-safe@4.0.0, forever-agent@0.5.0, aws-sign@0.3.0, tunnel-agent@0.3.0, oauth-sign@0.3.0, qs@0.6.5, cookie-jar@0.3.0, node-uuid@1.4.0, http-signature@0.9.11, form-data@0.0.8, hawk@0.13.1)
    ```

3.  Můžete ručně spusťte příkaz **ls** ověřte, že **uzel\_moduly** složky byl vytvořen. V této složce najděte balíček **azure** , který obsahuje knihoven potřebujete získat přístup k služby Bus témata.

### <a name="import-the-module"></a>Import modulu

Použití programu Poznámkový blok nebo jiném textovém editoru, přidejte následující text do horní části souboru **server.js** aplikace:

```
var azure = require('azure');
```

### <a name="set-up-a-service-bus-connection"></a>Vytvoření připojení služby Bus

Modul Azure přečte proměnné AZURE\_SERVICEBUS\_obor názvů a AZURE\_SERVICEBUS\_přístup\_klíčové informace potřebné pro připojení k Bus služby. Pokud nejsou tyto proměnné, je třeba určit informace o účtu při volání **createServiceBusService**.

Příklad nastavení proměnné prostředí v souboru konfigurace cloudové služby Azure najdete v článku [Node.js Cloudová služba úložiště][].

Příklad nastavení proměnné prostředí [Azure klasické portál][] Azure webu najdete v článku [Node.js webové aplikace s úložištěm][].

## <a name="create-a-topic"></a>Vytvoření tematického

Objekt **ServiceBusService** umožňuje pracovat s témata. Následující kód vytvoří objekt **ServiceBusService** . Přidejte ho do horní části souboru **server.js** po provedení příkazu importovat modul azure:

```
var serviceBusService = azure.createServiceBusService();
```

Zavoláním **createTopicIfNotExists** objektu **ServiceBusService** zadané téma vrátí (pokud existuje) nebo se vytvoří nového tématu se zadaným názvem. Následující kód používá **createTopicIfNotExists** k vytvoření nebo připojení k tématu s názvem "MyTopic":

```
serviceBusService.createTopicIfNotExists('MyTopic',function(error){
    if(!error){
        // Topic was created or exists
        console.log('topic created or exists.');
    }
});
```

**createServiceBusService** podporuje i další možnosti, které umožňují buď přijměte výchozí nastavení téma například zprávy hodnota time to live nebo tématu maximální velikost. V následujícím příkladě velikost maximální téma 5 GB s abychom live 1 minuty:

```
var topicOptions = {
        MaxSizeInMegabytes: '5120',
        DefaultMessageTimeToLive: 'PT1M'
    };

serviceBusService.createTopicIfNotExists('MyTopic', topicOptions, function(error){
    if(!error){
        // topic was created or exists
    }
});
```

### <a name="filters"></a>Filtry

Volitelné filtrování operace se dají použít k operací pomocí **ServiceBusService**. Filtrování operace může obsahovat protokolování automaticky opakování, atd. Filtry jsou objekty, které implementovat metodu k podpisu:

```
function handle (requestOptions, next)
```

Po dokončení předzpracování na žádost o možnostech, metoda volá `next` předávání zpětné podpisem následující:

```
function (returnObject, finalCallback, next)
```

V tomto zpětné a po zpracování **returnObject** (odpovědi žádosti o na serveru) potřeb zpětné buď vyvolat další pokud existuje pokračovat ve zpracování další filtry nebo vyvolat **finalCallback** jednoduše jinak skončily vyvolání služby.

Dva filtry, které implementace logiky opakovat jsou součástí SDK Azure Node.js, **ExponentialRetryPolicyFilter** a **LinearRetryPolicyFilter**. Následujícím příkladu je vytvořen **ServiceBusService** objekt, který používá **ExponentialRetryPolicyFilter**:

    var retryOperations = new azure.ExponentialRetryPolicyFilter();
    var serviceBusService = azure.createServiceBusService().withFilter(retryOperations);

## <a name="create-subscriptions"></a>Vytvoření předplatného

Téma předplatná vzniká za **ServiceBusService** objekt. Předplatné se s názvem a nemůže mít volitelné filtr, s omezením nastavení doručování zpráv do fronty virtuální předplatného.

> [AZURE.NOTE] Předplatná jsou trvalý a budou dál existují až do buď nebo tématu souvisejí s, budou odstraněny. Pokud aplikace obsahuje použití logických operátorů k vytvoření předplatné, ho nejdřív zkontrolujte Pokud předplatné už existuje pomocí metody **getSubscription** .

### <a name="create-a-subscription-with-the-default-matchall-filter"></a>Vytvoření předplatné s filtrem výchozí (MatchAll)

Filtr **MatchAll** je výchozí filtr, který se používá, pokud zadaný žádný filtr při vytvoření nové předplatné. Při použití filtru **MatchAll** všech zpráv publikovaných na téma jsou umístěny ve frontě virtuální předplatného. Následující příklad vytvoří předplatné s názvem "AllMessages" a používá jako výchozí **MatchAll** filtr.

```
serviceBusService.createSubscription('MyTopic','AllMessages',function(error){
    if(!error){
        // subscription created
    }
});
```

### <a name="create-subscriptions-with-filters"></a>Vytvoření předplatného s filtry

Můžete taky vytvořit filtry umožňující povolit, abyste obor, které zprávy odeslané na téma má zobrazit v rámci předplatného konkrétním tématu.

Flexibilní typ filtru nepodporuje pro předplatná je **SqlFilter**, které podmnožinu SQL92. Filtry SQL pracují s vlastnostmi zprávy, které jsou publikované na téma. Pro další podrobnosti o výrazy, které lze použít s filtrem SQL zkontrolujte [SqlFilter.SqlExpression] [ SqlFilter.SqlExpression] syntaxe.

Filtry můžete přidat k předplatnému pomocí metody **createRule** **ServiceBusService** objektu. Tento způsob umožňuje přidat nové filtry k existujícímu předplatnému.

> [AZURE.NOTE] Vzhledem k tomu, výchozí filtr je nepoužije automaticky pro všechny nové předplatné, je nutné nejprve odebrat výchozí filtr nebo **MatchAll** potlačí všechny filtry, které je možné zadat. Výchozí pravidlo můžete odebrat pomocí metody **DeletRule** **ServiceBusService** objektu.

Následující příklad vytvoří předplatné s názvem `HighMessages` s **SqlFilter** pouze vybere zprávy, které mají vyšší než 3 vlastní **messagenumber** vlastnost:

```
serviceBusService.createSubscription('MyTopic', 'HighMessages', function (error){
    if(!error){
        // subscription created
        rule.create();
    }
});
var rule={
    deleteDefault: function(){
        serviceBusService.deleteRule('MyTopic',
            'HighMessages', 
            azure.Constants.ServiceBusConstants.DEFAULT_RULE_NAME, 
            rule.handleError);
    },
    create: function(){
        var ruleOptions = {
            sqlExpressionFilter: 'messagenumber > 3'
        };
        rule.deleteDefault();
        serviceBusService.createRule('MyTopic', 
            'HighMessages', 
            'HighMessageFilter', 
            ruleOptions, 
            rule.handleError);
    },
    handleError: function(error){
        if(error){
            console.log(error)
        }
    }
}
```

Podobně následující příklad vytvoří předplatné s názvem `LowMessages` s **SqlFilter** pouze vybere zprávy, které mají **messagenumber** vlastnost menším větší nebo rovnu 3:

```
serviceBusService.createSubscription('MyTopic', 'LowMessages', function (error){
    if(!error){
        // subscription created
        rule.create();
    }
});
var rule={
    deleteDefault: function(){
        serviceBusService.deleteRule('MyTopic',
            'LowMessages', 
            azure.Constants.ServiceBusConstants.DEFAULT_RULE_NAME, 
            rule.handleError);
    },
    create: function(){
        var ruleOptions = {
            sqlExpressionFilter: 'messagenumber <= 3'
        };
        rule.deleteDefault();
        serviceBusService.createRule('MyTopic', 
            'LowMessages', 
            'LowMessageFilter', 
            ruleOptions, 
            rule.handleError);
    },
    handleError: function(error){
        if(error){
            console.log(error)
        }
    }
}
```

Kdy je teď odeslána zpráva `MyTopic`, bude vždy zašle příjemce, nemáte předplatné `AllMessages` téma předplatné a selektivním doručeno příjemce, nemáte předplatné `HighMessages` a `LowMessages` téma předplatná (v závislosti na obsah zprávy).

## <a name="how-to-send-messages-to-a-topic"></a>Odeslání zprávy na téma

Pokud chcete odeslat zprávu o na téma Bus služby, třeba aplikace použijte metodu **sendTopicMessage** **ServiceBusService** objektu.
Zprávy zasílané do služby Bus témata jsou **BrokeredMessage** objekty.
**BrokeredMessage** objekty mají sadu standardní vlastnosti (například **Popisek** a **TimeToLive**) slovník, který se používá k ukládání specifické vlastnosti vlastní aplikace a textu data řetězce. Aplikaci můžete nastavit textu zprávy předáním hodnotu řetězce **sendTopicMessage** a všechny požadované standardní vlastnosti se zobrazí výchozí hodnoty.

Následující příklad ukazuje, jak chcete poslat pět testování zprávy "MyTopic". Všimněte si, že hodnotu **messagenumber** každé zprávy se mění na opakování smyčky (to bude určovat, které předplatné obdrží):

```
var message = {
    body: '',
    customProperties: {
        messagenumber: 0
    }
}

for (i = 0;i < 5;i++) {
    message.customProperties.messagenumber=i;
    message.body='This is Message #'+i;
    serviceBusService.sendTopicMessage(topic, message, function(error) {
      if (error) {
        console.log(error);
      }
    });
}
```

Služba Bus témata podpory pro maximální velikost 256 KB [Standardní osy](service-bus-premium-messaging.md) a 1 MB [Premium osy](service-bus-premium-messaging.md). Záhlaví obsahuje vlastností standardních a vlastních aplikací, může mít maximální velikosti doručovaných 64 KB. Tam bez omezení počtu zpráv v tématu je ale zakončení na celkové velikosti zprávy uskutečňuje podle tématu. Toto téma velikost je definována na údaje o času vytvoření, s horní mez 5 GB.

## <a name="receive-messages-from-a-subscription"></a>Přijímání zpráv z předplatného

Zprávy jsou doručeny z předplatného pomocí metody **receiveSubscriptionMessage** **ServiceBusService** objektu. Ve výchozím nastavení jsou odstraněny zprávy z předplatného podle jejich přečtení; Můžete však čtení (náhledu) a zamkněte zprávu aniž by odstranil z předplatného nastavením volitelný parametr **isPeekLock** na **hodnotu true**.

Výchozí chování aplikace pro čtení a odstranění zprávu jako součást operace přijímání je nejjednodušší modelu a je nejvhodnější pro scénáře, které aplikace nevadí vám není zpracování zprávy v případě se nepovede. Pokud chcete porozumět tomu, zvažte scénáři, ve kterém příjemci problémy žádost přijmout a potom dojde k chybě před zpracování. Protože služby Bus bude označené zprávy jako spotřebované, a když aplikace restartuje, nebude zahájen jinými zprávy znovu ho bude uniknout zprávu, která byla spotřebované množství před k chybě.

Pokud parametr **isPeekLock** nastavena na **hodnotu true**, nebude příjem operace dva stupně, která umožní podpory aplikace, které nelze tolerovat chybějící zprávy. Když služby Bus obdrží žádost, zjistí další zprávu do být spotřebované množství, ji nechcete, aby ostatní uživatelé jeho přijetí zamkne a vrátí hodnotu aplikace.
Po aplikace dokončení zpracování zprávy (nebo problémy se spolehlivým ukládá pro budoucí zpracování), provede druhé fáze procesu přijmout volání **deleteMessage** metody a poskytnutím zpráva odstraněna jako parametr. Metoda **deleteMessage** označte zprávu jako právě spotřebované a odebrat z předplatného.

Následující příklad ukazuje, jak zpráv, stavu a zpracovány pomocí **receiveSubscriptionMessage**. Příklad nejdřív obdrží a odstraní zprávu z předplatného "LowMessages" a obdrží zprávy od "HighMessages" předplatného pomocí **isPeekLock** nastavena na hodnotu true. Pak odstraní zprávy pomocí **deleteMessage**:

```
serviceBusService.receiveSubscriptionMessage('MyTopic', 'LowMessages', function(error, receivedMessage){
    if(!error){
        // Message received and deleted
        console.log(receivedMessage);
    }
});
serviceBusService.receiveSubscriptionMessage('MyTopic', 'HighMessages', { isPeekLock: true }, function(error, lockedMessage){
    if(!error){
        // Message received and locked
        console.log(lockedMessage);
        serviceBusService.deleteMessage(lockedMessage, function (deleteError){
            if(!deleteError){
                // Message deleted
                console.log('message has been deleted.');
            }
        }
    }
});
```

## <a name="how-to-handle-application-crashes-and-unreadable-messages"></a>Postup v případě havárie aplikací a čitelný zprávy

Služba Bus poskytuje funkce vám řádně obnovení z chyb v aplikaci nebo potíže zpracování zprávy. Pokud je příjemce aplikace nelze ji zpracovat z nějakého důvodu, ho metodu **unlockMessage** zavolat na objekt **ServiceBusService** . To způsobí služby Bus odemknout zpráv v rámci předplatného a jeho Příprava získaná znovu stejné náročný aplikací nebo jinou náročný aplikací.

Je také časového limitu zprávy Uzamčeno v rámci předplatného a pokud aplikaci se nezdaří zpracování zprávy před časový limit zámku vyprší platnost (například pokud způsobí zhroucení aplikace), pak služby Bus odemčena zprávy automaticky a díky kterému je dostupný získaná znovu.

V případě, že po zpracování zprávy, ale před názvem metody **deleteMessage** způsobí zhroucení aplikace, pak zpráva bude být znovu dodány aplikaci po restartování. To je někdy označovány jako **Aspoň jednou Processing**, tedy každá zpráva budou zpracovány aspoň jednou, ale v některých případech může být znovu dodány stejná zpráva. Pokud scénáře nelze tolerovat duplicitní zpracování, pak vývojáři bychom přidat další použití logických operátorů k aplikaci, která bude řešit duplicitní zprávu doručení. To je často dosáhnout pomocí vlastnosti **MessageId** zprávy, která se nemění přes pokusy o doručení.

## <a name="delete-topics-and-subscriptions"></a>Odstranění témata a předplatného

Témata předplatná jsou trvalý a explicitně odstraníte přes [Azure klasické portál][] nebo programově.
Následující příklad ukazuje, jak odstranit téma s názvem `MyTopic`:

    serviceBusService.deleteTopic('MyTopic', function (error) {
        if (error) {
            console.log(error);
        }
    });

Odstranění tématu budou odstraněny také žádné předplatné, které jsou registrované s tématem. Předplatné můžete odstraní taky nezávisle na sobě. Následující příklad ukazuje, jak odstranit předplatné s názvem `HighMessages` z `MyTopic` tématu:

    serviceBusService.deleteSubscription('MyTopic', 'HighMessages', function (error) {
        if(error) {
            console.log(error);
        }
    });

## <a name="next-steps"></a>Další kroky

Teď, když jste se naučili základy služby Bus témata, tyto odkazy vedou na další informace.

-   Přečtěte si článek [fronty, témata a][].
-   Přehled rozhraní API pro [SqlFilter][].
-   Navštivte [Azure SDK uzel][] úložiště na GitHub.

  [Azure SDK uzel]: https://github.com/Azure/azure-sdk-for-node
  [Azure klasické portálu]: https://manage.windowsazure.com
  [SqlFilter.SqlExpression]: http://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.sqlfilter.sqlexpression.aspx
  [Fronty, témata a předplatné]: service-bus-queues-topics-subscriptions.md
  [SqlFilter]: http://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.sqlfilter.aspx
  [Node.js cloudové služby]: ../cloud-services/cloud-services-nodejs-develop-deploy-app.md
  [Vytvoření a nasazení aplikace Node.js k webovému serveru Azure]: ../app-service-web/web-sites-nodejs-develop-deploy-mac.md
  [Node.js Cloudová služba úložiště]: ../cloud-services/cloud-services-nodejs-develop-deploy-app.md
  [Node.js webové aplikace s úložištěm]: ../cloud-services/storage-nodejs-use-table-storage-cloud-service-app.md
 
