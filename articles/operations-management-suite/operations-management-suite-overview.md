<properties
   pageTitle="Přehled správy sady (OMS) operací | Microsoft Azure"
   description="Microsoft operace správy sady (OMS) je společnosti Microsoft cloudové IT řešení pro správu, která vám pomáhá spravovat a chránit vaše místních a cloudových infrastruktury.  Tento článek uvádí různé služby součástí OMS a obsahuje odkazy na podrobný obsah."
   services="operations-management-suite"
   documentationCenter=""
   authors="bwren"
   manager="jwhit"
   editor="tysonn" />
<tags
   ms.service="operations-management-suite"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/24/2016"
   ms.author="bwren" />

# <a name="what-is-operations-management-suite-oms"></a>Co je operace správy sady (OMS)?

Microsoft operace správy sady (OMS) je společnosti Microsoft cloudové IT řešení pro správu, která vám pomáhá spravovat a chránit vaše místních a cloudových infrastruktury.  Protože OMS implementovaná jako do cloudové služby, můžete jej máte zařídit i rychle s minimálními investic do infrastruktury služby.  Nové funkce doručované automaticky, tím vám probíhající údržby a upgrade náklady.

Kromě poskytování důležitých služeb samostatně OMS můžete integrovat System Center součásti například System Center Operations Manager rozšířit existující správy investic do cloudu.  System Center a OMS společně pracovat úplné hybridní prostředí management.

Následující části obsahují nejvyšší úrovně popis OMS a služby, které je provede oblasti jinou hodnotu.  Můžete vytvořit odkaz OMS architektura základní informace o různých součásti OMS před revizí podrobnou dokumentaci pro jednotlivá pole.


## <a name="insight-and-analyticsmediaoperations-management-suite-overviewicon-insight-analyticspng-insight-and-analytics"></a>![Přehled a technologie pro analýzu](media/operations-management-suite-overview/icon-insight-analytics.png) Přehled a technologie pro analýzu

[Protokol analýzy](http://azure.microsoft.com/documentation/services/log-analytics) umožňuje shromáždit sladit, Hledat a chovalo na protokolování a výkonu data generovaná operačních systémů a aplikací. Máte data v reálném provozním přehledy snadno analyzovat milióny záznamů ve všech pracovního vytížení a servery bez ohledu na jejich fyzické umístění pomocí integrovaného vyhledávání a vlastní řídicí panely.

Řešení můžete snadno přidat protokolu analýzy, které definují se shromažďují a logika pro analýzy.  Řešení může obsahovat další funkce, které automaticky dostane agentům minimálního nebo žádná konfigurace.  Kromě použití analytických nástrojů poskytnutých jednotlivých řešení, můžete vyhledávat vlastní napříč celou sadu k sladit dat mezi systémů a aplikací.  


## <a name="automation--controlmediaoperations-management-suite-overviewicon-automation-controlpng-automation--control"></a>![Automatizace a řízení](media/operations-management-suite-overview/icon-automation-control.png) Automatizace a řízení

Azure automatizaci automaticky pro správu procesů pomocí [runbooks](../automation/automation-runbook-types.md) podle prostředí PowerShell a spustit v Azure cloudu.  Runbooks zpřístupníte produkt nebo službu, dá se ovládat pomocí prostředí PowerShell včetně zdrojů v jiných mračnech například Amazon webové služby AWS.  Runbooks můžete provést taky na serveru, v místní datového centra pro správu místních zdrojů.

Azure automatizaci obsahuje konfigurační řízení pomocí [Prostředí PowerShell DSC](../automation/automation-dsc-overview.md).  Můžete vytvořit a spravovat zdroje DSC hostované v Azure a jejich použitím do cloudu a místní systémy definovat a automaticky vynutit jejich konfigurace.


## <a name="protection-and-recoverymediaoperations-management-suite-overviewicon-protection-recoverypng-protection-and-disaster-recovery"></a>![Ochrana a obnovení](media/operations-management-suite-overview/icon-protection-recovery.png) Ochrana a obnovení

[Zálohování Azure](http://azure.microsoft.com/documentation/services/backup) jenom pro roky bez kapitálových investic a s minimálními provozní náklady a chrání data aplikace.  Můžete zálohovat data z Windows serverů fyzické a virtuální kromě úloh aplikace třeba SQL Server a SharePoint.  Jej lze také tak, že systém Centrum Data Protection Manager (DPM) replikovat chráněné dat Azure pro zálohování a dlouhodobou úložiště.

[Obnovení webu Azure](http://azure.microsoft.com/documentation/services/site-recovery) přispívá k provozní kontinuitu a strategii havárie obnovení (BCDR) tak, že orchestrating replikace, převzetí a obnovení místní Hyper-V virtuálních počítačích, VMware virtuálních počítačích a fyzické servery systému Windows nebo Linux. Můžete replikovat počítačích sekundární datacentrem nebo rozšíření datového centra replikace do Azure. Obnovení webu poskytuje také jednoduché převzetí a obnovení pracovního vytížení. Lze integrovat s havárie obnovení mechanismy třeba SQL Server AlwaysOn a poskytuje obnovení plány pro snadno převzetí zatížení, které jsou vrstveny ve více počítačích.


## <a name="oms-security-and-compliancemediaoperations-management-suite-overviewicon-security-compliancepng-security-and-compliance"></a>![OMS zabezpečení a dodržování předpisů](media/operations-management-suite-overview/icon-security-compliance.png) Zabezpečení a dodržování předpisů
Zabezpečení a dodržování předpisů vám pomůže identifikovat, posuďte a zmírnění rizik zabezpečení pro infrastrukturu.  Tyto funkce OMS jsou implementovat více řešení v protokolu analýzy, který analýze dat protokolu a konfigurace z agenta systémů při zajištění probíhající zabezpečení prostředí.

- [Zabezpečení a auditování řešení](oms-security-getting-started.md ) shromažďuje a analyzuje události zabezpečení na spravované systémy k identifikaci podezřelých aktivity.
- [Antimalware řešení](log-analytics-malware.md ) zprávy o stavu antimalware ochrany v spravované sítě.  
- Řešení aktualizace systému provede analýzu aktualizace zabezpečení a jiné se aktualizuje v spravované sítě tak, aby snadno identifikovat systémy vyžadující opravy chyb.


## <a name="next-steps"></a>Další kroky
- Informace o [protokolu analýzy](http://azure.microsoft.com/documentation/services/log-analytics).
- Informace o [Azure automatizaci](../automation/automation-intro.md).
- Informace o [Azure zálohování](http://azure.microsoft.com/documentation/services/backup).
- Informace o [obnovení Azure webu](http://azure.microsoft.com/documentation/services/site-recovery).
