<properties
    pageTitle="Přiřadit role správců v náhledu Azure Active Directory uživatele | Microsoft Azure"
    description="Vysvětluje, jak můžete změnit informace pro správu uživatelů v Azure Active Directory"
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

# <a name="assign-a-user-to-administrator-roles-in-azure-active-directory-preview"></a>Přiřadit role správců v náhledu Azure Active Directory uživatele

Tento článek vysvětluje, jak přiřadit správní role uživatele v náhledu Azure Active Directory (Azure AD). [Novinky v náhledu](active-directory-preview-explainer.md) Informace o přidávání nových uživatelů ve vaší organizaci najdete v tématu [Přidání nových uživatelů do služby Azure Active Directory](active-directory-users-create-azure-portal.md). Přidané uživatelé nemají oprávnění správce ve výchozím nastavení, ale můžete přiřadit role je kdykoli.

## <a name="assign-a-role-to-a-user"></a>Přiřazení role uživatele

1.  Přihlaste se k [Azure portál](https://portal.azure.com) pomocí účtu, který je jako globální správce pro adresář.

2.  Vyberte **Další služby**, zadejte do textového pole **Uživatelé a skupiny** a pak stiskněte **Enter**.

    ![Otevření Správa uživatelů](./media/active-directory-users-assign-role-azure-portal/create-users-user-management.png)

3.  Na zásuvné **Uživatelé a skupiny** vyberte **Všichni uživatelé**.

    ![Otevření zásuvné všech uživatelů](./media/active-directory-users-assign-role-azure-portal/create-users-open-users-blade.png)

4. Na zásuvné **Uživatelé a skupiny – všichni uživatelé** vyberte uživatele ze seznamu.

5. Na zásuvné pro vybraného uživatele vyberte **adresář roli**a přiřaďte uživatele k roli v seznamu **adresářů role** . Další informace o uživateli a správcem rolí v tématu [přiřazení rolí správce v Azure AD](active-directory-assign-admin-roles.md).

      ![Přiřazení role uživatele](./media/active-directory-users-assign-role-azure-portal/create-users-assign-role.png)

6. Vyberte **Uložit**.


## <a name="whats-next"></a>Další krok

- [Přidání uživatele](active-directory-users-create-azure-portal.md)
- [Resetování hesla uživatele v novém portálu Azure](active-directory-users-reset-password-azure-portal.md)
- [Změňte informace o práci uživatelů](active-directory-users-work-info-azure-portal.md)
- [Správa profilů uživatelů](active-directory-users-profile-azure-portal.md)
- [Odstraněním uživatelského účtu v Azure AD](active-directory-users-delete-user-azure-portal.md)
