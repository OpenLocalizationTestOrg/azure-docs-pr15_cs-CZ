<properties 
   pageTitle="Začínáme s Azure dat jezera technologie pro analýzu pomocí prostředí PowerShell Azure | Azure" 
   description="Zjistěte, jak pomocí Powershellu Azure vytvořit účet úložiště jezera dat, vytvořit analýzy dat jezera úlohy pomocí U SQL a odesílat úkoly. " 
   services="data-lake-analytics" 
   documentationCenter="" 
   authors="edmacauley" 
   manager="jhubbard" 
   editor="cgronlun"/>
 
<tags
   ms.service="data-lake-analytics"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data" 
   ms.date="09/21/2016"
   ms.author="edmaca"/>

# <a name="tutorial-get-started-with-azure-data-lake-analytics-using-azure-powershell"></a>Kurz: Začínáme s Azure dat jezera technologie pro analýzu pomocí prostředí PowerShell Azure

[AZURE.INCLUDE [get-started-selector](../../includes/data-lake-analytics-selector-get-started.md)]

Zjistěte, jak pomocí Powershellu Azure vytvoření účtů jezera analýzy dat Azure, definujte úlohy jezera analýzy dat v [U SQL](data-lake-analytics-u-sql-get-started.md)a odesílat úlohy dat jezera analytický účty. Další informace o jezera analýzy dat najdete v tématu [Přehled analýzy jezera dat Azure](data-lake-analytics-overview.md).

V tomto kurzu se dají projektu, který bude číst na kartě soubor hodnot (TSV) oddělené a převede ho do souboru hodnoty oddělené čárkami (CSV). Projít stejné kurz používání jiných podporovaných nástrojů, klikněte na karty v horní v této části.

##<a name="prerequisites"></a>Zjistit předpoklady pro

Před zahájením tohoto kurzu, musíte mít takto:

- **Azure předplatného**. Viz [získání Azure bezplatnou zkušební verzi](https://azure.microsoft.com/pricing/free-trial/).
- **Pracoviště se Azure Powershellu**. Přečtěte si, [Jak nainstalovat a nakonfigurovat Azure Powershellu](../powershell-install-configure.md).
    
##<a name="create-data-lake-analytics-account"></a>Vytvoření účtu jezera analýzy dat

Před spuštěním všechny úlohy musíte mít účet analýzy dat jezera. Vytvoření účtu jezera analýzy dat, je nutné zadat takto:

- **Pole Skupina zdroje Azure**: je třeba vytvořit účet jezera analýzy dat A ve skupině Azure prostředků. [Správce prostředků Azure](../azure-resource-manager/resource-group-overview.md) umožňuje práce se zdroji v aplikaci jako skupinu. Můžete nasadit, aktualizace nebo odstranění všechny zdroje pro aplikaci v operaci jediné, koordinovaný.  

    Vytvořit výčet skupiny zdrojů ve vašem předplatném:
    
        Get-AzureRmResourceGroup
    
    Vytvoření nové skupiny prostředků:

        New-AzureRmResourceGroup `
            -Name "<Your resource group name>" `
            -Location "<Azure Data Center>" # For example, "East US 2"

- **Název účtu technologie pro analýzu dat jezera**
- **Umístění**: jeden z Azure datacentrech, které podporuje jezera analýzy dat.
- **Výchozí Data jezera účtu**: každý účet analýzy dat jezera má výchozí účet jezera Data.

    Vytvoření nového účtu jezera dat:

        New-AzureRmDataLakeStoreAccount `
            -ResourceGroupName "<Your Azure resource group name>" `
            -Name "<Your Data Lake account name>" `
            -Location "<Azure Data Center>"  # For example, "East US 2"

    > [AZURE.NOTE] Název účtu dat jezera musí obsahovat pouze malá písmena a číslice.



**Vytvoření účtu jezera analýzy dat**

1. Otevření prostředí PowerShell ISE z počítači Windows.
2. Spusťte tento skript:

        $resourceGroupName = "<ResourceGroupName>"
        $dataLakeStoreName = "<DataLakeAccountName>"
        $dataLakeAnalyticsName = "<DataLakeAnalyticsAccountName>"
        $location = "East US 2"
        
        Write-Host "Create a resource group ..." -ForegroundColor Green
        New-AzureRmResourceGroup `
            -Name  $resourceGroupName `
            -Location $location
        
        Write-Host "Create a Data Lake account ..."  -ForegroundColor Green
        New-AzureRmDataLakeStoreAccount `
            -ResourceGroupName $resourceGroupName `
            -Name $dataLakeStoreName `
            -Location $location 
        
        Write-Host "Create a Data Lake Analytics account ..."  -ForegroundColor Green
        New-AzureRmDataLakeAnalyticsAccount `
            -Name $dataLakeAnalyticsName `
            -ResourceGroupName $resourceGroupName `
            -Location $location `
            -DefaultDataLake $dataLakeStoreName
        
        Write-Host "The newly created Data Lake Analytics account ..."  -ForegroundColor Green
        Get-AzureRmDataLakeAnalyticsAccount `
            -ResourceGroupName $resourceGroupName `
            -Name $dataLakeAnalyticsName  

##<a name="upload-data-to-data-lake"></a>Odeslání dat do jezera dat

V tomto kurzu zpracuje některé protokoly vyhledávání.  Protokol hledání můžete uložené v úložišti jezera dat nebo úložiště objektů Blob Azure. 

Ukázkový soubor protokolu hledání byla zkopírována do kontejneru veřejné objektů Blob Azure. Použijte tento skript Powershellu ke stažení souboru na počítači a pak nahrajte soubor do výchozího účtu úložiště jezera dat analýzy dat jezera účtu.

    $dataLakeStoreName = "<The default Data Lake Store account name>"
    
    $localFolder = "C:\Tutorials\Downloads\" # A temp location for the file. 
    $storageAccount = "adltutorials"  # Don't modify this value.
    $container = "adls-sample-data"  #Don't modify this value.

    # Create the temp location  
    New-Item -Path $localFolder -ItemType Directory -Force 

    # Download the sample file from Azure Blob storage
    $context = New-AzureStorageContext -StorageAccountName $storageAccount -Anonymous
    $blobs = Azure\Get-AzureStorageBlob -Container $container -Context $context
    $blobs | Get-AzureStorageBlobContent -Context $context -Destination $localFolder

    # Upload the file to the default Data Lake Store account    
    Import-AzureRmDataLakeStoreItem -AccountName $dataLakeStoreName -Path $localFolder"SearchLog.tsv" -Destination "/Samples/Data/SearchLog.tsv"

Tento skript Powershellu ukazuje, jak získat výchozí úložiště jezera dat název účtu jezera analýzy dat:


    $resourceGroupName = "<ResourceGroupName>"
    $dataLakeAnalyticsName = "<DataLakeAnalyticsAccountName>"
    $dataLakeStoreName = (Get-AzureRmDataLakeAnalyticsAccount -ResourceGroupName $resourceGroupName -Name $dataLakeAnalyticsName).Properties.DefaultDataLakeAccount
    echo $dataLakeStoreName

>[AZURE.NOTE] Na portálu Azure poskytuje uživatelské rozhraní zkopírujte ukázkové datové soubory do výchozí úložiště jezera dat účet. Pokyny najdete v tématu [Začínáme s Azure dat jezera technologie pro analýzu portálu Azure](data-lake-analytics-get-started-portal.md#upload-data-to-the-default-data-lake-store-account).

Analýzy dat jezera taky přístup k úložišti objektů Blob Azure.  Odeslání dat k úložišti objektů Blob Azure, najdete v článku [použití Azure s úložištěm Azure](../storage/storage-powershell-guide-full.md).

##<a name="submit-data-lake-analytics-jobs"></a>Odeslat úlohy jezera analýzy dat

Úlohy jezera analýzy dat jsou napsané v jazyce U SQL. Další informace o U SQL, najdete v článku [Začínáme s jazyka U SQL](data-lake-analytics-u-sql-get-started.md) a funkce pro [odkazy jazyka U SQL](http://go.microsoft.com/fwlink/?LinkId=691348).

**Vytvořit skript úlohy jezera analýzy dat**

- Vytvoření textového souboru s následující skript U SQL a uložte textový soubor na počítači:

        @searchlog =
            EXTRACT UserId          int,
                    Start           DateTime,
                    Region          string,
                    Query           string,
                    Duration        int?,
                    Urls            string,
                    ClickedUrls     string
            FROM "/Samples/Data/SearchLog.tsv"
            USING Extractors.Tsv();
        
        OUTPUT @searchlog   
            TO "/Output/SearchLog-from-Data-Lake.csv"
        USING Outputters.Csv();

    Tento skript U SQL přečte souboru zdroje dat pomocí **Extractors.Tsv()**a vytvoří soubor csv pomocí **Outputters.Csv()**. 
    
    Neupravujte dvě cesty, pokud zkopírujete zdrojového souboru do jiného umístění.  Jezera analýzy dat vytvoří složku výstupu, pokud ho neexistuje.
    
    Je jednodušší použít relativní cesty pro soubory uložené ve výchozí data jezera účty. Můžete taky použít absolutní cesty.  Příklad 
    
        adl://<Data LakeStorageAccountName>.azuredatalakestore.net:443/Samples/Data/SearchLog.tsv
        
    Je nutné použít absolutní cesty pro přístup k souborům v propojené účty úložiště.  Syntaxe pro soubory uložené v propojený klient Azure úložiště je:
    
        wasb://<BlobContainerName>@<StorageAccountName>.blob.core.windows.net/Samples/Data/SearchLog.tsv

    >[AZURE.NOTE] Azure kontejneru objektů Blob s objekty BLOB veřejné nebo veřejné kontejnery přístupová oprávnění aktuálně nepodporuje.    
    
    
**Odeslání projektu**

1. Otevření prostředí PowerShell ISE z počítači Windows.
2. Spusťte tento skript:

        $dataLakeAnalyticsName = "<DataLakeAnalyticsAccountName>"
        $usqlScript = "c:\tutorials\data-lake-analytics\copyFile.usql"
        
        $job = Submit-AzureRmDataLakeAnalyticsJob -Name "convertTSVtoCSV" -AccountName $dataLakeAnalyticsName –ScriptPath $usqlScript 

        Wait-AdlJob -Account $dataLakeAnalyticsName -JobId $job.JobId

        Get-AzureRmDataLakeAnalyticsJob -AccountName $dataLakeAnalyticsName -JobId $job.JobId

    V skriptu uložený soubor skriptu U SQL na c:\tutorials\data-lake-analytics\copyFile.usql. Podle toho aktualizujte cesta k souboru.
 
Po dokončení projektu, můžete pomocí těchto rutin zobrazíte seznam souboru a budou moct soubor stáhnout:
    
    $resourceGroupName = "<Resource Group Name>"
    $dataLakeAnalyticName = "<Data Lake Analytic Account Name>"
    $destFile = "C:\tutorials\data-lake-analytics\SearchLog-from-Data-Lake.csv"
    
    $dataLakeStoreName = (Get-AzureRmDataLakeAnalyticsAccount -ResourceGroupName $resourceGroupName -Name $dataLakeAnalyticName).Properties.DefaultDataLakeAccount
    
    Get-AzureRmDataLakeStoreChildItem -AccountName $dataLakeStoreName -path "/Output"
    
    Export-AzureRmDataLakeStoreItem -AccountName $dataLakeStoreName -Path "/Output/SearchLog-from-Data-Lake.csv" -Destination $destFile

## <a name="see-also"></a>Viz taky

- Stejný kurz pomocí dalších nástrojů zobrazíte kliknutím na kartu voliče v horní části stránky.
- Složitější dotaz najdete v tématu [protokoly analyzovat webu pomocí analýzy jezera dat Azure](data-lake-analytics-analyze-weblogs.md).
- Začínáme s vývojem aplikací U SQL, najdete v článku [pomocí nástroje jezera datového Visual Studio skripty vyvíjet U-SQL](data-lake-analytics-data-lake-tools-get-started.md).
- Další U SQL najdete v tématu [Začínáme s jazykem dat Azure jezera analýzy U-SQL](data-lake-analytics-u-sql-get-started.md).
- Úlohy správy najdete v článku [Správa Azure dat jezera analýzy pomocí portálu Azure](data-lake-analytics-manage-use-portal.md).
- Získejte přehled o jezera analýzy dat, najdete v článku [Přehled analýzy jezera dat Azure](data-lake-analytics-overview.md).

