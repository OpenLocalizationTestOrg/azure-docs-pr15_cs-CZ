<properties 
    pageTitle="Nasazení a Správa oznámení rozbočovače pomocí prostředí PowerShell" 
    description="Jak vytvořit a spravovat rozbočovače oznámení pomocí Powershellu pro automatizaci" 
    services="notification-hubs" 
    documentationCenter="" 
    authors="ysxu" 
    manager="erikre" 
    editor="" />

<tags 
    ms.service="notification-hubs" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="powershell" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="06/29/2016" 
    ms.author="yuaxu"/>

# <a name="deploy-and-manage-notification-hubs-using-powershell"></a>Nasazení a Správa oznámení rozbočovače pomocí prostředí PowerShell

##<a name="overview"></a>Základní informace

V tomto článku se dozvíte, jak můžete vytvořit a spravovat Azure oznámení rozbočovače pomocí Powershellu. Následující běžné úlohy automatizaci jsou zobrazeny v tomto tématu.

+ Vytvoření rozbočovači oznámení
+ Nastavení přihlašovacích údajů

Pokud chcete vytvořit nové obor názvů bus služby pro oznámení rozbočovače, najdete v článku [Správa služby Bus pomocí prostředí PowerShell](../service-bus-messaging/service-bus-powershell-how-to-provision.md).

Správa oznámení rozbočovače nepodporuje přímo rutiny zahrnutý v sadě Azure Powershellu. Nejlepší přístup z prostředí PowerShell je neodkazuje sestavení Microsoft.Azure.NotificationHubs.dll. Sestavení je distribuovaná pomocí [Microsoft Azure oznámení rozbočovače NuGet balíčku](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/).


## <a name="prerequisites"></a>Zjistit předpoklady pro

Než začnete v tomto článku, musíte mít takto:

- Předplatné Azure. Azure je platforma na základě předplatného. Další informace o získání předplatné najdete v článku [Možnosti nákupu], [Nabízí člena]nebo [Bezplatnou zkušební verzi].

- Počítač s Azure Powershellu. Pokyny najdete v tématu [instalace a konfigurace prostředí PowerShell Azure].

- Obecné porozumění skriptů Powershellu, NuGet balíčků a .NET Framework.


## <a name="including-a-reference-to-the-net-assembly-for-service-bus"></a>Odkaz na sestavení .NET pro službu Bus včetně

Správa oznámení rozbočovače Azure ještě není součástí rutinách Powershellu v Azure Powershellu. Zřízení rozbočovače oznámení, můžete .NET klienta uvedených v [Microsoft Azure oznámení rozbočovače NuGet balíčku](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/).

Nejdřív zkontrolujte, že skript můžete vyhledat sestavení **Microsoft.Azure.NotificationHubs.dll** , která je nainstalovaná jako balíček NuGet ve Visual Studiu projektu. Pokud chcete být flexibilní, provede skript takto:

1. Určuje cestu niž byl spuštěn.
2. Projde cestu, dokud nenajde do složky nazvané `packages`. Vytvoření složky při instalaci NuGet balíčků pro projekty Visual Studio.
3. Hledání zpětně `packages` složku pro sestavení s názvem **Microsoft.Azure.NotificationHubs.dll**.
4. Odkazuje na sestavení tak, aby typy jsou dostupné pro pozdější použití.

Tady je implementaci tyto kroky v skript Powershellu:

``` powershell

try
{
    # WARNING: Make sure to reference the latest version of Microsoft.Azure.NotificationHubs.dll
    Write-Output "Adding the [Microsoft.Azure.NotificationHubs.dll] assembly to the script..."
    $scriptPath = Split-Path (Get-Variable MyInvocation -Scope 0).Value.MyCommand.Path
    $packagesFolder = (Split-Path $scriptPath -Parent) + "\packages"
    $assembly = Get-ChildItem $packagesFolder -Include "Microsoft.Azure.NotificationHubs.dll" -Recurse
    Add-Type -Path $assembly.FullName

    Write-Output "The [Microsoft.Azure.NotificationHubs.dll] assembly has been successfully added to the script."
}

catch [System.Exception]
{
    Write-Error("Could not add the Microsoft.Azure.NotificationHubs.dll assembly to the script. Make sure you build the solution before running the provisioning script.")
}
```

## <a name="create-the-namespacemanager-class"></a>Vytvoření NamespaceManager třídy

Zřízení rozbočovače oznámení, vytvořte instance třídy [NamespaceManager](https://msdn.microsoft.com/library/azure/microsoft.azure.notificationhubs.namespacemanager.aspx) ze sady SDK. 

Můžete použít rutinu [Get-AzureSBAuthorizationRule] zahrnutý v sadě Azure PowerShell k načtení ověřovací pravidlo, které slouží k poskytování připojovací řetězec. Jsme ukládat odkaz `NamespaceManager` instance v `$NamespaceManager` proměnné. Použijeme `$NamespaceManager` zřízení rozbočovači oznámení.

``` powershell
$sbr = Get-AzureSBAuthorizationRule -Namespace $Namespace
# Create the NamespaceManager object to create the hub
Write-Output "Creating a NamespaceManager object for the [$Namespace] namespace..."
$NamespaceManager=[Microsoft.Azure.NotificationHubs.NamespaceManager]::CreateFromConnectionString($sbr.ConnectionString);
Write-Output "NamespaceManager object for the [$Namespace] namespace has been successfully created."
```


## <a name="provisioning-a-new-notification-hub"></a>Zřízení nového centrální oznámení 

Zřízení nového centrální oznámení pomocí rozhraní [API .NET pro rozbočovače oznámení].

V této části skript nastavíte čtyři lokální proměnné. 

1. `$Namespace`: Nastavte název oboru místo, kam chcete vytvořit rozbočovači oznámení.
2. `$Path`: Nastavte tato cesta název nové rozbočovači oznámení.  Například "MyHub".    
3. `$WnsPackageSid`: Nastavte balíček ID zabezpečení pro vás aplikace pro Windows z [Centrum pro vývojáře Windows](http://go.microsoft.com/fwlink/p/?linkid=266582&clcid=0x409).
4. `$WnsSecretkey`: Nastavte tajné klíč pro vás aplikace pro Windows z [Centrum pro vývojáře Windows](http://go.microsoft.com/fwlink/p/?linkid=266582&clcid=0x409).

Tyto proměnné se používají pro připojení k oboru názvů a vytvořit nové centrum oznámení nakonfigurované tak, aby zpracovávat oznámení služby systému Windows (WNS) oznámení pomocí přihlašovacích údajů WNS aplikace Windows. Informace o získání balíček ID zabezpečení a tajné klíč zobrazíte kurz [Začínáte pracovat s rozbočovače oznámení](notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md) . 

+ Používá fragment skriptu `NamespaceManager` objekt, který chcete zkontrolovat, pokud centru oznámení označenými `$Path` existuje.

+ Pokud není k dispozici, skript vytvoří `NotificationHubDescription` s WNS pověření a předejte, který chcete `NamespaceManager` třídy `CreateNotificationHub` metody.

``` powershell

$Namespace = "<Enter your namespace>"
$Path  = "<Enter a name for your notification hub>"
$WnsPackageSid = "<your package sid>"
$WnsSecretkey = "<enter your secret key>"

$WnsCredential = New-Object -TypeName Microsoft.Azure.NotificationHubs.WnsCredential -ArgumentList $WnsPackageSid,$WnsSecretkey

# Query the namespace
$CurrentNamespace = Get-AzureSBNamespace -Name $Namespace

# Check if the namespace already exists
if ($CurrentNamespace)
{
    Write-Output "The namespace [$Namespace] in the [$($CurrentNamespace.Region)] region was found."

    # Create the NamespaceManager object used to create a new notification hub
    $sbr = Get-AzureSBAuthorizationRule -Namespace $Namespace
    Write-Output "Creating a NamespaceManager object for the [$Namespace] namespace..."
    $NamespaceManager = [Microsoft.Azure.NotificationHubs.NamespaceManager]::CreateFromConnectionString($sbr.ConnectionString);
    Write-Output "NamespaceManager object for the [$Namespace] namespace has been successfully created."

    # Check to see if the Notification Hub already exists
    if ($NamespaceManager.NotificationHubExists($Path))
    {
        Write-Output "The [$Path] notification hub already exists in the [$Namespace] namespace."  
    }
    else
    {
        Write-Output "Creating the [$Path] notification hub in the [$Namespace] namespace."
        $NHDescription = New-Object -TypeName Microsoft.Azure.NotificationHubs.NotificationHubDescription -ArgumentList $Path;
        $NHDescription.WnsCredential = $WnsCredential;
        $NamespaceManager.CreateNotificationHub($NHDescription);
        Write-Output "The [$Path] notification hub was created in the [$Namespace] namespace."
    }
}
else
{
    Write-Host "The [$Namespace] namespace does not exist."
}
```




## <a name="additional-resources"></a>Další zdroje informací

- [Správa služby Bus pomocí prostředí PowerShell](../service-bus-messaging/service-bus-powershell-how-to-provision.md)
- [Jak vytvořit služby Bus fronty, témata a předplatných pomocí skriptu prostředí PowerShell](http://blogs.msdn.com/b/paolos/archive/2014/12/02/how-to-create-a-service-bus-queues-topics-and-subscriptions-using-a-powershell-script.aspx)
- [Jak vytvořit Namespace Bus služby a rozbočovači událostí pomocí skriptu prostředí PowerShell](http://blogs.msdn.com/b/paolos/archive/2014/12/01/how-to-create-a-service-bus-namespace-and-an-event-hub-using-a-powershell-script.aspx)

Některé připravených skripty je také k dispozici ke stažení:
- [Skriptů Powershellu služby Bus](https://code.msdn.microsoft.com/windowsazure/Service-Bus-PowerShell-a46b7059)
 

[Možnosti nákupu]: http://azure.microsoft.com/pricing/purchase-options/
[Člen nabídky]: http://azure.microsoft.com/pricing/member-offers/
[Bezplatná zkušební verze]: http://azure.microsoft.com/pricing/free-trial/
[Instalace a konfigurace prostředí PowerShell Azure]: ../powershell-install-configure.md
[Rozhraní API .NET pro rozbočovače oznámení]: https://msdn.microsoft.com/library/azure/mt414893.aspx
[Get-AzureSBNamespace]: https://msdn.microsoft.com/library/azure/dn495122.aspx
[New-AzureSBNamespace]: https://msdn.microsoft.com/library/azure/dn495165.aspx
[Get-AzureSBAuthorizationRule]: https://msdn.microsoft.com/library/azure/dn495113.aspx
 
