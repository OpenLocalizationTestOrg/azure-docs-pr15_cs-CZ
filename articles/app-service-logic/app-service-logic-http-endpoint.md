<properties
   pageTitle="Použití logických operátorů aplikace jako možné volat koncové body"
   description="Postup vytvoření a konfigurace aktivace koncové body a jejich použití v aplikaci logiky v aplikaci služby Azure"
   services="logic-apps"
   documentationCenter=".net,nodejs,java"
   authors="jeffhollan"
   manager="erikre"
   editor=""/>

<tags
   ms.service="logic-apps"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="integration"
   ms.date="10/18/2016"
   ms.author="jehollan"/>


# <a name="logic-apps-as-callable-endpoints"></a>Použití logických operátorů aplikace jako možné volat koncové body

Použití logických operátorů aplikace nativně můžete vystavit synchronní koncový bod HTTP jako aktivační událost.  Můžete taky vzorek možné volat koncové body vyvolat logiky aplikace jako vnořené pracovní postup prostřednictvím "pracovního postupu" akce v aplikaci použití logických operátorů.

Existují 3 typy aktivačních událostí, které můžete přijímat požadavky:

* Žádost o
* ApiConnectionWebhook
* HttpWebhook

Zbývající v článku jako v příkladu použijeme **žádost** , ale všechny zásady použít stejně jako u ostatních 2 typů aktivačních událostí.

## <a name="adding-a-trigger-to-your-definition"></a>Přidání aktivační událost do vaší definice
Cílem prvního kroku je lze přidat aktivační vaší logiky aplikace definici, který může přijímat příchozí žádosti.  Je možné hledat v návrháři pro "HTTP požádat o" přidáte kartu aktivační událost. Můžete definovat textu žádosti o JSON schématu a návrháři vygeneruje tokeny vám pomůže analyzovat a předání dat z ruční spuštění pomocí pracovního postupu.  Můžu doporučujeme, abyste pomocí nástroje, například [jsonschema.net](http://jsonschema.net) vytvářet JSON schématu ze ukázkové datové části textu.

![Karta požadavku aktivační událost][2]

Po uložení logiky aplikace definice adresy URL zpětné vygeneruje se podobná této:
 
``` text
https://prod-03.eastus.logic.azure.com:443/workflows/080cb66c52ea4e9cabe0abf4e197deff/triggers/myendpointtrigger?*signature*...
```

Tato adresa URL zahrnuje klíč přidružení zabezpečení v parametry dotazu použitá k ověření.

Můžete taky dostanete tento koncový bod Azure portálu:

![][1]

Nebo tak, že zavoláte:

``` text
POST https://management.azure.com/{resourceID of your logic app}/triggers/myendpointtrigger/listCallbackURL?api-version=2015-08-01-preview
```

### <a name="security-for-the-trigger-url"></a>Zabezpečení pro adresu URL aktivační událost

Použití logických operátorů aplikace zpětné URL se vytvářejí bezpečně podpisem sdílené aplikace Access.  Podpis prochází jako parametr dotazu a musí ověřit před aplikaci logiky bude platit.  Vygeneruje se jedinečný kombinací tajné klíč za použití logických operátorů aplikace, název aktivační události a prováděnou operaci.  Pokud někdo má přístup k aplikaci klíči tajné použití logických operátorů, nebude moct generovat platný podpis.

## <a name="calling-the-logic-app-triggers-endpoint"></a>Volání koncový bod aktivační událost aplikace logiky

Po vytvoření koncového bodu aktivační události můžete aktivovat ho přes `POST` úplnou adresu URL. V těle může obsahovat další záhlaví a žádný obsah.

Pokud je typ obsahu `application/json` pak budou moct vlastnosti odkazu z do žádosti. V opačném ho se použije jako jednu jednotku binární, který může být předán jiných rozhraní API, ale nemůže odkazovat postupu bez převodu obsah.  Řekněme, že předáte `application/xml` obsahu můžete použít `@xpath()` dělat xpath oddělení, nebo `@json()` převést z XML na JSON.  Další informace o práci s obsahem typů [naleznete zde](app-service-logic-content-type.md)

Kromě toho můžete určit JSON schématu v definici. To způsobí, že návrháře generovat tokenů, které se předávají do kroků.  Například následující pokusy `title` a `name` token k dispozici v Návrháři:

```
{
    "properties":{
        "title": {
            "type": "string"
        },
        "name": {
            "type": "string"
        }
    },
    "required": [
        "title",
        "name"
    ],
    "type": "object"
}
```

## <a name="referencing-the-content-of-the-incoming-request"></a>Odkazování na obsah příchozí žádosti

`@triggerOutputs()` Funkce bude výstupní obsah příchozí žádosti o. Například bude výsledek podobný jako:

```
{
    "headers" : {
        "content-type" : "application/json"
    },
    "body" : {
        "myprop" : "a value"
    }
}
```

Můžete použít `@triggerBody()` zkratka pro přístup k `body` vlastnost zvlášť. 

## <a name="responding-to-the-request"></a>Odpověď na požadavek

Některé požadavky, které spustíte aplikaci použití logických operátorů můžete odpovědět se určitý obsah volajícímu. Existuje nový typ akce s názvem **odpověď** , která slouží k vytvoření stavový kód, textu a záhlaví pro svoji odpověď. Poznámku, pokud je k dispozici žádné obrazce **odpověď** , *okamžitě* bude koncový bod aplikace logiky odpoví **202 přijaté**.

![Odpověď HTTP akce][3]

``` json
"Response": {
            "conditions": [],
            "inputs": {
                "body": {
                    "name": "@{triggerBody()['name']}",
                    "title": "@{triggerBody()['title']}"
                },
                "headers": {
                    "content-type": "application/json"
                },
                "statusCode": 200
            },
            "type": "Response"
        }
```

Odpovědi na ně mít takto:

| Vlastnost | Popis |
| -------- | ----------- |
| statusCode | Stavový kód HTTP odpovědět, příchozí žádosti. Může být libovolný platný stavový kód, který začíná 2xx, 4xx nebo 5xx. nejsou povoleny 3xx stavů. | 
| textu | Objekt textu, který může být řetězec, JSON objektu nebo dokonce binární obsah odkazovat z předchozího kroku. | 
| záhlaví | Můžete definovat libovolný počet záhlaví mají být součástí odpovědi | 

Všechny kroky v aplikaci použití logických operátorů, které jsou zapotřebí pro odpověď, musí projít *60 sekund* pro původní žádost o zaslat odpověď **Pokud pracovní postup se nazývá jako vnořené logiky aplikace**. Pokud nic neudělat odpověď dosažení v rámci 60 sekund pak příchozí žádosti vyprší a dostanete odpověď **408 klienta vypršení časového limitu** HTTP.  Aplikace vnořené použití logických operátorů, nadřazený aplikace použití logických operátorů, zůstanou čekání na odpověď, dokud nebude dokončen, bez ohledu na množství času trvá.

## <a name="advanced-endpoint-configuration"></a>Konfigurace rozšířené koncového bodu

Použití logických operátorů aplikace jste vytvořili v článku podpora pro přímý přístup koncového bodu a vždy používat `POST` způsob, jak začít spustit aplikaci logiky. **Nastavit informace HTTP posluchače** rozhraní API aplikace dříve podporuje i změnou segmentů adresy URL a metodu HTTP. Může i nastavit další zabezpečení nebo vlastní doménu tak, že ho přidáte na hostiteli rozhraní API aplikace (Web app hostované aplikaci rozhraní API). 

Tato funkce je k dispozici prostřednictvím **rozhraní API správy**:
* [Změna způsobu žádosti](https://msdn.microsoft.com/library/azure/dn894085.aspx#SetRequestMethod)
* [Změna adresy URL segmentů žádosti o](https://msdn.microsoft.com/library/azure/7406a8ce-5f9c-4fae-9b0f-e574befb2ee9#RewriteURL)
* Nastavení správy domén rozhraní API na kartě **Konfigurovat** na portálu klasické Azure
* Nastavení zásad kontrolovaní základní ověřování (**odkaz potřeby**)

## <a name="summary-of-migration-from-2014-12-01-preview"></a>Přehled migrace z 2014 12 01 náhledu

|  2014 12 01 náhledu | 2016 06 01 |
|---------------------|--------------------|
| Klepněte na **HTTP posluchače** rozhraní API aplikace | Klikněte na **Ruční spuštění** (bez rozhraní API aplikace povinné) |
| Nastavit informace HTTP posluchače nastavení "*automaticky odešle odpověď*" | Buď zahrnout **odpověď** akce nebo není v definici pracovního postupu |
| Konfigurace ověřování OAuth nebo basic | pomocí rozhraní API správy |
| Konfigurace metoda HTTP | pomocí rozhraní API správy |
| Konfigurace relativní cestu | pomocí rozhraní API správy |
| Vytvořte odkaz příchozí textu přes`@triggerOutputs().body.Content` | Odkaz prostřednictvím`@triggerOutputs().body` |
| **Odeslání HTTP odpověď** akce na HTTP posluchače | Klikněte na **Odpovědět na žádost HTTP** (bez rozhraní API aplikace povinné)


[1]: ./media/app-service-logic-http-endpoint/manualtriggerurl.png
[2]: ./media/app-service-logic-http-endpoint/manualtrigger.png
[3]: ./media/app-service-logic-http-endpoint/response.png
