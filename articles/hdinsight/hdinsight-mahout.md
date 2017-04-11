<properties
    pageTitle="Generovat doporučení pomocí Mahout a HDInsight serveru s WIndows | Microsoft Azure"
    description="Naučte se používat počítač Apache Mahout výukové knihovny generovat doporučení video se systémem Windows HDInsight (Hadoop)."
    services="hdinsight"
    documentationCenter=""
    authors="Blackmist"
    manager="jhubbard"
    editor="cgronlun"
    tags="azure-portal"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/11/2016"
    ms.author="larryfr"/>

#<a name="generate-movie-recommendations-by-using-apache-mahout-with-hadoop-in-hdinsight"></a>Generování film doporučení pomocí Apache Mahout Hadoop v HDInsight

[AZURE.INCLUDE [mahout-selector](../../includes/hdinsight-selector-mahout.md)]

Naučte se používat počítač [Apache Mahout](http://mahout.apache.org) výukové knihovny s Azure HDInsight generovat film doporučení.

> [AZURE.NOTE] Postup v tomto dokumentu vyžaduje Windows klienta a serveru s Windows HDInsight obrázku. Informace o použití Mahout z Linux, OS X nebo Unix klienta na základě Linux HDInsight clusteru najdete v tématu [Generovat film doporučení pomocí Apache Mahout se systémem Linux Hadoop v HDInsight](hdinsight-hadoop-mahout-linux-mac.md)


##<a name="learn"></a>Co se dozvíte

Mahout je [počítač výukové] [ ml] Apache Hadoop v knihovně. Mahout obsahuje algoritmy pro zpracování dat, například filtry, zařazení a clusterů. V tomto článku se bude používat modul doporučení generovat doporučení video, které jsou založeny na filmy přáteli zobrazila. Můžete taky dozvíte, jak provádět klasifikace s strukturu rozhodnutí. Tím se naučíte takto:

* K tomu Mahout úlohy pomocí prostředí Windows PowerShell

* Spuštění úlohy Mahout z příkazového řádku Hadoop

* Jak nainstalovat Mahout HDInsight 3.0 a HDInsight 2.0 clusterů

    > [AZURE.NOTE] Mahout je k dispozici s podverzí HDInsight 3.1 clusterů. Pokud používáte dřívější verzi HDInsight, najdete v článku [Instalace Mahout](#install) před pokračovat.

##<a name="prerequisites"></a>zjistit předpoklady pro

- **Na základě Windows Hadoop obrázku v HDInsight**. Informace o vytváření jednu najdete v tématu [Začínáme s používáním Hadoop v HDInsight][getstarted]
- **Pracoviště se Azure Powershellu**.

    [AZURE.INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]


##<a name="recommendations"></a>Generovat doporučení pomocí prostředí Windows PowerShell

> [AZURE.NOTE] Přestože úlohy použít v této části funguje pomocí prostředí Windows PowerShell, spoustu tříd poskytovaných s Mahout nefungují aktuálně používat Windows PowerShell a spuštěním pomocí příkazového řádku Hadoop. Seznam třídy, které nefungují používat Windows PowerShell naleznete v části [Poradce při potížích](#troubleshooting) .
>
> Příklad použití Hadoop příkazového řádku pro spuštění Mahout úloh najdete v článku [třídění dat pomocí příkazového řádku Hadoop](#classify).

Jednou z funkce, které poskytuje společnost Mahout je modul doporučení. Tento modul přijme dat ve formátu `userID`, `itemId`, a `prefValue` (předvoleb uživatele položky). Mahout můžete pak analýza spolu occurance k určení: _Uživatelé, kteří mají předvoleb pro položku také přednost u těchto položek_. Mahout pak určuje uživatelé, kteří mají předvolby profesionálové položky, které mohou sloužit k doporučení.

Následujícím obrázku je příklad velmi jednoduché, který používá videa:

* __Spoluvytváření occurance__: Jana, Alice a Jan dali lajk _hvězda válek_ _Empire stávkám zpět_a _vrácení Jedi_. Mahout Určuje, že uživatelé, kteří taky jako jednu z těchto filmy jako ostatní dvě.

* __Spoluvytváření occurance__: Jan a Alice taky dali lajk _Menace fiktivní_ _útok klonů_a _odvety Sith_. Mahout Určuje, že uživatelé, kteří dali lajk předchozí tři filmy také jako tyto tři.

* __Podobnosti doporučení__: vzhledem k tomu, Jan dali lajk první tři filmy, Mahout sleduje videa, které ostatní uživatelé s podobným předvolby dali lajk, ale nebyla Jana sledované (dali lajk/hodnocení). V tomto případě Mahout doporučuje _Menace fiktivní_ _útok klonů_a _odvety Sith_.

###<a name="understanding-the-data"></a>Principy datového

Snadno, [GroupLens výzkumu] [ movielens] poskytuje hodnocení data pro video ve formátu, který je kompatibilní se službou Mahout. Tato data jsou k dispozici na svůj cluster výchozí úložiště na `/HdiSamples/MahoutMovieData`.

Existují dva soubory `moviedb.txt` (informace o videa,) a `user-ratings.txt`. Uživatel ratings.txt soubor je používán během analýzy, zatímco moviedb.txt slouží k poskytnutí informace o popisný text při zobrazování výsledky analýzy.

Data obsažená v uživatele ratings.txt obsahuje strukturu z `userID`, `movieID`, `userRating`, a `timestamp`, které víme, jak vysoce každý uživatel hodnocení videa. Tady je příklad dat:


    196 242 3   881250949
    186 302 3   891717742
    22  377 1   878887116
    244 51  2   880606923
    166 346 1   886397596

###<a name="run-the-job"></a>Spuštění úlohy

Použijte tento skript prostředí Windows PowerShell spuštění úlohy, který používá modul doporučení Mahout s daty videa:

    # The HDInsight cluster name.
    $clusterName = "the cluster name"
    
    #Get HTTPS/Admin credentials for submitting the job later
    $creds = Get-Credential
    #Get the cluster info so we can get the resource group, storage, etc.
    $clusterInfo = Get-AzureRmHDInsightCluster -ClusterName $clusterName
    $resourceGroup = $clusterInfo.ResourceGroup
    $storageAccountName=$clusterInfo.DefaultStorageAccount.split('.')[0]
    $container=$clusterInfo.DefaultStorageContainer
    $storageAccountKey=(Get-AzureRmStorageAccountKey `
        -Name $storageAccountName `
        -ResourceGroupName $resourceGroup)[0].Value
            
    #Create a storage content and upload the file
    $context = New-AzureStorageContext `
        -StorageAccountName $storageAccountName `
        -StorageAccountKey $storageAccountKey
            
    # NOTE: The version number in the file path
    # may change in future versions of HDInsight.
    $jarFile =  "file:///C:/apps/dist/mahout-0.9.0.2.2.9.1-8/examples/target/mahout-examples-0.9.0.2.2.9.1-8-job.jar"
    #
    # If you are using an earlier version of HDInsight,
    # set $jarFile to the jar file you
    # uploaded.
    # For example,
    # $jarFile = "wasbs:///example/jars/mahout-core-0.9-job.jar"

    # The arguments for this job
    # * input - the path to the data uploaded to HDInsight
    # * output - the path to store output data
    # * tempDir - the directory for temp files
    $jobArguments = "--similarityClassname", "recommenditembased", `
                    "-s", "SIMILARITY_COOCCURRENCE", `
                    "--input", "wasbs:///HdiSamples/MahoutMovieData/user-ratings.txt",
                    "--output", "wasbs:///example/out",
                    "--tempDir", "wasbs:///example/temp"

    # Create the job definition
    $jobDefinition = New-AzureRmHDInsightMapReduceJobDefinition `
      -JarFile $jarFile `
      -ClassName "org.apache.mahout.cf.taste.hadoop.item.RecommenderJob" `
      -Arguments $jobArguments

    # Start the job
    $job = Start-AzureRmHDInsightJob `
        -ClusterName $clusterName `
        -JobDefinition $jobDefinition `
        -HttpCredential $creds

    # Wait on the job to complete
    Write-Host "Wait for the job to complete ..." -ForegroundColor Green
    Wait-AzureRmHDInsightJob `
            -ClusterName $clusterName `
            -JobId $job.JobId `
            -HttpCredential $creds
    # Download the output
    Get-AzureStorageBlobContent `
            -Blob example/out/part-r-00000 `
            -Container $container `
            -Destination output.txt `
            -Context $context
            
    # Write out any error information
    Write-Host "STDERR"
    Get-AzureRmHDInsightJobOutput `
            -Clustername $clusterName `
            -JobId $job.JobId `
            -DefaultContainer $container `
            -DefaultStorageAccountName $storageAccountName `
            -DefaultStorageAccountKey $storageAccountKey `
            -HttpCredential $creds `
            -DisplayOutputType StandardError

> [AZURE.NOTE] Úlohy mahout neodebírat dočasná data, která se vytvoří při zpracování úlohy. `--tempDir` Parametr není zadán úlohy příklad vyčlenění dočasných souborů do určitého adresáře.

Úlohy Mahout nevrátí výstup StdOut. Místo toho uloží se v adresáři zadaný výstupní jako __součást r-00000__. Skript stáhne tento soubor __výstup.txt__ aktuálního adresáře na počítači.

Následujícím obrázku je příklad obsahu tento soubor:

    1   [234:5.0,347:5.0,237:5.0,47:5.0,282:5.0,275:5.0,88:5.0,515:5.0,514:5.0,121:5.0]
    2   [282:5.0,210:5.0,237:5.0,234:5.0,347:5.0,121:5.0,258:5.0,515:5.0,462:5.0,79:5.0]
    3   [284:5.0,285:4.828125,508:4.7543354,845:4.75,319:4.705128,124:4.7045455,150:4.6938777,311:4.6769233,248:4.65625,272:4.649266]
    4   [690:5.0,12:5.0,234:5.0,275:5.0,121:5.0,255:5.0,237:5.0,895:5.0,282:5.0,117:5.0]

První sloupec představuje `userID`. Hodnoty obsažené v ' ["a"] "jsou `movieId`:`recommendationScore`.

###<a name="view-the-output"></a>Zobrazit výstup

Ačkoli generovaný výstup může být OK pro použití v aplikaci, je velmi čitelný. `moviedb.txt` Ze serveru můžete použít k řešení `movieId` k videu název, ale musí nejdřív stáhněte tak soubor hodnocení ze serveru pomocí následující skript:

    # The HDInsight cluster name.
    $clusterName = "the cluster name"
    
    #Get the cluster info so we can get the resource group, storage, etc.
    $clusterInfo = Get-AzureRmHDInsightCluster -ClusterName $clusterName
    $resourceGroup = $clusterInfo.ResourceGroup
    $storageAccountName=$clusterInfo.DefaultStorageAccount.split('.')[0]
    $container=$clusterInfo.DefaultStorageContainer
    $storageAccountKey=(Get-AzureRmStorageAccountKey `
        -Name $storageAccountName `
        -ResourceGroupName $resourceGroup)[0].Value
    #Create a storage content and upload the file
    $context = New-AzureStorageContext `
        -StorageAccountName $storageAccountName `
        -StorageAccountKey $storageAccountKey
    #Download the files
    Get-AzureStorageBlobContent -blob "HdiSamples/MahoutMovieData/moviedb.txt" `
    -Container $container `
    -Destination moviedb.txt `
    -Context $context
    Get-AzureStorageBlobContent -blob "HdiSamples/MahoutMovieData/user-ratings.txt" `
    -Container $container `
    -Destination user-ratings.txt `
    -Context $context

Po stažení souborů použijte tento skript Powershellu Pokud chcete zobrazovat doporučení s názvy videa:

    <#
    .SYNOPSIS
        Displays recommendations for movies.
    .DESCRIPTION
        Displays recommendations generated by Mahout
        with HDInsight example in a human readable format.
    .EXAMPLE
        .\Show-Recommendation -userId 4
            -userDataFile "user-ratings.txt"
            -movieFile "moviedb.txt"
            -recommendationFile "output.txt"
    #>

    [CmdletBinding(SupportsShouldProcess = $true)]
    param(
        #The user ID
        [Parameter(Mandatory = $true)]
        [String]$userId,

        [Parameter(Mandatory = $true)]
        [String]$userDataFile,

        [Parameter(Mandatory = $true)]
        [String]$movieFile,

        [Parameter(Mandatory = $true)]
        [String]$recommendationFile
    )
    # Read movie ID & description into hash table
    Write-Host "Reading movies descriptions" -ForegroundColor Green
    $movieById = @{}
    foreach($line in Get-Content $movieFile)
    {
        $tokens = $line.Split("|")
        $movieById[$tokens[0]] = $tokens[1]
    }
    # Load movies user has already seen (rated)
    # into a hash table
    Write-Host "Reading rated movies" -ForegroundColor Green
    $ratedMovieIds = @{}
    foreach($line in Get-Content $userDataFile)
    {
        $tokens = $line.Split("`t")
        if($tokens[0] -eq $userId)
        {
            # Resolve the ID to the movie name
            $ratedMovieIds[$movieById[$tokens[1]]] = $tokens[2]
        }
    }
    # Read recommendations generated by Mahout
    Write-Host "Reading recommendations" -ForegroundColor Green
    $recommendations = @{}
    foreach($line in get-content $recommendationFile)
    {
        $tokens = $line.Split("`t")
        if($tokens[0] -eq $userId)
        {
            #Trim leading/treailing [] and split at ,
            $movieIdAndScores = $tokens[1].TrimStart("[").TrimEnd("]").Split(",")
            foreach($movieIdAndScore in $movieIdAndScores)
            {
                #Split at : and store title and score in a hash table
                $idAndScore = $movieIdAndScore.Split(":")
                $recommendations[$movieById[$idAndScore[0]]] = $idAndScore[1]
            }
            break
        }
    }

    Write-Host "Rated movies" -ForegroundColor Green
    Write-Host "---------------------------" -ForegroundColor Green
    $ratedFormat = @{Expression={$_.Name};Label="Movie";Width=40}, `
                   @{Expression={$_.Value};Label="Rating"}
    $ratedMovieIds | format-table $ratedFormat
    Write-Host "---------------------------" -ForegroundColor Green

    write-host "Recommended movies" -ForegroundColor Green
    Write-Host "---------------------------" -ForegroundColor Green
    $recommendationFormat = @{Expression={$_.Name};Label="Movie";Width=40}, `
                            @{Expression={$_.Value};Label="Score"}
    $recommendations | format-table $recommendationFormat

Následující obrázek je příkladem spuštění skriptu:

    PS C:\> show-recommendation.ps1 -userId 4 -userDataFile .\user-ratings.txt -movieFile .\moviedb.txt -recommendationFile .\output.txt

Výstup by měl vypadat takto:

    Reading movies descriptions
    Reading rated movies
    Reading recommendations
    Rated movies
    ---------------------------
    Movie                                    Rating
    -----                                    ------
    Devil's Own, The (1997)                  1
    Alien: Resurrection (1997)               3
    187 (1997)                               2
    (lines ommitted)

    ---------------------------
    Recommended movies
    ---------------------------

    Movie                                    Score
    -----                                    -----
    Good Will Hunting (1997)                 4.6504064
    Swingers (1996)                          4.6862745
    Wings of the Dove, The (1997)            4.6666665
    People vs. Larry Flynt, The (1996)       4.834559
    Everyone Says I Love You (1996)          4.707071
    Secrets & Lies (1996)                    4.818182
    That Thing You Do! (1996)                4.75
    Grosse Pointe Blank (1997)               4.8235292
    Donnie Brasco (1997)                     4.6792455
    Lone Star (1996)                         4.7099237  

##<a name="classify"></a>Klasifikovat dat pomocí příkazového řádku Hadoop

Klasifikace metody dostupné s Mahout reprodukujte vytvářet [náhodné doménové][forest]. Toto je více krocích, který využívá data školení pro vytvoření stromové struktury rozhodování, které pak slouží ke klasifikaci data. To používá třídu __org.apache.mahout.classifier.df.tools.Describe__ poskytovanou Mahout. Aktuálně musí být běžet pomocí příkazového řádku Hadoop.

###<a name="load-the-data"></a>Načtení dat

1. Stáhněte si následující soubory z [NSL KDD uvedenou množinu dat](http://nsl.cs.unb.ca/NSL-KDD/).

  * [KDDTrain +. ARFF](http://nsl.cs.unb.ca/NSL-KDD/KDDTrain+.arff): soubor školení

  * [KDDTest +. ARFF](http://nsl.cs.unb.ca/NSL-KDD/KDDTest+.arff): testovací data

2. Každý soubor otevřete a odstranění řádků nahoře, začíná '@', a poté soubory uložit. Pokud tyto nejsou odebrat, zobrazí se chybové zprávy při použití tato data v Mahout.

2. Nahrajte soubory do __vzorovými/daty__. Lze provést pomocí následující skript. Nahraďte __NÁZEV_CLUSTERU__ název clusteru HDInsight. Nahraďte název souboru název fo soubor na nahrát.

        #Get the cluster info so we can get the resource group, storage, etc.
        $clusterName="CLUSTERNAME"
        $fileToUpload="FILENAME"
        $blobPath="example/data/FILENAME"
        $clusterInfo = Get-AzureRmHDInsightCluster -ClusterName $clusterName
        $resourceGroup = $clusterInfo.ResourceGroup
        $storageAccountName=$clusterInfo.DefaultStorageAccount.split('.')[0]
        $container=$clusterInfo.DefaultStorageContainer
        $storageAccountKey=(Get-AzureRmStorageAccountKey `
            -Name $storageAccountName `
        -ResourceGroupName $resourceGroup)[0].Value
        
        #Create a storage content and upload the file
        $context = New-AzureStorageContext `
            -StorageAccountName $storageAccountName `
            -StorageAccountKey $storageAccountKey
            
        Set-AzureStorageBlobContent `
            -File $fileToUpload `
            -Blob $blobPath `
            -Container $container `
            -Context $context

###<a name="run-the-job"></a>Spuštění úlohy

1. Tento úkol vyžaduje Hadoop příkazového řádku. Povolit připojení ke vzdálené ploše pro HDInsight clusteru, potom je připojíte k němu postupujte podle pokynů na [připojit k clusterů HDInsight pomocí RDP](hdinsight-administer-use-management-portal.md#rdp).

3. Po připojení, použijte ikonu __Hadoop příkazového řádku__ otevřete Hadoop příkazového řádku:

    ![hadoop rozhraní příkazového řádku][hadoopcli]

3. Pomocí následujícího příkazu generovat popisovače souborů (__KDDTrain + .info__), který využívá Mahout.

        hadoop jar "c:/apps/dist/mahout-0.9.0.2.2.9.1-8/examples/target/mahout-examples-0.9.0.2.2.9.1-8-job.jar" org.apache.mahout.classifier.df.tools.Describe -p "wasbs:///example/data/KDDTrain+.arff" -f "wasbs:///example/data/KDDTrain+.info" -d N 3 C 2 N C 4 N C 8 N 2 C 19 N L

    `N 3 C 2 N C 4 N C 8 N 2 C 19 N L` Popisuje atributy data v souboru. Například L označuje popisku.

4. Vytvoření struktury stromové struktury rozhodování pomocí tento příkaz:

        hadoop jar c:/apps/dist/mahout-0.9.0.2.2.9.1-8/examples/target/mahout-examples-0.9.0.2.2.9.1-8-job.jar org.apache.mahout.classifier.df.mapreduce.BuildForest -Dmapred.max.split.size=1874231 -d wasbs:///example/data/KDDTrain+.arff -ds wasbs:///example/data/KDDTrain+.info -sl 5 -p -t 100 -o nsl-forest

    Výstup operaci je uložen v adresáři __nsl struktury__ , který je umístěn v úložišti pro svůj cluster HDInsight na __ wasbs://user/&lt;uživatelské jméno > / nsl doménové/nsl-forest.seq. &lt;Uživatelské jméno > je uživatelské jméno, které jste použili pro relace vzdálené plochy. Tento soubor je lidí.

5. Otestujte struktuře klasifikaci __KDDTest + .arff__ datovou sadu. Použijte tento příkaz:

        hadoop jar c:/apps/dist/mahout-0.9.0.2.2.9.1-8/examples/target/mahout-examples-0.9.0.2.2.9.1-8-job.jar org.apache.mahout.classifier.df.mapreduce.TestForest -i wasbs:///example/data/KDDTest+.arff -ds wasbs:///example/data/KDDTrain+.info -m nsl-forest -a -mr -o wasbs:///example/data/predictions

    Tento příkaz vrátí souhrnné informace o procesu klasifikace podobná této:

        14/07/02 14:29:28 INFO mapreduce.TestForest:

        =======================================================
        Summary
        -------------------------------------------------------
        Correctly Classified Instances          :      17560       77.8921%
        Incorrectly Classified Instances        :       4984       22.1079%
        Total Classified Instances              :      22544

        =======================================================
        Confusion Matrix
        -------------------------------------------------------
        a       b       <--Classified as
        9437    274      |  9711        a     = normal
        4710    8123     |  12833       b     = anomaly

        =======================================================
        Statistics
        -------------------------------------------------------
        Kappa                                       0.5728
        Accuracy                                   77.8921%
        Reliability                                53.4921%
        Reliability (standard deviation)            0.4933

  Tuto úlohu také vytvoří soubor c:\ __wasbs:///example/data/predictions/KDDTest+.arff.out__. Tento soubor je však není čitelný lidmi.

> [AZURE.NOTE] Úlohy mahout není přepsat soubory. Pokud chcete znovu provést tyto úlohy, je nutné odstranit soubory, které byly vytvořeny v předchozích úloh.

##<a name="troubleshooting"></a>Řešení potíží

###<a name="install"></a>Instalace Mahout

Mahout máte nainstalované na HDInsight 3.1 clusterů a je možné nainstalovat ručně na HDInsight 3.0 nebo HDInsight 2.1 clusterů pomocí následujících kroků:

1. Verze Mahout používat závisí na verzi HDInsight clusteru. Verze obrázku můžete najít v zobrazení vlastností pro cluster v portálu Azure.

  * __Pro 2.1 HDInsight__, můžete si stáhnout soubor archivu Java (SKLENICE), který obsahuje [Mahout 0,9](http://repo2.maven.org/maven2/org/apache/mahout/mahout-core/0.9/mahout-core-0.9-job.jar).

  * __Pro 3.0 HDInsight__, musíte [vytvořit Mahout ze zdroje] [ build] a určení verze Hadoop poskytovanou HDInsight. Instalace předpoklady uvedená na stránce vytvořit, stažení zdroje a pak použijte tento příkaz vytvářet soubory sklenice Mahout:

            mvn -Dhadoop2.version=2.2.0 -DskipTests clean package

        After the build completes, you can find the JAR file at __mahout\mrlegacy\target\mahout-mrlegacy-1.0-SNAPSHOT-job.jar__.

        > [AZURE.NOTE] Při uvolnění Mahout 1.0 by měl moct použít předdefinované balíčků s HDInsight 3.0.

2. Sklenice soubor nahrajte __Příklad/sklenic po g__ v úložišti výchozí pro svůj cluster. Nahraďte NÁZEV_CLUSTERU v následujícím skriptu název HDInsight clusteru a nahraďte cestu k souboru __mahout coure 0,9 job.jar__ název souboru.

        #Get the cluster info so we can get the resource group, storage, etc.
        $clusterName = "CLUSTERNAME"
        $fileToUpload = "FILENAME"
        $clusterInfo = Get-AzureRmHDInsightCluster -ClusterName $clusterName
        $resourceGroup = $clusterInfo.ResourceGroup
        $storageAccountName=$clusterInfo.DefaultStorageAccount.split('.')[0]
        $container=$clusterInfo.DefaultStorageContainer
        $storageAccountKey=(Get-AzureRmStorageAccountKey `
            -Name $storageAccountName `
        -ResourceGroupName $resourceGroup)[0].Value
        
        #Create a storage content and upload the file
        $context = New-AzureStorageContext `
            -StorageAccountName $storageAccountName `
            -StorageAccountKey $storageAccountKey
            
        Set-AzureStorageBlobContent `
            -File $fileToUpload `
            -Blob "example/jars/mahout-core-0.9-job.jar" `
            -Container $container `
            -Context $context

###<a name="cannot-overwrite-files"></a>Soubory nelze přepsat.

Použijte úlohy mahout vyčistit dočasných souborů, které byly vytvořené během zpracování. Kromě toho úlohy nepřepíše existující výstupní soubor.

Aby nedocházelo k chybám při spuštění úlohy Mahout, odstranit dočasné a výstupní soubory mezi spustí nebo použít názvy jedinečné dočasný a výstupní adresářů.

###<a name="cannot-find-the-jar-file"></a>Nemůžete najít soubor SKLENICE

HDInsight 3.1 clusterů zahrnout Mahout. Cesty a názvu souboru obsahovat číslo verze Mahout, které máte nainstalované na clusteru. Příklad skriptu prostředí Windows PowerShell v tomto kurzu používá cestu, která je platná od 2015 dne, ale číslo verze se změní v budoucích aktualizacích HDInsight. Pokud chcete zjistit aktuální cestu k souboru Mahout JAR pro svůj cluster, pomocí následujícího příkazu prostředí Windows PowerShell a změňte skript, který chcete odkazovat cesta k souboru, který je vrácen:

    Use-AzureRmHDInsightCluster -ClusterName $clusterName
    #Get the cluster info so we can get the resource group, storage, etc.
        $clusterInfo = Get-AzureRmHDInsightCluster -ClusterName $clusterName
        $resourceGroup = $clusterInfo.ResourceGroup
        $storageAccountName=$clusterInfo.DefaultStorageAccount.split('.')[0]
        $container=$clusterInfo.DefaultStorageContainer
        $storageAccountKey=(Get-AzureRmStorageAccountKey `
            -Name $storageAccountName `
        -ResourceGroupName $resourceGroup)[0].Value
    Invoke-AzureRmHDInsightHiveJob `
            -StatusFolder "wasbs:///example/statusout" `
            -DefaultContainer $container `
            -DefaultStorageAccountName $storageAccountName `
            -DefaultStorageAccountKey $storageAccountKey `
            -Query '!${env:COMSPEC} /c dir /b /s ${env:MAHOUT_HOME}\examples\target\*-job.jar'

###<a name="nopowershell"></a>Třídy, které nefungují používat Windows PowerShell

Úlohy mahout, které používají následující třídy vrátit řadu chybové zprávy při z prostředí Windows PowerShell:

* org.apache.mahout.utils.clustering.ClusterDumper
* org.apache.mahout.utils.SequenceFileDumper
* org.apache.mahout.utils.vectors.lucene.Driver
* org.apache.mahout.utils.vectors.arff.Driver
* org.apache.mahout.text.WikipediaToSequenceFile
* org.apache.mahout.clustering.streaming.tools.ResplitSequenceFiles
* org.apache.mahout.clustering.streaming.tools.ClusterQualitySummarizer
* org.apache.mahout.classifier.sgd.TrainLogistic
* org.apache.mahout.classifier.sgd.RunLogistic
* org.apache.mahout.classifier.sgd.TrainAdaptiveLogistic
* org.apache.mahout.classifier.sgd.ValidateAdaptiveLogistic
* org.apache.mahout.classifier.sgd.RunAdaptiveLogistic
* org.apache.mahout.classifier.sequencelearning.hmm.BaumWelchTrainer
* org.apache.mahout.classifier.sequencelearning.hmm.ViterbiEvaluator
* org.apache.mahout.classifier.sequencelearning.hmm.RandomSequenceGenerator
* org.apache.mahout.classifier.df.tools.Describe

Spuštění úlohy, které používají tyto třídy, připojte se k němu HDInsight a spuštění úlohy pomocí příkazového řádku Hadoop. Příklad naleznete v tématu [třídění dat pomocí příkazového řádku Hadoop](#classify) .

##<a name="next-steps"></a>Další kroky

Teď, když jste se naučili používání Mahout, seznamte se s další způsoby, jak pracovat s daty v HDInsight:

* [Podregistru s HDInsight](hdinsight-use-hive.md)
* [Prasátko s HDInsight](hdinsight-use-pig.md)
* [MapReduce s HDInsight](hdinsight-use-mapreduce.md)

[build]: http://mahout.apache.org/developers/buildingmahout.html
[aps]: ../powershell-install-configure.md
[movielens]: http://grouplens.org/datasets/movielens/
[100k]: http://files.grouplens.org/datasets/movielens/ml-100k.zip
[getstarted]: hdinsight-hadoop-linux-tutorial-get-started.md
[upload]: hdinsight-upload-data.md
[ml]: http://en.wikipedia.org/wiki/Machine_learning
[forest]: http://en.wikipedia.org/wiki/Random_forest
[management]: https://manage.windowsazure.com/
[enableremote]: ./media/hdinsight-mahout/enableremote.png
[connect]: ./media/hdinsight-mahout/connect.png
[hadoopcli]: ./media/hdinsight-mahout/hadoopcli.png
[tools]: https://github.com/Blackmist/hdinsight-tools
 
