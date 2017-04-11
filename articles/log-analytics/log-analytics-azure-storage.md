<properties
    pageTitle="Shromažďování dat Azure úložiště v protokolu analýzy přehled | Microsoft Azure"
    description="Azure zdroje můžete psát protokoly a metriky s klientem Azure úložiště často pomocí diagnostických nástrojů Azure. Technologie pro analýzu protokolu můžete indexovat tato data a usnadnit vyhledávání."
    services="log-analytics"
    documentationCenter=""
    authors="bandersmsft"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="log-analytics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/10/2016"
    ms.author="banders"/>

# <a name="collecting-azure-storage-data-in-log-analytics-overview"></a>Shromažďování dat Azure úložiště v protokolu analýzy přehledu

Mnoho Azure zdroje jsou možnost zápisu protokolů a metriky účet Azure úložiště. Technologie pro analýzu protokolu můžete používat tyto údaje a snadněji sledovat Azure prostředky.

Při psaní k základnímu úložišti Azure zdroj může použít Azure diagnostických nástrojů, nebo vlastní způsob, jak zápis dat. Tato data může napsaný v různých formátech na jednu z následujících umístění:

+ Tabulek Azure
+ Objektů blob Azure
+ EventHub

Protokol analýzy podporuje Azure služby, které zápisu dat pomocí [Azure diagnostické protokoly](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md). Kromě toho protokolu analýzy podporuje dalších službách, které výstup protokoly a metriky v různých formátech a umístění.  

>[AZURE.NOTE] Normální Azure dat sazby pro ukládání a transakce při odeslání diagnostiky účet úložiště a při protokolu analýzy načte data ze svého účtu úložiště bude účtovaná.

![Diagram Azure úložiště](media/log-analytics-azure-storage/azure-storage-diagram.png)

## <a name="supported-azure-resources"></a>Podporované Azure zdroje

Protokol analýzy můžete shromažďovat data o pro následující Azure prostředky:

| Pole Typ zdroje | Protokoly (diagnostiky kategorií) | Řešení analýzy protokolu |
| --------------------------------------- | -------------------------------- | --------------- |
| Přehledy aplikace | Dostupnost <br> Vlastní události <br> Výjimky <br> Požadavky <br> | Přehledy aplikace (verze Preview) |
| Rozhraní API správy | | *žádná* (Verze preview) |
| Automatizace <br> Microsoft.Automation/AutomationAccounts | JobLogs <br> JobStreams          | AzureAutomation (verze Preview) |
| Klíčové trezoru <br> Microsoft.KeyVault/Vaults               | AuditEvent                       | KeyVault (verze Preview) |
| Aplikace brány <br> Microsoft.Network/ApplicationGateways   | ApplicationGatewayAccessLog <br> ApplicationGatewayPerformanceLog | AzureNetworking (verze Preview) |
| Skupina zabezpečení sítě <br> Microsoft.Network/NetworkSecurityGroups | NetworkSecurityGroupEvent <br> NetworkSecurityGroupRuleCounter | AzureNetworking (verze Preview) |
| Služba struktury                          | ETWEvent <br> Funkční události <br> Spolehlivé Actor události <br> Spolehlivé služby události| ServiceFabric (verze Preview) |
| Virtuálních počítačích | Linux Syslog <br> Událostí systému Windows <br> Protokol Internetové informační služby <br> ETWEvent systému Windows | *žádná* |
| Role web <br> Pracovní role | Linux Syslog <br> Událostí systému Windows <br> Protokol Internetové informační služby <br> ETWEvent systému Windows | *žádná* |

>[AZURE.NOTE] Monitorování Azure virtuálních počítačích (Linux a Windows), doporučujeme nainstalovat [OM protokolu analýzy příponu](log-analytics-azure-vm-extension.md). Agent obsahuje důkladnější přehledy na virtuálních počítačích než když používáte diagnostiky napsali k základnímu úložišti.

Můžete Pomozte nám určit jejich prioritu další protokoly pro OMS a analyzujte data podle hlasování na naše [stránka pro zadání názoru](http://feedback.azure.com/forums/267889-azure-log-analytics/category/88086-log-management-and-log-collection-policy).


- Další informace o jak analýzy protokolu protokolů přečíst z Azure služeb, které podporují [protokoly pro diagnostiku Azure](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md)najdete v článku [Analýza Azure pomocí protokolu analýzy diagnostických protokolů](log-analytics-azure-storage-json.md) :
  - Azure klíčové trezoru
  - Automatizace Azure
  - Aplikace brány
  - Skupiny zabezpečení sítě
- Přečtěte si téma [Použití úložiště objektů blob pro službu IIS a úložiště tabulek pro události](log-analytics-azure-storage-iis-table.md) zobrazíte další informace o způsobu protokolu analýzy může číst protokoly pro tuto zápisu diagnostiky úložiště tabulek nebo došlo k úložišti objektů blob zápisu protokolů IIS služby Azure včetně:
  - Služba struktury
  - Role web
  - Pracovní role
  - Virtuálních počítačích


Aplikace přehledy je v soukromé náhled a jeho použití nepřetržitý exportovat do úložiště objektů blob. Připojit se ke soukromé náhled, kontaktujte tým Account Microsoft nebo odkazuje podrobnosti o [svůj názor webu](https://feedback.azure.com/forums/267889-log-analytics/suggestions/6519248-integration-with-app-insights).

## <a name="next-steps"></a>Další kroky

- [Protokoly pro diagnostiku analyzovat Azure pomocí protokolu analýzy](log-analytics-azure-storage-json.md) číst protokoly z Azure služeb, které zápisu diagnostiky k úložišti objektů blob ve formátu JSON.
- [Úložiště objektů blob použití služby IIS a úložiště tabulek pro události](log-analytics-azure-storage-iis-table.md) čtení protokoly pro Azure služeb této zápisu diagnostiky úložiště tabulek nebo došlo k úložišti objektů blob zápisu protokolů IIS.
- [Povolení řešení](log-analytics-add-solutions.md) poskytují přehled o data.
- [Použití vyhledávací dotazy](log-analytics-log-searches.md) k analýze dat.
