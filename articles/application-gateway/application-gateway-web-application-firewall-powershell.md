<properties
   pageTitle="Konfigurace brány Firewall webové aplikace na nové nebo existující aplikace brány | Microsoft Azure"
   description="Tento článek obsahuje informace o tom, jak začít používat bránu firewall webové aplikace na existující nebo nové aplikace bránu."
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
   ms.date="10/25/2016"
   ms.author="gwallace"/>

# <a name="configure-web-application-firewall-on-a-new-or-existing-application-gateway"></a>Konfigurace brány Firewall webové aplikace na nové nebo existující brány aplikace

> [AZURE.SELECTOR]
- [Azure portálu](application-gateway-web-application-firewall-portal.md)
- [Azure prostředí PowerShell správce prostředků](application-gateway-web-application-firewall-powershell.md)

Webové aplikace bránu firewall (WAF) v bráně aplikace Azure chrání před běžné web útoky jako vkládáním příkazu SQL, útoky skriptování webů a hijacks relace webových aplikací.

Azure aplikace brána je Vyrovnávání zatížení vrstvy-7. Poskytuje selhání směrování výkonu požadavků HTTP mezi jiné servery, ať už jde o v cloudu a místní. Aplikace nabízí spoustu z funkcí aplikace doručení řadiče ADC včetně HTTP Vyrovnávání zatížení, na základě souborů cookie relace spřažení, převzít Secure (Sockets Layer SSL), vlastní stavu sond podporu více webů a mnoha dalších. Úplný seznam podporovaných funkcí naleznete přehled brány aplikace

Tento článek ukazuje [Přidat web aplikace firewall k existující bráně aplikace](#add-web-application-firewall-to-an-existing-application-gateway) a [Vytvoření Aplikační brána firewall webové aplikace, která používá](#create-an-application-gateway-with-web-application-firewall).

![scénář obrázek][scenario]

## <a name="waf-configuration-differences"></a>Konfigurace rozdíly WAF

Pokud čtete [Vytvoření aplikace brány pomocí prostředí PowerShell](application-gateway-create-gateway-arm.md)vysvětlení nastavení SKU konfigurujte při vytváření aplikace brány. WAF poskytuje další nastavení můžete definovat při konfiguraci Skladovou na brány aplikace. Existuje žádné další změny provedené ve vlastní bráně aplikace.

**SKU** - brány normální aplikaci bez WAF podporuje **Standardní\_malé**, **Standardní\_střední**, a **Standardní\_velké** velikosti. Úvod WAF existují dva další skladové jednotky **WAF\_střední** a **WAF\_velké**. WAF není podporována ve bran malá aplikace.

**Osy** – **Standardní** nebo **WAF**jsou k dispozici hodnoty. Pokud používáte bránu Firewall webové aplikace, musí být vybrány **WAF** .

**Režim** - toto nastavení je režim WAF. **zjišťování** a **Zabránění**jsou povolené hodnoty. Když WAF nastavení v režimu detekce všechny hrozeb uložené v souboru protokolu. V režimu prevence události pořád přihlášení k lyncu, ale mohl obdrží 403 neoprávněným odpověď z aplikace brány.

## <a name="add-web-application-firewall-to-an-existing-application-gateway"></a>Přidání webových aplikací firewall k existující bráně aplikace

Ujistěte se, že používáte nejnovější verzi Azure Powershellu. Další informace najdete na [Pomocí Windows Powershellu pomocí Správce prostředků](../powershell-azure-resource-manager.md).

### <a name="step-1"></a>Krok 1

Přihlaste se k účtu Azure.

    Login-AzureRmAccount

### <a name="step-2"></a>Krok 2

Vyberte předplatné pro účely tento scénář.

    Select-AzureRmSubscription -SubscriptionName "<Subscription name>"

### <a name="step-3"></a>Krok 3

Načtení brány, který přidáváte Firewall webové aplikace.

    $gw = Get-AzureRmApplicationGateway -Name "AdatumGateway" -ResourceGroupName "MyResourceGroup"


### <a name="step-4"></a>Krok 4

Konfigurace brány firewall sku webové aplikace. Dostupné formáty **WAF\_velké** a **WAF\_střední**. Při použití webových aplikací firewall úrovně musí být **WAF**, kapacita musí potvrdit při nastavování sku.

    $gw | Set-AzureRmApplicationGatewaySku -Name WAF_Large -Tier WAF -Capacity 2

### <a name="step-5"></a>Krok 5

Konfigurace nastavení WAF definované v následujícím příkladu:

Nastavení **WafMode** dostupné hodnoty se zabránění duplicit.

    $gw | Set-AzureRmApplicationGatewayWebApplicationFirewallConfiguration -Enabled $true -FirewallMode Prevention

### <a name="step-6"></a>Krok 6

Aktualizujte bránu aplikace s nastaveními v předchozím kroku.

    Set-AzureRmApplicationGateway -ApplicationGateway $gw

Tento příkaz aktualizuje aplikace brány Firewall webové aplikace. Doporučujeme zobrazíte [Aplikace brány Diagnostika](application-gateway-diagnostics.md) pochopit, jak zobrazit protokoly aplikace brány. Vzhledem k zabezpečení druh WAF protokoly třeba, aby pravidelně pochopit postoje zabezpečení webových aplikací.

## <a name="create-an-application-gateway-with-web-application-firewall"></a>Vytvoření aplikace brány firewall webové aplikace

Podle těchto kroků můžete přejít celý procesem od začátku do konce pro vytváření brány aplikací pomocí webové aplikace Firewall.

Ujistěte se, že používáte nejnovější verzi Azure Powershellu. Další informace najdete na [Pomocí Windows Powershellu pomocí Správce prostředků](../powershell-azure-resource-manager.md).

### <a name="step-1"></a>Krok 1

Přihlaste se k Azure

    Login-AzureRmAccount

Zobrazí se výzva k ověření pomocí svých přihlašovacích údajů.

### <a name="step-2"></a>Krok 2

Zaškrtněte políčko předplatná pro účet.

    Get-AzureRmSubscription

### <a name="step-3"></a>Krok 3

Zvolte, které předplatné Azure používat.

    Select-AzureRmsubscription -SubscriptionName "<Subscription name>"

### <a name="step-4"></a>Krok 4

Vytvoření skupina zdroje (Přeskočit tento krok při použití existující skupiny zdrojů).

    New-AzureRmResourceGroup -Name appgw-rg -Location "West US"

Azure správce prostředků vyžaduje, aby všechny skupiny prostředků zadejte umístění. Toto umístění slouží jako výchozí umístění pro zdroje v dané skupině zdroje. Ujistěte se, že všechny příkazy k vytvoření brány aplikační používá stejné skupiny prostředků.

V tomto příkladu jsme vytvořili zdroje skupiny nazvané "appgw RG" a "Západní cz" umístění.

>[AZURE.NOTE] Pokud potřebujete konfigurovat vlastní zkušební aplikace brány, v tématu [Vytvoření brány aplikační s vlastní sond pomocí prostředí PowerShell](application-gateway-create-probe-ps.md). Podívejte se na [vlastní sond a sledování stavu](application-gateway-probe-overview.md) Další informace.

### <a name="step-5"></a>Krok 5

Přiřadíte rozsah adres podsítě bude použito brány aplikace samotné.

    $gwSubnet = New-AzureRmVirtualNetworkSubnetConfig -Name 'appgwsubnet' -AddressPrefix 10.0.0.0/24

> [AZURE.NOTE] Podsítě aplikace by měl mít aspoň 28 posunuto masky. Tato hodnota ponechá 10 dostupných adres ve podsítě instancí bran aplikace. Menší podsítě nebudete moci přidat další instanci aplikace brány v budoucnu.

### <a name="step-6"></a>Krok 6

Přiřadíte rozsah adres pro použití fondu adres back-end.

    $nicSubnet = New-AzureRmVirtualNetworkSubnetConfig  -Name 'appsubnet' -AddressPrefix 10.0.2.0/24

### <a name="step-7"></a>Krok 7

Vytvořit virtuální sítě s předchozím podsítí ve skupině prostředků vytvořili v kroku: [Vytvoření skupiny zdrojů](#create-the-resource-group)

    $vnet = New-AzureRmvirtualNetwork -Name 'appgwvnet' -ResourceGroupName appgw-rg -Location "West US" -AddressPrefix 10.0.0.0/16 -Subnet $gwSubnet, $nicSubnet

### <a name="step-8"></a>Krok 8

Načtení zdroje virtuální sítě a prostředků podsítě se nemusí používat v následujících kroků:

    $vnet = Get-AzureRmvirtualNetwork -Name 'appgwvnet' -ResourceGroupName appgw-rg
    $gwSubnet = Get-AzureRmVirtualNetworkSubnetConfig -Name 'appgwsubnet' -VirtualNetwork $vnet
    $nicSubnet = Get-AzureRmVirtualNetworkSubnetConfig -Name 'appsubnet' -VirtualNetwork $vnet

### <a name="step-9"></a>Krok 9

Vytvořte veřejné zdroje IP použije aplikace brány. Používá se tento veřejnou IP adresu v některém z následujících kroků:

    $publicip = New-AzureRmPublicIpAddress -ResourceGroupName appgw-rg -name 'appgwpip' -Location "West US" -AllocationMethod Dynamic

> [AZURE.IMPORTANT] Aplikace brány neposkytuje podporu veřejnou IP adresu vytvořili s popiskem domény definované. Je podporována pouze veřejnou IP adresu s popiskem dynamicky vytvořený domény. Pokud požadujete popisný dns název brány aplikace, doporučujeme použít záznam cname jako alias.

### <a name="step-10"></a>Krok 10

Všechny položky konfigurace musíte nastavit před vytvořením aplikace brány. Podle těchto kroků vytvořit konfigurační položky, které jsou potřebné pro zdroj brány aplikace.

Vytvořit konfiguraci brány IP aplikací, to nakonfiguruje jaké podsítě používá bránu aplikace. Po spuštění aplikace brány vyzvedne IP adresu z podsítě nakonfigurované a přesměrovává přenosy sítě IP adresy ve fondu IP back-end. Mějte na paměti, že pokaždé trvá IP adres.

    $gipconfig = New-AzureRmApplicationGatewayIPConfiguration -Name 'gwconfig' -Subnet $gwSubnet

### <a name="step-11"></a>Krok 11

Nakonfigurujte fondu back-end IP adres IP adresy back-end webových serverů. Nejsou tyto IP adresy IP adresy, které přijímají v síti pocházející z front-end koncovém. Nahraďte následující IP adresy přidat vlastní koncové body aplikace IP adres.

    $pool = New-AzureRmApplicationGatewayBackendAddressPool -Name 'pool01' -BackendIPAddresses 1.1.1.1, 2.2.2.2, 3.3.3.3

### <a name="step-12"></a>Krok 12

Odeslání certifikát, který má být použit na protokol ssl povoleno back-end fond zdrojů.

    $authcert = New-AzureRmApplicationGatewayAuthenticationCertificate -Name 'whitelistcert1' -CertificateFile <full path to .cer file>

### <a name="step-13"></a>Krok 13

Nastavení aplikace brány back-end http. Přiřadíte certifikát uložit v předchozím kroku, nastavení protokolu http.

    $poolSetting = New-AzureRmApplicationGatewayBackendHttpSettings -Name 'setting01' -Port 443 -Protocol Https -CookieBasedAffinity Enabled -AuthenticationCertificates $authcert

### <a name="step-14"></a>Krok 14

Konfigurace front-end IP port pro veřejné koncovém. Toto je port, že koncoví uživatelé mohli připojit k.

    $fp = New-AzureRmApplicationGatewayFrontendPort -Name 'port01'  -Port 443

### <a name="step-15"></a>Krok 15

Vytvoření front-end konfigurace IP, toto nastavení odpovídá soukromou nebo veřejnou ip adresu front-end brány aplikace. Podle následujících pokynů přidružuje front-end konfigurace IP veřejnou IP adresu v předchozím kroku.

    $fipconfig = New-AzureRmApplicationGatewayFrontendIPConfig -Name 'fip01' -PublicIPAddress $publicip

### <a name="step-16"></a>Krok 16

Konfigurace certifikátu brány aplikace. Certifikát se používá při dešifrování a znovu šifrovat komunikaci na brány aplikace.

    $cert = New-AzureRmApplicationGatewaySslCertificate -Name cert01 -CertificateFile <full path to .pfx file> -Password <password for certificate file>

### <a name="step-17"></a>Krok 17

Vytvořte posluchače HTTP brány aplikace. Přiřadíte front-end ip konfigurace, port a protokol ssl certifikát, který chcete použít.

    $listener = New-AzureRmApplicationGatewayHttpListener -Name listener01 -Protocol Https -FrontendIPConfiguration $fipconfig -FrontendPort $fp -SslCertificate $cert

### <a name="step-18"></a>Krok 18

Vytvoření zatížení vyrovnávání směrování pravidlo, které nakonfiguruje chování Vyrovnávání zatížení. V tomto příkladu je vytvořeno kruhového základní pravidlo.

    $rule = New-AzureRmApplicationGatewayRequestRoutingRule -Name 'rule01' -RuleType basic -BackendHttpSettings $poolSetting -HttpListener $listener -BackendAddressPool $pool
   
### <a name="step-19"></a>Krok 19

Konfigurace velikosti instance aplikace brány.

    $sku = New-AzureRmApplicationGatewaySku -Name WAF_Medium -Tier WAF -Capacity 2

>[AZURE.NOTE]  Můžete vybrat mezi **WAF\_střední** a **WAF\_velké**, osy po použití WAF vždy **WAF**. Kapacita je jiné číslo 1 až 10.

### <a name="step-20"></a>Krok 20

Konfigurace režimu pro WAF, povolené hodnoty jsou **prevence** a **zjišťování**.

    $config = New-AzureRmApplicationGatewayWafConfig -Enabled $true -WafMode "Prevention"

### <a name="step-21"></a>Krok 21

Vytvoření brány aplikační s všechny položky konfigurace z předchozích kroků. V tomto příkladu brány aplikace jmenuje "appgwtest".

    $appgw = New-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg -Location "West US" -BackendAddressPools $pool -BackendHttpSettingsCollection $poolSetting -FrontendIpConfigurations $fipconfig  -GatewayIpConfigurations $gipconfig -FrontendPorts $fp -HttpListeners $listener -RequestRoutingRules $rule -Sku $sku -WebApplicationFirewallConfig $config -SslCertificates $cert -AuthenticationCertificates $authcert

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

Naučíte se konfigurovat protokolování diagnostiky protokolování událostí, které jsou nerozpoznal nebo zabránit pomocí webové aplikace Firewall navštivte [Diagnostiky brány aplikace](application-gateway-diagnostics.md)


[scenario]: ./media/application-gateway-web-application-firewall-powershell/scenario.png