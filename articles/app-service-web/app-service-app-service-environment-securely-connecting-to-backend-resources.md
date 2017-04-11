<properties 
    pageTitle="Zabezpečené připojení k prostředkům back-end z prostředí aplikace služby" 
    description="Informace o tom, aby zajistila zabezpečené připojení k prostředkům back-end z prostředí aplikace služby." 
    services="app-service" 
    documentationCenter="" 
    authors="stefsch" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="app-service" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/04/2016" 
    ms.author="stefsch"/>   

# <a name="securely-connecting-to-backend-resources-from-an-app-service-environment"></a>Zabezpečené připojení k prostředkům back-end z prostředí aplikace služby #

## <a name="overview"></a>Základní informace ##
Protože prostředí služby aplikace se vždy vytvoří v **obou** virtuální sítě Správce prostředků Azure, **nebo** na klasické nasazení modelu [virtuální sítě][virtualnetwork], odchozí připojení z prostředí aplikace služby k jiným zdrojům back-end můžete tok výhradně přes virtuální síť.  Poslední změny provedené v červnu 2016 můžete být nasazené ASEs do virtuálního sítích, které používají veřejná adresa oblasti nebo RFC1918 adresu mezer (tedy soukromé adresy).  

Například může být SQL Server výpočetnímu clusteru virtuálních počítačích s portem 1433 uzamknout.  Koncový bod může být ACLd pouze přístupu z jiných zdrojů na stejné virtuální sítě.  

Jiný příklad citlivé koncové body může běžet místní a být připojeni k Azure prostřednictvím některém z [Webů webu] [ SiteToSite] nebo [Azure ExpressRoute] [ ExpressRoute] připojení.  Pouze zdrojů ve virtuálních sítí připojení k webu na webu nebo ExpressRoute tunelů jako výsledek, bude mít přístup k místním koncové body.

U všech podobnému sledu aplikace spuštěné v prostředí aplikace služby bude moct zabezpečené připojení k různých servery a prostředky.  Odchozí přenosy z aplikace spuštěna v prostředí aplikace služby do soukromé koncové body ve stejné síti virtuální (nebo připojení k stejné virtuální sítě), bude pouze toku přes virtuální sítě.  Odchozí komunikaci do soukromé koncové body nebude tok veřejné Internetu.

Jeden výstrahou platí pro odchozí přenosy z prostředí aplikace služby pro koncové body v rámci virtuální síť.  Aplikaci služby prostředí nelze kontaktovat koncové body virtuálních počítačích umístěny ve **stejné** podsítě jako prostředí aplikace služeb.  To obvykle neměli jít o problém, dokud aplikace služby prostředí jsou nasazeny do podsítě vyhrazená využívat prostředím pouze aplikaci služby.

[AZURE.INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)] 

## <a name="outbound-connectivity-and-dns-requirements"></a>Odchozí připojení a požadavky DNS ##
Prostředí aplikace služby budou fungovat správně vyžaduje odchozí přístup k různým koncové body. Úplný seznam externí koncové body používá pomocným je v části "Vyžaduje připojení k síti" v článku [Konfigurace sítě pro ExpressRoute](app-service-app-service-environment-network-configuration-expressroute.md#required-network-connectivity) .

Aplikace služby prostředí vyžaduje konfiguraci pro virtuální sítě platné infrastruktury služby DNS.  Pokud z nějakého důvodu dojde ke změně konfigurace DNS po vytvoření prostředí aplikace služby, můžete vynutit vývojáři prostředí aplikace služby vyberte nové konfigurace DNS.  Spuštění postupné restartování prostředí pomocí ikony "Restart" umístěný v horní části zásuvné správy prostředí aplikace služby na portálu způsobí prostředí vystopovat nové konfigurace DNS.

Doporučujeme také, že všechny vlastní serverů DNS na vnet být nastavení předem před vytvořením prostředí aplikace služby.  Pokud konfigurace DNS virtuální sítě se změní při vytváření prostředí aplikace služby, takže, výsledkem bude selhání proces vytváření prostředí aplikace služeb.  V souvislosti podobné vlastní server DNS existuje na druhou stranu brány VPN a DNS server je dostupný nebo nedostupné, znamená to proces vytváření prostředí aplikace služeb také nezdaří.

## <a name="connecting-to-a-sql-server"></a>Připojení k serveru SQL Server
Běžné konfigurace systému SQL Server má přijímá požadavky na port 1433 koncový bod:

![Koncový bod SQL serveru][SqlServerEndpoint]

K omezení přenosy tento koncový bod dvěma způsoby:


- [Sítě seznamy řízení přístupu] [ NetworkAccessControlLists] (síťový ACL)

- [Skupiny zabezpečení sítě][NetworkSecurityGroups]


## <a name="restricting-access-with-a-network-acl"></a>Omezení přístupu k síti ACL

Port 1433 lze zabezpečit pomocí seznamu řízení přístupu sítě.  V příkladu níže povolených programů klienta adresy pocházející z uvnitř virtuální sítě a blokuje přístup k jiných klientech.

![Příklad seznamu řízení přístupu sítě][NetworkAccessControlListExample]

Veškeré spuštěné v prostředí služby aplikace ve stejné síti virtuální jako SQL Server bude moct připojit k instanci systému SQL Server pomocí IP adresy **VNet interní** virtuálního počítače SQL serveru.  

Příklad připojovací řetězec dole odkazuje pomocí své soukromé IP adresu serveru SQL Server.

    Server=tcp:10.0.1.6;Database=MyDatabase;User ID=MyUser;Password=PasswordHere;provider=System.Data.SqlClient

I když virtuální počítač má veřejné koncový bod stejně, pokusy o připojení pomocí veřejné adresy IP odmítnuta kvůli sítě ACL. 

## <a name="restricting-access-with-a-network-security-group"></a>Omezení přístupu ke skupině zabezpečení sítě
Alternativní přístup k zabezpečení aplikace access se skupinu zabezpečení sítě.  Skupiny zabezpečení sítě se dají použít k jednotlivým virtuálních počítačích a podsítě obsahující virtuálních počítačích.

Skupiny zabezpečení sítě musí nejdříve vytvořit:

    New-AzureNetworkSecurityGroup -Name "testNSGexample" -Location "South Central US" -Label "Example network security group for an app service environment"

Omezení přístupu k pouze VNet interní přenos je velmi jednoduché s skupinu zabezpečení sítě.  Výchozí pravidla ve skupině zabezpečení sítě pouze povolit přístup z ostatních síťových klientů ve stejné síti virtuální.

Výsledkem uzamčení přístup k serveru SQL Server je jednoduchá – stačí skupiny zabezpečení síť s jeho výchozí pravidla podmíněného formátování, které buď virtuálních počítačích systém SQL Server nebo podsítě obsahující virtuálních počítačích.

Následující ukázce se týká skupinu zabezpečení sítě obsahující podsítě:

    Get-AzureNetworkSecurityGroup -Name "testNSGExample" | Set-AzureNetworkSecurityGroupToSubnet -VirtualNetworkName 'testVNet' -SubnetName 'Subnet-1'
    
V konečném důsledku je sady pravidel zabezpečení, které blokují externí přístup zároveň VNet interní access:

![Výchozí sítě zabezpečení pravidla][DefaultNetworkSecurityRules]


## <a name="getting-started"></a>Začínáme
Všechny články a jak-pro uživatele pro aplikaci služby prostředí jsou k dispozici v [souboru README pro aplikaci služby prostředí](../app-service/app-service-app-service-environments-readme.md).

Začínáme s prostředím služby aplikace, najdete v článku [Úvod do prostředí aplikace služby][IntroToAppServiceEnvironment]

Podrobnosti kolem řízení příchozích prostředí aplikace služby najdete v tématu [řízení příchozích prostředí aplikace služby][ControlInboundASE]

Další informace o aplikaci služby Azure platformu, najdete v článku [Aplikace služby Azure][AzureAppService].

[AZURE.INCLUDE [app-service-web-whats-changed](../../includes/app-service-web-whats-changed.md)]

[AZURE.INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]
 

<!-- LINKS -->
[virtualnetwork]: https://azure.microsoft.com/documentation/articles/virtual-networks-faq/
[ControlInboundTraffic]:  http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-control-inbound-traffic/
[SiteToSite]: https://azure.microsoft.com/documentation/articles/vpn-gateway-site-to-site-create/
[ExpressRoute]: http://azure.microsoft.com/services/expressroute/
[NetworkAccessControlLists]: https://azure.microsoft.com/documentation/articles/virtual-networks-acl/
[NetworkSecurityGroups]: https://azure.microsoft.com/documentation/articles/virtual-networks-nsg/
[IntroToAppServiceEnvironment]:  http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-intro/
[AzureAppService]: http://azure.microsoft.com/documentation/articles/app-service-value-prop-what-is/ 
[ControlInboundASE]:  http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-control-inbound-traffic/ 

<!-- IMAGES -->
[SqlServerEndpoint]: ./media/app-service-app-service-environment-securely-connecting-to-backend-resources/SqlServerEndpoint01.png
[NetworkAccessControlListExample]: ./media/app-service-app-service-environment-securely-connecting-to-backend-resources/NetworkAcl01.png
[DefaultNetworkSecurityRules]: ./media/app-service-app-service-environment-securely-connecting-to-backend-resources/DefaultNetworkSecurityRules01.png 
