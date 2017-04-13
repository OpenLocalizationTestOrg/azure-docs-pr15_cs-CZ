<properties 
    pageTitle="Použití služby Bus fronty s PHP | Microsoft Azure" 
    description="Naučte se používat službu Bus fronty v Azure. Ukázky napsané v PHP." 
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
    ms.date="10/04/2016" 
    ms.author="sethm"/>

# <a name="how-to-use-service-bus-queues"></a>Jak používat službu Bus fronty

[AZURE.INCLUDE [service-bus-selector-queues](../../includes/service-bus-selector-queues.md)]

Tato příručka, uvidíte, jak používat službu Bus fronty. Vzorky psaných PHP a použít v [Azure SDK PHP](../php-download-sdk.md). Scénáře nichž se uplatní zahrnují **vytváření fronty** **odesílání a přijímání zpráv**a **odstranění fronty**.

[AZURE.INCLUDE [howto-service-bus-queues](../../includes/howto-service-bus-queues.md)]

## <a name="create-a-php-application"></a>Vytvoření aplikace PHP

Pouze požadavku na vytvoření aplikace PHP, který má přístup ke službě objektů Blob Azure je odkazování tříd v [Azure SDK PHP](../php-download-sdk.md) z vašeho kódu. Libovolné vývojového nástroje slouží k vytvoření aplikace nebo Poznámkový blok.

> [AZURE.NOTE] Instalace PHP musí mít taky [OpenSSL rozšíření](http://php.net/openssl) nainstalované a povolené.

V této příručce použije funkce služby, které lze volat z aplikace PHP místně, nebo kód spuštěný v rámci Azure web role, kolegy nebo Web.

## <a name="get-the-azure-client-libraries"></a>Získání Azure klienta knihoven

[AZURE.INCLUDE [get-client-libraries](../../includes/get-client-libraries.md)]

## <a name="configure-your-application-to-use-service-bus"></a>Konfigurace aplikace pomocí služby Bus

Použití služby Bus fronty rozhraní API, postupujte takto:

1. Odkaz na soubor zavaděč pomocí [require_once] [ require_once] údajů.
2. Odkaz na všechny třídy, které používáte.

Následující příklad ukazuje, jak vložit soubor zavaděč a odkaz na třídu **ServicesBuilder** .

> [AZURE.NOTE] V tomto příkladu (a další příklady v tomto článku) předpokládá, že jste nainstalovali knihoven PHP klienta pro Azure prostřednictvím autora. Pokud jste si nainstalovali knihoven ručně nebo jako balíček HRUŠNÍ, musí odkazovat na **WindowsAzure.php** zavaděč soubor.

```
require_once 'vendor/autoload.php';
use WindowsAzure\Common\ServicesBuilder;
```

V následujících příkladech `require_once` údajů vždy se zobrazí, ale odkazuje pouze třídy potřebné třeba provádět.

## <a name="set-up-a-service-bus-connection"></a>Vytvoření připojení služby Bus

Pro vytvoření instance služby Bus klienta, musíte nejdřív máte platný připojovací řetězec v tomto formátu:

```
Endpoint=[yourEndpoint];SharedSecretIssuer=[Default Issuer];SharedSecretValue=[Default Key]
```

**Koncový bod** se obvykle formátu `[yourNamespace].servicebus.windows.net`.

Vytvoření libovolného Azure služby klienta musíte použít třídu **ServicesBuilder** . Můžeš:

* Předáte připojovací řetězec přímo.
* Zjistit víc externích zdrojů pro připojovací řetězec pomocí **CloudConfigurationManager (CCM)** :
    * Ve výchozím nastavení součástí Podpora externích zdrojů – proměnné prostředí
    * Přidávání nových zdrojů prodloužením **ConnectionStringSource** třídy

Příklady uvedené v tomto poli budou přímo předaný připojovací řetězec.

```
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;

$connectionString = "Endpoint=[yourEndpoint];SharedSecretIssuer=[Default Issuer];SharedSecretValue=[Default Key]";

$serviceBusRestProxy = ServicesBuilder::getInstance()->createServiceBusService($connectionString);
```

## <a name="how-to-create-a-queue"></a>Postup: vytvoření fronty

Můžete provádět operace správy fronty Bus služby prostřednictvím **ServiceBusRestProxy** předmětu. Objekt **ServiceBusRestProxy** je vytvořen pomocí metody factory **ServicesBuilder::createServiceBusService** s příslušnou připojovací řetězec, které je zahrnuta tokenu oprávnění ke správě ho.

Následující příklad ukazuje, jak vytvořit instanci **ServiceBusRestProxy** a volání **ServiceBusRestProxy -> createQueue** k vytvoření fronty s názvem `myqueue` v rámci `MySBNamespace` obor názvů služby:

```
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;
use WindowsAzure\Common\ServiceException;
use WindowsAzure\ServiceBus\Models\QueueInfo;

// Create Service Bus REST proxy.
$serviceBusRestProxy = ServicesBuilder::getInstance()->createServiceBusService($connectionString);
    
try {
    $queueInfo = new QueueInfo("myqueue");
        
    // Create queue.
    $serviceBusRestProxy->createQueue($queueInfo);
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

> [AZURE.NOTE] Můžete použít `listQueues` metoda na `ServiceBusRestProxy` objektů zaškrtněte, pokud fronty se zadaným názvem už existuje v rámci obor.

## <a name="how-to-send-messages-to-a-queue"></a>Postup: odeslání zprávy do fronty

Odeslání zprávy do fronty Bus služby, aplikace volá metodu **ServiceBusRestProxy -> sendQueueMessage** . Následující kód ukazuje, jak odeslat zprávu `myqueue` fronty dřív vytvořili v `MySBNamespace` obor názvů služby.

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
    $serviceBusRestProxy->sendQueueMessage("myqueue", $message);
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here: 
    // http://msdn.microsoft.com/library/windowsazure/hh780775
    $code = $e->getCode();
    $error_message = $e->getMessage();
    echo $code.": ".$error_message."<br />";
}
```

Zprávy zasílané (a přijaté od) služby Bus fronty jsou instancemi tříd **BrokeredMessage** . **BrokeredMessage** objekty mají sadu standardní metod (například **getLabel** **getTimeToLive**, **setLabel**a **setTimeToLive**) a vlastnosti, které se používají pro ukládání vlastních vlastností specifické pro aplikaci a textu data libovolného aplikací.

Fronty Bus služby podpory maximální velikost 256 KB [Standardní osy](service-bus-premium-messaging.md) a 1 MB v [Premium osy](service-bus-premium-messaging.md). Záhlaví obsahuje vlastností standardních a vlastních aplikací, může mít maximální velikosti doručovaných 64 KB. Tam bez omezení počtu zprávu pozdržet ve frontě je ale zakončení na celkové velikosti zprávy uskutečňuje fronty. Tento horní mez na velikost fronty je 5 GB.

## <a name="how-to-receive-messages-from-a-queue"></a>Jak přijímat zprávy z fronty

Nejlepší způsob, jak přijímat zprávy z fronty je použijte metodu **ServiceBusRestProxy -> receiveQueueMessage** . Zprávy stavu dvěma různými způsoby: **ReceiveAndDelete** (výchozí) a **PeekLock**.

Použití režimu **ReceiveAndDelete** přijímání při operaci jeden snímek – to znamená, když služby Bus obdrží žádost o přečtení zprávy ve frontě, ji označí zprávu jako právě spotřebované množství a vrátí do aplikace. Režim **ReceiveAndDelete** je nejjednodušší modelu a je nejvhodnější pro scénáře, které aplikace nevadí vám není zpracování zprávy v případě se nepovede. Pokud chcete porozumět tomu, zvažte scénáři, ve kterém příjemci problémy žádost přijmout a potom dojde k chybě před zpracování. Protože služby Bus bude označené zprávy jako spotřebované, a když aplikace restartuje, nebude zahájen jinými zprávy znovu ho bude uniknout zprávu, která byla spotřebované množství před k chybě.

V režimu **PeekLock** přijímat zprávy bude operace dva stupně, která umožní podpory aplikace, které nelze tolerovat chybějící zprávy. Když služby Bus obdrží žádost, zjistí další zprávu do být spotřebované množství, uzamkne ho chcete zabránit doručování ho ostatní uživatelé a vrátí aplikaci. Po aplikace dokončení zpracování zprávy (nebo problémy se spolehlivým ukládá pro budoucí zpracování), provede druhé fáze procesu přijímání **ServiceBusRestProxy -> deleteMessage**předáním přijaté zprávy. Když služby Bus uvidí **deleteMessage** volání, bude označte zprávu jako právě spotřebované a odebrat z fronty.

Následující příklad ukazuje, jak zprávu o stavu a zpracovat v režimu **PeekLock** (ne výchozí režim).

```
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;
use WindowsAzure\Common\ServiceException;
use WindowsAzure\ServiceBus\Models\ReceiveMessageOptions;

// Create Service Bus REST proxy.
$serviceBusRestProxy = ServicesBuilder::getInstance()->createServiceBusService($connectionString);
        
try {
    // Set the receive mode to PeekLock (default is ReceiveAndDelete).
    $options = new ReceiveMessageOptions();
    $options->setPeekLock();
        
    // Receive message.
    $message = $serviceBusRestProxy->receiveQueueMessage("myqueue", $options);
    echo "Body: ".$message->getBody()."<br />";
    echo "MessageID: ".$message->getMessageId()."<br />";
        
    /*---------------------------
        Process message here.
    ----------------------------*/
        
    // Delete message. Not necessary if peek lock is not set.
    echo "Message deleted.<br />";
    $serviceBusRestProxy->deleteMessage($message);
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here:
    // http://msdn.microsoft.com/library/windowsazure/hh780735
    $code = $e->getCode();
    $error_message = $e->getMessage();
    echo $code.": ".$error_message."<br />";
}
```

## <a name="how-to-handle-application-crashes-and-unreadable-messages"></a>Postup: zpracování chyb aplikace a čitelný zprávy

Služba Bus poskytuje funkce vám řádně obnovení z chyb v aplikaci nebo potíže zpracování zprávy. Pokud je příjemce aplikace nelze ji zpracovat z nějakého důvodu, ho metodu **unlockMessage** zavolat na přijaté zprávy (místo **deleteMessage** metoda). To způsobí služby Bus odemknout zprávy ve frontě a jeho Příprava získaná znovu stejné náročný aplikací nebo jinou náročný aplikací.

Je také časového limitu zprávy uzamčené ve frontě a pokud aplikaci se nezdaří zpracování zprávy před časový limit zámku vyprší platnost (například pokud způsobí zhroucení aplikace), potom služby Bus bude automaticky odemknout zprávu a jeho Příprava získaná znovu.

V případě, že po zpracování zprávy, ale před zadáním požadavku **deleteMessage** způsobí zhroucení aplikace, pak zpráva bude být znovu dodány aplikaci po restartování. Toto je někdy označovány jako **Aspoň jednou zpracování**; To znamená každou zprávu zpracovat aspoň jednou, ale v některých případech může být znovu dodány stejnou zprávu. Pokud scénáře nelze tolerovat duplicitní zpracování, pak přidejte další použití logických operátorů k aplikacím zpracovávání doručení duplicitní zpráv je vhodné. Dosahuje se často, metodou **getMessageId** zprávy, která konstantní přes neúspěšných pokusech o doručení.

## <a name="next-steps"></a>Další kroky

Teď, když jste se naučili základy služby Bus fronty, [fronty, témata a předplatné][] Další informace najdete.

Další informace najdete taky v [Středisko pro vývojáře PHP](/develop/php/).

[Fronty, témata a předplatné]: service-bus-queues-topics-subscriptions.md
[require_once]: http://php.net/require_once

 
