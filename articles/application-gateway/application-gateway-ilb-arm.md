<properties
   pageTitle="Vytváření a konfigurace brány aplikační s interní řešení pro vyrovnávání zatížení (ILB) pomocí Správce prostředků Azure | Microsoft Azure"
   description="Tato stránka obsahuje pokyny k vytváření, konfiguraci, spustit a odstranit bránu Azure aplikace s Vyrovnávání zatížení interní (ILB) pro správce prostředků Azure"
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
   ms.date="08/19/2016"
   ms.author="gwallace"/>


# <a name="create-an-application-gateway-with-an-internal-load-balancer-ilb-by-using-azure-resource-manager"></a>Vytvoření brány aplikační s Vyrovnávání zatížení vnitřní (ILB) pomocí Správce prostředků Azure

> [AZURE.SELECTOR]
- [Azure klasický kroky](application-gateway-ilb.md)
- [Postup v Powershellu správce prostředků](application-gateway-ilb-arm.md)

Azure brány aplikace můžete nakonfigurovat pomocí VIP internetového nebo interní koncový bod, nebude vystaven Internetu označovaná taky jako koncový bod vyrovnávání (ILB) interní načíst. Konfigurace brány pomocí ILB je užitečné pro interní řádek podnikových aplikací, které nejsou vystaven na Internetu. Se rovněž osvědčuje při služby a úrovní v rámci vícevrstvé aplikace, které sednout v hranici zabezpečení, který nebude vystaven Internetu, ale přesto vyžadují kruhového načtení rozdělení, lepivost relace nebo ukončení Secure (Sockets Layer SSL).

Tento článek vás provede kroky pro nastavení brány aplikační s ILB.

## <a name="before-you-begin"></a>Než začnete

1. Nainstalujte nejnovější verzi rutiny prostředí PowerShell Azure pomocí webové platformy. Můžete stáhnout a nainstalovat nejnovější verzi z části **Prostředí Windows PowerShell** [ke stažení stránky](https://azure.microsoft.com/downloads/).
2. Vytvořit virtuální sítě a podsítě aplikace brány. Ujistěte se, že bez cloudu nasazení nebo virtuálních počítačích jsou podsítě. Aplikace brány musí být osamoceně v podsítě virtuální sítě.
3. Servery, které můžete konfigurovat použití aplikace brány musí existovat nebo přiřadili jejich koncové body vytvořené v virtuální sítě nebo s veřejnou IP/VIP.

## <a name="what-is-required-to-create-an-application-gateway"></a>Co je nutné k vytvoření brány aplikační?


- **Back-end serveru fondu:** Seznam IP adres servery back-end. IP adresy uvedené by měla buď patřit virtuální síť, ale v různých podsítě brány aplikace nebo by měl být veřejnou IP/VIP.
- **Nastavení fondu back-end serveru:** Každý fondu má nastavení například port Protocol (protokol) a na základě souborů cookie spřažení. Toto nastavení je stejným do fondu a zaevidují do všech serverů v rámci fondu.
- **Front-end portu:** Toto je veřejné port, který je otevřen v bráně aplikace. Přenosy narazí tento port a potom přesměrována k některému z back-end serverů.
- **Posluchače:** Posluchače má front-end port protokol (Http nebo Https Toto jsou malá a velká písmena) a název certifikátu SSL (Pokud konfigurace SSL převzít).
- **Pravidlo:** Pravidlo váže posluchače a fondu serveru back-end a definuje které serveru back-end fondu přenos budou přesměrovány při narazí konkrétní posluchače. V současnosti je podporována pouze *základní* pravidlo. *Základní* pravidla je kruhového zatížení rozdělení.



## <a name="create-an-application-gateway"></a>Vytvoření brány pro aplikace

Rozdíl mezi používáním Azure klasické a správce prostředků Azure je pořadí, ve kterém vytvoříte aplikace brány a položky, které třeba nakonfigurovat.
Pomocí Správce prostředků nakonfigurovat jednotlivě a vložte všechny položky, které usnadňují brány aplikační můžete vytvořit prostředku brány aplikace.


Tady je postup, které jsou potřeba k vytvoření brány aplikace:

1. Vytvoření skupiny zdroje pro správce prostředků
2. Vytvořit virtuální sítě a podsítě brány aplikace
3. Vytvoření objektu konfigurace brány aplikace
4. Vytvoření brány prostředek aplikace


## <a name="create-a-resource-group-for-resource-manager"></a>Vytvoření skupiny zdroje pro správce prostředků

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

Vytvoření nové skupiny prostředků (Přeskočit tento krok při použití existující skupiny zdrojů).

    New-AzureRmResourceGroup -Name appgw-rg -location "West US"

Azure správce prostředků vyžaduje, aby všechny skupiny prostředků zadejte umístění. Se používá jako výchozí umístění pro zdroje v dané skupině zdroje. Ujistěte se, že všechny příkazy k vytvoření brány aplikační používá stejné skupiny prostředků.

Ve výše uvedeném příkladu jsme vytvořili zdroje skupiny nazvané "appgw rg" a "Západní nám" umístění.

## <a name="create-a-virtual-network-and-a-subnet-for-the-application-gateway"></a>Vytvořit virtuální sítě a podsítě brány aplikace

Následující příklad ukazuje, jak vytvořit virtuální sítě pomocí Správce zdrojů:

### <a name="step-1"></a>Krok 1

    $subnetconfig = New-AzureRmVirtualNetworkSubnetConfig -Name subnet01 -AddressPrefix 10.0.0.0/24

To 10.0.0.0/24 oblast adresu přiřadí proměnnou podsítě chcete použít k vytvoření virtuální síť.

### <a name="step-2"></a>Krok 2

    $vnet = New-AzureRmVirtualNetwork -Name appgwvnet -ResourceGroupName appgw-rg -Location "West US" -AddressPrefix 10.0.0.0/16 -Subnet $subnetconfig

Tím vytvoříte virtuální sítě s názvem "appgwvnet" v zdrojů skupina "appgw-rg" pro oblastí Západ USA pomocí 10.0.0.0/16 předponu s podsítě 10.0.0.0/24.

### <a name="step-3"></a>Krok 3

    $subnet = $vnet.subnets[0]

Tento objekt podsítě přiřadí proměnné $subnet k dalším krokům.

## <a name="create-an-application-gateway-configuration-object"></a>Vytvoření objektu konfigurace brány aplikace

### <a name="step-1"></a>Krok 1

    $gipconfig = New-AzureRmApplicationGatewayIPConfiguration -Name gatewayIP01 -Subnet $subnet

Tím vytvoříte konfigurace IP brány aplikace s názvem "gatewayIP01". Po spuštění aplikace brány vyzvedne IP adresu z podsítě nakonfigurované a sítě provoz směrovat na IP adresy IP fondu back-end. Mějte na paměti, že pokaždé trvá IP adres.


### <a name="step-2"></a>Krok 2

    $pool = New-AzureRmApplicationGatewayBackendAddressPool -Name pool01 -BackendIPAddresses 134.170.185.46, 134.170.188.221,134.170.185.50

To nakonfiguruje fondu back-end IP adres s názvem "pool01" k IP adresám "134.170.185.46, 134.170.188.221,134.170.185.50". Zadali jste IP adresy, které přijímají v síti pocházející z front-end koncovém. Nahraďte výše uvedeného přidat vlastní koncové body aplikace IP adresy IP adres.

### <a name="step-3"></a>Krok 3

    $poolSetting = New-AzureRmApplicationGatewayBackendHttpSettings -Name poolsetting01 -Port 80 -Protocol Http -CookieBasedAffinity Disabled

To z fondu back-end nakonfiguruje aplikace brány nastavení "poolsetting01" pro načíst Vyrovnávání zatížení sítě.

### <a name="step-4"></a>Krok 4

    $fp = New-AzureRmApplicationGatewayFrontendPort -Name frontendport01  -Port 80

To nakonfiguruje front-end IP port pro ILB s názvem "frontendport01".

### <a name="step-5"></a>Krok 5

    $fipconfig = New-AzureRmApplicationGatewayFrontendIPConfig -Name fipconfig01 -Subnet $subnet

Vytvoří front-end konfigurace IP s názvem "fipconfig01" a přidruží private IP z aktuální podsítě virtuální sítě.

### <a name="step-6"></a>Krok 6

    $listener = New-AzureRmApplicationGatewayHttpListener -Name listener01  -Protocol Http -FrontendIPConfiguration $fipconfig -FrontendPort $fp

Vytvoří posluchače s názvem "listener01" a přiřadí front-end port front-end IP konfiguraci.

### <a name="step-7"></a>Krok 7

    $rule = New-AzureRmApplicationGatewayRequestRoutingRule -Name rule01 -RuleType Basic -BackendHttpSettings $poolSetting -HttpListener $listener -BackendAddressPool $pool

Tím vytvoříte pravidla směrování Vyrovnávání zatížení s názvem "rule01", která konfiguruje chování Vyrovnávání zatížení.

### <a name="step-8"></a>Krok 8

    $sku = New-AzureRmApplicationGatewaySku -Name Standard_Small -Tier Standard -Capacity 2

To nakonfiguruje velikosti instance aplikace brány.

>[AZURE.NOTE]  Výchozí *InstanceCount* hodnotu 2, s maximální hodnotou 10. Výchozí *GatewaySize* hodnotu Střední. Můžete vybrat mezi Standard_Small Standard_Medium a Standard_Large.

## <a name="create-an-application-gateway-by-using-new-azureapplicationgateway"></a>Vytvoření brány aplikační pomocí nový AzureApplicationGateway

Vytvoří brány aplikační s všechny položky konfigurace z výše uvedené kroky. V tomto příkladu brány aplikace jmenuje "appgwtest".


    $appgw = New-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg -Location "West US" -BackendAddressPools $pool -BackendHttpSettingsCollection $poolSetting -FrontendIpConfigurations $fipconfig  -GatewayIpConfigurations $gipconfig -FrontendPorts $fp -HttpListeners $listener -RequestRoutingRules $rule -Sku $sku

Tím vytvoříte brány aplikační s všechny položky konfigurace z výše uvedené kroky. V příkladu brány aplikace jmenuje "appgwtest".


## <a name="delete-an-application-gateway"></a>Odstranění aplikace brány

Chcete-li odstranit bránu aplikace, musíte postup v pořadí:

1. Ukončení brány získáte pomocí rutiny **Stop AzureRmApplicationGateway** .
2. Chcete-li odebrat brány získáte pomocí rutiny **AzureRmApplicationGateway odebrat** .
3. Ověřte, že byla odebrána brány pomocí rutiny **Get-AzureApplicationGateway** .


### <a name="step-1"></a>Krok 1

Získat objekt brány aplikace a přidružení k proměnné "$getgw".

    $getgw =  Get-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg

### <a name="step-2"></a>Krok 2

Pomocí **Zastavit AzureRmApplicationGateway** ukončení aplikace brány. Tento příklad ukazuje rutinu **Zastavit AzureRmApplicationGateway** na prvním řádku a za ním uveďte výstupu.

    PS C:\> Stop-AzureRmApplicationGateway -ApplicationGateway $getgw  

    VERBOSE: 9:49:34 PM - Begin Operation: Stop-AzureApplicationGateway
    VERBOSE: 10:10:06 PM - Completed Operation: Stop-AzureApplicationGateway
    Name       HTTP Status Code     Operation ID                             Error
    ----       ----------------     ------------                             ----
    Successful OK                   ce6c6c95-77b4-2118-9d65-e29defadffb8

Po přestal stav brány aplikace získáte pomocí rutiny **Odebrat AzureRmApplicationGateway** odebrat službu.


    PS C:\> Remove-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg -Force

    VERBOSE: 10:49:34 PM - Begin Operation: Remove-AzureApplicationGateway
    VERBOSE: 10:50:36 PM - Completed Operation: Remove-AzureApplicationGateway
    Name       HTTP Status Code     Operation ID                             Error
    ----       ----------------     ------------                             ----
    Successful OK                   055f3a96-8681-2094-a304-8d9a11ad8301

>[AZURE.NOTE] **– Vynucení** přepnout mohou sloužit k potlačit potvrzovací zprávy odebrat.


Abyste ověřili, že službu odebrali jsme možnost, můžete použít rutinu **Get-AzureRmApplicationGateway** . Tento krok není povinný.


    PS C:\>Get-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg

    VERBOSE: 10:52:46 PM - Begin Operation: Get-AzureApplicationGateway

    Get-AzureApplicationGateway : ResourceNotFound: The gateway does not exist.
    .....

## <a name="next-steps"></a>Další kroky

Pokud chcete konfigurovat SSL převedení, přečtěte si téma [Konfigurace brány aplikační pro SSL převzít](application-gateway-ssl.md).

Pokud chcete konfigurovat brány aplikace pomocí služby ILB, v tématu [Vytvoření brány aplikační s Vyrovnávání zatížení vnitřní (ILB)](application-gateway-ilb.md).

Pokud budete potřebovat další informace o načtení vyrovnávající možnosti obecně, přečtěte si článek:

- [Vyrovnávání zatížení Azure](https://azure.microsoft.com/documentation/services/load-balancer/)
- [Azure přenosy správce](https://azure.microsoft.com/documentation/services/traffic-manager/)
