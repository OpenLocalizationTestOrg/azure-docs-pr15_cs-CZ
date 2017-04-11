<properties
    pageTitle="Správa Hadoop clusterů pomocí rozhraní příkazového řádku Azure | Microsoft Azure"
    description="Jak spravovat Hadoop clusterů HDIsight pomocí rozhraní příkazového řádku Azure"
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
    ms.date="08/10/2016"
    ms.author="jgao"/>

# <a name="manage-hadoop-clusters-in-hdinsight-using-the-azure-cli"></a>Správa Hadoop clusterů pomocí rozhraní příkazového řádku Azure HDInsight

[AZURE.INCLUDE [selector](../../includes/hdinsight-portal-management-selector.md)]

Naučte se používat [rozhraní Azure příkazového řádku](../xplat-cli-install.md) pro správu Hadoop clusterů Azure HDInsight. Rozhraní příkazového řádku Azure je součástí Node.js. Lze jej využít platformy, který podporuje Node.js, včetně systému Windows, Mac a Linux.

Tento článek se zabývá pouze pomocí rozhraní příkazového řádku Azure HDInsight. Obecné průvodce o používání rozhraní příkazového řádku Azure najdete v tématu [instalace a konfigurace rozhraní příkazového řádku Azure][azure-command-line-tools].

[AZURE.INCLUDE [use-latest-version](../../includes/hdinsight-use-latest-cli.md)]

##<a name="prerequisites"></a>Zjistit předpoklady pro

Než začnete v tomto článku, musíte mít takto:

- **Azure předplatného**. Viz [získání Azure bezplatnou zkušební verzi](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
- **Rozhraní příkazového řádku azure** – přečtěte si téma [instalace a konfigurace rozhraní příkazového řádku Azure](../xplat-cli-install.md) informace o instalaci a konfiguraci.
- **Připojení k Azure**pomocí následujícího příkazu:

        azure login

    Další informace o ověřování pomocí pracovního nebo školního účtu najdete v článku [připojení k předplatnému Azure z Azure rozhraní příkazového řádku](xplat-cli-connect.md).
    
- **Přepnutí do režimu správce prostředků Azure**pomocí následujícího příkazu:

        azure config mode arm

Potřebujete pomoct, použijte přepínač **-h** .  Příklad:

    azure hdinsight cluster create -h
    
##<a name="create-clusters"></a>Vytvoření clusterů

V tématu [Vytvoření Linux založené clusterů v pomocí rozhraní příkazového řádku Azure HDInsight](hdinsight-hadoop-create-linux-clusters-azure-cli.md).

##<a name="list-and-show-cluster-details"></a>Seznam a zobrazit podrobnosti obrázku
Pomocí následujících příkazů seznam a zobrazit podrobnosti obrázku:

    azure hdinsight cluster list
    azure hdinsight cluster show <Cluster Name>

![HDI. CLIListCluster][image-cli-clusterlisting]


##<a name="delete-clusters"></a>Odstranění clusterů

Umožňuje odstranit clusteru tento příkaz:

    azure hdinsight cluster delete <Cluster Name>

Můžete taky odstranit clusteru odstraněním skupina zdroje, který obsahuje clusteru. Dejte pozor, budou odstraněny všechny zdroje ve skupině včetně výchozí účet úložiště.

    azure group delete <Resource Group Name>

##<a name="scale-clusters"></a>Měřítko clusterů

Pokud chcete změnit velikost obrázku Hadoop:

    azure hdinsight cluster resize [options] <clusterName> <Target Instance Count>


## <a name="enabledisable-http-access-for-a-cluster"></a>Povolit nebo zakázat HTTP přístupu pro clusteru

    azure hdinsight cluster enable-http-access [options] <Cluster Name> <userName> <password>
    azure hdinsight cluster disable-http-access [options] <Cluster Name>

## <a name="enabledisable-rdp-access-for-a-cluster"></a>Povolit nebo zakázat RDP přístupu pro clusteru

    azure hdinsight cluster enable-rdp-access [options] <Cluster Name> <rdpUserName> <rdpPassword> <rdpExpiryDate>
    azure hdinsight cluster disable-rdp-access [options] <Cluster Name>


##<a name="next-steps"></a>Další kroky
V tomto článku se naučili provádět jiné HDInsight obrázku pro správu. Další informace naleznete v následujících článcích:

* [Spravovat pomocí portálu Azure HDInsight] [hdinsight-admin-portal]
* [Spravovat pomocí prostředí PowerShell Azure HDInsight] [hdinsight-admin-powershell]
* [Začínáme s Azure HDInsight] [hdinsight-get-started]
* [Jak používat rozhraní příkazového řádku Azure] [azure-command-line-tools]


[azure-command-line-tools]: ../xplat-cli-install.md
[azure-create-storageaccount]: ../storage-create-storage-account.md
[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/


[hdinsight-admin-portal]: hdinsight-administer-use-management-portal.md
[hdinsight-admin-powershell]: hdinsight-administer-use-powershell.md
[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md

[image-cli-account-download-import]: ./media/hdinsight-administer-use-command-line/HDI.CLIAccountDownloadImport.png
[image-cli-clustercreation]: ./media/hdinsight-administer-use-command-line/HDI.CLIClusterCreation.png
[image-cli-clustercreation-config]: ./media/hdinsight-administer-use-command-line/HDI.CLIClusterCreationConfig.png
[image-cli-clusterlisting]: ./media/hdinsight-administer-use-command-line/HDI.CLIListClusters.png "Seznam a zobrazit clusterů"
