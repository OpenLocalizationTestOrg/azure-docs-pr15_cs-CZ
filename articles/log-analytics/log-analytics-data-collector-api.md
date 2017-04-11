<properties
    pageTitle="Protokolování analýzy HTTP kolekcí rozhraní API | Microsoft Azure"
    description="Rozhraní API dat protokolu analýzy HTTP kolekcí slouží k přidání příspěvku JSON dat do úložiště protokolu analýzy z klienta, který můžete volat rozhraní REST API. Tento článek popisuje, jak používat rozhraní API a obsahuje příklady, jak publikovat dat pomocí programového jazyky."
    services="log-analytics"
    documentationCenter=""
    authors="bwren"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="log-analytics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/26/2016"
    ms.author="bwren"/>


# <a name="log-analytics-http-data-collector-api"></a>Protokol analýzy HTTP kolekcí rozhraní API

Při použití kolekcí API Azure protokolu HTTP technologie pro analýzu dat, můžete přidat příspěvek JavaScript Object Notation (JSON) data do úložiště protokolu analýzy z klienta, který můžete volat rozhraní REST API. Tímto způsobem můžete odeslat data z aplikace jiných výrobců nebo skripty, třeba z postupu runbook v Azure automatizaci.  

## <a name="create-a-request"></a>Vytvoření žádosti o

V dalších dvou tabulkách jsou uvedeny atributy, které jsou potřeba pro každý požadavek kolekcí API protokolu analýzy HTTP Data. Popis jednotlivých atributů podrobněji dále v tomto článku.

### <a name="request-uri"></a>Žádost o URI

| Atribut | Vlastnost |
|:--|:--|
| Metoda | PŘÍSPĚVEK |
| IDENTIFIKÁTOR URI | https://\<CustomerId\>.ods.opinsights.azure.com/api/logs?api-version=2016-04-01 |
| Typ obsahu | aplikace/json |

### <a name="request-uri-parameters"></a>Identifikátor URI parametry žádosti
| Parametr | Popis |
|:--|:--|
| CustomerID  | Jedinečný identifikátor Microsoft operace správy Suite pracovního prostoru. |
| Zdroje    | Název zdroje rozhraní API: / rozhraní api/protokoly. |
| Rozhraní API verze | Verze rozhraní API pro práci s tohoto požadavku. V současnosti je 2016 04 01. |

### <a name="request-headers"></a>Žádost o záhlaví
| Záhlaví | Popis |
|:--|:--|
| Povolení | Povolení podpis. Dále v tomto článku si můžete přečíst o postupu při vytváření HMAC SHA256 záhlaví. |
| Typ protokolu | Zadejte data, která je při odesílání typ záznamu. Typ protokolu v současné době podporuje pouze alfa znaky. Nepodporuje se číslovky ani speciální znaky. |
| x-ms-date | Datum, kdy byla zpracovány žádosti ve formátu RFC 1123. |
| čas generované pole | Název pole data, která obsahuje časové razítko položka data. Pokud zadáte pole obsah se používají pro **TimeGenerated**. Pokud toto pole není zadáno, výchozí **TimeGenerated** je údaj o čase, požití zprávu. Obsah pole zprávy postupujte podle formátu formátu ISO 8601 rrrr-MM-DDThh:mm:ssZ. |


## <a name="authorization"></a>Povolení

Jakékoliv žádosti o protokolu analýzy HTTP dat kolekcí API musí obsahovat záhlaví se tak mohli ověřovat. Ověření žádost, musíte se přihlásit na požadavek na primární nebo vedlejší klíč pro pracovní prostor, který posílá požadavek. Potom předejte podpis jako součást žádost.   

Tady je formát záhlaví se tak mohli ověřovat:

```
Authorization: SharedKey <WorkspaceID>:<Signature>
```

*WorkspaceID* je jedinečný identifikátor operace správy sadu pracovního prostoru. *Podpis* je [Na základě Hash zprávy ověřovací kód (HMAC)](https://msdn.microsoft.com/library/system.security.cryptography.hmacsha256.aspx) , který je vytvořen ze žádosti o a potom spočítat pomocí [SHA256 algoritmus](https://msdn.microsoft.com/library/system.security.cryptography.sha256.aspx). Pak můžete kódovat pomocí kódování ve formátu Base 64.

Použijte tento formát kódování řetězec **SharedKey** podpis:

```
StringToSign = VERB + "\n" +
               Content-Length + "\n" +
               Content-Type + "\n" +
               x-ms-date + "\n" +
               "/api/logs";
```

Tady je příklad řetězce podpis:

```
POST\n1024\napplication/json\nx-ms-date:Mon, 04 Apr 2016 08:00:00 GMT\n/api/logs
```

Pokud máte podpisu řetězec, kódování pomocí algoritmu HMAC SHA256 řetězec kódování UTF-8 a kódovat výsledek jako ve formátu Base 64. Použijte tento formát:

```
Signature=Base64(HMAC-SHA256(UTF8(StringToSign)))
```

Příklady v dalších částech mít ukázkový kód vám pomůže vytvořit se tak mohli ověřovat záhlaví.

## <a name="request-body"></a>Žádost o textu

Text zprávy musí být ve formátu JSON. Jeden nebo více záznamů pomocí vlastnosti název-hodnota dvojice musí obsahovat v tomto formátu:

```
{
"property1": "value1",
" property 2": "value2"
" property 3": "value3",
" property 4": "value4"
}
```

Dávkové více záznamů společně v jednom požadavku pomocí v tomto formátu. Všechny záznamy musí být stejného typu záznamu.

```
{
"property1": "value1",
" property 2": "value2"
" property 3": "value3",
" property 4": "value4"
},
{
"property1": "value1",
" property 2": "value2"
" property 3": "value3",
" property 4": "value4"
}
```

## <a name="record-type-and-properties"></a>Typ záznamu a vlastností

Při odešlete data prostřednictvím protokolu analýzy HTTP dat kolekcí API definovat vlastní typ záznamu. V současné době nejde zápis dat do existující typy záznamů, které byly vytvořené v jiných datových typů a řešení. Technologie pro analýzu protokolu čte příchozí data a potom vytvoří vlastnosti, které odpovídají typu dat zadaných hodnot.

Každý požadavek k rozhraní API technologie pro analýzu protokolu musí obsahovat záhlaví **Typ protokolu** s názvem pro typ záznamu. Přípona **_CL** automaticky přidána na název zadáte odlišil od jiných typů protokolu jako vlastní protokol. Pokud zadáte název **MyNewRecordType**, analýzy protokolu vytvoří záznam s typem **MyNewRecordType_CL**. Díky tomu, že nejsou žádné konflikty mezi uživatelským zadejte jména a můžou být součástí aktuální nebo budoucí řešení společnosti Microsoft.

Jak identifikovat datový typ vlastnosti, analýzy protokolu přidá příponu název vlastnosti. Pokud vlastnost obsahuje hodnotu null, vlastnost není součástí tohoto záznamu. Tato tabulka uvádí datový typ vlastnosti a odpovídající přípony:

| Datový typ vlastnosti | Přípona |
|:--|:--|
| Řetězec    | _M |
| Logická hodnota   | _B |
| Dvojité    | _d |
| Datum a čas | _T |
| IDENTIFIKÁTOR GUID      | _g |


Datový typ, který používá protokol Analytics pro každou vlastnost závisí na tom, jestli už existuje typ záznamu pro nový záznam.

- Pokud typ záznamu neexistuje, analýzy protokolu vytvoří novou. Protokolu analýzy používá při JSON typ odvozování k určení datového typu pro každou vlastnost pro nový záznam.
- Pokud existují typ záznamu, pokusí se protokolu analýzy vytvořte nový záznam na základě existující vlastností. Pokud datový typ pro vlastnost v novém záznamu neodpovídají a nelze převést na existující typ nebo pokud záznam obsahuje vlastnost, která neexistuje, analýzy protokolu vytvoří novou vlastnost, která obsahuje příslušné přípony.

Tento údaj odeslání by například vytvořit záznam s tři vlastnosti **number_d**, **boolean_b**a **string_s**:

![Ukázka záznam 1](media/log-analytics-data-collector-api/record-01.png)

Pokud pak odesláno této další položky, se všechny hodnoty řetězce ve formátu, nebude změnit vlastnosti. Tyto hodnoty lze převést na existující datové typy:

![Ukázka záznam 2](media/log-analytics-data-collector-api/record-02.png)

Ale pokud pak udělali tohoto další odeslání protokolu analýzy by vytvářet nové vlastnosti **boolean_d** a **string_d**. Nelze převést tyto hodnoty:

![Ukázka záznam 3](media/log-analytics-data-collector-api/record-03.png)

Pokud pak následující položku odesláno před vytvořením typ záznamu, by protokolu analýzy vytvořte záznam s tři vlastnosti **počet_úspěchů**, **boolean_s**a **string_s**. V této položce všech počáteční hodnoty se automaticky naformátuje jako řetězec:

![Ukázka záznam 4](media/log-analytics-data-collector-api/record-04.png)


## <a name="data-limits"></a>Omezení dat
Existují některá omezení kolem data zveřejněná ke kolekci technologie pro analýzu dat protokolu rozhraní API.

- Maximální 30 MB na příspěvek do protokolu technologie pro analýzu dat kolekcí API. Toto je omezení velikosti platí pro jeden příspěvek. Pokud data z jednoho publikovat, větší než 30 MB, měli byste rozdělení data až menší velikosti bloky a pošlete jim současně. 
- Maximální limit 32 KB hodnot polí. Pokud v poli hodnota je větší než 32 KB, bude zkrácen data. 
- Doporučené polí pro daný typ maximálně 50. Toto je praktické omezení z použitelnosti a hledání prostředí perspektivu.  


## <a name="return-codes"></a>Vrátí kódy

Stavový kód HTTP 202 znamená, že žádost přijme pro zpracování, ale zatím nebylo dokončeno zpracování. Tento údaj označuje, že operace byla úspěšně dokončena.

V této tabulce jsou uvedeny úplná sada stavů, které může vrátit služby:

| Kód | Stav | Kód chyby | Popis |
|:--|:--|:--|:--|
| 202 | Přijaté |  | Pokud chcete žádost bylo úspěšně přijato. |
| 400 | Chybný požadavek | InactiveCustomer | Pracovní prostor byl zavřen. |
| 400 | Chybný požadavek | InvalidApiVersion | Rozhraní API verze, kterou jste zadali nebyl rozpoznán službou. |
| 400 | Chybný požadavek | InvalidCustomerId | Zadané ID pracovního prostoru je neplatný. |
| 400 | Chybný požadavek | InvalidDataFormat | Neplatné JSON byla odeslána. Obsah odpovědí může obsahovat další informace o tom, jak vyřešit. |
| 400 | Chybný požadavek | InvalidLogType | Typ protokolu určen obsažené speciální znaky nebo číslovky. |
| 400 | Chybný požadavek | MissingApiVersion | Verze rozhraní API nebyla zadána. |
| 400 | Chybný požadavek | MissingContentType | Nebyla zadána typ obsahu. |
| 400 | Chybný požadavek | MissingLogType | Nebyla zadána protokolu zadejte požadovanou hodnotu. |
| 400 | Chybný požadavek | UnsupportedContentType | Typ obsahu nebyla nastavena na **aplikace a json**. |
| 403 | Zakázáno | InvalidAuthorization | Ověření žádost o službu se nepovedlo. Ověřte platnost klávesu ID a připojení pracovního prostoru. |
| 500 | Vnitřní chyba serveru | UnspecifiedError | Ve službě došlo k interní chybě. Opakujte žádost. |
| 503 | Služba není k dispozici | ServiceUnavailable | Služba momentálně není dostupná pro příjem žádosti o. Opakujte vaši žádost. |

## <a name="query-data"></a>Dotaz na data

Získat data odeslaná rozhraním API protokolu analýzy HTTP dat kolekcí vyhledávat záznamy pomocí **Typ** , který se rovná hodnotě **LogType** zadaných zadaný, připojené **_CL**. Například pokud jste použili **MyCustomLog**, pak můžete vrátí všechny záznamy obsahující **Typ = MyCustomLog_CL**.


## <a name="sample-requests"></a>Požadavky na ukázky

V dalších částech najdete příklady způsob odeslání dat do protokolu analýzy HTTP dat kolekcí API pomocí programového jazyky.

Pro každý vzorek, udělejte tyto kroky nastavení proměnné záhlaví se tak mohli ověřovat:

1. Na portálu operace správy sadu vyberte dlaždici **Nastavení** a pak klikněte na kartu **Připojení zdroje** .
2. Napravo od **ID pracovního prostoru**klikněte na ikonu kopírovat a vložit ID jako hodnotu proměnné **ID zákazníka** .
3. Napravo od **Primární klíč**vyberte ikonu kopírovat a vložte ID jako hodnotu proměnné **Sdílené klíče** .

Případně můžete změnit proměnné typu protokolu a JSON data.

### <a name="powershell-sample"></a>Ukázka prostředí PowerShell

```
# Replace with your Workspace ID
$CustomerId = "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"  

# Replace with your Primary Key
$SharedKey = "xxxxxxxxxxxxxxxxxxxxxxxxxxxxxx"

# Specify the name of the record type that you'll be creating
$LogType = "MyRecordType"

# Specify a field with the created time for the records
$TimeStampField = "DateValue"


# Create two records with the same set of properties to create
$json = @"
[{  "StringValue": "MyString1",
    "NumberValue": 42,
    "BooleanValue": true,
    "DateValue": "2016-05-12T20:00:00.625Z",
    "GUIDValue": "9909ED01-A74C-4874-8ABF-D2678E3AE23D"
},
{   "StringValue": "MyString2",
    "NumberValue": 43,
    "BooleanValue": false,
    "DateValue": "2016-05-12T20:00:00.625Z",
    "GUIDValue": "8809ED01-A74C-4874-8ABF-D2678E3AE23D"
}]
"@

# Create the function to create the authorization signature
Function Build-Signature ($customerId, $sharedKey, $date, $contentLength, $method, $contentType, $resource)
{
    $xHeaders = "x-ms-date:" + $date
    $stringToHash = $method + "`n" + $contentLength + "`n" + $contentType + "`n" + $xHeaders + "`n" + $resource

    $bytesToHash = [Text.Encoding]::UTF8.GetBytes($stringToHash)
    $keyBytes = [Convert]::FromBase64String($sharedKey)

    $sha256 = New-Object System.Security.Cryptography.HMACSHA256
    $sha256.Key = $keyBytes
    $calculatedHash = $sha256.ComputeHash($bytesToHash)
    $encodedHash = [Convert]::ToBase64String($calculatedHash)
    $authorization = 'SharedKey {0}:{1}' -f $customerId,$encodedHash
    return $authorization
}


# Create the function to create and post the request
Function Post-OMSData($customerId, $sharedKey, $body, $logType)
{
    $method = "POST"
    $contentType = "application/json"
    $resource = "/api/logs"
    $rfc1123date = [DateTime]::UtcNow.ToString("r")
    $contentLength = $body.Length
    $signature = Build-Signature `
        -customerId $customerId `
        -sharedKey $sharedKey `
        -date $rfc1123date `
        -contentLength $contentLength `
        -fileName $fileName `
        -method $method `
        -contentType $contentType `
        -resource $resource
    $uri = "https://" + $customerId + ".ods.opinsights.azure.com" + $resource + "?api-version=2016-04-01"

    $headers = @{
        "Authorization" = $signature;
        "Log-Type" = $logType;
        "x-ms-date" = $rfc1123date;
        "time-generated-field" = $TimeStampField;
    }

    $response = Invoke-WebRequest -Uri $uri -Method $method -ContentType $contentType -Headers $headers -Body $body -UseBasicParsing
    return $response.StatusCode

}

# Submit the data to the API endpoint
Post-OMSData -customerId $customerId -sharedKey $sharedKey -body ([System.Text.Encoding]::UTF8.GetBytes($json)) -logType $logType  
```

### <a name="c-sample"></a>Ukázka C#

```
using System;
using System.Net;
using System.Security.Cryptography;

namespace OIAPIExample
{
    class ApiExample
    {
// An example JSON object, with key/value pairs
        static string json = @"[{""DemoField1"":""DemoValue1"",""DemoField2"":""DemoValue2""},{""DemoField1"":""DemoValue3"",""DemoField2"":""DemoValue4""}]";

// Update customerId to your Operations Management Suite workspace ID
        static string customerId = "xxxxxxxx-xxx-xxx-xxx-xxxxxxxxxxxx";

// For sharedKey, use either the primary or the secondary Connected Sources client authentication key   
        static string sharedKey = "xxxxxxxxxxxxxxxxxxxxxxxxxxxxxx";

// LogName is name of the event type that is being submitted to Log Analytics
        static string LogName = "DemoExample";

// You can use an optional field to specify the timestamp from the data. If the time field is not specified, Log Analytics assumes the time is the message ingestion time
        static string TimeStampField = "";

        static void Main()
        {
// Create a hash for the API signature
            var datestring = DateTime.UtcNow.ToString("r");
            string stringToHash = "POST\n" + json.Length + "\napplication/json\n" + "x-ms-date:" + datestring + "\n/api/logs";
            string hashedString = BuildSignature(stringToHash, sharedKey);
            string signature = "SharedKey " + customerId + ":" + hashedString;

            PostData(signature, datestring, json);
        }

// Build the API signature
        public static string BuildSignature(string message, string secret)
        {
            var encoding = new System.Text.ASCIIEncoding();
            byte[] keyByte = Convert.FromBase64String(secret);
            byte[] messageBytes = encoding.GetBytes(message);
            using (var hmacsha256 = new HMACSHA256(keyByte))
            {
                byte[] hash = hmacsha256.ComputeHash(messageBytes);
                return Convert.ToBase64String(hash);
            }
        }

// Send a request to the POST API endpoint
        public static void PostData(string signature, string date, string json)
        {
            string url = "https://"+ customerId +".ods.opinsights.azure.com/api/logs?api-version=2016-04-01";
            using (var client = new WebClient())
            {
                client.Headers.Add(HttpRequestHeader.ContentType, "application/json");
                client.Headers.Add("Log-Type", LogName);
                client.Headers.Add("Authorization", signature);
                client.Headers.Add("x-ms-date", date);
                client.Headers.Add("time-generated-field", TimeStampField);
                client.UploadString(new Uri(url), "POST", json);
            }
        }
    }
}
```

### <a name="python-sample"></a>Ukázka Python

```
import json
import requests
import datetime
import hashlib
import hmac
import base64

# Update the customer ID to your Operations Management Suite workspace ID
customer_id = 'xxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx'

# For the shared key, use either the primary or the secondary Connected Sources client authentication key   
shared_key = "xxxxxxxxxxxxxxxxxxxxxxxxxxxxxx"

# The log type is the name of the event that is being submitted
log_type = 'WebMonitorTest'

# An example JSON web monitor object
json_data = [{
   "slot_ID": 12345,
    "ID": "5cdad72f-c848-4df0-8aaa-ffe033e75d57",
    "availability_Value": 100,
    "performance_Value": 6.954,
    "measurement_Name": "last_one_hour",
    "duration": 3600,
    "warning_Threshold": 0,
    "critical_Threshold": 0,
    "IsActive": "true"
},
{   
    "slot_ID": 67890,
    "ID": "b6bee458-fb65-492e-996d-61c4d7fbb942",
    "availability_Value": 100,
    "performance_Value": 3.379,
    "measurement_Name": "last_one_hour",
    "duration": 3600,
    "warning_Threshold": 0,
    "critical_Threshold": 0,
    "IsActive": "false"
}]
body = json.dumps(json_data)

#####################
######Functions######  
#####################

# Build the API signature
def build_signature(customer_id, shared_key, date, content_length, method, content_type, resource):
    x_headers = 'x-ms-date:' + date
    string_to_hash = method + "\n" + str(content_length) + "\n" + content_type + "\n" + x_headers + "\n" + resource
    bytes_to_hash = bytes(string_to_hash).encode('utf-8')  
    decoded_key = base64.b64decode(shared_key)
    encoded_hash = base64.b64encode(hmac.new(decoded_key, bytes_to_hash, digestmod=hashlib.sha256).digest())
    authorization = "SharedKey {}:{}".format(customer_id,encoded_hash)
    return authorization

# Build and send a request to the POST API
def post_data(customer_id, shared_key, body, log_type):
    method = 'POST'
    content_type = 'application/json'
    resource = '/api/logs'
    rfc1123date = datetime.datetime.utcnow().strftime('%a, %d %b %Y %H:%M:%S GMT')
    content_length = len(body)
    signature = build_signature(customer_id, shared_key, rfc1123date, content_length, method, content_type, resource)
    uri = 'https://' + customer_id + '.ods.opinsights.azure.com' + resource + '?api-version=2016-04-01'

    headers = {
        'content-type': content_type,
        'Authorization': signature,
        'Log-Type': log_type,
        'x-ms-date': rfc1123date
    }

    response = requests.post(uri,data=body, headers=headers)
    if (response.status_code == 202):
        print 'Accepted'
    else:
        print "Response code: {}".format(response.status_code)

post_data(customer_id, shared_key, body, log_type)
```

## <a name="next-steps"></a>Další kroky

- Umožňuje vytvářet vlastní zobrazení data, která odešlete [Zobrazení návrhu](log-analytics-view-designer.md) .
