<properties
    pageTitle="Upgrade z DirSync a Azure AD Sync | Microsoft Azure"
    description="Popisuje, jak upgradovat z DirSync a Azure AD Sync Azure AD Connect."
    services="active-directory"
    documentationCenter=""
    authors="andkjell"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="06/27/2016"
    ms.author="billmath"/>


# <a name="upgrade-windows-azure-active-directory-sync-dirsync-and-azure-active-directory-sync-azure-ad-sync"></a>Upgrade Windows Azure Active Directory Sync ("DirSync") a synchronizaci služby Azure Active Directory ("Azure AD Sync")
Azure AD Connect je nejlepší způsob, jak připojit místního adresáře s Azure AD a Office 365. Toto je skvělý čas upgradovat na Azure AD Connect z Windows Azure Active Directory Sync (DirSync) nebo Azure AD Sync, a mezi ně jsou teď zastaralé dosáhne konec support na 13 duben 2017.

Nástroje dvě identity synchronizace, které jsou změněny byly nabízených pro zákazníky jedné stromové struktury (DirSync) a pro více struktury a jiné pokročilé zákazníci (Azure AD Sync). Tyto starší nástroje nahradily jednoho řešení, která je dostupná pro všechny scénáře: Azure AD Connect. Nabízí nové funkce, funkci vylepšení a podpora pro nové scénáře. Abyste mohli pokračovat v synchronizaci místních identit dat Azure AD a Office 365, doporučujeme upgradovat na Azure AD Connect.

Poslední verzi DirSync byla vydána v července 2014 a poslední verzi Azure AD Sync byla vydána v květen 2015.

## <a name="what-is-azure-ad-connect"></a>Co je Azure AD Connect
Azure AD Connect je následník DirSync a Azure AD Sync. Kombinuje všech případech tyto dvě podporované. Další informace v [Integrace místních identit s Azure Active Directory](active-directory-aadconnect.md).

## <a name="deprecation-schedule"></a>Odstranění plánu

Datum | Komentář
 --- | ---
13 duben 2016 | Windows Azure Active Directory Sync ("DirSync") a Microsoft Azure Active Directory Sync ("Azure AD Sync") se ohlásí, jak změněny.
13 duben 2017 | Podpora konce. Platnost zákazníci budou moct otevřít případ podpory bez nutnosti aktualizace Azure AD Connect nejdřív.

## <a name="how-to-transition-to-azure-ad-connect"></a>Jak přechod na Azure AD Connect
Pokud používáte DirSync můžete upgradovat dvěma způsoby: místní upgrade nebo paralelní nasazení. Upgrade na místě je vhodné pro většinu zákazníků a máte poslední operačním systému a menší než 50 000 objektů. V ostatních případech je vhodné dělat paralelní nasazení, kde se konfiguraci DirSync přesune do nového serveru se službou Azure AD Connect.

Pokud používáte Azure AD Sync, doporučuje se upgradovat na místě. Pokud budete chtít, bude možné nainstalovat nový server Azure AD Connect paralelně a proveďte migraci dráha ze serveru Azure AD Sync do Azure AD Connect.

Řešení | Scénář
----- | -----
[Upgrade z DirSync](./connect/active-directory-aadconnect-dirsync-upgrade-get-started.md) | <li>Pokud máte existující server DirSync spuštěná.</li>
[Upgrade z Azure AD Sync](active-directory-aadconnect-upgrade-previous-version.md)| <li>Pokud přesouváte z Azure AD Sync.</li>

Pokud budete chtít najdete v článku Jak provést místní upgrade z DirSync Azure AD Connect, najdete v tomto videu 9 kanálu:

> [AZURE.VIDEO azure-active-directory-connect-in-place-upgrade-from-legacy-tools]

## <a name="faq"></a>NEJČASTĚJŠÍ DOTAZY
**Otázka: můžu přijali e-mailové oznámení z Azure týmu a/nebo zprávy z Centra zpráv Office 365, ale používám připojit.**  
Oznámení odeslal taky zákazníkům, kteří používají Azure AD Connect s číslem sestavení 1.0. \*.0 (pomocí verze pre 1.1). Microsoft doporučuje zákazníků, kteří mají mít nejnovější vydání Azure AD Connect. S 1.1, kterou funkci [automatické inovace](active-directory-aadconnect-feature-automatic-upgrade.md) znamená, že snadné vždy mít nejnovější verzi Azure AD Connect nainstalovaný.

**Otázka: bude DirSync/Azure AD Sync přestanou na 13 duben 2017?**  
Ne. Datum kdy tyto už bude moct komunikovat s Azure AD se ohlásí k pozdějšímu datu. Bude moct najít informace v tomto tématu, pokud jsou dostupné.

**Otázka: které verze DirSync můžete upgradovat z?**  
Tuto možnost podporuje upgradovat z verze všechny DirSync aktuálně používaných.

**Otázka: co dávat konektoru Azure AD pro FIM/MIM?**  
Konektor Azure AD FIM/MIM **není** po se ohlásí, jak změněny. Je na **Ukotvit funkce**; přidali novou funkci volání a přijímání žádné opravy chyb. Microsoft doporučuje zákazníkům, kteří používají plánovat přejdete z něho k Azure AD Connect. Důrazně doporučujeme nespouštět všechny nové nasazení s ním pracovat. Tato spojnice budou oznámeny změněny v budoucnu.

## <a name="additional-resources"></a>Další zdroje informací

* [Integrace místních identit s Azure Active Directory](active-directory-aadconnect.md)
