<properties
 pageTitle="Karta Vývojář v příručce - řízení přístupu k centrální IoT | Microsoft Azure"
 description="Karta Vývojář v Azure IoT centrální průvodce – pomocí řízení přístupu k centrální IoT a správě zabezpečení"
 services="iot-hub"
 documentationCenter=".net"
 authors="dominicbetts"
 manager="timlt"
 editor=""/>

<tags
 ms.service="iot-hub"
 ms.devlang="multiple"
 ms.topic="article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="09/30/2016" 
 ms.author="dobett"/>

# <a name="control-access-to-iot-hub"></a>Řízení přístupu k centrální IoT

## <a name="overview"></a>Základní informace

Tento článek popisuje možnosti pro vaše IoT Centrum zabezpečení. IoT centrální používá *oprávnění* udělujete přístup pro koncové body každý IoT centrální. Přístup k rozbočovači IoT podle funkce omezit oprávnění.

Tento článek popisuje:

- Jiná oprávnění, můžete udělit zařízení nebo back-end aplikace pro přístup k centrální IoT.
- Proces ověřování a tokeny používá k ověření oprávnění.
- Jak obor pověření omezit přístup k určitého zdroje.
- Podpora IoT centrální certifikáty X.509.
- Ověřovací mechanismy vlastní zařízení, které používají existující registry identity zařízení nebo režimy ověřování.

### <a name="when-to-use"></a>Kdy použít

Musí mít příslušná oprávnění pro přístup ke všem IoT centrální koncové body. Například zařízení musí obsahovat token obsahující zabezpečovací přihlašovací údaje spolu s každé odeslané rozešle IoT centrální zprávy.

## <a name="access-control-and-permissions"></a>Řízení přístupu a oprávnění

Udělení [oprávnění](#iot-hub-permissions) následujícím způsobem:

* **Centrální úroveň sdílené zásady přístupu**. Zásady sdílené přístupu můžete udělit libovolnou kombinací [oprávnění](#iot-hub-permissions). Definování zásad [Azure portál][lnk-management-portal], nebo programově pomocí [zprostředkovatele prostředků IoT centrální rozhraní REST API][lnk-resource-provider-apis]. Nově vytvořený centrální IoT obsahuje následující výchozí zásady:

    - **iothubowner**: zásad pro všechna oprávnění.
    - **služby**: zásada s ServiceConnect oprávnění.
    - **zařízení**: zásada s DeviceConnect oprávnění.
    - **registryRead**: zásada s RegistryRead oprávnění.
    - **registryReadWrite**: zásada s RegistryRead a RegistryWrite oprávnění.


* **Na zařízení zabezpečovací přihlašovací údaje**. Každý IoT centrální obsahuje [zařízení identity registru][lnk-identity-registry]. U každého zařízení v tomto registru můžete nakonfigurovat zabezpečovací přihlašovací údaje, které udělit oprávnění **DeviceConnect** obor vymezený na odpovídající koncové body zařízení.

Například v typické IoT řešení:

- Součást správy zařízení zásada *registryReadWrite* .
- Procesor pro platformu součást událostí zásada *služby* .
- Součást runtime zařízení obchodní logiky zásada *služby* .
- Jednotlivé zařízení připojit pomocí přihlašovacích údajů uložené v registru centrální IoT identity.

## <a name="authentication"></a>Ověřování

Azure IoT Centrum poskytuje přístup k koncové body ověřením token proti zásady sdílené přístupu a zařízení identity registru zabezpečovací přihlašovací údaje.

Zabezpečovací přihlašovací údaje, například symetrickou kláves, jsou nebyla odeslána prostřednictvím sítě.

> [AZURE.NOTE] Zprostředkovatel zdroje Azure IoT centrální zabezpečený prostřednictvím předplatného Azure jsou všech poskytovatelů ve [Správce prostředků Azure][lnk-azure-resource-manager].

Podrobnosti o tom, jak vytvářet a používat tokenů zabezpečení, najdete v článku [tokenů zabezpečení IoT centrální][lnk-sas-tokens].

### <a name="protocol-specifics"></a>Zvláštnosti Protocol (protokol)

Každý podporované Protocol (protokol), například MQTT, AMQP a HTTP přenosy tokeny různými způsoby.

Pokud chcete použít MQTT, paket připojit má deviceId jako ClientId {iothubhostname} / {deviceId} do pole uživatelské jméno a token přidružení zabezpečení do pole heslo. {iothubhostname} by měl být celé CName centru IoT (například contoso.azure-devices.net).

Při použití [AMQP][lnk-amqp], IoT centrální podporuje [SASL prostý] [ lnk-sasl-plain] a [AMQP nároky na základě – zabezpečení][lnk-cbs].

Pokud používáte AMQP nároky na základě zabezpečením, standardu Určuje, jak přenášet těchto tokenů.

Pro SASL prostý může být **uživatelské jméno** :

* `{policyName}@sas.root.{iothubName}`Pokud používáte rozbočovač úrovni tokeny.
* `{deviceId}@sas.{iothubname}`Pokud používáte zařízení omezené tokeny.

V obou případech obsahuje pole pro heslo tokenu, podle popisu v [tokenů zabezpečení IoT centrální][lnk-sas-tokens].

Nastavit informace HTTP používá ověřování zahrnutím platný token hlavičky **se tak mohli ověřovat** .

#### <a name="example"></a>Příklad

Uživatelské jméno (DeviceId je malá a velká písmena):`iothubname.azure-devices.net/DeviceId`

Heslo (Generovat přidružení s zařízení Explorer):`SharedAccessSignature sr=iothubname.azure-devices.net%2fdevices%2fDeviceId&sig=kPszxZZZZZZZZZZZZZZZZZAhLT%2bV7o%3d&se=1487709501`

> [AZURE.NOTE] [Azure IoT centrální SDK] [ lnk-sdks] automaticky generovat tokeny při připojení ke službám. V některých případech SDK nepodporují všechny protokoly nebo všechny metody ověřování.

### <a name="special-considerations-for-sasl-plain"></a>Důležité informace pro SASL prostý

Při použití SASL prostý s AMQP, klient připojení k rozbočovači IoT použít token jediný pro každé připojení TCP. Když skončí platnost tokenu, připojení pomocí protokolu TCP odpojí od služby a spustí o obnovení. Toto chování při není problematické pro komponentu back-end aplikace je velmi poškození straně zařízení aplikace z těchto důvodů:

*  Bran obvykle připojení jménem mnoha zařízeních. Pokud chcete použít SASL prostý, mají připojení různých TCP pro každé připojení k rozbočovači IoT zařízení. Tento scénář značně zvyšuje spotřebu power a sociální sítě zdrojů a zvyšují latence u každého připojení zařízení.
* Zařízení s omezenými zdroji se týká nepříznivě zvýšení využití prostředků připojit po jednotlivých tokenu vypršení platnosti.

## <a name="scope-hub-level-credentials"></a>Obor centrální úrovni přihlašovací údaje

Zásady zabezpečení na úrovni Centrální můžete omezit vytvořením tokeny s omezenými zdroje URI. Koncový bod k odesílání zpráv zařízení do cloudu z zařízení je například **/devices/ {deviceId} / zprávy nebo události**. Můžete také zásadu sdílený přístup k centrální úroveň oprávnění **DeviceConnect** podepsat token jehož resourceURI je **/devices/ {deviceId}**. Tento přístup vytvoří token, které lze použít k odeslání zprávy za zařízení **deviceId**pouze.

Tento postup je podobný [Zásada publikování události rozbočovače][lnk-event-hubs-publisher-policy]a umožňuje provádět způsobů vlastní ověřování.

## <a name="security-tokens"></a>Tokenů zabezpečení

Centrální IoT používá k ověření zařízení a služeb Vyhněte se odesílání klávesy na lince tokenů zabezpečení. Kromě toho tokenů zabezpečení omezený dobu platnosti a rozsahu. [Azure SDK centrální IoT] [ lnk-sdks] automaticky vytvořit tokeny bez nutnosti žádné zvláštní konfigurace. Některé scénáře však vyžadovat, aby uživatel a generovat tokenů zabezpečení přímo. Jedná se o přímé použití ploch MQTT, AMQP nebo HTTP nebo provádění vzorek Služba tokenů způsobem popsaným v tématu [ověření zařízení vlastní][lnk-custom-auth].

Rozbočovač IoT umožňuje, aby zařízení ověření s IoT centrální pomocí [certifikáty X.509][lnk-x509]. 

### <a name="security-token-structure"></a>Konstrukce tokenu zabezpečení
Pomocí tokenů zabezpečení udělit ohraničených čas přístup k zařízení a služby určitých funkcí v centrální IoT. Abyste měli jistotu, že můžete připojit jenom oprávněnými zařízení a služby, musíte být přihlášení tokenů zabezpečení s sdílených přístupová klávesa zásad nebo symetrickou klávesou uloženo se zařízení identit v registru identity.

Token podepsané sdílené zásad klíčové podpory přístupu ke všem funkcím spojené s sdílených zásady oprávnění. Token podepsané symetrickou klíč identita zařízení na druhé straně pouze udělí oprávnění **DeviceConnect** pro identitu přidružené zařízení.

Tokenu zabezpečení má v tomto formátu:

    SharedAccessSignature sig={signature-string}&se={expiry}&skn={policyName}&sr={URL-encoded-resourceURI}

Toto jsou očekávané hodnoty:

| Hodnota | Popis |
| ----- | ----------- |
| {podpis} | Řetězec HMAC SHA256 podpisu ve formuláři: `{URL-encoded-resourceURI} + "\n" + expiry`. **Důležité**: klávesu se dekódovat pomocí ve formátu Base 64 a slouží jako klíč provést výpočet HMAC SHA256. |
| {resourceURI} | Identifikátor URI předponu (podle segmentu) koncové body, které lze přistupovat pomocí tohoto tokenu počínaje hostname centru IoT (bez protocol). Například`myHub.azure-devices.net/devices/device1` |
| {vypršení platnosti} | UTF8 řetězce počet sekund od UTC 00:00:00 epocha na 1. ledna 1970. |
| {URL-zakódovaný resourceURI} | Nižší případu URL kódování identifikátor URI malými písmeny |
| {název_zásad} | Název zásady přístupu sdílené, na který odkazuje tento token. Chybí v případě tokeny odkazující na zařízení registru pověření. |

**Poznámka týkající se předponu**: URI předponu počítá segmentu a ne po jednotlivých znacích. Příklad `/a/b` je předpony pro `/a/b/c` , ale ne pro `/a/bc`.

Následující úryvek Node.js ukazuje funkci **generateSasToken** , který počítá token z vstupů `resourceUri, signingKey, policyName, expiresInMins`. Následující části obsahují podrobné informace postup inicializace různých vstupů pro případy použití různých tokenu.

    var generateSasToken = function(resourceUri, signingKey, policyName, expiresInMins) {
        resourceUri = encodeURIComponent(resourceUri.toLowerCase()).toLowerCase();

        // Set expiration in seconds
        var expires = (Date.now() / 1000) + expiresInMins * 60;
        expires = Math.ceil(expires);
        var toSign = resourceUri + '\n' + expires;

        // Use crypto
        var hmac = crypto.createHmac('sha256', new Buffer(signingKey, 'base64'));
        hmac.update(toSign);
        var base64UriEncoded = encodeURIComponent(hmac.digest('base64'));

        // Construct autorization string
        var token = "SharedAccessSignature sr=" + resourceUri + "&sig="
        + base64UriEncoded + "&se=" + expires;
        if (policyName) token += "&skn="+policyName;
        return token;
    };

Jako porovnat je ekvivalentní kód Python ke generování tokenu zabezpečení:

    from base64 import b64encode, b64decode
    from hashlib import sha256
    from time import time
    from urllib import quote_plus, urlencode
    from hmac import HMAC

    def generate_sas_token(uri, key, policy_name, expiry=3600):
        ttl = time() + expiry
        sign_key = "%s\n%d" % ((quote_plus(uri)), int(ttl))
        print sign_key
        signature = b64encode(HMAC(b64decode(key), sign_key, sha256).digest())

        rawtoken = {
            'sr' :  uri,
            'sig': signature,
            'se' : str(int(ttl))
        }

        if policy_name is not None:
            rawtoken['skn'] = policy_name

        return 'SharedAccessSignature ' + urlencode(rawtoken)

> [AZURE.NOTE] Protože platnost tokenu proběhne v centrální IoT počítačích, je důležité, aby posun v hodinách počítače generovaný tokenu minimální.

### <a name="use-sas-tokens-in-a-device-client"></a>Použít tokeny přidružení zabezpečení v klientovi zařízení

Existují dva způsoby, jak získat **DeviceConnect** oprávnění s IoT centrální s tokenů zabezpečení: [symetrickou zařízení klíče registru identity zařízení](#use-a-symmetric-key-in-the-identity-registry), nebo použít [klíčem zásad sdílený přístup](#use-a-shared-access-policy).

Mějte na paměti, že všechny funkce přístupné z zařízení zveřejněné příslušným návrhu na koncové body s předponou `/devices/{deviceId}`.

> [AZURE.IMPORTANT] Jediný způsob, jak IoT centrální ověří určitému zařízení používá symetrickou klíč identita zařízení. V případě případě sdílené přístupu přístup k funkcím zařízení řešení třeba vzít v úvahu vystavení tokenu zabezpečení jako důvěryhodný dílčí součásti.

Koncové body přístupným zařízení jsou (bez ohledu na protokol):

| Koncový bod | Funkce |
| ----- | ----------- |
| `{iot hub host name}/devices/{deviceId}/messages/events` | Odeslání zprávy zařízení do cloudu. |
| `{iot hub host name}/devices/{deviceId}/devicebound` | Zprávy cloudu zařízení. |

### <a name="use-a-symmetric-key-in-the-identity-registry"></a>Použití symetrickou klíče registru identity

Při generování token název_zásad pomocí symetrickou klíč identita zařízení (`skn`) prvek tokenu vynechán.

Například token vytvořené pro přístup ke všem funkcím zařízení měli následujících parametrů:

* identifikátor URI: `{IoT hub name}.azure-devices.net/devices/{device id}`,
* podpisový klíč: symetrickou klíč pro `{device id}` identity
* bez zásad jména
* kdykoli vypršení platnosti.

Příklad použití funkce uzel budou:

    var endpoint ="myhub.azure-devices.net/devices/device1";
    var deviceKey ="...";

    var token = generateSasToken(endpoint, deviceKey, null, 60);

Vypadal by výsledek, který zajišťuje přístup ke všem funkcím pro device1:

    SharedAccessSignature sr=myhub.azure-devices.net%2fdevices%2fdevice1&sig=13y8ejUk2z7PLmvtwR5RqlGBOVwiq7rQR3WZ5xZX3N4%3D&se=1456971697

> [AZURE.NOTE] Je možné generovat token zabezpečené nástrojem pro .NET [Zařízení Explorer][lnk-device-explorer].

### <a name="use-a-shared-access-policy"></a>Použití sdíleného přístupu

Při vytváření token ze sdílené přístupu, zásady název pole `skn` musí být nastavena na název zásady použít. Je také povinné zásady udělí oprávnění **DeviceConnect** .

Jsou dva hlavní scénáře použití zásad sdílený přístup pro přístup k funkcím zařízení:

* [cloud protokol bran][lnk-endpoints],
* [token služby] [ lnk-custom-auth] slouží k provádění systémů vlastní ověřování.

Protože sdílené přístupu můžete potenciálně udělit přístup k připojení jako libovolném zařízení, je důležité použít správné identifikátor URI při vytváření tokenů zabezpečení. To je zvlášť důležité tokenu služeb, které se mají rozsah token k určitému zařízení pomocí identifikátor URI. Tato bod platí menší protokol bran podle jejich jsou už jehož prostřednictvím je umožněn provoz na všech zařízeních.

Jako příklad by tokenu služba pomocí předem vytvořené sdílené přístupu **zařízení** s názvem vytvoření token pomocí následujících parametrů:

* identifikátor URI: `{IoT hub name}.azure-devices.net/devices/{device id}`,
* podpisový klíč: jeden klíče `device` zásady,
* Název zásady: `device`,
* kdykoli vypršení platnosti.

Příklad použití funkce uzel budou:

    var endpoint ="myhub.azure-devices.net/devices/device1";
    var policyName = 'device';
    var policyKey = '...';

    var token = generateSasToken(endpoint, policyKey, policyName, 60);

Vypadal by výsledek, který zajišťuje přístup ke všem funkcím pro device1:

    SharedAccessSignature sr=myhub.azure-devices.net%2fdevices%2fdevice1&sig=13y8ejUk2z7PLmvtwR5RqlGBOVwiq7rQR3WZ5xZX3N4%3D&se=1456971697&skn=device

Protokol brány použít stejný token zařízeních jednoduše nastavení identifikátor URI k `myhub.azure-devices.net/devices`.

### <a name="use-security-tokens-from-service-components"></a>Použití tokenů zabezpečení z součásti služby

Součásti služeb, které můžete generovat pouze tokenů zabezpečení pomocí sdílený přístup zásady poskytování potřebná oprávnění, jak je popsáno dříve.

Tyhle funkce služby vystaven na koncové body:

| Koncový bod | Funkce |
| ----- | ----------- |
| `{iot hub host name}/devices` | Vytvoření, aktualizace, načíst a odstranění identity zařízení. |
| `{iot hub host name}/messages/events` | Zprávy zařízení do cloudu. |
| `{iot hub host name}/servicebound/feedback` | Získávat názory ostatních cloudu zařízení zpráv. |
| `{iot hub host name}/devicebound` | Odeslání zprávy cloudu zařízení. |

Jako příklad služby generování pomocí předem vytvořené sdílené přístupu s názvem **registryRead** by vytvořte token pomocí následujících parametrů:

* identifikátor URI: `{IoT hub name}.azure-devices.net/devices`,
* podpisový klíč: jeden klíče `registryRead` zásady,
* Název zásady: `registryRead`,
* kdykoli vypršení platnosti.

    koncový bod var = "myhub.azure-devices.net/devices";   var Název_zásad = "zařízení";   var policyKey =...;

    var token = generateSasToken (koncový bod, policyKey název_zásad 60);

Vypadal by výsledek, který chcete udělit přístup ke čtení všech identit zařízení:

    SharedAccessSignature sr=myhub.azure-devices.net%2fdevices&sig=JdyscqTpXdEJs49elIUCcohw2DlFDR3zfH5KqGJo4r4%3D&se=1456973447&skn=registryRead

## <a name="supported-x509-certificates"></a>Podporované certifikáty X.509

Můžete všechny certifikátu X.509 ověření zařízení s IoT centrální. Tato volba zahrnuje:

-   **Stávající certifikát X.509**. Zařízení může být již certifikátu X.509 s ním spojené. Zařízení můžete použít tento certifikát ověření s IoT centrální.

-   **Certifikát X-509 generovaným a automatickým podpisem**. Výrobce zařízení nebo podnikových nasazovatel můžete generovat tato poukázky a uložit odpovídající soukromý klíč (a certifikát) na zařízení. Pomocí nástrojů, jako jsou [OpenSSL] [ lnk-openssl] a [Windows SelfSignedCertificate] [ lnk-selfsigned] nástroj k tomuto účelu.

-   **Certifikát podepsaný CA X.509**. Můžete také certifikátu X.509 vytvářejí a od certifikační autority (CA) přihlášení k identifikaci zařízení a ověření zařízení s IoT centrální.

Zařízení můžou použít certifikát X.509 nebo tokenu zabezpečení pro ověřování, ale ne obě.

### <a name="register-an-x509-client-certificate-for-a-device"></a>Registrace certifikát X.509 klienta pro zařízení

[Azure IoT služby SDK C#] [ lnk-service-sdk] (verze 1.0.8+) podporuje registrace zařízení, které používá certifikát X.509 klienta pro ověřování. Další rozhraní API například import nebo export zařízení také podporují X.509 certifikátů.

### <a name="c-support"></a>C\# podpory

**RegistryManager** předmětu umožňuje programové zaregistrovat zařízení. Zejména metody **AddDeviceAsync** a **UpdateDeviceAsync** povolení uživatele k registraci a aktualizovat zařízení v registru identity Iot Centrum zařízení. Tyto dva způsoby trvat instanci **zařízení** předávat na vstupu. Třídy **zařízení** obsahuje vlastnosti **ověřování** , což umožňuje uživateli určit primárních a sekundárních thumbprints X.509 certifikát. Miniatura představuje hash SHA-1 certifikátů X.509 (uložené kódováním Binární DER). Uživatelé mají možnost určit primární Miniatura nebo vedlejší Miniatura nebo obojí. Primární a sekundární thumbprints jsou podporovány pro zpracování scénáře při přechodu myší certifikát.

> [AZURE.NOTE] Rozbočovač IoT nevyžaduje ani ukládat celý X.509 certifikát klientského počítače a pouze miniatura.

Tady je ukázka C\# fragment kódu zaregistrovat zařízení pomocí certifikátu X.509 klienta:

```
var device = new Device(deviceId)
{
  Authentication = new AuthenticationMechanism()
  {
    X509Thumbprint = new X509Thumbprint()
    {
      PrimaryThumbprint = "921BC9694ADEB8929D4F7FE4B9A3A6DE58B0790B"
    }
  }
};
RegistryManager registryManager = RegistryManager.CreateFromConnectionString(deviceGatewayConnectionString);
await registryManager.AddDeviceAsync(device);
```

### <a name="use-an-x509-client-certificate-during-runtime-operations"></a>Použijte certifikát klienta X.509 během operace runtime

[Azure IoT zařízení SDK pro .NET] [ lnk-client-sdk] (verze 1.0.11+) podporuje použití X.509 certifikátů.

### <a name="c-support"></a>C\# podpory

Třídy **DeviceAuthenticationWithX509Certificate** podporuje vytvoření instance  **DeviceClient** pomocí certifikátu X.509 klienta.

Tady je ukázka fragment kódu:

```
var authMethod = new DeviceAuthenticationWithX509Certificate("<device id>", x509Certificate);

var deviceClient = DeviceClient.Create("<IotHub DNS HostName>", authMethod);
```

## <a name="custom-device-authentication"></a>Ověření vlastní zařízení

Používáte rozbočovač IoT [zařízení identity registru] [ lnk-identity-registry] ke konfiguraci na zařízení zabezpečovací přihlašovací údaje a přístup k ovládacímu prvku používání [tokenů][lnk-sas-tokens]. Ale pokud IoT řešení ve schématu registru a/nebo ověření identity vlastní zařízení už významné investice, můžete integrovat tento infrastruktury s IoT centrální vytvořením *Služba tokenů*. Tímto způsobem můžete pomocí dalších funkcí IoT ve vašem řešení.

Služba tokenů je vlastní Cloudová služba. Použije IoT centrální *sdílené přístupu* s **DeviceConnect** oprávnění k vytvoření tokeny *omezené zařízení* . Těchto tokenů povolením zařízení pro připojení k centrální IoT.

  ![Postup vzorku Služba tokenů][img-tokenservice]

Toto jsou hlavní kroky vzorku Služba tokenů:

1. Vytvoření zásady přístupu IoT centrální sdílené s oprávněními **DeviceConnect** IoT rozbočovače. Můžete vytvořit tuto zásadu [Azure portál] [ lnk-management-portal] nebo programově. Služba tokenů používá tuto zásadu podepsat tokenů, který předtím vytvořil.
2. Potřebujete-li získat přístup k centrální IoT zařízení, požadavky token podepsané z tokenu služby. Zařízení může ověřovat se schématem registru/ověření identity svůj vlastní zařízení zjistit identitu zařízení, se používá k vytváření tokenu Služba tokenů.
3. Služba tokenů vrátí token. Tokenu vytvořený s využitím `/devices/{deviceId}` jako `resourceURI`, s `deviceId` jako zařízení při ověřování. Služba tokenů použije sdílené přístupu k vytvoření tokenu.
4. Zařízení používá tokenu přímo pomocí centra IoT.

> [AZURE.NOTE] Můžete použít třídu .NET [SharedAccessSignatureBuilder] [ lnk-dotnet-sas] nebo tříd jazyka Java [IotHubServiceSasToken] [ lnk-java-sas] k vytvoření tokenu Služba tokenů.

Služba tokenů můžete nastavit vypršení platnosti tokenu podle potřeby. Když skončí platnost tokenu, centrální IoT přeruší připojení zařízení. Potom zařízení musí požádat o nové token služba tokenů. Pokud používáte čas krátké vypršení, této kombinace kláves rozšíří zatížení zařízení a služba tokenů.

Pro zařízení pro připojení k rozbočovače, je třeba pořád přidat registru IoT Centrum zařízení identity – i v případě, že zařízení používá token a jiný než zařízení klíč připojit se. Proto, můžete dál používat řízení přístupu na zařízení tak, že povolení nebo zakázání zařízení identit v [Centrální IoT identity registru] [ lnk-identity-registry] při ověří zařízení s tokem. To snižuje s riziky spojenými s tokeny pomocí dlouhé vypršení platnosti časy.

### <a name="comparison-with-a-custom-gateway"></a>Porovnání s vlastní brány

Služba tokenů vzorku je doporučený postup implementovat vlastní identitu registru/ověřování schéma s IoT centrální. Je vhodné protože IoT centrální pokračovat ve zpracování většina přenosů řešení. Můžou ale nastat případy, kdy schéma vlastní ověřování je tak vzájemně propojeny s protokolem požaduje služby zpracování všechny přenosy (*vlastní brány*). Příklad je [zabezpečení TLS (Transport Layer) a předem sdílené klíče (PSKs)][lnk-tls-psk]. Další informace najdete v tématu [protokol brány] [ lnk-protocols] tématu.

## <a name="reference-topics"></a>Odkazy na témata:

V těchto tématech poskytují další informace o řízení přístupu k centrální IoT.

## <a name="iot-hub-permissions"></a>Rozbočovač IoT oprávnění

V následující tabulce jsou uvedeny oprávnění, které používáte k řízení přístupu k centrální IoT.

| Oprávnění            | Poznámky |
| --------------------- | ----- |
| **RegistryRead**      | Příspěvky číst přístup k registru identity zařízení. Další informace najdete v tématu [zařízení identity registru][lnk-identity-registry]. |
| **RegistryReadWrite** | Podpory čtení a zápis registru identity zařízení. Další informace najdete v tématu [zařízení identity registru][lnk-identity-registry]. |
| **ServiceConnect**    | Příspěvky přístup ke cloudové služby webových komunikaci a sledování koncové body. Například udělí oprávnění back-end cloudovým službám zprávy zařízení do cloudu, posílat zprávy cloudu zařízení a načíst odpovídající potvrzení doručení. |
| **DeviceConnect**     | Poskytuje přístup k koncové body komunikace webových zařízení. Například udělí oprávnění k posílání zpráv zařízení do cloudu a přijímání zpráv cloudu zařízení. Toto oprávnění používanou zařízení. |

## <a name="additional-reference-material"></a>Další materiály

Další témata odkaz v příručce pro vývojáře patří:

- [Koncové body IoT centrální] [ lnk-endpoints] popisuje různé koncové body, které každého IoT rozbočovače zpřístupňuje pro operace runtime a správy.
- [Omezení a kvót] [ lnk-quotas] popisuje kvóty, které platí pro službu IoT rozbočovači a omezení chování má čekat při používání služby.
- [Rozbočovač IoT zařízení a přihlašovacích údajů SDK] [ lnk-sdks] uvádí různé jazykové sady SDK můžete použít při vytváření zařízení a služby aplikace, které spolupracují s IoT centrální.
- [Dotazy jazyka pro twins, metody a úlohy] [ lnk-query] popisuje dotazovací jazyk slouží k načtení informací z centrální IoT o zařízení twins, metody a úlohy.
- [Podpora IoT centrální MQTT] [ lnk-devguide-mqtt] poskytuje další informace o podpoře IoT centrální protokolu MQTT.

## <a name="next-steps"></a>Další kroky

Teď jste se naučili řízení přístupu IoT centrální, vás mohla zajímat v následujících tématech vývojář příručka:

- [Pomocí zařízení twins synchronizovat stav a konfigurace][lnk-devguide-device-twins]
- [Přímé metodu na zařízení][lnk-devguide-directmethods]
- [Plánování úlohy na několika zařízeních][lnk-devguide-jobs]

Pokud chcete vyzkoušet některé pojmy popisované v tomto článku, může být zajímavé v těchto kurzech IoT centrální:

- [Začínáme s centrální IoT Azure][lnk-getstarted-tutorial]
- [Odeslání zprávy cloudu zařízení s IoT centrální][lnk-c2d-tutorial]
- [Způsob zpracování zprávy IoT Centrum zařízení do cloudu][lnk-d2c-tutorial]

<!-- links and images -->

[img-tokenservice]: ./media/iot-hub-devguide-security/tokenservice.png
[lnk-endpoints]: iot-hub-devguide-endpoints.md
[lnk-quotas]: iot-hub-devguide-quotas-throttling.md
[lnk-sdks]: iot-hub-devguide-sdks.md
[lnk-query]: iot-hub-devguide-query-language.md
[lnk-devguide-mqtt]: iot-hub-mqtt-support.md
[lnk-openssl]: https://www.openssl.org/
[lnk-selfsigned]: https://technet.microsoft.com/library/hh848633

[lnk-resource-provider-apis]: https://msdn.microsoft.com/library/mt548492.aspx
[lnk-sas-tokens]: iot-hub-devguide-security.md#security-tokens
[lnk-amqp]: https://www.amqp.org/
[lnk-azure-resource-manager]: ../azure-resource-manager/resource-group-overview.md
[lnk-cbs]: https://www.oasis-open.org/committees/download.php/50506/amqp-cbs-v1%200-wd02%202013-08-12.doc
[lnk-event-hubs-publisher-policy]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-99ce67ab
[lnk-management-portal]: https://portal.azure.com
[lnk-sasl-plain]: http://tools.ietf.org/html/rfc4616
[lnk-identity-registry]: iot-hub-devguide-identity-registry.md
[lnk-dotnet-sas]: https://msdn.microsoft.com/library/microsoft.azure.devices.common.security.sharedaccesssignaturebuilder.aspx
[lnk-java-sas]: http://azure.github.io/azure-iot-sdks/java/service/api_reference/com/microsoft/azure/iot/service/auth/IotHubServiceSasToken.html
[lnk-tls-psk]: https://tools.ietf.org/html/rfc4279
[lnk-protocols]: iot-hub-protocol-gateway.md
[lnk-custom-auth]: iot-hub-devguide-security.md#custom-device-authentication
[lnk-x509]: iot-hub-devguide-security.md#supported-x509-certificates
[lnk-devguide-device-twins]: iot-hub-devguide-device-twins.md
[lnk-devguide-directmethods]: iot-hub-devguide-direct-methods.md
[lnk-devguide-jobs]: iot-hub-devguide-jobs.md
[lnk-service-sdk]: https://github.com/Azure/azure-iot-sdks/tree/master/csharp/service
[lnk-client-sdk]: https://github.com/Azure/azure-iot-sdks/tree/master/csharp/device
[lnk-device-explorer]: https://github.com/Azure/azure-iot-sdks/blob/master/tools/DeviceExplorer/doc/how_to_use_device_explorer.md

[lnk-getstarted-tutorial]: iot-hub-csharp-csharp-getstarted.md
[lnk-c2d-tutorial]: iot-hub-csharp-csharp-c2d.md
[lnk-d2c-tutorial]: iot-hub-csharp-csharp-process-d2c.md
