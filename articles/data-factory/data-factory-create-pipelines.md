<properties 
    pageTitle="Vytvoření/plán kanály, zřetězení aktivit najdete ve Data Factory | Microsoft Azure" 
    description="Naučte se vytvořit datový kanál v Azure Data Factory přesunout a transformace dat. Vytvoření databázových pracovního postupu k vytvoření připravená k použití informace." 
    keywords="data kanálem k odesílání zpráv, databázových pracovního postupu"
    services="data-factory" 
    documentationCenter="" 
    authors="sharonlo101" 
    manager="jhubbard" 
    editor="monicar"/>

<tags 
    ms.service="data-factory" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article"
    ms.date="09/12/2016" 
    ms.author="shlo"/>

# <a name="pipelines-and-activities-in-azure-data-factory"></a>Kanály a aktivity v Azure Data Factory
Tento článek vám pomůže pochopit kanály a aktivity v Azure Data Factory a použít k vytvoření začátku do konce postupů založených na datech pro přesun dat a zpracování dat scénáře.  

> [AZURE.NOTE] V tomto článku se předpokládá, aby je šlo prostřednictvím [Úvod k Azure Data Factory](data-factory-introduction.md). Pokud nemáte hands-na-zkušenosti s vytvářením data, která by pomoci továrny filtrované pomocí kurz [vytvoření první factory dat](data-factory-build-your-first-pipeline.md) porozumět lepší tohoto článku.  

## <a name="what-is-a-data-pipeline"></a>Co je datový kanál?
**Kanálem k odesílání zpráv** je seskupení logicky související **aktivity**. Používá se pro činnosti skupin do jednotky, které provede úkol. Pro lepší pochopení kanály, budete muset nejdřív Princip činnosti. 

## <a name="what-is-an-activity"></a>Co je aktivitu?
Aktivity definovat akce provádět s daty. Každou aktivitu zabírá žádný nebo více [datových sad](data-factory-create-datasets.md) jako vstupů a vytváří jednu nebo více datových sad jako výstup. 

Například můžete aktivitu kopírovat organizovat kopírování dat z jedné datový úložiště na jiný datový úložiště. Podobně můžete aktivitu HDInsight podregistru spuštění dotazu podregistru Azure HDInsight clusteru k transformaci dat. Azure Data Factory obsahuje řadu [transformace dat](data-factory-data-transformation-activities.md)a [Přesun dat](data-factory-data-movement-activities.md) aktivity. Můžete taky vytvořit vlastní aktivitou .NET spustit vlastní kód. 

## <a name="sample-copy-pipeline"></a>Ukázka kopírovat kanálem k odesílání zpráv
V následující ukázkové kanálu je jedna aktivitu typu **kopie** v části **aktivity** . V tomto příkladu [Kopírovat činnost](data-factory-data-movement-activities.md) slouží ke kopírování dat z úložiště objektů Blob Azure k databázi Azure SQL. 

    {
      "name": "CopyPipeline",
      "properties": {
        "description": "Copy data from a blob to Azure SQL table",
        "activities": [
          {
            "name": "CopyFromBlobToSQL",
            "type": "Copy",
            "inputs": [
              {
                "name": "InputDataset"
              }
            ],
            "outputs": [
              {
                "name": "OutputDataset"
              }
            ],
            "typeProperties": {
              "source": {
                "type": "BlobSource"
              },
              "sink": {
                "type": "SqlSink",
                "writeBatchSize": 10000,
                "writeBatchTimeout": "60:00:00"
              }
            },
            "Policy": {
              "concurrency": 1,
              "executionPriorityOrder": "NewestFirst",
              "retry": 0,
              "timeout": "01:00:00"
            }
          }
        ],
        "start": "2016-07-12T00:00:00Z",
        "end": "2016-07-13T00:00:00Z"
      }
    } 

Mějte na paměti následující skutečnosti:

- V části aktivity je pouze jeden aktivity, jehož **Typ** je nastavený na **Kopírovat**.
- Vstupní aktivity je nastavený na **InputDataset** a výstup aktivity je nastavený na **OutputDataset**.
- V části **typeProperties** **BlobSource** zadaný jako typ zdroje a **SqlSink** není zadán psaní jímky.

Úplné informace o vytváření tohoto kanálu, najdete v článku [kurz: kopírování dat z úložiště objektů Blob k SQL databázi](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md). 

## <a name="sample-transformation-pipeline"></a>Ukázka transformace kanálem k odesílání zpráv
V následující ukázkové kanálu je jedna aktivita typu **HDInsightHive** v části **aktivity** . V tomto příkladu [HDInsight podregistru aktivity](data-factory-hive-activity.md) transformace dat z úložiště objektů Blob Azure spuštěním soubor skriptu podregistru Azure HDInsight Hadoop clusteru. 

    {
        "name": "TransformPipeline",
        "properties": {
            "description": "My first Azure Data Factory pipeline",
            "activities": [
                {
                    "type": "HDInsightHive",
                    "typeProperties": {
                        "scriptPath": "adfgetstarted/script/partitionweblogs.hql",
                        "scriptLinkedService": "AzureStorageLinkedService",
                        "defines": {
                            "inputtable": "wasb://adfgetstarted@<storageaccountname>.blob.core.windows.net/inputdata",
                            "partitionedtable": "wasb://adfgetstarted@<storageaccountname>.blob.core.windows.net/partitioneddata"
                        }
                    },
                    "inputs": [
                        {
                            "name": "AzureBlobInput"
                        }
                    ],
                    "outputs": [
                        {
                            "name": "AzureBlobOutput"
                        }
                    ],
                    "policy": {
                        "concurrency": 1,
                        "retry": 3
                    },
                    "scheduler": {
                        "frequency": "Month",
                        "interval": 1
                    },
                    "name": "RunSampleHiveActivity",
                    "linkedServiceName": "HDInsightOnDemandLinkedService"
                }
            ],
            "start": "2016-04-01T00:00:00Z",
            "end": "2016-04-02T00:00:00Z",
            "isPaused": false
        }
    }

Mějte na paměti následující skutečnosti: 

- V části aktivity je pouze jeden aktivity, jehož **Typ** je nastavený na **HDInsightHive**.
- Soubor skriptu podregistru **partitionweblogs.hql**uložený v okně účet Azure úložiště (definované scriptLinkedService s názvem **AzureStorageLinkedService**) a ve složce **skript** v kontejneru **adfgetstarted**.
- V části **definuje** slouží k určení runtime nastavení, které předávají skriptu podregistru jako podregistru konfigurace hodnoty (například ${hiveconf: inputtable}, ${hiveconf:partitionedtable}).

Úplné informace o vytváření tohoto kanálu, najdete v článku [kurz: vytvoření vaše první kanálem k odesílání zpráv do procesu dat pomocí Hadoop obrázku](data-factory-build-your-first-pipeline.md). 

## <a name="chaining-activities"></a>Zřetězení aktivity
Pokud máte víc aktivity v kanálu a výstup aktivitu není vstup jiné činnosti, aktivit spustit paralelně Pokud připraveni výsečí zadávání dat pro činnosti. 

Zřetězit dvě aktivity tím, že datovou sadu výstup jeden aktivity jako vstupní datovou sadu jiné aktivitě. Činnosti může být ve stejném kanálu nebo v jiné kanály. Druhé činnosti spustí pouze při první úspěšně dokončena. 

Například zvažte následující případ:
 
1.  P1 kanálem k odesílání zpráv má aktivity A1, která vyžaduje vnější zadání datovou sadu D1 a plodiny **výstup** datovou sadu **D2**.
2.  P2 kanálem k odesílání zpráv má A2 aktivity, který vyžaduje **vstupní** ze datovou sadu **D2**a vytvoří výstup datovou sadu D3.
 
V tomto scénáři aktivity A1 spustí externí data je k dispozici a dosažení počet_plateb plánované dostupnosti.  Aktivity A2 spustí plánované výsečí z D2 k dispozici a dosažení počet_plateb plánované dostupnosti. Pokud dojde k chybě v jednom výsečí datovou sadu D2, A2 nejde spustit pro tuto řez, dokud nebude k dispozici.

Zobrazení diagramu:

![Zřetězení aktivit najdete ve dvou potrubí](./media/data-factory-create-pipelines/chaining-two-pipelines.png)

Zobrazení diagramu se obě aktivit najdete ve stejném kanálu: 

![Zřetězení aktivit najdete ve stejné potrubí](./media/data-factory-create-pipelines/chaining-one-pipeline.png)

Další informace najdete v tématu [plánování a provádění](#chaining-activities). 

## <a name="scheduling-and-execution"></a>Plánování a provádění
Zatím jste rozumí kanály a aktivity jaké. Máte taky dívali jak jsou definované a uceleném zobrazení aktivity v Azure Data Factory. Teď dejte nám podívejte se na tom, jak získat spouštět. 

Potrubí je aktivní pouze mezi počáteční čas a koncový čas. Není spouštět před času spuštění nebo po koncový čas. Pokud pozastavenou kanálu se nebudou provedeny bez ohledu na počáteční a koncový čas. Pro kanál spuštění ho neměli pozastavit. Ve skutečnosti ho není kanálu, který se spustí. Je aktivity v kanálu, které provedeny. Ale tak přehrávají v kontextu kanálu. 

V tématech [plánování a provádění](data-factory-scheduling-and-execution.md) pochopit, jak funguje plánování a provádění Azure Data Factory.

## <a name="create-pipelines"></a>Vytvoření potrubí
Azure Data Factory poskytuje různé mechanismy k vytváření a nasazení potrubí (obsahujících zase jeden nebo více aktivity v něm). 

### <a name="using-azure-portal"></a>Azure portálu
Editor Data Factory Azure portálu slouží k vytváření kanálů. Začátku do konce návod najdete v článku [Začínáme s Azure Data Factory (Editor Factory dat)](data-factory-build-your-first-pipeline-using-editor.md) . 

### <a name="using-visual-studio"></a>Používání aplikace Visual Studio 
Visual Studio můžete použít k vytváření a nasazení potrubí Azure Data Factory. Začátku do konce návod najdete v článku [Začínáme s Azure Data Factory (Visual Studio)](data-factory-build-your-first-pipeline-using-vs.md) . 

### <a name="using-azure-powershell"></a>Pomocí prostředí PowerShell Azure
Prostředí PowerShell Azure slouží k vytvoření potrubí v Azure Data Factory. Řekněme který jste definovali kanálu JSON v souboru na c:\DPWikisample.json. Můžete ho nahrát Azure Data Factory instanci jak je vidět v následujícím příkladu:

    New-AzureRmDataFactoryPipeline -ResourceGroupName ADF -Name DPWikisample -DataFactoryName wikiADF -File c:\DPWikisample.json

Začátku do konce návod k vytváření factory dat s kanálu najdete v článku [Začínáme s Azure Data Factory (Azure Powershellu)](data-factory-build-your-first-pipeline-using-powershell.md) . 

### <a name="using-net-sdk"></a>Použití .NET SDK
Můžete vytvořit a příliš nasazení prostřednictvím .NET SDK kanálem k odesílání zpráv. Tento postup lze použít k vytvoření potrubí programově. Další informace najdete v tématu [Vytvoření, spravovat a sledovat data továrny programově](data-factory-create-data-factories-programmatically.md). 


### <a name="using-azure-resource-manager-template"></a>Pomocí šablony správce prostředků Azure
Můžete vytvořit a nasazení kanálem k odesílání zpráv pomocí šablony správce prostředků Azure. Další informace najdete v tématu [Začínáme s Azure Data Factory (Správce prostředků Azure)](data-factory-build-your-first-pipeline-using-arm.md). 

### <a name="using-rest-api"></a>Pomocí rozhraní REST API
Můžete vytvořit a nasazení kanálem k odesílání zpráv pomocí rozhraní REST API příliš. Tento postup lze použít k vytvoření potrubí programově. Další informace najdete v tématu [Vytvoření nebo aktualizace a Pipeline](https://msdn.microsoft.com/library/azure/dn906741.aspx). 


## <a name="monitor-and-manage-pipelines"></a>Sledování a správa potrubí  
Po zavedení potrubí můžete spravovat a sledovat kanály, řezy a spustí. Přečtěte si další informace o ji zde: [sledování a správa potrubí](data-factory-monitor-manage-pipelines.md).


## <a name="pipeline-json"></a>Kanálem k odesílání zpráv JSON   
Dejte nám se blíže podívejte na definici potrubí ve formátu JSON. Obecná struktura potrubí vypadá takto:

    {
        "name": "PipelineName",
        "properties": 
        {
            "description" : "pipeline description",
            "activities":
            [
    
            ],
            "start": "<start date-time>",
            "end": "<end date-time>"
        }
    }

V části **aktivity** může mít jeden nebo více aktivity definované v něm obsažené. Jednotlivé aktivity obsahuje následující strukturu nejvyšší úrovně:

    {
        "name": "ActivityName",
        "description": "description", 
        "type": "<ActivityType>",
        "inputs":  "[]",
        "outputs":  "[]",
        "linkedServiceName": "MyLinkedService",
        "typeProperties":
        {
    
        },
        "policy":
        {
        }
        "scheduler":
        {
        }
    }

Následující tabulka popisují vlastnosti v rámci JSON definice aktivity a kanálem k odesílání zpráv:

Značka | Popis | Povinné
--- | ----------- | --------
Jméno | Název aktivity nebo kanálu. Zadejte název, který představuje akci, že aktivity nebo kanálem k odesílání zpráv nakonfigurovaný tak, aby se<br/><ul><li>Maximální počet znaků: 260</li><li>Musí začínat písmenem číslo nebo podtržítka (_)</li><li>Následující znaky nejsou povoleny: ".", "a", "?", "/", "<";">"; "*", "%", "&", ":","\\"</li></ul> | Ano
Popis | Text s popisem aktivity nebo kanálem k odesílání zpráv k čemu slouží | Ano
Typ | Určuje typ aktivity. Naleznete v článcích [Dat pohyb aktivity](data-factory-data-movement-activities.md) a [Aktivity transformace dat](data-factory-data-transformation-activities.md) u různých typů činností. | Ano
zadávání | Zadávání tabulek použitých aktivita<br/><br/>jeden vstupní tabulka<br/>"vstupů": [{"název": "inputtable1"}],<br/><br/>dvě vstupní tabulky <br/>"vstupů": [{"název": "inputtable1"}, {"název": "inputtable2"}], | Ano
výstup | Výstup tabulky použít tak, že activity.// tabulkami výstup<br/>"výstupy": [{"název": "outputtable1"}],<br/><br/>výstup tabulkami<br/>"výstupy": [{"název": "outputtable1"}, {"název": "outputtable2"}], | Ano
linkedServiceName | Název propojené služba používaná aktivity. <br/><br/>Aktivitu může vyžadovat určit propojené služba, která obsahuje odkazy na požadované výpočetním prostředí. | Ano pro HDInsight aktivity a Azure automatické učení dávku bodování aktivity <br/><br/>Ne pro všechny ostatní
typeProperties | Vlastnosti v části typeProperties závisí na typu aktivity. | Ne
zásady | Zásady, které ovlivňují chování běhu aktivity. Pokud není zadán, budou použity výchozí zásady. | Ne
zahájení | Počáteční datum a čas profilace. Musí být ve [formátu ISO](http://en.wikipedia.org/wiki/ISO_8601). Příklad: 2014-10 – 14T16:32:41Z. <br/><br/>Je možné stanovit místní čas, například EST čas. Tady je příklad: "2016 – 02-27T06:00:00**-05:00**", což je odhad dopoledne 6<br/><br/>Zahájení a ukončení vlastnosti společně určit aktivní období profilace. Výstup výsečí je vyrobeno pouze se v tomto aktivní období. | Ne<br/><br/>Pokud zadáte hodnotu pro vlastnost ukončit, zadejte hodnotu pro vlastnost start.<br/><br/>Počátečního a koncového času můžete i vyprázdní a vytvořte příležitost. Je nutné zadat hodnoty nastavíte aktivní dobu profilace spustit. Pokud nezadáte počátečního a koncového času při vytváření kanálů, můžete nastavit pomocí rutinu Set-AzureRmDataFactoryPipelineActivePeriod později.
ukončení | Koncové datum a čas profilace. Pokud je zadaná musí být ve formátu ISO. Příklad: 2014-10 – 14T17:32:41Z <br/><br/>Je možné stanovit místní čas, například EST čas. Tady je příklad: "2016 – 02-27T06:00:00**-05:00**", což je odhad dopoledne 6<br/><br/>Jako hodnotu pro vlastnost ukončit spustíte kanálu donekonečna udržovat zadejte 9999-09-09. | Ne <br/><br/>Pokud zadáte hodnotu pro vlastnost start, zadejte hodnotu pro vlastnost ukončit.<br/><br/>V části poznámky pro vlastnost **start** .
isPaused | Pokud nastavena na hodnotu true kanálu nespustí. Výchozí hodnota = false. Můžete tuto vlastnost Povolit nebo zakázat. | Ne 
Plánovač | Vlastnost "Plánovač" slouží k definování požadované plánování aktivity. Jeho podvlastnosti jsou stejné jako v [dostupnost vlastnosti datovou sadu](data-factory-create-datasets.md#Availability). | Ne |   
| pipelineMode | Metoda při plánování spustí profilace. Povolené hodnoty jsou: naplánované (výchozí), jednorázově.<br/><br/>"Pravidelnou" označuje, že kanálu běží na zadaný časový interval podle doby aktivní (čas zahájení a ukončení). "Jednorázově" označuje, kanálu spustí pouze jednou. Jednorázově potrubí po vytvoření se nedají upravit nebo aktualizovat aktuálně. Podrobnosti o jednorázově nastavení najdete v článku [Onetime kanálem k odesílání zpráv](data-factory-scheduling-and-execution.md#onetime-pipeline) . | Ne | 
| expirationTime | Dobu trvání, po vytvoření jehož kanálu platné a zůstanou zřizování. Pokud nemuselo mít už aktivní selže, nebo čeká na vyřízení spustil kanálu odstraněna automaticky po nedosáhnete doby vypršení platnosti. | Ne | 
| datové sady | Seznam datové sady pro použití v činnosti definované v kanálu. Tato vlastnost mohou sloužit k definování datové sady, které jsou specifické pro tento kanál a Nedefinováno v rámci výroby data. Datové sady definované v rámci tohoto kanálu lze použít pouze tak, že tohoto kanálu a nejde sdílet. Podrobnosti najdete v části [rozsah datové sady](data-factory-create-datasets.md#scoped-datasets) .| Ne |  
 

### <a name="policies"></a>Zásady
Zásady ovlivňovat běhu aktivity, konkrétně při zpracování výseč tabulky. Následující tabulka obsahuje podrobnosti.

Vlastnost | Povolené hodnoty | Výchozí hodnota | Popis
-------- | ----------- | -------------- | ---------------
souběžné | Celé číslo <br/><br/>Maximální hodnota: 10 | 1 | Počet souběžné spuštění aktivity.<br/><br/>Zjistí počet spuštění paralelní aktivity, ke kterým může dojít v různých výsečí. Například pokud je potřeba aktivitu absolvovat velké sadu dostupných údajů mají větší hodnotu souběžné budete urychlíte si zpracování dat. 
executionPriorityOrder | NewestFirst<br/><br/>OldestFirst | OldestFirst | Určuje pořadí výsečí dat, které zpracovávají.<br/><br/>Pokud máte 2 řezy (jeden později na 16: 00 a nějaký jiný na 17: 00) a jsou obě čekající na spuštění. Pokud jste nastavili executionPriorityOrder být NewestFirst, řez na 17: 00, je zpracován jako první. Podobně pokud nastavíte executionPriorityORder být OldestFIrst, potom na výseč na 16: 00 zpracování. 
Opakovat | Celé číslo<br/><br/>Maximální hodnota je 10 | 3 | Počet opakování před zpracování dat pro řez označena jako chyba. Spuštění aktivity řezu dat je opakovat až počet zadaný opakování. Co nejdříve po selhání probíhá opakovat.
časový limit | Časového rozpětí | 00:00:00 | Časový limit aktivity. Příklad: 00:10:00 (zahrnuje časový limit 10 minut)<br/><br/>Pokud hodnota není zadán nebo hodnotu 0, časový limit je neomezená.<br/><br/>Pokud čas zpracování dat na řez překročí časový limit, se zruší a systému pokusí opakovat zpracování. Počet opakování závisí na vlastnost opakování. Pokud dojde k časový limit, je nastaven stav TimedOut.
zpoždění | Časového rozpětí | 00:00:00 | Nastavit zpoždění zpracování dat spuštění výsečí.<br/><br/>Spuštění aktivity řezu dat je spuštěn po dobu dřívější předpokládanou dobu, po spuštění.<br/><br/>Příklad: 00:10:00 (zahrnuje zpoždění 10 minut)
longRetry | Celé číslo<br/><br/>Maximální hodnota: 10 | 1 | Počet dlouhé pokusů před selháním spuštění výsečí.<br/><br/>longRetry pokusy jsou rovnoměrně tak, že longRetryInterval. Pokud třeba Chcete-li zadat dobu mezi pokusů tak pomocí longRetry. Pokud nejsou zadány opakování a longRetry, každý pokus o longRetry obsahuje pokusů a maximální počet neúspěšných pokusech o je opakovat * longRetry.<br/><br/>Pokud například máme následující nastavení zásadám aktivity:<br/>Opakovat: 3<br/>longRetry: 2<br/>longRetryInterval: 01:00:00<br/><br/>Předpokládají, je jedna výseč provést (stav Čeká) a spuštění aktivity pokaždé, když dojde k chybě. Nejprve bude 3 pokusech po sobě jdoucí spuštění. Po jednotlivých pokus o stavu řez bude opakovat. Po první 3 pokusy prostřednictvím stavu řez bude LongRetry.<br/><br/>Za hodinu (to znamená longRetryInteval jeho hodnota) bude další dvojice 3 pokusech po sobě jdoucí spuštění. Poté stav výseč by se nepodařilo a napoprvé další by pokus o doručení. Proto celkové 6 byly provedeny pokusy.<br/><br/>Pokud libovolný spuštění úspěšné, stav řez bude připravených a napoprvé další jsou pokus o doručení.<br/><br/>longRetry lze v situacích, kde závislých dat dorazí determinizaci časy nebo celkového prostředí je flaky v části které zpracování dat probíhá. V takovém případě byste, které opakování jeden po druhém nepomůže a to po uplynutí čas výsledky v požadované výstupní.<br/><br/>Word upozornění: nastavení vysoké hodnot longRetry nebo longRetryInterval. Obvykle znamenat vyšší hodnoty ostatních systémová problémy. 
longRetryInterval | Časového rozpětí | 00:00:00 | Zpoždění mezi dlouhé pokusů 


## <a name="next-steps"></a>Další kroky

- Princip [plánování a provádění v Azure Data Factory](data-factory-scheduling-and-execution.md).  
- Přečtěte si téma [Přesun dat](data-factory-data-movement-activities.md) a [Možnosti transformace dat](data-factory-data-transformation-activities.md) v Azure Data Factory
- Princip [správy a sledování v Azure Data Factory](data-factory-monitor-manage-pipelines.md).
- [Vytvořte a nasaďte první kanálu](data-factory-build-your-first-pipeline.md). 
