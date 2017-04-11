<properties
    pageTitle="Azure AD a aplikací: vývojáři vedení | Microsoft Azure"
    description="Napsané pro IT specialisty, tento článek obsahuje pokyny pro integraci Azure aplikace se službou Active Directory."
    services="active-directory"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/03/2016"
    ms.author="kgremban"/>

# <a name="azure-ad-and-applications-develop-line-of-business-apps"></a>Azure AD a aplikace: vývoj řádku obchodních aplikací

Tato příručka obsahuje přehled vývoj aplikací (LoB) řádek obchodní pro Azure Active Directory (AD). Cílová skupina je globální správce Active Directory/Office 365.

## <a name="overview"></a>Základní informace

Vytváření aplikací integrovaný s Azure AD umožňuje uživatelům ve vaší organizaci jednotné přihlašování v Office 365. Problémy aplikace v Azure AD nabízí ovládat zásady ověřování aplikace. Další informace o podmíněné přístup a jak chránit aplikací klasické vícefaktorové ověřování (MFA) najdete v tématu [Konfigurace pravidla přístupu](active-directory-conditional-access-azuread-connected-apps.md).

Registrace aplikace pomocí služby Azure Active Directory. Registrace aplikace znamená, že vaše vývojáři používaný Azure AD pro ověřování uživatelů a požádat o přístup k uživatele zdroje, jako jsou e-mailu, kalendáři a dokumenty.

Každý člen adresáře (ne hostů) můžete zaregistrovat aplikaci, jinak jmenoval *vytváření objekt aplikace*.

Registrace aplikace umožňuje každému uživateli postupujte takto:

- Získání identitu jejich aplikace, které rozpozná Azure AD
- Získání jeden nebo více tajemství/klíčů používající aplikace a dokončit ověření AD
- Přizpůsobit aplikace na portálu Azure pomocí vlastního názvu, loga, atd.
- Použití funkce se tak mohli ověřovat Azure AD pro jejich aplikaci, včetně:
  - Řízení přístupu na základě rolí (RBAC)
  - Jako server se tak mohli ověřovat oAuth Azure Active Directory (secure rozhraní API zveřejněné příslušným aplikace)

- Deklarovat potřebná oprávnění potřebná pro použití funkce očekávaným způsobem, včetně: – oprávnění aplikace (jenom globální správci). Příklad: Role členství v jiné Azure AD aplikace členství role nebo relativní zdroj Azure, skupina zdrojů nebo předplatné - delegované oprávnění (všechny uživatele). Příklad: Azure AD přihlašování a profilu pro čtení


> [AZURE.NOTE]Ve výchozím nastavení může každý člen registrace aplikace. Zjistěte, jak omezit oprávnění pro registraci aplikace jen vybraným členům skupiny, najdete v článku [jak aplikací přidají Azure AD](active-directory-how-applications-are-added.md#who-has-permission-to-add-applications-to-my-azure-ad-instance).

Tady je co, globálního správce, musíte udělat, aby vývojáři znemožní jejich použití jste připraveni na výrobní:

- Konfigurace pravidla přístupu (zásady přístupu/MFA)
- Konfigurace aplikace vyžadují přiřazení uživatele a přiřadit uživatelům
- Potlačit výchozí uživatelské souhlas prostředí

## <a name="configure-access-rules"></a>Konfigurace pravidla přístupu

Konfigurace pravidel za aplikace přístup ke svým aplikacím SaaS. Můžete třeba vyžadovat MFA nebo jenom přístupu pro uživatele v síti důvěryhodné. Podrobnosti o to jsou k dispozici v dokumentu [Konfigurace pravidla přístupu](active-directory-conditional-access-azuread-connected-apps.md).

## <a name="configure-the-app-to-require-user-assignment-and-assign-users"></a>Konfigurace aplikace vyžadují přiřazení uživatele a přiřadit uživatelům

Ve výchozím nastavení mohou uživatelé aplikace bez přiřazení. Ale pokud aplikace zpřístupňuje role nebo pokud chcete zobrazit na panelu aplikace access uživatele aplikace, je potřeba povolit přiřazení uživatele.

[Vyžadování přiřazení uživatele](active-directory-applications-guiding-developers-requiring-user-assignment.md)

Pokud jste Azure AD Premium nebo Enterprise mobilita sady (EMS) odběratelů, důrazně doporučujeme používat skupiny. Přiřazení skupiny do aplikace umožňuje delegovat správu průběžného přístupu k vlastník skupiny. Můžete vytvořit skupinu nebo požádejte odpovědná strana ve vaší organizaci můžete vytvořit skupinu pomocí zařízení Správa skupiny.

[Přiřazení uživatelů do aplikace](active-directory-applications-guiding-developers-assigning-users.md)  
[Přiřazení skupin do aplikace](active-directory-applications-guiding-developers-assigning-groups.md)

## <a name="suppress-user-consent"></a>Potlačit souhlasu uživatele

Ve výchozím nastavení každý uživatel prochází setkat i v případě souhlasu se přihlásit. Prostředí souhlas s dotazem, uživatelům udělit oprávnění k aplikaci, může být zneklidňovat pro uživatele, kteří jsou seznámeni s rozhodování.

U aplikací, kterým důvěřujete můžete zjednodušit uživatelské prostředí tím souhlas s aplikací jménem vaší organizace.

Další informace o uživatele souhlas, souhlas prostředí v Azure najdete v článku [Aplikace integrace se službou Azure Active Directory](active-directory-integrating-applications.md).

##<a name="related-articles"></a>Související články

- [Povolení zabezpečené vzdálený přístup k aplikacím místní s proxy server Azure AD aplikace](active-directory-application-proxy-get-started.md)
- [Náhled Azure podmíněné přístup k aplikacím SaaS](active-directory-conditional-access-azuread-connected-apps.md)
- [Správa přístupu k aplikacím s Azure AD](active-directory-managing-access-to-apps.md)
- [Index článek Správa aplikací služby Azure Active Directory](active-directory-apps-index.md)
