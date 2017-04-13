<properties
   pageTitle="Jak používat Power BI vložený s ZBÝVAJÍCÍ | Microsoft Azure"
   description="Naučte se používat Power BI vložený s REST "
   services="power-bi-embedded"
   documentationCenter=""
   authors="guyinacube"
   manager="erikre"
   editor=""
   tags=""/>
<tags
   ms.service="power-bi-embedded"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="powerbi"
   ms.date="10/04/2016"
   ms.author="asaxton"/>

# <a name="how-to-use-power-bi-embedded-with-rest"></a>Jak používat Power BI vložený s REST


## <a name="power-bi-embedded-what-it-is-and-what-its-for"></a>Power BI vložený: Co je a k čemu slouží
Základní informace o Power BI vložený je popsán v oficiální [Power BI vložený webu](https://azure.microsoft.com/services/power-bi-embedded/), ale podívejme se snadné před jsme dostat do podrobněji, co s ním pracovat s ZBÝVAJÍCÍ.

Je velmi jednoduché, skutečně. Nezávislý často chce použití dynamických dat vizualizací [Power BI](https://powerbi.microsoft.com) ve své vlastní aplikaci jako uživatelského rozhraní stavebních bloků.

Ale víte, vložení sestavy Power BI a dlaždice do webové stránky je už možné bez službu Power BI vložený Azure pomocí rozhraní **Power BI API**. Když budete chtít sdílení sestav ve stejné organizaci, můžete vložit zprávy s Azure AD ověřování. Uživatele, kterému zobrazení sestavy musí přihlášení svůj uživatelský účet Azure AD. Když budete chtít sdílení sestav pro všechny uživatele (včetně externí uživatele), můžete jednoduše vložit při anonymním přístupu.

Uvidíte, tento jednoduchý vložení řešení, ale úplně nesplňuje požadavky aplikace nezávislí výrobci softwaru.
Většina aplikací nezávislých výrobců softwaru muset poskytovat data pro svoje vlastní zákazníky, ne nutně uživatele ve své vlastní organizační. Například pokud jste předvádění některé služby pro firmy A a B společnosti, uživatelům ve společnosti A mají jenom najdete v části data pro svoje vlastní firmu A. To znamená víceklientská potřebné pro doručování.

Aplikace nezávislých výrobců softwaru může taky nabízející vlastní metody ověřování jako je ověřování formulářů, základní ověřování a další... Potom vkládání řešení musí spolupracovat s toto existující metody ověřování bezpečné. Také je nutné, aby uživatelé moct používat tyto aplikace nezávislí výrobci softwaru bez navíc zakoupit nebo licencování předplatného Power BI.

 **Power BI vložený** je určený pro přesně tyto typy nezávislí výrobci softwaru scénáře. Takže teď máme rychlý úvod zmenšit, nastavíme do některé podrobnosti

Můžete použít .NET \(C#) nebo Node.js SDK, můžete snadno vytvořit aplikaci Power BI vložený. Ale v tomto článku jsme budete vysvětlit o HTTP toku \(včetně AuthN) verzi Power BI bez SDK. Principy tento toku, je možné vytvářet vaše aplikace **s libovolným programovacím jazykem**a můžete velký pochopit podstatu Power BI vložený.

## <a name="create-power-bi-workspace-collection-and-get-access-key-provisioning"></a>Vytvoření Power BI pracovního prostoru shromažďování a získat přístupová klávesa \(Provisioning)
Power BI vložený je jedním ze služby Azure. Pouze nezávislí používajícího Azure portál výrobci softwaru účtovaná za poplatky za použití \(relace hodinové uživatele), a uživatel, který zobrazí sestavu neúčtuje nebo dokonce vyžaduje předplatné Azure.
Před spuštěním naše vývoj aplikací, třeba vytvoříme **kolekce pracovního prostoru Power BI** pomocí portálu Azure.

Každý pracovní prostor aplikace Power BI vložený je pracovního prostoru pro jednotlivé zákazníky (klient) a přičteme hodně pracovních prostorů v každé kolekce pracovního prostoru. Stejné přístupová klávesa se používá v každé kolekce pracovního prostoru. V efekt, pracovního prostoru je hranici zabezpečení pro Power BI vložený.

![](media\power-bi-embedded-iframe\create-workspace.png)

Když jsme dokončete vytváření kolekci pracovního prostoru, zkopírujte přístupová klávesa z portálu Azure.

![](media\power-bi-embedded-iframe\copy-access-key.png)

> [AZURE.NOTE] Můžete taky zřízení kolekci pracovního prostoru jsme získat přístupová klávesa prostřednictvím rozhraní REST API. Další informace najdete v tématu [Power BI zdroje poskytovatele API](https://msdn.microsoft.com/library/azure/mt712306.aspx).

## <a name="create-pbix-file-with-power-bi-desktop"></a>Vytvoření souboru .pbix s Power BI Desktop
Pak musí vytvoříme datového připojení a sestavy vložení.
Pro tento úkol nemůže žádným způsobem programování ani kód. Budeme používat jenom Power BI Desktop.
V tomto článku jsme nebude absolvovat podrobnosti o tom, jak používat Power BI Desktop. Pokud se potřebujete ke nějakou nápovědu tady, přečtěte si článek [Začínáme s Power BI Desktop](https://powerbi.microsoft.com/documentation/powerbi-desktop-getting-started/). Pro zpět k našemu příkladu použijeme jenom [Maloobchodní analyzovaného vzorku](https://powerbi.microsoft.com/documentation/powerbi-sample-datasets/).

![](media\power-bi-embedded-iframe\power-bi-desktop-1.png)

## <a name="create-a-power-bi-workspace"></a>Vytvoření pracovního prostoru aplikace Groove Power BI

Teď zřizování všechny dokončení pusťme se do práce vytvoření pracovního prostoru zákazníka v kolekci pracovního prostoru pomocí rozhraní REST API. Následující HTTP příspěvek požádat o (ZBÝVAJÍCÍ) je vytvoření nového pracovního prostoru v naší existující kolekce pracovního prostoru. V našem příkladu je název kolekce pracovního prostoru **mypbiapp**.
Můžeme jednoduše nastavte přístupová klávesa, které jsme dříve zkopírovali, jako **AppKey**. Je velmi jednoduché ověřování.

**Žádost HTTP**

```
POST https://api.powerbi.com/v1.0/collections/mypbiapp/workspaces
Authorization: AppKey MpaUgrTv5e...
```

**Odpověď HTTP**

```
HTTP/1.1 201 Created
Content-Type: application/json; odata.metadata=minimal; odata.streaming=true
Location: https://wabi-us-east2-redirect.analysis.windows.net/v1.0/collections/mypbiapp/workspaces
RequestId: 4220d385-2fb3-406b-8901-4ebe11a5f6da

{
  "@odata.context": "http://wabi-us-east2-redirect.analysis.windows.net/v1.0/collections/mypbiapp/$metadata#workspaces/$entity",
  "workspaceId": "32960a09-6366-4208-a8bb-9e0678cdbb9d",
  "workspaceCollectionName": "mypbiapp"
}
```

Vrácené **workspaceId** se používá pro následující další hovory rozhraní API. Náš aplikace musíte zachovat tuto hodnotu.

## <a name="import-pbix-file-into-the-workspace"></a>Import souboru .pbix do pracovního prostoru
Každý pracovní prostor můžete hostovat jediný Power BI Desktop soubor s datovou sadu \(včetně nastavení zdroje dat) a sestavy. Náš .pbix soubor jsme můžete importovat do pracovního prostoru, jak ukazuje následující kód. Jak vidíte, jsme nahrajte binární soubor .pbix pomocí MIME multipart protokolu HTTP.

Fragment uri **32960a09-6366-4208-a8bb-9e0678cdbb9d** workspaceId označeným je parametr dotazu **datasetDisplayName** název sady dat k vytvoření. Vytvořené sadě dat obsahuje všechna data týkající se artefakty v .pbix souboru, například jako importovaná data, ukazatel myši na zdroj dat atd.

```
POST https://api.powerbi.com/v1.0/collections/mypbiapp/workspaces/32960a09-6366-4208-a8bb-9e0678cdbb9d/imports?datasetDisplayName=mydataset01
Authorization: AppKey MpaUgrTv5e...
Content-Type: multipart/form-data; boundary="A300testx"

--A300testx
Content-Disposition: form-data

{the content (binary) of .pbix file}
--A300testx--
```

Tento úkol import narazit určitou dobu. Až budete hotovi, můžete požádat naše aplikace stav úkolu pomocí id importu. V našem příkladu importu id je **4eec64dd-533b-47c3-a72c-6508ad854659**.

```
HTTP/1.1 202 Accepted
Content-Type: application/json; charset=utf-8
Location: https://wabi-us-east2-redirect.analysis.windows.net/v1.0/collections/mypbiapp/workspaces/32960a09-6366-4208-a8bb-9e0678cdbb9d/imports/4eec64dd-533b-47c3-a72c-6508ad854659?tenantId=myorg
RequestId: 658bd6b4-b68d-4ec3-8818-2a94266dc220

{"id":"4eec64dd-533b-47c3-a72c-6508ad854659"}
```

Stav pomocí tohoto id importu žádá:

```
GET https://api.powerbi.com/v1.0/collections/mypbiapp/workspaces/32960a09-6366-4208-a8bb-9e0678cdbb9d/imports/4eec64dd-533b-47c3-a72c-6508ad854659
Authorization: AppKey MpaUgrTv5e...
```

Není-li úkol dokončení, může být odpověď HTTP takto:

```
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8
RequestId: 614a13a5-4de7-43e8-83c9-9cd225535136

{
  "id": "4eec64dd-533b-47c3-a72c-6508ad854659",
  "importState": "Publishing",
  "createdDateTime": "2016-07-19T07:36:06.227",
  "updatedDateTime": "2016-07-19T07:36:06.227",
  "name": "mydataset01"
}
```

Pokud je úkol dokončen, může být odpověď HTTP další takto:

```
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8
RequestId: eb2c5a85-4d7d-4cc2-b0aa-0bafee4b1606

{
  "id": "4eec64dd-533b-47c3-a72c-6508ad854659",
  "importState": "Succeeded",
  "createdDateTime": "2016-07-19T07:36:06.227",
  "updatedDateTime": "2016-07-19T07:36:06.227",
  "reports": [
    {
      "id": "2027efc6-a308-4632-a775-b9a9186f087c",
      "name": "mydataset01",
      "webUrl": "https://app.powerbi.com/reports/2027efc6-a308-4632-a775-b9a9186f087c",
      "embedUrl": "https://app.powerbi.com/appTokenReportEmbed?reportId=2027efc6-a308-4632-a775-b9a9186f087c"
    }
  ],
  "datasets": [
    {
      "id": "458e0451-7215-4029-80b3-9627bf3417b0",
      "name": "mydataset01",
      "tables": [
      ],
      "webUrl": "https://app.powerbi.com/datasets/458e0451-7215-4029-80b3-9627bf3417b0"
    }
  ],
  "name": "mydataset01"
}
```

## <a name="data-source-connectivity-and-multi-tenancy-of-data"></a>Připojení zdroje dat \(a víceklientská dat)
Během téměř všechna artefakty .pbix souboru importu do naší pracovního prostoru přihlašovací údaje pro zdroje dat nejsou. Jako výsledek při práci **v režimu DirectQuery**vložený sestavy nelze zobrazit správně. Ale pokud chcete použít **režim importu**, můžete zobrazit sestavy pomocí existujícího importovaná data. V takovém případě jsme musíte nastavit přihlašovací údaje pomocí následujících kroků prostřednictvím ZBÝVAJÍCÍ volání.

Nejdřív jsme musíte nejprve získat zdroj dat brány. Jsme upozorní datovou sadu **id** je dříve vrácených id.

**Žádost HTTP**

```
GET https://api.powerbi.com/v1.0/collections/mypbiapp/workspaces/32960a09-6366-4208-a8bb-9e0678cdbb9d/datasets/458e0451-7215-4029-80b3-9627bf3417b0/Default.GetBoundGatewayDatasources
Authorization: AppKey MpaUgrTv5e...
```

**Odpověď HTTP**

```
GET HTTP/1.1 200 OK
Content-Type: application/json; odata.metadata=minimal; odata.streaming=true
RequestId: 574b0b18-a6fa-46a6-826c-e65840cf6e15

{
  "@odata.context": "http://wabi-us-east2-redirect.analysis.windows.net/v1.0/collections/mypbiapp/workspaces/32960a09-6366-4208-a8bb-9e0678cdbb9d/$metadata#gatewayDatasources",
  "value": [
    {
      "id": "5f7ee2e7-4851-44a1-8b75-3eb01309d0ea",
      "gatewayId": "ca17e77f-1b51-429b-b059-6b3e3e9685d1",
      "datasourceType": "Sql",
      "connectionDetails": "{\"server\":\"testserver.database.windows.net\",\"database\":\"testdb01\"}"
    }
  ]
}
```

Pomocí id vrácené brány a zdroj dat id \(podívejte se na předchozí **gatewayId** a **id** v vrácený výsledek), můžete Změníme pověření tento zdroj dat následujícím způsobem:

**Žádost HTTP**

```
PATCH https://api.powerbi.com/v1.0/collections/mypbiapp/workspaces/32960a09-6366-4208-a8bb-9e0678cdbb9d/gateways/ca17e77f-1b51-429b-b059-6b3e3e9685d1/datasources/5f7ee2e7-4851-44a1-8b75-3eb01309d0ea
Authorization: AppKey MpaUgrTv5e...
Content-Type: application/json; charset=utf-8

{
  "credentialType": "Basic",
  "basicCredentials": {
    "username": "demouser",
    "password": "P@ssw0rd"
  }
}
```

**Odpověď HTTP**

```
HTTP/1.1 200 OK
Content-Type: application/octet-stream
RequestId: 0e533c13-266a-4a9d-8718-fdad90391099
```

V výrobní jsme také nastavit jiné připojovací řetězec pro každou pracovního prostoru pomocí rozhraní REST API. \(tj, jsme můžete rozdělit databázi pro jednotlivé zákazníky.)

Následující změny připojovací řetězec zdroje dat přes ZBÝVAJÍCÍ.

```
POST https://api.powerbi.com/v1.0/collections/mypbiapp/workspaces/32960a09-6366-4208-a8bb-9e0678cdbb9d/datasets/458e0451-7215-4029-80b3-9627bf3417b0/Default.SetAllConnections
Authorization: AppKey MpaUgrTv5e...
Content-Type: application/json; charset=utf-8

{
  "connectionString": "data source=testserver02.database.windows.net;initial catalog=testdb02;persist security info=True;encrypt=True;trustservercertificate=False"
}
```

Nebo můžete použít řádek úrovně zabezpečení v Power BI vložený a můžeme oddělit data pro každého uživatele v jedné sestavy. As a result, jsme zřízení každou zprávu zákazníkům s stejné .pbix \(uživatelského rozhraní, atd.) a různých zdrojů dat.

> [AZURE.NOTE] Pokud používáte **režim importu** místo **režimu DirectQuery**, nejde žádným způsobem aktualizovat modely prostřednictvím rozhraní API. A místního zdroje dat pomocí Power BI brány není zatím podporované v Power BI vložený. Však budete Opravdu chcete sledovat v [Power BI blog](https://powerbi.microsoft.com/blog/) pro novinky a co je tu budoucí vydání.

## <a name="authentication-and-hosting-embedding-reports-in-our-web-page"></a>Ověřování a hostitelem (vložení) sestav v naší webovou stránku

V předchozí rozhraní REST API můžete používáme přístupová klávesa **AppKey** sebe sama jako záhlaví se tak mohli ověřovat. Protože tyto hovory můžete zpracování na straně serveru back-end, je bezpečné.

Ale pokud jsme vložení sestavu naše webovou stránku, tento druh informací o zabezpečení by zacházet sešity pomocí Javascriptového \(frontend). Potom musí být hodnota hlavičky se tak mohli ověřovat umístěna. Pokud naše přístupová klávesa zjistí se zlými úmysly nebo škodlivému kódu, můžete volat všechny operace pomocí tohoto klíče.

Když jsme vložení sestavu naše webovou stránku, používáme musí počítanou token místo přístupová klávesa **AppKey**. Náš aplikace musí vytvořit OAuth Json Web Token \(JWT) tvořené deklarace a počítanou digitální podpis. Jak je znázorněno níže, je tento OAuth JWT tokeny odděleného tečka zakódovaný řetězec.

![](media\power-bi-embedded-iframe\oauth-jwt.png)

Nejdřív jsme třeba připravit vstupní hodnotu, která je podepsána později. Tato hodnota ve formátu Base 64 překódován jako adresa url (rfc4648) řetězec z následujících json, a to jsou odděleny bodu \(.) znak. Později jsme budete popisují, jak získat id sestavy.

> [AZURE.NOTE] Pokud chceme pro práci s Power BI vložený řádek úroveň zabezpečení (RLS), jsme musí taky zadejte **uživatelské jméno** a **role** deklarace.

```
{
  "typ":"JWT",
  "alg":"HS256"
}
```

```
{
  "wid":"{workspace id}",
  "rid":"{report id}",
  "wcn":"{workspace collection name}",
  "iss":"PowerBISDK",
  "ver":"0.2.0",
  "aud":"https://analysis.windows.net/powerbi/api",
  "nbf":{start time of token expiration},
  "exp":{end time of token expiration}
}
```

Pak musí vytvoříme řetězec kódovaný ve formátu Base 64 HMAC \(podpis) s SHA256 algoritmus. Tento podepsané vstupní hodnotu předchozí řetězec.

Poslední, musíte spojením vstupní hodnotu, tak i podpis řetězce pomocí období \(.) znak. Dokončený řetězec je token aplikace pro vložení sestavy. I když se zlými úmysly zjistí token aplikace, dostanou nelze původní přístupová klávesa. Tento token aplikací vyprší rychle.

Tady je příklad PHP k provedení těchto kroků:

```
<?php
// 1. power bi access key
$accesskey = "MpaUgrTv5e...";

// 2. construct input value
$token1 = "{" .
  "\"typ\":\"JWT\"," .
  "\"alg\":\"HS256\"" .
  "}";
$token2 = "{" .
  "\"wid\":\"32960a09-6366-4208-a8bb-9e0678cdbb9d\"," . // workspace id
  "\"rid\":\"2027efc6-a308-4632-a775-b9a9186f087c\"," . // report id
  "\"wcn\":\"mypbiapp\"," . // workspace collection name
  "\"iss\":\"PowerBISDK\"," .
  "\"ver\":\"0.2.0\"," .
  "\"aud\":\"https://analysis.windows.net/powerbi/api\"," .
  "\"nbf\":" . date("U") . "," .
  "\"exp\":" . date("U" , strtotime("+1 hour")) .
  "}";
$inputval = rfc4648_base64_encode($token1) .
  "." .
  rfc4648_base64_encode($token2);

// 3. get encoded signature
$hash = hash_hmac("sha256",
    $inputval,
    $accesskey,
    true);
$sig = rfc4648_base64_encode($hash);

// 4. show result (which is the apptoken)
$apptoken = $inputval . "." . $sig;
echo($apptoken);

// helper functions
function rfc4648_base64_encode($arg) {
  $res = $arg;
  $res = base64_encode($res);
  $res = str_replace("/", "_", $res);
  $res = str_replace("+", "-", $res);
  $res = rtrim($res, "=");
  return $res;
}
?>
```

## <a name="finally-embed-the-report-into-the-web-page"></a>Nakonec vložte sestavu do webové stránky

Pro vložení naše sestavy, musíte získání adresy url embed jsme **id** pomocí následující rozhraní REST API sestavy.

**Žádost HTTP**

```
GET https://api.powerbi.com/v1.0/collections/mypbiapp/workspaces/32960a09-6366-4208-a8bb-9e0678cdbb9d/reports
Authorization: AppKey MpaUgrTv5e...
```

**Odpověď HTTP**

```
HTTP/1.1 200 OK
Content-Type: application/json; odata.metadata=minimal; odata.streaming=true
RequestId: d4099022-405b-49d3-b3b7-3c60cf675958

{
  "@odata.context": "http://wabi-us-east2-redirect.analysis.windows.net/v1.0/collections/mypbiapp/workspaces/32960a09-6366-4208-a8bb-9e0678cdbb9d/$metadata#reports",
  "value": [
    {
      "id": "2027efc6-a308-4632-a775-b9a9186f087c",
      "name": "mydataset01",
      "webUrl": "https://app.powerbi.com/reports/2027efc6-a308-4632-a775-b9a9186f087c",
      "embedUrl": "https://embedded.powerbi.com/appTokenReportEmbed?reportId=2027efc6-a308-4632-a775-b9a9186f087c",
      "isFromPbix": false
    }
  ]
}
```

Náš pomocí ve web appu předchozí token aplikace jsme můžete vložit sestavu.
Pokud se podíváme na Další ukázkové kód, bývalého část je stejné jako v předchozím příkladu. V části poslední Tato ukázka znázorňuje **embedUrl** \(předchozí výsledek) v prvku iframe a posílání token aplikace do iframe.

> [AZURE.NOTE] Budete muset nahraďte hodnotu id sestavy vlastní. Z důvodu chyb v našem systému správy obsahu značka iframe v ukázce kódu je přečtěte si taky, doslova. Odebrání vyloučením nejvyššího a nejnižšího textu z značku, pokud zkopírujete a vložíte tento ukázkový kód.

```
    <?php
    // 1. power bi access key
    $accesskey = "MpaUgrTv5e...";

    // 2. construct input value
    $token1 = "{" .
      "\"typ\":\"JWT\"," .
      "\"alg\":\"HS256\"" .
      "}";
    $token2 = "{" .
      "\"wid\":\"32960a09-6366-4208-a8bb-9e0678cdbb9d\"," . // workspace id
      "\"rid\":\"2027efc6-a308-4632-a775-b9a9186f087c\"," . // report id
      "\"wcn\":\"mypbiapp\"," . // workspace collection name
      "\"iss\":\"PowerBISDK\"," .
      "\"ver\":\"0.2.0\"," .
      "\"aud\":\"https://analysis.windows.net/powerbi/api\"," .
      "\"nbf\":" . date("U") . "," .
      "\"exp\":" . date("U" , strtotime("+1 hour")) .
      "}";
    $inputval = rfc4648_base64_encode($token1) .
      "." .
      rfc4648_base64_encode($token2);

    // 3. get encoded signature value
    $hash = hash_hmac("sha256",
        $inputval,
        $accesskey,
        true);
    $sig = rfc4648_base64_encode($hash);

    // 4. get apptoken
    $apptoken = $inputval . "." . $sig;

    // helper functions
    function rfc4648_base64_encode($arg) {
      $res = $arg;
      $res = base64_encode($res);
      $res = str_replace("/", "_", $res);
      $res = str_replace("+", "-", $res);
      $res = rtrim($res, "=");
      return $res;
    }
    ?>
    <!DOCTYPE html>
    <html>
    <head>
      <meta charset="utf-8" />
      <meta http-equiv="X-UA-Compatible" content="IE=edge">
      <title>Test page</title>
      <meta name="viewport" content="width=device-width, initial-scale=1">
    </head>
    <body>
      <button id="btnView">View Report !</button>
      <div id="divView">
        <**REMOVE THIS CAPPED TEXT IF COPIED** iframe id="ifrTile" width="100%" height="400"></iframe>
      </div>
      <script>
        (function () {
          document.getElementById('btnView').onclick = function() {
            var iframe = document.getElementById('ifrTile');
            iframe.src = 'https://embedded.powerbi.com/appTokenReportEmbed?reportId=2027efc6-a308-4632-a775-b9a9186f087c';
            iframe.onload = function() {
              var msgJson = {
                action: "loadReport",
                accessToken: "<?=$apptoken?>",
                height: 500,
                width: 722
              };
              var msgTxt = JSON.stringify(msgJson);
              iframe.contentWindow.postMessage(msgTxt, "*");
            };
          };
        }());
      </script>
    </body>
```

A tady je naše výsledek:

![](media\power-bi-embedded-iframe\view-report.png)

V současné době Power BI vložený pouze ukazuje sestavu v prvku iframe. Ale sledujte [Power BI Blog](). Další vylepšení pomocí nového klienta rozhraní API, která bude dejte nám odeslat informace do iframe i získat informace. Poutavý věci!


## <a name="see-also"></a>Viz taky
- [Ověřování a autorizace v Power BI vložený](power-bi-embedded-app-token-flow.md)
