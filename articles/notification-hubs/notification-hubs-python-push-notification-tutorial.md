<properties 
    pageTitle="Jak používat rozbočovače oznámení s Python" 
    description="Naučte se používat Azure oznámení rozbočovače z Python back-end." 
    services="notification-hubs" 
    documentationCenter="" 
    authors="ysxu"
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="notification-hubs" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="python" 
    ms.devlang="php" 
    ms.topic="article" 
    ms.date="06/29/2016" 
    ms.author="yuaxu"/>

# <a name="how-to-use-notification-hubs-from-python"></a>Jak používat rozbočovače oznámení z Python
[AZURE.INCLUDE [notification-hubs-backend-how-to-selector](../../includes/notification-hubs-backend-how-to-selector.md)]
        
Všechny funkce oznámení rozbočovače přístupný z Java/PHP/Python/skutečné serverová pomocí rozhraní REST centrální oznámení způsobem popsaným v tématu MSDN [Oznámení rozbočovače REST API](http://msdn.microsoft.com/library/dn223264.aspx).

> [AZURE.NOTE] To je ukázkový implementace odkaz pro provádění odešle oznámení v Python a není úředně podporované SDK Python centrální oznámení.
>
> V tomto příkladu je vytvořeno podle Python 3.4.

V tomto tématu ukážeme postup:

* Vytvoření ostatních klienta oznámení rozbočovače funkcí Python.
* Odešlete oznámení pomocí Python rozhraní REST API centrální oznámení. 
* Získáte výpis HTTP ZBÝVAJÍCÍ žádostí a odpovědí pro ladění vzdělávací účel. 

Můžete postupovat podle [Začínáme kurz Get](notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md) pro vaše mobilní platformu volbě implementace do back-end uspořádané v Python.

> [AZURE.NOTE] Obor výběru pouze omezenou o neodeslání oznámení a neprovádí všechny registrace správy.

## <a name="client-interface"></a>Rozhraní klienta
Hlavní klientského rozhraní můžete poskytnout stejné metody, které jsou k dispozici v [.NET oznámení rozbočovače SDK](http://msdn.microsoft.com/library/jj933431.aspx). To vám umožní přímo převést výukové programy a ukázky aktuálně dostupné na tomto webu a přispět tak, že na komunitu na Internetu.

Můžete najít všechny kód k dispozici ve [ZBÝVAJÍCÍ Python obálky vzorku].

Chcete-li například vytvořit klienta:

    isDebug = True
    hub = NotificationHub("myConnectionString", "myNotificationHubName", isDebug)
    
Odeslání oznámením Windows:
    
    wns_payload = """<toast><visual><binding template=\"ToastText01\"><text id=\"1\">Hello world!</text></binding></visual></toast>"""
    hub.send_windows_notification(wns_payload)
    
## <a name="implementation"></a>Implementace
Pokud jste už není, postupujte podle naše [Začínáme kurz získat] až části poslední, kde máte implementovat back-end.

Podrobnosti o implementovat úplné obálky ZBÝVAJÍCÍ najdete na [webu MSDN](http://msdn.microsoft.com/library/dn530746.aspx). V této části jsme popisují Python provádění hlavní kroky potřebné k přístupu koncové body ZBÝVAJÍCÍ rozbočovače oznámení a odeslat oznámení

1. Analyzovat připojovacího řetězce
2. Generování token se tak mohli ověřovat
3. Odeslání oznámení pomocí HTTP REST API

### <a name="parse-the-connection-string"></a>Analyzovat připojovacího řetězce

Tady je třída hlavní provádění klienta, jejichž konstruktor analyzuje připojovací řetězec:

    class NotificationHub:
        API_VERSION = "?api-version=2013-10"
        DEBUG_SEND = "&test"
    
        def __init__(self, connection_string=None, hub_name=None, debug=0):
            self.HubName = hub_name
            self.Debug = debug
    
            # Parse connection string
            parts = connection_string.split(';')
            if len(parts) != 3:
                raise Exception("Invalid ConnectionString.")
    
            for part in parts:
                if part.startswith('Endpoint'):
                    self.Endpoint = 'https' + part[11:]
                if part.startswith('SharedAccessKeyName'):
                    self.SasKeyName = part[20:]
                if part.startswith('SharedAccessKey'):
                    self.SasKeyValue = part[16:]


### <a name="create-security-token"></a>Vytvoření tokenu zabezpečení
Podrobnosti o vystavení tokenu zabezpečení jsou dostupné [tady](http://msdn.microsoft.com/library/dn495627.aspx).
Následující metody musí být přidán třídy **NotificationHub** vytvořit token podle URI aktuální žádost a její přihlašovací údaje extrahovaných z připojovací řetězec.

    @staticmethod
    def get_expiry():
        # By default returns an expiration of 5 minutes (=300 seconds) from now
        return int(round(time.time() + 300))

    @staticmethod
    def encode_base64(data):
        return base64.b64encode(data)

    def sign_string(self, to_sign):
        key = self.SasKeyValue.encode('utf-8')
        to_sign = to_sign.encode('utf-8')
        signed_hmac_sha256 = hmac.HMAC(key, to_sign, hashlib.sha256)
        digest = signed_hmac_sha256.digest()
        encoded_digest = self.encode_base64(digest)
        return encoded_digest

    def generate_sas_token(self):
        target_uri = self.Endpoint + self.HubName
        my_uri = urllib.parse.quote(target_uri, '').lower()
        expiry = str(self.get_expiry())
        to_sign = my_uri + '\n' + expiry
        signature = urllib.parse.quote(self.sign_string(to_sign))
        auth_format = 'SharedAccessSignature sig={0}&se={1}&skn={2}&sr={3}'
        sas_token = auth_format.format(signature, expiry, self.SasKeyName, my_uri)
        return sas_token

### <a name="send-a-notification-using-http-rest-api"></a>Odeslání oznámení pomocí HTTP REST API
Použití první, umožněte definovat třídy představující oznámení.

    class Notification:
        def __init__(self, notification_format=None, payload=None, debug=0):
            valid_formats = ['template', 'apple', 'gcm', 'windows', 'windowsphone', "adm", "baidu"]
            if not any(x in notification_format for x in valid_formats):
                raise Exception(
                    "Invalid Notification format. " +
                    "Must be one of the following - 'template', 'apple', 'gcm', 'windows', 'windowsphone', 'adm', 'baidu'")
    
            self.format = notification_format
            self.payload = payload
    
            # array with keynames for headers
            # Note: Some headers are mandatory: Windows: X-WNS-Type, WindowsPhone: X-NotificationType
            # Note: For Apple you can set Expiry with header: ServiceBusNotification-ApnsExpiry
            # in W3C DTF, YYYY-MM-DDThh:mmTZD (for example, 1997-07-16T19:20+01:00).
            self.headers = None

Tento předmětu je kontejner textu nativní oznámení nebo sadu vlastností pro nečekané oznámení šablonu sady záhlaví obsahuje vlastnosti specifické pro platformu (jako je Apple vypršení platnosti vlastnost a záhlaví WNS) a formátem (nativní platformy nebo šablonu).

Získáte [si přečtěte následující dokumentaci oznámení rozbočovače REST API](http://msdn.microsoft.com/library/dn495827.aspx) a formáty platformách konkrétní oznámení pro všechny dostupné možnosti.

Teď pomocí této třídy jsme můžete psát odeslat oznámení metody uvnitř třídy **NotificationHub** .

    def make_http_request(self, url, payload, headers):
        parsed_url = urllib.parse.urlparse(url)
        connection = http.client.HTTPSConnection(parsed_url.hostname, parsed_url.port)

        if self.Debug > 0:
            connection.set_debuglevel(self.Debug)
            # adding this querystring parameter gets detailed information about the PNS send notification outcome
            url += self.DEBUG_SEND
            print("--- REQUEST ---")
            print("URI: " + url)
            print("Headers: " + json.dumps(headers, sort_keys=True, indent=4, separators=(' ', ': ')))
            print("--- END REQUEST ---\n")

        connection.request('POST', url, payload, headers)
        response = connection.getresponse()

        if self.Debug > 0:
            # print out detailed response information for debugging purpose
            print("\n\n--- RESPONSE ---")
            print(str(response.status) + " " + response.reason)
            print(response.msg)
            print(response.read())
            print("--- END RESPONSE ---")

        elif response.status != 201:
            # Successful outcome of send message is HTTP 201 - Created
            raise Exception(
                "Error sending notification. Received HTTP code " + str(response.status) + " " + response.reason)

        connection.close()

    def send_notification(self, notification, tag_or_tag_expression=None):
        url = self.Endpoint + self.HubName + '/messages' + self.API_VERSION

        json_platforms = ['template', 'apple', 'gcm', 'adm', 'baidu']

        if any(x in notification.format for x in json_platforms):
            content_type = "application/json"
            payload_to_send = json.dumps(notification.payload)
        else:
            content_type = "application/xml"
            payload_to_send = notification.payload

        headers = {
            'Content-type': content_type,
            'Authorization': self.generate_sas_token(),
            'ServiceBusNotification-Format': notification.format
        }

        if isinstance(tag_or_tag_expression, set):
            tag_list = ' || '.join(tag_or_tag_expression)
        else:
            tag_list = tag_or_tag_expression

        # add the tags/tag expressions to the headers collection
        if tag_list != "":
            headers.update({'ServiceBusNotification-Tags': tag_list})

        # add any custom headers to the headers collection that the user may have added
        if notification.headers is not None:
            headers.update(notification.headers)

        self.make_http_request(url, payload_to_send, headers)

    def send_apple_notification(self, payload, tags=""):
        nh = Notification("apple", payload)
        self.send_notification(nh, tags)

    def send_gcm_notification(self, payload, tags=""):
        nh = Notification("gcm", payload)
        self.send_notification(nh, tags)

    def send_adm_notification(self, payload, tags=""):
        nh = Notification("adm", payload)
        self.send_notification(nh, tags)

    def send_baidu_notification(self, payload, tags=""):
        nh = Notification("baidu", payload)
        self.send_notification(nh, tags)

    def send_mpns_notification(self, payload, tags=""):
        nh = Notification("windowsphone", payload)

        if "<wp:Toast>" in payload:
            nh.headers = {'X-WindowsPhone-Target': 'toast', 'X-NotificationClass': '2'}
        elif "<wp:Tile>" in payload:
            nh.headers = {'X-WindowsPhone-Target': 'tile', 'X-NotificationClass': '1'}

        self.send_notification(nh, tags)

    def send_windows_notification(self, payload, tags=""):
        nh = Notification("windows", payload)

        if "<toast>" in payload:
            nh.headers = {'X-WNS-Type': 'wns/toast'}
        elif "<tile>" in payload:
            nh.headers = {'X-WNS-Type': 'wns/tile'}
        elif "<badge>" in payload:
            nh.headers = {'X-WNS-Type': 'wns/badge'}

        self.send_notification(nh, tags)

    def send_template_notification(self, properties, tags=""):
        nh = Notification("template", properties)
        self.send_notification(nh, tags)

Výše uvedené metody pošlete žádost HTTP POST /messages koncového bodu rozbočovače oznámení s správné textu a záhlaví odeslat oznámení.

### <a name="using-debug-property-to-enable-detailed-logging"></a>Použití ladění vlastnost Povolit podrobné protokolování
Povolení ladění vlastnost při inicializace centru oznámení zapíše podrobné protokolování informace o žádost HTTP a výpis odpověď, jakož i oznámení podrobné odeslat výsledek. Přidali jsme naposledy tato vlastnost s názvem [oznámení rozbočovače TestSend vlastnost](http://msdn.microsoft.com/library/microsoft.servicebus.notifications.notificationhubclient.enabletestsend.aspx) , která vrací podrobné informace o výsledku odeslat oznámení. Můžete ho - inicializace následujícími způsoby:

    hub = NotificationHub("myConnectionString", "myNotificationHubName", isDebug)

Žádost o odeslání oznámení centrální HTTP URL získá připojena jako výsledek "test" řetězce dotazu. 

##<a name="complete-tutorial"></a>Použití výukového
Nyní můžete provést kurz seznámení tak, že oznámení z Python back-end.

Inicializace oznámení rozbočovače klienta (náhradní řetězce a centrální název připojení jako instructed v [Začínáme kurz Get]):

    hub = NotificationHub("myConnectionString", "myNotificationHubName")

Přidejte do odesílání kód v závislosti na vaše mobilní platformu. V tomto příkladu taky přidá vyšší úroveň metody povolit odesílání oznámení podle platformy například send_windows_notification pro systém windows. send_apple_notification (pro apple) atd. 

### <a name="windows-store-and-windows-phone-81-non-silverlight"></a>Pro Windows Store a Windows Phone 8.1 (bez Silverlight)

    wns_payload = """<toast><visual><binding template=\"ToastText01\"><text id=\"1\">Test</text></binding></visual></toast>"""
    hub.send_windows_notification(wns_payload)

### <a name="windows-phone-80-and-81-silverlight"></a>Windows Phone 8.0 a 8.1 Silverlight

    hub.send_mpns_notification(toast)

### <a name="ios"></a>iOS

    alert_payload = {
        'data':
            {
                'msg': 'Hello!'
            }
    }
    hub.send_apple_notification(alert_payload)

### <a name="android"></a>Android
    gcm_payload = {
        'data':
            {
                'msg': 'Hello!'
            }
    }
    hub.send_gcm_notification(gcm_payload)

### <a name="kindle-fire"></a>Kindle Fire
    adm_payload = {
        'data':
            {
                'msg': 'Hello!'
            }
    }
    hub.send_adm_notification(adm_payload)

### <a name="baidu"></a>Baidu
    baidu_payload = {
        'data':
            {
                'msg': 'Hello!'
            }
    }
    hub.send_baidu_notification(baidu_payload)

Spuštění Python kódu, má vracet oznámení zobrazená v cílové zařízení.

## <a name="examples"></a>Příklady:

### <a name="enabling-debug-property"></a>Povolení ladění vlastnost
Pokud povolíte ladění příznak při inicializace NotificationHub pak uvidíte podrobný žádost HTTP a výpis odpověď, jakož i NotificationOutcome jako následující kde srozumitelný jaké HTTP záhlaví nejsou v žádosti a byla přijata odpověď jaké HTTP z centra oznámení:    ![][1]

Uvidíte podrobný oznámení centrální výsledek například 

- Pokud zpráva úspěšně odeslána ve službě nabízená oznámení. 
    
        <Outcome>The Notification was successfully sent to the Push Notification System</Outcome>

- Pokud došlo k žádné cíle nalezený pro každé nabízená oznámení pak pravděpodobně budete naleznete v následujících odpovědi (což znamená, že došlo k žádné registrace nalezené při oznámení pravděpodobně protože registrace mají některé neodpovídající značky)

        '<NotificationOutcome xmlns="http://schemas.microsoft.com/netservices/2010/10/servicebus/connect" xmlns:i="http://www.w3.org/2001/XMLSchema-instance"><Success>0</Success><Failure>0</Failure><Results i:nil="true"/></NotificationOutcome>'

### <a name="broadcast-toast-notification-to-windows"></a>Vysílání oznámením systému Windows 

Všimněte si, záhlaví, které získáte odeslala při odesílání vysílání oznámením klientovi Windows. 

    hub.send_windows_notification(wns_payload)

![][2]

### <a name="send-notification-specifying-a-tag-or-tag-expression"></a>Odeslání oznámení určení značky (značky výraz)

Všimněte si záhlaví HTTP značky, které se přidají do žádost HTTP (v následujícím příkladu jsme poslat oznámení pouze registrace s datové "sports")

    hub.send_windows_notification(wns_payload, "sports")

![][3]

### <a name="send-notification-specifying-multiple-tags"></a>Odeslání oznámení určující více značek

Všimněte si, jak záhlaví značky HTTP změní, když posílají více značek. 
    
    tags = {'sports', 'politics'}
    hub.send_windows_notification(wns_payload, tags)

![][4]

### <a name="templated-notification"></a>Šablony oznámení

Všimněte si, že se změní na záhlaví formát HTTP a textu datové odeslaný jako součást požadavek http:

**Na straně klienta - registrovaných šablon**

        var template =
                        @"<toast><visual><binding template=""ToastText01""><text id=""1"">$(greeting_en)</text></binding></visual></toast>";
            
**Na straně serveru – odeslání datové**
        
        template_payload = {'greeting_en': 'Hello', 'greeting_fr': 'Salut'}
        hub.send_template_notification(template_payload)

![][5]


## <a name="next-steps"></a>Další kroky
V tomto tématu jsme ukázal, jak vytvořit jednoduchý Python ZBÝVAJÍCÍ klienta pro rozbočovače oznámení. Tady můžete provést tyto akce:

* Stáhněte si úplné [Python ZBÝVAJÍCÍ obálky vzorku], ke které obsahuje tento kód výše.
* Přečtěte si víc o oznámení rozbočovače značení funkce [novinkách kurz]
* Přečtěte si víc o funkci šablony rozbočovače oznámení v [kurzu lokalizace příspěvků]

<!-- URLs -->
[Ukázka Python ZBÝVAJÍCÍ obálky]: https://github.com/Azure/azure-notificationhubs-samples/tree/master/notificationhubs-rest-python
[Získání Začínáme kurz]: http://azure.microsoft.com/documentation/articles/notification-hubs-windows-store-dotnet-get-started/
[Nejnovější příspěvky kurz]: http://azure.microsoft.com/documentation/articles/notification-hubs-windows-store-dotnet-send-breaking-news/
[Lokalizace kurz příspěvků]: http://azure.microsoft.com/documentation/articles/notification-hubs-windows-store-dotnet-send-localized-breaking-news/

<!-- Images. -->
[1]: ./media/notification-hubs-python-backend-how-to/DetailedLoggingInfo.png
[2]: ./media/notification-hubs-python-backend-how-to/BroadcastScenario.png
[3]: ./media/notification-hubs-python-backend-how-to/SendWithOneTag.png
[4]: ./media/notification-hubs-python-backend-how-to/SendWithMultipleTags.png
[5]: ./media/notification-hubs-python-backend-how-to/TemplatedNotification.png
 
