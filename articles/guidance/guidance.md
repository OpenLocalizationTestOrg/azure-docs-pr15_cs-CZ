
<properties
   pageTitle="Azure pokyny | Příklady a postupy | Microsoft Azure"
   description="Osvědčené postupy a pokyny pro Azure"
   services=""
   documentationCenter="na"
   authors="bennage"
   manager="marksou"
   editor=""
   tags=""/>

<tags
   ms.service="guidance"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/17/2016"
   ms.author="christb"/>

# <a name="azure-guidance"></a>Pokyny pro Azure

[AZURE.INCLUDE [pnp-header](../../includes/guidance-pnp-header-include.md)]

Týmu služeb Microsoft vzorků a postupy je součástí poradní týmu Azure zákazníka. Náš účel usnadnit vývojáři, tvůrci a odborníky úspěšný platformy Microsoft Azure. Jsme vyvíjet pokyny zobrazující doporučené postupy pro vytváření cloudové řešení na Azure.

## <a name="checklists"></a>Seznamy úkolů

Stručný přehled pro revize základní aspekty dostupnost a škálovatelnost jsou tyto seznamy. 

- [Kontrolní seznam dostupnosti][AvailabilityChecklist] 

    Přehled doporučené postupy pro zajištění dostupnosti.

- [Kontrolní seznam škálovatelnost:][ScalabilityChecklist]

    Přehled doporučené postupy pro návrh a implementace scalable služby a zpracování správy dat.

## <a name="best-practices-articles"></a>Doporučené postupy články

Tyto články Zadejte podrobný popis důležitých principů běžně přidružených cloudu výpočetních. 

- [Rozhraní API návrhu][APIDesign] 

    Diskusní zvážit při návrhu webového rozhraní API.

- [Implementace rozhraní API][APIImplementation] 

    Sada doporučené postupy pro implementaci a webového rozhraní API pro publikování.

- [Pokyny k rozhraní API zabezpečení](https://github.com/mspnp/azure-guidance/blob/master/API-security.md) 

    Diskuse a tak mohli ověřovat pochybnosti (například typy tokenů se tak mohli ověřovat protokoly, se tak mohli ověřovat toků a zmírnění hrozbou, že).

- [Pokyny pro neobsahovaly text][AutoscalingGuidance] 

    Přehled aspektech pro změnu velikosti řešení bez nutnosti pro ruční zásah.

- [Pokyny pro úlohy na pozadí][BackgroundJobsGuidance] 

    Popis jednotlivých možností a doporučené postupy pro provádění úkolů provedených na pozadí, nezávisle mezi libovolnými popředí nebo interaktivní operace.

- [Pokyny pro obsahu doručení Network (CDN)][CDNGuidance] 

    Obecné pokyny a doporučený postup pro použití CDN minimalizovat zatížení aplikace a maximalizovat dostupnost a výkonu.

- [Pokyny pro ukládání do mezipaměti][CachingGuidance] 

    Přehled použití mezipaměti zlepšit výkon a škálovatelnost systému.

- [Pokyny pro Partitioning dat][DataPartitioningGuidance]

    Strategie, můžete data oddílu zlepšit škálovatelnost omezit konflikty a optimalizaci výkonu.

- [Sledování a diagnostice pokyny][MonitoringandDiagnosticsGuidance] 

    Pokyny ke sledování jak uživatelé využít systému sledování využití prostředků a obecně sledovat výkon systému a stavu.

- [Doporučené názvů][naming-conventions] 

    Doporučené konvence pro Azure zdroje.

- [Opakovat obecné pokyny][RetryGeneralGuidance] 

    Informace o obecné koncepty pro zpracování přechodná chyby.

- [Opakovat pokyny specifických pro službu][RetryServiceSpecificGuidance]

    Přehled funkcí opakovat pro mnoho Azure služby, třeba informace týkající se použití, přizpůsobit nebo rozšíření mechanismus opakovat pro službu.

## <a name="scenario-guides"></a>Scénář vodítka

- [Spuštění Elasticsearch na Azure][elasticsearch] 
    
    Elasticsearch je velmi scalable otevřít zdroj vyhledávacím webu a databáze. Je vhodné v situacích, které vyžadují rychlé analýzy a vyhledání informací v případě velkých sad dat. Tento návod pohledů některé klíčové s navrhováním Elasticsearch obrázku.

- [Správa identit víceklientské aplikací][identity-multitenant] 
    
    Multitenancy je architektura kdy víc klientů sdílení aplikace, ale jsou izolovaný od sebe. Tyto pokyny jako ukázka správy identit uživatelů v víceklientské aplikace pomocí [Služby Azure Active Directory] [ AzureAD] zpracovávání přihlašování a ověření.
    
- [Vývoj řešení velký dat](https://msdn.microsoft.com/library/dn749874.aspx)

    Tato příručka, prozkoumá využívání HDInsight scénáře například iterativních průzkum jako datový sklad ETL procesy a integrace do stávajících systémů BI. Obsahuje taky pokyny ke Principy koncepcí velkých dat, plánování a řešení velký dat navrhování a implementaci tato řešení.
    
## <a name="patterns"></a>Vzorky

- [Cloudu provedeních: Pokyny pro obvyklé architektura získáte cloudu](https://msdn.microsoft.com/library/dn568099.aspx)

    Shluk provedeních je knihovna provedeních a podle pokynů souvisejících témat. Articulates výhody použití vzorků zobrazením, jak může každý úsek zapadá do cloudu aplikace architektury.
    
- [Optimalizace výkonu získáte cloudu](https://github.com/mspnp/performance-optimization)

    Zásady se průzkum běžné ochrany proti vzorky, které brání aplikace z měřítka zatížení. Obsahuje ukázky, které demonstrují osm ochrany proti vzorků a [Základy Analýza výkonu](https://github.com/mspnp/performance-optimization/blob/master/Performance-Analysis-Primer.md) a příručka pro členy tý [vyhodnocování výkonu proti klíčových metrik](https://github.com/mspnp/performance-optimization/blob/master/Assessing-System-Performance-Against-KPI.md).

## <a name="reference-architectures"></a>Odkaz architektury

Náš odkaz architektury jsou uspořádány podle scénáře.
Každou jednotlivé architekturu nabízí doporučené postupy a obvyklé kroky a komponentu spustitelný ztělesňuje doporučení.

Aktuální knihovnu architektury odkaz je dostupný na [http://aka.ms/architecture](http://aka.ms/architecture).

## <a name="resiliency-guidance"></a>Pokyny pro odolnost proti chybám

Tato témata popisují, jak k návrhu aplikace, které jsou pružné selhání v prostředí distribuované cloudu.   

- [Přehled odolnost proti chybám][ResiliencyOvervew]

     Postup vytvoření aplikace v Azure platformy, které můžete obnovit z selhání a dál fungovat. Popisuje strukturovaný přístup k dosažení odolnost proti chybám z návrh a implementace, nasazení a operace.

- [Kontrolní seznam odolnost proti chybám][resiliency-checklist]

    Kontrolní seznam doporučení, které vám usnadní plánování různé režimy selhání, ke kterým může dojít.

- [Chyba při režimu analýzy][resiliency-fma] 

    Režim analýzy selhání (FMA) je proces pro vytváření odolnost proti chybám do systému, tak, že identifikují body možnou chybu. Jako výchozí bod pro FMA procesu Tento článek obsahuje katalog potenciální režimy selhání a jejich mitigations. 

<!-- links -->

[AzureAD]: https://azure.microsoft.com/documentation/services/active-directory/

[PerformanceOptimization]: https://github.com/mspnp/performance-optimization

[APIDesign]: ../best-practices-api-design.md
[APIImplementation]: ../best-practices-api-implementation.md
[AutoscalingGuidance]: ../best-practices-auto-scaling.md
[BackgroundJobsGuidance]: ../best-practices-background-jobs.md
[CDNGuidance]: ../best-practices-cdn.md
[CachingGuidance]: ../best-practices-caching.md
[DataPartitioningGuidance]: ../best-practices-data-partitioning.md
[MonitoringandDiagnosticsGuidance]: ../best-practices-monitoring.md
[RetryGeneralGuidance]: ../best-practices-retry-general.md
[RetryServiceSpecificGuidance]: ../best-practices-retry-service-specific.md
[RetryPolicies]: Retry-Policies.md
[ScalabilityChecklist]: ../best-practices-scalability-checklist.md
[AvailabilityChecklist]: ../best-practices-availability-checklist.md
[naming-conventions]: guidance-naming-conventions.md

<!-- guidance projects -->
[elasticsearch]: guidance-elasticsearch.md
[identity-multitenant]: guidance-multitenant-identity.md

<!-- reference architectures -->
[ref-arch-single-vm-windows]: guidance-compute-single-vm.md
[ref-arch-single-vm-linux]: guidance-compute-single-vm-linux.md
[ref-arch-multi-vm]: guidance-compute-multi-vm.md
[ref-arch-3-tier]: guidance-compute-3-tier-vm.md
[ref-arch-n-tier-windows]: guidance-compute-n-tier-vm.md
[ref-arch-n-tier-linux]: guidance-compute-n-tier-vm-linux.md
[ref-arch-multi-dc-windows]: guidance-compute-multiple-datacenters.md
[ref-arch-multi-dc-linux]: guidance-compute-multiple-datacenters-linux.md

<!-- resiliency -->
[resiliency-fma]: guidance-resiliency-failure-mode-analysis.md
[resiliency-checklist]: guidance-resiliency-checklist.md
[ResiliencyOvervew]: guidance-resiliency-overview.md

