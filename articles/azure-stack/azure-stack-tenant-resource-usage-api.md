<properties
    pageTitle="Klient využití prostředků rozhraní API | Microsoft Azure"
    description="Odkaz pro používání zdrojů rozhraní API, které získat informace o využití vrstvě Azure."
    services="azure-stack"
    documentationCenter=""
    authors="AlfredoPizzirani"
    manager="byronr"
    editor=""/>

<tags
    ms.service="azure-stack"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="alfredop"/>

# <a name="tenant-resource-usage-api"></a>Klient využití prostředků rozhraní API

Ke klientovi pomocí rozhraní API klienta zobrazíte data využití prostředků klienta vlastní. Toto rozhraní API odpovídá rozhraní API použití Azure (aktuálně v soukromé verze preview).

Můžete použít rutinu Windows Powershellu **Get-UsageAggregates** získat použití zásad správy informací jako v Azure.

## <a name="api-call"></a>Rozhraní API volání

### <a name="request"></a>Žádost o

Žádost získá spotřebu podrobnosti pro požadované předplatné a pro požadované časový rámec. Existuje bez žádosti o textu.

| **Metoda**  | **Žádost o URI** |
| ------------ | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| ZÍSKÁNÍ         | https://{armendpoint}/subscriptions/{subId}/providers/Microsoft.Commerce/usageAggregates?reportedStartTime={reportedStartTime}&reportedEndTime={reportedEndTime}&aggregationGranularity={granularity}&api-version=2015-06-01-preview&continuationToken={token-value} |

### <a name="arguments"></a>Argumenty

| **Argument**             | **Popis** |
| -------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| *Armendpoint*             | Azure správce prostředků koncový bod prostředí Azure vrstvě. |
| *subId*                   | ID předplatného uživatele, kterému je volání. Toto rozhraní API pouze dotazu můžete použít pro použití jedné předplatného. Poskytovatelé můžete použít poskytovatele zdroje použití rozhraní API pro použití dotazu pro všechny klienty. |
| *reportedStartTime*       | Spuštění dotazu. Hodnota *DateTime* musí být UTC a na začátku hodinu, například 13:00. Pro denní agregace nastavte u této hodnoty na půlnoc UTC. Formát je *uvést* formátu ISO 8601, například 2015 06 16T18 % 3a53 % 3a11 % 2b00 % 3a00Z, kde je dvojtečka uvést k % 3a a plus je uvést % 2b, tak, aby byla URI popisný. |
| *reportedEndTime*         | Čas ukončení dotazu. Omezení, která platí pro *reportedStartTime* také použít tento argument. Hodnota pro *reportedEndTime* nemůže být v budoucnosti. |
| *aggregationGranularity*  | Volitelný parametr, který má dvě samostatná potenciální hodnoty: denně a každou hodinu. Jako hodnoty navrhnout, jednu vrátí data v denní granularity a druhý je hodinové rozlišení. Denní možnost je výchozí hodnota. |
| *subscriberId*            | ID předplatného. Filtrování dat získáte požaduje ID předplatného přímý tenanta, kterého poskytovatele. Pokud není zadán žádný parametr ID předplatného, hovor vrátí použití zásad správy informací pro všechny poskytovatelem přímé klienti. |
| *verze rozhraní API*             | Verzi protokolu, která se používají k vytvoření tohoto požadavku. Je nutné použít 2015 06 01 náhled. |
| *continuationToken*       | Token načtená z kontingenčního seznamu poslední volání použití rozhraní API poskytovatele. To je potřeba při odpověď je větší než 1 000 řádků. Toto je záložka průběhu. Pokud není k dispozici, načítání dat od začátku den nebo hodiny, podle granularity předaný. |

### <a name="response"></a>Odpověď

ZÍSKÁVÁNÍ /subscriptions/sub1/providers/Microsoft.Commerce/UsageAggregates?reportedStartTime=reportedStartTime=2014-05-01T00%3a00%3a00%2b00%3a00 a reportedEndTime = 2015 06 01T00 % 3a00 % 3a00 % 2b00 % 3a00 & aggregationGranularity = denní & verze rozhraní api = 1.0

{

"hodnotu":\[

{

"identifikátor": "/ subscriptions/sub1/providers/Microsoft.Commerce/UsageAggregate/sub1-meterID1",

"název": "sub1-meterID1"

"typ": "Microsoft.Commerce/UsageAggregate"

"vlastnosti": {

"subscriptionId": "sub1"

"usageStartTime": "2015-03-03T00:00:00 + 00:00",

"usageEndTime": "2015-03-04T00:00:00 + 00:00",

"instanceData": "{\\" Microsoft.Resources\\": {\\" resourceUri\\":\\" resourceUri1\\",\\" umístění\\":\\" Aljaška\\",\\" značky\\": hodnoty null,\\" additionalInfo\\": null}}",

"množství":2.4000000000

"meterId": "meterID1"

}

},

…

### <a name="response-details"></a>Podrobnosti odpovědi

| **Argument**      | **Popis** |
| ------------------ | ------------------------------------------------------------------------------------------------------------- |
| *ID*              | Jedinečné ID souhrn využití |
| *Jméno*            | Název souhrn využití |
| *Typ*            | Definice zdroje |
| *subscriptionId*  | Identifikátor URI předplatné Azure uživatele |
| *usageStartTime*  | UTC spuštění sady použití, ke kterému patří tento použití aggregate |
| *usageEndTime*    | Čas ukončení UTC sady použití, ke kterému patří tento použití aggregate |
| *instanceData*    | Klíč dvojice detailů instance (v novém formátu):<br>  *resourceUri*: plně kvalifikovaný pole číslo ID zdroje, včetně skupiny zdrojů a název instance <br>  *umístění*: oblast, ve kterém byl spuštěn tuto službu <br>  *značky*: značky zdroje, které uživatel zadá <br>  *additionalInfo*: Další podrobnosti o zdroje spotřebované, třeba typ operační systém verze nebo obrázek |
| *počet*        | Částka využití prostředků, který došlo v této časové rozmezí: |
| *meterId*         | Jedinečné ID pro zdroj, který byl spotřebované množství (taky se nazývá *ResourceID*) |

## <a name="next-steps"></a>Další kroky

[Nejčastější dotazy týkající se použití](azure-stack-usage-related-faq.md)
