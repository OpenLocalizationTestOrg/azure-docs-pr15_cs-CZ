<properties 
    pageTitle="Azure Powershellu pomocí Správce prostředků | Microsoft Azure" 
    description="Úvod do online přes Azure nasadit více zdrojů jako skupina zdroje pro Azure." 
    services="azure-resource-manager" 
    documentationCenter="" 
    authors="tfitzmac" 
    manager="timlt" 
    editor="tysonn"/>

<tags 
    ms.service="azure-resource-manager" 
    ms.workload="multiple" 
    ms.tgt_pltfrm="powershell" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/18/2016" 
    ms.author="tomfitz"/>

# <a name="using-azure-powershell-with-azure-resource-manager"></a>Správce Azure Powershellu s Azure zdroje

> [AZURE.SELECTOR]
- [Portál](azure-portal/resource-group-portal.md) 
- [Azure rozhraní příkazového řádku](xplat-cli-azure-resource-manager.md)
- [Azure Powershellu](powershell-azure-resource-manager.md)
- [ROZHRANÍ REST API](resource-manager-rest-api.md)


Azure správce prostředků používá moderní přístup k řízení životního cyklu Azure zdroje. Místo vytváření a správě jednotlivé zdroje, začněte tak, že představující si celé řešení, například blogu, Fotogalerie, SharePoint portal nebo wikiwebu. Pomocí šablony – deklarativní formátovanými jako řešení – definovat skupiny zdrojů, která obsahuje všechny zdroje musíte pro podporu řešení. Potom nasazení a správa tohoto pole Skupina zdroje jako logické jednotky. 

V tomto kurzu se naučíte, jak pomocí Powershellu Azure pomocí Správce prostředků Azure. Ho vás provede proces nasazení řešení a pracovat s ním řešení. Použijete Azure PowerShell a šablony správce prostředků pro nasazení:

- SQL server – hostovat databáze
- Databáze SQL - k ukládání dat.
- Pravidla brány firewall – umožňuje web appu pro připojení k databázi
- Plán služeb aplikací - pro definování funkcí a náklady na web appu
- Na webu – spuštění web appu
- Konfigurace webové - pro ukládání připojovací řetězec do databáze 
- Upozornění pravidla - pro sledování výkonu a chyby
- Aplikace přehledy - pro nastavení automatického měřítka

## <a name="prerequisites"></a>Zjistit předpoklady pro

K provedení tohoto kurzu, budete potřebovat:

- Účet Azure
  + Můžete [Otevřít účet Azure zdarma](/pricing/free-trial/?WT.mc_id=A261C142F): získání přeplatky můžete vyzkoušet placené služby Azure a i po byli zvyklí až můžete ponechat účet a použití uvolnit Azure služby, jako je weby. Platební kartou nikdy strhne příslušná, pokud explicitně změňte nastavení a požádat o platit.
  
  + Je možné [Aktivovat výhod odběratele MSDN](/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F): web MSDN pro vaše předplatné vám přeplatky každý měsíc, využívající služby placené Azure.
- Azure PowerShell 1.0. Informace o této verzi a jak ji nainstalovat najdete v tématu [instalace a konfigurace prostředí PowerShell Azure](powershell-install-configure.md).

Tento kurz je určený pro začátečníky Powershellu, ale předpokládá chápete základní koncepty, například modulů kontroly, rutin a relace.

## <a name="get-help-for-cmdlets"></a>Získání nápovědy pro rutiny

Zobrazíte podrobnou nápovědu pro všechny rutina, která se zobrazí v tomto kurzu získáte pomocí rutiny Get-Help. 

    Get-Help <cmdlet-name> -Detailed

Nápovědu k rutinu Get-AzureRmResource, zadejte například:

    Get-Help Get-AzureRmResource -Detailed

Zobrazte seznam rutin v modulu zdroje s souhrn nápovědy, zadejte: 

    Get-Command -Module AzureRM.Resources | Get-Help | Format-Table Name, Synopsis

Výstup bude vypadat podobně jako následující úryvek:

    Name                                   Synopsis
    ----                                   --------
    Find-AzureRmResource                   Searches for resources using the specified parameters.
    Find-AzureRmResourceGroup              Searches for resource group using the specified parameters.
    Get-AzureRmADGroup                     Filters active directory groups.
    Get-AzureRmADGroupMember               Get a group members.
    ...

Úplné nápovědu pro rutinu, zadejte příkaz s formátem:

    Get-Help <cmdlet-name> -Full
  
## <a name="login-to-your-azure-account"></a>Přihlášení k účtu Azure

Než začnete pracovat na řešení, musíte se přihlásit ke svému účtu.

Přihlášení k účtu Azure, získáte pomocí rutiny **AzureRmAccount přidat** .

    Add-AzureRmAccount

Rutiny výzvu k zadání přihlašovací údaje účtu Azure. Po přihlášení se stáhne nastavení účtu tak, aby byly k dispozici pro Azure PowerShell. 

Nastavení účtu platnost, budete muset aktualizovat občas. Abyste mohli aktualizovat nastavení účtu, spusťte **Přidat AzureRmAccount** znovu. 

>[AZURE.NOTE] Správce prostředků moduly vyžaduje AzureRmAccount přidat. Soubor nastavení publikování nestačí.     

Pokud máte víc předplatných, zadejte id předplatného, které chcete použít pro nasazení s rutinu **Set-AzureRmContext** .

    Set-AzureRmContext -SubscriptionID <YourSubscriptionId>

## <a name="create-a-resource-group"></a>Vytvoření skupiny prostředků

Než nasadíte zdroje k vašemu předplatnému, musíte vytvořit skupina zdroje, který bude obsahovat zdroje. 

Pokud chcete vytvořit skupinu zdrojů, použijte rutinu **New-AzureRmResourceGroup** .

Příkaz parametrem **název** zadejte název skupiny zdrojů a parametr **umístění** zadejte umístění. Založené na co jsme zjistil v předchozí části, použijeme "Západ USA" umístění.

    New-AzureRmResourceGroup -Name TestRG1 -Location "West US"
    
Výstup bude vypadat podobně jako:

    ResourceGroupName : TestRG1
    Location          : westus
    ProvisioningState : Succeeded
    Tags              :
    ResourceId        : /subscriptions/{guid}/resourceGroups/TestRG1

Skupina zdroje byly úspěšně vytvořila.

## <a name="deploy-your-solution"></a>Nasazení řešení

Toto téma není ukazují, jak vytvořit šablonu nebo diskutovat o struktuře šablony. Tyto informace najdete v článku [Správce Azure prostředků pro vytváření šablon](resource-group-authoring-templates.md) a [Návodu šablonu správce prostředků](resource-manager-template-walkthrough.md). Budete nasazovat předdefinované šablony [poskytování Web App s databázi SQL](https://azure.microsoft.com/documentation/templates/201-web-app-sql-database/) [Azure rychlý úvod](https://azure.microsoft.com/documentation/templates/)šablony.

Je potřeba skupina zdroje a máte šablony, takže jste připraveni teď nasazení infrastruktury definovaný v šabloně do skupiny zdrojů. Můžete nasadit zdroje s rutinu **New-AzureRmResourceGroupDeployment** . Šablona určuje mnoho výchozí hodnoty, které použijeme takže není potřeba zadat hodnoty pro tyto parametry. Základní syntaxi vypadá takto:

    New-AzureRmResourceGroupDeployment -ResourceGroupName TestRG1 -administratorLogin exampleadmin -TemplateUri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-web-app-sql-database/azuredeploy.json 

Zadáte pole Skupina zdroje a umístění šablony. Pokud je vaše šablona místní soubor, použít parametr **– TemplateFile** a zadejte cestu k šabloně. Můžete nastavit **-režimu** parametr **Přírůstková** nebo **Dokončeno**. Ve výchozím nastavení provede správce prostředků přírůstková aktualizace během nasazení; proto není nutné nastavit **-režimu** kdy se má **Přírůstková**. Rozdíly mezi těchto režimů nasazení, najdete v tématu [nasazení aplikace pomocí šablony správce prostředků Azure](resource-group-template-deploy.md). 

###<a name="dynamic-template-parameters"></a>Dynamické šablony parametry

Pokud máte zkušenosti s prostředím PowerShell, víte, že můžete přepínat mezi dostupné parametry rutina stisknutím kláves znaménko minus (-) a potom stisknutím klávesy TAB. Stejnou funkci taky označené jako pracuje s parametry, které definujete v šabloně. Hned zadejte název šablony rutinu načte šabloně analyzuje ho a přidá parametry šablony s příkazem dynamicky. To usnadňuje velmi zadejte hodnoty parametrů šablony.

Po zadání příkazu se zobrazí výzva pro chybějící povinné parametr **administratorLoginPassword**. A pokud zadáte heslo, zabezpečené řetězcovou hodnotu skryté. Tato strategie eliminuje riziko zadání hesla ve formátu prostého textu.

    cmdlet New-AzureRmResourceGroupDeployment at command pipeline position 1
    Supply values for the following parameters:
    (Type !? for Help.)
    administratorLoginPassword: ********

Pokud šablona obsahuje parametr s názvem, který odpovídá některému z parametrů v poli příkaz nasazení šablony (například včetně parametr s názvem **ResourceGroupName** v šabloně, která je stejná jako parametr **ResourceGroupName** v rutinu [New-AzureRmResourceGroupDeployment](https://msdn.microsoft.com/library/azure/mt679003.aspx) ), budete vyzváni k zadat hodnotu parametru s přípona **FromTemplate** (například **ResourceGroupNameFromTemplate**). Obecně neměli byste tento zapomíná a to tak, že není pojmenování parametrů se stejným názvem jako parametry používané pro nasazení operace.

Příkaz spustí a vrátí zprávy při vytvoření zdroje. Nakonec uvidíte výsledek nasazení.

    DeploymentName    : azuredeploy
    ResourceGroupName : TestRG1
    ProvisioningState : Succeeded
    Timestamp         : 4/11/2016 7:26:11 PM
    Mode              : Incremental
    TemplateLink      :
                Uri            : https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-web-app-sql-database/azuredeploy.json
                ContentVersion : 1.0.0.0
    Parameters        :
                Name             Type                       Value
                ===============  =========================  ==========
                skuName          String                     F1
                skuCapacity      Int                        1
                administratorLogin  String                  exampleadmin
                administratorLoginPassword  SecureString
                databaseName     String                     sampledb
                collation        String                     SQL_Latin1_General_CP1_CI_AS
                edition          String                     Basic
                maxSizeBytes     String                     1073741824
                requestedServiceObjectiveName  String       Basic

    Outputs           :
                Name             Type                       Value
                ===============  =========================  ==========
                siteUri          String                     websites5wdai7p2k2g4.azurewebsites.net
                sqlSvrFqdn       String                     sqlservers5wdai7p2k2g4.database.windows.net
                    
    DeploymentDebugLogLevel :

V několika krocích jsme vytvořili a nasazené prostředky potřebné pro složité Web. 

### <a name="log-debug-information"></a>Informace o ladění protokolu

Při nasazení šablony, se odhlásíte Další informace o žádostí a odpovědí zadáním parametru **- DeploymentDebugLogLevel** při spuštění **Nové AzureRmResourceGroupDeployment**. Tyto informace můžou pomoct Poradce při potížích nasazení. Výchozí hodnota je **žádná** což znamená, bez žádosti o nebo zaznamenané obsah odpověď. Můžete zadat protokolování obsah z žádost a odpověď.  Další informace o řešení potíží s nasazení a protokolování ladění informace najdete v článku [nasazení skupina zdroje Poradce při potížích s Azure Powershellu](resource-manager-troubleshoot-deployments-powershell.md). Následující příklad protokoly žádost o obsahu a odpověď obsahu pro nasazení.

    New-AzureRmResourceGroupDeployment -ResourceGroupName TestRG1 -DeploymentDebugLogLevel All -TemplateUri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-web-app-sql-database/azuredeploy.json 

> [AZURE.NOTE] Při nastavování parametr DeploymentDebugLogLevel, zvažte pečlivě typ informací, které předáváte během nasazení. Protokolování informace o žádosti nebo odpovědi může potenciálně vystavit citlivá data, která se načte prostřednictvím operace nasazení. 


## <a name="get-information-about-your-resource-groups"></a>Získat informace o skupinách zdroje

Po vytvoření skupiny zdrojů, můžete pomocí rutin v modulu Správce prostředků ke správě skupin zdroje.

- Skupina zdroje ve vašem předplatném, použijte rutinu **Get-AzureRmResourceGroup** :

        Get-AzureRmResourceGroup -ResourceGroupName TestRG1
    
    Které vrátí následující informace:

        ResourceGroupName : TestRG1
        Location          : westus
        ProvisioningState : Succeeded
        Tags              :
        ResourceId        : /subscriptions/{guid}/resourceGroups/TestRG
        
        ...

    Pokud nezadáte název skupiny a zdroje, rutinu všechny skupiny zdroje vrátí předplatné.

- Zdroje ve skupině prostředků, použijte rutinu **Najít AzureRmResource** a jeho **ResourceGroupNameContains** parametr. Bez parametrů najít AzureRmResource vstoupit předplatného Azure všechny zdroje.

        Find-AzureRmResource -ResourceGroupNameContains TestRG1
    
     Které vrátí seznam zdrojů uvedena ve správném formátu jako:
        
        Name              : sqlservers5wdai7p2k2g4
        ResourceId        : /subscriptions/{guid}/resourceGroups/TestRG1/providers/Microsoft.Sql/servers/sqlservers5wdai7p2k2g4
        ResourceName      : sqlservers5wdai7p2k2g4
        ResourceType      : Microsoft.Sql/servers
        Kind              : v2.0
        ResourceGroupName : TestRG1
        Location          : westus
        SubscriptionId    : {guid}
        Tags              : {System.Collections.Hashtable}
        ...
            
- Značky umožňuje logicky uspořádání zdrojů ve vašem předplatném a načíst zdroje s rutiny pro **Vyhledání AzureRmResource** a **Najít AzureRmResourceGroup** .

        Find-AzureRmResource -TagName displayName -TagValue Website

        Name              : webSites5wdai7p2k2g4
        ResourceId        : /subscriptions/{guid}/resourceGroups/TestRG1/providers/Microsoft.Web/sites/webSites5wdai7p2k2g4
        ResourceName      : webSites5wdai7p2k2g4
        ResourceType      : Microsoft.Web/sites
        ResourceGroupName : TestRG1
        Location          : westus
        SubscriptionId    : {guid}
                
      There is much more you can do with tags. For more information, see [Using tags to organize your Azure resources](resource-group-using-tags.md).

## <a name="add-to-a-resource-group"></a>Přidat do skupiny zdrojů

Přidání zdroje do skupiny zdrojů, můžete použít rutinu **New-AzureRmResource** . Přidání zdroje tímto způsobem však může být budoucí matoucí protože nový zdroj neexistuje v šabloně. Pokud znovu nasazený původní šabloně nasadíte by úplné řešení. Pokud často nasazení najdete je jednodušší a spolehlivější k přidání nového prostředku do šablony a znovu ho nasadit.

## <a name="move-a-resource"></a>Přesunutí zdroje

Existující zdroje můžete přesunout do nové skupiny prostředků. Příklady najdete v článku [Přesunutí zdrojů do nové skupiny prostředků nebo předplatného](resource-group-move-resources.md).

## <a name="export-template"></a>Export šablony

Pro existující skupiny zdrojů (nasazených pomocí prostředí PowerShell nebo z jiné metody, jako je portál) můžete zobrazit šablonu správce prostředků pro skupiny zdrojů. Export šablony nabízí dva výhody:

1. Můžete jednoduše automatizovat budoucí nasazení řešení, protože všechny infrastruktury je definován v šabloně.

2. Můžete seznámit se s syntaxe šablony hledáním v JavaScript Object Notation (JSON), který představuje řešení.

Pomocí prostředí PowerShell můžete generovat šablonu, která představuje aktuální stav skupiny prostředků nebo načtení šablonu, která byla použita pro konkrétní nasazení.

Export šablonu vhodnou pro skupina zdroje je užitečný změny provedené skupina zdroje a načtení JSON znázornění jeho aktuálním stavu. Však vygenerovaných šablona obsahuje pouze minimální počet parametrů a žádné proměnné. Většina hodnot v šabloně je pevně. Než nasadíte vygenerovaných šablonu, můžete chtít převést více hodnot parametry tak, aby si můžete přizpůsobit nasazení pro různá prostředí.

Export šablony pro konkrétní nasazení je užitečné, když potřebujete zobrazit skutečné šablonu, která byla použita pro nasazení zdroje. Šablona bude obsahovat všechny parametry a proměnné definované původní nasazení. Ale pokud někdo z vaší organizace provedla změny skupina zdroje mimo co je definována v šabloně, tato šablona nebude představují aktuální stav skupina zdroje.

> [AZURE.NOTE] Funkce šablony exportu je ve verzi preview a ne všechny typy zdrojů momentálně podporují export šablony. Při pokusu o exportu šablon, je chyba se může zobrazit, která tvrdí, že nebyly exportovány některé zdroje informací. V případě potřeby můžete ručně definujete tyto materiály v šabloně po si ji stáhnete.

###<a name="export-template-from-resource-group"></a>Export šablony z pole Skupina zdroje

Šablony pro skupinu prostředků zobrazíte spusťte rutinu **AzureRmResourceGroup exportovat** .

    Export-AzureRmResourceGroup -ResourceGroupName TestRG1 -Path c:\Azure\Templates\Downloads\TestRG1.json
    
###<a name="download-template-from-deployment"></a>Stažení šablony z nasazení

Ke stažení šablony pro konkrétní nasazení, spusťte rutinu **AzureRmResourceGroupDeploymentTemplate uložit** .

    Save-AzureRmResourceGroupDeploymentTemplate -DeploymentName azuredeploy -ResourceGroupName TestRG1 -Path c:\Azure\Templates\Downloads\azuredeploy.json

## <a name="delete-resources-or-resource-group"></a>Odstranění zdroje nebo skupina zdroje

- Odstranění zdroje ze skupiny zdrojů, získáte pomocí rutiny **AzureRmResource odebrat** . Tato rutina odstraní zdroje, ale neodstraníte skupina zdroje.

    Příkaz odebere TestSite web ze skupiny zdrojů TestRG1.

        Remove-AzureRmResource -Name TestSite -ResourceGroupName TestRG1 -ResourceType "Microsoft.Web/sites" -ApiVersion 2015-08-01

- Pokud chcete odstranit skupinu zdrojů, získáte pomocí rutiny **AzureRmResourceGroup odebrat** . Tato rutina odstraní skupina zdroje a jeho zdroje.

        Remove-AzureRmResourceGroup -Name TestRG1
        
    Zobrazí se výzva k potvrzení odstranění.
        
        Confirm
        Are you sure you want to remove resource group 'TestRG1'
        [Y] Yes  [N] No  [S] Suspend  [?] Help (default is "Y"): Y

## <a name="deployment-script"></a>Skript pro nasazení

Předchozích příkladech nasazení v tomto tématu ukázal jenom jednotlivé rutiny potřebné k nasazení zdroje Azure. Následující příklad ukazuje nasazení skript, který vytvoří skupina zdroje a nasadí zdroje.

    <#
      .SYNOPSIS
      Deploys a template to Azure
      
      .DESCRIPTION
      Deploys an Azure Resource Manager template

      .PARAMETER subscriptionId
      The subscription id where the template will be deployed.

      .PARAMETER resourceGroupName
      The resource group where the template will be deployed. Can be the name of an existing or a new resource group.

      .PARAMETER resourceGroupLocation
      Optional, a resource group location. If specified, will try to create a new resource group in this location. If not specified, assumes resource group is existing.

      .PARAMETER deploymentName
      The deployment name.

      .PARAMETER templateFilePath
      Optional, path to the template file. Defaults to template.json.

      .PARAMETER parametersFilePath
      Optional, path to the parameters file. Defaults to parameters.json. If file is not found, will prompt for parameter values based on template.
    #>

    param(
      [Parameter(Mandatory=$True)]
      [string]
      $subscriptionId,

      [Parameter(Mandatory=$True)]
      [string]
      $resourceGroupName,

      [string]
      $resourceGroupLocation,

      [Parameter(Mandatory=$True)]
      [string]
      $deploymentName,

      [string]
      $templateFilePath = "template.json",

      [string]
      $parametersFilePath = "parameters.json"
    )

    #******************************************************************************
    # Script body
    # Execution begins here 
    #******************************************************************************
    $ErrorActionPreference = "Stop"

    # sign in
    Write-Host "Logging in...";
    Add-AzureRmAccount;

    # select subscription
    Write-Host "Selecting subscription '$subscriptionId'";
    Set-AzureRmContext -SubscriptionID $subscriptionId;

    #Create or check for existing resource group
    $resourceGroup = Get-AzureRmResourceGroup -Name $resourceGroupName -ErrorAction SilentlyContinue
    if(!$resourceGroup)
    {
      Write-Host "Resource group '$resourceGroupName' does not exist. To create a new resource group, please enter a location.";
      if(!$resourceGroupLocation) {
        $resourceGroupLocation = Read-Host "resourceGroupLocation";
      }
      Write-Host "Creating resource group '$resourceGroupName' in location '$resourceGroupLocation'";
      New-AzureRmResourceGroup -Name $resourceGroupName -Location $resourceGroupLocation
    }
    else{
      Write-Host "Using existing resource group '$resourceGroupName'";
    }

    # Start the deployment
    Write-Host "Starting deployment...";
    if(Test-Path $parametersFilePath) {
      New-AzureRmResourceGroupDeployment -ResourceGroupName $resourceGroupName -TemplateFile $templateFilePath -TemplateParameterFile $parametersFilePath;
    } else {
      New-AzureRmResourceGroupDeployment -ResourceGroupName $resourceGroupName -TemplateFile $templateFilePath;
    }

## <a name="next-steps"></a>Další kroky

- Další informace o vytváření správce prostředků šablon najdete v tématu [Vytváření šablony Azure správce prostředků](./resource-group-authoring-templates.md).
- Další informace o nasazení šablon najdete v tématu [nasadit aplikaci Azure správce prostředků šablony](./resource-group-template-deploy.md).
- Podrobných příkladů nasazení projektu najdete v článku [nasazení microservices předvídatelností v Azure](app-service-web/app-service-deploy-complex-application-predictably.md).
- Další informace o odstraňování potíží, které se nepodařilo nasazení najdete v tématu [Poradce při potížích zdrojů skupina nasazení v Azure](./resource-manager-troubleshoot-deployments-powershell.md).

