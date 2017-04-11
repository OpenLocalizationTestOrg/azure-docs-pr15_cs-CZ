<properties 
    pageTitle="Transformace: Obrázku a transformace dat | Microsoft Azure" 
    description="Zjistěte, jak k transformaci dat nebo dat obrázku v Azure Data Factory pomocí Hadoop, výukové počítače nebo jezera analýzy dat Azure." 
    keywords="transformace, proces data transformace dat, transformace aktivity"
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
    ms.date="09/23/2016" 
    ms.author="shlo"/>

# <a name="transform-data-in-azure-data-factory"></a>Transformace dat v Azure Data Factory
> [AZURE.SELECTOR]
[Podregistru](data-factory-hive-activity.md)  
[Prasátko](data-factory-pig-activity.md)  
[MapReduce](data-factory-map-reduce.md)  
[Přenos Hadoop](data-factory-hadoop-streaming-activity.md)
[Počítače výukové](data-factory-azure-ml-batch-execution-activity.md) 
[Uložená procedura](data-factory-stored-proc-activity.md)
[Analýzy jezera dat U-SQL](data-factory-usql-activity.md)
[.NET vlastní](data-factory-use-custom-activities.md)
   

## <a name="overview"></a>Základní informace 
Tento článek vysvětluje aktivity transformace dat v Azure Data Factory, můžete použít k transformaci a procesu nezpracovaná data do předpovědí a přehledech. Transformace aktivity spustí v prostředí výpočetního clusteru Azure HDInsight ATP dávku Azure. Podrobné informace o jednotlivých aktivit transformace poskytuje odkazy na články.
 
Data Factory podporuje následující data transformace činnosti, které můžete přidat [potrubí](data-factory-create-pipelines.md) buď jednotlivě nebo zřetězené jiné aktivitě.

> [AZURE.NOTE] Návod podrobné pokyny najdete v článku [Vytvoření kanálu s podregistru transformace](data-factory-build-your-first-pipeline.md) článek.  

## <a name="hdinsight-hive-activity"></a>HDInsight podregistru aktivity
HDInsight podregistru aktivity v kanálu Data Factory provede podregistru dotazy na vlastní nebo na vyžádání/Linux serveru s Windows HDInsight obrázku. Viz článek [Podregistru aktivity](data-factory-hive-activity.md) podrobnosti o tuto aktivitu. 

## <a name="hdinsight-pig-activity"></a>Prasátko HDInsight aktivity
Prasátko HDInsight aktivity v kanálu Data Factory provede Prasátko dotazů na vlastní nebo na vyžádání/Linux serveru s Windows HDInsight obrázku. Článek [Prasátko aktivity](data-factory-pig-activity.md) najdete v článku podrobné informace o této aktivity. 

## <a name="hdinsight-mapreduce-activity"></a>HDInsight MapReduce aktivity
HDInsight MapReduce aktivity v kanálu Data Factory provede MapReduce aplikace na vlastní nebo na vyžádání/Linux serveru s Windows HDInsight obrázku. Viz článek [MapReduce aktivity](data-factory-map-reduce.md) podrobnosti o tuto aktivitu.

Aktivity MapReduce umožňuje Spark programy spusťte svůj cluster HDInsight Spark. Další informace najdete v článku [vyvolat Spark aplikací z Azure Data Factory](data-factory-spark.md) .

## <a name="hdinsight-streaming-activity"></a>HDInsight přenos aktivity
HDInsight datových proudů aktivity v kanálu Data Factory provede Hadoop streamování aplikace na vlastní nebo na vyžádání/Linux serveru s Windows HDInsight obrázku. Sledovat [aktivitu HDInsight Streaming](data-factory-hadoop-streaming-activity.md) podrobné informace o této aktivity.

## <a name="machine-learning-activities"></a>Aktivity výukové počítače
Azure Data Factory umožňuje snadno vytvářet kanály, které používají publikované webové služby Azure počítače výukové pro prediktivní analýzy. Použití [Aktivity spuštění dávky](data-factory-azure-ml-batch-execution-activity.md#invoking-a-web-service-using-batch-execution-activity) v Azure Data Factory kanálu, může vyvolat webové služby strojového výukové zvětšit předpovědí na data v listu.

V průběhu času prediktivní modely ve počítače výuky bodování pokusy muset být retrained pomocí nového vstupní datové sady. Po dokončení s přeškolení chcete aktualizovat hodnocení webové služby s modelem retrained výukové počítače. [Aktualizovat aktivity zdroje](data-factory-azure-ml-batch-execution-activity.md#updating-models-using-update-resource-activity) slouží k aktualizaci webové služby pomocí nově školení modelu.  

Podrobné informace o těchto činností výukové počítače najdete v článku [použití počítače výukové aktivity](data-factory-azure-ml-batch-execution-activity.md) . 

## <a name="stored-procedure-activity"></a>Uložená procedura aktivity
Umožňuje SQL Server uložená procedura aktivitu v kanálu Data Factory vyvolat proceduru uloženou v některém z následujících úložištích dat: Azure databáze SQL Azure SQL datový sklad databáze systému SQL Server ve vašem podniku nebo OM Azure. Najdete v článku [Uložené procedury aktivitu](data-factory-stored-proc-activity.md) podrobnosti.  

## <a name="data-lake-analytics-u-sql-activity"></a>Aktivity jezera analýzy U-SQL dat
Aktivity U SQL technologie pro analýzu dat jezera spustí skript U SQL Azure dat jezera analýzy clusteru. Viz článek [Aktivitu U SQL technologie pro analýzu dat](data-factory-usql-activity.md) podrobnosti. 

## <a name="net-custom-activity"></a>Vlastní aktivitou .NET
Pokud potřebujete transformace dat způsobem, který neposkytuje podporu Data Factory, můžete vytvořit vlastní aktivity s logiky zpracování dat a používat aktivitu v kanálu. Můžete nakonfigurovat vlastní aktivitou .NET spuštění pomocí služby Azure dávku nebo clusteru služby Azure HDInsight. Článek [Použití vlastní aktivity](data-factory-use-custom-activities.md) najdete v článku podrobnosti. 

Můžete vytvořit vlastní aktivitou pomocí skriptů R HDInsight clusteru R nainstalovaný. V části [Spustit skript R použití Azure Data Factory](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/RunRScriptUsingADFSample). 

## <a name="compute-environments"></a>Výpočet prostředí
Vytvořit propojený službu pro výpočetním prostředí a potom službu propojené při definování aktivitu transformace. Existují dva typy výpočetních prostředí nepodporuje Data Factory. 

1. **Na vyžádání**: V tomto případě prostředí plně spravuje Data Factory. Automaticky vytváří se službou Data Factory před úlohy odeslané do zpracování dat a po dokončení úlohy odebrány. Můžete nastavit a podrobného nastavení na vyžádání výpočetním prostředí pro spuštění úlohy správy obrázku a zavádění akce. 
2. **Přenést svůj vlastní**: V tomto případě registrovat vlastní výpočetního prostředí (třeba HDInsight clusteru) jako propojené službu v Data Factory. Prostředí spravuje se a službě Data Factory používá k provedení činnosti. 

Článek [Výpočet propojené služeb](data-factory-compute-linked-services.md) najdete v tématu Další informace o výpočtu služby podporované Data Factory. 


## <a name="summary"></a>Souhrn
Azure Data Factory podporuje následující činnosti transformace dat a výpočetním prostředí pro činnosti. Aktivity transformace můžete přidat potrubí buď jednotlivě nebo zřetězené jiné aktivitě.

Aktivity transformace dat |  Výpočet prostředí 
:----------------------- | :--------------------
[Podregistru](data-factory-hive-activity.md) | HDInsight [Hadoop] 
[Prasátko](data-factory-pig-activity.md) | HDInsight [Hadoop]  
[MapReduce](data-factory-map-reduce.md) | HDInsight [Hadoop]  
[Hadoop datových proudů](data-factory-hadoop-streaming-activity.md) | HDInsight [Hadoop]
[Počítače výukové aktivity: spuštění dávky a aktualizace zdroje](data-factory-azure-ml-batch-execution-activity.md) | Azure OM 
[Uložená procedura](data-factory-stored-proc-activity.md) | Azure SQL, Azure SQL datový sklad nebo SQL Server |
[Jezera analýzy dat U jazyka SQL](data-factory-usql-activity.md) | Azure dat jezera analýzy 
[DotNet](data-factory-use-custom-activities.md) | HDInsight [Hadoop] nebo Azure dávku
   

