<properties
    pageTitle="Nejčastější dotazy týkající se Azure Active Directory | Microsoft Azure"
    description="Azure Active Directory – nejčastější dotazy, které obsahuje odpovědi na otázky v souvislosti s přístupem k Azure a služba Azure Active Directory, heslo řízení a aplikace access."
    services="active-directory"
    documentationCenter=""
    authors="markusvi"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="08/16/2016"
    ms.author="markusvi"/>

# <a name="azure-active-directory-faq"></a>Nejčastější dotazy týkající se Azure Active Directory

Azure Active Directory je komplexní identitu jako řešení pro službu (IDaaS), který přesahuje všechny aspekty identit, správu přístupu a zabezpečení.


Další informace najdete v tématu [Co je Azure Active Directory?](active-directory-whatis.md).



## <a name="accessing-azure-and-azure-active-directory"></a>Přístup k Azure a Azure Active Directory


**Otázka: Proč dostávám "nebyl nalezen žádný předplatná" při pokusu o přístup k Azure AD na portálu Azure klasické (https://manage.windowsazure.com)?**

**A:** Přístup k portálu Azure klasické vyžaduje každému uživateli přidělit oprávnění Azure předplatného. Pokud máte placené Office 365 nebo Azure AD přejděte na [http://aka.ms/accessAAD](http://aka.ms/accessAAD) jednorázových aktivaci, jinak se musíte aktivovat úplné [zkušební verzi Azure](https://azure.microsoft.com/pricing/free-trial/) nebo na placené předplatné. 

Další informace najdete v tématu:

- [Jak Azure předplatná souvisí s Azure Active Directory](active-directory-how-subscriptions-associated-directory.md)

- [Správa v adresáři vašeho předplatného Office 365 v Azure](active-directory-manage-o365-subscription.md)

---

**Otázka: Jaký je vztah mezi Azure AD Office 365 a Azure?**

**A:** Azure Active Directory obsahuje běžné možnosti identit a přístup ke všem službám Microsoft online. Jestli používáte Office 365, Microsoft Azure, Intune nebo ostatní uživatelé, už používáte Azure AD povolit přihlašování a přístup k řízení pro všechny tyto služby. 

Všichni uživatelé, které jste zapnuli automatický přístup pro Microsoft Online services jsou ve skutečnosti rozumí uživatelské účty v jedné nebo víc instancí Azure AD. Můžete povolit tyto účty zdarma Azure AD funkce, například aplikace přístupu ke cloudu.
 
Navíc Azure AD zaplacený služby (například: Azure AD basic, Premium, EMS, atd.) jiné doplnit Online služeb, jako je Office 365 a Microsoft Azure pomocí komplexní enterprise měřítko správu a zabezpečení řešení.


---



## <a name="getting-started-with-hybrid-azure-ad"></a>Začínáme s hybridním Azure AD


**Otázka: jak se dá připojit místního adresáře Azure AD?**

**A:** Místního adresáře můžete připojit k Azure AD pomocí **Azure AD Connect**. 

Další informace najdete v tématu [Integrace místních identit s Azure Active Directory](active-directory-aadconnect.md).


---

**Otázka: jak nastavit jednotné přihlašování mezi místního adresáře a aplikace cloudu?**

**A:** Potřebujete jenom nastavení jednotného přihlašování mezi místního adresáře a Azure AD. Dokud prostřednictvím Azure AD zobrazíte cloudové aplikace služby automaticky jednotky uživatelům správně ověřovat pomocí svých přihlašovacích údajů místní.

Implementace jednotného přihlašování z místního můžete snadno dosáhnout s federace řešení například služby AD FS nebo nakonfigurováním hash synchronizaci hesel. Můžete nasadit snadno obou možnostech pomocí Průvodce konfigurací Azure AD Connect.
  

Další informace najdete v tématu [Integrace místních identit s Azure Active Directory](active-directory-aadconnect.md).
  

---

**Otázka: Azure Active Directory poskytuje samoobslužný portál pro uživatelé v naší organizaci?**

**A:** Ano, Azure Active Directory umožňuje [Panel Azure AD přístup](http://myapps.microsoft.com) pro uživatele samoobslužných funkcí a aplikace access. Pokud jste zákazníkem Office 365, najdete spoustu stejné možnosti na portálu Office 365. 

Další informace najdete v tématu [Úvod do panelu aplikace Access](active-directory-saas-access-panel-introduction.md). 



---

**Otázka: Azure AD pomohl správě Moje místní infrastruktury?**

**A:** Ano, nepodporuje. Azure AD Premium edition obsahuje **Stav připojení**. Stav připojení Azure AD pomáhá sledovat a získat přehled o infrastrukturu místních identit a služby synchronizace.  

Další podrobnosti najdete v tématu [sledování vaší místní identity infrastruktury a synchronizaci služby v cloudu](active-directory-aadconnect-health.md).  

---

## <a name="password-management"></a>Správa hesel

**Otázka: můžu používat zpětného zápisu heslo Azure AD bez synchronizaci hesel? (Označovaná taky jako, chci Azure AD SSPR pomocí hesla zpětného zápisu ale nechci, aby mých hesel uložené v cloud?)**

**A:** Nepotřebujete synchronizovat hesla AD Azure AD pro povolení zpětného zápisu. V prostředí federovaný Azure AD SSO závisí na místním adresáři k ověření uživatele. Tento scénář nevyžaduje heslo místního sledovat v Azure AD.

---

**Otázka: jak dlouho trvá k zadání hesla k zápisu zpátky na AD místní?**

**A:** Heslo zpětného zápisu pracuje v reálném čase. 

Další podrobnosti najdete v tématu [Začínáme s Správa hesel](active-directory-passwords-getting-started.md) 


---

**Otázka: můžu používat zpětného zápisu heslo pomocí hesel, která spravuje správce?**

**A:** Ano, pokud máte heslo zpětného zápisu povoleno, heslo operací správcem jsou zpět došlo k zápisu místního prostředí.  

Další odpovědi na heslo související dotazy, najdete v článku [Heslo správy nejčastějších dotazech](active-directory-passwords-faq.md).

---

## <a name="application-access"></a>Aplikace access


**Otázka: kde můžete najít seznam aplikací, které jsou předem integrovaný s Azure AD a jejich funkce?**

**A:** Azure AD má víc než 2600 předem integrované aplikace od společnosti Microsoft, poskytovatele služeb aplikací nebo partnery. Jednotné přihlašování podporují všechny předem integrované aplikace. Jednotné přihlašování vám umožní použít vaše organizace přihlašovací údaje pro přístup k aplikace. Některé aplikace také podporují automatické zřizování a zrušte pro zřízení

Úplný seznam předem integrované aplikace najdete v článku [Active Directory Marketplace](https://azure.microsoft.com/marketplace/active-directory/).


---

**Otázka: jak postupovat, pokud aplikace, který bych měl(a) není v Azure AD marketplace?**

**A:** S Azure AD Premium můžete přidat a nakonfigurovat aplikaci, které chcete. V závislosti na vaše aplikace možností a předvoleb můžete nakonfigurovat jednotné přihlašování a Automatická zřizování.  

Další informace najdete v tématu:

- [Konfigurace jednotného přihlašování k aplikacím, které nejsou v galerii aplikace služby Azure Active Directory](active-directory-saas-custom-apps.md)
- [Použití SCIM povolit automatické zřizování uživatelů a skupin služby Azure Active Directory pro aplikace](active-directory-scim-provisioning.md) 


---

**Otázka: jak uživatelé přihlásit do aplikace pomocí služby Azure Active Directory?**
 
**A:** Azure Active directory nabízí několik způsobů uživatelům prohlížet ani otevírat svoje aplikací, jako:

- Panel Azure AD přístup

- Spouštěč aplikací Office 365

- Přímé přihlášení do federovaný aplikací

- Hloubkové odkazy na federovaný, na základě heslo nebo existující aplikací

Další informace najdete v tématu [zavedení Azure AD integrované aplikace uživatelům](active-directory-appssoaccess-whatis.md#deploying-azure-ad-integrated-applications-to-users).


---

**Otázka: jaké různé způsoby Azure Active Directory umožňuje ověřování a jednotné přihlašování k aplikacím?**
 
**A:** Azure Active Directory podporuje mnoho standardizovaným protokoly pro a například SAML 2.0 OpenID připojení, OAuth 2.0 a WS Federation tak mohli ověřovat. Azure AD taky podporuje překlenutí vyrovnávací paměti heslo a funkcí automatického přihlášení k aplikacím, které podporují pouze ověřování pomocí formulářů.  

Další informace najdete v tématu:

- [Ověřování scénáře Azure AD](active-directory-authentication-scenarios.md)

- [Protokoly ověřování služby Active Directory](https://msdn.microsoft.com/library/azure/dn151124.aspx)

- [Jak jednotné přihlášení s prací Azure Active Directory?](active-directory-appssoaccess-whatis.md#how-does-single-sign-on-with-azure-active-directory-work)


---

**Otázka: můžu přidat aplikací používám místní?**

**A:** Proxy server Azure AD aplikace umožňuje snadno a zabezpečený přístup k místní webové aplikace, které zvolíte. Dostanete tyto aplikace stejným způsobem jako přistupujete SaaS aplikace v Azure Active Directory. Není potřeba VPN nebo změna vaši síťovou infrastrukturu.  

Další podrobnosti najdete v tématu [jak poskytnout zabezpečené vzdálený přístup k aplikacím místní](active-directory-application-proxy-get-started.md).


--- 

**Otázka: jak nevyžadují MFA uživatelům přístup k určité aplikace?**

**A:** Azure AD podmíněného přístupu můžete přiřadit zásadu jedinečné přístupu pro každou aplikaci. V zásady můžete požádat MFA vždy, nebo když uživatelé nejste připojení k místní síti.  

Další informace najdete v tématu [zabezpečení přístup k Office 365 a dalších aplikací připojení pro službu Azure Active Directory](active-directory-conditional-access.md).


---

**Otázka: co je aplikace SaaS automatické zřizování uživatele?**

**A:** Azure Active Directory umožňuje automatizovat vytváření, údržbu a odebrání identit uživatelů v mnoha oblíbených cloudové (SaaS) aplikace. 

Další informace najdete v tématu [automatizovat zřizování uživatelů a Deprovisioning aplikacím SaaS s Azure Active Directory](active-directory-saas-app-provisioning.md)

---



