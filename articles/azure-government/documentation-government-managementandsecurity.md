<properties
    pageTitle="Azure přečtěte následující dokumentaci pro státní správu | Microsoft Azure"
    description="To poskytuje srovnání funkcí a pokyny pro na vývoj aplikací pro státní správu Azure"
    services="Azure-Government"
    cloud="gov" 
    documentationCenter=""
    authors="scooxl"
    manager="zakramer"
    editor=""/>
<tags
    ms.service="multiple"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="azure-government"
    ms.date="10/25/2016"
    ms.author="scooxl"/>
#  <a name="azure-government-management-and-security"></a>Správa Azure Government a zabezpečení

## <a name="automation"></a>Automatizace

Automatizace je všeobecně dostupná v Azure Government.

### <a name="variations"></a>Varianty

Nejsou ve Azure Government momentálně dostupné tyto funkce automatizaci.

+ Vytvoření zásady služby přihlašovací údaje pro ověřování

Další informace najdete v tématu [automatizaci veřejné přečtěte následující dokumentaci pro](../automation/automation-intro.md).


##  <a name="key-vault"></a>Klíčové trezoru
Informace o této službě a jak se používá, najdete v článku <a href="https://azure.microsoft.com/documentation/services/key-vault">veřejné dokumentace Azure klíč trezoru.</a>
### <a name="data-considerations"></a>Důležité údaje související data
Tyto informace identifikuje hranici Azure vláda pro trezoru Azure klíč:

| Upraveno/řízená data povolené. | Upraveno/řízená data není povoleno |
|--------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Všechna data zašifrovaných klíčem Azure klíč trezoru může obsahovat Regulated/řízená data. | Azure klíč trezoru metadat není povoleno má být řízené exportovat data. Tato metadata zahrnuje všechna data konfigurace zadané při vytváření a udržování trezoru klíče.  Nezadáte Regulated/řízená data do následujících polí: pole názvy zdrojů skupina, klíč trezoru názvy, názvu předplatného. |

V Azure Government obecně neexistuje klíč trezoru. Jako veřejnosti je bez přípony tak, aby klíč trezoru dostupné prostřednictvím Powershellu a rozhraní příkazového řádku pouze.
## <a name="log-analytics"></a>Protokol analýzy
V Azure Government obecně neexistuje protokolu analýzy. 

### <a name="variations"></a>Varianty

Následující funkce Log analýzy a řešení nejsou aktuálně dostupné v Azure pro státní správu. Tento seznam se aktualizuje při stavu funkce / změní řešení.

+ Řešení, která jsou v náhledu na veřejné Azure, včetně:
  - Řešení sledování sítě
  - Sledování aplikací závislostí
  - Řešení Office 365
  - Windows 10 Upgrade analýzy řešení
  - Přehledy aplikace
  - Azure řešení sítě analýzy
  - Azure automatizaci analýzy
  - Analýzu klíčových trezoru
+ Řešení a funkce, které vyžadují aktualizace místní software, včetně
  - Integrace se službou systému Centrum operace správce 2016 (podporují předchozích verzí aplikace Operations Manager)
  - Skupiny počítačů ze Správce konfigurace System Center
  - Vystavení centrální řešení
+ Funkce, které nejsou v náhledu na veřejné Azure, včetně
  - Export dat PowerBI
+ Azure metriky a diagnostice Azure
+ OMS mobilní aplikace

Adresy URL pro analýzy protokolu se liší v Azure Government:

| Azure veřejné | Azure Government | Poznámky |
|--------------|------------------|-------|
| mms.microsoft.com | OMS.microsoft.us | Portál analýzy protokolu |
| *workspaceId*. ods.opinsights.azure.com | *workspaceId*. ods.opinsights.azure.us | [Shromažďování dat rozhraní API](../log-analytics/log-analytics-data-collector-api.md) 
| \*. ods.opinsights.azure.com | \*. ods.opinsights.azure.us | Agent komunikace – [Konfigurace nastavení brány firewall](../log-analytics/log-analytics-proxy-firewall.md) |
| \*. oms.opinsights.azure.com | \*. oms.opinsights.azure.us | Agent sdělení – [Konfigurace nastavení brány firewall](../log-analytics/log-analytics-proxy-firewall.md) |
| \*. blob.core.windows.net | \*. blob.core.usgovcloudapi.net | Agent sdělení – [Konfigurace nastavení brány firewall](../log-analytics/log-analytics-proxy-firewall.md) |


Následující funkce Log analýzy chovat různě v Azure Government:

+ Agent systému Windows musí stáhli z [portálu protokolu analýzy](https://oms.microsoft.us) pro státní správu Azure.
+ Serveru System Center Operations Manager Správa připojení k protokolu analýzy, budete muset stáhnout a import aktualizované Management Pack.
  1. Stáhněte si a uložte [aktualizace management Pack](http://go.microsoft.com/fwlink/?LinkId=828749)
  2. Rozbalení soubor, který jste stáhli
  3. Importujte sady správy do Operations Manager. Další informace o tom, jak importovat management pack z disku, naleznete v tématu [jak importovat Operations Manager Management Pack](http://technet.microsoft.com/library/hh212691.aspx) na webu Microsoft TechNet.
  4. Připojení Operations Manager k protokolu analýzy, postupujte podle [Připojení Operations Manager do protokolu analýzy](../log-analytics/log-analytics-om-agents.md) 



### <a name="frequently-asked-questions"></a>Nejčastější dotazy

+ Můžete migrovat data z protokolu analýzy ve veřejných Government Azure k Azure?
  - Ne. Není možné přesunout data nebo pracovního prostoru z veřejné Azure k Azure pro státní správu.
+ Lze přepínat veřejné Azure a Azure Government pracovní prostory z portálu OMS protokolu analýzy?
  - Ne. Portály pro veřejné Azure a Azure Government jsou oddělené a Nesdílet informace. 

Další informace najdete dokumentaci [protokolu analýzy veřejné](../log-analytics/log-analytics-overview.md).

## <a name="next-steps"></a>Další kroky

Doplňující informace a aktualizace se přihlásit k odběru <a href="https://blogs.msdn.microsoft.com/azuregov/">Microsoft Azure Government blogu.</a>
 
