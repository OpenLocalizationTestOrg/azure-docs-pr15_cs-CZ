## <a name="deploy-the-arm-template-by-using-the-azure-cli"></a>Nasazení šablony ARM pomocí rozhraní příkazového řádku Azure

Abyste mohli nasadit ARM šablonu, kterou jste si stáhli pomocí rozhraní příkazového řádku Azure, postupujte následujícím způsobem.

1. Pokud máte Azure rozhraní příkazového řádku, přečtěte si téma [instalace a konfigurace Azure rozhraní příkazového řádku](../articles/xplat-cli-install.md) a postupujte podle pokynů až do okamžiku, kdy vyberte svůj účet Azure a předplatné.
2. Spustit **`azure config mode`** příkaz přepněte do režimu správce prostředků, jak je ukázáno v následujícím příkladu.

        azure config mode arm

    Tady je očekávané výstup pro výše uvedené příkaz:

        info:    New mode is arm

3. V případě potřeby spustit **`azure group create`** k vytvoření nové skupiny prostředků, jak je ukázáno v následujícím příkladu. Všimněte si výstup příkazu. Seznam zobrazený po výstup vysvětluje parametry použité. Další informace o skupinách prostředků Navštěvujte blog o [Přehled Správce prostředků Azure](../articles/resource-group-overview.md).

        azure group create -n TestRG -l centralus

    Tady je očekávané výstup pro výše uvedené příkaz:

        info:    Executing command group create
        + Getting resource group TestRG
        + Creating resource group TestRG
        info:    Created resource group TestRG
        data:    Id:                  /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG
        data:    Name:                TestRG
        data:    Location:            centralus
        data:    Provisioning State:  Succeeded
        data:    Tags: null
        data:
        info:    group create command OK

    - **n (nebo – název)**. Název nové skupiny prostředků. Naše scénář *TestRG*.
    - **-l (nebo – umístění)**. Azure oblast, kde se bude vytvořena nová skupina zdrojů. Pro naše scénář *centralus*.

4. Spustit **`azure group deployment create`** rutina nasazení nové VNet pomocí šablony a parametr soubory můžete stáhnout a kterou nad. Seznam zobrazený po výstup vysvětluje parametry použité.

        azure group deployment create -g TestRG -n TestVNetDeployment -f C:\ARM\azuredeploy.json -e C:\ARM\azuredeploy-parameters.json

    Tady je očekávané výstup pro výše uvedené příkaz:

        info:    Executing command group deployment create
        + Initializing template configurations and parameters
        + Creating a deployment
        info:    Created template deployment "TestVNetDeployment"
        + Registering providers
        info:    Registering provider microsoft.network
        + Waiting for deployment to complete
        data:    DeploymentName     : TestVNetDeployment
        data:    ResourceGroupName  : TestRG
        data:    ProvisioningState  : Succeeded
        data:    Timestamp          : 2015-08-14T21:56:11.152759Z
        data:    Mode               : Incremental
        data:    Name           Type    Value
        data:    -------------  ------  --------------
        data:    location       String  Central US
        data:    vnetName       String  TestVNet
        data:    addressPrefix  String  192.168.0.0/16
        data:    subnet1Prefix  String  192.168.1.0/24
        data:    subnet1Name    String  FrontEnd
        data:    subnet2Prefix  String  192.168.2.0/24
        data:    subnet2Name    String  BackEnd
        info:    group deployment create command OK

    - **-g (nebo – skupina zdroje)**. Název skupiny prostředků nové VNet vytvoří v.
    - **-f (nebo – soubor šablony)**. Cesta k souboru šablony ARM.
    - **-e (nebo – souboru parametrů)**. Cesta k souboru ARM parametry.

5. Spustit **`azure network vnet show`** příkaz zobrazte vlastnosti nové vnet, jak je ukázáno v následujícím příkladu.

        azure network vnet show -g TestRG -n TestVNet

    Tady je očekávané výstup pro výše uvedené příkaz:

        info:    Executing command network vnet show
        + Looking up virtual network "TestVNet"
        data:    Id                              : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet
        data:    Name                            : TestVNet
        data:    Type                            : Microsoft.Network/virtualNetworks
        data:    Location                        : centralus
        data:    ProvisioningState               : Succeeded
        data:    Address prefixes:
        data:      192.168.0.0/16
        data:    Subnets:
        data:      Name                          : FrontEnd
        data:      Address prefix                : 192.168.1.0/24
        data:
        data:      Name                          : BackEnd
        data:      Address prefix                : 192.168.2.0/24
        data:
        info:    network vnet show command OK
