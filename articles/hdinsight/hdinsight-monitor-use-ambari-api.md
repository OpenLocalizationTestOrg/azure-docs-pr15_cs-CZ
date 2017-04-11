<properties
    pageTitle="Sledování Hadoop clusterů v HDInsight pomocí rozhraní API Ambari | Microsoft Azure"
    description="Použití rozhraní API Ambari Apache pro vytváření, Správa a sledování Hadoop clusterů. Rozhraní API a intuitivní operátor nástroje skrýt složitost Hadoop."
    services="hdinsight"
    documentationCenter=""
    tags="azure-portal"
    authors="mumian"
    editor="cgronlun"
    manager="jhubbard"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/10/2016"
    ms.author="jgao"/>

# <a name="monitor-hadoop-clusters-in-hdinsight-using-the-ambari-api"></a>Sledování Hadoop clusterů v HDInsight pomocí rozhraní API Ambari

Informace o sledování clusterů HDInsight pomocí rozhraní API Ambari.

> [AZURE.NOTE] Informace v tomto článku je určen pro serveru s Windows HDInsight clusterů, které se verzi rozhraní REST API Ambari jen pro čtení. Na základě Linux clusterů najdete v článku [Správa Hadoop clusterů pomocí Ambari](hdinsight-hadoop-manage-ambari.md).

## <a name="what-is-ambari"></a>Co je Ambari?

[Apache Ambari] [ ambari-home] se používá k vytváření, Správa a sledování Apache Hadoop clusterů. Zahrnuje intuitivní kolekce operátor nástroje a robustní sadu rozhraní API, která skrýt složitost Hadoop zjednodušení operaci clusterů. Další informace o rozhraní API, přečtěte si článek Principy [Ambari rozhraní API][ambari-api-reference]. 

HDInsight v současné době podporuje pouze Ambari sledování funkci. Rozhraní API 1.0 Ambari podporuje clusterů verze 3.0 a 2.1 HDInsight. Tento článek popisuje přístup k rozhraní API Ambari na HDInsight 3.1 a 2.1 verzemi. Klíčové rozdíl mezi těmito dvěma je, že některé součásti změnily zavedením nových funkcí (například Server historie úlohy). 

**Zjistit předpoklady pro**

Před zahájením tohoto kurzu, musíte mít takto:

- **Pracoviště se Azure Powershellu**.

    [AZURE.INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]

- (Volitelné) [otáčení] [curl]. Ji nainstalovat, najdete v článku [otáčení verzí a soubory ke stažení][curl-download].

    >[AZURE.NOTE] Při použití příkazu otočení v systému Windows, použití uvozovek místo jednoduchých uvozovek hodnot možností.

- **Azure HDInsight obrázku**. Informace o vytváření obrázku najdete v článku [Začínáme s používáním HDInsight] [ hdinsight-get-started] nebo [poskytování HDInsight clusterů][hdinsight-provision]. Budete potřebovat následujícími údaji projít kurzu:

    Vlastnost obrázku|Název proměnné Azure prostředí PowerShell|Hodnota|Popis
    ---|---|---|---
    Název clusteru HDInsight|$clusterName||Název svůj cluster HDInsight.
    Uživatelské jméno obrázku|$clusterUsername||Clusteru uživatelské jméno uvedené, kdy byl vytvořen clusteru.
    Heslo obrázku|$clusterPassword||Heslo uživatele obrázku.

    >[AZURE.NOTE] Fill-in hodnoty v tabulce. To bude vhodné pro filtrované pomocí tohoto kurzu.

## <a name="jump-start"></a>Přechod start

Existuje několik způsobů použití Ambari ke sledování clusterů HDInsight.

**Použití Powershellu Azure**

Následuje skript Powershellu Azure získat informace o sledování úlohy MapReduce *HDInsight 3.1 clusteru.*  Klíčové rozdíl je jsme vyžádat tyto údaje z vláken služby (spíše než MapReduce).

    $clusterName = "<HDInsightClusterName>"
    $clusterUsername = "<HDInsightClusterUsername>"
    $clusterPassword = "<HDInsightClusterPassword>"

    $ambariUri = "https://$clusterName.azurehdinsight.net:443/ambari"
    $uriJobTracker = "$ambariUri/api/v1/clusters/$clusterName.azurehdinsight.net/services/yarn/components/resourcemanager"

    $passwd = ConvertTo-SecureString $clusterPassword -AsPlainText -Force
    $creds = New-Object System.Management.Automation.PSCredential ($clusterUsername, $passwd)

    $response = Invoke-RestMethod -Method Get -Uri $uriJobTracker -Credential $creds -OutVariable $OozieServerStatus

    $response.metrics.'yarn.queueMetrics'

Následující obrázek je skript Powershellu Azure usnadňující modulu MapReduce úlohy sledování informací *v HDInsight 2.1 obrázku*:

    $clusterName = "<HDInsightClusterName>"
    $clusterUsername = "<HDInsightClusterUsername>"
    $clusterPassword = "<HDInsightClusterPassword>"

    $ambariUri = "https://$clusterName.azurehdinsight.net:443/ambari"
    $uriJobTracker = "$ambariUri/api/v1/clusters/$clusterName.azurehdinsight.net/services/mapreduce/components/jobtracker"

    $passwd = ConvertTo-SecureString $clusterPassword -AsPlainText -Force
    $creds = New-Object System.Management.Automation.PSCredential ($clusterUsername, $passwd)

    $response = Invoke-RestMethod -Method Get -Uri $uriJobTracker -Credential $creds -OutVariable $OozieServerStatus

    $response.metrics.'mapred.JobTracker'

Výstup je:

![Výstup Jobtracker][img-jobtracker-output]

**Pomocí otočení**

Následující obrázek je příkladem získání informací o clusteru pomocí otočení:

    curl -u <username>:<password> -k https://<ClusterName>.azurehdinsight.net:443/ambari/api/v1/clusters/<ClusterName>.azurehdinsight.net

Výstup je:

    {"href":"https://hdi0211v2.azurehdinsight.net/ambari/api/v1/clusters/hdi0211v2.azurehdinsight.net/",
     "Clusters":{"cluster_name":"hdi0211v2.azurehdinsight.net","version":"2.1.3.0.432823"},
     "services"[
       {"href":"https://hdi0211v2.azurehdinsight.net/ambari/api/v1/clusters/hdi0211v2.azurehdinsight.net/services/hdfs",
        "ServiceInfo":{"cluster_name":"hdi0211v2.azurehdinsight.net","service_name":"hdfs"}},
       {"href":"https://hdi0211v2.azurehdinsight.net/ambari/api/v1/clusters/hdi0211v2.azurehdinsight.net/services/mapreduce",
        "ServiceInfo":{"cluster_name":"hdi0211v2.azurehdinsight.net","service_name":"mapreduce"}}],
     "hosts":[
       {"href":"https://hdi0211v2.azurehdinsight.net/ambari/api/v1/clusters/hdi0211v2.azurehdinsight.net/hosts/headnode0",
        "Hosts":{"cluster_name":"hdi0211v2.azurehdinsight.net",
                 "host_name":"headnode0"}},
       {"href":"https://hdi0211v2.azurehdinsight.net/ambari/api/v1/clusters/hdi0211v2.azurehdinsight.net/hosts/workernode0",
        "Hosts":{"cluster_name":"hdi0211v2.azurehdinsight.net",
                 "host_name":"headnode0.{ClusterDNS}.azurehdinsight.net"}}]}

**10/8/2014 uvolněte**:

Při použití na koncový bod Ambari "https://{clusterDns}.azurehdinsight.net/ambari/api/v1/clusters/{clusterDns}.azurehdinsight.net/services/{servicename}/components/{componentname}" pole *název_hostitele* vrací plně kvalifikovaný název domény (FQDN) uzlu místo názvu hostitele. V tomto příkladu před uvolněním 10/8/2014 vrácena jednoduše "**headnode0**". Po uvolnění 10/8/2014 se zobrazí úplný název "**headnode0. { ClusterDNS} .azurehdinsight .net**", jak je ukázáno v předchozím příkladu. Tato změna bylo nutné usnadnění scénáře, kde můžete být nasazené více typů obrázku (například HBase a Hadoop) v jedné sítě virtuální (VNET). K tomu dojde, například při použití HBase jako back-end platformu pro Hadoop.

## <a name="ambari-monitoring-apis"></a>Ambari sledování rozhraní API

V následující tabulce jsou uvedeny některé nejčastější Ambari sledování rozhraní API volání. Další informace o rozhraní API, přečtěte si článek Principy [Ambari rozhraní API][ambari-api-reference].

Sledování rozhraní API volání|IDENTIFIKÁTOR URI|Popis
---|---|---
Získání clusterů|`/api/v1/clusters`|
Získáte informace o obrázku.|`/api/v1/clusters/<ClusterName>.azurehdinsight.net`|clusterů, službami, hosts
Získání služeb|`/api/v1/clusters/<ClusterName>.azurehdinsight.net/services`|Služby zahrnují: hdfs, mapreduce
Získáte informace o služby.|`/api/v1/clusters/<ClusterName>.azurehdinsight.net/services/<ServiceName>`|
Získání součásti služby|`/api/v1/clusters/<ClusterName>.azurehdinsight.net/services/<ServiceName>/components`|HDFS: namenode datanode<br/>MapReduce: jobtracker; tasktracker
Získáte informace o součásti.|`/api/v1/clusters/<ClusterName>.azurehdinsight.net/services/<ServiceName>/components/<ComponentName>`|ServiceComponentInfo-komponentami, metriky
Získání hosts|`/api/v1/clusters/<ClusterName>.azurehdinsight.net/hosts`|headnode0 workernode0
Získáte informace o Host (hostitel).|`/api/v1/clusters/<ClusterName>.azurehdinsight.net/hosts/<HostName>`|
Získání komponentami|`/api/v1/clusters/<ClusterName>.azurehdinsight.net/hosts/<HostName>/host_components`|namenode resourcemanager
Pokud potřebujete hostitele component informace.|`/api/v1/clusters/<ClusterName>.azurehdinsight.net/hosts/<HostName>/host_components/<ComponentName>`|HostRoles součásti, host, metriky
Získání konfigurace|`/api/v1/clusters/<ClusterName>.azurehdinsight.net/configurations`|Konfigurace typy: základní webu hdfs webu, mapred webu, podregistru webu
Získáte informace o konfiguraci.|`/api/v1/clusters/<ClusterName>.azurehdinsight.net/configurations?type=<ConfigType>&tag=<VersionName>`|Konfigurace typy: základní webu hdfs webu, mapred webu, podregistru webu


##<a name="next-steps"></a>Další kroky

Teď jste se naučili používání Ambari sledování rozhraní API volání. Další informace najdete v tématu:

- [Správa portálu Azure HDInsight clusterů][hdinsight-admin-portal]
- [Správa pomocí prostředí PowerShell Azure HDInsight clusterů][hdinsight-admin-powershell]
- [Správa clusterů HDInsight pomocí rozhraní příkazového řádku][hdinsight-admin-cli]
- [HDInsight si přečtěte následující dokumentaci][hdinsight-documentation]
- [Začínáme s HDInsight][hdinsight-get-started]



[ambari-home]: http://ambari.apache.org/
[ambari-api-reference]: https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/index.md

[curl]: http://curl.haxx.se
[curl-download]: http://curl.haxx.se/download.html

[microsoft-hadoop-SDK]: http://hadoopsdk.codeplex.com/wikipage?title=Ambari%20Monitoring%20Client

[powershell-install]: powershell-install-configure.md
[powershell-script]: http://technet.microsoft.com/library/ee176949.aspx

[hdinsight-admin-powershell]: hdinsight-administer-use-powershell.md
[hdinsight-admin-portal]: hdinsight-administer-use-management-portal.md
[hdinsight-admin-cli]: hdinsight-administer-use-command-line.md
[hdinsight-documentation]: /documentation/services/hdinsight/
[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md
[hdinsight-provision]: hdinsight-provision-clusters.md

[img-jobtracker-output]: ./media/hdinsight-monitor-use-ambari-api/hdi.ambari.monitor.jobtracker.output.png
