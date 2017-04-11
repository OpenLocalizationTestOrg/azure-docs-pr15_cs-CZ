<Properties
    pageTitle="Azure Active Directory podmíněného přístupu | Microsoft Azure"  
    description="Kontrola určité podmínky při ověřování pro přístup k aplikacím pomocí řízení přístupu podmíněné v Azure Active Directory."  
    services="active-directory"
    keywords="Podmíněné přístup k aplikací, podmíněné přístupu pomocí Azure AD, zabezpečeného přístupu k prostředkům společnosti, zásady podmíněné přístupu"
    documentationCenter=""
    authors="markusvi"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="identity"
    ms.date="09/21/2016"
    ms.author="markvi"/>


# <a name="conditional-access-in-azure-active-directory"></a>Podmíněné přístupu v Azure Active Directory   

Možnosti řízení v Azure Active Directory (Azure AD) podmíněné přístup nabízejí jednoduchých způsobů, jak pomoct zabezpečené zdrojů v cloudu a místní. Zásady podmíněné přístupu jako vícefaktorové ověřování můžete chránit před riziky z odcizení a phished pověření. Jiné zásady přístupu podmíněné můžou pomoct zachovat data organizace. Například kromě vyžaduje přihlašovací údaje, bude pravděpodobně zásad, které jenom zařízení, které je zaregistrované v systému správy mobilních zařízení Microsoft Intune přístup k vaší organizace citlivé služby, jako.


## <a name="prerequisites"></a>Zjistit předpoklady pro

Azure AD podmíněného přístupu je funkce [Azure Active Directory Premium](http://www.microsoft.com/identity). Každý uživatel, kdo má přístup k aplikaci, která obsahuje použité zásady podmíněné přístupu musí mít licenci Azure AD Premium. Další informace o licenční požadavky v [sestavě nelicencovaný uživatele](https://aka.ms/utc5ix).


## <a name="how-is-conditional-access-control-enforced"></a>Způsoby řízení podmíněné přístupu vynucování?  

K ovládacímu prvku podmíněného přístupu na místě, Azure AD kontroluje konkrétní podmínky nastavení pro uživatele k používání aplikace. Po přístup požadavků, uživatel ověřen a přístup k aplikaci.  

![Přehled podmíněné přístupu](./media/active-directory-conditional-access/conditionalaccess-overview.png)

## <a name="conditions"></a>Podmínky

Toto jsou podmínky, které lze zahrnout podmíněné přístupu:

- **Členství ve skupině**. Řízení přístupu uživatele na základě členství ve skupině.

- **Umístění**. Použít umístění uživateli aktivační událost vícefaktorové ověřování a používat ovládací prvky blok, když není uživatel v síti důvěryhodné.

- **Platformu zařízení**. Použití platformu zařízení ATP iOS, Android, Windows Mobile, Windows, jako podmínka pro použití zásad.

- **Zařízení s podporou**. Stav zařízení, zda povolit nebo zakázat, proběhne během hodnocení zásad zařízení. Pokud vypnete ztracené nebo krádež zařízení v adresáři, ji už vyhovovat požadavkům zásad.

- **Přihlášení a uživatel rizika**. [Ochrana Azure AD Identity](active-directory-identityprotection.md) můžete použít pro zásady rizika podmíněný přístupu. Pomáhají podmíněného přístupu rizika zásady ochraně vaší organizace advance na základě rizika událostí a aktivity neobvyklé přihlášení.


## <a name="controls"></a>Ovládací prvky

Toto jsou ovládací prvky, které můžete použít k jejímu vynucení podmíněný přístupu:

- **Vícefaktorové ověřování**. Můžete požádat silných ověřování prostřednictvím vícefaktorové ověřování. Můžete vícefaktorové ověřování Azure Vícefaktorové ověřování nebo pomocí zprostředkovatele místního vícefaktorové ověřování, spolu s Active Directory Federation Services (AD FS). Použití vícefaktorové ověřování chrání zdrojů z přistupuje neautorizovanému uživateli, který může získali přístup k pověření platného uživatele.

- **Blokovat**. Můžete použít podmínky jako umístění uživatele k blokování přístupu uživatelů. Můžete třeba blokovat přístup, když uživatel není v síti důvěryhodné.

- **Kompatibilní se standardem zařízení**. Nastavit zásady podmíněné přístupu na úrovni zařízení. Zásady můžou nastavit tak, aby jenom počítačů, které jsou doméně nebo mobilní zařízení, které jsou zahrnuty do aplikace pro správu mobilních zařízení můžete získat přístup k zdroje organizace. Můžete například zjistit dodržování předpisů zařízení pomocí Intune a když se uživatelé pokusí pro přístup k aplikaci ji nahlásit Azure AD výkon. Podrobné pokyny, jak používat Intune můžete chránit apps a dat najdete v článku [Ochrana apps a dat pomocí Microsoft Intune](https://docs.microsoft.com/intune/deploy-use/protect-apps-and-data-with-microsoft-intune). Taky můžete Intune k jejímu vynucení ochrana před ztrátou nebo odcizením zařízení. Další informace najdete v nápovědě [Ochrana dat pomocí úplné nebo selektivního vymazání pomocí Microsoft Intune](https://docs.microsoft.com/intune/deploy-use/use-remote-wipe-to-help-protect-data-using-microsoft-intune).

## <a name="applications"></a>Aplikace

Vynutit podmíněný přístupu na úrovni aplikace. Nastavení úrovní přístupu pro aplikací a služeb v cloudu a místní. Bude zásada použita přímo na webu nebo služby. Vynucení zásady pro přístup do prohlížeče a aplikací, které přístup ke službě.


## <a name="device-based-conditional-access"></a>Podmíněné přístupu na základě zařízení

Omezení přístupu k aplikacím ze zařízení, které jsou registrované s Azure AD, které splňují určité podmínky. Podmíněné přístupu na základě zařízení chrání zdroje organizace od uživatelů, kteří se pokusí o přístup ke zdrojům z:

- Neznámý nebo nespravovaném zařízení.
- Zařízení, které nevyhovují zásady zabezpečení vaší organizaci nastavená.

Můžete nastavit zásady založené na tyto požadavky:

- **Doméně zařízení**. Nastavení zásad omezení přístupu k zařízení, které jsou připojeny k místní domény Active Directory a, které jsou také registrovaný u Azure AD. Tuto zásadu platí pro stolní počítače, notebooky a enterprise tabletů Windows.
Další informace o tom, jak nastavit automatické registrace doméně zařízení s Azure AD najdete v článku [Nastavení automatické registrace Windows doméně zařízení s Azure Active Directory](active-directory-conditional-access-automatic-device-registration-setup.md).

- **Kompatibilní se standardem zařízení**. Nastavení zásad omezení přístupu k zařízení, které jsou označeny jako **kompatibilní se standardem** v adresáři systému správy. Tuto zásadu zaručuje pouze zařízení, které splňují zásady zabezpečení například vynucení šifrování souborů na zařízení je povolený přístup. Pomocí této zásady můžete omezit přístup z následujících zařízeních:

    - **Zařízení s Windows doméně**. Spravovat pomocí centra Správce konfigurace pro System (v aktuálním větve) používaný v hybridní konfiguraci.
    - **Windows 10 Mobile pracovní nebo osobních zařízení**. Spravovat Intune nebo systému řízení podporované jiného mobilního zařízení.
    - **iOS a Android**. Spravuje Intune.


Uživatelé s přístupem aplikace, které jsou chráněné technologií na zařízení, certifikační autorita zásad musí přístup k aplikaci ze zařízení splňující požadavky tuto zásadu. Přístup odepřen Pokud pokus o na zařízení je nesplňuje požadavky na zásady.

Informace o tom, jak nakonfigurovat zásady certifikační autorita založené na zařízení v Azure AD najdete v tématu [nastavení zásad podmíněné přístupu na základě zařízení připojené služby Azure Active Directory aplikací](active-directory-conditional-access-policy-connected-applications.md).

## <a name="resources"></a>Zdroje informací

Podívejte se na následující zdroje kategorie a články zobrazíte další informace o nastavení podmíněného přístupu pro vaši organizaci.


### <a name="multi-factor-authentication-and-location-policies"></a>Zásady Multi-Factor authentication a umístění

- [Začínáme s podmíněným přístup k Azure AD připojení aplikace založené na skupiny, umístění a zásady vícefaktorové ověřování](active-directory-conditional-access-azuread-connected-apps.md)
- [Aplikace, které podporují](active-directory-conditional-access-supported-apps.md)


### <a name="device-based-conditional-access"></a>Podmíněné přístupu na základě zařízení

- [Nastavení zásad podmíněné přístupu na základě zařízení pro řízení přístupu k aplikacím připojené služby Azure Active Directory](active-directory-conditional-access-policy-connected-applications.md)
- [Nastavení automatické registrace Windows doméně zařízení s Azure Active Directory](active-directory-conditional-access-automatic-device-registration-setup.md)
- [Poradce při potížích s Azure Active Directory access problémy](active-directory-conditional-access-device-remediation.md)
- [Ochranu dat na zařízeních s ztracené nebo krádež pomocí Microsoft Intune](https://docs.microsoft.com/intune/deploy-use/use-remote-wipe-to-help-protect-data-using-microsoft-intune)


### <a name="protect-resources-based-on-sign-in-risk"></a>Ochrana zdroje založené na přihlašovací rizika

-   [Ochrana Azure AD identity](active-directory-identityprotection.md)

### <a name="next-steps"></a>Další kroky

- [Podmíněný přístup časté otázky](active-directory-conditional-faqs.md)
- [Technický přehled](active-directory-conditional-access-technical-reference.md)
