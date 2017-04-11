<properties
    pageTitle="Plánování a provádění s Data Factory | Microsoft Azure"
    description="Zjistěte, plánování a provádění aspekty model aplikace Azure Data Factory."
    services="data-factory"
    documentationCenter=""
    authors="spelluru"
    manager="jhubbard"
    editor="monicar"/>

<tags
    ms.service="data-factory"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/22/2016"
    ms.author="spelluru"/>

# <a name="data-factory-scheduling-and-execution"></a>Plánování Factory dat a spuštění
Tento článek vysvětluje plánování a provádění aspekty model aplikace Azure Data Factory. 

## <a name="prerequisites"></a>Zjistit předpoklady pro
V tomto článku se předpokládá seznámení se základy Data Factory aplikace model konceptů, včetně aktivity, kanály, propojené služeb a datové sady. Základní pojmy Azure Data Factory najdete v následujících článcích:

- [Úvod do Data Factory](data-factory-introduction.md)
- [Potrubí](data-factory-create-pipelines.md)
- [Datové sady](data-factory-create-datasets.md) 

## <a name="schedule-an-activity"></a>Naplánování aktivita

S částí Plánovač aktivity JSON můžete určit plán opakování pro aktivitu. Například můžete naplánovat aktivitu každou hodinu následujícím způsobem:

    "scheduler": {
        "frequency": "Hour",
        "interval": 1
    },  

![Příklad Plánovač](./media/data-factory-scheduling-and-execution/scheduler-example.png)

Jak je vidět v diagramu, určující plánu aktivity vytvoří řadu tumbling oken. Tumbling windows jsou řadu pevnou velikostí nepřekrývající, více sousedících časových intervalů. Tato okna logické tumbling aktivity se označují jako *windows aktivity*.

Aktuálně spouštěné okna aktivity dostanete se k časový interval spojené s oknem aktivity s systémové proměnné [WindowStart](data-factory-functions-variables.md#data-factory-system-variables) a [WindowEnd](data-factory-functions-variables.md#data-factory-system-variables) činnosti JSON. Můžete použít tyto proměnné pro jiné účely v aktivitách JSON. Například můžete je výběr dat ze vstupní a výstupní datové sady představující dat časové řady.

Vlastnost **Plánovač** podporuje stejné podvlastnosti jako vlastnost **dostupnost** datovou sadu. Podrobnosti najdete v části [dostupnost datovou sadu](data-factory-create-datasets.md#Availability) . Příklady: plánování v určitém časovém posun nebo nastavení režimu zarovnat zpracování na začátku nebo na konci interval pro okna aktivity.

Můžete zadat vlastnosti **Plánovač** aktivity, ale tato vlastnost je **nepovinná**. Pokud nezadáte vlastnosti, se musí shodovat cadence, který určíte v definici výstup datovou sadu. Datovou sadu výstup je v současné době co jednotky s plánem, třeba vytvoříte datovou sadu výstup i v případě aktivity nevytvoří jakýkoli výstup. Pokud aktivity bude jakýkoli vstup, můžete přejít vytváření vstupních datovou sadu.

## <a name="time-series-datasets-and-data-slices"></a>Časové řady datové sady a data výsečí

Dat časové řady je nepřetržité datových bodů tvořená obvykle po sobě jdoucí měření myší na časový interval. Běžné dat časové řady příklady data snímače a aplikace telemetrickými daty.

Data Factory zpracovávat spuštění řady dávkový způsobem s aktivity. Je obvykle opakované cadence jakou vstupní data přijde a výstupní data musí být vyrobeno. Tento cadence modelovat zadáním **dostupnost** v sadě dat:

    "availability": {
      "frequency": "Hour",
      "interval": 1
    },

Každou jednotku dat spotřebované množství a vyrobeno spuštěním aktivity se nazývá řez data. Na následujícím obrázku vidíte příklad aktivity s jednu datovou sadu vstupní a výstupní jednu datovou sadu. Tyto datové sady mají **dostupnost** nastavena na každou hodinu četnosti.

![Dostupnost Plánovač](./media/data-factory-scheduling-and-execution/availability-scheduler.png)

Předchozí diagram znázorňuje hodinové výsečí dat pro datovou sadu vstupní a výstupní. Diagram ukazuje tři vstupní výsečí připravené pro zpracování. Aktivity 10 a 11 dop probíhá, vytváření výseč výstup 10 a 11 dop.

Přístup k časový interval spojené s adresami vytvořené v datové sadě JSON s proměnné [SliceStart](data-factory-functions-variables.md#data-factory-system-variables) a [SliceEnd](data-factory-functions-variables.md#data-factory-system-variables)aktuální výsečí.

V současné době Data Factory vyžaduje plán podle aktivity přesně odpovídá plánu podle **dostupnosti** datovou sadu výstupu. Proto **WindowStart**, **WindowEnd**, **SliceStart**a **SliceEnd** vždy mapují na stejné časové období a jeden výstup výsečí.

Další informace o různých vlastnosti umožňující části Dostupnost najdete v článku [Vytvoření datové sady](data-factory-create-datasets.md).

## <a name="move-data-from-sql-database-to-blob-storage"></a>Přesunutí dat z databáze SQL k úložišti objektů Blob

Pojďme umístění některé věci společně a v akci vytvořením kanálů, které slouží ke kopírování dat z tabulky databáze SQL Azure k úložišti objektů Blob Azure každou hodinu.

**Vstup: Sady dat Azure SQL databáze**

    {
        "name": "AzureSqlInput",
        "properties": {
            "published": false,
            "type": "AzureSqlTable",
            "linkedServiceName": "AzureSqlLinkedService",
            "typeProperties": {
                "tableName": "MyTable"
            },
            "availability": {
                "frequency": "Hour",
                "interval": 1
            },
            "external": true,
            "policy": {}
        }
    }


**Četnost** je nastavený na **hodiny** a **interval** je nastavena na hodnotu **1** v části dostupnost.

**Výstup: Datovou sadu úložiště objektů Blob Azure**

    {
        "name": "AzureBlobOutput",
        "properties": {
            "published": false,
            "type": "AzureBlob",
            "linkedServiceName": "StorageLinkedService",
            "typeProperties": {
                "folderPath": "mypath/{Year}/{Month}/{Day}/{Hour}",
                "format": {
                    "type": "TextFormat"
                },
                "partitionedBy": [
                    {
                        "name": "Year",
                        "value": {
                            "type": "DateTime",
                            "date": "SliceStart",
                            "format": "yyyy"
                        }
                    },
                    {
                        "name": "Month",
                        "value": {
                            "type": "DateTime",
                            "date": "SliceStart",
                            "format": "%M"
                        }
                    },
                    {
                        "name": "Day",
                        "value": {
                            "type": "DateTime",
                            "date": "SliceStart",
                            "format": "%d"
                        }
                    },
                    {
                        "name": "Hour",
                        "value": {
                            "type": "DateTime",
                            "date": "SliceStart",
                            "format": "%H"
                        }
                    }
                ]
            },
            "availability": {
                "frequency": "Hour",
                "interval": 1
            }
        }
    }


**Četnost** je nastavený na **hodiny** a **interval** je nastavena na hodnotu **1** v části dostupnost.



**Aktivity: Aktivity kopírovat**

    {
        "name": "SamplePipeline",
        "properties": {
            "description": "copy activity",
            "activities": [
                {
                    "type": "Copy",
                    "name": "AzureSQLtoBlob",
                    "description": "copy activity",
                    "typeProperties": {
                        "source": {
                            "type": "SqlSource",
                            "sqlReaderQuery": "$$Text.Format('select * from MyTable where timestampcolumn >= \\'{0:yyyy-MM-dd HH:mm}\\' AND timestampcolumn < \\'{1:yyyy-MM-dd HH:mm}\\'', WindowStart, WindowEnd)"
                        },
                        "sink": {
                            "type": "BlobSink",
                            "writeBatchSize": 100000,
                            "writeBatchTimeout": "00:05:00"
                        }
                    },
                    "inputs": [
                        {
                            "name": "AzureSQLInput"
                        }
                    ],
                    "outputs": [
                        {
                            "name": "AzureBlobOutput"
                        }
                    ],
                    "scheduler": {
                        "frequency": "Hour",
                        "interval": 1
                    }
                }
            ],
            "start": "2015-01-01T08:00:00Z",
            "end": "2015-01-01T11:00:00Z"
        }
    }


Vzorku zobrazuje plánu aktivity a oddíly dostupnost datovou sadu nastavte hodinové četnosti. Vzorku ukazuje, jak můžete použít **WindowStart** a **WindowEnd** vyberte relevantních dat pro aktivitu spustit a jeho zkopírování do objektů blob s příslušnou **cesta_ke_složce**. **Cesta_ke_složce** je s parametry, aby obsahoval samostatné složky pro každou hodinu.

Tři výsečí mezi 8 – 11 dop spustit, data v databázi SQL Azure při následujícím způsobem:

![Ukázková vstupní](./media/data-factory-scheduling-and-execution/sample-input-data.png)

Po nasadí profilace objektů blob Azure je vyplněné následujícím způsobem:

-   Soubor cesta/2015/1/1/8/Data. &lt;Guid&gt;txt s daty

            10002345,334,2,2015-01-01 08:24:00.3130000
            10002345,347,15,2015-01-01 08:24:00.6570000
            10991568,2,7,2015-01-01 08:56:34.5300000

    > [AZURE.NOTE] &lt;Identifikátor GUID&gt; nahrazen skutečný identifikátor guid. Příklad názvu souboru: Data.bcde1348-7620-4f93-bb89-0eed3455890b.txt
-   Soubor cesta/2015/1/1/9/Data. &lt;Guid&gt;txt s daty:

            10002345,334,1,2015-01-01 09:13:00.3900000
            24379245,569,23,2015-01-01 09:25:00.3130000
            16777799,21,115,2015-01-01 09:47:34.3130000
-   Soubor cesta/2015/1/1/10/Data. &lt;Guid&gt;txt bez informací.


## <a name="active-period-for-pipeline"></a>Aktivní dobu kanálem k odesílání zpráv

[Vytváření potrubí](data-factory-create-pipelines.md) zavedená koncepci aktivní dobu kanálu nastavil nastavení vlastností **začátek** a **Konec** .

Můžete nastavit jiné datum zahájení aktivního období kanálem k odesílání zpráv v minulosti. Data Factory automaticky vypočítá (zpět výplně) všechny výseče dat v minulosti a jejich zpracování, nebude zahájen.

## <a name="parallel-processing-of-data-slices"></a>Paralelní zpracování dat výsečí
Můžete nakonfigurovat vyplní zpět dat výsečí proběhnout paralelně nastavením vlastnosti **souběžné** v části zásady aktivity JSON. Další informace o této vlastnosti najdete v článku [Vytvoření kanály](data-factory-create-pipelines.md).

## <a name="rerun-a-failed-data-slice"></a>Spusťte selhalo výseč 
Spuštění výsečí můžete sledovat bohaté a vizuální způsobem. Podrobnosti najdete v části [sledování a správa potrubí pomocí Azure portálu listy](data-factory-monitor-manage-pipelines.md) nebo [aplikace pro sledování a správa](data-factory-monitor-manage-app.md) .

Příklad, který ukazuje dvě aktivity. Activity1 vytvoří sadu dat časové řady s výsečí jako výstup, který je předávat na vstupu využívána Activity2 plodiny datovou sadu konečný výstup časové řady.

![Selhalo výseč](./media/data-factory-scheduling-and-execution/failed-slice.png)

Diagram ukazuje, že mimo tři poslední výsečí došlo k chybě vytváření výseč dopoledne 9-10 pro Dataset2. Data Factory automaticky sleduje závislost pro datovou sadu časové řady. Nespustí v důsledku toho aktivity spustit pro příjem řez 9-10: 00.

Datové Factory monitorování a správu nástroje nastavil, abyste k podrobnostem diagnostické protokoly pro selhalo řez snadno najít příčinou pro tento problém a opravný nástroj fix it. Po mít podařilo odstranit problém, můžete snadno začít aktivity spustit k vytvoření selhalo výsečí. Podrobné informace o tom, jak znovu spustit a vysvětlení stavu přechody výsečí dat najdete v článku [monitorování a správu potrubí pomocí Azure portálu listy](data-factory-monitor-manage-pipelines.md) nebo [aplikace pro sledování a správa](data-factory-monitor-manage-app.md).

Po spusťte výseč 9-10: 00 pro **Dataset2**Data Factory spustit pro závislé řez 9-10 dopoledne začíná konečný datovou sadu.

![Znovu spustit selhalo výseč](./media/data-factory-scheduling-and-execution/rerun-failed-slice.png)

## <a name="run-activities-in-a-sequence"></a>Spuštění aktivity v posloupnosti
Zřetězit dvě aktivity (spuštění aktivity jeden po druhém) nastavením datovou sadu výstup jeden aktivity jako vstupní datovou sadu jiné aktivitě. Činnosti může být ve stejném kanálu nebo v jiné kanály. Druhé činnosti spustí pouze po první dokončení úspěšně.

Například zvažte následující případ:

1.  Profilace P1 má aktivity A1, který vyžaduje vnější zadání datovou sadu D1 a vytvoří výstup datovou sadu D2.
2.  P2 kanálem k odesílání zpráv má aktivity A2, který vyžaduje zadání vstupních hodnot z datovou sadu D2 a vytvoří výstup datovou sadu D3.

V tomto scénáři aktivity A1 a A2 jsou v jiném kanály. Aktivity A1 spustí externí data je k dispozici a dosažení počet_plateb plánované dostupnosti. Aktivity A2 spustí plánované výsečí z D2 k dispozici a dosažení počet_plateb plánované dostupnosti. Pokud dojde k chybě v jednom výsečí datovou sadu D2, A2 nejde spustit pro tuto řez, dokud nebude k dispozici.

Zobrazení diagramu výsledek podobný jako na následujícím obrázku:

![Zřetězení aktivit najdete ve dvou potrubí](./media/data-factory-scheduling-and-execution/chaining-two-pipelines.png)

Jak jsme zmínili dříve, může být aktivit ve stejném kanálu. Zobrazení diagramu se obě aktivit najdete ve stejné potrubí výsledek podobný jako na následujícím obrázku:

![Zřetězení aktivit najdete ve stejném profilace](./media/data-factory-scheduling-and-execution/chaining-one-pipeline.png)

### <a name="copy-sequentially"></a>Zkopírujte postupně
Je možné provádět operace více kopií jeden po druhém způsobem sekvenční/objednali. Například může mít dvě kopie aktivity v kanálu (CopyActivity1 a CopyActivity2) s následující datové sady výstup zadávání dat:   

CopyActivity1

Vstupní: datovou sadu. Výstup: Dataset2.

CopyActivity2

Vstupní: Dataset2.  Výstup: Dataset3.

CopyActivity2 by byl spuštěn pouze v případě CopyActivity1 úspěšném a Dataset2 je k dispozici.

Tady je ukázka kanál JSON:

    {
        "name": "ChainActivities",
        "properties": {
            "description": "Run activities in sequence",
            "activities": [
                {
                    "type": "Copy",
                    "typeProperties": {
                        "source": {
                            "type": "BlobSource"
                        },
                        "sink": {
                            "type": "BlobSink",
                            "copyBehavior": "PreserveHierarchy",
                            "writeBatchSize": 0,
                            "writeBatchTimeout": "00:00:00"
                        }
                    },
                    "inputs": [
                        {
                            "name": "Dataset1"
                        }
                    ],
                    "outputs": [
                        {
                            "name": "Dataset2"
                        }
                    ],
                    "policy": {
                        "timeout": "01:00:00"
                    },
                    "scheduler": {
                        "frequency": "Hour",
                        "interval": 1
                    },
                    "name": "CopyFromBlob1ToBlob2",
                    "description": "Copy data from a blob to another"
                },
                {
                    "type": "Copy",
                    "typeProperties": {
                        "source": {
                            "type": "BlobSource"
                        },
                        "sink": {
                            "type": "BlobSink",
                            "writeBatchSize": 0,
                            "writeBatchTimeout": "00:00:00"
                        }
                    },
                    "inputs": [
                        {
                            "name": "Dataset2"
                        }
                    ],
                    "outputs": [
                        {
                            "name": "Dataset3"
                        }
                    ],
                    "policy": {
                        "timeout": "01:00:00"
                    },
                    "scheduler": {
                        "frequency": "Hour",
                        "interval": 1
                    },
                    "name": "CopyFromBlob2ToBlob3",
                    "description": "Copy data from a blob to another"
                }
            ],
            "start": "2016-08-25T01:00:00Z",
            "end": "2016-08-25T01:00:00Z",
            "isPaused": false
        }
    }

Všimněte si, že v příkladu datovou sadu výstup první činnosti kopie (Dataset2) zadaný jako vstup pro druhé činnosti. Proto druhý aktivity spuštěn pouze v případě, že hotovou datovou sadu výstup z první činnosti.  

V příkladu CopyActivity2 může obsahovat jinou vstup, například Dataset3, ale zadáte Dataset2 vstupů pro CopyActivity2, aby aktivity nejde spustit až do dokončení CopyActivity1. Příklad:

CopyActivity1

Vstupní: Dataset1. Výstup: Dataset2.

CopyActivity2

Zadávání: Dataset3 Dataset2. Výstup: Dataset4.

    {
        "name": "ChainActivities",
        "properties": {
            "description": "Run activities in sequence",
            "activities": [
                {
                    "type": "Copy",
                    "typeProperties": {
                        "source": {
                            "type": "BlobSource"
                        },
                        "sink": {
                            "type": "BlobSink",
                            "copyBehavior": "PreserveHierarchy",
                            "writeBatchSize": 0,
                            "writeBatchTimeout": "00:00:00"
                        }
                    },
                    "inputs": [
                        {
                            "name": "Dataset1"
                        }
                    ],
                    "outputs": [
                        {
                            "name": "Dataset2"
                        }
                    ],
                    "policy": {
                        "timeout": "01:00:00"
                    },
                    "scheduler": {
                        "frequency": "Hour",
                        "interval": 1
                    },
                    "name": "CopyFromBlobToBlob",
                    "description": "Copy data from a blob to another"
                },
                {
                    "type": "Copy",
                    "typeProperties": {
                        "source": {
                            "type": "BlobSource"
                        },
                        "sink": {
                            "type": "BlobSink",
                            "writeBatchSize": 0,
                            "writeBatchTimeout": "00:00:00"
                        }
                    },
                    "inputs": [
                        {
                            "name": "Dataset3"
                        },
                        {
                            "name": "Dataset2"
                        }
                    ],
                    "outputs": [
                        {
                            "name": "Dataset4"
                        }
                    ],
                    "policy": {
                        "timeout": "01:00:00"
                    },
                    "scheduler": {
                        "frequency": "Hour",
                        "interval": 1
                    },
                    "name": "CopyFromBlob3ToBlob4",
                    "description": "Copy data from a blob to another"
                }
            ],
            "start": "2017-04-25T01:00:00Z",
            "end": "2017-04-25T01:00:00Z",
            "isPaused": false
        }
    }


Všimněte si, že v tomto příkladu jsou dvě vstupní datové sady určeným pro druhé činnosti kopírovat. Požádání více vstupů jenom první vstupní datovou sadu slouží ke kopírování dat, ale další datové sady jsou používána jako závislostí. CopyActivity2 by spustit až po jsou splněny následující podmínky:

- CopyActivity1 byla úspěšně dokončena a Dataset2 je k dispozici. Kopírování dat do Dataset4 není vyvolají tento datovou sadu. Pouze funguje jako závislostí plánování pro CopyActivity2.   
- Dataset3 je k dispozici. Tento datovou sadu představuje data, které se zkopírují do cíle.  



## <a name="model-datasets-with-different-frequencies"></a>Model datové sady s jinou četnosti

Ve vzorcích byly frekvencí pro vstupní a výstupní datové sady a okno Plán aktivity stejný. Některé scénáře vyžadují možnost vytvořit výstup frekvencí liší od četnosti vstupů jeden nebo více. Data Factory podporuje modelování podobnému sledu.

### <a name="sample-1-produce-a-daily-output-report-for-input-data-that-is-available-every-hour"></a>Příklad 1: Vytvoření sestavy na denní výstup pro zadávání dat, která je dostupná každou hodinu

Zvažte scénáři, ve kterém zadání měření dat ze senzorů k dispozici každou hodinu v úložišti objektů Blob Azure. Chcete-li vytvořit denní agregační sestava s statistiky například průměr, maximální a minimální pro den se [Data Factory podregistru aktivity](data-factory-hive-activity.md).

Tady je, jak můžete modelovat tento scénář se Data Factory:

**Zadávání datové sady**

Každou hodinu vstupní soubory se nezobrazí ve složce pro daný den. Dostupnost k zadání vstupních hodnot nastavena na **hodinu** (četnost: hodiny, intervalu: 1).

    {
      "name": "AzureBlobInput",
      "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
          "folderPath": "mycontainer/myfolder/{Year}/{Month}/{Day}/",
          "partitionedBy": [
            { "name": "Year", "value": {"type": "DateTime","date": "SliceStart","format": "yyyy"}},
            { "name": "Month","value": {"type": "DateTime","date": "SliceStart","format": "%M"}},
            { "name": "Day","value": {"type": "DateTime","date": "SliceStart","format": "%d"}}
          ],
          "format": {
            "type": "TextFormat"
          }
        },
        "external": true,
        "availability": {
          "frequency": "Hour",
          "interval": 1
        }
      }
    }

**Výstup datové sady**

Jeden výstupní soubor je vytvořen každý den ve složce na den. Dostupnost výstupu je nastavený na **den** (četnost: den a intervalu: 1).


    {
      "name": "AzureBlobOutput",
      "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
          "folderPath": "mycontainer/myfolder/{Year}/{Month}/{Day}/",
          "partitionedBy": [
            { "name": "Year", "value": {"type": "DateTime","date": "SliceStart","format": "yyyy"}},
            { "name": "Month","value": {"type": "DateTime","date": "SliceStart","format": "%M"}},
            { "name": "Day","value": {"type": "DateTime","date": "SliceStart","format": "%d"}}
          ],
          "format": {
            "type": "TextFormat"
          }
        },
        "availability": {
          "frequency": "Day",
          "interval": 1
        }
      }
    }

**Aktivita: podregistru aktivity v kanálu**

Skript podregistru obdrží příslušné informace *data a času* jako parametry, které používají proměnnou **WindowStart** uvedeno v následující ukázce. Skript podregistru používá tuto proměnnou k načtení dat z správné složce pro den a spusťte agregace generovat výstupu.

        {  
            "name":"SamplePipeline",
            "properties":{  
            "start":"2015-01-01T08:00:00",
            "end":"2015-01-01T11:00:00",
            "description":"hive activity",
            "activities": [
                {
                    "name": "SampleHiveActivity",
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
                    "linkedServiceName": "HDInsightLinkedService",
                    "type": "HDInsightHive",
                    "typeProperties": {
                        "scriptPath": "adftutorial\\hivequery.hql",
                        "scriptLinkedService": "StorageLinkedService",
                        "defines": {
                            "Year": "$$Text.Format('{0:yyyy}',WindowStart)",
                            "Month": "$$Text.Format('{0:%M}',WindowStart)",
                            "Day": "$$Text.Format('{0:%d}',WindowStart)"
                        }
                    },
                    "scheduler": {
                        "frequency": "Day",
                        "interval": 1
                    },          
                    "policy": {
                        "concurrency": 1,
                        "executionPriorityOrder": "OldestFirst",
                        "retry": 2,
                        "timeout": "01:00:00"
                    }
                 }
             ]
           }
        }

Na následujícím obrázku vidíte scénář z hlediska závislost data.

![Závislost typu dat](./media/data-factory-scheduling-and-execution/data-dependency.png)

Řez výstup pro každý den závisí na 24 hodinách výsečí ze vstupní datovou sadu. Data Factory vypočítá tyto závislosti automaticky po zjištění vstupních dat výseče, které spadají do stejné časové období jako výstup řezu vyrobit. Pokud některý z 24 vstupní výsečí není k dispozici, čeká Data Factory vstupní řez je připraven před spuštěním každodenní aktivity spustit.


### <a name="sample-2-specify-dependency-with-expressions-and-data-factory-functions"></a>Příklad 2: Zadejte závislost se výrazech a funkcích Data Factory

Dále se podívejme jiný scénář. Předpokládejme, že máte aktivitu podregistru, zpracuje dvě vstupní datové sady. Jedna z nich denně má nová data, ale jedna z nich se nová data každý týden. Předpokládejme, že jste chtěli udělat spojení mezi dva vstupy a nezpůsoboval výstup každý den.

Jednoduchý přístup které Data Factory automaticky obrázky, vpravo pro zadávání výsečí zpracuje zarovnáním do výstupu výseč dat časového období nefunguje.

Je třeba určit, že pro každou aktivitu spustit Data Factory používejte minulý týden výseč týdenní vstupní datovou sadu. Pomocí funkce Azure Data Factory uvedeno v následující ukázce implementace toto chování.

**Input1: Objektů blob Azure**

První vstup je objektů blob Azure aktualizaci denně.

    {
      "name": "AzureBlobInputDaily",
      "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
          "folderPath": "mycontainer/myfolder/{Year}/{Month}/{Day}/",
          "partitionedBy": [
            { "name": "Year", "value": {"type": "DateTime","date": "SliceStart","format": "yyyy"}},
            { "name": "Month","value": {"type": "DateTime","date": "SliceStart","format": "%M"}},
            { "name": "Day","value": {"type": "DateTime","date": "SliceStart","format": "%d"}}
          ],
          "format": {
            "type": "TextFormat"
          }
        },
        "external": true,
        "availability": {
          "frequency": "Day",
          "interval": 1
        }
      }
    }

**Input2: Objektů blob Azure**

Input2 je objektů blob Azure aktualizaci týdně.

    {
      "name": "AzureBlobInputWeekly",
      "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
          "folderPath": "mycontainer/myfolder/{Year}/{Month}/{Day}/",
          "partitionedBy": [
            { "name": "Year", "value": {"type": "DateTime","date": "SliceStart","format": "yyyy"}},
            { "name": "Month","value": {"type": "DateTime","date": "SliceStart","format": "%M"}},
            { "name": "Day","value": {"type": "DateTime","date": "SliceStart","format": "%d"}}
          ],
          "format": {
            "type": "TextFormat"
          }
        },
        "external": true,
        "availability": {
          "frequency": "Day",
          "interval": 7
        }
      }
    }

**Výstup: Objektů blob Azure**

Jeden výstupní soubor vytvoří se každý den ve složce pro den. Dostupnost výstupu je nastavený na **den** (četnost: den, intervalu: 1).

    {
      "name": "AzureBlobOutputDaily",
      "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
          "folderPath": "mycontainer/myfolder/{Year}/{Month}/{Day}/",
          "partitionedBy": [
            { "name": "Year", "value": {"type": "DateTime","date": "SliceStart","format": "yyyy"}},
            { "name": "Month","value": {"type": "DateTime","date": "SliceStart","format": "%M"}},
            { "name": "Day","value": {"type": "DateTime","date": "SliceStart","format": "%d"}}
          ],
          "format": {
            "type": "TextFormat"
          }
        },
        "availability": {
          "frequency": "Day",
          "interval": 1
        }
      }
    }

**Aktivita: podregistru aktivity v kanálu**

Aktivity podregistru má dvě vstupní hodnoty a vytvoří výstup výseč každý den. Můžete určit každý den výstup výseč závisí na vstupní výsečí v předchozím týdnu o zadání kritérií týdenní následujícím způsobem.

    {  
        "name":"SamplePipeline",
        "properties":{  
        "start":"2015-01-01T08:00:00",
        "end":"2015-01-01T11:00:00",
        "description":"hive activity",
        "activities": [
          {
            "name": "SampleHiveActivity",
            "inputs": [
              {
                "name": "AzureBlobInputDaily"
              },
              {
                "name": "AzureBlobInputWeekly",
                "startTime": "Date.AddDays(SliceStart, - Date.DayOfWeek(SliceStart))",
                "endTime": "Date.AddDays(SliceEnd,  -Date.DayOfWeek(SliceEnd))"  
              }
            ],
            "outputs": [
              {
                "name": "AzureBlobOutputDaily"
              }
            ],
            "linkedServiceName": "HDInsightLinkedService",
            "type": "HDInsightHive",
            "typeProperties": {
              "scriptPath": "adftutorial\\hivequery.hql",
              "scriptLinkedService": "StorageLinkedService",
              "defines": {
                "Year": "$$Text.Format('{0:yyyy}',WindowStart)",
                "Month": "$$Text.Format('{0:%M}',WindowStart)",
                "Day": "$$Text.Format('{0:%d}',WindowStart)"
              }
            },
            "scheduler": {
              "frequency": "Day",
              "interval": 1
            },          
            "policy": {
              "concurrency": 1,
              "executionPriorityOrder": "OldestFirst",
              "retry": 2,  
              "timeout": "01:00:00"
            }
           }
         ]
       }
    }


## <a name="data-factory-functions-and-system-variables"></a>Funkce Factory dat a systémových proměnných   

Seznam funkcí a proměnných systému, které podporuje Data Factory najdete v článku [funkce Factory dat a systémových proměnných](data-factory-functions-variables.md) .

## <a name="data-dependency-deep-dive"></a>Data závislost hloubkové postupy

Generovat řez datovou sadu spuštěním aktivity, Data Factory používá následující *závislost modelu* a zjistit vztahy mezi datové sady spotřebované množství a vytvořené pomocí aktivity.

Časový rozsah vstupní datové sady muset generovat výseč datovou sadu výstup nazývá *závislost období*.

Spustit aktivity vygeneruje řez datovou sadu až po výsečí data ve vstupní datové sady v období závislost jsou k dispozici. Jinými slovy všechny vstupní výsečí zahrnující období závislost musí být v **připravena aktivity** spustit plodiny výseč datovou sadu výstupu.

Generovat výseč datovou sadu [**start**; **Konec**], třeba funkce namapovat výseč datovou sadu doby závislost. Tato funkce je v podstatě vzorec, který se převede na začátek a konec období závislost typu zahájení a ukončení výseče datovou sadu. Další dříve:

    DatasetSlice = [start, end]
    DependecyPeriod = [f(start, end), g(start, end)]

Funkce, která provádí výpočet začátek a konec období závislost pro každou činnost při zadávání mapování **F** a **g** .

Jak je vidět ve vzorcích, je stejná jako dobu výseč, že je vyrobeno závislost období. V těchto případech Data Factory automaticky vypočítá vstupní řezy, které spadají do závislost období.  

Období výseč dat je v ukázku agregace kde denně výsledek a zadávání dat je k dispozici každou hodinu 24 hodin. Data Factory najde relevantní hodinové vstupní řezy pro tohoto časového období a chce výseč výstup závisí na vstupní výsečí.

Můžete taky zadat vlastní mapování dobu závislost uvedeno v vzorku, pokud vstupů je každý týden a řez výstup pochází denně.

## <a name="data-dependency-and-validation"></a>Závislost typu dat a ověření

Datovou sadu může obsahovat zásad ověření definované Určuje, jak lze data generovaného při spuštění řez ověřit předtím, než je připraven k spotřebu. Další informace najdete v článku [Vytvoření datové sady](data-factory-create-datasets.md) .

V takovém případě po spuštění, řez výstupní výseč stav se změní na **čekání** s podřízeného stavu **ověření**. Po ověření výseče výseč stav změní na **připravení**.

Pokud řez dat je vyrobeno však nesplňuje podmínky ověření aktivity spustí pro příjem řezy, které jsou závislé na tohoto výřezu se nezobrazí.

[Monitor a správa potrubí](data-factory-monitor-manage-pipelines.md) popisuje různé stavy dat výsečí Data Factory.

## <a name="external-data"></a>Externí data

Datovou sadu můžete označit jako externí (jak je uvedeno v následující ukázce JSON), zdání, že nebyl vytvořený pomocí Data Factory. V takovém případě můžete zásad datovou sadu další nastavení parametrů, které popisují ověření a opakujte zásad pro datovou sadu. Popis všech vlastností naleznete v tématu [Vytvoření kanály](data-factory-create-pipelines.md) .

Podobně jako datové sady vytvořený Data Factory výsečí dat na externí data třeba připravení před budou zpracovány závislá výsečí.

    {
        "name": "AzureSqlInput",
        "properties":
        {
            "type": "AzureSqlTable",
            "linkedServiceName": "AzureSqlLinkedService",
            "typeProperties":
            {
                "tableName": "MyTable"
            },
            "availability":
            {
                "frequency": "Hour",
                "interval": 1     
            },
            "external": true,
            "policy":
            {
                "externalData":
                {
                    "retryInterval": "00:01:00",
                    "retryTimeout": "00:10:00",
                    "maximumRetry": 3
                }
            }  
        }
    }


## <a name="onetime-pipeline"></a>Jednorázově kanálem k odesílání zpráv
Vytvoření a naplánování kanálu pravidelně spouštět (například: hodinové nebo denně) v rámci počátečního a koncového času určíte v definici kanálem k odesílání zpráv. Další informace najdete v článku [plánování aktivity](#scheduling-and-execution) . Můžete taky vytvořit kanál, který se spustí pouze jednou. Postup nastavení vlastnosti **pipelineMode** v definici kanálem k odesílání zpráv do **jednorázově** uvedeno v následujícím příkladu JSON. Výchozí hodnota pro tuto vlastnost je **naplánovaný**.

    {
        "name": "CopyPipeline",
        "properties": {
            "activities": [
                {
                    "type": "Copy",
                    "typeProperties": {
                        "source": {
                            "type": "BlobSource",
                            "recursive": false
                        },
                        "sink": {
                            "type": "BlobSink",
                            "writeBatchSize": 0,
                            "writeBatchTimeout": "00:00:00"
                        }
                    },
                    "inputs": [
                        {
                            "name": "InputDataset"
                        }
                    ],
                    "outputs": [
                        {
                            "name": "OutputDataset"
                        }
                    ]
                    "name": "CopyActivity-0"
                }
            ]
            "pipelineMode": "OneTime"
        }
    }

Poznámka:

- Čas **zahájení** a **ukončení** profilace nejsou zadány.
- **Dostupnost** vstupní a výstupní datové sady uvedené (**Četnost** a **intervalu**), i když Data Factory nepoužívá hodnoty.  
- Zobrazení diagramu nezobrazuje jednorázové kanály. Toto chování se o záměr.
- Jednorázové potrubí nelze aktualizovat. Můžete klonovat jednorázové kanálu, ji přejmenovat, aktualizovat vlastnosti a nasazení vytvořit jiné.
