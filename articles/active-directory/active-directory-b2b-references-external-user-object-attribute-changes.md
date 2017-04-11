<properties
   pageTitle="Externí uživatel objektu atributu změny Azure Active Directory B2B (verze Preview) spolupráce | Microsoft Azure"
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
   ms.workload="na"
   ms.date="05/09/2016"
   ms.author="viviali"/>

# <a name="azure-ad-b2b-collaboration-preview-external-user-object-attribute-changes"></a>Náhled spolupráce Azure AD B2B: externí uživatel objektu atributu změny

Každý uživatel v adresáři služby Azure AD reprezentované objektu uživatele. Objektu uživatele služby Azure AD obsahuje atribut změny v různých fází spolupráci B2B uplatnění pozvat toku. Objekt uživatele představující partnera uživatele v adresáři má atributy, které se změní na uplatnění čas, kdy partnera klepnutí na odkaz v e-mail s pozvánkou. Konkrétně:

- Vyplněné atributy **SignInName** a **AltSecId**
- Atribut **DisplayName** se změní na dotazy používající UPN název (user_fabrikam.com#EXT#@contoso.com) na přihlašovací jméno(user@fabrikam.com)

Sledování tyto atributy v Azure AD vám můžou pomoct řešit též uživatele partnera má uplatnili jejich B2B spolupráce pozvánku.

## <a name="related-articles"></a>Související články
Procházejte naše další články o Azure AD B2B spolupráci:

- [Co je Azure AD B2B spolupráce?](active-directory-b2b-what-is-azure-ad-b2b.md)
- [Jak to funguje](active-directory-b2b-how-it-works.md)
- [Podrobné informace](active-directory-b2b-detailed-walkthrough.md)
- [Odkaz na formát souboru CSV](active-directory-b2b-references-csv-file-format.md)
- [Formát tokenu externích uživatelů](active-directory-b2b-references-external-user-token-format.md)
- [Aktuální verze preview omezení](active-directory-b2b-current-preview-limitations.md)
- [Index článek Správa aplikací služby Azure Active Directory](active-directory-apps-index.md)
