<properties
    pageTitle="Připojení k síti virtuální aplikace pomocí prostředí PowerShell"
    description="Pokyny pro připojení k a pracovat s virtuální sítě pomocí prostředí PowerShell"
    services="app-service"
    documentationCenter=""
    authors="ccompy"
    manager="wpickett"
    editor="cephalin"/>

<tags
    ms.service="app-service"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/29/2016"
    ms.author="ccompy"/>

# <a name="connect-your-app-to-your-virtual-network-by-using-powershell"></a>Připojení k síti virtuální aplikace pomocí prostředí PowerShell #

## <a name="overview"></a>Základní informace ##

Aplikace služby Azure můžete připojení k síti Azure virtuální (VNet) aplikace (web, mobilní telefon nebo rozhraní API) ve vašem předplatném. Tato funkce se nazývá VNet integrace. Funkce integrace VNet byste neměli zaměňovat s funkci prostředí služby aplikace, která vám umožní pracovat instanci služby Azure aplikace v síti virtuální.

Funkce integrace VNet má uživatelské rozhraní (UI) v novém portálu využívající integrovat s virtuální sítě, které se instalují pomocí klasické nasazení modelu nebo správce prostředků Azure nasazení modelu. Pokud chcete další informace o funkci, přečtěte si článek [Integrace aplikace s Azure virtuální sítě](web-sites-integrate-with-vnet.md).

Tento článek se není o tom, jak používat uživatelské rozhraní, ale raději o tom, jak povolit integraci pomocí prostředí PowerShell. Protože příkazy pro každý model nasazení se liší, tento článek obsahuje oddíl pro každou nasazení modelu.  

Než budete pokračovat v tomto článku, potřebujete:

- V nejnovější Azure prostředí PowerShell SDK nainstalovaný. Můžete nainstalovat s webové platformy.
- Aplikace služby Azure aplikace spuštěné v standardní nebo Premium SKU.

## <a name="classic-virtual-networks"></a>Klasický virtuálních sítí ##

Tato část popisuje tři úkoly virtuálních sítí, které používají modelu klasické nasazení:

1. Aplikace pro připojení k existující virtuální síti, která má brána a nakonfigurovaný pro připojení čárky webu.
1. Aktualizace údajů o integration virtuální sítě aplikace.
1. Odpojení od virtuální sítě aplikace.

### <a name="connect-an-app-to-a-classic-vnet"></a>Připojení aplikace klasické VNet ###

Aplikace pro připojení k síti virtuální, postupujte podle tyto tři kroky:

1. Deklarujte k web appu, že budou připojovat konkrétní virtuální sítě. Aplikace vygeneruje certifikát, který budou mít virtuální sítě pro připojení čárky webu.
1. Odeslat certifikát webové aplikace virtuální sítě a potom načíst balíček VPN čárky webu URI.
1. Aktualizujte připojení virtuální sítě web appu balíček čárky webu URI.

První a třetí krok jsou plně skriptovatelné, ale druhým krokem vyžaduje jednorázový, ručně akci prostřednictvím portálu nebo access provádět akce **umístění** nebo **oprava** koncový bod správce prostředků Azure virtuální sítě. Kontaktujte podporu Azure je povolený. Než začnete, zkontrolujte, jestli klasické virtuální síť, která má čárky webu připojení zapnuto a nasazené bránu. Vytvoření brány a povolení připojení čárky webu, budete muset používat portál, jak je uvedeno na [Vytvoření brány VPN][createvpngateway].

Klasický virtuální sítě musí být v rámci stejného předplatného jako vaše aplikace plán služeb obsahující aplikace, integrace s.

##### <a name="set-up-azure-powershell-sdk"></a>Nastavení prostředí PowerShell SDK Azure #####

Otevřete okno prostředí PowerShell a nastavit účet Azure a předplatného pomocí:

    Login-AzureRmAccount

Tento příkaz se otevře okno s výzvou k získání přihlašovacích údajů Azure. Po přihlášení, použijte některý z následujících příkazů vyberte předplatné, ke které chcete použít. Ujistěte se, že používáte předplatné, které jsou v virtuální sítě a plán služeb aplikací.

    Select-AzureRmSubscription –SubscriptionName [WebAppSubscriptionName]

nebo

    Select-AzureRmSubscription –SubscriptionId [WebAppSubscriptionId]

##### <a name="variables-used-in-this-article"></a>Proměnné použitých v tomto článku #####

Zjednodušit příkazy, jsme nastavit proměnnou **$Configuration** prostředí PowerShell s konkrétní konfigurace.

Nastavte proměnnou takto v prostředí PowerShell pomocí následujících parametrů:

    $Configuration = @{}
    $Configuration.WebAppResourceGroup = "[Your web app resource group]"
    $Configuration.WebAppName = "[Your web app name]"
    $Configuration.VnetSubscriptionId = "[Your vnet subscription id]"
    $Configuration.VnetResourceGroup = "[Your vnet resource group]"
    $Configuration.VnetName = "[Your vnet name]"

Umístění aplikace by mělo být místě bez mezery. Západ USA je například westus.

    $Configuration.WebAppLocation = "[Your web app Location]"

Na další položku je, kdy má někdo podepsat certifikát. Je třeba zapisovatelný cestu ve vašem počítači. Nezapomeňte zadat .cer na konci.

    $Configuration.GeneratedCertificatePath = "[C:\Path\To\Certificate.cer]"

Pokud chcete zobrazit, můžete nastavit, zadejte **$Configuration**.

    > $Configuration

    Name                           Value
    ----                           -----
    GeneratedCertificatePath       C:\vnetcert.cer
    VnetSubscriptionId             efc239a4-88f9-2c5e-a9a1-3034c21ad496
    WebAppResourceGroup            vnetdemo-rg
    VnetResourceGroup              testase1-rg
    VnetName                       TestNetwork
    WebAppName                     vnetintdemoapp
    WebAppLocation                 centralus

Zbývající část tohoto oddílu předpokládá, že máte Proměnná vytvořená postupem uvedeným jenom.

##### <a name="declare-the-virtual-network-to-the-app"></a>Deklarovat virtuální sítě do aplikace #####

Pomocí následujícího příkazu zjistit aplikace, že se pomocí této konkrétní virtuální sítě. Dojde k aplikaci generovat potřebné certifikátů:

    $vnet = New-AzureRmResource -Name "$($Configuration.WebAppName)/$($Configuration.VnetName)" -ResourceGroupName $Configuration.WebAppResourceGroup -ResourceType "Microsoft.Web/sites/virtualNetworkConnections" -PropertyObject @{"VnetResourceId" = "/subscriptions/$($Configuration.VnetSubscriptionId)/resourceGroups/$($Configuration.VnetResourceGroup)/providers/Microsoft.ClassicNetwork/virtualNetworks/$($Configuration.VnetName)"} -Location $Configuration.WebAppLocation -ApiVersion 2015-07-01

Pokud tento příkaz úspěšné, **$vnet** měli proměnnou **Vlastnosti** v nich. Proměnná **Vlastnosti** by měl obsahovat miniaturu certifikátu a data certifikátu.

##### <a name="upload-the-web-app-certificate-to-the-virtual-network"></a>Nahrání webové aplikace certifikátu do virtuální sítě #####

Příručku, jednorázové kroku je nutný pro každého předplatného a kombinaci virtuální sítě. To znamená pokud se připojujete aplikace v předplatné A virtuální sítě a, je potřeba udělat tento krok pouze jednou bez ohledu na to, kolik aplikace nakonfigurujete. Chcete-li přidat novou aplikaci k jiné síti virtuální, musíte udělat znovu. Důvodem je, že sadu certifikáty generováno úrovni předplatné v aplikaci služby Azure a sadu vygeneruje se jednou pro každý virtuální sítě, které aplikace se připojí k.

Certifikáty se již byly nastavili Pokud jste postupovali podle těchto kroků nebo integrovaný s stejné virtuální sítě tak, že na portálu.

Cílem prvního kroku je vytvořit soubor .cer. Druhým krokem je nahrát soubor .cer virtuální sítě. Chcete-li soubor .cer negeneruje volání rozhraní API v předchozím kroku, spusťte následující příkazy.

    $certBytes = [System.Convert]::FromBase64String($vnet.Properties.certBlob)
    [System.IO.File]::WriteAllBytes("$($Configuration.GeneratedCertificatePath)", $certBytes)

Certifikát najdete v umístění, které **$Configuration.GeneratedCertificatePath** určuje.

Odeslat certifikát ručně, použijte [Azure portál] [ azureportal] a **Procházet virtuální sítě (klasické)** > **sítě VPN** > **bodu webu** > **Správa certifikátů**. Tady Nahrajte svůj certifikát.

##### <a name="get-the-point-to-site-package"></a>Získání balíček čárky webu #####

Dalším krokem při nastavování virtuální síťové připojení na web appu je získat balíček čárky webu a zadejte do webové aplikace.

Následující šablonu uložte do souboru s názvem GetNetworkPackageUri.json jinam, postupujte v počítači, například C:\Azure\Templates\GetNetworkPackageUri.json.

    {
        "$schema": "http://schema.management.azure.com/schemas/2014-04-01-preview/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
            "certData": {
                "type": "string"
            },
            "certThumbprint": {
                "type": "string"
            },
            "networkName": {
                "type": "string"
            }
        },
        "variables": {
            "legacyVnetName": "[concat('Group ', resourceGroup().name, ' ', parameters('networkName'))]"
            },
            "resources": [
            ],
        "outputs" : {
            "PackageUri" :
            {
            "value" : "[listPackage(resourceId('Microsoft.ClassicNetwork/virtualNetworks/gateways/clientRootCertificates', parameters('networkName'), 'primary', parameters('certThumbprint')), '2014-06-01').packageUri]", "type" : "string"
            }
        }
    }


Nastavení vstupní parametry:

    $parameters = @{"certData" = $vnet.Properties.certBlob ;
    certThumbprint = $vnet.Properties.certThumbprint ;
    "networkName" = $Configuration.VnetName }

Volejte skript:

    $output = New-AzureRmResourceGroupDeployment -Name unused -ResourceGroupName $Configuration.VnetResourceGroup -TemplateParameterObject $parameters -TemplateFile C:\PATH\TO\GetNetworkPackageUri.json


Proměnná **$output. Outputs.packageUri** se teď měla obsahovat balíček URI do dostali do webové aplikace.

##### <a name="upload-the-point-to-site-package-to-your-app"></a>Nahrání balíčku čárky webu do aplikace #####

Posledním krokem je poskytnout balíček aplikace. Spusťte následující příkaz:

    $vnet = New-AzureRmResource -Name "$($Configuration.WebAppName)/$($Configuration.VnetName)/primary" -ResourceGroupName $Configuration.WebAppResourceGroup -ResourceType "Microsoft.Web/sites/virtualNetworkConnections/gateways" -ApiVersion 2015-07-01 -PropertyObject @{"VnetName" = $Configuration.VnetName ; "VpnPackageUri" = $($output.Outputs.packageUri).Value } -Location $Configuration.WebAppLocation

Pokud zpráva žádostí o potvrzení, že jsou přepsat existující zdroj, ujistěte se k tomu, aby.

Po úspěšném tento příkaz aplikace by se připojili k virtuální sítě. Abyste si ověřili úspěch, přejděte do app konzoly a zadejte tento příkaz:

    SET WEBSITE_

Pokud je proměnná prostředí s názvem WEBSITE_VNETNAME obsahující hodnotu, která odpovídá názvu cílové virtuální síti, všechny konfigurace úspěšné.

### <a name="update-classic-vnet-integration-information"></a>Aktualizovat informace o integration klasické VNet ###

Aktualizace nebo synchronizován informacím, jednoduše opakováním kroků, které jste provedli při vytváření integrace na prvním místě. Tyto kroky jsou:

1. Definování informace o konfiguraci.
1. Deklarujte virtuální sítě do aplikace.
1. Pokud potřebujete balíček čárky webu.
1. Uložení balíčku čárky webu do aplikace.

### <a name="disconnect-your-app-from-a-classic-vnet"></a>Odpojení aplikace klasické VNet ###

Odpojení aplikaci, musíte informace o konfiguraci nastavené při integraci virtuální sítě. Pomocí těchto informací, je pak příkaz Odpojit aplikace od virtuální síť.

    $vnet = Remove-AzureRmResource -Name "$($Configuration.WebAppName)/$($Configuration.VnetName)" -ResourceGroupName $Configuration.WebAppResourceGroup -ResourceType "Microsoft.Web/sites/virtualNetworkConnections" -ApiVersion 2015-07-01

## <a name="resource-manager-virtual-networks"></a>Správce prostředků virtuálních sítí ##

Správce prostředků virtuální mít rozhraní API Správce prostředků Azure, která zjednodušit některé procesy ve srovnání s klasický virtuální sítě. Máme skript, který vám pomůže udělali úkoly podle těchto pokynů:

- Vytvoření správce prostředků virtuální sítě a integrovat aplikace.
- Vytvoření brány, konfigurace připojení k službě čárky webu v síti virtuální existující správce prostředků a integrace aplikace s ním.
- Integrace aplikace s existující virtuální síť správce prostředků, která má brána a bodu server připojení povolené.
- Odpojení od virtuální sítě aplikace.

### <a name="resource-manager-vnet-app-service-integration-script"></a>Správce prostředků VNet aplikaci služby integrace skriptu ###

Zkopírujte tento skript a uložte jej do souboru. Pokud nechcete používat skript, neváhejte čerpat z ho uvidíte, jak nastavit jednotlivé položky s virtuální sítě Správce prostředků.


    function ReadHostWithDefault($message, $default)
    {
        $result = Read-Host "$message [$default]"
        if($result -eq "")
        {
            $result = $default
        }
            return $result
        }

    function PromptCustom($title, $optionValues, $optionDescriptions)
    {
        Write-Host $title
        Write-Host
        $a = @()
        for($i = 0; $i -lt $optionValues.Length; $i++){
            Write-Host "$($i+1))" $optionDescriptions[$i]
        }
        Write-Host

        while($true)
        {
            Write-Host "Choose an option: "
            $option = Read-Host
            $option = $option -as [int]

            if($option -ge 1 -and $option -le $optionValues.Length)
            {
                return $optionValues[$option-1]
            }
        }
    }

    function PromptYesNo($title, $message, $default = 0)
    {
        $yes = New-Object System.Management.Automation.Host.ChoiceDescription "&Yes", ""
        $no = New-Object System.Management.Automation.Host.ChoiceDescription "&No", ""
        $options = [System.Management.Automation.Host.ChoiceDescription[]]($yes, $no)
        $result = $host.ui.PromptForChoice($title, $message, $options, $default)
        return $result
    }

    function CreateVnet($resourceGroupName, $vnetName, $vnetAddressSpace, $vnetGatewayAddressSpace, $location)
    {
        Write-Host "Creating a new VNET"
        $gatewaySubnet = New-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -AddressPrefix $vnetGatewayAddressSpace
        New-AzureRmVirtualNetwork -Name $vnetName -ResourceGroupName $resourceGroupName -Location $location -AddressPrefix $vnetAddressSpace -Subnet $gatewaySubnet
    }

    function CreateVnetGateway($resourceGroupName, $vnetName, $vnetIpName, $location, $vnetIpConfigName, $vnetGatewayName, $certificateData, $vnetPointToSiteAddressSpace)
    {
        $vnet = Get-AzureRmVirtualNetwork -Name $vnetName -ResourceGroupName $resourceGroupName
        $subnet=Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet

        Write-Host "Creating a public IP address for this VNET"
        $pip = New-AzureRmPublicIpAddress -Name $vnetIpName -ResourceGroupName $resourceGroupName -Location $location -AllocationMethod Dynamic
        $ipconf = New-AzureRmVirtualNetworkGatewayIpConfig -Name $vnetIpConfigName -Subnet $subnet -PublicIpAddress $pip

        Write-Host "Adding a root certificate to this VNET"
        $root = New-AzureRmVpnClientRootCertificate -Name "AppServiceCertificate.cer" -PublicCertData $certificateData

        Write-Host "Creating Azure VNET Gateway. This may take up to an hour."
        New-AzureRmVirtualNetworkGateway -Name $vnetGatewayName -ResourceGroupName $resourceGroupName -Location $location -IpConfigurations $ipconf -GatewayType Vpn -VpnType RouteBased -EnableBgp $false -GatewaySku Basic -VpnClientAddressPool $vnetPointToSiteAddressSpace -VpnClientRootCertificates $root
    }

    function AddNewVnet($subscriptionId, $webAppResourceGroup, $webAppName)
    {
        Write-Host "Adding a new Vnet"
        Write-Host
        $vnetName = Read-Host "Specify a name for this Virtual Network"

        $vnetGatewayName="$($vnetName)-gateway"
        $vnetIpName="$($vnetName)-ip"
        $vnetIpConfigName="$($vnetName)-ipconf"

        # Virtual Network settings
        $vnetAddressSpace="10.0.0.0/8"
        $vnetGatewayAddressSpace="10.5.0.0/16"
        $vnetPointToSiteAddressSpace="172.16.0.0/16"

        $changeRequested = 0
        $resourceGroupName = $webAppResourceGroup

        while($changeRequested -eq 0)
        {
            Write-Host
            Write-Host "Currently, I will create a VNET with the following settings:"
            Write-Host
            Write-Host "Virtual Network Name: $vnetName"
            Write-Host "Resource Group Name:  $resourceGroupName"
            Write-Host "Gateway Name: $vnetGatewayName"
            Write-Host "Vnet IP name: $vnetIpName"
            Write-Host "Vnet IP config name:  $vnetIpConfigName"
            Write-Host "Address Space:$vnetAddressSpace"
            Write-Host "Gateway Address Space:$vnetGatewayAddressSpace"
            Write-Host "Point-To-Site Address Space:  $vnetPointToSiteAddressSpace"
            Write-Host
            $changeRequested = PromptYesNo "" "Do you wish to change these settings?" 1

            if($changeRequested -eq 0)
            {
                $vnetName = ReadHostWithDefault "Virtual Network Name" $vnetName
                $resourceGroupName = ReadHostWithDefault "Resource Group Name" $resourceGroupName
                $vnetGatewayName = ReadHostWithDefault "Vnet Gateway Name" $vnetGatewayName
                $vnetIpName = ReadHostWithDefault "Vnet IP name" $vnetIpName
                $vnetIpConfigName = ReadHostWithDefault "Vnet IP configuration name" $vnetIpConfigName
                $vnetAddressSpace = ReadHostWithDefault "Vnet Address Space" $vnetAddressSpace
                $vnetGatewayAddressSpace = ReadHostWithDefault "Vnet Gateway Address Space" $vnetGatewayAddressSpace
                $vnetPointToSiteAddressSpace = ReadHostWithDefault "Vnet Point-to-site Address Space" $vnetPointToSiteAddressSpace
            }
        }

        $ErrorActionPreference = "Stop";

        # We create the virtual network and add it here. The way this works is:
        # 1) Add the VNET association to the App. This allows the App to generate certificates, etc. for the VNET.
        # 2) Create the VNET and VNET gateway, add the certificates, create the public IP, etc., required for the gateway
        # 3) Get the VPN package from the gateway and pass it back to the App.

        $webApp = Get-AzureRmResource -ResourceName $webAppName -ResourceType "Microsoft.Web/sites" -ApiVersion 2015-08-01 -ResourceGroupName $webAppResourceGroup
        $location = $webApp.Location

        Write-Host "Creating App association to VNET"
        $propertiesObject = @{
         "vnetResourceId" = "/subscriptions/$($subscriptionId)/resourceGroups/$($resourceGroupName)/providers/Microsoft.Network/virtualNetworks/$($vnetName)"
        }
        $virtualNetwork = New-AzureRmResource -Location $location -Properties $PropertiesObject -ResourceName "$($webAppName)/$($vnetName)" -ResourceType "Microsoft.Web/sites/virtualNetworkConnections" -ApiVersion 2015-08-01 -ResourceGroupName $webAppResourceGroup -Force

        CreateVnet $resourceGroupName $vnetName $vnetAddressSpace $vnetGatewayAddressSpace $location

        CreateVnetGateway $resourceGroupName $vnetName $vnetIpName $location $vnetIpConfigName $vnetGatewayName $virtualNetwork.Properties.CertBlob $vnetPointToSiteAddressSpace

        Write-Host "Retrieving VPN Package and supplying to App"
        $packageUri = Get-AzureRmVpnClientPackage -ResourceGroupName $resourceGroupName -VirtualNetworkGatewayName $vnetGatewayName -ProcessorArchitecture Amd64

        # Put the VPN client configuration package onto the App
        $PropertiesObject = @{
        "vnetName" = $VirtualNetworkName; "vpnPackageUri" = $packageUri
        }

        New-AzureRmResource -Location $location -Properties $PropertiesObject -ResourceName "$($webAppName)/$($vnetName)/primary" -ResourceType "Microsoft.Web/sites/virtualNetworkConnections/gateways" -ApiVersion 2015-08-01 -ResourceGroupName $webAppResourceGroup -Force

        Write-Host "Finished!"
    }

    function AddExistingVnet($subscriptionId, $resourceGroupName, $webAppName)
    {
        $ErrorActionPreference = "Stop";

        # At this point, the gateway should be able to be joined to an App, but may require some minor tweaking. We will declare to the App now to use this VNET
        Write-Host "Getting App information"
        $webApp = Get-AzureRmResource -ResourceName $webAppName -ResourceType "Microsoft.Web/sites" -ApiVersion 2015-08-01 -ResourceGroupName $resourceGroupName
        $location = $webApp.Location

        $webAppConfig = Get-AzureRmResource -ResourceName "$($webAppName)/web" -ResourceType "Microsoft.Web/sites/config" -ApiVersion 2015-08-01 -ResourceGroupName $resourceGroupName
        $currentVnet = $webAppConfig.Properties.VnetName
        if($currentVnet -ne $null -and $currentVnet -ne "")
        {
            Write-Host "Currently connected to VNET $currentVnet"
        }

        # Display existing vnets
        $vnets = Get-AzureRmVirtualNetwork
        $vnetNames = @()
        foreach($vnet in $vnets)
        {
            $vnetNames += $vnet.Name
        }

        Write-Host
        $vnet = PromptCustom "Select a VNET to integrate with" $vnets $vnetNames

        # We need to check if this VNET is able to be joined to a App, based on following criteria
            # If there is no gateway, we can create one.
            # If there is a gateway:
                # It must be of type Vpn
                # It must be of VpnType RouteBased
                # If it doesn't have the right certificate, we will need to add it.
                # If it doesn't have a point-to-site range, we will need to add it.

        $gatewaySubnet = $vnet.Subnets | Where-Object { $_.Name -eq "GatewaySubnet" }

        if($gatewaySubnet -eq $null -or $gatewaySubnet.IpConfigurations -eq $null -or $gatewaySubnet.IpConfigurations.Count -eq 0)
        {
            $ErrorActionPreference = "Continue";
            # There is no gateway. We need to create one.
            Write-Host "This Virtual Network has no gateway. I will need to create one."

            $vnetName = $vnet.Name
            $vnetGatewayName="$($vnetName)-gateway"
            $vnetIpName="$($vnetName)-ip"
            $vnetIpConfigName="$($vnetName)-ipconf"

            # Virtual Network settings
            $vnetAddressSpace="10.0.0.0/8"
            $vnetGatewayAddressSpace="10.5.0.0/16"
            $vnetPointToSiteAddressSpace="172.16.0.0/16"

            $changeRequested = 0

            Write-Host "Your VNET is in the address space $($vnet.AddressSpace.AddressPrefixes), with the following Subnets:"
            foreach($subnet in $vnet.Subnets)
            {
                Write-Host "$($subnet.Name): $($subnet.AddressPrefix)"
            }

            $vnetGatewayAddressSpace = Read-Host "Please choose a GatewaySubnet address space"

            while($changeRequested -eq 0)
            {
                Write-Host
                Write-Host "Currently, I will create a VNET gateway with the following settings:"
                Write-Host
                Write-Host "Virtual Network Name: $vnetName"
                Write-Host "Resource Group Name:  $($vnet.ResourceGroupName)"
                Write-Host "Gateway Name: $vnetGatewayName"
                Write-Host "Vnet IP name: $vnetIpName"
                Write-Host "Vnet IP config name:  $vnetIpConfigName"
                Write-Host "Address Space:$($vnet.AddressSpace.AddressPrefixes)"
                Write-Host "Gateway Address Space:$vnetGatewayAddressSpace"
                Write-Host "Point-To-Site Address Space:  $vnetPointToSiteAddressSpace"
                Write-Host
                $changeRequested = PromptYesNo "" "Do you wish to change these settings?" 1

                if($changeRequested -eq 0)
                {
                    $vnetGatewayName = ReadHostWithDefault "Vnet Gateway Name" $vnetGatewayName
                    $vnetIpName = ReadHostWithDefault "Vnet IP name" $vnetIpName
                    $vnetIpConfigName = ReadHostWithDefault "Vnet IP configuration name" $vnetIpConfigName
                    $vnetGatewayAddressSpace = ReadHostWithDefault "Vnet Gateway Address Space" $vnetGatewayAddressSpace
                    $vnetPointToSiteAddressSpace = ReadHostWithDefault "Vnet Point-to-site Address Space" $vnetPointToSiteAddressSpace
                }
            }

            $ErrorActionPreference = "Stop";

            Write-Host "Creating App association to VNET"
            $propertiesObject = @{
             "vnetResourceId" = "/subscriptions/$($subscriptionId)/resourceGroups/$($vnet.ResourceGroupName)/providers/Microsoft.Network/virtualNetworks/$($vnetName)"
            }

            $virtualNetwork = New-AzureRmResource -Location $location -Properties $PropertiesObject -ResourceName "$($webAppName)/$($vnet.Name)" -ResourceType "Microsoft.Web/sites/virtualNetworkConnections" -ApiVersion 2015-08-01 -ResourceGroupName $resourceGroupName -Force

            # If there is no gateway subnet, we need to create one.
            if($gatewaySubnet -eq $null)
            {
                $gatewaySubnet = New-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -AddressPrefix $vnetGatewayAddressSpace
                $vnet.Subnets.Add($gatewaySubnet);
                Set-AzureRmVirtualNetwork -VirtualNetwork $vnet
            }

            CreateVnetGateway $vnet.ResourceGroupName $vnetName $vnetIpName $location $vnetIpConfigName $vnetGatewayName $virtualNetwork.Properties.CertBlob $vnetPointToSiteAddressSpace

            $gateway = Get-AzureRmVirtualNetworkGateway -ResourceGroupName $vnet.ResourceGroupName -Name $vnetGatewayName
        }
        else
        {
            $uriParts = $gatewaySubnet.IpConfigurations[0].Id.Split('/')
            $gatewayResourceGroup = $uriParts[4]
            $gatewayName = $uriParts[8]

            $gateway = Get-AzureRmVirtualNetworkGateway -ResourceGroupName $vnet.ResourceGroupName -Name $gatewayName

            # validate gateway types, etc.
            if($gateway.GatewayType -ne "Vpn")
            {
                Write-Error "This gateway is not of the Vpn type. It cannot be joined to an App."
                return
            }

            if($gateway.VpnType -ne "RouteBased")
            {
                Write-Error "This gateways Vpn type is not RouteBased. It cannot be joined to an App."
                return
            }

            if($gateway.VpnClientConfiguration -eq $null -or $gateway.VpnClientConfiguration.VpnClientAddressPool -eq $null)
            {
                Write-Host "This gateway does not have a Point-to-site Address Range. Please specify one in CIDR notation, e.g. 10.0.0.0/8"
                $pointToSiteAddress = Read-Host "Point-To-Site Address Space"
                Set-AzureRmVirtualNetworkGatewayVpnClientConfig -VirtualNetworkGateway $gateway.Name -VpnClientAddressPool $pointToSiteAddress
            }

            Write-Host "Creating App association to VNET"
            $propertiesObject = @{
             "vnetResourceId" = "/subscriptions/$($subscriptionId)/resourceGroups/$($vnet.ResourceGroupName)/providers/Microsoft.Network/virtualNetworks/$($vnet.Name)"
            }

            $virtualNetwork = New-AzureRmResource -Location $location -Properties $PropertiesObject -ResourceName "$($webAppName)/$($vnet.Name)" -ResourceType "Microsoft.Web/sites/virtualNetworkConnections" -ApiVersion 2015-08-01 -ResourceGroupName $resourceGroupName -Force

            # We need to check if the certificate here exists in the gateway.
            $certificates = $gateway.VpnClientConfiguration.VpnClientRootCertificates

            $certFound = $false
            foreach($certificate in $certificates)
            {
                if($certificate.PublicCertData -eq $virtualNetwork.Properties.CertBlob)
                {
                    $certFound = $true
                    break
                }
            }

            if(-not $certFound)
            {
                Write-Host "Adding certificate"
                Add-AzureRmVpnClientRootCertificate -VpnClientRootCertificateName "AppServiceCertificate.cer" -PublicCertData $virtualNetwork.Properties.CertBlob -VirtualNetworkGatewayName $gateway.Name
            }
        }

        # Now finish joining by getting the VPN package and giving it to the App
        Write-Host "Retrieving VPN Package and supplying to App"
        $packageUri = Get-AzureRmVpnClientPackage -ResourceGroupName $vnet.ResourceGroupName -VirtualNetworkGatewayName $gateway.Name -ProcessorArchitecture Amd64

        # Put the VPN client configuration package onto the App
        $PropertiesObject = @{
        "vnetName" = $vnet.Name; "vpnPackageUri" = $packageUri
        }

        New-AzureRmResource -Location $location -Properties $PropertiesObject -ResourceName "$($webAppName)/$($vnet.Name)/primary" -ResourceType "Microsoft.Web/sites/virtualNetworkConnections/gateways" -ApiVersion 2015-08-01 -ResourceGroupName $resourceGroupName -Force

        Write-Host "Finished!"
    }

    function RemoveVnet($subscriptionId, $resourceGroupName, $webAppName)
    {
        $webAppConfig = Get-AzureRmResource -ResourceName "$($webAppName)/web" -ResourceType "Microsoft.Web/sites/config" -ApiVersion 2015-08-01 -ResourceGroupName $resourceGroupName
        $currentVnet = $webAppConfig.Properties.VnetName
        if($currentVnet -ne $null -and $currentVnet -ne "")
        {
            Write-Host "Currently connected to VNET $currentVnet"

            Remove-AzureRmResource -ResourceName "$($webAppName)/$($currentVnet)" -ResourceType "Microsoft.Web/sites/virtualNetworkConnections" -ApiVersion 2015-08-01 -ResourceGroupName $resourceGroupName
        }
          else
        {
          Write-Host "Not connected to a VNET."
        }
    }

    Write-Host "Please Login"
    Login-AzureRmAccount

    # Choose subscription. If there's only one we will choose automatically

    $subs = Get-AzureRmSubscription
    $subscriptionId = ""

    if($subs.Length -eq 0)
    {
        Write-Error "No subscriptions bound to this account."
        return
    }

    if($subs.Length -eq 1)
    {
        $subscriptionId = $subs[0].SubscriptionId
    }
    else
    {
        $subscriptionChoices = @()
        $subscriptionValues = @()

        foreach($subscription in $subs){
        $subscriptionChoices += "$($subscription.SubscriptionName) ($($subscription.SubscriptionId))";
        $subscriptionValues += ($subscription.SubscriptionId);
        }

        $subscriptionId = PromptCustom "Choose a subscription" $subscriptionValues $subscriptionChoices
    }

    Select-AzureRmSubscription -SubscriptionId $subscriptionId

    $resourceGroup = Read-Host "Please enter the Resource Group of your App"

    $appName = Read-Host "Please enter the Name of your App"

    $options = @("Add a NEW Virtual Network to an App", "Add an EXISTING Virtual Network to an App", "Remove a Virtual Network from an App");
    $optionValues = @(0, 1, 2)
    $option = PromptCustom "What do you want to do?" $optionValues $options

    if($option -eq 0)
    {
        AddNewVnet $subscriptionId $resourceGroup $appName
    }
    if($option -eq 1)
    {
        AddExistingVnet $subscriptionId $resourceGroup $appName
    }
    if($option -eq 2)
    {
        RemoveVnet $subscriptionId $resourceGroup $appName
    }

Uložte kopii skript. V tomto článku je označená jako V2VnetAllinOne.ps1, ale můžete použít jiný název. Existují pro tento skript žádné argumenty. Můžete jednoduše ho spusťte. První věc, kterou skript udělá se výzva k přihlášení. Po přihlášení, skript získá podrobnosti o účtu a vrátí seznam předplatných. Nepočítají žádosti o své přihlašovací údaje, spuštění počáteční skriptu vypadat takto:

    PS C:\Users\ccompy\Documents\VNET> .\V2VnetAllInOne.ps1
    Please Login

    Environment           : AzureCloud
    Account               : ccompy@microsoft.com
    TenantId              : 722278f-fef1-499f-91ab-2323d011db47
    SubscriptionId        : af5358e1-acac-2c90-a9eb-722190abf47a
    CurrentStorageAccount :

    Choose a subscription

    1) Ukázka předplatného (af5358e1-acac-2c90-a9eb-722190abf47a)
    2) Test MS (a5350f55-dd5a-41ec-2ddb-ff7b911bb2ef)
    3) Fialové ukázku předplatného (2d4c99a4-57f9-4d5e-a0a1-0034c52db59d)

    Vyberte jednu z možností: 3

    Účet: ccompy@microsoft.com prostředí: AzureCloud předplatné: 2d4c99a4-57f9-4d5e-a0a1-0034c52db59d klienta: 722278f-fef1-499f-91ab-2323d011db47

    Zadejte skupina zdroje aplikace: hcdemo rg zadejte název aplikace: v2vnetpowershell co chcete udělat?

    1) Přidání nového virtuální sítě do aplikace
    2) Přidání virtuální sítě existující do aplikace
    3) Odebrání aplikace virtuální sítě

Zbytek tento oddíl vysvětluje každý z těchto tří možností.

### <a name="create-a-resource-manager-vnet-and-integrate-with-it"></a>Vytvoření správce prostředků VNet a integrace s ním ###

Chcete-li vytvořit novou virtuální síti, která používá správce prostředků nasazení modelu a integrovat s aplikací, vyberte **1) přidat novou síť virtuální aplikaci ještě**. To vás vyzve k název virtuální sítě. Moje písmeny na začátku jak vidíte v následující nastavení můžu použili název v2pshell.

Skript zobrazí podrobnosti o virtuální sítě, ke které se vytváří. Pokud chcete, můžete změnit některou z hodnot V tomto příkladu spuštění jsem vytvořil(a) virtuální sítě, který má následující nastavení:

    Virtual Network Name:         v2pshell
    Resource Group Name:          hcdemo-rg
    Gateway Name:                 v2pshell-gateway
    Vnet IP name:                 v2pshell-ip
    Vnet IP config name:          v2pshell-ipconf
    Address Space:                10.0.0.0/8
    Gateway Address Space:        10.5.0.0/16
    Point-To-Site Address Space:  172.16.0.0/12

    Do you wish to change these settings?
    [Y] Yes  [N] No  [?] Help (default is "N"):

Pokud chcete změnit některou z hodnot, zadejte **Y** a proveďte požadované změny. Až budete spokojeni s nastavením virtuální sítě, zadejte **N** nebo stisknutím klávesy Enter po zobrazení výzvy o změně nastavení. V ní na až do dokončení skript se dozvíte, část jaký it "i's způsobem, dokud nezačne vytvořte bránu virtuální sítě. Tento krok může trvat až jednu hodinu. Existuje žádné indikátory průběhu v této fázi, ale skript vám poradí, kdy byl vytvořen brány.

Po dokončení skript bude uveden **Dokončeno**. V tomto okamžiku bude mít správce prostředků virtuální síť, která obsahuje název a nastavení, které jste vybrali. Tato nová virtuální síť se také integrována s aplikací.

### <a name="integrate-your-app-with-a-preexisting-resource-manager-vnet"></a>Integrace aplikace s existující VNet správce prostředků ###

Když máte integrace s existující virtuální sítě, pokud zadáte správce prostředků virtuální sítě, která neobsahuje brána nebo čárky webu připojení, skript, nastaví se. Pokud VNET už má tyto kroky nastavení, skript přejde přímo do integrace aplikace. Jestli Pokud chcete spustit tento proces, jednoduše vyberte **2) přidání existující virtuální síť do aplikace**.

Tato možnost funguje jenom v případě, že máte existující správce prostředků virtuální síti, která je ve stejném předplatném jako aplikace. Když vyberete možnost, zobrazí se seznam z vašich sítí virtuální správce prostředků.   

    Select a VNET to integrate with

    1) v2demonetwork
    2) v2pshell
    3) v2vnetintdemo
    4) v2asenetwork
    5) v2pshell2

    Vyberte jednu z možností: 5

Jednoduše vyberte virtuální sítě, ke které chcete integrovat. Pokud už máte brány, který má připojení čárky webů povolena, skript aplikace jednoduše integrace s virtuální síť. Pokud nemáte brány, musíte zadat podsítě brány. Vaší podsítě brány musí být v adresní prostor virtuální sítě a nesmí být v jiných podsítě. Pokud máte virtuální sítě bez brány a spustit tento krok, co potřebujete vypadat takto:

    This Virtual Network has no gateway. I will need to create one.
    Your VNET is in the address space 172.16.0.0/16, with the following Subnets:
    default: 172.16.0.0/24
    Please choose a GatewaySubnet address space: 172.16.1.0/26

V tomto příkladu jsem vytvořil(a) brány virtuální sítě, která obsahuje následující nastavení:

    Virtual Network Name:         v2pshell2
    Resource Group Name:          vnetdemo-rg
    Gateway Name:                 v2pshell2-gateway
    Vnet IP name:                 v2pshell2-ip
    Vnet IP config name:          v2pshell2-ipconf
    Address Space:                172.16.0.0/16
    Gateway Address Space:        172.16.1.0/26
    Point-To-Site Address Space:  172.16.0.0/12

    Do you wish to change these settings?
    [Y] Yes  [N] No  [?] Help (default is "N"):
    Creating App association to VNET

Pokud chcete změnit některou z těchto nastavení, můžete to provést. Jinak, stiskněte klávesu Enter a skript vytvoříte brány a připojení k síti virtuální aplikace. Údaje o času vytvoření brány je stále hodinu, i když, proto se ujistěte, že, mějte na paměti. Po dokončení všechno, co bude uveden skript **Dokončeno**.

### <a name="disconnect-your-app-from-a-resource-manager-vnet"></a>Odpojení od správce prostředků VNet aplikace ###

Odpojení virtuální sítě aplikace trvat dolů brány nebo zakázání připojení čárky webu. Přece používáte je něco jiného. Je taky neukončí ji z jiných aplikací než tu, kterou jste zadali. K provedení této akce, vyberte **3) odebrat virtuální síť z aplikace**. Když uděláte, zobrazí se přibližně takto:

    Currently connected to VNET v2pshell

    Confirm
    Are you sure you want to delete the following resource:
    /subscriptions/edcc99a4-b7f9-4b5e-a9a1-3034c51db496/resourceGroups/hcdemo-rg/providers/Microsoft.Web/sites/v2vnetpowers
    hell/virtualNetworkConnections/v2pshell
    [Y] Yes  [N] No  [S] Suspend  [?] Help (default is "Y"):

I když skript říká odstranit, nedojde k odstranění virtuální síť. Jenom odebírá integrace. Po ověření, že se jedná co chcete udělat, příkaz je úplně rychlé zpracování a zjistíte, **platí** při se to dělá.

<!--Links-->
[createvpngateway]: http://azure.microsoft.com/documentation/articles/vpn-gateway-point-to-site-create/
[azureportal]: http://portal.azure.com
