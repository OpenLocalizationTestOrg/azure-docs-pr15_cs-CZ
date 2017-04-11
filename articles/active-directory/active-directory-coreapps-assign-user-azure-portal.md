<properties
    pageTitle="Přiřadit aplikace enterprise v náhledu Azure Active Directory uživatele nebo skupiny | Microsoft Azure"
    description="Postup při výběru aplikace enterprise přiřazení uživatele nebo skupiny v Azure Active Directory"
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
    ms.date="10/03/2016"
    ms.author="curtand"/>

# <a name="assign-a-user-or-group-to-an-enterprise-app-in-azure-active-directory-preview"></a>Přiřadit aplikace enterprise v náhledu Azure Active Directory uživatele nebo skupiny

Není těžké si uživatele nebo skupiny přiřadit vašich podnikových aplikací v náhledu Azure Active Directory (Azure AD). [Novinky v náhledu](active-directory-preview-explainer.md) Máte potřebná oprávnění ke správě aplikaci organizace. V aktuálním náhledu musí být globální správce pro adresář.

## <a name="how-do-i-assign-user-access-to-an-enterprise-app"></a>Jak přiřadit přístup uživatelů k aplikaci enterprise?

1. Přihlaste se k [Azure portál](https://portal.azure.com) pomocí účtu, který je jako globální správce pro adresář.

2. Vyberte **Další služby**, zadejte Azure Active Directory do textového pole a potom stiskněte **Enter**.

3. Na * *Azure Active Directory - *directoryname* ** zásuvné (to znamená Azure AD zásuvné adresáře, které spravujete), vyberte **Enterprise aplikací **.

    ![Otevření podnikových aplikací](./media/active-directory-coreapps-assign-user-azure-portal/open-enterprise-apps.png)

4. Na zásuvné **podnikových aplikací** vyberte **všechny aplikace**. Zobrazí se seznam aplikace, které můžete spravovat.

5. Na zásuvné **podnikových aplikací – všechny aplikace** vyberte aplikace.

6. Na zásuvné ***název_aplikace*** (to znamená zásuvné s názvem aplikaci vybrané v názvu) vyberte **Uživatelé a skupiny**.

    ![Výběr příkazu všechny aplikace](./media/active-directory-coreapps-assign-user-azure-portal/select-app-users.png)

7. Na ***název_aplikace*** **-uživatele a skupiny přiřazení** zásuvné, vyberte příkaz **Přidat** .

8. Na zásuvné **Přidat přiřazení** vyberte **Uživatelé a skupiny**.

    ![Přiřazení uživatele nebo skupiny do aplikace](./media/active-directory-coreapps-assign-user-azure-portal/assign-users.png)

9. Na zásuvné **Uživatelé a skupiny** vyberte jeden nebo více uživatelů nebo skupin ze seznamu a klikněte na tlačítko **Vybrat** v dolní části zásuvné.

10. Na zásuvné **Přidat přiřazení** vyberte **roli**. Na zásuvné **Výběr Role** vyberte roli, kterou chcete použít pro vybrané uživatele nebo skupiny a pak klikněte na tlačítko **OK** v dolní části zásuvné.

11. Na zásuvné **Přidat přiřazení** vyberte tlačítko **přiřadit** v dolní části zásuvné. Přiřazené uživatelům nebo skupinám budou mít oprávnění definované vybranou roli pro tuto aplikaci organizace.

## <a name="next-steps"></a>Další kroky

- [Zobrazit všechny mé skupiny](active-directory-groups-view-azure-portal.md)
- [Odebrat uživatele nebo skupinu přiřazení z aplikace enterprise](active-directory-coreapps-remove-assignment-azure-portal.md)
- [Zakázání přihlášení uživatele aplikace enterprise](active-directory-coreapps-disable-app-azure-portal.md)
- [Změna názvu a loga aplikace enterprise](active-directory-coreapps-change-app-logo-user-azure-portal.md)
