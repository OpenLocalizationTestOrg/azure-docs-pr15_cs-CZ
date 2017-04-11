<properties 
    pageTitle="Sledování aplikací logiky služby Azure aplikace | Microsoft Azure" 
    description="Postup najdete v článku co máte hotové aplikací logiky" 
    authors="jeffhollan" 
    manager="erikre" 
    editor="" 
    services="logic-apps" 
    documentationCenter=""/>

<tags
    ms.service="logic-apps"
    ms.workload="integration"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="jehollan"/>

# <a name="monitor-your-logic-apps"></a>Sledování aplikací logiky

Po [Vytvoření aplikace logiku](app-service-logic-create-a-logic-app.md)můžete zobrazit celou historii spuštění Azure portálu.  Služby, jako třeba diagnostiky Azure a Azure upozornění můžete taky nastavit ke sledování událostí v reálném čase a upozornění pro zvláštní události jako "když víc než 5 běží v rámci za hodinu."

## <a name="monitor-in-the-azure-portal"></a>Sledování v portálu Azure

Pokud chcete zobrazit historii, vyberte **Procházet**a vyberte **Logiky aplikace**. Zobrazí se seznam všech logických aplikací ve vašem předplatném.  Vyberte aplikaci použití logických operátorů, který chcete sledovat.  Zobrazí se seznam všech akcí a aktivačních událostí, ke kterým došlo pro tuto aplikaci logiky.

![Základní informace](./media/app-service-logic-monitor-your-logic-apps/overview.png)

Existuje několik oddíly v tomto zásuvné, které jsou užitečné:

- **Souhrn** obsahuje **všechny spustí** a **Historie aktivační událost**
    - Seznam **všech spustí** nejnovější aplikaci logiku se spustí.  Klikněte na libovolnou řádek podrobné informace o spuštění nebo klikněte na dlaždici zobrazíte další spuštění.
    - **Historie aktivační událost** jsou uvedeny všechny aktivity aktivační události pro tuto aplikaci logiky.  Aktivační událost aktivity může být "Přeskočené" Vyhledat nová data (například chtějí ověřte, zda byl nový soubor přidána do FTP), "Bylo úspěšné" což znamená, že data vrátil aktivováno logiku aplikace nebo "Neúspěšný" odpovídající chybu v konfiguraci.
- **Diagnostika** umožňuje zobrazit podrobnosti runtime a událostí a přihlášení k odběru [Upozornění Azure](#adding-azure-alerts)

>[AZURE.NOTE] V ostatních v rámci služby logiku aplikace jsou šifrované runtime podrobnosti a události. Jsou pouze dešifrovat požádání zobrazení uživateli. Přístup k těmto událostem lze také řídit podle Azure Role-Based přístup ovládacího prvku (RBAC).

### <a name="view-the-run-details"></a>Zobrazení podrobností o běhu

Tento seznam se spustí zobrazuje **Stav**, **Čas zahájení**a **dobu trvání** zvláštní spustit. Vyberte libovolnou řádku na Zobrazit podrobnosti o spuštění.

Sledování zobrazení ukazuje každý krok spustit, vstupy a výstupy a chybové zprávy, které mohou mít occurre.

![Spustit a akce](./media/app-service-logic-monitor-your-logic-apps/monitor-view.png)

Pokud potřebujete další podrobnosti jako spuštění **ID korelace** (který se dá použít pro rozhraní REST API), můžete kliknutím na tlačítko **Spustit podrobnosti** .  Platí to i všechny kroky, stav a vstupy/výstupy pro spustit.

## <a name="azure-diagnostics-and-alerts"></a>Azure diagnostiky a oznámení

Kromě podrobnosti poskytovanou Azure portálem a rozhraní REST API výše můžete nakonfigurovat aplikaci logiky používat Azure Diagnostika pro další bohaté podrobnosti a ladění.

1. Klikněte na oddíl **diagnostických nástrojů** aplikace zásuvné logiky
1. Klikněte na konfigurovat **Diagnostické nastavení**
1. Nakonfigurujte centrální události účtu úložiště tak, aby posílat data

    ![Azure nastavení diagnostických nástrojů](./media/app-service-logic-monitor-your-logic-apps/diagnostics.png)

### <a name="adding-azure-alerts"></a>Přidání Azure upozornění

Po konfiguraci diagnostiky můžete přidat Azure upozornění aktivováno při určitých mezní hodnoty jsou palce.  V zásuvné **diagnostických nástrojů** vyberte dlaždici **oznámení** a **upozornění na Přidat**.  To vás provede jednotlivými Konfigurace upozornění na základě počtu limity a metriky.

![Azure upozornění metriky](./media/app-service-logic-monitor-your-logic-apps/alerts.png)

Podle potřeby můžete nakonfigurovat **stavu**, **mezní hodnota**a **období** .  Nakonec můžete nakonfigurovat e-mailovou adresu k odeslání oznámení nebo konfigurace webhook.  [Žádost o aktivační události](../connectors/connectors-native-reqres.md) v aplikaci logiky slouží ke spuštění na upozornění i (Pokud chcete například [Publikovat na časová rezerva](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-slack-with-logic-app), [Odeslat text](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-text-message-with-logic-app)nebo [přidejte zprávu do fronty](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-queue-with-logic-app)).

### <a name="azure-diagnostics-settings"></a>Nastavení Azure diagnostických nástrojů

Každá z těchto událostí obsahuje podrobné informace o použití logických operátorů aplikace a událost jako stav.  Tady je příklad událost nastavit jako *ActionCompleted* :

```javascript
{
            "time": "2016-07-09T17:09:54.4773148Z",
            "workflowId": "/SUBSCRIPTIONS/80D4FE69-ABCD-EFGH-A938-9250F1C8AB03/RESOURCEGROUPS/MYRESOURCEGROUP/PROVIDERS/MICROSOFT.LOGIC/WORKFLOWS/MYLOGICAPP",
            "resourceId": "/SUBSCRIPTIONS/80D4FE69-ABCD-EFGH-A938-9250F1C8AB03/RESOURCEGROUPS/MYRESOURCEGROUP/PROVIDERS/MICROSOFT.LOGIC/WORKFLOWS/MYLOGICAPP/RUNS/08587361146922712057/ACTIONS/HTTP",
            "category": "WorkflowRuntime",
            "level": "Information",
            "operationName": "Microsoft.Logic/workflows/workflowActionCompleted",
            "properties": {
                "$schema": "2016-06-01",
                "startTime": "2016-07-09T17:09:53.4336305Z",
                "endTime": "2016-07-09T17:09:53.5430281Z",
                "status": "Succeeded",
                "code": "OK",
                "resource": {
                    "subscriptionId": "80d4fe69-ABCD-EFGH-a938-9250f1c8ab03",
                    "resourceGroupName": "MyResourceGroup",
                    "workflowId": "cff00d5458f944d5a766f2f9ad142553",
                    "workflowName": "MyLogicApp",
                    "runId": "08587361146922712057",
                    "location": "eastus",
                    "actionName": "Http"
                },
                "correlation": {
                    "actionTrackingId": "e1931543-906d-4d1d-baed-dee72ddf1047",
                    "clientTrackingId": "my-custom-tracking-id"
                },
                "trackedProperties": {
                    "myProperty": "<value>"
                }
            }
        }
```

Dvě vlastnosti, které jsou užitečné zejména pro sledování a sledování se *clientTrackingId* *trackedProperties*.  

#### <a name="client-tracking-id"></a>Sledování ID klienta

Klient sledování ID je hodnota, která bude koordinaci události přes logiky aplikaci spustit, včetně nějaké vnořené pracovní postupy, kteří jsou označovaní jako součást aplikace použití logických operátorů.  Toto ID bude mít automaticky generované není-li k dispozici, ale můžete ručně zadat klienta sledování ID od aktivační události předáním `x-ms-client-tracking-id` záhlaví s hodnotou ID v žádosti o aktivační události (žádost o aktivační události údaji, HTTP aktivační událost nebo webhook).

#### <a name="tracked-properties"></a>Sledované vlastnosti

Sledované vlastnosti lze přidat do akce v definici pracovního postupu můžete sledovat vstupů nebo výstupy v datech diagnostických nástrojů.  To může být užitečné, když chcete sledovat data jako "ID objednávky" ve vaší telemetrie.  Pokud chcete přidat sledované vlastnost, zahrnout `trackedProperties` vlastnost na akce.  Sledované vlastnosti se dají jenom sledování jedné akce vstupy a výstupy, ale můžete použít `correlation` vlastnosti událostem ke koordinaci přes akcí v spustit.

```javascript
{
    "myAction": {
        "type": "http",
        "inputs": {
            "uri": "http://uri",
            "headers": {
                "Content-Type": "application/json"
            },
            "body": "@triggerBody()"
        },
        "trackedProperties":{
            "myActionHTTPStatusCode": "@action()['outputs']['statusCode']",
            "myActionHTTPValue": "@action()['outputs']['body']['foo']",
            "transactionId": "@action()['inputs']['body']['bar']"
        }
    }
}
```

### <a name="extending-your-solutions"></a>Rozšíření řešení

Můžete využít tento telemetrie z centrální událost nebo úložiště do jiné služby, jako třeba [Operace správy sadu](https://www.microsoft.com/cloud-platform/operations-management-suite) [Azure toku analýzy](https://azure.microsoft.com/services/stream-analytics/)a [Power BI](https://powerbi.com) mít spuštěný sledování pracovních postupů integrace.

## <a name="next-steps"></a>Další kroky
- [Běžné příklady a scénáře použití logických operátorů aplikace](app-service-logic-examples-and-scenarios.md)
- [Vytvoření šablony logiku aplikace nasazení](app-service-logic-create-deploy-template.md)
- [Funkce integrace Enterprise](app-service-logic-enterprise-integration-overview.md)
