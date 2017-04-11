<properties
    pageTitle="Azure přečtěte následující dokumentaci pro státní správu | Microsoft Azure"
    description="To poskytuje porovnání funkcí a pokyny pro na vývoj aplikací pro státní správu Azure."
    services="Azure-Government"
    cloud="gov"
    documentationCenter=""
    authors="ryansoc"
    manager="zakramer"
    editor=""/>

<tags
    ms.service="multiple"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="azure-government"
    ms.date="10/25/2016"
    ms.author="ryansoc"/>


#  <a name="azure-government-monitoring-and-management"></a>Azure Government monitorování a správu

Tento článek pojednává o sledování a správa služeb varianty a aspektech pro státní správu Azure prostředí.

## <a name="automation"></a>Automatizace

Automatizace je všeobecně dostupná v Azure Government.

### <a name="variations"></a>Varianty

Nejsou ve Azure Government momentálně dostupné tyto funkce automatizaci.

+ Vytvoření zásady služby přihlašovací údaje pro ověřování

Další informace najdete v tématu [automatizaci veřejné přečtěte následující dokumentaci pro](../automation/automation-intro.md).

## <a name="log-analytics"></a>Protokol analýzy

V Azure Government obecně neexistuje protokolu analýzy.

### <a name="variations"></a>Varianty

Následující funkce Log analýzy a řešení nejsou aktuálně dostupné v Azure pro státní správu.

+ Řešení, která jsou v náhledu na Microsoft Azure včetně:
  - Řešení sledování sítě
  - Závislost typu sledování řešení aplikace
  - Řešení Office 365
  - Windows 10 Upgrade analýzy řešení
  - Řešení přehledy aplikace
  - Azure řešení sítě analýzy
  - Azure řešení automatizaci analýzy
  - Klíč trezoru analýzy řešení
+ Řešení a funkce, které vyžadují aktualizace místní software, včetně:
  - Integrace se službou systému Centrum operace správce 2016 (podporují předchozích verzí aplikace Operations Manager)
  - Skupiny počítače z centra Správce konfigurace pro System
  - Vystavení centrální řešení
+ Funkce, které nejsou v náhledu na veřejné Azure, včetně:
  - Export dat Power BI
+ Azure metriky a diagnostice Azure
+ Operace sadu správy mobilních aplikací

Adresy URL pro analýzy protokolu se liší v Azure Government:

| Azure veřejné | Azure Government | Poznámky |
|--------------|------------------|-------|
| mms.microsoft.com | OMS.microsoft.us | Portál analýzy protokolu |
| *workspaceId*. ods.opinsights.azure.com | *workspaceId*. ods.opinsights.azure.us | [Shromažďování dat rozhraní API](../log-analytics/log-analytics-data-collector-api.md) 
| \*. ods.opinsights.azure.com | \*. ods.opinsights.azure.us | Agent komunikace – [Konfigurace nastavení brány firewall](../log-analytics/log-analytics-proxy-firewall.md) |
| \*. oms.opinsights.azure.com | \*. oms.opinsights.azure.us | Agent komunikace – [Konfigurace nastavení brány firewall](../log-analytics/log-analytics-proxy-firewall.md) |
| \*. blob.core.windows.net | \*. blob.core.usgovcloudapi.net | Agent komunikace – [Konfigurace nastavení brány firewall](../log-analytics/log-analytics-proxy-firewall.md) |


Následující protokolu analýzy funkce s odlišným chováním v Azure Government:

+ Agent systému Windows musí stáhli z [portálu protokolu analýzy](https://oms.microsoft.us) pro státní správu Azure.
+ Serveru System Center Operations Manager Správa připojení k protokolu analýzy, budete muset stáhnout a import aktualizované management Pack.
  1. Stáhněte si a uložte [aktualizace management Pack](http://go.microsoft.com/fwlink/?LinkId=828749).
  2. Rozbalení souboru, který jste stáhli.
  3. Importujte sady správy do Operations Manager. Další informace o tom, jak importovat management pack z disku, najdete v tématu [Import Operations Manager Management Pack](http://technet.microsoft.com/library/hh212691.aspx) na webu Microsoft TechNet.
  4. Připojení Operations Manager k protokolu analýzy, postupujte podle [Připojení Operations Manager do protokolu analýzy](../log-analytics/log-analytics-om-agents.md).


## <a name="frequently-asked-questions"></a>Nejčastější dotazy

+ Můžu migraci dat z protokolu analýzy v Microsoft Azure pro státní správu Azure?
  - Ne. Není možné přesunout data nebo pracovního prostoru z Microsoft Azure Azure Government.
+ Můžu přejít mezi Microsoft Azure a Azure Government pracovní prostory z portálu operace správy sadu protokolu analýzy?
  - Ne. Portálech pro Microsoft Azure a Azure Government jsou oddělené a Nesdílet informace.

Další informace najdete dokumentaci [protokolu analýzy veřejné](../log-analytics/log-analytics-overview.md).

## <a name="next-steps"></a>Další kroky

Doplňující informace a aktualizace se přihlásit k odběru <a href="https://blogs.msdn.microsoft.com/azuregov/">Microsoft Azure Government blogu.</a>
