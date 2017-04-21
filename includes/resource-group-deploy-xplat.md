## <a name="how-to-deploy-with-azure-cli"></a>Nasazení s Azure rozhraní příkazového řádku

1. Přihlaste se ke svému účtu Azure.

        azure login

  Po zadání přihlašovacích údajů, příkaz Umocní přihlašovací jméno.

        ...
        info:    login command OK

2. Pokud máte víc předplatných, zadejte id předplatného, které chcete použít pro nasazení.

        azure account set <YourSubscriptionNameOrId>

3. Přepnutí na modul Azure správce prostředků

        azure config mode arm

   Zobrazí se potvrzovací nový režim.

        info:     New mode is arm

4. Pokud nemáte existující skupiny zdrojů, vytvoření nové skupiny prostředků. Zadejte název pole Skupina zdroje a umístění, které potřebujete pro řešení.

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

5. Pokud chcete vytvořit nové nasazení skupiny zdrojů, spusťte tento příkaz a zadejte potřebné parametry. Parametry bude obsahovat název nasazení, název skupiny zdrojů, cestu nebo adresu URL šablony, kterou jste vytvořili a všechny ostatní parametry funkce potřebné pro nefunguje.

   Máte k dispozici následující možnosti umožňující hodnoty parametrů:

   - Použití vloženého parametrů a místní šablonu.

             azure group deployment create -f <PathToTemplate> {"ParameterName":"ParameterValue"} -g ExampleResourceGroup -n ExampleDeployment

   - Použití vloženého parametrů a odkaz na šablonu.

             azure group deployment create --template-uri <LinkToTemplate> {"ParameterName":"ParameterValue"} -g ExampleResourceGroup -n ExampleDeployment

   - Použití parametru souboru.

             azure group deployment create -f <PathToTemplate> -e <PathToParameterFile> -g ExampleResourceGroup -n ExampleDeployment

  Po nasazení skupina zdroje se najdete v článku Přehled nasazení.

         info:    Executing command group deployment create
         + Initializing template configurations and parameters
         + Creating a deployment
         ...
         info:    group deployment create command OK


6. Chcete-li získat informace o nejnovějších nasazení.

         azure group log show -l ExampleResourceGroup

7. Pro získání podrobných informací o selhání nasazení.

         azure group log show -l -v ExampleResourceGroup
