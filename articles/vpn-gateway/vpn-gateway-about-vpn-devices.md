<properties 
   pageTitle="Informace o VPN zařízení pro připojení VPN brány na webu Azure virtuálních sítí | Microsoft Azure"
   description="Tento článek popisuje počítače v síti VPN IPsec parametry a pro připojení S2S VPN brány a obsahuje odkazy na pokyny ke konfiguraci a ukázky."
   services="vpn-gateway"
   documentationCenter="na"
   authors="yushwang"
   manager="rossort"
   editor=""
  tags="azure-resource-manager, azure-service-management"/>
<tags 
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="09/13/2016"
   ms.author="yushwang;cherylmc" />

# <a name="about-vpn-devices-for-site-to-site-vpn-gateway-connections"></a>Informace o VPN zařízení pro připojení VPN brány na webu

Konfigurace připojení VPN webu (S2S) je potřeba VPN zařízení. Připojení k webu serveru můžete použít k vytvoření hybridního řešení nebo kdykoli chcete zabezpečené připojení mezi místní síti a virtuální sítě. Tento článek popisuje kompatibilní VPN zařízení a konfigurace parametry. 

>[AZURE.NOTE] Konfigurace připojení k webu, IPv4 IP adresa veřejného při potřebné pro zařízení s VPN.                                                                                                                                                                               

Pokud vaše zařízení nezobrazí v tabulce [Ověřit VPN zařízení](#devicetable) , naleznete v části [Ověřit jiné počítače v síti VPN](#additionaldevices) tohoto článku. Je možné, že zařízení může dál pracovat s Azure. Podpora zařízení VPN obraťte se na výrobce zařízení.

**Položky je potřeba pamatovat při prohlížení tabulky:**

- Zjištění změnu terminologie pro statické a dynamické směrování. Budete pravděpodobně nastat obě podmínky. Se stejně jako funkce, názvy pouze měníte.
    - Statické směrování = PolicyBased
    - Dynamické směrování = RouteBased
- Specifikace pro brána vysoký výkon VPN a brána RouteBased VPN jsou stejná, pokud není uvedeno jinak. Ověřený zařízení VPN kompatibilních s RouteBased VPN bran jsou například taky kompatibilní se službou Azure vysoký výkon VPN brány. 


## <a name="devicetable"></a>Ověřený VPN zařízení 

Ověření sadu standardní VPN zařízení ve spolupráci s dodavateli zařízení. Všechna zařízení v zařízení rodiny obsažené v následujícím seznamu měli spolupracovat s Azure VPN brány. V tématu [O Brána VPN](vpn-gateway-about-vpngateways.md) k ověření typ brány, který je potřeba vytvořit řešení, které chcete konfigurovat. 

Vám pomohou při konfiguraci VPN zařízení, podívejte se do odkazy, které odpovídají řady příslušné zařízení. Podpora zařízení VPN obraťte se na výrobce zařízení.



| **Dodavatele**                      | **Zařízení řady**                                        | **Minimální verze operačního systému**                             | **PolicyBased**                                                                                                                                                                                                             | **RouteBased**                                                                                                                                                                    |
|---------------------------------|----------------------------------------------------------|----------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Příbuzných Telesis                  | AR řady VPN směrovači                                    | 2.9.2                                              | Již brzy                                                                                                                                                                                                                                          | Není kompatibilní                                                                                                                                                                                               |
| Barracuda sítí, Inc.        | Brána NextGen Firewall barracuda F-řada             | PolicyBased: 5.4.3 RouteBased: 6.2.0  | [Pokyny ke konfiguraci](https://techlib.barracuda.com/NGF/AzurePolicyBasedVPNGW) | [Pokyny ke konfiguraci](https://techlib.barracuda.com/NGF/AzureRouteBasedVPNGW)                                                                                                                                                                                              |
| Barracuda sítí, Inc.        |  Barracuda NextGen brány Firewall X-řada                 | Barracuda Brána Firewall 6.5 | [Brána Firewall barracuda](https://techlib.barracuda.com/BFW/ConfigAzureVPNGateway) | Není kompatibilní                                                                                                                                                                                               |
| Žakárovém                         | Vyatta 5400 vRouter                                      | Virtuální směrovači 6.6R3 GA                            | [Pokyny ke konfiguraci](http://www1.brocade.com/downloads/documents/html_product_manuals/vyatta/vyatta_5400_manual/wwhelp/wwhimpl/js/html/wwhelp.htm#href=VPN_Site-to-Site%20IPsec%20VPN/Preface.1.1.html)                                       | Není kompatibilní                                                                                                                                                                                               |
| Kontrola bodu                     | Zabezpečení brány                                         | R75.40 R75.40VS                                     | [Pokyny ke konfiguraci](https://supportcenter.checkpoint.com/supportcenter/portal?eventSubmit_doGoviewsolutiondetails=&solutionid=sk101275)                                         | [Pokyny ke konfiguraci](https://supportcenter.checkpoint.com/supportcenter/portal?eventSubmit_doGoviewsolutiondetails=&solutionid=sk101275) |
| Cisco                           | ASA                                                      | 8.3                                                | [Cisco vzorky](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Cisco/Current/ASA)                                                                                                                                                                        | Není kompatibilní                                                                                                                                                                                               |
| Cisco                           | FUNKCI AUTOMATICKÉ OBNOVENÍ SYSTÉMU                                                      | IOS 15.1 (PolicyBased) IOS 15,2 (RouteBased)                | [Cisco vzorky](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Cisco/Current/ASR)                                                                                                                                                                        | [Cisco vzorky](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Cisco/Current/ASR)                                                                                                                                 |
| Cisco                           | ISR                                                      | IOS 15.0 (PolicyBased) IOS 15.1 (RouteBased *)               | [Cisco vzorky](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Cisco/Current/ISR)                                                                                                                                                                        | [Cisco ukázky *](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Cisco/Current/ISR)                                                                                                                                |
| Citrix                          | VPX NetScaler MPX SDX,      |10.1 a vyšší                                           | [Integrace pokyny](https://docs.citrix.com/en-us/netscaler/11-1/system/cloudbridge-connector-introduction/cloudbridge-connector-azure.html)                                                                                                                                                                            | Není kompatibilní                                                                                                                                                                                               |
| Dell SonicWALL                  | TZ série NÁVODU série SuperMassive řadu E-Class NÁVODU řady | SonicOS 5.8.x, [SonicOS 5.9.x](http://documents.software.dell.com/sonicos/5.9/microsoft-azure-configuration-guide/supported-platforms?ParentProduct=850), [SonicOS 6.x](http://documents.software.dell.com/sonicos/6.2/microsoft-azure-configuration-guide/supported-platforms?ParentProduct=646 )          | [Pokyny - SonicOS 6.2](http://documents.software.dell.com/sonicos/6.2/microsoft-azure-configuration-guide?ParentProduct=646) [Pokyny - SonicOS 5.9](http://documents.software.dell.com/sonicos/5.9/microsoft-azure-configuration-guide?ParentProduct=850)                                                                                                                                   | [Pokyny - SonicOS 6.2](http://documents.software.dell.com/sonicos/6.2/microsoft-azure-configuration-guide?ParentProduct=646) [Pokyny - SonicOS 5.9](http://documents.software.dell.com/sonicos/5.9/microsoft-azure-configuration-guide?ParentProduct=850)                                                                                                                                                                                      |
| F5                              | VELKÁ IP řady                                 |           ovládací prvky 12.0                                            | [Pokyny ke konfiguraci](https://devcentral.f5.com/articles/connecting-to-windows-azure-with-the-big-ip)                                                                                                                                                                          | [Pokyny ke konfiguraci](https://devcentral.f5.com/articles/big-ip-to-azure-dynamic-ipsec-tunneling)                                                                                                                                                                                         |
| Fortinet                        | FortiGate                                                | FortiOS 5.2.7                                      | [Pokyny ke konfiguraci](http://docs.fortinet.com/d/fortigate-configuring-ipsec-vpn-between-a-fortigate-and-microsoft-azure)                                                                                                                                                                          | [Pokyny ke konfiguraci](http://docs.fortinet.com/d/fortigate-configuring-ipsec-vpn-between-a-fortigate-and-microsoft-azure)                                                                                                                                  |
| Iniciativy Japonsko Internetu (IIJ) | SEIL řady                                              | SEIL / X 4.60, SEIL/B1 4.60 SEIL/x86 3,20            | [Pokyny ke konfiguraci](http://www.iij.ad.jp/biz/seil/ConfigAzureSEILVPN.pdf)                                                                                                                                                                   | Není kompatibilní                                                                                                                                                                                               |
| Juniper                         | TOMTO DOKUMENTU SE TÝKAJÍ                                                      | JunOS 10.2 (PolicyBased) JunOS 11.4 (RouteBased)            | [Juniper vzorky](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Juniper/Current/SRX)                                                                                                                                                                      | [Juniper vzorky](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Juniper/Current/SRX)                                                                                                                              |
| Juniper                         | J řady                                                 | JunOS 10.4r9 (PolicyBased) JunOS 11.4 (RouteBased)          | [Juniper vzorky](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Juniper/Current/JSeries)                                                                                                                                                                 | [Juniper vzorky](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Juniper/Current/JSeries)                                                                                                                         |
| Juniper                         | ISG                                                      | ScreenOS 6.3 (PolicyBased a RouteBased)                  | [Juniper vzorky](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Juniper/Current/ISG)                                                                                                                                                                      | [Juniper vzorky](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Juniper/Current/ISG)                                                                                                                              |
| Juniper                         | SSG                                                      | ScreenOS 6.2 (PolicyBased a RouteBased)                  | [Juniper vzorky](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Juniper/Current/SSG)                                                                                                                                                                      | [Juniper vzorky](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Juniper/Current/SSG)                                                                                                                              |
| Microsoft                       | Služba Směrování a vzdálený přístup                        | Windows Server 2012                                | Není kompatibilní                                                                                                                                                                                                                                       | [Microsoft vzorky](http://go.microsoft.com/fwlink/p/?LinkId=717761)                                                                                           |
| Otevřít AG systémy | Ovládací prvek mise zabezpečení brány | NENÍ K DISPOZICI | [Instalace Průvodce](https://www.open.ch/_pdf/Azure/AzureVPNSetup_Installation_Guide.pdf) | [Instalace Průvodce](https://www.open.ch/_pdf/Azure/AzureVPNSetup_Installation_Guide.pdf) |
| Openswan                        | Openswan                                                 | 2.6.32                                             | (Už brzo)                                                                                                                                                                                                                                        | Není kompatibilní                                                                                                                                                                                               |
| Palo Alto sítí              | Všechna zařízení POSOUVÁNÍ operační systém     | POSOUVÁNÍ OS 6.1.5 nebo novější (PolicyBased) POSOUVÁNÍ OS 7.0.5 nebo novější (RouteBased)       | [Pokyny ke konfiguraci]( https://live.paloaltonetworks.com/t5/Configuration-Articles/How-to-Configure-VPN-Tunnel-Between-a-Palo-Alto-Networks/ta-p/59065)                                                                                                                                                                                          | [Pokyny ke konfiguraci](https://live.paloaltonetworks.com/t5/Integration-Articles/Configuring-IKEv2-VPN-for-Microsoft-Azure-Environment/ta-p/60340)                                                                                                                                                                                    |
| WatchGuard                      | Všechny                                                      | Fireware XTM v11.x                                 | [Pokyny ke konfiguraci](http://customers.watchguard.com/articles/Article/Configure-a-VPN-connection-to-a-Windows-Azure-virtual-network/)                                                                                                                                                                          | Není kompatibilní                                                                                                                                                                                               |

(*) Řada 7200 ISR směrovači podporují pouze PolicyBased VPN.

## <a name="additionaldevices"></a>Ověřit jiné zařízení VPN

Pokud nevidíte zařízení uvedené v tabulce zařízení ověřit VPN, bude možná pracovat s připojením k webu. Ověřte, že VPN zařízení splňuje minimální požadavky uvedené v oddílu požadavky brány v článku [O bran VPN](vpn-gateway-about-vpngateways.md#gateway-requirements) . Zařízení splňuje minimální požadavky by také použít i s VPN brány. Výrobce zařízení další podporu a konfigurace pokyny.


## <a name="editing-device-configuration-samples"></a>Úpravy zařízení konfigurace vzorky

Po stažení ukázku konfigurace zařízení ujednaných VPN, musíte nahradit některé hodnoty tak, aby odrážely nastavení prostředí. 

**Chcete-li upravit výběru:**

1. Otevřete ukázkový poznámkový blok. 
1. Hledání a nahrazení všechny <*text*> řetězce s hodnotami, které se týkají prostředí. Nezapomeňte < a >. Pokud není zadán název, musí být jedinečný název, který jste vybrali. Pokud příkaz nefunguje, naleznete v dokumentaci výrobce zařízení.

| **Ukázkový text**                | **Změna na**                                                                                                        |
|----------------------------------|----------------------------------------------------------------------------------------------------------------------|
| &lt;RP_OnPremisesNetwork&gt;           | Zvolený názvu pro tento objekt. Příklad: myOnPremisesNetwork                                                       |
| &lt;RP_AzureNetwork&gt;                | Zvolený názvu pro tento objekt. Příklad: myAzureNetwork                                                            |
| &lt;RP_AccessList&gt;                  | Zvolený názvu pro tento objekt. Příklad: myAzureAccessList                                                         |
| &lt;RP_IPSecTransformSet&gt;           | Zvolený názvu pro tento objekt. Příklad: myIPSecTransformSet                                                       |
| &lt;RP_IPSecCryptoMap&gt;              | Zvolený názvu pro tento objekt. Příklad: myIPSecCryptoMap                                                          |
| &lt;SP_AzureNetworkIpRange&gt;         | Určete rozsah. Příklad: 192.168.0.0                                                                                  |
| &lt;SP_AzureNetworkSubnetMask&gt;      | Zadejte podsítě masku. Příklad: 255.255.0.0                                                                            |
| &lt;SP_OnPremisesNetworkIpRange&gt;    | Určete rozsah místní. Příklad: 10.2.1.0                                                                         |
| &lt;SP_OnPremisesNetworkSubnetMask&gt; | Zadejte místní podsítě masku. Příklad: 255.255.255.0                                                              |
| &lt;SP_AzureGatewayIpAddress&gt;       | Tyto informace specifické pro virtuální sítě a je umístěn v portálu pro správu jako **Adresa brány**. |
| &lt;SP_PresharedKey&gt;                | Tyto informace jsou specifické pro virtuální sítě a je umístěn v portálu pro správu jako spravovat klíč.          |



## <a name="ipsec-parameters"></a>IPsec parametry

>[AZURE.NOTE] I když hodnoty uvedené v následující tabulce jsou podporovány Brána VPN Azure, aktuálně nejde žádným způsobem můžete zadat nebo vybrat konkrétní kombinaci z Azure VPN brány. Je nutné zadat omezeními z místního VPN zařízení. Kromě toho musí podpěře MSS na 1350.

### <a name="ike-phase-1-setup"></a>Nastavení IKE fázi 1

| **Vlastnost**                                       | **PolicyBased** | **RouteBased a standardní nebo vysoký výkon VPN brány** |
|----------------------------------------------------|--------------------------------|------------------------------------------------------------------|
| IKE verze                                        | IKEv1                          | IKEv2                                                            |
| Skupina kompromitovat                               | Skupina 2 (1 024 bitů)             | Skupina 2 (1 024 bitů)                                               |
| Metody ověřování                              | Předem sdílený klíč                 | Předem sdílený klíč                                                   |
| Šifrování algoritmů                              | AES256 AES128 3DES             | AES256 3DES                                                      |
| Algoritmus hash                                  | SHA1(SHA128)                   | SHA1(SHA128) SHA2(SHA256)                                                     |
| Fázi 1 zabezpečení přidružení životnost (čas) | 28 800 sekund                 | 10,800 sekund                                                   |


### <a name="ike-phase-2-setup"></a>Nastavení IKE fázi 2

| **Vlastnost**                                                             | **PolicyBased**                 | **RouteBased a standardní nebo vysoký výkon VPN brány**   |
|--------------------------------------------------------------------------|------------------------------------------------|--------------------------------------------------------------------|
| IKE verze                                                              | IKEv1                                          | IKEv2                                                              |
| Algoritmus hash                                                        | SHA1(SHA128)                                   | SHA1(SHA128)                                                       |
| Fáze 2 zabezpečení přidružení životnost (čas)                        | 3600 sekund                                  | 3600 sekund                                                                  |
| Fáze 2 zabezpečení přidružení životnost (výkon)                  | 102,400,000 ZNALOSTNÍ BÁZI KNOWLEDGE BASE                                 | -                                                                  |
| Šifrování správce systému IPsec & ověřování nabízí (v pořadí předvoleb) | 1. ESP AES256 2. ESP AES128 3. ESP 3DES 4. NENÍ K DISPOZICI | V tématu *RouteBased brány IPsec zabezpečení přidružení nabízí* (viz níže) |
| Perfektní PFS (přeposílání)                                            | Ne                                             | Žádné (*)|
| Mrtvých zjišťování mezi dvěma účastníky                                                      | Nepodporovaná funkce                                  | Podporované                                                          |

(*) Azure brány jako odpovídající IKE zařízení jsou přijatelné PFS DH skupiny 1, 2, 5, 14, 24.

### <a name="routebased-gateway-ipsec-security-association-sa-offers"></a>Nabízí RouteBased brány IPsec zabezpečení přidružení

V následující tabulce jsou uvedeny šifrování IPsec správce systému ověřování nabízí. Nabídky jsou uvedeny směru předvoleb, že je nabídka prezentovány nebo přijetím odvolat.

| **Přidružení šifrování a nabídky k ověření** | **Azure brány jako autora**                               | **Azure brány jako odpovídající zařízení**                                   |
|---------------------------------------------------|--------------------------------------------------------------|--------------------------------------------------------------|
| 1                                                 | ESP AES_256 SHA                                              | ESP AES_128 SHA                                              |
| 2                                                 | ESP AES_128 SHA                                              | ESP 3_DES MD5                                                |
| 3                                                 | ESP 3_DES MD5                                                | ESP 3_DES SHA                                                |
| 4                                                 | ESP 3_DES SHA                                                | AH SHA1 s ESP AES_128 s hodnotou null HMAC                      |
| 5                                                 | AH SHA1 s ESP AES_256 s hodnotou null HMAC                      | AH SHA1 s ESP 3_DES s hodnotou null HMAC                        |
| 6                                                 | AH SHA1 s ESP AES_128 s hodnotou null HMAC                      | AH MD5 s ESP 3_DES s null HMAC, bez životnost navrhované |
| 7                                                 | AH SHA1 s ESP 3_DES s hodnotou null HMAC                        | AH SHA1 s ESP 3_DES SHA1 žádné životnost                    |
| 8                                                 | AH MD5 s ESP 3_DES s null HMAC, bez životnost navrhované | AH MD5 s ESP 3_DES MD5 žádné životnost                     |
| 9                                                 | AH SHA1 s ESP 3_DES SHA1 žádné životnost                    | ESP DES MD5                                                  |
| 10                                                | AH MD5 s ESP 3_DES MD5 žádné životnost                     | ESP DES SHA1 žádné životnost                                   |
| 11                                                | ESP DES MD5                                                  | AH SHA1 s ESP DES null HMAC žádné životnost navrhované        |
| 12                                                | ESP DES SHA1 žádné životnost                                   | AH MD5 s ESP DES null HMAC žádné životnost navrhované        |
| 13                                                | AH SHA1 s ESP DES null HMAC žádné životnost navrhované        | AH SHA1 s ESP DES SHA1 žádné životnost                      |
| 14                                                | AH MD5 s ESP DES null HMAC žádné životnost navrhované        | AH MD5 s ESP DES MD5 žádné životnost                       |
| 15                                                | AH SHA1 s ESP DES SHA1 žádné životnost                      | ESP SHA žádné životnost                                        |
| 16                                                | AH MD5 s ESP DES MD5 žádné životnost                       | ESP MD5 žádné životnost                                        |
| 17                                                | -                                                            | AH SHA, bez životnost                                         |
| 18                                                | -                                                            | AH MD5 žádné životnost                                        |


- Zadejte IPsec ESP NULL šifrování se RouteBased a vysoký výkon VPN brány. Null šifrování na základě neposkytuje ochranu data při přenosu šifrovaná a lze používat pouze při maximálního výkonu a minimální latence je povinný.  Klienti může rozhodnete sdělit nám to v scénáře komunikace VNet VNet nebo šifrování se nepoužijí jinde v řešení.

- Křížové místní připojení přes Internet pomocí nastavení výchozí Azure VPN brány šifrování a algoritmy hash uvedené v části tabulky nad zajistit bezpečnost vaší kritické komunikace.





