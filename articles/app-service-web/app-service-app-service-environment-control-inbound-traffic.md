<properties 
    pageTitle="Jak určit příchozích prostředí aplikace služby" 
    description="Informace o tom, jak konfigurovat sítě zabezpečení pravidla pro ovládací prvek příchozích prostředí aplikace služby." 
    services="app-service" 
    documentationCenter="" 
    authors="ccompy" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="app-service" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/02/2016" 
    ms.author="stefsch"/>   

# <a name="how-to-control-inbound-traffic-to-an-app-service-environment"></a>Jak určit příchozích prostředí aplikace služby

## <a name="overview"></a>Základní informace ##
Prostředí aplikace služby dá vytvořit v **obou** virtuální sítě Správce prostředků Azure, **nebo** na klasické nasazení modelu [virtuální sítě][virtualnetwork].  Nová virtuální síť a nové podsítě je to možné definovat v době, kdy se vytvoří prostředí aplikace služby.  Prostředí aplikace služby nebo můžete vytvořit ve stávajícím virtuální sítě a stávajících podsítě.  Poslední změny provedené v červnu 2016 můžete ASEs teď nasadit do virtuálního sítích, které používají veřejná adresa oblasti nebo RFC1918 adresu mezer (tedy soukromé adresy).  Další informace o vytváření prostředí aplikace služby najdete v článku [jak vytvořit prostředí aplikace služby][HowToCreateAnAppServiceEnvironment].

Prostředí aplikace služby musí vždy vytvořit v rámci podsítě, protože podsítě poskytuje hranice sítě, kterou lze použít k uzamknout příchozích za nadřazeného zařízení a služby tak, aby se přenosy protokolu HTTP a HTTPS nebude přijatá jenom z konkrétní nadřazeného IP adres.

Příchozí a odchozí v síti na podsítě řídí používání [skupiny zabezpečení sítě][NetworkSecurityGroups]. Řízení příchozích vyžaduje vytváření pravidla zabezpečení síť ve skupině zabezpečení síť a potom přiřazování zabezpečení sítě seskupení podsítě obsahující prostředí aplikace služeb.

Po sítě skupina zabezpečení má přiřazenou do podsítě, příchozí data do aplikací v prostředí služby aplikace je povolených a blokovaných založené na seznamu Akce povolené a odepřít pravidel definovaných ve skupině zabezpečení síť.

[AZURE.INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)] 

## <a name="network-ports-used-in-an-app-service-environment"></a>Síťové porty používané v prostředí aplikace služby ##
Před uzamčení příchozí v síti s skupinu zabezpečení síti, je důležité vědět sadu povinných a nepovinných síťové porty používá prostředí aplikace služby.  Omylem zavření vypnout umožnění datových přenosů do některé porty můžete mít za následek ztrátě funkčnosti v prostředí aplikace služby.

Následuje seznam porty používané prostředí aplikace služby. Pokud není uvedeno jinak jasně jsou všechny porty **TCP**:

- 454: **povinné port** používaný Azure infrastrukturu pro správu a Správa aplikací služby prostředí prostřednictvím SSL.  Umožnění datových přenosů do tento port nebudou blokovat.  Tento port je vždy vázaný na veřejné VIP pomocného mechanismu řízení.
- 455: **povinné port** používaný Azure infrastrukturu pro správu a Správa aplikací služby prostředí prostřednictvím SSL.  Umožnění datových přenosů do tento port nebudou blokovat.  Tento port je vždy vázaný na veřejné VIP pomocného mechanismu řízení.
- 80: výchozí port pro příchozí přenosy protokolu HTTP do aplikace spuštěné v aplikaci služby plány jednotného zasílání zpráv v prostředí aplikace služby.  Na pomocný podporou ILB tento port svázán s adresu ILB pomocného mechanismu řízení.
- 443: výchozí port pro příchozí SSL umožnění datových přenosů do aplikace spuštěné v aplikaci služby plány jednotného zasílání zpráv v prostředí aplikace služby.  Na pomocný podporou ILB tento port vázaný na adresu ILB pomocného mechanismu řízení.
- 21: řízení kanál FTP.  Tento port můžete bezpečně blokovány, pokud není použitý FTP.  Na pomocný podporou ILB tento port vázat adresu ILB pro pomocného mechanismu řízení.
- 990: řízení kanál FTPS.  Tento port můžete bezpečně blokovány, pokud není použitý FTPS.  Na pomocný podporou ILB tento port vázat adresu ILB pro pomocného mechanismu řízení.
- 10001 10020: datových kanálů FTP.  Stejně jako u kanálu ovládací prvek tyto porty můžete bezpečně blokovány, pokud není použitý FTP.  Na pomocný podporou ILB můžete tento port vázaná pomocného mechanismu řízení ILB adresu.
- 4016: slouží k vzdálené ladění s Visual Studio 2012.  Tento port můžete bezpečně blokován, pokud tato funkce není použitý.  Na pomocný podporou ILB tento port svázán s adresu ILB pomocného mechanismu řízení.
- 4018: slouží k vzdálené ladění s Visual Studio 2013.  Tento port můžete bezpečně blokovány, pokud tato funkce není použitý.  Na pomocný podporou ILB tento port vázaná na adresu ILB pomocného mechanismu řízení.
- 4020: slouží k vzdálené ladění s Visual Studio 2015.  Tento port můžete bezpečně blokovány, pokud tato funkce není použitý.  Na pomocný podporou ILB tento port svázán s adresu ILB pomocného mechanismu řízení.

## <a name="outbound-connectivity-and-dns-requirements"></a>Odchozí připojení a DNS požadavky ##
Prostředí aplikace služby budou fungovat správně vyžaduje odchozí přístup k různým koncové body. Úplný seznam externí koncové body používá pomocným je v části "Vyžaduje připojení k síti" v článku [Konfigurace sítě pro ExpressRoute](app-service-app-service-environment-network-configuration-expressroute.md#required-network-connectivity) .

Aplikace služby prostředí vyžaduje konfiguraci pro virtuální sítě platné infrastruktury služby DNS.  Pokud z nějakého důvodu dojde ke změně konfigurace DNS po vytvoření prostředí aplikace služby, můžete vynutit vývojáře prostředí aplikace služby vyberte nové konfigurace DNS.  Spuštění postupné restartování prostředí pomocí ikony "Restartujte" umístěný v horní části zásuvné správy prostředí aplikace služby [Azure portálu] [ NewPortal] způsobí prostředí vystopovat nové konfigurace DNS.

Doporučujeme také, že všechny vlastní serverů DNS na vnet být instalace předem před vytvořením prostředí aplikace služby.  Pokud konfigurace DNS virtuální sítě se změní při vytváření prostředí aplikace služby, takže, výsledkem bude selhání proces vytváření prostředí aplikace služeb.  V souvislosti podobné vlastní server DNS existuje na druhou stranu brány VPN a DNS server je dostupný nebo nedostupné, znamená to proces vytváření prostředí aplikace služeb také nezdaří.

## <a name="creating-a-network-security-group"></a>Vytvoření skupiny zabezpečení sítě ##
Úplné informace o tom, jak fungují skupiny zabezpečení sítě tyto [informace]najdete v článku[NetworkSecurityGroups].  Podrobnosti o dotykové ovládání na zvýrazní skupin zabezpečení síť s fokusem na konfigurace a používání skupinu zabezpečení sítě podsítě obsahující prostředí aplikace služby.

**Poznámka:** Skupiny zabezpečení sítě možné konfigurovat graficky pomocí [Portálu Azure](https://portal.azure.com) nebo prostřednictvím Powershellu Azure.

Skupiny zabezpečení sítě vzniká nejdřív jako samostatný entity přidružené k přihlášení k odběru. Vzhledem k tomu, že skupin zabezpečení sítě vytvořené v Azure oblasti, ujistěte se, vytvořené skupiny zabezpečení síť ve stejné oblasti jako prostředí aplikace služeb.

Následující ukazuje, vytvořit skupinu zabezpečení sítě:

    New-AzureNetworkSecurityGroup -Name "testNSGexample" -Location "South Central US" -Label "Example network security group for an app service environment"

Po vytvoření skupiny zabezpečení sítě jsou do ní přidat jeden nebo více pravidel zabezpečení sítě.  Protože sady pravidel v průběhu času mění, doporučujeme prostoru se používá pro priority pravidel pro snadnou vložení pravidla nehledá v čase schématu číslování.

Následující příklad ukazuje pravidlo, které explicitně povolí přístup k porty pro správu potřeby tak, že Azure infrastruktury spravovat a udržovat prostředí aplikace služby.  Poznámka: všechny přenosy správy přecházel přes připojení SSL je zabezpečená cenných papírů klienta, tak i když jsou tyto porty otevřené jsou nedostupné tak, že každý subjekt než infrastruktura Azure správy.


    Get-AzureNetworkSecurityGroup -Name "testNSGexample" | Set-AzureNetworkSecurityRule -Name "ALLOW AzureMngmt" -Type Inbound -Priority 100 -Action Allow -SourceAddressPrefix 'INTERNET'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '454-455' -Protocol TCP
    

Když uzamčení přístup k porty 80 a 443 "Skrýt" prostředí aplikace služby za nadřazeného zařízení nebo služeb, je potřeba vědět před IP adresu.  Například, pokud používáte bránu firewall webové aplikace (WAF), WAF bude mít vlastní IP adresy (nebo adresy) kterých se používá při proxy umožnění datových přenosů do podřízené prostředí aplikace služeb.  Musíte používat tuto IP adresu v parametru *SourceAddressPrefix* pravidlo zabezpečení sítě.

V následujícím příkladu je explicitně povolen příchozích z konkrétní nadřazeného IP adresy.  Adresa *1.2.3.4* slouží jako zástupný symbol na IP adresu nadřazeného WAF.  Změňte hodnotu podle adresa použitá nadřazeného zařízení nebo službou.

    Get-AzureNetworkSecurityGroup -Name "testNSGexample" | Set-AzureNetworkSecurityRule -Name "RESTRICT HTTP" -Type Inbound -Priority 200 -Action Allow -SourceAddressPrefix '1.2.3.4/32'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '80' -Protocol TCP
    Get-AzureNetworkSecurityGroup -Name "testNSGexample" | Set-AzureNetworkSecurityRule -Name "RESTRICT HTTPS" -Type Inbound -Priority 300 -Action Allow -SourceAddressPrefix '1.2.3.4/32'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '443' -Protocol TCP
    
Pokud budete chtít FTP podpora následujících pravidel lze jako šablonu udělit přístup k FTP řídicí port a datový kanál porty.  Protože FTP stavového Protocol (protokol), nebude možné směrovat přenosy v síti FTP přes tradiční zařízení brány firewall nebo proxy protokolu HTTP/HTTPS.  V tomto případě se musíte nastavit *SourceAddressPrefix* jinou hodnotu – třeba rozsah IP adres vývojář nebo nasazení počítačích, na které FTP klientských počítačích. 

    Get-AzureNetworkSecurityGroup -Name "testNSGexample" | Set-AzureNetworkSecurityRule -Name "RESTRICT FTPCtrl" -Type Inbound -Priority 400 -Action Allow -SourceAddressPrefix '1.2.3.4/32'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '21' -Protocol TCP
    Get-AzureNetworkSecurityGroup -Name "testNSGexample" | Set-AzureNetworkSecurityRule -Name "RESTRICT FTPDataRange" -Type Inbound -Priority 500 -Action Allow -SourceAddressPrefix '1.2.3.4/32'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '10001-10020' -Protocol TCP

(**Poznámka:** oblasti dat kanál port může změnit období náhledu.)

Pokud vzdálené ladění pomocí aplikace Visual Studio slouží následující pravidla demonstrují udělit přístup.  Od každého verze používá jiný port pro vzdálené ladění je samostatném pravidlo pro každou podporovanou verzi aplikace Visual Studio.  Stejně jako u přístup prostřednictvím protokolu FTP vzdálené ladění přenosy nemusí postupují správně tradiční WAF nebo zařízení proxy serveru.  *SourceAddressPrefix* můžete nastavit místo toho rozsah IP adres vývojář počítače se systémem Visual Studio.

    Get-AzureNetworkSecurityGroup -Name "testNSGexample" | Set-AzureNetworkSecurityRule -Name "RESTRICT RemoteDebuggingVS2012" -Type Inbound -Priority 600 -Action Allow -SourceAddressPrefix '1.2.3.4/32'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '4016' -Protocol TCP
    Get-AzureNetworkSecurityGroup -Name "testNSGexample" | Set-AzureNetworkSecurityRule -Name "RESTRICT RemoteDebuggingVS2013" -Type Inbound -Priority 700 -Action Allow -SourceAddressPrefix '1.2.3.4/32'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '4018' -Protocol TCP
    Get-AzureNetworkSecurityGroup -Name "testNSGexample" | Set-AzureNetworkSecurityRule -Name "RESTRICT RemoteDebuggingVS2015" -Type Inbound -Priority 800 -Action Allow -SourceAddressPrefix '1.2.3.4/32'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '4020' -Protocol TCP

## <a name="assigning-a-network-security-group-to-a-subnet"></a>Přiřadit podsítě síťové skupiny zabezpečení ##
Skupiny zabezpečení sítě má výchozí zabezpečení pravidlo, které odepřít přístup ke všem externím přenosy.  Výsledkem kombinace pravidel sítě zabezpečení jsme je popsali výše a výchozí pravidlo zabezpečení blokuje příchozí data, je tento pouze přenos z adresy zdroje, které oblastí přidružený k akci *Povolit* budou moct směrování přenosů na aplikace spuštěné v prostředí aplikace služby.

Po skupinu zabezpečení sítě je vyplněn pravidla zabezpečení, je nutné přidělovat podsítě obsahující prostředí aplikace služeb.  Příkaz přiřazení odkazuje na název virtuální sítě, kde jsou uložená prostředí aplikace služeb, jak název podsítě které byl vytvořen prostředí aplikace služeb.  

V následujícím příkladu skupinu zabezpečení sítě jsou přiřazené podsítě a virtuální sítě:


    Get-AzureNetworkSecurityGroup -Name "testNSGexample" | Set-AzureNetworkSecurityGroupToSubnet -VirtualNetworkName 'testVNet' -SubnetName 'Subnet-test'

Po přiřazení Skupina zabezpečení sítě se mu (přiřazení je dlouhodobě spuštěných operací a může trvat několik minut) jen příchozích pravidel *Povolit* úspěšně dosáhne aplikací v prostředí aplikace služeb.

Pro dokončení následující příklad ukazuje, jak odebrat a proto dis-přidružení skupiny zabezpečení síť z podsítě:


    Get-AzureNetworkSecurityGroup -Name "testNSGexample" | Remove-AzureNetworkSecurityGroupFromSubnet -VirtualNetworkName 'testVNet' -SubnetName 'Subnet-test'

## <a name="special-considerations-for-explicit-ip-ssl"></a>Důležité informace pro explicitní IP SSL ##
Pokud aplikace nakonfigurována explicitní SSL IP adresy (platí pro ASEs, kteří mají veřejné VIP), nepoužívejte výchozí IP adresu prostředí aplikace služeb HTTP a HTTPS provoz se přesune do podsítě přes jinou sadu porty než porty 80 a 443.

Jednotlivé pár porty tak, že každé IP SSL adresy používané najdete v portálu uživatelském rozhraní z prostředí aplikace služeb podrobnosti o činnosti koncového uživatele zásuvné.  Vyberte "všechna nastavení"--> "IP adresy".  Zásuvné "IP adresy" je zobrazena tabulka všechny adresy IP SSL explicitně konfigurované prostředí aplikace služeb, spolu s pár speciální port, který slouží ke směrování HTTP a HTTPS zatížení související s každé adresy IP SSL.  Je tento port dvojici, kterou je potřeba při použití parametrů DestinationPortRange konfigurace pravidel ve skupině zabezpečení sítě.

Když aplikace na pomocného mechanismu řízení je nakonfigurovaný na používání IP SSL, externí zákazníky neuvidíte a nemusíte bát mapování dvojici speciální port.  Umožnění datových přenosů do aplikace se obvykle flow nakonfigurované adresy IP SSL.  Překlad ke spárování speciální port automaticky stane interně během konečný nohy směrování přenosu do podsítě obsahující pomocného mechanismu řízení. 

## <a name="getting-started"></a>Začínáme

Začínáme s prostředím služby aplikace, najdete v článku [Úvod do prostředí aplikace služby][IntroToAppServiceEnvironment]

Všechny články a jak-pro uživatele pro aplikaci služby prostředí jsou k dispozici v [souboru README pro aplikaci služby prostředí](../app-service/app-service-app-service-environments-readme.md).

Podrobnosti tipy k aplikacím zabezpečené připojení k back-end zdroje z prostředí aplikace služby najdete v tématu [zabezpečené připojení k prostředkům back-end z prostředí aplikace služby][SecurelyConnecttoBackend]

Další informace o aplikaci služby Azure platformy, najdete v článku [Aplikace služby Azure][AzureAppService].

[AZURE.INCLUDE [app-service-web-whats-changed](../../includes/app-service-web-whats-changed.md)]

[AZURE.INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]

<!-- LINKS -->
[virtualnetwork]: https://azure.microsoft.com/documentation/articles/virtual-networks-faq/
[HowToCreateAnAppServiceEnvironment]: http://azure.microsoft.com/documentation/articles/app-service-web-how-to-create-an-app-service-environment/
[NetworkSecurityGroups]: https://azure.microsoft.com/documentation/articles/virtual-networks-nsg/
[AzureAppService]: http://azure.microsoft.com/documentation/articles/app-service-value-prop-what-is/
[IntroToAppServiceEnvironment]:  http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-intro/
[SecurelyConnecttoBackend]:  http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-securely-connecting-to-backend-resources/
[NewPortal]:  https://portal.azure.com  

<!-- IMAGES -->
 
