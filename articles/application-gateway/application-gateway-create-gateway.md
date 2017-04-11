<properties
   pageTitle="Vytvoření, spuštění nebo odstranění brány aplikační | Microsoft Azure"
   description="Tato stránka obsahuje pokyny k vytváření, konfiguraci, spustit a odstranit bránu Azure aplikace"
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

# <a name="create-start-or-delete-an-application-gateway"></a>Vytvoření, spuštění nebo odstranění aplikace brány

> [AZURE.SELECTOR]
- [Portál Azure](application-gateway-create-gateway-portal.md)
- [Azure prostředí PowerShell správce prostředků](application-gateway-create-gateway-arm.md)
- [Azure klasické prostředí PowerShell](application-gateway-create-gateway.md)
- [Azure správce prostředků šablony](application-gateway-create-gateway-arm-template.md)
- [Azure rozhraní příkazového řádku](application-gateway-create-gateway-cli.md)

Azure aplikace brána je Vyrovnávání zatížení vrstvy-7. Poskytuje selhání směrování výkonu požadavků HTTP mezi jiné servery, ať už jde o v cloudu a místní. Aplikace brány nabízí spoustu z funkcí aplikace doručení řadiče ADC včetně Vyrovnávání zatížení HTTP, na základě souborů cookie relace spřažení, převzít Secure (Sockets Layer SSL), vlastní stavu sond podporu více webů a mnoha dalších. Úplný seznam podporovaných funkcí naleznete [Přehled brány aplikace](application-gateway-introduction.md)

Tento článek vás provede kroky vytvoření, konfigurace, spustit a odstraňovat brány aplikace.

## <a name="before-you-begin"></a>Než začnete

1. Nainstalujte nejnovější verzi rutiny prostředí PowerShell Azure pomocí webové platformy. Můžete stáhnout a nainstalovat nejnovější verzi z části **Prostředí Windows PowerShell** [ke stažení stránky](https://azure.microsoft.com/downloads/).
2. Pokud máte existující virtuální sítě, vyberte existující podsítě prázdné nebo vytvoříte nové podsítě v síti existující virtuální pouze pro použití aplikace brány. Nelze nasadit brány aplikace k jiné síti virtuální než prostředky, které chcete nasadit za aplikace brány, pokud vnet prozkoumávání se používá. Další informace najdete [Vnet prozkoumávání](../virtual-network/virtual-network-peering-overview.md)
3. Zkontrolujte, jestli máte pracovní virtuální sítě s platnou podsítí. Ujistěte se, že bez cloudu nasazení nebo virtuálních počítačích jsou podsítě. Aplikace brány musí být osamoceně v podsítě virtuální sítě.
3. Servery, které můžete konfigurovat použití aplikace brány musí existovat nebo přiřadili jejich koncové body vytvořené v virtuální sítě nebo s veřejnou IP/VIP.

## <a name="what-is-required-to-create-an-application-gateway"></a>Co je nutné k vytvoření brány aplikační?

Při použití příkazu **Nový AzureApplicationGateway** vytvořte bránu aplikace žádná konfigurace se v tomto okamžiku a jsou nakonfigurované nově vytvořený zdrojů pomocí XML nebo konfiguračního objektu.

Hodnoty jsou:

- **Back-end serveru fondu:** Seznam IP adres servery back-end. IP adresy uvedené by měl buď patří do podsítě virtuální sítě nebo by měl být veřejnou IP/VIP.
- **Nastavení fondu back-end serveru:** Každý fondu má nastavení například port Protocol (protokol) a na základě souborů cookie spřažení. Toto nastavení je stejným do fondu a zaevidují do všech serverů v rámci fondu.
- **Front-end portu:** Toto je veřejné port, který je otevřen v bráně aplikace. Přenosy narazí tento port a potom přesměrována k některému z back-end serverů.
- **Posluchače:** Posluchače má front-end port protokol (Http nebo Https tyto hodnoty jsou malá a velká písmena) a název certifikátu SSL (Pokud konfigurace SSL převzít).
- **Pravidlo:** Pravidlo váže posluchače a fondu serveru back-end a definuje které serveru back-end fondu přenos budou přesměrovány při narazí konkrétní posluchače.

## <a name="create-an-application-gateway"></a>Vytvoření brány pro aplikace

Vytvoření brány aplikace:

1. Vytvoření brány prostředek aplikace.
2. Vytvoření konfiguračního souboru XML nebo konfiguračního objektu.
3. Potvrďte konfigurace brány zdroji nově vytvořený aplikace.

>[AZURE.NOTE] Pokud potřebujete konfigurovat vlastní zkušební aplikace brány, v tématu [Vytvoření brány aplikační s vlastní sond pomocí prostředí PowerShell](application-gateway-create-probe-classic-ps.md). Podívejte se na [vlastní sond a sledování stavu](application-gateway-probe-overview.md) Další informace.

![Příklad scénář][scenario]

### <a name="create-an-application-gateway-resource"></a>Vytvoření brány prostředek aplikace

Vytvořte bránu, použijte rutinu **New-AzureApplicationGateway** nahradit hodnoty vlastní. Fakturace brány nespustí v tomto okamžiku. Fakturace zahájí později, je-li brány úspěšně spuštěna.

Následující příklad vytvoří brány aplikační pomocí virtuální sítě s názvem "testvnet1" a podsítě s názvem "podsítě-1".


    New-AzureApplicationGateway -Name AppGwTest -VnetName testvnet1 -Subnets @("Subnet-1")

    VERBOSE: 4:31:35 PM - Begin Operation: New-AzureApplicationGateway
    VERBOSE: 4:32:37 PM - Completed Operation: New-AzureApplicationGateway
    Name       HTTP Status Code     Operation ID                             Error
    ----       ----------------     ------------                             ----
    Successful OK                   55ef0460-825d-2981-ad20-b9a8af41b399


 *Popis*, *InstanceCount*a *GatewaySize* jsou volitelné parametry.

Ověřte, že byl vytvořen brány, můžete použít rutinu **Get-AzureApplicationGateway** .

    Get-AzureApplicationGateway AppGwTest
    Name          : AppGwTest
    Description   :
    VnetName      : testvnet1
    Subnets       : {Subnet-1}
    InstanceCount : 2
    GatewaySize   : Medium
    State         : Stopped
    VirtualIPs    : {}
    DnsName       :

>[AZURE.NOTE]  Výchozí *InstanceCount* hodnotu 2, s maximální hodnotou 10. Výchozí *GatewaySize* hodnotu Střední. Můžete vybrat mezi malý, střední a velikost velké.

*VirtualIPs* a *Název_dns* jsou zobrazeny jako prázdné, protože brány ona nezačne. Tyto vytvořené po brána je ve stavu spuštěno.

## <a name="configure-the-application-gateway"></a>Konfigurace aplikace brány

Konfigurace aplikace brány pomocí XML nebo konfiguračního objektu.

## <a name="configure-the-application-gateway-by-using-xml"></a>Konfigurace aplikace brány pomocí XML

V následujícím příkladu pomocí souboru XML ke konfiguraci nastavení brány všechny aplikace a potvrďte zdroji brány aplikace.  

### <a name="step-1"></a>Krok 1  

Zkopírujte následující text do poznámkového bloku.

    <?xml version="1.0" encoding="utf-8"?>
    <ApplicationGatewayConfiguration xmlns:i="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/windowsazure">
        <FrontendPorts>
            <FrontendPort>
                <Name>(name-of-your-frontend-port)</Name>
                <Port>(port number)</Port>
            </FrontendPort>
        </FrontendPorts>
        <BackendAddressPools>
            <BackendAddressPool>
                <Name>(name-of-your-backend-pool)</Name>
                <IPAddresses>
                    <IPAddress>(your-IP-address-for-backend-pool)</IPAddress>
                    <IPAddress>(your-second-IP-address-for-backend-pool)</IPAddress>
                </IPAddresses>
            </BackendAddressPool>
        </BackendAddressPools>
        <BackendHttpSettingsList>
            <BackendHttpSettings>
                <Name>(backend-setting-name-to-configure-rule)</Name>
                <Port>80</Port>
                <Protocol>[Http|Https]</Protocol>
                <CookieBasedAffinity>Enabled</CookieBasedAffinity>
            </BackendHttpSettings>
        </BackendHttpSettingsList>
        <HttpListeners>
            <HttpListener>
                <Name>(name-of-the-listener)</Name>
                <FrontendPort>(name-of-your-frontend-port)</FrontendPort>
                <Protocol>[Http|Https]</Protocol>
            </HttpListener>
        </HttpListeners>
        <HttpLoadBalancingRules>
            <HttpLoadBalancingRule>
                <Name>(name-of-load-balancing-rule)</Name>
                <Type>basic</Type>
                <BackendHttpSettings>(backend-setting-name-to-configure-rule)</BackendHttpSettings>
                <Listener>(name-of-the-listener)</Listener>
                <BackendAddressPool>(name-of-your-backend-pool)</BackendAddressPool>
            </HttpLoadBalancingRule>
        </HttpLoadBalancingRules>
    </ApplicationGatewayConfiguration>

Upravte hodnoty závorky konfigurace položek. Uložte soubor s příponou .xml.

>[AZURE.IMPORTANT] Položka protokolu Http nebo Https je velká a malá písmena.

Následující příklad ukazuje, jak nastavení brány aplikace pomocí konfiguračního souboru. Příklad načíst zůstatky přenosy protokolu HTTP na veřejné portu 80 a odešle v síti s portem back-end 80 mezi dvěma IP adres.

    <?xml version="1.0" encoding="utf-8"?>
    <ApplicationGatewayConfiguration xmlns:i="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/windowsazure">
        <FrontendPorts>
            <FrontendPort>
                <Name>FrontendPort1</Name>
                <Port>80</Port>
            </FrontendPort>
        </FrontendPorts>
        <BackendAddressPools>
            <BackendAddressPool>
                <Name>BackendPool1</Name>
                <IPAddresses>
                    <IPAddress>10.0.0.1</IPAddress>
                    <IPAddress>10.0.0.2</IPAddress>
                </IPAddresses>
            </BackendAddressPool>
        </BackendAddressPools>
        <BackendHttpSettingsList>
            <BackendHttpSettings>
                <Name>BackendSetting1</Name>
                <Port>80</Port>
                <Protocol>Http</Protocol>
                <CookieBasedAffinity>Enabled</CookieBasedAffinity>
            </BackendHttpSettings>
        </BackendHttpSettingsList>
        <HttpListeners>
            <HttpListener>
                <Name>HTTPListener1</Name>
                <FrontendPort>FrontendPort1</FrontendPort>
                <Protocol>Http</Protocol>
            </HttpListener>
        </HttpListeners>
        <HttpLoadBalancingRules>
            <HttpLoadBalancingRule>
                <Name>HttpLBRule1</Name>
                <Type>basic</Type>
                <BackendHttpSettings>BackendSetting1</BackendHttpSettings>
                <Listener>HTTPListener1</Listener>
                <BackendAddressPool>BackendPool1</BackendAddressPool>
            </HttpLoadBalancingRule>
        </HttpLoadBalancingRules>
    </ApplicationGatewayConfiguration>


### <a name="step-2"></a>Krok 2

Pak nastavení brány aplikace. Použijte rutinu **Set-AzureApplicationGatewayConfig** s konfiguračního souboru XML.


    Set-AzureApplicationGatewayConfig -Name AppGwTest -ConfigFile "D:\config.xml"

    VERBOSE: 7:54:59 PM - Begin Operation: Set-AzureApplicationGatewayConfig
    VERBOSE: 7:55:32 PM - Completed Operation: Set-AzureApplicationGatewayConfig
    Name       HTTP Status Code     Operation ID                             Error
    ----       ----------------     ------------                             ----
    Successful OK                   9b995a09-66fe-2944-8b67-9bb04fcccb9d

## <a name="configure-the-application-gateway-by-using-a-configuration-object"></a>Konfigurace aplikace brány pomocí konfiguračního objektu

Následující příklad ukazuje, jak konfigurace brány aplikace pomocí konfigurace objektů. Všechny položky konfigurace musíte nakonfigurovat jednotlivě a potom přidán do aplikace objekt konfigurace brány. Po vytvoření konfigurační objekt, použijte příkaz **Nastavení AzureApplicationGateway** potvrďte konfigurace brány zdroji dříve vytvořené aplikace.

>[AZURE.NOTE] Před přiřazení hodnoty na jednotlivé objekty konfigurace, budete muset deklarovat, jaký druh objekt prostředí PowerShell používá pro úložiště. První řádek k vytvoření jednotlivé položky definuje jaké Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model (název_objektu) se používají.

### <a name="step-1"></a>Krok 1

Vytvoření jednotlivých konfigurace vše.

Vytvoření front-end IP, jak je vidět v následujícím příkladu.

    $fip = New-Object Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.FrontendIPConfiguration
    $fip.Name = "fip1"
    $fip.Type = "Private"
    $fip.StaticIPAddress = "10.0.0.5"

Vytvoření front-end portu, jak je vidět v následujícím příkladu.

    $fep = New-Object Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.FrontendPort
    $fep.Name = "fep1"
    $fep.Port = 80

Vytvoření fondu serveru back-end.

 Definování IP adresy, které se přidají do fondu serveru back-end, jak je vidět v následujícím příkladu.


    $servers = New-Object Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.BackendServerCollection
    $servers.Add("10.0.0.1")
    $servers.Add("10.0.0.2")

 Pomocí objektu $server k přidání hodnot do back-end fondu objektu ($pool).

    $pool = New-Object Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.BackendAddressPool
    $pool.BackendServers = $servers
    $pool.Name = "pool1"

Vytvoření fondu nastavení serveru back-end.

    $setting = New-Object Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.BackendHttpSettings
    $setting.Name = "setting1"
    $setting.CookieBasedAffinity = "enabled"
    $setting.Port = 80
    $setting.Protocol = "http"

Vytvoření posluchače.

    $listener = New-Object Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.HttpListener
    $listener.Name = "listener1"
    $listener.FrontendPort = "fep1"
    $listener.FrontendIP = "fip1"
    $listener.Protocol = "http"
    $listener.SslCert = ""

Vytvořte pravidlo.

    $rule = New-Object Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.HttpLoadBalancingRule
    $rule.Name = "rule1"
    $rule.Type = "basic"
    $rule.BackendHttpSettings = "setting1"
    $rule.Listener = "listener1"
    $rule.BackendAddressPool = "pool1"

### <a name="step-2"></a>Krok 2

Všechny položky jednotlivé konfigurace přiřadíte objekt konfigurace brány aplikace ($appgwconfig).

Přidání front-end IP konfiguraci.

    $appgwconfig = New-Object Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.ApplicationGatewayConfiguration
    $appgwconfig.FrontendIPConfigurations = New-Object "System.Collections.Generic.List[Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.FrontendIPConfiguration]"
    $appgwconfig.FrontendIPConfigurations.Add($fip)

Přidejte front-end port konfiguraci.

    $appgwconfig.FrontendPorts = New-Object "System.Collections.Generic.List[Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.FrontendPort]"
    $appgwconfig.FrontendPorts.Add($fep)

Přidání fondu serveru back-end konfiguraci.

    $appgwconfig.BackendAddressPools = New-Object "System.Collections.Generic.List[Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.BackendAddressPool]"
    $appgwconfig.BackendAddressPools.Add($pool)

Přidejte back-end fondu nastavení konfigurace.

    $appgwconfig.BackendHttpSettingsList = New-Object "System.Collections.Generic.List[Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.BackendHttpSettings]"
    $appgwconfig.BackendHttpSettingsList.Add($setting)

Přidáte posluchače konfiguraci.

    $appgwconfig.HttpListeners = New-Object "System.Collections.Generic.List[Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.HttpListener]"
    $appgwconfig.HttpListeners.Add($listener)

Přidání pravidla ke konfiguraci.

    $appgwconfig.HttpLoadBalancingRules = New-Object "System.Collections.Generic.List[Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.HttpLoadBalancingRule]"
    $appgwconfig.HttpLoadBalancingRules.Add($rule)

### <a name="step-3"></a>Krok 3

Potvrďte objekt konfigurace brány zdrojů aplikace pomocí **Nastavení AzureApplicationGatewayConfig**.

    Set-AzureApplicationGatewayConfig -Name AppGwTest -Config $appgwconfig

## <a name="start-the-gateway"></a>Zahájení brány

Po konfiguraci brány získáte pomocí rutiny **Start AzureApplicationGateway** spustit brány. Po úspěšném spuštění brány, nebude zahájen fakturace aplikací brány.

> [AZURE.NOTE] Rutina **Start AzureApplicationGateway** může trvat až 15 20 minut dokončit.

    Start-AzureApplicationGateway AppGwTest

    VERBOSE: 7:59:16 PM - Begin Operation: Start-AzureApplicationGateway
    VERBOSE: 8:05:52 PM - Completed Operation: Start-AzureApplicationGateway
    Name       HTTP Status Code     Operation ID                             Error
    ----       ----------------     ------------                             ----
    Successful OK                   fc592db8-4c58-2c8e-9a1d-1c97880f0b9b

## <a name="verify-the-gateway-status"></a>Zkontrolujte stav brány

Zkontrolujte stav brány získáte pomocí rutiny **Get-AzureApplicationGateway** . Pokud **Start AzureApplicationGateway** úspěšný v předchozím kroku, by měla běžet *stavu* a *Vip* a *Název_dns* by měla být platných položek.

Následující příklad ukazuje aplikace brány, který je až, spuštění, a jste připravení začít přenosy určené pro `http://<generated-dns-name>.cloudapp.net`.

    Get-AzureApplicationGateway AppGwTest

    VERBOSE: 8:09:28 PM - Begin Operation: Get-AzureApplicationGateway
    VERBOSE: 8:09:30 PM - Completed Operation: Get-AzureApplicationGateway
    Name          : AppGwTest
    Description   :
    VnetName      : testvnet1
    Subnets       : {Subnet-1}
    InstanceCount : 2
    GatewaySize   : Medium
    State         : Running
    Vip           : 138.91.170.26
    DnsName       : appgw-1b8402e8-3e0d-428d-b661-289c16c82101.cloudapp.net


## <a name="delete-an-application-gateway"></a>Odstranění aplikace brány

Chcete-li odstranit bránu aplikace:

1. Ukončení brány získáte pomocí rutiny **Stop AzureApplicationGateway** .
2. Chcete-li odebrat brány získáte pomocí rutiny **AzureApplicationGateway odebrat** .
3. Ověřte, že byla odebrána brány pomocí rutiny **Get-AzureApplicationGateway** .

Následující příklad ukazuje rutinu **Zastavit AzureApplicationGateway** na prvním řádku a za ním uveďte výstupu.

    Stop-AzureApplicationGateway AppGwTest

    VERBOSE: 9:49:34 PM - Begin Operation: Stop-AzureApplicationGateway
    VERBOSE: 10:10:06 PM - Completed Operation: Stop-AzureApplicationGateway
    Name       HTTP Status Code     Operation ID                             Error
    ----       ----------------     ------------                             ----
    Successful OK                   ce6c6c95-77b4-2118-9d65-e29defadffb8

Po přestal stav brány aplikace získáte pomocí rutiny **Odebrat AzureApplicationGateway** odebrat službu.


    Remove-AzureApplicationGateway AppGwTest

    VERBOSE: 10:49:34 PM - Begin Operation: Remove-AzureApplicationGateway
    VERBOSE: 10:50:36 PM - Completed Operation: Remove-AzureApplicationGateway
    Name       HTTP Status Code     Operation ID                             Error
    ----       ----------------     ------------                             ----
    Successful OK                   055f3a96-8681-2094-a304-8d9a11ad8301

Pokud chcete ověřit, že službu odebrali jsme možnost, můžete použít rutinu **Get-AzureApplicationGateway** . Tento krok není povinný.


    Get-AzureApplicationGateway AppGwTest

    VERBOSE: 10:52:46 PM - Begin Operation: Get-AzureApplicationGateway

    Get-AzureApplicationGateway : ResourceNotFound: The gateway does not exist.
    .....

## <a name="next-steps"></a>Další kroky

Pokud chcete konfigurovat SSL převedení, přečtěte si téma [Konfigurace brány aplikační pro SSL převzít](application-gateway-ssl.md).

Pokud chcete konfigurovat brány aplikace pomocí služby Vyrovnávání zatížení vnitřní, v tématu [Vytvoření brány aplikační s Vyrovnávání zatížení vnitřní (ILB)](application-gateway-ilb.md).

Pokud budete potřebovat další informace o načtení vyrovnávající možnosti obecně, přečtěte si článek:

- [Vyrovnávání zatížení Azure](https://azure.microsoft.com/documentation/services/load-balancer/)
- [Azure přenosy správce](https://azure.microsoft.com/documentation/services/traffic-manager/)

[scenario]: ./media/application-gateway-create-gateway/scenario.png