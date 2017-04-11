<properties
    pageTitle="Jaké mobilní aplikace"
    description="Zjistěte, jaké výhody aplikaci služby přenesení mobilních aplikací vaší organizace."
    services="app-service\mobile"
    documentationCenter=""
    authors="adrianhall"
    manager="yochayk"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="na"
    ms.tgt_pltfrm="mobile-multiple"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="10/01/2016"
    ms.author="adrianha"/>

# <a name="getting-started"> </a>Co je aplikace Mobile?

Azure aplikaci služby je plně spravovaných [platformy jako služba](https://azure.microsoft.com/overview/what-is-paas/) (PaaS) nabízející pro vývojáře profesionální, který bohatou sadu funkcí web, mobilní a scénáře integrace. *Mobilní aplikace* v *Aplikaci služby Azure* nabízejí vývojové platformy vysoce scalable, globálně dostupné mobilní aplikace pro vývojáře organizace a doplňky systému, která přiblíží bohatou sadu funkcí k mobilní vývojáři.

![Mobilní aplikace](./media/app-service-mobile-value-prop/overview.png)

##<a name="why-mobile-apps"></a>Proč mobilní aplikace?
*Mobilní aplikace* v *Aplikaci služby Azure* nabízí vývojové platformy vysoce scalable, globálně dostupné mobilní aplikace pro vývojáře organizace a doplňky systému, která přiblíží bohatou sadu funkcí k mobilní vývojáři. S aplikací Mobile máte tyto možnosti:

- **Vytvoření nativní a křížové platformy aplikace** - zda vytváříte nativní iOS, Android a Windows aplikace nebo různé platformy Xamarin nebo Cordova (Phonegap) aplikace, můžete využít služby aplikace pomocí nativního SDK.
- **Připojit k podnikové systémy** – pomocí mobilní aplikace můžete přidat podnikové znaménko na v minutách a připojení k vaší organizace na místní nebo zdroje v cloudu.
- **Vytvoření offline použité aplikace se synchronizací dat** – provést mobilní pracovníků produktivní vytváření aplikací, které fungují v režimu offline a provádějte mobilní aplikace na synchronizovat data na pozadí je k dispozici pomocí kterékoli z zdroje dat organizace nebo SaaS API připojení.
- **Nabízená oznámení na miliony v sekundách** - zapojit zákazníků pomocí rychlé nabízená oznámení na libovolném zařízení, přizpůsobit pro své potřeby odeslané čas je správné.

## <a name="mobile-app-features"></a>Funkce mobilní aplikaci
Tyto funkce jsou důležité do cloudu s podporou vývoj pro mobilní zařízení:

- **A mohli ověřovat** – vyberte ze seznamu někdy Kvetoucí Zprostředkovatelé identit jiní, včetně Azure Active Directory pro ověřování enterprise plus sociální poskytovatelů jako je Facebook, Google, Twitteru a Account Microsoft.  Azure aplikací Mobile obsahuje OAuth 2.0 služby pro každého poskytovatele.  Taky můžete integrovat SDK pro poskytovatele identity pro konkrétní funkce poskytovatele.

  Zjistěte víc o naše [funkcí ověřování].

- Poskytuje **Přístup k datům** - Azure mobilní aplikace ovládáním mobile OData v3 zdroje dat propojené s SQL Azure nebo místní SQL serveru.  Tuto službu můžou být založená na Entity Framework umožňuje snadno integrace s jiných NoSQL a SQL zprostředkovatelé dat, včetně [Úložiště tabulek Azure]MongoDB, [DocumentDB] a rozhraní API SaaS zprostředkovatelé, jako je Office 365 a Salesforce.com.
- **Offline synchronizace** - zpřístupněte náš klienta SDK snadno vytvářet výkonné a citlivé mobilní aplikace, které pracují s daty v režimu offline sadu můžete automaticky synchronizovat se back-end data, včetně podpory řešení konfliktu.

  Zjistěte víc o naše [data funkce].

- **Nabízená oznámení** - Our klienta SDK Bezproblémová integrace s možnostmi zápisu rozbočovače oznámení Azure, což vám odeslání nabízená oznámení miliony uživatelů najednou.

  Zjistěte víc o naše [nabízená oznámení funkce].

- **Klient SDK** – úplná sada SDK klienta, který zahrnovat rozvoje nativní ([iOS], [Android] a [Windows]), nabízíme vývojové platformy ([Xamarin pro iOS a Android], [Xamarin formulářů]) a vývoj aplikací hybridní ([Apache Cordova]).  Každý klient SDK je k dispozici s licencí MIT a otevřít zdroj.

## <a name="azure-app-service-features"></a>Funkce Azure aplikaci služby.
Následující funkce platformy jsou obvykle užitečné pro mobilní provozní weby.

- **Automatické změny velikosti** – aplikaci služby umožňuje rychle měřítko zdola nahoru nebo out zpracovávání všechny příchozí zatížení zákazníka. Ruční vyberte počet a velikost VMs nebo nastavit automatické měřítko měřítko vaší back-end mobilní aplikace založené na načíst nebo plánu.

  Zjistěte víc o [Automatické měřítko].

- **Pracovní prostředí** – aplikaci služby mohlo by umožnit spuštění více verzí webu, což umožňuje provádět A / testování B testovat v výrobní jako součást větší DevOps plán a v místní pracovní novou back-end.

  Zjistěte další informace o [pracovní prostředí].

- **Nepřetržitý nasazení** – aplikaci služby můžete integrovat s běžné Správce služeb systémy, umožňuje automaticky nasazení nová verze vaší back-end stisknutím na větev systému správce služeb.

  Zjistěte další informace o [možnostech nasazení].

- **Virtuální sítě** – aplikaci služby můžete připojit k místní zdrojů prostřednictvím virtuální sítě, ExpressRoute nebo hybridní připojení.

  Zjistěte další informace o [hybridním připojení], [virtuální sítě]a [ExpressRoute].

- **Izolace / vyhrazené prostředí** – můžete v plně izolace a vyhrazené prostředí pro bezpečné spuštění aplikace služby Azure aplikace ve velkém měřítku vysoké spustit aplikaci služby.  Toto je ideální pro aplikace úloh vyžadující velmi vysoké měřítko, izolace nebo bezpečný přístup.

  Zjistěte další informace o [Aplikaci služby prostředí].

## <a name="getting-started"></a>Začínáme ##
Začínáme s aplikací Mobile, postupujte podle kurzu [Začít pracovat] .  Tím se zabývat těmito oblastmi základní informace o vytváření mobilní back-end a klienta podle svého výběru a potom integrace ověřování, offline synchronizace a nabízená oznámení.  Můžete sledovat kurz [Seznámení] několikrát - jednou pro každý klientské aplikaci.

Další informace o Azure mobilní aplikace se seznamte s naše [výuky mapa].
Další informace o aplikaci služby Azure platformy najdete v článku [Aplikace služby Azure].

>[AZURE.NOTE]Pokud chcete začít pracovat s aplikaci služby Azure před registrací účet Azure, přejděte na [Zkuste aplikaci služby](https://tryappservice.azure.com/?appServiceName=mobile), které můžete okamžitě vytvořit web appu krátkodobý starter v aplikaci služby. Žádné povinné; kreditní karty žádné závazky.

<!-- URLs. -->
[Migrate your Mobile Service to App Service]: app-service-mobile-migrating-from-mobile-services.md
[Azure aplikace služby]: ../app-service/app-service-value-prop-what-is.md
[Začínáme]: app-service-mobile-ios-get-started.md
[Úložiště tabulek Azure]: ../storage/storage-getting-started-guide.md
[DocumentDB]: ../documentdb/documentdb-get-started.md
[funkce ověřování]: ./app-service-mobile-auth.md
[Funkce dat]: ./app-service-mobile-offline-data-sync.md
[Funkce nabízená oznámení]: ../notification-hubs/notification-hubs-push-notification-overview.md
[iOS]: ./app-service-mobile-ios-how-to-use-client-library.md
[Android]: ./app-service-mobile-android-how-to-use-client-library.md
[Windows]: ./app-service-mobile-dotnet-how-to-use-client-library.md
[Xamarin pro iOS a Android]: ./app-service-mobile-dotnet-how-to-use-client-library.md
[Xamarin formuláře]: ./app-service-mobile-xamarin-forms-get-started.md
[Apache Cordova]: ./app-service-mobile-cordova-how-to-use-client-library.md
[automatické změny velikosti]: ../app-service-web/web-sites-scale.md
[pracovní prostředí]: ../app-service-web/web-sites-staged-publishing.md
[Možnosti nasazení]: ../app-service-web/web-sites-deploy.md
[hybridní připojení]: ../app-service-web/web-sites-hybrid-connection-get-started.md
[virtuální sítě]: ../app-service-web/web-sites-integrate-with-vnet.md
[ExpressRoute]: ../app-service-web/app-service-app-service-environment-network-configuration-expressroute.md
[Prostředí aplikace služby]: ../app-service-web/app-service-app-service-environment-intro.md
[Přehled výukových mapy]: https://azure.microsoft.com/en-us/documentation/learning-paths/appservice-mobileapps/
