<properties
    pageTitle="Vytvoření Hadoop, HBase nebo bouře clusterů na Linux v HDInsight pomocí rozhraní příkazového řádku Azure různé platformy | Microsoft Azure"
    description="Naučte se vytvářet clusterů na základě Linux HDInsight pomocí rozhraní příkazového řádku Azure různé platformy, správce prostředků Azure šablony a rozhraní REST API Azure. Zadejte typ obrázku (Hadoop HBase a bouře) nebo pomocí skriptů nainstalovat vlastní součásti."
    services="hdinsight"
    documentationCenter=""
    authors="Blackmist"
    manager="jhubbard"
    editor="cgronlun"
    tags="azure-portal"/>

<tags
    ms.service="hdinsight"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="big-data"
    ms.date="09/20/2016"
    ms.author="larryfr"/>

#<a name="create-linux-based-clusters-in-hdinsight-using-the-azure-cli"></a>Vytvoření na základě Linux clusterů v pomocí rozhraní příkazového řádku Azure HDInsight

[AZURE.INCLUDE [selector](../../includes/hdinsight-selector-create-clusters.md)]

Rozhraní příkazového řádku Azure je různé platformy příkazového řádku nástroj, který umožňuje spravovat služby Azure. Jej lze použít, spolu s Azure zdrojů správy šablony k vytvoření HDInsight clusteru, spolu s účty přidružené úložiště a další služby.

Azure šablony řízení zdrojů jsou JSON dokumenty, které popisují __pole Skupina zdroje__ a všechny zdroje v něm (například HDInsight.) Tento přístup založena šablona umožňuje definovat všechny zdroje, které potřebujete pro HDInsight do jedné šablony. Také umožňuje spravovat změny do skupiny jako celek prostřednictvím __nasazení__, který použít změny celé skupiny najednou.

Kroky v tomto dokumentu projděte si proces vytvoření nového clusteru HDInsight pomocí rozhraní příkazového řádku Azure a šablony.

> [AZURE.IMPORTANT] Kroky v tomto dokumentu pomocí výchozího počtu uzlů pracovníka (4) pro HDInsight obrázku. Pokud máte v plánu uzlech pracovník víc než 32 (při vytváření obrázku nebo změna měřítka obrázku), je nutné vybrat velikostí hlavy uzel s 14 GB paměti ram a alespoň na úrovni 8 jádra.
>
> Další informace o velikosti uzel a související náklady najdete v článku [ceny HDInsight](https://azure.microsoft.com/pricing/details/hdinsight/).

##<a name="prerequisites"></a>Zjistit předpoklady pro

[AZURE.INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

- **Azure předplatného**. Viz [získání Azure bezplatnou zkušební verzi](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
- __Azure rozhraní příkazového řádku__. Kroky v tomto dokumentu byly poslední testováno s Azure rozhraní příkazového řádku verze 0.10.1.

    [AZURE.INCLUDE [use-latest-version](../../includes/hdinsight-use-latest-cli.md)] 


### <a name="access-control-requirements"></a>Požadavky na řízení přístupu

[AZURE.INCLUDE [access-control](../../includes/hdinsight-access-control-requirements.md)]

##<a name="log-in-to-your-azure-subscription"></a>Přihlaste se k předplatnému Azure

Postupujte podle kroků uvedených v části [připojit ke předplatné Azure z Azure rozhraní příkazového řádku (Azure rozhraní příkazového řádku)](../xplat-cli-connect.md) a připojte k vašemu předplatnému metodou __přihlášení__ .

##<a name="create-a-cluster"></a>Vytvoření clusteru

Podle těchto kroků se provádí z příkazového řádku, kostra domu nebo terminálové relace po instalaci a konfiguraci Azure rozhraní příkazového řádku.

1. Zadejte následující příkaz ověření k předplatnému Azure:

        azure login

    Zobrazí se výzva k zadání jména a hesla. Pokud máte víc předplatných Azure, `azure account set <subscriptionname>` nastavovaná předplatné, použijte příkazy Azure rozhraní příkazového řádku.

3. Přepněte do režimu správce prostředků Azure pomocí následujícího příkazu:

        azure config mode arm

4. Vytvoření skupiny prostředků. Tato skupina bude obsahovat clusteru HDInsight a problémů účtu úložiště.

        azure group create groupname location
        
    * Nahraďte jedinečný název pro skupinu __název skupiny__ . 
    * Nahraďte zeměpisná oblast, kterou chcete vytvořit skupinu v __umístění__ . 
    
        Seznam platná umístění, použít `azure location list` příkaz a potom použijte jeden z umístění ve sloupci __název__ .

5. Vytvoření účtu úložiště. Tento účet úložiště se použije jako výchozí úložiště pro clusteru HDInsight.

        azure storage account create -g groupname --sku-name RAGRS -l location --kind Storage storagename
        
     * Nahraďte __název skupiny__ na název skupiny vytvořili v předchozím kroku.
     * Nahraďte __umístění__ na stejném místě použili v předchozím kroku. 
     * Nahraďte __storagename__ jedinečný název účtu úložiště.
     
     > [AZURE.NOTE] Další informace o parametry použité v tomto příkazu `azure storage account create -h` se dá zobrazit Nápověda pro tento příkaz.

5. Načtení klávesu použitých při přístupu k účtu úložiště.

        azure storage account keys list -g groupname storagename
        
    * Nahraďte __název skupiny__ na název skupiny zdrojů.
    * Nahraďte __storagename__ název účtu úložiště.
    
    Data, která je vrácena uložte hodnotu __klíč__ pro __klíč1__.

6. Vytvoření clusteru HDInsight.

        azure hdinsight cluster create -g groupname -l location -y Linux --clusterType Hadoop --defaultStorageAccountName storagename.blob.core.windows.net --defaultStorageAccountKey storagekey --defaultStorageContainer clustername --workerNodeCount 2 --userName admin --password httppassword --sshUserName sshuser --sshPassword sshuserpassword clustername

    * Nahraďte __název skupiny__ na název skupiny zdrojů.

    * Nahraďte __Hadoop__ typ obrázku, který chcete vytvořit. Například `Hadoop`, `HBase`, `Storm` nebo `Spark`.

        > [AZURE.IMPORTANT] HDInsight clusterů mohou mít různé typy, které odpovídají pracovní zátěž nebo technologii clusteru optimalizovaných pro. Neexistuje žádná podporované metoda k vytvoření obrázku, který kombinuje více typů, například bouře a HBase na jednoho obrázku. 

    * Nahraďte __umístění__ na stejném místě v předchozích krocích použili.

    * Nahraďte __storagename__ název účtu úložiště.

    * Nahraďte __klíč úložiště__ key získaný v předchozím kroku. 

    * Pro `--defaultStorageContainer` parametr, použijte stejný název jako se používají pro clusteru.

    * Nahraďte __Správce__ a __httppassword__ jméno a heslo, které chcete použít při přístupu k clusteru prostřednictvím HTTPS.

    * Nahraďte __sshuser__ a __sshuserpassword__ uživatelské jméno a heslo, které chcete použít při přístupu k clusteru pomocí SSH

    Může trvat několik minut dokončení procesu vytvoření obrázku. Obvykle asi 15.

##<a name="next-steps"></a>Další kroky

Teď úspěšně jste vytvořili HDInsight clusteru pomocí rozhraní příkazového řádku Azure, použijte následující postup, jak pracovat s svůj cluster:

###<a name="hadoop-clusters"></a>Hadoop clusterů

* [Použití podregistru s HDInsight](hdinsight-use-hive.md)
* [Použití Prasátko s HDInsight](hdinsight-use-pig.md)
* [Použití MapReduce s HDInsight](hdinsight-use-mapreduce.md)

###<a name="hbase-clusters"></a>HBase clusterů

* [Začínáme s HBase na HDInsight](hdinsight-hbase-tutorial-get-started-linux.md)
* [Zabývají vývojem aplikací Java pro HBase na HDInsight](hdinsight-hbase-build-java-maven-linux.md)

###<a name="storm-clusters"></a>Bouře clusterů

* [Můžete vyvíjet Java topologie pro bouře na HDInsight](hdinsight-storm-develop-java-topology.md)
* [Použití Python součástí bouře na HDInsight](hdinsight-storm-develop-python-topology.md)
* [Nasazení a sledovat topologií s bouře na HDInsight](hdinsight-storm-deploy-monitor-topology-linux.md)
