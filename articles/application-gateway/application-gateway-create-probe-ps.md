<properties
   pageTitle="Vytvoření vlastní zkušební aplikace brány pomocí prostředí PowerShell správce prostředků | Microsoft Azure"
   description="Naučte se vytvářet vlastní zkušební aplikace brány pomocí prostředí PowerShell ve Správci zdrojů"
   services="application-gateway"
   documentationCenter="na"
   authors="georgewallace"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager"
/>
<tags  
   ms.service="application-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="09/06/2016"
   ms.author="gwallace" />

# <a name="create-a-custom-probe-for-azure-application-gateway-by-using-powershell-for-azure-resource-manager"></a>Vytvoření vlastní zkušební brány aplikace Azure pomocí prostředí PowerShell pro správce prostředků Azure

> [AZURE.SELECTOR]
- [Azure portálu](application-gateway-create-probe-portal.md)
- [Azure prostředí PowerShell správce prostředků](application-gateway-create-probe-ps.md)
- [Azure klasické prostředí PowerShell](application-gateway-create-probe-classic-ps.md)

[AZURE.INCLUDE [azure-probe-intro-include](../../includes/application-gateway-create-probe-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/learn-about-deployment-models-rm-include.md)][klasický nasazení modelu](application-gateway-create-probe-classic-ps.md).

[AZURE.INCLUDE [azure-ps-prerequisites-include.md](../../includes/azure-ps-prerequisites-include.md)]

### <a name="step-1"></a>Krok 1

Použijte AzureRmAccount přihlášení k ověření.

    Login-AzureRmAccount

### <a name="step-2"></a>Krok 2

Zaškrtněte políčko předplatná pro účet.

    Get-AzureRmSubscription

### <a name="step-3"></a>Krok 3

Zvolte, které předplatné Azure používat. <BR>


    Select-AzureRmSubscription -Subscriptionid "GUID of subscription"


### <a name="step-4"></a>Krok 4

Vytvoření skupina zdroje (Přeskočit tento krok při použití existující skupiny zdrojů).

    New-AzureRmResourceGroup -Name appgw-rg -Location "West US"

Azure správce prostředků vyžaduje, aby všechny skupiny prostředků zadejte umístění. Toto umístění slouží jako výchozí umístění pro zdroje v dané skupině zdroje. Ujistěte se, že všechny příkazy k vytvoření brány aplikační použít stejné skupiny prostředků.

Ve výše uvedeném příkladu jsme vytvořili zdroje skupiny nazvané "appgw RG" a "Západní nám" umístění.

## <a name="create-a-virtual-network-and-a-subnet-for-the-application-gateway"></a>Vytvořit virtuální sítě a podsítě brány aplikace

Podle těchto kroků vytvořit virtuální sítě a podsítě brány aplikace.

### <a name="step-1"></a>Krok 1


Přiřaďte 10.0.0.0/24 oblast adresu proměnnou podsítě chcete použít k vytvoření virtuální sítě.

    $subnet = New-AzureRmVirtualNetworkSubnetConfig -Name subnet01 -AddressPrefix 10.0.0.0/24

### <a name="step-2"></a>Krok 2

Vytvořte virtuální sítě s názvem "appgwvnet" v zdrojů skupina "appgw-rg" pro oblastí Západ USA pomocí 10.0.0.0/16 předponu s podsítě 10.0.0.0/24.

    $vnet = New-AzureRmVirtualNetwork -Name appgwvnet -ResourceGroupName appgw-rg -Location "West US" -AddressPrefix 10.0.0.0/16 -Subnet $subnet


### <a name="step-3"></a>Krok 3

Přiřadíte proměnnou podsítě pro další kroky, které vytvářejí brány aplikace.

    $subnet = $vnet.Subnets[0]

## <a name="create-a-public-ip-address-for-the-front-end-configuration"></a>Vytvořit veřejnou IP adresu front-end konfigurace


Vytvoření veřejného zdroje IP "publicIP01" v zdrojů skupina "appgw-rg" pro oblastí Západ USA.

    $publicip = New-AzureRmPublicIpAddress -ResourceGroupName appgw-rg -Name publicIP01 -Location "West US" -AllocationMethod Dynamic


## <a name="create-an-application-gateway-configuration-object-with-a-custom-probe"></a>Vytvoření objektu konfigurace brány aplikace s vlastní zkušební

Nastavit všechny položky konfigurace před vytvořením aplikace brány. Podle těchto kroků vytvořit konfigurační položky, které jsou potřebné pro zdroj brány aplikace.

### <a name="step-1"></a>Krok 1

Vytvoření konfigurace IP brány aplikace s názvem "gatewayIP01". Po spuštění aplikace brány vyzvedne IP adresu z podsítě nakonfigurované a sítě provoz směrovat na IP adresy IP fondu back-end. Mějte na paměti, že pokaždé trvá IP adres.

    $gipconfig = New-AzureRmApplicationGatewayIPConfiguration -Name gatewayIP01 -Subnet $subnet


### <a name="step-2"></a>Krok 2


Konfigurace fondu back-end IP adres s názvem "pool01" k IP adresám "134.170.185.46, 134.170.188.221,134.170.185.50". IP adresy, které obdržíte v síti pocházející z front-end koncovém, jsou tyto hodnoty. Nahraďte výše uvedeného přidat vlastní koncové body aplikace IP adresy IP adres.

    $pool = New-AzureRmApplicationGatewayBackendAddressPool -Name pool01 -BackendIPAddresses 134.170.185.46, 134.170.188.221,134.170.185.50


### <a name="step-3"></a>Krok 3


Vlastní zkušební nakonfigurovaný v tomto kroku.

Parametry použité jsou:

- **Interval** - nakonfiguruje kontroly interval zkušební v sekundách.
- **Časový limit** - definuje zkušební časový limit pro kontrolu HTTP odpověď.
- **-Hostname a cesty** - cestu úplnou adresu URL, která vyvolání aplikace brána stav instance. Například pokud máte web http://contoso.com/, potom vlastní zkušební lze nakonfigurovat pro "http://contoso.com/path/custompath.htm" zkušební kontroly mít úspěšné odpověď HTTP.
- **UnhealthyThreshold** - počet neúspěšných odpovědí HTTP potřebné k označení instance back-end jako *Chybná*.

<BR>

    $probe = New-AzureRmApplicationGatewayProbeConfig -Name probe01 -Protocol Http -HostName "contoso.com" -Path "/path/path.htm" -Interval 30 -Timeout 120 -UnhealthyThreshold 8

### <a name="step-4"></a>Krok 4

Konfigurace nastavení brány aplikace "poolsetting01" pro přenos z fondu back-end. Tento krok má i konfiguraci časový limit, která je určený pro back-end fondu odpověď na žádost brány aplikace. Když odpověď back-end narazí časový limit, zruší aplikace brány žádost. Tato hodnota se liší od zkušební časový limit, který se týká jen back-end odpověď probe kontroly.

    $poolSetting = New-AzureRmApplicationGatewayBackendHttpSettings -Name poolsetting01 -Port 80 -Protocol Http -CookieBasedAffinity Disabled -Probe $probe -RequestTimeout 80

### <a name="step-5"></a>Krok 5

Konfigurace front-end IP port pro veřejné koncovém s názvem "frontendport01".

    $fp = New-AzureRmApplicationGatewayFrontendPort -Name frontendport01 -Port 80

### <a name="step-6"></a>Krok 6

Vytvoření front-end IP konfiguraci s názvem "fipconfig01" a přidružit front-end konfigurace IP veřejnou IP adresu.


    $fipconfig = New-AzureRmApplicationGatewayFrontendIPConfig -Name fipconfig01 -PublicIPAddress $publicip

### <a name="step-7"></a>Krok 7

Vytvořit posluchače název "listener01" a přidružit front-end port front-end IP konfiguraci.

    $listener = New-AzureRmApplicationGatewayHttpListener -Name listener01  -Protocol Http -FrontendIPConfiguration $fipconfig -FrontendPort $fp

### <a name="step-8"></a>Krok 8

Vytvoření pravidla směrování Vyrovnávání zatížení s názvem "rule01", která konfiguruje chování Vyrovnávání zatížení.

    $rule = New-AzureRmApplicationGatewayRequestRoutingRule -Name rule01 -RuleType Basic -BackendHttpSettings $poolSetting -HttpListener $listener -BackendAddressPool $pool

### <a name="step-9"></a>Krok 9

Konfigurace velikosti instance aplikace brány.

    $sku = New-AzureRmApplicationGatewaySku -Name Standard_Small -Tier Standard -Capacity 2


>[AZURE.NOTE]  Výchozí *InstanceCount* hodnotu 2, s maximální hodnotou 10. Výchozí *GatewaySize* hodnotu Střední. Můžete vybrat mezi Standard_Small Standard_Medium a Standard_Large.

## <a name="create-an-application-gateway-by-using-new-azurermapplicationgateway"></a>Vytvoření brány aplikační pomocí nový AzureRmApplicationGateway

Vytvoření brány aplikační s všechny položky konfigurace z výše uvedené kroky. V tomto příkladu brány aplikace jmenuje "appgwtest".

    $appgw = New-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg -Location "West US" -BackendAddressPools $pool -Probes $probe -BackendHttpSettingsCollection $poolSetting -FrontendIpConfigurations $fipconfig  -GatewayIpConfigurations $gipconfig -FrontendPorts $fp -HttpListeners $listener -RequestRoutingRules $rule -Sku $sku

## <a name="add-a-probe-to-an-existing-application-gateway"></a>Přidat zkušební k existující bráně aplikace

Máte čtyři kroky pro přidání vlastních zkušební k existující bráně aplikace.

### <a name="step-1"></a>Krok 1

Načtení prostředku brány aplikace do proměnné prostředí PowerShell pomocí **Get-AzureRmApplicationGateway**.

    $getgw =  Get-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg

### <a name="step-2"></a>Krok 2

Přidejte zkušební existující konfiguraci brány.

    $getgw = Add-AzureRmApplicationGatewayProbeConfig -ApplicationGateway $getgw -Name probe01 -Protocol Http -HostName "contoso.com" -Path "/path/custompath.htm" -Interval 30 -Timeout 120 -UnhealthyThreshold 8


V příkladu vlastní zkušební nakonfigurovaný tak, aby kontrola contoso.com/path/custompath.htm cesta URL každých 30 sekund. Maximální počet 8 selhalo zkušební požadavky nastaven prahovou hodnotu časového limitu 120 sekund.

### <a name="step-3"></a>Krok 3

Přidání zkušební konfigurace nastavení back-end fondu a časového limitu pomocí **-Set-AzureRmApplicationGatewayBackendHttpSettings**.


     $getgw = Set-AzureRmApplicationGatewayBackendHttpSettings -ApplicationGateway $getgw -Name $getgw.BackendHttpSettingsCollection.name -Port 80 -Protocol Http -CookieBasedAffinity Disabled -Probe $probe -RequestTimeout 120

### <a name="step-4"></a>Krok 4

Vám ušetří konfigurace brány pro aplikace **Sady AzureRmApplicationGateway**.

    Set-AzureRmApplicationGateway -ApplicationGateway $getgw

## <a name="remove-a-probe-from-an-existing-application-gateway"></a>Odebrání zkušební z existující aplikace brány

Tady najdete postup, jak odebrat vlastní zkušební z existující aplikace brány.

### <a name="step-1"></a>Krok 1

Načtení prostředku brány aplikace do proměnné prostředí PowerShell pomocí **Get-AzureRmApplicationGateway**.

    $getgw =  Get-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg


### <a name="step-2"></a>Krok 2

Odebrání zkušební konfigurace z aplikace brány pomocí **AzureRmApplicationGatewayProbeConfig odebrat**.

    $getgw = Remove-AzureRmApplicationGatewayProbeConfig -ApplicationGateway $getgw -Name $getgw.Probes.name

### <a name="step-3"></a>Krok 3

Aktualizace nastavení back-end fondu odebrat zkušební a nastavení časového limitu pomocí **-Set-AzureRmApplicationGatewayBackendHttpSettings**.


     $getgw = Set-AzureRmApplicationGatewayBackendHttpSettings -ApplicationGateway $getgw -Name $getgw.BackendHttpSettingsCollection.name -Port 80 -Protocol http -CookieBasedAffinity Disabled

### <a name="step-4"></a>Krok 4

Ušetří konfigurace brány pro aplikace **Sady AzureRmApplicationGateway**. 

    Set-AzureRmApplicationGateway -ApplicationGateway $getgw

## <a name="get-application-gateway-dns-name"></a>Získání aplikace brány DNS jména

Po vytvoření brány dalším krokem je třeba nakonfigurovat front-end pro komunikaci. Při použití veřejnou IP vyžaduje aplikaci brány dynamicky přiřazené názvu DNS, které není vhodné. Zajistěte, aby koncoví uživatelé mohli přístupů brány aplikace záznam CNAME lze tak, aby ukazovaly na veřejné koncový bod brány aplikace. [Konfigurace vlastního názvu domény pro v Azure](../cloud-services/cloud-services-custom-domain-name-portal.md). K tomuto účelu načítejte podrobnosti o bráně aplikace a názvu přidružené IP/DNS pomocí PublicIPAddress prvek připojen k bráně aplikace. Název brány aplikace DNS bude použito k vytvoření záznamu CNAME, který odkazuje dvou webových aplikací pro tento název DNS. Použití záznamy o se nedoporučuje, protože VIP může změnit po restartování aplikace brány.
    
    Get-AzureRmPublicIpAddress -ResourceGroupName appgw-RG -Name publicIP01
        
    Name                     : publicIP01
    ResourceGroupName        : appgw-RG
    Location                 : westus
    Id                       : /subscriptions/<subscription_id>/resourceGroups/appgw-RG/providers/Microsoft.Network/publicIPAddresses/publicIP01
    Etag                     : W/"00000d5b-54ed-4907-bae8-99bd5766d0e5"
    ResourceGuid             : 00000000-0000-0000-0000-000000000000
    ProvisioningState        : Succeeded
    Tags                     : 
    PublicIpAllocationMethod : Dynamic
    IpAddress                : xx.xx.xxx.xx
    PublicIpAddressVersion   : IPv4
    IdleTimeoutInMinutes     : 4
    IpConfiguration          : {
                                 "Id": "/subscriptions/<subscription_id>/resourceGroups/appgw-RG/providers/Microsoft.Network/applicationGateways/appgwtest/frontendIP
                               Configurations/frontend1"
                               }
    DnsSettings              : {
                                 "Fqdn": "00000000-0000-xxxx-xxxx-xxxxxxxxxxxx.cloudapp.net"
                               }

## <a name="next-steps"></a>Další kroky

Informace o konfiguraci SSL odstranění navštivte [Konfigurace převzít SSL](application-gateway-ssl-arm.md)

