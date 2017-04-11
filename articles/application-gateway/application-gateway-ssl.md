<properties
   pageTitle="Konfigurace aplikace bráně pro převedení SSL pomocí klasické nasazení | Microsoft Azure"
   description="Tento článek obsahuje pokyny k vytvoření brány aplikace s SSL převzít pomocí modelu Azure klasické nasazení."
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

# <a name="configure-an-application-gateway-for-ssl-offload-by-using-the-classic-deployment-model"></a>Konfigurace brány aplikační pro převedení SSL pomocí klasické nasazení modelu

> [AZURE.SELECTOR]
-[Azure portálu](application-gateway-ssl-portal.md)
-[Azure správce prostředků PowerShell](application-gateway-ssl-arm.md)
-[Klasické prostředí PowerShell Azure](application-gateway-ssl.md)

Ukončit relaci Secure (Sockets Layer SSL) po bránu vyhnout nákladné úkoly dešifrování SSL stát na farmě web je možné konfigurovat Azure aplikace brány. Převedení SSL také zjednodušuje instalační program serveru front-end a správu webové aplikace.

## <a name="before-you-begin"></a>Než začnete

1. Nainstalujte nejnovější verzi rutiny prostředí PowerShell Azure pomocí webové platformy. Můžete stáhnout a nainstalovat nejnovější verzi z části **Prostředí Windows PowerShell** [ke stažení stránky](https://azure.microsoft.com/downloads/).
2. Zkontrolujte, jestli máte pracovní virtuální sítě s platnou podsítí. Ujistěte se, že bez cloudu nasazení nebo virtuálních počítačích jsou podsítě. Aplikace brány musí být osamoceně v podsítě virtuální sítě.
3. Servery, které můžete konfigurovat použití aplikace brány musí existovat nebo přiřadili jejich koncové body vytvořené v virtuální sítě nebo s veřejnou IP/VIP.

Pro nastavení SSL převedení brány aplikační, proveďte následující kroky v uvedeném pořadí:

1. [Vytvoření brány pro aplikace](#create-an-application-gateway)
2. [Nahrání certifikáty SSL](#upload-ssl-certificates)
3. [Konfigurace brány](#configure-the-gateway)
4. [Nastavení brány](#set-the-gateway-configuration)
5. [Zahájení brány](#start-the-gateway)
6. [Zkontrolujte stav brány](#verify-the-gateway-status)


## <a name="create-an-application-gateway"></a>Vytvoření brány pro aplikace

Vytvořte bránu, použijte rutinu **New-AzureApplicationGateway** nahradit hodnoty vlastní. Fakturace bránu pro nespustí v tomto okamžiku. Fakturace zahájí později, je-li brány úspěšně spuštěna.

    New-AzureApplicationGateway -Name AppGwTest -VnetName testvnet1 -Subnets @("Subnet-1")

Ověřte, že byl vytvořen brány, můžete použít rutinu **Get-AzureApplicationGateway** .

V ukázkové, *Popis*, *InstanceCount*a *GatewaySize* jsou volitelné parametry. Výchozí *InstanceCount* hodnotu 2, s maximální hodnotou 10. Výchozí *GatewaySize* hodnotu Střední. Malých a velkých jsou k dispozici hodnotám. *VirtualIPs* a *Název_dns* jsou zobrazeny jako prázdné, protože bránu ona nezačne. Tyto hodnoty vytvořené po brána je ve stavu spuštěno.

    Get-AzureApplicationGateway AppGwTest

## <a name="upload-ssl-certificates"></a>Nahrání certifikáty SSL

Umožňuje **Přidat AzureApplicationGatewaySslCertificate** odeslat certifikát serveru ve formátu *pfx* ho bráně aplikace. Název certifikátu je název vybrané uživatelem a musí být jedinečný v rámci brány aplikace. Certifikát je uvedené s tímto názvem všechny operace Správa certifikátů na bráně aplikace.

Následující příklad zobrazuje rutinu, nahraďte hodnotám ve vzorku vlastní.

    Add-AzureApplicationGatewaySslCertificate  -Name AppGwTest -CertificateName GWCert -Password <password> -CertificateFile <full path to pfx file>

Pak ověřte nahrávání certifikát. Použijte rutinu **Get-AzureApplicationGatewayCertificate** .

Tento příklad ukazuje rutinu na prvním řádku a za ním uveďte výstupu.

    Get-AzureApplicationGatewaySslCertificate AppGwTest

    VERBOSE: 5:07:54 PM - Begin Operation: Get-AzureApplicationGatewaySslCertificate
    VERBOSE: 5:07:55 PM - Completed Operation: Get-AzureApplicationGatewaySslCertificate
    Name           : SslCert
    SubjectName    : CN=gwcert.app.test.contoso.com
    Thumbprint     : AF5ADD77E160A01A6......EE48D1A
    ThumbprintAlgo : sha1RSA
    State..........: Provisioned

>[AZURE.NOTE] Heslo certifikát musí být mezi znaky, písmena nebo čísla 4 až 12. Nejsou přijaty speciální znaky.

## <a name="configure-the-gateway"></a>Konfigurace brány

Konfigurace bráně aplikace se skládá z více hodnot. Hodnoty můžete stejným můžete vytvářet konfiguraci.

Hodnoty jsou:

- **Back-end serveru fondu:** Seznam IP adres servery back-end. IP adresy uvedený by měl buď patří do podsítě virtuální sítě nebo by měl být veřejnou IP/VIP.
- **Nastavení fondu back-end serveru:** Každý fondu má nastavení například port Protocol (protokol) a na základě souborů cookie spřažení. Toto nastavení je stejným do fondu a zaevidují do všech serverů v rámci fondu.
- **Front-end portu:** Toto je veřejné port, který je otevřen ve bráně aplikace. Přenosy narazí tento port a potom přesměrována k některému z back-end serverů.
- **Posluchače:** Posluchače má front-end port protokol (Http nebo Https tyto hodnoty jsou malá a velká písmena) a název certifikátu SSL (Pokud konfigurace SSL převzít).
- **Pravidlo:** Pravidlo váže posluchače a fondu serveru back-end a definuje které serveru back-end fondu přenos budou přesměrovány při narazí konkrétní posluchače. V současnosti je podporována pouze *základní* pravidlo. *Základní* pravidla je kruhového zatížení rozdělení.

**Další konfiguraci poznámek**

Konfigurace certifikáty SSL protokolu **HttpListener** změňte na *Https* (malá a velká písmena). **SslCert** element přibude **HttpListener** s hodnotou stejný název jako používaných při nahrávání předcházejícího oddílu certifikáty SSL. Front-end port musí být aktualizován 443.

**Povolení souborů cookie základě spřažení**: bráně aplikace můžete nakonfigurovat tak, aby zajistit, aby se žádost o relace klienta vždy přesměrovávat do stejné OM ve farmě web. Tento scénář vystavila společnost vkládání cookie relace, který umožňuje brány tak, aby směrovaly řádně podporovat. Povolení souborů cookie základě spřažení, nastavte **CookieBasedAffinity** *povoleno* v elementu **BackendHttpSettings** .



Konfigurace můžete vytvořit tak, že vytvoříte konfiguračního objektu nebo pomocí konfiguračního souboru XML.
Vytvářet konfiguraci pomocí konfiguračního souboru XML, použijte v následujícím příkladu:

**Ukázka konfigurace XML**

    <?xml version="1.0" encoding="utf-8"?>
    <ApplicationGatewayConfiguration xmlns:i="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/windowsazure">
        <FrontendIPConfigurations />
        <FrontendPorts>
            <FrontendPort>
                <Name>FrontendPort1</Name>
                <Port>443</Port>
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
                <Protocol>Https</Protocol>
                <SslCert>GWCert</SslCert>
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


## <a name="set-the-gateway-configuration"></a>Nastavení brány

Potom nastavíte aplikace brány. Můžete použít rutinu **Set-AzureApplicationGatewayConfig** s objektem konfiguraci nebo pomocí konfiguračního souboru XML.

    Set-AzureApplicationGatewayConfig -Name AppGwTest -ConfigFile D:\config.xml

## <a name="start-the-gateway"></a>Zahájení brány

Po konfiguraci brány získáte pomocí rutiny **Start AzureApplicationGateway** spustit brány. Po úspěšném spuštění brány, nebude zahájen fakturace aplikací brány.


**Poznámka:** Rutina **Start AzureApplicationGateway** může trvat až 15 20 minut dokončit.

    Start-AzureApplicationGateway AppGwTest

## <a name="verify-the-gateway-status"></a>Zkontrolujte stav brány

Zkontrolujte stav brány získáte pomocí rutiny **Get-AzureApplicationGateway** . Pokud **Start AzureApplicationGateway** úspěšný v předchozím kroku, by měla běžet *stavu* a *VirtualIPs* a *Název_dns* by měla být platných položek.

Tento příklad ukazuje, systém, který je připravení psát provoz brány aplikace.

    Get-AzureApplicationGateway AppGwTest

    Name          : AppGwTest2
    Description   :
    VnetName      : testvnet1
    Subnets       : {Subnet-1}
    InstanceCount : 2
    GatewaySize   : Medium
    State         : Running
    VirtualIPs    : {23.96.22.241}
    DnsName       : appgw-4c960426-d1e6-4aae-8670-81fd7a519a43.cloudapp.net


## <a name="next-steps"></a>Další kroky


Pokud budete potřebovat další informace o načtení vyrovnávání možnosti obecně, přečtěte si článek:

- [Vyrovnávání zatížení Azure](https://azure.microsoft.com/documentation/services/load-balancer/)
- [Azure přenosy správce](https://azure.microsoft.com/documentation/services/traffic-manager/)