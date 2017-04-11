<properties
    pageTitle="Požadovat prognózy technické příručky energie | Microsoft Azure"
    description="Technická příručka k šabloně řešení s Microsoft Cortana měřítka pro službu Prognóza v energie."
    services="cortana-analytics"
    documentationCenter=""
    authors="yijichen"
    manager="ilanr9"
    editor="yijichen"/>

<tags
    ms.service="cortana-analytics"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="05/16/2016"
    ms.author="inqiu;yijichen;ilanr9"/>

# <a name="technical-guide-to-the-cortana-intelligence-solution-template-for-demand-forecast-in-energy"></a>Technická příručka k šabloně Cortana Intelligence řešení pro službu Prognóza v energie

## <a name="overview"></a>**Základní informace**

Řešení šablony jsou určeny pro urychlení proces vytváření E2E ukázku nad Cortana Intelligence sadu. Nasazeném šablony zřízení předplatného s potřebné součástí řady Cortana a vytvoření relace mezi. Také osazuje kanálu dat s ukázkovými daty začíná generované aplikace simulace data. Stažení data simulator uvedeného odkazu a nainstalovat ho na místním počítači, souboru readme.txt pokyny k používáte simulator. Informace shromážděné z simulator bude hydrate dat kanálem k odesílání zpráv a začněte vytvářet předpovědí výukové počítače, který můžete vizualizovat na řídicím panelu Power BI.

Šablona řešení najdete [tady](https://gallery.cortanaintelligence.com/SolutionTemplate/Demand-Forecasting-for-Energy-1) 

Procesu nasazení vás provede několik pokynů k pilotnímu přihlašovacích údajů řešení. Zkontrolujte, že záznam těchto přihlašovacích údajů například řešení jméno, uživatelské jméno a heslo zadané při nasazení.

Cílem tento dokument je popisují architektura odkaz a jednotlivých složek zřízení ve vašem předplatném jako součást této šablony řešení. Dokument taky o tom, jak nahraďte vzorová data s reálným daty vlastní přehledy/předpovědí od vás získané data viděl kdokoliv mluví. Kromě toho dokument hovoří o díly šablona řešení, které by bylo nutné změnit, pokud chcete přizpůsobit řešení na vlastních datech. Pokyny k vytváření řídicího panelu Power BI pro tuto šablonu řešení jsou k dispozici na konci.

## <a name="big-picture"></a>**Celkového rámce**

![](media\cortana-analytics-technical-guide-demand-forecast\ca-topologies-energy-forecasting.png)

### <a name="architecture-explained"></a>Architektura vysvětlení
Při nasazení řešení jsou aktivní různé služby Azure v rámci sady analýzy Cortana (*tedy* události centrální toku analýzy HDInsight, Data Factory, výukové počítače, *atd.*). Výše uvedené architektura diagram znázorňuje na vysoké úrovni, jak službu Prognózování energie řešení šablony je vytvořen od začátku do konce. Bude moct prozkoumat těchto služeb klepnutím na ně v diagramu šablony řešení vytvořená pomocí nasazení řešení. V následujících částech jsou popsány každý úsek.

## <a name="data-source-and-ingestion"></a>**Zdroje dat a přijímání**

### <a name="synthetic-data-source"></a>Zdroj syntetické dat

Pro tuto šablonu vygeneruje se z desktopové aplikace, který bude stáhnout a spustit místně po úspěšné zavedení zdroj dat použít. Najdete pokyny ke stažení a instalaci této aplikace v pruhu vlastnosti po výběru prvního uzel s názvem Simulator dat Prognózování energie v diagramu šablony řešení. Tato aplikace informační kanály služby [Azure události centrální](#azure-event-hub) datových bodů nebo událostí, které se použije ve zbývající části tok řešení.

Generování aplikace událost naplní centrální události Azure pouze během spouštění v počítači.

### <a name="azure-event-hub"></a>Centrální Azure události

Služba [Azure události centrální](https://azure.microsoft.com/services/event-hubs/) je příjemce zadání od syntetické zdroje dat jsme je popsali výše.

## <a name="data-preparation-and-analysis"></a>**Příprava dat a analýzy**


### <a name="azure-stream-analytics"></a>Azure toku analýzy

Služba [Azure toku Analytics](https://azure.microsoft.com/services/stream-analytics/) slouží k poskytnout poblíž v reálném čase analýzy na vstupní proudu služby [Azure události rozbočovači](#azure-event-hub) a publikovat výsledků do řídicího panelu [Power BI](https://powerbi.microsoft.com) a také archivace všech nezpracovanými příchozí událostí pro službu [Azure úložiště](https://azure.microsoft.com/services/storage/) pro pozdější zpracování službou [Azure Data Factory](https://azure.microsoft.com/documentation/services/data-factory/) .

### <a name="hd-insights-custom-aggregation"></a>Vlastní agregace HD přehledy

Služba Azure HD přehled slouží k spouštěly skripty [podregistru](http://blogs.msdn.com/b/bigdatasupport/archive/2013/11/11/get-started-with-hive-on-hdinsight.aspx) (orchestrated tak, že Azure Data Factory) dát agregace vazbu nezpracovanými události, které byly archivovány pomocí služby Azure toku analýzy.

### <a name="azure-machine-learning"></a>Výukové Azure počítače

Použití služby [Azure počítače výukové](https://azure.microsoft.com/services/machine-learning/) (orchestrated tak, že Azure Data Factory) zvětšit prognózy na spotřebu energie budoucí konkrétní oblast zadaných vstupů dostali.

## <a name="data-publishing"></a>**Publikování dat**


### <a name="azure-sql-database-service"></a>Azure SQL databáze služby

[Databáze SQL Azure](https://azure.microsoft.com/services/sql-database/) použití služby pro ukládání (spravuje Azure Data Factory) předpovědí službou Azure počítače výukové, které bude potřeba v řídicím panelu [Power BI](https://powerbi.microsoft.com) .

## <a name="data-consumption"></a>**Spotřeba dat**

### <a name="power-bi"></a>Power BI

Služba [Power BI](https://powerbi.microsoft.com) je použité ke znázornění řídicího panelu, který obsahuje agregace za předpokladu, že podle [Analýzy toku Azure](https://azure.microsoft.com/services/stream-analytics/) služby, jakož i službu prognózy výsledky uložených v [Databázi SQL Azure](https://azure.microsoft.com/services/sql-database/) , které byly vytvořené pomocí služby [Azure počítače výukové](https://azure.microsoft.com/services/machine-learning/) . Pokyny k vytváření řídicího panelu Power BI pro tuto šablonu řešení naleznete v části.

## <a name="how-to-bring-in-your-own-data"></a>**Jak zkrácení vlastními daty**

Tato část popisuje, jak přenesení vlastních dat Azure a oblasti, které vyžadují změny pro data, která přenést do této architektury.

Není pravděpodobné, že všechny datovou sadu, kterou můžete přenést by odpovídalo datové sady použita pro tuto šablonu řešení. Principy vaše data a požadavky budou důležité v tom, jak změnit tuto šablonu pro práci s vlastními daty. Pokud je první vystavení služby Azure počítače výukové, můžete získat Úvod k němu pomocí příkladu v [tom, jak vytvořit svůj první testu](machine-learning\machine-learning-create-experiment.md).

Následující části popisují, v částech šablony, která vyžaduje úpravy po nové datové sady.

### <a name="azure-event-hub"></a>Centrální Azure události

Služby [Azure události centrální](https://azure.microsoft.com/services/event-hubs/) je velmi obecný tak, aby se data můžete publikované na centrální ve formátu CSV nebo JSON. Žádné zvláštní zpracování probíhá v centru Azure událost, ale je důležité že pochopit data, která je vloženo do ho.

V tomto dokumentu není popisují jedí dat, ale jednu jednoduše posílat události nebo dat k rozbočovači události Azure pomocí rozhraní [API centrální události](event-hubs\event-hubs-programming-guide.md).

### <a name="azure-stream-analytics"></a>Azure toku analýzy

Služba [Azure toku Analytics](https://azure.microsoft.com/services/stream-analytics/) slouží k poskytování poblíž v reálném čase analýzy podle tématu z datových proudů a výstupu dat na libovolný počet zdrojů.

Pro službu odhady energie řešení šablony analýzy toku Azure dotazu sestává z dva dílčí dotazy a jinými události ze služby Azure události centrální jako vstupů a každého museli výstupy dvou různých místech. Tyto výstupy obsahují jednu datovou sadu Power BI a jednoho umístění v úložišti Azure.

Dotaz [Azure toku analýzy](https://azure.microsoft.com/services/stream-analytics/) najdete tak, že:

-   Přihlášení k [portálu Správa Azure](https://manage.windowsazure.com/)

-   Vyhledání úlohy analýzy toku ![](media\cortana-analytics-technical-guide-demand-forecast\icon-stream-analytics.png) , které byly vytvořeny při nasazení řešení. Jedna je pro předání dat do objektů blob úložiště (například mytest1streaming432822asablob) a druhý pro předání dat do Power BI (například mytest1streaming432822asapbi).


-   Výběr

    -   ***Zadávání*** zobrazíte vstup dotazu

    -   ***Dotaz*** můžete zobrazit samotný dotaz

    -   Chcete-li zobrazit různé výstupy ***výstup***

Informace o vytváření analýz toku Azure dotazu najdete v [Toku analýzy dotazu odkaz](https://msdn.microsoft.com/library/azure/dn834998.aspx) na webu MSDN.

V tomto řešení úlohy Azure toku analýzy výstupy datovou sadu s poblíž v reálném čase analýzy informace o příchozí toku dat do řídicího panelu Power BI zadané jako součást této šablony řešení. Protože implicitní znalost příchozí formát dat ve sloupcích se tyto dotazy by bylo nutné změnit podle svého formát dat ve sloupcích.

Další analýzy toku Azure úlohy výstup všech událostí [Události rozbočovači](https://azure.microsoft.com/services/event-hubs/) k [Základnímu úložišti Azure](https://azure.microsoft.com/services/storage/) a tedy vyžaduje žádné změny bez ohledu na formát dat jako informace úplné události se streamují k základnímu úložišti.

### <a name="azure-data-factory"></a>Azure Data Factory

Služby [Azure Data Factory](https://azure.microsoft.com/documentation/services/data-factory/) orchestrates pohybu a zpracování údajů. V prognózách služba šablona řešení energie factory dat je tvořen dvanáct [kanály](data-factory\data-factory-create-pipelines.md) , přesunutí a zpracování dat pomocí různých technologií.

  Po otevření uzel Data Factory v dolní části diagramu šablony řešení vytvořeného s nasazením řešení se dostanete data factory. Tím přejdete do továrny dat na portálu Správa Azure. Pokud se zobrazí pod vaší datové sady chyby, můžete můžou být ignorovat jsou kvůli dat factory nasazena před spuštěním generátor dat. Tyto chyby nezabrání dat factory fungovat.

Tato část popisuje potřebné [kanály](data-factory\data-factory-create-pipelines.md) a [aktivity](data-factory\data-factory-create-pipelines.md) obsažené v [Azure Data Factory](https://azure.microsoft.com/documentation/services/data-factory/). Níže je zobrazení diagramu řešení.

![](media\cortana-analytics-technical-guide-demand-forecast\ADF2.png)

Obsahuje pět potrubí tento factory skripty [podregistru](http://blogs.msdn.com/b/bigdatasupport/archive/2013/11/11/get-started-with-hive-on-hdinsight.aspx) , které se používají k oddílů a agregovat data. Když uvedeno, tyto skripty najdete v [Úložišti Azure](https://azure.microsoft.com/services/storage/) účet vytvořený během instalace. Bude jejich umístění: demandforecasting\\\\skript\\\\podregistru\\ \\ (nebo https://[Your řešení name].blob.core.windows.net/demandforecasting).

Podobně jako [Azure toku analýzy](#azure-stream-analytics-1) dotazů skripty [podregistru](http://blogs.msdn.com/b/bigdatasupport/archive/2013/11/11/get-started-with-hive-on-hdinsight.aspx) znají implicitní o příchozí formát dat, tyto dotazy by potřeba změnit podle vašim požadavkům pro formátování a [funkce inženýrské](machine-learning\machine-learning-feature-selection-and-engineering.md) dat.

#### <a name="aggregatedemanddatato1hrpipeline"></a>*AggregateDemandDataTo1HrPipeline*

Tento [profilace](data-factory\data-factory-create-pipelines.md) profilace obsahuje jednu aktivitu - [HDInsightHive](data-factory\data-factory-hive-activity.md) aktivity pomocí [HDInsightLinkedService](https://msdn.microsoft.com/library/azure/dn893526.aspx) spouštějící skript [podregistru](http://blogs.msdn.com/b/bigdatasupport/archive/2013/11/11/get-started-with-hive-on-hdinsight.aspx) agregovat každých 10 sekund streamují služba data v transformovny úroveň hodinové úroveň oblast a v [Úložišti Azure](https://azure.microsoft.com/services/storage/) prostřednictvím úlohy Azure toku analýzy.

Skript [podregistru](http://blogs.msdn.com/b/bigdatasupport/archive/2013/11/11/get-started-with-hive-on-hdinsight.aspx) rozdělení úkolu je ***AggregateDemandRegion1Hr.hql***


#### <a name="loadhistorydemanddatapipeline"></a>*LoadHistoryDemandDataPipeline*

Tento [kanálem k odesílání zpráv](data-factory\data-factory-create-pipelines.md) obsahuje dva aktivity:
- Aktivity [HDInsightHive](data-factory\data-factory-hive-activity.md) pomocí [HDInsightLinkedService](https://msdn.microsoft.com/library/azure/dn893526.aspx) spouštějící skript podregistru agregovat data hodinové historie služba transformovny úrovni pro každou hodinu úroveň oblast a umístění v úložišti Azure během úlohy analýzy toku Azure

- [Kopírovat](https://msdn.microsoft.com/library/azure/dn835035.aspx) aktivity na počítači se přesune souhrnných dat z Azure úložiště objektů blob k databázi SQL Azure, protože ji někdo zřídil jako součást šablony instalace řešení.

Skript [podregistru](http://blogs.msdn.com/b/bigdatasupport/archive/2013/11/11/get-started-with-hive-on-hdinsight.aspx) pro tento úkol je ***AggregateDemandHistoryRegion.hql***.


#### <a name="mlscoringregionxpipeline"></a>*MLScoringRegionXPipeline*

Tyto [potrubí](data-factory\data-factory-create-pipelines.md) obsahují několik aktivity a jehož konečným výsledkem je scored předpovědí z Azure počítače výukové testu související s touto šablonou řešení. Jsou tyto hodnoty téměř shodné s výjimkou jednotlivých hodnot pouze úchyty pro jiné oblasti, která vystavila společnost různých RegionID předaný ADF kanálu a podregistru skriptu pro každou oblast.  
Tyhle aktivity obsažené v takto:
-   Aktivity [HDInsightHive](data-factory\data-factory-hive-activity.md) pomocí [HDInsightLinkedService](https://msdn.microsoft.com/library/azure/dn893526.aspx) spouštějící skript podregistru provádět agregace a funkce potřebné pro experiment výukové počítače Azure inženýrské funkce Podregistru skripty pro tento úkol jsou odpovídajících ***PrepareMLInputRegionX.hql***.

-   [Kopírovat](https://msdn.microsoft.com/library/azure/dn835035.aspx) aktivity na počítači se přesune výsledky z [HDInsightHive](data-factory\data-factory-hive-activity.md) aktivitu na jeden objektů blob Azure úložiště, který může být přístup [AzureMLBatchScoring](https://msdn.microsoft.com/library/azure/dn894009.aspx) aktivity.

-   [AzureMLBatchScoring](https://msdn.microsoft.com/library/azure/dn894009.aspx) činnost, která volá výuky počítače Azure experimentovat, jehož výsledkem výsledky umístit do jedné Azure úložiště objektů blob.

#### <a name="copyscoredresultregionxpipeline"></a>*CopyScoredResultRegionXPipeline*
Tyto [potrubí](data-factory\data-factory-create-pipelines.md) obsahovat jedinou aktivitu - aktivitu [kopírování](https://msdn.microsoft.com/library/azure/dn835035.aspx) , přesouvání výsledků testu Azure počítače výukové od odpovídajících ***MLScoringRegionXPipeline*** k databázi SQL Azure, protože ji někdo zřídil jako součást šablony instalace řešení.

#### <a name="copyaggdemandpipeline"></a>*CopyAggDemandPipeline*
Tento [potrubí](data-factory\data-factory-create-pipelines.md) obsahovat jednoho aktivity - aktivita [kopírování](https://msdn.microsoft.com/library/azure/dn835035.aspx) , přesouvání dat agregované probíhající služba od ***LoadHistoryDemandDataPipeline*** k databázi SQL Azure, protože ji někdo zřídil jako součást šablony instalace řešení.

#### <a name="copyregiondatapipeline-copysubstationdatapipeline-copytopologydatapipeline"></a>*CopyTopologyDataPipeline CopyRegionDataPipeline, CopySubstationDataPipeline,*
Tyto [potrubí](data-factory\data-factory-create-pipelines.md) obsahovat jedinou aktivitu - aktivitu [kopírování](https://msdn.microsoft.com/library/azure/dn835035.aspx) , přesouvání referenčních dat oblast/transformovny/Topologygeo, která jsou uložena do Azure úložiště objektů blob jako součást instalace řešení šablony k databázi SQL Azure, protože ji někdo zřídil jako součást šablony instalace řešení.

### <a name="azure-machine-learning"></a>Výukové Azure počítače
Experiment [Azure počítače výukové](https://azure.microsoft.com/services/machine-learning/) použít pro tuto šablonu řešení poskytuje předpověď poptávka oblasti. Testu specifické pro uvedenou množinu dat spotřebované množství a proto bude vyžadovat změnu nebo nahrazení specifické pro data, která je uvedena v.

## <a name="monitor-progress"></a>**Sledování průběhu**
Po spuštění generátor dat kanálu začne získat HYDRATOVANÝ a jednotlivých součástí vašeho řešení začněte kicking do akce následující příkazy vydané Data Factory. Existují dva způsoby můžete sledovat kanálu.

1. Zaškrtněte políčko data z úložiště objektů Blob Azure.

    Jednu úlohy toku analýzy nezpracovaná příchozí data zapisuje k úložišti objektů blob. Když kliknete na **Úložiště objektů Blob Azure** součástí vašeho řešení na obrazovce se úspěšně nasazení řešení a v pravém panelu klikněte na příkaz **Otevřít** , přejdete na [portálu Správa Azure](https://portal.azure.com). Jednou, klikněte na tlačítko na **objekty BLOB**. V dalším panelu zobrazí se seznam kontejnery. Klikněte na **"energysadata"**. Na panelu Další uvidíte složku **"demandongoing"** . Ve složce prvotní_data uvidíte složky s názvy jako data = 2016 01 28 atd. Pokud se zobrazí u těchto složek, to znamená, že nezpracovanými daty je úspěšně vytvářejí ve vašem počítači a uložené v úložišti objektů blob. Měli byste vidět soubory, které by měla být omezená velikosti v Megabajtech těchto složek.

2. Zaškrtněte políčko data z databáze SQL Azure.

    Posledním krokem kanálu je k zápisu dat (například předpovědí z počítače výukové) do SQL databáze. Se může projevit až do velikosti 2 hodin pro zobrazení dat v SQL databázi. Je možné sledovat množství zpracovávaných dat je k dispozici v databázi SQL [Azure Správa](https://manage.windowsazure.com/)portálu. V levém panelu vyhledejte SQL databáze![](media\cortana-analytics-technical-guide-demand-forecast\SQLicon2.png) a klikněte na něj. Pak najděte databázi (tedy demo123456db) a klikněte na ni. Na další stránce v části **"Připojení k databázi"** klikněte na **"Dotazů spustit jazyce Transact-SQL databáze SQL"**.

    Tady, klikněte na nový dotaz a dotaz pro počet řádků (například "select count(*) z DemandRealHourly)" narůstající velikostí databáze, počet řádků v tabulce měli zvýšit.)

3. Zaškrtněte políčko data z řídicího panelu Power BI.

    Řídicí panel kritická cesta Power BI můžete nastavit sledování nezpracovaná příchozí data. Postupujte podle instrukcí v části "Řídicího panelu Power BI".



## <a name="power-bi-dashboard"></a>**Řídicího panelu Power BI**

### <a name="overview"></a>Základní informace

Tato část popisuje, jak nastavit řídicího panelu Power BI vizualizovat data reálného času z Azure toku analýzy (kritická cesta), jakož i prognózy výsledky z Azure počítače výuky (studenou cesta).


### <a name="setup-hot-path-dashboard"></a>Nastavení kritická cesta řídicího panelu

Podle těchto kroků průvodce vás jak vizualizovat data výstup reálném čase z toku analýzy úloh, které byly vytvořeny v době nasazení řešení. [Power BI online](http://www.powerbi.com/) účet je potřeba k provedení následujících kroků. Pokud nemáte účet, můžete [ho vytvořit](https://powerbi.microsoft.com/pricing).

1.  Přidejte výstupní Power BI v Azure toku analýzy (ASA).

    -  Budete muset postupujte podle pokynů uvedených v článku [Azure toku technologie pro analýzu a Power BI: řídicího panelu v reálném čase analytics pro přenos dat v reálném čase viditelnost](stream-analytics-power-bi-dashboard.md) nastavit výstup Azure toku analýzy práce jako řídicího panelu Power BI.

    - Vyhledejte úlohy analýzy toku na [portálu Správa Azure](https://manage.windowsazure.com). Název projektu, by měl být: YourSolutionName + "streamingjob" + náhodné číslo + "asapbi" (tedy demostreamingjob123456asapbi).

    - Přidání výstupu PowerBI ASA projektu. Nastavení **výstupní Alias** jako **"PBIoutput"**. Nastavte **Název sady dat** a **název tabulky** jako **"EnergyStreamData"**. Po přidání výstup, klikněte na **"Start"** v dolní části stránky spuštění úlohy toku analýzy. Dostanete zprávu s potvrzením (*například*, "Počáteční toku analýzy úlohy myteststreamingjob12345asablob úspěšně").

2. Přihlaste se k [Power BI online](http://www.powerbi.com)

    -   V levém podokně část datové sady v Můj pracovní prostor můžete by měl být k dispozici nové datové sady s v levém panelu Power BI. Toto je streamování data, která posune z Azure toku analýzy v předchozím kroku.

    -   Ujistěte se, podokně ***vizualizace*** , otevřený a se zobrazují na pravé straně obrazovky.

3. Vytvoření dlaždici "Služba podle časového razítka":
    -   Klikněte na datovou sadu **"EnergyStreamData"** v levém panelu oddíl datové sady.

    -   Klikněte na ikonu **"spojnicový graf"** ![](media\cortana-analytics-technical-guide-demand-forecast\PowerBIpic8.png).

    -   **Pole** panelu klikněte na tlačítko "EnergyStreamData".

    -   Klikněte na **"Časové razítko"** a ujistěte se, že se zobrazí pod zaškrtávacím políčkem "Osa". Klikněte na možnost **"Zatížení"** a ujistěte se, že se zobrazí v části "Hodnoty".

    -   Nahoře na tlačítko **Uložit** a pojmenujte sestavu jako "EnergyStreamDataReport". V části sestav v navigačním podokně na levé straně zobrazí zprávu s názvem "EnergyStreamDataReport".

    -   Klikněte na **"připnout vizuální"** ![](media\cortana-analytics-technical-guide-demand-forecast\PowerBIpic6.png) ikonu v pravém horním rohu tento spojnicový graf a okno s "Připnout do řídicího panelu" může zobrazit umožňující zvolit řídicího panelu. Vyberte "EnergyStreamDataReport" a pak klikněte na "Připnout".

    -   Umístěte ukazatel myši nad této dlaždici na řídicím panelu klikněte na "upravit" ikonu v pravém horním rohu změnit její název jako "Služba podle časového razítka"

4.  Vytvoření ostatní dlaždice řídicího panelu podle potřeby datové sady. Zobrazit konečný řídicích panelů jsou uvedeny níže.
        ![](media\cortana-analytics-technical-guide-demand-forecast\PBIFullScreen.png)


### <a name="setup-cold-path-dashboard"></a>Nastavení studenou cestu řídicího panelu
V studenou cestu datového kanálu základní cílem je najít služba předpověď každou oblast. Power BI se připojí k databázi Azure SQL jako zdroj dat, kde jsou uloženy výsledky předpovědí.

> [AZURE.NOTE] 1) trvá několik hodin shromažďování dostatečně prognózy výsledky řídicího panelu. Doporučujeme, abyste že tento proces spustit 2-3 hodin od něco generátor dat. 2) v tomto kroku je předpoklad si stáhněte a nainstalujte bezplatný software [Power BI desktop](https://powerbi.microsoft.com/desktop).



1.  Pokud potřebujete přihlašovací údaje pro databázi.

    Musíte se **název databázového serveru, jméno, uživatelské jméno a heslo databáze** teprve pak přejděte k dalším krokům. Tady najdete postup, jak vás jak je najít.

    -   Jakmile **"databáze SQL Azure"** v diagramu šablony řešení změní zelené, klikněte na ni a klikněte na **"Otevřít"**. Řídí na portálu Správa Azure a otevře se taky stránka informace databáze.

    -   Na stránce můžete najít oddíl "Databáze". Zobrazí se seznam se databáze, kterou jste vytvořili. Název databáze by měl být **"Váš název řešení náhodného čísla +"db""** (například "mytest12345db").

    -   Klikněte na svou databázi v nové pop mimo panel můžete najít název databázového serveru nahoře. Vaší databáze serveru název název očekávané být **"Váš název řešení náhodného čísla +"database.windows.net,1433""** (například "mytest12345.database.windows.net,1433").

    -   Databáze **uživatelské jméno** a **heslo** jsou stejné jako uživatelské jméno a heslo zaznamenaných během nasazení řešení.

2.  Aktualizace zdroje dat Power BI souboru studenou cesta
    -  Zkontrolujte, že máte nainstalovanou nejnovější verzi [Power BI desktop](https://powerbi.microsoft.com/desktop).

    -   Ve složce **"DemandForecastingDataGeneratorv1.0"** , který jste stáhli poklikejte na soubor **"Power BI Template\DemandForecastPowerBI.pbix"** . Počáteční vizualizace vycházejí formální data. **Poznámka:** Pokud se zobrazí chyba masáže, zkontrolujte, jestli jste nainstalovali nejnovější verzi Power BI Desktop.

        Po otevření, horní části souboru, klikněte na **Upravit dotazů**. V pop se okno poklikejte na **"Zdrojový"** na pravém panelu.
    ![](media\cortana-analytics-technical-guide-demand-forecast\PowerBIpic1.png)

    -   V pop se okno **"Server"** a **"Databáze"** nahraďte vlastní název serveru a databáze a klikněte na tlačítko **"OK"**. Název serveru zkontrolujte, že zadáte port 1433 (**YourSolutionName.database.windows.net 1433**). Ignorujte upozornění, které se zobrazí na obrazovce.

    -   V dalším pop se okno zobrazí se dvě možnosti v levém podokně (**Windows** a **databáze**). Klikněte na **"Databáze"**, zadejte **"Uživatelské jméno"** a **"Heslo"** (Toto je uživatelské jméno a heslo, které jste zadali při prvním nasazení řešení a vytvoření databáze Azure SQL). V části ***Vybrat stupeň použít toto nastavení***zaškrtněte možnost úrovně databáze. Klepněte na tlačítko **"Připojit"**.

    -   Až budete s asistencí zpátky na předchozí stránku, zavřete okno. Objeví se zpráva out – klikněte na **použít**. Nakonec klikněte na tlačítko **Uložit** uložte změny. Soubor Power BI teď vytvořila připojení k serveru. Pokud vaše vizualizace jsou prázdné, zkontrolujte, jestli že zrušíte na výběr vizualizace vizualizovat všechna data kliknutím na ikonu Guma v pravém horním rohu legendy. Pomocí tlačítka Aktualizovat, aby obsahoval nová data v vizualizace. Původně uvidíte pouze počáteční data vizualizaci nejdříve data factory aktualizovat každé 3 hodiny. Po 3 hodiny zobrazí se nové předpovědí projeví ve vizualizaci když aktualizujete data.

3. (Volitelné) Publikujte na řídicím panelu studenou cesta k [Power BI online](http://www.powerbi.com/). Všimněte si, že tento krok vyžaduje účet Power BI (nebo účet Office 365).

    -   Klikněte na **"Publikovat"** a zobrazí se okno několik sekund, než později zobrazení "Publikování na webu Power BI úspěch!" s zelená značka zaškrtnutí. Klikněte na odkaz "Otevřít demoprediction.pbix v Power BI". Podrobné pokyny najdete v části [Publikovat na Power BI Desktop](https://support.powerbi.com/knowledgebase/articles/461278-publish-from-power-bi-desktop).

    -   Při vytváření řídicího nového: klikněte **+** znaménko vedle části **řídicího panelu** v levém podokně. Zadejte název "Služba Prognózování ukázku" pro tento nový řídicí panel.

    -   Po otevření sestavy klikněte na tlačítko ![](media\cortana-analytics-technical-guide-demand-forecast\PowerBIpic6.png) Připnutí všechny vizualizace do řídicího panelu. Podrobné pokyny najdete v tématu [Připnout dlaždici řídicího panelu na Power BI ze sestavy](https://support.powerbi.com/knowledgebase/articles/430323-pin-a-tile-to-a-power-bi-dashboard-from-a-report).
        Přejděte na stránku řídicího panelu a upravte velikost a umístění vizualizaci a upravovat jejich názvů. Podrobné informace o tom, jak upravit dlaždic najdete v části [Upravit dlaždici – změnit velikost, přesunout, přejmenovat, kód pin, odstranit, a přidat hypertextový odkaz](https://powerbi.microsoft.com/documentation/powerbi-service-edit-a-tile-in-a-dashboard/#rename). Tady je příklad řídicího panelu s vizualizacemi některé studenou cestu připnuté k němu.

        ![](media\cortana-analytics-technical-guide-demand-forecast\PowerBIpic7.png)

4. (Volitelné) Naplánování aktualizace zdroje dat
    -     K naplánování aktualizace dat, umístěte ukazatel myši nad **EnergyBPI konečný** datovou sadu, klikněte na tlačítko ![](media\cortana-analytics-technical-guide-demand-forecast\PowerBIpic3.png) a klikněte na **Naplánovat aktualizaci**.
    **Poznámka:** Pokud se zobrazí upozornění masáž, klikněte na **Upravit pověření** a ujistěte se, vaše přihlašovací údaje pro databázi jsou popsané v kroku 1.

    ![](media\cortana-analytics-technical-guide-demand-forecast\PowerBIpic4.png)

    -   Rozbalení oddílu **Naplánovat aktualizaci** . Zapněte možnost "pravidelná aktualizace dat".

    -   Naplánování aktualizace podle vašich potřeb. Další informace najdete v tématu [aktualizace dat v Power BI](https://powerbi.microsoft.com/documentation/powerbi-refresh-data/).


## <a name="how-to-delete-your-solution"></a>**Jak odstranit řešení**
Ověřte, zda nástroj Generátor dat při použití nejsou aktivně řešení, jak systém generátor dat bude vynakládá vyšší. Pokud nepoužíváte ji odstraňte řešení. Odstranění řešení budou odstraněny všechny komponenty zřízení ve vašem předplatném při nasazení řešení. Chcete-li odstranit řešení klikněte na název vašeho řešení v levém panelu šablona řešení a klepněte na odstranit.

## <a name="cost-estimation-tools"></a>**Nástroje odhad nákladů**

Jsou dostupné vám lepší přehled o celkové náklady spojené s spuštěna služba Prognózování šablona energie řešení ve vašem předplatném následující dva nástroje:

-   [Nástroj odhad nákladů Microsoft Azure (online)](https://azure.microsoft.com/pricing/calculator/)

-   [Microsoft Azure náklady odhad nástroj (desktopová aplikace)](http://www.microsoft.com/download/details.aspx?id=43376)

## <a name="acknowledgements"></a>**Potvrzení**
Tento článek je autorem vědecký dat pracovník Yijing Chen a software pracovníkům Qiu Min společnosti Microsoft.
