<properties 
    pageTitle="Prostředí aplikace služeb | Microsoft Azure" 
    description="Co je prostředí aplikace služby Azure? Úvod do prostředí aplikace služeb." 
    keywords="Azure aplikace služby prostředí virtuální sítě, zabezpečené sítě"
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

# <a name="app-service-environment-documentation"></a>Si přečtěte následující dokumentaci prostředí aplikace služby

Prostředí aplikace služby je [Premium] [ PremiumTier] služby možnost plán služby Azure aplikace, která obsahuje plně izolace a vyhrazené prostředí pro bezpečné spuštění aplikace aplikaci služby Azure ve velkém měřítku vysoké, včetně [Webových aplikací Web Apps][WebApps], [Mobilní aplikace][MobileApps]a [Rozhraní API aplikace][APIApps].  

Aplikace služby prostředí jsou ideální pro aplikace úloh vyžadující:

- Velmi vysoké měřítko
- Izolace a bezpečný přístup

Zákazníci můžete vytvořit aplikaci služby prostředí s více v jedné oblasti Azure i napříč několika Azure oblastí.  Díky aplikaci služby prostředí ideální pro vodorovně měřítka stavu méně aplikací úrovní o vysoké RPS úloh.

Aplikaci služby prostředí izolovaná na spuštěn pouze jeden zákazníka aplikace a jsou vždy nasazené do virtuálního sítě.  Zákazníci měli jemně odstupňovaná kontrolu nad obou příchozí a odchozí aplikace v síti pomocí [skupin zabezpečení sítě][NetworkSecurityGroups].  Aplikace můžete také vytvořit vysokorychlostní zabezpečené připojení přes virtuální sítě pro místní podnikové prostředky.

Aplikace často nutné přístup k podnikové zdroje, jako jsou interní databází a webové služby.  Aplikace spuštěné v aplikaci služby prostředí můžete získat informace dostupné prostřednictvím [Webu webu] [ SiteToSite] VPN a [Azure ExpressRoute] [ ExpressRoute] připojení.

* [Co je prostředí služby aplikace?](../app-service-web/app-service-app-service-environment-intro.md)
* [Vytvoření prostředí aplikace služby](../app-service-web/app-service-web-how-to-create-an-app-service-environment.md)
* [Vytváření aplikací v prostředí aplikace služby](../app-service-web/app-service-web-how-to-create-a-web-app-in-an-ase.md)
* [Vytváření a používání vyrovnávání zatížení vnitřní s prostředím aplikace služby](../app-service-web/app-service-environment-with-internal-load-balancer.md)
* [Konfigurace prostředí aplikace služby](../app-service-web/app-service-web-configure-an-app-service-environment.md) 
* [Změna měřítka aplikací v prostředí aplikace služby](../app-service-web/app-service-web-scale-a-web-app-in-an-app-service-environment.md)
* [Zabezpečení sítě a architekturu](../app-service-web/app-service-app-service-environment-network-architecture-overview.md)

## <a name="how-tos"></a>Jak společnosti

[AZURE.INCLUDE [app-service-blueprint-app-service-environment](../../includes/app-service-blueprint-app-service-environment.md)]


## <a name="videos"></a>Videa
[AZURE.VIDEO azurecon-2015-deploying-highly-scalable-and-secure-web-and-mobile-apps]

[AZURE.VIDEO microsoft-ignite-2015-running-enterprise-web-and-mobile-apps-on-azure-app-service]


<!-- LINKS -->
[PremiumTier]: http://azure.microsoft.com/pricing/details/app-service/
[WebApps]: http://azure.microsoft.com/documentation/articles/app-service-web-overview/
[MobileApps]: http://azure.microsoft.com/documentation/articles/app-service-mobile-value-prop-preview/
[APIApps]: http://azure.microsoft.com/documentation/articles/app-service-api-apps-why-best-platform/
[NetworkSecurityGroups]: https://azure.microsoft.com/documentation/articles/virtual-networks-nsg/
[SiteToSite]: https://azure.microsoft.com/documentation/articles/vpn-gateway-site-to-site-create/
[ExpressRoute]: http://azure.microsoft.com/services/expressroute/
