<properties
    pageTitle="Nasazení OM pomocí C# a šablony správce prostředků | Microsoft Azure"
    description="Zjistěte, jak můžete pomocí C# a šablony správce prostředků pro nasazení OM Azure."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="davidmu1"
    manager="timlt"
    editor="tysonn"
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="na"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/10/2016"
    ms.author="davidmu"/>

# <a name="deploy-an-azure-virtual-machine-using-c-and-a-resource-manager-template"></a>Nasazení Azure virtuálního počítače pomocí C# a šablony správce prostředků

Pomocí šablon a zdrojů skupin, budete moct spravovat všechny zdroje společně podporujících aplikaci. Tento článek popisuje, jak pomocí aplikace Visual Studio a C# nastavit ověřování, vytvořte šablonu a nasazení Azure zdrojů pomocí šablony, kterou jste vytvořili.

Nejprve zkontrolujte, jestli že už máte hotové tyto kroky:

- Instalace [aplikace Visual Studio](http://msdn.microsoft.com/library/dd831853.aspx)
- Ověření instalace [systému Windows Management Framework 3.0](http://www.microsoft.com/download/details.aspx?id=34595) nebo [Windows Management Framework 4.0](http://www.microsoft.com/download/details.aspx?id=40855)
- Získání [ověřovací token](../resource-group-authenticate-service-principal.md)
- Vytvoření skupiny zdrojů pomocí [Prostředí PowerShell Azure](../resource-group-template-deploy.md), [Rozhraní příkazového řádku Azure](../resource-group-template-deploy-cli.md)nebo [Azure portálu](../resource-group-template-deploy-portal.md).

Udělejte tyto kroky trvá asi 30 minut.
    
## <a name="step-1-create-the-visual-studio-project-the-template-file-and-the-parameters-file"></a>Krok 1: Vytvoření projektů Visual Studiu, soubor šablony a souboru parametrů

### <a name="create-the-template-file"></a>Vytvoření šablony souboru

Správce prostředků Azure šablona umožňuje nasazením a správou Azure zdroje společně. Šablona je JSON popis zdroje a parametrů přidružené nasazení.

Ve Visual Studiu postupujte takto:

1. Klikněte na **soubor** > **nové** > **projektu**.

2. V **šablonách** > **Visual Basic**, zvolte **Aplikace konzoly**, zadejte název a umístění projektu a potom klikněte na **OK**.

3. Klikněte pravým tlačítkem myši na název projektu v Průzkumníku řešení, klikněte na tlačítko **Přidat** > **Nová položka**.

4. Klikněte na webu, vyberte soubor JSON, zadejte *VirtualMachineTemplate.json* pro název a klikněte na tlačítko **Přidat**.

5. V levé a pravé závorky souboru VirtualMachineTemplate.json přidejte požadované schéma a část požadované contentVersion:

        {
          "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json",
          "contentVersion": "1.0.0.0",
        }

6. [Parametry](../resource-group-authoring-templates.md#parameters) vždy nejsou povinná, ale poskytují způsob, jak vstupní hodnoty při zavedení šabloně. Přidání prvku parametry a podřízené elementy po contentVersion element:

        {
          "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json",
          "contentVersion": "1.0.0.0",
          "parameters": {
            "adminUserName": { "type": "string" },
            "adminPassword": { "type": "securestring" }
          },
        }

7. [Proměnné](../resource-group-authoring-templates.md#variables) lze do šablony zadejte hodnoty, které se mohou často měnit nebo hodnoty, které je potřeba vytvořit z kombinací hodnoty parametrů. Přidání prvku proměnné pod oddílem parametry:

        {
          "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json",
          "contentVersion": "1.0.0.0",
          "parameters": {
            "adminUsername": { "type": "string" },
            "adminPassword": { "type": "securestring" }
          },
          "variables": {
            "vnetID":"[resourceId('Microsoft.Network/virtualNetworks','myvn1')]",
            "subnetRef": "[concat(variables('vnetID'),'/subnets/mysn1')]"  
          },
        }

8. [Zdroje](../resource-group-authoring-templates.md#resources) , jako jsou virtuální počítač, virtuální sítě a úložiště účtu se definují další šablony. Přidání části zdroje pod oddílem proměnné:

        {
          "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json",
          "contentVersion": "1.0.0.0",
          "parameters": {
            "adminUsername": { "type": "string" },
            "adminPassword": { "type": "securestring" }
          },
          "variables": {
            "vnetID":"[resourceId('Microsoft.Network/virtualNetworks','myvn1')]",
            "subnetRef": "[concat(variables('vnetID'),'/subnets/mysn1')]"
          },
          "resources": [
            {
              "type": "Microsoft.Storage/storageAccounts",
              "name": "mystorage1",
              "apiVersion": "2015-06-15",
              "location": "[resourceGroup().location]",
              "properties": { "accountType": "Standard_LRS" }
            },
            {
              "apiVersion": "2016-03-30",
              "type": "Microsoft.Network/publicIPAddresses",
              "name": "myip1",
              "location": "[resourceGroup().location]",
              "properties": {
                "publicIPAllocationMethod": "Dynamic",
                "dnsSettings": { "domainNameLabel": "mydns1" }
              }
            },
            {
              "apiVersion": "2016-03-30",
              "type": "Microsoft.Network/virtualNetworks",
              "name": "myvnet1",
              "location": "[resourceGroup().location]",
              "properties": {
                "addressSpace": { "addressPrefixes": [ "10.0.0.0/16" ] },
                "subnets": [ {
                  "name": "mysn1",
                  "properties": { "addressPrefix": "10.0.0.0/24" }
                } ]
              }
            },
            {
              "apiVersion": "2016-03-30",
              "type": "Microsoft.Network/networkInterfaces",
              "name": "mync1",
              "location": "[resourceGroup().location]",
              "dependsOn": [
                "Microsoft.Network/publicIPAddresses/myip1",
                "Microsoft.Network/virtualNetworks/myvn1"
              ],
              "properties": {
                "ipConfigurations": [ {
                  "name": "ipconfig1",
                  "properties": {
                    "privateIPAllocationMethod": "Dynamic",
                    "publicIPAddress": {
                      "id": "[resourceId('Microsoft.Network/publicIPAddresses', 'myip1')]"
                    },
                    "subnet": { "id": "[variables('subnetRef')]" }
                  }
                } ]
              }
            },
            {
              "apiVersion": "2016-03-30",
              "type": "Microsoft.Compute/virtualMachines",
              "name": "myvm1",
              "location": "[resourceGroup().location]",
              "dependsOn": [
                "Microsoft.Network/networkInterfaces/mync1",
                "Microsoft.Storage/storageAccounts/mystorage1"
              ],
              "properties": {
                "hardwareProfile": { "vmSize": "Standard_A1" },
                "osProfile": {
                  "computerName": "myvm1",
                  "adminUsername": "[parameters('adminUsername')]",
                  "adminPassword": "[parameters('adminPassword')]"
                },
                "storageProfile": {
                  "imageReference": {
                    "publisher": "MicrosoftWindowsServer",
                    "offer": "WindowsServer",
                    "sku": "2012-R2-Datacenter",
                    "version" : "latest"
                  },
                  "osDisk": {
                    "name": "myosdisk1",
                    "vhd": {
                      "uri": "https://mystorage1.blob.core.windows.net/vhds/myosdisk1.vhd"
                    },
                    "caching": "ReadWrite",
                    "createOption": "FromImage"
                  }
                },
                "networkProfile": {
                  "networkInterfaces" : [ {
                    "id": "[resourceId('Microsoft.Network/networkInterfaces','mync1')]"
                  } ]
                }
              }
            } ]
          }
      
9. Uložte soubor šablony, kterou jste vytvořili.

### <a name="create-the-parameters-file"></a>Vytvoření souboru parametrů

Chcete-li zadat hodnoty parametrů zdroje, které byly definované v šabloně, vytvoříte soubor parametrů obsahující hodnoty, které se používají při nasazení šablony. Ve Visual Studiu postupujte takto:

1. Klikněte pravým tlačítkem myši na název projektu v Průzkumníku řešení, klikněte na tlačítko **Přidat** > **Nová položka**.

2. Klikněte na webu, vyberte soubor JSON, zadejte *Parameters.json* pro název a klikněte na tlačítko **Přidat**.

3. Otevřete soubor parameters.json a zadejte tento obsah JSON:

        {
          "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json",
          "contentVersion": "1.0.0.0",
          "parameters": {
            "adminUserName": { "value": "mytestacct1" },
            "adminPassword": { "value": "mytestpass1" }
          }
        }

    >[AZURE.NOTE] Tento článek vytvoří virtuální počítač s verzí operačního systému Windows Server. Další informace o výběru dalších obrázků najdete v tématu [Navigace a obrázky vyberte Azure virtuální počítač s Windows Powershellu a rozhraní příkazového řádku Azure](virtual-machines-linux-cli-ps-findimage.md).

4. Uložte soubor parametry, který jste vytvořili.

## <a name="step-2-install-the-libraries"></a>Krok 2: Nainstalujte knihovny

Nejjednodušší způsob, jak nainstalovat knihoven, které je potřeba dokončit tento kurz jsou NuGet balíčky. Potřebujete Azure knihovny správy zdrojů a Azure Active Directory Authentication Library vytvoření zdrojů. Chcete-li získat těchto knihoven ve Visual Studiu, udělejte tyto kroky:

1. Klikněte pravým tlačítkem myši na název projektu v Průzkumníku řešení, klikněte na **Spravovat balíčků NuGet**a potom klikněte na Procházet.

2. Typ *Služby Active Directory* do vyhledávacího pole, klikněte na tlačítko **instalovat** Active Directory Authentication Library balíčku a postupujte podle pokynů k instalaci balíčku.

4. V horní části stránky vyberte **Zahrnout zkušební**. Typ *Microsoft.Azure.Management.ResourceManager* do pole Hledat klikněte na tlačítko **instalovat** pro knihovny správy Microsoft Azure zdroje a pak postupujte podle pokynů nainstalujte balíček.

Teď jste připraveni začít používat knihoven k vytvoření aplikace.

## <a name="step-3-create-the-credentials-that-are-used-to-authenticate-requests"></a>Krok 3: Vytvoření přihlašovací údaje, které se používají k ověření požadavky

Vytvoření aplikace služby Azure Active Directory a je nainstalovaný knihovnu ověřování. Teď můžete formátovat informací aplikace do přihlašovací údaje, které slouží k ověření žádosti správce prostředků Azure.

1. Otevřete soubor Program.cs pro projekt, který jste vytvořili a přidejte tyto pomocí příkazů do horní části souboru:

        using Microsoft.Azure;
        using Microsoft.IdentityModel.Clients.ActiveDirectory;
        using Microsoft.Azure.Management.ResourceManager;
        using Microsoft.Azure.Management.ResourceManager.Models;
        using Microsoft.Rest;
        using System.IO;

2.  Přidejte tato metoda třídy Program pro získání token, který je potřeba k vytvoření pověření:

        private static async Task<AuthenticationResult> GetAccessTokenAsync()
        {
          var cc = new ClientCredential("{client-id}", "{client-secret}");
          var context = new AuthenticationContext("https://login.windows.net/{tenant-id}");
          var token = await context.AcquireTokenAsync("https://management.azure.com/", cc);
          if (token == null)
          {
            throw new InvalidOperationException("Could not get the token.");
          }
          return token;
        }

    {Id klienta} nahraďte identifikátor Azure Active Directory aplikace {klient tajná} s přístupová klávesa AD aplikace a {klienta id} identifikátorem klienta pro vaše předplatné. Id klienta můžete najít spuštěním Get-AzureRmSubscription. Přístupová klávesa můžete najít pomocí portálu Azure.

3. Pokud chcete vytvořit její přihlašovací údaje, přidejte tento kód metody hlavní Program.cs souboru:

        var token = GetAccessTokenAsync();
        var credential = new TokenCredentials(token.Result.AccessToken);

4. Uložte soubor Program.cs.

## <a name="step-4-deploy-the-template"></a>Krok 4: Nasazení šablony

V tomto kroku použijete skupina zdroje, který jste vytvořili, ale můžete taky vytvořit skupinu zdrojů pomocí [ResourceGroup](https://msdn.microsoft.com/library/azure/microsoft.azure.management.resources.models.resourcegroup.aspx) a třídy [ResourceManagementClient](https://msdn.microsoft.com/library/azure/microsoft.azure.management.resources.resourcemanagementclient.aspx) .

1. Přidání proměnné metody hlavní třídy Program k určení názvů zdrojů, které jste dříve vytvořili, názvu nasazení a identifikátor URI vašeho předplatného:

        var groupName = "resource group name";
        var subscriptionId = "subsciption id";
        var deploymentName = "deployment name";

    Nahraďte hodnoty název skupiny na název skupiny zdrojů. Nahraďte hodnotu deploymentName název, který chcete použít pro nasazení. Identifikátor předplatného můžete najít spuštěním Get-AzureRmSubscription.

2. Přidejte tuto metodu třídy aplikace do kterých se nasadí zdrojů Skupina zdroje pomocí šablony, kterou jste definovali:

        public static async Task<DeploymentExtended> CreateTemplateDeploymentAsync(
          TokenCredentials credential,
          string groupName,
          string deploymentName,
          string subscriptionId)
        {
          Console.WriteLine("Creating the template deployment...");
          var deployment = new Deployment();
          deployment.Properties = new DeploymentProperties
          {
            Mode = DeploymentMode.Incremental,
            Template = File.ReadAllText("..\\..\\VirtualMachineTemplate.json"),
            Parameters = File.ReadAllText("..\\..\\Parameters.json")
          };
          var resourceManagementClient = new ResourceManagementClient(credential) 
            { SubscriptionId = subscriptionId };
          return await resourceManagementClient.Deployments.CreateOrUpdateAsync(
            groupName,
            deploymentName,
            deployment);
        }

    Pokud byste chtěli nasazení šablony z účtu úložiště, můžete nahradit vlastnost TemplateLink vlastnosti šablony.

3. Volání metody, kterou jste právě přidali, přidejte tento kód metody hlavní:

        var dpResult = CreateTemplateDeploymentAsync(
          credential,
          groupName,
          deploymentName,
          subscriptionId);
        Console.WriteLine(dpResult.Result.Properties.ProvisioningState);
        Console.ReadLine();

## <a name="step-5-delete-the-resources"></a>Krok 5: Odstranění zdroje

Protože vám bude účtovaná za zdrojů použitých v Azure, je vždy vhodné odstranit prostředky, které už nepotřebujete. Není potřeba odstranit jednotlivé zdroje zvlášť ze skupiny zdrojů. Odstranění skupiny zdrojů a všechny zdroje jsou automaticky odstraní.

1.  Odstranit skupinu zdrojů, přidejte tento způsob třídy aplikace:

        public static async void DeleteResourceGroupAsync(
          TokenCredentials credential,
          string groupName,
          string subscriptionId)
        {
          Console.WriteLine("Deleting resource group...");
          var resourceManagementClient = new ResourceManagementClient(credential)
            { SubscriptionId = subscriptionId };
          await resourceManagementClient.ResourceGroups.DeleteAsync(groupName);
        }

2.  Volání metody, kterou jste právě přidali, přidejte tento kód metody hlavní:

        DeleteResourceGroupAsync(
          credential,
          groupName,
          subscriptionId);
        Console.ReadLine();

##<a name="step-6-run-the-console-application"></a>Krok 6: Spusťte aplikaci konzoly

1.  Pokud chcete spustit aplikaci konzoly, klikněte na tlačítko **Start** ve Visual Studiu a pak se přihlaste k Azure AD pomocí stejné přihlašovací údaje, které používáte k vašemu předplatnému.

2.  Až se zobrazí stav přijaté, stiskněte klávesu **Enter** .

    By měl trvá asi pět minut pro tuto aplikaci konzoly spuštění úplně od začátku do konce. Před stisknutím klávesy Enter spusťte odstraňování zdrojů, může trvat několik minut ověřit vytvoření zdrojů v portálu Azure před odstraněním.

3. Zobrazení stavu zdrojů, přejděte v protokolech auditování Azure portálu:

    ![Procházet protokolů auditování Azure portálu](./media/virtual-machines-windows-csharp-template/crpportal.png)

## <a name="next-steps"></a>Další kroky

- Pokud došlo k problémy s nasazení, dalším krokem bude visiové [nasazení skupina zdroje Poradce při potížích s Azure portálu](../resource-manager-troubleshoot-deployments-portal.md).
- Naučte se spravovat virtuální počítač, který jste vytvořili kontrolou [Spravovat virtuálních počítačích pomocí Správce prostředků Azure a Powershellu](virtual-machines-windows-csharp-manage.md).
