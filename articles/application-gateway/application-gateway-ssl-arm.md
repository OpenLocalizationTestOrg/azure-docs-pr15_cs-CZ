<properties
   pageTitle="Konfigurace brány aplikační pro převedení SSL pomocí Správce prostředků Azure | Microsoft Azure"
   description="Tato stránka obsahuje pokyny k vytvoření brány aplikační s SSL převzít pomocí Správce prostředků Azure"
   documentationCenter="na"
   services="application-gateway"
   authors="georgewallace"
   manager="carmonm"
   editor="tysonn"/>
<tags
   ms.service="application-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="09/09/2016"
   ms.author="gwallace"/>

# <a name="configure-an-application-gateway-for-ssl-offload-by-using-azure-resource-manager"></a>Konfigurace brány aplikační pro převedení SSL pomocí Správce prostředků Azure

> [AZURE.SELECTOR]
-[Azure portál](application-gateway-ssl-portal.md)
-[Azure prostředí PowerShell správce prostředků](application-gateway-ssl-arm.md)
-[Azure klasické prostředí PowerShell](application-gateway-ssl.md)

 Ukončit relaci Secure (Sockets Layer SSL) po bránu vyhnout nákladné úkoly dešifrování SSL stát na farmě web je možné konfigurovat Azure aplikace brány. Převedení SSL také zjednodušuje instalační program serveru front-end a správu webové aplikace.

## <a name="before-you-begin"></a>Než začnete

1. Nainstalujte nejnovější verzi rutiny prostředí PowerShell Azure pomocí webové platformy. Můžete stáhnout a nainstalovat nejnovější verzi z části **Prostředí Windows PowerShell** [ke stažení stránky](https://azure.microsoft.com/downloads/).
2. Vytvořit virtuální sítě a podsítě brány aplikace. Ujistěte se, že bez cloudu nasazení nebo virtuálních počítačích jsou podsítě. Aplikace brány musí být osamoceně v podsítě virtuální sítě.
3. Servery konfigurovat pro použití aplikace brány musí existovat nebo přiřadili jejich koncové body vytvořené v virtuální sítě nebo s veřejnou IP/VIP.

## <a name="what-is-required-to-create-an-application-gateway"></a>Co je nutné k vytvoření brány aplikační?


- **Back-end serveru fondu:** Seznam IP adres servery back-end. IP adresy uvedené by měl buď patří do podsítě virtuální sítě nebo by měl být veřejnou IP/VIP.
- **Nastavení fondu back-end serveru:** Každý fondu má nastavení například port Protocol (protokol) a na základě souborů cookie spřažení. Toto nastavení je stejným do fondu a použijí na všechny servery v rámci fondu.
- **Front-end portu:** Toto je veřejné port, který je otevřen v bráně aplikace. Přenosy narazí tento port a potom přesměrována k některému z back-end serverů.
- **Posluchače:** Posluchače má front-end port protokol (Http nebo Https tato nastavení jsou malá a velká písmena) a název certifikátu SSL (Pokud konfigurace SSL převzít).
- **Pravidlo:** Pravidlo váže posluchače a fondu serveru back-end a definuje které serveru back-end fondu přenos budou přesměrovány při narazí konkrétní posluchače. V současnosti je podporována pouze *základní* pravidlo. *Základní* pravidla je kruhového zatížení rozdělení.

**Další konfiguraci poznámek**

Konfigurace certifikáty SSL protokolu **HttpListener** změňte na *Https* (malá a velká písmena). **SslCertificate** element přibude **HttpListener** s hodnotou proměnné nakonfigurovány certifikát SSL. Front-end port musí být aktualizován 443.

**Povolení souborů cookie základě spřažení**: brány aplikační můžete nakonfigurovat tak, aby zajistit, aby se žádost o relace klienta vždy přesměrovávat do stejné OM ve farmě web. Tento scénář vystavila společnost vkládání cookie relace, který umožňuje brány tak, aby směrovaly řádně podporovat. Povolení souborů cookie základě spřažení, nastavte **CookieBasedAffinity** *povoleno* v elementu **BackendHttpSettings** .


## <a name="create-an-application-gateway"></a>Vytvoření brány pro aplikace

Rozdíl mezi používáním Azure klasické nasazení modelu a správce prostředků Azure je pořadí, ve kterém vytvoříte brány aplikační a položky, které třeba nakonfigurovat.

Pomocí Správce prostředků všechny komponenty brány aplikační jsou nakonfigurované jednotlivě a vložte můžete vytvořit prostředek brány aplikace.


Tady jsou kroky potřebné k vytvoření brány aplikace:

1. Vytvoření skupiny zdroje pro správce prostředků
2. Vytvořit virtuální sítě, podsítě a veřejnou IP brány aplikace
3. Vytvoření objektu konfigurace brány aplikace
4. Vytvoření brány prostředek aplikace


## <a name="create-a-resource-group-for-resource-manager"></a>Vytvoření skupina zdroje pro správce prostředků

Ujistěte se, přepínání režimu Powershellu pro používání rutin Správce prostředků Azure. Další informace najdete na [Pomocí Windows Powershellu pomocí Správce prostředků](../powershell-azure-resource-manager.md).

### <a name="step-1"></a>Krok 1

    Login-AzureRmAccount

### <a name="step-2"></a>Krok 2

Zaškrtněte políčko předplatná pro účet.

    Get-AzureRmSubscription

Zobrazí se výzva k ověření pomocí svých přihlašovacích údajů.<BR>

### <a name="step-3"></a>Krok 3

Zvolte, které předplatné Azure používat. <BR>


    Select-AzureRmSubscription -Subscriptionid "GUID of subscription"


### <a name="step-4"></a>Krok 4

Vytvoření skupina zdroje (Přeskočit tento krok při použití existující skupiny zdrojů).

    New-AzureRmResourceGroup -Name appgw-rg -Location "West US"

Azure správce prostředků vyžaduje, aby všechny skupiny prostředků zadejte umístění. Toto nastavení slouží jako výchozí umístění pro zdroje v dané skupině zdroje. Ujistěte se, že všechny příkazy k vytvoření brány aplikační používá stejné skupiny prostředků.

Ve výše uvedeném příkladu jsme vytvořili skupina zdroje s názvem "appgw RG" a "Západní nám" umístění.

## <a name="create-a-virtual-network-and-a-subnet-for-the-application-gateway"></a>Vytvořit virtuální sítě a podsítě brány aplikace

Následující příklad ukazuje, jak vytvořit virtuální sítě pomocí Správce zdrojů:

### <a name="step-1"></a>Krok 1

    $subnet = New-AzureRmVirtualNetworkSubnetConfig -Name subnet01 -AddressPrefix 10.0.0.0/24

V tomto příkladu přiřadí 10.0.0.0/24 oblast adresu proměnnou podsítě chcete použít k vytvoření virtuální sítě.

### <a name="step-2"></a>Krok 2

    $vnet = New-AzureRmVirtualNetwork -Name appgwvnet -ResourceGroupName appgw-rg -Location "West US" -AddressPrefix 10.0.0.0/16 -Subnet $subnet

Tento příklad vytvoří virtuální sítě s názvem "appgwvnet" v zdrojů skupina "appgw-rg" pro oblastí Západ USA pomocí 10.0.0.0/16 předponu s podsítě 10.0.0.0/24.

### <a name="step-3"></a>Krok 3

    $subnet = $vnet.Subnets[0]

V tomto příkladu přiřadí objekt podsítě proměnné $subnet k dalším krokům.

## <a name="create-a-public-ip-address-for-the-front-end-configuration"></a>Vytvořit veřejnou IP adresu front-end konfigurace

    $publicip = New-AzureRmPublicIpAddress -ResourceGroupName appgw-rg -name publicIP01 -location "West US" -AllocationMethod Dynamic

Tento příklad vytvoří veřejné zdroje IP "publicIP01" v zdroje skupiny "appgw-rg" pro oblastí Západ USA.


## <a name="create-an-application-gateway-configuration-object"></a>Vytvoření objektu konfigurace brány aplikace

### <a name="step-1"></a>Krok 1

    $gipconfig = New-AzureRmApplicationGatewayIPConfiguration -Name gatewayIP01 -Subnet $subnet

Tento příklad vytvoří konfigurace IP brány aplikace s názvem "gatewayIP01". Po spuštění aplikace brány vyzvedne IP adresu z podsítě nakonfigurované a sítě provoz směrovat na IP adresy IP fondu back-end. Mějte na paměti, že pokaždé trvá IP adres.

### <a name="step-2"></a>Krok 2

    $pool = New-AzureRmApplicationGatewayBackendAddressPool -Name pool01 -BackendIPAddresses 134.170.185.46, 134.170.188.221,134.170.185.50

V tomto příkladu nakonfiguruje fondu back-end IP adres s názvem "pool01" k IP adresám "134.170.185.46, 134.170.188.221,134.170.185.50." IP adresy, které obdržíte v síti pocházející z front-end koncovém, jsou tyto hodnoty. Nahraďte IP adresy z předchozího příkladu IP adresy koncové body vaší webové aplikace.

### <a name="step-3"></a>Krok 3

    $poolSetting = New-AzureRmApplicationGatewayBackendHttpSettings -Name poolsetting01 -Port 80 -Protocol Http -CookieBasedAffinity Enabled

V tomto příkladu konfiguruje nastavení brány aplikace "poolsetting01" na Vyrovnávání zatížení sítě přenosy ve fondu back-end.

### <a name="step-4"></a>Krok 4

    $fp = New-AzureRmApplicationGatewayFrontendPort -Name frontendport01  -Port 443

V tomto příkladu nakonfiguruje front-end IP port pro veřejné koncovém s názvem "frontendport01".

### <a name="step-5"></a>Krok 5

    $cert = New-AzureRmApplicationGatewaySslCertificate -Name cert01 -CertificateFile <full path for certificate file> -Password ‘<password>’

V tomto příkladu nakonfiguruje certifikátu použitém k připojení SSL. Certifikát musí být ve formátu .pfx a heslo musí být mezi znaky 4 až 12.

### <a name="step-6"></a>Krok 6

    $fipconfig = New-AzureRmApplicationGatewayFrontendIPConfig -Name fipconfig01 -PublicIPAddress $publicip

Tento příklad vytvoří front-end IP konfiguraci s názvem "fipconfig01" a přidruží veřejnou IP adresu front-end konfigurace IP.

### <a name="step-7"></a>Krok 7

    $listener = New-AzureRmApplicationGatewayHttpListener -Name listener01  -Protocol Https -FrontendIPConfiguration $fipconfig -FrontendPort $fp -SslCertificate $cert


Tento příklad vytvoří posluchače název "listener01" a přidruží front-end port front-end konfigurace IP a certifikát.

### <a name="step-8"></a>Krok 8

    $rule = New-AzureRmApplicationGatewayRequestRoutingRule -Name rule01 -RuleType Basic -BackendHttpSettings $poolSetting -HttpListener $listener -BackendAddressPool $pool

Tento příklad vytvoří pravidla směrování Vyrovnávání zatížení s názvem "rule01", která konfiguruje chování Vyrovnávání zatížení.

### <a name="step-9"></a>Krok 9

    $sku = New-AzureRmApplicationGatewaySku -Name Standard_Small -Tier Standard -Capacity 2

V tomto příkladu nakonfiguruje velikosti instance aplikace brány.

>[AZURE.NOTE]  Výchozí *InstanceCount* hodnotu 2, s maximální hodnotou 10. Výchozí *GatewaySize* hodnotu Střední. Můžete vybrat mezi Standard_Small Standard_Medium a Standard_Large.

## <a name="create-an-application-gateway-by-using-new-azureapplicationgateway"></a>Vytvoření brány aplikační pomocí nový AzureApplicationGateway

    $appgw = New-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg -Location "West US" -BackendAddressPools $pool -BackendHttpSettingsCollection $poolSetting -FrontendIpConfigurations $fipconfig  -GatewayIpConfigurations $gipconfig -FrontendPorts $fp -HttpListeners $listener -RequestRoutingRules $rule -Sku $sku -SslCertificates $cert

Tento příklad vytvoří brány aplikační s všechny položky konfigurace z předchozích kroků. V příkladu brány aplikace jmenuje "appgwtest".

## <a name="get-application-gateway-dns-name"></a>Získání aplikace brány DNS jména

Po vytvoření brány dalším krokem je třeba nakonfigurovat front-end pro komunikaci. Při použití veřejnou IP vyžaduje aplikaci brány dynamicky přiřazené názvu DNS, které není vhodné. Zajistěte, aby koncoví uživatelé mohli přístupů brány aplikace záznam CNAME lze tak, aby ukazovaly na veřejné koncový bod brány aplikace. [Konfigurace vlastního názvu domény pro v Azure](../cloud-services/cloud-services-custom-domain-name-portal.md). K tomuto účelu načítejte podrobnosti o bráně aplikace a názvu přidružené IP/DNS pomocí elementu PublicIPAddress připojena k bráně aplikace. Název brány aplikace DNS bude použito k vytvoření záznamu CNAME, který odkazuje dvou webových aplikací pro tento název DNS. Použití záznamy o se nedoporučuje, protože VIP může změnit po restartování aplikace brány.
    
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

Pokud chcete konfigurovat brány aplikace pomocí služby Vyrovnávání zatížení vnitřní (ILB), v tématu [Vytvoření brány aplikační s Vyrovnávání zatížení vnitřní (ILB)](application-gateway-ilb.md).

Pokud budete potřebovat další informace o načtení vyrovnávání možnosti obecně, přečtěte si článek:

- [Vyrovnávání zatížení Azure](https://azure.microsoft.com/documentation/services/load-balancer/)
- [Azure přenosy správce](https://azure.microsoft.com/documentation/services/traffic-manager/)
