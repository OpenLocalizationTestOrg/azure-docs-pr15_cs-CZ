<properties
   pageTitle="Nasazení zdrojů pomocí prostředí PowerShell a šablon | Microsoft Azure"
   description="Umožňuje Správce prostředků Azure a Azure PowerShell nasazení zdroje Azure. Zdroje jsou definované v šabloně správce prostředků."
   services="azure-resource-manager"
   documentationCenter="na"
   authors="tfitzmac"
   manager="timlt"
   editor="tysonn"/>

<tags
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/15/2016"
   ms.author="tomfitz"/>

# <a name="deploy-resources-with-resource-manager-templates-and-azure-powershell"></a>Nasazení zdroje se šablonami správce prostředků a Azure PowerShell

> [AZURE.SELECTOR]
- [Prostředí PowerShell](resource-group-template-deploy.md)
- [Azure rozhraní příkazového řádku](resource-group-template-deploy-cli.md)
- [Portál](resource-group-template-deploy-portal.md)
- [ROZHRANÍ REST API](resource-group-template-deploy-rest.md)

Toto téma vysvětluje, jak pomocí Powershellu Azure šablonami správce prostředků nasazení svých prostředcích Azure.  

> [AZURE.TIP] Pokud potřebujete pomoc s ladění chybu při nasazení najdete v článku:
>
> - [Zobrazit nasazení operace s Azure PowerShell](resource-manager-troubleshoot-deployments-powershell.md) Další informace o získání informací o, který vám pomůže Poradce při potížích s vaší chyby
> - [Poradce při běžných chyb při nasazení zdrojů Azure pomocí Správce prostředků Azure](resource-manager-common-deployment-errors.md) se dozvíte, jak vyřešit běžné chyby při zavedení

Šablony můžou být místní soubor nebo externí soubor, který je k dispozici prostřednictvím identifikátor URI. Pokud vaše šablona nachází v účtu úložiště, můžete omezit přístup k šabloně a zadat sdílené přístupový token podpisu (přidružení zabezpečení) během nasazení.

## <a name="quick-steps-to-deployment"></a>Rychlé kroky k nasazení

Tento článek popisuje všechny během nasazení k dispozici různé možnosti. Ale často stačí dva jednoduché příkazy. Chcete-li rychle začít pracovat s nasazení, použijte následující příkazy:

    New-AzureRmResourceGroup -Name ExampleResourceGroup -Location "West US"
    New-AzureRmResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName ExampleResourceGroup -TemplateFile <PathToTemplate> -TemplateParameterFile <PathToParameterFile>

Další informace o možnostech nasazení, který může být vhodnější pro nefunguje, pokračujte ve čtení tohoto článku.

[AZURE.INCLUDE [resource-manager-deployments](../includes/resource-manager-deployments.md)]

## <a name="deploy-with-powershell"></a>Nasazení pomocí prostředí PowerShell

1. Přihlaste se k účtu Azure.

        Add-AzureRmAccount

     Vrátí souhrn vašeho účtu.

        Environment : AzureCloud
        Account     : someone@example.com
        ...

2. Pokud máte víc předplatných, zadejte ID předplatného chcete použít pro nasazení pomocí příkazu **AzureRmContext sadu** . 

        Set-AzureRmContext -SubscriptionID <YourSubscriptionId>

3. Obvykle když nasadíte nové šablony, které chcete vytvořit skupina zdroje má být zdroje. Pokud máte existující skupiny zdrojů, kterou chcete nasadit, můžete tento krok přeskočit a pomocí tohoto pole Skupina zdroje. 

     Pokud chcete vytvořit skupinu zdrojů, zadejte název a umístění skupina zdroje. Potřebujete poskytuje místo, kde skupina zdroje, protože skupina zdroje ukládá metadata o zdroji. Dodržování předpisů důvodů můžete určit, kde je uložena že metadata. Obecně doporučujeme, abyste zadejte umístění, kde se bude nacházet většina prostředků. Pomocí na stejném místě, můžete zjednodušit vaši šablonu.

        New-AzureRmResourceGroup -Name ExampleResourceGroup -Location "West US"
   
     Vrátí souhrn nové skupiny prostředků.
   
        ResourceGroupName : ExampleResourceGroup
        Location          : westus
        ProvisioningState : Succeeded
        Tags              :
        Permissions       :
             Actions  NotActions
             =======  ==========
             *
        ResourceId        : /subscriptions/######/resourceGroups/ExampleResourceGroup

4. Před spuštěním nasazení, můžete ověřit nastavení nasazení. Rutina **Test AzureRmResourceGroupDeployment** umožňuje zjišťování problémů podle před vytvořením skutečnými zdroji. Následující příklad ukazuje, jak ověřit na nasazení.

        Test-AzureRmResourceGroupDeployment -ResourceGroupName ExampleResourceGroup -TemplateFile <PathToTemplate>

5. Abyste mohli nasadit zdrojů do skupiny zdrojů, spuštění příkazu **Nový AzureRmResourceGroupDeployment** a zadejte potřebné parametry. Parametry obsahovat název nasazení, název skupiny zdrojů, cestu nebo adresu URL šablony, kterou jste vytvořili a všechny ostatní parametry funkce potřebné pro nefunguje. Pokud není zadán parametr **režimu** , bude použita výchozí hodnota **Přírůstková** . Dokončení nasazení spustíte nastavení **režimu** **Dokončeno**. Buďte opatrní při použití úplné režimu jako můžete neúmyslně odstraníte prostředky, které nejsou v šabloně.

     Abyste mohli nasadit místní šablonu, použijte parametr **TemplateFile** :

        New-AzureRmResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName ExampleResourceGroup -TemplateFile <PathToTemplate>

     Abyste mohli nasadit externí šablony, použijte **TemplateUri** parametr:

        New-AzureRmResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName ExampleResourceGroup -TemplateUri <LinkToTemplate>
   
     Máte k dispozici následující možnosti umožňující hodnoty parametrů: 
   
     1. Použití vloženého parametrů.

            New-AzureRmResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName ExampleResourceGroup -TemplateFile <PathToTemplate> -myParameterName "parameterValue"

     2. Použijte parametr objektu.

            $parameters = @{"<ParameterName>"="<Parameter Value>"}
            New-AzureRmResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName ExampleResourceGroup -TemplateFile <PathToTemplate> -TemplateParameterObject $parameters

     3. Použití místní parametr souboru. Informace o souboru šablony najdete v tématu [soubor parametrů](#parameter-file).

            New-AzureRmResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName ExampleResourceGroup -TemplateFile <PathToTemplate> -TemplateParameterFile <PathToParameterFile>

     4. Použití externích parametr souboru. Informace o souboru šablony najdete v tématu [soubor parametrů](#parameter-file). 

            New-AzureRmResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName ExampleResourceGroup -TemplateUri <LinkToTemplate> -TemplateParameterUri <LinkToParameterFile>

        Při použití externí parametr souboru nemůžete předat hodnotám buď vložené nebo místní soubor. Další informace najdete v tématu [parametr Priorita](#parameter-precendence).

     Po nasazení zdroje se najdete v článku Přehled nasazení.

        DeploymentName    : ExampleDeployment
        ResourceGroupName : ExampleResourceGroup
        ProvisioningState : Succeeded
        Timestamp         : 4/14/2015 7:00:27 PM
        Mode              : Incremental
        ...

     Pokud šablona obsahuje parametr se stejným názvem jako parametrů v příkazu Powershellu, se zobrazí výzva k zadání hodnoty pro tento parametr. Parametr z vlastní šablony bude obsahovat přípona **FromTemplate**. Například parametr s názvem **ResourceGroupName** na šablony v konfliktu s parametrem **ResourceGroupName** v rutinu [New-AzureRmResourceGroupDeployment](https://msdn.microsoft.com/library/azure/mt679003.aspx) . Zobrazí se výzva k zadání hodnoty pro **ResourceGroupNameFromTemplate**. Obecně neměli byste tento zapomíná a to tak, že není pojmenování parametrů se stejným názvem jako parametry používané pro nasazení operace.

6. Pokud chcete zaznamenat další informace o nasazení, které vám mohou pomoci Poradce při potížích s chyby nasazení, použít parametr **DeploymentDebugLogLevel** . Můžete určit, že žádosti o obsah nebo obsah odpověď Zaprotokolují s operaci nasazení.

        New-AzureRmResourceGroupDeployment -Name ExampleDeployment -DeploymentDebugLogLevel All -ResourceGroupName ExampleResourceGroup -TemplateFile <PathOrLinkToTemplate>
        
     Další informace o použití této ladění obsah řešit problémy s nasazení najdete v článku [nasazení skupina zdroje Poradce při potížích s Azure Powershellu](resource-manager-troubleshoot-deployments-powershell.md).

## <a name="deploy-template-from-storage-with-sas-token"></a>Nasazení šablony z úložiště s token přidružení zabezpečení

Šablony můžete přidat k účtu úložiště a propojení jejích během nasazení s tokem přidružení zabezpečení.

> [AZURE.IMPORTANT] Pomocí následujícího postupu, bude přístupný pro jenom vlastník účtu objektů blob obsahující šablonu. Však při vytváření token přidružení zabezpečení objektů blob objektů blob přístupný pro každý, kdo má tuto URI. Pokud jiný uživatel zachytí identifikátor URI, tento uživatel má přístup k šabloně. Použití token přidružení zabezpečení je vhodná omezení přístupu k šablonám, ale nezahrnujte citlivá data například hesla přímo do šablony.

### <a name="add-private-template-to-storage-account"></a>Přidání soukromé šablony k účtu úložiště

Nastavení účtu úložiště šablon týkajících se podle těchto kroků:

1. Vytvoření skupiny prostředků.

        New-AzureRmResourceGroup -Name ManageGroup -Location "West US"

2. Vytvoření účtu úložiště. Název účtu úložiště musí být jedinečný přes Azure, tak zadejte vlastní název účtu.

        New-AzureRmStorageAccount -ResourceGroupName ManageGroup -Name storagecontosotemplates -Type Standard_LRS -Location "West US"

3. Nastavte aktuální účet úložiště.

        Set-AzureRmCurrentStorageAccount -ResourceGroupName ManageGroup -Name storagecontosotemplates

4. Vytvoření kontejneru. Oprávnění k nastavenou **vypnout** potvrzující že kontejneru je k dispozici pouze pro vlastníkem.

        New-AzureStorageContainer -Name templates -Permission Off
        
5. Přidání šablony do kontejneru.

        Set-AzureStorageBlobContent -Container templates -File c:\Azure\Templates\azuredeploy.json
        
### <a name="provide-sas-token-during-deployment"></a>Poskytnutí přidružení zabezpečení token během nasazení

Abyste mohli nasadit soukromé šablony v účtu úložiště, načtěte token přidružení zabezpečení a zahrnout do URI šablony.

1. Pokud jste změnili aktuálního účtu úložiště, nastavte aktuální účet úložiště snímek obsahující šablon.

        Set-AzureRmCurrentStorageAccount -ResourceGroupName ManageGroup -Name storagecontosotemplates

2. Vytvoření přidružení zabezpečení token s oprávnění pro čtení a čas ukončení omezit přístup. Načtení úplné URI šabloně včetně token přidružení zabezpečení.

        $templateuri = New-AzureStorageBlobSASToken -Container templates -Blob azuredeploy.json -Permission r -ExpiryTime (Get-Date).AddHours(2.0) -FullUri

3. Nasazení šablony zadáním identifikátor URI, který obsahuje token přidružení zabezpečení.

        New-AzureRmResourceGroupDeployment -ResourceGroupName ExampleResourceGroup -TemplateUri $templateuri

Příklad použití token přidružení zabezpečení propojené šablonami najdete v článku [použití šablon propojené s správce prostředků Azure](resource-group-linked-templates.md).

[AZURE.INCLUDE [resource-manager-parameter-file](../includes/resource-manager-parameter-file.md)]

## <a name="parameter-precedence"></a>Parametr priority

Můžete použít vložené parametry a soubor místní parametrů v rámci jedné operace nasazení. Můžete třeba zadat některé hodnoty v souboru místní parametrů a přidat další hodnoty vložené během nasazení. Zadání hodnoty parametru v souboru místní parametrů a vložené hodnotu vložené přednost.

Vložené parametry však nelze použít se souborem externí parametr. Při zadání parametru souboru v parametru **TemplateParameterUri** všechny parametry vložené jsou ignorovány. Je nutné zadat všechny hodnoty parametrů v externím souboru. Pokud šablona obsahuje citlivé hodnota, která v souboru parametrů se nedá vložit, přidejte tuto hodnotu do klíčové trezoru a odkaz klíčové trezoru v souboru externí parametr nebo dynamicky poskytovat všechny textu hodnoty parametrů.

Podrobné informace o použití odkazu na KeyVault k předání zabezpečené hodnot najdete v článku [předání zabezpečené hodnot během nasazení](resource-manager-keyvault-parameter.md).

## <a name="next-steps"></a>Další kroky
- Příklad nasazení prostředků přes klienta knihovnu .NET najdete v článku [nasazení zdrojů pomocí knihoven .NET a šablony](virtual-machines/virtual-machines-windows-csharp-template.md).
- Definice parametrů v šabloně, najdete v článku [vytváření šablon](resource-group-authoring-templates.md#parameters).
- Nasazení řešení různých prostředích, najdete v článku [test prostředí v Microsoft Azure a vývoj](solution-dev-test-environments.md).

