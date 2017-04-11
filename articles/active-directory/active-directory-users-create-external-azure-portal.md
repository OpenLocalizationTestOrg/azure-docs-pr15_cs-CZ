<properties
    pageTitle="Uživatele můžete přidat další adresáře nebo partnera společnosti v náhledu Azure Active Directory | Microsoft Azure"
    description="Vysvětluje, jak přidávat uživatele nebo změna informací o uživateli v Azure Active Directory, včetně externích a hosta uživatelů."
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
    ms.date="09/12/2016"
    ms.author="curtand"/>

# <a name="add-users-from-other-directories-or-partner-companies-in-azure-active-directory-preview"></a>Uživatele můžete přidat další adresáře nebo partnera společnosti v náhledu Azure Active Directory

> [AZURE.SELECTOR]
- [Azure portálu](active-directory-users-create-external-azure-portal.md)
- [Azure klasický portálu](active-directory-create-users-external.md)

Tento článek vysvětluje, jak přidávat uživatele z ostatních adresářů v náhledu Azure Active Directory (Azure AD) nebo od partnera společnosti. [Novinky v náhledu](active-directory-preview-explainer.md) Informace o přidávání nových uživatelů ve vaší organizaci a přidání uživatelů, kteří mají účty Microsoft najdete v tématu [Přidání nových uživatelů do služby Azure Active Directory](active-directory-users-create-azure-portal.md). Přidané uživatelé nemají oprávnění správce ve výchozím nastavení, ale můžete přiřadit role je kdykoli.

## <a name="add-a-user"></a>Přidání uživatele

1.  Přihlaste se k [Azure portál](https://portal.azure.com) pomocí účtu, který je jako globální správce pro adresář.

2.  Vyberte **Další služby**, zadejte do textového pole **Uživatelé a skupiny** a pak stiskněte **Enter**.

    ![Otevření Správa uživatelů](./media/active-directory-users-create-external-azure-portal/create-users-user-management.png)

3.  Na zásuvné **Uživatelé a skupiny** vyberte **uživatele**a pak vyberte **Přidat**.

    ![Výběr příkazu Přidat](./media/active-directory-users-create-external-azure-portal/create-users-add-command.png)

4. Na zásuvné **uživatele** zadejte zobrazované jméno do pole **název** a uživatelské přihlašovací jméno do pole **uživatelské jméno**.

5. Zkopírujte nebo jinak si poznamenejte heslo vygenerovaných uživatele tak, že můžete poskytnout ho uživatele po dokončení tohoto procesu.

6. V případě potřeby vyberte **profil** nejdřív přidáte uživatele a příjmení, funkci a název oddělení.
    
    ![Otevření profilu uživatele](./media/active-directory-users-create-external-azure-portal/create-users-user-profile.png)

    - Kliknutím na **skupiny** přidejte uživatele do jedné nebo více skupin.

        ![Přidání uživatele do skupiny](./media/active-directory-users-create-external-azure-portal/create-users-user-groups.png)

    - Vyberte **organizační roli** přiřadit roli uživatele ze seznamu **role** . Další informace o uživateli a správcem rolí v tématu [přiřazení rolí správce v Azure AD](active-directory-assign-admin-roles.md).

        ![Přiřazení role uživatele](./media/active-directory-users-create-external-azure-portal/create-users-assign-role.png)

7. Vyberte možnost **vytvořit**.

8. Bezpečné distribute vytvořené heslo pro nové uživatele tak, aby mohli přihlašovat uživatele.

> [AZURE.IMPORTANT] Pokud vaše organizace využívá víc než jednu doménu, byste měli vědět o těchto problémů po přidání uživatelského účtu:
>
> - Přidání uživatelských účtů s stejný hlavní název uživatele (UPN) mezi doménami, **nejprve** přidat, třeba geoffgrisso@contoso.onmicrosoft.com, **následovaný** geoffgrisso@contoso.com.
> - **Nepřidávat** geoffgrisso@contoso.com před přidáním geoffgrisso@contoso.onmicrosoft.com. Toto pořadí je důležité a může být náročný vrátit zpět.

Pokud změníte informace pro uživatele, jejichž identity synchronizované s místní službou Active Directory, nelze změnit informace o uživateli Azure klasické portálu. Pokud chcete změnit informace o uživateli, pomocí nástroje pro správu služby Active Directory vaší místní.


## <a name="whats-next"></a>Další krok

- [Přidání uživatele](active-directory-users-create-azure-portal.md)
- [Resetování hesla uživatele v novém portálu Azure](active-directory-users-reset-password-azure-portal.md)
- [Přiřadit role uživatele v Azure AD](active-directory-users-assign-role-azure-portal.md)
- [Změňte informace o práci uživatelů](active-directory-users-work-info-azure-portal.md)
- [Správa profilů uživatelů](active-directory-users-profile-azure-portal.md)
- [Odstraněním uživatelského účtu v Azure AD](active-directory-users-delete-user-azure-portal.md)
