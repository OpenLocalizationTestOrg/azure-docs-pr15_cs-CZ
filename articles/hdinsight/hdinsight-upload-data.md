<properties
    pageTitle="Odeslání dat pro Hadoop projekty v HDInsight | Microsoft Azure"
    description="Zjistěte, jak ukládat a získání přístupu k datům Hadoop úlohy v HDInsight pomocí rozhraní příkazového řádku Azure, Průzkumníka úložišť Azure, Azure Powershellu, Hadoop příkazového řádku nebo Sqoop."
    services="hdinsight,storage"
    documentationCenter=""
    tags="azure-portal"
    authors="mumian"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/10/2016"
    ms.author="jgao"/>



#<a name="upload-data-for-hadoop-jobs-in-hdinsight"></a>Odeslání dat pro Hadoop projekty v HDInsight

Azure HDInsight poskytuje plnohodnotně vybaveného Hadoop distributed systém souborů (HDFS) přes úložiště objektů Blob Azure. Slouží jako rozšíření HDFS poskytovat bezproblémovou práci zákazníkům. Umožňuje úplné sady součástí ekosystému Hadoop pracovat přímo na data, která spravuje. Úložiště objektů Blob Azure a HDFS jsou systémy různých souborů, které jsou optimalizováno pro ukládání dat a výpočty na tato data. Informace o výhodách používání úložiště objektů Blob Azure najdete v tématu [úložiště objektů Blob Azure použití s HDInsight][hdinsight-storage].

**Zjistit předpoklady pro**

Než začnete, mějte na paměti následující požadavky:

* Clusteru služby Azure HDInsight. Pokyny najdete v tématu [Začínáme s Azure HDInsight] [ hdinsight-get-started] nebo [poskytování HDInsight clusterů][hdinsight-provision].

##<a name="why-blob-storage"></a>Proč objektů blob úložiště?

Azure HDInsight clusterů jsou obvykle nasazené spustit MapReduce úlohy a clusterů se nezobrazí po provedení těchto úloh. Uchovávání dat v HDFS clusterů po dokončení výpočty by drahé umožňuje ukládat data. Úložiště objektů Blob Azure je velmi k dispozici, vysoce scalable, vysoké kapacitu, možnost nízké náklady a ke sdílení úložiště pro data, která má být zpracovány pomocí HDInsight. Ukládání dat v objektů blob umožňuje HDInsight clusterů, které slouží k výpočtu bezpečně vydávat bez ztráty dat.

###<a name="directories"></a>Adresáře

Azure kontejnery úložiště objektů Blob ukládají data jako klíč/dvojice a bez hierarchie adresář. Ale znak "/" lze v názvu klíče ji zobrazíte, jako kdybyste je soubor uložen struktuře adresář. HDInsight tyto uvidí, jako by byly skutečné adresáře.

Například objektů blob klíč může být *input/log1.txt*. Žádné skutečné "vstupní" adresář nachází, ale kvůli stavu "/" znak v názvu klíče má vzhled cestu k souboru.

Pokud pomocí Průzkumníka Azure nástrojů se mohou být některé soubory 0 bajtů. Tyto soubory slouží dva účelům:

- Pokud existují prázdné složky, označit jako existující složku. Úložiště objektů Blob Azure je natolik inteligentní vědět, že pokud existuje objektů blob s názvem foo/panel je složku s názvem **foo**. Ale je možné jenom znamenat vyprázdnit složku s názvem **foo** tím, že tento soubor zvláštní 0 bajt na místě.

- Mají zvláštní metadata, která není potřeba systém souborů Hadoop, zejména oprávnění a vlastníci pro složky.

##<a name="command-line-utilities"></a>Nástroje příkazového řádku

Microsoft poskytuje následující nástroje pro práci s úložiště objektů Blob Azure:

| Nástroj | Linux | OS X | Windows |
| ---- |:-----:|:----:|:-------:|
| [Azure rozhraní příkazového řádku][azurecli] | ✔ | ✔ | ✔ |
| [Azure Powershellu][azure-powershell] | | | ✔ |
| [AzCopy][azure-azcopy] | | | ✔ |
| [Příkaz Hadoop](#commandline) | ✔ | ✔ | ✔ |

> [AZURE.NOTE] Během Azure rozhraní příkazového řádku, Azure PowerShell a AzCopy můžete všechny použít z jiné než Azure, Hadoop příkaz je dostupný pouze na obrázku HDInsight a umožňuje pouze načtení dat z místního systému souborů do úložiště objektů Blob Azure.

###<a id="xplatcli"></a>Azure rozhraní příkazového řádku

Rozhraní příkazového řádku Azure je nástroj různé platformy, který umožňuje spravovat Azure služby. Odeslat data k úložišti objektů Blob Azure pomocí následujících kroků:

[AZURE.INCLUDE [use-latest-version](../../includes/hdinsight-use-latest-cli.md)]

1. [Instalace a konfigurace Azure rozhraní příkazového řádku pro Mac a Linux Windows](../xplat-cli-install.md).

2. Otevřete příkazový flám nebo jiných prostředí a můžete použít následující znaky k ověření k předplatnému Azure.

        azure login

    Po zobrazení výzvy zadejte uživatelské jméno a heslo pro vaše předplatné.

3. Zadejte tento příkaz seznam účtů úložiště u předplatného:

        azure storage account list

4. Vyberte účet úložiště, který obsahuje objektů blob, kterou chcete pracovat s a potom zadejte následující příkaz načíst klíč pro tento účet:

        azure storage account keys list <storage-account-name>

    To, měly vrátit **primárních** a **sekundárních** klíče. Hodnota **primárního** klíče zkopírujte, protože se použije v dalším krokům.

5. Načtěte seznam kontejnerů v rámci účtu úložiště objektů blob použijte tento příkaz:

        azure storage container list -a <storage-account-name> -k <primary-key>

6. Nahrávání a stahování souborů objektů blob pomocí následujících příkazů:

    * Pokud chcete nahrát soubor:

            azure storage blob upload -a <storage-account-name> -k <primary-key> <source-file> <container-name> <blob-name>

    * Stažení souboru:

            azure storage blob download -a <storage-account-name> -k <primary-key> <container-name> <blob-name> <destination-file>

> [AZURE.NOTE] Pokud budete pracovat vždy se stejným účtem úložiště, můžete nastavit následující proměnné místo zadání účtu a klíč pro každý příkaz:
>
> * **AZURE\_úložiště\_účtu**: název účtu úložiště
>
> * **AZURE\_úložiště\_přístup\_klíč**: klíč účtu úložiště

###<a id="powershell"></a>Azure Powershellu

Azure Powershellu je skriptovacích prostředí, které můžete použít k ovládání a automatizovat nasazení a řízení úloh v Azure. Informace o konfiguraci počítači spusťte Azure Powershellu najdete v tématu [instalace a konfigurace prostředí PowerShell Azure](../powershell-install-configure.md).

[AZURE.INCLUDE [use-latest-version](../../includes/hdinsight-use-latest-powershell.md)]

**Pokud chcete odeslat místní soubor k úložišti objektů Blob Azure**

1. Spusťte konzolu Azure PowerShell podle pokynů uvedených v [instalace a konfigurace prostředí PowerShell Azure](../powershell-install-configure.md).
2. Nastavení hodnoty prvních pět proměnných v tento skript:

        $resourceGroupName = "<AzureResourceGroupName>"
        $storageAccountName = "<StorageAccountName>"
        $containerName = "<ContainerName>"

        $fileName ="<LocalFileName>"
        $blobName = "<BlobName>"

        # Get the storage account key
        $storageAccountKey = (Get-AzureRmStorageAccountKey -ResourceGroupName $resourceGroupName -Name $storageAccountName)[0].Value
        # Create the storage context object
        $destContext = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageaccountkey

        # Copy the file from local workstation to the Blob container
        Set-AzureStorageBlobContent -File $fileName -Container $containerName -Blob $blobName -context $destContext

3. Vložení skriptu do konzoly Azure PowerShell spusťte ho zkopírujte soubor.

Například skriptů Powershellu, které jsou vytvořené pro práci s HDInsight, najdete v článku [Nástroje HDInsight](https://github.com/blackmist/hdinsight-tools).

###<a id="azcopy"></a>AzCopy

AzCopy je příkazového řádku nástroj, který slouží k zjednodušují přenos dat do a od účtu Azure úložiště. Můžete ho použít jako samostatný nástroj nebo zahrnutí tento nástroj v existující aplikace. [Stáhněte si AzCopy][azure-azcopy-download].

Syntaxe AzCopy je:

    AzCopy <Source> <Destination> [filePattern [filePattern...]] [Options]

Další informace najdete v tématu [AzCopy - nahrávání nebo stahování souborů pro objekty BLOB Azure][azure-azcopy].


###<a id="commandline"></a>Přepínač příkazového řádku Hadoop

Přepínač příkazového řádku Hadoop je platný pouze pro ukládání dat do úložiště objektů blob, pokud data již existuje v hlavní clusteru.

Pomocí příkazu Hadoop, musíte nejdřív připojit k headnode pomocí jedné z těchto způsobů:

* **HDInsight serveru s Windows**: [Připojit pomocí vzdálené plochy](hdinsight-administer-use-management-portal.md#connect-to-hdinsight-clusters-by-using-rdp)

* **Na základě Linux HDInsight**: připojit pomocí SSH ([příkazu SSH](hdinsight-hadoop-linux-use-ssh-unix.md#connect-to-a-linux-based-hdinsight-cluster) nebo [nátěrové](hdinsight-hadoop-linux-use-ssh-windows.md#connect-to-a-linux-based-hdinsight-cluster))

Po připojení k nahrání souboru do úložiště můžete používat tuto syntaxi.

    hadoop -copyFromLocal <localFilePath> <storageFilePath>

Například`hadoop fs -copyFromLocal data.txt /example/data/data.txt`

Protože se výchozí systém souborů pro HDInsight úložiště objektů Blob Azure, je /example/data.txt skutečně v úložišti objektů Blob Azure. Můžete také odkaz na soubor jako:

    wasbs:///example/data/data.txt

nebo

    wasbs://<ContainerName>@<StorageAccountName>.blob.core.windows.net/example/data/davinci.txt

Seznam Další Hadoop příkazy, které pracovat se soubory najdete v článku [http://hadoop.apache.org/docs/r2.7.0/hadoop-project-dist/hadoop-common/FileSystemShell.html](http://hadoop.apache.org/docs/r2.7.0/hadoop-project-dist/hadoop-common/FileSystemShell.html)

> [AZURE.WARNING] Ve HBase blokovat výchozí velikosti vyvolají zápisu dat je 256KB. Když to funguje rozhraní API HBase a rozhraní REST API, pomocí `hadoop` nebo `hdfs dfs` příkazy k zápisu dat větší než ~ 12GB za následek chybu. Naleznete v části [Výjimka úložiště pro zápis na objektů blob](#storageexception) pod další informace.

##<a name="graphical-clients"></a>Grafické klientů

Existuje několik aplikace, které obsahují grafického rozhraní pro práci s Azure úložiště. Následuje přehled některých tyto aplikace:

| Klient | Linux | OS X | Windows |
| ------ |:-----:|:----:|:-------:|
| [Microsoft Visual Studio Tools for HDInsight](hdinsight-hadoop-visual-studio-tools-get-started.md#navigate-the-linked-resources) | ✔ | ✔ | ✔ |
| [Průzkumník Azure úložiště](http://storageexplorer.com/) | ✔ | ✔ | ✔ |
| [Cloudové úložiště Studio 2](http://www.cerebrata.com/Products/CloudStorageStudio/) | | | ✔ |
| [CloudXplorer](http://clumsyleaf.com/products/cloudxplorer) | | | ✔ |
| [Azure Průzkumníka](http://www.cloudberrylab.com/free-microsoft-azure-explorer.aspx) | | | ✔ |
| [Cyberduck](https://cyberduck.io/) |  | ✔ | ✔ |

###<a name="visual-studio-tools-for-hdinsight"></a>Visual Studio Tools for HDInsight

Další informace najdete v tématu [přejděte propojených zdrojů](hdinsight-hadoop-visual-studio-tools-get-started.md#navigate-the-linked-resources).

###<a id="storageexplorer"></a>Průzkumník Azure úložiště

*Průzkumník úložišť Azure* je užitečným nástrojem pro podrobná a měnit data v objektů BLOB. Je zadarmo, otevřít zdroj nástroj, který si můžete stáhnout z [http://storageexplorer.com/](http://storageexplorer.com/). Zdrojový kód je k dispozici taky tento odkaz.

Před použitím nástroje, musíte znát kód název a účet Azure úložiště účtu. Získání tyto informace najdete v článku "jak: zobrazení, kopírovat a regenerovat úložiště přístupové klávesy" část [vytvořit, spravovat, nebo odstraněním účtu úložiště][azure-create-storage-account].  

1. Spusťte Průzkumníka Azure úložišť. Pokud to svoji práci ukládáte poprvé, ke kterým máte spustil Exploreru úložiště, zobrazí se výzva pro ___název účtu úložiště__ a __klíč účtu úložiště__. Pokud máte ho spustili před použitím na tlačítko __Přidat__ přidat nový název účtu úložiště a klíče.

    Zadejte název a klíč účtu úložiště použít HDinsight clusteru a potom vyberte __Uložit a OTEVŘETE__.

    ![HDI. AzureStorageExplorer][image-azure-storage-explorer]

5. V seznamu kontejnery nalevo od rozhraní klikněte na název kontejneru, který souvisí s svůj cluster HDInsight. Ve výchozím nastavení to je název HDInsight clusteru, ale může být různé, pokud jste zadali určitý název při vytváření clusteru.

6. Z panelu nástrojů klikněte na ikonu odeslání.

    ![Panel nástrojů se zvýrazněnou ikonou pro odeslání](./media/hdinsight-upload-data/toolbar.png)

7. Zadat soubor k nahrání a potom klikněte na **Otevřít**. Po zobrazení výzvy vyberte __Nahrát__ na odeslání souboru do kořenové úložiště kontejner. Pokud chcete nahrát soubor na určitou cestu, zadejte cestu do pole __cíl__ a potom vyberte __Uložit__.

    ![Dialogové okno odeslání souboru](./media/hdinsight-upload-data/fileupload.png)
    
    Po dokončení nahrávání souboru můžete z úlohy clusteru HDInsight.

##<a name="mount-azure-blob-storage-as-local-drive"></a>Úložiště objektů Blob Azure připojení jako místní disk

V tématu [úložišti objektů Blob Azure připojení jako místní disk](http://blogs.msdn.com/b/bigdatasupport/archive/2014/01/09/mount-azure-blob-storage-as-local-drive.aspx).

##<a name="services"></a>Služby

###<a name="azure-data-factory"></a>Azure Data Factory

Služby Azure Data Factory je plně spravované služby pro vytváření datový úložiště, zpracování dat a dat pohyb služby do potrubí výrobní optimalizovaného scalable a spolehlivé data.

Azure Data Factory slouží k přesunutí dat do úložiště objektů Blob Azure nebo vytvořit datové kanály, které přímo pomocí funkce HDInsight, například podregistru a Prasátko.

Další informace najdete v článku [si přečtěte následující dokumentaci Azure Data Factory](https://azure.microsoft.com/documentation/services/data-factory/).

###<a id="sqoop"></a>Apache Sqoop

Sqoop je nástroj určený pro přenos dat mezi Hadoop a relačních databází. Můžete ji používat k importu dat z relační databáze systému řízení (RDBMS), třeba SQL Server, MySQL nebo Oracle do systému souborů Hadoop distributed (HDFS), transformace dat v Hadoop s MapReduce nebo podregistru a potom soubor vyexportujte data zpět do RDBMS.

Další informace najdete v tématu [Použití Sqoop s HDInsight][hdinsight-use-sqoop].

##<a name="development-sdks"></a>Vývojové SDK

Úložiště objektů Blob Azure je také přístupný pomocí Azure SDK z následujících programovací jazycích:

* .NET
* Java
* Node.js
* PHP
* Python
* Skutečné

Další informace o instalaci SDK Azure najdete v článku [stažení Azure](https://azure.microsoft.com/downloads/)

## <a name="troubleshooting"></a>Řešení potíží

### <a id="storageexception"></a>Úložiště výjimku pro zápis na objektů blob

__Příznaky__: při použití `hadoop` nebo `hdfs dfs` příkazy k zápisu soubory, které jsou ~ 12 GB nebo větší HBase clusteru, může dojít k následující chybě: 

    ERROR azure.NativeAzureFileSystem: Encountered Storage Exception for write on Blob : example/test_large_file.bin._COPYING_ Exception details: null Error Code : RequestBodyTooLarge
    copyFromLocal: java.io.IOException
            at com.microsoft.azure.storage.core.Utility.initIOException(Utility.java:661)
            at com.microsoft.azure.storage.blob.BlobOutputStream$1.call(BlobOutputStream.java:366)
            at com.microsoft.azure.storage.blob.BlobOutputStream$1.call(BlobOutputStream.java:350)
            at java.util.concurrent.FutureTask.run(FutureTask.java:262)
            at java.util.concurrent.Executors$RunnableAdapter.call(Executors.java:471)
            at java.util.concurrent.FutureTask.run(FutureTask.java:262)
            at java.util.concurrent.ThreadPoolExecutor.runWorker(ThreadPoolExecutor.java:1145)
            at java.util.concurrent.ThreadPoolExecutor$Worker.run(ThreadPoolExecutor.java:615)
            at java.lang.Thread.run(Thread.java:745)
    Caused by: com.microsoft.azure.storage.StorageException: The request body is too large and exceeds the maximum permissible limit.
            at com.microsoft.azure.storage.StorageException.translateException(StorageException.java:89)
            at com.microsoft.azure.storage.core.StorageRequest.materializeException(StorageRequest.java:307)
            at com.microsoft.azure.storage.core.ExecutionEngine.executeWithRetry(ExecutionEngine.java:182)
            at com.microsoft.azure.storage.blob.CloudBlockBlob.uploadBlockInternal(CloudBlockBlob.java:816)
            at com.microsoft.azure.storage.blob.CloudBlockBlob.uploadBlock(CloudBlockBlob.java:788)
            at com.microsoft.azure.storage.blob.BlobOutputStream$1.call(BlobOutputStream.java:354)
            ... 7 more

__Příčina__: HBase na HDInsight při psaní k základnímu úložišti Azure clusterů výchozí velikost blok 256 kB. Když to funguje pro rozhraní API HBase nebo rozhraní REST API, výsledkem bude chybu při použití `hadoop` nebo `hdfs dfs` nástroje příkazového řádku.

__Řešení__: použijte `fs.azure.write.request.size` Chcete-li zadat větší velikost bloku. Můžete to uděláte na základě použití pomocí `-D` parametr. Následuje příklad použití tohoto parametru s `hadoop` příkaz:

    hadoop -fs -D fs.azure.write.request.size=4194304 -copyFromLocal test_large_file.bin /example/data

Můžete taky zvětšit přínosu `fs.azure.write.request.size` globálně pomocí Ambari. Podle těchto kroků mohou sloužit k změňte hodnotu v uživatelském rozhraní Ambari Web:

1. V prohlížeči přejděte na uživatelské rozhraní webu Ambari pro svůj cluster. Toto je https://CLUSTERNAME.azurehdinsight.net, kde __NÁZEV_CLUSTERU__ je název vašeho obrázku.

    Po zobrazení výzvy zadejte clusteru jménem a heslem správce.

2. V levé části obrazovky vyberte __HDFS__a pak vyberte kartu __Configs__ .

3. Do pole __filtr...__ zadejte `fs.azure.write.request.size`. Zobrazí se pole a aktuální hodnotu uprostřed stránky.

4. Změňte nastavení hodnoty z 262144 (256KB) na novou hodnotu. Například 4194304 (4MB).

![Obrázek změny hodnotu prostřednictvím uživatelského rozhraní Ambari webu](./media/hdinsight-upload-data/hbase-change-block-write-size.png)

Další informace o použití Ambari najdete v článku [Správa HDInsight clusterů pomocí rozhraní webových Ambari](hdinsight-hadoop-manage-ambari.md).

## <a name="next-steps"></a>Další kroky

Teď, když víte, jak k načtení dat do HDInsight, přečtěte si následující články se dozvíte, jak analýza:

* [Začínáme s Azure HDInsight][hdinsight-get-started]
* [Odeslat Hadoop úlohy programově][hdinsight-submit-jobs]
* [Použití podregistru s HDInsight][hdinsight-use-hive]
* [Použití Prasátko s HDInsight][hdinsight-use-pig]




[azure-management-portal]: https://porta.azure.com
[azure-powershell]: http://msdn.microsoft.com/library/windowsazure/jj152841.aspx

[azure-storage-client-library]: /develop/net/how-to-guides/blob-storage/
[azure-create-storage-account]: ../storage/storage-create-storage-account.md
[azure-azcopy-download]: ../storage/storage-use-azcopy.md
[azure-azcopy]: ../storage/storage-use-azcopy.md

[hdinsight-use-sqoop]: hdinsight-use-sqoop.md

[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md
[hdinsight-submit-jobs]: hdinsight-submit-hadoop-jobs-programmatically.md
[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md

[hdinsight-use-hive]: hdinsight-use-hive.md
[hdinsight-use-pig]: hdinsight-use-pig.md
[hdinsight-provision]: hdinsight-provision-clusters.md

[sqldatabase-create-configure]: ../sql-database-create-configure.md

[apache-sqoop-guide]: http://sqoop.apache.org/docs/1.4.4/SqoopUserGuide.html

[Powershell-install-configure]: ../powershell-install-configure.md

[azurecli]: ../xplat-cli-install.md


[image-azure-storage-explorer]: ./media/hdinsight-upload-data/HDI.AzureStorageExplorer.png
[image-ase-addaccount]: ./media/hdinsight-upload-data/HDI.ASEAddAccount.png
[image-ase-blob]: ./media/hdinsight-upload-data/HDI.ASEBlob.png
