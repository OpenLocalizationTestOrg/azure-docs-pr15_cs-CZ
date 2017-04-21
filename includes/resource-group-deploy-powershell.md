## <a name="how-to-deploy-with-powershell"></a>Nasazení pomocí prostředí PowerShell

1. Přihlaste se ke svému účtu Azure.

          Add-AzureAccount

   Po zadání přihlašovacích údajů, příkaz vrátí informace o účtu.

          Id                             Type       ...
          --                             ----    
          example@contoso.com            User       ...   

2. Pokud máte víc předplatných, zadejte id předplatného, které chcete použít pro nasazení. 

          Select-AzureSubscription -SubscriptionID <YourSubscriptionId>

3. Přejděte na modul Azure správce prostředků.

          Switch-AzureMode AzureResourceManager

4. Pokud nemáte existující skupiny zdrojů, vytvoření nové skupiny prostředků. Zadejte název pole Skupina zdroje a umístění, které potřebujete pro řešení.

        New-AzureResourceGroup -Name ExampleResourceGroup -Location "West US"

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

5. Pokud chcete vytvořit nové nasazení skupiny zdrojů, spuštění příkazu **Nový AzureResourceGroupDeployment** a zadejte potřebné parametry. Parametry bude obsahovat název nasazení, název skupiny zdrojů, cestu nebo adresu URL šablony, kterou jste vytvořili a všechny ostatní parametry funkce potřebné pro nefunguje. 
   
   Máte k dispozici následující možnosti umožňující hodnoty parametrů: 
   
   - Použití vloženého parametrů.

            New-AzureResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName ExampleResourceGroup -TemplateFile <PathOrLinkToTemplate> -myParameterName "parameterValue"

   - Použijte parametr objektu.

            $parameters = @{"<ParameterName>"="<Parameter Value>"}
            New-AzureResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName ExampleResourceGroup -TemplateFile <PathOrLinkToTemplate> -TemplateParameterObject $parameters

   - Použití parametru souborů.

            New-AzureResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName ExampleResourceGroup -TemplateFile <PathOrLinkToTemplate> -TemplateParameterFile <PathOrLinkToParameterFile>

  Po nasazení skupina zdroje se najdete v článku Přehled nasazení.

             DeploymentName    : ExampleDeployment
             ResourceGroupName : ExampleResourceGroup
             ProvisioningState : Succeeded
             Timestamp         : 4/14/2015 7:00:27 PM
             Mode              : Incremental
             ...

6. Chcete-li získat informace o selhání nasazení.

        Get-AzureResourceGroupLog -ResourceGroup ExampleResourceGroup -Status Failed

7. Pro získání podrobných informací o selhání nasazení.

        Get-AzureResourceGroupLog -ResourceGroup ExampleResourceGroup -Status Failed -DetailedOutput
