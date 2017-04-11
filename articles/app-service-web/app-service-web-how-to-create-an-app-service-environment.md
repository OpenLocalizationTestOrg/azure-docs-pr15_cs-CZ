<properties 
    pageTitle="Postup vytvoření prostředí aplikace služby" 
    description="Vytvoření toku popis služby prostředí aplikace" 
    services="app-service" 
    documentationCenter="" 
    authors="ccompy" 
    manager="stefsch" 
    editor=""/>

<tags 
    ms.service="app-service" 
    ms.workload="web" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/22/2016" 
    ms.author="ccompy"/>

# <a name="how-to-create-an-app-service-environment"></a>Postup vytvoření prostředí aplikace služby #

### <a name="overview"></a>Základní informace ###

Aplikace služby prostředí (pomocného mechanismu řízení) jsou možností služby Premium služby Azure aplikace, která poskytuje rozšířeného nastavení funkce, která není k dispozici ve více klienta razítek.  Funkce pomocného mechanismu řízení v podstatě nasadí aplikace služby Azure do virtuálního sítě zákazníků.  Pokud chcete získat větší Principy funkce nabízené služby prostředí aplikace přečíst, [Co je prostředí aplikace služby] [ WhatisASE] si přečtěte následující dokumentaci.

### <a name="before-you-create-your-ase"></a>Než vytvoříte vaší pomocného mechanismu řízení ###

Je důležité je potřeba vědět o věcech, které nelze změnit.  Tyto aspekty, které nelze změnit o vaší pomocného mechanismu řízení po vytvoření otevřela jsou:

- Umístění
- Předplatné
- Pole Skupina zdroje
- VNet používá
- Podsítě používá 
- Velikost podsítě

Při výběru VNet a určení podsítě, ujistěte se, je dostatečně velký k tomu pro všechny budoucí růst.  

### <a name="creating-an-app-service-environment"></a>Vytvoření prostředí aplikace služby ###

Existují dva způsoby, jak získat přístup k vytvoření pomocného mechanismu řízení uživatelského rozhraní.  Může to být nalezena vyhledáním v Azure Marketplace ***Prostředí aplikace služeb*** nebo prostřednictvím New -> Web + Mobile -> prostředí aplikace služeb.  Vytvoření pomocného mechanismu řízení:

1. Zadejte název vašeho pomocného mechanismu řízení.  Název, který je určený pro pomocného mechanismu řízení se použijí k aplikacím vytvořené v pomocného mechanismu řízení.  Je-li název pomocného mechanismu řízení appsvcenvdemo název subdoménu by měl být. *appsvcenvdemo.p.azurewebsites.net*.  Pokud jste aplikaci s názvem *mytestapp* tím vytvořili pak bude s možností zadání na *mytestapp.appsvcenvdemo.p.azurewebsites.net*.  Název vaší pomocného mechanismu řízení nelze použít prázdného místa mezi stránkami.  Pokud používáte na velká písmena znaky v názvu, název domény budou celkové malá písmena verzi tohoto názvu.  Pokud používáte ILB pak názvu pomocného mechanismu řízení ve vaší subdoménu nepoužívá, ale je místo toho explicitně uveden při vytváření pomocného mechanismu řízení

    ![][1]

2. Vyberte předplatné.  Předplatné pro vaše pomocného mechanismu řízení je také té, která se bude vytvořena všechny aplikace v této pomocného mechanismu řízení.  Vaše pomocného mechanismu řízení nemůžete zaškrtnout VNet, který je v jiné předplatné

3. Vyberte nebo zadejte nové skupiny prostředků.  Skupina zdroje pro vaše pomocného mechanismu řízení se musí shodovat, který se používá pro vaše VNet.  Pokud vyberete stávajících VNet bude výběr skupiny prostředků pro vašeho pomocného mechanismu řízení aktualizován aby odrážela vaší VNet.

    ![][2]

4. Vyberte požadované hodnoty virtuální sítě a umístění.  Můžete vytvořit nový VNet nebo vyberte stávající VNet.  Při výběru nové VNet je můžete zadat název a umístění. Nové VNet bude mít 192.268.250.0/23 oblast adresa a podsítě s názvem **výchozí** hodnotu, kterou je definována následujícím 192.168.250.0/24.  Můžete taky jednoduše vyberte stávajících Classic nebo VNet správce zdrojů.  Výběr typu VIP Určuje, zda vaše pomocného mechanismu řízení přímo přístupný z Internetu (externího) nebo pokud používá interní zatížení vyrovnávání (ILB).  Další informace o nich znáte přečteno [Interní Vyrovnávání zatížení s prostředím služby aplikace][ILBASE].  Pokud vyberete typ VIP externí můžete vybrat počet externích adres IP systém se vytvoří pomocí pro účely IPSSL.  Pokud vyberete interní budete muset zadat subdoména, kterou budou používat svůj pomocného mechanismu řízení.  ASEs může být nasazené do virtuálního sítích, které používají *některý z* oblasti veřejné adresy *nebo* RFC1918 adresu mezer (tedy soukromé adresy).  Abyste mohli používat virtuální síť s rozsahem veřejná adresa, bude potřeba vytvořit VNet předem.  Po výběru již existujícího VNet bude potřeba vytvořit nové podsítě při vytváření pomocného mechanismu řízení.  **Předem vytvořená podsítě nelze použít v portálu.  Pokud budete vytvářet svůj pomocného mechanismu řízení pomocí šablony správce zdrojů, můžete vytvořit pomocného mechanismu řízení s stávajících podsítě.**  Pokud chcete vytvořit pomocného mechanismu řízení z použití šablon informace [vytváření prostředí aplikace služby ze šablony] [ ILBAseTemplate] a tady se dozvíte, [vytváření prostředí ILB aplikace služby ze šablony][ASEfromTemplate].

### <a name="details"></a>Podrobnosti ###

Pomocného mechanismu řízení se vytvoří pomocí 2 front-end a 2 zaměstnanců.  Přední ukončí sloužit jako koncové body protokolu HTTP/HTTPS a směrování přenosů pracovníkům, které jsou role hostujících aplikace.   Můžete upravit množství po vytvoření pomocného mechanismu řízení a můžete dokonce nastavit automatické měřítko pravidla na tyto fondů zdrojů.  Podrobné informace kolem ruční změna velikosti, Správa a sledování prostředí aplikace služby přejděte sem: [jak nakonfigurovat prostředí aplikace služby][ASEConfig] 

Pouze jeden pomocného mechanismu řízení může existovat v podsítě použité pomocným.  Podsítě není možné použít pro jinou hodnotu než pomocného mechanismu řízení

### <a name="after-app-service-environment-creation"></a>Po vytvoření prostředí aplikace služby ###

Po vytvoření pomocného mechanismu řízení je možné upravit.

- Počet front-end (minimální: 2)
- Počet zaměstnanců (minimální: 2)
- Rozsah IP adresy dostupné pro IP SSL
- Výpočet velikosti zdrojů používaných front-end nebo dalšími lidmi (front-end minimální velikost je P2)


Existují další podrobnosti o ruční měřítko, Správa a sledování aplikací služby prostředí tady: [Konfigurace prostředí aplikace služby][ASEConfig] 

Další informace o neobsahovaly text je příručku tady: [jak nakonfigurovat automatické měřítko pro prostředí aplikace služby][ASEAutoscale]

Existují další závislosti, které nejsou k dispozici pro přizpůsobení třeba databáze a úložiště.  Jedná se uskutečněných jednotlivými Azure, které jsou součástí systému.  Úložiště systém podporuje maximálně 500 GB pro celé prostředí aplikace služby a databáze se upraví tak, že Azure podle potřeby měřítka systému.


## <a name="getting-started"></a>Začínáme
Všechny články a jak-pro uživatele pro aplikaci služby prostředí jsou k dispozici v [souboru README pro aplikaci služby prostředí](../app-service/app-service-app-service-environments-readme.md).

Začínáme s prostředím služby aplikace v tématu [Úvod do prostředí aplikace služby][WhatisASE]

Další informace o aplikaci služby Azure platformy, najdete v článku [Aplikace služby Azure][AzureAppService].

[AZURE.INCLUDE [app-service-web-whats-changed](../../includes/app-service-web-whats-changed.md)]

[AZURE.INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]
 

<!--Image references-->
[1]: ./media/app-service-web-how-to-create-an-app-service-environment/asecreate-basecreateblade.png
[2]: ./media/app-service-web-how-to-create-an-app-service-environment/asecreate-vnetcreation.png

<!--Links-->
[WhatisASE]: http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-intro/
[ASEConfig]: http://azure.microsoft.com/documentation/articles/app-service-web-configure-an-app-service-environment/
[AppServicePricing]: http://azure.microsoft.com/pricing/details/app-service/ 
[AzureAppService]: http://azure.microsoft.com/documentation/articles/app-service-value-prop-what-is/ 
[ASEAutoscale]: http://azure.microsoft.com/documentation/articles/app-service-environment-auto-scale/
[ILBASE]: http://azure.microsoft.com/documentation/articles/app-service-environment-with-internal-load-balancer/
[ILBAseTemplate]: http://azure.microsoft.com/documentation/templates/201-web-app-ase-create/
[ASEfromTemplate]: http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-create-ilb-ase-resourcemanager/
