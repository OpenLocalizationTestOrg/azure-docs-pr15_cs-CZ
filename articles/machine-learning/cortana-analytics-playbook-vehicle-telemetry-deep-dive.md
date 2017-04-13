<properties 
    pageTitle="Vozidla telemetrie analýzy řešení playbook: hloubkové postupy do řešení | Microsoft Azure" 
    description="Použití funkcí Cortana Intelligence získat v reálném čase a prediktivní Další informace o stavu vozidla a řízení zvyky." 
    services="machine-learning" 
    documentationCenter="" 
    authors="bradsev" 
    manager="jhubbard" 
    editor="cgronlun" />

<tags 
    ms.service="machine-learning" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/12/2016" 
    ms.author="bradsev" />


# <a name="vehicle-telemetry-analytics-solution-playbook-deep-dive-into-the-solution"></a>Vozidla telemetrie analýzy řešení playbook: hloubkové postupy do řešení

Tento odkazů v **nabídce** k částem tento playbook: 

[AZURE.INCLUDE [cap-vehicle-telemetry-playbook-selector](../../includes/cap-vehicle-telemetry-playbook-selector.md)]

Tento oddíl cvičení dolů do všech fází je vidět v architektury řešení s pokyny a tipů k úpravám. 

## <a name="data-sources"></a>Zdroje dat

Řešení je definována dvou různých zdrojů dat:

- **simulovaný vozidla signály a diagnostiky datovou sadu** a 
- **katalog vozidla**

Simulator telematika vozidla je součástí tohoto řešení. Posílá diagnostické informace a signalizuje odpovídající stav vozidla a řízení vzorek v daném okamžiku v čase. Klikněte na [Vozidla telematika Simulator](http://go.microsoft.com/fwlink/?LinkId=717075) ke stažení **Vozidla telematika Simulator Visual Studio řešení** pro přizpůsobení svým požadavkům. Katalog vozidla obsahuje datovou sadu odkaz s VIN modelu mapování.

![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig2-vehicle-telematics-simulator.png)

*Obrázek 2 – Simulator telematika vozidla*

Toto je datovou sadu ve formátu JSON, která obsahuje následující schéma.

Sloupec | Popis | Hodnoty 
 ------- | ----------- | --------- 
VIN | Náhodně generovaným číslem identifikace vozidla | To je získané hlavní seznam 10 000 náhodně generovaným vozidla identifikačních čísel.
Vnější teplotní | Venkovní teploty kde dohání k vozidla | Náhodně generovaným číslem od 0 až 100
Modul teplotní | Modul teplotní vozidla | Náhodně generovaným číslem od 0 500
Rychlost | Rychlost engine niž dohání k vozidla | Náhodně generovaným číslem od 0 až 100
Paliva | Úroveň paliva vozidla | Náhodně generovaným číslem od 0 až 100 (označuje paliva úrovně procenta)
EngineOil | Úroveň oil stroje vozidla | Náhodně generovaným číslem od 0 až 100 (označuje engine oil úrovně procenta)
Můžete zadat tlaku | Můžete zadat tlaku vozidla | Náhodně generované číslo od 0-50 (označuje plášť tlaku úrovně procenta)
Ujeté vzdálenosti | Čtení ujeté vzdálenosti vozidla | Náhodně generovaným číslem od 0 200000
Accelerator_pedal_position | Pozice přístupové Pedálové vozidla | Náhodně generovaným číslem od 0 až 100 (označuje přístupové úrovně procenta)
Parking_brake_status | Označuje, zda je vozidla parkování nebo ne | True nebo False
Headlamp_status | Označuje, kde světlometu na nebo ne | True nebo False
Brake_pedal_status | Označuje, zda pedálu brzdy stisknutí nebo ne | True nebo False
Transmission_gear_position | Pozice přenosu ozubeného kola vozidla | Státy: nejdřív druhá, třetí a čtvrté, šestého, sedmým, osmého
Ignition_status | Označuje, jestli je spuštěný nebo přestal vozidla | True nebo False
Windshield_wiper_status | Označuje, jestli je stírání windshield zapnuté nebo ne | True nebo False
ABS | Označuje, zda ABS provozuje nebo ne | True nebo False
Časové razítko | Časové razítko při vytvoření datového bodu | Datum
Město | Umístění vozidla | 4 měst v tomto řešení: Pardubicích Redmond, Sammamish, Seattlu


Datovou sadu vozidla modelu odkaz obsahuje VIN mapování modelu. 

VIN | Model |
--------------|------------------
FHL3O1SA4IEHB4WU1 | Limuzíny |
8J0U8XCPRGW4Z3NQE | Hybridní |
WORG68Z2PLTNZDBI7 | Rodinný Sedan |
JTHMYHQTEPP4WBMRN | Limuzíny |
W9FTHG27LZN1YWO0Y | Hybridní |
MHTP9N792PHK08WJM | Rodinný Sedan |
EI4QXI2AXVQQING4I | Limuzíny |
5KKR2VB4WHQH97PF8 | Hybridní |
W9NSZ423XZHAONYXB | Rodinný Sedan |
26WJSGHX4MA5ROHNL | Lze převést |
GHLUB6ONKMOSI7E77 | Kombi |
9C2RHVRVLMEJDBXLP | Kompaktní auta |
BRNHVMZOUJ6EOCP32 | Malé SUV |
VCYVW0WUZNBTM594J | Sportovní Auto |
HNVCE6YFZSA5M82NY | Střední SUV |
4R30FOR7NUOBL05GJ | Kombi |
WYNIIY42VKV6OQS1J | Velké SUV |
8Y5QKG27QET1RBK7I | Velké SUV |
DF6OX2WSRA6511BVG | Kupé |
Z2EOZWZBXAEW3E60T | Limuzíny |
M4TV6IEALD5QDS3IR | Hybridní |
VHRA1Y2TGTA84F00H | Rodinný Sedan |
R0JAUHT1L1R3BIKI0 | Limuzíny |
9230C202Z60XX84AU | Hybridní |
T8DNDN5UDCWL7M72H | Rodinný Sedan |
4WPYRUZII5YV7YA42 | Limuzíny |
D1ZVY26UV2BFGHZNO | Hybridní |
XUF99EW9OIQOMV7Q7 | Rodinný Sedan
8OMCL3LGI7XNCC21U | Lze převést |
…….  |   |


### <a name="to-generate-simulated-data"></a>Při generování simulovaný dat
1.  Pokud si Pokud chcete stáhnout balíček simulator dat, klikněte na šipku v pravém horním rohu na uzel vozidla telematika Simulator. Uložte a extrahujte soubory místně ve vašem počítači. ![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig3-vehicle-telemetry-blueprint.png)*Obrázek 3 – vozidla Telemetrie analýzy řešení přehled*

2.  V místním počítači přejděte do složky, které jste extrahovali balíček vozidla telematika Simulator. ![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig4-vehicle-telematics-simulator-folder.png)*Obrázek 4 – vozidla telematika Simulator složky*

3.  Spusťte aplikaci **CarEventGenerator.exe**.

### <a name="references"></a>Odkazy

[Řešení vozidla telematika Simulator Visual Studio](http://go.microsoft.com/fwlink/?LinkId=717075) 

[Centrální Azure události](https://azure.microsoft.com/services/event-hubs/)

[Azure Data Factory](https://azure.microsoft.com/documentation/learning-paths/data-factory/)


## <a name="ingestion"></a>Požití
Kombinace rozbočovače události Azure, toku technologie pro analýzu a Data Factory jsou využít k jedí vozidla signály diagnostických událostí a v reálném čase a dávkové analýzy. Tyto součásti vytváří a nakonfigurování jako součást nasazení řešení. 

### <a name="real-time-analysis"></a>Analýzy v reálném čase
Události generované Simulator telematika vozidla jsou publikované na událost centrální pomocí SDK centrální události. Úlohy toku analýzy ingests tyto události z centra událostí a procesů data v reálném čase účelem analýzy stavu vozidla. 

![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig5-vehicle-telematics-event-hub-dashboard.png) 

*Obrázek 5 – centrální události řídicího panelu*

![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig6-vehicle-telematics-stream-analytics-job-processing-data.png) 

*Obrázek 6 – úlohy analýzy toku zpracování dat.*

Technologie pro analýzu úlohy toku;

- ingests data z centra události 
- provede spojení s daty odkaz namapujte vozidla VIN na odpovídající modelu 
- je zachovává v úložišti objektů blob Azure pro bohaté dávku analýzy. 

Následující dotaz toku analýzy se používá k uchovávat data v úložišti objektů blob Azure. 

![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig7-vehicle-telematics-stream-analytics-job-query-for-data-ingestion.png) 

*Obrázek 7 – toku analýzy úlohy dotazu pro požití dat*

### <a name="batch-analysis"></a>Dávkové analýzy
Společnost Microsoft generují taky další objemu simulovaný vozidla signály a diagnostiky datovou sadu pro bohatší dávku analýzy. To je nutné zajistit hlasitost dobré zástupce dat pro dávkové zpracování. K tomuto účelu používáme příležitosti s názvem "PrepareSampleDataPipeline" v pracovním postupu Azure Data Factory generovat jeden rok zboží v hodnotě simulovaný vozidla signály a diagnostiky datovou sadu. Klikněte na [vlastní aktivitou Data Factory](http://go.microsoft.com/fwlink/?LinkId=717077) stáhnout Data Factory vlastní DotNet aktivity Visual Studio řešení pro přizpůsobení svým požadavkům. 

![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig8-vehicle-telematics-prepare-sample-data-for-batch-processing.png) 

*Obrázek 8 – Příprava ukázkových dat pro dávkové zpracování pracovního postupu*

Kanálu se skládá z vlastní .net ADF aktivity, zobrazit tady:

![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig9-vehicle-telematics-prepare-sample-data-pipeline.png) 

*Obrázek 9 - PrepareSampleDataPipeline*

Jakmile kanálu provede úspěšně a datovou sadu "RawCarEventsTable" je označený řádkem "Připravena", jeden rok jmění simulovaný vozidla signály a diagnostické je vyrobeno data. Zobrazí se následující složky a soubor vytvořený ve vašem účtu úložiště v části kontejneru "connectedcar":

![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig10-vehicle-telematics-prepare-sample-data-pipeline-output.png) 

*Obrázek 10 – PrepareSampleDataPipeline výstup*

### <a name="references"></a>Odkazy

[Azure SDK centrální událostí pro požití toku](../event-hubs/event-hubs-csharp-ephcs-getstarted.md)

[Možnosti pohybu dat Azure Data Factory](../data-factory/data-factory-data-movement-activities.md)
[Azure dat Factory DotNet aktivity](../data-factory/data-factory-use-custom-activities.md)

[Příprava ukázkových dat Azure vyřešíte aktivity visual studio DotNet Factory dat](http://go.microsoft.com/fwlink/?LinkId=717077) 


## <a name="partition-the-dataset"></a>Oddíl datové sady

Jako nezpracovaná částečně strukturovaná vozidla signály a diagnostických datovou sadu se oddíly v kroku Příprava dat do formátu roku a měsíce. Toto rozdělení líbí, zvýší efektivnější dotazy a scalable dlouhodobé úložiště povolením poruch převrácení z jednoho objektů blob účet do dalšího jako první účet zaplní. 

>[AZURE.NOTE] Tento krok v řešení platí pouze pro dávkové zpracování.

Vstupní a výstupní data Správa dat:

- **Výstupní data** (označena *PartitionedCarEventsTable*) jsou dlouhé časové období uchování jako základní / "rawest" formulář dat v zákazníka "Data jezera". 
- **Zadávání dat** do tohoto kanálu by zrušit obvykle jako výstupní data obsahuje celou věrností na vstup – stačí uložený (oddíly) lépe pro pozdější použití.

![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig11-vehicle-telematics-partition-car-events-workflow.png)

*Obrázek 11 – oddíl auta události pracovního postupu*

Nezpracovanými daty je oddílů, aktivitu podregistru HDInsight aplikaci "PartitionCarEventsPipeline". Ukázková data vytvořený v kroku 1 pro rok rozdělit podle roku a měsíce. Oddíly byla použita pro vytvoření vozidla signály a diagnostických dat pro každý měsíc (celková 12 oddíly) roku. 

![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig12-vehicle-telematics-partition-car-events-pipeline.png)

*Obrázek 12 - PartitionCarEventsPipeline*

Následující skript podregistru s názvem "partitioncarevents.hql", se používá k rozdělení a se nachází ve složce "\demo\src\connectedcar\scripts" stažený zip. 


    SET hive.exec.dynamic.partition=true;
    SET hive.exec.dynamic.partition.mode = nonstrict;
    set hive.cli.print.header=true;

    DROP TABLE IF EXISTS RawCarEvents; 
    CREATE EXTERNAL TABLE RawCarEvents 
    (
                vin                             string,
                model                           string,
                timestamp                       string,
                outsidetemperature              string,
                enginetemperature               string,
                speed                           string,
                fuel                            string,
                engineoil                       string,
                tirepressure                    string,
                odometer                        string,
                city                            string,
                accelerator_pedal_position      string,
                parking_brake_status            string,
                headlamp_status                 string,
                brake_pedal_status              string,
                transmission_gear_position      string,
                ignition_status                 string,
                windshield_wiper_status         string,
                abs                             string,
                gendate                         string
                
    ) ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' LINES TERMINATED BY '10' STORED AS TEXTFILE LOCATION '${hiveconf:RAWINPUT}'; 

    DROP TABLE IF EXISTS PartitionedCarEvents; 
    CREATE EXTERNAL TABLE PartitionedCarEvents 
    (
                vin                             string,
                model                           string,
                timestamp                       string,
                outsidetemperature              string,
                enginetemperature               string,
                speed                           string,
                fuel                            string,
                engineoil                       string,
                tirepressure                    string,
                odometer                        string,
                city                            string,
                accelerator_pedal_position      string,
                parking_brake_status            string,
                headlamp_status                 string,
                brake_pedal_status              string,
                transmission_gear_position      string,
                ignition_status                 string,
                windshield_wiper_status         string,
                abs                             string,
                gendate                         string
    ) partitioned by (YearNo int, MonthNo int) ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' LINES TERMINATED BY '10' STORED AS TEXTFILE LOCATION '${hiveconf:PARTITIONEDOUTPUT}';

    DROP TABLE IF EXISTS Stage_RawCarEvents; 
    CREATE TABLE IF NOT EXISTS Stage_RawCarEvents 
    (
                vin                             string,
                model                           string,
                timestamp                       string,
                outsidetemperature              string,
                enginetemperature               string,
                speed                           string,
                fuel                            string,
                engineoil                       string,
                tirepressure                    string,
                odometer                        string,
                city                            string,
                accelerator_pedal_position      string,
                parking_brake_status            string,
                headlamp_status                 string,
                brake_pedal_status              string,
                transmission_gear_position      string,
                ignition_status                 string,
                windshield_wiper_status         string,
                abs                             string,
                gendate                         string,
                YearNo                          int,
                MonthNo                         int) 
    ROW FORMAT delimited fields terminated by ',' LINES TERMINATED BY '10';

    INSERT OVERWRITE TABLE Stage_RawCarEvents
    SELECT
        vin,            
        model,
        timestamp,
        outsidetemperature,
        enginetemperature,
        speed,
        fuel,
        engineoil,
        tirepressure,
        odometer,
        city,
        accelerator_pedal_position,
        parking_brake_status,
        headlamp_status,
        brake_pedal_status,
        transmission_gear_position,
        ignition_status,
        windshield_wiper_status,
        abs,
        gendate,
        Year(gendate),
        Month(gendate)

    FROM RawCarEvents WHERE Year(gendate) = ${hiveconf:Year} AND Month(gendate) = ${hiveconf:Month}; 

    INSERT OVERWRITE TABLE PartitionedCarEvents PARTITION(YearNo, MonthNo) 
    SELECT
        vin,            
        model,
        timestamp,
        outsidetemperature,
        enginetemperature,
        speed,
        fuel,
        engineoil,
        tirepressure,
        odometer,
        city,
        accelerator_pedal_position,
        parking_brake_status,
        headlamp_status,
        brake_pedal_status,
        transmission_gear_position,
        ignition_status,
        windshield_wiper_status,
        abs,
        gendate,
        YearNo,
        MonthNo
    FROM Stage_RawCarEvents WHERE YearNo = ${hiveconf:Year} AND MonthNo = ${hiveconf:Month};

*Obrázek 13 - PartitionConnectedCarEvents podregistru skriptu*

Po spuštění kanálu úspěšně by se zobrazit následující oddíly generovaného ve vašem účtu úložiště v části kontejneru "connectedcar".

![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig14-vehicle-telematics-partitioned-output.png)

*Obrázek 14 – oddíly výstup*

Data teď optimalizován, se snadno spravuje a jste připraveni na další zpracování získat bohaté dávku přehledy. 

## <a name="data-analysis"></a>Analýza dat

V této části najdete v článku jak kombinovat Azure toku technologie pro analýzu, výukové počítače Azure, Azure Data Factory a Azure HDInsight bohatou pokročilé technologie pro analýzu na vozidla zdraví a řízení zvyky. Existují tři pododdílu tady:

1.  **Výukové počítače**: Tento pododdílu obsahuje informace o zjišťování testu odchylky použité mít v tomto řešení odhadnout vozidla vyžadující údržbu údržbu a vozidla vyžadující navrácení kvůli problémům s bezpečnosti k.
2.  **Data v reálném analýzy**: Tento pododdílu obsahuje informace týkající se v reálném čase analýzy pomocí jazyka dotazu toku technologie pro analýzu a operationalizing experiment výukové počítače v reálném čase pomocí vlastní aplikace.
3.  **Analýza dávku**: Tento pododdílu obsahuje informace o transformace a zpracování dávku dat pomocí Azure HDInsight a Azure počítače výuka operationalized Azure Data Factory.

### <a name="machine-learning"></a>Výukové počítače

Naším cílem je předpovídání vozidla, které vyžadují údržbu nebo odvolat podle určitých zdraví statistiky. Provedeme následující předpoklady

- Pravdivosti jednu z následujících tři podmínky, vozidla vyžadovat **údržbu údržbu**:
    - Můžete zadat tlaku je nízká
    - Úroveň oil stroje je nízká
    - Teplotní Engine nastavená dostatečně vysoká

- Pravdivosti jednu z následujících podmínek, vozidla může mít **bezpečnostního problému** a vyžadovat **odvolání**:
    - Teplotní Engine nastavená dostatečně vysoká, ale venkovní teploty je nízká
    - Teplotní Engine zhoršeným ale venkovní teploty vysoké

Na základě předchozí požadavků, jsme vytvořili dvou samostatných modelů zjišťování odchylky, jeden vozidla údržbu zjišťování a druhý pro zjišťování odvolání vozidla. V obou těchto modelech algoritmu předdefinované hlavní komponenty analýzy (kompatibilitou) slouží ke zjištění odchylky. 

**Údržby zjišťování modelu**

Pokud nějaká tři indikátory - plášť tlaku engine oil nebo engine teploty - vystihuje jeho odpovídajících podmínce, model zjišťování údržbu sestavy místním. V důsledku toho pouze potřebujeme zvažte tyto tři proměnné při vytváření modelu. V naší experiment Azure počítač přečíst jsme nejdřív pomocí modulu **Vybrat sloupce v sadě dat** extrahovat tyto tři proměnné. Další používáme modulu zjišťování na základě kompatibilitou odchylky k vytvoření modelu zjišťování odchylky. 

Hlavní součásti analýzy (kompatibilitou) je technika založení ve počítače učení, které se dají použít pro výběr funkcí, klasifikace a odchylky zjišťování. Kompatibilitou převede sadu případ obsahující případně vzájemném vztahu proměnných v množině hodnot s názvem hlavní součásti. Klíčové představu o na základě kompatibilitou modelování je k datům projektu na dolním dimenzionální plochu tak, aby funkce a odchylky můžete snadno identifikovat.
 
Pro každý nový předávat na vstupu modelu zjišťování detekce odchylky nejdřív vypočítá jeho projekci v eigenvectors a potom vypočítá normalizovanou obnovy chyby. Tato chyba normalizovanou je skóre odchylky. Vyšší chyba, více neobvyklých instance je. 

V zjišťování problému údržbu každý záznam považovat za definované bod mezerou 3 dimenzionální plášť tlaku, engine oil a engine teploty souřadnice. Zachycení tyto odchylky, jsme původní data do pole 3 dimenzionální projektu do 2 dimenzionální prostoru pomocí kompatibilitou. Proto jsme nastavit parametr počet komponenty pro použití v kompatibilitou je 2. Tento parametr hraje úlohu důležité při použití na základě kompatibilitou odchylky zjišťování. Po plánování dat pomocí kompatibilitou budou tyto odchylky popsána snadněji.

**Odvolání odchylky zjišťování modelu** Odvolání odchylky zjišťování modelu, používáme vybrat sloupce v sadě dat a na základě kompatibilitou odchylky zjišťování moduly podobným způsobem. Konkrétně jsme nejprve extrahovat třemi proměnnými - engine teplotní, mimo teplotní a rychlosti – pomocí modulu **Vybrat sloupce datové sady** . Budeme se týkají taky proměnnou rychlosti od teploty engine se obvykle vazba rychlosti. Další používáme modul zjišťování na základě kompatibilitou odchylky dat ze 3 – dimenzionální místo projektu na 2 dimenzionální plochu. Náhradní kritéria jsou splněny a vyžaduje odvolání vozidla při engine teploty a venkovní teploty vysoce negativně vazba. Použití algoritmus rozpoznávání na základě kompatibilitou odchylky, jsme zachytit nesrovnalosti po provedení kompatibilitou. 

Školení buď modelu, potřebujeme používat normální data, která není nutné zadávat údržbu nebo odvolání jako vstupní data, která chcete školení na základě kompatibilitou odchylky zjišťování modelu. V bodování experiment používáme modelu zjišťování školení odchylky zjistit, zda vozidla vyžaduje údržbu nebo odvolat. 


### <a name="real-time-analysis"></a>Analýzy v reálném čase

Následující dotaz SQL toku analýzy se používá k získání průměr všechny parametry důležité vozidla například rychlosti paliva úroveň, teploty engine, ujeté vzdálenosti čtení, můžete zadat tlaku, úroveň oil stroje a ostatní. Průměrů slouží k zjistí odchylky, problémů upozornění a zjistit celkový podmínky stavu vozidla spravovaný v určité oblasti a poté vzájemného vztahu s účastníky procesu. 

![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig15-vehicle-telematics-stream-analytics-query-for-real-time-processing.png)

Obrázek 15 – toku analýzy dotaz na data v reálném zpracování

Všechny průměry počítají přes TumblingWindow 3 sekundy. Používáme TubmlingWindow v tomto případě vzhledem k tomu budete potřebovat nepřekrývající a souvislé časových intervalů. 

Další informace o všech funkcích "Práce s okny" v Azure toku analýzy, klikněte na [práce s okny (Azure toku technologie pro analýzu)](https://msdn.microsoft.com/library/azure/dn835019.aspx).

**Data v reálném předpovědí**

Aplikace je součástí řešení, které umožňují modelu výukové počítače v reálném čase. Tato aplikace s názvem "RealTimeDashboardApp" je vytvoření a konfiguraci jako součást nasazení řešení. Aplikace provede následující:

1.  Okolním instanci události centrální místo, kam toku analýzy je publikování události ve vzorci nepřetržitě. ![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig16-vehicle-telematics-stream-analytics-query-for-publishing.png)*Obrázek 16 – dotazu toku analytics pro publikování dat na výstup instance centrální události* 

2.  Pro každé události, která bude tato aplikace: 

    - Zpracuje dat pomocí počítače výukové odpověď na žádost o hodnocení prostředku koncového bodu. Koncový bod RR automaticky publikován jako součást nasazení.
    - Výstup rr je publikován na datovou sadu PowerBI pomocí nabízených rozhraní API.

Tento způsob je také k dispozici scénáře, ve kterých chcete integrovat s aplikací obchodními (LoB) v reálném čase analýzy toku scénářích například upozornění, oznámení a zasílání zpráv.

Klikněte na [Stáhnout RealtimeDashboardApp](http://go.microsoft.com/fwlink/?LinkId=717078) ke stažení RealtimeDashboardApp Visual Studio řešení pro úpravy. 

**Ke spuštění aplikace řídicího panelu v reálném čase**

1.  Klikněte na uzel PowerBI v zobrazení diagramu a klikněte na odkaz "Stáhnout v reálném čase řídicí panel aplikace" v podokně Vlastnosti. ![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig17-vehicle-telematics-powerbi-dashboard-setup.png)*Obrázek 17 – pokyny k nastavení PowerBI řídicího panelu*
2.  Extrahování a uložit místně ![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig18-vehicle-telematics-realtimedashboardapp-folder.png) *obrázek 18 – RealtimeDashboardApp složky*
3.  Spuštění aplikace RealtimeDashboardApp.exe
4.  Poskytnutí platných přihlašovacích údajů Power BI, přihlaste se a klikněte na přijmout ![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig19a-vehicle-telematics-realtimedashboardapp-sign-in-to-powerbi.png)![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig19b-vehicle-telematics-realtimedashboardapp-sign-in-to-powerbi.png) 

*Obrázek 19 – RealtimeDashboardApp: Přihlášení k PowerBI*

>[AZURE.NOTE] Pokud chcete vyprázdnit datovou sadu PowerBI, spustit RealtimeDashboardApp s parametrem "flushdata": 

    RealtimeDashboardApp.exe -flushdata

### <a name="batch-analysis"></a>Dávkové analýzy

Cílem tady je zobrazit, jak Contoso motory využívá Azure výpočetní funkce pro svůj velký dat k získání bohaté Další informace o řízení vzorku, chování využití a stavu vozidla. To umožňuje:

- Zlepšit prostředí zákazníků a jeho levnější zadáním přehledy na řízení zvyky a paliva efektivní řízení chování
- Seznamte se s včasným zákazníků a jejich řidiče k jízdě patters řídí obchodní rozhodnutí a poskytnout nejlepší v předmětu produkty a služby

V tomto řešení jsme směrujete následující metriky:

1.  **Agresivní řízení chování**: identifikuje trend modelů, umístění, řidiče k jízdě podmínky a čas v roce získat další informace o agresivní řidiče k jízdě vzorků. Contoso motory můžete použít tyto přehledy pro marketingové kampaně řízení přizpůsobených novými funkcemi a na základě použití pojištění.
2.  **Efektivní řízení chování paliva**: identifikuje trend modelů, umístění, řidiče k jízdě podmínky a čas v roce získat přehledy na paliva efektivně řidiče k jízdě vzorků. Contoso motory můžete použít tyto přehledy pro marketingové kampaně, řízení novými funkcemi a aktivní vykazování ovladače nákladů platnosti a prostředí popisný řidiče k jízdě zvyky. 
3.  **Odvolání modely**: identifikuje modely vyžadující navrácení operationalizing počítače zjišťování odchylky výukové testu

Podívejme se na podrobné informace o všech těchto metriky


**Agresivní řidiče k jízdě vzorek**

Rozdělený vozidla signály a dat diagnostiky se zpracovávají v kanálu s názvem "AggresiveDrivingPatternPipeline" použití podregistru k určení modely umístění, vozidla, řidiče k jízdě podmínky a ostatní parametry funkce, která vykazuje agresivní řidiče k jízdě vzorku.

![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig20-vehicle-telematics-aggressive-driving-pattern.png) 
*Obrázek 20 – Aggressive řízení vzorek pracovního postupu*

Skript podregistru s názvem "aggresivedriving.hql" použít pro analýzu agresivní řidiče k jízdě vzorek podmínku je umístěn v "\demo\src\connectedcar\scripts" složky stažené zip. 

    DROP TABLE IF EXISTS PartitionedCarEvents; 
    CREATE EXTERNAL TABLE PartitionedCarEvents
    (
                vin                             string,
                model                           string,
                timestamp                       string,
                outsidetemperature              string,
                enginetemperature               string,
                speed                           string,
                fuel                            string,
                engineoil                       string,
                tirepressure                    string,
                odometer                        string,
                city                            string,
                accelerator_pedal_position      string,
                parking_brake_status            string,
                headlamp_status                 string,
                brake_pedal_status              string,
                transmission_gear_position      string,
                ignition_status                 string,
                windshield_wiper_status         string,
                abs                             string,
                gendate                         string
                                
    ) ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' LINES TERMINATED BY '10' STORED AS TEXTFILE LOCATION '${hiveconf:PARTITIONEDINPUT}';

    DROP TABLE IF EXISTS CarEventsAggresive; 
    CREATE EXTERNAL TABLE CarEventsAggresive
    (
                vin                         string, 
                model                       string,
                timestamp                   string,
                city                        string,
                speed                       string,
                transmission_gear_position  string,
                brake_pedal_status          string,
                Year                        string,
                Month                       string
                                
    ) ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' LINES TERMINATED BY '10' STORED AS TEXTFILE LOCATION '${hiveconf:AGGRESIVEOUTPUT}';



    INSERT OVERWRITE TABLE CarEventsAggresive
    select
    vin,
    model,
    timestamp,
    city,
    speed,
    transmission_gear_position,
    brake_pedal_status,
    "${hiveconf:Year}" as Year,
    "${hiveconf:Month}" as Month
    from PartitionedCarEvents
    where transmission_gear_position IN ('fourth', 'fifth', 'sixth', 'seventh', 'eight') AND brake_pedal_status = '1' AND speed >= '50'

*Obrázek 21 – Aggressive řízení vzorek podregistru dotazu*

Používá kombinaci vozidla přenosu ozubeného kola pozici brzdy Pedálové stav a rychlosti zjišťování reckless/agresivní řízení chování v závislosti na brzdění vzorek na vysokorychlostní. 

Po spuštění kanálu úspěšně by se zobrazit následující oddíly generovaného ve vašem účtu úložiště v části kontejneru "connectedcar".

![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig22-vehicle-telematics-aggressive-driving-pattern-output.png) 

*Obrázek 22 – AggressiveDrivingPatternPipeline výstup*


**Efektivní řízení vzorek paliva**

Rozdělený vozidla signály a dat diagnostiky se zpracovávají v kanálu s názvem "FuelEfficientDrivingPatternPipeline". Podregistru slouží k určení modely umístění, vozidla, řidiče k jízdě podmínky a další vlastnosti, které vykazují paliva efektivně řidiče k jízdě vzorem.

![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig23-vehicle-telematics-fuel-efficient-driving-pattern.png) 

*Obrázek 23 – paliva efektivně řidiče k jízdě vzorek pracovního postupu*

Skript podregistru s názvem "fuelefficientdriving.hql" použít pro analýzu agresivní řidiče k jízdě vzorek podmínku je umístěn v "\demo\src\connectedcar\scripts" složky stažené zip. 

    DROP TABLE IF EXISTS PartitionedCarEvents; 
    CREATE EXTERNAL TABLE PartitionedCarEvents
    (
                vin                             string,
                model                           string,
                timestamp                       string,
                outsidetemperature              string,
                enginetemperature               string,
                speed                           string,
                fuel                            string,
                engineoil                       string,
                tirepressure                    string,
                odometer                        string,
                city                            string,
                accelerator_pedal_position      string,
                parking_brake_status            string,
                headlamp_status                 string,
                brake_pedal_status              string,
                transmission_gear_position      string,
                ignition_status                 string,
                windshield_wiper_status         string,
                abs                             string,
                gendate                         string
                                
    ) ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' LINES TERMINATED BY '10' STORED AS TEXTFILE LOCATION '${hiveconf:PARTITIONEDINPUT}';

    DROP TABLE IF EXISTS FuelEfficientDriving; 
    CREATE EXTERNAL TABLE FuelEfficientDriving
    (
                vin                         string, 
                model                       string,
                city                        string,
                speed                       string,
                transmission_gear_position  string,                
                brake_pedal_status          string,            
                accelerator_pedal_position  string,                             
                Year                        string,
                Month                       string
                                
    ) ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' LINES TERMINATED BY '10' STORED AS TEXTFILE LOCATION '${hiveconf:FUELEFFICIENTOUTPUT}';



    INSERT OVERWRITE TABLE FuelEfficientDriving
    select
    vin,
    model,
    city,
    speed,
    transmission_gear_position,
    brake_pedal_status,
    accelerator_pedal_position,
    "${hiveconf:Year}" as Year,
    "${hiveconf:Month}" as Month
    from PartitionedCarEvents
    where transmission_gear_position IN ('fourth', 'fifth', 'sixth', 'seventh', 'eight') AND parking_brake_status = '0' AND brake_pedal_status = '0' AND speed <= '60' AND accelerator_pedal_position >= '50'


*Obrázek 24 – paliva efektivně řidiče k jízdě vzorek podregistru dotazu*

Používá kombinaci vozidla přenosu ozubeného kola pozici, brzdy Pedálové stav, rychlé a přístupové Pedálové pozici ke zjištění paliva efektivní řízení chování v závislosti na akcelerace brzdění a dosažení vzorků. 

Po spuštění kanálu úspěšně by se zobrazit následující oddíly generovaného ve vašem účtu úložiště v části kontejneru "connectedcar".

![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig25-vehicle-telematics-fuel-efficient-driving-pattern-output.png) 

*Obrázek 25 – FuelEfficientDrivingPatternPipeline výstup*


**Odvolání předpovědí**

Počítač výukové experiment zřízení a publikován jako webové služby jako součást nasazení řešení. Dávky bodování koncový bod využít v tento pracovní postup registrace jako služba factory propojená data a operationalized pomocí data factory dávku bodování aktivity.

![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig26-vehicle-telematics-machine-learning-endpoint.png) 

*Obrázek 26 – počítače výukové koncový bod registrovaná jako propojené služby na factory dat*

V DetectAnomalyPipeline použití registrovaných propojené služby k získání dat pomocí modelu zjišťování odchylky. 

![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig27-vehicle-telematics-aml-batch-scoring.png) 

*Obrázek 27 – Azure bodování dávku výukové počítače aktivity v factory dat* 

Existuje několik kroků v této kanálem k odesílání zpráv pro přípravu dat provést tak, aby ho mohli operationalized s listem bodování webové služby. 

![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig28-vehicle-telematics-pipeline-predicting-recalls.png) 

*Obrázek 28 – DetectAnomalyPipeline pro odhad vozidla vyžadující navrácení* 

Po dokončení nástroje až aktivitu HDInsight slouží k obrázku a agregovat data, která je považována za odchylky modelem se pravděpodobnost skóre 0,60 nebo vyšší.

    DROP TABLE IF EXISTS CarEventsAnomaly; 
    CREATE EXTERNAL TABLE CarEventsAnomaly 
    (
                vin                         string,
                model                       string,
                gendate                     string,
                outsidetemperature          string,
                enginetemperature           string,
                speed                       string,
                fuel                        string,
                engineoil                   string,
                tirepressure                string,
                odometer                    string,
                city                        string,
                accelerator_pedal_position  string,
                parking_brake_status        string,
                headlamp_status             string,
                brake_pedal_status          string,
                transmission_gear_position  string,
                ignition_status             string,
                windshield_wiper_status     string,
                abs                         string,
                maintenanceLabel            string,
                maintenanceProbability      string,
                RecallLabel                 string,
                RecallProbability           string
                                
    ) ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' LINES TERMINATED BY '10' STORED AS TEXTFILE LOCATION '${hiveconf:ANOMALYOUTPUT}';

    DROP TABLE IF EXISTS RecallModel; 
    CREATE EXTERNAL TABLE RecallModel 
    (

                vin                         string,
                model                       string,
                city                        string,
                outsidetemperature          string,
                enginetemperature           string,
                speed                       string,
                Year                        string,
                Month                       string              
                                
    ) ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' LINES TERMINATED BY '10' STORED AS TEXTFILE LOCATION '${hiveconf:RECALLMODELOUTPUT}';

    INSERT OVERWRITE TABLE RecallModel
    select
    vin,
    model,
    city,
    outsidetemperature,
    enginetemperature,
    speed,
    "${hiveconf:Year}" as Year,
    "${hiveconf:Month}" as Month
    from CarEventsAnomaly
    where RecallLabel = '1' AND RecallProbability >= '0.60'


Po spuštění kanálu úspěšně by se zobrazit následující oddíly generovaného ve vašem účtu úložiště v části kontejneru "connectedcar".

![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig30-vehicle-telematics-detect-anamoly-pipeline-output.png) 

*Obrázek 30 – obrázek 30 – DetectAnomalyPipeline výstup*


## <a name="publish"></a>Publikování

### <a name="real-time-analysis"></a>Analýzy v reálném čase

Některý z těchto dotazů do této úlohy analýzy toku publikuje události výstup instance centrální události. 

![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig31-vehicle-telematics-stream-analytics-job-publishes-output-event-hub.png)

*Obrázek 31 – toku analýzy úlohy publikuje výstup instance centrální události*

![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig32-vehicle-telematics-stream-analytics-query-publish-output-event-hub.png)

*Obrázek 32 – toku analýzy dotazu publikovat do výstupu instance centrální události*

Tomto proudu událostí je využívána RealTimeDashboardApp zahrnuté do řešení. Tato aplikace využívá webová služba strojového výukové žádost o odpovědi na data v reálném hodnocení a publikuje Výsledná data na datovou sadu PowerBI pro spotřebu. 

### <a name="batch-analysis"></a>Dávkové analýzy

Výsledky dávku a v reálném čase zpracování jsou publikované na tabulky databáze SQL Azure pro spotřebu. Azure SQL serveru, databázi a tabulky jsou vytvářeny automaticky jako součást instalačního skriptu. 

![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig33-vehicle-telematics-batch-processing-results-copy-to-data-mart.png)

*Obrázek 33 – dávkové zpracování výsledky kopírovat data Tržiště pracovního postupu*

![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig34-vehicle-telematics-stream-analytics-job-publishes-to-data-mart.png)

*Obrázek 34 – toku analýzy úlohy publikuje datového tržiště*

![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig35-vehicle-telematics-data-mart-setting-in-stream-analytics-job.png)

*Obrázek 35 – datového tržiště nastavení v toku analýzy projektu*


## <a name="consume"></a>Používání

Power BI vám bude radit toto řešení bohaté řídicí panel pro data v reálném čase a prediktivní analýzy vizualizace. 

Podrobné pokyny k nastavení PowerBI sestavy a na řídicím panelu, klikněte sem. Konečný řídicího panelu vypadat takto:

![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig36-vehicle-telematics-powerbi-dashboard.png)

*Obrázek 36 - PowerBI řídicího panelu*

## <a name="summary"></a>Souhrn

Tento dokument obsahuje podrobné podrobnostmi řešení vozidla Telemetrie analýzy. To hodnotí lambda architektura vzor pro v reálném čase a dávkové analýzy s předpovědí a akce. Tento způsob platí pro celou řadu případy použití, které vyžadují kritická cesta (v reálném čase) a technologie pro analýzu studenou path (dávku). 
