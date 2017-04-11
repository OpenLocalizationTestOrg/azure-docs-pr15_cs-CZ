<properties
    pageTitle="Používání zdrojů poskytovatele rozhraní API | Microsoft Azure"
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

# <a name="provider-resource-usage-api"></a>Používání zdrojů poskytovatelem rozhraní API

Zprostředkovatel termínů platí pro správce služby a delegované poskytovatelům. Správce služeb a delegované zprostředkovatelů můžete pomocí rozhraní API použití poskytovatele využití jejich přímé klienti. Například P0 upoutat rozhraní API poskytovatele získat informace o použití o P1 prvku a jeho P2 přímý využití a P1 upoutat informace o použití na P3 a P4.

![Koncepční modelu poskytovatelem hierarchie](media/azure-stack-provider-resource-api/image1.png)


## <a name="api-call-reference"></a>Rozhraní API volání odkaz

### <a name="request"></a>Žádost o

Žádost získá spotřebu podrobnosti pro požadované předplatné a pro požadované časový rámec. Existuje bez žádosti o textu.

Tento použití rozhraní API je API poskytovatelem, volající musí mít přiřazené role vlastníka, Přispěvatel nebo čtenáře v poskytovatele předplatného.

| **Metoda**  | **Žádost o URI** |
| ------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
|  ZÍSKÁNÍ        | https://{armendpoint}/subscriptions/{subId}/providers/Microsoft.Commerce/subscriberUsageAggregates?reportedStartTime={reportedStartTime}&reportedEndTime={reportedEndTime}&aggregationGranularity={granularity}&subscriberId={sub1.1}&api-version=2015-06-01-preview&continuationToken={token-value} |

### <a name="arguments"></a>Argumenty

| **Argument**              | **Popis** |
| -------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| *armendpoint*             | Azure správce prostředků koncový bod prostředí Azure vrstvě. Systém názvů Azure zásobníku je název koncového bodu ARM v https://api formát. {název domény} ". Například pokud název domény je azurestack.local, pak koncový bod ARM budou https://api.azurestack.local. |
| *subId*                   | ID předplatného uživatele, kterému je volání. |
| *reportedStartTime*       | Spuštění dotazu. Hodnota *DateTime* musí být UTC a na začátku hodinu, například 13:00. Pro denní agregace nastavte u této hodnoty na půlnoc UTC. Formát je *uvést* formátu ISO 8601, například 2015 06 16T18 % 3a53 % 3a11 % 2b00 % 3a00Z, kde je dvojtečka uvést k % 3a a plus je uvést % 2b, tak, aby byla URI popisný. |
| *reportedEndTime*         | Čas ukončení dotazu. Omezení, která platí pro *reportedStartTime* také použít tento argument. Hodnota pro *reportedEndTime* nemůže být v budoucnosti. |
| *aggregationGranularity*  | Volitelný parametr, který má dvě samostatná potenciální hodnoty: denně a každou hodinu. Jako hodnoty navrhnout, jednu vrátí data v denní granularity a druhý je hodinové rozlišení. Denní možnost je výchozí hodnota. |
| *subscriberId*            | ID předplatného. Filtrování dat získáte požaduje ID předplatného přímý tenanta, kterého poskytovatele. Pokud není zadán žádný parametr ID předplatného, hovor vrátí použití zásad správy informací pro všechny poskytovatelem přímé klienti. |
| *verze rozhraní API*             | Verzi protokolu, která se používají k vytvoření tohoto požadavku. Je nutné použít 2015 06 01 náhled. |
| *continuationToken*       | Token načtená z kontingenčního seznamu poslední volání použití rozhraní API poskytovatele. To je potřeba při odpověď je větší než 1 000 řádků. Toto je záložka průběhu. Pokud není k dispozici, načítání dat od začátku den nebo hodiny, podle granularity předaný. |



### <a name="response"></a>Odpověď

ZÍSKÁVÁNÍ /subscriptions/sub1/providers/Microsoft.Commerce/subscriberUsageAggregates?reportedStartTime=reportedStartTime=2014-05-01T00%3a00%3a00%2b00%3a00 a reportedEndTime = 2015 06 01T00 % 3a00 % 3a00 % 2b00 % 3a00 & aggregationGranularity = denní & subscriberId = sub1.1 & verze rozhraní api = 1.0

{

"hodnotu":\[

{

"identifikátor": "/subscriptions/sub1.1/providers/Microsoft.Commerce/UsageAggregate/sub1.1-

meterID1 ",

"název": "sub1.1-meterID1"

"typ": "Microsoft.Commerce/UsageAggregate"

"vlastnosti": {

"subscriptionId": "sub1.1"

"usageStartTime": "2015-03-03T00:00:00 + 00:00",

"usageEndTime": "2015-03-04T00:00:00 + 00:00",

"instanceData": "{\\" Microsoft.Resources\\": {\\" resourceUri\\":\\" resourceUri1\\",\\"umístění\\

":\\" Aljaška\\",\\" značky\\": hodnoty null,\\" additionalInfo\\": null}}",

"množství":2.4000000000

"meterId": "meterID1"

}

},

…

### <a name="response-details"></a>Podrobnosti odpovědi


| **Argument**       | **Popis**
| ------------------ | ------------------------------------------------------------------------------------------------------------- |
| *ID*               | Jedinečné ID souhrn využití
| *Jméno*             | Název souhrn využití
| *Typ*             | Definice zdroje
| *subscriptionId*   | Identifikátor URI předplatného uživatele Azure zásobníku
| *usageStartTime*   | UTC spuštění sady použití, ke kterému patří tento použití aggregate
| *usageEndTime*     | Čas ukončení UTC sady použití, ke kterému patří tento použití aggregate
| *instanceData*     | Klíč dvojice detailů instance (v novém formátu):<br> *resourceUri*: plně kvalifikovaný pole číslo ID zdroje, který obsahuje skupiny zdrojů a název instance <br> *umístění*: oblast, ve kterém byl spuštěn tuto službu <br> *značky*: značky zdroje, které jsou zadané uživatelem <br> *additionalInfo*: Další podrobnosti o zdroje spotřebované, třeba typ operační systém verze nebo obrázek |
| *počet*         | Částka využití prostředků, který došlo v této časové rozmezí: |
| *meterId*          | Jedinečné ID pro zdroj, který byl spotřebované množství (taky se nazývá *ResourceID*) |

## <a name="next-steps"></a>Další kroky

[Používání zdrojů klienta rozhraní API odkaz](azure-stack-tenant-resource-usage-api.md)

[Nejčastější dotazy týkající se použití](azure-stack-usage-related-faq.md)
