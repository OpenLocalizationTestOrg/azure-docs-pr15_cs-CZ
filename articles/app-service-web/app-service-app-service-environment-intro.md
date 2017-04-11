<properties 
    pageTitle="Úvod do prostředí aplikace služby" 
    description="Informace o funkci prostředí služby aplikace, která obsahuje zabezpečené připojení ke VNet, vyhrazené jednotkách spuštění všechny vaše aplikace." 
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

# <a name="introduction-to-app-service-environment"></a>Úvod do prostředí aplikace služby

## <a name="overview"></a>Základní informace ##
Prostředí aplikace služby je [Premium] [ PremiumTier] služby možnost plán služby Azure aplikace, která obsahuje plně izolace a vyhrazené prostředí pro bezpečné spuštění aplikace aplikaci služby Azure ve velkém měřítku vysoké, včetně [Webových aplikací Web Apps][WebApps], [Mobilní aplikace][MobileApps]a [Rozhraní API aplikace][APIApps].  

Aplikace služby prostředí jsou ideální pro aplikace úloh vyžadující:

- Velmi vysoké měřítko
- Izolace a bezpečný přístup

Zákazníci můžete vytvořit aplikaci služby prostředí s více v jedné oblasti Azure i napříč několika Azure oblastí.  Díky aplikaci služby prostředí ideální pro vodorovně měřítka stavu méně aplikací úrovní o vysoké RPS úloh.

Aplikaci služby prostředí izolovaná na spuštěn pouze jeden zákazníka aplikace a jsou vždy nasadit do virtuálního sítě.  Zákazníci měli jemně odstupňovaná kontrolu nad obou příchozí a odchozí aplikace v síti a aplikací lze vytvořit vysokorychlostní zabezpečené připojení přes virtuální sítě pro místní podnikové prostředky.

Všechny články a jak-pro uživatele o prostředí služby aplikace jsou dostupné v [souboru README pro aplikaci služby prostředí](../app-service/app-service-app-service-environments-readme.md).

Základní informace o způsobu prostředí aplikace služby vysoké měřítko povolit a zabezpečit přístup k síti najdete v článku [Hloubkové postupy AzureCon] [ AzureConDeepDive] v aplikaci služby prostředí!

Postupy důkladné na vodorovném škálování pomocí prostředí služby s více aplikací najdete v článku o tom, jak nastavit [stopu geo distribuované aplikace][GeodistributedAppFootprint].

Konfigurace Architektura zabezpečení v hloubkové postupy AzureCon najdete v článku o implementaci [vrstvách Architektura zabezpečení](app-service-app-service-environment-layered-security.md) s prostředím služby aplikace.

Aplikace spuštěné v aplikaci služby prostředí můžete využívat jejich závislé na nadřazeného zařízení, jako jsou webové aplikace brány (WAF).  V článku o [konfiguraci WAF pro aplikaci služby prostředí](app-service-app-service-environment-web-application-firewall.md) popisuje tento scénář. 

[AZURE.INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)] 

## <a name="dedicated-compute-resources"></a>Vyhrazená pro využití prostředků ##
Všechny zdroje výpočetním v prostředí aplikace služby jsou určené pouze pro jeden předplatné a prostředí aplikace služby lze nastavit až 50 (50) pro využití prostředků využívat v jedné aplikaci.

Prostředí služby aplikace se skládá z fondu zdrojů front-end výpočetním, jakož i fondů zdrojů jedno až tři pracovní výpočetním. 

Fondu front-end obsahuje výpočetním prostředky zodpovědný za SSL ukončení jako Vyrovnávání zatížení a automatické požadavků aplikací v prostředí aplikace služby. 

Každý pracovní fond obsahuje výpočetním zdroje přidělené [Aplikace služby plány jednotného zasílání zpráv][AppServicePlan], zase obsahující jeden nebo více aplikací služby Azure aplikace.  Protože v prostředí služby aplikací může být až tři různé pracovní skupiny, máte možnost zvolit jiné výpočetním zdroje pro každý fond pracovní.  

Například to umožňuje vytvořit jeden pracovní fond s méně výkonných výpočetních zdroje pro aplikaci služby plány jednotného zasílání zpráv určené pro vývoj nebo testování aplikace.  Fond druhou (nebo dokonce třetí) pracovních přes výkonnější zdrojů výpočetním určených pro aplikaci služby plány jednotného zasílání zpráv spuštěné výrobní aplikace.

Podrobné informace o počtu pro využití prostředků pro fondu front-end a pracovní postup [Konfigurace prostředí aplikace služby][HowToConfigureanAppServiceEnvironment].  

Podrobné informace o dostupných výpočet zdroje velikosti podporované v prostředí aplikace služby, použijte [Aplikaci služby ceny] [ AppServicePricing] stránky a prohlédněte dostupné možnosti aplikace služby prostředí v Premium ceny osy.

## <a name="virtual-network-support"></a>Podpora virtuální sítě ##
Prostředí aplikace služby dá vytvořit v **obou** virtuální sítě Správce prostředků Azure, **nebo** na klasické nasazení modelu virtuální sítě ([Další informace o virtuálních sítí][MoreInfoOnVirtualNetworks]).  Vzhledem k tomu prostředí aplikace služby vždy existuje v síti virtuální a přesněji v rámci podsítě virtuální síť, můžete využít funkce zabezpečení ve virtuálních sítí řídit obou příchozí a odchozí síťová komunikace.  

Prostředí služby aplikací může být buď internetové s veřejnou IP adresu nebo interní protilehlé s pouze adresu Azure interní zatížení vyrovnávání (ILB).

Můžete použít [skupiny zabezpečení sítě] [ NetworkSecurityGroups] omezení příchozí síťová komunikace podsítě, kde jsou uložená prostředí aplikace služby.  Umožňuje provádět spuštění aplikace za nadřazeného zařízení a služeb, jako je webové aplikace bránách firewall a poskytovatelů SaaS sítě.

Aplikace také často nutné přístup k podnikové zdroje, jako jsou interní databází a webové služby.  Běžné přístup je zpřístupnit tyto koncové body pouze vnitřní síti toku v Azure virtuální sítě.  Jakmile prostředí aplikace služby je připojen k stejné virtuální sítě jako interní služby, aplikací v prostředí se k nim, včetně koncové body dostupné prostřednictvím [Webu webu] [ SiteToSite] a [Azure ExpressRoute] [ ExpressRoute] připojení.

Pro další informace o fungování aplikace služby prostředí virtuální sítě a místní sítě naleznete v následujících článcích na [Architektura sítě][NetworkArchitectureOverview], [Řízení příchozí přenosy][ControllingInboundTraffic]a [Zabezpečené připojení k back-end][SecurelyConnectingToBackends]. 

## <a name="getting-started"></a>Začínáme

Začínáme s aplikací služby prostředí, najdete v článku [Jak k vytvoření aplikace aplikace služby prostředí][HowToCreateAnAppServiceEnvironment]

Všechny články a jak-pro uživatele pro aplikaci služby prostředí jsou k dispozici v [souboru README pro aplikaci služby prostředí](../app-service/app-service-app-service-environments-readme.md).

Další informace o aplikaci služby Azure platformu, najdete v článku [Aplikace služby Azure][AzureAppService].

Základní informace o aplikaci služby prostředí architektura sítě, najdete v článku [Přehled architektura sítě] [ NetworkArchitectureOverview] článek.

Pro informace o použití prostředí aplikace služby s ExpressRoute, naleznete v následujícím článku na [aplikaci služby prostředí a směrování Express][NetworkConfigDetailsForExpressRoute].

[AZURE.INCLUDE [app-service-web-whats-changed](../../includes/app-service-web-whats-changed.md)]

[AZURE.INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]

<!-- LINKS -->
[PremiumTier]: http://azure.microsoft.com/pricing/details/app-service/
[MoreInfoOnVirtualNetworks]: https://azure.microsoft.com/documentation/articles/virtual-networks-faq/
[AppServicePlan]: http://azure.microsoft.com/documentation/articles/azure-web-sites-web-hosting-plans-in-depth-overview/
[HowToCreateAnAppServiceEnvironment]: http://azure.microsoft.com/documentation/articles/app-service-web-how-to-create-an-app-service-environment/
[AzureAppService]: http://azure.microsoft.com/documentation/articles/app-service-value-prop-what-is/
[WebApps]: http://azure.microsoft.com/documentation/articles/app-service-web-overview/
[MobileApps]: http://azure.microsoft.com/documentation/articles/app-service-mobile-value-prop-preview/
[APIApps]: http://azure.microsoft.com/documentation/articles/app-service-api-apps-why-best-platform/
[LogicApps]: http://azure.microsoft.com/documentation/articles/app-service-logic-what-are-logic-apps/
[AzureConDeepDive]:  https://azure.microsoft.com/documentation/videos/azurecon-2015-deploying-highly-scalable-and-secure-web-and-mobile-apps/
[GeodistributedAppFootprint]:  https://azure.microsoft.com/documentation/articles/app-service-app-service-environment-geo-distributed-scale/
[NetworkSecurityGroups]: https://azure.microsoft.com/documentation/articles/virtual-networks-nsg/
[SiteToSite]: https://azure.microsoft.com/documentation/articles/vpn-gateway-site-to-site-create/
[ExpressRoute]: http://azure.microsoft.com/services/expressroute/
[HowToConfigureanAppServiceEnvironment]:  http://azure.microsoft.com/documentation/articles/app-service-web-configure-an-app-service-environment/
[ControllingInboundTraffic]:  https://azure.microsoft.com/documentation/articles/app-service-app-service-environment-control-inbound-traffic/
[SecurelyConnectingToBackends]:  https://azure.microsoft.com/documentation/articles/app-service-app-service-environment-securely-connecting-to-backend-resources/
[NetworkArchitectureOverview]:  https://azure.microsoft.com/documentation/articles/app-service-app-service-environment-network-architecture-overview/
[NetworkConfigDetailsForExpressRoute]:  https://azure.microsoft.com/documentation/articles/app-service-app-service-environment-network-configuration-expressroute/
[AppServicePricing]: http://azure.microsoft.com/pricing/details/app-service/ 

<!-- IMAGES -->

 
