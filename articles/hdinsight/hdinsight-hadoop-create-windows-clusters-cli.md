<properties
   pageTitle="Vytvoření serveru s Windows Hadoop clusterů v pomocí rozhraní příkazového řádku Azure HDInsight"
    description="Naučte se vytvářet clusterů pro Azure Hdinsightu pomocí rozhraní příkazového řádku Azure."
   services="hdinsight"
   documentationCenter=""
   tags="azure-portal"
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

# <a name="create-windows-based-hadoop-clusters-in-hdinsight-using-azure-cli"></a>Vytvoření serveru s Windows Hadoop clusterů v pomocí rozhraní příkazového řádku Azure HDInsight

[AZURE.INCLUDE [selector](../../includes/hdinsight-selector-create-clusters.md)]

Naučte se vytvářet pomocí rozhraní příkazového řádku Azure HDInsight clusterů. Vytváření jiných clusteru nástrojů a funkcí, a klikněte na kartu horní části této stránky nebo si přečtěte [způsobů vytvoření obrázku](hdinsight-provision-clusters.md#cluster-creation-methods).

##<a name="prerequisites"></a>Požadavky:

[AZURE.INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]


Než začnete pokyny v tomto článku, musíte mít takto:

- **Azure předplatného**. Viz [získání Azure bezplatnou zkušební verzi](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
- **Azure rozhraní příkazového řádku**.

    [AZURE.INCLUDE [use-latest-version](../../includes/hdinsight-use-latest-cli.md)] 

### <a name="access-control-requirements"></a>Požadavky na řízení přístupu

[AZURE.INCLUDE [access-control](../../includes/hdinsight-access-control-requirements.md)]

##<a name="connect-to-azure"></a>Připojení k Azure

Připojení k Azure zadejte následující příkaz:

    azure login

Další informace o ověřování pomocí pracovního nebo školního účtu najdete v článku [připojení k předplatnému Azure z Azure rozhraní příkazového řádku](../xplat-cli-connect.md).

Přepněte do režimu ARM použijte tento příkaz:

    azure config mode arm

Potřebujete pomoct, použijte přepínač **-h** .  Příklad:

    azure hdinsight cluster create -h

##<a name="create-clusters"></a>Vytvoření clusterů

Než budete moct vytvářet HDInsight clusteru musí mít Azure zdroje Management (ARM) a účet úložiště objektů Blob Azure. K vytvoření HDInsight clusteru, je nutné zadat takto:

- **Pole Skupina zdroje Azure**: je třeba vytvořit účet jezera analýzy dat A ve skupině Azure prostředků. Azure správce prostředků umožňuje práce se zdroji v aplikaci jako skupinu. Můžete nasadit, aktualizace nebo odstranění všechny zdroje pro aplikaci v operaci jediné, koordinovaný.

    Chcete-li zobrazit zdroje skupiny ve vašem předplatném:

        azure group list

    Vytvoření nové skupiny prostředků:

        azure group create -n "<Resource Group Name>" -l "<Azure Location>"

- **Název clusteru HDInsight**

- **Umístění**: jeden z Azure datacentrech, které podporuje clusterů HDInsight. Seznam podporovaných umístění najdete v tématu [ceny HDInsight](https://azure.microsoft.com/pricing/details/hdinsight/).

- **Výchozí úložiště účet**: kontejneru úložiště objektů Blob Azure HDInsight používá jako výchozí systém souborů. Vyžaduje se účet Azure úložiště, než budete moct vytvářet HDInsight obrázku.

    Vytvořte nový účet Azure úložiště:

        azure storage account create "<Azure Storage Account Name>" -g "<Resource Group Name>" -l "<Azure Location>" --type LRS

    > [AZURE.NOTE]Účet úložiště musí být použít pro společné umísťování s HDInsight v datovém centru.
    > Typ účtu úložiště nelze ZRS, protože ZRS, které nejsou podporovány tabulky.

    Informace o vytváření účet Azure úložiště pomocí portálu Azure najdete v tématu [vytvoření, spravovat nebo odstraněním účtu úložiště] [azure – vytvoření storageaccount].

    Pokud už máte účet úložiště, ale nebudou vědět, název účtu a klíč účtu, můžete získat informace následující příkazy:

        -- Lists Storage accounts
        azure storage account list
        -- Shows a Storage account
        azure storage account show "<Storage Account Name>"
        -- Lists the keys for a Storage account
        azure storage account keys list "<Storage Account Name>" -g "<Resource Group Name>"

    Podrobné získávání informací o pomocí portálu Azure naleznete v části "Spravovat svůj účet úložiště" [účty adresářové služby Azure o úložiště](../storage/storage-create-storage-account#manage-your-storage-account).

- **(Volitelné) výchozí Blob kontejner**: vytvoří příkaz **create azure hdinsight clusteru** kontejneru Pokud neexistuje. Pokud budete chtít vytvořit kontejner předem, můžete tento příkaz:

    Vytvoření kontejneru Azure úložiště – název účtu "<Storage Account Name>" – klíč účtu <Storage Account Key> [Název_kontejneru]

Až budete mít účet úložiště počítat, jste připraveni k vytvoření clusteru:


    azure hdinsight cluster create -g <Resource Group Name> -c <HDInsight Cluster Name> -l <Location> --osType <Windows | Linux> --version <Cluster Version> --clusterType <Hadoop | HBase | Spark | Storm> --workerNodeCount 2 --headNodeSize Large --workerNodeSize Large --defaultStorageAccountName <Azure Storage Account Name>.blob.core.windows.net --defaultStorageAccountKey "<Default Storage Account Key>" --defaultStorageContainer <Default Blob Storage Container> --userName admin --password "<HTTP User Password>" --rdpUserName <RDP Username> --rdpPassword "<RDP User Password" --rdpAccessExpiry "03/01/2016"


##<a name="create-clusters-using-configuration-files"></a>Vytvoření clusterů pomocí konfigurace souborů
Obvykle vytvoření clusteru HDInsight, spustíte úlohy na něj a potom odstraňte clusteru snížit náklady. Rozhraní příkazového řádku vám nabídne možnost při ukládání konfigurace do souboru, takže můžete znovu použít ji pokaždé, když vytvoříte clusteru.  

    azure hdinsight config create [options ] <Config File Path> <overwirte>
    azure hdinsight config add-config-values [options] <Config File Path>
    azure hdinsight config add-script-action [options] <Config File Path>

Příklad: Vytvoření konfigurační soubor, který obsahuje akci skript ke spuštění při vytváření clusteru.

    azure hdinsight config create "C:\myFiles\configFile.config"
    azure hdinsight config add-script-action --configFilePath "C:\myFiles\configFile.config" --nodeType HeadNode --uri <Script Action URI> --name myScriptAction --parameters "-param value"
    azure hdinsight cluster create --configurationPath "C:\myFiles\configFile.config"

##<a name="create-clusters-with-script-action"></a>Vytvoření clusterů pomocí skriptu akce

Vytvoření konfigurační soubor, který obsahuje akci skript ke spuštění při vytváření clusteru.

    azure hdinsight config create "C:\myFiles\configFile.config"
    azure hdinsight config add-script-action --configFilePath "C:\myFiles\configFile.config" --nodeType HeadNode --uri <scriptActionURI> --name myScriptAction --parameters "-param value"

Vytvoření clusteru s akci skriptu

    azure hdinsight cluster create -g myarmgroup01 -l westus -y Windows --clusterType Hadoop --version 3.2 --defaultStorageAccountName mystorageaccount --defaultStorageAccountKey <defaultStorageAccountKey> --defaultStorageContainer mycontainer --userName admin --password <clusterPassword> --sshUserName sshuser --sshPassword <sshPassword> --workerNodeCount 1 –configurationPath "C:\myFiles\configFile.config" myNewCluster01


Obecné skript akce informace najdete v tématu [přizpůsobení HDInsight clusterů pomocí skriptu akce (Linux)](hdinsight-hadoop-customize-cluster.md).


## <a name="create-clusters-using-arm-templates"></a>Vytvoření clusterů pomocí šablon ARM

Rozhraní příkazového řádku slouží k vytvoření clusterů tak, že zavoláte ARM šablony. V tématu [nasazení s Azure rozhraní příkazového řádku](hdinsight-hadoop-create-windows-clusters-arm-templates.md#deploy-with-azure-cli).

## <a name="see-also"></a>Viz taky

- [Začínáme s Azure HDInsight](hdinsight-hadoop-linux-tutorial-get-started.md) – zjistěte, jak začít pracovat s HDInsight obrázku
- [Odeslání Hadoop úlohy programově](hdinsight-submit-hadoop-jobs-programmatically.md) – zjistěte, jak můžou programově odeslat úlohy HDInsight
- [Správa Hadoop clusterů pomocí rozhraní příkazového řádku Azure HDInsight](hdinsight-administer-use-command-line.md)
- [Použití Azure rozhraní příkazového řádku pro Mac a Linux, Windows Azure služby Správa](../virtual-machines-command-line-tools.md)
