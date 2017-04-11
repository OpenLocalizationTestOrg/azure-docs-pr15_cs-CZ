<properties 
   pageTitle="Konfigurace HBase replikace mezi dvěma datacentrech | Microsoft Azure" 
   description="Zjistěte, jak nakonfigurovat HBase replikace mezi dvěma datacentrech a o případy použití replikace obrázku." 
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
   ms.date="07/25/2016"
   ms.author="jgao"/>

# <a name="configure-hbase-geo-replication-in-hdinsight"></a>Konfigurace HBase geo replikace v HDInsight

> [AZURE.SELECTOR]
- [Konfigurace připojení k síti VPN](../hdinsight-hbase-geo-replication-configure-vnets.md)
- [Konfigurace DNS](hdinsight-hbase-geo-replication-configure-dns.md)
- [Konfigurace HBase replikace](hdinsight-hbase-geo-replication.md) 
 
Zjistěte, jak konfigurovat HBase replikace mezi dvěma datacentrech. Některé případy použití clusteru replikace zahrnují:

- Zálohování a havárie obnovení
- Agregace dat
- Distribuce zeměpisných dat
- Požití online data společně s technologie pro analýzu dat v režimu offline

Replikace clusteru používá metodologii služby nabízených zdroje. HBase clusteru může být zdrojem, cíl, nebo můžete splnění obou role najednou. Replikace je asynchronní a cíl replikace případné konzistence. Přijaté zdroji upravit na skupinu sloupce s opakováním povoleno, dané úpravy rozšíří do všech clusterů cíl. Když replikace dat z jednoho obrázku do jiného zdroje obrázku a všechny clusterů, které jste už spotřebované množství dat sledují nechcete, aby replikace smyčky. Další informace najdete v tomto kurzu nakonfigurujete zdrojových a cílových replikace.  Další topologií obrázku najdete v článku [Apache HBase referenční příručka](http://hbase.apache.org/book.html#_cluster_replication).

Toto je třetí součástí řady:

- [Konfigurace připojení k síti VPN mezi dvěma virtuální sítěmi][hdinsight-hbase-replication-vnet]
- [Konfigurace DNS pro virtuální sítě][hdinsight-hbase-replication-dns]
- Konfigurace HBase geo replikace (Tento kurz)

Následující obrázek znázorňuje dva virtuálních sítí a připojení k síti jste vytvořili v [článku Konfigurace připojení k síti VPN mezi dvěma virtuální sítěmi] [ hdinsight-hbase-geo-replication-vnet] a [Konfigurace DNS pro virtuální sítě][hdinsight-hbase-replication-dns]: 

![HDInsight HBase replikace virtuální síťového diagramu][img-vnet-diagram]

## <a id="prerequisites"></a>Zjistit předpoklady pro

Před zahájením tohoto kurzu, musíte mít takto:

- **Azure předplatného**. Viz [získání Azure bezplatnou zkušební verzi](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).

- **Pracoviště se Azure Powershellu**.

    K provedení skriptů Powershellu, musíte Azure PowerShell spustit jako správce a nastavit zásady spuštění *RemoteSigned*. V tématu Použití rutinu Set-zásady spouštění.
    
    [AZURE.INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]

- **Dva Azure virtuální sítě s připojení k síti VPN a nakonfigurovány služby DNS**.  Pokyny najdete v tématu [Konfigurace připojení VPN mezi dvěma Azure virtuální sítěmi][hdinsight-hbase-replication-vnet]a [Konfigurace DNS mezi dvěma Azure virtuální sítěmi][hdinsight-hbase-replication-dns].


    Před spuštěním skriptů Powershellu, zkontrolujte, jestli že jste připojeni k předplatnému Azure pomocí následující rutinu:

        Add-AzureAccount

    Pokud máte víc předplatných Azure, můžete nastavit aktuálního předplatného pomocí následující rutinu:

        Select-AzureSubscription <AzureSubscriptionName>



## <a name="provision-hbase-clusters-in-hdinsight"></a>Zřízení HBase clusterů HDInsight

V [článku Konfigurace připojení VPN mezi dvěma Azure virtuální sítěmi][hdinsight-hbase-replication-vnet], nevytvoříte virtuální sítě Europe datacentra a virtuální sítě v datovém centru USA. Dva virtuální sítě připojeni přes VPN. V této relaci bude zřízení HBase obrázku ve všech virtuální sítě. Dál v tomto kurzu se uděláte jednu z clusterů HBase replikovat HBase clusteru.

Klasický portálu Azure, které nejsou podporovány zřizovací HDInsight clusterů s možností vlastní konfigurace. Například *hbase.replication* nastavena na *hodnotu true*. Pokud jste nastavili hodnota v souboru konfigurace po zřízení clusteru, ztratí se nastavení po clusteru je právě reimaged. Další informace najdete v článku [poskytnutí Hadoop clusterů HDInsight][hdinsight-provision]. Jednu z možností zřízení HDInsight obrázku s vlastní možnosti používá Azure Powershellu.


**Zřízení HBase obrázku v Evropské unie Contoso VNet** 

1. V počítači otevřete ISE v prostředí Windows PowerShell.
2. Nastavení proměnných začínají skriptu a potom následujícím způsobem.

        # create hbase cluster with replication enabled
        
        $azureSubscriptionName = "[AzureSubscriptionName]"
        
        $hbaseClusterName = "Contoso-HBase-EU" # This is the HBase cluster name to be used.
        $hbaseClusterSize = 1   # You must provision a cluster that contains at least 3 nodes for high availability of the HBase services.
        $hadoopUserLogin = "[HadoopUserName]"
        $hadoopUserpw = "[HadoopUserPassword]"
        
        $vNetName = "Contoso-VNet-EU"  # This name must match your Europe virtual network name.
        $subNetName = 'Subnet-1' # This name must match your Europe virtual network subnet name.  The default name is "Subnet-1".
        
        $storageAccountName = 'ContosoStoreEU'  # The script will create this storage account for you.  The storage account name doesn't support hyphens. 
        $storageAccountName = $storageAccountName.ToLower() #Storage account name must be in lower case.
        $blobContainerName = $hbaseClusterName.ToLower()  #Use the cluster name as the default container name.
        
        #connect to your Azure subscription
        Add-AzureAccount 
        Select-AzureSubscription $azureSubscriptionName
        
        # Create a storage account used by the HBase cluster
        $location = Get-AzureVNetSite -VNetName $vNetName | %{$_.Location} # use the virtual network location
        New-AzureStorageAccount -StorageAccountName $storageAccountName -Location $location
        
        # Create a blob container used by the HBase cluster
        $storageAccountKey = Get-AzureStorageKey -StorageAccountName $storageAccountName | %{$_.Primary}
        $storageContext = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageAccountKey
        New-AzureStorageContainer -Name $blobContainerName -Context $storageContext
        
        # Create provision configuration object
        $hbaseConfigValues = new-object 'Microsoft.WindowsAzure.Management.HDInsight.Cmdlet.DataObjects.AzureHDInsightHBaseConfiguration'
            
        $hbaseConfigValues.Configuration = @{ "hbase.replication"="true" } #this modifies the hbase-site.xml file
        
        # retrieve vnet id based on vnetname
        $vNetID = Get-AzureVNetSite -VNetName $vNetName | %{$_.id}
        
        $config = New-AzureHDInsightClusterConfig `
                        -ClusterSizeInNodes $hbaseClusterSize `
                        -ClusterType HBase `
                        -VirtualNetworkId $vNetID `
                        -SubnetName $subNetName `
                    | Set-AzureHDInsightDefaultStorage `
                        -StorageAccountName $storageAccountName `
                        -StorageAccountKey $storageAccountKey `
                        -StorageContainerName $blobContainerName `
                    | Add-AzureHDInsightConfigValues `
                        -HBase $hbaseConfigValues
        
        # provision HDInsight cluster
        $hadoopUserPassword = ConvertTo-SecureString -String $hadoopUserpw -AsPlainText -Force     
        $credential = New-Object System.Management.Automation.PSCredential($hadoopUserLogin,$hadoopUserPassword)
        
        $config | New-AzureHDInsightCluster -Name $hbaseClusterName -Location $location -Credential $credential



**Zřízení HBase clusteru Contoso VNet USA** 

- Používáte stejné skript se na tyto hodnoty:

        $hbaseClusterName = "Contoso-HBase-US" # This is the HBase cluster name to be used.
        $vNetName = "Contoso-VNet-US"  # This name must match your Europe virtual network name.
        $storageAccountName = 'ContosoStoreUS'  

    Vzhledem k tomu, že už jste připojili ke svému účtu Azure, nemusíte už spustit následující comlets:

        Add-AzureAccount 
        Select-AzureSubscription $azureSubscriptionName




## <a name="configure-dns-conditional-forwarder"></a>Konfigurace DNS podmíněné předávání

[Konfigurace]DNS pro virtuální sítě[hdinsight-hbase-replication-dns], jste nakonfigurovali serverů DNS pro obě sítě. Clusterů HBase mít přípony jinou doménu. Ano bude potřeba nakonfigurovat další podmíněná předávání.

Abyste mohli nakonfigurovat podmíněné předávání, je potřeba vědět přípony domén dvou clusterů HBase. 

**K vyhledání přípony domén dvou HBase clusterů**

1. RDP do **Contoso HBase EU**.  Pokyny najdete v tématu [Správa Hadoop clusterů HDInsight pomocí portálu klasické Azure][hdinsight-manage-portal]. Je skutečně headnode0 clusteru.
2. Otevřete konzoly Windows PowerShell nebo příkazový řádek.
3. Spuštění **příkazu ipconfig**a zapište si **přípona DNS připojení**.
4. Není zavřete RDP relace.  Budete potřebovat později a otestovat domény překlad.
5. Opakujte stejný postup zjistěte **přípona DNS připojení** **Contoso-HBase-US**.


**Konfigurace DNS pro předávání**
 
1.  RDP do **Evropské unie Contoso DNS**. 
2.  Klepněte na klávesu Windows v levém dolním.
2.  Klikněte na **Nástroje pro správu**.
3.  Klikněte na **DNS**.
4.  V levém podokně rozbalte **kódu oznámení o doručení**, **Contoso DNS EU**.
5.  Klikněte pravým tlačítkem **Podmíněné předávání**a potom klikněte na **Nové podmíněné předávání**. 
5.  Zadejte následující informace:
    - **DNS domény**: Zadejte přípony DNS spojené HBase Contoso. Příklad: Contoso-HBase US.f5.internal.cloudapp.net.
    - **IP adresy hlavní servery**: Zadejte 10.2.0.4, což je Contoso-DNS-USA na IP adresu. Zkontrolujte, zda IP. Různých IP adresa může být vašeho serveru DNS.
6.  Stiskněte klávesu **ENTER**a potom klikněte na **OK**.  Teď budete moct vyřešit Contoso-DNS-USA na IP adresu z Evropské unie Contoso DNS.
7.  Opakujte postup pro přidání DNS pro předávání podmíněné ke službě DNS v počítači virtuální Contoso DNS spojené s následující hodnoty:
    - **DNS domény**: Zadejte přípony DNS Contoso HBase EU. 
    - **IP adresy hlavní servery**: Zadejte 10.2.0.4, což je Contoso-DNS-Evropské unie IP adresa.

**Chcete-li otestovat překlad názvů domén**

1. Přechod do okna Contoso HBase EU RDP
2. Otevřete příkazový řádek.
3. Spuštění příkazu ping:

        ping headnode0.[DNS suffix of Contoso-HBase-US]

    Protokol korekce barev zapnuté pracovníka uzlů clusterů HBase

4. Nechejte RDP relace. Přesto musíte ho dál v tomto kurzu.
5. Opakujte stejný postup ping headnode0 Contoso-HBase-Evropské unie z spojené HBase Contoso.

>[AZURE.IMPORTANT] DNS musí pracovat před můžete přejít k dalším krokům.

## <a name="enable-replication-between-hbase-tables"></a>Povolení replikace mezi HBase tabulkami

Teď můžete vytvořit tabulku HBase vzorku, povolení replikace a otestujte jej s daty. Ukázková tabulka bude používat obsahuje dva sloupce rodiny: osobní a Office. 

V tomto kurzu vytvoříte Europe HBase obrázku jako zdroje obrázku a americké HBase obrázku jako cíle obrázku.

Vytvořte tabulku HBase se stejnými názvy a rodiny sloupec ve zdrojovém a cílovém tak, aby cílové clusteru zná místa uložení data, která se zobrazí. Další informace o použití HBase prostředí, najdete v článku [Začínáme s Apache HBase v HDInsight][hdinsight-hbase-get-started].

**Vytvoření tabulky HBase na Contoso HBase EU**

1. Přepnutí do okna RDP **Contoso HBase EU** .
2. Ze stolního počítače klikněte na **přepínač příkazového řádku Hadoop**.
2. Změna složky k adresáři domácí HBase:

        cd %HBASE_HOME%\bin
3. Otevřete HBase prostředí:

        hbase shell
4. Vytvoření tabulky HBase:

        create 'Contacts', 'Personal', 'Office'
5. Nechejte relace RDP ani okně Hadoop příkazového řádku. Přesto musíte je dál v tomto kurzu.
    
**Vytvoření tabulky aplikace HBase na Contoso-HBase-US**

- Opakujte stejný postup vytvoření stejné tabulky na Contoso-HBase-US.


**Chcete-li přidat Contoso-HBase-US jako partner replikace**

1. Přepnutí do okna RDP **Contso HBase_EU** .
2. V okně prostředí HBase přidáte cíl obrázku (Contoso spojené HBase) jako partnera, například:

        add_peer '1', 'zookeeper0.contoso-hbase-us.d4.internal.cloudapp.net,zookeeper1.contoso-hbase-us.d4.internal.cloudapp.net,zookeeper2.contoso-hbase-us.d4.internal.cloudapp.net:2181:/hbase'

    Ve vzorku je příponu domény *společnosti contoso hbase us.d4.internal.cloudapp.net*. Potřebujete aktualizovat ho, aby odpovídal vaše přípona doménu US HBase clusteru. Zkontrolujte, jestli že je bez mezer mezi názvy hostitelů.

**Konfigurace každý sloupec řady replikovat clusteru zdroje**

1. V okně prostředí HBase relace RDP **Contso HBase EU** konfigurace každý sloupec řady replikovat:

        disable 'Contacts'
        alter 'Contacts', {NAME => 'Personal', REPLICATION_SCOPE => '1'}
        alter 'Contacts', {NAME => 'Office', REPLICATION_SCOPE => '1'}
        enable 'Contacts'

**Chcete-li odeslat data do tabulky HBase hromadně**

Ukázka datového souboru obsahuje nahraná na kontejner veřejné objektů Blob Azure pomocí následující adresu URL:

        wasbs://hbasecontacts@hditutorialdata.blob.core.windows.net/contacts.txt

Obsah souboru:

        8396    Calvin Raji 230-555-0191    5415 San Gabriel Dr.
        16600   Karen Wu    646-555-0113    9265 La Paz
        4324    Karl Xie    508-555-0163    4912 La Vuelta
        16891   Jonathan Jackson    674-555-0110    40 Ellis St.
        3273    Miguel Miller   397-555-0155    6696 Anchor Drive
        3588    Osarumwense Agbonile    592-555-0152    1873 Lion Circle
        10272   Julia Lee   870-555-0110    3148 Rose Street
        4868    Jose Hayes  599-555-0171    793 Crawford Street
        4761    Caleb Alexander 670-555-0141    4775 Kentucky Dr.
        16443   Terry Chander   998-555-0171    771 Northridge Drive

Můžete odeslat stejný datový soubor do HBase clusteru a importovat data z tohoto umístění.

1. Přepnutí do okna RDP **Contoso HBase EU** .
2. Ze stolního počítače klikněte na **přepínač příkazového řádku Hadoop**.
3. Změnit složku, kterou chcete HBase adresáři:

        cd %HBASE_HOME%\bin

4. odeslání dat:

        hbase org.apache.hadoop.hbase.mapreduce.ImportTsv -Dimporttsv.columns="HBASE_ROW_KEY,Personal:Name, Personal:HomePhone, Office:Address" -Dimporttsv.bulk.output=/tmpOutput Contacts wasbs://hbasecontacts@hditutorialdata.blob.core.windows.net/contacts.txt

        hbase org.apache.hadoop.hbase.mapreduce.LoadIncrementalHFiles /tmpOutput Contacts


## <a name="verify-that-data-replication-is-taking-place"></a>Přesvědčte se, že data replikace probíhá v

Můžete ověřit, že replikace probíhá v tak, že prohledáte tabulek z obou clusterů pomocí následujících příkazů HBase prostředí:

        Scan 'Contacts'


## <a name="next-steps"></a>Další kroky

V tomto kurzu se naučili konfigurovat HBase replikace mezi dvěma datacentrech. Další informace o HDInsight a HBase, najdete tady:

- [HDInsight služby stránky](https://azure.microsoft.com/services/hdinsight/)
- [HDInsight si přečtěte následující dokumentaci](https://azure.microsoft.com/documentation/services/hdinsight/)
- [Začínáme s Apache HBase v HDInsight][hdinsight-hbase-get-started]
- [Přehled HDInsight HBase][hdinsight-hbase-overview]
- [Zřízení HBase clusterů virtuální síti Azure][hdinsight-hbase-provision-vnet]
- [Analyzovat data v reálném myšlenkou Twitter s HBase][hdinsight-hbase-twitter-sentiment]
- [Analýza dat senzor s bouře a HBase v HDInsight (Hadoop)][hdinsight-sensor-data]

[hdinsight-hbase-geo-replication-vnet]: hdinsight-hbase-geo-replication-configure-vnets.md
[hdinsight-hbase-geo-replication-dns]: ../hdinsight-hbase-geo-replication-configure-VNet.md


[img-vnet-diagram]: ./media/hdinsight-hbase-geo-replication/HDInsight.HBase.Replication.Network.diagram.png

[powershell-install]: powershell-install-configure.md
[hdinsight-hbase-get-started]: hdinsight-hbase-tutorial-get-started.md
[hdinsight-manage-portal]: hdinsight-administer-use-management-portal.md
[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-hbase-replication-vnet]: hdinsight-hbase-geo-replication-configure-vnets.md
[hdinsight-hbase-replication-dns]: hdinsight-hbase-geo-replication-configure-dns.md
[hdinsight-hbase-twitter-sentiment]: hdinsight-hbase-analyze-twitter-sentiment.md
[hdinsight-sensor-data]: hdinsight-storm-sensor-data-analysis.md
[hdinsight-hbase-overview]: hdinsight-hbase-overview.md
[hdinsight-hbase-provision-vnet]: hdinsight-hbase-provision-vnet.md
