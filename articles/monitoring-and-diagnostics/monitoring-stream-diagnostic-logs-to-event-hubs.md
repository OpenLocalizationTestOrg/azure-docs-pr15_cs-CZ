<properties
    pageTitle="Azure diagnostické protokoly událostí rozbočovače můžete vysílat datovými proudy | Microsoft Azure"
    description="Zjistěte, jak můžete vysílat datovými proudy Azure diagnostické protokoly událostí rozbočovače."
    authors="johnkemnetz"
    manager="rboucher"
    editor=""
    services="monitoring-and-diagnostics"
    documentationCenter="monitoring-and-diagnostics"/>

<tags
    ms.service="monitoring-and-diagnostics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/08/2016"
    ms.author="johnkem"/>

# <a name="stream-azure-diagnostic-logs-to-event-hubs"></a>Azure diagnostické protokoly událostí rozbočovače můžete vysílat datovými proudy

**[Protokoly pro diagnostiku Azure](monitoring-overview-of-diagnostic-logs.md)** můžete streamují v reálném čase nejblíže k libovolné aplikaci pomocí předdefinovaných možnost "Exportovat do události rozbočovače" v portálu nebo povolením Id služeb Bus pravidla v diagnostické nastavení pomocí rutin prostředí PowerShell Azure nebo Azure rozhraní příkazového řádku.

## <a name="what-you-can-do-with-diagnostics-logs-and-event-hubs"></a>Co můžete dělat s rozbočovače události a protokolování diagnostiky
Můžete použít funkci datových proudů pro protokoly pro diagnostiku jen pomocí několika způsoby:

- **Protokoly toku a 3 stran protokolování telemetrie systémy** – s časem události rozbočovače streamování stane mechanismus kanálu diagnostických protokolů do SIEMs třetích stran a přihlásit se analýzy řešení.

- **Zobrazení stavu služeb je tak, že streamování "kritická cesta" data PowerBI** – pomocí rozbočovače události, toku technologie pro analýzu a PowerBI, můžete snadno transformace dat diagnostiky do poblíž v reálném čase přehledy na služby Azure. [V tomto článku si přečtěte následující dokumentaci najdete skvělý přehled nastavení rozbočovače události, zpracování dat pomocí analýzy toku a použití PowerBI jako výstup](../stream-analytics/stream-analytics-power-bi-dashboard.md). Tady je několik tipů pro získání nastavit diagnostických protokolů:
    - Rozbočovače událostí pro kategorii protokoly pro diagnostiku automaticky se vytvoří při zaškrtnout v portálu nebo ji povolit prostřednictvím Powershellu, který chcete vybrat rozbočovače událostí v oboru služby Bus s názvem, který začíná na "přehledy-"
    - Tady je ukázka toku analýzy dotaz, který umožňuje jednoduše analyzovat všechna protokolu data do tabulky PowerBI:

```
SELECT
records.ArrayValue.[Properties you want to track]
INTO
[OutputSourceName – the PowerBI source]
FROM
[InputSourceName] AS e
CROSS APPLY GetArrayElements(e.records) AS records
```

- **Vytvořit vlastní telemetrie a protokolování platformu** – Pokud jste už uživatelském telemetrie platformu nebo jsou jenom přemýšlíte o vytváření jednu, vysoce scalable publikovat přihlášením druh události rozbočovače umožňuje pružně jedí diagnostické protokoly. [Průvodce najdete v článku Dan Rosanova prvku pomocí události rozbočovače telemetrie platformě globální měřítko tady](https://azure.microsoft.com/documentation/videos/build-2015-designing-and-sizing-a-global-scale-telemetry-platform-on-azure-event-Hubs/).

## <a name="enable-streaming-of-diagnostic-logs"></a>Povolit streamování diagnostických protokolů
Můžete povolit streamování protokoly pro diagnostiku programově prostřednictvím portálu nebo pomocí [Rozhraní REST API Azure Monitor](https://msdn.microsoft.com/library/azure/dn931943.aspx). V obou případech, který jste si vybrali Namespace Bus služby a rozbočovače události se vytvoří v oboru pro každou kategorii protokol, který povolíte. Diagnostických **Protokolů kategorie** je typ protokol, který může shromažďovat zdroje. Můžete vybrat protokolu kategorií, které chcete shromáždit určitého zdroje na portálu Azure klikněte v části Diagnostics zásuvné.

![Protokol kategorií v portálu](./media/monitoring-stream-diagnostic-logs-to-event-hubs/log-categories.png)

> [AZURE.WARNING] Povolení a datových proudů protokoly pro diagnostiku z výpočetním zdrojů (například VMs nebo služby struktury) [vyžaduje jinou sadu jednoduchých kroků](../event-hubs/event-hubs-streaming-azure-diags-data.md).

### <a name="via-powershell-cmdlets"></a>Pomocí rutin prostředí PowerShell
Pokud chcete povolit streamování pomocí [Rutin prostředí PowerShell Azure](insights-powershell-samples.md), můžete použít `Set-AzureRmDiagnosticSetting` rutina s těmito parametry:

```
Set-AzureRmDiagnosticSetting -ResourceId [your resource Id] -ServiceBusRuleId [your service bus rule id] -Enabled $true
```

ID služeb Bus pravidla je řetězec s v tomto formátu: `{service bus resource ID}/authorizationrules/{key name}`, například `/subscriptions/{subscription ID}/resourceGroups/Default-ServiceBus-WestUS/providers/Microsoft.ServiceBus/namespaces/{service bus namespace}/authorizationrules/RootManageSharedAccessKey`.


### <a name="via-azure-cli"></a>Přes Azure rozhraní příkazového řádku
Pokud chcete povolit datových proudů prostřednictvím [Rozhraní příkazového řádku Azure](insights-cli-samples.md), můžete použít `insights diagnostic set` příkazu takto:

```
azure insights diagnostic set --resourceId <resourceId> --serviceBusRuleId <serviceBusRuleId> --enabled true
```

Použijte stejný formát ID pravidla Bus služby způsobem popsaným pro rutiny prostředí PowerShell.

###<a name="via-azure-portal"></a>Prostřednictvím Azure portálu
Aby datových proudů prostřednictvím portálu Azure, přejděte na nastavení diagnostických nástrojů zdroje a vyberte "Exportovat do události centrální."

![Export do rozbočovače události na portálu](./media/monitoring-stream-diagnostic-logs-to-event-hubs/portal-export.png)

Ji nakonfigurovat, vyberte existující Namespace Bus služby. Obor názvů vybrané bude místo, kam je rozbočovače události vytvořili (pokud to vaše první čas streamování protokoly pro diagnostiku) nebo streamují k (pokud jsou už prostředky, které jsou streamování protokolu kategorie se tento obor názvů), a zásada definuje oprávnění, která obsahuje streamování mechanismus. Dnes datových proudů události rozbočovače vyžaduje oprávnění spravovat, číst a odesílat. Můžete vytvořit nebo upravit zásady přístupu sdílené Namespace Bus služby na portálu klasické klikněte v části "Konfigurovat" pro vaše Namespace Bus služby. Klient aktualizujte některé z těchto diagnostiky nastavení, musí mít oprávnění ListKey na služby Bus ověřovací pravidlo.

##<a name="how-do-i-consume-the-log-data-from-event-hubs"></a>Jak používat protokolu data z rozbočovače událostí?
Tady je ukázka výstupní data z rozbočovače událostí:

```
{
    "records": [
        {
            "time": "2016-07-15T18:00:22.6235064Z",
            "workflowId": "/SUBSCRIPTIONS/DF602C9C-7AA0-407D-A6FB-EB20C8BD1192/RESOURCEGROUPS/JOHNKEMTEST/PROVIDERS/MICROSOFT.LOGIC/WORKFLOWS/JOHNKEMTESTLA",
            "resourceId": "/SUBSCRIPTIONS/DF602C9C-7AA0-407D-A6FB-EB20C8BD1192/RESOURCEGROUPS/JOHNKEMTEST/PROVIDERS/MICROSOFT.LOGIC/WORKFLOWS/JOHNKEMTESTLA/RUNS/08587330013509921957/ACTIONS/SEND_EMAIL",
            "category": "WorkflowRuntime",
            "level": "Error",
            "operationName": "Microsoft.Logic/workflows/workflowActionCompleted",
            "properties": {
                "$schema": "2016-04-01-preview",
                "startTime": "2016-07-15T17:58:55.048482Z",
                "endTime": "2016-07-15T18:00:22.4109204Z",
                "status": "Failed",
                "code": "BadGateway",
                "resource": {
                    "subscriptionId": "df602c9c-7aa0-407d-a6fb-eb20c8bd1192",
                    "resourceGroupName": "JohnKemTest",
                    "workflowId": "243aac67fe904cf195d4a28297803785",
                    "workflowName": "JohnKemTestLA",
                    "runId": "08587330013509921957",
                    "location": "westus",
                    "actionName": "Send_email"
                },
                "correlation": {
                    "actionTrackingId": "29a9862f-969b-4c70-90c4-dfbdc814e413",
                    "clientTrackingId": "08587330013509921958"
                }
            }
        },
        {
            "time": "2016-07-15T18:01:15.7532989Z",
            "workflowId": "/SUBSCRIPTIONS/DF602C9C-7AA0-407D-A6FB-EB20C8BD1192/RESOURCEGROUPS/JOHNKEMTEST/PROVIDERS/MICROSOFT.LOGIC/WORKFLOWS/JOHNKEMTESTLA",
            "resourceId": "/SUBSCRIPTIONS/DF602C9C-7AA0-407D-A6FB-EB20C8BD1192/RESOURCEGROUPS/JOHNKEMTEST/PROVIDERS/MICROSOFT.LOGIC/WORKFLOWS/JOHNKEMTESTLA/RUNS/08587330012106702630/ACTIONS/SEND_EMAIL",
            "category": "WorkflowRuntime",
            "level": "Information",
            "operationName": "Microsoft.Logic/workflows/workflowActionStarted",
            "properties": {
                "$schema": "2016-04-01-preview",
                "startTime": "2016-07-15T18:01:15.5828115Z",
                "status": "Running",
                "resource": {
                    "subscriptionId": "df602c9c-7aa0-407d-a6fb-eb20c8bd1192",
                    "resourceGroupName": "JohnKemTest",
                    "workflowId": "243aac67fe904cf195d4a28297803785",
                    "workflowName": "JohnKemTestLA",
                    "runId": "08587330012106702630",
                    "location": "westus",
                    "actionName": "Send_email"
                },
                "correlation": {
                    "actionTrackingId": "042fb72c-7bd4-439e-89eb-3cf4409d429e",
                    "clientTrackingId": "08587330012106702632"
                }
            }
        }
    ]
}
```

| Název elementu | Popis                                            |
|--------------|--------------------------------------------------------|
|záznamy       | Matice protokolu událostí v této části.            |
|čas          | Čas, kdy došlo k události.                      |
|kategorie      | Protokol kategorie pro události.                           |
|resourceId    | Pole číslo ID zdroje zdroje, které vygenerovalo Tato událost. |
|Název operace | Název operace.                                 |
|úroveň         | Volitelné. Určuje úroveň protokolování událostí.               |
|Vlastnosti    | Vlastnosti události.                               |


Můžete zobrazit seznam všech poskytovatelů zdroje, které podporují na centrální události datového proudu [tady](monitoring-overview-of-diagnostic-logs.md).

##<a name="next-steps"></a>Další kroky
- [Přečtěte si něco víc o protokoly pro diagnostiku Azure](monitoring-overview-of-diagnostic-logs.md)
- [Začínáme s rozbočovače události](../event-hubs/event-hubs-csharp-ephcs-getstarted.md)
