<properties 
   pageTitle="Použití Apache Phoenix a SQuirreL v HDInsight | Microsoft Azure" 
   description="Zjistěte, jak používat Apache Phoenix v HDInsight a jak nainstalovat a nakonfigurovat SQuirreL na počítači se připojit k obrázku HBase v HDInsight." 
   services="hdinsight" 
   documentationCenter="" 
   authors="mumian" 
   manager="jhubbard" 
   editor="cgronlun"/>

<tags
   ms.service="hdinsight"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data" 
   ms.date="09/02/2016"
   ms.author="jgao"/>

# <a name="use-apache-phoenix-with-linux-based-hbase-clusters-in-hdinsight"></a>Použití Apache Phoenix s na základě Linux HBase clusterů HDInsight  

Zjistěte, jak používat [Apache Phoenix](http://phoenix.apache.org/) v HDInsight a jak používat SQLLine. Další informace o Phoenix najdete v článku [Phoenix v 15 minut a méně](http://phoenix.apache.org/Phoenix-in-15-minutes-or-less.html). Gramatiky Phoenix najdete v článku [Phoenix gramatiky](http://phoenix.apache.org/language/index.html).

>[AZURE.NOTE] Informace o verzi Phoenix v HDInsight, najdete v článku [Co je nového v verze obrázku Hadoop poskytovanou HDInsight?] [hdinsight-versions].

##<a name="use-sqlline"></a>Použití SQLLine
[SQLLine](http://sqlline.sourceforge.net/) je nástroj příkazového řádku pro spuštění SQL. 

###<a name="prerequisites"></a>Zjistit předpoklady pro
Než budete moct použít SQLLine, musíte mít takto:

- **A HBase obrázku v HDInsight**. Další informace o poskytování HBase clusteru najdete v článku [Začínáme s Apache HBase v HDInsight][hdinsight-hbase-get-started].
- **Připojit k obrázku HBase prostřednictvím protokolu vzdálené plochy**. Pokyny najdete v tématu [Správa Hadoop clusterů HDInsight pomocí portálu klasické Azure][hdinsight-manage-portal].


Když se připojíte ke HBase clusteru, musíte jedné Zookeepers. Každý cluster HDInsight má 3 Zookeepers. 

**Pokud chcete zjistit, název hostitele Zookeeper**

1. Otevřete Ambari Procházet **https://<ClusterName>. azurehdinsight.net**.
2. Zadání protokolu HTTP (clusteru) uživatelské jméno a heslo k přihlášení.
3. Klikněte v nabídce nalevo na **ZooKeeper** . Zobrazí se 3 **ZooKeeper serveru** uvedené.
4. Klikněte na jednu z **ZooKeeper serveru** uvedené. V podokně Souhrn najděte **název hostitele**. Je podobný *zk1 jdolehb.3lnng4rcvp5uzokyktxs4a5dhd.bx.internal.cloudapp.net*.

**Použití SQLLine**

1. Připojení k clusteru pomocí SSH. Pokyny najdete v tématu [Použití SSH s Hadoop Linux založené na HDInsight z Linux, Unix nebo OS X](hdinsight-hadoop-linux-use-ssh-unix.md) nebo [Použití SSH s Hadoop Linux založené na HDInsight z Windows](hdinsight-hadoop-linux-use-ssh-windows.md) v závislosti na vašem klientském počítači s operačním systémem.

2. Z SSH spusťte následující příkazy SQLLine:

        cd /usr/hdp/2.2.9.1-7/phoenix/bin
        ./sqlline.py <ClusterName>:2181:/hbase-unsecure

2. Následující příkazy k vytvoření HBase tabulky a vložte některá data:

        CREATE TABLE Company (COMPANY_ID INTEGER PRIMARY KEY, NAME VARCHAR(225));
    
        !tables
        
        UPSERT INTO Company VALUES(1, 'Microsoft');
        
        SELECT * FROM Company;
        
        !quit

Další informace najdete v tématu [Ruční SQLLine](http://sqlline.sourceforge.net/#manual) a [Phoenix gramatiku](http://phoenix.apache.org/language/index.html).


 
##<a name="next-steps"></a>Další kroky
V tomto článku se naučili použití Apache Phoenix v HDInsight.  Další informace najdete v tématu

- [Přehled HDInsight HBase][hdinsight-hbase-overview]: HBase je databáze NoSQL otevřít zdroje, Apache, založená na Hadoop, který poskytuje RAM a silné konzistenci pro velké objemy dat nestrukturovaná a semistructured.
- [Zřízení HBase clusterů Azure virtuální síti se systémem][hdinsight-hbase-provision-vnet]: integrace virtuální sítě HBase clusterů může být nasazené na stejné virtuální síti aplikace tak, aby aplikace komunikovat s HBase přímo.
- [Konfigurace HBase replikace v HDInsight](hdinsight-hbase-geo-replication.md): Přečtěte si, jak konfigurovat HBase replikace mezi dvěma Azure datacentrech. 
- [Analýza Twitteru myšlenkou s HBase v HDInsight][hbase-twitter-sentiment]: Přečtěte si, jak to v reálném čase [myšlenkou analýza](http://en.wikipedia.org/wiki/Sentiment_analysis) velkých dat pomocí HBase v clusteru Hadoop v HDInsight.

[azure-portal]: https://portal.azure.com
[vnet-point-to-site-connectivity]: https://msdn.microsoft.com/library/azure/09926218-92ab-4f43-aa99-83ab4d355555#BKMK_VNETPT

[hdinsight-versions]: hdinsight-component-versioning.md
[hdinsight-hbase-get-started]: hdinsight-hbase-tutorial-get-started.md
[hdinsight-manage-portal]: hdinsight-administer-use-management-portal.md#connect-to-hdinsight-clusters-by-using-rdp
[hdinsight-hbase-provision-vnet]: hdinsight-hbase-provision-vnet.md
[hdinsight-hbase-overview]: hdinsight-hbase-overview.md
[hbase-twitter-sentiment]: hdinsight-hbase-analyze-twitter-sentiment.md

[hdinsight-hbase-phoenix-sqlline]: ./media/hdinsight-hbase-phoenix-squirrel/hdinsight-hbase-phoenix-sqlline.png
[img-certificate]: ./media/hdinsight-hbase-phoenix-squirrel/hdinsight-hbase-vpn-certificate.png
[img-vnet-diagram]: ./media/hdinsight-hbase-phoenix-squirrel/hdinsight-hbase-vnet-point-to-site.png
[img-squirrel-driver]: ./media/hdinsight-hbase-phoenix-squirrel/hdinsight-hbase-squirrel-driver.png
[img-squirrel-alias]: ./media/hdinsight-hbase-phoenix-squirrel/hdinsight-hbase-squirrel-alias.png
[img-squirrel]: ./media/hdinsight-hbase-phoenix-squirrel/hdinsight-hbase-squirrel.png
[img-squirrel-sql]: ./media/hdinsight-hbase-phoenix-squirrel/hdinsight-hbase-squirrel-sql.png


 
