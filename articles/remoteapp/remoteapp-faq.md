<properties 
    pageTitle="Azure RemoteApp nejčastější dotazy týkající se | Microsoft Azure" 
    description="Přečtěte si odpovědi na časté otázky k Azure RemoteApp." 
    services="remoteapp" 
    documentationCenter="" 
    authors="lizap" 
    manager="swadhwa" 
    editor=""/>

<tags 
    ms.service="remoteapp" 
    ms.workload="compute" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="get-started-article" 
    ms.date="08/15/2016" 
    ms.author="elizapo"/>

# <a name="azure-remoteapp-faq"></a>Nejčastější dotazy týkající se Azure RemoteApp

> [AZURE.IMPORTANT]
> Azure RemoteApp je právě ukončené. Přečtěte si [oznámení](https://go.microsoft.com/fwlink/?linkid=821148) podrobnosti.

Často na následující otázky o Azure RemoteApp. Aby mohli ostatní uživatelé? Navštivte [fóra pro RemoteApp](https://social.msdn.microsoft.com/Forums/azure/home?forum=AzureRemoteApp) a dejte nám vědět, co je potřeba vědět nebo rozevírací poznámku pod.

## <a name="cant-find-what-youre-looking-for-have-a-question-we-didnt-answer"></a>Nemůžete najít, co hledáte? Máte dotaz, který jsme neměli odpovědí?
Pokud nemůžete najít informace budete potřebovat, nebo můžete mít další otázku, kterou jsme nejsou týkající se tady, přejděte na [Fórum komunity Azure RemoteApp](http://aka.ms/araforum) a zeptejte se tam. Vždy jsme můžete přidat další odpovědi tady.

## <a name="what-is-azure-remoteapp"></a>Co je Azure RemoteApp? ##


- **Co je Azure RemoteApp?** RemoteApp je že služby Azure vám pomůže zjistit zabezpečené, vzdálený přístup k aplikacím z mnoha různých zařízeních. Další informace o [Azure RemoteApp](remoteapp-whatis.md).
- **Jaké jsou možnosti nasazení?** Existují dva typy kolekcí RemoteApp: cloudu a hybridní. Který z nich budete potřebovat, závisí na mnoha různých faktorů, například jestli budete potřebovat připojení k doméně. Budeme všech těchto rozhodnutí [tady](remoteapp-collections.md).

## <a name="quick-tips-on-using-azure-remoteapp"></a>Tipy k použití Azure RemoteApp ##
- **Jak dlouho, dokud se mi to od ní odpojilo? Jak dlouho mám lze nedělá před prezentováním mě spouštěcí?** 4 hodiny. Pokud vy nebo jednoho nebo více uživatelů nečinnosti 4 hodiny, se automaticky přihlásíte mimo Azure RemoteApp. Podívejte se na výchozí nastavení v [Azure předplatné omezení služby, kvót a omezení](../azure-subscription-service-limits.md).
- **Můžu si vyzkoušet tuto službu zdarma?** Ano. 30 dní je k dispozici bezplatnou zkušební verzi. Po uplynutí zkušebního období, můžou přechodu na placené účtu (který můžete používat ve výrobním) nebo ukončit používání služby. Začněte tím, že přejdete na [portal.azure.com](http://portal.azure.com) bezplatnou zkušební verzi – vytvoření nové instance RemoteApp. Pomocí bezplatné zkušební verze je možné vytvářet 2 výskyty RemoteApp s 10 uživatelů za instance. Myslete na to, že tato zkušební verze pouze je 30 dní.
## <a name="azure-remoteapp-subscription-details"></a>Podrobnosti předplatného Azure RemoteApp ##

- **Jaká jsou omezení služeb?** Se dozvíte o výchozí nastavení a limitech služby Azure RemoteApp v [Azure předplatné omezení služby, kvót a omezení](../azure-subscription-service-limits.md). Dejte nám vědět, pokud máte další otázky.
- **Počet uživatelů, kteří mají mít?** Je minimálně 20 uživatelů. Manuální opakovat, že je velmi vymazat - minimální hodnota je 20. Můžete se vám nebudou účtovat poplatky pro 20. 
- **Kolik stojí RemoteApp?** Podívejte se na [Azure RemoteApp ceny podrobnosti ](https://azure.microsoft.com/pricing/details/remoteapp/).
- **Jeden typ kolekce stojí více než jiný?** Ano, může, v závislosti na vašim požadavkům kolekce. Kolekce hybridní vyžaduje připojení z Azure RemoteApp k místní síti. Pokud používáte existujícího VNET/Express postupu, je žádné další náklady. Ale pokud používáte nové VNET Azure a brány nebo Express směrování, můžete strhne příslušná [Brána VPN](https://azure.microsoft.com/pricing/details/vpn-gateway) nebo [Express postupu](https://azure.microsoft.com/pricing/details/expressroute/). Náklady (podrobně odkazy) je nad měsíční RemoteApp Azure nákladů.

## <a name="collections---whats-supported-which-should-you-use-and-others"></a>Kolekce – co bude podporované, které byste měli použít a další
- **Vlastní řádek obchodní (LOB) aplikace podporuje?** Ano. Použít vlastní aplikaci v Azure RemoteApp, vytvořte [vlastní šablony](remoteapp-create-custom-image.md)a pak ho nahrajte do kolekci RemoteApp.
- **Budou fungovat aplikace vlastní LOB v Azure RemoteApp?** Nejlepší způsob, jak obrázek, který se má ji otestujte. Podívejte se na [Centrum pro kompatibilitu RD](http://www.rdcompatibility.com/compatibility/default.aspx).
- **Kterou metodu nasazení (cloudu nebo hybridní) je nejlepší pro naši organizaci?** Hybridní kolekce zajistíte kompletní možnosti, pokud chcete úplné integrace se službou jednotné přihlašování (SSO) a zabezpečené místního síťové připojení. Shluk kolekce poskytují aktivní a snadný způsob vyčlenění nasazení pomocí několika metody ověřování. Přečtěte si další informace o [možnostech](remoteapp-whatis.md).
- **Máme SQL nebo jiné databáze buď místně nebo v Azure. Jaký typ nasazení máme použít?** To záleží na místo, kam je SQL nebo back-end databáze. Pokud je databáze privátní sítě, použijte kolekci hybridní. Pokud databáze je vystaven na Internetu a umožňuje klienta připojení se k ní připojit, můžete kolekci cloudu.
- **A co mapování jednotka USB a sériové portu, sdílení schránky a přesměrování tiskárny?** Tyto funkce jsou podporovány v Azure RemoteApp. Ve výchozím nastavení je povolena schránky sdílení a přesměrování tiskárny. Další informace o přesměrování [tady](remoteapp-redirection.md). 


## <a name="template-images"></a>Obrázky šablon
- **Můžu použít ke cloudové nebo existující virtuálního počítače jako šablonu pro osobní kolekce RemoteApp?** Ano! Můžete vytvořit obrázek založené na Azure OM, použijte jeden z obrázků součástí vašeho předplatného nebo vytvoření vlastního obrázku. Podívejte se na [Možnosti RemoteApp obrázku](remoteapp-imageoptions.md).


## <a name="network-options"></a>Možnosti sítě
- **Hybridní kolekce vyžaduje VNET. Máme použít naše existující VNET?** Je možné, pokud stávající VNET Azure VNET. V tématu "krok 1: nastavení sítě virtuální" v [hybridním kolekce pokyny](remoteapp-create-hybrid-deployment.md) pro další informace.
- **Můžete použít VNET s kolekcí cloudu?** Nakonec můžete. Podívejte se na [vytvořit kolekci cloudu](remoteapp-create-cloud-deployment.md), zejména kroku 1, pokud hledáte informace.

## <a name="authentication-options"></a>Možnosti ověřování



- **Takhle ověřování? Které metody podporuje?** V cloudu kolekci podporuje účtech Microsoft a Azure Active Directory účty, které jsou také účty Office 365. Hybridní kolekce podporuje pouze Azure Active Directory účty, které byly synchronizované (pomocí nástroje, například [Azure Active Directory Sync](http://blogs.technet.com/b/ad/archive/2014/09/16/azure-active-directory-sync-is-now-ga.aspx)) ze služby Active Directory pro Windows Server nasazení; Konkrétně nebo synchronizovány s parametrem synchronizace hesel synchronizují nakonfigurovali federace Active Directory Federation Services (AD FS). Můžete taky nakonfigurovat [Multi-Factor Authentication (MFA)](https://azure.microsoft.com/services/multi-factor-authentication/).

>[AZURE.NOTE]Uživatelé služby Azure Active Directory musí být z klienta, který máte přidružený k předplatnému. (Můžete zobrazit a upravovat vaše předplatné na kartě **Nastavení** na portálu. [Změna klienta služby Azure Active Directory používaný RemoteApp](remoteapp-changetenant.md) Další informace najdete.)

- **Proč nelze můžu podělit o svůj přístupu k účtu služby Azure Active Directory?** Uživatelé služby Azure Active Directory musí být v adresáři, který máte přidružený k předplatnému. Můžete zobrazit nebo upravit tento adresář na kartě nastavení na portálu. Další informace najdete v článku [Změna klienta služby Azure Active Directory RemoteApp používá](remoteapp-changetenant.md) .

## <a name="clients---what-device-can-i-use-to-access-azure-remoteapp"></a>Klienti – můžete jaké zařízení můžu používat pro přístup k Azure RemoteApp?
Můžete najít informace o dobré klienta, včetně kroků instalace různých klientů při [otevření aplikace v Azure RemoteApp](remoteapp-clients.md).

- **Jaké zařízení a operační systémy nepodporují klientské aplikace?**
První počítačů a tabletů: 
    - Windows 10 (verze preview klienta)
    - Windows 8.1 a Windows 8
    - Windows 7 Service Pack 1
    - Mac OS X
    - Windows RT
    - Tablety s androidem
    - Ipady a telefony:
    - iPhone
    - Telefon s androidem
    - Windows Phone
 
    [Stáhněte si](https://www.remoteapp.windowsazure.com/ClientDownload/AllClients.aspx) klient RemoteApp.
- **Podporuje Azure RemoteApp tenké klientů?** Ano, jsou podporovány následující Windows vložený tenké klienti:
    - Windows vložené standardní 7
    - Windows 8 standardní vložené
    - Vložené Windows 8.1 odvětví Pro
    - Windows 10 IoT Enterprise

- **Jakou verzi systému Windows Server je podporována pro vzdálené plochy relace hostitele (RDSH)?** Windows serveru 2012 R2.

## <a name="support-and-feedback"></a>Podpora a váš názor


- **Jaký je plán podpory pro RemoteApp?** Podpora správy fakturace a předplatného je k dispozici zdarma. Technická podpora je k dispozici prostřednictvím [služby Azure plány](https://azure.microsoft.com/support/plans/). Můžete taky načtete bezplatná podpora naše [Azure diskusní fóra](http://social.msdn.microsoft.com/Forums/windowsazure/home?forum=AzureRemoteApp). 
- **Jak poslat svůj názor?** Navštivte [Fórum komunity svůj názor](https://feedback.azure.com/forums/247748-azure-remoteapp/).
- **Kdo může mluvit Další informace o Azure RemoteApp?** Kromě naše [diskusní fóra](http://social.msdn.microsoft.com/Forums/windowsazure/home?forum=AzureRemoteApp), což je ideální místo k zveřejňujte otázky, se můžete připojit týdenní [Ask webinář odborníky](https://azureinfo.microsoft.com/US-Azure-WBNR-FY15-11Nov-AzureRemoteAppAskTheExperts-Registration-Page.html), které budeme všechno, co RemoteApp.
- **Co si přečtěte následující dokumentaci RemoteApp?** Mrzí rád tak, že se dotaz. Kromě obsah nápovědy v portálu nápovědy okraj (stačí kliknout **?** na kterékoli stránce v portálu), jsou k dispozici pro vás naučí v následujících článcích vše o RemoteApp:
    - **Začínáme:**
        - [Co je RemoteApp?](remoteapp-whatis.md)
        - [Co je obrázky šablon RemoteApp?](remoteapp-images.md)
        - [Jak funguje správa licencí](remoteapp-licensing.md)
        - [Jak spolupráce RemoteApp a Office?](remoteapp-o365.md)
        - [Jak přesměrování funguje v RemoteApp](remoteapp-redirection.md)?
    - **Nasazení:**
        - [Vytvoření vlastní šablony](remoteapp-create-custom-image.md)
        - [Vytvoření kolekce hybridní](remoteapp-create-hybrid-deployment.md)
        - [Vytvoření kolekce cloudu](remoteapp-create-cloud-deployment.md)
        - [Konfigurace služby Azure Active Directory RemoteApp](remoteapp-ad.md)
        - [Publikování aplikace v RemoteApp](remoteapp-publish.md)
    - **Spravovat:**
        - [Přidání uživatelů](remoteapp-user.md)
        - [Doporučené postupy pro konfiguraci a práce s nimi RemoteApp](remoteapp-bestpractices.md)  

    Videa! Máme také počet videa o RemoteApp. Některé poskytují úvod ([Úvod k Azure RemoteApp](https://azure.microsoft.com/documentation/videos/cloud-cover-ep-150-azure-remote-app-with-thomas-willingham-and-nihar-namjoshi/)), zatímco ostatní vás provede jednotlivými nasazení ([nasazení cloudu](https://www.youtube.com/watch?v=3NAv2iwZtGc&feature=youtu.be) a [hybridního nasazení](https://www.youtube.com/watch?v=GCIMxPUvg0c&feature=youtu.be)). Podívejte se na ně!

 
### <a name="help-us-help-you"></a>Pomozte nám vám pomůžou 
Věděli jste, že kromě hodnocení v tomto článku a vyjádřit dolů pod, můžete provádět změny samotný článek? Něco chybí? Něco špatně? Můžu zapsat něco, co je právě matoucí? Posunutí nahoru a klikněte na **příkaz Upravit v GitHub** ke změnám – můžou být chodily do nám k revizi a potom po jsme odhlášení k nim, uvidíte změny a vylepšení tady.
