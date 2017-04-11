<properties
    pageTitle="Dostupnost Hadoop clusterů v HDInsight | Microsoft Azure"
    description="HDInsight nasadí vysoce k dispozici a spolehlivé clusterů s další hlavy uzel."
    services="hdinsight"
    tags="azure-portal"
    editor="cgronlun"
    manager="jhubbard"
    authors="mumian"
    documentationCenter=""/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="multiple"
    ms.topic="article"
    ms.date="10/21/2016"
    ms.author="jgao"/>


#<a name="availability-and-reliability-of-windows-based-hadoop-clusters-in-hdinsight"></a>Dostupnost a spolehlivost serveru s Windows Hadoop clusterů HDInsight


>[AZURE.NOTE] Kroky v tomto dokumentu jsou specifické pro clusterů serveru s Windows HDInsight. Pokud používáte Linux clusteru, naleznete v tématu [dostupnost a spolehlivost na základě Linux Hadoop clusterů HDInsight](hdinsight-high-availability-linux.md) informace specifické pro Linux.

HDInsight umožňuje zákazníkům nasazení různé typy obrázku, k různým datovým analýzy úloh. Typy clusteru dnes, které jsou Hadoop clusterů dotazu a analýzy zatížení, HBase clusterů NoSQL úloh a bouře clusterů pracovního vytížení zpracování události reálném čase. V rámci typu dané clusteru jsou různé role pro různé uzlů. Příklad:



- Hadoop clusterů HDInsight jsou nasazené s dvě role:
    - Hlavy uzel (2 uzly)
    - Data uzel (minimálně 1)

- HBase clusterů HDInsight používají se tři role:
    - Hlavy servery (2 uzly)
    - Oblast servery (minimálně 1 uzel)
    - Hlavní/Zookeeper uzlů (3 uzly)

- Bouře clusterů HDInsight jsou nasazeny s tři role:
    - Nimbus uzlů (uzly, 2)
    - Správce serverů (minimálně 1 uzel)
    - Uzly zookeeper (3 uzly)

Standardní implementace Hadoop clusterů obvykle mít hlavy načítání. HDInsight odebere jeden bod selhání s přidáním vedlejší hlavního uzlu/HEAD serveru/Nimbus uzel větší dostupnost a spolehlivost služby potřebné ke správě úloh. Tyto uzly serverů/Nimbus hlavy uzly/vedoucí jsou určené ke správě selhání kolegy uzly hladce, ale žádné výpadků předlohy služeb spuštěných na uzel hlavy způsobí clusteru přestanou fungovat.


Uzly [zooKeeper](http://zookeeper.apache.org/ ) (ZKs) byly přidány a se používají pro vodicí znak zvolení hlavy uzlů a zajistit pracovníka brány (GWs) a uzly vědět, kdy převzít sekundární hlavy uzel (vedoucí Uzel1) při neaktivní aktivní hlavy uzel (vedoucí Node0).

![Diagram vysoce spolehlivé hlavy uzlů v implementaci HDInsight Hadoop.](./media/hdinsight-high-availability/hadoop.high.availability.architecture.diagram.png)




## <a name="check-active-head-node-service-status"></a>Zkontrolujte stav služby active hlavy uzel
Informace o stavu služby v hlavní uzel a určit, které hlavního uzlu je aktivní, musí připojíte k obrázku Hadoop pomocí vzdálené plochy RDP (Protocol). RDP pokyny najdete v tématu [Správa Hadoop clusterů pomocí portálu Azure HDInsight](hdinsight-administer-use-management-portal.md#connect-to-hdinsight-clusters-by-using-rdp). Jakmile jste připojili ke clusteru, poklikejte na ikonu **Hadoop služba dostupné** umístěný na ploše a získat stav o který hlavy uzel Namenode, Jobtracker Templeton, Oozieservice, Metastore a Hiveserver2 služby spuštění nebo služby HDI 3.0, Namenode, správce prostředků, historie serveru, Templeton, Oozieservice, Metastore a Hiveserver2.

![](./media/hdinsight-high-availability/Hadoop.Service.Availability.Status.png)

Na snímek je aktivní hlavy uzel *headnode0*.

## <a name="access-log-files-on-the-secondary-head-node"></a>Soubory protokolu přístupu na vedlejší hlavy uzel

Přístup k projektu přihlásí sekundární hlavy uzel v případě, že se aktivní hlavy uzel procházení uživatelského rozhraní JobTracker stále funguje stejně jako u primární aktivní uzel. Přístup k JobTracker, musí připojit k obrázku Hadoop pomocí RDP podle popisu v předchozí části. Až budete mít vzdálený do clusteru, poklikejte na ikony **Hadoop název uzel** umístěný na ploše a potom klikněte na **NameNode protokoly** pro přístup k adresáři přihlásí sekundární hlavy uzel.

![](./media/hdinsight-high-availability/Hadoop.Head.Node.Log.Files.png)


## <a name="configure-head-node-size"></a>Konfigurace hlavního uzlu velikost
Hlavy uzly přiřazených jako velké virtuálních počítačích (VMs) ve výchozím nastavení. Velikost je vhodné pro správu spustit clusteru většině Hadoop úlohy. Ale existují scénáře, které může vyžadovat velmi velké VMs hlavy uzlů. Příklad je po clusteru spravovat velké množství malé Oozie úlohy.

Velmi velké VMs je možné konfigurovat pomocí rutin prostředí PowerShell Azure nebo HDInsight SDK.

Vytváření a zřízení clusteru pomocí prostředí PowerShell Azure jsou popsány v [Spravovat HDInsight pomocí Powershellu](hdinsight-administer-use-powershell.md). Konfigurace velmi velké hlavního uzlu vyžaduje přidání `-HeadNodeVMSize ExtraLarge` parametr `New-AzureRmHDInsightcluster` rutina používán tento kód.

    # Create a new HDInsight cluster in Azure PowerShell
    # Configured with an ExtraLarge head-node VM
    New-AzureRmHDInsightCluster `
                -ResourceGroupName $resourceGroupName `
                -ClusterName $clusterName ` 
                -Location $location `
                -HeadNodeVMSize ExtraLarge `
                -DefaultStorageAccountName "$storageAccountName.blob.core.windows.net" `
                -DefaultStorageAccountKey $storageAccountKey `
                -DefaultStorageContainerName $containerName  `
                -ClusterSizeInNodes $clusterNodes

SDK je podobný textu. Vytváření a zřízení clusteru pomocí SDK jsou popsány v [Pomocí SDK .NET HDInsight](hdinsight-provision-clusters.md#sdk). Konfigurace velmi velké hlavního uzlu vyžaduje přidání `HeadNodeSize = NodeVMSize.ExtraLarge` parametr `ClusterCreateParameters()` způsob používané v tomto kódu.

    # Create a new HDInsight cluster with the HDInsight SDK
    # Configured with an ExtraLarge head-node VM
    ClusterCreateParameters clusterInfo = new ClusterCreateParameters()
    {
        Name = clustername,
        Location = location,
        HeadNodeSize = NodeVMSize.ExtraLarge,
        DefaultStorageAccountName = storageaccountname,
        DefaultStorageAccountKey = storageaccountkey,
        DefaultStorageContainer = containername,
        UserName = username,
        Password = password,
        ClusterSizeInNodes = clustersize
    };


## <a name="next-steps"></a>Další kroky

- [Apache ZooKeeper](http://zookeeper.apache.org/ )
- [Připojení k clusterů HDInsight pomocí RDP](hdinsight-administer-use-management-portal.md#rdp)
- [Použití HDInsight .NET SDK](hdinsight-provision-clusters.md#sdk)
