<properties
    pageTitle="Přihlašovací zkušenosti s ochranou Azure AD Identity | Microsoft Azure"
    description="Přehled rozsah možností uživatelů při ochrana Identity zmírnit nebo remediated uživatele nebo když se zásadami vyžaduje vícefaktorové ověřování."
    services="active-directory"
    keywords="Ochrana identity služby Azure active directory, zjišťování aplikace cloudu, Správa aplikací, zabezpečení, rizika, úroveň rizika, chyba, zásady zabezpečení"
    documentationCenter=""
    authors="markusvi"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/16/2016"
    ms.author="markvi"/>

# <a name="sign-in-experiences-with-azure-ad-identity-protection"></a>Přihlašovací zkušenosti s ochranou Azure AD Identity

S Azure Active Directory Identity Protection máte tyto možnosti:

- Pokud mají uživatelé zaregistrujte vícefaktorové ověřování

- zpracování riziková přihlášení, začátky hostují uživatelů

Odpověď systému těchto problémů má vliv na uživatele přihlášení, protože jenom přímo přihlášením zadáním uživatelské jméno a heslo nebude už možné. Další kroky jsou požadovány pro uživatele bezpečně zpět do firmy.

Toto téma obsahuje přehled uživatele přihlášení ve všech případech, ke kterým může dojít.

**Vícefaktorové ověřování**

- Registrace vícefaktorové ověřování



**Přihlaste se na rizika**

- Obnovení riziková přihlášení

- Riziková přihlašovací blokované

- Registrace vícefaktorové ověřování během riziková přihlášení
 

**Uživatel rizika**

- Obnovení hostují účtu

- Hostují účtu blokované




## <a name="multi-factor-authentication-registration"></a>Registrace vícefaktorové ověřování

Nejlepší možnosti uživatel pro obě, obnovení hostují účtu a riziková tok přihlašovací při automatické obnovit uživatele. Když uživatelé jsou registrované pro vícefaktorové ověřování, již telefonní číslo spojené s účtem, který lze použít k předání výzvy zabezpečení. Pomoc správce nebo stolního účast je potřeba k obnovení z narušení zabezpečení účtu. Tak důrazně doporučujeme vaši uživatelé registrované pro vícefaktorové ověřování. 

Správci můžou:

- nastavení zásad, které si žádá uživatelům nastavení svých účtů pro větší zabezpečení ověření. 
- Povolte vynechání vícefaktorové ověřování registrace až 30 dní, v případě potřeby uživatelům v období odkladu před registrací.

**Registrace vícefaktorové ověřování zahrnuje tři kroky:**

1. V prvním kroku uživatel obdrží upozornění na požadavek na nastavení účtu pro vícefaktorové ověřování. 

    ![Remediation] (./media/active-directory-identityprotection-flows/140.png "Remediation")


2. Pokud chcete nastavit vícefaktorové ověřování, budete muset nechat systému vědět, jak chcete kontaktovat.

    ![Remediation] (./media/active-directory-identityprotection-flows/141.png "Remediation")
 
3. Systém odešle může být obtížné a vy jste potřebovat reagovat.

    ![Remediation] (./media/active-directory-identityprotection-flows/142.png "Remediation")

 



## <a name="risky-sign-in-recovery"></a>Obnovení riziková přihlášení

Pokud správce nakonfiguroval zásady pro přihlášení rizika, oznámí se při pokusu o přihlášení ovlivněné uživatele. 

**Riziková tok přihlašovací obsahuje dva kroky:** 

1. Uživatel bude informován o tom, že něco neobvyklého byl zjištěn o své přihlašovací, například přihlašování do nového umístění, zařízení nebo aplikací. 

    ![Remediation] (./media/active-directory-identityprotection-flows/120.png "Remediation")

2. Uživatel je potřeba prokázali identitu tak, že řešení výzvy zabezpečení. Pokud je uživatel registrované pro vícefaktorové ověřování potřebují round-trip bezpečnostní kód své telefonní číslo. Protože to je jenom riziková přihlásit a ne hostují účtu, uživatel nebude mít ke změně hesla v tomto proudu. 

    ![Remediation] (./media/active-directory-identityprotection-flows/121.png "Remediation")



 
## <a name="risky-sign-in-blocked"></a>Riziková přihlašovací blokované
Správci také možné nastavit zásady přihlašovací rizika blokování uživatelů na přihlásit se v závislosti na úrovni rizika. Získáte odblokování koncoví uživatelé musíte kontaktovat správce nebo help desk nebo se zkuste přihlásit ze známých umístění nebo zařízení. Automatické obnovení tak, že řešení vícefaktorové ověřování není jednu z možností v tomto případě.

![Remediation] (./media/active-directory-identityprotection-flows/200.png "Remediation")




## <a name="compromised-account-recovery"></a>Obnovení hostují účtu

Když nakonfiguroval uživatelských zásad zabezpečení rizika uživatelů, kteří splňují uživatel rizika podle zásady (a, jsou proto považovány za ohroženo) musí absolvovat toku obnovení narušení zabezpečení uživatel předtím, než se můžete přihlásit. 

**Obnovení tok narušení zabezpečení uživatele zahrnuje tři kroky:**

1. Uživatel bude informován o tom, že jejich zabezpečení účtu není riziko kvůli podezřelé aktivity nebo prozrazený přihlašovací údaje.

    ![Remediation] (./media/active-directory-identityprotection-flows/101.png "Remediation")

2.  Uživatel je potřeba prokázali identitu tak, že řešení výzvy zabezpečení. Pokud je uživatel registrované pro vícefaktorové ověřování můžete automatické obnovení z právě ohroženo. Budou se muset round-trip bezpečnostní kód své telefonní číslo. 

    ![Remediation] (./media/active-directory-identityprotection-flows/110.png "Remediation")


3.  Nakonec uživatel bude muset změnit své heslo, protože jiný uživatel měl přístup ke svému účtu. Snímky obrazovek této možnosti se pod.
 
    ![Remediation] (./media/active-directory-identityprotection-flows/111.png "Remediation")



## <a name="compromised-account-blocked"></a>Hostují účtu blokované 

Uživatel, který zablokoval uživatelských zásad zabezpečení rizika odblokování, uživatel musí kontaktujte správce nebo HelpDesk. Automatické obnovení tak, že řešení vícefaktorové ověřování není jednu z možností v tomto případě.


![Remediation] (./media/active-directory-identityprotection-flows/104.png "Remediation")



 
## <a name="reset-password"></a>Resetování hesla

Pokud hostují uživatelé jsou blokovány přihlásíte, správce vygenerovat dočasné heslo pro ně. Uživatelé budou muset při další přihlašovací změnit své heslo.

![Remediation] (./media/active-directory-identityprotection-flows/160.png "Remediation")


 




 

## <a name="see-also"></a>Viz taky

- [Ochrana Identity Azure Active Directory](active-directory-identityprotection.md) 