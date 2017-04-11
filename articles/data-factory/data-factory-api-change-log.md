<properties 
    pageTitle="Data Factory – protokol změn rozhraní API .NET | Microsoft Azure" 
    description="Popisuje nejnovějších změnách, funkce doplňky, opravy chyb atd. v určitých verzi rozhraní API .NET pro Azure Data Factory." 
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
    ms.date="09/21/2016" 
    ms.author="spelluru"/>

# <a name="azure-data-factory---net-api-change-log"></a>Azure Data Factory - rozhraní API .NET – protokol změn 
Tento článek obsahuje informace o změnách SDK Factory dat Azure konkrétní verzi. O Azure Data Factory najdete nejnovější balíček NuGet [tady](https://www.nuget.org/packages/Microsoft.Azure.Management.DataFactories) 

## <a name="version-4110"></a>Verze 4.11.0
Přidání funkcí:

- Byly přidány následující typy propojené služby:
    - [OnPremisesMongoDbLinkedService](https://msdn.microsoft.com/library/mt765129.aspx)
    - [AmazonRedshiftLinkedService](https://msdn.microsoft.com/library/mt765121.aspx)
    - [AwsAccessKeyLinkedService](https://msdn.microsoft.com/library/mt765144.aspx)
- Byly přidány následující typy datovou sadu: 
    - [MongoDbCollectionDataset](https://msdn.microsoft.com/library/mt765145.aspx)
    - [AmazonS3Dataset](https://msdn.microsoft.com/library/mt765112.aspx)
- Byly přidány následující typy zdrojů kopii:
    - [MongoDbSource](https://msdn.microsoft.com/en-US/library/mt765123.aspx)

## <a name="version-4100"></a>Verze 4.10.0
- Následující vlastnosti volitelné přidané do formát textu:
    - [SkipLineCount](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.textformat.skiplinecount.aspx)
    - [FirstRowAsHeader](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.textformat.firstrowasheader.aspx)
    - [TreatEmptyAsNull](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.textformat.treatemptyasnull.aspx)
- Byly přidány následující typy propojené služby:
    - [OnPremisesCassandraLinkedService](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.onpremisescassandralinkedservice.aspx)
    - [SalesforceLinkedService](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.salesforcelinkedservice.aspx)
- Byly přidány následující typy datovou sadu:
    - [OnPremisesCassandraTableDataset](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.onpremisescassandratabledataset.aspx)
- Byly přidány následující typy zdrojů kopii:
    - [CassandraSource](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.cassandrasource.aspx)
- Přidejte [WebServiceInputs](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.azuremlbatchexecutionactivity.webserviceinputs.aspx) vlastnost AzureMLBatchExecutionActivity 
    - Povolení předávání více webové služby vstupní hodnoty pro pokus výukové počítače Azure


## <a name="version-491"></a>Verze 4.9.1

### <a name="bug-fix"></a>Oprava chyb

- Neschválení ověřování na základě WebApi pro [WebLinkedService](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.weblinkedservice.authenticationtype.aspx).

## <a name="version-490"></a>Verze 4.9.0

### <a name="feature-additions"></a>Přidání funkce

- Přidáte [EnableStaging](https://msdn.microsoft.com/library/mt767916.aspx) a [StagingSettings](https://msdn.microsoft.com/library/mt767918.aspx) vlastnosti CopyActivity. Podrobné informace o funkci najdete v článku [Fázovaná kopírovat](data-factory-copy-activity-performance.md#staged-copy) .


### <a name="bug-fix"></a>Oprava chyb

- Představte přetížení [ActivityWindowOperationExtensions.List](https://msdn.microsoft.com/library/mt767915.aspx) metodu, která trvá [ActivityWindowsByActivityListParameters](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.activitywindowsbyactivitylistparameters.aspx) instance.
- Označte [WriteBatchSize](https://msdn.microsoft.com/library/dn884293.aspx) a [WriteBatchTimeout](https://msdn.microsoft.com/library/dn884245.aspx) volitelné v CopySink.

## <a name="version-480"></a>Verze 4.8.0

### <a name="feature-additions"></a>Přidání funkce
- Následující vlastnosti volitelné přidané do Kopírovat typ aktivity povolte ladění výkonu kopie:
    - [ParallelCopies](https://msdn.microsoft.com/library/mt767910.aspx)
    - [CloudDataMovementUnits](https://msdn.microsoft.com/library/mt767912.aspx)

## <a name="version-470"></a>Verze 4.7.0

### <a name="feature-additions"></a>Přidání funkce
* Přidat nový typ StorageFormat [OrcFormat](https://msdn.microsoft.com/library/mt723391.aspx) typ kopírování souborů ve sloupcovém formátu (ORC) optimalizované řádku.
* Přidáte [AllowPolyBase](https://msdn.microsoft.com/library/mt723396.aspx) a PolyBaseSettings vlastnosti SqlDWSink.
    * Umožňuje použití informačních PolyBase zkopírujte data do SQL datový sklad.

## <a name="version-461"></a>Verze 4.6.1

### <a name="bug-fixes"></a>Opravy chyb
* Žádost HTTP pro vypsání aktivity windows se dá vyřešit.
    * Odebere skupinu název zdroje a název factory dat z datové žádost o.

## <a name="version-460"></a>Verze 4.6.0

### <a name="feature-additions"></a>Přidání funkce

- Následující vlastnosti přidané do [PipelineProperties](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.pipelineproperties_properties.aspx):
    - [PipelineMode](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.pipelineproperties.pipelinemode.aspx)
    - [ExpirationTime](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.pipelineproperties.expirationtime.aspx)
    - [Datové sady](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.pipelineproperties.datasets.aspx)
- Následující vlastnosti přidané do [PipelineRuntimeInfo](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.common.models.pipelineruntimeinfo.aspx):
    - [PipelineState](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.common.models.pipelineruntimeinfo.pipelinestate.aspx)
- Přidali novou [StorageFormat](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.storageformat.aspx) typ [JsonFormat](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.jsonformat.aspx) definovat datové sady s daty, která je ve formátu JSON. 

## <a name="version-450"></a>Verze 4.5.0

### <a name="feature-additions"></a>Přidání funkce
* Přidané [seznam operace aktivitu okna](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.activitywindowoperationsextensions.aspx).
    * Přidané metod k načtení aktivitu windows s filtry založené na typy entit (to znamená dat továrny datové sady, kanály a aktivity).
* Byly přidány následující typy propojené služby: 
    * [ODataLinkedService](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.odatalinkedservice.aspx), [WebLinkedService](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.weblinkedservice.aspx)
* Byly přidány následující typy datovou sadu: 
    * [ODataResourceDataset](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.odataresourcedataset.aspx), [WebTableDataset](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.webtabledataset.aspx)
* Byly přidány následující typy zdrojů kopii:  
    * [WebSource](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.websource.aspx)

## <a name="version-440"></a>Verze 4.4.0

### <a name="feature-additions"></a>Přidání funkce

- Tento typ propojené služby byly případně přidané jako zdroje dat a propadů za kopii:
    - [AzureStorageSasLinkedService](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.azurestoragesaslinkedservice.aspx). Naleznete v tématu [Propojené služba Azure úložiště přidružení zabezpečení](data-factory-azure-blob-connector.md#azure-storage-sas-linked-service) informací vysvětlujících a příklady. 

## <a name="version-430"></a>Verze 4.3.0

### <a name="feature-additions"></a>Přidání funkce

- Následující nebyla propojená služby typy některá někdo přidá jako zdroje dat pro kopírovat aktivity:
    - [HdfsLinkedService](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.hdfslinkedservice.aspx). Naleznete v tématu [přesunutí dat z HDFS pomocí Data Factory](data-factory-hdfs-connector.md) informací vysvětlujících a příklady. 
    - [OnPremisesOdbcLinkedService](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.onpremisesodbclinkedservice.aspx). Naleznete v tématu [přesunutí dat ODBC z dat ukládá pomocí Azure Data Factory](data-factory-odbc-connector.md) koncepční informace a příklady. 

## <a name="version-420"></a>Verze 4.2.0

### <a name="feature-additions"></a>Přidání funkce

- Tento typ nového aktivitu přidala: [AzureMLUpdateResourceActivity](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuremlupdateresourceactivity.aspx). Podrobnosti o aktivitě, najdete v tématu [aktualizace ML Azure modely použití aktivity aktualizace zdroje](data-factory-azure-ml-batch-execution-activity.md#updating-azure-ml-models-using-the-update-resource-activity).
- Nové volitelné vlastnost [updateResourceEndpoint](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuremllinkedservice.updateresourceendpoint.aspx) přidala [AzureMLLinkedService předmětu](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuremllinkedservice.aspx). 
- Vlastnosti [LongRunningOperationInitialTimeout](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.datafactorymanagementclient.longrunningoperationinitialtimeout.aspx) a [LongRunningOperationRetryTimeout](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.datafactorymanagementclient.longrunningoperationretrytimeout.aspx) přidané do [DataFactoryManagementClient](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.datafactorymanagementclient.aspx) předmětu. 
- Povolte Konfigurace časové limity pro volání klientů ke službě Data Factory. 


## <a name="version-410"></a>Verze 4.1.0

### <a name="feature-additions"></a>Přidání funkce
* Byly přidány následující typy propojené služby: 
    * [AzureDataLakeStoreLinkedService](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuredatalakestorelinkedservice.aspx)
    * [AzureDataLakeAnalyticsLinkedService](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuredatalakeanalyticslinkedservice.aspx)
* Byly přidány následující typy aktivity: 
    * [DataLakeAnalyticsUSQLActivity](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.datalakeanalyticsusqlactivity.aspx)
* Byly přidány následující typy datovou sadu: 
    * [AzureDataLakeStoreDataset](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuredatalakestoredataset.aspx)
* Byly přidány následující typy zdrojových a jímky kopírovat aktivity:
    * [AzureDataLakeStoreSource](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuredatalakestoresource.aspx)
    * [AzureDataLakeStoreSink](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuredatalakestoresink.aspx)


## <a name="version-401"></a>Verze 4.0.1

### <a name="breaking-changes"></a>Zrušení změn
Přejmenované následujících tříd. Pokud chcete nová jména byly původní názvy tříd 4.0.0 uvolnit dříve než. 
 
Jméno 4.0.0 | Jméno 4.0.1
:------------ | :------------ 
AzureSqlDataWarehouseDataset | [AzureSqlDataWarehouseTableDataset](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuresqldatawarehousetabledataset.aspx)
AzureSqlDataset | [AzureSqlTableDataset](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuresqltabledataset.aspx)
AzureDataset | [AzureTableDataset](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuretabledataset.aspx)
OracleDataset | [OracleTableDataset](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.oracletabledataset.aspx)
RelationalDataset | [RelationalTableDataset](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.relationaltabledataset.aspx)
SqlServerDataset | [SqlServerTableDataset](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.sqlservertabledataset.aspx)


## <a name="version-400"></a>Verze 4.0.0

### <a name="breaking-changes"></a>Zrušení změn



- Přejmenované následující třídy/rozhraní.

| Původní název | Nový název |
| :-------- | :-------- |
| ITableOperations | [IDatasetOperations](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.idatasetoperations.aspx) |  
| Tabulky | [Datové sady](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.dataset.aspx) | 
| TableProperties | [DatasetProperties](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.datasetproperties.aspx) | 
| TableTypeProprerties | [DatasetTypeProperties](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.datasettypeproperties.aspx) |
| TableCreateOrUpdateParameters | [DatasetCreateOrUpdateParameters](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.datasetcreateorupdateparameters.aspx) | 
| TableCreateOrUpdateResponse | [DatasetCreateOrUpdateResponse](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.datasetcreateorupdateresponse.aspx) | 
| TableGetResponse | [DatasetGetResponse](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.datasetgetresponse.aspx) | 
| TableListResponse | [DatasetListResponse](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.datasetlistresponse.aspx) |
| CreateOrUpdateWithRawJsonContentParameters | [DatasetCreateOrUpdateWithRawJsonContentParameters](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.datasetcreateorupdatewithrawjsoncontentparameters.aspx) | 
    

- **Seznam** metod vrátit stránkovaného výsledky. Obsahuje-li odpověď vlastnost **NextLink** neprázdné, musí klientské aplikaci pokračovat načítání další stránky, dokud se vracejí všechny stránky.  Tady je příklad: 

        PipelineListResponse response = client.Pipelines.List("ResourceGroupName", "DataFactoryName");
        var pipelines = new List<Pipeline>(response.Pipelines);
    
        string nextLink = response.NextLink;
        while (string.IsNullOrEmpty(response.NextLink))
        {
            PipelineListResponse nextResponse = client.Pipelines.ListNext(nextLink);
            pipelines.AddRange(nextResponse.Pipelines);
    
            nextLink = nextResponse.NextLink;
        }
    
- **Seznam** kanálem k odesílání zpráv rozhraní API Vrátí souhrn kanálu místo úplné podrobnosti. Například aktivity v přehledu kanálem k odesílání zpráv obsahovat pouze název a typ.

### <a name="feature-additions"></a>Přidání funkce
- Třídy [SqlDWSink](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.sqldwsink.aspx) podporuje dva nové vlastnosti, **SliceIdentifierColumnName** a **SqlWriterCleanupScript**, aby podporoval co idempotent kopírovat do datový sklad SQL Azure. Naleznete v článku [Datový sklad SQL Azure](data-factory-azure-sql-data-warehouse-connector.md) , konkrétně [mechanismus 1](data-factory-azure-sql-data-warehouse-connector.md#mechanism-1) a [2 mechanismus](data-factory-azure-sql-data-warehouse-connector.md#mechanism-2) oddíly, podrobnosti o těchto vlastností.

- Nyní podporuje spuštění uložené procedury databáze SQL Azure a Azure SQL datový sklad zdrojů v rámci aktivity kopírovat. Třídy [SqlSource](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.sqlsource.aspx) a [SqlDWSource](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.sqldwsource.aspx) mají následující vlastnosti: **SqlReaderStoredProcedureName** a **StoredProcedureParameters**. Další informace o těchto vlastností naleznete v článcích [Databáze SQL Azure](data-factory-azure-sql-connector.md#sqlsource) a [Azure SQL datový sklad](data-factory-azure-sql-data-warehouse-connector.md#sqldwsource) na Azure.com.  