
<properties
    pageTitle="Technické informace o Azure Active Directory podmíněného přístupu | Microsoft Azure"
    description="Řízení přístupu podmíněné zkontroluje Azure Active Directory určité podmínky, které jste vybrali při ověřování uživatele a před umožněním přístup k aplikaci. Když jsou splněné tyto podmínky, uživatel ověření a povolený přístup k aplikaci."
    services="active-directory"
    documentationCenter=""
    authors="MarkusVi"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="identity" 
    ms.date="10/20/2016"
    ms.author="markvi"/>

# <a name="azure-active-directory-conditional-access-technical-reference"></a>Technické informace o Azure Active Directory podmíněného přístupu

## <a name="services-enabled-with-conditional-access"></a>Povolenými službami s podmíněným přístupem
Pravidla podmíněného Access podporuje přes různé typy aplikací Azure AD. Tento seznam obsahuje:

- Federované aplikací z Galerie aplikace Azure AD
- Aplikace jednotného přihlašování heslo z Galerie aplikace Azure AD
- Aplikace registrovaný u aplikace Proxy Azure
- Vyvinutý řádek firmy a klienta více aplikací registrovaný u Azure AD
- Online Visual Studio
- Azure vzdálené aplikace
-   Dynamics CRM
- Microsoft Office 365 Yammeru
- Microsoft Office 365 Exchange Online
- Microsoft SharePoint Online v Office 365 (včetně Onedrivu pro firmy)


## <a name="enable-access-rules"></a>Povolte pravidla přístupu

Každé pravidlo můžete povolit nebo zakázat na za použití základu. Když pravidla platí **na** bude ho povolit nebo nevynucují uživatelům přístup k aplikaci. Okamžiku, kdy je **Vypnuto, pokud** se nepoužijí a nebudou mít vliv na uživatele přihlášení.

## <a name="applying-rules-to-specific-users"></a>Použití pravidel u konkrétních uživatelů
Pravidla lze použít pro určité skupiny uživatelů na základě zabezpečení skupiny pomocí nastavení **Použít**. **Použít na** můžete nastavit **Všem uživatelům** nebo **skupinám**. Pokud je nastavena pro **Všechny uživatele** se použijí pravidla všichni uživatelé s přístupem k aplikaci. Možnost **skupiny** umožňuje konkrétní zabezpečení a distribučními skupinami vybranými pravidla budou vynucené pouze pro tyto skupiny.

Když nasadíte pravidla, není společné nejdřív použijte omezená sada uživatelů, které jsou součástí skupiny piloting. Po dokončení pravidlo lze použít pro **Všechny uživatele**. Toto pravidlo pro všechny uživatele v organizaci vynucené způsobí.

Vybrané skupiny mohou rovněž výjimku ze zásad použití **kromě** možnosti. Všechny členy skupiny bude vyňatých, i když se zobrazí v součástí skupiny.

## <a name="at-work-networks"></a>"V práci" sítí


Pravidla podmíněného přístupu, které používají síť "v práci" spolehnout důvěryhodných rozsahy IP adres, které jste nakonfigurovali ve Azure AD, nebo použijte deklarace "v síti corpnet" ze služby AD FS. Tato pravidla obsahují:

- Vyžadovat vícefaktorové ověřování při není v praxi
- Zablokování přístupu při není v praxi

Možnosti pro zadat sítí "v práci"

1. Na [stránce konfigurace vícefaktorové ověřování](../multi-factor-authentication/multi-factor-authentication-whats-next.md)konfigurace důvěryhodných rozsahy IP adres. Podmíněné zásady přístupu pomocí konfigurovaných rozsahů na každou žádost o ověření a vydání tokenu vyhodnotit pravidla. 
2. Konfigurace využívání vnitřní síti corpnet převzetí, tuto možnost, můžete použít se federované adresářů pomocí služby AD FS. [Další informace o uvnitř coronet deklarací](../multi-factor-authentication/multi-factor-authentication-whats-next.md#trusted-ips).
3. Konfigurace veřejné rozsahy IP adres. Na kartě Konfigurovat adresáře, můžete nastavit veřejný IP adres. Podmíněné Access jako budou používat tyto "v práci" IP adresy, díky tomu další rozsahy IP adres konfigurace nad 50 IP adresu limit, který je nevynucují tak, že na stránce nastavení MFA.



## <a name="rules-based-on-application-sensitivity"></a>Pravidla založená na aplikaci utajení

Pravidla se konfigurují na aplikaci povolení služby vysoká hodnota zabezpečená bez vlivu na přístup k jiným službám. Na kartě **Konfigurovat** aplikace je možné konfigurovat pravidla podmíněného přístupu. 

Pravidla, které aktuálně nabízíme:

- **Vyžadovat vícefaktorové ověřování**
 - Všichni uživatelé, kteří tyto zásady se vztahují k budete vyzváni k ověřovat pomocí vícefaktorové ověřování aspoň jednou.
 
- **Vyžadovat vícefaktorové ověřování při není v praxi**
 - Pokud je zásada, budete vyzváni k provedli vícefaktorové ověřování aspoň jednou, pokud jsou přístup ke službě ze vzdáleného místa nefunkční všem uživatelům. Pokud se přejít z pracovní vzdáleném umístění, bude být nutné provádět vícefaktorové ověřování při přístupu ke službě.
 
- **Zablokování přístupu při není v praxi** 
 - Pokud přesouváte z práce na vzdáleném umístění, bude Pokud zásada "Blokovat přístup, když není v práci" se použije pro je blokován.  Budou se znovu mít přístup na pracovní umístění.


## <a name="related-topics"></a>Příbuzná témata

- [Zabezpečení přístupu k Office 365 a dalších aplikací připojení pro službu Azure Active Directory](active-directory-conditional-access.md)
- [Index článek Správa aplikací služby Azure Active Directory](active-directory-apps-index.md)
