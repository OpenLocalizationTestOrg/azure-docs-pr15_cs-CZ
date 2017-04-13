<properties
    pageTitle="Technická příručka k šabloně Cortana Intelligence řešení za účelem prediktivní údržby šestinásobné a jiných firmy | Microsoft Azure"
    description="Technická příručka k šabloně řešení s Microsoft Cortana Intelligence za účelem prediktivní údržby šestinásobné, nástroje a doprava."
    services="cortana-analytics"
    documentationCenter=""
    authors="fboylu"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="cortana-analytics"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/13/2016"
    ms.author="fboylu" />

# <a name="technical-guide-to-the-cortana-intelligence-solution-template-for-predictive-maintenance-in-aerospace-and-other-businesses"></a>Technická příručka k šabloně Cortana Intelligence řešení za účelem prediktivní údržby šestinásobné a jiných firmy

## <a name="acknowledgements"></a>**Potvrzení**
Tento článek je dílem dat vědeckých Jan Potokar Gauher Shaheen, Fidan Boylu Uz a software pracovníkům Dan Grecoe společnosti Microsoft.

## <a name="overview"></a>**Základní informace**

Řešení šablony jsou určeny pro urychlení proces vytváření E2E ukázku nad Cortana Intelligence sadu. Nasazení šablony zřízení předplatného s Cortana Intelligence přehrávaných a vytvoření vztahů mezi nimi. Také osazuje kanálu dat s ukázkovými daty generované aplikaci generátor data, která bude stáhnout a nainstalovat na místním počítači po nasazení řešení šablony. Data generované generátor bude hydrate dat kanálem k odesílání zpráv a začněte vytvářet předpovědí výukové stroje, které můžete vizualizovat na řídicím panelu Power BI. Procesu nasazení vás provede několik pokynů k pilotnímu přihlašovacích údajů řešení. Zkontrolujte, že záznam těchto přihlašovacích údajů například řešení jméno, uživatelské jméno a heslo zadané při nasazení.  

Cílem tento dokument je popisují architektura odkaz a jednotlivých složek zřízení ve vašem předplatném jako součást této šablony řešení. Dokument taky o tom, jak nahraďte vzorová data s reálným daty vlastní neuvidíte přehledy a předpovědí na vlastních datech mluví. Navíc dokument popisuje díly šabloně řešení, která by být nutné upravit Kdybyste chtěli přizpůsobení řešení s vlastními daty. Pokyny k vytváření řídicího panelu Power BI pro tuto šablonu řešení jsou k dispozici na konci.

>[AZURE.TIP] Můžete stáhnout a vytisknout [verze PDF v tomto dokumentu](http://download.microsoft.com/download/F/4/D/F4D7D208-D080-42ED-8813-6030D23329E9/cortana-analytics-technical-guide-predictive-maintenance.pdf).

## <a name="big-picture"></a>**Celkového rámce**

![](media/cortana-analytics-technical-guide-predictive-maintenance\predictive-maintenance-architecture.png)

Při nasazení řešení jsou aktivní různé služby Azure v rámci sady analýzy Cortana (*tedy* události centrální toku analýzy HDInsight, Data Factory, výukové počítače, *atd.*). Výše uvedené architektura diagram znázorňuje na vysoké úrovni, jak je vytvořen prediktivní údržbu Aerospace šablona řešení od začátku do konce. Bude moct prozkoumat tyto služby azure portálu klepnutím na ně v diagramu šablony řešení vytvořená pomocí nasazení řešení s výjimkou HDInsight jako tuto službu máte k dispozici jako služba při aktivity související kanálu podporují spouštět a odstraněné později.
Stáhněte si v [plné velikosti verzi diagramu](http://download.microsoft.com/download/1/9/B/19B815F0-D1B0-4F67-AED3-A40544225FD1/ca-topologies-maintenance-prediction.png).

V následujících částech jsou popsány každý úsek.

## <a name="data-source-and-ingestion"></a>**Zdroje dat a přijímání**

### <a name="synthetic-data-source"></a>Zdroj syntetické dat

Pro tuto šablonu vygeneruje se z desktopové aplikace, který bude stáhnout a spustit místně po úspěšné zavedení zdroj dat použít. Najdete pokyny ke stažení a instalaci této aplikace v pruhu vlastnosti po výběru prvního uzel s názvem prediktivní generátor správy dat v diagramu šablony řešení. Tato aplikace informační kanály služby [Azure události centrální](#azure-event-hub) datových bodů nebo událostí, které se použije ve zbývající části tok řešení. Tento zdroj dat se skládá ze nebo odvozeno z veřejně dostupný data z [úložiště NASA dat](http://ti.arc.nasa.gov/tech/dash/pcoe/prognostic-data-repository/) pomocí [Engine Turbofan snížení úrovně uvedenou množinu dat simulace](http://ti.arc.nasa.gov/tech/dash/pcoe/prognostic-data-repository/#turbofan).

Generování aplikace událost naplní centrální události Azure pouze během spouštění v počítači.

### <a name="azure-event-hub"></a>Centrální Azure události

Služba [Azure události centrální](https://azure.microsoft.com/services/event-hubs/) je příjemce zadání od syntetické zdroje dat jsme je popsali výše.

## <a name="data-preparation-and-analysis"></a>**Příprava dat a analýzy**


### <a name="azure-stream-analytics"></a>Azure toku analýzy

Služby [Azure toku analýzy](https://azure.microsoft.com/services/stream-analytics/) slouží k poskytnutí poblíž v reálném čase analýzy na vstupní proudu služby [Azure události centrální](#azure-event-hub) a publikovat výsledků do řídicího panelu [Power BI](https://powerbi.microsoft.com) a také archivace všech nezpracovanými příchozí událostí pro službu [Azure úložiště](https://azure.microsoft.com/services/storage/) pro pozdější zpracování službou [Azure Data Factory](https://azure.microsoft.com/documentation/services/data-factory/) .

### <a name="hd-insights-custom-aggregation"></a>Vlastní agregace HD přehledy

Služba Azure HD přehled slouží k spouštěly skripty [podregistru](http://blogs.msdn.com/b/bigdatasupport/archive/2013/11/11/get-started-with-hive-on-hdinsight.aspx) (orchestrated tak, že Azure Data Factory) dát agregace vazbu nezpracovanými události, které byly archivovány pomocí služby Azure toku analýzy.

### <a name="azure-machine-learning"></a>Výukové Azure počítače

Použití služby [Azure počítače výukové](https://azure.microsoft.com/services/machine-learning/) (orchestrated tak, že Azure Data Factory) zvětšit předpovědí na zbývající životnosti (RUL) modul konkrétní letadla zadaných vstupů dostali.

## <a name="data-publishing"></a>**Publikování dat**


### <a name="azure-sql-database-service"></a>Azure SQL databáze služby

[Databáze SQL Azure](https://azure.microsoft.com/services/sql-database/) použití služby pro ukládání (spravuje Azure Data Factory) předpovědí službou Azure počítače výukové, které bude potřeba v řídicím panelu [Power BI](https://powerbi.microsoft.com) .

## <a name="data-consumption"></a>**Spotřeba dat**

### <a name="power-bi"></a>Power BI

Služba [Power BI](https://powerbi.microsoft.com) je použité ke znázornění řídicího panelu, který obsahuje agregace a upozornění poskytovanou služby [Azure toku analýzy](https://azure.microsoft.com/services/stream-analytics/) , jakož i RUL předpovědí uložených v [Databázi SQL Azure](https://azure.microsoft.com/services/sql-database/) , které byly vytvořené pomocí služby [Azure počítače výukové](https://azure.microsoft.com/services/machine-learning/) . Pokyny k vytváření řídicího panelu Power BI pro tuto šablonu řešení naleznete v části.

## <a name="how-to-bring-in-your-own-data"></a>**Jak zkrácení vlastními daty**

Tato část popisuje, jak přenesení vlastních dat Azure a oblasti, které vyžadují změny pro data, která přenést do této architektury.

Není pravděpodobné, že všechny datovou sadu, kterou můžete přenést by odpovídalo datovou sadu používaný [Engine Turbofan snížení úrovně uvedenou množinu dat simulace](http://ti.arc.nasa.gov/tech/dash/pcoe/prognostic-data-repository/#turbofan) použitá pro tuto šablonu řešení. Principy vaše data a požadavky budou důležité v tom, jak změnit tuto šablonu pro práci s vlastními daty. Pokud je první vystavení služby Azure počítače výukové, můžete získat Úvod k němu pomocí příkladu v [tom, jak vytvořit svůj první testu](machine-learning-create-experiment.md).

Následující části popisují, v částech šablony, která vyžaduje úpravy po nové datové sady.

### <a name="azure-event-hub"></a>Centrální Azure události

Služby Azure události centrální je velmi obecný tak, aby se data můžete publikované na centrální ve formátu CSV nebo JSON. Žádné zvláštní zpracování probíhá v centru Azure událost, ale je důležité pochopit data, která je vloženo do ho.

V tomto dokumentu není popisují jedí dat, ale můžete snadno odeslat události nebo dat k rozbočovači události Azure pomocí rozhraní API centrální události.

### <a name="azure-stream-analytics"></a>Azure toku analýzy

Služba Azure toku Analytics slouží k poskytování poblíž v reálném čase analýzy podle tématu z datových proudů a výstupu dat na libovolný počet zdrojů.

Prediktivní údržbu Aerospace řešení šablony dotazu toku analýzy Azure tvořen čtyři dílčí dotazy, každý náročný události ze služby Azure události centrální a výstupy museli čtyři různých míst. Tyto výstupy tvořeny tři datové sady Power BI a jednoho umístění v úložišti Azure.

Dotaz Azure toku analýzy najdete tak, že:

-   Přihlášení k portálu Azure

-   Vyhledání úlohy analýzy toku ![](media\cortana-analytics-technical-guide-predictive-maintenance\icon-stream-analytics.png) , které byly vytvořeny při nasazení řešení (*například* **maintenancesa02asapbi** a **maintenancesa02asablob** řešení prediktivní údržby)

-   Výběr

    -   ***Zadávání*** zobrazíte vstup dotazu

    -   ***Dotaz*** můžete zobrazit samotný dotaz

    -   Chcete-li zobrazit různé výstupy ***výstup***

Informace o vytváření analýz toku Azure dotazu najdete v [Toku analýzy dotazu odkaz](https://msdn.microsoft.com/library/azure/dn834998.aspx) na webu MSDN.

V tomto řešení výstupní dotazy tři datové sady s poblíž v reálném čase analýzy informace o příchozí toku dat pro Power BI řídicí panel, který slouží jako součást této šablony řešení. Protože implicitní znalost příchozí formát dat ve sloupcích se tyto dotazy by bylo nutné změnit podle svého formát dat ve sloupcích.

Dotaz ve druhém **maintenancesa02asablob** úlohy analýzy toku jednoduše výstup všech událostí [Události centrální](https://azure.microsoft.com/services/event-hubs/) k [Základnímu úložišti Azure](https://azure.microsoft.com/services/storage/) a tedy vyžaduje žádné změny bez ohledu na formát dat jako informace úplné události se streamují k základnímu úložišti.

### <a name="azure-data-factory"></a>Azure Data Factory

Služby [Azure Data Factory](https://azure.microsoft.com/documentation/services/data-factory/) orchestrates pohybu a zpracování údajů. Prognostické udržování Aerospace šablona řešení factory dat tvoří tři [kanály](../data-factory/data-factory-create-pipelines.md) , přesunutí a zpracování dat pomocí různých technologií.  Otevřením se dostanete factory dat uzel Data Factory v dolní části diagramu šablony řešení vytvořená pomocí nasazení řešení. Tím přejdete do továrny dat na portálu Azure. Pokud se zobrazí pod vaší datové sady chyby, můžete můžou být ignorovat jsou kvůli dat factory nasazena před spuštěním generátor dat. Tyto chyby nezabrání dat factory fungovat.

![](media/cortana-analytics-technical-guide-predictive-maintenance/data-factory-dataset-error.png)

Tato část popisuje potřebné [kanály](../data-factory/data-factory-create-pipelines.md) a [aktivity](../data-factory/data-factory-create-pipelines.md) obsažené v [Azure Data Factory](https://azure.microsoft.com/documentation/services/data-factory/). Níže je zobrazení diagramu řešení.

![](media/cortana-analytics-technical-guide-predictive-maintenance/azure-data-factory.png)

Dvě potrubí tento factory obsahovat skripty [podregistru](http://blogs.msdn.com/b/bigdatasupport/archive/2013/11/11/get-started-with-hive-on-hdinsight.aspx) , které se používají k oddílů a agregovat data. Když uvedeno, tyto skripty najdete v [Úložišti Azure](https://azure.microsoft.com/services/storage/) účet vytvořený během instalace. Bude jejich umístění: maintenancesascript\\\\skript\\\\podregistru\\ \\ (nebo https://[Your řešení name].blob.core.windows.net/maintenancesascript).

Podobně jako [Azure toku analýzy](#azure-stream-analytics-1) dotazů skripty [podregistru](http://blogs.msdn.com/b/bigdatasupport/archive/2013/11/11/get-started-with-hive-on-hdinsight.aspx) znají implicitní o příchozí formát dat, tyto dotazy by potřeba změnit podle vašim požadavkům pro formátování a [funkce inženýrské](machine-learning-feature-selection-and-engineering.md) dat.

#### <a name="aggregateflightinfopipeline"></a>*AggregateFlightInfoPipeline*

Tento [kanálem k odesílání zpráv](../data-factory/data-factory-create-pipelines.md) obsahuje jednu aktivitu - [HDInsightHive](../data-factory/data-factory-hive-activity.md) aktivity pomocí [HDInsightLinkedService](https://msdn.microsoft.com/library/azure/dn893526.aspx) spouštějící skript [podregistru](http://blogs.msdn.com/b/bigdatasupport/archive/2013/11/11/get-started-with-hive-on-hdinsight.aspx) do oddílů umístění v [Úložišti Azure](https://azure.microsoft.com/services/storage/) během úlohy [Azure toku analýzy](https://azure.microsoft.com/services/stream-analytics/) dat.

Skript [podregistru](http://blogs.msdn.com/b/bigdatasupport/archive/2013/11/11/get-started-with-hive-on-hdinsight.aspx) rozdělení úkolu je ***AggregateFlightInfo.hql***

#### <a name="mlscoringpipeline"></a>*MLScoringPipeline*

Tento [kanálem k odesílání zpráv](../data-factory/data-factory-create-pipelines.md) obsahuje několik aktivity a jehož konečný výsledek, že scored předpovědí z [Azure počítače výukové](https://azure.microsoft.com/services/machine-learning/) experimentovat souvisí s touto šablonou řešení.

Tyhle aktivity obsažené v takto:

-   Aktivity [HDInsightHive](../data-factory/data-factory-hive-activity.md) pomocí [HDInsightLinkedService](https://msdn.microsoft.com/library/azure/dn893526.aspx) spouštějící [podregistru](http://blogs.msdn.com/b/bigdatasupport/archive/2013/11/11/get-started-with-hive-on-hdinsight.aspx) skript pro zajištění agregace a funkce potřebné pro experiment [Výukové počítače Azure](https://azure.microsoft.com/services/machine-learning/) inženýrské funkce
    Skript [podregistru](http://blogs.msdn.com/b/bigdatasupport/archive/2013/11/11/get-started-with-hive-on-hdinsight.aspx) rozdělení úkolu je ***PrepareMLInput.hql***.

-   [Kopírovat](https://msdn.microsoft.com/library/azure/dn835035.aspx) aktivity na počítači se přesune výsledky z aktivity [HDInsightHive](../data-factory/data-factory-hive-activity.md) do jednoho objektů blob [Azure úložiště](https://azure.microsoft.com/services/storage/) , kterou lze přístup aktivita [AzureMLBatchScoring](https://msdn.microsoft.com/library/azure/dn894009.aspx) .

-   [AzureMLBatchScoring](https://msdn.microsoft.com/library/azure/dn894009.aspx) aktivity, která volá [Azure počítače výukové](https://azure.microsoft.com/services/machine-learning/) experimentovat, jehož výsledkem výsledky umístit do jedné objektů blob [Azure úložiště](https://azure.microsoft.com/services/storage/) .

#### <a name="copyscoredresultpipeline"></a>*CopyScoredResultPipeline*

Tento [kanálem k odesílání zpráv](../data-factory/data-factory-create-pipelines.md) obsahuje jedné aktivity - aktivitu [Kopírovat](https://msdn.microsoft.com/library/azure/dn835035.aspx) , která přesouvá že výsledky [Azure počítače výukové](#azure-machine-learning) experimentovat z ***MLScoringPipeline*** k [Databázi SQL Azure](https://azure.microsoft.com/services/sql-database/) , protože ji někdo zřídil jako součást šablony instalace řešení.

### <a name="azure-machine-learning"></a>Výukové Azure počítače

Experiment [Azure počítače výukové](https://azure.microsoft.com/services/machine-learning/) použít pro tuto šablonu řešení poskytuje zbývající užitečné životního (RUL) stroje letadla. Testu specifické pro uvedenou množinu dat spotřebované množství a proto bude vyžadovat změnu nebo nahrazení specifické pro data, která je uvedena v.

Informace o vytvoření testu Azure počítače výukové najdete v tématu [prediktivní údržbu: kroku 1 ze 3, Příprava dat a inženýrské funkce](http://gallery.cortanaanalytics.com/Experiment/Predictive-Maintenance-Step-1-of-3-data-preparation-and-feature-engineering-2).

## <a name="monitor-progress"></a>**Sledování průběhu**
 Po spuštění generátor dat kanálu začne získat HYDRATOVANÝ a jednotlivých součástí vašeho řešení začněte kicking do akce následující příkazy vydané Data Factory. Existují dva způsoby můžete sledovat kanálu.

1. Jednu úlohy toku analýzy nezpracovaná příchozí data zapisuje k úložišti objektů blob. Když kliknete na úložiště objektů Blob součástí vašeho řešení na obrazovce se úspěšně nasazení řešení a klikněte na Otevřít v pravém panelu, přejdete na [portálu Správa](https://portal.azure.com/). Jednou, klikněte na tlačítko na objekty BLOB. V dalším panelu zobrazí se seznam kontejnery. Klikněte na **maintenancesadata**. Na panelu Další uvidíte složku **prvotní_data** . Ve složce prvotní_data uvidíte složky s názvy jako hodinu = 17 hodinu = 18 atd. Pokud se zobrazí u těchto složek, to znamená, že nezpracovanými daty je úspěšně vytvářejí ve vašem počítači a uložené v úložišti objektů blob. Měli byste vidět csv souborů, které by měla být omezená velikosti v Megabajtech těchto složek.

2. Posledním krokem kanálu je k zápisu dat (například předpovědí z počítače výukové) do SQL databáze. Může projevit až do velikosti tři hodiny pro data umístěná v databázi SQL. Je možné sledovat množství zpracovávaných dat je k dispozici v databázi SQL [azure](https://manage.windowsazure.com/)portálu. V levém panelu vyhledejte SQL databáze ![](media\cortana-analytics-technical-guide-predictive-maintenance\icon-SQL-databases.png) a klikněte na něj. Pak najděte databázi **pmaintenancedb** a klikněte na ni. Na další stránce dole klikněte na SPRAVOVAT

    ![](media\cortana-analytics-technical-guide-predictive-maintenance\icon-manage.png).

    Tady můžete kliknout na nový dotaz a dotaz pro počet řádků (například vybrat count(*) z PMResult). Narůstající velikostí databáze, měli zvýšit počet řádků v tabulce.


## <a name="power-bi-dashboard"></a>**Řídicího panelu Power BI**

### <a name="overview"></a>Základní informace

Tato část popisuje jak nastavit řídicího panelu Power BI vizualizovat data reálného času z Azure toku analýzy (kritická cesta), i dávku předpovědí výsledky z Azure počítače výuky (studenou cesta).

### <a name="setup-cold-path-dashboard"></a>Nastavení studenou cestu řídicího panelu

V kanálu dat studenou cestu základní cílem je získat prediktivní RUL (zbývající životnost) každý modul letadla po dokončení letu (obrázku). Výsledek předpovídání pomocí se aktualizuje každé 3 hodiny pro odhad letadla moduly, které jste dokončili let během posledních 3 hodiny.

Power BI se připojí k databázi Azure SQL jako zdroj dat, kde jsou uloženy výsledky předpovědí. Poznámka: 1) při nasazení řešení, skutečné předpověď se zobrazí v databázi v rámci 3 hodiny.
Soubor pbix dodaného s stahovat generátor obsahuje několik počáteční data tak, aby při vytváření řídicího panelu Power BI hned. 2) v tomto kroku je předpoklad si stáhněte a nainstalujte bezplatný software [Power BI desktop](https://powerbi.microsoft.com/documentation/powerbi-desktop-get-the-desktop/).

Podle těchto kroků průvodce vás o tom, jak připojit soubor pbix k SQL databázi, která byla nespředený nahoru v době nasazení řešení obsahující data (*například*. výsledky předpovědí) pro vizualizaci.

1.  Pokud potřebujete přihlašovací údaje pro databázi.

    Musíte mít **název databázového serveru, jméno, uživatelské jméno a heslo databáze** teprve pak přejděte k dalším krokům. Tady najdete postup, jak vás jak je najít.

    -   Po **"databáze SQL Azure"** v diagramu šablony řešení změní zelené, klikněte na ni a klikněte na tlačítko **Otevřít"**.

    -   Zobrazí se nové kartu nebo okno prohlížeče zobrazující Azure stránku portálu. Klikněte na **zdroje skupiny** v levém panelu.

    -   Vyberte předplatné, kterou používáte pro nasazení řešení a pak vyberte **"YourSolutionName\_ResourceGroup"**.

    -   V nové pop se panely, klepněte ![](media\cortana-analytics-technical-guide-predictive-maintenance\icon-sql.png) ikonu získat přístup k databázi. Název databáze je vedle tohoto ikonu (*například*, **"pmaintenancedb"**) a **název databázového serveru** je uvedená v části vlastnosti název serveru a by měla vypadat podobně jako **YourSoutionName.database.windows.net**.

    -   Databáze **uživatelské jméno** a **heslo** jsou stejné jako uživatelské jméno a heslo zaznamenaných během nasazení řešení.

2.  Aktualizace zdroje dat souboru studenou cestu zprávy s Power BI Desktop.

    -   Ve složce na počítači, kde jste si stáhli a unzipped souboru generátor, poklikejte **PowerBI\\PredictiveMaintenanceAerospace.pbix** soubor. Pokud se zobrazí všechny zprávy upozornění při otevření souboru, je ignorujte. Nahoře na souboru klikněte na **Upravit dotazů**.

        ![](media\cortana-analytics-technical-guide-predictive-maintenance\edit-queries.png)

    -   Zobrazí se dvě tabulky, **RemainingUsefulLife** a **PMResult**. Vyberte první tabulky a klikněte na ![](media\cortana-analytics-technical-guide-predictive-maintenance\icon-query-settings.png) vedle **"Zdrojový"** v části **Použitý postup** na pravém panelu **Dotazu nastavení** . Ignorujte všechny zprávy upozornění, které se zobrazí.

    -   V pop se okno **"Server"** a **"Databáze"** nahraďte vlastní název serveru a databáze a klikněte na tlačítko **"OK"**. Název serveru zkontrolujte, že zadáte port 1433 (**YourSoutionName.database.windows.net 1433**). Nechte pole databáze jako **pmaintenancedb**. Ignorujte upozornění, které se zobrazí na obrazovce.

    -   V dalším pop se okno zobrazí se dvě možnosti v levém podokně (**Windows** a **databáze**). Klikněte na **"Databáze"**, zadejte **"Uživatelské jméno"** a **"Heslo"** (Toto je uživatelské jméno a heslo, které jste zadali při prvním nasazení řešení a vytvoření databáze Azure SQL). V části ***Vybrat stupeň použít toto nastavení***zaškrtněte možnost úrovně databáze. Klepněte na tlačítko **"Připojit"**.

    -   Klepnutím na druhou tabulku **PMResult** ![](media\cortana-analytics-technical-guide-predictive-maintenance\icon-navigation.png) vedle **"Zdrojový"** v části **Použitý postup** na pravém panelu **Dotazu nastavení** aktualizace název serveru a databáze jako výše uvedené kroky a potom klikněte na OK.

    -   Až budete s asistencí zpátky na předchozí stránku, zavřete okno. Objeví se zpráva out – klikněte na **použít**. Nakonec klikněte na tlačítko **Uložit** uložte změny. Soubor Power BI teď vytvořila připojení k serveru. Pokud vaše vizualizace jsou prázdné, zkontrolujte, jestli že zrušíte na výběr vizualizace vizualizovat všechna data kliknutím na ikonu Guma v pravém horním rohu legendy. Pomocí tlačítka Aktualizovat, aby obsahoval nová data v vizualizace. Původně uvidíte pouze počáteční data vizualizaci nejdříve data factory aktualizovat každé 3 hodiny. Po 3 hodiny zobrazí se nové předpovědí projeví ve vizualizaci když aktualizujete data.

3.  (Volitelné) Publikujte na řídicím panelu studenou cesta k [Power BI online](http://www.powerbi.com/). Všimněte si, že tento krok vyžaduje účet Power BI (nebo účet Office 365).

    -   Klikněte na **"Publikovat"** a zobrazí se okno několik sekund, než později zobrazení "Publikování na webu Power BI úspěch!" s zelená značka zaškrtnutí. Klikněte na odkaz "Otevřít PredictiveMaintenanceAerospace.pbix v Power BI". Podrobné pokyny najdete v části [Publikovat na Power BI Desktop](https://support.powerbi.com/knowledgebase/articles/461278-publish-from-power-bi-desktop).

    -   Při vytváření řídicího nového: klikněte **+** znaménko vedle části **řídicího panelu** v levém podokně. Zadejte název "Prediktivní údržbu ukázku" pro tento nový řídicí panel.

    -   Po otevření sestavy klikněte na tlačítko ![](media\cortana-analytics-technical-guide-predictive-maintenance\icon-pin.png) Připnutí všechny vizualizace do řídicího panelu. Podrobné pokyny najdete v tématu [Připnout dlaždici řídicího panelu na Power BI ze sestavy](https://support.powerbi.com/knowledgebase/articles/430323-pin-a-tile-to-a-power-bi-dashboard-from-a-report).
    Přejděte na stránku řídicího panelu a upravte velikost a umístění vizualizaci a upravovat jejich názvů. Podrobné informace o tom, jak upravit dlaždic najdete v části [Upravit dlaždici – změnit velikost, přesunout, přejmenovat, kód pin, odstranit, a přidat hypertextový odkaz](https://powerbi.microsoft.com/documentation/powerbi-service-edit-a-tile-in-a-dashboard/#rename). Tady je příklad řídicího panelu s vizualizacemi některé studenou cestu připnuté k němu.  Podle toho, jak dlouho spuštění generátor dat se může lišit čísel na vizualizace.
    <br/>
    ![](media\cortana-analytics-technical-guide-predictive-maintenance\final-view.png)
<br/>
    -   K naplánování aktualizace dat, umístěte ukazatel myši nad **PredictiveMaintenanceAerospace** datovou sadu, klikněte na tlačítko ![](media\cortana-analytics-technical-guide-predictive-maintenance\icon-elipsis.png) a klikněte na **Naplánovat aktualizaci**.
<br/>
        **Poznámka:** Pokud se zobrazí upozornění masáž, klikněte na **Upravit pověření** a ujistěte se, vaše přihlašovací údaje pro databázi jsou popsané v kroku 1.
<br/>
    ![](media\cortana-analytics-technical-guide-predictive-maintenance\schedule-refresh.png)
<br/>
    -   Rozbalení oddílu **Naplánovat aktualizaci** . Zapněte možnost "pravidelná aktualizace dat".
    <br/>
    -   Naplánování aktualizace podle vašich potřeb. Další informace najdete v tématu [aktualizace dat v Power BI](https://support.powerbi.com/knowledgebase/articles/474669-data-refresh-in-power-bi).

### <a name="setup-hot-path-dashboard"></a>Nastavení kritická cesta řídicího panelu

Podle těchto kroků průvodce vás jak vizualizovat data výstup reálném čase z toku analýzy úloh, které byly vytvořeny v době nasazení řešení. [Power BI online](http://www.powerbi.com/) účet je potřeba k provedení následujících kroků. Pokud nemáte účet, můžete [ho vytvořit](https://powerbi.microsoft.com/pricing).

1.  Přidejte výstupní Power BI v Azure toku analýzy (ASA).

    -  Budete muset postupujte podle pokynů uvedených v článku [Azure toku technologie pro analýzu a Power BI: řídicího panelu v reálném čase analytics pro přenos dat v reálném čase viditelnost](stream-analytics-power-bi-dashboard.md) nastavit výstup Azure toku analýzy práce jako řídicího panelu Power BI.
    - ASA dotaz má tři výstupy, které jsou **aircraftmonitor** **aircraftalert**a **flightsbyhour**. Dotaz můžete zobrazit kliknutím na kartu dotazu. Odpovídající každá z těchto tabulek, musíte přidat výstup ASA. Po přidání prvního výstup (*například* **aircraftmonitor**) zkontrolujte, že **Alias výstup**, **Název sady dat** a **Název tabulky** jsou stejné (**aircraftmonitor**). Opakujte postup pro přidání výstupy **aircraftalert**a **flightsbyhour**. Po přidání všechny tři výstup tabulek a spuštění úlohy ASA by měla získat zprávu s potvrzením (*například*, "Počáteční toku analýzy úlohy maintenancesa02asapbi úspěšně").

2. Přihlaste se k [Power BI online](http://www.powerbi.com)

    -   V levém podokně část datové sady v Moje pracovního prostoru ***datovou sadu*** názvů **aircraftmonitor** **aircraftalert**a **flightsbyhour** by se měly. Toto je streamování data, která posune z Azure toku analýzy v předchozím kroku. Datovou sadu **flightsbyhour** nemusí neprojevila ve stejnou dobu jako ostatní dvě datové sady kvůli přírodní dotaz SQL zobrazený pod ní. Však se má zobrazit za hodinu.
    -   Zkontrolujte, v podokně ***vizualizace*** je otevřít a se zobrazují na pravé straně obrazovky.

3. Až budete mít toku do Power BI dat, můžete začít vizualizace streamování data. Níže je příklad řídicího panelu s vizualizacemi některé kritická cesta Připne ho. Můžete vytvořit další dlaždice řídicího panelu podle potřeby datové sady. Podle toho, jak dlouho spuštění generátor dat se může lišit čísel na vizualizace.


    ![](media\cortana-analytics-technical-guide-predictive-maintenance\dashboard-view.png)

4. Tady je několik kroků k vytvoření jednoho z výše uvedených dlaždice – dlaždice "loďstva zobrazení z senzor 11 porovnání mezní 48.26":

    -   Klikněte na datovou sadu **aircraftmonitor** v levém panelu oddíl datové sady.

    -   Klikněte na ikonu **Spojnicový graf** .

    -   Aby se zobrazil pod zaškrtávacím políčkem "Osa" v podokně **vizualizace** , klikněte v podokně **pole** na **zpracovávané** .

    -   Klikněte na "s.11" a "s.11\_upozornění" tak, aby oba se v části "Hodnoty". Kliknutím na malou šipku vedle **s.11** a **s.11\_upozornění**, změňte "Součet" na "Průměru".

    -   Nahoře na tlačítko **Uložit** a pojmenujte sestavu "aircraftmonitor". Sestavy s názvem "aircraftmonitor" zobrazí v části **sestavy** v **navigačním** podokně na levé straně.

    -   Klepněte na ikonu **Připínáčku vizuální** v pravém horním rohu tento spojnicový graf. Okno "Připnout do řídicího panelu" může zobrazit umožňující zvolit řídicího panelu. Vyberte "Prediktivní údržbu ukázku" a potom klikněte na "Připnout".

    -   Ukazatel myši této dlaždici na řídicím panelu, klikněte na ikonu "úpravy" v pravém horním rohu můžete změnit jeho název "Loďstva zobrazení z senzor 11 a prahové hodnoty 48.26" a podnadpis "Průměru přes loďstva v čase".

## <a name="how-to-delete-your-solution"></a>**Jak odstranit řešení**
Ověřte, zda nástroj Generátor dat při použití nejsou aktivně řešení, jak systém generátor dat bude vynakládá vyšší. Pokud nepoužíváte ji odstraňte řešení. Odstranění řešení budou odstraněny všechny komponenty zřízení ve vašem předplatném při nasazení řešení. Chcete-li odstranit řešení klikněte na název vašeho řešení v levém panelu šablona řešení a klepněte na odstranit.

## <a name="cost-estimation-tools"></a>**Odhad nákladů nástroje**

Jsou dostupné vám lepší přehled o celkové náklady spojené s spuštěna prediktivní údržbu Aerospace šablona řešení ve vašem předplatném následující dva nástroje:

-   [Nástroj odhad nákladů Microsoft Azure (online)](https://azure.microsoft.com/pricing/calculator/)

-   [Microsoft Azure náklady odhad nástroj (desktopová aplikace)](http://www.microsoft.com/download/details.aspx?id=43376)
