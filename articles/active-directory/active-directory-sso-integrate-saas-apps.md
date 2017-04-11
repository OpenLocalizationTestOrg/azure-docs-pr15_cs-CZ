<properties
    pageTitle="Integrace služby Azure Active Directory jednotné přihlašování s aplikacemi jiných SaaS |  Microsoft Azure"
    description="Povolte ověřování přihlašování a uživatel zřizování Správa centralizované přístupu SaaS aplikací v Azure Active Directory. Základní informace o tom, jak integrovat Azure Active Directory do SaaS aplikací."
    services="active-directory"
      keywords="integrace Azure AD s aplikacemi jiných SaaS"
    documentationCenter=""
    authors="curtand"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="identity"
    ms.date="09/30/2016"
    ms.author="curtand"/>

# <a name="integrate-azure-active-directory-single-sign-on-with-saas-apps"></a>Integrace služby Azure Active Directory jednotné přihlašování s aplikacemi jiných SaaS  

> [AZURE.SELECTOR]
- [Azure portálu](active-directory-enterprise-apps-manage-sso.md)
- [Azure klasické portál](active-directory-sso-integrate-saas-apps.md)

[AZURE.INCLUDE [active-directory-sso-use-case-intro](../../includes/active-directory-sso-use-case-intro.md)]

Začněte tím, nastavení jednotného přihlašování aplikace, která jste jejich přenesením do vaší organizace, budete používat existující adresář v Azure Active Directory (Azure AD). Můžete použít Azure AD adresář, který můžete získat prostřednictvím Microsoft Azure, Office 365 nebo Windows Intune. Pokud máte dvě nebo víc z těchto článků, najdete v článku [Správa adresáři Azure AD](active-directory-administer.md) pro určení, která chcete použít.

## <a name="authentication"></a>Ověřování

Aplikace, které podporují SAML 2.0 WS-Federation nebo OpenID připojení protokoly přihlášení certifikáty stanovit vztahy důvěryhodnosti používá Azure Active Directory. Další informace naleznete v tématu [Správa certifikátů pro federované jednotné přihlašování](active-directory-sso-certs.md).

U aplikací, které podporují pouze HTML založené na formulářích přihlašovací Azure Active Directory používá "heslo překlenutí vyrovnávací paměti" stanovit vztahy důvěryhodnosti. To umožňuje uživatelům ve vaší organizaci se automaticky přihlásit k aplikaci SaaS tak, že Azure AD pomocí informací o uživatelském účtu z aplikace SaaS. Azure AD shromažďuje a bezpečně ukládá informací o uživatelském účtu a související heslo. Další informace najdete v tématu [na základě heslo jednotného přihlašování](active-directory-appssoaccess-whatis.md#password-based-single-sign-on).

## <a name="authorization"></a>Povolení

Zřizování účet umožňuje uživatelům oprávnění používat aplikaci po jejich ověření prostřednictvím jednotného přihlašování. Zřizování uživatelů se teď dá ručně nebo v některých případech můžete přidat nebo odebrat uživatele z aplikace SaaS na základě změn provedených v Azure Active Directory. Další informace o použití existující spojnic Azure AD pro automatické vytváření najdete v článku [Automatické uživatele zřizování a zrušte zřizování SaaS aplikací](active-directory-saas-app-provisioning.md).

Jinak můžete ručně přidat informace o uživateli aplikace, nebo použít jiné zřizovací řešení, které jsou dostupné v tržiště.

## <a name="access"></a>Aplikace Access

Azure AD několika způsoby přizpůsobitelné nasazení aplikace koncovým uživatelům ve vaší organizaci. Nejsou uzamčené do určitého nasazení nebo řešení přístupu. Můžete použít [řešení, které nejlépe odpovídá vašim potřebám](active-directory-appssoaccess-whatis.md#deploying-azure-ad-integrated-applications-to-users).

## <a name="additional-considerations-for-applications-already-in-use"></a>Další informace pro aplikace už používá

Nastavení jednotného přihlášení na pro aplikace, která už vaše organizace používá jiný proces se od proces vytváření nových účtů pro nové aplikace. Existuje několik Příprava včetně: mapování identity uživatele v aplikaci Azure AD identit a vysvětlení, jak uživatelé budou mít přihlášení do aplikace po integraci.

> [AZURE.NOTE] Nastavíte jednotné přihlašování pro existující aplikaci, musíte mít oprávnění globálního správce v obou Azure AD a SaaS aplikace.

### <a name="mapping-user-accounts"></a>Mapování uživatelských účtů

Identita uživatele má obvykle jedinečného identifikátoru, který může být-mailovou adresu nebo hlavní jméno uživatele (UPN). Budete muset odkaz (mapa) identitu všech uživatelů k jejich Azure AD identity. Existuje několik způsobů, jak provést podle toho, jak požadavky ověřování aplikace.

Další informace o mapování aplikace identit s Azure AD identit najdete v článku [přizpůsobení atribut mapování pro vytváření](active-directory-saas-customizing-attribute-mappings.md)a [přizpůsobení deklarací Vystavitel tokenu SAML](http://social.technet.microsoft.com/wiki/contents/articles/31257.azure-active-directory-customizing-claims-issued-in-the-saml-token-for-pre-integrated-apps.aspx) .

### <a name="understanding-the-users-log-in-experience"></a>Principy protokolu uživatele v prostředí

Při integraci jednotné přihlašování k aplikaci, která se už používá, je třeba si uvědomit, že bude to mít vliv na uživatelské prostředí. Pro všechny aplikace uživatelé začnou, přihlaste se pomocí své přihlašovací údaje Azure AD. Také je možné, že musí používat portál jiné aplikace.

Jednotné přihlašování pro některé aplikace se teď dá na znaménko aplikace v rozhraní, ale pro ostatní aplikace, bude mít uživatel projít centrální portál ATP ([Moje aplikace](http://myapps.microsoft.com) [Office 365](http://portal.office.com/myapps)) přihlásit. Další informace o různých typů jednotného přihlašování a jejich uživatelského prostředí v [aplikaci access a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).

V následujícím článku [směrných vývojáři](active-directory-applications-guiding-developers-for-lob-applications.md) je jiné cenný zdroj *souhlas Suppressing uživatele* .

## <a name="next-steps"></a>Další kroky


SaaS aplikací, které můžete najít v galerii aplikace Azure AD přináší celou řadu [výukové programy pro o tom, jak integrovat SaaS aplikace](active-directory-saas-tutorial-list.md).

Pokud není aplikace do Galerie aplikací, můžete [Přidat do Galerie Azure AD aplikace jako vlastní aplikace](http://blogs.technet.com/b/ad/archive/2015/06/17/bring-your-own-app-with-azure-ad-self-service-saml-configuration-gt-now-in-preview.aspx).

Existuje víc podrobností ve všech těchto problémů v knihovně Azure.com začínající [Co je aplikace access a jednotné přihlašování s Azure Active Directory.](active-directory-appssoaccess-whatis.md).

## <a name="see-also"></a>Viz taky

- [Index článek Správa aplikací služby Azure Active Directory](active-directory-apps-index.md)
