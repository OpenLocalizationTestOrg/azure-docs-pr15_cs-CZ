<properties
    pageTitle="Všech témat služby Data Factory | Microsoft Azure"
    description="Tabulka všech témat pro službu Azure s názvem Data Factory, která jsou na http://azure.microsoft.com/documentation/articles/, název a popis."
    services="data-factory"
    documentationCenter=""
    authors="spelluru"
    manager="jhubbard"
    editor="MightyPen"/>

<tags
    ms.service="data-factory"
    ms.workload="data-factory"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/05/2016"
    ms.author="spelluru"/>


# <a name="all-topics-for-azure-data-factory-service"></a>Všech témat služby Azure Data Factory

Toto téma obsahuje seznam všech téma, které slouží k použití přímo ve službě **Factory dat** z Azure. Hledat tuto webovou stránku klíčových slov pomocí **Kombinace kláves Ctrl + F**, najdete v tématech aktuální zájmu.




## <a name="new"></a>Nový

| &nbsp; | Název | Popis |
| --: | :-- | :-- |
| 1 | [Přesunutí dat z Amazon Redshift pomocí Azure Data Factory](data-factory-amazon-redshift-connector.md) | Informace o tom, jak přesunout data z Amazon Redshift pomocí Azure Data Factory. |
| 2 | [Přesunutí dat z Amazon jednoduché úložiště služby pomocí Azure Data Factory](data-factory-amazon-simple-storage-service-connector.md) | Informace o tom, jak přesunout data z Amazon jednoduché úložiště služby (S3) pomocí Azure Data Factory. |
| 3 | [Azure Data Factory – Průvodce kopírováním](data-factory-azure-copy-wizard.md) | Informace o tom, jak pomocí Průvodce dat Azure Factory Kopírovat zkopírujte data z podporovaných zdrojů dat do propadů. |
| 4 | [Kurz: Vytvoření výrobce první Azure dat pomocí datového Factory REST API](data-factory-build-your-first-pipeline-using-rest-api.md) | V tomto kurzu vytvoříte ukázková Azure Data Factory kanálem k odesílání zpráv pomocí rozhraní REST API dat Factory. |
| 5 | [Kurz: Vytváření kanálů se kopie aktivity pomocí rozhraní API .NET](data-factory-copy-activity-tutorial-using-dotnet-api.md) | V tomto kurzu vytvoříte Azure Data Factory kanálu aktivitu kopírovat pomocí rozhraní API .NET. |
| 6 | [Kurz: Vytváření kanálů se kopie aktivity pomocí rozhraní REST API](data-factory-copy-activity-tutorial-using-rest-api.md) | V tomto kurzu vytvoříte Azure Data Factory kanálu aktivitu kopírovat pomocí rozhraní REST API. |
| 7 | [Průvodce kopírováním Factory dat](data-factory-copy-wizard.md) | Informace o tom, jak pomocí Průvodce kopírovat Factory dat ke kopírování dat z podporovaných zdrojů dat do propadů. |
| 8 | [Brána pro správu dat](data-factory-data-management-gateway.md) | Nastavení brány dat přesun dat mezi místním prostředím a cloudu. Přesunutí dat pomocí Brána pro správu dat v Azure Data Factory. |
| 9 | [Přesunutí dat z databáze Cassandra místní pomocí Azure Data Factory](data-factory-onprem-cassandra-connector.md) | Informace o tom, jak přesunout data z místního Cassandra databáze pomocí Azure Data Factory. |
| 10 | [Přesunutí dat z MongoDB pomocí Azure Data Factory](data-factory-on-premises-mongodb-connector.md) | Informace o tom, jak přesunout data z databáze MongoDB pomocí Azure Data Factory. |
| 11 | [Přesunutí dat ze služby Salesforce pomocí Azure Data Factory](data-factory-salesforce-connector.md) | Informace o tom, jak přesunout data ze služby Salesforce pomocí Azure Data Factory. |


## <a name="updated-articles-data-factory"></a>Aktualizované články, Data Factory

V této části jsou uvedeny články, které byly aktualizovány naposledy, kde aktualizace proběhla velká nebo významný. Pro každý aktualizovaný článek se zobrazí hrubá fragment kódu přidané markdown textu. V rozsahu kalendářních dat **2016-08-22** **2016 10 05**aktualizovaly se tyto články.

| &nbsp; | Článek | Aktualizované text, fragment kódu | Aktualizace |
| --: | :-- | :-- | :-- |
| 12 | [Azure Data Factory – protokol změn rozhraní API .NET](data-factory-api-change-log.md) | Tento článek obsahuje informace o změnách SDK Factory dat Azure konkrétní verzi. O Azure Data Factory tady (https://www.nuget.org/packages/Microsoft.Azure.Management.DataFactories) **verze 4.11.0** funkce dodatky najdete nejnovější balíček NuGet: / byly přidány následující typy propojené služby: AmazonRedshiftLinkedService - OnPremisesMongoDbLinkedService (https://msdn.microsoft.com/library/mt765129.aspx) - (https://msdn.microsoft.com/library/mt765121.aspx) - AwsAccessKeyLinkedService (https://msdn.microsoft.com/library/mt765144.aspx) / byly přidány následující typy datovou sadu: AmazonS3Dataset - MongoDbCollectionDataset (https://msdn.microsoft.com/library/mt765145.aspx) - (https://msdn.microsoft.com/library/mt765112.aspx) / byly přidány následující typy zdrojů kopie :-MongoDbSource (https://msdn.microsoft.com/en-US/library/mt765123.aspx) **verze 4.10.0** / následující volitelná vlastnosti přidané do formát textu:-lyží | 2016-09-22 |
| 13 | [Přesunutí dat do a z objektů Blob Azure pomocí Azure Data Factory](data-factory-azure-blob-connector.md) |  / copyBehavior / definuje kopírovat chování po zdroji BlobSource nebo systému souborů.  / **PreserveHierarchy:** zachová hierarchii souborů do cílové složky. Relativní cestu zdrojového souboru do složky zdroje je shodný relativní cestu cílový soubor do cílové složky. br /. br /. **FlattenHierarchy:** všechny soubory ve složce zdroje jsou v první úrovně cílovou složku. Cílové soubory mít automatické generování název. .br /. br /. **MergeFiles: (výchozí)** sloučí všechny soubory ve složce zdroje na jeden soubor. Pokud není zadán název souboru a objektů Blob, název sloučeného souboru by měl být se zadaným názvem; v ostatních případech bude název automaticky generované souboru.  / Ne / **BlobSource** taky podporuje tyto dvě vlastnosti z důvodu zpětné kompatibility. / **treatEmptyAsNull**: Určuje, jestli se považovat za prázdnou hodnotu null nebo prázdný řetězec. / **skipHeaderLineCount** - Určuje, kolik řádků potřebovat vynechány. Vztahuje se pouze při zadávání datovou sadu používá formát textu. Podobně **BlobSink** podporuje ář | 2016 09 28 |
| 14 | [Vytvoření prediktivní potrubí pomocí Azure počítače výukové aktivit](data-factory-azure-ml-batch-execution-activity.md) | **Webová služba vyžaduje více vstupů** Pokud webová služba trvá více vstupy, použijte místo použití **webServiceInput**vlastnost **webServiceInputs** . Datové sady, které se odkazuje podle **webServiceInputs** musí být součástí aktivity **vstupů**. V Azure ML pokusu webové služby vstupní a výstupní porty a protokoly globální parametry mít výchozí názvy ("input1", "input2"), které je možné upravit. Názvy, které používáte pro webServiceInputs webServiceOutputs, nastavení a globalParameters se musí přesně shodovat názvy při pokusech. Na stránce Nápověda spuštění dávky pro Azure ML koncový bod pro ověření očekávané mapování můžete zobrazit ukázkové datové žádost.    {"název": "PredictivePipeline", "vlastnosti": {"Popis": "použití modelu AzureML", "aktivity": {"název": "MLActivity", "typ": "AzureMLBatchExecution", "Popis": "předpovídání pomocí analýzy v listu při zadávání", "vstupů": {"název": "inputDataset1"}, {"název": "inputDatase | 2016-09-13 |
| 15 | [Zkopírujte aktivitu výkon a ladění příručka](data-factory-copy-activity-performance.md) | 1. **navázat směrný plán**. Během fáze vývoje testovat svého kanálu pomocí Kopírovat aktivity proti výběru zástupce data. Chcete-li omezit množství dat, se kterými pracujete s můžete Data Factory rozdělení na řezy modelu (dat – factory plánování a execution.md time-series-datasets-and-data-slices).  Shromáždit času spuštění a vlastnosti výkon při používání **sledování a Správa aplikací**. Na domovské stránce Data Factory zvolte **Monitor a spravovat** . Ve stromovém zobrazení vyberte **datovou sadu výstupu**. V seznamu **Aktivity Windows** zvolte Kopírovat aktivity spustit. Doba trvání kopírovat aktivity a velikost dat, které se zkopírují uvádí seznam **Aktivity Windows** . Výkon je uvedená v **Průzkumníku okno aktivity**. Další informace o aplikaci najdete v tématu sledování a správa Azure Data Factory potrubí při používání sledování a Správa aplikací (dat – factory-monitor – Správa app.md).  ! Aktivity spustit podrobnosti (./media/data-factory-copy-activity-performance/mmapp-activity-run-details.pn | 2016-09-27 |
| 16 | [Kurz: Vytváření kanálů se kopie aktivity pomocí aplikace Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) |   Poznámka: následující body:-datovou sadu **Typ** je nastavený na **AzureBlob**.     - **linkedServiceName** je nastavený na **AzureStorageLinkedService**. Tuto službu propojené jste vytvořili v kroku 2.     - **cesta_ke_složce** je nastavený na kontejner **adftutorial** . Můžete také zadat název objektů blob ve složce pomocí vlastnosti **název souboru** . Protože nejsou zadáním názvu objektů blob, data ze všech objektů BLOB v kontejneru považovány za vstupní data.    -Formát **Typ** je nastavený na **Formát textu** – zobrazí se dvě pole v text soubor ΓÇô **jméno** a **Příjmení** ΓÇô odděleni znak čárky (**columnDelimiter**) – **dostupnost** je nastavený na **každou hodinu** (**počet_plateb** je nastavený na **hodiny** a **interval** je nastavena na hodnotu **1**). Proto Data Factory vyhledá zadávání dat každou hodinu v kořenové složce kontejneru objektů blob (**adftutorial**) jste zadali.  Pokud nezadáte **název souboru** pro **zadávání** datovou sadu, všechny soubory nebo objekty BLOB ve složce vstupní (**cesta_ke_složce**) jsou consid | 2016-09 – 29 |
| 17 | [Vytvoření, sledování a správa továrny Azure dat pomocí Data Factory .NET SDK](data-factory-create-data-factories-programmatically.md) | Poznamenejte si ID aplikace a heslo (tajná klienta) a použijte v tomto návodu. **Získání Azure předplatné a klientovi ID** Pokud nemáte nejnovější verzi prostředí PowerShell nainstalované v počítači, postupujte podle pokynů v tématu jak instalace a konfigurace prostředí PowerShell Azure (... / prostředí powershell nainstalovat configure.md) článek případně ji nainstalujte. 1. Spusťte Azure PowerShell a spusťte tento příkaz 2. Spusťte tento příkaz a zadejte uživatelské jméno a heslo, které používáte pro přihlášení k portálu Azure.         Přihlášení AzureRmAccount Pokud máte jenom jednu Azure předplatné přidružený k tomuto účtu, se nemusí provádět následující dva kroky. 3. Spusťte tento příkaz Zobrazit všechna předplatná pro tento účet.       Get-AzureRmSubscription 4. Spusťte tento příkaz vyberte předplatné, ke kterým chcete pracovat s. Nahraďte název předplatného Azure **NameOfAzureSubscription** .       Get-AzureRmSubscription - SubscriptionName NameOfAzureSubscription / AzureRmCo nastavení | 2016-09-14 |
| 18 | [Kanály a aktivity v Azure Data Factory](data-factory-create-pipelines.md) |       "start": "2016 – 07-12T00:00:00Z", "konec": "2016 – 07-13T00:00:00Z"}} Poznámka: následující body: / v části aktivity je pouze jeden aktivity, jehož **Typ** je nastavený na **Kopírovat**. / Vstupní aktivity je nastavený na **InputDataset** a výstup pro aktivitu je nastavený na **OutputDataset**. A v části **typeProperties** **BlobSource** zadaný jako typ zdroje a **SqlSink** není zadán psaní jímky. Úplné informace o vytváření tohoto kanálu, v tématu kurz: kopírování dat z úložiště objektů Blob k SQL databázi (data-factory-copy-data-from-azure-blob-storage-to-sql-database.md). **Ukázka transformace kanálem k odesílání zpráv** V následující ukázkové kanálu je jedna aktivita typu **HDInsightHive** v části **aktivity** . V tomto příkladu aktivity HDInsight podregistru (dat – factory podregistru activity.md) transformace dat z úložiště objektů Blob Azure spuštěním soubor skriptu podregistru Azure HDInsight Hadoop clusteru.  {"název": "TransformPipeline", "p | 2016-09-27 |
| 19 | [Transformace dat v Azure Data Factory](data-factory-data-transformation-activities.md) | Data Factory podporuje následující činnosti transformace dat, které můžete přidat potrubí (dat – factory – vytvoření pipelines.md) buď jednotlivě nebo zřetězené jiné aktivitě. .  AZURE. Poznámka návodu s podrobnými pokyny najdete v článku Vytvoření kanálu v článku podregistru transformace (data-factory-build-your-first-pipeline.md). **HDInsight podregistru aktivity** HDInsight podregistru aktivity v kanálu Data Factory provede podregistru dotazy na vlastní nebo na vyžádání/Linux serveru s Windows HDInsight obrázku. Najdete v článku (dat – factory podregistru activity.md) podregistru aktivity podrobné informace o této aktivity. **Prasátko HDInsight aktivity** Prasátko HDInsight aktivity v kanálu Data Factory provede Prasátko dotazy na vlastní nebo na vyžádání/Linux serveru s Windows HDInsight obrázku. Najdete v článku (dat – factory Prasátko activity.md) Prasátko aktivity podrobné informace o této aktivity. **HDInsight MapReduce aktivity** HDInsight MapReduce aktivity v kanálu Data Factory je prováděn MapReduce programy y | 2016-09-26 |
| 20 | [Plánování Factory dat a spuštění](data-factory-scheduling-and-execution.md) | CopyActivity2 by byl spuštěn pouze v případě CopyActivity1 úspěšném a Dataset2 je k dispozici. Tady je ukázka kanál JSON: {"název": "ChainActivities", "vlastnosti": {"Popis": "Spustit aktivity v posloupnosti", "aktivity": {"typ": "Kopírovat", "typeProperties": {"zdrojový": {"typ": "BlobSource"}, "jímky": {"typ": "BlobSink", "copyBehavior": "PreserveHierarchy", "writeBatchSize": 0; "writeBatchTimeout": "00: 00:00"}}, "vstupů": {"název": "Dataset1"}, "výstupy": {"název": "Dataset2"}, "zásad": {"časový limit": "01: 00:00"}, "Plánovač": {"počet_plateb": "Hodinu", "interval": 1}, "název": "CopyFromBlob1ToBlob2", "Popis": "Data můžete kopírovat z objektů blob do jiného"}, {"typ": "Kopírovat", "typeProperties": {"zdrojový": {"typ": "BlobSource"}, "jímky" : {"typ": "BlobSink", "writeBatchSize": 0; "writeBatchTimeout": "00: 00:00"}}, "v | 2016 09 28 |





## <a name="tutorials"></a>Výukové programy

| &nbsp; | Název | Popis |
| --: | :-- | :-- |
| 21 | [Kurz: Vytvoření první pipeline na zpracování dat pomocí Hadoop obrázku](data-factory-build-your-first-pipeline.md) | Tento kurz Azure Data Factory ukazuje, jak můžete vytvořit a naplánovat si data factory zpracovávající data pomocí skriptu podregistru Hadoop clusteru. |
| 22 | [Kurz: Vytvoření výrobce první Azure dat pomocí šablony správce prostředků Azure](data-factory-build-your-first-pipeline-using-arm.md) | V tomto kurzu vytvoříte ukázková Azure Data Factory kanálem k odesílání zpráv pomocí šablony správce prostředků Azure. |
| 23 | [Kurz: Vytvoření výrobce první Azure dat Azure portálu](data-factory-build-your-first-pipeline-using-editor.md) | V tomto kurzu vytvoříte ukázková Azure Data Factory kanálem k odesílání zpráv pomocí editoru Factory dat na portálu Azure. |
| 24 | [Kurz: Vytvoření výrobce první Azure dat pomocí prostředí PowerShell Azure](data-factory-build-your-first-pipeline-using-powershell.md) | V tomto kurzu vytvoříte ukázková Azure Data Factory kanálem k odesílání zpráv pomocí prostředí PowerShell Azure. |
| 25 | [Kurz: Sestavení Azure první dat factory pomocí Microsoft Visual Studio](data-factory-build-your-first-pipeline-using-vs.md) | V tomto kurzu vytvoříte ukázková Azure Data Factory kanálem k odesílání zpráv pomocí aplikace Visual Studio. |
| 26 | [Kurz: Vytváření kanálů s kopírovat aktivitou portálu Azure](data-factory-copy-activity-tutorial-using-azure-portal.md) | V tomto kurzu vytvoříte Azure Data Factory kanálu aktivitu kopírovat pomocí editoru Factory dat na portálu Azure. |
| 27 | [Kurz: Vytváření kanálů se kopie aktivity pomocí prostředí PowerShell Azure](data-factory-copy-activity-tutorial-using-powershell.md) | V tomto kurzu vytvoříte Azure Data Factory kanálu aktivitu kopírovat pomocí prostředí PowerShell Azure. |
| 28 | [Kurz: Vytváření kanálů se kopie aktivity pomocí aplikace Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) | V tomto kurzu vytvoříte Azure Data Factory kanálu aktivitu kopírovat pomocí aplikace Visual Studio. |
| 29 | [Kopírování dat z úložiště objektů Blob k SQL databázi pomocí Data Factory](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) | Tento kurz se dozvíte, jak můžete kopírovat aktivity v Azure Data Factory kanálu ke kopírování dat z úložiště objektů Blob k SQL databázi. |
| 30 | [Kurz: Vytváření kanálů se kopie aktivity pomocí Průvodce kopírováním Factory dat](data-factory-copy-data-wizard-tutorial.md) | V tomto kurzu vytvoříte Azure Data Factory kanálu aktivitu kopírovat pomocí Průvodce kopírováním nepodporuje Data Factory |



## <a name="data-movement"></a>Přesun dat

| &nbsp; | Název | Popis |
| --: | :-- | :-- |
| 31 | [Přesunutí dat do a z objektů Blob Azure pomocí Azure Data Factory](data-factory-azure-blob-connector.md) | Informace o kopírování dat objektů blob Azure Data Factory. Použití našimi ukázkovými: kopírování dat z úložiště objektů Blob Azure nebo databáze SQL Azure. |
| 32 | [Přesunutí dat do a z úložiště jezera dat Azure pomocí Azure Data Factory](data-factory-azure-datalake-connector.md) | Naučte se přesuňte data z úložiště jezera dat Azure pomocí Azure Data Factory |
| 33 | [Přesunutí dat a od DocumentDB pomocí Azure Data Factory](data-factory-azure-documentdb-connector.md) | Zjistěte, jak přesunout data do/z kolekce Azure DocumentDB pomocí Azure Data Factory |
| 34 | [Přesunutí dat do a z databáze SQL Azure pomocí Azure Data Factory](data-factory-azure-sql-connector.md) | Další informace o přesunutí dat z databáze SQL Azure pomocí Azure Data Factory. |
| 35 | [Přesunutí dat a od datový sklad SQL Azure pomocí Azure Data Factory](data-factory-azure-sql-data-warehouse-connector.md) | Naučte se přesouvat dat z Azure SQL datový sklad pomocí Azure Data Factory |
| 36 | [Přesunutí dat do a z tabulky Azure pomocí Azure Data Factory](data-factory-azure-table-connector.md) | Zjistěte, jak chcete přesunout data z úložiště tabulek Azure pomocí Azure Data Factory. |
| 37 | [Zkopírujte aktivitu výkon a ladění příručka](data-factory-copy-activity-performance.md) | Informace o klíčových faktory, které ovlivňují výkon přesun dat v Azure Data Factory při použití kopírovat aktivity. |
| 38 | [Přesunutí dat pomocí kopírování aktivity](data-factory-data-movement-activities.md) | Další informace o přesun dat v Data Factory potrubí: migrace dat mezi cloudové úložiště a mezi místní úložiště a cloudu. Pomocí kopírování aktivity. |
| 39 | [Poznámky k verzi pro Brána pro správu dat](data-factory-gateway-release-notes.md) | Poznámky k verzi text Brána pro správu dat |
| 40 | [Přesuňte data z místního HDFS pomocí Azure Data Factory](data-factory-hdfs-connector.md) | Informace o tom, jak přesunout data z místního HDFS pomocí Azure Data Factory. |
| 41 | [Sledování a správa Azure Data Factory potrubí pomocí nových sledování a Správa aplikací](data-factory-monitor-manage-app.md) | Naučte se používat ke sledování a správa dat Azure továrny a potrubí sledování a Správa aplikací. |
| 42 | [Přesouvání dat mezi zdrojů na pracovišti a cloud se Brána pro správu dat](data-factory-move-data-between-onprem-and-cloud.md) | Nastavení brány dat přesun dat mezi místním prostředím a cloudu. Přesunutí dat pomocí Brána pro správu dat v Azure Data Factory. |
| 43 | [Přesunutí dat z OData zdroj pomocí Azure Data Factory](data-factory-odata-connector.md) | Informace o tom, jak přesunout data ze zdrojů OData pomocí Azure Data Factory. |
| 44 | [Přesunutí dat ODBC z dat ukládají pomocí Azure Data Factory](data-factory-odbc-connector.md) | Informace o tom, jak přesunout data z úložiště dat ODBC pomocí Azure Data Factory. |
| 45 | [Přesunutí dat z DB2 pomocí Azure Data Factory](data-factory-onprem-db2-connector.md) | Informace o přesunutí dat z databáze DB2 pomocí Azure Data Factory |
| 46 | [Přesunutí dat do a z systému souborů místní pomocí Azure Data Factory](data-factory-onprem-file-system-connector.md) | Naučte se přesouvat data do systému souborů místní pomocí Azure Data Factory. |
| 47 | [Přesunutí dat z MySQL pomocí Azure Data Factory](data-factory-onprem-mysql-connector.md) | Informace o tom, jak přesunout data z databáze MySQL pomocí Azure Data Factory. |
| 48 | [Přesuňte data z místního Oracle pomocí Azure Data Factory](data-factory-onprem-oracle-connector.md) | Další informace o přesunutí dat z databáze Oracle, který je místní pomocí Azure Data Factory. |
| 49 | [Přesuňte data z PostgreSQL pomocí Azure Data Factory](data-factory-onprem-postgresql-connector.md) | Informace o tom, jak přesunout data z databáze PostgreSQL pomocí Azure Data Factory. |
| 50 | [Přesuňte data z Sybase pomocí Azure Data Factory](data-factory-onprem-sybase-connector.md) | Informace o tom, jak přesunout data z databáze Sybase pomocí Azure Data Factory. |
| 51 | [Přesuňte data z Teradata pomocí Azure Data Factory](data-factory-onprem-teradata-connector.md) | Další informace o Teradata konektor služby Data Factory, který umožňuje přesuňte data z databáze Teradata |
| 52 | [Přesunutí dat do a z SQL serveru místní nebo na IaaS (Azure OM) pomocí Azure Data Factory](data-factory-sqlserver-connector.md) | Informace o tom, jak přesunout data z databáze SQL serveru, který je místní nebo OM Azure pomocí Azure Data Factory. |
| 53 | [Přesunutí dat ze zdroje webové tabulky pomocí Azure Data Factory](data-factory-web-table-connector.md) | Informace o tom, jak přesunout data z místního tabulky na webovou stránku pomocí Azure Data Factory. |



## <a name="data-transformation"></a>Transformace dat

| &nbsp; | Název | Popis |
| --: | :-- | :-- |
| 54 | [Vytvoření prediktivní potrubí pomocí Azure počítače výukové aktivit](data-factory-azure-ml-batch-execution-activity.md) | Popisuje, jak vytvořit vytvořit prediktivní potrubí pomocí Azure Data Factory a Azure počítače výuka |
| 55 | [Výpočet propojené služby](data-factory-compute-linked-services.md) | Informace o výpočetním enviornments, můžete v Azure Data Factory potrubí transformace/proces data. |
| 56 | [Zpracovat rozsáhlé datové sady pomocí Data Factory a list](data-factory-data-processing-using-batch.md) | Popisuje, jak chcete zpracovat velké objemy dat v Azure Data Factory kanálu pomocí funkce souběžné zpracování Azure listu. |
| 57 | [Transformace dat v Azure Data Factory](data-factory-data-transformation-activities.md) | Zjistěte, jak k transformaci dat nebo dat obrázku v Azure Data Factory pomocí Hadoop, výukové počítače nebo jezera analýzy dat Azure. |
| 58 | [Aktivity streamování Hadoop](data-factory-hadoop-streaming-activity.md) | Zjistěte, jak pomocí Hadoop datových proudů aktivity v Azure dat factory spustit přenos Hadoop programy na služba/svůj vlastní clusteru HDInsight. |
| 59 | [Aktivity podregistru](data-factory-hive-activity.md) | Zjistěte, jak pomocí podregistru aktivity v Azure dat factory spouštění dotazů podregistru na služba/svůj vlastní clusteru HDInsight. |
| 60 | [Vyvolání MapReduce programů Data Factory](data-factory-map-reduce.md) | Naučte se zpracování dat spuštěním MapReduce programy Azure HDInsight clusteru z Azure dat factory. |
| 61 | [Prasátko aktivity](data-factory-pig-activity.md) | Zjistěte, jak pomocí aktivity Prasátko v Azure dat factory spouštěly skripty Prasátko na služba/svůj vlastní clusteru HDInsight. |
| 62 | [Vyvolání Spark programů Data Factory](data-factory-spark.md) | Naučte se vyvolat Spark programů factory Azure dat pomocí MapReduce aktivity. |
| 63 | [SQL Server uložené procedury aktivity](data-factory-stored-proc-activity.md) | Zjistěte, jak pomocí SQL Server uložené procedury aktivitu vyvolat proceduru uloženou v databázi SQL Azure nebo Azure SQL datový sklad od Data Factory kanálu. |
| 64 | [Použití vlastní aktivity v Azure Data Factory kanálu](data-factory-use-custom-activities.md) | Zjistěte, jak vytvořit vlastní aktivity a jejich použití v Azure Data Factory kanálu. |
| 65 | [Spuštění skript U SQL na analýzy jezera Azure dat z Azure Data Factory](data-factory-usql-activity.md) | Naučte se zpracování dat spuštěním skripty U SQL ve výpočetním služby Azure dat jezera analýzy. |



## <a name="samples"></a>Vzorky

| &nbsp; | Název | Popis |
| --: | :-- | :-- |
| 66 | [Azure Data Factory - vzorky](data-factory-samples.md) | Obsahuje podrobné informace o vzorcích, které jsou součástí se službou Azure Data Factory. |



## <a name="use-cases"></a>Případy použití

| &nbsp; | Název | Popis |
| --: | :-- | :-- |
| 67 | [Zákaznické případové studie](data-factory-customer-case-studies.md) | Zjistěte, jak některé z jejích zákazníků jste používali Azure Data Factory. |
| 68 | [Použít písmena – vytvoření profilu zákazníka](data-factory-customer-profiling-usecase.md) | Zjistěte, jak Azure Data Factory slouží k vytvoření založených na datech pracovního postupu (kanálem k odesílání zpráv) do profilu herní zákazníci. |
| 69 | [Použití případu – doporučení produktu](data-factory-product-reco-usecase.md) | Informace o případu použití implementovaná pomocí Azure Data Factory spolu s dalšími službami. |



## <a name="monitor-and-manage"></a>Sledování a Správa

| &nbsp; | Název | Popis |
| --: | :-- | :-- |
| 70 | [Sledování a správa Azure Data Factory potrubí](data-factory-monitor-manage-pipelines.md) | Naučte se používat portál Azure a Azure Powershellu ke sledování a správa dat Azure továrny a potrubí, kterou jste vytvořili. |



## <a name="sdk"></a>SDK

| &nbsp; | Název | Popis |
| --: | :-- | :-- |
| 71 | [Azure Data Factory – protokol změn rozhraní API .NET](data-factory-api-change-log.md) | Popisuje nejnovějších změnách, funkce doplňky, opravy chyb atd. v určitých verzi rozhraní API .NET pro Azure Data Factory. |
| 72 | [Vytvoření, sledování a správa továrny Azure dat pomocí Data Factory .NET SDK](data-factory-create-data-factories-programmatically.md) | Zjistěte, jak programově vytvářet, sledování a správa továrny Azure dat pomocí Data Factory SDK. |
| 73 | [Referenční informace pro vývojáře Factory Azure dat](data-factory-sdks.md) | Další informace o různých způsobů vytvoření, sledování a správa továrny Azure dat |



## <a name="miscellaneous"></a>Různé

| &nbsp; | Název | Popis |
| --: | :-- | :-- |
| 74 | [Azure Data Factory – nejčastější dotazy](data-factory-faq.md) | Nejčastější dotazy k Azure Data Factory. |
| 75 | [Azure Data Factory – funkce a systémových proměnných](data-factory-functions-variables.md) | Obsahuje seznam funkcí Azure Data Factory a proměnných systému |
| 76 | [Azure Data Factory - pojmenování pravidel](data-factory-naming-rules.md) | Popisuje naming pravidla pro Data Factory entity. |
| 77 | [Poradce při potížích Data Factory](data-factory-troubleshoot.md) | Zjistěte, jak řešit problémy s použitím Azure Data Factory. |

