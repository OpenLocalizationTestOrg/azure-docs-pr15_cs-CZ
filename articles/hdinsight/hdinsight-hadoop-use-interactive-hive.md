<properties
    pageTitle="Použití interaktivní podregistru v HDInsight | Microsoft Azure"
    description="Naučte se používat interaktivní podregistru (podregistru na LLAP) v HDInsight."
    keywords=""
    services="hdinsight"
    documentationCenter=""
    tags="azure-portal"
    authors="mumian" 
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/27/2016"
    ms.author="jgao"/>


# <a name="use-interactive-hive-in-hdinsight-preview"></a>Použití interaktivní podregistru v HDInsight (verze Preview)

Interaktivní podregistru (také [Dynamický dlouhé a proces]( https://cwiki.apache.org/confluence/display/Hive/LLAP)) je nový HDInsight [Typ obrázku]( hdinsight-hadoop-provision-linux-clusters.md#cluster-types).  Interaktivní podregistru umožňuje ukládání do mezipaměti, který mnohem víc interaktivní a rychlejší díky podregistru dotazů. Nová funkce převede HDInsight jednu Světová organizace většina performant flexibilní, a otevřete velký datové řešení v cloudu pomocí v paměti ukládání do mezipaměti (pomocí podregistru a Spark) a Upřesnit analýzy pomocí rozsáhlá integrace služby R. 

Interaktivní podregistru obrázku se liší od Hadoop obrázku. Obsahuje pouze službu podregistru. 

> [AZURE.NOTE] MapReduce Prasátko, Sqoop, Oozie a jiných služeb, budou odebrány z tohoto typu clusteru brzy bude k dispozici.
Služba podregistru interaktivní podregistru clusteru je pouze přístupných zobrazení Ambari podregistru, Beeline a podregistru ODBC. Není k dispozici prostřednictvím podregistru konzoly Templeton, Azure rozhraní příkazového řádku a Azure Powershellu. 


 


## <a name="create-an-interactive-hive-cluster"></a>Vytvoření interaktivního podregistru obrázku

Interaktivní podregistru clusteru je podporována pouze na základě Linux clusterů. Informace o vytváření HDInsight clusterů najdete v tématu [na základě vytvořit Linux Hadoop clusterů HDInsight](hdinsight-hadoop-provision-linux-clusters.md).


## <a name="execute-hive-from-interactive-hive"></a>Spuštění podregistru z interaktivní podregistru

Existují různé možnosti, jak můžete provést podregistru dotazy:

- Spuštění podregistru pomocí zobrazení Ambari podregistru

    Informace o použití zobrazení podregistru najdete v tématu [Použití zobrazení podregistru s Hadoop v HDInsight]( hdinsight-hadoop-use-hive-ambari-view.md).

- Spuštění podregistru pomocí Beeline

    Informace o použití Beeline na HDInsight najdete v tématu [Použití podregistru s Hadoop v Hdinsightu pomocí Beeline](hdinsight-hadoop-use-hive-beeline.md).

    Používáte Beeline headnode nebo uzel prázdné okraje.  Pomocí Beeline z prázdné postranní uzel se doporučuje.  Informace o vytváření HDInsight obrázku se prázdný edgenode najdete v tématu [použití prázdné okraj uzlů v HDInsight](hdinsight-apps-use-edge-node.md).

- Spuštění podregistru pomocí podregistru ODBC

    Informace o použití podregistru ODBC najdete v tématu [Připojit Excel k Hadoop s ovladač ODBC podregistru Microsoft](hdinsight-connect-excel-hive-odbc-driver.md).

**Jak vyhledat JDBC připojovací řetězec:**

1.  Přihlaste se k Ambari pomocí následující adresu URL: https://<ClusterName>. AzureHDInsight.net.
2.  Klikněte v levé nabídce **podregistru** .
3.  Klikněte na ikonu zvýrazněný a zkopírujte adresu URL:

    ![HDInsight Hadoop interaktivní podregistru LLAP JDBC](./media/hdinsight-hadoop-use-interactive-hive/hdinsight-hadoop-use-interactive-hive-jdbc.png)

## <a name="see-also"></a>Viz taky
-   [Na základě vytvořit Linux Hadoop clusterů HDInsight](hdinsight-hadoop-provision-linux-clusters.md): Naučte se vytvářet interaktivní podregistru clusterů v HDInsight.
-   [Použití podregistru s Hadoop v Hdinsightu pomocí Beeline](hdinsight-hadoop-use-hive-beeline.md): Naučte se používat Beeline můžou odeslat podregistru dotazů.
-   [Připojit Excel k Hadoop s ovladač ODBC podregistru Microsoft](hdinsight-connect-excel-hive-odbc-driver.md): Přečtěte si, jak připojit Excel k podregistru.
