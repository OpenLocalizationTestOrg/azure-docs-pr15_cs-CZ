<properties
    pageTitle="Pokročilé scénáře s Azure Vícefaktorové ověřování a 3 večírek VPN"
    description="Tato stránka obsahuje informace o konfiguraci podrobné instalačního programu pro Azure MFA s 3 stran produkty."
    services="multi-factor-authentication"
    documentationCenter=""
    authors="kgremban" 
    manager="femila"
    editor="curtland"/>

<tags
    ms.service="multi-factor-authentication"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/04/2016"
    ms.author="kgremban"/>

# <a name="advanced-scenarios-with-azure-multi-factor-authentication-and-3rd-party-vpn"></a>Pokročilé scénáře s Azure Vícefaktorové ověřování a 3 večírek VPN
Azure Vícefaktorové ověřování mohou sloužit k bezproblémové připojení s různými 3 řešení virtuální privátní sítě dodavatelů.  Platí to i Cisco® ASA VPN spotřebiče, spotřebiče Citrix NetScaler SSL VPN a zařízení Juniper sítích zabezpečeného přístupu/Pulse zabezpečené připojení zabezpečené SSL VPN.

## <a name="cisco-asa-vpn-appliance-and-azure-multi-factor-authentication"></a>Zařízení ASA VPN Cisco a Azure Vícefaktorové ověřování
Azure Vícefaktorové ověřování bezproblémové integraci s zařízení Cisco® ASA VPN pro další zabezpečení VPN Cisco AnyConnect® přihlášení a přístup k portálu.  Lze to provést pomocí protokolu LDAP nebo RADIUS.  Vyberte jednu z následujících úkonů stažení příručky podrobné podrobné konfigurace.

Průvodce konfigurací  | Popis
------------- | ------------- |
[Cisco ASA s konfigurací MFA Anyconnect VPN a Azure protokolu LDAP](http://download.microsoft.com/download/A/2/0/A201567C-C3DE-4227-AF89-4567A470899E/Cisco_ASA_Azure_MFA_LDAP.docx) | Bezproblémová integrace s MFA Azure pomocí protokolu LDAP zařízení Cisco ASA VPN|
[Cisco ASA Anyconnect VPN a Azure konfigurace MFA RADIUS](http://download.microsoft.com/download/4/5/7/4579C1CF-35B0-4FBE-8A1A-B49CB2CC0382/Cisco_ASA_Azure_MFA_RADIUS.docx) | Bezproblémová integrace s MFA Azure pomocí RADIUS zařízení Cisco ASA VPN

## <a name="citrix-netscaler-ssl-vpn-and-azure-multi-factor-authentication"></a>Vícefaktorové ověřování Citrix NetScaler SSL VPN a Azure
Azure Vícefaktorové ověřování bezproblémové integraci s zařízení Citrix NetScaler SSL VPN pro další zabezpečení Citrix NetScaler SSL VPN přihlášení a přístup k portálu.  Lze to provést pomocí protokolu LDAP nebo RADIUS.  Vyberte jednu z následujících úkonů stažení příručky podrobné podrobné konfigurace.

Průvodce konfigurací  | Popis
------------- | ------------- |
[Konfigurace MFA Citrix NetScaler SSL VPN a Azure protokolu LDAP](http://download.microsoft.com/download/2/4/E/24E1E722-72DF-471F-A88A-D1338DB1AF83/Citrix_NS_Azure_MFA_LDAP.docx) | Bezproblémová integrace s Azure MFA zařízení pomocí protokolu LDAP vaší Citrix NetScaler SSL VPN|
[Konfigurace MFA Citrix NetScaler SSL VPN a Azure pro RADIUS](http://download.microsoft.com/download/1/A/4/1A482764-4A63-45C2-A5EC-2B673ACCDD12/Citrix_NS_Azure_MFA_RADIUS.docx) | Bezproblémová integrace s MFA Azure pomocí RADIUS zařízení Citrix NetScaler SSL VPN

##<a name="juniperpulse-secure-ssl-vpn-appliance-and-azure-multi-factor-authentication"></a>V takovém Juniper/Pulse zabezpečené SSL VPN a Azure Vícefaktorové ověřování
Azure Vícefaktorové ověřování bezproblémové integraci s zařízení Juniper/Pulse zabezpečené SSL VPN pro další zabezpečení Juniper/Pulse zabezpečené SSL VPN přihlášení a přístup k portálu.  Lze to provést pomocí protokolu LDAP nebo RADIUS.  Vyberte jednu z následujících úkonů stažení příručky podrobné podrobné konfigurace.

Průvodce konfigurací  | Popis
------------- | ------------- |
[Konfigurace MFA Juniper/Pulse zabezpečené SSL VPN a Azure protokolu LDAP](http://download.microsoft.com/download/6/5/8/6587B418-75B1-4FCB-84D4-984BC479309E/JuniperPulse_Azure_MFA_LDAP.docx)| Bezproblémová integrace s zařízení Azure MFA pomocí protokolu LDAP vaší Juniper/Pulse zabezpečené SSL VPN|
[Konfigurace MFA Juniper/Pulse zabezpečené SSL VPN a Azure pro RADIUS](http://download.microsoft.com/download/7/9/A/79AB3DAD-4799-4379-B1DA-B95ABDF231DC/JuniperPulse_Azure_MFA_RADIUS.docx) | Bezproblémová integrace s MFA Azure pomocí RADIUS zařízení Juniper/Pulse zabezpečené SSL VPN
