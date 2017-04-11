<properties     
    pageTitle="Azure Data Factory - vzorky" 
    description="Obsahuje podrobné informace o vzorcích, které jsou součástí se službou Azure Data Factory." 
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
    ms.date="10/18/2016" 
    ms.author="shlo"/>

# <a name="azure-data-factory---samples"></a>Azure Data Factory - vzorky

## <a name="samples-on-github"></a>Ukázky na GitHub
[Úložiště GitHub Azure-DataFactory](https://github.com/azure/azure-datafactory) obsahuje několik ukázek, které vám pomůžou rychle množství postupně zvyšovat službou Azure Data Factory nebo upravit skripty a použít vlastní aplikaci. Složka Samples\JSON obsahuje JSON fragmenty pro běžné scénáře.

| Ukázka | Popis |
| :----- | :---------- | 
| [Návod ADF](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/ADFWalkthrough) | V tomto příkladu poskytuje začátku do konce návod pro zpracování souborů protokolů Azure Data Factory hodit data ze souborů protokolu ve přehledy. <br/><br/>V tomto návodu kanálu Data Factory shromažďují protokoly vzorku, procesy obohacuje data z protokoly s referenčních dat a transformací data, která chcete vyhodnotit efektivitu marketingové kampaně naposledy spuštěné. |
| [JSON vzorky](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/JSON) | V tomto příkladu jsou uvedeny příklady JSON pro běžné scénáře. | 
| [Ukázka stahování HTTP dat](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/HttpDataDownloaderSample) | Tato ukázka server Showcase stahování dat z HTTP koncový bod k úložišti objektů Blob Azure pomocí vlastní aktivitou .NET. |
| [Křížové ukázkové čisté aktivity doménou tečka](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/CrossAppDomainDotNetActivitySample) | V tomto příkladu umožňuje vytvářet vlastní aktivitou .NET, který není omezena pouze sestavení verzí používaných Spouštěči ADF (například WindowsAzure.Storage v4.3.0 Newtonsoft.Json v6.0.x, atd.). |
| [Spustit skript R](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/RunRScriptUsingADFSample) |  V tomto příkladu obsahuje Data Factory vlastní činnost, které se mohou sloužit k vyvolat RScript.exe. V tomto příkladu funguje jenom s vlastní clusteru HDInsight (ne na vyžádání), která na něm už má nainstalovanou R. |
| [Vyvolání Spark úlohy clusteru HDInsight Hadoop](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/Spark) | Tento příklad ukazuje, jak používat MapReduce aktivitu vyvolat Spark programu. Spark program jenom slouží ke kopírování dat z jedné kontejneru objektů Blob Azure do jiného. |
| [Analýza Twitter pomocí Azure počítače výukové dávku bodování aktivity](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/TwitterAnalysisSample-AzureMLBatchScoringActivity) | Tento příklad ukazuje, jak používat AzureMLBatchScoringActivity vyvolat Azure počítače výukové model, který provádí analýzu myšlenkou twitter bodování předpovědí atd. |
| [Analýza Twitter pomocí vlastní aktivity](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/TwitterAnalysisSample-CustomC%23Activity) |  Tento příklad ukazuje, jak používat s vlastní aktivitou .NET pro vyvolání Azure počítače výukové model, který provádí analýzu myšlenkou twitter bodování předpovědí atd. |
| [Parametry potrubí výukové Azure počítače](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/ParameterizedPipelinesForAzureML/) | Příklad obsahuje začátku do konce C# kód pro nasazení N potrubí bodování a přeškolení oba objekty mají parametr jiné oblasti, odkud pocházejí seznam oblastí parameters.txt soubor, který je součástí v tomto příkladu. | 
| [Přehled aktualizace dat pro analýzy toku Azure úlohy](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/ReferenceDataRefreshForASAJobs) |  Tento příklad ukazuje, jak používat Azure Data Factory Azure toku analýzy společně ke spouštění dotazů s referenčních dat a nastavení aktualizace dat odkaz na plánu. |
| [Hybridní kanálem k odesílání zpráv s místním Hortonworks Hadoop](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/HybridPipelineWithOnPremisesHortonworksHadoop) | Příklad používá clusteru Hadoop místní jako cíle pro využití pro spuštění úlohy v Data Factory úplně stejně, jako by přidáte jiné výpočetním cíle jako HDInsight založeny Hadoop obrázku v cloudu. |
| [Konverze JSON](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/JSONConversionTool) | Tento nástroj umožňuje převést JSONs z verze před 2015-07-01náhled nejnovější nebo 2015 07 01 náhled (výchozí). |  
| [U SQL ukázková vstupní soubor](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/U-SQL%20Sample%20Input%20File) |  Tento soubor je používán aktivitu U SQL ukázkový soubor. | 

## <a name="azure-resource-manager-templates"></a>Azure správce prostředků šablony
O Data Factory na Github najdete následující šablony správce prostředků Azure. 

| Šablony | Popis |
| -------- | ----------- | 
| [Zkopírujte z úložišti objektů Blob Azure k databázi Azure SQL](https://github.com/Azure/azure-quickstart-templates/tree/master/101-data-factory-blob-to-sql-copy) | Nasazení Tato šablona vytvoří factory Azure dat s kanálu, který slouží ke kopírování dat z zadaný úložišti objektů blob Azure k databázi Azure SQL |    
| [Zkopírujte do úložišti objektů Blob Azure ze služby Salesforce](https://github.com/Azure/azure-quickstart-templates/tree/master/101-data-factory-salesforce-to-blob-copy) | Nasazení Tato šablona vytvoří factory Azure dat s kanálu, který slouží ke kopírování dat z určitého účtu služby Salesforce k základnímu úložišti objektů blob Azure. |    
| [Transformace dat spuštěním podregistru skriptu clusteru Azure HDInsight](https://github.com/Azure/azure-quickstart-templates/tree/master/101-data-factory-hive-transformation) | Nasazení Tato šablona vytvoří factory Azure dat s kanálů, což transformace dat spuštěním ukázkový podregistru skript Azure HDInsight Hadoop clusteru. | 


## <a name="samples-in-azure-portal"></a>Ukázky Azure portálu
Můžete dlaždici **vzorku kanály** na domovské stránce výrobce dat nasazení potrubí ukázkové a jejich přidružených entit (datové sady a propojené služby) v data factory. 

1. Vytvořte factory dat nebo otevřete existující factory data. Najdete v článku [Začínáme s Azure Data Factory](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md#CreateDataFactory) postup vytvoření data factory.
2. Na zásuvné **DATA FACTORY** pro factory dat klikněte na dlaždici **potrubí vzorku** .

    ![Ukázka potrubí dlaždice](./media/data-factory-samples/SamplePipelinesTile.png)

2. V **ukázkové potrubí** zásuvné klikněte **vzorku** , kterou chcete nasadit. 
    
    ![Ukázka potrubí zásuvné](./media/data-factory-samples/SampleTile.png)

3. Zadání nastavení konfigurační vzorku. Například svůj klíč účtu a název účtu Azure úložiště název serveru Azure SQL, databáze, ID uživatele a heslo, atd. 

    ![Ukázka zásuvné](./media/data-factory-samples/SampleBlade.png)

4. Po dokončení práce s konfigurací nastavení, klikněte na **vytvořit** vytvořit/nasadit ukázkové kanály a propojené služby a tabulek použitých kanály.
5. Zobrazení stavu nasazení na dlaždici vzorku, který dříve klepnete na zásuvné **potrubí vzorku** .

    ![Stav nasazení](./media/data-factory-samples/DeploymentStatus.png)

6. Až uvidíte zprávu **úspěšném nasazení** na dlaždici vzorku, zavřete zásuvné **potrubí vzorku** .  
5. Na zásuvné **DATA FACTORY** uvidíte, že propojené služby, sady dat a kanály se přidají do data factory.  

    ![Zásuvné Factory dat](./media/data-factory-samples/DataFactoryBladeAfter.png)
   
## <a name="samples-in-visual-studio"></a>Ukázky ve Visual Studiu

### <a name="prerequisites"></a>Zjistit předpoklady pro

Musí mít na počítači nainstalovaný následující: 

- Visual Studio 2013 nebo Visual Studio 2015
- Stáhněte si Azure SDK Visual Studio 2013 nebo Visual Studio 2015. Přejděte na [Stránku Stáhnout Azure](https://azure.microsoft.com/downloads/) a klikněte na **a 2013** nebo **2015 a** v části **.NET** .
- Stáhněte si nejnovější modul plug-in Azure Data Factory for Visual Studio: [2013 a](https://visualstudiogallery.msdn.microsoft.com/754d998c-8f92-4aa7-835b-e89c8c954aa5) nebo [a 2015](https://visualstudiogallery.msdn.microsoft.com/371a4cf9-0093-40fa-b7dd-be3c74f49005). Pokud používáte Visual Studio 2013, můžete taky aktualizovat modul plug-in provedením následujících kroků: V nabídce klikněte na **Nástroje** -> **rozšíření a aktualizace** -> **Online** -> **Galerie Visual Studio** -> **Microsoft Azure datové Factory nástroje for Visual Studio** -> **Aktualizovat**.

### <a name="use-data-factory-templates"></a>Použití šablon Factory dat

1. V nabídce klikněte na **soubor** , přejděte na **Nový**a klikněte na **projekt**. 
2. V dialogovém okně **Nový projekt** proveďte následující kroky: 
    1. Vyberte **DataFactory** v části **šablony**. 
    2. V pravém podokně vyberte položku **Data Factory šablony** . 
    3. Zadejte **název** projektu. 
    4. Vyberte **umístění** projektu. 
    5. Klikněte na **OK**. 

    ![Dialogové okno Nový projekt](./media/data-factory-samples/vs-new-project-adf-templates.png)
6. V dialogovém okně **Šablony Factory dat** vyberte ukázkové šablony z oddílu **Případu použití šablony** a klikněte na tlačítko **Další**. Podle těchto kroků vás provede jednotlivými pomocí šablony **Vytváření profilu zákazníka** . Kroky jsou podobné u ostatních vzorků. 

    ![Dialogové okno Factory šablony dat](./media/data-factory-samples/vs-data-factory-templates-dialog.png) 
7. V dialogovém okně **Konfigurace Factory dat** klikněte na **Další** na stránce **Základy Factory dat** .
8. Na stránce **Konfigurovat factory dat** proveďte následující kroky: 
    1. Vyberte možnost **vytvořit nový Data Factory**. Můžete také vybrat **použití existující data factory**.
    2. Zadejte **název** pro data factory.
    3. Vyberte **Azure předplatné** , ve kterém chcete factory dat vytvořit. 
    4. Vyberte **pole Skupina zdroje** pro data factory.
    5. Vyberte **Západní USA**, **Východoasijských USA**nebo **Severní Europe** pro **oblast**.
    6. Klikněte na tlačítko **Další**. 
9. Na stránce **Konfigurovat dat ukládá** zadejte stávající **databázi Azure SQL** a **účet Azure úložiště** (nebo) vytvořit databázi/úložiště a klikněte na tlačítko Další. 
10. Na stránce **Konfigurovat výpočet** vyberte výchozí hodnoty a klikněte na tlačítko **Další**. 
11. Na stránce **Souhrn** zkontrolujte všechna nastavení a klikněte na tlačítko **Další**. 
12. Na stránce **Stav nasazení** počkejte na dokončení nasazení a klikněte na tlačítko **Dokončit**.
13. Klikněte pravým tlačítkem na projektu v Průzkumníku řešení a klikněte na **Publikovat**. 
19. Pokud se zobrazí dialogové okno **přihlásit ke svému účtu Microsoft** , zadejte svoje přihlašovací údaje pro účet, který má Azure předplatné a klikněte na **přihlásit**.
20. Měli byste vidět dialogovým oknem takto:

    ![Dialogové okno Publikovat](./media/data-factory-build-your-first-pipeline-using-vs/publish.png)

21. Na stránce **Konfigurovat factory dat** proveďte následující kroky: 
    1. Potvrďte tuto možnost **použít existující data factory** .
    2. Výběr **dat factory** , kterou jste měli výběr při použití šablony. 
    6. Klikněte na tlačítko **Další** přejděte na stránku **Publikování položek** . (Stisknutím klávesy **TAB** přesunout z pole název, pokud je zakázané na tlačítko **Další** .) 
23. Na stránce **Publikování položek** zajistit, aby všechny zdroje dat entity nedošlo k výběru a klikněte na tlačítko **Další** přejděte na stránku **souhrnu** .     
24. Zkontrolujte souhrnné a klikněte na **Další** zahájení procesu nasazení a zobrazit **Stav nasazení**.
25. Na stránce **Stav nasazení** byste měli vidět stav procesu nasazení. Po dokončení nasazení, klikněte na Dokončit. 

Podrobné informace o používání aplikace Visual Studio k vytváření entity Factory dat a jejich publikováním Azure najdete v článku [vytvoření první dat factory (Visual Studio)](data-factory-build-your-first-pipeline-using-vs.md) .          