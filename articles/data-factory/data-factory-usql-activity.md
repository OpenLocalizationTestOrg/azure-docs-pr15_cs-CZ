<properties 
    pageTitle="Spuštění skript U SQL na analýzy jezera Azure dat z Azure Data Factory" 
    description="Naučte se zpracování dat spuštěním skripty U SQL ve výpočetním služby Azure dat jezera analýzy." 
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
    ms.date="09/12/2016" 
    ms.author="spelluru"/>

# <a name="run-u-sql-script-on-azure-data-lake-analytics-from-azure-data-factory"></a>Spuštění skript U SQL na analýzy jezera Azure dat z Azure Data Factory
> [AZURE.SELECTOR]
[Podregistru](data-factory-hive-activity.md)  
[Prasátko](data-factory-pig-activity.md)  
[MapReduce](data-factory-map-reduce.md)  
[Přenos Hadoop](data-factory-hadoop-streaming-activity.md)
[Počítače výukové](data-factory-azure-ml-batch-execution-activity.md) 
[Uložená procedura](data-factory-stored-proc-activity.md)
[Analýzy jezera dat U-SQL](data-factory-usql-activity.md)
[.NET vlastní](data-factory-use-custom-activities.md)
 
Kanálů v Azure dat factory zpracovává data v propojených úložiště služby podnikové propojené výpočetním. Obsahuje řadu aktivity místo, kam jednotlivé aktivity provádí operace konkrétní zpracování. Tento článek popisuje **Dat jezera analýzy U SQL aktivity** , které se spouští skript **U SQL** na služby **Azure dat jezera analýzy** výpočetní propojené. 

> [AZURE.NOTE] 
> Vytvořte účet Azure dat jezera analýzy před vytvořením kanálů aktivitu U SQL technologie pro analýzu dat jezera. Další informace o jezera analýzy dat Azure, najdete v článku [Začínáme s Azure dat jezera analýzy](../data-lake-analytics/data-lake-analytics-get-started-portal.md).
>  
> Prohlédněte si [vytvořit svůj první kurz kanálem k odesílání zpráv](data-factory-build-your-first-pipeline.md) podrobný postup k vytvoření factory dat, propojené služeb, datových sad a potrubí. Pomocí JSON fragmenty Editor Factory dat nebo Visual Studio nebo Azure PowerShell vytvořit Data Factory entity.

## <a name="azure-data-lake-analytics-linked-service"></a>Azure dat jezera analýzy propojené služby
Vytvoření služby **Azure dat jezera analýzy** propojené propojíte služby Azure dat jezera analýzy výpočetním factory Azure data. Technologie pro analýzu jezera dat U-SQL aktivity v kanálu odkazuje na tuto službu propojené. 

Následující příklad uvádí JSON definici služby Azure dat jezera technologie pro analýzu propojení. 

    {
        "name": "AzureDataLakeAnalyticsLinkedService",
        "properties": {
            "type": "AzureDataLakeAnalytics",
            "typeProperties": {
                "accountName": "adftestaccount",
                "dataLakeAnalyticsUri": "datalakeanalyticscompute.net",
                "authorization": "<authcode>",
                "sessionId": "<session ID>", 
                "subscriptionId": "<subscription id>",
                "resourceGroupName": "<resource group name>"
            }
        }
    }


V následující tabulce jsou uvedeny popisy vlastností použita při definici JSON. 

Vlastnost | Popis | Povinné
-------- | ----------- | --------
Typ | Vlastnost typ je třeba nastavit na: **AzureDataLakeAnalytics**. | Ano
název účtu | Název účtu Azure dat jezera analýzy. | Ano
dataLakeAnalyticsUri | Identifikátor URI jezera analýzy dat Azure. |  Ne 
povolení | Povolení kód načítá automaticky, kliknete na tlačítko **Ověřit** v editoru Factory dat a dokončení OAuth přihlášení.  | Ano 
subscriptionId | Id předplatného Azure | Ne (Pokud není zadán, předplatné factory dat je použit). 
resourceGroupName | Název skupiny Azure zdroje |  Ne (Pokud není zadán, skupina zdroje dat factory slouží).
ID relace | id relace z relace se tak mohli ověřovat OAuth. Každé relace id je jedinečný a se může použít jenom jednou. Relace Id je automaticky generované v editoru Factory Data. | Ano

Kód se tak mohli ověřovat vytvořeného pomocí tlačítka **Ověřit** vyprší později. Po dobu platnosti pro různé typy uživatelských účtů najdete v následující tabulce. Může zobrazit chybová zpráva ověřování **vyprší jejich platnost token**: pověření Chyba operace: invalid_grant - AADSTS70002: chyby ověření pověření. AADSTS70008: Udělení ujednaných přístup vyprší nebo odvolat. Sledování ID: ID korelace d18629e8-af88-43c5-88e3-d8419eb1fca1: fac30a0c-6be6-4e02-8d69-a776d2ffefd7 časové razítko: 2015-12-15 21:09:31Z

 
| Uživatelský typ | Vyprší po |
| :-------- | :----------- | 
| Uživatelské účty není spravuje Azure Active Directory (@hotmail.com, @live.com, atd.) | 12 hodin |
| Uživatelské účty spravovaných tak, že Azure Active Directory (AAD) | Spusťte 14 dní po poslední výsečí. <br/><br/>90 dní, pokud je řezu založeného na propojený služeb na základě OAuth spuštěn aspoň jednou každých 14 dní. |

Vyhněte se/vyřešit tuto chybu, autorizovat pomocí **Povolení** tlačítko kdy **vyprší platnost token** a opětovném nasazení propojené služby. Můžete taky vygenerování hodnot vlastností **ID relace** a **Povolení** programově pomocí kódu v následující části. 

  
### <a name="to-programmatically-generate-sessionid-and-authorization-values"></a>K programově vygenerování hodnot ID relace a ověření 

    if (linkedService.Properties.TypeProperties is AzureDataLakeStoreLinkedService ||
        linkedService.Properties.TypeProperties is AzureDataLakeAnalyticsLinkedService)
    {
        AuthorizationSessionGetResponse authorizationSession = this.Client.OAuth.Get(this.ResourceGroupName, this.DataFactoryName, linkedService.Properties.Type);

        WindowsFormsWebAuthenticationDialog authenticationDialog = new WindowsFormsWebAuthenticationDialog(null);
        string authorization = authenticationDialog.AuthenticateAAD(authorizationSession.AuthorizationSession.Endpoint, new Uri("urn:ietf:wg:oauth:2.0:oob"));

        AzureDataLakeStoreLinkedService azureDataLakeStoreProperties = linkedService.Properties.TypeProperties as AzureDataLakeStoreLinkedService;
        if (azureDataLakeStoreProperties != null)
        {
            azureDataLakeStoreProperties.SessionId = authorizationSession.AuthorizationSession.SessionId;
            azureDataLakeStoreProperties.Authorization = authorization;
        }

        AzureDataLakeAnalyticsLinkedService azureDataLakeAnalyticsProperties = linkedService.Properties.TypeProperties as AzureDataLakeAnalyticsLinkedService;
        if (azureDataLakeAnalyticsProperties != null)
        {
            azureDataLakeAnalyticsProperties.SessionId = authorizationSession.AuthorizationSession.SessionId;
            azureDataLakeAnalyticsProperties.Authorization = authorization;
        }
    }

V tématech [AzureDataLakeStoreLinkedService třídy](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuredatalakestorelinkedservice.aspx) [AzureDataLakeAnalyticsLinkedService předmětu](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuredatalakeanalyticslinkedservice.aspx)a [Třídy AuthorizationSessionGetResponse](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.authorizationsessiongetresponse.aspx) podrobnosti o Data Factory třídy používané v kódu. Přidání odkazu do: Microsoft.IdentityModel.Clients.ActiveDirectory.WindowsForms.dll WindowsFormsWebAuthenticationDialog předmětu. 
 
 
## <a name="data-lake-analytics-u-sql-activity"></a>Aktivity U SQL jezera analýzy dat 

Následující úryvek JSON definuje kanálu aktivitu U SQL technologie pro analýzu dat jezera. Definice aktivitu obsahuje odkaz na službu Azure dat jezera technologie pro analýzu propojení, kterou jste dříve vytvořili.   
  

    {
        "name": "ComputeEventsByRegionPipeline",
        "properties": {
            "description": "This is a pipeline to compute events for en-gb locale and date less than 2012/02/19.",
            "activities": 
            [
                {
                    "type": "DataLakeAnalyticsU-SQL",
                    "typeProperties": {
                        "scriptPath": "scripts\\kona\\SearchLogProcessing.txt",
                        "scriptLinkedService": "StorageLinkedService",
                        "degreeOfParallelism": 3,
                        "priority": 100,
                        "parameters": {
                            "in": "/datalake/input/SearchLog.tsv",
                            "out": "/datalake/output/Result.tsv"
                        }
                    },
                    "inputs": [
                        {
                            "name": "DataLakeTable"
                        }
                    ],
                    "outputs": 
                    [
                        {
                            "name": "EventsByRegionTable"
                        }
                    ],
                    "policy": {
                        "timeout": "06:00:00",
                        "concurrency": 1,
                        "executionPriorityOrder": "NewestFirst",
                        "retry": 1
                    },
                    "scheduler": {
                        "frequency": "Day",
                        "interval": 1
                    },
                    "name": "EventsByRegion",
                    "linkedServiceName": "AzureDataLakeAnalyticsLinkedService"
                }
            ],
            "start": "2015-08-08T00:00:00Z",
            "end": "2015-08-08T01:00:00Z",
            "isPaused": false
        }
    }


Následující tabulka popisuje názvů a popisy vlastností, které jsou specifické pro tuto aktivitu. 

Vlastnost | Popis | Povinné
:-------- | :----------- | :--------
Typ | Vlastnost typ nastavte **DataLakeAnalyticsU -**SQL. | Ano
scriptPath | Cesta k složku obsahující skript U SQL. Název souboru je velká a malá písmena. | Ne (Pokud používáte skript)
scriptLinkedService | Propojené služby, který propojuje úložiště, který obsahuje skript factory dat | Ne (Pokud používáte skript)
skript | Určení vložené skripty místo určení scriptPath a scriptLinkedService. Příklad: "skript": "Vytvořit databázi test". | Ne (Pokud používáte scriptPath a scriptLinkedService)
degreeOfParallelism | Maximální počet uzlů současně použít ke spuštění úlohy. | Ne
priority (priorita) | Určuje, které úkoly mimo vše, co jsou ve frontě by měla být vybraná spuštění první. Nižší číslo, vyšší priorita. | Ne 
Parametry | Parametry pro skript U SQL | Ne 

Definice skript naleznete v tématu [Definice skript SearchLogProcessing.txt](#script-definition) . 

## <a name="sample-input-and-output-datasets"></a>Ukázková vstupní a výstupní datové sady

### <a name="input-dataset"></a>Zadávání datové sady
V tomto příkladu vstupní data uložena v úložišti jezera dat Azure (SearchLog.tsv soubor ve složce datalake/input). 

    {
        "name": "DataLakeTable",
        "properties": {
            "type": "AzureDataLakeStore",
            "linkedServiceName": "AzureDataLakeStoreLinkedService",
            "typeProperties": {
                "folderPath": "datalake/input/",
                "fileName": "SearchLog.tsv",
                "format": {
                    "type": "TextFormat",
                    "rowDelimiter": "\n",
                    "columnDelimiter": "\t"
                }
            },
            "availability": {
                "frequency": "Day",
                "interval": 1
            }
        }
    }   

### <a name="output-dataset"></a>Výstup datové sady
V tomto příkladu výstupní data vytvořené pomocí skriptu U SQL uložené v úložišti jezera dat Azure (datalake/výstupní složku). 

    {
        "name": "EventsByRegionTable",
        "properties": {
            "type": "AzureDataLakeStore",
            "linkedServiceName": "AzureDataLakeStoreLinkedService",
            "typeProperties": {
                "folderPath": "datalake/output/"
            },
            "availability": {
                "frequency": "Day",
                "interval": 1
            }
        }
    }

### <a name="sample-data-lake-store-linked-service"></a>Ukázková Data jezera úložiště propojené služby
Tady je definici vzorku úložiště jezera dat Azure propojené služba používaná vstupní a výstupní datové sady. 

    {
        "name": "AzureDataLakeStoreLinkedService",
        "properties": {
            "type": "AzureDataLakeStore",
            "typeProperties": {
                "dataLakeUri": "https://<accountname>.azuredatalakestore.net/webhdfs/v1",
                "sessionId": "<session ID>",
                "authorization": "<authorization URL>"
            }
        }
    }

Článek [přesunutí dat do a z úložiště jezera dat Azure](data-factory-azure-datalake-connector.md) najdete v tématu popisy vlastností JSON. 

## <a name="sample-u-sql-script"></a>Ukázka skriptu U SQL 

    @searchlog =
        EXTRACT UserId          int,
                Start           DateTime,
                Region          string,
                Query           string,
                Duration        int?,
                Urls            string,
                ClickedUrls     string
        FROM @in
        USING Extractors.Tsv(nullEscape:"#NULL#");
    
    @rs1 =
        SELECT Start, Region, Duration
        FROM @searchlog
    WHERE Region == "en-gb";
    
    @rs1 =
        SELECT Start, Region, Duration
        FROM @rs1
        WHERE Start <= DateTime.Parse("2012/02/19");
    
    OUTPUT @rs1   
        TO @out
          USING Outputters.Tsv(quoting:false, dateTimeFormat:null);

Hodnoty pro **@in** a **@out** parametrů v skript U SQL předává dynamicky ADF pomocí oddílu "parametry". Naleznete v části "parametry" v definici kanálem k odesílání zpráv.

Můžete použít jiné vlastnosti například degreeOfParallelism a priority (priorita) i v definici kanálem k odesílání zpráv pro úlohy spuštěné v operačním systému služby Azure dat jezera analýzy.

## <a name="dynamic-parameters"></a>Dynamické parametry
V definici kanálem k odesílání zpráv vzorku nebo zmenšit parametry, mají přiřazenou pevně hodnotami. 

    "parameters": {
        "in": "/datalake/input/SearchLog.tsv",
        "out": "/datalake/output/Result.tsv"
    }

Je možné používat dynamické parametry místo. Příklad: 

    "parameters": {
        "in": "$$Text.Format('/datalake/input/{0:yyyy-MM-dd HH:mm:ss}.tsv', SliceStart)",
        "out": "$$Text.Format('/datalake/output/{0:yyyy-MM-dd HH:mm:ss}.tsv', SliceStart)"
    }

V tomto případě vstupní soubory jsou pořád vybraných kontakty ze složky /datalake/input a výstupní soubory se vytvářejí ve složce /datalake/output. Názvy souborů jsou dynamické podle času spuštění výsečí.  