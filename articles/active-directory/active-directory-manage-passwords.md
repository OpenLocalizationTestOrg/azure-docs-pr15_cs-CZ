<properties
    pageTitle="Práce s hesly v Azure Active Directory | Microsoft Azure"
    description="Jak spravovat hesla v Azure Active Directory."
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
    ms.date="08/23/2016"
    ms.author="curtand"/>

# <a name="manage-passwords-in-azure-active-directory"></a>Práce s hesly v Azure Active Directory

Pokud jste správce, můžete obnovit hesla uživatele v Azure Active Directory (Azure AD) Azure klasické portálu. Klikněte na název vašeho adresáře a na stránce Uživatelé, klikněte na jméno uživatele a v dolní části na portálu klikněte na **Resetovat heslo**.

Zbývající část Toto téma popisuje úplnou sadu možností správy heslo, které podporuje Azure AD, včetně:

- Změna **hesla samoobslužné** umožňuje koncovým uživatelům nebo správci změnit svoje hesla vypršela platnost nebo není prošlý bez volání správce nebo helpdesk podpory.
- **Samoobslužné** resetování umožňuje koncovým uživatelům nebo správce resetování hesla automaticky bez volání správce nebo helpdesk podpory. Resetování hesla samoobslužné vyžaduje Azure AD Premium nebo Basic. Další informace najdete v tématu [edicí Azure Active Directory](active-directory-editions.md).
- **Resetování hesla správce inicializovaný** umožňuje správcům heslo koncového uživatele ani jiným správcem z v Azure klasické portálu.
- **Sestavy činnosti správy heslo** přidělit správce podstatu heslo resetovat a registrace aktivity ve své organizaci.
- **Heslo zpětným** umožňuje správu místních hesel z cloudu, aby všechny výše uvedené scénáře lze provést pomocí nebo jménem, federované nebo heslo synchronizovaných uživatelů. Heslo zpětným vyžaduje Azure AD Premium. Další informace najdete v tématu [Začínáme s Azure Active Directory Premium](active-directory-get-started-premium.md).

> [AZURE.NOTE]
> Azure AD Premium je k dispozici pro čínštinu zákazníkům, kteří používají z celého světa výskyt Azure AD. Azure AD Premium není aktuálně podporován ve službě Microsoft Azure provozovaný společností 21Vianet v Číně. Další informace najdete kontaktujte na [Fórum komunity Azure Active Directory](https://feedback.azure.com/forums/169401-azure-active-directory/).

Přejít si přečtěte následující dokumentaci, které vás zajímají nejčastěji pomocí následující odkazy:

- [Přehled: Správa hesel v Azure AD](active-directory-passwords-how-it-works.md)
- [Samoobslužné resetování v Azure AD hesla: povolení, konfigurace a otestujte resetování hesla samoobslužných funkcí](active-directory-passwords-getting-started.md#enable-users-to-reset-their-azure-ad-passwords)
- [Samoobslužné resetování v Azure AD hesla: přizpůsobení heslo resetovat vlastním potřebám](active-directory-passwords-customize.md)
- [Samoobslužné resetování v Azure AD hesla: doporučené postupy pro nasazení a správu](active-directory-passwords-best-practices.md)
- [Správa hesel sestavy v Azure AD: jak zobrazit činnosti správy heslo ve vašem klientovi](active-directory-passwords-get-insights.md)
- [Heslo zpětným: jak nakonfigurovat Azure AD pro správu místních hesla](active-directory-passwords-getting-started.md#enable-users-to-reset-or-change-their-ad-passwords)
- [Poradce při potížích s Azure AD Správa hesel](active-directory-passwords-troubleshoot.md)
- [Nejčastější dotazy týkající se Azure AD Správa hesel](active-directory-passwords-faq.md)


## <a name="whats-next"></a>Další krok

- [Správa Azure AD](active-directory-administer.md)
- [Vytváření nebo úpravy uživatelů v Azure AD](active-directory-create-users.md)
- [Správa skupin v Azure AD](active-directory-manage-groups.md)
