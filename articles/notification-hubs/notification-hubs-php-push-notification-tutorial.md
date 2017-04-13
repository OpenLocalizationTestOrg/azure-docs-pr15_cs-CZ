<properties 
    pageTitle="Jak používat rozbočovače oznámení s PHP" 
    description="Naučte se používat Azure oznámení rozbočovače z PHP back-end." 
    services="notification-hubs" 
    documentationCenter="" 
    authors="ysxu" 
    manager="dwrede" 
    editor=""/>

<tags 
    ms.service="notification-hubs" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="php" 
    ms.devlang="php" 
    ms.topic="article" 
    ms.date="06/07/2016" 
    ms.author="yuaxu"/>

# <a name="how-to-use-notification-hubs-from-php"></a>Jak používat rozbočovače oznámení z PHP
[AZURE.INCLUDE [notification-hubs-backend-how-to-selector](../../includes/notification-hubs-backend-how-to-selector.md)]

Všechny funkce oznámení rozbočovače přístupný z Java/PHP/skutečné koncových pomocí rozhraní REST centrální oznámení způsobem popsaným v tématu MSDN [Oznámení rozbočovače REST API](http://msdn.microsoft.com/library/dn223264.aspx).

V tomto tématu ukážeme postup:

* Vytvoření ostatních klienta oznámení rozbočovače funkcí PHP;
* Postupujte podle [Začínáme kurz Get](notification-hubs-ios-apple-push-notification-apns-get-started.md) pro vaše mobilní platformu volbě implementace do back-end uspořádané v PHP.

## <a name="client-interface"></a>Rozhraní klienta
Hlavní klientského rozhraní umožňují stejné metody, které jsou k dispozici v [.NET oznámení rozbočovače SDK](http://msdn.microsoft.com/library/jj933431.aspx), bude možné přímo překladu výukové programy a ukázky aktuálně dostupné na tomto webu a přispět tak, že na komunitu na Internetu.

Můžete najít všechny kód k dispozici ve [ZBÝVAJÍCÍ PHP obálky vzorku].

Chcete-li například vytvořit klienta:

    $hub = new NotificationHub("connection string", "hubname"); 

Odeslání iOS nativní oznámení:
    
    $notification = new Notification("apple", '{"aps":{"alert": "Hello!"}}');
    $hub->sendNotification($notification, null);

## <a name="implementation"></a>Implementace
Pokud jste už není, postupujte podle našeho [Začínáme kurz získat] až části poslední, kde máte implementovat back-end.
Také chcete-li můžete pomocí kódu z [PHP ZBÝVAJÍCÍ obálky vzorku] a přejít přímo do části [Dokončeno kurzu](#complete-tutorial) .

Podrobnosti o implementovat úplné obálky ZBÝVAJÍCÍ najdete na [webu MSDN](http://msdn.microsoft.com/library/dn530746.aspx). V této části jsme popisují PHP provádění hlavní kroky potřebné pro přístup k koncové body ZBÝVAJÍCÍ rozbočovače oznámení:

1. Analyzovat připojovacího řetězce
2. Generování token se tak mohli ověřovat
3. Provedení volání HTTP

### <a name="parse-the-connection-string"></a>Analyzovat připojovacího řetězce

Tady je třída hlavní provádění klienta, jejichž konstruktor, které rozdělí připojovací řetězec:

    class NotificationHub {
        const API_VERSION = "?api-version=2013-10";
    
        private $endpoint;
        private $hubPath;
        private $sasKeyName;
        private $sasKeyValue;
    
        function __construct($connectionString, $hubPath) {
            $this->hubPath = $hubPath;
    
            $this->parseConnectionString($connectionString);
        }
    
        private function parseConnectionString($connectionString) {
            $parts = explode(";", $connectionString);
            if (sizeof($parts) != 3) {
                throw new Exception("Error parsing connection string: " . $connectionString);
            }
    
            foreach ($parts as $part) {
                if (strpos($part, "Endpoint") === 0) {
                    $this->endpoint = "https" . substr($part, 11);
                } else if (strpos($part, "SharedAccessKeyName") === 0) {
                    $this->sasKeyName = substr($part, 20);
                } else if (strpos($part, "SharedAccessKey") === 0) {
                    $this->sasKeyValue = substr($part, 16);
                }
            }
        }
    }


### <a name="create-security-token"></a>Vytvoření tokenu zabezpečení
Podrobnosti o vystavení tokenu zabezpečení jsou dostupné [tady](http://msdn.microsoft.com/library/dn495627.aspx).
Podle pokynů pro musí být přidán třídy **NotificationHub** vytvořit token podle URI aktuální žádost a její přihlašovací údaje extrahovaných z připojovací řetězec.

    private function generateSasToken($uri) {
        $targetUri = strtolower(rawurlencode(strtolower($uri)));

        $expires = time();
        $expiresInMins = 60;
        $expires = $expires + $expiresInMins * 60;
        $toSign = $targetUri . "\n" . $expires;

        $signature = rawurlencode(base64_encode(hash_hmac('sha256', $toSign, $this->sasKeyValue, TRUE)));

        $token = "SharedAccessSignature sr=" . $targetUri . "&sig="
                    . $signature . "&se=" . $expires . "&skn=" . $this->sasKeyName;

        return $token;
    }

### <a name="send-a-notification"></a>Odeslání oznámení
Dejte nám definujte nejdřív třídy představující oznámení.

    class Notification {
        public $format;
        public $payload;
    
        # array with keynames for headers
        # Note: Some headers are mandatory: Windows: X-WNS-Type, WindowsPhone: X-NotificationType
        # Note: For Apple you can set Expiry with header: ServiceBusNotification-ApnsExpiry in W3C DTF, YYYY-MM-DDThh:mmTZD (for example, 1997-07-16T19:20+01:00).
        public $headers;
    
        function __construct($format, $payload) {
            if (!in_array($format, ["template", "apple", "windows", "gcm", "windowsphone"])) {
                throw new Exception('Invalid format: ' . $format);
            }
    
            $this->format = $format;
            $this->payload = $payload;
        }
    }

Tento předmětu je kontejner textu nativní oznámení nebo sadu vlastností v případě oznámení šablony a sady záhlaví, která obsahuje formát (nativní platformy nebo šablona) a vlastnosti specifické pro platformu (jako je Apple vypršení platnosti vlastnost a WNS záhlaví).

Získáte [si přečtěte následující dokumentaci oznámení rozbočovače REST API](http://msdn.microsoft.com/library/dn495827.aspx) platformy konkrétní oznámení a formáty a pro k dispozici všechny možnosti.

Ozbrojených s této třídy, můžete teď psaní odeslat oznámení metody uvnitř třídy **NotificationHub** .

    public function sendNotification($notification, $tagsOrTagExpression="") {
        if (is_array($tagsOrTagExpression)) {
            $tagExpression = implode(" || ", $tagsOrTagExpression);
        } else {
            $tagExpression = $tagsOrTagExpression;
        }

        # build uri
        $uri = $this->endpoint . $this->hubPath . "/messages" . NotificationHub::API_VERSION;
        $ch = curl_init($uri);

        if (in_array($notification->format, ["template", "apple", "gcm"])) {
            $contentType = "application/json";
        } else {
            $contentType = "application/xml";
        }

        $token = $this->generateSasToken($uri);

        $headers = [
            'Authorization: '.$token,
            'Content-Type: '.$contentType,
            'ServiceBusNotification-Format: '.$notification->format
        ];

        if ("" !== $tagExpression) {
            $headers[] = 'ServiceBusNotification-Tags: '.$tagExpression;
        }

        # add headers for other platforms
        if (is_array($notification->headers)) {
            $headers = array_merge($headers, $notification->headers);
        }
        
        curl_setopt_array($ch, array(
            CURLOPT_POST => TRUE,
            CURLOPT_RETURNTRANSFER => TRUE,
            CURLOPT_SSL_VERIFYPEER => FALSE,
            CURLOPT_HTTPHEADER => $headers,
            CURLOPT_POSTFIELDS => $notification->payload
        ));

        // Send the request
        $response = curl_exec($ch);

        // Check for errors
        if($response === FALSE){
            throw new Exception(curl_error($ch));
        }

        $info = curl_getinfo($ch);

        if ($info['http_code'] <> 201) {
            throw new Exception('Error sending notificaiton: '. $info['http_code'] . ' msg: ' . $response);
        }
    } 

Výše uvedené metody pošlete žádost HTTP POST koncový bod /messages oznámení rozbočovače s správné textu a záhlaví odeslat oznámení.

##<a name="complete-tutorial"></a>Použití výukového
Nyní můžete provést kurz seznámení tak, že oznámení z PHP back-end.

Inicializace oznámení rozbočovače klienta (náhradní řetězce a rozbočovači název připojení jako instructed v [Začínáme kurz Get]):

    $hub = new NotificationHub("connection string", "hubname"); 

Přidejte do odesílání kód v závislosti na vaše mobilní platformu.

### <a name="windows-store-and-windows-phone-81-non-silverlight"></a>Pro Windows Store a Windows Phone 8.1 (bez Silverlight)

    $toast = '<toast><visual><binding template="ToastText01"><text id="1">Hello from PHP!</text></binding></visual></toast>';
    $notification = new Notification("windows", $toast);
    $notification->headers[] = 'X-WNS-Type: wns/toast';
    $hub->sendNotification($notification, null);

### <a name="ios"></a>iOS

    $alert = '{"aps":{"alert":"Hello from PHP!"}}';
    $notification = new Notification("apple", $alert);
    $hub->sendNotification($notification, null);

### <a name="android"></a>Android
    $message = '{"data":{"msg":"Hello from PHP!"}}';
    $notification = new Notification("gcm", $message);
    $hub->sendNotification($notification, null);

### <a name="windows-phone-80-and-81-silverlight"></a>Windows Phone 8.0 a 8.1 Silverlight

    $toast = '<?xml version="1.0" encoding="utf-8"?>' .
                '<wp:Notification xmlns:wp="WPNotification">' .
                   '<wp:Toast>' .
                        '<wp:Text1>Hello from PHP!</wp:Text1>' .
                   '</wp:Toast> ' .
                '</wp:Notification>';
    $notification = new Notification("windowsphone", $toast);
    $notification->headers[] = 'X-WindowsPhone-Target : toast';
    $notification->headers[] = 'X-NotificationClass : 2';
    $hub->sendNotification($notification, null);


### <a name="kindle-fire"></a>Kindle Fire
    $message = '{"data":{"msg":"Hello from PHP!"}}';
    $notification = new Notification("adm", $message);
    $hub->sendNotification($notification, null);

Spuštění PHP kódu, má vracet teď oznámení zobrazená v cílové zařízení.


## <a name="next-steps"></a>Další kroky
V tomto tématu jsme ukázal, jak vytvořit jednoduchý Java ZBÝVAJÍCÍ klienta pro rozbočovače oznámení. Tady můžete provést tyto akce:

* Stáhněte si úplné [PHP ZBÝVAJÍCÍ obálky vzorku], ke které obsahuje tento kód výše.
* Přečtěte si víc o oznámení rozbočovače značení funkce [novinkách kurz]
* Další informace o předání oznámení jednotlivým uživatelům v [upozornit uživatele kurzu]

Další informace najdete v tématu taky [Středisko pro vývojáře PHP](/develop/php/).

[Ukázka PHP ZBÝVAJÍCÍ obálky]: https://github.com/Azure/azure-notificationhubs-samples/tree/master/notificationhubs-rest-php
[Získání Začínáme kurz]: http://azure.microsoft.com/documentation/articles/notification-hubs-ios-get-started/
 
