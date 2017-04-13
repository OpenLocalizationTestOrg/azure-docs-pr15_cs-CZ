<properties
    pageTitle="Sdílené přístup podpisy přehled | Microsoft Azure"
    description="Co jsou sdílené podpisy přístup, jak pracují a jak je lze používat z uzel PHP a C#."
    services="service-bus"
    documentationCenter="na"
    authors="djrosanova"
    manager="timlt"
    editor=""/>

<tags
    ms.service="service-bus"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="10/02/2016"
    ms.author="darosa;sethm"/>

# <a name="shared-access-signatures"></a>Sdílený přístup podpisy

*Sdílené přístup podpisy* (Přidružení zabezpečení) jsou mechanismus primární zabezpečení pro Bus služby, včetně události rozbočovače brokered zasílání zpráv (fronty a témata) a přenos zasílání zpráv. Tento článek popisuje sdílené podpisy přístup, jak fungují a jak je používat způsobem, bez ohledu na platformu.

## <a name="overview-of-sas"></a>Přehled přidružení

Metoda ověřování na základě zabezpečené hash SHA 256 nebo identifikátory URI jsou sdílené podpisy přístup. Přidružení je velmi výkonné mechanismus, který používá všechny služby Bus služby. V praxi přidružení zabezpečení má dvě součásti: *Sdílené zásady přístupu* a *Sdílené podpis aplikace Access* (někdy označovány jako *tokenu*).

Podrobnější informace o sdílené přístup podpisy, služby Bus můžete najít v [ověřování sdílené podpis aplikace Access s Bus služby](service-bus-shared-access-signature-authentication.md).

## <a name="shared-access-policy"></a>Sdílené přístupu

Důležité pochopit o přidružení zabezpečení je, že všechny začíná zásadu. Pro každého zásadu se rozhodnete na tři části informací: **název**, **rozsah**a **oprávnění**. **Název** je právě; Jedinečný název v daném oboru. Obor je natolik snadná: je URI dotyčného zdroje. Pro službu Bus názvů, je oborem plně kvalifikovaný název domény (FQDN), jako například `https://<yournamespace>.servicebus.windows.net/`.

K dispozici oprávnění pro zásady jsou převážně zřejmé:

  + Odeslání
  + Poslech
  + Správa

Po vytvoření zásad přidělený *Primární klíč* a *Sekundárního klíče*. Toto jsou neposkytuje silných klíče. Není ztratit nebo proniknout je – vždy budete mít k dispozici v [Azure portálu][]. Můžete použít buď generovaného klíče a můžete je obnovit kdykoli. Ale pokud opětovnému vytvoření nebo změna primárního klíče do zásad, budou neplatné Access všechny sdílené podpisy, vytvořené z něho.

Při vytváření názvů služby Bus zásady se automaticky vytvoří celý obor názvů s názvem **RootManageSharedAccessKey**a tuto zásadu obsahuje všechna oprávnění. Není přihlásíte jako **kořenovou**tak nepoužívejte tuto zásadu, pokud je ale skutečně pádný důvod. Další zásady můžete vytvořit na kartě **Konfigurovat** pro názvů na portálu. Je důležité poznámky typu single strom úroveň v Bus služby (názvů, fronty, událostí rozbočovači atd.) může obsahovat pouze až 12 zásady připojené k němu.

## <a name="shared-access-signature-token"></a>Sdílené přístup podpisu (token)

Zásady samotné není přístupový token Bus služby. Je objekt, ze kterého je generováno přístupový token – pomocí klávesy primární a sekundární. Token je generováno aplikací pečlivě věnujte řetězec v tomto formátu:

```
SharedAccessSignature sig=<signature-string>&se=<expiry>&skn=<keyName>&sr=<URL-encoded-resourceURI>
```

Kde `signature-string` je hash SHA 256 rozsahu token (podle popisu v předchozí části**obor** ) s Line FEED přidaným a vypršení platnosti čas (v sekundách od epocha: `00:00:00 UTC` na 1. ledna 1970).

Algoritmus hash vypadá podobně jako následující pseudokód a vrátí 32 bajtů.

```
SHA-256('https://<yournamespace>.servicebus.windows.net/'+'\n'+ 1438205742)
```

Hodnoty zatříděna jsou řetězce **SharedAccessSignature** tak, aby příjemce můžete vypočítat hash se stejnými parametry, abyste měli jistotu, že vrátí stejný výsledek. Identifikátor URI určuje rozsah a název klíče identifikuje zásadu použít pro výpočet algoritmus hash. Co je důležité z hlediska zabezpečení. Pokud podpis neodpovídá, které vypočítá příjemce (služby Bus), přístup odepřen. V tomto okamžiku chcete mít jistotu, že odesílatel měli přístup ke klíči a vyhoví práva podle zásadu.

## <a name="generating-a-signature-from-a-policy"></a>Generování podpis ze zásady

Jak ve skutečnosti to uděláte v kódu? Podívejme se na některé z těchto.

### <a name="nodejs"></a>NodeJS

```
function createSharedAccessToken(uri, saName, saKey) { 
    if (!uri || !saName || !saKey) { 
            throw "Missing required parameter"; 
        } 
    var encoded = encodeURIComponent(uri); 
    var now = new Date(); 
    var week = 60*60*24*7;
    var ttl = Math.round(now.getTime() / 1000) + week;
    var signature = encoded + '\n' + ttl; 
    var signatureUTF8 = utf8.encode(signature); 
    var hash = crypto.createHmac('sha256', saKey).update(signatureUTF8).digest('base64'); 
    return 'SharedAccessSignature sr=' + encoded + '&sig=' +  
        encodeURIComponent(hash) + '&se=' + ttl + '&skn=' + saName; 
}
``` 

### <a name="java"></a>Java

```
private static String GetSASToken(String resourceUri, String keyName, String key)
  {
      long epoch = System.currentTimeMillis()/1000L;
      int week = 60*60*24*7;
      String expiry = Long.toString(epoch + week);

      String sasToken = null;
      try {
          String stringToSign = URLEncoder.encode(resourceUri, "UTF-8") + "\n" + expiry;
          String signature = getHMAC256(key, stringToSign);
          sasToken = "SharedAccessSignature sr=" + URLEncoder.encode(resourceUri, "UTF-8") +"&sig=" +
                  URLEncoder.encode(signature, "UTF-8") + "&se=" + expiry + "&skn=" + keyName;
      } catch (UnsupportedEncodingException e) {

          e.printStackTrace();
      }

      return sasToken;
  }


public static String getHMAC256(String key, String input) {
    Mac sha256_HMAC = null;
    String hash = null;
    try {
        sha256_HMAC = Mac.getInstance("HmacSHA256");
        SecretKeySpec secret_key = new SecretKeySpec(key.getBytes(), "HmacSHA256");
        sha256_HMAC.init(secret_key);
        Encoder encoder = Base64.getEncoder();

        hash = new String(encoder.encode(sha256_HMAC.doFinal(input.getBytes("UTF-8"))));

    } catch (InvalidKeyException e) {
        e.printStackTrace();
    } catch (NoSuchAlgorithmException e) {
        e.printStackTrace();
   } catch (IllegalStateException e) {
        e.printStackTrace();
    } catch (UnsupportedEncodingException e) {
        e.printStackTrace();
    }

    return hash;
}
```

### <a name="php"></a>PHP

```
function generateSasToken($uri, $sasKeyName, $sasKeyValue) 
{ 
$targetUri = strtolower(rawurlencode(strtolower($uri))); 
$expires = time();  
$expiresInMins = 60; 
$week = 60*60*24*7;
$expires = $expires + $week; 
$toSign = $targetUri . "\n" . $expires; 
$signature = rawurlencode(base64_encode(hash_hmac('sha256',             
 $toSign, $sasKeyValue, TRUE))); 

$token = "SharedAccessSignature sr=" . $targetUri . "&sig=" . $signature . "&se=" . $expires .      "&skn=" . $sasKeyName; 
return $token; 
}
```
 
### <a name="c35"></a>C a #35;

```
private static string createToken(string resourceUri, string keyName, string key)
{
    TimeSpan sinceEpoch = DateTime.UtcNow - new DateTime(1970, 1, 1);
    var week = 60 * 60 * 24 * 7;
    var expiry = Convert.ToString((int)sinceEpoch.TotalSeconds + week);
    string stringToSign = HttpUtility.UrlEncode(resourceUri) + "\n" + expiry;
    HMACSHA256 hmac = new HMACSHA256(Encoding.UTF8.GetBytes(key));
    var signature = Convert.ToBase64String(hmac.ComputeHash(Encoding.UTF8.GetBytes(stringToSign)));
    var sasToken = String.Format(CultureInfo.InvariantCulture, "SharedAccessSignature sr={0}&sig={1}&se={2}&skn={3}", HttpUtility.UrlEncode(resourceUri), HttpUtility.UrlEncode(signature), expiry, keyName);
    return sasToken;
}
```

## <a name="using-the-shared-access-signature-at-http-level"></a>Používání podpisu sdílené aplikace Access (na úrovni HTTP)
 
Teď když víte, jak vytvořit sdílené podpisy přístup pro libovolné entity Bus služby, jste připraveni k provedení HTTP příspěvku:

```
POST https://<yournamespace>.servicebus.windows.net/<yourentity>/messages
Content-Type: application/json
Authorization: SharedAccessSignature sr=https%3A%2F%2F<yournamespace>.servicebus.windows.net%2F<yourentity>&sig=<yoursignature from code above>&se=1438205742&skn=KeyName
ContentType: application/atom+xml;type=entry;charset=utf-8
``` 
    
Nezapomeňte, že to funguje pro všechny položky. Přidružení zabezpečení můžete vytvořit pro fronty, téma, předplatné, centrální událost nebo předávání. Pokud používáte za Publisheru identity pro událost rozbočovače, jednoduše přidat `/publishers/< publisherid>`.

Pokud zadáte název odesílatele nebo klienta token přidružení zabezpečení, tito uživatelé nemají klávesu přímo a nejde vrátit hash ji získat. Jako takové budete mít kontrolu nad co mají přístup a jak dlouho. Důležité si zapamatovat je-li změnit primární klíč v zásadu, budou neplatné Access všechny sdílené podpisy, vytvořené z něho.

## <a name="using-the-shared-access-signature-at-amqp-level"></a>Používání podpisu sdílené aplikace Access (na úrovni AMQP)

V předchozí části viděli, jak pomocí přidružení zabezpečení token žádost HTTP POST k odesílání dat Bus služby. Jak víte, můžete využít služby Bus pomocí rozšířený zpráva řízení fronty Protocol (AMQP), která je preferovaný protokol používat důvodů výkonu v mnoha případech. Použití tokenu přidružení zabezpečení s AMQP je popsaná v dokumentu [AMQP Claim-Based zabezpečení verze 1.0](https://www.oasis-open.org/committees/download.php/50506/amqp-cbs-v1%200-wd02%202013-08-12.doc) , který je v pracovní koncept od 2013 ale dobře podporovaný Azure dnes.

Před spuštěním odeslání dat do služby Bus vydavatele musí poslat token přidružení zabezpečení uvnitř zprávu AMQP dobře definovaný AMQP uzel s názvem **$cbs** (uvidíte ho jako "speciální" fronty služba používá k získání a ověření všech tokenů přidružení zabezpečení). Vydavatel musíte zadat pole **ReplyTo** uvnitř AMQP zprávy. Toto je uzel, ve kterém službu odpovědi vydavateli s výsledkem tokenu ověření (jednoduchý žádost nebo odpovědi vzorek mezi Publisheru a přihlašovacích údajů). Vytvořit tato odpověď uzel "průběžně," mluvíte o "dynamické vytvoření Vzdálená uzlů" popsané ve specifikaci AMQP 1.0. Po ověření, zda token přidružení zabezpečení je platný, vydavatele Přechod vpřed a začněte odeslání dat do služby.

Podle těchto kroků ukazují, jak poslat token přidružení zabezpečení s protokolem AMQP [AMQP.Net Lite](https://github.com/Azure/amqpnetlite) knihovnu. Toto je užitečné, pokud nelze použít v oficiální Bus SDK služby (třeba na WinRT, .net Compact Framework a .net Micro Framework a Mono) vývoj C\#. Samozřejmě, je vhodné porozumět tomu, jak na základě deklarací identit zabezpečení tuto knihovnu označené jako pracuje na úrovni AMQP, jak jste viděli, jak to funguje na úrovni protokolu HTTP (s žádost HTTP příspěvku a přidružení zabezpečení token odeslaný do záhlaví "Se tak mohli ověřovat"). Pokud nepotřebujete hloubkové poznatky o AMQP, můžete v oficiální Bus SDK služby s .net Framework aplikace, které se to udělá za vás.

### <a name="c35"></a>C a #35;

```
/// <summary>
/// Send claim-based security (CBS) token
/// </summary>
/// <param name="shareAccessSignature">Shared access signature (token) to send</param>
private bool PutCbsToken(Connection connection, string sasToken)
{
    bool result = true;
    Session session = new Session(connection);

    string cbsClientAddress = "cbs-client-reply-to";
    var cbsSender = new SenderLink(session, "cbs-sender", "$cbs");
    var cbsReceiver = new ReceiverLink(session, cbsClientAddress, "$cbs");

    // construct the put-token message
    var request = new Message(sasToken);
    request.Properties = new Properties();
    request.Properties.MessageId = Guid.NewGuid().ToString();
    request.Properties.ReplyTo = cbsClientAddress;
    request.ApplicationProperties = new ApplicationProperties();
    request.ApplicationProperties["operation"] = "put-token";
    request.ApplicationProperties["type"] = "servicebus.windows.net:sastoken";
    request.ApplicationProperties["name"] = Fx.Format("amqp://{0}/{1}", sbNamespace, entity);
    cbsSender.Send(request);

    // receive the response
    var response = cbsReceiver.Receive();
    if (response == null || response.Properties == null || response.ApplicationProperties == null)
    {
        result = false;
    }
    else
    {
        int statusCode = (int)response.ApplicationProperties["status-code"];
        if (statusCode != (int)HttpStatusCode.Accepted && statusCode != (int)HttpStatusCode.OK)
        {
            result = false;
        }
    }

    // the sender/receiver may be kept open for refreshing tokens
    cbsSender.Close();
    cbsReceiver.Close();
    session.Close();

    return result;
}
```

`PutCbsToken()` Metoda přijímat *připojení* (AMQP připojení instance třídy podle [AMQP .NET Lite knihovny](https://github.com/Azure/amqpnetlite)), které představuje připojení pomocí protokolu TCP služby a *sasToken* parametr, který je token přidružení zabezpečení odeslat. 

> [AZURE.NOTE] Je důležité, připojení se vytvoří pomocí **mechanismus ověřování SASL nastavena na externí** (a nikoli prostý výchozí pomocí jména a hesla vyvolají nemusíte odeslat token přidružení zabezpečení).

Pak vydavatele vytvoří dva odkazy AMQP token přidružení zabezpečení odesílání a přijímání odpovědět (výsledek tokenu ověření) ze služby.

Zpráva AMQP obsahuje sadu vlastností a dalších informací než jednoduchý zprávy. Přidružení zabezpečení token je textu zprávy (použití jeho konstruktoru). Vlastnost **"ReplyTo"** je nastavena na uzel název pro příjem výsledku ověření na odkaz příjemci (můžete změnit jeho název Pokud chcete, a bude vytvořena dynamicky službou). Posledních třech aplikace nebo vlastní vlastnosti používají službu označíte, jaký druh operace má provést. Jak je popsáno ve specifikaci CBS koncept, musí být **název operace** ("umístění token"), **Zadejte tokenu** (v tomto případě "servicebus.windows.net:sastoken") a **"název" cílové skupiny** , do kterého tokenu platí (celé entity).

Po odeslání token přidružení zabezpečení na odkaz odesílatele, musí vydavatele přečtěte si odpovědi na odkaz příjemci. Odpověď je jednoduchý AMQP zpráva s aplikací vlastností s názvem **"stavový kód"** , která může obsahovat stejné hodnoty jako stavový kód HTTP. 

## <a name="next-steps"></a>Další kroky

V části [Služba Bus REST API odkaz](https://msdn.microsoft.com/library/azure/hh780717.aspx) Další informace o jaké máte možnosti s těchto tokenů přidružení zabezpečení.

Další informace o ověřování služby Bus najdete v článku [Bus služby a tak mohli ověřovat](service-bus-authentication-and-authorization.md). 

Další příklady přidružení v jazyce C# a skriptu Java jsou v [tomto příspěvku blogu](http://developers.de/blogs/damir_dobric/archive/2013/10/17/how-to-create-shared-access-signature-for-service-bus.aspx).

[Azure portálu]: https://portal.azure.com