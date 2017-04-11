<properties
   pageTitle="Integrace služby Azure Active Directory s aplikacemi začíná Příručka Začínáme |  Microsoft Azure"
   description="Tento článek se dosáhnout toho, aby příručku Začínáme pro integraci Azure Active Directory (AD) s místním a aplikacemi cloudu."
   services="active-directory"
   documentationCenter=""
   authors="ihenkel"
   manager="femila"
   editor=""/>

   <tags
      ms.service="active-directory"
      ms.devlang="na"
      ms.topic="article"
      ms.tgt_pltfrm="na"
      ms.workload="identity"
      ms.date="02/09/2016"
      ms.author="inhenk"/>

# <a name="integrating-azure-active-directory-with-applications-getting-started-guide"></a>Integrace služby Azure Active Directory s aplikacemi začíná Příručka Začínáme
## <a name="overview"></a>Základní informace
Toto téma je určené vám umožní návod pro integraci aplikace s Azure Active Directory (AD). Každý z níže obsahuje stručný souhrn podrobnější téma proto můžete určit, které části příručku Začínáme dosáhnout toho, aby se vás týkají.  Pomocí odkazů pro hlubší postupy v jednotlivých předmět.

## <a name="before-you-begin-take-inventory"></a>Než začnete, udělejte zásob
Než přejdete do integrace aplikace s Azure AD, je důležité vědět, kde se nacházíte a místo, kam chcete přejít.  Na následující otázky jsou určeny vám myslete Azure AD aplikace integrace projektu.

### <a name="application-inventory"></a>Aplikace zásob
- Kde jsou všechny vaše aplikace? Kdo je vlastní?
- Jaký druh ověřování aplikace vyžadují?
- Kdo potřebuje přístup k aplikacím, které?
- Chcete nasaďte novou aplikaci?
  - Bude vytvářet podnikových a nasadit na instanci Azure výpočetním?
  - Použijete, který je k dispozici v galerii aplikace Azure?

### <a name="user-and-group-inventory"></a>Zásob uživatelů a skupin
- Kde jsou uloženy uživatelských účtů?
 - Místní služby Active Directory
 - Azure AD
 - V samostatné aplikace databázi, kterou vlastníte
 - V aplikacích unsanctioned
 - Všechny výše uvedené
- Co jsou oprávnění a přiřazování rolí jednotliví uživatelé aktuálně mají? Potřebujete zkontrolovat jejich přístup nebo jste si jistí, že uživatel přístup a rolí přiřazení jsou optimální teď?
- Jsou skupiny ho už povolili v na adresářová služba Active Directory?
 - Uspořádání jsou skupin
 - Kteří patří skupiny?
 - K čemu přiřazování oprávnění a rolí skupiny aktuálně mají?
- Potřebují vyčištění uživatele nebo skupinu databází před integrací?  (Toto je poměrně důležité otázku. Chybné v uvolnění mimo.)

### <a name="access-management-inventory"></a>Správa zásob přístup
- Jak se aktuálně spravovat přístup uživatelů k aplikacím? Musím, změnit?  Zvážení další způsoby správy přístupu, jako je třeba s [RBAC](role-based-access-control-configure.md) třeba
- Kdo potřebuje přístup k tomu, co?

Možná jste si ještě odpovědi všem z těchto otázek předem ale to je v pořádku.  Tato příručka, můžete odpovědět některé z těchto otázek a některé rozhodovat.

## <a name="prerequisites"></a>Zjistit předpoklady pro
- Předplatné Azure a adresáře služby Azure Active Directory.  Pokud ještě nemáte předplatné Azure, můžete vyzkoušet Azure zdarma po 30 dní. [Vyzkoušejte si to!](https://azure.microsoft.com/trial/get-started-active-directory/)

## <a name="application-integration-with-azure-ad"></a>Aplikace integrace se službou Azure AD
### <a name="finding-unsanctioned-cloud-applications-with-cloud-app-discovery"></a>Zjištění unsanctioned cloudové aplikace s cloudu aplikace zjišťování
Výše uvedené, může být aplikace, které nebyly spravovat vaši organizaci do nyní.  Jako součást procesu zásob je možné najít unsanctioned cloudu aplikace. Najdete v článku [hledání unsanctioned cloudové aplikace s zjišťování aplikace cloudu](active-directory-cloudappdiscovery-whatis.md).

### <a name="authentication-types"></a>Typ ověřování
Všech vašich aplikací může být různé ověřování požadavky. S Azure AD podepisování certifikáty mohou sloužit k aplikacím, které lze použít SAML 2.0, WS-Federation nebo OpenID připojení protokoly i heslo jednotné přihlašování. Další informace o aplikaci typů ověřování pro použití s Azure AD najdete v článku [Správa certifikátů pro federované jednotné přihlašování v Azure Active Directory](active-directory-sso-certs.md) a [heslo na základě jednotného přihlášení](active-directory-appssoaccess-whatis.md).

### <a name="enabling-sso-with-azure-ad-app-proxy"></a>Povolení jednotné přihlašování s Azure AD aplikace Proxy
S Microsoft Azure AD aplikace Proxy můžete poskytnout přístup k aplikacím nalepený uvnitř privátní sítě bezpečně, odkudkoli a na libovolném zařízení. Po instalaci konektor proxy aplikací v prostředí ji možné snadno konfigurovat s Azure AD.

### <a name="integrating-applications-with-azure-ad"></a>Integrace aplikace s Azure AD
V následujících článcích diskutovat o různých způsobech aplikací integrace s Azure AD a obsahují některé pokyny.

- [Zjištění, které služba Active Directory použít](active-directory-administer.md)
- [Použití aplikací v galerii Azure aplikace](active-directory-appssoaccess-whatis.md)
- [Integrace SaaS aplikací výukové programy pro seznam](active-directory-saas-tutorial-list.md)

## <a name="managing-access-to-applications"></a>Správa přístupu k aplikacím
V následujících článcích se dočtete způsoby, jak se dá řídit přístup k aplikacím po jejich bylo integrováno Azure AD pomocí Azure AD spojnic a Azure AD.

- [Správa přístupu k aplikacím pomocí Azure AD](active-directory-managing-access-to-apps.md)
- [Automatizace s Azure AD spojnic](active-directory-saas-app-provisioning.md)
- [Přiřazení uživatelů do aplikace](active-directory-applications-guiding-developers-assigning-users.md)
- [Přiřazení skupin do aplikace](active-directory-applications-guiding-developers-assigning-groups.md)
- [Sdílení účty](active-directory-sharing-accounts.md)

## <a name="integrating-custom-applications"></a>Integrace vlastní aplikace
Pokud jsou psaní novou aplikaci a chcete pomáhat vývojáři v využívání power Azure AD, najdete v článku [směrných vývojáři](active-directory-applications-guiding-developers-for-lob-applications.md).

Pokud chcete přidat vlastní aplikace do Galerie aplikací Azure, najdete v článku ["Přenést vlastní aplikaci" s konfigurací Azure AD samoobslužné SAML](http://blogs.technet.com/b/ad/archive/2015/06/17/bring-your-own-app-with-azure-ad-self-service-saml-configuration-gt-now-in-preview.aspx).

## <a name="see-also"></a>Viz taky

- [Index článek Správa aplikací služby Azure Active Directory](active-directory-apps-index.md)
