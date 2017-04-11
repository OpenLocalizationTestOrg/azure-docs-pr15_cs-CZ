<properties 
    pageTitle="GEO Distributed měřítko s prostředím aplikace služby" 
    description="Zjistěte, jak ve vodorovném směru zobrazit aplikace pomocí geo rozdělení s vedoucím přenosy a aplikace služeb prostředí." 
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
    ms.date="09/07/2016" 
    ms.author="stefsch"/>   

# <a name="geo-distributed-scale-with-app-service-environments"></a>GEO Distributed měřítko s prostředím aplikace služby

## <a name="overview"></a>Základní informace ##
Scénáře aplikací, které vyžadují velmi vysoké měřítko můžete překročit kapacitu zdroje výpočetním k dispozici jedno nasazení aplikace.  Hlasování aplikací, sportovní a událostech televizní zábavu příkladů všechny scénáře, které vyžadují velmi vysoké měřítko. Vysoký měřítka lze požadavků roztažením vodorovně, aplikace, s několika nasazení aplikace provádějí v rámci jedné oblasti i přes oblasti zpracovávat požadavky krajní načíst.

Aplikace služby prostředí jsou ideální platformu pro vodorovné měřítko.  Jednou aplikace prostředí služby vybraná konfigurace podporující sazbu známé žádost, vývojářů nástroje můžete nasazovat další prostředí aplikace služby způsobem "souborů cookie ořezávání" k dosažení užitečnou požadované pole Špička.

Předpokládejme například, že otestování aplikace spuštěna konfigurace prostředí aplikace služby pro zpracování žádostí o 20 K sekundu (RPS).  Pokud kapacitu načíst požadovaná pole Špička je 100 kB RPS, pak pěti (5) aplikace služby prostředí lze vytvořit a nakonfigurované tak, aby zajistěte, aby že aplikace můžete zpracovat maximální plánované načíst.

Vzhledem k tomu, že zákazníci obvykle k aplikací pomocí vlastní (nebo marnivost) doménu, třeba vývojáři způsob, jak distribuovat aplikaci požadavky ve všech instancí aplikace služby prostředí.  Je skvělý způsob, jak provést řešení vlastní doménu s [profil správce přenosy Azure][AzureTrafficManagerProfile].  Profil správce přenosy je možné konfigurovat tak, aby ukazovaly na všechny jednotlivé služby prostředí aplikace.  Přenosy správce bude automaticky zpracovávat distribuce zákazníci ve všech prostředí služby aplikace založené na nastavení profilu přenosy Správce vyrovnávání zatížení.  Tento postup funguje bez ohledu na to, zda žádné z prostředí aplikace služby jsou nasazenou v jedné oblasti Azure nebo nasazení po celém světe napříč několika Azure oblastí.

Kromě toho od zákazníků zobrazíte aplikace prostřednictvím marnivost domény zákazníci neví počtu prostředí služby aplikace spuštěna aplikace.  Výsledkem vývojáři můžete rychle a snadno přidání a odebrání prostředí služby aplikace založené na zatížení pozorovanými provozu.

Dole konceptuální diagram znázorňuje aplikace ve vodorovném směru diagramů s měřítky se přes tři aplikace služby prostředí v rámci jedné oblasti.

![Koncepční architektura][ConceptualArchitecture] 

Zbývající část tohoto tématu provede jednotlivými kroky nastavit distribuované topologie pro aplikaci Ukázka použití více aplikací služby prostředí.

## <a name="planning-the-topology"></a>Plánování topologii ##
Před vytvoření stopu distribuované aplikace, je dobré mít několik informace použitelné předem.

- **Vlastní domény pro aplikaci:**  Co je vlastní název domény, který zákazníkům bude používat pro přístup k aplikaci?  Ukázka aplikace vlastní název domény je *www.scalableasedemo.com*
- **Přenosy správce domény:**  Název domény je potřeba vybrat při vytváření [profil správce přenosy Azure][AzureTrafficManagerProfile].  Tento název spojí s příponou *trafficmanager.net* registrovat položku pro doménu, která spravuje správcem přenosy.  Ukázka aplikace je v poli Název zvolili *scalable ukázku pomocného mechanismu řízení*.  Úplný název domény, která spravuje správcem přenosy jako výsledek je *scalable demo.trafficmanager.net pomocného mechanismu řízení*.
- **Strategie pro změnu velikosti stopu aplikace:**  Bude stopu aplikace být rozvržena více aplikací služby prostředí v jedné oblasti?  Více oblastí?  Kombinace a odpovídající oba přístupy?  Rozhodnutí by podle očekávání kde bude pocházejí provozu zákazníků, stejně jako chod můžete změnit velikost zbytek aplikaci pro podporu back-end infrastruktury.  Například s 100 % příslušnosti aplikace pomocí aplikace je možné použít datových měřítko pomocí kombinace více prostředí aplikace služby Azure oblast vynásobené prostředí služby aplikací nasazena napříč několika Azure oblastí.  S 15 + veřejné Azure oblasti k dispozici a vyberte si z zákazníci vytvořit skutečně stopu world wide hyper měřítko aplikace.  Ukázka aplikace pro tento článek tři aplikace služby prostředí vytvořených v jedné oblasti Azure (Jižní centrální NAS).
- **Konvence pro aplikaci služby prostředí:**  Každé aplikaci služby prostředí vyžaduje jedinečný název.  Za jednu nebo dvě prostředí aplikace služby je vhodné mít stejný systém názvů k identifikaci každé aplikaci služby prostředí.  Ukázka aplikace byl použit jednoduché pojmenování.  Názvy tři služby prostředí aplikace jsou *fe1ase*, *fe2ase*a *fe3ase*.
- **Konvence pro aplikace:**  Vzhledem k tomu, že má být nasazené víc instancí aplikace, název potřebné pro každý výskyt nasazeném aplikace.  Jednu málo známé, ale je užitečná součást aplikace služby prostředí je možnost stejný název aplikace v několika prostředích služby aplikace.  Od každého prostředí aplikace služeb má příponu jedinečné domény, můžete zvolit vývojáři opakované použití přesné stejný název aplikace v každém prostředí.  Příklad vývojář může mít aplikace s názvem takto: *myapp.foo1.p.azurewebsites.net* *myapp.foo2.p.azurewebsites.net*, *myapp.foo3.p.azurewebsites.net*, atd.  Ukázka aplikace přes jednotlivé instance aplikace taky má jedinečný název.  Názvy instancí aplikace použité jsou *webfrontend1*, *webfrontend2*a *webfrontend3*.


## <a name="setting-up-the-traffic-manager-profile"></a>Nastavení profilu přenosy správce ##
Po několika instancí aplikace je zavedení na více aplikací služby prostředí, může být instance jednotlivé aplikace registrováno pomocí Správce přenosy.  Ukázka aplikace správce dopravy profilu potřebné pro *scalable pomocného mechanismu řízení demo.trafficmanager.net* můžete směrovat zákazníci některou z následujících případech nasazeném aplikace:

- **webfrontend1.fe1ase.p.azurewebsites.net:**  Instance aplikace ukázkové nasazený na první prostředí aplikace služeb.
- **webfrontend2.fe2ase.p.azurewebsites.net:**  Instance aplikace ukázkové nasazený na druhý prostředí aplikace služeb.
- **webfrontend3.fe3ase.p.azurewebsites.net:**  Instance aplikace ukázkové nasazené na třetí prostředí aplikace služeb.

Nejjednodušší způsob, jak zaregistrovat více Azure aplikace koncové body služby, všechny spuštěné v **stejné** Azure oblast, je pomocí prostředí Powershell [Azure zdroje správce přenosy správce podporují][ARMTrafficManager].  

Cílem prvního kroku je vytvořit profil aplikace Azure přenosy správce.  Následující kód ukazuje vytvoření profilu aplikace vzorku:

    $profile = New-AzureTrafficManagerProfile –Name scalableasedemo -ResourceGroupName yourRGNameHere -TrafficRoutingMethod Weighted -RelativeDnsName scalable-ase-demo -Ttl 30 -MonitorProtocol HTTP -MonitorPort 80 -MonitorPath "/"

Všimněte si, jak byla *RelativeDnsName* parametr nastaven na *scalable ukázku pomocného mechanismu řízení*.  Toto je týkající se název domény *scalable demo.trafficmanager.net pomocného mechanismu řízení* vytvořené a související s profilem přenosy správce.

Parametr *TrafficRoutingMethod* definuje zásada používané přenosy správce a zjistěte, jak roztažením zákazníka zatížení ve všech dostupných koncové body Vyrovnávání zatížení.  V tomto příkladu *vážená* byla vybrána metody.  Výsledkem bude požadavků zákazníků rozšířit mezi koncové body registrovaných aplikace založené na relativní tloušťky přidružené každý koncový bod. 

S profilem vytvořili jednotlivé instance aplikace přibude profil jako nativní Azure koncový bod.  Následující kód načte odkaz na každý front-end web appu a potom přidá jednotlivé aplikace jako koncový bod přenosy správce jako parametr *TargetResourceId* .


    $webapp1 = Get-AzureRMWebApp -Name webfrontend1
    Add-AzureTrafficManagerEndpointConfig –EndpointName webfrontend1 –TrafficManagerProfile $profile –Type AzureEndpoints -TargetResourceId $webapp1.Id –EndpointStatus Enabled –Weight 10

    $webapp2 = Get-AzureRMWebApp -Name webfrontend2
    Add-AzureTrafficManagerEndpointConfig –EndpointName webfrontend2 –TrafficManagerProfile $profile –Type AzureEndpoints -TargetResourceId $webapp2.Id –EndpointStatus Enabled –Weight 10

    $webapp3 = Get-AzureRMWebApp -Name webfrontend3
    Add-AzureTrafficManagerEndpointConfig –EndpointName webfrontend3 –TrafficManagerProfile $profile –Type AzureEndpoints -TargetResourceId $webapp3.Id –EndpointStatus Enabled –Weight 10
    
    Set-AzureTrafficManagerProfile –TrafficManagerProfile $profile
    
Všimněte si, jak je jedno volání *Přidat AzureTrafficManagerEndpointConfig* pokaždé jednotlivé aplikace.  Parametr *TargetResourceId* do každého příkazu Powershellu odkazuje nějaká tři instance nasazené aplikace.  Profil správce dopravy rozšířit zatížení mezi všechny tři koncové body registrované v profilu.

Tři koncové body použít stejnou hodnotu (10) pro parametr *weight (váha)* .  Výsledkem šíření požadavků zákazníků přenosy správce ve všech instancích tři aplikace relativně rovnoměrně. 


## <a name="pointing-the-apps-custom-domain-at-the-traffic-manager-domain"></a>Ukazatel ve tvaru v aplikaci vlastní domény u domény přenosy správce ##
Posledním krokem potřeby je tak, aby ukazovaly vlastní doménu aplikace v doméně přenosy správce.  Ukázka aplikace to znamená ukazující *www.scalableasedemo.com* v *scalable demo.trafficmanager.net pomocného mechanismu řízení*.  Tento krok je potřeba udělat u doménového registrátora, který má na starosti vlastní doménu.  

Pomocí nástroje pro správu registrátora vaší domény, CNAME záznamy je potřeba vytvořit která odkazuje vlastní domény u domény přenosy správce.  Následující obrázek ukazuje příklad vypadá tato CNAME konfigurace:

![CNAME pro vlastní doménu][CNAMEforCustomDomain] 

I když není uvedené v tomto tématu, mějte na paměti, že pokaždé jednotlivé aplikace, musí mít vlastní doménu zaregistrovanou s ním i.  V opačném případě pokud žádost o umožňuje přístup k instanci aplikace a aplikace, které neobsahují vlastní doménu zaregistrovanou s aplikací, žádost se nezdaří.  

V tomto příkladu je vlastní doména *www.scalableasedemo.com*a jednotlivé instance aplikace má vlastní doménu s ním spojené.

![Vlastní doménu][CustomDomain] 

Recap registrace vlastní doménu s aplikacemi jiných aplikací služby Azure, naleznete v následujícím článku k [registraci vlastní domény, které][RegisterCustomDomain].

## <a name="trying-out-the-distributed-topology"></a>Vyzkoušet topologii Distributed ##
V konečném důsledku konfigurace přenosy správce a DNS je žádosti o *www.scalableasedemo.com* bude postupují následujícím pořadí:

1. Prohlížeč nebo zařízení, aby vyhledávání DNS pro *www.scalableasedemo.com*
2. Vyhledání DNS přesměrováni správci přenosy Azure způsobí položka CNAME u doménového registrátora.
3. Vyhledání DNS je volání pro *scalable pomocného mechanismu řízení demo.trafficmanager.net* proti jedné serverů DNS přenosy správce Azure.
4. Podle zásada ( *TrafficRoutingMethod* použitým dříve při vytváření profilu přenosy správce) Vyrovnávání zatížení, přenosy správce vyberte jednu z nakonfigurované koncové body a plně kvalifikovaný název domény, které koncový bod vrátit prohlížeč nebo zařízení.
5.  Protože plně kvalifikovaný název domény koncový bod je adresa Url instance aplikace spuštěné v prostředí aplikace služby, prohlížeč nebo zařízení zobrazí dotaz, serveru Microsoft Azure DNS pro překládal plně kvalifikovaný název domény k IP adrese. 
6. Prohlížeč nebo zařízení odešle žádost HTTP/S k IP adrese.  
7. Žádost budou přicházet na jednom instancí aplikace na některém z prostředí aplikace služby.

Konzoly obrázku dole můžete vidět vyhledávání DNS pro vlastní doménu ukázkové aplikace úspěšně řešení k instanci aplikace na některém z tři ukázkové aplikace služby prostředí (v tomto případě druhý tři prostředí aplikace služby):

![Vyhledání DNS][DNSLookup] 

## <a name="additional-links-and-information"></a>Další odkazů a informací ##
Všechny články a jak-pro uživatele pro aplikaci služby prostředí jsou k dispozici v [souboru README pro aplikaci služby prostředí](../app-service/app-service-app-service-environments-readme.md).

Si přečtěte následující dokumentaci v prostředí Powershell [Azure zdroje správce přenosy správce podporují][ARMTrafficManager].  

[AZURE.INCLUDE [app-service-web-whats-changed](../../includes/app-service-web-whats-changed.md)]

[AZURE.INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]

<!-- LINKS -->
[AzureTrafficManagerProfile]:  https://azure.microsoft.com/documentation/articles/traffic-manager-manage-profiles/
[ARMTrafficManager]:  https://azure.microsoft.com/documentation/articles/traffic-manager-powershell-arm/
[RegisterCustomDomain]:  https://azure.microsoft.com/en-us/documentation/articles/web-sites-custom-domain-name/


<!-- IMAGES -->
[ConceptualArchitecture]: ./media/app-service-app-service-environment-geo-distributed-scale/ConceptualArchitecture-1.png
[CNAMEforCustomDomain]:  ./media/app-service-app-service-environment-geo-distributed-scale/CNAMECustomDomain-1.png
[DNSLookup]:  ./media/app-service-app-service-environment-geo-distributed-scale/DNSLookup-1.png
[CustomDomain]:  ./media/app-service-app-service-environment-geo-distributed-scale/CustomDomain-1.png 
