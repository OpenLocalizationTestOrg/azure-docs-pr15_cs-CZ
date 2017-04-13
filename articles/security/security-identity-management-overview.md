<properties
   pageTitle="Přehled správy zabezpečení Azure identity | Microsoft Azure"
   description=" Microsoft identit a přístup k řešení Nápověda ke správě informačních chránit přístup k aplikacím a zdroje přes podnikové datacentra a do cloudu, povolení dalších úrovní ověření například vícefaktorové ověřování a zásady podmíněné přístupu. Tento článek obsahuje přehled základní funkcemi zabezpečení Azure pomocí služby Správa identit. "
   services="security"
   documentationCenter="na"
   authors="TerryLanfear"
   manager="MBaldwin"
   editor="TomSh"/>

<tags
   ms.service="security"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/09/2016"
   ms.author="terrylan"/>

# <a name="azure-identity-management-security-overview"></a>Přehled správy zabezpečení Azure identity

Microsoft identit a přístup k řešení Nápověda ke správě informačních chránit přístup k aplikacím a zdroje přes podnikové datacentra a do cloudu, povolení dalších úrovní ověření například vícefaktorové ověřování a zásady podmíněné přístupu. Sledování podezřelé aktivity prostřednictvím pokročilým zabezpečením vytváření sestav, auditování a výstrahy, že pomáhá zmírnění potenciálních problémů zabezpečení. [Azure Active Directory Premium](../active-directory/active-directory-editions.md) poskytuje jednotného přihlašování k tisícům cloudu aplikace (SaaS) a přístup k webových aplikací web apps vám nestalo místní.

Výhody z Azure Active Directory (AD) umožňují:

- Vytváření a správa jednoho identity pro každého uživatele ve vaší organizaci hybridní udržování uživatelé, skupiny a zařízení synchronizace
- Poskytnutí přístupu jednoho přihlašování k aplikacím včetně tisíce předem integrované aplikace SaaS
- Povolit zabezpečení aplikace access vynucení Multi-Factor ověřování na základě pravidel pro obě místních a cloudových aplikací
- Poskytnutí zabezpečeného vzdálený přístup k místním webových aplikací prostřednictvím proxy server Azure AD aplikace

Hledání v tomto článku je uvádějí přehled základní funkcemi zabezpečení Azure pomocí služby Správa identit. Také nabízíme odkazy na články, které vám umožní podrobnosti každé funkce, které se můžou dozvědět víc.  

Tento článek se zaměřuje na následující možnosti správy identit Azure core:

- Jednotné přihlašování
- Reverzního proxy
- Vícefaktorové ověřování
- Sledování zabezpečení, upozornění a sestavy založené na výukové počítače
- Správa identity a přístup příjemce
- Registrace zařízení
- Správa privilegovaných identit
- Ochrana identity
- Správa identit hybridní

## <a name="single-sign-on"></a>Jednotné přihlašování

Jediný přihlašování (SSO) dopravní nebudete mít přístup k celému aplikací a zdroje informací, které je potřeba Jednejte tak, že v jenom jednou pomocí jednoho uživatelského účtu. Po přihlášení se zobrazí všechny aplikace, budete potřebovat bez nutnosti ověřování (například zadat heslo) ještě jednou.

Mnoho organizací potřebují software jako aplikací služby (SaaS) například Office 365 graf a služby Salesforce produktivity koncového uživatele. Z historických důvodů zaměstnance IT potřebné k jednotlivě vytvořit a aktualizovat uživatelské účty v každé aplikaci SaaS a uživatelé museli heslo pro každou SaaS aplikaci.

Azure AD slouží k rozšíření místních Active Directory do cloudu, uživatelům umožňuje použít jejich primární účet organizace a nejen Přihlaste se k jejich doméně zařízení společnosti zdroje, ale taky webu a SaaS aplikace potřebná pro svou práci.

Pouze uživatelé nemají ke správě více sad uživatelská jména a hesla, aplikace access můžete automaticky zřizování nebo zrušit zřizování podle skupin a jejich stav zaměstnance. Azure AD uvádí zabezpečení a přístupu zásady správného řízení ovládacích prvků, které vám umožní centrálně spravovat přístup uživatelů ve všech aplikacích SaaS.

Víc se uč:

- [Přehled jednotného přihlašování](https://azure.microsoft.com/documentation/videos/overview-of-single-sign-on/)
- [Co je aplikace access a jednotné přihlašování s Azure Active Directory?](../active-directory/active-directory-appssoaccess-whatis.md)
- [Integrace služby Azure Active Directory jednotné přihlašování s aplikacemi jiných SaaS](../active-directory/active-directory-sso-integrate-saas-apps.md)

## <a name="reverse-proxy"></a>Reverzního proxy

Proxy server Azure AD aplikace umožňuje publikovat místní aplikace, třeba weby [SharePoint](https://support.office.com/article/What-is-SharePoint-97b915e6-651b-43b2-827d-fb25777f446f?ui=en-US&rs=en-US&ad=US) , [Outlook Web App](https://technet.microsoft.com/library/jj657718.aspx)a [služby IIS](http://www.iis.net/)-na základě aplikace uvnitř privátní sítě a poskytuje zabezpečený přístup uživatelům mimo vaši síť. Proxy aplikace poskytuje vzdáleného přístupu a jednotné přihlašování (SSO) pro mnoho typů webových aplikací místní tisíců SaaS aplikace, které podporuje Azure AD. Zaměstnanců můžou přihlásit do aplikace z Domů na svoje zařízení a ověření prostřednictvím tento cloudové proxy.

Víc se uč:

- [Povolení proxy server Azure AD aplikace](../active-directory/active-directory-application-proxy-enable.md)
- [Publikování aplikací pomocí proxy server Azure AD aplikace](../active-directory/active-directory-application-proxy-publish.md)
- [Jednoduchá jednotného přihlašování k aplikaci Proxy](../active-directory/active-directory-application-proxy-sso-using-kcd.md)
- [Práce s podmíněným přístupem](../active-directory/active-directory-application-proxy-conditional-access.md)

## <a name="multi-factor-authentication"></a>Vícefaktorové ověřování
Azure vícefaktorové ověřování (MFA) je metody ověřování, který je potřeba použít více než jednu metodu ověření a uživatel přihlášení, začátky transakce se přidá kritické druhý vrstvy cenného papíru. MFA pomáhá chránit přístup k datům a aplikace během schůzky požádání uživatele pro jednoduchý proces přihlášení. Poskytuje silných ověřovat pomocí celou řadu možností ověření – telefonní hovor, textová zpráva nebo mobilní aplikaci oznámení nebo ověření kód a 3rd stran OAuth tokeny.

Víc se uč:

- [Vícefaktorové ověřování](https://azure.microsoft.com/documentation/services/multi-factor-authentication/)
- [Co je Azure Vícefaktorové ověřování?](../multi-factor-authentication/multi-factor-authentication.md)
- [Jak funguje Azure Vícefaktorové ověřování](../multi-factor-authentication/multi-factor-authentication-how-it-works.md)

## <a name="security-monitoring-alerts-and-machine-learning-based-reports"></a>Sledování zabezpečení, upozornění a sestavy založené na výukové počítače

Sledování zabezpečení a upozornění a počítač výukové sestavy založené určující vzorky konzistentní přístup pomáhá chránit vaši firmu. Pokud chcete získat přehled o integrita a zabezpečení adresáře vaší organizace můžete přístup Azure Active Directory a sestavy využití. Tyto informace jako správce adresáře můžete lépe zjistit, kde může být možné zabezpečení rizikové tak, aby dostatečně můžou plánovat pomoci tato rizika zmírnit.

Na portálu Azure klasické sestavy zařazené do různých následujícími způsoby:

- Sestavy odchylky – obsahovat přihlásit události, které jsme našli být neobvyklých. Naším cílem je přesně přehled o těchto aktivitě a umožňuje vám budete moci určit, zda je podezřelý události.
- Integrované aplikace sestavy – poskytují podstatu používání cloudové aplikace ve vaší organizaci. Azure Active Directory umožňuje integraci s tisíce cloudové aplikace.
- Zpráv o chybách – Označit chyby, které se mohou vyskytnout, když zřizujete účtů externích aplikacích.
- Sestavy specifické pro uživatele – zobrazení zařízení/znaménko aktivity dat pro konkrétní uživatele.
- Protokoly aktivity – obsahovat záznam všechny ověřené události v rámci posledních 24 hodin, posledních 7 dnech nebo posledních 30 dní i změn aktivity skupin a heslo resetovat a registrace aktivity.

Víc se uč:

- [Zobrazení sestav aplikace access a použití](../active-directory/active-directory-view-access-usage-reports.md)
- [Začínáme s Azure Active Directory vytváření sestav](../active-directory/active-directory-reporting-getting-started.md)
- [Příručka pro vytváření sestav Azure Active Directory](../active-directory/active-directory-reporting-guide.md)

## <a name="consumer-identity-and-access-management"></a>Správa identity a přístup příjemce

Azure Active Directory B2C je služba správy identit vysoce dostupné, globální pro spotř webových aplikací, který rozšiřuje stovky miliony identity. Může být integrovaná přes mobilní telefon a webových platformách. Uživatele můžete Přihlaste se ke všem aplikacím prostřednictvím přizpůsobitelné prostředí pomocí své existující účty sociálních nebo vytvořením nového přihlašovací údaje.

V minulosti vývojáře aplikací, kteří chtěli zaregistrovat a přihlaste se do svých aplikací spotřebitele by vytvořilo vlastní kód. A má mít používají místní databáze nebo systémů pro ukládání uživatelská jména a hesla. Azure Active Directory B2C nabízí lepší způsob správy identit spotř integrovat do pomocí zabezpečené a standardy platformu a velké sadu extensible zásad vaší organizace.

Při použití Azure Active Directory B2C vaše uživatelé můžou zaregistrovat ke aplikace pomocí své existující účty sociálních (Facebook Google, Amazon, Linkedinu) nebo vytváření nových přihlašovacích údajů (e-mailovou adresu a heslo, nebo uživatelské jméno a heslo).

Víc se uč:

- [Co je Azure Active Directory B2C?](https://azure.microsoft.com/services/active-directory-b2c/)
- [Azure Active Directory B2C náhledu: Přihlaste nahoru a přihlaste se uživatelé v aplikacích](../active-directory-b2c/active-directory-b2c-overview.md)
- [Náhled B2C Azure Active Directory: Typy aplikací](../active-directory-b2c/active-directory-b2c-apps.md)

## <a name="device-registration"></a>Registrace zařízení

Registrace zařízení Azure AD se základem [podmíněného přístupu](../active-directory/active-directory-conditional-access-on-premises-setup.md) na základě zařízení scénářích. Při registraci zařízení, poskytuje Azure Active Directory zařízení registrace zařízení k identitě, která slouží k ověření zařízení, pokud se uživatel přihlásí. Ověření zařízení a atributy zařízení, potom slouží k jejímu vynucení zásady podmíněné přístupu pro aplikace, které jsou hostované v cloudu a místní.

Další informace o zařízení aktualizují v kombinaci s řešení pro správu (MDM) mobilní zařízení, třeba Intune, atributy zařízení v Azure Active Directory. Umožňuje vytvořit pravidla podmíněného přístupu vynucující přístup z zařízení, které odpovídají vašim požadavkům na zabezpečení a dodržování předpisů.

Víc se uč:

- [Začínáme s Azure Active Directory zařízení registrace](../active-directory/active-directory-conditional-access-device-registration-overview.md)
- [Nastavovat místní podmíněné připojení přes Azure Active Directory zařízení registrace](../active-directory/active-directory-conditional-access-on-premises-setup.md)
- [Registrace automatické zařízení se zařízeními doméně Azure Active Directory pro Windows](../active-directory/active-directory-conditional-access-automatic-device-registration.md)

## <a name="privileged-identity-management"></a>Správa privilegovaných identit
Správa identit privilegovaných služby Azure Active Directory (AD) umožňuje spravovat, řídit a sledovat privilegovaných identit a přístup k zdrojů v Azure AD, jakož i ostatní online služby jako je Office 365 nebo Microsoft Intune.

Uživatelé někdy potřebovat provádět privilegovaných operace v Azure nebo Office 365 zdrojů nebo jiných aplikací SaaS. Často znamená to, že organizacemi muset udělte jim přístupová oprávnění trvalé privilegovaných v Azure AD. Totiž rostoucí rizika zabezpečení pro hostované cloudové zdroje organizace nemůžete sledovat dostatečně co dělají tyto uživatele se jeho oprávnění správce. Navíc pokud ohroženo uživatelský účet s přístup privilegovaných daného jeden porušení můžou mít vliv na celkové zabezpečení cloudové. Správa identit Azure AD oprávněními pomůže vyřešit toto riziko.

Správa identit Azure AD oprávněními umožňuje:

- Zjistit, které uživatelé jsou správci Azure AD
- Povolit na vyžádání "time" pro správu přístup ke službám Microsoft Online Services, jako je Office 365 a Intune
- Získání sestav o zjišťování Správce zobrazení historie a změny přiřazení správce
- Získat oznámení o přístup k roli privilegovaných

Víc se uč:

- [Správa identit Azure AD oprávněními](../active-directory/active-directory-privileged-identity-management-configure.md)
- [Role ve službě Správa identit Azure AD oprávněními](../active-directory/active-directory-privileged-identity-management-roles.md)
- [Azure AD privilegovaných Správa identit: Jak pokud chcete přidat nebo odebrat role uživatele](../active-directory/active-directory-privileged-identity-management-how-to-add-role-to-user.md)

## <a name="identity-protection"></a>Ochrana identity
Ochrana Azure AD Identity je služba zabezpečení, která nabízí do rizika událostí a potenciální chyby došlo k ovlivnění identit vaší organizace. Ochrana identity využívá existující Azure Active Directory společnosti odchylky zjišťování možnosti (dostupné prostřednictvím Azure AD neobvyklých aktivity sestavy) a uvádí nové typy události rizika, které rozpozná odchylky v reálném čase.

Víc se uč:

- [Ochrana Identity Azure Active Directory](../active-directory/active-directory-identityprotection.md)
- [Kanál 9: Azure AD a Identity zobrazit: náhled ochranu Identity](https://channel9.msdn.com/Series/Azure-AD-Identity/Azure-AD-and-Identity-Show-Identity-Protection-Preview)

## <a name="hybrid-identity-management"></a>Správa identit hybridní

Přístupu společnosti Microsoft k identity přesahuje místním prostředím a cloudem, vytváření jednoho uživatele identity pro a tak mohli ověřovat pro všechny zdroje, bez ohledu na umístění.

Víc se uč:

- [Dokument white paper hybridní identity](http://download.microsoft.com/download/D/B/A/DBA9E313-B833-48EE-998A-240AA799A8AB/Hybrid_Identity_White_Paper.pdf)
- [Azure Active Directory](https://azure.microsoft.com/documentation/services/active-directory/)
- [Blog týmu služby Active Directory](https://blogs.technet.microsoft.com/ad/)
