<properties
    pageTitle="Vytvoření HBase clusterů v síti virtuální | Microsoft Azure"
    description="Začínáte používat HBase v Azure HDInsight. Naučte se vytvářet HDInsight HBase clusterů Azure virtuální síti se systémem."
    keywords=""
    services="hdinsight,virtual-network"
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
   ms.date="10/18/2016"
   ms.author="jgao"/>

# <a name="create-hbase-clusters-in-azure-virtual-network"></a>Vytvoření HBase clusterů v Azure virtuální sítě 

Naučte se vytvářet Azure HDInsight HBase clusterů v [Azure virtuální sítě][1].

Integrace virtuální sítě můžete být nasazené HBase clusterů na stejné virtuální sítě jako aplikace tak, aby aplikace komunikovat s HBase přímo. Mezi výhody patří:

- Přímé připojení webové aplikace na uzly clusteru HBase, což umožňuje komunikace přes HBase Java vzdálené řízení volání rozhraní API (RPC).
- Zvýšený výkon tak, že nemají přenosy pro vaši přejděte myší několika bran a vyrovnávání zatížení.
- Možnost zpracuje citlivé informace bezpečnější způsobem bez vystavení veřejné koncového bodu.

###<a name="prerequisites"></a>Zjistit předpoklady pro
Před zahájením tohoto kurzu, musíte mít takto:

- **Azure předplatného**. Viz [získání Azure bezplatnou zkušební verzi](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).

- **Pracoviště se Azure Powershellu**. V tématu [instalace a používání Azure Powershellu](https://azure.microsoft.com/documentation/videos/install-and-use-azure-powershell/). 

## <a name="create-hbase-cluster-into-virtual-network"></a>Vytvoření HBase obrázku do virtuálního sítě

V této části vytvoříte na základě Linux HBase obrázku s závislá účet Azure úložiště v Azure virtuální sítě pomocí [Správce prostředků Azure šablony](../resource-group-template-deploy.md). Jiné způsoby vytvoření obrázku a vysvětlení nastavení, v tématu [Vytvoření HDInsight clusterů](hdinsight-hadoop-provision-linux-clusters.md). Další informace o použití šablony k vytvoření Hadoop clusterů v Hdinsightu najdete v tématu [Vytvoření Hadoop clusterů pomocí šablon správce prostředků Azure HDInsight](hdinsight-hadoop-create-windows-clusters-arm-templates.md)

> [AZURE.NOTE] Některé vlastnosti byly pevně do šablony. Příklad:
>
> * __Umístění__: východoasijských USA 2
> * __Cluster verze: 3.4
> * __Počet uzel pracovníka clusterů__: 4
> * __Výchozí úložiště účet__: &lt;název clusteru > Uložit
> * __Virtuální síťový název__: &lt;název clusteru >-vnet
> * __Virtuální sítě adresní prostor__: 10.0.0.0/16
> * __Podsítě název__: výchozí
> * __Rozsah adres podsítí__: 10.0.0.0/24
>
> &lt;Clusteru název > nahradí s názvem clusteru zadáte při použití šablony.

1. Klikněte na následujícím obrázku otevření šablony na portálu Azure. Šablona se nachází v kontejneru veřejné objektů blob. 

    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fhditutorialdata.blob.core.windows.net%2Farmtemplates%2Fcreate-linux-based-hbase-cluster-in-vnet.json" target="_blank"><img src="https://acom.azurecomcdn.net/80C57D/cdn/mediahandler/docarticles/dpsmedia-prod/azure.microsoft.com/en-us/documentation/articles/hdinsight-hbase-tutorial-get-started-linux/20160201111850/deploy-to-azure.png" alt="Deploy to Azure"></a>

2. Z zásuvné **Vlastní nasazení** zadejte následující údaje:

    - **Předplatné**: Vyberte předplatné Azure použitých při tvorbě HDInsight clusteru, závislé účtu úložiště a Azure virtuální sítě.
    - **Pole Skupina zdroje**: vyberte **vytvořit nový**a zadejte název nové skupiny prostředků.
    - **Umístění**: Vyberte umístění pro skupiny zdrojů.
    - **Název_clusteru**: Zadejte název Hadoop obrázku, který chcete vytvořit.
    - **Shluk přihlašovací jméno a heslo**: výchozí přihlašovací jméno je **Správce**.
    - **SSH uživatelské jméno a heslo**: výchozí uživatelské jméno je **sshuser**.  Můžete ji přejmenovat. 
    - **Můžu souhlasit s podmínkami a ujednáními výše uvedených**: (vyberte)
    

3. Klikněte na **koupit**. Vytvoření clusteru trvá o asi 20 minut. Po vytvoření clusteru kliknete zásuvné obrázku na portálu a otevřete ho.

Po dokončení kurzu, můžete odstranit clusteru. S HDInsight data uložena v úložišti Azure, takže bezpečně odstraněním clusteru nepoužívá v. Můžete taky účtovány HDInsight clusteru, i když není použití. Protože poplatky za clusteru se opakovaně pokoušeli více než poplatky za úložiště, má smysl economic odstranit clusterů, pokud nejsou ve použití. Postup odstranění clusteru naleznete v tématu [Správa Hadoop clusterů HDInsight pomocí portálu Azure](hdinsight-administer-use-management-portal.md#delete-clusters).

Pokud chcete začít pracovat s novou HBase obrázku, můžete použít postupy součástí [začít používat HBase s Hadoop v HDInsight](hdinsight-hbase-tutorial-get-started.md).

## <a name="connect-to-the-hbase-cluster-using-hbase-java-rpc-apis"></a>Připojte se k němu HBase pomocí rozhraní API RPC HBase Java

1.  Vytvoření infrastrukturu jako virtuálního počítače služby (IaaS) do stejné Azure virtuální sítě a stejné podsítě. Pokyny pro vytvoření nového IaaS virtuálního počítače najdete v článku [Vytvoření virtuálního počítače spuštění systému Windows Server](../virtual-machines/virtual-machines-windows-hero-tutorial.md). Při postupovat podle kroků v tomto dokumentu, je nutné pro konfiguraci sítě použít následující:

    - __Virtuální sítě__: &lt;název clusteru >-vnet
    - __Podsítě__: výchozí

    > [AZURE.IMPORTANT] Nahrazení &lt;název clusteru > s názvem jste použili při vytváření clusteru HDInsight v předchozích krocích.

    Pomocí těchto hodnot nastaví její konfiguraci virtuálního počítače jako obrázku HDInsight použít stejné virtuální sítě a podsítě. To vám umožní, aby přímo vzájemně komunikovat.

2.  Při používání aplikace Java se připojit k HBase vzdáleně, že použijete plně kvalifikovaný název domény (FQDN). Co Pokud chcete zjistit, musíte nejprve získat přípony DNS specifické pro připojení HBase obrázku. Aby je dostala, můžete použít jednu z těchto způsobů:

    * Pomocí webového prohlížeče hovor Ambari:
    
        Přejděte na https://&lt;Název_clusteru >.azurehdinsight.net/api/v1/clusters/&lt;Název_clusteru > / hosts?minimal_response = true. Změní JSON soubor s přípony DNS.

    * Použití webu Ambari:

        1. Přejděte na https://&lt;Název_clusteru >. azurehdinsight.net.
        2. Klikněte na **Hosts** z nabídky nejvyšší.

    * Použití otočení ZBÝVAJÍCÍ volání:

            curl -u <username>:<password> -k https://<clustername>.azurehdinsight.net/ambari/api/v1/clusters/<clustername>.azurehdinsight.net/services/hbase/components/hbrest

        V JavaScript Object Notation (JSON) data vrácená vyhledejte položku "název_hostitele". To bude obsahovat úplný uzlů v clusteru. Příklad:

            ...
            "host_name": "wordkernode0.<clustername>.b1.cloudapp.net
            ...

        Část začátek název domény s názvem clusteru je přípona DNS. Například mycluster.b1.cloudapp.net.

    * Použití Powershellu Azure
    
        Použijte tento skript Powershellu Azure zaregistrovat funkci **Get-ClusterDetail** , která mohou sloužit k vrátit přípona DNS:

            function Get-ClusterDetail(
                [String]
                [Parameter( Position=0, Mandatory=$true )]
                $ClusterDnsName,
                [String]
                [Parameter( Position=1, Mandatory=$true )]
                $Username,
                [String]
                [Parameter( Position=2, Mandatory=$true )]
                $Password,
                [String]
                [Parameter( Position=3, Mandatory=$true )]
                $PropertyName
                )
            {
            <#
                .SYNOPSIS
                 Displays information to facilitate an HDInsight cluster-to-cluster scenario within the same virtual network.
                .Description
                 This command shows the following 4 properties of an HDInsight cluster:
                 1. ZookeeperQuorum (supports only HBase type cluster)
                    Shows the value of HBase property "hbase.zookeeper.quorum".
                 2. ZookeeperClientPort (supports only HBase type cluster)
                    Shows the value of HBase property "hbase.zookeeper.property.clientPort".
                 3. HBaseRestServers (supports only HBase type cluster)
                    Shows a list of host FQDNs that run the HBase REST server.
                 4. FQDNSuffix (supports all cluster types)
                    Shows the FQDN suffix of hosts in the cluster.
                .EXAMPLE
                 Get-ClusterDetail -ClusterDnsName {clusterDnsName} -Username {username} -Password {password} -PropertyName ZookeeperQuorum
                 This command shows the value of HBase property "hbase.zookeeper.quorum".
                .EXAMPLE
                 Get-ClusterDetail -ClusterDnsName {clusterDnsName} -Username {username} -Password {password} -PropertyName ZookeeperClientPort
                 This command shows the value of HBase property "hbase.zookeeper.property.clientPort".
                .EXAMPLE
                 Get-ClusterDetail -ClusterDnsName {clusterDnsName} -Username {username} -Password {password} -PropertyName HBaseRestServers
                 This command shows a list of host FQDNs that run the HBase REST server.
                .EXAMPLE
                 Get-ClusterDetail -ClusterDnsName {clusterDnsName} -Username {username} -Password {password} -PropertyName FQDNSuffix
                 This command shows the FQDN suffix of hosts in the cluster.
            #>

                $DnsSuffix = ".azurehdinsight.net"

                $ClusterFQDN = $ClusterDnsName + $DnsSuffix
                $webclient = new-object System.Net.WebClient
                $webclient.Credentials = new-object System.Net.NetworkCredential($Username, $Password)

                if($PropertyName -eq "ZookeeperQuorum")
                {
                    $Url = "https://" + $ClusterFQDN + "/ambari/api/v1/clusters/" + $ClusterFQDN + "/configurations?type=hbase-site&tag=default&fields=items/properties/hbase.zookeeper.quorum"
                    $Response = $webclient.DownloadString($Url)
                    $JsonObject = $Response | ConvertFrom-Json
                    Write-host $JsonObject.items[0].properties.'hbase.zookeeper.quorum'
                }
                if($PropertyName -eq "ZookeeperClientPort")
                {
                    $Url = "https://" + $ClusterFQDN + "/ambari/api/v1/clusters/" + $ClusterFQDN + "/configurations?type=hbase-site&tag=default&fields=items/properties/hbase.zookeeper.property.clientPort"
                    $Response = $webclient.DownloadString($Url)
                    $JsonObject = $Response | ConvertFrom-Json
                    Write-host $JsonObject.items[0].properties.'hbase.zookeeper.property.clientPort'
                }
                if($PropertyName -eq "HBaseRestServers")
                {
                    $Url1 = "https://" + $ClusterFQDN + "/ambari/api/v1/clusters/" + $ClusterFQDN + "/configurations?type=hbase-site&tag=default&fields=items/properties/hbase.rest.port"
                    $Response1 = $webclient.DownloadString($Url1)
                    $JsonObject1 = $Response1 | ConvertFrom-Json
                    $PortNumber = $JsonObject1.items[0].properties.'hbase.rest.port'

                    $Url2 = "https://" + $ClusterFQDN + "/ambari/api/v1/clusters/" + $ClusterFQDN + "/services/hbase/components/hbrest"
                    $Response2 = $webclient.DownloadString($Url2)
                    $JsonObject2 = $Response2 | ConvertFrom-Json
                    foreach ($host_component in $JsonObject2.host_components)
                    {
                        $ConnectionString = $host_component.HostRoles.host_name + ":" + $PortNumber
                        Write-host $ConnectionString
                    }
                }
                if($PropertyName -eq "FQDNSuffix")
                {
                    $Url = "https://" + $ClusterFQDN + "/ambari/api/v1/clusters/" + $ClusterFQDN + "/services/YARN/components/RESOURCEMANAGER"
                    $Response = $webclient.DownloadString($Url)
                    $JsonObject = $Response | ConvertFrom-Json
                    $FQDN = $JsonObject.host_components[0].HostRoles.host_name
                    $pos = $FQDN.IndexOf(".")
                    $Suffix = $FQDN.Substring($pos + 1)
                    Write-host $Suffix
                }
            }

        Po spuštění skript Powershellu Azure, použijte následující příkaz vrátit přípony DNS pomocí funkce **Získat ClusterDetail** . Při použití tohoto příkazu zadejte název clusteru HDInsight HBase, jménem správce a hesla správce.

            Get-ClusterDetail -ClusterDnsName <yourclustername> -PropertyName FQDNSuffix -Username <clusteradmin> -Password <clusteradminpassword>

        Vrátí přípony DNS. Například **yourclustername.b4.internal.cloudapp.net**.

    * Použití RDP
    
        Můžete také vzdálené plochy se připojit k obrázku HBase (budete připojeni k uzlu hlavy) a spusťte **ipconfig** z příkazového řádku získat přípony DNS. Pokyny k povolení vzdálené plochy RDP (Protocol) a připojení k clusteru pomocí RDP, najdete v článku [Správa Hadoop clusterů pomocí portálu Azure HDInsight][hdinsight-admin-portal].
        
        ![hdinsight.hbase.DNS.surffix][img-dns-surffix]


<!--
3.  Change the primary DNS suffix configuration of the virtual machine. This enables the virtual machine to automatically resolve the host name of the HBase cluster without explicit specification of the suffix. For example, the *workernode0* host name will be correctly resolved to workernode0 of the HBase cluster.

    To make the configuration change:

    1. RDP into the virtual machine.
    2. Open **Local Group Policy Editor**. The executable is gpedit.msc.
    3. Expand **Computer Configuration**, expand **Administrative Templates**, expand **Network**, and then click **DNS Client**.
    - Set **Primary DNS Suffix** to the value obtained in step 2:

        ![hdinsight.hbase.primary.dns.suffix][img-primary-dns-suffix]
    4. Click **OK**.
    5. Reboot the virtual machine.
-->

Potvrďte, že virtuální počítač mohli komunikovat s clusteru HBase příkazem `ping headnode0.<dns suffix>` z virtuální počítač. Například ping headnode0.mycluster.b1.cloudapp.net.

Tyto informace použít v aplikaci Java, můžete postupujte podle kroků v tématu [Použití Maven k vytváření Java aplikací používajících HBase s HDInsight (Hadoop)](hdinsight-hbase-build-java-maven.md) k vytvoření aplikace. K dispozici připojení k vzdálený server HBase, upravte soubor **hbase site.xml** v tomto příkladu pro účely Zookeeper úplný název domény. Příklad:

    <property>
        <name>hbase.zookeeper.quorum</name>
        <value>zookeeper0.<dns suffix>,zookeeper1.<dns suffix>,zookeeper2.<dns suffix></value>
    </property>

> [AZURE.NOTE] Další informace o překladu v Azure virtuální sítě, včetně použití vlastní serveru DNS, najdete v článku [Vyřešení DNS (Name)](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md).

##<a name="next-steps"></a>Další kroky

V tomto kurzu se naučíte vytvořit HBase obrázku. Další informace najdete v tématu:

- [Začínáme s HDInsight](hdinsight-hadoop-linux-tutorial-get-started.md)
- [Konfigurace HBase replikace v HDInsight](hdinsight-hbase-geo-replication.md)
- [Vytvoření Hadoop clusterů v HDInsight](hdinsight-provision-clusters.md)
- [Začínáme s používáním HBase s Hadoop v HDInsight](hdinsight-hbase-tutorial-get-started.md)
- [Analýza Twitter myšlenkou s HBase v HDInsight](hdinsight-hbase-analyze-twitter-sentiment.md)
- [Přehled virtuální sítě][vnet-overview]


[1]: http://azure.microsoft.com/services/virtual-network/
[2]: http://technet.microsoft.com/library/ee176961.aspx
[3]: http://technet.microsoft.com/library/hh847889.aspx

[hbase-get-started]: hdinsight-hbase-tutorial-get-started.md
[hbase-twitter-sentiment]: hdinsight-hbase-analyze-twitter-sentiment.md
[vnet-overview]: ../virtual-network/virtual-networks-overview.md
[vm-create]: ../virtual-machines/virtual-machines-windows-hero-tutorial.md

[azure-portal]: https://portal.azure.com
[azure-create-storageaccount]: ../storage-create-storage-account.md
[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/

[hdinsight-admin-powershell]: hdinsight-administer-use-powershell.md
[hdinsight-admin-portal]: hdinsight-administer-use-management-portal.md#rdp

[hdinsight-powershell-reference]: https://msdn.microsoft.com/library/dn858087.aspx


[twitter-streaming-api]: https://dev.twitter.com/docs/streaming-apis
[twitter-statuses-filter]: https://dev.twitter.com/docs/api/1.1/post/statuses/filter


[powershell-install]: powershell-install-configure.md


[hdinsight-customize-cluster]: hdinsight-hadoop-customize-cluster.md
[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md
[hdinsight-storage-powershell]: ../hdinsight-hadoop-use-blob-storage.md#powershell
[hdinsight-analyze-flight-delay-data]: hdinsight-analyze-flight-delay-data.md
[hdinsight-storage]: ../hdinsight-hadoop-use-blob-storage.md
[hdinsight-use-sqoop]: hdinsight-use-sqoop.md
[hdinsight-power-query]: hdinsight-connect-excel-power-query.md
[hdinsight-hive-odbc]: hdinsight-connect-excel-hive-ODBC-driver.md
[hdinsight-hbase-replication-dns]: hdinsight-hbase-geo-replication-configure-DNS.md

[img-dns-surffix]: ./media/hdinsight-hbase-provision-vnet/DNSSuffix.png
[img-primary-dns-suffix]: ./media/hdinsight-hbase-provision-vnet/PrimaryDNSSuffix.png
[img-provision-cluster-page1]: ./media/hdinsight-hbase-provision-vnet/hbasewizard1.png "Poskytování podrobnosti o nové HBase obrázku"
[img-provision-cluster-page5]: ./media/hdinsight-hbase-provision-vnet/hbasewizard5.png "Přizpůsobení HBase clusteru pomocí skriptu akce"

[azure-preview-portal]: https://portal.azure.com
