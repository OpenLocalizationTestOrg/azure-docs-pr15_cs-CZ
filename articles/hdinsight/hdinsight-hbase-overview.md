<properties
    pageTitle="Co je HBase v HDInsight? | Microsoft Azure"
    description="Úvod k Apache HBase v HDInsight databázi NoSQL vycházejí Hadoop. Další informace o případy použití a porovnejte HBase jiných Hadoop clusterů."
    keywords="bigtable, nosql, co je hbase"
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
    ms.topic="get-started-article"
    ms.date="09/14/2016"
    ms.author="jgao"/>



# <a name="what-is-hbase-in-hdinsight-a-nosql-database-that-provides-bigtable-like-capabilities-for-hadoop"></a>Co je HBase v HDInsight: A NoSQL databázi, která poskytuje možnosti BigTable profesionálové pro Hadoop

Apache HBase je databázi NoSQL otevřít – zdroj, který je založený na Hadoop a modelovat po Google BigTable. HBase poskytuje RAM a silných konzistence pro velké objemy dat nestrukturovaná a semistructured v databázi schemaless uspořádaný podle skupiny sloupců.

Data se ukládají v řádcích tabulky a data v řádku jsou seskupena podle sloupce řady. HBase je schemaless databáze znamená, že je potřeba je to možné definovat před jejich použitím sloupce ani typ dat uložených v nich. Otevřete zdrojový kód měřítko lineárně zpracovávání petabytes údajů o tisíce uzlů. Můžete spolehnout na redundance dat, dávkové zpracování a dalších funkcí, které jsou k dispozici v ekosystému Hadoop distributed aplikacemi.

## <a name="how-is-hbase-implemented-in-azure-hdinsight"></a>Implementaci HBase v Azure HDInsight

HDInsight HBase nabízeny jako spravovanou obrázku, který je součástí Azure prostředí. Clusterů nakonfigurovaná pro uložení dat přímo v úložišti objektů Blob Azure, který obsahuje zhoršeným latence a lepší pružnost v možnostmi výkonu a náklady. Díky zákazníci k vytvoření interaktivních webů, které fungují s velkých sad dat můžete vytvořit služby, které obsahují senzor a telemetrie data z miliony vrcholy a analyzovat tato data pomocí Hadoop úlohy. Je dobré HBase a Hadoop počáteční body velký dat projektu v Azure; zejména můžete povolit aplikace pro práci s velkých sad dat v reálném čase.

Implementace HDInsight využívá škálování architektura HBase poskytnout automatické sharding tabulkami, silných konzistence pro čtení a zápis a automatické převzetí. V paměti ukládání do mezipaměti pro čtení a vysoký výkon streamování u zápisů zvyšují výkon. Zřízení virtuální sítě je také k dispozici pro HDInsight HBase. Další informace najdete v tématu [poskytování HDInsight clusterů Azure virtuální síti se systémem] [hbase-provision-vnet].

## <a name="how-is-data-managed-in-hdinsight-hbase"></a>Správě dat v HDInsight HBase

Data můžete spravovat v HBase pomocí `create`, `get`, `put`, a `scan` příkazy z prostředí HBase. Zápisu dat do databáze pomocí `put` a číst pomocí `get`. `scan` Příkaz se používá k získání dat z více řádků v tabulce. Data můžete taky spravován pomocí HBase C# rozhraní API, který obsahuje knihovnu klienta nad rozhraní REST API HBase. Databáze aplikace HBase můžete taky dotazovaném pomocí podregistru. Úvod do těchto programovací modelů, najdete v článku [Začínáme s používáním HBase s Hadoop v HDInsight][hbase-get-started]. Spoluvytváření procesorů jsou k dispozici, které umožní zpracování dat v uzlech, které hostují databáze.


## <a name="scenarios-use-cases-for-hbase"></a>Scénáře: Použití balení HBase
Vytvoření případu použití kanonický, pro které BigTable (a tím HBase) byla hledání na webu. Vyhledávací weby vytvořit indexy, které mapování podmínky na webové stránky, které je obsahují. Ale existují mnoho dalších případy použití, které je vhodné pro HBase — některé z nich jsou rozpis v této části.

- Klíč hodnota store

    HBase mohou sloužit jako úložiště klíč hodnota a je vhodné pro správu systémy zprávy. Facebook používá HBase pro zasílání zpráv systému a je ideální pro ukládání a Správa internetové komunikace. WebTable používá HBase moct vyhledat a správa tabulek, které jsou extrahované z webové stránky.

- Data snímače

    HBase slouží k zaznamenání dat, která se shromažďují postupně z různých zdrojů. Platí to i pro sociální analýzy, časové řady, uchovávání interaktivní řídicí panely aktuální a mají trendů a čítačů a Správa systémů protokolu auditování. Jako příklad lze uvést Bloomberg účastník Terminálová a otevřené čas řady databáze (OpenTSDB), která ukládá a poskytuje přístup k metriky shromažďované o stavu systému serveru.

- Data v reálném dotazu

    [Phoenix](http://phoenix.apache.org/) je optimalizována dotazu pro Apache HBase. Přistupovat jako ovladač JDBC a umožňuje nemusí dotazy a správa HBase tabulkami pomocí příkazů SQL.

- HBase jako platformy

    Aplikace fungovat nad HBase pomocí jako úložiště. Jako příklad lze uvést Phoenix, OpenTSDB, Kiji a Titan. Aplikace mohou také integrovat s HBase. Jako příklad lze uvést podregistru Prasátko, Solr, bouře, Flume, Impala, Spark, uzlin a přechodu na podrobnosti.


##<a name="next-steps"></a>Další kroky

- [Začínáme s používáním HBase s Hadoop v HDInsight][hbase-get-started]
- [Zřízení clusterů virtuální síti Azure HDInsight] [hbase-provision-vnet]
- [Konfigurace HBase replikace v HDInsight](hdinsight-hbase-geo-replication.md)
- [Analýza Twitter myšlenkou s HBase v HDInsight][hbase-twitter-sentiment]
- [Použít Maven k vytvoření Java aplikace, které používají HBase s HDInsight (Hadoop)][hbase-build-java-maven]

##<a name="see-also"></a>Viz taky

- [Apache HBase](https://hbase.apache.org/)
- [Bigtable: Distribuované úložiště systém strukturovaná Data](http://research.google.com/archive/bigtable.html)




[hbase-provision-vnet]: hdinsight-hbase-provision-vnet.md

[hbase-twitter-sentiment]: hdinsight-hbase-analyze-twitter-sentiment.md

[hbase-build-java-maven]: hdinsight-hbase-build-java-maven.md

[hdinsight-use-hive]: hdinsight-use-hive.md

[hdinsight-storage]: ../hdinsight-hadoop-use-blob-storage.md

[hbase-get-started]: http://azure.microsoft.com/documentation/articles/hdinsight-hbase-get-started/

[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[azure-management-portal]: https://portal.azure.com/
[azure-create-storageaccount]: ../storage-create-storage-account.md

[apache-hadoop]: http://hadoop.apache.org/
