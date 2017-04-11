<properties 
   pageTitle="Scénáře data týkající se úložiště jezera dat | Microsoft Azure" 
   description="Porozumět různých scénářích a nástroje pro data, která můžete požití zpracovat, stažení a vizualizovat v úložišti jezera dat" 
   services="data-lake-store" 
   documentationCenter="" 
   authors="nitinme" 
   manager="jhubbard" 
   editor="cgronlun"/>
 
<tags
   ms.service="data-lake-store"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data" 
   ms.date="09/06/2016"
   ms.author="nitinme"/>

# <a name="using-azure-data-lake-store-for-big-data-requirements"></a>Použití úložiště jezera dat Azure požadavky velký dat

Existují čtyři klíčové fáze velký zpracování dat:

* Ingesting velké objemy dat do úložiště v reálném čase minimálně v listech
* Zpracování dat.
* Stažení dat
* Vizualizace dat

V tomto článku se podíváme na tyto fáze z hlediska Azure jezera úložiště vysvětlení možností a nástrojů pro vlastním potřebám velký data.

## <a name="ingest-data-into-data-lake-store"></a>Jedí dat do úložiště jezera dat

Tato část se zvýrazní různých zdrojů dat a různými způsoby, ve kterém můžete tato data požití zohledňovala jezera úložiště.

![Ingest data do úložiště jezera dat] (./media/data-lake-store-data-scenarios/ingest-data.png "Ingest data do úložiště jezera dat")

### <a name="ad-hoc-data"></a>Ad hoc dat

Představuje menší sadami dat, která se používají pro vytváření prototypů velký dat aplikace. Ingesting ad hoc dat v závislosti na zdroj dat různými způsoby.

| Zdroj dat        | Pomocí jedí                                                                        |
|--------------------|----------------------------------------------------------------------------------------|
| Místního počítače     | <ul> <li>[Portál Azure](/data-lake-store-get-started-portal.md)</li> <li>[Azure Powershellu](data-lake-store-get-started-powershell.md)</li> <li>[Azure různé platformy rozhraní příkazového řádku](data-lake-store-get-started-cli.md)</li> <li>[Pomocí Data jezera Tools for Visual Studio](../data-lake-analytics/data-lake-analytics-data-lake-tools-get-started.md#upload-source-data-files) </li></ul> |
| Azure úložiště objektů Blob | <ul> <li>[Azure Data Factory](../data-factory/data-factory-azure-datalake-connector.md#sample-copy-data-from-azure-blob-to-azure-data-lake-store)</li> <li>[Nástroj AdlCopy](data-lake-store-copy-data-azure-storage-blob.md)</li><li>[DistCp výpočetnímu clusteru HDInsight](data-lake-store-copy-data-wasb-distcp.md)</li> </ul> |

 
### <a name="streamed-data"></a>Datové proudy

Představuje data, která může vytvořit různých zdrojů, jako jsou aplikace zařízení, senzory, atd. Tato data můžete požití do úložiště jezera dat pomocí různých nástrojů. Tyto nástroje obvykle zachycení a zpracování dat na základě události událostí v v reálném čase a zapište události v dávkách do úložiště jezera dat tak, že můžou být zpracovány dál. 

Jsou nástroje, které můžete použít následující:
 
* [Azure toku analýzy] (.. / toku a technologie pro analýzu dat – jezera výstup) - události požití do události rozbočovače můžete být došlo k zápisu dat jezera Azure pomocí výstup úložiště jezera dat Azure.
* [Azure HDInsight bouře](../hdinsight/hdinsight-storm-write-data-lake-store.md) - zápisu dat přímo na úložiště jezera dat z bouře clusteru.
* [EventProcessorHost](../event-hubs/event-hubs-csharp-ephcs-getstarted.md#receive-messages-with-eventprocessorhost) – příjem událostí z rozbočovače události a potom ho k zápisu úložiště jezera dat pomocí [Data jezera úložiště .NET SDK](data-lake-store-get-started-net-sdk.md).

### <a name="relational-data"></a>Relačních dat

Můžete taky zdroje dat z relačních databází. Relační databáze průběhu určitého časového období, shromáždit velké objemy dat, který nabízí klíčové přehledy Pokud zpracovat prostřednictvím velký datový kanál. Následující nástroje slouží k přesunutí tato data do úložiště jezera dat.

* [Apache Sqoop](data-lake-store-data-transfer-sql-sqoop.md)
* [Azure Data Factory](../data-factory/data-factory-data-movement-activities.md)

### <a name="web-server-log-data-upload-using-custom-applications"></a>Web serveru protokolu dat (odeslání pomocí vlastních aplikací)

Tento typ datovou sadu je konkrétně vyznačenými protože analýzy dat protokolu webový server je běžné případu použití pro velké dat aplikace a vyžaduje velkých objemů protokoly nepovolila jezera úložišti. Můžete použít některou z následujících nástrojů pro psaní skriptů nebo aplikací k nahrání tyto údaje.

* [Azure různé platformy rozhraní příkazového řádku](data-lake-store-get-started-cli.md)
* [Azure Powershellu](data-lake-store-get-started-powershell.md)
* [Úložiště jezera dat Azure .NET SDK](data-lake-store-get-started-net-sdk.md)
* [Azure Data Factory](../data-factory/data-factory-data-movement-activities.md)

Pro odeslání dat protokolu webového serveru a taky pro odeslání jiné druhy data (například data, sociální chráněny) je dobrým přístupem zapsat vlastní vlastních skriptů/aplikací, protože nabízí možnost zahrnout data nahrávání součásti jako součást aplikace větší velký data. V některých případech může trvat tento kód formuláři skriptu nebo jednoduché příkazového řádku. V ostatních případech kód lze integrovat velký zpracování dat do aplikace business nebo řešení.


### <a name="data-associated-with-azure-hdinsight-clusters"></a>Data spojená se clusterů Azure HDInsight

Většina HDInsight typů obrázku (Hadoop, HBase, bouře) podporuje úložiště jezera dat jako datový úložiště úložiště. HDInsight clusterů získat přístup k datům z Azure úložiště objektů BLOB (WASB). Pro lepší výkon můžete do úložiště jezera dat účet spojený s clusteru zkopírujte data z WASB. Následující nástroje můžete kopírovat data.

* [Apache DistCp](data-lake-store-copy-data-wasb-distcp.md)
* [AdlCopy služby](data-lake-store-copy-data-azure-storage-blob.md)
* [Azure Data Factory](../data-factory/data-factory-azure-datalake-connector.md#sample-copy-data-from-azure-blob-to-azure-data-lake-store)

### <a name="data-stored-in-on-premise-or-iaas-hadoop-clusters"></a>Data uložená v místním nebo IaaS Hadoop clusterů

Velké objemy dat mohou být uložena v existující Hadoop clusterů, místně na počítačích pomocí HDFS. Clusterů Hadoop může být v místním nasazení nebo může být v rámci IaaS clusteru na Azure. Může to být požadavky zkopírujte tato data do úložiště jezera dat Azure jednorázový přístup nebo opakované způsobem. Existují různé možnosti, které můžete dosáhnout. Tady je seznam alternativy a související střídání.

| Přístup  | Podrobnosti | Výhody   | Co byste měli zvážit  |
|-----------|---------|--------------|-----------------|
| Použití Azure dat Factory (ADF) zkopírujte data přímo z Hadoop clusterů do úložiště jezera dat Azure | [ADF podporuje HDFS jako zdroj dat](../data-factory/data-factory-hdfs-connector.md) | ADF podporuje mimo pole HDFS a první začátku do konce Správa a sledování | Vyžaduje Brána pro správu dat chcete být nasazené na místní nebo IaaS obrázku |
| Exportujte dat z Hadoop jako soubory. Zkopírujte do úložiště jezera dat Azure pomocí příslušných mechanismus soubory.                                   | Soubory můžete zkopírovat do úložiště jezera dat Azure pomocí: <ul><li>[Azure Powershellu pro Windows s operačním systémem](data-lake-store-get-started-powershell.md)</li><li>[Azure různé platformy rozhraní příkazového řádku pro Windows OS](data-lake-store-get-started-cli.md)</li><li>Vlastní aplikace pomocí libovolné Data jezera úložiště SDK</li></ul> | Snadné začít pracovat. Můžete provést vlastní nahrávání                                                   | Vícekrokové proces, který zahrnuje víc technologiích. Správa a sledování rozrůstat být složité časem dané přizpůsobený podstatě nástroji |
| Použití Distcp ke kopírování dat z Hadoop k základnímu úložišti Azure. Zkopírujte data z Azure úložiště úložiště jezera dat pomocí příslušných mechanismus. | Kopírování dat z Azure úložiště do úložiště jezera dat pomocí: <ul><li>[Azure Data Factory](../data-factory/data-factory-data-movement-activities.md)</li><li>[Nástroj AdlCopy](data-lake-store-copy-data-azure-storage-blob.md)</li><li>[Apache DistCp spuštěna HDInsight clusterů](data-lake-store-copy-data-wasb-distcp.md)</li></ul>| Můžete otevřít zdroj nástroje. | Vícekrokové proces, který zahrnuje víc technologie |

### <a name="really-large-datasets"></a>Skutečně velkých sad dat

Pro odeslání datové sady, které oblasti v několika TB, pomocí metody jsme je popsali výše může být někdy pomalé a nákladné. V takovém případě můžete pomocí následujících možností.

* **Použití Azure ExpressRoute**. Azure ExpressRoute umožňuje vytvářet soukromé připojení mezi Azure datacentrech a infrastrukturu ve svém místním prostředí. Spolehlivé možnost pro přenos velké objemy dat nabízí. Další informace najdete v dokumentaci [Azure ExpressRoute](../expressroute/expressroute-introduction.md).


* **"Offline" Odeslat data**. Pokud pomocí Azure ExpressRoute není možné z nějakého důvodu, můžete dodat pevných discích s daty Azure datacentrem [služby Azure Import nebo Export](../storage/storage-import-export-service.md) . Data se nejprve nahrát objektů BLOB Azure úložiště. Pak můžete [Azure Data Factory](../data-factory/data-factory-azure-datalake-connector.md#sample-copy-data-from-azure-blob-to-azure-data-lake-store) nebo [AdlCopy nástroj](data-lake-store-copy-data-azure-storage-blob.md) ke kopírování dat z Azure úložiště objektů BLOB jezera úložiště.

    >[AZURE.NOTE] Při použití služby Import nebo Export velikosti souborů na discích dodávané Azure datacentrem nesmí být větší než 200 GB.


## <a name="process-data-stored-in-data-lake-store"></a>Zpracování dat uložených v úložišti jezera dat

Po data k dispozici v úložišti jezera dat možné spouštět analýzu na tato data pomocí aplikací podporovaného velký. V současné době můžete Azure HDInsight a jezera analýzy dat Azure spuštění úlohy analýzy dat z dat uložených v úložišti jezera. 

![Analýza dat v úložišti jezera dat] (./media/data-lake-store-data-scenarios/analyze-data.png "Analýza dat v úložišti jezera dat")

Můžete si může samostatně prohlížet následující příklady.

* [Vytvoření clusteru HDInsight s úložiště jezera dat jako úložiště](data-lake-store-hdinsight-hadoop-use-portal.md)
* [Použití analýzy jezera Azure dat s jezera úložiště dat](../data-lake-analytics/data-lake-analytics-get-started-portal.md)



## <a name="download-data-from-data-lake-store"></a>Stažení dat z úložiště jezera dat

Můžete taky chtít stáhnout nebo přesuňte data z úložiště jezera dat Azure scénáře, jako:

* Přesunutí dat do jiných úložištích rozhraní s existující kanály zpracování dat. Můžete třeba přesuňte data z úložiště jezera dat do databáze SQL Azure nebo místní SQL Server.
* Stažení dat do místního počítače pro zpracování v prostředí integrovaném vývojovém prostředí při vytváření prototypů aplikace.

![Výstupní data z úložiště jezera dat] (./media/data-lake-store-data-scenarios/egress-data.png "Výstupní data z úložiště jezera dat")

V takovém případě můžete použít některou z následujících možností:

* [Apache Sqoop](data-lake-store-data-transfer-sql-sqoop.md)
* [Azure Data Factory](../data-factory/data-factory-data-movement-activities.md)
* [Apache DistCp](data-lake-store-copy-data-wasb-distcp.md)

Můžete také následujících metod k zápisu vlastní skript/aplikaci pro stažení dat z úložiště jezera dat.

* [Azure různé platformy rozhraní příkazového řádku](data-lake-store-get-started-cli.md)
* [Azure Powershellu](data-lake-store-get-started-powershell.md)
* [Úložiště jezera dat Azure .NET SDK](data-lake-store-get-started-net-sdk.md)

## <a name="visualize-data-in-data-lake-store"></a>Vizualizace dat v úložišti jezera dat

Kombinaci služby umožňuje vytvářet vizuální znázornění dat uložených v úložišti jezera.

![Vizualizace dat v úložišti jezera dat] (./media/data-lake-store-data-scenarios/visualize-data.png "Vizualizace dat v úložišti jezera dat")

* Můžete začít s použitím [Azure Data Factory přesuňte data z úložiště jezera datový sklad SQL Azure](../data-factory/data-factory-data-movement-activities.md#supported-data-stores)
* Až to můžete [integrovat Power BI s Azure SQL datový sklad](../sql-data-warehouse/sql-data-warehouse-integrate-power-bi.md) k vytvoření vizuální znázornění dat.
