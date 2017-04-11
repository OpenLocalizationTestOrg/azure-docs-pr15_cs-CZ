<properties
    pageTitle="Správa Azure CDN pomocí prostředí PowerShell | Microsoft Azure"
    description="Naučte se spravovat Azure CDN pomocí rutin prostředí PowerShell Azure."
    services="cdn"
    documentationCenter=""
    authors="camsoper"
    manager="erikre"
    editor=""/>

<tags
    ms.service="cdn"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/17/2016"
    ms.author="casoper"/>


# <a name="manage-azure-cdn-with-powershell"></a>Správa Azure CDN pomocí prostředí PowerShell

Prostředí PowerShell nabízí flexibilní metod ke správě profilů Azure CDN a koncové body.  Můžete prostředí PowerShell interaktivně nebo tak, že psaní skriptů automatizace úkolů správy.  Tento kurz ukazuje několik nejčastější úkoly se dají dělat pomocí Powershellu ke správě profilů Azure CDN a koncové body.

## <a name="prerequisites"></a>Zjistit předpoklady pro

Použití Powershellu ke správě profilů Azure CDN a koncové body, musíte mít instalace modulu Azure Powershellu.  Se dozvíte, jak nainstalovat Azure PowerShell a připojit k Azure pomocí `Login-AzureRmAccount` rutina, přečtěte si, [Jak nainstalovat a nakonfigurovat Azure Powershellu](../powershell-install-configure.md).

>[AZURE.IMPORTANT] Musíte se přihlásit se `Login-AzureRmAccount` před spuštěním rutiny prostředí PowerShell Azure.

## <a name="listing-the-azure-cdn-cmdlets"></a>Výpis rutiny Azure CDN

Seznam všech rutiny Azure CDN pomocí `Get-Command` rutiny.

```text
PS C:\> Get-Command -Module AzureRM.Cdn

CommandType     Name                                               Version    Source
-----------     ----                                               -------    ------
Cmdlet          Get-AzureRmCdnCustomDomain                         2.0.0      AzureRm.Cdn
Cmdlet          Get-AzureRmCdnEndpoint                             2.0.0      AzureRm.Cdn
Cmdlet          Get-AzureRmCdnEndpointNameAvailability             2.0.0      AzureRm.Cdn
Cmdlet          Get-AzureRmCdnOrigin                               2.0.0      AzureRm.Cdn
Cmdlet          Get-AzureRmCdnProfile                              2.0.0      AzureRm.Cdn
Cmdlet          Get-AzureRmCdnProfileSsoUrl                        2.0.0      AzureRm.Cdn
Cmdlet          New-AzureRmCdnCustomDomain                         2.0.0      AzureRm.Cdn
Cmdlet          New-AzureRmCdnEndpoint                             2.0.0      AzureRm.Cdn
Cmdlet          New-AzureRmCdnProfile                              2.0.0      AzureRm.Cdn
Cmdlet          Publish-AzureRmCdnEndpointContent                  2.0.0      AzureRm.Cdn
Cmdlet          Remove-AzureRmCdnCustomDomain                      2.0.0      AzureRm.Cdn
Cmdlet          Remove-AzureRmCdnEndpoint                          2.0.0      AzureRm.Cdn
Cmdlet          Remove-AzureRmCdnProfile                           2.0.0      AzureRm.Cdn
Cmdlet          Set-AzureRmCdnEndpoint                             2.0.0      AzureRm.Cdn
Cmdlet          Set-AzureRmCdnOrigin                               2.0.0      AzureRm.Cdn
Cmdlet          Set-AzureRmCdnProfile                              2.0.0      AzureRm.Cdn
Cmdlet          Start-AzureRmCdnEndpoint                           2.0.0      AzureRm.Cdn
Cmdlet          Stop-AzureRmCdnEndpoint                            2.0.0      AzureRm.Cdn
Cmdlet          Test-AzureRmCdnCustomDomain                        2.0.0      AzureRm.Cdn
Cmdlet          Unpublish-AzureRmCdnEndpointContent                2.0.0      AzureRm.Cdn
```

## <a name="getting-help"></a>Získání nápovědy

Získání nápovědy pomocí kterékoli z těchto rutin pomocí `Get-Help` rutiny.  `Get-Help`poskytuje syntaxi a použití a volitelně zobrazí příklady.

```text
PS C:\> Get-Help Get-AzureRmCdnProfile

NAME
    Get-AzureRmCdnProfile

SYNOPSIS
    Gets an Azure CDN profile.


SYNTAX
    Get-AzureRmCdnProfile [-ProfileName <String>] [-ResourceGroupName <String>] [-InformationAction
    <ActionPreference>] [-InformationVariable <String>] [<CommonParameters>]


DESCRIPTION
    Gets an Azure CDN profile and all related information.


RELATED LINKS

REMARKS
    To see the examples, type: "get-help Get-AzureRmCdnProfile -examples".
    For more information, type: "get-help Get-AzureRmCdnProfile -detailed".
    For technical information, type: "get-help Get-AzureRmCdnProfile -full".

```

## <a name="listing-existing-azure-cdn-profiles"></a>Seznam existujících Azure CDN profilů

`Get-AzureRmCdnProfile` Rutiny bez parametrů načte všechny existující CDN profil.

```powershell
Get-AzureRmCdnProfile
```

Tento výstup lze přesměrovat na rutiny pro výčet.

```powershell
# Output the name of all profiles on this subscription.
Get-AzureRmCdnProfile | ForEach-Object { Write-Host $_.Name }

# Return only **Azure CDN from Verizon** profiles.
Get-AzureRmCdnProfile | Where-Object { $_.Sku.Name -eq "StandardVerizon" }
```

Můžete také vracet jeden profil zadáním skupiny prostředků a název profilu.

```powershell
Get-AzureRmCdnProfile -ProfileName CdnDemo -ResourceGroupName CdnDemoRG
```

>[AZURE.TIP] Je možné mít více profily CDN se stejným názvem tak dlouho, dokud spadají do jiné skupiny prostředků.  Vynechání `ResourceGroupName` parametr vrací všechny profily s odpovídajícím názvem.

## <a name="listing-existing-cdn-endpoints"></a>Seznam existující CDN koncové body

`Get-AzureRmCdnEndpoint`můžete získat jednotlivé koncový bod nebo koncové body v profilu.  

```powershell
# Get a single endpoint.
Get-AzureRmCdnEndpoint -ProfileName CdnDemo -ResourceGroupName CdnDemoRG -EndpointName cdndocdemo

# Get all of the endpoints on a given profile. 
Get-AzureRmCdnEndpoint -ProfileName CdnDemo -ResourceGroupName CdnDemoRG

# Return all of the endpoints on all of the profiles.
Get-AzureRmCdnProfile | Get-AzureRmCdnEndpoint

# Return all of the endpoints in this subscription that are currently running.
Get-AzureRmCdnProfile | Get-AzureRmCdnEndpoint | Where-Object { $_.ResourceState -eq "Running" }
```

## <a name="creating-cdn-profiles-and-endpoints"></a>Vytváření profilů CDN a koncové body

`New-AzureRmCdnProfile`a `New-AzureRmCdnEndpoint` slouží k vytvoření CDN profily a koncové body.

```powershell
# Create a new profile
New-AzureRmCdnProfile -ProfileName CdnPoshDemo -ResourceGroupName CdnDemoRG -Sku StandardAkamai -Location "Central US"

# Create a new endpoint
New-AzureRmCdnEndpoint -ProfileName CdnPoshDemo -ResourceGroupName CdnDemoRG -Location "Central US" -EndpointName cdnposhdoc -OriginName "Contoso" -OriginHostName "www.contoso.com"

# Create a new profile and endpoint (same as above) in one line
New-AzureRmCdnProfile -ProfileName CdnPoshDemo -ResourceGroupName CdnDemoRG -Sku StandardAkamai -Location "Central US" | New-AzureRmCdnEndpoint -EndpointName cdnposhdoc -OriginName "Contoso" -OriginHostName "www.contoso.com"

```

## <a name="checking-endpoint-name-availability"></a>Kontrola dostupnosti název koncového bodu

`Get-AzureRmCdnEndpointNameAvailability`Vrátí objekt označující, zda je k dispozici název koncového bodu.

```powershell
# Retrieve availability
$availability = Get-AzureRmCdnEndpointNameAvailability -EndpointName "cdnposhdoc"

# If available, write a message to the console.
If($availability.NameAvailable) { Write-Host "Yes, that endpoint name is available." }
Else { Write-Host "No, that endpoint name is not available." }
```

## <a name="adding-a-custom-domain"></a>Přidání vlastní domény

`New-AzureRmCdnCustomDomain`Přidá vlastního názvu domény k existující koncový bod.

>[AZURE.IMPORTANT] Musíte nastavit záznamy CNAME u poskytovatele DNS podle popisu v tématu [jak mapovat vlastní doménu koncový bod obsahu doručení Network (CDN)](./cdn-map-content-to-custom-domain.md).  Testování mapování před úpravami koncový bod pomocí `Test-AzureRmCdnCustomDomain`.

```powershell
# Get an existing endpoint
$endpoint = Get-AzureRmCdnEndpoint -ProfileName CdnPoshDemo -ResourceGroupName CdnDemoRG -EndpointName cdnposhdoc

# Check the mapping
$result = Test-AzureRmCdnCustomDomain -CdnEndpoint $endpoint -CustomDomainHostName "cdn.contoso.com"

# Create the custom domain on the endpoint
If($result.CustomDomainValidated){ New-AzureRmCdnCustomDomain -CustomDomainName Contoso -HostName "cdn.contoso.com" -CdnEndpoint $endpoint }
```

## <a name="modifying-an-endpoint"></a>Úprava koncový bod

`Set-AzureRmCdnEndpoint`Změní existující koncový bod.

```powershell
# Get an existing endpoint
$endpoint = Get-AzureRmCdnEndpoint -ProfileName CdnPoshDemo -ResourceGroupName CdnDemoRG -EndpointName cdnposhdoc

# Set up content compression
$endpoint.IsCompressionEnabled = $true
$endpoint.ContentTypesToCompress = "text/javascript","text/css","application/json"

# Save the changed endpoint and apply the changes
Set-AzureRmCdnEndpoint -CdnEndpoint $endpoint
```

## <a name="purgingpre-loading-cdn-assets"></a>Vymazání/komůrkový-loading CDN prostředky

`Unpublish-AzureRmCdnEndpointContent`Vymazat cached prostředky, zatímco `Publish-AzureRmCdnEndpointContent` předem načte prostředky na podporovaných koncové body.

```powershell
# Purge some assets.
Unpublish-AzureRmCdnEndpointContent -ProfileName CdnDemo -ResourceGroupName CdnDemoRG -EndpointName cdndocdemo -PurgeContent "/images/kitten.png","/video/rickroll.mp4"

# Pre-load some assets.
Publish-AzureRmCdnEndpointContent -ProfileName CdnDemo -ResourceGroupName CdnDemoRG -EndpointName cdndocdemo -LoadContent "/images/kitten.png","/video/rickroll.mp4"

# Purge everything in /images/ on all endpoints.
Get-AzureRmCdnProfile | Get-AzureRmCdnEndpoint | Unpublish-AzureRmCdnEndpointContent -PurgeContent "/images/*"
```

## <a name="startingstopping-cdn-endpoints"></a>Spuštění nebo zastavení CDN koncové body
`Start-AzureRmCdnEndpoint`a `Stop-AzureRmCdnEndpoint` lze spustit a zastavit jednotlivé koncové body nebo skupiny koncové body.

```powershell
# Stop the cdndocdemo endpoint
Stop-AzureRmCdnEndpoint -ProfileName CdnDemo -ResourceGroupName CdnDemoRG -EndpointName cdndocdemo

# Stop all endpoints
Get-AzureRmCdnProfile | Get-AzureRmCdnEndpoint | Stop-AzureRmCdnEndpoint

# Start all endpoints
Get-AzureRmCdnProfile | Get-AzureRmCdnEndpoint | Start-AzureRmCdnEndpoint
```

## <a name="deleting-cdn-resources"></a>Odstranění CDN prostředků

`Remove-AzureRmCdnProfile`a `Remove-AzureRmCdnEndpoint` mohou sloužit k odebrání profily a koncové body.

```powershell
# Remove a single endpoint
Remove-AzureRmCdnEndpoint -ProfileName CdnPoshDemo -ResourceGroupName CdnDemoRG -EndpointName cdnposhdoc

# Remove all the endpoints on a profile and skip confirmation (-Force)
Get-AzureRmCdnProfile -ProfileName CdnPoshDemo -ResourceGroupName CdnDemoRG | Get-AzureRmCdnEndpoint | Remove-AzureRmCdnEndpoint -Force

# Remove a single profile
Remove-AzureRmCdnProfile -ProfileName CdnPoshDemo -ResourceGroupName CdnDemoRG
```

## <a name="next-steps"></a>Další kroky

Zjistěte, jak můžete automatizovat Azure CDN s [.NET](./cdn-app-dev-net.md) nebo [Node.js](./cdn-app-dev-node.md).

Další informace o funkcích CDN najdete v tématu [Přehled CDN](./cdn-overview.md).


