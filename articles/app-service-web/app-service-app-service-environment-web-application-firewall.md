<properties 
    pageTitle="Konfigurace brány Firewall webové aplikace (WAF) pro aplikaci služby prostředí" 
    description="Zjistěte, jak nakonfigurovat bránu firewall webové aplikace před prostředí aplikace služby." 
    services="app-service\web" 
    documentationCenter="" 
    authors="naziml" 
    manager="wpickett" 
    editor="jimbe"/>

<tags 
    ms.service="app-service" 
    ms.workload="web" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/17/2016" 
    ms.author="naziml"/>    

# <a name="configuring-a-web-application-firewall-waf-for-app-service-environment"></a>Konfigurace brány Firewall webové aplikace (WAF) pro aplikaci služby prostředí

## <a name="overview"></a>Základní informace ##
Webová aplikace bránách firewall jako [Barracuda WAF pro Azure](https://www.barracuda.com/programs/azure) , která je dostupná na zabezpečené že webových aplikací kontrolou příchozí web data blokovat vložení SQL, skriptování, malware nahrávání a aplikace Denial a jinými útoky pomáhá [Azure Marketplace](https://azure.microsoft.com/marketplace/partners/barracudanetworks/waf-byol/) . Je také zkontroluje, zda obsahuje odpovědi z back-end webových serverů pro prevenci ztráty dat (DLP). Spojení s izolace a další stejné měřítko poskytované aplikace služby prostředí, zajistíte ideální prostředí pro hostování firmy kritické webových aplikací, které je potřeba nebyly škodlivým žádosti o schůzku a velkého množství přenosů.

+[AZURE.INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)] 

## <a name="setup"></a>Nastavení ##
Pro tento dokument, který jsme nastaví její konfiguraci a naše prostředí aplikace služby za více zatížení rovnoměrně výskyty Barracuda WAF tak, aby jenom adres WAF zobrazíte prostředí aplikace služby a nebude blokům DMZ. Jsme bude mít taky zajištěný správce dopravy Azure před naše instance Barracuda WAF Vyrovnávání zatížení přes Azure datacentrech a oblastí. Nejvyšší úrovně diagram nastavení výsledek podobný jako obsah zobrazený dole.

![Architektura][Architecture] 

> Poznámka: Zavedením [ILB podpory pro aplikaci služby prostředí](app-service-environment-with-internal-load-balancer.md)můžete nakonfigurovat pomocného mechanismu řízení a být nedostupné z DMZ pouze možné privátní sítě. 

## <a name="configuring-your-app-service-environment"></a>Konfigurace prostředí aplikace služby ##
Konfigurace prostředí aplikace služby v nápovědě k [naše si přečtěte následující dokumentaci](app-service-web-how-to-create-an-app-service-environment.md) si toto téma prostudujte. Až budete mít prostředí služby aplikaci vytvořili, můžete vytvořit [Web Apps](app-service-web-overview.md), [Aplikace rozhraní API](../app-service-api/app-service-api-apps-why-best-platform.md) a [Mobilní aplikace](../app-service-mobile/app-service-mobile-value-prop.md) v tomto prostředí, které budou všechny chráněny za WAF jsme konfigurace v další části.

## <a name="configuring-your-barracuda-waf-cloud-service"></a>Konfigurace služby cloudu WAF Barracuda ##
Barracuda obsahuje [podrobný článek](https://campus.barracuda.com/product/webapplicationfirewall/article/WAF/DeployWAFInAzure) o nasazení jeho WAF na počítač virtuální v Azure. Ale protože jsme má redundance a ne Představte selhání v jednom místě, kterou chcete nasadit alespoň na úrovni 2 VMs instance WAF do stejné cloudové služby při následujícího postupu.

### <a name="adding-endpoints-to-cloud-service"></a>Přidání koncové body cloudových služeb ###
Až budete mít 2 nebo více WAF OM instancí služby cloudu slouží k přidání HTTP a HTTPS koncové body, které se používají v aplikaci, jak je znázorněno na následujícím obrázku [je portál Azure](https://portal.azure.com/) .

![Konfigurace koncového bodu][ConfigureEndpoint]

Pokud aplikace použít jiné koncové body, zkontrolujte, že přidejte je do seznamu nepromítnou stejně. 

### <a name="configuring-barracuda-waf-through-its-management-portal"></a>Konfigurace Barracuda WAF prostřednictvím portálu pro správu ###
Barracuda WAF používá TCP Port 8000 pro konfiguraci pomocí portálu pro správu. Když máme víc instancí WAF VMs, budete muset zopakujte tímto postupem pro jednotlivé instance OM. 


> Poznámka: Po dokončení konfigurace WAF odebráním koncového bodu TCP/8000 všechny vaše WAF VMs zabezpečit vaše WAF.

Přidejte koncový bod správy, jak je vidět na následujícím obrázku konfigurace Barracuda WAF.

![Přidání koncový bod správy][AddManagementEndpoint]
 
Použijte jiný prohlížeč umožňuje přecházet na Správa koncového bodu na vaše cloudové služby. Pokud vaše cloudové služby je místo toho test.cloudapp.net, byste měli přístup k tento koncový bod procházením http://test.cloudapp.net:8000. Byste měli vidět přihlašovací stránky jako pod, že se přihlásíte pomocí přihlašovacích údajů zadaná ve fázi WAF OM nastavení.

![Správa přihlašovací stránka][ManagementLoginPage]

Po přihlášení můžete byste měli vidět řídicího panelu jako na následujícím obrázku, který bude prezentovat základních statistických údajů o ochraně WAF.

![Řídicí panel pro správu][ManagementDashboard]

Po kliknutí na kartu služby bude umožňují konfigurovat WAF pro služby, které je ochrana. Podrobné informace o konfiguraci Barracuda WAF se může v dokumentaci [jejich](https://techlib.barracuda.com/waf/getstarted1). V dalším příkladu vrací webovou aplikaci Azure podávání přenosy na HTTP a HTTPS nakonfigurované.

![Správa přidat služby][ManagementAddServices]

> Poznámka: V závislosti na konfiguraci aplikace a jaké funkce jsou používány v prostředí aplikace služby, budete muset přenosu pro TCP porty než 80 a 443, například pokud máte IP SSL nastavení pro Web App. Seznam síťové porty používané v aplikaci služby prostředí naleznete v [dokumentaci ovládací prvek příchozí přenosy](app-service-app-service-environment-control-inbound-traffic.md) síťové porty oddíl.

## <a name="configuring-microsoft-azure-traffic-manager-optional"></a>Konfigurace Microsoft Azure přenosy správce (volitelné) ##
Pokud pak budete chtít načíst je k dispozici ve více oblastech aplikace zůstatek je za [Azure přenosy správce](../traffic-manager/traffic-manager-overview.md). K tomu nevyzve můžete přidat koncový bod [Azure klasické portálu](https://manage.azure.com) nazvanou podle cloudové služby pro vaše WAF v profilu přenosy správce jak je znázorněno na následujícím obrázku. 

![Koncový bod přenosy správce][TrafficManagerEndpoint]

Pokud aplikace vyžaduje ověřování, zkontrolujte, zda že jsou některé zdroje, který nevyžaduje žádné ověřování pro správce přenosy ping dostupnost aplikace. Adresu URL v části konfigurovat můžete nakonfigurovat pro [Azure klasické portálu](https://manage.azure.com) , jak je ukázáno v následujícím příkladu.

![Konfigurace přenosy správce][ConfigureTrafficManager]

Přesměrovat příkazu ping přenosy správce z vaší WAF aplikaci, musíte na nastavení webu překladů na Barracuda WAF pro přenos dat do aplikace, jak je vidět v následujícím příkladu.

![Web překladů][WebsiteTranslations]

## <a name="securing-traffic-to-app-service-environment-using-network-security-groups-nsg"></a>Zabezpečení přenosů na prostředí aplikace služeb pomocí sítě skupin zabezpečení (NSG)##
Postupujte podle [si přečtěte následující dokumentaci ovládací prvek příchozí přenosy](app-service-app-service-environment-control-inbound-traffic.md) podrobné informace o omezení přenosy prostředí služby aplikace z WAF jen kontaktovat pomocí adresy VIP cloudové služby. Tady je ukázka příkazu Powershellu pro provedení tohoto úkolu pro TCP port 80.


    Get-AzureNetworkSecurityGroup -Name "RestrictWestUSAppAccess" | Set-AzureNetworkSecurityRule -Name "ALLOW HTTP Barracuda" -Type Inbound -Priority 201 -Action Allow -SourceAddressPrefix '191.0.0.1'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '80' -Protocol TCP

Nahraďte SourceAddressPrefix virtuální IP adresu (VIP) vaší WAF cloudové služby.

> Poznámka: Virtuální cloudové služby se změní, když odstraníte a znovu vytvořit Cloudovou službu. Zkontrolujte, že po provedete aktualizovat na IP adresu ve skupině sítě zdroje. 
 
<!-- IMAGES -->
[Architecture]: ./media/app-service-app-service-environment-web-application-firewall/Architecture.png
[ConfigureEndpoint]: ./media/app-service-app-service-environment-web-application-firewall/ConfigureEndpoint.png
[AddManagementEndpoint]: ./media/app-service-app-service-environment-web-application-firewall/AddManagementEndpoint.png
[ManagementAddServices]: ./media/app-service-app-service-environment-web-application-firewall/ManagementAddServices.png
[ManagementDashboard]: ./media/app-service-app-service-environment-web-application-firewall/ManagementDashboard.png
[ManagementLoginPage]: ./media/app-service-app-service-environment-web-application-firewall/ManagementLoginPage.png
[TrafficManagerEndpoint]: ./media/app-service-app-service-environment-web-application-firewall/TrafficManagerEndpoint.png
[ConfigureTrafficManager]: ./media/app-service-app-service-environment-web-application-firewall/ConfigureTrafficManager.png
[WebsiteTranslations]: ./media/app-service-app-service-environment-web-application-firewall/WebsiteTranslations.png
