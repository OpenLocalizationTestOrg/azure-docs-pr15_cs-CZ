<properties
    pageTitle="Služba Azure aplikací pro web, mobilní telefon a rozhraní API aplikace | Microsoft Azure"
    description="Přečtěte si aplikaci služby Azure vám pomůže vývoj, nasazení a správu web mobilních aplikací."
    keywords="aplikaci služby azure aplikaci služby, aplikaci služby náklady, měřítko, scalable, nasazení aplikace, nasazení azure aplikace, paas, platformy jako služby, webu, webu, web, azure mobilní"
    services="app-service"
    documentationCenter=""
    authors="omarkmsft"
    manager="erikre"
    editor="cephalin"/>

<tags
    ms.service="app-service"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/26/2016"
    ms.author="omark"/>

# <a name="what-is-azure-app-service"></a>Co je aplikace služby Azure?

*Aplikace služby* je [platformy jako služby](https://en.wikipedia.org/wiki/Platform_as_a_service) (PaaS) nabízející z Microsoft Azure. Vytvořte web a mobilní aplikace pro platformu nebo zařízení. Integrace aplikace s SaaS řešení, komunikovat s místní aplikací a automatizace firemních procesů. Azure spuštěna aplikace na plně spravovaných virtuálních počítačích (VMs), s volbou OM sdílených nebo vyhrazené VMs.

Aplikaci služby včetně web a mobilní funkcí, které jsme dříve samostatně Doručená jako Azure webů a služby Azure Mobile. Nové funkce pro automatizaci obchodních procesů a hostování cloudu rozhraní API také. Jako jediná integrovaná služba aplikaci služby vám umožní vytvořte různými součástmi – weby, zpět konce mobilní aplikaci, RESTful rozhraní API a obchodních procesů – do jednoho řešení.

Následující video 4 minutu obsahuje stručný skladovými aplikaci služby starší Azure nabídky a co je nového v nich.

+[AZURE.VIDEO app-service-history-lesson]

## <a name="why-use-app-service"></a>Proč používat aplikaci služby?

Tady jsou některé klíčové funkce a funkce aplikace služby:

- **Více jazyků a rámce** - aplikace přihlašovacích údajů podporuje prvotřídní pro ASP.NET Node.js, Java, PHP a Python. Můžete taky spustit [Windows Powershellu a jiných skripty nebo spustitelné soubory](../app-service-web/web-sites-create-web-jobs.md) v aplikaci služby VMs.

- **Optimalizace DevOps** – nastavení [Nepřetržitý integrace a nasazení](../app-service-web/app-service-continuous-deployment.md) aplikace Visual Studio týmovou, GitHub nebo BitBucket. Zvýšení úrovně aktualizace prostřednictvím [test a pracovní prostředí](../app-service-web/web-sites-staged-publishing.md). Provedení [A a B testování](../app-service-web/app-service-web-test-in-production-get-start.md). Správa aplikací v aplikaci služby pomocí [Prostředí PowerShell Azure](../powershell-install-configure.md) nebo [různé platformy rozhraní příkazového řádku (rozhraní příkazového řádku)](../xplat-cli-install.md).

- **Globální měřítko s vysokou dostupnost** – měřítko [nahoru](../app-service-web/web-sites-scale.md) nebo [,](../monitoring-and-diagnostics/insights-how-to-scale.md) ať už ručně nebo automaticky. Hostovat aplikace kdekoli v globální datacentra infrastruktury společnosti Microsoft a služba aplikace [SLA](https://azure.microsoft.com/support/legal/sla/app-service/) slibuje dostupnost.

- **Připojení k SaaS platformách a místní data** – z víc než 50 [spojnic](../connectors/apis-list.md) pro systémy enterprise (například SAP, Siebel a Oracle), zvolte SaaS služeb (například Salesforce a Office 365) a internetové služby (třeba Facebook a Twitter). Místní přístup k datům pomocí [Hybridní připojení](../biztalk-services/integration-hybrid-connection-overview.md) a [Azure virtuální sítě](../app-service-web/web-sites-integrate-with-vnet.md).

- **Zabezpečení a dodržování předpisů** - aplikaci služby je [ISO, SOC a PCI kompatibilní](https://www.microsoft.com/TrustCenter/).

- **Šablony aplikace** - zvolte z rozsáhlého seznamu šablon aplikace v [Azure Marketplace](https://azure.microsoft.com/marketplace/) , které umožňují použít průvodce nainstalovat Oblíbené zdroje otevřít software například WordPress, Joomla a Drupal.

- **Integrace aplikace visual Studio** - vyhrazené nástroje ve Visual Studiu zjednodušit práce pro vytváření, nasazení a ladění.

## <a name="app-types-in-app-service"></a>Typy aplikace v aplikaci služby

Aplikace služby nabízí několik *typů aplikace*, přičemž každý z nich je určená hostovat konkrétní druh pracovní zátěž:

- [**Web Apps**](../app-service-web/app-service-web-overview.md) – pro hostování webů a webových aplikací.

- [**Mobilní aplikace**](../app-service-mobile/app-service-mobile-value-prop.md) Pro hostování mobilní aplikace zpátky konce.

- [**Rozhraní API aplikace**](../app-service-api/app-service-api-apps-why-best-platform.md) – pro hostování RESTful rozhraní API.

- [**Použití logických operátorů aplikace**](../app-service-logic/app-service-logic-what-are-logic-apps.md) – pro automatizaci obchodních procesů a integrace systémy a dat přes mračnech bez psaní kódu.

Word *aplikace* tady odkazuje hostingu zdroje snaží o spuštění úlohu. Pořizování "v prohlížeči" jako příklad, budete asi zvyklí přemýšlení do webových aplikací pro využití prostředků a kód aplikace, který společně poskytovat funkce na prohlížeč. Ale v aplikaci služby *v prohlížeči* výpočetním prostředky, které Azure poskytuje pro hostování kód aplikace. Aplikace se skládá z webových front-end a rozhraní API RESTful serverová, může nasazení obojí můžete do webových aplikací nebo může nasadíte front-end kód do webových aplikací a back-end kód do aplikace pro rozhraní API. Aplikace se skládá z více aplikací aplikaci služby různých druhů.

## <a name="app-service-plans"></a>Aplikace služby plány jednotného zasílání zpráv

[Aplikace služby plány jednotného zasílání zpráv](azure-web-sites-web-hosting-plans-in-depth-overview.md) zadejte typ zdroje výpočetním spuštěné v operačním systému aplikace. Pokud očekáváte light přenosy zatížení, můžete použít sdílené VMs (**zdarma** a **sdílené** ceny úrovně). Pro vyšší zatížení můžete vybrat z několika velikosti vyhrazené VMs (**základní** **Standardní**a **Premium** úrovně). Více aplikací aplikaci služby můžete sdílet se stejným plánem a jejich velikost nahoru nebo dolů ceny úrovní pohromadě v plánu.

Pokud potřebujete další škálovatelnost a izolace sítě, můžete spustit aplikací v [Prostředí aplikace služeb](../app-service-web/app-service-app-service-environment-intro.md).

## <a name="pricing"></a>Ceny

Informace o tom, náklady množství aplikaci služby najdete v článku [Aplikace služby ceny](https://azure.microsoft.com/pricing/details/app-service/).

## <a name="test-drive-app-service"></a>Zjišťovat aplikace služby

[Vytvořit web appu vzorku, mobilní aplikace nebo aplikace logiku](http://go.microsoft.com/fwlink/?LinkId=523751) a přehráním s ním pro 1 hodinu, bez platební kartou potřeby žádné závazky žádné potíže.

Nebo si potřebujete založit [účet Azure zdarma](https://azure.microsoft.com/pricing/free-trial/)a proveďte jednu z našich výukové programy Začínáme –:

* [Kurz: Vytváření do webových aplikací](../app-service-web/app-service-web-get-started.md)
* [Kurz: Vytváření mobilní aplikaci](../app-service-mobile/app-service-mobile-android-get-started.md)
* [Kurz: Vytvoření aplikace pro rozhraní API](../app-service-api/app-service-api-dotnet-get-started.md)
* [Kurz: Vytvoření aplikace logiky](../app-service-logic/app-service-logic-create-a-logic-app.md)
