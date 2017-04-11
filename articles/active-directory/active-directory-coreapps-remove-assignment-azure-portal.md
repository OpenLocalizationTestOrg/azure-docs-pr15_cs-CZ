<properties
    pageTitle="Odebrat uživatele nebo skupinu přiřazení z aplikace enterprise v náhledu Azure Active Directory | Microsoft Azure"
    description="Jak odebrat přiřazení přístupu uživatele nebo skupiny z aplikace enterprise v Azure Active Directory"
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
    ms.date="09/30/2016"
    ms.author="curtand"/>


# <a name="remove-a-user-or-group-assignment-from-an-enterprise-app-in-azure-active-directory-preview"></a>Odebrat uživatele nebo skupiny přiřazení z aplikace enterprise v náhledu Azure Active Directory

Není těžké si odebrání přiřazený přístup pro jeden z vašich podnikových aplikací v náhledu Azure Active Directory (Azure AD) uživatele nebo skupiny. [Novinky v náhledu](active-directory-preview-explainer.md) Máte potřebná oprávnění ke správě aplikaci organizace. V aktuálním náhledu musí být globální správce pro adresář.

## <a name="how-do-i-remove-a-user-or-group-assignment"></a>Jak odebrat uživatele nebo skupiny přiřazení?

1. Přihlaste se k [Azure portál](https://portal.azure.com) pomocí účtu, který je jako globální správce pro adresář.

2. Vyberte **Další služby**, zadejte **Azure Active Directory** do textového pole a potom stiskněte **Enter**.

3. Na * *Azure Active Directory - *directoryname* ** zásuvné (to znamená Azure AD zásuvné adresáře, které spravujete), vyberte **Enterprise aplikací **.

    ![Otevření podnikových aplikací](./media/active-directory-coreapps-remove-assignment-user-azure-portal/open-enterprise-apps.png)

4. Na zásuvné **podnikových aplikací** vyberte **všechny aplikace**. Zobrazí se seznam aplikace, které můžete spravovat.

5. Na zásuvné **podnikových aplikací – všechny aplikace** vyberte aplikace.

6. Na zásuvné ***název_aplikace*** (to znamená zásuvné s názvem aplikaci vybrané v názvu) vyberte **Uživatelé a skupiny**.

    ![Výběrem uživatele nebo skupiny](./media/active-directory-coreapps-remove-assignment-user-azure-portal/remove-app-users.png)

7. Na ***název_aplikace*** **-uživatele a skupiny přiřazení** zásuvné, vyberte některou z více uživatelů a skupin a klikněte na příkaz **Odebrat** . Potvrďte příkazového řádku.

    ![Výběr příkazu odebrat](./media/active-directory-coreapps-remove-assignment-user-azure-portal/remove-users.png)

## <a name="next-steps"></a>Další kroky

- [Zobrazit všechny mé skupiny](active-directory-groups-view-azure-portal.md)
- [Přiřadit aplikace enterprise uživatele nebo skupiny](active-directory-coreapps-assign-user-azure-portal.md)
- [Zakázání přihlášení uživatele aplikace enterprise](active-directory-coreapps-disable-app-azure-portal.md)
- [Změna názvu a loga aplikace enterprise](active-directory-coreapps-change-app-logo-user-azure-portal.md)
