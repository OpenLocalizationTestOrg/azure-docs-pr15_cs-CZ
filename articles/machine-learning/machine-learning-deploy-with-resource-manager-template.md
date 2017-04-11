<properties
    pageTitle="Nasazení počítače výukové pracovního prostoru pomocí šablon Azure správce prostředků | Microsoft Azure"
    description="Nasazení pracovní prostor pro vzdělávání počítače Azure pomocí šablony správce prostředků Azure"
    services="machine-learning"
    documentationCenter=""
    authors="ahgyger"
    manager="haining"
    editor="garye"/>

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/23/2016"
    ms.author="ahgyger"/>
# <a name="deploy-machine-learning-workspace-using-azure-resource-manager"></a>Nasazení počítače výukové pracovního prostoru pomocí Azure správce prostředků

## <a name="introduction"></a>Úvod
Pomocí Správce prostředků Azure nasazení šablony šetří čas tím, že scalable způsob, jak nasadit propojené součásti s ověření a opakujte mechanismus. Pokud chcete nastavit pracovní prostory výukové Azure počítač, například potřebujete nejdřív nakonfigurovat účet Azure úložiště a nainstalujte ji pracovního prostoru. Představte si tím ručně stovky pracovní prostory. Snadnější alternativy je použití šablony pro správce prostředků Azure nasadit pracovního prostoru Azure počítače učení a jeho závislosti. Tento článek vás provede tento proces podrobné. Skvělý Přehled Správce prostředků Azure najdete v tématu [Přehled Správce prostředků Azure](../azure-resource-manager/resource-group-overview.md).

## <a name="step-by-step-create-a-machine-learning-workspace"></a>Podrobné pokyny: vytvořit pracovní prostor výukové počítače
Jsme vytvoří skupinu Azure zdroje a potom nasazení nový účet Azure úložiště a nového Azure počítače výukové pracovního prostoru pomocí šablony správce prostředků. Po dokončení nasazení jsme vytiskne důležité informace o pracovní prostory, které byly vytvořené (primární klíč workspaceID a adresu URL do pracovního prostoru).

### <a name="create-an-azure-resource-manager-template"></a>Vytvoření šablony pro správce prostředků Azure
Pracovní prostor výukové počítač vyžaduje účet Azure úložiště pro ukládání datovou sadu s klientem propojený.
Následující šablona používá název skupiny prostředků pro generování název účtu úložiště a název pracovního prostoru.  Taky používá název účtu úložiště jako vlastnost při vytváření pracovního prostoru.

```
{
    "contentVersion": "1.0.0.0",
    "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "variables": {
        "namePrefix": "[resourceGroup().name]",
        "location": "[resourceGroup().location]",
        "mlVersion": "2016-04-01",
        "stgVersion": "2015-06-15",
        "storageAccountName": "[concat(variables('namePrefix'),'stg')]",
        "mlWorkspaceName": "[concat(variables('namePrefix'),'mlwk')]",
        "mlResourceId": "[resourceId('Microsoft.MachineLearning/workspaces', variables('mlWorkspaceName'))]",
        "stgResourceId": "[resourceId('Microsoft.Storage/storageAccounts', variables('storageAccountName'))]",
        "storageAccountType": "Standard_LRS"
    },
    "resources": [
        {
            "apiVersion": "[variables('stgVersion')]",
            "name": "[variables('storageAccountName')]",
            "type": "Microsoft.Storage/storageAccounts",
            "location": "[variables('location')]",
            "properties": {
                "accountType": "[variables('storageAccountType')]"
            }
        },
        {
            "apiVersion": "[variables('mlVersion')]",
            "type": "Microsoft.MachineLearning/workspaces",
            "name": "[variables('mlWorkspaceName')]",
            "location": "[variables('location')]",
            "dependsOn": ["[variables('stgResourceId')]"],
            "properties": {
                "UserStorageAccountId": "[variables('stgResourceId')]"
            }
        }
    ],
    "outputs": {
        "mlWorkspaceObject": {"type": "object", "value": "[reference(variables('mlResourceId'), variables('mlVersion'))]"},
        "mlWorkspaceToken": {"type": "string", "value": "[listWorkspaceKeys(variables('mlResourceId'), variables('mlVersion')).primaryToken]"},
        "mlWorkspaceWorkspaceID": {"type": "string", "value": "[reference(variables('mlResourceId'), variables('mlVersion')).WorkspaceId]"},
        "mlWorkspaceWorkspaceLink": {"type": "string", "value": "[concat('https://studio.azureml.net/Home/ViewWorkspace/', reference(variables('mlResourceId'), variables('mlVersion')).WorkspaceId)]"}
    }
}

```
Tato šablona uložte jako soubor mlworkspace.json pod c:\temp\.

### <a name="deploy-the-resource-group-based-on-the-template"></a>Nasazení skupina zdroje založené na šabloně
* Otevření prostředí PowerShell
* Nainstalovat moduly pro správce prostředků Azure a Správa služby Azure  

```
# Install the Azure Resource Manager modules from the PowerShell Gallery (press “A”)
Install-Module AzureRM -Scope CurrentUser

# Install the Azure Service Management modules from the PowerShell Gallery (press “A”)
Install-Module Azure -Scope CurrentUser
```

   Tento postup stáhnout a nainstalovat moduly potřebné k dokončení zbývající kroky. To stačí udělat jednou v prostředí, kde jsou spuštění příkazu Powershellu.   

* Ověření Azure  

```
# Authenticate (enter your credentials in the pop-up window)
Add-AzureRmAccount
```
Tento krok je potřeba se opakuje pro každou relaci. Po ověření, budou zobrazeny informace o předplatném.

![Účet Azure][1]

Teď můžeme mít přístup k Azure, můžeme vytvořit skupiny zdrojů.

* Vytvoření skupiny prostředků

```
$rg = New-AzureRmResourceGroup -Name "uniquenamerequired523" -Location "South Central US"
$rg
```

Ověřte, že skupina zdroje je správně zřízení. **ProvisioningState** by měl být "byla úspěšně dokončena."
Název skupiny zdroje se používá šablonou generovat název účtu úložiště. Název účtu úložiště musí být mezi 3 a 24 znaků a použijte čísla a malými písmeny.

![Pole Skupina zdroje][2]

* Použití nasazení skupina zdroje, nasazení nového pracovního prostoru výukové počítače.

```
# Create a Resource Group, TemplateFile is the location of the JSON template.
$rgd = New-AzureRmResourceGroupDeployment -Name "demo" -TemplateFile "C:\temp\mlworkspace.json" -ResourceGroupName $rg.ResourceGroupName
```

Po dokončení nasazení je jednoduchá přístup vlastností pracovního prostoru, který jste nainstalovali. Například dostanete se k tokenu primární klíč.

```
# Access Azure ML Workspace Token after its deployment.
$rgd.Outputs.mlWorkspaceToken.Value
```

Další způsob, jak získat tokeny existující pracovního prostoru je použití příkazu vyvolat AzureRmResourceAction. Můžete například seznam primárních a sekundárních tokeny všechny pracovní prostory.

```  
# List the primary and secondary tokens of all workspaces
Get-AzureRmResource |? { $_.ResourceType -Like "*MachineLearning/workspaces*"} |% { Invoke-AzureRmResourceAction -ResourceId $_.ResourceId -Action listworkspacekeys -Force}  
```
Po zřízení pracovní prostor můžete také automatizovat mnoho Azure počítače výukové Studio úkolů pomocí [Modul Powershellu pro výukové počítače Azure](http://aka.ms/amlps).

## <a name="next-steps"></a>Další kroky 
* Další informace o [Vytváření šablon Azure správce prostředků](../resource-group-authoring-templates.md). 
* Si v [Azure rychlý úvod šablony úložiště](https://github.com/Azure/azure-quickstart-templates). 
* Podívejte se na toto video o [Správce prostředků Azure](https://channel9.msdn.com/Events/Ignite/2015/C9-39). 
 
<!--Image references-->
[1]: ../media/machine-learning-deploy-with-resource-manager-template/azuresubscription.png
[2]: ../media/machine-learning-deploy-with-resource-manager-template/resourcegroupprovisioning.png


<!--Link references-->
