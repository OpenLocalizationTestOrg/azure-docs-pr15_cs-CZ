<properties
    pageTitle="Analýza dat zpoždění letů pomocí Hadoop v HDInsight | Microsoft Azure"
    description="Zjistěte, jak pomocí jednoho skriptu prostředí Windows PowerShell vytvoření clusteru HDInsight úlohu podregistru, úlohu Sqoop a odstranění clusteru."
    services="hdinsight"
    documentationCenter=""
    authors="mumian"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/19/2016"
    ms.author="jgao"/>

#<a name="analyze-flight-delay-data-by-using-hive-in-hdinsight"></a>Analýza dat zpoždění letů pomocí podregistru v HDInsight

Podregistru prostředkem k projdete Hadoop MapReduce úlohy skriptovacího jazyka SQL profesionálové s názvem * [HiveQL][hadoop-hiveql]*, které se dají použít k vytváření souhrnů, dotazování a analýza velkých objemů dat.

> [AZURE.NOTE] Kroky v tomto dokumentu vyžadují clusteru serveru s Windows HDInsight. Postup, které spolupracují s Linux clusteru najdete v tématech [daty zpoždění letů analyzovat pomocí podregistru v HDInsight (Linux)](hdinsight-analyze-flight-delay-data-linux.md).

Jednou z hlavních výhod Azure HDInsight je oddělení ukládání dat a výpočetním. Úložiště objektů Blob Azure HDInsight používá pro ukládání dat. Typické projektu zahrnuje tři části:

1. **Ukládání dat v úložišti objektů Blob Azure.**  Například počasí data, data snímače, protokoly web, a v tomto případě daty zpoždění letů ukládají do úložiště objektů Blob Azure.
2. **Spuštění úlohy.** Po je čas na zpracování dat, spustit skriptu prostředí Windows PowerShell (nebo klientské aplikaci) vytvoření HDInsight clusteru, spusťte úlohy a odstraňovat clusteru. Úlohy uložit výstupní data k úložišti objektů Blob Azure. Výstupní data se zachovají i po odstranění clusteru. Tímto způsobem, můžete zaplatit pouze co budete mít spotřebované množství.
3. **Načtení výstup z úložiště objektů Blob Azure**, nebo v tomto kurzu exportovat data k databázi Azure SQL.

Následující obrázek znázorňuje scénáře a struktury tohoto kurzu:

![HDI. FlightDelays.flow][img-hdi-flightdelays-flow]

**Poznámka**: čísla v diagramu odpovídají nadpisy oddílů. **M** zastupuje hlavní proces. **A** zastupuje obsahu v dodatku.

Hlavní část kurzu se dozvíte, jak pomocí jednoho skriptu prostředí Windows PowerShell dělat tyto věci:

- Vytvoření clusteru HDInsight.
- Spuštění úlohy podregistru clusteru pro výpočet průměrných zpoždění letišť. Daty zpoždění letů je uložené na účtu úložiště objektů Blob Azure.
- Spuštění úlohy Sqoop export výstupu projektu podregistru do databáze Azure SQL.
- Odstranění obrázku HDInsight.

V dodatky najdete pokyny pro nahrávání daty zpoždění letů, vytváření a nahrávání řetězci dotazu podregistru a příprava schůzky v databázi Azure SQL Sqoop úlohy.

> [AZURE.NOTE] Kroky v tomto dokumentu jsou specifické pro clusterů HDInsight serveru s Windows. Postup, které spolupracují s Linux clusteru najdete v tématech [analyzovat letu zpoždění dat pomocí podregistru v HDInsight (Linux)](hdinsight-analyze-flight-delay-data-linux.md)

###<a name="prerequisites"></a>Zjistit předpoklady pro

Před zahájením tohoto kurzu, musíte mít následující položky:

- **Azure předplatného**. Viz [získání Azure bezplatnou zkušební verzi](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).

- **Pracoviště se Azure Powershellu**.

    [AZURE.INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]

**Soubory používané v tomto kurzu**

Tento kurz používá ve včasných výkon letech letecké společnosti ze [zdroje informací a inovativní technologii Správa, Statistika dopravy Bureau nebo RITĚ] [rita-website]. Kopírovat data byl odeslán do kontejneru úložiště objektů Blob Azure veřejné objektů Blob oprávnění pro přístup. Součástí skript Powershellu slouží ke kopírování dat z veřejné objektů blob kontejneru do kontejneru výchozí objektů blob clusteru. Skript HiveQL zkopírována do stejného kontejneru objektů Blob. Pokud budete chtít zjistěte, jak získat/odeslat data, která chcete účtu úložiště a jak vytvořit a nahrát soubor skriptu HiveQL, naleznete v [dodatku A](#appendix-a) a [B dodatek](#appendix-b).

V následující tabulce jsou uvedeny soubory používané v tomto kurzu:

<table border="1">
<tr><th>Soubory</th><th>Popis</th></tr>
<tr><td>wasbs://flightdelay@hditutorialdata.blob.core.windows.net/flightdelays.hql</td><td>Soubor skriptu HiveQL používaný podregistru projektu. Tento skript byl odeslán k účtu úložiště objektů Blob Azure s veřejným přístupem. <a href="#appendix-b">Dodatek B</a> obsahuje pokyny pro přípravu a odeslání tento soubor do vašeho účtu úložiště objektů Blob Azure.</td></tr>
<tr><td>wasbs://flightdelay@hditutorialdata.blob.core.windows.net/2013Data</td><td>Zadávání dat pro danou úlohu podregistru. K účtu úložiště objektů Blob Azure s veřejným přístupem je uložená data. <a href="#appendix-a">Dodatek A</a> obsahuje pokyny k získání dat a uložení dat do vašeho účtu úložiště objektů Blob Azure.</td></tr>
<tr><td>\tutorials\flightdelays\output</td><td>Výstup cesty pro daný úkol podregistru. Výchozí kontejner se používá k ukládání dat výstupu.</td></tr>
<tr><td>\tutorials\flightdelays\jobstatus</td><td>Podregistru složka stavu úlohy na výchozí kontejner.</td></tr>
</table>


##<a name="create-cluster-and-run-hivesqoop-jobs"></a>Vytvoření obrázku a spuštění úlohy podregistru/Sqoop

Hadoop MapReduce je dávkové zpracování. Nejefektivnější způsob spuštění úlohy podregistru je vytvoření clusteru úlohy a odstranění úlohy po dokončení projektu. Tento skript zahrnuje celého procesu. Další informace o vytváření HDInsight obrázku a spuštěním úloh podregistru, najdete v článku [Vytvoření Hadoop clusterů HDInsight] [ hdinsight-provision] a [Použití podregistru s HDInsight][hdinsight-use-hive].

**Chcete-li spouštění dotazů podregistru pomocí prostředí PowerShell Azure**

1. Vytvoření databáze Azure SQL a tabulky jako výstupního Sqoop úlohy pomocí pokynů uvedených v [Dodatku C](#appendix-c).
3. Otevřete Windows PowerShell ISE a spuštěním následujícího skriptu:

        $subscriptionID = "<Azure Subscription ID>"
        $nameToken = "<Enter an Alias>" 
        
        ###########################################
        # You must configure the follwing variables
        # for an existing Azure SQL Database
        ###########################################
        $existingSqlDatabaseServerName = "<Azure SQL Database Server>"
        $existingSqlDatabaseLogin = "<Azure SQL Database Server Login>"
        $existingSqlDatabasePassword = "<Azure SQL Database Server login password>"
        $existingSqlDatabaseName = "<Azure SQL Database name>"
        
        $localFolder = "E:\Tutorials\Downloads\" # A temp location for copying files. 
        $azcopyPath = "C:\Program Files (x86)\Microsoft SDKs\Azure\AzCopy" # depends on the version, the folder can be different
        
        ###########################################
        # (Optional) configure the following variables
        ###########################################
        
        $namePrefix = $nameToken.ToLower() + (Get-Date -Format "MMdd")

        $resourceGroupName = $namePrefix + "rg"
        $location = "EAST US 2"
        
        $HDInsightClusterName = $namePrefix + "hdi"
        $httpUserName = "admin"
        $httpPassword = "<Enter the Password>"
        
        $defaultStorageAccountName = $namePrefix + "store"
        $defaultBlobContainerName = $HDInsightClusterName # use the cluster name
        
        $existingSqlDatabaseTableName = "AvgDelays"
        $sqlDatabaseConnectionString = "jdbc:sqlserver://$existingSqlDatabaseServerName.database.windows.net;user=$existingSqlDatabaseLogin@$existingSqlDatabaseServerName;password=$existingSqlDatabaseLogin;database=$existingSqlDatabaseName"
        
        $hqlScriptFile = "/tutorials/flightdelays/flightdelays.hql" 
        
        $jobStatusFolder = "/tutorials/flightdelays/jobstatus"
        
        ###########################################
        # Login
        ###########################################
        try{
            $acct = Get-AzureRmSubscription
        }
        catch{
            Login-AzureRmAccount
        }
        Select-AzureRmSubscription -SubscriptionID $subscriptionID
        
        ###########################################
        # Create a new HDInsight cluster
        ###########################################
        
        # Create ARM group
        New-AzureRmResourceGroup -Name $resourceGroupName -Location $location
        
        # Create the default storage account
        New-AzureRmStorageAccount -ResourceGroupName $resourceGroupName -Name $defaultStorageAccountName -Location $location -Type Standard_LRS
        
        # Create the default Blob container
        $defaultStorageAccountKey = (Get-AzureRmStorageAccountKey -ResourceGroupName $resourceGroupName -Name $defaultStorageAccountName)[0].Value
        $defaultStorageAccountContext = New-AzureStorageContext -StorageAccountName $defaultStorageAccountName -StorageAccountKey $defaultStorageAccountKey 
        New-AzureStorageContainer -Name $defaultBlobContainerName -Context $defaultStorageAccountContext 
        
        # Create the HDInsight cluster
        $pw = ConvertTo-SecureString -String $httpPassword -AsPlainText -Force
        $httpCredential = New-Object System.Management.Automation.PSCredential($httpUserName,$pw)
        
        New-AzureRmHDInsightCluster `
            -ResourceGroupName $resourceGroupName `
            -ClusterName $HDInsightClusterName `
            -Location $location `
            -ClusterType Hadoop `
            -OSType Windows `
            -ClusterSizeInNodes 2 `
            -HttpCredential $httpCredential `
            -DefaultStorageAccountName "$defaultStorageAccountName.blob.core.windows.net" `
            -DefaultStorageAccountKey $defaultStorageAccountKey `
            -DefaultStorageContainer $existingDefaultBlobContainerName 
        
        
        ###########################################
        # Prepare the HiveQL script and source data
        ###########################################
        
        # Create the temp location  
        New-Item -Path $localFolder -ItemType Directory -Force 
        
        # Download the sample file from Azure Blob storage
        $context = New-AzureStorageContext -StorageAccountName "hditutorialdata" -Anonymous
        $blobs = Get-AzureStorageBlob -Container "flightdelay" -Context $context
        #$blobs | Get-AzureStorageBlobContent -Context $context -Destination $localFolder
        
        # Upload data to default container
        
        $azcopycmd = "cmd.exe /C '$azcopyPath\azcopy.exe' /S /Source:'$localFolder' /Dest:'https://$defaultStorageAccountName.blob.core.windows.net/$defaultBlobContainerName/tutorials/flightdelays' /DestKey:$defaultStorageAccountKey"
        
        Invoke-Expression -Command:$azcopycmd
        
        ###########################################
        # Submit the Hive job
        ###########################################
        Use-AzureRmHDInsightCluster -ClusterName $HDInsightClusterName -HttpCredential $httpCredential
        $response = Invoke-AzureRmHDInsightHiveJob `
                        -Files $hqlScriptFile `
                        -DefaultContainer $defaultBlobContainerName `
                        -DefaultStorageAccountName $defaultStorageAccountName `
                        -DefaultStorageAccountKey $defaultStorageAccountKey `
                        -StatusFolder $jobStatusFolder 
        
        write-Host $response
        
        
        ###########################################
        # Submit the Sqoop job
        ###########################################
        $exportDir = "wasbs://$defaultBlobContainerName@$defaultStorageAccountName.blob.core.windows.net/tutorials/flightdelays/output"
        
        $sqoopDef = New-AzureRmHDInsightSqoopJobDefinition `
                        -Command "export --connect $sqlDatabaseConnectionString --table $sqlDatabaseTableName --export-dir $exportDir --fields-terminated-by \001 "
        $sqoopJob = Start-AzureRmHDInsightJob `
                        -ResourceGroupName $resourceGroupName `
                        -ClusterName $hdinsightClusterName `
                        -HttpCredential $httpCredential `
                        -JobDefinition $sqoopDef #-Debug -Verbose
        
        Wait-AzureRmHDInsightJob `
            -ResourceGroupName $resourceGroupName `
            -ClusterName $HDInsightClusterName `
            -HttpCredential $httpCredential `
            -WaitTimeoutInSeconds 3600 `
            -Job $sqoopJob.JobId
        
        
        Get-AzureRmHDInsightJobOutput `
            -ResourceGroupName $resourceGroupName `
            -ClusterName $hdinsightClusterName `
            -HttpCredential $httpCredential `
            -DefaultContainer $existingDefaultBlobContainerName `
            -DefaultStorageAccountName $defaultStorageAccountName `
            -DefaultStorageAccountKey $defaultStorageAccountKey `
            -JobId $sqoopJob.JobId `
            -DisplayOutputType StandardError
        
        ###########################################
        # Delete the cluster
        ###########################################
        Remove-AzureRmHDInsightCluster -ResourceGroupName $resourceGroupName -ClusterName $hdinsightClusterName

6. Připojení k databázi SQL a najdete v článku průměr letů podle města v tabulce AvgDelays:

    ![HDI. FlightDelays.AvgDelays.Dataset][image-hdi-flightdelays-avgdelays-dataset]



---
##<a id="appendix-a"></a>Dodatek A - daty zpoždění letů nahrát k úložišti objektů Blob Azure
Odeslání souboru dat a soubory skriptů HiveQL (naleznete v [Dodatku B](#appendix-b)) vyžaduje některé plánování. Cílem je ukládat datové soubory a HiveQL soubor ještě před vytvořením HDInsight obrázku a úloha podregistru. Máte dvě možnosti:

- **Použijte stejný účet Azure úložiště, který bude clusteru HDInsight používá jako výchozí systém souborů.** Protože clusteru HDInsight bude mít přístup klíč účtu úložiště, nemusíte proveďte případné další změny.
- **Použijte jiný účet Azure úložiště ze systému souborů výchozí clusteru HDInsight.** Pokud jde o případ, je třeba změnit vytváření část skriptu prostředí Windows PowerShell součástí [vytvořit HDInsight obrázku a spuštění úlohy podregistru/Sqoop](#runjob) propojila účtu úložiště jako další úložiště účet. Pokyny najdete v tématu [Vytvoření Hadoop clusterů HDInsight][hdinsight-provision]. HDInsight obrázku potom zná přístupová klávesa účtu úložiště.

>[AZURE.NOTE] Cesta úložiště objektů Blob k datovému souboru je špatně kódovaný v soubor skriptu HiveQL. Je třeba jej aktualizovat příslušným způsobem.

**Pokud si chcete stáhnout data letech**

1. Běžte na [zdroje informací a inovativní technologii Správa Bureau s jednou variantou dopravy][rita-website].
2. Na stránce vyberte tyto hodnoty:

    <table border="1">
    <tr><th>Jméno</th><th>Hodnota</th></tr>
    <tr><td>Filtr rok</td><td>2013 </td></tr>
    <tr><td>Filtrování období</td><td>Leden</td></tr>
    <tr><td>Pole</td><td>*Rok*, *FlightDate*, *UniqueCarrier*, *Carrier*, *FlightNum*, *OriginAirportID*, *Origin*, *OriginCityName*, *OriginState*, *DestAirportID*, *cíl*, *DestCityName*, *DestState*, *DepDelayMinutes*, *ArrDelay*, *ArrDelayMinutes*, *CarrierDelay*, *WeatherDelay*, *NASDelay*, *SecurityDelay*, *LateAircraftDelay* (zrušte zaškrtnutí všech polí)</td></tr>
    </table>

3. Klikněte na **Stáhnout**.
4. Rozbalení souboru do složky **C:\Tutorials\FlightDelay\2013Data** . Každý soubor je soubor CSV a velikosti přibližně 60GB.
5.  Přejmenujte soubor na název měsíce obsahující data. Například soubor s daty leden by být s názvem *January.csv*.
6. Opakujte kroky 2 a 5 pro stažení souboru pro jednotlivá pole 12 měsíců v 2013. Budete potřebovat aspoň jeden soubor spustit kurzu.  

**Pokud chcete odeslat daty zpoždění letů k úložišti objektů Blob Azure**

1. Příprava parametry:

    <table border="1">
    <tr><th>Název proměnné</th><th>Poznámky</th></tr>
    <tr><td>$storageAccountName</td><td>Účet Azure úložiště místo, kam chcete data, která chcete nahrát.</td></tr>
    <tr><td>$blobContainerName</td><td>Kontejner objektů Blob místo, kam chcete data, která chcete nahrát.</td></tr>
    </table>
2. Otevřete Azure ISE Powershellu.
3. Vložte tento skript do podokna skript:

        [CmdletBinding()]
        Param(

            [Parameter(Mandatory=$True,
                       HelpMessage="Enter the Azure storage account name for creating a new HDInsight cluster. If the account doesn't exist, the script will create one.")]
            [String]$storageAccountName,

            [Parameter(Mandatory=$True,
                       HelpMessage="Enter the Azure blob container name for creating a new HDInsight cluster. If not specified, the HDInsight cluster name will be used.")]
            [String]$blobContainerName
        )

        #Region - Variables
        $localFolder = "C:\Tutorials\FlightDelay\2013Data"  # The source folder
        $destFolder = "tutorials/flightdelay/2013data"     #The blob name prefix for the files to be uploaded
        #EndRegion
        
        #Region - Connect to Azure subscription
        Write-Host "`nConnecting to your Azure subscription ..." -ForegroundColor Green
        try{Get-AzureRmContext}
        catch{Login-AzureRmAccount}
        #EndRegion
        
        #Region - Validate user input
        Write-Host "`nValidating the Azure Storage account and the Blob container..." -ForegroundColor Green
        # Validate the Storage account
        if (-not (Get-AzureRmStorageAccount|Where-Object{$_.StorageAccountName -eq $storageAccountName}))
        {
            Write-Host "The storage account, $storageAccountName, doesn't exist." -ForegroundColor Red
            exit
        }
        else{
            $resourceGroupName = (Get-AzureRmStorageAccount|Where-Object{$_.StorageAccountName -eq $storageAccountName}).ResourceGroupName
        }
        
        # Validate the container
        $storageAccountKey = (Get-AzureRmStorageAccountKey -StorageAccountName $storageAccountName -ResourceGroupName $resourceGroupName)[0].Value
        $storageContext = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageAccountKey
        
        if (-not (Get-AzureStorageContainer -Context $storageContext |Where-Object{$_.Name -eq $blobContainerName}))
        {
            Write-Host "The Blob container, $blobContainerName, doesn't exist" -ForegroundColor Red
            Exit
        }
        #EngRegion
        
        #Region - Copy the file from local workstation to Azure Blob storage  
        if (test-path -Path $localFolder)
        {
            foreach ($item in Get-ChildItem -Path $localFolder){
                $fileName = "$localFolder\$item"
                $blobName = "$destFolder/$item"
        
                Write-Host "Copying $fileName to $blobName" -ForegroundColor Green
        
                Set-AzureStorageBlobContent -File $fileName -Container $blobContainerName -Blob $blobName -Context $storageContext
            }
        }
        else
        {
            Write-Host "The source folder on the workstation doesn't exist" -ForegroundColor Red
        }
        
        # List the uploaded files on HDInsight
        Get-AzureStorageBlob -Container $blobContainerName  -Context $storageContext -Prefix $destFolder
        #EndRegion




4. Stisknutím klávesy **F5** následujícím způsobem.

Pokud se rozhodnete sdělit nám jinou metodu pro odeslání souboru, přejděte prosím zkontrolujte, že cesta k souboru je výukové programy pro/flightdelay/data. Syntaxe pro přístup k souborům je:

    wasbs://<ContainerName>@<StorageAccountName>.blob.core.windows.net/tutorials/flightdelay/data

Virtuální složka, kterou jste vytvořili při odeslání soubory, které se cestu výukové programy pro/flightdelay/data. Ověřte, jestli jsou 12 souborů, jeden za každý měsíc.

>[AZURE.NOTE] Musíte aktualizační dotaz podregistru číst ze na nové místo.

> Buď musíte nakonfigurovat přístupová oprávnění kontejneru k veřejné nebo účtu úložiště svázat clusteru HDInsight. Řetězec dotazu podregistru jinak, nebudou mít přístup k datovým souborům.

---
##<a id="appendix-b"></a>Dodatek B – vytvoření a odeslání HiveQL skriptem

Pomocí prostředí PowerShell Azure, můžete spustit několik výrokových HiveQL jeden po druhém nebo balíček HiveQL údajů do soubor skriptu. V této části se dozvíte, jak vytvořit skript HiveQL a nahrajte skript k úložišti objektů Blob Azure pomocí prostředí PowerShell Azure. Podregistru vyžaduje skripty HiveQL uložené v úložišti objektů Blob Azure.

Skript HiveQL bude postupujte takto:

1. **Odstranit tabulku delays_raw**, v případě tabulky už existuje.
2. **Vytvořit externí tabulku podregistru delays_raw** přejdete umístění v úložišti objektů Blob se soubory, zpoždění letů. Tento dotaz Určuje, že jsou pole odděleny "," a že řádky ukončí "\n". Představuje problém, pokud hodnoty polí obsahují středníky, protože podregistru nedají odlišit to znamená, že je oddělovač a ten, který je součástí hodnotu pole (což je případ hodnoty v polích ORIGIN\_Město\_název a cíl\_Město\_název). Při řešení to, vytvoří dotaz TEMP sloupce pro uložení data, která je nesprávně rozdělit na sloupce.  
3. **Zpoždění tabulku**, v případě tabulky už existuje.
4. **Vytvoření tabulky zpoždění**. Je vhodné vyčištění data před další zpracování. Tento dotaz vytvoří nová tabulka, *zpoždění*z tabulky delays_raw. Všimněte si, že se nezkopírují TEMP sloupců (jak jsme zmínili dříve) a funkce **podřetězec** používá znak uzavírající odeberete data.
5. **Výpočet průměru počasí zpoždění a skupiny výsledky podle názvu města.** Vypíše také výsledky k úložišti objektů Blob. Všimněte si, že dotaz bude apostrofy odebrání data a vyloučí řádky, kde **weather_delay** hodnotu null. Je to nutné, protože Sqoop používá dál v tomto kurzu nezpracuje jsou tyto hodnoty řádně ve výchozím nastavení.

Úplný seznam příkazů HiveQL naleznete v tématu [Podregistru Data Definition Language][hadoop-hiveql]. Jednotlivé příkazy HiveQL musí ukončit středníkem.

**Aby se vytvořil soubor skriptu HiveQL**

1. Příprava parametry:

    <table border="1">
    <tr><th>Název proměnné</th><th>Poznámky</th></tr>
    <tr><td>$storageAccountName</td><td>Účet Azure úložiště místo, kam chcete nahrát skript HiveQL.</td></tr>
    <tr><td>$blobContainerName</td><td>Kontejner objektů Blob místo, kam chcete nahrát skript HiveQL.</td></tr>
    </table>
2. Otevřete Azure ISE Powershellu.

3. Zkopírujte a vložte následující skript do podokna skript:

        [CmdletBinding()]
        Param(

            # Azure Blob storage variables
            [Parameter(Mandatory=$True,
                       HelpMessage="Enter the Azure storage account name for creating a new HDInsight cluster. If the account doesn't exist, the script will create one.")]
            [String]$storageAccountName,

            [Parameter(Mandatory=$True,
                       HelpMessage="Enter the Azure blob container name for creating a new HDInsight cluster. If not specified, the HDInsight cluster name will be used.")]
            [String]$blobContainerName
        )

        #region - Define variables
        # Treat all errors as terminating
        $ErrorActionPreference = "Stop"
        
        # The HiveQL script file is exported as this file before it's uploaded to Blob storage
        $hqlLocalFileName = "e:\tutorials\flightdelay\flightdelays.hql"
        
        # The HiveQL script file will be uploaded to Blob storage as this blob name
        $hqlBlobName = "tutorials/flightdelay/flightdelays.hql"
        
        # These two constants are used by the HiveQL script file
        #$srcDataFolder = "tutorials/flightdelay/data"
        $dstDataFolder = "/tutorials/flightdelay/output"
        #endregion

        #Region - Connect to Azure subscription
        Write-Host "`nConnecting to your Azure subscription ..." -ForegroundColor Green
        try{Get-AzureRmContext}
        catch{Login-AzureRmAccount}
        #EndRegion
        
        #Region - Validate user input
        Write-Host "`nValidating the Azure Storage account and the Blob container..." -ForegroundColor Green
        # Validate the Storage account
        if (-not (Get-AzureRmStorageAccount|Where-Object{$_.StorageAccountName -eq $storageAccountName}))
        {
            Write-Host "The storage account, $storageAccountName, doesn't exist." -ForegroundColor Red
            exit
        }
        else{
            $resourceGroupName = (Get-AzureRmStorageAccount|Where-Object{$_.StorageAccountName -eq $storageAccountName}).ResourceGroupName
        }
        
        # Validate the container
        $storageAccountKey = (Get-AzureRmStorageAccountKey -StorageAccountName $storageAccountName -ResourceGroupName $resourceGroupName)[0].Value
        $storageContext = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageAccountKey
        
        if (-not (Get-AzureStorageContainer -Context $storageContext |Where-Object{$_.Name -eq $blobContainerName}))
        {
            Write-Host "The Blob container, $blobContainerName, doesn't exist" -ForegroundColor Red
            Exit
        }
        #EngRegion
        
        #region - Validate the file and file path
        
        # Check if a file with the same file name already exists on the workstation
        Write-Host "`nvalidating the folder structure on the workstation for saving the HQL script file ..."  -ForegroundColor Green
        if (test-path $hqlLocalFileName){
        
            $isDelete = Read-Host 'The file, ' $hqlLocalFileName ', exists.  Do you want to overwirte it? (Y/N)'
        
            if ($isDelete.ToLower() -ne "y")
            {
                Exit
            }
        }
        
        # Create the folder if it doesn't exist
        $folder = split-path $hqlLocalFileName
        if (-not (test-path $folder))
        {
            Write-Host "`nCreating folder, $folder ..." -ForegroundColor Green
        
            new-item $folder -ItemType directory  
        }
        #end region
        
        #region - Write the Hive script into a local file
        Write-Host "`nWriting the Hive script into a file on your workstation ..." `
                    -ForegroundColor Green
        
        $hqlDropDelaysRaw = "DROP TABLE delays_raw;"
        
        $hqlCreateDelaysRaw = "CREATE EXTERNAL TABLE delays_raw (" +
                "YEAR string, " +
                "FL_DATE string, " +
                "UNIQUE_CARRIER string, " +
                "CARRIER string, " +
                "FL_NUM string, " +
                "ORIGIN_AIRPORT_ID string, " +
                "ORIGIN string, " +
                "ORIGIN_CITY_NAME string, " +
                "ORIGIN_CITY_NAME_TEMP string, " +
                "ORIGIN_STATE_ABR string, " +
                "DEST_AIRPORT_ID string, " +
                "DEST string, " +
                "DEST_CITY_NAME string, " +
                "DEST_CITY_NAME_TEMP string, " +
                "DEST_STATE_ABR string, " +
                "DEP_DELAY_NEW float, " +
                "ARR_DELAY_NEW float, " +
                "CARRIER_DELAY float, " +
                "WEATHER_DELAY float, " +
                "NAS_DELAY float, " +
                "SECURITY_DELAY float, " +
                "LATE_AIRCRAFT_DELAY float) " +
            "ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' " +
            "LINES TERMINATED BY '\n' " +
            "STORED AS TEXTFILE " +
            "LOCATION 'wasbs://flightdelay@hditutorialdata.blob.core.windows.net/2013Data';"
        
        $hqlDropDelays = "DROP TABLE delays;"
        
        $hqlCreateDelays = "CREATE TABLE delays AS " +
            "SELECT YEAR AS year, " +
                "FL_DATE AS flight_date, " +
                "substring(UNIQUE_CARRIER, 2, length(UNIQUE_CARRIER) -1) AS unique_carrier, " +
                "substring(CARRIER, 2, length(CARRIER) -1) AS carrier, " +
                "substring(FL_NUM, 2, length(FL_NUM) -1) AS flight_num, " +
                "ORIGIN_AIRPORT_ID AS origin_airport_id, " +
                "substring(ORIGIN, 2, length(ORIGIN) -1) AS origin_airport_code, " +
                "substring(ORIGIN_CITY_NAME, 2) AS origin_city_name, " +
                "substring(ORIGIN_STATE_ABR, 2, length(ORIGIN_STATE_ABR) -1)  AS origin_state_abr, " +
                "DEST_AIRPORT_ID AS dest_airport_id, " +
                "substring(DEST, 2, length(DEST) -1) AS dest_airport_code, " +
                "substring(DEST_CITY_NAME,2) AS dest_city_name, " +
                "substring(DEST_STATE_ABR, 2, length(DEST_STATE_ABR) -1) AS dest_state_abr, " +
                "DEP_DELAY_NEW AS dep_delay_new, " +
                "ARR_DELAY_NEW AS arr_delay_new, " +
                "CARRIER_DELAY AS carrier_delay, " +
                "WEATHER_DELAY AS weather_delay, " +
                "NAS_DELAY AS nas_delay, " +
                "SECURITY_DELAY AS security_delay, " +
                "LATE_AIRCRAFT_DELAY AS late_aircraft_delay " +
            "FROM delays_raw;"
        
        $hqlInsertLocal = "INSERT OVERWRITE DIRECTORY '$dstDataFolder' " +
            "SELECT regexp_replace(origin_city_name, '''', ''), " +
                "avg(weather_delay) " +
            "FROM delays " +
            "WHERE weather_delay IS NOT NULL " +
            "GROUP BY origin_city_name;"
        
        $hqlScript = $hqlDropDelaysRaw + $hqlCreateDelaysRaw + $hqlDropDelays + $hqlCreateDelays + $hqlInsertLocal
        
        $hqlScript | Out-File $hqlLocalFileName -Encoding ascii -Force
        #endregion
        
        #region - Upload the Hive script to the default Blob container
        Write-Host "`nUploading the Hive script to the default Blob container ..." -ForegroundColor Green
        
        # Create a storage context object
        $storageAccountKey = (Get-AzureRmStorageAccountKey -StorageAccountName $storageAccountName -ResourceGroupName $resourceGroupName)[0].Value
        $destContext = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageAccountKey
        
        # Upload the file from local workstation to Blob storage
        Set-AzureStorageBlobContent -File $hqlLocalFileName -Container $blobContainerName -Blob $hqlBlobName -Context $destContext
        #endregion
        
        Write-host "`nEnd of the PowerShell script" -ForegroundColor Green

    Tady jsou proměnných používaných v skript:

    - **$hqlLocalFileName** - skript uloží soubor skriptu HiveQL místně před nahráním k úložišti objektů Blob. Toto je název souboru. Výchozí hodnota je <u>C:\tutorials\flightdelay\flightdelays.hql</u>.
    - **$hqlBlobName** – to je HiveQL názvu souboru skriptu objektů blob použité v úložišti objektů Blob Azure. Výchozí hodnota je tutorials/flightdelay/flightdelays.hql. Protože soubor bude přímo k úložišti objektů Blob Azure napsali, není "/" na začátku názvu objektů blob. Pokud chcete získat přístup k souboru v úložišti objektů Blob, musíte přidat "/" na začátku názvu souboru.
    - **$srcDataFolder** a **$dstDataFolder** - = "výukové programy pro/flightdelay/data" = "výukové programy pro/flightdelay/výstup"


---
##<a id="appendix-c"></a>Dodatek C - Příprava výstupu projektu Sqoop databáze Azure SQL
**Příprava databáze SQL (sloučit to se skriptem Sqoop)**

1. Příprava parametry:

    <table border="1">
    <tr><th>Název proměnné</th><th>Poznámky</th></tr>
    <tr><td>$sqlDatabaseServerName</td><td>Název databázového serveru Azure SQL. Zadejte, co vytvořit nový server.</td></tr>
    <tr><td>$sqlDatabaseUsername</td><td>Přihlašovací jméno pro databázový server Azure SQL. Pokud $sqlDatabaseServerName existující server, přihlašovací jméno a heslo pro přihlášení slouží k ověření se serverem. V opačném případě slouží k vytvoření nového serveru.</td></tr>
    <tr><td>$sqlDatabasePassword</td><td>Přihlašovací heslo pro databázový server Azure SQL.</td></tr>
    <tr><td>$sqlDatabaseLocation</td><td>Tato hodnota se používá pouze v případě, že vytváříte nový Azure databázovým serverem.</td></tr>
    <tr><td>$sqlDatabaseName</td><td>Databáze SQL slouží k vytvoření tabulky AvgDelays Sqoop projektu. Necháte prázdné vytvoří databázi s názvem HDISqoop. Název tabulky výstupu projektu Sqoop je AvgDelays. </td></tr>
    </table>
2. Otevřete Azure ISE Powershellu.
3. Zkopírujte a vložte následující skript do podokna skript:

        [CmdletBinding()]
        Param(
        
            # Azure Resource group variables
            [Parameter(Mandatory=$True,
                    HelpMessage="Enter the Azure resource group name. It will be created if it doesn't exist.")]
            [String]$resourceGroupName,
        
            # SQL database server variables
            [Parameter(Mandatory=$True,
                    HelpMessage="Enter the Azure SQL Database Server Name. It will be created if it doesn't exist.")]
            [String]$sqlDatabaseServer, 
        
            [Parameter(Mandatory=$True,
                    HelpMessage="Enter the Azure SQL Database admin user.")]
            [String]$sqlDatabaseLogin,
        
            [Parameter(Mandatory=$True,
                    HelpMessage="Enter the Azure SQL Database admin user password.")]
            [String]$sqlDatabasePassword,
        
            [Parameter(Mandatory=$True,
                    HelpMessage="Enter the region to create the Database in.")]
            [String]$sqlDatabaseLocation,   #For example, West US.
        
            # SQL database variables
            [Parameter(Mandatory=$True,
                    HelpMessage="Enter the database name. It will be created if it doesn't exist.")]
            [String]$sqlDatabaseName # specify the database name if you have one created. Otherwise use "" to have the script create one for you.
        )
        
        # Treat all errors as terminating
        $ErrorActionPreference = "Stop"
        
        #region - Constants and variables
        
        # IP address REST service used for retrieving external IP address and creating firewall rules
        [String]$ipAddressRestService = "http://bot.whatismyipaddress.com"
        [String]$fireWallRuleName = "FlightDelay"
        
        # SQL database variables
        [String]$sqlDatabaseMaxSizeGB = 10
        
        #SQL query string for creating AvgDelays table
        [String]$sqlDatabaseTableName = "AvgDelays"
        [String]$sqlCreateAvgDelaysTable = " CREATE TABLE [dbo].[$sqlDatabaseTableName](
                    [origin_city_name] [nvarchar](50) NOT NULL,
                    [weather_delay] float,
                CONSTRAINT [PK_$sqlDatabaseTableName] PRIMARY KEY CLUSTERED
                (
                    [origin_city_name] ASC
                )
                )"
        #endregion
        
        #Region - Connect to Azure subscription
        Write-Host "`nConnecting to your Azure subscription ..." -ForegroundColor Green
        try{Get-AzureRmContext}
        catch{Login-AzureRmAccount}
        #EndRegion
        
        #region - Create and validate Azure resouce group
        try{
            Get-AzureRmResourceGroup -Name $resourceGroupName
        }
        catch{
            New-AzureRmResourceGroup -Name $resourceGroupName -Location $sqlDatabaseLocation
        }
        
        #EndRegion
        
        #region - Create and validate Azure SQL database server
        try{
            Get-AzureRmSqlServer -ServerName $sqlDatabaseServer -ResourceGroupName $resourceGroupName}
        catch{
            Write-Host "`nCreating SQL Database server ..."  -ForegroundColor Green
        
            $sqlDatabasePW = ConvertTo-SecureString -String $sqlDatabasePassword -AsPlainText -Force
            $credential = New-Object System.Management.Automation.PSCredential($sqlDatabaseLogin,$sqlDatabasePW)
        
            $sqlDatabaseServer = (New-AzureRmSqlServer -ResourceGroupName $resourceGroupName -ServerName $sqlDatabaseServer -SqlAdministratorCredentials $credential -Location $sqlDatabaseLocation).ServerName
            Write-Host "`tThe new SQL database server name is $sqlDatabaseServer." -ForegroundColor Cyan
        
            Write-Host "`nCreating firewall rule, $fireWallRuleName ..." -ForegroundColor Green
            $workstationIPAddress = Invoke-RestMethod $ipAddressRestService
            New-AzureRmSqlServerFirewallRule -ResourceGroupName $resourceGroupName -ServerName $sqlDatabaseServer -FirewallRuleName "$fireWallRuleName-workstation" -StartIpAddress $workstationIPAddress -EndIpAddress $workstationIPAddress
        
            #To allow other Azure services to access the server add a firewall rule and set both the StartIpAddress and EndIpAddress to 0.0.0.0. Note that this allows Azure traffic from any Azure subscription to access the server.
            New-AzureRmSqlServerFirewallRule -ResourceGroupName $resourceGroupName -ServerName $sqlDatabaseServer -FirewallRuleName "$fireWallRuleName-Azureservices" -StartIpAddress "0.0.0.0" -EndIpAddress "0.0.0.0"
        }
        
        #endregion
        
        #region - Create and validate Azure SQL database
        
        try {
            Get-AzureRmSqlDatabase -ResourceGroupName $resourceGroupName -ServerName $sqlDatabaseServer -DatabaseName $sqlDatabaseName
        }
        catch {
            Write-Host "`nCreating SQL Database, $sqlDatabaseName ..."  -ForegroundColor Green
            New-AzureRMSqlDatabase -ResourceGroupName $resourceGroupName -ServerName $sqlDatabaseServer -DatabaseName $sqlDatabaseName -Edition "Standard" -RequestedServiceObjectiveName "S1"
        }
        
        #endregion
        
        #region -  Execute an SQL command to create the AvgDelays table
        
        Write-Host "`nCreating SQL Database table ..."  -ForegroundColor Green
        $conn = New-Object System.Data.SqlClient.SqlConnection
        $conn.ConnectionString = "Data Source=$sqlDatabaseServer.database.windows.net;Initial Catalog=$sqlDatabaseName;User ID=$sqlDatabaseLogin;Password=$sqlDatabasePassword;Encrypt=true;Trusted_Connection=false;"
        $conn.open()
        $cmd = New-Object System.Data.SqlClient.SqlCommand
        $cmd.connection = $conn
        $cmd.commandtext = $sqlCreateAvgDelaysTable
        $cmd.executenonquery()
        
        $conn.close()
        
        Write-host "`nEnd of the PowerShell script" -ForegroundColor Green

    >[AZURE.NOTE] Skript používá representational stavová služba převodu (REST), http://bot.whatismyipaddress.com, k načítání externích IP adresu. IP adresu slouží k vytváření pravidla brány firewall pro databázi SQL serveru.  

    Tady jsou některé proměnné ve skriptu používá:

    - **$ipAddressRestService** - výchozí hodnota je http://bot.whatismyipaddress.com. Je veřejnou IP adresu služba REST usnadňující externí IP adresu. Chcete-li můžete použít jiné služby. Externí IP adresu načtené prostřednictvím služby se použijí k vytvoření pravidla brány firewall pro databázový server Azure SQL, tak, že máte přístup k databázi v počítači (pomocí skriptu prostředí Windows PowerShell).
    - **$fireWallRuleName** – to je název pravidla brány firewall pro databázový server Azure SQL. Výchozí název je <u>FlightDelay</u>. Chcete-li můžete ji přejmenovat.
    - **$sqlDatabaseMaxSizeGB** – tato hodnota se používá pouze v případě, že vytváříte novou databázi serveru Azure SQL. Výchozí hodnota je 10GB. 10GB stačí pro účely tohoto návodu.
    - **$sqlDatabaseName** – tato hodnota se používá pouze v případě, že vytváříte novou databázi Azure SQL. Výchozí hodnota je HDISqoop. Pokud přejmenujete, musíte aktualizovat skriptu prostředí Windows PowerShell Sqoop příslušným způsobem.

4. Stisknutím klávesy **F5** následujícím způsobem.
5. Ověření výstupu skriptu. Ujistěte se, že skript proběhla úspěšně.

##<a id="nextsteps"></a>Další kroky
Teď víte, jak nahrát soubor k úložišti objektů Blob Azure, jak k naplnění tabulku podregistru pomocí dat z úložiště objektů Blob Azure, k tomu podregistru dotazů a jak používat Sqoop exportovat data z HDFS k databázi Azure SQL. Další informace naleznete v následujících článcích:

* [Začínáme s HDInsight][hdinsight-get-started]
* [Použití podregistru s HDInsight][hdinsight-use-hive]
* [Použití Oozie s HDInsight][hdinsight-use-oozie]
* [Použití Sqoop s HDInsight][hdinsight-use-sqoop]
* [Použití Prasátko s HDInsight][hdinsight-use-pig]
* [Můžete vyvíjet aplikace Java MapReduce pro HDInsight][hdinsight-develop-mapreduce]



[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/


[rita-website]: http://www.transtats.bts.gov/DL_SelectFields.asp?Table_ID=236&DB_Short_Name=On-Time
[powershell-install-configure]: powershell-install-configure.md

[hdinsight-use-oozie]: hdinsight-use-oozie.md
[hdinsight-use-hive]: hdinsight-use-hive.md
[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md
[hdinsight-use-sqoop]: hdinsight-use-sqoop.md
[hdinsight-use-pig]: hdinsight-use-pig.md
[hdinsight-develop-mapreduce]: hdinsight-develop-deploy-java-mapreduce-linux.md

[hadoop-hiveql]: https://cwiki.apache.org/confluence/display/Hive/LanguageManual+DDL
[hadoop-shell-commands]: http://hadoop.apache.org/docs/r0.18.3/hdfs_shell.html

[technetwiki-hive-error]: http://social.technet.microsoft.com/wiki/contents/articles/23047.hdinsight-hive-error-unable-to-rename.aspx

[image-hdi-flightdelays-avgdelays-dataset]: ./media/hdinsight-analyze-flight-delay-data/HDI.FlightDelays.AvgDelays.DataSet.png
[img-hdi-flightdelays-run-hive-job-output]: ./media/hdinsight-analyze-flight-delay-data/HDI.FlightDelays.RunHiveJob.Output.png
[img-hdi-flightdelays-flow]: ./media/hdinsight-analyze-flight-delay-data/HDI.FlightDelays.Flow.png
