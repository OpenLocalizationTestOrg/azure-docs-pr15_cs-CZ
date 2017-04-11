<properties
    pageTitle="K instalaci Solr Hadoop clusteru použijte akci skriptu | Microsoft Azure"
    description="Informace o přizpůsobení HDInsight obrázku s Solr pomocí skriptu akce."
    services="hdinsight"
    documentationCenter=""
    authors="nitinme"
    manager="jhubbard"
    editor="cgronlun"
    tags="azure-portal"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="02/05/2016"
    ms.author="nitinme"/>

# <a name="install-and-use-solr-on-hdinsight-hadoop-clusters"></a>Instalace a používání Solr v HDInsight Hadoop clusterů

Zjistěte, jak můžete přizpůsobit podle obrázku HDInsight Windows s Solr pomocí skriptu akce a jak pomocí Solr vyhledávat data. Informace o použití Solr s Linux clusteru najdete v tématu [instalace a použití Solr na clusterů HDinsight Hadoop (Linux)](hdinsight-hadoop-solr-install-linux.md).
 
Solr typy dosažených obrázku (Hadoop bouře, HBase, Spark) můžete nainstalovat na Azure Hdinsightu pomocí *Skriptu akce*. Ukázka skriptu nainstalovat Solr HDInsight clusteru je k dispozici jen pro čtení Azure úložiště objektů blob na [https://hdiconfigactions.blob.core.windows.net/solrconfigactionv01/solr-installer-v01.ps1](https://hdiconfigactions.blob.core.windows.net/solrconfigactionv01/solr-installer-v01.ps1). 

Ukázka skriptu funguje jenom s HDInsight clusteru podverzí 3.1. Další informace o verzích clusteru Hdinsightu najdete v článku [verze obrázku HDInsight](hdinsight-component-versioning.md).

Ukázka skriptu použité v tomto tématu vytvoří serveru s Windows Solr obrázku s konkrétní konfigurace. Podle potřeby můžete nakonfigurovat Solr obrázku s jinou kolekcí shards, schémat, repliky, atd musí změnit skriptu a Solr binární příslušným způsobem.

**Související články**

- [Instalace a používání Solr v clusterů HDinsight Hadoop (Linux)](hdinsight-hadoop-solr-install-linux.md)
- [Vytvoření Hadoop clusterů HDInsight](hdinsight-provision-clusters.md): Obecné informace o vytváření clusterů HDInsight.
- [Přizpůsobení clusteru HDInsight pomocí skriptu akce][hdinsight-cluster-customize]: Obecné informace o úpravách clusterů HDInsight pomocí skriptu akce.
- [Vývoj akci skriptu skripty pro HDInsight](hdinsight-hadoop-script-actions.md).


## <a name="what-is-solr"></a>Co je Solr?

<a href="http://lucene.apache.org/solr/features.html" target="_blank">Apache Solr</a> je platforma hledání organizace, který umožňuje výkonné fulltextové vyhledávání na data. Během Hadoop umožňuje ukládání a správa velké množství dat, Apache Solr poskytuje možnosti vyhledávání rychle načítat data. 

## <a name="install-solr-using-portal"></a>Instalace Solr portálu

1. Zahájení vytváření clusteru pomocí možnost **Vytvořit vlastní** , jak je uvedeno na stránce [vytvořit Hadoop clusterů HDInsight](hdinsight-provision-clusters.md#portal).
2. Na stránce **Akce skriptu** průvodce klikněte na **Přidat akci skript** k poskytování údajů o akci skript, jak je ukázáno v následujícím příkladu:

    ![Použití akce skript k přizpůsobení clusteru] (./media/hdinsight-hadoop-solr-install/hdi-script-action-solr.png "Použití akce skript k přizpůsobení clusteru")

    <table border='1'>
        <tr><th>Vlastnost</th><th>Hodnota</th></tr>
        <tr><td>Jméno</td>
            <td>Zadejte název akce skriptu. Například <b>Instalace Solr</b>.</td></tr>
        <tr><td>Skript URI</td>
            <td>Zadejte identifikátor URI (Uniform Resource) skript, který je zavolat a přizpůsobení clusteru. Například <i>https://hdiconfigactions.blob.core.windows.net/solrconfigactionv01/solr-installer-v01.ps1</i></td></tr>
        <tr><td>Typ uzel</td>
            <td>Určete uzly, na kterých běží skript přizpůsobení. Můžete vybrat <b>všechny uzly</b>, <b>pouze záhlaví uzlů</b>nebo <b>pouze pracovníka uzlů</b>.
        <tr><td>Parametry</td>
            <td>Zadejte parametry, v případě potřeby skriptem. Skript, který chcete nainstalovat Solr nevyžaduje všechny parametry, takže to můžou být prázdné.</td></tr>
    </table>

    Můžete přidat víc akci skriptu pro instalaci více součástí na clusteru. Až přidáte skripty, klikněte na zaškrtnutí začnete vytvářet clusteru.


## <a name="use-solr"></a>Použití Solr

Musí začínat indexování Solr s některé datové soubory. Pomocí Solr můžete pak dělat vyhledávacích dotazů indexovaná data. Proveďte následující kroky pro použití Solr v HDInsight obrázku:

1. **Použití vzdálené plochy RDP (Protocol) na vzdálený do clusteru HDInsight s Solr nainstalovaný**. Z portálu Azure povolte připojení ke vzdálené ploše pro obrázku, který jste vytvořili pomocí Solr nainstalovaný a potom remote do clusteru. Pokyny najdete v tématu [připojení clusterů HDInsight pomocí RDP](hdinsight-administer-use-management-portal.md#connect-to-clusters-using-rdp).

2. **Index Solr uložením datové soubory**. Když indexu Solr dokumenty do něj napíšete, budete muset vyhledat. Indexování Solr, umožňuje RDP vzdálené do clusteru, přejděte na plochu, otevřete příkazového řádku Hadoop a k **C:\apps\dist\solr-4.7.2\example\exampledocs**. Spusťte tento příkaz:

        java -jar post.jar solr.xml monitor.xml

    Zobrazí se následující výstup konzole:

        POSTing file solr.xml
        POSTing file monitor.xml
        2 files indexed.
        COMMITting Solr index changes to http://localhost:8983/solr/update..
        Time spent: 0:00:01.624

    Nástroj post.jar indexy Solr s **solr.xml** a **monitor.xml**Dvouvýběrový párový dokumenty. Nástroj post.jar a ukázkové dokumenty jsou dostupné instalace Solr.

3. **Použití Solr řídicí panel hledání v rámci indexované dokumenty**. V relaci RDP HDInsight clusteru, spusťte aplikaci Internet Explorer a spuštění Solr řídicího panelu na **http://headnodehost:8983/solr / #/**. V levém podokně **Volič Core** rozevíracím seznamu vyberte **collection1**v rámci tohoto klepněte a **dotazů**. Jako příklad vyberte a vrátit všechny dokumenty, které v Solr, zadejte tyto hodnoty:

    * Do textového pole **otázky** zadávání ** \*:**\*. Všechny dokumenty, které indexovaných to bude vrácená v Solr. Pokud chcete najít určitý řetězec v rámci dokumenty, můžete zadat Tento řetězec.
    
    * Do textového pole **wt** vyberte výstupní formát. Výchozí hodnota je **json**. Klikněte na tlačítko **Spustit dotaz**.

    ![Použití akce skript k přizpůsobení clusteru] (./media/hdinsight-hadoop-solr-install/hdi-solr-dashboard-query.png "Spuštění dotazu na Solr řídicího panelu")
    
    Vrátí dva dokumenty, které použité pro indexování Solr. Výstup vypadá takto:

            "response": {
                "numFound": 2,
                "start": 0,
                "maxScore": 1,
                "docs": [
                  {
                    "id": "SOLR1000",
                    "name": "Solr, the Enterprise Search Server",
                    "manu": "Apache Software Foundation",
                    "cat": [
                      "software",
                      "search"
                    ],
                    "features": [
                      "Advanced Full-Text Search Capabilities using Lucene",
                      "Optimized for High Volume Web Traffic",
                      "Standards Based Open Interfaces - XML and HTTP",
                      "Comprehensive HTML Administration Interfaces",
                      "Scalability - Efficient Replication to other Solr Search Servers",
                      "Flexible and Adaptable with XML configuration and Schema",
                      "Good unicode support: héllo (hello with an accent over the e)"
                    ],
                    "price": 0,
                    "price_c": "0,USD",
                    "popularity": 10,
                    "inStock": true,
                    "incubationdate_dt": "2006-01-17T00:00:00Z",
                    "_version_": 1486960636996878300
                  },
                  {
                    "id": "3007WFP",
                    "name": "Dell Widescreen UltraSharp 3007WFP",
                    "manu": "Dell, Inc.",
                    "manu_id_s": "dell",
                    "cat": [
                      "electronics and computer1"
                    ],
                    "features": [
                      "30\" TFT active matrix LCD, 2560 x 1600, .25mm dot pitch, 700:1 contrast"
                    ],
                    "includes": "USB cable",
                    "weight": 401.6,
                    "price": 2199,
                    "price_c": "2199,USD",
                    "popularity": 6,
                    "inStock": true,
                    "store": "43.17614,-90.57341",
                    "_version_": 1486960637584081000
                  }
                ]
              }


4. **Doporučené: zálohovat indexované data z Solr k úložišti objektů Blob Azure přidružený k obrázku HDInsight**. Jako vhodné byste měli zálohovat indexovaná data z Solr uzlů do úložiště objektů Blob Azure. Proveďte následující postup:

    1. Z RDP relace spusťte aplikaci Internet Explorer a přejděte na následující adresu URL:

            http://localhost:8983/solr/replication?command=backup

        Měli byste vidět odpověď takto:

            <?xml version="1.0" encoding="UTF-8"?>
            <response>
              <lst name="responseHeader">
                <int name="status">0</int>
                <int name="QTime">9</int>
              </lst>
              <str name="status">OK</str>
            </response>

    2. V vzdálenou relaci, přejděte na {SOLR_HOME}\{kolekce} \data. Pro clusteru vytvořený prostřednictvím skriptu ukázkové to by měl být **C:\apps\dist\solr-4.7.2\example\solr\collection1\data**. Na tomto místě byste měli vidět snímek složky vytvořili s názvem podobný * *snímek.* časové razítko***.

    3. ZIP složce snímek a nahrajte k úložišti objektů Blob Azure. Z příkazového řádku Hadoop přejděte na umístění složky snímek pomocí tento příkaz:

              hadoop fs -CopyFromLocal snapshot._timestamp_.zip /example/data

        Tento příkaz slouží ke kopírování snímku do /example/data/v části kontejneru v rámci výchozí úložiště účet spojený s clusteru.

## <a name="install-solr-using-aure-powershell"></a>Instalace Solr pomocí prostředí Aure PowerShell

V tématu [přizpůsobení HDInsight clusterů pomocí skriptu akce](hdinsight-hadoop-customize-cluster.md#call_scripts_using_powershell).  Vzorku ukazuje, jak nainstalovat Spark pomocí prostředí PowerShell Azure. Budete muset přizpůsobit skript, který chcete použít [https://hdiconfigactions.blob.core.windows.net/solrconfigactionv01/solr-installer-v01.ps1](https://hdiconfigactions.blob.core.windows.net/solrconfigactionv01/solr-installer-v01.ps1).

## <a name="install-solr-using-net-sdk"></a>Instalace Solr pomocí .NET SDK

V tématu [přizpůsobení HDInsight clusterů pomocí skriptu akce](hdinsight-hadoop-customize-cluster.md#call_scripts_using_azure_powershell). Vzorku ukazuje, jak nainstalovat Spark pomocí .NET SDK. Budete muset přizpůsobit skript, který chcete použít [https://hdiconfigactions.blob.core.windows.net/solrconfigactionv01/solr-installer-v01.ps1](https://hdiconfigactions.blob.core.windows.net/solrconfigactionv01/solr-installer-v01.ps1).



## <a name="see-also"></a>Viz taky

- [Instalace a používání Solr v clusterů HDinsight Hadoop (Linux)](hdinsight-hadoop-solr-install-linux.md)
- [Vytvoření Hadoop clusterů HDInsight](hdinsight-provision-clusters.md): Obecné informace o vytváření clusterů HDInsight.
- [Přizpůsobení clusteru HDInsight pomocí skriptu akce][hdinsight-cluster-customize]: Obecné informace o úpravách clusterů HDInsight pomocí skriptu akce.
- [Vývoj akci skriptu skripty pro HDInsight](hdinsight-hadoop-script-actions.md).
- [Instalace a používání Spark v HDInsight clusterů][hdinsight-install-spark]: Ukázka skriptu akci o instalaci Spark.
- [Instalace R na HDInsight clusterů][hdinsight-install-r]: Ukázka skriptu akci o instalaci R.
- [Instalace Giraph na HDInsight clusterů](hdinsight-hadoop-giraph-install.md): Ukázka skriptu akci o instalaci Giraph.


[powershell-install-configure]: powershell-install-configure.md
[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-install-r]: hdinsight-hadoop-r-scripts.md
[hdinsight-install-spark]: hdinsight-hadoop-spark-install.md
[hdinsight-cluster-customize]: hdinsight-hadoop-customize-cluster.md
