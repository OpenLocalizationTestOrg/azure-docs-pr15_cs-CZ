<properties
    pageTitle="Zkoumání dat v tabulkách podregistru s dotazy podregistru | Microsoft Azure"
    description="Zkoumání dat v tabulkách podregistru podregistru dotazů."
    services="machine-learning"
    documentationCenter=""
    authors="bradsev"
    manager="jhubbard"
    editor="cgronlun"  />

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/13/2016"
    ms.author="bradsev" />

# <a name="explore-data-in-hive-tables-with-hive-queries"></a>Zkoumání dat v tabulkách podregistru s podregistru dotazů

Tento dokument obsahuje ukázky skriptů podregistru, které se používají k prohlížení dat v tabulkách podregistru clusteru HDInsight Hadoop.

Následující **nabídky** odkazy na témata, která popisují, jak používat nástroje, které můžete prozkoumat data z různých prostředích úložiště.

[AZURE.INCLUDE [cap-explore-data-selector](../../includes/cap-explore-data-selector.md)]

## <a name="prerequisites"></a>Zjistit předpoklady pro
V tomto článku se předpokládá, že máte:

* Vytvoření účtu Azure úložiště. Pokud potřebujete pokyny najdete v článku [Vytvoření účet Azure úložiště](../storage/storage-create-storage-account.md#create-a-storage-account)
* Zřízení přizpůsobený clusteru Hadoop ke službě HDInsight. Pokud potřebujete pokyny, přečtěte si článek [Přizpůsobení Azure HDInsight Hadoop clusterů pokročilé technologie pro analýzu](machine-learning-data-science-customize-hadoop-cluster.md).
* Data je uložená na podregistru tabulky v Azure HDInsight Hadoop clusterů. Pokud ne, postupujte podle pokynů v [Create a načítání dat do tabulek podregistru](machine-learning-data-science-move-hive-tables.md) odeslání dat do tabulky podregistru nejdřív.
* Povolit vzdálený přístup k clusteru. Pokud potřebujete pokyny najdete v článku [přístup vedoucí uzel Hadoop obrázku](machine-learning-data-science-customize-hadoop-cluster.md#headnode).
* Pokud potřebujete informace o odesílání podregistru dotazů, přečtěte si, [jak odeslat podregistru dotazů](machine-learning-data-science-move-hive-tables.md#submit)

## <a name="example-hive-query-scripts-for-data-exploration"></a>Příklad podregistru dotazu skripty pro průzkum

1. Zjištění počtu pozorování na oddíl `SELECT <partitionfieldname>, count(*) from <databasename>.<tablename> group by <partitionfieldname>;`

2. Zjištění počtu pozorování denně `SELECT to_date(<date_columnname>), count(*) from <databasename>.<tablename> group by to_date(<date_columnname>);`

3. Získání úrovně do kategorií sloupce  
    `SELECT  distinct <column_name> from <databasename>.<tablename>`

4. Počet úrovní vstoupit spojení dvou sloupců kategorií `SELECT <column_a>, <column_b>, count(*) from <databasename>.<tablename> group by <column_a>, <column_b>`

5. Získání rozdělení pro číselné sloupce  
    `SELECT <column_name>, count(*) from <databasename>.<tablename> group by <column_name>`

6. Extrahování záznamy z spojení dvou tabulek

        SELECT
            a.<common_columnname1> as <new_name1>,
            a.<common_columnname2> as <new_name2>,
            a.<a_column_name1> as <new_name3>,
            a.<a_column_name2> as <new_name4>,
            b.<b_column_name1> as <new_name5>,
            b.<b_column_name2> as <new_name6>
        FROM
            (
            SELECT <common_columnname1>,
                <common_columnname2>,
                <a_column_name1>,
                <a_column_name2>,
            FROM <databasename>.<tablename1>
            ) a
            join
            (
            SELECT <common_columnname1>,
                <common_columnname2>,
                <b_column_name1>,
                <b_column_name2>,
            FROM <databasename>.<tablename2>
            ) b
            ON a.<common_columnname1>=b.<common_columnname1> and a.<common_columnname2>=b.<common_columnname2>

## <a name="additional-query-scripts-for-taxi-trip-data-scenarios"></a>Další výrazy skripty taxi ze služební cesty dat scénáře

Příklady dotazy, které jsou specifické pro scénáře [NYC Taxi ze služební cesty dat](http://chriswhong.com/open-data/foil_nyc_taxi/) jsou také podle [Github úložiště](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/DataScienceProcess/DataScienceScripts). Tyto dotazy už máte schéma dat zadali a je připraven k spuštění.
