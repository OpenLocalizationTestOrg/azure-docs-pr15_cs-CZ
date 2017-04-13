<properties
    pageTitle="Správa služby Bus pomocí prostředí PowerShell | Microsoft Azure"
    description="Správa služby Bus pomocí skriptů Powershellu"
    services="service-bus"
    documentationCenter=".net"
    authors="sethmanheim"
    manager="timlt"
    editor=""/>

<tags
    ms.service="service-bus"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/03/2016"
    ms.author="sethm"/>

# <a name="manage-service-bus-with-powershell"></a>Správa služby Bus pomocí prostředí PowerShell

## <a name="overview"></a>Základní informace

Microsoft Azure PowerShell je skriptu prostředí, které můžete použít k ovládání a automatizovat nasazení a řízení úloh v Azure. Tento článek popisuje, jak pomocí Powershellu vytvořit a spravovat službu Bus entity například obory názvů, fronty a událostí rozbočovače pomocí místního prostředí PowerShell Azure konzoly.

## <a name="prerequisites"></a>Zjistit předpoklady pro

Než začnete v tomto článku, musíte mít následující požadavky:

- Předplatné Azure. Azure je platforma na základě předplatného. Další informace o získání předplatné najdete v článku [Možnosti nákupu][], [Nabízí člena][]nebo [Bezplatnou zkušební verzi][].

- Počítač s Azure Powershellu. Pokyny najdete v tématu [instalace a konfigurace prostředí PowerShell Azure][].

- Obecné porozumění skriptů Powershellu, NuGet balíčků a .NET Framework.

## <a name="including-a-reference-to-the-net-assembly-for-service-bus"></a>Odkaz na sestavení .NET pro službu Bus včetně

Nejsou k dispozici pro správu služby Bus omezený počet rutiny prostředí PowerShell. Zřízení entity, které nejsou k dispozici prostřednictvím existující rutin, můžete klientovi .NET pro službu Bus [balíčku služby Bus NuGet][].

Nejdřív zkontrolujte, zda skript můžete vyhledat sestavení **Microsoft.ServiceBus.dll** , který se instaluje s balíček NuGet. Pokud chcete být flexibilní, provede skript takto:

1. Určuje cestu niž byl spuštěn.
2. Projde cestu, dokud nenajde do složky nazvané `packages`. Vytvoření složky při instalaci NuGet balíčků.
3. Hledání zpětně `packages` složku pro sestavení s názvem **Microsoft.ServiceBus.dll**.
4. Odkazuje na sestavení tak, aby typy jsou dostupné pro pozdější použití.

Tady je implementaci tyto kroky v skript Powershellu:

```
try
{
    # WARNING: Make sure to reference the latest version of Microsoft.ServiceBus.dll
    Write-Output "Adding the [Microsoft.ServiceBus.dll] assembly to the script..."
    $scriptPath = Split-Path (Get-Variable MyInvocation -Scope 0).Value.MyCommand.Path
    $packagesFolder = (Split-Path $scriptPath -Parent) + "\packages"
    $assembly = Get-ChildItem $packagesFolder -Include "Microsoft.ServiceBus.dll" -Recurse
    Add-Type -Path $assembly.FullName

    Write-Output "The [Microsoft.ServiceBus.dll] assembly has been successfully added to the script."
}

catch [System.Exception]
{
    Write-Error "Could not add the Microsoft.ServiceBus.dll assembly to the script. Make sure you build the solution before running the provisioning script."
}
```

## <a name="provision-a-service-bus-namespace"></a>Poskytování služby Bus obor názvů

Dva rutiny prostředí PowerShell podporují službu Bus názvů operace. Místo rozhraní API SDK .NET můžete [Získat AzureSBNamespace][] a [Nový AzureSBNamespace][].

Tento příklad vytvoří několik lokální proměnné v skript; `$Namespace` and `$Location`.

- `$Namespace`stejný název služby Bus názvů, se kterým chcete pracovat.
- `$Location`Určuje datovém centru, ve kterém skript ustanovení obor názvů.
- `$CurrentNamespace`ukládá obor názvů odkaz, který skript načte (nebo vytvoří).

Ve skriptu skutečné `$Namespace` a `$Location` můžete předané jako parametry.

Tato část skript provede tyto věci:

1. Pokusí se získat služby Bus obor názvů s zadané jméno.
2. Pokud oboru nachází, sestavy, data nalezená.
3. Pokud není nalezen obor názvů, vytvoří obor názvů a jeho obnovení nově vytvořený názvů.

    ```
    $Namespace = "MyServiceBusNS"
    $Location = "West US"
    
    # Query to see if the namespace currently exists
    $CurrentNamespace = Get-AzureSBNamespace -Name $Namespace
    
    # Check if the namespace already exists or needs to be created
    if ($CurrentNamespace)
    {
        Write-Output "The namespace [$Namespace] already exists in the [$($CurrentNamespace.Region)] region."
    }
    else
    {
        Write-Host "The [$Namespace] namespace does not exist."
        Write-Output "Creating the [$Namespace] namespace in the [$Location] region..."
        New-AzureSBNamespace -Name $Namespace -Location $Location -CreateACSNamespace $false -NamespaceType Messaging
        $CurrentNamespace = Get-AzureSBNamespace -Name $Namespace
        Write-Host "The [$Namespace] namespace in the [$Location] region has been successfully created."
    }
    ```

Zřízení jiné služby Bus entity, vytvořte instance třídy [NamespaceManager][] ze sady SDK.
Můžete použít rutinu [Get-AzureSBAuthorizationRule][] k načtení ověřovací pravidlo, které slouží k poskytování připojovací řetězec. Jsme ukládat odkaz `NamespaceManager` instance v `$NamespaceManager` proměnné. Použijeme `$NamespaceManager` dál v skript zřízení jiné entity.

``` powershell
$sbr = Get-AzureSBAuthorizationRule -Namespace $Namespace
# Create the NamespaceManager object to create the event hub
Write-Output "Creating a NamespaceManager object for the [$Namespace] namespace..."
$NamespaceManager = [Microsoft.ServiceBus.NamespaceManager]::CreateFromConnectionString($sbr.ConnectionString);
Write-Output "NamespaceManager object for the [$Namespace] namespace has been successfully created."
```

## <a name="provisioning-other-service-bus-entities"></a>Zřízení jiné služby Bus entity

Aby bylo možné vytvořit další entity, například fronty, témata a událostí rozbočovače pomocí rozhraní [API .NET pro službu Bus][]. Tento článek se zaměřuje pouze na rozbočovače událost, ale kroky pro další entity jsou podobné. Kromě toho podrobnější příklady, včetně dalších entity odkazují na konci tohoto článku.

Tato část skript vytvoří čtyři další lokální proměnné. Tyto proměnné se používají pro vytvoření instance `EventHubDescription` objektu. Skript provede tyto věci:

1. Použití `NamespaceManager` objekt, zkontrolujte, pokud rozbočovači události označenými `$Path` existuje.
2. Pokud není k dispozici, vytvořte `EventHubDescription` a předejte, který chcete `NamespaceManager` třídy `CreateEventHubIfNotExists` metody.
3. Po zjištění, neexistuje centrální události, vytvořte pomocí skupiny spotř `ConsumerGroupDescription` a `NamespaceManager`.

    ```
    $Path  = "MyEventHub"
    $PartitionCount = 12
    $MessageRetentionInDays = 7
    $UserMetadata = $null
    $ConsumerGroupName = "MyConsumerGroup"
        
    # Check to see if the Event Hub already exists
    if ($NamespaceManager.EventHubExists($Path))
    {
        Write-Output "The [$Path] event hub already exists in the [$Namespace] namespace."  
    }
    else
    {
        Write-Output "Creating the [$Path] event hub in the [$Namespace] namespace: PartitionCount=[$PartitionCount] MessageRetentionInDays=[$MessageRetentionInDays]..."
        $EventHubDescription = New-Object -TypeName Microsoft.ServiceBus.Messaging.EventHubDescription -ArgumentList $Path
        $EventHubDescription.PartitionCount = $PartitionCount
        $EventHubDescription.MessageRetentionInDays = $MessageRetentionInDays
        $EventHubDescription.UserMetadata = $UserMetadata
        $EventHubDescription.Path = $Path
        $NamespaceManager.CreateEventHubIfNotExists($EventHubDescription);
        Write-Output "The [$Path] event hub in the [$Namespace] namespace has been successfully created."
    }
        
    # Create the consumer group if it doesn't exist
    Write-Output "Creating the consumer group [$ConsumerGroupName] for the [$Path] event hub..."
    $ConsumerGroupDescription = New-Object -TypeName Microsoft.ServiceBus.Messaging.ConsumerGroupDescription -ArgumentList $Path, $ConsumerGroupName
    $ConsumerGroupDescription.UserMetadata = $ConsumerGroupUserMetadata
    $NamespaceManager.CreateConsumerGroupIfNotExists($ConsumerGroupDescription);
    Write-Output "The consumer group [$ConsumerGroupName] for the [$Path] event hub has been successfully created."
    ```

## <a name="migrate-a-namespace-to-another-azure-subscription"></a>Migrace oboru do jiné předplatné Azure

Následující posloupnost příkazů přesune obor z Azure předplatných na druhou. Provádět operace oboru již musí být aktivní a uživatel spouštějící příkazy Powershellu musíte být správcem v do něj zdrojovou a cílovou předplatná.

```
# Create a new resource group in target subscription
Select-AzureRmSubscription -SubscriptionId 'ffffffff-ffff-ffff-ffff-ffffffffffff'
New-AzureRmResourceGroup -Name 'targetRG' -Location 'East US'

# Move namespace from source subscription to target subscription
Select-AzureRmSubscription -SubscriptionId 'aaaaaaaa-aaaa-aaaa-aaaa-aaaaaaaaaaaa'
$res = Find-AzureRmResource -ResourceNameContains mynamespace -ResourceType 'Microsoft.ServiceBus/namespaces'
Move-AzureRmResource -DestinationResourceGroupName 'targetRG' -DestinationSubscriptionId 'ffffffff-ffff-ffff-ffff-ffffffffffff' -ResourceId $res.ResourceId
```

## <a name="next-steps"></a>Další kroky

Tento článek přidělený základní osnovu zřízení služby Bus entity pomocí Powershellu. Všechno, co můžete udělat pomocí knihoven .NET klienta, můžete taky udělat v skript Powershellu.

Na těchto příspěvků na blogu jsou k dispozici více podrobných příkladů:

- [Jak vytvořit služby Bus fronty, témata a předplatných pomocí skriptu prostředí PowerShell](http://blogs.msdn.com/b/paolos/archive/2014/12/02/how-to-create-a-service-bus-queues-topics-and-subscriptions-using-a-powershell-script.aspx)
- [Jak vytvořit Namespace Bus služby a rozbočovači událostí pomocí skriptu prostředí PowerShell](http://blogs.msdn.com/b/paolos/archive/2014/12/01/how-to-create-a-service-bus-namespace-and-an-event-hub-using-a-powershell-script.aspx)

Některé připravených skripty je také k dispozici ke stažení:
- [Skriptů Powershellu služby Bus](https://code.msdn.microsoft.com/Service-Bus-PowerShell-a46b7059)

<!--Link references-->
[Možnosti nákupu]: http://azure.microsoft.com/pricing/purchase-options/
[Člen nabídky]: http://azure.microsoft.com/pricing/member-offers/
[Bezplatná zkušební verze]: http://azure.microsoft.com/pricing/free-trial/
[Instalace a konfigurace prostředí PowerShell Azure]: ../powershell-install-configure.md
[Služba Bus NuGet balíčku]: http://www.nuget.org/packages/WindowsAzure.ServiceBus/
[Get-AzureSBNamespace]: https://msdn.microsoft.com/library/azure/dn495122.aspx
[Nové AzureSBNamespace]: https://msdn.microsoft.com/library/azure/dn495165.aspx
[Get-AzureSBAuthorizationRule]: https://msdn.microsoft.com/library/azure/dn495113.aspx
[Rozhraní API .NET pro službu Bus]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.aspx
[NamespaceManager]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.aspx
