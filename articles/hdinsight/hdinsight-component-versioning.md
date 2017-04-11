<properties
    pageTitle="Jaké jsou různé součásti dostupné s HDInsight obrázku? | Microsoft Azure"
    description="HDInsight podporuje více verzí a nasadit součásti clusteru Hadoop. Viz podporované rozdělení verze Hadoop a HortonWorks dat platformu (HDP)."
    services="hdinsight"
    editor="cgronlun"
    manager="jhubbard"
    authors="mumian"
    tags="azure-portal"
    documentationCenter=""/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/27/2016"
    ms.author="jgao"/>


# <a name="what-are-the-different-hadoop-components-available-with-hdinsight"></a>Jaké jsou různé součásti Hadoop dostupné s HDInsight?

Zjistěte, jakým jiné službě úrovně nabízená HDInsight, jakož i verze součástí různých hadoop zahrnutý v sadě HDInsight.

## <a name="hdinsight-standard-and-hdinsight-premium"></a>HDInsight standardních a HDInsight Premium

Azure HDInsight obsahuje nabídky cloudu velký dat ve dvou kategorií: **Standardní** a **Premium**. V tabulce pod částí jsou funkce, které jsou k dispozici **pouze v rámci Premium**. Funkce, které nejsou vyznačenými explicitně v tabulce tady jsou k dispozici v rámci standardní.

>[AZURE.NOTE] HDInsight Premium nabízející momentálně v náhledu a k dispozici pouze pro clusterů Linux.

| Funkce HDInsight Premium | Popis |
|--------------|---------------|
| Doméně clusterů HDInsight       | Spojení HDInsight clusterů a domény Azure Active Directory (AAD) pro zabezpečení na úrovni organizace. Můžete nakonfigurovat seznam zaměstnanců z vašeho podniku, který může ověřovat pomocí služby Azure Active Directory pro přihlášení k obrázku HDInsight. Řízení přístupu na základě rolí podregistru cenného papíru pomocí [Apache škálu](http://hortonworks.com/apache/ranger/), tedy omezení přístupu k datům na pouze velmi podobným způsobem podle potřeby můžete konfigurovat taky správce organizace. Nakonec správce auditovat dat povolený přístup zaměstnanců a všechny změny provedené zásady řízení přístupu, tedy dosáhnout vysokého stupně zásady správného řízení podnikových zdrojů. Další informace najdete v tématu [Konfigurace doméně clusterů HDInsight](hdinsight-domain-joined-configure).

### <a name="cluster-types-supported-for-premium"></a>Podporované pro Premium typy obrázku

Následující tabulka uvádí typ obrázku HDInsight a Premium podpory matici.

| Typ obrázku | Standardní  | Premium |
|--------------|---------------|--------------|
| Hadoop       | Ano           | Ano (pouze HDInsight 3,5)         |
| Spark        | Ano           | Ne          |
| HBase        | Ano           | Ne           |
| Bouře        | Ano           | Ne           |
| Interaktivní podregistru (verze Preview) | Ano | Ne |
| R Server (verze Preview) | Ano | Ne |

V této tabulce se aktualizují při další typy clusteru jsou součástí HDInsight Premium.

### <a name="pricing-and-sla"></a>Ceny a SLA

Informace o cenách a smlouvy o úrovni služeb pro HDInsight Premium najdete v tématu [ceny HDInsight](https://azure.microsoft.com/pricing/details/hdinsight/).

## <a name="hadoop-components-available-with-different-hdinsight-versions"></a>Součásti Hadoop dostupné s různými verzemi HDInsight

Azure HDInsight podporuje více verzí Hadoop obrázku, které můžou být nasazené kdykoli. Každou volbu verze vytvoří konkrétní verzi rozdělení Hortonworks dat platformu (HDP) a sadu součástí, které jsou obsaženy v této rozdělení. V následující tabulce jsou rozpis verze součástí přidružené verze obrázku HDInsight. Poznámka: výchozí verze obrázku používaný Azure HDInsight právě 3.4, od 09/14/2016 podle HDP 2.4.

> [AZURE.NOTE] Na výchozí verzi ze služby mohou změnit bez předchozího upozornění. Doporučujeme určit verzi při vytváření clusterů pomocí prostředí PowerShell SDK/Azure .NET a rozhraní Azure příkazového řádku, pokud máte závislost verze. 

Součásti|HDInsight verze 3.5 | HDInsight verze 3.4 (výchozí) | HDInsight verze 3.3 | HDInsight verze 3,2 |HDInsight podverzí 3.1 |HDInsight verze 3.0|
---|---|---|---|---|---|---
Platformu Hortonworks dat|2.5|2.4|2.3|2.2|2.1.7|2.0|
Apache Hadoop a vláken|2.7.3|2.7.1|2.7.1|2.6.0|2.4.0|2.2.0|
Apache Tez|0.7.0|0.7.0|0.7.0 | 0.5.2|0.4.0||
Prasátko Apache|0.16.0|0.15.0|0.15.0|0.14.0|0.12.1|0.12.0|
Apache podregistru & HCatalog|1.2.1.2.5|1.2.1|1.2.1|0.14.0|0.13.1|0.12.0|
Apache HBase |1.1.2|1.1.2|1.1.1|0.98.4|0.98.0||
Apache Sqoop|1.4.6|1.4.6|1.4.6|1.4.5|1.4.4|1.4.4|1.4.3
Apache Oozie|4.2.0|4.2.0|4.2.0|4.1.0|4.0.0|4.0.0|
Apache Zookeeper|3.4.6|3.4.6|3.4.6|3.4.6|3.4.5|3.4.5|
Apache bouře|1.0.1|0.10.0|0.10.0|0.9.3|0.9.1||
Apache Mahout|0.9.0+|0.9.0+|0.9.0+|0.9.0|0.9.0||
Apache Phoenix|4.7.0|4.4.0|4.4.0|4.2.0|4.0.0.2.1.7.0-2162||
Apache Spark|1.6.2 + 2.0 (pouze Linux)|1.6.0 (pouze Linux)|1.5.2 (jen/pokusným Linux Tvůrce dotazů)|1.3.1 (pouze pro systém Windows)|||

**Získat informace o aktuální verzi součásti**

Verze součástí přidružené verze obrázku HDInsight může změnit v budoucích aktualizacích HDInsight. Jedním ze způsobů a určit, k dispozici součásti pro ověření, které verze použily clusteru je použití rozhraní REST API Ambari. Příkaz **GetComponentInformation** mohou sloužit k vyhledání informací o součásti služby. Podrobnosti najdete v tématu [si přečtěte následující dokumentaci Ambari][ambari-docs]. Další způsob, jak získat tyto informace se a přihlaste se k clusteru pomocí vzdálené plochy ověřit obsah "C:\apps\dist\" adresáře přímo.


**Poznámky k verzi**

Další poznámky na nejnovější verze Hdinsightu najdete v článku [HDInsight poznámky k verzi](hdinsight-release-notes.md) .


## <a name="supported-hdinsight-versions"></a>Podporované HDInsight verze
Následující tabulka uvádí verzích HDInsight momentálně neexistuje, odpovídající verze platformy Hortonworks dat, které používají a jejich vydání data. Známé, jejich data vypršení platnosti a odstranění podpory jsou rovněž k dispozici. Dejte pozor následující:

* Vysoce dostupných clusterů s dva hlavní uzly jsou ve výchozím nastavení nasazeny HDInsight 2.1 a nad. Nejsou k dispozici pro clusterů 1,6 HDInsight.
* Po vypršení platnosti podpory pro konkrétní verzi nemusí být k dispozici prostřednictvím portálu Azure. Následující tabulka uvádí, které jsou k dispozici na portálu klasické Azure. Verze obrázku budou k dispozici pomocí `Version` parametrů v příkazu prostředí Windows PowerShell [New-AzureRmHDInsightCluster](https://msdn.microsoft.com/library/mt619331.aspx) a .NET SDK do jeho odstranění data.

HDInsight verze|HDP verze|S OPERAČNÍM SYSTÉMEM OM|Dostupnost|Datum vydání|K dispozici v Azure portálu|Datum vypršení platnosti podpory|Odstranění datum
---|---|---|---|---|---|---|---
HDI 3.5 | HDP 2.5| Se systémem Ubuntu 16 | Ano | 9/30/2016 | Ano | 
HDI 3.4|HDP 2.4|Systémem Ubuntu 14.0.4 l|Ano|03/29/2016|Ano|12/29/2016|1/9/2018|
HDI 3.3|HDP 2.3|Systémem Ubuntu 14.0.4 2012R2 l nebo Windows Server|Ano|12/02/2015|Ano|06/27/2016|07/31/2017
HDI 3,2|HDP 2.2|Systémem Ubuntu 12.04 2012R2 l nebo Windows Server|Ano|2/18/2015|Ano|3/1/2016|04/01/2017
HDI 3.1|HDP 2.1|Windows Server 2012R2|Ano|6/24/2014|Ne|05/18/2015|06/30/2016
HDI 3.0|HDP 2.0|Windows Server 2012R2|Ano|02/11/2014|Ne|09/17/2014|06/30/2015
HDI 2.1|HDP 1.3|Windows Server 2012R2|Ano|10/28/2013|Ne|05/12/2014|05/31/2015
HDI 1,6|HDP 1.1||Ne|10/28/2013|Ne|04/26/2014|05/31/2015

**Nasazení jiného než výchozího clusterů**

### <a name="the-service-level-agreement-for-hdinsight-cluster-versions"></a>Smlouva úrovni služeb pro HDInsight verze obrázku

SLA je definována "Podpory okno". Podpora okno odkazuje po dobu podporovaný verze obrázku HDInsight Microsoft zákaznické služby a podpora. HDInsight clusteru je mimo okno podpory, pokud jeho verzi obsahuje **Datum vypršení platnosti podpory** za aktuální datum. Seznam podporovaných verzí obrázku Hdinsightu najdete v předchozí tabulce. Datum vypršení platnosti podpory pro dané HDInsight verze X (Jakmile je k dispozici novější verze X + 1) se vypočítá jako pozdější části:  

- Vzorec 1: Přidání 180 dnů na verzi obrázku HDInsight datum vydání X.
- Vzorec 2: Přidání 90 dnů k určitému datu HDInsight verze obrázku X + 1 (na pozdější verzi po X) je k dispozici na portálu.

**Odstranění datum** je datum, po jejímž uplynutí verze obrázku na nelze vytvořit HDInsight.

> [AZURE.NOTE] Serveru s Windows HDInsight obrázku (včetně verze 2.1 3.0, 3.1, 3,2 a 3.3) v Azure hosta OS řady 4, která používá 64bitovou verzi Windows serveru 2012 R2 a podporuje .NET Framework 4.0, 4.5, 4.5.1 a 4.5.2 spustit.

## <a name="hortonworks-release-notes-associated-with-hdinsight-versions"></a>Hortonworks poznámky přidružené HDInsight verze##

* Verze obrázku HDInsight 3.4 používá rozdělení Hadoop založený na [Hortonworks dat platformy 2.4](http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.4.0/bk_HDP_RelNotes/content/ch_relnotes_v240.html). Toto je **výchozí** cluster Hadoop vytvořili pomocí portálu.



* Verze obrázku HDInsight 3.3 používá rozdělení Hadoop založený na [Hortonworks dat platformy 2.3](http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.3.0/bk_HDP_RelNotes/content/ch_relnotes_v230.html).
    * Poznámky k verzi Apache bouře jsou dostupné [tady](https://storm.apache.org/2015/11/05/storm0100-released.html).
    * Poznámky k verzi Apache podregistru jsou dostupné [tady](https://issues.apache.org/jira/secure/ReleaseNote.jspa?version=12332384&styleName=Text&projectId=12310843).

* Verze obrázku HDInsight 3,2 používá rozdělení Hadoop založený na [Hortonworks dat platformy 2.2][hdp-2-2].  

    * Poznámky k verzi pro konkrétní součásti Apache - [podregistru 0.14](https://issues.apache.org/jira/secure/ReleaseNote.jspa?projectId=12310843&version=12326450) [Prasátko 0.14](https://issues.apache.org/jira/secure/ReleaseNote.jspa?projectId=12310730&version=12326954), [HBase 0.98.4](https://issues.apache.org/jira/secure/ReleaseNote.jspa?projectId=12310753&version=12326810), [Phoenix 4.2.0](https://issues.apache.org/jira/secure/ReleaseNote.jspa?projectId=12315120&version=12327581), [2.6 M/R](https://issues.apache.org/jira/secure/ReleaseNote.jspa?projectId=12310941&version=12327180), [HDFS 2.6](https://issues.apache.org/jira/secure/ReleaseNote.jspa?projectId=12310942&version=12327181), [2.6 vláken](https://issues.apache.org/jira/secure/ReleaseNote.jspa?projectId=12313722&version=12327197), [běžné](https://issues.apache.org/jira/secure/ReleaseNote.jspa?projectId=12310240&version=12327179), [Tez 0.5.2](https://issues.apache.org/jira/secure/ReleaseNote.jspa?projectId=12314426&version=12328742), [Ambari 2.0](https://issues.apache.org/jira/secure/ReleaseNote.jspa?projectId=12312020&version=12327486), [bouře 0.9.3](https://issues.apache.org/jira/secure/ReleaseNote.jspa?projectId=12314820&version=12327112), [Oozie 4.1.0](https://issues.apache.org/jira/secure/ReleaseNote.jspa?version=12324960&projectId=12311620).


* Verze obrázku HDInsight 3.1 používá rozdělení Hadoop založený na [Hortonworks datovou platformu 2.1.7][hdp-2-1-7]. HDInsight 3.1 clusterů vytvořené před 11/7/2014 byly založeny na [Hortonworks datovou platformu 2.1.1][hdp-2-1-1].

* HDInsight clusteru verze 3.0 používá rozdělení Hadoop založený na [Hortonworks dat Platform 2.0][hdp-2-0-8].

* HDInsight clusteru verze 2.1 používá rozdělení Hadoop založený na [Hortonworks dat platformy 1.3][hdp-1-3-0].

* Verze obrázku HDInsight 1,6 používá rozdělení Hadoop založený na [Hortonworks dat platformy 1.1][hdp-1-1-0].


[image-hdi-versioning-versionscreen]: ./media/hdinsight-component-versioning/hdi-versioning-version-screen.png

[wa-forums]: http://azure.microsoft.com/support/forums/

[connect-excel-with-hive-ODBC]: hdinsight-connect-excel-hive-ODBC-driver.md

[hdp-2-2]: http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.2.0/HDP_2.2.0_Release_Notes_20141202_version/index.html

[hdp-2-1-7]: http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.1.7-Win/bk_releasenotes_HDP-Win/content/ch_relnotes-HDP-2.1.7.html

[hdp-2-1-1]: http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.1.1/bk_releasenotes_hdp_2.1/content/ch_relnotes-hdp-2.1.1.html

[hdp-2-0-8]: http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.0.8.0/bk_releasenotes_hdp_2.0/content/ch_relnotes-hdp2.0.8.0.html

[hdp-1-3-0]: http://docs.hortonworks.com/HDPDocuments/HDP1/HDP-1.3.0/bk_releasenotes_hdp_1.x/content/ch_relnotes-hdp1.3.0_1.html

[hdp-1-1-0]: http://docs.hortonworks.com/HDPDocuments/HDP1/HDP-Win-1.1/bk_releasenotes_HDP-Win/content/ch_relnotes-hdp-win-1.1.0_1.html

[ambari-docs]: https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/index.md

[zookeeper]: http://zookeeper.apache.org/
