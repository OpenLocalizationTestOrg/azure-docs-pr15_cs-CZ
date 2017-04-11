<properties 
   pageTitle="Vytváření a konfigurace brány aplikační s interní řešení pro vyrovnávání zatížení (ILB) v síti virtuální | Microsoft Azure"
   description="Tato stránka obsahuje pokyny ke konfiguraci brány aplikační Azure ke koncovému interní Vyrovnávání zatížení"
   documentationCenter="na"
   services="application-gateway"
   authors="georgewallace"
   manager="jdial"
   editor="tysonn"/>
<tags 
   ms.service="application-gateway"
   ms.devlang="na"
   ms.topic="article" 
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services" 
   ms.date="08/19/2016"
   ms.author="gwallace"/>

# <a name="create-an-application-gateway-with-an-internal-load-balancer-ilb"></a>Vytvoření brány aplikační s interní řešení pro vyrovnávání zatížení (ILB)

> [AZURE.SELECTOR]
- [Azure klasický kroky](application-gateway-ilb.md)
- [Postup v Powershellu správce prostředků](application-gateway-ilb-arm.md)

Aplikace brány můžete nakonfigurovat pomocí internetové virtuální IP nebo interní bod není vystaven na Internetu, nazývaný také koncový bod interní Vyrovnávání zatížení (ILB). Konfigurace brány pomocí ILB je užitečné pro interní řádku obchodní aplikace není vystaven na Internetu. Slouží také k služby/úrovní v rámci vícevrstvé aplikace, který je umístěn v hranici zabezpečení není vystaven na Internetu, ale přesto vyžadují distribuci kruhového zatížení, lepivost relace nebo ukončení SSL. Tento článek vás provede kroky pro nastavení brány aplikační s ILB.

## <a name="before-you-begin"></a>Než začnete

1. Nainstalujte nejnovější verzi rutin prostředí PowerShell Azure pomocí webové platformy. Můžete stáhnout a nainstalovat nejnovější verzi z části **Prostředí Windows PowerShell** [Stáhnout stránky](https://azure.microsoft.com/downloads/).
2. Zkontrolujte, jestli máte pracovní virtuální sítě s platnou podsítí.
3. Zkontrolujte, jestli máte back-end servery v virtuální sítě nebo s veřejnou IP/VIP přiřazené.


Vytvoření brány aplikační, proveďte následující kroky v uvedeném pořadí. 

1. [Vytvoření brány pro aplikace](#create-a-new-application-gateway)
2. [Konfigurace brány](#configure-the-gateway)
3. [Nastavení brány](#set-the-gateway-configuration)
4. [Zahájení brány](#start-the-gateway)
4. [Ověření brány](#verify-the-gateway-status)



## <a name="create-an-application-gateway"></a>Vytvoření brány aplikace:

**Vytvořte bránu**, použít `New-AzureApplicationGateway` rutina nahradit hodnoty vlastní. Všimněte si, že fakturace brány se nespustí v tomto okamžiku. Fakturace zahájí později, je-li brány úspěšně spuštěna.

    PS C:\> New-AzureApplicationGateway -Name AppGwTest -VnetName testvnet1 -Subnets @("Subnet-1")

    VERBOSE: 4:31:35 PM - Begin Operation: New-AzureApplicationGateway 
    VERBOSE: 4:32:37 PM - Completed Operation: New-AzureApplicationGateway
    Name       HTTP Status Code     Operation ID                             Error 
    ----       ----------------     ------------                             ----
    Successful OK                   55ef0460-825d-2981-ad20-b9a8af41b399

**Ověřte** , že byl vytvořen brány, můžete použít `Get-AzureApplicationGateway` rutiny. 

V ukázkové, *Popis*, *InstanceCount*a *GatewaySize* jsou volitelné parametry. Výchozí *InstanceCount* hodnotu 2, s maximální hodnotou 10. Výchozí *GatewaySize* hodnotu Střední. Malých a velkých jsou k dispozici hodnotám. *VIP* a *Název_dns* jsou zobrazeny jako prázdné, protože brány ona nezačne. Tyto vytvořené po brána je ve stavu spuštěno. 

    PS C:\> Get-AzureApplicationGateway AppGwTest

    VERBOSE: 4:39:39 PM - Begin Operation:
    Get-AzureApplicationGateway VERBOSE: 4:39:40 PM - Completed 
    Operation: Get-AzureApplicationGateway
    Name: AppGwTest 
    Description: 
    VnetName: testvnet1 
    Subnets: {Subnet-1} 
    InstanceCount: 2 
    GatewaySize: Medium 
    State: Stopped 
    VirtualIPs: 
    DnsName:


## <a name="configure-the-gateway"></a>Konfigurace brány

Konfigurace brány aplikace se skládá z více hodnot. Hodnoty můžete stejným můžete vytvářet konfiguraci.
 
Hodnoty jsou:

- **Back-end serveru fondu:** Seznam adres IP back-end serverů. IP adresy uvedené by měl buď patří do podsítě VNet nebo by měl být veřejnou IP/VIP. 
- **Nastavení fondu back-end serveru:** Každý fondu má nastavení například port Protocol (protokol) a na základě souborů cookie spřažení. Toto nastavení je stejným do fondu a zaevidují do všech serverů v rámci fondu.
- **Frontend portu:** Toto je veřejné port otevřít v aplikaci brány. Přenosy narazí tento port a potom přesměrována k některému z back-end serverů.
- **Posluchače:** Posluchače má frontend port protokol (Http nebo Https Toto jsou malá a velká písmena) a název certifikátu SSL (Pokud konfigurace SSL převzít). 
- **Pravidlo:** Pravidlo váže posluchače a fondu back-end serveru a definuje které fondu serveru back-end přenos budou přesměrovány při narazí konkrétní posluchače. V současnosti je podporována pouze *základní* pravidlo. *Základní* pravidla je kruhového zatížení rozdělení.

Konfigurace můžete vytvořit tak, že vytvoříte konfiguračního objektu nebo pomocí konfiguračního souboru XML. Vytvářet konfiguraci pomocí konfiguračního souboru XML, použijte uvedené ukázkové dole.



Poznámka:


- *FrontendIPConfigurations* element popisuje relevantní pro konfiguraci aplikace brány pomocí ILB ILB podrobnosti. 

- Frontend IP *Typ* je třeba nastavit na "Soukromé"

- *StaticIPAddress* je třeba nastavit na požadované interní IP obdrží provoz brány. Všimněte si, že je volitelný prvek *StaticIPAddress* . Není-li nastavit, k dispozici interní IP z nasazeném podsítě je vybrán. 

- Hodnoty *název* elementu podle *FrontendIPConfiguration* bude použito v HTTPListener *FrontendIP* prvek odkazovat FrontendIPConfiguration.

 **Ukázka konfigurace XML**

 

        <?xml version="1.0" encoding="utf-8"?>
        <ApplicationGatewayConfiguration xmlns:i="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/windowsazure">
            <FrontendIPConfigurations>
                <FrontendIPConfiguration>
                    <Name>fip1</Name> 
                    <Type>Private</Type> 
                    <StaticIPAddress>10.0.0.10</StaticIPAddress> 
                </FrontendIPConfiguration>
            </FrontendIPConfigurations>
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
                    <FrontendIP>fip1</FrontendIP>
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
    


## <a name="set-the-gateway-configuration"></a>Nastavení brány

Pak vytvoříte brány aplikace. Můžete použít `Set-AzureApplicationGatewayConfig` rutina s konfiguračního objektu nebo se konfiguračního souboru XML. 

    PS C:\> Set-AzureApplicationGatewayConfig -Name AppGwTest -ConfigFile D:\config.xml

    VERBOSE: 7:54:59 PM - Begin Operation: Set-AzureApplicationGatewayConfig 
    VERBOSE: 7:55:32 PM - Completed Operation: Set-AzureApplicationGatewayConfig
    Name       HTTP Status Code     Operation ID                             Error 
    ----       ----------------     ------------                             ----
    Successful OK                   9b995a09-66fe-2944-8b67-9bb04fcccb9d

## <a name="start-the-gateway"></a>Zahájení brány

Po konfiguraci brány, použít `Start-AzureApplicationGateway` rutina zahájíte brány. Po úspěšném spuštění brány, nebude zahájen fakturace aplikací brány. 


> [AZURE.NOTE] `Start-AzureApplicationGateway` Rutina může trvat až 15 20 minut. 
   
    PS C:\> Start-AzureApplicationGateway AppGwTest 

    VERBOSE: 7:59:16 PM - Begin Operation: Start-AzureApplicationGateway 
    VERBOSE: 8:05:52 PM - Completed Operation: Start-AzureApplicationGateway
    Name       HTTP Status Code     Operation ID                             Error 
    ----       ----------------     ------------                             ----
    Successful OK                   fc592db8-4c58-2c8e-9a1d-1c97880f0b9b

## <a name="verify-the-gateway-status"></a>Zkontrolujte stav brány

Použití `Get-AzureApplicationGateway` rutina a zkontrolujte stav brány. Pokud *Start AzureApplicationGateway* úspěšný v předchozím kroku, stav by měl být *spuštěný*a Vip a Název_dns by měla být platných položek. Tento příklad ukazuje rutinu na prvním řádku a za ním uveďte výstupu. V tomto příkladu brána je spuštěná a je připravení psát přenosy. 

> [AZURE.NOTE] Aplikační brána je nakonfigurován pro přenosů na nakonfigurováno ILB koncový bod 10.0.0.10 v tomto příkladu.

    PS C:\> Get-AzureApplicationGateway AppGwTest 

    VERBOSE: 8:09:28 PM - Begin Operation: Get-AzureApplicationGateway 
    VERBOSE: 8:09:30 PM - Completed Operation: Get-AzureApplicationGateway
    Name          : AppGwTest
    Description   : 
    VnetName      : testvnet1
    Subnets       : {Subnet-1}
    InstanceCount : 2
    GatewaySize   : Medium
    State         : Running
    VirtualIPs    : {10.0.0.10}
    DnsName       : appgw-b2a11563-2b3a-4172-a4aa-226ee4c23eed.cloudapp.net

## <a name="next-steps"></a>Další kroky


Pokud budete potřebovat další informace o načtení vyrovnávající možnosti obecně, přečtěte si článek:

- [Vyrovnávání zatížení Azure](https://azure.microsoft.com/documentation/services/load-balancer/)
- [Azure přenosy správce](https://azure.microsoft.com/documentation/services/traffic-manager/)
