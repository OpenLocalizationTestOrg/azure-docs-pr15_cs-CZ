<properties 
    pageTitle="Konfigurace nastavení sítě pro práci s směrování Express" 
    description="Konfigurace nastavení sítě pro spuštění aplikace služby prostředí virtuální sítě připojení k ExpressRoute okruh." 
    services="app-service" 
    documentationCenter="" 
    authors="stefsch" 
    manager="nirma" 
    editor=""/>

<tags 
    ms.service="app-service" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/14/2016" 
    ms.author="stefsch"/>   

# <a name="network-configuration-details-for-app-service-environments-with-expressroute"></a>Konfigurace nastavení sítě pro aplikaci služby prostředí ExpressRoute 

## <a name="overview"></a>Základní informace ##
Zákazníci se můžete připojit [Azure ExpressRoute] [ ExpressRoute] okruh jejich virtuální síťové infrastruktury, tak na Azure rozšířit jejich místní síti.  Prostředí aplikace služby lze vytvořit podsítí tento [virtuální sítě] [ virtualnetwork] infrastruktury.  Aplikace spuštěné v prostředí aplikace služeb můžete pak vytvořit zabezpečené připojení k prostředkům back-end přístupných osobám s postižením pouze pomocí připojení ExpressRoute.  

Prostředí aplikace služby dá vytvořit v **obou** virtuální sítě Správce prostředků Azure, **nebo** na klasické nasazení modelu virtuální sítě.  Poslední změny provedené v červnu 2016 můžete ASEs nyní také nasadit do virtuálního sítích, které používají veřejná adresa oblasti nebo RFC1918 adresu mezer (tedy soukromé adresy). 

[AZURE.INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)] 

## <a name="required-network-connectivity"></a>Připojení k síti požadovaných ##
Existuje požadavky připojení sítě pro aplikaci služby prostředí, které nemusí být splněná původně v síti virtuální připojený k ExpressRoute.  Aplikace služby prostředí celý následující postup vyžaduje fungovat správně:


-  Odchozí síťové připojení k úložišti Azure koncové body Celosvětová dostupnost na obou porty 80 a 443.  Platí to i pro koncové body umístěny ve stejné oblasti jako prostředí aplikace služeb, jakož i koncové body úložiště součástí **jiných** Azure oblastí.  Azure koncové body úložiště vyřešit podle těchto DNS domény: *table.core.windows.net* *blob.core.windows.net*, *queue.core.windows.net* a *file.core.windows.net*.  
-  Připojení k síti odchozí pro službu Azure soubory na portu 445.
-  Připojení k síti odchozí pro koncové body Sql databáze umístěny ve stejné oblasti jako prostředí aplikace služeb.  Koncové body SQL databáze vyřešit podle tuto doménu: *database.windows.net*.  Při této akci musí otevření přístup k porty 1433 11000 11999 a 14000 14999.  Další informace naleznete v [článku o využití port V12 databáze Sql](../sql-database/sql-database-develop-direct-route-ports-adonet-v12.md).
-  Připojení k síti odchozí pro koncové body rovině Azure management (koncové body ASM a ARM).  Jedná se o odchozí připojení ke službě *management.core.windows.net* i *management.azure.com*. 
-  Připojení k síti odchozí *ocsp.msocsp.com*, *mscrl.microsoft.com* a *crl.microsoft.com*.  To je potřeba k podpoře funkcí SSL.
-  Konfigurace DNS pro virtuální sítě musí být může řešení všechny koncové body a domény uvedené v dřívějších bodů.  Pokud tyto koncové body nelze přeložit, se nepovede pokusech po vytvoření aplikace služby prostředí a existující prostředí aplikace služby bude označeno jako chybné.
-  Přístup pro odchozí připojení na portu 53 je potřebný pro komunikaci s serverů DNS.
-  Pokud vlastní DNS server existuje na druhou stranu brány VPN, musí být DNS server k nim přistupovat z podsítě obsahující prostředí aplikace služeb. 
-  Odchozí síťové cesty nejde přecházet mezi interní firemní proxy servery, ani může být vyšší vytvořena do místního nasazení.  Z prostředí aplikace služeb změní se tím adresu efektivní překladu síťových adres odchozí provozu v síti.  Změna adresy překladu síťových adres z prostředí aplikace služby odchozí v síti způsobí selhání připojení ku mnoha koncové body výše uvedené.  Výsledkem neúspěšné pokusy vytvoření aplikace služby prostředí, jakož i dříve správný prostředí služby aplikace se označí jako chybný.  
-  Příchozí přístup k síti na požadované porty pro aplikaci služby prostředí musí mít způsobem popsaným v tomto [článku][requiredports].

Požadavky na DNS můžete být splněná zajišťuje nakonfigurované a udržovat pro virtuální sítě platné infrastruktury služby DNS.  Pokud z nějakého důvodu dojde ke změně konfigurace DNS po vytvoření prostředí aplikace služby, můžete vynutit vývojáři prostředí aplikace služby vyberte nové konfigurace DNS.  Spouštějící postupné restartování prostředí pomocí ikony "Restartujte" umístěný v horní části zásuvné správy prostředí aplikace služby [Azure portál] [ NewPortal] způsobí prostředí vystopovat nové konfigurace DNS.

Příchozí síťové access může být požadavků nakonfigurováním [skupiny zabezpečení sítě] [ NetworkSecurityGroups] v prostředí aplikace služeb podsítě přístupu požadované způsobem popsaným v tomto [článku][requiredports].

## <a name="enabling-outbound-network-connectivity-for-an-app-service-environment"></a>Povolení odchozí připojení k síti určité prostředí aplikace služby##
Ve výchozím nastavení oznamuje nově vytvořený okruh ExpressRoute výchozí trasu, která povolí odchozí připojení k Internetu.  V této konfiguraci prostředí aplikace služby bude moct připojit k jiné Azure koncové body.

Běžné konfigurace zákazníka je ale definovat vlastní výchozí trasy (0.0.0.0/0) vynutí, odchozí internetový provoz místo toho na plovoucí dlaždice místní.  Tento tok vždy konce aplikace služby prostředí, protože odchozí přenosy buď blokovaných místně, nebo překladu síťových adres chcete do exportu sady adres, které už práce s různými Azure koncové body.

Řešení je definovat jeden nebo více zástupců směruje definované uživatelem (UDRs) na podsítě obsahující prostředí aplikace služeb.  UDR definuje podsítě specifické postupy, které bude použito místo výchozího postupu.

Pokud je to možné doporučujeme použít následující konfigurace:

- Konfigurace ExpressRoute oznamuje 0.0.0.0/0 a ve výchozím nastavení vynutit propojení všechny odchozí přenosy místní.
- UDR použité u podsítě obsahující prostředí aplikace služeb definuje 0.0.0.0/0 s typem další směrování internetových (například je dolů dále v tomto článku).

Kombinované efekt tento postup je, že úroveň podsítě UDR přednost ExpressRoute vynucené tunneling, aby byla zajištěna odchozí přístup k Internetu z prostředí aplikace služeb.

> [AZURE.IMPORTANT] Směruje podle UDR, **musí** být dostatečně specifické pro přednost před všechny tras oznamovaných konfigurací ExpressRoute.  V příkladu níže používá rozsah adres obecných 0.0.0.0/0 a jako takové můžete potenciálně omylem přepsat trasách pomocí konkrétnější rozsahy adres.
>
>Aplikace služby prostředí nejsou podporované s konfigurací ExpressRoute této **křížově inzerce směruje z veřejné peering cestu k soukromé peering cestu**.  ExpressRoute konfigurace veřejné prozkoumávání nakonfigurován, dostanou trasách od společnosti Microsoft pro velké množství rozsahy Microsoft Azure IP adres.  Pokud tyto rozsahy adres křížově webového soukromé peering cestu, v konečném důsledku společnosti, po které bude všechny odchozí síťové pakety z prostředí aplikace služeb podsítě platnost vytvořena pro zákazníka místní síťovou infrastrukturu.  Tento vývojový sítě aktuálně nepodporuje s prostředím služby aplikace.  Řešením tohoto problému je zastavit křížově reklamě směruje z veřejné peering cesty soukromé peering cestu.

Základní informace o směrování definované uživatelem je k dispozici v tomto [Přehled][UDROverview].  

Informace o vytváření a konfigurace uživatelem definované směruje je k dispozici v této [Příručky][UDRHowTo].

## <a name="example-udr-configuration-for-an-app-service-environment"></a>Příklad UDR konfigurace prostředí aplikace služby ##

**Předpoklady**

1. Instalace prostředí Powershell Azure ze [stránky souborů ke stažení Azure] [ AzureDownloads] (datem dne 2015 nebo novější).  V části "Příkazového řádku nástroje" je "Instalace" odkaz v části "Prostředí Windows Powershell", bude nainstalovat nejnovější rutiny prostředí Powershell.

2. Doporučujeme, aby jedinečné podsítě vytvořit využívat prostředí aplikace služby.  Zajistíte tím, že UDRs použité u podsítě pouze otevřené odchozí přenosy pro aplikaci služby prostředí.
3. **Důležité**: nasazení prostředí aplikace služeb až **Po** sledované následující kroky konfigurace.  Zajistíte tak, že připojení k síti odchozí neexistuje před pokusem o nasazení prostředí aplikace služby.

**Krok 1: Vytvoření trasu s tímto názvem tabulky**

Následující úryvek vytvoří tabulku postupu v oblasti Azure západní nám s názvem "DirectInternetRouteTable":

    New-AzureRouteTable -Name 'DirectInternetRouteTable' -Location uswest

**Krok 2: Vytvoření jedné nebo více trasách v tabulce směrování**

Bude potřebujete přidat jeden nebo více postupů k tabulce směrování Pokud chcete povolit odchozí přístup k Internetu.  

Doporučený postup pro konfiguraci odchozího přístup k Internetu je definici cesty pro 0.0.0.0/0 jak je ukázáno v následujícím příkladu.
  
    Get-AzureRouteTable -Name 'DirectInternetRouteTable' | Set-AzureRoute -RouteName 'Direct Internet Range 0' -AddressPrefix 0.0.0.0/0 -NextHopType Internet

Mějte na paměti, které 0.0.0.0/0 je oblast obecných adresa a jako takové se přepíšou konkrétnější rozsahy adres inzerovaných ExpressRoute.  Znovu iterace dřívějšího doporučení, bude použito UDR 0.0.0.0/0 postupu ve spojení s konfigurací ExressRoute, pouze oznamuje 0.0.0.0/0 stejně. 

Jako alternativu můžete si stáhnout komplexní a aktualizovaný seznam rozsahů CIDR nepoužívá Azure.  Soubor Xml, ve kterém všech požadovaných oblastí Azure IP adresu neexistuje z [Webu služby Stažení softwaru][DownloadCenterAddressRanges].  

Mějte, že tyto oblasti v průběhu času mění, tedy vyžadující pravidelných ruční aktualizace uživatelem definované postupy pro synchronizaci.  Taky protože je výchozí horní mez 100 cest v jedné UDR, budete muset "souhrn" rozsahy Azure IP adres tak, aby odpovídaly limit 100 směrování uchovávání v paměti, že UDR definované směruje potřeba konkrétnější než trasy inzerovaných vaší ExpressRoute.  


**Krok 3: Přiřazení tabulce směrování adres podsítí obsahující prostředí aplikace služby**

Poslední krok konfigurace má přidružení tabulce směrování do podsítě místo, kam má být nasazené prostředí aplikace služeb.  Tento příkaz přiřadí "DirectInternetRouteTable" k "ASESubnet", který bude obsahovat postupně prostředí aplikace služby.

    Set-AzureSubnetRouteTable -VirtualNetworkName 'YourVirtualNetworkNameHere' -SubnetName 'ASESubnet' -RouteTableName 'DirectInternetRouteTable'


**Krok 4: Poslední kroky**

Po tabulce směrování svázán podsítě, doporučujeme nejdřív otestovat a potvrďte zamýšleného efekt.  Například nasazení virtuálního počítače do podsítě a ověřte, že:


- Odchozí přenosy pro koncové body Azure i bez Azure uvedených dříve v tomto článku **není** toku dolů okruh ExpressRoute.  Je velmi důležité pro ověření toto chování, protože pokud je odchozí přenosy z podsítě pořád vynucené vytvořena místním prostředí aplikace služeb vytváření vždy se nezdaří. 
- Vyhledávání DNS pro koncové body uvedených dříve všechny řešení správně. 

Jakmile se pole Potvrzeno výše uvedené kroky, musíte odstranit virtuální počítač, protože podsítě třeba "prázdné" v době, kdy se vytvoří prostředí aplikace služeb.
 
Potom pokračujte ve vytváření prostředí aplikace služby!

## <a name="getting-started"></a>Začínáme
Všechny články a jak-pro uživatele pro aplikaci služby prostředí jsou k dispozici v [souboru README pro aplikaci služby prostředí](../app-service/app-service-app-service-environments-readme.md).

Začínáme s prostředím služby aplikace, najdete v článku [Úvod do prostředí aplikace služby][IntroToAppServiceEnvironment]

Další informace o aplikaci služby Azure platformu, najdete v článku [Aplikace služby Azure][AzureAppService].

<!-- LINKS -->
[virtualnetwork]: http://azure.microsoft.com/services/virtual-network/
[ExpressRoute]: http://azure.microsoft.com/services/expressroute/
[requiredports]: http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-control-inbound-traffic/
[NetworkSecurityGroups]: http://azure.microsoft.com/documentation/articles/virtual-networks-nsg/
[UDROverview]: http://azure.microsoft.com/documentation/articles/virtual-networks-udr-overview/
[UDRHowTo]: http://azure.microsoft.com/documentation/articles/virtual-networks-udr-how-to/
[HowToCreateAnAppServiceEnvironment]: http://azure.microsoft.com/documentation/articles/app-service-web-how-to-create-an-app-service-environment/
[AzureDownloads]: http://azure.microsoft.com/en-us/downloads/ 
[DownloadCenterAddressRanges]: http://www.microsoft.com/download/details.aspx?id=41653  
[NetworkSecurityGroups]: https://azure.microsoft.com/documentation/articles/virtual-networks-nsg/
[AzureAppService]: http://azure.microsoft.com/documentation/articles/app-service-value-prop-what-is/
[IntroToAppServiceEnvironment]:  http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-intro/
[NewPortal]:  https://portal.azure.com
 

<!-- IMAGES -->
