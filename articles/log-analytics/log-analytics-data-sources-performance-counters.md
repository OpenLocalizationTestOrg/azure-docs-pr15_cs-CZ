<properties 
   pageTitle="Výkon systému Windows a Linux čítače v protokolu analýzy | Microsoft Azure"
   description="Výkonnosti shromážděné protokolu analýzy a analyzujte data výkon agentů Windows a Linux.  Tento článek popisuje, jak konfigurovat shromažďování výkonnosti pro obě Windows a Linux agenti – podrobnosti o budou uložené v úložišti OMS a jak analyzovat na portálu OMS."
   services="log-analytics"
   documentationCenter=""
   authors="bwren"
   manager="jwhit"
   editor="tysonn" />
<tags 
   ms.service="log-analytics"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/27/2016"
   ms.author="bwren" />

# <a name="windows-and-linux-performance-data-sources-in-log-analytics"></a>Windows a Linux výkonu zdroje dat v protokolu analýzy 

Výkonnosti v systému Windows a Linux poskytují přehled o výkonu hardwarové součásti, operačních systémů a aplikací.  Protokol analýzy získáte výkonnosti v pravidelných intervalech pro analýzu poblíž reálném čase (NRT) kromě agregaci data o výkonu delší dobu analýzy a vytváření sestav.

![Výkonnosti](media/log-analytics-data-sources-performance-counters/overview.png)

## <a name="configuring-performance-counters"></a>Konfigurace výkonnosti

Konfigurace výkonnosti v [nabídce Data v dialogovém okně nastavení technologie pro analýzu protokolu](log-analytics-data-sources.md#configuring-data-sources).

Při první konfiguraci Windows nebo Linux výkonu čítače pro nový pracovní prostor OMS, budete mít možnost rychle vytvořit několik běžných čítačů.  Jsou uvedeny s zaškrtávací políčko vedle každého.  Zajistěte, aby všechny čítače, které chcete během vytváření jsou zaškrtnuté a klikněte na **Přidat vybrané výkonnosti**.

![Konfigurace systému Windows výkonnosti](media/log-analytics-data-sources-performance-counters/configure-windows.png)

Pomocí následujícího postupu přidáte nového Windows výkonu proti shromáždit.

1. Zadejte název čítači v textovém poli v formát *objektu (instance) \counter*.  Když začnete psát, se zobrazí odpovídající seznam běžných čítačů.  Můžete buď vyberete čítač ze seznamu nebo zadejte vlastní.  Můžete také vracet všechny instance pro konkrétní čítač zadáním *object\counter*. 
2. Klikněte na **+** nebo stisknutím klávesy **Enter** přidejte čítač do seznamu.
3. Při přidání čítač používá jako výchozí hodnotu 10 sekund jeho **Ukončen**.  Můžete určit to na hodnotu vyšší až 1800 sekund, než (30 minut) Pokud chcete snížit požadavky na úložiště dat shromážděných výkonu.
4. Po dokončení přidávání čítače, klikněte na tlačítko **Uložit** v horní části obrazovky konfiguraci uložte.

![Konfigurace Linux výkonnosti](media/log-analytics-data-sources-performance-counters/configure-linux.png)

Pomocí následujícího postupu přidáte nového Linux výkonu proti shromáždit.

1. Ve výchozím nastavení se všechny změny konfigurace automaticky posunou všechny agentů.  Pro Linux agenti – konfiguračního souboru jsou odeslány Fluentd shromažďování dat  Pokud chcete upravit tento soubor ručně na každý Linux agent, potom zrušte zaškrtnutí políčka *použít pod konfigurace Moje počítačům Linux*.
2. Zadejte název čítači v textovém poli v formát *objektu (instance) \counter*.  Když začnete psát, se zobrazí odpovídající seznam běžných čítačů.  Můžete buď vyberete čítač ze seznamu nebo zadejte vlastní.  
2. Klikněte na **+** nebo stisknutím klávesy **Enter** přidejte čítač do seznamu dalších čítačů objektu.
3. Peaks pro konkrétní objekt použít stejné **Ukončen**.  Výchozí hodnota je 10 sekund.  Můžete změnit na hodnotu vyšší až 1800 sekund, než (30 minut) Pokud chcete snížit požadavky na úložiště dat shromážděných výkonu.
4. Po dokončení přidávání čítače, klikněte na tlačítko **Uložit** v horní části obrazovky konfiguraci uložte.

## <a name="data-collection"></a>Shromažďování dat

Přihlaste se všechny zadané výkonnosti intervalech jejich zadaný vzorku na všechny agentů, které máte nainstalované proti shromažďuje analýzy.  Není agregovaný údaj a původní data je k dispozici ve všech zobrazeních hledání protokolu po celou dobu nastavil předplatného OMS.


## <a name="performance-record-properties"></a>Výkon dialogovém okně Vlastnosti záznamu

Výkon záznamy s typem **výkon** a mít vlastnosti v následující tabulce.

| Vlastnost | Popis |
|:--|:--|
| Počítač         | Počítač, který byl odebrané události. |
| Název_čítače      | Název čítače výkonu |
| Cesta_k_čítači      | Úplnou cestu čítače ve formuláři \\ \\ \<počítače >\\objekt(instance)\\čítač. |
| Přepočtené     | Číselná hodnota čítače.  |
| Název_instance     | Název instance události.  Vyprázdnění Pokud žádná instance. |
| Název objektu       | Název objektu výkonu |
| SourceSystem  | Typ agent data byla vybrána z. <br> OpsManager – agent systému Windows, buď přímé připojení nebo SCOM <br> Linux – všechny Linux agentů  <br> AzureStorage – Azure diagnostiky |
| TimeGenerated       | Datum a čas, kdy odběr data. |


## <a name="sizing-estimates"></a>Odhadne pro změnu velikosti

 Odhad kolekce jednotlivých čítačů intervalech 10 sekund je asi 1 MB denně za instance.  Požadavky na úložiště jednotlivých čítačů pomocí tohoto vzorce lze odhadnout.

    1 MB x (number of counters) x (number of agents) x (number of instances)

## <a name="log-searches-with-performance-records"></a>Hledání protokolu se záznamy výkonu

V následující tabulce jsou uvedeny různých příklady protokolu hledání, která načítat záznamy výkonu.

| Dotaz | Popis |
|:--|:--|
| Typ = výkonu | Všechna data o výkonu |
| Typ = výkon počítače = "Počítač" | Všechna data o výkonu z určitého počítače |
| Typ = výkon název_čítače = "Aktuální délka fronty disku" | Všechna data o výkonu pro konkrétní čítač |
| Typ = výkon (název_objektu = procesor) název_čítače = "% času procesoru" Název_instance = _Total & #124; míra Avg(Average) jako AVGCPU ve počítače | Průměrná využití procesoru ve všech počítačích |
| Typ = výkon (název_čítače = "% času procesoru") & #124;  míra max(Max) ve počítače | Maximální využití procesoru ve všech počítačích |
| Typ = výkon název_objektu = logický disk název_čítače = "Aktuální Disk délka fronty" počítač = "MyComputerName" & #124; míra Avg(Average) tak, že Název_instance | Průměrná délka aktuální fronty disku ve všech instancích daný počítač |
| Typ = výkon název_čítače = "DiskTransfers/sec" & #124; míra percentile95(Average) ve počítače | 95. percentilu z disku přenosy/s ve všech počítačích |
| Typ = výkon název_čítače = "% času procesoru" Název_instance = "_Total" & #124; Změřte avg(CounterValue) ve počítače intervalu 1 HODINU | Každou hodinu průměr využití procesoru ve všech počítačích |
| Typ = výkon počítače = "Počítač" název_čítače = % * Název_instance = _Total & #124; míra percentile70(CounterValue) název_čítače intervalu 1 HODINU | Každou hodinu 70 percentilu všech procentuálně čítač % pro určitý počítač |
| Typ = výkon název_čítače = "% času procesoru" Název_instance = "_Total" (počítač = "Počítač") & #124; Změřte min(CounterValue) avg(CounterValue), percentile75(CounterValue), max(CounterValue) ve počítače intervalu 1 HODINU | Každou hodinu průměr, minimální, maximální a 75 procent využití procesoru pro konkrétní počítač |

## <a name="viewing-performance-data"></a>Zobrazení data o výkonu

Při spuštění protokolu vyhledejte data o výkonu **protokolu** zobrazení se zobrazí ve výchozím nastavení.  Pokud chcete zobrazit data v grafu, klikněte na **metriky**.  Podrobné grafické zobrazení, klikněte **+** vedle čítač.  

![Zobrazení metriky sbalit](media/log-analytics-data-sources-performance-counters/metricscollapsed.png)

Pokud časový rozsah, který jste vybrali je 6 hodin nebo nižší, zobrazí se v grafu se aktualizuje každých několik sekund, než.  Dynamická data se zobrazí v pravé části grafu v světle modrá.

![Zobrazení metriky rozbaleným panelem s dynamickými daty](media/log-analytics-data-sources-performance-counters/metricsexpanded.png)

Agregovat data o výkonu v protokolu vyhledávání naleznete v [metrických agregace na vyžádání a vizualizace v OMS](http://blogs.technet.microsoft.com/msoms/2016/02/26/on-demand-metric-aggregation-and-visualization-in-oms/).

## <a name="next-steps"></a>Další kroky

- Informace o [hledání protokolu](log-analytics-log-searches.md) k analýze dat shromážděných ze zdroje dat a jejich řešení.  
- Exportujte dat shromážděných k [Power BI](log-analytics-powerbi.md) pro další vizualizace a analýzy.