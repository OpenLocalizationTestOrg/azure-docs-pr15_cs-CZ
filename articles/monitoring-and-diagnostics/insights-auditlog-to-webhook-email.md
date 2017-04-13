<properties
    pageTitle="Konfigurace webhook v protokolu činnosti Azure upozornění | Microsoft Azure"
    description="Přečtěte si, jak pomocí protokolu činnosti upozornění můžete volat webhooks. "
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
    ms.date="10/20/2016"
    ms.author="ashwink"/>

# <a name="configure-a-webhook-on-an-azure-activity-log-alerts"></a>Konfigurace webhook na Azure protokolu činnosti upozornění

Webhooks umožňují směrovat Azure oznámení na jiných systémů pro po zpracování nebo vlastní akce. Můžete webhook směrování do služby, které odeslání zprávy SMS protokolu chyb, upozorněte na něj tým prostřednictvím služby zasílání zpráv a konverzaci a proveďte libovolné číslo jiné akce na upozornění. Tento článek popisuje jak nastavit webhook na protokolu činnosti Azure upozornění a vypadá datové části pro HTTP příspěvek webhook. Informace o nastavení a schématu Azure metrických upozornění [na této stránce najdete místo](insights-webhooks-alerts.md). Můžete taky nastavit upozornění protokolu činnosti odeslání e-mailu při aktivaci.

>[AZURE.NOTE] Tato funkce momentálně v náhledu, takže očekávat proměnných kvality a výkonu.

Můžete nastavit pomocí [Rutin prostředí PowerShell Azure](insights-powershell-samples.md#create-alert-rules), [Různé platformy rozhraní příkazového řádku](insights-cli-samples.md#work-with-alerts)nebo [Azure Monitor REST API](https://msdn.microsoft.com/library/azure/dn933805.aspx)upozornění protokolu činnosti.

## <a name="authenticating-the-webhook"></a>Ověřování webhook
Volitelná přihrádka webhook může ověřovat pomocí některý z těchto postupů:

1. **Tokeny se tak mohli ověřovat** – webhook URI je uloženo společně s tokenu ID, např. `https://mysamplealert/webcallback?tokenid=sometokenid&someparameter=somevalue`
2.  **Základní se tak mohli ověřovat** – webhook URI budou uložena společně se uživatelské jméno a heslo, např. `https://userid:password@mysamplealert/webcallback?someparamater=somevalue&foo=bar`

## <a name="payload-schema"></a>Datové schématu
Operace příspěvek obsahuje následující datové JSON a schématu pro všechna upozornění na základě protokolu činnosti. Toto schéma je podobná té používaný upozornění na základě metrické.

```
{
        "status": "Activated",
        "context": {
                "resourceProviderName": "Microsoft.Web",
                "event": {
                        "$type": "Microsoft.WindowsAzure.Management.Monitoring.Automation.Notifications.GenericNotifications.Datacontracts.InstanceEventContext, Microsoft.WindowsAzure.Management.Mon.Automation",
                        "authorization": {
                                "action": "Microsoft.Web/sites/start/action",
                                "scope": "/subscriptions/s1/resourcegroups/rg1/providers/Microsoft.Web/sites/leoalerttest"
                        },
                        "eventDataId": "327caaca-08d7-41b1-86d8-27d0a7adb92d",
                        "category": "Administrative",
                        "caller": "myname@mycompany.com",
                        "httpRequest": {
                                "clientRequestId": "f58cead8-c9ed-43af-8710-55e64def208d",
                                "clientIpAddress": "104.43.166.155",
                                "method": "POST"
                        },
                        "status": "Succeeded",
                        "subStatus": "OK",
                        "level": "Informational",
                        "correlationId": "4a40beaa-6a63-4d92-85c4-923a25abb590",
                        "eventDescription": "",
                        "operationName": "Microsoft.Web/sites/start/action",
                        "operationId": "4a40beaa-6a63-4d92-85c4-923a25abb590",
                        "properties": {
                                "$type": "Microsoft.WindowsAzure.Management.Common.Storage.CasePreservedDictionary, Microsoft.WindowsAzure.Management.Common.Storage",
                                "statusCode": "OK",
                                "serviceRequestId": "f7716681-496a-4f5c-8d14-d564bcf54714"
                        }
                },
                "timestamp": "Friday, March 11, 2016 9:13:23 PM",
                "id": "/subscriptions/s1/resourceGroups/rg1/providers/microsoft.insights/alertrules/alertonevent2",
                "name": "alertonevent2",
                "description": "test alert on event start",
                "conditionType": "Event",
                "subscriptionId": "s1",
                "resourceId": "/subscriptions/s1/resourcegroups/rg1/providers/Microsoft.Web/sites/leoalerttest",
                "resourceGroupName": "rg1"
        },
        "properties": {
                "key1": "value1",
                "key2": "value2"
        }
}
```

|Název elementu       |Popis|
|---                |---|
|Stav             |Použít pro metrických upozornění. Vždy nastavte "aktivace" pro upozornění na protokolu činnosti.|
|kontext            |Kontext události.|
|resourceProviderName|Zprostředkovatel zdroje dotčeném zdroje.|
|conditionType      |Vždy "událost,."|
|Jméno               |Název pravidla výstrahy.|
|ID                 |Pole číslo ID zdroje upozornění.|
|Popis        |Popis oznámení jak je to nastavené při vytváření upozornění.|
|subscriptionId     |ID Azure předplatného.|
|časové razítko          |Čas, kdy událost vygenerované službou Azure, které zpracovány žádosti.|
|resourceId         |Pole číslo ID zdroje dotčeném zdroje.|
|resourceGroupName  |Název skupiny zdrojů dotčeném zdroje|
|Vlastnosti         |Sada `<Key, Value>` dvojice (tedy `Dictionary<String, String>`), který bude zahrnovat podrobností o události.|
|události              |Element obsahující metadata o události.|
|povolení      |Vlastnosti RBAC události. Obvykle jedná se o "akce", "role" a "rozsah".|
|kategorie           |Kategorie události. Podporované hodnotami jsou: pro správu, upozornění zabezpečení, ServiceHealth, doporučení.|
|volající             |E-mailovou adresu uživatele, který provádí operace, deklarace nebo deklaraci identity hlavního názvu služby podle dostupnosti. Může být null pro určité systémová volání.|
|correlationId      |Obvykle GUID ve formátu řetězce. Události se correlationId patří do stejné větší akce a obvykle sdílet correlationId.|
|eventDescription   |Statický text popis události.|
|eventDataId        |Jedinečný identifikátor pro událost.|
|ZDROJ_UDÁLOSTI        |Název služby Azure nebo infrastruktury, které vygenerovalo události.|
|požadavek protokolu HTTP        |Obvykle obsahuje "clientRequestId", "clientIpAddress" a "metoda" (metoda HTTP například umístění).|
|úroveň              |Jednu z následujících hodnot: "Kritické", "Chyba", "Upozornění", "Informační" a "Podrobné."|
|operationId        |Obvykle identifikátor GUID sdíleny události, které odpovídají jedné operace.|
|Název operace      |Název operace.|
|Vlastnosti         |Vlastnosti události.|
|Stav             |Řetězec. Stav operace. Běžné hodnotami jsou: "Začít", "Probíhá", "Úspěšně", "Se nezdařila", "Aktivní", "Vyřešit".|
|dílčí stav          |Obvykle obsahuje stavový kód HTTP odpovídající ZBÝVAJÍCÍ hovoru. Mohou také obsahovat ostatní řetězce s popisem o dílčí stav. Běžné hodnoty podřízeného stavu zahrnují: OK (HTTP stavový kód: 200), které byly vytvořeny (HTTP stavový kód: 201), platné (HTTP stavový kód: 202), nevidí žádný obsah (HTTP stavový kód: 204), chybná požádat o (HTTP stavový kód: 400), nebyla nalezena (HTTP stavový kód: 404), konflikt (HTTP stavový kód: 409), vnitřní chyba serveru (HTTP stavový kód: 500), služba není k dispozici (HTTP stavový kód: 503), vypršel časový limit brány (HTTP stavový kód: 504)|

## <a name="next-steps"></a>Další kroky
- [Další informace o protokolu činnosti](monitoring-overview-activity-logs.md)
- [Spustit skripty pro automatizaci Azure (Runbooks) v Azure upozornění](http://go.microsoft.com/fwlink/?LinkId=627081)
- [Použití logických operátorů aplikace odeslání zprávy pomocí Twilio z Azure upozornění](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-text-message-with-logic-app). V tomto příkladu je metrických upozornění, ale může upravit tak, aby práce s protokolu činnosti upozornění.
- [Používejte aplikaci použití logických operátorů k odeslání časová rezerva zprávy z Azure upozornění](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-slack-with-logic-app). V tomto příkladu je metrických upozornění, ale může upravit tak, aby práce s protokolu činnosti upozornění.
- [Použití logických operátorů aplikaci můžete poslat zprávu do fronty Azure z Azure upozornění](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-queue-with-logic-app). V tomto příkladu je metrických upozornění, ale může upravit tak, aby práce s protokolu činnosti upozornění.
