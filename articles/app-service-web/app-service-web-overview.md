<properties
    pageTitle="Webová aplikace přehled | Microsoft Azure"
    description="Zjistěte, jak aplikaci služby Azure pomáhá vyvíjet a hostování webových aplikací"
    services="app-service\web"
    documentationCenter=""
    authors="cephalin"
    manager="erikre"
    editor=""/>

<tags
    ms.service="app-service-web"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/28/2016"
    ms.author="cephalin"/>

# <a name="web-apps-overview"></a>Přehled webových aplikací

*Aplikace služby Web Apps* je plně spravovaných výpočetním platformy, která je optimalizovaná pro hostování webů a webových aplikací. Tuto nabídku [platformy jako služby](https://en.wikipedia.org/wiki/Platform_as_a_service) (PaaS) umožňuje Microsoft Azure se zaměřit na obchodní logiky během Azure stará infrastruktury spustit a měřítko aplikace.

Následující video 5 minut představuje Azure aplikace služby Web Apps.

[AZURE.VIDEO azure-app-service-web-apps-with-yochay-kiriaty]

>[AZURE.INCLUDE [app-service-linux](../../includes/app-service-linux.md)]

## <a name="what-is-a-web-app-in-app-service"></a>Co je web app v aplikaci služby?

V aplikaci služby *web appu* je výpočetním prostředky, které Azure poskytuje hostingu webu nebo webové aplikace.  

Pro využití prostředků může být na vyhrazené nebo sdílené virtuálních počítačích (VMs), v závislosti na ceny osy, které zvolíte. Kód aplikace běží ve spravovaných OM, který je izolovaný od ostatních zákazníků.

Kód může být v jazyce nebo framework podporovaný [Azure aplikaci služby](../app-service/app-service-value-prop-what-is.md), jako je ASP.NET Node.js, Java, PHP nebo Python. Můžete taky spustit skripty, které používají [Powershellu a jiných jazyků skriptování](web-sites-create-web-jobs.md#acceptablefiles) ve web appu.

Příklady scénářů Typická aplikace, které můžete použít Web aplikace pro naleznete v tématu [webové aplikace scénáře](https://azure.microsoft.com/documentation/scenarios/web-app/) a **scénáře a doporučení** část [aplikaci služby Azure, porovnání virtuálních počítačích, struktury služby a služby cloudu](choose-web-site-cloud-service-vm.md#scenarios).

## <a name="why-use-web-apps"></a>Proč používat Web Apps?

Tady jsou některé klíčové funkce aplikace služby, které se vztahují k Web Apps:

- **Více jazyků a rámce** - aplikace přihlašovacích údajů podporuje prvotřídní pro ASP.NET Node.js, Java, PHP a Python. Můžete taky spustit [Powershellu a jiných skripty nebo spustitelné soubory](../app-service-web/web-sites-create-web-jobs.md) v aplikaci služby VMs.

- **Optimalizace DevOps** – nastavení [Nepřetržitý integrace a nasazení](../app-service-web/app-service-continuous-deployment.md) aplikace Visual Studio týmovou, GitHub nebo BitBucket. Zvýšení úrovně aktualizace prostřednictvím [test a pracovní prostředí](../app-service-web/web-sites-staged-publishing.md). Provedení [A a B testování](../app-service-web/app-service-web-test-in-production-get-start.md). Správa aplikací v aplikaci služby pomocí [Prostředí PowerShell Azure](../powershell-install-configure.md) nebo [různé platformy rozhraní příkazového řádku (rozhraní příkazového řádku)](../xplat-cli-install.md).

- **Globální měřítko s vysokou dostupnost** – měřítko [nahoru](../app-service-web/web-sites-scale.md) nebo [,](../monitoring-and-diagnostics/insights-how-to-scale.md) ať už ručně nebo automaticky. Hostovat aplikace kdekoli v globální datacentra infrastruktury společnosti Microsoft a služba aplikace [SLA](https://azure.microsoft.com/support/legal/sla/app-service/) slibuje dostupnost.

- **Připojení k SaaS platformy a místní data** – z víc než 50 [spojnic](../connectors/apis-list.md) pro systémy enterprise (například SAP, Siebel a Oracle), zvolte SaaS služeb (například Salesforce a Office 365) a internetové služby (třeba Facebook a Twitter). Místní přístup k datům pomocí [Hybridní připojení](../biztalk-services/integration-hybrid-connection-overview.md) a [Azure virtuální sítě](../app-service-web/web-sites-integrate-with-vnet.md).

- **Zabezpečení a dodržování předpisů** - aplikaci služby je [ISO, SOC a PCI kompatibilní](https://www.microsoft.com/TrustCenter/).

- **Šablony aplikace** - zvolte z rozsáhlého seznamu šablon aplikace v [Azure Marketplace](https://azure.microsoft.com/marketplace/) , které umožňují použít průvodce nainstalovat Oblíbené zdroje otevřít software například WordPress, Joomla a Drupal.

- **Integrace aplikace visual Studio** - vyhrazené nástroje ve Visual Studiu zjednodušit práce pro vytváření, nasazení a ladění.

Kromě toho web appu můžete využít funkce nabízené [Rozhraní API aplikace](../app-service-api/app-service-api-apps-why-best-platform.md) (jako je CORS podporují) a [Mobilní aplikace](../app-service-mobile/app-service-mobile-value-prop.md) (například nabízená oznámení). Další informace o typech aplikace v aplikaci služby najdete v článku [Přehled služby Azure aplikace](../app-service/app-service-value-prop-what-is.md).

Kromě Web Apps v aplikaci služby Azure nabízí dalších službách, které se dá použít pro hostování webů a webových aplikací. Pro většinu scénářů Web Apps je nejlepší varianta.  Pro microservice architekturu zvažte [Služby struktury](https://azure.microsoft.com/documentation/services/service-fabric)a pokud potřebujete větší kontrolu nad VMs, které váš kód běží na zvažte [Azure virtuálních počítačích](https://azure.microsoft.com/documentation/services/virtual-machines/). Další informace o tom, jak zvolit mezi tyto služby Azure najdete v článku [aplikaci služby Azure virtuálních počítačích, služby struktury a porovnání Cloudovým službám](choose-web-site-cloud-service-vm.md).

## <a name="getting-started"></a>Začínáme

Začít nasazením ukázkový kód pro nové webové aplikace v aplikaci služby, postupujte podle kurzu [nasazení první webovou aplikaci pro Azure v 5 minut](app-service-web-get-started.md) . Musíte bezplatný účet Azure.

Pokud chcete začít pracovat s aplikaci služby Azure před registrací účet Azure, přejděte na [Zkuste aplikaci služby](http://go.microsoft.com/fwlink/?LinkId=523751), které můžete okamžitě vytvořit web appu krátkodobý starter v aplikaci služby. Žádné povinné; kreditní karty žádné závazky.
