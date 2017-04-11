<properties 
    pageTitle="Přehled architektura sítě z prostředí aplikace služby" 
    description="Přehled architektury ofApp topologie sítě prostředí služby." 
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

# <a name="network-architecture-overview-of-app-service-environments"></a>Přehled architektura sítě z prostředí aplikace služby

## <a name="introduction"></a>Úvod ##
Aplikace služby prostředí vždy vytvořené v rámci podsítě [virtuální sítě] [ virtualnetwork] -aplikace spuštěné v prostředí aplikace služby mohli komunikovat s soukromé koncové body nachází v rámci stejné topologie virtuální sítě.  Od zákazníků může uzamknout díly jejich virtuální síťové infrastruktury, je důležité pochopit typy sítě komunikace toků probíhajících s prostředím služby aplikace.

## <a name="general-network-flow"></a>Obecný síť toku ##
 
Pokud aplikaci služby prostředí řízení používá veřejné virtuální IP adresu (VIP) k aplikacím, všechny příchozí přenosy přijde na této veřejné VIP.  Jedná se o HTTP a HTTPS návštěvníci aplikace i ostatní přenosy protokolu FTP, funkce vzdáleného ladění a operace Azure správy.  Úplný seznam konkrétní porty (povinné i volitelné), které jsou k dispozici na veřejné VIP najdete v článku o [řízení příchozích] [ controllinginboundtraffic] k prostředí aplikace služby. 

Aplikace služby prostředí také podporují pracovního aplikace svázané pouze na virtuální sítě interní adresu někdy označovány jako adresu ILB (Vyrovnávání zatížení interní).  Na ILB povolené pomocného mechanismu řízení, HTTP a HTTPS přenosy aplikace, jakož i vzdálený ladění hovory, se ukládají na adrese ILB.  U nejběžnější konfigurací ILB pomocného mechanismu řízení přenosy protokolu FTP/FTPS taky dorazí na adrese ILB.  Ale operace Azure správy budou pořád flow porty 454/455 na veřejné VIP ILB povoleno pomocného mechanismu řízení.

Následující obrázek ukazuje základní informace o různých toků příchozí a odchozí sítě pro prostředí aplikace služby, kde jsou aplikace vázaný na veřejnou IP adresu virtuální:

![Obecný síť toků][GeneralNetworkFlows]

Prostředí aplikace služby komunikovat s celou řadou koncové body soukromé zákazníka.  Například aplikací v prostředí služby aplikace můžete připojit k databázi servery spuštěna IaaS virtuálních počítačích v topologii stejné virtuální sítě.

>[AZURE.IMPORTANT] Prohlížíte síťový diagram, jsou "Jiný výpočet zdroje" nasazené v různých podsítě z prostředí aplikace služeb. Nasazení zdrojů ve stejném podsítě s pomocného mechanismu řízení zablokuje připojení z pomocného mechanismu řízení podívejte se na tyto materiály (s výjimkou konkrétní uvnitř-pomocného mechanismu řízení směrování). Nasazení jiné podsítě místo toho (ve stejném VNET). Prostředí aplikace služeb pak budou moct připojit. Stačí nevyžaduje žádnou další konfiguraci.

Aplikace služby prostředí také komunikovat databáze Sql Azure úložiště a materiály potřebné pro správu a provozní prostředí aplikace služby.  Některé Sql a úložiště prostředky, které informuje uživatele o prostředí aplikace služby s nacházejí ve stejné oblasti jako prostředí aplikace služeb zatímco ostatní se nacházejí v vzdálené Azure oblastí.  V důsledku toho odchozí připojení k Internetu vždycky je nutný pro prostředí aplikace služby budou fungovat správně. 

Protože prostředí služby aplikací nasazena v podsítě, můžete skupiny zabezpečení sítě slouží k určení příchozích do podsítě.  Podrobnosti o tom, jak určit příchozích prostředí aplikace služby najdete následující [článek][controllinginboundtraffic].

Podrobnosti o tom, jak povolit odchozí připojení k Internetu z prostředí aplikace služby najdete v článku následující článek o práci s [Express směrování][ExpressRoute].  Stejný přístup popsaná v článku platí při práci s připojení k webu a pomocí vynucená tunneling.

## <a name="outbound-network-addresses"></a>Odchozí adresy ##
Když prostředí aplikace služby provede odchozí hovory, k IP adrese přidružen vždy odchozí hovory.  IP adresu, který se používá závisí na tom koncový bod volání umístěn v rámci topologii virtuální sítě nebo mimo ni topologii virtuální sítě.

Pokud koncový bod volání **mimo** virtuální topologie, odchozí adresy (označovaná taky jako odchozí adresy překladu síťových adres), který se používá je veřejné VIP prostředí aplikace služeb.  Tuto adresu najdete v portálu uživatelského rozhraní pro aplikaci služby prostředí ve vlastnosti zásuvné.
 
![Odchozích IP adresa][OutboundIPAddress]

Tuto adresu lze také stanovit pro ASEs, kteří mají veřejné VIP vytvořením aplikace v prostředí aplikace služeb a provádění *nslookup* aplikace na adresu. Výsledné IP adresu je veřejné VIP, jak odchozí adresy překladu síťových adres prostředí aplikace služeb.

Pokud koncový bod volání je **uvnitř** virtuální topologie, budou odchozí adresy volání aplikace interní IP adresu zdroje jednotlivé výpočetních spuštění aplikace.  Ale není trvalý mapování virtuální sítě interní IP adresy tak, aby aplikace.  Aplikace se můžete pohybovat různých různých pro využití prostředků a fondu k dispozici pro využití zdrojů v prostředí aplikace služby můžete změnit kvůli měřítka operace.

Však od prostředí služby aplikace se vždy nachází v rámci podsítě, můžete je zaručena, vnitřní IP adresu zdroje výpočetním spuštění aplikace se vždy leží rozsahu CIDR podsítě.  Jako výsledek Pokud jemně odstupňovaná ACL nebo skupiny zabezpečení sítě pro zabezpečený přístup k jiné koncové body v rámci virtuální sítě, podsítě oblast obsahující prostředí aplikace služby je potřeba mít udělený přístup.

Na následujícím obrázku vidíte koncepce podrobněji:

![Odchozí adresy][OutboundNetworkAddresses]

V diagramu:

- Protože veřejné VIP prostředí aplikace služeb je 192.23.1.2, který je odchozích IP adresu vyvolají volání koncové body "Internet".
- CIDR oblast obsahující podsítě prostředí služby aplikace je 10.0.1.0/26.  Další koncové body v rámci stejné virtuální síťovou infrastrukturu uvidí hovory z aplikací jako pocházející z jinam, postupujte v rozmezí adresu.

## <a name="calls-between-app-service-environments"></a>Volání mezi prostředími aplikace služby ##
Složitější situace může dojít, pokud nasazení služeb prostředí s více aplikací ve stejné síti virtuální a odchozí hovory z jednoho prostředí služby aplikace do jiné aplikace služby prostředí.  Tyto typy přes aplikaci služby prostředí budou volání považovány také jako "Internet" volání.

Na následujícím obrázku vidíte příklad vrstvy architektury s aplikací v jedné aplikaci služby prostředí (například "Přední dveří" webových aplikací web apps) volání aplikace na druhý prostředí služby aplikace (například interní back-end rozhraní API aplikace není určená k byly přístupné z Internetu). 

![Volání mezi prostředími aplikace služby][CallsBetweenAppServiceEnvironments] 

Ve výše uvedeném příkladu má prostředí služby aplikace "Pomocného mechanismu řízení jeden" odchozích IP adresy 192.23.1.2.  Pokud aplikace spuštěné v této aplikaci služby prostředí něco v ní odchozího hovoru do aplikace spuštěné v druhé aplikaci služby prostředí ("pomocného mechanismu řízení dvě") umístěn ve stejné virtuální sítě odchozí hovory budou považovány za pozvání "Internet".  Výsledkem provozu v síti odeslané na druhý prostředí aplikace služeb řádku se zobrazí jako pocházející z 192.23.1.2 (tedy ne podsítě adresu oblast první prostředí aplikace služby).

I když je volání mezi různých prostředích služby aplikace je považováno za "Internet" hovory, když obě prostředí služby aplikace nacházejí v oblasti stejné Azure provozu v síti zůstane v místní síti Azure a nebude fyzicky pokračujících veřejné Internetu.  Jako výsledek můžete skupinu zabezpečení sítě podsítě druhý prostředí služby aplikace umožňuje pouze příchozí hovory z první prostředí služby aplikace (u odchozích IP adresu je 192.23.1.2), tedy zajištění zabezpečená komunikace mezi prostředími služby aplikace.

## <a name="additional-links-and-information"></a>Další odkazů a informací ##
Všechny články a jak-pro uživatele pro aplikaci služby prostředí jsou k dispozici v [souboru README pro aplikaci služby prostředí](../app-service/app-service-app-service-environments-readme.md).

Podrobnosti o příchozí porty používané aplikace služby prostředí a pomocí skupin zabezpečení síť pro řízení příchozích neexistuje [tady][controllinginboundtraffic].

Informace o použití uživatele definované směruje udělit přístup k aplikaci služby prostředí Internetu je k dispozici v tomto [článku][ExpressRoute]. 


<!-- LINKS -->
[virtualnetwork]: http://azure.microsoft.com/services/virtual-network/
[controllinginboundtraffic]:  http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-control-inbound-traffic/
[ExpressRoute]:  http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-network-configuration-expressroute/

<!-- IMAGES -->
[GeneralNetworkFlows]: ./media/app-service-app-service-environment-network-architecture-overview/NetworkOverview-1.png
[OutboundIPAddress]: ./media/app-service-app-service-environment-network-architecture-overview/OutboundIPAddress-1.png
[OutboundNetworkAddresses]: ./media/app-service-app-service-environment-network-architecture-overview/OutboundNetworkAddresses-1.png
[CallsBetweenAppServiceEnvironments]: ./media/app-service-app-service-environment-network-architecture-overview/CallsBetweenEnvironments-1.png

