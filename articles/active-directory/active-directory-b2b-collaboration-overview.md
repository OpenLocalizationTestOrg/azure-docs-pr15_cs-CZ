<properties
   pageTitle="Spolupráce na Azure Active Directory B2B | Microsoft Azure"
   description="Spolupráce na Azure Active Directory B2B umožňuje firmy partnerům umožňuje přístup k podnikové aplikace, s každým uživatelům představované jednoho Azure AD účtu"
   services="active-directory"
   documentationCenter=""
   authors="curtand"
   manager="femila"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="08/23/2016"
   ms.author="curtand"/>

# <a name="azure-active-directory-b2b-collaboration"></a>Azure Active Directory B2B spolupráce

Spolupráce na Azure Active Directory (Azure AD) B2B umožňuje povolit přístup k podnikové aplikace od partnera Správa přístupových práv identit. Relace mezi více společnostmi můžete vytvořit tak, že pozvete a ověřování uživatelů společností partnera pro přístup k zdroje. Složitost je nižší, protože každé společnosti federates jednou s Azure Active Directory a každý uživatel představované jedním Azure AD účtu. Zabezpečení se zvyšuje, když obchodní partnery spravovat svých účtů v Azure AD, protože access je odvolaný při partnerů jsou odebrány z jejich organizace a zabránit před nechtěným přístup přes členství v interní adresáře. Pro obchodní partnery, kteří nemají už Azure AD má spolupráci B2B optimalizovaného registrace setkat i v případě poskytovat Azure AD účtů vašich obchodních partnerů.

-   Partneři firmy pomocí svoje přihlašovací údaje, které není od Správa externích partnera adresář a od potřeba odebrat přístup, pokud uživatelé opustí organizaci partnera.

-   Správa přístupu ke svým aplikacím nezávisle na cyklus účtu obchodního partnera. To znamená, například, že přistup můžete zrušit, aniž byste museli dotaz pracovníky oddělení IT partnera firmy Hotovo.

## <a name="capabilities"></a>Funkce

Spolupráce na B2B zjednodušuje správu a zlepšuje zabezpečení přístupem partnerů k podnikové materiálů, včetně SaaS aplikací, jako je Office 365, Salesforce, služby Azure a každý mobile cloudu a místní aplikace. Partnerům umožňuje spolupráce B2B Správa svých účtů a podniky můžete použít zásady zabezpečení přístupem partnerů.

Azure Active Directory B2B, který se dá snadno nakonfigurovat spolupráce zjednodušené registrace pro partnery všech velikostí, i když tito uživatelé nemají vlastní Azure Active Directory prostřednictvím proces ověření e-mailu. Je také snadná s žádné externí adresáře nebo za konfigurace federace partnera.

## <a name="b2b-collaboration-process"></a>Proces B2B spolupráce

1. Azure AD B2B spolupráci s powerpointem můžou company Administrators pozvat a povolte sadu externí uživatelé uložením souboru hodnoty oddělené čárkami (CSV) víc než 2 000 řádků na portál B2B spolupráce.

  ![Dialogové okno nahrávání souboru CSV](./media/active-directory-b2b-collaboration-overview/upload-csv.png)

2. Na portálu odešle e-mailovou pozvánku tyto externí uživatele.

3. Pozvaných uživatelů se přihlaste do existujícího účtu práce s Microsoftem (spravovaný v Azure AD) nebo získat nový účet práce v Azure AD.

4. Po přihlášení uživatele přesměrovaní na aplikaci, která s nimi bylo nasdílené.

Pozvánku e-mailové adresy příjemce (například Gmail nebo [*služby comcast.net*](http://comcast.net/)) aktuálně nepodporuje.

Další informace o tom, jak B2B spolupráce funguje podívejte se na [Toto video](http://aka.ms/aadshowb2b).

## <a name="next-steps"></a>Další kroky
Projděte si naše další články o Azure AD B2B spolupráci.

- [Co je Azure AD B2B spolupráce?](active-directory-b2b-what-is-azure-ad-b2b.md)
- [Jak to funguje](active-directory-b2b-how-it-works.md)
- [Podrobné informace](active-directory-b2b-detailed-walkthrough.md)
- [Odkaz na formát souboru CSV](active-directory-b2b-references-csv-file-format.md)
- [Formát tokenu externích uživatelů](active-directory-b2b-references-external-user-token-format.md)
- [Externí uživatel objektu atributu změny](active-directory-b2b-references-external-user-object-attribute-changes.md)
- [Aktuální verze preview omezení](active-directory-b2b-current-preview-limitations.md)
- [Index článek Správa aplikací služby Azure Active Directory](active-directory-apps-index.md)
