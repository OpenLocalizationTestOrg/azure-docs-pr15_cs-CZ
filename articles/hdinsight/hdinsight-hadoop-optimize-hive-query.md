<properties
   pageTitle="Optimalizace dotazech podregistru pro rychlejší spuštění HDInsight | Microsoft Azure"
   description="Zjistěte, jak optimalizovat dotazech podregistru pro Hadoop v HDInsight."
   services="hdinsight"
   documentationCenter=""
   authors="rashimg"
   manager="mwinkle"
   editor="cgronlun"
   tags="azure-portal"/>

<tags
   ms.service="hdinsight"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="07/28/2015"
   ms.author="rashimg"/>


# <a name="optimize-hive-queries-for-hadoop-in-hdinsight"></a>Optimalizace podregistru dotazů pro Hadoop v HDInsight

Ve výchozím nastavení nejsou optimalizované Hadoop clusterů výkon. Tento článek popisuje pár nejběžnějších podregistru výkonu optimalizace metody, které můžete použít naše dotazů.

##<a name="scale-out-worker-nodes"></a>Rozšiřování pracovníka uzlů

Zvýšení počtu uzlů pracovníka v clusteru můžete využít další mappers a přechodky proběhnout souběžně. Zvětšit měřítko, v HDInsight dvěma způsoby:

- Při poskytování můžete určit počet pracovníka uzlů pomocí portálu Azure, Azure PowerShell nebo rozhraní různé platformy příkazového řádku.  Další informace najdete v tématu [clusterů poskytování HDInsight](hdinsight-provision-clusters.md). Na následující obrazovce zobrazovat pracovníka uzel konfigurace na portálu Azure:

    ![scaleout_1][image-hdi-optimize-hive-scaleout_1]

- Za běhu můžete taky rozšiřování clusteru bez znovu vytvářeli jeden. Zobrazí se pod.
![scaleout_1][image-hdi-optimize-hive-scaleout_2]

Podrobné informace o různých virtuálních počítačích nepodporuje Hdinsightu najdete v článku [ceny HDInsight](https://azure.microsoft.com/pricing/details/hdinsight/).

##<a name="enable-tez"></a>Povolení Tez

[Apache Tez](http://hortonworks.com/hadoop/tez/) je modul alternativní spuštění pro modul MapReduce:

![tez_1][image-hdi-optimize-hive-tez_1]


Tez totiž rychleji:

- Spuštění řízené Acyklické grafu (DAG) jako jednu úlohu v modulu MapReduce, DAG vyjádřený vyžaduje každou sadu mappers následovat jednu sadu přechodky. To způsobí, že více úloh MapReduce nespředený pro každý podregistru dotaz. Tez nemá těchto omezení a zpracování složitých DAG jako jednu úlohu tedy minimalizace režijních spuštění úlohy.
- **Data zapisuje Avoids nepotřebné** Vzhledem k více úloh nespředený pro stejný dotaz podregistru v modulu MapReduce výstup každého projektu je došlo k zápisu HDFS intermediate data. Protože Tez minimalizuje několik úloh pro každý podregistru dotaz je moct vyhnout zbytečnými zápisu.
- **Zpoždění spuštění Minimizes** Tez je lepší Minimalizovat zpoždění spuštění omezit tak počet mappers nutnou k zahájení a také vylepšení optimalizace v celém.
- **Opakované používání kontejnery** Pokud je možné Tez možnost opakovaného použití kontejnery zajistit sníží latence kvůli spuštění kontejnery.
- **Nepřetržitý optimalizační postupy** Obvykle optimalizace platné fázi kompilace. Však je k dispozici další informace o vstupní hodnoty, které umožňují lepší optimalizaci za běhu. Tez pomocí nepřetržité optimalizace, které umožňuje Optimalizace plánu dál do fáze runtime.

Podrobné informace o těchto koncepty, klikněte [sem](http://hortonworks.com/hadoop/tez/)

Můžete provést libovolný podregistru dotaz Tez povolit tak, že obsahující předponu dotaz s nastavením níže:

    set hive.execution.engine=tez;

U serveru s Windows HDInsight clusterů musí být povolený Tez při poskytování. Následující obrázek je ukázkový skript Azure PowerShell pro zřizování Hadoop obrázku s Tez povolené:

[AZURE.INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]

    $clusterName = "[HDInsightClusterName]"
    $location = "[AzureDataCenter]" #i.e. West US
    $dataNodes = 32 # number of worker nodes in the cluster

    $defaultStorageAccountName = "[DefaultStorageAccountName]"
    $defaultStorageContainerName = "[DefaultBlobContainerName]"
    $defaultStorageAccountKey = $defaultStorageAccountKey = Get-AzureStorageKey $defaultStorageAccountName.ToLower() | %{ $_.Primary }

    $hdiUserName = "[HTTPUserName]"
    $hdiPassword = "[HTTPUserPassword]"

    $hdiSecurePassword = ConvertTo-SecureString $hdiPassword -AsPlainText -Force
    $hdiCredential = New-Object System.Management.Automation.PSCredential($hdiUserName, $hdiSecurePassword)

    $hiveConfig = new-object 'Microsoft.WindowsAzure.Management.HDInsight.Cmdlet.DataObjects.AzureHDInsightHiveConfiguration'
    $hiveConfig.Configuration = @{ "hive.execution.engine"="tez" }

    New-AzureHDInsightClusterConfig -ClusterSizeInNodes $dataNodes -HeadNodeVMSize Standard_D14 -DataNodeVMSize Standard_D14 |
    Set-AzureHDInsightDefaultStorage -StorageAccountName "$defaultStorageAccountName.blob.core.windows.net" -StorageAccountKey $defaultStorageAccountKey -StorageContainerName $defaultStorageContainerName |
    Add-AzureHDInsightConfigValues -Hive $hiveConfig |
    New-AzureHDInsightCluster -Name $clusterName -Location $location -Credential $hdiCredential

    
> [AZURE.NOTE] Na základě Linux clusterů HDInsight mít Tez standardně zapnutá.
    

## <a name="hive-partitioning"></a>Podregistru rozdělení

Operace vstupu a výstupu není kritická hlavní výkon při spouštění dotazů podregistru. Výkon můžete zlepšit, pokud lze snížit množství dat, kterou je potřeba otevřít. Ve výchozím nastavení podregistru celými tabulkami podregistru dotazů naskenovaný obrázek. Toto je ideální pro dotazy jako prohledávají tabulku, ale dotazy, které stačí na malou část dat (například dotazů k filtrování), vytvoří se nepotřebné režijních. Rozdělení podregistru umožňuje vytvářet dotazy podregistru přístup jenom potřebné množství dat v tabulkách podregistru.

Rozdělení podregistru prováděna uspořádáním původní data do nové adresáře se každý oddíl s vlastním adresář – kde oddílu definované uživatelem. Následující obrázek znázorňuje rozdělení tabulku podregistru podle sloupce *Year*. Pro každý rok se vytvoří nový adresář.

![rozdělení][image-hdi-optimize-hive-partitioning_1]

Několik tipů rozdělení:

- **Proveďte není snížení oddíl** - oddílů na sloupce s jenom několik hodnotami mohou způsobit málo oddíly. Například oddílů na gender bude pouze vytvořit dva oddíly vytvořit (muže a ženy), tedy pouze zmenšit latence maximálně polovinu.

- **Není povolená oddílů** – další krajní vytváření oddílů na sloupce s jedinečnou hodnotu (například ID uživatele) způsobí více oddílů, které jsou příčinou spoustu namáhání na namenode obrázku se mají zpracovat velké množství adresáře.

- **Nepoužívejte dat zkosení** - promyšlený výběr rozdělení klíč tak, aby všechny oddíly i velikost. Příklad rozdělení na *Stav* tuto chybu může způsobovat počtu zobrazených záznamů v části Kalifornie být téměř 30 argumenty x, Vermont kvůli rozdíl v základního souboru.

Pokud chcete vytvořit tabulku oddílu, použijte klauzule *Oddíly tak, že* :

    CREATE TABLE lineitem_part
        (L_ORDERKEY INT, L_PARTKEY INT, L_SUPPKEY INT,L_LINENUMBER INT,
         L_QUANTITY DOUBLE, L_EXTENDEDPRICE DOUBLE, L_DISCOUNT DOUBLE,
         L_TAX DOUBLE, L_RETURNFLAG STRING, L_LINESTATUS STRING,
         L_SHIPDATE_PS STRING, L_COMMITDATE STRING, L_RECEIPTDATE        STRING, L_SHIPINSTRUCT STRING, L_SHIPMODE STRING,
         L_COMMENT STRING)
    PARTITIONED BY(L_SHIPDATE STRING)
    ROW FORMAT DELIMITED FIELDS TERMINATED BY '\t'
    STORED AS TEXTFILE;

Po vytvoření oddílů tabulky můžete vytvořit statické oddílů nebo dynamické rozdělení.

- **Statická rozdělení** znamená, že máte už sharded data v příslušné adresáře a požádáte podregistru oddíly ručně podle umístění adresáře. To je ukázáno v následujícím fragment kódu.

        INSERT OVERWRITE TABLE lineitem_part
        PARTITION (L_SHIPDATE = ‘5/23/1996 12:00:00 AM’)
        SELECT * FROM lineitem 
        WHERE lineitem.L_SHIPDATE = ‘5/23/1996 12:00:00 AM’

        ALTER TABLE lineitem_part ADD PARTITION (L_SHIPDATE = ‘5/23/1996 12:00:00 AM’))
        LOCATION ‘wasbs://sampledata@ignitedemo.blob.core.windows.net/partitions/5_23_1996/'

- **Dynamické rozdělení** znamená, že chcete, aby ho podregistru za vás automaticky vytvoří oddíly. Protože jsme jste již vytvořili rozdělení tabulky z pracovní tabulky, potřebujeme dělat stačí vložit dat k tabulce oddíly, jak je ukázáno v následujícím příkladu:

        SET hive.exec.dynamic.partition = true;
        SET hive.exec.dynamic.partition.mode = nonstrict;
        INSERT INTO TABLE lineitem_part
        PARTITION (L_SHIPDATE)
        SELECT L_ORDERKEY as L_ORDERKEY, L_PARTKEY as L_PARTKEY , 
             L_SUPPKEY as L_SUPPKEY, L_LINENUMBER as L_LINENUMBER,
             L_QUANTITY as L_QUANTITY, L_EXTENDEDPRICE as L_EXTENDEDPRICE,
             L_DISCOUNT as L_DISCOUNT, L_TAX as L_TAX, L_RETURNFLAG as       L_RETURNFLAG, L_LINESTATUS as L_LINESTATUS, L_SHIPDATE as       L_SHIPDATE_PS, L_COMMITDATE as L_COMMITDATE, L_RECEIPTDATE as   L_RECEIPTDATE, L_SHIPINSTRUCT as L_SHIPINSTRUCT, L_SHIPMODE as      L_SHIPMODE, L_COMMENT as L_COMMENT, L_SHIPDATE as L_SHIPDATE FROM lineitem;

Další podrobnosti najdete v tématu [Oddíly tabulek](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+DDL#LanguageManualDDL-PartitionedTables).

##<a name="use-the-orcfile-format"></a>Použití formátu ORCFile

Podregistru podporuje jiných formátech souboru. Příklad:

- **Text**: Toto je výchozí formát souborů která funguje s většině případů
- **Avro**: vhodný pro scénáře spolupráce
- **ORC/Parquet**: nejvhodnější pro výkonu

Formát ORC (optimalizované řádku sloupcovém) je vysoce efektivní způsob, jak uložit podregistru data. Porovnání do jiných formátů, ORC má následující výhody:

- Podpora pro komplexní typy včetně data a času a typy složité a částečně strukturovaná
- až komprese 70 %
- indexy každé 10 000 řádky, které umožňují řádků
- významný pokles spuštění běhu

Povolit ORC formát, nejdřív vytvořit tabulku s klauzulí *uložené jako ORC*:

    CREATE TABLE lineitem_orc_part
        (L_ORDERKEY INT, L_PARTKEY INT,L_SUPPKEY INT, L_LINENUMBER INT,
         L_QUANTITY DOUBLE, L_EXTENDEDPRICE DOUBLE, L_DISCOUNT DOUBLE,
         L_TAX DOUBLE, L_RETURNFLAG STRING, L_LINESTATUS STRING,
         L_SHIPDATE_PS STRING, L_COMMITDATE STRING, L_RECEIPTDATE STRING,
         L_SHIPINSTRUCT STRING, L_SHIPMODE STRING, L_COMMENT     STRING)
    PARTITIONED BY(L_SHIPDATE STRING)
    STORED AS ORC;

Pak vložíte data do tabulky ORC z pracovní tabulky. Příklad:

    INSERT INTO TABLE lineitem_orc
    SELECT L_ORDERKEY as L_ORDERKEY, 
           L_PARTKEY as L_PARTKEY , 
           L_SUPPKEY as L_SUPPKEY,
           L_LINENUMBER as L_LINENUMBER,
           L_QUANTITY as L_QUANTITY, 
           L_EXTENDEDPRICE as L_EXTENDEDPRICE,
           L_DISCOUNT as L_DISCOUNT,
           L_TAX as L_TAX,
           L_RETURNFLAG as L_RETURNFLAG,
           L_LINESTATUS as L_LINESTATUS,
           L_SHIPDATE as L_SHIPDATE,
           L_COMMITDATE as L_COMMITDATE,
           L_RECEIPTDATE as L_RECEIPTDATE, 
           L_SHIPINSTRUCT as L_SHIPINSTRUCT,
           L_SHIPMODE as L_SHIPMODE,
           L_COMMENT as L_COMMENT
    FROM lineitem;

Si můžete přečíst informace o formátu ORC [tady](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+ORC).

##<a name="vectorization"></a>Vectorization

Vectorization umožňuje podregistru zpracuje dávku řádky 1 024 společně místo zpracování jeden řádek po druhém. To znamená, že jednoduché operace dělají rychleji protože méně vnitřní kód je potřeba spustit.

Chcete-li povolit vectorization připojit znak před podregistru dotaz použité toto nastavení:

    set hive.vectorized.execution.enabled = true;

Další informace najdete v tématu [spuštění dotazu Vectorized](https://cwiki.apache.org/confluence/display/Hive/Vectorized+Query+Execution).


##<a name="other-optimization-methods"></a>Další metody optimalizace

Způsoby další optimalizace, které můžete se rozhodnout, například:

- **Podregistru bucketing:** postup umožňující clusteru nebo segmentech velkých sad dat pro optimalizaci výkonu dotazu.
- **Připojení optimalizace:** optimalizace spuštění dotazu na podregistru plánování ke zvýšení efektivity spojení a snížit potřebu uživatelské pokyny. Další informace najdete v tématu [připojení optimalizace](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+JoinOptimization#LanguageManualJoinOptimization-JoinOptimization).
- **zvětšení přechodky**

##<a id="nextsteps"></a>Další kroky
V tomto článku se jste se naučili několika běžných postupů optimalizace dotazu podregistru. Další informace naleznete v následujících článcích:

- [Použití Apache podregistru v HDInsight](hdinsight-use-hive.md)
- [Analýza dat zpoždění letů pomocí podregistru v HDInsight](hdinsight-analyze-flight-delay-data.md)
- [Analýza dat Twitteru pomocí podregistru v HDInsight](hdinsight-analyze-twitter-data.md)
- [Analýza senzor dat pomocí dotazu konzoly podregistru na Hadoop v HDInsight](hdinsight-hive-analyze-sensor-data.md)
- [Pomocí podregistru HDInsight analyzovat protokoly z webové stránky](hdinsight-hive-analyze-website-log.md)


[image-hdi-optimize-hive-scaleout_1]: ./media/hdinsight-hadoop-optimize-hive-query/scaleout_1.png
[image-hdi-optimize-hive-scaleout_2]: ./media/hdinsight-hadoop-optimize-hive-query/scaleout_2.png
[image-hdi-optimize-hive-tez_1]: ./media/hdinsight-hadoop-optimize-hive-query/tez_1.png
[image-hdi-optimize-hive-partitioning_1]: ./media/hdinsight-hadoop-optimize-hive-query/partitioning_1.png
