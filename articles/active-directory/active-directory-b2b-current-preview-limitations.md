<properties
   pageTitle="Aktuální verze preview omezení pro spolupráci Azure Active Directory B2B | Microsoft Azure"
   description="Azure Active Directory B2B podporuje vztahy mezi více společnostmi povolením obchodní partnery selektivně přístup k podnikové aplikace"
   services="active-directory"
   documentationCenter=""
   authors="viv-liu"
   manager="cliffdi"
   editor=""
   tags=""/>

<tags
   ms.service="active-directory"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="identity"
   ms.date="05/09/2016"
   ms.author="viviali"/>

# <a name="azure-ad-b2b-collaboration-preview-current-preview-limitations"></a>Náhled spolupráce Azure AD B2B: aktuální náhled omezení

- Vícefaktorové ověřování (MFA) není podporováno v externí uživatelé. Například pokud Contoso má MFA, nikoli však organizace partnera, pak partnera organizační uživatelům nelze udělit MFA prostřednictvím B2B spolupráce.
- Je možné jenom pomocí souboru CSV; pozvánky jednotlivé pozvánky a rozhraní API aplikace access nejsou podporované.
- Jenom globální správci služby Azure AD nahrávat soubory CSV.
- Pozvánku e-mailové adresy příjemce (třeba hotmail.com, – Gmail.com nebo služby comcast.net) nejsou v současnosti podporované.
- Přístup externích uživatelů k místní aplikací není testovat.
- Externí uživatelé nejsou vyčistí automaticky při skutečné uživatele se odstraní z jejich adresáře.
- Pozvánku distribuční seznamy nepodporuje.
- Maximálně 2 000 záznamů lze odeslat pomocí souboru CSV.

## <a name="related-articles"></a>Související články
Procházejte naše další články o Azure AD B2B spolupráci:

- [Co je Azure AD B2B spolupráce?](active-directory-b2b-what-is-azure-ad-b2b.md)
- [Jak to funguje](active-directory-b2b-how-it-works.md)
- [Podrobné informace](active-directory-b2b-detailed-walkthrough.md)
- [Odkaz na formát souboru CSV](active-directory-b2b-references-csv-file-format.md)
- [Formát tokenu externích uživatelů](active-directory-b2b-references-external-user-token-format.md)
- [Externí uživatel objektu atributu změny](active-directory-b2b-references-external-user-object-attribute-changes.md)
- [Index článek Správa aplikací služby Azure Active Directory](active-directory-apps-index.md)
