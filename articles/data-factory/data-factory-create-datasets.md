<properties 
    pageTitle="Vytvoření datové sady v Azure Data Factory | Microsoft Azure" 
    description="Zjistěte, jak vytvořit datové sady v Azure Data Factory s příklady, které používají vlastnosti například posun a anchorDateTime."
    keywords="Vytvoření sady dat, například datovou sadu, posun příklad"
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
    ms.date="09/13/2016" 
    ms.author="shlo"/>

# <a name="datasets-in-azure-data-factory"></a>Datové sady v Azure Data Factory
Tento článek popisuje datové sady v Azure Data Factory a obsahuje příklady například posun, anchorDateTime a posun/styl databází.

Při vytváření datovou sadu vytváříte odkaz na data, která chcete zpracovat. (Vstupní a výstupní) činnost zpracování dat a aktivity jsou obsaženy v kanálu. Zadávání datovou sadu představuje vstupní aktivita v kanálu a datovou sadu výstup představuje výstup aktivity.

Datové sady identifikaci dat oblasti jiný datový úložiště, jako jsou tabulky, soubory, složky a dokumenty. Po vytvoření datovou sadu, můžete ho použít s aktivity v kanálu. Například datovou sadu lze vstupní a výstupní datovou sadu z aktivitu kopírovat nebo HDInsightHive. Portál Azure vám vizuálním rozložení kanály a data vstupů a výstupů. Na první pohled se zobrazí všechny relace a závislosti vaší potrubí ze všech zdrojů abyste pořád věděli, kde je dat pocházejících z a místo, kam má dělat.

V Azure Data Factory můžete načtení dat z datovou sadu pomocí kopírování aktivity v kanálu.

> [AZURE.NOTE] Pokud začínáte Azure Data Factory, najdete v článku [Úvod k Azure Data Factory](data-factory-introduction.md) přehled o služby Azure Data Factory. V tématu [vytvoření první factory dat](data-factory-build-your-first-pipeline.md) kurz vytvoření první factory data. Tyto články poskytují že základní informace, abyste mohli lépe porozumět tomu tohoto článku. 

## <a name="define-datasets"></a>Definice datové sady
Sady dat Azure dat výrobce je definována následujícím způsobem: 


    {
        "name": "<name of dataset>",
        "properties": {
            "type": "<type of dataset: AzureBlob, AzureSql etc...>",
            "external": <boolean flag to indicate external data. only for input datasets>,
            "linkedServiceName": "<Name of the linked service that refers to a data store.>",
            "structure": [
                {
                    "name": "<Name of the column>",
                    "type": "<Name of the type>"
                }
            ],
            "typeProperties": {
                "<type specific property>": "<value>",
                "<type specific property 2>": "<value 2>",
            },
            "availability": {
                "frequency": "<Specifies the time unit for data slice production. Supported frequency: Minute, Hour, Day, Week, Month>",
                "interval": "<Specifies the interval within the defined frequency. For example, frequency set to 'Hour' and interval set to 1 indicates that new data slices should be produced hourly>"
            },
           "policy": 
            {      
            }
        }
    }

V následující tabulce jsou popsané vlastnosti ve výše uvedené JSON:   

| Vlastnost | Popis | Povinné | Výchozí |
| -------- | ----------- | -------- | ------- |
| Jméno | Název datové sady. Pravidla pro pojmenování naleznete v tématu [Azure Data Factory - pojmenování pravidel](data-factory-naming-rules.md) . | Ano | NEDEF |
| Typ | Typ datové sady. Zadat jeden z typů podporovaných Azure Data Factory (například: AzureBlob, AzureSqlTable). <br/><br/>Podrobnosti najdete v části [Typ datovou sadu](#Type) . | Ano | NEDEF |
| Struktura | Schéma datové sady<br/><br/>Podrobnosti najdete v tématu [Datovou sadu strukturu](#Structure) oddíl. | Ne. | NEDEF |
| typeProperties | Vlastnosti odpovídající vybraného typu. Podrobnosti o podporovaných typů a jejich vlastnosti jsou uvedeny v části [Typ datovou sadu](#Type) . | Ano | NEDEF |
| externí | Logická příznak můžete určit, zda datovou sadu explicitně pochází factory kanálem dat nebo ne.  | Ne | NEPRAVDA | 
| dostupnost | Definuje okno zpracování nebo řezů modelu doplňku pro datovou sadu výroby. <br/><br/>Podrobnosti najdete v tématu [Datovou sadu dostupnost](#Availability) oddíl. <br/><br/>Podrobné informace o datovou sadu rozdělení na řezy modelu, najdete v článku [plánování a provádění](data-factory-scheduling-and-execution.md) . | Ano | NEDEF
| zásady | Určuje kritéria nebo podmínky, které musí splňovat výsečí datovou sadu. <br/><br/>Další informace najdete v tématu [Zásady datovou sadu](#Policy) oddíl. | Ne | NEDEF |

## <a name="dataset-example"></a>Příklad datové sady
V následujícím příkladu představuje sadě dat v tabulce s názvem **Tabulka** v **databázi Azure SQL**. 

    {
        "name": "DatasetSample",
        "properties": {
            "type": "AzureSqlTable",
            "linkedServiceName": "AzureSqlLinkedService",
            "typeProperties": 
            {
                "tableName": "MyTable"
            },
            "availability": 
            {
                "frequency": "Day",
                "interval": 1
            }
        }
    }

Mějte na paměti následující skutečnosti: 

- Typ je nastavený na AzureSqlTable.
- Název tabulky zadejte (specifické pro typ AzureSqlTable) je nastavena na tabulka.
- linkedServiceName odkazuje na propojený služba typu AzureSqlDatabase. V tématu definice následující propojené služby. 
- dostupnost počet_plateb je nastavený na den a interval je nastavena na hodnotu 1, což znamená, že řez denně vyrobeno.  

AzureSqlLinkedService je definována následujícím způsobem:

    {
        "name": "AzureSqlLinkedService",
        "properties": {
            "type": "AzureSqlDatabase",
            "description": "",
            "typeProperties": {
                "connectionString": "Data Source=tcp:<servername>.database.windows.net,1433;Initial Catalog=<databasename>;User ID=<username>@<servername>;Password=<password>;Integrated Security=False;Encrypt=True;Connect Timeout=30"
            }
        }
    }

Ve výše uvedené JSON: 

- Typ je nastavený na AzureSqlDatabase
- vlastnost typu connectionString Určuje informace k připojení k databázi Azure SQL.  


Můžete vidět, službu propojené Určuje, jak se připojit k databázi Azure SQL. Datové definuje tabulky se používá jako vstupní a výstupní aktivity v kanálu. V části aktivity v seznamu [kanálem k odesílání zpráv](data-factory-create-pipelines.md) JSON Určuje, zda datové používá jako vstupní a výstupní sadu.


> [AZURE.IMPORTANT] Pokud datovou sadu je adresami vytvořené pomocí Azure Data Factory, mají označit jako **externí**. Toto nastavení obecně platí pro zadávání první činnosti v kanálu.   

## <a name="Type"></a>Typ datové sady
Podporované zdroje dat a datové sady typy jsou zarovnané. V tématech odkazuje ve článku [Aktivity pohyb dat](data-factory-data-movement-activities.md#supported-data-stores) pro informace o typech a konfigurace datové sady. Například pokud používáte dat z databáze Azure SQL, klikněte na databázi SQL Azure v seznamu podporovaný datový úložiště zobrazíte podrobné informace.  

## <a name="Structure"></a>Struktura datové sady
Část **struktury** definuje schéma datové sady. Obsahuje kolekce názvy a datových typů sloupců.  V následujícím příkladu datové má tři sloupce slicetimestamp názevprojektu a pageviews a jsou typu: řetězce, řetězce a desetinné čárky v tomto pořadí.

    structure:  
    [ 
        { "name": "slicetimestamp", "type": "String"},
        { "name": "projectname", "type": "String"},
        { "name": "pageviews", "type": "Decimal"}
    ]

## <a name="Availability"></a>Sady dat dostupnosti
V části **dostupnost** v datovou sadu definuje okno zpracování (každou hodinu, denně, týdně atd.) nebo řezů modelu pro datovou sadu. Najdete v článku [plánování a provádění](data-factory-scheduling-and-execution.md) článku podrobné informace o modelu rozdělení na řezy a závislostí datovou sadu. 

V následující části Dostupnost určuje, že datovou sadu výstup výroby hodinové (nebo) vstupní datovou sadu neexistuje hodinové:

    "availability": 
    {   
        "frequency": "Hour",        
        "interval": 1   
    }

V následující tabulce jsou popsané vlastnosti, které můžete použít v části dostupnost: 

| Vlastnost | Popis | Povinné | Výchozí |
| -------- | ----------- | -------- | ------- |
| četnosti | Určuje časové jednotky datovou sadu výseč výroby.<br/><br/>**Podporované počet_plateb**: minuty, hodiny, den, týden, měsíc | Ano | NEDEF |
| interval | Určuje násobek intervalu<br/><br/>"Interval frekvence x" Určuje, jak často se vyrábí řez.<br/><br/>Pokud budete potřebovat sadu rozdělena hodinu, nastavte **počet_plateb** **hodiny**a **intervalu** **1**.<br/><br/>**Poznámka:** Pokud zadáte počet_plateb minuty, doporučujeme, abyste nastavení intervalu méně než 15 | Ano | NEDEF |
| Styl | Určuje, zda by mělo být řez vyrobeno na začátek/konec intervalu.<ul><li>StartOfInterval</li><li>EndOfInterval</li></ul><br/><br/>Pokud je počet_plateb nastavené měsíc a sadě stylů lze EndOfInterval, řez vyrobeno na poslední den měsíce. Pokud požadovaný styl je nastaveno StartOfInterval, řez vyrobeno na první den v měsíci.<br/><br/>Jestliže počet_plateb je nastavený na den a sadě stylů lze EndOfInterval, řez vyrábějí v poslední hodinu dne.<br/><br/>Jestliže počet_plateb je nastavena na hodinu a sadě stylů lze EndOfInterval, řez vyrobeno na konci hodinu. Řezu dobu 1 PM – 2 hodin, například řez pochází na 2 odp. | Ne | EndOfInterval |
| anchorDateTime | Definuje absolutní umístění v čase použití Plánovač pro výpočet hranice výseč datovou sadu. <br/><br/>**Poznámka:** Pokud AnchorDateTime obsahuje data z jednotlivých částí, které je podrobnějších než frekvence podrobnějších části ignorovány. <br/><br/>Například, pokud je **interval** **hodinové** (četnost: hodiny a intervalu: 1) a **AnchorDateTime** obsahuje **minuty a sekundy**a potom **minuty a sekundy** díly AnchorDateTime jsou ignorovány. | Ne | 01/01/0001 |
| Posun | Časový interval prodloužením doby zahájení a dokončení všech výsečí datovou sadu posunuty směrem. <br/><br/>**Poznámka:** Pokud nejsou zadány anchorDateTime a posun, výsledkem je kombinované shift. | Ne | NEDEF |

### <a name="offset-example"></a>Posun příklad

Denní řezy spuštěné v 6: 00 místo výchozí půlnoci. 

    "availability":
    {
        "frequency": "Day",
        "interval": 1,
        "offset": "06:00:00"
    }

**Četnost** nastavena na **den** a **interval** je nastavena na hodnotu **1** (jednou za den): Pokud výseč vyrobit na 6: 00 místo ve výchozím nastavení dobu: 00 dop. Myslete na to, že Tentokrát je času UTC. 

## <a name="anchordatetime-example"></a>Příklad anchorDateTime

**Příklad:** 23 hodin datovou sadu výseče, které začínají na 2007 – 04-19T08:00:00

    "availability": 
    {   
        "frequency": "Hour",        
        "interval": 23, 
        "anchorDateTime":"2007-04-19T08:00:00"  
    }

## <a name="offsetstyle-example"></a>Posun/styl příklad

Pokud potřebujete datovou sadu na měsíčně na určité datum a čas (na 3rd každý měsíc ve 8:00) Předpokládejme, že použití by měla běžet značku **Odsazení** do nastavení data a času. 

    {
      "name": "MyDataset",
      "properties": {
        "type": "AzureSqlTable",
        "linkedServiceName": "AzureSqlLinkedService",
        "typeProperties": {
          "tableName": "MyTable"
        },
        "availability": {
          "frequency": "Month",
          "interval": 1,
          "offset": "3.08:10:00",
          "style": "StartOfInterval"
        }
      }
    }


## <a name="Policy"></a>Zásady datové sady

V části **zásady** v datovou sadu definice definuje kritéria nebo podmínky, které musí splňovat výsečí datovou sadu.

### <a name="validation-policies"></a>Ověření zásady

| Název zásady | Popis | Použije | Povinné | Výchozí |
| ----------- | ----------- | ---------- | -------- | ------- |
| minimumSizeMB | Ověří, že data ve **objektů blob Azure** splňuje požadavky na minimální velikost (v megabajtech). | Objektů Blob Azure | Ne | NEDEF |
|minimumRows | Ověří, že data v **databázi Azure SQL** nebo **tabulek Azure** obsahuje minimální počet řádků. | <ul><li>Databáze Azure SQL</li><li>Azure Table</li></ul> | Ne | NEDEF

#### <a name="examples"></a>Příklady

**minimumSizeMB:**

    "policy":
    
    {
        "validation":
        {
            "minimumSizeMB": 10.0
        }
    }

**minimumRows**

    "policy":
    {
        "validation":
        {
            "minimumRows": 100
        }
    }

### <a name="external-datasets"></a>Externí datové sady

Externí datové sady jsou ty, které nejsou vytvořené pomocí pracovního kanálu v data factory. Pokud datové označen jako **externí**, může ovlivnit chování dostupnost výseč datovou sadu definovány zásady **ExternalData** . 

Pokud datovou sadu je adresami vytvořené pomocí Azure Data Factory, mají označit jako **externí**. Toto nastavení se obecně platí pro zadávání první činnosti v kanálu Pokud aktivity nebo zřetězení kanálem k odesílání zpráv používá. 

| Jméno | Popis | Povinné | Výchozí hodnota  |
| ---- | ----------- | -------- | -------------- |
| dataDelay | Časový interval Kontrola dostupnosti externích dat pro dané výsečí. Například data by měla být k dispozici každou hodinu, může být zaškrtnutí zobrazíte externí data je k dispozici a odpovídající výseč připravených zpožděn pomocí dataDelay.<br/><br/>Platí jenom pro současné době.  Například, pokud je to 1:00 odp teď a tato hodnota je 10 minut ověření spustí: 10: 00.<br/><br/>Toto nastavení nemá vliv na výsečí v minulosti (výsečí pomocí výsečí koncový čas + dataDelay < teď) jsou zpracovány bez zpoždění.<br/><br/>Čas větší než 23:59, která hodin muset zadaný day.hours:minutes:seconds formátu. Například chcete-li 24 hodin, nepoužívejte 24:00:00 Místo toho používejte 1.00:00:00. Pokud používáte 24:00:00, považuje se za 24 dní (24.00:00:00). Pro 1 den a 4 hodiny zadejte 1:04:00:00. | Ne | 0 |
| retryInterval | Prodleva mezi selhání a dalšího opakujte pokus o. Platí pro současné době; Pokud předchozí akci selhalo, jsme počkejte to dlouho po poslední vyzkoušet. <br/><br/>Je-li 1:00 odp teď, začneme napoprvé. Pokud je doba trvání dokončete první ověření minutu a operace se nezdařila, další opakovat je na 1:00 1 min (doba_trvání) + 1min (interval opakování) = 1:02 odp. <br/><br/>U výsečí v minulosti je bez zpoždění. Opakovat se stane, okamžitě. | Ne | 00:01:00 (1 minutu) | 
| retryTimeout | Časový limit pro každý pokus opakovat.<br/><br/>Pokud tato vlastnost je nastavena na 10 minut, je potřeba dokončit 10 minut ověření. Pokud trvá delší než 10 minut provádět ověřování, vyprší časový limit opakovat.<br/><br/>Všechny pokusy pro ověření vyprší její časový limit, řez označena jako TimedOut. | Ne | 00:10:00 (10 minut) |
| maximumRetry | Počet Kontrola dostupnosti externí data. Povolené maximální hodnotu 10. | Ne | 3 | 

## <a name="scoped-datasets"></a>Obory datové sady
Můžete vytvořit datové sady, které mají rozsah potrubí pomocí vlastnosti **datové sady** . Tyto datové sady lze použít pouze aktivity v rámci tohoto kanálu, ale ne aktivity v jiných kanály. Následující příklad definuje kanálu se dvěma datové sady - připojení ke vzdálené ploše InputDataset a OutputDataset-připojení ke vzdálené ploše - lze používat v rámci kanálu:  

> [AZURE.IMPORTANT] Obory datové sady podporuje pouze s jednorázové potrubí (**pipelineMode** ve nastavena na **OneTime**). Další informace najdete v článku [Onetime kanálem k odesílání zpráv](data-factory-scheduling-and-execution.md#onetime-pipeline) .

    {
        "name": "CopyPipeline-rdc",
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
                            "name": "InputDataset-rdc"
                        }
                    ],
                    "outputs": [
                        {
                            "name": "OutputDataset-rdc"
                        }
                    ],
                    "scheduler": {
                        "frequency": "Day",
                        "interval": 1,
                        "style": "StartOfInterval"
                    },
                    "name": "CopyActivity-0"
                }
            ],
            "start": "2016-02-28T00:00:00Z",
            "end": "2016-02-28T00:00:00Z",
            "isPaused": false,
            "pipelineMode": "OneTime",
            "expirationTime": "15.00:00:00",
            "datasets": [
                {
                    "name": "InputDataset-rdc",
                    "properties": {
                        "type": "AzureBlob",
                        "linkedServiceName": "InputLinkedService-rdc",
                        "typeProperties": {
                            "fileName": "emp.txt",
                            "folderPath": "adftutorial/input",
                            "format": {
                                "type": "TextFormat",
                                "rowDelimiter": "\n",
                                "columnDelimiter": ","
                            }
                        },
                        "availability": {
                            "frequency": "Day",
                            "interval": 1
                        },
                        "external": true,
                        "policy": {}
                    }
                },
                {
                    "name": "OutputDataset-rdc",
                    "properties": {
                        "type": "AzureBlob",
                        "linkedServiceName": "OutputLinkedService-rdc",
                        "typeProperties": {
                            "fileName": "emp.txt",
                            "folderPath": "adftutorial/output",
                            "format": {
                                "type": "TextFormat",
                                "rowDelimiter": "\n",
                                "columnDelimiter": ","
                            }
                        },
                        "availability": {
                            "frequency": "Day",
                            "interval": 1
                        },
                        "external": false,
                        "policy": {}
                    }
                }
            ]
        }
    }