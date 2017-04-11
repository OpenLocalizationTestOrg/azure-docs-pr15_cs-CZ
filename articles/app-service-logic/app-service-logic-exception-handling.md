<properties
   pageTitle="Použití logických operátorů aplikace výjimek | Microsoft Azure"
   description="Další vzorce pro chyby a zpracování s aplikacemi jiných logiky Azure výjimek"
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

# <a name="logic-apps-error-and-exception-handling"></a>Použití logických operátorů aplikace chyby a výjimek

Logiku aplikace obsahuje celá řada nástrojů a vzorků chcete, aby vaše integrací se robustní pružné proti chybám.  Jedním z problémů s všechny Architektura integrace zajistit řádně podporovat zpracování prostoje nebo problémy závislá systémů.  Použití logických operátorů aplikace zajišťuje zpracování chyb prvotřídní možnosti, která nabízí nástroje, které je potřeba pracovat s výjimky a chyb v rámci pracovních postupů.

## <a name="retry-policies"></a>Opakovat zásady

Základní typ výjimky a zpracování chyb je zásadu opakovat.  Tuto zásadu Určuje, zda by měl v případě původní žádost vypršení časového limitu nebo se nepodařilo opakovat akce (žádost, jejichž výsledkem 429 nebo odpověď 5xx).  Ve výchozím nastavení opakovat všechny akce 4 další časy intervalech 20 sekund.  Ano, pokud dostali první žádost `500 Internal Server Error` odpověď modul pracovního postupu pozastaví 20 sekund a pokus o žádost znovu.  Pokud po několika pokusech všechny odpovědi stále výjimku nebo selhání, bude pracovní postup pokračovat a označují stav akci jako `Failed`.

Konfigurace zásad opakovat v **zadávání** určité akce.  Opakovat zásad je možné konfigurovat zkusit intervalů až 4 dobu delší než 1 hodinu.  Úplné informace o zadávání vlastnosti může být [naleznete na webu MSDN][retryPolicyMSDN].

```json
"retryPolicy" : {
      "type": "<type-of-retry-policy>",
      "interval": <retry-interval>,
      "count": <number-of-retry-attempts>
    }
```

Pokud je potřeba vzít HTTP opakovat 4 časy a až 10 minut mezi jednotlivými pokusy byste měli definici takto:

```json
"HTTP": 
{
    "inputs": {
        "method": "GET",
        "uri": "http://myAPIendpoint/api/action",
        "retryPolicy" : {
            "type": "fixed",
            "interval": "PT10M",
            "count": 4
        }
    },
    "runAfter": {},
    "type": "Http"
}
```

Podrobné informace o podporovaných syntaxe získáte v [části zásady opakování na webu MSDN][retryPolicyMSDN].

## <a name="runafter-property-to-catch-failures"></a>Vlastnost RunAfter k zachycení chyb

Každou akci aplikace logiky deklaruje akcí, které nutné udělat předtím, než se spustí akci.  Si můžete představit to jako řazení kroky v pracovním postupu.  Toto řazení se nazývá `runAfter` vlastnost v definici akce.  Je objekt, který popisuje jaké akce a stavy akci by provedení akce.  Ve výchozím nastavení jsou nastavené všechny akce Přidat s použitím návrháře `runAfter` v předchozím kroku, pokud jste v předchozím kroku `Succeeded`.  Však můžete přizpůsobit tuto hodnotu aktivováno akce při předchozí akce jsou `Failed`, `Skipped`, nebo možné množiny tyto hodnoty.  Pokud byste chtěli přidat položky do určené téma Bus služby po určité akce `Insert_Row` nepovede, použijete následující `runAfter` konfigurace:

```json
"Send_message": {
    "inputs": {
        "body": {
            "ContentData": "@{encodeBase64(body('Insert_Row'))}",
            "ContentType": "{ \"content-type\" : \"application/json\" }"
        },
        "host": {
            "api": {
                "runtimeUrl": "https://logic-apis-westus.azure-apim.net/apim/servicebus"
            },
            "connection": {
                "name": "@parameters('$connections')['servicebus']['connectionId']"
            }
        },
        "method": "post",
        "path": "/@{encodeURIComponent('failures')}/messages"
    },
    "runAfter": {
        "Insert_Row": [
            "Failed"
        ]
    }
}
```

Oznámení `runAfter` vlastnost je nastavena na spuštěno, pokud `Insert_Row` akci `Failed`.  Pokud chcete spustit akci, pokud je stav akce `Succeeded`, `Failed`, nebo `Skipped` bude syntaxe:

```json
"runAfter": {
        "Insert_Row": [
            "Failed", "Succeeded", "Skipped"
        ]
    }
```

>[AZURE.TIP] Akce, které spuštění po předchozí akci se nezdařilo a úspěšně dokončena, budou označeny jako `Succeeded`.  Toto chování znamená, že pokud se úspěšně skutečné všechny chyby v pracovním postupu spustit samotné označeno jako `Succeeded`.

## <a name="scopes-and-results-to-evaluate-actions"></a>Obory a výsledky vyhodnocení akce

Podobně jako jak spuštěním po jednotlivých akce, můžete také skupiny akce společně uvnitř [obor](app-service-logic-loops-and-scopes.md) -, které slouží jako logické seskupení akce.  Obory jsou užitečné pro uspořádání akcí logiky aplikace i pro provádění agregační hodnocení se stav obor.  Po dokončení všech akcí v rámci oboru, dostanou obor samotné stav.  Stav obor, je určený se stejnými kritérii jako spustit – Pokud je poslední akce v větev spuštění `Failed` nebo `Aborted` stavu `Failed`.

Je možné `runAfter` označil obor `Failed` aktivováno určité akce pro všechny chyby, ke kterým došlo v rozsahu.  Spuštění po selhání obor umožňuje vytvořit jednu akci zachytit k chybám, pokud se nezdaří *akcích v rozsahu* .

### <a name="getting-the-context-of-failures-with-results"></a>Získávání kontextu selhání s výsledky

Zachycení chyb z oboru je velmi užitečné, ale můžete taky kontextu porozumět přesně akcí, které se nezdařila a chyby nebo stavů, které byly výsledky.  `@result()` Funkce pracovního postupu poskytuje kontext do výsledek všech akcí v rámci oboru.

`@result()`má jeden parametr, název oboru a vrátí matici všechny akce výsledky v daném oboru.  Tyto objekty akce patří stejné atributy jako `@actions()` objektu, včetně času spuštění akce čas ukončení akce, stav akce, akce vstupy, ID korelace akce a akce výstup.  Můžete snadno párování `@result()` pracovat `runAfter` odeslat kontextu všechny akce, které se nezdařilo v rámci oboru.

Pokud chcete spustit akci *pro každou* akci v oboru, `Failed`, můžete spárování `@result()` s akce **[Pole filtru](../connectors/connectors-native-query.md)** a **[ForEach](app-service-logic-loops-and-scopes.md)** smyčce.  To vám umožní filtrovat matici výsledků akce, které se nezdařila.  Můžete udělat matice filtrovaný výsledek a provedení akce pro každý selhání pomocí **ForEach** opakovat.  Tady je příklad dole, následovaný podrobné vysvětlení.  V tomto příkladu odešle žádost HTTP POST s odpověď textu všechny akce, které se nezdařilo v rozsahu `My_Scope`.

```json
"Filter_array": {
    "inputs": {
        "from": "@result('My_Scope')",
        "where": "@equals(item()['status'], 'Failed')"
    },
    "runAfter": {
        "My_Scope": [
            "Failed"
        ]
    },
    "type": "Query"
},
"For_each": {
    "actions": {
        "Log_Exception": {
            "inputs": {
                "body": "@item()['outputs']['body']",
                "method": "POST",
                "headers": {
                    "x-failed-action-name": "@item()['name']",
                    "x-failed-tracking-id": "@item()['clientTrackingId']"
                },
                "uri": "http://requestb.in/"
            },
            "runAfter": {},
            "type": "Http"
        }
    },
    "foreach": "@body('Filter_array')",
    "runAfter": {
        "Filter_array": [
            "Succeeded"
        ]
    },
    "type": "Foreach"
}
```

Tady je podrobné informace o co je nového:

1. **Pole filtru** akci, kterou chcete filtrovat `@result('My_Scope')` k získání požadovaného výsledku všech akcí v rámci`My_Scope`
1. Podmínka **Pole filtru** je any `@result()` položky se stavem rovno `Failed`.  To bude filtrovat pole všechny akce pro výsledky `My_Scope` na pouze matici výsledků selhalo akce.
1. Provedení akce **pro jednotlivá pole** na výstupy **Filtrované pole** .  Tímto postupem provedete akci *pro každou* se nepodařilo akce výsledek, který jsme filtrováno nad.
    - Pokud jste udělali jednoduchá akce, které se nepodařilo akce v rozsahu `foreach` by spustit pouze jednou.  Počet neúspěšných akce způsobí jedna akce za chyba.
1. Odeslat příspěvek HTTP na `foreach` položka odpovědi textu nebo `@item()['outputs']['body']`.  `@result()` Položky obrazce je stejná jako `@actions()` přizpůsobování a můžete analyzovat stejným způsobem.
1. Součástí dvě vlastní záhlaví s názvem selhalo akce `@item()['name']` a selhalo spustit klienta ID sledování `@item()['clientTrackingId']`.

Pro odkaz, tady je příklad typu single `@result()` položky.  Zobrazí se `name`, `body`, a `clientTrackingId` vlastnosti analyzovat v předchozím příkladu.  Je třeba poznamenat, mimo `foreach`, `@result()` vrátí matici tyto objekty.

```json
{
    "name": "Example_Action_That_Failed",
    "inputs": {
        "uri": "https://myfailedaction.azurewebsites.net",
        "method": "POST"
    },
    "outputs": {
        "statusCode": 404,
        "headers": {
            "Date": "Thu, 11 Aug 2016 03:18:18 GMT",
            "Server": "Microsoft-IIS/8.0",
            "X-Powered-By": "ASP.NET",
            "Content-Length": "68",
            "Content-Type": "application/json"
        },
        "body": {
            "code": "ResourceNotFound",
            "message": "/docs/foo/bar does not exist"
        }
    },
    "startTime": "2016-08-11T03:18:19.7755341Z",
    "endTime": "2016-08-11T03:18:20.2598835Z",
    "trackingId": "bdd82e28-ba2c-4160-a700-e3a8f1a38e22",
    "clientTrackingId": "08587307213861835591296330354",
    "code": "NotFound",
    "status": "Failed"
}
```

Pomocí výše uvedené výrazy provádět jiné zpracování vzorků výjimek.  Můžete provést jednu výjimku zpracování akce mimo rozsah, který přijme celé filtrované pole selhání a odebrat `foreach`.  Můžete zahrnout taky další zajímavé vlastnosti `@result()` odpověď nahoře.

## <a name="azure-diagnostics-and-telemetry"></a>Azure diagnostiky a telemetrie

Vzorky výše jsou skvělým způsobem pro zpracování chyby a výjimky v rámci spustit, ale můžete taky určit a odpovídání na chyby nezávisle na spustit vlastní.  [Diagnostika Azure](app-service-logic-monitor-your-logic-apps.md) poskytuje jednoduchý způsob, jak odeslat všechny události pracovního postupu (včetně všech spustit a akci stavy) účet Azure úložiště nebo rozbočovači Azure události.  Můžete sledovat protokoly a metriky nebo publikovat do libovolné preferovaného vyhodnotit spuštění stavy nástroj pro sledování.  Jednou z možností potenciální je stáhnete všechny události prostřednictvím Azure události rozbočovače do [Analýzy proudu](https://azure.microsoft.com/services/stream-analytics/).  V toku analýzy můžete napsat živou dotazy z jakékoli odchylky, průměry nebo k chybám v protokolech diagnostiky.  Technologie pro analýzu toku můžete snadno vytvořit k jiným zdrojům dat, například fronty témata, SQL, DocumentDB a Power BI.

## <a name="next-steps"></a>Další kroky
- [V tématu jak jeden customer integrované robustní zpracování chyb ve vzorcích s aplikacemi jiných logiky](app-service-logic-scenario-error-and-exception-handling.md)
- [Hledání dalších aplikací logiky příklady a scénáře](app-service-logic-examples-and-scenarios.md)
- [Naučte se vytvářet automatizovaných nasazení aplikace logiky](app-service-logic-create-deploy-template.md)
- [Navrhněte a nasazení aplikace použití logických operátorů z aplikace Visual Studio](app-service-logic-deploy-from-vs.md)


<!-- References -->
[retryPolicyMSDN]: https://msdn.microsoft.com/library/azure/mt643939.aspx#Anchor_9