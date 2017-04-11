<properties
    pageTitle="Azure Active Directory ochrana Identity – jak zrušit blokování uživatelů | Microsoft Azure"
    description="Zjistěte, jak zrušit blokování uživatelů, které byly blokovány zásad Azure Active Directory Identity Protection."
    services="active-directory"
    keywords="Ochrana identity služby Azure active directory, odblokování uživatele"
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
    ms.date="09/20/2016"
    ms.author="markvi"/>

#<a name="azure-active-directory-identity-protection---how-to-unblock-users"></a>Azure Active Directory ochrana Identity – jak zrušit blokování uživatelů

Azure Active Directory Identity ochrana můžete nakonfigurovat zásady blokování uživatelů, pokud jsou splněny nakonfigurované podmínky. Obvykle blokovaného uživatele kontakty technické podpory osvobozením od odblokovat. Tato témata vysvětluje postup můžete provádět odblokovat blokovaného uživatele.


## <a name="determine-the-reason-for-blocking"></a>Zjistěte, proč pro blokování

Prvním krokem odblokovat uživatele musíte zjistit typ zásad, které má uživatel zablokovaná, protože další kroky se v závislosti na tom. Azure Active Directory Identity ochrana uživatele může být buď blokovány přihlašovací riziko nebo zásady rizika uživatele. 

Můžete získat typ zásad, které zablokoval uživatele ze záhlaví v dialogovém okně, které jste se uživateli zobrazí při pokusu o přihlášení:

|Zásady | Dialogové okno uživatele|
|--- | --- |
|Přihlašovací rizika | ![Blokovaných přihlásit](./media/active-directory-identityprotection-unblock-howto/02.png) |
|Riziko pro uživatele | ![Zablokovaného účtu](./media/active-directory-identityprotection-unblock-howto/104.png) |


Uživatel, který je blokován funkcí:

- Zásady přihlašovací rizika jsou označovaná taky jako podezřelé přihlásit
- Zásady rizika uživatele je označovaná taky jako účet u rizika

 
## <a name="unblocking-suspicious-sign-ins"></a>Odblokování podezřelé přihlášení

Odblokovat podezřelé přihlášení, máte následující možnosti:

1. **Přihlášení z známé umístění nebo zařízení** - běžné důvod, proč se blokovaných podezřelé odhlásit: doplňky, které jsou přihlašovací pokusy cizí umístění nebo zařízení. Uživatelé mohli rychle zjistit, zda je blokování z důvodu tak, že pokusu o přihlášení z známé umístění nebo zařízení.


3. **Vyloučit z zásad** – Pokud si myslíte, že aktuální konfigurace zásad přihlašovací způsobuje problémy s pro konkrétní uživatele, kterým vyloučíte uživatelů z něho. V tématu [zásady přihlašovací rizika](active-directory-identityprotection.md#sign-in-risk-policy) další podrobnosti.
 
4. **Zásada** – Pokud si myslíte, že konfigurace zásad způsobuje problémy s pro všechny uživatele, můžete zakázat zásadu. V tématu [zásady přihlašovací rizika](active-directory-identityprotection.md#sign-in-risk-policy) další podrobnosti.


## <a name="unblocking-accounts-at-risk"></a>Odblokování účty riziko

Odblokovat účet u rizika, máte k dispozici následující možnosti:

1. **Resetování hesla** – můžete resetovat jeho heslo. V tématu [Ruční zabezpečené heslo resetovat](active-directory-identityprotection.md#manual-secure-password-reset) další podrobnosti.

2. **Zavřete všechny události rizika** - zásady rizika uživatele blokuje uživatele, pokud dosáhl úroveň rizika nakonfigurované uživatelů pro blokování přístup. Zmenšení uživatele na úroveň rizika ručně zavřením hlášené rizika události. Další podrobnosti najdete v tématu [Ruční zavření rizikové události](active-directory-identityprotection.md#closing-risk-events-manually).

3. **Vyloučit z zásad** – Pokud si myslíte, že aktuální konfigurace zásad přihlašovací způsobuje problémy s pro konkrétní uživatele, kterým vyloučíte uživatelů z něho. V tématu [zásady rizika uživatele](active-directory-identityprotection.md#user-risk-policy) další podrobnosti.
 
4. **Zásada** – Pokud si myslíte, že konfigurace zásad způsobuje problémy s pro všechny uživatele, můžete zakázat zásadu. V tématu [zásady rizika uživatele](active-directory-identityprotection.md#user-risk-policy) další podrobnosti.




## <a name="next-steps"></a>Další kroky

 Chcete se dozvědět víc o ochraně Azure AD Identity? Podívejte se na [Ochranu Azure Active Directory Identity](active-directory-identityprotection.md).
 

