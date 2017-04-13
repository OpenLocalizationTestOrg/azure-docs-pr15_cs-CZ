<properties 
    pageTitle="Co je Azure RemoteApp? | Microsoft Azure" 
    description="Naučte se sdílet aplikace a prostředků na libovolném zařízení přes Azure RemoteApp." 
    services="remoteapp" 
    documentationCenter="" 
    authors="lizap" 
    manager="mbaldwin" 
    editor=""/>

<tags 
    ms.service="remoteapp" 
    ms.workload="compute" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="get-started-article" 
    ms.date="08/15/2016" 
    ms.author="elizapo"/>

# <a name="what-is-azure-remoteapp"></a>Co je Azure RemoteApp?

> [AZURE.IMPORTANT]
> Azure RemoteApp je právě ukončené. Přečtěte si [oznámení](https://go.microsoft.com/fwlink/?linkid=821148) podrobnosti.

Azure RemoteApp přináší funkce programu Microsoft RemoteApp místní zálohy tak, že Vzdálená plocha na Azure. Azure RemoteApp vám pomůže zjistit zabezpečené, vzdálený přístup k aplikacím z mnoha různých zařízeních. Azure RemoteApp v podstatě hostuje dočasnou relací terminálu serveru v cloudu a přístup k jejich použití a sdílet s jinými uživateli.

Azure RemoteApp můžete sdílet aplikace a zdroje s uživateli na téměř libovolném zařízení. Jsme hostovat aplikace v cloudu, což znamená, že jsme starat o hardware a stejné měřítko požadavkům uživatele. Je potřeba udělat stačí nahrát aplikace, které chcete sdílet a pak získat uživatelé můžou používat těmito aplikacemi. [Uživatelé dostanou zachovat svoje zařízení](remoteapp-clients.md), zatímco všechno Azure portálu Správa. Můžete dokonce mít možnost pomocí svých přihlašovacích údajů podnikové umožňuje zajistit zabezpečení aplikace a data.

Přečtěte si další informace o Azure RemoteApp, nebo pokud se vám můžeme můžete [vyzkoušet, teď](https://azure.microsoft.com/services/remoteapp/)už přesvědčeny.

Máte další otázky o Azure RemoteApp? Podívejte se na našeho [Časté otázky](remoteapp-faq.md).

Azure RemoteApp je součástí [Virtuální plochy infrastruktury společnosti Microsoft](http://www.microsoft.com/server-cloud/products/virtual-desktop-infrastructure/explore.aspx).

**Nové!** Chcete se dozvědět víc o Azure RemoteApp? Nebo připravená k ověření Azure RemoteApp ve velkém měřítku? Připojit se ke naše týdně [požádejte odborníci webinář](https://azureinfo.microsoft.com/AzureRemoteAppAskTheExperts-Registration-Page.html?ls=Website).

## <a name="azure-remoteapp-collections"></a>Azure kolekce RemoteApp
Existují dva typy kolekcí [Azure RemoteApp](remoteapp-collections.md):


- **Shluk kolekce** je hostovaný ve a ukládá data pro aplikace v cloudu. Uživatelé můžou získat přístup ke aplikace přihlašování pomocí jejich účet Microsoft nebo firemní pověření synchronizované nebo federovaní Azure Active Directory.

    Kolekce cloudu až zvolte aplikaci, kterou chcete sdílet nevyžaduje připojení pro všechny zdroje soukromé síti vaší společnosti (například přes VPN zařízení). Pokud aplikace využívá zdroje na Internetu, Onedrivu nebo Azure, bude pro vás vhodná kolekce cloudu. Je také nejrychleji vytvořit.

- **Hybridní kolekce** je hostovaný ve a uchovává data v Azure cloudu, ale taky umožňuje uživatelům přístup k datům a zdroje informací uložených v místní síti. Uživatelé můžou získat přístup ke aplikace přihlašování pomocí své firemní přihlašovací údaje synchronizované nebo federovaní Azure Active Directory.

    Zvolte kolekci hybridní pokud vyžadují připojení k zdroje na soukromé síti vaší společnosti. Například, pokud aplikace potřebuje přístup do jedné z následujících akcí:

    - Servery soubor umístěný v síti intranet
    - Quicken
    - Databáze za bránou firewall

    Toto je obecně zvýšíte jeho přínos pro velké podniky s velkým množstvím zdroje na svých soukromých sítích, které nejdou přesunout do cloudu.

Různé kolekce mají různé možnosti, včetně sítích, takže obrázek, [který kolekce](remoteapp-collections.md) nejvhodnější. 


### <a name="updating-your-collection"></a>Aktualizace kolekci
Jednou z důležité rozdíly mezi kolekcemi hybridní a cloudu je, jak se zachází s aktualizací softwaru. S kolekcí cloudu, který používá předinstalovaných obrázek Office 365 ProPlus nebo Office 2013 není nutné starat o nějaké aktualizace. Služba udržuje sebe sama a vrátí aktualizací průběžně, aplikace a operační systém.

Pro hybridní kolekce i kolekce cloudu, které můžete použít obrázek vlastní šablonu jste zodpovědní za zachování obrázek a aplikace. Pro doméně obrázky můžete určit aktualizace pomocí nástrojů, jako je Windows Update, zásad skupiny nebo System Center.

Po aktualizaci obrázek vlastní šablonu nahrát nový obrázek Azure cloudu a změňte kolekci použít nový obrázek. (Lze to provést z Azure RemoteApp **Úvodní** stránku nebo na řídicím panelu.)

Další informace najdete v tématu [aktualizace kolekci](remoteapp-update.md) .

## <a name="supported-remoteapp-clients"></a>Podporovaní klienti RemoteApp
Azure RemoteApp je podporován v RemoteApp klientské aplikace pro Windows a Windows RT, jakož i aplikace Microsoft připojení ke vzdálené ploše pro Mac, iOS a Android. Uživatele můžete použít tyto aplikace na jejich mobilní telefon nebo výpočet zařízení přístup k novým programům Azure RemoteApp.

Další informace o klientech v tématu [otevření aplikace v Azure RemoteApp](remoteapp-clients.md) .

## <a name="next-steps"></a>Další kroky
Přejděte! Vyzkoušejte si to! Tyto články vám pomůžou začít s Azure RemoteApp:

- [Jaký druh kolekce je potřeba Azure RemoteApp?](remoteapp-collections.md)
- [Vytvořit obrázek Azure RemoteApp](remoteapp-imageoptions.md)
- [Jak vytvořit sadu cloudu Azure RemoteApp](remoteapp-create-cloud-deployment.md)
- [Jak vytvořit sadu hybridní Azure RemoteApp](remoteapp-create-hybrid-deployment.md)
- [Jak licencování funguje v Azure RemoteApp?](remoteapp-licensing.md)
- [Osvědčené postupy pro používání Azure RemoteApp](remoteapp-bestpractices.md)
- [Nejčastější dotazy týkající se Azure RemoteApp](remoteapp-faq.md)
 

### <a name="help-us-help-you"></a>Pomozte nám vám pomůžou 
Věděli jste, že kromě hodnocení v tomto článku a vyjádřit dolů pod, můžete provádět změny samotný článek? Něco chybí? Něco špatně? Můžu zapsat něco, co je právě matoucí? Posunutí nahoru a klikněte na **Upravit na GitHub** nebo **Upravit** ke změnám – můžou být chodily do nám k revizi a potom po jsme odhlášení k nim, uvidíte změny a vylepšení tady.