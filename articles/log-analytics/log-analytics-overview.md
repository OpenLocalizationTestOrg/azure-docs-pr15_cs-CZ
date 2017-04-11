<properties
   pageTitle="Co je technologie pro analýzu protokolu? | Microsoft Azure"
   description="Technologie pro analýzu protokolu je služba v operace správy sady (OMS), který vám pomůže shromažďovat a analyzovat provozní data generovaná zdrojů v vaše cloudové a místního prostředí.  Tento článek obsahuje stručný přehled různých složek protokolu technologie pro analýzu a odkazy na podrobný obsah."
   services="log-analytics"
   documentationCenter=""
   authors="bwren"
   manager="jwhit"
   editor="tysonn" />
<tags
   ms.service="log-analytics"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="09/06/2016"
   ms.author="bwren" />

# <a name="what-is-log-analytics"></a>Co je technologie pro analýzu protokolu?
Protokol analýzy je služba v [operace správy sadu \(OMS\) ](../operations-management-suite/operations-management-suite-overview.md) , který umožňuje shromáždit a analýza dat generovaných zdrojů ve vaší cloudu a místního prostředí. Máte data v reálném přehledy snadno analyzovat milióny záznamů ve všech pracovního vytížení a servery bez ohledu na jejich fyzické umístění pomocí integrovaného vyhledávání a vlastní řídicí panely.


## <a name="log-analytics-components"></a>Součásti analýzy protokolu
V centru protokolu analýzy je OMS úložiště, který je hostovaný v Azure cloudu.  Data se shromažďují do úložiště z propojených zdrojů konfiguraci zdroje dat a přidání řešení k předplatnému.  Zdroje dat a řešení každý vytvoří různé typy záznamů, které jste svoje vlastní sadu vlastností, ale může pořád analýzy pohromadě v dotazech úložiště.  Umožňuje použít stejné nástroje a metody pro práci s různými typy dat shromážděných podle různých zdrojů.


![OMS úložiště](media/log-analytics-overview/overview.png)


Připojení zdroje jsou počítači a dalších zdrojů, které mohou generovat data shromážděná protokolu analýzy.  To můžete zahrnout agentů nainstalovaný v [systému Windows](log-analytics-windows-agents.md) a [Linux](log-analytics-linux-agents.md) počítačích přímé připojení nebo agentům [připojení System Center Operations Manager Správa skupiny](log-analytics-om-agents.md).  Protokol analýzy taky můžete shromáždit data od [Azure úložiště](log-analytics-azure-storage.md).

[Zdroje dat](log-analytics-data-sources.md) jsou různé druhy dat shromážděných z každé připojení zdroje.  Platí to i událostí a [data o výkonu](log-analytics-data-sources-performance-counters.md) z [Windows](log-analytics-data-sources-windows-events.md) a Linux agentů kromě zdrojům, jako jsou [služby IIS protokoly](log-analytics-data-sources-iis-logs.md)a [vlastního textu](log-analytics-data-sources-custom-logs.md).  Konfigurace jednotlivé zdroje dat, která chcete shromažďovat a konfiguraci automaticky doručila ji do každého připojení zdroje.


## <a name="analyzing-log-analytics-data"></a>Analýza protokolu analýzy dat
Většinu interakce s protokolu analýzy budou prostřednictvím portálu OMS, který běží v jakémkoliv prohlížeči a poskytuje můžete s přístupem k nastavení a více nástroje analyzovat a pracovat na údaje.  Z portálu můžete využít [vyhledávání protokolu](log-analytics-log-searches.md) kde vytvářet dotazy pro účely analýzy dat shromážděných, [řídicí panely](log-analytics-dashboards.md) , kterou si můžete přizpůsobit pomocí grafické zobrazení nejužitečnějšího hledání a [řešení](log-analytics-add-solutions.md) , které poskytují další funkce a nástrojů pro analýzu.

![Portál OMS](media/log-analytics-overview/portal.png)


Protokol analýzy poskytuje syntaxi dotazu můžete rychle načíst a sloučení dat v úložišti.  Můžete vytvořit a uložit [Protokolu vyhledávání](log-analytics-log-searches.md) k analýze dat na portálu OMS přímo nebo hledání protokolu spuštění automaticky vytvořit upozornění, pokud výsledky dotazu označení důležitých podmínky.

![Hledání protokolu](media/log-analytics-overview/log-search.png)

A udělte tak rychlý grafický zobrazení stavu celkové prostředí, můžete přidat vizualizace uložený protokol vyhledávání [řídicího panelu](log-analytics-dashboards.md).   

![Řídicí panel](media/log-analytics-overview/dashboard.png)

K analýze dat mimo protokolu analýzy, můžete exportovat data z úložiště OMS do nástrojů, jako jsou [Power BI](log-analytics-powerbi.md) nebo Excel.  Můžete taky využít [Rozhraní API pro hledání protokolu](log-analytics-log-search-api.md) k vytvoření vlastních řešení, které využívají protokol analýzy dat nebo integrovat s jinými systémy.

## <a name="solutions"></a>Řešení
Řešení přidat funkci do protokolu analýzy.  Tyto primárně spustit v cloudu a poskytnout analýzu dat shromážděných v úložišti OMS. Mohou také definovat nové typy záznamů Shromažďované, které lze analyzovat protokolu vyhledávání nebo další uživatelské rozhraní služby řešení v řídicím panelu OMS.  

![Změna řešení sledování](media/log-analytics-overview/change-tracking.png)


Jsou k dispozici pro různé funkce řešení a můžete snadno procházet dostupné řešení a [přidejte je do pracovního prostoru OMS](log-analytics-add-solutions.md) z Galerie řešení.  V mnoha bude automaticky nasazeném a začít pracovat, když ostatní může vyžadovat některé konfiguraci.

![Galerie řešení](media/log-analytics-overview/solution-gallery.png)

## <a name="log-analytics-architecture"></a>Architektura technologie pro analýzu protokolu
Nasazení požadavkům analýzy protokolu jsou minimální, protože centrální součásti hostovaný v Azure cloudu.  Jedná se o úložišti kromě služby, které umožňují sladit a analýza dat shromážděných.  Na portálu můžete k nim získat přístup z libovolného prohlížeče, není k dispozici žádné povinné pro klientský software.

Je nutné nainstalovat agentů počítačích [Windows](log-analytics-windows-agents.md) a [Linux](log-analytics-linux-agents.md) , ale není žádný další agent povinné u počítačů, které už jsou členy [připojení SCOM Správa skupiny](log-analytics-om-agents.md).  SCOM agentů zůstanou komunikace se servery správy, které bude předávat data do protokolu analýzy.  Řešení některých přes budou vyžadovat agentů přímo komunikovat s protokolu analýzy.  Dokumentace pro každou řešení určí dodržováním předpisů komunikace.

Po [registraci protokolu technologie pro analýzu](log-analytics-get-started.md)vytvoříte pracovního prostoru OMS.  Pracovní prostor si můžete představit jako jedinečné prostředí OMS s vlastním datový úložiště zdroje dat a řešení. Můžete vytvořit více pracovních prostorů ve vašem předplatném podporu více prostředí například výrobní a otestovat.

![Architektura technologie pro analýzu protokolu](media/log-analytics-overview/architecture.png)


## <a name="next-steps"></a>Další kroky

- [Zaregistrovat si bezplatný účet protokolu technologie pro analýzu](log-analytics-get-started.md) a otestujte v vlastní prostředí.
- Zobrazení různých [Zdrojů dat](log-analytics-data-sources.md) k dispozici pro shromažďování dat do úložiště OMS.
- [Procházet dostupné řešení v Galerii řešení](log-analytics-add-solutions.md) pro přidání funkcí analýzy protokolu.
