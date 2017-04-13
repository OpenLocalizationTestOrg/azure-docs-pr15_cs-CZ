<properties
    pageTitle="Nasazení Azure zdrojů prostřednictvím C# | Microsoft Azure"
    description="Naučte se používat C# a správce prostředků Azure k vytvoření Microsoft Azure zdroje."
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
    ms.date="10/06/2016"
    ms.author="davidmu"/>

# <a name="deploy-azure-resources-using-c"></a>Nasazení Azure zdrojů prostřednictvím C# 

Tento článek popisuje, jak vytvořit Azure zdrojů prostřednictvím C#.

Nejprve zkontrolujte, jestli že budete mít tyto úkoly:

- Instalace [aplikace Visual Studio](http://msdn.microsoft.com/library/dd831853.aspx)
- Ověření instalace [systému Windows Management Framework 3.0](http://www.microsoft.com/download/details.aspx?id=34595) nebo [Windows Management Framework 4.0](http://www.microsoft.com/download/details.aspx?id=40855)
- Získání [ověřovací token](../resource-group-authenticate-service-principal.md)

Udělejte tyto kroky trvá asi 30 minut.

## <a name="step-1-create-a-visual-studio-project-and-install-the-libraries"></a>Krok 1: Vytvoření projektu Visual Studiu a nainstalujte knihoven

Nejjednodušší způsob, jak nainstalovat knihoven, které je potřeba dokončit tento kurz jsou NuGet balíčky. Chcete-li získat knihoven, které potřebujete ve Visual Studiu, udělejte tyto kroky:

1. Klikněte na **soubor** > **nové** > **projektu**.

2. V **šablonách** > **Visual Basic**, zvolte **Aplikace konzoly**, zadejte název a umístění projektu a potom klikněte na **OK**.

3. Klikněte pravým tlačítkem myši na název projektu v Průzkumníku řešení a potom klikněte na **Spravovat balíčků NuGet**.

4. Typ *Služby Active Directory* do vyhledávacího pole, klikněte na tlačítko **instalovat** Active Directory Authentication Library balíčku a postupujte podle pokynů k instalaci balíčku.

5. V horní části stránky vyberte **Zahrnout zkušební**. Typ *Microsoft.Azure.Management.Compute* do vyhledávacího pole pro výpočet .NET knihovny klikněte na tlačítko **instalovat** a pak postupujte podle pokynů nainstalujte balíček.

6. Typ *Microsoft.Azure.Management.Network* do vyhledávacího pole, klikněte na tlačítko **instalovat** pro knihovny .NET sítě a postupujte podle pokynů k instalaci balíčku.

7. Typ *Microsoft.Azure.Management.Storage* do vyhledávacího pole, klikněte na tlačítko **instalovat** pro knihovny .NET úložiště a postupujte podle pokynů k instalaci balíčku.

8. Do vyhledávacího pole zadejte *Microsoft.Azure.Management.ResourceManager* , klikněte na tlačítko **instalovat** pro knihovny správy zdrojů.

Teď jste připraveni začít používat knihoven k vytvoření aplikace.

## <a name="step-2-create-the-credentials-that-are-used-to-authenticate-requests"></a>Krok 2: Vytvoření přihlašovací údaje, které se používají k ověření požadavky

Teď můžete formátovat informace o aplikaci, že jste vytvořili do přihlašovací údaje, které slouží k ověření požadavků pro správce prostředků Azure.

1. Otevřete soubor Program.cs pro projekt, který jste vytvořili a přidejte tyto pomocí příkazů do horní části souboru:

        using Microsoft.Azure;
        using Microsoft.IdentityModel.Clients.ActiveDirectory;
        using Microsoft.Azure.Management.ResourceManager;
        using Microsoft.Azure.Management.ResourceManager.Models;
        using Microsoft.Azure.Management.Storage;
        using Microsoft.Azure.Management.Storage.Models;
        using Microsoft.Azure.Management.Network;
        using Microsoft.Azure.Management.Network.Models;
        using Microsoft.Azure.Management.Compute;
        using Microsoft.Azure.Management.Compute.Models;
        using Microsoft.Rest;

2. Pokud chcete vytvořit token, který je potřeba, přidejte tento způsob třídy aplikace:

        private static async Task<AuthenticationResult> GetAccessTokenAsync()
        {
          var cc = new ClientCredential("{client-id}", "{client-secret}");
          var context = new AuthenticationContext("https://login.windows.net/{tenant-id}");
          var token = await context.AcquireTokenAsync("https://management.azure.com/", cc);
          if (token == null)
          {
            throw new InvalidOperationException("Could not get the token");
          }
          return token;
        }

    {Id klienta} nahraďte identifikátor Azure Active Directory aplikace {klient tajná} s přístupová klávesa AD aplikace a {klienta id} identifikátorem klienta pro vaše předplatné. Id klienta můžete najít spuštěním Get-AzureRmSubscription. Přístupová klávesa můžete najít pomocí portálu Azure.

3. Zavolejte si metodu, která jste dříve přidali, přidejte tento kód metody hlavní v souboru Program.cs:

        var token = GetAccessTokenAsync();
        var credential = new TokenCredentials(token.Result.AccessToken);

4. Uložte soubor Program.cs.

## <a name="step-3-register-the-resource-providers-and-create-the-resources"></a>Krok 3: Registrace poskytovatelů zdroje a vytvoření zdrojů

### <a name="register-the-providers-and-create-a-resource-group"></a>Registrace zprostředkovatelé a vytvořit skupinu zdrojů

Všechny zdroje musí být součástí skupiny zdrojů. Před zdrojů můžete přidat do skupiny, musí být vaše předplatné registrován u poskytovatele zdroje.

1. Přidání proměnné metody hlavní třídy aplikace můžete určit, názvy, které chcete použít pro zdroje:

        var groupName = "resource group name";
        var subscriptionId = "subsciption id";
        var location = "location name";
        var storageName = "storage account name";
        var ipName = "public ip name";
        var subnetName = "subnet name";
        var vnetName = "virtual network name";
        var nicName = "network interface name";
        var avSetName = "availability set name";
        var vmName = "virtual machine name";  
        var adminName = "administrator account name";
        var adminPassword = "administrator account password";
        
    Nahraďte všechny hodnotách proměnných jména a identifikátoru, který chcete použít. Identifikátor předplatného můžete najít spuštěním Get-AzureRmSubscription.

2. Vytvořit skupiny zdrojů a zaregistrovat poskytovatelů, přidejte tento způsob třídy aplikace:

        public static async Task<ResourceGroup> CreateResourceGroupAsync(
          TokenCredentials credential,
          string groupName,
          string subscriptionId,
          string location)
        {
          var resourceManagementClient = new ResourceManagementClient(credential)
            { SubscriptionId = subscriptionId };
            
          Console.WriteLine("Registering the providers...");
          var rpResult = resourceManagementClient.Providers.Register("Microsoft.Storage");
          Console.WriteLine(rpResult.RegistrationState);
          rpResult = resourceManagementClient.Providers.Register("Microsoft.Network");
          Console.WriteLine(rpResult.RegistrationState);
          rpResult = resourceManagementClient.Providers.Register("Microsoft.Compute");
          Console.WriteLine(rpResult.RegistrationState);
          
          Console.WriteLine("Creating the resource group...");
          var resourceGroup = new ResourceGroup { Location = location };
          return await resourceManagementClient.ResourceGroups.CreateOrUpdateAsync(groupName, resourceGroup);
        }

3. Zavolejte si metodu, která jste dříve přidali, přidejte tento kód metody hlavní:

        var rgResult = CreateResourceGroupAsync(
          credential,
          groupName,
          subscriptionId,
          location);
        Console.WriteLine(rgResult.Result.Properties.ProvisioningState);
        Console.ReadLine();

### <a name="create-a-storage-account"></a>Vytvoření účtu úložiště

[Účet úložiště](../storage/storage-create-storage-account.md) je potřeba k ukládání, která se vytvoří soubor virtuální pevný disk virtuální počítač.

1. Vytvoření účtu úložiště, přidejte tento způsob třídy aplikace:

        public static async Task<StorageAccount> CreateStorageAccountAsync(
          TokenCredentials credential,       
          string groupName,
          string subscriptionId,
          string location,
          string storageName)
        {
          Console.WriteLine("Creating the storage account...");
          var storageManagementClient = new StorageManagementClient(credential)
            { SubscriptionId = subscriptionId };
          return await storageManagementClient.StorageAccounts.CreateAsync(
            groupName,
            storageName,
            new StorageAccountCreateParameters()
            {
              Sku = new Microsoft.Azure.Management.Storage.Models.Sku() 
                { Name = SkuName.StandardLRS},
              Kind = Kind.Storage,
              Location = location
            }
          );
        }

2. Zavolejte si metodu, která jste dříve přidali, přidejte tento kód metody hlavní třídy aplikace:

        var stResult = CreateStorageAccountAsync(
          credential,
          groupName,
          subscriptionId,
          location,
          storageName);
        Console.WriteLine(stResult.Result.ProvisioningState);  
        Console.ReadLine();

### <a name="create-the-public-ip-address"></a>Vytvořit veřejnou IP adresu

Veřejnou IP adresu je potřeba komunikovat s virtuální počítač.

1. Pokud chcete vytvořit veřejnou IP adresu virtuální počítač, přidejte tento způsob třídy aplikace:

        public static async Task<PublicIPAddress> CreatePublicIPAddressAsync(
          TokenCredentials credential,  
          string groupName,
          string subscriptionId,
          string location,
          string ipName)
        {
          Console.WriteLine("Creating the public ip...");
          var networkManagementClient = new NetworkManagementClient(credential)
            { SubscriptionId = subscriptionId };
          return await networkManagementClient.PublicIPAddresses.CreateOrUpdateAsync(
            groupName,
            ipName,
            new PublicIPAddress
            {
              Location = location,
              PublicIPAllocationMethod = "Dynamic"
            }
          );
        }

2. Zavolejte si metodu, která jste dříve přidali, přidejte tento kód metody hlavní třídy aplikace:

        var ipResult = CreatePublicIPAddressAsync(
          credential,
          groupName,
          subscriptionId,
          location,
          ipName);
        Console.WriteLine(ipResult.Result.ProvisioningState);  
        Console.ReadLine();

### <a name="create-the-virtual-network"></a>Vytvořit virtuální sítě

Virtuální počítač, který je vytvořený pomocí Správce prostředků nasazení modelu musí být v virtuální sítě.

1. Pokud chcete vytvořit podsítě a virtuální sítě, přidejte tento způsob třídy aplikace:

        public static async Task<VirtualNetwork> CreateVirtualNetworkAsync(
          TokenCredentials credential,
          string groupName,
          string subscriptionId,
          string location,
          string vnetName,
          string subnetName)
        {
          Console.WriteLine("Creating the virtual network...");
          var networkManagementClient = new NetworkManagementClient(credential)
            { SubscriptionId = subscriptionId };
          
          var subnet = new Subnet
          {
            Name = subnetName,
            AddressPrefix = "10.0.0.0/24"
          };
          
          var address = new AddressSpace {
            AddressPrefixes = new List<string> { "10.0.0.0/16" }
          };
          
          return await networkManagementClient.VirtualNetworks.CreateOrUpdateAsync(
            groupName,
            vnetName,
            new VirtualNetwork
            {
              Location = location,
              AddressSpace = address,
              Subnets = new List<Subnet> { subnet }
            }
          );
        }
        
2. Zavolejte si metodu, která jste dříve přidali, přidejte tento kód metody hlavní třídy aplikace:

        var vnResult = CreateVirtualNetworkAsync(
          credential,
          groupName,
          subscriptionId,
          location,
          vnetName,
          subnetName);
        Console.WriteLine(vnResult.Result.ProvisioningState);  
        Console.ReadLine();
        
### <a name="create-the-network-interface"></a>Vytvoření rozhraní sítě

Virtuální počítač vyžaduje rozhraní sítě komunikovat v síti virtuální.

1. Aby mohlo vzniknout rozhraní sítě, přidejte tento způsob třídy aplikace:

        public static async Task<NetworkInterface> CreateNetworkInterfaceAsync(
          TokenCredentials credential,
          string groupName,
          string subscriptionId,
          string location,
          string subnetName,
          string vnetName,
          string ipName,
          string nicName)
        {
          Console.WriteLine("Creating the network interface...");
          var networkManagementClient = new NetworkManagementClient(credential)
            { SubscriptionId = subscriptionId };
          var subnetResponse = await networkManagementClient.Subnets.GetAsync(
            groupName,
            vnetName,
            subnetName
          );
          var pubipResponse = await networkManagementClient.PublicIPAddresses.GetAsync(groupName, ipName);

          return await networkManagementClient.NetworkInterfaces.CreateOrUpdateAsync(
            groupName,
            nicName,
            new NetworkInterface
            {
              Location = location,
              IpConfigurations = new List<NetworkInterfaceIPConfiguration>
              {
                new NetworkInterfaceIPConfiguration
                {
                  Name = nicName,
                  PublicIPAddress = pubipResponse,
                  Subnet = subnetResponse
                }
              }
            }
          );
        }

2. Zavolejte si metodu, která jste dříve přidali, přidejte tento kód metody hlavní třídy aplikace:

        var ncResult = CreateNetworkInterfaceAsync(
          credential,
          groupName,
          subscriptionId,
          location,
          subnetName,
          vnetName,
          ipName,
          nicName);
        Console.WriteLine(ncResult.Result.ProvisioningState);  
        Console.ReadLine();

### <a name="create-an-availability-set"></a>Vytvoření sady dostupnosti

Dostupnost sady usnadňují Správa údržbu virtuálních počítačích používané aplikací.

1. Pokud chcete vytvořit sadu dostupnost, přidejte tento způsob třídy aplikace:

        public static async Task<AvailabilitySet> CreateAvailabilitySetAsync(
          TokenCredentials credential,
          string groupName,
          string subscriptionId,
          string location,
          string avsetName)
        {
          Console.WriteLine("Creating the availability set...");
          var computeManagementClient = new ComputeManagementClient(credential)
            { SubscriptionId = subscriptionId };
          return await computeManagementClient.AvailabilitySets.CreateOrUpdateAsync(
            groupName,
            avsetName,
            new AvailabilitySet()
            {
              Location = location
            }
          );
        }

2. Zavolejte si metodu, která jste dříve přidali, přidejte tento kód metody hlavní třídy aplikace:

        var avResult = CreateAvailabilitySetAsync(
          credential,  
          groupName,
          subscriptionId,
          location,
          avSetName);
        Console.ReadLine();

### <a name="create-a-virtual-machine"></a>Vytvoření virtuálního počítače

Teď, když jste vytvořili podpůrné materiály, můžete vytvořit virtuální počítač.

1. Pokud chcete vytvořit virtuální počítač, přidejte tento způsob třídy aplikace:

        public static async Task<VirtualMachine> CreateVirtualMachineAsync(
          TokenCredentials credential, 
          string groupName,
          string subscriptionId,
          string location,
          string nicName,
          string avsetName,
          string storageName,
          string adminName,
          string adminPassword,
          string vmName)
        {
          var networkManagementClient = new NetworkManagementClient(credential)
            { SubscriptionId = subscriptionId };
          var nic = networkManagementClient.NetworkInterfaces.Get(groupName, nicName);

          var computeManagementClient = new ComputeManagementClient(credential);
          computeManagementClient.SubscriptionId = subscriptionId;
          var avSet = computeManagementClient.AvailabilitySets.Get(groupName, avsetName);

          Console.WriteLine("Creating the virtual machine...");
          return await computeManagementClient.VirtualMachines.CreateOrUpdateAsync(
            groupName,
            vmName,
            new VirtualMachine
            {
              Location = location,
              AvailabilitySet = new Microsoft.Azure.Management.Compute.Models.SubResource
              {
                Id = avSet.Id
              },
              HardwareProfile = new HardwareProfile
              {
                VmSize = "Standard_A0"
              },
              OsProfile = new OSProfile
              {
                AdminUsername = adminName,
                AdminPassword = adminPassword,
                ComputerName = vmName,
                WindowsConfiguration = new WindowsConfiguration
                {
                  ProvisionVMAgent = true
                }
              },
              NetworkProfile = new NetworkProfile
              {
                NetworkInterfaces = new List<NetworkInterfaceReference>
                {
                  new NetworkInterfaceReference { Id = nic.Id }
                }
              },
              StorageProfile = new StorageProfile
              {
                ImageReference = new ImageReference
                {
                  Publisher = "MicrosoftWindowsServer",
                  Offer = "WindowsServer",
                  Sku = "2012-R2-Datacenter",
                  Version = "latest"
                },
                OsDisk = new OSDisk
                {
                  Name = "mytestod1",
                  CreateOption = DiskCreateOptionTypes.FromImage,
                  Vhd = new VirtualHardDisk
                  {
                    Uri = "http://" + storageName + ".blob.core.windows.net/vhds/mytestod1.vhd"
                  }
                }
              }
            }
          );
        }

    >[AZURE.NOTE] Tento kurz vytvoří virtuální počítač s verzí operačního systému Windows Server. Další informace o výběru dalších obrázků najdete v tématu [Navigace a obrázky vyberte Azure virtuální počítač s Windows Powershellu a rozhraní příkazového řádku Azure](virtual-machines-linux-cli-ps-findimage.md).

2. Zavolejte si metodu, která jste dříve přidali, přidejte tento kód metody hlavní:

        var vmResult = CreateVirtualMachineAsync(
          credential,
          groupName,
          subscriptionId,
          location,
          nicName,
          avSetName,
          storageName,
          adminName,
          adminPassword,
          vmName);
        Console.WriteLine(vmResult.Result.ProvisioningState);
        Console.ReadLine();

##<a name="step-4-delete-the-resources"></a>Krok 4: Odstranění zdroje

Protože vám bude účtovaná za zdrojů použitých v Azure, je vždy vhodné odstranit prostředky, které už nepotřebujete. Pokud chcete odstranit virtuálních počítačích a podpůrné materiály, které je potřeba udělat stačí odstranit skupina zdroje.

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

2.  Zavolejte si metodu, která jste dříve přidali, přidejte tento kód metody hlavní:

        DeleteResourceGroupAsync(
          credential,
          groupName,
          subscriptionId);
        Console.ReadLine();

## <a name="step-5-run-the-console-application"></a>Krok 5: Spusťte aplikaci konzoly

1. Pokud chcete spustit aplikaci konzoly, klikněte na tlačítko **Start** ve Visual Studiu a pak se přihlaste k Azure AD pomocí stejné uživatelské jméno a heslo, které používáte k vašemu předplatnému.

2. Po vytvoření jednotlivé zdroje je vrácena každý stavový kód, stiskněte klávesu **Enter** . Po vytvoření virtuální počítač proveďte další krok před stisknutím klávesy Enter a vymažete všechny zdroje.

    By měl trvá asi pět minut pro tuto aplikaci konzoly spuštění úplně od začátku do konce. Před stisknutím klávesy Enter spusťte odstraňování zdrojů, může trvat několik minut ověřit vytvoření zdrojů v portálu Azure před odstraněním.

3. Zobrazení stavu zdrojů, přejděte v protokolech auditování Azure portálu:

    ![Procházet protokolů auditování Azure portálu](./media/virtual-machines-windows-csharp/crpportal.png)
    
## <a name="next-steps"></a>Další kroky

- Výhodou používání šablony k vytvoření virtuálního počítače pomocí informací v [nasazení virtuálního počítače Azure pomocí C# a šablony správce prostředků](virtual-machines-windows-csharp-template.md).
- Naučte se spravovat virtuální počítač, který jste vytvořili kontrolou [Spravovat virtuálních počítačích pomocí Správce prostředků Azure a Powershellu](virtual-machines-windows-csharp-manage.md).
