<properties
    pageTitle="Konfigurace zásad SSL a konce SSL aplikace brána | Microsoft Azure"
    description="Tento článek popisuje, jak nakonfigurovat aplikaci brány pomocí Správce prostředků Powershellu Azure konce SSL"
    services="application-gateway"
    documentationCenter="na"
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

# <a name="configure-ssl-policy-and-end-to-end-ssl-with-application-gateway-using-powershell"></a>Konfigurace zásad SSL a konce SSL s aplikací brány pomocí prostředí PowerShell

## <a name="overview"></a>Základní informace

Brána pro aplikace podporuje šifrování konce provoz. Aplikace brány k tomu ukončení připojení SSL v bráně aplikace. Brána pak se týká pravidla směrování přenosu, znovu zašifrování paketu a předá paket odpovídající back-end podle pravidla směrování definované. Odpovědi z webového serveru prochází stejný postup zpátky na koncového uživatele.

Jiné funkce, které bráně aplikace podporuje zakázání některých verzí protokol SSL. Aplikace brány podporuje zakázání následující verzi protokolu; TLSv1.0 TLSv1.1 a TLSv1.2.

> [AZURE.NOTE] Protokol SSL 2.0 SSL 3.0 ve výchozím nastavení jsou zakázaná a nelze povolit. Jsou považovány za nezabezpečenou a ostatní vás mohou být použity s aplikací brány

![scénář obrázek][scenario]

## <a name="scenario"></a>Scénář

V tomto scénáři je Naučte se vytvářet aplikace brány pomocí konce SSL pomocí Powershellu.

Tento scénář udělejte toto:

- Vytvoření skupiny zdroje s názvem "appgw rg"
- Vytvořte virtuální sítě s názvem "appgwvnet" s rezervovaná CIDR blokem 10.0.0.0/16.
- Vytvořte dva podsítí s názvem "appgwsubnet" a "appsubnet".
- Vytvoření brány malá aplikace podporuje šifrování SSL konce, který zakáže určitých protokoly SSL.

## <a name="before-you-begin"></a>Než začnete

Konfigurace konce SSL s brány aplikační, je nutný pro brány certifikát a certifikáty jsou potřeba pro servery back-end. Certifikát brány se používá k šifrování a dešifrování přenosů do ní protokolem SSL. Certifikát brány musí být ve formátu osobní informace Exchange (pfx). V tomto formátu umožňuje privátním klíčem k exportu umožňující brána aplikace slouží k šifrování a dešifrování provoz.

Pro šifrování ssl konce back-end musí být povolené s aplikací brány. Důvodem je tím, že odešlete veřejné certifikátu back-end k bráně aplikace. Zajistíte tím, že bráně aplikace pouze informuje uživatele o s známé back-end instance. Tato další zabezpečuje komunikace konce.

Tento postup je popsán v následujících krocích:

## <a name="create-the-resource-group"></a>Vytvoření skupiny zdrojů

V této části vás provede vytváření skupiny prostředků, obsahující brány aplikace.

### <a name="step-1"></a>Krok 1

Přihlaste se k účtu Azure.

    Login-AzureRmAccount

### <a name="step-2"></a>Krok 2

Vyberte předplatné pro účely tento scénář.

    Select-AzureRmsubscription -SubscriptionName "<Subscription name>"

### <a name="step-3"></a>Krok 3

Vytvoření skupina zdroje (Přeskočit tento krok při použití existující skupiny zdrojů).

    New-AzureRmResourceGroup -Name appgw-rg -Location "West US"

## <a name="create-a-virtual-network-and-a-subnet-for-the-application-gateway"></a>Vytvořit virtuální sítě a podsítě brány aplikace

Následující příklad vytvoří virtuální sítě a dvě podsítí. Jeden podsítě se používá k ukládání brány aplikace. Další podsítě se používá k back-end hostingu webové aplikace.

### <a name="step-1"></a>Krok 1

Přiřadíte rozsah adres podsítě bude použito brány aplikace samotné.

    $gwSubnet = New-AzureRmVirtualNetworkSubnetConfig -Name 'appgwsubnet' -AddressPrefix 10.0.0.0/24

> [AZURE.NOTE] Podsítě konfigurované aplikace brány mají správnou velikost. Aplikační brána je možné konfigurovat pro až 10 instance. Jednotlivé instance trvá 1 IP adresu z podsítě. Moc malé podsítě mohou nepříznivě ovlivnit rozšiřování brány aplikace.

### <a name="step-2"></a>Krok 2

Přiřadíte rozsah adres pro použití fondu adres back-end.

    $nicSubnet = New-AzureRmVirtualNetworkSubnetConfig  -Name 'appsubnet' -AddressPrefix 10.0.2.0/24

### <a name="step-3"></a>Krok 3

Vytvořte virtuální sítě s podsítí podle předchozích kroků.

    $vnet = New-AzureRmvirtualNetwork -Name 'appgwvnet' -ResourceGroupName appgw-rg -Location "West US" -AddressPrefix 10.0.0.0/16 -Subnet $gwSubnet, $nicSubnet

### <a name="step-4"></a>Krok 4

Načtení zdroje virtuální sítě a prostředků podsítě se nemusí používat v následujících kroků:

    $vnet = Get-AzureRmvirtualNetwork -Name 'appgwvnet' -ResourceGroupName appgw-rg
    $gwSubnet = Get-AzureRmVirtualNetworkSubnetConfig -Name 'appgwsubnet' -VirtualNetwork $vnet
    $nicSubnet = Get-AzureRmVirtualNetworkSubnetConfig -Name 'appsubnet' -VirtualNetwork $vnet

## <a name="create-a-public-ip-address-for-the-front-end-configuration"></a>Vytvořit veřejnou IP adresu front-end konfigurace

Vytvořte veřejné zdroje IP použije aplikace brány. Tento veřejnou IP adresu slouží následující kroku.

    $publicip = New-AzureRmPublicIpAddress -ResourceGroupName appgw-rg -Name 'publicIP01' -Location "West US" -AllocationMethod Dynamic

> [AZURE.IMPORTANT] Aplikace brány neposkytuje podporu veřejnou IP adresu vytvořili s popiskem domény definované. Je podporována pouze veřejnou IP adresu s popiskem dynamicky vytvořený domény. Pokud požadujete popisný dns název brány aplikace, doporučujeme použít záznam cname jako alias.

## <a name="create-an-application-gateway-configuration-object"></a>Vytvoření objektu konfigurace brány aplikace

Všechny položky konfigurace musíte nastavit před vytvořením aplikace brány. Podle těchto kroků vytvořit konfigurační položky, které jsou potřebné pro zdroj brány aplikace.

### <a name="step-1"></a>Krok 1

Vytvoření konfiguraci aplikace brány IP, toto nastavení konfiguruje jaké podsítě používá brány aplikace. Po spuštění aplikace brány vyzvedne IP adresu z podsítě nakonfigurované a přesměrovává přenosy sítě k IP adrese z fondu IP back-end. Mějte na paměti, že pokaždé přenese IP adres.

    $gipconfig = New-AzureRmApplicationGatewayIPConfiguration -Name 'gwconfig' -Subnet $gwSubnet

### <a name="step-2"></a>Krok 2

Vytvoření front-end konfigurace IP, toto nastavení odpovídá soukromou nebo veřejnou ip adresu front-end brány aplikace. Podle následujících pokynů přidružuje front-end konfigurace IP veřejnou IP adresu v předchozím kroku.

    $fipconfig = New-AzureRmApplicationGatewayFrontendIPConfig -Name 'fip01' -PublicIPAddress $publicip

### <a name="step-3"></a>Krok 3

Nakonfigurujte fondu back-end IP adres IP adresy back-end webových serverů. Nejsou tyto IP adresy IP adresy, které přijímají v síti pocházející z front-end koncovém. Nahraďte následující IP adresy přidat vlastní koncové body aplikace IP adres.

    $pool = New-AzureRmApplicationGatewayBackendAddressPool -Name 'pool01' -BackendIPAddresses 1.1.1.1, 2.2.2.2, 3.3.3.3

> [AZURE.NOTE] Plně kvalifikovaný název domény (FQDN) je také platnou hodnotu místo ip adresa pro servery back-end přepínačem - BackendFqdns.

### <a name="step-4"></a>Krok 4

Konfigurace front-end IP port pro veřejné koncovém. Toto je port, že koncoví uživatelé mohli připojit k.

    $fp = New-AzureRmApplicationGatewayFrontendPort -Name 'port01'  -Port 443

### <a name="step-5"></a>Krok 5

Konfigurace certifikátu brány aplikace. Certifikát se používá při dešifrování a znovu šifrovat komunikaci na brány aplikace.

    $cert = New-AzureRmApplicationGatewaySslCertificate -Name cert01 -CertificateFile <full path to .pfx file> -Password <password for certificate file>

> [AZURE.NOTE] V tomto příkladu nakonfiguruje certifikát používaný pro připojení SSL. Certifikát musí být ve formátu .pfx a heslo musí být mezi znaky 4 až 12.

### <a name="step-6"></a>Krok 6

Vytvořte posluchače HTTP brány aplikace. Přiřadíte front-end ip konfigurace, port a protokol ssl certifikát, který chcete použít.

    $listener = New-AzureRmApplicationGatewayHttpListener -Name listener01 -Protocol Https -FrontendIPConfiguration $fipconfig -FrontendPort $fp -SslCertificate $cert

### <a name="step-7"></a>Krok 7

Odeslání certifikát, který má být použit na protokol ssl povoleno back-end fond zdrojů.

> [AZURE.NOTE] Výchozí zkušební získává veřejný klíč z **výchozí** vazby SSL na IP adresu back-end a porovnává veřejné hodnoty klíče, které obdrží hodnotě veřejné key, který tady zadáte. Načtená veřejným klíčem nemusí být nutně požadovanou sítí, ke kterému přenosy půjde, **Pokud** používáte záhlaví Host (hostitel) a SNI na back-end. Pokud jste na pochybách, navštěvujte blog o https://127.0.0.1/ na koncích zpět k potvrzení, který certifikát se používá pro vazbu **výchozí** SSL. Pomocí klávesy veřejné tohoto požadavku v této části. Pokud používáte záhlaví hostitele a SNI na HTTPS vazby a nezobrazí odpověď a certifikát požadavku ruční prohlížeče na https://127.0.0.1/ na koncích zpět, musíte nastavit výchozí vazby SSL na koncích zpět. Pokud není uděláte, se nepovede sond a back-end nebudete povolené.

    $authcert = New-AzureRmApplicationGatewayAuthenticationCertificate -Name 'whitelistcert1' -CertificateFile C:\users\gwallace\Desktop\cert.cer

> [AZURE.NOTE] Certifikát uvedených v tomto kroku by měl být veřejným klíčem prezentovat na back-end pfx certifikátu. Exportujte certifikát (ne kořenového certifikátu) nainstalovaný na serveru back-end. CER naformátovat a použít v tomto kroku. Tento krok povolených programů back-end brána aplikace.

### <a name="step-8"></a>Krok 8

Nastavení aplikace brány back-end http. Přiřadíte certifikát uložit v předchozím kroku, nastavení protokolu http.

    $poolSetting = New-AzureRmApplicationGatewayBackendHttpSettings -Name 'setting01' -Port 443 -Protocol Https -CookieBasedAffinity Enabled -AuthenticationCertificates $authcert

### <a name="step-9"></a>Krok 9

Vytvoření zatížení vyrovnávání směrování pravidlo, které nakonfiguruje chování Vyrovnávání zatížení. V tomto příkladu je vytvořeno kruhového základní pravidlo.

    $rule = New-AzureRmApplicationGatewayRequestRoutingRule -Name 'rule01' -RuleType basic -BackendHttpSettings $poolSetting -HttpListener $listener -BackendAddressPool $pool

### <a name="step-10"></a>Krok 10

Konfigurace velikosti instance aplikace brány.  Dostupné formáty **Standardní\_malé**, **Standardní\_střední**, a **Standardní\_velké**.  Pro kapacitu jsou k dispozici hodnoty 1 až 10.

    $sku = New-AzureRmApplicationGatewaySku -Name Standard_Small -Tier Standard -Capacity 2

>[AZURE.NOTE] Počet instancí 1 lze vybrat pro účely testování. Je důležité vědět, že všechny instance počet v části dvě instance nevztahuje SLA a proto se nedoporučuje. Malé bran se mají být použity pro vývojáře test a ne pro účely výroby.

### <a name="step-11"></a>Krok 11

Konfigurace zásad protokol SSL pro použití na brány aplikace. Bráně aplikace podporuje možnost zakázat některé verze protokol SSL.

Seznam verzí Protocol (protokol), které mohou být zakázány jsou tyto hodnoty.

- **TLSv1_0**
- **TLSv1_1**
- **TLSv1_2**

Následující příklad zakáže TLSv1\_0.

    $sslPolicy = New-AzureRmApplicationGatewaySslPolicy -DisabledSslProtocols TLSv1_0

## <a name="create-the-application-gateway"></a>Vytvoření brány pro aplikace

Pomocí předchozích kroků můžete vytvořte aplikace brány. Vytvoření brány je dlouho spuštěný proces.

    $appgw = New-AzureRmApplicationGateway -Name appgateway -SslCertificates $cert -ResourceGroupName "appgw-rg" -Location "West US" -BackendAddressPools $pool -BackendHttpSettingsCollection $poolSetting -FrontendIpConfigurations $fipconfig -GatewayIpConfigurations $gipconfig -FrontendPorts $fp -HttpListeners $listener -RequestRoutingRules $rule -Sku $sku -SslPolicy $sslPolicy -AuthenticationCertificates $authcert -Verbose

## <a name="disable-ssl-protocol-versions-on-an-existing-application-gateway"></a>Zakázání verze protokol SSL ve stávající bráně aplikace

Předchozích kroků vrátíte vytvořením aplikace s konce ssl a zakázání některých verzí protokol SSL. Následující příklad zakáže určité zásady SSL ve stávající bráně aplikace.

### <a name="step-1"></a>Krok 1

Načtení aplikace brány aktualizovat.

    $gw = Get-AzureRmApplicationGateway -Name AdatumAppGateway -ResourceGroupName AdatumAppGatewayRG

### <a name="step-2"></a>Krok 2

Definování zásad SSL. V následujícím příkladu jsou zakázány TLSv1.0 a TLSv1.1.

    Set-AzureRmApplicationGatewaySslPolicy -DisabledSslProtocols TLSv1_0, TLSv1_1 -ApplicationGateway $gw

### <a name="step-3"></a>Krok 3

Nakonec aktualizujte bránu. Je důležité mít na paměti, že tento poslední krok zbývá dlouho pracovního úkolu. Když ho uděláte, konce ssl nakonfigurovaný na brány aplikace.

    $gw | Set-AzureRmApplicationGateway

## <a name="get-application-gateway-dns-name"></a>Získání aplikace brány DNS jména

Po vytvoření brány dalším krokem je třeba nakonfigurovat front-end pro komunikaci. Při použití veřejnou IP vyžaduje aplikaci brány dynamicky přiřazené názvu DNS, které není vhodné. Zajistěte, aby koncoví uživatelé mohli přístupů brány aplikace záznam CNAME lze tak, aby ukazovaly na veřejné koncový bod brány aplikace. [Konfigurace název vlastní domény v Azure](../cloud-services/cloud-services-custom-domain-name-portal.md). K tomuto účelu načítejte podrobnosti o bráně aplikace a názvu přidružené IP/DNS pomocí PublicIPAddress prvek připojen k bráně aplikace. Název aplikace brány DNS bude použito k vytvoření záznamu CNAME, který odkazuje dvou webových aplikací pro tento název DNS. Použití záznamy o se nedoporučuje, protože VIP mohou měnit po restartování aplikace brány.
    
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

Další informace o hardening zabezpečení webových aplikací pomocí webové aplikace Firewall prostřednictvím aplikace brány navštivte [Web aplikace Brána Firewall – přehled](application-gateway-webapplicationfirewall-overview.md)

[scenario]: ./media/application-gateway-end-to-end-ssl-powershell/scenario.png
