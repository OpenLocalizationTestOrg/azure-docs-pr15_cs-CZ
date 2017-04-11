<properties 
    pageTitle="Použití počítače výukové aktivity | Microsoft Azure" 
    description="Popisuje, jak vytvořit vytvořit prediktivní potrubí pomocí Azure Data Factory a Azure počítače výuka" 
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
    ms.date="09/06/2016" 
    ms.author="shlo"/>

# <a name="create-predictive-pipelines-using-azure-machine-learning-activities"></a>Vytvoření prediktivní potrubí pomocí Azure počítače výukové aktivit   
> [AZURE.SELECTOR]
[Podregistru](data-factory-hive-activity.md)  
[Prasátko](data-factory-pig-activity.md)  
[MapReduce](data-factory-map-reduce.md)  
[Přenos Hadoop](data-factory-hadoop-streaming-activity.md)
[Počítače výukové](data-factory-azure-ml-batch-execution-activity.md) 
[Uložená procedura](data-factory-stored-proc-activity.md)
[Analýzy jezera dat U-SQL](data-factory-usql-activity.md)
[.NET vlastní](data-factory-use-custom-activities.md)

## <a name="introduction"></a>Úvod

[Výukové počítače Azure](https://azure.microsoft.com/documentation/services/machine-learning/) umožňuje vytvářet, otestujte a nasazení řešení prediktivní analýzy. Z uceleném pohledu probíhá ve třech krocích: 

1. **Vytvoření školení testu**. Provádění tohoto kroku pomocí Azure ML Studio. ML studio je pro spolupráci vizuální vývojové prostředí, které používáte pro školení a test prognostické analýzy modelu pomocí školení data.
2. **Převeďte ji na prognostické testu**. Jakmile modelu byla školení s existující data a můžete ji používat ke skóre nová data, připravit a zjednodušit experiment hodnocení.
3. **Nasazení ho ve formátu webové služby**. Můžete publikovat bodování experiment jako Azure webové služby. Můžete odeslat data do modelu pomocí této webové služby koncového bodu a zobrazí výsledek předpovědí z modelu.  

Azure Data Factory umožňuje snadno vytvářet kanály, které používají publikovaných [Azure počítače výukové] [ azure-machine-learning] webová služba pro prediktivní analýzy. Najdete v článku [Úvod k Azure Data Factory](data-factory-introduction.md) a [sestavte svůj první kanálem k odesílání zpráv](data-factory-build-your-first-pipeline.md) články vám pomůžou rychle začít se službou Azure Data Factory. 

Použití **Aktivity spuštění dávky** v Azure Data Factory kanálu, může vyvolat webové služby Azure ML zvětšit předpovědí na data v listu. [Vyvolání Azure ML webové služby pomocí aktivity spuštění dávky](#invoking-an-azure-ml-web-service-using-the-batch-execution-activity) v části Podrobnosti.

V průběhu času prediktivní modely v Azure ML bodování pokusy muset být retrained pomocí nového vstupní datové sady. Retrain modelu Azure ML od Data Factory kanálu provedením následujících kroků: 

1. Publikujte experiment školení (ne prediktivní experiment) jako webové služby. Tento krok Azure ML Studio provedete stejně jako při vystavit prognostické experiment jako webové služby v předchozím případě.
2. Spuštění činnost Azure ML dávku slouží k vyvolat webové služby pro vzdělávání testu. V podstatě můžete aktivitu spuštění dávky ML Azure vyvolat školení webové služby a bodování webové služby. 
  
Po dokončení s přeškolení chcete aktualizovat hodnocení webové služby (prognostické experiment vystavit jako webové služby) se nově školení modelu. Tady je postup: 

1. Přidání jiného než výchozího koncový bod bodování webové služby. Výchozí koncový bod webové služby nelze aktualizovat, aby potřebujete k vytvoření koncového bodu jiného než výchozího pomocí portálu Azure. Naleznete v článku [Vytvoření koncové body](../machine-learning/machine-learning-create-endpoint.md) informací vysvětlujících a procedurální kroky.
2. Aktualizace stávající služby Azure ML propojené hodnocení na používání jiného než výchozího koncového bodu. Začínáme používat nový koncový bod používat webové služby, které se aktualizuje.
3. **Aktivity zdroje aktualizovat ML Azure** slouží k aktualizaci webové služby nově školení modelu.  

[Aktualizace ML Azure modely použití aktivity zdroje aktualizace](#updating-azure-ml-models-using-the-update-resource-activity) v části Podrobnosti. 

## <a name="invoking-a-web-service-using-batch-execution-activity"></a>Volání webové služby pomocí aktivity spuštění dávky

Umožňuje Azure Data Factory organizovat přesun dat a zpracování a proveďte spuštění dávky pomocí výukové počítače Azure. Ukážeme si nejvyšší úrovně:

1. Vytvoření služby Azure počítače výukové propojené. Budete potřebovat:
    1. **Identifikátor URI požadavku** na spuštění dávky rozhraní API. Identifikátor URI požádat o najdete po kliknutí na odkaz **DÁVKU spuštění** na webové stránce služby.
    1. **Rozhraní API klíč** pro publikované webové služby Azure počítače učení. Po kliknutí na webové služby, které jste publikovali můžete najít klíč rozhraní API. 
 2. Použití **AzureMLBatchExecution** aktivity.

    ![Řídicí panel výukové počítače](./media/data-factory-azure-ml-batch-execution-activity/AzureMLDashboard.png)

    ![Dávkové URI](./media/data-factory-azure-ml-batch-execution-activity/batch-uri.png)


### <a name="scenario-experiments-using-web-service-inputsoutputs-that-refer-to-data-in-azure-blob-storage"></a>Scénář: Pokusy pomocí webové služby vstupy/výstupy, které odkazují na data v úložišti objektů Blob Azure
V tomto scénáři Azure počítače výukové webová služba díky použití dat ze souboru v úložišti objektů blob Azure předpovědí a výsledek předpovídání pomocí uloží do úložiště objektů blob. Následující JSON definuje Data Factory profilace s AzureMLBatchExecution aktivity. Aktivity má datovou sadu **DecisionTreeInputBlob** předávat na vstupu a **DecisionTreeResultBlob** jako výstup. **DecisionTreeInputBlob** předaných jako vstup webové služby pomocí vlastnosti **webServiceInput** JSON. **DecisionTreeResultBlob** předána jako výstup webové služby pomocí vlastnosti **webServiceOutputs** JSON.  

> [AZURE.IMPORTANT] 
> Pokud webová služba trvá více vstupy, použijte místo použití **webServiceInput**vlastnost **webServiceInputs** . Naleznete v části [Webová služba vyžaduje více vstupů](#web-service-requires-multiple-inputs) pro příklad použití vlastnost webServiceInputs.
>  
> Datové sady, které se odkazuje podle **webServiceInput**/**webServiceInputs** a **webServiceOutputs** vlastnosti ( **typeProperties**) musí taky být součástí aktivity **vstupů** a **uloží**.
> 
> V Azure ML pokusu webové služby vstupní a výstupní porty a protokoly globální parametry mít výchozí názvy ("input1", "input2"), které je možné upravit. Názvy, které používáte pro webServiceInputs webServiceOutputs, nastavení a globalParameters se musí přesně shodovat názvy při pokusech. Na stránce Nápověda spuštění dávky pro Azure ML koncový bod pro ověření očekávané mapování můžete zobrazit ukázkové datové žádost. 


    {
      "name": "PredictivePipeline",
      "properties": {
        "description": "use AzureML model",
        "activities": [
          {
            "name": "MLActivity",
            "type": "AzureMLBatchExecution",
            "description": "prediction analysis on batch input",
            "inputs": [
              {
                "name": "DecisionTreeInputBlob"
              }
            ],
            "outputs": [
              {
                "name": "DecisionTreeResultBlob"
              }
            ],
            "linkedServiceName": "MyAzureMLLinkedService",
            "typeProperties":
            {
                "webServiceInput": "DecisionTreeInputBlob",
                "webServiceOutputs": {
                    "output1": "DecisionTreeResultBlob"
                }                
            },
            "policy": {
              "concurrency": 3,
              "executionPriorityOrder": "NewestFirst",
              "retry": 1,
              "timeout": "02:00:00"
            }
          }
        ],
        "start": "2016-02-13T00:00:00Z",
        "end": "2016-02-14T00:00:00Z"
      }
    }

> [AZURE.NOTE] Pouze vstupů a výstupů aktivity AzureMLBatchExecution lze předat jako parametry webové služby. DecisionTreeInputBlob je výše úryvek JSON vstup AzureMLBatchExecution činnost, která předané jako vstup k webové službě webServiceInput parametrem.   

### <a name="example"></a>Příklad

Tento příklad používá Azure úložiště pro uložení vstupní a výstupní data. 

Doporučujeme absolvovat [vytvořit svůj první kanálem k odesílání zpráv s Data Factory] [ adf-build-1st-pipeline] výuková před přechodem prostřednictvím v tomto příkladu. Použití editoru Factory dat k vytvoření Data Factory artefakty (propojené služeb, datové sady, kanálem k odesílání zpráv) v tomto příkladu.   
 

1. Vytvořte **propojené služby** **Azure úložiště**. Pokud jsou vstupní a výstupní soubory v různých úložiště účty, musíte dvě propojené služby. Tady je příklad JSON:

        {
          "name": "StorageLinkedService",
          "properties": {
            "type": "AzureStorage",
            "typeProperties": {
              "connectionString": "DefaultEndpointsProtocol=https;AccountName=[acctName];AccountKey=[acctKey]"
            }
          }
        }

2. Vytvořte **vstupní** Azure Data Factory **datovou sadu**. Na rozdíl od některé další Data Factory datové sady musí obsahovat tyto datové sady **cesta_ke_složce** a **název souboru** hodnot. Pomocí rozdělení způsobilo každé spuštění dávku (každý výseč) pro zpracování nebo vytvořit jedinečný vstupní a výstupní soubory. Budete muset obsahovat některé nadřazeného aktivity transformace zadání do formátu CSV a jeho umístění v účtu úložiště jednotlivých výsečí. V takovém případě by obsahuje nastavení **externího** a **externalData** vidět v následujícím příkladu, a vaše DecisionTreeInputBlob bude výstupní datovou sadu různé činnosti.

        {
          "name": "DecisionTreeInputBlob",
          "properties": {
            "type": "AzureBlob",
            "linkedServiceName": "StorageLinkedService",
            "typeProperties": {
              "folderPath": "azuremltesting/input",
              "fileName": "in.csv",
              "format": {
                "type": "TextFormat",
                "columnDelimiter": ","
              }
            },
            "external": true,
            "availability": {
              "frequency": "Day",
              "interval": 1
            },
            "policy": {
              "externalData": {
                "retryInterval": "00:01:00",
                "retryTimeout": "00:10:00",
                "maximumRetry": 3
              }
            }
          }
        }
    
    Soubor csv vstupní musí mít řádek záhlaví sloupců. Pokud používáte **Aktivity kopii** Pokud chcete vytvořit/přesuňte csv úložiště objektů blob, je třeba nastavit vlastnost jímky **blobWriterAddHeader** **logickou hodnotu PRAVDA**. Příklad:
    
         sink: 
         {
             "type": "BlobSink",     
             "blobWriterAddHeader": true 
         }
     
    Pokud soubor csv nemá řádek záhlaví, může se zobrazit tato chyba: **chyby ve aktivity: Chyba při čtení řetězec. Neočekávané token: StartObject. Cesta ", řádek 1, pozice 1**.
3. Vytvořte **výstup** Azure Data Factory **datovou sadu**. Tento příklad používá k vytvoření cesty jedinečné výstup pro každé spuštění výseč rozdělení. Bez oddílů, aktivitu by přepsat soubor.

        {
          "name": "DecisionTreeResultBlob",
          "properties": {
            "type": "AzureBlob",
            "linkedServiceName": "StorageLinkedService",
            "typeProperties": {
              "folderPath": "azuremltesting/scored/{folderpart}/",
              "fileName": "{filepart}result.csv",
              "partitionedBy": [
                {
                  "name": "folderpart",
                  "value": {
                    "type": "DateTime",
                    "date": "SliceStart",
                    "format": "yyyyMMdd"
                  }
                },
                {
                  "name": "filepart",
                  "value": {
                    "type": "DateTime",
                    "date": "SliceStart",
                    "format": "HHmmss"
                  }
                }
              ],
              "format": {
                "type": "TextFormat",
                "columnDelimiter": ","
              }
            },
            "availability": {
              "frequency": "Day",
              "interval": 15
            }
          }
        }

4. Vytvoření **propojené služby** typu: **AzureMLLinkedService**, poskytuje rozhraní API klíče a modelu URL spuštění dávky.
        
        {
          "name": "MyAzureMLLinkedService",
          "properties": {
            "type": "AzureML",
            "typeProperties": {
              "mlEndpoint": "https://[batch execution endpoint]/jobs",
              "apiKey": "[apikey]"
            }
          }
        }
5. Nakonec vytváříte kanálu obsahující **AzureMLBatchExecution** aktivity. Za běhu provede kanálem k odesílání zpráv podle těchto kroků:
    1. Získá vstupní soubor z vstupní datové sady.
    2. Spustí dávku spuštění Azure počítače výukové rozhraní API
    3. Zkopíruje výstup spuštění dávky objektů blob zadaných v sadě dat výstupu. 

    > [AZURE.NOTE] Aktivita AzureMLBatchExecution může mít žádný nebo více vstupů a jeden nebo více výstupů.

        {
          "name": "PredictivePipeline",
          "properties": {
            "description": "use AzureML model",
            "activities": [
              {
                "name": "MLActivity",
                "type": "AzureMLBatchExecution",
                "description": "prediction analysis on batch input",
                "inputs": [
                  {
                    "name": "DecisionTreeInputBlob"
                  }
                ],
                "outputs": [
                  {
                    "name": "DecisionTreeResultBlob"
                  }
                ],
                "linkedServiceName": "MyAzureMLLinkedService",
                "typeProperties":
                {
                    "webServiceInput": "DecisionTreeInputBlob",
                    "webServiceOutputs": {
                        "output1": "DecisionTreeResultBlob"
                    }                
                },
                "policy": {
                  "concurrency": 3,
                  "executionPriorityOrder": "NewestFirst",
                  "retry": 1,
                  "timeout": "02:00:00"
                }
              }
            ],
            "start": "2016-02-13T00:00:00Z",
            "end": "2016-02-14T00:00:00Z"
          }
        }

    **Začátek** a **Konec** data a času musí být ve [formátu ISO](http://en.wikipedia.org/wiki/ISO_8601). Příklad: 2014-10 – 14T16:32:41Z. **Koncový** čas je nepovinný krok. Pokud nezadáte hodnoty pro vlastnost **Ukončit** , se vypočítá jako "**zahájení + 48 hodin.**" Jako hodnotu pro vlastnost **Ukončit** spustíte kanálu donekonečna udržovat zadejte **9999-09-09** . Přečtěte si článek [JSON skriptování](https://msdn.microsoft.com/library/dn835050.aspx) podrobné informace o vlastnostech JSON.

    > [AZURE.NOTE] Určení vstup pro AzureMLBatchExecution aktivity vynechán. 

### <a name="scenario-experiments-using-readerwriter-modules-to-refer-to-data-in-various-storages"></a>Scénář: Pokusy pomocí čtečky/zapisovací moduly odkazují na data v různých úložiště

Jiné běžné situace při vytváření Azure ML pokusy, je použít čtečka a Autor moduly. Modul čtečka slouží k načtení dat do pokus a zapisovací modul je ukládat data z vaší pokusy. Podrobnosti o čtečka a Autor moduly tématech [čtečka](https://msdn.microsoft.com/library/azure/dn905997.aspx) a [Autor](https://msdn.microsoft.com/library/azure/dn905984.aspx) na knihovna MSDN.     

Při použití čtečky a Autor moduly, je vhodné použít parametr webové služby pro každou vlastnost moduly čtečka nebo Redaktor. Tyto webové parametry umožňují konfigurovat hodnoty za běhu. Můžete například vytvořit pokus pomocí modulu reader, který používá databáze SQL Azure: XXX.database.windows.net. Po nasazení webové služby, které chcete povolit spotřebitele webové služby, chcete-li zadat jinou Azure SQL Server s názvem YYY.database.windows.net. Můžete použít parametr webové služby umožňující tuto hodnotu na konfigurovat.

> [AZURE.NOTE] Web služby vstupní a výstupní se liší od parametry webové služby. V první scénář jste viděli, jak vstupní a výstupní lze zadat Azure ML webové služby. V tomto scénáři předat parametry webové služby, které odpovídají vlastnosti čtečka/zapisovací moduly. 

Podívejme se na scénáře pro používání parametry webové služby. Máte nasazeném Azure počítače výukové webové služby, který používá modulu čtečka číst data z jednoho zdroje dat podporované službou Azure počítače učení (například: databáze SQL Azure). Po provedení spuštění dávky výsledky jsou vytvořeny pomocí zapisovací modul (databáze SQL Azure).  V pokusy jsou definované žádné webové služby vstupů a výstupů. V tomto případě doporučujeme konfigurace parametry příslušné webové služby pro čtečky a Autor moduly. Tuto konfiguraci umožňuje čtečka/pisatele moduly konfiguraci při použití AzureMLBatchExecution aktivity. Zadejte parametry webové služby v části **globalParameters** činnosti JSON následujícím způsobem. 


    "typeProperties": {
        "globalParameters": {
            "Param 1": "Value 1",
            "Param 2": "Value 2"
        }
    }

Taky můžete použít [Funkce Factory dat](https://msdn.microsoft.com/library/dn835056.aspx) ve předávání hodnot pro parametry webové služby, jak je vidět v následujícím příkladu:

    "typeProperties": {
        "globalParameters": {
           "Database query": "$$Text.Format('SELECT * FROM myTable WHERE timeColumn = \\'{0:yyyy-MM-dd HH:mm:ss}\\'', Time.AddHours(WindowStart, 0))"
        }
    }
 
> [AZURE.NOTE] Webu služby rozlišují velká, tak zajistit, že jména, která určíte v aktivity JSON shoduje s vystaveným webové služby. 

### <a name="using-a-reader-module-to-read-data-from-multiple-files-in-azure-blob"></a>Načtení dat z víc souborů v objektů Blob Azure pomocí modulu Readeru
Velký dat potrubí s činnostmi, jako je Prasátko a podregistru můžete vytvořit jednu nebo více výstupních bez přípony souborů. Třeba když zadáte externí tabulku podregistru, data pro externí tabulku podregistru mohou být uložena v úložišti objektů blob Azure pomocí následující 000000_0 název. Pomocí modulu čtečka v pokus zobrazíte více souborů a účely jejich předpovědí. 

Pokud chcete použít modulu čtečka pokus výukové počítače Azure, můžete určit objektů Blob Azure jako vstup. Soubory v úložišti objektů blob Azure může být výstupní soubory (Příklad: 000000_0), která je vyrobeno skriptem Prasátko a podregistru spuštěna HDInsight. Modul čtečka umožňuje jen pro čtení souborů (bez přípony) konfigurace **cestu do kontejneru, adresář/objektů blob**. **Cesta k kontejneru** odkazuje na kontejner a **adresáře/blob** odkazuje na složku obsahující soubory, jak je znázorněno na následujícím obrázku. Hvězdičky, \*) **Určuje, že všechny soubory v kontejneru/složku (to znamená, data/aggregateddata/rok = 2014/měsíc-6 /\*)** jako součást testu se budou číst.

![Vlastnosti objektů Blob Azure](./media/data-factory-create-predictive-pipelines/azure-blob-properties.png)


### <a name="example"></a>Příklad 
#### <a name="pipeline-with-azuremlbatchexecution-activity-with-web-service-parameters"></a>Příležitostí s AzureMLBatchExecution aktivity s parametry webové služby

    {
      "name": "MLWithSqlReaderSqlWriter",
      "properties": {
        "description": "Azure ML model with sql azure reader/writer",
        "activities": [
          {
            "name": "MLSqlReaderSqlWriterActivity",
            "type": "AzureMLBatchExecution",
            "description": "test",
            "inputs": [
              {
                "name": "MLSqlInput"
              }
            ],
            "outputs": [
              {
                "name": "MLSqlOutput"
              }
            ],
            "linkedServiceName": "MLSqlReaderSqlWriterDecisionTreeModel",
            "typeProperties":
            {
                "webServiceInput": "MLSqlInput",
                "webServiceOutputs": {
                    "output1": "MLSqlOutput"
                }
                "globalParameters": {
                    "Database server name": "<myserver>.database.windows.net",
                    "Database name": "<database>",
                    "Server user account name": "<user name>",
                    "Server user account password": "<password>"
                }              
            },
            "policy": {
              "concurrency": 1,
              "executionPriorityOrder": "NewestFirst",
              "retry": 1,
              "timeout": "02:00:00"
            },
          }
        ],
        "start": "2016-02-13T00:00:00Z",
        "end": "2016-02-14T00:00:00Z"
      }
    }
 
V tomto příkladu JSON:

- Nasazeném Azure počítače výukové webová služba používá pro čtení a zápis dat z/k databázi SQL Azure čtení a zápis modulu. Tato webová služba poskytuje následující čtyři parametry: název serveru, název databáze, název serveru uživatelského účtu a Server heslo databáze.  
- **Začátek** a **Konec** data a času musí být ve [formátu ISO](http://en.wikipedia.org/wiki/ISO_8601). Příklad: 2014-10 – 14T16:32:41Z. **Koncový** čas je nepovinný krok. Pokud nezadáte hodnoty pro vlastnost **Ukončit** , se vypočítá jako "**zahájení + 48 hodin.**" Jako hodnotu pro vlastnost **Ukončit** spustíte kanálu donekonečna udržovat zadejte **9999-09-09** . Přečtěte si článek [JSON skriptování](https://msdn.microsoft.com/library/dn835050.aspx) podrobné informace o vlastnostech JSON.

### <a name="other-scenarios"></a>Další scénáře

#### <a name="web-service-requires-multiple-inputs"></a>Webová služba vyžaduje více vstupů
Pokud webová služba trvá více vstupy, použijte místo použití **webServiceInput**vlastnost **webServiceInputs** . Datové sady, které se odkazuje podle **webServiceInputs** musí být součástí aktivity **vstupů**.
 
V Azure ML pokusu webové služby vstupní a výstupní porty a protokoly globální parametry mít výchozí názvy ("input1", "input2"), které je možné upravit. Názvy, které používáte pro webServiceInputs webServiceOutputs, nastavení a globalParameters se musí přesně shodovat názvy při pokusech. Na stránce Nápověda spuštění dávky pro Azure ML koncový bod pro ověření očekávané mapování můžete zobrazit ukázkové datové žádost.


    {
        "name": "PredictivePipeline",
        "properties": {
            "description": "use AzureML model",
            "activities": [{
                "name": "MLActivity",
                "type": "AzureMLBatchExecution",
                "description": "prediction analysis on batch input",
                "inputs": [{
                    "name": "inputDataset1"
                }, {
                    "name": "inputDataset2"
                }],
                "outputs": [{
                    "name": "outputDataset"
                }],
                "linkedServiceName": "MyAzureMLLinkedService",
                "typeProperties": {
                    "webServiceInputs": {
                        "input1": "inputDataset1",
                        "input2": "inputDataset2"
                    },
                    "webServiceOutputs": {
                        "output1": "outputDataset"
                    }
                },
                "policy": {
                    "concurrency": 3,
                    "executionPriorityOrder": "NewestFirst",
                    "retry": 1,
                    "timeout": "02:00:00"
                }
            }],
            "start": "2016-02-13T00:00:00Z",
            "end": "2016-02-14T00:00:00Z"
        }
    }

#### <a name="web-service-does-not-require-an-input"></a>Webová služba nevyžaduje vstup

Azure ML dávku spuštění webové služby mohou sloužit k spouštění pracovních postupů, R nebo Python příklad skriptů, které nevyžadují nějaké vstupy. Nebo testu je možné nakonfigurovat tak pomocí modulu Reader, který nezobrazuje žádné GlobalParameters. V takovém případě aktivity AzureMLBatchExecution by nastavit takto:

    {
        "name": "scoring service",
        "type": "AzureMLBatchExecution",
        "outputs": [
            {
                "name": "myBlob"
            }
        ],
        "typeProperties": {
            "webServiceOutputs": {
                "output1": "myBlob"
            }              
         },
        "linkedServiceName": "mlEndpoint",
        "policy": {
            "concurrency": 1,
            "executionPriorityOrder": "NewestFirst",
            "retry": 1,
            "timeout": "02:00:00"
        }
    },
   

#### <a name="web-service-does-not-require-an-inputoutput"></a>Webová služba nevyžaduje vstupní a výstupní
Webové služby Azure ML dávku spuštění nemá žádný webová služba výstup nakonfigurované. V tomto příkladu nemůže žádným způsobem webová služba vstupní ani výstup ani nakonfigurované všechny GlobalParameters. Přesto se řídí výstup nakonfigurovaný na vlastní aktivity, ale není zadán jako webServiceOutput.

    {
        "name": "retraining",
        "type": "AzureMLBatchExecution",
        "outputs": [
            {
                "name": "placeholderOutputDataset"
            }
        ],
        "typeProperties": {
         },
        "linkedServiceName": "mlEndpoint",
        "policy": {
            "concurrency": 1,
            "executionPriorityOrder": "NewestFirst",
            "retry": 1,
            "timeout": "02:00:00"
        }
    },

#### <a name="web-service-uses-readers-and-writers-and-the-activity-runs-only-when-other-activities-have-succeeded"></a>Webové služby používá čteček a autoři a aktivity spuštěn pouze v případě, že úspěšně jinou práci.

Azure ML webové služby čtečka a Autor moduly může být nakonfigurované pro spuštění pomocí hesla nebo bez jakékoli GlobalParameters. Chcete však vložení volání služby v kanálu, který používá datovou sadu závislosti vyvolat službu pouze v případě, že některé nadřazeného zpracování dokončil. Některé akce můžete aktivovat také po spuštění dávky dokončení pomocí tento přístup. V takovém případě můžete vyjádřit pomocí vstupů aktivity a výstupy bez pojmenování libovolné z nich jako vstupů webové služby nebo výstupy závislosti.

    {
        "name": "retraining",
        "type": "AzureMLBatchExecution",
        "inputs": [
            {
                "name": "upstreamData1"
            },
            {
                "name": "upstreamData2"
            }
        ],
        "outputs": [
            {
                "name": "downstreamData"
            }
        ],
        "typeProperties": {
         },
        "linkedServiceName": "mlEndpoint",
        "policy": {
            "concurrency": 1,
            "executionPriorityOrder": "NewestFirst",
            "retry": 1,
            "timeout": "02:00:00"
        }
    },

**Takeaways** jsou:

-   Pokud koncový bod experiment používá webServiceInput: je znázorněn datovou sadu objektů blob a je součástí vstupů aktivity a vlastnost webServiceInput. V opačném vlastnost webServiceInput vynecháte. 
-   Pokud koncový bod experiment používá webServiceOutput(s): představují datové sady objektů blob a jsou součástí výstupy aktivity a vlastnost webServiceOutputs. Výstup aktivity a webServiceOutputs jsou namapované podle názvu každé výstup v testu. V opačném vlastnost webServiceOutputs vynechán.
-   Pokud koncový bod experiment zpřístupní globalParameter(s), jsou uvedeny v vlastnosti globalParameters aktivity jako dvojice klíčů. V opačném vlastnost globalParameters vynechán. Klávesy se rozlišují velká a malá písmena. [Funkce Azure Data Factory](data-factory-scheduling-and-execution.md#data-factory-functions-reference) lze do pole hodnoty. 
- Další datové sady může být součástí aktivity vlastnosti vstupů a výstupů bez odkazovaný v typeProperties aktivity. Tyto datové sady určují způsob spuštění pomocí výsečí závislosti ale ignorovány jinak AzureMLBatchExecution aktivity. 


## <a name="updating-models-using-update-resource-activity"></a>Aktualizace modely použití aktualizace zdroje aktivity
V průběhu času prediktivní modely v Azure ML bodování pokusy muset být retrained pomocí nového vstupní datové sady. Po dokončení s přeškolení chcete aktualizovat hodnocení webová služba s modelem retrained ML. Typický postup, jak povolit rekvalifikaci a aktualizace modely Azure ML pomocí webových služeb jsou: 

1. Vytvoření pokus v [Azure ML Studio](https://studio.azureml.net). 
2. Jakmile budete spokojeni s modelem, pomocí Azure ML Studio publikovat webové služby pro obě **experimentovat školení** a bodování /**prediktivní testu**.

Následující tabulka popisuje webové služby v tomto příkladě použita.  Podrobnosti najdete v článku [Retrain výukové počítače modely programově](../machine-learning/machine-learning-retrain-models-programmatically.md) .

| Typ webové služby | Popis 
| :------------------ | :---------- 
| **Školicí kurzy webové služby** | Načte data školení a vytvoří školení modely. Výstup rekvalifikačního je soubor .ilearner v úložišti objektů Blob Azure.  **Výchozí koncový bod** se automaticky vytvoří při publikování experiment školení jako webové služby. Můžete vytvořit další koncové body, ale v příkladu jsou použity jenom výchozí koncový bod |
| **Hodnocení webové služby** | Obdrží příklady neoznačeném dat a chce předpovědí. Výstup předpovědí může mít různých formulářů, například souboru .csv nebo řádky v databázi Azure SQL, v závislosti na konfiguraci testu. Výchozí koncový bod se automaticky vytvoří při publikování prediktivní testu jako webové služby. Vytvoření druhého **jiného než výchozího a aktualizovatelné koncový bod** pomocí [Azure portálu](https://manage.windowsazure.com). Můžete vytvořit další koncové body, ale tento příklad používá pouze jeden aktualizovatelné koncový bod jiné než výchozí. Naleznete v článku [Vytvoření koncové body](../machine-learning/machine-learning-create-endpoint.md) postup.       
 
Následující obrázek znázorňuje vztah mezi školení a bodování koncové body v Azure ML. 

![Webové služby](./media/data-factory-azure-ml-batch-execution-activity/web-services.png)


Vyvoláte **školení webové služby** pomocí **Azure ML dávku spuštění aktivity**. Volání webové služby školení je shodný s volání webové služby Azure ML (bodování webová služba) pro bodování data. V předchozích částech vysvětlovat, jak vyvolat webové služby Azure ML z Azure Data Factory kanálu podrobně. 
  
Vyvolání **bodování webové služby** pomocí **Aktivity zdroje aktualizovat ML Azure** k aktualizaci modelu nově školení webové služby. Jak je uvedeno v předchozí tabulce, musíte vytvořit a použití jiného než výchozího aktualizovatelné koncového bodu. Kromě toho aktualizujte všechny existující odkazované služby u výrobce dat použití jiného než výchozího koncového bodu tak, aby vždy používat nejnovější retrained modelu. 

Následující situaci najdete další informace. Je příklad rekvalifikací a Azure ML modely z Azure Data Factory kanálu. 
 
### <a name="scenario-retraining-and-updating-an-azure-ml-model"></a>Scénář: rekvalifikací a modelu Azure ML
Tato část obsahuje ukázkové kanálem k odesílání zpráv, který používá **spuštění dávky ML Azure aktivity** se přeškolit modelu. **Aktivity Azure ML aktualizace zdroje** kanálu taky používá k aktualizaci modelu bodování webové služby. V části poskytuje také JSON fragmenty propojené služby, datových sad a kanálem k odesílání zpráv v příkladu. 

Tady je zobrazení diagramu kanálu vzorku. Jak můžete vidět, aktivity spuštění dávky ML Azure zabírá vstupní školení a výstup školení (iLearner soubor). Aktivita zdroje Azure ML aktualizace trvá tento výstup školení a aktualizuje modelu v bodování koncový bod webové služby. Aktivity zdroje aktualizace nevytvoří jakýkoli výstup. PlaceholderBlob je právě formální výstup datovou sadu, která je potřebná službou Azure Data Factory spuštění profilace. 

![diagram kanálem k odesílání zpráv](./media/data-factory-azure-ml-batch-execution-activity/update-activity-pipeline-diagram.png)


#### <a name="azure-blob-storage-linked-service"></a>Úložiště objektů Blob Azure propojené služby:
Úložiště Azure obsahuje následující údaje:

- školení data. Zadávání dat pro službu Azure ML školení web.  
- soubor iLearner. Výstup z webové služby Azure ML školení. Tento soubor je také vstupů pro zdroj aktualizace aktivity.  
   
Tady je ukázka definice JSON propojené služby: 

    {
        "name": "StorageLinkedService",
        "properties": {
            "type": "AzureStorage",
            "typeProperties": {
                "connectionString": "DefaultEndpointsProtocol=https;AccountName=name;AccountKey=key"
            }
        }
    }


#### <a name="training-input-dataset"></a>Školení vstupní datovou sadu:
Následující datové představuje vstupní školení data webové služby Azure ML školení. Spuštění dávky ML Azure aktivity, která bude tento datovou sadu jako vstup. 

    {
        "name": "trainingData",
        "properties": {
            "type": "AzureBlob",
            "linkedServiceName": "StorageLinkedService",
            "typeProperties": {
                "folderPath": "labeledexamples",
                "fileName": "labeledexamples.arff",
                "format": {
                    "type": "TextFormat"
                }
            },
            "availability": {
                "frequency": "Week",
                "interval": 1
            },
            "policy": {          
                "externalData": {
                    "retryInterval": "00:01:00",
                    "retryTimeout": "00:10:00",
                    "maximumRetry": 3
                }
            }
        }
    }

#### <a name="training-output-dataset"></a>Výstup datovou sadu školení:
Následující datové představuje výstupní soubor iLearner z webové služby Azure ML školení. Spuštění aktivity Azure ML dávku vytvoří tento datovou sadu. Tento datovou sadu je také vstupní aktivitou Azure ML aktualizace zdroje.

    {
        "name": "trainedModelBlob",
        "properties": {
            "type": "AzureBlob",
            "linkedServiceName": "StorageLinkedService",
            "typeProperties": {
                "folderPath": "trainingoutput",
                "fileName": "model.ilearner",
                "format": {
                    "type": "TextFormat"
                }
            },
            "availability": {
                "frequency": "Week",
                "interval": 1
            }
        }
    }

#### <a name="linked-service-for-azure-ml-training-endpoint"></a>Propojené služby Azure ML školení koncového bodu 
Následující úryvek JSON definuje služby Azure počítače výukové propojené odkazující na výchozí koncový bod školení webové služby. 

    {   
        "name": "trainingEndpoint",
        "properties": {
            "type": "AzureML",
            "typeProperties": {
                "mlEndpoint": "https://ussouthcentral.services.azureml.net/workspaces/xxx/services/--training experiment--/jobs",
                "apiKey": "myKey"
            }
        }
    }

V **Azure ML Studio**řiďte se kroky k získání hodnot pro **mlEndpoint** a **apiKey**:

1. Klikněte v nabídce nalevo na **Webové služby** .
2. Klikněte na možnost **školení webové služby** v seznamu webové služby. 
3. Klepněte na tlačítko Kopírovat vedle **rozhraní API klíč** textové pole. Vložte klávesu ve schránce sady dat Factory JSON editor.
4. V **Azure ML studio**klikněte na odkaz **Spuštění dávky** .
5. Zkopírujte **Požádat o URI** z části **požádat o** a vložit ho do editoru Data Factory JSON.   


#### <a name="linked-service-for-azure-ml-updatable-scoring-endpoint"></a>Propojené služby Azure ML aktualizovatelné bodování koncového bodu:
Následující úryvek JSON definuje služby Azure počítače výukové propojené odkazující na jiné než výchozí aktualizovatelné koncový bod bodování webové služby.  

    {
        "name": "updatableScoringEndpoint2",
        "properties": {
            "type": "AzureML",
            "typeProperties": {
                "mlEndpoint": "https://ussouthcentral.services.azureml.net/workspaces/xxx/services/--scoring experiment--/jobs",
                "apiKey": "endpoint2Key",
                "updateResourceEndpoint": "https://management.azureml.net/workspaces/xxx/webservices/--scoring experiment--/endpoints/endpoint2"
            }
        }
    }


Před vytvoření a nasazení Azure ML propojeny služby, postupujte podle pokynů v článku [Vytvoření koncové body](../machine-learning/machine-learning-create-endpoint.md) vytvoření druhého (jiné než výchozí a aktualizovatelné) koncového bodu bodování webové služby.

Po vytvoření koncového bodu aktualizovatelné jiné než výchozí, postupujte takto:

- Klikněte na **Spuštění dávky** zobrazíte URI hodnotu pro vlastnost **mlEndpoint** JSON.
- Klikněte na odkaz **Zdroj aktualizace** zobrazíte hodnotu URI pro vlastnost **updateResourceEndpoint** JSON. Rozhraní API klíčem je na stránce koncového bodu (v pravý dolní roh). 

![aktualizovatelné koncový bod](./media/data-factory-azure-ml-batch-execution-activity/updatable-endpoint.png)

 
#### <a name="placeholder-output-dataset"></a>Zástupný symbol výstup datovou sadu:
Aktivity Azure ML aktualizace zdroje negeneruje žádný výstup. Azure Data Factory však vyžaduje výstup datovou sadu k řízení plánu kanálů. Proto používáme datovou sadu figurínu/zástupný symbol v tomto příkladu.  

    {
        "name": "placeholderBlob",
        "properties": {
            "availability": {
                "frequency": "Week",
                "interval": 1
            },
            "type": "AzureBlob",
            "linkedServiceName": "StorageLinkedService",
            "typeProperties": {
                "folderPath": "any",
                "format": {
                    "type": "TextFormat"
                }
            }
        }
    }


#### <a name="pipeline"></a>Kanálem k odesílání zpráv
Kanálu má dvě aktivity: **AzureMLBatchExecution** a **AzureMLUpdateResource**. Spuštění dávky ML Azure aktivity trvá dat školení předávat na vstupu a vytvoří soubor iLearner jako výstup. Aktivity spustí webová služba školení (školení experiment vystavit jako webové služby) s údaji vstupní školení volání a přijímání souborů ilearner webservice. PlaceholderBlob je právě formální výstup datovou sadu, která je potřebná službou Azure Data Factory spuštění profilace. 

![diagram kanálem k odesílání zpráv](./media/data-factory-azure-ml-batch-execution-activity/update-activity-pipeline-diagram.png)


    {
        "name": "pipeline",
        "properties": {
            "activities": [
                {
                    "name": "retraining",
                    "type": "AzureMLBatchExecution",
                    "inputs": [
                        {
                            "name": "trainingData"
                        }
                    ],
                    "outputs": [
                        {
                            "name": "trainedModelBlob"
                        }
                    ],
                    "typeProperties": {
                        "webServiceInput": "trainingData",
                        "webServiceOutputs": {
                            "output1": "trainedModelBlob"
                        }              
                     },
                    "linkedServiceName": "trainingEndpoint",
                    "policy": {
                        "concurrency": 1,
                        "executionPriorityOrder": "NewestFirst",
                        "retry": 1,
                        "timeout": "02:00:00"
                    }
                },
                {
                    "type": "AzureMLUpdateResource",
                    "typeProperties": {
                        "trainedModelName": "Training Exp for ADF ML [trained model]",
                        "trainedModelDatasetName" :  "trainedModelBlob"
                    },
                    "inputs": [
                        {
                            "name": "trainedModelBlob"
                        }
                    ],
                    "outputs": [
                        {
                            "name": "placeholderBlob"
                        }
                    ],
                    "policy": {
                        "timeout": "01:00:00",
                        "concurrency": 1,
                        "retry": 3
                    },
                    "name": "AzureML Update Resource",
                    "linkedServiceName": "updatableScoringEndpoint2"
                }
            ],
            "start": "2016-02-13T00:00:00Z",
            "end": "2016-02-14T00:00:00Z"
        }
    }


### <a name="reader-and-writer-modules"></a>Redaktor moduly a Readeru

Běžné situace pro práci s parametry webové služby je použití Azure SQL čtení a zápis. Modul čtečka slouží k načtení dat do pokus z dat správu přístupových mimo Azure počítače výukové Studio. Na zapisovací modul je uložení dat z vaší pokusy do služby správy dat mimo Azure počítače výukové Studio.  

Podrobnosti o objektů Blob/Azure SQL Azure čtečka/zapisovací tématech [čtečka](https://msdn.microsoft.com/library/azure/dn905997.aspx) a [Autor](https://msdn.microsoft.com/library/azure/dn905984.aspx) na knihovna MSDN. Příklad v předchozí části používá objektů Blob Azure čtečka a Autor objektů Blob Azure. Tato část popisuje pomocí čtečky Azure SQL a zápis Azure SQL.


## <a name="frequently-asked-questions"></a>Nejčastější dotazy

**Q:** Mám najednou několik souborů generovaná Moje velký datové kanály. Můžete použít aktivity AzureMLBatchExecution k práci se soubory?

**A:** Ano. Naleznete v části **použití modulu čtenáře k načtení dat z víc souborů v objektů Blob Azure** podrobnosti. 

## <a name="azure-ml-batch-scoring-activity"></a>Azure dávku ML bodování aktivity
Pokud používáte aktivity **AzureMLBatchScoring** integrovat s Azure počítače výuka, doporučujeme používat nejnovější aktivitu **AzureMLBatchExecution** . 

Zavádí AzureMLBatchExecution aktivity v srpen 2015 verzi Azure SDK a Azure Powershellu.

Pokud chcete pokračovat v používání AzureMLBatchScoring aktivity, pokračujte ve čtení prostřednictvím v této části.  

### <a name="azure-ml-batch-scoring-activity-using-azure-storage-for-inputoutput"></a>Azure ML dávku bodování aktivity pomocí Azure úložiště pro vstupní a výstupní 

    {
      "name": "PredictivePipeline",
      "properties": {
        "description": "use AzureML model",
        "activities": [
          {
            "name": "MLActivity",
            "type": "AzureMLBatchScoring",
            "description": "prediction analysis on batch input",
            "inputs": [
              {
                "name": "ScoringInputBlob"
              }
            ],
            "outputs": [
              {
                "name": "ScoringResultBlob"
              }
            ],
            "linkedServiceName": "MyAzureMLLinkedService",
            "policy": {
              "concurrency": 3,
              "executionPriorityOrder": "NewestFirst",
              "retry": 1,
              "timeout": "02:00:00"
            }
          }
        ],
        "start": "2016-02-13T00:00:00Z",
        "end": "2016-02-14T00:00:00Z"
      }
    }

### <a name="web-service-parameters"></a>Parametry webové služby
Chcete-li zadat hodnoty parametrů webové služby, přidáte oddíl **typeProperties** do oddílu **AzureMLBatchScoringActivty** v kanálu JSON, jak je vidět v následujícím příkladu: 

    "typeProperties": {
        "webServiceParameters": {
            "Param 1": "Value 1",
            "Param 2": "Value 2"
        }
    }


Taky můžete použít [Funkce Factory dat](https://msdn.microsoft.com/library/dn835056.aspx) ve předávání hodnot pro parametry webové služby, jak je vidět v následujícím příkladu:

    "typeProperties": {
        "webServiceParameters": {
           "Database query": "$$Text.Format('SELECT * FROM myTable WHERE timeColumn = \\'{0:yyyy-MM-dd HH:mm:ss}\\'', Time.AddHours(WindowStart, 0))"
        }
    }
 
> [AZURE.NOTE] Webu služby rozlišují velká, tak zajistit, že jména, která určíte v aktivity JSON shoduje s vystaveným webové služby. 

## <a name="see-also"></a>Viz taky

- [Příspěvek na Azure blog: Začínáme s Azure Data Factory a Azure počítače výuka](https://azure.microsoft.com/blog/getting-started-with-azure-data-factory-and-azure-machine-learning-4/)





[adf-build-1st-pipeline]: data-factory-build-your-first-pipeline.md

[azure-machine-learning]: http://azure.microsoft.com/services/machine-learning/


 
