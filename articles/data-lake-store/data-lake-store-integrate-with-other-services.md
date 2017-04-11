<properties
   pageTitle="Integrace úložiště jezera dat s jinými službami Azure | Microsoft Azure"
   description="Vysvětlení, jak úložiště jezera dat je spojené s jinými službami Azure"
   documentationCenter=""
   services="data-lake-store"
   authors="nitinme"
   manager="jhubbard"
   editor="cgronlun"/>

<tags
   ms.service="data-lake-store"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="10/28/2016"
   ms.author="nitinme"/>

# <a name="integrating-data-lake-store-with-other-azure-services"></a>Integrace úložiště jezera dat s jinými službami Azure

Úložiště jezera dat Azure lze spolu s dalšími službami Azure povolit může používat větší okruh scénáře. Následující článek obsahuje seznam služby, které úložiště jezera dat lze integrovat s.

## <a name="use-data-lake-store-with-azure-hdinsight"></a>Použití úložiště jezera s Azure HDInsight

Můžete vytvořit clusteru [Azure Hdinsightu](https://azure.microsoft.com/documentation/learning-paths/hdinsight-self-guided-hadoop-training/) , který používá úložiště jezera dat jako vyhovujících zápisu HDFS úložiště. V této verzi produktu pro Hadoop a bouře clusterů v systému Windows a Linux, můžete úložiště jezera dat jenom jako další úložiště. Tyto clusterů dál používat jako výchozí úložiště Azure úložiště (WASB). HBase clusterů v systému Windows a Linux, ale můžete úložiště jezera dat jako výchozí úložiště další úložiště nebo obojí.

O tom, jak zřídit HDInsight obrázku s úložiště jezera dat najdete v článku:

* [Zřízení HDInsight obrázku s úložištěm jezera dat pomocí portálu Azure](data-lake-store-hdinsight-hadoop-use-portal.md)
* [Zřízení s úložiště jezera dat pomocí prostředí PowerShell Azure HDInsight obrázku](data-lake-store-hdinsight-hadoop-use-powershell.md)

**Chcete raději videa?** Postupujte podle těchto odkazů na videí s pokyny, jak používat úložiště jezera dat s clusterů HDInsight.

* [Vytvoření clusteru HDInsight s přístupem k jezera úložiště dat](https://mix.office.com/watch/l93xri2yhtp2)
* Jakmile clusteru nastavenou, [přístup k datům v úložišti jezera dat pomocí podregistru a Prasátko skriptů](https://mix.office.com/watch/1n9g5w0fiqv1q)


## <a name="use-data-lake-store-with-azure-data-lake-analytics"></a>Použití úložiště jezera dat pomocí analýzy jezera Azure dat

[Jezera analýzy dat Azure](../data-lake-analytics/data-lake-analytics-overview.md) umožňuje pracovat s velký daty ve velkém měřítku cloudu. Dynamicky předpisy zdrojů a umožňuje analýzy na TB nebo dokonce exabajtů data, která mohou být uloženy v části podporované zdroje dat, jedna z nich je úložiště jezera dat počet. Jezera analýzy dat je speciálně optimalizována pro práci s Azure jezera úložišti - poskytující nejvyšší úroveň výkonu, výkonu a paralelního zpracování za vás pracovního vytížení velký data.

Pokyny o tom, jak pomocí analýzy dat jezera úložiště jezera dat najdete v tématu [Začínáme s jezera analýzy dat pomocí jezera úložiště](../data-lake-analytics/data-lake-analytics-get-started-portal.md).

**Chcete raději videa?** Postupujte podle těchto odkazů na videí s pokyny, jak používat úložiště jezera dat s clusterů HDInsight.

* [Připojení dat Azure jezera analýzy úložiště jezera dat Azure](https://mix.office.com/watch/qwji0dc9rx9k)
* [Access Azure jezera úložiště prostřednictvím jezera analýzy dat](https://mix.office.com/watch/1n0s45up381a8)


## <a name="use-data-lake-store-with-azure-data-factory"></a>Použití úložiště jezera s Azure Data Factory

[Azure Data Factory](https://azure.microsoft.com/services/data-factory/) umožňuje jedí dat z Azure tabulky databáze SQL Azure, Azure SQL DataWarehouse, Azure úložiště objektů BLOB a místní databází. S občanstvím prvotřídní v Azure ekosystému, Azure Data Factory mohou sloužit k organizovat požití data z těchto zdroje úložiště jezera dat Azure.

Jak používat Azure Data Factory s úložiště jezera dat najdete v článku [přesunutí dat do a z úložiště jezera dat pomocí Data Factory](../data-factory/data-factory-azure-datalake-connector.md).

**Videa znovu!** V tématu [Průběhu dat pomocí Azure Data Factory pro úložiště jezera dat Azure](https://mix.office.com/watch/1oa7le7t2u4ka). 

## <a name="copy-data-from-azure-storage-blobs-into-data-lake-store"></a>Kopírování dat z Azure úložiště objektů BLOB do úložiště jezera dat

Úložiště jezera dat Azure poskytuje nástroj příkazového řádku, AdlCopy, který umožňuje kopírování dat z úložiště objektů Blob Azure zohledňovala jezera úložiště. Další informace najdete v tématu [kopírování dat z Azure úložiště objektů BLOB dat jezera Store](data-lake-store-copy-data-azure-storage-blob.md).

## <a name="copy-data-between-azure-sql-database-and-data-lake-store"></a>Kopírování dat mezi databáze SQL Azure a jezera úložiště dat

Apache Sqoop umožňuje import a export dat mezi databáze SQL Azure a jezera úložiště. Další informace najdete v tématu [kopírování dat mezi úložiště jezera dat a databáze Azure SQL pomocí Sqoop](data-lake-store-data-transfer-sql-sqoop.md).

**V tomto videu** na [Používání Apache Sqoop přesun dat mezi relačním zdroji a úložiště jezera dat Azure](https://mix.office.com/watch/1butcdjxmu114).

## <a name="use-data-lake-store-with-stream-analytics"></a>Použití úložiště jezera dat pomocí analýzy toku

Úložiště jezera dat jako jednu z výstupy umožňuje ukládat data streamují pomocí analýzy toku Azure. Další informace najdete v tématu [toku dat z Azure úložiště objektů Blob do úložiště jezera dat pomocí analýzy toku Azure](data-lake-store-stream-analytics.md).

## <a name="use-data-lake-store-with-power-bi"></a>Použití úložiště jezera dat s Power BI

Import dat z účtu úložiště jezera dat analýza a vizualizace dat můžou používat Power BI. Další informace najdete v tématu [Analýza dat v úložišti jezera dat pomocí Power BI](data-lake-store-power-bi.md).

## <a name="use-data-lake-store-with-data-catalog"></a>Použití úložiště jezera dat pomocí katalogu dat

Záznam dat z úložiště jezera dat do katalogu dat Azure zpřístupnění data konfigurací v celé organizaci. Další informace najdete v článku [záznam dat z úložiště jezera dat v katalogu dat Azure](data-lake-store-with-data-catalog.md).


## <a name="see-also"></a>Viz taky

- [Základní informace o úložiště jezera dat Azure](data-lake-store-overview.md)
- [Začínáme s úložiště jezera dat pomocí portálu](data-lake-store-get-started-portal.md)
- [Začínáme s úložiště jezera dat pomocí prostředí PowerShell](data-lake-store-get-started-powershell.md)  
