
<properties
    pageTitle="Poradce při potížích s dynamické členství ve skupinách | Microsoft Azure"
    description="Poradce při potížích s tipy pro dynamické členství ve skupinách v Azure AD."
    services="active-directory"
    documentationCenter=""
    authors="curtand"
    manager="femila"
    editor=""
    />

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/10/2016"
    ms.author="curtand"/>


# <a name="troubleshooting-dynamic-memberships-for-groups"></a>Poradce při potížích s dynamické členství ve skupinách

**Nakonfigurovali pravidlo ve skupině, ale bez členství aktualizují ve skupině**<br/>Ověřte, že je na kartě **Konfigurovat** nastavení **Povolit delegované správy skupiny** nastavena na **hodnotu Ano** . Zobrazí se toto nastavení pouze v případě, že jste přihlášení jako uživatel, kterému je přiřazena licenci Azure Active Directory Premium. Zkontrolujte hodnoty pro uživatele atributy pravidla: existují uživatelů, které splňují pravidlo?

**Nakonfigurovali pravidlo, ale teď odebrat stávající členy pravidla**<br/>Toto je očekávaná. Při povolují nebo změnit pravidlo budou odebrány stávající členy skupiny. Uživatelé vrácených hodnocení pravidla jsou přidat jako členy skupiny.     

**Proč nevidím členství změny okamžitě při přidání nebo změna pravidla, proč není?**<br/>Hodnocení vyhrazené členství pravidelně dokončení procesu asynchronní pozadí. Jak dlouho trvá procesu je určený podle počet uživatelů v adresáři vašeho a velikost skupiny vytvořili důsledku pravidlo. Obvykle adresářů s malý počet uživatelů uvidí změn členství ve skupinách menší než za několik minut. Adresářů s velkým počtem uživatelů může trvat 30 minut či delší k naplnění.

Tyto články poskytují další informace o Azure Active Directory.

* [Správa přístupu k prostředkům pomocí skupin služby Azure Active Directory](active-directory-manage-groups.md)
* [Index článek Správa aplikací služby Azure Active Directory](active-directory-apps-index.md)
* [Co je Azure Active Directory?](active-directory-whatis.md)
* [Integrace místních identit s Azure Active Directory](active-directory-aadconnect.md)
