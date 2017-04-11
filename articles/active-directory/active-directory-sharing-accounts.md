<properties
    pageTitle="Sdílení účtů v Azure AD |  Microsoft Azure"
    description="Popisuje, jak Azure Active Directory umožňuje organizacím účty pro místní aplikace a služby cloudu zákaznické bezpečné sdílení."
    services="active-directory"
    documentationCenter=""
    authors="msStevenPo"
    manager="femila"
    editor=""/>

 <tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="02/09/2016"  
    ms.author="stevenpo"/>

# <a name="sharing-accounts-with-azure-ad"></a>Účty nasdílet Azure AD

## <a name="overview"></a>Základní informace
Někdy organizacemi muset používat jediné uživatelské jméno a heslo pro víc lidem. Obvykle se stane v obou případech:

- Při práci s aplikací, které vyžadují jedinečné přihlašovací jméno a heslo pro každého uživatele, zda místní aplikací ani spotř cloudových služeb (například podnikové sociální média účty).
- Při vytváření prostředí více uživatelů. Pravděpodobně jednoho místní účet zvýšenými oprávněními a mohou sloužit k základní činnosti instalační program, Správa a obnovení (například místní "globální správce" účet pro Office 365) nebo kořenové v služby Salesforce.

Obvykle tyto účty by nesou distribuci přihlašovacích údajů (uživatelské jméno a heslo) doprava osob nebo je uložíte do sdíleného umístění, kde několika důvěryhodným agentů se k nim.

Tradiční sdílení model obsahuje několik nevýhody:

- Povolení přístupu k nové aplikace vyžaduje, abyste distribuce přihlašovací údaje pro všechny uživatele, které potřebuje přístup.
- Každá sdílené aplikace může vyžadovat vlastním jedinečnou sadu sdílené v části přihlašovací údaje uživatelích zapamatovatelné více sad přihlašovacích údajů. Pokud mají uživatelé zapamatovatelné mnoho přihlašovací údaje, riziko kombinace kláves rozšíří bude použít riziková postupy. (například zapíšete hesla).
- Neumí určit, kdo má přístup k aplikaci.
- Neumí určit, kdo má *přístup* aplikace.
- Pokud potřebujete odebrat přístup k aplikaci, musíte aktualizovat pověření a znovu distribuovat pro všechny uživatele, který potřebuje přístup k této aplikaci.

## <a name="azure-active-directory-account-sharing"></a>Sdílení Azure Active Directory účtu

Azure AD nabízí nový přístup k používání sdílené účty, které odstraňuje tyto nevýhody.

Správce Azure AD nakonfiguruje aplikace, které uživatel může dostat pomocí panelu přístup a zvolíte typ nejlepší přihlašování vhodný pro tuto aplikaci. Jeden z těchto typů *na základě heslo jednotného přihlašování na*umožňuje Azure AD představovat druh "zprostředkovatele" během přihlašování této aplikace.

Uživatelé přihlásit jednou s účtem organizace. Toto je stejný účet, který jim pravidelně anonymní přístup k jejich plochu nebo e-mailu. Můžete prozkoumat a přístup jenom aplikace, které jsou přiřazené. Tento seznam aplikací s sdílených účty, může obsahovat libovolný počet sdílených přihlašovací údaje. Mějte na paměti nebo si poznamenejte různých účty, které je možné, že používáte nemusí koncového uživatele.

Sdílené účty nejen zvětšit dohled a vylepšení použitelnosti, také zvýšit zabezpečení. Uživatelé s oprávněními, abyste mohli pověření nezobrazuje sdílené heslo, ale nechcete dostat oprávněními, abyste mohli heslo jako součást toku orchestrated ověřování. Dále v některých aplikacích jednotné přihlašování heslo, máte možnost mít Azure AD pravidelně přechodu (Aktualizovat) heslo složitých heslem zvýšení zabezpečení účtu. Správce může snadno udělit nebo odvolat přístup k aplikaci a je dobré vědět, kdo má přístup k tomuto účtu a kdo ji v minulosti.

Azure AD podporuje sdílené účty pro všechny Enterprise mobilita sady (EMS), Premium nebo základní licencované uživatele všech typů heslo jednoho přihlášení aplikací. Můžete sdílet účty z některého z tisíců předem integrované aplikace v galerii aplikace a můžete přidat vlastní heslo ověřování aplikaci [vlastních aplikací jednotné přihlašování](active-directory-sso-integrate-saas-apps.md).

Azure AD k funkcím podporující nástroj sdílení účtu, patří:

- [Heslo jednotné přihlašování](active-directory-appssoaccess-whatis.md#password-based-single-sign-on)
- Heslo jednoho přihlašování agent
- [Přiřazení skupiny](active-directory-accessmanagement-self-service-group-management.md)
- Vlastní heslo aplikace
- [Sestavy řídicího panelu použití aplikace](active-directory-passwords-get-insights.md)
- Koncový uživatel přístup portály
- [Proxy aplikací](active-directory-application-proxy-get-started.md)
- [Tržiště služby Active Directory](https://azure.microsoft.com/marketplace/active-directory/all/)

## <a name="sharing-an-account"></a>Sdílení účet
Používat Azure AD ke sdílení účet, který byste potřebovali:

- Přidání aplikace [Galerie aplikací](https://azure.microsoft.com/marketplace/active-directory/) nebo [vlastní aplikace](http://blogs.technet.com/b/ad/archive/2015/06/17/bring-your-own-app-with-azure-ad-self-service-saml-configuration-gt-now-in-preview.aspx)
- Konfigurace aplikace pro heslo jednoho přihlašování (SSO)
- Pomocí [přiřazení na základě skupiny](active-directory-accessmanagement-group-saasapps.md) a vyberte možnost zadání sdílené přihlašovacích údajů
- Volitelné: v některých aplikacích, jako je Facebooku, Twitteru nebo LinkedIn, můžete povolit možnost pro [Automatické hesla Azure AD převrácení](http://blogs.technet.com/b/ad/archive/2015/02/20/azure-ad-automated-password-roll-over-for-facebook-twitter-and-linkedin-now-in-preview.aspx)

Můžete také vytvořit účet sdílené bezpečnější s Multi-Factor Authentication (MFA) (Další informace o [zabezpečení aplikace s Azure AD](../multi-factor-authentication/multi-factor-authentication-get-started.md)) a můžete delegovat možnost spravovat, kdo má přístup k aplikaci [Azure AD Samoobslužná](active-directory-accessmanagement-self-service-group-management.md) Správa skupiny.

## <a name="related-articles"></a>Související články

- [Index článek Správa aplikací služby Azure Active Directory](active-directory-apps-index.md)
- [Ochrana aplikace s podmíněným přístupem](active-directory-conditional-access.md)
- [Samoobslužné skupiny správy/SSAA](active-directory-accessmanagement-self-service-group-management.md)
