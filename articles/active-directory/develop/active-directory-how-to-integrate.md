<properties
   pageTitle="Integrace služby Azure Active Directory | Microsoft Azure"
   description="Příručka k výhody a materiály pro integrace se službou Azure Active Directory."
   services="active-directory"
   documentationCenter="dev-center-name"
   authors="bryanla"
   manager="mbaldwin"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="09/16/2016"
   ms.author="mbaldwin"/>

# <a name="integrating-with-azure-active-directory"></a>Integrace se službou Azure Active Directory

[AZURE.INCLUDE [active-directory-devguide](../../../includes/active-directory-devguide.md)]

Azure Active Directory slouží organizacím s Správa identit podnikové získáte cloudu.  Azure AD integrace nabídne uživatelů optimalizovaného přihlášení a pomáhá aplikace odpovídat zásadám IT.

## <a name="how-to-integrate"></a>Jak integrovat

Existuje několik způsobů aplikace integrovat s Azure AD.  Využívejte jako mnoho nebo málo podobnému sledu podle příslušné aplikace.

### <a name="support-azure-ad-as-a-way-to-sign-in-to-your-application"></a>Podpora Azure AD jako způsob, jak přihlásit do aplikace

**Omezit přihlásit tření a snížit náklady na podporu.** Pomocí Azure AD se přihlásit do aplikace nebude mít vaši uživatelé více názvů a při přihlášení.  Vývojář budete mít jedno méně heslo k ukládání a chránit.  Nemusíte dělat obnovení zapomenutého hesla může být významné úspor samostatně.  Azure AD zajišťuje přihlásit pro některé aplikace na světě nejoblíbenější cloudu, včetně služeb Office 365 a Microsoft Azure.  S stovky miliony uživatelům miliony organizace, je pravděpodobné, uživatel už přihlášení k Azure AD.  Další informace o [Přidání podpory pro Azure AD přihlásit](../active-directory-authentication-scenarios.md).

**Zjednodušte Odhlásit se aplikace.**  Během registrace aplikace Azure AD můžete odeslat základní informace o uživateli, můžete předem vyplnit vaše registrace formuláře nebo úplně odstranit.  Uživatelé můžou zaregistrovat ke aplikace pomocí svůj účet Azure AD pomocí známé souhlas setkat i v případě podobné prvkům v sociální média a mobilní aplikace.  Všichni uživatelé můžete zaregistrovat a přihlaste se k aplikaci, která je integrována s Azure AD bez nutnosti zapojení IT.  Další informace o [přihlašování nakreslenými aplikace pro přihlašování pomocí zvláštního účtu Azure AD](../../app-service-mobile/app-service-mobile-how-to-configure-active-directory-authentication.md).

### <a name="browse-for-users-manage-user-provisioning-and-control-access-to-your-application"></a>Vyhledejte uživatele zřizování uživatelů a spravovat pomocí řízení přístupu k aplikaci

**Vyhledejte uživatelů v adresáři.**  Pomoc uživatelům hledat a vyhledejte dalším osobám ve své organizaci při pozvete jiné uživatele pomocí rozhraní API grafu nebo udělení přístupu místo vyžaduje, aby zadejte e-mailové adresy.  Uživatelé, můžete přecházet pomocí známé adresu knihy styl rozhraní, včetně zobrazení podrobností o hierarchie v organizaci.  Další informace o rozhraní [API grafu](../active-directory-graph-api.md).

**Opakované použití skupin Active Directory a distribuční seznamy, které už správy zákazníkovi.**  Azure AD obsahuje skupiny, že váš zákazník už používáte pro distribuci e-mailu a správa přístupu.  Rozhraní API grafu, znovu pomocí těchto skupin místo vyžadující zákazníkovi můžete vytvořit a spravovat samostatných sadu skupin v aplikaci.  Informace o skupinách také odesílat aplikaci v přihlašovacím tokeny.  Další informace o rozhraní [API grafu](../active-directory-graph-api.md).

**Použití Azure AD cílem určit, kdo má přístup k aplikaci.**  Správci a vlastníci aplikace v Azure AD můžete přiřadit přístup k aplikacím konkrétním uživatelům a skupinám.  Rozhraní API grafu můžete číst tento seznam a použít k ovládání zřizování a zrušte zřizování zdrojů a přístup k vaší aplikaci.

**Řízení přístupu na základě použití Azure AD pro role.**  Správci a vlastníci aplikace můžete přiřadit uživatelům a skupinám role, které definujete při registraci aplikace v Azure AD.  Informace o rolích odeslaný do aplikace v přihlašovacím tokeny a můžete taky přečíst pomocí rozhraní API grafu.  Další informace o [používání Azure AD pro povolení](http://blogs.technet.com/b/ad/archive/2014/12/18/azure-active-directory-now-with-group-claims-and-application-roles.aspx).

### <a name="get-access-to-users-profile-calendar-email-contacts-files-and-more"></a>Získání přístupu k profilu uživatele, kalendář, e-mailu, kontaktů, soubory a další

**Azure AD je server se tak mohli ověřovat pro Office 365 a další služby od Microsoftu business.**  Pokud podporují Azure AD pro přihlášení k aplikaci nebo podpora propojení aktuální uživatelských účtů na uživatelské účty Azure AD pomocí OAuth 2.0, můžete požádat o čtení a zápis uživatelského profilu, kalendář, e-mailu, kontaktů, soubory a dalších informací.  Můžete Bezproblémová zapsat události do kalendáře uživatele a číst nebo zapisovat soubory na svém Onedrivu.  Další informace o [přístup k rozhraní API Office 365](https://msdn.microsoft.com/office/office365/howto/platform-development-overview).

### <a name="promote-your-application-in-the-azure-and-office-365-marketplaces"></a>Zvýšení úrovně aplikace v Azure a tržiště Office 365

**Zvýšení úrovně aplikaci miliony organizacích, kteří už používají Azure AD.**  Uživatelé, kteří Hledat a vyhledejte tyto tržiště již používají jednu nebo více cloud services, což je kvalifikované služeb zákazníkům cloudu.  Další informace o podpoře aplikace v [Azure Marketplace](https://azure.microsoft.com/marketplace/partner-program/).

**Když se uživatelé zaregistrují k aplikaci, se zobrazí v jejich panel Azure AD přístup a Spouštěč aplikací Office 365.**  Uživatelé budou moct snadno a rychle vrátit k vaší aplikaci později, vylepšení uživatelského engagement.  Další informace o [Azure AD přístup k panelu](../active-directory-saas-access-panel-introduction.md).

### <a name="secure-device-to-service-and-service-to-service-communication"></a>Zabezpečená komunikace zařízení služby a služby Service

**Použití Azure AD pro Správa identit služeb a zařízení snižuje kód, který potřebujete napsat a umožňují IT spravovat přístup.**  Služby a zařízeních můžete získat tokenů z Azure AD pomocí OAuth a těchto tokenů pro přístup k webu rozhraní API.  Použití Azure AD se můžete vyhnout psaní složité ověřovací kód.  Protože identitami služeb a zařízení uložené v Azure AD IT klíče a odvolání na jednom místě, takže není nutné provést samostatně aplikace můžete spravovat.

## <a name="benefits-of-integration"></a>Výhody integrace

Integrace se službou Azure AD získáváte přínosů, které nevyžadují další kódu.

### <a name="integration-with-enterprise-identity-management"></a>Integrace se službou Správa identit Enterprise

**Nápověda aplikace dodržování zásady.**  Organizace integrace jejich organizace systémech správy identit s Azure AD, takže pokud osobu opustí organizaci, budou se automaticky ztratíte přístup k aplikaci bez IT museli provést další kroky.  IT můžete spravovat, kdo má přístup k aplikaci a zjistit, jaké zásady přístupu jsou potřeba – pro příklad vícefaktorové ověřování – zmenšení potřeby si podklady kód dodržovat složité podnikových zásad.  Azure AD poskytuje správcům protokol podrobné auditování kdo přihlášení do aplikace tak IT můžete sledovat použití.

**Azure AD rozšiřuje služby Active Directory do cloudu, aby vaše aplikace můžete integrovat s AD.**  Více organizacemi po celém světě pomocí služby Active Directory jejich hlavní přihlásit a systému správy identit a vyžadovat jejich aplikací pro práci s AD.  Integrace s Azure AD integruje aplikace se službou Active Directory.

### <a name="advanced-security-features"></a>Rozšířené funkce zabezpečení

**Vícefaktorové ověřování.**  Azure AD poskytuje nativní vícefaktorové ověřování.  Správce IT může vyžadují vícefaktorové ověřování pro přístup k aplikaci nemáte kódu toto odborné pomoci sami.  Další informace o [Vícefaktorové ověřování](https://azure.microsoft.com/documentation/services/multi-factor-authentication/).

**Neobvyklých přihlašovací rozpoznávání.**  Azure AD zpracuje víc než miliard přihlášení denně při používání algoritmů výukové počítače ke zjištění podezřelé aktivity a upozornit správce IT a obsahuje možné problémy.  Podporou přihlašovací Azure AD získá aplikace výhodu ochrany. Další informace o [zobrazení Azure Active Directory sestava aplikace access](../active-directory-view-access-usage-reports.md).

**Podmíněné přístup.**  Kromě vícefaktorové ověřování, můžete správci vyžadují určité podmínky být splněny uživatelé mohli přihlašovat do aplikace.  Podmínky, které můžete nastavit zahrnují rozsah IP adres klientských zařízení, členství ve skupinách zadaný a stav zařízení používá pro access.  Další informace o [podmíněném přístupu Azure Active Directory](../active-directory-conditional-access.md).

### <a name="easy-development"></a>Snadno vývoj

**Standardních protokolů.**  Společnost Microsoft se zavázala na referenční oborové standardy.  Azure AD podporuje protokoly ověřování SAML 2.0, OpenID připojení 1.0, OAuth 2.0 a WS Federation 1.2.  Rozhraní API graf je kompatibilní se standardem OData 4.0.  Pokud již aplikace podporuje protokoly SAML 2.0 nebo 1.0 OpenID připojit k federovaným přihlášení, může být přidání podpory pro Azure AD jednoduchých.  Další informace o [protokolech ověřování Azure AD podporované](../active-directory-authentication-protocols.md).

**Otevření knihovny zdrojů.**  Microsoft poskytuje knihovny plně podporované otevřít zdroj pro oblíbené jazyky a platformách urychlit vývoj.  Zdrojový kód je licencován Apache 2.0 a máte volno stůl a přispívání zpátky do projektů.  Další informace o [knihovnách ověřování Azure AD](../active-directory-authentication-libraries.md).

### <a name="worldwide-presence-and-high-availability"></a>Celosvětová dostupnost informací o stavu a dostupnost

**Azure AD nasazenou v datacentrech celého světa se spravovaného a sledovat nepřetržitě.**  Azure AD je systému správy identit Microsoft Azure a Office 365 a nasazení v 28 datacentrech ve světě.  Data v adresáři zaručena replikovat do nejméně tři datacentrech.  Vyrovnávání zatížení globální zajistit přístup uživatelům nejbližší kopii Azure AD obsahující data a automaticky znovu směrovat požadavky na jiných datacentrech Pokud byl zjištěn problém.

## <a name="next-steps"></a>Další kroky

[Začněte psát kód](../active-directory-developers-guide.md#getting-started).

[Symbol uživatele při používání Azure AD](../active-directory-authentication-scenarios.md)
