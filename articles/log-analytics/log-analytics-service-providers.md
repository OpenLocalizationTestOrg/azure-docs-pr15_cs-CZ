<properties
    pageTitle="Odhlášení funkcí analýzy poskytovatele služeb | Microsoft Azure"
    description="Protokol technologie pro analýzu a můžou pomoct spravovaných poskytovatele služeb (MSPs) rozsáhlých společnostech (nezávisle na softwaru ISV) poskytovatelů hostingu služby Správa a sledování serverů v zákazníka místní nebo cloudové infrastruktury."
    services="log-analytics"
    documentationCenter=""
    authors="richrundmsft"
    manager="jochan"
    editor=""/>

<tags
    ms.service="log-analytics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/25/2016"
    ms.author="richrund"/>

# <a name="log-analytics-features-for-service-providers"></a>Funkce log technologie pro analýzu pro poskytovatele služeb

Protokol analýzy dávají spravovaných poskytovatelé (MSPs), rozsáhlých společnostech, nezávislí dodavatelé softwaru (tvůrce programů) a poskytovatelů hostingu služby Správa a sledování serverů v zákazníka místní nebo cloudové infrastruktury. 

Velké podniky nasdílet mnoho podobnosti poskytovatele služeb, zejména v případě centralizované týmu IT, který je zodpovědný za správu IT pro mnoho různých organizačních jednotek. Pro zjednodušení tento dokument používá termínů *poskytovatele* ale stejnou funkci je také k dispozici pro podniky a ostatních zákazníků.

## <a name="cloud-solution-provider"></a>Cloudové řešení poskytovatele

Pro partnery a poskytovatelé, kteří jsou součástí aplikace [Cloudové řešení poskytovatele (CSP)](https://partner.microsoft.com/Solutions/cloud-reseller-overview) protokol analýzy je jedním z Azure službách CSP předplatného. 

Pro analýzu protokolu následující možnosti jsou povoleny v předplatných *Poskytovatele cloudové řešení* .

Jako *Poskytovatele řešení cloudu* můžete:

+ Vytváření analýz protokolu pracovních prostorů v klientovi (zákazník) předplatné.
+ Pracovní prostory aplikace Access vytvořil klienti. 
+ Přidání a odebrání přístupu uživatelů do pracovního prostoru Správa Azure uživatele. Pokud v pracovním prostoru ke klientovi na portálu OMS stránku Správa uživatelů v části nastavení není k dispozici
  - Protokol analýzy nepodporuje přístupu na základě rolí a zároveň - pojmenování uživatele `reader` oprávnění v portálu Azure umožňuje provádět změny konfigurace OMS portálu

K přihlášení k odběru ke klientovi, budete muset zadat identifikátor klienta. Identifikátor klienta je často poslední část e-mailová adresa použitá k přihlášení.

+ Na portálu OMS přidat `?tenant=contoso.com` adresu URL portálu. Například`mms.microsoft.com/?tenant=contoso.com`
+ V prostředí PowerShell, použít `-Tenant contoso.com` parametr při použití `Add-AzureRmAccount` rutiny
+ Identifikátor klienta se automaticky přidají i při použití `OMS portal` odkaz z portálu Microsoft Azure otevřít a přihlaste se k portálu OMS vybraném pracovním prostoru

Jako *zákazníka* poskytovatele řešení cloudu můžete provést tyto akce:

+ Vytváření protokolu analýzy pracovních prostorů v CSP předplatného
+ Pracovní prostory aplikace Access vytvořil Zprostředkovatel
  -  Použití `OMS portal` odkaz z portálu Microsoft Azure otevřít a přihlaste se k portálu OMS vybraném pracovním prostoru
+ Zobrazení a na stránce Správa uživatelů v části nastavení na portálu OMS

>[AZURE.NOTE] Zálohování a obnovení webu řešení pro analýzy protokolu nejsou možné se připojit k obnovení služby trezoru a v CSP předplatného se nedají konfigurovat.

## <a name="managing-multiple-customers-using-log-analytics"></a>Správa více zákazníkům, kteří používají protokol analýzy 

Je vhodné vytvořit pracovní prostor protokolu Analytics pro jednotlivé zákazníky, které spravujete. Pracovní prostor protokolu analýzy obsahuje:

+ Zeměpisné polohy data, která chcete uložit. 
+ Rozlišení k fakturaci 
+ Izolace dat 
+ Jedinečný konfigurace

Vytvořením pracovního prostoru pro zákazníka, budou moct oddělit jednotlivé zákazníky dat a také sledovat použití jednotlivé zákazníky.

Další informace o kdy a proč vytvářet několika pracovní prostory je popsán v [Správa přístupu k přihlášení analýzy] (log-analytics-manage-access.md#determine-the-number-of-workspaces-you-need).

Vytváření a konfigurace pracovních prostorů zákazníka můžete automatizovat pomocí [prostředí PowerShell](log-analytics-powershell-workspace-configuration.md) [Správce prostředků šablon](log-analytics-template-workspace-configuration.md), nebo pomocí [Rozhraní REST API](https://www.nuget.org/packages/Microsoft.Azure.Management.OperationalInsights/).

Použití šablon správce prostředků pro pracovní prostor konfigurace umožňuje mít hlavní konfiguraci, která slouží k vytváření a konfigurace pracovní prostory. Uživatelé budou mít jistotu, že při vytvoření pracovních prostorů pro zákazníky, budou se konfigurují na k vašim požadavkům. Při aktualizaci vašim požadavkům na šabloně je aktualizován a potom ho znovu použít existujících pracovních prostorů. Tento proces zajišťuje i existujících pracovních prostorů zahájit nový standardy.    

Pokud spravujete víc pracovní prostory protokolu analýzy, doporučujeme, abyste integrace jednotlivých pracovních prostorech s existující ticketing systému / operace konzoly pomocí funkce [výstrahy](log-analytics-alerts.md) . Integrací se stávajícími systémy můžete dál postupujte podle jejich známé procesy pracovníky podpory. Technologie pro analýzu protokolu pravidelně kontroluje jednotlivých pracovních prostorech týkající se upozornění se zadaným kritériím a vygeneruje upozornění, když je potřeba akce.

Pro vedení úrovně zprávu, která souhrny dat v pracovních prostorech můžete použít integrace mezi protokolu technologie pro analýzu a [PowerBI](log-analytics-powerbi.md). Pokud potřebujete integrace s jiného podřízenosti systému, můžete ke spouštění dotazů a export výsledků hledání rozhraní API pro hledání (pomocí prostředí PowerShell nebo [REST](log-analytics-log-search-api.md)).

## <a name="next-steps"></a>Další kroky

+ Automatické vytváření a konfigurace pracovních prostorů pomocí [šablony správce zdrojů](log-analytics-template-workspace-configuration.md)
+ Automatizovat vytváření poštovních pracovních prostorů pomocí [prostředí PowerShell](log-analytics-powershell-workspace-configuration.md) 
+ Použití [upozornění](log-analytics-alerts.md) ke integrovat s existujícími systémy
+ Souhrnné sestavy pomocí [PowerBI](log-analytics-powerbi.md)
