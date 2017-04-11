<properties
   pageTitle="Náhled spolupráce Azure AD B2B: Jak funguje | Microsoft Azure"
   description="Popisuje, jak Azure Active Directory B2B spolupráce podporuje vztahy mezi více společnostmi povolením obchodní partnery selektivně přístup k podnikové aplikace"
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

# <a name="azure-ad-b2b-collaboration-preview-how-it-works"></a>Náhled spolupráce Azure AD B2B: jak to funguje
Azure AD B2B spolupráce vychází z pozvat a uplatněte modelu. Zadejte e-mailové adresy účastníků, kterou chcete pracovat, spolu s aplikací budete chtít, aby použít. Azure AD jim poslala pozvat e-mailu s odkazem. Uživatel partnera zkratky odkaz a znovu se zobrazí výzva k přihlášení svůj účet Azure AD neboli přihlašovací pro nové Azure AD účtu.

1. Správce vyzývá partnerů uložením [souboru .csv strukturovaných](active-directory-b2b-references-csv-file-format.md) pomocí portálu Azure.
2. Portál odešle pozvání e-mailů pro tyto uživatele partnera.
3. Uživatelé partnera klikněte na odkaz v e-mailu a se zobrazí výzva, přihlaste se pomocí svých přihlašovacích údajů práce (pokud jsou už v Azure AD) nebo zaregistrovat jako uživatel Azure AD B2B spolupráce.
4. Uživatelé partnera přesměrováni aplikaci, kterou budou jste pozvaní, které teď máte přístup.

## <a name="directory-operations"></a>Operace s adresářem
Uživatelé partnera, které v Azure AD jako externí uživatelé. To znamená správce zřízení licence, přiřazení členství ve skupinách a dalších udělit přístup k podnikové aplikace prostřednictvím portálu Azure nebo pomocí Powershellu Azure stejně jako pro uživatele ve vaší firmě.

Při placené Azure AD předplatného (Basic nebo Premium) není nutné použít Azure AD B2B klienti, kteří mají placené předplatné Azure AD (Basic nebo Premium) získáte následující další výhody:

 - Správci můžou přiřazovat skupiny k aplikace, poskytnutí jednodušší řízení přístupu pozvaných uživatelů.
 - Správce klienta branding slouží k označení pozvánku e-mailů a zaruč_cena prostředí poskytují další kontext pozvaných uživatelů partnera.

## <a name="related-articles"></a>Související články
 Procházet naše další články o Azure AD B2B spolupráci

 - [Co je Azure AD B2B spolupráce?](active-directory-b2b-what-is-azure-ad-b2b.md)
 - [Podrobné informace](active-directory-b2b-detailed-walkthrough.md)
 - [Odkaz na formát souboru CSV](active-directory-b2b-references-csv-file-format.md)
 - [Formát tokenu externích uživatelů](active-directory-b2b-references-external-user-token-format.md)
 - [Externí uživatel objektu atributu změny](active-directory-b2b-references-external-user-object-attribute-changes.md)
 - [Aktuální verze preview omezení](active-directory-b2b-current-preview-limitations.md)
 - [Index článek Správa aplikací služby Azure Active Directory](active-directory-apps-index.md)
