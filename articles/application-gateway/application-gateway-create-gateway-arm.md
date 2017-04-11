<properties
   pageTitle="Vytvoření, spuštění nebo odstranění aplikace brány pomocí Správce prostředků Azure | Microsoft Azure"
   description="Tato stránka obsahuje pokyny k vytváření, konfiguraci, spustit a odstranění Azure aplikace brány pomocí Správce prostředků Azure"
   documentationCenter="na"
   services="application-gateway"
   authors="georgewallace"
   manager="carmonm"
   editor="tysonn"/>
<tags
   ms.service="application-gateway"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/25/2016"
   ms.author="gwallace"/>


# <a name="create-start-or-delete-an-application-gateway-by-using-azure-resource-manager"></a>Vytvoření, spuštění nebo odstranění aplikace brány pomocí Správce prostředků Azure

> [AZURE.SELECTOR]
- [Azure portálu](application-gateway-create-gateway-portal.md)
- [Azure prostředí PowerShell správce prostředků](application-gateway-create-gateway-arm.md)
- [Azure klasické prostředí PowerShell](application-gateway-create-gateway.md)
- [Azure správce prostředků šablony](application-gateway-create-gateway-arm-template.md)
- [Azure rozhraní příkazového řádku](application-gateway-create-gateway-cli.md)

Azure aplikace brána je Vyrovnávání zatížení vrstvy-7. Poskytuje selhání směrování výkonu požadavků HTTP mezi jiné servery, ať už jde o v cloudu a místní. Aplikace brány nabízí spoustu z funkcí aplikace doručení řadiče ADC včetně Vyrovnávání zatížení HTTP, na základě souborů cookie relace spřažení, převzít Secure (Sockets Layer SSL), vlastní stavu sond podporu více webů a mnoha dalších. Úplný seznam podporovaných funkcí naleznete [Přehled brány aplikace](application-gateway-introduction.md)

Tento článek vás provede kroky vytvoření, konfigurace, spustit a odstraňovat brány aplikace.

>[AZURE.IMPORTANT] Než začnete pracovat s Azure zdrojů, je důležité pochopit Azure má nyní dvě modelů nasazení: Správce zdrojů a klasické. Ujistěte se, než začnete pracovat s Azure zdroje porozumět [modelů nasazení a nástroje](../azure-classic-rm.md) . V dokumentaci k různých nástrojů zobrazíte kliknutím na karet v horní části tohoto článku. Tento dokument obsahuje, vytvoření aplikace brány pomocí Správce prostředků Azure. Použití klasická verze, přejděte na téma [vytváření klasické nasazení aplikace brány pomocí prostředí PowerShell](application-gateway-create-gateway.md).


## <a name="before-you-begin"></a>Než začnete

1. Nainstalujte nejnovější verzi rutiny prostředí PowerShell Azure pomocí webové platformy. Můžete stáhnout a nainstalovat nejnovější verzi z části **Prostředí Windows PowerShell** [ke stažení stránky](https://azure.microsoft.com/downloads/).
2. Pokud máte existující virtuální sítě, vyberte existující podsítě prázdné nebo vytvoříte podsítě v síti existující virtuální pouze pro použití bráně aplikace. Nelze nasadit brány aplikace k jiné síti virtuální než prostředky, které chcete nasadit za brány aplikace.
3. Servery, které můžete konfigurovat použití aplikace brány musí existovat nebo přiřadili jejich koncové body vytvořené v virtuální sítě nebo s veřejnou IP/VIP.

## <a name="what-is-required-to-create-an-application-gateway"></a>Co je nutné k vytvoření brány aplikační?

- **Back-end serveru fondu:** Seznam IP adres servery back-end. IP adresy uvedený by měl buď patří do podsítě virtuální sítě nebo by měl být veřejnou IP/VIP.
- **Nastavení fondu back-end serveru:** Každý fondu má nastavení například port Protocol (protokol) a na základě souborů cookie spřažení. Toto nastavení je stejným do fondu a zaevidují do všech serverů v rámci fondu.
- **Front-end portu:** Toto je veřejné port, který je otevřen ve bráně aplikace. Přenosy narazí tento port a potom přesměrována k některému z back-end serverů.
- **Posluchače:** Posluchače má front-end port protokol (Http nebo Https tyto hodnoty jsou malá a velká písmena) a název certifikátu SSL (Pokud konfigurace SSL převzít).
- **Pravidlo:** Pravidlo váže posluchače fondu serveru back-end a definuje které serveru back-end fondu přenos budou přesměrovány při narazí konkrétní posluchače.

## <a name="create-an-application-gateway"></a>Vytvoření brány pro aplikace

Rozdíl mezi používáním Azure klasické a správce prostředků Azure je pořadí, ve kterém vytvoříte aplikace brány a položky, které třeba nakonfigurovat.

Pomocí Správce prostředků všechny položky, které usnadňují brány aplikační jsou nakonfigurované jednotlivě a vložte můžete vytvořit prostředku brány aplikace.

Následují kroky, které jsou potřeba k vytvoření brány aplikace.

## <a name="create-a-resource-group-for-resource-manager"></a>Vytvoření skupiny zdroje pro správce prostředků

Ujistěte se, že používáte nejnovější verzi Azure Powershellu. Další informace najdete na [Pomocí Windows Powershellu pomocí Správce prostředků](../powershell-azure-resource-manager.md).

### <a name="step-1"></a>Krok 1

Přihlaste se k Azure

    Login-AzureRmAccount

Zobrazí se výzva k ověření pomocí svých přihlašovacích údajů.

### <a name="step-2"></a>Krok 2

Zkontrolujte předplatná pro účet.

    Get-AzureRmSubscription

### <a name="step-3"></a>Krok 3

Zvolte, které předplatné Azure používat.

    Select-AzureRmSubscription -Subscriptionid "GUID of subscription"

### <a name="step-4"></a>Krok 4

Vytvoření skupina zdroje (Přeskočit tento krok při použití existující skupiny zdrojů).

    New-AzureRmResourceGroup -Name appgw-rg -Location "West US"

Azure správce prostředků vyžaduje, aby všechny skupiny prostředků zadejte umístění. Toto umístění slouží jako výchozí umístění pro zdroje v dané skupině zdroje. Ujistěte se, že všechny příkazy k vytvoření brány aplikační používá stejné skupiny prostředků.

Ve výše uvedeném příkladu jsme vytvořili zdroje skupiny nazvané "appgw RG" a "Západní nám" umístění.

>[AZURE.NOTE] Pokud potřebujete konfigurovat vlastní zkušební aplikace brány, v tématu [Vytvoření brány aplikace s vlastní sond pomocí prostředí PowerShell](application-gateway-create-probe-ps.md). Podívejte se na [vlastní sond a sledování stavu](application-gateway-probe-overview.md) Další informace.

## <a name="create-a-virtual-network-and-a-subnet-for-the-application-gateway"></a>Vytvořit virtuální síť a podsítě brány aplikace

Následující příklad ukazuje, jak vytvořit virtuální síť pomocí Správce prostředků.

### <a name="step-1"></a>Krok 1

Přiřaďte 10.0.0.0/24 oblast adresu podsítě proměnnou použít k vytvoření virtuální síť.

    $subnet = New-AzureRmVirtualNetworkSubnetConfig -Name subnet01 -AddressPrefix 10.0.0.0/24

### <a name="step-2"></a>Krok 2

Vytvořte virtuální sítě s názvem "appgwvnet" v zdrojů skupina "appgw-rg" pro oblastí Západ USA pomocí 10.0.0.0/16 předponu s podsítě 10.0.0.0/24.

    $vnet = New-AzureRmVirtualNetwork -Name appgwvnet -ResourceGroupName appgw-rg -Location "West US" -AddressPrefix 10.0.0.0/16 -Subnet $subnet

### <a name="step-3"></a>Krok 3

Přiřadíte proměnnou podsítě pro další kroky, které vytvářejí brány aplikace.

    $subnet=$vnet.Subnets[0]

## <a name="create-a-public-ip-address-for-the-front-end-configuration"></a>Vytvořit veřejnou IP adresu front-end konfigurace

Vytvoření veřejného zdroje IP "publicIP01" v zdrojů skupina "appgw-rg" pro oblastí Západ USA.

    $publicip = New-AzureRmPublicIpAddress -ResourceGroupName appgw-rg -name publicIP01 -location "West US" -AllocationMethod Dynamic


## <a name="create-an-application-gateway-configuration-object"></a>Vytvoření objektu konfigurace bráně aplikace

Všechny položky konfigurace musíte nastavit před vytvořením aplikace brány. Podle těchto kroků vytvořit konfigurační položky, které jsou potřebné pro zdroj brány aplikace.

### <a name="step-1"></a>Krok 1

Vytvoření konfigurace IP brány aplikace s názvem "gatewayIP01". Po spuštění aplikace brány vyzvedne IP adresu z podsítě nakonfigurované a přesměrovává přenosy sítě k IP adrese z fondu IP back-end. Mějte na paměti, že pokaždé trvá IP adres.

    $gipconfig = New-AzureRmApplicationGatewayIPConfiguration -Name gatewayIP01 -Subnet $subnet

### <a name="step-2"></a>Krok 2

Konfigurace fondu back-end IP adres s názvem "pool01" k IP adresám "134.170.185.46, 134.170.188.221,134.170.185.50". Tyto adresy IP jsou IP adresy, které přijímají síťová komunikace, pochází z front-end koncovém. Nahraďte předchozí IP adresy přidat vlastní koncové body aplikace IP adres.

    $pool = New-AzureRmApplicationGatewayBackendAddressPool -Name pool01 -BackendIPAddresses 134.170.185.46, 134.170.188.221,134.170.185.50

### <a name="step-3"></a>Krok 3

Konfigurace nastavení brány aplikace "poolsetting01" pro vyrovnávání zatížení sítě přenosy ve fondu back-end.

    $poolSetting = New-AzureRmApplicationGatewayBackendHttpSettings -Name poolsetting01 -Port 80 -Protocol Http -CookieBasedAffinity Disabled

### <a name="step-4"></a>Krok 4

Konfigurace front-end port IP s názvem "frontendport01" pro veřejnou IP koncový bod.

    $fp = New-AzureRmApplicationGatewayFrontendPort -Name frontendport01  -Port 80

### <a name="step-5"></a>Krok 5

Vytvoření front-end IP konfiguraci s názvem "fipconfig01" a přidružit front-end konfigurace IP veřejnou IP adresu.

    $fipconfig = New-AzureRmApplicationGatewayFrontendIPConfig -Name fipconfig01 -PublicIPAddress $publicip

### <a name="step-6"></a>Krok 6

Vytvořit posluchače název "listener01" a přidružit front-end port front-end IP konfiguraci.

    $listener = New-AzureRmApplicationGatewayHttpListener -Name listener01  -Protocol Http -FrontendIPConfiguration $fipconfig -FrontendPort $fp

### <a name="step-7"></a>Krok 7

Vytvoření pravidla směrování Vyrovnávání zatížení s názvem "rule01", která konfiguruje chování Vyrovnávání zatížení.

    $rule = New-AzureRmApplicationGatewayRequestRoutingRule -Name rule01 -RuleType Basic -BackendHttpSettings $poolSetting -HttpListener $listener -BackendAddressPool $pool

### <a name="step-8"></a>Krok 8

Konfigurace velikosti instance aplikace brány.

    $sku = New-AzureRmApplicationGatewaySku -Name Standard_Small -Tier Standard -Capacity 2

>[AZURE.NOTE]  Výchozí *InstanceCount* hodnotu 2, s maximální hodnotou 10. Výchozí *GatewaySize* hodnotu Střední. Můžete vybrat mezi Standard_Small Standard_Medium a Standard_Large.

## <a name="create-an-application-gateway-by-using-new-azurermapplicationgateway"></a>Vytvoření brány aplikační pomocí nový AzureRmApplicationGateway

Vytvoření brány aplikační s všechny položky konfigurace z předchozích kroků. V tomto příkladu brány aplikace jmenuje "appgwtest".

    $appgw = New-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg -Location "West US" -BackendAddressPools $pool -BackendHttpSettingsCollection $poolSetting -FrontendIpConfigurations $fipconfig  -GatewayIpConfigurations $gipconfig -FrontendPorts $fp -HttpListeners $listener -RequestRoutingRules $rule -Sku $sku

### <a name="step-9"></a>Krok 9

Načtení DNS a VIP podrobnosti o bráně aplikace z veřejné zdroje IP připojené k bráně aplikace.

    Get-AzureRmPublicIpAddress -Name publicIP01 -ResourceGroupName appgw-rg  

## <a name="delete-an-application-gateway"></a>Odstranění aplikace brány

Chcete-li odstranit bránu aplikace, postupujte takto:

### <a name="step-1"></a>Krok 1

Získat objekt brány aplikace a přidružení k proměnné "$getgw".

    $getgw = Get-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg

### <a name="step-2"></a>Krok 2

Pomocí **Zastavit AzureRmApplicationGateway** ukončení aplikace brány.

    Stop-AzureRmApplicationGateway -ApplicationGateway $getgw  

Po přestal stav brány aplikace získáte pomocí rutiny **Odebrat AzureRmApplicationGateway** odebrat službu.

    Remove-AzureRmApplicationGateway -Name $appgwtest -ResourceGroupName appgw-rg -Force

>[AZURE.NOTE] **– Vynucení** přepnout mohou sloužit k potlačit potvrzovací zprávy odebrat.

Pokud chcete ověřit, že službu odebrali jsme možnost, můžete použít rutinu **Get-AzureRmApplicationGateway** . Tento krok není povinný.

    Get-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg

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

Pokud chcete konfigurovat SSL převedení, přečtěte si téma [Konfigurace bráně aplikace pro SSL převzít](application-gateway-ssl.md).

Pokud chcete konfigurovat brány aplikace pomocí služby Vyrovnávání interní zatížení, v tématu [Vytvoření brány aplikační s interní řešení pro vyrovnávání zatížení (ILB)](application-gateway-ilb.md).

Pokud budete potřebovat další informace o načtení vyrovnávání možnosti obecně, přečtěte si článek:

- [Vyrovnávání zatížení Azure](https://azure.microsoft.com/documentation/services/load-balancer/)
- [Azure přenosy správce](https://azure.microsoft.com/documentation/services/traffic-manager/)
