<properties
    pageTitle="Přidání nových uživatelů do služby Azure Active Directory | Microsoft Azure"
    description="Vysvětluje, jak přidat nové uživatele nebo změna informací o uživateli v Azure Active Directory."
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
    ms.topic="get-started-article"
    ms.date="09/22/2016"
    ms.author="curtand"/>

# <a name="add-new-users--or-users-with-microsoft-accounts-to-azure-active-directory"></a>Přidání noví uživatelé nebo uživatelé, kteří mají účty Microsoft Azure Active Directory

Přidání uživatelů k naplnění adresáře. Tento článek vysvětluje, jak přidat nové uživatele ve vaší organizaci a jak přidávat uživatele, kteří mají účty Microsoft. Další informace o přidávání uživatelů z ostatních adresářů v Azure Active Directory nebo přidávání uživatelů z partnera společnosti najdete v tématu [Přidání uživatelů z jiných adresářů nebo partnera společnosti v Azure Active Directory](active-directory-create-users-external.md). Přidané uživatelé nemají oprávnění správce ve výchozím nastavení, ale můžete přiřadit role je kdykoli.

## <a name="add-a-user"></a>Přidání uživatele

1. Přihlaste se k [Azure klasické portál](https://manage.windowsazure.com) pomocí účtu, který je jako globální správce pro adresář.
2. Vyberte **Služby Active Directory**a pak vyberte název vaší organizace adresáře.
3. Vyberte kartu **uživatelů** a potom v panelu s příkazy, vyberte **Přidat uživatele**.
4. Na stránce **námi o tohoto uživatele** ve skupinovém rámečku **Typ uživatele**na výběr tyhle možnosti:

    - **Nového uživatele ve vaší organizaci** – přidá nový uživatelský účet v adresáři vašeho.
    - **Uživatel s existujícího účtu Microsoft** – přidá existujícího spotř účtu Microsoft do adresáře (například účet aplikace Outlook)

5. V závislosti na **typu uživatele**zadejte uživatelské jméno (pro nové uživatele) nebo e-mailovou adresu (pro uživatele s účtem Microsoft).
6. Na stránce **profilu** uživatele zadejte jméno a příjmení jméno, popisný název a roli uživatele ze seznamu **role** . Další informace o uživateli a správcem rolí v tématu [přiřazení rolí správce v Azure AD](active-directory-assign-admin-roles.md). Určení jestli chcete **Povolit Vícefaktorové ověřování** pro uživatele.
7. Na stránce **stažení dočasné heslo** vyberte **vytvořit**.

> [AZURE.IMPORTANT] Pokud vaše organizace využívá víc než jednu doménu, byste měli vědět o těchto problémů po přidání uživatelského účtu:
>
> - Přidání uživatelských účtů s stejný hlavní název uživatele (UPN) mezi doménami, **nejprve** přidat, třeba geoffgrisso@contoso.onmicrosoft.com, **následovaný** geoffgrisso@contoso.com.
> - **Nepřidávat** geoffgrisso@contoso.com před přidáním geoffgrisso@contoso.onmicrosoft.com. Toto pořadí je důležité a může být náročný vrátit zpět.

## <a name="change-user-information"></a>Změňte informace o uživatelích

Můžete změnit všechny uživatele atributy s výjimkou ID objektu.

1. Otevření adresáře.
2. Vyberte kartu **uživatele** a pak vyberte zobrazované jméno uživatele, kterého chcete změnit.
3. Dokončete změny a klikněte na tlačítko **Uložit**.

Pokud uživatel, který chcete změnit se synchronizuje s své místní služby Active Directory, nemůžete změnit informace o uživateli pomocí tohoto postupu. Pokud chcete změnit uživatele, pomocí nástroje pro správu služby Active Directory vaší místní.

## <a name="guest-user-management-and-limitations"></a>Správa uživatelů hosta a omezení

Účtů hosta jsou uživatelé z ostatních adresářů, které jste pozvaní k adresáři přístup dokumentů SharePoint, aplikací nebo jiných Azure zdroje. Účtu hosta ve vašem adresáři obsahuje atribut podkladového UserType nastavena na "Host." Běžná (konkrétně členové adresáři) uživatelé atribut UserType "Člen."

Ve složce hostů máte omezené sadu oprávnění. Tato práva omezit možnost hostů zjistit informace o ostatních uživatelů v adresáři. Uživatele typu Host můžete pořád komunikovat s uživateli a skupinami přidružené prostředky, které zrovna pracují. Uživatele typu Host tyto možnosti:

- Zobrazit další uživatelé a skupiny přidružené k předplatnému Azure, ke kterému jsou přiřazené
- Zobrazit členy této skupiny, do kterých patří
- Vyhledání dalších uživatelů v adresáři, pokud znáte úplnou e-mailovou adresu uživatele
- Zobrazit pouze omezenou sadu atributy uživatelů, které budou vyhledat – omezené na zobrazované jméno, e-mailovou adresu, hlavní jméno uživatele (UPN) a miniaturu fotografie
- Zobrazte seznam ověřených domén v adresáři
- Souhlas s aplikací, poskytnutí je stejný přístup, který členové mají v adresáři

## <a name="set-guest-user-access-policies"></a>Nastavení zásad přístup guest uživatelů

**Konfigurace** karta adresář obsahuje možnosti řízení přístupu pro uživatele typu Host. Tyto možnosti mohou měnit pouze v Azure klasické portál adresáře globálního správce. V současné době není žádný prostředí PowerShell nebo rozhraní API způsob.

Otevření karty **Konfigurovat** v portálu Azure klasické, vyberte **Služby Active Directory**a vyberte název v adresáři.

![Konfigurace kartu v Azure Active Directory][1]

Můžete upravovat přímo možnosti řízení přístupu pro uživatele typu Host.

![možnosti řízení přístupu pro uživatele typu Host][2]


## <a name="whats-next"></a>Další krok

- [Uživatele můžete přidat další adresáře nebo partnera společnosti v Azure Active Directory](active-directory-create-users-external.md)
- [Správa Azure AD](active-directory-administer.md)
- [Práce s hesly v Azure AD](active-directory-manage-passwords.md)
- [Správa skupin v Azure AD](active-directory-manage-groups.md)

<!--Image references-->
[1]: ./media/active-directory-create-users/RBACDirConfigTab.png
[2]: ./media/active-directory-create-users/RBACGuestAccessControls.png
