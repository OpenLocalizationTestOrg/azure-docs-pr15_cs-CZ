<properties
   pageTitle="Integrace s operace správy sady (OMS) | Microsoft Azure"
   description="Kromě použití standardních funkcí OMS, ho můžete integrovat s jinými aplikacemi, Správa a službám mohl vám poskytovat hybridního prostředí management, zadat vlastní správy scénáře jedinečné pro prostředí nebo vlastní managementu možnosti pro zákazníky.  Tento článek obsahuje přehled různých možností pro integraci s OMS a odkazy na články poskytující podrobné technické informace."
   services="operations-management-suite"
   documentationCenter=""
   authors="bwren"
   manager="jwhit"
   editor="tysonn" />
<tags
   ms.service="operations-management-suite"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="09/23/2016"
   ms.author="bwren" />

# <a name="integrating-with-operations-management-suite-oms"></a>Integrace s operace správy sady (OMS)

Operace správy sady je společnosti Microsoft cloudové IT řešení pro správu, která vám pomáhá spravovat a chránit vaše místních a cloudových infrastruktury.  Kromě použití standardních funkcí OMS, ho můžete integrovat s jinými aplikacemi, Správa a službám mohl vám poskytovat hybridního prostředí management, zadat vlastní správy scénáře jedinečné pro prostředí nebo vlastní managementu možnosti pro zákazníky.  Tento článek obsahuje přehled různých možností pro integraci se službami OMS a odkazy na články poskytující podrobné technické informace. 



## <a name="log-analytics"></a>Protokol analýzy
Správa data shromážděná protokolu analýzy uložený v úložišti, který je hostovaný v Azure.  V protokolu hledání, které poskytují rychlá analýza přes velmi velké objemy dat neexistuje všech dat uložených v úložišti.  Integrace požadavky může být k naplnění úložiště novými daty zpřístupnění analýza nebo extrahovat data v úložišti poskytovat novou vizualizaci nebo integrace s jiný nástroj pro správu.

Všechny dat v úložišti uloženy jako záznam.  Po naplnění úložiště se má uživatelům poskytnout typ záznamu, který používá řešení a popis vlastností.  Když data načtete, musíte tyto informace o datech, se kterou pracujete s.

![Vyplnění OMS úložiště](media/operations-management-suite-integration/repository.png)


### <a name="populate-the-log-analytics-repository"></a>Vyplnění úložišti protokolu analýzy
Vyplnění úložišti OMS několika způsoby.  Metoda použitém závisí na faktory například kde jsou uložená data zdroje, formát data a které klientů potřebujete podporu.  Jakmile se data uložena v úložišti, nezáleží na tom jak byly shromážděny.

Následující oddíly popisují různých možností pro vyplnění OMS úložiště.

#### <a name="connected-sources-and-data-sources"></a>Připojení zdroje a zdrojů dat 
Připojení zdroje jsou umístění, kde můžete data načtená pro OMS úložiště.  Zdroje dat a řešení spustit připojení zdroje a definovat data, která se shromažďují.  Pokud vaše aplikace zapisuje dat na jeden z těchto zdrojů dat, můžete ho shromažďovat nakonfigurováním zdroje dat.  Například pokud aplikace vytvoří Syslog události, pak se můžete shromažďovat zdrojem dat Syslog na agenta Linux.

- [Zdroje dat v protokolu analýzy](../log-analytics/log-analytics-data-sources.md)

#### <a name="solutions"></a>Řešení

Řešení rozšiřují OMS.  Řešení může shromáždit data od zdroji připojení nebo ho může analýza záznamů už shromažďované v úložišti.  Každé řešení poskytované společností Microsoft obsahuje jednotlivé příspěvku, který obsahuje data, která shromažďuje podrobnosti.

- [Řešení v protokolu analýzy](../log-analytics/log-analytics-add-solutions.md)



#### <a name="http-data-collector-api"></a>Shromažďování dat HTTP rozhraní API

Rozhraní API dat protokolu analýzy HTTP kolekcí je rozhraní REST API, která umožňuje přidat JSON dat do úložiště protokolu analýzy.  Toto rozhraní API můžete využít, když budete mít aplikaci, která se zdrojem dat pomocí jedné z jiných zdrojů dat a řešení.  Slouží k naplnění úložiště klienta, který můžete volat rozhraní API a ne závisí na plán kolekce zdroje dat nebo řešení.

- [Protokol analýzy HTTP kolekcí rozhraní API](../log-analytics/log-analytics-data-collector-api.md)


### <a name="retrieve-data-from-the-log-analytics-repository"></a>Načtení dat z úložiště protokolu analýzy

Načtení dat z úložiště OMS několika způsoby.  Mohou uživatelé k načtení dat pomocí konzole OMS a dejte jí různé druhy vizualizace a analýzy.  Můžete taky načtení dat z externí proces například jiné řešení pro správu.

#### <a name="log-searches"></a>Hledání protokolu

Všech dat uložených v úložišti OMS je k dispozici prostřednictvím protokolu vyhledávání.  Uživatelům může provést vlastní ad hoc analýzu v konzole OMS nebo vytvoření řídicího panelu s vizualizace pro konkrétní protokolu vyhledávání.  Řešení může obsahovat vlastní zobrazení s vizualizacemi podle předdefinované vyhledávání.  Rozhraní API pro hledání protokolu můžete použít pro přístup k datům v úložišti OMS u externím nástroji aplikace nebo Správa.  

- [Protokol vyhledávání v protokolu analýzy](../log-analytics/log-analytics-log-searches.md)
- [Protokol analýzy protokolu hledání rozhraní REST API](../log-analytics/log-analytics-log-search-api.md)
- [Rutiny pro analýzy protokolu](https://msdn.microsoft.com/library/mt188224.aspx)



#### <a name="custom-views"></a>Vlastní zobrazení 
Zobrazení návrhu umožňuje vytvářet vlastní zobrazení v konzole OMS, která uživatelům poskytnout vizualizace a analýzy dat ve vašem řešení.  Každé zobrazení obsahuje, se zobrazí na hlavní stránce konzoly a jiné číslo částí vizualizace, které jsou založeny na protokolu hledání, které definujete dlaždici.
  
- [Návrhář zobrazení technologie pro analýzu protokolu](../log-analytics/log-analytics-view-designer.md)


#### <a name="power-bi"></a>Power BI

Technologie pro analýzu protokolu můžete automaticky exportovat data z úložiště OMS do Power BI, můžete využít jeho vizualizace a nástrojů pro analýzu.  Slouží k provedení export podle plánu, bude k dispozici aktuální data. 

- [Export protokolu analýzy dat k Power BI](../log-analytics/log-analytics-powerbi.md)




## <a name="automation"></a>Automatizace

OMS automatizovat procesy reagovat na údaje nebo provádět další funkce správy.  Může shromáždit data od aplikace a vložit ho do úložiště OMS nebo může automatické opravy známých problémech na základě dat v úložišti. 

![Automatizace](media/operations-management-suite-integration/automate.png)

### <a name="runbooks"></a>Runbooks

Runbooks v Azure automatické spuštění skriptů Powershellu a pracovní postupy v Azure cloudu.  Je můžete používat ke správě zdrojů v Azure nebo jiných zdrojů, které jsou přístupné z cloudu.  Runbooks můžete taky spustit v místním datacentra pomocí pracovního postupu Runbook hybridní.  Postupu runbook lze spustit z portálu Microsoft Azure nebo z externího procesů pomocí několik způsobů například prostředí PowerShell nebo rozhraní API automatizaci.

- [Spuštění postupu runbook v Azure automatizaci](../automation/automation-starting-a-runbook.md)
- [Azure rutiny pro automatizaci](https://msdn.microsoft.com/library/dn690262.aspx)
- [Automatizace REST API](https://msdn.microsoft.com/library/mt662285.aspx)
- [Automatizace .NET](https://msdn.microsoft.com//library/mt465763.aspx)

### <a name="alerts"></a>Upozornění

Pravidla výstrah automaticky vyhledávat protokolu podle plánu.  Pokud výsledky podle určitého kritéria výsledné upozornění můžete spustit postupu runbook v Azure automatizaci nebo volání webhook, které můžete začít externí proces.  Obě tyto odpovědi může obsahovat podrobnosti o upozornění včetně data vrácená do pole Hledat protokolu.

- [Upozornění v protokolu analýzy](../log-analytics/log-analytics-alerts.md)
- [Rozhraní API protokolu analýzy upozornění](../log-analytics/log-analytics-api-alerts.md)


## <a name="backup-and-site-recovery"></a>Zálohování a obnovení webu

Azure zálohování a obnovení webu poskytování služeb pro ochranu podnikových dat a za ověření oprávněnosti dostupnost servery a aplikací.  Můžete využít těchto služeb k provedení těchto situací poskytování služby zálohování aplikace nebo zahájení přepojení virtuálního počítače.

- [Azure rutiny pro zálohování](https://msdn.microsoft.com/library/mt619253.aspx)
- [Obnovení Azure webu rozhraní REST API](https://msdn.microsoft.com/library/azure/mt750497.aspx)
- [Rutiny pro obnovení Azure webů](https://msdn.microsoft.com/library/mt637930.aspx)

## <a name="custom-solutions"></a>Vlastní řešení

Zapouzdřit integrace logiky do vlastního řešení spuštěním pracovního prostoru nebo v pracovním prostoru zákazníka.  Řešení mohou obsahovat metody integrace v tomto článku kromě dalších zdrojů k poskytování scénáři úplné řízení.  Zdroje v řešení stažení a instalace balíčku tak, aby se při odebrání řešení všechny zdroje, které vytvořil jsou odebrali Azure předplatného a OMS pracovního prostoru.

Řešení může například automatizaci postupu runbook shromáždění a zpracování dat a potom naplnění úložišti protokolu analýzy pomocí rozhraní API HTTP dat kolekcí.  Mohou také obsahovat vlastní zobrazení, které představuje a analýzy dat shromážděných.  

- Vytvořením vlastních řešení (už brzo)    

## <a name="next-steps"></a>Další kroky
- Odkaz na [OMS SDK](operations-management-suite-sdk.md) technické informace o automatizaci OMS služby.  
