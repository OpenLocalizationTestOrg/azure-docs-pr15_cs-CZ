<properties
    pageTitle="Azure Active Directory: Vytvoření tématu Podpora klienta | Microsoft Azure"
    description="Vytváření tenanta služby Azure Active Directory nebo Azure Active Directory B2C klienta: problémů a řešení"
    services="active-directory-b2c"
    documentationCenter=""
    authors="swkrish"
    manager="msmbaldwin"
    editor="bryanla"/>

<tags
    ms.service="active-directory-b2c"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/30/2016"
    ms.author="swkrish"/>

# <a name="creating-an-azure-active-directory-azure-ad-tenant-or-azure-ad-b2c-tenant-issues-and-resolutions"></a>Vytváření klienta služby Azure Active Directory (Azure AD) nebo Azure AD B2C klienta: problémů a řešení

## <a name="creating-an-azure-ad-tenant"></a>Vytváření Azure AD klienta

Pokud tenanta Azure AD nelze vytvořit poprvé, zkuste to znovu. Pokud problém přetrvává, kontaktujte podporu.

## <a name="creating-an-azure-ad-b2c-tenant"></a>Vytváření Azure AD B2C klienta

Pokud dojde k potížím při [vytváření Azure AD B2C klienta](active-directory-b2c-get-started.md), zkuste toto:
 
- Pokud klienta Azure AD B2C nezobrazuje v seznamu klientů, zkuste to znovu.
- Pokud klienta Azure AD B2C objeví v seznamu klientů a zobrazí se chybová zpráva ("nelze dokončit vystavením klienta B2C"contosob2c". Navštivte tento [odkaz](http://go.microsoft.com/fwlink/?LinkID=624192&clcid=0x409) další pokyny."), odstraňte klienta, který jste právě vytvořili a zkuste to znovu.
- Všimněte si, že jsou známé problémy při odstranění stávajícímu klientovi B2C a opětovné vytvoření se stejným názvem domény. Budete muset vytvoření klienta B2C s jiným názvem domény.
- Pokud tyto rozlišení nefungují vám, kontaktujte podporu. Další informace o [tom, jak požadavky na podporu soubor pro Azure AD B2C](active-directory-b2c-support.md).
