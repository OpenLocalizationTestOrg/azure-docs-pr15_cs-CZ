<properties
   pageTitle="Vytvoření vlastní zkušební aplikace brány pomocí prostředí PowerShell v modelu klasické nasazení | Microsoft Azure"
   description="Naučte se vytvářet vlastní zkušební aplikace brány pomocí prostředí PowerShell v modelu klasické nasazení"
   services="application-gateway"
   documentationCenter="na"
   authors="georgewallace"
   manager="carmonm"
   editor=""
   tags="azure-service-management"
/>
<tags  
   ms.service="application-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/25/2016"
   ms.author="gwallace" />

# <a name="create-a-custom-probe-for-azure-application-gateway-classic-by-using-powershell"></a>Vytvoření vlastní zkušební Azure aplikace brány (klasické) pomocí prostředí PowerShell

> [AZURE.SELECTOR]
- [Azure portálu](application-gateway-create-probe-portal.md)
- [Azure prostředí PowerShell správce prostředků](application-gateway-create-probe-ps.md)
- [Azure klasické prostředí PowerShell](application-gateway-create-probe-classic-ps.md)

[AZURE.INCLUDE [azure-probe-intro-include](../../includes/application-gateway-create-probe-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/learn-about-deployment-models-classic-include.md)]Zjistěte, jak provádět [tyto kroky pomocí modelu správce prostředků](application-gateway-create-probe-ps.md).

[AZURE.INCLUDE [azure-ps-prerequisites-include.md](../../includes/azure-ps-prerequisites-include.md)]

## <a name="create-an-application-gateway"></a>Vytvoření brány pro aplikace

Vytvoření brány aplikace:

1. Vytvoření brány prostředek aplikace.
2. Vytvoření konfiguračního souboru XML nebo konfiguračního objektu.
3. Potvrďte konfigurace brány zdroji nově vytvořený aplikace.

### <a name="create-an-application-gateway-resource"></a>Vytvoření brány prostředek aplikace

Vytvořte bránu, použijte rutinu **New-AzureApplicationGateway** nahradit hodnoty vlastní. Fakturace brány nespustí v tomto okamžiku. Fakturace zahájí později, je-li brány úspěšně spuštěna.

Následující příklad vytvoří brány aplikační pomocí virtuální sítě s názvem "testvnet1" a podsítě s názvem "podsítě-1".

    New-AzureApplicationGateway -Name AppGwTest -VnetName testvnet1 -Subnets @("Subnet-1")

Ověřte, že byl vytvořen brány, můžete použít rutinu **Get-AzureApplicationGateway** .

    Get-AzureApplicationGateway AppGwTest

>[AZURE.NOTE]  Výchozí *InstanceCount* hodnotu 2, s maximální hodnotou 10. Výchozí *GatewaySize* hodnotu Střední. Můžete vybrat mezi malý, střední a velikost velké.

 *VirtualIPs* a *Název_dns* jsou zobrazeny jako prázdné, protože brány ona nezačne. Tyto vytvořené po brána je ve stavu spuštěno.

## <a name="configure-an-application-gateway"></a>Konfigurace brány aplikace

Konfigurace aplikace brány pomocí XML nebo konfiguračního objektu.

## <a name="configure-an-application-gateway-by-using-xml"></a>Konfigurace brány aplikační pomocí XML

V následujícím příkladu pomocí souboru XML ke konfiguraci nastavení brány všechny aplikace a potvrďte zdroji brány aplikace.  

### <a name="step-1"></a>Krok 1

Zkopírujte následující text do poznámkového bloku.

    <ApplicationGatewayConfiguration xmlns:i="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/windowsazure">
    <FrontendIPConfigurations>
        <FrontendIPConfiguration>
            <Name>fip1</Name>
            <Type>Private</Type>
        </FrontendIPConfiguration>
    </FrontendIPConfigurations>    
    <FrontendPorts>
        <FrontendPort>
            <Name>port1</Name>
            <Port>80</Port>
        </FrontendPort>
    </FrontendPorts>
    <Probes>
        <Probe>
            <Name>Probe01</Name>
            <Protocol>Http</Protocol>
            <Host>contoso.com</Host>
            <Path>/path/custompath.htm</Path>
            <Interval>15</Interval>
            <Timeout>15</Timeout>
            <UnhealthyThreshold>5</UnhealthyThreshold>
        </Probe>
      </Probes>
     <BackendAddressPools>
        <BackendAddressPool>
            <Name>pool1</Name>
            <IPAddresses>
                <IPAddress>1.1.1.1</IPAddress>
                <IPAddress>2.2.2.2</IPAddress>
            </IPAddresses>
        </BackendAddressPool>
    </BackendAddressPools>
    <BackendHttpSettingsList>
        <BackendHttpSettings>
            <Name>setting1</Name>
            <Port>80</Port>
            <Protocol>Http</Protocol>
            <CookieBasedAffinity>Enabled</CookieBasedAffinity>
            <RequestTimeout>120</RequestTimeout>
            <Probe>Probe01</Probe>
        </BackendHttpSettings>
    </BackendHttpSettingsList>
    <HttpListeners>
        <HttpListener>
            <Name>listener1</Name>
            <FrontendIP>fip1</FrontendIP>
        <FrontendPort>port1</FrontendPort>
            <Protocol>Http</Protocol>
        </HttpListener>
    </HttpListeners>
    <HttpLoadBalancingRules>
        <HttpLoadBalancingRule>
            <Name>lbrule1</Name>
            <Type>basic</Type>
            <BackendHttpSettings>setting1</BackendHttpSettings>
            <Listener>listener1</Listener>
            <BackendAddressPool>pool1</BackendAddressPool>
        </HttpLoadBalancingRule>
    </HttpLoadBalancingRules>
    </ApplicationGatewayConfiguration>


Upravte hodnoty závorky konfigurace položek. Uložte soubor s příponou .xml.

Následující příklad ukazuje, jak nastavení brány aplikace zavádění zůstatek přenosy protokolu HTTP na veřejné portu 80 a odeslat v síti s portem back-end 80 mezi dvěma IP adres pomocí vlastní zkušební pomocí konfiguračního souboru.

>[AZURE.IMPORTANT] Položka protokolu Http nebo Https je velká a malá písmena.

Nová položka konfigurace \<Probe\> se přidá ke konfiguraci vlastní sond.

Konfigurace parametry jsou:

- **Název** - název odkazu pro vlastní zkušební.
- **Protocol (protokol)** – protokol sloužící (možné hodnoty jsou HTTP nebo HTTPS).
- **Host (hostitel)** a **cesta** - cestu úplnou adresu URL, která vyvolání brána aplikace stav instance. Například pokud máte web http://contoso.com/, potom vlastní zkušební lze nakonfigurovat pro "http://contoso.com/path/custompath.htm" zkušební kontroly mít úspěšné odpověď HTTP.
- **Interval** - nakonfiguruje kontroly interval zkušební v sekundách.
- **Časový limit** - definuje zkušební časový limit pro kontrolu HTTP odpověď.
- **UnhealthyThreshold** - počet neúspěšných odpovědí HTTP potřebné k označení instance back-end jako *Chybná*.

Název zkušební odkazuje <BackendHttpSettings> konfigurace přiřazení které fondu back-end používá vlastní zkušební nastavení.

## <a name="add-a-custom-probe-configuration-to-an-existing-application-gateway"></a>Přidání vlastní zkušební konfigurace k existující bráně aplikace

Změna aktuální konfigurace brány aplikační vyžaduje tři kroky: získání aktuální konfigurační soubor XML, změnit mít vlastní zkušební a nakonfigurujte bránu aplikace nové nastavení XML.

### <a name="step-1"></a>Krok 1

Pokud potřebujete souboru XML pomocí get-AzureApplicationGatewayConfig. To exportuje konfiguraci XML upravit tak, aby přidejte zkušební nastavení.

    Get-AzureApplicationGatewayConfig -Name "<application gateway name>" -Exporttofile "<path to file>"


### <a name="step-2"></a>Krok 2

Otevřete soubor XML v textovém editoru. Přidat `<probe>` oddílu po `<frontendport>`.

    <Probes>
        <Probe>
            <Name>Probe01</Name>
            <Protocol>Http</Protocol>
            <Host>contoso.com</Host>
            <Path>/path/custompath.htm</Path>
            <Interval>15</Interval>
            <Timeout>15</Timeout>
            <UnhealthyThreshold>5</UnhealthyThreshold>
        </Probe>
    </Probes>

V části backendHttpSettings XML přidáte název zkušební, jak je vidět v následujícím příkladu:

        <BackendHttpSettings>
            <Name>setting1</Name>
            <Port>80</Port>
            <Protocol>Http</Protocol>
            <CookieBasedAffinity>Enabled</CookieBasedAffinity>
            <RequestTimeout>120</RequestTimeout>
            <Probe>Probe01</Probe>
        </BackendHttpSettings>

Uložte soubor XML.

### <a name="step-3"></a>Krok 3

Pomocí **Nastavení AzureApplicationGatewayConfig**aktualizujte konfigurace brány aplikace s nový soubor XML. Brána aplikace se aktualizuje s novou konfiguraci.

    Set-AzureApplicationGatewayConfig -Name "<application gateway name>" -Configfile "<path to file>"


## <a name="next-steps"></a>Další kroky

Pokud chcete konfigurovat převedení Secure (Sockets Layer SSL), přečtěte si téma [Konfigurace brány aplikační pro SSL převzít](application-gateway-ssl.md).

Pokud chcete konfigurovat brány aplikace pomocí služby Vyrovnávání zatížení vnitřní, v tématu [Vytvoření brány aplikační s Vyrovnávání zatížení vnitřní (ILB)](application-gateway-ilb.md).
