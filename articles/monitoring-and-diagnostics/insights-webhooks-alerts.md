<properties
    pageTitle="Konfigurace webhooks na Azure metrických upozornění | Microsoft Azure"
    description="Přesměrovat Azure upozorňování na jiných systémů není Azure."
    authors="kamathashwin"
    manager="carolz"
    editor=""
    services="monitoring-and-diagnostics"
    documentationCenter="monitoring-and-diagnostics"/>

<tags
    ms.service="monitoring-and-diagnostics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/15/2016"
    ms.author="ashwink"/>

# <a name="configure-a-webhook-on-an-azure-metric-alert"></a>Konfigurace webhook na Azure metrických upozornění

Webhooks umožňují směrovat Azure oznámení na jiných systémů pro po zpracování nebo vlastní akce. Můžete webhook směrování do služby, které odeslání zprávy SMS protokolu chyb, upozorněte na něj tým prostřednictvím služby zasílání zpráv a konverzaci a proveďte libovolné číslo jiné akce na upozornění. Tento článek popisuje jak nastavit webhook na Azure metrických upozornění a vypadá datové části pro HTTP příspěvek webhook. Informace o nastavení a schématu pro protokol činnosti Azure upozornit (upozornění u této události), [na této stránce najdete místo](./insights-auditlog-to-webhook-email.md).

Azure upozornění HTTP publikovat obsah výstrahy ve formátu JSON formát schématu níže uvedených, k webhook URI poskytovaným při vytváření upozornění. Tento identifikátor URI musí být platná HTTP nebo HTTPS koncového bodu. Azure příspěvky jedna položka pro každý požadavek upozornění při aktivaci.

## <a name="configuring-webhooks-via-the-portal"></a>Konfigurace webhooks prostřednictvím portálu

Můžete přidat nebo aktualizovat webhook URI v okně Vytvořit/aktualizovat upozornění na [portálu](https://portal.azure.com/).

![Přidat pravidlo upozornění](./media/insights-webhooks-alerts/Alertwebhook.png)

Můžete taky nakonfigurovat upozornění pro vystavení prezentace webhook URI pomocí [Rutin prostředí PowerShell Azure](./insights-powershell-samples.md#create-alert-rules), [Různé platformy rozhraní příkazového řádku](./insights-cli-samples.md#work-with-alerts)nebo [Azure Monitor REST API](https://msdn.microsoft.com/library/azure/dn933805.aspx).

## <a name="authenticating-the-webhook"></a>Ověřování webhook

Webhook může ověřovat pomocí některý z těchto postupů:

1. **Tokeny se tak mohli ověřovat** – webhook URI je uloženo společně s tokenu ID, např. `https://mysamplealert/webcallback?tokenid=sometokenid&someparameter=somevalue`
2.  **Základní se tak mohli ověřovat** – webhook URI budou uložena společně se uživatelské jméno a heslo, např. `https://userid:password@mysamplealert/webcallback?someparamater=somevalue&foo=bar`

## <a name="payload-schema"></a>Datové schématu

Operace příspěvek obsahuje následující datové JSON a schématu pro všechna upozornění na základě míru.

```JSON
{
"status": "Activated",
"context": {
            "timestamp": "2015-08-14T22:26:41.9975398Z",
            "id": "/subscriptions/s1/resourceGroups/useast/providers/microsoft.insights/alertrules/ruleName1",
            "name": "ruleName1",
            "description": "some description",
            "conditionType": "Metric",
            "condition": {
                        "metricName": "Requests",
                        "metricUnit": "Count",
                        "metricValue": "10",
                        "threshold": "10",
                        "windowSize": "15",
                        "timeAggregation": "Average",
                        "operator": "GreaterThanOrEqual"
                },
            "subscriptionId": "s1",
            "resourceGroupName": "useast",                                
            "resourceName": "mysite1",
            "resourceType": "microsoft.foo/sites",
            "resourceId": "/subscriptions/s1/resourceGroups/useast/providers/microsoft.foo/sites/mysite1",
            "resourceRegion": "centralus",
            "portalLink": "https://portal.azure.com/#resource/subscriptions/s1/resourceGroups/useast/providers/microsoft.foo/sites/mysite1"
},
"properties": {
              "key1": "value1",
              "key2": "value2"
              }
}
```


| Pole | Povinné | Pevné sadu hodnot | Poznámky |
| :-------------| :-------------   | :-------------   | :-------------   |
|Stav|Y|"Aktivován", "Vyřešit"|Stav pro upozornění na základě mimo podmínky jste nastavili.|
|kontext| Y | | Upozornění kontext.|
|časové razítko| Y | | Čas, kdy byla výstraha.|
|ID | Y | | Každé pravidlo výstrahy obsahuje jedinečné id.|
|Jméno               |Y                  |                   | Název upozornění.|
|Popis        |Y                  |                           |Popis upozornění.|
|conditionType      |Y                  |"Metrických", "Událost"          |Dva typy upozornění nejsou podporované. Jeden založené na podmínce metrických a druhý podle události v protokolu činnosti. Použijte tuto hodnotu ke kontrole, pokud se upozornění podle metrický nebo události.|
|podmínky          |Y                  |                           | Konkrétní pole vyhledat podle conditionType.|
|metricName         |Metriky upozornění  |                           |Název, který definuje sleduje pravidlo míru.|
|metricUnit         |Metriky upozornění  |"Bajtů", "BytesPerSecond", "počet slov", "CountPerSecond", "Procentuálně", "Sekund"|     Jednotka povolené v metrice. [Tady jsou vypsané povolené hodnoty](https://msdn.microsoft.com/library/microsoft.azure.insights.models.unit.aspx).|
|metricValue        |Metriky upozornění  |                           |Skutečná hodnota míru, která způsobila upozornění.|
|mezní hodnota          |Metriky upozornění  |                           |Mezní hodnota, pro niž upozornění na aktivaci.|
|velikost_okna         |Metriky upozornění  |                           |Po dobu použitý sledování upozornění podle je mezní hodnota. Musí být mezi 5 minut a 1 den. Formát formátu ISO 8601 doby trvání.|
|Agregace času    |Metriky upozornění  |"Průměru", "Příjmení", "Nejvyšší", "Minimální", "Žádná", "celkové" | Jak by měl data, která se shromažďují zkombinovat určitou dobu. Výchozí hodnota je průměr. [Tady jsou vypsané povolené hodnoty](https://msdn.microsoft.com/library/microsoft.azure.insights.models.aggregationtype.aspx).|
|operátor           |Metriky upozornění  |                           |Operátor použitý k porovnání aktuální metrických data, která chcete nastavit mezní.|
|subscriptionId     |Y                  |                           |ID Azure předplatného.|
|resourceGroupName  |Y                  |                           |Název skupiny zdrojů dotčeném zdroje.|
|resourceName       |Y                  |                           |Název zdroje dotčeném zdroje.|
|resourceType       |Y                  |                           |Typ zdroje dotčeném zdroje.|
|resourceId         |Y                  |                           |Pole číslo ID zdroje dotčeném zdroje.|
|resourceRegion     |Y                  |                           |Oblast nebo umístění dotčeném zdroje.|
|portalLink         |Y                  |                           |Přímý odkaz na souhrnné stránce portálu zdroje.|
|Vlastnosti         |N                  |Volitelné                   |Sada `<Key, Value>` dvojice (tedy `Dictionary<String, String>`), který bude zahrnovat podrobností o události. Vlastnosti pole není povinné. Ve vlastní uživatelské rozhraní nebo použití logických operátorů aplikace založené pracovní postup mohou uživatelé zadávat/hodnoty klíče, které mohou být předána prostřednictvím datové. Alternativní předat vlastní vlastnosti zpět webhook se prostřednictvím uri webhook (jako parametry dotazu)|


>[AZURE.NOTE] Vlastnosti pole můžete nastavit jenom pomocí [Rozhraní REST API Azure Monitor](https://msdn.microsoft.com/library/azure/dn933805.aspx).

## <a name="next-steps"></a>Další kroky

- Další informace o Azure upozornění a webhooks videa [Integrace Azure upozornění s PagerDuty](http://go.microsoft.com/fwlink/?LinkId=627080)
- [Spustit skripty pro automatizaci Azure (Runbooks) v Azure upozornění](http://go.microsoft.com/fwlink/?LinkId=627081)
- [Použití logických operátorů aplikace odeslání zprávy pomocí Twilio z Azure upozornění](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-text-message-with-logic-app)
- [Použití logických operátorů aplikace posílání časová rezerva zprávy z Azure upozornění](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-slack-with-logic-app)
- [Použití logických operátorů aplikace můžete poslat zprávu do fronty Azure z Azure upozornění](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-queue-with-logic-app)
