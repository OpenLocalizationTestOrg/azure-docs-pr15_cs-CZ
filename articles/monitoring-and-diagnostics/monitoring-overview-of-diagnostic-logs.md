<properties
    pageTitle="Základní informace o Azure protokoly pro diagnostiku | Microsoft Azure"
    description="Zjistěte, jaké protokoly pro diagnostiku Azure a jak je můžete používat pochopit událostí v Azure zdroje."
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
    ms.date="10/12/2016"
    ms.author="johnkem; magoedte"/>

# <a name="overview-of-azure-diagnostic-logs"></a>Základní informace o Azure protokoly pro diagnostiku
**Protokoly pro diagnostiku Azure** jsou protokolů, které zdroje, které jsou zdrojem dat bohaté a časté o operaci daný zdroj. Obsah tyto protokoly se liší podle typu zdroje (například protokoly událostí systému Windows jednu kategorii diagnostickém protokolu VMs a objektů blob, tabulky a fronty protokoly kategorie protokoly pro diagnostiku pro úložiště účty) a se liší od [protokolu činnosti (dřív jmenoval protokolu auditování nebo provozní protokol)](monitoring-overview-activity-logs.md), poskytující pochopení operace, které byly provedeny zdrojů ve vašem předplatném. Ne všechny prostředky podpory nový typ protokoly pro diagnostiku zde popsané. V seznamu podporovaných službami jsou uvedeny typy zdrojů, které podporují nové diagnostické protokoly.

![Logické umístění protokoly pro diagnostiku](./media/monitoring-overview-of-diagnostic-logs/logical-placement-chart.png)

## <a name="what-you-can-do-with-diagnostic-logs"></a>Co můžete dělat s protokoly pro diagnostiku
Tady jsou některé věci, které můžete dělat s diagnostických protokolů:

- Uložte je do **Účtu úložiště** auditování nebo ruční kontroly. Můžete zadat dobu uchovávání informací (ve dnech), pomocí **Diagnostických nastavení**.
- [Můžete vysílat datovými proudy, aby **Rozbočovače událostí** ](monitoring-stream-diagnostic-logs-to-event-hubs.md) pro požití služby třetích stran nebo vlastní analytics řešení například PowerBI.
- Analýza s [OMS protokolu analýzy](../log-analytics/log-analytics-azure-storage-json.md)

## <a name="diagnostic-settings"></a>Diagnostické nastavení
Protokoly pro diagnostiku pro není výpočetním zdroje se konfigurují diagnostiky nastavení. **Diagnostické nastavení** ovládacího prvku zdroje:

- Protokoly pro diagnostiku místo, kam se odesílají (účtu úložiště, rozbočovače událost nebo OMS protokolu analýzy).
- Jsou odeslány kategorií, které protokolu.
- Jak dlouho kategoriemi protokolu by měl být zachován v účtu úložiště – uchování nula dní znamená, že stále uchovávat protokoly. V opačném tuto hodnotu v rozsahu 1 až 2147483647. Pokud jsou nastavené zásady uchovávání informací, ale je vypnutá, ukládání protokolů v úložišti účtu (například-li vybrán rozbočovače událost nebo OMS možnosti pouze), zásady uchovávání informací mít žádný vliv.

Toto nastavení je snadno nakonfigurováno prostřednictvím zásuvné diagnostiky zdroje Azure portálu prostřednictvím příkazy Powershellu Azure a rozhraní příkazového řádku nebo prostřednictvím [Rozhraní REST API Azure Monitor](https://msdn.microsoft.com/library/azure/dn931943.aspx).

> [AZURE.WARNING] Protokoly pro diagnostiku a metriky výpočetním zdrojů (například VMs nebo struktury služby) použijte [samostatný mechanismus při konfiguraci a výběr výstupy](../azure-diagnostics.md).

## <a name="how-to-enable-collection-of-diagnostic-logs"></a>Jak povolit kolekce protokoly pro diagnostiku
Kolekce protokoly pro diagnostiku může být užitečné v rámci vytvořit zdroj nebo zdroj je vytvořený prostřednictvím zásuvné zdroje na portálu. Můžete taky povolit protokoly pro diagnostiku kdykoli příkazy Powershellu Azure nebo rozhraní příkazového řádku nebo pomocí Azure Monitor REST API.

> [AZURE.TIP] Přímo na každý zdroj nemusí vztahovat tyto pokyny. Zobrazení propojení schématu v dolní části této stránky a vysvětlení zvláštní kroky, které se můžou týkat určité typy zdrojů.

[Tento článek popisuje, jak můžete pomocí šablony zdroje povolit diagnostické nastavení při vytváření zdroje](./monitoring-enable-diagnostic-logs-using-template.md)

### <a name="enable-diagnostic-logs-in-the-portal"></a>Povolení protokoly pro diagnostiku na portálu
Můžete zapnout diagnostické protokoly Azure portálu při vytváření výpočetních typů zdrojů povolením systému Windows nebo Linux Azure diagnostiky e-mailem:

1.  Přejděte na **Nový** a zvolte zdroje, které vás zajímají.
2.  Po konfiguraci základní nastavení a vyberte velikost – v zásuvné **Nastavení** v části **Sledování**, vyberte **Povolit** a zvolte účet úložiště místo, kam chcete ukládat protokoly pro diagnostiku. Normální dat sazeb pro ukládání a transakce bude účtovaná, když budete posílat diagnostiky účtu úložiště.

    ![Povolení protokoly pro diagnostiku při vytváření zdroje](./media/monitoring-overview-of-diagnostic-logs/enable-portal-new.png)
3.  Klikněte na **OK** a vytvořte zdroje.

Není výpočetním zdrojů můžete povolit protokoly pro diagnostiku Azure portálu po zdroj byl vytvořen následujícím způsobem:

1.  Přejděte na zásuvné pro daný zdroj a otevřete zásuvné **diagnostických nástrojů** .
2.  Klikněte **na** a vyberte účet úložiště a/nebo centrální události.

    ![Po vytvoření prostředku povolit protokoly pro diagnostiku](./media/monitoring-overview-of-diagnostic-logs/enable-portal-existing.png)
3.  V části **protokoly**vyberte, které **Protokolu kategorie** chcete shromáždit nebo datového proudu.
4.  Klikněte na **Uložit**.

### <a name="enable-diagnostic-logs-via-powershell"></a>Povolení protokoly pro diagnostiku pomocí prostředí PowerShell
Povolit protokoly pro diagnostiku pomocí rutin prostředí PowerShell Azure, použijte následující příkazy.

Povolit ukládání diagnostických protokolů účet úložiště, použijte tento příkaz:

    Set-AzureRmDiagnosticSetting -ResourceId [your resource id] -StorageAccountId [your storage account id] -Enabled $true

ID účtu úložiště je číslo id zdroje pro úložiště účet, ke kterému chcete poslat protokoly.

Povolit streamování diagnostických protokolů k rozbočovači události, použijte tento příkaz:

    Set-AzureRmDiagnosticSetting -ResourceId [your resource id] -ServiceBusRuleId [your service bus rule id] -Enabled $true

ID služeb Bus pravidla je řetězec s v tomto formátu: `{service bus resource ID}/authorizationrules/{key name}`.

Povolit odesílání protokoly pro diagnostiku do pracovního prostoru protokolu analýzy, použijte tento příkaz:

    Set-AzureRmDiagnosticSetting -ResourceId [your resource id] -WorkspaceId [log analytics workspace id] -Enabled $true

> [AZURE.NOTE] Parametr WorkspaceId není dostupné ve verzi dne. Budou k dispozici ve verzi listopadu.

Můžete získat své ID pracovního prostoru protokolu analýzy na portálu Azure.

Můžete kombinovat tyto parametry povolit více možnosti výstupu.

### <a name="enable-diagnostic-logs-via-cli"></a>Povolení protokoly pro diagnostiku prostřednictvím rozhraní příkazového řádku
Povolit protokoly pro diagnostiku prostřednictvím rozhraní příkazového řádku Azure, použijte následující příkazy:

Povolit ukládání diagnostických protokolů účet úložiště, použijte tento příkaz:

    azure insights diagnostic set --resourceId <resourceId> --storageId <storageAccountId> --enabled true

ID účtu úložiště je číslo id zdroje pro úložiště účet, ke kterému chcete poslat protokoly.

Povolit streamování diagnostických protokolů k rozbočovači události, použijte tento příkaz:

    azure insights diagnostic set --resourceId <resourceId> --serviceBusRuleId <serviceBusRuleId> --enabled true

ID služeb Bus pravidla je řetězec s v tomto formátu: `{service bus resource ID}/authorizationrules/{key name}`.

Povolit odesílání protokoly pro diagnostiku do pracovního prostoru protokolu analýzy, použijte tento příkaz:

    azure insights diagnostic set --resourceId <resourceId> --workspaceId <workspaceId> --enabled true

> [AZURE.NOTE] Parametr workspaceId není dostupné ve verzi dne. Budou k dispozici ve verzi listopadu.

Můžete získat své ID pracovního prostoru protokolu analýzy na portálu Azure.

Můžete kombinovat tyto parametry povolit více možnosti výstupu.

### <a name="enable-diagnostic-logs-via-rest-api"></a>Povolení protokoly pro diagnostiku prostřednictvím rozhraní REST API
Nastavení diagnostiky pomocí rozhraní Azure Monitor REST API, najdete v článku [tohoto dokumentu](https://msdn.microsoft.com/library/azure/dn931931.aspx).

## <a name="manage-diagnostic-settings-in-the-portal"></a>Na portálu Správa diagnostické nastavení

Zajistit, aby všechny zdroje jsou správně nastavená diagnostiky nastavení můžete přejděte na zásuvné **Sledování** na portálu a otevřete zásuvné **Diagnostické protokoly** .

![Diagnostické protokolování zásuvné na portálu](./media/monitoring-overview-of-diagnostic-logs/manage-portal-nav.png)

Možná bude potřeba "Další služby" klikněte na Najít zásuvné sledování.

V tomto zásuvné můžete zobrazit a filtrovat všechny zdroje, které podporují protokoly pro diagnostiku zobrazíte, pokud mají diagnostiky povolené a které účtu úložiště, centrální událost nebo pracovního prostoru protokolu analýzy jsou pokračuje jsou protokoly.

![Výsledky diagnostických protokolů zásuvné portálu](./media/monitoring-overview-of-diagnostic-logs/manage-portal-blade.png)

Po kliknutí na zdroje řádku se zobrazí všechny protokolů, které máte uložené na účtu úložiště a poskytnout možnost vypnout nebo změna nastavení diagnostických. Klikněte na ikonu stahování stažení protokoly pro určitého časového období.

![Diagnostické protokolování zásuvné jeden zdroj](./media/monitoring-overview-of-diagnostic-logs/manage-portal-logs.png)

> [AZURE.NOTE] Protokoly pro diagnostiku pouze v tomto zobrazení a být k dispozici ke stažení, pokud jste nakonfigurovali diagnostiky nastavení je uložíte do účtu úložiště.

Po kliknutí na odkaz pro **Diagnostiku nastavení** zobrazíte zásuvné diagnostiky nastavení, kde můžete povolení, zakázání nebo změna nastavení diagnostických pro vybraný zdroj.

## <a name="supported-services-and-schema-for-diagnostic-logs"></a>Podporované služby a schématu pro protokoly pro diagnostiku
Schéma protokoly pro diagnostiku liší se podle kategorie zdrojů a protokolu. Tady jsou podporované služeb a jejich schématu.

| Služba                       | Schéma a dokumenty                                                                                                   |
|-------------------------------|-----------------------------------------------------------------------------------------------------------------|
|    Vyrovnávání zatížení software     |    [Protokol analýz pro vyrovnávání zatížení Azure (verze Preview)](../load-balancer/load-balancer-monitor-log.md)             |
|    Skupiny zabezpečení sítě    |    [Protokol analýzy sítě skupin zabezpečení (NSGs)](../virtual-network/virtual-network-nsg-manage-log.md)     |
|    Aplikace bran       |    [Protokolování diagnostiky aplikace brány](../application-gateway/application-gateway-diagnostics.md)     |
|    Klíčové trezoru                  |    [Azure klíčové trezoru protokolování](../key-vault/key-vault-logging.md)                                                 |
|    Azure hledání               |    [Povolení a používání technologie pro analýzu přenosy hledání](../search/search-traffic-analytics.md)                         |
|    Úložiště jezera dat            |    [Přístup k protokoly pro diagnostiku pro úložiště jezera dat Azure](../data-lake-store/data-lake-store-diagnostic-logs.md) |
|    Jezera analýzy dat        |    [Přístup k protokoly pro diagnostiku pro analýzu dat jezera Azure](../data-lake-analytics/data-lake-analytics-diagnostic-logs.md) |
|    Použití logických operátorů aplikace                 |    K dispozici žádné schéma.                                                                                         |
|    Azure dávku                |    [Azure dávku diagnostické protokolování](../batch/batch-diagnostics.md)                                              |
|    Automatizace Azure           |    [Protokol analýz pro automatizaci Azure](../automation/automation-manage-send-joblogs-log-analytics.md)          |
|    Centrální události                  |    K dispozici žádné schéma.                                                                                         |
|    Služba Bus                |    K dispozici žádné schéma.                                                                                         |
|    Technologie pro analýzu toku           |    K dispozici žádné schéma.                                                                                         |

## <a name="supported-log-categories-per-resource-type"></a>Podporované protokolu kategorií podle typu zdroje

|Pole Typ zdroje|Kategorie|Kategorie zobrazované jméno|
|---|---|---|
|Microsoft.Automation/automationAccounts|JobLogs|Protokoly projektu|
|Microsoft.Automation/automationAccounts|JobStreams|Toky projektu|
|Microsoft.Batch/batchAccounts|ServiceLog|Protokoly služby|
|Microsoft.DataLakeAnalytics/accounts|Auditování|Protokolů auditování|
|Microsoft.DataLakeAnalytics/accounts|Požadavky|Žádost o protokoly|
|Microsoft.DataLakeStore/accounts|Auditování|Protokolů auditování|
|Microsoft.DataLakeStore/accounts|Požadavky|Žádost o protokoly|
|Microsoft.EventHub/namespaces|ArchiveLogs|Protokoly archivu|
|Microsoft.EventHub/namespaces|OperationalLogs|Provozní protokoly|
|Microsoft.KeyVault/vaults|AuditEvent|Protokolů auditování|
|Microsoft.Logic/workflows|WorkflowRuntime|Pracovní postup runtime diagnostických událostí|
|Microsoft.Network/networksecuritygroups|NetworkSecurityGroupEvent|Skupina zabezpečení sítě události|
|Microsoft.Network/networksecuritygroups|NetworkSecurityGroupRuleCounter|Skupina zabezpečení sítě pravidlo čítač|
|Microsoft.Network/networksecuritygroups|NetworkSecurityGroupFlowEvent|Skupina zabezpečení sítě pravidla toku události|
|Microsoft.Network/loadBalancers|LoadBalancerAlertEvent|Oznámení událostí Vyrovnávání zatížení|
|Microsoft.Network/loadBalancers|LoadBalancerProbeHealthStatus|Stav stavu zkušební Vyrovnávání zatížení|
|Microsoft.Network/applicationGateways|ApplicationGatewayAccessLog|Aplikace Access brány|
|Microsoft.Network/applicationGateways|ApplicationGatewayPerformanceLog|Protokol výkonu aplikace brány|
|Microsoft.Network/applicationGateways|ApplicationGatewayFirewallLog|Protokol aplikace brány Firewall|
|Microsoft.Search/searchServices|OperationLogs|Operace protokoly|
|Microsoft.ServerManagement/nodes|RequestLogs|Žádost o protokoly|
|Microsoft.ServiceBus/namespaces|OperationalLogs|Provozní protokoly|
|Microsoft.StreamAnalytics/streamingjobs|Spuštění|Spuštění|
|Microsoft.StreamAnalytics/streamingjobs|Vytváření|Vytváření|

## <a name="next-steps"></a>Další kroky
- [Protokoly pro diagnostiku toku k **rozbočovače události**](monitoring-stream-diagnostic-logs-to-event-hubs.md)
- [Diagnostické nastavení změnit pomocí rozhraní Azure Monitor REST API](https://msdn.microsoft.com/library/azure/dn931931.aspx)
- [Analýza protokoly s OMS protokolu analýzy](../log-analytics/log-analytics-azure-storage-json.md)
