<properties 
   pageTitle="Sledovat protokoly přístup a výkonu a metriky aplikace brány | Microsoft Azure"
   description="Zjistěte, jak povolit a spravovat přístup a výkon protokoly aplikace brány"
   services="application-gateway"
   documentationCenter="na"
   authors="amitsriva"
   manager="rossort"
   editor="tysonn"
   tags="azure-resource-manager"
/>
<tags 
   ms.service="application-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="09/26/2016"
   ms.author="amitsriva" />

# <a name="diagnostics-logging-and-metrics-for-application-gateway"></a>Protokolování diagnostiky a metriky pro bráně aplikace

Azure poskytuje možnost sledování zdroje s protokolování a metriky

[**Protokolování**](#enable-logging-with-powershell) - protokolování umožňuje výkonu, přístup a jiné protokolů ukládání nebo spotřebované množství od zdroje pro účely sledování.

[**Metriky**](#metrics) – aktuálně brány aplikace obsahuje jednu míru. Tento míru opatření výkon brány aplikace v bajtech sekundu.

Různé druhy protokoly v Azure umožňuje spravovat a odstraňovat brány aplikace. Některé z těchto protokoly můžete k nim získat přístup prostřednictvím portálu a všechny protokoly extrahovaných z úložiště objektů blob Azure a zobrazit v různých nástrojů, jako jsou [Protokolu analýzy](../log-analytics/log-analytics-azure-networking-analytics.md), Excel a PowerBI. Je další informace o různých typů protokolů z následujícího seznamu:

- **Protokolů auditování:** Chcete-li zobrazit všechny operace při odesílání do předplatného Azure a jejich stav můžete [Protokolů auditování Azure](../monitoring-and-diagnostics/insights-debugging-with-events.md) (dříve označované jako provozní protokoly). Protokolů auditování jsou standardně a můžete zobrazit v portálu Azure náhledu.
- **Přístup k protokolů:** Tento protokol můžete použít k prohlížení aplikace bráně aplikace access vzorku a analýze důležité informace včetně volajícího IP adresu URL na vyžádání latence odpověď, nebo zmenšit vrátit kód, bajtů. Přístup k protokolu se shromažďují každých 300 sekund. Tento protokol obsahuje jeden záznam pro každou instanci aplikace bráně. Brána instance aplikace můžete označenými vlastnost "ID instance".
- **Protokolování výkonu:** Chcete-li zobrazit, jak funguje aplikace instancí bran můžete tento protokol. Tento protokol zaznamenává informace o výkonu na základě jednotlivé instance včetně celkové žádost podávané množství, výkon v bajtech, podávané množství celkový počet požadavků, počet neúspěšných požadavku, počet správný a chybná back-end instancí. Protokolování výkonu se shromažďují každých 60 sekund.
- **Brána firewall protokolů:** Použijte tento protokol zobrazíte požadavky, které jsou zaznamenány prostřednictvím zjišťování nebo zabránění režimu aplikace brány, který nastaven firewall webové aplikace.

>[AZURE.WARNING] Protokoly jsou dostupné jenom pro zdroje nasazenou v modelu nasazení Správce prostředků. Protokoly nelze použít pro zdroje v modelu klasické nasazení. Pro lepší znalost dvou modelů, referenční článek [Správce prostředků Principy nasazení a klasické nasazení](../resource-manager-deployment-model.md) .

## <a name="enable-logging-with-powershell"></a>Povolení protokolování pomocí prostředí PowerShell

Protokolování auditu je automaticky povoleno pro každý zdroj správce prostředků. Je třeba povolit přístup a výkon protokolování zahájíte shromažďování dat prostřednictvím těchto protokoly. Chcete-li povolit protokolování, najdete v článku podle těchto kroků: 

1. Poznámka: účtu úložiště pole číslo ID zdroje, kam se ukládají data protokolu. To bude ve formuláři: /subscriptions/\<subscriptionId\>/resourceGroups/\<název skupiny prostředků\>/providers/Microsoft.Storage/storageAccounts/\<název účtu úložiště\>. Můžete použít libovolný úložiště účet předplatného. Pomocí portálu Náhled můžete tyto informace.

    ![Zobrazení náhledu portálu - diagnostiky brány aplikace](./media/application-gateway-diagnostics/diagnostics1.png)

2. Poznámka: brány aplikace pole číslo ID zdroje pro které protokolování se má povolit. To bude ve formuláři: /subscriptions/\<subscriptionId\>/resourceGroups/\<název skupiny prostředků\>/providers/Microsoft.Network/applicationGateways/\<název brány aplikace\>. Pomocí portálu náhledu můžete tyto informace.

    ![Zobrazení náhledu portálu - diagnostiky brány aplikace](./media/application-gateway-diagnostics/diagnostics2.png)

3. Povolení protokolování diagnostiky pomocí následující rutinu powershellu:

        Set-AzureRmDiagnosticSetting  -ResourceId /subscriptions/<subscriptionId>/resourceGroups/<resource group name>/providers/Microsoft.Network/applicationGateways/<application gateway name> -StorageAccountId /subscriptions/<subscriptionId>/resourceGroups/<resource group name>/providers/Microsoft.Storage/storageAccounts/<storage account name> -Enabled $true  

>[AZURE.INFORMATION] protokolů auditování nevyžadují účet samostatný úložiště. Použití úložiště pro access a výkonu protokolování poněkud poplatky.

## <a name="enable-logging-with-azure-portal"></a>Povolení protokolování portálem Azure

### <a name="step-1"></a>Krok 1

Přejděte na zdroje na portálu Azure. Klikněte na **protokoly pro diagnostiku**. Pokud se jedná poprvé konfigurace Diagnostika zásuvné vypadá jako na následujícím obrázku:

Aplikace brány nejsou k dispozici 3 protokoly.

- Přístup k protokolu
- Protokolování výkonu
- Protokol brány firewall

Klikněte na tlačítko start shromažďování dat **Zapnout diagnostických nástrojů** .

![zásuvné nastavení diagnostických nástrojů][1]

### <a name="step-2"></a>Krok 2

Na zásuvné **Nastavení diagnostických nástrojů** jako jak diagnostických protokolů nastavení. V tomto příkladu protokolu analýzy slouží k uložení protokoly. V části **Log technologie pro analýzu** konfigurace pracovního prostoru klikněte na **Konfigurovat** . Událost rozbočovače a účet úložiště lze uložit protokolech diagnostiky.

![Diagnostika zásuvné][2]

### <a name="step-3"></a>Krok 3

Vyberte stávající schůzek OMS nebo vytvořte nový účet. V tomto příkladě se používá již existujícího.

![pracovní prostory OMS][3]

### <a name="step-4"></a>Krok 4

Až budete hotovi, potvrďte nastavení a klikněte na tlačítko **Uložit** uložte nastavení.

![Potvrďte výběr][4]

## <a name="audit-log"></a>Protokol auditování

Protokol (dříve označované jako "provozní protokol") je generováno aplikací Azure ve výchozím nastavení.  Protokoly zachovají 90 dnů v úložišti Azure na protokoly událostí. Další informace o těchto protokoly článek [Zobrazit události a protokolů auditování](../monitoring-and-diagnostics/insights-debugging-with-events.md) .

## <a name="access-log"></a>Přístup k protokolu

Tento protokol vytváří pouze, pokud jste ho povolili na jednoho základna brány aplikace podle předchozích kroků. Uložení dat v okně úložiště účet, který jste zadali při povolíte protokolování. Jednotlivé aplikace brány je přihlášeni ve formátu JSON, jak je vidět v následujícím příkladu:

    {
        "resourceId": "/SUBSCRIPTIONS/<subscription id>/RESOURCEGROUPS/<resource group name>/PROVIDERS/MICROSOFT.NETWORK/APPLICATIONGATEWAYS/<application gateway name>",
        "operationName": "ApplicationGatewayAccess",
        "time": "2016-04-11T04:24:37Z",
        "category": "ApplicationGatewayAccessLog",
        "properties": {
            "instanceId":"ApplicationGatewayRole_IN_0",
            "clientIP":"37.186.113.170",
            "clientPort":"12345",
            "httpMethod":"HEAD",
            "requestUri":"/xyz/portal",
            "requestQuery":"",
            "userAgent":"-",
            "httpStatus":"200",
            "httpVersion":"HTTP/1.0",
            "receivedBytes":"27",
            "sentBytes":"202",
            "timeTaken":"359",
            "sslEnabled":"off"
        }
    }

## <a name="performance-log"></a>Protokolování výkonu

Tento protokol vytváří pouze, pokud jste ho povolili na jednoho základna brány aplikace podle předchozích kroků. Uložení dat v okně úložiště účet, který jste zadali při povolíte protokolování. Zaznamenané následující údaje:

    {
        "resourceId": "/SUBSCRIPTIONS/<subscription id>/RESOURCEGROUPS/<resource group name>/PROVIDERS/MICROSOFT.NETWORK/APPLICATIONGATEWAYS/<application gateway name>",
        "operationName": "ApplicationGatewayPerformance",
        "time": "2016-04-09T00:00:00Z",
        "category": "ApplicationGatewayPerformanceLog",
        "properties": 
        {
            "instanceId":"ApplicationGatewayRole_IN_1",
            "healthyHostCount":"4",
            "unHealthyHostCount":"0",
            "requestCount":"185",
            "latency":"0",
            "failedRequestCount":"0",
            "throughput":"119427"
        }
    }


## <a name="firewall-log"></a>Protokol brány firewall

Tento protokol vytváří pouze, pokud jste ho povolili na jednoho základna brány aplikace podle předchozích kroků. Tento protokol také vyžaduje konfiguraci brány firewall této webové aplikace na brány aplikace. Uložení dat v okně úložiště účet, který jste zadali při povolíte protokolování. Zaznamenané následující údaje:

    {
        "resourceId": "/SUBSCRIPTIONS/<subscriptionId>/RESOURCEGROUPS/<resourceGroupName>/PROVIDERS/MICROSOFT.NETWORK/APPLICATIONGATEWAYS/<applicationGatewayName>",
        "operationName": "ApplicationGatewayFirewall",
        "time": "2016-09-20T00:40:04.9138513Z",
        "category": "ApplicationGatewayFirewallLog",
        "properties":     {
            "instanceId":"ApplicationGatewayRole_IN_0",
            "clientIp":"108.41.16.164",
            "clientPort":1815,
            "requestUri":"/wavsep/active/RXSS-Detection-Evaluation-POST/",
            "ruleId":"OWASP_973336",
            "message":"XSS Filter - Category 1: Script Tag Vector",
            "action":"Logged",
            "site":"Global",
            "message":"XSS Filter - Category 1: Script Tag Vector",
            "details":{"message":" Warning. Pattern match "(?i)(<script","file":"/owasp_crs/base_rules/modsecurity_crs_41_xss_attacks.conf","line":"14"}}
    }

## <a name="view-and-analyze-the-audit-log"></a>Prohlížení a analýze protokol auditování

Můžete zobrazit a analýza dat protokolu auditování pomocí kterékoli z těchto způsobů:

- **Azure nástroje:** Získat informace z protokolů auditování pomocí prostředí PowerShell Azure, Azure rozhraní příkazového řádku (rozhraní příkazového řádku), rozhraní REST API Azure nebo portálu Azure náhled.  Podrobné pokyny pro každou metodu jsou uvedené v článku [auditování operace s správce prostředků](../resource-group-audit.md) .
- **Power BI:** Pokud ještě nemáte účet [Power BI](https://powerbi.microsoft.com/pricing) , můžete ho zdarma. Použití [protokolů auditování Azure obsah pack pro Power BI](https://powerbi.microsoft.com/en-us/documentation/powerbi-content-pack-azure-audit-logs/) můžete analýza dat pomocí předkonfigurovaná řídicí panely, které můžete použít jako-je nebo upravit.

## <a name="view-and-analyze-the-access-performance-and-firewall-log"></a>Prohlížení a analýze protokol přístup, výkon a brány firewall

Azure [Protokolu analýzy](../log-analytics/log-analytics-azure-networking-analytics.md) získáte čítači v protokolu událostí soubory a z vašeho účtu úložiště objektů Blob obsahuje vizualizace a výkonné možnosti vyhledávání a analyzujte data protokolů.

Také se můžete připojit ke svému účtu úložiště a načítat položky JSON protokolu protokolů přístup a výkonu. Po stažení JSON soubory, které převádět CSV a zobrazení v aplikaci Excel, PowerBI nebo jiné vizualizace nástroje data.

>[AZURE.TIP] Pokud znáte Visual Studia a základní koncepty změny hodnoty konstanty a proměnných v jazyce C#, můžete použít [protokol převaděče nástroje](https://github.com/Azure-Samples/networking-dotnet-log-converter) Github k dispozici.

## <a name="metrics"></a>Metriky

Metriky je funkce pro některé Azure prostředky, kde si můžete pustit výkonnosti na portálu. Aplikace bránu pro jednu míru neexistuje při psaní v tomto článku. Tento míru je výkon a můžete vidět na portálu. Přejděte k bráně aplikace a klikněte na **metrik**.  Výkon vyberte v části **dostupné metriky** zobrazení hodnot. Na následujícím obrázku zobrazí se příkladu s filtry, které lze použít k zobrazení dat v oblasti jiný čas.

Seznam aktuální podporují metriky naleznete [podporovaný metriky s Azure monitoru](../monitoring-and-diagnostics/monitoring-supported-metrics.md)

![metrických zobrazení][5]

## <a name="alert-rules"></a>Upozornění pravidel

Upozornění pravidel lze spustit z podle metriky zdroje. Znamená to, že aplikace bránu pro upozornění volání webhook nebo e-mailové správce, pokud výkon bráně aplikace nad, pod nebo za prahovou hodnotu stanovený počet minut.

Následující příklad vás provede jednotlivými vytváření pravidla výstrahy, která odešle e-mailu pro správce po porušení výkon mezní hodnota.

### <a name="step-1"></a>Krok 1

Klikněte na **Přidat metrických upozornění** na spustit. Tento zásuvné můžete přejít také z zásuvné metriky.

![pravidla výstrah zásuvné][6]

### <a name="step-2"></a>Krok 2

V zásuvné **Přidat pravidlo** vyplňte název podmínky a upozornit oddíly a klikněte na tlačítko **OK** po dokončení.

Volič **Podmínka** umožňuje 4 hodnoty **větší než**, **větší než nebo rovná**, **menší než**nebo **menší nebo rovna hodnotě**.

Volič **období** umožňuje výběru letech 5 minut až 6 hodin.

Výběrem **Vlastníci e-mailu, přispěvatelé a čtenáři** e-mailu může být dynamické podle uživatelů, kteří mají přístup k tomuto zdroji. Hodnoty s oddělovači seznam uživatelů můžete být uvedeno v textovém poli **email(s) další správce** .

![Přidání pravidla zásuvné][7]

Pokud je mezní hodnota je porušena, přijde e-mailu podobnou té na následujícím obrázku:

![mezní hodnota porušena e-mailu][8]

Jakmile metrických upozornění byla vytvořena a nabízí přehled všechna upozornění pravidla je zobrazený seznam upozornění.

![zobrazení pravidlo výstrahy][9]

Další informace o oznámení, navštěvujte blog o [přijímání oznámení](../monitoring-and-diagnostics/insights-receive-alert-notifications.md)

Pokud chcete zjistit další informace o webhooks a jak je můžete používat s upozornění, navštěvujte blog o [Konfigurovat webhook na Azure metrických upozornění](../monitoring-and-diagnostics/insights-webhooks-alerts.md)

## <a name="next-steps"></a>Další kroky

- Vizualizace čítač a protokoly událostí pomocí [Protokolu analýzy](../log-analytics/log-analytics-azure-networking-analytics.md) 
- Příspěvek na blog [vizualizace protokolů auditování Azure pomocí Power BI](http://blogs.msdn.com/b/powerbi/archive/2015/09/30/monitor-azure-audit-logs-with-power-bi.aspx) .
- [Zobrazení a analýza protokolů auditování Azure v Power BI a dělat spoustu dalších](https://azure.microsoft.com/blog/analyze-azure-audit-logs-in-powerbi-more/) příspěvek na blog.

[1]: ./media/application-gateway-diagnostics/figure1.png
[2]: ./media/application-gateway-diagnostics/figure2.png
[3]: ./media/application-gateway-diagnostics/figure3.png
[4]: ./media/application-gateway-diagnostics/figure4.png
[5]: ./media/application-gateway-diagnostics/figure5.png
[6]: ./media/application-gateway-diagnostics/figure6.png
[7]: ./media/application-gateway-diagnostics/figure7.png
[8]: ./media/application-gateway-diagnostics/figure8.png
[9]: ./media/application-gateway-diagnostics/figure9.png