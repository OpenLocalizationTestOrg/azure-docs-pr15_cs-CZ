<properties 
   pageTitle="Správa Azure dat jezera technologie pro analýzu pomocí prostředí PowerShell Azure | Azure" 
   description="Naučte se spravovat jezera analýzy dat projektů, zdrojů dat, uživatelé. " 
   services="data-lake-analytics" 
   documentationCenter="" 
   authors="edmacauley" 
   manager="jhubbard" 
   editor="cgronlun"/>
 
<tags
   ms.service="data-lake-analytics"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data" 
   ms.date="05/16/2016"
   ms.author="edmaca"/>

# <a name="manage-azure-data-lake-analytics-using-azure-powershell"></a>Správa Azure dat jezera technologie pro analýzu pomocí prostředí PowerShell Azure

[AZURE.INCLUDE [manage-selector](../../includes/data-lake-analytics-selector-manage.md)]

Naučte se spravovat účty jezera analýzy dat Azure, zdrojů dat, uživatelů a úlohy pomocí prostředí PowerShell Azure. Správa témat pomocí dalších nástrojů zobrazíte kliknutím na nahoře vyberte kartu.

**Zjistit předpoklady pro**

Před zahájením tohoto kurzu, musíte mít takto:

- **Azure předplatného**. Viz [získání Azure bezplatnou zkušební verzi](https://azure.microsoft.com/pricing/free-trial/).


<!-- ################################ -->
<!-- ################################ -->


##<a name="install-azure-powershell-10-or-greater"></a>Instalace Azure PowerShell 1.0 nebo vyšší

Naleznete v části základní [Pomocí prostředí PowerShell Azure pomocí Správce prostředků Azure](powershell-azure-resource-manager.md#prerequisites).
    
## <a name="manage-accounts"></a>Správa účtů

Před spuštěním všechny úlohy jezera analýzy dat, musíte mít účet analýzy dat jezera. Na rozdíl od Azure HDInsight není zaplatit účet analýzy když neběží projektu.  Pouze zaplatit čas, kdy běží projektu.  Další informace najdete v tématu [Přehled technologie pro analýzu dat jezera Azure](data-lake-analytics-overview.md).  

###<a name="create-accounts"></a>Vytvoření účtů

    $resourceGroupName = "<ResourceGroupName>"
    $dataLakeStoreName = "<DataLakeAccountName>"
    $dataLakeAnalyticsAccountName = "<DataLakeAnalyticsAccountName>"
    $location = "<Microsoft Data Center>"
    
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
        -Name $dataLakeAnalyticsAccountName `
        -ResourceGroupName $resourceGroupName `
        -Location $location `
        -DefaultDataLake $dataLakeStoreName
    
    Write-Host "The newly created Data Lake Analytics account ..."  -ForegroundColor Green
    Get-AzureRmDataLakeAnalyticsAccount `
        -ResourceGroupName $resourceGroupName `
        -Name $dataLakeAnalyticsAccountName  

Můžete taky použít šablonu Azure pole Skupina zdroje. Šablona pro vytváření jezera analýzy dat a závislá účet úložiště jezera dat je v [dodatku A](#appendix-a). Šablonu uložte do souboru se šablonou .json a potom použijte tento skript Powershellu volání:


    $AzureSubscriptionID = "<Your Azure Subscription ID>"
    
    $ResourceGroupName = "<New Azure Resource Group Name>"
    $Location = "EAST US 2"
    $DefaultDataLakeStoreAccountName = "<New Data Lake Store Account Name>"
    $DataLakeAnalyticsAccountName = "<New Data Lake Analytics Account Name>"
    
    $DeploymentName = "MyDataLakeAnalyticsDeployment"
    $ARMTemplateFile = "E:\Tutorials\ADL\ARMTemplate\azuredeploy.json"  # update the Json template path 
    
    Login-AzureRmAccount
    
    Select-AzureRmSubscription -SubscriptionId $AzureSubscriptionID
    
    # Create the resource group
    New-AzureRmResourceGroup -Name $ResourceGroupName -Location $Location
    
    # Create the Data Lake Analytics account with the default Data Lake Store account.
    $parameters = @{"adlAnalyticsName"=$DataLakeAnalyticsAccountName; "adlStoreName"=$DefaultDataLakeStoreAccountName}
    New-AzureRmResourceGroupDeployment -Name $DeploymentName -ResourceGroupName $ResourceGroupName -TemplateFile $ARMTemplateFile -TemplateParameterObject $parameters 

 
###<a name="list-account"></a>Seznam účtu

Seznam dat jezera analýzy účty v rámci aktuálního předplatného

    Get-AzureRmDataLakeAnalyticsAccount
    
Výstup:

    Id         : /subscriptions/xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx/resourceGroups/learn1021rg/providers/Microsoft.DataLakeAnalytics/accounts/learn1021adla
    Location   : eastus2
    Name       : learn1021adla
    Properties : Microsoft.Azure.Management.DataLake.Analytics.Models.DataLakeAnalyticsAccountProperties
    Tags       : {}
    Type       : Microsoft.DataLakeAnalytics/accounts

Účty analýzy jezera dat seznamu do určité skupiny zdrojů

    Get-AzureRmDataLakeAnalyticsAccount -ResourceGroupName $resourceGroupName

Zobrazení podrobností konkrétní účtu jezera analýzy dat

    Get-AzureRmDataLakeAnalyticsAccount -Name $adlAnalyticsAccountName

Test existenci konkrétní účet jezera analýzy dat

    Test-AzureRmDataLakeAnalyticsAccount -Name $adlAnalyticsAccountName

Rutiny vrátí **hodnotu True** nebo **False**.

###<a name="delete-data-lake-analytics-accounts"></a>Odstranění účtů jezera analýzy dat

    $resourceGroupName = "<ResourceGroupName>"
    $dataLakeAnalyticsAccountName = "<DataLakeAnalyticsAccountName>"
    
    Remove-AzureRmDataLakeAnalyticsAccount -Name $dataLakeAnalyticsAccountName 

Odstranit Data jezera analýzy účtu se neodstraní závislá jezera úložný prostor účtu. Následující příklad odstraní účet jezera analýzy dat a výchozí účet jezera úložiště dat

    $resourceGroupName = "<ResourceGroupName>"
    $dataLakeAnalyticsAccountName = "<DataLakeAnalyticsAccountName>"
    $dataLakeStoreName = (Get-AzureRmDataLakeAnalyticsAccount -ResourceGroupName $resourceGroupName -Name $dataLakeAnalyticAccountName).Properties.DefaultDataLakeAccount

    Remove-AzureRmDataLakeAnalyticsAccount -ResourceGroupName $resourceGroupName -Name $dataLakeAnalyticAccountName 
    Remove-AzureRmDataLakeStoreAccount -ResourceGroupName $resourceGroupName -Name $dataLakeStoreName

<!-- ################################ -->
<!-- ################################ -->
## <a name="manage-account-data-sources"></a>Správa zdrojů dat účtu

V následujících zdrojích dat v současné době podporuje jezera analýzy dat:

- [Úložiště jezera dat Azure](../data-lake-store/data-lake-store-overview.md)
- [Azure úložiště](storage-introduction.md)

Při vytváření účtu analýzy, je třeba určit účet Azure úložný prostor jezera výchozí účet úložiště. Výchozí úložiště jezera dat účet se používá k ukládání protokolů auditování metadata a úlohy projektu. Po vytvoření účtu analýzy, můžete přidat další úložný prostor jezera účty a/nebo účet Azure úložiště. 

### <a name="find-the-default-data-lake-store-account"></a>Vyhledání výchozí účet jezera úložiště dat

    $resourceGroupName = "<ResourceGroupName>"
    $dataLakeAnalyticsAccountName = "<DataLakeAnalyticsAccountName>"
    $dataLakeStoreName = (Get-AzureRmDataLakeAnalyticsAccount -ResourceGroupName $resourceGroupName -Name $dataLakeAnalyticAccountName).Properties.DefaultDataLakeAccount


### <a name="add-additional-azure-blob-storage-accounts"></a>Přidat další účty úložiště objektů Blob Azure

    $resourceGroupName = "<ResourceGroupName>"
    $dataLakeAnalyticsAccountName = "<DataLakeAnalyticsAccountName>"
    $AzureStorageAccountName = "<AzureStorageAccountName>"
    $AzureStorageAccountKey = "<AzureStorageAccountKey>"
    
    Add-AzureRmDataLakeAnalyticsDataSource -ResourceGroupName $resourceGroupName -Account $dataLakeAnalyticAccountName -AzureBlob $AzureStorageAccountName -AccessKey $AzureStorageAccountKey

### <a name="add-additional-data-lake-store-accounts"></a>Přidat další účty jezera úložiště dat

    $resourceGroupName = "<ResourceGroupName>"
    $dataLakeAnalyticsAccountName = "<DataLakeAnalyticsAccountName>"
    $AzureDataLakeName = "<DataLakeStoreName>"
    
    Add-AzureRmDataLakeAnalyticsDataSource -ResourceGroupName $resourceGroupName -Account $dataLakeAnalyticAccountName -DataLake $AzureDataLakeName 

### <a name="list-data-sources"></a>Seznam zdrojů dat:

    $resourceGroupName = "<ResourceGroupName>"
    $dataLakeAnalyticsAccountName = "<DataLakeAnalyticsAccountName>"

    (Get-AzureRmDataLakeAnalyticsAccount -ResourceGroupName $resourceGroupName -Name $dataLakeAnalyticAccountName).Properties.DataLakeStoreAccounts
    (Get-AzureRmDataLakeAnalyticsAccount -ResourceGroupName $resourceGroupName -Name $dataLakeAnalyticAccountName).Properties.StorageAccounts
    


<!-- ################################ -->
<!-- ################################ -->
## <a name="manage-jobs"></a>Správa úloh

Než budete moct vytvářet úlohy, musíte mít účet jezera analýzy dat.  Další informace najdete v tématu [Správa analýzy dat jezera účty](#manage-data-lake-analytics-accounts).

### <a name="list-jobs"></a>Seznam úloh

    $dataLakeAnalyticsAccountName = "<DataLakeAnalyticsAccountName>"
    
    Get-AzureRmDataLakeAnalyticsJob -Account $dataLakeAnalyticAccountName
    
    Get-AzureRmDataLakeAnalyticsJob -Account $dataLakeAnalyticAccountName -State Running, Queued
    #States: Accepted, Compiling, Ended, New, Paused, Queued, Running, Scheduling, Starting
    
    Get-AzureRmDataLakeAnalyticsJob -Account $dataLakeAnalyticAccountName -Result Cancelled
    #Results: Cancelled, Failed, None, Successed 
    
    Get-AzureRmDataLakeAnalyticsJob -Account $dataLakeAnalyticAccountName -Name <Job Name>
    Get-AzureRmDataLakeAnalyticsJob -Account $dataLakeAnalyticAccountName -Submitter <Job submitter>

    # List all jobs submitted on January 1 (local time)
    Get-AzureRmDataLakeAnalyticsJob -Account $dataLakeAnalyticAccountName `
        -SubmittedAfter "2015/01/01"
        -SubmittedBefore "2015/01/02"   

    # List all jobs that succeeded on January 1 after 2 pm (UTC time)
    Get-AzureRmDataLakeAnalyticsJob -Account $dataLakeAnalyticAccountName `
        -State Ended
        -Result Succeeded
        -SubmittedAfter "2015/01/01 2:00 PM -0"
        -SubmittedBefore "2015/01/02 -0"

    # List all jobs submitted in the past hour
    Get-AzureRmDataLakeAnalyticsJob -Account $dataLakeAnalyticAccountName `
        -SubmittedAfter (Get-Date).AddHours(-1)

### <a name="get-job-details"></a>Zobrazení podrobností projektu

    $dataLakeAnalyticsAccountName = "<DataLakeAnalyticsAccountName>"
    Get-AzureRmDataLakeAnalyticsJob -Account $dataLakeAnalyticAccountName -JobID <Job ID>
    
### <a name="submit-jobs"></a>Odeslat úlohy

    $dataLakeAnalyticsAccountName = "<DataLakeAnalyticsAccountName>"

    #Pass script via path
    Submit-AzureRmDataLakeAnalyticsJob -Account $dataLakeAnalyticAccountName `
        -Name $jobName `
        -ScriptPath $scriptPath

    #Pass script contents
    Submit-AzureRmDataLakeAnalyticsJob -Account $dataLakeAnalyticAccountName `
        -Name $jobName `
        -Script $scriptContents

> [AZURE.NOTE] Priority (priorita) výchozí úlohy je 1000 a je výchozí stupeň paralelismus pro projekt 1.


### <a name="cancel-jobs"></a>Zrušení úloh

    Stop-AzureRmDataLakeAnalyticsJob -Account $dataLakeAnalyticAccountName `
        -JobID $jobID


## <a name="manage-catalog-items"></a>Správa položky katalogu

Katalog U SQL slouží k vytvoření struktury dat a kód tak, aby se smí zobrazovat skripty U SQL. V katalogu umožňuje možné s daty v Azure dat jezera nejvyšší výkon. Další informace najdete v tématu [použití U-SQL katalogu](data-lake-analytics-use-u-sql-catalog.md).

###<a name="list-catalog-items"></a>Seznam položky katalogu

    #List databases
    Get-AzureRmDataLakeAnalyticsCatalogItem `
        -Account $adlAnalyticsAccountName `
        -ItemType Database
    
    
    
    #List tables
    Get-AzureRmDataLakeAnalyticsCatalogItem `
        -Account $adlAnalyticsAccountName `
        -ItemType Table `
        -Path "master.dbo"

###<a name="get-catalog-item-details"></a>Zobrazení podrobností položky katalogu 

    #Get a database
    Get-AzureRmDataLakeAnalyticsCatalogItem `
        -Account $adlAnalyticsAccountName `
        -ItemType Database `
        -Path "master"
    
    #Get a table
    Get-AzureRmDataLakeAnalyticsCatalogItem `
        -Account $adlAnalyticsAccountName `
        -ItemType Table `
        -Path "master.dbo.mytable"

###<a name="test-existence-of--catalog-item"></a>Test existenci položky katalogu

    Test-AzureRmDataLakeAnalyticsCatalogItem  `
        -Account $adlAnalyticsAccountName `
        -ItemType Database `
        -Path "master"

###<a name="create-catalog-secret"></a>Vytvoření tajná katalogu
    New-AzureRmDataLakeAnalyticsCatalogSecret  `
            -Account $adlAnalyticsAccountName `
            -DatabaseName "master" `
            -Secret (Get-Credential -UserName "username" -Message "Enter the password")

### <a name="modify-catalog-secret"></a>Úprava tajná katalogu
    Set-AzureRmDataLakeAnalyticsCatalogSecret  `
            -Account $adlAnalyticsAccountName `
            -DatabaseName "master" `
            -Secret (Get-Credential -UserName "username" -Message "Enter the password")



###<a name="delete-catalog-secret"></a>Odstranění tajná katalogu
    Remove-AzureRmDataLakeAnalyticsCatalogSecret  `
            -Account $adlAnalyticsAccountName `
            -DatabaseName "master"


## <a name="use-azure-resource-manager-groups"></a>Používání skupin správce prostředků Azure

Aplikace se obvykle vytvářejí součástí mnoho, třeba do webových aplikací databáze, databázový server, úložiště a 3 stran služby. Správce Azure zdroje (ARM) umožňuje pracovat s zdrojům v aplikaci jako skupinu, označovaný taky jako skupina zdroje Azure. Můžete nasadit, aktualizace, sledovat nebo odstranit všechny zdroje pro aplikaci v operaci jediné, koordinovaný. Použití šablony pro nasazení a této šablony můžete pracovat na jiném prostředí například testování pracovní a výroby. Fakturace můžete vysvětlit pro vaši organizaci, zobrazením nákladů úrovní pro celou skupinu. Další informace najdete v tématu [Přehled Správce prostředků Azure](../azure-resource-manager/resource-group-overview.md). 

Služba jezera analýzy dat může obsahovat tyto prvky:

- Účet technologie pro analýzu dat jezera Azure
- Požadované výchozí účet pro ukládání jezera dat Azure
- Účty jezera dat Azure další úložiště
- Další úložiště Azure účty

Můžete vytvořit tyto komponenty v jedné skupině ARM pro jejich snadněji spravovat.

![Účet Azure dat jezera technologie pro analýzu a úložiště](./media/data-lake-analytics-manage-use-portal/data-lake-analytics-arm-structure.png)

Účet jezera analýzy dat a účty závislá úložiště musí být umístěny ve stejné Azure datovém centru.
Skupina ARM však mohou být umístěny v různých datovém centru.  

##<a name="see-also"></a>Viz taky 

- [Přehled analýzy dat jezera Microsoft Azure](data-lake-analytics-overview.md)
- [Začínáme s jezera analýzy dat pomocí portálu Azure](data-lake-analytics-get-started-portal.md)
- [Správa portálu Azure analýzy jezera dat Azure](data-lake-analytics-manage-use-portal.md)
- [Sledování a odstraňování případných problémů jezera analýzy dat Azure úlohy pomocí portálu Azure](data-lake-analytics-monitor-and-troubleshoot-jobs-tutorial.md)

##<a name="appendix-a---data-lake-analytics-arm-template"></a>Dodatek A - ARM technologie pro analýzu dat jezera šablony

Následující šablonu ARM mohou sloužit k nasazení jezera analýzy dat účtu a jeho závislá jezera úložiště.  Uložení jako soubor json a pomocí skriptů Powershellu můžete volat šabloně. Další informace najdete v tématu [vytváření správce prostředků Azure šablony](../resource-group-authoring-templates.md)a [nasazení aplikace šablonou správce prostředků Azure](../resource-group-template-deploy.md#deploy-with-powershell) .

    {
      "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
      "contentVersion": "1.0.0.0",
      "parameters": {
        "adlAnalyticsName": {
          "type": "string",
          "metadata": {
            "description": "The name of the Data Lake Analytics account to create."
          }
        },
        "adlStoreName": {
          "type": "string",
          "metadata": {
            "description": "The name of the Data Lake Store account to create."
          }
        }
      },
      "resources": [
        {
          "name": "[parameters('adlStoreName')]",
          "type": "Microsoft.DataLakeStore/accounts",
          "location": "East US 2",
          "apiVersion": "2015-10-01-preview",
          "dependsOn": [ ],
          "tags": { }
        },
        {
          "name": "[parameters('adlAnalyticsName')]",
          "type": "Microsoft.DataLakeAnalytics/accounts",
          "location": "East US 2",
          "apiVersion": "2015-10-01-preview",
          "dependsOn": [ "[concat('Microsoft.DataLakeStore/accounts/',parameters('adlStoreName'))]" ],
          "tags": { },
          "properties": {
            "defaultDataLakeStoreAccount": "[parameters('adlStoreName')]",
            "dataLakeStoreAccounts": [
              { "name": "[parameters('adlStoreName')]" }
            ]
          }
        }
      ],
      "outputs": {
        "adlAnalyticsAccount": {
          "type": "object",
          "value": "[reference(resourceId('Microsoft.DataLakeAnalytics/accounts',parameters('adlAnalyticsName')))]"
        },
        "adlStoreAccount": {
          "type": "object",
          "value": "[reference(resourceId('Microsoft.DataLakeStore/accounts',parameters('adlStoreName')))]"
        }
      }
    }

