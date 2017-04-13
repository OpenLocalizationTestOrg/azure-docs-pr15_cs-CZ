<properties
    pageTitle="Správa VMs pomocí Správce prostředků Azure a C# | Microsoft Azure"
    description="Správa virtuálních počítačích pomocí Správce prostředků Azure a C#."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="davidmu1"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="na"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/27/2016"
    ms.author="davidmu"/>

# <a name="manage-azure-virtual-machines-using-azure-resource-manager-and-c"></a>Správa virtuálních počítačích Azure pomocí Správce prostředků Azure a C#  

Úkoly v tomto článku se dozvíte, jak spravovat virtuálních počítačích, například počáteční krátkou zprávu a aktualizace. Virtuální počítač musí existovat v skupiny zdrojů a udělali úkoly podle pokynů v tomto článku.

Aby udělali úkoly podle pokynů v tomto článku, musíte:

- [Visual Studio](http://msdn.microsoft.com/library/dd831853.aspx)
- [Ověřovací token](../resource-group-authenticate-service-principal.md)

## <a name="create-a-visual-studio-project-and-install-packages"></a>Vytvoření projektu Visual Studiu a nainstalujte balíčky

Balíčky NuGet způsoby nejjednodušší nainstalovat knihoven, které potřebujete k dokončení úkolů v tomto článku. Knihoven, které instalace pro tento článek se Azure Active Directory Authentication Library knihovnu výpočet poskytovatele zdroje. Udělejte Tyhle kroky získat knihoven ve Visual Studiu:

1. Klikněte na **soubor** > **nové** > **projektu**.

2. V **šablonách** > **Visual Basic**, zvolte **Aplikace konzoly**, zadejte název a umístění projektu a potom klikněte na **OK**.

3. Klikněte pravým tlačítkem myši na název projektu v Průzkumníku řešení a potom klikněte na **Spravovat balíčků NuGet**.

4. Typ *Služby Active Directory* do vyhledávacího pole, klikněte na tlačítko **instalovat** Active Directory Authentication Library balíčku a postupujte podle pokynů k instalaci balíčku.

5. V horní části stránky vyberte **Zahrnout zkušební**. Typ *Microsoft.Azure.Management.Compute* do vyhledávacího pole pro výpočet .NET knihovny klikněte na tlačítko **instalovat** a pak postupujte podle pokynů nainstalujte balíček.

Teď jste připraveni začít používat knihoven ke správě virtuálních počítačích.

## <a name="set-up-the-project"></a>Nastavení projektu

Vytvoření aplikace a knihoven nainstalovaných, vytvoříte token pomocí informace o aplikaci. Tento token slouží k ověření požadavků pro správce prostředků Azure.

1. Otevřete soubor Program.cs pro projekt, který jste vytvořili a přidejte tyto pomocí příkazů do horní části souboru:

        using Microsoft.Azure;
        using Microsoft.IdentityModel.Clients.ActiveDirectory;
        using Microsoft.Azure.Management.Compute;
        using Microsoft.Azure.Management.Compute.Models;
        using Microsoft.Rest;
        
2. Přidání proměnné metody hlavní třídy Program k určení názvu skupiny zdrojů a název virtuálního počítače a identifikátor URI vašeho předplatného:

        var groupName = "resource group name";
        var vmName = "virtual machine name";  
        var subscriptionId = "subsciption id";

    Identifikátor předplatného můžete najít spuštěním Get-AzureRmSubscription.
    
3. Chcete-li získat token, který je potřeba k vytvoření přihlašovací údaje, přidejte tento způsob třídy aplikace:

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
    
4. Pokud chcete vytvořit její přihlašovací údaje, přidejte tento kód metody hlavní v Program.cs:

        var token = GetAccessTokenAsync();
        var credential = new TokenCredentials(token.Result.AccessToken);

5. Uložte soubor Program.cs.

## <a name="display-information-about-a-virtual-machine"></a>Zobrazení informací o virtuálního počítače

1. Přidejte tuto metodu třídy Program v projektu, který jste vytvořili:

        public static async void GetVirtualMachineAsync(
          TokenCredentials credential, 
          string groupName, 
          string vmName, 
          string subscriptionId)
        {
          Console.WriteLine("Getting information about the virtual machine...");

          var computeManagementClient = new ComputeManagementClient(credential)
            { SubscriptionId = subscriptionId };
          var vmResult = await computeManagementClient.VirtualMachines.GetAsync(
            groupName, 
            vmName, 
            InstanceViewTypes.InstanceView);

          Console.WriteLine("hardwareProfile");
          Console.WriteLine("   vmSize: " + vmResult.HardwareProfile.VmSize);

          Console.WriteLine("\nstorageProfile");
          Console.WriteLine("  imageReference");
          Console.WriteLine("    publisher: " + vmResult.StorageProfile.ImageReference.Publisher);
          Console.WriteLine("    offer: " + vmResult.StorageProfile.ImageReference.Offer);
          Console.WriteLine("    sku: " + vmResult.StorageProfile.ImageReference.Sku);
          Console.WriteLine("    version: " + vmResult.StorageProfile.ImageReference.Version);
          Console.WriteLine("  osDisk");
          Console.WriteLine("    osType: " + vmResult.StorageProfile.OsDisk.OsType);
          Console.WriteLine("    name: " + vmResult.StorageProfile.OsDisk.Name);
          Console.WriteLine("    createOption: " + vmResult.StorageProfile.OsDisk.CreateOption);
          Console.WriteLine("    uri: " + vmResult.StorageProfile.OsDisk.Vhd.Uri);
          Console.WriteLine("    caching: " + vmResult.StorageProfile.OsDisk.Caching);

          Console.WriteLine("\nosProfile");
          Console.WriteLine("  computerName: " + vmResult.OsProfile.ComputerName);
          Console.WriteLine("  adminUsername: " + vmResult.OsProfile.AdminUsername);
          Console.WriteLine("  provisionVMAgent: " + vmResult.OsProfile.WindowsConfiguration.ProvisionVMAgent.Value);
          Console.WriteLine("  enableAutomaticUpdates: " + vmResult.OsProfile.WindowsConfiguration.EnableAutomaticUpdates.Value);

          Console.WriteLine("\nnetworkProfile");
          foreach (NetworkInterfaceReference nic in vmResult.NetworkProfile.NetworkInterfaces)
          {
            Console.WriteLine("  networkInterface id: " + nic.Id);
          }

          Console.WriteLine("\nvmAgent");
          Console.WriteLine("  vmAgentVersion" + vmResult.InstanceView.VmAgent.VmAgentVersion);
          Console.WriteLine("    statuses");
          foreach (InstanceViewStatus stat in vmResult.InstanceView.VmAgent.Statuses)
          {
            Console.WriteLine("    code: " + stat.Code);
            Console.WriteLine("    level: " + stat.Level);
            Console.WriteLine("    displayStatus: " + stat.DisplayStatus);
            Console.WriteLine("    message: " + stat.Message);
            Console.WriteLine("    time: " + stat.Time);
          }

          Console.WriteLine("\ndisks");
          foreach (DiskInstanceView idisk in vmResult.InstanceView.Disks)
          {
            Console.WriteLine("  name: " + idisk.Name);
            Console.WriteLine("  statuses");
            foreach (InstanceViewStatus istat in idisk.Statuses)
            {
              Console.WriteLine("    code: " + istat.Code);
              Console.WriteLine("    level: " + istat.Level);
              Console.WriteLine("    displayStatus: " + istat.DisplayStatus);
              Console.WriteLine("    time: " + istat.Time);
            }
          }

          Console.WriteLine("\nVM general status");
          Console.WriteLine("  provisioningStatus: " + vmResult.ProvisioningState);
          Console.WriteLine("  id: " + vmResult.Id);
          Console.WriteLine("  name: " + vmResult.Name);
          Console.WriteLine("  type: " + vmResult.Type);
          Console.WriteLine("  location: " + vmResult.Location);
          Console.WriteLine("\nVM instance status");
          foreach (InstanceViewStatus istat in vmResult.InstanceView.Statuses)
          {
            Console.WriteLine("\n  code: " + istat.Code);
            Console.WriteLine("  level: " + istat.Level);
            Console.WriteLine("  displayStatus: " + istat.DisplayStatus);
          }
          
        }

2. Volání metody, kterou jste právě přidali, přidejte tento kód metody hlavní:

        GetVirtualMachineAsync(
          credential,
          groupName,
          vmName,
          subscriptionId);
        Console.WriteLine("\nPress enter to continue...");
        Console.ReadLine();
    
3. Uložte soubor Program.cs.

4. Klikněte na tlačítko **Start** ve Visual Studiu a pak se přihlaste k Azure AD pomocí stejné uživatelské jméno a heslo, které používáte k vašemu předplatnému.

    Když spustíte tento způsob, byste měli vidět vypadá podobně jako tento příklad:
    
        Getting information about the virtual machine...
        hardwareProfile
          vmSize: Standard_A0

        storageProfile
          imageReference
            publisher: MicrosoftWindowsServer
            offer: WindowsServer
            sku: 2012-R2-Datacenter
            version: latest
          osDisk
            osType: Windows
            name: myosdisk
            createOption: FromImage
            uri: http://store1.blob.core.windows.net/vhds/myosdisk.vhd
            caching: ReadWrite

          osProfile
            computerName: vm1
            adminUsername: account1
            provisionVMAgent: True
            enableAutomaticUpdates: True

          networkProfile
            networkInterface 
              id: /subscriptions/{subscription-id}
                /resourceGroups/rg1/providers/Microsoft.Network/networkInterfaces/nc1

          vmAgent
            vmAgentVersion2.7.1198.766
            statuses
            code: ProvisioningState/succeeded
            level: Info
            displayStatus: Ready
            message: GuestAgent is running and accepting new configurations.
            time: 4/13/2016 8:35:32 PM

          disks
            name: myosdisk
            statuses
              code: ProvisioningState/succeeded
              level: Info
              displayStatus: Provisioning succeeded
              time: 4/13/2016 8:04:36 PM

          VM general status
            provisioningStatus: Succeeded
            id: /subscriptions/{subscription-id}
              /resourceGroups/rg1/providers/Microsoft.Compute/virtualMachines/vm1
            name: vm1
            type: Microsoft.Compute/virtualMachines
            location: centralus

          VM instance status
            code: ProvisioningState/succeeded
              level: Info
              displayStatus: Provisioning succeeded
            code: PowerState/running
              level: Info
              displayStatus: VM running

## <a name="stop-a-virtual-machine"></a>Ukončení virtuálního počítače

Můžete zastavit virtuálního počítače dvěma způsoby. Můžete zastavit virtuálního počítače a zachovat jeho nastavení, ale nadále platit pro něj nebo zastavení virtuálního počítače a ji zrušit. Uvolnit virtuálního počítače všechny prostředky přidružená nejsou také zrušeny, takže a fakturační období jej.

1. Poznámky jakýkoli kód, že jste dříve přidali k metodu hlavní kromě kódu a získání pověření.

2. Přidejte tuto metodu třídy aplikace:

        public static async void StopVirtualMachineAsync(
          TokenCredentials credential, 
          string groupName, 
          string vmName, 
          string subscriptionId)
        {
          Console.WriteLine("Stopping the virtual machine...");
          var computeManagementClient = new ComputeManagementClient(credential)
            { SubscriptionId = subscriptionId };
          await computeManagementClient.VirtualMachines.PowerOffAsync(groupName, vmName);
        }

    Pokud chcete zrušit virtuální počítač, změňte vypnutí volání tento kód:

        computeManagementClient.VirtualMachines.Deallocate(groupName, vmName);

3. Volání metody, kterou jste právě přidali, přidejte tento kód metody hlavní:

        StopVirtualMachineAsync(
          credential,
          groupName,
          vmName,
          subscriptionId);
        Console.WriteLine("\nPress enter to continue...");
        Console.ReadLine();

4. Uložte soubor Program.cs.

5. Klikněte na tlačítko **Start** ve Visual Studiu a pak se přihlaste k Azure AD pomocí stejné uživatelské jméno a heslo, které používáte k vašemu předplatnému.

    Měli byste vidět stav změnit virtuálního počítače do ukončení uživatelem. Pokud jste spustili požadovanou metodu ukončit volání Deallocate stavu (uvolnit).

## <a name="start-a-virtual-machine"></a>Spuštění virtuálního počítače

1. Poznámky jakýkoli kód, že jste dříve přidali k metodu hlavní kromě kódu a získání pověření.

2. Přidejte tuto metodu třídy aplikace:

        public static async void StartVirtualMachineAsync(
          TokenCredentials credential, 
          string groupName, 
          string vmName, 
          string subscriptionId)
        {
          Console.WriteLine("Starting the virtual machine...");
          var computeManagementClient = new ComputeManagementClient(credential)
            { SubscriptionId = subscriptionId };
          await computeManagementClient.VirtualMachines.StartAsync(groupName, vmName);
        }

3. Volání metody, kterou jste právě přidali, přidejte tento kód metody hlavní:

        StartVirtualMachineAsync(
          credential,
          groupName,
          vmName,
          subscriptionId);
        Console.WriteLine("\nPress enter to continue...");
        Console.ReadLine();

4. Uložte soubor Program.cs.

5. Klikněte na tlačítko **Start** ve Visual Studiu a pak se přihlaste k Azure AD pomocí stejné uživatelské jméno a heslo, které používáte k vašemu předplatnému.

    Měli byste vidět stav virtuálního počítače změnit na spuštěno.

## <a name="restart-a-running-virtual-machine"></a>Restartujte pracovního virtuálního počítače

1. Poznámky jakýkoli kód, že jste dříve přidali k metodu hlavní kromě kódu a získání pověření.

2. Přidejte tuto metodu třídy aplikace:

        public static async void RestartVirtualMachineAsync(
          TokenCredentials credential,
          string groupName,
          string vmName,
          string subscriptionId)
        {
          Console.WriteLine("Restarting the virtual machine...");
          var computeManagementClient = new ComputeManagementClient(credential)
            { SubscriptionId = subscriptionId };
          await computeManagementClient.VirtualMachines.RestartAsync(groupName, vmName);
        }

3. Volání metody, kterou jste právě přidali, přidejte tento kód metody hlavní:

        RestartVirtualMachineAsync(
          credential,
          groupName,
          vmName,
          subscriptionId);
        Console.WriteLine("\nPress enter to continue...");
        Console.ReadLine();

4. Uložte soubor Program.cs.

5. Klikněte na tlačítko **Start** ve Visual Studiu a pak se přihlaste k Azure AD pomocí stejné uživatelské jméno a heslo, které používáte k vašemu předplatnému.

## <a name="resize-a-virtual-machine"></a>Změna velikosti virtuálního počítače

Tento příklad ukazuje, jak změnit velikost pracovního virtuálního počítače.

1. Poznámky jakýkoli kód, že jste dříve přidali k metodu hlavní kromě kódu a získání pověření.

2. Přidejte tuto metodu třídy aplikace:

        public static async void UpdateVirtualMachineAsync(
          TokenCredentials credential, 
          string groupName, 
          string vmName, 
          string subscriptionId)
        {
          Console.WriteLine("Updating the virtual machine...");
          var computeManagementClient = new ComputeManagementClient(credential)
            { SubscriptionId = subscriptionId };
          var vmResult = await computeManagementClient.VirtualMachines.GetAsync(groupName, vmName);
          vmResult.HardwareProfile.VmSize = "Standard_A1";
          await computeManagementClient.VirtualMachines.CreateOrUpdateAsync(groupName, vmName, vmResult);
        }

3. Volání metody, kterou jste právě přidali, přidejte tento kód metody hlavní:

        UpdateVirtualMachineAsync(
          credential,
          groupName,
          vmName,
          subscriptionId);
        Console.WriteLine("\nPress enter to continue...");
        Console.ReadLine();

4. Uložte soubor Program.cs.

5. Klikněte na tlačítko **Start** ve Visual Studiu a pak se přihlaste k Azure AD pomocí stejné uživatelské jméno a heslo, které používáte k vašemu předplatnému.

    Měli byste vidět velikost do Standard_A1 změnit virtuálního počítače.

## <a name="add-a-data-disk-to-a-virtual-machine"></a>Přidání dat disku do virtuálního počítače

Tento příklad ukazuje, jak přidat disk dat do spuštěného virtuálního počítače.

1. Poznámky jakýkoli kód, že jste dříve přidali k metodu hlavní kromě kódu a získání pověření.

2. Přidejte tuto metodu třídy aplikace:

        public static async void AddDataDiskAsync(
          TokenCredentials credential, 
          string groupName, 
          string vmName, 
          string subscriptionId)
        {
          Console.WriteLine("Adding the disk to the virtual machine...");
          var computeManagementClient = new ComputeManagementClient(credential)
            { SubscriptionId = subscriptionId };
          var vmResult = await computeManagementClient.VirtualMachines.GetAsync(groupName, vmName);
          vmResult.StorageProfile.DataDisks.Add(
            new DataDisk
              {
                Lun = 0,
                Name = "mydatadisk1",
                Vhd = new VirtualHardDisk
                  {
                    Uri = "https://mystorage1.blob.core.windows.net/vhds/mydatadisk1.vhd"
                  },
                CreateOption = DiskCreateOptionTypes.Empty,
                DiskSizeGB = 2,
                Caching = CachingTypes.ReadWrite
              });
          await computeManagementClient.VirtualMachines.CreateOrUpdateAsync(groupName, vmName, vmResult);
        }

3. Volání metody, kterou jste právě přidali, přidejte tento kód metody hlavní:

        AddDataDiskAsync(
          credential,
          groupName,
          vmName,
          subscriptionId);
        Console.WriteLine("\nPress enter to continue...");
        Console.ReadLine();

4. Uložte soubor Program.cs.

5. Klikněte na tlačítko **Start** ve Visual Studiu a pak se přihlaste k Azure AD pomocí stejné uživatelské jméno a heslo, které používáte k vašemu předplatnému.

## <a name="delete-a-virtual-machine"></a>Odstranění virtuálního počítače

1. Poznámky jakýkoli kód, že jste dříve přidali k metodu hlavní kromě kódu a získání pověření.

2. Přidejte tuto metodu třídy aplikace:

        public static async void DeleteVirtualMachineAsync(
          TokenCredentials credential, 
          string groupName, 
          string vmName, 
          string subscriptionId)
        {
          Console.WriteLine("Deleting the virtual machine...");
          var computeManagementClient = new ComputeManagementClient(credential)
            { SubscriptionId = subscriptionId };
          await computeManagementClient.VirtualMachines.DeleteAsync(groupName, vmName);
        }

3. Volání metody, kterou jste právě přidali, přidejte tento kód metody hlavní:

        DeleteVirtualMachineAsync(
          credential,
          groupName,
          vmName,
          subscriptionId);
        Console.WriteLine("\nPress enter to continue...");
        Console.ReadLine();

4. Uložte soubor Program.cs.

5. Klikněte na tlačítko **Start** ve Visual Studiu a pak se přihlaste k Azure AD pomocí stejné uživatelské jméno a heslo, které používáte k vašemu předplatnému.

## <a name="next-steps"></a>Další kroky

Pokud nejsou problémy s nasazení, může vypadat při [nasazení skupina zdroje Poradce při potížích s Azure portálu](../resource-manager-troubleshoot-deployments-portal.md)
