<properties
    pageTitle="Zásady pro hesla a omezení v Azure Active Directory | Microsoft Azure"
    description="Popisuje zásad, které se vztahují k heslům v Azure Active Directory, včetně povolené znaky, délka a vypršení platnosti"
  services="active-directory"
    documentationCenter=""
    authors="curtand"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/04/2016"
    ms.author="curtand"/>


# <a name="password-policies-and-restrictions-in-azure-active-directory"></a>Zásady pro hesla a omezení v Azure Active Directory

Tento článek popisuje zásady pro hesla a složitost přidružené uživatelských účtů uložené v adresáři Azure AD.

> [AZURE.IMPORTANT] **Jste si tady, protože máte problémy s přihlášením?** Pokud ano, [tady je jak můžete změnit a resetování vlastního hesla](active-directory-passwords-update-your-own-password.md).

## <a name="userprincipalname-policies-that-apply-to-all-user-accounts"></a>Zásady UserPrincipalName, které platí pro všechny uživatelské účty

Každý uživatelský účet, kterou je potřeba se přihlásit k ověřování systému Azure AD musí mít hodnotu atributu jedinečné uživatelského jména (UPN) přidruženým k tomuto účtu. V následující tabulce osnovy zásad, které platí pro obě místní adresářové služby Active Directory získaná (synchronizované do cloudu) a jen cloudu uživatelské účty.

|   Vlastnost           |     Požadavky na UserPrincipalName  |
|   ----------------------- |   ----------------------- |
|  Znaky povolené    |  <ul> <li>A-Z</li> <li>-z </li><li>0 – 9</li> <li> . - \_ ! \# ^ \~</li></ul> |
|  Znaky není povolené  | <ul> <li>Všechny '@' znak, který není oddělení uživatelské jméno z domény.</li> <li>Nesmí obsahovat znak tečky "." bezprostředně před '@' symbol</li></ul> |
| Délka omezení  |       <ul> <li>Celková délka nesmí překročit 113 znaků</li><li>64 znaků před ‘@’ symbol</li><li>48 znaků po ‘@’ symbol</li></ul>

## <a name="password-policies-that-apply-only-to-cloud-user-accounts"></a>Zásady pro hesla, které se použije jenom pro cloudu uživatelských účtů

Následující tabulka popisuje nastavení zásad dostupné heslo, které se dají použít pro uživatelské účty, které se vytvářejí a spravují v Azure AD.

|  Vlastnost       |    Požadavky          |
|   ----------------------- |   ----------------------- |
|  Znaky povolené   |   <ul><li>A-Z</li><li>-z </li><li>0 – 9</li> <li>@# $ % ^ & * - _ ! + [] znaky {} = & #124; \ : ‘ , . ? / ` ~ “ ( ) ;</li></ul> |
|  Znaky není povolené   |       <ul><li>Znaků kódu Unicode</li><li>Mezery</li><li> **Silných hesel**: nesmí obsahovat tečkou "." bezprostředně před '@' symbol</li></ul> |
|   Omezení hesla | <ul><li>8 znaky minimální a maximální 16 znaků</li><li>**Silných hesel**: vyžaduje 3 mimo 4 z následujících akcí:<ul><li>Malá písmena</li><li>Velká písmena</li><li>Čísla (0 až 9)</li><li>Symboly (viz výše uvedené omezení hesla)</li></ul></li></ul> |
| Doba trvání vypršení platnosti hesla      | <ul><li>Výchozí hodnota: **90** dnů </li><li>Hodnota je nakonfigurovat pomocí rutinu Set-MsolPasswordPolicy z modul Azure Active Directory pro Windows PowerShell.</li></ul> |
| Upozornění na vypršení platnosti hesla |  <ul><li>Výchozí hodnota: **14** dní (před vypršením platnosti hesla)</li><li>Hodnota je nakonfigurovat pomocí rutinu Set-MsolPasswordPolicy.</li></ul> |
| Vypršení platnosti hesla |  <ul><li>Výchozí hodnota: **false** dny (označuje, že vypršení platnosti hesla je povoleno) </li><li>Hodnota je možné konfigurovat pomocí rutinu Set-MsolUser jednotlivých uživatelských účtů. </li></ul> |
|  Historii hesel  | Poslední heslo se nedají použít znovu. |
|  Doba trvání historii hesel | Trvale |
|  Uzamčení účtu | Po 10 přihlašovací pokusech (nesprávné heslo) uživatel uzamčen jednu minutu. Další nesprávné přihlašovací neúspěšných pokusech o bude uzamčeno uživatele pro zvýšení doby trvání. |


## <a name="next-steps"></a>Další kroky

* **Jste si tady, protože máte problémy s přihlášením?** Pokud ano, [tady je jak můžete změnit a resetování vlastního hesla](active-directory-passwords-update-your-own-password.md).
* [Správa hesla odkudkoli](active-directory-passwords.md)
* [Jak funguje správa hesel](active-directory-passwords-how-it-works.md)
* [Začínáme s řídícího heslo](active-directory-passwords-getting-started.md)
* [Přizpůsobení Správa hesel](active-directory-passwords-customize.md)
* [Doporučené postupy správy hesel](active-directory-passwords-best-practices.md)
* [Jak získat provozní interpretaci s sestavy správy hesel](active-directory-passwords-get-insights.md)
* [Správa hesel časté otázky](active-directory-passwords-faq.md)
* [Poradce při potížích s Správa hesel](active-directory-passwords-troubleshoot.md)
* [Víc se uč](active-directory-passwords-learn-more.md)
