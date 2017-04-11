<properties 
    pageTitle="Architektura zabezpečení ve vrstvách s prostředím aplikace služby" 
    description="Implementace architektura ve vrstvách zabezpečení s prostředím služby aplikace." 
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
    ms.date="08/30/2016" 
    ms.author="stefsch"/>   

# <a name="implementing-a-layered-security-architecture-with-app-service-environments"></a>Provádění architektura ve vrstvách zabezpečení s prostředím aplikace služby

## <a name="overview"></a>Základní informace ##
 
Protože aplikace služby prostředí poskytovat prostředí izolace runtime nasadit do virtuální síť, můžete vytvořit vývojáři architektura ve vrstvách zabezpečení poskytující různé úrovně přístupu k síti pro každé osy fyzickou aplikace.

Společné přání je skrýt rozhraní API back zakončení z přístup k Internetu a povolit pouze rozhraní API pro volání aplikace nadřazeného webu.  [Sítě skupin zabezpečení (NSGs)] [ NetworkSecurityGroups] lze podsítí obsahující aplikace služby prostředí omezit veřejné přístup k rozhraní API aplikace.

Následující obrázek ukazuje příklad architektury pomocí aplikace WebAPI na základě nasazené na prostředí aplikace služby.  Tři instance aplikace samostatnou webovou nasazený na tři samostatné aplikace služby prostředí volání back-end stejné WebAPI aplikace.

![Koncepční architektura][ConceptualArchitecture] 

Zelené znaménko plus označuje, že síť skupiny zabezpečení na podsítě obsahující "apiase" umožňuje příchozí hovory v aplikacích nadřazeného webu jako volání a od sebe.  Ale stejné skupiny zabezpečení sítě explicitně odepřít přístup k obecné příchozí data z Internetu. 

Zbývající část tohoto tématu provede kroky potřebné k nastavení skupiny zabezpečení sítě podsítě obsahující "apiase".

## <a name="determining-the-network-behavior"></a>Určení chování v síti ##
Pokud chcete zjistit, jaká pravidla zabezpečení sítě jsou potřebné, budete muset zjistit, jaký klienty bude povoleno dosáhla prostředí aplikace služeb obsahující rozhraní API aplikace a klientů, kteří budou blokovány.

Od [sítě skupin zabezpečení (NSGs)] [ NetworkSecurityGroups] zaevidují do podsítí a nasazení aplikace služby prostředí do podsítí, součástí NSG pravidla platí pro **všechny** aplikace spuštěné v prostředí aplikace služby.  Použití vzorku architektury v tomto článku po skupinu zabezpečení sítě se použije pro podsítě obsahující "apiase", všech aplikací na "apiase" prostředí aplikace služeb chráněny stejnou sadu pravidel zabezpečení. 

- **Určit odchozích IP adresu nadřazeného volající:**  Co je IP adresa nebo adresy nadřazeného volající?  Tyto adresy muset explicitně mít přístup v NSG.  Protože volání mezi aplikací služby prostředí jsou považovány za "Internet" hovory, znamená to, že odchozích IP adresy přiřazené k všech tři nadřazeného prostředí služby aplikace musí být povolený přístup v NSG podsítě "apiase".   Podrobné informace o určení odchozích IP adresu aplikace spuštěné v prostředí aplikace služby najdete v článku [Architektura sítě] [ NetworkArchitecture] článek Přehled.
- **Rozhraní API aplikace back-end muset volat samotné?**  Někdy přehlíženým a jemné bod je scénář kde back-end potřebuje volat přímo.  Pokud do back-end rozhraní API aplikací v prostředí aplikace služby potřebuje volat sám sebe, to taky zpracován jako "Internet" hovor.  V ukázkové architektura vyžaduje povolení přístupu z odchozích IP adresu "apiase" prostředí aplikace služeb stejně.

## <a name="setting-up-the-network-security-group"></a>Nastavení sítě skupiny zabezpečení ##
Po nastavení odchozích IP adres je známo, dalším krokem je vytvoření skupiny zabezpečení síť.  Pro oba zdroje správce založené virtuálních sítí, jakož i klasické virtuální sítě se dají vytvářet skupiny zabezpečení síť.  Následující příklady ukazují, vytváření a konfigurace NSG v síti klasický virtuální pomocí Powershellu.

Ukázka architektury prostředí se nacházíte, Jižní centrální cz, aby prázdné NSG se vytvoří v dané oblasti:

    New-AzureNetworkSecurityGroup -Name "RestrictBackendApi" -Location "South Central US" -Label "Only allow web frontend and loopback traffic"

Nejdřív explicitní povolit přidáno pravidlo pro infrastruktura Azure správy, jak je uvedeno v následujícím článku na [příchozích] [ InboundTraffic] pro aplikaci služby prostředí.

    #Open ports for access by Azure management infrastructure
    Get-AzureNetworkSecurityGroup -Name "RestrictBackendApi" | Set-AzureNetworkSecurityRule -Name "ALLOW AzureMngmt" -Type Inbound -Priority 100 -Action Allow -SourceAddressPrefix 'INTERNET' -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '454-455' -Protocol TCP
    
Pak dvě pravidla se přidají do umožňují HTTP a HTTPS voláním z první odesílání dat aplikace služeb prostředí ("fe1ase").

    #Grant access to requests from the first upstream web front-end
    Get-AzureNetworkSecurityGroup -Name "RestrictBackendApi" | Set-AzureNetworkSecurityRule -Name "ALLOW HTTP fe1ase" -Type Inbound -Priority 200 -Action Allow -SourceAddressPrefix '65.52.xx.xyz'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '80' -Protocol TCP
    Get-AzureNetworkSecurityGroup -Name "RestrictBackendApi" | Set-AzureNetworkSecurityRule -Name "ALLOW HTTPS fe1ase" -Type Inbound -Priority 300 -Action Allow -SourceAddressPrefix '65.52.xx.xyz'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '443' -Protocol TCP

Vypláchněte a opakujte pro druhé a třetí odesílání dat aplikace služeb prostředí ("fe2ase" a "fe3ase").

    #Grant access to requests from the second upstream web front-end
    Get-AzureNetworkSecurityGroup -Name "RestrictBackendApi" | Set-AzureNetworkSecurityRule -Name "ALLOW HTTP fe2ase" -Type Inbound -Priority 400 -Action Allow -SourceAddressPrefix '191.238.xyz.abc'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '80' -Protocol TCP
    Get-AzureNetworkSecurityGroup -Name "RestrictBackendApi" | Set-AzureNetworkSecurityRule -Name "ALLOW HTTPS fe2ase" -Type Inbound -Priority 500 -Action Allow -SourceAddressPrefix '191.238.xyz.abc'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '443' -Protocol TCP
    
    #Grant access to requests from the third upstream web front-end
    Get-AzureNetworkSecurityGroup -Name "RestrictBackendApi" | Set-AzureNetworkSecurityRule -Name "ALLOW HTTP fe3ase" -Type Inbound -Priority 600 -Action Allow -SourceAddressPrefix '23.98.abc.xyz'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '80' -Protocol TCP
    Get-AzureNetworkSecurityGroup -Name "RestrictBackendApi" | Set-AzureNetworkSecurityRule -Name "ALLOW HTTPS fe3ase" -Type Inbound -Priority 700 -Action Allow -SourceAddressPrefix '23.98.abc.xyz'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '443' -Protocol TCP

Nakonec udělte přístup k odchozích IP adresu rozhraní API back-end aplikace služby prostředí tak, že můžete volat zpět do sebe.

    #Allow apps on the apiase environment to call back into itself
    Get-AzureNetworkSecurityGroup -Name "RestrictBackendApi" | Set-AzureNetworkSecurityRule -Name "ALLOW HTTP apiase" -Type Inbound -Priority 800 -Action Allow -SourceAddressPrefix '70.37.xyz.abc'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '80' -Protocol TCP
    Get-AzureNetworkSecurityGroup -Name "RestrictBackendApi" | Set-AzureNetworkSecurityRule -Name "ALLOW HTTPS apiase" -Type Inbound -Priority 900 -Action Allow -SourceAddressPrefix '70.37.xyz.abc'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '443' -Protocol TCP

Žádná další pravidla zabezpečení sítě třeba konfigurovat, protože každé NSG obsahuje sady výchozí pravidla, které blokují příchozí přístup z Internetu ve výchozím nastavení.

Úplný seznam pravidel ve skupině zabezpečení sítě jsou uvedeny níže.  Všimněte si, jak poslední pravidlo, které se zvýrazněným tlačítkem blokuje příchozí přístup ze všech volající než těch, které máte explicitně udělený přístup.

![Konfigurace NSG][NSGConfiguration] 

Posledním krokem je použít NSG podsítě obsahující "apiase" prostředí aplikace služeb.  

     #Apply the NSG to the backend API subnet
    Get-AzureNetworkSecurityGroup -Name "RestrictBackendApi" | Set-AzureNetworkSecurityGroupToSubnet -VirtualNetworkName 'yourvnetnamehere' -SubnetName 'API-ASE-Subnet'

S NSG použité u podsítě pouze tři odesílání dat aplikace služeb prostředí a prostředí aplikace služeb obsahující rozhraní API back-end, budou moci zavolat a připojit se prostředí "apiase".


## <a name="additional-links-and-information"></a>Další odkazů a informací ##
Všechny články a jak-pro uživatele pro aplikaci služby prostředí jsou k dispozici v [souboru README pro aplikaci služby prostředí](../app-service/app-service-app-service-environments-readme.md).

Informace o [skupinách zabezpečení sítě](../virtual-network/virtual-networks-nsg.md). 

Principy [odchozích IP adres] [ NetworkArchitecture] a aplikace služeb prostředí.

[Sítě porty] [ InboundTraffic] používané aplikace služby prostředí.

[AZURE.INCLUDE [app-service-web-whats-changed](../../includes/app-service-web-whats-changed.md)]

[AZURE.INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]

<!-- LINKS -->
[NetworkSecurityGroups]: https://azure.microsoft.com/documentation/articles/virtual-networks-nsg/
[NetworkArchitecture]:  https://azure.microsoft.com/documentation/articles/app-service-app-service-environment-network-architecture-overview/
[InboundTraffic]:  https://azure.microsoft.com/en-us/documentation/articles/app-service-app-service-environment-control-inbound-traffic/

<!-- IMAGES -->
[ConceptualArchitecture]: ./media/app-service-app-service-environment-layered-security/ConceptualArchitecture-1.png
[NSGConfiguration]:  ./media/app-service-app-service-environment-layered-security/NSGConfiguration-1.png
