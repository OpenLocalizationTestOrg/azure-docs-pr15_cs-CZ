<properties
    pageTitle="Přidání nových uživatelů do služby Azure Active Directory náhled | Microsoft Azure"
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
    ms.topic="article"
    ms.date="09/12/2016"
    ms.author="curtand"/>


# <a name="add-new-users-to-azure-active-directory-preview"></a>Přidání nových uživatelů do služby Azure Active Directory náhledu

> [AZURE.SELECTOR]
- [Azure portálu](active-directory-users-create-azure-portal.md)
- [Azure klasický portálu](active-directory-create-users.md)

Tento článek vysvětluje, jak přidat nové uživatele ve vaší organizaci v náhledu Azure Active Direstory (Azure AD). [Novinky v náhledu](active-directory-preview-explainer.md)

1.  Přihlaste se k [Azure portál](https://portal.azure.com) pomocí účtu, který je jako globální správce pro adresář.

2.  Vyberte **Další služby**, zadejte do textového pole **Uživatelé a skupiny** a pak stiskněte **Enter**.

    ![Otevření Správa uživatelů](./media/active-directory-users-create-azure-portal/create-users-user-management.png)

3.  Na zásuvné **Uživatelé a skupiny** vyberte **Všichni uživatelé**a potom vyberte **Přidat**.

    ![Výběr příkazu Přidat](./media/active-directory-users-create-azure-portal/create-users-add-command.png)

4.  Zadejte informace pro uživatele, třeba **název** a **uživatelské jméno**. Část uživatelské jméno pro název domény musí být výchozí název domény "foo.onmicrosoft.com" název domény, nebo ověřenou, nefederované doméně například "contoso.com."

5. Zkopírujte nebo jinak si poznamenejte heslo vygenerovaných uživatele tak, že můžete poskytnout ho uživatele po dokončení tohoto procesu.

6. Pokud chcete můžete otevřít a vyplňte informace o zásuvné **profilu** , zásuvné **skupiny** nebo zásuvné **adresáře role** pro uživatele. Další informace o uživateli a správcem rolí v tématu [přiřazení rolí správce v Azure AD](active-directory-assign-admin-roles.md).

7.  Na zásuvné **uživatele** vyberte možnost **vytvořit**.

8. Bezpečné distribute vytvořené heslo pro nové uživatele tak, aby mohli přihlašovat uživatele.

## <a name="whats-next"></a>Další krok

- [Přidání externího uživatele](active-directory-users-create-external-azure-portal.md)
- [Resetování hesla uživatele v novém portálu Azure](active-directory-users-reset-password-azure-portal.md)
- [Změňte informace o práci uživatelů](active-directory-users-work-info-azure-portal.md)
- [Správa profilů uživatelů](active-directory-users-profile-azure-portal.md)
- [Odstraněním uživatelského účtu v Azure AD](active-directory-users-delete-user-azure-portal.md)
- [Přiřadit role uživatele v Azure AD](active-directory-users-assign-role-azure-portal.md)
