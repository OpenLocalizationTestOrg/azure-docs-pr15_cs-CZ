<properties
    pageTitle="Vytvoření webové aplikace z webu Azure Marketplace | Microsoft Azure"
    description="Naučte se vytvářet nové webové aplikace WordPress z Azure Marketplace pomocí portálu Azure."
    services="app-service\web"
    documentationCenter=""
    authors="rmcmurray"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="app-service-web"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="09/20/2016"
    ms.author="robmcm"/>

<!-- Note: This article replaces web-sites-php-web-site-gallery.md -->

# <a name="create-a-web-app-from-the-azure-marketplace"></a>Vytvoření webové aplikace z webu Azure Marketplace

[AZURE.INCLUDE [tabs](../../includes/app-service-web-get-started-nav-tabs.md)]

Azure Marketplace zpřístupní do široké řady oblíbených webových aplikacích vyvinutý společností Microsoft, společnosti třetích stran a iniciativy software otevřít zdroj. Například WordPress, Umbraco cm, Drupal, atd. Tyto webové aplikace jsou vytvořené v celé řadě Oblíbené rámců, například [PHP] v této WordPress příklad, [.NET], [Node.js], [Java]a [Python]pojmenování několik. Při vytváření webové aplikace z webu Azure Marketplace pouze software potřebný je prohlížeč, který používáte pro [Portál Azure].

V tomto kurzu se dozvíte postup:

* Vyhledání a jejich vytváření v prohlížeči v aplikaci služby Azure, který je založený na šabloně webu Azure Marketplace.
* Konfigurace nastavení služby Azure aplikace pro nové webové aplikace.
* Spuštění a spravovat svoji webovou aplikaci.

Pro účely tohoto kurzu budete nasazovat blogu WordPress z Azure Marketplace. Po dokončení kroků v tomto kurzu budete mít vlastní WordPress vytvoření a spuštění serveru v cloudu.

![Příklad řídicího panelu aplikace wep WordPress][WordPressDashboard1]

WordPress web, který budete nasadíte v tomto kurzu používá databáze MySQL. Pokud chcete místo toho použít databáze SQL databáze, najdete v článku [Nami projektu], která je také k dispozici prostřednictvím webu Azure Marketplace.

> [AZURE.NOTE]
> Pro dokončení tohoto kurzu, třeba účet Microsoft Azure. Pokud nemáte účet, můžete to udělat [Aktivujte své Visual Studio účastnická výhody] [ activate] nebo [zaregistrovat bezplatnou zkušební verzi][free trial].
>
> Pokud chcete začít pracovat s aplikaci služby Azure před registraci účet Azure, přejděte na [Zkuste aplikaci služby]. Odtud můžete okamžitě vytvořit web appu krátkodobý starter v aplikaci služby – bez platební kartou požaduje a nejsou žádné závazky.

## <a name="find-and-create-a-web-app-in-azure-app-service"></a>Vyhledání a vytvořte webovou aplikaci služby Azure aplikace

1. Přihlaste se k [portálu Azure].

1. Klikněte na **Nový**.
    
    ![Vytvoření nového prostředku Azure][MarketplaceStart]
    
1. Vyhledejte **WordPress**a potom klikněte na **WordPress**. (Pokud chcete místo MySQL použijte databáze SQL, vyhledejte **Projektu Nami**.)

    ![Vyhledejte WordPress v Marketplace][MarketplaceSearch]
    
1. Po přečtení popis WordPress aplikaci, klikněte na **vytvořit**.

    ![Vytvoření WordPress web appu][MarketplaceCreate]

## <a name="configure-azure-app-service-settings-for-your-new-web-app"></a>Konfigurace nastavení služby Azure aplikace pro nový Web App

1. Po vytvoření nové webové aplikace zásuvné nastavení WordPress se zobrazí, které se používají k proveďte následující kroky:

    ![Konfigurace nastavení WordPress web app][ConfigStart]

1. Zadejte název pro web app v poli **v prohlížeči** .

    Tento název musí být jedinečný v doméně azurewebsites.net, protože bude adresa URL webové aplikace *{název}*. azurewebsites.net. Není-li zadané jméno jedinečné, zobrazí se v textovém poli červený vykřičník.

    ![Konfigurace název WordPress webové aplikace][ConfigAppName]

1. Pokud máte víc předplatných, vyberte tu, kterou chcete použít. 

    ![Konfigurace předplatné pro web app][ConfigSubscription]

1. **Pole Skupina zdroje** vyberte nebo vytvořte nový účet.

    Další informace o skupinách prostředků, najdete v článku [Přehled Správce prostředků Azure][ResourceGroups].

    ![Konfigurace skupina zdroje pro web app][ConfigResourceGroup]

1. Vyberte **Aplikaci služby plán/umístění** nebo vytvořte nový účet.

    Další informace o různých plánech aplikaci služby, najdete v článku [Přehled plány aplikace služby Azure][AzureAppServicePlans]. 

    ![Konfigurace plán služeb pro web app][ConfigServicePlan]

1. Klikněte na **databáze**a v zásuvné **Nové databáze MySQL** zadejte požadované hodnoty pro konfiguraci databáze MySQL.

    na. Zadejte nový název nebo ponechte výchozí název.

    b. Nechte **Databáze typ** nastavený na **Sdíleno**.

    c. Zvolte stejného umístění, ze kterého jste se rozhodli pro web app.

    d. Zvolte cenových osy. **Rtuti** – tedy zdarma s minimálními připojení a místa na disku – je v pořádku pro účely tohoto návodu.

    e. V **Nové databáze MySQL** zásuvné přijměte podmínky pro právnické a klikněte na **OK**. 

    ![Konfigurace nastavení databáze pro web app][ConfigDatabase]

1. V zásuvné **WordPress** třeba souhlasit s podmínkami právnické a potom klikněte na **vytvořit**. 

    ![Dokončete nastavení web appu a klikněte na tlačítko OK][ConfigFinished]

    Web app a služby Azure aplikace obvykle vytvoří za méně než jednu minutu. Po kliknutí na ikony zvonku v horní části stránky portálu můžete sledovat průběh.

    ![Indikátor průběhu][ConfigProgress]

## <a name="launch-and-manage-your-wordpress-web-app"></a>Spuštění a spravovat svoji WordPress webovou aplikaci
    
1. Po vytvoření webové aplikace přejděte na portálu Azure ke skupině zdroje, ve kterém jste aplikaci vytvořili a uvidíte web app a databáze.

    Další zdroje s ikonou žárovka je [Aplikace přehledy][ApplicationInsights], která poskytuje sledování služby pro web app.

1. V zásuvné **pole Skupina zdroje** klikněte na řádku web app.

    ![Vyberte webovou aplikaci WordPress][WordPressSelect]

1. Ve webové aplikaci zásuvné klikněte na **Procházet**.

    ![Vyhledejte webovou aplikaci WordPress][WordPressBrowse]

1. Pokud se zobrazí výzva k výběru jazyka blogu WordPress, vyberte požadovaný jazyk a pak klikněte na **pokračovat**.

    ![Nakonfigurujte jazyk pro webovou aplikaci WordPress][WordPressLanguage]

1. Na stránce WordPress **úvodní** zadejte informace o konfiguraci vyžadované WordPress a klikněte na **Nainstalovat WordPress**.

    ![Konfigurace nastavení webové aplikace WordPress][WordPressConfigure]

1. Přihlaste se pomocí přihlašovacích údajů, kterou jste vytvořili na **úvodní** stránce.  

1. Stránky řídicího panelu webu otevřít a zobrazit informace, které jste zadali.    

    ![Zobrazení řídicího panelu WordPress][WordPressDashboard2]

## <a name="next-steps"></a>Další kroky

V tomto kurzu jste viděli, jak vytvářet a publikovat webovou aplikaci příklad z Azure Marketplace.

Další informace o práci s aplikací služby Web Apps najdete v tématech na levé straně stránky (windows) wide prohlížeče nebo v horní části stránky (windows úzký prohlížeč).

Další informace o vývoji WordPress web apps na Azure najdete v tématu [Vývoj WordPress v aplikaci služby Azure][WordPressOnAzure]. 

<!-- URL List -->

[PHP]: https://azure.microsoft.com/develop/php/
[.NET]: https://azure.microsoft.com/develop/net/
[Node.js]: https://azure.microsoft.com/develop/nodejs/
[Java]: https://azure.microsoft.com/develop/java/
[Python]: https://azure.microsoft.com/develop/python/
[activate]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[free trial]: https://azure.microsoft.com/pricing/free-trial/
[Vyzkoušejte aplikaci služby]: http://go.microsoft.com/fwlink/?LinkId=523751
[ResourceGroups]: ../resource-group-overview.md
[AzureAppServicePlans]: ../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md
[ApplicationInsights]: https://azure.microsoft.com/services/application-insights/
[Portál Azure]: https://portal.azure.com/
[Nami projektu]: http://projectnami.org/
[WordPressOnAzure]: ./develop-wordpress-on-app-service-web-apps.md

<!-- IMG List -->

[MarketplaceStart]: ./media/app-service-web-create-web-app-from-marketplace/marketplacestart.png
[MarketplaceSearch]: ./media/app-service-web-create-web-app-from-marketplace/marketplacesearch.png
[MarketplaceCreate]: ./media/app-service-web-create-web-app-from-marketplace/marketplacecreate.png
[ConfigStart]: ./media/app-service-web-create-web-app-from-marketplace/configstart.png
[ConfigAppName]: ./media/app-service-web-create-web-app-from-marketplace/configappname.png
[ConfigSubscription]: ./media/app-service-web-create-web-app-from-marketplace/configsubscription.png
[ConfigResourceGroup]: ./media/app-service-web-create-web-app-from-marketplace/configresourcegroup.png
[ConfigServicePlan]: ./media/app-service-web-create-web-app-from-marketplace/configserviceplan.png
[ConfigDatabase]: ./media/app-service-web-create-web-app-from-marketplace/configdatabase.png
[ConfigFinished]: ./media/app-service-web-create-web-app-from-marketplace/configfinished.png
[ConfigProgress]: ./media/app-service-web-create-web-app-from-marketplace/configprogress.png
[WordPressSelect]: ./media/app-service-web-create-web-app-from-marketplace/wpselect.png
[WordPressBrowse]: ./media/app-service-web-create-web-app-from-marketplace/wpbrowse.png
[WordPressLanguage]: ./media/app-service-web-create-web-app-from-marketplace/wplanguage.png
[WordPressDashboard1]: ./media/app-service-web-create-web-app-from-marketplace/wpdashboard1.png
[WordPressDashboard2]: ./media/app-service-web-create-web-app-from-marketplace/wpdashboard2.png
[WordPressConfigure]: ./media/app-service-web-create-web-app-from-marketplace/wpconfigure.png
