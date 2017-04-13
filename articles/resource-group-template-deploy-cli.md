<properties
   pageTitle="Nasazení zdroje s Azure rozhraní příkazového řádku a šablon | Microsoft Azure"
   description="Pomocí Správce prostředků Azure a Azure rozhraní příkazového řádku pro nasazení zdroje Azure. Zdroje jsou definované v šabloně správce prostředků."
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

# <a name="deploy-resources-with-resource-manager-templates-and-azure-cli"></a>Nasazení zdroje se šablonami správce prostředků a rozhraní příkazového řádku Azure

> [AZURE.SELECTOR]
- [Prostředí PowerShell](resource-group-template-deploy.md)
- [Azure rozhraní příkazového řádku](resource-group-template-deploy-cli.md)
- [Portál](resource-group-template-deploy-portal.md)
- [ROZHRANÍ REST API](resource-group-template-deploy-rest.md)

Toto téma vysvětluje, jak pomocí služby Azure rozhraní příkazového řádku správce prostředků šablony nasadit svých prostředcích Azure.  

> [AZURE.TIP] Pokud potřebujete pomoc s ladění chybu při nasazení najdete v článku:
>
> - [Zobrazit nasazení operace s Azure rozhraní příkazového řádku](resource-manager-troubleshoot-deployments-cli.md) se naučit používat začíná informací, které vám pomůže vyřešit potíže s vaší chybovými
> - [Poradce při běžných chyb při nasazení zdrojů Azure pomocí Správce prostředků Azure](resource-manager-common-deployment-errors.md) se dozvíte, jak vyřešit běžné chyby při zavedení

Šablony můžou být místní soubor nebo externí soubor, který je k dispozici prostřednictvím identifikátor URI. Pokud vaše šablona nachází v účtu úložiště, můžete omezit přístup k šabloně a zadat sdílené přístupový token podpisu (přidružení zabezpečení) během nasazení.

## <a name="quick-steps-to-deployment"></a>Rychlé kroky k nasazení

Tento článek popisuje všechny během nasazení k dispozici různé možnosti. Ale často stačí dva jednoduché příkazy. Chcete-li rychle začít pracovat s nasazení, použijte následující příkazy:

    azure group create -n ExampleResourceGroup -l "West US"
    azure group deployment create -f <PathToTemplate> -e <PathToParameterFile> -g ExampleResourceGroup -n ExampleDeployment

Další informace o možnostech nasazení, který může být vhodnější pro nefunguje, pokračujte ve čtení tohoto článku.

[AZURE.INCLUDE [resource-manager-deployments](../includes/resource-manager-deployments.md)]

## <a name="deploy-with-azure-cli"></a>Nasazení s Azure rozhraní příkazového řádku

Pokud jste to ještě nepoužívali Azure rozhraní příkazového řádku pomocí Správce prostředků, najdete v článku [použití Azure rozhraní příkazového řádku pro Mac a Linux, Windows s Azure řízení zdrojů](xplat-cli-azure-resource-manager.md).

1. Přihlaste se k účtu Azure. Po zadání přihlašovacích údajů, příkaz Umocní přihlašovací jméno.

        azure login
  
        ...
        info:    login command OK

2. Pokud máte víc předplatných, zadejte id předplatného, které chcete použít pro nasazení.

        azure account set <YourSubscriptionNameOrId>

3. Přejděte na modul Azure správce prostředků. Dostanete potvrzení vaší nový režim.

        azure config mode arm
   
        info:     New mode is arm

4. Pokud nemáte existující skupiny zdrojů, vytvořte skupinu zdrojů. Zadejte název pole Skupina zdroje a umístění, které potřebujete pro řešení. Potřebujete poskytuje místo, kde skupina zdroje, protože skupina zdroje ukládá metadata o zdroji. Dodržování předpisů důvodů můžete určit, kde je uložena že metadata. Obecně doporučujeme, abyste zadejte umístění, kde se bude nacházet většina prostředků. Pomocí na stejném místě, můžete zjednodušit vaši šablonu.

        azure group create -n ExampleResourceGroup -l "West US"

     Vrátí souhrn nové skupiny prostředků.
   
        info:    Executing command group create
        + Getting resource group ExampleResourceGroup
        + Creating resource group ExampleResourceGroup
        info:    Created resource group ExampleResourceGroup
        data:    Id:                  /subscriptions/####/resourceGroups/ExampleResourceGroup
        data:    Name:                ExampleResourceGroup
        data:    Location:            westus
        data:    Provisioning State:  Succeeded
        data:    Tags:
        data:
        info:    group create command OK

5. Ověření nasazení před spuštěním spuštěním příkazu **Ověřit šablona azure skupiny** . Při testování nasazení, zadejte parametry přesně stejně jako při provádění nasazení (viz v dalším kroku).

        azure group template validate -f <PathToTemplate> -p "{\"ParameterName\":{\"value\":\"ParameterValue\"}}" -g ExampleResourceGroup

5. Abyste mohli nasadit zdrojů do skupiny zdrojů, spusťte tento příkaz a zadejte potřebné parametry. Parametry obsahovat název pro nasazení, název skupiny zdrojů, cestu nebo adresu URL šablony a všechny ostatní parametry funkce potřebné pro nefunguje. 
   
     Máte tři možnosti poskytují hodnoty parametrů: 

     1. Použití vloženého parametrů a místní šablonu. Každý parametr je ve formátu: `"ParameterName": { "value": "ParameterValue" }`. Následující příklad ukazuje parametry se řídicí znaky.

            azure group deployment create -f <PathToTemplate> -p "{\"ParameterName\":{\"value\":\"ParameterValue\"}}" -g ExampleResourceGroup -n ExampleDeployment

     2. Použití vloženého parametrů a odkaz na šablonu.

            azure group deployment create --template-uri <LinkToTemplate> -p "{\"ParameterName\":{\"value\":\"ParameterValue\"}}" -g ExampleResourceGroup -n ExampleDeployment

     3. Použití parametru souboru. Informace o souboru šablony najdete v tématu [soubor parametrů](#parameter-file).
    
            azure group deployment create -f <PathToTemplate> -e <PathToParameterFile> -g ExampleResourceGroup -n ExampleDeployment

     Po zdroje již byly nasazeny prostřednictvím jedním ze tří způsobů výše uvedené, zobrazí se přehled nasazení.
  
        info:    Executing command group deployment create
        + Initializing template configurations and parameters
        + Creating a deployment
        ...
        info:    group deployment create command OK

     Dokončení nasazení spustíte nastavení **režimu** **Dokončeno**. Buďte opatrní při použití tohoto režimu, jak můžete neúmyslně odstraníte prostředky, které nejsou v šabloně.

        azure group deployment create --mode Complete -f <PathToTemplate> -e <PathToParameterFile> -g ExampleResourceGroup -n ExampleDeployment

6. Pokud chcete zaznamenávat další informace o nasazení, které vám mohou pomoci Poradce při potížích s chyby nasazení, použít parametr **ladění nastavení** . Můžete určit, že žádosti o obsah nebo obsah odpověď Zaprotokolují s operaci nasazení.

        azure group deployment create --debug-setting All -f <PathToTemplate> -e <PathToParameterFile> -g ExampleResourceGroup -n ExampleDeployment

## <a name="deploy-template-from-storage-with-sas-token"></a>Nasazení šablony z úložiště s token přidružení zabezpečení

Šablony můžete přidat k účtu úložiště a propojení jejích během nasazení s tokem přidružení zabezpečení.

> [AZURE.IMPORTANT] Pomocí následujícího postupu, bude přístupný pro jenom vlastník účtu objektů blob obsahující šablonu. Však při vytváření token přidružení zabezpečení objektů blob objektů blob přístupný pro každý, kdo má tuto URI. Pokud jiný uživatel zachytí identifikátor URI, tento uživatel má přístup k šabloně. Použití token přidružení zabezpečení je vhodná omezení přístupu k šablonám, ale nezahrnujte citlivá data například hesla přímo do šablony.

### <a name="add-private-template-to-storage-account"></a>Přidání soukromé šablony k účtu úložiště

Nastavení účtu úložiště šablon týkajících se podle těchto kroků:

1. Vytvoření skupiny prostředků.

        azure group create -n "ManageGroup" -l "westus"

2. Vytvoření účtu úložiště. Název účtu úložiště musí být jedinečný přes Azure, tak zadejte vlastní název účtu.

        azure storage account create -g ManageGroup -l "westus" --sku-name LRS --kind Storage storagecontosotemplates

3. Nastavení proměnné pro úložiště klienta a klíče.

        export AZURE_STORAGE_ACCOUNT=storagecontosotemplates
        export AZURE_STORAGE_ACCESS_KEY={storage_account_key}

4. Vytvoření kontejneru. Oprávnění k nastavenou **vypnout** potvrzující že kontejneru je k dispozici pouze pro vlastníkem.

        azure storage container create --container templates -p Off 
        
4. Přidání šablony do kontejneru.

        azure storage blob upload --container templates -f c:\Azure\Templates\azuredeploy.json
        
### <a name="provide-sas-token-during-deployment"></a>Poskytnutí přidružení zabezpečení token během nasazení

Abyste mohli nasadit soukromé šablony v účtu úložiště, načtěte token přidružení zabezpečení a zahrnout do URI šablony.

1. Vytvoření přidružení zabezpečení token s oprávnění pro čtení a čas ukončení omezit přístup. Nastavte hodnotu time vypršení platnosti umožňuje dostatek času na nasazení. Načtení úplné URI šabloně včetně token přidružení zabezpečení.

        expiretime=$(date -I'minutes' --date "+30 minutes")
        fullurl=$(azure storage blob sas create --container templates --blob azuredeploy.json --permissions r --expiry $expiretimetime --json  | jq ".url")

2. Nasazení šablony zadáním identifikátor URI, který obsahuje token přidružení zabezpečení.

        azure group deployment create --template-uri $fullurl -g ExampleResourceGroup

Příklad použití token přidružení zabezpečení propojené šablonami najdete v článku [použití šablon propojené s správce prostředků Azure](resource-group-linked-templates.md).

[AZURE.INCLUDE [resource-manager-parameter-file](../includes/resource-manager-parameter-file.md)]

## <a name="next-steps"></a>Další kroky
- Příklad nasazení prostředků přes klienta knihovnu .NET najdete v článku [nasazení zdrojů pomocí knihoven .NET a šablony](virtual-machines/virtual-machines-windows-csharp-template.md).
- Definice parametrů v šabloně, najdete v článku [vytváření šablon](resource-group-authoring-templates.md#parameters).
- Nasazení řešení různých prostředích, najdete v článku [test prostředí v Microsoft Azure a vývoj](solution-dev-test-environments.md).
- Podrobné informace o použití odkazu na KeyVault k předání zabezpečené hodnot najdete v článku [předání zabezpečené hodnot během nasazení](resource-manager-keyvault-parameter.md).

