Azure Data Factory podporuje následující transformace činnosti, které můžete přidat potrubí buď jednotlivě nebo zřetězené jiné aktivitě.

Aktivity transformace dat |  Výpočet prostředí 
:----------------------- | :--------------------
[Podregistru](../articles/data-factory/data-factory-hive-activity.md) | HDInsight [Hadoop] 
[Prasátko](../articles/data-factory/data-factory-pig-activity.md) | HDInsight [Hadoop]  
[MapReduce](../articles/data-factory/data-factory-map-reduce.md) | HDInsight [Hadoop]  
[Hadoop datových proudů](../articles/data-factory/data-factory-hadoop-streaming-activity.md) | HDInsight [Hadoop]
[Počítače výukové aktivity: spuštění dávky a aktualizace zdroje](../articles/data-factory/data-factory-azure-ml-batch-execution-activity.md) | Azure OM 
[Uložená procedura](../articles/data-factory/data-factory-stored-proc-activity.md) | Azure SQL, Azure SQL datový sklad nebo SQL Server |
[Jezera technologie pro analýzu dat U jazyka SQL](../articles/data-factory/data-factory-usql-activity.md) | Technologie pro analýzu jezera Azure dat 
[DotNet](../articles/data-factory/data-factory-use-custom-activities.md) | HDInsight [Hadoop] nebo Azure dávku
   
> [AZURE.NOTE] 
> Aktivity MapReduce umožňuje Spark programy spusťte cluster HDInsight Spark. Další informace najdete v článku [vyvolat Spark aplikací z Azure Data Factory](../articles/data-factory/data-factory-spark.md) .
> Můžete vytvořit vlastní aktivitou pomocí skriptů R HDInsight clusteru R nainstalovaný. V části [Spustit skript R použití Azure Data Factory](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/RunRScriptUsingADFSample).