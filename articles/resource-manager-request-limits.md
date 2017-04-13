<properties
   pageTitle="Azure limity žádosti správce prostředků | Microsoft Azure"
   description="Popisuje, jak používat omezení s žádostí o správce prostředků Azure při dosáhli omezení předplatného."
   services="azure-resource-manager"
   documentationCenter="na"
   authors="tfitzmac"
   manager="timlt"
   editor="tysonn"/>

<tags
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/07/2016"
   ms.author="tomfitz"/>

# <a name="throttling-resource-manager-requests"></a>Omezení žádosti správce prostředků

Pro předplatné a každého klienta omezení zdrojů správce žádosti o 15 000 / hod čtení a zápis požadavků na 1 200 / hod. Pokud vaše aplikace nebo skript dosáhne tato omezení, budete muset omezení vašich požadavků. V tomto tématu se dozvíte, jak lze zjistit zbývající požadavky, které musíte před dosažením limit a jak reagovat, když máte limitu.

Až dojdete limit, zobrazí stavový kód HTTP **429 příliš mnoho požadavků**.

Počet požadavků má obor vymezený na předplatného nebo vašeho klienta. Pokud máte víc, se přidají souběžné aplikací vytváření žádosti o ve vašem předplatném požadavky z těchto aplikací můžete určit počet zbývající požadavků.

Požadavky na odběr omezené jsou ty zahrnují předejte předplatné id, třeba načítání zdroje skupiny ve vašem předplatném. Žádosti klienta omezené nezahrnujte svého předplatného id, třeba načítání platná Azure umístění.

## <a name="remaining-requests"></a>Zbývající požadavky

Můžete zjistit počet zbývající požadavků porovnáním záhlaví odpověď. Každý požadavek obsahuje hodnoty pro číslo zbývající čtení a zápis žádosti. Následující tabulka popisuje hlavičky odpovědi, které můžete prozkoumat na tyto hodnoty:

| Odpověď záhlaví | Popis |
| --------------- | ----------- |
| x-MS-ratelimit-Remaining-Subscription-reads | Předplatné omezené přečte zbývající |
| x-MS-ratelimit-Remaining-Subscription-Writes | Předplatné omezené zapíše zbývající |
| x-MS-ratelimit-Remaining-tenant-reads | Pole zbývající přečte klienta rozsah |
| x-MS-ratelimit-Remaining-tenant-Writes | Pole zbývající zapíše klienta rozsah |
| x-MS-ratelimit-Remaining-Subscription-Resource-Requests | Předplatné omezené zbývající požadavky typ zdroje.<br /><br />Tato hodnota záhlaví je vrácena pouze v případě službu má přepsat výchozí limit. Správce prostředků přidá tuto hodnotu místo předplatné čtení nebo zápisy. |
| x-MS-ratelimit-Remaining-Subscription-Resource-entities-Read | Předplatné omezené zbývající požadavky kolekce typ zdroje.<br /><br />Tato hodnota záhlaví je vrácena pouze v případě službu má přepsat výchozí limit. Tuto hodnotu obsahuje počet zbývající kolekce požadavků (seznam zdrojů). |
| x-MS-ratelimit-Remaining-tenant-Resource-Requests | Klienta omezené zbývající požadavky typ zdroje.<br /><br />Toto záhlaví přibude pouze pro požadavky na úrovni klienta a pouze v případě službu má přepsat výchozí limit. Správce prostředků přidá tuto hodnotu místo klienta čtení nebo zápisy. |
| x-MS-ratelimit-Remaining-tenant-Resource-entities-Read | Klienta omezené zbývající požadavky kolekce typ zdroje.<br /><br />Toto záhlaví přibude pouze pro požadavky na úrovni klienta a pouze v případě službu má přepsat výchozí limit. |

## <a name="retrieving-the-header-values"></a>Načítání hodnot záhlaví

Načítání tyto hodnoty záhlaví v kód nebo skript neliší od načítání libovolná hodnota záhlaví. 

Například v **jazyce C#**načtete hodnotu záhlaví z objektu **HttpWebResponse** s názvem **odpověď** s kódem takto:

    response.Headers.GetValues("x-ms-ratelimit-remaining-subscription-reads").GetValue(0)

V **prostředí PowerShell**načtete z operaci vyvolat WebRequest hodnotu záhlaví.

    $r = Invoke-WebRequest -Uri https://management.azure.com/subscriptions/{guid}/resourcegroups?api-version=2016-09-01 -Method GET -Headers $authHeaders
    $r.Headers["x-ms-ratelimit-remaining-subscription-reads"]
    
Nebo pokud chcete vidět zbývající žádosti o ladění, můžete zadat **-ladění** parametr rutiny **prostředí PowerShell** .

    Get-AzureRmResourceGroup -Debug
    
Vracející hodně informací, včetně následující hodnotu odpověď:

    ...
    DEBUG: ============================ HTTP RESPONSE ============================

    Status Code:
    OK

    Headers:
    Pragma                        : no-cache
    x-ms-ratelimit-remaining-subscription-reads: 14999
    ...

V **Azure rozhraní příkazového řádku**načtete hodnotu záhlaví pomocí možnosti podrobnější.

    azure group list -vv --json

Vracející hodně informací, včetně následujících objektu:

    ...
    silly: returnObject
    {
      "statusCode": 200,
      "header": {
        "cache-control": "no-cache",
        "pragma": "no-cache",
        "content-type": "application/json; charset=utf-8",
        "expires": "-1",
        "x-ms-ratelimit-remaining-subscription-reads": "14998",
        ...

## <a name="waiting-before-sending-next-request"></a>Čekání před odesláním další žádosti

Až dojdete limit požadavku, zdroje správce vrátí stavový kód **429** HTTP a **Opakovat po** hodnotu v záhlaví. **Opakovat po** hodnota určuje počet sekund by měl čekat aplikace (nebo sleep) před odesláním dalším požadavku. Pokud odešlete žádost o než hodnota parametru uplynul, není zpracovány žádosti o a vrácená novou hodnotu opakovat.
