<properties 
    pageTitle="Použití služby Bus témata s PHP | Microsoft Azure" 
    description="Naučte se používat službu Bus témata s PHP v Azure." 
    services="service-bus" 
    documentationCenter="php" 
    authors="sethmanheim" 
    manager="timlt" 
    editor=""/>

<tags 
    ms.service="service-bus" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="PHP" 
    ms.topic="article" 
    ms.date="10/14/2016" 
    ms.author="sethm"/>


# <a name="how-to-use-service-bus-topics-and-subscriptions"></a>Používání služby Bus témata a předplatného

[AZURE.INCLUDE [service-bus-selector-topics](../../includes/service-bus-selector-topics.md)]

Tento článek popisuje, jak používat službu Bus témata a předplatná. Vzorky psaných PHP a použít v [Azure SDK PHP](../php-download-sdk.md). Scénáře nichž se uplatní patří **vytváření témata a předplatná**, **vytváření filtrů předplatného**, **odesílání zpráv na téma**, **přijímat zprávy z předplatného**a **Odstranění témata a předplatná**.

[AZURE.INCLUDE [howto-service-bus-topics](../../includes/howto-service-bus-topics.md)]

## <a name="create-a-php-application"></a>Vytvoření aplikace PHP

Pouze požadavku na vytvoření aplikace PHP, který má přístup ke službě objektů Blob Azure je neodkazuje tříd v [Azure SDK PHP](../php-download-sdk.md) z vašeho kódu. Libovolné vývojového nástroje slouží k vytvoření aplikace nebo Poznámkový blok.

> [AZURE.NOTE] Instalace PHP musí mít taky [OpenSSL rozšíření](http://php.net/openssl) nainstalované a povolené.

Tento článek popisuje, jak používat funkce služby, které lze volat aplikace PHP místně, nebo kód spuštěný v rámci Azure web role, kolegy nebo Web.

## <a name="get-the-azure-client-libraries"></a>Získání Azure klienta knihoven

[AZURE.INCLUDE [get-client-libraries](../../includes/get-client-libraries.md)]

## <a name="configure-your-application-to-use-service-bus"></a>Konfigurace aplikace pomocí služby Bus

Použití rozhraní API Bus služby:

1. Odkaz na soubor zavaděč pomocí [require_once] [ require-once] údajů.
2. Odkaz na všechny třídy, které používáte.

Následující příklad ukazuje, jak vložit soubor zavaděč a odkaz na třídu **ServiceBusService** .

> [AZURE.NOTE] V tomto příkladu (a další příklady v tomto článku) předpokládá, že jste nainstalovali knihoven PHP klienta pro Azure prostřednictvím autora. Pokud jste si nainstalovali knihoven ručně nebo jako balíček HRUŠNÍ, musí odkazovat na **WindowsAzure.php** zavaděč soubor.

```
require_once 'vendor\autoload.php';
use WindowsAzure\Common\ServicesBuilder;
```

V následujících příkladech `require_once` údajů vždy se zobrazí, ale odkazuje pouze třídy potřebné třeba provádět.

## <a name="set-up-a-service-bus-connection"></a>Vytvoření připojení služby Bus

Pro vytvoření instance služby Bus klienta musíte nejdřív máte platný připojovací řetězec v tomto formátu:

```
Endpoint=[yourEndpoint];SharedSecretIssuer=[Default Issuer];SharedSecretValue=[Default Key]
```

Kde `Endpoint` je obvykle formátu `https://[yourNamespace].servicebus.windows.net`.

Vytvoření libovolného Azure služby klienta musíte použít třídu **ServicesBuilder** . Můžeš:

* Předáte připojovací řetězec přímo.
* Zjistit víc externích zdrojů pro připojovací řetězec pomocí **CloudConfigurationManager (CCM)** :
    * Ve výchozím nastavení je součástí Podpora externích zdrojů – proměnné prostředí.
    * Přidávání nových zdrojů prodloužením **ConnectionStringSource** předmětu.

Příklady uvedené v tomto poli připojovací řetězec předán přímo.

```
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;
    
$connectionString = "Endpoint=[yourEndpoint];SharedSecretIssuer=[Default Issuer];SharedSecretValue=[Default Key]";

$serviceBusRestProxy = ServicesBuilder::getInstance()->createServiceBusService($connectionString);
```

## <a name="create-a-topic"></a>Vytvoření tematického

Můžete provádět operace správy témat Bus služby prostřednictvím **ServiceBusRestProxy** předmětu. Objekt **ServiceBusRestProxy** je vytvořen pomocí metody factory **ServicesBuilder::createServiceBusService** s příslušnou připojovací řetězec, které je zahrnuta tokenu oprávnění ke správě ho.

Následující příklad ukazuje, jak vytvořit instanci **ServiceBusRestProxy** a volání **ServiceBusRestProxy -> createTopic** k vytvoření tematického s názvem `mytopic` v rámci `MySBNamespace` názvů:

```
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;
use WindowsAzure\Common\ServiceException;
use WindowsAzure\ServiceBus\Models\TopicInfo;
    
// Create Service Bus REST proxy.
$serviceBusRestProxy = ServicesBuilder::getInstance()->createServiceBusService($connectionString);
    
try {       
    // Create topic.
    $topicInfo = new TopicInfo("mytopic");
    $serviceBusRestProxy->createTopic($topicInfo);
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here: 
    // http://msdn.microsoft.com/library/windowsazure/dd179357
    $code = $e->getCode();
    $error_message = $e->getMessage();
    echo $code.": ".$error_message."<br />";
}
```

> [AZURE.NOTE] Můžete použít `listTopics` metoda na `ServiceBusRestProxy` objektů zaškrtněte, pokud téma se zadaným názvem už existuje v rámci služby názvů.

## <a name="create-a-subscription"></a>Vytvoření předplatného

Téma předplatná také vytvoříte pomocí metody **ServiceBusRestProxy -> createSubscription** . Předplatné se s názvem a může obsahovat volitelné filtr, který omezuje sadu zpráv do fronty virtuální předplatného.

### <a name="create-a-subscription-with-the-default-matchall-filter"></a>Vytvoření předplatné s filtrem výchozí (MatchAll)

Filtr **MatchAll** je výchozí filtr, který se používá, pokud zadaný žádný filtr při vytvoření nové předplatné. Při použití filtru **MatchAll** všech zpráv publikovaných na téma jsou umístěny ve frontě virtuální předplatného. Následující příklad vytvoří předplatné s názvem "mysubscription" a používá jako výchozí **MatchAll** filtr.

```
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;
use WindowsAzure\Common\ServiceException;
use WindowsAzure\ServiceBus\Models\SubscriptionInfo;

// Create Service Bus REST proxy.
$serviceBusRestProxy = ServicesBuilder::getInstance()->createServiceBusService($connectionString);
    
try {
    // Create subscription.
    $subscriptionInfo = new SubscriptionInfo("mysubscription");
    $serviceBusRestProxy->createSubscription("mytopic", $subscriptionInfo);
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here: 
    // http://msdn.microsoft.com/library/azure/dd179357
    $code = $e->getCode();
    $error_message = $e->getMessage();
    echo $code.": ".$error_message."<br />";
}
```

### <a name="create-subscriptions-with-filters"></a>Vytvoření předplatného s filtry

Můžete taky nastavit filtry, které umožňují určit, které zprávy poslané na téma by se měly objevit během určitého tématu předplatné. Flexibilní typ filtru nepodporuje pro předplatná je **SqlFilter**, které podmnožinu SQL92. Filtry SQL pracují s vlastnostmi zprávy, které jsou publikované na téma. Další informace o SqlFilters najdete v tématu [SqlFilter.SqlExpression vlastnost][sqlfilter].

> [AZURE.NOTE] Všechna pravidla na předplatné zpracování příchozích zpráv nezávisle na sobě, přidání jednotlivých zpráv výsledek k předplatnému. Kromě toho každé nové předplatné má výchozí **pravidla** objekt s filtrem, který přidá všechny zprávy z tématu k předplatnému. Aby se zprávy pouze odpovídajících filtru, musíte odebrat výchozí pravidlo. Odebrání výchozí pravidlo pomocí `ServiceBusRestProxy->deleteRule` metody.

Následující příklad vytvoří předplatné s názvem **HighMessages** s **SqlFilter** pouze vybere zprávy, které mají vlastních vlastností **MessageNumber** větší než 3 (viz téma [odesílat zprávy na téma](#send-messages-to-a-topic) informace o přidání vlastních vlastností zprávy):

```
$subscriptionInfo = new SubscriptionInfo("HighMessages");
$serviceBusRestProxy->createSubscription("mytopic", $subscriptionInfo);

$serviceBusRestProxy->deleteRule("mytopic", "HighMessages", '$Default');

$ruleInfo = new RuleInfo("HighMessagesRule");
$ruleInfo->withSqlFilter("MessageNumber > 3");
$ruleResult = $serviceBusRestProxy->createRule("mytopic", "HighMessages", $ruleInfo);
```

Poznámka: Tento kód vyžaduje použití další oboru názvů: `WindowsAzure\ServiceBus\Models\SubscriptionInfo`.

Následující příklad vytvoří podobně předplatné s názvem **LowMessages** s **SqlFilter** pouze vybere zprávy, které mají **MessageNumber** vlastnost menším větší nebo rovnu 3:

```
$subscriptionInfo = new SubscriptionInfo("LowMessages");
$serviceBusRestProxy->createSubscription("mytopic", $subscriptionInfo);

$serviceBusRestProxy->deleteRule("mytopic", "LowMessages", '$Default');

$ruleInfo = new RuleInfo("LowMessagesRule");
$ruleInfo->withSqlFilter("MessageNumber <= 3");
$ruleResult = $serviceBusRestProxy->createRule("mytopic", "LowMessages", $ruleInfo);
```

Teď, když je odeslána zpráva `mytopic` tématu je vždy dostane příjemce, nemáte předplatné `mysubscription` předplatného a selektivně doručeno příjemce, nemáte předplatné `HighMessages` a `LowMessages` předplatná (v závislosti na obsah zprávy).

## <a name="send-messages-to-a-topic"></a>Odeslání zprávy na téma

Zaslání zprávy na téma Bus služby aplikace volá metodu **ServiceBusRestProxy -> sendTopicMessage** . Následující kód ukazuje, jak odeslat zprávu `mytopic` téma dřív vytvořili v `MySBNamespace` obor názvů služby.

```
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;
use WindowsAzure\Common\ServiceException;
use WindowsAzure\ServiceBus\Models\BrokeredMessage;

// Create Service Bus REST proxy.
$serviceBusRestProxy = ServicesBuilder::getInstance()->createServiceBusService($connectionString);
        
try {
    // Create message.
    $message = new BrokeredMessage();
    $message->setBody("my message");
    
    // Send message.
    $serviceBusRestProxy->sendTopicMessage("mytopic", $message);
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here: 
    // http://msdn.microsoft.com/library/azure/hh780775
    $code = $e->getCode();
    $error_message = $e->getMessage();
    echo $code.": ".$error_message."<br />";
}
```

Zprávy zasílané do služby Bus témata jsou instancemi tříd **BrokeredMessage** . **BrokeredMessage** objekty mají sadu standardní vlastnosti a metody (například **getLabel** **getTimeToLive**, **setLabel**a **setTimeToLive**), stejně jako vlastnosti, které lze použít pro uložení vlastní vlastnosti specifické pro aplikaci. Následující příklad ukazuje, jak k odesílání zpráv 5 test `mytopic` téma dřív vytvořili. Metodu **setProperty** slouží k přidání vlastních vlastností (`MessageNumber`) pro každé zprávy. Všimněte si, že `MessageNumber` hodnota vlastnost se liší podle u každé zprávy (můžete použijete tuto hodnotu k určení které předplatná zobrazit, jak je vidět v části [Vytvoření předplatné](#create-a-subscription) ):

```
for($i = 0; $i < 5; $i++){
    // Create message.
    $message = new BrokeredMessage();
    $message->setBody("my message ".$i);
            
    // Set custom property.
    $message->setProperty("MessageNumber", $i);
            
    // Send message.
    $serviceBusRestProxy->sendTopicMessage("mytopic", $message);
}
```

Služba Bus témata podpory pro maximální velikost 256 KB [Standardní osy](service-bus-premium-messaging.md) a 1 MB [Premium osy](service-bus-premium-messaging.md). Záhlaví obsahuje vlastností standardních a vlastních aplikací, může mít maximální velikosti doručovaných 64 KB. Tam bez omezení počtu zpráv v tématu je ale zakončení na celkové velikosti zprávy uskutečňuje podle tématu. Tento horní mez na téma velikost je 5 GB. Další informace o kvótách najdete v článku [služby Bus kvóty][].

## <a name="receive-messages-from-a-subscription"></a>Přijímání zpráv z předplatného

Nejlepší způsob, jak přijímat zprávy od předplatné je použijte metodu **ServiceBusRestProxy -> receiveSubscriptionMessage** . Přijatých zpráv, můžete pracovat v dvěma různými způsoby: **ReceiveAndDelete** (výchozí) a **PeekLock**.

Použití režimu **ReceiveAndDelete** přijímání při operaci jeden snímek; To znamená když služby Bus obdrží žádost o přečtení zprávy v předplatné, ji označí zprávu jako právě spotřebované a vrátí do aplikace. Režim **ReceiveAndDelete** je nejjednodušší modelu a je nejvhodnější pro scénáře, které aplikace nevadí vám není zpracování zprávy v případě se nepovede. Pokud chcete porozumět tomu, zvažte scénáři, ve kterém příjemci problémy žádost přijmout a potom dojde k chybě před zpracování. Protože služby Bus bude označené zprávy jako spotřebované, a když aplikace restartuje, nebude zahájen jinými zprávy znovu ho bude uniknout zprávu, která byla spotřebované množství před k chybě.

V režimu **PeekLock** přijímat zprávy bude operace dva stupně, která umožní podpory aplikace, které nelze tolerovat chybějící zprávy. Když služby Bus obdrží žádost, zjistí další zprávu do být spotřebované množství, ji nechcete, aby ostatní uživatelé jeho přijetí zamkne a vrátí hodnotu aplikace. Po aplikace dokončení zpracování zprávy (nebo problémy se spolehlivým ukládá pro budoucí zpracování), provede druhé fáze procesu přijímání **ServiceBusRestProxy -> deleteMessage**předáním přijaté zprávy. Když služby Bus uvidí **deleteMessage** volání, bude označte zprávu jako právě spotřebované a odebrat z fronty.

Následující příklad ukazuje, jak zobrazit a zpracování zprávy pomocí **PeekLock** mode (ne výchozí režim). 

```
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;
use WindowsAzure\Common\ServiceException;
use WindowsAzure\ServiceBus\Models\ReceiveMessageOptions;

// Create Service Bus REST proxy.
$serviceBusRestProxy = ServicesBuilder::getInstance()->createServiceBusService($connectionString);
        
try {
    // Set receive mode to PeekLock (default is ReceiveAndDelete)
    $options = new ReceiveMessageOptions();
    $options->setPeekLock();
    
    // Get message.
    $message = $serviceBusRestProxy->receiveSubscriptionMessage("mytopic", "mysubscription", $options);

    echo "Body: ".$message->getBody()."<br />";
    echo "MessageID: ".$message->getMessageId()."<br />";
        
    /*---------------------------
        Process message here.
    ----------------------------*/
        
    // Delete message. Not necessary if peek lock is not set.
    echo "Deleting message...<br />";
    $serviceBusRestProxy->deleteMessage($message);
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here:
    // http://msdn.microsoft.com/library/azure/hh780735
    $code = $e->getCode();
    $error_message = $e->getMessage();
    echo $code.": ".$error_message."<br />";
}
```

## <a name="how-to-handle-application-crashes-and-unreadable-messages"></a>Postup: zpracování chyb aplikace a čitelný zprávy

Služba Bus poskytuje funkce vám řádně obnovení z chyb v aplikaci nebo potíže zpracování zprávy. Pokud je příjemce aplikace nelze ji zpracovat z nějakého důvodu, ho metodu **unlockMessage** zavolat na přijaté zprávy (místo **deleteMessage** metoda). To způsobí služby Bus odemknout zprávy ve frontě a jeho Příprava získaná znovu stejné náročný aplikací nebo jinou náročný aplikací.

Je také časového limitu zprávy uzamčené ve frontě a pokud aplikaci se nezdaří zpracování zprávy před časový limit zámku vyprší platnost (například pokud způsobí zhroucení aplikace), potom služby Bus bude automaticky odemknout zprávu a jeho Příprava získaná znovu.

V případě, že po zpracování zprávy, ale před zadáním požadavku **deleteMessage** způsobí zhroucení aplikace, pak zpráva bude být znovu dodány aplikaci po restartování. Toto je někdy označovány jako **Aspoň jednou zpracování**; To znamená každou zprávu zpracovat aspoň jednou, ale v některých případech může být znovu dodány stejnou zprávu. Pokud scénáře nelze tolerovat duplicitní zpracování, pak vývojáři bychom přidat další logiku aplikacím řešit duplicitní zprávu doručení. Dosahuje se často, metodou **getMessageId** zprávy, která konstantní přes neúspěšných pokusech o doručení.

## <a name="delete-topics-and-subscriptions"></a>Odstranění témata a předplatného

Téma nebo předplatného, můžete odstranit **ServiceBusRestProxy -> deleteTopic** nebo metody **ServiceBusRestProxy -> deleteSubscripton** v tomto pořadí. Všimněte si, že odstranění tématu odstraněny také žádné předplatné, které jsou registrované s tématem.

Následující příklad ukazuje, jak odstranit téma s názvem `mytopic` a jeho registrovaných předplatné.

```
require_once 'vendor/autoload.php';

use WindowsAzure\ServiceBus\ServiceBusService;
use WindowsAzure\ServiceBus\ServiceBusSettings;
use WindowsAzure\Common\ServiceException;

// Create Service Bus REST proxy.
$serviceBusRestProxy = ServicesBuilder::getInstance()->createServiceBusService($connectionString);
    
try {       
    // Delete topic.
    $serviceBusRestProxy->deleteTopic("mytopic");
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here: 
    // http://msdn.microsoft.com/library/azure/dd179357
    $code = $e->getCode();
    $error_message = $e->getMessage();
    echo $code.": ".$error_message."<br />";
}
```

Pomocí metody **deleteSubscription** můžete odstranit předplatné nezávisle na sobě:

```
$serviceBusRestProxy->deleteSubscription("mytopic", "mysubscription");
```

## <a name="next-steps"></a>Další kroky

Teď, když jste se naučili základy služby Bus fronty, [fronty, témata a předplatné][] Další informace najdete.

[Fronty, témata a předplatné]: service-bus-queues-topics-subscriptions.md
[sqlfilter]: http://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.sqlfilter.sqlexpression.aspx
[require-once]: http://php.net/require_once
[Služba Bus kvót]: service-bus-quotas.md
